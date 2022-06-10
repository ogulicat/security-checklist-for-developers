### Updated: 10 June 2022

##### AUTHENTICATION SYSTEMS (Signup/Signin/2 Factor/Password reset) 
- [ ] Use HTTPS everywhere.
- [ ] Force to use strong password (e.g.: Choose a password with at least 8 characters, upper and lower case letters, numbers and special symbols).
- [ ] Store password hashes using `Bcrypt` (no salt necessary - `Bcrypt` does it for you).
- [ ] Destroy the session identifier after `logout`.  
- [ ] Destroy all active sessions on reset password (or offer to).  
- [ ] Must have the `state` parameter in OAuth2.
- [ ] No open redirects after successful login or in any other intermediate redirects.
- [ ] When parsing Signup/Login input, sanitize for javascript://, data://, CRLF characters. 
- [ ] Set secure, httpOnly cookies.
- [ ] In Mobile `OTP` based mobile verification, do not send the OTP back in the response when `generate OTP` or `Resend OTP`  API is called.
- [ ] Limit attempts to `Login`, `Verify OTP`, `Resend OTP` and `generate OTP` APIs for a particular user. Have an exponential backoff set or/and something like a captcha based challenge.
- [ ] Check for randomness of reset password token in the emailed link or SMS.
- [ ] Set an expiration on the reset password token for a reasonable period.
- [ ] Expire the reset token after it has been successfully used.


##### USER DATA & AUTHORIZATION
- [ ] Any resource access like, `my cart`, `my history` should check the logged in user's ownership of the resource using session id.
- [ ] Serially iterable resource id should be avoided. Use `/me/orders` instead of `/user/37153/orders`. This acts as a sanity check in case you forgot to check for authorization token. 
- [ ] `Edit email/phone number` feature should be accompanied by a verification email to the owner of the account. 
- [ ] For file uploads, you should check the extension after the "last" dot (E.g.: shellfile.jpeg.php). Also, you should check the actual type of the file with "File Header Signatures". 
- [ ] Again file uploads, it is important to give a random name to the filename and limit the size of the uploaded file.
- [ ] For user ids and other ids, use [RFC compliant ](http://www.ietf.org/rfc/rfc4122.txt) `UUID` instead of integers. In this way, we may have taken a precaution against `Brute-Force` attacks.
- [ ] JWT are awesome. Use them if required for your single page app/APIs.


##### SECURITY HEADERS & CONFIGURATIONS
- [ ] `Add` [CSP](https://en.wikipedia.org/wiki/Content_Security_Policy) header to mitigate XSS and data injection attacks. This is important.
- [ ] `Add` [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) header to prevent cross site request forgery. Also add [SameSite](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-same-site-00) attributes on cookies (You can use `Lax` or `Strict` mode).
- [ ] `Add` [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) header to prevent SSL stripping attack (E.g.: max-age=31536000 ; includeSubDomains).
- [ ] Use HSTS responses to force TLS only access. Redirect all HTTP request to HTTPS on the server as backup.
- [ ] `Add` [X-Frame-Options](https://en.wikipedia.org/wiki/Clickjacking#X-Frame-Options) to protect against Clickjacking.
- [ ] `Add` [X-XSS-Protection](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#X-XSS-Protection) header to mitigate XSS attacks (E.g.: X-XSS-Protection: 1; mode=block).
- [ ] Update DNS records to add [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) record to mitigate spam and phishing attacks.
- [ ] Add [subresource integrity checks](https://en.wikipedia.org/wiki/Subresource_Integrity) if loading your JavaScript libraries from a third party CDN. For extra security, add the [require-sri-for](https://w3c.github.io/webappsec-subresource-integrity/#parse-require-sri-for) CSP-directive so you don't load resources that don't have an SRI sat.  
- [ ] Do not use critical data or tokens in GET request parameters. Exposure of server logs or a machine/stack processing them would expose user data in turn.  
- [ ] Use a `Referrer-Policy` to prevent URL addresses from leaking to other websites.
- [ ] Don't use `CORS` unless you have to, and if you have to, be careful with it. 
- [ ] X-Content-Type-Options: nosniff for XSS protection.
- [ ] X-Frame-Options: DENY for `clickjacking` protection.


##### SANITIZATION OF INPUT
- [ ] `Sanitize` all user inputs or any input parameters exposed to user to prevent [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting) (E.g.: use htmlspecialshart w/ ENT_QUOTES | ENT_HTML_5 flag in PHP). 
- [ ] Always use parameterized queries to prevent [SQL Injection](https://en.wikipedia.org/wiki/SQL_injection).  
- [ ] Sanitize user input if using it directly for functionalities like CSV import.
- [ ] `Sanitize` user input for special cases like robots.txt as profile names in case you are using a url pattern like blabla.com/username. 
- [ ] Do not hand code or build JSON by string concatenation ever, no matter how small the object is. Use your language defined libraries or framework.
- [ ] Sanitize inputs that take some sort of URLs to prevent [SSRF](https://docs.google.com/document/d/1v1TkWZtrhzRLy0bYXBcdLUedXGb9njTNIJXa3u9akHM/edit#heading=h.t4tsk5ixehdd).
- [ ] Sanitize Outputs before displaying to users.
- [ ] Limit the length of the text written to the input (form fuzzy protection).


##### DATABASE
- [ ] Don't store sensitive datas (e.g.: name, surname, email, phone number, address etc.) unless you truly need it. If you really need it, Ecrypt all this information.
- [ ] Use minimal privilege for the database access user account. Don’t use the database root account and check for unused accounts and accounts with bad passwords.
- [ ] Fully prevent SQL injection by only using SQL prepared statements. For example: if using NPM, don’t use npm-mysql, use npm-mysql2 which supports prepared statements.
- [ ] Just in case, back up your database regularly.


##### APIs & WEB SERVICES
- [ ] Ensure that users are fully authenticated and authorized appropriately when using your APIs.
- [ ] The application should be designed to deliver web services with a well-structured protocol that offers at least TLS v1.2 and equivalent security measures.
- [ ] The application should be designed in such a way that it does not contain scripts in the data sent with the web service.


##### ANDROID / IOS APP
- [ ] `salt` from payment gateways should not be hardcoded.
- [ ] `secret` / `auth token` from 3rd party SDK's should not be hardcoded.
- [ ] API calls intended to be done `server to server` should not be done from the app.
- [ ] In Android, all the granted  [permissions](https://developer.android.com/guide/topics/security/permissions.html) should be carefully evaluated.
- [ ] On iOS, store sensitive information (authentication tokens, API keys, etc.) in the system keychain. Do __not__ store this kind of information in the user defaults.
- [ ] [Certificate pinning](https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning) is highly recommended.


##### PEOPLE & INCIDENT RESPONSE
- [ ] Set up an email (e.g. security@blabla.com) and a page for security researchers to report vulnerabilities.
- [ ] Depending on what you are making, limit access to your user databases.
- [ ] In case of a hack or data breach, check previous logs for data access, ask people to change passwords. You might require an audit by external agencies depending on where you are incorporated.  
- [ ] Have a practiced security incident plan (Hope you don't need it, but keep it in the corner anyway :)).
