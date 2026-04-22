<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see useful information and inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 2

Conversion time: 14.758 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs™ to Markdown version 2.0β2
* Wed Apr 22 2026 14:01:11 GMT-0700 (Pacific Daylight Time)
* Source doc: Shell Operations / things 
----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 2.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>

**<span style="text-decoration:underline;">Shell Operations</span>**

**Shell -** A program that acts as an intermediary between a user and the Linux Kernel, allowing you to do everything from checking files and running programs to customizing how the system works. 

Bash variables typically have a $ sign in front when you want to **access, print, or use** the data stored inside that variable.  (A fetch command)

**<span style="text-decoration:underline;">User and Session Environmental Variables</span>** : Define personal details, like username, or session-specific settings, like preferred command prompt style. 

* When software / applications need to use 



* **Variable :** A name that holds a piece of information (Its like a box with a label on it telling you what’s inside)
* **Environment Variables :** Special variables used by the Linux system, shell, or applications to store system-wide and process-specific information 
* **DISPLAY Variable :** Determines where to send graphical output in a Linux system running the X Window system
    * If the PATH variable is the directories for finding programs DISPLAY tells those programs where to show up

	Example: echo $DISPLAY

		     :0.0



* If you are X-Forwarding with SSH the system will show localhost:10.0 when you do echo $DISPLAY

	:0 = Display Number : Usually refers to the entire collection of monitors connected to one keyboard / mouse set

	.0 = Screen Number : This is the logical screen, which is almost always .0 because linux treats multiple monitors as 1 logical display to be able to grag windows between them. 

**When you would see :1.0** (Local Multi-Session)


* If you login and you still have a user logged in 
* Manual Second X Server : if you manually start a second, completely independent window manager, it will take the next available slot which is :1 
* **USER Variable :** Acts as a personal name tag that software and scripts use to identify the current operator of the machine 

    Example: echo $USER (if we were logged into fred user it would say following)


    	               Fred 

* **HOME Variable : **Points to a user’s home directory, where personal files, documents, and settings are stored. 

    * This file path ie where the home variable points to, can be used by programs to identify where they can save or retrieve my user-specific data


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

\w = wide (whole path) like a kid, they are short but wide and fat 

\W = working directory, the working man MAN which are big so big W 

**The “Expansion Rule” :** 

* Double Quotes(“  “)**Are Weak**: Allow variables to “Expand” (change) 
    * When you open the terminal, the shell reads .bashrc, and sets the variable once, and that variable is set to that for the entire session. 
    * So for \t in double quotes, it would be stuck and wouldn't change after each time you hit enter.
    * Its like a snapshot of the variable at the start of the session
* Single Quotes (‘   ‘)**Are Strong**: Keep everything literal
    * It updates every time you start up the shell / every time you hit enter it updates for the new prompt
    * So for \t every time you hit enter to a new prompt it will look at the variable and update it. This makes it look like your clock is ticking.    

**Editing the PS1 variables**

**(User Level)**

**File Path:** ~/.bashrc

**Use Case:** If you want to change the root user prompt style to easily know when you are in root user, but don’t want to change any other user prompt style. 

**(Global Level)**

**File Path:** <span style="text-decoration:underline;">/etc/bash.bashrc</span> (Debian/Ubuntu) or <span style="text-decoration:underline;">/etc/bashrc</span> (RHEL/CentOS)

**Use Case:** Setting a “company standard” prompt or adding a “Production Server” warning to the prompt for all staff

Other Prompt variables (less about needing to know and more of just having an idea of them) 

