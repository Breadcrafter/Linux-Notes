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
    
- **HOME Variable :** points to a user’s home directory, where personal files, documents, and settings are stored.  
     
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
  - It's like a snapshot of the variable at the start of the session  
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
  -   
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

**Here Document (Heredoc):** A way to send **multi-line** **text** into a command without using an external file. You are telling the command “Accept everything I type as input until you see my ‘STOP’ word”

**Syntax Template (this is typed directly into the command line)**

command \<\< DELIMITER  
Line 1 of text  
Line 2 of text  
DELIMITER 

- **\<\< :** The Heredoc operator (what tells the shell that it is a “heredoc”  
- **DELIMITER :** A stop word you choose (standard is **EOF**)  
- **The Closing Word :** Must be on its own line with **no spaces** around it (this is also a **DELIMITER**) 

Examples: 

| Use Case | Code Example | Result |
| :---- | :---- | :---- |
| Print to Screen | cat \<\< EOF Hello World EOF | Displays “Hello World” immediately |
| Create a File | cat \<\< EOF \> notes.txt Linux+ Study EOF | Saves “Linux+ Study” into notes.txt |
| Filter Text | grep “Error” \<\< EOF Log: Success Log: Error EOF | Only outputs the line containing “Error” |

**Heredoc Redirection Modifiers**

- **Variable Expansion:** By default, if the shell finds a **$** followed by a name it will attempt to “expand” it by replacing that label with the specific info stored inside that box  
    
- **Strong (single) Quotes (Literal Rule) :** This is an application of “Strong Quotes” rule. Using **(‘EOF‘)** tells the shell to ignore all special characters and keep the text exactly as typed.   
  (You put the single quotes around the first delimiter, it will still run but every variable inside that heredoc will not expand they will all be literally what they are)   
  Example:


| Typed directly into the shell (Input) | Output / what's inside notes.txt |
| :---- | :---- |
| cat \<\< ‘FINISH’ \> notes.txt The user is $USERFINISH | The user is $USER |


  

- **The Dash Operator (\<\<-) (Whitespace Handling) :** This is a **Formatting Modifier**. Its only job is to clean up the “Path of Execution” inside a script by removing leading tabs so the code stays readable. 

**Appending & Here String**

- **Append Output (\>\>) :** \> which wipes or sets the file to 0 bits before the command is actually ran to prepare the file for the output. **\>\>** starts a new line, then “pastes” or adds the output.   
  - It will start a new line below the last line that you made / have even if that line is empty if you hit enter didn’t put anything you will see a space between what you typed and the output of that command.   
- **Here String (\<\<\<) :** A Here String is a way to “force-feed” a single piece of text directly into a command.   
  - Normally commands like **tr, grep, or bc** expect to read from a file, but if you don’t have a file and just want to use a word or a variable, the Here String acts like a funnel 

**The 3 ways to feed a command**

- **The File Method (\<) :** tr ‘a-z’ ‘A-Z’ \< filename.txt  
  - Analogy: Handing the machine a physical book to read  
- **The Pipe Method ( | ) :** echo “linux” | tr ‘a-z’ ‘A-Z’  
  - Analogy: One machine (echo) splitting text into the next machine (tr)  
- **The Here String Method (\<\<\<) :** tr ‘a-z’ ‘A-Z’ \<\<\< “linux”  
  - Analogy: You personally dropping a single note into the machine 

The tr command explained a little bit more

The tr command can be used to do more than just change works or files from uppercase to lowercase and vise versa. It is basically a **“character-level formatter”**  
It acts only as a filter for data passing through it   
**Basic Syntax**  
	**tr \[OPTIONS\] SET1 \[SET2\]**

- **Options:** Special flags that change how tr behaves (like deleting or squeezing)  
- **SET1:** The “Search” character. This is what you currently have in the file  
- **SET2:** The “Replace” characters. This is what you want them to currently become  
  - **SET2** is optional, if you are using the delete flag (-d) you only need **SET1** 

Examples 

- **Making the $PATH variable readable:**  
  - tr ‘:’ ‘\\n’ \<\<\< $PATH  
  - **Output:** /usr/bin  
    	   /bin  
    	   /usr/local/bin  
    	   /usr/sbin

- **Cleaning Up Extra Whitespace (Squeeze):**   
  - tr \-s ‘ ‘ \<\<\< “Too     many     spaces”  
  - **Output:** Too many spaces 

- **Converting CSV Data to a Vertical List:**  
  - CVS file \= jdoe,asmith,tstark,pizzo  
  - tr ‘,’ ‘\\n’ \< users.cvs   
  - **Output:** jdoe

       asmith 

       tstark 

       pizzo

- **Stripping Numerical Junk (Delete):**  
  - tr \-d ‘0-9’ \<\<\< “L1i2n3u4x5  
  - **Output:** Linux 

- **Reformatting a List into a Tab-Separated Row**   
  - List.txt \=   
    - blue   
    - yellow   
    - red   
    - green   
  - tr ‘\\n’ ‘\\t’ \< list.txt  
  - **Output:** blue    yellow  red     green   localhost:\~\# 	  
    

**Error Redirection**  
   
**Redirecting Standard Error (2\>) :** By default **\>** ONLY moves **stdout**. If a command fails, the error message bypasses your redirection and hits the screen anyways. To catch it, you use its File Descriptor number **2**. 

- **Overwrite Errors:** command 2\> error.log  
- **Append Errors:** command 2\>\> error.log

**The “Everything” Reducer (&\>) :** If you want to send everything **(stdout and stderr)** to the same file, use the ampersand shortcut **(&)**.

- **command &\> all\_output.txt**

**The “Black Hole” (/dev/null) :** If you want to not see errors like “permission Denied” when searching a whole system, you can send them to **/dev/null**, which is a special virtual file that just deletes whatever you send to it.

- **Mute Errors:** find / \-name “secret.txt” 2\> /dev/null  
- **Total Silence:** command &\> /dev/null

**The tee Command (The “Y-Splitter”)**  
Normally, redirection is either one place or another not both, Either: Output goes to the screen **(Stdout)**, or it goes to a file **(\>)**. But **tee** does **both**. 

- **It takes data coming through a pipe and splits it so you can see it on your screen AND save it to a file simultaneously**

**Basic Syntax**  
You almost always use **tee** with pipe **( | )** because it needs to “catch” the data from a command.   
**command | tee filename.txt**  
What happens:

- **command:** runs and generates data  
- **| :** catches that data (stdout) and puts it into (stdin) of the second command  
- **tee:** grabs the “input” ie. stdout of |, and   
  - **Action 1:** Prints it on your screen so you can watch it   
  - **Action 2:** Writes it into filename.txt

**The “Append” Mode (-a)**  
Tee automatically wipes the file by default will **overwrite** the file file. If you want to **append** (add to the bottom), you use the \-a flag. 

- **Overwrite:** command | tee log.txt  
- **Append:** command | tee \-a log.txt

**If you want to “append a log while monitoring the output” the answer is tee \-a**

**Real World Examples of the tee command**

**Monitoring Long-Running Installs (standard tee)**  
**Scenario:** Running a system update or build script  
**Command:** make | tee build\_log.txt  
**Result:** You watch the build progress live, but **build\_log.txt** captures everything for troubleshooting 

**The “Sudo Redirection” Workaround**  
**Scenario:** Adding a new DNS server to your protected configuration   
**The Fail:** sudo echo “nameserver 8.8.8.8” \> /etc/resolv.conf (Permission Denied)  
**The Fix:** echo “nameserver 8.8.8.8” | sudo tee \-a /etc/resolv.conf  
**Result:** The string successfully appends to the end of the file, and the file isn’t wiped

**Continuous Auditing with tee \-a (Append)**  
**Scenario:** Tracking server health checks throughout the day  
**Command:** uptime | tee \-a server\_health.log  
**Result:** Every time you run this it adds a new timestamped line to the bottom of server\_health.log instead of deleting the previous checks. 

**Writing to Multiple files at once** 

- **Command:** command | tee file1.txt file2.txt file3.txt  
- **Use Case:** Sending logs to a local folder, a backup drive, and a network share all in one go. 

Examples   
**The “Triple threat” Logging** 

- **Command:** ./deploy\_script.sh | tee internal.log /shared/network/deploy.log backup.log  
- **Use Case:** You save a copy for yourself, a copy for your teams shared drive, and a backup copy simultaneously. 

**Version Control & Latest State**

- **Command:** system\_audit.sh | tee audit\_$(date \+%F).log latest\_audit.log  
- **Use Case:** You create a unique file with today’s date (e.g., audit\_2024\_05-12.log) but also update a file called latest\_audit.log. This allows other scripts to always find the newest data without needing to know the date. 

**Handling Interruptions (The \-i flag)**

- **Command:** long\_script.sh | tee \-i output.log  
- **Use Case:** This ensures that even if the terminal session gets jittery or someone tries to kill the process, tee finishes writing the buffered data to the file before exiting, preventing log corruption

**Permission Errors**   
If you use the tee command to write to a file that you do not have access to, you must put sudo in front of the tee command and not the first command in the line. As when you pipe, sudo only applies to the command that it is in front of and ends at the start or end of the pipe.   
**Examples**

- **sudo dpkg \- \-get-selections | tee /var/log/packages.log                             (wont work, because tee doesn’t have access only dpkg does)**  
    
- **dpkg \- \-get-selections | sudo tee /var/log/packages.log**  
  **(will work because tee does get sudo privileges to access that file)**

**The “Sudo Tee” Workaround**  
**sudo echo “text” \> /file** (fails because \> /file doesn’t have sudo only echo does)  
**echo “test” | sudo tee /etc/protected.conf** (works because tee has sudo privileges, and which is required to open and write to the file) 

**The Shell Redirection “Order of Operations”**

- The shell processes redirection from left to right, before the command even runs. 

Examples   
**sort file.txt \> file.txt**  
The shell sees \> file.txt, and immediately opens file.txt to wipe it, and now that the file is empty the shell finally runs the sort command but fails because the file is empty.

**sort \< file.txt \> file2.txt**   
The shell does go from left to right so it sees \< file.txt and prepares to plug that file into the commands **stdin (0)**, then the shell sees \> file2.txt and wipes the file and prepares it to get the stdout of the command.  
This happens almost simultaneously but the shell technically does read from left to right just happens so fast. 

**“No Clobber” (Safety Mechanism)**  
**Command:** set \-o noclobber **(to turn noclobber on)**  
**Command:** set \+o noclobber **(to turn noclobber off)**

**Effect:** This prevents you from accidentally overwriting an existing file with \>. If you try, the shell will throw an error.

**The Bypass:** If **noclobber** is on, you can use **\>|** to force the overwrite anyways. 

**Input Redirection vs. Arguments**

- **Arguments:** cat file.txt (The Command opens the file)  
- **Redirection:** cat \< file.txt (The shell opens the file and feeds the “insides to the command)  
- **Exam Logic:**If a question asks how to feed a string to a command that **does not** accept file arguments (like **tr** or **bc**) Redirection or Pipes are the **Only** answer. 

**Regex (Regular Expression)**  
A specialized language and essential administrator tool used to describe and match patterns within text. 

Regex is not a command, it is a series of character-based structures that define patterns used for searching text. 

Regex is used with the grep command to search for more specific strings / words inside a text document or file 

**Uses:**

- **Troubleshooting**   
- **Incident Response**  
- **Filter output from commands**  
- **Automate text processing tasks**

**Symbols**

**The Characters and Wildcards (“The What”)**

- **abc (Literal Characters):** Matches those exact letters in that exact order.

- **. (Period):** The wildcard which matches **any single character** (a letter, a number, a space, or a symbol)   
  - Just like a joker in a card game it can represent any card in the deck

- **\\ (Backslash):** The Escape Character, It forces the system to treat the very next character as a literal character. (e.g., \\. means a literal period, not a wildcard)


**Character Classes (“The Pick One”)**  
	The Square brackets is like a list of acceptable choices, they always match exactly **one** character

- **\[ \] (Square Brackets):** Matches any single character thats inside the brackets. (e.g., \[aeiou\] matches any single vowel)  
  - They are kind of like an escape character where anything inside them is literal, and not with special features, kind of. 


- **\- (Hyphen inside brackets):** Specifies a sequential range, **\[a-z\]** for lowercase letters, **\[A-Z\]** for uppercase letters, or **\[0-9\]** for numbers  
    
- **^ (Caret inside brackets):** The not, It means match anything except what is in these brackets, **\[^0-9\]** means any single character that is NOT a number. 

**Anchors (“The Where”)**  
	Anchors don’t match actual characters they specify the exact location on the line where the match must happen

- **^ (Caret outside brackets):** “Locks” the match to the **beginning of a line**   
  - Example: **^system** means the line must start with “system” 

- **$ (Dollar Sign):** “Locks” the match to the **end of a line**  
  - Example: **false$** means the line must end with “false”


- **\\\< (Backslash \+ Less Than):** “Locks” the match to the **beginning of a word**  
- **\\\> (Backslash \+ Greater Than):** “Locks” the match to the **end of a word**

**Quantifiers (The “How Many Times”)**  
	These require Extended Regex (grep \-E or egrep). They come right before the single character or brackets telling it how many times to repeat 

- **? (Question Mark):** The preceding item is optional. It can match **0 or 1 time**  
  - **Example:** **ab?c** will match either the strings “abc” or “ac” because the b is considered optional  
- **\* (Asterisk):** The preceding item can repeat **0 or more times**  
- **\+ (Plus Sign):** The preceding item must appear **1 or more times** (mandatory, can’t be skipped)  
- **{ } (Curly Brackets):** Explicit count  
  - **{3}** means exactly 3 times  
  - **{2,5}** means between 2 and 5 times  
  - **{2, }** means 2 or more times

**Alternation and Grouping (The “Logic”)**  
These also require Extended Grep or grep \-E 

- **| (Pipe):** The logical **OR** operator, lets you match the full pattern on the left OR the full pattern on the right   
- **(  ) (Parentheses):** Groups characters together, so you can treat them as a single unit or apply a quantifier to the whole group   
  - Example: **(copy)+** looks for “copy”, “copycopy”, etc

**MetaCharacters**  
MetaCharacters are special characters in regex that can be used to match specific character types. 

- **\\d :** Can be used to match **any digit from 0 to 9**  
- 

**Search & Extract Commands**

- **awk (The Data Slicer)**  
  - **What it does:** Processes **structured data** (columns/fields)   
  - **Syntax:** awk ‘/pattern/ {print $1, $5}’ filename   
  - **How it works:** */pattern/* acts like a filter (only looks at lines containing that word)  
    - **{print $1, $5}** extracts specific columns. **$1** is column 1, **$5** is column 5\.  
  - **$0** means the entire line

- **grep (The Pattern Finder)**  
  - **What it does:** Searches text files line-by-line for specific words or regex matches  
  - **The Must Know Flag:** \-i (Case-insensitive) it forces grep to find “fail”, “FAIL”,  or “Fail”  
  - **Syntax:** grep \-i “fail” /var/log/auth.log  
    -  grep \-E ‘regex pattern’ filename

- **cut (The Column Clipper)**  
  - **What it does:** Extracts vertical columns from files that use a specific separator (delimiter) like colons, commas, or tabs  
  - **Must Know Flags:**  
    - **\-d:** Delimiter (the character separating the columns)  
    - **\-f:** Field (the specific column number you want to extract)  
  - **Syntax:** cut \-d':' \-f1 /etc/passwd

**Modify and Replace**

- **sed (Stream Editor)**  
  - **What it does:** Modifies, replaces, or deletes text in a file line-by-line **without manually opening it**.  
  - **Syntax:** sed ‘s/old/new/g’ filename   
    - **s \=** Substitute (tells sed we are replacing something)  
    - **/old/** **\=** The text you are searching for  
    - **/new/ \=** The text you want to replace it with  
    - **g \=** Global (forces it to replace every instance on a line, not just the first one)  
  - **Other Flags**   
    - **\-n** with **p:** Suppresses normal output and only prints explicit matches (sed \-n ‘/pattern/p’)  
    - **d :** Deletes lines matching a pattern (sed ‘/pattern/d’)  
  - **IMPORTANT**  
    - By **DEFAULT**, running sed only displays the changes on your screen; it does not actually change the file.   
      - **To save the changes directly to the file, you must add the \-i (in-place) flag:**   
        - sed \-i ‘s/old/new/g’ filename


  **Sort and Count**


- **wc (Word Count)**  
  - **What it does:** Counts the lines, words, and bytes in a file  
  - **Flag need to know:** \-l (count lines)  
  - **Exam Scenario:** Used to see how fast a log file is growing or how many items are in a list.  
  - **Syntax to know:** wc \-l /var/log/syslog (outputs the total number of lines)

- **sort (Data Organizer)**  
  - **What it does:** Rearranges text lines alphabetically or numerically  
  - **Must Know Flags:**  
    - **\-t :** Sets the delimiter/separator (just like \-d in cut)  
    - **\-k :** Specifies the column (key) number to sort by  
    - **\-n :** Sorts **numerically** (otherwise, 10 comes before 2 alphabetically)  
    - **\-r : Reverses** the sort order (highest to lowest)  
  - **High-Value Syntax:** sort \-t: \-k3,3 \-n \-r /etc/passwd  
  - Translation: Open ‘/etc/passwd’, split it by colons (‘-t:’), look at column 3 (‘-k3,3’), sort it by actual numbers (‘-n’), and reverse it (‘-r’) so the biggest UID number is at the top) 


