# RedBullBasement-Nexus
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Future Quest | Red Bull × Nexus</title>

<!-- Chart.js -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root {
    --rb-navy:#000b49;
    --rb-red:#ed1c24;
    --rb-yellow:#ffcc00;
    --rb-silver:#f1f1f1;
}

body {
    font-family: 'Helvetica Neue', Arial;
    background-color: var(--rb-silver);
    margin: 0;
}

header {
    background: var(--rb-navy);
    color: white;
    padding: 20px;
    text-align: center;
    border-bottom: 5px solid var(--rb-red);
}

.container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
}

section {
    background: white;
    border-radius: 15px;
    padding: 20px;
    margin-bottom: 20px;
}

h2 {
    color: var(--rb-red);
    border-left: 5px solid var(--rb-yellow);
    padding-left: 10px;
}

label {
    display: block;
    margin-top: 10px;
}

input, textarea, select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
}

button {
    background: var(--rb-red);
    color: white;
    border: none;
    padding: 12px;
    border-radius: 20px;
    margin-top: 10px;
    width: 100%;
    cursor: pointer;
}

button:hover {
    background: var(--rb-navy);
}

.energy-counter {
    background: var(--rb-yellow);
    padding: 10px;
    text-align: center;
    font-weight: bold;
}

.prompt-box {
    background: #eee;
    padding: 10px;
    margin-top: 10px;
    border-radius: 10px;
}
</style>
</head>

<body>

<header>
<h1>My Future Quest</h1>
<p>Red Bull × Nexus Workshop</p>
</header>

<div class="container">

<!-- Step1 -->
<section>
<h2>Step 1: Character Creation</h2>

<label>没頭してしまうこと</label>
<input type="text" id="habit">

<label>ステータス診断</label>

行動力<input type="range" id="s1" min="1" max="10" value="5" oninput="updateChart()">
コミュ力<input type="range" id="s2" min="1" max="10" value="5" oninput="updateChart()">
創造性<input type="range" id="s3" min="1" max="10" value="5" oninput="updateChart()">
分析力<input type="range" id="s4" min="1" max="10" value="5" oninput="updateChart()">
挑戦心<input type="range" id="s5" min="1" max="10" value="5" oninput="updateChart()">
継続力<input type="range" id="s6" min="1" max="10" value="5" oninput="updateChart()">

<canvas id="radarChart"></canvas>

</section>

<!-- Step2 -->
<section>
<h2>Step 2 & 3: AI Alchemy</h2>

<label>社会課題</label>
<input type="text" id="issue_detail">

<label>強み</label>
<input type="text" id="strength">

<button onclick="generatePrompt()">AIプロンプト生成</button>

<div id="prompt_result" class="prompt-box" style="display:none;">
あなたは、世界中のスタートアップに精通した起業家です。

下の私の強みと社会課題を組み合わせて、ビジネスアイディアを3つ提案してください。

“自分の強み”
<span id="p_str"></span>

“社会課題”
<span id="p_issue"></span>

提案の条件
1.タイトル
面白いキャッチーなタイトルにしてください

2.概要
誰の悩みをどう解決するかを簡潔に教えて

3.マネタイズ（どこでお金が発生するのか）

4.独自性、差別化

これらの条件も満たすビジネスアイディアを作成してください。
</div>

<button id="copy_btn" style="display:none;" onclick="copyPrompt()">コピー</button>

<a href="https://chat.openai.com" target="_blank">
<button>ChatGPTで実行する</button>
</a>

</section>

<!-- Step4 -->
<section>
<h2>Step 4: Evolution</h2>

<label>ターゲット</label>
<input type="text">

<label>解決する問題</label>
<textarea></textarea>

</section>

<!-- Energy -->
<div class="energy-counter">
残りエネルギー: <span id="energy_left">100</span> pt
</div>

<!-- Workshop2 -->
<section>
<h2>University Quest</h2>

<div>
<input type="checkbox" class="quest" data-pt="80" onchange="calcEnergy()"> ビジコン (80)
</div>
<div>
<input type="checkbox" class="quest" data-pt="70" onchange="calcEnergy()"> 海外 (70)
</div>
<div>
<input type="checkbox" class="quest" data-pt="30" onchange="calcEnergy()"> サークル (30)
</div>

<h3>自分でクエスト作成</h3>

<label>挑戦したいこと</label>
<input type="text" id="custom_quest">

<label>エネルギー</label>
<input type="number" id="custom_pt" value="0" oninput="calcEnergy()">

</section>

<!-- Summary -->
<section>
<h2>Final Summary</h2>

<button onclick="generateSummary()">ワークを可視化</button>

<div id="summary"></div>

</section>

</div>

<script>

// Chart
let chart;

function initChart(){
    chart = new Chart(document.getElementById('radarChart'), {
        type: 'radar',
        data: {
            labels: ['行動','コミュ','創造','分析','挑戦','継続'],
            datasets: [{
                data: [5,5,5,5,5,5],
                backgroundColor:'rgba(237,28,36,0.2)',
                borderColor:'#ed1c24'
            }]
        }
    });
}

function updateChart(){
    chart.data.datasets[0].data = [
        s1.value, s2.value, s3.value, s4.value, s5.value, s6.value
    ];
    chart.update();
}

window.onload = initChart;

// Prompt
function generatePrompt(){
    p_str.innerText = strength.value;
    p_issue.innerText = issue_detail.value;
    prompt_result.style.display = "block";
    copy_btn.style.display = "block";
}

// Copy
function copyPrompt(){
    navigator.clipboard.writeText(prompt_result.innerText);
    alert("コピーしました！");
}

// Energy
function calcEnergy(){
    let total = 0;

    document.querySelectorAll('.quest:checked').forEach(el=>{
        total += parseInt(el.dataset.pt);
    });

    total += parseInt(custom_pt.value) || 0;

    energy_left.innerText = 100 - total;
}

// Summary
function generateSummary(){
    summary.innerHTML = `
    <h3>あなたの未来</h3>
    <p>没頭: ${habit.value}</p>
    <p>課題: ${issue_detail.value}</p>
    <p>強み: ${strength.value}</p>
    <p>挑戦: ${custom_quest.value}</p>
    `;
}

</script>

</body>
</html>
