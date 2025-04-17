//html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Гравитационный симулятор</title>
  <style>
    body {
      margin: 0;
      background: black;
      overflow: hidden;
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <canvas id="space"></canvas>
  <script src="script.js"></script>
</body>
</html>

//js
const canvas = document.getElementById('space');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const planet = {
  x: canvas.width / 2,
  y: canvas.height / 2,
  mass: 10000,
  radius: 30
};

const rocket = {
  x: 100,
  y: 100,
  vx: 0,
  vy: 2,
  ax: 0,
  ay: 0
};

function gravity(obj, target) {
  const dx = target.x - obj.x;
  const dy = target.y - obj.y;
  const distSq = dx * dx + dy * dy;
  const dist = Math.sqrt(distSq);
  const force = target.mass / distSq;
  const ax = (dx / dist) * force;
  const ay = (dy / dist) * force;
  obj.ax = ax;
  obj.ay = ay;
}

function update() {
  gravity(rocket, planet);
  rocket.vx += rocket.ax;
  rocket.vy += rocket.ay;
  rocket.x += rocket.vx;
  rocket.y += rocket.vy;
}

function draw() {
  ctx.fillStyle = 'black';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // planet
  ctx.beginPath();
  ctx.arc(planet.x, planet.y, planet.radius, 0, Math.PI * 2);
  ctx.fillStyle = 'blue';
  ctx.fill();

  // rocket
  ctx.beginPath();
  ctx.arc(rocket.x, rocket.y, 5, 0, Math.PI * 2);
  ctx.fillStyle = 'white';
  ctx.fill();
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}

loop();