- **uniq (The Duplicate Destroyer)**  
  - **What it does:** Removes duplicate lines  
  - **IMPORTANT:** uniq ONLY removes duplicate lines that are right next to each other, (adjacent). Therefore you MUST run sort before running uniq.   
  - **Must Know Flag:** **\-c** (count). It tells you how many times a duplicate line appeared.   
  - **Best Combo:**  
  - **sort /var/log/auth.log | uniq \-c**  
  - Translation: sort the log file alphabetically so all identical errors sit next to each other, then count how many times each error happened. **Good for spotting brute force hacking attempts**

- **xargs (The Bulk Executor)**  
  - **What It Does:** Takes a list of items from a previous command and feeds them into a new command one by one.   
  - **Why Used:** Commands like cp, rm, or kill can't read text streams natively, so xargs bridges that gap  
  - **Syntax:** find /home/logs \-name “\*.log” | xargs rm  
  - Translation: Find all files ending in .log and pass that list to xargs, which immediately executes rm on every single one of them to delete them. 

**View and Navigate**

- **head (View the Beginning)**  
  - **What it does:** Displays the beginning of a file. Default prints first 10 lines  
  - **Flags:** \-n : Specifies the number of lines to print   
  - **Syntax:** head \-n 50 /etc/apache2/httpd.conf   
    - This will display the first 50 lines of the apache2 httpd config file

