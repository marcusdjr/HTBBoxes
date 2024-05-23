# Shocker 😲

Lets begin with an NMAP scan to see what ports we see open.

    nmap -sC -sV 10.10.10.56

Results:
![image](https://github.com/marcusdjr/disney/assets/31329300/60980d16-1f76-488f-9304-3396c3f78469)

Ok so we see that we have port 80 open, if we go to the site we dont see much so lets try and spin up the good ole gobuster.

    gobuster dir -u http://10.10.10.56 -w /usr/share/dirb/wordlists/small.txt -o gobuster.txt -x sh  

So after running gobuster with find /cgi-bin/ but with a 403 status code, meaning we potentially don't have access to that sites contents.

![image](https://github.com/marcusdjr/disney/assets/31329300/83c339a3-4862-4d3e-859f-03220d36b2f1)

Lets to use gobuster on  http://10.10.10.56cgin-bin/ 

    gobuster dir -u http://10.10.10.56/cgi-bin/ -w /usr/share/dirb/wordlists/small.txt -o gobuster.txt -x sh 

Aha! /user.sh

![image](https://github.com/marcusdjr/disney/assets/31329300/1cb4233c-aa06-4b42-bfe3-3696fde11825)

When we go to /user.sh, it seems to download a file. If we run burpsuit and look at our request and responses we see the reason why is because it sees "Content-Type: text/x-sh" and doesnt know what to do with it so it just saves it as a file.

![image](https://github.com/marcusdjr/disney/assets/31329300/cc59f9d8-ff14-4c85-95c3-e21f5b89b210)

This box is called shocker 🤔, so lets try to see if its vulnerable to shell shock.

There we go it does seems to be vulnerabnle to Shell Shock.

![image](https://github.com/marcusdjr/disney/assets/31329300/fb2c8f9d-c489-488c-8b29-bf2286b2c428)

Before we proceed lets discuss what Shell Shock even is.

    Shellshock or Bash vulnerability was discovered in 2014. Upon its discovery, Shellshock posed a serious threat to the security of many systems worldwide.

    It affected Bash versions 1.0.3 through 4.3. Patches and updates were quickly released by operating system vendors and software developers to address the vulnerability.

    But many systems were still vulnerable at the time of the disclosure.

    As a result, the Shellshock vulnerability was widely exploited by attackers, and it is estimated that millions of systems were compromised.

How can it be exploited?
    Shellshock can be exploited in the following ways:

    An attacker can send a specially crafted environment variable to a Bash-based application. This environment variable will contain a function definition that will be executed by Bash (Bash is a Command language interpreter). The function definition can contain any arbitrary command, which will be executed with the privileges of the user running the Bash-based application.

    An attacker can create a malicious web page that contains a specially crafted environment variable. When a user visits the web page, the environment variable will be sent to the web server, which will then execute the arbitrary command.

    An attacker can send a specially crafted email message that contains a specially crafted environment variable. When the user opens the email message, the environment variable will be sent to the email client, which will then execute the arbitrary command.

If we go back to burp and send the GET request to Repeater and then modify the Cookie and replace it with a command such as ls, we see that we get a response 😲

![image](https://github.com/marcusdjr/disney/assets/31329300/ba981370-1f79-4b03-8916-ce6304fca816)

It seems as though we cannot modify we can't run ls without specifying the path first, lets fix that so that we can execute commands freely.

Lets go back to the Nmap script,