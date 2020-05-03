# Notes

## Generate site

Use [zola](https://www.getzola.org/) tool to generate the static site. All the work is in `dev` branch. The static site is generated and saved into `public` folder.

## Publish site

Use [gh-pages](https://www.npmjs.com/package/gh-pages) tool to publish. Currently the tool is installed using [volta](https://volta.sh/) tool

use: `$ gh-pages -d public -b master -m "Publish from 362579e"`
