# Informal Q&A on GrapheneOS Security
This guide is intended to be for users that would like to know a bit more about GrapheneOS, and the security enhancements it makes.

It's just a draft and is some pretty off the top stuff that I've just been putting together off the top of my head mostly, so bear with me for now. 

## "I've got a great idea for a security measure in GrapheneOS! Why isn't it implemented?"
At this present point in time, GrapheneOS has a number of specific goals for any security measures that it implements:
* To make full use of all hardware security features available for a device, which includes things like Android Verified Boot, hardware-bound encryption, hardware-backed keystore, In/Out Memory Management for modem and radio isolation. 
* To build improved memory safety measures and exploit mitigations into the operating system itself. 
* Any security measures beyond this that the project takes need to have a clear threat model decided. This means that they must have a practical application and there must be a clear and defined threat vector that is applicable and addressable using a technological solution, and be of a high enough priority to merit assigning limited developer time. See for example "Plausible Deniability."


## "How can I tune GrapheneOS for maximum hardening and privacy and security? Where are the enhanced security and privacy controls?"

You don't. 

Security measures that GrapheneOS adopts must not simply place the burden onto the user, nor should they be dependent on user judgement. One of the longest standing design goals for the project is that security in GrapheneOS should function transparently to the user and function in such a way that it is unobtrusive, unnoticeable, and automatic.

As a result the design choices, default connections, and many other settings on GrapheneOS have been very carefully and conservatively set, and security measures should be as transparent to the user as possible, or where user-facing settings are inevitable incorporated into existing user interfaces to make the experience as seamless and intuitive as possible. Buttons, control panels, tunables, knobs and settings are undesirable, as the settings in GrapheneOS should be enforced to safe values and not allowed to to rolled back or downgraded. 

As a result, GrapheneOS lacks a "Security Center" or "Advanced Privacy Controls" menu. This is intentional. GrapheneOS won't bug you about what security measures it's doing nor ask you for your input. Everything in GrapheneOS is already set for maximum privacy and security. Anything else would just be theatrics. 


## "What does/will GrapheneOS do about persistent hardware identifiers like your IMEI or phone Serial number?"
Starting with the Android 10 specification, apps can no longer extract the phone's IMEI or Serial Number, SIM Card Serial Number, Subscriber ID, MAC Address or other non-resettable unique device identifiers, even if granted access to `READ_PHONE_STATE`. Apps must have the `READ_PRIVILEGED_PHONE_STATE` (new to Android 10) in order to get access to any of these non-resettable, persistent device identifiers. Apps using the Android 10 API will recieve a `SecurityException` error, and any older apps simply get an empty value if the `READ_PHONE_STATE` permission has been granted to them, or a `SecurityException` error if they don't. MAC Addresses are randomized per WiFi network on GrapheneOS. Apps, even if granted full network access, cannot read nor change the MAC Address. 

GrapheneOS does not utilize Advertising IDs, even though this is resettable. 

`ANDROID_ID` is persistent between application installs but is resettable and under your control. GrapheneOS may be able to improve on this, so if you have the necessary skills to make the improvement, please send it in via pull request on our Github. 


## "Why is Vanadium based on Chromium? Isn't Firefox better for privacy?"

Vanadium builds upon and improves the security and privacy of Chromium rather than try to build on Firefox for a number of reasons:

Firefox has an "all your eggs in one basket" approach, running as a single process. Outside of its reliance on the Android Sandbox, Firefox on Android makes no attempt to isolate and contain the different websites it handles from each other and adding this after the fact would be much more difficult without completely redesigning Firefox from the ground up. Chromium instead uses a much safer approach where the different websites are kept sandboxed not only from the operating system, but also from each other. Firefox comes with its own web engine, which on GrapheneOS would be redundant and also adds a considerable amount of attack surface that isn't needed. 

Vanadium takes several of its own measures to improve the privacy of Chromium: it does not include telemetry and does not phone home, disables privacy invasive antifeatures such as media routing, site prefetching, Google's safebrowsing, disallows sensor access to prevent device sidechannel fingerprinting via sensor calibration, and even goes the extra mile to prevent websites from reading battery state and using that to fingerprint your device by always reporting battery at 100% and plugged in. All of these features have specific threat models and purposes to reduce the uniqueness of the browser, and aren't simply included to win "feature points".


