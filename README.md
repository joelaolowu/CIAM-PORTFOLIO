

# CIAM-PORTFOLIO

www.linkedin.com/in/joelaolowu

My Customer Identity and Access Management projects with multiple core use cases 



::PROJECT:: 


SIMPLIFYTECH WANTS TO LAUNCH A CUSTOMER PORTAL



::MY JOB:: 


TO DESIGN, CHOOSE A CIAM PLATFORM AND BUILD A CUSTOMER PORTAL FOR NEW USERS, RETURNING CUSTOMERS AND PARTNER USERS FOR DIFFERENT USE CASES.THE USE CASES ARE:


1,  B2C SELF REGISTRATION

2,  B2B PARTNER FEDERATION

3,  PROGRESSIVE PROFILLING

4,  ACCOUNT LINKING






I SPENT A WEEK WITH THE PRODUCT DESIGN TEAMS AND OTHER STAKE HOLDERS ON THE USERS PERSONAS, THE BUSINESS, THE COST EFFECTIVE CIAM PLATFORM, THE TARGET CUSTOMERS AND CUSTOMERS RETENTION. THE MAJOR CIAM PRODUCTS AND VENDORS CONSIDERED WERE:

1, MICROSOFT ENTRA ID(EXTERNAL)

2  PING IDENTITY

3, OKTA CUSTOMER IDENTITY CLOUD( FORMERLY AUTH0)

4, CYBERARK IDENTITY





OKTA CUSTOMER IDENTITY CLOUD (FORMERLY AUTH0) was decided because it's the cheapest especially being free for the first thirty days without Bank details and it's very good for a start up like Simplyfytech. 
Key Features and Use Cases



Auth0 supports various use cases, including:


User Authentication and Authorization: 

Users can log in using identifiers like username, email, or phone number, or through social accounts such as Facebook or Twitter. Auth0 retrieves user profiles post-login to customize the UI and apply authorization policies.


API Security: 

Auth0 secures APIs using OAuth 2.0, ensuring that only authorized users can access the API.



Single Sign-On (SSO): 


Auth0 enables SSO across multiple applications, allowing users to log in once and access all authorized applications without re-entering credentials.


Multi-Factor Authentication (MFA): 

Auth0 enforces MFA for accessing sensitive data, adding an extra layer of security.


Passwordless Login: 

Users can log in with one-time codes delivered via email or SMS, eliminating the need for passwords.



Compliance: 

Auth0 helps organizations comply with regulations such as SOC2, GDPR, PCI DSS, and HIPAA.


Integration and Customization:

Auth0 provides a user-friendly dashboard and Management API to create and configure authentication and authorization settings. Developers can customize login behaviors, connect user data stores, manage users, and establish authentication factors. Auth0 supports various application types, including web apps, mobile apps, and machine-to-machine apps.


Security and Monitoring:

Auth0 offers several security features to protect against malicious attacks, such as:


Bot Detection: 

Prevents cyber attacks using Google reCAPTCHA Enterprise.


Breached Password Detection: 

Alerts users if their credentials are compromised.


Brute-Force Protection: 

Limits login attempts and blocks malicious IPs.


Suspicious IP Throttling: 

Blocks traffic from IPs attempting rapid signups or logins.

Auth0 also provides error tracking and alerts to monitor deployments and ensure smooth operation.


:::Conclusion:::


Auth0 is a powerful and adaptable IAM solution that simplifies the implementation of authentication and authorization in applications. It offers robust security features, customization options, and compliance support, making it an ideal choice for developers looking to enhance their application's security and user experience.





Step 1 , 

register your application with the identity provider

Every provider requires that you register a client so it can issue a client ID (and sometimes a client secret). During registration you’ll supply a name, contact information, and one or more redirect URIs: the exact url(s) where the provider will send the user back with an authorization code or error. Redirect URIs must match precisely; mismatches are a frequent cause of failures. After registration, note the authorization endpoint, token endpoint, and any endpoints for userinfo, revocation, or introspection the provider documents.

Step 2 , 

Choose the correct grant and scopes

Match the grant to your client type. For single-page apps and native mobile apps use Authorization Code with PKCE so you do not need to store a secret on the client. For server-side web apps use standard Authorization Code and keep the client secret confidential on the server. For machine-to-machine interactions, consider Client Credentials grant. Scopes are permissions your app requests from the user; request the minimal scopes needed for the task and request additional scopes later if needed to reduce friction at consent time.

Step 3 , 

Build the authorization request

The authorization request directs the user to the provider so they can authenticate and consent. It is an HTTP GET to the provider’s authorization endpoint with parameters such as client_id, redirect_uri, response_type=code, scope, and state. If you use PKCE, generate a code_verifier (a high-entropy random string) and send its transformed value, code_challenge, along with code_challenge_method=S256. The state parameter is used to prevent CSRF attacks,store it on the client (or server) and compare it after redirect.



Authorization request example (template)

GET 

client_id=YOUR_CLIENT_ID

&redirect_uri=https%3A%2F%2Fyourapp.example.com%2Fcallback

