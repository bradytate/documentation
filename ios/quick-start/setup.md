# Geting started with Layer

```emphasis
If you are interested in trying out Atlas, Layer's open source user interface components, visit [Experience Atlas](/signup/atlas).
```
Layer is the full-stack building block for communications.</br>

This Quick Start guide will get you up and running with a project powered by Layer as fast as possible. When you're done, you'll be able to send messages between an iOS Device and Simulator, see typing indicators, and synchronize metadata. You will need a Layer account to complete this Quick Start guide so if you don't have an account yet, you can [sign up here](https://developer.layer.com/signup).

<video controls poster="https://s3.amazonaws.com/static.layer.com/web/docs/ios/quick-start.png" style="width:740px;">
  <source src="https://s3.amazonaws.com/static.layer.com/web/docs/ios/quick-start.mp4" type="video/mp4"/>
  <source src="https://s3.amazonaws.com/static.layer.com/web/docs/ios/quick-start.webm" type="video/webm"/>
</video>

## Set up the Quick Start project
There are two ways to set up the project. The easiest method is via our install script, or you can perform the steps manually. Both methods require [CocoaPods](http://cocoapods.org).

### Option 1: Script
If you execute this command in your terminal, we will place a version of the Quick Start Project in your `~/Downloads` folder, pre-configured with your Layer App ID:<br/>
```console
$ curl -L https://raw.githubusercontent.com/layerhq/quick-start-ios/master/install.sh | bash -s "%%C-INLINE-APPID%%"
```
### Option 2: Manual Instructions
Not a fan of scripts? That's OK, just follow these instructions:<br/>

1. Clone the project from GitHub:

  ```console
  $ git clone https://github.com/layerhq/quick-start-ios.git
  ```
2. Install the dependencies via CocoaPods:

  ```console
  $ pod install
  ```
3. Open QuickStart.xcworkspace in Xcode.
4. Replace `LAYER_APP_ID` in LQSAppDelegate.m (line 16) with the App ID. Your App ID can be found in the dashboard under the "Keys" section.</br>
  **Warning: If you skip this step you will get an error on app launch.**

## See Layer in Action
Open `Quickstart.xcworkspace` if it's not already open. Build and run the Quick Start application on a Simulator and a physical Device to start a 1:1 conversation between them.
### Send Messages
Go ahead and send a few messages between your Device and Simulator. In this example your message is just text, but with Layer you can send **anything**, as long as you can turn it into binary data, between participants.
### Typing Indicators
Start typing on your Device. The Simulator's text field will say "Device is typing..."</br>
This is an example of typing indicators in Layer. To see how easy it is to use them, check out the `textViewDidBeginEditing` and `textViewDidEndEditing` methods in `LQSViewController.m`
### Delivery and Read Receipts
Now that you've sent a few messages take a look at the status next to the Sender's name.  With Layer, you can easily see if a message has been sent, delivered, or read. Check out the `willDisplayCell` method in `LQSViewController.m` for information about read receipts.<br><br>
With Delivery and Read Receipts, you no longer need to wonder if a message was delivered or read.
### Offline Support
Next, try putting your Device in Airplane mode.  Send a message from the Simulator. The message will be marked as Sent in the Simulator.  Now, take the Device off airplane mode.  The message from the Simulator should now automatically appear on the Device, and the message will be marked as Read on the Simulator.<br><br>
Cool, right? You get offline support out of the box with Layer. Layer handles all network connectivity so you don't need to worry about it. No additional coding is necessary.
### Metadata
Shake your Device in your hand. Notice that the navigation bar changes color, **and** that the Simulator's navigation bar also changes color?<br><br>
This is another great feature that Layer provides: metadata. With metadata, you can attach any extra content that you want to a Conversation (e.g. a title for the Conversation), and that data will be synchronized between Participants.
To see how easy it is to add metadata to a Conversation, check out `motionEnded` method in `LQSViewController.m`.
### Rich Content
Now that you've sent a message, let's send a photo. Click on the Camera icon in the upper left corner.  Select a photo and tap Send. You can send any kind of payload with Layer: photos, gif's, videos, audio, even blocks of JSON. We will the store the content on our servers so you don't need to host it elsewhere. Layer also provides way to track the progress of an upload or download. To see how you can send photo in the code, check out the `sendMessage`  method in `LQSViewController.m`, and to see how to display the photo check out the `configureCell` method in  `LQSViewController.m`.
### Announcements
See that bell in the upper left corner? Tap on that and you'll see a list of what we call "Announcements". Announcements are messages that are sent directly to users, outside of the context of a conversation. They're great for telling two users that they have been matched in a dating app, broadcasting details about a special offer to a subset of users in a shopping app, informing all users of new features in your latest app update, etc. If you've launched the Quick Start app for the first time you probably won't have any announcements. Not to worry, sending announcements is easy with Layer's [Platform API](https://developer.layer.com/docs/platform). Here is a sample `curl` command you can execute from the terminal to quickly send an announcement to your device and simulator:
```console
$ curl  -X POST \
      -H 'Accept: application/vnd.layer+json; version=1.0' \
      -H 'Authorization: Bearer PLATFORM_API_TOKEN' \
      -H 'Content-Type: application/json' \
      -d '{"parts": [{"body": "Message from Platform API!", "mime_type": "text/plain"}], \
           "notification": {"text": "New Message from Platform API"}, \
           "sender": {"name": "Platform"}, \
           "recipients": ["Simulator","Device"]}' \
      https://api.layer.com/apps/APP_UUID/announcements
```
 Before sending this command, you will need to replace two fields: PLATFORM_API_TOKEN and APP_UUID. You can find these values under the "[Keys](https://developer.layer.com/projects/keys)" section of the developer dashboard. The PLATFORM_API_TOKEN must be generated as it is not created by default. Once you have executed the curl command, tap on the Bell icon again and you should see an announcement with the text "Message from Platform API!" from the sender "Platform". To learn more about announcements, check out the [Announcements](https://developer.layer.com/docs/platform#send-an-announcement) documentation. 
### Cross Platform Messaging
Also developing for Android? Build the [Android](/docs/android) Quick Start App using the same App ID, and your users will receive their messages no matter where they are!

## What's Next
Now that you've seen just a sample of what Layer can do, check out the [Integration](/docs/ios/integration) docs to learn some of the concepts behind Layer.<br/>
Interested in creating conversations and sending messages from your backend server?  The Layer Platform API is designed to empower developers to automate, extend, and integrate functionality provided by the Layer platform with other services and applications. For more information, check out the [Platform API](/docs/platform) docs.
