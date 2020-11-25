# record-locator-specification

An API for retrieving pointers to records and documents from a Record Locator (RL).

## Table of Contents

- [Quick start](#quick-start)
- [Contributing](#contributing)
- [Versioning](#versioning)
- [Copyright and license](#copyright-and-license)

## Quick Start

Option 1:

- View the [latest release](https://nhsconnect.github.io/record-locator-specification/index.html)

Option 2 (running documentation locally):

- Clone the repo: `git clone https://github.com/nhsconnect/record-locator-specification`
- Delete the `Gemfile.lock` file in the project root.
- Install [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - If installing on Windows, use the [RubyInstaller](https://rubyinstaller.org/).
- Install `bundler`:
    - `$ gem install bundler`
- Install the dependencies:
    - `$ bundle`
    - `$ bundle lock --add-platform ruby`
    - `$ bundle lock --add-platform x86-mingw32`
    - `$ bundle lock --add-platform x64-mingw32`
- Run locally:
    - `$ bundle exec jekyll serve`
    - Browse to [`localhost:4005`](http://localhost:4005)

Learn more about using Jekyll by reading its [documentation](https://jekyllrb.com/docs/home/).

## Contributing

Notice a defect or have a feature request? Check the existing [issues](https://github.com/nhsconnect/record-locator-specification/issues) first. If your problem or idea is not addressed yet, feel free to [open a new issue](https://github.com/nhsconnect/record-locator-specification/issues/new).

## Versioning

For transparency into our release cycle and in striving to maintain backward compatibility, this repo is maintained under [the Semantic Versioning guidelines](http://semver.org/).

See the [JIRA releases](https://github.com/nhsconnect/record-locator-specification/releases) or [release notes](https://nhsconnect.github.io/record-locator-specification/overview_release_notes.html) page for version changelogs.

## Copyright and License

Copyright 2011-2020:

- NHS Digital Code released under the [MIT License](https://opensource.org/licenses/MIT).
- Documentation released under [Creative Commons License](https://creativecommons.org/licenses/by-sa/4.0/).
