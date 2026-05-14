<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>C O ₂ 网格 · 鼠标光晕气泡</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.min.js"></script>
  <style>
    body {
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background: #000;
    }
  </style>
</head>
<body>
<script>
/**
 * 仅当鼠标在画布内且发生移动时在指针附近生成气泡；
 * 气泡为 rgb(255,60,0) 径向渐变光晕，边缘透明虚化；
 * 文字整体运动更慢。
 */

const W = 800;
const H = 600;

const CHAR_POOL = ['C', 'O', '₂'];

let fontSize = 15;
let cellW;
let cellH;
let cols;
let rows;
let charCells = [];

const BUBBLE_COLOR = [255, 60, 0];
const MAX_BUBBLES = 72;

/** 文字：更慢的下落、回弹与气泡推挤 */
const GRAVITY = 0.095;
const LIFT_STRENGTH = 0.38;
const GLYPH_FRICTION = 0.982;
const ANCHOR_PULL = 0.011;
const CONTACT_SECONDS = 2.2;
const CONTACT_FRAMES = Math.round(CONTACT_SECONDS * 60);

const MOUSE_LETTER_R = 42;
const MOUSE_SPAWN_JITTER = 38;

let bubbleLayer;
let strandLayer;

function mouseInCanvas() {
  return mouseX >= 0 && mouseX < W && mouseY >= 0 && mouseY < H;
}

function spawnBubbleNear(mx, my) {
  if (bubbles.length >= MAX_BUBBLES) return;
  let r;
  const u = random();
  if (u < 0.52) r = random(3.2, 11);
  else if (u < 0.82) r = random(8, 18);
  else r = random(12, 26);
  r = constrain(r, 3, 28);
  const x = constrain(mx + random(-MOUSE_SPAWN_JITTER, MOUSE_SPAWN_JITTER), r + 2, W - r - 2);
  const y = constrain(my + random(-MOUSE_SPAWN_JITTER, MOUSE_SPAWN_JITTER), r + 2, H - r - 2);
  bubbles.push(new Bubble(x, y, r));
}

/** 仅在画布内且指针有位移时生泡（静止不产生） */
function tickMouseBubbleSpawner() {
  if (!mouseInCanvas()) return;
  const step = dist(mouseX, mouseY, pmouseX, pmouseY);
  if (step < 0.45) return;
  const chance = min(0.72, 0.12 + step * 0.09);
  if (random() < chance) spawnBubbleNear(mouseX, mouseY);
}

function shuffleInPlace(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    const j = floor(random(i + 1));
    const t = arr[i];
    arr[i] = arr[j];
    arr[j] = t;
  }
  return arr;
}

function measureCellSize() {
  textSize(fontSize);
  const w1 = max(textWidth('C'), textWidth('O'));
  textSize(fontSize * 0.62);
  const w2 = textWidth('₂');
  const cw = max(w1, w2) + 8;
  const ch = fontSize * 1.45;
  return { cw, ch };
}

function pickRandomChar() {
  return random(CHAR_POOL);
}

class CharCell {
  constructor(col, row, ox, oy, initialCh) {
    this.col = col;
    this.row = row;
    this.origX = ox;
    this.origY = oy;
    this.ch = initialCh;
    this.isSub = initialCh === '₂';
    this.x = ox + random(-3, 3);
    this.y = oy + random(-3, 3);
    this.vx = 0;
    this.vy = 0;
    this.contactFrames = 0;
    this.visible = true;
    this.opacity = 255;
  }

