Amber
===================================

Amber is a super simple and flexible static website generator, with good support for localization and navigation.

This is still experimental code.

Amber has much in common with other static page generators, but has several features that make it unique:

* I18n: Primary focus is on very good multi-lingual and localization support.
* Inheritance: Properties are inherited in the page hierarchy.
* TOC: Support for table of contents (including inserting TOC into other pages).
* Flexible: Ability to set custom page path aliases, render partials, rich navigation, and so on.

Installation
---------------------------------

Installing from gem:

    sudo apt-get install rubygems ruby-dev build-essential
    sudo gem install amber

Alternately, you can run directly from source:

    sudo apt-get install rubygems ruby-dev build-essential
    sudo gem install bundler
    git clone https://github.com/leapcode/amber
    cd amber
    bundle install
    sudo ln -s `pwd`/bin/amber /usr/local/bin

Directory structure
---------------------------------

A simple website has this structure:

    mysite/
      amber/
        config.rb
        locales/
          en.yml
        menu.txt
        layouts/
          default.haml
      pages/
        page1.en.md
        page2.en.md
      public/
        page1.en.html
        page1.en.html

menu.txt
---------------------------------

A page does not show up in the navigation unless it appears in menu.txt.

The order in menu.txt determines the order in the navigation. For example:

    aaa
    ccc
    bbb

Page hierarchy is represented by two spaces:

    aaa
      bbb
      ccc
        ddd
    eee

The items in the menu.txt file must match the names of the pages (the filename with the suffix and locale stripped away).

Supported syntaxes
---------------------------------

Depending the the file extension, the file with be parsed like so:

    .haml       -- HAML
    .md         -- Markdown
    .markdown   -- Markdown
    .text       -- Textile
    .textile    -- Textile
    .html       -- HTML

You can use the native linking method for each markup, or you can use the Amber link method:

    [[label -> page-name]]
    or
    [[label => page-name]]
    or
    [[label -> absolute-url]]
    or
    [[page-name]]

In these examples, `page-name` can be the name of the page, optionally
qualified by some context. For example, if you had two pages called
`security`, you could link to `[[chat/security]]` or `[[email/security]]`.

This double bracket link notation will automatically find the right
path for the page with the specified name. Also, Amber will warn you if the page
name is missing and it will ensure that the link is created with the correct
language prefix. In haml, you can get the same effect using
`link 'label' => 'page-name'`

If writing in a right to left script, the links look like:

    [[page-name <= label]]

But only if your text editor correctly displays right to left script.

Previewing pages
-------------------------------------

To preview your site, run `amber server`. Alternately, there are a couple
options to preview your source files without needing to run the web server:

* Markdown preview for Chrome: https://chrome.google.com/webstore/detail/markdown-preview/jmchmkecamhbiokiopfpnfgbidieafmd
* Markdown preview for Sublime: https://github.com/revolunet/sublimetext-markdown-preview
* Markdown preview for Firefox: https://addons.mozilla.org/de/firefox/addon/markdown-viewer/  (see https://addons.mozilla.org/en-US/firefox/addon/markdown-viewer/reviews/423328/ for rendering .md file extensions)

Setting page properties
---------------------------------

HAML files are rendered as templates, but the other lightweight markup files are treated as static files.

The one exception is that every file can have a "properties header". It looks like this:

    @title = "A fine page"
    @toc = false

    continue on here with body text.

The properties start with '@' and are stripped out of the source file before it is rendered. Property header lines are evaluated as ruby. All properties are optional and they are inherited, including `@title`.

Available properties:

* `@title` -- The title for the page, appearing as in an H1 on the top of the page and as the HTML title. Also used for navigation title if `@nav_title` is not set. The inline H1 title does not appear unless `@title` is explicitly set for this page (i.e. the inherited value of `@title` does not trigger the automatic H1).
* `@nav_title` -- The title for the navigation to this page, as well as the HTML title if @title is not set.
* `@toc` -- If set to `false`, don't include a table of contents when rendering the file. This only applies to .rst and .md files.
* `@layout` -- Manually set the layout template to use for rendering this page.
* `@author` -- The author credit for the page.
* `@this.alias` -- Alternate paths for the page to be rendered on. May be an array. The first path will be used when linking.

To make a property none-inheritable, specify it like so: `@this.layout = 'home'`. For some properties, like `alias`, it does not make sense for the property to be inheritable.

Troubleshooting
-----------------------------------

If you get an error at some point like this:

    WARNING: Nokogiri was built against LibXML version 2.8.0, but has dynamically loaded 2.9.1

It just means that libxml has been upgraded since the gem nokogiri was installed. To fix, run this:

    sudo gem pristine nokogiri


