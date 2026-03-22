# ATOTO S8 Pro Custom ROM Install



Welcome!

Thanks to the long hours spent by [Shunhax](https://xdaforums.com/t/rooting-atoto-s8-premium-gen2-head-units.4737904/) to find out how to gain access to root!

> DISCLAIMER!!! This is NOT an official update, nor is it endorsed or anyhow affiliated with ATOTO. While flashing this update is considered stable from my tests, it is not guaranteed to work for everyone. These risks must be considered! I am not liable for any damage done to your device!

> 

> If you agree to the above statements, proceed.



## INSTALLATION



**Step 1:** Download the required files found in the [Releases](../../releases) page: `6315_1.zip`, `lsec6315update`, and `AllAppUpdate.bin`.



**Step 2:** Grab at most a 32GB microSD card formatted as FAT32, and copy those three files to the root of the card.



**Step 3:** On the ATOTO, head to settings and factory data reset the device.



**Step 4:** After the ATOTO resets, insert your microSD card and wait for a few seconds. A dialogue asking for a firmware update will appear. Click **Start**.



**Step 5:** Enjoy! :)



## FEATURES



* **Adds Root access through [Magisk](https://github.com/topjohnwu/Magisk) out of the box.**

* **Fully De-Bloated, with all potential spyware removed.**

* **Default Android Launcher and features restored.**

* **Custom Boot Animation added for additional Flair.**

* **Fully Decompressed `.img` files for easy customization.**



## COMPATIBILITY



This update is intended for the ATOTO S8 Pro, but other devices may be compatible. **TEST AT YOUR OWN RISK!!**



---



## DEVELOPMENT



If you want to dive into customizing this update yourself, I've tried to take a lot of the hoops out of the process, with the intention to hopefully make it easier for you. It is recommended to use Debian-based Linux or WSL for best results.



### Prerequisites



```bash

# Install the latest version of Java 

sudo apt install default-jre

```

Download `SignAPK.jar` from [Techexpertize](https://github.com/techexpertize/SignApk).

After that, you should be ready to start!



---



### Editing `AllAppUpdate.bin`



To add or remove apps to the preinstall list, you can do so easily in the `AllAppUpdate.bin` file. This file is essentially a zero-compression zip file with a password-protected lock preventing unauthorized unzipping. Fortunately, due to the incredible work of [Eliminater74](https://github.com/Eliminater74/atoto_firmware_downloader), I was able to extract the password to the file.



Open the terminal in the directory that contains the `AllAppUpdate.bin` to unzip it with the password.



```bash

# Unzip the AllAppUpdate.bin with the password.

unzip -P 048a02243bb74474b25233bda3cd02f8 AllAppUpdate.bin

```



This will drop the folder with all of the APKs of the apps injected into the system. Add or remove as desired.



To re-zip the files into the `.bin` file, open the extracted `AllAppUpdate` folder and run this in the terminal to re-zip it.



```bash

# Remove the old AllAppUpdate.bin.

rm ../AllAppUpdate.bin



# Re-zip the injected APKs into AllAppUpdate.bin.

zip -0 -r -P 048a02243bb74474b25233bda3cd02f8 ../AllAppUpdate.bin *

```



---



### Editing System Images



To edit the actual `system.img` or similar, follow these instructions to mount and edit contents of the image files.



Navigate to the directory where you downloaded `6315_1.zip` and open the terminal window there.



```bash

# Unzip the file.

unzip 6315_1.zip

```

This will extract everything to a folder in the same directory as the zip. 



To make changes, mount any one of the `.img` files using these commands.



```bash

# Make a directory to mount the image in.

mkdir img_mount_dir



# Mount an .img there.

sudo mount system.img img_mount_dir



# Change directory to the image

cd img_mount_dir



# NOTE: For some files, you may need to change permissions to the root:root user so the OS reads it.

```



> **NOTE:** When copying large amounts of files to the image, you might need to expand the image size. This is beyond the scope of this guide, so if you need to do that, you will need to find another source. If you do make any changes to the sizes of any of the images, make sure to update the byte sizes in `dynamic_partitions_op_list` found in the `6315_1` directory. Also note that the maximum sum for the dynamic partitions size is 4.0GB.



After you are done editing, change directories back to the main `6315_1` directory and enter these commands to unmount the image.



```bash

# Unmount the .img and remove the img_mount_dir directory

sudo umount img_mount_dir

rm -rf img_mount_dir

```



### Recompressing and Signing



After this has been done, you will need to recompress the directory and sign it using the `signapk.jar` from Techexpertize. As far as I'm aware, the ATOTO is only looking for *something* to be there as some sort of a signature, so it shouldn't need to be anything crazy. 



First, you will need to re-zip the `6315_1` directory.



```bash

# Re-zip the 6315_1 directory. Make sure you are running this inside of the 6315_1 directory.

sudo zip -r ../6315_12.zip *



# The reason we named it 6315_12.zip is so that we know which one is signed and which one is not signed in the future.



# Remove the old directory.

sudo rm -rf 6315_1

```



Now that we have the unsigned zip, move it to the same directory as the `signapk.jar`.



```bash

# Sign the zip.

java -jar signapk.jar 6315_12.zip 6315_1.zip



# Remove the old unsigned zip.

rm 6315_12.zip

```



Done! You can then move this new zip to your SD card and update it like before.

