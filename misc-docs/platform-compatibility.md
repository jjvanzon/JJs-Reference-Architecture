⚠ Platform Compatibility 
=========================

[back](..)

<h3>Contents</h3>

- [Introduction](#introduction)
- [Unity 4.3.4 / .NET Compatibility Issues](#unity-434--net-compatibility-issues)
- [Unity Free 4.3.4 Compatibility Issues](#unity-free-434-compatibility-issues)
- [Android / Unity 4.3.4 Issues](#android--unity-434-issues)
- [Windows Phone 8 Compatibility Issues](#windows-phone-8-compatibility-issues)
- [Windows Phone 8 / Unity 4.3.4 Compatibility Issues](#windows-phone-8--unity-434-compatibility-issues)
- [Mac OS / Unity 4.3.4 Compatibility Issues](#mac-os--unity-434-compatibility-issues)
- [iOS 6 / Unity 4.3.4 Compatibility Issues](#ios-6--unity-434-compatibility-issues)
- [Information](#information)


Introduction
------------

Deploying [`.NET`](https://dotnet.microsoft.com/) code to mobile platforms meant dealing with various compatibility issues. This document lists the issues found while deploying of [`.NET`](https://dotnet.microsoft.com/) code to iOS, Windows Phone and Android. The deployment was done using a tool called Unity, which is a Mono-based game engine.

The project during which this was done was [`JJ.SaveText`](https://github.com/jjvanzon/JJ.SaveText).

Some work-arounds had been given a place in [`JJ.Framework.PlaformCompatibility`](https://www.nuget.org/packages/JJ.Framework.PlatformCompatibility) to keep overview of these issues. But that package may not be relevant for later [`.NET`](https://dotnet.microsoft.com/) versions anymore.


Unity 4.3.4 / .NET Compatibility Issues
---------------------------------------

The following things might need to be taken into consideration to make the [`.NET`](https://dotnet.microsoft.com/) code work in Unity.

- ### Use .NET 3.5

  Unity / Mono (as of 2014-05-17) appears to be only compatible with .NET 3.5, not 4.0 or 4.5, so things were targeted to .NET 3.5 in the [`.NET`](https://dotnet.microsoft.com/) code and workarounds were made for missing features.

- ### Copy your own Compiled Assemblies

  To use self-made [`.NET`](https://dotnet.microsoft.com/) assemblies in Unity, they were copied to the `Assets` folder or a sub-folder in there in the Unity project.

- ### Target .NET 2.0

  ".NET 2.0" was targeted in Unity. (".NET 2.0 Subset" would not work.)  
  (<http://answers.unity3d.com/questions/30881/compiling-error-with-mysql-pls-help.html>)

- ### Copy Mono .NET Assemblies

  The Mono versions of [`.NET`](https://dotnet.microsoft.com/) assemblies might need to be used, that are not included by default in the Unity projects. To work with [`.NET`](https://dotnet.microsoft.com/) assemblies not standardly included by Unity (for instance `System.ServiceModel` and `System.Runtime.Serialization`) the dll's were copied from the Unity program files to the Unity project. The location of these dll's was in the program files folder of Unity and then: `Editor/Data/Mono/lib/mono/2.0`. They were copied to the Unity project somewhere in the `Assets` folder. It might be an idea to create a new sub-folder in the `Assets` folder and possibly name it `Plugins`.


Unity Free 4.3.4 Compatibility Issues
-------------------------------------

- ### System.Net.Sockets

  Unity Free did not support deploying applications that use `System.Net.Sockets`. That only seemed available in Unity Pro. This also means any part of [`.NET`](https://dotnet.microsoft.com/) that indirectly uses `System.Net.Sockets`. So basically connecting to a network from the [`.NET`](https://dotnet.microsoft.com/) code is disabled. The emulator in Unity could use it, but deploying it to a device did not work. To make the deployment succeed, removing `System.ServiceModel.dll` from the `Assets` folder might work and anything else that uses `System.Net.Sockets`.


Android / Unity 4.3.4 Issues
----------------------------

- ### Install Android SDK

  For the Android deployment to work, Android SDK was installed. 

- ### Build and Run may not Work

  Building the APK file may work, while running it on the device directly from Untiy does not.  
  The APK deployment file might be installed on the device instead.

- ### Installing Deployment Files

  The APK file can be installed on the Android device by copying the file some somewhere on the device when it is  connected it to the PC. A file file browser that works on Android may help (e.g. ASTRO File Manager). Find the APK file and click it. 


Windows Phone 8 Compatibility Issues
------------------------------------

- ### Windows 8

  Windows 8 was minimally needed to deploy to Windows Phone 8. It appeared not to be possible from Windows 7.

- ### System.Configuration

  Was not supported. (Linking problems could be prevented, by loading the assembly dynamically, but it could not be used it in runtime on Windows Phone 8.)

- ### System.Xml

  Support limited up to the point that it was practically unusable. `System.Xml.Linq` was the alternative. 

- ### System.Type.GetInterface(string name)

  Not supported by Windows Phone 8. Alternative: `System.Type.GetInterface(string name, bool ignoreCase)`

- ### System.Xml.Linq limitations

  On Windows Phone 8:

    - `XDocument.Save(string fileName)` did not exist.
    - `XElement.Save(string fileName)` did not exist either.
    - `XElement.Save(Stream)` existed on Windows Phone 8, but not in .NET 3.5.
    - so `XElement.Save(TextWriter)` was used instead.

- ### CultureInfo.GetCultureInfo(string)

  Not supported. Alternative: `new CultureInfo(string)`. 


Windows Phone 8 / Unity 4.3.4 Compatibility Issues
--------------------------------------------------

The items below were compatibility problems related to how Unity interoperated with Windows Phone.

- ### System.Reflection.MemberInfo.MemberType

  Threw an exception when deployed using Unity: `Method not found: 'System.Reflection.MemberTypes'`

  Officially it is supported:  
  <http://msdn.microsoft.com/en-us/library/windowsphone/develop/system.reflection.memberinfo.membertype%28v=vs.105%29.aspx>

  Maybe not supported on lower Windows Phone .NET framework version?

  Alternative code:

  ```cs
  if (memberInfo is FieldInfo) { ... }
  if (memberInfo is PropertyInfo) { ... }
  // etc.
  ```

- ### Application.persistentDataPath (in Unity)

  `persistentDataPath` could not be an absolute path on Windows Phone. Unity's `Application.persistentDataPath` returned an absolute path, which the Windows Phone `System.Linq.Xml` API did not appear to handle. The alternative for now would be to do an `if` in the Unity code:

  ```cs
  string folderPath;
  if (Application.platform == RuntimePlatform.WP8Player) { folderPath = ""; }
  else { folderPath = Application.persistentDataPath; }
  ```

  A better alternative might be finding a property in the Unity framework that does return a path you can write files to, that works on all the targeted platforms.

- ### Type.GetProperties(BindingFlags)

  In runtime it said the method `Type.GetProperties(BindingFlags)` was not found.  

  This post said they know about the bug, they found the problem, but they won't fix it; just copy a file from an earlier Unity version they say...  
  <http://forum.unity3d.com/threads/223065-Unity-4-3-3-Type-GetMembers%28BindingFlags%29-crash>  

  It appeared the programmers of Unity added types to their library of [`.NET`](https://dotnet.microsoft.com/) stubs (or something) that are already present in the Windows Phone 8 version of the .NET Framework and this seems to result in two types with the same name arbitrarily used in different places. When a piece of code runs, that wants the two types to be the same, it appears to crash with a method not found exception.  

  The workaround mentioned in the post was to install version 4.3.2 of Unity in a seperate folder and replace the original Unity (4.3.3) installation's:  
  `Editor\Data\PlaybackEngines\wp8support\Managed\Win RTLegacy.dll`
  with the one from version 4.3.2.  
  It also worked if the original Unity version was 4.3.4.  

- ### Mono .NET Assemblies

  It wouldn't deploy to Windows Phone with the mono `System.Runtime.Serialization` in the `Assets` folder.  
  Possibly any .NET Mono assembly put in the `Assets` folder may give problems when deploying to Windows Phone (not tested).

- ### Resource Assemblies

  Unity would not automatically deploy the 'satellite assemblies' with translations of texts into multiple languages. A trick that appeared to work, was to simply copy the folders such as `nl-NL` and `en-US` out of the [`.NET`](https://dotnet.microsoft.com/) `bin` folder, to the Windows Phone deployment folder of the Unity project. The 'language' folders would be placed right beside where the Unity put the `.csproj` file when doing a Windows Phone build.

- ### Resource Assemblies

  To make translations in resource files work, the supported cultures in the Properties editor in the Windows Phone 8 Visual Studio project needed to be checked.

- ### "Development Build" Kept Showing

  In the Visual Studio project, selecting the Configuration "Master" instead of "Release" or "Debug" made the text "Development Build" disappear from the application running on the phone.

- ### System.ServiceModel

  `[ NOT TESTED ]`

  Does not seem to work. When trying to instantiate a service client, the following exception showed: `Entry point not found`. A solution might be to program our own SOAP client based on `System.Xml.Linq`.

  This was even partially achieved in the project [`JJ.Framework.Soap`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Soap/overview)

- ### Constructor WWW(url, byte[], header)

  The `WWW` class is a Unity framework class that allows sending HTTP requests. The header parameter of this constructor is a `Dictionary<string, string>` for Windows Phone 8 and a `Hashtable` for Android and iOS. Used the compiler directive `UNITY_WP8` to use a variable of a different type. 

- ### Encoding.GetString

  Was missing the overload that takes `byte[]`, but did have an overload with `byte[], index and count`.

- ### System.Diagnostics.Trace Class

  Not available on Windows Phone 8. (This made [`JJ.Framework.Logging`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Logging/overview) not usable.) 

Mac OS / Unity 4.3.4 Compatibility Issues
-----------------------------------------

- ### Copy System.Runtime.Serialization

  On Mac OS, the Unity emulator would not run a program that required `System.Runtime.Serialization`, unless the Mono version of the DLL is copied to the `Assets` folder of the Unity project.

  The location of these dll's in the Unity program files on Windows was (the program files folder of Unity and then: `Editor/Data/Mono/lib/mono/2.0` (The location on Mac OS might be similar.)

- ### Mac OS Version

  Mac that is too old or whose system specs are too low, might not run the XCode version that is required to deploy to a particular version of iOS. XCode 4.6 seems to be required for deployment to iOS 6, which would require Mac OS X 10.6.7 Lion, which would require a minimum of 2 GB of memory. XCode 4.2 would be the highest version that would run on Mac OS X 10.6.8 Snow Leopard. XCode 3.2 [4.2?] was not a high enough version to run the project that is output by Unity. So quite a few conditions to meet.

- ### Connecting to Windows Share

  Connecting to a Windows share from Mac OS would hang on my Mac a lot. It would work one time, and then not anymore. Alternatively I turned on sharing on the Mac instead and connected to it from Windows by going to `\\192.168.1.1\` (numbers may vary). I also tried RDP'ing from the Mac to the Windows machine, configured to allow accessing local local folders on the Mac from the RDP session and then copied files from the Windows PC to the Mac inside the RDP session. I could also use VNC to connect from a Windows machine to a Mac, which might have been convenient, but performance was not great.

- ### Errors in Unity that can be Ignored

  The following errors may appear in Unity. The deployment might still work, even when ignoring the errors (using the 'Build' button, not 'Build and Run' and then run it from XCode):

    - `Socket: bind failed, error: Address already in use (48)`
    - `Unable to join player connection multicast group` 


iOS 6 / Unity 4.3.4 Compatibility Issues
----------------------------------------

- ### Mac OS

  Mac OS was needed to deploy to iOS. It does not seem to be possible on Windows.

- ### JIT Compilation

  Any JIT compilation might not be permitted on iOS. Some functions of [`.NET`](https://dotnet.microsoft.com/) (or the Mono versions of `.NET`) automatically seem to JIT something on the fly.

  (Sources: Google: "unity" "ios" "propertyinfo.GetValue" <http://forum.unity3d.com/threads/43038-ExecutionEngineException-Attempting-to-JIT-compile-method>)

- ### PropertyInfo.GetValue()

  Might JIT compile under the hood, which was not permitted by iOS, when it is performed on a generic type argument, e.g.
  
  ```cs
  typeof(T).GetProperty().GetValue()
	```

  Alternative:

  ```cs
  PropertyInfo.GetGetMethod().Invoke()
  ```

  There may be a faster alternative, but it seemed more complicated:  
  <http://whydoidoit.com/2012/04/18/faster-invoke-for-reflected-property-access-and-method-invocation-with-aot-compilation/>

- ### Thread.CurrentThread.SetUICulture

  Would do JIT compilation under the hood, which is not permitted. For resource texts you can set the Culture property of the resource classes themselves. If those are not public you might be able to use reflection to access them (not tested).

- ### Thread.CurrentThread.SetCulture

  Will do JIT compilation under the hood, which is not permitted. Alternative for number formats etc. is yet to be determined. It might be to simply pass the culture to the formatting functions explicitly as the `formatProvider`.

- ### Compile for Release

  Resources might not work when you do not compile [`.NET`](https://dotnet.microsoft.com/) assemblies for release.


- ### System.ServiceModel

  `[ NOT TESTED ]`

  Not supported. Cross-compilation fails because the Mono implementation of `System.ServiceModel` references parts of `Mono.WebBrowser`, which is not supported on iOS 6.  
  A solution could be to program a SOAP client ourselves based on `System.Xml.Linq`.

  This was partially achieved in the project [`JJ.Framework.Soap`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Soap/overview).

- ### System.Web.Services

  `[ NOT TESTED ]`

  Not supported. When trying to send a message, the `XmlSerializer` is used, which calls `MonoProperty.GetValue`, which attempts to JIT Compile, which is not supported.  
  A solution could be to program a SOAP client ourselves based on `System.Xml.Linq`.


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

[back](..)
