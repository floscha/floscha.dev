---
title: "Using uv Scripts with Raycast"
date: 2025-08-17
---

## Introduction

Over the years, I have accumulated various scripts for smaller automations.
One pretty trivial example is a script which takes the currently opened browser tab and stores the markdown link to this page (something like `[Page Title](example.com)`) in the clipboard.

Most of these are written in Python but I neither want to spend time managing virtual environments nor global dependencies.
Fortunately, [uv scripts](https://docs.astral.sh/uv/guides/scripts/#using-a-shebang-to-create-an-executable-file) are an excellent tool for the job since they automatically create an virtual environment for the script but cache dependencies to not having to install all depencencies for every additional script.

The remaining question is how to launch those scripts.
An obvious answer would be to use a terminal but opening an extra terminal window was too much friction for me (even when using Quake-like terminals such as Ghostty's [Quick Terminal](https://hachyderm.io/@mitchellh/113222119716853963)).
The perfect solution for me are Raycast's [Script Commands](https://www.raycast.com/blog/getting-started-with-script-commands).
Since I'm already using Raycast for many of its features, this didn't require me to install additional software but instead allows me to run commands conveniently from a familiar interface.

This post will provide a full walkthrough on how to use uv and Raycast to launch arbitrary Python scripts using a command bar interface.

## Prerequisites

To use custom scripts with Raycast, all you have to do is tell it where those scripts are located.
To do so, open **Raycast Settings**, **Add New**, **Add Script Directory**.
Then, the directory on your machine where you want to store the files.

## Creating a new script

For new scripts, I always use the following template which covers all required boilerplate code and let's you get straight to coding the actual functionality.

```python
#!/usr/bin/env -S uv run
#
# @raycast.schemaVersion 1
# @raycast.title Copy Markdown Link
# @raycast.mode silent
# 
# /// script
# dependencies = [
#   "some-dependency==1.0.0"
# ]
# ///

# TODO: Put your code here!
```

The first line makes it a [uv script](https://docs.astral.sh/uv/guides/scripts/#using-a-shebang-to-create-an-executable-file) and lets you execute it with `./my_script.py` instead of requiring to add `python` or `uv run` (make sure to adjust the permissions with `chmod u+x my_script`).

Next, the comments with `@raycast` add the required metadata for the script to be run as a Raycast script:

- `@raycast.schemaVersion` adds the schema version and for the time being should always be `1`.
- `@raycast.title` sets the title to be displayed in Raycast.
- `@raycast.mode` defines how the output of the script is displayed in Raycast. For details see [Output Modes](https://github.com/raycast/script-commands/blob/master/documentation/OUTPUTMODES.md).

Then, the `/// script` block is used by uv to define dependencies and the required Python version can also be set here.
While exact versions for dependencies are optional, they are recommended to make the script more reproducible independent of what systems it is run on.

Bonus tip: Create a [Raycast Snippet](https://manual.raycast.com/snippets) for the template to quickly create future scripts.

## Using the script

When saving the script to the earlier defined script directory, Raycast will automatically find it by searching for the text you've used for `@raycast.title`.
You can then also find it in **Settings** under **Extensions** where you can assign a hotkey to the script.

## Full Example

The following code shows the full implementation of the *markdown link* script I've mentioned in the introduction.

```python
#!/usr/bin/env -S uv run
#
# @raycast.schemaVersion 1
# @raycast.title Copy Markdown Link
# @raycast.mode silent
# 
# /// script
# dependencies = [
#   "sh",
# ]
# ///

from sh import osascript

if __name__ == "__main__":
    page_title = osascript(e='tell application "Google Chrome" to get title of active tab of first window').strip()
    page_url = osascript(e='tell application "Google Chrome" to get URL of active tab of first window').strip()
    markdown_link = f"[{page_title}]({page_url})"
    osascript(e=f'set the clipboard to "{markdown_link}"')

    print(f"📋 Successfully copied {markdown_link} to clipboard.")
```
