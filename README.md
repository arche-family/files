# Files

The Arche phone's files app, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, which the phone has from its first start. That world is Nimadir's mini-app world and what a phone adds to it, here the phone's shared files; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it. Nimadir's own shell, which has none of it, does not run it.

It works like Android's Files app:

- The phone's shared files, a folder at a time: Pictures, Music, Documents and Download at the top. A folder's folders come first, then its files, each with its size.
- A tap on a folder opens it, and ‹ goes up.
- A tap on a file shows its name, its size and, when it is text, the text, with Back and Delete.
- A new folder is its name typed and Make. An empty folder, not one of the top's, can be deleted from inside it.

It reaches the shared files through the phone world's `files` interface (`list`, `read`, `write`, `make-folder`, `remove`), which the phone gives an app whose manifest needs `files`. It is an app of the page: its taps come as click events, and its typing as input events.

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . files.component.wasm
```

`manifest` is what the phone's repository packages with the component. It gives the app's name, version and title, and what it may do: read and change the shared files (`files`).

## Test

`media/` is a folder of shared files as the phone keeps them: two text files in Documents, one in Download, a small PNG in Pictures and a short WAV in Music. `headless` holds the taps and the typing of a headless run, with `media/` shared as the phone shares its files (arche-wasm's `--share`): Documents opened and a text file shown; up; a folder made, opened and deleted; Pictures opened, its picture shown (no preview) and deleted. `expected.txt` holds what arche-wasm's interpreter said, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, each in a fresh copy of this directory, and compares what it says with `expected.txt`. CI runs it on every push.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```

## License

MIT or Apache-2.0, at your option ([LICENSE](LICENSE)).
