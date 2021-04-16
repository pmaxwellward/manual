# Installing & Running

<a id="sysreq"></a>
## System Requirements

VCV Rack is free software, so you may simply download and run the software to see if it works.
However, if Rack does not run or you are experiencing performance issues, make sure you have at least the following hardware.
(Also see [How do I improve performance of VCV Rack?](FAQ#how-do-i-improve-performance-of-vcv-rack))
- Operating system: MacOS 10.7+, Windows 7+, or Linux (such as Ubuntu 16.04+)
- CPU: Intel/AMD 64-bit processor from \~2011 or later
- Graphics: Dedicated graphics card from \~2013 or later with the latest driver software update:
	- [Nvidia drivers](https://www.nvidia.com/Download/index.aspx)
	- [AMD drivers](https://www.amd.com/en/support)
	- [Intel drivers](https://downloadcenter.intel.com/product/80939/Graphics-Drivers). Integrated (non-dedicated) graphics such as Intel HD/Iris are not recommended and cause significantly increased CPU usage.
- RAM: 1GB
- Disk space: 1GB

<a id="installing"></a>
## Installing Rack

Download Rack on the [VCV Rack website](https://vcvrack.com/).

### Installing on Mac

Download, unzip, and copy the Rack app to your Applications folder.

### Installing on Windows

Run the installer.

### Installing on Linux

Unzip the zip file.

## Installing plugins

Plugins extend VCV Rack's functionality by adding one or more modules to use in your patch.
Plugins are typically installed via the [VCV Library](https://library.vcvrack.com/).
See the *VCV Library Instructions* section at the bottom of the VCV Library page.

If your computer is offline, you may download plugins using another computer and transfer `<Rack user folder>/plugins-v*` (See [Where is the “Rack user folder”?](FAQ#where-is-the-rack-user-folder)) to the offline computer.
Downloading plugins directly from the VCV Library is not supported at this time.

## Installing plugins not available on the VCV Library

*Install third-party plugins at your own risk. Like VST plugins, installing plugins from unknown sources may compromise your computer and personal information.*

Plugins for Rack are distributed as ZIP files with the format `<plugin slug>-<version>-<arch>.zip`, e.g. `MyPlugin-1.0.0-mac.zip`.
Download the plugin ZIP package from the vendor's website to `<Rack user folder>/plugins-v*` (See [Where is the "Rack user folder"?](FAQ#where-is-the-rack-user-folder)).
Do not extract the ZIP package yourself.
Rack will extract and load the plugin upon launch.

Note: Plugins must be built (compiled) before Rack can load them.
GitHub hosts plugin source code, not plugin binaries, so GitHub's green "Clone or download" button will *not* give you a plugin binary.
However, some plugin maintainer make plugin builds available in the "Releases" section of their GitHub repository.

Note: The "major" version number (e.g. the `1` in `v1.2.3`) must match the major version number of Rack. See [ABI/API Version](Version).


<a id="running"></a>
## Running Rack

### Running on Mac

Launch Rack from the Applications folder or the dock.

### Running on Windows

Click on Rack in the Start Menu.

### Running on Linux

Double click the `Rack` binary.
Or with the command-line, `cd` into the `Rack` folder and run `./Rack`.

### Command line usage

To launch Rack from the command line, `cd` into Rack's folder, and run `./Rack`, optionally with the following options.
- `<patch filename>`: Loads a patch file.
- `-s <Rack system folder>`: Sets Rack's system folder, containing read-only program resources. Defaults to
	- MacOS: `<app bundle path>/Contents/Resources`
	- Windows: install location, such as `C:\Program Files\VCV\Rack`
	- Linux: current working directory
- `-u <Rack user folder>`: Sets Rack's user folder, containing user settings, plugins, and patches. See [Where is the “Rack user folder”?](FAQ#where-is-the-rack-user-folder) for the default location.
- `-d`: Enables development mode.
This sets the system and user folders to the current working directory, uses the terminal (stderr) for logging, and disables Rack's Library menu to prevent overwriting plugins.
