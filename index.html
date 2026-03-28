<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>GMF App — Guess My Fart</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
<style>
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
  html,body{height:100%;-webkit-font-smoothing:antialiased;font-family:'Open Sans',sans-serif;background:#F5F3FA;overflow:hidden;}
  #root{height:100vh;}
  @keyframes barPulse{0%,100%{height:5px;opacity:.25;}50%{height:42px;opacity:.9;}}
  @keyframes blink{0%,100%{opacity:1;}50%{opacity:0;}}
  @keyframes slideUp{from{transform:translateY(100%);}to{transform:translateY(0);}}
  ::-webkit-scrollbar{display:none;}
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useRef, useEffect, useCallback } = React;

const C = {
  purple:"#7B5EA7", purpleLight:"#9B7EC8", purpleDark:"#5D3F8A",
  purpleBg:"#F3F0F9", purpleBorder:"rgba(123,94,167,0.18)",
  white:"#ffffff", bg:"#F5F3FA", border:"rgba(60,60,67,0.10)",
  text:"#1a1a2e", text2:"#4a4a6a", text3:"rgba(74,74,106,0.6)", text4:"rgba(74,74,106,0.35)",
  green:"#22c55e",
};

const fmt = s => { const n=Math.floor(s); return `${Math.floor(n/60)}:${String(n%60).padStart(2,'0')}`; };
const formatDate = d => {
  const now=new Date(), today=now.toDateString()===d.toDateString();
  return today ? d.toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'})
    : d.toLocaleDateString('en-US',{month:'short',day:'numeric'})+' · '+d.toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
};

function drawWaveform(canvas,buffer,color){
  if(!canvas||!buffer)return;
  const ctx=canvas.getContext('2d'),W=canvas.width,H=canvas.height;
  const data=buffer.getChannelData(0),step=Math.ceil(data.length/W),amp=H/2;
  ctx.clearRect(0,0,W,H); ctx.strokeStyle=color||C.purple; ctx.lineWidth=1.5; ctx.beginPath();
  for(let i=0;i<W;i++){
    let mn=1,mx=-1;
    for(let j=0;j<step;j++){const v=data[i*step+j]||0;if(v<mn)mn=v;if(v>mx)mx=v;}
    ctx.moveTo(i+.5,amp+mn*amp*.82); ctx.lineTo(i+.5,amp+mx*amp*.82);
  }
  ctx.stroke();
}

function extractFeatures(buf){
  const d=buf.getChannelData(0),len=d.length;
  let ss=0; for(let i=0;i<len;i++) ss+=d[i]*d[i];
  const rms=Math.sqrt(ss/len);
  let zcr=0; for(let i=1;i<len;i++) if((d[i]>=0)!==(d[i-1]>=0)) zcr++; zcr/=len;
  const fs=2048,fr=Math.floor(len/fs); let cS=0;
  for(let f=0;f<fr;f++){let w=0,t=0;for(let k=0;k<fs;k++){const m=Math.abs(d[f*fs+k]);w+=k*m;t+=m;}cS+=t?w/t:0;}
  const seg=Math.floor(len/10),env=[];
  for(let s=0;s<10;s++){let e=0;for(let k=0;k<seg;k++)e+=Math.abs(d[s*seg+k]||0);env.push(e/seg);}
  return{dur:buf.duration,rms,zcr,cent:cS/(fr||1),env};
}

function compareAudio(orig,guess){
  const a=extractFeatures(orig),b=extractFeatures(guess);
  const dur=Math.min(a.dur,b.dur)/Math.max(a.dur,b.dur);
  const rms=Math.max(0,1-Math.abs(a.rms-b.rms)/(Math.max(a.rms,b.rms)||.001));
  const zcr=Math.max(0,1-Math.abs(a.zcr-b.zcr)/(Math.max(a.zcr,b.zcr)||.001)*2);
  const cen=Math.max(0,1-Math.abs(a.cent-b.cent)/(Math.max(a.cent,b.cent)||.001));
  let env=0; const aM=Math.max(...a.env)||1,bM=Math.max(...b.env)||1;
  for(let i=0;i<10;i++) env+=1-Math.min(1,Math.abs(a.env[i]/aM-b.env[i]/bM)); env/=10;
  return Math.round((dur*.25+rms*.15+zcr*.25+cen*.15+env*.20)*100);
}

