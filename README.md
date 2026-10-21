<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Калькулятор объёма 3D-модели</title>
<style>
:root{--bg:#f5f8fb;--panel:#ffffff;--border:#dbe4ee;--text:#14212e;--muted:#5c6d7d;--accent:#0061ae;--accent-hover:#004d8a;--ok:#1a7f45;--ok-bg:#e9f7ef;--err:#b3261e;--err-bg:#fdecea;--warn:#8a5a00;--warn-bg:#fff4e0;--canvas-bg:#eef2f7}
@media(prefers-color-scheme:dark){:root{--bg:#12171d;--panel:#1a2229;--border:#2c3947;--text:#eaf1f8;--muted:#9aacbc;--accent:#4ea3e0;--accent-hover:#6fb7e8;--ok:#6fdb9c;--ok-bg:#123322;--err:#ff8a80;--err-bg:#3a1a18;--warn:#f0c36d;--warn-bg:#3a2c0d;--canvas-bg:#0f1418}}
*{box-sizing:border-box}
body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;background:var(--bg);color:var(--text);padding:32px 16px 64px;line-height:1.5}
.wrap{max-width:1300px;margin:0 auto}
h1{font-size:1.4rem;margin:0 0 6px}
.subtitle{color:var(--muted);margin:0 0 24px;font-size:.95rem}
.top-header-row{display:flex;align-items:center;justify-content:space-between;gap:16px;flex-wrap:wrap;margin:0 0 24px}
.top-buttons{display:flex;gap:10px;flex-wrap:wrap}
.top-nav-btn{display:inline-block;padding:10px 18px;border-radius:8px;background:var(--accent);color:#fff;text-decoration:none;font-size:.95rem;transition:background .15s}
.top-nav-btn:hover{background:var(--accent-hover);color:#fff}
.top-logo{height:48px;width:auto;display:block;object-fit:contain}
.panel{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:20px;margin-bottom:20px}
#dropzone{border:2px dashed var(--border);border-radius:10px;padding:32px 16px;text-align:center;cursor:pointer;transition:border-color .15s,background .15s}
#dropzone.drag{border-color:var(--accent);background:color-mix(in srgb,var(--accent)8%,transparent)}
#dropzone p{margin:0 0 12px;color:var(--muted)}
#dropzone .hint{font-size:.85rem;margin-top:8px}
button{font:inherit;background:var(--accent);color:#fff;border:none;padding:10px 18px;border-radius:8px;cursor:pointer}
button:hover{background:var(--accent-hover)}
input[type=file]{display:none}
select{font:inherit;padding:6px 10px;border-radius:6px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.unit-row{display:flex;align-items:center;gap:10px;flex-wrap:wrap;margin-top:16px;padding-top:14px;border-top:1px solid var(--border);font-size:.9rem;color:var(--muted)}
.muted{color:var(--muted);font-size:.85rem}
.badge{display:inline-block;padding:2px 8px;border-radius:999px;font-size:.75rem;font-weight:600}
.badge-ok{background:var(--ok-bg);color:var(--ok)}
.badge-err{background:var(--err-bg);color:var(--err)}
.badge-warn{background:var(--warn-bg);color:var(--warn)}
.err-text{color:var(--err);font-size:.85rem}
details{margin-top:4px}
summary{cursor:pointer;color:var(--accent);font-size:.9rem}
code{background:color-mix(in srgb,var(--border)60%,transparent);padding:1px 5px;border-radius:4px;font-size:.85em}
.status-line{display:flex;align-items:center;gap:8px;color:var(--muted);font-size:.85rem;margin-top:10px}
.spinner{width:14px;height:14px;border-radius:50%;border:2px solid var(--border);border-top-color:var(--accent);animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
#results{display:flex;flex-direction:column;gap:14px}
.card{border:1px solid var(--border);border-radius:10px;overflow:hidden}
.card-header{display:flex;align-items:flex-start;justify-content:space-between;gap:10px;flex-wrap:wrap;padding:10px 14px;background:color-mix(in srgb,var(--border)25%,transparent);font-size:.9rem}
.card-header .name{font-weight:600;overflow-wrap:anywhere}
.card-header-left{display:flex;flex-direction:column;gap:5px;min-width:220px}
.card-header-right{display:flex;align-items:center;gap:8px}
.acct-line{display:flex;align-items:center;gap:8px;flex-wrap:wrap;font-size:.78rem;color:var(--muted)}
.acct-number-label{display:flex;align-items:center;gap:4px;cursor:help}
.acct-number-input{font:inherit;font-family:ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;font-weight:600;color:var(--accent);font-size:.8rem;width:120px;padding:2px 6px;border-radius:5px;border:1px solid var(--border);background:var(--panel)}
.acct-number-input.manual{border-color:var(--accent);background:color-mix(in srgb,var(--accent)10%,var(--panel))}
.serial-label,.author-label{display:flex;align-items:center;gap:4px}
.serial-input{font:inherit;font-size:.8rem;width:74px;padding:2px 6px;border-radius:5px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.author-select{font:inherit;font-size:.8rem;padding:2px 4px;border-radius:5px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.btn-delete{display:flex;align-items:center;justify-content:center;width:26px;height:26px;padding:0;border-radius:6px;line-height:1;font-size:.9rem;background:transparent;color:var(--muted);border:1px solid var(--border);cursor:pointer}
.btn-delete:hover{color:var(--err);border-color:var(--err)}
.card-body{display:flex;gap:16px;padding:14px;flex-wrap:wrap;align-items:flex-start}
.preview-wrap{flex:0 1 900px;min-width:0;max-width:900px}
canvas.preview{width:100%;height:695px;max-width:100%;background:#fff;border-radius:8px;display:block;border:1px solid var(--border)}
.preview-actions{display:flex;align-items:center;justify-content:space-between;gap:8px;flex-wrap:wrap;margin-top:6px}
.preview-caption{font-size:.72rem;color:var(--muted)}
.preview-btns{display:flex;align-items:center;gap:6px;flex-wrap:wrap;min-width:0}
.btn-download,.btn-download-no-logo,.btn-production{font-size:1.44rem;padding:8px 20px;border-radius:10px;background:var(--accent);color:#fff;border:none;cursor:pointer}
.btn-download:hover,.btn-download-no-logo:hover,.btn-production:hover{background:var(--accent-hover)}
.btn-download:disabled,.btn-download-no-logo:disabled,.btn-production:disabled{opacity:.4;cursor:not-allowed}
.btn-reset{font-size:1.6rem;line-height:1;padding:8px 16px;border-radius:10px;background:transparent;color:var(--muted);border:1px solid var(--border);cursor:pointer}
.btn-hd{font-size:1.2rem;padding:8px 16px;border-radius:10px;background:transparent;color:var(--accent);border:1px solid var(--accent);cursor:pointer;font-weight:600;transition:background .15s}
.btn-hd:hover{background:color-mix(in srgb,var(--accent)15%,transparent)}
.btn-hd.hd-active{background:var(--warn);color:#fff;border-color:var(--warn)}
.btn-hd:disabled{opacity:.5;cursor:wait}
.btn-reset:hover{color:var(--text);border-color:var(--muted)}
.btn-zoom-reset{font-size:1.2rem;padding:8px 16px;border-radius:10px;background:transparent;color:var(--muted);border:1px solid var(--border);cursor:pointer}
.btn-zoom-reset:hover{color:var(--text);border-color:var(--muted)}
canvas.preview{touch-action:none;cursor:grab}
canvas.preview.dragging{cursor:grabbing}
.volume-block{flex:1 1 220px;min-width:200px}
.volume-value{font-size:1.3rem;font-weight:700;color:var(--ok)}
.mass-value{font-size:1.05rem;font-weight:600;color:var(--text);margin-top:2px}
.qty-row{margin-top:8px}
.qty-label{display:flex;align-items:center;gap:6px;font-size:.85rem;color:var(--muted)}
.qty-input{width:64px;font:inherit;padding:3px 6px;border-radius:5px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.manual-row{margin-top:8px;display:flex;gap:12px;flex-wrap:wrap}
.manual-label{display:flex;align-items:center;gap:4px;font-size:.85rem;color:var(--muted)}
.manual-volume-input,.manual-mass-input{font:inherit;padding:2px 6px;border-radius:5px;border:1px solid var(--border);background:var(--panel);color:var(--text);width:80px}
.manual-volume-input:focus,.manual-mass-input:focus{outline:none;border-color:var(--accent)}
.price-row{margin-top:10px}
.price-label{display:flex;align-items:center;gap:6px;font-size:.85rem;color:var(--muted);flex-wrap:wrap}
.price-per-cm3-input,.postproc-price-input{width:84px;font:inherit;padding:3px 6px;border-radius:5px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.price-per-cm3-input:focus,.postproc-price-input:focus{outline:none;border-color:var(--accent)}
.price-total{margin-top:4px;font-size:.9rem;color:var(--muted)}
.price-total b{color:var(--text);font-weight:700}
.price-postproc-block{margin-top:8px;padding-top:8px;border-top:1px dashed var(--border)}
.volume-note{margin-top:4px}
.real-volume-note,.hash-note{font-size:.85rem;color:var(--muted);margin-top:4px}
@media(max-width:480px){.card-body{flex-direction:column}.preview-wrap{max-width:100%;flex:1 1 auto}canvas.preview{width:100%;height:460px}}
.req-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px 16px;margin-bottom:16px}
.req-field{display:flex;flex-direction:column;gap:4px;font-size:.85rem;color:var(--muted)}
.req-field input,.req-field select,.req-field textarea{font:inherit;font-size:.95rem;padding:7px 10px;border-radius:6px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.req-field textarea{resize:vertical;font-family:inherit}
.req-notes{margin-bottom:16px}
.req-postproc{margin-bottom:16px}
.req-postproc-title{font-size:.85rem;color:var(--muted);margin-bottom:6px}
.req-postproc-list{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:6px 14px}
.req-postproc-list label{display:flex;align-items:center;gap:6px;font-size:.88rem;cursor:pointer}
.req-actions{margin-bottom:18px;display:flex;gap:10px;flex-wrap:wrap}
#reqDownloadBtn:disabled,#reqXlsBtn:disabled,#reqAllBtn:disabled,#reqPriceBtn:disabled{opacity:.4;cursor:not-allowed}
.btn-secondary{background:transparent;color:var(--text);border:1px solid var(--border)}
.btn-secondary:hover{background:color-mix(in srgb,var(--border)25%,transparent)}
.req-preview-wrap{border-top:1px solid var(--border);padding-top:16px}
.req-preview-caption{margin-bottom:8px}
.req-preview{background:#fff;color:#1a1a1a;border:1px solid var(--border);border-radius:8px;padding:24px 28px;font-family:Calibri,Arial,sans-serif;font-size:.92rem;line-height:1.55;max-width:780px;overflow-x:auto}
.req-preview img.req-logo{width:auto;height:20px;margin-bottom:14px;display:block}
.req-preview .req-title{font-weight:700;font-size:1.05rem;margin:0 0 8px}
.req-preview table.req-dateline{width:100%;border-collapse:collapse;font-weight:700;margin-bottom:12px}
.req-preview table.req-dateline td{padding:0;border:none}
.req-preview p{margin:0 0 8px}
.req-preview .u{text-decoration:underline;font-weight:700}
.req-preview b{font-weight:700}
.req-preview ul{margin:4px 0 8px;padding-left:22px}
.req-preview .req-model-line{margin-left:200px}
.req-preview table.req-sign{width:100%;border-collapse:collapse;margin-top:22px;font-size:.88rem}
.req-preview table.req-sign td{vertical-align:top;padding:4px 10px 4px 0;border:none}
.req-preview .req-sign-line{padding-top:34px}
.req-preview .req-footnotes{margin-top:16px;font-size:.75rem;color:#444}
.req-preview .req-footnotes p{margin:0 0 4px}
@media(max-width:480px){.req-preview{padding:16px}.req-preview .req-model-line{margin-left:0}}
.postproc-label{display:inline-block;padding:6px 12px;margin:2px 4px;border:1px solid var(--border);border-radius:6px;cursor:pointer;transition:background .15s,border-color .15s,color .15s;user-select:none}
.postproc-label.checked{background:var(--accent);color:#fff;border-color:var(--accent)}
.postproc-label input[type=checkbox]{margin-right:6px}
.clip-controls{display:flex;align-items:center;gap:12px;flex-wrap:wrap;margin-top:8px;padding:8px 12px;background:color-mix(in srgb,var(--border)15%,transparent);border-radius:8px;font-size:.85rem;color:var(--text);border:1px solid var(--border)}
.clip-controls .clip-toggle{background:var(--accent);color:#fff;border:none;padding:4px 14px;border-radius:6px;font-size:.85rem;cursor:pointer;font-weight:600;transition:background .15s}
.clip-controls .clip-toggle:hover{background:var(--accent-hover)}
.clip-controls .clip-toggle.active{background:var(--err)}
.clip-controls .clip-toggle.active:hover{background:#8b1a14}
.clip-controls label{display:flex;align-items:center;gap:4px}
.clip-controls select{padding:2px 6px;font-size:.8rem;border-radius:4px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.clip-controls input[type="range"]{width:120px;padding:0;accent-color:var(--accent)}
.clip-controls .clip-pos-value{min-width:32px;text-align:center;font-weight:600;color:var(--accent)}
.floating-panel{position:fixed;right:16px;top:50%;transform:translateY(-50%);display:flex;flex-direction:column;gap:10px;z-index:1000;width:180px}
.drag-handle{width:100%;height:22px;background:color-mix(in srgb,var(--border) 60%,transparent);border-radius:10px 10px 4px 4px;cursor:grab;display:flex;justify-content:center;align-items:center;color:var(--muted);margin-bottom:-2px;user-select:none;touch-action:none;transition:background .2s, color .2s}
.drag-handle:hover{background:var(--border);color:var(--text)}
.drag-handle:active{cursor:grabbing;background:var(--accent);color:#fff}
.drag-handle::after{content:'⠿';font-size:14px;opacity:.7}
.floating-dropzone{width:100%;padding:16px 10px;background:var(--panel);border:2px dashed var(--border);border-radius:12px;text-align:center;cursor:pointer;transition:border-color .2s,background .2s,box-shadow .2s;box-shadow:0 4px 12px rgba(0,0,0,0.08);font-size:.85rem;color:var(--muted);user-select:none}
.floating-dropzone.drag{border-color:var(--accent);background:color-mix(in srgb,var(--accent)8%,var(--panel));box-shadow:0 4px 20px rgba(0,97,174,0.15)}
.floating-dropzone .icon{font-size:1.8rem;display:block;margin-bottom:6px}
.floating-dropzone .hint{font-size:.75rem;margin-top:4px;color:var(--muted)}
.floating-info{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:12px 10px;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.08);font-size:.85rem;color:var(--text);max-height:calc(100vh - 260px);overflow-y:auto}
.floating-info-title{font-size:.68rem;color:var(--muted);text-transform:uppercase;letter-spacing:.04em;margin-top:10px;font-weight:600}
.floating-info-title:first-child{margin-top:0}
.floating-info-value{font-size:1.02rem;font-weight:700;color:var(--accent);font-family:ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;word-break:break-word;margin-top:2px;line-height:1.25}
.floating-info-postproc-list{margin-top:2px;font-size:.82rem;font-weight:600;color:var(--accent);text-align:left;line-height:1.35}
.floating-info-postproc-item{display:flex;align-items:flex-start;gap:4px;padding:1px 0;word-break:break-word}
.floating-info-postproc-item .check{color:var(--ok);flex-shrink:0}
.site-footer{margin-top:24px;text-align:center;color:var(--muted);font-size:.85rem;padding:16px 8px;border-top:1px solid var(--border)}
.group-controls{display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:14px 18px;align-items:end}
.group-field{display:flex;flex-direction:column;gap:6px;font-size:.88rem;color:var(--muted)}
.group-label{display:block;min-height:4.2em;line-height:1.4}
.group-field input{font:inherit;font-size:.95rem;padding:8px 10px;border-radius:6px;border:1px solid var(--border);background:var(--panel);color:var(--text)}
.group-field input:focus{outline:none;border-color:var(--accent)}
@media(max-width:768px){.floating-panel{right:8px;width:130px}.drag-handle{height:18px}.floating-dropzone{padding:12px 6px;font-size:.75rem}.floating-dropzone .icon{font-size:1.4rem}.floating-info{padding:8px 6px}.floating-info-title{font-size:.58rem}.floating-info-value{font-size:.82rem}.floating-info-postproc-list{font-size:.72rem}.top-logo{height:36px}.group-label{min-height:auto}}
</style>
</head>
<body>
<div class="wrap">
<h1>Калькулятор объёма 3D-модели</h1>
<p class="subtitle">Поддерживаются STL и STEP (.stl,.step,.stp). Всё в браузере, файлы никуда не отправляются. Объём в см³.</p>

<div class="top-header-row">
    <div style="display:flex; gap:20px; align-items:center;">
        <img id="topLogo" class="top-logo" alt="Логотип (водяной знак)" title="Логотип (водяной знак)">
        <img id="topLogoAT" class="top-logo" alt="Логотип (компания)" title="Логотип (компания)">
    </div>
    <div class="top-buttons">
        <a href="https://stlcalc.tb.ru/page4" class="top-nav-btn" target="_blank" rel="noopener noreferrer">Генератор паспортов</a>
        <a href="https://stlcalc.tb.ru/page5" class="top-nav-btn" target="_blank" rel="noopener noreferrer">Базовый расчет стоимости</a>
    </div>
</div>

<div class="panel">
<div id="dropzone">
<p><strong>Перетащите файл(ы)</strong> сюда или</p>
<button id="pickBtn" type="button">Выбрать файл</button>
<div class="hint muted">Можно выбрать сразу несколько</div>
<input id="fileInput" type="file" accept=".stl,.step,.stp" multiple>
</div>
<div class="unit-row">
<label for="stlUnitSelect">Единицы в STL (формат их не хранит):</label>
<select id="stlUnitSelect"><option value="mm" selected>миллиметры (мм)</option><option value="cm">сантиметры (см)</option><option value="m">метры (м)</option><option value="in">дюймы (in)</option></select>
</div>
<div class="unit-row">
<label for="materialSelect">Материал (для массы):</label>
<select id="materialSelect"></select>
</div>
<div class="unit-row">
<label for="authorSelect">Автор (для номера бухгалтерии):</label>
<select id="authorSelect"></select>
</div>
<div class="unit-row">
<label for="fastLargeToggle" title="Для файлов ≥ 10 МБ строится упрощённый меш (~60 тыс. треугольников) для быстрого предпросмотра. Объём, масса и все выгрузки считаются по полному мешу."><input type="checkbox" id="fastLargeToggle" checked> Ускоренная обработка больших моделей (≥ 10 МБ) — упрощённый предпросмотр</label>
</div>
<div id="statusLine" class="status-line" style="display:none;"></div>
</div>
<div id="resultsPanel" style="display:none;"><div id="results"></div></div>
<div class="panel"><details><summary>Подробнее о расчётах</summary><div style="margin-top:12px;font-size:.9rem;"><p><strong>STL</strong> – объём по теореме о дивергенции. Единицы выбираются вручную.</p><p><strong>STEP</strong> – подгружается OpenCASCADE (WASM) с CDN, тесселируется, объём по сетке.</p><p><strong>Масса</strong> = объём × плотность.</p><p><strong>Номер</strong> = год_неделя_день_автор-номер.</p><p><strong>Хэш-сумма</strong> – SHA-256 от исходного файла, отображается на изображении.</p><p><strong>Реальный объём</strong> – чистый объём модели (без учёта тонкостенности).</p><p><strong>Стоимость</strong> = объём × цена за 1 см³ × количество.</p><p><strong>LOD</strong> – для больших файлов предпросмотр строится по упрощённому мешу, все расчёты — по полному.</p></div></details></div>

<div class="panel" id="groupControlsPanel">
<h1 style="font-size:1.15rem;margin:0 0 12px;">Групповые операции</h1>
<div class="group-controls">
<label class="group-field">
    <span class="group-label">Количество каждого вида изделий (проставляется одно и то же количество во всех изделиях):</span>
    <input type="number" id="groupQtyInput" min="1" step="1" placeholder="напр. 5">
</label>
<label class="group-field">
    <span class="group-label">Цена за 1 см³ для всех изделий (групповое изменение цены 1 см³ для всех загруженных изделий):</span>
    <input type="number" id="groupPriceInput" min="0" step="0.01" placeholder="напр. 60">
</label>
<label class="group-field">
    <span class="group-label">Продолжить нумерацию изделий начиная с:</span>
    <input type="number" id="groupSerialInput" min="1" step="1" placeholder="напр. 10">
</label>
</div>
</div>

<div class="panel" id="requestPanel">
<h1 style="font-size:1.15rem;margin:0 0 6px;">Генератор заявок</h1>
<p class="subtitle" style="margin-bottom:16px;">Список моделей, формат, материал и суммарный объём подтягиваются из калькулятора.</p>
<div id="requestEmpty" class="muted">Загрузите хотя бы одну модель.</div>
<div id="requestForm" style="display:none;">
<div class="req-grid">
<label class="req-field">Дата заявки <input type="date" id="reqDate"></label>
<label class="req-field">№ заявки за день <input type="text" id="reqDailyNumber" inputmode="numeric" placeholder="напр. 1"></label>
<label class="req-field">Номер КП <input type="text" id="reqKp" placeholder="ШК00-000651"></label>
<label class="req-field">Покупатель <input type="text" id="reqCustomer" placeholder='АО "ГЛОБАТЭК"'></label>
<label class="req-field">Срок, дней <input type="number" id="reqDeadline" min="1" step="1" placeholder="7"></label>
<label class="req-field">Чертёж 2D <select id="reqDrawing"><option value="ДА">ДА</option><option value="НЕТ" selected>НЕТ</option></select></label>
<label class="req-field">Технология <select id="reqTech"><option value="FDM">FDM</option><option value="SLS" selected>SLS</option></select></label>
</div>
<div class="req-postproc"><div class="req-postproc-title">Пост-обработка</div><div class="req-postproc-list" id="reqPostprocList"></div></div>
<label class="req-field req-notes">Доп. информация <textarea id="reqNotes" rows="2" placeholder="пожелания по упаковке"></textarea></label>
<div class="req-actions">
<button type="button" id="reqRefreshBtn" class="btn-secondary">Обновить</button>
<button type="button" id="reqDownloadBtn">Скачать заявку (.docx)</button>
<button type="button" id="reqOldDownloadBtn" class="btn-secondary">Заявка по старой форме (.docx)</button>
<button type="button" id="reqXlsBtn" class="btn-secondary">Скачать таблицу изделий (.xlsx)</button>
<button type="button" id="reqPriceBtn" class="btn-secondary">Расчет Стоимости</button>
<button type="button" id="req1Cbtn" class="btn-secondary">Скачать таблицу 1С</button>
<button type="button" id="reqPhotosWmBtn" class="btn-secondary">Скачать архив с WM</button>
<button type="button" id="reqPhotosNoWmBtn" class="btn-secondary">Скачать архив без WM</button>
<button type="button" id="reqProdPhotosBtn" class="btn-secondary">Скачать архив фото для производства</button>
<button type="button" id="reqProdOrderBtn" class="btn-secondary">Таблица для заказа на производство</button>
<button type="button" id="reqAllBtn" class="btn-secondary">Скачать всё (.zip)</button>
<button type="button" id="reqSaveToFolderBtn" class="btn-secondary">📁 Сохранить в папку…</button>
</div>
<div class="req-preview-wrap"><div class="req-preview-caption muted">Предпросмотр</div><div id="reqPreview" class="req-preview"></div></div>
</div>
</div>
</div>

<div class="floating-panel" id="floatingPanel">
    <div class="drag-handle" id="dragHandle" title="Перетащить панель"></div>
    <div id="floatingDropzone" class="floating-dropzone">
        <span class="icon">📂</span>
        <span>Перетащите<br>файлы сюда</span>
        <div class="hint">или кликните</div>
    </div>
    <div class="floating-info">
        <div class="floating-info-title">Общий объём</div>
        <div class="floating-info-value"><span id="floatingTotalVolume">0.00</span> см³</div>
        <div class="floating-info-title">Общий вес</div>
        <div class="floating-info-value"><span id="floatingTotalMass">0.00</span> г</div>
        <div class="floating-info-title">Общая стоимость</div>
        <div class="floating-info-value"><span id="floatingTotalPrice">0.00</span> ₽</div>
        <div class="floating-info-title" id="floatingTotalPostprocTitle" style="display:none;">Стоимость с пост-обработкой</div>
        <div class="floating-info-value" id="floatingTotalPostprocValue" style="display:none;"><span id="floatingTotalPostprocPrice">0.00</span> ₽</div>
        <div class="floating-info-title">Количество моделей</div>
        <div class="floating-info-value" id="floatingModelsCount">0</div>
        <div class="floating-info-title">Всего деталей по заявке</div>
        <div class="floating-info-value" id="floatingTotalParts">0</div>
        <div class="floating-info-title">Пост-обработка</div>
        <div class="floating-info-postproc-list" id="floatingPostproc"></div>
        <div class="floating-info-title">Дата</div>
        <div class="floating-info-value" id="floatingDate">—</div>
        <div class="floating-info-title">Время</div>
        <div class="floating-info-value" id="floatingTime">—</div>
    </div>
</div>

<footer class="site-footer">
    Генератор заявок v59 (21092026). ООО "Аврора Карбон Технолоджи". Все права защищены. 2026г.
</footer>
</div>

<script>
(async function(){
let LOGO_DATA_URI = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII=';
let LETTERHEAD_LOGO_DATA_URI = LOGO_DATA_URI;

try {
    const wmRes = await fetch('https://stlcalc.tb.ru/page14');
    if (wmRes.ok) {
        const htmlText = await wmRes.text();
        const dummy = document.createElement('div');
        dummy.innerHTML = htmlText;
        const cleanText = (dummy.textContent || "").replace(/\s+/g, '');
        const matchDataUri = cleanText.match(/data:image\/[a-zA-Z]+;base64,[A-Za-z0-9+/=]+/);
        if (matchDataUri) { LOGO_DATA_URI = matchDataUri[0]; } 
        else {
            const matchRaw = cleanText.match(/(iVBORw0KGgo[A-Za-z0-9+/=]{100,})/);
            if (matchRaw) { LOGO_DATA_URI = 'data:image/png;base64,' + matchRaw[1]; }
        }
    }
} catch (e) { console.warn('Ошибка загрузки logo_wm', e); }

try {
    const atRes = await fetch('https://stlcalc.tb.ru/logo_at');
    if (atRes.ok) {
        const htmlText = await atRes.text();
        const dummy = document.createElement('div');
        dummy.innerHTML = htmlText;
        const cleanText = (dummy.textContent || "").replace(/\s+/g, '');
        const matchDataUri = cleanText.match(/data:image\/[a-zA-Z]+;base64,[A-Za-z0-9+/=]+/);
        if (matchDataUri) { LETTERHEAD_LOGO_DATA_URI = matchDataUri[0]; } 
        else {
            const matchRaw = cleanText.match(/(iVBORw0KGgo[A-Za-z0-9+/=]{100,})/);
            if (matchRaw) { LETTERHEAD_LOGO_DATA_URI = 'data:image/png;base64,' + matchRaw[1]; }
        }
    }
} catch (e) { console.warn('Ошибка загрузки logo_at', e); }

const logoImg = new Image(); logoImg.src = LOGO_DATA_URI;
const letterheadImg = new Image(); letterheadImg.src = LETTERHEAD_LOGO_DATA_URI;
document.getElementById('topLogo').src = LOGO_DATA_URI;
document.getElementById('topLogoAT').src = LETTERHEAD_LOGO_DATA_URI;

await Promise.all([
    new Promise(r=>{if(logoImg.complete&&logoImg.naturalWidth>0)r();else{logoImg.onload=r;logoImg.onerror=r}}),
    new Promise(r=>{if(letterheadImg.complete&&letterheadImg.naturalWidth>0)r();else{letterheadImg.onload=r;letterheadImg.onerror=r}})
]).catch(()=>{});

const dropzone=document.getElementById('dropzone'),fileInput=document.getElementById('fileInput'),pickBtn=document.getElementById('pickBtn'),resultsBody=document.getElementById('results'),resultsPanel=document.getElementById('resultsPanel'),statusLine=document.getElementById('statusLine'),stlUnitSelect=document.getElementById('stlUnitSelect'),materialSelect=document.getElementById('materialSelect'),authorSelect=document.getElementById('authorSelect');
const requestEmpty=document.getElementById('requestEmpty'),requestForm=document.getElementById('requestForm'),reqDate=document.getElementById('reqDate'),reqDailyNumber=document.getElementById('reqDailyNumber'),reqKp=document.getElementById('reqKp'),reqCustomer=document.getElementById('reqCustomer'),reqDeadline=document.getElementById('reqDeadline'),reqDrawing=document.getElementById('reqDrawing'),reqTech=document.getElementById('reqTech'),reqPostprocList=document.getElementById('reqPostprocList'),reqNotes=document.getElementById('reqNotes'),reqDownloadBtn=document.getElementById('reqDownloadBtn'),reqOldDownloadBtn=document.getElementById('reqOldDownloadBtn'),reqRefreshBtn=document.getElementById('reqRefreshBtn'),reqXlsBtn=document.getElementById('reqXlsBtn'),reqPriceBtn=document.getElementById('reqPriceBtn'),req1Cbtn=document.getElementById('req1Cbtn'),reqPhotosWmBtn=document.getElementById('reqPhotosWmBtn'),reqPhotosNoWmBtn=document.getElementById('reqPhotosNoWmBtn'),reqProdPhotosBtn=document.getElementById('reqProdPhotosBtn'),reqProdOrderBtn=document.getElementById('reqProdOrderBtn'),reqAllBtn=document.getElementById('reqAllBtn'),reqPreview=document.getElementById('reqPreview');
const floatingTotalVolumeEl=document.getElementById('floatingTotalVolume'),floatingTotalMassEl=document.getElementById('floatingTotalMass'),floatingTotalPriceEl=document.getElementById('floatingTotalPrice');
const floatingTotalPostprocTitleEl=document.getElementById('floatingTotalPostprocTitle'),floatingTotalPostprocValueEl=document.getElementById('floatingTotalPostprocValue'),floatingTotalPostprocPriceEl=document.getElementById('floatingTotalPostprocPrice');
const floatingModelsCountEl=document.getElementById('floatingModelsCount'),floatingTotalPartsEl=document.getElementById('floatingTotalParts'),floatingPostprocEl=document.getElementById('floatingPostproc'),floatingDateEl=document.getElementById('floatingDate'),floatingTimeEl=document.getElementById('floatingTime');
const groupQtyInput=document.getElementById('groupQtyInput'),groupPriceInput=document.getElementById('groupPriceInput'),groupSerialInput=document.getElementById('groupSerialInput');
const fastLargeToggle=document.getElementById('fastLargeToggle');

// === ПЛАВАЮЩАЯ ЗОНА ===
const floatingDropzone=document.getElementById('floatingDropzone');
const floatingPanel=document.getElementById('floatingPanel');
const dragHandle=document.getElementById('dragHandle');

let isDraggingPanel=false, pStartX, pStartY, pInitialLeft, pInitialTop;
function pStartDrag(e){
    if(e.type==='touchstart') e=e.touches[0];
    isDraggingPanel=true;
    pStartX=e.clientX; pStartY=e.clientY;
    const rect=floatingPanel.getBoundingClientRect();
    floatingPanel.style.right='auto';
    floatingPanel.style.bottom='auto';
    floatingPanel.style.transform='none';
    floatingPanel.style.left=rect.left+'px';
    floatingPanel.style.top=rect.top+'px';
    pInitialLeft=rect.left; pInitialTop=rect.top;
    document.addEventListener('mousemove',pDragMove,{passive:false});
    document.addEventListener('mouseup',pDragEnd);
    document.addEventListener('touchmove',pDragMove,{passive:false});
    document.addEventListener('touchend',pDragEnd);
    if(e.type!=='touchstart') e.preventDefault();
}
function pDragMove(e){
    if(!isDraggingPanel) return;
    let evt=e.type==='touchmove'?e.touches[0]:e;
    floatingPanel.style.left=(pInitialLeft + evt.clientX - pStartX)+'px';
    floatingPanel.style.top=(pInitialTop + evt.clientY - pStartY)+'px';
    e.preventDefault();
}
function pDragEnd(){
    isDraggingPanel=false;
    document.removeEventListener('mousemove',pDragMove);
    document.removeEventListener('mouseup',pDragEnd);
    document.removeEventListener('touchmove',pDragMove);
    document.removeEventListener('touchend',pDragEnd);
}
dragHandle.addEventListener('mousedown',pStartDrag);
dragHandle.addEventListener('touchstart',pStartDrag,{passive:false});

floatingDropzone.addEventListener('click', ()=>fileInput.click());
['dragenter','dragover'].forEach(evt=>floatingDropzone.addEventListener(evt,e=>{e.preventDefault();floatingDropzone.classList.add('drag')}));
['dragleave','drop'].forEach(evt=>floatingDropzone.addEventListener(evt,e=>{e.preventDefault();floatingDropzone.classList.remove('drag')}));
floatingDropzone.addEventListener('drop', e=>{if(e.dataTransfer.files.length)handleFiles(e.dataTransfer.files); e.preventDefault();});

pickBtn.onclick=()=>fileInput.click();
dropzone.onclick=e=>{if(e.target===dropzone)fileInput.click()};
['dragenter','dragover'].forEach(evt=>dropzone.addEventListener(evt,e=>{e.preventDefault();dropzone.classList.add('drag')}));
['dragleave','drop'].forEach(evt=>dropzone.addEventListener(evt,e=>{e.preventDefault();dropzone.classList.remove('drag')}));
dropzone.addEventListener('drop',e=>{if(e.dataTransfer.files.length)handleFiles(e.dataTransfer.files)});
fileInput.addEventListener('change',()=>{if(fileInput.files.length)handleFiles(fileInput.files)});

const MATERIALS={
  'precimid-1176-pro-gf30':{label:'Precimid 1176 Pro GF30',density:1.3},
  'abs':{label:'ABS пластик',density:1.08},
  'petg':{label:'PETG пластик',density:1.3},
  'pla':{label:'PLA пластик',density:1.25},
  'nylon':{label:'Нейлон (Полиамид)',density:1.15},
  'pc':{label:'Поликарбонат (PC)',density:1.2}
};
let currentMaterialKey=Object.keys(MATERIALS)[0];
for(const key of Object.keys(MATERIALS)){const opt=document.createElement('option');opt.value=key;opt.textContent=MATERIALS[key].label+' (ρ='+MATERIALS[key].density+' г/см³)';materialSelect.appendChild(opt)}
materialSelect.value=currentMaterialKey;

const AUTHORS=[{code:'А',label:'Антон'},{code:'И',label:'Иван'},{code:'Д',label:'Другой'}];
for(const a of AUTHORS){const opt=document.createElement('option');opt.value=a.code;opt.textContent=a.label+' ('+a.code+')';authorSelect.appendChild(opt)}
authorSelect.value=AUTHORS[0].code;
let nextSerialNumber=1;

const DEFAULT_PRICE_PER_CM3 = 60;
const POSTPROC_PRICE_COEFFS = { 'Химическое окрашивание': 1.25 };

function formatMass(massG){if(massG===0)return'0.00 г';const abs=Math.abs(massG);if(abs>=1000)return(massG/1000).toFixed(2)+' кг';if(abs>=0.005)return massG.toFixed(2)+' г';return massG.toExponential(2)+' г'}
function formatVolume(v){if(v===0)return'0.00';const abs=Math.abs(v);if(abs>=0.005&&abs<1e9)return v.toFixed(2);return v.toExponential(2)}
function escapeHtml(s){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function escapeXml(s){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&apos;'}[c]))}
function safeFileNamePart(s){return String(s).replace(/[\\/:*?"<>|]/g,'_').trim()}
function getEffectiveVolume(entry){if(entry.modelEntry.manualVolumeEnabled&&entry.modelEntry.manualVolume>0)return entry.modelEntry.manualVolume;return entry.getVolumeCm3()}
function getEffectiveMass(entry){if(entry.modelEntry.manualMassEnabled&&entry.modelEntry.manualMass>0)return entry.modelEntry.manualMass;return getEffectiveVolume(entry)*MATERIALS[currentMaterialKey].density}
function getSelectedPostprocList(){return Array.from(reqPostprocList.querySelectorAll('input:checked')).map(cb=>cb.value);}
function getPostprocForPricing(){const list=getSelectedPostprocList();const nonSand=list.filter(p=>p!=='Пескоструйная обработка');return nonSand.length?nonSand[0]:null;}
function getPostprocCoefficient(name){if(!name)return 1;return POSTPROC_PRICE_COEFFS[name]||1;}

function updateCardPriceDisplay(entry){
  if(!entry||!entry.priceUnitEl||!entry.priceBatchEl)return;
  const volume=getEffectiveVolume(entry);
  const price=Number(entry.modelEntry.pricePerCm3)||0;
  const qty=Math.max(0,Math.round(Number(entry.modelEntry.quantity)||0));
  const unitPrice=volume*price;
  const batchPrice=unitPrice*qty;
  entry.priceUnitEl.textContent=unitPrice.toFixed(2)+' ₽';
  entry.priceBatchEl.textContent=batchPrice.toFixed(2)+' ₽';
  const pp=Number(entry.modelEntry.postprocPricePerCm3)||0;
  const ppUnit=volume*pp;
  const ppBatch=ppUnit*qty;
  if(entry.postprocPriceUnitEl)entry.postprocPriceUnitEl.textContent=ppUnit.toFixed(2)+' ₽';
  if(entry.postprocPriceBatchEl)entry.postprocPriceBatchEl.textContent=ppBatch.toFixed(2)+' ₽';
}
function updateAllCardsPostprocView(){
  const name=getPostprocForPricing();
  const coeff=getPostprocCoefficient(name);
  for(const entry of allCards){
    const root=entry.root;
    if(!root)continue;
    const block=root.querySelector('.price-postproc-block');
    const nameLabel=root.querySelector('.postproc-name-label');
    const ppInput=root.querySelector('.postproc-price-input');
    if(!block||!nameLabel||!ppInput)continue;
    if(name){
      block.style.display='block';
      nameLabel.textContent=name;
      if(!entry.modelEntry.postprocPriceManuallyEdited){
        const base=Number(entry.modelEntry.pricePerCm3)||0;
        const val=base*coeff;
        entry.modelEntry.postprocPricePerCm3=val;
        ppInput.value=val.toFixed(2);
      }
    } else {
      block.style.display='none';
      entry.modelEntry.postprocPricePerCm3=0;
    }
    updateCardPriceDisplay(entry);
  }
}
function updateFloatingInfo(){
  let totalVol=0,totalMass=0,totalQty=0,totalPrice=0,totalPostprocPrice=0;
  for(const entry of allCards){
    const v=getEffectiveVolume(entry);
    const m=getEffectiveMass(entry);
    const q=Math.max(0,Math.round(Number(entry.modelEntry.quantity)||0));
    const price=Number(entry.modelEntry.pricePerCm3)||0;
    const ppPrice=Number(entry.modelEntry.postprocPricePerCm3)||0;
    totalVol+=v*q;
    totalMass+=m*q;
    totalQty+=q;
    totalPrice+=v*price*q;
    totalPostprocPrice+=v*ppPrice*q;
  }
  floatingTotalVolumeEl.textContent=formatVolume(totalVol);
  floatingTotalMassEl.textContent=totalMass.toFixed(2);
  if(floatingTotalPriceEl)floatingTotalPriceEl.textContent=totalPrice.toFixed(2);
  const postprocName=getPostprocForPricing();
  const showPostproc=!!postprocName&&allCards.length>0;
  if(floatingTotalPostprocTitleEl)floatingTotalPostprocTitleEl.style.display=showPostproc?'':'none';
  if(floatingTotalPostprocValueEl)floatingTotalPostprocValueEl.style.display=showPostproc?'':'none';
  if(floatingTotalPostprocPriceEl)floatingTotalPostprocPriceEl.textContent=totalPostprocPrice.toFixed(2);
  if(floatingModelsCountEl)floatingModelsCountEl.textContent=String(allCards.length);
  if(floatingTotalPartsEl)floatingTotalPartsEl.textContent=String(totalQty);
  if(floatingPostprocEl){
    const list=getSelectedPostprocList();
    if(list.length){
      floatingPostprocEl.innerHTML=list.map(p=>'<div class="floating-info-postproc-item"><span class="check">✓</span><span>'+escapeHtml(p)+'</span></div>').join('');
    } else {
      floatingPostprocEl.innerHTML='<div class="floating-info-postproc-item"><span class="check">✓</span><span>—</span></div>';
    }
  }
}
function updateFloatingDateTime(){
  const now=new Date();
  if(floatingDateEl){
    const dd=String(now.getDate()).padStart(2,'0');
    const mm=String(now.getMonth()+1).padStart(2,'0');
    const yyyy=now.getFullYear();
    floatingDateEl.textContent=dd+'.'+mm+'.'+yyyy;
  }
  if(floatingTimeEl){
    const hh=String(now.getHours()).padStart(2,'0');
    const min=String(now.getMinutes()).padStart(2,'0');
    const ss=String(now.getSeconds()).padStart(2,'0');
    floatingTimeEl.textContent=hh+':'+min+':'+ss;
  }
}
setInterval(updateFloatingDateTime,1000);
updateFloatingDateTime();
function updateDisplay(entry){
    const volume=getEffectiveVolume(entry),mass=getEffectiveMass(entry),
    volumeText=formatVolume(volume)+' см³',massText=formatMass(mass);
    entry.volumeEl.textContent=volumeText;
    entry.massEl.textContent='Масса: '+massText;
    const real = entry.modelEntry.realVolumeCm3;
    let differs = false;
    if (real !== undefined && real !== null && Number.isFinite(real)) {
        if (Math.abs(real) < 1e-9 && Math.abs(volume) < 1e-9) {
            differs = false;
        } else {
            const relTol = 1e-6;
            const denom = Math.max(Math.abs(real), Math.abs(volume), 1e-12);
            differs = Math.abs(real - volume) / denom > relTol;
        }
    }
    entry.volumeEl.style.color = differs ? 'var(--err)' : '';
    entry.modelEntry.label=[volumeText,massText];
    renderIsometric(entry.modelEntry.canvas,entry.modelEntry.triangles,entry.modelEntry.label,entry.modelEntry.dimsCm,entry.modelEntry.nameLines,entry.modelEntry.rotation,entry.modelEntry);
    updateCardPriceDisplay(entry);
}
function updateRealVolumeDisplay(entry){const real=entry.modelEntry.realVolumeCm3;if(entry.realNoteEl)entry.realNoteEl.textContent='Реальный объём: '+formatVolume(real)+' см³'}
function attachManualInputs(card,modelEntry,entry){card.manualVolumeInput.addEventListener('input',()=>{const val=parseFloat(card.manualVolumeInput.value.trim());if(!isNaN(val)&&val>0){modelEntry.manualVolumeEnabled=true;modelEntry.manualVolume=val}else{modelEntry.manualVolumeEnabled=false;modelEntry.manualVolume=0}updateDisplay(entry);refreshRequestPreview()});card.manualMassInput.addEventListener('input',()=>{const val=parseFloat(card.manualMassInput.value.trim());if(!isNaN(val)&&val>0){modelEntry.manualMassEnabled=true;modelEntry.manualMass=val}else{modelEntry.manualMassEnabled=false;modelEntry.manualMass=0}updateDisplay(entry);refreshRequestPreview()})}
function attachPriceInput(card,modelEntry,entry){
  card.priceInput.addEventListener('input',()=>{
    const v=parseFloat(card.priceInput.value);
    modelEntry.pricePerCm3=Number.isFinite(v)&&v>=0?v:0;
    if(!modelEntry.postprocPriceManuallyEdited){
      const name=getPostprocForPricing();
      const coeff=getPostprocCoefficient(name);
      if(name){
        const newPp=modelEntry.pricePerCm3*coeff;
        modelEntry.postprocPricePerCm3=newPp;
        card.postprocPriceInput.value=newPp.toFixed(2);
      }
    }
    updateCardPriceDisplay(entry);
    refreshRequestPreview();
  });
  card.postprocPriceInput.addEventListener('input',()=>{
    const v=parseFloat(card.postprocPriceInput.value);
    modelEntry.postprocPricePerCm3=Number.isFinite(v)&&v>=0?v:0;
    modelEntry.postprocPriceManuallyEdited=true;
    updateCardPriceDisplay(entry);
    refreshRequestPreview();
  });
}
function addCard(name){
resultsPanel.style.display='';
const card=document.createElement('div');
card.className='card';
card.innerHTML='<div class="card-header"><div class="card-header-left"><span class="name">'+escapeHtml(name)+'</span><span class="acct-line"><label class="acct-number-label" title="Номер для бухгалтерии">№: <input type="text" class="acct-number-input" inputmode="text"></label><label class="author-label">Автор: <select class="author-select">'+AUTHORS.map(a=>'<option value="'+escapeHtml(a.code)+'">'+escapeHtml(a.label)+' ('+escapeHtml(a.code)+')</option>').join('')+'</select></label><label class="serial-label">№ в партии: <input type="text" class="serial-input" placeholder="напр. 005" inputmode="numeric"></label></span></div><div class="card-header-right"><span class="badge badge-warn status-badge">считаю…</span><button type="button" class="btn-delete" title="Удалить">✕</button></div></div><div class="card-body"><div class="preview-wrap"><canvas class="preview" width="900" height="695"></canvas><div class="preview-actions"><span class="preview-caption">перетащите, чтобы повернуть</span><div class="preview-btns"><button type="button" class="btn-reset" title="Сбросить вид">изометрия</button><button type="button" class="btn-zoom-reset" title="Сбросить зум">Сброс зума</button><button type="button" class="btn-download" disabled>Скачать фото</button><button type="button" class="btn-download-no-logo" disabled>Без лого</button><button type="button" class="btn-production" disabled>Фото для производства</button><button type="button" class="btn-hd" style="display:none;" title="Переключить качество предпросмотра">HD Качество</button></div></div></div><div class="volume-block"><div class="volume-value">…</div><div class="mass-value muted"></div><div class="qty-row"><label class="qty-label">Кол-во для заявки, шт: <input type="number" class="qty-input" min="1" step="1" value="1"></label></div><div class="manual-row"><label class="manual-label">Объём (см³): <input type="text" class="manual-volume-input" placeholder="авто"></label><label class="manual-label">Масса (г): <input type="text" class="manual-mass-input" placeholder="авто"></label></div><div class="price-row"><label class="price-label">Цена за 1 см³: <input type="number" class="price-per-cm3-input" min="0" step="0.01" value="'+DEFAULT_PRICE_PER_CM3+'"> ₽</label></div><div class="price-total">Цена за изделие: <b class="price-unit-value">0.00 ₽</b></div><div class="price-total">Цена за партию: <b class="price-batch-value">0.00 ₽</b></div><div class="price-postproc-block" style="display:none;"><div class="price-row"><label class="price-label">Цена за 1 см³ с пост-обработкой (<span class="postproc-name-label">—</span>): <input type="number" class="postproc-price-input" min="0" step="0.01" value="0"> ₽</label></div><div class="price-total">Цена за изделие с пост-обработкой: <b class="postproc-price-unit-value">0.00 ₽</b></div><div class="price-total">Цена за партию с пост-обработкой: <b class="postproc-price-batch-value">0.00 ₽</b></div></div><div class="volume-note muted"></div></div></div>';
const previewWrap=card.querySelector('.preview-wrap');
const clipDiv=document.createElement('div');clipDiv.className='clip-controls';
const toggleBtn=document.createElement('button');toggleBtn.type='button';toggleBtn.className='clip-toggle';toggleBtn.textContent='Включить разрез';clipDiv.appendChild(toggleBtn);
const labelAxis=document.createElement('label');labelAxis.appendChild(document.createTextNode('Ось: '));
const sel=document.createElement('select');sel.className='clip-axis';['x','y','z'].forEach(val=>{const opt=document.createElement('option');opt.value=val;opt.textContent=val.toUpperCase();sel.appendChild(opt)});labelAxis.appendChild(sel);clipDiv.appendChild(labelAxis);
const labelPos=document.createElement('label');labelPos.appendChild(document.createTextNode('Поз.: '));
const range=document.createElement('input');range.type='range';range.className='clip-position';range.min=0;range.max=1;range.step=0.01;range.value=0.5;
const span=document.createElement('span');span.className='clip-pos-value';span.textContent='0.50';labelPos.appendChild(range);labelPos.appendChild(span);clipDiv.appendChild(labelPos);
previewWrap.appendChild(clipDiv);
const realNoteEl=document.createElement('div');realNoteEl.className='real-volume-note muted';realNoteEl.style.marginTop='6px';previewWrap.appendChild(realNoteEl);
const hashNoteEl=document.createElement('div');hashNoteEl.className='hash-note muted';hashNoteEl.style.marginTop='2px';hashNoteEl.textContent='Хэш-сумма: вычисляется…';previewWrap.appendChild(hashNoteEl);
resultsBody.appendChild(card);
return{root:card,badge:card.querySelector('.status-badge'),canvas:card.querySelector('canvas.preview'),volumeEl:card.querySelector('.volume-value'),massEl:card.querySelector('.mass-value'),noteEl:card.querySelector('.volume-note'),downloadBtn:card.querySelector('.btn-download'),downloadNoLogoBtn:card.querySelector('.btn-download-no-logo'),productionBtn:card.querySelector('.btn-production'),hdBtn:card.querySelector('.btn-hd'),resetBtn:card.querySelector('.btn-reset'),zoomResetBtn:card.querySelector('.btn-zoom-reset'),deleteBtn:card.querySelector('.btn-delete'),acctInput:card.querySelector('.acct-number-input'),authorSelect:card.querySelector('.author-select'),serialInput:card.querySelector('.serial-input'),qtyInput:card.querySelector('.qty-input'),manualVolumeInput:card.querySelector('.manual-volume-input'),manualMassInput:card.querySelector('.manual-mass-input'),priceInput:card.querySelector('.price-per-cm3-input'),priceUnitEl:card.querySelector('.price-unit-value'),priceBatchEl:card.querySelector('.price-batch-value'),postprocBlock:card.querySelector('.price-postproc-block'),postprocNameLabel:card.querySelector('.postproc-name-label'),postprocPriceInput:card.querySelector('.postproc-price-input'),postprocPriceUnitEl:card.querySelector('.postproc-price-unit-value'),postprocPriceBatchEl:card.querySelector('.postproc-price-batch-value'),realNoteEl:realNoteEl,hashNoteEl:hashNoteEl,
clipToggle:toggleBtn,clipAxis:sel,clipPosition:range,clipPosValue:span}}
const LENGTH_TO_CM={mm:.1,cm:1,m:100,dm:10,km:100000,um:.0001,in:2.54};const UNIT_LABELS={mm:'мм',cm:'см',m:'м',dm:'дм',km:'км',um:'мкм',in:'дюймы'};function lengthUnitToCm(key){return LENGTH_TO_CM[key]??LENGTH_TO_CM.mm}function unitLabel(key){return UNIT_LABELS[key]||key}
function signedTetraVolume(v0,v1,v2){const cx=v1[1]*v2[2]-v1[2]*v2[1],cy=v1[2]*v2[0]-v1[0]*v2[2],cz=v1[0]*v2[1]-v1[1]*v2[0];return(v0[0]*cx+v0[1]*cy+v0[2]*cz)/6}
function isBinarySTL(buffer){if(buffer.byteLength<84)return false;const dv=new DataView(buffer);const n=dv.getUint32(80,true);return 84+50*n===buffer.byteLength}
function trianglesFromBinarySTL(buffer){const dv=new DataView(buffer),n=dv.getUint32(80,true),triangles=[];let offset=84;for(let i=0;i<n;i++){offset+=12;const v0=[dv.getFloat32(offset,true),dv.getFloat32(offset+4,true),dv.getFloat32(offset+8,true)];offset+=12;const v1=[dv.getFloat32(offset,true),dv.getFloat32(offset+4,true),dv.getFloat32(offset+8,true)];offset+=12;const v2=[dv.getFloat32(offset,true),dv.getFloat32(offset+4,true),dv.getFloat32(offset+8,true)];offset+=12;offset+=2;triangles.push([v0,v1,v2])}return triangles}
function trianglesFromAsciiSTL(text){const vertexRe=/vertex\s+([-\d.eE+]+)\s+([-\d.eE+]+)\s+([-\d.eE+]+)/g;let match,verts=[],triangles=[];while((match=vertexRe.exec(text))!==null){verts.push([parseFloat(match[1]),parseFloat(match[2]),parseFloat(match[3])]);if(verts.length===3){triangles.push(verts);verts=[]}}return triangles}
function trianglesFromStl(buffer){const triangles=isBinarySTL(buffer)?trianglesFromBinarySTL(buffer):trianglesFromAsciiSTL(new TextDecoder('utf-8').decode(buffer));if(triangles.length===0)throw new Error('В файле не найдено ни одного треугольника (это точно STL?)');return triangles}
function volumeOfTriangleSoup(triangles){let volume=0;for(const[v0,v1,v2]of triangles)volume+=signedTetraVolume(v0,v1,v2);return Math.abs(volume)}
function boundingBoxSize(triangles){let min=[Infinity,Infinity,Infinity],max=[-Infinity,-Infinity,-Infinity];for(const tri of triangles){for(const v of tri){for(let k=0;k<3;k++){if(v[k]<min[k])min[k]=v[k];if(v[k]>max[k])max[k]=v[k]}}}return{x:max[0]-min[0],y:max[1]-min[1],z:max[2]-min[2]}}
const MIN_SIDE_FOR_MESH_VOLUME_CM=1.5;function isThinPart(dimsCm){const minSide=Math.min(dimsCm.x,dimsCm.y,dimsCm.z);return minSide>0&&minSide<MIN_SIDE_FOR_MESH_VOLUME_CM}function effectiveVolumeCm3(meshVolumeCm3,dimsCm){return isThinPart(dimsCm)?dimsCm.x*dimsCm.y*dimsCm.z:meshVolumeCm3}
const OCCT_VERSION='0.0.23',OCCT_BASE='https://cdn.jsdelivr.net/npm/occt-import-js@'+OCCT_VERSION+'/dist/';let occtModulePromise=null;
function loadOcctScript(){return new Promise((resolve,reject)=>{if(typeof window.occtimportjs==='function'){resolve();return}const script=document.createElement('script');script.src=OCCT_BASE+'occt-import-js.js';script.onload=()=>resolve();script.onerror=()=>reject(new Error('Не удалось загрузить OpenCASCADE с CDN.'));document.head.appendChild(script)})}
async function getOcctModule(){if(!occtModulePromise){occtModulePromise=loadOcctScript().then(()=>window.occtimportjs({locateFile:path=>OCCT_BASE+path}))}return occtModulePromise}
function guessStepLengthUnitKey(text){const re=/SI_UNIT\s*\(\s*(\.\w+\.)?\s*,\s*\.(\w+)\.\s*\)/i;const m=re.exec(text);if(!m||m[2].toUpperCase()!=='METRE')return null;const prefix=(m[1]||'').replace(/\./g,'').toUpperCase();const map={MILLI:'mm',CENTI:'cm','':'m',DECI:'dm',KILO:'km',MICRO:'um'};return map[prefix]??null}
const STEP_TESSELLATION_PARAMS={linearDeflectionType:'bounding_box_ratio',linearDeflection:.0002,angularDeflection:.1};
async function trianglesAndVolumeFromStep(buffer){const occt=await getOcctModule();const fileBuffer=new Uint8Array(buffer);const result=occt.ReadStepFile(fileBuffer,STEP_TESSELLATION_PARAMS);if(!result||!result.success||!result.meshes)throw new Error('OpenCASCADE не смог разобрать STEP-файл');let totalVolume=0;const allTriangles=[];for(const mesh of result.meshes){const pos=mesh.attributes&&mesh.attributes.position&&mesh.attributes.position.array;const idx=mesh.index&&mesh.index.array;if(!pos||!idx)continue;let vol=0;for(let i=0;i<idx.length;i+=3){const a=idx[i]*3,b=idx[i+1]*3,c=idx[i+2]*3;const v0=[pos[a],pos[a+1],pos[a+2]],v1=[pos[b],pos[b+1],pos[b+2]],v2=[pos[c],pos[c+1],pos[c+2]];vol+=signedTetraVolume(v0,v1,v2);allTriangles.push([v0,v1,v2])}totalVolume+=Math.abs(vol)}if(totalVolume===0||allTriangles.length===0)throw new Error('Не удалось получить тесселяцию (0 треугольников)');return{triangles:allTriangles,volume:totalVolume}}
const ISO_SCALE=Math.sqrt(3/2),DEFAULT_AZIMUTH=Math.PI/4,DEFAULT_ELEVATION=Math.atan(1/Math.sqrt(2));
function isoProject(v,a,e){const aa=a==null?DEFAULT_AZIMUTH:a,ee=e==null?DEFAULT_ELEVATION:e;const ce=Math.cos(ee),se=Math.sin(ee);const cx=ce*Math.cos(aa),cy=ce*Math.sin(aa),cz=se;let rx=-cy,ry=cx,rz=0;const rlen=Math.hypot(rx,ry,rz);if(rlen<1e-6){rx=1;ry=0;rz=0}else{rx/=rlen;ry/=rlen;rz/=rlen}const ux=cy*rz-cz*ry,uy=cz*rx-cx*rz,uz=cx*ry-cy*rx;const x=v[0],y=v[1],z=v[2];const screenX=ISO_SCALE*(x*rx+y*ry+z*rz),screenY=ISO_SCALE*(x*ux+y*uy+z*uz),depth=x*cx+y*cy+z*cz;return[screenX,screenY,depth]}
function normalize3(v){const len=Math.hypot(v[0],v[1],v[2])||1;return[v[0]/len,v[1]/len,v[2]/len]}
const LIGHT_DIR=normalize3([.55,.8,.35]),FILL_DIR=normalize3([-.45,.25,-.6]);
function computeFullRenderScale(cssW,cssH){const dpr=window.devicePixelRatio||1;return Math.min(dpr,3)}
const SUPERSAMPLE_AA=2;
function computeFaceNormals(triangles){return triangles.map(([v0,v1,v2])=>{const e1=[v1[0]-v0[0],v1[1]-v0[1],v1[2]-v0[2]],e2=[v2[0]-v0[0],v2[1]-v0[1],v2[2]-v0[2]];let n=[e1[1]*e2[2]-e1[2]*e2[1],e1[2]*e2[0]-e1[0]*e2[2],e1[0]*e2[1]-e1[1]*e2[0]];const len=Math.hypot(n[0],n[1],n[2])||1;return[n[0]/len,n[1]/len,n[2]/len]})}
const SMOOTH_COS_THRESHOLD=Math.cos(40*Math.PI/180);
function buildSmoothVertexNormals(triangles,faceNormals){let min=[Infinity,Infinity,Infinity],max=[-Infinity,-Infinity,-Infinity];for(const tri of triangles){for(const v of tri){for(let k=0;k<3;k++){if(v[k]<min[k])min[k]=v[k];if(v[k]>max[k])max[k]=v[k]}}}const diag=Math.hypot(max[0]-min[0],max[1]-min[1],max[2]-min[2])||1;const invEps=1/(diag*2e-5);function keyOf(v){return Math.round(v[0]*invEps)+'_'+Math.round(v[1]*invEps)+'_'+Math.round(v[2]*invEps)}const posToFaces=new Map();triangles.forEach((tri,ti)=>{for(const v of tri){const k=keyOf(v);let arr=posToFaces.get(k);if(!arr){arr=[];posToFaces.set(k,arr)}if(arr[arr.length-1]!==ti)arr.push(ti)}});const cache=new Map();function vertexNormalFor(v,ti){const posKey=keyOf(v),cacheKey=posKey+'|'+ti;const cached=cache.get(cacheKey);if(cached)return cached;const myNormal=faceNormals[ti];const faces=posToFaces.get(posKey)||[ti];let sum=[0,0,0];for(const fj of faces){const n=faceNormals[fj];const dot=n[0]*myNormal[0]+n[1]*myNormal[1]+n[2]*myNormal[2];if(dot>=SMOOTH_COS_THRESHOLD){sum[0]+=n[0];sum[1]+=n[1];sum[2]+=n[2]}}const len=Math.hypot(sum[0],sum[1],sum[2])||1;const result=[sum[0]/len,sum[1]/len,sum[2]/len];cache.set(cacheKey,result);return result}return{vertexNormalFor}}
const modelNormalsCache=new WeakMap();
const LARGE_MESH_TRIANGLE_LIMIT = 80000;
function getModelNormals(triangles){
  let cached=modelNormalsCache.get(triangles);if(cached)return cached;
  const faceNormals=computeFaceNormals(triangles);
  let vertexNormals;
  if(triangles.length>LARGE_MESH_TRIANGLE_LIMIT){
    vertexNormals=triangles.map((tri,ti)=>{const n=faceNormals[ti];return[n,n,n]});
  }else{
    const{vertexNormalFor}=buildSmoothVertexNormals(triangles,faceNormals);
    vertexNormals=triangles.map((tri,ti)=>tri.map(v=>vertexNormalFor(v,ti)));
  }
  cached={faceNormals,vertexNormals};modelNormalsCache.set(triangles,cached);return cached;
}
function cameraDirFor(a,e){const aa=a==null?DEFAULT_AZIMUTH:a,ee=e==null?DEFAULT_ELEVATION:e;const ce=Math.cos(ee),se=Math.sin(ee);return[ce*Math.cos(aa),ce*Math.sin(aa),se]}
function shadeFromNormal(n,backface){if(backface){const key=Math.max(0,n[0]*LIGHT_DIR[0]+n[1]*LIGHT_DIR[1]+n[2]*LIGHT_DIR[2]);const fill=Math.max(0,n[0]*FILL_DIR[0]+n[1]*FILL_DIR[1]+n[2]*FILL_DIR[2]);return Math.min(1,.25+key*.35+fill*.15)}else{const key=Math.max(0,n[0]*LIGHT_DIR[0]+n[1]*LIGHT_DIR[1]+n[2]*LIGHT_DIR[2]);const fill=Math.max(0,n[0]*FILL_DIR[0]+n[1]*FILL_DIR[1]+n[2]*FILL_DIR[2]);return Math.min(1,.4+key*.5+fill*.18)}}
const canvasBuffersCache=new WeakMap();
function getFrameBuffers(canvas,ssW,ssH,pxW,pxH){let buf=canvasBuffersCache.get(canvas);if(!buf||buf.ssW!==ssW||buf.ssH!==ssH){buf={ssData:new Uint8ClampedArray(ssW*ssH*4),depthBuf:new Float32Array(ssW*ssH),ssW,ssH};canvasBuffersCache.set(canvas,buf)}if(!buf.imageData||buf.pxW!==pxW||buf.pxH!==pxH){buf.imageData=new ImageData(pxW,pxH);buf.pxW=pxW;buf.pxH=pxH}return buf}
const BOX_EDGES=(()=>{const edges=[];for(let a=0;a<8;a++){for(let b=a+1;b<8;b++){const diff=a^b;if(diff===1||diff===2||diff===4)edges.push([a,b])}}return edges})();
function clipTriangleByPlane(triangle, axis, cutCoord){const eps=1e-9;const verts=triangle;const signs=verts.map(v=>v[axis]-cutCoord);const allInside=signs.every(s=>s>=-eps);const allOutside=signs.every(s=>s<-eps);if(allInside)return[triangle];if(allOutside)return[];const resultVerts=[];const n=verts.length;for(let i=0;i<n;i++){const j=(i+1)%n;const si=signs[i],sj=signs[j];const vi=verts[i],vj=verts[j];if(si>=-eps)resultVerts.push(vi.slice());if((si>=-eps&&sj<-eps)||(si<-eps&&sj>=-eps)){const t=-si/(sj-si);resultVerts.push([vi[0]+t*(vj[0]-vi[0]),vi[1]+t*(vj[1]-vi[1]),vi[2]+t*(vj[2]-vi[2])])}}if(resultVerts.length<3)return[];if(resultVerts.length===3)return[[resultVerts[0],resultVerts[1],resultVerts[2]]];else if(resultVerts.length===4){return[[resultVerts[0],resultVerts[1],resultVerts[2]],[resultVerts[0],resultVerts[2],resultVerts[3]]]}else{const triList=[];for(let i=1;i<resultVerts.length-1;i++)triList.push([resultVerts[0],resultVerts[i],resultVerts[i+1]]);return triList}}
function simplifyTriangles(triangles, step){if(step<=1||triangles.length<=step)return triangles;const simplified=[];for(let i=0;i<triangles.length;i+=step)simplified.push(triangles[i]);return simplified}
function computeSimplifyStep(count){if(count<50000)return 1;if(count<200000)return 3;if(count<500000)return 5;return 8}

function buildLodMesh(triangles, targetTriangles){
  if(!triangles||triangles.length<=targetTriangles||targetTriangles<=0)return triangles;
  let min=[Infinity,Infinity,Infinity],max=[-Infinity,-Infinity,-Infinity];
  for(let i=0;i<triangles.length;i++){
    const t=triangles[i];
    for(let k=0;k<3;k++){
      const v=t[k];
      if(v[0]<min[0])min[0]=v[0]; if(v[0]>max[0])max[0]=v[0];
      if(v[1]<min[1])min[1]=v[1]; if(v[1]>max[1])max[1]=v[1];
      if(v[2]<min[2])min[2]=v[2]; if(v[2]>max[2])max[2]=v[2];
    }
  }
  const dx=max[0]-min[0],dy=max[1]-min[1],dz=max[2]-min[2];
  const diag=Math.hypot(dx,dy,dz)||1;
  let cells=Math.round(Math.cbrt(targetTriangles)*4);
  if(cells<16)cells=16; if(cells>200)cells=200;
  const invCell=cells/diag;
  const map=new Map();
  const clusters=[];
  function getCluster(v){
    const cx=Math.floor((v[0]-min[0])*invCell);
    const cy=Math.floor((v[1]-min[1])*invCell);
    const cz=Math.floor((v[2]-min[2])*invCell);
    const key=cx*65536+cy*256+cz;
    let idx=map.get(key);
    if(idx===undefined){
      idx=clusters.length;
      clusters.push({count:0,sx:0,sy:0,sz:0});
      map.set(key,idx);
    }
    const c=clusters[idx];
    c.count++; c.sx+=v[0]; c.sy+=v[1]; c.sz+=v[2];
    return idx;
  }
  const triClusters=new Array(triangles.length);
  for(let i=0;i<triangles.length;i++){
    const t=triangles[i];
    triClusters[i]=[getCluster(t[0]),getCluster(t[1]),getCluster(t[2])];
  }
  const centers=new Array(clusters.length);
  for(let i=0;i<clusters.length;i++){
    const c=clusters[i];
    centers[i]=[c.sx/c.count,c.sy/c.count,c.sz/c.count];
  }
  const used=new Set();
  const out=[];
  for(let i=0;i<triangles.length;i++){
    const t=triClusters[i];
    const a=t[0],b=t[1],c=t[2];
    if(a===b||b===c||a===c)continue;
    const s0=Math.min(a,b,c), s2=Math.max(a,b,c), s1=(a+b+c)-s0-s2;
    const key=s0*4194304+s1*2048+s2;
    if(used.has(key))continue;
    used.add(key);
    out.push([centers[a],centers[b],centers[c]]);
  }
  return out;
}

function renderIsometric(canvas,triangles,labelLines,dimsCm,nameLines,rotation,modelEntry,productionOpts){
if(modelEntry&&modelEntry.meshLod&&!productionOpts&&!modelEntry.hdMode){triangles=modelEntry.meshLod;}
const cssW=canvas.clientWidth||parseInt(canvas.getAttribute('width'),10)||260,cssH=canvas.clientHeight||parseInt(canvas.getAttribute('height'),10)||200;
const isDragging = modelEntry && modelEntry._dragging;
let scale, AA;
if (isDragging) {
  const dpr = window.devicePixelRatio || 1;
  scale = Math.min(dpr, 0.5);
  AA = 1;
} else {
  scale = computeFullRenderScale(cssW, cssH);
  AA = SUPERSAMPLE_AA;
}
const pxW=Math.max(1,Math.round(cssW*scale)),pxH=Math.max(1,Math.round(cssH*scale));
const ssW=pxW*AA,ssH=pxH*AA;
const az=rotation?rotation.azimuth:void 0,el=rotation?rotation.elevation:void 0;
canvas.width=pxW;canvas.height=pxH;
const ctx=canvas.getContext('2d');
ctx.setTransform(1,0,0,1,0,0);
const{ssData,depthBuf,imageData}=getFrameBuffers(canvas,ssW,ssH,pxW,pxH);
ssData.fill(255);depthBuf.fill(-Infinity);
let boxScreenCorners=null,labelPoints=null,fitScaleFinal=0;
let clipScreenCorners=null,clipCenter=null;
let clipEnabled=modelEntry&&modelEntry.clipEnabled;
let midX=0,midY=0;
if(triangles&&triangles.length){
  let min=[Infinity,Infinity,Infinity],max=[-Infinity,-Infinity,-Infinity];
  for(const tri of triangles){for(const v of tri){for(let k=0;k<3;k++){if(v[k]<min[k])min[k]=v[k];if(v[k]>max[k])max[k]=v[k]}}}
  const center=[(min[0]+max[0])/2,(min[1]+max[1])/2,(min[2]+max[2])/2];
  const hx=(max[0]-min[0])/2,hy=(max[1]-min[1])/2,hz=(max[2]-min[2])/2;
  let trianglesToRender=triangles;
  if(clipEnabled){
    const axisIdx={x:0,y:1,z:2}[modelEntry.clipAxis||'x'];
    const t=modelEntry.clipPosition||0.5;
    const minVal=min[axisIdx],maxVal=max[axisIdx];
    const cutCoord=minVal+t*(maxVal-minVal);
    const clipped=[];for(const tri of trianglesToRender){const result=clipTriangleByPlane(tri,axisIdx,cutCoord);for(const newTri of result)clipped.push(newTri);}
    trianglesToRender=clipped;
  }
  const renderTriangles=trianglesToRender;
  let faceNormals=[],vertexNormals=[];
  if(renderTriangles.length){const normalsData=getModelNormals(renderTriangles);faceNormals=normalsData.faceNormals;vertexNormals=normalsData.vertexNormals;}
  const camDir=cameraDirFor(az,el);
  const projected=[];
  for(let ti=0;ti<renderTriangles.length;ti++){
    const fn=faceNormals[ti];
    const backface=(fn&&fn[0]*camDir[0]+fn[1]*camDir[1]+fn[2]*camDir[2]<=0);
    if(!clipEnabled&&backface)continue;
    const tri=renderTriangles[ti];const vn=vertexNormals[ti];
    const pv=[0,1,2].map(vi=>{const v=tri[vi];const p=isoProject([v[0]-center[0],v[1]-center[1],v[2]-center[2]],az,el);const shade=clipEnabled&&backface?shadeFromNormal(vn[vi],true):shadeFromNormal(vn[vi],false);return{x:p[0],y:p[1],z:p[2],shade:shade}});
    projected.push(pv);
  }
  const boxProjected=[];for(let i=0;i<8;i++){const sx=i&4?hx:-hx,sy=i&2?hy:-hy,sz=i&1?hz:-hz;const p=isoProject([sx,sy,sz],az,el);boxProjected.push({x:p[0],y:p[1]});}
  let pMin=[Infinity,Infinity],pMax=[-Infinity,-Infinity];
  for(const p of boxProjected){if(p.x<pMin[0])pMin[0]=p.x;if(p.x>pMax[0])pMax[0]=p.x;if(p.y<pMin[1])pMin[1]=p.y;if(p.y>pMax[1])pMax[1]=p.y;}
  const spanX=(pMax[0]-pMin[0])||1,spanY=(pMax[1]-pMin[1])||1;
  const pad=30*scale*AA;
  const zoom=(modelEntry&&modelEntry.zoomFactor)?modelEntry.zoomFactor:1;
  const fitScale=Math.min((ssW-2*pad)/spanX,(ssH-2*pad)/spanY)*zoom;
  midX=(pMin[0]+pMax[0])/2;midY=(pMin[1]+pMax[1])/2;
  fitScaleFinal=fitScale/AA;
  function toScreen(p){return[ssW/2+(p.x-midX)*fitScale,ssH/2-(p.y-midY)*fitScale];}
  const base=[148,163,184];
  for(const tri of projected){
    const s0=toScreen(tri[0]),s1=toScreen(tri[1]),s2=toScreen(tri[2]);
    const x0=s0[0],y0=s0[1],x1=s1[0],y1=s1[1],x2=s2[0],y2=s2[1];
    const minX=Math.max(0,Math.floor(Math.min(x0,x1,x2))),maxX=Math.min(ssW-1,Math.ceil(Math.max(x0,x1,x2)));
    const minY=Math.max(0,Math.floor(Math.min(y0,y1,y2))),maxY=Math.min(ssH-1,Math.ceil(Math.max(y0,y1,y2)));
    if(minX>maxX||minY>maxY)continue;
    const v0x=x1-x0,v0y=y1-y0,v1x=x2-x0,v1y=y2-y0;
    const d00=v0x*v0x+v0y*v0y,d01=v0x*v1x+v0y*v1y,d11=v1x*v1x+v1y*v1y;
    const denom=d00*d11-d01*d01;if(Math.abs(denom)<1e-9)continue;
    const invDenom=1/denom;
    const z0=tri[0].z,z1=tri[1].z,z2=tri[2].z,sh0=tri[0].shade,sh1=tri[1].shade,sh2=tri[2].shade;
    for(let py=minY;py<=maxY;py++){const cy=py+0.5,rowOff=py*ssW;for(let px=minX;px<=maxX;px++){const cx=px+0.5;const v2x=cx-x0,v2y=cy-y0;const d20=v2x*v0x+v2y*v0y,d21=v2x*v1x+v2y*v1y;const v=(d11*d20-d01*d21)*invDenom;const w=(d00*d21-d01*d20)*invDenom;const w0=1-v-w;if(w0<-1e-4||v<-1e-4||w<-1e-4)continue;const z=w0*z0+v*z1+w*z2;const idx=rowOff+px;if(z<=depthBuf[idx])continue;depthBuf[idx]=z;const shade=w0*sh0+v*sh1+w*sh2;const di=idx*4;ssData[di]=base[0]*shade;ssData[di+1]=base[1]*shade;ssData[di+2]=base[2]*shade;ssData[di+3]=255;}}
  }
  if(clipEnabled&&triangles&&triangles.length){
    let minClip=[Infinity,Infinity,Infinity],maxClip=[-Infinity,-Infinity,-Infinity];
    for(const tri of triangles){for(const v of tri){for(let k=0;k<3;k++){if(v[k]<minClip[k])minClip[k]=v[k];if(v[k]>maxClip[k])maxClip[k]=v[k];}}}
    const axisIdx={x:0,y:1,z:2}[modelEntry.clipAxis||'x'];
    const t=modelEntry.clipPosition||0.5;
    const minVal=minClip[axisIdx],maxVal=maxClip[axisIdx];
    const cutCoord=minVal+t*(maxVal-minVal);
    const axes=[0,1,2];const otherAxes=axes.filter(a=>a!==axisIdx);const a1=otherAxes[0],a2=otherAxes[1];
    const corners3D=[[0,0,0],[0,0,0],[0,0,0],[0,0,0]];
    const vals1=[minClip[a1],maxClip[a1]],vals2=[minClip[a2],maxClip[a2]];
    let idx=0;for(let i=0;i<2;i++){for(let j=0;j<2;j++){const p=[0,0,0];p[axisIdx]=cutCoord-center[axisIdx];p[a1]=vals1[i]-center[a1];p[a2]=vals2[j]-center[a2];corners3D[idx++]=p;}}
    const pts=corners3D.map(p=>isoProject(p,az,el));
    const cx_=pts.reduce((s,p)=>s+p[0],0)/pts.length,cy_=pts.reduce((s,p)=>s+p[1],0)/pts.length;
    pts.sort((a,b)=>Math.atan2(a[1]-cy_,a[0]-cx_)-Math.atan2(b[1]-cy_,b[0]-cx_));
    const screenCorners=pts.map(p=>[pxW/2+(p[0]-midX)*fitScaleFinal,pxH/2-(p[1]-midY)*fitScaleFinal]);
    clipScreenCorners=screenCorners;
    const cx2=(screenCorners[0][0]+screenCorners[1][0]+screenCorners[2][0]+screenCorners[3][0])/4;
    const cy2=(screenCorners[0][1]+screenCorners[1][1]+screenCorners[2][1]+screenCorners[3][1])/4;
    clipCenter=[cx2,cy2];
  }
  function toScreenFinal(p){return[pxW/2+(p.x-midX)*fitScaleFinal,pxH/2-(p.y-midY)*fitScaleFinal];}
  boxScreenCorners=boxProjected.map(toScreenFinal);
  const topCorner=1,topPlusX=5,topPlusY=3,frontBottom=6,frontTop=7;
  const mid=(a,b)=>[(boxScreenCorners[a][0]+boxScreenCorners[b][0])/2,(boxScreenCorners[a][1]+boxScreenCorners[b][1])/2];
  labelPoints={x:mid(topCorner,topPlusX),y:mid(topCorner,topPlusY),z:mid(frontBottom,frontTop)};
}
const data=imageData.data;const norm=1/(AA*AA);
for(let fy=0;fy<pxH;fy++){for(let fx=0;fx<pxW;fx++){let r=0,g=0,b=0,a=0;for(let sy=0;sy<AA;sy++){let si=((fy*AA+sy)*ssW+fx*AA)*4;for(let sx=0;sx<AA;sx++,si+=4){r+=ssData[si];g+=ssData[si+1];b+=ssData[si+2];a+=ssData[si+3];}}const di=(fy*pxW+fx)*4;data[di]=r*norm;data[di+1]=g*norm;data[di+2]=b*norm;data[di+3]=a*norm;}}
ctx.putImageData(imageData,0,0);
if(clipEnabled&&clipScreenCorners){const sc=clipScreenCorners;ctx.save();ctx.beginPath();ctx.moveTo(sc[0][0],sc[0][1]);for(let i=1;i<4;i++)ctx.lineTo(sc[i][0],sc[i][1]);ctx.closePath();ctx.fillStyle='rgba(0,0,0,0.2)';ctx.fill();ctx.strokeStyle='#ff8c00';ctx.lineWidth=2.5*scale;ctx.stroke();if(clipCenter){ctx.fillStyle='#ff8c00';ctx.font='bold '+(15*scale)+'px sans-serif';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText('✂',clipCenter[0],clipCenter[1]);}ctx.restore();}
if(boxScreenCorners){ctx.save();ctx.strokeStyle='#f2c230';ctx.lineWidth=Math.max(1,.9*scale);for(const[a,b]of BOX_EDGES){ctx.beginPath();ctx.moveTo(boxScreenCorners[a][0],boxScreenCorners[a][1]);ctx.lineTo(boxScreenCorners[b][0],boxScreenCorners[b][1]);ctx.stroke();}ctx.restore();if(dimsCm){drawCenteredTag(ctx,labelPoints.x[0],labelPoints.x[1],dimsCm.x.toFixed(2)+' см',scale);drawCenteredTag(ctx,labelPoints.y[0],labelPoints.y[1],dimsCm.y.toFixed(2)+' см',scale);drawCenteredTag(ctx,labelPoints.z[0],labelPoints.z[1],dimsCm.z.toFixed(2)+' см',scale);}}
drawAxisGizmo(ctx,pxW,pxH,scale,az,el);
if(productionOpts){
    drawProductionLabels(ctx,pxW,pxH,modelEntry,productionOpts,scale);
} else {
    if(labelLines&&labelLines.length){drawVolumeLabel(ctx,pxW,pxH,Array.isArray(labelLines)?labelLines:[labelLines],scale);}
    if(nameLines&&nameLines.length){drawNameLabel(ctx,pxW,pxH,Array.isArray(nameLines)?nameLines:[nameLines],scale);}
}
if (modelEntry && modelEntry.realVolumeCm3 !== undefined && modelEntry.realVolumeCm3 !== null) {
    drawRealVolumeLabel(ctx, pxW, pxH, modelEntry.realVolumeCm3, scale, !!productionOpts);
}
if (!productionOpts && modelEntry && modelEntry.fileHash) {
    drawHashLabel(ctx, pxW, pxH, 'Хэш-сумма файла 3D-модели: ' + modelEntry.fileHash, scale);
}
}
function drawProductionLabels(ctx, pxW, pxH, modelEntry, opts, scale) {
    if (!modelEntry) return;
    const acct = displayedAcctNumber(modelEntry);
    const postprocList = Array.isArray(opts.postprocList) ? opts.postprocList : [];
    const lines = [
        modelEntry.fileName + ' / ' + acct,
        '',
        'Номер заявки: ' + (opts.orderNumber || '—'),
        'Дата заявки: ' + (opts.orderDate || '—'),
        'Покупатель: ' + (opts.customer || '—'),
        'Объем изделия: ' + formatVolume(opts.volumeCm3) + ' см³',
        'вес изделия: ' + formatMass(opts.massG),
        'Материал: ' + MATERIALS[currentMaterialKey].label,
        'кол-во в заявке: ' + opts.qty + ' шт',
        'Пост-обработка:'
    ];
    if (postprocList.length) {
        for (const p of postprocList) lines.push('  ✓ ' + p);
    } else {
        lines.push('  ✓ —');
    }
    const fontSize = 16 * scale;
    ctx.save();
    ctx.font = '600 ' + fontSize + 'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';
    const paddingX = 14 * scale;
    const paddingY = 10 * scale;
    const lineH = 24 * scale;
    const maxW = pxW * 0.72;
    const truncated = lines.map(line => line ? truncateToWidth(ctx, line, maxW) : '');
    let textWidth = 0;
    for (const line of truncated) textWidth = Math.max(textWidth, ctx.measureText(line).width);
    const boxW = textWidth + paddingX * 2;
    const boxH = paddingY * 2 + lineH * truncated.length;
    const x = 8 * scale, y = 8 * scale;
    ctx.fillStyle = 'rgba(17,17,20,0.72)';
    roundRectPath(ctx, x, y, boxW, boxH, 9 * scale);
    ctx.fill();
    ctx.fillStyle = '#fff';
    ctx.textBaseline = 'middle';
    truncated.forEach((line, i) => {
        if (line) ctx.fillText(line, x + paddingX, y + paddingY + lineH * i + lineH / 2 + 1 * scale);
    });
    ctx.restore();
}
function drawRealVolumeLabel(ctx, pxW, pxH, volume, scale, bottomOnly) {
    const text = volume.toFixed(2);
    const maxWidth = pxW - 20 * scale;
    let fontSize = 12 * scale;
    ctx.save();
    ctx.font = '600 ' + fontSize + 'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';
    while (ctx.measureText(text).width > maxWidth && fontSize > 6) {
        fontSize -= 0.5;
        ctx.font = '600 ' + fontSize + 'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';
    }
    const paddingX = 10 * scale;
    const paddingY = 4 * scale;
    const textWidth = ctx.measureText(text).width;
    const boxW = textWidth + paddingX * 2;
    const boxH = fontSize * 1.3 + paddingY * 2;
    const hashHeightEstimate = 22 * scale;
    const bottomOffset = 8 * scale;
    const gap = 4 * scale;
    const y = bottomOnly
        ? pxH - bottomOffset - boxH
        : pxH - bottomOffset - hashHeightEstimate - gap - boxH;
    const x = pxW - boxW - 8 * scale;
    ctx.fillStyle = 'rgba(17,17,20,0.72)';
    roundRectPath(ctx, x, y, boxW, boxH, 9 * scale);
    ctx.fill();
    ctx.fillStyle = '#fff';
    ctx.textBaseline = 'middle';
    ctx.fillText(text, x + paddingX, y + boxH / 2 + 1 * scale);
    ctx.restore();
}
function drawHashLabel(ctx, pxW, pxH, text, scale) {
    const maxWidth = pxW - 20 * scale;
    let fontSize = 14 * scale;
    ctx.save();
    ctx.font = '600 ' + fontSize + 'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';
    while (ctx.measureText(text).width > maxWidth && fontSize > 6) {
        fontSize -= 0.5;
        ctx.font = '600 ' + fontSize + 'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';
    }
    const paddingX = 10 * scale;
    const paddingY = 6 * scale;
    const textWidth = ctx.measureText(text).width;
    const boxW = textWidth + paddingX * 2;
    const boxH = fontSize * 1.4 + paddingY * 2;
    const x = pxW - boxW - 8 * scale;
    const y = pxH - boxH - 8 * scale;
    ctx.fillStyle = 'rgba(17,17,20,0.72)';
    roundRectPath(ctx, x, y, boxW, boxH, 9 * scale);
    ctx.fill();
    ctx.fillStyle = '#fff';
    ctx.textBaseline = 'middle';
    ctx.fillText(text, x + paddingX, y + boxH / 2 + 1 * scale);
    ctx.restore();
}
function roundRectPath(ctx,x,y,w,h,r){ctx.beginPath();ctx.moveTo(x+r,y);ctx.arcTo(x+w,y,x+w,y+h,r);ctx.arcTo(x+w,y+h,x,y+h,r);ctx.arcTo(x,y+h,x,y,r);ctx.arcTo(x,y,x+w,y,r);ctx.closePath()}
function drawVolumeLabel(ctx,pxW,pxH,lines,scale){ctx.save();ctx.font='600 '+(19.5*scale)+'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';const paddingX=12*scale,paddingY=9*scale,lineH=30*scale;let textWidth=0;for(const line of lines)textWidth=Math.max(textWidth,ctx.measureText(line).width);const boxW=textWidth+paddingX*2,boxH=paddingY*2+lineH*lines.length,x=8*scale,y=8*scale;ctx.fillStyle='rgba(17,17,20,0.72)';roundRectPath(ctx,x,y,boxW,boxH,9*scale);ctx.fill();ctx.fillStyle='#fff';ctx.textBaseline='middle';lines.forEach((line,i)=>{ctx.fillText(line,x+paddingX,y+paddingY+lineH*i+lineH/2+1*scale)});ctx.restore()}
function drawCenteredTag(ctx,cx,cy,text,scale){ctx.save();ctx.font='600 '+(19.5*scale)+'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';const paddingX=12*scale,boxH=36*scale;const textWidth=ctx.measureText(text).width,boxW=textWidth+paddingX*2,x=cx-boxW/2,y=cy-boxH/2;ctx.fillStyle='rgba(17,17,20,0.72)';roundRectPath(ctx,x,y,boxW,boxH,9*scale);ctx.fill();ctx.fillStyle='#fff';ctx.textBaseline='middle';ctx.fillText(text,x+paddingX,y+boxH/2+1.5*scale);ctx.restore()}
function truncateToWidth(ctx,text,maxWidth){if(ctx.measureText(text).width<=maxWidth)return text;let t=text;while(t.length>1&&ctx.measureText(t+'…').width>maxWidth)t=t.slice(0,-1);return t+'…'}
function drawNameLabel(ctx,pxW,pxH,lines,scale){ctx.save();ctx.font='600 '+(19.5*scale)+'px -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif';const paddingX=12*scale,paddingY=9*scale,lineH=30*scale,maxTextWidth=pxW*.55-paddingX*2;const truncated=lines.map(line=>truncateToWidth(ctx,line,maxTextWidth));let textWidth=0;for(const line of truncated)textWidth=Math.max(textWidth,ctx.measureText(line).width);const boxW=textWidth+paddingX*2,boxH=paddingY*2+lineH*truncated.length,x=pxW-8*scale-boxW,y=8*scale;ctx.fillStyle='rgba(17,17,20,0.72)';roundRectPath(ctx,x,y,boxW,boxH,9*scale);ctx.fill();ctx.fillStyle='#fff';ctx.textBaseline='middle';truncated.forEach((line,i)=>{ctx.fillText(line,x+paddingX,y+paddingY+lineH*i+lineH/2+1*scale)});ctx.restore()}
function drawAxisGizmo(ctx,pxW,pxH,scale,a,e){const origin=[26*scale,pxH-22*scale],len=20*scale;const axes=[{v:[1,0,0],color:'#e0524c',label:'X'},{v:[0,1,0],color:'#3aa15c',label:'Y'},{v:[0,0,1],color:'#3a7de0',label:'Z'}];ctx.save();ctx.font=(10*scale)+'px sans-serif';for(const ax of axes){const p=isoProject(ax.v,a,e);const dx=p[0]*len,dy=-p[1]*len;ctx.strokeStyle=ax.color;ctx.fillStyle=ax.color;ctx.lineWidth=1.6*scale;ctx.beginPath();ctx.moveTo(origin[0],origin[1]);ctx.lineTo(origin[0]+dx,origin[1]+dy);ctx.stroke();ctx.fillText(ax.label,origin[0]+dx*1.3-3*scale,origin[1]+dy*1.3+3*scale)}ctx.restore()}
function drawWatermark(ctx,pxW,pxH,scale){if(!logoImg.complete||logoImg.naturalWidth===0){ctx.save();ctx.globalAlpha=0.15;ctx.font='bold 48px Arial';ctx.fillStyle='#1a5276';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText('ЛОГО',pxW/2,pxH/2);ctx.restore();return;}const tileW=pxW*0.12;const tileH=tileW*(logoImg.naturalHeight/logoImg.naturalWidth);const stepX=tileW*1.8;const stepY=tileH*1.8;ctx.save();ctx.globalAlpha=0.15;let row=0;for(let y=-tileH;y<pxH+tileH;y+=stepY){const offsetX=row%2===0?0:stepX/2;for(let x=-tileW+offsetX;x<pxW+tileW;x+=stepX){ctx.drawImage(logoImg,x,y,tileW,tileH);}row++;}ctx.restore();}
async function canvasToJpegBlob(canvas){if(!logoImg.complete||logoImg.naturalWidth===0){await new Promise(r=>{logoImg.onload=r;logoImg.onerror=r;if(logoImg.complete)r();});}const ctx=canvas.getContext('2d');const backup=ctx.getImageData(0,0,canvas.width,canvas.height);drawWatermark(ctx,canvas.width,canvas.height,1);const outW=Math.round(canvas.clientWidth)||canvas.width;const outH=Math.round(canvas.clientHeight)||canvas.height;let exportCanvas=canvas;if(outW!==canvas.width||outH!==canvas.height){exportCanvas=document.createElement('canvas');exportCanvas.width=outW;exportCanvas.height=outH;exportCanvas.getContext('2d').drawImage(canvas,0,0,outW,outH);}const promise=new Promise(r=>{exportCanvas.toBlob(blob=>r(blob),'image/jpeg',0.92);});ctx.putImageData(backup,0,0);return promise;}
async function canvasToJpegBlobNoLogo(canvas){const ctx=canvas.getContext('2d');const backup=ctx.getImageData(0,0,canvas.width,canvas.height);const outW=Math.round(canvas.clientWidth)||canvas.width;const outH=Math.round(canvas.clientHeight)||canvas.height;let exportCanvas=canvas;if(outW!==canvas.width||outH!==canvas.height){exportCanvas=document.createElement('canvas');exportCanvas.width=outW;exportCanvas.height=outH;exportCanvas.getContext('2d').drawImage(canvas,0,0,outW,outH);}const promise=new Promise(r=>{exportCanvas.toBlob(blob=>r(blob),'image/jpeg',0.92);});ctx.putImageData(backup,0,0);return promise;}
function triggerBlobDownload(blob,filename){const url=URL.createObjectURL(blob),a=document.createElement('a');a.href=url;a.download=filename;document.body.appendChild(a);a.click();a.remove();setTimeout(()=>URL.revokeObjectURL(url),2e3);}
function downloadCanvasAsJpeg(canvas,filename,withLogo=true){const fn=withLogo?canvasToJpegBlob:canvasToJpegBlobNoLogo;fn(canvas).then(blob=>{if(blob)triggerBlobDownload(blob,filename);});}
function enableDownload(card,modelEntry){card.downloadBtn.disabled=false;card.downloadNoLogoBtn.disabled=false;card.downloadBtn.onclick=()=>downloadCanvasAsJpeg(card.canvas,buildDownloadFileName(modelEntry),true);card.downloadNoLogoBtn.onclick=()=>downloadCanvasAsJpeg(card.canvas,buildDownloadFileName(modelEntry),false);}
const ACCOUNTING_START_YEAR=2026;
function isoWeekInfo(date){const d=new Date(Date.UTC(date.getFullYear(),date.getMonth(),date.getDate()));const dayNum=(d.getUTCDay()+6)%7;d.setUTCDate(d.getUTCDate()-dayNum+3);const isoYear=d.getUTCFullYear();const firstThursday=new Date(Date.UTC(isoYear,0,4));const firstDayNum=(firstThursday.getUTCDay()+6)%7;firstThursday.setUTCDate(firstThursday.getUTCDate()-firstDayNum+3);const week=1+Math.round((d-firstThursday)/(7*24*3600*1e3));return{isoYear,week};}
function isoDayOfWeek(date){return((date.getDay()+6)%7)+1;}
function accountingDatePrefix(date){const{isoYear,week}=isoWeekInfo(date),day=isoDayOfWeek(date),workYear=isoYear-ACCOUNTING_START_YEAR+1;return workYear+'_'+week+'_'+day;}
function acctNumberText(modelEntry){const withAuthor=modelEntry.acctDatePrefix+'_'+modelEntry.author;return modelEntry.serial?withAuthor+'-'+modelEntry.serial:withAuthor;}
function displayedAcctNumber(modelEntry){return modelEntry.numberManuallyEdited&&modelEntry.manualNumber?modelEntry.manualNumber:acctNumberText(modelEntry);}
function baseNameNoExt(name){const idx=name.lastIndexOf('.');return idx>0?name.slice(0,idx):name;}
function describeModelLine(fileName,acctNumber){const ext=(fileName.split('.').pop()||'').toUpperCase();return 'Изделие пластмассовое -'+MATERIALS[currentMaterialKey].label+'-, '+ext+'-файл «'+baseNameNoExt(fileName)+'» / '+acctNumber;}
function buildDownloadFileName(modelEntry){
    const padded = String(modelEntry.orderIndex).padStart(3, '0');
    const base = baseNameNoExt(modelEntry.fileName);
    const acct = displayedAcctNumber(modelEntry);
    const ts = getTimestampSuffix();
    return padded + '_' + safeFileNamePart(base + '_' + acct) + ' (' + ts + ').jpg';
}
function buildProductionFileName(modelEntry){
    const padded = String(modelEntry.orderIndex).padStart(3, '0');
    const base = baseNameNoExt(modelEntry.fileName);
    const acct = displayedAcctNumber(modelEntry);
    const ts = getTimestampSuffix();
    return padded + '_PROD_' + safeFileNamePart(base + '_' + acct) + ' (' + ts + ').jpg';
}
function buildSourceFileName(modelEntry, ts) {
    const padded = String(modelEntry.orderIndex).padStart(3, '0');
    const orig = modelEntry.fileName;
    const dotIdx = orig.lastIndexOf('.');
    const stem = dotIdx > 0 ? orig.slice(0, dotIdx) : orig;
    const ext = dotIdx > 0 ? orig.slice(dotIdx) : '';
    return padded + '_' + safeFileNamePart(stem) + ' (' + ts + ')' + ext;
}
function getProductionOptsForEntry(entry) {
    const postprocList = getSelectedPostprocList();
    const data = collectRequestData();
    return {
        volumeCm3: getEffectiveVolume(entry),
        massG: getEffectiveMass(entry),
        qty: Math.max(0, Math.round(Number(entry.modelEntry.quantity) || 0)),
        postprocList: postprocList,
        orderNumber: data.orderNumber,
        orderDate: data.dateParts.ddmmyyyy,
        customer: data.customer
    };
}
async function renderProductionPhotoBlob(entry, opts) {
    const me = entry.modelEntry;
    const origCanvas = me.canvas;
    const cw = origCanvas.clientWidth || 900;
    const ch = origCanvas.clientHeight || 695;
    const tmpCanvas = document.createElement('canvas');
    tmpCanvas.width = origCanvas.width || 900;
    tmpCanvas.height = origCanvas.height || 695;
    tmpCanvas.style.cssText = 'position:fixed;left:-99999px;top:0;width:'+cw+'px;height:'+ch+'px;';
    document.body.appendChild(tmpCanvas);
    try {
        const productionOpts = opts || getProductionOptsForEntry(entry);
        renderIsometric(
            tmpCanvas,
            me.triangles,
            me.label,
            me.dimsCm,
            me.nameLines,
            me.rotation,
            me,
            productionOpts
        );
        return await new Promise(r => tmpCanvas.toBlob(b => r(b), 'image/jpeg', 0.92));
    } finally {
        document.body.removeChild(tmpCanvas);
    }
}
function enableProductionButton(card, entry) {
    card.productionBtn.disabled = false;
    card.productionBtn.onclick = async () => {
        const prev = card.productionBtn.textContent;
        card.productionBtn.disabled = true;
        card.productionBtn.innerHTML = '<span class="spinner"></span>';
        try {
            const blob = await renderProductionPhotoBlob(entry);
            if (blob) triggerBlobDownload(blob, buildProductionFileName(entry.modelEntry));
        } catch (err) {
            console.error(err);
            alert('Не удалось сформировать фото для производства: ' + (err.message || String(err)));
        } finally {
            card.productionBtn.disabled = false;
            card.productionBtn.textContent = prev;
        }
    };
}
function removeFromArray(arr,predicate){const idx=arr.findIndex(predicate);if(idx!==-1)arr.splice(idx,1);}
function attachSerialInput(card,modelEntry){if(!card.serialInput.value&&modelEntry.serial){card.serialInput.value=modelEntry.serial;}card.serialInput.addEventListener('input',function(){const val=this.value.trim();if(val){modelEntry.serialManuallyEdited=true;modelEntry.serial=val;}else{modelEntry.serialManuallyEdited=false;modelEntry.serial=modelEntry.autoSerial;this.value=modelEntry.autoSerial;}updateAcctDisplay(card,modelEntry);refreshRequestPreview();});}
function attachAuthorSelect(card,modelEntry){card.authorSelect.value=modelEntry.author;card.authorSelect.addEventListener('change',()=>{modelEntry.author=card.authorSelect.value;updateAcctDisplay(card,modelEntry);refreshRequestPreview();});}
function attachAcctNumberInput(card,modelEntry){card.acctInput.addEventListener('input',()=>{const typed=card.acctInput.value.trim();if(typed){modelEntry.numberManuallyEdited=true;modelEntry.manualNumber=typed;}else{modelEntry.numberManuallyEdited=false;modelEntry.manualNumber='';card.acctInput.value=acctNumberText(modelEntry);}card.acctInput.classList.toggle('manual',modelEntry.numberManuallyEdited);const acct=displayedAcctNumber(modelEntry);modelEntry.nameLines=[modelEntry.fileName,acct];renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);refreshRequestPreview();});}
function attachQtyInput(card,modelEntry,entry){card.qtyInput.addEventListener('input',()=>{const v=Math.round(Number(card.qtyInput.value));modelEntry.quantity=Number.isFinite(v)&&v>0?v:0;if(entry)updateCardPriceDisplay(entry);refreshRequestPreview();});}
function attachDeleteButton(card,modelEntry){card.deleteBtn.addEventListener('click',()=>{removeFromArray(renderedModels,e=>e===modelEntry);removeFromArray(stlCards,item=>item.modelEntry===modelEntry);removeFromArray(allCards,item=>item.modelEntry===modelEntry);card.root.remove();if(!resultsBody.children.length)resultsPanel.style.display='none';updateAllCardsPostprocView();refreshRequestPreview();});}
function updateAcctDisplay(card,modelEntry){const acct=displayedAcctNumber(modelEntry);if(!modelEntry.numberManuallyEdited)card.acctInput.value=acct;modelEntry.nameLines=[modelEntry.fileName,acct];renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);}
const allCards=[],stlCards=[],renderedModels=[];
function readAsArrayBuffer(file){return new Promise((resolve,reject)=>{const reader=new FileReader();reader.onload=()=>resolve(reader.result);reader.onerror=()=>reject(new Error('Не удалось прочитать файл'));reader.readAsArrayBuffer(file);});}
function setStatus(html){statusLine.style.display=html?'flex':'none';statusLine.innerHTML=html||'';}
function setCardErr(card,message){card.badge.className='badge badge-err status-badge';card.badge.textContent='ошибка';card.volumeEl.textContent='—';card.volumeEl.style.color='var(--muted)';card.noteEl.innerHTML='<span class="err-text">'+escapeHtml(message)+'</span>';}
function setCardOk(card,volumeCm3,noteText){card.badge.className='badge badge-ok status-badge';card.badge.textContent='готово';card.volumeEl.textContent=formatVolume(volumeCm3)+' см³';card.volumeEl.style.color='';card.noteEl.textContent=noteText;}
async function computeFileHash(buffer) {
    try {
        const hashBuffer = await crypto.subtle.digest('SHA-256', buffer);
        const hashArray = Array.from(new Uint8Array(hashBuffer));
        return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
    } catch (e) {
        console.warn('Ошибка вычисления SHA-256:', e);
        return null;
    }
}
function getThumbnailDataUrl(canvas, targetHeightPx = 200) {
    if (!canvas || canvas.width === 0 || canvas.height === 0) return null;
    const ratio = canvas.width / canvas.height;
    const targetW = Math.round(targetHeightPx * ratio);
    const tempCanvas = document.createElement('canvas');
    tempCanvas.width = targetW;
    tempCanvas.height = targetHeightPx;
    const ctx = tempCanvas.getContext('2d');
    ctx.drawImage(canvas, 0, 0, targetW, targetHeightPx);
    return tempCanvas.toDataURL('image/jpeg', 0.85);
}
function getTimestampSuffix() {
    const now = new Date();
    const dd = String(now.getDate()).padStart(2, '0');
    const mm = String(now.getMonth() + 1).padStart(2, '0');
    const yy = String(now.getFullYear()).slice(-2);
    const hh = String(now.getHours()).padStart(2, '0');
    const min = String(now.getMinutes()).padStart(2, '0');
    return dd + mm + yy + hh + min;
}
function attachHdButton(card,modelEntry){
    if(!card.hdBtn)return;
    modelEntry.hdMode=false;
    card.hdBtn.style.display='';
    card.hdBtn.onclick=()=>{
        if(card.hdBtn.disabled)return;
        modelEntry.hdMode=!modelEntry.hdMode;
        card.hdBtn.textContent=modelEntry.hdMode?'LQ Качество':'HD Качество';
        card.hdBtn.classList.toggle('hd-active',modelEntry.hdMode);
        card.hdBtn.disabled=true;
        setStatus('<span class="spinner"></span> '+(modelEntry.hdMode?'Рисую в HD-качестве, это может занять время…':'Возвращаюсь в быстрый LQ-режим…'));
        setTimeout(()=>{
            renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);
            card.hdBtn.disabled=false;
            setStatus('');
        },30);
    };
}
async function downloadPhotosZip(withLogo) {
    if (!allCards.length) return;
    const btn = withLogo ? reqPhotosWmBtn : reqPhotosNoWmBtn;
    const prev = btn.textContent;
    btn.disabled = true;
    btn.innerHTML = '<span class="spinner"></span> Собираю...';
    try {
        const usedNames = new Set();
        const files = [];
        const folderName = withLogo ? 'Фото с водяным знаком' : 'Фото без водяного знака';
        const fn = withLogo ? canvasToJpegBlob : canvasToJpegBlobNoLogo;
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const blob = await fn(me.canvas);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildDownloadFileName(me), usedNames);
                files.push({ name: folderName + '/' + name, data: buf });
            }
        }
        if (!files.length) {
            alert('Нет фотографий для архива.');
            btn.disabled = false;
            btn.textContent = prev;
            return;
        }
        const zip = await buildZip(files);
        const data = collectRequestData();
        const ts = getTimestampSuffix();
        const parts = [withLogo ? 'Фото с WM' : 'Фото без WM', data.orderNumber];
        if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) parts.push(data.customer);
        triggerBlobDownload(zip, safeFileNamePart(parts.join(' ')) + ' (' + ts + ').zip');
    } catch (err) {
        console.error(err);
        setStatus('Не удалось собрать архив фото: ' + escapeHtml(err.message || String(err)));
    } finally {
        btn.disabled = false;
        btn.textContent = prev;
    }
}
async function saveZipToFolder() {
    if (!allCards.length) {
        alert('Нет моделей для архива.');
        return;
    }
    if (!window.showDirectoryPicker) {
        alert('Ваш браузер не поддерживает выбор папки. Будет выполнено обычное скачивание.');
        await downloadAllZip();
        return;
    }
    const btn = document.getElementById('reqSaveToFolderBtn');
    const prev = btn.textContent;
    btn.disabled = true;
    btn.innerHTML = '<span class="spinner"></span> Собираю…';
    try {
        const dirHandle = await window.showDirectoryPicker();
        const files = [];
        const usedPhotoWm = new Set();
        const usedPhotoNoWm = new Set();
        const usedProd = new Set();
        const usedSrc = new Set();
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const blob = await canvasToJpegBlob(me.canvas);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildDownloadFileName(me), usedPhotoWm);
                files.push({ name: 'Фото с водяным знаком/' + name, data: buf });
            }
        }
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const blob = await canvasToJpegBlobNoLogo(me.canvas);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildDownloadFileName(me), usedPhotoNoWm);
                files.push({ name: 'Фото без водяного знака/' + name, data: buf });
            }
        }
        {
            const postprocList = getSelectedPostprocList();
            const reqData = collectRequestData();
            const commonOpts = {
                postprocList: postprocList,
                orderNumber: reqData.orderNumber,
                orderDate: reqData.dateParts.ddmmyyyy,
                customer: reqData.customer
            };
            for (const entry of allCards) {
                const me = entry.modelEntry;
                const opts = Object.assign({}, commonOpts, {
                    volumeCm3: getEffectiveVolume(entry),
                    massG: getEffectiveMass(entry),
                    qty: Math.max(0, Math.round(Number(me.quantity) || 0))
                });
                const blob = await renderProductionPhotoBlob(entry, opts);
                if (blob) {
                    const buf = new Uint8Array(await blob.arrayBuffer());
                    const name = dedupeZipName(buildProductionFileName(me), usedProd);
                    files.push({ name: 'Фото для производства/' + name, data: buf });
                }
            }
        }
        const ts = getTimestampSuffix();
        for (const entry of allCards) {
            const me = entry.modelEntry;
            if (me.originalBuffer) {
                const name = dedupeZipName(buildSourceFileName(me, ts), usedSrc);
                files.push({ name: 'Исходники/' + name, data: new Uint8Array(me.originalBuffer) });
            }
        }
        const data = collectRequestData();

        const docxNewBlob = await buildDocxBlob(data, false);
        const docParts = ['Заявка', data.orderNumber];
        if (data.dateParts.ddmmyyyy) docParts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) docParts.push(data.customer);
        files.push({ name: safeFileNamePart(docParts.join(' ')) + ' (' + ts + ').docx', data: new Uint8Array(await docxNewBlob.arrayBuffer()) });

        const docxOldBlob = await buildDocxBlob(data, true);
        const docPartsOld = ['Заявка (старая форма)', data.orderNumber];
        if (data.dateParts.ddmmyyyy) docPartsOld.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) docPartsOld.push(data.customer);
        files.push({ name: safeFileNamePart(docPartsOld.join(' ')) + ' (' + ts + ').docx', data: new Uint8Array(await docxOldBlob.arrayBuffer()) });

        const rows1C = allCards.map(entry => {
            const modelEntry = entry.modelEntry;
            const acctNumber = displayedAcctNumber(modelEntry);
            const name = describeModelLine(modelEntry.fileName, acctNumber);
            const weight = getEffectiveMass(entry);
            const volume = getEffectiveVolume(entry);
            return { name, weight, volume };
        });
        const xlsxBlob1C = await build1CXlsxBlob(rows1C);
        const xlsxBytes1C = new Uint8Array(await xlsxBlob1C.arrayBuffer());
        const hasOrder = !!(data.dateParts.yymmdd && reqDailyNumber.value.trim());
        const xls1CParts = ['Таблица 1С'];
        if (hasOrder) xls1CParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(xls1CParts.join(' ')) + ' (' + ts + ').xlsx', data: xlsxBytes1C });

        const rowsInv = collectInventoryRows();
        const invXlsxBlob = await buildInventoryXlsxBlob(rowsInv);
        const xlsParts = ['Таблица изделий'];
        if (hasOrder) xlsParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(xlsParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await invXlsxBlob.arrayBuffer()) });

        const priceXlsxBlob = await buildPriceCalculationXlsxBlob();
        const priceParts = ['Расчет Стоимости'];
        if (hasOrder) priceParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(priceParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await priceXlsxBlob.arrayBuffer()) });

        const prodRows = allCards.map((entry, idx) => {
            const modelEntry = entry.modelEntry;
            const acctNumber = displayedAcctNumber(modelEntry);
            const name = baseNameNoExt(modelEntry.fileName) + '/' + acctNumber;
            const weight = getEffectiveMass(entry);
            const qty = Math.max(0, Math.round(Number(modelEntry.quantity) || 0));
            const totalWeight = weight * qty;
            return { number: idx + 1, name, weight, qty, totalWeight };
        });
        const prodXlsxBlob = await buildProductionOrderXlsxBlob(prodRows);
        const prodParts = ['Заказ в производство'];
        if (hasOrder) prodParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(prodParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await prodXlsxBlob.arrayBuffer()) });

        const customerPart = data.customer ? data.customer.replace(/[^a-zA-Zа-яА-Я0-9\-_ ]/g, '').trim() : 'Без_покупателя';
        const orderPart = data.orderNumber || 'без_номера';
        const datePart = data.dateParts.ddmmyyyy || new Date().toLocaleDateString('ru-RU').replace(/\./g, '_');
        const zipName = safeFileNamePart(`${customerPart} _ ${orderPart} _ ${datePart} _ ${ts}`) + '.zip';
        const zipBlob = await buildZip(files);
        const zipArrayBuffer = await zipBlob.arrayBuffer();
        const fileHandle = await dirHandle.getFileHandle(zipName, { create: true });
        const writable = await fileHandle.createWritable();
        await writable.write(zipArrayBuffer);
        await writable.close();
        alert(`Архив успешно сохранён в папку:\n${dirHandle.name}\nИмя файла: ${zipName}`);
    } catch (err) {
        if (err.name !== 'AbortError') {
            console.error(err);
            alert('Не удалось сохранить архив: ' + err.message);
        }
    } finally {
        btn.disabled = false;
        btn.textContent = prev;
    }
}
async function downloadProductionPhotosZip() {
    if (!allCards.length) return;
    const btn = reqProdPhotosBtn;
    const prev = btn.textContent;
    btn.disabled = true;
    btn.innerHTML = '<span class="spinner"></span> Собираю...';
    try {
        const postprocList = getSelectedPostprocList();
        const usedNames = new Set();
        const files = [];
        const commonOpts = (() => {
            const reqData = collectRequestData();
            return {
                postprocList: postprocList,
                orderNumber: reqData.orderNumber,
                orderDate: reqData.dateParts.ddmmyyyy,
                customer: reqData.customer
            };
        })();
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const opts = Object.assign({}, commonOpts, {
                volumeCm3: getEffectiveVolume(entry),
                massG: getEffectiveMass(entry),
                qty: Math.max(0, Math.round(Number(me.quantity) || 0))
            });
            const blob = await renderProductionPhotoBlob(entry, opts);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildProductionFileName(me), usedNames);
                files.push({ name: name, data: buf });
            }
        }
        if (!files.length) {
            alert('Нет фотографий для архива.');
            btn.disabled = false;
            btn.textContent = prev;
            return;
        }
        const zip = await buildZip(files);
        const data = collectRequestData();
        const ts = getTimestampSuffix();
        const parts = ['Фото для производства'];
        if (data.orderNumber && !data.orderNumber.startsWith('……')) parts.push(data.orderNumber);
        if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) parts.push(data.customer);
        triggerBlobDownload(zip, safeFileNamePart(parts.join(' ')) + ' (' + ts + ').zip');
    } catch (err) {
        console.error(err);
        setStatus('Не удалось собрать архив фото для производства: ' + escapeHtml(err.message || String(err)));
    } finally {
        btn.disabled = false;
        btn.textContent = prev;
    }
}
reqProdPhotosBtn.addEventListener('click', downloadProductionPhotosZip);
document.getElementById('reqSaveToFolderBtn').addEventListener('click', saveZipToFolder);
async function handleFiles(fileList){setStatus('');for(const file of Array.from(fileList)){const card=addCard(file.name);const ext=file.name.toLowerCase().split('.').pop();try{const buffer=await readAsArrayBuffer(file);if(ext==='stl'){const triangles=trianglesFromStl(buffer);const useFast=fastLargeToggle.checked&&file.size>=10*1024*1024;let meshLod=null;if(useFast){setStatus('<span class="spinner"></span> Строю упрощённый меш для быстрого просмотра…');await new Promise(r=>setTimeout(r,0));meshLod=buildLodMesh(triangles,60000);setStatus('');}const rawVolume=volumeOfTriangleSoup(triangles),rawSize=boundingBoxSize(triangles);const unitKey=stlUnitSelect.value,unitCm=lengthUnitToCm(unitKey);const dimsCm={x:rawSize.x*unitCm,y:rawSize.y*unitCm,z:rawSize.z*unitCm};const volumeCm3=effectiveVolumeCm3(rawVolume*unitCm*unitCm*unitCm,dimsCm);const realVolumeCm3=rawVolume*unitCm*unitCm*unitCm;const thinNote=isThinPart(dimsCm)?' — тонкая деталь (мин. сторона < '+MIN_SIDE_FOR_MESH_VOLUME_CM+' см), объём по габаритам':'';setCardOk(card,volumeCm3,'из STL, единицы файла: '+unitLabel(unitKey)+thinNote);const labelText=card.volumeEl.textContent,acctDatePrefix=accountingDatePrefix(new Date());const trianglesCm=triangles.map(tri=>tri.map(v=>[v[0]*unitCm,v[1]*unitCm,v[2]*unitCm]));
const currentSerial = nextSerialNumber++;
const autoSerial=String(currentSerial);
const modelEntry={canvas:card.canvas,triangles,trianglesCm,meshLod,label:labelText,volumeText:labelText,dimsCm,fileName:file.name,serial:autoSerial,autoSerial:autoSerial,serialManuallyEdited:false,acctDatePrefix,author:authorSelect.value,numberManuallyEdited:false,manualNumber:'',nameLines:[],quantity:1,originalBuffer:buffer,rotation:{azimuth:DEFAULT_AZIMUTH,elevation:DEFAULT_ELEVATION},zoomFactor:1,realVolumeCm3,manualVolumeEnabled:false,manualVolume:0,manualMassEnabled:false,manualMass:0,pricePerCm3:DEFAULT_PRICE_PER_CM3,postprocPricePerCm3:0,postprocPriceManuallyEdited:false,clipEnabled:false,clipAxis:'x',clipPosition:0.5,_dragging:false,orderIndex:currentSerial,hdMode:false};
if(meshLod&&meshLod.length<triangles.length){const lodBadge=document.createElement('span');lodBadge.className='badge badge-warn';lodBadge.textContent='LOD';lodBadge.title='Упрощённый предпросмотр. Объём и масса — точные (по полному мешу).';const right=card.root.querySelector('.card-header-right');right.insertBefore(lodBadge,right.firstChild);attachHdButton(card,modelEntry);}
modelEntry.nameLines=[file.name,acctNumberText(modelEntry)];
card.acctInput.value=acctNumberText(modelEntry);card.serialInput.value=autoSerial;
renderedModels.push(modelEntry);
attachRotation(modelEntry,card);
attachSerialInput(card,modelEntry);attachAuthorSelect(card,modelEntry);attachAcctNumberInput(card,modelEntry);attachDeleteButton(card,modelEntry);
card.clipToggle.addEventListener('click',function(){modelEntry.clipEnabled=!modelEntry.clipEnabled;this.textContent=modelEntry.clipEnabled?'Выключить разрез':'Включить разрез';this.classList.toggle('active',modelEntry.clipEnabled);renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.clipAxis.addEventListener('change',function(){modelEntry.clipAxis=this.value;if(modelEntry.clipEnabled)renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.clipPosition.addEventListener('input',function(){const val=parseFloat(this.value);modelEntry.clipPosition=val;card.clipPosValue.textContent=val.toFixed(2);if(modelEntry.clipEnabled)renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.zoomResetBtn.addEventListener('click',function(){modelEntry.zoomFactor=1;renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
const stlEntry={root:card.root,rawVolume,rawSize,volumeEl:card.volumeEl,noteEl:card.noteEl,massEl:card.massEl,modelEntry,priceUnitEl:card.priceUnitEl,priceBatchEl:card.priceBatchEl,postprocPriceUnitEl:card.postprocPriceUnitEl,postprocPriceBatchEl:card.postprocPriceBatchEl,realNoteEl:card.realNoteEl,getVolumeCm3:()=>{const uc=lengthUnitToCm(stlUnitSelect.value),dims={x:rawSize.x*uc,y:rawSize.y*uc,z:rawSize.z*uc};return effectiveVolumeCm3(rawVolume*uc*uc*uc,dims);}};
stlCards.push(stlEntry);allCards.push(stlEntry);attachManualInputs(card,modelEntry,stlEntry);attachPriceInput(card,modelEntry,stlEntry);attachQtyInput(card,modelEntry,stlEntry);updateDisplay(stlEntry);updateRealVolumeDisplay(stlEntry);enableDownload(card,modelEntry);enableProductionButton(card,stlEntry);
await new Promise(r=>setTimeout(r,0));
const hash = await computeFileHash(buffer);
card.hashNoteEl.textContent = hash ? 'Хэш-сумма: ' + hash : 'Хэш-сумма: ошибка вычисления';
modelEntry.fileHash = hash;
renderIsometric(modelEntry.canvas, modelEntry.triangles, modelEntry.label, modelEntry.dimsCm, modelEntry.nameLines, modelEntry.rotation, modelEntry);
}else if(ext==='step'||ext==='stp'){setStatus('<span class="spinner"></span> Загружаю OpenCASCADE (WASM) с CDN...');const text=new TextDecoder('utf-8').decode(buffer);const detectedUnit=guessStepLengthUnitKey(text),unitKey=detectedUnit||'mm',unitCm=lengthUnitToCm(unitKey);const{triangles,volume:rawVolume}=await trianglesAndVolumeFromStep(buffer);const useFast=fastLargeToggle.checked&&file.size>=10*1024*1024;let meshLod=null;if(useFast){setStatus('<span class="spinner"></span> Строю упрощённый меш для быстрого просмотра…');await new Promise(r=>setTimeout(r,0));meshLod=buildLodMesh(triangles,60000);setStatus('');}const rawSize=boundingBoxSize(triangles),dimsCm={x:rawSize.x*unitCm,y:rawSize.y*unitCm,z:rawSize.z*unitCm};const volumeCm3=effectiveVolumeCm3(rawVolume*unitCm*unitCm*unitCm,dimsCm);const realVolumeCm3=rawVolume*unitCm*unitCm*unitCm;const thinNote=isThinPart(dimsCm)?' — тонкая деталь (мин. сторона < '+MIN_SIDE_FOR_MESH_VOLUME_CM+' см), объём по габаритам':'';const note=(detectedUnit?'из STEP, единицы по заголовку файла: '+unitLabel(unitKey):'из STEP, единицы не определены — предполагаем '+unitLabel(unitKey))+thinNote;setCardOk(card,volumeCm3,note);const labelText=card.volumeEl.textContent,acctDatePrefix=accountingDatePrefix(new Date());const trianglesCm=triangles.map(tri=>tri.map(v=>[v[0]*unitCm,v[1]*unitCm,v[2]*unitCm]));
const currentSerial = nextSerialNumber++;
const autoSerial=String(currentSerial);
const modelEntry={canvas:card.canvas,triangles,trianglesCm,meshLod,label:labelText,volumeText:labelText,dimsCm,fileName:file.name,serial:autoSerial,autoSerial:autoSerial,serialManuallyEdited:false,acctDatePrefix,author:authorSelect.value,numberManuallyEdited:false,manualNumber:'',nameLines:[],quantity:1,originalBuffer:buffer,rotation:{azimuth:DEFAULT_AZIMUTH,elevation:DEFAULT_ELEVATION},zoomFactor:1,realVolumeCm3,manualVolumeEnabled:false,manualVolume:0,manualMassEnabled:false,manualMass:0,pricePerCm3:DEFAULT_PRICE_PER_CM3,postprocPricePerCm3:0,postprocPriceManuallyEdited:false,clipEnabled:false,clipAxis:'x',clipPosition:0.5,_dragging:false,orderIndex:currentSerial,hdMode:false};
if(meshLod&&meshLod.length<triangles.length){const lodBadge=document.createElement('span');lodBadge.className='badge badge-warn';lodBadge.textContent='LOD';lodBadge.title='Упрощённый предпросмотр. Объём и масса — точные (по полному мешу).';const right=card.root.querySelector('.card-header-right');right.insertBefore(lodBadge,right.firstChild);attachHdButton(card,modelEntry);}
modelEntry.nameLines=[file.name,acctNumberText(modelEntry)];
card.acctInput.value=acctNumberText(modelEntry);card.serialInput.value=autoSerial;
renderedModels.push(modelEntry);
attachRotation(modelEntry,card);
attachSerialInput(card,modelEntry);attachAuthorSelect(card,modelEntry);attachAcctNumberInput(card,modelEntry);attachDeleteButton(card,modelEntry);
card.clipToggle.addEventListener('click',function(){modelEntry.clipEnabled=!modelEntry.clipEnabled;this.textContent=modelEntry.clipEnabled?'Выключить разрез':'Включить разрез';this.classList.toggle('active',modelEntry.clipEnabled);renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.clipAxis.addEventListener('change',function(){modelEntry.clipAxis=this.value;if(modelEntry.clipEnabled)renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.clipPosition.addEventListener('input',function(){const val=parseFloat(this.value);modelEntry.clipPosition=val;card.clipPosValue.textContent=val.toFixed(2);if(modelEntry.clipEnabled)renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
card.zoomResetBtn.addEventListener('click',function(){modelEntry.zoomFactor=1;renderIsometric(modelEntry.canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);});
const stepEntry={root:card.root,volumeEl:card.volumeEl,massEl:card.massEl,noteEl:card.noteEl,modelEntry:modelEntry,priceUnitEl:card.priceUnitEl,priceBatchEl:card.priceBatchEl,postprocPriceUnitEl:card.postprocPriceUnitEl,postprocPriceBatchEl:card.postprocPriceBatchEl,realNoteEl:card.realNoteEl,getVolumeCm3:()=>volumeCm3};
allCards.push(stepEntry);attachManualInputs(card,modelEntry,stepEntry);attachPriceInput(card,modelEntry,stepEntry);attachQtyInput(card,modelEntry,stepEntry);updateDisplay(stepEntry);updateRealVolumeDisplay(stepEntry);enableDownload(card,modelEntry);enableProductionButton(card,stepEntry);setStatus('');
await new Promise(r=>setTimeout(r,0));
const hash = await computeFileHash(buffer);
card.hashNoteEl.textContent = hash ? 'Хэш-сумма: ' + hash : 'Хэш-сумма: ошибка вычисления';
modelEntry.fileHash = hash;
renderIsometric(modelEntry.canvas, modelEntry.triangles, modelEntry.label, modelEntry.dimsCm, modelEntry.nameLines, modelEntry.rotation, modelEntry);
}else throw new Error('Неизвестное расширение — .stl, .step или .stp');
}catch(err){console.error(err);setCardErr(card,err.message||String(err));setStatus('');}}
fileInput.value='';updateAllCardsPostprocView();refreshRequestPreview();
}
const AZIMUTH_SENS=.01,ELEVATION_SENS=.01,ELEVATION_LIMIT=1.45;
function attachRotation(modelEntry,card){const canvas=card.canvas;let dragging=!1,lastX=0,lastY=0,rafPending=!1,pointerId=null;
function fullRender(){renderIsometric(canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);}
function draftRender(){modelEntry._dragging=true;renderIsometric(canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);}
function scheduleRender(){if(rafPending)return;rafPending=!0;requestAnimationFrame(()=>{rafPending=!1;if(dragging){draftRender();}else{modelEntry._dragging=false;fullRender();}});}
canvas.addEventListener('pointerdown',e=>{if(!modelEntry.triangles||!modelEntry.triangles.length)return;dragging=!0;pointerId=e.pointerId;try{canvas.setPointerCapture(pointerId);}catch(err){}lastX=e.clientX;lastY=e.clientY;canvas.classList.add('dragging');modelEntry._dragging=true;e.preventDefault();});
canvas.addEventListener('pointermove',e=>{if(!dragging)return;const dx=e.clientX-lastX,dy=e.clientY-lastY;lastX=e.clientX;lastY=e.clientY;modelEntry.rotation.azimuth-=dx*AZIMUTH_SENS;let el=modelEntry.rotation.elevation+dy*ELEVATION_SENS;el=Math.max(-ELEVATION_LIMIT,Math.min(ELEVATION_LIMIT,el));modelEntry.rotation.elevation=el;scheduleRender();});
function endDrag(){if(!dragging)return;dragging=!1;canvas.classList.remove('dragging');modelEntry._dragging=false;if(pointerId!=null)try{canvas.releasePointerCapture(pointerId);}catch(err){}scheduleRender();}
canvas.addEventListener('pointerup',endDrag);canvas.addEventListener('pointercancel',endDrag);
canvas.addEventListener('wheel',function(e){e.preventDefault();const delta=e.deltaY>0?-0.1:0.1;let newZoom=(modelEntry.zoomFactor||1)+delta;newZoom=Math.max(0.1,Math.min(10,newZoom));modelEntry.zoomFactor=newZoom;renderIsometric(canvas,modelEntry.triangles,modelEntry.label,modelEntry.dimsCm,modelEntry.nameLines,modelEntry.rotation,modelEntry);},{passive:false});
if(card.resetBtn){card.resetBtn.addEventListener('click',()=>{modelEntry.rotation.azimuth=DEFAULT_AZIMUTH;modelEntry.rotation.elevation=DEFAULT_ELEVATION;fullRender();});}
}
const POSTPROC_OPTIONS=['Пескоструйная обработка','Химическое окрашивание','Химическое травление','Механообработка','Убрать фаски','Галтовка','Шкурение','Окраска','Сглаживание','Лазерная гравировка'];
reqPostprocList.innerHTML=POSTPROC_OPTIONS.map(opt=>'<label class="postproc-label"><input type="checkbox" value="'+escapeHtml(opt)+'"> <span>'+escapeHtml(opt)+'</span></label>').join('');
reqPostprocList.addEventListener('change',e=>{if(e.target&&e.target.matches('input[type="checkbox"]')){const label=e.target.closest('.postproc-label');if(label)label.classList.toggle('checked',e.target.checked);updateAllCardsPostprocView();refreshRequestPreview();}});
function pad2(n){return String(n).padStart(2,'0');}
function todayDateInputValue(){const d=new Date();return d.getFullYear()+'-'+pad2(d.getMonth()+1)+'-'+pad2(d.getDate());}
reqDate.value=todayDateInputValue();
const RU_MONTHS_GEN=['января','февраля','марта','апреля','мая','июня','июля','августа','сентября','октября','ноября','декабря'];
function formatRequestDate(dateStr){if(!dateStr)return{yymmdd:'',ddmmyyyy:'',long:''};const[y,m,d]=dateStr.split('-').map(Number);if(!y||!m||!d)return{yymmdd:'',ddmmyyyy:'',long:''};return{yymmdd:String(y).slice(-2)+pad2(m)+pad2(d),ddmmyyyy:pad2(d)+'.'+pad2(m)+'.'+y,long:'«'+pad2(d)+'» '+RU_MONTHS_GEN[m-1]+' '+y+'г.'};}
function ruDaysWord(n){const mod100=Math.abs(n)%100,mod10=mod100%10;if(mod100>10&&mod100<20)return'дней';if(mod10===1)return'день';if(mod10>=2&&mod10<=4)return'дня';return'дней';}
function formatDeadline(daysStr){const n=Math.round(Number(daysStr));if(!daysStr||!Number.isFinite(n)||n<=0)return'';return'До '+n+' '+ruDaysWord(n);}
function collectRequestData() {
    const models = allCards.map(entry => {
        const fileName = entry.modelEntry.fileName;
        const ext = (fileName.split('.').pop() || '').toUpperCase();
        const qty = Math.max(0, Math.round(Number(entry.modelEntry.quantity) || 0));
        const acctNumber = displayedAcctNumber(entry.modelEntry);
        const volume = getEffectiveVolume(entry);
        const imageDataUrl = getThumbnailDataUrl(entry.modelEntry.canvas, 200);
        return { fileName, ext, qty, acctNumber, volumeCm3: volume, imageDataUrl };
    });
    const totalQty = models.reduce((s, m) => s + m.qty, 0);
    const totalVolume = models.reduce((s, m) => s + m.qty * m.volumeCm3, 0);
    const exts = Array.from(new Set(models.map(m => m.ext).filter(Boolean)));
    const postproc = Array.from(reqPostprocList.querySelectorAll('input:checked')).map(cb => cb.value);
    const dateParts = formatRequestDate(reqDate.value);
    const dailyNumber = reqDailyNumber.value.trim();
    const orderNumber = (dateParts.yymmdd || '……') + '-' + (dailyNumber || '…');
    return {models,totalQty,totalVolume,exts,postproc,dateParts,orderNumber,kp:reqKp.value.trim(),customer:reqCustomer.value.trim(),deadline:formatDeadline(reqDeadline.value),drawing:reqDrawing.value,tech:reqTech.value,material:MATERIALS[currentMaterialKey].label,notes:reqNotes.value.trim()};
}
function buildRequestInnerHtmlOld(data) {const modelLine = m => escapeHtml(describeModelLine(m.fileName, m.acctNumber)) + ' (' + m.qty + ' шт);';const modelsHtml = data.models.length ?'<p>Список изделий: <b>' + modelLine(data.models[0]) + '</b></p>' +data.models.slice(1).map(m => '<p class="req-model-line"><b>' + modelLine(m) + '</b></p>').join(''):'<p>Список изделий: <b>—</b></p>';const extText = data.exts.length ? '*. ' + data.exts.join(', *. ') : '—';const drawingHtml = data.drawing === 'ДА' ? '<span class="u">ДА</span>/НЕТ' : 'ДА/<span class="u">НЕТ</span>';const techHtml = data.tech === 'FDM' ? '<span class="u">FDM</span>/SLS' : 'FDM/<span class="u">SLS</span>';const postprocHtml = data.postproc.length ? '<ul>' + data.postproc.map(p => '<li>' + escapeHtml(p) + '</li>').join('') + '</ul>' : '<ul><li>—</li></ul>';const notesHtml = data.notes ? ' ' + escapeHtml(data.notes) : '';const customerHtml = escapeHtml(data.customer || '…');return '<img class="req-logo" src="' + LETTERHEAD_LOGO_DATA_URI + '" alt="Логотип">' +'<p class="req-title">Заявка № ' + escapeHtml(data.orderNumber) + ' по КП ' + escapeHtml(data.kp || '…') + ' от ' + (data.dateParts.ddmmyyyy || '…') + '</p>' +'<table class="req-dateline"><tr><td>г. Москва</td><td style="text-align:right;">' + (data.dateParts.long || '') + '</td></tr></table>' +'<p>Наименование Покупателя (Заказчика): <b>' + customerHtml + '</b></p>' +'<p>Срок изготовления изделий (не позднее): ' + escapeHtml(data.deadline || '…') + '</p>' +modelsHtml +'<p>Формат 3D модели: <b>' + extText + '</b>;</p>' +'<p>Общее количество изделий: ' + data.totalQty + ' шт.</p>' +'<p>Наличие чертежа 2D* (нужное подчеркнуть): ' + drawingHtml + '</p>' +'<p>Технология печати (нужное подчеркнуть): ' + techHtml + '</p>' +'<p>Материал: <b>' + escapeHtml(data.material) + '</b></p>' +'<p>Пост-обработка:</p>' + postprocHtml +'<p>Дополнительная информация: Объем изделий по заказу ' + formatVolume(data.totalVolume) + ' см3;' + notesHtml + '</p>' +'<table class="req-sign"><tr><td><span class="u">Утверждаю</span></td><td><span class="u">Согласовано</span></td></tr>' +'<tr><td>Исполнитель:<br>ООО «Аврора Карбон Технолоджи»<br>Генеральный директор</td><td>Заказчик:<br>' + customerHtml + '</td></tr>' +'<tr><td class="req-sign-line">____________________П.Б. Лустин</td><td class="req-sign-line">____________________</td></tr></table>' +'<div class="req-footnotes"><p>* без предоставления 2D чертежа, точность изготовления изделия не гарантируется</p>' +'<p>** если материал изделия неизвестен, необходимо проведение анализа и подбор материала для печати в соответствии с требованиями Заказчика. При отказе от услуги по подбору материала, после согласования марки материала, в случае деформации изделия в процессе тестирования или эксплуатации, претензии к качеству изготовления изделия не принимаются.</p>' +'<p>*** Заказчик гарантирует (и несет полную ответственность за нарушение данного обязательства), что все предоставленные им информационные материалы, требуемые для выполнения работ, в рамках данного договора свободны от претензий третьих лиц и факт их передачи Исполнителю не нарушает патентных, авторских и смежных прав, а также прав на коммерческую тайну третьих лиц.</p>' +'<p>**** Указанные в настоящей заявке модели изделий являются изделиями общепромышленного назначения</p></div>';}
function buildRequestInnerHtml(data) {let tableRows = '';if (data.models.length) {tableRows = data.models.map(m => {const nameCell = escapeHtml(describeModelLine(m.fileName, m.acctNumber)) + ' (' + m.qty + ' шт);';const imgHtml = m.imageDataUrl? `<img src="${m.imageDataUrl}" style="height:3cm; width:auto; max-width:100%;" alt="${escapeHtml(m.fileName)}">`: '';return `<tr><td style="padding:4px 8px;">${nameCell}</td><td style="text-align:center; padding:4px 8px;">${imgHtml}</td></tr>`;}).join('');} else {tableRows = `<tr><td colspan="2" style="padding:4px 8px;">—</td></tr>`;}const modelsHtml = `<table style="width:100%; border-collapse:collapse; margin:8px 0; font-size:inherit;"><tr><th style="text-align:left; border-bottom:1px solid #ccc; padding:4px 8px;">Наименование</th><th style="text-align:center; border-bottom:1px solid #ccc; padding:4px 8px;">Изображение</th></tr>${tableRows}</table>`;const extText = data.exts.length ? '*. ' + data.exts.join(', *. ') : '—';const drawingHtml = data.drawing === 'ДА' ? '<span class="u">ДА</span>/НЕТ' : 'ДА/<span class="u">НЕТ</span>';const techHtml = data.tech === 'FDM' ? '<span class="u">FDM</span>/SLS' : 'FDM/<span class="u">SLS</span>';const postprocHtml = data.postproc.length ? '<ul>' + data.postproc.map(p => '<li>' + escapeHtml(p) + '</li>').join('') + '</ul>' : '<ul><li>—</li></ul>';const notesHtml = data.notes ? ' ' + escapeHtml(data.notes) : '';const customerHtml = escapeHtml(data.customer || '…');return '<img class="req-logo" src="' + LETTERHEAD_LOGO_DATA_URI + '" alt="Логотип">' +'<p class="req-title">Заявка № ' + escapeHtml(data.orderNumber) + ' по КП ' + escapeHtml(data.kp || '…') + ' от ' + (data.dateParts.ddmmyyyy || '…') + '</p>' +'<table class="req-dateline"><tr><td>г. Москва</td><td style="text-align:right;">' + (data.dateParts.long || '') + '</td></tr></table>' +'<p>Наименование Покупателя (Заказчика): <b>' + customerHtml + '</b></p>' +'<p>Срок изготовления изделий (не позднее): ' + escapeHtml(data.deadline || '…') + '</p>' +modelsHtml +'<p>Формат 3D модели: <b>' + extText + '</b>;</p>' +'<p>Общее количество изделий: ' + data.totalQty + ' шт.</p>' +'<p>Наличие чертежа 2D* (нужное подчеркнуть): ' + drawingHtml + '</p>' +'<p>Технология печати (нужное подчеркнуть): ' + techHtml + '</p>' +'<p>Материал: <b>' + escapeHtml(data.material) + '</b></p>' +'<p>Пост-обработка:</p>' + postprocHtml +'<p>Дополнительная информация: Объем изделий по заказу ' + formatVolume(data.totalVolume) + ' см3;' + notesHtml + '</p>' +'<table class="req-sign"><tr><td><span class="u">Утверждаю</span></td><td><span class="u">Согласовано</span></td></tr>' +'<tr><td>Исполнитель:<br>ООО «Аврора Карбон Технолоджи»<br>Генеральный директор</td><td>Заказчик:<br>' + customerHtml + '</td></tr>' +'<tr><td class="req-sign-line">____________________П.Б. Лустин</td><td class="req-sign-line">____________________</td></tr></table>' +'<div class="req-footnotes"><p>* без предоставления 2D чертежа, точность изготовления изделия не гарантируется</p>' +'<p>** если материал изделия неизвестен, необходимо проведение анализа и подбор материала для печати в соответствии с требованиями Заказчика. При отказе от услуги по подбору материала, после согласования марки материала, в случае деформации изделия в процессе тестирования или эксплуатации, претензии к качеству изготовления изделия не принимаются.</p>' +'<p>*** Заказчик гарантирует (и несет полную ответственность за нарушение данного обязательства), что все предоставленные им информационные материалы, требуемые для выполнения работ, в рамках данного договора свободны от претензий третьих лиц и факт их передачи Исполнителю не нарушает патентных, авторских и смежных прав, а также прав на коммерческую тайну третьих лиц.</p>' +'<p>**** Указанные в настоящей заявке модели изделий являются изделиями общепромышленного назначения</p></div>';}

// ============================================================
// === УТИЛИТЫ ДЛЯ XLSX / DOCX ===
// ============================================================
function dataUrlToBytes(dataUrl) {
    const base64 = String(dataUrl).split(',')[1] || '';
    const binary = atob(base64);
    const bytes = new Uint8Array(binary.length);
    for (let i = 0; i < binary.length; i++) bytes[i] = binary.charCodeAt(i);
    return bytes;
}
function colLetter(n){let s='';while(n>0){const r=(n-1)%26;s=String.fromCharCode(65+r)+s;n=Math.floor((n-1)/26);}return s;}

async function buildGenericXlsxBlob(opts) {
    const {
        sheetName = 'Sheet1',
        headers,
        rows,
        colWidths,
        yellowIndices = [],
        hiddenCols = [],
        headerBg = 'FFEFEFEF',
        yellowBg = 'FFFFFF00',
        fontSize = 10
    } = opts;
    const N = headers.length;
    let sheetData = '';
    sheetData += '<row r="1" ht="15" customHeight="1">';
    for (let i = 1; i <= N; i++) sheetData += `<c r="${colLetter(i)}1" s="3"/>`;
    sheetData += '</row>';
    sheetData += '<row r="2" ht="40" customHeight="1">';
    headers.forEach((h, i) => {
        const s = yellowIndices.includes(i) ? '1' : '2';
        sheetData += `<c r="${colLetter(i+1)}2" t="inlineStr" s="${s}"><is><t xml:space="preserve">${escapeXml(h)}</t></is></c>`;
    });
    sheetData += '</row>';
    rows.forEach((row, ri) => {
        const rowNum = ri + 3;
        sheetData += `<row r="${rowNum}">`;
        row.forEach((val, i) => {
            const s = yellowIndices.includes(i) ? '1' : '0';
            if (typeof val === 'number' && Number.isFinite(val)) {
                sheetData += `<c r="${colLetter(i+1)}${rowNum}" s="${s}"><v>${val}</v></c>`;
            } else {
                const t = String(val == null ? '' : val);
                if (t === '') sheetData += `<c r="${colLetter(i+1)}${rowNum}" s="${s}"/>`;
                else sheetData += `<c r="${colLetter(i+1)}${rowNum}" t="inlineStr" s="${s}"><is><t xml:space="preserve">${escapeXml(t)}</t></is></c>`;
            }
        });
        sheetData += '</row>';
    });
    let colsXml = '<cols>';
    colWidths.forEach((w, i) => {
        const hid = hiddenCols.includes(i) ? ' hidden="1"' : '';
        colsXml += `<col min="${i+1}" max="${i+1}" width="${w}" customWidth="1"${hid}/>`;
    });
    colsXml += '</cols>';
    const sheetXml = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><worksheet xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main">' + colsXml + '<sheetData>' + sheetData + '</sheetData></worksheet>';
    const contentTypes = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types"><Default Extension="rels" ContentType="application/vnd.openxmlformats-package.relationships+xml"/><Default Extension="xml" ContentType="application/xml"/><Override PartName="/xl/workbook.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet.main+xml"/><Override PartName="/xl/worksheets/sheet1.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.worksheet+xml"/><Override PartName="/xl/styles.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.styles+xml"/></Types>';
    const rels = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships"><Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument" Target="xl/workbook.xml"/></Relationships>';
    const workbook = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><workbook xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main" xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships"><sheets><sheet name="'+escapeXml(sheetName)+'" sheetId="1" r:id="rId1"/></sheets></workbook>';
    const workbookRels = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships"><Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/worksheet" Target="worksheets/sheet1.xml"/><Relationship Id="rId2" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles" Target="styles.xml"/></Relationships>';
    const styles = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?><styleSheet xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main">'
        + '<fonts count="2"><font><sz val="'+fontSize+'"/><name val="Calibri"/></font><font><b/><sz val="'+fontSize+'"/><name val="Calibri"/></font></fonts>'
        + '<fills count="4"><fill><patternFill patternType="none"/></fill><fill><patternFill patternType="gray125"/></fill><fill><patternFill patternType="solid"><fgColor rgb="'+headerBg+'"/><bgColor indexed="64"/></patternFill></fill><fill><patternFill patternType="solid"><fgColor rgb="'+yellowBg+'"/><bgColor indexed="64"/></patternFill></fill></fills>'
        + '<borders count="1"><border><left/><right/><top/><bottom/><diagonal/></border></borders>'
        + '<cellStyleXfs count="1"><xf numFmtId="0" fontId="0" fillId="0" borderId="0"/></cellStyleXfs>'
        + '<cellXfs count="4">'
        +   '<xf numFmtId="0" fontId="0" fillId="0" borderId="0" xfId="0" applyFont="1" applyAlignment="1"><alignment wrapText="1" vertical="top"/></xf>'
        +   '<xf numFmtId="0" fontId="0" fillId="3" borderId="0" xfId="0" applyFill="1" applyFont="1" applyAlignment="1"><alignment wrapText="1" vertical="top"/></xf>'
        +   '<xf numFmtId="0" fontId="1" fillId="2" borderId="0" xfId="0" applyFont="1" applyFill="1" applyAlignment="1"><alignment wrapText="1" vertical="top"/></xf>'
        +   '<xf numFmtId="0" fontId="0" fillId="0" borderId="0" xfId="0" applyAlignment="1"><alignment vertical="top"/></xf>'
        + '</cellXfs>'
        + '<cellStyles count="1"><cellStyle name="Normal" xfId="0" builtinId="0"/></cellStyles>'
        + '</styleSheet>';
    return await buildZip([
        { name: '[Content_Types].xml', data: utf8Bytes(contentTypes) },
        { name: '_rels/.rels', data: utf8Bytes(rels) },
        { name: 'xl/workbook.xml', data: utf8Bytes(workbook) },
        { name: 'xl/_rels/workbook.xml.rels', data: utf8Bytes(workbookRels) },
        { name: 'xl/styles.xml', data: utf8Bytes(styles) },
        { name: 'xl/worksheets/sheet1.xml', data: utf8Bytes(sheetXml) },
    ]);
}

async function build1CXlsxBlob(rows) {
  const headers = ['Наименование','Группа','Единица хранения','Артикул','Оформление продажи','Единица измерения веса','Вес (знаменатель)','Вес','Вес можно указывать в документах','Вес (числитель)','Вид номенклатуры','Группа доступа','Наименование для печати','Использование характеристик','Группа настроек фин. учета','Ставка НДС','Тип номенклатуры','Файл картинки','Единица измерения объема','Объем (знаменатель)','Объем','Объем можно указывать в документах','Объем (числитель)','Схема обеспечения','Обособленное обеспечение','Группа аналитического учета','Код ОКПД2','Страна происхождения'];
  const yellowIndices = [0, 3, 9, 22, 26];
  const HIDDEN_COL = 25;
  const colWidths = [27,18,8,12,20,9,7,6,9,7,18,10,27,14,18,8,10,12,10,7,6,9,7,14,7,10,14,10];
  const bodyRows = rows.map(r => [
    r.name,
    'Готовая продукция (3D печать)',
    'шт',
    '',
    'Реализация товаров и услуг',
    'г',
    '1',
    'Да',
    'да',
    parseFloat(r.weight.toFixed(2)),
    'ГОТОВАЯ ПРОДУКЦИЯ_2026',
    'Прочее',
    r.name,
    'Не используются',
    'Готовая продукция (43)',
    '22%',
    'Товар',
    '',
    'см3 (мл)',
    '1',
    'Да',
    'Да',
    parseFloat(r.volume.toFixed(2)),
    'Готовая продукция',
    'Да',
    '',
    '22.29.29.190',
    'РОССИЯ'
  ]);
  return await buildGenericXlsxBlob({ sheetName: 'Номенклатура', headers, rows: bodyRows, colWidths, yellowIndices, hiddenCols: [HIDDEN_COL], fontSize: 10 });
}

async function buildInventoryXlsxBlob(rows) {
    const headers = ['Название файла','Номер для бухгалтерии','Название как в заявке','Объём, см³','Масса, г','Количество, шт'];
    const colWidths = [40, 25, 55, 12, 12, 12];
    const bodyRows = rows.map(r => [
        r.fileName, r.acctNumber, r.requestName,
        Math.round(r.volumeCm3*100)/100,
        Math.round(r.massG*100)/100,
        r.qty
    ]);
    return await buildGenericXlsxBlob({ sheetName: 'Изделия', headers, rows: bodyRows, colWidths, yellowIndices: [], hiddenCols: [], fontSize: 10 });
}

async function buildProductionOrderXlsxBlob(rows) {
    const headers = ['Номер','Артикул','Наименование','Количество','Количество порошка в граммах','Количество грамм всего'];
    const colWidths = [8, 12, 55, 12, 20, 20];
    const bodyRows = rows.map(r => [
        r.number, '', r.name, r.qty,
        Math.round(r.weight*100)/100,
        Math.round(r.totalWeight*100)/100
    ]);
    return await buildGenericXlsxBlob({ sheetName: 'Заказ в производство', headers, rows: bodyRows, colWidths, yellowIndices: [], hiddenCols: [], fontSize: 10 });
}

async function buildPriceCalculationXlsxBlob() {
    const postprocName = getPostprocForPricing();
    const col8Header = postprocName
        ? ('Цена за 1 см³ с пост-обработкой (' + postprocName + ')')
        : 'Цена за 1 см³ с пост-обработкой';
    const headers = [
        'Название файла',
        'Номер для бухгалтерии',
        'Название как в заявке',
        'Объём, см³',
        'Масса, г',
        'Количество, шт',
        'Цена за 1 см³',
        col8Header,
        'Цена за одно изделие без пост-обработки',
        'Цена за одно изделие с пост-обработкой',
        'Итого цена за партию изделий',
        'Итого цена за партию изделий с пост-обработкой'
    ];
    const colWidths = [35, 22, 50, 12, 12, 12, 14, 26, 22, 22, 22, 30];
    let totalNo = 0;
    let totalWith = 0;
    const bodyRows = allCards.map(entry => {
        const me = entry.modelEntry;
        const acctNumber = displayedAcctNumber(me);
        const requestName = describeModelLine(me.fileName, acctNumber);
        const volume = getEffectiveVolume(entry);
        const mass = getEffectiveMass(entry);
        const qty = Math.max(0, Math.round(Number(me.quantity) || 0));
        const price = Number(me.pricePerCm3) || 0;
        const priceWithPost = Number(me.postprocPricePerCm3) || 0;
        const unitNo = volume * price;
        const unitWith = volume * priceWithPost;
        const sumNo = unitNo * qty;
        const sumWith = unitWith * qty;
        totalNo += sumNo;
        totalWith += sumWith;
        return [
            me.fileName,
            acctNumber,
            requestName,
            Math.round(volume * 100) / 100,
            Math.round(mass * 100) / 100,
            qty,
            Math.round(price * 100) / 100,
            Math.round(priceWithPost * 100) / 100,
            Math.round(unitNo * 100) / 100,
            Math.round(unitWith * 100) / 100,
            Math.round(sumNo * 100) / 100,
            Math.round(sumWith * 100) / 100
        ];
    });
    bodyRows.push([
        'ИТОГО', '', '', '', '', '', '', '', '', '',
        Math.round(totalNo * 100) / 100,
        Math.round(totalWith * 100) / 100
    ]);
    return await buildGenericXlsxBlob({
        sheetName: 'Расчет стоимости',
        headers,
        rows: bodyRows,
        colWidths,
        yellowIndices: [],
        hiddenCols: [],
        fontSize: 10
    });
}

// === DOCX (WordprocessingML) ===
function xmlEscapeDocx(s) {
    return String(s == null ? '' : s)
        .replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')
        .replace(/"/g,'&quot;').replace(/'/g,'&apos;');
}
function docxRun(text, opts) {
    opts = opts || {};
    const rPr = [];
    if (opts.bold) rPr.push('<w:b/>');
    if (opts.italic) rPr.push('<w:i/>');
    if (opts.underline) rPr.push('<w:u w:val="single"/>');
    const sz = (opts.size || 11) * 2;
    rPr.push('<w:sz w:val="'+sz+'"/><w:szCs w:val="'+sz+'"/>');
    return '<w:r><w:rPr>'+rPr.join('')+'</w:rPr><w:t xml:space="preserve">'+xmlEscapeDocx(text)+'</w:t></w:r>';
}
function docxParagraph(runs, opts) {
    opts = opts || {};
    const pPr = [];
    if (opts.align) pPr.push('<w:jc w:val="'+opts.align+'"/>');
    if (opts.spacingAfter != null) pPr.push('<w:spacing w:after="'+(opts.spacingAfter*20)+'"/>');
    const p = pPr.length ? '<w:pPr>'+pPr.join('')+'</w:pPr>' : '';
    return '<w:p>'+p+(runs||'')+'</w:p>';
}
function docxImageParagraph(rId, cx, cy, picId, align) {
    const alignXml = align ? '<w:jc w:val="'+align+'"/>' : '';
    return '<w:p><w:pPr>'+alignXml+'</w:pPr>'+docxImageRunInline(rId, cx, cy, picId)+'</w:p>';
}
function docxImageRunInline(rId, cx, cy, picId) {
    return '<w:r><w:drawing><wp:inline distT="0" distB="0" distL="0" distR="0">'
        + '<wp:extent cx="'+cx+'" cy="'+cy+'"/>'
        + '<wp:effectExtent l="0" t="0" r="0" b="0"/>'
        + '<wp:docPr id="'+picId+'" name="Picture'+picId+'"/>'
        + '<wp:cNvGraphicFramePr><a:graphicFrameLocks xmlns:a="http://schemas.openxmlformats.org/drawingml/2006/main" noChangeAspect="1"/></wp:cNvGraphicFramePr>'
        + '<a:graphic xmlns:a="http://schemas.openxmlformats.org/drawingml/2006/main">'
        + '<a:graphicData uri="http://schemas.openxmlformats.org/drawingml/2006/picture">'
        + '<pic:pic xmlns:pic="http://schemas.openxmlformats.org/drawingml/2006/picture">'
        + '<pic:nvPicPr><pic:cNvPr id="'+picId+'" name="Picture'+picId+'"/><pic:cNvPicPr/></pic:nvPicPr>'
        + '<pic:blipFill><a:blip r:embed="'+rId+'"/><a:stretch><a:fillRect/></a:stretch></pic:blipFill>'
        + '<pic:spPr><a:xfrm><a:off x="0" y="0"/><a:ext cx="'+cx+'" cy="'+cy+'"/></a:xfrm>'
        + '<a:prstGeom prst="rect"><a:avLst/></a:prstGeom></pic:spPr>'
        + '</pic:pic></a:graphicData></a:graphic>'
        + '</wp:inline></w:drawing></w:r>';
}
function docxTableWithImages(models, media) {
    const colW = [7000, 3000];
    const gridCols = colW.map(w => '<w:gridCol w:w="'+w+'"/>').join('');
    function cell(runs, w, align) {
        const alignXml = align ? '<w:jc w:val="'+align+'"/>' : '';
        return '<w:tc><w:tcPr><w:tcW w:w="'+w+'" w:type="dxa"/></w:tcPr><w:p><w:pPr>'+alignXml+'</w:pPr>'+runs+'</w:p></w:tc>';
    }
    const headerRow = '<w:tr>' + cell(docxRun('Наименование', { bold: true }), colW[0])
                     + cell(docxRun('Изображение', { bold: true }), colW[1], 'center') + '</w:tr>';
    const dataRows = models.map((m, i) => {
        const name = describeModelLine(m.fileName, m.acctNumber) + ' (' + m.qty + ' шт);';
        const nameRuns = docxRun(name);
        const thumbMedia = media.find(x => x.rId === 'rIdImg' + (i+1));
        let imgRuns = '';
        if (thumbMedia) {
            const targetH = 1080000;
            const targetW = Math.round(targetH * (900/695));
            imgRuns = docxImageRunInline(thumbMedia.rId, targetW, targetH, 100 + i);
        }
        return '<w:tr>' + cell(nameRuns, colW[0]) + cell(imgRuns, colW[1], 'center') + '</w:tr>';
    }).join('');
    return '<w:tbl><w:tblPr><w:tblW w:w="0" w:type="auto"/><w:tblBorders>'
        + '<w:top w:val="single" w:sz="4" w:color="cccccc"/>'
        + '<w:left w:val="single" w:sz="4" w:color="cccccc"/>'
        + '<w:bottom w:val="single" w:sz="4" w:color="cccccc"/>'
        + '<w:right w:val="single" w:sz="4" w:color="cccccc"/>'
        + '<w:insideH w:val="single" w:sz="4" w:color="cccccc"/>'
        + '<w:insideV w:val="single" w:sz="4" w:color="cccccc"/>'
        + '</w:tblBorders></w:tblPr><w:tblGrid>'+gridCols+'</w:tblGrid>'
        + headerRow + dataRows + '</w:tbl>';
}

async function buildDocxBlob(data, isOldForm) {
    const media = [];
    const rels = [];
    const logoBytes = dataUrlToBytes(LETTERHEAD_LOGO_DATA_URI);
    media.push({ name: 'image1.png', data: logoBytes, rId: 'rIdLogo', isJpeg: false });
    rels.push({ id: 'rIdLogo', target: 'media/image1.png' });

    if (!isOldForm) {
        data.models.forEach((m, i) => {
            if (!m.imageDataUrl) return;
            try {
                const b = dataUrlToBytes(m.imageDataUrl);
                const name = 'image' + (i+2) + '.jpg';
                const rId = 'rIdImg' + (i+1);
                media.push({ name, data: b, rId, isJpeg: true });
                rels.push({ id: rId, target: 'media/' + name });
            } catch(e) {}
        });
    }

    const blocks = [];
    let logoW = 1800000, logoH = 400000;
    if (letterheadImg && letterheadImg.naturalWidth > 0) {
        const ar = letterheadImg.naturalWidth / letterheadImg.naturalHeight;
        logoH = Math.round(logoW / ar);
    }
    blocks.push(docxImageParagraph('rIdLogo', logoW, logoH, 1, 'left'));

    blocks.push(docxParagraph(
        docxRun('Заявка № ' + data.orderNumber + ' по КП ' + (data.kp || '…') + ' от ' + (data.dateParts.ddmmyyyy || '…'), { bold: true, size: 14 }),
        { spacingAfter: 8 }
    ));
    blocks.push(docxParagraph(
        docxRun('г. Москва') + docxRun('\t\t\t\t\t\t\t' + (data.dateParts.long || ''), { bold: true }),
        { spacingAfter: 8 }
    ));
    blocks.push(docxParagraph(
        docxRun('Наименование Покупателя (Заказчика): ') + docxRun(data.customer || '…', { bold: true }),
        { spacingAfter: 6 }
    ));
    blocks.push(docxParagraph(
        docxRun('Срок изготовления изделий (не позднее): ' + (data.deadline || '…')),
        { spacingAfter: 6 }
    ));

    if (!isOldForm) {
        blocks.push(docxParagraph(docxRun('Список изделий:', { bold: true, size: 12 }), { spacingAfter: 4 }));
        blocks.push(docxTableWithImages(data.models, media));
    } else {
        blocks.push(docxParagraph(docxRun('Список изделий:', { bold: true }), { spacingAfter: 4 }));
        data.models.forEach((m) => {
            blocks.push(docxParagraph(docxRun(describeModelLine(m.fileName, m.acctNumber) + ' (' + m.qty + ' шт);', { bold: true }), { spacingAfter: 2 }));
        });
    }

    const extText = data.exts.length ? '*. ' + data.exts.join(', *. ') : '—';
    blocks.push(docxParagraph(docxRun('Формат 3D модели: ') + docxRun(extText, { bold: true }), { spacingAfter: 4 }));
    blocks.push(docxParagraph(docxRun('Общее количество изделий: ' + data.totalQty + ' шт.'), { spacingAfter: 4 }));
    blocks.push(docxParagraph(docxRun('Наличие чертежа 2D* (нужное подчеркнуть): ') + docxRun(data.drawing === 'ДА' ? 'ДА' : 'НЕТ', { underline: true, bold: true }) + docxRun('/' + (data.drawing === 'ДА' ? 'НЕТ' : 'ДА')), { spacingAfter: 4 }));
    blocks.push(docxParagraph(docxRun('Технология печати (нужное подчеркнуть): ') + docxRun(data.tech === 'FDM' ? 'FDM' : 'SLS', { underline: true, bold: true }) + docxRun('/' + (data.tech === 'FDM' ? 'SLS' : 'FDM')), { spacingAfter: 4 }));
    blocks.push(docxParagraph(docxRun('Материал: ') + docxRun(data.material, { bold: true }), { spacingAfter: 4 }));

    blocks.push(docxParagraph(docxRun('Пост-обработка:', { bold: true }), { spacingAfter: 2 }));
    if (data.postproc.length) {
        data.postproc.forEach(p => blocks.push(docxParagraph(docxRun('• ' + p), { spacingAfter: 1 })));
    } else {
        blocks.push(docxParagraph(docxRun('—'), { spacingAfter: 1 }));
    }
    blocks.push(docxParagraph(docxRun('Дополнительная информация: Объем изделий по заказу ' + formatVolume(data.totalVolume) + ' см3; ' + (data.notes || '')), { spacingAfter: 6 }));

    blocks.push(docxParagraph(docxRun('Утверждаю', { bold: true, underline: true }) + docxRun('\t\t\t\t\t') + docxRun('Согласовано', { bold: true, underline: true }), { spacingAfter: 4 }));
    blocks.push(docxParagraph(docxRun('Исполнитель:') + docxRun('\t\t\t\t\t') + docxRun('Заказчик:'), { spacingAfter: 2 }));
    blocks.push(docxParagraph(docxRun('ООО «Аврора Карбон Технолоджи»') + docxRun('\t\t\t\t\t') + docxRun(data.customer || '…'), { spacingAfter: 2 }));
    blocks.push(docxParagraph(docxRun('Генеральный директор'), { spacingAfter: 6 }));
    blocks.push(docxParagraph(docxRun('____________________П.Б. Лустин') + docxRun('\t\t\t\t\t') + docxRun('____________________'), { spacingAfter: 6 }));

    const footNotes = [
        '* без предоставления 2D чертежа, точность изготовления изделия не гарантируется',
        '** если материал изделия неизвестен, необходимо проведение анализа и подбор материала для печати в соответствии с требованиями Заказчика. При отказе от услуги по подбору материала, после согласования марки материала, в случае деформации изделия в процессе тестирования или эксплуатации, претензии к качеству изготовления изделия не принимаются.',
        '*** Заказчик гарантирует (и несет полную ответственность за нарушение данного обязательства), что все предоставленные им информационные материалы, требуемые для выполнения работ, в рамках данного договора свободны от претензий третьих лиц и факт их передачи Исполнителю не нарушает патентных, авторских и смежных прав, а также прав на коммерческую тайну третьих лиц.',
        '**** Указанные в настоящей заявке модели изделий являются изделиями общепромышленного назначения'
    ];
    footNotes.forEach(note => blocks.push(docxParagraph(docxRun(note, { size: 8 }), { spacingAfter: 1 })));

    const bodyXml = blocks.join('');

    const documentXml = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>'
        + '<w:document xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main" '
        + 'xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships" '
        + 'xmlns:wp="http://schemas.openxmlformats.org/drawingml/2006/wordprocessingDrawing" '
        + 'xmlns:a="http://schemas.openxmlformats.org/drawingml/2006/main" '
        + 'xmlns:pic="http://schemas.openxmlformats.org/drawingml/2006/picture">'
        + '<w:body>' + bodyXml
        + '<w:sectPr><w:pgSz w:w="11906" w:h="16838"/>'
        + '<w:pgMar w:top="1134" w:right="850" w:bottom="1134" w:left="1701" w:header="708" w:footer="708" w:gutter="0"/>'
        + '</w:sectPr></w:body></w:document>';

    const relsXmlParts = rels.map(r =>
        '<Relationship Id="'+r.id+'" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="'+r.target+'"/>'
    ).join('');
    const documentRelsXml = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>'
        + '<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">'
        + relsXmlParts + '</Relationships>';

    const stylesXml = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>'
        + '<w:styles xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main">'
        + '<w:docDefaults><w:rPrDefault><w:rPr><w:rFonts w:ascii="Calibri" w:hAnsi="Calibri"/><w:sz w:val="22"/></w:rPr></w:rPrDefault></w:docDefaults>'
        + '</w:styles>';

    const hasJpeg = media.some(m => m.isJpeg);
    const contentTypes = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>'
        + '<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">'
        + '<Default Extension="rels" ContentType="application/vnd.openxmlformats-package.relationships+xml"/>'
        + '<Default Extension="xml" ContentType="application/xml"/>'
        + '<Default Extension="png" ContentType="image/png"/>'
        + (hasJpeg ? '<Default Extension="jpg" ContentType="image/jpeg"/>' : '')
        + '<Override PartName="/word/document.xml" ContentType="application/vnd.openxmlformats-officedocument.wordprocessingml.document.main+xml"/>'
        + '<Override PartName="/word/styles.xml" ContentType="application/vnd.openxmlformats-officedocument.wordprocessingml.styles+xml"/>'
        + '</Types>';

    const rootRels = '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>'
        + '<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">'
        + '<Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument" Target="word/document.xml"/>'
        + '</Relationships>';

    const files = [
        { name: '[Content_Types].xml', data: utf8Bytes(contentTypes) },
        { name: '_rels/.rels', data: utf8Bytes(rootRels) },
        { name: 'word/document.xml', data: utf8Bytes(documentXml) },
        { name: 'word/_rels/document.xml.rels', data: utf8Bytes(documentRelsXml) },
        { name: 'word/styles.xml', data: utf8Bytes(stylesXml) },
    ];
    media.forEach(m => files.push({ name: 'word/media/' + m.name, data: m.data }));

    return await buildZip(files);
}

async function downloadRequestDoc() {
    const data = collectRequestData();
    try {
        const blob = await buildDocxBlob(data, false);
        const ts = getTimestampSuffix();
        const parts = ['Заявка', data.orderNumber];
        if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) parts.push(data.customer);
        triggerBlobDownload(blob, safeFileNamePart(parts.join(' ')) + ' (' + ts + ').docx');
    } catch (e) {
        console.error(e);
        alert('Не удалось сформировать DOCX: ' + e.message);
    }
}

async function downloadRequestDocOld() {
    const data = collectRequestData();
    try {
        const blob = await buildDocxBlob(data, true);
        const ts = getTimestampSuffix();
        const parts = ['Заявка (старая форма)', data.orderNumber];
        if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) parts.push(data.customer);
        triggerBlobDownload(blob, safeFileNamePart(parts.join(' ')) + ' (' + ts + ').docx');
    } catch (e) {
        console.error(e);
        alert('Не удалось сформировать DOCX: ' + e.message);
    }
}

function collectInventoryRows(){return allCards.map(entry=>{const modelEntry=entry.modelEntry,acctNumber=displayedAcctNumber(modelEntry),volume=getEffectiveVolume(entry),mass=getEffectiveMass(entry);return{fileName:modelEntry.fileName,acctNumber,requestName:describeModelLine(modelEntry.fileName,acctNumber),volumeCm3:volume,massG:mass,qty:Math.max(0,Math.round(Number(modelEntry.quantity)||0))};});}

async function downloadInventoryXls() {
    if (!allCards.length) return;
    const rows = collectInventoryRows();
    const blob = await buildInventoryXlsxBlob(rows);
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    const data = collectRequestData();
    const ts = getTimestampSuffix();
    const parts = ['Таблица изделий'];
    if (data.dateParts.yymmdd && reqDailyNumber.value.trim()) parts.push(data.orderNumber);
    a.download = safeFileNamePart(parts.join(' ')) + ' (' + ts + ').xlsx';
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(() => URL.revokeObjectURL(url), 2000);
}

async function downloadPriceCalculationXls() {
    if (!allCards.length) return;
    const blob = await buildPriceCalculationXlsxBlob();
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    const data = collectRequestData();
    const ts = getTimestampSuffix();
    const parts = ['Расчет Стоимости'];
    if (data.dateParts.yymmdd && reqDailyNumber.value.trim()) parts.push(data.orderNumber);
    if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
    if (data.customer) parts.push(data.customer);
    a.download = safeFileNamePart(parts.join(' ')) + ' (' + ts + ').xlsx';
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(() => URL.revokeObjectURL(url), 2000);
}

async function downloadProductionOrderXls() {
    if (!allCards.length) return;
    const rows = allCards.map((entry, idx) => {
        const modelEntry = entry.modelEntry;
        const acctNumber = displayedAcctNumber(modelEntry);
        const name = baseNameNoExt(modelEntry.fileName) + '/' + acctNumber;
        const weight = getEffectiveMass(entry);
        const qty = Math.max(0, Math.round(Number(modelEntry.quantity) || 0));
        const totalWeight = weight * qty;
        return { number: idx + 1, name, weight, qty, totalWeight };
    });
    const blob = await buildProductionOrderXlsxBlob(rows);
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    const data = collectRequestData();
    const ts = getTimestampSuffix();
    const parts = ['Заказ в производство', data.orderNumber];
    if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
    if (data.customer) parts.push(data.customer);
    a.download = safeFileNamePart(parts.join(' ')) + ' (' + ts + ').xlsx';
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(() => URL.revokeObjectURL(url), 2000);
}

async function download1CExcel() {
    if (!allCards.length) return;
    const rows = allCards.map(entry => {
        const modelEntry = entry.modelEntry;
        const acctNumber = displayedAcctNumber(modelEntry);
        const name = describeModelLine(modelEntry.fileName, acctNumber);
        const weight = getEffectiveMass(entry);
        const volume = getEffectiveVolume(entry);
        return { name, weight, volume };
    });
    const blob = await build1CXlsxBlob(rows);
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    const data = collectRequestData();
    const ts = getTimestampSuffix();
    const parts = ['Таблица 1С', data.orderNumber];
    if (data.dateParts.ddmmyyyy) parts.push('от', data.dateParts.ddmmyyyy);
    if (data.customer) parts.push(data.customer);
    a.download = safeFileNamePart(parts.join(' ')) + ' (' + ts + ').xlsx';
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(() => URL.revokeObjectURL(url), 2000);
}

const CRC32_TABLE=(()=>{const t=new Uint32Array(256);for(let n=0;n<256;n++){let c=n;for(let k=0;k<8;k++)c=(c&1)?0xEDB88320^(c>>>1):c>>>1;t[n]=c>>>0;}return t;})();
function crc32(bytes){let c=0xFFFFFFFF;for(let i=0;i<bytes.length;i++)c=CRC32_TABLE[(c^bytes[i])&0xFF]^(c>>>8);return(c^0xFFFFFFFF)>>>0;}
async function deflateRaw(bytes){const stream=new Blob([bytes]).stream().pipeThrough(new CompressionStream('deflate-raw'));const buf=await new Response(stream).arrayBuffer();return new Uint8Array(buf);}
function dosDateTime(date){const dosTime=((date.getHours()&0x1F)<<11)|((date.getMinutes()&0x3F)<<5)|((date.getSeconds()>>1)&0x1F);const dosDate=(((date.getFullYear()-1980)&0x7F)<<9)|(((date.getMonth()+1)&0xF)<<5)|(date.getDate()&0x1F);return{dosTime,dosDate};}
function utf8Bytes(str){return new TextEncoder().encode(str);}
async function buildZip(files){const{dosTime,dosDate}=dosDateTime(new Date());const localParts=[],centralParts=[];let offset=0;for(const file of files){const nameBytes=utf8Bytes(file.name),dataBytes=file.data,crc=crc32(dataBytes),compressed=await deflateRaw(dataBytes);const localHeader=new Uint8Array(30+nameBytes.length),lv=new DataView(localHeader.buffer);lv.setUint32(0,0x04034b50,true);lv.setUint16(4,20,true);lv.setUint16(6,0x0800,true);lv.setUint16(8,8,true);lv.setUint16(10,dosTime,true);lv.setUint16(12,dosDate,true);lv.setUint32(14,crc,true);lv.setUint32(18,compressed.length,true);lv.setUint32(22,dataBytes.length,true);lv.setUint16(26,nameBytes.length,true);lv.setUint16(28,0,true);localHeader.set(nameBytes,30);localParts.push(localHeader,compressed);const centralHeader=new Uint8Array(46+nameBytes.length),cv=new DataView(centralHeader.buffer);cv.setUint32(0,0x02014b50,true);cv.setUint16(4,20,true);cv.setUint16(6,20,true);cv.setUint16(8,0x0800,true);cv.setUint16(10,8,true);cv.setUint16(12,dosTime,true);cv.setUint16(14,dosDate,true);cv.setUint32(16,crc,true);cv.setUint32(20,compressed.length,true);cv.setUint32(24,dataBytes.length,true);cv.setUint16(28,nameBytes.length,true);cv.setUint16(30,0,true);cv.setUint16(32,0,true);cv.setUint16(34,0,true);cv.setUint16(36,0,true);cv.setUint32(38,0,true);cv.setUint32(42,offset,true);centralHeader.set(nameBytes,46);centralParts.push(centralHeader);offset+=localHeader.length+compressed.length;}const centralSize=centralParts.reduce((s,p)=>s+p.length,0),centralOffset=offset,eocd=new Uint8Array(22),ev=new DataView(eocd.buffer);ev.setUint32(0,0x06054b50,true);ev.setUint16(4,0,true);ev.setUint16(6,0,true);ev.setUint16(8,files.length,true);ev.setUint16(10,files.length,true);ev.setUint32(12,centralSize,true);ev.setUint32(16,centralOffset,true);ev.setUint16(20,0,true);return new Blob([...localParts,...centralParts,eocd],{type:'application/zip'});}
function dedupeZipName(name,usedNames){if(!usedNames.has(name)){usedNames.add(name);return name;}const dot=name.lastIndexOf('.'),base=dot>0?name.slice(0,dot):name,ext=dot>0?name.slice(dot):'';let n=2,candidate=base+' ('+n+')'+ext;while(usedNames.has(candidate)){n++;candidate=base+' ('+n+')'+ext;}usedNames.add(candidate);return candidate;}

async function downloadAllZip(){
    if(!allCards.length)return;
    reqAllBtn.disabled=true;
    const prev=reqAllBtn.textContent;
    reqAllBtn.innerHTML='<span class="spinner"></span> Собираю архив…';
    try{
        const files=[];
        const usedPhotoWm = new Set();
        const usedPhotoNoWm = new Set();
        const usedProd = new Set();
        const usedSrc = new Set();
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const blob = await canvasToJpegBlob(me.canvas);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildDownloadFileName(me), usedPhotoWm);
                files.push({ name: 'Фото с водяным знаком/' + name, data: buf });
            }
        }
        for (const entry of allCards) {
            const me = entry.modelEntry;
            const blob = await canvasToJpegBlobNoLogo(me.canvas);
            if (blob) {
                const buf = new Uint8Array(await blob.arrayBuffer());
                const name = dedupeZipName(buildDownloadFileName(me), usedPhotoNoWm);
                files.push({ name: 'Фото без водяного знака/' + name, data: buf });
            }
        }
        {
            const postprocList = getSelectedPostprocList();
            const reqData = collectRequestData();
            const commonOpts = {
                postprocList: postprocList,
                orderNumber: reqData.orderNumber,
                orderDate: reqData.dateParts.ddmmyyyy,
                customer: reqData.customer
            };
            for (const entry of allCards) {
                const me = entry.modelEntry;
                const opts = Object.assign({}, commonOpts, {
                    volumeCm3: getEffectiveVolume(entry),
                    massG: getEffectiveMass(entry),
                    qty: Math.max(0, Math.round(Number(me.quantity) || 0))
                });
                const blob = await renderProductionPhotoBlob(entry, opts);
                if (blob) {
                    const buf = new Uint8Array(await blob.arrayBuffer());
                    const name = dedupeZipName(buildProductionFileName(me), usedProd);
                    files.push({ name: 'Фото для производства/' + name, data: buf });
                }
            }
        }
        const ts = getTimestampSuffix();
        for (const entry of allCards) {
            const me = entry.modelEntry;
            if (me.originalBuffer) {
                const name = dedupeZipName(buildSourceFileName(me, ts), usedSrc);
                files.push({ name: 'Исходники/' + name, data: new Uint8Array(me.originalBuffer) });
            }
        }
        const data = collectRequestData();

        const docxNewBlob = await buildDocxBlob(data, false);
        const docParts = ['Заявка', data.orderNumber];
        if (data.dateParts.ddmmyyyy) docParts.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) docParts.push(data.customer);
        files.push({ name: safeFileNamePart(docParts.join(' ')) + ' (' + ts + ').docx', data: new Uint8Array(await docxNewBlob.arrayBuffer()) });

        const docxOldBlob = await buildDocxBlob(data, true);
        const docPartsOld = ['Заявка (старая форма)', data.orderNumber];
        if (data.dateParts.ddmmyyyy) docPartsOld.push('от', data.dateParts.ddmmyyyy);
        if (data.customer) docPartsOld.push(data.customer);
        files.push({ name: safeFileNamePart(docPartsOld.join(' ')) + ' (' + ts + ').docx', data: new Uint8Array(await docxOldBlob.arrayBuffer()) });

        const rows1C = allCards.map(entry => {
            const modelEntry = entry.modelEntry;
            const acctNumber = displayedAcctNumber(modelEntry);
            const name = describeModelLine(modelEntry.fileName, acctNumber);
            const weight = getEffectiveMass(entry);
            const volume = getEffectiveVolume(entry);
            return { name, weight, volume };
        });
        const xlsxBlob1C = await build1CXlsxBlob(rows1C);
        const hasOrder = !!(data.dateParts.yymmdd && reqDailyNumber.value.trim());
        const xls1CParts = ['Таблица 1С'];
        if (hasOrder) xls1CParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(xls1CParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await xlsxBlob1C.arrayBuffer()) });

        const rowsInv = collectInventoryRows();
        const invXlsxBlob = await buildInventoryXlsxBlob(rowsInv);
        const xlsParts = ['Таблица изделий'];
        if (hasOrder) xlsParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(xlsParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await invXlsxBlob.arrayBuffer()) });

        const priceXlsxBlob = await buildPriceCalculationXlsxBlob();
        const priceParts = ['Расчет Стоимости'];
        if (hasOrder) priceParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(priceParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await priceXlsxBlob.arrayBuffer()) });

        const prodRows = allCards.map((entry, idx) => {
            const modelEntry = entry.modelEntry;
            const acctNumber = displayedAcctNumber(modelEntry);
            const name = baseNameNoExt(modelEntry.fileName) + '/' + acctNumber;
            const weight = getEffectiveMass(entry);
            const qty = Math.max(0, Math.round(Number(modelEntry.quantity) || 0));
            const totalWeight = weight * qty;
            return { number: idx + 1, name, weight, qty, totalWeight };
        });
        const prodXlsxBlob = await buildProductionOrderXlsxBlob(prodRows);
        const prodParts = ['Заказ в производство'];
        if (hasOrder) prodParts.push(data.orderNumber);
        files.push({ name: safeFileNamePart(prodParts.join(' ')) + ' (' + ts + ').xlsx', data: new Uint8Array(await prodXlsxBlob.arrayBuffer()) });

        const customerPart = data.customer ? data.customer.replace(/[^a-zA-Zа-яА-Я0-9\-_ ]/g, '').trim() : 'Без_покупателя';
        const orderPart = data.orderNumber || 'без_номера';
        const datePart = data.dateParts.ddmmyyyy || new Date().toLocaleDateString('ru-RU').replace(/\./g, '_');
        const zipName = safeFileNamePart(`${customerPart} _ ${orderPart} _ ${datePart} _ ${ts}`) + '.zip';
        const zip = await buildZip(files);
        triggerBlobDownload(zip, zipName);
    } catch(err) {
        console.error(err);
        setStatus('Не удалось собрать архив: ' + escapeHtml(err.message || String(err)));
    } finally {
        reqAllBtn.disabled = allCards.length === 0;
        reqAllBtn.textContent = prev;
    }
}

function refreshRequestPreview(){
    const has=allCards.length>0;
    requestEmpty.style.display=has?'none':'';
    requestForm.style.display=has?'':'none';
    reqDownloadBtn.disabled=!has;
    reqOldDownloadBtn.disabled=!has;
    reqXlsBtn.disabled=!has;
    reqPriceBtn.disabled=!has;
    req1Cbtn.disabled=!has;
    reqPhotosWmBtn.disabled=!has;
    reqPhotosNoWmBtn.disabled=!has;
    reqProdPhotosBtn.disabled=!has;
    reqProdOrderBtn.disabled=!has;
    reqAllBtn.disabled=!has;
    updateFloatingInfo();
    updateAllCardsPostprocView();
    if(!has)return;
    reqPreview.innerHTML=buildRequestInnerHtml(collectRequestData());
}
[reqDate,reqDailyNumber,reqKp,reqCustomer,reqDeadline,reqDrawing,reqTech,reqNotes].forEach(el=>{el.addEventListener('input',refreshRequestPreview);el.addEventListener('change',refreshRequestPreview);});
reqDownloadBtn.addEventListener('click',downloadRequestDoc);
reqOldDownloadBtn.addEventListener('click',downloadRequestDocOld);
reqRefreshBtn.addEventListener('click',refreshRequestPreview);
reqXlsBtn.addEventListener('click',downloadInventoryXls);
reqPriceBtn.addEventListener('click',downloadPriceCalculationXls);
req1Cbtn.addEventListener('click',download1CExcel);
reqPhotosWmBtn.addEventListener('click',() => downloadPhotosZip(true));
reqPhotosNoWmBtn.addEventListener('click',() => downloadPhotosZip(false));
reqProdOrderBtn.addEventListener('click',downloadProductionOrderXls);
reqAllBtn.addEventListener('click',downloadAllZip);
stlUnitSelect.addEventListener('change',()=>{const uc=lengthUnitToCm(stlUnitSelect.value);for(const item of stlCards){const dims={x:item.rawSize.x*uc,y:item.rawSize.y*uc,z:item.rawSize.z*uc};const cm3=effectiveVolumeCm3(item.rawVolume*uc*uc*uc,dims);const label=formatVolume(cm3)+' см³';const thin=isThinPart(dims)?' — тонкая деталь (мин. сторона < '+MIN_SIDE_FOR_MESH_VOLUME_CM+' см), объём по габаритам':'';item.volumeEl.textContent=label;item.noteEl.textContent='из STL, единицы файла: '+unitLabel(stlUnitSelect.value)+thin;item.modelEntry.volumeText=label;item.modelEntry.dimsCm=dims;const uc2=uc;item.modelEntry.trianglesCm=item.modelEntry.triangles.map(tri=>tri.map(v=>[v[0]*uc2,v[1]*uc2,v[2]*uc2]));const real=item.rawVolume*uc*uc*uc;item.modelEntry.realVolumeCm3=real;updateRealVolumeDisplay(item);updateDisplay(item);}refreshRequestPreview();});
materialSelect.addEventListener('change',()=>{currentMaterialKey=materialSelect.value;for(const entry of allCards){updateDisplay(entry);updateRealVolumeDisplay(entry);}refreshRequestPreview();});

groupQtyInput.addEventListener('input',()=>{
    const raw=parseInt(groupQtyInput.value,10);
    if(!Number.isFinite(raw)||raw<1)return;
    for(const entry of allCards){
        const me=entry.modelEntry;
        me.quantity=raw;
        const inp=entry.root.querySelector('.qty-input');
        if(inp)inp.value=raw;
        updateCardPriceDisplay(entry);
    }
    refreshRequestPreview();
});
groupPriceInput.addEventListener('input',()=>{
    const raw=parseFloat(groupPriceInput.value);
    if(!Number.isFinite(raw)||raw<0)return;
    const name=getPostprocForPricing();
    const coeff=getPostprocCoefficient(name);
    for(const entry of allCards){
        const me=entry.modelEntry;
        me.pricePerCm3=raw;
        const inp=entry.root.querySelector('.price-per-cm3-input');
        if(inp)inp.value=raw;
        if(!me.postprocPriceManuallyEdited&&name){
            const newPp=raw*coeff;
            me.postprocPricePerCm3=newPp;
            const ppInp=entry.root.querySelector('.postproc-price-input');
            if(ppInp)ppInp.value=newPp.toFixed(2);
        }
        updateCardPriceDisplay(entry);
    }
    refreshRequestPreview();
});
groupSerialInput.addEventListener('input',()=>{
    const raw=parseInt(groupSerialInput.value,10);
    if(!Number.isFinite(raw)||raw<1)return;
    let serial=raw;
    for(const entry of allCards){
        const me=entry.modelEntry;
        me.autoSerial=String(serial);
        if(!me.serialManuallyEdited){
            me.serial=me.autoSerial;
        }
        const inp=entry.root.querySelector('.serial-input');
        if(inp)inp.value=me.serial;
        const acctInp=entry.root.querySelector('.acct-number-input');
        if(acctInp&&!me.numberManuallyEdited)acctInp.value=acctNumberText(me);
        me.nameLines=[me.fileName,acctNumberText(me)];
        renderIsometric(me.canvas,me.triangles,me.label,me.dimsCm,me.nameLines,me.rotation,me);
        serial++;
    }
    nextSerialNumber=serial;
    refreshRequestPreview();
});

window.addEventListener('resize',()=>{for(const entry of renderedModels)renderIsometric(entry.canvas,entry.triangles,entry.label,entry.dimsCm,entry.nameLines,entry.rotation,entry);});
refreshRequestPreview();
})();
</script>
</body>
</html>
