<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <link rel="manifest" href="/manifest.json">
  <meta name="theme-color" content="#0b7">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-title" content="DAVENPORT ARCH APP V.1">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <link rel="apple-touch-icon" href="/icon-512.svg">
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>DAVENPORT ARCH APP V.1 — Architectural Planner</title>
  <link rel="stylesheet" href="css/ai-graphics.css">
  <style>
    :root{--toolbar-bg:#28963d;--panel-bg:#b51414;--accent:#0b7;--premium:#9b4dca;}
    html,body{height:100%;margin:0;font-family:Segoe UI,Arial,sans-serif}
    .subscription-panel { padding: 12px 0; }
    .plan-box { border: 1px solid #ddd; padding: 15px; margin: 10px 0; border-radius: 8px; }
    .plan-box.premium { border-color: var(--premium); background: rgba(155,77,202,0.05); }
    .plan-box h4 { margin: 0 0 10px 0; color: #333; }
    .plan-box.premium h4 { color: var(--premium); }
    .plan-box ul { margin: 0; padding-left: 20px; font-size: 13px; }
    .plan-box li { margin: 5px 0; color: #666; }
    .subscribe-btn { background: var(--premium) !important; width: 100%; margin-top: 10px; color: white !important; }
    .pro-badge { background: var(--premium); color: white; padding: 2px 6px; border-radius: 3px; font-size: 11px; margin-left: 6px; }
    
    /* Menu Styles */
    .options-menu { padding: 15px 0; }
    .menu-section { margin-bottom: 20px; }
    .menu-section h4 { margin: 0 0 10px 0; color: #333; }
    .menu-links { display: flex; flex-direction: column; gap: 8px; }
    .menu-link { color: var(--accent); text-decoration: none; padding: 6px 0; }
    .menu-link:hover { text-decoration: underline; }
    .tool-buttons { display: flex; flex-direction: column; gap: 8px; }
    .menu-btn { background: var(--accent); color: white; border: none; padding: 8px; border-radius: 4px; cursor: pointer; }
    .menu-btn:hover { opacity: 0.9; }
    .mobile-badges { display: flex; flex-direction: column; gap: 10px; align-items: flex-start; }
  #app{display:flex;height:100vh;gap:8px;background:linear-gradient(135deg,#4b0082,#8a2be2);padding:8px;box-sizing:border-box}
    .left{width:72px;background:var(--toolbar-bg);color:#fff;border-radius:6px;padding:8px;display:flex;flex-direction:column;gap:8px}
    .left button{background:transparent;color:inherit;border:0;padding:8px;border-radius:4px;cursor:pointer}
    .left button.active{background:#333}
    .main{flex:1;display:flex;flex-direction:column;border-radius:6px;overflow:hidden;background:#fff}
    .topbar{display:flex;align-items:center;gap:8px;padding:8px;background:var(--panel-bg);border-bottom:1px solid #ddd}
    .topbar label{font-size:13px;color:#333}
    .controls{display:flex;gap:8px;align-items:center}
    .canvas-wrap{position:relative;flex:1;background:#ddd}
    canvas{display:block;width:100%;height:100%}
    .sidebar{width:320px;background:#fff;border-left:1px solid #ddd;padding:12px;box-sizing:border-box}
    .tool-row{display:flex;gap:6px;align-items:center}
    input[type=range]{width:140px}
    .hint{font-size:12px;color:#555;margin-top:8px}
    .footer{padding:8px;font-size:12px;color:#666;border-top:1px solid #eee;background:#fafafa}
    .btn{background:var(--accent);color:#042;padding:6px 8px;border-radius:4px;border:0;cursor:pointer}
    .info{font-size:13px;color:#222}
    .small{font-size:12px;color:#666}
    .modal{position:fixed;left:0;right:0;top:0;bottom:0;background:rgba(0,0,0,0.4);display:flex;align-items:center;justify-content:center}
    .card{background:#fff;padding:16px;border-radius:8px;min-width:320px}
  </style>
  <style>
    /* Layout bar styles */
    .layout-bar { display:flex;flex-direction:column;gap:8px;margin-top:10px }
    .layout-thumbs { display:flex;gap:8px;flex-wrap:wrap }
    .layout-thumb { width:90px;height:64px;border:1px solid #e0e0e0;border-radius:6px;overflow:hidden;cursor:pointer;display:flex;align-items:center;justify-content:center;background:#fff }
    .layout-thumb img{width:100%;height:100%;object-fit:cover}
    .layout-actions { display:flex;gap:6px;flex-wrap:wrap }
    .layout-actions button{ padding:6px 8px;border-radius:6px;border:0;cursor:pointer;background:#0b7;color:#fff;font-size:12px }
    .layout-actions button.secondary{ background:#eee;color:#000;border:1px solid #ddd }
    .layout-label{font-size:13px;color:#333;margin-bottom:4px}
    .layout-item{display:flex;flex-direction:column;align-items:flex-start;gap:6px}
  </style>
</head>
<body>
  <div id="app">
    <div class="left" title="tools">
      <button id="tool-select" data-tool="select">🔲</button>
      <button id="tool-pencil" data-tool="pencil">✏️</button>
      <button id="tool-line" data-tool="line">/\</button>
      <button id="tool-rect" data-tool="rect">▭</button>
      <button id="tool-circle" data-tool="circle">◯</button>
      <button id="tool-erase" data-tool="erase">🧽</button>
      <button id="tool-measure" data-tool="measure">📏</button>
      <div style="flex:1"></div>
      <button id="undo">↶</button>
      <button id="redo">↷</button>
    </div>

    <div class="main">
      <div class="topbar">
        <div class="controls">
          <div class="site-logo" style="display:flex;align-items:center;gap:12px;margin-right:24px;padding:8px;background:rgba(255,255,255,0.1);border-radius:8px">
            <img src="assets/logo-davenport.svg" alt="Davenport Arch App Logo" style="height:48px;width:auto;">
            <span style="font-weight:600;color:#fff;font-size:18px;white-space:nowrap;margin-left:8px">DAVENPORT ARCH APP V.1</span>
          </div>
          <label>Color <input id="color" type="color" value="#000000"></label>
          <label>Width <input id="width" type="range" min="1" max="40" value="2"></label>
          <label style="margin-left:8px">Zoom <button id="zoom-out" title="Zoom out">−</button> <span id="zoom-display">100%</span> <button id="zoom-in" title="Zoom in">+</button></label>
          <button id="fit" class="btn" style="background:#eee;color:#000;margin-left:6px">Fit</button>
          <label style="margin-left:8px">Snap tol: <input id="snap-tol" type="number" value="6" style="width:60px"> px</label>
          <label>Grid <input id="grid-toggle" type="checkbox" checked></label>
          <label>Snap <input id="snap-toggle" type="checkbox"></label>
          <label>Grid size <input id="grid-size" type="number" value="20" min="4" style="width:70px"></label>
          <label>Room <input id="room-toggle" type="checkbox"></label>
          <div style="display:flex;gap:6px;margin-left:8px">
            <button id="clear-canvas" class="btn" style="background:#fff;color:#c00;border:1px solid #ddd">Clear</button>
            <button id="delete-selected" class="btn" style="background:#fff;color:#c00;border:1px solid #ddd">Delete</button>
            <button id="dup-selected" class="btn" style="background:#fff;color:#000;border:1px solid #ddd">Duplicate</button>
          </div>
        </div>
        <div style="flex:1"></div>
        <div class="controls">
          <input id="bgfile" type="file" accept="image/*" style="display:none">
          <button id="import-bg" class="btn">Import Background</button>
          <button id="export" class="btn">Export PNG</button>
          <button id="save" class="btn" style="background:#0a74d1;color:#fff">Save</button>
          <input id="loadfile" type="file" accept="application/json" style="display:none">
          <button id="load" class="btn">Load</button>
          <label style="margin-left:8px">Online <input id="online-toggle" type="checkbox"></label>
          <button id="signin" class="btn" style="background:#fff;color:#0b7;border:1px solid #0b7">Sign in</button>
          <div id="userDisplay" class="small" style="margin-left:6px"></div>
        </div>
      </div>

      <div class="canvas-wrap">
        <canvas id="board"></canvas>
      </div>

      <div class="footer">
        <span id="status">Ready</span>
        <a id="terms-link" style="margin-left:12px;font-size:12px;color:#0b7;cursor:pointer">Terms &amp; Conditions</a>
      </div>
    </div>

    <div class="sidebar">
      <h3>Subscription Status</h3>
      <div id="subscription-panel" class="subscription-panel">
        <div id="free-features" class="plan-box">
          <h4>Free Plan</h4>
          <ul>
            <li>Basic drawing tools</li>
            <li>Limited exports</li>
            <li>Basic shapes</li>
          </ul>
        </div>
        <div id="pro-features" class="plan-box premium">
          <h4>Pro Plan - $9.99/month</h4>
          <ul>
            <li>✨ Advanced tools</li>
            <li>✨ Unlimited exports</li>
            <li>✨ AI Features</li>
            <li>✨ Cloud storage</li>
            <li>✨ Priority support</li>
          </ul>
          <button id="subscribe-btn" class="btn subscribe-btn">Upgrade to Pro</button>
        </div>
      </div>

      <h3>Project / Scale</h3>
      <div class="tool-row">
        <label>Scale: <input id="scale" type="number" value="100" min="1" style="width:80px"> px/unit</label>
        <select id="unit"><option value="m">m</option><option value="ft">ft</option></select>
      </div>
      <p class="hint">Use the measure tool to click start and end to see distances. Change scale to map pixels to real units.</p>

      <h3>Selection</h3>
      <div class="info">
        <div>Selected: <span id="sel-index">—</span></div>
        <div>Width: <span id="sel-width">—</span></div>
        <div>Height: <span id="sel-height">—</span></div>
        <div>Area: <span id="sel-area">—</span></div>
      </div>

      <h3>Total Areas</h3>
      <div class="info">
        <div>Total room area: <strong id="total-area">0</strong> <span id="total-unit">m²</span></div>
      </div>

      <h3>Quick tips</h3>
      <ul>
        <li>Switch tools from the left toolbar.</li>
        <li>Mark "Room" to have the shape counted towards total area.</li>
        <li>Import a background plan image and draw over it.</li>
      </ul>

      <h3>Calculations & Estimates</h3>
      <div class="tool-row">
        <label>Room height (units): <input id="room-height" type="number" value="2.8" step="0.1" style="width:80px"></label>
      </div>
      <div style="margin-top:8px">
        <label>Material cost per unit (<span id="unit-label">m²</span>): <input id="material-cost" type="number" value="20" step="0.01" style="width:100px"></label>
        <button id="calc-rooms" class="btn" style="margin-left:8px">Compute</button>
      </div>
      <div id="calc-results" class="hint" style="margin-top:8px">No calculations yet.</div>

      <h3>Menu</h3>
      <div class="options-menu">
        <div class="menu-section">
          <h4>🌐 Online Services</h4>
          <div class="menu-links">
            <a href="https://davenport-docs.example.com" target="_blank" class="menu-link">Documentation</a>
            <a href="https://davenport-community.example.com" target="_blank" class="menu-link">Community</a>
            <a href="https://davenport-support.example.com" target="_blank" class="menu-link">Support</a>
            <a href="https://davenport-tutorials.example.com" target="_blank" class="menu-link">Tutorials</a>
          </div>
        </div>

        <div class="menu-section">
          <h4>📱 Mobile Apps</h4>
          <div class="menu-links">
            <a href="https://play.google.com/store/apps/details?id=com.davenport.arch" target="_blank" class="menu-link">
              <img src="https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png" alt="Get it on Google Play" height="40">
            </a>
            <a href="https://apps.apple.com/app/davenport-arch/id123456789" target="_blank" class="menu-link">
              <img src="https://developer.apple.com/app-store/marketing/guidelines/images/badge-download-on-the-app-store.svg" alt="Download on the App Store" height="40">
            </a>
          </div>
        </div>

        <div class="menu-section">
          <h4>🎨 Design Tools</h4>
          <div class="tool-buttons">
            <button id="templates-btn" class="menu-btn">Templates</button>
            <button id="export-options-btn" class="menu-btn">Export Options</button>
            <button id="share-btn" class="menu-btn">Share Project</button>
          </div>
        </div>
    
        <div class="menu-section">
          <h4>🤖 Copilot Assistance</h4>
          <div class="menu-links">
            <div style="display:flex;gap:8px;flex-direction:column">
              <button id="copilot-open" class="menu-btn">Open Copilot (web)</button>
              <button id="copilot-demo" class="menu-btn" style="background:#222">Ask Copilot (demo)</button>
            </div>
            <div class="small hint">Copilot web opens GitHub Copilot page. The demo gives quick tips generated locally.</div>
          </div>
        </div>
      </div>

      <h3>AI Graphics Generation</h3>
      <div id="ai-graphics-panel"></div>

      <!-- AI Layouts / Image Layout Bar -->
      <h3 style="margin-top:12px">AI Layouts</h3>
      <div id="layout-bar" class="layout-bar">
        <div class="layout-label">Generated house layouts (click thumbnail to preview)</div>
        <div id="layout-thumbs" class="layout-thumbs">
          <!-- thumbnails rendered by JS -->
        </div>
        <div class="layout-actions" style="margin-top:4px">
          <button id="refresh-layouts" class="secondary" style="background:#fff;color:#000;border:1px solid #ddd">Refresh</button>
          <button id="open-layouts-folder" class="secondary">More Layouts</button>
        </div>
      </div>

      <h3 style="margin-top:12px">Profiles</h3>
      <div class="hint" style="display:flex;flex-direction:column;gap:8px">
        <div style="display:flex;gap:8px;align-items:center">
          <label style="flex:1">Profile name: <input id="profile-name" type="text" placeholder="My default layout" style="width:100%"></label>
          <button id="load-sample-profile" class="btn" title="Load sample house plan">Load Sample House Plan</button>
        </div>
        <div style="display:flex;gap:8px">
          <button id="save-profile" class="btn">Save Profile</button>
          <button id="export-profiles" class="btn" style="background:#9d2b2b;color:#000">Export All</button>
          <input id="profile-file" type="file" accept="application/json" style="display:none">
          <button id="import-profiles" class="btn" style="background:#1793bc;color:#000">Import</button>
        </div>
        <div id="profiles-list" style="display:flex;flex-direction:column;gap:8px;max-height:180px;overflow:auto"></div>
      </div>
    </div>
  </div>

  <!-- Sign-in modal (simple demo/local) -->
  <div id="signin-modal" class="modal" style="display:none">
    <div class="card">
      <h3>Sign in</h3>
      <div style="display:flex;flex-direction:column;gap:8px">
        <input id="username" placeholder="username">
        <input id="password" type="password" placeholder="password">
        <label style="font-size:13px"><input id="accept-terms" type="checkbox"> I accept the <a id="terms-link-modal" style="color:#0b7;cursor:pointer">Terms &amp; Conditions</a></label>
        <div style="display:flex;gap:8px;justify-content:flex-end">
          <button id="modal-close" class="btn" style="background:#eee;color:#0bc424">Cancel</button>
          <button id="modal-signin" class="btn">Sign in</button>
        </div>
      </div>
      <p class="small">This demo stores a local session. To enable server save/load, run the provided server and toggle Online.</p>
    </div>
  </div>

  <!-- Terms & Conditions modal -->
  <div id="terms-modal" class="modal" style="display:none">
    <div class="card">
      <h3>Terms &amp; Conditions</h3>
      <div style="max-height:60vh;overflow:auto;font-size:13px;line-height:1.4;color:#e717c1">
        <p>Welcome to DAVENPORT. This deals with architechture planning ,house plan layouts. Please read them carefully.</p>
        <h4>1. Use of the Internet and Third-Party Services</h4>
        <p>The app may use the internet to access cloud services, APIs, or to save and load projects. You are responsible for any data charges from your internet provider. The app may connect to third-party services which have their own terms and privacy practices.</p>
        <h4>2. Data and Privacy</h4>
        <p>Your drawings and project data are stored locally in your browser by default (downloadable JSON). If you choose to use the Online mode, you consent to upload project data to the server you configure. Do not upload sensitive personal information. Review any server operator's privacy policy before using Online features.</p>
        <h4>3. User Content</h4>
        <p>You retain ownership of the content you create. By using Online save features you grant the server operator a license to store your content for the purpose of providing the service. You must not upload content that violates laws, copyrights, or the rights of others.</p>
        <h4>4. Liability and No Warranty</h4>
        <p>The app is provided "as is" without warranties of any kind. The authors are not liable for any direct, indirect, incidental, or consequential damages resulting from use of the app, including loss of data or work.</p>
        <h4>5. Account and Access</h4>
        <p>Sign-in in this demo creates a local session token; it is not a secure authentication mechanism. For production online use, a proper backend and secure authentication are required.</p>
        <h4>6. Termination</h4>
        <p>We reserve the right to terminate access to online services if you violate these terms or applicable laws.</p>
        <h4>7. Governing Law</h4>
        <p>These terms are governed by the laws of the jurisdiction where the service operator is established.</p>
        <h4>8. Contact</h4>
        <p>For questions about these terms, contact the app operator or the person who provided you with the app.</p>
      </div>
      <div style="display:flex;justify-content:flex-end;gap:8px;margin-top:12px">
        <button id="terms-close" class="btn" style="background:#eee;color:#000">Close</button>
      </div>
    </div>

  <!-- Copilot demo modal -->
  <div id="copilot-modal" class="modal" style="display:none">
    <div class="card">
      <h3>Copilot (Demo)</h3>
      <div style="display:flex;flex-direction:column;gap:8px">
        <textarea id="copilot-prompt" placeholder="Ask Copilot for drawing tips, e.g. 'best grid size for 1:100'" style="width:480px;height:80px"></textarea>
        <div style="display:flex;gap:8px;justify-content:flex-end">
          <button id="copilot-close" class="btn" style="background:#eee;color:#000">Close</button>
          <button id="copilot-ask" class="btn">Ask Copilot</button>
        </div>
        <div id="copilot-response" style="margin-top:8px;font-size:13px;color:#222;max-height:240px;overflow:auto"></div>
      </div>
    </div>
  </div>
  </div>

  <!-- Load dependencies -->
  <script src="https://js.stripe.com/v3/"></script>
  <script src="js/ai-graphics-controller.js"></script>
  <script src="js/ai-graphics-ui.js"></script>
  <script>
    // Subscription handling
    const stripe = Stripe('your_publishable_key'); // Replace with your Stripe publishable key
    
    // Subscription state
    let subscriptionStatus = {
      isPro: false,
      expiresAt: null
    };

    // Handle subscription button
    document.getElementById('subscribe-btn').addEventListener('click', async () => {
      if (!state.user) {
        updateStatus('Please sign in first');
        document.getElementById('signin-modal').style.display = 'flex';
        return;
      }

      try {
        // Call your backend to create a Stripe checkout session
        const response = await fetch('/api/create-subscription', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${state.token}`
          }
        });

        const session = await response.json();
        
        // Redirect to Stripe checkout
        const result = await stripe.redirectToCheckout({
          sessionId: session.id
        });

        if (result.error) {
          updateStatus('Payment failed: ' + result.error.message);
        }
      } catch (error) {
        updateStatus('Error starting subscription');
        console.error('Subscription error:', error);
      }
    });

    // Check subscription status
    async function checkSubscription() {
      if (!state.token) return;
      
      try {
        const response = await fetch('/api/check-subscription', {
          headers: {
            'Authorization': `Bearer ${state.token}`
          }
        });
        
        const data = await response.json();
        subscriptionStatus = data;
        
        // Update UI based on subscription status
        document.getElementById('free-features').style.display = data.isPro ? 'none' : 'block';
        document.getElementById('pro-features').style.display = data.isPro ? 'block' : 'none';
        
        if (data.isPro) {
          // Add PRO badge to username
          const badge = document.createElement('span');
          badge.className = 'pro-badge';
          badge.textContent = 'PRO';
          if (!ui.userDisplay.querySelector('.pro-badge')) {
            ui.userDisplay.appendChild(badge);
          }
        }
      } catch (error) {
        console.error('Error checking subscription:', error);
      }
    }

    // Canvas & app state
    const canvas = document.getElementById('board');
    const ctx = canvas.getContext('2d');
    let DPR = window.devicePixelRatio || 1;

    let bgImage = null; // Image object for background

    function resize() {
      const rect = canvas.getBoundingClientRect();
      canvas.width = Math.floor(rect.width * DPR);
      canvas.height = Math.floor(rect.height * DPR);
      ctx.setTransform(DPR,0,0,DPR,0,0);
      redraw();
    }
    window.addEventListener('resize', resize);

    const ui = {
      selIndexEl: document.getElementById('sel-index'),
      selWidthEl: document.getElementById('sel-width'),
      selHeightEl: document.getElementById('sel-height'),
      selAreaEl: document.getElementById('sel-area'),
      totalAreaEl: document.getElementById('total-area'),
      totalUnitEl: document.getElementById('total-unit'),
      userDisplay: document.getElementById('userDisplay')
    };

    let state = {
      tool: 'pencil',
      color: '#000000',
      width: 2,
      gridOn: true,
      snap: false,
      gridSize: 20,
      scalePxPerUnit: 100,
      zoom: 1,
      snapTol: 6,
      unit: 'm',
      shapes: [], // vector shapes
      selectedIndex: -1,
      online: false,
      user: null
    };

    let history = [], future = [];

    function pushHistory(){ history.push(JSON.parse(JSON.stringify(state.shapes))); if(history.length>200) history.shift(); future=[]; }
    function undo(){ if(!history.length) return; future.push(JSON.parse(JSON.stringify(state.shapes))); state.shapes = history.pop(); redraw(); }
    function redo(){ if(!future.length) return; history.push(JSON.parse(JSON.stringify(state.shapes))); state.shapes = future.pop(); redraw(); }

  function snapCoord(c){ if(!state.snap) return c; const g=state.gridSize; return Math.round(c/g)*g; }

  // pointer coordinate conversion respects current zoom
  function toLocal(e){ const r=canvas.getBoundingClientRect(); const x = (e.clientX - r.left) / state.zoom; const y = (e.clientY - r.top) / state.zoom; return {x: snapCoord(x), y: snapCoord(y)}; }

    function drawGrid(){ if(!state.gridOn) return; const w=canvas.width/DPR, h=canvas.height/DPR, g=state.gridSize; ctx.save(); ctx.strokeStyle='#cfcfcf'; ctx.lineWidth=1; for(let x=0;x<=w;x+=g){ ctx.beginPath(); ctx.moveTo(x+0.5,0); ctx.lineTo(x+0.5,h); ctx.stroke(); } for(let y=0;y<=h;y+=g){ ctx.beginPath(); ctx.moveTo(0,y+0.5); ctx.lineTo(w,y+0.5); ctx.stroke(); } ctx.restore(); }

    function computeShapeArea(s){ // returns area in px^2 for room shapes
      if(!s) return 0;
      if(s.tool==='rect'){ const w=Math.abs(s.x2-s.x1), h=Math.abs(s.y2-s.y1); return w*h; }
      if(s.tool==='circle'){ const rx=Math.abs((s.x2-s.x1)/2), ry=Math.abs((s.y2-s.y1)/2); return Math.PI*rx*ry; }
      return 0; // others ignored for area
    }

    function computeTotalArea(){ let totalPx=0; for(const s of state.shapes){ if(s.isRoom) totalPx += computeShapeArea(s); } const unit = state.unit; const scale = state.scalePxPerUnit; const totalUnits = totalPx / (scale*scale); return {px: totalPx, units: totalUnits, unitLabel: unit + (unit==='m'? '²':'²')}; }

    function redraw(){ const w=canvas.width/DPR, h=canvas.height/DPR; ctx.clearRect(0,0,w,h); // background
      // draw background and shapes with zoom applied
      ctx.save();
      // apply zoom in drawing space (after DPR transform)
      ctx.scale(state.zoom, state.zoom);
      if(bgImage){ try{ ctx.drawImage(bgImage,0,0,w/state.zoom,h/state.zoom); }catch(e){} } else { ctx.fillStyle='#fff'; ctx.fillRect(0,0,w/state.zoom,h/state.zoom); }
      drawGrid();
      state.shapes.forEach((s,idx)=>{ ctx.save(); ctx.lineWidth=s.width; ctx.strokeStyle=s.color; ctx.fillStyle=s.color; if(s.tool==='pencil'){ ctx.beginPath(); const pts=s.points; if(pts.length){ ctx.moveTo(pts[0].x,pts[0].y); for(let i=1;i<pts.length;i++) ctx.lineTo(pts[i].x,pts[i].y); ctx.stroke(); } } else if(s.tool==='line' || s.tool==='measure'){ ctx.beginPath(); ctx.moveTo(s.x1,s.y1); ctx.lineTo(s.x2,s.y2); ctx.stroke(); if(s.tool==='measure'){ const dx=s.x2-s.x1, dy=s.y2-s.y1; const px=Math.hypot(dx,dy); const units=(px/state.scalePxPerUnit).toFixed(2)+' '+state.unit; ctx.fillStyle='#000'; ctx.font='12px sans-serif'; ctx.fillText(units,(s.x1+s.x2)/2+6,(s.y1+s.y2)/2-6); } } else if(s.tool==='rect'){ const x=Math.min(s.x1,s.x2), y=Math.min(s.y1,s.y2), wRect=Math.abs(s.x2-s.x1), hRect=Math.abs(s.y2-s.y1); ctx.strokeRect(x,y,wRect,hRect); if(s.isRoom){ ctx.fillStyle='rgba(10,120,80,0.06)'; ctx.fillRect(x,y,wRect,hRect); } } else if(s.tool==='circle'){ const cx=(s.x1+s.x2)/2, cy=(s.y1+s.y2)/2, rx=Math.abs((s.x2-s.x1)/2), ry=Math.abs((s.y2-s.y1)/2); ctx.beginPath(); ctx.ellipse(cx,cy,rx,ry,0,0,Math.PI*2); ctx.stroke(); if(s.isRoom){ ctx.save(); ctx.globalAlpha=0.06; ctx.fillStyle='green'; ctx.fill(); ctx.restore(); } } else if(s.tool==='erase'){ ctx.globalCompositeOperation='destination-out'; ctx.beginPath(); const pts=s.points; ctx.moveTo(pts[0].x,pts[0].y); for(let i=1;i<pts.length;i++) ctx.lineTo(pts[i].x,pts[i].y); ctx.lineWidth=s.width; ctx.stroke(); ctx.globalCompositeOperation='source-over'; } if(idx===state.selectedIndex){ // highlight selection
        ctx.save(); ctx.strokeStyle='#f40'; ctx.lineWidth=s.width+1; if(s.tool==='rect'){ const x=Math.min(s.x1,s.x2), y=Math.min(s.y1,s.y2), wRect=Math.abs(s.x2-s.x1), hRect=Math.abs(s.y2-s.y1); ctx.strokeRect(x-3,y-3,wRect+6,hRect+6); } ctx.restore(); }
        ctx.restore(); });
      ctx.restore();
      // update total area UI
      const total = computeTotalArea(); ui.totalAreaEl.textContent = total.units.toFixed(2); ui.totalUnitEl.textContent = state.unit + '²';
    }

    // UI wiring
    function setTool(t){ state.tool = t; document.querySelectorAll('.left button').forEach(b=>b.classList.toggle('active', b.dataset.tool===t)); updateStatus('Tool: '+t); }
    document.getElementById('tool-pencil').addEventListener('click', ()=>setTool('pencil'));
  document.getElementById('tool-select').addEventListener('click', ()=>setTool('select'));
    document.getElementById('tool-line').addEventListener('click', ()=>setTool('line'));
    document.getElementById('tool-rect').addEventListener('click', ()=>setTool('rect'));
    document.getElementById('tool-circle').addEventListener('click', ()=>setTool('circle'));
    document.getElementById('tool-erase').addEventListener('click', ()=>setTool('erase'));
    document.getElementById('tool-measure').addEventListener('click', ()=>setTool('measure'));

    document.getElementById('color').addEventListener('input', e=>{ state.color = e.target.value; });
    document.getElementById('width').addEventListener('input', e=>{ state.width = Number(e.target.value); });
    document.getElementById('grid-toggle').addEventListener('change', e=>{ state.gridOn = e.target.checked; redraw(); });
    document.getElementById('snap-toggle').addEventListener('change', e=>{ state.snap = e.target.checked; });
    document.getElementById('grid-size').addEventListener('input', e=>{ state.gridSize = Number(e.target.value); redraw(); });
    document.getElementById('scale').addEventListener('input', e=>{ state.scalePxPerUnit = Number(e.target.value); });
    document.getElementById('unit').addEventListener('change', e=>{ state.unit = e.target.value; });

    document.getElementById('undo').addEventListener('click', undo);
    document.getElementById('redo').addEventListener('click', redo);

    // Background import
    document.getElementById('import-bg').addEventListener('click', ()=>document.getElementById('bgfile').click());
    document.getElementById('bgfile').addEventListener('change', e=>{
      const f = e.target.files[0]; if(!f) return; const r = new FileReader(); r.onload = ()=>{ const img = new Image(); img.onload = ()=>{ bgImage = img; redraw(); updateStatus('Background loaded'); }; img.src = r.result; }; r.readAsDataURL(f);
    });

    // Save/Load (local or server)
    document.getElementById('save').addEventListener('click', async ()=>{
      const acceptedAt = localStorage.getItem('dav_terms_accepted_at') || null;
      const payload = {state:{gridSize:state.gridSize,scalePxPerUnit:state.scalePxPerUnit,unit:state.unit,termsAcceptedAt:acceptedAt},shapes:state.shapes};
      if(document.getElementById('online-toggle').checked && state.user && state.token){
        // attempt server save
        try{
          const res = await fetch((window.location.origin||'') + '/api/save',{method:'POST',headers:{'Content-Type':'application/json','Authorization':'Bearer '+state.token},body:JSON.stringify({name:'project',project:payload})});
          if(res.ok){ updateStatus('Saved to server'); return; }
        }catch(e){ updateStatus('Server save failed'); }
      }
      // fallback: download
      const data = JSON.stringify(payload);
      const blob = new Blob([data],{type:'application/json'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = 'davenport_project.json'; a.click(); URL.revokeObjectURL(url); updateStatus('Downloaded JSON');
    });

    document.getElementById('load').addEventListener('click', ()=>document.getElementById('loadfile').click());
    document.getElementById('loadfile').addEventListener('change', e=>{ const f=e.target.files[0]; if(!f) return; const r=new FileReader(); r.onload=()=>{ try{ const parsed=JSON.parse(r.result); state.gridSize = parsed.state?.gridSize||state.gridSize; state.scalePxPerUnit = parsed.state?.scalePxPerUnit||state.scalePxPerUnit; state.unit = parsed.state?.unit||state.unit; state.shapes = parsed.shapes||[]; pushHistory(); redraw(); updateStatus('Loaded file'); }catch(err){ updateStatus('Invalid JSON'); } }; r.readAsText(f); });

    document.getElementById('export').addEventListener('click', ()=>{ const w=canvas.width, h=canvas.height; const tmp=document.createElement('canvas'); tmp.width=w; tmp.height=h; const tctx=tmp.getContext('2d'); tctx.scale(DPR,DPR); if(bgImage) tctx.drawImage(bgImage,0,0,canvas.width/DPR,canvas.height/DPR); else { tctx.fillStyle='#fff'; tctx.fillRect(0,0,canvas.width/DPR,canvas.height/DPR); } tctx.drawImage(canvas,0,0); const url=tmp.toDataURL('image/png'); const a=document.createElement('a'); a.href=url; a.download='davenport_export.png'; a.click(); updateStatus('Exported PNG'); });

  // When saving include terms acceptance timestamp if present
  const originalSaveHandler = document.getElementById('save').onclick;


  // Drawing
  let drawing=false, current=null;

    canvas.addEventListener('pointerdown', ev=>{ canvas.setPointerCapture(ev.pointerId); drawing=true; const p=toLocal(ev); if(state.tool==='pencil' || state.tool==='erase'){ current={tool:state.tool,color:state.color,width:state.width,points:[p],isRoom:document.getElementById('room-toggle').checked}; } else if(['line','rect','circle','measure'].includes(state.tool)){ current={tool:state.tool,color:state.color,width:state.width,x1:p.x,y1:p.y,x2:p.x,y2:p.y,isRoom:document.getElementById('room-toggle').checked}; } updateStatus('Drawing...'); });

    canvas.addEventListener('pointermove', ev=>{ if(!drawing || !current) return; const p=toLocal(ev); if(current.points){ current.points.push(p); } else { current.x2=p.x; current.y2=p.y; } redraw(); // draw current on top
      ctx.save(); ctx.lineWidth=current.width; ctx.strokeStyle=current.color; ctx.fillStyle=current.color; if(current.tool==='pencil'){ ctx.beginPath(); const pts=current.points; ctx.moveTo(pts[0].x,pts[0].y); for(let i=1;i<pts.length;i++) ctx.lineTo(pts[i].x,pts[i].y); ctx.stroke(); } else if(current.tool==='erase'){ ctx.globalCompositeOperation='destination-out'; ctx.beginPath(); const pts=current.points; ctx.moveTo(pts[0].x,pts[0].y); for(let i=1;i<pts.length;i++) ctx.lineTo(pts[i].x,pts[i].y); ctx.lineWidth=current.width; ctx.stroke(); ctx.globalCompositeOperation='source-over'; } else if(current.tool==='line' || current.tool==='measure'){ ctx.beginPath(); ctx.moveTo(current.x1,current.y1); ctx.lineTo(current.x2,current.y2); ctx.stroke(); } else if(current.tool==='rect'){ const x=Math.min(current.x1,current.x2), y=Math.min(current.y1,current.y2), wRect=Math.abs(current.x2-current.x1), hRect=Math.abs(current.y2-current.y1); ctx.strokeRect(x,y,wRect,hRect); } else if(current.tool==='circle'){ const cx=(current.x1+current.x2)/2, cy=(current.y1+current.y2)/2, rx=Math.abs((current.x2-current.x1)/2), ry=Math.abs((current.y2-current.y1)/2); ctx.beginPath(); ctx.ellipse(cx,cy,rx,ry,0,0,Math.PI*2); ctx.stroke(); } ctx.restore(); });

    canvas.addEventListener('pointerup', ev=>{ if(!drawing || !current) return; drawing=false; canvas.releasePointerCapture(ev.pointerId); state.shapes.push(current); state.selectedIndex = state.shapes.length -1; pushHistory(); current=null; redraw(); updateSelectionUI(); updateStatus('Added shape'); });

    function updateSelectionUI(){ const i = state.selectedIndex; if(i<0 || i>=state.shapes.length){ ui.selIndexEl.textContent='—'; ui.selWidthEl.textContent='—'; ui.selHeightEl.textContent='—'; ui.selAreaEl.textContent='—'; return; } const s=state.shapes[i]; ui.selIndexEl.textContent = i; if(s.tool==='rect'){ const w=Math.abs(s.x2-s.x1), h=Math.abs(s.y2-s.y1); ui.selWidthEl.textContent = w.toFixed(1)+' px ('+(w/state.scalePxPerUnit).toFixed(2)+' '+state.unit+')'; ui.selHeightEl.textContent = h.toFixed(1)+' px ('+(h/state.scalePxPerUnit).toFixed(2)+' '+state.unit+')'; ui.selAreaEl.textContent = (computeShapeArea(s)/ (state.scalePxPerUnit*state.scalePxPerUnit)).toFixed(2)+' '+state.unit+'²'; } else if(s.tool==='circle'){ const rx=Math.abs((s.x2-s.x1)/2), ry=Math.abs((s.y2-s.y1)/2); ui.selWidthEl.textContent=(2*rx).toFixed(1)+' px'; ui.selHeightEl.textContent=(2*ry).toFixed(1)+' px'; ui.selAreaEl.textContent=(computeShapeArea(s)/(state.scalePxPerUnit*state.scalePxPerUnit)).toFixed(2)+' '+state.unit+'²'; } else { ui.selWidthEl.textContent='—'; ui.selHeightEl.textContent='—'; ui.selAreaEl.textContent='—'; } const total=computeTotalArea(); ui.totalAreaEl.textContent=total.units.toFixed(2); ui.totalUnitEl.textContent=state.unit+'²'; }

    function updateStatus(t){ document.getElementById('status').textContent = t; }

    // Keyboard
    window.addEventListener('keydown', e=>{ if((e.ctrlKey||e.metaKey)&&e.key.toLowerCase()==='z'){ undo(); e.preventDefault(); } if((e.ctrlKey||e.metaKey)&&e.key.toLowerCase()==='y'){ redo(); e.preventDefault(); } });

    // Simple sign-in (demo/local storage) and online toggle handling
    const modal = document.getElementById('signin-modal');
    document.getElementById('signin').addEventListener('click', ()=>{ modal.style.display='flex'; });
    document.getElementById('modal-close').addEventListener('click', ()=>{ modal.style.display='none'; });
    document.getElementById('terms-link').addEventListener('click', ()=>{ document.getElementById('terms-modal').style.display='flex'; });
    document.getElementById('terms-link-modal').addEventListener('click', ()=>{ document.getElementById('terms-modal').style.display='flex'; });
    document.getElementById('terms-close').addEventListener('click', ()=>{ document.getElementById('terms-modal').style.display='none'; });

    // Copilot buttons: open web Copilot and demo assistant
    document.getElementById('copilot-open').addEventListener('click', ()=>{
      // open GitHub Copilot landing page in a new tab
      window.open('https://github.com/features/copilot', '_blank');
      updateStatus('Opened GitHub Copilot');
    });

    // Demo assistant (local canned responses)
    const copilotModal = document.getElementById('copilot-modal');
    document.getElementById('copilot-demo').addEventListener('click', ()=>{ copilotModal.style.display='flex'; document.getElementById('copilot-response').innerHTML=''; document.getElementById('copilot-prompt').value=''; });
    document.getElementById('copilot-close').addEventListener('click', ()=>{ copilotModal.style.display='none'; });

    const copilotTips = [
      'Try grid size 20 px for comfortable drawing at 1:100 scale; increase to 40 px for coarse layouts.',
      'Use the measure tool and set scale to 100 px/unit to map pixels to real-world meters.',
      'Group rooms by drawing rectangles and enabling "Room" to include them in area totals.',
      'For export quality, set zoom to 100% and export PNG; for vector use Export SVG.',
      'Enable snapping when drawing tightly-aligned walls; disable for freeform sketches.'
    ];

    function generateCopilotResponse(prompt){
      // local demo: match keywords or return a random tip
      const q = (prompt||'').toLowerCase();
      if(q.includes('grid')) return 'Recommendation: use 20 px grid for moderate detail, 10 px for fine detail.';
      if(q.includes('scale')) return 'Set scalePxPerUnit to 100 px/unit to represent meters; adjust for your plan.';
      if(q.includes('export')) return 'Export PNG for raster output; use Export SVG for scalable vector output.';
      if(q.includes('snap')) return 'Enable Snap for aligned walls, and set Snap tolerance to 4-8 px.';
      // fallback: random tip
      return copilotTips[Math.floor(Math.random()*copilotTips.length)];
    }

    document.getElementById('copilot-ask').addEventListener('click', ()=>{
      const prompt = document.getElementById('copilot-prompt').value.trim();
      const respEl = document.getElementById('copilot-response');
      if(!prompt){ respEl.innerHTML = '<div class="small">Type a question or click Ask to get a random tip.</div>'; return; }
      // generate local response
      const r = generateCopilotResponse(prompt);
      respEl.innerHTML = '<div style="white-space:pre-wrap">' + r + '</div>';
    });

    // Enforce terms acceptance on sign-in and store timestamp
    document.getElementById('modal-signin').addEventListener('click', async ()=>{
      const u = document.getElementById('username').value.trim(); const p = document.getElementById('password').value;
      const accepted = document.getElementById('accept-terms').checked;
      if(!u) return updateStatus('Enter username');
      if(!accepted) return updateStatus('You must accept the Terms & Conditions to sign in');
      const acceptedAt = new Date().toISOString();
      // local demo: accept any username/password and store token in localStorage
      state.user = u; state.token = 'local-'+Date.now(); localStorage.setItem('dav_user',u); localStorage.setItem('dav_token',state.token); localStorage.setItem('dav_terms_accepted_at', acceptedAt); ui.userDisplay.textContent = u; modal.style.display='none'; updateStatus('Signed in (local)');
      // Check subscription status after sign in
      await checkSubscription();
      // If online is requested, attempt server login
      if(document.getElementById('online-toggle').checked){ try{ const res = await fetch((window.location.origin||'') + '/api/login',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({username:u,password:p})}); if(res.ok){ const j=await res.json(); state.token=j.token; state.user=u; ui.userDisplay.textContent=u; updateStatus('Signed in to server'); } else { updateStatus('Server sign-in failed (continuing local)'); } }catch(e){ updateStatus('Server sign-in failed'); } }
    });

    // online toggle
    document.getElementById('online-toggle').addEventListener('change', e=>{ state.online = e.target.checked; updateStatus(state.online? 'Online mode' : 'Offline mode'); });

    // Profiles: save/load/apply templates stored in localStorage under 'dav_profiles'
    function getProfiles(){ try{ return JSON.parse(localStorage.getItem('dav_profiles')||'[]')||[] }catch(e){ return []; } }
    function setProfiles(arr){ localStorage.setItem('dav_profiles', JSON.stringify(arr)); }

    function generateThumbnailDataURL(){ try{
      const srcW = canvas.width / DPR, srcH = canvas.height / DPR;
      const thumbW = 320; const thumbH = Math.round(thumbW * srcH / srcW);
      const tmp = document.createElement('canvas'); tmp.width = thumbW; tmp.height = thumbH; const tctx = tmp.getContext('2d');
      // white bg
      tctx.fillStyle = '#fff'; tctx.fillRect(0,0,thumbW,thumbH);
      // draw the current canvas scaled
      tctx.drawImage(canvas, 0, 0, thumbW, thumbH);
      return tmp.toDataURL('image/png');
    }catch(e){ return null; } }

    function renderProfiles(){ const list = document.getElementById('profiles-list'); list.innerHTML=''; const profiles = getProfiles(); if(!profiles.length){ list.innerHTML = '<div class="small">No profiles saved.</div>'; return; } profiles.forEach(p=>{
      const item = document.createElement('div'); item.style.display='flex'; item.style.gap='8px'; item.style.alignItems='center'; item.style.border='1px solid #eee'; item.style.padding='6px'; item.style.borderRadius='6px';
      const img = document.createElement('img'); img.src = p.thumbnail || ''; img.style.width='80px'; img.style.height='60px'; img.style.objectFit='cover'; img.style.border='1px solid #ddd';
      const meta = document.createElement('div'); meta.style.flex='1'; meta.innerHTML = `<div style="font-weight:600">${p.name}</div><div class="small">${new Date(p.createdAt).toLocaleString()}</div>`;
      const actions = document.createElement('div'); actions.style.display='flex'; actions.style.flexDirection='column'; actions.style.gap='6px';
      const applyBtn = document.createElement('button'); applyBtn.textContent='Apply'; applyBtn.className='btn'; applyBtn.onclick = ()=>applyProfile(p.id);
      const exportBtn = document.createElement('button'); exportBtn.textContent='Export'; exportBtn.className='btn'; exportBtn.style.background='#eee'; exportBtn.style.color='#000'; exportBtn.onclick = ()=>exportProfile(p.id);
      const delBtn = document.createElement('button'); delBtn.textContent='Delete'; delBtn.className='btn'; delBtn.style.background='#fff'; delBtn.style.color='#c00'; delBtn.onclick = ()=>{ if(confirm('Delete profile "'+p.name+'"?')){ const remaining = getProfiles().filter(x=>x.id!==p.id); setProfiles(remaining); renderProfiles(); updateStatus('Profile deleted'); } };
      actions.appendChild(applyBtn); actions.appendChild(exportBtn); actions.appendChild(delBtn);
      item.appendChild(img); item.appendChild(meta); item.appendChild(actions); list.appendChild(item);
    }); }

    async function saveProfile(){ const nameEl = document.getElementById('profile-name'); let name = (nameEl.value||'').trim(); if(!name) name = 'Profile '+new Date().toLocaleString(); const id = 'p-'+Date.now(); const createdAt = new Date().toISOString(); const thumbnail = generateThumbnailDataURL(); const profile = { id, name, createdAt, thumbnail, metadata:{ gridSize: state.gridSize, scalePxPerUnit: state.scalePxPerUnit, unit: state.unit }, shapes: JSON.parse(JSON.stringify(state.shapes)), termsAcceptedAt: localStorage.getItem('dav_terms_accepted_at') || null };
      const profiles = getProfiles(); profiles.unshift(profile); setProfiles(profiles); renderProfiles(); updateStatus('Profile saved'); }

    function applyProfile(id){ const profiles = getProfiles(); const p = profiles.find(x=>x.id===id); if(!p) return updateStatus('Profile not found'); if(state.shapes.length && !confirm('Applying a profile will replace current shapes. Continue?')) return; state.gridSize = p.metadata?.gridSize || state.gridSize; state.scalePxPerUnit = p.metadata?.scalePxPerUnit || state.scalePxPerUnit; state.unit = p.metadata?.unit || state.unit; state.shapes = JSON.parse(JSON.stringify(p.shapes || [])); pushHistory(); redraw(); updateSelectionUI(); updateStatus('Profile applied: '+p.name); }

    function exportProfile(id){ const profiles = getProfiles(); const p = profiles.find(x=>x.id===id); if(!p) return updateStatus('Profile not found'); const blob = new Blob([JSON.stringify(p, null, 2)],{type:'application/json'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = (p.name||'profile') + '.davenport.profile.json'; a.click(); URL.revokeObjectURL(url); updateStatus('Profile exported'); }

    function exportAllProfiles(){ const profiles = getProfiles(); const blob = new Blob([JSON.stringify(profiles, null, 2)],{type:'application/json'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = 'davenport_profiles.json'; a.click(); URL.revokeObjectURL(url); updateStatus('Exported all profiles'); }

    function importProfilesFromFile(file){ const r = new FileReader(); r.onload = ()=>{ try{ const parsed = JSON.parse(r.result); const existing = getProfiles(); if(Array.isArray(parsed)){ // merge
          const merged = parsed.concat(existing); setProfiles(merged); renderProfiles(); updateStatus('Imported '+parsed.length+' profiles'); } else if(parsed && parsed.id){ existing.unshift(parsed); setProfiles(existing); renderProfiles(); updateStatus('Imported profile '+parsed.name); } else { updateStatus('Invalid profile file'); } }catch(e){ updateStatus('Invalid JSON in import'); } }; r.readAsText(file); }

    // Wire profile UI buttons
    document.getElementById('save-profile').addEventListener('click', saveProfile);
    document.getElementById('export-profiles').addEventListener('click', exportAllProfiles);
    document.getElementById('import-profiles').addEventListener('click', ()=>document.getElementById('profile-file').click());
    document.getElementById('profile-file').addEventListener('change', e=>{ const f = e.target.files[0]; if(!f) return; importProfilesFromFile(f); e.target.value=''; });

    // --- Sample profile (house plan) support ---
    const sampleThumbnailSVG = encodeURIComponent(`
      <svg xmlns='http://www.w3.org/2000/svg' width='320' height='180' viewBox='0 0 320 180'>
        <rect width='100%' height='100%' fill='#fff' />
        <rect x='20' y='30' width='120' height='80' fill='none' stroke='#0b7' stroke-width='3'/>
        <rect x='160' y='30' width='120' height='80' fill='none' stroke='#0b7' stroke-width='3'/>
        <rect x='20' y='120' width='260' height='40' fill='none' stroke='#0b7' stroke-width='3'/>
        <line x1='80' y1='30' x2='80' y2='110' stroke='#0b7' stroke-width='2' stroke-dasharray='4 2'/>
        <text x='30' y='20' fill='#222' font-size='12'>Sample House Plan</text>
      </svg>
    `);
    const sampleThumbnailDataUrl = 'data:image/svg+xml;utf8,' + sampleThumbnailSVG;

    const sampleProfile = {
      id: 'sample-house',
      name: 'Sample House Plan',
      createdAt: new Date().toISOString(),
      thumbnail: sampleThumbnailDataUrl,
      metadata: { gridSize: state.gridSize, scalePxPerUnit: state.scalePxPerUnit, unit: state.unit },
      shapes: [
        // simple room rectangles to illustrate a house layout
        { tool: 'rect', x1: 20, y1: 30, x2: 140, y2: 110, color: '#000000', width: 2, isRoom: true },
        { tool: 'rect', x1: 160, y1: 30, x2: 280, y2: 110, color: '#000000', width: 2, isRoom: true },
        { tool: 'rect', x1: 20, y1: 120, x2: 280, y2: 160, color: '#000000', width: 2, isRoom: true }
      ],
      termsAcceptedAt: localStorage.getItem('dav_terms_accepted_at') || null
    };

    function loadSampleProfile(){
      if(state.shapes.length && !confirm('Loading the sample profile will replace your current drawing. Continue?')) return;
      // add to stored profiles (if not present) so it shows in Profiles list
      const existing = getProfiles();
      if(!existing.find(p=>p.id===sampleProfile.id)){
        existing.unshift(sampleProfile);
        setProfiles(existing);
      }
      // apply the profile
      const p = JSON.parse(JSON.stringify(sampleProfile));
      state.gridSize = p.metadata?.gridSize || state.gridSize;
      state.scalePxPerUnit = p.metadata?.scalePxPerUnit || state.scalePxPerUnit;
      state.unit = p.metadata?.unit || state.unit;
      state.shapes = JSON.parse(JSON.stringify(p.shapes || []));
      pushHistory();
      renderProfiles();
      redraw();
      updateSelectionUI();
      updateStatus('Sample house plan loaded');
    }

    // wire sample load button
    const sampleBtn = document.getElementById('load-sample-profile');
    if(sampleBtn) sampleBtn.addEventListener('click', loadSampleProfile);

    // --- Extra controls wiring: zoom, select, delete, duplicate, ordering, lock, clear, export SVG ---
    function updateZoomDisplay(){ document.getElementById('zoom-display').textContent = Math.round(state.zoom*100) + '%'; }
    function zoomIn(){ state.zoom = Math.min(4, state.zoom * 1.2); redraw(); updateZoomDisplay(); }
    function zoomOut(){ state.zoom = Math.max(0.25, state.zoom / 1.2); redraw(); updateZoomDisplay(); }
    function fitToContent(){ // compute bounding box of shapes and fit to canvas
      if(!state.shapes.length){ state.zoom = 1; redraw(); updateZoomDisplay(); return; }
      let minX=Infinity,minY=Infinity,maxX=-Infinity,maxY=-Infinity;
      for(const s of state.shapes){ if(s.tool==='pencil'){ s.points.forEach(p=>{ minX=Math.min(minX,p.x); minY=Math.min(minY,p.y); maxX=Math.max(maxX,p.x); maxY=Math.max(maxY,p.y); }); } else { const x1=Math.min(s.x1||0,s.x2||0), y1=Math.min(s.y1||0,s.y2||0), x2=Math.max(s.x1||0,s.x2||0), y2=Math.max(s.y1||0,s.y2||0); minX=Math.min(minX,x1); minY=Math.min(minY,y1); maxX=Math.max(maxX,x2); maxY=Math.max(maxY,y2); } }
      const contentW = maxX - minX || 1; const contentH = maxY - minY || 1;
      const viewW = canvas.width / DPR; const viewH = canvas.height / DPR;
      const zx = viewW / contentW; const zy = viewH / contentH; state.zoom = Math.min(zx, zy) * 0.9; if(state.zoom>4) state.zoom=4; if(state.zoom<0.25) state.zoom=0.25; redraw(); updateZoomDisplay();
    }

    document.getElementById('zoom-in').addEventListener('click', zoomIn);
    document.getElementById('zoom-out').addEventListener('click', zoomOut);
    document.getElementById('fit').addEventListener('click', fitToContent);
    document.getElementById('snap-tol').addEventListener('input', e=>{ state.snapTol = Number(e.target.value) || 6; });

    // Simple hit-test for selection
    function pointToSegDist(px,py,x1,y1,x2,y2){ const A=px-x1, B=py-y1, C=x2-x1, D=y2-y1; const dot = A*C + B*D; const len2 = C*C + D*D; let t = len2 ? dot / len2 : -1; t = Math.max(0, Math.min(1, t)); const projx = x1 + t*C, projy = y1 + t*D; const dx = px - projx, dy = py - projy; return Math.hypot(dx,dy); }
    function hitTest(x,y){ // returns index or -1
      for(let i=state.shapes.length-1;i>=0;i--){ const s=state.shapes[i]; if(s.locked) continue; if(s.tool==='rect'){ const x0=Math.min(s.x1,s.x2), y0=Math.min(s.y1,s.y2), wRect=Math.abs(s.x2-s.x1), hRect=Math.abs(s.y2-s.y1); if(x>=x0 && x<=x0+wRect && y>=y0 && y<=y0+hRect) return i; } else if(s.tool==='circle'){ const cx=(s.x1+s.x2)/2, cy=(s.y1+s.y2)/2, rx=Math.abs((s.x2-s.x1)/2), ry=Math.abs((s.y2-s.y1)/2); if(rx>0 && ry>0){ const nx=(x-cx)/rx, ny=(y-cy)/ry; if(nx*nx+ny*ny<=1) return i; } } else if(s.tool==='line' || s.tool==='measure'){ const d = pointToSegDist(x,y,s.x1,s.y1,s.x2,s.y2); if(d <= (state.snapTol||6)) return i; } else if(s.tool==='pencil' || s.tool==='erase'){ const pts = s.points||[]; for(let k=0;k<pts.length-1;k++){ const p1=pts[k], p2=pts[k+1]; const d = pointToSegDist(x,y,p1.x,p1.y,p2.x,p2.y); if(d <= (state.snapTol||6)) return i; } }
      }
      return -1;
    }

    // selection via click
    canvas.addEventListener('click', ev=>{
      if(drawing) return; const p = toLocal(ev); const idx = hitTest(p.x,p.y); state.selectedIndex = idx; updateSelectionUI(); redraw(); if(idx>=0) updateStatus('Selected shape '+idx); else updateStatus('No selection');
    });

    function deleteSelected(){ const i = state.selectedIndex; if(i<0) return updateStatus('No selection'); const s = state.shapes[i]; if(s.locked) return updateStatus('Shape is locked'); if(!confirm('Delete selected shape?')) return; state.shapes.splice(i,1); state.selectedIndex=-1; pushHistory(); redraw(); updateSelectionUI(); updateStatus('Shape deleted'); }
    function duplicateSelected(){ const i = state.selectedIndex; if(i<0) return updateStatus('No selection'); const s = JSON.parse(JSON.stringify(state.shapes[i])); s.id = 'dup-'+Date.now(); state.shapes.push(s); pushHistory(); redraw(); updateStatus('Shape duplicated'); }
    function bringForward(){ const i=state.selectedIndex; if(i<0 || i>=state.shapes.length-1) return; const s=state.shapes.splice(i,1)[0]; state.shapes.splice(i+1,0,s); state.selectedIndex = i+1; pushHistory(); redraw(); }
    function sendBackward(){ const i=state.selectedIndex; if(i<=0) return; const s=state.shapes.splice(i,1)[0]; state.shapes.splice(i-1,0,s); state.selectedIndex = i-1; pushHistory(); redraw(); }
    function toggleLockSelected(){ const i=state.selectedIndex; if(i<0) return updateStatus('No selection'); state.shapes[i].locked = !state.shapes[i].locked; updateStatus('Shape '+(state.shapes[i].locked? 'locked':'unlocked')); pushHistory(); redraw(); }
    function clearCanvas(){ if(!confirm('Clear all shapes?')) return; state.shapes = []; state.selectedIndex=-1; pushHistory(); redraw(); updateSelectionUI(); updateStatus('Canvas cleared'); }

    document.getElementById('delete-selected').addEventListener('click', deleteSelected);
    document.getElementById('dup-selected').addEventListener('click', duplicateSelected);
    document.getElementById('clear-canvas').addEventListener('click', clearCanvas);

    // Export shapes to a simple SVG
    function exportSVG(){ const w = canvas.width / DPR, h = canvas.height / DPR; let svg = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 ${w} ${h}" width="${w}" height="${h}">`;
      for(const s of state.shapes){ if(s.tool==='rect'){ const x=Math.min(s.x1,s.x2), y=Math.min(s.y1,s.y2), ww=Math.abs(s.x2-s.x1), hh=Math.abs(s.y2-s.y1); svg += `<rect x="${x}" y="${y}" width="${ww}" height="${hh}" stroke="${s.color}" stroke-width="${s.width}" fill="${s.isRoom? 'rgba(10,120,80,0.06)':'none'}"/>`; } else if(s.tool==='circle'){ const cx=(s.x1+s.x2)/2, cy=(s.y1+s.y2)/2, rx=Math.abs((s.x2-s.x1)/2), ry=Math.abs((s.y2-s.y1)/2); svg += `<ellipse cx="${cx}" cy="${cy}" rx="${rx}" ry="${ry}" stroke="${s.color}" stroke-width="${s.width}" fill="${s.isRoom? 'rgba(10,120,80,0.06)':'none'}"/>`; } else if(s.tool==='line' || s.tool==='measure'){ svg += `<line x1="${s.x1}" y1="${s.y1}" x2="${s.x2}" y2="${s.y2}" stroke="${s.color}" stroke-width="${s.width}" />`; } else if(s.tool==='pencil' || s.tool==='erase'){ const pts = (s.points||[]).map(p=>`${p.x},${p.y}`).join(' '); svg += `<polyline points="${pts}" fill="none" stroke="${s.color}" stroke-width="${s.width}" stroke-linecap="round" stroke-linejoin="round" />`; } }
      svg += '</svg>';
      const blob = new Blob([svg],{type:'image/svg+xml'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = 'davenport_export.svg'; a.click(); URL.revokeObjectURL(url); updateStatus('Exported SVG'); }

    const exportBtnSvg = document.createElement('button'); exportBtnSvg.textContent='Export SVG'; exportBtnSvg.className='btn'; exportBtnSvg.style.marginLeft='8px'; document.querySelector('.topbar .controls').appendChild(exportBtnSvg); exportBtnSvg.addEventListener('click', exportSVG);

    // Initialize AI Graphics Controller
    const aiController = new AIGraphicsController({
      endpoint: '/api/generate',
      apiKey: state.token
    });

    // Initialize AI Graphics UI
    const aiUI = new AIGraphicsUI(aiController, {
      container: document.getElementById('ai-graphics-panel'),
      onResult: (generation) => {
        if (generation?.result?.data?.[0]?.url || generation?.result?.data?.[0]?.b64_json) {
          const imgUrl = generation.result.data[0].url || 
                        `data:image/png;base64,${generation.result.data[0].b64_json}`;
          const img = new Image();
          img.onload = () => {
            bgImage = img;
            redraw();
            updateStatus('Generated image applied as background');
          };
          img.src = imgUrl;
        }
      }
    });

    // --- AI Layouts: render thumbnails and provide actions (apply background / load vector / download) ---
    function createLayoutThumbElement(profile){
      const el = document.createElement('div'); el.className = 'layout-item';
      const thumb = document.createElement('div'); thumb.className = 'layout-thumb';
      const img = document.createElement('img'); img.alt = profile.name || 'layout-thumb'; img.src = profile.thumbnail || '';
      thumb.appendChild(img);

      const actions = document.createElement('div'); actions.style.display='flex'; actions.style.gap='6px'; actions.style.marginTop='6px';
      const btnBg = document.createElement('button'); btnBg.textContent = 'Apply as Background'; btnBg.className='';
      const btnLoad = document.createElement('button'); btnLoad.textContent = 'Load Layout'; btnLoad.className='secondary';
      const btnDown = document.createElement('button'); btnDown.textContent = 'Download'; btnDown.className='secondary';

      btnBg.addEventListener('click', ()=>{
        // apply thumbnail (SVG or raster) as bgImage
        if(!profile.thumbnail) return updateStatus('No thumbnail available');
        const imgObj = new Image(); imgObj.onload = ()=>{ bgImage = imgObj; redraw(); updateStatus('Applied layout as background: '+(profile.name||'layout')); };
        imgObj.src = profile.thumbnail;
      });

      btnLoad.addEventListener('click', ()=>{
        if(!profile.shapes || !profile.shapes.length) return updateStatus('No vector layout data in this profile');
        if(state.shapes.length && !confirm('Loading this layout will replace your current drawing. Continue?')) return;
        state.shapes = JSON.parse(JSON.stringify(profile.shapes)); pushHistory(); redraw(); updateStatus('Loaded layout: '+(profile.name||'layout'));
      });

      btnDown.addEventListener('click', ()=>{
        // temporarily apply thumbnail as bg, then export PNG of canvas
        if(profile.thumbnail){ const prevBg = bgImage; const tmpImg = new Image(); tmpImg.onload = ()=>{ bgImage = tmpImg; redraw(); // export using existing export logic
            setTimeout(()=>{ // use export button logic: create download
              const w=canvas.width, h=canvas.height; const tmp=document.createElement('canvas'); tmp.width=w; tmp.height=h; const tctx=tmp.getContext('2d'); tctx.scale(DPR,DPR); if(bgImage) tctx.drawImage(bgImage,0,0,canvas.width/DPR,canvas.height/DPR); else { tctx.fillStyle='#fff'; tctx.fillRect(0,0,canvas.width/DPR,canvas.height/DPR); } tctx.drawImage(canvas,0,0); const url=tmp.toDataURL('image/png'); const a=document.createElement('a'); a.href=url; a.download=(profile.name||'layout')+'.png'; a.click(); updateStatus('Downloaded layout PNG'); bgImage = prevBg; redraw(); }, 250);
          }; tmpImg.src = profile.thumbnail; } else { updateStatus('No thumbnail to download'); }
      });

      actions.appendChild(btnBg); actions.appendChild(btnLoad); actions.appendChild(btnDown);

      el.appendChild(thumb); el.appendChild(actions);
      return el;
    }

    function renderLayoutBar(){
      const container = document.getElementById('layout-thumbs'); container.innerHTML = '';
      // built-in sample layout(s)
      const builtins = [];
      try{ if(typeof sampleProfile !== 'undefined') builtins.push(sampleProfile); }catch(e){}
      // include stored profiles as layouts as well
      const stored = getProfiles();
      const combined = builtins.concat(stored.slice(0,6));
      if(!combined.length){ container.innerHTML = '<div class="small">No layouts available yet.</div>'; return; }
      combined.forEach(p=>{
        const el = createLayoutThumbElement(p);
        container.appendChild(el);
      });
    }

    document.getElementById('refresh-layouts').addEventListener('click', ()=>{ renderLayoutBar(); updateStatus('Layouts refreshed'); });
    document.getElementById('open-layouts-folder').addEventListener('click', ()=>{ // open profiles UI
      if(typeof renderProfiles === 'function'){ renderProfiles(); updateStatus('Opened profiles list'); }
    });

    // initial render of the layout bar (after profiles sample defined)
    try{ renderLayoutBar(); }catch(e){ console.warn('renderLayoutBar init failed', e); }


    // init
  setTool('pencil'); resize(); pushHistory(); updateSelectionUI(); updateZoomDisplay();
    // register service worker if available (enables PWA install and offline caching when served from server)
    if('serviceWorker' in navigator){
      navigator.serviceWorker.register('/service-worker.js').then(reg=>{
        console.log('ServiceWorker registered', reg.scope);
      }).catch(err=>{
        console.warn('ServiceWorker registration failed', err);
      });
    }
  </script>
</body>
</html>    improve the above code 

