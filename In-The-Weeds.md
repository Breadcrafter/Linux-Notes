<!-----



Conversion time: 1.138 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs™ to Markdown version 2.0β2
* Mon Apr 27 2026 04:42:09 GMT-0700 (Pacific Daylight Time)
* Source doc: Deeper in the weeds of Bash / Shell
----->


**<span style="text-decoration:underline;">How Display Manager Works to load shell configs</span>**

When you type your username and password into the Login screen / Display Manager, the display manager starts a temporary, or invisible shell (called something like Xsession or wayland-session)

This shell is like a script that goes and Sources your profile files like /etc/profile and ~/.bash_profile,  



* This is where the “”shell”” acts as a login shell and not like a non-login shell

**<span style="text-decoration:underline;">Things to look up when I come back to this:</span>**



* **GUI Session Initialization (Specifically Display Manager Startup & Environment Propagation)**
* **Xsession / Wayland-Session scripts**
* **Process Inheritance**
* **Display Manager Architecture**
* **Environment Variable Propagation**
