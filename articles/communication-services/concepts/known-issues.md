---
title: Azure Communication Services - known issues
description: Learn more about Azure Communication Services
author: rinarish
manager: jken
services: azure-communication-services

ms.author: mikben
ms.date: 03/10/2021
ms.topic: troubleshooting
ms.service: azure-communication-services
---

# Known issues: Azure Communication Services Calling SDKs
This article provides information about limitations and known issues related to the Azure Communication Services Calling SDKs.

> [!IMPORTANT]
> There are multiple factors that can affect the quality of your calling experience. Refer to the **[network requirements](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/network-requirements)** documentation to learn more about Communication Services network configuration and testing best practices.


## JavaScript SDK

This section provides information about known issues associated with the Azure Communication Services JavaScript voice and video calling SDKs.

### Refreshing a page doesn't immediately remove the user from their call

If a user is in a call and decides to refresh the page, the Communication Services media service won't remove this user immediately from the call. It will wait for the user to rejoin. The user will be removed from the call after the media service times out.

It's best to build user experiences that don't require end-users to refresh the page of your application while in a call. If a user refreshes the page, reuse the same Communication Services user ID after they return back to the application.

From the perspective of other participants in the call, the user will remain in the call for period of time (1-2 minutes). 
If the user rejoins with the same Communication Services user ID, they'll be represented as the same, existing object in the `remoteParticipants` collection.

If the user was sending video before refreshing, the `videoStreams` collection will keep the previous stream information until the service times out and removes it. In this scenario, the application may decide to observe any new streams added to the collection and render one with the highest `id`. 


### It's not possible to render multiple previews from multiple devices on web
This is a known limitation. For more information, refer to the [calling SDK overview](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/calling-sdk-features).

### Enumerating devices isn't possible in Safari when the application runs on iOS or iPadOS

Applications can't enumerate/select mic/speaker devices (like Bluetooth) on Safari iOS/iPad. This is a known operating system limitation.

If you're using Safari on macOS, your app will not be able to enumerate/select speakers through the Communication Services Device Manager. In this scenario, devices must be selected via the OS. If you use Chrome on macOS, the app can enumerate/select devices through the Communication Services Device Manager.

### Audio connectivity is lost when receiving SMS messages or calls during an ongoing VoIP call
This problem may occur due to multiple reasons:

- Some mobile browsers don't maintain connectivity while in the background state. This can lead to a degraded call experience if the VoIP call was interrupted by an event that pushes your application into the background. 
- Sometimes, an SMS or PSTN call captures the audio sound, and doesn't release audio back to the VoIP call. Apple fixed this issue in iOS versions 14.4.1+. 

<br/>Client library: Calling (JavaScript)
<br/>Browsers: Safari, Chrome
<br/>Operating System: iOS, Android

### Repeatedly switching video devices may cause video streaming to temporarily stop

Switching between video devices may cause your video stream to pause while the stream is acquired from the selected device.

#### Possible causes
Switching between devices frequently can cause performance degradation. Developers are encouraged to stop one device stream before starting another.

### Bluetooth headset microphone is not detected therefore is not audible during the call on Safari on iOS
Bluetooth headsets aren't supported by Safari on iOS. Your Bluetooth device will not be listed in available microphone options and other participants will not be able to hear you if you try using Bluetooth over Safari.

#### Possible causes
This is a known macOS/iOS/iPadOS operating system limitation. 

With Safari on **macOS** and **iOS/iPadOS**, it is not possible to enumerating/selecting speaker devices through the Communication Services Device Manager since speakers enumeration/selection is not supported by Safari. In this scenario, your device selection should be updated via the operating system.

### Rotation of a device can create poor video quality
Users may experience degraded video quality when devices are rotated.

<br/>Devices affected: Google Pixel 5, Google Pixel 3a, Apple iPad 8, and Apple iPad X
<br/>Client library: Calling (JavaScript)
<br/>Browsers: Safari, Chrome
<br/>Operating System: iOS, Android


### Camera switching makes the screen freeze 
When a Communication Services user joins a call using the JavaScript calling SDK and then hits the camera switch button, the UI may become unresponsive until the application is refreshed or browser is pushed to the background by user.

<br/>Devices affected: Google Pixel 4a
<br/>Client library: Calling (JavaScript)
<br/>Browsers: Chrome
<br/>Operating System: iOS, Android


#### Possible causes
Under investigation.

### If the video signal was stopped while the call is in "connecting" state, the video will not be sent after the call started 
If users decide to quickly turn video on/off while call is in `Connecting` state - this may lead to problem with stream acquired for the call. We encourage developers to build their apps in a way that doesn't require video to be turned on/off while call is in `Connecting` state. This issue may cause degraded video performance in the following scenarios:

 - If the user starts with audio and then start and stop video while the call is in `Connecting` state.
 - If the user starts with audio and then start and stop video while the call is in `Lobby` state.

#### Possible causes
Under investigation.

### "Fail to place call: No camera device found" error on Safari on desktop and on iOS 
If camera access was already granted after some time end-user will see the error:"Fail to place call: No camera device found". Safari on desktop and on iOS does not keep permissions for very long time unless there is a stream acquired. The simplest way to work around this is to call ADP before actually joining the call as well. If the permissions are there, then user will not see anything, if not, it will re-prompt.

<br/>Devices affected: iPhone
<br/>Client library: Calling (JavaScript)
<br/>Browsers: Safari
<br/>Operating System: iOS

###  Sometimes it takes a long time to render remote participant videos
During an ongoing group call, _User A_ sends video and then _User B_ joins the call. Sometimes, User B doesn't see video from User A, or User A's video begins rendering after a long delay. This issue could be caused by a network environment that requires further configuration. Refer to the [network requirements](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/network-requirements) documentation for network configuration guidance.
