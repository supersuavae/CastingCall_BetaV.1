<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Casting Call — The Filmmaker Puzzle</title>
<style>
  :root {
    --bg: #0a0a0a;
    --surface: #141414;
    --surface2: #1e1e1e;
    --border: #2a2a2a;
    --border2: #333;
    --text: #f0f0f0;
    --text2: #999;
    --text3: #555;
    --gold: #c9a84c;
    --gold-light: #e8c96a;
    --gold-bg: rgba(201,168,76,0.08);
    --gold-bg2: rgba(201,168,76,0.15);
    --green: #1d9e75;
    --green-bg: rgba(29,158,117,0.1);
    --red: #e24b4a;
    --red-bg: rgba(226,75,74,0.1);
    --purple: #7f77dd;
    --purple-bg: rgba(127,119,221,0.1);
    --teal: #1d9e75;
    --radius: 10px;
    --radius-sm: 6px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; min-height: 100vh; }

  /* NAV */
  .nav { display: flex; align-items: center; justify-content: space-between; padding: 1rem 1.5rem; border-bottom: 1px solid var(--border); background: var(--surface); position: sticky; top: 0; z-index: 100; }
  .nav-logo { display: flex; align-items: center; gap: 10px; }
  .nav-logo-icon { width: 32px; height: 32px; background: var(--gold); border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 16px; }
  .nav-logo-text { font-size: 15px; font-weight: 600; color: var(--text); }
  .nav-logo-sub { font-size: 10px; color: var(--text2); letter-spacing: .1em; text-transform: uppercase; }
  .nav-stats { display: flex; gap: 16px; align-items: center; }
  .nav-stat { display: flex; align-items: center; gap: 5px; font-size: 13px; font-weight: 500; }
  .nav-stat-icon { font-size: 14px; }

  /* SCREENS */
  .screen { display: none; max-width: 680px; margin: 0 auto; padding: 2rem 1rem; }
  .screen.active { display: block; }

  /* HOME */
  .home-hero { text-align: center; padding: 3rem 1rem 2rem; }
  .home-title { font-size: 42px; font-weight: 700; color: var(--gold); letter-spacing: -.02em; margin-bottom: 8px; }
  .home-sub { font-size: 16px; color: var(--text2); margin-bottom: 2rem; line-height: 1.6; }
  .home-tiers { display: grid; grid-template-columns: repeat(5,1fr); gap: 8px; margin-bottom: 2rem; }
  .tier-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: .6rem .4rem; text-align: center; }
  .tier-chip-name { font-size: 11px; font-weight: 600; margin-bottom: 2px; }
  .tier-chip-roles { font-size: 10px; color: var(--text2); }
  .home-btn { background: var(--gold); color: #000; border: none; border-radius: var(--radius); padding: 14px 40px; font-size: 16px; font-weight: 700; cursor: pointer; width: 100%; max-width: 320px; transition: background .15s; }
  .home-btn:hover { background: var(--gold-light); }
  .home-desc { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 1.25rem; margin-top: 1.5rem; text-align: left; }
  .home-desc p { font-size: 13px; color: var(--text2); line-height: 1.7; margin-bottom: .5rem; }
  .home-desc p:last-child { margin-bottom: 0; }

  /* GAME */
  .tier-bar { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: .9rem 1rem; margin-bottom: 1rem; }
  .tier-bar-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
  .tier-bar-name { font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: .08em; }
  .tier-bar-progress { font-size: 12px; color: var(--text2); }
  .tier-steps { display: flex; gap: 4px; }
  .tier-step { flex: 1; height: 3px; border-radius: 2px; background: var(--border2); transition: background .3s; }
  .tier-step.done { background: var(--gold); }
  .tier-step.current { background: var(--gold-light); }
  .tier-nav { display: flex; gap: 6px; margin-bottom: 1rem; }
  .tier-nav-item { flex: 1; text-align: center; padding: 5px 4px; border-radius: var(--radius-sm); border: 1px solid var(--border); font-size: 10px; color: var(--text3); font-weight: 500; transition: all .2s; }
  .tier-nav-item.active { border-color: var(--gold); color: var(--gold); background: var(--gold-bg); }
  .tier-nav-item.completed { border-color: var(--green); color: var(--green); background: var(--green-bg); }

  .scene-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 1.25rem; margin-bottom: 1rem; }
  .scene-label { font-size: 10px; font-weight: 600; color: var(--text3); text-transform: uppercase; letter-spacing: .08em; margin-bottom: 6px; }
  .scene-title { font-size: 18px; font-weight: 700; color: var(--text); margin-bottom: 6px; }
  .scene-roles-count { font-size: 12px; color: var(--gold); margin-bottom: 8px; }
  .scene-desc { font-size: 14px; color: var(--text2); line-height: 1.65; }
  .scene-warning { font-size: 11px; color: var(--red); margin-top: 8px; font-style: italic; }

  .roles-section { margin-bottom: 1rem; }
  .roles-label { font-size: 12px; font-weight: 600; color: var(--text2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 8px; }
  .role-slot { background: var(--surface); border: 1.5px dashed var(--border2); border-radius: var(--radius); padding: .75rem 1rem; margin-bottom: 6px; display: flex; align-items: center; gap: 12px; cursor: pointer; transition: all .15s; min-height: 58px; }
  .role-slot:hover { border-color: var(--gold); }
  .role-slot.filled { border-style: solid; border-color: var(--border2); background: var(--surface2); }
  .role-slot.correct { border-color: var(--green); background: var(--green-bg); }
  .role-slot.wrong { border-color: var(--red); background: var(--red-bg); }
  .role-num { width: 26px; height: 26px; border-radius: 50%; background: var(--surface2); border: 1px solid var(--border2); display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 600; color: var(--text2); flex-shrink: 0; }
  .role-slot.correct .role-num { background: var(--green); border-color: var(--green); color: #fff; }
  .role-slot.wrong .role-num { background: var(--red); border-color: var(--red); color: #fff; }
  .role-info { flex: 1; }
  .role-name { font-size: 12px; font-weight: 600; color: var(--text2); margin-bottom: 2px; }
  .role-cast { font-size: 14px; font-weight: 600; color: var(--gold); }
  .role-empty { font-size: 13px; color: var(--text3); }
  .role-check { font-size: 18px; }

  .pool-section { margin-bottom: 1rem; }
  .pool-label { font-size: 12px; font-weight: 600; color: var(--text2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 8px; display: flex; justify-content: space-between; }
  .pool-count { font-weight: 400; color: var(--text3); }
  .pool-grid { display: flex; flex-wrap: wrap; gap: 7px; }
  .char-chip { border: 1px solid var(--border2); border-radius: 20px; padding: 7px 14px; font-size: 12px; font-weight: 500; color: var(--text); background: var(--surface); cursor: pointer; transition: all .12s; user-select: none; }
  .char-chip:hover { border-color: var(--gold); color: var(--gold); }
  .char-chip.selected { background: var(--gold-bg2); border-color: var(--gold); color: var(--gold-light); }
  .char-chip.used { opacity: .25; pointer-events: none; }

  .action-row { display: flex; gap: 8px; margin-bottom: 1rem; }
  .btn-primary { flex: 1; background: var(--gold); color: #000; border: none; border-radius: var(--radius); padding: 12px; font-size: 14px; font-weight: 700; cursor: pointer; transition: background .15s; }
  .btn-primary:hover { background: var(--gold-light); }
  .btn-secondary { background: transparent; color: var(--text2); border: 1px solid var(--border2); border-radius: var(--radius); padding: 12px 20px; font-size: 14px; font-weight: 500; cursor: pointer; transition: all .15s; }
  .btn-secondary:hover { border-color: var(--text2); color: var(--text); }

  .feedback-card { border-radius: var(--radius); padding: 1rem 1.1rem; margin-bottom: 1rem; display: none; }
  .feedback-card.show { display: block; }
  .feedback-card.ok { background: var(--green-bg); border: 1px solid var(--green); }
  .feedback-card.fail { background: var(--red-bg); border: 1px solid var(--red); }
  .feedback-header { display: flex; align-items: center; gap: 8px; margin-bottom: 6px; }
  .feedback-icon { font-size: 18px; }
  .feedback-title { font-size: 14px; font-weight: 700; }
  .feedback-title.ok { color: var(--green); }
  .feedback-title.fail { color: var(--red); }
  .feedback-points { font-size: 12px; font-weight: 600; color: var(--gold); margin-bottom: 6px; }
  .feedback-explain { font-size: 13px; color: var(--text2); line-height: 1.6; }

  /* TIER COMPLETE */
  .tier-complete { text-align: center; padding: 2rem 1rem; }
  .tc-icon { font-size: 48px; margin-bottom: 1rem; }
  .tc-title { font-size: 26px; font-weight: 700; color: var(--gold); margin-bottom: 6px; }
  .tc-sub { font-size: 14px; color: var(--text2); margin-bottom: .5rem; line-height: 1.6; }
  .tc-score { font-size: 16px; font-weight: 600; color: var(--text); margin-bottom: 1.5rem; }
  .tc-next { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 1rem; margin-bottom: 1.5rem; text-align: left; }
  .tc-next-label { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 4px; }
  .tc-next-name { font-size: 15px; font-weight: 600; margin-bottom: 2px; }
  .tc-next-desc { font-size: 12px; color: var(--text2); }

  /* END SCREEN */
  .end-screen { text-align: center; padding: 2rem 1rem; }
  .end-title { font-size: 32px; font-weight: 800; color: var(--gold); margin-bottom: 8px; letter-spacing: -.02em; }
  .end-verdict { font-size: 15px; color: var(--text2); margin-bottom: 1.5rem; line-height: 1.7; max-width: 400px; margin-left: auto; margin-right: auto; }
  .end-scorecard { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 1.25rem; margin-bottom: 1.25rem; text-align: left; }
  .end-scorecard-title { font-size: 11px; font-weight: 600; color: var(--text3); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 1rem; }
  .end-tier-row { display: flex; align-items: center; justify-content: space-between; padding: .5rem 0; border-bottom: 1px solid var(--border); }
  .end-tier-row:last-child { border-bottom: none; }
  .end-tier-name { font-size: 13px; font-weight: 500; }
  .end-tier-score { font-size: 14px; font-weight: 700; color: var(--gold); }
  .end-total { display: flex; align-items: center; justify-content: space-between; margin-top: 1rem; padding-top: 1rem; border-top: 1px solid var(--border2); }
  .end-total-label { font-size: 14px; font-weight: 600; }
  .end-total-score { font-size: 24px; font-weight: 800; color: var(--gold); }
  .end-badges { display: flex; flex-wrap: wrap; gap: 6px; justify-content: center; margin-bottom: 1.5rem; }
  .end-badge { background: var(--gold-bg); border: 1px solid var(--gold); border-radius: 20px; padding: 5px 12px; font-size: 11px; font-weight: 600; color: var(--gold); }
  .end-btn-row { display: flex; gap: 8px; justify-content: center; flex-wrap: wrap; }
  .end-btn { border: 1px solid var(--border2); border-radius: var(--radius); padding: 10px 22px; font-size: 13px; font-weight: 600; cursor: pointer; background: transparent; color: var(--text); transition: all .15s; }
  .end-btn:hover { border-color: var(--text2); }
  .end-btn.primary { background: var(--gold); color: #000; border-color: var(--gold); }
  .end-btn.primary:hover { background: var(--gold-light); }

  /* SHARE CARD */
  .share-card { background: var(--surface2); border: 1px solid var(--gold); border-radius: var(--radius); padding: 1.25rem; margin-bottom: 1rem; display: none; }
  .share-card.show { display: block; }
  .share-card-title { font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 8px; }
  .share-text { font-family: monospace; font-size: 13px; color: var(--gold-light); line-height: 1.6; white-space: pre-wrap; background: var(--bg); border-radius: var(--radius-sm); padding: .75rem; margin-bottom: .75rem; }
  .copy-btn { background: var(--gold-bg2); border: 1px solid var(--gold); border-radius: var(--radius-sm); padding: 8px 16px; font-size: 12px; font-weight: 600; color: var(--gold); cursor: pointer; transition: all .15s; }
  .copy-btn:hover { background: var(--gold); color: #000; }

  /* STRIKE DISPLAY */
  .strikes-display { display: flex; gap: 6px; }
  .strike-dot { width: 10px; height: 10px; border-radius: 50%; border: 1.5px solid var(--border2); transition: all .2s; }
  .strike-dot.hit { background: var(--red); border-color: var(--red); }

  @media (max-width: 500px) {
    .home-title { font-size: 32px; }
    .home-tiers { grid-template-columns: repeat(5,1fr); }
    .tier-chip-name { font-size: 9px; }
    .tier-nav-item { font-size: 9px; padding: 4px 2px; }
    .nav-logo-sub { display: none; }
  }
</style>
</head>
<body>

<nav class="nav">
  <div class="nav-logo">
    <div class="nav-logo-icon">🎬</div>
    <div>
      <div class="nav-logo-text">Casting Call</div>
      <div class="nav-logo-sub">Envious Culture</div>
    </div>
  </div>
  <div class="nav-stats" id="nav-stats" style="display:none;">
    <div class="nav-stat"><span class="nav-stat-icon">🏆</span><span id="nav-score">0</span></div>
    <div class="strikes-display" id="nav-strikes">
      <div class="strike-dot" id="s1"></div>
      <div class="strike-dot" id="s2"></div>
      <div class="strike-dot" id="s3"></div>
    </div>
  </div>
</nav>

<!-- HOME -->
<div class="screen active" id="screen-home">
  <div class="home-hero">
    <div class="home-title">Casting Call</div>
    <div class="home-sub">Match character archetypes to cinematic roles.<br>Five tiers. Twenty-five scenes. One question:<br><strong>Do you really know your craft?</strong></div>
    <div class="home-tiers">
      <div class="tier-chip"><div class="tier-chip-name" style="color:#888;">Intern</div><div class="tier-chip-roles">2 roles</div></div>
      <div class="tier-chip"><div class="tier-chip-name" style="color:#378add;">Director</div><div class="tier-chip-roles">3 roles</div></div>
      <div class="tier-chip"><div class="tier-chip-name" style="color:#d85a30;">Executive</div><div class="tier-chip-roles">4 roles</div></div>
      <div class="tier-chip"><div class="tier-chip-name" style="color:#7f77dd;">Legend</div><div class="tier-chip-roles">5 roles</div></div>
      <div class="tier-chip"><div class="tier-chip-name" style="color:#1d9e75;">Auteur</div><div class="tier-chip-roles">5 roles</div></div>
    </div>
    <button class="home-btn" onclick="startGame()">Start your career</button>
    <div class="home-desc">
      <p>Select a character from the pool, then tap a role slot to cast them. Every scene has a correct cast based on real storytelling logic — genre, emotional subtext, and character function.</p>
      <p>Three strikes and your run ends. Score high enough and you might just call yourself an Auteur.</p>
      <p style="color:var(--gold);font-size:12px;font-weight:600;">Built by Envious Culture Productions</p>
    </div>
  </div>
</div>

<!-- GAME -->
<div class="screen" id="screen-game">
  <div class="tier-nav" id="tier-nav"></div>
  <div class="tier-bar">
    <div class="tier-bar-top">
      <div class="tier-bar-name" id="tb-name"></div>
      <div class="tier-bar-progress" id="tb-progress"></div>
    </div>
    <div class="tier-steps" id="tier-steps"></div>
  </div>
  <div class="scene-card">
    <div class="scene-label">Scene brief</div>
    <div class="scene-title" id="scene-title"></div>
    <div class="scene-roles-count" id="scene-roles-count"></div>
    <div class="scene-desc" id="scene-desc"></div>
    <div class="scene-warning" id="scene-warning"></div>
  </div>
  <div class="roles-section">
    <div class="roles-label">Cast the roles</div>
    <div id="role-slots"></div>
  </div>
  <div class="pool-section">
    <div class="pool-label">Talent pool <span class="pool-count" id="pool-count"></span></div>
    <div class="pool-grid" id="pool-grid"></div>
  </div>
  <div class="feedback-card" id="feedback"></div>
  <div class="action-row">
    <button class="btn-primary" id="submit-btn" onclick="submitCast()">Lock in cast</button>
    <button class="btn-secondary" onclick="clearSlots()">Clear</button>
  </div>
</div>

<!-- TIER COMPLETE -->
<div class="screen" id="screen-tier-complete">
  <div class="tier-complete">
    <div class="tc-icon" id="tc-icon"></div>
    <div class="tc-title" id="tc-title"></div>
    <div class="tc-sub" id="tc-sub"></div>
    <div class="tc-score" id="tc-score"></div>
    <div class="tc-next" id="tc-next"></div>
    <button class="btn-primary" id="tc-btn" onclick="advanceTier()"></button>
  </div>
</div>

<!-- END -->
<div class="screen" id="screen-end">
  <div class="end-screen">
    <div class="end-title" id="end-title"></div>
    <div class="end-verdict" id="end-verdict"></div>
    <div class="end-scorecard" id="end-scorecard"></div>
    <div class="end-badges" id="end-badges"></div>
    <div class="share-card" id="share-card">
      <div class="share-card-title">Share your result</div>
      <div class="share-text" id="share-text"></div>
      <button class="copy-btn" onclick="copyShare()">Copy to clipboard</button>
    </div>
    <div class="end-btn-row">
      <button class="end-btn primary" onclick="restartGame()">Play again</button>
      <button class="end-btn" onclick="toggleShare()">Share result</button>
      <button class="end-btn" onclick="showScreen('screen-home')">Home</button>
    </div>
  </div>
</div>

<script>
const TIERS = [
  { name:'Intern', color:'#888780', desc:'2 roles per scene. Clear genre logic. Learn the craft.' },
  { name:'Director', color:'#378add', desc:'3 roles per scene. Subtext matters as much as text.' },
  { name:'Executive', color:'#d85a30', desc:'4 roles per scene. Decoys designed to mislead.' },
  { name:'Legend', color:'#7f77dd', desc:'5 roles per scene. Abstract and thematic.' },
  { name:'Auteur', color:'#1d9e75', desc:'5 roles per scene. No rules. Borderline impossible.' }
];

const SCENES = [
  // INTERN
  {tier:0,title:'The Setup — crime drama',desc:'Two criminals scope a job. One is the brains, one is the muscle. The scene needs quiet menace, not chaos.',roles:['The Brains','The Muscle'],pool:['The Calculated Loner','The Hothead','The Loyal Soldier','The Charming Snake','The Grieving Mother','The Comic Relief'],correct:['The Calculated Loner','The Loyal Soldier'],explain:'Menace needs control. The Calculated Loner plans in silence; the Loyal Soldier executes without question.'},
  {tier:0,title:'First Love — coming-of-age',desc:'Two teenagers at the edge of something. The scene needs vulnerability, not performance.',roles:['The Pursuer','The Uncertain One'],pool:['The Reckless Hero','The Quiet Dreamer','The Wallflower','The Class Clown','The Overachiever','The Hothead'],correct:['The Quiet Dreamer','The Wallflower'],explain:'Pursuit driven by genuine feeling, not bravado. Uncertainty lands harder when it\'s shy, not dramatic.'},
  {tier:0,title:'The Argument — family drama',desc:'A parent and adult child rehash a wound that never healed. Restraint beats screaming every time.',roles:['The Parent','The Child'],pool:['The Guilt-Ridden Saint','The Proud Outcast','The Overactor','The Stoic Provider','The Broken Idealist','The Enabler'],correct:['The Stoic Provider','The Broken Idealist'],explain:'A stoic provider inflicts damage with silence. The broken idealist carries the wound of unmet expectations.'},
  {tier:0,title:'The Heist — action comedy',desc:'One last score before retirement. The scene needs tension cut with comic timing.',roles:['The Veteran','The Wildcard'],pool:['The Calculated Loner','The Loveable Screw-Up','The Straight Man','The Comic Relief','The Hothead','The Ghost'],correct:['The Straight Man','The Loveable Screw-Up'],explain:'Comedy needs contrast. The Straight Man\'s frustration amplifies every mistake the Loveable Screw-Up makes.'},
  {tier:0,title:'The Warning — thriller',desc:'Someone knows something dangerous. They have to warn someone without being seen doing it.',roles:['The Messenger','The Receiver'],pool:['The Paranoid Witness','The Calculated Loner','The Naive Idealist','The Skeptic','The Ghost','The Hothead'],correct:['The Paranoid Witness','The Skeptic'],explain:'Paranoia creates urgency. The Skeptic creates dramatic irony — the audience knows more than they do.'},
  // DIRECTOR
  {tier:1,title:'The Interrogation — neo-noir',desc:'Detective, suspect, and a lawyer who may be playing both sides. Power shifts every 30 seconds.',roles:['The Detective','The Suspect','The Lawyer'],pool:['The Relentless Investigator','The Charming Snake','The Morally Flexible Ally','The Hothead','The Broken Idealist','The Calculated Loner','The Enabler','The Ghost'],correct:['The Relentless Investigator','The Calculated Loner','The Morally Flexible Ally'],explain:'Relentless pursuit vs. controlled silence. The Morally Flexible Ally keeps everyone — including the audience — off balance.'},
  {tier:1,title:'The Vigil — war drama',desc:'Three soldiers wait for orders that may never come. The scene is about what isn\'t said.',roles:['The Leader','The Believer','The Doubter'],pool:['The Stoic Protector','The Loyal Soldier','The Broken Idealist','The Hothead','The Ghost','The Calculated Loner','The Reckless Hero','The Quiet Observer'],correct:['The Stoic Protector','The Loyal Soldier','The Broken Idealist'],explain:'Leadership under silence needs composure. Faith needs someone who still believes. Doubt humanizes the whole scene.'},
  {tier:1,title:'The Pitch — satirical comedy',desc:'A desperate creator sells an idea to two executives who\'ve heard everything. The scene mocks Hollywood without breaking character.',roles:['The Creator','The Cynical Exec','The Quietly Interested Exec'],pool:['The Eager Newcomer','The Jaded Veteran','The Secret Romantic','The Hothead','The Comic Relief','The Overachiever','The Charming Snake','The Straight Man'],correct:['The Eager Newcomer','The Jaded Veteran','The Secret Romantic'],explain:'Desperation vs. boredom is the engine. But the scene needs one exec who secretly wants to believe — that\'s where the satire bites.'},
  {tier:1,title:'The Reunion — psychological drama',desc:'Three estranged siblings at their childhood home. Each one carries a different version of the same memory.',roles:['The One Who Left','The One Who Stayed','The One Who Rewrote It'],pool:['The Proud Outcast','The Enabler','The Guilt-Ridden Saint','The Overactor','The Broken Idealist','The Stoic Provider','The Paranoid Witness','The Quiet Observer'],correct:['The Proud Outcast','The Guilt-Ridden Saint','The Enabler'],explain:'Leaving breeds pride. Staying breeds guilt. And someone always enables the family myth — that\'s the one who rewrote it.'},
  {tier:1,title:'The Betrayal — political thriller',desc:'A senator, their chief of staff, and a journalist. Someone is leaking. Someone knows who. Someone is pretending not to.',roles:['The Senator','The Chief of Staff','The Journalist'],pool:['The Arrogant Gatekeeper','The Morally Flexible Ally','The Relentless Investigator','The Calculated Loner','The Charming Snake','The Paranoid Witness','The Ghost','The Loyal Soldier'],correct:['The Arrogant Gatekeeper','The Morally Flexible Ally','The Relentless Investigator'],explain:'Arrogance makes power fragile. The Chief\'s flexibility is the leak. And no journalist stops digging — that\'s the moral spine of the scene.'},
  // EXECUTIVE
  {tier:2,title:'The Trial — courtroom drama',desc:'A case that isn\'t really about guilt. Four roles: truth, image, silence, and sacrifice.',roles:['Truth-Seeker','Image Protector','The Silent Witness','The Sacrificed'],pool:['The Relentless Investigator','The Arrogant Gatekeeper','The Quiet Observer','The Broken Idealist','The Charming Snake','The Naive Idealist','The Loyal Soldier','The Paranoid Witness','The Enabler','The Ghost'],correct:['The Relentless Investigator','The Arrogant Gatekeeper','The Quiet Observer','The Broken Idealist'],explain:'Truth needs obsession. Image needs charm-as-armor. Silence needs stillness, not paranoia. What gets destroyed is always the idealist.'},
  {tier:2,title:'The Collapse — financial thriller',desc:'A company implodes in real time. Architect, Cassandra, opportunist, and the one left holding the wreckage.',roles:['The Architect','The Cassandra','The Opportunist','The Left Behind'],pool:['The Charming Snake','The Paranoid Witness','The Morally Flexible Ally','The Broken Idealist','The Calculated Loner','The Arrogant Gatekeeper','The Enabler','The Loyal Soldier','The Ghost','The Eager Newcomer'],correct:['The Charming Snake','The Paranoid Witness','The Morally Flexible Ally','The Broken Idealist'],explain:'Collapse needs charisma at the top. Cassandras are dismissed. The opportunist stays flexible to profit. It\'s always a broken idealist left in the rubble.'},
  {tier:2,title:'The Escape — action thriller',desc:'Four characters, one exit. They can\'t all make it. The scene is about who decides who stays.',roles:['The Decider','The Sacrifice','The Reluctant Survivor','The One Who Runs'],pool:['The Stoic Protector','The Loyal Soldier','The Broken Idealist','The Reckless Hero','The Calculated Loner','The Coward','The Ghost','The Hothead','The Quiet Observer','The Eager Newcomer'],correct:['The Stoic Protector','The Loyal Soldier','The Broken Idealist','The Coward'],explain:'The Decider needs moral authority. Sacrifice is loyalty — the soldier stays. Reluctance is idealism colliding with survival. Someone always runs.'},
  {tier:2,title:'The Last Sermon — religious drama',desc:'A congregation fractures. Four voices: the preacher losing faith, the devoted, the apostate, and the one asking what no one wants answered.',roles:['The Preacher','The Devoted','The Apostate','The Questioner'],pool:['The Guilt-Ridden Saint','The Enabler','The Proud Outcast','The Naive Idealist','The Broken Idealist','The Calculated Loner','The Relentless Investigator','The Stoic Provider','The Paranoid Witness','The Charming Snake'],correct:['The Guilt-Ridden Saint','The Enabler','The Proud Outcast','The Naive Idealist'],explain:'Guilt drives the preacher\'s crisis. Devotion needs an enabler. Return needs pride. The dangerous question always comes from innocence.'},
  {tier:2,title:'The Negotiation — espionage',desc:'Four people in a room. Two sides. But allegiances aren\'t what they seem. Theme: trust is a transaction.',roles:['Side A Lead','Side B Lead','The Double Agent','The Variable'],pool:['The Calculated Loner','The Charming Snake','The Morally Flexible Ally','The Ghost','The Relentless Investigator','The Arrogant Gatekeeper','The Paranoid Witness','The Stoic Protector','The Loyal Soldier','The Eager Newcomer'],correct:['The Calculated Loner','The Arrogant Gatekeeper','The Morally Flexible Ally','The Ghost'],explain:'Side A needs control. Side B needs overconfidence — that\'s the exploitable flaw. The double agent stays flexible. The Variable is whoever has no clear loyalty.'},
  // LEGEND
  {tier:3,title:'The Mirror — surreal drama',desc:'A character confronts five aspects of themselves: who they were, who they are, who they want to be, who others see, and who they\'re afraid of becoming.',roles:['Who They Were','Who They Are','Who They Want to Be','Who Others See','Who They Fear'],pool:['The Broken Idealist','The Calculated Loner','The Quiet Dreamer','The Arrogant Gatekeeper','The Ghost','The Charming Snake','The Naive Idealist','The Stoic Protector','The Paranoid Witness','The Proud Outcast','The Enabler','The Reckless Hero'],correct:['The Naive Idealist','The Calculated Loner','The Quiet Dreamer','The Charming Snake','The Ghost'],explain:'We begin naive. We become calculated. We dream of peace. We perform charm for the world. And we\'re haunted by what we might erase to get there.'},
  {tier:3,title:'The Reckoning — abstract crime',desc:'Five archetypes in a room where a crime occurred — but the crime is metaphorical. A dying indie film studio. What was stolen was vision itself.',roles:['The Victim','The Perpetrator','The Witness','The Beneficiary','The Innocent'],pool:['The Broken Idealist','The Arrogant Gatekeeper','The Quiet Observer','The Morally Flexible Ally','The Eager Newcomer','The Ghost','The Calculated Loner','The Guilt-Ridden Saint','The Enabler','The Loyal Soldier','The Paranoid Witness','The Charming Snake'],correct:['The Broken Idealist','The Arrogant Gatekeeper','The Quiet Observer','The Morally Flexible Ally','The Eager Newcomer'],explain:'Vision belongs to the idealist — so the idealist loses it. Power kills it. Someone saw it happen and stayed quiet. Flexibility profited. Someone new arrives after it\'s already gone.'},
  {tier:3,title:'The Transmission — sci-fi drama',desc:'Five roles across time. A message is sent. By the time it arrives, everything has changed. Theme: what we meant vs. what was received.',roles:['The Sender','The Translator','The Distortion','The Receiver','The Echo'],pool:['The Quiet Dreamer','The Morally Flexible Ally','The Paranoid Witness','The Broken Idealist','The Ghost','The Calculated Loner','The Eager Newcomer','The Enabler','The Stoic Protector','The Charming Snake','The Loyal Soldier','The Naive Idealist'],correct:['The Quiet Dreamer','The Morally Flexible Ally','The Paranoid Witness','The Broken Idealist','The Ghost'],explain:'Dreamers send hope. Flexible interpreters shift meaning in transit. Paranoia distorts signal into threat. The broken receiver hears only what confirms the wound. An echo was never really there.'},
  {tier:3,title:'The Archive — historical drama',desc:'History is being recorded — but by five people with five agendas. Theme: power decides what is remembered.',roles:['The Conqueror','The Scribe','The Erased','The Myth-Maker','The Truth-Keeper'],pool:['The Arrogant Gatekeeper','The Morally Flexible Ally','The Ghost','The Charming Snake','The Broken Idealist','The Guilt-Ridden Saint','The Loyal Soldier','The Paranoid Witness','The Proud Outcast','The Relentless Investigator','The Naive Idealist','The Quiet Observer'],correct:['The Arrogant Gatekeeper','The Morally Flexible Ally','The Ghost','The Charming Snake','The Relentless Investigator'],explain:'Power rewrites with confidence. The scribe adapts to survive. The erased becomes a ghost. Charm builds the myth. Someone relentless keeps digging for what actually happened.'},
  {tier:3,title:'The Last Frame — meta cinema',desc:'The final scene of a film that doesn\'t exist yet. Five archetypes must carry the entire emotional weight of a story we never saw.',roles:['The One We Followed','The One We Forgot','The One Who Changed Everything','The One Left Behind','The One Who Was Always There'],pool:['The Broken Idealist','The Ghost','The Calculated Loner','The Loyal Soldier','The Quiet Observer','The Charming Snake','The Naive Idealist','The Proud Outcast','The Relentless Investigator','The Enabler','The Stoic Protector','The Guilt-Ridden Saint'],correct:['The Broken Idealist','The Ghost','The Calculated Loner','The Loyal Soldier','The Quiet Observer'],explain:'We follow the one who breaks under their own ideals. We forget the ghost. The calculated one made the choice that pivoted everything. Loyalty is what gets left behind. The observer was always there — we just weren\'t paying attention.'},
  // AUTEUR
  {tier:4,title:'The Void — experimental horror',desc:'Five archetypes trapped in a space with no exit. The horror isn\'t external — it\'s what each character reveals when hope disappears. Cast for psychological texture, not plot function.',roles:['The Rationalizer','The Believer','The One Who Accepts','The One Who Breaks','The One Who Was Never Really There'],pool:['The Calculated Loner','The Naive Idealist','The Quiet Observer','The Hothead','The Ghost','The Broken Idealist','The Arrogant Gatekeeper','The Enabler','The Stoic Protector','The Paranoid Witness','The Charming Snake','The Guilt-Ridden Saint'],correct:['The Calculated Loner','The Naive Idealist','The Stoic Protector','The Hothead','The Ghost'],explain:'Rationalization is control maintaining itself in chaos. Faith needs naivety to stay intact. Acceptance needs composure beyond ego. Breaking needs someone whose identity depends on external validation. The one never really there was always the ghost.'},
  {tier:4,title:'The Inheritance — magical realism',desc:'A family divides an inheritance that isn\'t money — it\'s a burden, a secret, and a curse. Cast for what each heir does with what they receive.',roles:['Who Gets the Burden','Who Gets the Secret','Who Gets the Curse','Who Gets Nothing','Who Gets Everything and Loses It'],pool:['The Loyal Soldier','The Guilt-Ridden Saint','The Paranoid Witness','The Ghost','The Arrogant Gatekeeper','The Broken Idealist','The Enabler','The Charming Snake','The Quiet Observer','The Reckless Hero','The Stoic Provider','The Proud Outcast'],correct:['The Loyal Soldier','The Guilt-Ridden Saint','The Paranoid Witness','The Arrogant Gatekeeper','The Broken Idealist'],explain:'Burdens are carried by the loyal — they don\'t put them down. Guilt keeps secrets. Paranoia is the curse — it sees everything and is believed by no one. Arrogance takes everything. The idealist gets everything and is destroyed by what it means.'},
  {tier:4,title:'The Frequency — psychological sci-fi',desc:'Five characters connected across timelines by a signal they didn\'t ask to receive. Theme: the cost of being witnessed.',roles:['The Origin','The Amplifier','The Filter','The Static','The Silence'],pool:['The Quiet Dreamer','The Eager Newcomer','The Morally Flexible Ally','The Paranoid Witness','The Ghost','The Calculated Loner','The Broken Idealist','The Stoic Protector','The Charming Snake','The Naive Idealist','The Guilt-Ridden Saint','The Enabler'],correct:['The Quiet Dreamer','The Eager Newcomer','The Morally Flexible Ally','The Paranoid Witness','The Ghost'],explain:'Vision lives in the dreamer. Amplification needs someone new enough to transmit without filtering. Flexibility distorts meaning in transit. Paranoia turns signal into noise. Silence is the ghost — receives everything, responds with nothing.'},
  {tier:4,title:'The Apology — abstract drama',desc:'An apology is being made. We never find out what for. Cast for emotional function, not narrative logic.',roles:['The Apologizer','The Unforgiving','The Proxy','The Indifferent','The Real Target'],pool:['The Guilt-Ridden Saint','The Proud Outcast','The Enabler','The Quiet Observer','The Ghost','The Broken Idealist','The Calculated Loner','The Stoic Provider','The Naive Idealist','The Paranoid Witness','The Charming Snake','The Loyal Soldier'],correct:['The Guilt-Ridden Saint','The Proud Outcast','The Enabler','The Stoic Provider','The Ghost'],explain:'Guilt is the only engine for a real apology. Pride makes forgiveness structurally impossible. An enabler accepts things on behalf of others — that\'s their function in every room. The indifferent one doesn\'t need what\'s being offered. The real target was always the ghost — the one not in the room.'},
  {tier:4,title:'The Last Collaboration — meta auteur',desc:'Two filmmakers at the end of their partnership. Five roles aren\'t characters — they\'re elements of the collaboration itself. The hardest cast in the game.',roles:['The Vision','The Execution','The Compromise','The Doubt','The Thing That Made It Worth It'],pool:['The Quiet Dreamer','The Calculated Loner','The Morally Flexible Ally','The Broken Idealist','The Loyal Soldier','The Ghost','The Arrogant Gatekeeper','The Naive Idealist','The Paranoid Witness','The Enabler','The Guilt-Ridden Saint','The Quiet Observer'],correct:['The Quiet Dreamer','The Calculated Loner','The Morally Flexible Ally','The Broken Idealist','The Ghost'],explain:'Vision lives in the dreamer — it doesn\'t explain itself. Execution is the calculated loner. Every collaboration compromises — the flexible one absorbs the loss. Doubt is always the broken idealist. The thing that made it worth it is always a ghost. You only understand what it was after it\'s gone.'}
];

// Game state
let state = {};

function initState() {
  state = {
    tierIdx: 0,
    roundIdx: 0,
    score: 0,
    strikes: 0,
    streak: 0,
    tierScores: [0,0,0,0,0],
    perfectTiers: [],
    selectedChar: null,
    slots: [],
    submitted: false,
    phase: 'game'
  };
}

function shuffle(arr) { return arr.slice().sort(() => Math.random() - .5); }

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function startGame() {
  initState();
  document.getElementById('nav-stats').style.display = 'flex';
  showScreen('screen-game');
  renderGame();
}

function getScenesByTier(t) {
  return SCENES.filter(s => s.tier === t);
}

function getCurrentScene() {
  return getScenesByTier(state.tierIdx)[state.roundIdx];
}

function updateNavStats() {
  document.getElementById('nav-score').textContent = state.score;
  ['s1','s2','s3'].forEach((id,i) => {
    document.getElementById(id).className = 'strike-dot' + (i < state.strikes ? ' hit' : '');
  });
}

function renderTierNav() {
  const el = document.getElementById('tier-nav');
  el.innerHTML = TIERS.map((t,i) => {
    let cls = 'tier-nav-item';
    if (i < state.tierIdx) cls += ' completed';
    else if (i === state.tierIdx) cls += ' active';
    return `<div class="${cls}" style="${i===state.tierIdx?'border-color:'+t.color+';color:'+t.color+';background:'+t.color+'18':i<state.tierIdx?'border-color:#1d9e75;color:#1d9e75;background:#1d9e7510':''}">${t.name}</div>`;
  }).join('');
}

function renderTierBar() {
  const t = TIERS[state.tierIdx];
  const scenes = getScenesByTier(state.tierIdx);
  document.getElementById('tb-name').textContent = t.name + ' Tier';
  document.getElementById('tb-name').style.color = t.color;
  document.getElementById('tb-progress').textContent = `Scene ${state.roundIdx + 1} of ${scenes.length}`;
  const steps = document.getElementById('tier-steps');
  steps.innerHTML = scenes.map((_,i) => {
    let cls = 'tier-step';
    if (i < state.roundIdx) cls += ' done';
    else if (i === state.roundIdx) cls += ' current';
    return `<div class="${cls}" style="${i<=state.roundIdx?'background:'+t.color:''}"></div>`;
  }).join('');
}

function renderGame() {
  if (state.phase === 'tier-complete') { renderTierComplete(); return; }
  if (state.phase === 'end') { renderEnd(); return; }

  const scene = getCurrentScene();
  state.slots = scene.roles.map(() => null);
  state.submitted = false;
  state.selectedChar = null;

  renderTierNav();
  renderTierBar();
  updateNavStats();

  document.getElementById('scene-title').textContent = scene.title;
  document.getElementById('scene-roles-count').textContent = scene.roles.length + ' roles to cast';
  document.getElementById('scene-desc').textContent = scene.desc;
  const warn = document.getElementById('scene-warning');
  warn.textContent = state.tierIdx >= 2 ? 'Warning: this tier includes decoys designed to mislead.' : '';

  renderSlots();
  renderPool(shuffle(scene.pool));
  hideFeedback();
}

function renderSlots() {
  const scene = getCurrentScene();
  const el = document.getElementById('role-slots');
  el.innerHTML = scene.roles.map((role, i) => {
    const cast = state.slots[i];
    let cls = 'role-slot' + (cast ? ' filled' : '');
    if (state.submitted) {
      cls += cast === scene.correct[i] ? ' correct' : ' wrong';
    }
    const check = state.submitted ? (cast === scene.correct[i] ? '✓' : '✗') : '';
    return `<div class="${cls}" onclick="assignToSlot(${i})">
      <div class="role-num">${i+1}</div>
      <div class="role-info">
        <div class="role-name">${role}</div>
        ${cast ? `<div class="role-cast">${cast}</div>` : `<div class="role-empty">tap to cast</div>`}
      </div>
      ${check ? `<div class="role-check">${check}</div>` : ''}
    </div>`;
  }).join('');
}

function renderPool(chars) {
  const el = document.getElementById('pool-grid');
  const avail = chars.filter(c => !state.slots.includes(c)).length;
  document.getElementById('pool-count').textContent = avail + ' available';
  el.innerHTML = chars.map(c => {
    let cls = 'char-chip';
    if (state.slots.includes(c)) cls += ' used';
    else if (state.selectedChar === c) cls += ' selected';
    return `<div class="${cls}" onclick="selectChar('${c.replace(/'/g,"\\'")}', this)">${c}</div>`;
  }).join('');
}

function selectChar(name, el) {
  if (state.submitted) return;
  if (state.selectedChar === name) { state.selectedChar = null; }
  else { state.selectedChar = name; }
  renderPool(shuffle(getCurrentScene().pool));
}

function assignToSlot(i) {
  if (state.submitted || !state.selectedChar) return;
  const prev = state.slots.indexOf(state.selectedChar);
  if (prev !== -1) state.slots[prev] = null;
  state.slots[i] = state.selectedChar;
  state.selectedChar = null;
  renderSlots();
  renderPool(shuffle(getCurrentScene().pool));
}

function clearSlots() {
  if (state.submitted) return;
  state.slots = getCurrentScene().roles.map(() => null);
  state.selectedChar = null;
  renderSlots();
  renderPool(shuffle(getCurrentScene().pool));
  hideFeedback();
}

function submitCast() {
  if (state.submitted) return;
  const scene = getCurrentScene();
  if (state.slots.some(s => !s)) {
    showFeedback('Fill all roles before locking in.', false, '', '');
    return;
  }
  state.submitted = true;
  document.getElementById('submit-btn').disabled = true;

  const allCorrect = scene.roles.every((_,i) => state.slots[i] === scene.correct[i]);
  renderSlots();

  const bonusMap = [100, 150, 200, 250, 300];
  const base = bonusMap[state.tierIdx];

  if (allCorrect) {
    const streakBonus = state.streak >= 2 ? 50 : 0;
    const pts = base + streakBonus;
    state.score += pts;
    state.tierScores[state.tierIdx] += pts;
    state.streak++;
    const msg = streakBonus ? `Streak bonus +50! ` : '';
    showFeedback('Perfect cast!', true, `+${pts} points${streakBonus ? ' (streak bonus)' : ''}`, msg + scene.explain);
  } else {
    state.strikes++;
    state.streak = 0;
    showFeedback('Wrong cast.', false, '', scene.explain);
  }

  updateNavStats();

  setTimeout(() => {
    document.getElementById('submit-btn').disabled = false;
    if (state.strikes >= 3) { state.phase = 'end'; renderEnd(); return; }
    const tierScenes = getScenesByTier(state.tierIdx);
    state.roundIdx++;
    if (state.roundIdx >= tierScenes.length) {
      state.phase = 'tier-complete';
      if (allCorrect) state.perfectTiers.push(state.tierIdx);
      renderTierComplete();
    } else {
      renderGame();
    }
  }, 2800);
}

function showFeedback(title, ok, points, explain) {
  const el = document.getElementById('feedback');
  el.className = 'feedback-card show ' + (ok ? 'ok' : 'fail');
  el.innerHTML = `
    <div class="feedback-header">
      <div class="feedback-icon">${ok ? '✓' : '✗'}</div>
      <div class="feedback-title ${ok?'ok':'fail'}">${title}</div>
    </div>
    ${points ? `<div class="feedback-points">${points}</div>` : ''}
    ${explain ? `<div class="feedback-explain">${explain}</div>` : ''}
  `;
}

function hideFeedback() {
  document.getElementById('feedback').className = 'feedback-card';
}

function renderTierComplete() {
  showScreen('screen-tier-complete');
  const t = TIERS[state.tierIdx];
  const ts = state.tierScores[state.tierIdx];
  const maxMap = [500, 750, 1000, 1250, 1500];
  const pct = Math.round((ts / maxMap[state.tierIdx]) * 100);
  let verdict = pct >= 80 ? 'Flawless. You read every scene.' : pct >= 50 ? 'Solid instincts. A few miscasts.' : 'You survived. Barely.';

  document.getElementById('tc-icon').textContent = pct >= 80 ? '🎬' : pct >= 50 ? '🎥' : '📋';
  document.getElementById('tc-title').textContent = t.name + ' — cleared';
  document.getElementById('tc-title').style.color = t.color;
  document.getElementById('tc-sub').textContent = verdict;
  document.getElementById('tc-score').textContent = `Tier score: ${ts}`;

  const nextT = TIERS[state.tierIdx + 1];
  const tcNext = document.getElementById('tc-next');
  if (nextT) {
    tcNext.innerHTML = `<div class="tc-next-label">Next tier</div><div class="tc-next-name" style="color:${nextT.color}">${nextT.name}</div><div class="tc-next-desc">${nextT.desc}</div>`;
  } else {
    tcNext.innerHTML = `<div class="tc-next-label">Final tier complete</div><div class="tc-next-name" style="color:var(--gold)">Career run complete</div><div class="tc-next-desc">See your final results and scorecard.</div>`;
  }

  const btn = document.getElementById('tc-btn');
  btn.textContent = nextT ? `Enter ${nextT.name} tier →` : 'See final results →';
}

function advanceTier() {
  state.tierIdx++;
  state.roundIdx = 0;
  state.submitted = false;
  state.selectedChar = null;
  state.slots = [];
  if (state.tierIdx >= TIERS.length) {
    state.phase = 'end';
    renderEnd();
  } else {
    state.phase = 'game';
    showScreen('screen-game');
    renderGame();
  }
}

function renderEnd() {
  showScreen('screen-end');
  const total = state.score;
  const bustedOut = state.strikes >= 3;
  const pct = Math.round((total / 3750) * 100);
  let title, verdict;

  if (bustedOut) {
    title = 'Struck out.';
    verdict = 'Three bad casts and the greenlight is gone. Study the briefs — genre logic and emotional subtext are non-negotiable. Come back when you\'re ready.';
  } else if (pct >= 90) {
    title = 'Legend.';
    verdict = 'Perfect cast across all five tiers. Envious Culture would greenlight anything you brought. You don\'t just know the craft — you live it.';
  } else if (pct >= 70) {
    title = 'Auteur in progress.';
    verdict = 'Strong vision, a few blind spots. The upper tiers exposed where instinct overrides craft. Study the explanations and run it again.';
  } else if (pct >= 50) {
    title = 'Working director.';
    verdict = 'You know the craft. Now trust it more. The Legend and Auteur tiers showed the gap between technique and true storytelling.';
  } else {
    title = 'Back to the lot.';
    verdict = 'The scenes beat you. But you made it through — that means something. Read every explanation carefully. They\'re the real lesson.';
  }

  document.getElementById('end-title').textContent = title;
  document.getElementById('end-verdict').textContent = verdict;

  const tierReached = TIERS[Math.min(state.tierIdx, TIERS.length-1)].name;
  const sc = document.getElementById('end-scorecard');
  sc.innerHTML = `
    <div class="end-scorecard-title">Career scorecard</div>
    ${TIERS.map((t,i) => `
      <div class="end-tier-row">
        <div class="end-tier-name" style="color:${t.color}">${t.name}</div>
        <div class="end-tier-score">${state.tierScores[i] || '—'}</div>
      </div>`).join('')}
    <div class="end-total">
      <div class="end-total-label">Final score</div>
      <div class="end-total-score">${total}</div>
    </div>
  `;

  const badges = [];
  if (state.streak >= 5) badges.push('Hot streak');
  if (state.perfectTiers.length >= 3) badges.push('Triple perfect');
  if (state.perfectTiers.includes(4)) badges.push('Auteur perfect');
  if (!bustedOut && state.tierIdx >= 4) badges.push('Full run complete');
  if (total >= 2000) badges.push('2000 club');
  if (pct >= 90) badges.push('Elite caster');
  document.getElementById('end-badges').innerHTML = badges.map(b => `<div class="end-badge">${b}</div>`).join('');

  buildShareCard(title, total, tierReached, badges);
}

function buildShareCard(title, total, tier, badges) {
  const stars = total >= 2500 ? '★★★★★' : total >= 2000 ? '★★★★☆' : total >= 1500 ? '★★★☆☆' : total >= 1000 ? '★★☆☆☆' : '★☆☆☆☆';
  const badgeStr = badges.length ? badges.join(' · ') : 'Keep studying the craft';
  const shareStr = `🎬 CASTING CALL — The Filmmaker Puzzle

Result: ${title}
Score: ${total} ${stars}
Tier reached: ${tier}
${badgeStr}

Do you know your craft?
Play at: enviousculture.com/casting-call

Built by Envious Culture Productions`;
  document.getElementById('share-text').textContent = shareStr;
}

function toggleShare() {
  const card = document.getElementById('share-card');
  card.classList.toggle('show');
}

function copyShare() {
  const text = document.getElementById('share-text').textContent;
  navigator.clipboard.writeText(text).then(() => {
    const btn = document.querySelector('.copy-btn');
    btn.textContent = 'Copied!';
    setTimeout(() => btn.textContent = 'Copy to clipboard', 2000);
  });
}

function restartGame() {
  initState();
  document.getElementById('nav-stats').style.display = 'flex';
  showScreen('screen-game');
  state.phase = 'game';
  renderGame();
}
</script>
</body>
</html>
