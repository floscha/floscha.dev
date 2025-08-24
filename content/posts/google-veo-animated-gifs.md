---
title: "Using Google Veo to Create Animated GIFs"
date: 2025-08-24
---

## Introduction

I recently had the idea that the hero image from [my homepage](https://floscha.dev) and [my GitHub profile](https://github.com/floscha) would be more engaging if it were animated instead of static.
Since I'm not an expert in animation, I wondered if an image-to-video model like Google's [Veo](https://deepmind.google/models/veo/) could handle the animation task for me.

This post is a walkthrough for getting from my hero image below to an animated GIF of it.

![hero](hero.png)

## The Steps

### Correct the Aspect Ratio

Veo supports only two aspect ratios:

- 16:9
- 9:16

Therefore, use an image editing tool ([Photopea](https://www.photopea.com/) if you have nothing installed) and use the **Canvas Size** setting to change the ratio of your image to one of the above.

### Mask Transparent Sections

Unfortunately, Veo doesn't support transparency.
If you upload an image with a transparent background, Veo will simply turn the transparent sections black. As a workaround, similar to green screens, we can mask the transparent sections with a dedicated color like green (`#00FF00`) and later remove this color during post-processing.

### Use Veo on AI Studio to Generate the Video

In [Google AI Studio](https://aistudio.google.com/), go to **Generate Media > Animated Scene For Looping**.
There, upload your image and add a prompt similar to this:

```plaintext
Make the person on the image type.
Make their eyes slightly go from side to side as when following text on the screen.
Make the coffee cup steam.
Gently wave the leaves of the plant from side to side.

Make sure the video can be looped continuously.
```

Finally, press run to generate the video and once it's done, download the `.mp4` file from AI Studio.

### Convert MP4 to GIF

With [ffmpeg](https://github.com/FFmpeg/FFmpeg) installed, run the command below to convert the `.mp4` file to a `.gif` file.

```bash
ffmpeg -i input.mp4 -vf "fps=10,scale=480:-1:flags=lanczos" -c:v gif output.gif
```

If you prefer an online tool, you can also use [Ezgif](https://ezgif.com/) from the next section in this post.

### Make Background Transparent Again

After unfortunately not getting [ImageMagick](https://github.com/ImageMagick/ImageMagick) to work with `magick input.gif -transparent '#00ff00' output.gif`, I decided to go with the online tool [Ezgif](https://ezgif.com/).

There, you can go to **Effects**, upload an image, and go to the **Remove background** tab.
In the **Replace the background color with transparency** section, select **custom (HEX)** with the value `#00ff00` (or whatever color you chose as a transparency mask).

You'll notice that the GIF will still have many green parts around the edges.
This is because Veo also introduces some noise during the generation process.
To deal with this, play around with the **Fuzz %** value (`38` worked well in my case).
Once you're happy with the result, download the GIF from the **Output image** section.

## The Result

This is what the final result looks like:

![hero](https://github.com/user-attachments/assets/5978019b-01a1-4e34-a910-8ac7030fdea4)

Unfortunately, the GIF is still rough around the edges from the background removal, but other than that, I'm really pleased with the result.
