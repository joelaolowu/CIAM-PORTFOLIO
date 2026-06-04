

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






I SPENT A WEEK WITH THE PRODUCT DESIGN TEAMS AND OTHER STAKE HOLDERS(PRODUCT OWNERS, UX DESIGNERS, AUDIT TEAM, BUSINESS OWNERS, IT SECURITY AND CIAM TEAM) ON THE USERS PERSONAS, THE BUSINESS, THE COST EFFECTIVE CIAM PLATFORM, THE TARGET CUSTOMERS AND CUSTOMERS RETENTION. THE MAJOR CIAM PRODUCTS AND VENDORS CONSIDERED WERE:

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




5, Password Reset: The Forgotten Password User Journey


Password reset is one of the highest-volume user journeys in any CIAM deployment. A poorly designed reset flow generates support tickets, causes customer dropout, and is a common attack vector. Auth0 handles the mechanics — your job is to understand what happens at each step and how it affects the user experience.

Why this matters beyond the lab:

Password reset is not just a feature. It is a trust moment.
If the reset email takes 5 minutes to arrive: customers assume something is broken and contact support.
If the reset link expires too quickly: customers have to start over and often give up.
If there is no rate limiting on reset requests: attackers can flood a user's inbox as a harassment tactic.

In a real CIAM project you document the reset flow, agree on link expiry with the security team,
and test the email delivery time in every target market before go-live.


The Password Reset User Journey for Alex Turner
This is the flow a customer goes through when they forget their password:

Alex tries to log in → enters email → clicks Forgot Password
→ Auth0 sends a password reset email to alex@example.com
→ Alex clicks the link in the email
→ Universal Login page shows password reset form
→ Alex enters new password and confirms
→ Auth0 updates the credential
→ Alex is redirected to log in with the new password
→ Login succeeds → JWT issued


Step 5a: Trigger a password reset from the Universal Login page

Use the authorize URL you built in Step 3. This time go through the forgot password flow instead of signing in.
Paste the authorize URL into a browser tab
-The Universal Login page opens

-Enter the email of the user you created in Step 3

-Click Continue

-On the password screen: click Forgot Password

-Auth0 shows: We have sent you an email to reset your password

-Check your email for the Auth0 password reset email

-Click the Reset Password link in the email

-The Universal Login page shows a new password form

-Enter a new password and confirm it

-Auth0 confirms: Your password has been reset successfully




✅  Verify: Password reset complete. You can now log in with the new password using the authorize URL.


Step 5b: Configure the password reset email template

Auth0 sends the password reset email using a default template. You can customise the sender name, subject line, and body.

-Go to Branding in the left menu

-Click Email Templates

-Select Change Password from the dropdown

-Review the default template

-Change the From name to: SimplifyTech

-Change the Subject to: Reset your SimplifyTech password

-Click Save

✅  Verify: Email template updated. From name: SimplifyTech. Subject line updated.



Step 5c: Review the password policy

-Auth0 enforces password complexity rules. Review what is configured on your tenant.

-Go to Authentication → Database → Username-Password-Authentication

-Click the Password Policy tab

-Note the current Password Strength setting

-Check Password History: how many previous passwords are remembered

-Check Password Dictionary: blocks common passwords like Password123

-Password policy decisions in CIAM are a balance between security and conversion.

-Too strict: customers cannot create a password that passes and abandon registration.

-Too loose: weak passwords increase account takeover risk.

-Common enterprise CIAM policy: minimum 8 characters, at least one number, one uppercase.

-Common B2C policy: minimum 8 characters with a strength indicator (weak/fair/strong).

-Password history of 5: prevents users from cycling through the same passwords on every reset.

-Do not block copy-paste on the password field.

-It prevents password manager usage and increases weak password adoption.





:::What the customer sees vs what you see in Auth0:::

The customer sees: a simple Forgot Password link, an email, and a form to enter a new password.
They never see: the Auth0 Dashboard, the email template editor, or the password policy settings.

Auth0 is infrastructure. The customer experience is Universal Login page plus the email.
Every customisation you make in the Dashboard affects what the customer sees in those two surfaces.



6,Terms and Conditions Enforcement



