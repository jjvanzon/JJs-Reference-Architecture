<style>table td:first-child { white-space: nowrap; vertical-align:top; }</style>

Multi-Platform Compatibility Issues
===================================

*JJ van Zon, 2014*

<h2>Contents</h2>

- [Introduction](#introduction)
- [Unity 4.3.4 / .NET Compatibility Issues](#unity-434--net-compatibility-issues)
    - [Use .NET 3.5](#use-net-35)
    - [Copy your Compiled Assemblies](#copy-your-compiled-assemblies)
    - [Target .NET 2.0](#target-net-20)
    - [Copy Mono .NET Assemblies](#copy-mono-net-assemblies)
- [Unity Free 4.3.4 Compatibility Issues](#unity-free-434-compatibility-issues)
    - [System.Net.Sockets](#systemnetsockets)
- [Android / Unity 4.3.4 Issues](#android--unity-434-issues)
    - [Install Android SDK](#install-android-sdk)
    - [Build and Run may not Work](#build-and-run-may-not-work)
    - [Installing Deployment Files](#installing-deployment-files)
- [Windows Phone 8 Compatibility Issues](#windows-phone-8-compatibility-issues)
    - [Windows 8](#windows-8)
    - [System.Configuration](#systemconfiguration)
    - [System.Xml](#systemxml)
    - [System.Type.GetInterface(string name)](#systemtypegetinterfacestring-name)
    - [System.Xml.Linq limitations](#systemxmllinq-limitations)
    - [CultureInfo.GetCultureInfo(string)](#cultureinfogetcultureinfostring)
- [Windows Phone 8 / Unity 4.3.4 Compatibility Issues](#windows-phone-8--unity-434-compatibility-issues)
    - [System.Reflection.MemberInfo.MemberType](#systemreflectionmemberinfomembertype)
    - [Application.persistentDataPath (in Unity)](#applicationpersistentdatapath-in-unity)
    - [Type.GetProperties(BindingFlags)](#typegetpropertiesbindingflags)
    - [Mono .NET Assemblies](#mono-net-assemblies)
    - [Resource Assemblies](#resource-assemblies)
    - [Resource Assemblies](#resource-assemblies-1)
    - [“Development Build” Keeps Showing](#development-build-keeps-showing)
    - [System.ServiceModel](#systemservicemodel)
    - [Constructor WWW(url, byte[], header)](#constructor-wwwurl-byte-header)
    - [Encoding.GetString](#encodinggetstring)
    - [System.Diagnostics.Trace Class](#systemdiagnosticstrace-class)
- [Mac OS / Unity 4.3.4 Compatibility Issues](#mac-os--unity-434-compatibility-issues)
    - [Copy System.Runtime.Serialization](#copy-systemruntimeserialization)
    - [Mac OS Version](#mac-os-version)
    - [Connecting to Windows Share](#connecting-to-windows-share)
    - [Errors in Unity that can be Ignored](#errors-in-unity-that-can-be-ignored)
- [iOS 6 / Unity 4.3.4 Compatibility Issues](#ios-6--unity-434-compatibility-issues)
    - [Mac OS](#mac-os)
    - [JIT Compilation](#jit-compilation)
    - [PropertyInfo.GetValue()](#propertyinfogetvalue)
    - [Thread.CurrentThread.SetUICulture](#threadcurrentthreadsetuiculture)
    - [Thread.CurrentThread.SetCulture](#threadcurrentthreadsetculture)
    - [Compile for Release](#compile-for-release)
    - [System.ServiceModel](#systemservicemodel-1)
    - [System.Web.Services](#systemwebservices)
- [Information](#information)


Introduction
------------

Deploying .NET code to mobile platforms meant dealing with a variety of compatibility issues. This document lists the issues found while deploying of .NET code to iOS, Windows Phone and Android. This document may not be exhaustive and might be extended in the future as new issues come to light. The deployment was done using a tool called Unity, which is a Mono-based game engine.


Unity 4.3.4 / .NET Compatibility Issues
---------------------------------------

The following things needed to be taken into consideration to make the .NET code work in Unity.

### Use .NET 3.5

Unity / Mono (as of 2014-05-17) seems only compatible with .NET 3.5, not 4.0 or 4.5, so things were targeted to .NET 3.5 in the .NET code and workarounds were made for missing features.

### Copy your Compiled Assemblies

To use self-made .NET assemblies in Unity, they were copied to the `Assets` folder or a sub-folder there in the Unity project.

### Target .NET 2.0

You have to target .NET 2.0 in Unity (not '.NET 2.0 Subset')  
(<http://answers.unity3d.com/questions/30881/compiling-error-with-mysql-pls-help.html>)

### Copy Mono .NET Assemblies

You might need to use the Mono versions of .NET assemblies that are not included by default in your Unity projects. To work with .NET assemblies not standardly included by Unity (for instance System.ServiceModel and System.Runtime.Serialization) you need to copy the dll’s from the Unity program files to your Unity project. The location of these dll’s is the program files folder of Unity and then: `Editor/Data/Mono/lib/mono/2.0`. They have to be copied to your Unity project somewhere in the Assets folder. You might want to create a new sub-folder in your `Assets` folder, possibly named `Plugins`.


Unity Free 4.3.4 Compatibility Issues
-------------------------------------

### System.Net.Sockets

Unity Free does not support deploying applications that use `System.Net.Sockets`. You would need Unity Pro for that. This also means any part of .NET that indirectly uses System.Net.Sockets. So basically connecting to a network from your .NET code is disabled. You can use it in the emulator in Unity, but you cannot deploy it to a device. If you want to deploy, you must remove the `System.ServiceModel.dll` from your Assets folder and anything else that uses `System.Net.Sockets`.


Android / Unity 4.3.4 Issues
----------------------------

### Install Android SDK

For the Android deployment to work, you need to install Android SDK. 

### Build and Run may not Work

Building the APK file may work, while running it on the device directly from Untiy might not work.  
Install the APK deployment file on the device instead.

### Installing Deployment Files

You can install the APK file on your Android device by simply copying the file some somewhere on the device when you have connected it to your PC. Find a file browser that works on Android (e.g. ASTRO File Manager), find the APK file and click it. 


Windows Phone 8 Compatibility Issues
------------------------------------

### Windows 8

You need Windows 8 to deploy to Windows Phone 8. You cannot do it with Windows 7.

### System.Configuration

Not supported. (You can prevent linking problems by loading the assembly dynamically, but you cannot use it in runtime.)

### System.Xml

Support limited up until the point of unusability. `System.Xml.Linq` is an alternative. 

### System.Type.GetInterface(string name)

Not supported. Alternative: `System.Type.GetInterface(string name, bool ignoreCase)`

### System.Xml.Linq limitations

On Windows Phone 8:

- `XDocument.Save(string fileName)` does not exist.
- `XElement.Save(string fileName)` does not exist either.
- `XElement.Save(Stream)` exists on Windows Phone 8, but not in .NET 3.5.
- so use `XElement.Save(TextWriter)`.

### CultureInfo.GetCultureInfo(string)

Not supported. Alternative: `new CultureInfo(string)`. 


Windows Phone 8 / Unity 4.3.4 Compatibility Issues
--------------------------------------------------

The items below are compatibility problems related to how Unity interoperates with Windows Phone.

### System.Reflection.MemberInfo.MemberType

Threw a strange exception when deployed using Unity: `Method not found: 'System.Reflection.MemberTypes'`

Officially it is supported:  
<http://msdn.microsoft.com/en-us/library/windowsphone/develop/system.reflection.memberinfo.membertype%28v=vs.105%29.aspx>

Maybe not supported on lower Windows Phone .NET framework version?

Alternative code:

```cs
if (memberInfo is FieldInfo) { ... }
if (memberInfo is PropertyInfo) { ... }
// etc.
```

### Application.persistentDataPath (in Unity)

Persistent data path cannot be an absolute path on Windows Phone. Unity’s `Application.persistentDataPath` returns an absolute path, which the Windows Phone `System.Linq.Xml` API cannot handle. The alternative for now is to do an `if` in the Unity code:

```cs
string folderPath;
if (Application.platform == RuntimePlatform.WP8Player) { folderPath = ""; }
else { folderPath = Application.persistentDataPath; }
```

A better alternative might be finding a property in the Unity framework that does return a path you can write your files to, that works on all the targeted platforms.

### Type.GetProperties(BindingFlags)

In runtime it says the method `Type.GetProperties(BindingFlags)` is not found.  
This post says they know about the bug, they found the problem, but they won’t fix it; just copy a file from an earlier Unity version...  
<http://forum.unity3d.com/threads/223065-Unity-4-3-3-Type-GetMembers%28BindingFlags%29-crash>  
Basically, the programmers of Unity added types to their library of .NET stubs (or something) that are already present in the Windows Phone 8.0 version of the .NET framework and this results in two types with the same name arbitrarily used in different places. When a piece of code runs, that wants the two types to be the same, it crashes with a method not found exception.  
The workaround mentioned in the post is to install version 4.3.2 of Unity in a seperate folder and replace the original Unity (4.3.3) installation’s:  
`Editor\Data\PlaybackEngines\wp8support\Managed\Win RTLegacy.dll`
with the one from version 4.3.2.  
It also works if your original Unity version is 4.3.4.  

### Mono .NET Assemblies

It won’t deploy to Windows Phone with the mono `System.Runtime.Serialization` in the `Assets` folder.  
Probably any .NET Mono assembly you put in the `Assets` folder will give you problems when deploying to Windows Phone (not tested).

### Resource Assemblies

Unity does not automatically deploy the ‘satellite assemblies’ with translations of texts into multiple languages. The trick is to simply copy the folders such as `nl-NL` and `en-US` out of the .NET `bin` folder, to the Windows Phone deployment folder of your Unity project. The 'language' folders should be placed right beside where the Unity puts the `.csproj` file when you do a Windows Phone build.

### Resource Assemblies

To make translations in resource files work, you need to check the supported cultures in the Properties editor in the Windows Phone 8 Visual Studio project.

### “Development Build” Keeps Showing

In the Visual Studio project, select the Configuration “Master” instead of “Release” or “Debug” to make the text “Development Build” disappear from the application running on the phone.

### System.ServiceModel

`[ NOT TESTED ]`

Does not seem to work. When trying to instantiate a service client, you get the exception: `Entry point not found`. A solution might be to program our own SOAP client based on `System.Xml.Linq`.

### Constructor WWW(url, byte[], header)

The `WWW` class is a Unity framework class that allows you to send HTTP requests. The header parameter of this constructor is a `Dictionary<string, string>` for Windows Phone 8 and a `Hashtable` for Android and iOS. Use the compiler directive `UNITY_WP8` to use a variable of a different type. 

### Encoding.GetString

Does not have the overload that takes `byte[]`, but does have an overload with `byte[], index and count`.

### System.Diagnostics.Trace Class

Not available on Windows Phone 8. (This makes `JJ.Framework.Logging` not usable.) 

Mac OS / Unity 4.3.4 Compatibility Issues
-----------------------------------------

### Copy System.Runtime.Serialization

On Mac OS, the Unity emulator will not run a program that requires `System.Runtime`.Serialization if you do not copy the Mono version of the DLL to the `Assets` folder of the Unity project.

The location of these dll’s in the Unity program files on Windows is (the program files folder of Unity and then: `Editor/Data/Mono/lib/mono/2.0` (The location on Mac OS might be similar.)

### Mac OS Version

Mac that is too old or whose system specs are too low, might not run the XCode version that is required to deploy to your particular version of iOS. XCode 4.6 is required for deployment to iOS 6, which requires Mac OS X 10.6.7 Lion, which requires a minimum of 2 GB of memory. XCode 4.2 is the highest version that will run on Mac OS X 10.6.8 Snow Leopard. XCode 3.2 is not a high enough version to run the project that is output by Unity.

### Connecting to Windows Share

Connecting to a Windows share from Mac OS hangs on my Mac all the time. It works one time, and then not anymore. Alternatively you can turn on sharing on the Mac and connect to the shares from Windows by simply going to [\\192.168.1.1\](\\192.168.1.1\) (numbers may vary). You could also RDP from the Mac to the Windows machine, configure that you can access local local folders on the Mac from your RDP session and then copy files from the Windows PC to the Mac inside the RDP session. You can also use VNC to connect from a Windows machine to a Mac, which might be convenient, but might also have bad performance.

### Errors in Unity that can be Ignored

You can get the following errors in Unity. The deployment will still work, even when you ignore the errors (if you use the ‘Build’ button, not ‘Build and Run’ and then run it from XCode):

- `Socket: bind failed, error: Address already in use (48)`
- `Unable to join player connection multicast group` 


iOS 6 / Unity 4.3.4 Compatibility Issues
----------------------------------------

### Mac OS

You definitely need Mac OS in order to deploy to iOS. You cannot do it in Windows.

### JIT Compilation

Any JIT compilation is not permitted on iOS. Some functions of .NET (or the Mono versions of .NET) automatically JIT something on the fly.

(Sources: Google: "unity" "ios" "propertyinfo.GetValue" <http://forum.unity3d.com/threads/43038-ExecutionEngineException-Attempting-to-JIT-compile-method>)

### PropertyInfo.GetValue()

Might JIT compile under the hood, which is not permitted, when it is performed on a generic type argument, e.g. `typeof(T).GetProperty().GetValue()`  
Alternative:
`PropertyInfo.GetGetMethod().Invoke()`  
There may be a faster alternative, but it is more complicated:  
<http://whydoidoit.com/2012/04/18/faster-invoke-for-reflected-property-access-and-method-invocation-with-aot-compilation/>

### Thread.CurrentThread.SetUICulture

Will do JIT compilation under the hood, which is not permitted. For resource texts you can set the Culture property of the resource classes themselves. If those are not public you might be able to use reflection to access them (not tested).

### Thread.CurrentThread.SetCulture

Will do JIT compilation under the hood, which is not permitted. Alternative for number formats etc. is yet to be determined. It might be to simply pass the culture to the formatting functions explicitly as the formatProvider.

### Compile for Release

Resources might not work when you do not compile .NET assemblies for release.

### System.ServiceModel

`[ NOT TESTED ]`

Not supported. Cross-compilation fails because the Mono implementation of `System.ServiceModel` references parts of `Mono.WebBrowser`, which is not supported on iOS 6.  
A solution is to program your own SOAP client based on `System.Xml.Linq`.

### System.Web.Services

`[ NOT TESTED ]`

Not supported. When trying to send a message, the `XmlSerializer` is used, which calls `MonoProperty.GetValue`, which attempts to JIT Compile, which is not supported.  
A solution is to program your own SOAP client based on `System.Xml.Linq`.  


Information
-----------

About compatibility between Mac OS versions and versions of Xcode:

<https://discussions.apple.com/thread/3924758>

Downloading specific versions of Mac software:

<https://developer.apple.com/downloads/index.action#>

iOS deployment using Unity:

<http://mobilegbl.wordpress.com/2013/05/21/from-unity-to-an-ios-device/>

Google "multi-platform solutions"

<http://simpleprogrammer.com/2013/07/01/cross-platform-mobile-development>

<http://sixrevisions.com/mobile/cross-platform-mobile-apps/>
