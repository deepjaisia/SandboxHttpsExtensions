# SandboxHttpsExtensions

Augment the current sandbox API to allow HTTPS (or TLS/SSL) function calls from the Python libraries to be used in a secure, performance-isolated fashion inside of Repy.

## "httpsget" Tutorial

### Introduction

This guide gives an introduction about the function call as well as its usage, the function call can be used by the client to download files from a sever (Apache server in this case). The manual can also be used as a guide on how to create a function call for yourself in the RepyV2 Sandbox. The commands used in this manual are created with reference to "UBUNTU 16.04" OS, but it can also be used with "Windows 10" OS since recently "Bash" support has been released for Windows.

It is assumed you have some basic knowledge of ports, self-signed certificate, certificate verification, "Apache" webserver. Lastly, a pretty basic understanding of Python Programming language is also required. The tutorial over [here]("http://www.python.org/doc/") might help you brush up with some basic concepts and get you started ASAP or you could always try "CodeAcademy" or "Learning Python the Hard Way" if you feel like you want to dwelve more into python and have time at hand. You do not have to be an expert at Python, but being able to write a simple program in Python is essential.

### General Operation

#### Cloning Repy Repository

We will be cloining the following "repy" repository for testing this call: "https://github.com/deepjaisia/repy_v2.git". You can refer "https://github.com/deepjaisia/SandboxHttpsExtensions/blob/master/5_min_walkthrough.md" for instructions on how clone and setup repy You have to install Repy before you can start using the call itself. This link : "https://seattle.poly.edu/wiki/RepyV2Tutorial" provides gives a small tour to the Sandbox and the installation procedure as well. Once you are done with the setup of "Repy".

Once we have cloned the "repy" repository and setup "repy" accordingly we can start using the call. But if we intend to use a self-signed certificate for a localhost server we can follow the procedure given below :-
   
