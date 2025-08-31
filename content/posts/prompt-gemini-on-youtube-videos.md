---
title: "Prompt Gemini on YouTube Videos"
date: 2025-08-31
---

Today's post is a handy Raycast script that can be used to run prompts on the content of the given YouTube video.

This can be achived by using Gemini's [URL Context](https://ai.google.dev/gemini-api/docs/url-context), which allows simply passing the video URL into the full prompt.

```python
#!/usr/bin/env -S uv run
#
# @raycast.schemaVersion 1
# @raycast.title Prompt Gemini on YouTube Video
# @raycast.mode fullOutput
# @raycast.argument1 { "type": "text", "placeholder": "Video ID" }
# @raycast.argument2 { "type": "text", "placeholder": "Prompt" }
#
# /// script
# dependencies = [
#   "google-genai>=1.30.0",
#   "python-dotenv",
# ]
# ///

import os
import sys

from dotenv import load_dotenv
from google import genai
from google.genai.types import GenerateContentConfig


load_dotenv()


def prompt_video(video_id: str, prompt: str, model_id: str = "gemini-2.5-flash"):
    client = genai.Client()

    response = client.models.generate_content(
        model=model_id,
        contents=f"Given the YouTube video {video_id}, {prompt}",
        config=GenerateContentConfig(tools=[{"url_context": {}}])
    )
    for each in response.candidates[0].content.parts:
        print(each.text)

if __name__ == "__main__":
    video_id = sys.argv[1]
    prompt = sys.argv[2]
    prompt_video(video_id, prompt)
```
