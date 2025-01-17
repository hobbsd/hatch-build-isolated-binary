# hatch-build-isolated-binary

[`hatch`](https://hatch.pypa.io/latest/) isn't currently able to create a distributable binary that can
bootstrap itself on first run without having a network connection, so I wrote this script
to automate building that sort of binary.

## Requirements

- `hatch` is being used for the project
- the script depends on the python version being set in `pyproject.toml` (from [hatch docs](https://hatch.pypa.io/1.13/plugins/builder/binary/#configuration)), e.g.:

    ```toml
    [tool.hatch.build.targets.binary]
    python-version = "3.13"
    ```

- a network connection is required during the build process

## Instructions

Put the `build_binary.py` script somewhere and run it like:

`hatch run python <path-to>/build_binary.py`

Or the package can be installed and then run via the `build-binary` command.

Here's a breakdown of what the script will do:

- use `hatch` to download a Python distribution into a directory named `<project-name>-<project-version>` within the directory returned by `tempfile.gettempdir()`
- build a wheel of the project
- install the project wheel into the Python dist
- make a bzip archive of the dist named `python.bz2` in the aforementioned temp dir
- use hatch to build a binary with the archived dist embedded in it

## Caveats

I did try to make it platform-agnostic, and it works for me on Windows, but I haven't needed it on Linux or Mac, so I haven't tried them yet.

## TODO

- It can be helpful to keep the temp files (the Python distribution) around for faster rebuilds, but maybe add an easy way to remove them.
