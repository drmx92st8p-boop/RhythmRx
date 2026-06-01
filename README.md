# RhythmRx
Cyclical bioidentical hormone therapy tracking app
}
function selAdj(t,el){
  const g=t==='e'?'#adjEG':'#adjPG';
  document.querySelectorAll(g+' .pill').forEach(p=>p.classList.remove('on'));
  el.classList.add('on');
  if(t==='e')adjE=parseInt(el.dataset.lv);else adjP=parseInt(el.dataset.lv);
}
async function applyAdj(){
  S.eLevel=adjE;S.eAdj={level:adjE,fromMs:new Date(0).toISOString()};
  S.pLevel=adjP;S.pAdj={level:adjP,fromMs:new Date(0).toISOString()};
  await saveSettings();updSettings();
  document.getElementById('adjMsg').textContent='✓ Applied';document.getElementById('adjMsg').className='ok';
  setTimeout(()=>{closeOL('adjOL');render();},700);
}
async function resetAdj(){
  S.eAdj=null;S.pAdj=null;
  await saveSettings();updSettings();render();closeOL('settingsOL');
}

// ── SETTINGS ──
function updSettings(){
  document.getElementById('setCalLbl').textContent=S.calType==='lunar'?'🌕 Lunar Calendar':'📅 28-Day Calendar';
  document.getElementById('setELbl').textContent=lvLbl('e',S.eLevel);
  document.getElementById('setPLbl').textContent=lvLbl('p',S.pLevel);
  const adjEl=document.getElementById('setAdjLbl');
  if(adjEl)adjEl.textContent=(S.eAdj||S.pAdj)?'Active — tap Reset to clear':'None active';
  if(currentUser)document.getElementById('setEmail').textContent=currentUser.email;
}

// ── OVERLAY HELPERS ──
function openOL(id){document.getElementById(id).classList.add('vis');}
function closeOL(id){document.getElementById(id).classList.remove('vis');}
function bgClose(e,id){if(e.target.classList.contains('overlay'))closeOL(id);}

// ── INIT ──
sb.auth.onAuthStateChange(async (event,session)=>{
  if(session&&session.user){
    currentUser=session.user;
    await loadSettings();
    await loadLogs();
    document.getElementById('authScreen').classList.remove('active');
    if(S.setup){showMain();}
    else{document.getElementById('setupScreen').classList.add('active');}
  } else {
    currentUser=null;
    document.getElementById('authScreen').classList.add('active');
    document.getElementById('mainScreen').classList.remove('active');
    document.getElementById('setupScreen').classList.remove('active');
  }
});
</script>
</body>
</html>
