# my-site
Public
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Проверка сети</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    background:#000;
    color:white;
    font-family:-apple-system,BlinkMacSystemFont,sans-serif;
    overflow:hidden;
}

.screen{
    position:fixed;
    inset:0;
    display:flex;
    justify-content:center;
    align-items:center;
    flex-direction:column;
}

.hidden{
    display:none;
}

.btn{
    padding:18px 30px;
    border:none;
    border-radius:16px;
    font-size:20px;
    background:#007aff;
    color:white;
}

.ring{
    width:180px;
    height:180px;
    border-radius:50%;
    position:relative;
}

.ring svg{
    width:100%;
    height:100%;
    transform:rotate(-90deg);
}

.ring-bg{
    fill:none;
    stroke:#222;
    stroke-width:10;
}

.ring-progress{
    fill:none;
    stroke:#00ff88;
    stroke-width:10;
    stroke-linecap:round;
    stroke-dasharray:565;
    stroke-dashoffset:565;
    filter:drop-shadow(0 0 15px #00ff88);
}

.percent{
    position:absolute;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    font-size:34px;
    font-weight:bold;
}

.title{
    margin-top:25px;
    font-size:24px;
}

.subtitle{
    margin-top:10px;
    color:#aaa;
}

#imageScreen{
    position:fixed;
    inset:0;
    display:none;
    justify-content:center;
    align-items:center;
    background:white;
}

#imageScreen img{
    max-width:100%;
    max-height:100%;
    object-fit:contain;
}

.glow{
    position:absolute;
    width:400px;
    height:400px;
    border-radius:50%;
    background:radial-gradient(circle,#00ff8850,transparent 70%);
    animation:pulse 2s infinite;
}

@keyframes pulse{
    0%{transform:scale(0.8);}
    50%{transform:scale(1.2);}
    100%{transform:scale(0.8);}
}
</style>
</head>
<body>

<div id="start" class="screen">
    <button class="btn" onclick="startCheck()">
        📶 Подключиться к общественной сети
    </button>
</div>

<div id="scan" class="screen hidden">

    <div class="glow"></div>

    <div class="ring">
        <svg>
            <circle class="ring-bg" cx="90" cy="90" r="80"/>
            <circle id="progress" class="ring-progress" cx="90" cy="90" r="80"/>
        </svg>
        <div id="percent" class="percent">0%</div>
    </div>

    <div class="title">Проверка доступа</div>
    <div id="status" class="subtitle">Инициализация...</div>

</div>

<div id="imageScreen">
    <img src="https://pin.it/qjKWG2xlt">
</div>

<script>

function startCheck(){

    document.getElementById("start").classList.add("hidden");
    document.getElementById("scan").classList.remove("hidden");

    if(navigator.vibrate){
        navigator.vibrate(100);
    }

    const circle = document.getElementById("progress");
    const percent = document.getElementById("percent");
    const status = document.getElementById("status");

    const statuses = [
        "Поиск сети...",
        "Проверка устройства...",
        "Проверка доступа...",
        "Подтверждение...",
        "Доступ разрешён ✓"
    ];

    let p = 0;

    const timer = setInterval(()=>{

        p++;

        const offset = 565 - (565*p/100);

        circle.style.strokeDashoffset = offset;
        percent.textContent = p + "%";

        if(p===20) status.textContent = statuses[1];
        if(p===45) status.textContent = statuses[2];
        if(p===70) status.textContent = statuses[3];
        if(p===100){
            status.textContent = statuses[4];
            clearInterval(timer);

            if(navigator.vibrate){
                navigator.vibrate([100,50,100]);
            }

            setTimeout(()=>{
                document.getElementById("scan").style.display="none";
                document.getElementById("imageScreen").style.display="flex";
            },1000);
        }

    },35);

}

</script>

</body>
</html>