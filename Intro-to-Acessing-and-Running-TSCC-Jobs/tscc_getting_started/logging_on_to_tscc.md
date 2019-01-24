# Preparing to Work on TSCC: Accounts,  Connecting, and Logging on


[//]: # " Comment example "

[//]: # ( Comment2 )

In this exercise, you must use your SDSC or XSEDE account to log onto the TSCC cluster. This exercise verifies that your account is working, that your laptop is properly configured, and that your TSCC user environment is correctly setup.

<a name="top">In this document, we will show you how to:
    
* [Obtain your TSCC account](#obtain-your-tscc-account)
* [Using the Terminal Application to connect to TSCC](#term-app)
    - [Mac Users](#term-app-mac-users)
    - [Windows Users](#term-app-windows-users)
    - [Terminal Connection Example](#term-app-example)
    - [Getting Domain Name & Host Information](#term-app-dn-info)
* [Expand your knowledge using TSCC User guide](#tscc-user-guide)

Note: if you have any difficulties completing this task, please contact Institute staff at <consult@sdsc.edu>.

## <a name="obtain-your-tscc-account"></a>Obtain your tscc account:

To obtain a trial TSCC account see the TSCC Guide:  https://www.sdsc.edu/support/user_guides/tscc-quick-start.html

[Back to Top](#top)
<hr>

## <a name="term-app"></a>How to Use the Terminal Application:

The terminal applications are used to connect clients (you and your laptop) to remote computers (such as TSCC). See https://en.wikipedia.org/wiki/Secure_Shell for more information. The best known example of using a terminal is for logging in/connecting to a remote computer systems by users. This is called a client-server connection. Terminals are interactive: you type in a command to run, and the outputs are displayed on the terminal. Executing any command is done by typing it and pressing Enter.

<img src="ssh-login.png" alt="SSH Connection" width="300px" />

SSH provides a secure channel over any network in a client-server architecture. You will be using your laptop to access SDSC’s HPC systems using the secure shell command `ssh`. It is essential that you be able to run secure shell (or a similar connection tool) with X11 forwarding enabled, which allows you to have data encryption and to launch windows applications (e.g. plotting, or a browser).

*NOTE: The `hostname` for TSCC is `tscc.sdsc.edu`
*NOTE: The login node for TSCC is `tscc-login.sdsc.edu`

<img src="cluster-connection-diagram.png" alt="SSH Connection" width="350px" />

[Back to Top](#top)
<hr>

## <a name="term-app-mac-users"></a>Mac Users
For Mac users, the Terminal application is typically used for connections. This is done from the command line:

    ssh -X username@tscc.sdsc.edu

 If you are having trouble, try running `ssh` in verbose mode:

     ssh -v -X username@tscc.sdsc.edu


[Back to Top](#top)
<hr>

## <a name="term-app-windows-users"></a>Windows users
Windows users will need to run an X Server and an ssh-like client. [Cygwin](https://www.cygwin.com) provides a comprehensive Linux-like environment and an X server (Cygwin/X). Putty will also work for direct access to TSCC, it is only used for file transfers. For download and installation instructions, see:

* http://www.cygwin.com/
* http://x.cygwin.com/
* https://www.putty.org/

[Back to Top](#top)
<hr>

## <a name="term-app-example"></a>Example of a terminal connection:
using Secure Shell (ssh) commands that may be used to login to the TSCC:
```


ssh <your_username>@tscc-login.sdsc.edu
ssh -l <your_username> tscc-login.sdsc.edu

```

[Back to Top](#top)
<hr>

## <a name="term-app-dn-info"></a>Getting Domain Name & Host Information
Each machine you work with will have a `<domain_name>`,  `<hostname>` or `<ip_address>`. You can learn about IP addresses and domain names here: https://computer.howstuffworks.com/dns.htm.

* NOTE: The *DN* (domain name) for TSCC is    `tscc.sdsc.edu`

You may need to know the physical IP address of the cluster. To do this, run the `nslookup` command from the command line of your terminal window
```
[mthomas@newton:~] nslookup tscc-login.sdsc.edu
Server:		198.202.75.26
Address:	198.202.75.26#53

tscc-login.sdsc.edu	canonical name = tscc.sdsc.edu.
Name:	tscc.sdsc.edu
Address: 132.249.107.90
Name:	tscc.sdsc.edu
Address: 132.249.107.88

```

The IP address is the  line labeled "Address" and for TSCC there are two. YOu can log onto TSCC using either the DN or the IP addresses.

[Back to Top](#top)

<hr>

## TSCC User Guide

Please read the TSCC user guide and familiarize yourself with the hardware, file systems, batch job submission, compilers and modules. The guide can be found here:

https://www.sdsc.edu/support/user_guides/tscc.html
