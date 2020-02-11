# GrapheneOS compared to other mobile operating systems
## GrapheneOS
### Project Goals
* Improve the low-level security characteristics and code quality of the Android Open Source Project including the Kernel, Bionic C Library, and Memory Allocator to reduce attack surface and mitigate potential exploits before they become vulnerabilities. 
* Implement and continuously improve remote intrusion detection systems via Android Remote Attestation capabilities included in handsets that shipped with Android 8.0 or later (See the Auditor Project) using hardware-backed keystore and Strongbox Keymaster.
* Perform firmware and hardware security research and analysis on the devices with the best security.
* Improve the privacy and security characteristics of Chromium by removing the telemetry and improving upon the sandboxing and isolation features of Chromium.
* Minimize privilege, minimize access, and in doing so minimize attack surface by implementing more restrictive security policies beyond what Android normally enforces.

### Upstream Providers
* Android Open Source Project
* Chromium 

### Currently Supported Devices
* Google Pixel 2 and 2XL
* Google Pixel 3 and 3XL
* Google Pixel 3a and 3aXL


## CalyxOS
### Project Goals
* Build on the Android Open Source Project to provide a privacy-centric, but out-of-the-box entirely usabile experience using open-source alternatives to popular proprietary applications and services. 
* Incorporate opinionated operating system compile-time options, software settings, included applications, and included networks such as Tor and the CalyxOS Institute's VPN service to provide readily available privacy-enhancing tools without requiring users  to have to set them up for themselves.
* Utilize the hardware security features such as Verified Boot. Co-operation between CalyxOS and GrapheneOS has allowed CalyxOS to be used with the GrapheneOS device integrity monitoring. 
* Remain as compatible as possible with existing Android applications by including an open-source handler to some of Google's Network Services. 

### Upstream Providers
* Android Open Source Project
* F-Droid
* MicroG
* Various software application providers, including K-9 Mail, OpenKeyChain, Signal Private Messenger, DuckDuckGo Privacy Browser, Tor Browser.
* GrapheneOS for Auditor

### Currently Supported Devices
* Google Pixel 2 and 2XL
* Google Pixel 3 and 3XL
* Google Pixel 3a and 3aXL


## Rattlesnake OS
### Project Goals
* Provide the user with the means to quickly and readily provision an Amazon Web Services S3 Bucket to self-host their own automatic building, signing, and Over-The-Air update server to build the Android Open Source Project and self-sign their own builds for Pixel Phones. 
* Give the user agency over their own update infrastructure via Amazon Web Services if they cannot afford to buy and run their own build servers, or lack the expertise to be able to do so independently. 

### Upstream Providers
* Android Open Source Project
* F-Droid

### Currently Supported Devices
* Google Pixel 2 and 2XL
* Google Pixel 3 and 3XL
* Google Pixel 3a and 3aXL


## HashBang OS 
### Project Goals
* Improve upon the Android Open Source Project build stack to more easily allow independent multiparty verification of the builds and increased transparency and auditability in the build process. 
* Far future goals for the project include using some of the security and privacy improvements from GrapheneOS as an upstream provider. 

### Upstream Providers
* Android Open Source Project
* GrapheneOS (GrapheneOS Seamless Update Client, Vanadium) 
* F-Droid 

### Currently Supported Devices
Not Yet Released. Future releases have been stated to be target Google Pixel 2s and later. 


## LineageOS
### Project Goals
* Allow the user permissiveness to tinker with and customize the operating system as desired, including the ability to remove unwanted applications that are normally not removable, as well as the ability to install applications with functionality that would normally violate Android Security Policies or the Android security model. 
* Extend the usable lifetime of devices that are no longer actively supported by their vendors.
* Achieve broad device support.

