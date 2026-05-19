# Capturas del Proyecto

## Captura del código corriendo en Wokwi

<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/1e26483e-d7c0-4ea7-831a-2d95b299d96f" />

---

## Captura del programa ejecutando

<img width="761" height="667" alt="image" src="https://github.com/user-attachments/assets/77a8e50d-c324-411d-9358-e1ce2f6d84d5" />

---
## Captura del programa ejecutando en Thonny

<img width="1917" height="875" alt="image" src="https://github.com/user-attachments/assets/591fcf70-b65a-442d-87b6-f10e140bc00d" />

---
# Código Fuente Thonny

```python
import network
import socket
from machine import Pin
import time

# ==================================
# WIFI
# ==================================
ssid = 'TP-Link_8A73'
password = 'Ccomputo0908'

wifi = network.WLAN(network.STA_IF)
wifi.active(True)

print("Conectando WiFi...")
wifi.connect(ssid, password)

timeout = 20

while timeout > 0:
    if wifi.isconnected():
        break

    timeout -= 1
    print(".", end="")
    time.sleep(1)

if not wifi.isconnected():
    print("\nERROR: No se pudo conectar al WiFi")
    raise Exception("Sin conexión WiFi")

print("\nWiFi conectado!")
print("IP:", wifi.ifconfig()[0])

# ==================================
# LED INTERNO
# ==================================
led = Pin("LED", Pin.OUT)

# ==================================
# HTML
# ==================================
html = """
<!DOCTYPE html>
<html lang="es">

<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>CORE MONITOR</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial;
}

body{
    background:#050b1d;
    color:white;
    display:flex;
}

/* SIDEBAR */

.sidebar{
    width:250px;
    background:#0d1324;
    min-height:100vh;
    padding:20px;
    border-right:1px solid #13203d;
}

.logo{
    color:#00d9ff;
    font-size:30px;
    font-weight:bold;
    margin-bottom:30px;
}

.menu button{
    width:100%;
    padding:14px;
    margin-bottom:12px;
    border:none;
    border-radius:12px;
    background:#111c35;
    color:white;
    cursor:pointer;
    text-align:left;
    transition:0.3s;
}

.menu button:hover{
    background:#1677ff;
}

.reboot{
    background:red !important;
}

/* MAIN */

.main{
    flex:1;
    padding:20px;
}

.topbar{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:20px;
}

.topbar h1{
    color:#00d9ff;
}

.grid{
    display:grid;
    grid-template-columns:2fr 1fr;
    gap:20px;
}

/* PANELS */

.panel{
    background:#0f172b;
    border:1px solid #1c2b52;
    border-radius:22px;
    padding:20px;
    box-shadow:0 0 20px rgba(0,0,0,0.5);
}

.panel h2{
    margin-bottom:20px;
}

/* CARDS */

.cards{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:15px;
}

.card{
    background:#101c34;
    border-radius:18px;
    padding:20px;
}

.card h3{
    color:#00d9ff;
    font-size:35px;
    margin-top:10px;
}

.alert{
    color:#ff9b9b !important;
}

/* BARS */

.bar-bg{
    width:100%;
    height:12px;
    background:#1c2740;
    border-radius:20px;
    margin-top:10px;
    overflow:hidden;
}

.bar{
    height:100%;
    border-radius:20px;
}

.ram{
    width:78%;
    background:#4de1ff;
}

.vram{
    width:91%;
    background:#1677ff;
}

.temp{
    width:82%;
    background:#ff9b9b;
}

/* CHART */

.chart{
    height:200px;
    border:2px dashed #20304d;
    border-radius:20px;
    margin-top:20px;
    display:flex;
    align-items:center;
    justify-content:center;
    color:#4d5d7d;
}

/* CONTROL */

.switch{
    width:55px;
    height:28px;
    background:#00d9ff;
    border-radius:20px;
}

/* INPUTS */

.slider{
    width:100%;
    margin-top:10px;
}

/* CONFIG */

.config-grid{
    display:grid;
    grid-template-columns:repeat(2,1fr);
    gap:15px;
}

.input-box{
    background:#101c34;
    border-radius:14px;
    padding:15px;
}

.warning{
    background:#11243f;
    border:1px solid #00d9ff;
    padding:18px;
    border-radius:16px;
    margin-top:20px;
    color:#c7eaff;
}

.save-btn{
    width:100%;
    padding:16px;
    margin-top:20px;
    border:none;
    border-radius:16px;
    background:#4de1ff;
    color:black;
    font-weight:bold;
    cursor:pointer;
}

/* BOTONES */

.blue-btn{
    background:#4de1ff;
    border:none;
    padding:12px 20px;
    border-radius:12px;
    font-weight:bold;
    cursor:pointer;
}

.action-btn{
    width:100%;
    padding:15px;
    margin-top:20px;
    border:none;
    border-radius:14px;
    background:#1677ff;
    color:white;
    font-weight:bold;
    cursor:pointer;
    transition:0.3s;
}

.action-btn:hover{
    background:#00d9ff;
    color:black;
}

</style>

</head>

<body>

<!-- SIDEBAR -->
<div class="sidebar">

    <div class="logo">
        CORE MONITOR
    </div>

    <div class="menu">
        <button>Overview</button>
        <button>Telemetry</button>
        <button>Performance</button>
        <button>Lab Config</button>

        <button class="reboot">
            REBOOT SYSTEM
        </button>
    </div>

</div>

<!-- MAIN -->
<div class="main">

    <div class="topbar">
        <h1>Dashboard</h1>

        <button class="blue-btn">
            Actualizar Datos
        </button>
    </div>

    <!-- DASHBOARD -->
    <div class="panel">

        <h2>🖥 Device Dashboard</h2>

        <div class="cards">

            <div class="card">
                <p>RAM Disponible</p>
                <h3>4 GB</h3>
            </div>

            <div class="card">
                <p>VRAM Disponible</p>
                <h3>2 GB</h3>
            </div>

            <div class="card">
                <p>Equipos Afectados</p>
                <h3 class="alert">65%</h3>
            </div>

        </div>

    </div>

    <br>

    <div class="grid">

        <!-- SISTEMA -->
        <div class="panel">

            <h2>📊 Uso del Sistema</h2>

            <p>RAM USAGE - 78%</p>
            <div class="bar-bg">
                <div class="bar ram"></div>
            </div>

            <br>

            <p>VRAM USAGE - 91%</p>
            <div class="bar-bg">
                <div class="bar vram"></div>
            </div>

            <br>

            <p>GPU TEMPERATURE - 82°C</p>
            <div class="bar-bg">
                <div class="bar temp"></div>
            </div>

            <div class="chart">
                HISTORICAL TELEMETRY FEED
            </div>

        </div>

        <!-- CONTROL -->
        <div class="panel">

            <h2>🎛 Centro de Control</h2>

            <p>Performance Mode</p>
            <div class="switch"></div>

            <br><br>

            <p>VRAM Optimization</p>
            <div class="switch"></div>

            <br><br>

            <p>Fan Speed (75%)</p>
            <input type="range" class="slider" value="75">

            <br><br>

            <p>RAM Allocation (60%)</p>
            <input type="range" class="slider" value="60">

            <button class="action-btn">
                Aplicar Configuración
            </button>

            <button class="action-btn"
            onclick="location.href='/led/on'">
                Encender LED
            </button>

            <button class="action-btn"
            onclick="location.href='/led/off'">
                Apagar LED
            </button>

        </div>

    </div>

    <br>

    <!-- CONFIG -->
    <div class="panel">

        <h2>⚙ Configuración</h2>

        <div class="config-grid">

            <div class="input-box">
                <p>LAB NAME</p>
                <h3>Laboratorio ITT</h3>
            </div>

            <div class="input-box">
                <p>REGION CODE</p>
                <h3>MX-BC-TJ</h3>
            </div>

            <div class="input-box">
                <p>RECOMMENDED RAM</p>
                <h3>16 GB</h3>
            </div>

            <div class="input-box">
                <p>RECOMMENDED VRAM</p>
                <h3>8 GB</h3>
            </div>

        </div>

        <div class="warning">
            Se detectó escasez de RAM y VRAM en múltiples equipos del Instituto Tecnológico de Tijuana.
        </div>

        <button class="save-btn">
            Guardar Configuración
        </button>

    </div>

</div>

</body>
</html>
"""

# ==================================
# SERVIDOR WEB
# ==================================
addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]

server = socket.socket()
server.bind(addr)
server.listen(1)

print("\nServidor iniciado")
print("Abrir navegador:")
print("http://" + wifi.ifconfig()[0])

# ==================================
# LOOP PRINCIPAL
# ==================================
while True:

    client, addr = server.accept()

    print("Cliente conectado desde:", addr)

    request = client.recv(1024)
    request = str(request)

    print("Peticion:", request)

    # LED ON
    if '/led/on' in request:
        led.value(1)

    # LED OFF
    if '/led/off' in request:
        led.value(0)

    # RESPUESTA
    client.send('HTTP/1.1 200 OK\r\n')
    client.send('Content-Type: text/html\r\n')
    client.send('Connection: close\r\n\r\n')

    client.sendall(html)

    client.close()

```

