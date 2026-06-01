---
order: 3
title: Агент сканирования Flow — для администраторов
---

[html]

<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
  :root {
    --c-bg: #f7f8fa; --c-surface: #ffffff; --c-border: #e4e7ec;
    --c-text: #111827; --c-muted: #6b7280; --c-accent: #2563eb;
    --c-accent-light: #eff6ff; --c-warn: #fef3c7; --c-warn-border: #f59e0b;
    --c-warn-text: #92400e; --c-success: #ecfdf5; --c-success-border: #10b981;
    --c-success-text: #065f46; --c-win: #dbeafe; --c-win-text: #1e40af;
    --c-lin: #d1fae5; --c-lin-text: #065f46;
    --font: 'Segoe UI', -apple-system, system-ui, sans-serif;
    --mono: 'Consolas', 'Courier New', monospace;
    --radius: 8px; --radius-lg: 12px;
  }
  html { scroll-behavior: smooth; }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: var(--font); background: var(--c-bg); color: var(--c-text); font-size: 15px; line-height: 1.6; }
  .page { max-width: 860px; margin: 0 auto; padding: 40px 24px 80px; }
  .header { background: var(--c-surface); border: 1px solid var(--c-border); border-radius: var(--radius-lg); padding: 28px 32px; margin-bottom: 32px; display: flex; align-items: flex-start; gap: 20px; }
  .header-icon { width: 48px; height: 48px; background: var(--c-accent-light); border-radius: var(--radius); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .header-icon svg { width: 26px; height: 26px; color: var(--c-accent); }
  .header h1 { font-size: 22px; font-weight: 600; margin-bottom: 4px; }
  .header p { color: var(--c-muted); font-size: 14px; }
  .toc { background: var(--c-surface); border: 1px solid var(--c-border); border-radius: var(--radius-lg); padding: 20px 24px; margin-bottom: 32px; }
  .toc-label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.08em; color: var(--c-muted); margin-bottom: 10px; }
  .toc ol { padding-left: 20px; }
  .toc li { margin: 4px 0; }
  .toc a { color: var(--c-accent); text-decoration: none; font-size: 14px; }
  .toc a:hover { text-decoration: underline; }
  .section { background: var(--c-surface); border: 1px solid var(--c-border); border-radius: var(--radius-lg); padding: 28px 32px; margin-bottom: 24px; }
  .section-title { font-size: 18px; font-weight: 600; margin-bottom: 16px; display: flex; align-items: center; gap: 10px; padding-bottom: 14px; border-bottom: 1px solid var(--c-border); }
  .section-title .num { width: 28px; height: 28px; background: var(--c-accent); color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 14px; font-weight: 600; flex-shrink: 0; }
  h3 { font-size: 15px; font-weight: 600; margin: 20px 0 8px; }
  h3:first-child { margin-top: 0; }
  p { margin-bottom: 10px; color: var(--c-text); }
  p:last-child { margin-bottom: 0; }
  .os-split { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin: 16px 0; }
  .os-block { background: var(--c-bg); border: 1px solid var(--c-border); border-radius: var(--radius); padding: 16px; }
  .os-block-title { font-size: 13px; font-weight: 600; margin-bottom: 10px; }
  .os-block p { font-size: 14px; color: var(--c-muted); }
  @media (max-width: 600px) { .os-split { grid-template-columns: 1fr; } }
  .badge { display: inline-flex; align-items: center; gap: 4px; font-size: 12px; font-weight: 500; padding: 3px 10px; border-radius: 20px; }
  .badge-win { background: var(--c-win); color: var(--c-win-text); }
  .badge-lin { background: var(--c-lin); color: var(--c-lin-text); }
  .os-badges { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 10px; }
  .steps { display: flex; flex-direction: column; gap: 14px; }
  .step { display: flex; gap: 14px; align-items: flex-start; }
  .step-num { width: 26px; height: 26px; border-radius: 50%; border: 2px solid var(--c-accent); color: var(--c-accent); font-size: 13px; font-weight: 600; display: flex; align-items: center; justify-content: center; flex-shrink: 0; margin-top: 1px; }
  .step-body { flex: 1; }
  .step-body p { margin-bottom: 6px; }
  pre { background: #1e293b; color: #e2e8f0; font-family: var(--mono); font-size: 13px; line-height: 1.6; padding: 14px 16px; border-radius: var(--radius); margin: 10px 0; overflow-x: auto; }
  code { font-family: var(--mono); font-size: 13px; background: #f1f5f9; color: #334155; padding: 2px 6px; border-radius: 4px; }
  .comment { color: #64748b; }
  .callout { border-radius: var(--radius); padding: 12px 16px; margin: 14px 0; display: flex; gap: 10px; align-items: flex-start; }
  .callout-warn { background: var(--c-warn); border: 1px solid var(--c-warn-border); }
  .callout-warn p { color: var(--c-warn-text); margin: 0; font-size: 14px; }
  .callout-success { background: var(--c-success); border: 1px solid var(--c-success-border); }
  .callout-success p { color: var(--c-success-text); margin: 0; font-size: 14px; }
  .callout-info { background: var(--c-accent-light); border: 1px solid #93c5fd; }
  .callout-info p { color: #1e40af; margin: 0; font-size: 14px; }
  .callout-icon { font-size: 16px; flex-shrink: 0; margin-top: 1px; }
  .img-wrap { margin: 14px 0; border: 1px solid var(--c-border); border-radius: var(--radius); overflow: hidden; }
  .img-wrap img { width: 100%; display: block; cursor: zoom-in; transition: opacity 0.15s; }
  .img-wrap img:hover { opacity: 0.9; }
  .img-caption { font-size: 12px; color: var(--c-muted); padding: 7px 12px; background: var(--c-bg); border-top: 1px solid var(--c-border); }
  .img-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin: 14px 0; }
  @media (max-width: 600px) { .img-row { grid-template-columns: 1fr; } }
  .iface-blocks { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin: 14px 0; }
  @media (max-width: 600px) { .iface-blocks { grid-template-columns: 1fr; } }
  .iface-block { background: var(--c-bg); border: 1px solid var(--c-border); border-radius: var(--radius); padding: 14px; }
  .iface-block-label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; color: var(--c-muted); margin-bottom: 6px; }
  .iface-block-name { font-size: 14px; font-weight: 600; margin-bottom: 4px; }
  .iface-block p { font-size: 13px; color: var(--c-muted); margin: 0; }
  .dot { display: inline-block; width: 8px; height: 8px; border-radius: 50%; margin-right: 4px; }
  .dot-left { background: #2563eb; } .dot-center { background: #10b981; } .dot-right { background: #f59e0b; }
  ul { padding-left: 20px; margin: 8px 0; }
  ul li { margin: 4px 0; font-size: 14px; color: var(--c-muted); }
  hr { border: none; border-top: 1px solid var(--c-border); margin: 20px 0; }
</style>
</head>
<body>
<div class="page">

  <div class="header">
    <div class="header-icon">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8M12 17v4"/><polyline points="17 8 19 10 23 6"/>
      </svg>
    </div>
    <div>
      <h1>Агент сканирования Flow — для администраторов</h1>
      <p>Диагностика, проверка статуса и решение проблем с драйверами</p>
    </div>
  </div>

  <div class="toc">
    <div class="toc-label">Содержание</div>
    <ol>
      <li><a href="#status">Проверка статуса агента</a></li>
      <li><a href="#drivers">Требования к драйверам</a></li>
      <li><a href="#oldscanner">Заглушка для старого сканера на Ubuntu</a></li>
    </ol>
  </div>

  <div class="section" id="status">
    <div class="section-title"><span class="num">1</span> Проверка статуса агента</div>
    <p>Команды для проверки состояния агента и просмотра журнала на Linux:</p>
    <pre><span class="comment"># Статус службы</span>
systemctl --user status flow-agent

<span class="comment"># Журнал в реальном времени</span>
journalctl --user -u flow.agent -f</pre>
  </div>

  <div class="section" id="drivers">
    <div class="section-title"><span class="num">2</span> Требования к драйверам</div>
    <div class="callout callout-warn"><span class="callout-icon">⚠️</span><p>Агент поддерживает только <strong>TWAIN</strong>-драйверы на Windows и <strong>SANE</strong>-драйверы на Linux. Если драйвер сканера установлен в другом формате, сканирование работать не будет.</p></div>
    <div class="os-split">
      <div class="os-block">
        <div class="os-block-title"><span class="badge badge-win">Windows</span></div>
        <p>При скачивании драйвера с сайта производителя выбирайте тип <strong>TWAIN</strong>.</p>
      </div>
      <div class="os-block">
        <div class="os-block-title"><span class="badge badge-lin">Linux</span></div>
        <p>В Astra Linux, RedOS и Rosa Linux утилиты SANE установлены по умолчанию. Для ALT Linux и Ubuntu — см. статью об установке.</p>
      </div>
    </div>
  </div>

  <div class="section" id="oldscanner">
    <div class="section-title"><span class="num">3</span> Заглушка для старого сканера на Ubuntu</div>
    <p>Если сканер очень старый и стандартный драйвер не устанавливается, можно создать заглушку:</p>
    <pre>sudo apt install equivs
equivs-control libsane-dummy</pre>
    <p>Откройте созданный файл и добавьте три строки:</p>
    <pre>nano libsane-dummy

<span class="comment"># Package: libsane</span>
<span class="comment"># Version: 1.0.21</span>
<span class="comment"># Depends: libsane1</span></pre>
    <p>Соберите и установите пакет:</p>
    <pre>equivs-build libsane-dummy
sudo dpkg -i libsane_1.0.21_all.deb</pre>
  </div>

</div>

<div id="lightbox" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.85);z-index:9999;cursor:zoom-out;align-items:center;justify-content:center;padding:24px;">
  <img id="lightbox-img" src="" style="max-width:100%;max-height:100%;border-radius:6px;box-shadow:0 8px 40px rgba(0,0,0,0.6);object-fit:contain;">
</div>
<script>
  document.querySelectorAll('.img-wrap img').forEach(function(img) {
    img.addEventListener('click', function() {
      document.getElementById('lightbox-img').src = this.src;
      document.getElementById('lightbox').style.display = 'flex';
      document.body.style.overflow = 'hidden';
    });
  });
  document.getElementById('lightbox').addEventListener('click', function() {
    this.style.display = 'none';
    document.body.style.overflow = '';
  });
  document.addEventListener('keydown', function(e) {
    if (e.key === 'Escape') { document.getElementById('lightbox').style.display = 'none'; document.body.style.overflow = ''; }
  });
</script>

</body>
</html>

[/html]