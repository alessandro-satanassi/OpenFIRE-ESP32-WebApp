# OpenFIRE ESP32 — Web App

The OpenFIRE Web App as it is published, nothing else: this repository is the site that
GitHub Pages serves at

    https://alessandro-satanassi.github.io/OpenFIRE-ESP32-WebApp/

## These files are generated: do not edit them here

The source of the app lives in the firmware repository, in `lightgun/webapp`:

    https://github.com/alessandro-satanassi/OpenFIRE-Firmware-ESP32

Everything here is written by `lightgun/scripts/webapp_build.py`. A change made here is
lost at the next publication: change the source, rebuild, publish what the build wrote.

## One folder per version

Every firmware has its own app, built at the same moment, and every version stays
published. An app never talks to a lightgun of another version without saying so, so the
version the firmware runs is the one that decides which app is opened.

```
/                    the home page: the Connect button, and nothing else
/launcher.js         what makes it work
/versions.json       the versions published so far
/v/6.2/              the app of firmware 6.2, exactly as it was published
/v/6.1/              the app of firmware 6.1, untouched since its day
/.nojekyll           GitHub Pages serves the files as they are, without Jekyll
```

**The home page is not the app.** It has one button: it opens the port, asks the lightgun
which firmware it runs — the version is the first thing the board says, so nothing else is
read — closes the port, looks that version up in `versions.json` and opens `v/<version>/`.
The port is handed over to the app that opens, which takes it back by itself: the lightgun
is connected twice, but you click once.

When the firmware's version is not published nothing is opened by itself: the versions that
are there are offered, so a firmware nobody made an app for can still be tried with a
neighbouring one. It should never happen, since an app is published with every firmware.

**A published app never goes looking for another one.** It is the app of its version and it
stays there. If the lightgun runs a firmware of another version it says so and asks whether
to carry on or to go back to the home page, which opens the right one.

`versions.json` is the whole list:

```json
{ "latest": "6.2",
  "versions": [ { "id": "6.2", "label": "6.2.0", "type": "stable" },
                { "id": "6.1", "label": "6.1.0", "type": "stable" } ] }
```

The `id` is the number the firmware itself sends when the app connects (`OPENFIRE_VERSION`
in `src/OpenFIREversion.h`, so `6.2`): it is the only version an already installed lightgun
can tell the app about, and it names the folder. `label` and `type` are only what the pages
show.

## Publishing a version

From the `lightgun` folder of the firmware repository, on the tag of that version:

```
python scripts/webapp_build.py site        ->  dist/site      (the app of this firmware)
python scripts/webapp_build.py launcher    ->  dist/launcher  (the home page)
```

then, in this repository:

1. copy `dist/site` into `v/<version>/` — a new folder, the existing ones are never touched;
2. copy `dist/launcher` over the root (`index.html`, `launcher.js`, `.nojekyll`);
3. add the version to `versions.json` and set `latest` to it;
4. commit and push.

Only the folder of the version being published is written. Raising the version number in
`OpenFIREversion.h` therefore leaves every previous folder exactly as it was.

## Settings of this repository

GitHub Pages: **Deploy from a branch**, branch `main`, folder `/ (root)`.

## Licence

GNU General Public License, like the rest of the project: see `LICENSE`.
