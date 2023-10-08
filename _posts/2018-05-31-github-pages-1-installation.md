---
layout: post
title: "Setting Up GitHub Pages &mdash; Part I"
subtitle: "An Installation Manual"
date: 2018-05-31
header-style: text
tags:
    - personal website
    - GitHub Pages
    - Jekyll
---
(Last updated: August 12, 2021)

I'm going to build my personal website on GitHub Pages with Jekyll.
This article is sort of a manual/log of setting up the required local environments on macOS.
So, let it be the very first post of my blog.

Before everything, one needs to learn a little bit about [Git](https://git-scm.com/), [GitHub](https://github.com/), [GitHub Pages](https://pages.github.com/), and [Jekyll](https://jekyllrb.com/). [Working with GitHub Pages](https://docs.github.com/en/github/working-with-github-pages) is very helpful. Read the official documents and follow their steps should be sufficient, unless we encounter any errors, for which [Stack Overfow](https://stackoverflow.com/) or some blogs can be very useful. No need to read second-hand tutorials or blogs. (Seems like I'm writing one though.)

# Setting up the environment

Essentially, one could follow the steps in [Testing your GitHub Pages site locally with Jekyll - GitHub Docs](https://docs.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll), [Installation - Jekyll](https://jekyllrb.com/docs/installation/), and the detailed guide [Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/). I'm just gonna put down everything I did.

[//]: # (But some errors can be quite annoying and not easy to handle by amateur programmers like me. Here is what I encountered.)

First check the original version of Ruby. 2.4.0 or higher is required by Jekyll as of January 2021. (My macOS version is Big Sur 11.1.)
In Terminal, run:
```zsh
$ ruby -v
# Mine is:
$ ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin20]
```
Ruby is included in macOS by default, but we wouldn't want to directly upgrade the original Ruby in case of any incompatibility.
We mainly follow [Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/).

## Install Command Line Tools:
```zsh
$ xcode-select --install
xcode-select: note: install requested for command line developer tools
```
This would take 3 minutes.

## Install Ruby with Homebrew
Homebrew is a useful package manager for macOS. If you don't have it, just run
```zsh
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
to install it. I have installed it before on version Mojave (for the package aria2), but on Big Sur when I checked the version
```zsh
$ brew -v
```
I got errors:
```zsh
Traceback (most recent call last):
	10: from /usr/local/Homebrew/Library/Homebrew/brew.rb:23:in `<main>'
	 9: from /usr/local/Homebrew/Library/Homebrew/brew.rb:23:in `require_relative'
	 8: from /usr/local/Homebrew/Library/Homebrew/global.rb:28:in `<top (required)>'
	 7: from /usr/local/Homebrew/Library/Homebrew/global.rb:28:in `require'
	 6: from /usr/local/Homebrew/Library/Homebrew/os.rb:3:in `<top (required)>'
	 5: from /usr/local/Homebrew/Library/Homebrew/os.rb:21:in `<module:OS>'
	 4: from /usr/local/Homebrew/Library/Homebrew/os/mac.rb:58:in `prerelease?'
	 3: from /usr/local/Homebrew/Library/Homebrew/os/mac.rb:24:in `version'
	 2: from /usr/local/Homebrew/Library/Homebrew/os/mac.rb:24:in `new'
	 1: from /usr/local/Homebrew/Library/Homebrew/os/mac/version.rb:26:in `initialize'
/usr/local/Homebrew/Library/Homebrew/version.rb:368:in `initialize': Version value must be a string; got a NilClass () (TypeError)
```
Then, I got the solution from [Homebrew fails on macOS Big Sur](https://stackoverflow.com/questions/64821648/homebrew-fails-on-macos-big-sur):
```zsh
$ brew upgrade
```
This "upgrade" seems to do a lot of stuff: updating, cleaning up, etc. Now I have
```zsh
$ brew -v
Homebrew 2.7.7
```
Now we install Ruby
```zsh
$ brew install ruby
```
(Unimportant: New installations of Homebrew should be fine. An error pops up:
```zsh
Error: 
  homebrew-core is a shallow clone.
To `brew update`, first run:
  git -C /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core fetch --unshallow
This command may take a few minutes to run due to the large size of the repository.
This restriction has been made on GitHub's request because updating shallow
clones is an extremely expensive operation due to the tree layout and traffic of
Homebrew/homebrew-core and Homebrew/homebrew-cask. We don't do this for you
automatically to avoid repeatedly performing an expensive unshallow operation in
CI systems (which should instead be fixed to not use shallow clones). Sorry for
the inconvenience!
```
This is supposed to be a dated problem of Homebrew according to [this](https://stackoverflow.com/questions/45782694/how-to-remove-the-shallow-clone-warning-from-homebrew), we don't really need to know about the history, so we just follow the instructions. The unshallowing takes a few minutes.)

Then, add the brew ruby path to shell configuration:
```zsh
# To check which shell you are using, run
# echo $SHELL
# If you're using Bash, run:
# echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
# I'm using zsh, so run
$ echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
```
Relaunch terminal and check Ruby setup:
```zsh
$ which ruby
/usr/local/opt/ruby/bin/ruby

$ ruby -v
ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [x86_64-darwin20]
```
Now Ruby (and Homebrew) is ready.

## Install Jekyll and Bundler
Install the bundler and jekyll gems:
```zsh
$ gem install --user-install bundler jekyll
```

Then, write the following path to the shell configuration file. Replace "X.X" with the first two digits of your Ruby version (I have 3.0.).
```zsh
$ echo 'export PATH="$HOME/.gem/ruby/3.0.0/bin:$PATH"' >> ~/.zshrc
# If you're using Bash, run
# echo 'export PATH="$HOME/.gem/ruby/X.X.0/bin:$PATH"' >> ~/.bash_profile
```
Last step, check if "GEM PATHS:" points to your home directory:
```zsh
$ gem env
RubyGems Environment:
  - GEM PATHS:
      - /usr/local/lib/ruby/gems/3.0.0
      - /Users/username/.local/share/gem/ruby/3.0.0 # You should see your "username"
      - /usr/local/Cellar/ruby/3.0.0_1/lib/ruby/gems/3.0.0
```

Finally, the necessary environment is ready.

## Use Bundler to install necessary dependencies locally
I refer to this tutorial: [Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/).
This tutorial is useful for creating a new website.
Since we usually borrow some template and there would already exist a Gemfile in the project directory, we directly use that one and skip the step `bundle init`.
(Of course you could remove the original Gemfile and generate a new one.)
I also skip these two steps: `bundle add jekyll` and `bundle exec jekyll new --force --skip-bundle`.
(If we do `jekyll new`, it would generate some default pages and _config.yml file that will mess up everything.)

First go to your GitHub Pages directory.
```zsh
$ cd /Users/username/GitHub/githubusername.github.io
```

Configure Bundler install path to be `./vendor/bundle/` subdirectory, and add jekyll and dependencies there:
```zsh
$ bundle config set --local path 'vendor/bundle'
```
(If skip this path setting, there might be some issues related to Ruby gems. I encountered this error: `bundler: command not found: jekyll`. A [solution](https://stackoverflow.com/questions/8146249/jekyll-command-not-found) just for reference.)

Then install gem files:
```zsh
$ bundle install
```
And finally serve the site:
```zsh
$ bundle exec jekyll serve
```
(At first attempt, the serving fails due to `cannot load such file -- webrick (LoadError)`. It is because a dependency called `webrick` is missing in Ruby 3.0. [This](https://github.com/jekyll/jekyll/issues/8523) solves the problem. Simply add it by `bundle add webrick`.)

Then you should be able to visit the website locally at http://127.0.0.1:4000/ or http://localhost:4000 (by default).
Feel free to modify it and the webpage would be reloaded every time you make changes.