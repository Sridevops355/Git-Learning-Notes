<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GitHub Profile README — Preview</title>
<link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600&family=JetBrains+Mono:wght@300;400;700&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #0d1117;
    color: #c9d1d9;
    font-family: 'Fira Code', monospace;
    line-height: 1.7;
    min-height: 100vh;
  }

  /* GitHub-like container */
  .gh-container {
    max-width: 780px;
    margin: 0 auto;
    padding: 32px 24px 80px;
    background: #0d1117;
  }

  /* Top label */
  .preview-label {
    text-align: center;
    font-size: 11px;
    letter-spacing: 0.15em;
    color: #30363d;
    text-transform: uppercase;
    margin-bottom: 32px;
    border-bottom: 1px solid #21262d;
    padding-bottom: 16px;
  }

  .preview-label span {
    background: #161b22;
    border: 1px solid #30363d;
    padding: 4px 12px;
    border-radius: 20px;
    color: #00d4ff;
  }

  /* Terminal banner */
  .terminal-banner {
    background: #010409;
    border: 1px solid #21262d;
    border-radius: 10px;
    padding: 20px 24px;
    margin-bottom: 28px;
    font-family: 'JetBrains Mono', monospace;
    position: relative;
    overflow: hidden;
    animation: fadeIn 0.6s ease;
  }

  .terminal-banner::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, #00d4ff, #7c3aed, #10b981);
  }

  .terminal-dots {
    display: flex;
    gap: 6px;
    margin-bottom: 14px;
  }

  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot.r { background: #ff5f57; }
  .dot.y { background: #febc2e; }
  .dot.g { background: #28c840; }

  .terminal-code {
    font-size: 13px;
    color: #00d4ff;
    line-height: 1.5;
  }

  .terminal-code .dim { color: #30363d; }
  .terminal-code .green { color: #39d353; }

  /* Heading sections */
  .section-heading {
    font-family: 'JetBrains Mono', monospace;
    font-size: 15px;
    font-weight: 700;
    color: #00d4ff;
    border-bottom: 1px solid #21262d;
    padding-bottom: 10px;
    margin: 32px 0 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .section-heading::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, #21262d, transparent);
  }

  /* YAML block */
  .yaml-block {
    background: #010409;
    border: 1px solid #21262d;
    border-radius: 8px;
    padding: 20px 24px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.8;
    animation: fadeIn 0.6s 0.1s both;
  }

  .yaml-key { color: #7ee787; }
  .yaml-val { color: #a5d6ff; }
  .yaml-str { color: #ff7b72; }
  .yaml-comment { color: #8b949e; }
  .yaml-arr { color: #ffa657; }

  /* Badge grid */
  .badge-section {
    margin-bottom: 20px;
    animation: fadeIn 0.6s 0.2s both;
  }

  .badge-label {
    font-size: 12px;
    color: #8b949e;
    margin-bottom: 10px;
    font-family: 'JetBrains Mono', monospace;
  }

  .badge-row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 16px;
  }

  .badge {
    height: 28px;
    border-radius: 4px;
    display: flex;
    align-items: center;
    padding: 0 12px;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 0.04em;
    gap: 6px;
    font-family: 'Fira Code', monospace;
  }

  .badge-aws { background: #FF9900; color: #000; }
  .badge-azure { background: #0072C6; color: #fff; }
  .badge-tf { background: #5835CC; color: #fff; }
  .badge-docker { background: #0db7ed; color: #fff; }
  .badge-k8s { background: #326ce5; color: #fff; }
  .badge-gh { background: #2671E5; color: #fff; }
  .badge-jenkins { background: #2C5263; color: #fff; }
  .badge-selenium { background: #43B02A; color: #fff; }
  .badge-postman { background: #FF6C37; color: #fff; }
  .badge-python { background: #3670A0; color: #ffdd54; }
  .badge-bash { background: #121011; color: #fff; border: 1px solid #333; }
  .badge-git { background: #F05033; color: #fff; }
  .badge-linux { background: #FCC624; color: #000; }

  /* Journey log */
  .journey-block {
    background: #010409;
    border: 1px solid #21262d;
    border-radius: 8px;
    padding: 20px 24px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12.5px;
    line-height: 2;
    animation: fadeIn 0.6s 0.3s both;
  }

  .j-year { color: #ffa657; font-weight: 700; }
  .j-arrow { color: #30363d; }
  .j-title { color: #e6edf3; font-weight: 600; }
  .j-sub { color: #8b949e; padding-left: 20px; display: block; }
  .j-now { color: #00d4ff; font-weight: 700; }

  /* Projects */
  .projects-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    animation: fadeIn 0.6s 0.35s both;
  }

  .project-card {
    background: #161b22;
    border: 1px solid #21262d;
    border-radius: 8px;
    padding: 14px 16px;
    transition: border-color 0.2s;
  }

  .project-card:hover { border-color: #00d4ff; }

  .project-icon { font-size: 18px; margin-bottom: 6px; }
  .project-title { font-size: 13px; font-weight: 600; color: #e6edf3; margin-bottom: 4px; }
  .project-desc { font-size: 11.5px; color: #8b949e; line-height: 1.5; }

  /* Stats */
  .stats-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    animation: fadeIn 0.6s 0.4s both;
  }

  .stat-card {
    background: #161b22;
    border: 1px solid #21262d;
    border-radius: 8px;
    padding: 20px;
    text-align: center;
  }

  .stat-number { font-size: 28px; font-weight: 700; color: #00d4ff; display: block; }
  .stat-label { font-size: 11px; color: #8b949e; margin-top: 4px; }

  /* Connect */
  .connect-section {
    background: #010409;
    border: 1px solid #21262d;
    border-radius: 8px;
    padding: 24px;
    text-align: center;
    animation: fadeIn 0.6s 0.45s both;
  }

  .connect-btns {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }

  .connect-btn {
    height: 32px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    padding: 0 16px;
    font-size: 12px;
    font-weight: 600;
    gap: 6px;
    text-decoration: none;
    font-family: 'Fira Code', monospace;
  }

  .btn-li { background: #0077B5; color: #fff; }
  .btn-pt { background: #00d4ff22; border: 1px solid #00d4ff55; color: #00d4ff; }
  .btn-em { background: #D14836; color: #fff; }

  .connect-code {
    background: #0d1117;
    border: 1px solid #30363d;
    border-radius: 6px;
    padding: 14px 20px;
    font-size: 12px;
    line-height: 1.8;
    color: #8b949e;
    text-align: left;
    display: inline-block;
    width: 100%;
  }

  .connect-code .gt { color: #00d4ff; }
  .connect-code .avail { color: #39d353; }

  .divider {
    border: none;
    border-top: 1px solid #21262d;
    margin: 28px 0;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 600px) {
    .projects-grid, .stats-row { grid-template-columns: 1fr; }
  }

  /* Typing animation */
  .typing-line {
    overflow: hidden;
    border-right: 2px solid #00d4ff;
    white-space: nowrap;
    animation: typing 3s steps(40) 0.5s both, blink 0.75s step-end infinite;
    color: #00d4ff;
    font-size: 14px;
    max-width: fit-content;
    margin: 0 auto;
  }

  @keyframes typing {
    from { max-width: 0; }
    to { max-width: 100%; }
  }

  @keyframes blink {
    50% { border-color: transparent; }
  }

  .profile-views {
    display: inline-block;
    background: #161b22;
    border: 1px solid #00d4ff33;
    color: #00d4ff;
    font-size: 11px;
    padding: 3px 12px;
    border-radius: 4px;
    margin-top: 16px;
    font-family: 'JetBrains Mono', monospace;
  }
</style>
</head>
<body>
<div class="gh-container">

  <div class="preview-label">
    <span>⚡ GitHub Profile README — Preview</span>
  </div>

  <!-- Terminal Header -->
  <div class="terminal-banner">
    <div class="terminal-dots">
      <div class="dot r"></div>
      <div class="dot y"></div>
      <div class="dot g"></div>
    </div>
    <div class="terminal-code">
      <div class="dim">╔══════════════════════════════════════════════════════════╗</div>
      <div>&nbsp;&nbsp;INITIALIZING PROFILE... <span class="green">██████████████ 100%</span></div>
      <div>&nbsp;&nbsp;CLOUD &amp; DEVOPS ENGINEER &nbsp;·&nbsp; QA → INFRA</div>
      <div class="dim">╚══════════════════════════════════════════════════════════╝</div>
    </div>
  </div>

  <!-- Name & Typing -->
  <div style="text-align:center; margin-bottom: 28px;">
    <h1 style="font-family:'JetBrains Mono',monospace; font-size:26px; color:#e6edf3; margin-bottom:8px;">
      <span style="color:#00d4ff">$ whoami</span> — Cloud &amp; DevOps Engineer
    </h1>
    <p style="color:#8b949e; font-size:13px; margin-bottom:14px;">QA background · Cloud-native mindset · Automation-first thinking</p>
    <div class="typing-line">Building reliable infrastructure at scale...</div>
  </div>

  <hr class="divider">

  <!-- About -->
  <div class="section-heading">$ cat about_me.txt</div>
  <div class="yaml-block">
    <span class="yaml-key">name</span><span class="yaml-comment">:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="yaml-str">"Your Name"</span><br>
    <span class="yaml-key">role</span><span class="yaml-comment">:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="yaml-str">"Cloud &amp; DevOps Engineer | Independent Consultant"</span><br>
    <span class="yaml-key">location</span><span class="yaml-comment">:&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="yaml-str">"Your City, Country"</span><br>
    <span class="yaml-key">experience</span><span class="yaml-comment">:&nbsp;&nbsp;</span><span class="yaml-str">"5 years QA/Automation → Cloud &amp; DevOps"</span><br><br>
    <span class="yaml-arr">currently</span><span class="yaml-comment">:</span><br>
    &nbsp;&nbsp;<span class="yaml-comment">- </span><span class="yaml-val">"Designing cloud infrastructure on AWS &amp; Azure"</span><br>
    &nbsp;&nbsp;<span class="yaml-comment">- </span><span class="yaml-val">"Building CI/CD pipelines with GitHub Actions &amp; Jenkins"</span><br>
    &nbsp;&nbsp;<span class="yaml-comment">- </span><span class="yaml-val">"Working with Docker, Kubernetes &amp; Terraform"</span><br>
    &nbsp;&nbsp;<span class="yaml-comment">- </span><span class="yaml-val">"Bridging QA discipline with DevOps engineering"</span><br><br>
    <span class="yaml-key">philosophy</span><span class="yaml-comment">:&nbsp;&nbsp;</span><span class="yaml-str">"Quality isn't just in the code. It's in the pipeline."</span>
  </div>

  <!-- Tech Stack -->
  <div class="section-heading">$ ls ./tech-stack/</div>

  <div class="badge-section">
    <div class="badge-label">☁️ Cloud Platforms</div>
    <div class="badge-row">
      <span class="badge badge-aws">☁ AWS</span>
      <span class="badge badge-azure">⬡ Azure</span>
    </div>

    <div class="badge-label">⚙️ DevOps & Infrastructure</div>
    <div class="badge-row">
      <span class="badge badge-tf">◆ Terraform</span>
      <span class="badge badge-docker">🐳 Docker</span>
      <span class="badge badge-k8s">⎈ Kubernetes</span>
      <span class="badge badge-gh">⚡ GitHub Actions</span>
      <span class="badge badge-jenkins">⚙ Jenkins</span>
    </div>

    <div class="badge-label">✅ QA & Automation (My Foundation)</div>
    <div class="badge-row">
      <span class="badge badge-selenium">✓ Selenium</span>
      <span class="badge badge-postman">◉ Postman</span>
    </div>

    <div class="badge-label">💻 Languages & Tools</div>
    <div class="badge-row">
      <span class="badge badge-python">🐍 Python</span>
      <span class="badge badge-bash">$ Bash</span>
      <span class="badge badge-git">⑂ Git</span>
      <span class="badge badge-linux">🐧 Linux</span>
    </div>
  </div>

  <!-- Journey -->
  <div class="section-heading">$ cat journey.log</div>
  <div class="journey-block">
    <span class="j-year">[2019]</span> <span class="j-arrow">──►</span> <span class="j-title">Entered tech as QA Engineer</span><br>
    <span class="j-sub">└─ Manual testing, test planning, defect tracking</span>
    <br>
    <span class="j-year">[2021]</span> <span class="j-arrow">──►</span> <span class="j-title">Leveled up to Automation Engineering</span><br>
    <span class="j-sub">└─ Selenium, API testing, CI/CD integration</span>
    <br>
    <span class="j-year">[2023]</span> <span class="j-arrow">──►</span> <span class="j-title">Discovered the DevOps rabbit hole</span><br>
    <span class="j-sub">└─ Started learning Docker, Kubernetes, Terraform</span>
    <br>
    <span class="j-year">[2024]</span> <span class="j-arrow">──►</span> <span class="j-title">Went independent as Cloud &amp; DevOps Consultant</span><br>
    <span class="j-sub">└─ AWS, Azure, GitHub Actions, Jenkins, IaC</span>
    <br>
    <span class="j-now">[NOW]</span>&nbsp; <span class="j-arrow">──►</span> <span class="j-title">Building cloud-native infrastructure</span><br>
    <span class="j-sub">└─ Quality-driven systems, automation-first mindset</span>
  </div>

  <!-- Projects -->
  <div class="section-heading">$ tail -f ./current_projects.log</div>
  <div class="projects-grid">
    <div class="project-card">
      <div class="project-icon">🔧</div>
      <div class="project-title">CI/CD Pipeline Automation</div>
      <div class="project-desc">GitHub Actions + Docker + AWS ECS end-to-end deployment pipeline</div>
    </div>
    <div class="project-card">
      <div class="project-icon">☁️</div>
      <div class="project-title">Cloud Infrastructure IaC</div>
      <div class="project-desc">Terraform modules for multi-environment AWS setup</div>
    </div>
    <div class="project-card">
      <div class="project-icon">📊</div>
      <div class="project-title">DevOps Monitoring Stack</div>
      <div class="project-desc">Prometheus + Grafana deployed on Kubernetes</div>
    </div>
    <div class="project-card">
      <div class="project-icon">🧪</div>
      <div class="project-title">Test Infrastructure Migration</div>
      <div class="project-desc">Moving legacy QA pipelines to cloud-native architecture</div>
    </div>
  </div>

  <!-- Connect -->
  <div class="section-heading">$ ping connect</div>
  <div class="connect-section">
    <div class="connect-btns">
      <a href="#" class="connect-btn btn-li">💼 LinkedIn</a>
      <a href="#" class="connect-btn btn-pt">🌐 Portfolio</a>
      <a href="#" class="connect-btn btn-em">✉ Email</a>
    </div>
    <div class="connect-code">
      <span class="gt">&gt;</span> <span class="avail">Available</span> for Cloud &amp; DevOps consulting engagements<br>
      <span class="gt">&gt;</span> <span class="avail">Open</span> to full-time Cloud / DevOps / SRE engineering roles<br>
      <span class="gt">&gt;</span> Let's build reliable infrastructure together.
    </div>
    <div class="profile-views">👁 PROFILE VIEWS: 1,024+</div>
  </div>

</div>
</body>
</html>