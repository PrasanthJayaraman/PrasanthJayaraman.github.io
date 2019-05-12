---
title:  "NodeJs - Send Push Notifications to devices using firebase"
categories: 
  - Javascript
tags:
  - NodeJs
comments: true
---

You can send push notifications to any android devices using [**Firebase**](https://firebase.google.com/). I will explain you how to get the key from the firebase project and write a simple nodejs code using library **fcm-node** to send push notifications to android devices.

## Requirements

To start with, You should have a firebase account, i will consider you have an firebase account else create one. 

Go to [Firebase Console](https://console.firebase.google.com/) and select your project or create a project. Select the **Settings icon** and click **Project settings**

[![screenshot1](https://prasanthj.com/assets/uploads/firebase-1.png)](https://prasanthj.com/assets/uploads/firebase-1.png)

Select **Could Messaging** tab and copy **Server API** key, we need to use this in nodejs applciation.

[![screenshot2](https://prasanthj.com/assets/uploads/firebase-2.png)](https://prasanthj.com/assets/uploads/firebase-2.png)

---

Now install the [**fcm-node**](https://www.npmjs.com/package/fcm-node) in your nodejs project.

```javascript
    npm install fcm-node
```

This is a simple nodejs firebase cloud messaging library which sends push to one or multiple devices using a function ```fcm.send```, it also supports promises instead callbacks. Do not forget to get the android device token from your android device and save it in your database.

```javascript
    // Notifications.js

    var FCM = require('fcm-node');
    var serverKey = 'YOURSERVERKEYHERE'; // put your server key here
    var fcm = new FCM(serverKey);
 
    var message = { //this may vary according to the message type (single recipient, multicast, topic, et cetera)
        to: 'registration_token', 
        collapse_key: 'your_collapse_key',
        
        notification: {
            title: 'Title of your push notification', 
            body: 'Body of your push notification' 
        },
        
        data: {  //you can send only notification or only data(or include both)
            my_key: 'my value',
            my_another_key: 'my another value'
        }
    };
    
    fcm.send(message, function(err, response){
        if (err) {
            console.log("Something has gone wrong!");
        } else {
            console.log("Successfully sent with response: ", response);
        }
    });
```

You can collapse multiple notifcations of same application in your android device with ```collapse_key``` and maximum of 4 collapse key is used. 

With ```fcm-node``` you can send notifications to multiple devices by replacing ```to``` with ```registration_ids``` in the message object.

```javascript
    var message = { 
        registration_ids: ['registration_tokens'], // Multiple tokens in an array
        collapse_key: 'your_collapse_key',
        
        notification: {
            title: 'Title of your push notification', 
            body: 'Body of your push notification' 
        },
        
        data: {  //you can send only notification or only data(or include both)
            my_key: 'my value',
            my_another_key: 'my another value'
        }
    };
```

**Note:** Device registration token in android will be updated some time so make sure you are sending to the notifications with latest device tokens.