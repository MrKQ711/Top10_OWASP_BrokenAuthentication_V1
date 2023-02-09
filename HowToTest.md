# HowToTest

**TEST FOR BROKEN AUTHENTICATION IN WEB APPLICATION**

Authentication is a mechanism put in place to determine if a user is who they say they are, either through a password-based system or any other form of authentication system.

In this article, we’ll be going through how to test for broken authentication, their impact, and how to mitigate them.

**TABLE OF CONTENTS**

---

*What is broken authentication?*

---

*Checklist for broken authentication*

- ***Application Password Functionality***
- ***Additional Authentication Functionality***

---

*Impact of broken authentication*

---

*Prevention of broken authentication*

---

*Conclusion*

---

1. **What is broken authentication?**
    - Broken authentication is a type of vulnerability that allows attacker to get into a web application without proper credentials
    - This could be carried out either by bypassing the authentication mechanism put in place or by brute-force another user’s account.
    - If the attacker successfully bypassed or brute-force his way into another user’s account, they gain access to all the data and privileges of that user account.
    - According to the OWASP Top 10 2021 report, broken authentication is ranked number 7 and is ranked number 7 and is grouped under Identification and Authentication Failures.
    - This category slipped down from second place and now contains Common Weakness Enumerations (CWEs) relating to identification issues. It was previously known as broken authentication.
    - The severity of this vulnerability can be so high. Say, an attacker was able to brute-force his way into the administrator account of a web application, this means he gets full control over the web application.
    - This article seeks to demonstrate how an attacker tests for broken authentication in a web application and how to prevent them.
