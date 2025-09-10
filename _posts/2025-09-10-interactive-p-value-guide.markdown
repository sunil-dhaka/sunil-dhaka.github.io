---
layout: full-width
title: "Interactive P-Value Guide"
date: 2025-09-10
categories: ['statistics']
tags: ['p-value', 'statistics', 'interactive-visualization']
description: "An interactive guide to understanding p-values."
author: Sunil Dhaka
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive P-Value Guide</title>
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js for visualizations -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Chart.js Annotation Plugin -->
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-annotation@3.0.1/dist/chartjs-plugin-annotation.min.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* Custom styles for a cleaner look */
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Custom styles for range inputs to match the theme */
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 16px;
            height: 16px;
            background: #3b82f6; /* blue-600 */
            border-radius: 50%;
            cursor: pointer;
            margin-top: -6px;
            border: 2px solid white;
            box-shadow: 0 0 0 2px #e2e8f0;
        }
        input[type="range"]::-moz-range-thumb {
            width: 16px;
            height: 16px;
            background: #3b82f6;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
            box-shadow: 0 0 0 2px #e2e8f0;
        }
    </style>
     <link rel="preconnect" href="https://rsms.me/">
     <link rel="stylesheet" href="https://rsms.me/inter/inter.css">
</head>
<body class="bg-slate-50 text-slate-700 antialiased">

    <div id="app-container" class="min-h-screen p-4 sm:p-6 lg:p-8">
        <div class="max-w-7xl mx-auto">
            <!-- Header -->
            <header class="text-center mb-10">
                <h1 class="text-3xl sm:text-4xl lg:text-5xl font-bold text-slate-900 mb-3">
                    Interactive P-Value Guide
                </h1>
                <p class="text-base sm:text-lg text-slate-600 max-w-3xl mx-auto">
                    Develop a deep, mathematical intuition for statistical inference through visualization and hands-on experimentation.
                </p>
            </header>

            <!-- Navigation -->
            <nav class="mb-8">
                <div id="nav-container" class="flex flex-wrap justify-center gap-2 sm:gap-3">
                    <!-- Nav buttons will be inserted here by JS -->
                </div>
            </nav>

            <!-- Progress Bar -->
            <div class="mb-8">
                <div class="w-full bg-slate-200 rounded-full h-1.5">
                    <div id="progress-bar" class="bg-blue-600 h-1.5 rounded-full transition-all duration-500" style="width: 0%;"></div>
                </div>
            </div>

            <!-- Main Content Area -->
            <main id="main-content" class="bg-white rounded-xl border border-slate-200 shadow-sm p-6 sm:p-8 lg:p-10 mb-8">
                <!-- Content will be rendered here by JS -->
            </main>

            <!-- Footer Navigation -->
            <footer class="flex justify-between items-center">
                <button id="prev-btn" class="px-5 py-2 rounded-md font-medium text-sm bg-white border border-slate-300 text-slate-700 hover:bg-slate-50 disabled:bg-slate-100 disabled:text-slate-400 disabled:cursor-not-allowed transition-colors">
                    Previous
                </button>
                <button id="next-btn" class="px-5 py-2 rounded-md font-medium text-sm bg-blue-600 text-white hover:bg-blue-700 disabled:bg-slate-300 disabled:cursor-not-allowed transition-colors">
                    Next
                </button>
            </footer>
        </div>
    </div>

    <script type="module">
        // --- State Variables ---
        let currentSection = 0;
        let sampleSize = 30;
        let trueMean = 0;
        let sampleMean = 1.5;
        let sampleStd = 2;
        let significance = 0.05;
        let testType = 'two-tailed';
        let simulations = [];
        let currentSample = [];

        // Chart instances need to be tracked to destroy them on re-render
        let chartInstances = {};

        // --- Mathematical & Data Utility Functions ---
        const normalPDF = (x, mu = 0, sigma = 1) => (1 / (sigma * Math.sqrt(2 * Math.PI))) * Math.exp(-0.5 * Math.pow((x - mu) / sigma, 2));
        const erf = (x) => {
            const a1 = 0.254829592, a2 = -0.284496736, a3 = 1.421413741, a4 = -1.453152027, a5 = 1.061405429, p = 0.3275911;
            const sign = x >= 0 ? 1 : -1;
            x = Math.abs(x);
            const t = 1.0 / (1.0 + p * x);
            const y = 1.0 - (((((a5 * t + a4) * t) + a3) * t + a2) * t + a1) * t * Math.exp(-x * x);
            return sign * y;
        };
        const normalCDF = (x, mu = 0, sigma = 1) => 0.5 * (1 + erf(((x - mu) / sigma) / Math.sqrt(2)));
        const generateNormalData = (n, mu, sigma) => {
            const data = [];
            for (let i = 0; i < n; i++) {
                const u1 = Math.random(), u2 = Math.random();
                const z = Math.sqrt(-2 * Math.log(u1)) * Math.cos(2 * Math.PI * u2);
                data.push(mu + sigma * z);
            }
            return data;
        };

        // --- Calculation Functions ---
        const calculateTestStats = () => {
            if (currentSample.length === 0) return null;
            const mean = currentSample.reduce((a, b) => a + b, 0) / currentSample.length;
            const variance = currentSample.reduce((a, b) => a + Math.pow(b - mean, 2), 0) / (currentSample.length - 1);
            const std = Math.sqrt(variance);
            const standardError = std / Math.sqrt(currentSample.length);
            const tStat = (mean - trueMean) / standardError;
            const df = currentSample.length - 1;
            let pValue;
            if (testType === 'two-tailed') pValue = 2 * (1 - normalCDF(Math.abs(tStat)));
            else if (testType === 'right-tailed') pValue = 1 - normalCDF(tStat);
            else pValue = normalCDF(tStat);
            return { mean, std, standardError, tStat, df, pValue };
        };

        const generateDistributionData = (testStats) => {
            const data = [];
            const range = testStats ? Math.max(6, Math.abs(testStats.tStat) + 2) : 6;
            for (let x = -range; x <= range; x += 0.1) {
                data.push({
                    x: parseFloat(x.toFixed(2)),
                    y: parseFloat(normalPDF(x).toFixed(4)),
                    isExtreme: testStats ? (testType === 'two-tailed' ? Math.abs(x) >= Math.abs(testStats.tStat) : (testType === 'right-tailed' ? x >= testStats.tStat : x <= testStats.tStat)) : false
                });
            }
            return data;
        };

        // --- Sections Data ---
        const sections = [
            { title: "What is a P-Value?", icon: "book-open", content: "fundamental-concept" },
            { title: "Probability Foundation", icon: "calculator", content: "probability-foundation" },
            { title: "Interactive Testing", icon: "trending-up", content: "interactive-testing" },
            { title: "Visual Calculation", icon: "lightbulb", content: "visual-calculation" },
            { title: "Common Misconceptions", icon: "rotate-ccw", content: "misconceptions" }
        ];

        // --- DOM Elements ---
        const mainContent = document.getElementById('main-content');
        const navContainer = document.getElementById('nav-container');
        const progressBar = document.getElementById('progress-bar');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        
        // --- Core Render Function ---
        const render = () => {
            const testStats = calculateTestStats();
            
            // Destroy old charts
            Object.values(chartInstances).forEach(chart => chart.destroy());
            chartInstances = {};

            // Render section content
            mainContent.innerHTML = getSectionHTML(currentSection, testStats);

            // Update UI elements
            updateNavigation();
            updateProgressBar();
            updateFooterButtons();

            // Re-initialize charts and event listeners for the new content
            initializeSectionUI(currentSection, testStats);

            // Render icons
            lucide.createIcons();
        };

        // --- HTML Generation ---
        const getSectionHTML = (sectionIndex, testStats) => {
            const section = sections[sectionIndex];
            switch (section.content) {
                case 'fundamental-concept': return `
                    <div class="space-y-8">
                        <div class="bg-slate-100 p-6 rounded-lg border border-slate-200">
                            <h3 class="text-xl font-semibold mb-3 text-slate-800">The Core Definition</h3>
                            <div class="text-lg mb-4 font-mono bg-white p-4 rounded-md">P-value = P(Data ≥ Observed | H₀ is true)</div>
                            <p class="text-slate-600 leading-relaxed max-w-prose">A p-value is the probability of observing data as extreme as, or more extreme than, what we actually observed, <strong class="font-semibold text-slate-700">assuming the null hypothesis is true</strong>.</p>
                        </div>
                        <div class="grid md:grid-cols-2 gap-6">
                            <div class="bg-white p-6 rounded-lg border border-slate-200">
                                <h4 class="font-semibold mb-4 text-slate-800">What P-values Tell Us</h4>
                                <ul class="space-y-3 text-slate-600">
                                    <li class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> How surprising our data is under H₀</li>
                                    <li class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> The strength of evidence against H₀</li>
                                    <li class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> How incompatible our data is with H₀</li>
                                </ul>
                            </div>
                            <div class="bg-white p-6 rounded-lg border border-slate-200">
                                <h4 class="font-semibold mb-4 text-slate-800">What P-values DON'T Tell Us</h4>
                                <ul class="space-y-3 text-slate-600">
                                    <li class="flex items-start"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> The probability that H₀ is true</li>
                                    <li class="flex items-start"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> The probability that H₁ is true</li>
                                    <li class="flex items-start"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> The size or practical importance of an effect</li>
                                </ul>
                            </div>
                        </div>
                        <div class="bg-blue-50 text-blue-800 p-6 rounded-lg border border-blue-200">
                            <h4 class="font-semibold mb-2">Intuitive Analogy</h4>
                            <p class="text-sm leading-6">Think of a p-value as a "surprise index." If you assume a coin is fair (H₀) but flip 10 heads in a row, the p-value is tiny. It tells you: "If the coin really is fair, this outcome is exceptionally rare and surprising." A low p-value signals a surprising result.</p>
                        </div>
                    </div>`;

                case 'probability-foundation': return `
                    <div class="space-y-8">
                        <div class="p-6 rounded-lg border border-slate-200 bg-white">
                            <h3 class="text-xl font-semibold mb-6 text-slate-800">Mathematical Foundation</h3>
                            <div class="space-y-8">
                                <div>
                                    <h4 class="font-semibold mb-3 text-slate-700">1. Test Statistics</h4>
                                    <div class="bg-slate-50 p-4 rounded-md border border-slate-200 font-mono text-sm text-slate-700 space-y-2">
                                        <div><span class="font-semibold text-slate-800">Z-score:</span> Z = (x̄ - μ₀)/(σ/√n) ~ N(0,1)</div>
                                        <div><span class="font-semibold text-slate-800">t-statistic:</span> t = (x̄ - μ₀)/(s/√n) ~ t(n-1)</div>
                                        <div><span class="font-semibold text-slate-800">χ² statistic:</span> χ² = Σ(Oᵢ - Eᵢ)²/Eᵢ ~ χ²(df)</div>
                                    </div>
                                </div>
                                <div>
                                    <h4 class="font-semibold mb-3 text-slate-700">2. Sampling Distribution</h4>
                                    <p class="text-sm text-slate-600 mb-4 max-w-prose">Under H₀, our test statistic follows a known probability distribution. This allows us to calculate the probability of observing our result.</p>
                                    <div style="height: 250px;"><canvas id="dist-line-chart"></canvas></div>
                                </div>
                                <div>
                                    <h4 class="font-semibold mb-3 text-slate-700">3. P-value Calculation</h4>
                                    <div class="bg-slate-50 p-4 rounded-md border border-slate-200 text-sm text-slate-700 space-y-2">
                                        <div><strong>Two-tailed:</strong> P = 2 × P(|T| ≥ |t_obs|)</div>
                                        <div><strong>Right-tailed:</strong> P = P(T ≥ t_obs)</div>
                                        <div><strong>Left-tailed:</strong> P = P(T ≤ t_obs)</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>`;

                case 'interactive-testing': return `
                    <div class="space-y-6">
                        <div class="bg-white p-6 rounded-lg border border-slate-200">
                            <h3 class="text-xl font-semibold mb-6 text-slate-800">Interactive Hypothesis Test</h3>
                            <div class="grid lg:grid-cols-5 gap-8">
                                <div class="lg:col-span-2 space-y-6">
                                    <div>
                                        <label class="block text-sm font-medium mb-2 text-slate-600">Sample Size (n)</label>
                                        <input id="sample-size-slider" type="range" min="10" max="200" value="${sampleSize}" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer" />
                                        <span class="text-sm text-slate-500">n = ${sampleSize}</span>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium mb-2 text-slate-600">Null Hypothesis Mean (μ₀)</label>
                                        <input id="true-mean-slider" type="range" min="-3" max="3" step="0.1" value="${trueMean}" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer" />
                                        <span class="text-sm text-slate-500">μ₀ = ${trueMean.toFixed(1)}</span>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium mb-2 text-slate-600">Observed Sample Mean (x̄)</label>
                                        <input id="sample-mean-slider" type="range" min="-3" max="3" step="0.1" value="${sampleMean}" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer" />
                                        <span class="text-sm text-slate-500">x̄ = ${sampleMean.toFixed(1)}</span>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium mb-2 text-slate-600">Test Type</label>
                                        <select id="test-type-select" class="w-full p-2 border border-slate-300 rounded-md bg-white text-sm">
                                            <option value="two-tailed" ${testType === 'two-tailed' ? 'selected' : ''}>Two-tailed (μ ≠ μ₀)</option>
                                            <option value="right-tailed" ${testType === 'right-tailed' ? 'selected' : ''}>Right-tailed (μ > μ₀)</option>
                                            <option value="left-tailed" ${testType === 'left-tailed' ? 'selected' : ''}>Left-tailed (μ < μ₀)</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="lg:col-span-3 bg-slate-50 p-6 rounded-lg border border-slate-200">
                                    <h4 class="font-semibold mb-4 text-slate-800">Test Results</h4>
                                    ${testStats ? `
                                    <div class="space-y-3 text-sm">
                                        <div class="flex justify-between"><span>Sample Mean:</span> <span class="font-mono text-slate-700">${testStats.mean.toFixed(3)}</span></div>
                                        <div class="flex justify-between"><span>Standard Error:</span> <span class="font-mono text-slate-700">${testStats.standardError.toFixed(3)}</span></div>
                                        <div class="flex justify-between"><span>t-statistic:</span> <span class="font-mono text-slate-700">${testStats.tStat.toFixed(3)}</span></div>
                                        <div class="flex justify-between"><span>Degrees of Freedom:</span> <span class="font-mono text-slate-700">${testStats.df}</span></div>
                                        <div class="border-t border-slate-200 my-3"></div>
                                        <div class="flex justify-between items-center text-base">
                                            <strong class="font-semibold">P-value:</strong>
                                            <span class="font-mono font-semibold px-2 py-1 rounded-md ${testStats.pValue < significance ? 'bg-red-100 text-red-700' : 'bg-green-100 text-green-700'}">${testStats.pValue.toFixed(6)}</span>
                                        </div>
                                        <div class="text-right text-sm font-semibold ${testStats.pValue < significance ? 'text-red-600' : 'text-green-600'}">${testStats.pValue < significance ? 'Statistically Significant' : 'Not Statistically Significant'}</div>
                                    </div>` : ''}
                                </div>
                            </div>
                        </div>
                        <div class="bg-white p-6 rounded-lg border border-slate-200">
                            <div class="flex flex-col sm:flex-row justify-between sm:items-center mb-4">
                                <h4 class="font-semibold text-slate-800 mb-3 sm:mb-0">Sample Distribution (n = ${currentSample.length})</h4>
                                <button id="new-sample-btn" class="px-4 py-2 bg-slate-700 text-white text-sm font-medium rounded-md hover:bg-slate-800 transition-colors">Generate New Sample</button>
                            </div>
                            <div style="height: 250px;"><canvas id="sample-scatter-chart"></canvas></div>
                        </div>
                    </div>`;

                case 'visual-calculation': return `
                    <div class="space-y-6">
                        <div class="bg-white p-6 rounded-lg border border-slate-200">
                            <h3 class="text-xl font-semibold mb-2 text-slate-800">Visual P-Value Calculation</h3>
                            <p class="text-sm text-slate-600 mb-6 max-w-prose">The p-value is the total shaded area under the curve representing regions as, or more, extreme than your observed test statistic.</p>
                            <div style="height: 300px;"><canvas id="pval-area-chart"></canvas></div>
                        </div>
                        <div class="mt-4 p-6 bg-slate-50 rounded-lg border border-slate-200">
                            <h4 class="font-semibold mb-3 text-slate-800">Simulation</h4>
                            <p class="text-sm text-slate-600 mb-4 max-w-prose">Run 1000 simulations using samples drawn from a population where H₀ is true. See how often we get a "significant" result just by chance. This illustrates the Type I error rate.</p>
                            <button id="run-simulation-btn" class="px-5 py-2 bg-blue-600 text-white text-sm font-medium rounded-md hover:bg-blue-700 transition-colors mb-4">Run 1000 Simulations</button>
                            <div id="simulation-results">
                                ${simulations.length > 0 ? `
                                <div class="space-y-4">
                                    <p class="text-sm text-slate-600">Significant results (p < ${significance}): <strong>${simulations.filter(s => s.significant).length} / 1000</strong> (${(simulations.filter(s => s.significant).length / 10).toFixed(1)}%)</p>
                                    <div style="height: 200px;"><canvas id="simulation-bar-chart"></canvas></div>
                                </div>` : ''}
                            </div>
                        </div>
                    </div>`;

                case 'misconceptions': return `
                    <div class="space-y-6">
                        <div class="bg-red-50 border-l-4 border-red-400 p-6"><h3 class="text-xl font-semibold text-red-900">Common Misconceptions</h3></div>
                        <div class="space-y-4">
                            <div class="bg-white p-5 rounded-lg border border-slate-200">
                                <p class="flex items-start mb-2"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Wrong:</strong> "A p-value of 0.03 means there's a 3% chance the null hypothesis is true."</p>
                                <p class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Correct:</strong> "If the null hypothesis were true, there would be a 3% chance of observing data this extreme."</p>
                            </div>
                            <div class="bg-white p-5 rounded-lg border border-slate-200">
                                <p class="flex items-start mb-2"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Wrong:</strong> "A non-significant result (p > 0.05) means the null hypothesis is true."</p>
                                <p class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Correct:</strong> "A non-significant result means we failed to find sufficient evidence to reject the null hypothesis."</p>
                            </div>
                            <div class="bg-white p-5 rounded-lg border border-slate-200">
                                <p class="flex items-start mb-2"><i data-lucide="x-circle" class="w-5 h-5 text-red-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Wrong:</strong> "A small p-value means the effect is large and important."</p>
                                <p class="flex items-start"><i data-lucide="check-circle-2" class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0"></i> <strong class="text-slate-700">Correct:</strong> "A p-value is influenced by both effect size and sample size. A tiny effect can be statistically significant with a large enough sample."</p>
                            </div>
                        </div>
                        <div class="bg-slate-50 p-6 rounded-lg border border-slate-200">
                            <h3 class="text-lg font-semibold mb-4 text-slate-800">Best Practices</h3>
                            <div class="grid md:grid-cols-2 gap-6 text-sm">
                                <div>
                                    <h4 class="font-semibold mb-3 text-green-700">Do</h4>
                                    <ul class="space-y-2 text-slate-600">
                                        <li class="flex items-start"><i data-lucide="check-circle-2" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-green-500"></i> Report effect sizes and confidence intervals.</li>
                                        <li class="flex items-start"><i data-lucide="check-circle-2" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-green-500"></i> Pre-register your analysis plan.</li>
                                        <li class="flex items-start"><i data-lucide="check-circle-2" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-green-500"></i> Interpret p-values in the context of the research.</li>
                                    </ul>
                                </div>
                                <div>
                                    <h4 class="font-semibold mb-3 text-red-700">Don't</h4>
                                    <ul class="space-y-2 text-slate-600">
                                        <li class="flex items-start"><i data-lucide="x-circle" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-red-500"></i> Treat 0.05 as a sacred, absolute threshold.</li>
                                        <li class="flex items-start"><i data-lucide="x-circle" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-red-500"></i> Engage in "p-hacking" (testing until significance is found).</li>
                                        <li class="flex items-start"><i data-lucide="x-circle" class="w-4 h-4 mr-2 mt-0.5 flex-shrink-0 text-red-500"></i> Base conclusions solely on p-values.</li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>`;

                default: return `<div>Content not found</div>`;
            }
        };

        // --- UI Initialization and Event Listeners ---
        const initializeSectionUI = (sectionIndex, testStats) => {
            const section = sections[sectionIndex];
            switch (section.content) {
                case 'probability-foundation':
                    renderDistLineChart(testStats);
                    break;
                case 'interactive-testing':
                    document.getElementById('sample-size-slider').addEventListener('input', e => { sampleSize = parseInt(e.target.value); render(); });
                    document.getElementById('true-mean-slider').addEventListener('input', e => { trueMean = parseFloat(e.target.value); render(); });
                    document.getElementById('sample-mean-slider').addEventListener('input', e => { sampleMean = parseFloat(e.target.value); render(); });
                    document.getElementById('test-type-select').addEventListener('change', e => { testType = e.target.value; render(); });
                    document.getElementById('new-sample-btn').addEventListener('click', () => { currentSample = generateNormalData(sampleSize, sampleMean, sampleStd); render(); });
                    renderSampleScatterChart(testStats);
                    break;
                case 'visual-calculation':
                    document.getElementById('run-simulation-btn').addEventListener('click', () => {
                        simulations = []; // Clear old before running new
                        for (let i = 0; i < 1000; i++) {
                            const sample = generateNormalData(sampleSize, trueMean, 2);
                            const mean = sample.reduce((a, b) => a + b, 0) / sample.length;
                            const std = Math.sqrt(sample.reduce((a, b) => a + Math.pow(b - mean, 2), 0) / (sample.length - 1));
                            const tStat = (mean - trueMean) / (std / Math.sqrt(sample.length));
                            const pValue = 2 * (1 - normalCDF(Math.abs(tStat)));
                            simulations.push({ pValue, significant: pValue < significance });
                        }
                        render(); // Re-render to show simulation results
                    });
                    renderPvalAreaChart(testStats);
                    if (simulations.length > 0) renderSimulationBarChart();
                    break;
            }
        };

        // --- Chart Rendering Functions ---
        const commonChartOptions = {
            maintainAspectRatio: false,
            responsive: true,
            plugins: { legend: { display: false }, tooltip: {
                backgroundColor: '#ffffff', titleColor: '#1e293b', bodyColor: '#475569',
                borderColor: '#e2e8f0', borderWidth: 1, boxPadding: 8,
            }},
            scales: {
                x: { ticks: { color: '#64748b', font: { size: 12 } }, grid: { color: '#e2e8f0' } },
                y: { ticks: { color: '#64748b', font: { size: 12 } }, grid: { color: '#e2e8f0' } }
            }
        };

        const renderDistLineChart = (testStats) => {
            const ctx = document.getElementById('dist-line-chart')?.getContext('2d');
            if (!ctx) return;
            const data = generateDistributionData(testStats);
            chartInstances.distLine = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: data.map(d => d.x),
                    datasets: [{
                        label: 'Probability Density',
                        data: data.map(d => d.y),
                        borderColor: '#3b82f6',
                        borderWidth: 2,
                        pointRadius: 0,
                        tension: 0.1
                    }]
                },
                options: commonChartOptions
            });
        };

        const renderSampleScatterChart = (testStats) => {
            const ctx = document.getElementById('sample-scatter-chart')?.getContext('2d');
            if (!ctx) return;
            const data = currentSample.map((val, i) => ({ x: i, y: val }));
            chartInstances.scatter = new Chart(ctx, {
                type: 'scatter',
                data: {
                    datasets: [{
                        label: 'Sample Data',
                        data: data,
                        backgroundColor: '#3b82f6',
                    }]
                },
                options: { ...commonChartOptions,
                    scales: {
                         ...commonChartOptions.scales,
                         x: { ...commonChartOptions.scales.x, title: { display: true, text: 'Sample Index', color: '#64748b' } },
                         y: { ...commonChartOptions.scales.y, title: { display: true, text: 'Value', color: '#64748b' } }
                    },
                    plugins: { ...commonChartOptions.plugins,
                       annotation: {
                            annotations: {
                                h0Line: { type: 'line', yMin: trueMean, yMax: trueMean, borderColor: '#ef4444', borderWidth: 2, borderDash: [6, 6], label: { content: `H₀: μ₀ = ${trueMean.toFixed(1)}`, enabled: true, position: 'start', backgroundColor: 'rgba(255,255,255,0.7)', color: '#ef4444' } },
                                meanLine: { type: 'line', yMin: testStats?.mean || 0, yMax: testStats?.mean || 0, borderColor: '#16a34a', borderWidth: 2, borderDash: [6, 6], label: { content: `x̄ = ${testStats?.mean.toFixed(2)}`, enabled: true, position: 'end', backgroundColor: 'rgba(255,255,255,0.7)', color: '#16a34a' } }
                            }
                        }
                    }
                }
            });
        };
        
        const renderPvalAreaChart = (testStats) => {
            const ctx = document.getElementById('pval-area-chart')?.getContext('2d');
            if (!ctx) return;
            const data = generateDistributionData(testStats);
            
            const backgroundColors = data.map(d => d.isExtreme ? 'rgba(239, 68, 68, 0.3)' : 'rgba(59, 130, 246, 0.1)');

            chartInstances.area = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: data.map(d => d.x),
                    datasets: [{
                        label: 'Probability Density',
                        data: data.map(d => d.y),
                        borderColor: '#3b82f6',
                        borderWidth: 2,
                        pointRadius: 0,
                        fill: true,
                        backgroundColor: (context) => {
                            const chart = context.chart;
                            const {ctx, chartArea} = chart;
                            if(!chartArea) return null;
                            const gradient = ctx.createLinearGradient(chartArea.left, 0, chartArea.right, 0);
                            const tStatValue = testStats ? testStats.tStat : null;
                            const range = data[data.length - 1].x - data[0].x;

                            // Default fill
                            gradient.addColorStop(0, 'rgba(219, 234, 254, 0.5)');
                            gradient.addColorStop(1, 'rgba(219, 234, 254, 0.5)');
                            
                            // Add red stops for extreme areas
                            if(tStatValue !== null){
                                data.forEach((d, i) => {
                                    if(d.isExtreme){
                                        const stopPosition = (d.x - data[0].x) / range;
                                        gradient.addColorStop(Math.max(0, stopPosition-0.001), 'rgba(254, 202, 202, 0.7)');
                                        gradient.addColorStop(Math.min(1, stopPosition), 'rgba(254, 202, 202, 0.7)');
                                    }
                                });
                            }
                            return gradient;
                        },
                        tension: 0.1
                    }]
                },
                options: { ...commonChartOptions,
                   plugins: {
                        ...commonChartOptions.plugins,
                        annotation: {
                            annotations: {
                                ...(testStats && { tStatLine: { type: 'line', xMin: testStats.tStat, xMax: testStats.tStat, borderColor: '#ef4444', borderWidth: 2, label: { content: `t = ${testStats.tStat.toFixed(2)}`, enabled: true, position: 'start', yAdjust: -10, backgroundColor: 'rgba(255,255,255,0.7)', color: '#ef4444' } } }),
                                ...(testStats && testType === 'two-tailed' && { tStatLineNeg: { type: 'line', xMin: -testStats.tStat, xMax: -testStats.tStat, borderColor: '#ef4444', borderWidth: 2 } })
                            }
                        }
                   }
                }
            });
        };

        const renderSimulationBarChart = () => {
            const ctx = document.getElementById('simulation-bar-chart')?.getContext('2d');
            if (!ctx) return;
            const data = [
                simulations.filter(s => s.pValue < 0.01).length,
                simulations.filter(s => s.pValue >= 0.01 && s.pValue < 0.05).length,
                simulations.filter(s => s.pValue >= 0.05 && s.pValue < 0.1).length,
                simulations.filter(s => s.pValue >= 0.1).length
            ];
            chartInstances.bar = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['p < 0.01', 'p < 0.05', 'p < 0.1', 'p ≥ 0.1'],
                    datasets: [{
                        label: 'Count',
                        data: data,
                        backgroundColor: ['#dc2626', '#f97316', '#eab308', '#22c55e']
                    }]
                },
                options: commonChartOptions
            });
        };


        // --- UI Update Functions ---
        const updateNavigation = () => {
            navContainer.innerHTML = sections.map((section, index) => `
                <button data-index="${index}" class="nav-btn flex items-center space-x-2 px-3 py-2 sm:px-4 sm:py-2.5 rounded-full text-sm sm:text-base font-medium transition-all duration-200 outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-blue-500 ${currentSection === index ? 'bg-blue-600 text-white shadow-md' : 'bg-white text-slate-600 hover:bg-slate-100 hover:text-slate-800 border border-slate-200'}">
                    <i data-lucide="${section.icon}"></i>
                    <span class="hidden md:inline">${section.title}</span>
                    <span class="md:hidden">${section.title.split(' ')[0]}</span>
                </button>
            `).join('');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    currentSection = parseInt(e.currentTarget.dataset.index);
                    render();
                });
            });
        };

        const updateProgressBar = () => {
            const progress = sections.length > 1 ? (currentSection / (sections.length - 1)) * 100 : 0;
            progressBar.style.width = `${progress}%`;
        };

        const updateFooterButtons = () => {
            prevBtn.disabled = currentSection === 0;
            nextBtn.disabled = currentSection === sections.length - 1;
        };

        // --- Initial Setup ---
        document.addEventListener('DOMContentLoaded', () => {
            currentSample = generateNormalData(sampleSize, sampleMean, sampleStd);

            prevBtn.addEventListener('click', () => {
                if (currentSection > 0) {
                    currentSection--;
                    render();
                }
            });
            nextBtn.addEventListener('click', () => {
                if (currentSection < sections.length - 1) {
                    currentSection++;
                    render();
                }
            });

            render(); // Initial render
        });
    </script>
</body>
</html>