### Upstream Providers
* Android Open Source Project
* Cyanogenmod (Formerly, now discontinued)
* F-Droid (Optional, at user's discretion)
* MicroG (Optional, at user's discretion)

### Currently Supported Devices
Over 100 as of last year. 

### Advisories
* LineageOS does not allow for Verified Boot, and hardware security features dependent on it also will fail to work.
* Quality of support varies between devices, depending on vendor support, availability of firmware updates, and maintainer attentiveness and willingness to ship available firmware updates. Not all devices are created equal.


## /e/
### Project Goals
* Lower the barriers to adoption of an operating system similar to LineageOS by providing preconfigured, precompiled downloads readily available and precompiled for various handsets. 
* Provide an open-source handler to Google's network services already built into the operating system. 

### Upstream Providers
* LineageOS
* MicroG

### Currently Supported Devices
Claimed 91 as of this year. 

### Advisories
* As with LineageOS, /e/ does not allow for Verified Boot, and hardware security features dependent on it will fail to work.
* As with LineageOS, quality of support varies between devices, depending on vendor support, availability of firmware updates, and maintainer attentiveness and willingness to ship available firmware updates. Not all devices are created equal.


## Replicant
### Project Goals
* Achieve a fully or near-fully libre software and firmware stack on mobile devices. 
* Allow the user permissiveness to tinker with and customize the operating system as desired, as LineageOS does. 
* Extend the usable lifetime of devices that are out of vendor support, as LineageOS does. 
* Broad device support is not a goal of Replicant. Instead, Replicant supports devices that do not enforce signature validation on their firmware and can be made to run open-source firmware developed by reverse-engineering. As some parts such as the modems and radios by law must run government-approved software, devices running Replicant must have their radios and modems external to the SOC. 

### Upstream Providers
* LineageOS
* F-Droid (Optional, at user's discretion)

### Currently Supported Devices 
8 in total, all from approximately 2012 or earlier. 

### Advisories
* Devices that support Replicant predate secure elements, verified boot and the associated hardware security features entirely. 
* WiFi, Camera and Cellular modem support varies by device. Some Replicant devices lack support for them; this is intentional due to the radios and modems requiring non-libre firmware which is rejected from Replicant on ideological grounds. Some devices require external modems to be plugged in via the Micro USB port to provide network access.


## PureOS Mobile
### Project Goals
* Port the desktop stack (systemd, GNOME, Wayland) to a mobile device. 

### Upstream Providers
* Debian

### Currently Supported Devices
* Librem 5

### Advisories
* The Librem 5 is not as open or libre as its marketing has tried to insinuate, simply having its binary blob signed and validated firmware saved in write-protected read-only memory and loaded by a secondary coprocessor to exploit a loophole in the definiton of "libre" hardware to allow it to qualify for the FSF's definiton of "Free" hardware. This renders the firmware unupdateable without shorting a connection. In the event a vulnerability is discovered in the modems or radios, the firmware cannot be updated without physically dismantling the phone. Firmware initialization is also no longer under the control of the host operating system because the initialization is carried out from outside the OS: changing or updating software on the host will not address these design defects. Although the modems and radios are not attached to the host via DMA, they rely on USB for isolation, which simply shifts the trust from the kernel driver to the kernel USB stack, and USB was never designed with distrusting the device plugged into it in mind unlike SMMU/IOMMU, which is specifically designed to mitigate unconstrained DMA.
* Current releases of the Librem 5 have been plagued by thermal throttling issues and poor battery life which in some cases has clocked in at less than 1 hour at idle.
* The Librem 5 does not even support software encryption and no progress has been made toward adding even LUKS encryption. The Librem 5 lacks a secure element for any hardware binding on the encryption and so would be entirely dependent on software-only encryption. 
* The rebranded version of Debian that the Librem 5 uses as an operating system uses the same security model as the desktop stack, which is a perimeter or "all or nothing" security model. In the future, applications may be installed utilizing FlatPak. The threat model and measures FlatPak takes to meet it are as of yet unclear and uncertain.


