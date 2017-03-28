# Tutorial

In this 5-minute walkthrough, we will check out the HTTPS-enabled RepyV2 sandbox and run a simple example script. We assume you have Git and Python 2 installed. The repo that we will be cloning will be is "https://github.com/deepjaisia/repy_v2.git". Clone the appropriate repo and build it (see the Seattle Testbed BuildInstructions [https://seattle.poly.edu/wiki/RepyV2Tutorial] for details):

```
~$ git clone https://github.com/deepjaisia/repy_v2.git -b add_test_call_to_sandbox ./deep_repy_v2  
Cloning into './deep_repy_v2'...  
cd remote: Counting objects: 1796, done.  
remote: Compressing objects: 100% (7/7), done.  
remote: Total 1796 (delta 2), reused 0 (delta 0), pack-reused 1789  
Receiving objects: 100% (1796/1796), 1.37 MiB | 0 bytes/s, done.  
Resolving deltas: 100% (930/930), done.  
Checking connectivity... done.
```
 
After cloning the repository to the **"deep_repy_v2"** directory we move into the directory where the repo is cloned and build repy_v2.

```~$ cd ./deep_repy_v2  
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
```  

RUNNABLE now contains a sandbox runtime directory including the HTTPS extensions.

Next, we will create a file named **"get_seattle_webpage.r2py"** containing this simple example program:

```
log(httpsget("seattle.poly.edu", "/", "", False), "\n")
```

Run this script like so,

```
python repy.py restrictions.default get_seattle_webpage.r2py
```

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

## Advanced

After completing a 5-minute Tutorial we will now see how to use the call and host a server and use the server for fetching files from the server. A detailed step by step guide is provided here(https://github.com/deepjaisia/SandboxHttpsExtensions/blob/master/README.md) on how to setup the localhost server and how to create a self-signed certificate and start using the server for hosting files and use the call functionality to check the authenticity of the server. To run some of the examples below you might need to setup a "localhost" server and create a self-signed certificate.

## Examples

We are assuming that the user is acquainted on how to setup a "localhost" server and how to create self-signed certificate. If not refer the "Advanced" section of this documentation. All the examples stated here are taken as a reference from there itself only. We will be saving the examples with an extension ".r2py".

The command that we will be using to run the program is as follows, where "test_https.r2py" is the name of the file:

```
sudo python repy.py restrictions.default test_https.r2py
```

1.  

```
a, b, c = httpsget('localhost', '/test_https.py.zip', 'my_site.crt', True)      
log(a, b, c, "\n")      
file2 = openfile('asdf', True)      
file2.writeat(b,0)
```  

"/test_https.py.zip" is a file hosted by the "localhost" server.
"my_site.crt" is the name of the self-signed certificate and it has to be stored in the same directory as that of "/RUNNABLE".

2.

```  
a, b, c = httpsget('www.google.com', '/', '', False)      
log(a, b, c, "\n")      
file2 = openfile('asdf', True)      
file2.writeat(b,0)
```