# Código Fuente

```python
import network
import socket
from machine import Pin
import time

# ==================================
# WIFI
# ==================================
ssid = 'Wokwi-GUEST'
password = ''

wifi = network.WLAN(network.STA_IF)
wifi.active(True)

print("Conectando WiFi...")
wifi.connect(ssid, password)

while not wifi.isconnected():
    time.sleep(0.5)
    print(".", end="")

print("\nWiFi conectado!")
print("IP:", wifi.ifconfig()[0])

# ==================================
# LED INTERNO
# ==================================
led = Pin("LED", Pin.OUT)

# ==================================
# PAGINA HTML
# ==================================
html = """
<!DOCTYPE html>
<html lang="es">
<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>CORE MONITOR</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial;
}

body{
    background:#050b1d;
    color:white;
    display:flex;
}

/* ========================= */
/* SIDEBAR */
/* ========================= */

.sidebar{
    width:250px;
    background:#0d1324;
    min-height:100vh;
    padding:20px;
    border-right:1px solid #13203d;
}

.logo{
    color:#00d9ff;
    font-size:30px;
    font-weight:bold;
    margin-bottom:30px;
}

.menu button{
    width:100%;
    padding:14px;
    margin-bottom:12px;
    border:none;
    border-radius:12px;
    background:#111c35;
    color:white;
    cursor:pointer;
    text-align:left;
}

.menu button:hover{
    background:#1677ff;
}

.reboot{
    background:red !important;
}

/* ========================= */
/* MAIN */
/* ========================= */

.main{
    flex:1;
    padding:20px;
}

.topbar{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:20px;
}

.topbar h1{
    color:#00d9ff;
}

.grid{
    display:grid;
    grid-template-columns:2fr 1fr;
    gap:20px;
}

/* ========================= */
/* PANELS */
/* ========================= */

.panel{
    background:#0f172b;
    border:1px solid #1c2b52;
    border-radius:22px;
    padding:20px;
    box-shadow:0 0 20px rgba(0,0,0,0.5);
}

.panel h2{
    margin-bottom:20px;
    color:#ffffff;
}

/* ========================= */
/* CARDS */
/* ========================= */

.cards{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:15px;
}

.card{
    background:#101c34;
    border-radius:18px;
    padding:20px;
}

.card h3{
    color:#00d9ff;
    font-size:35px;
    margin-top:10px;
}

.alert{
    color:#ff9b9b !important;
}

/* ========================= */
/* BARRAS */
/* ========================= */

.bar-bg{
    width:100%;
    height:12px;
    background:#1c2740;
    border-radius:20px;
    margin-top:10px;
    overflow:hidden;
}

.bar{
    height:100%;
    border-radius:20px;
}

.ram{
    width:78%;
    background:#4de1ff;
}

.vram{
    width:91%;
    background:#1677ff;
}

.temp{
    width:82%;
    background:#ff9b9b;
}

/* ========================= */
/* GRAFICA */
/* ========================= */

.chart{
    height:200px;
    border:2px dashed #20304d;
    border-radius:20px;
    margin-top:20px;
    display:flex;
    align-items:center;
    justify-content:center;
    color:#4d5d7d;
}

/* ========================= */
/* CONTROL */
/* ========================= */

.switch{
    width:55px;
    height:28px;
    background:#00d9ff;
    border-radius:20px;
}

.slider{
    width:100%;
    margin-top:10px;
}

/* ========================= */
/* CONFIG */
/* ========================= */

.config-grid{
    display:grid;
    grid-template-columns:repeat(2,1fr);
    gap:15px;
}

.input-box{
    background:#101c34;
    border-radius:14px;
    padding:15px;
}

.warning{
    background:#11243f;
    border:1px solid #00d9ff;
    padding:18px;
    border-radius:16px;
    margin-top:20px;
    color:#c7eaff;
}

.save-btn{
    width:100%;
    padding:16px;
    margin-top:20px;
    border:none;
    border-radius:16px;
    background:#4de1ff;
    color:black;
    font-weight:bold;
    cursor:pointer;
}

/* ========================= */
/* BOTONES */
/* ========================= */

.blue-btn{
    background:#4de1ff;
    border:none;
    padding:12px 20px;
    border-radius:12px;
    font-weight:bold;
    cursor:pointer;
}

.action-btn{
    width:100%;
    padding:15px;
    margin-top:20px;
    border:none;
    border-radius:14px;
    background:#1677ff;
    color:white;
    font-weight:bold;
}

</style>
</head>

<body>

<!-- SIDEBAR -->
<div class="sidebar">

    <div class="logo">
        CORE MONITOR
    </div>

    <div class="menu">
        <button>Overview</button>
        <button>Telemetry</button>
        <button>Performance</button>
        <button>Lab Config</button>

        <button class="reboot">
            REBOOT SYSTEM
        </button>
    </div>

</div>

<!-- MAIN -->
<div class="main">

    <div class="topbar">
        <h1>Dashboard</h1>

        <button class="blue-btn">
            Actualizar Datos
        </button>
    </div>

    <!-- DEVICE DASHBOARD -->
    <div class="panel">

        <h2>🖥 Device Dashboard</h2>

        <div class="cards">

            <div class="card">
                <p>RAM Disponible</p>
                <h3>4 GB</h3>
            </div>

            <div class="card">
                <p>VRAM Disponible</p>
                <h3>2 GB</h3>
            </div>

            <div class="card">
                <p>Equipos Afectados</p>
                <h3 class="alert">65%</h3>
            </div>

        </div>

    </div>

    <br>

    <div class="grid">

        <!-- USO SISTEMA -->
        <div class="panel">

            <h2>📊 Uso del Sistema</h2>

            <p>RAM USAGE - 78%</p>
            <div class="bar-bg">
                <div class="bar ram"></div>
            </div>

            <br>

            <p>VRAM USAGE - 91%</p>
            <div class="bar-bg">
                <div class="bar vram"></div>
            </div>

            <br>

            <p>GPU TEMPERATURE - 82°C</p>
            <div class="bar-bg">
                <div class="bar temp"></div>
            </div>

            <div class="chart">
                HISTORICAL TELEMETRY FEED
            </div>

        </div>

        <!-- CONTROL -->
        <div class="panel">

            <h2>🎛 Centro de Control</h2>

            <p>Performance Mode</p>
            <div class="switch"></div>

            <br><br>

            <p>VRAM Optimization</p>
            <div class="switch"></div>

            <br><br>

            <p>Fan Speed (75%)</p>
            <input type="range" class="slider" value="75">

            <br><br>

            <p>RAM Allocation (60%)</p>
            <input type="range" class="slider" value="60">

            <button class="action-btn">
                Aplicar Configuración
            </button>

            <button class="action-btn"
            onclick="location.href='/led/on'">
                Encender LED
            </button>

            <button class="action-btn"
            onclick="location.href='/led/off'">
                Apagar LED
            </button>

        </div>

    </div>

    <br>

    <!-- CONFIG -->
    <div class="panel">

        <h2>📁 Configuración</h2>

        <div class="config-grid">

            <div class="input-box">
                <p>LAB NAME</p>
                <h3>Laboratorio ITT</h3>
            </div>

            <div class="input-box">
                <p>REGION CODE</p>
                <h3>MX-BC-TJ</h3>
            </div>

            <div class="input-box">
                <p>RECOMMENDED RAM</p>
                <h3>16 GB</h3>
            </div>

            <div class="input-box">
                <p>RECOMMENDED VRAM</p>
                <h3>8 GB</h3>
            </div>

        </div>

        <div class="warning">
            Se detectó escasez de RAM y VRAM en múltiples equipos del Instituto Tecnológico de Tijuana.
        </div>

        <button class="save-btn">
            Guardar Configuración
        </button>

    </div>

</div>

</body>
</html>
"""

# ==================================
# SERVIDOR WEB
# ==================================
addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]

server = socket.socket()
server.bind(addr)
server.listen(1)

print("Servidor iniciado")
print("Abrir navegador:")
print("http://" + wifi.ifconfig()[0])

# ==================================
# LOOP
# ==================================
while True:

    client, addr = server.accept()

    request = client.recv(1024)
    request = str(request)

    print("Peticion:", request)

    if '/led/on' in request:
        led.value(1)

    if '/led/off' in request:
        led.value(0)

    client.send('HTTP/1.1 200 OK\\r\\n')
    client.send('Content-Type: text/html\\r\\n')
    client.send('Connection: close\\r\\n\\r\\n')

    client.sendall(html)

    client.close()
```
