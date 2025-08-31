---
layout: full-width
title: "Rotary Position Embedding – An Interactive Guide"
date: 2025-08-11
categories: ['ai']
tags: ['rope', 'positional-encoding', 'transformers', 'interactive-visualization']
description: "An interactive guide to the math and intuition behind RoPE."
author: Sunil Dhaka
---

<html lang="en">
  
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rotary Position Embedding – An Interactive Guide</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- KaTeX for Math Rendering -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" crossorigin="anonymous">
    </script>

    <style>
      body { 
        font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, Helvetica Neue, Arial, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
        background-color: #f8fafc;
      }
      .katex { font-size: 1.1em; }
      .bg-grid {
        background-image:
          linear-gradient(to right, #e5e7eb 1px, transparent 1px),
          linear-gradient(to bottom, #e5e7eb 1px, transparent 1px);
        background-size: 20px 20px;
      }
    </style>
  </head>
  <body class="bg-gray-50 text-gray-800 antialiased">

    <div class="max-w-7xl mx-auto p-4 md:p-8">
      <header class="text-center mb-10">
        <h1 class="text-4xl font-bold text-gray-900 mb-2 flex items-center justify-center gap-3">
          <i data-lucide="rotate-3d" class="w-10 h-10 text-blue-600"></i>
          Rotary Position Embedding (RoPE)
        </h1>
        <p class="text-lg text-gray-600 max-w-3xl mx-auto">
          An interactive guide to the math and intuition behind RoPE.
        </p>
      </header>

      <!-- Tab Buttons -->
      <div class="flex flex-wrap gap-2 mb-6 border-b border-gray-200" id="tabs">
        <button data-tab="math" class="flex items-center gap-2 px-4 py-2 rounded-t-lg font-medium transition-colors bg-blue-500 text-white border-b-2 border-blue-500">
          <i data-lucide="sigma" class="w-4 h-4"></i>
          The Math
        </button>
        <button data-tab="viz" class="flex items-center gap-2 px-4 py-2 rounded-t-lg font-medium transition-colors bg-gray-100 text-gray-700 hover:bg-gray-200">
          <i data-lucide="eye" class="w-4 h-4"></i>
          Core Visualization
        </button>
        <button data-tab="alt" class="flex items-center gap-2 px-4 py-2 rounded-t-lg font-medium transition-colors bg-gray-100 text-gray-700 hover:bg-gray-200">
          <i data-lucide="git-compare-arrows" class="w-4 h-4"></i>
          Relative View
        </button>
        <button data-tab="ref" class="flex items-center gap-2 px-4 py-2 rounded-t-lg font-medium transition-colors bg-gray-100 text-gray-700 hover:bg-gray-200">
          <i data-lucide="book-marked" class="w-4 h-4"></i>
          References
        </button>
      </div>

      <!-- Tab Content -->
      <div>
        <!-- Tab 1: The Math -->
        <div id="tab-math" class="space-y-8">
          <!-- Top Section: 2D and General Form -->
          <div class="grid md:grid-cols-2 gap-8">
            <div class="bg-white p-6 rounded-lg border border-gray-200 shadow-sm">
              <h3 class="text-xl font-semibold mb-4 text-blue-700">1. The 2D Case: Rotation as Complex Multiplication</h3>
              <p class="mb-4">
                For a 2D vector $x_m$ (representing an input token's embedding at position $m$), RoPE applies a linear transformation $W_q$ (e.g., to create a query vector) and then rotates it based on its position $m$. This elegant rotation can be represented using complex numbers. If $W_q x_m$ is viewed as a complex number, its rotation by an angle $m\theta$ is simply:
              </p>
              <div class="bg-gray-50 p-4 rounded-lg text-center overflow-x-auto">
                $ f_q(x_m, m) = (W_q x_m) \cdot e^{im\theta} $
              </div>
              <p class="mt-4">
                Here, $e^{im\theta} = \cos(m\theta) + i\sin(m\theta)$ is Euler's formula. When expanded, this complex multiplication is equivalent to a standard 2D rotation matrix applied to the vector $(W_q x_m)$: 
              </p>
              <div class="bg-gray-50 p-4 mt-2 rounded-lg text-center overflow-x-auto">
                $ 
                  f_q(x_m, m) = 
                  \begin{pmatrix} \cos(m\theta) & - \sin(m\theta) \\ \sin(m\theta) & \cos(m\theta) 
                  \end{pmatrix}
                  (W_q x_m)
                $ 
              </div>
              <p class="mt-4 text-gray-700">
                This means each 2D vector is rotated by an angle directly proportional to its position $m$, scaled by a base frequency $\theta$.
              </p>
            </div>
            <div class="bg-white p-6 rounded-lg border border-gray-200 shadow-sm">
              <h3 class="text-xl font-semibold mb-4 text-blue-700">2. The General d-Dimensional Form: Block-Diagonal Rotations</h3>
              <p class="mb-4">
                RoPE extends this concept to a $d$-dimensional vector by pairing up features. For example, dimensions $(0, 1)$ form the first pair, $(2, 3)$ the second, and so on, up to $(d-2, d-1)$. Each of these $\frac{d}{2}$ pairs is rotated independently. Crucially, each pair uses a different rotation frequency, $\theta_i$, forming a block-diagonal rotation matrix $R^d_{\Theta,m}$:
              </p>
              <div class="bg-gray-50 p-4 rounded-lg text-center overflow-x-auto">
                $ f_q(x_m, m) = R^d_{\Theta,m} W_q x_m $
              </div>
              <p class="mt-4">
                The rotation frequencies $\theta_i$ for each pair $i \in [0, \frac{d}{2}-1]$ are defined as:
              </p>
              <div class="bg-gray-50 p-4 mt-2 rounded-lg text-center overflow-x-auto">
                $ \theta_i = \text{base}^{-2i/d} $
              </div>
              <p class="mt-4 text-gray-700">
                Here, `base` is a hyperparameter (commonly $10000$). This formula ensures a spectrum of frequencies: pairs with smaller $i$ (earlier dimensions) have higher frequencies (rotate faster), while pairs with larger $i$ (later dimensions) have lower frequencies (rotate slower). This allows the model to capture both fine-grained and coarse-grained positional information.
              </p>
            </div>
          </div>
          
          <!-- Bottom Section: Application to Attention -->
          <div class="bg-white p-6 rounded-lg border border-gray-200 shadow-sm">
            <h3 class="text-xl font-semibold mb-4 text-blue-700">3. Application to Self-Attention: Relative Position Encoding</h3>
            <p class="mb-4">
              The true power of RoPE shines in the self-attention mechanism. The attention score between a query vector $q_m$ (derived from $x_m$ at position $m$) and a key vector $k_n$ (derived from $x_n$ at position $n$) is their dot product. RoPE's design ensures that this dot product naturally incorporates relative positional information.
            </p>
            <div class="bg-gray-50 p-4 rounded-lg text-center overflow-x-auto">
              $ 
              \begin{aligned}
              q_m^T k_n &= (R^d_{\Theta,m} W_q x_m)^T (R^d_{\Theta,n} W_k x_n) \\
              &= (W_q x_m)^T (R^d_{\Theta,m})^T R^d_{\Theta,n} (W_k x_n) \\
              &= (W_q x_m)^T R^d_{\Theta, n-m} (W_k x_n)
              \end{aligned}
              $ 
            </div>
            <p class="mt-4 text-gray-700">
              The key step is $(R^d_{\Theta,m})^T R^d_{\Theta,n} = R^d_{\Theta, n-m}$. This identity, which holds for rotation matrices, means that the combined effect of rotating $q_m$ by $m$ and $k_n$ by $n$ is equivalent to rotating $k_n$ by the relative position $(n-m)$ with respect to $q_m$.
              This final equation shows that the attention score is no longer a function of the absolute positions $m$ and $n$, but rather a function of the input vectors $x_m, x_n$ and their **relative displacement**, $n-m$. This is how RoPE elegantly injects relative position information directly into the self-attention calculation, which is critical for sequence understanding in models like LLMs.
            </p>
          </div>
        </div>

        <!-- Tab 2: Core Visualization -->
        <div id="tab-viz" class="space-y-6 hidden">
          <div class="bg-white p-4 rounded-lg border border-gray-200 shadow-sm">
            <h3 class="text-lg font-semibold mb-4">Controls</h3>
            <div class="flex flex-wrap items-center gap-x-6 gap-y-4">
              <label class="flex items-center gap-2">
                <span class="text-sm font-medium text-gray-700">Dimension (d)</span>
                <select id="dimSelect" class="px-2 py-1 border rounded-md bg-white shadow-sm">
                </select>
              </label>
              <label class="flex-grow flex items-center gap-3" style="min-width: 300px;">
                <span class="text-sm font-medium text-gray-700">Position (m)</span>
                <!-- Changed max to 127 to match maxPos in frequency analysis -->
                <input id="posSlider" type="range" min="0" max="127" value="0" class="w-full" />
                <span id="posValue" class="text-sm font-mono bg-gray-100 px-2 py-1 rounded-md">0</span>
              </label>
              <button id="animateBtn" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md flex items-center gap-2 transition-colors">
                <i data-lucide="play" class="w-4 h-4"></i>
                <span id="animateBtnText">Animate</span>
              </button>
            </div>
          </div>

          <div class="grid lg:grid-cols-2 gap-6">
            <div class="bg-white p-4 rounded-lg border border-gray-200 shadow-sm">
              <h4 class="font-semibold mb-2 text-center text-blue-700">2D Rotation Visualization: How Rotations Happen</h4>
              <canvas id="rotationCanvas" width="500" height="400" class="w-full h-auto border rounded-md bg-grid"></canvas>
              <p class="text-xs text-gray-500 mt-2 text-center">
                Each colored vector represents a 2D pair within the larger embedding. Its angle is dynamically determined by its position $m$ and its unique frequency $\theta_i$. Observe how each pair rotates proportionally to the current position.
              </p>
            </div>
            <div class="bg-white p-4 rounded-lg border border-gray-200 shadow-sm">
              <h4 class="font-semibold mb-2 text-center text-blue-700">Frequency Analysis: Positional Encodings for a Given Position</h4>
              <canvas id="frequencyCanvas" width="500" height="400" class="w-full h-auto border rounded-md bg-grid"></canvas>
              <p class="text-xs text-gray-500 mt-2 text-center">
                This plot shows the sine wave associated with each pair's rotation frequency. The horizontal axis represents different positions, and the vertical axis shows the sine component of the rotation angle $m\theta_i$. The **red vertical line** marks the current position $m$, illustrating how the angular value for *all* dimensions (grouped into pairs) changes at that specific position. This reveals how positional encodings affect the entire embedding.
              </p>
            </div>
          </div>
        </div>

        <!-- Tab 3: Alternative View -->
        <div id="tab-alt" class="space-y-6 hidden">
          <div class="bg-white p-4 rounded-lg border border-gray-200 shadow-sm">
            <h3 class="text-lg font-semibold mb-4">Relative Position Explorer</h3>
            <div class="flex flex-wrap items-center gap-x-6 gap-y-4">
              <label class="flex items-center gap-2">
                <span class="text-sm font-medium text-gray-700">Dimension (d)</span>
                <select id="dimSelect2" class="px-2 py-1 border rounded-md bg-white shadow-sm">
                </select>
              </label>
              <label class="flex-grow flex items-center gap-3" style="min-width: 250px;">
                <span class="text-sm font-medium text-blue-600">Position (m)</span>
                <!-- Changed max to 127 -->
                <input id="posSliderM" type="range" min="0" max="127" value="10" class="w-full" />
                <span id="posValueM" class="text-sm font-mono bg-blue-50 text-blue-700 px-2 py-1 rounded-md">10</span>
              </label>
              <label class="flex-grow flex items-center gap-3" style="min-width: 250px;">
                <span class="text-sm font-medium text-red-600">Position (n)</span>
                <!-- Changed max to 127 -->
                <input id="posSliderN" type="range" min="0" max="127" value="26" class="w-full" />
                <span id="posValueN" class="text-sm font-mono bg-red-50 text-red-700 px-2 py-1 rounded-md">26</span>
              </label>
            </div>
            <p class="text-sm text-gray-600 mt-4">
              This view demonstrates the core property of RoPE: the dot product between two vectors, $q_m$ and $k_n$, depends only on their **relative distance** ($m-n$). 
              Each panel below shows a single 2D pair. The **blue vector** represents the rotation for position $m$, and the **red vector** for position $n$. Observe how the angle between them, $\Delta\theta = (m-n)\theta_i$, is the critical factor. This relative angle is precisely what the attention mechanism "sees," highlighting RoPE's effectiveness in capturing positional relationships.
            </p>
          </div>
          <div id="pairGrid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"></div>
        </div>

        <!-- Tab 4: References -->
        <div id="tab-ref" class="space-y-6 hidden">
          <div class="bg-white p-6 rounded-lg border border-gray-200 shadow-sm">
            <h3 class="text-xl font-semibold mb-4 text-blue-700">Visuals from the Paper</h3>
            <div class="grid md:grid-cols-2 gap-4 mb-6">
              <img src="/assets/images/rope_2d_case.png" alt="2D Case" class="rounded-lg border">
              <img src="/assets/images/rope_general_form.png" alt="General Form" class="rounded-lg border">
            </div>
            <div class="flex justify-center">
              <img src="/assets/images/RoPE_visualize.png" alt="RoPE Visualization" class="rounded-lg border max-w-full md:max-w-2xl">
            </div>
          </div>
          <div class="bg-white p-6 rounded-lg border border-gray-200 shadow-sm">
            <h3 class="text-xl font-semibold mb-4 text-blue-700">Further Reading & Resources</h3>
            <ul class="list-disc list-inside space-y-2">
              <li><a href="https://arxiv.org/pdf/2104.09864" target="_blank" class="text-blue-600 hover:underline">Rotary Position Embeddings (RoPE) - Official Paper</a></li>
              <li><a href="https://kexue.fm/archives/8130" target="_blank" class="text-blue-600 hover:underline">Transformer升级之路：2、博采众长的旋转式位置编码 (Chinese)</a></li>
              <li><a href="https://kexue.fm/archives/8265" target="_blank" class="text-blue-600 hover:underline">Transformer升级之路：4、RoPE是一种β进制编码 (Chinese)</a></li>
              <li><a href="https://jerryxiong.ng/posts/nd-rope/" target="_blank" class="text-blue-600 hover:underline">Understanding N-Dimensional Rotary Position Embedding</a></li>
              <li><a href="https://blog.eleuther.ai/rotary-embeddings/" target="_blank" class="text-blue-600 hover:underline">Rotary Embeddings: A Relative Revolution - EleutherAI Blog</a></li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <script>
      // --- Global State & Config ---
      const state = {
        dim: 16,
        pos: 0,
        posM: 10,
        posN: 26,
        base: 10000,
        animFrameId: null,
      };

      // --- DOM Elements ---
      const $ = (selector) => document.querySelector(selector);
      const queryAll = (selector) => document.querySelectorAll(selector);
      
      const elements = {
        tabs: queryAll('#tabs button'),
        tabContents: queryAll('[id^="tab-"]'),
        dimSelect: $('#dimSelect'),
        dimSelect2: $('#dimSelect2'),
        posSlider: $('#posSlider'),
        posValue: $('#posValue'),
        animateBtn: $('#animateBtn'),
        animateBtnText: $('#animateBtnText'),
        rotationCanvas: $('#rotationCanvas'),
        frequencyCanvas: $('#frequencyCanvas'),
        posSliderM: $('#posSliderM'),
        posValueM: $('#posValueM'),
        posSliderN: $('#posSliderN'),
        posValueN: $('#posValueN'),
        pairGrid: $('#pairGrid'),
      };

      // --- Tab Handling ---
      function setActiveTab(tabKey) {
        elements.tabs.forEach(btn => {
          const isActive = btn.dataset.tab === tabKey;
          btn.className = 'flex items-center gap-2 px-4 py-2 rounded-t-lg font-medium transition-colors ' +
            (isActive ? 'bg-blue-500 text-white border-b-2 border-blue-500' : 'bg-gray-100 text-gray-700 hover:bg-gray-200');
        });
        elements.tabContents.forEach(content => {
          content.classList.toggle('hidden', content.id !== `tab-${tabKey}`);
        });
        // Re-render KaTeX when switching tabs, as hidden elements might not render correctly initially
        // This call ensures math is rendered after the tab content becomes visible.
        if (window.renderMathInElement) {
          renderMathInElement(document.body, { delimiters: [{left: '$$', right: '$$', display: true},{left: '$', right: '$', display: false}] });
        }
      }

      // --- Math Helpers ---
      // Calculates the frequency (theta) for a given pair index and dimension.
      // The formula is base^(-2 * i / d), where i is the pair index.
      const getTheta = (pairIndex, dim, base) => Math.pow(base, -2 * pairIndex / dim);

      // --- Visualization Logic ---
      function drawRotationVisualization() {
        const { dim, pos, base } = state;
        const canvas = elements.rotationCanvas;
        const ctx = canvas.getContext('2d');
        const w = canvas.width, h = canvas.height;
        const cx = w / 2, cy = h / 2;
        const radius = Math.min(w, h) * 0.35;

        ctx.clearRect(0, 0, w, h);

        // Draw axes
        ctx.strokeStyle = '#d1d5db';
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(0, cy); ctx.lineTo(w, cy);
        ctx.moveTo(cx, 0); ctx.lineTo(cx, h);
        ctx.stroke();

        // Draw circle
        ctx.beginPath();
        ctx.arc(cx, cy, radius, 0, 2 * Math.PI);
        ctx.stroke();

        const pairs = dim / 2;
        for (let i = 0; i < pairs; i++) {
          // Calculate the unique frequency for this pair
          const theta = getTheta(i, dim, base);
          // Calculate the rotation angle based on current position 'pos'
          const angle = pos * theta;
          const x = cx + radius * Math.cos(angle);
          const y = cy + radius * Math.sin(angle);
          // Assign a unique color to each pair for clear distinction
          const color = `hsl(${(i * 360 / pairs)}, 70%, 50%)`;
          
          ctx.strokeStyle = color;
          ctx.lineWidth = 3;
          ctx.beginPath();
          ctx.moveTo(cx, cy);
          ctx.lineTo(x, y);
          ctx.stroke();
          
          ctx.fillStyle = color;
          ctx.beginPath();
          ctx.arc(x, y, 5, 0, 2 * Math.PI);
          ctx.fill();
        }
      }

      function drawFrequencyAnalysis() {
        const { dim, pos, base } = state;
        const canvas = elements.frequencyCanvas;
        const ctx = canvas.getContext('2d');
        const w = canvas.width, h = canvas.height;
        // Max position to display on the frequency graph
        const maxPos = 127; // Max position for frequency plot is 127 (0 to 127)

        ctx.clearRect(0, 0, w, h);

        const pairs = dim / 2;
        for (let i = 0; i < pairs; i++) {
          const theta = getTheta(i, dim, base);
          const color = `hsl(${(i * 360 / pairs)}, 70%, 50%)`;
          ctx.strokeStyle = color;
          ctx.lineWidth = 1.5;
          ctx.beginPath();
          // Draw the sine wave for each pair's frequency across positions
          for (let p = 0; p <= maxPos; p++) {
            const angle = p * theta; // Angle for position 'p'
            const x = (p / maxPos) * w;
            const y = h / 2 + Math.sin(angle) * (h / 2.5); // Sine wave
            if (p === 0) ctx.moveTo(x, y);
            else ctx.lineTo(x, y);
          }
          ctx.stroke();
        }

        // Draw the red line indicating the current position 'm'
        if (pos <= maxPos) {
          const x = (pos / maxPos) * w;
          ctx.strokeStyle = '#ef4444'; // Red color
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(x, 0);
          ctx.lineTo(x, h);
          ctx.stroke();
        }
      }

      function drawPairExplorer() {
          const { dim, posM, posN, base } = state;
          elements.pairGrid.innerHTML = ''; // Clear previous content
          const pairs = dim / 2;

          for (let i = 0; i < pairs; i++) {
              // Create a wrapper div for each pair's canvas and label
              const wrapper = document.createElement('div');
              wrapper.className = 'bg-white p-3 rounded-lg border border-gray-200 shadow-sm';
              const canvas = document.createElement('canvas');
              canvas.width = 250;
              canvas.height = 200;
              canvas.className = 'w-full h-auto rounded-md bg-grid';
              
              const label = document.createElement('p');
              label.className = 'text-sm font-medium text-center mt-2';
              label.textContent = `Pair ${i*2}-${i*2+1}`; // Display dimensions for the pair
              
              wrapper.appendChild(canvas);
              wrapper.appendChild(label);
              elements.pairGrid.appendChild(wrapper);

              const ctx = canvas.getContext('2d');
              const w = canvas.width, h = canvas.height;
              const cx = w / 2, cy = h / 2;
              const radius = Math.min(w, h) * 0.3;

              const theta = getTheta(i, dim, base);
              // Calculate angles for both positions m and n
              const angleM = posM * theta;
              const angleN = posN * theta;

              // Draw arc representing the relative angle between the two vectors
              ctx.beginPath();
              ctx.arc(cx, cy, radius * 0.6, Math.min(angleM, angleN), Math.max(angleM, angleN));
              ctx.strokeStyle = 'rgba(139, 92, 246, 0.5)'; // Purple-ish
              ctx.lineWidth = 15;
              ctx.stroke();

              // Draw Vector M (blue)
              ctx.beginPath();
              ctx.moveTo(cx, cy);
              ctx.lineTo(cx + radius * Math.cos(angleM), cy + radius * Math.sin(angleM));
              ctx.strokeStyle = '#2563eb'; // Blue
              ctx.lineWidth = 3;
              ctx.stroke();

              // Draw Vector N (red)
              ctx.beginPath();
              ctx.moveTo(cx, cy);
              ctx.lineTo(cx + radius * Math.cos(angleN), cy + radius * Math.sin(angleN));
              ctx.strokeStyle = '#dc2626'; // Red
              ctx.lineWidth = 3;
              ctx.stroke();
          }
      }

      // --- Animation ---
      function toggleAnimation() {
        if (state.animFrameId) {
          // Stop animation
          cancelAnimationFrame(state.animFrameId);
          state.animFrameId = null;
          elements.animateBtnText.textContent = 'Animate';
          elements.animateBtn.classList.remove('bg-red-500', 'hover:bg-red-600');
          elements.animateBtn.classList.add('bg-blue-500', 'hover:bg-blue-600');
        } else {
          // Start animation
          elements.animateBtnText.textContent = 'Stop';
          elements.animateBtn.classList.remove('bg-blue-500', 'hover:bg-blue-600');
          elements.animateBtn.classList.add('bg-red-500', 'hover:bg-red-600');
          const animate = () => {
            // Cycle position from 0 up to the new max of 127
            state.pos = (state.pos + 1) % (parseInt(elements.posSlider.max) + 1); 
            elements.posSlider.value = state.pos;
            elements.posValue.textContent = state.pos;
            drawRotationVisualization();
            drawFrequencyAnalysis();
            state.animFrameId = requestAnimationFrame(animate);
          };
          animate();
        }
      }

      // --- Initial Setup & Event Listeners ---
      function init() {
        // Init Lucide Icons
        if (window.lucide) {
          lucide.createIcons();
        }
        
        // Manually render KaTeX after the DOM is fully loaded and elements exist.
        // This replaces the onload attribute on the script tag.
        if (window.renderMathInElement) {
          renderMathInElement(document.body, { 
            delimiters: [
              {left: '$$', right: '$$', display: true},
              {left: '$', right: '$', display: false}
            ]
          });
        }

        // Populate dimension selects (even numbers from 2 to 16)
        for (let d = 2; d <= 16; d += 2) {
          const option = document.createElement('option');
          option.value = d;
          option.textContent = d;
          elements.dimSelect.appendChild(option.cloneNode(true));
          elements.dimSelect2.appendChild(option.cloneNode(true));
        }
        elements.dimSelect.value = state.dim;
        elements.dimSelect2.value = state.dim;

        // Ensure initial slider values respect max limits
        elements.posSlider.value = Math.min(state.pos, elements.posSlider.max);
        elements.posValue.textContent = elements.posSlider.value;
        elements.posSliderM.value = Math.min(state.posM, elements.posSliderM.max);
        elements.posValueM.textContent = elements.posSliderM.value;
        elements.posSliderN.value = Math.min(state.posN, elements.posSliderN.max);
        elements.posValueN.textContent = elements.posSliderN.value;


        // Setup event listeners
        elements.tabs.forEach(btn => {
          btn.addEventListener('click', () => setActiveTab(btn.dataset.tab));
        });

        elements.dimSelect.addEventListener('change', (e) => {
          state.dim = Number(e.target.value);
          elements.dimSelect2.value = state.dim; // Keep both selects in sync
          drawRotationVisualization();
          drawFrequencyAnalysis();
          drawPairExplorer(); // Redraw pair explorer when dimension changes
        });
        
        elements.dimSelect2.addEventListener('change', (e) => {
          state.dim = Number(e.target.value);
          elements.dimSelect.value = state.dim; // Keep both selects in sync
          drawPairExplorer();
          drawRotationVisualization(); // Redraw other visualizations as well
          drawFrequencyAnalysis();
        });

        elements.posSlider.addEventListener('input', (e) => {
          state.pos = Number(e.target.value);
          elements.posValue.textContent = state.pos;
          drawRotationVisualization();
          drawFrequencyAnalysis();
        });

        elements.animateBtn.addEventListener('click', toggleAnimation);

        elements.posSliderM.addEventListener('input', (e) => {
          state.posM = Number(e.target.value);
          elements.posValueM.textContent = state.posM;
          drawPairExplorer();
        });

        elements.posSliderN.addEventListener('input', (e) => {
          state.posN = Number(e.target.value);
          elements.posValueN.textContent = state.posN;
          drawPairExplorer();
        });

        // Initial render of all active visualizations
        setActiveTab('math'); // Set default tab
        drawRotationVisualization();
        drawFrequencyAnalysis();
        drawPairExplorer();
      }

      document.addEventListener('DOMContentLoaded', init);
    </script>
  </body>
</html>
