# SandboxHttpsExtensions
Augment the current sandbox API to allow HTTPS (or TLS/SSL) function calls from the Python libraries to be used in a secure, performance-isolated fashion inside of Repy.

"httpsget" Tutorial

Introduction

This guide gives an introduction about the function call as well as its usage, the function call can be used by the client to download files from a sever (Apache server in this case). The manual can also be used as a guide on how to create a function call for yourself in the RepyV2 Sandbox. 

It is assumed you have some basic knowledge of ports, self-signed certificate, certificate verification, "Apache" webserver. Lastly, a pretty basic understanding of Python Programming language is also required. The tutorial at  "http://www.python.org/doc/" might help you brush up with some basic concepts and get you started ASAP or you could always try "CodeAcademy" or "Learning Python the Hard Way" if you feel like you want to dwelve more into python and have time at hand. You do not have to be an expert at Python, but being able to write a simple program in Python is essential.


General Operation

Before we start to use the function call we need to install some things first. The list with details is shown below:-

1. Repy 

You have to install Repy before you can start using the call itself. This link : "https://seattle.poly.edu/wiki/RepyV2Tutorial" provides gives a small tour to the Sandbox and the installation procedure as well. Once you are done with the setup of "Repy"
	
2. Apache Server

This is not necessary to install, but if you want to make any changes and test out the call on your localsystem installing this server is the best option. 

You can install Apache Server using the command given below in the Ubuntu Terminal or download it from this link (https://httpd.apache.org/download.cgi) according to your Operating System.

'sudo apt-get install apache2'

You would also need to enable SSL on the Apache server once you've installed it. The new Apache Server is packaged with SSL Support so there is no need to dive into the configuration files like we had to in the early days. There are set of 3 commands that will help you enable SSL on Apache in no time. You could always follow this link (https://help.ubuntu.com/14.04/serverguide/httpd.html) if you need more info or you get lost.

sudo a2endmod ssl                
sudo a2ensite default-ssl        
sudo service apache2 restart     

List of Errors encountered during the addition of HTTPS Call to the sandbox.
Suppose we are making an API call for repy that will fetch "Weather News" for us and the name of that module/python_file that will do this for us is "weather_report". The same module name will be used in the document for reference. 

1. Unsafe Call - getattr, hasattr, __import__(*github is removing some of the underscores for 'import') 

   Solution :-
   
   Follow the 'a' part of the solution and if that doesn't work look at 'b' part of the solution

   a. The first approach should be to 'import' the module that is giving you error to your "weather_report" module.

   Eg.)
  
   Unsafe call: ("Unsafe call '__import__' with args '('encodings.ascii',)', kwargs '{'fromlist': ['*'], 'level': 0}'",)
   
   In this case the module is 'ascii' in encodings. We 'import' this module to "weather_report". To do this, we will add the following line to "weather_report":
   
   import encodings.ascii
   
   or
   
   from encodings import ascii
   
   AVOID USING : 
   from encodings import * 
   ->Do not use this because it makes it difficult to determine where a particular function or attribute came from, and makes debugging difficult. This also imports all names except those beginning with an underscore (_)
   
   b. Add the following line of code to your own module  
   ->module_name.getattr = getattr  (without the arrow)
   Where module_name = The name of the module which gives the getattr/hasattr/__import__ error.

   Eg.)
   
   Unsafe call: ("Unsafe call 'getattr' with args '(<module '_ssl' from '/usr/lib/python2.7/lib-dynload/_ssl.x86_64-linux-gnu.so'>, 'OP_NO_COMPRESSION', 0)', kwargs '{}'",)

  The module name in the above case is "ssl" so we will add the following line of code to "weather_report" :-
  ssl.getattr = getattr 
  
  This same strategy could be adopted for any other "Unsafe Call" errors that are encountered while making your own API call for the 'repyV2' sandbox. There is no surity that it will work for sure and use it at your own risk.
  
  References:
  
  http://stackoverflow.com/questions/448271/what-is-init-py-for
  https://github.com/SeattleTestbed/repy_v2/issues/92
  http://mikegrouchy.com/blog/2012/05/be-pythonic-__init__py.html
  http://stackoverflow.com/questions/1944625/what-is-the-relationship-between-getattr-and-getattr
  https://docs.python.org/3/tutorial/modules.html
  https://github.com/burnash/gspread/issues/223
  https://github.com/burnash/gspread/pull/228
  https://www.python.org/dev/peps/pep-0476/#opting-out
  
  
  
  
   
