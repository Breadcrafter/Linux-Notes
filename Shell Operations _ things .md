**Shell Operations**

**Shell \-** A program that acts as an intermediary between a user and the Linux Kernel, allowing you to do everything from checking files and running programs to customizing how the system works. 

Bash variables typically have a $ sign in front when you want to **access, print, or use** the data stored inside that variable.  (A fetch command)

**User and Session Environmental Variables** : Define personal details, like username, or session-specific settings, like preferred command prompt style. 

\* When software / applications need to use 

- **Variable :** A name that holds a piece of information (Its like a box with a label on it telling you what’s inside)  
- **Environment Variables :** Special variables used by the Linux system, shell, or applications to store system-wide and process-specific information 

- **DISPLAY Variable :** Determines where to send graphical output in a Linux system running the X Window system  
  - If the PATH variable is the directories for finding programs DISPLAY tells those programs where to show up

	Example: echo $DISPLAY  
		     :0.0

- If you are X-Forwarding with SSH the system will show localhost:10.0 when you do echo $DISPLAY

	:0 \= Display Number : Usually refers to the entire collection of monitors connected to one keyboard / mouse set  
	.0 \= Screen Number : This is the logical screen, which is almost always .0 because linux treats multiple monitors as 1 logical display to be able to grag windows between them.   
**When you would see :1.0** (Local Multi-Session)

- If you login and you still have a user logged in   
- Manual Second X Server : if you manually start a second, completely independent window manager, it will take the next available slot which is :1 

- **USER Variable :** Acts as a personal name tag that software and scripts use to identify the current operator of the machine   
  Example: echo $USER (if we were logged into fred user it would say following)  
  	               Fred   
    
- **HOME Variable :** P  
-   
- points to a user’s home directory, where personal files, documents, and settings are stored.   
  \* This file path ie where the home variable points to, can be used by programs to identify where they can save or retrieve my user-specific data  
  Example: echo $HOME  
                /home/Fred   
    
- **SHELL Variable :** Indicates the command-line interpreter running the current terminal session. The SHELL variable is the “language interpreter” for everything said to the computer.   
  echo $SHELL  
  /bin/bash  
  Examples:              
  	Bash  
  	Zshell   
    
- **PS1 (Prompt String 1\) Variable :** Defines the style and content of the command prompt. This variable uses escape characters, which can be useful when managing linux servers.   
  Escape Characters need to know  
