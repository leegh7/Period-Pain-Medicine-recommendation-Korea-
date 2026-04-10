<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>생리통 약 추천 퀴즈 🌸</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI','Apple SD Gothic Neo','Noto Sans KR',sans-serif;background:#fff5f8;min-height:100vh;display:flex;align-items:flex-start;justify-content:center;padding:2rem 1rem}
#app{width:100%;max-width:480px}
.progress-bar{height:4px;background:#f0d4de;border-radius:2px;margin-bottom:1.5rem}
.progress-fill{height:100%;background:#D4537E;border-radius:2px;transition:width .4s ease}
.step-label{font-size:12px;color:#999;margin-bottom:0.5rem}
.question-card{background:#fff;border:1px solid #f5c0d0;border-radius:16px;padding:1.25rem;margin-bottom:1rem}
.question-emoji{font-size:24px;margin-bottom:0.5rem}
.question-text{font-size:16px;font-weight:500;color:#222;line-height:1.5}
.options{display:flex;flex-direction:column;gap:8px;margin-top:1rem}
.option-btn{background:#fff;border:1px solid #eeccd8;border-radius:10px;padding:12px 14px;font-size:14px;font-family:inherit;color:#333;cursor:pointer;text-align:left;line-height:1.4;transition:border-color .15s,background .15s}
.option-btn:hover{border-color:#D4537E;background:#fff0f5}
.option-btn.selected{border-color:#D4537E;background:#fff0f5;color:#4B1528;font-weight:500}
.nav{display:flex;justify-content:space-between;align-items:center;margin-top:1rem}
.btn-back{background:#fff;border:1px solid #ddd;border-radius:10px;padding:8px 16px;font-size:13px;font-family:inherit;color:#888;cursor:pointer}
.btn-back:disabled{opacity:.3;cursor:default}
.btn-next{background:#D4537E;border:none;border-radius:10px;padding:8px 20px;font-size:13px;font-family:inherit;color:#fff;cursor:pointer;font-weight:500;opacity:.4;pointer-events:none;transition:opacity .15s}
.btn-next.active{opacity:1;pointer-events:auto}
.result-header{text-align:center;padding:1rem 0 0.5rem}
.result-emoji{font-size:36px}
.result-title{font-size:13px;color:#999;margin-top:0.25rem}
.product-card{background:#fff;border:1.5px solid #ED93B1;border-radius:16px;padding:1.25rem;margin:0.75rem 0}
.product-name{font-size:17px;font-weight:600;color:#993556}
.product-sub{font-size:12px;color:#999;margin-top:2px}
.product-reason{font-size:13px;color:#333;margin-top:10px;line-height:1.7}
.caution-box{background:#fff8ee;border:1px solid #fac775;border-radius:10px;padding:10px 12px;margin-top:10px}
.caution-label{font-size:11px;font-weight:600;color:#854F0B;margin-bottom:4px}
.caution-text{font-size:12px;color:#854F0B;line-height:1.6}
.alt-section{margin-top:1rem}
.alt-label{font-size:12px;color:#999;margin-bottom:6px}
.alt-card{background:#fafafa;border:1px solid #eee;border-radius:10px;padding:10px 12px;margin-bottom:6px}
.alt-name{font-size:14px;font-weight:500;color:#333}
.alt-reason{font-size:12px;color:#888;margin-top:2px;line-height:1.5}
.btn-restart{display:block;width:100%;margin-top:1.25rem;background:#fff;border:1px solid #ED93B1;border-radius:10px;padding:10px;font-size:13px;font-family:inherit;color:#993556;cursor:pointer;text-align:center}
.disclaimer{font-size:11px;color:#bbb;text-align:center;margin-top:1rem;line-height:1.6}
</style>
</head>
<body>
<div id="app"></div>
<script>
const questions=[
  {id:'pain_type',emoji:'💫',text:'생리통이 주로 어떤 느낌인가요?',options:[
    {label:'꼬이고 쥐어짜는 느낌 (경련성)',value:'cramp'},
    {label:'묵직하고 욱신거리는 느낌',value:'dull'},
    {label:'아랫배 전체가 아프고 오래 지속돼요',value:'lasting'}
  ]},
  {id:'swell',emoji:'🌊',text:'생리 전후로 손발이 붓거나 몸이 무겁게 느껴지나요?',options:[
    {label:'네, 많이 부어요',value:'yes'},
    {label:'약간 있어요',value:'mild'},
    {label:'부종은 별로 없어요',value:'no'}
  ]},
  {id:'stomach',emoji:'🫙',text:'빈속에 약을 먹으면 속이 쓰린 편인가요?',options:[
    {label:'네, 위장이 약한 편이에요',value:'sensitive'},
    {label:'조금 불편한 편이에요',value:'mild'},
    {label:'괜찮은 편이에요',value:'ok'}
  ]},
  {id:'nsaids',emoji:'🌿',text:'이부프로펜 계열 약을 드셨을 때 알레르기나 심한 부작용이 있었나요?',options:[
    {label:'네, 있어요 (알레르기/부작용)',value:'yes'},
    {label:'아니요, 없어요',value:'no'},
    {label:'잘 모르겠어요',value:'unknown'}
  ]},
  {id:'drink',emoji:'🍵',text:'생리 기간 중 음주를 하는 편인가요?',options:[
    {label:'네, 마시는 편이에요',value:'yes'},
    {label:'거의 안 마셔요',value:'no'}
  ]},
  {id:'strength',emoji:'⚡',text:'생리통 강도가 어느 정도인가요?',options:[
    {label:'가벼운 편 (일상 생활 가능)',value:'mild'},
    {label:'보통 (집중이 어려울 정도)',value:'moderate'},
    {label:'심한 편 (누워있어야 할 정도)',value:'severe'}
  ]}
];

const ans={};
let step=0;

function getResult(){
  const p=ans.pain_type,sw=ans.swell,st=ans.stomach,ns=ans.nsaids,dk=ans.drink,str=ans.strength;
  if(p==='cramp'||ns==='yes'){
    return {
      main:{name:'부스코판 플러스',sub:'부틸스코폴라민브롬화물 10mg + 아세트아미노펜 500mg',reason:'경련성(쥐어짜는) 통증이나 NSAIDs 복용이 어려운 경우에 적합해요. 자궁 평활근 경련을 억제해 뒤틀리는 느낌을 빠르게 완화시켜줍니다.',caution:'음주 후 복용 금지(간독성). 눈이 침침해질 수 있어 운전 주의. 녹내장 환자는 복용 불가.'},
      alt:[{name:'샤이닝정 / 포나민정',reason:'부스코판 플러스와 동일 성분의 동등의약품. 가격이 조금 더 합리적일 수 있어요.'}]
    };
  }
  if(str==='severe'){
    return {
      main:{name:'탁센 / 이지엔6 스트롱',sub:'나프록센 250mg (연질캡슐)',reason:'심한 생리통에는 반감기가 긴 나프록센 계열이 효과적이에요. 이부프로펜보다 오래(12~15시간) 지속되어 자주 먹지 않아도 돼요.',caution:'위장장애 위험이 높아 반드시 식후 복용. 음주 금지. 타 NSAIDs와 함께 복용 금지.'},
      alt:[{name:'아나프록스',reason:'나프록센나트륨 275mg 정제 형태. 흡수가 약간 빠르고 정제를 선호하는 경우 추천.'}]
    };
  }
  if(st==='sensitive'){
    return {
      main:{name:'탁센 레이디',sub:'이부프로펜 200mg + 파마브롬 25mg + 산화마그네슘 83mg',reason:'이부프로펜의 진통 효과에 파마브롬으로 부기까지 잡고, 산화마그네슘이 위장 부작용을 완화해줘요. 속쓰림이 걱정될 때 제격이에요.',caution:'공복 복용 피하기. 마그네슘 과다 시 설사 가능. 임신 30주 이후 금기.'},
      alt:[
        {name:'이지엔6 이브',reason:'부기 완화 기본 조합(이부프로펜+파마브롬). 연질캡슐로 흡수 빠름.'},
        {name:'우먼스 타이레놀',reason:'아세트아미노펜 기반이라 위장 부담 최소. NSAIDs 대신 선택 가능.'}
      ]
    };
  }
  if(sw==='yes'||sw==='mild'){
    return {
      main:{name:'이지엔6 이브',sub:'이부프로펜 200mg + 파마브롬 25mg (연질캡슐)',reason:'통증과 부기를 동시에 잡는 기본 조합이에요. 연질캡슐 형태라 흡수가 빠르고 효과 발현도 빠른 편이에요.',caution:'공복 복용 금지. 음주 시 위장출혈 위험. 임신 30주 이후 금기.'},
      alt:[
        {name:'게보린 소프트',reason:'이부프로펜 250mg으로 함량이 더 높아 통증이 좀 더 강할 때 선택.'},
        {name:'탁센 레이디',reason:'위장이 예민하다면 산화마그네슘이 추가된 탁센 레이디도 좋은 선택.'}
      ]
    };
  }
  if(dk==='yes'||ns==='yes'){
    return {
      main:{name:'우먼스 타이레놀',sub:'아세트아미노펜 500mg + 파마브롬 25mg',reason:'NSAIDs가 아닌 아세트아미노펜 기반이라 위장 부담이 적어요. 부기도 함께 잡아줘요.',caution:'음주자 복용 금지(간독성). 타 아세트아미노펜 제품과 중복 복용 금지.'},
      alt:[{name:'부스코판 플러스',reason:'경련성 통증이 동반된다면 진경 작용이 있는 부스코판 플러스도 고려해요.'}]
    };
  }
  return {
    main:{name:'이지엔6 이브',sub:'이부프로펜 200mg + 파마브롬 25mg (연질캡슐)',reason:'가볍거나 보통 강도의 생리통에 가장 기본적이고 효과적인 선택이에요. 연질캡슐로 흡수가 빠르고 부기도 함께 완화돼요.',caution:'공복 복용 금지. 음주 시 위장 부작용 주의. 임신 30주 이후 금기.'},
    alt:[
      {name:'애드빌',reason:'부기 없이 통증만 있다면 이부프로펜 단일 성분 애드빌도 충분해요.'},
      {name:'탁센 레이디',reason:'속쓰림이 있다면 산화마그네슘 추가된 탁센 레이디를 선택해요.'}
    ]
  };
}

function render(){
  const app=document.getElementById('app');
  if(step<questions.length){
    const q=questions[step];
    const pct=Math.round((step/questions.length)*100);
    const sel=ans[q.id];
    app.innerHTML=`
      <div class="step-label">${step+1} / ${questions.length}</div>
      <div class="progress-bar"><div class="progress-fill" style="width:${pct}%"></div></div>
      <div class="question-card">
        <div class="question-emoji">${q.emoji}</div>
        <div class="question-text">${q.text}</div>
      </div>
      <div class="options">${q.options.map(o=>`<button class="option-btn${sel===o.value?' selected':''}" data-val="${o.value}">${o.label}</button>`).join('')}</div>
      <div class="nav">
        <button class="btn-back"${step===0?' disabled':''}>← 이전</button>
        <button class="btn-next${sel?' active':''}">다음 →</button>
      </div>
    `;
    app.querySelectorAll('.option-btn').forEach(btn=>{
      btn.addEventListener('click',()=>{ans[q.id]=btn.dataset.val;render();});
    });
    app.querySelector('.btn-back').addEventListener('click',()=>{if(step>0){step--;render();}});
    app.querySelector('.btn-next').addEventListener('click',()=>{if(ans[q.id]){step++;render();}});
  } else {
    const r=getResult();
    app.innerHTML=`
      <div class="result-header">
        <div class="result-emoji">🌸</div>
        <div class="result-title">나에게 맞는 생리통 약은?</div>
      </div>
      <div class="product-card">
        <div class="product-name">${r.main.name}</div>
        <div class="product-sub">${r.main.sub}</div>
        <div class="product-reason">${r.main.reason}</div>
        <div class="caution-box">
          <div class="caution-label">복용 시 주의하세요</div>
          <div class="caution-text">${r.main.caution}</div>
        </div>
      </div>
      ${r.alt.length?`<div class="alt-section"><div class="alt-label">이런 제품도 맞을 수 있어요</div>${r.alt.map(a=>`<div class="alt-card"><div class="alt-name">${a.name}</div><div class="alt-reason">${a.reason}</div></div>`).join('')}</div>`:''}
      <button class="btn-restart">처음부터 다시 하기</button>
      <div class="disclaimer">이 결과는 일반적인 정보 제공 목적입니다.<br>정확한 복약 정보는 약사 또는 의사와 상담하세요.</div>
    `;
    app.querySelector('.btn-restart').addEventListener('click',()=>{Object.keys(ans).forEach(k=>delete ans[k]);step=0;render();});
  }
}
render();
</script>
</body>
</html>
