Rift Fall Game design code - <!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Parallel Worlds — Fracture</title>

<style>
:root{
  font-family:Inter,system-ui,Segoe UI,Arial,sans-serif;
  color:#edf7ff;
  background:#050812
}

*{box-sizing:border-box}

html,body{
  margin:0;
  width:100%;
  height:100%;
  overflow:hidden;
  background:#050812
}

canvas{display:block}

#menu{
  position:fixed;
  inset:0;
  display:flex;
  align-items:center;
  justify-content:center;
  z-index:50;
  background:
    radial-gradient(
      circle at 50% 40%,
      rgba(57,121,181,.25),
      rgba(2,6,15,.97) 65%
    )
}

.card{
  width:min(820px,92vw);
  padding:34px;
  border:1px solid rgba(168,226,255,.25);
  border-radius:24px;
  background:rgba(7,13,25,.84);
  box-shadow:0 30px 100px rgba(0,0,0,.55);
  backdrop-filter:blur(12px)
}

.kicker{
  text-transform:uppercase;
  letter-spacing:.22em;
  font-size:11px;
  opacity:.62
}

.title{
  font-size:56px;
  font-weight:950;
  line-height:.94;
  letter-spacing:-.05em;
  margin:10px 0 14px
}

.title span{
  opacity:.45;
  font-weight:600
}

.intro{
  font-size:16px;
  line-height:1.6;
  max-width:740px;
  opacity:.82
}

.grid{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:8px;
  margin:24px 0
}

.grid div{
  padding:10px 11px;
  border:1px solid rgba(255,255,255,.09);
  border-radius:11px;
  background:rgba(255,255,255,.025);
  font-size:12px
}

.grid b{
  display:block;
  font-size:12px;
  margin-bottom:4px
}

button{
  border:0;
  border-radius:12px;
  padding:14px 20px;
  background:#e7f7ff;
  color:#07111a;
  font-weight:900;
  cursor:pointer;
  font-size:15px
}

button:hover{
  transform:translateY(-1px);
  filter:brightness(1.05)
}

#hud{
  position:fixed;
  inset:0;
  display:none;
  pointer-events:none;
  z-index:20
}

.top{
  position:absolute;
  left:16px;
  right:16px;
  top:14px;
  display:flex;
  justify-content:space-between;
  align-items:flex-start;
  gap:12px
}

.panel{
  background:rgba(4,9,18,.72);
  border:1px solid rgba(255,255,255,.1);
  box-shadow:0 10px 30px rgba(0,0,0,.22);
  backdrop-filter:blur(9px);
  border-radius:14px
}

.mission{
  width:min(500px,58vw);
  padding:13px 15px
}

.eyebrow{
  font-size:9px;
  letter-spacing:.18em;
  text-transform:uppercase;
  opacity:.5
}

.mission strong{
  display:block;
  font-size:17px;
  margin-top:5px
}

.mission p{
  margin:4px 0 0;
  font-size:12px;
  opacity:.72;
  line-height:1.38
}

.stats{
  min-width:250px;
  padding:12px 14px
}

.row{
  display:flex;
  justify-content:space-between;
  font-size:11px;
  margin:5px 0
}

.bar{
  height:7px;
  background:#172231;
  border-radius:99px;
  overflow:hidden
}

.bar i{
  display:block;
  height:100%;
  width:100%;
  border-radius:99px;
  background:linear-gradient(90deg,#7de7ff,#9bffd6)
}

#prompt{
  position:absolute;
  left:50%;
  bottom:112px;
  transform:translateX(-50%);
  padding:10px 14px;
  border-radius:12px;
  background:rgba(2,6,13,.86);
  border:1px solid rgba(134,220,255,.28);
  font-size:13px;
  opacity:0;
  transition:opacity .12s;
  white-space:nowrap
}

.key{
  display:inline-block;
  padding:2px 7px;
  border-radius:6px;
  background:#eaf8ff;
  color:#06111a;
  font-weight:950;
  margin-right:7px
}

#cross{
  position:absolute;
  left:50%;
  top:50%;
  width:24px;
  height:24px;
  transform:translate(-50%,-50%)
}

#cross:before,
#cross:after{
  content:"";
  position:absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%);
  background:rgba(255,255,255,.9)
}

#cross:before{
  height:20px;
  width:2px
}

#cross:after{
  height:2px;
  width:20px
}

#weapon{
  position:absolute;
  right:16px;
  bottom:12px;
  width:min(245px,calc(100vw - 32px));
  max-height:calc(100vh - 112px);
  overflow:auto;
  padding:12px 14px
}

.weaponName{
  font-size:20px;
  font-weight:950
}

.ammo{
  font-size:27px;
  font-weight:950;
  margin-top:2px
}

.weaponInfo{
  font-size:10px;
  text-transform:uppercase;
  letter-spacing:.14em;
  opacity:.48
}

.weaponSlot{
  display:flex;
  gap:6px;
  margin-top:8px;
  flex-wrap:wrap
}

.weaponSlot span{
  padding:4px 6px;
  border-radius:6px;
  background:rgba(255,255,255,.05);
  font-size:10px
}

.weaponSlot .sel{
  outline:1px solid rgba(170,236,255,.55);
  background:rgba(112,211,255,.12)
}

#dialogue{
  position:absolute;
  left:50%;
  bottom:20px;
  transform:translateX(-50%);
  width:min(820px,90vw);
  padding:16px 18px;
  display:none
}

.speaker{
  font-weight:950;
  letter-spacing:.14em;
  text-transform:uppercase;
  font-size:11px;
  opacity:.62
}

.line{
  font-size:17px;
  line-height:1.5;
  margin-top:5px
}

.note{
  font-size:10px;
  opacity:.45;
  margin-top:8px
}

#toast{
  position:absolute;
  left:50%;
  top:18%;
  transform:translateX(-50%);
  padding:10px 15px;
  border-radius:11px;
  background:rgba(8,13,24,.9);
  border:1px solid rgba(127,232,255,.25);
  font-size:12px;
  opacity:0;
  transition:opacity .15s;
  max-width:min(700px,90vw);
  text-align:center
}

.worldBadge{
  position:absolute;
  left:50%;
  top:19px;
  transform:translateX(-50%);
  padding:7px 12px;
  border-radius:999px;
  font-size:10px;
  font-weight:900;
  letter-spacing:.18em;
  text-transform:uppercase;
  background:rgba(7,13,24,.65);
  border:1px solid rgba(255,255,255,.12)
}

#flash,
#hitFlash{
  position:fixed;
  inset:0;
  pointer-events:none;
  opacity:0
}

#flash{
  background:#dffaff;
  z-index:15
}

#hitFlash{
  background:
    radial-gradient(
      circle,
      transparent 35%,
      rgba(255,40,80,.42)
    );
  z-index:16
}

#ending{
  position:fixed;
  inset:0;
  z-index:60;
  display:none;
  align-items:center;
  justify-content:center;
  background:rgba(2,5,12,.95)
}

.end{
  width:min(820px,92vw);
  padding:38px;
  text-align:center;
  border:1px solid rgba(170,230,255,.18);
  border-radius:22px;
  background:rgba(7,13,25,.9)
}

.end h1{
  font-size:42px;
  margin:10px 0
}

.end p{
  opacity:.76;
  line-height:1.6
}

.choices{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:10px;
  margin-top:24px
}

.choices button{
  min-width:170px
}

.sideQuest{position:absolute;left:16px;bottom:16px;width:min(330px,42vw);padding:12px 14px;display:none;pointer-events:none}
.sideQuest strong{display:block;font-size:14px;margin-top:4px}.sideQuest p{margin:4px 0 0;font-size:11px;opacity:.68;line-height:1.4}.sideQuest .sqProgress{margin-top:8px;font-size:10px;opacity:.6;letter-spacing:.08em;text-transform:uppercase}

.compass{
  position:absolute;
  left:50%;
  top:58px;
  transform:translateX(-50%);
  width:310px;
  height:62px;
  padding:8px 11px;
  display:flex;
  align-items:center;
  gap:12px;
  background:rgba(4,9,18,.76)
}

.compassTarget{
  min-width:112px;
  display:flex;
  flex-direction:column;
  gap:2px
}

.compassTarget b{
  font-size:12px;
  white-space:nowrap;
  overflow:hidden;
  text-overflow:ellipsis
}

.compassTarget span:last-child{
  font-size:10px;
  opacity:.58
}

.compassDial{
  position:relative;
  flex:1;
  height:38px;
  overflow:hidden;
  border:1px solid rgba(255,255,255,.08);
  border-radius:9px;
  background:
    linear-gradient(
      90deg,
      rgba(99,218,255,.08),
      rgba(255,255,255,.015),
      rgba(99,218,255,.08)
    );
  display:flex;
  align-items:center;
  justify-content:space-around;
  font-size:10px;
  font-weight:900;
  opacity:.85
}

.compassDial:before{
  content:"";
  position:absolute;
  left:0;
  right:0;
  top:50%;
  height:1px;
  background:rgba(255,255,255,.12)
}

.compassArrow{
  position:absolute;
  left:50%;
  top:1px;
  transform:translateX(-50%);
  font-size:22px;
  line-height:28px;
  color:#72efff;
  text-shadow:0 0 12px rgba(114,239,255,.75);
  transform-origin:50% 18px;
  transition:transform .08s linear
}

.guideOverlay{
  position:fixed;
  inset:0;
  z-index:55;
  display:none;
  align-items:center;
  justify-content:center;
  background:rgba(2,5,12,.72);
  backdrop-filter:blur(12px)
}

.guidePanel{
  width:min(980px,94vw);
  max-height:88vh;
  overflow:auto;
  padding:24px;
  border:1px solid rgba(139,227,255,.22);
  border-radius:22px;
  background:
    linear-gradient(
      135deg,
      rgba(8,18,31,.97),
      rgba(8,11,21,.96)
    );
  box-shadow:0 30px 100px rgba(0,0,0,.6)
}

.guideHead{
  display:flex;
  justify-content:space-between;
  align-items:flex-start;
  gap:20px
}

.guideHead h2{
  font-size:31px;
  margin:5px 0 0;
  letter-spacing:-.03em
}

.guideHead button{
  background:rgba(255,255,255,.08);
  color:#e8f8ff;
  border:1px solid rgba(255,255,255,.12);
  padding:9px 13px;
  font-size:11px
}

.guideHead button span{
  padding:2px 6px;
  margin-left:6px;
  background:#e7f7ff;
  color:#07111a;
  border-radius:5px
}

.guideGrid{
  display:grid;
  grid-template-columns:1.05fr .95fr;
  gap:22px;
  margin-top:20px
}

.guideGrid section{
  border:1px solid rgba(255,255,255,.08);
  border-radius:16px;
  padding:16px;
  background:rgba(255,255,255,.025)
}

.guideGrid h3{
  font-size:10px;
  letter-spacing:.17em;
  opacity:.48;
  margin:0 0 10px
}

.guideCurrent{
  padding:13px;
  border-radius:12px;
  background:rgba(91,225,255,.07);
  border:1px solid rgba(91,225,255,.14);
  margin-bottom:16px
}

.guideCurrent b{
  font-size:18px
}

.guideCurrent p{
  font-size:12px;
  opacity:.7;
  margin:6px 0 0
}

.guideSteps{
  display:grid;
  gap:7px
}

.guideStep{
  display:flex;
  gap:9px;
  align-items:center;
  padding:8px 10px;
  border-radius:9px;
  background:rgba(255,255,255,.025);
  font-size:11px
}

.guideStep .n{
  width:22px;
  height:22px;
  border-radius:7px;
  display:grid;
  place-items:center;
  background:rgba(255,255,255,.07);
  font-weight:900
}

.guideStep.current{
  outline:1px solid rgba(115,236,255,.3);
  background:rgba(92,218,255,.08)
}

.guideStep.done{
  opacity:.45
}

.controlList{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:7px;
  margin-bottom:16px
}

.controlList div{
  display:flex;
  flex-direction:column;
  gap:3px;
  padding:9px 10px;
  background:rgba(255,255,255,.03);
  border-radius:9px
}

.controlList b{
  font-size:11px
}

.controlList span{
  font-size:10px;
  opacity:.58
}

.tip{
  padding:10px;
  margin-top:7px;
  border-radius:9px;
  background:rgba(255,255,255,.025);
  font-size:11px;
  line-height:1.45
}

.tip b{
  color:#8deeff
}

.vehicleDrive{
  position:absolute;
  left:50%;
  top:126px;
  transform:translateX(-50%);
  display:none;
  padding:7px 12px;
  border-radius:999px;
  background:rgba(5,10,19,.72);
  border:1px solid rgba(114,239,255,.22);
  font-size:10px;
  letter-spacing:.1em;
  text-transform:uppercase
}

.scanGlow{
  pointer-events:none;
  position:absolute;
  inset:0;
  background:
    radial-gradient(
      circle at 50% 50%,
      transparent 0,
      rgba(91,228,255,.08) 42%,
      transparent 70%
    );
  opacity:0;
  transition:opacity .12s
}

.vehicleDrive b{
  color:#84f2ff
}

@media(max-width:760px){
  .guideGrid{grid-template-columns:1fr}
  .compass{width:270px}
  .controlList{grid-template-columns:1fr}
  .guideHead h2{font-size:24px}
}

@media(max-width:760px){
  .title{font-size:42px}
  .grid{grid-template-columns:1fr 1fr}
  .stats{min-width:190px}
  .mission{width:55vw}
}
@media(max-height:680px){
  .top{top:8px}
  .mission{padding:9px 11px}
  .mission strong{font-size:14px}
  .mission p{font-size:10px}
  .stats{padding:8px 10px}
  .row{margin:3px 0}
  .bar{height:5px}
  #weapon{bottom:8px;max-height:42vh;padding:9px 11px}
  .weaponName{font-size:17px}
  .ammo{font-size:22px}
}


#cutsceneOverlay{
  position:fixed;
  inset:0;
  z-index:80;
  display:none;
  pointer-events:none;
  background:
    radial-gradient(circle at 50% 50%, rgba(91,228,255,.05), transparent 46%),
    rgba(2,5,12,.08);
}
#cutsceneBars{
  position:absolute;
  inset:0;
  background:
    linear-gradient(
      to bottom,
      rgba(0,0,0,.9) 0,
      rgba(0,0,0,.9) 9%,
      transparent 9%,
      transparent 91%,
      rgba(0,0,0,.9) 91%,
      rgba(0,0,0,.9) 100%
    );
}
#cutsceneText{
  position:absolute;
  left:50%;
  bottom:13%;
  transform:translateX(-50%);
  width:min(900px,86vw);
  text-align:center;
  text-shadow:0 3px 20px rgba(0,0,0,.9);
}
#cutsceneKicker{
  font-size:10px;
  letter-spacing:.28em;
  text-transform:uppercase;
  opacity:.58;
  margin-bottom:8px;
}
#cutsceneTitle{
  font-size:clamp(26px,4vw,52px);
  font-weight:950;
  letter-spacing:.08em;
  text-transform:uppercase;
  opacity:0;
  transition:opacity .5s;
}
#cutsceneSubtitle{
  margin-top:10px;
  font-size:clamp(13px,1.6vw,18px);
  line-height:1.55;
  opacity:0;
  transition:opacity .5s;
}

</style>
</head>

<body>


<div id="menu">
  <div class="card">

    <div class="kicker">
      Parallel Worlds / Fracture / 3D Adventure
    </div>

    <div class="title">
      PARALLEL WORLDS<br>
      <span>— FRACTURE</span>
    </div>

    <div class="intro">
      Aurelia is the living timeline. Vanta is the future left behind
      after a reality experiment tore the two together. Explore a huge
      connected zone, use machinery across realities, fight Rift
      creatures, unlock a Rift Runner, restore an underground facility,
      and choose the fate of both worlds.
    </div>

    <div class="grid">
      <div><b>WASD</b>Move</div>
      <div><b>Mouse</b>Look</div>
      <div><b>SHIFT</b>Switch reality</div>
      <div><b>E</b>Interact</div>
      <div><b>F / Click</b>Fire</div>
      <div><b>R</b>Reload</div>
      <div><b>1–7</b>Weapons</div>
      <div><b>SPACE</b>Jump</div>
      <div><b>G</b>Guide</div>
      <div><b>Q</b>Rift scan</div>
    </div>

    <button id="start">START FRACTURE</button>

  </div>