  applyBubbleForces(bubbles, hitOut) {
    const tr = this.isSub ? fontSize * 0.34 : fontSize * 0.4;
    for (const b of bubbles) {
      const dx = this.x - b.x;
      const dy = this.y - b.y;
      const d = Math.hypot(dx, dy);
      const reach = b.collisionRadius() + tr;
      if (d < reach) {
        hitOut.v = true;
        const pen = reach - d;
        const nx = dx / (d + 1e-4);
        const ny = dy / (d + 1e-4);
        const buoy = LIFT_STRENGTH * pen * 0.088;
        this.vx += nx * buoy * 0.42 * 0.48;
        this.vy -= abs(ny) * buoy * 1.1 * 0.48 + buoy * 0.32 * 0.48;
        const tx = -ny;
        const ty = nx;
        const tang = b.vy * 0.055 * 0.5;
        this.vx += tx * tang;
        this.vy += ty * tang * 0.18;
      }
    }
  }

  originTouchedByBubble(bubbles) {
    for (const b of bubbles) {
      if (dist(b.x, b.y, this.origX, this.origY) < b.collisionRadius() + cellW * 0.38) {
        return true;
      }
    }
    return false;
  }

  mouseTouchingLetter() {
    return mouseInCanvas() && dist(this.x, this.y, mouseX, mouseY) < MOUSE_LETTER_R;
  }

  update(bubbles) {
    if (!this.visible) {
      if (this.originTouchedByBubble(bubbles)) {
        this.contactFrames++;
        if (this.contactFrames >= CONTACT_FRAMES) this.regenerateAtOrigin();
      } else {
        this.contactFrames = 0;
      }
      return;
    }

    const hit = { v: false };
    this.applyBubbleForces(bubbles, hit);

    const mouseHit = this.mouseTouchingLetter();
    if (mouseHit) {
      this.vy += GRAVITY * 2.1;
      this.vx += random(-0.22, 0.22);
    }

    if (hit.v) {
      this.contactFrames++;
      if (this.contactFrames >= CONTACT_FRAMES) {
        this.regenerateAtOrigin();
        return;
      }
    } else {
      this.contactFrames = 0;
      if (!mouseHit) this.vy += GRAVITY;
    }

    const pull = mouseHit ? ANCHOR_PULL * 0.12 : ANCHOR_PULL;
    this.vx += (this.origX - this.x) * pull;
    this.vy += (this.origY - this.y) * pull;
    this.vx *= GLYPH_FRICTION;
    this.vy *= GLYPH_FRICTION;
    this.x += this.vx;
    this.y += this.vy;

    if (this.y > H + cellH * 0.7) {
      this.visible = false;
      this.opacity = 0;
      this.contactFrames = 0;
    }
  }

  regenerateAtOrigin() {
    this.ch = pickRandomChar();
    this.isSub = this.ch === '₂';
    this.x = this.origX + random(-2, 2);
    this.y = this.origY + random(-2, 2);
    this.vx = this.vy = 0;
    this.contactFrames = 0;
    this.visible = true;
    this.opacity = 255;
  }

  drawCell() {
    if (!this.visible) return;
    push();
    translate(this.x, this.y);
    textAlign(CENTER, CENTER);
    noStroke();
    fill(255, this.opacity);
    if (this.isSub) {
      textSize(fontSize * 0.62);
      translate(0, fontSize * 0.28);
    } else {
      textSize(fontSize);
    }
    text(this.ch, 0, 0);
    pop();
  }
}

class Bubble {
  constructor(x, y, r) {
    this.x = x;
    this.y = y;
    this.r = r;
    this.vx = 0;
    const rf = constrain(r, 2.8, 52);
    const upBase = pow(26 / rf, 0.92) * 1.15 * 0.62;
    this.vy = -upBase * random(0.58, 1.35);
    this.rising = true;
    this.splitCooldown = 0;
    this.morphClock = random(4);
    /** 整体不透明度系数（70%～100%），与渐变 stops 相乘 */
    this.fillAlpha = random(0.7, 1.0) * 255;
  }

  collisionRadius() {
    return this.r * 1.05;
  }

  rCircle(a) {
    return 1;
  }

  rEllipse(a) {
    const ex = 1.32;
    const ey = 0.78;
    return 1 / sqrt(sq(cos(a) / ex) + sq(sin(a) / ey));
  }

