Immutable Passport Integration Guide
Creating a simple application
First, create a new directory for your application and navigate to it. Then, run the following command to initialize a new npm project:

npm init -y

Next, install the Passport SDK:

npm install @imtbl/sdk

Registering the application on Immutable Developer Hub:

To register your application on Immutable Developer Hub, go to the Immutable Developer Hub: https://hub.immutable.com/ and create an account. Once you have created an account, click on the Applications tab and click on the Create Application button.

Enter a name and description for your application and click on the Create button.

On the next page, you will be given your application's client ID and redirect URI. Copy these values and store them in a safe place.

Installing and initialising the Passport client
In your application, install the Passport SDK:

npm install @imtbl/sdk

Next, initialize the Passport client:

import { config, passport } from '@imtbl/sdk';

const passportInstance = new passport.Passport({
  baseConfig: new config.ImmutableConfiguration({
    environment: config.Environment.PRODUCTION,
  }),
  clientId: '<YOUR_CLIENT_ID>',
  redirectUri: '<YOUR_REDIRECT_URI>',
  logoutRedirectUri: '<YOUR_REDIRECT_URI>',
  audience: 'platform_api',
  scope: 'openid offline_access email transact',
});
Logging in a user with Passport
To log in a user with Passport, you can use the following code:

const passportInstance = new passport.Passport({
  // ...
});

const loginButton = document.querySelector('#login-button');

loginButton.addEventListener('click', async () => {
  const loginUrl = await passportInstance.getLoginUrl();
  window.location.href = loginUrl;
});

When the user clicks on the login button, they will be redirected to the Immutable Passport login page. Once they have logged in, they will be redirected back to your application with an authorization code in the URL.

You can then use the authorization code to obtain an access token and ID token from Passport:

const loginUrl = await passportInstance.getLoginUrl();
window.location.href = loginUrl;

// Get the authorization code from the URL
const authorizationCode = new URLSearchParams(window.location.search).get('code');

// Exchange the authorization code for an access token and ID token
const tokens = await passportInstance.exchangeCodeForTokens(authorizationCode);

// Store the access token and ID token in a secure place
Displaying on the app the id token, access token obtained from authenticating with Passport after login, and the user's nickname
Once you have obtained the access token and ID token, you can use them to display the user's nickname on your application:

const tokens = await passportInstance.exchangeCodeForTokens(authorizationCode);

// Get the user's nickname
const nickname = await passportInstance.getUserNickname();

// Display the user's nickname
const nicknameElement = document.querySelector('#nickname');
nicknameElement.textContent = nickname;
Logging out a user
To log out a user, you can use the following code:

const logoutButton = document.querySelector('#logout-button');

logoutButton.addEventListener('click', async () => {
  await passportInstance.logout();
});
When the user clicks on the logout button, they will be logged out of your application and Immutable Passport.

Initiating a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash
To initiate a transaction from Passport, you can use the following code:

const tokens = await passportInstance.exchangeCodeForTokens(authorizationCode);

// Create a transaction object
const transaction = new Transaction({
  to: '0x0000000000000000000000000000000000000000',
  value: 0,
  data: 'placeholder string',
});

//```



This application is very simple, but it demonstrates the basic steps involved in integrating Passport into your application. For more information, please see the Passport documentation: https://docs.immutable.com/docs/x/passport.

GitHub repository link
The guide and sample application can be found in the following GitHub repository:

https://github.com/AkhilaReddy1513/Sample-passport-Integration
