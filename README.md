<html lang="th">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Japanese Vowel Battle!</title>

<style>
    body{
        margin:0;
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg,#ffb6c1,#ffd166,#9bf6ff);
        overflow:hidden;
        text-align:center;
    }

    h1{
        margin-top:20px;
        font-size:50px;
        color:white;
        text-shadow:3px 3px 0 #000;
    }

    #score{
        font-size:30px;
        color:#fff;
        margin:10px;
        font-weight:bold;
    }

    #gameArea{
        position:relative;
        width:100vw;
        height:80vh;
        overflow:hidden;
    }

    .kana{
        position:absolute;
        width:100px;
        height:100px;
        border-radius:50%;
        background:white;
        display:flex;
        justify-content:center;
        align-items:center;
        font-size:60px;
        cursor:pointer;
        user-select:none;
        transition: transform .2s;
        box-shadow:0 0 15px rgba(0,0,0,.3);
    }

    .kana:hover{
        transform:scale(1.15) rotate(10deg);
    }

    .boom{
        animation:boom .4s forwards;
    }

    @keyframes boom{
        0%{
            transform:scale(1);
            opacity:1;
        }
        100%{
            transform:scale(2);
            opacity:0;
        }
    }

    #message{
        position:absolute;
        top:40%;
        left:50%;
        transform:translate(-50%,-50%);
        font-size:80px;
        font-weight:bold;
        color:white;
        text-shadow:4px 4px 0 black;
        display:none;
        z-index:999;
    }

    #startBtn{
        padding:15px 40px;
        font-size:30px;
        border:none;
        border-radius:20px;
        background:#ff006e;
        color:white;
        cursor:pointer;
        margin-top:10px;
        box-shadow:0 5px 0 #b0004f;
    }

    #startBtn:hover{
        transform:scale(1.05);
    }

    #target{
        font-size:45px;
        color:#fff;
        margin-top:10px;
        text-shadow:2px 2px 0 #000;
    }
</style>
</head>

<body>

<h1>ญี่ปุ่นสายฮา 😂</h1>

<div id="score">คะแนน: 0</div>
<div id="target">กดตัว: あ</div>

<button id="startBtn">เริ่มเกม!</button>

<div id="gameArea"></div>

<div id="message"></div>

<script>

const kanaList = ["あ","い","う","え","お"];

const funnyMessages = [
    "โอ้ยยย 😂",
    "เร็วเกินนนน 🔥",
    "เซียนญี่ปุ่นมาเอง!",
    "อาริกาโตะะะ 🤣",
    "นิฮงโกะ POWER!!",
    "ตึงจัดดด 😎",
    "ผิดดดดดด ❌"
];

const gameArea = document.getElementById("gameArea");
const scoreText = document.getElementById("score");
const targetText = document.getElementById("target");
const message = document.getElementById("message");
const startBtn = document.getElementById("startBtn");

let score = 0;
let targetKana = "あ";
let gameRunning = false;

function randomKana(){
    return kanaList[Math.floor(Math.random()*kanaList.length)];
}

function randomMessage(){
    return funnyMessages[Math.floor(Math.random()*funnyMessages.length)];
}

function showMessage(text){
    message.innerText = text;
    message.style.display = "block";

    setTimeout(()=>{
        message.style.display = "none";
    },700);
}

function createKana(){

    if(!gameRunning) return;

    const kana = document.createElement("div");
    kana.classList.add("kana");

    const letter = randomKana();
    kana.innerText = letter;

    kana.style.left = Math.random() * (window.innerWidth - 120) + "px";
    kana.style.top = "-120px";

    gameArea.appendChild(kana);

    let speed = 2 + Math.random()*4;

    let move = setInterval(()=>{

        let top = parseFloat(kana.style.top);

        kana.style.top = top + speed + "px";

        if(top > window.innerHeight){
            clearInterval(move);
            kana.remove();
        }

    },20);

    kana.addEventListener("click",()=>{

        kana.classList.add("boom");

        if(letter === targetKana){
            score++;
            showMessage(randomMessage());
        }else{
            score--;
            showMessage("ผิดตัวววว 🤣");
        }

        scoreText.innerText = "คะแนน: " + score;

        targetKana = randomKana();
        targetText.innerText = "กดตัว: " + targetKana;

        setTimeout(()=>{
            kana.remove();
        },300);

        clearInterval(move);

    });

}

function startGame(){

    score = 0;
    gameRunning = true;

    scoreText.innerText = "คะแนน: 0";

    targetKana = randomKana();
    targetText.innerText = "กดตัว: " + targetKana;

    setInterval(createKana,700);

}

startBtn.addEventListener("click",()=>{

    startBtn.style.display = "none";

    startGame();

});

</script>

</body>
</html>
