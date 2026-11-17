# Files

The Arche phone's files app, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, which the phone has from its first start. That world is Nimadir's mini-app world and what a phone adds to it, here the phone's shared files; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it. Nimadir's own shell, which has none of it, does not run it.

It works like a phone's file manager (HyperOS's), two pages with tabs along the bottom:

- Recent: the shared files, the newest first, under the day each was made (a camera's names say when), four across: each one's thumbnail or its kind's icon, its name, its size and its folder.
- Browse: the kinds (Docs, Images, Videos, Music), each opening its folder, and the folders of the phone's storage with how many items each has; + makes a folder, its name typed in a sheet.
- A folder shows where it is (Internal storage › Pictures), its folders first, then its files, a picture's thumbnail by its name; an empty folder, not one at the top, can be deleted.
- A file opens: a picture fitted to the screen, a text's text, a song with Play (the phone world's `audio`); each with its size and Delete.
- The phone's Back, and the arrow at the top, go back one.

The phone decodes the pictures (`files.image`) and keeps their thumbnails, the gallery's size, so that one kept serves both.

It reaches the shared files through the phone world's `files` interface (`list`, `read`, `write`, `make-folder`, `remove`), which the phone gives an app whose manifest needs `files`. It is an app of the page: its taps come as click events, and its typing as input events.

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . files.component.wasm
```

`manifest` is what the phone's repository packages with the component. It gives the app's name, version and title, and what it may do: read and change the shared files (`files`).

## Test

`media/` is a folder of shared files as the phone keeps them: two text files in Documents, one in Download, a small PNG in Pictures and a short WAV in Music. `headless` holds the taps and the typing of a headless run, with `media/` shared as the phone shares its files (arche-wasm's `--share`): Documents opened and a text file shown; up; a folder made, opened and deleted; Pictures opened, its picture shown (no preview) and deleted. `expected.txt` holds what arche-wasm's interpreter said, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, each in a fresh copy of this directory, and compares what it says with `expected.txt`. CI runs it when started by hand (Actions' Run workflow).

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```

## License

MIT or Apache-2.0, at your option ([LICENSE](LICENSE)).
