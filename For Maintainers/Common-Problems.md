# Troubleshooting Common Problems

## android-prepare-vendor

### curl fails with "HTTP/2 stream 0 not closed cleanly"
If you get the following error code: `curl: (92) HTTP/2 stream 0 was not closed cleanly: Unknown error code (err 1)"` and cannot resolve it, there may be a conflicting file in your device directory. If this is the case, simply delete the directory for the device's tag to force android-prepare-vendor to start over. For instance, if your device is `blueline` and you were trying to download update `qq2a.200501.001.b2` you can try deleting `vendor/android-prepare-vendor/vendor/blueline/qq2a.200501.001.b2` to make it try again.

If all else fails, see offline extraction below:

#### Offline extraction
Download both the factory-image zip file, and the over-the-air update zip file, and place them into a location you can easily memorize, then run `android-prepare-vendor` pointing the script to the files you've just downloaded and pass the `--img` and `--ota` flags to the script to indicate it to use files you've already downloaded, rather than automatically search for and download the files itself.

For example: 
```
vendor/android-prepare-vendor/execute-all.sh -d blueline -b QQ2A.200501.001.B2 -o vendor/android-prepare-vendor --img $HOME/Downloads/blueline-factory-qq2a.200501.001.b2.zip --ota $HOME/Downloads/blueline-ota-qq2a.200501.001.b2.zip
```
