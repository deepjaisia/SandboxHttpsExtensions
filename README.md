# SandboxHttpsExtensions
Augment the current sandbox API to allow HTTPS (or TLS/SSL) function calls from the Python libraries to be used in a secure, performance-isolated fashion inside of Repy.

List of Errors encountered during the addition of HTTPS Call to the sandbox.
Suppose we are making an API call for repy that will fetch "Weather News" for us and the name of the module/python_file that will do this for is "weather_report". The same module name will be used in the document for reference. 

1. Unsafe Call - getattr, hasattr, __import__(*github is removing some of the underscores for 'import') 

   Solution :-
   
   Follow the 'a' part of the solution and if that doesn't work look at 'b' part of the solution

   a. The first approach should be to 'import' the module that is giving you error to your "weather_report" module.

   Eg.)
  
   Unsafe call: ("Unsafe call '__import__' with args '('encodings.ascii',)', kwargs '{'fromlist': ['*'], 'level': 0}'",)
   
   In this case the module is 'ascii' in encodings. We 'import' this module to "weather_report". To do this add we will add the following line to "weather_report":
   
   import encodings.ascii
   
   or
   
   from encodings import ascii
   
   AVOID USING : 
   from encodings import * 
   ->Do not use this because it makes it difficult to determine where a particular function or attribute came from, and makes debugging difficult.
   
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
  
  
  
  
   
