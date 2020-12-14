# adham_research_week7

## http vs https
The main difference between http and https is that the second uses a SSL(secure socket layer). 
http operates at application layer while https operates at transporting layer which is above of the network layer, also http sends data on port 80 while https sends data on port 433.
in https protocol the data is encrypted before sending but no encryption in http protocol.
To be able to send data via https protocol you need a SSL certificate which is data file that is hosted in our server, the file contains data like the public key and the website identity, the domain, the expiration date of the certificate ...etc.

There are many ways to generate a certificate in node one of them is to create a SSL certificate by generating public and private keys, but this certificate won’t be trusted, so it’s good for internal usage. 
another way is to generate a certificate signed by a certificate authority, there are services to do that like certbot.
In conclusion the https protocol is important to be implemented in our project, it’s more secure and protects users of our server  from attacks like MITM (man in the middle).   

## stateful vs stateless

In a stateful model we create a session in the server and save it somewhere in there, the session id sent to the client. So every time the client wants to send a request to the server, the request must carry the id to the server, then the server will check if the id is correct and correspond. 
So if we delete the session from the backend, the session id that the user has is completely meaningless.
stateful advantages:
 - Revoke the session anytime
 - Easy to implement and manage for one-session-server scenario 
  - Session data can be changed later 
 
stateful disadvantages:
 - Increasing server overhead
 - Fail to scale



 
On the other hand, in stateless models we store the session in the client side which is the browsers, So  the server only has the capability to verify its validity by checking whether the payload and the signature match. It's also called (token-based authentication) because it enables users to obtain a token that allows them to access a service and fetch a specific resource without using their username and password to authenticate every request, So the client is responsible for sending any state information to the server whenever it is needed. In this case, the server side does not need to maintain the state of a user. Another name for this approach is passwordless approach.
 
 - advantages of stateless: stateless approach has the opposite of the stateful disadvantages, So in this approach it’s easy to scale the app, and there’s no overhead on the server.
 
 - disadvantages  of stateless: 
 
     - Since the user session is stored at client side, the server does not have any rights to delete the session.
    
     - it increases the technical complexity and it is not extremely useful when we only have one-session-server.
    
     - Session data cannot be changed until its expiration time
   
 ## Sessions
Session is used to store information about users, unlike cookies, sessions stored on the client and server sides.
Session creates a file in the server where the session variable and their values are assigned. So this data will be available to every page in the site. the session ends when the user closes the browser or after leaving the site, the server will terminate the session after a predetermined period of time
Express makes use of the express-session node package to manage the session and session-file-store to store session data in a session file. 
so we need to install these two messages, import and save them in variables.
 
  const session = require('express-session')

  const FileStore = require('session-file-store')(session)
  
  app.use(session({name:'session-id', secret:'123456xxx', store:new FileStore()}))
  
  
  .............
  
  ## Attacks
  
 
MITM
A man in the middle attacks as the name says it’s a way that the hacker puts himself in the middle between the user and the server, and listens to the requests and the response.
The  goal of this is to steal the data like personal informations, login credentials, account details and any other important data.
  
 
XSS
Cross-Site Scripting attacks are a type of injection where the hacker injects some scripts in a way that looks very normal, a common way for this type of injection is to use a form.  the script can access any cookies, session tokens, or other sensitive information retained by the browser and used with that site.
 
 
CSRF
Cross Site Request Forgery is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated.
if the user is authenticated in a website, the attackers use some tricks like chat or emails to send a url to the user, so when the user open the url, it will send a request to the application that he is authenticate at, and the app will not distinguish between user and hacker and it will let him access to data like session or make the hacker ables to change data like cookies or username and password.
 

Cookies can be secured using the following attributes.
  - The Secure attribute instructs the browser to set cookies over HTTPS only. This attribute prevents MITM attacks since the transfer is over TLS.
  - The HttpOnly attribute blocks the ability to use the document.cookie object. This prevents XSS attacks from stealing the session identifier.
  - The SameSite attribute blocks the ability to send a cookie in a cross-origin request. This provides limited protection against CSRF attacks.
  - Setting Domain & Path attributes can limit the exposure of a cookie. By default, Domain should not be set and Path should be restricted.
  - Expire & Max-Age allow us to set the persistence of a cookie.
