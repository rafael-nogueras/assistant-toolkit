# Enabling security for Watson Assistant web chat

**For a full walk through of how this code works, please visit the [tutorial page](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-web-chat-develop-security) in the Watson Assistant documentation or view the [security overview](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-web-chat-security).**

This code is for extending the Watson Assistant web chat. If you are new to developing with web chat, please start with the [web chat development overview](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-web-chat-develop). The code in this folder is commented with links and references to the web chat APIs used.

This example demonstrates how to enable security with web chat. It will show how to create a JWT that can be used to securely authorize a webpage to access your web chat. It also demonstrates how to use a new JWT for the use case when a user logs into to a site in the middle of a Watson Assistant chat session as well as how to send the user's ID to Watson Assistant so it can be used in an action or an extension.

It demonstrates:

- How to generate public and private keys that are required for creating JWTs.
- How to use a NodeJS Express server for creating a JWT from your private key.
- How to use an [**identityTokenExpired**](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#identityexpired) event handler to request a new JWT from your server.
- How to use the [**updateIdentityToken**](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updateidentity) instance function to send update web chat with a new JWT when the user has changed.
- How to set the user ID used by web chat.
- How to encrypt a payload in a JWT so that secret information can be sent to the assistant.
- How to use a custom variable to change the user ID in the middle of a session.

The server code for generating a JWT can be found in [createJWT.js](server/nodejs-express/routes/createJWT.js). The web chat code that uses the JWT can be found in [index.html](server/nodejs-express/static/index.html).

## Running the Code

**DO NOT USE THE PUBLIC AND PRIVATE KEYS FROM THIS EXAMPLE FOR PRODUCTION USE!** You should generate your own public and private keys.

If you do not have a public and private key pair, you can generate one using the following commands:
```
ssh-keygen -t rsa -b 4096 -m PEM -f jwtRS256.key
openssl rsa -in jwtRS256.key -pubout -outform PEM -out jwtRS256.key.pub
```

### Running the Server

This example requires having NodeJS installed.

The server is required for creating the JWTs that are required to enable security and for authenticating a user. It serves requests from `http://localhost:3001`.

1. `cd server/nodejs-express`
2. `npm install`
3. `npm run start`
4. The server will be available at `http://localhost:3001`.

### Running the JavaScript Example

- After starting the server, open [http://localhost:3001/index.html](http://localhost:3001/index.html).
- Open web chat and click or type "Send something secure".
- Click the login/logout button to log in or out of the site and use web chat to send "Send something secure" to see Watson Assistant get updated with the changing user ID.

## Setting up your own assistant

This example is configured to use an existing assistant set up for common use by anyone running this example. If you want to set up your own assistant, you'll need to perform the steps below.

- Import the [actions.json](actions.json) file located in the repository for this example into your assistant.
- Modify the `integrationID`, `region`, `serviceInstanceID` and `subscriptionID` (only for enterprise accounts) in the `watsonAssistantChatOptions` used in this example to match those in the web chat embed code for your assistant.
- Copy your public key into the file `server/nodejs-express/keys/jwtRS256.key.pub` and copy your private key into the file `server/nodejs-express/keys/jwtRS256.key`.
- Open the Security tab for the web chat settings page.
- Copy your public key into the "Your public key" field.
- Copy the "IBM provided public key" into the file `server/nodejs-express/keys/ibmPublic.key.pub`.
