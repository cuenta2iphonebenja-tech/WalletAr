#   
  
<!DOCTYPE html>  
<html lang="es">  
<head>  
<meta charset="UTF-8" />  
<meta name="viewport" content="width=device-width, initial-scale=1.0" />  
<title>WalletAR</title>  
<link rel="manifest" href="manifest.json">  
<style>  
:root{  
  --primary:#009ee3;  
  --bg:#f4f6f8;  
  --card:#ffffff;  
}  
body{  
  margin:0;  
  font-family:-apple-system, BlinkMacSystemFont, sans-serif;  
  background:var(--bg);  
}  
header{  
  background:var(--primary);  
  color:#fff;  
  padding:20px;  
  font-size:22px;  
  font-weight:600;  
}  
.balance{  
  background:var(--card);  
  margin:15px;  
  padding:20px;  
  border-radius:14px;  
}  
.balance h2{  
  margin:0;  
  font-size:16px;  
  color:#555;  
}  
.balance h1{  
  margin:10px 0 0;  
  font-size:32px;  
}  
.actions{  
  display:flex;  
  gap:10px;  
  margin:15px;  
}  
button{  
  flex:1;  
  padding:14px;  
  border:none;  
  border-radius:12px;  
  background:var(--primary);  
  color:white;  
  font-size:16px;  
}  
.card{  
  background:var(--card);  
  margin:15px;  
  padding:15px;  
  border-radius:14px;  
}  
input{  
  width:100%;  
  padding:12px;  
  margin-top:10px;  
  border-radius:10px;  
  border:1px solid #ccc;  
  font-size:15px;  
}  
.hidden{display:none;}  
.tx{  
  font-size:14px;  
  border-bottom:1px solid #eee;  
  padding:8px 0;  
}  
</style>  
</head>  
<body>  
  
<header id="header">WalletAR</header>  
  
<div class="balance">  
  <h2>Dinero disponible</h2>  
  <h1 id="saldo">$ 1.000.000</h1>  
</div>  
  
<div class="actions">  
  <button onclick="mostrarTransferencia()">Transferir</button>  
</div>  
  
<div class="card hidden" id="transferencia">  
  <h3>Transferir dinero</h3>  
  <input id="destino" placeholder="Destinatario" />  
  <input id="monto" type="number" placeholder="Monto" />  
  <button onclick="transferir()">Confirmar</button>  
</div>  
  
<div class="card hidden" id="comprobante"></div>  
  
<div class="card">  
  <h3>Movimientos</h3>  
  <div id="historial"></div>  
</div>  
  
<script>  
let saldo = 1000000;  
let admin = false;  
  
function formato(n){  
  return "$ " + n.toLocaleString("es-AR");  
}  
  
function mostrarTransferencia(){  
  document.getElementById("transferencia").classList.toggle("hidden");  
}  
  
function transferir(){  
  const d = document.getElementById("destino").value;  
  const m = parseInt(document.getElementById("monto").value);  
  if(!d || !m || m>saldo) return alert("Datos inválidos");  
  
  saldo -= m;  
  document.getElementById("saldo").innerText = formato(saldo);  
  
  const fecha = new Date().toLocaleString("es-AR");  
  const tx = `Transferencia a ${d} - ${formato(m)} - ${fecha}`;  
  document.getElementById("historial").innerHTML =  
    `<div class="tx">${tx}</div>` + document.getElementById("historial").innerHTML;  
  
  document.getElementById("comprobante").innerHTML = `  
    <h3>Comprobante</h3>  
    <p>${tx}</p>  
    <button onclick="compartir('${tx}')">Compartir</button>  
  `;  
  document.getElementById("comprobante").classList.remove("hidden");  
}  
  
function compartir(texto){  
  if(navigator.share){  
    navigator.share({text:texto});  
  } else {  
    alert(texto);  
  }  
}  
  
/* 🔒 MODO OCULTO */  
let taps = 0;  
document.getElementById("header").addEventListener("click", ()=>{  
  taps++;  
  if(taps===7){  
    const pass = prompt("Código:");  
    if(pass==="7429"){  
      admin = true;  
      alert("Modo edición activado");  
      document.getElementById("destino").value="Editable";  
    }  
    taps=0;  
  }  
});  
</script>  
  
</body>  
</html>  