  rCashew(a) {
    return (
      0.9 +
      0.14 * cos(a - 0.55) +
      0.11 * cos(2 * a + 0.35) +
      0.06 * sin(3 * a - 0.2)
    );
  }

  rDroplet(a) {
    const u = a + HALF_PI;
    const tear = 0.62 + 0.38 * (1 - cos(u));
    return tear + 0.05 * sin(2 * u + 0.4);
  }

  baseShapeRadius(a) {
    const mc = this.morphClock % 4;
    const seg = floor(mc);
    const t = mc - seg;
    const e = t * t * (3 - 2 * t);
    const rc = this.rCircle(a);
    const re = this.rEllipse(a);
    const rk = this.rCashew(a);
    const rd = this.rDroplet(a);
    if (seg === 0) return lerp(rc, re, e);
    if (seg === 1) return lerp(re, rk, e);
    if (seg === 2) return lerp(rk, rd, e);
    return lerp(rd, rc, e);
  }

  radiusAt(angle) {
    const rad = this.baseShapeRadius(angle);
    return this.r * constrain(rad, 0.62, 1.45);
  }

  /** 光晕外缘半径（大于碰撞半径，用于渐变铺色） */
  haloOuterR() {
    return this.r * 2.45;
  }

  update() {
    this.morphClock += 0.01;
    if (this.morphClock >= 4) this.morphClock -= 4;

    const kDrag = 0.082;
    const rf = constrain(this.r, 2.8, 52);
    const riseMag = (0.95 + 3.15 * pow(13 / rf, 0.88)) * 0.68;
    const fallMag = (0.62 + 2.45 * pow(13 / rf, 0.78)) * 0.68;
    const vTermRise = -riseMag;
    const vTermFall = fallMag;

    if (this.rising && this.y - this.collisionRadius() < 26) {
      this.rising = false;
      this.vy = max(0.45, abs(this.vy) * random(0.9, 1.12));
    }
    if (!this.rising && this.y + this.collisionRadius() > H + 38) {
      this.rising = true;
      this.vy = min(-0.48, -abs(this.vy) * random(0.95, 1.18));
    }

    if (this.rising) this.vy += (vTermRise - this.vy) * kDrag;
    else this.vy += (vTermFall - this.vy) * kDrag;

    this.y += this.vy;
  }

  displayIntoLayer(g) {
    const R = this.haloOuterR();
    const cr = BUBBLE_COLOR[0];
    const cg = BUBBLE_COLOR[1];
    const cb = BUBBLE_COLOR[2];
    const a0 = (this.fillAlpha / 255) * 0.78;
    const a1 = (this.fillAlpha / 255) * 0.4;
    const a2 = (this.fillAlpha / 255) * 0.14;
    const a3 = (this.fillAlpha / 255) * 0.04;

    g.push();
    g.translate(this.x, this.y);
    const ctx = g.drawingContext;
    const grd = ctx.createRadialGradient(0, 0, 0, 0, 0, R);
    grd.addColorStop(0, `rgba(${cr},${cg},${cb},${a0.toFixed(4)})`);
    grd.addColorStop(0.28, `rgba(${min(255, cr + 18)},${min(255, cg + 28)},${min(255, cb + 8)},${a1.toFixed(4)})`);
    grd.addColorStop(0.55, `rgba(${min(255, cr + 35)},${min(255, cg + 55)},${min(255, cb + 18)},${a2.toFixed(4)})`);
    grd.addColorStop(0.82, `rgba(${min(255, cr + 20)},${min(255, cg + 35)},${min(255, cb + 10)},${a3.toFixed(4)})`);
    grd.addColorStop(1, `rgba(${cr},${cg},${cb},0)`);
    ctx.fillStyle = grd;
    ctx.beginPath();
    ctx.arc(0, 0, R, 0, Math.PI * 2);
    ctx.fill();
    g.pop();
  }
}

let bubbles = [];