</div>



<div id="hud">

  <div class="top">

    <div class="mission panel">
      <div class="eyebrow">Current objective</div>

      <strong id="mName">
        Meet Mira
      </strong>

      <p id="mDesc"></p>
    </div>

    <div class="stats panel">

      <div class="row">
        <span>REALITY</span>
        <b id="world">AURELIA</b>
      </div>

      <div class="row">
        <span>HP</span>
        <b id="hp">100 / 100</b>
      </div>

      <div class="bar">
        <i id="hpBar"></i>
      </div>

      <div class="row">
        <span>RIFT ENERGY</span>
        <b id="energy">100%</b>
      </div>

      <div class="bar">
        <i id="enBar"></i>
      </div>

      <div class="row">
        <span>RIFT SIGNAL</span>
        <b id="signal">0%</b>
      </div>

    </div>

  </div>


  <div id="sideQuest" class="sideQuest panel"><div class="eyebrow">Side Quest</div><strong id="sqName">No side quest</strong><p id="sqDesc"></p><div class="sqProgress" id="sqProgress"></div></div>

 

  <div class="compass panel" id="compass">

    <div class="compassTarget">

      <span class="eyebrow">
        Navigation
      </span>

      <b id="compassName">
        Mira
      </b>

      <span id="compassDistance">
        0 m
      </span>

    </div>

    <div class="compassDial">

      <span>N</span>
      <span>E</span>
      <span>S</span>
      <span>W</span>

      <div
        class="compassArrow"
        id="compassArrow">
        ▲
      </div>

    </div>

  </div>


 

  <div
    class="vehicleDrive"
    id="vehicleDrive">
    RIFT RUNNER · W/S DRIVE · A/D STEER · SHIFT BOOST
  </div>


  <div id="toast"></div>



  <div id="prompt"></div>



  <div id="cross"></div>



  <div id="weapon" class="panel">

    <div class="weaponInfo">
      Equipped weapon
    </div>

    <div
      class="weaponName"
      id="weaponName">
      Rift Pistol
    </div>

    <div
      class="ammo"
      id="ammo">
      12 / 72
    </div>

    <div class="weaponInfo">
      1–4 switch · R reload · F / Click use
    </div>

    <div
      class="weaponSlot"
      id="weaponSlot">
    </div>

  </div>



  <div
    id="dialogue"
    class="panel">

    <div
      class="speaker"
      id="speaker">
    </div>

    <div
      class="line"
      id="line">
    </div>

    <div class="note">
      E — continue dialogue
    </div>

  </div>

</div>


<div id="flash"></div>

<div id="hitFlash"></div>



<div id="guide" class="guideOverlay">

  <div class="guidePanel">

    <div class="guideHead">

      <div>

        <div class="kicker">
          Rift Runner Field Guide
        </div>

        <h2>
          HOW TO SURVIVE THE FRACTURE
        </h2>

      </div>

      <button id="guideClose">
        CLOSE <span>G</span>
      </button>

    </div>


    <div class="guideGrid">

      <section>

        <h3>
          YOUR CURRENT MISSION
        </h3>

        <div class="guideCurrent">

          <b id="guideMission">
            Meet Mira
          </b>

          <p id="guideMissionDesc"></p>

        </div>


        <h3>
          MISSION ROADMAP
        </h3>

        <div
          id="guideSteps"
          class="guideSteps">
        </div>

      </section>


      <section>

        <h3>
          CONTROLS
        </h3>

        <div class="controlList">

          <div>
            <b>W A S D</b>
            <span>Move / drive</span>
          </div>

          <div>
            <b>MOUSE</b>
            <span>Look around</span>
          </div>

          <div>
            <b>E</b>
            <span>Talk / interact / enter vehicle</span>
          </div>

          <div>
            <b>SHIFT</b>
            <span>Switch reality</span>
          </div>

          <div>
            <b>F / LEFT CLICK</b>
            <span>Fire weapon</span>
          </div>

          <div>
            <b>R</b>
            <span>Reload</span>
          </div>

          <div>
            <b>1–7</b>
            <span>Equip weapon</span>
          </div>

          <div>
            <b>SPACE</b>
            <span>Jump</span>
          </div>

          <div>
            <b>G</b>
            <span>Open / close guide</span>
          </div>

          <div>
            <b>Q</b>
            <span>Rift scan nearby interactables</span>
          </div>

        </div>


        <h3>
          REALITY RULES
        </h3>

        <div class="tip">
          <b>AURELIA</b> —
          living infrastructure, stable machinery,
          powered structures.
        </div>

        <div class="tip">
          <b>VANTA</b> —
          ruined routes, abandoned tech,
          hidden shortcuts and Rift creatures.
        </div>

        <div class="tip">
          <b>WATCH THE COMPASS</b> —
          the cyan arrow points toward your active mission target.
        </div>

      </section>

    </div>

  </div>

</div>



<div id="ending">

  <div class="end">

    <div class="kicker">
      Rift Core decision
    </div>

    <h1 id="endTitle">
      The worlds are in your hands.
    </h1>

    <p id="endText"></p>

    <div class="choices">

      <button data-choice="merge">
        Merge worlds
      </button>

      <button data-choice="aurelia">
        Save Aurelia
      </button>

      <button data-choice="vanta">
        Save Vanta
      </button>

      <button data-choice="destroy">
        Destroy the Rift
      </button>

    </div>

  </div>

</div>



<div id="cutsceneOverlay">
  <div id="cutsceneBars"></div>
  <div id="cutsceneText">
    <div id="cutsceneKicker">Parallel Worlds — Fracture</div>
    <div id="cutsceneTitle"></div>
    <div id="cutsceneSubtitle"></div>
  </div>
</div>

<script type="module">



import * as THREE from
'https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js';


const $ = id => document.getElementById(id);

const clamp = (v,a,b) =>
  Math.max(a,Math.min(b,v));


const scene = new THREE.Scene();

scene.background =
  new THREE.Color(0x7bb8d1);

scene.fog =
  new THREE.Fog(
    0x7bb8d1,
    80,
    280
  );


const camera =
  new THREE.PerspectiveCamera(
    67,
    innerWidth / innerHeight,
    .1,
    500
  );

camera.position.set(
  0,
  4,
  72
);


const renderer =
  new THREE.WebGLRenderer({
    antialias:false,
    powerPreference:'high-performance'
  });


renderer.setPixelRatio(
  Math.min(devicePixelRatio,1.25)
);

renderer.setSize(
  innerWidth,
  innerHeight
);

renderer.outputColorSpace =
  THREE.SRGBColorSpace;

renderer.toneMapping =
  THREE.ACESFilmicToneMapping;

renderer.toneMappingExposure = 1.15;

document.body.appendChild(
  renderer.domElement
);




const hemi =
  new THREE.HemisphereLight(
    0xcff2ff,
    0x22301d,
    1.8
  );

const sun =
  new THREE.DirectionalLight(
    0xffffff,
    2.0
  );

const fill =
  new THREE.PointLight(
    0x58caff,
    50,
    90
  );

scene.add(
  hemi,
  sun,
  fill
);

sun.position.set(
  40,
  80,
  20
);

fill.position.set(
  0,
  20,
  30
);


const clock =
  new THREE.Clock();


const world = {
  value:0
};


let started = false;
let pointer = false;
let mission = 0;
let hp = 100;
let energy = 100;
let signal = 0;
let yaw = 0;
let pitch = -0.08;
let velY = 0;
let onGround = false;
let inVehicle = false;
let dialogueOpen = false;
let dialogueQueue = [];
let lastShot = 0;
let weaponIndex = 0;
let gameTime = 0;
let toastTimer = 0;
let guideOpen = false;
let vehicleVelocity = 0;
let scanTimer = 0;
let meleeTimer = 0;
let finalePulse = 0;
let guardianDefeated = false;
let cutsceneActive = false;
let cutsceneTime = 0;
let cutsceneChoice = null;
let cutsceneStartPos = new THREE.Vector3();


let checkpoint = {
  x:0,
  y:1.1,
  z:68,
  world:0
};


const keys =
  new Set();

const interactables = [];
const enemies = [];
const projectiles = [];
const particles = [];
const worldPlatforms = [];


const solidObstacles = [];




const weapons = [

  {
    name:'Rift Pistol',
    damage:22,
    rate:.34,
    magSize:12,
    mag:12,
    reserve:72,
    range:70,
    color:0x64e8ff,
    spread:0
  },

  {
    name:'Pulse Rifle',
    damage:12,
    rate:.11,
    magSize:30,
    mag:30,
    reserve:150,
    range:85,
    color:0x78ffb8,
    spread:.015
  },

  {
    name:'Arc Shotgun',
    damage:10,
    rate:.75,
    magSize:6,
    mag:6,
    reserve:42,
    range:38,
    color:0xffd166,
    spread:.11,
    pellets:7
  },

  {
    name:'Rail Cannon',
    damage:75,
    rate:1.15,
    magSize:4,
    mag:4,
    reserve:20,
    range:120,
    color:0xff78e8,
    spread:0
  },

  {
    name:'Void SMG',
    damage:8,
    rate:.075,
    magSize:45,
    mag:45,
    reserve:220,
    range:78,
    color:0xc17cff,
    spread:.025
  },

  {
    name:'Gravity Launcher',
    damage:42,
    rate:.9,
    magSize:5,
    mag:5,
    reserve:35,
    range:55,
    color:0xff784f,
    spread:.035
  },

  {
    name:'Photon Blade',
    damage:70,
    rate:.62,
    magSize:1,
    mag:1,
    reserve:0,
    range:6.2,
    color:0x9cfff7,
    spread:0,
    melee:true
  }

];




function mat(
  color,
  rough=.75,
  metal=.15,
  emis=0
){

  return new THREE.MeshStandardMaterial({
    color,
    roughness:rough,
    metalness:metal,
    emissive:emis,
    emissiveIntensity:
      emis ? 2.2 : 0
  });

}


function box(
  x,
  y,
  z,
  m
){

  return new THREE.Mesh(
    new THREE.BoxGeometry(
      x,
      y,
      z
    ),
    m
  );

}


function cyl(
  r,
  h,
  m,
  s=10
){

  return new THREE.Mesh(
    new THREE.CylinderGeometry(
      r,
      r,
      h,
      s
    ),
    m
  );

}


function sphere(
  r,
  m,
  s=12
){

  return new THREE.Mesh(
    new THREE.SphereGeometry(
      r,
      s,
      Math.max(
        6,
        Math.floor(s*.75)
      )
    ),
    m
  );

}


function add(
  o,
  p=scene
){

  p.add(o);
  return o;

}




const player =
  new THREE.Group();

player.position.set(
  0,
  1.1,
  68
);

scene.add(player);


const body =
  add(
    box(
      .9,
      1.45,
      .55,
      mat(
        0x263d5c,
        .52,
        .35
      )
    ),
    player
  );

body.position.y=.35;


const jacket =
  add(
    box(
      1.02,
      .72,
      .62,
      mat(
        0x39a9d9,
        .5,
        .25,
        0x0d3244
      )
    ),
    player
  );

jacket.position.y=.35;


const head =
  add(
    sphere(
      .38,
      mat(
        0xd3dde7,
        .88,
        .05
      ),
      12
    ),
    player
  );

head.position.y=1.35;


const visor =
  add(
    box(
      .48,
      .18,
      .38,
      mat(
        0x72edff,
        .2,
        .8,
        0x1ed5ff
      )
    ),
    player
  );

visor.position.set(
  0,
  1.37,
  .33
);


const ring =
  add(
    new THREE.Mesh(
      new THREE.TorusGeometry(
        .7,
        .055,
        8,
        20
      ),
      mat(
        0x64e8ff,
        .2,
        .65,
        0x64e8ff
      )
    ),
    player
  );

ring.rotation.x =
  Math.PI/2;

ring.position.y=-.04;




const weaponRig =
  new THREE.Group();

camera.add(
  weaponRig
);

weaponRig.position.set(
  .72,
  -.68,
  -1.38
);

let weaponMesh = null;


function buildWeaponVisual(){

  if(weaponMesh)
    weaponRig.remove(
      weaponMesh
    );

  const g =
    new THREE.Group();

  const w =
    weapons[weaponIndex];


  const base =
    add(
      box(
        .32,
        .18,
        .9,
        mat(
          0x202b38,
          .38,
          .6
        )
      ),
      g
    );

  base.rotation.x=-.08;


  const grip =
    add(
      box(
        .14,
        .28,
        .3,
        mat(
          0x15202b,
          .65,
          .45
        )
      ),
      g
    );

  grip.position.set(
    -.02,
    -.2,
    .16
  );

  grip.rotation.x=-.23;


  const barrel =
    add(
      cyl(
        .055,
        .62,
        mat(
          w.color,
          .2,
          .75,
          w.color
        ),
        8
      ),
      g
    );

  barrel.rotation.x =
    Math.PI/2;

  barrel.position.z=-.57;


  const sight =
    add(
      box(
        .08,
        .06,
        .17,
        mat(
          w.color,
          .2,
          .55,
          w.color
        )
      ),
      g
    );

  sight.position.set(
    0,
    .12,
    -.24
  );


  if(weaponIndex===2){

    for(
      let i=-1;
      i<=1;
      i++
    ){

      const p =
        add(
          cyl(
            .045,
            .55,
            mat(
              w.color,
              .18,
              .75,
              w.color
            ),
            8
          ),
          g
        );

      p.rotation.x =
        Math.PI/2;

      p.position.set(
        i*.09,
        0,
        -.52
      );

    }

  }


  if(weaponIndex===3){

    const rail =
      add(
        box(
          .15,
          .12,
          1.2,
          mat(
            w.color,
            .18,
            .75,
            w.color
          )
        ),
        g
      );

    rail.position.y=.1;

  }


  if(weaponIndex===4){

    const mag =
      add(
        box(
          .16,
          .34,
          .33,
          mat(
            0x39265a,
            .48,
            .45
          )
        ),
        g
      );

    mag.position.set(
      0,
      -.2,
      .08
    );

  }


  if(weaponIndex===5){

    const chamber =
      add(
        sphere(
          .18,
          mat(
            w.color,
            .15,
            .6,
            w.color
          ),
          10
        ),
        g
      );

    chamber.position.z=-.48;


    const brace =
      add(
        box(
          .24,
          .1,
          .72,
          mat(
            0x282f38,
            .4,
            .7
          )
        ),
        g
      );

    brace.position.z=-.23;

  }


  if(weaponIndex===6){
    const hilt=add(
      box(.16,.22,.5,mat(0x25313b,.3,.75)),
      g
    );
    hilt.position.set(0,-.2,.2);
    hilt.rotation.x=-.3;

    const guard=add(
      box(.5,.08,.12,mat(0x9cefff,.18,.7,0x65eaff)),
      g
    );
    guard.position.set(0,-.02,-.02);

    const blade=add(
      box(.09,.09,2.35,mat(0xcfffff,.12,.6,0x6affff)),
      g
    );
    blade.position.set(0,.03,-1.15);

    const edge=add(
      box(.035,.14,2.42,mat(0xffffff,.1,.35,0xb8ffff)),
      g
    );
    edge.position.set(.07,.03,-1.15);
  }

  weaponMesh = g;

  weaponRig.add(g);

  updateWeaponUI();

}




const groundMatA =
  mat(
    0x274d35,
    .98,
    .02
  );

const groundMatB =
  mat(
    0x342d31,
    .98,
    .02
  );

const roadMatA =
  mat(
    0x293d4c,
    .9,
    .08
  );

const roadMatB =
  mat(
    0x1b2027,
    .9,
    .1
  );


function terrainRect(
  x,
  z,
  w,
  d,
  m
){

  const o =
    box(
      w,
      .7,
      d,
      m
    );

  o.position.set(
    x,
    -.35,
    z
  );

  scene.add(o);

  return o;

}


terrainRect(
  0,
  40,
  114,
  56,
  groundMatA
);

terrainRect(
  -25,
  0,
  64,
  42,
  groundMatA
);

terrainRect(
  27,
  0,
  54,
  42,
  groundMatA
);

