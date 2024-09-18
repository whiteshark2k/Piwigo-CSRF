## In Piwigo v14.5.0, I found CSRF Vulnerability in function Change Name Album </br>
Piwigo v14.5.0: [https://github.com/Piwigo/Piwigo] </br>

In the Edit album function, the application uses Anti-CSRF token but does not handle it correctly, resulting in the fact that when deleting this field, the request to change information can still be made.</br>

## Reproduce bug:</br>
Step 1: Login with admin account and go to Edit album Screen.</br>
![Alt text](test1.jpg)
We can see Album name is test1 </br>

Step 2: Request Edit Album Original
![Alt text](test5.jpg)

Step 3: Payload attack CSRF change Album name to test0123 (pwd_token field removed, pwd_token is the anti-csrf field deployed by the application)
![Alt text](test2.jpg)

Step 4: Request Edit Album CSRF to test0123 after victim clicks on trap page
![Alt text](test3.jpg)

Step 5: Album name changed to test0123 successfully
![Alt text](test4.jpg)

We can go into the processing code in the pwg.categories.php file </br>
![Alt text](test6.jpg)
It can be seen that in this function, from line 888->891, the application only checks the pwg_token if it exists, in case the pwg_token does not exist, it will be completely ignored.
