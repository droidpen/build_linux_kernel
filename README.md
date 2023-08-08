# build_linux_kernel
Instructions/scripts on how to make linux kernel from scratch

## Step 1: Download the Source Code
1. Visit the official kernel website and download the latest kernel version. The downloaded file contains a compressed source code.
https://www.kernel.org/
https://phoenixnap.com/kb/check-linux-kernel-version
https://phoenixnap.com/glossary/what-is-source-code

Downloading the latest Linux kernel.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/017c26b9-769b-4b8e-a7f2-3a4cf2e8a0cd)

2. Open the terminal and use the wget command to download the Linux kernel source code:

```
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.7.tar.xz
```

The output shows the “saved” message when the download completes.

Using the wget command to download the source code.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/2e87ab75-72fb-4fde-ba7b-39c5ac5e3f46)

## Step 2: Extract the Source Code
When the file is ready, run the tar command to extract the source code:

```
tar xvf linux-6.0.7.tar.xz
```

The output displays the extracted kernel source code:
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/6eeaad4b-a963-4dbc-9b9a-1bb2cf78511f)

## Step 3: Install Required Packages
Install additional packages before building a kernel. To do so, run this command:

```
sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison
```

The command we used above installs the following packages:

| Package |	Package description |
| --- | --- |
| git | Tracks and makes a record of all changes during development in the source code. It also allows reverting the changes. |
| fakeroot | Creates the fake root environment.
| build-essential | Installs development tools such as C, C++, gcc, and g++.
| ncurses-dev | Provides API for the text-based terminals.
| xz-utils | Provides fast file compression and decompression.
| libssl-dev | Supports SSL and TSL that encrypt data and make the internet connection secure.
| bc (Basic Calculator) | Supports the interactive execution of statements.
| flex (Fast Lexical Analyzer Generator) | Generates lexical analyzers that convert characters into tokens.
| libelf-dev | Issues a shared library for managing ELF files (executable files, core dumps and object code)
| bison | Converts grammar description to a C program.

![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/73528796-b304-49dd-9c8d-877031747578)


##Step 4: Configure Kernel
The Linux kernel source code comes with the default configuration. However, you can adjust it to your needs. To do so, follow the steps below:

1. Navigate to the linux-6.0.7 directory using the cd command:
```
cd linux-6.0.7
```
2. Copy the existing configuration file using the cp command:
```
cp -v /boot/config-$(uname -r) .config
```
Navigating to the kernel folder and copying configuration file.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/8c2d29e7-5eb4-4374-b450-e7d3180dde14)

3. To make changes to the configuration file, run the make command:
```
make menuconfig
```
The command launches several scripts that open the configuration menu:
Loading the configuration menu.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/460ff163-66f3-4f21-a6f6-c1b98cd8aa27)

4. The configuration menu includes options such as [firmware](https://phoenixnap.com/glossary/firmware), file system, network, and memory settings. Use the arrows to make a selection or choose **Help** to learn more about the options. When you finish making the changes, select **Save**, and then exit the menu.

Changing the settings in the navigation menu.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/e51d5232-50e4-4838-8395-630bc9d9efbe)

Note: Changing settings for some options can lead to a non-functional kernel. If you are unsure what to change, leave the default settings.


##Step 5: Build the Kernel
1. Start building the kernel by running the following command:
```
make
```
The process of building and compiling the Linux kernel takes some time to complete.

The terminal lists all Linux kernel components: [memory management](https://phoenixnap.com/glossary/memory-management), hardware device drivers, filesystem drivers, network drivers, and process management.

Building a Linux kernel with the make command.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/68b96a71-2e8d-419f-add1-659866d9a017)

If you are compiling the kernel on Ubuntu, you may receive the following error that interrupts the building process:
```
No rule to make target 'debian/canonical-certs.pem
```
Disable the conflicting security certificates by executing the two commands below:
```
scripts/config --disable SYSTEM_TRUSTED_KEYS
```
```
scripts/config --disable SYSTEM_REVOCATION_KEYS
```
The commands return no output. Start the building process again with make, and press Enter repeatedly to confirm the default options for the generation of new certificates.

2. Install the required modules with this command:
```
sudo make modules_install
```
Installing kernel modules.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/1a71560d-5c35-4745-b84b-b2d044df591e)

3. Finally, install the kernel by typing:
```
sudo make install 
```
The output shows **done** when finished:

Installing the kernel.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/7fc200e4-b12a-4127-ac78-dbd7b3ea9ba8)


##Step 6: Update the Bootloader (Optional)
The GRUB bootloader is the first program that runs when the system powers on.

The **make install** command performs this process automatically, but you can also do it manually.

1. Update the initramfs to the installed kernel version:
```
sudo update-initramfs -c -k 6.0.7
```
2. Update the GRUB bootloader with this command:
```
sudo update-grub
```
The terminal prints out the process and confirmation message:

Updating the grub.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/9fc0743c-ee83-4725-9529-0f31c29bf101)


**Step 7: Reboot and Verify Kernel Version**
When you complete the steps above, reboot the machine.

When the system boots up, verify the kernel version using the [uname](https://phoenixnap.com/kb/uname-linux) command:
```
uname -mrs
```
The terminal prints out the current Linux kernel version.

Verify the current Linux Kernel version.
![image](https://github.com/droidpen/build_linux_kernel/assets/54828368/e6011fc3-0dd7-422d-a9a2-9ee40381687e)

##Conclusion

In this step-by-step guide, you learned how to build a Linux kernel from scratch and install the required packages.

If you follow the instructions carefully, the process will complete successfully on your Linux machine.

The Linux kernel has a modular design. Functionality is extendible with modules or drivers. Learn how to use the [modprobe command](https://phoenixnap.com/kb/modprobe-command) to add or remove modules on Linux.
