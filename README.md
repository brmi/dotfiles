# Setup a Ruby on Rails enviroment for OSX

These steps will help you through setting up a new computer with the following:

## Table of Contents

  1. [Homebrew](#homebrew)
  2. [Git](#git)
  3. [Ruby](#ruby)
  4. [Rails](#rails)
  5. [Databases](#databases)
  6. [Sublime](#sublime)
  7. [Environment Settings](#environment-settings)
  8. [Iterm](#iterm)
  9. [Git Commands](#git-commands)
  10. [Gems](#gems)
  11. [Yard](#yard)
  12. [References](#references)

## Homebrew

First, we need to install Homebrew. Homebrew allows us to install and compile software packages easily from source.
Homebrew comes with a very simple install script. When it asks you to install XCode CommandLine Tools, say yes.
Open Terminal and run the following command:

        ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

## Git

We'll be using Git for our version control system so we're going to set it up to match our Github account. If you don't already have a Github account, make sure to register. It will come in handy for the future.
Replace the example name and email address in the following steps with the ones you used for your Github account.

        git config --global color.ui true
        git config --global user.name "YOUR NAME"
        git config --global user.email "YOUR@EMAIL.com"
        ssh-keygen -t rsa -C "YOUR@EMAIL.com"

The next step is to take the newly generated SSH key and add it to your Github account. You want to copy and paste the output of the following command and paste it here.

        cat ~/.ssh/id_rsa.pub

Once you've done this, you can check and see if it worked:

        ssh -T git@github.com

You should get a message like this:
Hi excid3! You've successfully authenticated, but GitHub does not provide shell access.

## Ruby

Choose the version of Ruby you want to install (e.g 2.4.2).
Now that we have Homebrew installed, we can use it to install Ruby.

We're going to use rbenv to install and manage our Ruby versions.

To do this, run the following commands in your Terminal:

        brew install rbenv ruby-build

        # Add rbenv to bash so that it loads every time you open a terminal
        echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
        source ~/.bash_profile

        # Install Ruby
        rbenv install 2.4.2
        rbenv global 2.4.2
        ruby -v

## Rails

Choose the version of Rails you want to install (e.g 5.1.4).

Installing Rails is as simple as running the following command in your Terminal:

        gem install rails -v 5.1.4

Rails is now installed, but in order for us to use the rails executable, we need to tell rbenv to see it:

        rbenv rehash

And now we can verify Rails is installed:

        rails -v
        # Rails 5.1.4

## Databases

**SQLite**

        brew install sqlite3

Rails ships with sqlite3 as the default database. Chances are you won't want to use it because it's stored as a simple file on disk. You'll probably want something more robust like MySQL or PostgreSQL.

**MySQL**

You can install MySQL server and client from Homebrew:

        brew install mysql

Once this command is finished, it gives you a couple commands to run. Follow the instructions and run them:

        # To have launchd start mysql at login:
        brew services start mysql

By default the mysql user is root with no password.

**PostgreSQL**

You can install PostgreSQL server and client from Homebrew:

        brew install postgresql

Once this command is finished, it gives you a couple commands to run. Follow the instructions and run them:

        # To have launchd start postgresql at login:
        brew services start postgresql

By default the postgresql user is your current OS X username with no password. For example, my OS X user is named chris so I can login to postgresql with that username.

**Final Steps**

And now for the moment of truth. Let's create your first Rails application:

        rails new myapp

        #### If you want to use MySQL
        rails new myapp -d mysql

        #### If you want to use Postgres
        # Note you will need to change config/database.yml's username to be
        # the same as your OSX user account. (for example, mine is 'chris')
        rails new myapp -d postgresql

        # Move into the application directory
        cd myapp

        # If you setup MySQL or Postgres with a username/password, modify the
        # config/database.yml file to contain the username/password that you specified

        # Create the database
        rake db:create

        rails server

You can now visit http://localhost:3000 to view your new website!

## Sublime

Installation:

Install [sublime-text-3](https://www.sublimetext.com/3).

Setup:

	# Create symlink
	ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl

	# Add below path to your .bash_profile or .bashrc
	export PATH=/bin:/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin:$PATH
	export EDITOR='subl -w'

	# Runs below for immediate command:
	source ~/.bash_profile

Theme:

- Ayu (ayu-mirage.sublime-theme)

Sublime Packages:

The simplest method of installation is through the Sublime Text console. The console is accessed via the ctrl+` shortcut or the View > Show Console menu. Once open, paste the appropriate Python code for your version of Sublime Text into the console. Code is available [here](https://packagecontrol.io/installation).

- AdvancedNewFile
- Better Coffeescript
- DashDoc
- DocBlockr
- Emmet
- GitGutter
- Markdown Preview
- Whitespace
- Beautify Ruby
- SublimeCodeIntel
- SublimeLinter

User Settings:

```JSON
{
    "color_scheme": "Packages/ayu/ayu-mirage.tmTheme",
    "ensure_newline_at_eof_on_save": true,
    "font_size": 11,
    "ignored_packages":
    [
        "Vintage"
    ],
    "rulers":
    [
        50,
        80
    ],
    "run_on_save": true,
    "tab_size": 2,
    "theme": "ayu-mirage.sublime-theme",
    "translate_tabs_to_spaces": true,
    "ui_font_size_small": true,
    "ui_separator": true,
    "ui_sidebar_highlight_row": true
}
```

Pry Keybinding:

```JSON
[
  { "keys": ["alt+super+p"], "command": "insert", "args": {"characters": "require 'pry';binding.pry"} },
  { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} },
]
```

Syntax Specific for Whitespace package:
```
{
    "remove_trailing_whitespace_on_save": true,
    "ensure_single_trailing_newline": true,
    "ignore_whitespace_only_lines": false,
    "ignore_whitespace_on_current_line": true
}
```

## Environment Settings

1. Fork this project into your own Github account
2. Clone on your machine:

        git clone https://github.com/<YOUR_GITHUB_USERNAME>/dotfiles.git ~/bin

3. Make sure the `.bashrc` file is loaded every time you open the Terminal:

        echo "if [ -f ~/.bashrc ]; then
          source ~/.bashrc
        fi" > ~/.bash_profile

4. Load **environment variables**, **aliases** and shell **settings**:

        ln -sf ~/bin/dotfiles/bashrc     ~/.bashrc

5. Enter your Github credentials in [gitconfig](http://git.io/-MEnNw), then load the **git settings**:

        ln -sf ~/bin/dotfiles/git/config  ~/.gitconfig

6. Load **git ignore settings**:

        ln -sf ~/bin/dotfiles/git/ignore  ~/.gitignore

6. Load **git global hooks**:

		mkdir -p ~/.git_template/hooks/
        ln -sf ~/bin/dotfiles/git/pre-commit  ~/.git_template/hooks/

7. Load **rubygems settings**:

        ln -sf ~/bin/dotfiles/gemrc      ~/.gemrc

8. Load **pry settings** for Rails:

        ln -sf ~/bin/dotfiles/pryrc      ~/.pryrc

9. Load **SSH settings**:

        mkdir -p ~/.ssh
        ln -sf ~/bin/dotfiles/ssh/config ~/.ssh/config

10. Load **git prompt** support:

        source ~/.bashrc
        vcprompt-install

11. Load **hub** support (optional):

        hub-install

12. Go and edit your aliases, configuration, settings, then push to your Github account!

# Iterm

1. Install [Iterm2 ](https://iterm2.com/downloads/stable/latest)
2. Launch -> Preferences (Command + ,), -> Profiles -> Add a profile by clicking + -> Set as default profile
3. Under the same profile go to Colors -> Color Presets -> Import -> Navigate to ayu.itermcolors under this project.

# Git Commands

**Remote GitHub**

- Create a new repository on the command line

        echo "# foodie" >> README.md
        git init
        git add README.md
        git commit -m "first commit"
        git remote add origin git@github.com:S1v4/foodie.git
        git push -u origin master

- Push an existing repository from the command line

        git remote add origin git@github.com:S1v4/foodie.git
        git push -u origin master

**CONFIGURE TOOLING - Configure user information for all local repositories**

        git config --global user.name "[name]"

Sets the name you want atached to your commit transactions

        git config --global user.email "[email address]"

Sets the email you want atached to your commit transactions

        git config --global color.ui auto

Enables helpful colorization of command line output

**CREATE REPOSITORIES - Start a new repository or obtain one from an existing URL**

        git init [project-name]

Creates a new local repository with the specified name

        git clone [url]

Downloads a project and its entire version history

**MAKE CHANGES - Review edits and craf a commit transaction**

        git status

Lists all new or modified files to be commited

        git add [file]

Snapshots the file in preparation for versioning

        git reset [file]

Unstages the file, but preserve its contents

        git diff

Shows file differences not yet staged

        git diff --staged

Shows file differences between staging and the last file version

        git commit -m "[descriptive message]"

Records file snapshots permanently in version history

**GROUP CHANGES - Name a series of commits and combine completed efforts**

        git branch

Lists all local branches in the current repository

        git branch [branch-name]

Creates a new branch

        git checkout [branch-name]

Switches to the specified branch and updates the working directory

        git merge [branch]

Combines the specified branch’s history into the current branch

        git branch -d [branch-name]

Deletes the specified branch

**REFACTOR FILENAMES - Relocate and remove versioned files**

        git rm [file]

Deletes the file from the working directory and stages the deletion

        git rm --cached [file]

Removes the file from version control but preserves the file locally

        git mv [file-original] [file-renamed]

Changes the file name and prepares it for commit

**SUPPRESS TRACKING - Exclude temporary files and paths**

        git ls-files --other --ignored --exclude-standard

Lists all ignored files in this project

        *.log
        build/
        temp-*

A text file named .gitignore suppresses accidental versioning of files and paths matching the specified paterns.

**SAVE FRAGMENTS - Shelve and restore incomplete changes**

        git stash

Temporarily stores all modified tracked files

        git stash list

Lists all stashed changesets

        git stash pop

Restores the most recently stashed files

        git stash drop

Discards the most recently stashed changeset

**REVIEW HISTORY - Browse and inspect the evolution of project files**

        git log

Lists version history for the current branch

        git log --follow [file]

Lists version history for a file, including renames

        git diff [first-branch]...[second-branch]

Shows content differences between two branches

        git show [commit]

Outputs metadata and content changes of the specified commit

**REDO COMMITS - Erase mistakes and craf replacement history**

        git reset [commit]

Undoes all commits afer [commit], preserving changes locally

        git reset --hard [commit]

Discards all history and changes back to the specified commit

**SYNCHRONIZE CHANGES - Register a repository bookmark and exchange version history**

        git fetch [bookmark]

Downloads all history from the repository bookmark

        git merge [bookmark]/[branch]

Combines bookmark’s branch into current local branch

        git push [alias] [branch]

Uploads all local branch commits to GitHub

        git pull

Downloads bookmark history and incorporates changes

**Alias from this project**

        alias  gs='git status'
        alias  gd='git diff'
        alias gdh='git diff HEAD HEAD~'
        alias gdc='git diff --cached'
        alias gdf='git diff --name-status'
        alias  gh='git show'
        alias gsf='git show --name-status'
        alias  ga='git add'
        alias  gl='git log'
        alias  gf='git fetch'
        alias gl-='git log --follow --'
        alias glf='git log --pretty=medium'
        alias ghh='git show HEAD'
        alias glm='git log --pretty=full --author `git config user.name`'
        alias ghm='git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short'
        alias  gu='git pull'
        alias  gv='git mv'
        alias gli='git update-index --assume-unchanged' # locally ignore changes
        alias glu='git update-index --no-assume-unchanged' # stop ignoring changes
        alias  gc='git commit'
        alias  go='git checkout'
        alias gcp='git cherry-pick'
        alias  gb="git for-each-ref --sort=committerdate refs/heads/ --format='%1B[0;33m%(authordate:short)%1B[0;31m %(refname:short)%1B[m | %(subject)' refs/heads"
        alias gbr='git branch -r'
        alias  gm='git merge'
        alias gmf='git merge --no-ff --no-edit'
        alias gom='git checkout master'
        alias gos='git checkout staging'
        alias gomm='git checkout origin/master -b master'
        alias gmm='git merge master'
        alias grm='git rebase master'
        alias grs='git rebase staging'
        alias  gr='git rebase --preserve-merges'
        alias  gp='git push -u'
        alias  gt='git stash -u'
        alias gta='git stash apply'
        alias gca='git add -A . && git commit --amend --no-edit'
        alias grh='git reset HEAD'
        alias grhh='git reset --hard HEAD'
        alias  gla='git shortlog -ns' # Summary by author
        alias  gbn="git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'" # current branch name
        alias  gra='git rebase --abort'
        alias  grc='git rebase --continue'
        alias  gpr='git branch | cut -c 3- | grep -v master | xargs -L1 git branch -d 2> /dev/null' # Prune already merged local branches
        alias  gwh='git branch -a | grep' # git which: which remote branch matches the regex
        alias grpo='git remote prune origin'
        alias   g!='git fetch && git rebase origin/master && gpr'
        alias gbpurge='git branch --merged | grep -v "\*" | grep -v "master" | grep -v "develop" | grep -v "staging" | xargs -n 1 git branch -d'

# Gems

Ensure that the name of the gem is not taken on [RubyGems](https://rubygems.org/).
Create a new repository on GitHub by New -> Repository name -> Public -> Create repository.
Do not change anything else for making gems - README and license are taken care of by `bundle gem gem_name`.

This guide was made using version 1.9.0 of bundler. We can follow along with other versions, but we might not get the exact same output. To check which version of bundler we currently have, lets run the following command:

        bundle -v

We should see something close to `Bundler version 1.9.0`. If necessary, we can update to the newest version of Bundler by running

        gem update bundler

To begin to create a gem using Bundler, use the bundle gem command like this:

        bundle gem foodie

Complete the TODO's under the foodie.gemspec like such:

```ruby
# coding: utf-8
lib = File.expand_path("../lib", __FILE__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require "foodie/version"

Gem::Specification.new do |spec|
  spec.name          = "foodie"
  spec.version       = Foodie::VERSION
  spec.authors       = ["s1v4"]
  spec.email         = ["hdao61@gmail.com]

  spec.summary       = %q{Returns something..}
  spec.homepage      = "https://github.com/S1v4/foodie"
  spec.license       = "MIT"

  spec.required_ruby_version = '>= 2.4'

  spec.files         = `git ls-files -z`.split("\x0").reject do |f|
    f.match(%r{^(test|spec|features)/})
  end
  spec.bindir        = "exe"
  spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
  spec.require_paths = ["lib"]

  spec.add_runtime_dependency "docx", "~> 1.2.1"
  spec.add_development_dependency "yard", "~> 0.9.9"
  spec.add_development_dependency "bundler", "~> 1.15"
  spec.add_development_dependency "rake", "~> 10.0"
  spec.add_development_dependency "rspec", "~> 3.0"
  spec.add_development_dependency "coveralls"
  spec.add_development_dependency "pry-nav"
end
```

Release:

        git commit -m "first commit"
        git remote add origin git@github.com:S1v4/foodie.git
        git push -u origin master
        rake build
        gem install pkg/foodie-0.1.0.gem
        rake release

For later versions follow:

        gem bump --version minor # bumps to the next minor version
        gem bump --version major # bumps to the next major version
        gem bump --version 1.1.1 # bumps to the specified version
        rake release

# Yard

**Modules**

        # Namespace for classes and modules that handle serving documentation over HTTP
        # @since 0.6.0

**Classes**

        # Abstract base class for CLI utilities. Provides some helper methods for
        # the option parser
        #
        # @author Full Name
        # @abstract
        # @since 0.6.0
        # @attr [Types] attribute_name a full description of the attribute
        # @attr_reader [Types] name description of a readonly attribute
        # @attr_writer [Types] name description of writeonly attribute
        # @deprecated Describe the reason or provide alt. references here

**Methods**

        # An alias to {Parser::SourceParser}'s parsing method
        #
        # @author Donovan Bray
        #
        # @see http://example.com Description of URL
        # @see SomeOtherClass#method
        #
        # @deprecated Use {#my_new_method} instead of this method because
        #   it uses a library that is no longer supported in Ruby 1.9.
        #   The new method accepts the same parameters.
        #
        # @abstract
        # @private
        #
        # @param [Hash] opts the options to create a message with.
        # @option opts [String] :subject The subject
        # @option opts [String] :from ('nobody') From address
        # @option opts [String] :to Recipient email
        # @option opts [String] :body ('') The email's body
        #
        # @param (see User#initialize)
        # @param [OptionParser] opts the option parser object
        # @param [Array<String>] args the arguments passed from input. This
        #   array will be modified.
        # @param [Array<String, Symbol>] list the list of strings and symbols.
        #
        # The options parsed out of the commandline.
        # Default options are:
        #   :format => :dot
        #
        # @example Reverse a string
        #   "mystring.reverse" #=> "gnirtsym"
        #
        # @example Parse a glob of files
        #   YARD.parse('lib/**/*.rb')
        #
        # @raise [ExceptionClass] description
        #
        # @return [optional, types, ...] description
        # @return [true] always returns true
        # @return [void]
        # @return [String, nil] the contents of our object or nil
        #   if the object has not been filled with data.
        #
        # We don't care about the "type" here:
        # @return the object
        #
        # @return [String, #read] a string or object that responds to #read
        # @return description here with no types

**Anywhere**

        # @todo Add support for Jabberwocky service
        #   There is an open source Jabberwocky library available
        #   at http://somesite.com that can be integrated easily
        #   into the project.

**Blocks**

        # for block {|a, b, c| ... }
        # @yield [a, b, c] Description of block
        #
        # @yieldparam [optional, types, ...] argname description
        # @yieldreturn [optional, types, ...] description

# References

This guide is complete from the help of others:

- [Go Rails](https://gorails.com/setup/osx/10.12-sierra)
- [GitHub Services](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
- [bundler.io](https://bundler.io/v1.15/guides/creating_gem.html)
- [Chetan Sarva](https://gist.github.com/chetan/1827484)

#
