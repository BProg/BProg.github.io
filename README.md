# Notes

## Required tools

[zola](https://www.getzola.org/) - for static site genration

[gh-pages](https://www.npmjs.com/package/gh-pages) - for publish to github pages

## Setup

```sh
# Install [zola](https://www.getzola.org/)
$ brew install zola
# Install hyde theme in theme folder
$ cd theme; git clone git@github.com:getzola/hyde.git
# Install volta 
$ curl https://get.volta.sh | bash
# Install gh-pages
$ volta install gh-pages
```

## Workflow

Work in the `dev` branch. publish to `master` branch

## Publish site

use: `$ gh-pages -d public -b master -m "Publish from 362579e"`
