---
layout: post
title: "Setting Up GitHub Pages &mdash; Part II"
subtitle: "Amateur Customization"
date: 2018-12-28
header-style: text
tags:
    - personal website
    - GitHub Pages
    - Jekyll
---
(Last updated: August 12, 2021)

I'm going to do some customization to the [blog template I borrowed](https://github.com/Huxpro/huxpro.github.io).
This requires some front-end development environments and techniques.
I'll just make this post the second episode of my website-building series.

- In _config.yml add pagination_path and set it to `/blog/:num/`.
(See [How to use Jekyll-paginate without index.html?](https://stackoverflow.com/questions/46182805/how-to-use-jekyll-paginate-without-index-html).)
- Navigation bar: order of tabs. add a `_data` folder and create a `nav.yml`.
(See [How to stop pagenator-v2-generated pages from appearing in nav menu](https://talk.jekyllrb.com/t/how-to-stop-pagenator-v2-generated-pages-from-appearing-in-nav-menu/1465) and [Jekyll creating navigation for blog pages](https://stackoverflow.com/questions/48713854/jekyll-creating-navigation-for-blog-pages).)
<!-- - [Google AdSense](https://www.google.com/adsense/start/) -->
-  google-site-verification in head.html. (See https://stackoverflow.com/questions/57384269/github-pages-blog-and-google-search-console-is-it-safe-to-follow-these-steps-fo)
    - Verify ownership on Google.
- Favicon: Create our own website icon with [RealFaviconGenerator](https://realfavicongenerator.net/).
- Google Analytics, Disqus in _config.yml
- Choose some nice background pictures.

# Installing Necessary Environments

The template indicates that [Grunt](https://gruntjs.com/), a JavaScript task runner, is required, which is "installed and manged via [npm](https://www.npmjs.com/), the [Node.js](https://nodejs.org/en/) package manager" ([Getting started - Grunt](https://gruntjs.com/getting-started)), which in turn requires Node.js, yet npm "strongly recommend[s] using a Node version manager to install Node.js and npm" ([Downloading and installing Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)).

First check the version of Node.js and npm, or whether they are installed:
```bash
$ node -v
$ npm -v
```
Then install [nvm](https://github.com/creationix/nvm), the Node Version Manager, following nvm's instructions.
Install script:
```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```
Run the following to make nvm work (indicated by the installation result):
```bash
$ export NVM_DIR="$HOME/.nvm"
$ [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
$ [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
Verify installation:
```bash
$ command -v nvm
# should output nvm:
nvm
```
Install the latest LTS (Long Term Support) node:
```bash
$ nvm install --lts
# result:
Now using node v10.15.0 (npm v6.4.1)
```
Update npm to the latest version ([Getting started - Grunt](https://gruntjs.com/getting-started)):
```bash
$ npm update -g npm # updated to npm v6.5.0
```
Install Grunt's CLI (command line tool):
```bash
$ npm install -g grunt-cli
```
Very smooth this time, probably thanks to the troubleshooting in [Part I]({{ site.baseurl }}{% post_url 2018-05-31-github-pages-1-installation %}).

# Hacking Style Sheet

Now I shall work on the Grunt project to modify the style.
According to [Hux](https://github.com/Huxpro/huxpro.github.io#customization), Grunt environment performs tasks like "minification of the JavaScript, compiling of the LESS files, adding banners to keep the Apache 2.0 license intact, and watching for changes".
(Seems like a one-stop resolution, which is perfect for us.)

Simply follow [Working with an existing Grunt project](https://gruntjs.com/getting-started#working-with-an-existing-grunt-project) on [Getting started - Grunt](https://gruntjs.com/getting-started) to configure `package.json` and `Gruntfile.js`.
First `cd` to the GitHub Pages directory and install project dependencies with `npm install`:
```bash
$ cd somepath/username.github.io
$ npm install
```
(Note that you might want to take a look at the `package.json` to change the name, title, author, etc. of your project.
This will rename some `*.min.js` and `*.css` files, so you need to change the theme JavaScript source (src) in `head.html` and `footer.html`.)

`npm install` results in warnings that the original `package.json` has deprecated dependencies, such as
```bash
npm WARN deprecated coffee-script@1.3.3: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
added 145 packages from 155 contributors and audited 215 packages in 11.577s
found 31 vulnerabilities (10 low, 14 moderate, 7 high)
  run `npm audit fix` to fix them, or `npm audit` for details
```
Simply do what it says and the versions in `package.json` should be updated. 
Finally `grunt` should work fine.
In my case I just need to locate and modify the style settings that I want to change in the `*.less` file, and then run `grunt` to let it take care of all the compiling work and beyond.