**PS2 :** The “continuation” prompt (If you type a command and hit Enter without finishing it (like an open quote), you see a >. Thats PS2.

**PS3 : **Used as the prompt for the select loop in scripts

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

  **source ~/.bashrc **

  (This command applies the changes you made to the PATH variable / bashrc file) 

Example I might use this for:

   If you wanted to have an easy way to start firefox from the terminal, you could add the directory that firefox was located in and add it to the PATH variable. Then type firefox and it would run / open firefox. 

**Delete a Variable**

**unset** - This command removes the variable from the current shell’s memory entirely 

**unset PS1** (this will remove the PS1 variable from the current session (or memory)

* The unset command will delete the entire variable, you CAN’T just remove part of the variable, and there is NO way to do that.

* This command only affects the current shell session, to completely remove the variable you have to manually delete it from the ~/.bashrc file

Some new Commands: 

which (shows exactly which folder a command is being pulled from)

**which ls**

**# Result: /bin/ls**

**Commands to view these variables** 

**(&) Backgrounding : ** Put & at the end of a command to tell the shell to run the process in the background (You can keep your terminal open while the program stays open)

**<code>printenv</code>**: Displays environment variables (the exported ones).

**<code>env</code>**: Similar to `printenv', often used to run a program in a modified environment.

**<code>set</code>**: Displays **all** variables (including local ones and shell functions). This list is much longer than `printenv'.

**source ~/.bashrc** and **. ~/.bashrc** both commands do the same thing that is applying the current bashrc modified configuration to the shell permanently. And both commands work in RHEL and debian based distros. 

**~/.bashrc vs ~/.bashrc_profile**

* **~/.bashrc:** The file that is executed for non-login shells (Local login shells)
* **~/.bashrc_profile:** The file that is executed for login shells (Remote login shells / SSH login)
* It is like this for **security** ensuring all security scripts are run, **Environment Isolation** teats ssh login as a fresh login to ensure it only loads what is necessary for a remote connection.
* SSH ignores ~/.bashrc unless the ~/.bashrc_profile tells it to look there

**Where (Global vs. Local Scope)**

* **Local (~/.bashrc):** Only affects **you**, if Fred changes this, Tony won’t see any difference
* **Global (/etc/…):** Affects **everyone**, if you want a company standart you must go to the /etc/ folder because that's the “”Master Control room”” for the whole system

**When (Login vs Non-Login)**

* **Login Shell:** You just arrived at the server (SSH or logging into the physical screen)
* **Non-Login Shell:** You are already at your desktop and you just opened a new terminal window

**The “Debian Trick”:** Debian/Ubuntu sets up a “bridge” When you login, the system reads /etc/profile, but then /etc/profile immediately turns around and turns around and says, “Wait, I should also read /etc/bash.bashrc while i'm at it.

* **Red Hat / RHEL:** calls the file /etc/bashrc
* **Debian / Ubuntu:** calls the file /etc/bash.bashrc

If you wanted a company standard on RHEL you edit /etc/bashrc and Debian /etc/bash/bashrc

**<span style="text-decoration:underline;">Paths</span>**

Paths - Directions to specific locations, folders, or files

Using the correct path tells Linux precisely which file or folder to work with

**Absolute Paths :** The complete address for the destination allows access regardless of the current location 

* An absolute path is like saying, take me to <span style="text-decoration:underline;">123 Main Street</span>, it doesn’t matter where you currently are because you have the complete address for the destination, so you can get there no matter where you
    * **Root Directory (/):** Represents the root directory, which is the highest level of the filesystem. **All absolute paths start with this /** 
    * You are telling the Linux System to start at the root directory
    * **Home Directory (~):** The tilde (~) represents the current user’s home directory, providing an easy shortcut to a user-specific personal directory.
    * Also common to use the tilde when referring to folders or files inside your home directory such as ~/Documents

The main difference between the tilde (~) and the root directory (/) is that the forward slash is the route directory for every user, and the tilde is user specific 

Example: 
**/usr/local/bin**

System Starts at **root directory**, then goes to **usr directory**, then to **local directory**, finally to **bin directory**.

**Relative Paths :** A path that does **NOT** start with ~ or /. It is a set of directions based solely on your current location / pwd 


* **Directional Rules**
    * **(..) :** Means the **Parent Directory**, allows you to move up 1 directory / going closer to the root directory 
    * **(.) :** Means the **Current Directory**, allows you to tell the shell to look inside the current directory you are in for the file.
    * **(-) :** A shortcut used with the cd command to return to the **Previous working directory** (this is stored in the **$OLDPWD** variable) Used as a redo / works on History, or looks at the last place you were and puts you there. 

cd.. Is based on the **folder structure**; cd - is based on **your previous action** 


        Examples: 
        **./myscript.sh** 

		This tells the shell to look for a file named myscript.sh and run it. The . tells the shell that the file is in the current directory so it doesn’t need to look for it. 

		Or say you are in your home folder, you can do the following.

		**./scripts/myscript.sh** 

		This tells the shell to go into the directory named scripts and find the file named myscripts.sh and run it. 

**Tilde Expansion Phase**

In the Linux execution cycle the shell performs the Tilde Expansion phase as one of its first steps. 



* When the shell sees ~ it immediately replaces it with the string found in the $HOME environment variable 

    ~   =   /home/Fred   (~ is converted to /home/localuser 

* Since the final result of this expansion always starts with a /, the system treats this path as an absolute path 

**Diagnostic Commands**



* **pwd (Print Working Directory) :** Shows the absolute path of your current directory
* **realpath : ** Command that converts a relative path into an absolute path. 
    * You are in ~ and type,

        **realpath scripts/test.sh **


        Output: /home/fred/scripts/test.sh 

* **basename : **Returns only the filename from a path
    * **basename /etc/passwd**
    * Output: **passwd **
* **dirname : **Returns only the directory part of the path 
    * **dirname /etc/passwd**
    * Output: **/etc**

**The Hidden Execution Rule** 

Even if you are in the same folder as a file, the shell will not find it by name alone because the current directory is not in the $PATH variable.



* **myscript.sh** = The shell looks in the systems official folders ie $PATH variable (command fails)
* **./myscript.sh **= ignores the system folders / $PATH; instead looks only in the directory that you are currently in. (command runs / works) 

**Security Reason:** **./** prevents **Path Hijacking**, it ensures that a malicious file named ls in your current folder doesn’t accidentally run when you intended to run the real system /bin/ls 
