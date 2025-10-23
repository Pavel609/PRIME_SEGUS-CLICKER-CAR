# PRIME_SEGUS-CLICKER-CAR
CLICKER from yt prime_segus
<!doctype html>
<html lang="cs">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Auto Clicker — Klikací hra (1000 aut + hudba)</title>
<style>
  :root{--bg:#0f1724;--card:#0b1220;--accent:#ffb703;--muted:#9aa6b2;--glass:rgba(255,255,255,0.03)}
  body{font-family:Inter,system-ui,Segoe UI,Roboto,Helvetica,Arial; background:linear-gradient(180deg,#071428 0%, #0f1724 100%); color:#e6eef6; margin:0; padding:20px}
  .wrap{max-width:1150px;margin:0 auto;display:grid;grid-template-columns:1fr 420px;gap:20px}
  header{grid-column:1/-1;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
  h1{font-size:20px;margin:0}
  .panel{background:var(--card);border-radius:12px;padding:16px;box-shadow:0 6px 24px rgba(2,6,23,0.6);border:1px solid rgba(255,255,255,0.03)}
  .big-click{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:28px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));}
  .money{font-size:28px;font-weight:700;color:var(--accent)}
  .cps{color:var(--muted);margin-top:6px}
  button.click-btn{margin-top:18px;background:linear-gradient(180deg,#ff8a00,#ff6b00);border:none;color:#081225;padding:18px 22px;border-radius:12px;font-weight:700;font-size:20px;cursor:pointer;box-shadow:0 6px 18px rgba(255,107,0,0.18)}
  .shop{max-height:720px;overflow:auto;padding:8px;margin-top:12px;border-radius:8px;background:var(--glass);display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:8px}
  .car-card{display:flex;gap:10px;align-items:center;padding:10px;border-radius:8px;margin-bottom:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00));border:1px solid rgba(255,255,255,0.02)}
  .thumb{width:96px;height:64px;border-radius:6px;overflow:hidden;flex-shrink:0;display:flex;align-items:center;justify-content:center;background:#09111a}
  .logo{width:46px;height:46px;border-radius:6px;display:flex;align-items:center;justify-content:center;font-weight:700;background:rgba(255,255,255,0.02)}
  .meta{font-size:14px}
  .price{font-weight:700;color:var(--accent)}
  .controls{display:flex;gap:8px;align-items:center}
  input[type=search]{width:100%;padding:10px;border-radius:10px;border:none;background:rgba(255,255,255,0.02);color:inherit}
  .top-stats{display:flex;gap:12px;align-items:center}
  .small{font-size:12px;color:var(--muted)}
  .owned{font-size:13px;color:#94f9b8;font-weight:700}
  .buy-btn{background:#0ea5a2;border:none;color:#001821;padding:8px 10px;border-radius:8px;cursor:pointer;font-weight:700}
  .disabled{opacity:0.45;pointer-events:none}
  .footer{grid-column:1/-1;text-align:center;color:var(--muted);margin-top:14px;font-size:13px}
  .audio-controls{display:flex;gap:8px;align-items:center;margin-top:12px}
  @media(max-width:980px){.wrap{grid-template-columns:1fr;}.shop{max-height:420px}}
</style>
</head>
<body>
<div class="wrap">
<header>
  <h1>Auto Clicker — klikni pro peníze</h1>
  <div class="top-stats small">
    <div>Hráč: <strong>Pavel</strong></div>
    <div id="version">Auta: <strong id="totalCars">1000</strong></div>
  </div>
</header>

<section class="panel big-click">
  <div class="money" id="money">0 Kč</div>
  <div class="cps" id="cps">0 / sek</div>
  <button class="click-btn" id="clickBtn">🚗 Klikni!</button>
  <div style="width:100%;margin-top:16px;display:flex;gap:8px">
    <input id="search" type="search" placeholder="Hledat značku nebo název (např. Toyota, Model 12)"/>
    <select id="sort">
      <option value="price_asc">Nejnižší cena</option>
      <option value="price_desc">Nejvyšší cena</option>
      <option value="cps_desc">Nejvyšší CPS</option>
    </select>
  </div>

  <div class="panel" style="width:100%;margin-top:12px">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <div class="small">Obchody</div>
      <div class="small">Filtrovat a kupovat pasivní příjem</div>
    </div>
    <div class="shop" id="shop"></div>
  </div>

  <div class="audio-controls">
    <button class="buy-btn" id="audioToggle">🔊 Hudba: Zapnuto</button>
    <label class="small">Hlasitost</label>
    <input id="volume" type="range" min="0" max="1" step="0.01" value="0.25" />
  </div>
</section>

<aside class="panel">
  <h3 style="margin-top:0">Rychlý přehled</h3>
  <div style="margin-top:8px">
    <div class="small">Klikací zisk na klik</div>
    <div style="font-weight:700;font-size:20px;margin-top:6px" id="perClick">1 Kč</div>
  </div>

  <div style="margin-top:12px">
    <div class="small">Dostupné upgrady</div>
    <div style="display:flex;gap:8px;margin-top:8px">
      <button class="buy-btn" id="upgradeClick">Zvýšit klik (+1 Kč) — cena: <span id="upgradeClickPrice">100</span> Kč</button>
    </div>
  </div>

  <div style="margin-top:12px">
    <div class="small">Rychlé ovládání</div>
    <div style="display:flex;gap:8px;margin-top:8px">
      <button class="buy-btn" id="sellAll">Prodat všechno (50% návrat)</button>
    </div>
  </div>

  <div style="margin-top:12px">
    <div class="small">Statistiky</div>
    <div style="margin-top:6px" class="meta">
      Vlastněno: <span class="owned" id="ownedCount">0</span><br/>
      Peníze utraceno: <span id="spent">0</span> Kč
    </div>
  </div>
</aside>

<footer class="footer">Vytvořil Pavel Matuška — hra pro zábavu. Použité názvy značek jsou skutečné, loga nejsou oficiální.</footer>
</div>

<script>
// --- Brands ---
const brands = ["Toyota","Volkswagen","Ford","Honda","BMW","Mercedes-Benz","Audi","Hyundai","Nissan","Kia","Renault","Peugeot","Chevrolet","Mazda","Subaru","Volvo","Jaguar","Land Rover","Porsche","Ferrari","Lamborghini","Bentley","Rolls-Royce","Mitsubishi","Suzuki","Mini","Alfa Romeo","Citroen","Dacia","Tesla","Infiniti","Acura","Genesis","Saab","Skoda","Seat","Opel","Chrysler","Dodge","Jeep","GMC","Ram","Iveco","McLaren","Aston Martin","Pagani","Koenigsegg","Rivian","Lucid","Polestar","Mahindra","Tata","BYD"];

// --- Generate 1000 cars dynamically ---
function generateCars(count=1000){
  const cars = [];
  let id = 1;
  for(let i=0;i<count;i++){
    const brand = brands[i % brands.length];
    const modelNum = Math.floor(i/brands.length)+1;
    const modelSuffix = [""," GT"," Sport"," S"," X"," Plus"," RS"," Turbo"," Hybrid"," EV"," R"][i % 11];
    const name = brand + " Model " + modelNum + modelSuffix;
    const base = 50 + i*8; // lower scaling so prices aren't astronomic
    const price = Math.round(base * Math.pow(1.028, i));
    const cps = Math.max(1, Math.round(price / 3000));
    cars.push({ id: id++, brand, name, price, cps, owned:0 });
  }
  return cars;
}
let cars = generateCars(1000);

// --- Game state ---
let money = 0;
let perClick = 1;
let totalCPS = 0;
let spent = 0;

// --- UI refs ---
const moneyEl = document.getElementById('money');
const cpsEl = document.getElementById('cps');
const perClickEl = document.getElementById('perClick');
const shopEl = document.getElementById('shop');
const ownedCountEl = document.getElementById('ownedCount');
const spentEl = document.getElementById('spent');
const totalCarsEl = document.getElementById('totalCars');
totalCarsEl.textContent = cars.length;

function fmt(n){
  if(n>=1e9) return (n/1e9).toFixed(2)+' G';
  if(n>=1e6) return (n/1e6).toFixed(2)+' M';
  if(n>=1e3) return (n/1e3).toFixed(2)+' k';
  return Math.round(n);
}

function updateUI(){
  moneyEl.textContent = fmt(money) + ' Kč';
  cpsEl.textContent = fmt(totalCPS) + ' / sek';
  perClickEl.textContent = fmt(perClick) + ' Kč';
  ownedCountEl.textContent = cars.reduce((s,c)=>s+c.owned,0);
  spentEl.textContent = fmt(spent);
  renderShop();
}

// create a simple SVG thumbnail (data URL) for each car — acts as "photo" placeholder
function makeThumbSVG(brand, name, w=192, h=128){
  const bgColors = ['#0ea5a2','#ff8a00','#7c3aed','#06b6d4','#ef4444','#06b6d4','#8b5cf6','#f97316'];
  const bg = bgColors[(brand.length + name.length) % bgColors.length];
  const text = brand.split(' ')[0];
  const svg = `<?xml version='1.0' encoding='utf-8'?><svg xmlns='http://www.w3.org/2000/svg' width='${w}' height='${h}' viewBox='0 0 ${w} ${h}'>
    <defs>
      <linearGradient id='g' x1='0' x2='1'><stop offset='0' stop-color='${bg}' stop-opacity='0.9'/><stop offset='1' stop-color='#021124' stop-opacity='0.6'/></linearGradient>
    </defs>
    <rect width='100%' height='100%' fill='url(#g)' rx='8'/>
    <g transform='translate(8,8)'>
      <rect x='0' y='0' width='176' height='80' rx='6' fill='rgba(255,255,255,0.04)' />
      <text x='10' y='28' font-family='Inter,Arial' font-size='18' fill='white' font-weight='700'>${escapeXml(text)}</text>
      <text x='10' y='52' font-family='Inter,Arial' font-size='12' fill='rgba(255,255,255,0.85)'>${escapeXml(name)}</text>
    </g>
  </svg>`;
  return 'data:image/svg+xml;base64,' + btoa(svg);
}

function makeLogoSVG(brand,w=46,h=46){
  const initials = brand.split(' ').map(s=>s[0]).slice(0,2).join('').toUpperCase();
  const colors = ['#ffb703','#fb7185','#60a5fa','#34d399','#f472b6','#f59e0b','#a78bfa'];
  const bg = colors[brand.length % colors.length];
  const svg = `<?xml version='1.0' encoding='utf-8'?><svg xmlns='http://www.w3.org/2000/svg' width='${w}' height='${h}' viewBox='0 0 ${w} ${h}'>
    <rect width='100%' height='100%' rx='8' fill='${bg}'/>
    <text x='50%' y='52%' text-anchor='middle' font-family='Inter,Arial' font-size='18' fill='#021124' font-weight='800'>${escapeXml(initials)}</text>
  </svg>`;
  return 'data:image/svg+xml;base64,' + btoa(svg);
}

function escapeXml(unsafe){
  return unsafe.replace(/[&<>\"']/g, function(c){
    return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&apos;"}[c];
  });
}

function renderShop(){
  const q = document.getElementById('search').value.trim().toLowerCase();
  const sort = document.getElementById('sort').value;
  let list = cars.filter(c=> c.name.toLowerCase().includes(q) || c.brand.toLowerCase().includes(q));
  if(sort==='price_asc') list.sort((a,b)=>a.price-b.price);
  if(sort==='price_desc') list.sort((a,b)=>b.price-a.price);
  if(sort==='cps_desc') list.sort((a,b)=>b.cps-a.cps);
  shopEl.innerHTML = '';
  for(const c of list){
    const card = document.createElement('div'); card.className = 'car-card';
    const thumb = document.createElement('div'); thumb.className='thumb';
    const img = document.createElement('img'); img.src = makeThumbSVG(c.brand,c.name,192,128); img.alt = c.name; img.style.width='100%'; img.style.height='100%'; img.style.objectFit='cover';
    thumb.appendChild(img);
    const left = document.createElement('div'); left.style.flex='1';
    const logo = document.createElement('div'); logo.className='logo';
    const logoImg = document.createElement('img'); logoImg.src = makeLogoSVG(c.brand,46,46); logoImg.alt = c.brand; logoImg.style.width='100%'; logoImg.style.height='100%'; logo.appendChild(logoImg);
    const title = document.createElement('div'); title.className='meta'; title.innerHTML = `<strong>${c.name}</strong><div class='small'>${c.brand} • CPS: ${c.cps}</div>`;
    left.appendChild(logo);
    left.appendChild(title);
    const controls = document.createElement('div'); controls.className='controls';
    const price = document.createElement('div'); price.className='price'; price.textContent = fmt(c.price) + ' Kč';
    const btn = document.createElement('button'); btn.className='buy-btn'; btn.textContent='Koupit'; btn.dataset.id = c.id;
    btn.addEventListener('click', ()=> buyCar(c.id));
    controls.appendChild(price); controls.appendChild(btn);
    card.appendChild(thumb);
    card.appendChild(left);
    card.appendChild(controls);
    shopEl.appendChild(card);
  }
}

// --- Actions ---
document.getElementById('clickBtn').addEventListener('click', ()=> {
  money += perClick;
  updateUI();
});
document.getElementById('search').addEventListener('input', renderShop);
document.getElementById('sort').addEventListener('change', renderShop);
function buyCar(id){
  const car = cars.find(x=>x.id===id);
  if(!car) return;
  if(money < car.price){ alert('Nedostatek peněz!'); return; }
  money -= car.price;
  car.owned += 1;
  spent += car.price;
  totalCPS += car.cps;
  updateUI();
}

// upgrade click
let upgradeClickPrice = 100;
document.getElementById('upgradeClickPrice').textContent = fmt(upgradeClickPrice);
document.getElementById('upgradeClick').addEventListener('click', ()=> {
  if(money < upgradeClickPrice){ alert('Potřebuješ víc peněz.'); return; }
  money -= upgradeClickPrice;
  perClick += 1;
  upgradeClickPrice = Math.round(upgradeClickPrice * 2.2);
  document.getElementById('upgradeClickPrice').textContent = fmt(upgradeClickPrice);
  updateUI();
});
// sell all
document.getElementById('sellAll').addEventListener('click', ()=> {
  const owned = cars.reduce((s,c)=>s+c.owned,0);
  if(owned===0){ alert('Nic k prodeji.'); return; }
  let refund = 0;
  cars.forEach(c=> { if(c.owned>0){ refund += c.price * c.owned * 0.5; totalCPS -= c.cps * c.owned; c.owned = 0; } });
  money += Math.floor(refund);
  updateUI();
});
// passive income tick
setInterval(()=> {
  money += totalCPS;
  updateUI();
}, 1000);

// autosave to localStorage
function save(){
  const state = { money, perClick, totalCPS, spent, cars };
  try{ localStorage.setItem('auto_clicker_save_v2', JSON.stringify(state)); }catch(e){console.warn('Save failed',e);}
}
function load(){
  const s = localStorage.getItem('auto_clicker_save_v2');
  if(!s) return;
  try{
    const st = JSON.parse(s);
    money = st.money||money;
    perClick = st.perClick||perClick;
    totalCPS = st.totalCPS||totalCPS;
    spent = st.spent||spent;
    if(st.cars && st.cars.length===cars.length){
      cars.forEach((c,i)=> c.owned = st.cars[i].owned || 0);
    }
  }catch(e){ console.warn('Load failed',e); }
}
window.addEventListener('beforeunload', save);
document.addEventListener('visibilitychange', ()=> { if(document.visibilityState==='hidden') save(); });
// keyboard shortcuts
document.addEventListener('keydown',(e)=>{
  if(e.key===' ') { e.preventDefault(); document.getElementById('clickBtn').click(); }
  if(e.key==='s' && (e.ctrlKey||e.metaKey)){ e.preventDefault(); save(); alert('Uloženo'); }
});
// initial
load();
updateUI();

// --- Embedded chill music using WebAudio (no external files) ---
let audioCtx, masterGain, isPlaying=false;
function initAudio(){
  if(audioCtx) return;
  audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  masterGain = audioCtx.createGain();
  masterGain.gain.value = Number(document.getElementById('volume').value || 0.25);
  masterGain.connect(audioCtx.destination);

  // soft pads: two detuned oscillators with slow LFO
  const osc1 = audioCtx.createOscillator(); osc1.type='sine'; osc1.frequency.value = 110; // A2
  const osc2 = audioCtx.createOscillator(); osc2.type='sine'; osc2.frequency.value = 110 * 1.005; // slight detune
  const padGain = audioCtx.createGain(); padGain.gain.value = 0.0; // start silent

  osc1.connect(padGain); osc2.connect(padGain); padGain.connect(masterGain);

  // slow ADSR envelope loop
  function fadeInOut(){
    const t = audioCtx.currentTime;
    padGain.gain.cancelScheduledValues(t);
    padGain.gain.setValueAtTime(0.0,t);
    padGain.gain.linearRampToValueAtTime(0.12,t+6);
    padGain.gain.linearRampToValueAtTime(0.08,t+16);
    padGain.gain.linearRampToValueAtTime(0.12,t+26);
  }
  setInterval(fadeInOut, 25000);
  fadeInOut();

  // gentle noise (wind) using buffer
  const bufferSize = 2*audioCtx.sampleRate;
  const noiseBuffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
  const output = noiseBuffer.getChannelData(0);
  for (let i = 0; i < bufferSize; i++) output[i] = (Math.random()*2-1)*0.15;
  const noise = audioCtx.createBufferSource(); noise.buffer = noiseBuffer; noise.loop=true;
  const noiseFilter = audioCtx.createBiquadFilter(); noiseFilter.type='lowpass'; noiseFilter.frequency.value=900;
  const noiseGain = audioCtx.createGain(); noiseGain.gain.value = 0.04;
  noise.connect(noiseFilter); noiseFilter.connect(noiseGain); noiseGain.connect(masterGain);

  // subtle detune LFO
  const lfo = audioCtx.createOscillator(); lfo.frequency.value = 0.1; // very slow
  const lfoGain = audioCtx.createGain(); lfoGain.gain.value = 0.5;
  lfo.connect(lfoGain);
  lfoGain.connect(osc2.frequency);

  // reverb using convolver with short impulse
  const convolver = audioCtx.createConvolver();
  const irLength = audioCtx.sampleRate * 2.0;
  const ir = audioCtx.createBuffer(2, irLength, audioCtx.sampleRate);
  for(let ch=0; ch<2; ch++){
    const irData = ir.getChannelData(ch);
    for(let i=0;i<irLength;i++) irData[i] = (Math.random()*2-1)*Math.pow(1 - i/irLength, 2);
  }
  convolver.buffer = ir;

  // connect padGain -> convolver -> master
  const wetGain = audioCtx.createGain(); wetGain.gain.value = 0.25;
  padGain.connect(convolver);
  convolver.connect(wetGain);
  wetGain.connect(masterGain);

  // start sources
  osc1.start(); osc2.start(); noise.start(); lfo.start();

  // store refs
  audioCtx._refs = {osc1,osc2,noise,lfo,padGain,noiseGain,wetGain};
}

function startAudio(){
  if(!audioCtx) initAudio();
  if(audioCtx.state === 'suspended') audioCtx.resume();
  isPlaying = true;
  document.getElementById('audioToggle').textContent = '🔊 Hudba: Zapnuto';
}
function stopAudio(){
  if(!audioCtx) return;
  try{ audioCtx.suspend(); }catch(e){}
  isPlaying = false;
  document.getElementById('audioToggle').textContent = '🔈 Hudba: Vypnuto';
}

// toggle button
const audioToggleBtn = document.getElementById('audioToggle');
audioToggleBtn.addEventListener('click', ()=>{
  if(!audioCtx) initAudio();
  if(!isPlaying){ startAudio(); } else { stopAudio(); }
});

// volume control
const vol = document.getElementById('volume');
vol.addEventListener('input', ()=>{ if(masterGain) masterGain.gain.value = Number(vol.value); });

// auto-start music silently allowed by browsers? we wait for first user gesture (click) to enable play.
// To make music "there" right away without external file, we initialize audio on first interaction.
function ensureAudioOnFirstGesture(){
  function handler(){
    if(!audioCtx) initAudio();
    // start but keep suspended until user toggles explicitly; we'll start muted briefly to satisfy gesture
    document.removeEventListener('click', handler);
  }
  document.addEventListener('click', handler);
}
ensureAudioOnFirstGesture();

</script>
</body>
</html>
