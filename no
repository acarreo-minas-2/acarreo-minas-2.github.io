<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Acarreo de Minerales y Cálculo de Eficiencia</title>
  <style>
    /* Estilos para Código A */
    #canvasContainer {
      position: relative;
    }
    #drawingCanvas {
      border: 2px solid black;
      cursor: crosshair;
    }
    #yellowSquare {
      position: absolute;
      width: 20px;
      height: 20px;
      background-color: yellow;
      pointer-events: none;
    }
    #buttonPanel {
      background-color: #34495e;
      padding: 10px;
      color: white;
      display: flex;
      align-items: center;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    }
    #buttonPanel button {
      background-color: #5d6d7e;
      color: white;
      border: none;
      padding: 10px;
      cursor: pointer;
      transition: background-color 0.3s;
      border-radius: 4px;
    }
    #buttonPanel button:hover {
      background-color: #85a5cc;
    }
    #title {
      font-size: 32px;
      font-weight: bold;
      text-align: center;
      color: #1c2833;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    }
    #truckTable {
      margin: 20px auto;
      border-collapse: collapse;
      width: 80%;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
    }
    #truckTable th {
      background-color: #f39c12;
      color: white;
      padding: 15px;
      text-align: center;
      border: 1px solid #ddd;
    }
    #truckTable td {
      background-color: #fce5cd;
      border: 1px solid #ddd;
    }
    #truckTable tr:nth-child(even) {
      background-color: #f8c471;
    }

    /* Estilos para Código B */
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }
    .container {
      max-width: 600px;
      margin: auto;
    }
    label, input {
      display: block;
      margin: 10px auto;
      width: 80%;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
    }
    .result {
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <!-- Sección de Código A -->
  <div id="title">Acarreo de Minerales</div>
  <div id="canvasContainer">
    <canvas id="drawingCanvas" width="800" height="600"></canvas>
    <div id="yellowSquare"></div>
  </div>
  <div id="buttonPanel">
    <button onclick="startMovement()">Empezar Movimiento</button>
    <button onclick="stopMovement()">Detener Movimiento</button>
    <button onclick="clearCanvas()">Limpiar</button>
    <label for="speedControl">Velocidad:</label>
    <input type="range" id="speedControl" min="1" max="10" value="5">
    <label for="colorControl">Color:</label>
    <select id="colorControl">
      <option value="red">Rojo</option>
      <option value="blue">Azul</option>
      <option value="green">Verde</option>
      <option value="black">Negro</option>
    </select>
  </div>
  <table id="truckTable">
    <thead>
      <tr>
        <th>Característica</th>
        <th>Descripción</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Modelo</td>
        <td>CAT 797F</td>
      </tr>
      <tr>
        <td>Velocidad Máxima</td>
        <td>67 km/h</td>
      </tr>
      <tr>
        <td>Capacidad de Acarreo</td>
        <td>400 toneladas</td>
      </tr>
    </tbody>
  </table>

  <!-- Script de Código A -->
  <script>
    const canvas = document.getElementById('drawingCanvas');
    const ctx = canvas.getContext('2d');
    const yellowSquare = document.getElementById('yellowSquare');
    const speedControl = document.getElementById('speedControl');
    const colorControl = document.getElementById('colorControl');
    let drawing = false;
    let path = [];
    let animationFrameId;
    let index = 0;
    let movementActive = false;

    canvas.addEventListener('mousedown', (e) => {
      drawing = true;
      ctx.strokeStyle = colorControl.value;
      ctx.lineWidth = 2;
      ctx.beginPath();
      ctx.moveTo(e.offsetX, e.offsetY);
      path.push({ x: e.offsetX, y: e.offsetY, color: colorControl.value });
    });

    canvas.addEventListener('mousemove', (e) => {
      if (!drawing) return;
      ctx.lineTo(e.offsetX, e.offsetY);
      ctx.stroke();
      path.push({ x: e.offsetX, y: e.offsetY, color: colorControl.value });
    });

    canvas.addEventListener('mouseup', () => {
      drawing = false;
    });

    function startMovement() {
      if (path.length === 0 || movementActive) return;
      movementActive = true;
      index = 0;
      moveSquare();
    }

    function moveSquare() {
      while (index < path.length && path[index].color !== 'red') {
        index++;
      }
      if (index >= path.length) {
        index = 0;
      }
      if (path[index] && path[index].color === 'red') {
        yellowSquare.style.left = path[index].x + 'px';
        yellowSquare.style.top = path[index].y + 'px';
        index += parseInt(speedControl.value);
        animationFrameId = requestAnimationFrame(moveSquare);
      }
    }

    function stopMovement() {
      movementActive = false;
      cancelAnimationFrame(animationFrameId);
    }

    function clearCanvas() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      path = [];
      index = 0;
      movementActive = false;
      cancelAnimationFrame(animationFrameId);
    }
  </script>

  <!-- Sección de Código B -->
  <div class="container">
    <h1>Cálculo de Eficiencia de Acarreo</h1>
    <img src="https://via.placeholder.com/400" alt="Esquema del tajo con tramos">
    <h2>Datos de Velocidad y Capacidad de Camiones</h2>
    <label>Velocidad de Camión Vacío 1 (KM/H): <input type="number" id="vel_vacio_1" value="10"></label>
    <label>Velocidad de Camión Vacío 2 (KM/H): <input type="number" id="vel_vacio_2" value="10"></label>
    <label>Velocidad de Camión Vacío 3 (KM/H): <input type="number" id="vel_vacio_3" value="10"></label>
    <label>Velocidad de Camión Lleno 1 (KM/H): <input type="number" id="vel_lleno_1" value="8"></label>
    <label>Velocidad de Camión Lleno 2 (KM/H): <input type="number" id="vel_lleno_2" value="8"></label>
    <label>Velocidad de Camión Lleno 3 (KM/H): <input type="number" id="vel_lleno_3" value="8"></label>
    <label>Capacidad de Carga de Camión 1 (T): <input type="number" id="capacidad_1" value="10"></label>
    <label>Capacidad de Carga de Camión 2 (T): <input type="number" id="capacidad_2" value="10"></label>
    <label>Capacidad de Carga de Camión 3 (T): <input type="number" id="capacidad_3" value="10"></label>
    <button onclick="calcularEficiencia()">Calcular</button>
    <div id="resultado" class="result"></div>
  </div>

  <!-- Script de Código B -->
  <script>
    const distancias = [2, 2, 2];

    function calcularTiempoTramo(distancia, velocidad) {
      return distancia / velocidad;
    }

    function calcularEficiencia() {
      const vel_vacio = [
        parseFloat(document.getElementById("vel_vacio_1").value),
        parseFloat(document.getElementById("vel_vacio_2").value),
        parseFloat(document.getElementById("vel_vacio_3").value)
      ];
      const vel_lleno = [
        parseFloat(document.getElementById("vel_lleno_1").value),
        parseFloat(document.getElementById("vel_lleno_2").value),
        parseFloat(document.getElementById("vel_lleno_3").value)
      ];
      const capacidades = [
        parseFloat(document.getElementById("capacidad_1").value),
        parseFloat(document.getElementById("capacidad_2").value),
        parseFloat(document.getElementById("capacidad_3").value)
      ];

      const tiempos_ida = distancias.map((distancia, i) => calcularTiempoTramo(distancia, vel_vacio[i]));
      const tiempos_vuelta = distancias.map((distancia, i) => calcularTiempoTramo(distancia, vel_lleno[i]));

      const tiempo_ida_total = tiempos_ida.reduce((acc, tiempo) => acc + tiempo, 0);
      const tiempo_vuelta_total = tiempos_vuelta.reduce((acc, tiempo) => acc + tiempo, 0);
      const tiempo_ciclo_total = tiempo_ida_total + tiempo_vuelta_total;

      const ciclos_por_hora = 1 / tiempo_ciclo_total;
      const tonelaje_por_hora_total = capacidades.reduce((acc, capacidad) => acc + capacidad, 0) * ciclos_por_hora;

      const esEficiente = tonelaje_por_hora_total > 1000;
      let resultadoHTML = `<p>Tiempo de Ciclo Total: ${tiempo_ciclo_total.toFixed(2)} horas</p>`;
      resultadoHTML += `<p>Ciclos por Hora: ${ciclos_por_hora.toFixed(2)}</p>`;
      resultadoHTML += `<p>Tonelaje por Hora Total: ${tonelaje_por_hora_total.toFixed(2)} T</p>`;
      resultadoHTML += `<p>${esEficiente ? "El ciclo de acarreo ES eficiente." : "El ciclo de acarreo NO es eficiente."}</p>`;

      document.getElementById("resultado").innerHTML = resultadoHTML;
    }
  </script>
</body>
</html>
