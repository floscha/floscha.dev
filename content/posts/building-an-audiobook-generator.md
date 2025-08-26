---
title: "Building an Audiobook Generator With Google Gemini"
date: 2025-08-19
---

## Introduction

The other day, I was wondering whether I could use AI to generate custom audiobooks based on existing documents like PDFs.
While professionally recorded audiobooks already exist for most books, I wanted something more customizable where you could, for example, use only chapter summaries instead of the full chapters.
I also wanted the audiobooks to be in the `m4b` format, which comes as a single file but still supports chapters for easier navigation (unlike `wav` or `mp3`).

To achieve this, I had to solve the following parts:

- Extract relevant content from the source text.
- Split the content into chapters.
- Create audio for each chapter.
- Merge all chapters into a single audiobook.

The final project can be found on GitHub: [floscha/gemini-audiobook-generator](https://github.com/floscha/gemini-audiobook-generator)

The goal of this post isn't to walk you through the full implementation but rather to discuss some things I found easier and some I found more challenging while developing this project.

## What Just Worked

### Gemini API

To use Google's Gemini AI models from Python code, you need a Gemini API key.
Creating this key through [Google AI Studio](https://aistudio.google.com/) was really straightforward and required only one or two clicks.

Similarly, writing the Python code to call the models was effortless because of the excellent [developer documentation](https://ai.google.dev/gemini-api/docs/), which has Python snippets ready for many practical use cases.
These include processing text from PDF files and converting text to audio—exactly what I needed.
Because of this, I was able to build the core AI logic within about 5 minutes.

## Challenges

### Prompting

Writing the prompt to preprocess the source document into customized chapters turned out to be slightly more challenging than I initially thought.

For example, I use a simple heuristic where I take the first line of a chapter as the chapter name.
Unfortunately, this doesn't work for some formatting like the one below.

```plaintext
Chapter 1
Name of the Chapter
```

To make the heuristic work, I added the following prompt:

```plaintext
Keep the heading in a single line.
```

This usually does the trick, but due to the probabilistic nature of LLMs, issues can still occur—for example, dropping the `Chapter 1` part of the example above.

### Reusing Intermediate Files

While running the whole script from scratch might work for other tools, it's much more practical to store intermediate files from which the script can recover in case of failure or to speed up experimentation during development.

Even though the Gemini free tier is really generous, a medium-sized PDF file can quickly result in 100k tokens per request, leading to higher than desirable token consumption.
Creating the audio files, on the other hand, wasn't as token-expensive but took several minutes, leading to slower development speed.

The simple solution to this problem was to provide a `keep_intermediate_files` option that can be used during development.
When turned on, intermediate files are kept, and when re-running the script, it recovers from those files rather than starting from scratch.

### Merging Audio Files

After the AI-specific code was implemented rather quickly, writing the code to merge the audio files for the individual chapters into a combined `m4b` file turned out to be much more tedious.
This is mainly due to the fact that no Python library with this functionality exists.

Instead, I had to fall back to using [ffmpeg](https://github.com/FFmpeg/FFmpeg) through `subprocess` calls.
Even worse, this approach required generating lots of hacky metadata files through Python, which took me several tries to get right.

Finally, I see the lack of a Python `m4b` converter as an opportunity to create one myself, which I might do in the near future.
