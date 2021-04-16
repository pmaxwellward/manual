# Migrating v1 Plugins to v2

The API of VCV Rack v2 has been designed to be nearly backward-compatible with v1 plugins, so updating your plugin to v2 likely only involves a version update and a recompile.

## Update plugin version to v2

The major version number (`vMAJOR.MINOR.REVISION`) of your plugin must match the major version of Rack, so change the version number in your plugin's `plugin.json` manifest to `2`.
For example, change
```json
{
  "slug": "Fundamental",
  "version": "1.2.3",
  ...
```
to
```json
{
  "slug": "Fundamental",
  "version": "2.0.0",
  ...
```
If you wish, you may keep the minor and revision numbers unchanged, e.g. `2.2.3`.

Download the latest [Rack v2 SDK](https://vcvrack.com/downloads/).
Clean and compile the plugin.
```bash
export RACK_SDK=/path/to/Rack-SDK
make clean
make dist
```
If your plugin compiles successfully, you are ready to test and release.
Or, you may take advantage of new v2 features below.


## Potential bugs

### Avoid keeping `Font` (and `Image`) references across multiple frames

*Rack for DAWs* uses a different OpenGL context each time the plugin editor window is closed and reopened.
This means that if you load a `Font` in a widget's constructor with `font = APP->window->loadFont(...)`, the OpenGL font reference will be invalid if the window is reopened later.

Instead, save only the font path, and fetch the font each frame in `draw()`. Example:
```cpp
std::shared_ptr<Font> font = APP->window->loadFont(fontPath);
if (font) {
	nvgFontFaceId(args.vg, font->handle);
	...
}
```
`loadFont()` caches the font by its path, so this operation can be efficiently called in `draw()` every frame.

*All of the above also applies to `Image` and `loadImage()`.*

## Optional v2 API features

### SVG load function
`Window::loadSvg()` has been moved to `Svg::load()`. Search and replace:
```bash
perl -i -pe 's/APP->window->loadSvg\b/Svg::load/g' src/*
```

### Port labels

TOOD

### Light labels

TODO

### TODO

