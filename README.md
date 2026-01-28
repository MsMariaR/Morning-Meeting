<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Morning Meeting Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Custom Palette - Energetic & Playful */
        :root {
            --color-primary: #4F46E5; /* Indigo 600 */
            --color-secondary: #F59E0B; /* Amber 500 */
            --color-accent: #EC4899; /* Pink 500 */
            --color-bg: #F3F4F6; /* Gray 100 */
            --color-card: #FFFFFF;
        }

        body {
            background-color: var(--color-bg);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            color: #1F2937;
        }

        /* Chart Container Styling - Responsive Boundaries */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px; /* Constraint for larger screens */
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }

        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }

        /* Utility for step arrows without SVG */
        .step-arrow::after {
            content: "➔";
            position: absolute;
            right: -20px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 1.5rem;
            color: #9CA3AF;
            display: none;
        }

        @media (min-width: 768px) {
            .step-arrow::after {
                display: block;
            }
            .step-arrow:last-child::after {
                display: none;
            }
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <!-- HTML Comment: Palette Selection -->
    <!-- Palette: Energetic & Playful (Indigo, Amber, Pink). Vibrant and engaging for students. -->

    <!-- HTML Comment: Narrative Plan -->
    <!-- 
    Section 1: Header - Welcomes students and sets the tone.
    Section 2: The Flow - Visualizes the 3-step check-in process (Organize/Flow).
    Section 3: Data Aids - Charts to help students decide their numbers and words (Inform/Compare).
       - Chart A: Wellness Scale Definitions (Bar Chart) - Defining what 1-10 means.
       - Chart B: Emotion Wheel (Polar Area) - Providing vocabulary.
    Section 4: The Question - The specific prompt for community building.
    Section 5: Expectations - The "Circle Norms" displayed clearly (Organize/List).
    -->

    <!-- HTML Comment: Chart Selection Justification -->
    <!--
    1. Wellness Scale: Horizontal Bar Chart. Goal: Compare/Inform. Why: Clearly maps the abstract numbers (1-10) to concrete feelings/energy levels.
    2. Emotion Vocabulary: Polar Area Chart. Goal: Inform/Organize. Why: Displays categorical options in a visually engaging "wheel" format without implying a strict hierarchy.
    Confirming NO SVG and NO Mermaid JS used.
    -->

    <!-- Header Section -->
    <header class="max-w-7xl mx-auto mb-10 text-center">
        <h1 class="text-4xl md:text-5xl font-extrabold text-indigo-600 mb-2">Morning Community Circle</h1>
        <p class="text-xl text-gray-600">Check-In, Connect, & Prepare to Learn</p>
        <div class="mt-4 inline-block bg-white px-6 py-2 rounded-full shadow-sm">
            <span class="font-bold text-gray-500 uppercase tracking-wider text-sm">Today's Focus:</span>
            <span class="ml-2 font-medium text-indigo-500">Empathy & Understanding</span>
        </div>
    </header>

    <!-- Main Content Grid -->
    <main class="max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-12 gap-8">

        <!-- SECTION: THE CHECK-IN FLOW (Full Width) -->
        <section class="col-span-1 md:col-span-12">
            <div class="bg-white rounded-xl shadow-lg p-6 border-t-4 border-indigo-500">
                <h2 class="text-2xl font-bold mb-4 text-gray-800">📋 The Check-In Protocol</h2>
                <p class="mb-6 text-gray-600">Please prepare your response for the circle. Follow these three steps when it's your turn to share.</p>
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- Step 1 -->
                    <div class="step-arrow relative bg-indigo-50 rounded-lg p-6 text-center border border-indigo-100 hover:shadow-md transition-shadow">
                        <div class="text-4xl mb-3">1️⃣</div>
                        <h3 class="text-xl font-bold text-indigo-700 mb-2">Wellness Number</h3>
                        <p class="text-sm text-gray-600">Rate your energy and readiness to learn on a scale of <strong>1 to 10</strong>.</p>
                    </div>
                    <!-- Step 2 -->
                    <div class="step-arrow relative bg-pink-50 rounded-lg p-6 text-center border border-pink-100 hover:shadow-md transition-shadow">
                        <div class="text-4xl mb-3">2️⃣</div>
                        <h3 class="text-xl font-bold text-pink-700 mb-2">Current Emotion</h3>
                        <p class="text-sm text-gray-600">Identify one specific word that describes how you are <strong>feeling right now</strong>.</p>
                    </div>
                    <!-- Step 3 -->
                    <div class="step-arrow relative bg-amber-50 rounded-lg p-6 text-center border border-amber-100 hover:shadow-md transition-shadow">
                        <div class="text-4xl mb-3">3️⃣</div>
                        <h3 class="text-xl font-bold text-amber-700 mb-2">Community Question</h3>
                        <p class="text-sm text-gray-600">Share your answer to the <strong>Question of the Day</strong> (below).</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- SECTION: VISUAL AIDS (Left Column - 7 cols) -->
        <div class="col-span-1 md:col-span-7 flex flex-col gap-8">
            
            <!-- Wellness Scale Reference -->
            <section class="bg-white rounded-xl shadow-lg p-6 border-l-4 border-blue-500">
                <div class="mb-4">
                    <h2 class="text-xl font-bold text-gray-800">📊 Wellness Scale Reference</h2>
                    <p class="text-sm text-gray-500">Not sure what number to pick? Use this guide to gauge your energy.</p>
                </div>
                <div class="chart-container">
                    <canvas id="wellnessChart"></canvas>
                </div>
            </section>

            <!-- Emotion Vocabulary -->
            <section class="bg-white rounded-xl shadow-lg p-6 border-l-4 border-pink-500">
                <div class="mb-4">
                    <h2 class="text-xl font-bold text-gray-800">🧠 Emotion Vocabulary</h2>
                    <p class="text-sm text-gray-500">Expand your emotional intelligence. Are you just "Good," or are you actually...</p>
                </div>
                <div class="chart-container">
                    <canvas id="emotionChart"></canvas>
                </div>
            </section>

        </div>

        <!-- SECTION: QUESTION & EXPECTATIONS (Right Column - 5 cols) -->
        <div class="col-span-1 md:col-span-5 flex flex-col gap-8">

            <!-- Question of the Day -->
            <section class="bg-gradient-to-br from-amber-100 to-orange-100 rounded-xl shadow-lg p-8 border-2 border-amber-200 text-center transform hover:scale-[1.02] transition-transform duration-300">
                <div class="text-5xl mb-4">💡</div>
                <h2 class="text-xl font-bold text-amber-800 uppercase tracking-widest mb-2">Question of the Day</h2>
                <hr class="border-amber-300 mb-6 mx-auto w-16">
                <p class="text-2xl font-bold text-gray-800 leading-snug">
                    "If you could have any superpower specifically to help others, what would it be and why?"
                </p>
            </section>

            <!-- Circle Expectations -->
            <section class="bg-white rounded-xl shadow-lg p-6 border-t-4 border-green-500 flex-grow">
                <h2 class="text-2xl font-bold mb-6 text-gray-800">🤝 Circle Expectations</h2>
                
                <div class="space-y-4">
                    <!-- Expectation 1 -->
                    <div class="flex items-start p-3 bg-gray-50 rounded-lg">
                        <div class="text-2xl mr-4">👂</div>
                        <div>
                            <h4 class="font-bold text-gray-800">Active Listening</h4>
                            <p class="text-sm text-gray-600">Listen with your eyes, ears, and heart. No side conversations.</p>
                        </div>
                    </div>

                    <!-- Expectation 2 -->
                    <div class="flex items-start p-3 bg-gray-50 rounded-lg">
                        <div class="text-2xl mr-4">🤐</div>
                        <div>
                            <h4 class="font-bold text-gray-800">Mutual Respect</h4>
                            <p class="text-sm text-gray-600">What is said in the circle, stays in the circle. Respect privacy.</p>
                        </div>
                    </div>

                    <!-- Expectation 3 -->
                    <div class="flex items-start p-3 bg-gray-50 rounded-lg">
                        <div class="text-2xl mr-4">🎤</div>
                        <div>
                            <h4 class="font-bold text-gray-800">Speak Your Truth</h4>
                            <p class="text-sm text-gray-600">Use "I" statements. Be honest but kind. Share your own story.</p>
                        </div>
                    </div>

                    <!-- Expectation 4 -->
                    <div class="flex items-start p-3 bg-gray-50 rounded-lg">
                        <div class="text-2xl mr-4">🛑</div>
                        <div>
                            <h4 class="font-bold text-gray-800">Right to Pass</h4>
                            <p class="text-sm text-gray-600">It is okay to pass if you are not ready to share today.</p>
                        </div>
                    </div>
                </div>
            </section>

        </div>

    </main>

    <footer class="max-w-7xl mx-auto mt-12 text-center text-gray-400 text-sm pb-8">
        <p>Morning Meeting Dashboard • Facilitating Connection & Wellness</p>
    </footer>

    <script>
        // --- DATA PROCESSING UTILITIES ---

        // Helper to wrap text for chart labels (16 char limit)
        function wrapLabel(label) {
            const MAX_LENGTH = 16;
            if (label.length <= MAX_LENGTH) return label;
            
            const words = label.split(' ');
            const lines = [];
            let currentLine = words[0];

            for (let i = 1; i < words.length; i++) {
                if (currentLine.length + 1 + words[i].length <= MAX_LENGTH) {
                    currentLine += ' ' + words[i];
                } else {
                    lines.push(currentLine);
                    currentLine = words[i];
                }
            }
            lines.push(currentLine);
            return lines;
        }

        // Shared Tooltip Configuration
        const commonTooltipConfig = {
            callbacks: {
                title: function(tooltipItems) {
                    const item = tooltipItems[0];
                    let label = item.chart.data.labels[item.dataIndex];
                    if (Array.isArray(label)) {
                        return label.join(' ');
                    } else {
                        return label;
                    }
                }
            }
        };

        // --- CHART GENERATION ---

        // 1. Wellness Scale Chart (Horizontal Bar)
        // Helps students correlate their number (1-10) with a description.
        const ctxWellness = document.getElementById('wellnessChart').getContext('2d');
        
        // Data prep: Labels are the ranges, data is just visual height to show progression
        const wellnessLabels = ["1-2: Low Energy / Need Support", "3-4: Struggling / Distracted", "5-6: Coping / Okay", "7-8: Good / Ready to Learn", "9-10: Thriving / Energized"];
        const wrappedWellnessLabels = wellnessLabels.map(wrapLabel);

        new Chart(ctxWellness, {
            type: 'bar',
            data: {
                labels: wrappedWellnessLabels,
                datasets: [{
                    label: 'Energy Level',
                    data: [2, 4, 6, 8, 10], // Visual progression
                    backgroundColor: [
                        '#EF4444', // Red
                        '#F97316', // Orange
                        '#F59E0B', // Amber
                        '#10B981', // Emerald
                        '#3B82F6'  // Blue
                    ],
                    borderRadius: 5,
                    borderWidth: 1,
                    borderColor: '#fff'
                }]
            },
            options: {
                indexAxis: 'y', // Horizontal bar
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        ...commonTooltipConfig, // Spread common config
                        callbacks: {
                            ...commonTooltipConfig.callbacks,
                            label: function(context) {
                                // Custom tooltip label to give more context
                                const descriptions = [
                                    "I might need a check-in with a teacher.",
                                    "I'm here, but it's hard to focus.",
                                    "I'm neutral. Doing fine.",
                                    "I'm feeling positive and ready.",
                                    "I'm excited and feeling my best!"
                                ];
                                return descriptions[context.dataIndex];
                            }
                        }
                    }
                },
                scales: {
                    x: {
                        min: 0,
                        max: 10,
                        title: {
                            display: true,
                            text: 'Wellness Scale (1-10)'
                        }
                    }
                }
            }
        });

        // 2. Emotion Vocabulary (Polar Area)
        // Offers a menu of emotions beyond "Good" or "Bad".
        const ctxEmotion = document.getElementById('emotionChart').getContext('2d');
        
        const emotionLabels = ["Optimistic", "Anxious", "Grateful", "Exhausted", "Curious", "Frustrated"];
        // Labels are short enough here, but wrapping for consistency
        const wrappedEmotionLabels = emotionLabels.map(wrapLabel);

        new Chart(ctxEmotion, {
            type: 'polarArea',
            data: {
                labels: wrappedEmotionLabels,
                datasets: [{
                    label: 'Common Feelings',
                    data: [8, 5, 7, 4, 6, 5], // Arbitrary visual weighting for balance
                    backgroundColor: [
                        'rgba(245, 158, 11, 0.7)',  // Amber (Optimistic)
                        'rgba(107, 114, 128, 0.7)', // Gray (Anxious)
                        'rgba(16, 185, 129, 0.7)',  // Green (Grateful)
                        'rgba(99, 102, 241, 0.7)',  // Indigo (Exhausted)
                        'rgba(59, 130, 246, 0.7)',  // Blue (Curious)
                        'rgba(239, 68, 68, 0.7)'    // Red (Frustrated)
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    r: {
                        ticks: {
                            display: false // Hide numbers on radial axis
                        },
                        grid: {
                            color: '#e5e7eb'
                        }
                    }
                },
                plugins: {
                    legend: {
                        position: 'right',
                        labels: {
                            usePointStyle: true,
                            font: {
                                size: 12
                            }
                        }
                    },
                    tooltip: commonTooltipConfig
                }
            }
        });

    </script>
</body>
</html>
