<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>競馬ツール v2</title>
</head>

<body style="font-family:sans-serif;padding:20px;max-width:700px;margin:auto;">

<h2>競馬期待値ツール v2（STEP1）</h2>

<label>人気</label><br>
<select id="rank" style="width:100%;padding:10px;">
  <option value="">--選択--</option>
  <option value="1">1人気</option>
  <option value="5">5人気</option>
  <option value="8">8人気</option>
</select>

<br><br>

<label>脚質</label><br>
<select id="style" style="width:100%;padding:10px;">
  <option value="">--選択--</option>
  <option value="逃げ">逃げ</option>
  <option value="先行">先行</option>
  <option value="差し">差し</option>
</select>

<br><br>

<label>騎手</label><br>
<select id="jockey" style="width:100%;padding:10px;">
  <option value="">--選択--</option>
  <option value="take">武豊</option>
  <option value="iwata">岩田康</option>
  <option value="other">その他</option>
</select>

<br><br>

<button id="btn" style="width:100%;padding:15px;font-size:18px;">
判定
</button>

<hr>

<div id="result" style="font-size:30px;font-weight:bold;text-align:center;margin-top:20px;"></div>

<script>

document.getElementById("btn").onclick = function(){

    var r = document.getElementById("rank").value;
    var s = document.getElementById("style").value;
    var j = document.getElementById("jockey").value;

    var score = 100;

    if(r == "5") score += 100;
    if(s == "逃げ") score += 30;
    if(j == "take") score += 50;

    var text = "";

    if(score >= 220){
        text = "🔥 激アツ期待値馬";
    }else if(score >= 180){
        text = "◎ 高期待値馬";
    }else{
        text = "✕ 見送り";
    }

    document.getElementById("result").innerHTML = text;
};

</script>

</body>
</html>
