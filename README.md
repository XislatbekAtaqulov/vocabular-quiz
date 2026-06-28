<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vocabulary Quiz Generator</title>

<style>
body{
font-family:Arial,sans-serif;
background:linear-gradient(135deg,#4f46e5,#7c3aed);
margin:0;
padding:20px;
color:white;
}

.container{
max-width:900px;
margin:auto;
}

.card{
background:white;
color:black;
padding:20px;
border-radius:20px;
margin-top:20px;
box-shadow:0 10px 30px rgba(0,0,0,.2);
}

textarea{
width:100%;
height:250px;
padding:10px;
border-radius:10px;
font-size:16px;
}

button{
padding:12px 20px;
border:none;
border-radius:10px;
cursor:pointer;
font-size:16px;
margin:5px;
}

.primary{
background:#4f46e5;
color:white;
}

.option{
display:block;
width:100%;
margin:10px 0;
padding:15px;
font-size:16px;
}

.correct{
background:#22c55e !important;
color:white;
}

.wrong{
background:#ef4444 !important;
color:white;
}

.hidden{
display:none;
}

#feedback{
font-size:22px;
font-weight:bold;
margin-top:15px;
}

#score{
font-size:22px;
}
</style>

</head>

<body>

<div class="container">

<h1>📚 Vocabulary Quiz Generator</h1>

<div id="editor" class="card">

<h2>Paste Words & Definitions</h2>

<p>Format:</p>

<pre>
Word = Definition
Analyze = Examine carefully
Infer = Reach a conclusion
</pre>

<textarea id="wordsInput">
Analyze = Examine carefully
Infer = Reach a conclusion
Evidence = Facts proving something
Contrast = Show differences
Determine = Find out exactly
Prosperity = State of wealth and success
Resilience = Ability to recover quickly
Empathy = Understanding others' feelings
</textarea>

<br><br>

<button class="primary" onclick="startQuiz()">
Generate Quiz
</button>

</div>

<div id="quizSection" class="card hidden">

<h2 id="progress"></h2>

<div id="score">Score: 0</div>

<h1 id="question"></h1>

<div id="options"></div>

<div id="feedback"></div>

<button id="nextBtn" class="primary hidden" onclick="nextQuestion()">
Next Question
</button>

</div>

<div id="finishSection" class="card hidden">

<h1>🎉 Quiz Finished!</h1>

<h2 id="finalScore"></h2>

<button class="primary" onclick="restartQuiz()">
Back to Edit Vocabulary
</button>

</div>

</div>

<script>

let vocabulary = [];
let currentIndex = 0;
let score = 0;
let currentCorrect = "";

function shuffle(arr){
for(let i=arr.length-1;i>0;i--){
const j=Math.floor(Math.random()*(i+1));
[arr[i],arr[j]]=[arr[j],arr[i]];
}
return arr;
}

function startQuiz(){

const lines =
document.getElementById("wordsInput")
.value
.trim()
.split("\n");

vocabulary=[];

for(const line of lines){

if(line.includes("=")){

const parts=line.split("=");

vocabulary.push({
word:parts[0].trim(),
definition:parts[1].trim()
});

}
}

if(vocabulary.length<4){
alert("Please enter at least 4 words.");
return;
}

shuffle(vocabulary);

currentIndex=0;
score=0;

document.getElementById("editor").classList.add("hidden");
document.getElementById("finishSection").classList.add("hidden");
document.getElementById("quizSection").classList.remove("hidden");

showQuestion();
}

function showQuestion(){

document.getElementById("feedback").innerHTML="";
document.getElementById("nextBtn").classList.add("hidden");

let item=vocabulary[currentIndex];

currentCorrect=item.definition;

document.getElementById("question").innerText=
item.word;

document.getElementById("progress").innerText=
"Question "+(currentIndex+1)+" / "+vocabulary.length;

document.getElementById("score").innerText=
"Score: "+score;

let options=[item.definition];

while(options.length<4){

let random=
vocabulary[Math.floor(Math.random()*vocabulary.length)]
.definition;

if(!options.includes(random)){
options.push(random);
}
}

shuffle(options);

let html="";

for(let option of options){

html+=`
<button class="option"
onclick="checkAnswer(this,'${option.replace(/'/g,"")}')">
${option}
</button>
`;

}

document.getElementById("options").innerHTML=html;
}

function checkAnswer(button,answer){

const buttons=
document.querySelectorAll(".option");

buttons.forEach(btn=>btn.disabled=true);

if(answer===currentCorrect){

button.classList.add("correct");

score++;

document.getElementById("feedback").innerHTML=
"✅ Correct!";

}else{

button.classList.add("wrong");

document.getElementById("feedback").innerHTML=
"❌ Correct Answer: "+currentCorrect;

buttons.forEach(btn=>{

if(btn.innerText===currentCorrect){
btn.classList.add("correct");
}

});

}

document.getElementById("score").innerText=
"Score: "+score;

document.getElementById("nextBtn").classList.remove("hidden");
}

function nextQuestion(){

currentIndex++;

if(currentIndex>=vocabulary.length){

finishQuiz();
return;
}

showQuestion();
}

function finishQuiz(){

document.getElementById("quizSection")
.classList.add("hidden");

document.getElementById("finishSection")
.classList.remove("hidden");

document.getElementById("finalScore").innerText=
"Final Score: "+score+" / "+vocabulary.length;
}

function restartQuiz(){

document.getElementById("editor")
.classList.remove("hidden");

document.getElementById("quizSection")
.classList.add("hidden");

document.getElementById("finishSection")
.classList.add("hidden");
}

</script>

</body>
</html>
