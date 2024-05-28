# Bashed

As always lets start with an Nmap scan.

    nmap -sC -sV 10.10.10.68
![image](https://github.com/marcusdjr/HTBBoxes/assets/31329300/bd336561-3d24-496a-8eec-a75eccd1c6c3)


We see that port 80 is open, lets visit the web page to see what we see.

![image](https://github.com/marcusdjr/HTBBoxes/assets/31329300/8bc81bcc-cc18-46be-86a6-30a7e1ceaf40)

The site also refrences a GitHub page

https://github.com/Arrexel/phpbash

Ok.. good to know, lets get in a gobuster scan and see what we find.

    gobuster dir -u http://10.10.10.68 -w /usr/share/dirb/wordlists/small.txt -o gobuster.txt -x sh  

    Interesting! we found a few directories.

![image](https://github.com/marcusdjr/HTBBoxes/assets/31329300/3334a6e8-468f-4d53-9db8-9559c5b81b16)

Lets take a look at their contents..

If we navigate to /dev we see a phpbash.min.php lets give that a click and see whats there.

![image](https://github.com/marcusdjr/HTBBoxes/assets/31329300/0a5d6bf5-84a3-4aae-a22c-606452049d75)

Oh wow! A terminal..

![image](https://github.com/marcusdjr/HTBBoxes/assets/31329300/32b091dd-4f2c-4d1d-8954-2dd0c1e83f6e)

Now we do some priv esc and get that flag!