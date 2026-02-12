if(document.getElementById('marioConsola')) document.getElementById('marioConsola').remove();

const canvas = document.createElement('canvas');
canvas.id = 'marioConsola';
canvas.width = 500; canvas.height = 200;
canvas.style = 'position:fixed; top:10px; left:10px; z-index:10000; border:5px solid #f1c40f; background:#5c94fc;';
document.body.appendChild(canvas);

const ctx = canvas.getContext('2d');
let x = 30, y = 150, velY = 0, jumping = false, nivel = 1;
const keys = {};

window.onkeydown = (e) => { keys[e.code] = true; e.preventDefault(); };
window.onkeyup = (e) => { keys[e.code] = false; };

function gameLoop() {
    ctx.clearRect(0, 0, 500, 200);
    
    // Dibujar Suelo y Meta
    ctx.fillStyle = '#8b4513'; ctx.fillRect(0, 180, 500, 20); // Suelo
    ctx.fillStyle = '#f1c40f'; ctx.fillRect(470, 100, 10, 80); // Banderín/Meta

    // Texto de Nivel
    ctx.fillStyle = 'white'; ctx.font = '16px Arial';
    ctx.fillText(`Nivel: ${nivel} - ¡Llega a la meta!`, 10, 25);

    // Controles y Física
    if (keys['ArrowRight']) x += 4;
    if (keys['ArrowLeft']) x -= 4;
    if (keys['Space'] && !jumping) { velY = -11; jumping = true; }

    velY += 0.6; y += velY;
    if (y > 160) { y = 160; jumping = false; velY = 0; }
    if (x < 0) x = 0;

    // LÓGICA DE AVANZAR NIVEL
    if (x > 460) { 
        nivel++; 
        x = 30; // Mario vuelve al inicio
        alert(`¡Nivel ${nivel-1} Completado! Pregunta de examen: ¿Cuál es la fórmula del Activo? (A+P o P+P)`);
    }

    // Dibujar Mario
    ctx.fillStyle = 'red'; ctx.fillRect(x, y, 20, 20);
    requestAnimationFrame(gameLoop);
}
gameLoop();
