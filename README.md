# IQplot

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>IQ Predictor - Visual Test</title>

<style>
* {
  box-sizing: border-box;
  font-family: 'Segoe UI', Arial, sans-serif;
}

body {
  margin: 0;
  min-height: 100vh;
  background: #080f20;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
}

.app {
  width: 100%;
  max-width: 500px;
  background: #17243a;
  border: 1px solid #334155;
  border-radius: 22px;
  padding: 22px;
  box-shadow: 0 15px 45px #0008;
  animation: appear .5s ease;
}

@keyframes appear {
  from {opacity:0; transform:translateY(20px)}
  to {opacity:1; transform:translateY(0)}
}

h1 {
  text-align: center;
  color: #facc15;
  margin: 5px 0;
}

.sub {
  text-align: center;
  color: #94a3b8;
  font-size: 14px;
}

input {
  width: 100%;
  padding: 14px;
  border: 1px solid #475569;
  border-radius: 10px;
  background: #26364f;
  color: white;
  margin: 12px 0;
  font-size: 16px;
}

.btn {
  width: 100%;
  padding: 14px;
  border: none;
  border-radius: 12px;
  background: #facc15;
  color: #111827;
  font-size: 16px;
  font-weight: 900;
  cursor: pointer;
  margin-top: 12px;
}

.btn:active {
  transform: scale(.97);
}

.hidden {display:none}

.progress {
  height: 9px;
  background: #334155;
  border-radius: 10px;
  overflow: hidden;
  margin: 15px 0;
}

.bar {
  height: 100%;
  width: 0%;
  background: #facc15;
  transition: .4s;
}

.top {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
}

