# Work
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Sort It!</title>
<style>
body{background:#e7f4e8;color:#183238;font-family:"微软雅黑",Arial;padding-top:25px}
#hezi{background:#fff;width:680px;margin:0 auto;padding:15px 10px 25px 10px;border-radius:10px}
#kapian{width:200px;height:120px;margin:18px auto;padding-top:25px;border:3px dashed #649b72;background:#fffdf5;font-size:18px}
#kapian span{display:block;font-size:50px;margin-bottom:5px}
.lajitong{width:160px;height:110px;margin:5px;border:0;color:#fff;font-size:15px;font-weight:bold;cursor:pointer}
.lajitong span{display:block;font-size:35px;margin-bottom:3px}
#chonglai{margin-top:20px;padding:6px 14px;border:0;background:#f7bf4f;font-weight:bold;cursor:pointer}
h1{margin:10px 0}
.btn2{box-shadow:0 2px 6px #999}
</style>
</head>
<body>
<center>
<div id="hezi">
<h1>Sort It!</h1>
<p>Drag the item to a bin, or click a bin to choose it.</p >
<h2 id="fenshu">Score: 0 / 6</h2>

<div id="kapian"></div>

<p id="tishi"></p >

<button class="lajitong" style="background:#3f8157" onclick="panduan('organic')" ondragover="allowDrop(event)" ondrop="drop(event,'organic')"><span>🍂</span>Organics</button>

<button class="lajitong" style="background:#2879ad" onclick="panduan('recycling')" ondragover="allowDrop(event)" ondrop="drop(event,'recycling')"><span>♻️</span>Recycling</button>

<button class="lajitong" style="background:#5c5c5c" onclick="panduan('landfill')" ondragover="allowDrop(event)" ondrop="drop(event,'landfill')"><span>🗑️</span>Landfill</button>

<br><br>
<button id="chonglai" onclick="chonglai()">Restart game</button>
</div>
</center>

<script>
//三个数组分别存名字、图片、类型，下标一样的就是同一个垃圾
var names = ["Banana peel", "Apple core", "Aluminium can", "Plastic bottle", "Broken mug", "Chip packet"];
var icons = ["🍌", "🍎", "🥫", "🧴", "☕", "🥔"];
var types = ["organic", "organic", "recycling", "recycling", "landfill", "landfill"];

var num = 0;
var score = 0;
var keyi = true;

//原来是一个一个写 if(num==0)... 太长了，改成数组
//var a1 = "Banana peel";
//var a2 = "Apple core";

xianshi();

function xianshi() {
    var ka = document.getElementById("kapian");
    if (num == names.length) {
        ka.innerHTML = "<span>🏆</span>Game complete!";
        ka.draggable = false;
        document.getElementById("tishi").innerHTML = "Press Restart game to play again.";
        return;
    }
    ka.innerHTML = "<span>" + icons[num] + "</span>" + names[num];
    ka.draggable = true;
    document.getElementById("tishi").innerHTML = "Choose the correct bin.";
    keyi = true;
}

function panduan(t) {
    if (keyi == false) {
        return;
    }
    if (num == names.length) {
        return;
    }
    if (t == types[num]) {
        keyi = false;
        score = score + 1;
        document.getElementById("fenshu").innerHTML = "Score: " + score + " / 6";
        document.getElementById("tishi").innerHTML = "Correct!";
        setTimeout(function() {
            num = num + 1;
            xianshi();
        }, 700);
    } else {
        document.getElementById("tishi").innerHTML = "Not quite. Try again.";
    }
}

//这两个是拖拽要用的，不写的话东西放不进去
function allowDrop(ev) {
    ev.preventDefault();
}

function drop(ev, t) {
    ev.preventDefault();
    panduan(t);
}

function chonglai() {
    location.reload();
}
</script>

</body>
</html>    showItem();
  </script>
</body>
</html>