Privacy laws across the world require that users explicitly agree to how their data will be used before you process it. T&C acceptance is both a legal requirement in most markets and a trust signal for your customers. The specific law varies by jurisdiction. The technical implementation in your CIAM platform is the same everywhere: gate access until the user has accepted, record the acceptance with a timestamp and version, and make it as easy to withdraw as it was to give.

What is an Auth0 Action?

An Auth0 Action is a serverless JavaScript function that runs at a specific point in the identity pipeline.
For the Login flow, your Action runs after the user successfully authenticates but before Auth0 issues the token.
This gives you a gate: you can inspect the user's profile, add custom claims to the token, or deny the login entirely.

The Action you are about to create does three things:

1. Reads app_metadata.tc_accepted and app_metadata.tc_version from the user's profile
2. If they match the expected version: adds tc_accepted and tc_version as custom claims to the JWT
3. If they do not match: blocks the login with an access_denied error

Students never see Actions in the token unless you explicitly add them with api.idToken.setCustomClaim.
That is the bridge between what is in the user profile and what appears in the JWT.


Step 6a: Create the Action

-Go to Actions in the left menu

-Click Library

-Click Create Custom Action (or Build Custom)


Name: Enforce T&C Acceptance

-Trigger: Login / Post Login — Node 22

-Click Create

-Replace all code in the editor with this:

exports.onExecutePostLogin = async (event, api) => {
  const TC_VERSION = '1.0';

  // First login: user has just registered.
  // They have not had a chance to accept T&C yet.
  // Allow through and set tc_required so the app knows to show the T&C page.
  if (event.stats.loginsCount <= 1) {
    api.idToken.setCustomClaim('tc_required', true);
    return;
  }

  const accepted = event.user.app_metadata?.tc_accepted;
  const acceptedVersion = event.user.app_metadata?.tc_version;

  // Returning user who has not accepted T&C: block login.
  if (!accepted || acceptedVersion !== TC_VERSION) {
    api.access.deny('Terms and Conditions not accepted.');
    return;
  }

  // Accepted: add claims to the JWT.
  api.idToken.setCustomClaim('tc_accepted', true);
  api.idToken.setCustomClaim('tc_version', TC_VERSION);
};


Click Deploy

Why loginsCount <= 1 for new users?

event.stats.loginsCount is how many times this user has completed a login.
A brand new user who just registered has loginsCount = 1 on their first login.
They have never seen a T&C page yet — blocking them at this point would be a bad experience.
So we let them through on the first login and set tc_required: true in the token.
The downstream app sees tc_required: true and shows them the T&C page.
When they accept, your backend calls the Auth0 Management API to write tc_accepted: true to app_metadata.
On their next login, loginsCount = 2 and the full check runs.

This is the correct production pattern for first-time T&C enforcement.



Step 6b: Attach the Action to the Login flow

-Go to Actions in the left menu

-Click Triggers

-Find Post Login in the list and click it

-On the flow diagram, find Enforce T&C Acceptance in the Add Action panel on the right under Custom

-Drag it into the flow between Start and Complete
-Click Apply to save changes (top right)



Step 6c: Test with a new user (first login)


-Use the authorize URL you built in Step 3 and log in as your test user created in Step 3.

-This is their first login after registration (loginsCount = 1). The Action should allow them through.

-After login you are redirected to jwt.io. In the decoded payload you should see:
"tc_required": true
✅  Verify: jwt.io shows decoded token with tc_required: true in the payload. No error. Login succeeded.




Step 6d: Simulate T&C acceptance — add app_metadata to the user

In production your application shows a T&C page and calls the Auth0 Management API to update app_metadata when the user accepts. In the lab you do this manually to simulate that flow.

Why are you editing this in Auth0 directly?

app_metadata is the field in Auth0 where your backend application stores data about the user.
Only the application can write to app_metadata — users cannot modify it themselves.
In production, when Alex accepts T&C on your consent page, your backend makes an API call:
PATCH /api/v2/users/{id} with body: { app_metadata: { tc_accepted: true, tc_version: '1.0' } }

In the lab you are acting as the backend application and writing this data manually.
This is exactly what the backend would write — you are just doing it through the UI instead of an API call.



Go to User Management → Users → click your test user

-Scroll down to the app_metadata section

-Click the pencil icon to edit

-Replace the content with:

{
  "tc_accepted": true,
  "tc_version": "1.0",
  "tc_accepted_date": "2026-05-30"
}



-Click Save





Step 6e: Test the second login (returning user with T&C accepted)

-Paste the authorize URL into a new browser tab and log in as the same user again.

-This is now their second login (loginsCount = 2). The full T&C check runs.
✅  Verify: jwt.io shows decoded token with tc_accepted: true and tc_version: 1.0 in the payload.
What you just built in production terms:


-First login: user gets through with tc_required: true. App shows T&C page. User accepts.

-Backend writes tc_accepted: true and tc_version: 1.0 to app_metadata.

-Second login: Action reads app_metadata, verifies version matches, adds claims to JWT.

-Downstream app sees tc_accepted: true and grants full access.

When T&C update to version 2.0: change TC_VERSION in the Action to '2.0' and redeploy.
Every user who accepted 1.0 will be blocked (tc_version mismatch) until they accept 2.0.
This is the version enforcement mechanism. One change in the Action enforces re-acceptance for millions of users.


7, Controlled Failure: Break the Callback, Read the Error


You set the callback URL in Step 2. Now break it on purpose. Recognising this error in one second is one of the most useful things you carry out of today, because callback mismatch is the most common CIAM login support ticket in the real world.

-Break It

-Go to Applications, SimplifyTech Customer Portal, Settings

-Change Allowed Callback URLs to a wrong value, for example https://jwt.io/wrong

-Save Changes

-Run your authorize URL from Step 3 and attempt to log in



-Observe It


-Auth0 refuses with a callback mismatch error and the login never completes

-Read the error text. This is exactly what a stranded customer hits when an app is misconfigured



-Fix It

-Set Allowed Callback URLs back to https://jwt.io exactly

-Save Changes, run the login again, confirm it succeeds
✅  Verify: Login fails with a callback mismatch when the URL is wrong, and succeeds again once the exact URL is restored.
Callback mismatch is the number one CIAM login error in production. The cause is almost always http versus https, a trailing slash, or a typo. Auth0 only sends tokens to URLs on the allowed list, so the fix is always to make the allowed list match the application exactly.



:::Block 2: SSO Flow and JWT Decode:::





Trigger a full OIDC login. Capture the token. Decode every claim. Understand what the app sees.


8, Trigger the SSO Flow and Decode the JWT


Use the authorize URL you built in Step 3. Log in as your test user who has tc_accepted in app_metadata.

Build the same URL with your tenant domain and client ID:

https://YOUR-TENANT-DOMAIN/authorize
  ?client_id=YOUR-CLIENT-ID
  &redirect_uri=https://jwt.io
  &response_type=id_token
  &scope=openid%20profile%20email
  &nonce=testvalue123


-Paste the URL into a browser tab

-Log in as your test user (the one with tc_accepted in app_metadata from Step 6d)

-After login, Auth0 redirects to https://jwt.io
jwt.io automatically decodes the token — the payload appears on the right side

✅  Verify: Token decoded at jwt.io. Payload visible. tc_accepted: true and tc_version: 1.0 visible in payload.

:::Map every claim in the JWT :::

Claim

Example Value

What it means in CIAM
sub
auth0|abc123def
Unique user ID. Immutable. Never reused. This is how apps identify the user across sessions.
iss
https://dev-abc.us.auth0.com/
Your Auth0 tenant. The app verifies this before trusting the token.
aud
your-client-id
Which application this token is for. App rejects tokens intended for other apps.
exp
1748608800
Token expiry timestamp. CIAM tokens are short-lived — typically 1 hour.
iat
1748605200
Issued at timestamp. Token age = exp minus iat.
email
alex@example.com
User email. Only present if email scope was requested.
email_verified
true
Has the user verified their email. Critical trust signal in CIAM.
tc_accepted
true
Custom claim. Added by our T&C Action. App checks before granting access.
tc_version
1.0
Which T&C version was accepted. Forces re-acceptance when updated to 2.0.








Block 3: SAML B2B Federation


Federate the partner persona. Capture and decode a real SAML assertion.


Alex Turner the consumer authenticated with OIDC. Marcus Webb Ltd the B2B partner authenticates with SAML, because partner and enterprise systems still run on it. Same job as OIDC, who are you, but an XML assertion instead of a JSON token. Now you build it and read a real assertion by eye.