2. **Checklist for broken authentication**
    - ***Application Password Functionality***
        - Test for weak lock out mechanism
            - *An example test may be as follows:*
                
                 1.   Attempt to log in with an incorrect password 3 times.
                
                1. Successfully log in with the correct password, thereby showing that the lockout mechanism doesn't trigger after 3 incorrect authentication attempts.
                2. Attempt to log in with an incorrect password 4 times.
                3. Successfully log in with the correct password, thereby showing that the lockout mechanism doesn't trigger after 4 incorrect authentication attempts.
                4. Attempt to log in with an incorrect password 5 times.
                5. Attempt to log in with the correct password. The application returns "Your account is locked out.", thereby confirming that the account is locked out after 5 incorrect authentication attempts.
                6. Attempt to log in with the correct password 5 minutes later. The application returns "Your account is locked out.", thereby showing that the lockout mechanism does not automatically unlock after 5 minutes.
                7. Attempt to log in with the correct password 10 minutes later. The application returns "Your account is locked out.", thereby showing that the lockout mechanism does not automatically unlock after 10 minutes.
                8. Successfully log in with the correct password 15 minutes later, thereby showing that the lockout mechanism automatically unlocks after a 10 to 15 minute period.
        - Test remember me functionality
            - Look for passwords being stored in a cookie. Examine the cookies stored by the application. Verify that the credentials are not stored in clear text, but are hashed.
            - Examine the hashing mechanism: if it is a common, well-known algorithm, check for its strength; in homegrown hash functions, attempt several usernames to check whether the hash function is easily guessable.
            - Verify that the credentials are only sent during the log in phase, and not sent together with every request to the application.
            - Consider other sensitive form fields (e.g. an answer to a secret question that must be entered in a password recovery or account unlock form).
        - Test password reset and/or recovery
            - *What information is required to reset the password ?*
                - The first step is to check whether secret questions are required. Sending the password (or a password reset link) to the user email address without first asking for a secret question means relying 100% on the security of that email address, which is not suitable if the application needs a high level of security.
            - *Are reset passwords generated randomly ?*
                - If the application sends or visualizes the old password in clear text because this means that passwords are not stored in a hashed form, which is a security issue in itself.
            - *Is the reset password functionality requesting confirmation before changing the password?*
                - To limit denial-of-service attacks the application should email a link to the user with a random token, and only if the user visits the link then the reset procedure is completed. This ensures that the current password will still be valid until the reset has been confirmed.
        - Test password change process
            - *Is the old password requested to complete the change?*
                - The most insecure scenario here is if the application permits the change of the password without requesting the current password. Indeed if an attacker is able to take control of a valid session they could easily change the victim's password.
        - Test CAPTCHA
            - Verify the time duration in which the captcha is loaded on the webpage.
            - Test captcha should not be by passed by clicking on the captcha multiple time when the captcha is not loaded and shown on the webpage during web page loading.
            - Check the time out for the Captcha. The time in which the captcha become unchecked.
            - Test the captcha on slow internet. An invalid captcha error message should not be shown.
            - Verify the captcha and click on the submit button two times. It should not display an invalid captcha error.
        - Test multi-factor authentication
            - *User registration*
                - When a user registers for an app, they are usually asked to input their name and email address. To validate the email address, the app sends an email containing a confirmation link. After the user receives the link and opens it, they can continue with registration.
            - *Device Authentication*
                - Sometimes, users need to access their email from a new device. To distinguish the real login from a hacking attempt, the email vendor sends a suspicious activity alert and SMS containing an OTP to the registered mobile number. The user logs into the application with the OTP. The security notification alerts the user if there is a security breach.
            - *Password reset*
                - Whenever a user requests a password reset, an OTP goes to the phone number associated with the app. Again, the user enters the OTP and moves on to resetting the password.
        - Test for default login
            - Check the limit on the total number of unsuccessful login attempts. So that a user cannot use a brute-force mechanism to try all possible combinations of username-password.
            - Verify that in case of incorrect credentials, a message like “incorrect username or password” should get displayed. Instead of an exact message pointing to the incorrect field.
            - Verify the login session timeout duration. So, once logged in a user cannot be authenticated for a lifetime.
        - Test for weak security question/answer
            - *Testing for weak  pre-generated questions*
                - Try to obtain a list of security questions by creating a new account or by following the “I don’t remember my password”-process.
                - Try to generate as many questions as possible to get a good idea of the type of security questions that are asked.
            - *Testing for weak self-generated questions*
                - Try to create security questions by creating a new account or by configuring your existing account’s password recovery properties.
    - ***Additional Authentication Functionality***
        - Test for account enumeration and guessable user account base on HTTP Response Message
            - *Check the valid credentials*
                - Record the server answer when you submit a valid user ID and valid password
            - *Testing for valid user with wrong password*
                - Insert a valid user ID and a wrong password and record the error message generated by the application. The browser should display a message:
                
                ![Untitled](Image/Untitled.png)
                
                       or unlike any message that reveals the existence of the user like the following:
                
                *`Login for User foo: invalid password`*
                
            - *Testing for a Nonexistent Username*
                - Insert an invalid user ID and a wrong password and record the server answer (the tester should be confident that the username is not valid in the application). Record the error message and the server answer. If the tester enters a nonexistent user ID, they can receive a message similar to :
                    
                    ![Untitled](Image/Untitled%201.png)
                    
                    *or a message like the following one:*
                    
                    *`Login failed for User foo: invalid Account`*
                    
        - Test for authentication bypass schema
            - *Direct page request*
                - If a web application implements access control only on the log in page, the authentication schema could be bypassed. For example, if a user directly requests a different page via forced browsing, that page may not check the credentials of the user before granting access. Attempt to directly access a protected page through the address bar in your browser to test using this method.
            - *Session ID Prediction*
                - Many web applications manage authentication by using session identifiers (session IDs). Therefore, if session ID generation is predictable, a malicious user could be able to find a valid session ID and gain unauthorized access to the application, impersonating a previously authenticated user. In the following figure, values inside cookies increase linearly, so it could be easy for an attacker to guess a valid session ID.
        - Test for brute force protection
            - What characters are permitted and forbidden for use within a password? Is the user required to use characters from different character sets such as lower and uppercase letters, digits and special symbols?
            - How often can a user change their password? How quickly can a user change their password after a previous change?
            - When must a user change their password? After 90 days? After account lockout due to excessive log on attempts?
            - How different must the next password be from the last password?
            - Is the user prevented from using his username or other account information (such as first or last name) in the password?
        - Test for credentials transports over an encrypted channel
            - *Sending data with POST method through HTTP*
                
                Suppose that the login page presents a form with fields User, Pass, and the Submit button to authenticate and give access to the application. If we look at the headers of our request with WebScarab we can get something like this:
                
            
            ```
            POST http://www.example.com/AuthenticationServlet HTTP/1.1
            Host: www.example.com
            User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; it; rv:1.8.1.14) Gecko/20080404
            Accept: text/xml,application/xml,application/xhtml+xml
            Accept-Language: it-it,it;q=0.8,en-us;q=0.5,en;q=0.3
            Accept-Encoding: gzip,deflate
            Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
            Keep-Alive: 300
            Connection: keep-alive
            Referer: http://www.example.com/index.jsp
            Cookie: JSESSIONID=LVrRRQQXgwyWpW7QMnS49vtW1yBdqn98CGlkP4jTvVCGdyPkmn3S!
            Content-Type: application/x-www-form-urlencoded
            Content-length: 64
            
            delegated_service=218&User=test&Pass=test&Submit=SUBMIT
            
            ```
            
            From this example the tester can understand that the POST request sends the data to the page *www.example.com/AuthenticationServlet* using HTTP. So the data is transmitted without encryption and a malicious user could intercept the username and password by simply sniffing the network with a tool like Wireshark.
            
            - *Sending data with POST method through HTTPS*
            
            Suppose that our web application uses the HTTPS protocol to encrypt the data we are sending (or at least for transmitting sensitive data like credentials). In this case, when logging on to the web application the header of our POST request would be similar to the following:
            
            ```
            POST https://www.example.com:443/cgi-bin/login.cgi HTTP/1.1
            Host: www.example.com
            User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; it; rv:1.8.1.14) Gecko/20080404
            Accept: text/xml,application/xml,application/xhtml+xml,text/html
            Accept-Language: it-it,it;q=0.8,en-us;q=0.5,en;q=0.3
            Accept-Encoding: gzip,deflate
            Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
            Keep-Alive: 300
            Connection: keep-alive
            Referer: https://www.example.com/cgi-bin/login.cgi
            Cookie: language=English;
            Content-Type: application/x-www-form-urlencoded
            Content-length: 50
            
            Command=Login&User=test&Pass=test
            
            ```
            
            We can see that the request is addressed to *www.example.com:443/cgi-bin/login.cgi* using the HTTPS protocol. This ensures that our credentials are sent using an encrypted channel and that the credentials are not readable by a malicious user using a sniffer.
            
        - Test for weaker authentication in alternative channel
            - *Identify other channels by using the following methods:*
                - Reading site content, especially the home page, contact us, help pages, support articles and FAQs, T&Cs, privacy notices, the robots.txt file and any sitemap.xml files.
                - Searching HTTP proxy logs, recorded during previous information gathering and testing, for strings such as “mobile”, “android”, blackberry”, “ipad”, “iphone”, “mobile app”, “e-reader”, “wireless”, “auth”, “sso”, “single sign on” in URL paths and body content.
                - Use search engines to find different websites from the same organization, or using the same domain name, that have similar home page content or which also have authentication mechanisms
            - *Enumerate Authentication Functionality*
                - For each alternative channel where user accounts or functionality are shared, identify if all the authentication functions of the primary channel are available, and if anything extra exists. It may be useful to create a grid like the one below:
                    
                    ![Untitled](Image/Untitled%202.png)
                    