.timer {color: #fb7185}

.question {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  margin: 20px 0;
}

.visual {
  min-height: 100px;
  background: #080f20;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
  padding: 20px;
  font-size: 35px;
  margin-bottom: 20px;
}

.options {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.options button {
  background: #334155;
  color: white;
  border: 1px solid #475569;
  padding: 15px 8px;
  min-height: 60px;
  border-radius: 12px;
  font-size: 17px;
  cursor: pointer;
}

.options button:disabled {
  opacity: .9;
}

.options button.correct {
  background: #15803d;
  border-color: #22c55e;
}

.options button.wrong {
  background: #991b1b;
  border-color: #ef4444;
}

.score {
  font-size: 75px;
  font-weight: 900;
  color: #facc15;
  text-align: center;
}

.badge {
  display: inline-block;
  background: #facc15;
  color: #111;
  padding: 8px 18px;
  border-radius: 30px;
  font-weight: bold;
  margin: 10px;
}

.result {
  text-align: center;
}

.stats {
  background: #26364f;
  padding: 15px;
  border-radius: 12px;
  text-align: left;
  line-height: 2;
  margin-top: 15px;
}

.small {
  text-align: center;
  color: #94a3b8;
  font-size: 12px;
}

.feedback {
  text-align: center;
  min-height: 25px;
  margin-top: 12px;
  font-weight: bold;
}

</style>
</head>

<body>

<div class="app">

<!-- START SCREEN -->

<div id="startScreen">

<h1>🧠 IQ PREDICTOR</h1>

<p class="sub">
10 Visual Questions • Shapes • Patterns • Colours • Logic
</p>

<input id="name" placeholder="Enter your name">

<button class="btn" onclick="startTest()">
START VISUAL TEST →
</button>

<p class="small">
90 seconds • 10 questions<br>
For entertainment, not a clinical IQ test.
</p>

</div>


<!-- QUIZ SCREEN -->

<div id="quizScreen" class="hidden">

<div class="top">
<span id="qCount">Q 1/10</span>
<span class="timer">⏱ <span id="time">90</span>s</span>
</div>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="question" id="question"></div>

<div class="visual" id="visual"></div>

<div class="options" id="options"></div>

<div class="feedback" id="feedback"></div>

</div>


<!-- RESULT SCREEN -->

<div id="resultScreen" class="hidden result">

<h1>YOUR RESULT</h1>

<p id="userName"></p>

<div class="score" id="iqScore">0</div>

<div class="badge" id="iqLevel"></div>

<p id="iqDesc"></p>

<div class="stats">

✅ Correct answers:
<b id="correctCount"></b> / 10

<br>

⏱ Time used:
<b id="timeUsed"></b> seconds

<br>

👁️ Visual score:
<b id="visualScore"></b>%

<br>

🧠 Brain type:
<b id="brainType"></b>

</div>

<p class="small">
This is a playful score estimate, not a validated IQ result.
</p>

<button class="btn" onclick="location.reload()">
RETAKE TEST ↻
</button>

<button class="btn" style="background:white"
onclick="shareResult()">
SHARE RESULT ↗
</button>

</div>

</div>


<script>

// QUESTIONS

const questions = [

{
q:"Which shape comes next?",
visual:"🔺 🔺 🔻 🔺 🔺 ?",
o:["🔻","🔺","🔵","⬛"],
a:0,
type:"Pattern"
},

{
q:"Complete the colour pattern",
visual:"🔴 🔵 🔴 🔵 ?",
o:["🔴","🔵","🟢","🟡"],
a:0,
type:"Colour"
},

{
q:"How many triangles do you see?",
visual:"▲ ▲ ▲",
o:["2","3","4","5"],
a:1,
type:"Visual"
},

{
q:"Which is the odd one out?",
visual:"🟦 🟦 🟦 🟥",
o:["🟦","🟥","🟦","🟦"],
a:1,
type:"Visual"
},

{
q:"What comes next? 1 dot, 2 dots, 3 dots...",
visual:"•   ••   •••   ?",
o:["••••","••","•","•••••"],
a:0,
type:"Logic"
},

{
q:"Which colour comes next?",
visual:"🔴 🔵 🔴 🔵 🔴 ?",
o:["🔴","🔵","🟢","🟡"],
a:1,
type:"Colour"
},

{
q:"If a triangle has 3 sides, a square 4 sides, how many sides does a pentagon have?",
visual:"🔺  🟦  ⬠",
o:["5 sides","4 sides","6 sides","3 sides"],
a:0,
type:"Logic"
},

{
q:"Find the missing shape",
visual:"⬛ ⬜ ⬛ ⬜ ?",
o:["⬛","⬜","🔺","🔵"],
a:0,
type:"Pattern"
},

{
q:"Which emoji is different?",
visual:"😀 😀 😀 😁",
o:["1st","2nd","3rd","4th"],
a:3,
type:"Visual"
},

{
q:"Complete the number sequence",
visual:"2  4  6  8  ?",
o:["9","10","11","12"],
a:1,
type:"Logic"
}

];


// VARIABLES

let current = 0;
let score = 0;
let timeLeft = 90;
let timer;
let startTime;
let answerLocked = false;
let playerName = "";


// START TEST

function startTest() {

playerName =
document.getElementById("name").value.trim() || "Player";

document.getElementById("startScreen").classList.add("hidden");

document.getElementById("quizScreen").classList.remove("hidden");

startTime = Date.now();

showQuestion();

timer = setInterval(() => {

timeLeft--;

document.getElementById("time").textContent = timeLeft;

if(timeLeft <= 0) {
clearInterval(timer);
showResult();
}

},1000);

}


// DISPLAY QUESTIONS

function showQuestion() {

if(current >= questions.length) {
showResult();
return;
}

answerLocked = false;

let q = questions[current];

document.getElementById("qCount").textContent =
`Q ${current+1}/10`;

document.getElementById("bar").style.width =
(current / questions.length * 100) + "%";

document.getElementById("question").textContent = q.q;

document.getElementById("visual").innerHTML = q.visual;

document.getElementById("feedback").textContent = "";

let options = document.getElementById("options");

options.innerHTML = "";

q.o.forEach((option,index) => {

let button = document.createElement("button");

button.textContent = option;

button.onclick = () => checkAnswer(index,button);

options.appendChild(button);

});

}


// CHECK ANSWER

function checkAnswer(index,button) {

if(answerLocked) return;

answerLocked = true;

let q = questions[current];

let buttons = document.querySelectorAll(".options button");

buttons.forEach(b => b.disabled = true);

if(index === q.a) {

score++;

button.classList.add("correct");

document.getElementById("feedback").textContent =
"✓ Correct answer!";

document.getElementById("feedback").style.color =
"#4ade80";

} else {

button.classList.add("wrong");

buttons[q.a].classList.add("correct");

document.getElementById("feedback").textContent =
"✗ Incorrect answer!";

document.getElementById("feedback").style.color =
"#f87171";

}

setTimeout(() => {

current++;

showQuestion();

},900);

}


// SHOW RESULTS

function showResult() {

clearInterval(timer);

document.getElementById("quizScreen").classList.add("hidden");

document.getElementById("resultScreen").classList.remove("hidden");

let used = Math.min(
90,
Math.floor((Date.now()-startTime)/1000)
);

let percentage = score * 10;

// Playful score, not actual IQ

let estimated = 70 + score * 10;

let level = "";
let description = "";

if(score <= 2) {

level = "Beginner Explorer";
description = "Keep practising patterns and visual puzzles!";

} else if(score <= 4) {

level = "Developing Thinker";
description = "You're building your visual reasoning skills.";

} else if(score <= 6) {

level = "Good Observer";
description = "You showed some promising pattern recognition.";

} else if(score <= 8) {

level = "Sharp Thinker";
description = "Excellent performance on these puzzles!";

} else {

level = "Master Observer";
description = "Outstanding performance on this visual challenge!";

}

let brain = "";

if(score >= 8) {
brain = "Pattern Master";
} else if(score >= 6) {
brain = "Visual Thinker";
} else if(score >= 4) {
brain = "Logical Explorer";
} else {
brain = "Creative Explorer";
}

document.getElementById("userName").textContent =
"Congratulations, " + playerName + "!";

document.getElementById("iqScore").textContent =
estimated;

document.getElementById("iqLevel").textContent =
level;

document.getElementById("iqDesc").textContent =
description;

document.getElementById("correctCount").textContent =
score;

document.getElementById("timeUsed").textContent =
used;

document.getElementById("visualScore").textContent =
percentage;

document.getElementById("brainType").textContent =
brain;

document.getElementById("bar").style.width = "100%";

}


// SHARE RESULTS

function shareResult() {

let message =
`🧠 IQ PREDICTOR RESULT

Name: ${playerName}

Score: ${score}/10

Estimated score: ${70 + score*10}

Performance: ${document.getElementById("iqLevel").textContent}

Brain type: ${document.getElementById("brainType").textContent}

Try the visual challenge yourself!`;

if(navigator.share) {

navigator.share({
title:"My IQ Predictor Result",
text:message
}).catch(() => {});

} else {

navigator.clipboard.writeText(message)
.then(() => alert("Result copied!"))
.catch(() => alert(message));

}

}

</script>

</body>
</html>
