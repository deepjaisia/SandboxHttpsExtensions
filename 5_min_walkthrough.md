#Tutorial

In this 5-minute walkthrough, we will check out the HTTPS-enabled RepyV2 sandbox and run a simple example script. We assume you have Git and Python 2 installed. Clone the appropriate repo and build it (see the Seattle Testbed BuildInstructions [https://seattle.poly.edu/wiki/RepyV2Tutorial] for details):

```
~$ git clone https://github.com/deepjaisia/repy_v2.git -b add_test_call_to_sandbox ./deep_repy_v2  
Cloning into './deep_repy_v2'...  
cd remote: Counting objects: 1796, done.  
remote: Compressing objects: 100% (7/7), done.  
remote: Total 1796 (delta 2), reused 0 (delta 0), pack-reused 1789  
Receiving objects: 100% (1796/1796), 1.37 MiB | 0 bytes/s, done.  
Resolving deltas: 100% (930/930), done.  
Checking connectivity... done.  
~$ cd ./deep_repy_v2  
~/deep_repy_v2$ cd scripts/  
~/deep_repy_v2/scripts$ python initialize.py   
Checking out repo from https://github.com/SeattleTestbed/seattlelib_v2 ...  
pyDone!  
Checking out repo from https://github.com/SeattleTestbed/portability ...  
thon Done!  
Checking out repo from https://github.com/SeattleTestbed/seash ...  
bzuuDone!  
Checking out repo from https://github.com/SeattleTestbed/affix ...  
  Done!  
Checking out repo from https://github.com/SeattleTestbed/common ...  
Done!  
Checking out repo from https://github.com/SeattleTestbed/utf ...  
Done!  
~/deep_repy_v2/scripts$ python build.py   
Building into /home/albert/deep_repy_v2/RUNNABLE  
Done building!  
~/deep_repy_v2/scripts$ cd ../RUNNABLE/  
~/deep_repy_v2/RUNNABLE$
```  

RUNNABLE now contains a sandbox runtime directory including the HTTPS extensions.

Next, create a file named get_seattle_webpage.r2py containing this simple example program:

```log(httpsget("seattle.poly.edu", "GET", "/", "", False), "\n")```

Run this script like so,

```~/deep_repy_v2/RUNNABLE$ python repy.py restrictions.default get_seattle_webpage.r2py```

Your output should be similar to this:

```
302 <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">  
<html><head>  
<title>302 Found</title>  
</head><body>  
<h1>Found</h1>  
<p>The document has moved <a href="https://seattle.poly.edu/html/">here</a>.</p>  
<hr>  
<address>Apache/2.2.8 (Ubuntu) DAV/2 SVN/1.4.6 mod_python/3.3.1 Python/2.5.2 PHP/5.2.4-2ubuntu5.27 with Suhosin-Patch mod_ssl/2.2.8 OpenSSL/0.9.8g Server at seattle.poly.edu Port 443</address>  
</body></html>
```

Congratulations, you've just written your first program to use the HTTPS-enabled sandbox! This concludes the 5-minute walkthrough.

##Advanced

After completing a 5-minute Tutorial we will now see how to use the call and host a server and use the server for fetching files from the server. A lot of reference would be used from: https://github.com/deepjaisia/SandboxHttpsExtensions/blob/master/README.md, so please be sure to check this manual out to get a more elaborate explanation of the steps involved in this tutorial. The steps involved are numbered as follows:

- Hosting an Apache Server
We need to set up a localhost server in order to save files to the localhost server and then server files to other clients or just use it for your own testing purposes. The server for now can serve variety of files supported by Apache itself. 

Check the link: https://github.com/deepjaisia/SandboxHttpsExtensions/blob/master/README.md for a more elaborate explanation on how to set up the server and host files.

#Knowing The Call

   Now after completing the 5-Minute walkthrough successfully there might be some things that the user might be curious. We'll first go    through some of the parameters of the function call one by one below :-
    
   ##[httpsget('server_name', 'method', 'webpage_within_server', 'name_of_cert', trust_on_server)]      

   * server_name : The name of the server needs to be specified. It could be 'https://google.com' or any other website. It can also be your 'localhost' server.

   * method : It is used to specify which method do you want to use when fetching contents from server using this API Call. It can be      'GET' or 'PUT'. 'GET' is working for now but functionality for 'PUT' still needs to be tested out.

   * webpage_within_server : This is used to give the name of the webpage that the user wants to get the information of. It can be left    blank or you can input '/', if there is no specific webpage that the user is hunting for within the server.
  
   * name_of_cert : This field needs to be filled by the user and the he/she needs to specify the name of self-signed certificate they      have used for their local server. Prequisite to this field requires the user to copy and save the self-signed certificate to the same folder from where they are running the "Repy" code.

   * trust_on_server : It is a boolean value (True/False). If the user wants to trust the server and has the certificate for the         particular server he/she is connecting then he/she can put 'True' in this field (Eg. Connecting to localhost or some known local     servers). But if the server is unknown it is better to fill the field with 'False' (Eg. Connecting to servers on the internet like     'Google', 'Yahoo', 'YouTube').