// ── Waveform canvases ──
function MiniWave({buffer}){
  const ref=useRef(null);
  useEffect(()=>{ if(buffer&&ref.current) drawWaveform(ref.current,buffer,C.purpleLight); },[buffer]);
  return <canvas ref={ref} width={36} height={24} style={{display:'block',width:36,height:24}}/>;
}
function BigWave({buffer,color,height=64}){
  const ref=useRef(null);
  useEffect(()=>{ if(buffer&&ref.current) drawWaveform(ref.current,buffer,color||C.purple); },[buffer,color]);
  return <canvas ref={ref} width={600} height={height} style={{width:'100%',height,display:'block',borderRadius:10,background:C.purpleBg}}/>;
}

// ── Record Button ──
function RecordButton({recording,onToggle,size='large'}){
  const os=size==='large'?120:80, is=size==='large'?88:60, sq=size==='large'?34:24, bw=size==='large'?4:3;
  return (
    <button onClick={onToggle} style={{
      width:os,height:os,borderRadius:'50%',border:`${bw}px solid ${C.purple}`,
      background:'white',display:'flex',alignItems:'center',justifyContent:'center',
      cursor:'pointer',padding:0,WebkitTapHighlightColor:'transparent',flexShrink:0,
      boxShadow:`0 4px 20px rgba(123,94,167,0.25)`,
    }}>
      <div style={{
        width:recording?sq:is, height:recording?sq:is,
        borderRadius:recording?(size==='large'?12:8):'50%',
        background:C.purple, transition:'all .28s cubic-bezier(.4,0,.2,1)',
      }}/>
    </button>
  );
}

// ── NavBar ──
function NavBar({title,onBack,rightSlot}){
  return (
    <div style={{
      background:'rgba(245,243,250,0.96)',backdropFilter:'blur(20px)',
      borderBottom:`1px solid ${C.purpleBorder}`,padding:'0 16px',
      display:'flex',alignItems:'flex-end',height:88,paddingBottom:12,flexShrink:0,gap:8,
    }}>
      {onBack ? (
        <button onClick={onBack} style={{
          background:'none',border:'none',color:C.purple,fontFamily:"'Open Sans',sans-serif",
          fontSize:17,cursor:'pointer',display:'flex',alignItems:'center',gap:5,minWidth:80,padding:0,
        }}>
          <svg width="10" height="17" viewBox="0 0 10 17" fill="none">
            <path d="M8.5 1.5L1.5 8.5L8.5 15.5" stroke={C.purple} strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round"/>
          </svg>
          GMF App
        </button>
      ) : (
        <div style={{minWidth:80,display:'flex',alignItems:'center',gap:8}}>
          <div style={{width:28,height:28,borderRadius:8,background:`linear-gradient(135deg,${C.purple},${C.purpleLight})`,display:'flex',alignItems:'center',justifyContent:'center'}}>
            <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
              <path d="M7 1C4 1 2 3 2 5.5c0 2 1.5 3.5 3 4.5L7 13l2-3c1.5-1 3-2.5 3-4.5C12 3 10 1 7 1z" fill="white"/>
            </svg>
          </div>
          <span style={{fontSize:15,fontWeight:700,color:C.purple,letterSpacing:'-0.01em'}}>GMF App</span>
        </div>
      )}
      <div style={{flex:1,textAlign:'center',fontSize:17,fontWeight:600,color:C.text,letterSpacing:'-0.02em'}}>{title}</div>
      <div style={{minWidth:80,display:'flex',justifyContent:'flex-end'}}>{rightSlot}</div>
    </div>
  );
}

