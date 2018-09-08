commandctl - Linux command alias and function manager
  
  
  Command alias and custom function are an overlooked component of the system management in the linux ecosystem. Lack of clear and managed location for these components creates much redundancy and cognitive load when you need to find a very specific command configuration or when youre deciding how and where to save a function that is only mean tot be a test. Often those test scripts become long term tenants in places you wouldn't normally have scripting files at all.
  
  Commandctl is a basic utility to create, manage, and configure command aliases and functions and attempts to create a standard template for most common operations or the more complex tasks.
  
  ## Usage 
  
  
  commandctl command [option]
  
  ## Commands
  
    - help                  - print help message
    - list                  - list all commands
    - search                - search commands by keyword
    - modules               - list command modules and status
    - show                  - show a module commands
    - edit                  - edit a module commands
    - new                   - create a new command module
    - goto                  - change directory to the command repository
    - reload                - reload the command files after a change is made
  
  
  Commandctl also has its own aliases for the most commonly used routines. This is a test feature and will be able to disable in the config
  
  
      comctl              = commandctl
      comctl.edit         = commandctl edit
      comctl.goto         = commandctl goto
      comctl.help         = commandctl help
      comctl.info         = commandctl info
      comctl.list         = commandctl list
      comctl.reload       = commandctl reload
      comctl.search       = commandctl search
      comctl.show         = commandctl show
  
  
  This in essence give you a single letter autocomplete for each of the commands, and is a common theme through many of the provided alias modules.
  
  
  ## Command Module 
  
  The common routine for managing user commands is to keep all aliases in either the uesr .bashrc file or more advanced users create a seperate .bash_aliases file to keep their aliases groupe together. User funtions and scripts and typically scattere over several different '/bin' and '/scripts' folders, and the user environment variables are handled through sorcery and prayers during program execution.
  
  
  
  
  
  ### Example Modules 
  
  __Example: Groupped by management task__
  
  For Gitlab Selfhosted Server 
  
  ```sh
  alias gl="gitlab-ctl"
  alias gl.show="gitlab-ctl show-config"
  alias gl.edit="nano /etc/gitlab/gitlab.rb"
  alias gl.status="gitlab-ctl status"
  alias gl.check="gitlab-ctl check-config"
  alias gl.backup="cp /etc/gitlab/gitlab.rb ~/Gitlab_Ops/gitlab.rb.bak"
  alias gl.update="gitlab-ctl reconfigure"
  alias gl.show.ports="grep -v \\# /etc/gitlab/gitlab.rb | grep port"
  alias gl.show.urls="grep -v \\# /etc/gitlab/gitlab.rb | grep http"
  alias gl.show.paths="grep -v \\# /etc/gitlab/gitlab.rb | grep /"
  ```
  
  Setting up and reconfiguring Gitlab can be very tedious. Especially if you do not use any sort of alias or scripts. There is a usually trial and error and edit-update-rerun-redit-reupdate workflow, however you can remove most typing if you create just a few aliases.
  
  
  
  
  __Example: Highly customizable with lots of option 
  
  Creating some standard commands for journalctl 
  
  
  
  
  
  
  
  
  
  ## Experimental Features (WIP) 
  
  - Tagging modules with to allow group control (on/off) or for exporting to a different system (approoved command groups to  be forwareded to a remote session / or configuring a new machine)
  
  - Save commands (from history or interactive) to new alias
  - Filter and parse history for commands that should be aliases.
'''
tags: []
isStarred: false
isTrashed: false
