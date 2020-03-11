<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Accessing files in WSL
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/misc/filesWSL/filesWSL.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

> If you are NOT a Windows user, you can safely ignore this section.

If you are using Windows for CS2030, chances are you are using Windows Subsystem for Linux.
You might be wondering: where do you find your files? Where should you keep your files?

First of all, we need to understand that WSL is a completely different system from Windows.
Windows is based on (for argument's sake, due to the long history of Windows), the Windows
NT kernel, while Linux is based on, well, Linux kernel. Let's just say Linux is Unix-like.

As such, the ways they manage their files (filesystem) are very different as well. Windows
prefers NTFS (the Windows NT File System), while Linux prefers ext4 or other filesystems.

These filesystems affect the properties of a file/folder, called **metadata**. They are 
managed by filesystems, and contain information such as date modified, date created, or 
**permissions**. The latter is bold because that is the most important aspect in a file's
metadata.

One can imagine that modifying Linux files in Windows may result in Windows **not** meeting
the metadata requirements of Linux's filesystem, such that when the file/folder is accessed
with WSL (Linux), errors such as file lock, insufficient permissions, or even corruption 
may occur.

Therefore, even though one knows that the `home` (or, tilde `~`) directory for WSL is located
at 
```
C:\Users\%USERNAME%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\home
```
this directory **shall never be touched or accessed through Windows' File Explorer** as 
Windows may unintentionally replace the metadata of whatever related to that directory, causing
problems later in WSL. An experienced WSL user always knows this.

Now, you may ask, how do we access our files then? There are two ways to access WSL files 
safely and **correctly**, as advised by Microsoft:
1. Accessing the root filesystem `/` directory with `\\wsl$\<DistroName>\` (e.g. `\\wsl$\Ubuntu\`
for Ubuntu users, `\\wsl$\Kali\` users, etc). This will create a specialised P9 fileserver, which
will expose the Linux file system as a local network directory in Windows. Whatever you do there
will be properly adjusted by the magic of Windows x WSL.

2. Accessing the Linux directory by invoking `explorer.exe .` (yes, the last `.` there is NOT a 
typo) **in WSL**. This will open up a File Explorer window and redirect you to your current
working directory. The directory opened is also a form of the aforementioned P9 fileserver.

If the problems caused by accessing the Linux files through `AppData` is just a permissions 
mismatch, one can fix by using `chmod`. But, if the problem is file corruption, there might not 
be a way back. Therefore, this section intends to disseminate **the correct way** of accessing 
files in WSL. I hope you will become one of the informed ones.

You can read more about this issue in [this Microsoft DevBlogs article](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/).
