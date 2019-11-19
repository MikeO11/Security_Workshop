# Security Workshop

## Required Downloads
* Docker - https://docs.docker.com/v17.12/docker-for-mac/install/

* Firefox - https://www.mozilla.org/en-US/firefox/new/

* Postman - https://www.getpostman.com/downloads/

* Burp Suite Community - https://portswigger.net/burp/communitydownload

## Firefox Setup

<details><summary>Change Firefox network Settings</summary>
<p>
1. Open Firefox's Preferences 
 
 ![image](https://user-images.githubusercontent.com/40209642/69169586-d826c800-0ab5-11ea-94df-35483161338c.png)
 
2. Search 'proxy' and change to settings to the following:

- HTTP Proxy: 127.0.0.1

- port 8080

- check 'Use this proxy server for all protocols'

![image](https://user-images.githubusercontent.com/40209642/69174413-0230b800-0abf-11ea-99c4-6d7294394dd0.png)


3. Open burp → Select Temp project → Next → Start Burp

4. In Burp, click → Proxy → Intercept

5. In the Intercept section, turn Intercept off by clicking the button

![image](https://user-images.githubusercontent.com/40209642/69174547-46bc5380-0abf-11ea-87c6-d7069e0b9204.png)

</p>
</details>
Foxy Proxy can also be used instead of editing the network settings - https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/

## Docker Setup

Run the following in a terminal 

`docker pull bkimminich/juice-shop` <br/>
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
Possible answers

admin' or '1'='1'--

' or 1=1/*

' or 1=1;--

Logout and login as Bender

UN: ' or 1=1 and email like('%bender%');--

PW: Anything will work
</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Explanation**

The application is vulnerable to injection attacks (see OWASP Top 10: A1). Data entered by the user is integrated 1:1 in an SQL command that is otherwise constant. The statement can then be amended/extended as appropriate. The Administrator is the first to appear in the selection list and is therefore logged on.


**Lessons Learned**

User input should always be subject to a sanitizing or validation process on the server side before being processed. Because this case deals with an SQL injection, input data should be adjusted server side by interpretable SQL symbols and instructions. Established functions should be used for this not in-house developments (e.g. self-generated RegEx), because this cannot ensure that all cases have been taken into consideration.

**Prevention**

Use Prepared statements instead of dynamic SQL

- https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html#remediation

- https://en.wikipedia.org/wiki/Prepared_statement

**News**

“Hackers sentenced for SQL injections that cost $300 million” 

https://nakedsecurity.sophos.com/2018/02/19/hackers-sentenced-for-sql-injections-that-cost-300-million/
</p>
</details>

------------
##### Perform a DOM XSS attack with <iframe src="javascript:alert(`xss`)">
Attack Page: http://localhost.:3000/#/search
 
Tools: None
<details><summary>Hint</summary>
<p>
The code needs to to be pasted into an input field
</p>
</details>

<details><summary>Answer</summary>
<p>
Click search and paste <iframe src="javascript:alert(`xss`)"> in search field

Press Enter/Search
</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Explanation**

The application is vulnerable for DOM XSS because user input is returned 1:1 by the application just as the user entered it.

**Lessons learned**

'How To Prevent DOM XSS

The primary rule that you must follow to prevent DOM XSS is: sanitize all untrusted data, even if it is only used in client-side scripts. If you have to use user input on your page, always use it in the text context, never as HTML tags or any other potential code.

Avoid methods such as `document.innerHTML` and instead use safer functions, for example, `document.innerText` and `document.textContent`. If you can, entirely avoid using user input, especially if it affects DOM elements such as the `document.url`, the `document.location`, or the `document.referrer`.

Also, keep in mind that DOM XSS and other types of XSS are not mutually exclusive. Your application can be vulnerable to both reflected/stored XSS and DOM XSS. The good news is that if user input is handled properly at the foundation level (e.g. your framework), you should be able to mitigate all XSS vulnerabilities.'

https://www.acunetix.com/blog/web-security-zone/how-to-prevent-dom-based-cross-site-scripting/

**News**

"Hackers exploit cross-site scripting vulnerability in WordPress plugin to attack Mailgun site” https://www.computing.co.uk/ctg/news/3074027/hackers-exploit-cross-site-scripting-vulnerability-in-wordpress-plugin-to-attack-mailgun-site
</p>
</details>


------------

##### Give a devastating zero-star feedback to the store. 
Requirements:
- Use the admin account (perfrom the SQLi)
- Attack page page: http://localhost.:3000/#/contact
- Tools: Burp (turn intercept on)

<details><summary>Hint</summary>
<p>
1. Navigate to the contact page
 
2. Open Burp and turn 'Intercept' to ON

3. view the request

</p>
</details>

<details><summary>Answer</summary>
<p>
Change request from:

`{"UserId":1,"captchaId":21,"captcha":"9","comment":"this juice is grossssss","rating":1}`

TO (rating was changed from 1 to 0)

`{"UserId":1,"captchaId":21,"captcha":"9","comment":"this juice is grossssss","rating":0}`
 
</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Lessons Learned:**

The user rating is not being checked on the backend. It should be checking for a rating between 1-5.
</p>
</details>

------------

##### Post some feedback in another users name

Requirements
- login to the admin account using SQLi

Attack Page:  http://localhost.:3000/#/contact

Tools: Burp (turn intercept on)

<details><summary>Hint</summary>
<p>
Navigate to the contact page

Open Burp and turn 'Intercept' to ON

view the request
</p>
</details>

<details><summary>Answer</summary>
<p>
Change request from:

`{"UserId":16,"captchaId":14,"captcha":"21","comment":"test","rating":1}`

(change the userid)

`{"UserId":2,"captchaId":14,"captcha":"21","comment":"test","rating":1}`
</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Lessons Learned**

The user does have a valid account but should not be able to access another users information.

"Insecure Direct Object References occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability attackers can bypass authorization and access resources in the system directly, for example database records or files.

https://www.owasp.org/index.php/Testing_for_Insecure_Direct_Object_References_(OTG-AUTHZ-004)

**Prevention:**

The application should perform an access control check to ensure the user is authorized for the request object or service

**News**

https://krebsonsecurity.com/2018/04/panerabread-com-leaks-millions-of-customer-records/

"some of the customer records include unique identifiers that increment by one for each new record, making it potentially simple for someone to scrape all available customer accounts. The format of the database also lets anyone search for customers via a variety of data points, including by phone number.”
</p>
</details>


------------

##### Place an order that makes you rich

Attack Page: Follow the purchase flow (add an item to the basket)

Tools: Burp

<details><summary>Hint</summary>
<p>
1. Open Burp and turn ‘Intercept’ to ON
 
2. add an item(s) to your basket 

3. View the requests

NOTE: this is a bit tricky – it has to do with how many you are buying… or how many they are giving you

</p>
</details>

<details><summary>Answer</summary>
<p>
Change the request from

`{"ProductId":24,"BasketId":"6","quantity":10}`

to

`{"ProductId":24,"BasketId":"6","quantity":-100}`

click forward and check your basket

</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Prevention**

Validate the the quality is a reasonable positive number

**Story**

Shuttle company
</p>
</details>

Try The Challenge again with postman
<details><summary>Postman</summary>
<p>
1.Log in as any user.
 
2. Put at least one item into your shopping basket.

3. Note that reducing the quantity of a basket item below 1 is not possible via the UI

4. When changing the quantity via the UI, you will notice PUT requests to http://localhost:3000/api/BasketItems/{id} in the 5. Network tab of your DevTools

5. Memorize the `{id}` of any item in your basket

6. Copy your `Authorization` header from any HTTP request submitted via browser.

7. Submit a `PUT` request to http://localhost:3000/api/BasketItems/{id} replacing `{id}` with the memorized number from 5. and with:

`{"quantity": -100}` as body,

`application/json` as `Content-Type`

and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.

Dev Tools View

![image](https://user-images.githubusercontent.com/40209642/69178896-c6e6b700-0ac7-11ea-9888-906783d2c265.png)


Postman

![image](https://user-images.githubusercontent.com/40209642/69178995-f1387480-0ac7-11ea-961d-5dff143f8ed1.png)


Authorization Tab
![image](https://user-images.githubusercontent.com/40209642/69179053-1331f700-0ac8-11ea-8d15-47d9a09c2607.png)



</p>
</details>
------------

##### View another user's shopping basket

1. Add a few items to your basket 

2. go to http://localhost.:3000/#/basket 


<details><summary>Hint</summary>
<p>
Check out the storage tab

</p>
</details>

<details><summary>Answer</summary>
<p>
in ‘Session Storage’ change the ‘bid’ value to a lower number (if this were a real app you could also change it to a higher number)
</p>
</details>

<details><summary>More Info</summary>
<p>
 
**Lessons Learned**

The user does have a valid account but should not be able to access another users basket

“Insecure Direct Object References occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability attackers can bypass authorization and access resources in the system directly, for example database records or files.

-https://www.owasp.org/index.php/Testing_for_Insecure_Direct_Object_References_(OTG-AUTHZ-004)

**Prevention**

The application should perform an access control check to ensure the user is authorized for the request object or service

**News**

https://krebsonsecurity.com/2018/04/panerabread-com-leaks-millions-of-customer-records/

“some of the customer records include unique identifiers that increment by one for each new record, making it potentially simple for someone to scrape all available customer accounts. The format of the database also lets anyone search for customers via a variety of data points, including by phone number.”
</p>
</details>