terrainRect(
  -2,
  -46,
  110,
  42,
  roadMatA
);

terrainRect(
  -2,
  -84,
  72,
  28,
  groundMatB
);

terrainRect(
  32,
  -116,
  50,
  38,
  roadMatA
);

terrainRect(
  -28,
  -132,
  42,
  46,
  roadMatB
);

terrainRect(
  0,
  -168,
  96,
  28,
  roadMatB
);

terrainRect(
  0,
  -213,
  84,
  54,
  mat(
    0x161a20,
    .99,
    .08
  )
);

terrainRect(
  0,
  -258,
  74,
  36,
  mat(
    0x0e1117,
    .99,
    .1
  )
);




function platform(
  x,
  y,
  z,
  w,
  d,
  wid,
  color
){

  const g =
    box(
      w,
      .45,
      d,
      mat(
        color,
        .42,
        .5,
        color
      )
    );

  g.position.set(
    x,
    y,
    z
  );

  scene.add(g);

  g.userData = {
    world:wid,
    label:'Reality platform'
  };

  worldPlatforms.push(g);

  return g;

}


platform(
  36,
  .55,
  -66,
  20,
  12,
  0,
  0x58dfff
);

platform(
  -36,
  .55,
  -96,
  26,
  10,
  1,
  0x88949f
);

platform(
  0,
  .4,
  -192,
  20,
  10,
  1,
  0x6e7d8d
);

platform(
  0,
  2.2,
  -205,
  18,
  5,
  0,
  0x6ef0ff
);

platform(
  0,
  2.2,
  -205,
  18,
  5,
  1,
  0x98676b
);




const vantaCauseway =
  platform(
    -20,
    .52,
    -67,
    32,
    4,
    1,
    0x6e7f88
  );

const vantaCauseway2 =
  platform(
    -31,
    .52,
    -80,
    14,
    24,
    1,
    0x59666f
  );

const vantaStep =
  platform(
    -36,
    .62,
    -90,
    12,
    8,
    1,
    0x77828a
  );


vantaCauseway.userData.label =
  'Vanta Causeway';

vantaCauseway2.userData.label =
  'Vanta Causeway';

vantaStep.userData.label =
  'Vanta Causeway';




const trees = [];


function tree(
  x,
  z,
  s=1
){

  const g =
    new THREE.Group();

  g.position.set(
    x,
    0,
    z
  );

  g.scale.setScalar(s);

  solidObstacles.push({
    type:'tree',
    x,
    z,
    radius:0.9*s
  });


  const t =
    add(
      cyl(
        .32,
        3.1,
        mat(
          0x553720
        ),
        8
      ),
      g
    );

  t.position.y=1.55;


  const crown =
    add(
      new THREE.Mesh(
        new THREE.ConeGeometry(
          1.75,
          4.4,
          7
        ),
        mat(
          0x40ad67,
          .95,
          .02
        )
      ),
      g
    );

  crown.position.y=4;

  g.userData={
    crown
  };

  scene.add(g);

  trees.push(g);

}


for(
  let z=19;
  z>-25;
  z-=8
){

  for(
    let x=-50;
    x<=50;
    x+=12
  ){

    if(
      (x/12+z)%3!==0
    ){

      tree(
        x+(z%5),
        z,
        .8+
        (
          Math.abs(x+z)%5
        )/13
      );

    }

  }

}




function building(
  x,
  z,
  w,
  h,
  d,
  color
){

  const g =
    new THREE.Group();

  g.position.set(
    x,
    h/2,
    z
  );


  add(
    box(
      w,
      h,
      d,
      mat(
        color,
        .68,
        .45
      )
    ),
    g
  );


  for(
    let r=0;
    r<Math.floor(h/4);
    r++
  ){

    for(
      let c=-1;
      c<=1;
      c++
    ){

      const win =
        add(
          box(
            w/6,
            .85,
            .08,
            mat(
              0x83dbff,
              .2,
              .6,
              0x2da9ff
            )
          ),
          g
        );

      win.position.set(
        c*w*.28,
        (
          r-Math.floor(h/8)
        )*4,
        d/2+.04
      );

    }

  }


  scene.add(g);

 
  solidObstacles.push({
    type:'box',
    x,
    z,
    halfW:w/2,
    halfD:d/2
  });

  return g;

}




const controlCenter =
  building(
    -31,
    -24.5,
    15,
    9,
    12,
    0x233b52
  );

const controlRoof =
  add(
    box(
      17,
      .45,
      14,
      mat(
        0x152637,
        .35,
        .6
      )
    ),
    controlCenter
  );

controlRoof.position.y=4.7;


const controlCore =
  add(
    cyl(
      1.35,
      5.2,
      mat(
        0x2f526e,
        .3,
        .75,
        0x42dbff
      ),
      12
    ),
    controlCenter
  );

controlCore.rotation.z =
  Math.PI/2;

controlCore.position.set(
  0,
  1.4,
  6.1
);


const controlSign =
  add(
    box(
      9,
      1.1,
      .22,
      mat(
        0x72edff,
        .22,
        .65,
        0x2ed9ff
      )
    ),
    controlCenter
  );

controlSign.position.set(
  0,
  3.1,
  6.12
);




const centralBridgeA =
  platform(
    0,
    .45,
    -34.5,
    110,
    7,
    0,
    0x69eaff
  );

const centralBridgeB =
  platform(
    0,
    .45,
    -34.5,
    110,
    7,
    1,
    0x80545d
  );

centralBridgeA.userData.label =
  'Aurelia Skybridge';

centralBridgeB.userData.label =
  'Vanta Scrap Bridge';


centralBridgeA.material.emissiveIntensity=1.7;

centralBridgeB.material.emissiveIntensity=1.2;

centralBridgeA.userData.central=true;
centralBridgeB.userData.central=true;




function bridgeRail(
  x,
  z,
  worldId
){

  const r =
    box(
      2,
      .8,
      6,
      mat(
        worldId===0
          ? 0x8befff
          : 0x60444a,
        .3,
        .6,
        worldId===0
          ? 0x3ee6ff
          : 0x512e32
      )
    );

  r.position.set(
    x,
    1,
    z
  );

  r.userData.world =
    worldId;

  scene.add(r);

  return r;

}


for(
  const x of
  [-50,-35,-20,-5,10,25,40,55]
){

  bridgeRail(
    x,
    -38.1,
    0
  );

  bridgeRail(
    x,
    -30.9,
    0
  );

  bridgeRail(
    x,
    -38.1,
    1
  );

  bridgeRail(
    x,
    -30.9,
    1
  );

}




for(
  const b of [
    [-40,-118,18,24,16,0x26364b],
    [-16,-143,20,31,17,0x29374b],
    [18,-121,21,28,18,0x2b3951],
    [42,-145,17,22,15,0x233246],
    [-39,-160,20,20,18,0x24313e]
  ]
){

  building(...b);

}




for(
  const x of
  [-42,-30,-18,18,30,42]
){

  const p =
    box(
      1.2,
      7,
      1.2,
      mat(
        0x2a3038,
        .75,
        .5
      )
    );

  p.position.set(
    x,
    3.5,
    -213
  );

  scene.add(p);


  const l =
    add(
      new THREE.PointLight(
        0x58dfff,
        7,
        18
      ),
      p
    );

  l.position.y=2.8;

}


for(
  const z of
  [-199,-218,-237,-256]
){

  const wall =
    box(
      2,
      5,
      10,
      mat(
        0x20262e,
        .9,
        .35
      )
    );

  wall.position.set(
    -40,
    2.5,
    z
  );

  scene.add(wall);


  const wall2 =
    wall.clone();

  wall2.position.x=40;

  scene.add(wall2);

}


for(
  const z of
  [-208,-228,-248]
){

  const strip =
    box(
      62,
      .12,
      .35,
      mat(
        0x5eefff,
        .25,
        .7,
        0x2ed6ff
      )
    );

  strip.position.set(
    0,
    4.7,
    z
  );

  scene.add(strip);

}




function rock(
  x,
  z,
  s=1
){

  const r =
    add(
      new THREE.Mesh(
        new THREE.DodecahedronGeometry(
          3.2*s,
          0
        ),
        mat(
          0x675347,
          .95,
          .02
        )
      )
    );

  r.scale.y=.8;

  r.position.set(
    x,
    2*s,
    z
  );

}


for(
  let z=-52;
  z>-102;
  z-=9
){

  rock(
    -49,
    z,
    1+
    (
      Math.abs(z)%7
    )/15
  );

  rock(
    48,
    z+4,
    1.1
  );

}




const riftPylons=[];

for(
  const p of [
    [-52,-28],
    [52,-48],
    [-49,-112],
    [49,-132],
    [-46,-188],
    [46,-221]
  ]
){

  const g =
    new THREE.Group();

  g.position.set(
    p[0],
    0,
    p[1]
  );


  const stem =
    add(
      cyl(
        .22,
        3.4,
        mat(
          0x53616d,
          .3,
          .65,
          0x55ddff
        ),
        8
      ),
      g
    );

  stem.position.y=1.7;


  const orb =
    add(
      sphere(
        .42,
        mat(
          0x62ecff,
          .12,
          .55,
          0x3cdcff
        ),
        12
      ),
      g
    );

  orb.position.y=3.65;


  g.userData={
    orb
  };


  scene.add(g);

  riftPylons.push(g);

}




function marker(
  color=0x65eaff
){

  const g =
    new THREE.Group();


  const r =
    add(
      new THREE.Mesh(
        new THREE.TorusGeometry(
          .8,
          .06,
          8,
          20
        ),
        mat(
          color,
          .22,
          .6,
          color
        )
      ),
      g
    );

  r.rotation.x =
    Math.PI/2;

  r.position.y=.08;


  const c =
    add(
      sphere(
        .18,
        mat(
          color,
          .2,
          .6,
          color
        ),
        10
      ),
      g
    );

  c.position.y=.75;


  g.userData.marker=r;

  return g;

}


function makeInteract(
  label,
  x,
  z,
  kind,
  fn,
  range=4
){

  const g =
    new THREE.Group();

  g.position.set(
    x,
    0,
    z
  );


  g.userData = {
    label,
    kind,
    fn,
    range
  };


  g.add(
    marker(
      0x64e8ff
    )
  );

  g.userData.marker =
    g.children[0].userData.marker;


  scene.add(g);

  interactables.push(g);

  return g;

}


function machine(
  label,
  x,
  z,
  color=0x67eaff
){

  const g =
    makeInteract(
      label,
      x,
      z,
      'machine',
      null,
      5
    );


  const b =
    add(
      box(
        2.6,
        1.75,
        2.1,
        mat(
          0x242e38,
          .52,
          .55
        )
      ),
      g
    );

  b.position.y=.88;


  const screen =
    add(
      box(
        .82,
        .55,
        .08,
        mat(
          color,
          .2,
          .7,
          color
        )
      ),
      g
    );

  screen.position.set(
    0,
    1.35,
    .6
  );


  const lever =
    add(
      cyl(
        .09,
        .9,
        mat(
          0x909eaa,
          .3,
          .8
        ),
        8
      ),
      g
    );

  lever.rotation.z=.5;

  lever.position.set(
    1.18,
    1.15,
    0
  );


  g.userData.light =
    screen;

  return g;

}


function npcLabel(
  name,
  color
){

  const c =
    document.createElement(
      'canvas'
    );

  c.width=360;
  c.height=70;


  const x =
    c.getContext(
      '2d'
    );


  x.font =
    '900 30px Arial';

  x.textAlign='center';
  x.textBaseline='middle';


  x.fillStyle =
    'rgba(3,8,15,.88)';

  if(x.roundRect){

    x.roundRect(
      8,
      7,
      344,
      56,
      14
    );

    x.fill();

  }
  else{

    x.fillRect(
      8,
      7,
      344,
      56
    );

  }


  x.strokeStyle =
    '#'+
    color.toString(16)
      .padStart(6,'0');

  x.lineWidth=4;


  if(x.roundRect){

    x.beginPath();

    x.roundRect(
      8,
      7,
      344,
      56,
      14
    );

    x.stroke();

  }
  else{

    x.strokeRect(
      8,
      7,
      344,
      56
    );

  }


  x.fillStyle='#fff';

  x.fillText(
    name,
    180,
    37
  );


  const tex =
    new THREE.CanvasTexture(c);

  tex.minFilter =
    THREE.LinearFilter;


  const sp =
    new THREE.Sprite(
      new THREE.SpriteMaterial({
        map:tex,
        transparent:true,
        depthTest:false
      })
    );


  sp.scale.set(
    3.4,
    .67,
    1
  );

  sp.position.y=3.15;

  return sp;

}



const npcDefs = [

  {
    name:'Mira',
    x:3,
    z:58,
    c:0xffa15c,
    need:0,
    lines:[
      'You are finally here. I am Mira, field engineer for the Transit Grid.',
      'A Rift pulse split our infrastructure between two realities.',
      'Scan the unstable anomaly south of the plaza. Then we can reopen the bridge.'
    ]
  },

  {
    name:'Ilan',
    x:-8,
    z:48,
    c:0x9e91ff,
    need:1,
    lines:[
      'Reality shifting is not teleportation.',
      'It is choosing which version of the place is real for you.',
      'Use it to make broken paths become useful ones.'
    ]
  },

  {
    name:'Kea',
    x:35,
    z:-7,
    c:0x5be3c3,
    need:2,
    lines:[
      'The eastern skybridge survives only in Aurelia.',
      'The scrap bridge exists in Vanta. The canyon is deliberately split.'
    ]
  },

  {
    name:'Rook',
    x:-32,
    z:-48,
    c:0xff766b,
    need:2,
    lines:[
      'Something is hunting in the canyon.',
      'Keep your weapons ready. Rift creatures are attracted to your signal.'
    ]
  },

  {
    name:'Sena',
    x:28,
    z:-115,
    c:0xf5d35a,
    need:4,
    lines:[
      'The Rift Runner is in the garage.',
      'Get it running and take the industrial road south.'
    ]
  },

  {
    name:'Orin',
    x:-44,
    z:-143,
    c:0x7ac7ff,
    need:5,
    lines:[
      'The underground facility still has three independent power relays.',
      'Bring all three online. The Core door will only open after that.'
    ]
  },

  {
    name:'Vale',
    x:22,
    z:-173,
    c:0xe3a2ff,
    need:6,
    lines:[
      'The Core lies below.',
      'The Guardian was built to mirror reality itself.',
      'Watch its shield. You will have to switch worlds to damage it.'
    ]
  },

  {
    name:'Echo',
    x:17,
    z:-224,
    c:0x74fff1,
    need:7,
    lines:[
      'The Architect made me to watch the Core.',
      'I cannot change its decision—but you can.'
    ]
  }

];


const npcs=[];


function makeNPC(d){

  const g =
    new THREE.Group();

  g.position.set(
    d.x,
    0,
    d.z
  );


  const col =
    mat(
      d.c,
      .62,
      .2
    );


  const b =
    add(
      cyl(
        .45,
        1.4,
        col,
        10
      ),
      g
    );

  b.position.y=.7;


  const h =
    add(
      sphere(
        .34,
        mat(
          0xdce4ec,
          .85,
          .05
        )
      ),
      g
    );

  h.position.y=1.62;


  const ring =
    add(
      new THREE.Mesh(
        new THREE.TorusGeometry(
          .33,
          .045,
          8,
          16
        ),
        mat(
          d.c,
          .2,
          .6,
          d.c
        )
      ),
      g
    );

  ring.rotation.x =
    Math.PI/2;

  ring.position.y=1.45;


  g.add(
    npcLabel(
      d.name,
      d.c
    )
  );


  g.userData={
    kind:'npc',
    npc:d,
    range:4
  };


  scene.add(g);

  interactables.push(g);
  npcs.push(g);

  return g;

}


npcDefs.forEach(
  makeNPC
);


npcs[2].userData.vantaOnly=true;
npcs[6].userData.vantaOnly=true;
npcs[7].userData.aureliaOnly=true;




const ambient=[];


