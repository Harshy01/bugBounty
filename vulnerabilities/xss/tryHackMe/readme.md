# Cross-site Scripting

Author: [tryhackme](https://tryhackme.com/p/tryhackme)
Link: [Cross-site Scripting](https://tryhackme.com/room/xss)

##  Room Brief
    Prerequisites:
    it's good to have knowledge of javascript as it will have you learn about how xss actually works and how the js is executed on the client's web-browser
    Cross-site Scripting: is classified as an injection attack where malicious javascript gets injected into a web application with the intention of executed on other users.

##  XSS Payloads
    What is a payload?
    In XSS payload is javascript code we wish to be executed on target's computer, there are two parts to it: intention and modification.
### 1) intention: 
    is what we wish the javascript to actually do.
    
    Example:
#### Proof Of Concept
     This is basically the simplest js payload that we use to demonstrate that xss exists on that website and can be achieved.
     this is often done by using alert box to popup with a string of text.
     
     ```
     <script>alert("xss");</script>
     ```

#### Session Steling
     Details of a user session, such as login tokens, are often stored in cookies in target machine, the below js takes the cookies and sends it to the attacker website using base64 encoding  


### 2) modification: 
    is the changes to the code that we need to make to make it execute as every scenario is different.
    
    
## Task:3
## Task:4
   ### HomePage 
    ![Home Webpage](./../home_website.png "Website Home page")
 
   ### login Page
    ![Login Webpage](./../home_website.png "Website Login page")
   ### post Page
    ![Post Webpage](./../home_website.png "Website Post page")

   ### register Page
    ![Register Webpage](./../home_website.png "Website Register page")
     
   ### user Page
    ![User Webpage](./../home_website.png "Website User page")
    
   ### terms-and-conditions Page
    ![Terms and Conditions Webpage](./../home_website.png "Website Terms and Conditions page")
          