// ── Share Modal ──
function ShareModal({sub,onClose}){
  const [copied,setCopied]=useState(false);
  const code=`GMF-${String(sub.id).slice(-6).toUpperCase()}`;
  const msg=`Guess My Fart!\n\nEnter code: ${code} at watchgabe.github.io/gmf\nCan you match this fart?`;
  const copy=()=>{ navigator.clipboard.writeText(msg).catch(()=>{}); setCopied(true); setTimeout(()=>setCopied(false),2000); };
  return (
    <div onClick={onClose} style={{position:'fixed',inset:0,zIndex:100,background:'rgba(26,26,46,0.5)',display:'flex',alignItems:'flex-end',justifyContent:'center'}}>
      <div onClick={e=>e.stopPropagation()} style={{background:C.white,borderRadius:'20px 20px 0 0',padding:'28px 24px 48px',width:'100%',maxWidth:480,boxShadow:'0 -8px 40px rgba(123,94,167,0.15)',animation:'slideUp .3s ease'}}>
        <div style={{width:40,height:4,borderRadius:2,background:C.border,margin:'0 auto 24px'}}/>
        <div style={{fontSize:20,fontWeight:700,color:C.text,marginBottom:6,letterSpacing:'-0.02em'}}>Invite to Guess</div>
        <div style={{fontSize:14,color:C.text3,marginBottom:20,lineHeight:1.5}}>Share this with your group. They visit the link and enter the code to guess your fart.</div>
        <div style={{background:C.purpleBg,borderRadius:14,border:`1px solid ${C.purpleBorder}`,padding:'20px 24px',textAlign:'center',marginBottom:16}}>
          <div style={{fontSize:11,fontWeight:600,color:C.text3,letterSpacing:'0.08em',textTransform:'uppercase',marginBottom:8}}>Session Code</div>
          <div style={{fontSize:34,fontWeight:700,color:C.purple,letterSpacing:'0.15em'}}>{code}</div>
        </div>
        <div style={{background:'#f5f5f5',borderRadius:10,padding:'14px 16px',marginBottom:16,fontSize:14,color:C.text2,lineHeight:1.6,whiteSpace:'pre-line'}}>{msg}</div>
        <button onClick={copy} style={{width:'100%',padding:16,borderRadius:14,border:'none',background:copied?C.green:`linear-gradient(135deg,${C.purple},${C.purpleLight})`,color:'white',fontFamily:"'Open Sans',sans-serif",fontSize:16,fontWeight:700,cursor:'pointer',marginBottom:10,transition:'background .25s'}}>
          {copied?'Copied!':'Copy Invite Message'}
        </button>
        {navigator.share && <button onClick={()=>navigator.share({text:msg}).catch(()=>{})} style={{width:'100%',padding:16,borderRadius:14,border:`1.5px solid ${C.purpleBorder}`,background:'white',fontFamily:"'Open Sans',sans-serif",fontSize:16,fontWeight:600,color:C.purple,cursor:'pointer'}}>Share via…</button>}
      </div>
    </div>
  );
}

// ── useRecorder ──
function useRecorder(){
  const [recording,setRecording]=useState(false);
  const [secs,setSecs]=useState(0);
  const [audioUrl,setAudioUrl]=useState(null);
  const [audioBuffer,setAudioBuffer]=useState(null);
  const mr=useRef(null),chunks=useRef([]),timer=useRef(null);

  const start=useCallback(async()=>{
    try{
      const stream=await navigator.mediaDevices.getUserMedia({audio:true});
      chunks.current=[];
      const rec=new MediaRecorder(stream); mr.current=rec;
      rec.ondataavailable=e=>{ if(e.data.size>0) chunks.current.push(e.data); };
      rec.onstop=async()=>{
        stream.getTracks().forEach(t=>t.stop());
        const blob=new Blob(chunks.current,{type:'audio/webm'});
        setAudioUrl(URL.createObjectURL(blob));
        try{
          const ab=await blob.arrayBuffer();
          const ctx=new(window.AudioContext||window.webkitAudioContext)();
          setAudioBuffer(await ctx.decodeAudioData(ab));
        }catch(e){console.warn(e);}
      };
      rec.start(100); setRecording(true); setSecs(0);
      timer.current=setInterval(()=>setSecs(s=>s+1),1000);
    }catch(e){ alert('Microphone access required. Please allow it and try again.'); }
  },[]);

  const stop=useCallback(()=>{ clearInterval(timer.current); setRecording(false); if(mr.current&&mr.current.state!=='inactive') mr.current.stop(); },[]);
  const reset=useCallback(()=>{ stop(); setAudioUrl(null); setAudioBuffer(null); setSecs(0); },[stop]);
  const toggle=useCallback(()=>{ recording?stop():start(); },[recording,start,stop]);
  return{recording,secs,audioUrl,audioBuffer,toggle,reset};
}

