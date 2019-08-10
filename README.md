

# CommandCtl 

Linux command alias and function manager

> One command to make them, one command to find them, one command to edit them and with an alias bind them.



### Background 

Linux is full of '*ctl' utilities.  

`something-ctl` is usually a wrapper script that helps manage commands and groups of commands with that have lots of options or sub commands.  

There is `systemctl` and `sysctl` for you system, `journalctl` to see system `journal`, `networkctl` and `hostnamectl` for network tasks. 

__The only thing missing is a control command to control commands__



The `commandctl` utility helps you to organize, manage, create, and find your custom shell aliases and functions.  It provides a fixed location for your aliases, using a modular file based system to load all your commands. It also makes it easier to search, edit, and create new ones from a template.



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


__You can use commandctl to load custom commands for commandctl__

    # ./cmd.d/commandctl.cmdrc
    comctl              = commandctl
    comctl.edit         = commandctl edit
    comctl.goto         = commandctl goto
    comctl.help         = commandctl help
    comctl.info         = commandctl info
    comctl.list         = commandctl list
    comctl.reload       = commandctl reload
    comctl.search       = commandctl search
    comctl.show         = commandctl show

## Command Conventions

There are a few common conventions that have been very useful and I would recommend implementing in your own setup.  

1. Use only lower case - Save your hands the trouble of switching cases constantly. This also helps interop due to differences in shell options between user.  

2. A good command module name is one word.  The ultimate goal is quicker commands and more powerful commands by default. Avoid  compounding word-such-as-this  or  using_underscores_to_make_something_read_well. The more you thing that you add with compund words end up making your autocomplete less useful since it stops every 3 letters to check your next input.

3. Use the `.` character or `-` create sets of commands that include preset options and repeated tasks

4. Don't consume your own aliases. Including aliases in others to make more complicated commands is a bad practice and will make your setup very prone to errors. Since all of the command modules are sourced in your bashrc file when your session starts, you can do anything you would normally do in the `.bashrc` or shell script file. For complex tasks you should to write functions and use aliases to create simple easy to read commands.  You're also able to set environment variables that which can be used in your aliases to create a reusable configuration with command line flags.  

5. Make your aliases short but make sense. Create sub aliases for less used commands. If you use sub aliases you don't need to make them all the same depth. Keep the most frequently used task as a default command for that section. 

   _Do this_

   ```sh
   cmd="some-long-command --verbose"
   cmd.task="command for task related not necessarily the command itself"
   cmd.task.other="variation of previous command used in some other away"
   ```

   _Don't this_

   ````sh
   cmd="some-long-command --verbose"
   cmd.task.first="command for a related task"
   cmd.task.second="variation of the previous"	
   ````


## Command Module Files

The common routine for managing user commands is to keep all aliases in either the user `.bashrc` file or more advanced users create a separate `.bash_aliases` file to keep their aliases grouped together.  User functions and scripts and typically scattered over several different project `/bin` and  `/scripts` folders. The `<command>.cmdrc`  file is a blank template that will be loaded with you include it in the `.commandrc` file.  It is a good practice to name you the files the same thing as the command they are creating shortcuts for instead of an abstract concept. So use `git.cmdrc` ,  not  `devutils.cmdrc`.  



### Example Modules

__Example: Common tasks for every command__ 

These are generally useful command aliases that are useful in any command group

__goto__

If you find yourself often needing to jump to a config directory, then bad to a data directory very far away consider adding a `.goto` command. This was you don't need to remember the exact path for every task just the name of the command `<task>.goto.config` and `<server>.goto.data` Once you have a few of these for your modst used commands getting around becomes very fast

__search, show, list__

You can use setup preconfigured informational commands with a more complicated to quickly get information in a name spaced fashion.

__edit, create__

You can create commands to generate files from a template or edit an existing very quickly.

__logs__

linux `syslog` and `journalctl` have tons of options and parameters, log commands for specific tasks are good to add to every group, when you need to find something in the logs it can be frustrating to remember all the flags and what each option means right in the moment.  Alias them instead and have it ready when you need it.


__Example: DevOps Server Modules Group__

```sh
# gitlab.cmdrc 
alias gl="gitlab-ctl"
alias gl.show="gitlab-ctl show-config"
alias gl.status"=gitlab-ctl status"
alias gl.update="gitlab-ctl reconfigure"
alias gl.check="gitlab-ctl check-config"
alias gl.backup="cp /etc/gitlab/gitlab.rb /etc/gitlab/gitlab.rb.bak"
alias gl.edit="nano /etc/gitlab/gitlab.rb"
alias gl.get.paths="grep -v \# /etc/gitlab/gitlab.rb | grep \/"
alias gl.get.ports="grep -v \# /etc/gitlab/gitlab.rb | grep port"
alias gl.get.urls="grep -v \# /etc/gitlab/gitlab.rb | grep http"
alias gl.goto.etc="cd /etc/gitlab"
alias gl.goto.nginx="cd /var/opt/gitlab/nginx/conf"
alias gitlabctl="gitlab-ctl"

# nginx.cmdrc
alias nx="nginx"
alias nx.cat="cd /etc/nginx/sites-available && cat"
alias nx.dump="sudo nginx -T"
alias nx.goto="cd /etc/nginx"
alias nx.goto.etc="cd /etc/gitlab"
alias nx.logs.acc='cat /var/log/nginx/access.log | sort | column -t -s "\[\]\(\)\"" | grep -v 199.66.69.218 | less'
alias nx.logs.acc.cat="cat /var/log/nginx/access.log"
alias nx.logs.acc.ips="cat /var/log/nginx/access.log | column -t | cut -b -20 | sort --unique"
alias nx.logs.acc.tail="tail -f /var/log/nginx/access.log"
alias nx.reload="sudo systemctl reload nginx.service"
alias nx.restart="sudo systemctl restart nginx.service"
alias nx.status="sudo systemctl status nginx.service"
alias nx.test="sudo nginx -t"

## networking.cmdrc
alias ls.net.4.resolve="lsof -P -i4"
alias ls.net.4="lsof -P -n -i4"
alias ls.net.6="lsof -P -n -i6"
alias ls.net.localhost="lsof -P -n  -i@localhost"
alias ls.net.tcp="lsof -P -n -iTCP"
alias ls.net.udp="lsof -P -n -iUDP"
alias ls.net="lsof -P -n -i"
alias ls.ports="netstat -tunlap"
alias ls.routes="netstat -r"
alias ls.socks.long="ss --processes --numeric --udp --ipv4 --listening --tcp --unix --contexts"
alias ls.socks="ss -4ltdn"
```

For more examples see the `/cmd.d`directory 

## Experimental Features (WIP)
 - Tagging modules with to allow group control (on/off) or for exporting to a different system (approved command groups to forward to a remote session / or a new machine)  
 - Save commands (from history or interactive) to new alias  
 - Filter and parse history for commands that should be aliases  


https://github.com/charrismatic/commandctl
