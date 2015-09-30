# Ghost instructions

## Purpose of these instructions

These instructions aim to teach you how to use Ghost Console **11.5.1.2298**
in conjunction with Ghost Server **2.5.1.2298**, in Duke Biology's environment.
Note that Ghost 2 can only handle Windows computers.
Ghost 3 was released in June 2015 and claims it can handle Windows, OS X, and
Linux, but has not been tested enough in our envrionment.

## Registering a computer

### Prerequisites before registering

-   The computer should be on the **public VLAN**.
This won't be a requirement in the future.
Ryan will need to figure some things out before this restriction is lifted.

-   The **built-in** admin account should be enabled and have a password.
The built-in account's name doesn't have to be `Administrator`.

-   The **"domain"** firewall should be turned off.
The "public networks" or "private networks" firewalls don't need to be off.

 -  The firewall can be turned off via the following steps: Control Panel >
 Click on `System and Security` > Click on `Windows Firewall` > On the left
 sidebar, there's `Turn Windows Firewall on or off`.
 
 -  Often, Symantec prevents you from having access to enabling/disabling
 Windows Firewall.
 In such a case, follow these steps: Open the command line in **Adminstrator**
 mode > Type `start smc -stop` > Close the Windows Firewall window and reopen
 it.
 You should now see you're able to access `Turn Windows Firewall on or off`.
 
-   The computer should be bound to the `win.duke.edu` domain.

-   Double-check if you can actually sign into the computer using the
*built-in admin account*.
It's happened before- for some reason local admin accounts wouldn't have access
into the computer.
**Tip**: In such a situation, re-bind the computer to `win.duke.edu`.

-   There have been cases where the computer had to be signed in to the built-in
admin account in order to become discoverable to the Ghost server.
If possible, stay signed in to the built-in account.
This will need further investigation.

### How to register a computer

-   On Ghost Console, go to `Tools > Remote Client Install > Add`.

-   For the `DOMAIN\COMPUTER` part, write `win.duke.edu\BIO-COMPUTER-NAME`

 -   The `BIO-COMPUTER-NAME` portion, of course, has to be replaced with the
 computer name as bound on the `win.duke.edu` domain.
 
 -   If the computer name has ever been cut off due to the
 15-character NetBIOS limit, *use the cut-off name, not the full one*.
 
 -   Whether to capitalize the domain or the computer name doesn’t seem t
 matter.
 
 -   The domain `WIN\` doesn’t work.
 It has to be `win.duke.edu\`.
 
-   If the progress bar does seem to add up, then things are going well.

 -   If the progress bar stalls at `Connecting...` and then fails, click `View
 Log...`, go to the end of the error log, and read the most recent error
 message.
  
  -   If the message says `Logon failure`, you probably have entered incorrect
  credentials.
  
  -   If the message says `Error connecting to client`, then hopefully the
  computer in question does not keep one of the requirements discussed in the
  previous section.
  If you're absolutely sure that all of the requirements have been met, then
  you have discovered a whole new error- please update this documentation.
  
-   Even when the progress bar adds up and the registration seems to complete,
you'll get one error saying `error copying results file back from the client`
in the error log.
Just ignore this message.
If this error message shows up, that means registration is successful.

 -   If, instead, you see a message saying `Error opening Service Manager`, use
 a different admin account to sign in to the Ghost server.
 You may create a new account and sign in to that.
 This solution has worked in the past.
 (Source: <https://support.symantec.com/en_US/article.TECH199522.html>)
 
-   **You're not done yet**.
Go to `Machine Groups > Default`.

-   Double-click on the computer you've just added.

-   Go to the `Client` tab.

-   Change `Virtual Partition PreOS` to `Winpe3`.
**This last step is very important**.

## Creating an image off of a computer

### Prerequisites before creating an image

- The computer in question, of course, must have already been registered on our
Ghost system.
- Make sure the computer is currently connected to the Ghost network.
You can confirm this by going to `Machine Groups > Default` and checking the
icon next to the computer name in question.
If the icon has a little network wire running through the lower half, then
you're ready to roll.

### Creating an image space

The first thing you have to do, before actually starting to clone an image off
of a computer, is to create a space into which the cloned image will be saved.

-   Go to `Configuration Resources > Images`.

-   Right click on the wide main area. Select `New Image`.

-   Give it a name like `Dell Latitude 3150 factory image (Windows 7 64bit)`.
Make sure to include the precise operating system somewhere in the name.

-   Don't check `Image is located on the Client Machine`.

-   For `Location`, write something like
`D:\images\dell_latitude_thirtyonefifty_win_seven_sixtyfourbit.gho`.

 -   It is important that this file path should **not include any numbers***.
 There is a bug in Ghost 2 that errs when the file name includes a number.
 (Source: <https://support.symantec.com/en_US/article.TECH108828.html>)
 
 -   Also, the path must be inside `D:\images`.
 Ryan did some things to the `D:` drive specifically to make its I/O faster.
 And `\images` is just the conventional location where we're storing image
 files.
 
 - The file extension has to be **.gho**.
 
-   Hit `OK`.

### Executing the clone task

-   On Ghost Console, go to `Tasks`.

-   On the main panel, click on either `Create image (with sysprep)` or `Create
image (without sysprep)`, depending on your needs.

-   For `Source machine`, browse and choose the right computer off of which you
want to clone an image.

-   For `Image`, browse and choose the image space you created above.

-   Don't change anything else.
Hit `Execute`.
**Do not hit `Save`**.

    -   If the task fails after a while at the step `To Virtual Partition`, it's
    probably because you didn't change the `Virtual Partition PreOS` to `Winpe3`
    during the last step in the section above How to register a computer.

-   Once the task seems to be going smoothly, you can go over to the target
computer and see the progress there (if you want to burn some calories).
You can also monitor the progress of the transfer/deployment on the Ghost server
with the Symantec GhostCast server which should have opened up automatically.

-   When the task finishes, go to the target machine and you will see a command
prompt that keeps pinging the Ghost server.
This means everything has worked well.
Close that command prompt by clicking `X`.

-   There should be another prompt behind that you can type into.
Type `exit` and hit Enter.

-   The computer will reboot, and will start un-sysprepping.
*You're done!*

## Deploying an image to a machine

### Prerequisites before deloying

-   The computer in question, of course, must have already been registered on
our Ghost system.

-   If the hard disk is encrypted with BitLocker, it must be completely
**decrypted** in advance.

-   Make sure the computer is currently connected to the Ghost network.
You can confirm this by going to `Machine Groups > Default` and checking the
icon next to the computer name in question.
If the icon has a little network wire running through the lower half, then
you're ready to roll.

### How to deploy an image

-   Go to `Tasks` on the left sidebar menu.

-   Click on `Deploy Windows 7 64bit` if that's the OS you want to deploy.

-   For `Target Machine Group / Machine`, browse and choose the right
computer(s).

-   Don't change anything else.
Hit `Execute`.
**Do not hit `Save`**.

-   That's all, assuming you don't run into errors.
All known and understood errors observed so far originate from not properly
following the guidelines in the section `Registering a Computer`.
Read through that section and check if you've followed every instruction
detailed there.
If it has, and you still run into errors, then you've discovered an unknown
error.
Please investigate, fix, and document.

-   When you're done with re-imaging, take care of all the things you normally
would after a fresh Windows install: Windows Updates, antivirus, etc.
