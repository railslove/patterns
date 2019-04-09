# Patterns

A collection of patterns that is often use and should not be rewritten multiple
times a year by multiple people.

## Contents
1. [Purpose](#porpose)
2. [Adding new patterns](#adding-new-patterns)
3. [Improving existing patterns](#improving-existing-patterns)

## Areas
* [CSS](https://github.com/railslove/patterns/tree/master/css)
* [Ruby on
  Rails](https://github.com/railslove/patterns/tree/master/ruby_on_rails)

### Porpose

This repository aims to collect generic patterns that are often used over many
different projects, but are generic enough that they do not need to be rewritten
for every of these projects.  
As a concrete example, we discovered in a code review that both of the
participants had written a module to fade out text in CSS multiple times only in
the present year.

This repository aims build a central hub for such patterns. We believe that it
yields better outcomes to have such patterns listed here and improved by
multiple people. Patterns listed here should be generic enough to be used
independently of the project.

Even though this starts as a library for CSS patterns, it should not be limited
to only css.

### Adding new patterns

**New patterns should always be added through a Pull Request**, so they can be
reviewed and improved by multiple people. For now, we have directories for
different languages, which themselves have a `Readme` listing the contents.

A pattern should life inside a distinct markdown file. It should explain what
the patterns does and of course containt the source code to implement that
pattern.

### Improving existing patterns

An existing pattern should never be changed directly. Always open a Pull Request
to create a discussion regarding the changes to the pattern.
