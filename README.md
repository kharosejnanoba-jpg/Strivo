<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Strivo — Live Random Video Chat</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <meta name="description" content="Strivo — Meet new people via secure, privacy-first live video chat." />
  <link rel="icon" href="data:image/svg+xml,...">

  <style>
    /* ---- Professional, compact styles ---- */
    :root{
      --bg:#071026; --card:rgba(255,255,255,0.04); --muted:#9aa6b2; --text:#e6eef6;
      --accent1:#0ea5e9; --accent2:#22d3ee; --radius:14px; --shadow: 0 20px 50px rgba(2,6,23,0.6);
      font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial;
    }
    *{box-sizing:border-box} html,body{height:100%;margin:0;color:var(--text);background:radial-gradient(700px 400px at 10% 10%, rgba(14,165,233,0.08), transparent),var(--bg)}
    .container{width:min(1200px,94%);margin:0 auto;padding-bottom:60px}
    header{padding:18px 0;display:flex;align-items:center;justify-content:space-between}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{width:44px;height:44px;border-radius:10px;display:grid;place-items:center;font-weight:800;background:linear-gradient(135deg,var(--accent1),var(--accent2));color:#022}
    nav{display:flex;gap:12px;align-items:center}
    a{color:var(--muted);text-decoration:none;font-weight:600;padding:8px 10px;border-radius:10px}
    a:hover{background:var(--card);color:var(--text)}
    .cta{background:linear-gradient(135deg,var(--accent1),var(--accent2));color:#022;padding:10px 14px;border-radius:12px;font-weight:800}
    main{display:grid;grid-template-columns:1fr 420px;gap:24px;align-items:start}
    .hero{padding:22px;border-radius:var(--radius);background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);border:1px solid rgba(255,255,255,0.03)}
    h1{margin:6px 0 8px;font-size:36px}
    p.muted{color:var(--muted)}
    .card{background:var(--card);padding:14px;border-radius:12px;border:1px solid rgba(255,255,255,0.04)}
    .video-frame{border-radius:10px;overflow:hidden;border:1px solid rgba(255,255,255,0.04);background:#000;height:260px;display:flex;align-items:center;justify-content:center}
    video{width:100%;height:100%;object-fit:cover;background:#000;display:block}
    .controls{display:flex;gap:8px;margin-top:10px;flex-wrap:wrap}
    button{padding:10px 12px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text);cursor:pointer}
    button.primary{background:linear-gradient(135deg,var(--accent1),var(--accent2));color:#022;border:none}
    .muted-small{font-size:13px;color:var(--muted)}
    .hidden{display:none}
    footer{margin-top:36px;padding:20px 0;color:var(--muted);font-size:13px;text-align:center}
    @media(max-width:980px){ main{grid-template-columns:1fr} .video-frame{height:200px} nav{display:none} }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <div class="logo" aria-hidden="true">S</div>
        <div>
          <div style="font-weight:700">Strivo</div>
          <div class="muted-small">Live. Random. Private.</div>
        </div>
      </div>
      <nav>
        <a href="#home">Home</a><a href="#about">About</a><a href="#safety">Safety</a><a href="#faq">FAQ</a><a href="#contact">Contact</a>
        <a class="cta" href="#chat">Start Chat</a>
      </nav>
    </header>

    <main>
      <!-- Left: hero & info -->
      <section class="hero" id="home" tabindex="-1">
        <span style="display:inline-block;background:rgba(14,165,233,0.08);padding:6px 10px;border-radius:999px;color:#bfefff;font-weight:700">Privacy-first</span>
        <h1>Meet the world — securely & instantly</h1>
        <p class="muted">Strivo connects two people via real-time video. We provide a modern UI, report/block tools and privacy-first defaults. For production reliability we use TURN relays and a signaling server.</p>

        <div style="display:flex;gap:10px;margin-top:14px;flex-wrap:wrap">
          <a class="cta" href="#chat">Start Chat</a>
          <button id="btnLearn" class="button">How it works</button>
        </div>

        <div style="margin-top:18px;display:grid;grid-template-columns:repeat(3,1fr);gap:12px">
          <div class="card"><strong>WebRTC</strong><div class="muted-small">Peer-to-peer low-latency video</div></div>
          <div class="card"><strong>Safety</strong><div class="muted-small">Report / block / moderation hooks</div></div>
          <div class="card"><strong>Scalable</strong><div class="muted-small">Redis + TURN ready for production</div></div>
        </div>
      </section>

      <!-- Right: live preview + controls -->
      <aside class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div><strong>Live Preview</strong><div class="muted-small">Your camera (private preview)</div></div>
          <div class="muted-small">Beta</div>
        </div>

        <div class="video-frame" style="margin-top:12px">
          <video id="preview" playsinline muted></video>
        </div>

        <div class="controls" style="margin-top:12px">
          <button id="enableCam" class="primary">Enable Camera</button>
          <button id="mutePreview">Toggle Mic</button>
          <button id="openChat">Open Chat</button>
        </div>

        <p class="muted-small" style="margin-top:12px">Tip: Use HTTPS in production. For local testing use ngrok or host on an HTTPS-enabled host.</p>
      </aside>
    </main>

    <!-- Chat section (initially hidden until route #chat) -->
    <section id="chat" class="card hidden" tabindex="-1" style="margin-top:24px">
      <h2>Random Video Chat</h2>
      <p class="muted-small">Real matching uses a signaling server + TURN. The UI below connects to your signaling server to find a peer and establish a direct peer-to-peer call (or through TURN).</p>

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:12px">
        <div class="card">
          <strong>You</strong>
          <div class="video-frame" style="height:260px;margin-top:8px"><video id="local" autoplay playsinline muted></video></div>
          <div class="controls" style="margin-top:10px">
            <button id="startLocal" class="primary">Start</button>
            <button id="stopLocal">Stop</button>
            <button id="mirror">Mirror</button>
            <button id="blur">Blur</button>
          </div>
        </div>

        <div class="card">
          <strong>Partner</strong>
          <div class="video-frame" style="height:260px;margin-top:8px"><video id="remote" autoplay playsinline></video></div>
          <div class="muted-small" id="partnerStatus" style="margin-top:8px">Not connected</div>
          <div class="controls" style="margin-top:10px">
            <select id="region" style="padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text)">
              <option value="global">Global</option><option value="india">India</option><option value="usa">USA</option>
            </select>
            <button id="btnFind" class="primary">Find Match</button>
            <button id="btnSkip">Skip</button>
            <button id="btnReport">Report</button>
            <button id="btnBlock">Block</button>
          </div>
        </div>
      </div>

      <div style="margin-top:12px" class="muted-small">Interests filter and premium queues are server-side features you can add later.</div>
    </section>

    <!-- Other pages -->
    <section id="about" class="card hidden" tabindex="-1" style="margin-top:18px">
      <h2>About</h2>
      <p class="muted-small">Strivo is built for private, fast, and friendly conversations. This deployment includes a full signaling server and production guidance to scale with Redis and TURN.</p>
    </section>

    <section id="safety" class="card hidden" tabindex="-1" style="margin-top:18px">
      <h2>Safety Guidelines</h2>
      <ul class="muted-small">
        <li>No nudity, harassment, hate speech, or illegal behavior.</li>
        <li>Use Report / Block to flag abuses. Moderators can review logs.</li>
        <li>Users must be 18+. Minors are not permitted.</li>
      </ul>
    </section>

    <section id="faq" class="card hidden" tabindex="-1" style="margin-top:18px">
      <h2>FAQ</h2>
      <details class="muted-small"><summary>Are chats recorded?</summary><div>No by default. We only store metadata needed for moderation.</div></details>
      <details class="muted-small"><summary>Why TURN?</summary><div>TURN relays media when direct peer-to-peer is blocked by networks.</div></details>
    </section>

    <section id="contact" class="card hidden" tabindex="-1" style="margin-top:18px">
      <h2>Contact</h2>
      <p class="muted-small">Email: <a href="mailto:kharosejnanoba@gmail.com">kharosejnanoba@gmail.com</a></p>
      <p class="muted-small">WhatsApp: <a href="https://wa.me/917499177409" target="_blank">+91 7499177409</a></p>
    </section>

    <footer>
      <div class="muted-small">© <span id="year"></span> Strivo • Built for privacy & speed</div>
    </footer>
  </div>

  <!-- Cookie consent -->
  <div id="consent" style="position:fixed;left:50%;transform:translateX(-50%);bottom:18px;background:var(--card);padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.04);display:none;z-index:60">
    <div style="display:flex;gap:12px;align-items:center">
      <div class="muted-small">We use essential cookies. Accept to enable analytics.</div>
      <div style="margin-left:12px">
        <button id="declineCookies" style="margin-right:6px">Decline</button>
        <button id="acceptCookies" class="primary">Accept</button>
      </div>
    </div>
  </div>

<script>
/* =========================
   Client-side WebRTC + Signaling
   ========================= */

const SIGNALING_SERVER = (location.hostname === 'localhost') ? 'http://localhost:3000' : 'https://YOUR_SIGNALLING_SERVER'; 
// Replace 'https://YOUR_SIGNALLING_SERVER' with your deployed signaling server (https) before production.

const ICE_SERVERS = [
  { urls: 'stun:stun.l.google.com:19302' },
  // Add TURN server here in production:
  // { urls: 'turn:TURN_SERVER:3478', username: 'TURN_USER', credential: 'TURN_PASS' }
];

let socket = null;
let pc = null;
let localStream = null;
let remoteStream = null;
let currentPeerId = null;
let isInitiator = false;

const previewEl = document.getElementById('preview');
const localEl = document.getElementById('local');
const remoteEl = document.getElementById('remote');
const partnerStatus = document.getElementById('partnerStatus');

document.getElementById('year').textContent = new Date().getFullYear();

// --- SPA routing
const routes = ['home','chat','about','safety','faq','contact'];
function routeTo(hash){
  const id = (hash || location.hash.replace('#','') || 'home');
  routes.forEach(r=>{
    const el = document.getElementById(r);
    if(!el) return;
    if(r === id) el.classList.remove('hidden'); else el.classList.add('hidden');
  });
  // focus
  const active = document.querySelector('#' + id + ' h2, #' + id + ' h1');
  if(active){ active.setAttribute('tabindex','-1'); active.focus(); }
}
window.addEventListener('hashchange', ()=>routeTo(location.hash.replace('#','')));
window.addEventListener('load', ()=>{ routeTo(location.hash.replace('#','')); showConsent(); });

// --- cookie consent
const consentKey = 'strivo:consent';
function showConsent(){ if(!localStorage.getItem(consentKey)) document.getElementById('consent').style.display='block'; }
document.getElementById('acceptCookies').onclick = ()=>{ localStorage.setItem(consentKey,'accepted'); document.getElementById('consent').style.display='none'; /* load analytics here */ };
document.getElementById('declineCookies').onclick = ()=>{ localStorage.setItem(consentKey,'declined'); document.getElementById('consent').style.display='none'; };

// --- preview camera
let previewStream = null;
document.getElementById('enableCam').addEventListener('click', async ()=>{
  try{
    if(previewStream){ previewStream.getTracks().forEach(t=>t.stop()); previewEl.srcObject=null; previewStream=null; document.getElementById('enableCam').textContent='Enable Camera'; return; }
    const s = await navigator.mediaDevices.getUserMedia({ video:{width:{ideal:1280}}, audio:false });
    previewEl.srcObject = s; previewStream = s; await previewEl.play();
    document.getElementById('enableCam').textContent='Disable Camera';
  }catch(e){ alert('Camera permission needed (use HTTPS).'); console.error(e); }
});

// --- basic local stream controls for chat UI (start/stop)
document.getElementById('startLocal').addEventListener('click', async ()=>{
  if(localStream) return;
  try{
    localStream = await navigator.mediaDevices.getUserMedia({ video:{width:{ideal:1280}}, audio:true });
    localEl.srcObject = localStream; await localEl.play();
    // in demo we mirror to remote only after connection; not necessary here
  }catch(e){ alert('Camera & mic required. Use HTTPS.'); console.error(e); }
});
document.getElementById('stopLocal').addEventListener('click', ()=>{
  if(!localStream) return;
  localStream.getTracks().forEach(t=>t.stop()); localEl.srcObject=null; localStream=null;
});

// --- signaling connect
async function connectSocket(){
  if(socket && socket.connected) return socket;
  // load socket.io client dynamically (CDN) if not present
  if(typeof io === 'undefined'){
    await new Promise((res,rej)=>{
      const s=document.createElement('script'); s.src='https://cdn.socket.io/4.7.2/socket.io.min.js'; s.onload=res; s.onerror=rej; document.head.appendChild(s);
    });
  }
  socket = io(SIGNALING_SERVER, { transports:['websocket'], reconnectionAttempts:5 });
  socket.on('connect', ()=>{ console.log('Connected to signaling', socket.id); partnerStatus.textContent = 'Connected to signaling'; socket.emit('set-meta', { region: document.getElementById('region')?.value || 'global' }) });
  socket.on('queued', data => { partnerStatus.textContent = `Queued (#${data.position})`; });
  socket.on('found', async data => { // { peer, initiator }
    if(data.alreadyPaired) { partnerStatus.textContent = 'Already paired'; return; }
    currentPeerId = data.peer; isInitiator = !!data.initiator;
    partnerStatus.textContent = `Matched: ${currentPeerId} (initiator=${isInitiator})`;
    await createPeerConnection();
    if(isInitiator){
      const offer = await pc.createOffer();
      await pc.setLocalDescription(offer);
      socket.emit('signal', { to: currentPeerId, type: 'offer', data: offer });
    }
  });
  socket.on('signal', async ({ from, type, data }) => {
    if(!pc) await createPeerConnection();
    if(type === 'offer'){
      await pc.setRemoteDescription(new RTCSessionDescription(data));
      const answer = await pc.createAnswer();
      await pc.setLocalDescription(answer);
      socket.emit('signal', { to: from, type: 'answer', data: answer });
    } else if(type === 'answer'){
      await pc.setRemoteDescription(new RTCSessionDescription(data));
    } else if(type === 'ice'){
      try{ await pc.addIceCandidate(data); }catch(e){ console.warn('ICE add failed', e); }
    }
  });
  socket.on('peer-left', ({reason}) => {
    partnerStatus.textContent = 'Peer left: ' + (reason||'unknown'); cleanupPeer();
  });
  socket.on('disconnect', ()=>{ partnerStatus.textContent = 'Signaling disconnected'; });
  return socket;
}

// --- create RTCPeerConnection and handlers
async function createPeerConnection(){
  if(pc) return pc;
  pc = new RTCPeerConnection({ iceServers: ICE_SERVERS });
  remoteStream = new MediaStream();
  remoteEl.srcObject = remoteStream;

  pc.ontrack = (ev) => { ev.streams[0].getTracks().forEach(t=>remoteStream.addTrack(t)); };
  pc.onicecandidate = (ev) => { if(ev.candidate && socket && currentPeerId) socket.emit('signal', { to: currentPeerId, type: 'ice', data: ev.candidate }); };
  pc.onconnectionstatechange = () => {
    console.log('pc state', pc.connectionState);
    if(pc.connectionState === 'connected') partnerStatus.textContent = 'In call';
    if(['disconnected','failed','closed'].includes(pc.connectionState)) { partnerStatus.textContent = 'Call ended'; cleanupPeer(); }
  };

  // add local tracks
  if(!localStream){
    try{
      localStream = await navigator.mediaDevices.getUserMedia({video:{width:{ideal:1280}}, audio:true});
      localEl.srcObject = localStream; await localEl.play();
    }catch(e){
      alert('Camera & mic access required.');
      throw e;
    }
  }
  localStream.getTracks().forEach(track => pc.addTrack(track, localStream));
  return pc;
}

// cleanup
function cleanupPeer(){
  try{
    if(pc){ pc.getSenders().forEach(s=>{ try{ s.track && s.track.stop(); }catch{} }); pc.close(); pc=null; }
    if(remoteStream){ remoteStream.getTracks().forEach(t=>t.stop()); remoteStream=null; remoteEl.srcObject=null; }
    currentPeerId = null;
  }catch(e){ console.warn('cleanup', e); }
}

// --- UI actions (Find/Skip/Report/Block)
document.getElementById('btnFind').addEventListener('click', async ()=>{
  await connectSocket();
  const interests = []; const region = document.getElementById('region').value || 'global';
  socket.emit('find', { interests, region, userId: socket.id });
  partnerStatus.textContent = 'Searching...';
});
document.getElementById('btnSkip').addEventListener('click', ()=>{
  if(socket) socket.emit('skip');
  cleanupPeer();
  partnerStatus.textContent = 'Skipped';
});
document.getElementById('btnReport').addEventListener('click', ()=>{
  const reason = prompt('Report reason (spam/abuse/other):');
  if(socket && currentPeerId && reason) socket.emit('report', { reportedId: currentPeerId, reason, reporterId: socket.id });
  alert('Report submitted. Thank you.');
});
document.getElementById('btnBlock').addEventListener('click', ()=>{
  if(!confirm('Block this user?')) return;
  if(socket && currentPeerId){ socket.emit('block', { blockedId: currentPeerId }); cleanupPeer(); alert('User blocked.'); }
});

// mirror/blur
document.getElementById('mirror').addEventListener('click', ()=>{ [localEl,remoteEl].forEach(v=>v.style.transform = v.style.transform==='scaleX(-1)' ? '' : 'scaleX(-1)'); });
document.getElementById('blur').addEventListener('click', ()=>{ [localEl,remoteEl].forEach(v=> v.style.filter = v.style.filter === 'blur(8px)' ? 'none' : 'blur(8px)'); });

// open chat button scroll
document.getElementById('openChat').addEventListener('click', ()=>{ location.hash='#chat'; routeTo('chat'); });

// unload cleanup
window.addEventListener('beforeunload', ()=>{ if(socket) socket.disconnect(); if(localStream) localStream.getTracks().forEach(t=>t.stop()); cleanupPeer(); });

</script>
</body>
</html>
