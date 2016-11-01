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
