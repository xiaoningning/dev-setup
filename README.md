<p align="center">
  <img src="http://i.imgur.com/GMnses0.png">
</p>

dev-setup
============

Dev machine setup instructions, dotfiles/scripts, and tools. Also includes dotfiles/scripts for python data analysis, AWS, and hacker defaults for OSX. https://bit.ly/git-dotfiles

## Motivation

Setting up a new developer machine can be an ad-hoc, manual, and time-consuming task.  This repo aims to simplify the process with easy-to-understand instructions and dotfiles/scripts that automate the following:

* Customize commonly-used apps
* Customize the terminal and vim
* Setup defaults for OSX users
* Install common Homebrew formulae for OSX users
* Install common modules used for Python data analysis
* Setup the Amazon Web Services (AWS) environment
* Setup common databases

Sections Summary:
* Section 1 contains the dotfiles/scripts to setup your system (TLDR version).
* Sections 2 through 6 detail more information about what is installed in Section 1.  It also describes some intallation details if you prefer to install only specific components.

This repo builds on the awesome work from [Mathias Bynens](https://github.com/mathiasbynens) and [Nicolas Hery](https://github.com/nicolashery), listed in the [Credits](#credits).

## Section 1: Installation

* [Step 1: Update the Operating System](https://github.com/donnemartin/dev-setup#step-1-update-the-operating-system)
    * [Install Xcode Command Line Tools](https://github.com/donnemartin/dev-setup#install-xcode-command-line-tools)
    * [Optional: Install Apps](https://github.com/donnemartin/dev-setup#optional-install-apps)
* [Step 2: Run the bootstrap.sh Script](https://github.com/donnemartin/dev-setup#step-2-run-the-bootstrapsh-script)
    * [Running with Git](https://github.com/donnemartin/dev-setup#running-with-git)
    * [Running without Git](https://github.com/donnemartin/dev-setup#running-without-git)
    * [Optional: Specify PATH](https://github.com/donnemartin/dev-setup#optional-add-custom-commands)
    * [Optional: Add Custom Commands](https://github.com/donnemartin/dev-setup)
* [Step 3: Run the .osx Script](https://github.com/donnemartin/dev-setup#step-3-run-the-osx-script)
* [Step 4: Run the brew.sh Script](https://github.com/donnemartin/dev-setup#step-4-run-the-brewsh-script)
* [Step 5: Run the .pydata Script](https://github.com/donnemartin/dev-setup#step-5-run-the-pydata-script)
* [Step 6: Run the .aws Script](https://github.com/donnemartin/dev-setup#step-6-run-the-.aws-script)

## Section 2: General Apps and Tools

* [Google Chrome](#google-chrome)
* [Homebrew](#homebrew)
* [Sublime Text](#sublime-text)
* [Atom](#atom)
* [Terminal Customization](#terminal-customization)
* [Git](#git)
* [Vim](#vim)
* [Python](#python)
* [Pip](#pip)
* [Virtualenv](#virtualenv)
* [Ruby and RVM](#ruby-and-rvm)

## Section 3: Python Data Analysis

* [Anaconda](#anaconda)
* [IPython Notebook](#ipython-notebook)
* [NumPy](#numpy)
* [Pandas](#pandas)
* [Matplotlib](#matplotlib)
* [Seaborn](#seaborn)
* [Scikit-learn](#scikit-learn)
* [SciPy](#scipy)
* [Flask](#flask)
* [Bokeh](#bokeh)

## Section 4: Amazon Web Services (AWS)

* [Spark](#spark)
* [MapReduce](#mapreduce)
* [Boto](#boto)
* [S3cmd](#s3cmd)
* [S3DistCp](#s3distcp)
* [S3-parallel-put](#s3-parallel-put)
* [Redshift](#redshift)
* [Kinesis](#kinesis)
* [Lambda](#lambda)
* [AWS Machine Learning](#aws-machine-learning)

## Section 5: Databases

* [MySQL](#mysql)
* [MySQL Workbench](#mysql-workbench)
* [MongoDB](#)

## Section 6: Misc

* [Contributions](#contributions)
* [Credits](#credits)
* [License](#license)

## Section 1: Installation

### Step 1: Update the Operating System

First thing you need to do on any OS, is to update the system.  On a Mac run the "App Store" and select the "Updates" icon and update both the OS and installed apps.

#### Install Xcode Command Line Tools

An important dependency before many tools such as Homebrew can work is the **Command Line Tools** for **Xcode**. These include compilers like gcc that will allow you to build from source.  Git is also included.

Now, Xcode weighs something like 2GB, and you don't need it unless you're developing iPhone or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode.

If you are running **OS X 10.9 Mavericks or later**, then you can install the Xcode Command Line Tools directly from the command line with:

```bash
xcode-select --install
```

If you're running 10.8 or older, you'll need to go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID (the same one you use for iTunes and app purchases). Unfortunately, you're greeted by a rather annoying questionnaire. All questions are required, so feel free to answer at random.

Once you reach the downloads page, search for "command line tools", and download the latest **Command Line Tools (OS X Mountain Lion) for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

#### Optional: Install Apps

Some of the scripts tweak settings on apps such as [Google Chrome](#google-chrome) and [Sublime Text](#sublime-text).  If you use these apps, it might be useful to install them first.

### Step 2: Run the bootstrap.sh Script

The bootstrap.sh script will sync the dev-tools repo to your local machine.  This will include customizations for Vim, bash, curl, git, tab completion, aliases, a number of utility functions, etc.  Section 2 of this README describes some of the customizations.

Below you'll find two ways to run the bootstrap script, one using Git and the other using curl.

#### Running with Git

Git should have been installed from the section [Install Xcode Command Line Tools](https://github.com/donnemartin/dev-setup#install-xcode-command-line-tools).  You can clone the repository wherever you want. (I like to keep it in `~/dev/dev-setup`, with `~/dev-setup` as a symlink.) The bootstrapper script will pull in the latest version and copy the files to your home folder.

```bash
git clone https://github.com/donnemartin/dev-setup.git && cd dev-setup && source bootstrap.sh
```

To update, `cd` into your local `dev-setup` repository and then:

```bash
source bootstrap.sh
```

Alternatively, to update while avoiding the confirmation prompt:

```bash
set -- -f; source bootstrap.sh
```

#### Running without Git

To install these dotfiles without Git:

```bash
cd; curl -#L https://github.com/donnemartin/dev-setup/tarball/master | tar -xzv --strip-components 1 --exclude={README.md,bootstrap.sh,LICENSE-MIT.txt}
```

To update later on, just run that command again.

#### Optional: Specify PATH

If `~/.path` exists, it will be sourced along with the other files, before any feature testing (such as [detecting which version of `ls` is being used](https://github.com/donnemartin/dev-setup/blob/aff769fd75225d8f2e481185a71d5e05b76002dc/.aliases#L21-26)) takes place.

Here’s an example `~/.path` file that adds `/usr/local/bin` to the `$PATH`:

```bash
export PATH="/usr/local/bin:$PATH"
```

#### Optional: Add Custom Commands

If `~/.extra` exists, it will be sourced along with the other files. You can use this to add a few custom commands without the need to fork this entire repository, or to add commands you don’t want to commit to a public repository.

My `~/.extra` looks something like this:

```bash
# Git credentials
GIT_AUTHOR_NAME="Donne Martin"
GIT_COMMITTER_NAME="$GIT_AUTHOR_NAME"
git config --global user.name "$GIT_AUTHOR_NAME"
GIT_AUTHOR_EMAIL="donne.martin@gmail.com"
GIT_COMMITTER_EMAIL="$GIT_AUTHOR_EMAIL"
git config --global user.email "$GIT_AUTHOR_EMAIL"

# Pip should only run if there is a virtualenv currently activated
export PIP_REQUIRE_VIRTUALENV=true

# Install or upgrade a global package
# Usage: syspip install –upgrade pip setuptools virtualenv
gpip(){
   PIP_REQUIRE_VIRTUALENV="" pip "$@"
}
```

You could also use `~/.extra` to override settings, functions and aliases from my dotfiles repository. It’s probably better to [fork this repository](https://github.com/donnemartin/dev-setup/fork) instead, though.

### Step 3: Run the .osx Script

When setting up a new Mac, you may want to set some sensible OS X defaults.  This script also configures common third-party apps such as Chrome and Sublime Text.  For a full listing of configuration changes, refer to the commented [source file](https://github.com/donnemartin/dev-setup/blob/master/.osx) directly.  Section 2 of this README describes some of the customizations.

```bash
./.osx
```

**For your terminal customization to take full effect, quit and re-start the terminal.**

### Step 4: Run the brew.sh Script

First, install [Homebrew](http://brew.sh/), a package manager that simplifies installing and updating applications or libraries.

Run the following command and follow the steps on the screen:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

When setting up a new Mac, you may want to install some common Homebrew formulae.  For a full listing of installed formulae, refer to the commented [source file](https://github.com/donnemartin/dev-setup/blob/master/brew.sh) directly.

```bash
./brew.sh
```

This will include the latest version of Python 2 and Python 3.

Restart your terminal.  If you run into the following error, it is because brew installed an updated bash version, but it doesn't switch it to be your default:

```bash
-bash: declare: -A: invalid option
declare: usage: declare [-afFirtx] [-p] [name[=value] ...]
-bash: complete: -D: invalid option
complete: usage: complete [-abcdefgjksuv] [-pr] [-o option] [-A action] [-G globpat] [-W wordlist] [-P prefix] [-S suffix] [-X filterpat] [-F function] [-C command] [name ...]
```

Add the newly installed shell to the list of allowed shells:

```bash
sudo bash -c 'echo /usr/local/bin/bash >> /etc/shells'
```

Change to the new shell:

```bash
chsh -s /usr/local/bin/bash
```

**For your terminal customization to take full effect, quit and re-start the terminal.**

### Step 5: Run the pydata.sh Script

If you'd like to set up a development environment to work with Python and data analysis, run the following script:

```bash
./pydata.sh
```

Section 3 of this README describes the installed packages and usage.

### Step 6: Run the aws.sh Script

[Coming soon]

If you'd like to set up a development environment to work Amazon Web Services, run the following script:

```bash
./aws.sh
```

Section 4 of this README describes the installed packages and usage.

## Section 2: General Apps and Tools

### Google Chrome

Install your favorite browser, mine happens to be Chrome.

Download from [www.google.com/chrome](https://www.google.com/intl/en/chrome/browser/). Open the **.dmg** file once it's done downloading (this will mount the disk image), and drag and drop the **Google Chrome** app into the Applications folder (on the Mac, most applications are installed this way). When done, you can unmount the disk in Finder (the small "eject" icon next to the disk under **Devices**).

#### Configuration

The section [Step 3: Run the .osx Script](https://github.com/donnemartin/dev-setup#step-3-run-the-osx-script) contains Chrome configurations.

### Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

#### Configuration

The section [Step 4: Run the brew.sh Script](https://github.com/donnemartin/dev-setup#step-4-run-the-brewsh-script) installs Homebrew and a number of useful Homebrew formulae.

#### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>

To update Homebrew's directory of formulae, run:

    $ brew update

**Note**: I've seen that command fail sometimes because of a bug. If that ever happens, run the following (when you have Git installed):

    $ cd /usr/local
    $ git fetch origin
    $ git reset --hard origin/master

To see if any of your packages need to be updated:

    $ brew outdated

To update a package:

    $ brew upgrade <formula>

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

    $ brew cleanup

To see what you have installed (with their version numbers):

    $ brew list --versions

### Sublime Text

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

Go ahead and [download](http://www.sublimetext.com/) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

**Note**: At this point I'm going to create a shortcut on the OS X Dock for both for Sublime Text. To do so, right-click on the running application and select **Options > Keep in Dock**.

Sublime Text is not free, but I think it has an unlimited "evaluation period". Anyhow, we're going to be using it so much that even the seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome tool.

#### Configuration

The section [Step 3: Run the .osx Script](https://github.com/donnemartin/dev-setup#step-3-run-the-osx-script) contains Sublime Text configurations.

#### Soda Theme

The [Soda Theme](https://github.com/buymeasoda/soda-theme) is a great UI theme for Sublime Text, especially if you use a dark theme and think the side bar sticks out like a sore thumb.

##### Installation with Sublime Package Control

If you are using Will Bond's excellent [Sublime Package Control](http://wbond.net/sublime_packages/package_control), you can easily install Soda Theme via the `Package Control: Install Package` menu item. The Soda Theme package is listed as `Theme - Soda` in the packages list.

##### Installation with Git

Alternatively, if you are a git user, you can install the theme and keep up to date by cloning the repo directly into your `Packages` directory in the Sublime Text application settings area.

You can locate your Sublime Text `Packages` directory by using the menu item `Preferences -> Browse Packages...`.

While inside the `Packages` directory, clone the theme repository using the command below:

```bash
git clone https://github.com/buymeasoda/soda-theme/ "Theme - Soda"
```

##### Activating the Theme on Sublime Text 2

* Open your User Settings Preferences file `Sublime Text 2 -> Preferences -> Settings - User`
* Add (or update) your theme entry to be `"theme": "Soda Light.sublime-theme"` or `"theme": "Soda Dark.sublime-theme"`

**Example Sublime Text 2 User Settings**

    {
        "theme": "Soda Light.sublime-theme"
    }

##### Activating the Theme on Sublime Text 3

* Open your User Settings Preferences file `Sublime Text -> Preferences -> Settings - User`
* Add (or update) your theme entry to be `"theme": "Soda Light 3.sublime-theme"` or `"theme": "Soda Dark 3.sublime-theme"`

**Example Sublime Text 3 User Settings**

    {
        "theme": "Soda Light 3.sublime-theme"
    }

### Atom

[Atom](https://github.com/atom/atom) is a great open-source editor from GitHub that is rapidly gaining contributors and popularity.  Unfortunately I have found that it does not perform as well when working with very large files that you typically encounter while working with data.  As Atom matures, I'm hopeful its performance will improve.

### Terminal Customization

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place.

#### Configuration

The sections [Step 2: Run the bootstrap.sh Script](https://github.com/donnemartin/dev-setup#step-2-run-the-bootstrapsh-script) and [Step 3: Run the .osx Script](https://github.com/donnemartin/dev-setup#step-3-run-the-osx-script) contain terminal customizations.

### Git

#### Configuration

Git should have been installed when you ran through the [Install Xcode Command Line Tools](https://github.com/donnemartin/dev-setup#install-xcode-command-line-tools) section.

#### Usage

What's a developer without [Git](http://git-scm.com/)?

    $ git --version

And `$ which git` should output `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](https://raw.githubusercontent.com/donnemartin/dev-setup/master/.gitconfig) file to your home directory:

    $ cd ~
    $ curl -O https://raw.githubusercontent.com/donnemartin/dev-setup/master/.gitconfig

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

    $ git config --global user.name "Your Name Here"
    $ git config --global user.email "your_email@youremail.com"

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, we're going to use the recommended HTTPS method (versus SSH). So you don't have to type your username and password everytime, let's enable Git password caching as described [here](https://help.github.com/articles/set-up-git):

    $ git config --global credential.helper osxkeychain

**Note**: On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](https://github.com/donnemartin/dev-setup/blob/master/.gitignore) file for inspiration.

### Vim

Although Sublime Text will be our main editor, it is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. After that it's just remembering a few important keys.

#### Configuration

The sections [Step 2: Run the bootstrap.sh Script](https://github.com/donnemartin/dev-setup#step-2-run-the-bootstrapsh-script) and [Step 3: Run the .osx Script](https://github.com/donnemartin/dev-setup#step-3-run-the-osx-script) contain Vim customizations.

### Python

OS X, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version with Homebrew. It will also allow us to get the very latest version of Python 2.7 and Python 3.

The section [Step 4: Run the brew.sh Script](https://github.com/donnemartin/dev-setup#step-4-run-the-brewsh-script) installs the latest versions of Python 2 and Python 3.

### Pip

Here are a couple Pip commands to get you started. To install a Python package:

    $ pip install <package>

To upgrade a package:

    $ pip install --upgrade <package>

To see what's installed:

    $ pip freeze

To uninstall a package:

    $ pip uninstall <package>

### Virtualenv

[Virtualenv](http://www.virtualenv.org/) is a tool that creates an isolated Python environment for each of your projects. For a particular project, instead of installing required packages globally, it is best to install them in an isolated folder in the project (say a folder named `venv`), that will be managed by virtualenv.

The advantage is that different projects might require different versions of packages, and it would be hard to manage that if you install packages globally. It also allows you to keep your global `/usr/local/lib/python2.7/site-packages` folder clean.

#### Install

To install virtualenv, simply run:

    $ pip install virtualenv

#### Usage

Let's say you have a project in a directory called `myproject`. To set up virtualenv for that project:

    $ cd myproject/
    $ virtualenv venv --distribute

If you want your virtualenv to also inherit globally installed packages (like IPython or Numpy mentioned above), use:

    $ virtualenv venv --distribute --system-site-packages

These commands create a `venv` subdirectory in your project where everything is installed. You need to **activate** it first though (in every terminal where you are working on your project):

    $ source venv/bin/activate

You should see a `(venv)` appear at the beginning of your terminal prompt indicating that you are working inside the virtualenv. Now when you install something:

    $ pip install <package>

It will get installed in the `venv` folder, and not conflict with other projects.

**Important**: Remember to add `venv` to your project's `.gitignore` file so you don't include all of that in your source code!

### Ruby and RVM

Like Python, [Ruby](http://www.ruby-lang.org/) is already installed on Unix systems. But we don't want to mess around with that installation. More importantly, we want to be able to use the latest version of Ruby.

#### Install

When installing Ruby, best practice is to use [RVM](https://rvm.io/) (Ruby Version Manager) which allows you to manage multiple versions of Ruby on the same machine. Installing RVM, as well as the latest version of Ruby, is very easy. Just run:

    $ curl -L https://get.rvm.io | bash -s stable --ruby

When it is done, both RVM and a fresh version of Ruby 2.0 are installed. The following line was also automatically added to your `.bash_profile`:

```bash
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

I prefer to move that line to the `.extra` file, keeping my `.bash_profile` clean. I suggest you do the same.

After that, start a new terminal and run:

    $ type rvm | head -1

You should get the output `rvm is a function`.

#### Usage

The following command will show you which versions of Ruby you have installed:

    $ rvm list

The one that was just installed, Ruby 2.0, should be set as default. When managing multiple versions, you switch between them with:

    $ rvm use system # Switch back to system install (1.8)
    $ rvm use 2.0.0 --default # Switch to 2.0.0 and sets it as default

Run the following to make sure the version you want is being used (in our case, the just-installed Ruby 1.9.3):

    $ which ruby
    $ ruby --version

You can install another version with:

    $ rvm install 1.9.3

To update RVM itself, use:

    $ rvm get stable

[RubyGems](http://rubygems.org/), the Ruby package manager, was also installed:

    $ which gem

Update to its latest version with:

    $ gem update --system

To install a "gem" (Ruby package), run:

    $ gem install <gemname>

To install without generating the documentation for each gem (faster):

    $ gem install <gemname> --no-document

To see what gems you have installed:

    $ gem list

To check if any installed gems are outdated:

    $ gem outdated

To update all gems or a particular gem:

    $ gem update [<gemname>]

RubyGems keeps old versions of gems, so feel free to do come cleaning after updating:

    $ gem cleanup

I mainly use Ruby for the CSS pre-processor [Compass](http://compass-style.org/), which is built on top of [Sass](http://sass-lang.com/):

    $ gem install compass --no-document

## Section 3: Python Data Analysis

### Anaconda

Anaconda is a free distribution of the Python programming language for large-scale data processing, predictive analytics, and scientific computing that aims to simplify package management and deployment.

Follow instructions to install [Anaconda](http://docs.continuum.io/anaconda/install.html) or the more lightweight [miniconda](http://conda.pydata.org/miniconda.html).

### IPython Notebook

[IPython](http://ipython.org/) is an awesome project which provides a much better Python shell than the one you get from running `$ python` in the command-line. It has many cool functions (running Unix commands from the Python shell, easy copy & paste, creating Matplotlib charts in-line, etc.) and I'll let you refer to the [documentation](http://ipython.org/ipython-doc/stable/index.html) to discover them.

IPython Notebook is a web-based interactive computational environment where you can combine code execution, text, mathematics, plots and rich media into a single document.

#### Install

    $ pip install "ipython[notebook]"

If you run into an issue about pyzmq, refer to the following [Stack Overflow post](http://stackoverflow.com/questions/24995438/pyzmq-missing-when-running-ipython-notebook) and run:

    $ pip uninstall ipython
    $ pip install "ipython[all]"

#### Usage

    $ ipython notebook

For an example of IPython Notebooks used in Data Science, see this [repo](https://github.com/donnemartin/data-science-ipython-notebooks)

### NumPy

NumPy adds Python support for large, multi-dimensional arrays and matrices, along with a large library of high-level mathematical functions to operate on these arrays.

#### Install and Usage

    $ pip install numpy

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#numpy).

### Pandas

Pandas is a software library written for data manipulation and analysis in Python. Offers data structures and operations for manipulating numerical tables and time series.

#### Install and Usage

    $ pip install pandas

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#pandas).

### Matplotlib

Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms.

#### Install and Usage

    $ pip install matplotlib

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#matplotlib).

### Scikit-learn

Scikit-learn adds Python support for large, multi-dimensional arrays and matrices, along with a large library of high-level mathematical functions to operate on these arrays.

#### Install and Usage

    $ pip install scikit-learn

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#scikit-learn).

### Seaborn

Seaborn is a Python visualization library based on matplotlib. It provides a high-level interface for drawing attractive statistical graphics.

#### Installation

The section [Step 5: Run the pydata.sh Scripts](https://github.com/donnemartin/dev-setup#step-5-run-the-pydata-script) installs matplotlib.  If you prefer to install it separately, run:

    $ pip install seaborn

#### Usage

Refer to the following [matplotlib with Seaborn IPython Notebooks](https://github.com/donnemartin/data-science-ipython-notebooks#matplotlib).

### SciPy

SciPy is a collection of mathematical algorithms and convenience functions built on the Numpy extension of Python. It adds significant power to the interactive Python session by providing the user with high-level commands and classes for manipulating and visualizing data.

#### Install and Usage

    $ pip install scipy

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#statistical-inference-scipy).

### Flask

Flask is a micro web application framework written in Python.

#### Installation

The section [Step 5: Run the pydata.sh Script](https://github.com/donnemartin/dev-setup#step-5-run-the-pydata-script) installs SciPy.  If you prefer to install it separately, run:

    $ pip install Flask

#### Usage

Refer to the following [Flask IPython Notebooks](https://github.com/donnemartin/data-science-ipython-notebooks#).

### Bokeh

Bokeh is a Python interactive visualization library that targets modern web browsers for presentation. Its goal is to provide elegant, concise construction of novel graphics in the style of D3.js, but also deliver this capability with high-performance interactivity over very large or streaming datasets. Bokeh can help anyone who would like to quickly and easily create interactive plots, dashboards, and data applications.

#### Install and Usage

Coming Soon

## Section 4: Amazon Web Services (AWS)

### Spark

Spark is an in-memory cluster computing framework, up to 100 times faster for certain applications and is well suited for machine learning algorithms.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#spark).

### MapReduce

Mrjob supports MapReduce jobs in Python, running them locally or on Hadoop clusters.

#### Install and Usage

    $ pip install mrjob

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#mapreduce-python).

### Boto

Boto is the official AWS SDK for Python.

#### Install and Usage

Coming Soon

### S3cmd

S3cmd interacts with S3 through the command line.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#).

### S3DistCp

S3DistCp combines smaller files and aggregates them together by taking in a pattern and target file. S3DistCp can also be used to transfer large volumes of data from S3 to your Hadoop cluster.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#).

### S3-parallel-put

S3-parallel-put uploads multiple files to S3 in parallel.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#aws).

### Redshift

Redshift is a fast data warehouse built on top of technology from massive parallel processing (MPP).

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#aws).

### Kinesis

Kinesis streams data in real time with the ability to process thousands of data streams per second.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#aws).

### Lambda

Lambda runs code in response to events, automatically managing compute resources.

#### Install and Usage

See the following [notebook](https://github.com/donnemartin/data-science-ipython-notebooks#aws).

### AWS Machine Learning

Amazon Machine Learning is a service that makes it easy for developers of all skill levels to use machine learning technology. Amazon Machine Learning provides visualization tools and wizards that guide you through the process of creating machine learning (ML) models without having to learn complex ML algorithms and technology. Once your models are ready, Amazon Machine Learning makes it easy to obtain predictions for your application using simple APIs, without having to implement custom prediction generation code, or manage any infrastructure.

#### Install and Usage

Coming Soon

## Section 5: Databases

### MySQL

#### Install

We will install [MySQL](http://www.mysql.com/) using Homebrew, which will also install some header files needed for MySQL bindings in different programming languages (MySQL-Python for one).

To install, run:

    $ brew update # Always good to do
    $ brew install mysql

As you can see in the ouput from Homebrew, before we can use MySQL we first need to set it up with:

    $ unset TMPDIR
    $ mkdir /usr/local/var
    $ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

#### Usage

To start the MySQL server, use the `mysql.server` tool:

    $ mysql.server start

To stop it when you are done, run:

    $ mysql.server stop

You can see the different commands available for `mysql.server` with:

    $ mysql.server --help

To connect with the command-line client, run:

    $ mysql -uroot

(Use `exit` to quit the MySQL shell.)

**Note**: By default, the MySQL user `root` has no password. It doesn't really matter for a local development database. If you wish to change it though, you can use `$ mysqladmin -u root password 'new-password'`.

#### MySQL Workbench

In terms of a GUI client for MySQL, I'm used to the official and free [MySQL Workbench](http://www.mysql.com/products/workbench/). But feel free to use whichever you prefer.

You can find the MySQL Workbench download [here](http://www.mysql.com/downloads/workbench/). (**Note**: It will ask you to sign in, you don't need to, just click on "No thanks, just start my download!" at the bottom.)

## Section 6: Misc

### Contributions

Bug reports and suggestions are [welcome](https://github.com/donnemartin/dev-setup/issues)!

### Credits

This repo builds on the awesome work from the following repos:

* [dotfiles](https://github.com/mathiasbynens/dotfiles) by Mathias Bynens
* [mac-dev-setup](https://github.com/nicolashery/mac-dev-setup) by Nicolas Hery

### License

This repository contains a variety of content; some developed by Donne Martin, and some from third-parties.  The third-party content is distributed under the license provided by those parties.

The content developed by Donne Martin is distributed under the following license:

    Copyright 2015 Donne Martin

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