- **\\u** : **U**sername (u for user)  
- **\\h** : **H**ostname (h for host)  
- **\\w** : **W**orking Directory (full path)  
- **\\W** : **W**orking Directory / current folder (Trailing/Short name only)  
- **\\$** : Privilege Indicator (Shows **\#** for Root, $ for all other users)

Difference between \\w and \\W  
**Lowercase `\w` (Wide):** Shows the whole path: `[tony@linux-srv/var/www/html]$`  
**Uppercase `\W` (Window):** Shows just the current folder: `[tony@linux-srv html]$`  
**Examples:**   
PS1=”\\u@\\h:\\w\\$ “  
Result: tony@linux-lab: /var/log$   
\\w \= wide (whole path) like a kid, they are short but wide and fat   
\\W \= working directory, the working man MAN which are big so big W 

**The “Expansion Rule” :** 

- Double Quotes(“  “)**Are Weak**: Allow variables to “Expand” (change)   
  - When you open the terminal, the shell reads .bashrc, and sets the variable once, and that variable is set to that for the entire session.   
  - So for \\t in double quotes, it would be stuck and wouldn't change after each time you hit enter.  
  - Its like a snapshot of the variable at the start of the session  
- Single Quotes (‘   ‘)**Are Strong**: Keep everything literal  
  - It updates every time you start up the shell / every time you hit enter it updates for the new prompt  
  - So for \\t every time you hit enter to a new prompt it will look at the variable and update it. This makes it look like your clock is ticking.    

**Editing the PS1 variables**  
**(User Level)**  
**File Path:** \~/.bashrc  
**Use Case:** If you want to change the root user prompt style to easily know when you are in root user, but don’t want to change any other user prompt style. 

**(Global Level)**  
**File Path:** /etc/bash.bashrc (Debian/Ubuntu) or /etc/bashrc (RHEL/CentOS)  
**Use Case:** Setting a “company standard” prompt or adding a “Production Server” warning to the prompt for all staff

Other Prompt variables (less about needing to know and more of just having an idea of them) 

**PS2 :** The “continuation” prompt (If you type a command and hit Enter without finishing it (like an open quote), you see a \>. Thats PS2.  
**PS3 :** Used as the prompt for the select loop in scripts  
**PS4 :** Used when debugging scripts (bash \-x) to show execution traces

- **PATH Variable :** A colon-separated list of directories where the shell looks for executable files. Its like a list of directories that when you run a command or something like ls the shell will look through the path variable directories from left to right until it finds the file or ls file and then it runs it.   
  Example: echo $PATH   
                  /usr/local/bin:/usr/bin:/bin:/usr/games   
    
  So when you type the ls command it goes through that list from left to right  
1. Is ls in /usr/local/bin (no)  
2. Is ls in /usr/bin (no)  
3. Is ls in /bin (yes, and runs the file / command) 

- **Modifying PATH Variable :** These are how you would add a new folder like /opt/myapps to the path so you can run custom tools easily   
- **Temporary Way (Current Session) :** literally type in the command 

  export PATH=$PATH:/opt/myapps	  (this will add the /op/myapps directory to the end of the list)

  export PATH=/opt/myapps:$PATH  (this will add directory to beginning of list)


- **Permanent Way :** To make the change stick, you have to write that equation / command into the file the shell reads every time it start up. (\~/.bashrc) \<------ The file you modify 

  **vim \~/.bashrc**

  (At the very bottom of the file type following) 

  **export PATH=$PATH:/home/fred/bin** 

  (adds the folder called bin in Fred’s home directory to the end of the list)

  **Save and Exit**

  **source \~/.bashrc** 

  (This command applies the changes you made to the PATH variable / bashrc file) 


		Example I might use this for:  
		If you wanted to have an easy way to start firefox from the terminal, you could add the directory that firefox was located in and add it to the PATH variable. Then type firefox and it would run / open firefox. 

**Delete a Variable**  
**unset** \- This command removes the variable from the current shell’s memory entirely   
**unset PS1** (this will remove the PS1 variable from the current session (or memory)

\* The unset command will delete the entire variable, you CAN’T just remove part of the variable, and there is NO way to do that.  
\* This command only affects the current shell session, to completely remove the variable you have to manually delete it from the \~/.bashrc file

Some new Commands: 

which (shows exactly which folder a command is being pulled from)  
**which ls**  
**\# Result: /bin/ls**

**Commands to view these variables**   
**(&) Backgrounding :**  Put & at the end of a command to tell the shell to run the process in the background (You can keep your terminal open while the program stays open)  
**`printenv`**: Displays environment variables (the exported ones).  
**`env`**: Similar to `printenv`, often used to run a program in a modified environment.  
**`set`**: Displays **all** variables (including local ones and shell functions). This list is much longer than `printenv`.

**source \~/.bashrc** and **. \~/.bashrc** both commands do the same thing that is applying the current bashrc modified configuration to the shell permanently. And both commands work in RHEL and debian based distros. 

**\~/.bashrc vs \~/.bashrc\_profile**

- **\~/.bashrc:** The file that is executed for non-login shells (Local login shells)  
- **\~/.bashrc\_profile:** The file that is executed for login shells (Remote login shells / SSH login)  
- It is like this for **security** ensuring all security scripts are run, **Environment Isolation** teats ssh login as a fresh login to ensure it only loads what is necessary for a remote connection.  
- SSH ignores \~/.bashrc unless the \~/.bashrc\_profile tells it to look there

**Where (Global vs. Local Scope)**

- **Local (\~/.bashrc):** Only affects **you**, if Fred changes this, Tony won’t see any difference  
- **Global (/etc/…):** Affects **everyone**, if you want a company standart you must go to the /etc/ folder because that's the “”Master Control room”” for the whole system

**When (Login vs Non-Login)**

- **Login Shell:** You just arrived at the server (SSH or logging into the physical screen)  
- **Non-Login Shell:** You are already at your desktop and you just opened a new terminal window

**The “Debian Trick”:** Debian/Ubuntu sets up a “bridge” When you login, the system reads /etc/profile, but then /etc/profile immediately turns around and turns around and says, “Wait, I should also read /etc/bash.bashrc while i'm at it.

- **Red Hat / RHEL:** calls the file /etc/bashrc  
- **Debian / Ubuntu:** calls the file /etc/bash.bashrc

If you wanted a company standard on RHEL you edit /etc/bashrc and Debian /etc/bash/bashrc

**Paths**

Paths \- Directions to specific locations, folders, or files  
Using the correct path tells Linux precisely which file or folder to work with

**Absolute Paths :** The complete address for the destination allows access regardless of the current location 

- An absolute path is like saying, take me to 123 Main Street, it doesn’t matter where you currently are because you have the complete address for the destination, so you can get there no matter where you  
  - **Root Directory (/):** Represents the root directory, which is the highest level of the filesystem. **All absolute paths start with this /**   
  - You are telling the Linux System to start at the root directory  
      
  - **Home Directory (\~):**  The tilde (\~) represents the current user’s home directory, providing an easy shortcut to a user-specific personal directory.  
  - Also common to use the tilde when referring to folders or files inside your home directory such as \~/Documents

The main difference between the tilde (\~) and the root directory (/) is that the forward slash is the route directory for every user, and the tilde is user specific 

Example:   
**/usr/local/bin**  
System Starts at **root directory**, then goes to **usr directory**, then to **local directory**, finally to **bin directory**.

**Relative Paths :** A path that does **NOT** start with \~ or /. It is a set of directions based solely on your current location / pwd 

- **Directional Rules**  
  - **(..) :** Means the **Parent Directory**, allows you to move up 1 directory / going closer to the root directory   
  - **(.) :** Means the **Current Directory**, allows you to tell the shell to look inside the current directory you are in for the file.  
  - **(-) :** A shortcut used with the cd command to return to the **Previous working directory** (this is stored in the **$OLDPWD** variable) Used as a redo / works on History, or looks at the last place you were and puts you there. 

cd.. Is based on the **folder structure**; cd \- is based on **your previous action** 

Examples:   
**./myscript.sh**   
		This tells the shell to look for a file named myscript.sh and run it. The . tells the shell that the file is in the current directory so it doesn’t need to look for it. 

		Or say you are in your home folder, you can do the following.  
		**./scripts/myscript.sh**   
		This tells the shell to go into the directory named scripts and find the file named myscripts.sh and run it. 

**Tilde Expansion Phase**  
In the Linux execution cycle the shell performs the Tilde Expansion phase as one of its first steps. 

- When the shell sees \~ it immediately replaces it with the string found in the $HOME environment variable   
  \~   \=   /home/Fred   (\~ is converted to /home/localuser   
- Since the final result of this expansion always starts with a /, the system treats this path as an absolute path 

**Diagnostic Commands**

- **pwd (Print Working Directory) :** Shows the absolute path of your current directory  
- **realpath :**  Command that converts a relative path into an absolute path.   
  - You are in \~ and type,  
    **realpath scripts/test.sh**   
    Output: /home/fred/scripts/test.sh   
- **basename :** Returns only the filename from a path  
  - **basename /etc/passwd**  
  - Output: **passwd**   
- **dirname :** Returns only the directory part of the path   
  - **dirname /etc/passwd**  
  - Output: **/etc**

**The Hidden Execution Rule** 

Even if you are in the same folder as a file, the shell will not find it by name alone because the current directory is not in the $PATH variable.

- **myscript.sh** \= The shell looks in the systems official folders ie $PATH variable (command fails)  
- **./myscript.sh** \= ignores the system folders / $PATH; instead looks only in the directory that you are currently in. (command runs / works) 

**Security Reason:** **./** prevents **Path Hijacking**, it ensures that a malicious file named ls in your current folder doesn’t accidentally run when you intended to run the real system /bin/ls 

**\~ VS /** 

- **\~ (Tilde) :** The tilde is the local users home directory, **Local / personal** space, every user is able to access and make changes to there own home directory.  
  - Changes made here affect **ONLY YOU**  
- **/ (Root) :** Access usually requires sudo or being logged into the root user. Not anyone can just change things in this directory, but every user can almost always look / read the / root directory just no change   
  - Rooms / directories that only the root has access to:  
    - /root \- the root users home directory  
    - /lost+found \- a system recovery folder that is usually restricted   
    - /etc/shadow \- A file that contains encrypted passwords for users  
  - Changes made here affect **EVERYONE**

**Difference Between Login User and Non-Login user**

- **Login User:** This occurs when you directly authenticate to the system to begin your entire session.   
  - If you are **logging into the shell directly** to get into the system  
  - Scenarios:  
    - Logging into a physical server at the text-based prompt (No GUI)   
    - Connecting to a remote server via **SSH**  
    - Switching users completely in the terminal with **su \- username**  
      - Note that the \- is what tells the shell to treat this as a login shell  
  - **Login User “Path of Execution” table**

| Step | Level | File | Behavior  |
| :---- | :---- | :---- | :---- |
| 1 | Global | /etc/profile | Always runs for everyone |
| 2 | Local | \~/.bash\_profile | Runs **only if** it exists; stops the search |
| 3 | Local | \~/.bash\_login | Runs **only if** Step 2 fails  |
| 4 | Local | \~/.profile | The **Fallback**; runs if Steps 2 & 3 fail |
| 5 | Local | \~/.bash\_logout | Runs when the session ends  |

    

**Login User “Path of Execution” files explained**

- **/etc/profile:** This file is the system-wide .profile file for the bash shell, it sets a number of variables that every user of the system gets. 

- **\~/.bash\_profile:** This file is bash specific and used for configuring the user environment in login user shells. It can also be used to source or get configuration files from either or both of \~/.bash\_login and \~/.profile files. 

- **\~/.bash\_login:** Also bash specific file, that is only executed if there is no **\~/.bash\_profile** file. This file is only executed for **Login users ONLY**. You also would normally have a line that sources from \~/.bashrc to get the actual session configs, this file is also only run once at first login.

 

- **\~/.profile:** Not a bash specific file, and is used if neither **\~/.bash\_profile** nor **\~/.bash\_login** exists which is normally the case. Main purpose of \~/.profile is to check if a Bash Shell is running and if so source from **\~/.bashrc** if it exists, usually sets the variable PATH so that it includes the user’s private \~/bin directory

- **\~/.bash\_logout:** It is a script that runs automatically the moment you exit a **Login Shell** (like logging out of an SSH session or a text-based server)  
  - **What it handles**  
    - **Clearing the Screen:** It runs the clear command so the next person sitting at that computer can’t see what you were doing  
    - **Deleting Temporary Files:** It wipes out any temporary files you created on the hard drive during your session.  
    - **Logging:** It can record exactly what time you left the system for security records.

**Non-Login User “Path of Execution” Files explained**

- **/etc/bash.bashrc:** This is the system-wide **.bashrc** file for interactive bash shells. It makes sure that it is being ran by a human, it checks window size after each command (updating the values of LINES and COLUMNS if needed) and sets some variables like aliases and PS1 variables.   
    
- **\~/.bashrc:** This is the local config file that the shell looks at to get the shell configs for the local user. Similar to those described for /etc/bash.bashrc.   
  This file usually sets some history variables and sources \~/.bash\_aliases if it exists. **Normally used to store users’ specific aliases and functions** 

**PATH Pollution :** The repetitive appending of the same directories to the $PATH variable caused by re-reading configuration files in sub-shells ie. **If its a variable that inherits like $PATH, only define it in the file that runs once (/etc/profile)**   
Example  
/etc/profile \= export PATH=/usr/bin:$PATH  
\~/.bashrc \= export PATH=$PATH:/opt/special\_tool/bin  
What path variable looks like if you had this set up   
$PATH \= /usr/bin:/opt/special\_tool/bin

**But if you open a child shell the $PATH variable just gets more messy and it takes the shell longer to search through 50 directories for one file where it wouldn’t normally take as long.** 

**Commands**

- **su (Substitute User) :** allows you to start a new shell session as another user without logging out of your current account   
  - If you don’t specify a name it assumes you want to be the root user  
  - If you do like **su bob** it will try to switch you to the user ‘bob’   
  - **What happens:** You switch users but your shell does this for environment variables  
    - **Inherit** the environment from the parent shell (the previous user)  
    - **Read** /etc/bash.bashrc (System-wide settings for non-login shells)  
    - **Read** \~/.bashrc (Bob’s personal settings which if there are overlapping variables this overrides those)  
  - This starts a **Non-Login Shell**  
- **su \-** (**The Login Way) this is how you should do it**  
  - **What Happens:** This is a full login, The system acts as if that user just sat down at the computer, it loads their specific .bash.profile, changes to their home directory, and sets their $PATH  
  - This is a **Login Shell**   
  - You want to always use **su \-** because it puts the terminal into a fresh new state that you know doesn’t have any leftover things from the previous user, so you know that any problems you run into is going to be in your users configs and not the last guys messing yours up. 


**History And Shortcuts**

**history Command :** Displays a list of all commands entered in current and previous sessions, making it easy to recall and reuse them. 

- A linux admin might use this command to review past system commands to troubleshoot issues or ensure that recent configs were applied correctly  
- History is stored in the **\~/.bash\_history** file located the user home directory

**Memory vs Disk**

- **HISTSIZE** **:** The number of commands kept in RAM during the current session  
- **HISTFILESIZE :** The number of commands kept in the \~/.bash\_history file on the disk

**Clearing History**

- **history \-c :** Clears the current session’s history from RAM  
- **history \-w :** Forces the current RAM history to be written to the disk file immediately

Example:   
	history | grep systemctl   
	Output: would list all system service management commands that have executed in the terminal 

**\!\! Operator (bang bang) :** A quick way to re-run the very last command you executed 

- If a recent attempt to restart a service resulted in an error, simply type \!\! to rerun the last command without typing it. 


**\! Operator (bang) :** Allows user to execute a command from the history file that starts with a specific prefix or is identified by a line number  
Example:   
	History command output: 43 systemctl restart networkmanager.service  
	                             Typing **\!43** will re-execute the command   
	Command that will be executed:	**systemctl restart networkmanager.service**   
	**\!sys** : will recall the most recent command that started with the string “sys”

**\!$ Operator (The Last Argument) :** Recalls only the last word from the previous command  
Example:   
You type : mkdir /var/www/html/long\_folder\_name  
                 	cd \!$  
	It will put you in the /var/www/html/long\_folder\_name directory 

**alias Command :** Creates custom shortcuts for longer or more complex commands, tailoring the workflow to specific needs 

- Typically defined on a per user basis, so alias that you create by default cannot be used by other users  
- You would usually put these in your \~/.bashrc file because it is local, but you can put them in the /etc/  
- To make an alias active only fo the current session, type the alias directly into the terminal

**Alias commands**

- **alias :** list every alias currently active in your shell  
- **unalias {name} :** use to delete an alias for the current session  
- **Bypassing an Alias:** if you have **alias ls=’ls \-la’** you can do **\\ls** to run the default command

Example:   
alias restartnm=’sudo systemctl restart networkmanager.service’  
alisa {name}=‘command’  
You only have to enter **restartnm** to run the command to restart the networkmanager

\* History and shortcut commands can streamline workflow, making it easier to work with linux, and manage and reuse commands quickly and efficiently

**KeyBoard Shortcuts:**

| Shortcut | Action | Memory Trick |
| :---- | :---- | :---- |
| Ctrl \+ a | Move cursor to the start of the line | **A** is the first letter |
| Ctrl \+ e | Move cursor to the end of the line | **E** for end of the line |
| Ctrl \+ u | Delete/Cut everything from the cursor to the start | **U**n do the start |
| Ctrl \+ k | Delete/Cut everything from the cursor to the end | **K**ill the end |
| Ctrl \+ r | Search history (reverse search) | **R**everse |
| Ctrl \+ l | Clear the screen | Same as the clear command |

**Input Redirection** 

 **Three Standard Streams** 

- **Standard Input (stdin)** **:** File Descriptor **0**, This is usually your **keyboard**   
- **Standard Output (stdout) :** File Descriptor **1**, This is usually your **screen**  
- **Standard Error (stderr) :** File Descriptor **2**, This is usually also your **screen**, but its a separate “pipe” for error messages

**Input Redirection :** Tells a command to read data from a **file** instead of waiting for you to type it into the keyboard. 

**Syntax**  
**command \< file**  
Examples:  
**Counting Lines/Words**

- **Standart:** **wc \-l input.txt** → Output: **10 input.txt** (Command knows the file name  
- **Redirected:** **wc \-l \< input.txt** → Output: **10** (Command only sees a stream of text)   
  \*In this example the redirection “Hides” the source from the command 

**Feeding Data to Commands**

- **Standard: sort unsorted.txt** →Output: **Sorted List**, and the command opens the file directly   
- **Redirected:** **sort \< unsorted.txt** → Output: **Sorted List,** but the **Shell** opens the file and passes the data to **sort**

**Character Translation (tr)**

- **Standard: tr ‘a-z’ ‘A-Z’** → Output: (Blinks and waits for keyboard input because **tr** **CANNOT** take a filenames as an argument)  
- **Redirected:** **tr ‘a-z’ ‘A-Z’ \< file.txt** → Output: Uppercase version of the file (Redirection is **required** here to process a file)

**Database Import**

- **Standard: mysql \-u user \-p database\_name** → Output: Drops you into an interactive MySQL prompt waiting for commands  
- **Redirected: mysql \-u user \-p database\_name \< structure.sql** → Output: Executes all SQL commands inside the file and exits (Automates the process)

**Combining I/O**

- **Standard: sort input.txt** → Output: Displays sorted text on your screen (stdout)  
- **Redirected: sort \< input.txt \> output.txt** → Output: Nothing on your screen; the shell pulls data from one file and pushes the result into another   
  (ie, the output gets put into a file)

**Here Document (Heredoc):** A special type of redirection that allows you to feed a multi-line block of text into a command or script without using an external file.

**Syntax Breakdown:**

To start a Heredoc you use **\<\<** operator followed by a **delimiter** (A word you choose to signal the end)

**This is all inside here.txt (the here document)**

| Syntax | Actual example |
| :---- | :---- |
| command \<\< DELIMITER Line 1 of your text Line 2 of your text DELIMITER | cat \<\< STOP this is a  here document STOP |

**Ways to run this Heredoc**

**The “Interpreter” Way**  
**Command: bash here.sh**

- **The Logic:** You are manually handling the file 