function ambientNPC(
  x,
  z,
  c=0x6aa7c7,
  scale=1
){

  const g =
    new THREE.Group();

  g.position.set(
    x,
    0,
    z
  );

  g.scale.setScalar(
    scale
  );


  const b =
    add(
      cyl(
        .38,
        1.2,
        mat(
          c,
          .7,
          .15
        )
      ),
      g
    );

  b.position.y=.6;


  const h =
    add(
      sphere(
        .3,
        mat(
          0xd7dce2,
          .9,
          .02
        )
      ),
      g
    );

  h.position.y=1.42;


  const bag =
    add(
      box(
        .28,
        .42,
        .18,
        mat(
          0x303c4c,
          .8,
          .2
        )
      ),
      g
    );

  bag.position.set(
    .4,
    .65,
    -.05
  );


  scene.add(g);

  ambient.push(g);

  return g;

}


for(
  const p of [
    [-15,51,0x6bb5dd,.9],
    [14,48,0x8ad0b6,.95],
    [-22,38,0xd5a46d,.9],
    [26,25,0x9e8fe0,.92],
    [-45,13,0x86b7d8,.85],
    [42,5,0xd78d79,.95],
    [-48,-8,0x76c8a6,.9],
    [47,-18,0xd6b768,.9],
    [-45,-126,0x7b91ad,.95],
    [-4,-137,0xa7a4b5,.9],
    [38,-132,0xa7bd83,.92],
    [47,-158,0xc58ca4,.88],
    [-42,-177,0x7ca6bd,.9],
    [14,-177,0xb28fd0,.92]
  ]
){

  ambientNPC(...p);

}




const anomaly =
  makeInteract(
    'Scan unstable Rift anomaly',
    18,
    -24,
    'anomaly',
    null,
    6
  );


add(
  new THREE.Mesh(
    new THREE.TorusGeometry(
      2.1,
      .13,
      8,
      24
    ),
    mat(
      0x74ecff,
      .18,
      .7,
      0x4de7ff
    )
  ),
  anomaly
).rotation.x =
  Math.PI/2;


anomaly.children[
  anomaly.children.length-1
].position.y=2.7;


const cell =
  makeInteract(
    'Pick up power cell',
    -39,
    -95,
    'cell',
    null,
    4
  );


const cellBox =
  add(
    box(
      .9,
      1,
      .7,
      mat(
        0xffcf57,
        .25,
        .7,
        0xffa000
      )
    ),
    cell
  );

cellBox.position.y=.65;


const bridgeConsole =
  machine(
    'Activate skybridge control',
    -31,
    -21.0
  );

bridgeConsole.userData.controlCenter=true;


bridgeConsole.scale.setScalar(
  1.25
);


const garage =
  machine(
    'Insert power cell',
    42,
    -113
  );


const facilityEntry =
  machine(
    'Enter Industrial Basin Facility',
    -17,
    -160
  );

facilityEntry.userData.range=7.5;




const ccBeacon =
  add(
    new THREE.Group()
  );

ccBeacon.position.set(
  -31,
  0,
  -22.2
);

scene.add(
  ccBeacon
);


const ccRing =
  add(
    new THREE.Mesh(
      new THREE.TorusGeometry(
        1.45,
        .09,
        8,
        24
      ),
      mat(
        0x66eaff,
        .18,
        .65,
        0x66eaff
      )
    ),
    ccBeacon
  );

ccRing.rotation.x =
  Math.PI/2;

ccRing.position.y=.08;


const ccPillar =
  add(
    box(
      .18,
      4.2,
      .18,
      mat(
        0x7ceeff,
        .18,
        .65,
        0x7ceeff
      )
    ),
    ccBeacon
  );

ccPillar.position.y=2.1;


const ccArrow =
  add(
    new THREE.Mesh(
      new THREE.ConeGeometry(
        .42,
        .9,
        8
      ),
      mat(
        0x7ceeff,
        .18,
        .65,
        0x7ceeff
      )
    ),
    ccBeacon
  );

ccArrow.position.y=4.7;

ccArrow.rotation.z =
  Math.PI;




const ccEntrance =
  add(
    box(
      5.5,
      3.8,
      .5,
      mat(
        0x182632,
        .32,
        .55,
        0x1d98b8
      )
    ),
    controlCenter
  );

ccEntrance.position.set(
  0,
  1.9,
  6.45
);


const ccDoorGlow =
  add(
    box(
      3.1,
      2.7,
      .08,
      mat(
        0x67edff,
        .15,
        .65,
        0x25d9ff
      )
    ),
    controlCenter
  );

ccDoorGlow.position.set(
  0,
  1.65,
  6.73
);




const relayDefs = [
  [-30,-195],
  [20,-202],
  [34,-182]
];


const relays =
  relayDefs.map(
    (p,i)=>{

      const g =
        machine(
          'Activate power relay '+
          (i+1),
          p[0],
          p[1],
          0x7cffbe
        );

      g.userData.relay=i+1;

      return g;

    }
  );


const coreDoor =
  machine(
    'Enter Rift Core',
    0,
    -244,
    0xff78e8
  );



const stabilizerDefs = [
  [-8,-257],
  [8,-257],
  [0,-269]
];

const stabilizers = stabilizerDefs.map((p,i)=>{
  const g = machine(
    'Activate Rift stabilizer '+(i+1),
    p[0],
    p[1],
    0x7de8ff
  );
  g.userData.stabilizer=i;
  g.userData.available=()=>mission===9 && !flags.stabilizers[i];
  g.userData.fn=()=>{
    if(mission!==9 || flags.stabilizers[i]) return;
    flags.stabilizers[i]=true;
    g.userData.light.material.emissive.setHex(0x55ff9c);
    signal=clamp(signal+12,0,100);
    finalePulse=Math.max(finalePulse,1.4);
    burst(g.position.clone().add(new THREE.Vector3(0,1,0)),0x73efff,18);

    const count=flags.stabilizers.filter(Boolean).length;
    if(count===3){
      setMission(10);
      showToast('RIFT CORE STABLE — the final decision is now yours.');
      setTimeout(()=>showToast('Mira: The Core is listening. Make your choice carefully.'),1800);
    }else{
      showToast('Core stabilizer '+(i+1)+' online — '+(3-count)+' remaining.');
    }
  };
  return g;
});




const objectiveBeacon =
  new THREE.Group();


const beaconRing =
  add(
    new THREE.Mesh(
      new THREE.TorusGeometry(
        1.15,
        .08,
        8,
        24
      ),
      mat(
        0x74efff,
        .18,
        .7,
        0x74efff
      )
    ),
    objectiveBeacon
  );

beaconRing.rotation.x =
  Math.PI/2;


const beaconBeam =
  add(
    new THREE.Mesh(
      new THREE.CylinderGeometry(
        .045,
        .045,
        7,
        8
      ),
      mat(
        0x74efff,
        .18,
        .55,
        0x74efff
      )
    ),
    objectiveBeacon
  );

beaconBeam.position.y=3.5;


const beaconTop =
  add(
    sphere(
      .25,
      mat(
        0xffffff,
        .1,
        .45,
        0x74efff
      )
    ),
    objectiveBeacon
  );

beaconTop.position.y=7;

objectiveBeacon.visible=false;

scene.add(
  objectiveBeacon
);

objectiveBeacon.userData={
  kind:'objectiveBeacon',
  label:'Objective Beacon',
  range:2.8,
  available:()=>started && mission>=1 && mission<=10
};
interactables.push(objectiveBeacon);



const vehicle =
  new THREE.Group();

vehicle.position.set(
  42,
  1,
  -116
);

vehicle.userData={
  kind:'vehicle',
  label:'Rift Runner',
  range:6
};

scene.add(vehicle);
interactables.push(vehicle);


const vb =
  add(
    box(
      2.7,
      .6,
      4,
      mat(
        0x1e2b3b,
        .35,
        .72,
        0x183e67
      )
    ),
    vehicle
  );

vb.position.y=.6;


const canopy =
  add(
    box(
      1.8,
      .55,
      1.6,
      mat(
        0x74efff,
        .12,
        .65,
        0x1f7ea0
      )
    ),
    vehicle
  );

canopy.position.set(
  0,
  1.1,
  .2
);


const nose =
  add(
    box(
      1.2,
      .25,
      1.4,
      mat(
        0x3a506b,
        .35,
        .55
      )
    ),
    vehicle
  );

nose.position.set(
  0,
  .35,
  -1.4
);


for(
  const x of [-1.15,1.15]
){

  for(
    const z of [-1.35,1.35]
  ){

    const thr =
      add(
        cyl(
          .23,
          .16,
          mat(
            0x70efff,
            .2,
            .7,
            0x44dfff
          ),
          10
        ),
        vehicle
      );

    thr.rotation.z =
      Math.PI/2;

    thr.position.set(
      x,
      .1,
      z
    );

  }

}



const seat =
  add(
    box(
      .72,
      .28,
      .95,
      mat(
        0x0e161f,
        .45,
        .7
      )
    ),
    vehicle
  );

seat.position.set(
  0,
  .85,
  .45
);


const handle =
  add(
    box(
      1.05,
      .12,
      .16,
      mat(
        0x72edff,
        .18,
        .7,
        0x36dfff
      )
    ),
    vehicle
  );

handle.position.set(
  0,
  1.05,
  -.55
);


const thrusters=[];


for(
  const x of [-.85,.85]
){

  const glow =
    add(
      sphere(
        .22,
        mat(
          0x55eaff,
          .16,
          .55,
          0x39dfff
        ),
        10
      ),
      vehicle
    );

  glow.position.set(
    x,
    .28,
    1.85
  );

  thrusters.push(glow);

}



const scanMat =
  mat(
    0x74efff,
    .12,
    .6,
    0x74efff
  );

scanMat.transparent=true;
scanMat.opacity=1;


const scanRing =
  add(
    new THREE.Mesh(
      new THREE.TorusGeometry(
        .8,
        .055,
        8,
        32
      ),
      scanMat
    ),
    player
  );

scanRing.rotation.x =
  Math.PI/2;

scanRing.position.y=.04;

scanRing.scale.setScalar(.1);

scanRing.visible=false;




function locker(
  x,
  z,
  i
){

  const g =
    makeInteract(
      'Equip '+
      weapons[i].name,
      x,
      z,
      'locker',
      ()=>equip(i),
      3
    );


  const b =
    add(
      box(
        1.4,
        2,
        .8,
        mat(
          0x1a2631,
          .55,
          .55
        )
      ),
      g
    );

  b.position.y=1;


  const led =
    add(
      sphere(
        .18,
        mat(
          weapons[i].color,
          .2,
          .7,
          weapons[i].color
        )
      ),
      g
    );

  led.position.y=1.65;

  return g;

}


locker(
  -28,
  42,
  0
);

locker(
  -28,
  48,
  1
);

locker(
  38,
  -39,
  2
);

locker(
  -37,
  -127,
  3
);

locker(
  44,
  -142,
  4
);

locker(
  -30,
  -170,
  5
);

locker(
  8,
  -188,
  6
);



function supplyCrate(
  x,
  z,
  label='Supply Cache'
){

  const g =
    makeInteract(
      label,
      x,
      z,
      'supply',
      null,
      3.2
    );


  const body =
    add(
      box(
        1.4,
        1.1,
        1.15,
        mat(
          0x263540,
          .55,
          .6
        )
      ),
      g
    );

  body.position.y=.56;


  const stripe =
    add(
      box(
        1.45,
        .1,
        .22,
        mat(
          0xffd85a,
          .25,
          .55,
          0xffae00
        )
      ),
      g
    );

  stripe.position.y=.92;


  g.userData.available =
    ()=>true;


  g.userData.fn =
    ()=>{

      hp =
        clamp(
          hp+25,
          0,
          100
        );


      for(
        const w of weapons
      ){

        w.reserve +=
          Math.floor(
            w.magSize*.6
          );

      }


      g.visible=false;

      showToast(
        'Supply cache opened — health and ammunition restored.'
      );

    };


  return g;

}


supplyCrate(
  -18,
  42
);

supplyCrate(
  43,
  -82
);

supplyCrate(
  -5,
  -149
);

supplyCrate(
  22,
  -232
);




function makeHealthLabel(
  name,
  col
){

  const c =
    document.createElement(
      'canvas'
    );

  c.width=300;
  c.height=85;


  const x =
    c.getContext(
      '2d'
    );


  x.font =
    '900 22px Arial';

  x.textAlign='center';


  x.fillStyle='#fff';

  x.fillText(
    name,
    150,
    23
  );


  x.fillStyle =
    'rgba(255,255,255,.16)';

  x.fillRect(
    30,
    40,
    240,
    12
  );


  x.fillStyle =
    '#'+
    col.toString(16)
      .padStart(6,'0');

  x.fillRect(
    30,
    40,
    240,
    12
  );


  const tex =
    new THREE.CanvasTexture(c);

  tex.minFilter =
    THREE.LinearFilter;


  const sp =
    new THREE.Sprite(
      new THREE.SpriteMaterial({
        map:tex,
        transparent:true,
        depthTest:false
      })
    );


  sp.scale.set(
    3.2,
    .9,
    1
  );

  sp.position.y=3;

  return sp;

}


function enemy(
  type,
  x,
  z
){

  const g =
    new THREE.Group();

  g.position.set(
    x,
    0,
    z
  );


  const cfg = {
    stalker:{
      hp:65,
      speed:2.8,
      col:0x58ff9a,
      name:'RIFT STALKER'
    },

    brute:{
      hp:140,
      speed:1.65,
      col:0xff5d4f,
      name:'RIFT BRUTE'
    },

    warden:{
      hp:90,
      speed:2.1,
      col:0xa67cff,
      name:'RIFT WARDEN'
    },

    hunter:{
      hp:95,
      speed:3.9,
      col:0xffc650,
      name:'REALITY HUNTER'
    }

  }[type];


  const body =
    add(
      cyl(
        type==='brute'
          ? .78
          : .5,
        type==='brute'
          ? 1.95
          : 1.45,
        mat(
          cfg.col,
          .4,
          .3,
          cfg.col
        )
      ),
      g
    );

  body.position.y =
    type==='brute'
      ? 1
      : .72;

  body.scale.x=1.2;


  const core =
    add(
      sphere(
        type==='brute'
          ? .3
          : .22,
        mat(
          0xffffff,
          .12,
          .4,
          0xffffff
        ),
        10
      ),
      g
    );

  core.position.set(
    0,
    1.15,
    .37
  );


  const horn1 =
    add(
      cyl(
        .1,
        .7,
        mat(
          cfg.col,
          .35,
          .4,
          cfg.col
        ),
        7
      ),
      g
    );

  horn1.rotation.z=.5;

  horn1.position.set(
    -.32,
    1.55,
    0
  );


  const horn2 =
    horn1.clone();

  horn2.position.x=.32;

  g.add(horn2);


  const eyeL =
    add(
      sphere(
        type==='brute'
          ? .14
          : .1,
        mat(
          0xfff8ff,
          .05,
          .25,
          0xff385d
        ),
        8
      ),
      g
    );

  eyeL.position.set(
    -.2,
    1.28,
    .42
  );


  const eyeR =
    eyeL.clone();

  eyeR.position.x=.2;

  g.add(eyeR);


  if(
    type==='warden' ||
    type==='hunter'
  ){

    const halo =
      add(
        new THREE.Mesh(
          new THREE.TorusGeometry(
            .72,
            .055,
            8,
            24
          ),
          mat(
            cfg.col,
            .2,
            .6,
            cfg.col
          )
        ),
        g
      );

    halo.rotation.x =
      Math.PI/2;

    halo.position.y=.65;

  }


  if(
    type==='brute'
  ){

    const shoulderL =
      add(
        box(
          .35,
          .32,
          .55,
          mat(
            cfg.col,
            .38,
            .45,
            cfg.col
          )
        ),
        g
      );

    shoulderL.position.set(
      -.65,
      .95,
      0
    );


    const shoulderR =
      shoulderL.clone();

    shoulderR.position.x=.65;

    g.add(shoulderR);

  }


  g.add(
    makeHealthLabel(
      cfg.name,
      cfg.col
    )
  );


  g.userData={

    kind:'enemy',

    type,

    hp:cfg.hp,

    maxHp:cfg.hp,

    speed:cfg.speed,

    attack:0,

    col:cfg.col,

    dead:false,

    world:
      type==='hunter'
        ? 1
        : null,

    spawnX:x,
    spawnZ:z

  };


  scene.add(g);

  enemies.push(g);

  return g;

}