10, Create a SAML Application and Enable the SAML2 Addon

This application makes Auth0 act as the SAML Identity Provider for the partner. In production the partner would run their own IdP and Auth0 would be the SP. Today you are both sides so you can see the assertion.
Go to Applications, Create Application
Name: SimplifyTech Partner Portal, Type: Regular Web Application, Create
Open the Addons tab on the new application
Toggle on SAML2 Web App
In the addon Settings, set Application Callback URL to https://jwt.io so the assertion posts to a page you can see
Click Enable, then Save
Open the Usage tab, note the Identity Provider Login URL, and download the metadata. Open the metadata and find the signing certificate
✅  Verify: SimplifyTech Partner Portal created, SAML2 Web App addon enabled, metadata downloaded, signing certificate visible in the metadata XML.
Auth0 is the IdP here, issuing a signed assertion. In production the partner brings their own IdP and Auth0 becomes the SP that consumes it. Same protocol, reversed direction. The signing certificate in the metadata is what lets the SP trust the assertion.


11, Capture and Decode the SAML Assertion with SAML-tracer

SAML-tracer is a Chrome extension that records every SAML message as you browse, so you can read the raw assertion the moment it is sent. This is the SAML version of the jwt.io decode you did in Step 8.
Add SAML-tracer to Chrome from the Chrome Web Store
Open SAML-tracer from the extensions menu so it is recording
Open the Identity Provider Login URL from the addon Usage tab to start the SAML flow
Log in as your test user
In SAML-tracer, find the request carrying the SAML POST and open the SAML tab to see the decoded XML
Read the NameID, the AttributeStatement, and the Signature block



12, Enable Brute Force Protection


Go to Security in the left menu
Click Attack Protection
Click Brute Force Protection
Toggle it on
Maximum failed login attempts: 10
Policy: Block the user account for 10 minutes
Save

✅  Verify: Brute Force Protection enabled. Threshold: 10 attempts. Block for 10 minutes.
Production context: 10 attempts is a starting point.
Too low: legitimate customers get locked out when they forget their password on a mobile device.
Too high: attackers get too many attempts before being blocked.
The helpdesk implication: poor brute force configuration generates hundreds of unlock tickets per day.
Getting this threshold right after go-live based on real support volumes is a routine tuning task.



13, Right to Erasure: Delete a User


A customer requests deletion of their data. Most privacy laws globally give users this right. Your CIAM platform is the starting point for honouring the request.

Go to User Management → Users
Click on any test user you want to remove
Click the three dots menu at the top right of the user page
Click Delete User
Confirm deletion

✅  Verify: User deleted from Auth0. No user record remains.


Deleting the Auth0 user is step one. A complete erasure workflow also requires:
Delete or anonymise the user's data from your CRM
Remove from email marketing platform
Anonymise analytics events associated with their sub claim
Log the deletion with timestamp and requestor for the audit trail
Confirm to the user within the legally required timeframe for their jurisdiction







:::What I Built:::




-Defined three external user personas and mapped Alex Turner's full identity flow before touching Auth0


-Designed the user attribute schema separating core profile, user_metadata, and app_metadata



-Created SimplifyTech Customer Portal OIDC application with jwt.io as callback



-Went through self-registration as Alex Turner — confirmed user created in Auth0



-Added Google social login and tested sign-in with a Google account



-Built a T&C Login Action that allows first-time users through and blocks returning users who have not accepted



-Simulated T&C acceptance by writing app_metadata manually — understanding what the backend does in production



-Triggered full OIDC login and decoded the JWT at jwt.io — mapped every claim to a CIAM concept


-Enabled brute force protection and understood the threshold decision



-Performed a right to erasure deletion and understood the full scope of the obligation



-Broke a callback URL and recognised the mismatch error in one second



-Configured Auth0 as a SAML IdP and decoded a raw SAML assertion with SAML-tracer


--Set up a free Salesforce Developer Edition account at developer.salesforce.com/signup.



--Configure a Connected App in Salesforce and Auth0 as the OIDC Identity Provider.



--Test the SSO flow and post a screenshot of the login working in Skool Lab Help.








