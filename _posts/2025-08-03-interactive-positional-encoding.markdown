---
layout: full-width
title: "Interactive Positional Encoding"
date: 2025-08-03
categories: ['machine-learning']
tags: ['transformers', 'positional-encoding', 'interactive-visualization']
description: "An interactive visualization of positional encoding in Transformers."
author: Sunil Dhaka
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Positional Encoding</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://d3js.org/d3.v7.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            overscroll-behavior: none;
        }
        .latex {
            font-family: "Times New Roman", Times, serif;
            font-style: italic;
        }
        .axis path,
        .axis line {
            stroke: #9ca3af; /* gray-400 */
        }
        .axis text {
            fill: #6b7280; /* gray-500 */
            font-size: 12px;
        }
        .grid-line {
            stroke: #e5e7eb; /* gray-200 */
            stroke-dasharray: 2,2;
        }
        .slider-thumb::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            background: #3b82f6; /* blue-500 */
            cursor: pointer;
            border-radius: 50%;
            margin-top: -6px;
        }
        .slider-thumb::-moz-range-thumb {
            width: 20px;
            height: 20px;
            background: #3b82f6; /* blue-500 */
            cursor: pointer;
            border-radius: 50%;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 antialiased">

    <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-3xl sm:text-4xl font-bold tracking-tight text-gray-900">Visualizing Positional Encoding</h1>
            <p class="mt-3 max-w-2xl mx-auto text-lg text-gray-600">An interactive guide to understanding how Transformers encode position.</p>
        </div>

        <!-- Equations -->
        <div class="bg-white rounded-xl shadow-sm p-6 mb-8 ring-1 ring-gray-200">
             <h2 class="text-xl font-semibold text-gray-800 mb-4">The Core Equations</h2>
             <div class="flex flex-col md:flex-row justify-center items-center space-y-4 md:space-y-0 md:space-x-8 text-lg text-gray-700">
                <div class="bg-blue-50 text-blue-800 p-4 rounded-lg text-center">
                    <span class="latex">PE(pos, 2i) = sin(pos / 10000<sup>2i/d</sup>)</span>
                </div>
                <div class="bg-red-50 text-red-800 p-4 rounded-lg text-center">
                    <span class="latex">PE(pos, 2i+1) = cos(pos / 10000<sup>2i/d</sup>)</span>
                </div>
            </div>
            <p class="mt-4 text-center text-gray-600">These equations generate a unique vector for each position (<span class="latex">pos</span>) in a sequence, using sine and cosine waves of varying frequencies determined by the dimension index (<span class="latex">i</span>).</p>
        </div>

        <!-- Main Container -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            
            <!-- Controls and Main Plot -->
            <div class="lg:col-span-2 bg-white rounded-xl shadow-sm p-6 ring-1 ring-gray-200">
                <h2 class="text-xl font-semibold text-gray-800 mb-1">Positional Encoding Vector</h2>
                <p class="text-gray-600 mb-6">This chart shows the final encoding vector for a given position. Drag the sliders to see how it changes.</p>
                
                <!-- Controls -->
                <div class="space-y-4">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 pb-4 border-b">
                        <div>
                            <label for="max-pos-input" class="block text-sm font-medium text-gray-700">Max Position</label>
                            <input id="max-pos-input" type="number" value="1024" min="16" max="2048" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                        </div>
                        <div>
                            <label for="max-d-input" class="block text-sm font-medium text-gray-700">Max Dimension</label>
                            <input id="max-d-input" type="number" value="512" min="8" max="1024" step="8" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                        </div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 pt-4">
                        <div>
                            <label for="pos-slider" class="block text-sm font-medium text-gray-700">Position (<span class="latex">pos</span>): <span id="pos-value" class="font-bold text-blue-600">0</span></label>
                            <input id="pos-slider" type="range" min="0" max="1023" value="0" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer slider-thumb">
                        </div>
                        <div>
                            <label for="d-slider" class="block text-sm font-medium text-gray-700">Dimension (<span class="latex">d</span>): <span id="d-value" class="font-bold text-blue-600">32</span></label>
                            <input id="d-slider" type="range" min="8" max="512" value="32" step="8" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer slider-thumb">
                        </div>
                    </div>
                </div>

                <!-- Main PE Vector Plot -->
                <div id="pe-vector-chart" class="w-full h-80 mt-6"></div>
            </div>

            <!-- Wave Plots Container -->
            <div class="bg-white rounded-xl shadow-sm p-6 ring-1 ring-gray-200">
                <h2 class="text-xl font-semibold text-gray-800 mb-1">Underlying Sine & Cosine Waves</h2>
                <p class="text-gray-600 mb-4">Each pair of dimensions in the vector comes from a sine/cosine wave pair. The vertical line shows the current position.</p>
                <p class="text-xs text-amber-600 bg-amber-50 p-2 rounded-md mb-4">Note: Rendering may be slow with high dimension values.</p>
                <div id="wave-plots" class="space-y-4 max-h-[600px] overflow-y-auto pr-2">
                    <!-- Wave plots will be generated here -->
                </div>
            </div>
        </div>
        
        <!-- Heatmap Container -->
        <div class="bg-white rounded-xl shadow-sm p-6 mt-8 ring-1 ring-gray-200">
            <h2 class="text-xl font-semibold text-gray-800 mb-1">Full Positional Encoding Matrix</h2>
            <p class="text-gray-600 mb-4">This heatmap shows the entire PE matrix. The highlighted row corresponds to the selected position.</p>
            <div class="flex items-center">
                <!-- Y-axis Label -->
                <div class="flex justify-center items-center" style="writing-mode: vertical-rl; transform: rotate(180deg);">
                    <p class="text-sm text-gray-600 font-medium p-2 whitespace-nowrap">Position (<span class="latex">pos</span>)</p>
                </div>
                <!-- Heatmap and X-axis Label -->
                <div class="flex-grow flex flex-col items-center">
                    <div id="heatmap-wrapper" class="w-full" style="height: 400px;">
                        <canvas id="heatmap-canvas" class="w-full h-full"></canvas>
                    </div>
                    <!-- X-axis Label -->
                    <div class="mt-2">
                         <p class="text-sm text-gray-600 font-medium">Dimension Index (<span class="latex">d</span>)</p>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- UTILITY FUNCTIONS ---
        const getPEPair = (pos, i, d_model) => {
            const angle = pos / Math.pow(10000, (2 * i) / d_model);
            return [Math.sin(angle), Math.cos(angle)];
        };

        const getPEValue = (pos, dim, d_model) => {
            const i = Math.floor(dim / 2);
            const angle = pos / Math.pow(10000, (2 * i) / d_model);
            if (dim % 2 === 0) {
                return Math.sin(angle);
            } else {
                return Math.cos(angle);
            }
        };

        // --- DOM ELEMENTS ---
        const posSlider = document.getElementById('pos-slider');
        const dSlider = document.getElementById('d-slider');
        const maxPosInput = document.getElementById('max-pos-input');
        const maxDimInput = document.getElementById('max-d-input');
        const posValueLabel = document.getElementById('pos-value');
        const dValueLabel = document.getElementById('d-value');
        const peVectorContainer = document.getElementById('pe-vector-chart');
        const wavePlotsContainer = document.getElementById('wave-plots');
        const heatmapCanvas = document.getElementById('heatmap-canvas');
        const heatmapCtx = heatmapCanvas.getContext('2d');

        // --- STATE ---
        let state = {
            pos: 0,
            d_model: 32,
            max_pos: 1023
        };

        // --- D3 AND VISUALIZATION SETUP ---
        const margin = { top: 20, right: 20, bottom: 40, left: 40 };
        let mainChartWidth, mainChartHeight;
        let mainSvg, mainX, mainY, mainXAxis, mainYAxis;
        const colorScale = d3.scaleSequential(d3.interpolateViridis).domain([-1, 1]);

        function setupMainChart() {
            peVectorContainer.innerHTML = '';
            mainChartWidth = peVectorContainer.clientWidth - margin.left - margin.right;
            mainChartHeight = peVectorContainer.clientHeight - margin.top - margin.bottom;

            mainSvg = d3.select("#pe-vector-chart").append("svg")
                .attr("width", mainChartWidth + margin.left + margin.right)
                .attr("height", mainChartHeight + margin.top + margin.bottom)
                .append("g")
                .attr("transform", `translate(${margin.left},${margin.top})`);

            mainX = d3.scaleBand().range([0, mainChartWidth]).padding(0.1);
            mainY = d3.scaleLinear().range([mainChartHeight, 0]).domain([-1, 1]);

            mainSvg.append("g").attr("class", "grid-line").call(d3.axisLeft(mainY).tickSize(-mainChartWidth).tickFormat(""));
            mainSvg.append("line").attr("x1", 0).attr("x2", mainChartWidth).attr("y1", mainY(0)).attr("y2", mainY(0)).attr("stroke", "#9ca3af").attr("stroke-width", 1.5);
            mainXAxis = mainSvg.append("g").attr("class", "axis x-axis").attr("transform", `translate(0,${mainChartHeight})`);
            mainYAxis = mainSvg.append("g").attr("class", "axis y-axis").call(d3.axisLeft(mainY));
            mainSvg.append("text").attr("text-anchor", "middle").attr("x", mainChartWidth / 2).attr("y", mainChartHeight + margin.bottom - 5).text("Dimension Index");
            mainSvg.append("text").attr("text-anchor", "middle").attr("transform", "rotate(-90)").attr("y", -margin.left + 10).attr("x", -mainChartHeight / 2).text("Value");
        }

        function updateMainChart() {
            const { pos, d_model } = state;
            const data = [];
            for (let i = 0; i < d_model / 2; i++) {
                const [sinVal, cosVal] = getPEPair(pos, i, d_model);
                data.push({ dim: 2 * i, value: sinVal, type: 'sin' });
                data.push({ dim: 2 * i + 1, value: cosVal, type: 'cos' });
            }

            mainX.domain(data.map(d => d.dim));
            mainXAxis.call(d3.axisBottom(mainX).tickValues(d3.range(0, d_model, Math.max(2, Math.floor(d_model/8)))));
            const bars = mainSvg.selectAll("rect").data(data, d => d.dim);
            bars.enter().append("rect").attr("fill", d => d.type === 'sin' ? '#3b82f6' : '#ef4444')
                .merge(bars).transition().duration(50)
                .attr("x", d => mainX(d.dim)).attr("width", mainX.bandwidth())
                .attr("y", d => d.value > 0 ? mainY(d.value) : mainY(0))
                .attr("height", d => Math.abs(mainY(d.value) - mainY(0)));
            bars.exit().remove();
        }

        function setupWavePlots() {
            wavePlotsContainer.innerHTML = '';
            const { d_model } = state;
            for (let i = 0; i < d_model / 2; i++) {
                const wrapper = document.createElement('div');
                const title = document.createElement('p');
                title.className = "text-xs text-gray-500 mb-1";
                title.innerHTML = `Dimensions <span class="font-semibold text-blue-600">${2*i} (sin)</span> & <span class="font-semibold text-red-600">${2*i+1} (cos)</span>`;
                const waveDiv = document.createElement('div');
                waveDiv.id = `wave-plot-${i}`;
                waveDiv.className = 'w-full h-24';
                wrapper.appendChild(title);
                wrapper.appendChild(waveDiv);
                wavePlotsContainer.appendChild(wrapper);
                createSingleWavePlot(i);
            }
        }

        function createSingleWavePlot(i) {
            const container = document.getElementById(`wave-plot-${i}`);
            if (!container || container.clientWidth === 0) return;
            const width = container.clientWidth - 10, height = container.clientHeight - 10;
            const svg = d3.select(container).append("svg").attr("width", width + 10).attr("height", height + 10).append("g").attr("transform", `translate(5,5)`);
            const x = d3.scaleLinear().domain([0, state.max_pos]).range([0, width]);
            const y = d3.scaleLinear().domain([-1.2, 1.2]).range([height, 0]);
            const waveData = d3.range(0, state.max_pos + 1).map(p => { const [sinVal, cosVal] = getPEPair(p, i, state.d_model); return { pos: p, sin: sinVal, cos: cosVal }; });
            svg.append("path").datum(waveData).attr("fill", "none").attr("stroke", "#60a5fa").attr("stroke-width", 1.5).attr("d", d3.line().x(d => x(d.pos)).y(d => y(d.sin)));
            svg.append("path").datum(waveData).attr("fill", "none").attr("stroke", "#f87171").attr("stroke-width", 1.5).attr("d", d3.line().x(d => x(d.pos)).y(d => y(d.cos)));
            svg.append("line").attr("y1", y(-1.2)).attr("y2", y(1.2)).attr("stroke", "#9ca3af").attr("stroke-width", 1.5).attr("class", "position-line");
            svg.append("circle").attr("r", 4).attr("fill", "#3b82f6").attr("class", "sin-dot");
            svg.append("circle").attr("r", 4).attr("fill", "#ef4444").attr("class", "cos-dot");
        }
        
        function updateWavePlots() {
            const { pos, d_model } = state;
            for (let i = 0; i < d_model / 2; i++) {
                const svg = d3.select(`#wave-plot-${i} svg g`);
                if (svg.empty()) continue;
                const container = document.getElementById(`wave-plot-${i}`);
                const x = d3.scaleLinear().domain([0, state.max_pos]).range([0, container.clientWidth - 10]);
                const [sinVal, cosVal] = getPEPair(pos, i, d_model);
                const y = d3.scaleLinear().domain([-1.2, 1.2]).range([container.clientHeight - 10, 0]);
                svg.select(".position-line").transition().duration(50).attr("x1", x(pos)).attr("x2", x(pos));
                svg.select(".sin-dot").transition().duration(50).attr("cx", x(pos)).attr("cy", y(sinVal));
                svg.select(".cos-dot").transition().duration(50).attr("cx", x(pos)).attr("cy", y(cosVal));
            }
        }

        // --- HEATMAP FUNCTIONS ---
        let heatmapData = [];

        function prepareHeatmapData() {
            const { d_model, max_pos } = state;
            heatmapData = [];
            for (let p = 0; p <= max_pos; p++) {
                let row = [];
                for (let d = 0; d < d_model; d++) {
                    row.push(getPEValue(p, d, d_model));
                }
                heatmapData.push(row);
            }
        }

        function drawHeatmap() {
            const { d_model, max_pos } = state;
            
            const canvasWidth = heatmapCanvas.clientWidth;
            const canvasHeight = heatmapCanvas.clientHeight;
            if (canvasWidth === 0 || canvasHeight === 0) return;
            heatmapCanvas.width = canvasWidth;
            heatmapCanvas.height = canvasHeight;

            const cellWidth = canvasWidth / d_model;
            const cellHeight = canvasHeight / (max_pos + 1);

            heatmapCtx.clearRect(0, 0, canvasWidth, canvasHeight);

            for (let p = 0; p < heatmapData.length; p++) {
                for (let d = 0; d < heatmapData[p].length; d++) {
                    const value = heatmapData[p][d];
                    heatmapCtx.fillStyle = colorScale(value);
                    heatmapCtx.fillRect(d * cellWidth, p * cellHeight, cellWidth, cellHeight);
                }
            }
        }

        function highlightHeatmapRow() {
            const { pos, max_pos } = state;
            const canvasWidth = heatmapCanvas.width;
            const canvasHeight = heatmapCanvas.height;
            if (canvasWidth === 0 || canvasHeight === 0) return;

            drawHeatmap();

            const cellHeight = canvasHeight / (max_pos + 1);
            const y = pos * cellHeight;

            heatmapCtx.fillStyle = 'rgba(255, 255, 255, 0.5)';
            heatmapCtx.fillRect(0, y, canvasWidth, cellHeight);
            heatmapCtx.strokeStyle = '#000';
            heatmapCtx.lineWidth = 2;
            heatmapCtx.strokeRect(0, y, canvasWidth, cellHeight);
        }

        // --- UPDATE & EVENT HANDLERS ---
        function fullReset() {
            setupMainChart();
            setupWavePlots();
            prepareHeatmapData();
            updateMainChart();
            updateWavePlots();
            highlightHeatmapRow();
        }
        
        posSlider.addEventListener('input', (e) => {
            state.pos = +e.target.value;
            posValueLabel.textContent = state.pos;
            updateMainChart();
            updateWavePlots();
            highlightHeatmapRow();
        });

        dSlider.addEventListener('input', (e) => {
            state.d_model = +e.target.value;
            dValueLabel.textContent = state.d_model;
            fullReset();
        });

        maxPosInput.addEventListener('change', (e) => {
            const newMax = parseInt(e.target.value);
            if (isNaN(newMax) || newMax < 16) return;
            state.max_pos = newMax;
            posSlider.max = newMax;
            if (state.pos > newMax) {
                state.pos = newMax;
                posSlider.value = newMax;
                posValueeLabel.textContent = newMax;
            }
            fullReset();
        });

        maxDimInput.addEventListener('change', (e) => {
            let newMax = parseInt(e.target.value);
            if (isNaN(newMax) || newMax < 8) return;
            newMax = Math.round(newMax / 8) * 8;
            e.target.value = newMax;
            
            dSlider.max = newMax;
            if (state.d_model > newMax) {
                state.d_model = newMax;
                dSlider.value = newMax;
                dValueLabel.textContent = newMax;
            }
            fullReset();
        });
        
        window.addEventListener('resize', fullReset);

        function initializeVisuals() {
            posSlider.max = maxPosInput.value;
            dSlider.max = maxDimInput.value;
            
            posValueLabel.textContent = posSlider.value;
            dValueLabel.textContent = dSlider.value;
            state.pos = +posSlider.value;
            state.d_model = +dSlider.value;
            state.max_pos = +posSlider.max;

            fullReset();
        }

        window.addEventListener('load', initializeVisuals);

    </script>
</body>
</html>
