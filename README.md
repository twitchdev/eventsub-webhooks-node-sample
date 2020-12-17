# Sample Code for Twitch EventSub Webhooks using NodeJS

The purpose of this sample is to provide guidance on how to start using Twitch's new webhooks that are a part of [EventSub](https://dev.twitch.tv/docs/eventsub). The sample is built in NodeJS using the Express.js framework.

**This sample will cover**

1. Webhook Subscription creation
2. Signature Verification
3. Handling the callback verification challenge
4. Handling webhook event data

## How to Use
### Installation
Run `npm install` to install the proyect's dependencies.

### Setting an https endpoint
All callback URLs set for Twitch's webhook notifications need to support https. To facilitate developmemnt in a local environment we recommend setting up an ngrok tunnel. Follow [this link](https://ngrok.com/download) and download the latest version for your OS.

Once you have ngrok installed run `./ngrok http 3000` to start a tunnel.

### Set .env file
Create a .env file with the following variables

```
TWITCH_CLIENT_ID=
TWITCH_ACCESS_TOKEN=
NGROK_TUNNEL_URL=
```
Set the above values accordingly based on your Twitch app and the ngrok tunnel created in the previous step.

### Start the Server
run `npm start` to run the sample.


## Webhook Subscription Creation
Once the server is up and running you can send a POST to `http://localhost:3000/createWebhook/<twitchID>` to create a new *channel.follow* webhook. 

You can modify the *createWebHookBody* payload in the index.js to try out other webhooks as well.

In order to create a new webhook subscription you'll need to send a POST request to the https://api.twitch.tv/helix/eventsub/subscriptions endpoint.

Once the subscription is created you should expect an almost immediate call to you callback URL with a challenge that will be logged into the console and echoed back to Twitch for the process to be completed.

## Signature Verification
Everytime you callback is reached by Twitch we will include a sha256 signature encrypted with the secret you provided during subscription creation. Signature verification is done by crafting the same message that is encrypted on Twitch's side; signing the crafted message with the shared secret; and comparing if the messages match.

We are covering this step in the `verifySignature` function in the index.js file.

## Handling Notifications
The `app.post('/notification',...)` function will handle all notifications sent to your callback URL. It will reply to Twitch with the proper responses to complete the webhook flows succesfuly plus logging payloads to the console.
