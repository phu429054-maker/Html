<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Share HTML</title>
<style>
:root{
  --bg:#0b0d0e; --panel:#15191b; --line:#262c2f; --fg:#f2f5f4; --muted:#8d9a97;
  --acc:#1b8f7b; --acc-fg:#fff; --err:#e8736f; --ok:#4fc3a1;
  color-scheme: dark;
  padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px); box-sizing:border-box;
}
@media (prefers-color-scheme: light){
  :root{ --bg:#f5f7f6; --panel:#fff; --line:#d9e0de; --fg:#101514; --muted:#5d6b68; --acc:#14806c; --err:#c9443f; --ok:#14806c; color-scheme: light; }
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;min-height:100%;background:var(--bg);color:var(--fg);font-family:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{padding:0 0 28px}
:focus-visible{outline:2px solid var(--acc);outline-offset:2px}

.wrap{max-width:560px;margin:0 auto;padding:28px 18px 8px}
h1{font-size:28px;font-weight:600;margin:0 0 4px;letter-spacing:-.4px}
.sub{color:var(--muted);margin:0 0 26px;font-size:15px;line-height:1.5}

.field{margin-bottom:22px}
label.t{display:block;font-size:14px;color:var(--muted);margin-bottom:8px}
.name{display:flex;align-items:center;background:var(--panel);border:1px solid var(--line);border-radius:12px;overflow:hidden}
.name:focus-within{border-color:var(--acc)}
.name input{flex:1;min-width:0;background:none;border:0;color:var(--fg);font-size:17px;padding:14px 14px;outline:none}
.slug{padding:0 14px 0 0;color:var(--muted);font-size:13px;white-space:nowrap;max-width:48%;overflow:hidden;text-overflow:ellipsis}

.tabs{display:grid;grid-template-columns:1fr 1fr;background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:4px;gap:4px;margin-bottom:12px}
.tab{background:none;border:0;color:var(--muted);font-size:15px;padding:10px;border-radius:9px;cursor:pointer}
.tab[aria-selected="true"]{background:var(--acc);color:var(--acc-fg)}

.drop{display:flex;flex-direction:column;align-items:center;justify-content:center;gap:6px;text-align:center;min-height:170px;border:1.5px dashed var(--line);border-radius:14px;background:var(--panel);padding:22px;cursor:pointer;transition:border-color .15s,background .15s}
.drop:hover,.drop.over{border-color:var(--acc)}
.drop strong{font-size:16px;font-weight:500}
.drop span{color:var(--muted);font-size:14px}
.drop.has{border-style:solid;border-color:var(--acc)}
.drop input{display:none}

textarea{width:100%;min-height:210px;resize:vertical;background:var(--panel);border:1px solid var(--line);border-radius:14px;color:var(--fg);font:13px/1.5 ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;padding:14px;outline:none;tab-size:2}
textarea:focus{border-color:var(--acc)}
[hidden]{display:none!important}

.go{width:100%;border:0;border-radius:14px;background:var(--acc);color:var(--acc-fg);font-size:18px;font-weight:600;padding:16px;cursor:pointer;transition:transform .08s,filter .15s}
.go:active{transform:scale(.985);filter:brightness(1.15)}
.go:disabled{opacity:.6}
.msg{min-height:20px;margin:10px 2px 0;font-size:14px;color:var(--err)}

.result{margin-top:22px;background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:16px}
.result h2{font-size:15px;font-weight:500;margin:0 0 10px;color:var(--ok)}
.link{width:100%;background:var(--bg);border:1px solid var(--line);border-radius:10px;color:var(--fg);font:13px ui-monospace,Menlo,Consolas,monospace;padding:11px 12px}
.row{display:flex;gap:8px;margin-top:10px}
.row button,.row a{flex:1;text-align:center;text-decoration:none;background:var(--line);color:var(--fg);border:0;border-radius:10px;padding:12px;font-size:15px;cursor:pointer}
.row .main{background:var(--acc);color:var(--acc-fg)}
.note{color:var(--muted);font-size:13px;line-height:1.5;margin:12px 0 0}
.note.warn{color:var(--err)}
.credit{text-align:right;font-size:11px;color:var(--muted);opacity:.7;padding:18px 18px 0;max-width:560px;margin:0 auto}

/* viewer */
.viewer{position:fixed;inset:0;background:#fff;display:none}
.viewer iframe{width:100%;height:100%;border:0;display:block;background:#fff}
body.view .viewer{display:block}
body.view .wrap,body.view .credit{display:none}
.vmsg{position:fixed;inset:0;display:none;align-items:center;justify-content:center;flex-direction:column;gap:12px;padding:24px;text-align:center;background:var(--bg)}
body.view.bad .vmsg{display:flex}
body.view.bad .viewer{display:none}
.vmsg a{color:var(--ok)}
</style>
</head>
<body>

<main class="wrap" id="creator">
  <h1>Share HTML</h1>
  <p class="sub">Tải file hoặc dán code HTML, đặt tên cho web, rồi tạo link để gửi cho người khác.</p>

  <div class="field">
    <label class="t" for="name">Tên web</label>
    <div class="name">
      <input id="name" type="text" placeholder="vd: may-tinh-cua-tui" autocomplete="off" maxlength="60">
      <span class="slug" id="slug"></span>
    </div>
  </div>

  <div class="field">
    <div class="tabs" role="tablist">
      <button class="tab" role="tab" id="tabFile" aria-selected="true">Tải file</button>
      <button class="tab" role="tab" id="tabCode" aria-selected="false">Dán code</button>
    </div>

    <label class="drop" id="drop" for="file">
      <strong id="dropTitle">Chọn file HTML</strong>
      <span id="dropSub">hoặc kéo thả vào đây (.html, .htm)</span>
      <input id="file" type="file" accept=".html,.htm,.txt,text/html,text/plain">
    </label>

    <textarea id="code" placeholder="Dán code HTML vào đây..." spellcheck="false" hidden></textarea>
  </div>

  <button class="go" id="create">Tạo link</button>
  <div class="msg" id="msg" role="alert"></div>

  <section class="result" id="result" hidden>
    <h2 id="resTitle">Link đã sẵn sàng</h2>
    <input class="link" id="link" type="text" readonly aria-label="Link chia sẻ">
    <div class="row">
      <button class="main" id="copy">Chép link</button>
      <a id="open" href="#" target="_blank" rel="noopener">Mở thử</a>
    </div>
    <p class="note" id="info"></p>
  </section>
</main>

<div class="credit">Power made by hoangtt.03</div>

<div class="viewer" id="viewer"></div>
<div class="vmsg" id="vmsg">
  <div id="vtext">Không mở được web này.</div>
  <a href="#" id="vback">Về trang tạo link</a>
</div>

<script>
(function(){
  var $ = function(id){ return document.getElementById(id); };
  var mode = 'file';          // 'file' | 'code'
  var fileText = '';
  var fileName = '';

  /* ---------- helpers ---------- */
  function slugify(s){
    return s.normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/đ/g,'d').replace(/Đ/g,'D')
      .toLowerCase().replace(/[^a-z0-9]+/g,'-').replace(/^-+|-+$/g,'').slice(0,40);
  }
  function b64u(bytes){
    var s = '', CH = 0x8000;
    for (var i = 0; i < bytes.length; i += CH) s += String.fromCharCode.apply(null, bytes.subarray(i, i + CH));
    return btoa(s).replace(/\+/g,'-').replace(/\//g,'_').replace(/=+$/,'');
  }
  function unb64u(str){
    str = str.replace(/-/g,'+').replace(/_/g,'/');
    while (str.length % 4) str += '=';
    var bin = atob(str), out = new Uint8Array(bin.length);
    for (var i = 0; i < bin.length; i++) out[i] = bin.charCodeAt(i);
    return out;
  }
  async function pipe(bytes, stream){
    var w = stream.writable.getWriter();
    w.write(bytes); w.close();
    return new Uint8Array(await new Response(stream.readable).arrayBuffer());
  }
  async function pack(text){
    var bytes = new TextEncoder().encode(text);
    if (window.CompressionStream){
      try { return 'z' + b64u(await pipe(bytes, new CompressionStream('deflate-raw'))); } catch(e){}
    }
    return 'r' + b64u(bytes);
  }
  async function unpack(s){
    var kind = s[0], bytes = unb64u(s.slice(1));
    if (kind === 'z') bytes = await pipe(bytes, new DecompressionStream('deflate-raw'));
    return new TextDecoder().decode(bytes);
  }

  /* ---------- creator UI ---------- */
  var nameEl = $('name'), slugEl = $('slug'), msgEl = $('msg');
  nameEl.addEventListener('input', function(){
    var s = slugify(nameEl.value);
    slugEl.textContent = s ? '/' + s : '';
  });

  function setMode(m){
    mode = m;
    $('tabFile').setAttribute('aria-selected', m === 'file');
    $('tabCode').setAttribute('aria-selected', m === 'code');
    $('drop').hidden = (m !== 'file');
    $('code').hidden = (m !== 'code');
    msgEl.textContent = '';
  }
  $('tabFile').onclick = function(){ setMode('file'); };
  $('tabCode').onclick = function(){ setMode('code'); };

  function readFile(f){
    if (!f) return;
    if (f.size > 4 * 1024 * 1024){ msgEl.textContent = 'File quá lớn (tối đa 4 MB).'; return; }
    var r = new FileReader();
    r.onload = function(){
      fileText = String(r.result || '');
      fileName = f.name;
      $('drop').classList.add('has');
      $('dropTitle').textContent = f.name;
      $('dropSub').textContent = (f.size / 1024).toFixed(1) + ' KB · bấm để chọn file khác';
      if (!nameEl.value.trim()){
        nameEl.value = f.name.replace(/\.[^.]+$/, '');
        nameEl.dispatchEvent(new Event('input'));
      }
      msgEl.textContent = '';
    };
    r.onerror = function(){ msgEl.textContent = 'Không đọc được file này.'; };
    r.readAsText(f);
  }
  $('file').addEventListener('change', function(e){ readFile(e.target.files[0]); });
  var drop = $('drop');
  ['dragenter','dragover'].forEach(function(ev){
    drop.addEventListener(ev, function(e){ e.preventDefault(); drop.classList.add('over'); });
  });
  ['dragleave','drop'].forEach(function(ev){
    drop.addEventListener(ev, function(e){ e.preventDefault(); drop.classList.remove('over'); });
  });
  drop.addEventListener('drop', function(e){ readFile(e.dataTransfer.files[0]); });

  $('create').onclick = async function(){
    msgEl.textContent = '';
    var name = nameEl.value.trim();
    var html = mode === 'file' ? fileText : $('code').value;
    if (!name){ msgEl.textContent = 'Nhập tên web trước.'; nameEl.focus(); return; }
    if (!html.trim()){ msgEl.textContent = mode === 'file' ? 'Chưa chọn file HTML.' : 'Chưa có code để tạo link.'; return; }
    var btn = $('create'); btn.disabled = true; btn.textContent = 'Đang tạo...';
    try {
      var data = await pack(html);
      var base = location.href.split('#')[0];
      var url = base + '#n=' + encodeURIComponent(name) + '&d=' + data;
      $('link').value = url;
      $('open').href = url;
      $('resTitle').textContent = 'Link của "' + name + '" đã sẵn sàng';
      var info = $('info');
      var len = url.length;
      info.className = 'note';
      info.textContent = 'Độ dài link: ' + len.toLocaleString('vi-VN') + ' ký tự. Toàn bộ web nằm trong link, không có gì được tải lên server.';
      if (location.protocol === 'file:'){
        info.className = 'note warn';
        info.textContent += ' Bạn đang mở file này từ máy, nên link chỉ chạy trên máy bạn. Đưa file share-html.html lên một hosting (Netlify Drop, GitHub Pages...) rồi tạo link ở đó để gửi cho người khác.';
      } else if (len > 32000){
        info.className = 'note warn';
        info.textContent += ' Link khá dài, một số app chat hoặc trình duyệt có thể cắt mất link. Nên rút gọn code hoặc dùng hosting.';
      }
      $('result').hidden = false;
      $('result').scrollIntoView({behavior:'smooth', block:'nearest'});
    } catch(err){
      msgEl.textContent = 'Không tạo được link: ' + (err && err.message ? err.message : 'lỗi không rõ');
    }
    btn.disabled = false; btn.textContent = 'Tạo link';
  };

  $('copy').onclick = async function(){
    var inp = $('link'), b = $('copy');
    try { await navigator.clipboard.writeText(inp.value); }
    catch(e){ inp.focus(); inp.select(); try { document.execCommand('copy'); } catch(_){} }
    b.textContent = 'Đã chép';
    setTimeout(function(){ b.textContent = 'Chép link'; }, 1600);
  };
  $('link').addEventListener('focus', function(){ this.select(); });

  /* ---------- viewer ---------- */
  async function route(){
    var h = location.hash.slice(1);
    var params = new URLSearchParams(h);
    var d = params.get('d');
    document.body.classList.remove('view','bad');
    $('viewer').innerHTML = '';
    if (!d){ document.title = 'Share HTML'; return; }
    document.body.classList.add('view');
    $('vback').href = location.href.split('#')[0];
    var name = params.get('n') || 'Web';
    try {
      var html = await unpack(d);
      document.title = name;
      var f = document.createElement('iframe');
      f.setAttribute('sandbox', 'allow-scripts allow-forms allow-popups allow-modals allow-downloads');
      f.title = name;
      f.srcdoc = html;
      $('viewer').appendChild(f);
    } catch(e){
      document.body.classList.add('bad');
      $('vtext').textContent = 'Link này bị lỗi hoặc đã bị cắt mất một phần, nên không mở được.';
    }
  }
  window.addEventListener('hashchange', route);
  route();
})();
</script>
</body>
</html>
