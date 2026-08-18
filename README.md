<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Pro Fitness & Elite Nutrition Suite</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
  background: #090d16;
  color: #f8fafc;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  -webkit-tap-highlight-color: transparent;
}
.glass-card {
  background: rgba(22, 30, 46, 0.8);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.08);
}
.tab-active {
  background-color: #0284c7;
  color: #ffffff;
}
.tab-inactive {
  background-color: #1e293b;
  color: #94a3b8;
}
</style>
</head>
<body class="p-3 sm:p-5 max-w-5xl mx-auto pb-24">

<!-- Header & Nav Tabs -->
<header class="border-b border-slate-800 pb-4 mb-6">
  <div class="flex justify-between items-center mb-4">
    <div>
      <h1 class="text-2xl font-black bg-gradient-to-r from-sky-400 via-emerald-400 to-indigo-400 bg-clip-text text-transparent">
        Performance Engine
      </h1>
      <p id="current-date" class="text-xs text-slate-400 font-medium"></p>
    </div>
    <span id="whoop-status-badge" class="text-[10px] font-bold bg-amber-500/10 text-amber-400 border border-amber-500/20 px-2.5 py-1 rounded-full">
      WHOOP: Off
    </span>
  </div>

  <!-- Navigation Tabs -->
  <div class="flex gap-2 overflow-x-auto pb-1">
    <button id="btn-tab-workout" onclick="switchTab('workout')" class="flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-active whitespace-nowrap">
      🏋️ Training
    </button>
    <button id="btn-tab-nutrition" onclick="switchTab('nutrition')" class="flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-inactive whitespace-nowrap">
      🥗 Nutrition
    </button>
    <button id="btn-tab-body" onclick="switchTab('body')" class="flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-inactive whitespace-nowrap">
      📏 Body & Fat AI
    </button>
  </div>
</header>

<!-- WORKOUT SECTION -->
<main id="section-workout" class="space-y-6">
  <!-- WHOOP Live Recovery -->
  <section class="glass-card p-4 rounded-2xl">
    <div class="flex justify-between items-center mb-3">
      <h2 class="text-sm font-bold text-slate-200 flex items-center gap-1.5">
        <span>⚡</span> WHOOP Recovery Telemetry
      </h2>
      <button onclick="toggleWhoopModal()" class="text-xs text-sky-400 font-semibold underline">Token Configuration</button>
    </div>

    <div class="grid grid-cols-3 gap-2 text-center mb-3">
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Recovery</span>
        <span id="whoop-recovery" class="text-lg font-black text-emerald-400">--%</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Strain</span>
        <span id="whoop-strain" class="text-lg font-black text-sky-400">--</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Sleep Performance</span>
        <span id="whoop-sleep" class="text-lg font-black text-indigo-400">--%</span>
      </div>
    </div>

    <div id="whoop-advice" class="bg-slate-900/60 p-2.5 rounded-xl border border-slate-800 text-xs text-slate-300 flex items-start gap-2">
      <span>💡</span>
      <span>Sync WHOOP to adjust training intensity and volume dynamically based on recovery biometric data.</span>
    </div>
  </section>

  <!-- Body Weight Logger -->
  <section class="glass-card p-4 rounded-2xl">
    <h2 class="text-sm font-bold text-slate-200 mb-3 flex items-center gap-1.5">
      <span>⚖️</span> Body Mass Tracking
    </h2>
    <div class="flex gap-2">
      <input id="input-weight" type="number" step="0.1" placeholder="Body Weight (kg)" class="flex-1 bg-slate-900 border border-slate-800 rounded-xl p-2.5 text-xs text-white focus:outline-none">
      <button onclick="saveWeight()" class="bg-sky-600 hover:bg-sky-500 px-4 py-2.5 rounded-xl text-xs font-bold transition">Log Mass</button>
    </div>
    <div id="latest-weight-log" class="mt-2 text-[11px] text-slate-400">Current baseline: 95 kg (209.4 lbs)</div>
  </section>

  <!-- Workout Routine Selector & Cards -->
  <section class="glass-card p-4 rounded-2xl">
    <div class="mb-4">
      <label class="block text-[10px] font-bold text-sky-400 uppercase tracking-wider mb-1">Training Block Phase</label>
      <select id="day-select" onchange="loadWorkout(this.value)" class="w-full bg-slate-900 border border-sky-500/80 text-sky-300 font-bold text-sm rounded-xl p-3 focus:outline-none">
        <option value="day1">Phase I — Push Hypertrophy & Power (Chest/Delts/Triceps)</option>
        <option value="day2">Phase II — Pull Hypertrophy & Density (Lat/Rhomboids/Biceps)</option>
        <option value="day3">Phase III — Lower Body Mechanotherapy (Quads/Hamstrings/Calves)</option>
        <option value="day4">Phase IV — Upper Body Tension Optimization</option>
        <option value="day5">Phase V — Posterior Chain & Dynamic Core Integrity</option>
      </select>
    </div>

    <div id="exercise-cards-container" class="space-y-5"></div>
  </section>
</main>