3. **Impact of broken authentication**
    - Compromising an account allows the attacker access to unauthorized information.
    - It could lead to full application takeover.
    - Loss of sensitive and confidential business information.
4. **Prevention of broken authentication**
    - [Don’t expose sessions IDs in URLs](https://julienprog.wordpress.com/2017/08/17/session-id-in-the-url-is-it-a-vulnerability/) - [Session fixation attack](https://www.netsparker.com/blog/web-security/session-fixation-attacks/) is a vulnerability that allows an attacker to hijack a user session.
    - [Don’t give room for user enumeration](https://www.virtuesecurity.com/kb/username-enumeration/) - It’s critical to utilize identical, generic error messages and to double-check that they’re the same. With every login request, you should return the same HTTP status code.
    - [Implement a strong password policy](https://en.wikibooks.org/wiki/Web_Application_Security_Guide/Password_security) - Allowing the use of weak and well-known passwords is not a good idea. After a given number of login attempts, require users to pass a CAPTCHA test.
    - [brute-force protection](https://predatech.co.uk/protecting-your-web-app-brute-force-login-attacks/) - prevent brute-force login attempts.
    - [Multi-factor authentication](https://auth0.com/docs/secure/multi-factor-authentication/step-up-authentication/configure-step-up-authentication-for-web-apps) - provides an extra layer of security for users.
    - Bot detection - Bot detection is designed to combat credential stuffing and other forms of bot-driven attacks. It works by correlating a variety of internal and external data sources to identify and mitigate bot-driven attacks before login. When an IP address is deemed suspicious, it is presented with a CAPTCHA on login, which prevents most bot attacks from successfully authenticating.
    - Breached Password Detection - A Credential stuffing attacks are a major threat to any web application. These attacks rely on users reusing a password that was previously compromised in another breach. Auth0 keeps a continually updated database of known breached credentials. When a user is detected using breached credentials, admins can choose to warn them but allow the login, deny the login and force a password reset, or trigger MFA.
5. **Conclusion**
    - To summarize, we have gone through how to set up an OWASP Juice shop. We have also earned how to test for broken authentication in web applications, the impact and prevention of broken authentication applications.
    - Broken authentication is a vulnerability that must be prevented by all means. Ensuring you have a proper and secured authentication mechanism is very important.