for(
  const p of [
    [-19,-31,'stalker'],
    [-2,-35,'stalker'],
    [18,-35,'warden'],
    [-37,-64,'brute'],
    [34,-72,'stalker'],
    [-43,-91,'hunter'],
    [2,-111,'stalker'],
    [19,-131,'stalker'],
    [34,-151,'brute'],
    [-21,-157,'warden'],
    [11,-173,'hunter']
  ]
){

  enemy(
    p[2],
    p[0],
    p[1]
  );

}


let guardian = null;


function spawnGuardian(){

  if(
    guardian &&
    !guardian.userData.dead
  ){

    return;

  }


  guardian =
    enemy(
      'brute',
      0,
      -266
    );


  guardian.userData.guardian=true;

  guardian.userData.hp=520;
  guardian.userData.maxHp=520;
  guardian.userData.speed=1.6;

  guardian.scale.setScalar(
    1.8
  );


  guardian.children.forEach(
    c=>{
      if(c.material)
        c.material.emissive
          ?.setHex(0xff4be8)
    }
  );


  showToast(
    'THE GUARDIAN HAS AWAKENED — its shield is active in VANTA.'
  );

}




const memoryShardDefs=[[-12,-68,0],[24,-96,1],[8,-154,1]];
const memoryShards=memoryShardDefs.map((p,i)=>{
  const g=makeInteract('Recover memory shard '+(i+1),p[0],p[1],'sidequest',null,4.5);
  g.userData.aureliaOnly=p[2]===0;g.userData.vantaOnly=p[2]===1;
  const glow=sphere(.32,mat(0x9cfbff,.15,.7,0x65eaff),10);glow.position.y=1.05;g.add(glow);
  g.userData.available=()=>sideQuest.id==='memories'&&!sideQuest.completed&&g.visible;
  g.userData.fn=()=>{if(sideQuest.id!=='memories'||sideQuest.completed)return;g.visible=false;sideQuestProgress();};
  g.visible=false;return g;
});

const supplyCrateDefs=[[16,-121,0],[-33,-151,1],[31,-188,1]];
const supplyCrates=supplyCrateDefs.map((p,i)=>{
  const g=makeInteract('Recover supply crate '+(i+1),p[0],p[1],'sidequest',null,4.8);
  g.userData.aureliaOnly=p[2]===0;g.userData.vantaOnly=p[2]===1;
  const crate=box(1.25,.95,1.05,mat(0x9b704e,.8,.08));crate.position.y=.48;
  const strap=box(1.31,.12,.12,mat(0x6fdfff,.3,.55,0x35dfff));strap.position.set(0,.72,.53);g.add(crate,strap);
  g.userData.available=()=>sideQuest.id==='freight'&&!sideQuest.completed&&g.visible;
  g.userData.fn=()=>{if(sideQuest.id!=='freight'||sideQuest.completed)return;g.visible=false;sideQuestProgress();};
  g.visible=false;return g;
});



const missions = [

  [
    'Meet Mira',
    'Talk to Mira in the Aurelia transit plaza.'
  ],

  [
    'Scan the anomaly',
    'Travel south and scan the unstable Rift anomaly.'
  ],

  [
    'Activate the skybridge',
    'Use the Aurelia control station.'
  ],

  [
    'Retrieve the power cell',
    'Switch to Vanta and recover the abandoned power cell.'
  ],

  [
    'Unlock the Rift Runner',
    'Return to the garage and insert the power cell.'
  ],

  [
    'Reach the Industrial Basin',
    'Enter the Rift Runner or walk to the cyan facility beacon at the southern industrial road, then press E.'
  ],

  [
    'Restore underground power',
    'At the basin entrance, activate Relay 1, Relay 2 and Relay 3. The compass moves to the next relay after each one.'
  ],

  [
    'Reach the Rift Core',
    'Open the Core door after restoring all relays.'
  ],

  [
    'Defeat the final Guardian',
    'Switch realities to expose the Guardian, then defeat it.'
  ],

  [
    'Stabilize the Rift Core',
    'Reach the Core and activate the three stabilization nodes before the fracture collapses.'
  ],

  [
    'Make the final decision',
    'The Core is stable. Choose the fate of Aurelia, Vanta, and the Rift.'
  ]

];


const flags = {

  anomaly:false,
  bridge:false,
  cell:false,
  garage:false,

  relays:[
    false,
    false,
    false
  ],

  core:false,

  stabilizers:[
    false,
    false,
    false
  ]

};




const sideQuest={id:null,title:'',desc:'',progress:0,total:0,completed:false};

function updateSideQuestUI(){
  const p=$('sideQuest');
  if(!p)return;
  p.style.display=sideQuest.id?'block':'none';
  if(sideQuest.id){
    $('sqName').textContent=sideQuest.completed?sideQuest.title+' — COMPLETE':sideQuest.title;
    $('sqDesc').textContent=sideQuest.desc;
    $('sqProgress').textContent=sideQuest.completed?'Reward received':'Progress: '+sideQuest.progress+' / '+sideQuest.total;
  }
}

function clearSideQuest(){
  sideQuest.id=null;sideQuest.title='';sideQuest.desc='';sideQuest.progress=0;sideQuest.total=0;sideQuest.completed=false;updateSideQuestUI();
}

function startSideQuest(id){
  if(sideQuest.id===id && !sideQuest.completed)return;
  if(id==='cull'){sideQuest.id='cull';sideQuest.title='Rift Cull';sideQuest.desc='Rook wants three Rift creatures eliminated.';sideQuest.progress=0;sideQuest.total=3;sideQuest.completed=false;}
  if(id==='memories'){sideQuest.id='memories';sideQuest.title='Echoes of the Lost';sideQuest.desc='Recover three memory shards split between Aurelia and Vanta.';sideQuest.progress=0;sideQuest.total=3;sideQuest.completed=false;memoryShards.forEach(g=>g.visible=true);applyWorld();}
  if(id==='freight'){sideQuest.id='freight';sideQuest.title='Lost Freight';sideQuest.desc='Recover three abandoned supply crates from the industrial routes.';sideQuest.progress=0;sideQuest.total=3;sideQuest.completed=false;supplyCrates.forEach(g=>g.visible=true);applyWorld();}
  updateSideQuestUI();
  showToast('SIDE QUEST STARTED: '+sideQuest.title);
}

function sideQuestProgress(){
  if(!sideQuest.id||sideQuest.completed)return;
  sideQuest.progress=clamp(sideQuest.progress+1,0,sideQuest.total);
  if(sideQuest.progress>=sideQuest.total){
    sideQuest.completed=true;
    if(sideQuest.id==='cull'){energy=clamp(energy+35,0,100);weapons[0].reserve+=24;showToast('RIFT CULL COMPLETE — +35 Energy, +24 Pistol ammo.');}
    if(sideQuest.id==='memories'){energy=clamp(energy+25,0,100);weapons[1].reserve+=45;showToast('ECHOES OF THE LOST COMPLETE — +25 Energy, +45 Rifle ammo.');}
    if(sideQuest.id==='freight'){hp=clamp(hp+30,0,100);weapons[4].reserve+=70;showToast('LOST FREIGHT COMPLETE — +30 HP, +70 SMG ammo.');}
  }
  updateSideQuestUI();
}

function setMission(n){

  mission=n;

  $('mName').textContent =
    missions[n][0];

  $('mDesc').textContent =
    missions[n][1];


  updateObjectiveVisibility();


  if(guideOpen)
    updateGuide();


  updateCompass();


  if(
    started &&
    !inVehicle &&
    n>0
  ){

    checkpoint={
      x:player.position.x,
      y:1.1,
      z:player.position.z,
      world:world.value
    };

  }

}


setMission(0);


function showToast(t){

  $('toast').textContent=t;

  $('toast').style.opacity='1';

  toastTimer=2.4;

}


function showDialogue(npc){

  dialogueQueue =
    [...npc.userData.npc.lines];

  dialogueOpen=true;

  $('dialogue').style.display='block';

  $('speaker').textContent =
    npc.userData.npc.name;

  $('line').textContent =
    dialogueQueue.shift();

}


function nextDialogue(){

  if(!dialogueOpen)
    return;


  if(dialogueQueue.length){

    $('line').textContent =
      dialogueQueue.shift();

    return;

  }


  dialogueOpen=false;

  $('dialogue').style.display='none';


  const name =
    $('speaker').textContent;


  if(
    name==='Mira' &&
    mission===0
  ){

    setMission(1);

  }


  if(
    name==='Kea' &&
    mission===2
  ){

    showToast(
      'The canyon routes are now marked on your mission compass.'
    );

  }

}


  if(name==='Rook' && mission>=2 && (!sideQuest.id || sideQuest.completed || sideQuest.id==='cull')) startSideQuest('cull');
  if(name==='Ilan' && mission>=1 && (!sideQuest.id || sideQuest.completed || sideQuest.id==='memories')) startSideQuest('memories');
  if(name==='Sena' && mission>=4 && (!sideQuest.id || sideQuest.completed || sideQuest.id==='freight')) startSideQuest('freight');




anomaly.userData.fn =
  ()=>{

    if(
      mission===1 &&
      !flags.anomaly
    ){

      flags.anomaly=true;
      signal=25;

      setMission(2);

      showToast(
        'Anomaly synchronized. Bridge controls are online.'
      );

    }

  };


bridgeConsole.userData.fn =
  ()=>{

    if(
      mission===2 &&
      !flags.bridge
    ){

      flags.bridge=true;


      bridgeConsole
        .userData
        .light
        .material
        .emissive
        .setHex(
          0x58ff9a
        );


      applyWorld();

      setMission(3);


      showToast(
        'SKYBRIDGE ONLINE. The Aurelia crossing is restored — now shift to Vanta for the power cell.'
      );

    }

  };


cell.userData.fn =
  ()=>{

    if(
      mission===3 &&
      world.value===1 &&
      !flags.cell
    ){

      flags.cell=true;

      cell.visible=false;

      setMission(4);

      showToast(
        'Power cell secured. Return to the city garage.'
      );

    }

  };


garage.userData.fn =
  ()=>{

    if(
      mission===4 &&
      flags.cell &&
      !flags.garage
    ){

      flags.garage=true;


      garage
        .userData
        .light
        .material
        .emissive
        .setHex(
          0x58ff9a
        );


      setMission(5);


      showToast(
        'Rift Runner unlocked. Enter it with E and drive south.'
      );

    }

  };


facilityEntry.userData.fn =
  ()=>{

    if(mission!==5)
      return;

    if(inVehicle){
      inVehicle=false;
      player.visible=true;
      player.position.copy(vehicle.position);
      player.position.y=1.1;
      vehicleVelocity=0;
      $('vehicleDrive').style.display='none';
    }

    setMission(6);

    showToast(
      'Industrial Basin secured. Activate the three nearby power relays.'
    );

  };


relays.forEach(
  (g,i)=>{

    g.userData.fn =
      ()=>{

        if(
          mission===6 &&
          !flags.relays[i]
        ){

          flags.relays[i]=true;

          g.userData
            .light
            .material
            .emissive
            .setHex(
              0x55ff9c
            );


          signal =
            clamp(
              signal+8,
              0,
              100
            );


          if(
            flags.relays.every(
              Boolean
            )
          ){

            setMission(7);

            showToast(
              'POWER RESTORED — Core access unlocked.'
            );

          }
          else{

            showToast(
              'Relay '+
              (i+1)+
              ' online.'
            );

          }

        }

      };

  }
);


coreDoor.userData.fn =
  ()=>{

    if(
      mission===7 &&
      flags.relays.every(Boolean)
    ){

      flags.core=true;

      setMission(8);

      spawnGuardian();

      showToast(
        'RIFT CORE OPEN. Defeat the Guardian.'
      );

      return;
    }

  
    if(
      mission===10 &&
      guardianDefeated &&
      flags.stabilizers.every(Boolean)
    ){

      showEnding();
    }

  };


anomaly.userData.available =
  ()=>mission===1;

bridgeConsole.userData.available =
  ()=>mission===2;

cell.userData.available =
  ()=>mission===3 &&
  world.value===1;

garage.userData.available =
  ()=>mission===4 &&
  flags.cell;

facilityEntry.userData.available =
  ()=>mission===5;


relays.forEach(
  (r,i)=>{

    r.userData.available =
      ()=>mission===6 &&
      !flags.relays[i];

  }
);


coreDoor.userData.available =
  ()=>
    (mission===7 && flags.relays.every(Boolean)) ||
    (mission===10 && guardianDefeated && flags.stabilizers.every(Boolean));


vehicle.userData.available =
  ()=>mission>=5 &&
  flags.garage;




function toggleGuide(){

  if(!started)
    return;


  guideOpen =
    !guideOpen;


  if(guideOpen){

    document
      .exitPointerLock?.();

    $('guide').style.display='flex';

    updateGuide();

  }
  else{

    $('guide').style.display='none';

    renderer
      .domElement
      .requestPointerLock?.();

  }

}


function updateGuide(){

  $('guideMission')
    .textContent=
      missions[mission][0];

  $('guideMissionDesc')
    .textContent=
      missions[mission][1];


  $('guideSteps')
    .innerHTML =

    missions.map(
      (m,i)=>

      `
        <div class="
          guideStep
          ${i===mission?'current ':''}
          ${i<mission?'done':''}
        ">

          <span class="n">
            ${i<mission?'✓':i+1}
          </span>

          <span>
            ${m[0]}
          </span>

        </div>
      `
    ).join('');

}


function updateCompass(){

  const p =
    objectivePos();


  if(
    !started ||
    !p
  ){

    $('compassName')
      .textContent=
        'NO OBJECTIVE';

    $('compassDistance')
      .textContent='—';

    return;

  }


  const dx =
    p.x -
    player.position.x;

  const dz =
    p.z -
    player.position.z;


  const dist =
    Math.hypot(
      dx,
      dz
    );


  $('compassName')
    .textContent=
      missions[
        Math.min(
          mission,
          missions.length-1
        )
      ][0];


  $('compassDistance')
    .textContent=
      Math.round(dist)+
      ' m';


  const targetBearing =
    Math.atan2(
      dx,
      -dz
    );


  const heading =
    -yaw;


  let rel =
    targetBearing -
    heading;


  rel =
    Math.atan2(
      Math.sin(rel),
      Math.cos(rel)
    );


  $('compassArrow')
    .style.transform=
      `
        translateX(-50%)
        rotate(${rel}rad)
      `;

}


$('guideClose').onclick =
  toggleGuide;




function applyWorld(){

  const a =
    world.value===0;


  $('world').textContent =
    a
      ? 'AURELIA'
      : 'VANTA';


  scene.background
    .setHex(
      a
        ? 0x77bed5
        : 0x24151b
    );


  scene.fog.color
    .setHex(
      a
        ? 0x77bed5
        : 0x24151b
    );


  scene.fog.near =
    a ? 75 : 55;


  scene.fog.far =
    a ? 290 : 220;


  hemi.color
    .setHex(
      a
        ? 0xcfefff
        : 0xffc1b2
    );


  hemi.groundColor
    .setHex(
      a
        ? 0x263e22
        : 0x3a2824
    );


  sun.intensity =
    a ? 2 : 1.45;


  fill.color
    .setHex(
      a
        ? 0x48c8ff
        : 0xff654f
    );


  fill.intensity =
    a ? 55 : 75;


  trees.forEach(
    t=>{

      t.visible=true;

      t.userData
        .crown
        .material
        .color
        .setHex(
          a
            ? 0x40ad67
            : 0x283330
        );


      t.userData
        .crown
        .material
        .emissive
        ?.setHex(
          a
            ? 0x000000
            : 0x061a17
        );

    }
  );


  worldPlatforms.forEach(
    p=>{
      p.visible =
        p.userData.world ===
        world.value;
    }
  );


  scene.traverse(
    o=>{

      if(
        o.userData &&
        o.userData.world!==undefined &&
        o.userData.world!==null &&
        !worldPlatforms.includes(o)
      ){

        o.visible =
          o.userData.world ===
          world.value;

      }

    }
  );


  npcs.forEach(
    n=>{

      n.visible =
        !(n.userData.vantaOnly && a) &&
        !(n.userData.aureliaOnly && !a);

    }
  );

  interactables.forEach(o=>{
    if(o.userData && (o.userData.aureliaOnly || o.userData.vantaOnly)){
      o.visible=!(o.userData.vantaOnly && a)&&!(o.userData.aureliaOnly&&!a);
    }
  });


  flash.style.opacity='.48';

  setTimeout(
    ()=>{
      flash.style.opacity='0'
    },
    90
  );


  buildWeaponVisual();

}


