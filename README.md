<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Login Amendoza</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:#0d0d0f;
  color:#fff;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
}

.card{
  background:#141416;
  padding:25px;
  border-radius:14px;
  width:320px;
  border:1px solid #222;
}

.input-group{position:relative;}

input{
  width:100%;
  padding:10px;
  margin:6px 0;
  border-radius:8px;
  border:1px solid #333;
  background:#1c1c20;
  color:#fff;
}

.eye{
  position:absolute;
  right:10px;
  top:50%;
  transform:translateY(-50%);
  cursor:pointer;
}

button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border:none;
  border-radius:8px;
  background:#a28fff;
  cursor:pointer;
  font-weight:bold;
}

.captcha{
  text-align:center;
  background:#1c1c20;
  padding:8px;
  margin:6px 0;
  border-radius:8px;
  font-weight:bold;
}

.bar{
  height:6px;
  background:#222;
  border-radius:10px;
  overflow:hidden;
  margin-top:12px;
}

.fill{
  height:100%;
  width:100%;
  background:#6fdfb8;
}

.timer{
  text-align:center;
  margin-top:8px;
  font-size:13px;
  color:#6fdfb8;
}

.hidden{display:none}
</style>
</head>

<body>

<div class="card" id="loginBox">
  <h2>Login</h2>

  <input id="user" placeholder="Usuario">

  <div class="input-group">
    <input id="pass" type="password" placeholder="Contraseña">
    <span class="eye" id="eyeBtn">👁</span>
  </div>

  <div class="captcha" id="captchaText"></div>
  <input id="captchaInput" placeholder="Resultado">

  <button id="loginBtn">Entrar</button>
</div>

<div class="card hidden" id="homeBox">
  <h2 id="welcome"></h2>

  <div class="bar">
    <div class="fill" id="fill"></div>
  </div>

  <div class="timer" id="timer">10s restantes</div>

  <button id="logoutBtn">Salir</button>
</div>

<script>
const userInput = document.getElementById("user");
const passInput = document.getElementById("pass");
const captchaText = document.getElementById("captchaText");
const captchaInput = document.getElementById("captchaInput");
const loginBox = document.getElementById("loginBox");
const homeBox = document.getElementById("homeBox");
const welcome = document.getElementById("welcome");
const timer = document.getElementById("timer");
const fill = document.getElementById("fill");

const loginBtn = document.getElementById("loginBtn");
const logoutBtn = document.getElementById("logoutBtn");
const eyeBtn = document.getElementById("eyeBtn");

let captchaAnswer = 0;
let time = 10;
let interval;

// CAPTCHA
function generateCaptcha(){
  const a = Math.floor(Math.random()*10);
  const b = Math.floor(Math.random()*10);
  captchaAnswer = a + b;
  captchaText.innerText = `${a} + ${b} = ?`;
}

// 👁 mostrar/ocultar
eyeBtn.addEventListener("click", ()=>{
  passInput.type = passInput.type === "password" ? "text" : "password";
});

// LOGIN
loginBtn.addEventListener("click", ()=>{
  const u = userInput.value.trim();
  const p = passInput.value;
  const c = captchaInput.value;

  if(parseInt(c) !== captchaAnswer){
    alert("CAPTCHA incorrecto");
    generateCaptcha();
    return;
  }

  if(!u || !p){
    alert("Completa los datos");
    return;
  }

  // ❌ bloquear gmail.com y gmail.comm
  const correo = u.toLowerCase();
  if(
    correo.endsWith("@gmail.com") ||
    correo.endsWith("@gmail.comm")
  ){
    alert("No se permiten correos Gmail");
    return;
  }

  welcome.innerText = "Bienvenido " + u;
  loginBox.classList.add("hidden");
  homeBox.classList.remove("hidden");

  startTimer();
});

// TIMER
function startTimer(){
  clearInterval(interval);
  time = 10;

  interval = setInterval(()=>{
    time--;

    timer.innerText = time + "s restantes";
    fill.style.width = (time/10)*100 + "%";

    if(time <= 0){
      clearInterval(interval);
      alert("Sesión expirada");
      logout();
    }
  },1000);
}

// LOGOUT
function logout(){
  clearInterval(interval);
  loginBox.classList.remove("hidden");
  homeBox.classList.add("hidden");
  userInput.value = "";
  passInput.value = "";
  captchaInput.value = "";
  generateCaptcha();
}

logoutBtn.addEventListener("click", logout);

generateCaptcha();
</script>

</body>
</html>
