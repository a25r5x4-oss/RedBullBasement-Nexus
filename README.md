# RedBullBasement-Nexus
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Future Quest | Red Bull × Nexus</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root {
    --rb-navy:#000b49;
    --rb-red:#ed1c24;
    --rb-yellow:#ffcc00;
    --rb-silver:#f1f1f1;
}

body{
    font-family:Arial;
    background: linear-gradient(135deg,#000b49,#1a1a1a);
    color:white;
    margin:0;
}

header{
    text-align:center;
    padding:20px;
}

.logo-area img{
    height:60px;
    margin:10px;
}

.container{
    max-width:700px;
    margin:auto;
    padding:20px;
}

section{
    background:white;
    color:black;
    padding:20px;
    border-radius:15px;
    margin-bottom:20px;
}

h2{
    color:var(--rb-red);
}

button{
    background:var(--rb-red);
    color:white;
    padding:12px;
    border:none;
    border-radius:20px;
    width:100%;
    margin-top:10px;
}

.energy-counter{
    background:var(--rb-yellow);
    color:black;
    padding:10px;
    text-align:center;
    font-weight:bold;
}

canvas{
    margin-top:20px;
}

/* エフェクト */
.glow{
    box-shadow:0 0 15px var(--rb-red);
}
</style>
</head>

<body>

<header>
    <h1>My Future Quest</h1>

    <!-- ロゴ追加 -->
    <div class="logo-area">
        <img src="https://upload.wikimedia.org/wikipedia/en/thumb/f/f5/Red_Bull_Basement_logo.png/320px-Red_Bull_Basement_logo.png">
        <img src="YOUR_NEXUS_LOGO_URL">
    </div>

    <p>Energy Your Future</p>
</header>

<div class="container">

<!-- Step1 -->
<section>
<h2>Step1: Character Creation</h2>

<label>没頭すること</label>
<input id="habit">

<label>ステータス</label>

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
<h2>Step2: AI Alchemy</h2>

<label>課題</label>
<input id="issue">

<label>強み</label>
<input id="strength">

<button onclick="generatePrompt()">生成</button>

<div id="promptBox"></div>

<button onclick="copyPrompt()">コピー</button>

<!-- ChatGPTリンク -->
<a href="https://chat.openai.com" target="_blank">
<button>ChatGPTを開く</button>
</a>

</section>

<!-- Workshop2 -->
<div class="energy-counter">
残エネルギー: <span id="energy">100</span>
</div>

<section>
<h2>University Quest</h2>

<label>挑戦したいこと</label>
<input id="customQuest">

<label>必要エネルギー</label>
<input type="number" id="customEnergy" oninput="calcEnergy()">

</section>

<!-- 最終まとめ -->
<section>
<h2>Final Summary</h2>
<button onclick="generateSummary()">ワークシートを可視化</button>
<div id="summary"></div>
</section>

</div>

<script>

// レーダーチャート
let chart = new Chart(document.getElementById("radarChart"),{
type:"radar",
data:{
labels:["行動","コミュ","創造","分析","挑戦","継続"],
datasets:[{
label:"あなたの能力",
data:[5,5,5,5,5,5],
backgroundColor:"rgba(237,28,36,0.3)",
borderColor:"red"
}]
}
});

function updateChart(){
chart.data.datasets[0].data=[
s1.value,s2.value,s3.value,s4.value,s5.value,s6.value
];
chart.update();
}

// プロンプト生成
function generatePrompt(){
let text=`あなたの強み「${strength.value}」と課題「${issue.value}」からビジネス案を3つ提案せよ`;
document.getElementById("promptBox").innerText=text;
}

// コピー
function copyPrompt(){
navigator.clipboard.writeText(promptBox.innerText);
alert("コピー完了！");
}

// エネルギー
function calcEnergy(){
let used = parseInt(customEnergy.value)||0;
document.getElementById("energy").innerText=100-used;
}

// 最終まとめ
function generateSummary(){
document.getElementById("summary").innerHTML = `
<h3>あなたの未来設計</h3>
<p>没頭: ${habit.value}</p>
<p>課題: ${issue.value}</p>
<p>強み: ${strength.value}</p>
<p>挑戦: ${customQuest.value}</p>
`;
}

</script>

</body>
</html>
