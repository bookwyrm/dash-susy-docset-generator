# dash-susy-docset-generator: Generate a Dash docset for Susy

Generates a [Dash](http://kapeli.com/dash) docset for [Susy](http://susy.oddbird.net).

## Requirements

* [sphinx_rtd_theme](https://github.com/snide/sphinx_rtd_theme) - for ReadTheDocs theme which is used to format the Susy docs

## Build Docset

```bash
$ bundle install  # for nokogiri
$ rake
$ open Susy.docset
```

## Todo

* Remove online dependencies (Modernizer and Google Fonts)
* Remove search JS from Docset
* Add [Table of Contents Support](http://kapeli.com/docsets#tableofcontents)

## Other

### Thanks

This build process was based on the [Rakefile](https://github.com/indirect/dash-casperjs/blob/master/Rakefile) from [dash-casperjs](https://github.com/indirect/dash-casperjs).

### License

MIT License, copyright 2015 by Matt Vanderpol