/** 在独立图层上绘制拉丝，最后叠在主画布文字之上，保证盖住 C/O/₂ */
function drawBubbleStrands(arr, g) {
  if (!g || !arr || arr.length < 2) return;
  const MERGE_K = 0.74;
  g.push();
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      const A = arr[i];
      const B = arr[j];
      const d = dist(A.x, A.y, B.x, B.y);
      const sumR = A.collisionRadius() + B.collisionRadius();
      const dMerge = sumR * MERGE_K;
      if (d <= dMerge) continue;
      const dMax = sumR * 2.38;
      if (d >= dMax) continue;

      const span = dMax - dMerge;
      const u = constrain((d - dMerge) / span, 0, 1);
      const strength = sin((1 - u) * PI);

      const hx = (B.x - A.x) / (d + 1e-5);
      const hy = (B.y - A.y) / (d + 1e-5);
      const px = -hy;
      const py = hx;

      const surfA = A.r * 0.9;
      const surfB = B.r * 0.9;
      const sagAmp = span * 0.14 * strength;
      const phase = frameCount * 0.009 + i * 0.37 + j * 0.29;

      const pairW = 0.55;
      const offs = [-pairW, pairW];
      for (let s = 0; s < offs.length; s++) {
        const w = offs[s];
        const ax = A.x + hx * surfA + px * w;
        const ay = A.y + hy * surfA + py * w;
        const bx = B.x - hx * surfB + px * w;
        const by = B.y - hy * surfB + py * w;
        const sag = sagAmp * sin(phase + s * 0.9);
        const c1x = lerp(ax, bx, 0.3) + px * sag;
        const c1y = lerp(ay, by, 0.3) + py * sag;
        const c2x = lerp(ax, bx, 0.7) - px * sag * 0.38;
        const c2y = lerp(ay, by, 0.7) - py * sag * 0.38;

        const tPair = constrain(noise(i * 0.31, j * 0.29, s * 0.7), 0, 1);
        const alphaMax = lerp(0.22, 0.36, tPair) * 255 * strength;
        g.stroke(BUBBLE_COLOR[0], BUBBLE_COLOR[1], BUBBLE_COLOR[2], alphaMax);
        g.strokeWeight(0.55 + strength * 1.1);
        g.noFill();
        g.bezier(ax, ay, c1x, c1y, c2x, c2y, bx, by);
      }
    }
  }
  g.pop();
}

function partitionCharCounts(n, maxSpread) {
  let nC = floor(n / 3);
  let nO = floor(n / 3);
  let n2 = n - nC - nO;
  const steps = min(400, max(30, n));
  for (let step = 0; step < steps; step++) {
    if (random() < 0.015) break;
    const from = floor(random(3));
    const to = floor(random(3));
    if (from === to) continue;
    const cur = [nC, nO, n2];
    if (cur[from] <= 1) continue;
    cur[from]--;
    cur[to]++;
    const hi = Math.max(cur[0], cur[1], cur[2]);
    const lo = Math.min(cur[0], cur[1], cur[2]);
    if (hi - lo <= maxSpread) {
      nC = cur[0];
      nO = cur[1];
      n2 = cur[2];
    }
  }
  return [nC, nO, n2];
}

function buildTextGrid() {
  charCells = [];
  const { cw, ch } = measureCellSize();
  cellW = cw;
  cellH = ch;
  cols = floor(W / cellW);
  rows = floor((H - 38) / cellH);
  const slots = [];
  for (let j = 0; j < rows; j++) {
    for (let i = 0; i < cols; i++) {
      const ox = i * cellW + cellW * 0.5;
      const oy = j * cellH + cellH * 0.5 + 36;
      if (random() < 0.04) continue;
      slots.push({ i, j, ox, oy });
    }
  }

  const n = slots.length;
  let bag = [];
  if (n === 0) return;
  if (n < 3) {
    for (let k = 0; k < n; k++) bag.push(pickRandomChar());
  } else {
    const MAX_COUNT_SPREAD = 50;
    const [nC, nO, n2] = partitionCharCounts(n, MAX_COUNT_SPREAD);
    bag = [
      ...Array(nC).fill('C'),
      ...Array(nO).fill('O'),
      ...Array(n2).fill('₂'),
    ];
    shuffleInPlace(bag);
  }

  for (let k = 0; k < n; k++) {
    const s = slots[k];
    charCells.push(new CharCell(s.i, s.j, s.ox, s.oy, bag[k] ?? pickRandomChar()));
  }
}