function shiftWorld(){

  if(
    dialogueOpen ||
    inVehicle
  )
    return;


  if(
    energy<28
  ){

    showToast(
      'Not enough Rift energy.'
    );

    return;

  }


  energy-=28;


  world.value =
    1 -
    world.value;


  signal =
    clamp(
      signal+10,
      0,
      100
    );


  applyWorld();


  showToast(
    world.value===0
      ? 'AURELIA — the living timeline.'
      : 'VANTA — the ruined timeline.'
  );

}




function groundY(
  x,
  z
){

  let y=0;


  for(
    const p of worldPlatforms
  ){

    if(
      p.visible &&
      x >
        p.position.x -
        p.geometry.parameters.width/2 &&
      x <
        p.position.x +
        p.geometry.parameters.width/2 &&
      z >
        p.position.z -
        p.geometry.parameters.depth/2 &&
      z <
        p.position.z +
        p.geometry.parameters.depth/2
    ){

      y=
        Math.max(
          y,
          p.position.y+.45
        );

    }

  }


  return y;

}


const holes = [

  {
    x1:-57,
    x2:57,
    z1:-38,
    z2:-31
  },

  {
    x1:22,
    x2:57,
    z1:-72,
    z2:-58
  },

  {
    x1:-50,
    x2:-21,
    z1:-101,
    z2:-88
  },

  {
    x1:-18,
    x2:18,
    z1:-207,
    z2:-198
  }

];


function canStand(
  x,
  z
){

  const playerRadius = 0.55;

  for(
    const o of solidObstacles
  ){
    if(o.type==='tree'){
      const dx=x-o.x;
      const dz=z-o.z;
      const r=o.radius+playerRadius;

      if(dx*dx+dz*dz < r*r)
        return false;
    }else{
      const nearestX = Math.max(
        o.x-o.halfW,
        Math.min(x,o.x+o.halfW)
      );

      const nearestZ = Math.max(
        o.z-o.halfD,
        Math.min(z,o.z+o.halfD)
      );

      const dx=x-nearestX;
      const dz=z-nearestZ;

      if(dx*dx+dz*dz < playerRadius*playerRadius)
        return false;
    }
  }

  for(
    const h of holes
  ){

    if(
      x>h.x1 &&
      x<h.x2 &&
      z>h.z1 &&
      z<h.z2
    ){

      const bridge =
        worldPlatforms.some(
          p=>

          p.visible &&

          x>
            p.position.x -
            p.geometry.parameters.width/2 &&

          x<
            p.position.x +
            p.geometry.parameters.width/2 &&

          z>
            p.position.z -
            p.geometry.parameters.depth/2 &&

          z<
            p.position.z +
            p.geometry.parameters.depth/2
        );


      if(!bridge)
        return false;

    }

  }


  return (
    x>-56 &&
    x<56 &&
    z>-276 &&
    z<74
  );

}


function move(dt){



  if(inVehicle){

    let throttle=0;

    if(keys.has('KeyW'))
      throttle+=1;

    if(keys.has('KeyS'))
      throttle-=.7;


    const boost =
      keys.has('ShiftLeft') ||
      keys.has('ShiftRight');


    const maxSpeed =
      boost
        ? 27
        : 17;


    vehicleVelocity +=
      throttle *
      24 *
      dt;


    vehicleVelocity *=
      Math.pow(
        .08,
        dt
      );


    vehicleVelocity =
      clamp(
        vehicleVelocity,
        -8,
        maxSpeed
      );


    if(keys.has('KeyA'))

      yaw +=
        1.45 *
        dt *
        (
          Math.abs(vehicleVelocity)/8+
          .45
        );


    if(keys.has('KeyD'))

      yaw -=
        1.45 *
        dt *
        (
          Math.abs(vehicleVelocity)/8+
          .45
        );


    vehicle.rotation.y =
      yaw;


    const dir =
      new THREE.Vector3(
        -Math.sin(yaw),
        0,
        -Math.cos(yaw)
      );


    const nx =
      vehicle.position.x+
      dir.x*
      vehicleVelocity*
      dt;


    const nz =
      vehicle.position.z+
      dir.z*
      vehicleVelocity*
      dt;


    if(
      canStand(
        nx,
        nz
      )
    ){

      vehicle.position.x=nx;
      vehicle.position.z=nz;

    }
    else{

      vehicleVelocity*=-.2;

      if(toastTimer<=0){

        showToast(
          'Vehicle blocked — find a route through the active reality.'
        );

      }

    }


    player.position.set(
      vehicle.position.x,
      1.15,
      vehicle.position.z
    );


    player.rotation.y =
      yaw;


    for(
      const t of thrusters
    ){

      t.scale.setScalar(
        1+
        (
          boost
            ? .35
            : .12
        )*
        Math.sin(
          gameTime*16
        )**2
      );

    }


    onGround=true;
    velY=0;

    return;

  }



  const speed=6.2;

  let ix=0;
  let iz=0;


  if(keys.has('KeyW'))
    iz--;

  if(keys.has('KeyS'))
    iz++;

  if(keys.has('KeyA'))
    ix--;

  if(keys.has('KeyD'))
    ix++;


  if(ix||iz){

    const len =
      Math.hypot(
        ix,
        iz
      );

    ix/=len;
    iz/=len;


    const f =
      new THREE.Vector3(
        -Math.sin(yaw),
        0,
        -Math.cos(yaw)
      );


    const r =
      new THREE.Vector3(
        Math.cos(yaw),
        0,
        -Math.sin(yaw)
      );


    const dir =
      new THREE.Vector3()
        .addScaledVector(
          f,
          -iz
        )
        .addScaledVector(
          r,
          ix
        )
        .normalize();


    const nx =
      player.position.x+
      dir.x*
      speed*
      dt;


    const nz =
      player.position.z+
      dir.z*
      speed*
      dt;


    if(
      canStand(
        nx,
        nz
      )
    ){

      player.position.x=nx;
      player.position.z=nz;
      player.rotation.y=yaw;

    }
    else
    if(toastTimer<=0){

      showToast(
        'The fracture blocks this route. Switch realities or use the active bridge.'
      );

    }

  }


  const gy =
    groundY(
      player.position.x,
      player.position.z
    );


  if(
    keys.has('Space') &&
    !inVehicle &&
    onGround
  ){

    velY=7.4;
    onGround=false;

  }


  velY-=19*dt;

  player.position.y +=
    velY*dt;


  if(
    player.position.y<=gy+1.08
  ){

    player.position.y=
      gy+1.08;

    velY=0;
    onGround=true;

  }


  if(
    player.position.y<-4
  ){

    player.position.set(
      0,
      1.1,
      68
    );

    velY=0;

    hp =
      clamp(
        hp-15,
        1,
        100
      );

    showToast(
      'The Rift returned you to the last safe ground.'
    );

  }

}



function cameraUpdate(dt){

  const subject =
    inVehicle
      ? vehicle
      : player;


  const f =
    new THREE.Vector3(
      -Math.sin(yaw),
      0,
      -Math.cos(yaw)
    );


  const desired =
    subject.position
      .clone()
      .addScaledVector(
        f,
        inVehicle
          ? -11
          : -6.2
      );


  desired.y +=
    inVehicle
      ? 5.2
      : 3.5;


  camera.position.lerp(
    desired,
    1-Math.pow(
      .0015,
      dt
    )
  );


  const aimPitch =
    clamp(pitch, -1.05, 0.95);

  const lookDir =
    new THREE.Vector3(
      -Math.sin(yaw) * Math.cos(aimPitch),
      Math.sin(aimPitch),
      -Math.cos(yaw) * Math.cos(aimPitch)
    ).normalize();

  const look =
    subject.position.clone();

  look.y +=
    inVehicle
      ? 1.1
      : 1.35;

  look.add(
    lookDir.multiplyScalar(8)
  );

  camera.lookAt(look);


  weaponRig.visible =
    !inVehicle &&
    !guideOpen;

}




function nearest(){

  let best=null;
  let bd=Infinity;


  for(
    const o of interactables
  ){

    if(!o.visible)
      continue;


    if(
      o.userData.vantaOnly &&
      world.value===0
    )
      continue;


    if(
      o.userData.aureliaOnly &&
      world.value===1
    )
      continue;


    if(
      o.userData.available &&
      !o.userData.available()
    )
      continue;


    const d =
      o.position.distanceTo(
        player.position
      );


    if(
      d<
      (
        o.userData.range ||
        4
      ) &&
      d<bd
    ){

      best=o;
      bd=d;

    }

  }


  return best;

}


function nearestObjectiveInteractable(){
  const targetPos=objectivePos();
  let best=null;
  let bestDist=Infinity;

  for(const o of interactables){
    if(o===objectiveBeacon || !o.visible)
      continue;

    if(o.userData.available && !o.userData.available())
      continue;

    const d=o.position.distanceTo(player.position);
    const targetDist=o.position.distanceTo(targetPos);

    if(
      targetDist<3.5 &&
      d<=(o.userData.range||5)+0.75 &&
      d<bestDist
    ){
      best=o;
      bestDist=d;
    }
  }

  return best;
}

function doInteract(o){

  if(!o)
    return;


  const k =
    o.userData.kind;


  if(k==='objectiveBeacon'){
    const target=nearestObjectiveInteractable();
    if(target)
      target.userData.fn();
    return;
  }

  if(k==='npc'){

    const need =
      o.userData.npc.need;


    if(
      mission>=need &&
      mission<=need+3
    ){

      showDialogue(o);

    }
    else{

      showToast(
        o.userData.npc.name+
        ': I will be here if you need me.'
      );

    }


    return;

  }


  if(k==='vehicle'){

    if(!flags.garage){

      showToast(
        'The Rift Runner is locked.'
      );

      return;

    }


    inVehicle =
      !inVehicle;


    if(inVehicle){

      player.position.set(
        vehicle.position.x,
        1.15,
        vehicle.position.z
      );

      vehicle.rotation.y =
        yaw;

      vehicleVelocity=0;

      player.visible=false;

      weaponRig.visible=false;

      $('vehicleDrive')
        .style.display='block';


      showToast(
        'RIFT RUNNER ONLINE — W/S drive · A/D steer · SHIFT boost · E exit'
      );

    }
    else{

      player.visible=true;

      player.position.copy(
        vehicle.position
      );

      player.position.y=1.1;

      vehicleVelocity=0;

      $('vehicleDrive')
        .style.display='none';


      showToast(
        'Rift Runner parked.'
      );

    }


    return;

  }


  if(
    k==='locker' ||
    k==='supply'
  ){

    o.userData.fn();

    return;

  }


  if(
    k==='machine' ||
    k==='anomaly' ||
    k==='cell' ||
    k==='sidequest'
  ){

    o.userData.fn();

  }

}


function updatePrompt(){

  const near =
    nearest();


  for(
    const o of interactables
  ){

    if(o.userData.marker){

      const active =
        o===near ||

        (
          o.userData.available &&
          o.userData.available() &&
          o.userData.kind!=='npc' &&
          o.position.distanceTo(
            player.position
          )<14
        );


      o.userData
        .marker
        .material
        .emissiveIntensity =
          active
            ? 4.5
            : 1.2;


      o.userData
        .marker
        .material
        .color
        .setHex(
          active
            ? 0x74efff
            : 0x516579
        );

    }

  }


  if(!near){

    $('prompt')
      .style.opacity='0';

    return;

  }


  let txt =
    near.userData.kind==='npc'
      ? 'Talk to '+
        near.userData.npc.name
      : near.userData.kind==='objectiveBeacon'
        ? 'Use Objective Beacon — '+missions[mission][0]
        : near.userData.label;


  if(
    near.userData.kind==='vehicle'
  ){

    txt =
      inVehicle
        ? 'Exit Rift Runner'
        : 'Enter Rift Runner';

  }


  $('prompt')
    .innerHTML =
      '<span class="key">E</span>'+
      txt;


  $('prompt')
    .style.opacity='1';

}



function aimDir(){

  return new THREE.Vector3(
    0,
    0,
    -1
  )
  .applyQuaternion(
    camera.quaternion
  )
  .normalize();

}


function shoot(){

  if(
    !started ||
    dialogueOpen ||
    inVehicle ||
    performance.now()/1000 -
      lastShot <
      weapons[
        weaponIndex
      ].rate
  )
    return;


  const w =
    weapons[
      weaponIndex
    ];


  if(w.melee){

    lastShot =
      performance.now()/1000;

    meleeTimer=.24;

    const origin =
      camera.position.clone();

    const d =
      aimDir();

    hitScan(
      origin,
      d,
      w
    );

    weaponRig.rotation.x=-.58;
    weaponRig.rotation.y=-.16;
    weaponRig.position.z=-1.06;

    setTimeout(
      ()=>{
        weaponRig.rotation.x=0;
        weaponRig.rotation.y=0;
        weaponRig.position.z=-1.38;
      },
      145
    );

    updateWeaponUI();

    return;

  }


  if(w.mag<=0){

    reload();

    return;

  }


  w.mag--;

  lastShot =
    performance.now()/1000;


  const origin =
    camera.position.clone();


  const base =
    aimDir();


  const shots =
    w.pellets ||
    1;


  for(
    let i=0;
    i<shots;
    i++
  ){

    const d =
      base.clone();


    d.x +=
      (
        Math.random()-.5
      )*
      w.spread;


    d.y +=
      (
        Math.random()-.5
      )*
      w.spread*.65;


    d.z +=
      (
        Math.random()-.5
      )*
      w.spread;


    d.normalize();


    hitScan(
      origin,
      d,
      w
    );

  }


  weaponRig.position.z=
    -1.38;


  setTimeout(
    ()=>{
      weaponRig.position.z=
        -1.31
    },
    45
  );


  updateWeaponUI();

}

function hitScan(
  o,
  d,
  w
){

  let best=null;
  let bestProj=Infinity;


  for(
    const e of enemies
  ){

    if(
      e.userData.dead ||
      !e.visible
    )
      continue;


    if(
      e.userData.world!==null &&
      e.userData.world!==world.value
    )
      continue;


    const center =
      e.position
        .clone()
        .add(
          new THREE.Vector3(
            0,
            1,
            0
          )
        );


    const to =
      center
        .clone()
        .sub(o);


    const proj =
      to.dot(d);


    if(
      proj<0 ||
      proj>w.range ||
      proj>bestProj
    )
      continue;


    const closest =
      o.clone()
        .addScaledVector(
          d,
          proj
        );


    const radius =
      e.userData.guardian
        ? 2.2
        : .9;


    if(
      closest.distanceTo(center)<
      radius
    ){

      best=e;
      bestProj=proj;

    }

  }


  if(best)
    damageEnemy(
      best,
      w.damage
    );


  makeTracer(
    o,
    d,
    w.range,
    w.color
  );

}


function makeTracer(
  o,
  d,
  len,
  color
){

  const end =
    o.clone()
      .addScaledVector(
        d,
        len
      );


  const g =
    new THREE.BufferGeometry()
      .setFromPoints([
        o,
        end
      ]);


  const l =
    new THREE.Line(
      g,
      new THREE.LineBasicMaterial({
        color,
        transparent:true,
        opacity:.9
      })
    );


  scene.add(l);


  setTimeout(
    ()=>{
      scene.remove(l);
      g.dispose();
    },
    70
  );

}