// ════════════ LIST ════════════
function ListScreen({submissions,onOpen,onSubmitNew}){
  const [shareTarget,setShareTarget]=useState(null);
  return (
    <div style={{display:'flex',flexDirection:'column',height:'100%',background:C.bg}}>
      <NavBar title=""/>
      <div style={{flex:1,overflowY:'auto',WebkitOverflowScrolling:'touch',paddingBottom:140}}>
        <div style={{padding:'6px 20px 20px'}}>
          <div style={{fontSize:34,fontWeight:700,color:C.text,letterSpacing:'-0.025em',lineHeight:1.1}}>All Farts</div>
        </div>
        {submissions.length>0&&(
          <div style={{padding:'0 20px',marginBottom:24}}>
            <div style={{fontSize:12,fontWeight:600,color:C.text3,letterSpacing:'0.07em',textTransform:'uppercase',marginBottom:10,paddingLeft:2}}>Recordings</div>
            <div style={{background:C.white,borderRadius:14,overflow:'hidden',boxShadow:'0 2px 12px rgba(123,94,167,0.07)'}}>
              {submissions.map((sub,i)=>(
                <div key={sub.id} style={{display:'flex',alignItems:'center',borderBottom:i<submissions.length-1?`1px solid ${C.border}`:'none'}}>
                  <div onClick={()=>onOpen(sub)} style={{flex:1,display:'flex',alignItems:'center',padding:'14px 16px',gap:12,cursor:'pointer',WebkitTapHighlightColor:'transparent'}}>
                    <MiniWave buffer={sub.audioBuffer}/>
                    <div style={{flex:1,minWidth:0}}>
                      <div style={{fontSize:15,fontWeight:600,color:C.text,whiteSpace:'nowrap',overflow:'hidden',textOverflow:'ellipsis',marginBottom:2}}>
                        {sub.name}
                        {sub.isPlaceholder&&<span style={{marginLeft:8,fontSize:11,fontWeight:600,color:C.purple,background:C.purpleBg,borderRadius:4,padding:'2px 6px'}}>Sample</span>}
                      </div>
                      <div style={{fontSize:12,color:C.text3}}>{formatDate(sub.date)} · {(sub.guesses||[]).length} guess{(sub.guesses||[]).length!==1?'es':''}</div>
                    </div>
                    <div style={{fontSize:13,color:C.text3,flexShrink:0,marginRight:6}}>{fmt(sub.duration)}</div>
                    <svg width="8" height="13" viewBox="0 0 8 13" fill="none" style={{flexShrink:0}}><path d="M1.5 1.5L6.5 6.5L1.5 11.5" stroke={C.text4} strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round"/></svg>
                  </div>
                  <button onClick={e=>{e.stopPropagation();setShareTarget(sub);}} style={{width:44,height:44,display:'flex',alignItems:'center',justifyContent:'center',background:'none',border:'none',cursor:'pointer',color:C.purple,marginRight:8,flexShrink:0,WebkitTapHighlightColor:'transparent'}}>
                    <svg width="18" height="20" viewBox="0 0 18 20" fill="none">
                      <path d="M12 4L9 1M9 1L6 4M9 1V13" stroke={C.purple} strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round"/>
                      <path d="M3 9H2C1.44772 9 1 9.44772 1 10V18C1 18.5523 1.44772 19 2 19H16C16.5523 19 17 18.5523 17 18V10C17 9.44772 16.5523 9 16 9H15" stroke={C.purple} strokeWidth="1.8" strokeLinecap="round"/>
                    </svg>
                  </button>
                </div>
              ))}
            </div>
          </div>
        )}
        {submissions.length===0&&(
          <div style={{padding:'40px 20px',textAlign:'center'}}>
            <div style={{fontSize:18,fontWeight:600,color:C.text,marginBottom:8}}>No farts recorded yet.</div>
            <div style={{fontSize:14,color:C.text3,lineHeight:1.5}}>Tap below to record the first one.</div>
          </div>
        )}
      </div>
      {/* Pinned bottom bar */}
      <div style={{flexShrink:0,background:'rgba(245,243,250,0.96)',backdropFilter:'blur(20px)',borderTop:`1px solid ${C.purpleBorder}`,display:'flex',flexDirection:'column',alignItems:'center',paddingTop:16,paddingBottom:32,gap:8}}>
        <button onClick={onSubmitNew} style={{width:88,height:88,borderRadius:'50%',border:`3.5px solid ${C.purple}`,background:C.white,display:'flex',alignItems:'center',justifyContent:'center',cursor:'pointer',padding:0,WebkitTapHighlightColor:'transparent',boxShadow:`0 6px 28px rgba(123,94,167,0.28)`}}>
          <div style={{width:64,height:64,borderRadius:'50%',background:C.purple}}/>
        </button>
        <div style={{fontSize:12,color:C.text3,fontWeight:500,letterSpacing:'0.01em'}}>Submit a Fart</div>
      </div>
      {shareTarget&&<ShareModal sub={shareTarget} onClose={()=>setShareTarget(null)}/>}
    </div>
  );
}

