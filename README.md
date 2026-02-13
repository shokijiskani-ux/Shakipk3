<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SahkiPKR - Quiz Game</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:Arial;background:linear-gradient(135deg,#667eea,#764ba2);min-height:100vh;display:flex;justify-content:center;align-items:center;padding:20px}
.container{background:white;border-radius:15px;box-shadow:0 10px 40px rgba(0,0,0,0.3);width:100%;max-width:700px;padding:40px}
h1{color:#667eea;text-align:center}
.info{display:flex;justify-content:space-around;margin:20px 0;flex-wrap:wrap}
.box{background:#f0f0f0;padding:15px;border-radius:5px;text-align:center;margin:5px}
.box h3{color:#667eea;margin:0}
.box p{font-size:1.5em;font-weight:bold;margin:5px 0}
.question{background:#f9f9f9;padding:20px;border-left:4px solid #667eea;margin:20px 0;font-size:1.1em}
.options{list-style:none;padding:0}
.option{background:#f5f5f5;padding:15px;margin:10px 0;border-radius:5px;cursor:pointer}
.option:hover{background:#e8e8e8}
.option.active{background:#667eea;color:white}
button{width:100%;padding:12px;background:#667eea;color:white;border:none;border-radius:5px;cursor:pointer;font-size:1em;margin-top:20px}
button:hover{background:#5568d3}
button:disabled{background:#ccc}
.hidden{display:none}
.results{text-align:center}
.results h2{color:#667eea;margin:20px 0}
.result-score{font-size:2.5em;color:#667eea;font-weight:bold;margin:20px 0}
</style>
</head>
<body>
<div class="container">
<h1>🌍 SahkiPKR Quiz</h1>

<div id="start">
<h2 style="text-align:center;color:#667eea">خوش آمدید!</h2>
<p style="text-align:center;margin:20px 0">World Knowledge Quiz Game</p>
<p style="text-align:center;color:#999">50+ سوالات - اردو & انگریزی</p>
<button onclick="startGame()">شروع کریں / Start</button>
</div>

<div id="quiz" class="hidden">
<div class="info">
<div class="box"><h3>سکور</h3><p id="score">0</p></div>
<div class="box"><h3>وقت</h3><p id="time">30</p></div>
<div class="box"><h3>سوال</h3><p id="qcount">1/50</p></div>
</div>
<div class="question" id="question">Question?</div>
<ul class="options" id="options"></ul>
<button id="nextBtn" onclick="nextQuestion()" disabled>اگلا / Next</button>
</div>

<div id="result" class="results hidden">
<h2>مبارک ہو!</h2>
<div class="result-score" id="final">0/50</div>
<p id="msg" style="font-size:1.2em;margin:20px 0"></p>
<button onclick="location.reload()">دوبارہ / Play Again</button>
</div>
</div>

<script>
const questions=[
{q:"فرانس کا دارالحکومت؟",o:["Berlin","Paris","Rome","Madrid"],a:1},
{q:"جاپان کا دارالحکومت؟",o:["Tokyo","Osaka","Kyoto","Yokohama"],a:0},
{q:"برازیل کا دارالحکومت؟",o:["Rio","Brasília","São Paulo","Salvador"],a:1},
{q:"مصر کا دارالحکومت؟",o:["Alexandria","Cairo","Giza","Luxor"],a:1},
{q:"آسٹریلیا کا دارالحکومت؟",o:["Sydney","Melbourne","Canberra","Brisbane"],a:2},
{q:"کینیڈا کا دارالحکومت؟",o:["Toronto","Vancouver","Ottawa","Montreal"],a:2},
{q:"انڈیا کا دارالحکومت؟",o:["Mumbai","Delhi","Bangalore","Hyderabad"],a:1},
{q:"روس کا دارالحکومت؟",o:["St. Petersburg","Moscow","Novosibirsk","Yekaterinburg"],a:1},
{q:"جنوبی افریقہ کا دارالحکومت؟",o:["Johannesburg","Cape Town","Pretoria","Durban"],a:2},
{q:"چین کا دارالحکومت؟",o:["Shanghai","Beijing","Guangzhou","Chongqing"],a:1},
{q:"دنیا میں براعظم کتنے ہیں؟",o:["5","6","7","8"],a:2},
{q:"سب سے بڑا محیط؟",o:["Atlantic","Indian","Arctic","Pacific"],a:3},
{q:"سب سے لمبا دریا؟",o:["Amazon","Nile","Yangtze","Mississippi"],a:1},
{q:"ماؤنٹ ایورسٹ کی اونچائی؟",o:["8448m","8649m","8849m","8949m"],a:2},
{q:"سب سے بڑا صحرا؟",o:["Kalahari","Sahara","Arabian","Gobi"],a:1},
{q:"سب سے بڑا ملک (رقبہ میں)؟",o:["Canada","Russia","USA","China"],a:1},
{q:"سب سے آبادی والا ملک؟",o:["India","China","USA","Indonesia"],a:0},
{q:"پاکستان میں صوبے کتنے؟",o:["3","4","5","6"],a:1},
{q:"سب سے تیز جانور؟",o:["Falcon","Cheetah","Peregrine","Lion"],a:2},
{q:"سب سے بڑا جانور؟",o:["Elephant","Giraffe","Blue Whale","Hippo"],a:2},
{q:"موناِلزا کے مصنف؟",o:["Michelangelo","Leonardo da Vinci","Raphael","Donatello"],a:1},
{q:"سسٹین چپل کے مصنف؟",o:["Leonardo","Michelangelo","Raphael","Botticelli"],a:1},
{q:"تاج محل کسے بنایا گیا؟",o:["Nur Jahan","Mumtaz Mahal","Daughter","Jahangir"],a:1},
{q:"بگ بین کہاں ہے؟",o:["Paris","Rome","London","Berlin"],a:2},
{q:"ایفل ٹاور کہاں ہے؟",o:["London","Rome","Paris","Berlin"],a:2},
{q:"کولسیم کہاں ہے؟",o:["Greece","Rome","Egypt","Turkey"],a:1},
{q:"سٹیچو آف لبرٹی کہاں ہے؟",o:["Boston","Washington","New York","Philadelphia"],a:2},
{q:"انگریزی میں حروف کتنے؟",o:["24","25","26","27"],a:2},
{q:"عربی میں حروف کتنے؟",o:["26","28","30","32"],a:1},
{q:"اردو میں حروف کتنے؟",o:["36","37","38","39"],a:2},
{q:"ہفتے میں دن کتنے؟",o:["5","6","7","8"],a:2},
{q:"سال میں مہینے کتنے؟",o:["10","11","12","13"],a:2},
{q:"سال میں دن کتنے؟",o:["360","365","366","370"],a:1},
{q:"ورلڈ کپ کتنے سال میں؟",o:["2","3","4","5"],a:2},
{q:"اولمپک کتنے سال میں؟",o:["2","3","4","5"],a:2},
{q:"کرکٹ میں کھلاڑی کتنے؟",o:["9","10","11","12"],a:2},
{q:"فٹ بال میں کھلاڑی کتنے؟",o:["10","11","12","13"],a:1},
{q:"H2O کیا ہے؟",o:["Salt","Water","Sugar","Acid"],a:1},
{q:"خون میں لال رنگ کیوں؟",o:["Iron","Copper","Zinc","Magnesium"],a:0},
{q:"روشنی کی رفتار؟",o:["3×10^7","3×10^8","3×10^9","3×10^10"],a:1},
{q:"انسان کی ہڈیاں کتنی؟",o:["186","206","226","246"],a:1},
{q:"سب سے بڑا عضو کون سا؟",o:["Heart","Brain","Skin","Liver"],a:2},
{q:"گریز دریافت کنندہ؟",o:["Einstein","Newton","Galileo","Kepler"],a:1},
{q:"الیکٹرن دریافت کنندہ؟",o:["Rutherford","JJ Thomson","Bohr","Planck"],a:1},
{q:"DNA کی شکل؟",o:["Circle","Square","Double Helix","Triangle"],a:2},
{q:"سب سے سخت معدن؟",o:["Gold","Diamond","Ruby","Sapphire"],a:1},
{q:"امریکہ کی آزادی کب؟",o:["1773","1775","1776","1778"],a:2},
{q:"پاکستان کی آزادی کب؟",o:["1946","1947","1948","1949"],a:1},
{q:"برلن دیوار کب گری؟",o:["1987","1988","1989","1990"],a:2},
{q:"تاج محل کب بنایا گیا؟",o:["1623","1632","1643","1653"],a:1}
];

let current=0,score=0,selected=-1,timer;

function startGame(){
document.getElementById('start').classList.add('hidden');
document.getElementById('quiz').classList.remove('hidden');
showQuestion();
startTimer();
}

function showQuestion(){
if(current>=questions.length){endGame();return;}
let q=questions[current];
document.getElementById('question').innerHTML=q.q;
document.getElementById('qcount').innerHTML=(current+1)+'/'+questions.length;
let opts=document.getElementById('options');
opts.innerHTML='';
selected=-1;
document.getElementById('nextBtn').disabled=true;
q.o.forEach((o,i)=>{
let li=document.createElement('li');
li.className='option';
li.innerHTML=o;
li.onclick=()=>{selected=i;document.querySelectorAll('.option').forEach(el=>el.classList.remove('active'));li.classList.add('active');document.getElementById('nextBtn').disabled=false;};
opts.appendChild(li);
});
}

function startTimer(){
let t=30;
document.getElementById('time').innerHTML=t;
timer=setInterval(()=>{
t--;
document.getElementById('time').innerHTML=t;
if(t<=0){clearInterval(timer);nextQuestion();}
},1000);
}

function nextQuestion(){
clearInterval(timer);
if(selected===questions[current].a){score++;}
document.getElementById('score').innerHTML=score;
current++;
if(current<questions.length){showQuestion();startTimer();}else{endGame();}
}

function endGame(){
clearInterval(timer);
document.getElementById('quiz').classList.add('hidden');
document.getElementById('result').classList.remove('hidden');
let percent=Math.round((score/questions.length)*100);
document.getElementById('final').innerHTML=score+'/'+questions.length;
if(percent>=80){document.getElementById('msg').innerHTML='بہترین! شاندار کارکردگی! 🎉';}
else if(percent>=60){document.getElementById('msg').innerHTML='اچھا! اور بہتری کر سکتے ہو! 👍';}
else if(percent>=40){document.getElementById('msg').innerHTML='ٹھیک ہے! مزید کوشش کریں! 💪';}
else{document.getElementById('msg').innerHTML='شروع تو ہو گیا! دوبارہ کوشش کریں! 📚';}
}
</script>
</body>
</html>