- **tail (View the End)**  
  - **What it does:** Displays the end of a file. Default is last 10 lines  
  - **Flags:**   
    - **\-n :** Number of lines   
    - **\-f :** Follow (very important, **It displays the end of a file LIVE as new data is appended to it**   
  - **Syntax:** tail \-n 20 \-f /var/log/syslog   
    - Displays the last 20 lines of syslog, AND keeps the terminal open to show new logs in real time

- **more (Basic Pager)**  
  - **What it does:** Utility that stops text from flying off your screen by displaying it one screenful (page) at a time (Can only scroll forward not backwards)  
  - **Navigation:**  
    - **Spacebar:** Advance one full page  
    - **Enter Key:** Advance one single line 

- **less (Advanced Pager)**  
  - **What it does:** better version of more (“Less is more, but better”)  
  - **How its better:** Lets you scroll both forward and backward, and search within the stream  
  - **Navigation (important to know)**  
    - **Spacebar:** Scroll forward one page  
    - **b :** Scroll **backward** one page   
    - **/ (forward slash) :** Activates interactive in-file search (Type **/error** to jump straight to matches)   
    - **q :** Quit and exit the viewer (if you get “stuck” in a terminal on exam, press q always pres q after done viewing) 

**Output Redirection Commands** 

- **read**   
  - **What it does:** Pauses a script or terminal session to accept interactive text input typed by a user and stores it into a custom variable.   
  - **Flag:** **\-p :** Prompt (displays a custom message on the screen so the user knows what to type)  
  - **Syntax:** read \-p “Enter your name: “ username   
    - This displays the prompt message “Enter your name: “, wait for the user to type something and hit enter, and then dynamically save whatever they typed into a new variable named **username**

- **echo**   
  - **What it does:** Prints text strings or the contents of variables directly to the screen or redirects them into a file.   
  - **Syntax:** echo “Welcome, $username\!”   
    - $username \= cody  
    - This prints “Welcome cody\!”

- **cat**  
  - **What it does:** Concatenates (merges) and reads files sequentially from left to right  
  - **Syntax:** cat log1.txt log2.txt log3.txt \> full\_log.txt  
    - This will basically combine all of the logs into one file named full\_log.txt. So you don’t have to sift through each individual file its all in one place. 

**Math and Format**

- **printf**   
  - **What it does:** Prints a formatted text and variables. Allows you to build highly structured layouts, tables, and spacing using strict formatting.  
  - **Flags / Specifiers:**   
    - **%s :** Declares that a text string variable will be inserted here  
    - **\\n :** Forces a newline break (unlike **echo**, printf doesn’t automatically create a newline at the end of a sentence unless you explicitly use \\n)  
  - **Syntax:** printf “User: %s\\nHome Directory: %s\\n” “$USER” “$HOME”   
    - So what this is going to print is the following below. Easiest way to explain is that any time **%s** is used, it is going to use the next unused variable after the “prompt” so the first variable used will be $USER, so the %s will be using $HOME. Like a fill in the blank.  
    - User: cody  
      Home Directory: /home/cody/

- **bc (Basic Calculator)**   
  - **What it does:** Allows bash to do math with decimals, by processing advanced expressions and decimals   
  - **Must Know Variable:** **scale=\[number\]** : Specifies exactly how many decimal places you want bc to calculate to. If you don’t specify defaults back to whole integers which defeats the purpose of using bc.   
  - **Syntax?:** echo “scale=2; (2.45 \+ 3.67 \+ 4.89) / 3” | bc   
    - Does a lot of math mainly know that scale \= 2, so the final answer is going to have 2 decimal places. 

**System and Scripts** 

- **uname**   
  - **What it does:** Prints detailed hardware and software info about current Linux machine, kernel config, and cpu architecture.  
  - **Flags:**  
    - **\-r :** Kernel **Release** (prints just the specific version string of the active kernel)  
    - **\-i :** Hardware Platform / Instruction set architecture  
    - **\-a :** **All** available system metadata printed in a single text block  
  - **Syntax:** echo “System Info: $(uname \-a)” \>\> /var/log/sys\_update.log  
    - You put uname \-a into a $() command substitution wrapper to capture the raw block of info and hand it directly to the echo command. This basically just appends everything that uname \-a gives you and appends it to the sys update log. 

- **source**   
  - **What it does:** Executes a script or text file directly inside the **current** shell session.  
    - (Source forces the script to run in your current active terminal, so that environment variables, paths, and shortcuts stay configured permanently after the script ends)   
  - **Syntax:**   
    - **. (dot):** The dot command is a built-in synonym for source, running  **. script.sh**, does the same thing as **source script.sh**   
    - source /etc/profile.d/custom\_vars.sh && echo “Success”  
      - Basically saying, execute and make the changes that are inside this script custom\_vars.sh, and make those changes inside the current active terminal session, without forcing users to log out and log back in. If it does it successfully print the word success to the screen. 

**Text Editors (mostly vim / vi things)**

**Vi / Vim modes**

- **Command Mode:** used for navigation and editing, not directly typing into a file  
  - Pressing the letter **i** from command mode takes you to insert mode

- **Insert Mode:** Text can be entered like in a regular text editor  
  - Once changes made are done press **Esc** to return to Command mode

- **Execute Mode (Ex Mode or Last Line Mode):** Used to enter commands that perform actions like saving, quitting, searching, and replacing text.   
  - Press **: (colon)** to enter Execute Mode from Command mode  
  - **:wq**  \- Save & quit   
  - **:q\!**    \- Quit without saving   
  - **:set hlsearch**   \- Highlight all matches of last search   
  - **:s/old/new/g**   \- Replace all instances in current line   
  - **:\!ls**   \- List files in the current directory (ie. you can run shell commands) 