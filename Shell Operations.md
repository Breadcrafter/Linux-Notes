<!-----



Conversion time: 9.315 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs™ to Markdown version 2.0β2
* Thu Apr 16 2026 05:02:40 GMT-0700 (Pacific Daylight Time)
* Source doc: Shell Operations / things 
----->


**<span style="text-decoration:underline;">Shell Operations</span>**

**Shell -** A program that acts as an intermediary between a user and the Linux Kernel, allowing you to do everything from checking files and running programs to customizing how the system works. 

Bash variables typically have a $ sign in front when you want to **access, print, or use** the data stored inside that variable.  (A fetch command)

**<span style="text-decoration:underline;">User and Session Environmental Variables</span>** : Define personal details, like username, or session-specific settings, like preferred command prompt style. 

* When software / applications need to use 



* **Variable :** A name that holds a piece of information (Its like a box with a label on it telling you what’s inside)
* **Environment Variables :** Special variables used by the Linux system, shell, or applications to store system-wide and process-specific information 
* **USER Variable :** Acts as a personal name tag that software and scripts use to identify the current operator of the machine 

    Example: echo $USER (if we were logged into fred user it would say following)


    	               Fred 

* **HOME Variable :** Points to a user’s home directory, where personal files, documents, and settings are stored. 

    * This file path ie where the home variable points to, can be used by porgrams to identify where they can save or retrieve my user-specific data


    Example: echo $HOME


                  /home/Fred 

* **SHELL Variable :** Indicates the command-line interpreter running the current terminal session. The SHELL variable is the “language interpreter” for everything said to the computer. 

    echo $SHELL


    /bin/bash


    Examples:            


    	Bash


    	Zshell 

* **PS1 (Prompt String 1) Variable :** Defines the style and content of the command prompt. This variable uses escape characters, which can be useful when managing linux servers. 

    Escape Characters need to know

* **\u** : **U**sername (u for user)
* **\h** : **H**ostname (h for host)
* **\w** : **W**orking Directory (full path)
* **\W** : **W**orking Directory / current folder (Trailing/Short name only)
* **\$** : Privilege Indicator (Shows **#** for Root, $ for all other users)

Difference between \w and \W

**Lowercase <code>\w</code> (Wide):** Shows the whole path: `[tony@linux-srv/var/www/html]$`

**Uppercase <code>\W</code> (Window):** Shows just the current folder: `[tony@linux-srv html]$`

**Examples:** 

PS1=”\u@\h:\w\$ “

Result: tony@linux-lab: /var/log$ 

**Editing the PS1 variables**

**(User Level)**

**File Path:** ~/.bashrc

**Use Case:** If you want to change the root user prompt style to easily know when you are in root user, but don’t want to change any other user prompt style. 

**(Global Level)**

**File Path:** <span style="text-decoration:underline;">/etc/bash.bashrc</span> (Debian/Ubuntu) or <span style="text-decoration:underline;">/etc/bashrc</span> (RHEL/CentOS)

**Use Case:** Setting a “company standard” prompt or adding a “Production Server” warning to the prompt for all staff

Other Prompt variables (less about needing to know and more of just having an idea of them) 

**PS2 :** The “continuation” prompt (If you type a command and hit Enter without finishing it (like an open quote), you see a >. Thats PS2.

**PS3 :** Used as the prompt for the select loop in scripts

**PS4 :** Used when debugging scripts (bash -x) to show execution traces



* **PATH Variable :** A colon-separated list of directories where the shell looks for executable files. Its like a list of directories that when you run a command or something like ls the shell will look through the path variable directories from left to right until it finds the file or ls file and then it runs it. 

    Example: echo $PATH 


                    /usr/local/bin:/usr/bin:/bin:/usr/games 


    So when you type the ls command it goes through that list from left to right

1. Is ls in /usr/local/bin (no)
2. Is ls in /usr/bin (no)
3. Is ls in /bin (yes, and runs the file / command) 
* **Modifying PATH Variable :** These are how you would add a new folder like /opt/myapps to the path so you can run custom tools easily 
* **Temporary Way (Current Session) :** literally type in the command 

        export PATH=$PATH:/opt/myapps	  (this will add the /op/myapps directory to the end of the list)


        export PATH=/opt/myapps:$PATH  (this will add directory to beginning of list)

* **Permanent Way :** To make the change stick, you have to write that equation / command into the file the shell reads every time it start up. (~/.bashrc) &lt;------ The file you modify 

 	**vim ~/.bashrc**

	(At the very bottom of the file type following) 

	**export PATH=$PATH:/home/fred/bin** 

	(adds the folder called bin in Fred’s home directory to the end of the list)

	**Save and Exit**

	**source ~/.bashrc**

	(This command applies the changes you made to the PATH variable / bashrc file) 


		Example I might use this for:

		If you wanted to have an easy way to start firefox from the terminal, you could add the directory that firefox was located in and add it to the PATH variable. Then type firefox and it would run / open firefox. 

Some new Commands: 

which (shows exactly which folder a command is being pulled from)

**which ls**

**# Result: /bin/ls**

**Commands to view these variables** 

**<code>printenv</code>**: Displays environment variables (the exported ones).

**<code>env</code>**: Similar to `printenv`, often used to run a program in a modified environment.

**<code>set</code>**: Displays **all** variables (including local ones and shell functions). This list is much longer than `printenv`.

**source ~/.bashrc** and **. ~/.bashrc** both commands do the same thing that is applying the current bashrc modified configuration to the shell permanently. And both commands work in RHEL and debian based distros. 
