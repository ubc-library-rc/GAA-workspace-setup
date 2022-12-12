---
layout: default
title: Workflow
nav_order: 1
---

This is a simple workflow designed to help new GAAs set up a developer environment on their personal computer. If your role requires you to build or edit workshops using git and GitHub, this guide may help you.     
_Note: as of now, the guide is for mac users, though in time it may be adjusted for windows operating systems as well._

# GAA Workspace Setup

## set shell to bash 
if your shell is zsh run ```chsh -s /bin/bash``` then quit and re-open terminal
    
for further introduction on unix shell, see [this workshop](https://ubc-library-rc.github.io/intro-shell/content/01-what-is-the-shell.html) 

## install code editor
like [Visual Studio Code](https://code.visualstudio.com/download) or [Notepad++](https://notepad-plus-plus.org/) or [Sublime Text](https://www.sublimetext.com/3).
__if you want terminal command ```code``` to always open vs code, then in vs code go to settings --> command palette --> Shell Command: install 'code' command in PATH__

## install Xcode Command Line Tools
```bash
$ xcode-select --install
```
for more info see the [installation guide](https://mac.install.guide/commandlinetools/4.html)

## install homebrew
```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## install git
```bash
$ brew install git
```
for a brief overview of git commands see this [workshop page](https://ubc-library-rc.github.io/intro-git/content/01_what_is_git.html)

## create GitHub account 
if you don't have a github account already, [create one here](https://github.com/signup?source=login)
once you have an account, ask your research cluster team head to add you as member of [github.com/ubc-library-rc](https://github.com/ubc-library-rc) as well as any cluster-based page. 

## configure git 
enter the following commands in the terminal, replacing the text within quotations with your name, the email associated to your GitHub account, and the code editor of your choice 

``` bash
$ git config --global user.name "Your Name"
$ git config --global user.email "your@email"
$ git config --global core.editor "vscode"
```

## generate ssh key 
read about [SSH here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh)    
    
tldr: SSH ensures git workflows between your personal device and the library's Github page are secured through a passphrase-based key. You will generate a public/private key pair. The private key is saved to your personal device and the "public" key is saved to your Github account. You only need to set this up once per account and device. No one else will see or use your passphrase. 
    
1. in the terminal, enter the following command substituting the email associated with your GitHub account
```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```    

it will return 
```bash
> Generating public/private ed25519 key pair
```

when it asks you to  ```Enter file in which to save the key (/Users/"your-home-directory"/.ssh/id_ed25519):``` copy the file path inside the parentheses and paste it as your file path. hit return.    
    
2. enter a secure passphrase twice for confirmation. your key will be generated.     
    
3. start the agent in the background by running the command ```eval "$(ssh-agent -s)"```    
    
4. run the command ```pbcopy < ~/.ssh/id_ed25519.pub```     
this copies the public key to your clipboard.    
    
5. Go to your GitHub account. Click the drop down arrow by your profile icon in the top right of the page. Go to settings. Click the **SSH and GPG keys** menu on the left panel. Click **new SSH key**. Give it a name like "lilys macbook" and control paste the public key into the dialogue box. Its format should be ssh-ed25519 SHA256: followed by a long string of numbers beginning with As and ending with a space and your email. Save your key. 
    
***Note:*** The first time you try to clone a library repository, or attempt to push, pull, commit, or otherwise make changes that would require your passphrase, you will get a warning in the command line saying ```the authenticity of github host cannot be established >  Are you sure you want to continue connecting (yes/no/[fingerprint])?``` to which you should return ```yes```   
You will see ```Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.```
This is good. Now, and from now on, you will be prompted to enter your passphrase. 

# working locally

## making and initializing a directory for your work
- open terminal and type ```pwd`` to ensure you are in your home directory (likely _/Users/your-name_)    
- its useful to have one folder (aka directory) for all git repositories you may clone and work on from your personal computer. 
- to create one such directory, type the following command in your terminal ```mkdir GitHub```   
- its important to have a capital letter, so as not to become confused later on with any github files
- ```cd GitHub``` to enter that folder
- ```git init``` to initialize the folder. (again, learn more about [git basics here](https://ubc-library-rc.github.io/intro-git/content/02_getting_started.html))

## creating a new repository 
follow [this workshop](https://ubc-library-rc.github.io/rc-workshop-template/#set-up-the-rc-workshop-site) if you're developing an entirely new workshop for the research commons. 

## cloning an existing repository 
editing, improving, completing an existing workshop? 
1. go to the workshop's repository in GitHub and click the green ```<> Code``` button. Under the Local clone tab select SSH and copy the line of code.    

2. Navigate to the initialized GitHub work folder on your computer and run the command (all in one line) ```git clone``` followed by code you just copied. enter your passphrase when prompted.   
   
3. Now you're ready to cd into your workshop's directory, run ```code .```, and begin editing. Want to see your changes before pushing them to the remote origin? follow the steps below to spin up a local site... 

# for running sites locally 
the RC workshop template site has instructions on this next bit but here's a simpler, more up-to-date and integrated way

## install ruby 
run ```ruby -v``` 
if it returns anything below 3.1.2, you have the default ruby installed on your computer. you need to install the latest version and ensure that is the version being used. 

run ```brew install ruby``` (need to remember why not ```ruby-install ruby```)

after installation, there will be a note saying ```If you need to have ruby first in your PATH, run:```       
run that command.
run ```cat ~/.bash_profile```
run ```source ~/.bash_profile```
then run ```ruby -v```
you should have the latest version now

## install jekyll
1. to see which gems are there, run ```gem list```   
2. to install gems needed to run a local site, run the following commands line by line. after each installation you should see the message "1 gem installed"

```bash
$ gem install jekyll
$ gem install jekyll-seo-tag
$ gem install jekyll-remote-theme
```

3. ```gem env``` to see file paths   

4. run ```code ~/.bash_profile``` to open up your bash profile in the code editor. you will see two paths set from earlier. in order for jekyll to run you need to set the file paths of the RubyGems, add the following lines into the text file in the order below. save and close your bash profile.
    
```bash
eval "$(/opt/homebrew/bin/brew shellenv)"
export PATH="/usr/local/opt/ruby/bin:$PATH"
export PATH="/opt/homebrew/lib/ruby/gems/3.1.0/bin:$PATH"
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
export PATH="/opt/homebrew/Cellar/ruby/3.1.3/lib/ruby/gems/3.1.0:$PATH"
```

5. now you can run any workshop site locally by changing to the outermost directory of the repo and running the terminal command ```jekyll serve```
6. cut and paste the server address for the local host into any web-browser
