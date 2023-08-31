---
title: IT Support
slug: it-support
aliases: ksc
summary:
description: 
date: 2023-08-25T19:36:35+03:00
categories: [Technical]
tags: []
draft: false
---
{{< alert icon="circle-info" >}}
Use Table of Content to quickly jump to desired section 
{{< /alert >}}

## Where to download Windows ISO files

For Windows disk image (ISO) files, you can get them directly from Microsoft via these links:
- [Windows 10](https://www.microsoft.com/en-us/software-download/windows10ISO)
- [Windows 11](https://www.microsoft.com/software-download/windows11)

## Office image files 

## Activating Windows 
1. Disable your third-party antivirus if applicable. For ESET, this will mean disabling real-time protection and HIPS.
2. Windows will renable Windows Security, so we will need to disable it. 
 - Head over to virus and threat protection settings, disable all the options 
 - Head over to app and browser control, then into reputation based settings and disable all the options 
 - For Windows 8/8.1 open Windows Defender and disable realtime protection
3. Open KMS Auto Net (do not unzip)
4. Run the executable 
5. Select activation and pick Windows
6. After activation, reenable Windows Security 
7. Reenable your third-party antivirus


## Activating Office 

**For Office 2016**, follow the same instructions as Windows but upon running the KMS software, select Office.

**For Office 2019**, open powershell as an administrator and run this command:

**For Office 2021:**
- Copy the script below into notepad and save it.
- Rename the file to change the file extension to .cmd (instead of .txt). 
  
  *If you can't see the file extension, head over to File Explorer and enable file extensions.*
- Run the script as administrator

If it fails to reach a server, try over and over. When successful, it will prompt you to read the BLOG_NAME blog. Type Y if you'd like to read it or N for no. 

## Resolve 'Get Genuine Office' Warning

If Office prompts you to get genuine office, run the script below. What it does is downgrade the office to an older version that doesn't have this warning. After which you can disable office updates otherwise it will upgrade back to a higher version with the same warning 

## Flash Windows onto a USB stick

### All Windows Version 
- Connect USB stick to computer 
- Run rufus software
- Ensure the program has selected the correct external storage (your USB stick)
- Click select to pick your Windows ISO file 
- Choose between GPT or MBR partition scheme depending on the computer you want to install Windows. General rule of thumb, pick GPT for newer computers and MBR for older ones
- Leave the remaining options as is. 
- Click start
- Once complete you can eject the USB stick (except for Windows 8.1, continue with the guide below)

### Windows 8.1 skip product key

By default, Windows 8.1 installation does not have the option to skip adding a product key but we can bring it back. 

After flashing Windows 8.1 onto a USB stick, open file explorer and head over to `/sources` folder in the USB stick.
Create a text file called `ei.cfg` and paste this. 

```text
[EditionID]

[Channel]
Retail
[VL]
0
```

Source: [Elmo on StackExchange](https://superuser.com/a/498437)

## Some Useful Tools: 
1. [Rufus](https://rufus.ie/en/)
2. [Ventoy](https://www.ventoy.net/)
3. [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
