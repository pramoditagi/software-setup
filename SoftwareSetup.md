# Only for MAC

### Command Line Tools.
Before installing anything, some command line tools need to be installed on your Mac. These can be done by running the following command from the Terminal window and following the instructions that come up.

    xcode-select --install


### Homebrew
Installing packages and software for OS X can be a challenge because OS X lacks a package manager similar to Linux systems. `Homebrew` solves this problem. Installation is as simple as the following command from a Terminal window.

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

After installing Homebrew, there are some additional commands to run. They essentially provide more repos from which to install packages. Run the following commands from your terminal.

    brew tap homebrew/core

### Homebrew Casks
After installing `Homebrew/Core`, there is another tool built off of Homebrew called `Homebrew Cask` that simplifies installing Mac applications or large binaries. It removes the need to go to various websites, download the Mac app and then open the app for installation. All of that can be done with a single command from a Terminal. It is quite powerful and quick. The rest of this guide will use Cask to install the applications we use on a daily basis. You need to run the following commands to setup Cask.

    brew tap homebrew/cask
    brew tap homebrew/cask-versions

### Git
Git is used for version control of our code and other things related to development. Run the following commands to install git.

    sudo chown -R $(whoami) /usr/local/share/zsh /usr/local/share/zsh/site-functions
    chmod u+w /usr/local/share/zsh /usr/local/share/zsh/site-functions
    brew install git

After git is installed, you need to configure it as follows. Replace the dummy data with your information.

    git config --global user.name 'John Doe'
    git config --global user.email 'john.doe@cerner.com'
    git config --global github.user jd038983 #Run this only if user id is present or given

If you want to use SSH for communicating with Github, then follow the instructions below. Otherwise, you can use the Git Configuration from Web Developer - Getting Started to setup HTTPS communication with Github. It's a personal preference and either one will work.

### Tab Completion
When working in a Terminal it is extremely helpful to use Tab to complete commands. Install the following utility to get complete all bash commands including Git branches (which comes in handy).

    brew install bash-completion

After that completes, there will be instructions on adding a snippet of code to your ~/.bash_profile so that it works in a shell. The snippet was the following at the time of writing this guide. Defer to the commands instructions.

Add the following line to your ~/.bash_profile:

    [[ -r "/usr/local/etc/profile.d/bash_completion.sh" ]] && . "/usr/local/etc/profile.d/bash_completion.sh"


SSH Configuration
If you want to use SSH, I recommend that you follow the SSH key generation section from Github. It is basically just running ssh-keygen. I usually DO NOT enter a passphrase when prompted because it kind of defeats the purpose of avoiding to enter your password each time you want to make git requests. If that makes you uncomfortable, then I suggest you just go with the HTTPS configuration mentioned above.

After the SSH key is setup, you need to add the SSH key to your Github account. If you do not have a Github account, then follow the instructions on the Github section of Web Developer - Getting Started. You can continue on with other portions of this setup guide until that account is created and come back to this when you account is ready. Once you have a Github account, you can add the SSH key by following Adding a new SSH key instructions on Github. The only thing different is that you go to https://github.cerner.com/settings/ssh instead of github.com to add your key.


### Java
Our REST services are coded with Java, OS X does not include a JDK for development so you need to install Java with the following command. We are currently on java 8, but "brew cask install java" currently pulls down java 9, so specify java8 in the command (as shown below). Note - We have edited this command on 9-Aug-2021 to ensure we are using adoptopenjdk8 instead of java8

    brew install --cask adoptopenjdk8
After installing Java, add the following to your ~/.bash_profile file so that a Java item doesn't appear in your dock each time a Java command line program is run.

    export JAVA_TOOL_OPTIONS='-Djava.awt.headless=true'

### Ruby
Our application is written using Ruby on Rails. We use RVM (Ruby Version Manager) to install and manage multiple versions of Ruby on our machines. Defer to the RVM installation instructions at https://rvm.io/rvm/install for details or if there is an issue. The command should be

    curl -sSL https://get.rvm.io | bash -s stable --ruby
After that is run, add the following to ~/.bash_profile so that RVM is sourced in each shell.

    source ~/.rvm/scripts/rvm
After installing RVM, open a new terminal window and run the following command to install the version of Ruby that we currently use. Always check https://github.cerner.com/GenomicSolutions/lims-app/blob/master/.ruby-version for the specific version of Ruby that we are using and replace that version with the one below in the command.

    rvm install ruby-2.7

### NodeJS
In our application, we rely on extensive Javascript so we have a need to install NodeJS on our development machines. We use NVM to manage multiple versions of NodeJS similar to RVM for Ruby. Install NVM by following the command on https://github.com/creationix/nvm. The command was this at the time the guide was written so defer to the NVM documentation.

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
After installing NVM, open a new terminal window and run the following command to install the version of NodeJS that we currently use. Always check https://github.cerner.com/GenomicSolutions/lims-app/blob/master/.nvmrc for the exact version of NodeJS we are using as it will probably be different than what is below.

    nvm install 10.22.0
If you have restarted your terminal and it still thinks nvm is not a command then you may need to add the line below to your ~/.bash_profile. The curl install command above automatically adds export lines to .bashrc which may not be used automatically for your terminal on a mac.

    source ~/.bashrc

You can refer to Javascript Ecosystem for details about how Javascript is used at Cerner and other tools available.

### Maven
We use Maven to manage Java projects.

    brew install maven

### MySQL
We use MySQL for our database backing our services.

    brew install mysql@5.7
After installed, run the following command to make sure MySQL runs at startup

    brew tap homebrew/services
    brew services start mysql@5.7
If you get,

**_-bash: mysql: command not found error while starting MySQL_**, then run the following command to link the MySQL to 5.6 version

    brew link mysql@5.7 --force

## Applications
### Browsers

    brew install google-chrome
    brew install firefox


### IDEs
Development is best done using an IDE. We use IntelliJ for Java and RubyMine for Rails development.

    brew install intellij-idea-ce
    brew install rubymine

### Docker
    brew install --cask docker
    docker run -d -p 80:80 docker/getting-started

### SourceTree
A visual Git app for managing your Git repos.

    brew install sourcetree

##Team Communication
We use Microsoft Teams for communication between the team since it has advantages over Lync. Request an invitation to the team after installing the Microsoft Teams Mac app.

    brew install microsoft-teams

### MySQL Workbench
Since we deal with a database frequently, it is helpful to have a UI to interact with the DB and issue queries.

    brew install mysqlworkbench
if unable to launch mysql workbench, try installing any previous version using https://downloads.mysql.com/archives/workbench/

### Postman
We use postman for accessing the service or api endpoints.

    brew install postman
After installing postman application, open postman application, go to settings→General and Turn off SSL certificate verification. Otherwise you will receive Unauthorized error while accessing end points.

### Install Yarn
Do this only if necessary, first check if Yarn is installed or not by running yarn -v command. If you dont get versions for example 1.22.11 etc then run the following command to install yarn from homebrew -

    brew install yarn


### Miscellaneous
The following tools are all optional and you will find some of them listed on https://connect.ucern.com/thread/725668. Below are a couple that can easily be installed via Cask.

    brew install alfred
    brew install flycut
