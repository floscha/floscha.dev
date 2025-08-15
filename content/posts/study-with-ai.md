---
title: "How I Study With AI"
date: 2025-08-15
ShowToc: false
---

## Introduction

This post describes how I use several AI tools to help me study topics of any kind.

For this, I usually go through the following 3 steps:

1. Assemble the learning material with GitHub Copilot
2. Create cards with AI Studio
3. Create audio summary with [NotebookLM](https://notebooklm.google.com/) and AI Studio

## Assemble Learning Material

First, I collect all the study material I already have.
For topics where I don't have any material or just need a lot more, I use an agentic tool like GitHub Copilot, which I am most comfortable with as a software engineer.
Also, running the agent locally has the benefit that you can easily split your material across multiple files instead of bundling it in a large response.
In the future, however, I should also try this approach with other agentic tools like ChatGPT Deep Research.

I then pass the agent a prompt similar to this:

```plaintext
Based on the topics and subtopics represented as topics below, for each bullet, create a comprehensive section, explaining all related concepts in detail. Take your information from <some-related-website> and all sub pages. Create a markdown file for each topic. Place the markdown files in a <name-of-the-subject> folder.
```

This leaves you with an organized number of markdown files which can already be used for studying by themselves.

## Create Cards

Next, we use the markdown files to create flashcards for the [Mochi](https://mochi.cards/) app, which lets you easily import cards for studying using [spaced repetition](https://en.wikipedia.org/wiki/Spaced_repetition).

This can be achieved with the same local agent as above using this prompt:

```plaintext
For each topic markdown file, create flashcards in csv format, where the first column is the question and the second column the answer. Do not use headers.
```

These CSV files can then be imported in Mochi using the **Import** menu and the **CSV** option.
Make sure to set **Quote** to `"`.

## Create Audio Summary

Create a new notebook for each topic in [NotebookLM](https://notebooklm.google.com/) and either upload the markdown file or copy its content via **Paste text > Copied text**.
Then, click on **Audio Overview** to create a podcast based on the imported study material.

While the interaction of the two virtual podcast hosts can be entertaining, I often prefer a more concise single-speaker audio summary, which we can obtain using the following steps (inspired by [Tina Huang](https://www.youtube.com/watch?v=qbt-MFVvQQY)):

- Download the podcast audio file in the playback section of NotebookLM.
- Import the audio file in a tool like [AI Studio](https://aistudio.google.com/).
- Transcribe it using a prompt like `Accurately transcribe the uploaded podcast with two speakers. Do not use timestamps.`
- `Convert this transcript into a single host format and remove all extraneous information and commentary. Stay concise but do not remove any facts. Write it such that it can be read by a single speaker.`
- Copy the output.
- Go to **Generate Media > Generate speech** and paste the output from the previous step.
- Wait until the generation has finished and either listen in AI Studio or download the audio to listen offline.