function damageEnemy(
  e,
  dmg
){

  if(
    e.userData.guardian &&
    world.value===1
  ){

    showToast(
      'Guardian shield active in VANTA — switch to AURELIA!'
    );

    return;

  }


  e.userData.hp-=dmg;

  signal =
    clamp(
      signal+1,
      0,
      100
    );


  burst(
    e.position
      .clone()
      .add(
        new THREE.Vector3(
          0,
          1,
          0
        )
      ),
    e.userData.col,
    8
  );


  updateEnemyBar(e);


  if(
    e.userData.hp<=0
  ){

    e.userData.dead=true;

    e.visible=false;


    burst(
      e.position
        .clone()
        .add(
          new THREE.Vector3(
            0,
            1,
            0
          )
        ),
      e.userData.col,
      24
    );


    if(
      e.userData.guardian
    ){

      guardianDefeated=true;
      finalePulse=2.5;
      setMission(9);

      showToast('GUARDIAN DEFEATED — RIFT CORE INSTABILITY DETECTED.');

      setTimeout(()=>{
        if(guardianDefeated && mission===9)
          showToast('MIRA: The Core is collapsing! Activate all three stabilizers!');
      },1800);

      setTimeout(()=>{
        if(guardianDefeated && mission===9)
          showToast('Your compass marks the next stabilization node. Move fast.');
      },4200);

    }
    else{

      const cullActive=sideQuest.id==='cull'&&!sideQuest.completed;
      if(cullActive) sideQuestProgress();
      if(!cullActive || !sideQuest.completed){
        showToast(
          e.userData.type==='brute'
            ? 'Rift Brute defeated.'
            : 'Rift creature eliminated.'
        );
      }

    }

  }

}


function updateEnemyBar(
  e
){

  const sp =
    e.children.find(
      c=>c.isSprite
    );


  if(!sp)
    return;


  const c =
    sp.material.map.image
      .getContext('2d');


  c.clearRect(
    0,
    0,
    300,
    85
  );


  const u=e.userData;


  const color =
    '#'+
    u.col
      .toString(16)
      .padStart(
        6,
        '0'
      );


  c.font =
    '900 22px Arial';

  c.textAlign='center';


  c.fillStyle='#fff';


  c.fillText(
    u.type==='brute'
      ? 'RIFT BRUTE'
      : u.type==='warden'
        ? 'RIFT WARDEN'
        : u.type==='hunter'
          ? 'REALITY HUNTER'
          : 'RIFT STALKER',
    150,
    23
  );


  c.fillStyle =
    'rgba(255,255,255,.18)';


  c.fillRect(
    30,
    40,
    240,
    12
  );


  c.fillStyle=color;


  c.fillRect(
    30,
    40,
    240 *
      clamp(
        u.hp/u.maxHp,
        0,
        1
      ),
    12
  );


  sp.material
    .map
    .needsUpdate=true;

}


function reload(){

  const w=
    weapons[
      weaponIndex
    ];

  if(w.melee)
    return;


  const need =
    w.magSize-w.mag;


  if(
    need<=0 ||
    w.reserve<=0
  )
    return;


  const take =
    Math.min(
      need,
      w.reserve
    );


  w.mag+=take;

  w.reserve-=take;

  updateWeaponUI();

}


function equip(i){

  weaponIndex=i;

  buildWeaponVisual();

  showToast(
    'Equipped '+
    weapons[i].name
  );

}


function updateWeaponUI(){

  $('weaponName')
    .textContent=
      weapons[
        weaponIndex
      ].name;


  $('ammo')
    .textContent =
      weapons[weaponIndex].melee
        ? 'MELEE'
        : weapons[weaponIndex].mag+
          ' / '+
          weapons[weaponIndex].reserve;


  $('weaponSlot')
    .innerHTML =

    weapons.map(
      (w,i)=>

      `
        <span class="${
          i===weaponIndex
            ? 'sel'
            : ''
        }">
          ${i+1}
          ${
            w.name
              .replace(
                'Rift ',
                ''
              )
          }
        </span>
      `
    ).join('');

}




function resetEnemiesAfterRespawn(){

  for(const e of enemies){

    if(e.userData.guardian)
      continue;

    e.userData.dead=false;
    e.userData.hp=e.userData.maxHp;
    e.userData.attack=0;
    e.position.set(
      e.userData.spawnX,
      0,
      e.userData.spawnZ
    );
    e.rotation.set(0,0,0);
    e.visible=true;
    updateEnemyBar(e);

  }

}


function hurt(d){

  if(inVehicle)
    d*=.4;


  hp =
    clamp(
      hp-d,
      0,
      100
    );


  $('hitFlash')
    .style.opacity='.85';


  setTimeout(
    ()=>{
      $('hitFlash')
        .style.opacity='0'
    },
    90
  );


  if(hp<=0){

    hp=100;

    inVehicle=false;

    vehicleVelocity=0;

    player.visible=true;

    $('vehicleDrive')
      .style.display='none';


    player.position.set(
      checkpoint.x,
      1.1,
      checkpoint.z
    );


    world.value =
      checkpoint.world;


    resetEnemiesAfterRespawn();
    applyWorld();


    showToast(
      'You were pulled back to your last Rift checkpoint.'
    );

  }

}


function enemyAI(dt){

  for(
    const e of enemies
  ){

    if(
      e.userData.dead ||
      !e.visible
    )
      continue;


    const u =
      e.userData;


    if(
      u.guardian &&
      mission===8
    ){

      const d =
        e.position
          .distanceTo(
            player.position
          );


      u.attack-=dt;


      if(d<34){

        e.lookAt(
          player.position.x,
          e.position.y,
          player.position.z
        );


        if(
          d>5 &&
          !inVehicle
        ){

          e.position.add(
            player.position
              .clone()
              .sub(e.position)
              .normalize()
              .multiplyScalar(
                u.speed*dt
              )
          );

        }


        if(
          world.value===1 &&
          u.attack<=0
        ){

          u.attack=1.4;

          shootEnemyOrb(e);

        }


        if(
          d<5 &&
          u.attack<=0
        ){

          u.attack=.95;

          hurt(20);

        }

      }


      continue;

    }


    if(
      u.world!==null &&
      u.world!==world.value
    )
      continue;


    const to =
      player.position
        .clone()
        .sub(e.position);


    const d =
      to.length();


    u.attack-=dt;


    if(d<25){

      e.lookAt(
        player.position.x,
        e.position.y,
        player.position.z
      );


      if(
        d>2.2 &&
        !inVehicle
      ){

        e.position.add(
          to.normalize()
            .multiplyScalar(
              u.speed*dt
            )
        );

      }


      if(
        d<2.8 &&
        !inVehicle &&
        u.attack<=0
      ){

        u.attack =
          u.type==='brute'
            ? 1.6
            : .95;


        hurt(
          u.type==='brute'
            ? 18
            : 10
        );

      }


      if(
        (
          u.type==='warden' ||
          u.type==='hunter'
        ) &&
        d>8 &&
        u.attack<=0
      ){

        u.attack =
          u.type==='hunter'
            ? 1.6
            : 2.1;


        shootEnemyOrb(e);

      }

    }

  }

}


function shootEnemyOrb(e){

  const dir =
    player.position
      .clone()
      .add(
        new THREE.Vector3(
          0,
          1,
          0
        )
      )
      .sub(
        e.position
          .clone()
          .add(
            new THREE.Vector3(
              0,
              1,
              0
            )
          )
      )
      .normalize();


  const p =
    sphere(
      .16,
      mat(
        0xff4b66,
        .2,
        .5,
        0xff2444
      ),
      8
    );


  p.position.copy(
    e.position
  )
  .add(
    new THREE.Vector3(
      0,
      1,
      0
    )
  );


  p.userData={
    v:dir.multiplyScalar(10),
    life:3.5
  };


  scene.add(p);

  projectiles.push(p);

}


function updateProjectiles(dt){

  for(
    let i=
      projectiles.length-1;
    i>=0;
    i--
  ){

    const p =
      projectiles[i];


    p.position.addScaledVector(
      p.userData.v,
      dt
    );


    p.userData.life-=dt;


    if(
      p.position
        .distanceTo(
          player.position
        )<1.25
    ){

      hurt(12);

      p.userData.life=0;

    }


    if(
      p.userData.life<=0
    ){

      scene.remove(p);

      projectiles.splice(
        i,
        1
      );

    }

  }

}




function burst(
  pos,
  color,
  n
){

  for(
    let i=0;
    i<Math.min(n,20);
    i++
  ){

    const p =
      sphere(
        .055,
        mat(
          color,
          .3,
          .2,
          color
        ),
        6
      );


    p.position.copy(
      pos
    );


    p.userData={
      v:new THREE.Vector3(
        (Math.random()-.5)*6,
        Math.random()*7,
        (Math.random()-.5)*6
      ),
      life:.35+
        Math.random()*.6
    };


    scene.add(p);

    particles.push(p);

  }

}


function updateParticles(dt){

  for(
    let i=
      particles.length-1;
    i>=0;
    i--
  ){

    const p =
      particles[i];


    p.position.addScaledVector(
      p.userData.v,
      dt
    );


    p.userData.v.y -=
      10*dt;


    p.userData.life -=
      dt;


    if(
      p.userData.life<=0
    ){

      scene.remove(p);

      particles.splice(
        i,
        1
      );

    }

  }

}




function objectivePos(){

  switch(mission){

    case 0:
      return npcs[0].position;

    case 1:
      return anomaly.position;

    case 2:
      return bridgeConsole.position;

    case 3:
      return cell.position;

    case 4:
      return garage.position;

    case 5:
      return facilityEntry.position;

    case 6:
      return (
        relays.find(
          (r,i)=>
            !flags.relays[i]
        )?.position ||
        relays[0].position
      );

    case 7:
      return coreDoor.position;

    case 8:
      return guardian?.position ||
        player.position;

    case 9:
      return (
        stabilizers.find(
          (r,i)=>!flags.stabilizers[i]
        )?.position ||
        stabilizers[0].position
      );

    case 10:
      return coreDoor.position;

    default:
      return player.position;

  }

}


function updateObjectiveVisibility(){

  interactables.forEach(
    o=>{

      const k =
        o.userData.kind;

      if(
        k==='machine' ||
        k==='anomaly' ||
        k==='cell'
      ){

        o.children[0]
          ?.userData &&
        (
          o.children[0]
            .userData.target=true
        );

      }

    }
  );

}




let symbolicLeft = null;
let symbolicRight = null;
let symbolicCore = null;
let symbolicBurst = null;

function setCutsceneText(title, subtitle='', kicker='Rift Core'){
  $('cutsceneKicker').textContent=title==='THE END' ? 'Parallel Worlds — Fracture' : kicker;
  $('cutsceneTitle').textContent=title;
  $('cutsceneSubtitle').textContent=subtitle;
  $('cutsceneTitle').style.opacity='1';
  $('cutsceneSubtitle').style.opacity=subtitle ? '1' : '0';
}

function makeSymbolicOrb(color, radius, y, x){
  const g=new THREE.Group();
  g.position.set(x,y,-269);

  const halo=new THREE.Mesh(
    new THREE.TorusGeometry(radius,.07,12,72),
    new THREE.MeshStandardMaterial({
      color,
      emissive:color,
      emissiveIntensity:3,
      transparent:true,
      opacity:.95
    })
  );

  const inner=new THREE.Mesh(
    new THREE.SphereGeometry(.42,18,14),
    new THREE.MeshStandardMaterial({
      color,
      emissive:color,
      emissiveIntensity:5,
      transparent:true,
      opacity:.9
    })
  );

  g.add(halo,inner);
  g.userData.halo=halo;
  g.userData.inner=inner;
  scene.add(g);
  return g;
}

function restoreAfterCutscene(){
  cutsceneActive=false;
  $('cutsceneOverlay').style.display='none';
  $('flash').style.opacity='0';
  $('cutsceneTitle').style.opacity='0';
  $('cutsceneSubtitle').style.opacity='0';

  if(symbolicLeft){ scene.remove(symbolicLeft); symbolicLeft=null; }
  if(symbolicRight){ scene.remove(symbolicRight); symbolicRight=null; }
  if(symbolicCore){ scene.remove(symbolicCore); symbolicCore=null; }
  if(symbolicBurst){ scene.remove(symbolicBurst); symbolicBurst=null; }

  weaponRig.visible=true;
  camera.position.copy(cutsceneStartPos);
  cameraUpdate(0.016);
  renderer.domElement.requestPointerLock?.();
}

function runDecisionCutscene(choice){
  if(cutsceneActive) return;

  cutsceneActive=true;
  cutsceneTime=0;
  cutsceneChoice=choice;
  cutsceneStartPos.copy(camera.position);

  $('ending').style.display='none';
  $('cutsceneOverlay').style.display='block';

  started=false;
  dialogueOpen=false;
  keys.clear();
  document.exitPointerLock?.();

  const origin=coreDoor.position.clone().add(new THREE.Vector3(0,2.5,0));

  const outcomes={
    merge:[
      'TWO BECOME ONE',
      'The boundary between Aurelia and Vanta begins to disappear.',
      'Not an ending. A new world.'
    ],
    aurelia:[
      'THE LIGHT REMAINS',
      'Aurelia holds firm as Vanta slips beyond the closing fracture.',
      'One timeline survives. One becomes a memory.'
    ],
    vanta:[
      'THE EMBER SURVIVES',
      'Vanta remains — broken, quiet, but still capable of tomorrow.',
      'The last future gets another chance.'
    ],
    destroy:[
      'THE THREAD IS CUT',
      'The Core releases both timelines and the fracture folds into silence.',
      'No world wins. Both are free.'
    ]
  };

  const o=outcomes[choice];
  setCutsceneText('THE DECISION','The Core listens.','Final Sequence');

  finalePulse=4;

  camera.position.copy(player.position)
    .add(new THREE.Vector3(0,7,12));
  camera.lookAt(origin);

  symbolicLeft=makeSymbolicOrb(0x72e9ff,3.0,3.5,-3.2);
  symbolicRight=makeSymbolicOrb(0xff6786,3.0,3.5,3.2);

  symbolicCore=new THREE.Mesh(
    new THREE.SphereGeometry(.55,20,16),
    new THREE.MeshStandardMaterial({
      color:0xffffff,
      emissive:0xffffff,
      emissiveIntensity:8,
      transparent:true,
      opacity:1
    })
  );
  symbolicCore.position.copy(origin);
  scene.add(symbolicCore);

  setTimeout(()=>{
    if(!cutsceneActive) return;
    setCutsceneText(o[0],o[1],'Rift Core');
  },1400);

  setTimeout(()=>{
    if(!cutsceneActive) return;
    setCutsceneText('THE FRACTURE CHANGES',o[2],'Aftermath');
  },4200);

  setTimeout(()=>{
    if(!cutsceneActive) return;
    setCutsceneText('THE END','What remains is shaped by your choice.','Parallel Worlds — Fracture');
  },7000);

  setTimeout(()=>{
    if(!cutsceneActive) return;
    restoreAfterCutscene();
    showFinalAftermath(choice);
  },9300);
}

function showFinalAftermath(choice){
  const epilogues={
    merge:[
      'A New World',
      'The two timelines finally meet without destroying each other. The old boundary is gone. What rises from the ruins is unfamiliar — but shared.'
    ],
    aurelia:[
      'The Living Timeline',
      'Aurelia wakes beneath a clear sky. The Rift is gone. Somewhere in its records, the memory of Vanta remains as proof that another future once existed.'
    ],
    vanta:[
      'The Last Future',
      'Vanta remains beneath a darker sky. It is still wounded, but the future is no longer predetermined. The survivors will decide what comes next.'
    ],
    destroy:[
      'Silence Beyond the Rift',
      'The connection is gone. Aurelia and Vanta continue separately, carrying the scars of the fracture — but neither is trapped by it anymore.'
    ]
  };

  const data=epilogues[choice];

  $('ending').style.display='flex';
  $('endTitle').textContent=data[0];
  $('endText').textContent=data[1];

  document.querySelector('.choices').innerHTML=
      '<div style="width:100%;font-size:14px;opacity:.68;line-height:1.7;margin-bottom:8px;">The fracture is over. Your decision is permanent.</div>'
    + '<div style="width:100%;font-size:22px;font-weight:950;letter-spacing:.12em;margin-bottom:14px;">THE END</div>'
    + '<button id="again">PLAY AGAIN</button>';

  $('again').onclick=()=>location.reload();
}