// ════════════ RECORD ════════════
function RecordScreen({onBack,onSave}){
  const rec=useRecorder();
  const handleSave=()=>{
    if(!rec.audioBuffer||!rec.audioUrl)return;
    const now=new Date();
    onSave({id:Date.now(),name:formatDate(now),date:now,duration:rec.audioBuffer.duration,audioUrl:rec.audioUrl,audioBuffer:rec.audioBuffer,guesses:[]});
  };
  const bars=Array.from({length:9});
  return (
    <div style={{display:'flex',flexDirection:'column',height:'100%',background:C.white}}>
      <NavBar title="New Fart" onBack={()=>{rec.reset();onBack();}}/>
      <div style={{flex:1,display:'flex',flexDirection:'column',alignItems:'center',justifyContent:'center',padding:'32px 30px'}}>
        {rec.recording&&(
          <div style={{display:'flex',gap:5,height:56,alignItems:'flex-end',marginBottom:36}}>
            {bars.map((_,i)=><div key={i} style={{width:5,background:C.purple,borderRadius:3,opacity:.7,animation:`barPulse 1.2s ease-in-out ${i*.13}s infinite`}}/>)}
          </div>
        )}
        {!rec.recording&&rec.audioBuffer&&(
          <div style={{width:'100%',marginBottom:28}}>
            <BigWave buffer={rec.audioBuffer} height={80}/>
            <div style={{display:'flex',justifyContent:'space-between',marginTop:8}}>
              <span style={{fontSize:12,color:C.text3}}>0:00</span>
              <span style={{fontSize:12,color:C.text3}}>{fmt(rec.audioBuffer.duration)}</span>
            </div>
          </div>
        )}
        {!rec.recording&&!rec.audioBuffer&&<div style={{height:80,marginBottom:28}}/>}
        <div style={{display:'flex',alignItems:'center',gap:8,background:C.purpleBg,borderRadius:100,padding:'9px 20px',marginBottom:60,border:`1px solid ${C.purpleBorder}`}}>
          {rec.recording&&<div style={{width:8,height:8,borderRadius:'50%',background:C.purple,animation:'blink 1s step-end infinite'}}/>}
          <div style={{fontSize:16,fontWeight:600,color:C.text,fontVariantNumeric:'tabular-nums',letterSpacing:'0.05em'}}>{fmt(rec.secs)}</div>
        </div>
      </div>
      <div style={{display:'flex',alignItems:'center',justifyContent:'space-between',padding:'0 40px 48px'}}>
        <button onClick={rec.reset} style={{width:52,height:52,borderRadius:'50%',background:C.purpleBg,border:`1px solid ${C.purpleBorder}`,display:'flex',alignItems:'center',justifyContent:'center',cursor:'pointer',opacity:(rec.recording||rec.audioBuffer)?1:0,pointerEvents:(rec.recording||rec.audioBuffer)?'auto':'none',transition:'opacity .2s',WebkitTapHighlightColor:'transparent'}}>
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none"><path d="M1 1L13 13M13 1L1 13" stroke={C.purple} strokeWidth="1.8" strokeLinecap="round"/></svg>
        </button>
        <div style={{display:'flex',flexDirection:'column',alignItems:'center',gap:12}}>
          <RecordButton recording={rec.recording} onToggle={rec.toggle} size="large"/>
          <div style={{fontSize:13,color:C.text3,fontWeight:500}}>{rec.recording?'Tap to stop':rec.audioBuffer?'Tap to re-record':'Tap to record'}</div>
        </div>
        <button onClick={handleSave} style={{width:52,height:52,borderRadius:'50%',background:C.purple,border:'none',display:'flex',alignItems:'center',justifyContent:'center',cursor:'pointer',opacity:(!rec.recording&&rec.audioBuffer)?1:0,pointerEvents:(!rec.recording&&rec.audioBuffer)?'auto':'none',transition:'opacity .2s',WebkitTapHighlightColor:'transparent',boxShadow:`0 4px 16px rgba(123,94,167,0.35)`}}>
          <svg width="18" height="14" viewBox="0 0 18 14" fill="none"><path d="M1.5 7L6.5 12L16.5 1.5" stroke="white" strokeWidth="2.2" strokeLinecap="round" strokeLinejoin="round"/></svg>
        </button>
      </div>
    </div>
  );
}

