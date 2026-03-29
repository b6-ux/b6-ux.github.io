
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Level Up Life</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:#050510;--panel:#0d0d2b;--panel2:#111133;
    --violet:#7c3aed;--violet2:#a855f7;--blue:#3b82f6;--cyan:#06b6d4;
    --gold:#f59e0b;--red:#ef4444;--green:#10b981;--text:#e2e8f0;
    --muted:#64748b;--border:rgba(124,58,237,0.3);
    --glow:0 0 20px rgba(124,58,237,0.5);--glow2:0 0 40px rgba(168,85,247,0.3);
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  body{background:var(--bg);color:var(--text);font-family:'Rajdhani',sans-serif;min-height:100vh;overflow-x:hidden;}
  #particles{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:0;}
  .app{display:flex;flex-direction:column;min-height:100vh;position:relative;z-index:1;max-width:480px;margin:0 auto;}

  /* HEADER */
  .header{padding:16px 20px 12px;background:linear-gradient(180deg,rgba(7,7,25,0.98),rgba(5,5,16,0.9));border-bottom:1px solid var(--border);position:sticky;top:0;z-index:100;backdrop-filter:blur(20px);}
  .header-top{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
  .logo{font-family:'Orbitron',monospace;font-weight:900;font-size:18px;background:linear-gradient(135deg,var(--violet2),var(--cyan));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
  .rank-badge{background:linear-gradient(135deg,var(--violet),var(--violet2));padding:4px 12px;border-radius:20px;font-family:'Orbitron',monospace;font-size:13px;font-weight:700;box-shadow:var(--glow);letter-spacing:2px;}
  .player-row{display:flex;align-items:center;gap:12px;}
  .avatar{width:52px;height:52px;border-radius:50%;border:2px solid var(--violet2);background:radial-gradient(circle at 35% 35%,#1e1b4b,#050510);display:flex;align-items:center;justify-content:center;font-size:24px;position:relative;flex-shrink:0;box-shadow:0 0 15px rgba(124,58,237,0.6);}
  .avatar::after{content:'';position:absolute;inset:-4px;border-radius:50%;border:1px solid rgba(168,85,247,0.3);animation:pulse-ring 2s ease infinite;}
  @keyframes pulse-ring{0%,100%{opacity:1;transform:scale(1)}50%{opacity:0.4;transform:scale(1.1)}}
  .player-info{flex:1;}
  .player-name{font-family:'Orbitron',monospace;font-weight:700;font-size:14px;color:#fff;}
  .player-level{color:var(--violet2);font-size:12px;font-weight:600;letter-spacing:1px;}
  .xp-bar-bg{height:6px;background:rgba(255,255,255,0.08);border-radius:3px;overflow:hidden;margin-top:5px;}
  .xp-bar{height:100%;width:0%;border-radius:3px;background:linear-gradient(90deg,var(--violet),var(--violet2),var(--cyan));box-shadow:0 0 8px rgba(168,85,247,0.8);transition:width 0.8s cubic-bezier(0.22,1,0.36,1);position:relative;overflow:hidden;}
  .xp-bar::after{content:'';position:absolute;top:0;left:-100%;width:100%;height:100%;background:linear-gradient(90deg,transparent,rgba(255,255,255,0.4),transparent);animation:shimmer 2s ease infinite;}
  @keyframes shimmer{to{left:200%;}}
  .xp-text{font-size:10px;color:var(--muted);margin-top:2px;}
  .streak-pill{background:rgba(245,158,11,0.15);border:1px solid rgba(245,158,11,0.4);border-radius:20px;padding:4px 10px;font-size:12px;color:var(--gold);font-weight:700;display:flex;align-items:center;gap:4px;}

  /* NAV */
  .nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:480px;background:rgba(8,8,32,0.97);border-top:1px solid var(--border);display:flex;z-index:100;backdrop-filter:blur(20px);padding:8px 0 max(8px,env(safe-area-inset-bottom));}
  .nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;padding:6px 0;cursor:pointer;color:var(--muted);transition:all 0.2s;font-size:10px;font-weight:600;letter-spacing:0.5px;text-transform:uppercase;position:relative;}
  .nav-item .icon{font-size:22px;transition:transform 0.2s;}
  .nav-item.active{color:var(--violet2);}
  .nav-item.active .icon{transform:scale(1.15);filter:drop-shadow(0 0 6px var(--violet2));}
  .nav-item.active::after{content:'';position:absolute;top:-1px;left:50%;transform:translateX(-50%);width:30px;height:2px;background:var(--violet2);border-radius:0 0 3px 3px;box-shadow:0 0 8px var(--violet2);}

  /* MAIN */
  .main{flex:1;overflow-y:auto;padding:16px;padding-bottom:90px;scrollbar-width:none;}
  .main::-webkit-scrollbar{display:none;}
  .page{display:none;animation:fadeIn 0.3s ease;}
  .page.active{display:block;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}

  .sec{font-family:'Orbitron',monospace;font-size:11px;font-weight:700;letter-spacing:3px;text-transform:uppercase;color:var(--violet2);margin-bottom:12px;display:flex;align-items:center;gap:8px;}
  .sec::after{content:'';flex:1;height:1px;background:linear-gradient(90deg,var(--border),transparent);}

  .card{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:16px;margin-bottom:12px;position:relative;overflow:hidden;}
  .card::before{content:'';position:absolute;top:0;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,var(--violet2),transparent);opacity:0.6;}
  .card-glow{box-shadow:var(--glow2);}

  /* STATS */
  .stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
  .stat-card{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:12px;text-align:center;}
  .stat-icon{font-size:24px;margin-bottom:6px;}
  .stat-name{font-size:10px;letter-spacing:2px;text-transform:uppercase;color:var(--muted);}
  .stat-val{font-family:'Orbitron',monospace;font-size:22px;font-weight:700;}
  .sbar-bg{height:4px;background:rgba(255,255,255,0.07);border-radius:2px;margin-top:8px;}
  .sbar{height:100%;border-radius:2px;transition:width 1s ease;}
  .sbar.f{background:linear-gradient(90deg,#ef4444,#f97316);box-shadow:0 0 6px #ef4444;}
  .sbar.e{background:linear-gradient(90deg,#3b82f6,#06b6d4);box-shadow:0 0 6px #3b82f6;}
  .sbar.d{background:linear-gradient(90deg,#8b5cf6,#a855f7);box-shadow:0 0 6px #8b5cf6;}
  .sbar.s{background:linear-gradient(90deg,#10b981,#34d399);box-shadow:0 0 6px #10b981;}
  .sbar.r{background:linear-gradient(90deg,#06b6d4,#67e8f9);box-shadow:0 0 6px #06b6d4;}

  /* QUESTS */
  .q-tabs{display:flex;gap:8px;margin-bottom:14px;overflow-x:auto;scrollbar-width:none;}
  .q-tabs::-webkit-scrollbar{display:none;}
  .q-tab{padding:6px 16px;border-radius:20px;border:1px solid var(--border);font-size:12px;font-weight:700;letter-spacing:1px;cursor:pointer;white-space:nowrap;transition:all 0.2s;background:var(--panel);flex-shrink:0;}
  .q-tab.active{background:var(--violet);border-color:var(--violet2);color:#fff;box-shadow:0 0 10px rgba(124,58,237,0.5);}
  .qi{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:14px;margin-bottom:8px;display:flex;align-items:center;gap:12px;cursor:pointer;transition:all 0.2s;position:relative;overflow:hidden;}
  .qi:hover{border-color:var(--violet2);transform:translateX(3px);}
  .qi.done{opacity:0.5;pointer-events:none;}
  .qcheck{width:28px;height:28px;border-radius:50%;border:2px solid var(--border);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:14px;transition:all 0.3s;}
  .qinfo{flex:1;}
  .qname{font-weight:700;font-size:15px;margin-bottom:2px;}
  .qsub{font-size:11px;color:var(--muted);}
  .qxp{font-family:'Orbitron',monospace;font-size:12px;font-weight:700;color:var(--gold);white-space:nowrap;}
  .qtag{position:absolute;top:0;right:0;padding:2px 8px;font-size:9px;font-weight:700;letter-spacing:1px;border-radius:0 10px 0 8px;}
  .td{background:rgba(16,185,129,0.2);color:var(--green);}
  .tw{background:rgba(59,130,246,0.2);color:var(--blue);}

  /* WORKOUT */
  .wt-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin-bottom:16px;}
  .wt{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:12px 8px;text-align:center;cursor:pointer;transition:all 0.25s;}
  .wt:hover,.wt.sel{border-color:var(--violet2);background:rgba(124,58,237,0.15);box-shadow:0 0 15px rgba(124,58,237,0.3);}
  .wt-ic{font-size:26px;margin-bottom:6px;}
  .wt-nm{font-size:11px;font-weight:700;letter-spacing:1px;}
  .ig{margin-bottom:14px;}
  .il{font-size:11px;letter-spacing:2px;text-transform:uppercase;color:var(--muted);margin-bottom:6px;}
  .inf{width:100%;background:var(--panel2);border:1px solid var(--border);border-radius:8px;padding:12px 14px;color:var(--text);font-family:'Rajdhani',sans-serif;font-size:15px;font-weight:600;outline:none;transition:border-color 0.2s;}
  .inf:focus{border-color:var(--violet2);}
  .int-row{display:flex;gap:8px;}
  .int-btn{flex:1;padding:10px;border:1px solid var(--border);border-radius:8px;background:var(--panel);text-align:center;cursor:pointer;transition:all 0.2s;font-family:'Rajdhani',sans-serif;font-weight:700;font-size:13px;}
  .int-btn.sel{border-color:var(--violet2);background:rgba(124,58,237,0.2);color:var(--violet2);}
  .x2-tog{background:rgba(245,158,11,0.1);border:1px solid rgba(245,158,11,0.4);border-radius:10px;padding:14px;display:flex;align-items:center;justify-content:space-between;cursor:pointer;margin-bottom:14px;}
  .tog{width:44px;height:24px;background:rgba(255,255,255,0.1);border-radius:12px;position:relative;transition:background 0.3s;cursor:pointer;flex-shrink:0;}
  .tog.on{background:var(--gold);}
  .tog::after{content:'';position:absolute;top:3px;left:3px;width:18px;height:18px;background:#fff;border-radius:50%;transition:transform 0.3s;}
  .tog.on::after{transform:translateX(20px);}

  .btn{width:100%;padding:16px;border:none;border-radius:10px;cursor:pointer;font-family:'Orbitron',monospace;font-weight:700;font-size:14px;letter-spacing:2px;transition:all 0.3s;position:relative;overflow:hidden;}
  .bp{background:linear-gradient(135deg,var(--violet),var(--violet2));color:#fff;box-shadow:0 4px 20px rgba(124,58,237,0.5);}
  .bp:hover{transform:translateY(-2px);box-shadow:0 8px 30px rgba(124,58,237,0.7);}
  .bg{background:linear-gradient(135deg,#059669,#10b981);color:#fff;box-shadow:0 4px 20px rgba(16,185,129,0.4);}
  .bg:hover{transform:translateY(-2px);}
  .bo{background:transparent;border:1px solid var(--border);color:var(--muted);}
  .bo:hover{border-color:var(--violet2);color:var(--violet2);}

  /* BOSS */
  .boss-card{background:linear-gradient(135deg,rgba(239,68,68,0.08),rgba(124,58,237,0.08));border:1px solid rgba(239,68,68,0.35);border-radius:14px;padding:20px;margin-bottom:12px;}
  .boss-lbl{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:3px;color:var(--red);margin-bottom:8px;}
  .boss-nm{font-family:'Orbitron',monospace;font-size:18px;font-weight:900;margin-bottom:6px;}
  .boss-desc{font-size:13px;color:var(--muted);margin-bottom:12px;}
  .boss-bar-bg{height:10px;background:rgba(239,68,68,0.2);border-radius:5px;overflow:hidden;}
  .boss-bar-fill{height:100%;background:linear-gradient(90deg,var(--red),#f97316);border-radius:5px;box-shadow:0 0 10px var(--red);transition:width 0.5s ease;}
  .boss-xp{font-family:'Orbitron',monospace;color:var(--gold);font-weight:700;}

  /* CHEST */
  .chest{background:linear-gradient(135deg,rgba(245,158,11,0.1),rgba(124,58,237,0.1));border:1px solid rgba(245,158,11,0.4);border-radius:14px;padding:20px;text-align:center;margin-bottom:12px;cursor:pointer;transition:all 0.3s;}
  .chest:hover{transform:scale(1.02);box-shadow:0 0 30px rgba(245,158,11,0.3);}
  .chest-ic{font-size:48px;margin-bottom:10px;animation:cg 2s ease infinite;}
  @keyframes cg{0%,100%{filter:drop-shadow(0 0 5px var(--gold))}50%{filter:drop-shadow(0 0 20px var(--gold))}}
  .chest-tmr{font-family:'Orbitron',monospace;font-size:20px;color:var(--gold);margin-top:8px;font-weight:700;}

  /* GRAPH */
  .mini-g{display:flex;align-items:flex-end;gap:4px;height:60px;}
  .bar{flex:1;border-radius:3px 3px 0 0;background:linear-gradient(0deg,var(--violet),var(--violet2));opacity:0.5;min-width:0;}
  .bar.ab{opacity:1;}

  /* MODAL */
  .modal-ov{position:fixed;inset:0;background:rgba(0,0,0,0.8);z-index:1000;display:flex;align-items:center;justify-content:center;backdrop-filter:blur(8px);opacity:0;pointer-events:none;transition:opacity 0.3s;}
  .modal-ov.show{opacity:1;pointer-events:all;}
  .lu-modal{background:var(--panel);border:2px solid var(--violet2);border-radius:20px;padding:40px 30px;text-align:center;width:90%;max-width:360px;box-shadow:0 0 60px rgba(124,58,237,0.5);position:relative;overflow:hidden;animation:mPop 0.5s cubic-bezier(0.22,1,0.36,1);}
  @keyframes mPop{from{transform:scale(0.5);opacity:0}to{transform:scale(1);opacity:1}}
  .lu-modal::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,var(--violet),var(--violet2),var(--cyan),var(--violet2),var(--violet));animation:gm 2s linear infinite;background-size:200%;}
  @keyframes gm{to{background-position:200%;}}
  .lu-lbl{font-family:'Orbitron',monospace;font-size:11px;letter-spacing:4px;color:var(--violet2);margin-bottom:8px;}
  .lu-lvl{font-family:'Orbitron',monospace;font-size:64px;font-weight:900;background:linear-gradient(135deg,var(--violet2),var(--cyan));-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1;animation:lp 0.5s ease infinite alternate;}
  @keyframes lp{from{filter:drop-shadow(0 0 10px var(--violet2))}to{filter:drop-shadow(0 0 30px var(--cyan))}}
  .lu-title{font-size:18px;font-weight:700;margin:12px 0 6px;}
  .lu-sub{color:var(--muted);font-size:13px;margin-bottom:24px;}
  .conf{position:absolute;top:0;left:0;right:0;bottom:0;pointer-events:none;overflow:hidden;}
  .cp{position:absolute;width:8px;height:8px;border-radius:1px;animation:fall 2s ease-in forwards;}
  @keyframes fall{0%{transform:translateY(-20px) rotate(0deg);opacity:1}100%{transform:translateY(600px) rotate(720deg);opacity:0}}

  /* TOAST */
  .toast{position:fixed;top:80px;left:50%;transform:translateX(-50%) translateY(-100px);background:linear-gradient(135deg,var(--violet),var(--violet2));color:#fff;padding:12px 24px;border-radius:30px;font-family:'Orbitron',monospace;font-size:12px;font-weight:700;letter-spacing:2px;z-index:500;transition:transform 0.4s cubic-bezier(0.22,1,0.36,1);box-shadow:0 4px 20px rgba(124,58,237,0.6);white-space:nowrap;max-width:90vw;overflow:hidden;text-overflow:ellipsis;}
  .toast.show{transform:translateX(-50%) translateY(0);}

  /* GOOGLE FIT */
  .fit-card{background:linear-gradient(135deg,rgba(16,185,129,0.06),rgba(59,130,246,0.06));border:1px solid rgba(16,185,129,0.3);border-radius:14px;padding:20px;margin-bottom:12px;position:relative;overflow:hidden;}
  .fit-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,#10b981,#3b82f6);}
  .fit-hdr{display:flex;align-items:center;gap:12px;margin-bottom:16px;}
  .fit-logo{width:44px;height:44px;border-radius:12px;background:linear-gradient(135deg,#ea4335,#fbbc04,#34a853,#4285f4);display:flex;align-items:center;justify-content:center;font-size:22px;flex-shrink:0;}
  .fit-title{font-family:'Orbitron',monospace;font-size:14px;font-weight:700;}
  .fit-sub{font-size:12px;color:var(--muted);}
  .fit-status{display:flex;align-items:center;gap:6px;font-size:12px;font-weight:700;margin-top:2px;}
  .fdot{width:8px;height:8px;border-radius:50%;flex-shrink:0;animation:pd 2s ease infinite;}
  .fdot.on{background:var(--green);box-shadow:0 0 8px var(--green);}
  .fdot.off{background:var(--muted);animation:none;}
  .fdot.sync{background:var(--blue);box-shadow:0 0 8px var(--blue);animation:pd 0.4s ease infinite;}
  @keyframes pd{0%,100%{opacity:1}50%{opacity:0.4}}
  .fit-metrics{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px;}
  .fit-m{background:rgba(0,0,0,0.3);border-radius:10px;padding:12px;text-align:center;border:1px solid rgba(255,255,255,0.05);}
  .fit-m-ic{font-size:20px;margin-bottom:4px;}
  .fit-m-val{font-family:'Orbitron',monospace;font-size:18px;font-weight:700;margin-bottom:2px;}
  .fit-m-nm{font-size:10px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;}
  .fit-xp-row{background:rgba(124,58,237,0.1);border:1px solid var(--border);border-radius:10px;padding:12px;display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;}
  .fit-xp-lbl{font-size:13px;color:var(--muted);}
  .fit-xp-bk{font-size:11px;color:var(--muted);margin-top:2px;}
  .fit-xp-amt{font-family:'Orbitron',monospace;font-size:20px;font-weight:700;color:var(--gold);}
  .fit-act{display:flex;align-items:center;gap:10px;padding:10px;background:rgba(0,0,0,0.2);border-radius:8px;margin-bottom:6px;}
  .fit-act-ic{font-size:18px;}
  .fit-act-info{flex:1;}
  .fit-act-nm{font-size:13px;font-weight:700;}
  .fit-act-sub{font-size:11px;color:var(--muted);}
  .fit-act-xp{font-family:'Orbitron',monospace;font-size:12px;color:var(--gold);font-weight:700;}
  .fit-act.imp{opacity:0.6;}
  .perm-list{margin:12px 0;}
  .perm-item{display:flex;align-items:center;gap:10px;padding:8px 0;font-size:13px;color:var(--muted);border-bottom:1px solid rgba(255,255,255,0.04);}
  .perm-item:last-child{border-bottom:none;}
  .perm-ck{margin-left:auto;color:var(--green);}
  .scope-info{background:rgba(0,0,0,0.3);border-radius:10px;padding:12px;font-size:11px;color:var(--muted);line-height:1.7;margin-bottom:14px;}
  .scope-info strong{color:var(--text);}
  .btn-sm{padding:8px 14px;font-size:11px;width:auto;letter-spacing:1px;}

  /* PROFILE */
  .prof-hdr{text-align:center;padding:20px 0;}
  .prof-av{width:90px;height:90px;border-radius:50%;margin:0 auto 12px;border:3px solid var(--violet2);background:radial-gradient(circle at 35% 35%,#1e1b4b,#050510);display:flex;align-items:center;justify-content:center;font-size:42px;box-shadow:0 0 30px rgba(124,58,237,0.5);position:relative;}
  .prof-av::after{content:'';position:absolute;inset:-6px;border-radius:50%;border:1px dashed rgba(168,85,247,0.4);animation:spin 8s linear infinite;}
  @keyframes spin{to{transform:rotate(360deg);}}
  .prof-name{font-family:'Orbitron',monospace;font-size:20px;font-weight:900;margin-bottom:4px;}
  .prof-ttl{color:var(--violet2);font-size:13px;letter-spacing:2px;}
  .ach-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;}
  .ach{background:var(--panel);border:1px solid var(--border);border-radius:8px;padding:10px 6px;text-align:center;}
  .ach.ul{border-color:var(--gold);background:rgba(245,158,11,0.08);}
  .ach-ic{font-size:24px;margin-bottom:4px;}
  .ach-nm{font-size:9px;color:var(--muted);letter-spacing:0.5px;}
  .ach.ul .ach-nm{color:var(--gold);}

  /* LEADERBOARD */
  .lb-item{display:flex;align-items:center;gap:12px;padding:12px 14px;background:var(--panel);border:1px solid var(--border);border-radius:10px;margin-bottom:8px;}
  .lb-item.me{border-color:var(--violet2);background:rgba(124,58,237,0.1);}
  .lb-rank{font-family:'Orbitron',monospace;font-weight:900;font-size:18px;width:32px;text-align:center;}
  .r1{color:var(--gold);text-shadow:0 0 10px var(--gold);}
  .r2{color:#94a3b8;}.r3{color:#cd7f32;}
  .lb-av{width:40px;height:40px;border-radius:50%;background:radial-gradient(circle,#1e1b4b,#050510);display:flex;align-items:center;justify-content:center;font-size:20px;border:1px solid var(--border);}
  .lb-nm{flex:1;font-weight:700;font-size:15px;}
  .lb-rb{font-size:11px;color:var(--muted);}
  .lb-xp{font-family:'Orbitron',monospace;font-size:12px;color:var(--violet2);font-weight:700;}
  .ch-card{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:16px;margin-bottom:10px;display:flex;gap:12px;align-items:center;}
  .ch-vs{font-family:'Orbitron',monospace;font-size:16px;font-weight:900;color:var(--red);padding:0 4px;}
  .ch-pl{flex:1;text-align:center;}
  .ch-av{font-size:28px;margin-bottom:4px;}
  .ch-nm{font-size:12px;font-weight:700;}
  .ch-pr{font-family:'Orbitron',monospace;font-size:14px;font-weight:700;}

  /* WORKOUT HISTORY */
  .wh-item{display:flex;align-items:center;gap:12px;padding:12px 14px;background:var(--panel);border:1px solid var(--border);border-radius:10px;margin-bottom:8px;}
  .wh-ic{font-size:24px;}
  .wh-info{flex:1;}
  .wh-nm{font-weight:700;font-size:14px;}
  .wh-sub{font-size:11px;color:var(--muted);}
  .wh-xp{font-family:'Orbitron',monospace;font-size:13px;color:var(--gold);font-weight:700;}
  .empty{text-align:center;padding:40px 20px;color:var(--muted);}
  .empty .ei{font-size:48px;margin-bottom:12px;opacity:0.4;}
  .empty .et{font-size:14px;line-height:1.5;}
  .hidden{display:none;}
</style>
</head>
<body>
<canvas id="particles"></canvas>
<div class="toast" id="toast"></div>

<!-- LEVEL UP MODAL -->
<div class="modal-ov" id="luModal">
  <div class="lu-modal">
    <div class="conf" id="conf"></div>
    <div class="lu-lbl">LEVEL UP !</div>
    <div class="lu-lvl" id="luLvl">2</div>
    <div class="lu-title">🎉 Nouveau rang !</div>
    <div class="lu-sub" id="luSub">Tu progresses !</div>
    <button class="btn bp" onclick="closeLU()">CONTINUER ⚡</button>
  </div>
</div>

<div class="app">
  <!-- HEADER -->
  <div class="header">
    <div class="header-top">
      <div class="logo">⚡ LEVEL UP LIFE</div>
      <div style="display:flex;gap:8px;align-items:center;">
        <div class="streak-pill">🔥 <span id="sVal">0</span> jours</div>
        <div class="rank-badge" id="rBadge">RANG E</div>
      </div>
    </div>
    <div class="player-row">
      <div class="avatar">🧑‍🦱</div>
      <div class="player-info">
        <div class="player-name">SHADOW_HUNTER</div>
        <div class="player-level">NIVEAU <span id="pLvl">1</span> • <span id="tXP">0</span> XP TOTAL</div>
        <div class="xp-bar-bg"><div class="xp-bar" id="xpBar"></div></div>
        <div class="xp-text"><span id="cXP">0</span> / <span id="xpNext">100</span> XP → Niveau <span id="nLvl">2</span></div>
      </div>
    </div>
  </div>

  <div class="main">

    <!-- HOME -->
    <div class="page active" id="page-home">
      <div class="sec">TABLEAU DE BORD</div>
      <div class="stats-grid">
        <div class="stat-card"><div class="stat-icon">💪</div><div class="stat-name">FORCE</div><div class="stat-val" style="color:#ef4444" id="sv-f">0</div><div class="sbar-bg"><div class="sbar f" id="sb-f" style="width:0%"></div></div></div>
        <div class="stat-card"><div class="stat-icon">⚡</div><div class="stat-name">ENDURANCE</div><div class="stat-val" style="color:#3b82f6" id="sv-e">0</div><div class="sbar-bg"><div class="sbar e" id="sb-e" style="width:0%"></div></div></div>
        <div class="stat-card"><div class="stat-icon">🧠</div><div class="stat-name">DISCIPLINE</div><div class="stat-val" style="color:#a855f7" id="sv-d">0</div><div class="sbar-bg"><div class="sbar d" id="sb-d" style="width:0%"></div></div></div>
        <div class="stat-card"><div class="stat-icon">❤️</div><div class="stat-name">SANTÉ</div><div class="stat-val" style="color:#10b981" id="sv-s">0</div><div class="sbar-bg"><div class="sbar s" id="sb-s" style="width:0%"></div></div></div>
      </div>

      <div class="card card-glow">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:12px;">
          <div><div style="font-family:'Orbitron',monospace;font-size:11px;letter-spacing:2px;color:var(--muted);">XP CETTE SEMAINE</div><div style="font-family:'Orbitron',monospace;font-size:28px;font-weight:900;color:var(--violet2);" id="wXP">+0 XP</div></div>
          <div style="text-align:right;"><div style="font-size:11px;color:var(--muted);">Aujourd'hui</div><div style="font-family:'Orbitron',monospace;font-size:18px;color:var(--gold);font-weight:700;" id="dXP">+0</div></div>
        </div>
        <div class="mini-g" id="weekG">
          <div class="bar" style="height:2%"></div><div class="bar" style="height:2%"></div><div class="bar" style="height:2%"></div><div class="bar" style="height:2%"></div><div class="bar" style="height:2%"></div><div class="bar" style="height:2%"></div><div class="bar ab" style="height:2%"></div>
        </div>
        <div style="display:flex;justify-content:space-between;margin-top:4px;font-size:10px;color:var(--muted);"><span>Lun</span><span>Mar</span><span>Mer</span><span>Jeu</span><span>Ven</span><span>Sam</span><span>Dim</span></div>
      </div>

      <div class="boss-card">
        <div class="boss-lbl">⚔️ BOSS DE LA SEMAINE</div>
        <div class="boss-nm">Le Défi du Débutant</div>
        <div class="boss-desc">Complète 3 séances de sport cette semaine pour débloquer ta première récompense.</div>
        <div style="display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:4px;"><span>PROGRESSION</span><span id="bossP">0/3 séances</span></div>
        <div class="boss-bar-bg"><div class="boss-bar-fill" id="bossBar" style="width:0%"></div></div>
        <div style="display:flex;align-items:center;gap:8px;font-size:13px;margin-top:12px;"><span>🎁</span><span class="boss-xp">+300 XP</span><span style="color:var(--muted)">+ Titre "Chasseur Novice"</span></div>
      </div>

      <div class="chest" onclick="openChest()" id="chestCard">
        <div class="chest-ic" id="chestIc">📦</div>
        <div style="font-family:'Orbitron',monospace;font-size:14px;font-weight:700;margin-bottom:4px;">COFFRE JOURNALIER</div>
        <div style="font-size:12px;color:var(--muted);">Ouvre ton coffre pour des récompenses</div>
        <div class="chest-tmr" id="chestTmr">DISPONIBLE !</div>
      </div>
    </div>

    <!-- QUÊTES -->
    <div class="page" id="page-quetes">
      <div class="sec">SYSTÈME DE QUÊTES</div>
      <div class="q-tabs">
        <div class="q-tab active" onclick="qTab(this,'qd')">🟢 Quotidien</div>
        <div class="q-tab" onclick="qTab(this,'qw')">🔵 Hebdo</div>
        <div class="q-tab" onclick="qTab(this,'qb')">🔴 Boss</div>
      </div>
      <div id="qd">
        <div class="qi" id="qq_bed" onclick="cq(this,'bed',5,{d:2})"><div class="qcheck">🛏️</div><div class="qinfo"><div class="qname">Faire son lit</div><div class="qsub">Commence la journée avec discipline</div></div><div class="qxp">+5 XP</div><div class="qtag td">DAILY</div></div>
        <div class="qi" id="qq_sport" onclick="cq(this,'sport',50,{f:3,e:3,s:2})"><div class="qcheck">🏃</div><div class="qinfo"><div class="qname">30 min de sport</div><div class="qsub">Cardio, muscu ou sport libre</div></div><div class="qxp">+50 XP</div><div class="qtag td">DAILY</div></div>
        <div class="qi" id="qq_shower" onclick="cq(this,'shower',20,{d:3,s:2})"><div class="qcheck">🚿</div><div class="qinfo"><div class="qname">Douche froide</div><div class="qsub">Mode guerrier activé 😈</div></div><div class="qxp">+20 XP</div><div class="qtag td">DAILY</div></div>
        <div class="qi" id="qq_water" onclick="cq(this,'water',15,{s:3})"><div class="qcheck">💧</div><div class="qinfo"><div class="qname">Hydratation</div><div class="qsub">2L d'eau aujourd'hui</div></div><div class="qxp">+15 XP</div><div class="qtag td">DAILY</div></div>
        <div class="qi" id="qq_food" onclick="cq(this,'food',10,{s:2,d:1})"><div class="qcheck">🥗</div><div class="qinfo"><div class="qname">Manger sain</div><div class="qsub">Pas de junk food</div></div><div class="qxp">+10 XP</div><div class="qtag td">DAILY</div></div>
        <div class="qi" id="qq_wake" onclick="cq(this,'wake',25,{d:4},true)"><div class="qcheck">🌅</div><div class="qinfo"><div class="qname">Lever tôt</div><div class="qsub">Avant 7h du matin</div></div><div class="qxp">+25 XP</div><div class="qtag td">DAILY</div></div>
      </div>
      <div id="qw" class="hidden">
        <div class="qi" id="qq_wsport" onclick="cq(this,'wsport',200,{f:8,e:8,s:5})"><div class="qcheck">🏋️</div><div class="qinfo"><div class="qname">3 séances de sport</div><div class="qsub">Cette semaine • <span id="wsc">0</span>/3</div></div><div class="qxp">+200 XP</div><div class="qtag tw">HEBDO</div></div>
        <div class="qi" id="qq_wfood" onclick="cq(this,'wfood',150,{d:6,s:6})"><div class="qcheck">🚫</div><div class="qinfo"><div class="qname">0 Junk Food</div><div class="qsub">7 jours sans fast food</div></div><div class="qxp">+150 XP</div><div class="qtag tw">HEBDO</div></div>
        <div class="qi" id="qq_wsleep" onclick="cq(this,'wsleep',175,{s:7,r:8})"><div class="qcheck">😴</div><div class="qinfo"><div class="qname">7h de sommeil / nuit</div><div class="qsub">Récupère comme un pro</div></div><div class="qxp">+175 XP</div><div class="qtag tw">HEBDO</div></div>
      </div>
      <div id="qb" class="hidden">
        <div class="boss-card" style="cursor:pointer" onclick="showToast('⚔️ Défi activé ! 7 jours !')">
          <div class="boss-lbl">⚔️ DONJON BOSS</div><div class="boss-nm">🔥 Défi 7 jours Discipline</div>
          <div class="boss-desc">7 jours consécutifs avec toutes les quêtes quotidiennes complétées.</div>
          <div style="display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:4px;"><span>PROGRESSION</span><span>0/7 jours</span></div>
          <div class="boss-bar-bg"><div class="boss-bar-fill" style="width:0%"></div></div>
          <div style="display:flex;align-items:center;gap:8px;font-size:13px;margin-top:12px;"><span class="boss-xp">+1000 XP</span><span style="color:var(--muted)">+ Titre "Guerrier Discipliné"</span></div>
        </div>
        <div class="boss-card" style="cursor:pointer" onclick="showToast('💪 Transformation lancée !')">
          <div class="boss-lbl">⚔️ MÉGA BOSS</div><div class="boss-nm">⚡ 30 Jours Transformation</div>
          <div class="boss-desc">30 jours de transformation physique totale.</div>
          <div style="display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:4px;"><span>PROGRESSION</span><span>0/30 jours</span></div>
          <div class="boss-bar-bg"><div class="boss-bar-fill" style="width:0%"></div></div>
          <div style="display:flex;align-items:center;gap:8px;font-size:13px;margin-top:12px;"><span class="boss-xp">+5000 XP</span><span style="color:var(--muted)">+ Rang "SHADOW MONARCH"</span></div>
        </div>
      </div>
    </div>

    <!-- SPORT -->
    <div class="page" id="page-sport">
      <div class="sec">AJOUTER UNE SÉANCE</div>
      <div class="wt-grid">
        <div class="wt sel" onclick="selWT(this,'muscu')"><div class="wt-ic">🏋️</div><div class="wt-nm">MUSCU</div></div>
        <div class="wt" onclick="selWT(this,'cardio')"><div class="wt-ic">🏃</div><div class="wt-nm">CARDIO</div></div>
        <div class="wt" onclick="selWT(this,'libre')"><div class="wt-ic">⚽</div><div class="wt-nm">SPORT LIBRE</div></div>
      </div>
      <div class="ig"><div class="il">Durée (minutes)</div><input type="number" class="inf" id="durIn" placeholder="ex: 45" min="1" oninput="updXPEst()"></div>
      <div class="ig"><div class="il">Exercices (optionnel)</div><input type="text" class="inf" id="exIn" placeholder="ex: Squat, Développé couché..."></div>
      <div class="ig"><div class="il">Intensité</div><div class="int-row"><div class="int-btn" onclick="selInt(this,1)">😌 Léger</div><div class="int-btn sel" onclick="selInt(this,1.5)">💪 Moyen</div><div class="int-btn" onclick="selInt(this,2.5)">🔥 Intense</div></div></div>
      <div class="x2-tog" onclick="togX2()"><div><div style="font-weight:700;font-size:15px;">⚡ Mode Entraînement Intense</div><div style="font-size:12px;color:var(--gold);">XP x2 !</div></div><div class="tog" id="x2Btn"></div></div>
      <div class="card" style="text-align:center;margin-bottom:14px;"><div style="font-size:11px;letter-spacing:2px;color:var(--muted);margin-bottom:4px;">XP ESTIMÉ</div><div style="font-family:'Orbitron',monospace;font-size:36px;font-weight:900;color:var(--violet2);" id="xpEst">+0 XP</div><div style="font-size:12px;color:var(--muted);" id="xpBk">Entre une durée pour voir</div></div>
      <button class="btn bp" onclick="submitWT()">⚡ VALIDER LA SÉANCE</button>
      <div class="sec" style="margin-top:20px;">HISTORIQUE</div>
      <div id="wtHist"><div class="empty"><div class="ei">🏋️</div><div class="et">Aucune séance encore.<br>Lance-toi !</div></div></div>
    </div>

    <!-- GOOGLE FIT -->
    <div class="page" id="page-fit">
      <div class="sec">GOOGLE FIT SYNC</div>

      <div id="fitDisc">
        <div class="fit-card">
          <div class="fit-hdr">
            <div class="fit-logo">🏃</div>
            <div><div class="fit-title">Google Fit</div><div class="fit-sub">Synchronise tes activités automatiquement</div></div>
          </div>
          <div class="scope-info"><strong>Données qui seront importées :</strong><br>Pas, activités physiques, calories brûlées, fréquence cardiaque, distance parcourue et durée d'entraînement.</div>
          <div class="perm-list">
            <div class="perm-item"><span>👟</span> Nombre de pas quotidiens<span class="perm-ck">✓</span></div>
            <div class="perm-item"><span>🏃</span> Activités physiques<span class="perm-ck">✓</span></div>
            <div class="perm-item"><span>🔥</span> Calories brûlées<span class="perm-ck">✓</span></div>
            <div class="perm-item"><span>❤️</span> Fréquence cardiaque<span class="perm-ck">✓</span></div>
            <div class="perm-item"><span>📍</span> Distance parcourue<span class="perm-ck">✓</span></div>
          </div>
          <button class="btn bg" onclick="connectFit()"><span id="fitBtnTxt">🔗 CONNECTER GOOGLE FIT</span></button>
          <div style="text-align:center;margin-top:10px;font-size:11px;color:var(--muted);">Connexion OAuth 2.0 sécurisée • Données jamais partagées</div>
        </div>
        <div class="card" style="margin-top:4px;">
          <div style="font-weight:700;color:var(--text);margin-bottom:10px;font-family:'Orbitron',monospace;font-size:12px;letter-spacing:2px;">COMMENT ÇA MARCHE ?</div>
          <div style="font-size:13px;color:var(--muted);line-height:1.9;">① Connecte Google Fit ci-dessus<br>② Autorise l'accès à tes données<br>③ Tes activités sont converties en XP<br>④ Chaque pas, chaque séance = progression 🔥</div>
        </div>
      </div>

      <div id="fitConn" class="hidden">
        <div class="fit-card">
          <div class="fit-hdr">
            <div class="fit-logo">🏃</div>
            <div style="flex:1"><div class="fit-title">Google Fit</div><div class="fit-status"><div class="fdot on" id="fDot"></div><span id="fStat" style="color:var(--green)">Connecté</span></div></div>
            <button class="btn bo btn-sm" onclick="syncFit()">🔄 SYNC</button>
          </div>
          <div class="fit-metrics">
            <div class="fit-m"><div class="fit-m-ic">👟</div><div class="fit-m-val" id="fSteps" style="color:var(--cyan)">0</div><div class="fit-m-nm">Pas</div></div>
            <div class="fit-m"><div class="fit-m-ic">🔥</div><div class="fit-m-val" id="fCals" style="color:var(--red)">0</div><div class="fit-m-nm">Calories</div></div>
            <div class="fit-m"><div class="fit-m-ic">⏱️</div><div class="fit-m-val" id="fMins" style="color:var(--violet2)">0</div><div class="fit-m-nm">Min actives</div></div>
            <div class="fit-m"><div class="fit-m-ic">📍</div><div class="fit-m-val" id="fDist" style="color:var(--green)">0</div><div class="fit-m-nm">km</div></div>
          </div>
          <div class="fit-xp-row">
            <div><div class="fit-xp-lbl">XP disponible à importer</div><div class="fit-xp-bk" id="fXpBk">En attente de sync...</div></div>
            <div class="fit-xp-amt" id="fPendXP">+0 XP</div>
          </div>
          <button class="btn bp" onclick="importFitXP()" id="fitImpBtn" disabled style="opacity:0.5">⚡ IMPORTER L'XP GOOGLE FIT</button>
        </div>
        <div class="sec" style="margin-top:8px;">ACTIVITÉS DÉTECTÉES</div>
        <div id="fitHist"><div class="empty"><div class="ei">📡</div><div class="et">Sync pour voir tes activités</div></div></div>
        <button class="btn bo" style="margin-top:8px;" onclick="discFit()">🔌 Déconnecter Google Fit</button>
      </div>
    </div>

    <!-- CLASSEMENT -->
    <div class="page" id="page-classement">
      <div class="sec">CLASSEMENT GLOBAL</div>
      <div class="lb-item"><div class="lb-rank r1">1</div><div class="lb-av">🦁</div><div style="flex:1"><div class="lb-nm">TITAN_OMEGA</div><div class="lb-rb">RANG SS • Niveau 47</div></div><div class="lb-xp">28,450 XP</div></div>
      <div class="lb-item"><div class="lb-rank r2">2</div><div class="lb-av">🐺</div><div style="flex:1"><div class="lb-nm">DARK_BLADE</div><div class="lb-rb">RANG S • Niveau 38</div></div><div class="lb-xp">21,890 XP</div></div>
      <div class="lb-item"><div class="lb-rank r3">3</div><div class="lb-av">🦊</div><div style="flex:1"><div class="lb-nm">PHANTOM_X</div><div class="lb-rb">RANG A • Niveau 29</div></div><div class="lb-xp">15,670 XP</div></div>
      <div style="text-align:center;padding:8px 0;color:var(--muted);font-size:12px;">• • •</div>
      <div class="lb-item me"><div class="lb-rank" style="color:var(--violet2)">—</div><div class="lb-av">🧑‍🦱</div><div style="flex:1"><div class="lb-nm">SHADOW_HUNTER <span style="color:var(--violet2);font-size:11px">(Toi)</span></div><div class="lb-rb">RANG <span id="myRk">E</span> • Niveau <span id="myRkLvl">1</span></div></div><div class="lb-xp" id="myRkXP">0 XP</div></div>
      <div class="sec" style="margin-top:20px;">DÉFIS AMIS</div>
      <div class="ch-card">
        <div class="ch-pl"><div class="ch-av">🧑‍🦱</div><div class="ch-nm">Toi</div><div class="ch-pr" id="myCh">0 séances</div></div>
        <div><div class="ch-vs">VS</div><div style="font-size:10px;color:var(--muted);text-align:center;">EN COURS</div></div>
        <div class="ch-pl"><div class="ch-av">🦊</div><div class="ch-nm">Alex_D</div><div class="ch-pr" style="color:var(--muted)">0 séances</div></div>
      </div>
    </div>

    <!-- PROFIL -->
    <div class="page" id="page-profil">
      <div class="prof-hdr">
        <div class="prof-av">🧑‍🦱</div>
        <div class="prof-name">SHADOW_HUNTER</div>
        <div class="prof-ttl" id="profTtl">⚔️ CHASSEUR NOVICE</div>
        <div style="display:flex;justify-content:center;gap:20px;margin-top:16px;">
          <div style="text-align:center"><div style="font-family:'Orbitron',monospace;font-size:22px;font-weight:900;color:var(--violet2)" id="profLvl">1</div><div style="font-size:11px;color:var(--muted)">NIVEAU</div></div>
          <div style="width:1px;background:var(--border)"></div>
          <div style="text-align:center"><div style="font-family:'Orbitron',monospace;font-size:22px;font-weight:900;color:var(--gold)" id="profStr">0</div><div style="font-size:11px;color:var(--muted)">STREAK</div></div>
          <div style="width:1px;background:var(--border)"></div>
          <div style="text-align:center"><div style="font-family:'Orbitron',monospace;font-size:22px;font-weight:900;color:var(--green)" id="profSes">0</div><div style="font-size:11px;color:var(--muted)">SÉANCES</div></div>
        </div>
      </div>
      <div class="sec">STATISTIQUES COMPLÈTES</div>
      <div class="card">
        <div style="display:flex;flex-direction:column;gap:14px;">
          <div><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span>💪 <span style="color:var(--muted)">Force</span></span><span style="font-family:'Orbitron',monospace;font-size:13px;color:#ef4444" id="pf-f">0/100</span></div><div class="sbar-bg"><div class="sbar f" id="pb-f" style="width:0%"></div></div></div>
          <div><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span>⚡ <span style="color:var(--muted)">Endurance</span></span><span style="font-family:'Orbitron',monospace;font-size:13px;color:#3b82f6" id="pf-e">0/100</span></div><div class="sbar-bg"><div class="sbar e" id="pb-e" style="width:0%"></div></div></div>
          <div><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span>🧠 <span style="color:var(--muted)">Discipline</span></span><span style="font-family:'Orbitron',monospace;font-size:13px;color:#a855f7" id="pf-d">0/100</span></div><div class="sbar-bg"><div class="sbar d" id="pb-d" style="width:0%"></div></div></div>
          <div><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span>❤️ <span style="color:var(--muted)">Santé</span></span><span style="font-family:'Orbitron',monospace;font-size:13px;color:#10b981" id="pf-s">0/100</span></div><div class="sbar-bg"><div class="sbar s" id="pb-s" style="width:0%"></div></div></div>
          <div><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span>💤 <span style="color:var(--muted)">Récupération</span></span><span style="font-family:'Orbitron',monospace;font-size:13px;color:#06b6d4" id="pf-r">0/100</span></div><div class="sbar-bg"><div class="sbar r" id="pb-r" style="width:0%"></div></div></div>
        </div>
      </div>
      <div class="sec">SUCCÈS</div>
      <div class="ach-grid">
        <div class="ach" id="a_str10"><div class="ach-ic" style="filter:grayscale(1)">🔥</div><div class="ach-nm">10 Streak</div></div>
        <div class="ach" id="a_ses10"><div class="ach-ic" style="filter:grayscale(1)">💪</div><div class="ach-nm">10 Séances</div></div>
        <div class="ach" id="a_wake"><div class="ach-ic" style="filter:grayscale(1)">🌅</div><div class="ach-nm">Lève-tôt</div></div>
        <div class="ach" id="a_fit"><div class="ach-ic" style="filter:grayscale(1)">🏃</div><div class="ach-nm">Fit Sync</div></div>
        <div class="ach" id="a_rkC"><div class="ach-ic" style="filter:grayscale(1)">⚡</div><div class="ach-nm">Rang C</div></div>
        <div class="ach" id="a_rkS"><div class="ach-ic" style="filter:grayscale(1)">🦁</div><div class="ach-nm">Rang S</div></div>
        <div class="ach" id="a_rkSS"><div class="ach-ic" style="filter:grayscale(1)">👑</div><div class="ach-nm">Rang SS</div></div>
        <div class="ach" id="a_rkSSS"><div class="ach-ic" style="filter:grayscale(1)">🌟</div><div class="ach-nm">SSS Rang</div></div>
      </div>
    </div>

  </div><!-- /main -->

  <nav class="nav">
    <div class="nav-item active" onclick="goPage('home',this)"><div class="icon">🏠</div><span>Accueil</span></div>
    <div class="nav-item" onclick="goPage('quetes',this)"><div class="icon">📋</div><span>Quêtes</span></div>
    <div class="nav-item" onclick="goPage('sport',this)"><div class="icon">⚡</div><span>Sport</span></div>
    <div class="nav-item" onclick="goPage('fit',this)" style="position:relative">
      <div class="icon" id="fitNavIc">🏃</div><span>Fit</span>
      <div id="fitNavDot" class="hidden" style="position:absolute;top:5px;right:calc(50% - 14px);width:8px;height:8px;border-radius:50%;background:var(--green);box-shadow:0 0 6px var(--green);"></div>
    </div>
    <div class="nav-item" onclick="goPage('classement',this)"><div class="icon">🏆</div><span>Rang</span></div>
    <div class="nav-item" onclick="goPage('profil',this)"><div class="icon">👤</div><span>Profil</span></div>
  </nav>
</div>

<script>
// ══════════════════════════════════════
//  STATE — tout à 0
// ══════════════════════════════════════
const BASE_XP = 100;
let G = {
  lvl:1, cxp:0, xpNext:BASE_XP, txp:0, streak:0,
  stats:{f:0,e:0,d:0,s:0,r:0},
  sessions:0, weekSessions:0,
  weekXP:[0,0,0,0,0,0,0], dayXP:0,
  quests:{}, wtHistory:[], chestDone:false,
  x2:false, intMult:1.5, wtType:'muscu',
  fitConn:false, fitToken:null, fitData:null,
  fitPendXP:0, fitImported:false, fitHistory:[]
};

// ══════════════════════════════════════
//  RANKS
// ══════════════════════════════════════
function rank(l){return l<5?'E':l<10?'D':l<15?'C':l<22?'B':l<30?'A':l<40?'S':l<50?'SS':'SSS';}
function rankTitle(r){return{E:'Chasseur Novice',D:'Chasseur Rang D',C:'Chasseur Rang C',B:'Chasseur Expérimenté',A:'Chasseur Élite',S:'Chasseur Légendaire',SS:'Ombre Puissante',SSS:'Shadow Monarch'}[r]||'Chasseur';}

// ══════════════════════════════════════
//  PARTICLES
// ══════════════════════════════════════
const cv=document.getElementById('particles'),cx=cv.getContext('2d');
cv.width=window.innerWidth;cv.height=window.innerHeight;
const pts=[];
for(let i=0;i<60;i++)pts.push({x:Math.random()*cv.width,y:Math.random()*cv.height,r:Math.random()*1.5+.5,vx:(Math.random()-.5)*.3,vy:(Math.random()-.5)*.3,a:Math.random()*.5+.1,c:Math.random()>.5?'124,58,237':'6,182,212'});
(function dp(){cx.clearRect(0,0,cv.width,cv.height);pts.forEach(p=>{cx.beginPath();cx.arc(p.x,p.y,p.r,0,Math.PI*2);cx.fillStyle=`rgba(${p.c},${p.a})`;cx.fill();p.x+=p.vx;p.y+=p.vy;if(p.x<0||p.x>cv.width)p.vx*=-1;if(p.y<0||p.y>cv.height)p.vy*=-1;});requestAnimationFrame(dp);})();

// ══════════════════════════════════════
//  NAV
// ══════════════════════════════════════
function goPage(n,el){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(x=>x.classList.remove('active'));
  document.getElementById('page-'+n).classList.add('active');
  el.classList.add('active');
}

// ══════════════════════════════════════
//  XP & LEVEL
// ══════════════════════════════════════
function gainXP(amt){
  G.cxp+=amt; G.txp+=amt; G.dayXP+=amt;
  const di=new Date().getDay();
  G.weekXP[di===0?6:di-1]=(G.weekXP[di===0?6:di-1]||0)+amt;
  while(G.cxp>=G.xpNext){G.cxp-=G.xpNext;G.lvl++;G.xpNext=Math.floor(BASE_XP*Math.pow(1.25,G.lvl-1));setTimeout(()=>showLU(G.lvl),400);}
  render(); checkAch();
}
function gainStats(d){for(const[k,v]of Object.entries(d)){if(G.stats[k]!==undefined)G.stats[k]=Math.min(100,G.stats[k]+v);}render();}

// ══════════════════════════════════════
//  RENDER
// ══════════════════════════════════════
function render(){
  const pct=(G.cxp/G.xpNext)*100;
  document.getElementById('xpBar').style.width=pct+'%';
  document.getElementById('cXP').textContent=G.cxp.toLocaleString();
  document.getElementById('xpNext').textContent=G.xpNext.toLocaleString();
  document.getElementById('tXP').textContent=G.txp.toLocaleString();
  document.getElementById('pLvl').textContent=G.lvl;
  document.getElementById('nLvl').textContent=G.lvl+1;
  document.getElementById('sVal').textContent=G.streak;
  const r=rank(G.lvl);
  document.getElementById('rBadge').textContent='RANG '+r;
  // home stats
  const sk=['f','e','d','s'];const snm=['force','endurance','discipline','santé'];
  sk.forEach(k=>{const v=G.stats[k]||0;const sv=document.getElementById('sv-'+k);if(sv)sv.textContent=v;const sb=document.getElementById('sb-'+k);if(sb)sb.style.width=v+'%';});
  // profile stats
  ['f','e','d','s','r'].forEach(k=>{const v=G.stats[k]||0;const pf=document.getElementById('pf-'+k);if(pf)pf.textContent=v+'/100';const pb=document.getElementById('pb-'+k);if(pb)pb.style.width=v+'%';});
  // profile header
  document.getElementById('profLvl').textContent=G.lvl;
  document.getElementById('profStr').textContent=G.streak;
  document.getElementById('profSes').textContent=G.sessions;
  document.getElementById('profTtl').textContent='⚔️ '+rankTitle(r).toUpperCase();
  // leaderboard me
  document.getElementById('myRk').textContent=r;
  document.getElementById('myRkLvl').textContent=G.lvl;
  document.getElementById('myRkXP').textContent=G.txp.toLocaleString()+' XP';
  document.getElementById('myCh').textContent=G.weekSessions+' séances';
  // week graph
  const wt=G.weekXP.reduce((a,b)=>a+b,0);
  document.getElementById('wXP').textContent='+'+wt+' XP';
  document.getElementById('dXP').textContent='+'+G.dayXP;
  const mx=Math.max(...G.weekXP,1);
  document.querySelectorAll('.mini-g .bar').forEach((b,i)=>b.style.height=Math.max(2,(G.weekXP[i]/mx)*100)+'%');
  // boss bar
  const bp=Math.min(G.weekSessions,3);
  document.getElementById('bossP').textContent=bp+'/3 séances';
  document.getElementById('bossBar').style.width=(bp/3*100)+'%';
  document.getElementById('wsc').textContent=G.weekSessions;
}

// ══════════════════════════════════════
//  QUESTS
// ══════════════════════════════════════
function cq(el,id,xp,sd,isStreak){
  if(G.quests[id])return;
  G.quests[id]=true;
  if(isStreak)G.streak++;
  el.classList.add('done');
  const ck=el.querySelector('.qcheck');
  ck.textContent='✓';ck.style.background='rgba(16,185,129,0.2)';ck.style.borderColor='var(--green)';ck.style.color='var(--green)';
  gainXP(xp); gainStats(sd||{});
  showToast('+'+xp+' XP GAGNÉ ! 🎉');
}
function qTab(el,t){
  document.querySelectorAll('.q-tab').forEach(x=>x.classList.remove('active'));el.classList.add('active');
  ['qd','qw','qb'].forEach(x=>{const d=document.getElementById(x);if(d)d.classList.toggle('hidden',x!==t);});
}

// ══════════════════════════════════════
//  WORKOUT
// ══════════════════════════════════════
function selWT(el,t){document.querySelectorAll('.wt').forEach(x=>x.classList.remove('sel'));el.classList.add('sel');G.wtType=t;updXPEst();}
function selInt(el,m){document.querySelectorAll('.int-btn').forEach(x=>x.classList.remove('sel'));el.classList.add('sel');G.intMult=m;updXPEst();}
function togX2(){G.x2=!G.x2;document.getElementById('x2Btn').classList.toggle('on',G.x2);updXPEst();}
function updXPEst(){
  const dur=parseInt(document.getElementById('durIn').value)||0;
  if(!dur){document.getElementById('xpEst').textContent='+0 XP';document.getElementById('xpBk').textContent='Entre une durée';return;}
  let b=Math.round(dur*1.5*G.intMult);if(G.x2)b*=2;
  document.getElementById('xpEst').textContent='+'+b+' XP';
  document.getElementById('xpBk').textContent=dur+' min × '+G.intMult+(G.x2?' × 2':'');
}
function submitWT(){
  const dur=parseInt(document.getElementById('durIn').value);
  if(!dur||dur<1){showToast('⚠️ Entre une durée valide !');return;}
  const ex=document.getElementById('exIn').value||'—';
  let b=Math.round(dur*1.5*G.intMult);if(G.x2)b*=2;
  const ic={muscu:'🏋️',cardio:'🏃',libre:'⚽'}[G.wtType];
  const nm={muscu:'Musculation',cardio:'Cardio',libre:'Sport Libre'}[G.wtType];
  const sd={muscu:{f:Math.ceil(dur/10),e:Math.ceil(dur/15)},cardio:{e:Math.ceil(dur/8),s:Math.ceil(dur/12)},libre:{f:Math.ceil(dur/12),e:Math.ceil(dur/10),s:Math.ceil(dur/15)}}[G.wtType];
  G.wtHistory.unshift({type:G.wtType,nm,ic,dur,ex,xp:b,dt:new Date().toLocaleDateString('fr-FR')});
  G.sessions++;G.weekSessions++;
  gainXP(b);gainStats(sd||{});
  showToast('⚡ SÉANCE VALIDÉE ! +'+b+' XP');
  document.getElementById('durIn').value='';document.getElementById('exIn').value='';
  renderWTHist();
  if(G.weekSessions>=3&&!G.quests['wsport']){setTimeout(()=>{showToast('🏆 3 séances ! +200 XP bonus');gainXP(200);G.quests['wsport']=true;},1000);}
}
function renderWTHist(){
  const c=document.getElementById('wtHist');
  if(!G.wtHistory.length){c.innerHTML='<div class="empty"><div class="ei">🏋️</div><div class="et">Aucune séance encore.<br>Lance-toi !</div></div>';return;}
  c.innerHTML=G.wtHistory.slice(0,5).map(e=>`<div class="wh-item"><div class="wh-ic">${e.ic}</div><div class="wh-info"><div class="wh-nm">${e.nm}</div><div class="wh-sub">${e.dur} min • ${e.ex} • ${e.dt}</div></div><div class="wh-xp">+${e.xp} XP</div></div>`).join('');
}

// ══════════════════════════════════════
//  GOOGLE FIT
// ══════════════════════════════════════
// ⚠️ Remplace ce CLIENT_ID par ton vrai Client ID Google Cloud Console
const GF_CLIENT_ID = 'TON_CLIENT_ID.apps.googleusercontent.com';
const GF_SCOPES = [
  'https://www.googleapis.com/auth/fitness.activity.read',
  'https://www.googleapis.com/auth/fitness.body.read',
  'https://www.googleapis.com/auth/fitness.heart_rate.read',
  'https://www.googleapis.com/auth/fitness.location.read'
].join(' ');

function connectFit(){
  const btn=document.getElementById('fitBtnTxt');
  btn.textContent='🔄 Connexion en cours...';

  // MODE DEMO si pas de vrai Client ID
  if(GF_CLIENT_ID.startsWith('TON_')){
    showToast('🔗 Connexion Google Fit (demo)...');
    setTimeout(()=>{G.fitToken='DEMO';onFitConn();},1500);
    return;
  }

  // OAuth 2.0 PKCE popup réel
  const params=new URLSearchParams({
    client_id: GF_CLIENT_ID,
    redirect_uri: window.location.origin+window.location.pathname,
    response_type: 'token',
    scope: GF_SCOPES,
    include_granted_scopes: 'true'
  });
  const popup=window.open('https://accounts.google.com/o/oauth2/v2/auth?'+params,'GFit','width=520,height=620,left=200,top=80');
  const ti=setInterval(()=>{
    try{
      if(popup.closed){clearInterval(ti);btn.textContent='🔗 CONNECTER GOOGLE FIT';return;}
      const url=popup.location.href;
      if(url.includes('access_token=')){
        clearInterval(ti);popup.close();
        G.fitToken=new URLSearchParams(url.split('#')[1]).get('access_token');
        onFitConn();
      }
    }catch(e){}
  },600);
}

function onFitConn(){
  G.fitConn=true;
  document.getElementById('fitDisc').classList.add('hidden');
  document.getElementById('fitConn').classList.remove('hidden');
  document.getElementById('fitNavDot').classList.remove('hidden');
  showToast('✅ Google Fit connecté !');
  syncFit();
}

async function syncFit(){
  const dot=document.getElementById('fDot');const st=document.getElementById('fStat');
  dot.className='fdot sync';st.textContent='Synchronisation...';st.style.color='var(--blue)';

  try{
    if(G.fitToken==='DEMO'){
      await new Promise(r=>setTimeout(r,1800));
      // Données simulées réalistes basées sur une journée active
      G.fitData={
        steps:Math.floor(Math.random()*7000)+1500,
        cals:Math.floor(Math.random()*250)+80,
        mins:Math.floor(Math.random()*40)+5,
        dist:parseFloat((Math.random()*4+0.3).toFixed(1)),
        activities:[
          {nm:'Course à pied',dur:30,cals:240,ic:'🏃',imp:false},
          {nm:'Marche active',dur:20,cals:90,ic:'🚶',imp:false},
        ]
      };
    } else {
      // VRAI appel Google Fitness REST API
      const now=Date.now();
      const sod=new Date();sod.setHours(0,0,0,0);
      const body={
        aggregateBy:[
          {dataTypeName:'com.google.step_count.delta'},
          {dataTypeName:'com.google.calories.expended'},
          {dataTypeName:'com.google.active_minutes'},
          {dataTypeName:'com.google.distance.delta'}
        ],
        bucketByTime:{durationMillis:86400000},
        startTimeMillis:sod.getTime(),
        endTimeMillis:now
      };
      const res=await fetch('https://www.googleapis.com/fitness/v1/users/me/dataset:aggregate',{
        method:'POST',
        headers:{'Authorization':'Bearer '+G.fitToken,'Content-Type':'application/json'},
        body:JSON.stringify(body)
      });
      const data=await res.json();
      if(data.error)throw new Error(data.error.message);
      const bk=data.bucket?.[0]||{};
      const gv=(b,i)=>{const pt=b.dataset?.[i]?.point?.[0];return pt?.value?.[0]?.intVal||pt?.value?.[0]?.fpVal||0;};
      G.fitData={
        steps:Math.round(gv(bk,0)),
        cals:Math.round(gv(bk,1)),
        mins:Math.round(gv(bk,2)),
        dist:parseFloat((gv(bk,3)/1000).toFixed(1)),
        activities:[]
      };
    }
    renderFitData();
    dot.className='fdot on';
    const t=new Date().toLocaleTimeString('fr-FR',{hour:'2-digit',minute:'2-digit'});
    st.textContent='Synchronisé · '+t;st.style.color='var(--green)';
    showToast('✅ Google Fit synchronisé !');
  }catch(err){
    dot.className='fdot off';st.textContent='Erreur de sync';st.style.color='var(--red)';
    showToast('❌ Erreur : '+err.message);
  }
}

function renderFitData(){
  if(!G.fitData)return;
  const d=G.fitData;
  document.getElementById('fSteps').textContent=d.steps.toLocaleString();
  document.getElementById('fCals').textContent=d.cals;
  document.getElementById('fMins').textContent=d.mins;
  document.getElementById('fDist').textContent=d.dist;
  // Calcul XP
  let xp=0;
  xp+=Math.floor(d.steps/1000)*5;       // 5 XP / 1000 pas
  xp+=Math.floor(d.mins/10)*8;          // 8 XP / 10 min actives
  xp+=Math.floor(d.cals/50)*3;          // 3 XP / 50 cal
  if(parseFloat(d.dist)>1)xp+=10;       // bonus 1km
  G.fitPendXP=xp;
  G.fitImported=false;
  document.getElementById('fPendXP').textContent='+'+xp+' XP';
  document.getElementById('fXpBk').textContent=
    Math.floor(d.steps/1000)*5+' (pas) + '+Math.floor(d.mins/10)*8+' (min) + '+Math.floor(d.cals/50)*3+' (cal)';
  const btn=document.getElementById('fitImpBtn');
  if(xp>0){btn.disabled=false;btn.style.opacity='1';}
  if(d.activities&&d.activities.length){G.fitHistory=d.activities;renderFitHist();}
}

function renderFitHist(){
  const c=document.getElementById('fitHist');
  if(!G.fitHistory.length){c.innerHTML='<div class="empty"><div class="ei">📡</div><div class="et">Sync pour voir tes activités</div></div>';return;}
  c.innerHTML=G.fitHistory.map(a=>{
    const xp=Math.round(a.dur*1.5);
    return `<div class="fit-act${a.imp?' imp':''}"><div class="fit-act-ic">${a.ic||'🏃'}</div><div class="fit-act-info"><div class="fit-act-nm">${a.nm}</div><div class="fit-act-sub">${a.dur} min • ${a.cals} kcal${a.imp?' • ✅ Importé':''}</div></div><div class="fit-act-xp">+${xp} XP</div></div>`;
  }).join('');
}

function importFitXP(){
  if(G.fitImported||!G.fitPendXP)return;
  G.fitImported=true;
  const xp=G.fitPendXP;
  gainXP(xp);
  gainStats({e:Math.ceil((G.fitData.mins||0)/10),s:Math.ceil((G.fitData.steps||0)/2000)});
  G.fitHistory.forEach(a=>a.imp=true);
  renderFitHist();
  const btn=document.getElementById('fitImpBtn');
  btn.disabled=true;btn.style.opacity='0.5';btn.textContent='✅ XP IMPORTÉ !';
  document.getElementById('fPendXP').textContent='+0 XP';
  G.fitPendXP=0;
  showToast('🏃 +'+xp+' XP depuis Google Fit !');
  unlockAch('a_fit');
}

function discFit(){
  G.fitConn=false;G.fitToken=null;G.fitData=null;G.fitImported=false;
  document.getElementById('fitConn').classList.add('hidden');
  document.getElementById('fitDisc').classList.remove('hidden');
  document.getElementById('fitBtnTxt').textContent='🔗 CONNECTER GOOGLE FIT';
  document.getElementById('fitNavDot').classList.add('hidden');
  document.getElementById('fitImpBtn').textContent="⚡ IMPORTER L'XP GOOGLE FIT";
  showToast('🔌 Google Fit déconnecté');
}

// ══════════════════════════════════════
//  CHEST
// ══════════════════════════════════════
function openChest(){
  if(G.chestDone)return;
  G.chestDone=true;
  const xp=[30,50,75,100][Math.floor(Math.random()*4)];
  gainXP(xp);
  showToast('📦 COFFRE : +'+xp+' XP 🎉');
  document.getElementById('chestIc').textContent='🪹';
  document.getElementById('chestTmr').textContent='Revient demain !';
  document.getElementById('chestCard').style.opacity='0.6';
  document.getElementById('chestCard').style.cursor='default';
}

// ══════════════════════════════════════
//  LEVEL UP MODAL
// ══════════════════════════════════════
function showLU(lvl){
  document.getElementById('luLvl').textContent=lvl;
  document.getElementById('luSub').innerHTML='Tu es maintenant <strong>'+rankTitle(rank(lvl))+'</strong>';
  mkConf();document.getElementById('luModal').classList.add('show');
}
function closeLU(){document.getElementById('luModal').classList.remove('show');}
function mkConf(){
  const c=document.getElementById('conf');c.innerHTML='';
  const cols=['#7c3aed','#a855f7','#06b6d4','#f59e0b','#10b981','#3b82f6'];
  for(let i=0;i<30;i++){const el=document.createElement('div');el.className='cp';el.style.cssText=`left:${Math.random()*100}%;top:-10px;background:${cols[Math.floor(Math.random()*cols.length)]};animation-delay:${Math.random()*.5}s;animation-duration:${1.5+Math.random()}s;`;c.appendChild(el);}
}

// ══════════════════════════════════════
//  ACHIEVEMENTS
// ══════════════════════════════════════
function checkAch(){
  const r=rank(G.lvl);
  if(G.streak>=10)unlockAch('a_str10');
  if(G.sessions>=10)unlockAch('a_ses10');
  if(G.quests['wake'])unlockAch('a_wake');
  if(['C','B','A','S','SS','SSS'].includes(r))unlockAch('a_rkC');
  if(['S','SS','SSS'].includes(r))unlockAch('a_rkS');
  if(['SS','SSS'].includes(r))unlockAch('a_rkSS');
  if(r==='SSS')unlockAch('a_rkSSS');
}
function unlockAch(id){
  const el=document.getElementById(id);
  if(el&&!el.classList.contains('ul')){el.classList.add('ul');el.querySelector('.ach-ic').style.filter='';}
}

// ══════════════════════════════════════
//  TOAST
// ══════════════════════════════════════
let toastT;
function showToast(msg){
  const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');
  clearTimeout(toastT);toastT=setTimeout(()=>t.classList.remove('show'),2500);
}

// INIT
render();renderWTHist();updXPEst();
</script>
</body>
</html>
