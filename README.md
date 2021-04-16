# VCV Manual

Markdown source repo for https://manual.vcvrack.com/

## Contributing

This repo is like a wiki, but you must send a quality pull request to gain direct push access.
Fork this repo, make a quality improvement, and open a pull request.
If I accept it, I will add you as a repo maintainer so you can push to `master` directly.
manual.vcvrack.com is updated immediately after `master` is updated.

You can add images to the manual if they are necessary for your article.
Embed images with a relative link:
```md
![alt text](my-image.png)
```

You can use HTML in your Markdown if it is absolutely necessary, e.g. to embed a video.
If you abuse this feature, your maintainer privilege will be removed.

## Hosting your plugin's manual

Fork this repo and add a folder matching your slug.
Add a single link to your plugin's main page to [toc.md](toc.md).
Example folder structure:
- `/Core/index`: Hosted at https://manual.vcvrack.com/Core/
- `/Core/MIDI-CV`: Hosted at https://manual.vcvrack.com/Core/MIDI-CV
- `/Core/MIDI-CV.png`: Hosted at https://manual.vcvrack.com/Core/MIDI-CV.png

## License

All documentation text in this repository is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
Contributers agree to license their text contributions under this license.

Images such as screenshots and logos are licensed by their respective owner.