// ════════════ DETAIL ════════════
function DetailScreen({sub,onBack,onGuessAdded}){
  const [playing,setPlaying]=useState(false);
  const [analysing,setAnalysing]=useState(false);
  const [aiText,setAiText]=useState(null);
  const [guesses,setGuesses]=useState(sub?.guesses||[]);
  const [shareOpen,setShareOpen]=useState(false);
  const audioRef=useRef(null);
  const rec=useRecorder();

  useEffect(()=>{
    if(sub?.audioUrl){ audioRef.current=new Audio(sub.audioUrl); audioRef.current.onended=()=>setPlaying(false); }
    return()=>{ if(audioRef.current) audioRef.current.pause(); rec.reset(); };
  },[sub?.id]);

  const togglePlay=()=>{
    if(!audioRef.current)return;
    if(playing){ audioRef.current.pause(); audioRef.current.currentTime=0; setPlaying(false); }
    else{ audioRef.current.currentTime=0; audioRef.current.play().catch(()=>{}); setPlaying(true); }
  };

  const analyse=async()=>{
    if(!rec.audioBuffer||!sub?.audioBuffer)return;
    setAnalysing(true);
    const score=compareAudio(sub.audioBuffer,rec.audioBuffer);
    let text=`Match score: ${score}%.`;
    try{
      const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:250,messages:[{role:'user',content:`You are a dry fart acoustics analyst. Match: ${score}%. Write 2 short sentences. Direct, no exclamation marks, no emoji.`}]})});
      const data=await res.json(); text=data.content?.[0]?.text||text;
    }catch(e){}
    const ng={id:Date.now(),name:`Guest ${guesses.length+1}`,score,analysis:text,audioUrl:rec.audioUrl};
    const updated=[...guesses,ng]; setGuesses(updated); onGuessAdded(sub.id,updated);
    setAiText(text); setAnalysing(false); rec.reset();
  };

  if(!sub)return null;
  return (
    <div style={{display:'flex',flexDirection:'column',height:'100%',background:C.white}}>
      <NavBar title={sub.name} onBack={()=>{if(audioRef.current)audioRef.current.pause();rec.reset();onBack();}}
        rightSlot={<button onClick={()=>setShareOpen(true)} style={{background:'none',border:'none',color:C.purple,cursor:'pointer',fontSize:14,fontWeight:600,fontFamily:"'Open Sans',sans-serif"}}>Share</button>}
      />
      <div style={{flex:1,overflowY:'auto',WebkitOverflowScrolling:'touch'}}>
        <div style={{padding:'24px 20px',borderBottom:`1px solid ${C.border}`}}>
          <div style={{fontSize:12,color:C.text3,marginBottom:16,fontWeight:500}}>
            {formatDate(sub.date)} · {fmt(sub.duration)}
            {sub.isPlaceholder&&<span style={{marginLeft:8,color:C.purple,background:C.purpleBg,borderRadius:4,padding:'2px 7px',fontSize:11,fontWeight:600}}>Sample</span>}
          </div>
          <div style={{marginBottom:16}}><BigWave buffer={sub.audioBuffer} height={64}/></div>
          <div style={{display:'flex',alignItems:'center',gap:14}}>
            <button onClick={togglePlay} style={{width:44,height:44,borderRadius:'50%',background:`linear-gradient(135deg,${C.purple},${C.purpleLight})`,border:'none',display:'flex',alignItems:'center',justifyContent:'center',cursor:'pointer',color:'white',boxShadow:`0 4px 14px rgba(123,94,167,0.3)`}}>
              {playing
                ?<svg width="12" height="14" viewBox="0 0 12 14" fill="none"><rect x="1" y="1" width="3.5" height="12" rx="1" fill="white"/><rect x="7.5" y="1" width="3.5" height="12" rx="1" fill="white"/></svg>
                :<svg width="14" height="16" viewBox="0 0 14 16" fill="none"><path d="M2 1.5L13 8L2 14.5V1.5Z" fill="white"/></svg>}
            </button>
            <div style={{fontSize:14,color:C.text3}}>{playing?'Playing…':'Tap to play original'}</div>
          </div>
        </div>
        <div style={{padding:'24px 20px 0'}}>
          <div style={{fontSize:22,fontWeight:700,color:C.text,letterSpacing:'-0.02em',marginBottom:4}}>Guesses</div>
          <div style={{fontSize:14,color:C.text3,marginBottom:20,lineHeight:1.5}}>Record your fart. AI will score it against the original.</div>
          {guesses.length>0&&(
            <div style={{display:'flex',flexDirection:'column',gap:8,marginBottom:24}}>
              {guesses.map(g=>(
                <div key={g.id} style={{background:C.purpleBg,borderRadius:12,padding:'14px 16px',display:'flex',alignItems:'center',gap:10,border:`1px solid ${C.purpleBorder}`}}>
                  <button onClick={()=>{if(g.audioUrl){const a=new Audio(g.audioUrl);a.play().catch(()=>{})}}} style={{width:34,height:34,borderRadius:'50%',background:C.white,border:`1px solid ${C.purpleBorder}`,display:'flex',alignItems:'center',justifyContent:'center',cursor:'pointer',flexShrink:0,boxShadow:`0 2px 8px rgba(123,94,167,0.12)`}}>
                    <svg width="10" height="12" viewBox="0 0 10 12" fill="none"><path d="M1 1L9 6L1 11V1Z" fill={C.purple}/></svg>
                  </button>
                  <div style={{flex:1,minWidth:0}}>
                    <div style={{fontSize:14,fontWeight:600,color:C.text}}>{g.name}</div>
                    {g.analysis&&<div style={{fontSize:12,color:C.text3,marginTop:2,lineHeight:1.4}}>{g.analysis}</div>}
                  </div>
                  <div style={{fontSize:18,fontWeight:700,color:C.purple,flexShrink:0}}>{g.score}%</div>
                </div>
              ))}
            </div>
          )}
          <div style={{background:C.purpleBg,borderRadius:14,padding:'28px 24px',display:'flex',flexDirection:'column',alignItems:'center',gap:16,marginBottom:40,border:`1px solid ${C.purpleBorder}`}}>
            <div style={{fontSize:16,fontWeight:700,color:C.text}}>Your guess</div>
            <div style={{fontSize:13,color:C.text3,textAlign:'center',marginTop:-6}}>{rec.audioBuffer?'Tap to re-record.':'Record your fart below.'}</div>
            <div style={{display:'flex',flexDirection:'column',alignItems:'center',gap:10}}>
              <RecordButton recording={rec.recording} onToggle={rec.toggle} size="small"/>
              <div style={{fontSize:12,color:C.text3}}>{rec.recording?`Recording · ${fmt(rec.secs)}`:rec.audioBuffer?'Tap to re-record':'Tap to record'}</div>
            </div>
            {rec.audioBuffer&&!rec.recording&&<div style={{width:'100%'}}><BigWave buffer={rec.audioBuffer} height={40} color={C.purpleLight}/></div>}
            {rec.audioBuffer&&!rec.recording&&(
              <button onClick={analyse} disabled={analysing} style={{width:'100%',padding:14,borderRadius:12,border:'none',background:analysing?`rgba(123,94,167,0.5)`:`linear-gradient(135deg,${C.purple},${C.purpleLight})`,color:'white',fontFamily:"'Open Sans',sans-serif",fontSize:15,fontWeight:700,cursor:analysing?'not-allowed':'pointer',boxShadow:analysing?'none':`0 4px 16px rgba(123,94,167,0.3)`}}>
                {analysing?'Analysing…':'Submit Guess'}
              </button>
            )}
            {aiText&&(
              <div style={{background:C.white,borderRadius:10,padding:'14px 16px',width:'100%',border:`1px solid ${C.purpleBorder}`}}>
                <div style={{fontSize:10,fontWeight:700,textTransform:'uppercase',letterSpacing:'0.08em',color:C.text3,marginBottom:5}}>Analysis</div>
                <div style={{fontSize:13,color:C.text2,lineHeight:1.6}}>{aiText}</div>
              </div>
            )}
          </div>
        </div>
      </div>
      {shareOpen&&<ShareModal sub={sub} onClose={()=>setShareOpen(false)}/>}
    </div>
  );
}