function updateDecisionCutscene(dt){
  if(!cutsceneActive) return;

  cutsceneTime+=dt;
  const t=cutsceneTime;
  const origin=coreDoor.position.clone().add(new THREE.Vector3(0,2.4,0));

  const phase=Math.min(t/3.4,1);
  const angle=-0.55 + phase*1.25;
  const radius=12 - phase*4.5;

  let cam;

  if(t<6.2){
    cam=origin.clone().add(new THREE.Vector3(
      Math.sin(angle)*radius,
      4.2 + Math.sin(t*.8)*.25,
      Math.cos(angle)*radius
    ));
  }else{
    const wide=Math.min((t-6.2)/2.7,1);
    cam=origin.clone().add(new THREE.Vector3(
      18-10*wide,
      9+3*wide,
      15-7*wide
    ));
  }

  camera.position.lerp(cam,Math.min(1,dt*2.8));
  camera.lookAt(origin);
  weaponRig.visible=false;

  const pulse=1+Math.sin(t*7)*.32;

  for(const g of riftPylons){
    if(g.userData.orb?.material){
      g.userData.orb.material.emissiveIntensity=2.5+pulse*2;
    }
  }

  if(symbolicLeft && symbolicRight && symbolicCore){
    symbolicLeft.rotation.z += dt*.65;
    symbolicRight.rotation.z -= dt*.65;
    symbolicLeft.rotation.y += dt*.45;
    symbolicRight.rotation.y -= dt*.45;

    const breathe=1+Math.sin(t*3)*.08;
    symbolicLeft.scale.setScalar(breathe);
    symbolicRight.scale.setScalar(breathe);
    symbolicCore.scale.setScalar(1+Math.sin(t*6)*.12);

    if(cutsceneChoice==='merge'){
      const p=Math.min(1,Math.max(0,(t-1.2)/5.0));
      const e=p*p*(3-2*p);
      symbolicLeft.position.x=-3.2*(1-e);
      symbolicRight.position.x=3.2*(1-e);
      symbolicLeft.scale.setScalar(1+e*.7);
      symbolicRight.scale.setScalar(1+e*.7);
      symbolicCore.material.emissiveIntensity=8+e*10;
    }
    else if(cutsceneChoice==='aurelia'){
      const p=Math.min(1,Math.max(0,(t-1.4)/4.5));
      symbolicRight.position.y=3.5+p*4.5;
      symbolicRight.scale.setScalar(Math.max(.05,1-p));
      symbolicLeft.scale.setScalar(1+p*.7);
    }
    else if(cutsceneChoice==='vanta'){
      const p=Math.min(1,Math.max(0,(t-1.4)/4.5));
      symbolicLeft.position.y=3.5+p*4.5;
      symbolicLeft.scale.setScalar(Math.max(.05,1-p));
      symbolicRight.scale.setScalar(1+p*.7);
    }
    else if(cutsceneChoice==='destroy'){
      const p=Math.min(1,Math.max(0,(t-2.0)/3.4));
      const e=p*p*(3-2*p);
      symbolicLeft.scale.setScalar(1+e*3.5);
      symbolicRight.scale.setScalar(1+e*3.5);
      symbolicCore.scale.setScalar(1+e*12);
      symbolicCore.material.opacity=Math.max(0,1-e);
    }
  }

  if(cutsceneChoice==='merge'){
    scene.background.lerp(new THREE.Color(0x5d8cc1),Math.min(1,dt*.8));
    fill.color.setHex(0x72f5ff);
  }
  else if(cutsceneChoice==='aurelia'){
    scene.background.lerp(new THREE.Color(0x78bcd6),Math.min(1,dt*.8));
    fill.color.setHex(0x7ddfff);
  }
  else if(cutsceneChoice==='vanta'){
    scene.background.lerp(new THREE.Color(0x241b25),Math.min(1,dt*.8));
    fill.color.setHex(0xff76df);
  }
  else if(cutsceneChoice==='destroy'){
    scene.background.lerp(new THREE.Color(0x05050a),Math.min(1,dt*.65));
    fill.color.setHex(0xcfefff);
  }

  fill.intensity=55+Math.sin(t*5)*12+Math.max(0,3.8-t)*35;

  if(t>7.0){
    $('flash').style.opacity=String(
      Math.max(0,Math.min(.28,(t-7)*.11))
    );
  }
}



function showEnding(){

  if(mission!==10) return;

  started=false;

  document
    .exitPointerLock?.();


  $('ending')
    .style.display='flex';


  $('endText')
    .textContent=
      'The Guardian collapses. The Rift Core is no longer forcing the two timelines together. Your choice will decide what remains.';

}


document
  .querySelectorAll(
    '#ending button'
  )
  .forEach(
    b=>

    b.addEventListener(
      'click',
      ()=>{

        const c =
          b.dataset.choice;


        const data = {

          merge:[
            'A New World',
            'You merge Aurelia and Vanta. The past and future overlap into one unpredictable world where both must rebuild together.'
          ],

          aurelia:[
            'The Living Timeline',
            'You anchor Aurelia. Its cities survive and the fracture closes, leaving Vanta behind as a memory inside the Core.'
          ],

          vanta:[
            'The Last Future',
            'You save Vanta. The survivors inherit a damaged world, but they finally have a future of their own.'
          ],

          destroy:[
            'Silence Beyond the Rift',
            'You destroy the Core. The realities separate forever. Neither world gets everything, but neither is controlled by the Rift again.'
          ]

        }[c];


        $('endTitle')
          .textContent=
            data[0];


        $('endText')
          .textContent=
            data[1];

        
        document
          .querySelector('.choices')
          .innerHTML=
            '<div style="width:100%;text-align:center;opacity:.68;font-size:11px;letter-spacing:.14em;text-transform:uppercase;margin:4px 0 12px;">Decision locked — begin final sequence</div>'
          + '<button id="beginCutscene">CONTINUE</button>';

        $('beginCutscene').onclick =
          ()=>{
            runDecisionCutscene(c);
          };

      }
    )
  );




function riftScan(){

  if(
    !started ||
    guideOpen ||
    dialogueOpen
  )
    return;


  scanTimer=3.2;


  signal =
    clamp(
      signal+8,
      0,
      100
    );


  scanRing.visible=true;

  scanRing.scale.setScalar(.2);


  showToast(
    'RIFT SCAN — nearby NPCs, machinery, caches and the mission route are highlighted.'
  );

}


function updateScan(dt){

  if(scanTimer<=0){

    scanRing.visible=false;

    return;

  }


  scanTimer-=dt;


  const t =
    1-
    scanTimer/3.2;


  scanRing.scale.setScalar(
    .2+t*13
  );


  scanRing.material.opacity =
    clamp(
      1-t,
      0,
      1
    );


  for(
    const o of interactables
  ){

    if(
      o.visible &&
      o.userData.marker
    ){

      o.userData.marker
        .scale.setScalar(
          1+
          Math.sin(
            gameTime*18
          )*.15+
          Math.max(
            0,
            scanTimer
          )*.08
        );

    }

  }

}




function devJumpToMission(targetMission){

  started=true;
  dialogueOpen=false;
  dialogueQueue=[];
  guideOpen=false;
  inVehicle=false;
  vehicleVelocity=0;
  keys.clear();
  guardianDefeated=false;
  finalePulse=0;
  clearSideQuest();
  memoryShards.forEach(g=>g.visible=false);
  supplyCrates.forEach(g=>g.visible=false);
  velY=0;
  onGround=false;


  flags.anomaly = targetMission >= 2;
  flags.bridge = targetMission >= 3;
  flags.cell = targetMission >= 4;
  flags.garage = targetMission >= 5;
  flags.relays = targetMission >= 7
    ? [true,true,true]
    : [false,false,false];
  flags.core = targetMission >= 8;
  flags.stabilizers = targetMission >= 10
    ? [true,true,true]
    : [false,false,false];


  const spots = {
    0:[3,53],
    2:[-31,-26],
    5:[-17,-154.5],
    7:[0,-247.5],
    8:[0,-257],
    9:[-8,-251.5],
    10:[0,-247.5]
  };

  const spot = spots[targetMission] || spots[10];

  player.position.set(
    spot[0],
    1.1,
    spot[1]
  );

  world.value=0;
  applyWorld();

  
  if(targetMission===8){
    spawnGuardian();
    guardianDefeated=false;
    guardian.visible=true;
    guardian.userData.dead=false;
    guardian.userData.hp=140;
    guardian.userData.maxHp=140;
  }
  else if(targetMission<8 && guardian){
    guardian.userData.dead=true;
    guardian.visible=false;
  }
  else if(targetMission>=9){
   
    guardianDefeated=true;
    if(guardian){
      guardian.userData.dead=true;
      guardian.visible=false;
    }
  }

  if(targetMission===8){
    guardian.userData.hp=1;
  }

  setMission(targetMission);

  $('menu').style.display='none';
  $('hud').style.display='block';
  $('prompt').style.opacity='0';
  buildWeaponVisual();

  
  const objective=objectivePos();
  if(objective){
    yaw=Math.atan2(
      -(objective.x-player.position.x),
      -(objective.z-player.position.z)
    );
    pitch=-0.08;
  }

  camera.position.set(
    player.position.x,
    player.position.y+3,
    player.position.z+7
  );
  camera.lookAt(
    player.position.x,
    player.position.y+1.1,
    player.position.z
  );

  showToast(
    targetMission===8
      ? 'DEV TEST — Guardian ready. One hit defeats it.'
      : targetMission===9
        ? 'DEV TEST — Stabilizer 1 ready. Press E when close.'
        : targetMission===10
          ? 'DEV TEST — Final decision ready. Walk to the Core and press E.'
          : 'DEV TEST — Mission '+targetMission+' loaded.'
  );
}

$('start').onclick =
  ()=>{

    started=true;

    $('menu')
      .style.display='none';

    $('hud')
      .style.display='block';


    renderer
      .domElement
      .requestPointerLock?.();


    buildWeaponVisual();


    showToast(
      'MIRA — orange suit, bright nameplate. Walk to her and press E.'
    );

  };


renderer
  .domElement
  .addEventListener(
    'click',
    ()=>{

      if(
        started &&
        !pointer &&
        !dialogueOpen &&
        !guideOpen
      ){

        renderer
          .domElement
          .requestPointerLock?.();

      }

    }
  );


document
  .addEventListener(
    'pointerlockchange',
    ()=>{

      pointer =
        document.pointerLockElement ===
        renderer.domElement;

    }
  );


let mouseDragging=false;

renderer
  .domElement
  .addEventListener(
    'mousedown',
    e=>{
      if(!started || dialogueOpen || guideOpen)
        return;
      if(e.button===0)
        mouseDragging=true;
    }
  );

window
  .addEventListener(
    'mouseup',
    e=>{
      if(e.button===0)
        mouseDragging=false;
    }
  );

document
  .addEventListener(
    'mousemove',
    e=>{

      if(
        (!pointer && !mouseDragging) ||
        dialogueOpen ||
        !started ||
        guideOpen
      )
        return;

      const mx =
        pointer ? e.movementX : e.movementX;

      const my =
        pointer ? e.movementY : e.movementY;

      yaw -= mx*.0026;

      pitch -= my*.0034;


      pitch =
        clamp(
          pitch,
          -1.1,
          1.0
        );

    }
  );


document
  .addEventListener(
    'keydown',
    e=>{

      if(!e.repeat){
        const devTargets={
          F1:0,
          F2:2,
          F3:5,
          F4:7,
          F5:8,
          F6:9,
          F7:10
        };

        if(devTargets[e.code] !== undefined){
          devJumpToMission(devTargets[e.code]);
          return;
        }

        if(e.code==='F8'){
          devJumpToMission(10);
          showToast('DEV TEST — Final decision ready. Walk to the Core and press E.');
          return;
        }
      }

      if(
        e.code==='Escape' &&
        guideOpen
      ){

        toggleGuide();

        return;

      }


      if(
        e.code==='KeyG' &&
        !e.repeat
      ){

        toggleGuide();

        return;

      }


      if(
        e.code==='KeyQ' &&
        !e.repeat
      ){

        riftScan();

        return;

      }


      if(guideOpen)
        return;


      keys.add(
        e.code
      );


      if(
        e.repeat &&
        [
          'KeyE',
          'ShiftLeft',
          'ShiftRight',
          'KeyF',
          'KeyR'
        ].includes(e.code)
      )
        return;


      if(
        e.code==='KeyE'
      ){

        if(dialogueOpen)
          nextDialogue();
        else
          doInteract(
            nearest()
          );

      }

      else if(
        e.code==='ShiftLeft' ||
        e.code==='ShiftRight'
      ){

        shiftWorld();

      }

      else if(
        e.code==='KeyF'
      ){

        shoot();

      }

      else if(
        e.code==='KeyR'
      ){

        reload();

      }

      else if(
        /^Digit[1-7]$/.test(
          e.code
        )
      ){

        equip(
          Number(
            e.code.slice(-1)
          )-1
        );

      }

    }
  );


document
  .addEventListener(
    'keyup',
    e=>
      keys.delete(
        e.code
      )
  );


renderer
  .domElement
  .addEventListener(
    'mousedown',
    e=>{

      if(
        started &&
        !dialogueOpen &&
        pointer &&
        e.button===0
      ){

        shoot();

      }

    }
  );



function updateBeacon(){

  const p =
    objectivePos();


  if(
    !started ||
    !p
  ){

    objectiveBeacon.visible=false;

    return;

  }


  objectiveBeacon.visible=true;


  objectiveBeacon.position.set(
    p.x,
    groundY(
      p.x,
      p.z
    ),
    p.z
  );


  beaconRing.rotation.z =
    gameTime*1.8;


  beaconRing.scale.setScalar(
    1+
    Math.sin(
      gameTime*4
    )*.12
  );


  beaconBeam.scale.y =
    .75+
    Math.sin(
      gameTime*3
    )*.08;

}




function updateUI(dt){

  $('vehicleDrive')
    .style.display =
      inVehicle
        ? 'block'
        : 'none';


  updateCompass();


  $('hp')
    .textContent =
      Math.round(hp)+
      ' / 100';


  $('hpBar')
    .style.width=
      hp+'%';


  $('energy')
    .textContent =
      Math.round(energy)+
      '%';


  $('enBar')
    .style.width=
      energy+'%';


  $('signal')
    .textContent =
      Math.round(signal)+
      '%';


  if(
    toastTimer>0
  ){

    toastTimer-=dt;


    if(
      toastTimer<=0
    ){

      $('toast')
        .style.opacity='0';

    }

  }


  updatePrompt();

}




function tick(){

  requestAnimationFrame(
    tick
  );


  const dt =
    Math.min(
      .032,
      clock.getDelta()
    );


  if(cutsceneActive){
    gameTime+=dt;
    updateParticles(dt);
    updateDecisionCutscene(dt);

  }else if(started){

    gameTime+=dt;


    energy =
      clamp(
        energy+8*dt,
        0,
        100
      );


    signal =
      clamp(
        signal-.7*dt,
        0,
        100
      );


    move(dt);

    cameraUpdate(dt);

    enemyAI(dt);

    updateProjectiles(dt);

    updateParticles(dt);

    updateScan(dt);

    updateBeacon();

    updateUI(dt);

  }


  if(meleeTimer>0)
    meleeTimer-=dt;

  weaponRig.position.y =
    -.68+
    Math.sin(
      gameTime*8
    )*
    (
      keys.size
        ? .018
        : 0
    );


  weaponRig.rotation.z =
    Math.sin(
      gameTime*5
    )*.012;


  for(
    const g of riftPylons
  ){

    g.userData
      .orb
      .position.y =
        3.65+
        Math.sin(
          gameTime*2+
          g.position.x
        )*.18;

  }

  if(finalePulse>0){
    finalePulse=Math.max(0,finalePulse-dt);
    const intensity=1+Math.sin(gameTime*22)*0.35;
    fill.intensity=(world.value===0?55:75)+(finalePulse*45*intensity);
    $('flash').style.opacity=String(Math.min(.22,finalePulse*.06));
  }else{
    fill.intensity=(world.value===0?55:75);
  }


  renderer.render(
    scene,
    camera
  );

}




applyWorld();

updateWeaponUI();

tick();


addEventListener(
  'resize',
  ()=>{

    camera.aspect =
      innerWidth/
      innerHeight;

    camera.updateProjectionMatrix();

    renderer.setSize(
      innerWidth,
      innerHeight
    );

  }
);

</script>

</body>
</html>
