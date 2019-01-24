# A Better rails db:seed task

The problem with `rails db:seed` the way we know it is: seeds are hard to maintain and almost instant outdated. I like the way [Thoughtbot](https://robots.thoughtbot.com/priming-the-pump) is handling this:

1. `rails db:seed` is reserved for data that must be present for your application to function in any environment. One example of this may be a list of US States in your database that your address form relies on.
2. `rails dev:prime` Fills the database with data that looks and feels similar to what your users see. But instead of generating some fake data I like to use a production database dump that is cleaned up a little bit.

A possible solution for this could look like this `lib/tasks/dev.rake`:

```ruby
require 'colorize'

namespace :dev do
  DUMP_FILE_PATH = Rails.root.join('tmp', 'production.dump')

  desc 'Checks if heroku & postgres tooling is available and setup'
  task :pre_checks do
    puts 'Check requirements'.yellow
    # Check if heroku cli is installed
    sh 'which heroku &> /dev/null', verbose: false do |ok, res|
      if !ok
        puts 'You need to install the heroku cli:'.red
        puts '  https://devcenter.heroku.com/articles/heroku-cli'
        exit(-1)
      end
    end

    # Check if pg_restore is available
    sh 'which pg_restore &> /dev/null', verbose: false do |ok, res|
      if !ok
        puts 'Command `pg_restore` not found.'.red
        puts 'You need to install postgres cli utilities.'
        exit(-1)
      end
    end

    # Check if user has access to railslove on heroku
    sh 'heroku apps --team=railslove &> /dev/null', verbose: false do |ok, res|
      if !ok
        puts 'Your heroku-cli account is not connected to team railslove.'.red
        puts 'Use an account which has access to team railslove.'
        puts '  https://github.com/heroku/heroku-accounts'
        exit(-1)
      end
    end

    puts 'Everything is okay!'.green
    puts ''
  end

  desc 'Fetch production database dump from heroku'
  task fetch_db_dump: :environment do
    puts "Fetching dump from heroku ..."
    sh "heroku pg:backups:download --output=#{DUMP_FILE_PATH}"
    puts "done.".green
  end

  desc "Import dump stored in #{DUMP_FILE_PATH}"
  task :import_db_dump do
    cmd = nil
    with_config do |app, host, db, user, port|
      cmd = "pg_restore --verbose --host #{host} --port #{port} --username #{user} --clean --no-owner --no-acl --dbname #{db} #{DUMP_FILE_PATH}"
    end
    sh cmd
    puts "done.".green
  end
  
  desc "Cleanup data from imported dump"
  task cleanup_db_import: :environment do
    print "Cleanup imported data for development... "

    # Change all email adresses from foo@bar.com to foo@barfaked.com to
    # prevent accidentially sending mails to real people
    # (except admin@example.com)
    User.where.not(id: 1).find_each do |user|
      fake_email = user.email.gsub(/\.[a-zA-Z]*$/, 'faked\\0')
      user.update(email: fake_email, email_confirmation: fake_email)
    end
    puts "done."
  end

  desc "Fetch and import production data"
  task :prime do
    Rake::Task["dev:fetch_db_dump"].invoke
    Rake::Task["dev:import_db_dump"].invoke
    Rake::Task["dev:cleanup_db_import"].invoke
    Rake::Task["db:migrate"].invoke
    puts "done.".green
  end

  task :fetch_db_dump => :pre_checks
  task :import_db_dump => :pre_checks
  task :prime => :pre_checks
end

private

def with_config
  yield Rails.application.class.parent_name.underscore,
    ActiveRecord::Base.connection_config[:host],
    ActiveRecord::Base.connection_config[:database],
    ActiveRecord::Base.connection_config[:username],
    ActiveRecord::Base.connection_config[:port]
end
```
