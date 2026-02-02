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

I always thought FFmpeg was just for converting videos. Turns out it's a hidden generative art studio with 450+ filters.

### What You Can Create

**Audio visualizations** — spectrograms, piano roll displays, 3D scopes, Lissajous patterns. Feed it music, get mesmerizing visuals.

**Math-generated videos** — Mandelbrot zooms, Conway's Game of Life, cellular automata, Perlin noise clouds. No source footage needed.

**Pixel shaders** — plasma effects, hypnotic rings, tunnel animations. Each pixel computed from equations.

**Image-to-sound** — draw something, hear what it sounds like.

### Prompts for Your AI Agent

Just tell your favourite coding agent:

- *"Create a scrolling spectrogram video from this audio file with nebula colors"*
- *"Generate a 60-second Mandelbrot fractal zoom video"*
- *"Make a Conway's Game of Life animation with cyan cells and mold trails"*
- *"Create animated Perlin noise clouds in 1080p"*
- *"Generate hypnotic expanding rings using FFmpeg's geq filter"*

### What I Made

Six meditation videos, pure FFmpeg:

1. [Cosmic Fractals](https://www.youtube.com/watch?v=DeCT8ht0UQw) — Mandelbrot + Game of Life
2. [Ethereal Clouds](https://www.youtube.com/watch?v=yszP8qn-8CI) — Perlin noise
3. [Digital Patterns](https://www.youtube.com/watch?v=R19gydkJNxg) — Cellular automata
4. [Plasma Dreams](https://www.youtube.com/watch?v=0Vhk8BbG_Cw) — Mathematical shaders
5. [Chromatic Waves](https://www.youtube.com/watch?v=6jn9tMBggIU) — Animated gradients
6. [Sound Geometry](https://www.youtube.com/watch?v=LNjB6iWaSdc) — Music-reactive visuals

PS: [Gist with full commands](https://gist.github.com/sunil-dhaka/4f1cd5e6b65faa92345f25e79976ec8f) if you want to peek under the hood.