&response_type=code

&scope=openid%20profile%20email

&state=RANDOM_STATE

&code_challenge=CODE_CHALLENGE

&code_challenge_method=S256


Step 4 , 

Handle the redirect and exchange the code for tokens
After the user approves access, the provider will redirect to your redirect URI with parameters such as code and state. Verify the state matches what you issued earlier. Then exchange the authorization code for tokens by POSTing to the token endpoint. For confidential clients include the client_secret in a secure server-side request; for PKCE clients include the original code_verifier. The token response typically includes an access_token, an id_token (if using OpenID Connect), and sometimes a refresh_token.

Token exchange example (curl)

POST 

Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code

&code=AUTHORIZATION_CODE

&redirect_uri=https%3A%2F%2Fyourapp.example.com%2Fcallback

&client_id=YOUR_CLIENT_ID

&code_verifier=ORIGINAL_CODE_VERIFIER


Step 5 , 

Validate, store, and use tokens
Once you receive tokens, validate them before using protected APIs. For JWTs, check the signature (using the provider’s public keys), expiry (exp), audience (aud), and issuer (iss). If an id_token is present, validate the nonce if you supplied one earlier to prevent replay attacks. Store access and refresh tokens securely,on the server in an encrypted datastore, or in memory/secure storage for native apps. Avoid long-lived tokens in insecure places like localStorage for browser apps. Use the access_token as a Bearer token in API requests, and use refresh tokens (if issued) to obtain new access tokens without user interaction.


Step 6 , 

Support refresh, revocation, and logout
Implement token refresh logic so your app can recover from access token expiry using the refresh_token grant. Be prepared for refresh failures (user revoked consent, refresh token expired) by directing users back to the authorization flow. Support token revocation or sign-out by calling the provider’s revocation endpoint and clearing local session state. If using OpenID Connect, consider calling the provider’s end_session_endpoint for centralized logout so the user is logged out from the identity provider as well.


:::Security and operational best practices:::

Protect client secrets: store them only on trusted servers and rotate them if you suspect compromise. Always use HTTPS for redirect URIs and token exchanges. Use PKCE for public clients to prevent intercepted authorization codes from being used by attackers. Limit scope requests to what your app needs and request additional permissions only as required. Implement strict redirect URI matching on the provider side, validate state and nonce values, and check token claims (audience, issuer, expiry). Monitor and log auth failures for debugging, but avoid logging secrets or tokens.


:::Testing and troubleshooting tips:::

Use your provider’s developer console to inspect registered clients and logs. Tools like Postman, OAuth Playground, or provider-specific SDK debug pages let you simulate flows and view tokens. If you see “redirect_uri_mismatch”, confirm the redirect URI exactly matches the registered URI including scheme, hostname, path, and trailing slash. For “invalid_grant” errors during token exchange, check that you are exchanging the most recent authorization code (codes are single use) and that the code_verifier matches the code_challenge if using PKCE. When tokens fail validation, compare the token’s issuer and audience to your configured values and fetch the provider’s JWKS (json Web Key Set) to verify signatures.




In Summary:



