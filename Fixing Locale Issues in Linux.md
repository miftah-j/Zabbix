# [Debian/Ubuntu] Fixing Locale Issues in Linux ("Falling back to the standard locale ("C").")

### Important!

if you see an error regarding the Locale ("Locale for language "en_US" is not found on the web server."), then please [follow this guide to solve this problem](https://community.time4vps.com/discussion/8042/debian-ubuntu-fixing-locale-issues-in-linux-falling-back-to-the-standard-locale-c#latest "follow this guide to solve this problem").

![](https://community.time4vps.com/uploads/editor/ze/lh887fzn90pq.png)



## Introduction

If you're encountering warnings related to locale settings on your Linux system, such as:

> perl: warning: Falling back to the standard locale ("C").  
> locale: Cannot set LC_* to default locale: No such file or directory

These errors generally indicate that the system is not configured with the correct locale settings. While this may not prevent you from installing software or running most applications, fixing it ensures a cleaner system environment and prevents potential issues.

### Why Fix It?

The main reasons to fix it would be:

**Cleaner System:**  Resolving the warnings eliminates unnecessary errors, making system logs cleaner and easier to troubleshoot.

**Locale Consistency:**  Some applications rely on proper locale settings for correct text encoding, date formats, and character display.

**Avoid Future Issues:**  A properly configured locale prevents potential issues related to character encoding or regional settings in software.

### If You're Not Concerned

If you're not experiencing any issues beyond the warnings, fixing the locale is not strictly necessary. Your system will still function properly for most tasks, including software installations.

## Guide to Fix Locale Issues

### 1. Edit the /etc/locale.gen File

This file controls which locales are generated for your system.

Open the file for editing:

`vi /etc/locale.gen`

Find and ensure the following lines are uncommented (remove the # at the beginning if present):

> en_US.UTF-8 UTF-8  
> lt_LT.UTF-8 UTF-8

Save and exit.

### 2. Regenerate Locales

Run the following command to apply the changes:

`locale-gen`

This will generate the necessary locales based on the settings in  **/etc/locale.gen.**

### 3. Verify Available Locales

To check which locales are now available on your system, run:

`locale -a`

You should see  **en_US.UTF-8**  and  **lt_LT.UTF-8**  listed.

### 4. Update Locale Settings

To apply the correct locale settings, run:

`update-locale LANG=en_US.UTF-8 LC_CTYPE=en_US.UTF-8 LC_MESSAGES=en_US.UTF-8`

### 5 Check /etc/default/locale

Ensure the /etc/default/locale file contains the correct settings by running:

`cat /etc/default/locale`

It should output:

> LANG=en_US.UTF-8  
> LC_CTYPE=en_US.UTF-8  
> LC_MESSAGES=en_US.UTF-8

### 6. Reboot Your System

Reboot to ensure all changes take effect:

`reboot`

### 7. Verify After Reboot

After restarting, check if the locale settings are now correctly applied:

`locale`

You should see the correct locale settings without errors.

> LANG=en_US.UTF-8  
> LANGUAGE=  
> LC_CTYPE=en_US.UTF-8  
> LC_NUMERIC=lt_LT.UTF-8  
> LC_TIME=lt_LT.UTF-8  
> LC_COLLATE="en_US.UTF-8"  
> LC_MONETARY=lt_LT.UTF-8  
> LC_MESSAGES=en_US.UTF-8  
> LC_PAPER=lt_LT.UTF-8  
> LC_NAME=lt_LT.UTF-8  
> LC_ADDRESS=lt_LT.UTF-8  
> LC_TELEPHONE=lt_LT.UTF-8  
> LC_MEASUREMENT=lt_LT.UTF-8  
> LC_IDENTIFICATION=lt_LT.UTF-8  
> LC_ALL=


After that, restart Apache and Zabbix and try to complete the Zabbix interface configuration again:

`systemctl restart apache2`  
`systemctl restart zabbix-server`