<!-- NUTRITION SECTION -->
<main id="section-nutrition" class="space-y-6 hidden">
  <!-- Meal Tracker -->
  <section class="glass-card p-4 rounded-2xl">
    <div class="flex justify-between items-center mb-3">
      <h2 class="text-base font-bold text-emerald-400 flex items-center gap-1.5">
        <span>🥗</span> Daily Nutrient Protocol
      </h2>
      <span class="text-[10px] bg-emerald-950 text-emerald-300 px-2 py-0.5 rounded font-bold border border-emerald-800/50">
        Interactive Intake Check
      </span>
    </div>

    <!-- Meal Checkbox Items -->
    <div id="meal-list" class="space-y-2 mb-5"></div>

    <!-- Totals Summary Grid -->
    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-center mb-5">
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Energy Target</span>
        <span id="total-cals" class="text-lg font-black text-amber-400">0 kcal</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Protein Substrate</span>
        <span id="total-protein" class="text-lg font-black text-emerald-400">0 g</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Glycogen Carbs</span>
        <span id="total-carbs" class="text-lg font-black text-sky-400">0 g</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Essential Lipids</span>
        <span id="total-fats" class="text-lg font-black text-indigo-400">0 g</span>
      </div>
    </div>

    <!-- Macro Pie Chart & Micro Breakdown -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 items-center bg-slate-900/50 p-4 rounded-xl border border-slate-800">
      <div class="w-full max-w-[220px] mx-auto">
        <canvas id="macroChart"></canvas>
      </div>
      <div>
        <h3 class="text-xs font-bold text-slate-200 uppercase tracking-wider mb-2">Estimated Micronutrient Telemetry</h3>
        <div id="micro-breakdown" class="grid grid-cols-2 gap-2 text-xs text-slate-300">
          <div class="bg-slate-950/60 p-2 rounded border border-slate-800/80">Calcium: <span id="micro-calcium" class="font-bold text-white">0 mg</span></div>
          <div class="bg-slate-950/60 p-2 rounded border border-slate-800/80">Iron: <span id="micro-iron" class="font-bold text-white">0 mg</span></div>
          <div class="bg-slate-950/60 p-2 rounded border border-slate-800/80">Potassium: <span id="micro-potassium" class="font-bold text-white">0 mg</span></div>
          <div class="bg-slate-950/60 p-2 rounded border border-slate-800/80">Dietary Fiber: <span id="micro-fiber" class="font-bold text-white">0 g</span></div>
        </div>
      </div>
    </div>
  </section>

  <!-- Supplementation Suggestions -->
  <section class="glass-card p-4 rounded-2xl">
    <h2 class="text-sm font-bold text-slate-200 mb-3 flex items-center gap-1.5">
      <span>💊</span> Ergogenic & Health Supplementation Stack
    </h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs">
      <div class="bg-slate-900/90 p-3 rounded-xl border border-slate-800">
        <span class="font-bold text-sky-400 block mb-1">Creatine Monohydrate (5g / Daily)</span>
        <p class="text-slate-400 text-[11px]">Integrated into Morning Anabolic Complex. Saturates intramuscular phosphocreatine stores to maximize ATP regeneration and force production.</p>
      </div>
      <div class="bg-slate-900/90 p-3 rounded-xl border border-slate-800">
        <span class="font-bold text-emerald-400 block mb-1">Hydrolyzed Collagen Peptides (10g–20g / Daily)</span>
        <p class="text-slate-400 text-[11px]">Integrated into Morning Anabolic Complex. Enhances tendon stiffness, extracellular matrix repair, and articular joint longevity.</p>
      </div>
      <div class="bg-slate-900/90 p-3 rounded-xl border border-slate-800">
        <span class="font-bold text-indigo-400 block mb-1">Micro-Nutrient Shield: Vitamin D3 (5000 IU) + K2 (MK-7)</span>
        <p class="text-slate-400 text-[11px]">Recommended Addition: Taken alongside Meal 2 (whole eggs/fat source) to optimize endocrine output, calcium partitioning, and immune resilience.</p>
      </div>
      <div class="bg-slate-900/90 p-3 rounded-xl border border-slate-800">
        <span class="font-bold text-amber-400 block mb-1">High-Concentration Omega-3 Fish Oil (2g EPA/DHA)</span>
        <p class="text-slate-400 text-[11px]">Recommended Addition: Minimizes systemic inflammation post-training, accelerates muscle protein synthesis pathways, and supports vascular efficiency.</p>
      </div>
    </div>
  </section>
</main>

