# aws-sec-userpool-google

https://youtu.be/r1P_glQGvfo

## Definition

- user pool: a collection of users
    - user = a set of attribute (email, first name)
    - user can be created in Cognito
    - user can come from third party provider    

## Cognito User Pools

1. AWS Cognito
- Manage User Pools
- Create a User Pool
    - Provide it a name
    - Review defaults
- Create an App client
    - App clients
        - Add an app client (leave all as default)
        - Create app client
- Create a Domain name (where client will authenticate)
    - App Integration > Domain Name
        - Add the prefix for the url (you can use a custom address instead of cognito one)
        - Save
    - App client Settings
        - [X] Cognito User Pool
        - Define the callback url on successful sign-in: https://www.example.com/cb
            - identity token & access token will be passed as parameter
        - Define the sign-out url: https://www.example.com/signout
        - OAuth 2.0
            - Authorization code grant : Requires a backend server that validates token. A temporary token is provided to get the real one in a second step.
            - Implicit grant : (simplest for Human interaction) does not require 3rd party, access token is returned directly in the header (can be intercepted by Man in the middle attack)
            - Client Credentials: Machine to machine credential
            - Allowed OAuth Scopes: phone, email, profile (defines authorization)
            - Save changes
        - Can now "Launch Hosted UI" (asks only or user and password, no Google integration)
                   
2. Google developer console

https://console.cloud.google.com/home/dashboard?hl=FR&project=theatomicity

- New Project: GoogleProjectSignOn
- Modify OAuth Consent Screen
- OAuth consent screen
    - External
    - App Information: GoogleSocialSignOn
    - User support email
    - Authorized Domain : amazoncognito.com
- Scopes
    - Save and continue
- Test and user
    - Save and continue

- Credentials
    - OAuth 2.0 client Ids
        - Create Credentials (on Top), OAuth client ID.
        - Application Type: Web application
        - Authorized JavaScript origins
            - Add Cognito URI : https://the-atomicity.auth.eu-west-1.amazoncognito.com/
        - Authorized redirect URIs
            - Add Cognito URI with redirect path : https://the-atomicity.auth.eu-west-1.amazoncognito.com/oauth2/idpresponse
        - Your get an client ID / client Secret
           
26:20
    
3. Back to AWS Cognito
- Federation
    - Identity Providers > Google
        - Google App Id: <web.client_id>
        - App Secret : <web.client_secret>
        - Score: profile email openid
        - Enable Google
    - Attribute Mapping
        - [X] Map email attribute to Email
- App integration:
    - App client settings
        - Enable Google identity provider : 
         - [X] Google
        
4. Test
- App integration:
    - App client settings
        - Launch Hosted UI
        - Continue with Google
        - Access Token is passed in callback : https://www.example.com/cb#id_token=<token>&expires_in=3600&token_type=Bearer
        - Token can be decoded with https://jwt.io/

A new user is added to 
- General Settings
    - Users and groups      
   