## "Does GrapheneOS phone home at all? Is there any analytics or telemetry?"

No.

From the Usage guide on GrapheneOS: 
> "GrapheneOS makes connections to the outside world to test connectivity, detect captive portals and download updates. No data varying per user / installation is sent in these connections. There aren't analytics / telemetry in GrapheneOS." 

(Usage | GrapheneOS, https://grapheneos.org/usage#default-connections, retrieved 29th January 2020)

GrapheneOS does make some standard connections that regular Android does to retrieve status codes. This is done to detect things like captive web portals and connectivity. Retrieval of status codes do not send Google any identifiable information and is done intentionally for the sole purpose of camouflauging devices running GrapheneOS as regular Android handsets. This helps to combat network-level device fingerprinting. Changing the URLs or switching it off entirely will almost certainly give your device a much more unique presence on the network since it will stand out as not retrieving status codes and make your particular device easier to identify, isolate, and track your device via the network. 


## "If I deny an app the 'Sensors' permission what happens? This permission toggle isn't available on regular Android."

If you deny an application permission to read your phone's sensors, GrapheneOS will zero out sensor input to it and the app will get the impression the phone is lying perfectly flat and perfectly still. 

While your phone's sensors can be used to gather a great many things on your state or activity, some apps actually have a legitimate need to have access to them. Super Tux Kart, for example, uses your phone's tilt sensors to allow you to utilize your phone as your go-kart's steering wheel. Some navigation applications and fitness tracking applications such as pedometers which measure your physical activity use your phone's motion sensors and accelerometers to determine how fast you are going, won't be able to do their job if this permission has been switched off. 


## "Does Graphene allow me to nuke my data if say, the phone gets ten incorrect guesses at the pincode?" 

Device Wipe on authentication failure is redundant, unnecessary, and dangerous. GrapheneOS does not need to use it to keep the data safe, and has a very different way of handling brute force attempts at the password than simply deleting the keys.

Modern Pixel handsets use the Titan M security chip for pin authentication and password authentication. The Titan M contains its own internal clock that the operating system has no influence over and as a result cannot spoof. The Titan chip itself is small (almost the size of a grain of rice) and by nature of its design is extremely difficult to attack. Since the Titan M also is integrated into the disk encryption process, it also stores the tokens used to derive the keys to the files on the SSD and won't release the tokens to the operating system unless it gets the correct passphrase or pin, meaning that any attempts to access the files on the phone's drive need to be made through that specific Titan chip, on that specific phone. This is to prevent offline cracking attempts where the phone is physically cut open, the SSD is removed from the PCB, mirrored via NAND cloning, and the encrypted data on it subjected to guessing attacks run on a much more powerful system that can run more guesses than the phone would otherwise allows.

During a brute force attack when many attempts may be entered into the phone in an attempt to find the right one, the Titan M will enforce a progressively lengthening time. After the first five attempts, the Titan will shut off access to the tokens for 30 seconds and switch off the fingerprint reader. Even if the operating system is exploited and the clock is bypassed, the Titan M answers only to its own timer housed within the chip and can ignore further attempts to guess the pincode or password. Continuing to attempt to make attempts on the pincode without waiting will result in the Titan punishing the attempt by progressively increasing the timeout for longer and longer periods of time, up to a maximum of only allowing one guess every day. At this rate, searching through the available keyspace of only a relatively weak 4-digit pin code of 0000 to 9950 could take over 27 years or approximately a third of a lifetime, or for a simple two-word passphrase from a word list of 2048 words (for reference, the Pocket Edition of the Oxford English Dictionary contains over 50,000 words), almost eleven and a half thousand years before the entire key space can be exhausted. 

Because of the safety implications this has of data loss, and the strength of the Titan M being able to enforce this timeout, Data Wipe on Authentication failure is obsolete. This won't protect you if your password is "password" or "1234" or something easily guessable like the year of your high school graduation or your birthday. However, it will bolster the strength of a short, but hard-to-guess pincode or passord to keep your data safe, without needing to keep it in a perpetually precarious situation where it's liable to be accidentally destroyed.

If keeping your data safe for longer than eleven thousand years is a requirement, GrapheneOS also allows you to set passphrases of up to 64 characters. A four-word passphrase generated from entries selected at random from the Pocket Oxford Dictionary is both easily memorable, can be easily typed in, and would require 1.71e16 or approximately seventeen quadrillion years to brute force.


## "Does GrapheneOS offer protection against Stingrays?"

GrapheneOS always considers the network to be hostile.

Telephones were invented over a hundred years ago and SMS dates back to the 1980's. It's now 2020 and you should utilize an encrypted instant messaging application like Signal Private Messenger. When you use Signal, an adversary monitoring the network would only be able to see your phone connecting, via TLS to an Amazon AWS instance. Signal chose to utilize AWS because Amazon AWS already hosts more than half of all smartphone backends, making it difficult to block without breaking most of the Internet and difficult to tell apart from other traffic. Signal uses the Intel SGX for contact discovery and routing and sealed sender to mitigate metadata leakage, which helps to combat social graphing, and is relatively easy for non-technical users to install and use without changing any of their use habits. 

Signal is fully tested and fully working on GrapheneOS and a signed apk installer package is available from their website directly, or you can clone the repository and build it yourself. 

If you were to use an encrypted SMS application like Silence, the network operator (or the Stingray operator impersonating them to you) would not only be able to see your number and the phone number of who you are messaging and what time you contacted them, but also see you're using encryption with that other number. Since adoption of encrypted SMS tools is very low and SMS is a security-agnostic protocol that trusts the service provider and all the network inbetween with everything in plaintext, this would make not only you, but also your contact stand out from the crowd.


## "Does GrapheneOS offer protection against evil backdoor basebands?" 

GrapheneOS only elects to support devices that offer the best modem and radio isolation via IOMMU mitigations and policies. IOMMU only allows a device attached to the host memory by direct memory access to read or write to memory values that the driver in the kernel specifically allows it to, and only elects to support devices that have open source drivers which are readily supported by their vendors and updated frequently. 

The firmware for the radios and modems is not state-carrying. GrapheneOS will load the firmware binaries when the operating system boots. Prior to this, the system image (which contains the firmware binaries) will be verified via Android Verified Boot which is keyed to Daniel Micay's signing keys. Should any of the firmware be exploited, it is already limited from the host memory via IOMMU, and rebooting it will restore it to a known good state. 


## "Does GrapheneOS offer Disk Encryption? With what cipher?"

Yes, of course! All modern phones **should** offer strong file based, hardware-bound encryption, rather than full disk software-only encryption. The reason for this is simple: phones aren't laptops. They're left on around the clock more often than not, during which, the hard drive will need to be unlocked in order for the phone to function. Park this in your mind, since there's more on that in the question immediately after the one following this one. 

GrapheneOS makes full use of the hardware-bound encryption in modern Android devices. The Titan M handles the user's pincode or password. Upon getting the correct password, the Titan M will calculate, then release an access token to the phone that the phone in turn can use to derive the keys via a key derivation function which also uses a mathematical proof of uniqueness called a "cryptographic hash" of the first 2,000 bytes of a special partition set aside on the phone's SSD as a safeguard to mitigate data remnance issues. Each file is encrypted with AES-256-XTS, and the filesystem metadata is encrypted with AES-256-HEH on modern handsets (older handsets use AES-256-CTS for the filenames, but these are on the way out). 


## "I'm giving my phone to someone else, selling it to a stranger secondhand, or giving it to a recycler for disposal, and I want all the data on it gone. What guarantees do I have of this?"

When you hit factory reset on your cellphone, the Titan will destroy its access tokens and the phone will change the header partition. Without either the access tokens or if even one bit is changed of the header partition, the key derivation function will produce an incorrect result, and the data on the drive will be lost forever. The key derivation function by virtue of its design cannot be reversed. The data on it will be gone. Forever. No ifs, ands, or buts. 

There is no recovery from this state, so if there's anything on your phone that you really don't want to lose, make sure you have it backed up separately beforehand. This includes changing your 2FA tokens, password manager, exported any bookmarks you don't want to lose, saved any pictures, videos, or movies, anything stored on your phone. 


## "Why doesn't Graphene offer Full Disk Encryption? Why File Based Encryption? Why not Both?"

File Based Encryption has several advantages over Full Disk Encryption:
* It allows work profiles to be made more secure, because when one user is logged out, the keys can be ejected from memory. This isn't doable with full disk encryption because full disk encryption is all-or-nothing. 
* When files are not in use, such as if the screen is locked, their keys can be ejected from memory. 
* With the use of the Strongbox Key Master on modern Android Handsets (Pixel 3 and later), pictures can be taken and written to the drive encrypted while the screen is still locked, without having to unlock the drive so that files can be written to it. 
* The phone can handle updates even while locked and more importantly, the data is at rest. 
* It allows some features most people have come to expect out of cellphones, such as direct boot. 

The filesystem metadata on modern GrapheneOS handsets is itself padded to disguise filename lengths, then is encrypted with AES-256-HEH, so adding another layer of encryption on top would be redundant, and unnecessary. It would also break other security features such as direct boot and background updates, making updating and patching more difficult. Since phones spend a majority of their time powered on, the gains of layering one within the other would likely never be realized. It would add more complexity to the entire stack and increase the amount of attack surface and amount of code that would need to be maintained and the amount of places that bugs could hide for very little gain. In short, the tradeoffs of simply "add both" would not be worth the benefits, would break certain features that are more important than file metadata which itself is already encrypted, and would represent a number of steps backward.


## "Can I have a different passphrase to boot the phone and a different passphrase to unlock the disk?"

Encryption in GrapheneOS uses per-profile keys, where each profile gets a different key to the files on the disk. Some applications additionally support the use of the Strongbox Keymaster API on Android which allows their keys to be ejected from memory whenever the phone's screen is locked, which makes this redundant. 

In the eras past the disk encryption password could be made different from the lock screen password, but this was because back in those eras, Android used a very different way of handling filesystem encryption, and used full disk encryption. Full disk encryption has numerous weaknesses, which include not being able to granularize access and get data at rest (where the encryption could actually provide a useful security measure) when it isn't being in use.

You can still choose to use a strong passphrase to lock your phone, and then use a fingerprint to unlock it. 


## "I don't feel comfortable adding just a fingerprint unlock to my phone, but entering my EFF Diceware Passphrase every time is too cumbersome. Could I have a way to have a pincode with my fingerprint?"

This feature has been requested for years. Pull Requests are welcome. If you can't code, pay someone else who can; an open source project requires collaboration and contributors.


## "I want to root my GrapheneOS!"

Don't. 

GrapheneOS is designed to minimize privilege, minimize attack surface, and minimize access. App-accessible root destroys the security model of Android, increases the attack surface, and is mutually exclusive with important security measures such as verified boot. If you have app accessible root on, by design Android Verified Boot would detect any persistent changes you make to the system partition, and simply undo them to revert your system partition to a state where it will pass signature validation. So either this would mean you would need to go without verified boot, or any changes you made as root would end up being reverted the moment you rebooted. 

GrapheneOS is not only utilizes reproducible builds but each GrapheneOS installation on any given device is also bitwise identical, is cryptographically validated by Android Verified Boot, and its authenticity can be proven remotely via Android Remote Attestation to show that the operating system installed is the same operating system that the developers wrote, and Daniel Micay personally inspected, compiled and digitally signed on a system he physically possesses, bought and paid for.

You're welcome to fork, build and compile and self-sign builds of your own that allow for app-accessible root, but at this point, you're not really running GrapheneOS, and are recieving far fewer of the benefits you would by doing so normally.


## "i2p Bote, Plumble, and some old F-Droid apps that haven't been updated in years suddenly crash as soon as they connect to the network. This behaviour is not observed on the Google Android. Why?"

These crashes are likely caused by the app being unsafe. A number of the security improvements in GrapheneOS are designed to catch unsafe memory corruption bugs. Although GrapheneOS is designed to try to handle security exceptions gracefully, apps that misbehave and could produce vulnerabilities or exploits are forcefully terminated by the operating system before they could present attack surface or attempt to exploit the operating system.

More information on how to deal with this is here: https://grapheneos.org/usage#bugs-uncovered-by-security-features


## "I want to trace GrapheneOS source code to the actual builds. How can I do this?"

GrapheneOS is reproducible. More information on reproducing builds by setting the time and build versions to be the same as release versions is contained here: https://grapheneos.org/build#reproducible-builds 

As you would require the private keys to reproduce the digital signatures on all signed parts of the operating system, you would need to inspect the compiled images yourself prior to putting together the factory image. Android Open Source Project contains tooling to inspect these builds.

More information on manual inspection will follow at a later date.


## "I'd like GrapheneOS to have a plausible deniability feature, so when I travel across the border I can give away a decoy password!"

The threat model of GrapheneOS is to protect you from memory corruption vulnerabilities and software exploits, and contain apps you've installed on your device and prevent them from exploiting your operating system. An adversary that has full legal authority to put you into a jail cell filled with rats and not let you out until starve to death or give them the information they want is outside of the threat model of the operating system.

It isn't practical for GrapheneOS to attempt to do plausible deniability or hidden operating systems for cross-border searchers, as with some rudimentary data forensics it is almost trivially easy to determine if the ciphertext contains a hidden volume encrypted to a different key (see the Veracrypt documentation on hidden operating systems), and maintaining a convincing hidden partition or hidden operating system requires that a user be able to maintain and use it convinicingly enough to stand up to scrutiny. On top of that, if the border services agency is really out to get you, do you actually want to lie to them, and then have them catch you red-handed if they decide to take a closer look at any of your devices and see a discrepancy because the usage patterns in the decoy don't match up with their expectations? If this is truly in your threat model, what are you looking to fool? Only a bored border officer who's going to spend five seconds to go "looks legit" and what happens if they turn over the siezed phone to someone who's going to probe it more thoroughly while you're detained? What happens when this kind of security measure becomes known (and it will become known, since GrapheneOS is extensively documented and sources are publicly available), and it becomes expected that users of these devices will be using this feature? 

A far easier way that requries no long term commitment to lay a convincing decoy trail to protect against this vector, and then hoping your adversary falls for it is to create and store an encrypted backup somewhere accessible to you at a later date, factory reset your device before taking it through customs, and then download and restore your backup once you're through.


## "What security measures does GrapheneOS have against those cell phone unlockers used by the military like Cellebrite, Graykey, etc? What about nation states with unlimited resources?"

Before we get into this subject, understand that if an adversary has physical control of the hardware and unlimited resources, it's not a matter of whether it will or won't be possible for them to decrypt the cellphone, just a matter of how much time, effort, and money they are able to spend and how many people they are willing to intimidate, kidnap, torture, or murder in order to get what they want. 

Many cellphone extraction systems such as GrayKey and Cellebrite mostly involve extracting the contents of the SSD of an unlocked phone. This alone isn't very interesting and it doesn't mean much if a device is listed as being supported. Cellphone unlocking systems that offer lock bypasses usually involve an exploit on the enclave itself to bypass rate limiting to allow more guesses at the password than the phones are otherwise supposed to allow in that span of time enough to make a brute-force attack on the six-digit pincode feasible, since many users only use a four or a six-digit pincode. 

GrapheneOS allows for passwords of 64 characters, and prefers handsets such as the Pixel 3, which use a separate coprocessor to calculate and release the access tokens to the phone for key derivation. Such implementations offer substantially reduced attack surface reduction and at the present date, no exploits have been publicly documented for them. However, if you consider these exploits to be in your threat model and assume that at a future date one could be discovered, or the HSM in your phone could be delidded and disassembled via the use of lasers and precision robotics, cloned, and then brute force attacks run in parallel (All in all, an extremely targeted attack that could easily end up costing tens of millions of dollars and only work for that particular phone at this point in time), there is an easy defence: utilize the extra security of a long and cryptographically strong passphrase to thwart brute force guessing.