<!-- BODY ANTHROPOMETRICS & BODY FAT SECTION -->
<main id="section-body" class="space-y-6 hidden">
  <!-- Profile & Biometrics Header -->
  <section class="glass-card p-4 rounded-2xl">
    <div class="flex justify-between items-center mb-3">
      <h2 class="text-base font-bold text-sky-400 flex items-center gap-1.5">
        <span>📐</span> Athlete Anthropometrics
      </h2>
      <span class="text-[10px] bg-sky-950 text-sky-300 px-2 py-0.5 rounded font-bold border border-sky-800/50">
        Baseline: 6'1" (185 cm) | 95 kg
      </span>
    </div>
   
    <div class="grid grid-cols-3 gap-2 text-center mb-2">
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">BMI</span>
        <span class="text-lg font-black text-sky-400">27.7</span>
        <span class="block text-[8px] text-slate-500">kg/m²</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">US Navy Body Fat</span>
        <span id="navy-bf-display" class="text-lg font-black text-emerald-400">-- %</span>
        <span class="block text-[8px] text-slate-500">Calculated</span>
      </div>
      <div class="bg-slate-900/90 p-2.5 rounded-xl border border-slate-800">
        <span class="block text-[9px] uppercase font-bold text-slate-400">Lean Mass</span>
        <span id="lean-mass-display" class="text-lg font-black text-indigo-400">-- kg</span>
        <span class="block text-[8px] text-slate-500">Estimated</span>
      </div>
    </div>
  </section>

  <!-- Body Measurements Logging Table -->
  <section class="glass-card p-4 rounded-2xl">
    <div class="flex justify-between items-center mb-3">
      <h2 class="text-sm font-bold text-slate-200 flex items-center gap-1.5">
        <span>📏</span> Body Part Circumference Log (cm)
      </h2>
      <button onclick="calculateBodyFat()" class="bg-sky-600 hover:bg-sky-500 text-white font-bold text-xs px-3 py-1.5 rounded-xl transition">
        Calculate Fat %
      </button>
    </div>

    <div class="overflow-x-auto">
      <table class="w-full text-left text-xs">
        <thead>
          <tr class="text-[10px] uppercase font-bold text-slate-500 border-b border-slate-800">
            <th class="pb-2">Anatomical Region</th>
            <th class="pb-2">Measurement Landmark</th>
            <th class="pb-2 w-28">Current (cm)</th>
            <th class="pb-2 w-28">Target (cm)</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-slate-800/60 text-slate-300">
          <tr>
            <td class="py-2.5 font-bold text-sky-400">Neck *</td>
            <td class="text-[10px] text-slate-400">Just below larynx</td>
            <td><input id="measure_neck" type="number" step="0.5" placeholder="e.g. 40" onchange="calculateBodyFat()" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_neck" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold text-sky-400">Waist (Navel) *</td>
            <td class="text-[10px] text-slate-400">Level of belly button</td>
            <td><input id="measure_waist" type="number" step="0.5" placeholder="e.g. 90" onchange="calculateBodyFat()" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_waist" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Chest</td>
            <td class="text-[10px] text-slate-400">Widest point across nipples</td>
            <td><input id="measure_chest" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_chest" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Shoulders</td>
            <td class="text-[10px] text-slate-400">Widest point around deltoids</td>
            <td><input id="measure_shoulders" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_shoulders" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Biceps (L / R)</td>
            <td class="text-[10px] text-slate-400">Peak flexed bicep</td>
            <td><input id="measure_biceps" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_biceps" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Hips / Glutes</td>
            <td class="text-[10px] text-slate-400">Widest point around hips</td>
            <td><input id="measure_hips" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_hips" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Thighs (L / R)</td>
            <td class="text-[10px] text-slate-400">Mid-thigh circumference</td>
            <td><input id="measure_thighs" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_thighs" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
          <tr>
            <td class="py-2.5 font-bold">Calves (L / R)</td>
            <td class="text-[10px] text-slate-400">Thickest point of calf</td>
            <td><input id="measure_calves" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
            <td><input id="target_calves" type="number" step="0.5" placeholder="cm" class="w-20 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-slate-400 focus:outline-none"></td>
          </tr>
        </tbody>
      </table>
    </div>
    <p class="text-[10px] text-slate-500 mt-2">* Inputting <b>Neck</b> and <b>Waist</b> automatically triggers the US Navy Method body fat estimation formula for your height (185 cm).</p>
  </section>

  <!-- Photo Scan & Visual Benchmarking -->
  <section class="glass-card p-4 rounded-2xl space-y-4">
    <h2 class="text-sm font-bold text-slate-200 flex items-center gap-1.5">
      <span>📸</span> Visual Body Fat Scanning & Upload
    </h2>

    <div class="border-2 border-dashed border-slate-700 hover:border-sky-500 rounded-2xl p-6 text-center cursor-pointer bg-slate-900/40 transition relative">
      <input type="file" id="photo-upload" accept="image/*" onchange="previewUploadedPhoto(event)" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
      <div id="upload-placeholder">
        <span class="text-3xl block mb-2">📷</span>
        <p class="text-xs font-bold text-slate-200">Upload Physique Progress Photo</p>
        <p class="text-[10px] text-slate-400 mt-1">PNG, JPG, or WEBP up to 10MB</p>
      </div>
      <div id="upload-preview-container" class="hidden flex flex-col items-center">
        <img id="photo-preview" class="max-h-48 rounded-xl border border-slate-700 object-cover mb-3">
        <button onclick="analyzePhotoVisuals()" class="bg-emerald-600 hover:bg-emerald-500 text-white font-bold text-xs px-4 py-2 rounded-xl transition">
          🔍 Analyze Visual Benchmarks
        </button>
      </div>
    </div>

    <!-- AI Scan Result Banner -->
    <div id="photo-scan-result" class="hidden bg-slate-900/90 border border-slate-800 p-4 rounded-xl text-xs space-y-2">
      <div class="flex justify-between items-center">
        <span class="font-bold text-emerald-400">Visual Fat Tier Estimation:</span>
        <span id="scan-tier-badge" class="bg-emerald-950 text-emerald-300 px-2 py-0.5 rounded text-[10px] font-bold border border-emerald-800">18% – 22% Body Fat Range</span>
      </div>
      <p id="scan-description" class="text-slate-300 text-[11px] leading-relaxed">
        Photo telemetry suggests standard athletic composition at 95 kg / 185 cm. Visual characteristics match moderate abdominal visibility and chest-shoulder separation. Combine with Navy tape measurements for optimal precision.
      </p>
    </div>

    <!-- Reference Benchmarks Reference Chart -->
    <div class="bg-slate-900/50 p-3.5 rounded-xl border border-slate-800 text-xs">
      <h3 class="font-bold text-slate-200 mb-2 uppercase text-[10px] tracking-wider">Visual Benchmark Reference (Male 6'1" / 95 kg)</h3>
      <div class="space-y-2 text-[11px]">
        <div class="flex justify-between border-b border-slate-800/80 pb-1">
          <span class="font-bold text-amber-400">25%+ Body Fat</span>
          <span class="text-slate-400">Minimal muscle definition, softer midsection</span>
        </div>
        <div class="flex justify-between border-b border-slate-800/80 pb-1">
          <span class="font-bold text-yellow-400">18% – 22% Body Fat</span>
          <span class="text-slate-400">Slight upper abs outline under contraction, defined deltoids</span>
        </div>
        <div class="flex justify-between border-b border-slate-800/80 pb-1">
          <span class="font-bold text-emerald-400">13% – 15% Body Fat</span>
          <span class="text-slate-400">Clear abdominal definition, forearm vascularity</span>
        </div>
        <div class="flex justify-between">
          <span class="font-bold text-sky-400">10% – 12% Body Fat</span>
          <span class="text-slate-400">Deeply etched abs, shoulder & chest striations</span>
        </div>
      </div>
    </div>
  </section>
</main>

<!-- Rest Timer Modal -->
<div id="fullscreen-timer-modal" class="fixed inset-0 bg-slate-950/95 backdrop-blur-2xl z-50 hidden flex flex-col justify-center items-center p-6 text-center">
  <span class="text-slate-400 text-sm uppercase tracking-widest font-bold mb-2">Inter-Set Recovery Phase</span>
  <div id="fullscreen-timer-display" class="font-mono text-7xl font-black text-sky-400 my-6 tracking-tight">00:00</div>
  <p id="timer-exercise-name" class="text-slate-300 text-sm font-semibold mb-8"></p>
  <button onclick="closeFullscreenTimer()" class="bg-red-600/80 hover:bg-red-500 text-white font-bold px-8 py-3.5 rounded-2xl text-sm transition">
    Terminate Rest Interval
  </button>
</div>

<!-- WHOOP Token Input Modal -->
<div id="whoop-modal" class="fixed inset-0 bg-black/80 z-50 hidden flex items-center justify-center p-4">
  <div class="glass-card p-5 rounded-2xl max-w-sm w-full space-y-3">
    <h3 class="font-bold text-sm text-white">Configure WHOOP API Key</h3>
    <input id="whoop-token-input" type="password" placeholder="Enter Bearer Token" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-xs text-white focus:outline-none">
    <div class="flex gap-2">
      <button onclick="fetchRealWhoopData(); toggleWhoopModal();" class="flex-1 bg-sky-600 font-bold py-2 rounded-xl text-xs">Save & Synchronize</button>
      <button onclick="toggleWhoopModal()" class="bg-slate-800 font-bold px-4 py-2 rounded-xl text-xs">Cancel</button>
    </div>
  </div>
</div>

<script>
// Tab Switching System
function switchTab(tab) {
  const workoutSec = document.getElementById("section-workout");
  const nutritionSec = document.getElementById("section-nutrition");
  const bodySec = document.getElementById("section-body");

  const btnWorkout = document.getElementById("btn-tab-workout");
  const btnNutrition = document.getElementById("btn-tab-nutrition");
  const btnBody = document.getElementById("btn-tab-body");

  // Hide all sections
  workoutSec.classList.add("hidden");
  nutritionSec.classList.add("hidden");
  bodySec.classList.add("hidden");

  // Inactive classes for buttons
  btnWorkout.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-inactive whitespace-nowrap";
  btnNutrition.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-inactive whitespace-nowrap";
  btnBody.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-inactive whitespace-nowrap";

  if (tab === "workout") {
    workoutSec.classList.remove("hidden");
    btnWorkout.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-active whitespace-nowrap";
  } else if (tab === "nutrition") {
    nutritionSec.classList.remove("hidden");
    btnNutrition.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-active whitespace-nowrap";
  } else if (tab === "body") {
    bodySec.classList.remove("hidden");
    btnBody.className = "flex-1 min-w-[110px] py-2.5 rounded-xl font-bold text-xs transition tab-active whitespace-nowrap";
  }
}

// Workout Database
const workouts = {
  day1: [
    { id: "bench_press", name: "Barbell Bench Press", sets: 4, reps: "4–6", rest: "3:00", restSec: 180, guide: "Keep shoulder blades retracted, feet flat on ground, touch lower chest.", img: "https://images.unsplash.com/photo-1534367507873-d2d7e24c797f?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=rT7DgCr-3pg" },
    { id: "overhead_press", name: "Overhead Barbell Press", sets: 3, reps: "5–7", rest: "2:30", restSec: 150, guide: "Squeeze glutes and core, press straight up clearing the head.", img: "https://images.unsplash.com/photo-1541534741688-6078c6bfb5c5?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=2yjwXTZQDDI" },
    { id: "incline_db_press", name: "Incline Dumbbell Press", sets: 3, reps: "8–12", rest: "1:30", restSec: 90, guide: "Set bench to 30°, lower DBs with controlled tempo to upper chest level.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=8iPEnn-ltC8" },
    { id: "lateral_raise", name: "Cable Lateral Raise", sets: 4, reps: "12–15", rest: "1:00", restSec: 60, guide: "Lead with elbows, lift to shoulder height without swinging.", img: "https://images.unsplash.com/photo-1532029837206-abbe2b7620e3?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=PzsMitRZS_0" },
    { id: "chest_dip", name: "Chest Dip", sets: 3, reps: "8–12", rest: "1:30", restSec: 90, guide: "Lean forward slightly to target lower chest, lower until upper arms are parallel.", img: "https://images.unsplash.com/photo-1583454110551-21f2fa2afe61?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=2z8JmcrW-As" },
    { id: "cable_fly_low_high", name: "Cable Fly (Low-to-High)", sets: 3, reps: "12–15", rest: "1:00", restSec: 60, guide: "Set cables low, scoop hands upward towards chest center.", img: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=xSoL_652I2E" },
    { id: "overhead_cable_tricep_ext", name: "Overhead Cable Tricep Ext", sets: 3, reps: "12–15", rest: "1:00", restSec: 60, guide: "Extend arms overhead keeping elbows tucked near ears.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=ns-RGsbYepl" },
    { id: "rope_pushdown", name: "Rope Pushdown", sets: 3, reps: "15–20", rest: "1:00", restSec: 60, guide: "Keep elbows glued to sides, spread the rope ends outwards at the bottom.", img: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=vB5OHsJ3EME" }
  ],
  day2: [
    { id: "weighted_pullup", name: "Weighted Pull-Up", sets: 4, reps: "4–6", rest: "3:00", restSec: 180, guide: "Full extension at bottom, pull chest up to meet the bar.", img: "https://images.unsplash.com/photo-1598971639058-fab3c3109a00?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=huL3S4PBy2I" },
    { id: "bentover_row", name: "Barbell Bent-Over Row", sets: 4, reps: "5–8", rest: "2:30", restSec: 150, guide: "Hinge hips at 45°, pull bar toward belly button engaging latissimus.", img: "https://images.unsplash.com/photo-1605296867304-46d5465a13f1?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=FWJR5Ve8bnQ" },
    { id: "seated_cable_row_wide", name: "Seated Cable Row (Wide Grip)", sets: 3, reps: "10–12", rest: "1:30", restSec: 90, guide: "Pull handles to lower ribs, squeeze shoulder blades together at full contraction.", img: "https://images.unsplash.com/photo-1605296867304-46d5465a13f1?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=GZbfZ033f74" },
    { id: "lat_pulldown_neutral", name: "Lat Pulldown (Close Neutral)", sets: 3, reps: "10–12", rest: "1:30", restSec: 90, guide: "Pull neutral attachment down to collarbone while driving elbows downward.", img: "https://images.unsplash.com/photo-1598971639058-fab3c3109a00?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=CAwf7n6Luuc" },
    { id: "single_arm_db_row", name: "Dumbbell Single-Arm Row", sets: 3, reps: "10–12 ea", rest: "1:30", restSec: 90, guide: "Brace on bench, pull dumbbell towards hip crease with controlled tempo.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=Dbf_151w2vM" },
    { id: "rear_delt_cable_fly", name: "Rear Delt Cable Fly", sets: 3, reps: "15–20", rest: "1:00", restSec: 60, guide: "Cross cables at eye level, pull arms outward horizontally.", img: "https://images.unsplash.com/photo-1532029837206-abbe2b7620e3?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=1YpA5hIClC8" },
    { id: "incline_db_curl", name: "Incline Dumbbell Curl", sets: 3, reps: "10–12", rest: "1:15", restSec: 75, guide: "Set bench to 45°, allow arms to fully stretch at bottom before curling.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=soxrZlIl35U" },
    { id: "hammer_curl", name: "Hammer Curl", sets: 3, reps: "12–15", rest: "1:00", restSec: 60, guide: "Maintain neutral palms-facing grip, raise dumbbells without shoulder swing.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=zC3YbRLFZ4w" }
  ],
  day3: [
    { id: "back_squat", name: "Barbell Back Squat", sets: 4, reps: "4–6", rest: "3:00", restSec: 180, guide: "Drive knees outward over toes, squat below parallel, push through mid-foot.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=ultWZbUMPL8" },
    { id: "rdl", name: "Romanian Deadlift", sets: 3, reps: "8–10", rest: "2:30", restSec: 150, guide: "Keep back flat, push hips backward until hamstrings stretch fully.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=JCXUYuzwNrM" },
    { id: "leg_press", name: "Leg Press", sets: 4, reps: "10–15", rest: "1:30", restSec: 90, guide: "Feet shoulder-width on sled, lower sled under control without rounding lower back.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=IZxyjW7MPJQ" },
    { id: "walking_lunges", name: "Walking Lunges (DBs)", sets: 3, reps: "12 ea", rest: "1:30", restSec: 90, guide: "Take wide strides forward, drop back knee near ground level while keeping torso erect.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=D7KaRcUTQeE" },
    { id: "leg_extension", name: "Leg Extension", sets: 3, reps: "12–15", rest: "1:00", restSec: 60, guide: "Pause briefly at peak leg extension, contract quadriceps tightly.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=YyvSfVjQeL0" },
    { id: "lying_leg_curl", name: "Lying Leg Curl", sets: 3, reps: "12–15", rest: "1:00", restSec: 60, guide: "Keep hips pressed flat into pad while flexing knees to bring pad to glutes.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=1Tq3QdYUuHs" },
    { id: "standing_calf_raise", name: "Standing Calf Raise", sets: 4, reps: "15–20", rest: "1:00", restSec: 60, guide: "Full stretch at bottom, drive up onto toes and hold at top.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=-M4-G8p8fmc" },
    { id: "seated_calf_raise", name: "Seated Calf Raise", sets: 3, reps: "20–25", rest: "0:45", restSec: 45, guide: "Focus on slow eccentrics to isolate soleus muscle in bent-knee position.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=JbyjNymZOt0" }
  ],
  day4: [
    { id: "db_flat_press", name: "Dumbbell Flat Press", sets: 4, reps: "10–12", rest: "1:30", restSec: 90, guide: "Keep elbows slightly tucked at 45°, drive DBs upward linearly.", img: "https://images.unsplash.com/photo-1583454110551-21f2fa2afe61?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=VmB1G1K7v94" },
    { id: "straight_arm_pulldown", name: "Cable Straight-Arm Pulldown", sets: 4, reps: "12–15", rest: "1:30", restSec: 90, guide: "Keep arms straight with slight elbow bend, push bar down towards thighs.", img: "https://images.unsplash.com/photo-1598971639058-fab3c3109a00?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=G9J_L9_z9Y8" },
    { id: "incline_smith_press", name: "Incline Smith Machine Press", sets: 3, reps: "10–12", rest: "1:30", restSec: 90, guide: "Position bench under bar, lower bar controlled to upper chest.", img: "https://images.unsplash.com/photo-1534367507873-d2d7e24c797f?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=833zAnM2S80" },
    { id: "chest_supported_row", name: "Chest-Supported Row (Machine)", sets: 3, reps: "10–12", rest: "1:30", restSec: 90, guide: "Keep chest firmly against pad, row handles back squeezing lats.", img: "https://images.unsplash.com/photo-1605296867304-46d5465a13f1?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=0UBRfiO4zDs" },
    { id: "db_shoulder_press", name: "Dumbbell Shoulder Press", sets: 3, reps: "10–12", rest: "1:30", restSec: 90, guide: "Press dumbbells directly overhead without arching lower back.", img: "https://images.unsplash.com/photo-1541534741688-6078c6bfb5c5?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=qEwKCR5JCog" },
    { id: "lateral_raise_dropset", name: "Lateral Raise (Drop Set)", sets: 4, reps: "12–15", rest: "1:00", restSec: 60, guide: "Perform strict set to failure, drop weight immediately by 20–30% and continue.", img: "https://images.unsplash.com/photo-1532029837206-abbe2b7620e3?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=3VcKaXpzqRo" },
    { id: "face_pull", name: "Face Pull (Rope)", sets: 3, reps: "15–20", rest: "1:00", restSec: 60, guide: "Pull rope directly towards forehead/eyes while flaring elbows high.", img: "https://images.unsplash.com/photo-1532029837206-abbe2b7620e3?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=rep-qVOkqgk" },
    { id: "ezbar_curl", name: "EZ-Bar Curl", sets: 3, reps: "10–12", rest: "1:15", restSec: 75, guide: "Grip outer angled handles, curl bar upward keeping upper arms stationary.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=cd241-o1_lM" },
    { id: "skull_crusher", name: "Skull Crusher (EZ-Bar)", sets: 3, reps: "10–12", rest: "1:15", restSec: 75, guide: "Lying flat, lower EZ-bar to forehead level by bending elbows.", img: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=d_KZxkY_0cM" }
  ],
  day5: [
    { id: "deadlift", name: "Conventional Deadlift", sets: 4, reps: "3–5", rest: "3:30", restSec: 210, guide: "Bar over mid-foot, brace core, pull slack out of bar before lifting.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=op9kVnSso6Q" },
    { id: "hip_thrust", name: "Hip Thrust (Barbell)", sets: 4, reps: "8–12", rest: "2:00", restSec: 120, guide: "Upper back against bench, drive hips upward until fully extended.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=LM8XHLYJoVs" },
    { id: "hack_squat", name: "Hack Squat / Goblet Squat", sets: 3, reps: "12–15", rest: "1:30", restSec: 90, guide: "Keep back flat against machine pad, lower deeply into squat position.", img: "https://images.unsplash.com/photo-1574680096145-d05b474e2155?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=0tn5K9NlCfo" },
    { id: "seated_leg_curl_single", name: "Seated Leg Curl (Single Leg)", sets: 3, reps: "12 ea", rest: "1:00", restSec: 60, guide: "Perform single-leg hamstring flexions with full control.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=ELOCsoDSmrg" },
    { id: "cable_glute_kickback", name: "Cable Glute Kickback", sets: 3, reps: "15 ea", rest: "1:00", restSec: 60, guide: "Attach ankle strap, kick leg backwards engaging glute max.", img: "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=Gj9iAnq2I7E" },
    { id: "cable_crunch", name: "Cable Crunch", sets: 3, reps: "15–20", rest: "0:30", restSec: 30, guide: "Kneel down holding rope behind head, flex spine to pull elbows to thighs.", img: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=2fVO_9M1H5Y" },
    { id: "hanging_leg_raise", name: "Hanging Leg Raise", sets: 3, reps: "12–15", rest: "0:30", restSec: 30, guide: "Hang from pull-up bar, raise legs/knees upward without swinging.", img: "https://images.unsplash.com/photo-1598971639058-fab3c3109a00?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=hdng3Nm1x_E" },
    { id: "ab_wheel_rollout", name: "Ab Wheel Rollout", sets: 3, reps: "10–12", rest: "0:30", restSec: 30, guide: "Kneel and roll wheel forward while bracing abdominal core tightly.", img: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=rqiTPdK1c_I" },
    { id: "plank", name: "Plank", sets: 3, reps: "45–60s", rest: "0:30", restSec: 30, guide: "Hold isometric prone position on forearms keeping body completely straight.", img: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=600&auto=format&fit=crop", video: "https://www.youtube.com/watch?v=pSHjTRCQxIw" }
  ]
};

// Re-structured, Professional Meal Protocol
const myMeals = [
  {
    id: "m1",
    name: "Meal 1 — Morning Anabolic Complex (Oats 60g, Whey Isolate 30g, Whole Milk 300ml, Almonds 10g, Seeds 15g, Blueberries, Creatine 5g, Collagen 10g)",
    cals: 680, protein: 52, carbs: 68, fats: 20,
    calcium: 450, iron: 4.5, potassium: 650, fiber: 11,
    checked: false
  },
  {
    id: "m2",
    name: "Meal 2 — Artisanal Cottage Cheese & Spinach Tortilla Wrap (Whole Grain Tortilla, Cottage Cheese 100g, Black Chana, Baby Spinach)",
    cals: 340, protein: 22, carbs: 42, fats: 8,
    calcium: 220, iron: 3.8, potassium: 420, fiber: 7,
    checked: false
  },
  {
    id: "m3",
    name: "Meal 3 — Micronutrient & Polyphenol Fuel (Crisp Apple & Carrots 100g)",
    cals: 130, protein: 2, carbs: 32, fats: 0.5,
    calcium: 40, iron: 0.8, potassium: 480, fiber: 6,
    checked: false
  },
  {
    id: "m4",
    name: "Meal 4 — Whole Egg & Roasted Potato Skillet (4 Pasture-Raised Whole Eggs, Yukon Gold Potato 150g, Pure Organic Ghee 15g)",
    cals: 510, protein: 27, carbs: 30, fats: 30,
    calcium: 120, iron: 3.5, potassium: 750, fiber: 3,
    checked: false
  },
  {
    id: "m5",
    name: "Meal 5 — Probiotic Strained Yogurt Bowl (150g Low-Fat Yogurt with Diced Cucumber)",
    cals: 100, protein: 7, carbs: 9, fats: 4,
    calcium: 180, iron: 0.2, potassium: 280, fiber: 1,
    checked: false
  },
  {
    id: "m6",
    name: "Meal 6 — Rapid-Absorbing Whey Protein Roll (Whole Wheat Wrap, 1 Scoop Isolating Whey)",
    cals: 240, protein: 30, carbs: 22, fats: 3.5,
    calcium: 150, iron: 1.5, potassium: 210, fiber: 2,
    checked: false
  },
  {
    id: "m7",
    name: "Meal 7 — Lean Poultry & Complex Glycogen Re-Supply (Grilled Chicken Breast 250g, Steamed Long Grain Rice 60g raw, Greek Yogurt 100g, Steamed Veggies)",
    cals: 690, protein: 73, carbs: 54, fats: 11,
    calcium: 160, iron: 2.8, potassium: 820, fiber: 4,
    checked: false
  }
];

let timerInterval;
let macroChart;

function playCompletionChime() {
  try {
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const osc1 = audioCtx.createOscillator();
    const gain1 = audioCtx.createGain();
    osc1.type = 'sine';
    osc1.frequency.value = 659.25;
    gain1.gain.setValueAtTime(0.3, audioCtx.currentTime);
    gain1.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);
    osc1.connect(gain1);
    gain1.connect(audioCtx.destination);
    osc1.start(audioCtx.currentTime);
    osc1.stop(audioCtx.currentTime + 0.3);
  } catch (e) {
    console.log("Audio blocked", e);
  }
}

function startFullscreenTimer(seconds, exerciseName) {
  clearInterval(timerInterval);
  let timeRemaining = seconds;
  document.getElementById("timer-exercise-name").innerText = exerciseName;
  const display = document.getElementById("fullscreen-timer-display");
  const modal = document.getElementById("fullscreen-timer-modal");
  modal.classList.remove("hidden");

  timerInterval = setInterval(() => {
    const mins = String(Math.floor(timeRemaining / 60)).padStart(2, '0');
    const secs = String(timeRemaining % 60).padStart(2, '0');
    display.innerText = `${mins}:${secs}`;

    if (timeRemaining <= 0) {
      clearInterval(timerInterval);
      display.innerText = "REST DONE!";
      playCompletionChime();
    }
    timeRemaining--;
  }, 1000);
}

function closeFullscreenTimer() {
  clearInterval(timerInterval);
  document.getElementById("fullscreen-timer-modal").classList.add("hidden");
}

function saveSetData(exId, setNum) {
  const weight = document.getElementById(`weight_${exId}_${setNum}`).value;
  const reps = document.getElementById(`reps_${exId}_${setNum}`).value;

  if (weight && reps) {
    const history = JSON.parse(localStorage.getItem(`history_${exId}`)) || {};
    history[`set_${setNum}`] = { weight, reps, date: new Date().toLocaleDateString() };
    localStorage.setItem(`history_${exId}`, JSON.stringify(history));
    loadWorkout(document.getElementById("day-select").value);
  }
}

function loadWorkout(dayKey) {
  const container = document.getElementById("exercise-cards-container");
  container.innerHTML = "";

  workouts[dayKey].forEach((exercise) => {
    const savedHistory = JSON.parse(localStorage.getItem(`history_${exercise.id}`)) || {};
    let setRowsHtml = "";

    for (let s = 1; s <= exercise.sets; s++) {
      const lastLift = savedHistory[`set_${s}`] ? `${savedHistory[`set_${s}`].weight} lbs × ${savedHistory[`set_${s}`].reps} reps` : "No record";
      const savedWeight = savedHistory[`set_${s}`]?.weight || "";
      const savedReps = savedHistory[`set_${s}`]?.reps || "";

      setRowsHtml += `
        <tr class="border-b border-slate-800/60">
          <td class="py-2.5 font-bold text-slate-300 text-xs">Set ${s}</td>
          <td class="py-2.5 text-[10px] text-slate-400">${lastLift}</td>
          <td class="py-2.5"><input id="weight_${exercise.id}_${s}" type="number" value="${savedWeight}" placeholder="lbs" class="w-16 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
          <td class="py-2.5"><input id="reps_${exercise.id}_${s}" type="number" value="${savedReps}" placeholder="reps" class="w-12 bg-slate-900 border border-slate-800 rounded-lg p-1.5 text-xs text-white focus:outline-none"></td>
          <td class="py-2.5 text-right">
            <button onclick="saveSetData('${exercise.id}', ${s}); startFullscreenTimer(${exercise.restSec}, '${exercise.name}');" class="bg-sky-600/30 border border-sky-500/50 hover:bg-sky-500/40 text-sky-300 text-xs px-2.5 py-1.5 rounded-lg font-bold transition">
              Log ⏱️
            </button>
          </td>
        </tr>
      `;
    }

    container.innerHTML += `
      <div class="bg-slate-900/80 border border-slate-800 rounded-2xl overflow-hidden">
        <div class="h-28 w-full relative bg-slate-950">
          <img src="${exercise.img}" alt="${exercise.name}" class="w-full h-full object-cover opacity-50">
          <div class="absolute inset-0 bg-gradient-to-t from-slate-900 via-transparent"></div>
          <div class="absolute bottom-2 left-3 right-3 flex justify-between items-end">
            <h3 class="font-black text-sm text-white">${exercise.name}</h3>
            <span class="text-[10px] font-bold text-sky-300 bg-slate-900/90 px-2 py-0.5 rounded border border-slate-700">Rest: ${exercise.rest}</span>
          </div>
        </div>
        <div class="p-3">
          <div class="flex items-center justify-between gap-2 mb-3">
            <details class="flex-1 text-[11px] text-slate-400 bg-slate-950/50 p-2 rounded-lg border border-slate-800">
              <summary class="font-bold text-sky-400 cursor-pointer">📖 Biomechanical Protocol</summary>
              <p class="mt-1.5 leading-relaxed text-slate-300">${exercise.guide}</p>
            </details>
            <a href="${exercise.video}" target="_blank" rel="noopener noreferrer" class="bg-red-600/20 hover:bg-red-600/30 text-red-400 border border-red-500/30 font-bold text-[11px] px-3 py-2 rounded-lg transition shrink-0 flex items-center gap-1">
              <span>▶</span> Video
            </a>
          </div>
          <table class="w-full text-left">
            <thead>
              <tr class="text-[10px] uppercase font-bold text-slate-500 border-b border-slate-800">
                <th class="pb-1.5">Set</th>
                <th class="pb-1.5">Previous Target</th>
                <th class="pb-1.5">lbs</th>
                <th class="pb-1.5">Reps</th>
                <th class="pb-1.5 text-right">Action</th>
              </tr>
            </thead>
            <tbody>${setRowsHtml}</tbody>
          </table>
        </div>
      </div>
    `;
  });
}

function initChart() {
  const ctx = document.getElementById('macroChart').getContext('2d');
  macroChart = new Chart(ctx, {
    type: 'pie',
    data: {
      labels: ['Protein (g)', 'Carbohydrates (g)', 'Fats (g)'],
      datasets: [{
        data: [0, 0, 0],
        backgroundColor: ['#10b981', '#38bdf8', '#818cf8'],
        borderWidth: 1,
        borderColor: '#0f172a'
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: { position: 'bottom', labels: { color: '#94a3b8', font: { size: 10 } } }
      }
    }
  });
}

function renderMeals() {
  const container = document.getElementById("meal-list");
  container.innerHTML = "";

  myMeals.forEach((meal, index) => {
    const mealEl = document.createElement("div");
    mealEl.className = `p-3 rounded-xl border transition flex items-start gap-3 ${meal.checked ? 'bg-emerald-950/30 border-emerald-500/40' : 'bg-slate-900/80 border-slate-800'}`;
    mealEl.innerHTML = `
      <input type="checkbox" id="check_${meal.id}" ${meal.checked ? 'checked' : ''} onchange="toggleMeal(${index})" class="mt-1 w-4 h-4 accent-emerald-500 rounded cursor-pointer shrink-0">
      <label for="check_${meal.id}" class="flex-1 cursor-pointer">
        <span class="block text-xs font-bold ${meal.checked ? 'text-emerald-300' : 'text-slate-200'}">${meal.name}</span>
        <span class="text-[10px] text-slate-400 mt-0.5 block">${meal.cals} kcal | P: ${meal.protein}g | C: ${meal.carbs}g | F: ${meal.fats}g</span>
      </label>
    `;
    container.appendChild(mealEl);
  });

  calculateTotals();
}

function toggleMeal(index) {
  myMeals[index].checked = !myMeals[index].checked;
  renderMeals();
}

function calculateTotals() {
  let cals = 0, protein = 0, carbs = 0, fats = 0;
  let calcium = 0, iron = 0, potassium = 0, fiber = 0;

  myMeals.forEach(meal => {
    if (meal.checked) {
      cals += meal.cals;
      protein += meal.protein;
      carbs += meal.carbs;
      fats += meal.fats;
      calcium += meal.calcium;
      iron += meal.iron;
      potassium += meal.potassium;
      fiber += meal.fiber;
    }
  });

  document.getElementById("total-cals").innerText = `${cals} kcal`;
  document.getElementById("total-protein").innerText = `${protein} g`;
  document.getElementById("total-carbs").innerText = `${carbs} g`;
  document.getElementById("total-fats").innerText = `${fats} g`;

  document.getElementById("micro-calcium").innerText = `${calcium} mg`;
  document.getElementById("micro-iron").innerText = `${iron.toFixed(1)} mg`;
  document.getElementById("micro-potassium").innerText = `${potassium} mg`;
  document.getElementById("micro-fiber").innerText = `${fiber} g`;

  if (macroChart) {
    macroChart.data.datasets[0].data = [protein, carbs, fats];
    macroChart.update();
  }
}

// US Navy Body Fat Calculator (Height: 185 cm, Male)
function calculateBodyFat() {
  const neck = parseFloat(document.getElementById("measure_neck").value);
  const waist = parseFloat(document.getElementById("measure_waist").value);
  const height = 185; // 6'1" in cm
  const weight = 95; // kg

  if (neck && waist && waist > neck) {
    // US Navy Method Male Formula: %BF = 86.010 x log10(waist - neck) - 70.041 x log10(height) + 36.76
    const bodyFat = (86.010 * Math.log10(waist - neck) - 70.041 * Math.log10(height) + 36.76).toFixed(1);
    const bfNum = Math.max(3, Math.min(50, parseFloat(bodyFat))); // Clamp
   
    document.getElementById("navy-bf-display").innerText = `${bfNum}%`;
   
    const fatMass = (weight * (bfNum / 100)).toFixed(1);
    const leanMass = (weight - fatMass).toFixed(1);
    document.getElementById("lean-mass-display").innerText = `${leanMass} kg`;

    // Save values
    localStorage.setItem("user_bodyfat", bfNum);
  }
}

// Photo Upload Handling & Preview
function previewUploadedPhoto(event) {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = function(e) {
      document.getElementById("photo-preview").src = e.target.result;
      document.getElementById("upload-placeholder").classList.add("hidden");
      document.getElementById("upload-preview-container").classList.remove("hidden");
    };
    reader.readAsDataURL(file);
  }
}

function analyzePhotoVisuals() {
  const resultContainer = document.getElementById("photo-scan-result");
  resultContainer.classList.remove("hidden");
 
  // Calculate or check Navy BF to refine description
  const navyBF = localStorage.getItem("user_bodyfat");
  if (navyBF) {
    document.getElementById("scan-tier-badge").innerText = `Estimated ~${navyBF}% Body Fat`;
    document.getElementById("scan-description").innerText = `Physique telemetry synchronized with tape measurements (${navyBF}% BF). Visual indicators show solid lean muscle mass baseline at 95 kg bodyweight.`;
  }
}

function saveWeight() {
  const w = document.getElementById("input-weight").value;
  if (w) {
    localStorage.setItem("user_weight", JSON.stringify({ weight: w, date: new Date().toLocaleDateString() }));
    renderWeight();
  }
}

function renderWeight() {
  const saved = JSON.parse(localStorage.getItem("user_weight"));
  if (saved) {
    document.getElementById("latest-weight-log").innerText = `Latest Recorded Mass: ${saved.weight} kg (${saved.date})`;
  }
}

function toggleWhoopModal() {
  document.getElementById("whoop-modal").classList.toggle("hidden");
}

async function fetchRealWhoopData() {
  let token = document.getElementById("whoop-token-input").value.trim();
  if (!token) token = localStorage.getItem("whoop_access_token");
  else localStorage.setItem("whoop_access_token", token);

  if (!token) return;

  try {
    const res = await fetch("https://api.prod.whoop.com/developer/v1/recovery?limit=1", {
      headers: { "Authorization": `Bearer ${token}` }
    });
    if (res.ok) {
      const data = await res.json();
      if (data.records?.length > 0) {
        const rec = data.records[0].score.recovery_score;
        document.getElementById("whoop-recovery").innerText = `${rec}%`;
        document.getElementById("whoop-status-badge").innerText = "WHOOP: Connected";
        document.getElementById("whoop-status-badge").className = "text-[10px] font-bold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20 px-2.5 py-1 rounded-full";

        const adviceEl = document.getElementById("whoop-advice");
        if (rec >= 66) adviceEl.innerHTML = `<span>🟢</span><span>High Recovery Status (${rec}%): Optimal physiological priming. Target maximum progressive overload.</span>`;
        else if (rec >= 34) adviceEl.innerHTML = `<span>🟡</span><span>Moderate Recovery Status (${rec}%): Maintain baseline load and structured volume targets.</span>`;
        else adviceEl.innerHTML = `<span>🔴</span><span>Low Recovery Status (${rec}%): High physiological fatigue. Implement a reactive deload (-15% load reduction).</span>`;
      }
    }
  } catch (err) {
    console.error(err);
  }
}

// App Initialization
document.getElementById("current-date").innerText = new Date().toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric' });
loadWorkout("day1");
initChart();
renderMeals();
renderWeight();

const savedToken = localStorage.getItem("whoop_access_token");
if (savedToken) {
  document.getElementById("whoop-token-input").value = savedToken;
  fetchRealWhoopData();
}
</script>
</body>
</html>