#### 1. Apache Server

  * Installation      

     This is not necessary to install, but if you want to make any changes and test out the call on your localsystem installing this server is the best option.

     You can install Apache Server using the command given below in the Ubuntu Terminal or download it from this link (https://httpd.apache.org/download.cgi) according to your Operating System.

     ```
     sudo apt-get install apache2
     ```

  * Enabling SSL

     You would also need to enable SSL on the Apache server once you've installed it. The new Apache Server is packaged with SSL Support so there is no need to dive into the configuration files like we had to in the early days. There are set of 3 commands that will help you enable SSL on Apache in no time. You could always follow this link (https://help.ubuntu.com/14.04/serverguide/httpd.html) if you need more info or you get lost.

     ```
     sudo a2enmod ssl                
     sudo a2ensite default-ssl      
     sudo service apache2 restart
     ```

  * Adding files to Apache Server

     When you have enabled SSL on Apache, the first thing that the user would be doing is adding some webpages or files to your own server and test out if the server is actually functioning properly. In order for that to happen the user has to add the files to a location on the machine from where the server could access the files. One thing to note is that the Apache Server can serve some of the basic file types by default like : .html, .txt, .py, .zip and some more. For "apache2" the user can add the files to the following folder.  

     **"/var/www/html"**
  
  * Running Apache Server on a Custom Port
    
    Once you have setup Apache Server and we want to run Apache server on a custom port number apart from the defualt : '443'. We can change the port number by following the simple steps. We have to make changes to configuration files residing in the Apache server folder. The location of the two files is given as follows:
    
    **"/etc/apache2/ports.conf"**
    **"/etc/apache2/sites-enabled/default-ssl.conf"**
    
    Suppose we want to setup the server at "Port Number" : 10800, we just have to make changes to these two files. Open your command prompt and follow the commands below:
    
    ```
    cd /etc/apache2
    sudo gedit ports.conf
    ```
    
    Type in your password and now a windows will open up where you can edit the file "ports.conf". There will be a section as follows:
    
    ```
    <IfModule ssl_module>
	   Listen 443
    </IfModule>
    ```
    
    **Change it to:**
    
    ```
    <IfModule ssl_module>
	   Listen 10800
    </IfModule>
    ```
    
    Now just save the file and exit the editor. We have successfully changed the port to which the server will listen for ssl services and half-way through the services.
    
    We have to make changes to the next file in order to complete the process. Follow the commands belows to make changes to the next file.
    
    ```
    cd /etc/apache2/sites-enabled/
    sudo gedit default-ssl.conf
    ```
    
    Type in your password and now a windows will open up where you can edit the file "default-ssl.conf". There will be a section as follows:
    
    ```
    <IfModule mod_ssl.c>
	   <VirtualHost _default_:443>
		  ServerAdmin webmaster@localhost 
        ...........
    ```
    
    **Change it to:**
    
    ```
    <IfModule mod_ssl.c>
	   <VirtualHost _default_:10800>
		  ServerAdmin webmaster@localhost 
        ...........
    ```
    
    Now again save and exit the file editor. We are now completely setup.
    
    We just have to restart the "Apache" server to make these changes takes place. Just run the following command to perform the operation.
    
    ```
    sudo service apache2 restart
    ```
    
    Hooray, we are all set and now the Apache Server on the newly assigned port: 10800 setup by us. You can check it by running the following command in the terminal.
    
    ```
    sudo netstat -tulpn | grep apache2
    ```
    You will get a result something like this:
    
    ```
    tcp6       0      0 :::80                   :::*                    LISTEN      4718/apache2    
    tcp6       0      0 :::10800                :::*                    LISTEN      4718/apache2 
    ```

  * Creating Self-Signed Certificate for your Apache Server

     Aapche comes with it's own certificate and key when you enable 'SSL' on Apache. But it doesn't stop the user from playing        with their own Self-Signed Certificates for the server. This part of guide will help you on how you can create a Self-Signed Certificate and use that for your Apache Server. We are going to use OpenSSL from the terminal window to create the certificate and then some minor modifications to the configuration file of the Server will get you up and running in no time.

     For the certificate, just run the following command in the terminal window. You will come across some fields that you        have to fill out.

     Note : If you are prompter to enter a password, remember the certificate password as it will be used everytime you restart the service for the server.

     ```
     openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout my_site.key -out my_site.crt
     ```

     This command will create a two files with extenstions ".crt" and ".key "which will contain the 'certificate' and the 'key' respectively. Explanation of the different attributes used in the command is given below:
     
     **-x509** : This field outputs a x509 structure.
     
     **-newkey** : This specifies that by using which algorithm we are going to create the key of the specified size. In this case it is RSA of 4096 bits.

     **-keyout** : This specifies the name of the key that would be created. It creates a '.key' object but it can also create '.pem' object.

     **-out** : This specifies the name of the certificate that would be created. It can be a '.crt' object also but for this case we are going to take a '.pem' object.

     **-days** : This specifies the number of days for which the certificate will be valid for. It is 365 days in this case.
 
     We are now almost there, we have created a Self-Signed Certifcate and now all that we have to do is make Apache Server find this certificate and use this as default. Firstly we have to find the configuration file and then edit it. Mostly for Ubuntu users you could find the configuration file at this location : '/etc/apache2/sites-enabled/default-ssl.conf'. 

     'default-ssl.conf' is the configuration file that we are looking for. Upon opening up the file in a text editor we have to search for the following lines in the configuration file :  

     **SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem**       
     **SSLCertificateKeyFile   /etc/ssl/private/ssl-cert-snakeoil.key**

     We replace the location of the SSLCertificateFile and the SSLCertificateKeyFile to the location where we saved our own Self-Signed Certificate and Key. The location for the 'Self-Signed Certificate' and 'Key File' will be the same directory where the user ran their "OpenSSL" command in the terminal. The user can check their current directory by typing 'pwd' in the terminal. The location for me in this case was 'Home' so I changed the location for the SSLCertificateFile and SSLCertificateKeyFile as follows:        

     **SSLCertificateFile      /home/frostbyte/my_site.crt**       
     **SSLCertificateKeyFile   /home/frostbyte/my_site.key**       

     After changing the locations for both the files, just save the configuration file. We have successfully now configured SSL on Apache Server using our Self-Signed Certificate and Key.

#### 2. Cloning Repository

   If the user wants to use the call and make some testing they could clone the repository on their machine and then test it out. The user can use the following command to Clone the repository to their machine. First thing to check is the current directory that the user is working and make a new directory within the current one where the "RepyV2" will be stored. The new directory that I would be making in this example would be "RepyTesting". 
   
   ```
   mkdir RepyTesting
   ```
   
   After making a new directory we would move to the new directory where we would clone the repository.
   
   ```
   cd RepyTesting
   ```
   
   Before using the command to clone the repo make sure you have 'git' installed on your machine. To install git the user can use the command given below.
   
   ```
   sudo apt-get install git
   ```
   
   After we have 'git' on our machine we could go about and clone the repo.
   
   ```
   git clone -b name_of_branch https://github.com/username/name_of_repo.git
   ```
   
   The user can skip '-b name_of_branch' attribute to the git command if he/she wants to clone the "Master" branch. 
   
   name_of_branch : Specific branch that the user wants to clone.    
   username : User Name of the github account from where the user wants to clone the repo.  
   name_of_repo : Name of the main repository.  
   
   The command in this case being where you want to download "Repy" with the 'httpsget' call is as follows.
   
   ```
   git clone -b add_test_call_to_sandbox https://github.com/deepjaisia/repy_v2.git
   ```

### [httpsget('server_name', 'method', 'webpage_within_server', 'name_of_cert', trust_on_server)]

### Doc String:

#### 1. Purpose:      

  Provides you with the content of the webpage that is stored at the address location of the webserver provided by the user. The content could be the source code of the webpage, a text document or even a zip file.

#### 2. Arguments:      

  * server_name : The name of the server needs to be specified. It could be 'https://google.com' or any other website. It can also be your 'localhost' server.

  * webpage_within_server : This is used to give the name of the webpage that the user wants to get the information of. It can be left blank or you can input '/', if there is no specific webpage that the user is hunting for within the server.
  
  * name_of_cert : This field needs to be filled by the user and the he/she needs to specify the name of self-signed certificate they have used for their local server. Prequisite to this field requires the user to copy and save the self-signed certificate to the same folder from where they are running the "Repy" code.

  * trust_on_server : It is a boolean value (True/False). If the user wants to trust the server and has the certificate for the particular server he/she is connecting then he/she can put 'True' in this field (Eg. Connecting to localhost or some known local servers). But if the server is unknown it is better to fill the field with 'False' (Eg. Connecting to servers   on the internet like 'Google', 'Yahoo', 'YouTube').


#### 3. Exceptions:      

  * NameError : If the boolean expression of "trust_on_server" field is inappropriate      
  * SSLError : If the SSL certificate with the user is not genuine.
  * CertiError : If "name_of_cert" field is left blank when "trust_on_server" field is "True".

#### 4. Side Effects      

  * None recorded so far.

#### 5. Returns      
 
  * The status of the server      
  * The contents of the webpage that is requested within the webserver.
  * Headers of the webpage

## Example:

Below are some examples on how to use the httpsget and start downloading some files or content of a webpage from the server to your own folder. Before the user use the function call there are small prerequisite that needs to be fulfilled for proper functioning of the "localhost" server. The prereqs are listed below as follows :-

* 
    If the user intend to use 'localhost' server for testing purposes he/she needs to follow the steps that are listed above on how to       setup a local server for testing purposes. After the server is completely setup and the user is ready to perform some tests, he/she     needs to **copy and save** the certificate that they are using in the same directory they are running "Repy" from which in most         cases would be the "RUNNABLE" directory. The gist is just save the certificate to the same directory that you are using for the         Apache Server to the same directory. 

    If the user is not using self-signed certificate then they could locate the default certificate that the Aapache Server is using for     HTTPS. The location for the certificate can be found from the path defined in the "default-ssl.conf" file. The location for this         file will be mainly "/etc/apache2/sites-enabled/default-ssl.conf" for most cases for Apache Server. We have to now search for the       line of code within the "default-ssl" file to find the location of the certificate listed below: 

    **SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem**       
    **SSLCertificateKeyFile   /etc/ssl/private/ssl-cert-snakeoil.key**
     
    From the above lines we know that the location for the certificate is "/etc/ssl/certs/ssl-cert-snakeoil.pem". Once we know the           location for the "Certificate" we can copy the certificate to our destination folder. To perform the copy operation we can copy         command from the terminal or a simple copy and paste from the GUI would also do. Anyways I'll provide the terminal command for the       people who would still love to use the terminal for copying the contents. Firstly we have to navigate to the source directory for       the "Certificate" from the terminal and the location for the certificate is showed above and this can be done using the command         shown below. 
     
    ```
    cd /etc/ssl/certs/ssl-cert-snakeoil.pem
    ```
     
    After navigating to the directory use the command below to copy the certificate.
     
    ```
    sudo cp ssl-cert-snakeoil.pem /your_repy_runnable_directory
    ```

Below are some the examples on how you could use the call to implement simple programs that allow the user to download files that the 'localhost' server serves or the get the web content of a webpage. 
     
#### 1. Here is a little example on how you can use the function call and use it to your benefit so you can download the contents of a website or you can even download a zip file from the server and save it on your computer.

  ```  
  a, b, c = httpsget('localhost', '/test_https.py.zip', 'my_site.crt', True)      
  log(a, b, c, "\n")      
  file2 = openfile('asdf', True)      
  file2.writeat(b,0)
  ```  

  The user can use this as a test code and save it with '.r2py' extension for usage. To run this program the user can type     the following line in the terminal. The user should run the program in 'sudo' mode if "Permission Denied" error pops up     during running the code. If this doesn't help it might be an restriction within "Repy" Sandbox itself. We are going to       assume that the user saves the file with the name "test_https.r2py".

  ```
  sudo python repy.py restrictions.default test_https.r2py
  ```

  Since we know that the API Call returns two values therefore that's the reason we have two variables "'a' and 'b'". 'a'     gives us the status of the website and 'b' returns the content of the webpage that we have requested from the server         'localost' which is "/test_https.py.zip". And from this we can conclude that we are requesting a zip file from the server   stored at that location. 

  We are using 'GET' method over here. We are also giving a boolean value 'True' since we trust the server in this case. Use   'False' in the last field if you are connecting to some unknown server.

  We are using the other Repy API Calls to write the contents that we fetched from the server to a file.

#### 2. This is an another example where if the user wants to just want to see the HTML content of any webpage on the internet.

  ```  
  a, b, c = httpsget('www.google.com', '/', '', False)      
  log(a, b, c, "\n")      
  file2 = openfile('asdf', True)      
  file2.writeat(b,0)
  ```  

  The user can use this as a test code and save it with '.r2py' extension for usage. To run this program the user can type     the following line in the terminal. The user should run the program in 'sudo' mode if "Permission Denied" error pops up     during running the code. If this doesn't help it might be an restriction within "Repy" Sandbox itself. We are going to       assume that the user saves the file with the name "test_https.r2py".

  ```
  sudo python repy.py restrictions.default test_https.r2py
  ```

  This 'Repy' program returns the content of the webpage 'www.google.com' and then displays the content of the webpage and     also write the same output to the file which is named 'asdf'. See how we put 'False' in the last field of the 'httpsget'     call in the first line of the program, that is because we don't know if the server on the internet is certified or not.

---------------------------------------------------------------------------------------------------------------------------------------

## Help Guide

This is a list of Errors I encountered during the addition of HTTPS Call to the sandbox. This guide might help the user get over some of the similar kind of errors that the he/she might be experiencing during addition of an API Call to "Repy" Sandbox. To make things simpler we are going to assume that the user is making an API call for Repy that will fetch "Weather News" so the name of the user's own module/python_file that the user is working on is named "weather_report". The same module name will be used in the document for reference. 

1. Unsafe Call - getattr, hasattr, __import__(*github is removing some of the underscores for 'import') 

   Solution :-
   
   Follow the 'a' part and if that doesn't work look at 'b' part of the solution to remove the errors.

   a. The first approach should be to 'import' the module that is giving you error to your "weather_report" file.

     Eg.)
  
       Unsafe call: ("Unsafe call '__import__' with args '('encodings.ascii',)', kwargs '{'fromlist': ['*'], 'level': 0}'",)
   
       In this case the module is 'ascii' within the 'encodings' module. We 'import' this module to "weather_report" file.          To alleviate this error, we will add the following line to "weather_report":
   
       ```
       import encodings.ascii
       ```
   
       or
   
       ```
       from encodings import ascii
       ```
   
       **AVOID USING :** 
   
       ```
       from abc import *
       ```
       
       Where 'abc' is any module that the user wants to import.
   
       Do not use this kind of import for any kind of module because it makes it difficult to determine where a particular          function or attribute came from, and makes debugging difficult. This also imports all names except those                    beginning with an underscore (_)
   
   b. Add the following line of code to your own module
   
     ```
     module_name.getattr = getattr
     ```
   
     Where module_name = The name of the module which gives the getattr/hasattr/__import__ error.
   
     This technique is called monkey-patching. You could read about this over here                                                (http://stackoverflow.com/questions/5626193/what-is-a-    monkey-patch) or Wikipedia is the best place to look and          understand. 

     Eg.)
   
       Unsafe call: ("Unsafe call 'getattr' with args '(<module '_ssl' from '/usr/lib/python2.7/lib-dynload/_ssl.x86_64-            linux-gnu.so'>,'OP_NO_COMPRESSION', 0)', kwargs '{}'",)

       The module name in the above case is "ssl" so we will add the following line of code to "weather_report" file:-
     
       ```
       ssl.getattr = getattr
       ``` 
  
       This same strategy could be adopted for any other "Unsafe Call" errors that are encountered while making your own API        call for the 'repyV2' sandbox. There is no surity that it will work for sure and use it at your own risk.
  
## References:
  
http://stackoverflow.com/questions/448271/what-is-init-py-for  
https://github.com/SeattleTestbed/repy_v2/issues/92  
http://mikegrouchy.com/blog/2012/05/be-pythonic-__init__py.html  
http://stackoverflow.com/questions/1944625/what-is-the-relationship-between-getattr-and-getattr   https://docs.python.org/3/tutorial/modules.html  
https://github.com/burnash/gspread/issues/223  
https://github.com/burnash/gspread/pull/228  
https://www.python.org/dev/peps/pep-0476/#opting-out  
