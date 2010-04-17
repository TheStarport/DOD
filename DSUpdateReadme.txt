Discovery Update - v1.0
An automatic update program for Freelancer by Cannon

Overview
========

This is a simple automatic update program for Freelancer. When Freelancer 
is started it will connect to an update server and download and install any 
required updates. Once the update is complete, Freelancer will be restarted 
automatically.

How it works
=============

When Freelancer is started, the file DSUpdate.dll is loaded and executed. This 
file reads the 'dsupdate.cfg' file and retrieves the list of urls. The program 
selects one of the urls an downloads the 'dsversion.cfg' file.

The program compares the version number in the 'dsversion.cfg' file to 
the version number in the dsupdate.cfg file. If the version number of the 
installation is less than the one in the dsversion.cfg then the Freelancer
installation is considered to be out of date. If the installation is not out
date then Freelancer is launched.

If the installation is out of date, Freelancer is minimised and the DSUpdate.exe
program is executed. This program requires administrator priviledges and will
request them in you're running on Vista.

The DSUpdate.exe program again contacts the update server and checks for an update.
If  the Freelancer installation is considered to be out of date, then Freelancer is
terminated and the user is asked if they wish to download the update. 

Once the update is downloaded, the update is extracted into the Freelancer
folder and optionally a FLMM mod folder and then Freelancer is launched.

Up to 9 urls can be defined in the 'dsversion.cfg' file. The 
program will randomly select one the urls and download an update zip 
file containing the updated game data files.

How to include the updater with your mod.
=========================================

1. Copy all of the files in the bin directory into the freelancer/exe directory
   of your mod. The line DSUpdate.dll needs to be added to the libraries section
   of dacom.ini. Either use the dacom.ini included in the bin directory or add
   it to you your one manually.

2. Edit the dsupdate.cfg file 'updateserver1' parameter to point to your update 
   server. The dsupdate.cfg file must be in the exe directory.
   
3. Edit the dsupdate.cfg file to contain the current version number of your mod.   

See the dsupdate.cfg file for a list of the other parameters.  


How to release an update
========================

Create a zip file of all of the files to be updated with paths related to the
Freelancer base directory. Include in the zip file a dsupdate.cfg to replace
the one that has been included with the mod. This file should contain an incremented
version number. Copy the zip to your update server(s).

Create an updated dsversion.cfg. This file should point to the zip file containing
the updated files and contain the new mod version numbers. Upload dsversion.cfg onto 
your update server(s).


DSVersion.cfg File Format
=============================

In section [version]

<uf1>
  The url to download the update zip file from. This parameter should be in 
  the form 'http://example.com/updates/updateXX.zip'. Both ftp and http protocols 
  are supported.

  Up to 9 fileservers may be specified in the dsversion.cfg file. The program will 
  select one server at random to download the update from. Additional file 
  servers are specified in the form uf2, uf3, etc.

<desc>
  The description of the update presented to the user.

<vernum>
  The version of the available update.

Example:

[version]
vernum = 4854
desc = 4.85 Beta 4
uf1 = http://igiss.net/devupload/dsupdate/update4854.zip

DSUpdate.cfg File Format
========================

In section [version]

<vernum>
  The version of the current mod.

In section [updatesettings]
  
<updateserver1>
  The url for the server to load the version.ini file from: "http://host/path/version.ini"
  or "ftp://host/path/version.ini".
  
  Up to 9 updateservers may be specified in the dsupdate.cfg file. The program will 
  select one server at random to download the update from. Additional update 
  servers are specified in the form updateserver2, updateserver3, updateserver4.

<requirevalidinstall>
  If an install error occurs then don't launch Freelancer. By default 1.
   
<verbose>
  If "0" then don't report non-critical errors. If "1" report errors to user. By default 0.
  
<flargs>
  Arguments for freelancer.exe; Any command line arguments to the update program 
  override this setting.

<flexe>
  The name of the freelancer executable.
  
<fldir>
  The relative or absolute path to the Freelancer base directory from the directory 
  dsupdate.exe is executed from.

Example: 

[version]
vernum = 4854
[updatesettings]
flmoddir = Discovery4854beta
flargs = -s82.113.48.5:2302
updateserver1 = http://igiss.net/dsupdate/dsversion.cfg
updateserver2 = 
updateserver3 = 
updateserver4 = 
updateserver5 = 
updateserver6 = 
updateserver7 = 
updateserver8 = 
updateserver9 = 
  
  
Other notes
===========

If a player is behind a http proxy server then they may need to enter your proxy
settings by adding the command line argument -proxy=server:port to the shortcut
when launching Freelancer.

The DSUpdate.dll also patches Freelancer to automatically include the servers
defined by the flargs parameter in the dsupdate.cfg file and it increases the
default chat history and chat input window sizes.

This package also includes DSInstallChecker.exe. This program generates a 
checksum for all files in a Freelancer installation. You can use this to 
validate an installation.

  
Source
======

The source for the project is located in the src folder. You may use it for whatever 
purpose you wish. If you use it in one of your projects, I would appreciate being notified
and/or mentioned somewhere.

If you make changes to this software and would like them incorporated into the
official release then contact Cannon at www.the-starport.net or www.discovergc.com.

Zlib:
This software statically links with zlib from the zlib 1.2.x sources, see src/zlib 
directory for more details.

libcurl:
This software statically links libcurl. See src/libcurl for more details.

Unzipper.h:
This software includes Unzipper.h an interface for the CUnzipper class (c) daniel godson 2002.
See unzipper.h for details.

This was built with Visual Studio C++ 2003.


Changes
=======
0.7:
- Simplified update creator.
- Added install checker utility.
- Fix bug to allow launching from FAM.
0.8:
- Merge dsupdate.cfg arguments and shortcut arguments.
- Will try multiple servers if one is failed.
- Removed installation validation and removed update-creator as it was only
  needed to produce a checksum file and this is no longer required
- Added version number support to prevent 'bad installation' dialog if no
  update is available at initial mod installation.
- Configurable dialog strings.
0.9:
- Added DSUpdate.dll to allow launching from Freelancer.
- Added chat patch to Freelancer (chat offsets by Motah, see www.the-star-port.net)
- Added manifest to DSUpdate.exe to require administrator permissions.
- Statically linked everything to reduce the number of required files and reduce
  dependancies (no more need for msvcrt7.dll)
- Fixed extraction bug when extracting a file that doesn't already exist. On
  some computers the installation would fail.
- Fixed server selection crash if the dsversion.cfg file is corrupted.
- Improved error messages to try to better describe the fault conditions.
1.0:
- Fixed version number check so that the updater is not launched if the current
  version number is greater than the server version.