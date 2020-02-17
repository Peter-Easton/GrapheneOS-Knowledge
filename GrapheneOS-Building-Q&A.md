# GrapheneOS Building Q&A
This is a little bit about building GrapheneOS.

## How long does it take to build GrapheneOS from source? 
On a Ryzen 2600 with 32 GB of RAM, it takes roughly 3 hours to build Vanadium, and 2-1/2 hours to build the operating system for the Google Pixel, including the time it takes to build the kernel. 

This does not take into account the time it takes to download the repositories, sync, and download the source code. A good internet connection is recommended. The first time the source trees are downloaded may take close to 100GB, so don't forget to take this into account on your connection.

## I want to become a GrapheneOS developer. What should be on my shopping list of things to buy? How much money will I need to spend? 
See "Your GrapheneOS Shopping List" in this directory for more details on what you will need or things that may come in handy. Requirements for building GrapheneOS are fairly modest and an acceptable building workstation can be put together for less than the cost of the phones it will be building for. Of course, more powerful computers will result in faster build times and less having to wait and watch stdout scroll past. 

## Are GrapheneOS builds reproducible?
GrapheneOS is reproducible, and uses multiple levels of digital signing and validation to assure authenticity and integrity. By design, digital signatures cannot be reproduced without the same private keys used to produce them, which necessitates manual comparison of everything except for the signatures. See https://grapheneos.org/build#reproducible-builds for more details. 

## Are there any recommendations for incorporating Wireguard into the kernel?
Yes. The recommendation is: **don't**. At the present time there's no userspace support for the kernel module to integrate. Apps cannot use the kernel module, since using it requires privileges that only netd has as a part of the AOSP security model.
