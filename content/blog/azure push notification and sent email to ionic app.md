---
title: azure push notification and sent email to ionic app
date: "2015-05-28T22:40:32.169Z"
tags: [azure,ionic,app] 
description: [hybrid app with ionic]
---


------
Ionic is a powerful HTML5 native app development framework that helps you build native-feeling mobile apps all with web technologies like HTML, CSS, and Javascript.Ionic is focused mainly on the look and feel, and Integration Cordova、Angular、Node、Grunt、Bowser、Sass .Mixed development solution.

## [Push Notifications to PhoneGap Apps using Notification Hubs Integration][1]
Ionic apps have some access to native resources, such as push notifications, the accelerometer, camera, storage, geolocation, the in-app browser. Ionic apps feel a bit like Web apps, but often with the behavior of native device apps.
To learn how to use Mobile Services and Azure Notification Hubs to send push notifications to a Ionic app,see [TodoList notifications sample for PhoneGap][2]
tips：

> * To develop an app using the Google Play services APIs, you need to set up your project with the Google Play services SDK.(here means Android SDK should installed Google play Service)

**Using Google Cloud Messaging**
Google Cloud Messaging (GCM) is a service for Android devices to send and receive Android push notification messages. The typical flow looks like: 
![此处输入图片的描述][3]

------

## Email Service 
[Send email from Mobile Services with SendGrid][4]
Your can also use the [Amazon SES][5]


------

**Further reading：**
[Google Cloud Messaging for Android][6]


  [1]: http://blogs.msdn.com/b/azuremobile/archive/2014/06/17/push-notifications-to-phonegap-apps-using-notification-hubs-integration.aspx
  [2]: https://github.com/Azure/mobile-services-samples/tree/master/TodoListNotifications
  [3]: http://img555.qiniudn.com/gcm
  [4]: http://azure.microsoft.com/en-us/documentation/articles/store-sendgrid-mobile-services-send-email-scripts/#sign-up
  [5]: http://aws.amazon.com/cn/ses/developer-resources/
  [6]: http://developer.android.com/google/gcm/index.html

