# FAQ

<a id="acronym"></a>
## What does "VCV" stand for?

There is no official meaning of the name "VCV", but some users have suggested "Virtual Control Voltage" or "Voltage Controlled Virtualization".
These are good guesses, but "VCV" was chosen simply because it is easy to remember and type.
[VCV Rack](https://vcvrack.com/Rack) is the full name of our flagship software product.


<a id="userfolder"></a>
## Where is the "Rack user folder"?

The Rack user folder stores data readable/writable by Rack.
You can open it by choosing `Help > Open user folder` in the Rack [menu bar](MenuBar.html), or by navigating to

- MacOS: `Documents/Rack/`
- Windows: `My Documents/Rack/`
- Linux: `~/.Rack/`

When running Rack in development mode, it is your current working directory instead.


<a id="plugin"></a>
## What is a VCV Rack plugin?

A plugin is a single software unit typically developed by one company or individual that can contain multiple VCV Rack modules.
Plugins are loaded from `<Rack user folder>/plugins-v*`.

When Rack v2 is released, you will be able to add individual modules to your personal module library, rather than entire plugins, so the concept of a "plugin" will soon be hidden from end users and exposed only to developers.
VCV also plans to offer individual commercial modules for sale, as well as discounted bundles of multiple modules.


<a id="daws"></a>
## Is VCV Rack available as a VST/AU/AAX plugin for DAWs?

VCV Rack can be fully considered a DAW itself rather than a "synthesizer plugin", so Rack is a standalone application.
However, due to overwhelming user demand, a new product called *VCV Rack for DAWs* will be available as a 64-bit VST2 plugin for around $99 shortly after Rack v2 is released.
VST3/AU/AAX/LV2 versions might be released afterwards, but this is not yet confirmed.
All Rack v2 plugins will be compatible with the plugin version of Rack.
The standalone version of Rack v2 will continue to be free/open-source.

Follow the [Rack development blog](https://community.vcvrack.com/t/rack-development-blog/5864) for the most up-to-date Rack development news.


<a id="performance"></a>
## How do I improve performance of VCV Rack?

VCV Rack simulates a modular synthesizer where each module itself can be a challenge to simulate on a modern computer, whether it's a virtual analog model with hundreds of analog components to simulate, or a digital module designed to be run on an ARM microprocessor similar to your smart phone's.
A common patch of a hundred modules can require billions of floating point calculations per second to simulate and millions of 2D path elements to draw using OpenGL.
Therefore, sometimes the following undesirable symptoms can occur when using Rack.

- audio **hiccups**: Sparse and random millisecond-long audio delays. Caused by sporadically missing the audio block deadline, due to thread switching due to high CPU of other threads/processes/applications, disk activity, uncommon high-CPU branches in the DSP code, etc. during audio block processing. Depending on the audio driver and device, either zeros or the last generated sample value is inserted until the audio block is ready.
- audio **stuttering**: Consistent audio delays every audio buffer block. Caused by the minimum CPU time required to process an audio block being greater than the real-time duration of the audio block.
- high **power usage**: Causes laptops to drain their battery faster than during idle or typical use. More power is used by CPUs and GPUs when they are idling less often (on the microsecond scale) and operating at higher clock frequencies (managed by the OS or BIOS based on average resource load).
- high CPU/GPU/case **temperature**: Causes the fan regulator to increase fan speeds. Heat is directly proportional to power usage.

If any of these symptoms occur, you can attempt to treat them using the following tips.
Note that some tips have trade-offs or might not provide any benefit for your situation.

- Rack DSP:
	- Use Rack's [CPU meter](MenuBar.html#cpu-meter) to identify high-CPU modules that you could remove or replace.
	- Turn off the CPU meter when you don't need it.
	Measuring the CPU time of each module in your patch consumes significant CPU.
	- Increase [VCV Audio](Core.html#audio)'s block size to the highest tolerable number.
	This results in proportionally higher audio latency but proportionally decreases block processing overhead and allows higher jitter in sample processing CPU times.
	- Use the lowest tolerable Engine [sample rate](MenuBar.html#sample-rate).
	Engine CPU usage is almost exactly proportional to its sample rate.
	- Disable modules in your patch that you aren't currently using by right-clicking on their panels and choosing Disable.

- Rack multi-threading:
	- To maximize the number of modules in your patch without audio stuttering, increase the number of Rack engine threads until no stuttering occurs.
	See [Threads](MenuBar.html#threads).
	- To reduce power usage and temperature, use the smallest number of Rack engine threads that does not cause audio stuttering.

- Graphics:
	- Decrease Rack's frame rate to the smallest tolerable value. See [Frame rate](MenuBar.html#frame-rate).
	- Use a dedicated (discrete) graphics card, such as Nvidia or AMD.
	Rack is not designed for integrated graphics such as Intel HD/Iris. See [System Requirements](Installing.html#system-requirements).
	- Make sure your graphics drivers are up-to-date.
	- If using an Apple Retina display on your Mac, set the Rack app to use [Low Resolution Mode](https://www.maketecheasier.com/launch-apps-low-resolution-mac/).
	- Use Rack in [fullscreen](MenuBar.html#fullscreen) mode so the graphics card does not need to render the OS user interface and other applications.

- Audio hardware:
	- Use a dedicated audio interface rather than your motherboard's audio hardware.
	This slightly decreases the CPU overhead of sending/receiving audio buffers.
	- Avoid using the DirectSound driver on Windows.
	Instead use ASIO or WASAPI.
	If you do not have an audio interface with an ASIO or WASAPI, install the freeware software [ASIO4ALL](https://www.asio4all.org/).
	This installs an ASIO driver that communicates directly with [WDM](https://en.wikipedia.org/wiki/Windows_Driver_Model) audio interface drivers, bypassing [MME](https://en.wikipedia.org/wiki/Windows_legacy_audio_components#Multimedia_Extensions_(MME)) and [DirectSound](https://en.wikipedia.org/wiki/DirectSound).

- Operating system configuration:
	- Avoid running unnecessary programs while Rack is running.
	- Configure your CPU to run in maximum performance mode (not energy-saving mode).
	On Windows, see [How to enable the High performance power plan by Ableton](https://help.ableton.com/hc/en-us/articles/115000211304-How-to-enable-the-High-performance-power-plan-Windows-).
	Mac automatically adjusts your CPU performance when applications require high CPU.
	- If you are able, plug in your laptop.
	Operating systems reduce CPU performance and power consumption while running on battery.

- Audio/video recording:
	- Use [VCV Recorder](https://library.vcvrack.com/VCV-Recorder/Recorder) to record audio or video.
	Unlike non-Rack-specific screen recording software, it operates in Rack engine "time", not real-time, and therefore does not record real-time audio hiccups/stuttering.
	This means that any hiccups/stuttering you hear in real-time will not be present when the recording is played back.

- Computer hardware:
	- Although Rack's [System Requirements](Installing.html#system-requirements) suggest that computers as old as 2013 can run Rack, it is recommended to use a computer from 2016 or later that is designed for gaming.
	There are many gaming laptop and desktop computers on the market for as low as $300, the price of an average hardware Eurorack module.
	Unfortunately, Apple's MacBook Air and older MacBook Pro models are not designed for gaming (despite their high price!) and are therefore not recommended for VCV Rack.


<a id="touch"></a>
## Does VCV Rack work with touch screens?

Rack's window library GLFW does not support [touch input](https://github.com/glfw/glfw/issues/42) yet, so Rack relies on the operating system to control the mouse cursor using the touch screen.
This means that multi-touch gestures do not work.
However, you can disable "View > Lock cursor while dragging" in the menu bar to prevent Rack from grabbing the mouse cursor when interacting with knobs.


<a id="res"></a>
## Rack fails to launch on Mac with the error "Rack’s resource directory "n_…/res" does not exist"

Follow the instructions in [this comment](https://github.com/VCVRack/Rack/issues/1623#issuecomment-551374084).


<a id="bridge"></a>
## What was VCV Bridge?

[VCV Bridge](https://github.com/VCVRack/Bridge) was an experimental project for transferring audio/MIDI between VCV Rack and another DAW via a VST2/AU plugin.
It relied on [inter-process communication (IPC)](https://en.wikipedia.org/wiki/Inter-process_communication) between Rack (server) and the DAW plugin (client), similar to [ReWire](https://en.wikipedia.org/wiki/ReWire_(software_protocol)).
Because real-time IPC of audio cannot be achieved on non-[real-time operating systems](https://en.wikipedia.org/wiki/Real-time_operating_system), it was never intended as more than a fun experiment, and the project was concluded a month after development started.
One could say the experiment "failed", but its purpose was primarily to see how much it would fail.
The conclusion was that it was not reliable enough for the majority of users.
VCV Bridge was deprecated in July 2018 and is now unsupported.
The Bridge VST2/AU plugin was removed in Rack 1.0 (although it can be found in [earlier Rack packages](https://vcvrack.com/downloads/)), and the Bridge audio/MIDI driver will be removed in Rack 2.0.


<a id="mobile"></a>
## Will Rack be ported to iOS or Android?

It is not planned. There are many issues with such a project.

- Technical:
	- Tablet and phone users don't normally use mice, so a touch driver would need to be written. If GLFW is still used, [touch support](https://github.com/glfw/glfw/issues/42) would need to be added to the library.
	- There is no user-managed filesystem on iOS, and forcing users to mess with the filesystem is bad UX on Android, so plugin folders and patch files would need to be managed entirely by Rack itself.
	- RtAudio and RtMidi don't have iOS Core Audio/MIDI or Android HAL/OpenSL ES backends, so they would need to be added and tested.
	- Apple does not allow apps distributed through the store to download and execute code, so either all plugins would need to be included in the distributable, or it could only be distributed on jailbroken iOS devices, which is an absurd user requirement.

- Business:
	- Such a port would be expensive to develop, so it would need to be sold commercially. Some plugins (proprietary, GPL, etc) would need special licensing agreements in order to be included in the package. Some plugins would increase the cost of the product if included in the package. Others would simply be omitted from the third-party plugin collection.
	- The friction for a developer to build and test their plugins on iOS/Android is significantly higher than the three desktop OS's, which may decrease their willingness to develop Rack plugins.
	- When serving an app on the App Store or Google Play, Apple and Google are not obligated to continue serving an app and may remove it at will or change policies on a whim that can disrupt VCV's business model. This would place a large risk upon VCV.
