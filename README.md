# PortableDevices.Plus
**PortableDevices.Plus** is a .NET wrapper around WIN32_API classes, which allows you to communicate with music players, storage devices, mobile phones, cameras and many other types of connected devices the easy way.


To transfer files, create folders and make other wondrous things through MTP with C#:

##### Download this NuGet package:
[Portable Devices.Plus.x64](https://www.nuget.org/packages/PortableDevices.Plus.x64/) or [Portable Devices.Plus.x86](https://www.nuget.org/packages/PortableDevices.Plus.x86/)

##### Add references to these 4 COM libraries:

- PortableDeviceClassExtension
- PortableDeviceConnectApi
- PortableDeviceTypes
- PortableDeviceApi

##### Take the following dll's under obj\Debug (or get them from CompiledDLLs folder in this repo) and put them into bin\Debug:

- Interop.PortableDeviceClassExtension.dll
- Interop.PortableDeviceConnectApiLib.dll
- Interop.PortableDeviceTypesLib.dll
- Interop.PortableDeviceApiLib.dll


##### Sample usage:

```csharp
            //Creating instances
            PortableDevice tablet;
            PortableDeviceCollection devices = new PortableDeviceCollection();
            devices.Refresh();
			
            //Connect to MTP devices and pick up the first one
            tablet = devices.First();
            tablet.Connect();

            //Getting root directory
            var root = tablet.GetContents();

	    //Finding neccessary folder inside the root
            var folder = (root.Files.FirstOrDefault() as PortableDeviceFolder).
                Files.FirstOrDefault(x => x.Name == "Folder") as PortableDeviceFolder;

            //Finding file inside the folder
            var file = (folder as PortableDeviceFolder)?.Files?.FirstOrDefault(x => x.Name == "File");

            //Deleting file device-side
            Tablet.DeleteFile(file as PortableDeviceFile);

            //Transfering file into byte array
            var fileIntoByteArr = Tablet.DownloadFileToStream(file as PortableDeviceFile);

            //Transfering file into file system
            Tablet.DownloadFile(file as PortableDeviceFile, "\\LOCALPATH");

            //Transfering file from file system into device folder
            Tablet.TransferFileToDevice("\\LOCALPATH", folder.Id);

            //Creating folder device-side
            Tablet.CreateFolder("FOLDER NAME", folder.Id);
        
            //Deleting folder device-side
            Tablet.DeleteFolder(folder);
			
            //Close the connection
            tablet.Disconnect();
```

##### License:
MIT
