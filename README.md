# Retrieve SMS history from Pipedream

This npm library enhances Cypress testing capabilities by providing convenient commands for retrieving received SMS history from Pipedream.  
Simplify your end-to-end testing workflow by effortlessly integrating SMS history verification into your Cypress tests.

<h3 align="center">
  <a href="https://www.npmjs.com/package/cypress-pipedream-plugin">
    <img src="https://img.shields.io/npm/v/cypress-pipedream-plugin" align="center" />
  </a>
  <a href="https://www.npmjs.com/package/cypress-pipedream-plugin">
    <img src="https://img.shields.io/npm/dm/cypress-pipedream-plugin"  align="center" />
  </a>
  <a href="https://paypal.me/AlecMestroni?country.x=IT&locale.x=it_IT">
      <img src="https://raw.githubusercontent.com/alecmestroni/cypress-xray-junit-reporter/main/img/badge.svg" align="center" />
  </a>
</h3>

## Install

```shell
$ npm install cypress-pipedream-plugin --save-dev
```

Alternatively, you can install it as a global module:

```shell
$ npm install -g cypress-pipedream-plugin
```

## Prerequisites

A pipedream source: https://pipedream.com/sources/new.  
Pay attention to set the Body Only option to true when creating the source, if u haven't and you have already created the Pipedream-Source enables it here: https://pipedream.com/sources/{your-source}/configuration

![](https://raw.githubusercontent.com/alecmestroni/cypress-pipedream-plugin/main/img/SourceTahBodyOnly.png)

## Added commands

| Commands                          | Notes                                                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| cy.getLastMessage()               | Attempts to retrieve last (NEWEST) SMS for a specific Pipedream Source History within a specified time limit.  |
| cy.getFirstMessage()              | Attempts to retrieve first (OLDEST) SMS for a specific Pipedream Source History within a specified time limit. |
| cy.clearMessagesHistory()         | Clears the SMS history for a specific Pipedream Source.                                                        |
| cy.clearHistorySendSmsAndGetSMS() | This command simplifies the usage by executing the following commands in sequence:                             |

```javascript
cy.clearMessagesHistory() //
cy.get('[data-cy="send-sms"]', { timeout: 120000 }).click() // Command to send the SMS from the frontend
cy.getLastMessage()
```

According to [cypress documentation](https://docs.cypress.io/guides/references/best-practices#Selecting-Elements) adding the data-cy tag to your frontend, is the best way to keep your test updated and maintainable.

## Configuration

### Inside `cypress/support/e2e.js`

At the top of your support file (usually cypress/support/e2e.js for e2e testing type):

```javascript
import 'cypress-pipedream-plugin'
```

### Inside your environment file

You MUST set these environment variables to make this plugin working

| Parameter           | Mandatory | Notes                                                     | Default                         |
| ------------------- | --------- | --------------------------------------------------------- | ------------------------------- |
| pipedreamBearer     | TRUE      | Bearer used for Pipedream Auth                            | \                               |
| pipedreamUrl        | TRUE      | Your Pipedream Source URL                                 | \                               |
| pipedreamMaxRetries | FALSE     | Max retires value for the command cy.getMessagesHistory() | 120 (seconds)                   |
| pipedreamFolderPath | FALSE     | Folder where the SMS body will be saved                   | 'cypress/fixtures/sms-response' |

To set these variables dynamically in a multi environment cypress-test, you can use the following plugin:
[cypress-env](https://www.npmjs.com/package/cypress-env)

### Considerations

SMSs that are to be tested, in most cases should contain OTP or ULR.  
OTP will be saved in a file named OTP.json, while ULR will be saved in a file named ULR.json.  
To check the file contents, run the following command:

```javascript
cy.readFile('cypress/fixtures/sms_response/OTP.json', {
	timeout: 60000,
	retries: 3,
}).then((otp) {
  ... // do something with the saved OTP or URL
})
```

### Missing configuration error:

```
────────────────────────────────────────────────────────────────────────────────────────────────────

  Running:  myFirstTest.cy.js                                                               (1 of 1)

    An uncaught error was detected outside of a test (Attempt 1 of 2)
    Error: The following error originated from your test code, not from Cypress.
  1) An uncaught error was detected outside of a test

  0 passing (1s)
  1 failing

  1) An uncaught error was detected outside of a test:
     Error: The following error originated from your test code, not from Cypress.

  > CYPRESS-PIPEDREAM-PLUGIN | Missing environment variables: env('pipedreamBearer') & env('pipedreamUrl') needed

────────────────────────────────────────────────────────────────────────────────────────────────────
```

## THE JOB IS DONE!

Happy testing to everyone!

ALEC-JS

<h3 align="center">
🙌 Donate to support my work & further development! 🙌
</h3>

<h3 align="center">
  <a href="https://paypal.me/AlecMestroni?country.x=IT&locale.x=it_IT">
    <img src="https://raw.githubusercontent.com/alecmestroni/cypress-xray-junit-reporter/main/img/badge.svg" width="111" align="center" />
  </a>
</h3>
