---
title: "Installing MacOS in VMware"
datePublished: Wed Jan 31 2024 11:30:03 GMT+0000 (Coordinated Universal Time)
cuid: cls1ph1fl00040al4hbw5epop
slug: macos-vm
tags: mac, virtual-machine, ios, macos, vmware

---

This guide will teach you on how you can set up a MacOS VM using VMWare in Windows. I'll try to make this as simple as possible. I'm currently using VMware Workstation Pro (paid), but you can also use VMWare Workstation Player (free).

Before you start, make sure your PC has virtualization enabled by opening Task Manager, tapping `Performance`, `CPU` and looking at `Virtualization`. If you don't have it enabled, just enable it in BIOS. There are plenty of tutorials on Youtube on how to do that.

## Step 1: Downloading a copy of MacOS

This step is pretty simple. First, make sure you have the latest version of Python installed. If you don't, install in [the python.org website](https://www.python.org/downloads/).

You now need to download OpenCore. Go to [https://github.com/acidanthera/OpenCorePkg/releases](https://github.com/acidanthera/OpenCorePkg/releases) and download the latest releases ZIP file. It should be something like `OpenCore-XXXXX-RELEASE.zip` .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706696610597/4ae2d9c9-cf56-4a2e-9b4f-0ec388906cc0.png align="center")

Then, simply extract it. On the extracted folder, go to `Utilities` &gt; `macrecovery` . Then, right-click any empty space, tap "More options" and "Open in terminal" or "Open PowerShell window here".

Next, you want to go [here](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html#downloading-macos), scroll a bit down, and run the command for your version. It might take a while. Close the window and go back to the `macrecovery` folder. Look for the newly generated `BaseSystem.dmg` and copy it to your Desktop or Documents folder.

## Step 2: Create a VMware disk

### Installing QEMU

Now that we have the BaseSystem.dmg file, we want to make it so VMware can read it. For this, we're using an open-source software called `QEMU` .

Go to [https://qemu.weilnetz.de/w64/](https://qemu.weilnetz.de/w64/) and select the `qemu-w64-setup-XXXXXXXXX.exe` file. Run the installer. If you get a Windows Smartscreen warning, simply tap "Run Anyway".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706697144362/a49ca29f-c65e-447a-8e71-04db8918f542.png align="center")

Wait for QEMU to install. If you get an error during installation saying a file cannot be written, just tap `Retry` button. Then, close the installation window.

### Creating the VMDK

After installing QEMU, we need to use it to convert the `BaseSystem.dmg` to a VMWare `.vmdk` disk image. Open the folder where you put the `BaseSystem.dmg` file, open powershell again and run the following command:

```powershell
& "C:\Program Files\qemu\qemu-img.exe" convert -O vmdk -o compat6 BaseSystem.dmg recovery.vmdk
```

As soon as the script ends, you should see a new `recovery.vmdk` file in your computer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706697497690/b46e2cbe-7a51-4977-9808-23ae2211fe5b.png align="center")

## Step 3: Preparing & Unlocking VMWare

For you to even be able to load a macOS system into VMWare, you will need to use a program called `Auto-Unlocker` to patch your VMWare installation.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">If you have VMWare version 15 or older, use the following URL: <a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/paolo-projects/unlocker/releases/tag/3.0.4" style="pointer-events: none">https://github.com/paolo-projects/unlocker/releases/tag/3.0.4</a>, extract the zip, open <code>win-install.cmd</code> <em>as Administrator</em></div>
</div>

Download the file [here](https://github.com/paolo-projects/auto-unlocker/releases/tag/v2.0.0a) and extract the zip file. Open the Unlocker.exe file and tap "Patch".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706697733024/4084182a-7665-428b-9766-676a3b769100.png align="center")

Once the patch is complete, close the app.

## Step 4: Creating the VM

This section will detail how to create the base virtual machine using the recovery disk we created earlier.

1. Open VMware and press File &gt; New Virtual Machine
    
2. Tap `Custom Installation` and start navigating through the New Virtual Machine setup screens.
    
3. In "Install from", select "Install OS later"
    
4. On the `Select a Guest Operating System` page, select `Apple Mac OS X`, as well as the corresponding macOS version
    
5. On the `Processor Configuration` page, select `1` for the `Number of processors`.
    
6. On the `Memory for the Virtual Machine` page, select at least 4096 MB (recommended 8096 MB)
    
7. On the `Network Type` page, select `Use network address translation (NAT)`.
    
8. On the `Select a Disk` page, select `Use an existing virtual disk`.
    
9. On the `Select an Existing Disk` page, browse & select the `recovery.vmdk` disk we created earlier.
    
10. Finish the setup.
    
11. Edit your VM's settings.
    
12. Add a new Hard disk:
    
    On the `Hardware Type` page, select `Hard Disk`.
    
    On the `Disk Type` page, select `SATA`.
    
    On the `Select a Disk` page, select `Create a new virtual disk`
    
    On the `Specify Disk Capacity` page, enter an amount that makes sense (min 50GB, recommended 80GB)
    
    Complete the Hard Disk setup.
    

## Step 4: Patching the Virtual Machine

1. First, navigate to your VM's files (C:\\Users\\&lt;username&gt;\\Documents\\Virtual Machines\\&lt;machine name&gt;).
    
2. Duplicate the `.vmx` file and append .bak to the end of it
    
3. Open your virtual machine's original `Configuration File` (`.vmx`) in a text editor
    
4. Paste this into the file:
    
    ```plaintext
    smc.version = "0"
    ```
    
5. If you have an AMD CPU (like me), also add the following to your `.vmx` file.
    
    ```plaintext
    cpuid.0.eax = "0000:0000:0000:0000:0000:0000:0000:1011"
    cpuid.0.ebx = "0111:0101:0110:1110:0110:0101:0100:0111"
    cpuid.0.ecx = "0110:1100:0110:0101:0111:0100:0110:1110"
    cpuid.0.edx = "0100:1001:0110:0101:0110:1110:0110:1001"
    cpuid.1.eax = "0000:0000:0000:0001:0000:0110:0111:0001"
    cpuid.1.ebx = "0000:0010:0000:0001:0000:1000:0000:0000"
    cpuid.1.ecx = "1000:0010:1001:1000:0010:0010:0000:0011"
    cpuid.1.edx = "0000:0111:1000:1011:1111:1011:1111:1111"
    ```
    
    1. If your OS in macOS Ventura, also add/edit this (if it's already in there, you don't need to add it again):
        
        ```plaintext
        ethernet0.virtualDev = "vmxnet3"
        ```
        

Now, save the file and try booting your VM. If everything goes well, you should have MacOS boot up!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706699605658/9dd12357-bf69-41c8-af1c-cbb6a67807b1.png align="center")

### **Common Issues**

* **Stuck on Apple Boot Logo**: If you attempt to boot your virtual machine and it's stuck on the Apple Boot Logo, do the following to fix the issue. Power off the Virtual Machine, then open the virtual machine's settings. Once the settings window opened beside the `Hardware` tab click on `Options`. Change the `Apple Mac OS X` selection to `Microsoft Windows` then click OK. Power on the virtual machine again. Once all installed then go back to settings and set it back to `Apple Mac OS X`
    
* **The CPU has been disabled by the guest operating system**: Enable virtualization in your computer's BIOS
    
* **Feature 'cpuid.ds' was absent, but must be present**: This is due to a corrupted vmx file. Try using a backup of your `.vmx` file and edit the `.vmx` file carefully this time.
    
* **Module 'featurecompat' power on failed**: In your `.vmx` file, make sure that your editor is *not* converting the quotes (`"`) to "greek" quotes
    
* **Not enought physical memory**: Run VMware as administrator
    

But we're not finished yet. Now, we need to install MacOS.

## Step 5: Installing MacOS

Once you're able to boot into the recovery setup, follow these steps to properly install macOS.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Note</strong>: You may want to power off your virtual machine and take a snapshot before continuing. If anything goes wrong during setup, you can always revert back to your snapshot.</div>
</div>

1. Start by selecting your language.
    
2. As soon as the "Recovery" page pops up, select `Disk Utility` and hit `Continue`.
    
3. On the `Disk Utility` page, select the Hard Disk you created (not `macOS Base System)`.
    
4. With the Hard Disk selected, click `Erase` in the top right of Disk Utility.
    
5. Close Disk Utility.
    
6. On the recovery page, select `Reinstall macOS <version>` and hit `Continue`.
    
7. Continue through the setup.
    
8. When prompted to select the disk to install macOS, select the disk you just formatted.
    
9. Once the operating system is installed, navigate through the macOS setup.
    
10. Create your macOS computer account, finish the setup, and login.
    

Congratulations! You've just created your first MacOS Virtual Machine!

Once everything is setup, you may edit the Virtual Machine's settings and remove the `recovery.vmdk` Hard Disk. Do not delete the Hard Disk you created manually (to store the install).

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">You may want to power off your virtual machine and take another snapshot before continuing.</div>
</div>

<div data-node-type="callout">
<div data-node-type="callout-emoji">⚠</div>
<div data-node-type="callout-text">By default, your macOS deployment will <em>not</em> support iServices such as iMessage. To solve this, please use <a target="_blank" rel="noopener noreferrer nofollow" href="https://docs.bluebubbles.app/server/advanced/macos-virtualization/running-a-macos-vm/enabling-imessage-in-a-vm" style="pointer-events: none">this guide</a>.</div>
</div>