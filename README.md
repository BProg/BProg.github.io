# Notes

## Required tools

[zola](https://www.getzola.org/) - for static site genration

[gh-pages](https://www.npmjs.com/package/gh-pages) - for publish to github pages

## Setup

```sh
# Install [zola](https://www.getzola.org/)
brew install zola
# Install hyde theme in theme folder
cd theme; git clone git@github.com:getzola/hyde.git
# Install volta 
curl https://get.volta.sh | bash
# Install gh-pages, version 2.2.0 works as expected
volta install gh-pages@2.2.0
```

## Workflow

Work in the `dev` branch. publish to `master` branch

## Publish site

on the dev branch, run

```sh
zola build
gh-pages -d public -b master -m "Publish from <hash>"`
```
