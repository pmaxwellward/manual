# Plugin Development Tutorial

## Prerequisites

- Familiarity with C++, although creating Rack plugins is a great way to learn programming and C++. Rack plugins are written in [C++11](https://en.cppreference.com/w/cpp/11).
- Familiarity with navigating the command line (`cd`, `ls`, etc).
- Familiarity with modular synthesizers. [Digital signal processing (DSP)](DSP) knowledge is only required if creating sound generators and processors.
- Download and install [VCV Rack](https://vcvrack.com/Rack.html).
- Follow the steps to [set up your build environment](Building#setting-up-your-development-environment) for your operating system.
You do not need to build Rack from source if using the Rack SDK.
- [Download the latest Rack SDK](https://vcvrack.com/downloads/) and extract.
This contains the Rack API headers and build system for compiling your plugin.

In your terminal, run
```bash
export RACK_DIR=<Rack SDK folder>
```
to specify the absolute path of the extracted Rack SDK.
You may wish to add this line to your `~/.bashrc` or other shell environment, so you don't have to define it every time you open a terminal.

## Creating the template plugin

The `helper.py` script included in the Rack SDK is an easy way to create a plugin template.
You can run it with no arguments to show documentation.

Choose a [slug](Manifest#slug) for your plugin, a unique string containing letters, numbers, `-`, or `_`.
We will use `MyPlugin` for this tutorial.
Run
```bash
$RACK_DIR/helper.py createplugin MyPlugin
```
to create a folder called `MyPlugin/` in your current directory.
Example session:
```text
Plugin name [MyPlugin]: My Plugin
Version [1.0.0]:
License (if open-source, use license identifier from https://spdx.org/licenses/) [proprietary]: CC0-1.0
Brand (prefix for all module names) [My Plugin]:
Author []: VCV
Author email (optional) []: support@vcvrack.com
Author website URL (optional) []: https://vcvrack.com/
Plugin website URL (optional) []:
Manual website URL (optional) []:
Source code URL (optional) []:
Donate URL (optional) []:
Manifest written to MyPlugin/plugin.json
Created template plugin in MyPlugin/
Initialized empty Git repository in /home/VCV/MyPlugin/.git/
```
You can change this manifest later by editing `plugin.json`. (See [Manifest](Manifest)).

To test your build system, you may run `make` in the plugin directory.
If it succeeds, an "empty" plugin will be built containing no modules.
However, this is an good opportunity to check that your build environment is set up correctly.

## Creating panels

For each module you wish to create, follow the [Panel Guide](Panel) to design an SVG panel graphic.

For this tutorial, we will create a module with the slug `MyModule` and panel file [MyModule.svg](images/MyModule.svg).
Save this file to `res/` and run
```bash
<Rack SDK folder>/helper.py createmodule MyModule res/MyModule.svg src/MyModule.cpp
```
This will create a C++ file automatically from components in the SVG file.
Example session:
```text
Module name [MyModule]: My Module
One-line description (optional) []: Simple sine oscillator
Tags (comma-separated, case-insensitive, see https://github.com/VCVRack/Rack/blob/v1/src/tag.cpp for list) []: VCO
Added MyModule to plugin.json
Panel found at res/MyModule.svg. Generating source file.
Found 1 params, 1 inputs, 1 outputs, 0 lights, and 0 custom widgets.
Components extracted from res/MyModule.svgSource file generated at src/MyModule.cpp

To enable the module, add
extern Model *modelMyModule;
to plugin.hpp, and add
p->addModel(modelMyModule);
to the init() function in plugin.cpp.
```
Open `MyModule.svg` with Inkscape, open the Layers panel, and hide the `components` layer to hide component placeholders.

## Implementing the DSP kernel

Rack modules have four basic components, as we saw in the [Panel Guide](Panel).
- **Param**: Read with `params[...].getValue()`
- **Input**: Read with `inputs[...].getVoltage()`
- **Output**: Write with `outputs[...].setVoltage(voltage)`
- **Light**: Write with `lights[...].setBrightness(brightness)`

In this tutorial, we will implement a simple sine oscillator with a **PITCH** param, 1V/oct **PITCH** input, **SINE** output, and a **BLINK** light that flashes at 1 Hz.

Open the generated `src/MyModule.cpp` source file and add the following member variables to the `Module` class.
```cpp
	float phase = 0.f;
	float blinkPhase = 0.f;
```
These variables store the internal state of the module.
Then add the following code to the `process()` function, which is called every audio frame (e.g. 44,100 times per second if the sample rate is 44,100 Hz).
```cpp
	void process(const ProcessArgs &args) override {
		// Compute the frequency from the pitch parameter and input
		float pitch = params[PITCH_PARAM].getValue();
		pitch += inputs[PITCH_INPUT].getVoltage();
		pitch = clamp(pitch, -4.f, 4.f);
		// The default pitch is C4 = 261.6256f
		float freq = dsp::FREQ_C4 * std::pow(2.f, pitch);

		// Accumulate the phase
		phase += freq * args.sampleTime;
		if (phase >= 0.5f)
			phase -= 1.f;

		// Compute the sine output
		float sine = std::sin(2.f * M_PI * phase);
		// Audio signals are typically +/-5V
		// https://vcvrack.com/manual/VoltageStandards
		outputs[SINE_OUTPUT].setVoltage(5.f * sine);

		// Blink light at 1Hz
		blinkPhase += args.sampleTime;
		if (blinkPhase >= 1.f)
			blinkPhase -= 1.f;
		lights[BLINK_LIGHT].setBrightness(blinkPhase < 0.5f ? 1.f : 0.f);
	}
```
Compile your plugin with `make`.
If the build succeeds, you can generate a distributable plugin package with `make dist`, which places it in `dist/`.

You can automatically install the plugin package to your [Rack user folder](manual/FAQ#where-is-the-rack-user-folder) with `make install`.
You should now be able to test your plugin by opening Rack and choosing your module in the Module Browser.

If you don't see your plugin in Rack's Module Browser, check the `log.txt` file in your Rack user folder.
This file contains warnings about plugins that fail to load and error messages if your plugin crashes.

## Beyond the tutorial

The Rack API is very flexible for creating custom [DSP](DSP) algorithms and custom interactive widgets handling many types of events from the keyboard, mouse, etc.
See the [Rack API headers](https://github.com/VCVRack/Rack/tree/v1/include) or the [Rack API documentation](https://vcvrack.com/docs/namespaces.html) for the full reference, or review the source code of the many open-source plugins if you prefer learning by example.

The [Voltage Standards](VoltageStandards) article defines the behavior for handling signals in a consistent way.

You can find a wealth of information on the [Developer category](https://community.vcvrack.com/c/development) of the [VCV Community](https://community.vcvrack.com/) forum by searching or creating a new thread.

## Releasing

Eventually you may want to release your hard work.

See [Plugin Licensing](PluginLicensing) for information about following Rack's license, particularly if developing a commercial plugin.
It is recommended to add a `LICENSE.txt` file to your plugin's root folder that specifies your preferred license (whether open-source or proprietary).

Review your `plugin.json` [manifest](Manifest) file for correctness, spelling, and capitalization.
Finally, submit your plugin to the [VCV Library](https://github.com/VCVRack/library#adding-your-plugin-to-the-vcv-library-for-open-source-plugins) to allow users to easily download your plugin from their VCV account.
