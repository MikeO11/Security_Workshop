# Security Workshop

## Required Downloads
* Docker - https://docs.docker.com/v17.12/docker-for-mac/install/

* Firefox - https://www.mozilla.org/en-US/firefox/new/

* Postman - https://www.getpostman.com/downloads/

* Burp Suite Community - https://portswigger.net/burp/communitydownload

## Firefox Setup
(Pick one)


<details><summary>Option 1 - Use FoxyProxy</summary>
<p>
1. Open Firefox's Preferences


4. Search 'proxy' and change to settings to the following:

- HTTP Proxy: 127.0.0.1

- port 8080

- check 'Use this proxy server for all protocols'

13. Open burp → Select Temp project → Next → Start Burp

15. In Burp, click → Proxy → Intercept

17. In the Intercept section, turn Intercept off by clicking the button

</p>
</details>

## Docker Setup
Run the following in a terminal

`docker pull bkimminich/juice-shop`  
`docker run --rm -p 3000:3000 bkimminich/juice-shop`

Using Firefox, browse to http://localhost.:3000 (on macOS and Windows browse to http://192.168.99.100:3000 if you are using docker-machine instead of the native docker installation) **don't forget the '.' after local host** – This will force the localhost to use the same proxy settings as one would with an internet connection/adapter

## Burp Suite Intro


## Challenges

##### Freebie - Find the Scoreboard: http://localhost.:3000/#/score-board

------------



##### login as admin - SQL injection
Attack Page: http://localhost.:3000/#/login
Tools: None
<details><summary>Hint</summary>
<p>
Use a SQL injection on the UN field any anything in the password field

SQL injection cheetsheet: https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/
</p>
</details>

<details><summary>Answer</summary>
<p>
`admin' or '1'='1'--`

`' or 1=1/*`

`' or 1=1;--`

 

Logout and login as Bender

UN` ' or 1=1 and email like('%bender%');--`

PW: Anything will work
</p>
</details>

<details><summary>More Info</summary>
<p>
**Explanation**

The application is vulnerable to injection attacks (see OWASP Top 10: A1). Data entered by the user is integrated 1:1 in an SQL command that is otherwise constant. The statement can then be amended/extended as appropriate. The Administrator is the first to appear in the selection list and is therefore logged on.


**Lessons Learned**
User input should always be subject to a sanitizing or validation process on the server side before being processed. Because this case deals with an SQL injection, input data should be adjusted server side by interpretable SQL symbols and instructions. Established functions should be used for this not in-house developments (e.g. self-generated RegEx), because this cannot ensure that all cases have been taken into consideration.

**Prevention **

Use Prepared statements instead of dynamic SQL

- https://en.wikipedia.org/wiki/Prepared_statement

**News**

“Hackers sentenced for SQL injections that cost $300 million” 

https://nakedsecurity.sophos.com/2018/02/19/hackers-sentenced-for-sql-injections-that-cost-300-million/
</p>
</details>
------------

<details><summary>Hint</summary>
<p>

</p>
</details>

<details><summary>Answer</summary>
<p>

</p>
</details>

<details><summary>More Info</summary>
<p>

</p>
</details>
