<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>競馬期待値ツール v3（完全UI版）</title>
</head>

<body style="font-family:sans-serif;max-width:800px;margin:auto;padding:20px;">

<h2>競馬期待値ツール v3（完全UIベース）</h2>

<hr>

<h3>基本情報</h3>

<label>単勝人気</label><br>
<select id="rank" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<script>
for(let i=1;i<=18;i++){
document.write(`<option value="${i}">${i}人気</option>`);
}
</script>
</select>

<br><br>

<label>脚質</label><br>
<select id="style" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<option value="逃げ">逃げ</option>
<option value="先行">先行</option>
<option value="差し">差し</option>
<option value="追込">追込</option>
</select>

<br><br>

<label>騎手</label><br>
<select id="jockey" style="width:100%;padding:10px;">
<option value="">--選択--</option>

<option value="sakai">坂井瑠星</option>
<option value="tokei">戸崎圭太</option>
<option value="lemaire">ルメール</option>
<option value="kawada">川田将雅</option>
<option value="take">武豊</option>

<option value="sugawara">菅原明良</option>
<option value="iwata_y">岩田康誠</option>
<option value="iwata_mi">岩田望来</option>

<option value="moreira">モレイラ</option>
<option value="matsuyama">松山弘平</option>
<option value="yokoyama_t">横山武史</option>

<option value="demuro">デムーロ</option>
<option value="yutaka_other">その他</option>

</select>

<br><br>

<label>距離</label><br>
<select id="dist" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<option value="short">短距離（1000〜1500）</option>
<option value="mid">中距離（1600〜2200）</option>
<option value="long">長距離（2400以上）</option>
</select>

<br><br>

<label>馬場</label><br>
<select id="surface" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<option value="芝">芝</option>
<option value="ダート">ダート</option>
</select>

<br><br>

<label>クラス</label><br>
<select id="race_class" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<option value="平場">平場</option>
<option value="OP">OP</option>
<option value="G23">G2・G3</option>
<option value="G1">G1</option>
</select>

<hr>

<h3>前走情報</h3>

<label>前走人気</label><br>
<select id="prev_rank" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<script>
for(let i=1;i<=18;i++){
document.write(`<option value="${i}">${i}人気</option>`);
}
</script>
</select>

<br><br>

<label>前走着順</label><br>
<select id="prev_order" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<script>
for(let i=1;i<=18;i++){
if(i<=10){
document.write(`<option value="${i}">${i}着</option>`);
}
}
document.write(`<option value="10">10着以上</option>`);
</script>
</select>

<br><br>

<label>前走着差</label><br>
<select id="prev_diff" style="width:100%;padding:10px;">
<option value="">--選択--</option>
<option value="0.1">僅差（〜0.1）</option>
<option value="0.5">0.2〜0.5差</option>
<option value="1.0">1秒以上負け</option>
</select>

<hr>

<h3>オッズ</h3>

<label>単勝オッズ</label><br>
<input id="win_odds" type="number" style="width:100%;padding:10px;">

<br><br>

<label>複勝オッズ</label><br>
<input id="pl_odds" type="number" style="width:100%;padding:10px;">

<br><br>

<button id="btn" style="width:100%;padding:15px;font-size:20px;">
判定する
</button>

<hr>

<div id="result" style="font-size:28px;font-weight:bold;text-align:center;margin-top:20px;"></div>

<script>

// =========================
// STEP1（最低ロジック）
// =========================

document.getElementById("btn").onclick = function(){

let score = 100;

let r = document.getElementById("rank").value;
let s = document.getElementById("style").value;
let j = document.getElementById("jockey").value;

let pr = document.getElementById("prev_rank").value;
let po = document.getElementById("prev_order").value;
let pd = document.getElementById("prev_diff").value;

let w = document.getElementById("win_odds").value;
let p = document.getElementById("pl_odds").value;

// 人気
if(r == "5") score += 20;

// 脚質
if(s == "逃げ") score += 10;

 // =========================
 // 騎手補正（フル版）
 // =========================

 if(j == "lemaire"){
     score += 18;
     if(r <= 5) score += 10;
     if(r >= 6) score -= 5;
 }

 else if(j == "kawada"){
     score += 15;
     if(s == "先行") score += 10;
 }

 else if(j == "take"){
     score += 12;
     if(s == "逃げ") score += 12;
     if(document.getElementById("dist").value == "long") score += 10;
 }

 else if(j == "sakai"){
     score += 10;
     if(r >= 4 && r <= 8) score += 8;
 }

 else if(j == "tokei"){
     score += 10;
     if(s == "差し") score += 8;
 }

 else if(j == "sugawara"){
     score += 8;
     if(s == "差し" || s == "追込") score += 10;
 }

 else if(j == "iwata_y"){
     score += 10;
     if(r <= 5) score += 8;
 }

 else if(j == "iwata_mi"){
     score += 8;
     if(r >= 6) score += 10;
 }

 else if(j == "moreira"){
     score += 20;
 }

 else if(j == "matsuyama"){
     score += 9;
 }

 else if(j == "yokoyama_t"){
     score += 11;
     if(s == "先行") score += 6;
 }

 else if(j == "demuro"){
     score += 9;
 }

 // =========================
 // 前走ロジック（完全版）
 // =========================

 let prevScore = 0;

 // 前走人気（能力評価）
 if(pr == "1"){
     prevScore += 12;
 } else if(pr == "2"){
     prevScore += 9;
 } else if(pr == "3"){
     prevScore += 6;
 } else if(pr >= "4

// オッズ（仮）
if(w && w > 10) score += 10;

let text = "";

if(score >= 220){
text = "🔥 激アツ期待値馬";
}else if(score >= 180){
text = "◎ 高期待値馬";
}else if(score >= 150){
text = "○ 相手候補";
}else{
text = "✕ 見送り";
}

document.getElementById("result").innerHTML = text;

};

</script>

</body>
</html>