// ════════════ PLACEHOLDER ════════════
function makePlaceholder(){
  return new Promise(resolve=>{
    try{
      const sr=44100,dur=2.1,ac=new OfflineAudioContext(1,Math.floor(sr*dur),sr);
      const buf=ac.createBuffer(1,Math.floor(sr*dur),sr),d=buf.getChannelData(0);
      for(let i=0;i<d.length;i++){const t=i/sr,env=Math.sin(Math.PI*t/dur)*Math.exp(-t*.8);d[i]=env*(Math.sin(2*Math.PI*80*t)*.4+(Math.random()*2-1)*.6);}
      const src=ac.createBufferSource();src.buffer=buf;src.connect(ac.destination);src.start();
      ac.startRendering().then(rendered=>{
        const data=rendered.getChannelData(0),out=new Int16Array(data.length);
        for(let i=0;i<data.length;i++) out[i]=Math.max(-1,Math.min(1,data[i]))*0x7fff;
        const hdr=new ArrayBuffer(44),v=new DataView(hdr);
        const ws=(o,s)=>{for(let i=0;i<s.length;i++)v.setUint8(o+i,s.charCodeAt(i));};
        ws(0,'RIFF');v.setUint32(4,36+out.byteLength,true);ws(8,'WAVE');ws(12,'fmt ');
        v.setUint32(16,16,true);v.setUint16(20,1,true);v.setUint16(22,1,true);
        v.setUint32(24,sr,true);v.setUint32(28,sr*2,true);v.setUint16(32,2,true);v.setUint16(34,16,true);
        ws(36,'data');v.setUint32(40,out.byteLength,true);
        const final=new Uint8Array(44+out.byteLength);final.set(new Uint8Array(hdr));final.set(new Uint8Array(out.buffer),44);
        const url=URL.createObjectURL(new Blob([final],{type:'audio/wav'}));
        resolve({buf:rendered,url});
      }).catch(()=>resolve(null));
    }catch(e){resolve(null);}
  });
}