function mergeBubbles() {
  const maxPasses = 10;
  for (let pass = 0; pass < maxPasses; pass++) {
    let merged = false;
    outer: for (let i = 0; i < bubbles.length; i++) {
      for (let j = i + 1; j < bubbles.length; j++) {
        const A = bubbles[i];
        const B = bubbles[j];
        const d = dist(A.x, A.y, B.x, B.y);
        if (d < (A.collisionRadius() + B.collisionRadius()) * 0.74) {
          const newR = sqrt(A.r * A.r + B.r * B.r) * 1.04;
          const nx = (A.x * A.r + B.x * B.r) / (A.r + B.r);
          const ny = (A.y * A.r + B.y * B.r) / (A.r + B.r);
          const M = new Bubble(nx, ny, constrain(newR, 5, 96));
          M.vx = 0;
          M.vy = (A.vy * A.r + B.vy * B.r) / (A.r + B.r);
          M.rising = A.y < B.y ? A.rising : B.rising;
          M.morphClock = ((A.morphClock + B.morphClock) * 0.5) % 4;
          M.fillAlpha = constrain(
            (A.fillAlpha * A.r + B.fillAlpha * B.r) / (A.r + B.r),
            0.7 * 255,
            255
          );
          bubbles.splice(j, 1);
          bubbles.splice(i, 1);
          bubbles.push(M);
          merged = true;
          break outer;
        }
      }
    }
    if (!merged) break;
  }
}

function splitBubbles() {
  for (let i = bubbles.length - 1; i >= 0; i--) {
    const b = bubbles[i];
    if (b.splitCooldown > 0) {
      b.splitCooldown--;
      continue;
    }
    if (b.r > 35 && random() < 0.0032) {
      const r1 = b.r * random(0.46, 0.54);
      const r2 = b.r * random(0.46, 0.54);
      const off = b.r * 0.3;
      const c1 = new Bubble(b.x - off, b.y, r1);
      const c2 = new Bubble(b.x + off, b.y, r2);
      c1.vx = 0;
      c2.vx = 0;
      c1.vy = b.vy * random(0.9, 1.08);
      c2.vy = b.vy * random(0.9, 1.08);
      c1.rising = c2.rising = b.rising;
      c1.morphClock = b.morphClock;
      c2.morphClock = (b.morphClock + 0.28) % 4;
      c1.splitCooldown = c2.splitCooldown = 85;
      bubbles.splice(i, 1);
      bubbles.push(c1, c2);
    }
  }
}

function setup() {
  createCanvas(W, H);
  bubbleLayer = createGraphics(W, H);
  strandLayer = createGraphics(W, H);
  randomSeed(floor(random(1e9)));
  textFont('sans-serif');
  buildTextGrid();
}

function draw() {
  background(0);

  bubbleLayer.clear();
  for (const b of bubbles) {
    b.update();
    b.displayIntoLayer(bubbleLayer);
  }
  mergeBubbles();
  splitBubbles();

  bubbles = bubbles.filter((b) => b.x > -130 && b.x < W + 130);

  tickMouseBubbleSpawner();

  image(bubbleLayer, 0, 0);

  for (const cell of charCells) {
    cell.update(bubbles);
    cell.drawCell();
  }

  strandLayer.clear();
  drawBubbleStrands(bubbles, strandLayer);
  image(strandLayer, 0, 0);
}

function windowResized() {}
</script>
</body>
</html>
