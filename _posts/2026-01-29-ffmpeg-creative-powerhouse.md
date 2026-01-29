---
layout: post
title: "FFmpeg: More Than a Video Converter"
date: 2026-01-29
categories: ['til']
tags: ['ffmpeg', 'creative-coding', 'youtube']
description: "Discovered that FFmpeg has 450+ filters for generative art, audio visualization, and mathematical rendering."
author: Sunil Dhaka and Claude Opus 4.5
---

![ffmpeg generative art](/assets/images/ffmpeg-generative.svg)

I always assumed FFmpeg was just a video conversion utility. Today I learned it's actually a generative art studio hiding in plain sight.

### Audio Visualization

FFmpeg converts audio into visualizations directly:

- **Spectrograms** with 15+ color palettes (nebulae, fire, magma, viridis)
- **Constant-Q Transform** displays resembling piano rolls
- **Continuous Wavelet Transform** with mel/bark frequency scales
- **3D audio scopes** with camera controls
- **Polar vectorscopes** creating Lissajous patterns

```bash
ffmpeg -i music.mp3 -filter_complex "[0:a]showspectrum=color=nebulae:slide=scroll[v]" -map "[v]" output.mp4
```

### Generative Art From Math

FFmpeg generates video from mathematical equations alone:

**Mandelbrot fractals** with infinite zoom:
```bash
ffmpeg -f lavfi -i "mandelbrot=s=1920x1080:end_scale=0.0001" -t 60 fractal.mp4
```

**Conway's Game of Life** with mold trails:
```bash
ffmpeg -f lavfi -i "life=rule=B3/S23:mold=10:life_color=cyan" -t 30 life.mp4
```

**Cellular automata** (Rule 30, Rule 110):
```bash
ffmpeg -f lavfi -i "cellauto=rule=110" -t 20 automata.mp4
```

**Perlin noise** for organic textures:
```bash
ffmpeg -f lavfi -i "perlin=octaves=5:tscale=0.01" -t 30 clouds.mp4
```

### Pixel-Level Shaders

The `geq` filter computes each pixel's color via expressions. Plasma effects, hypnotic rings, tunnel animations:

```bash
ffmpeg -f lavfi -i "color=black:s=1920x1080:d=10" \
  -vf "geq=r='128+127*sin(hypot(X-960,Y-540)/30+N*0.1)':g='128+127*sin(hypot(X-960,Y-540)/30+N*0.08)':b='128+127*sin(hypot(X-960,Y-540)/30+N*0.06)'" \
  rings.mp4
```

### Video to Audio

`spectrumsynth` converts images back into sound. Draw a picture, hear what it sounds like.

### Videos Created

Using only FFmpeg filters, I made six meditation videos:

1. [Cosmic Fractals](https://www.youtube.com/watch?v=DeCT8ht0UQw) — Mandelbrot + Game of Life
2. [Ethereal Clouds](https://www.youtube.com/watch?v=yszP8qn-8CI) — Perlin noise with color palettes
3. [Digital Patterns](https://www.youtube.com/watch?v=R19gydkJNxg) — Cellular automata (Rule 30/90/110)
4. [Plasma Dreams](https://www.youtube.com/watch?v=0Vhk8BbG_Cw) — Mathematical shaders
5. [Chromatic Waves](https://www.youtube.com/watch?v=6jn9tMBggIU) — Animated gradients
6. [Sound Geometry](https://www.youtube.com/watch?v=LNjB6iWaSdc) — Music-reactive visualizations

No external software. Just FFmpeg commands.

### Reference

FFmpeg has over 450 filters. The documentation is worth browsing:

```bash
ffmpeg -filters | less
ffmpeg -h filter=showcqt
```

*Tools: ffmpeg 7.1.1 with libx264, showcqt, showspectrum, mandelbrot, life, cellauto, perlin, geq, gradients filters.*

PS: [Gist](https://gist.github.com/sunil-dhaka/4f1cd5e6b65faa92345f25e79976ec8f) — view HTML with `?GIST_ID` via [gistpreview](https://gistpreview.github.io/) or [gisthost](https://gisthost.github.io/); details in [Simon Willison's post](https://simonwillison.net/2026/Jan/1/gisthost/).
