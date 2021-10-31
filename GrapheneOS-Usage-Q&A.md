# GrapheneOS Usage Q&A
This has some general questions and answers to GrapheneOS.

## What app store do you recommend?
A first-party app store may be in the works in the future, but for now, you will need to find your own. Some users have found luck with Aurora and Yalp. 

At this moment, F-Droid is not recommended. This is due to F-Droid having accumulated large numbers of apps that are now unmaintained and target much older versions of the Android API all the way back to before Android 6, and not warning the user that the apps are written and compiled for older versions of Android. Obsolete apps that target older versions of the API will almost certainly not run correctly on modern versions of the factory versions of Android or on GrapheneOS. Since developers do not release directly via F-Droid, but rather, F-Droid functions as a trusted third party to deliver the updates and signed builds rather than updates being published directly by the developers, F-Droid has a much slower update cycle, often taking years to update their apps or simply not updating them at all and leaving a large collection of obsolete, old, or abandoned apps in the F-Droid repositories.

## What is the best and most secure instant messaging application you can get on GrapheneOS?
Signal Private Messenger. Please either compile the app from source and self-sign your build, or obtain the signed apk available on Signal.org. Signal is working fine and is extensively tested on GrapheneOS.

## What's the best for taking pictures with GrapheneOS?
Google Camera, but it requires sandboxed Play Services. Additionally, a new camera app is being developed by GrapheneOS as a replacement of AOSP Camera. OpenCamera can be used if using Play services is not desired.

## What prevents me from loading a counterfeit application?
GrapheneOS observes the Android security model of application installation. When an app is first installed, its developer's certificate included in the apk installation bundle is pinned via a trust-on-first-use mechanism, and the app is validated to that particular certificate. Future updates installed to the phone must be signed with the same certificate for the rest of the app's life, and Android will not permit the certificates to be changed as long as the app is installed. Other developers cannot present updates for that given app without them being signed by the key to the certificate that was present at the time of installation. This decentralized signing model is an integral part of the overall Android security model by allowing the infrastructure to be distrusted and not tying the means to authenticate updates to a single set of keys given to a trusted third party, the way Debian's apt system functions.

The question then becomes finding a genuine copy of the first app to install without a preauthenticated back-channel to the developers, which is why at this moment GrapheneOS is considered not production ready, as not everyone will have a preauthenticated back-channel to their app developers, and not everyone will know how to use it.

## Can I have MicroG with signature spoofing?
No, MicroG is not supported. Signature spoofing is not supported either, due to the pandora's box of attack surface that it presents. You would have dramatically better privacy simply using factory Android with Google in it rather than employ signature spoofing.

## I'm getting some issues with alarms functioning. Alarms for reminders aren't going off, or apps aren't displaying notifications properly.
This is generally encountered on apps from F-Droid and is a problem with the application itself and not GrapheneOS. This behaviour is symptomatic of the app itself been written and compiled for a much older version of the Android API and the functionality it depended on has been depreciated by upstream. This also happens on the factory versions of Android. 

Please contact the app developer and ask them to update the API, and if you obtained it from F-Droid, contact the F-Droid developers to update the software. Apps should be coded to use the `setExactAndAllowWhileIdle` functionality in the alarm documentation, at https: https://developer.android.com/reference/android/app/AlarmManager.html#set(int,%20long,%20android.app.PendingIntent)