-Create Your Auth0 Free Tenant like i just did with Simplytech service Company by goint to auth0.com to sign up`

-Click Start Building for Free or Sign Up

-Sign up using your email address (not Google SSO - we want email login as the starting point. I used olowuahj@gmail.com)

-Verify your email when Auth0 sends the confirmation link

-Set the Password after clicking the confirmation link

-Account Type: Personal

-No Advanced Settings

-When asked to select a region: Leave it to default
Auth0 creates your tenant automatically and takes you to the Dashboard

✅  Verify: You can see the Auth0 Dashboard at manage.auth0.com. You are logged in like i just did. mine is dev-tjzm2vfebnkrsh7w
full name is dev-tjzm2vfebnkrsh7w.us.auth0.com . My tenant's domain is also called auth0 domain. 


-In the Auth0 Dashboard go to Authentication in the left menu

-Click Database

-Click on Username-Password-Authentication (this is the default connection)

-Make sure it is enabled

-Scroll down - confirm Requires Username is OFF (we use email as the identifier)
-Save if you made any changes



THE USE CASES/EXAMPLES ARE:

A

Alex Turner  |  B2C Customer

Self-registered consumer. Uses the SimplifyTech customer portal to track orders. Signed up with email. Low initial trust, builds over time through verified actions.
Lab use case: Registration flow, Google social login, T&C consent, MFA


B

Marcus Webb Ltd  |  B2B Partner

External company employee. Accesses the SimplifyTech partner portal. Uses corporate credentials. High trust established by the signed partnership agreement.
Lab use case: SAML B2B federation concept — discussed, not configured in the lab


C

Trial User  |  Guest / Time-Limited

No full account. 30-day free trial. Minimal data collected. Consent required but lightweight.
Lab use case: Discussed as a concept. Handled via Auth0 application-level token expiry.




in all, 




Six scenarios you encounter on every CIAM engagement. let's Understand them before configuring anything.

1. Self-Registration vs Invited User
   
Alex Turner self-registers. Marcus Webb Ltd employees are invited by an admin. Different flows, different trust levels, different data collected. In Auth0 these are different connections and application types.

2. Social Login
   
Customers sign in with Google or Apple instead of creating a password. Reduces friction. Increases registration conversion. The CIAM platform creates an identity linked to the social provider. Identity linking: what happens when the same email exists in both username-password and Google social? This needs a deliberate policy decision before go-live. This is account linking, the CIAM version of correlation from Week 2: correlation stops duplicate identities for employees, account linking stops them for customers.

3. Progressive Profiling
   
Collect email and password first. On second login ask for phone number. On fifth login ask for preferences. Auth0 Actions implement this by checking loginsCount and requesting additional fields progressively. Reduces registration dropout significantly.

4. Consent and Terms & Conditions Versioning
   
Privacy laws across the world require that users explicitly agree to how their data will be used before you process it. T&C acceptance is both a legal requirement in most markets and a trust signal for your customers. The specific law varies by jurisdiction. The technical implementation is the same everywhere: gate access until the user has accepted, record the acceptance with a timestamp and version, and make it as easy to withdraw as it was to give.

5. Account Lockout and Brute Force

Without brute force protection, attackers attempt thousands of password guesses. Auth0 Attack Protection blocks this. The threshold decision matters: too low and legitimate users get locked out, too high and attackers get too many attempts.

6. Right to Erasure and Global Privacy Law

A customer asks to have their data deleted. Privacy laws in most markets give users this right. The CIAM platform is where the request lands first, but the obligation extends to every downstream system that received the user's data via token claims.



















The Three Protocols: When to Use What


Before you open Auth0, know which protocol each persona needs. A client describes what they need and you should hear the protocol before they finish the sentence. That is what experience sounds like.



OIDC


Modern apps, mobile, single page apps. JSON tokens. The default for anything new. Answers who are you for modern systems. Alex Turner the B2C customer authenticates with OIDC.



SAML
Enterprise and partner SSO, legacy platforms. XML assertions. Answers who are you for established systems. Marcus Webb Ltd the B2B partner authenticates with SAML.



OAuth
API authorization. Not about identity, about permission. Answers what are you allowed to do. It carries scopes, not a user.






:::CIAM Reference Architecture:::


Same shape as the IGA pipeline from Week 1. The source and the rules are just different. Auth0 plays the platform in the middle today.


External Users


New Visitor, Returning Customer, Partner User



CIAM Platform


Self-Registration
Consent Management
Session Management
Token Issuance



Downstream Systems

Web App, Mobile App, API Gateway



:::Let's create the OIDC Application:::



-Create an application in Auth0 that represents the SimplifyTech Customer Portal. Alex Turner authenticates through this application.

-Go to Applications in the left menu

-Click Applications

-Click Create Application

-Name: SimplifyTech Customer Portal

-Type: Regular Web Application

-Click Create

-Go to the Settings tab

-Scroll to Application URIs

-Allowed Callback URLs: https://jwt.io

-Allowed Logout URLs: https://jwt.io

-Allowed Web Origins: https://jwt.io

-Click Save Changes







:::SELF REGISTRATION:::


Go through the registration flow yourself as Alex Turner. This is the journey every external user takes when they first arrive at your product.

Build your test URL
There is no built-in Try button in the current Auth0 UI. You construct the authorize URL manually. You need two values from your application:
Your tenant domain — visible at the top left of the Auth0 Dashboard (e.g. dev-abc123.us.auth0.com)
Your Client ID — visible on the Settings tab of the SimplifyTech Customer Portal application

Build the URL using this pattern:

https://YOUR-TENANT-DOMAIN/authorize
  ?client_id=YOUR-CLIENT-ID
  &redirect_uri=https://jwt.io
  &response_type=id_token
  &scope=openid%20profile%20email
  &nonce=testvalue123


Replace YOUR-TENANT-DOMAIN and YOUR-CLIENT-ID with your actual values. Put it all on one line with no spaces and paste it into a browser tab.

Go through the registration flow
Paste the URL into a browser tab and press Enter
The Auth0 Universal Login page opens
Click Sign Up
Enter a real email address you can access
Enter a password and click Continue
Check your email for an Auth0 verification email
Click the verification link in the email




:::ADDING GOOGLE SOCIAL LOGIN:::



Allow customers to sign in with Google. Standard in B2C CIAM. Reduces password friction and increases registration conversion.
Go to Authentication in the left menu
Click Social
Click Create Social Connection
Select Google / Gmail from the list — if it already exists click the three dots → Settings on google-oauth2
Leave the Client ID and Client Secret as the Auth0 dev defaults — these work for testing without needing a Google Cloud project
Click the Applications tab and toggle on SimplifyTech Customer Portal
Click Save

Test Google login
The Try button is available on the social connection itself, not on the application:
Go to Authentication → Social → Google / Gmail
Click on the Google connection
Click the Try button at the top right of the connection settings page
A popup opens showing the Auth0 login screen with Continue with Google
Click it and sign in with your own Google account
Auth0 shows the raw user profile it received from Google



