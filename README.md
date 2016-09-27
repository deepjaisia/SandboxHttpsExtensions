#SandboxHttpsExtensions

Augment the current sandbox API to allow HTTPS (or TLS/SSL) function calls from the Python libraries to be used in a secure, performance-isolated fashion inside of Repy.

"httpsget" Tutorial

Introduction

This guide gives an introduction about the function call as well as its usage, the function call can be used by the client to download files from a sever (Apache server in this case). The manual can also be used as a guide on how to create a function call for yourself in the RepyV2 Sandbox. 

It is assumed you have some basic knowledge of ports, self-signed certificate, certificate verification, "Apache" webserver. Lastly, a pretty basic understanding of Python Programming language is also required. The tutorial at  "http://www.python.org/doc/" might help you brush up with some basic concepts and get you started ASAP or you could always try "CodeAcademy" or "Learning Python the Hard Way" if you feel like you want to dwelve more into python and have time at hand. You do not have to be an expert at Python, but being able to write a simple program in Python is essential.


General Operation

Before we start to use the function call we need to install some things first. The list with details is shown below:-

-> Repy

You have to install Repy before you can start using the call itself. This link : "https://seattle.poly.edu/wiki/RepyV2Tutorial" provides gives a small tour to the Sandbox and the installation procedure as well. Once you are done with the setup of "Repy"
	
-> Apache Server

This is not necessary to install, but if you want to make any changes and test out the call on your localsystem installing this server is the best option.

You can install Apache Server using the command given below in the Ubuntu Terminal or download it from this link (https://httpd.apache.org/download.cgi) according to your Operating System.

'sudo apt-get install apache2'

You would also need to enable SSL on the Apache server once you've installed it. The new Apache Server is packaged with SSL Support so there is no need to dive into the configuration files like we had to in the early days. There are set of 3 commands that will help you enable SSL on Apache in no time. You could always follow this link (https://help.ubuntu.com/14.04/serverguide/httpd.html) if you need more info or you get lost.

sudo a2endmod ssl                
sudo a2ensite default-ssl        
sudo service apache2 restart


*****************************************************************************
*httpsget('server_name', 'method', 'webpage_within_server', trust_on_server)*
*****************************************************************************

##############
#>Doc string:#
##############

>Purpose:
Provides you with the content of the webpage that is stored at the address location of the webserver provided by the user. The content could be the source code of the webpage, a text document or even a zip file.

>Arguments:
->server_name : The name of the server needs to be specified. It could be 'https://google.com' or any other website. It can also be your 'localhost' server.

->method : It is used to specify which method do you want to use when fetching contents from server using this API Call. It can be 'GET' or 'PUT'. 'GET' is working for now but functionality for 'PUT' still needs to be tested out.

->webpage_within_server : This is used to give the name of the webpage that the user wants to get the information of. It can be left blank or you can input '/', if there is no specific webpage that the user is hunting for within the server.

->trust_on_server : It is a boolean value (True/False). If the user wants to trust the server and has the certificate for the particular server he/she is connecting then he/she can put 'True' in this field (Eg. Connecting to localhost or some known local servers). But if the server is unknown it is better to fill the field with 'False' (Eg. Connecting to servers on the internet like 'Google', 'Yahoo', 'YouTube').


>Exceptions:
-> NameError : If the boolean expression of "trust_on_server" field is inappropriate
-> SSLError : If the SSL certificate of the user is not genuine.


>Side Effects
None so far.

>Returns
1. The status of the server
2. The contents of the webpage that is requested within the webserver.

##########
#Example:#
##########

Here is a little example on how you can use the function call and use it to your benefit so you can download the contents of a website or you can even download a zip file from the server and save it on your computer.

>a, b = httpsget('localhost', 'GET', '/test_https.py.zip', True)
>log(a, b, "\n")
>file2 = openfile('asdf', True)
>file2.writeat(b,0)

The user can use this as a test code and save it with '.r2py' extension for usage.

Since we know that the API Call returns two values therefore that's the reason we have two variables "'a' and 'b'". 'a' gives us the status of the website and 'b' returns the content of the webpage that we have requested from the server 'localost' which is "/test_https.py.zip". And from this we can conclude that we are requesting a zip file from the server stored at that location. 

We are using 'GET' method over here. We are also giving a boolean value 'True' since we trust the server in this case. 

We are using the other Repy API Calls to write the contents that we fetched to a file.

---------------------------------------------------------------------------------------------------------------------------------------

List of Errors encountered during the addition of HTTPS Call to the sandbox.
Suppose we are making an API call for repy that will fetch "Weather News" for us and the name of that module/python_file that will do this for us is "weather_report". The same module name will be used in the document for reference. 

1. Unsafe Call - getattr, hasattr, __import__(*github is removing some of the underscores for 'import') 

   Solution :-
   
   Follow the 'a' part of the solution and if that doesn't work look at 'b' part of the solution

   a. The first approach should be to 'import' the module that is giving you error to your "weather_report" module.

   Eg.)
  
   Unsafe call: ("Unsafe call '__import__' with args '('encodings.ascii',)', kwargs '{'fromlist': ['*'], 'level': 0}'",)
   
   In this case the module is 'ascii' in encodings. We 'import' this module to "weather_report". To do this, we will add the following    line to "weather_report":
   
   import encodings.ascii
   
   or
   
   from encodings import ascii
   
   AVOID USING : 
   from encodings import * 
   ->Do not use this because it makes it difficult to determine where a particular function or attribute came from, and makes debugging difficult. This also imports all names except those beginning with an underscore (_)
   
   b. Add the following line of code to your own module
   
   ->module_name.getattr = getattr  (without the arrow)
   Where module_name = The name of the module which gives the getattr/hasattr/__import__ error.
   
   This technique is called monkey-patching. You could read about this over here (http://stackoverflow.com/questions/5626193/what-is-a-monkey-patch) or Wikipedia is the best place to look and understand. 

   Eg.)
   
   Unsafe call: ("Unsafe call 'getattr' with args '(<module '_ssl' from '/usr/lib/python2.7/lib-dynload/_ssl.x86_64-linux-gnu.so'>, 'OP_NO_COMPRESSION', 0)', kwargs '{}'",)

  The module name in the above case is "ssl" so we will add the following line of code to "weather_report" :-
  ssl.getattr = getattr 
  
  This same strategy could be adopted for any other "Unsafe Call" errors that are encountered while making your own API call for the 'repyV2' sandbox. There is no surity that it will work for sure and use it at your own risk.
  
References:
  
http://stackoverflow.com/questions/448271/what-is-init-py-for
https://github.com/SeattleTestbed/repy_v2/issues/92
http://mikegrouchy.com/blog/2012/05/be-pythonic-__init__py.html
http://stackoverflow.com/questions/1944625/what-is-the-relationship-between-getattr-and-getattr https://docs.python.org/3/tutorial/modules.html
https://github.com/burnash/gspread/issues/223
https://github.com/burnash/gspread/pull/228
https://www.python.org/dev/peps/pep-0476/#opting-out
