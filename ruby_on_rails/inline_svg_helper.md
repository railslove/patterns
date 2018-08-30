# Inline SVGs

Often times, we need inline svgs in Rails, which is not natively supported.
There's a rather simple approach to adding this functionality by adding a helper
to `application_helper.rb` (taken from
https://cobwwweb.com/render-inline-svg-rails-middleman):

```ruby
  module ApplicationHelper
    def inline_svg(name)
      file_path = "#{Rails.root}/app/assets/images/#{name}.svg"
      return File.read(file_path).html_safe if File.exists?(file_path)
      "[svg '#{name}' not found]"
    end
  end
```

In your views, you then can simply call

```haml
  .aIcon= inline_svg('icon_close')
```