// ════════════ ROOT ════════════
function App(){
  const [screen,setScreen]=useState('list');
  const [submissions,setSubmissions]=useState([]);
  const [activeSub,setActiveSub]=useState(null);
  const [hist,setHist]=useState(['list']);

  useEffect(()=>{
    makePlaceholder().then(r=>{
      if(!r)return;
      const now=new Date(Date.now()-8*60*1000);
      setSubmissions([{id:Date.now()-9999,name:'Sample Fart',date:now,duration:r.buf.duration,audioUrl:r.url,audioBuffer:r.buf,guesses:[],isPlaceholder:true}]);
    });
  },[]);

  const navTo=s=>{ setHist(h=>[...h,s]); setScreen(s); };
  const pop=()=>setHist(h=>{ const n=h.slice(0,-1); setScreen(n[n.length-1]); return n; });

  const slide=s=>({
    position:'absolute',inset:0,
    transform:screen===s?'translateX(0)':hist.indexOf(s)<hist.indexOf(screen)?'translateX(-28%)':'translateX(100%)',
    transition:'transform .34s cubic-bezier(.4,0,.2,1)',
    zIndex:screen===s?2:1,
  });

  return (
    <div style={{height:'100vh',overflow:'hidden',fontFamily:"'Open Sans',sans-serif",position:'relative',background:C.bg}}>
      <div style={slide('list')}>
        <ListScreen submissions={submissions} onOpen={sub=>{setActiveSub(sub);navTo('detail');}} onSubmitNew={()=>navTo('record')}/>
      </div>
      <div style={slide('record')}>
        <RecordScreen onBack={pop} onSave={sub=>{setSubmissions(s=>[sub,...s]);pop();}}/>
      </div>
      <div style={slide('detail')}>
        {activeSub&&<DetailScreen
          sub={submissions.find(s=>s.id===activeSub.id)||activeSub}
          onBack={pop}
          onGuessAdded={(id,g)=>setSubmissions(p=>p.map(s=>s.id===id?{...s,guesses:g}:s))}
        />}
      </div>
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>
</body>
</html>
