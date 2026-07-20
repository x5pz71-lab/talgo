<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Buscador de Fichas</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  :root {
    --bg: #0a0c12; --surface: #12151e; --card: #161a25; --card-hover: #1b2130;
    --border: #262b3a; --accent: #c9a961; --accent2: #e3c988; --text: #f0f1f5;
    --text-muted: #8b93a3; --text-dim: #575e6d; --tag-bg: #1d2230; --tag-text: #d9bd82;
    --green: #4ade80; --red: #f87171; --yellow: #c9a961; --radius: 18px; --radius-sm: 12px;
  }
  body {
    background:
      radial-gradient(circle at top right, rgba(201,169,97,0.10), transparent 28%),
      radial-gradient(circle at bottom left, rgba(201,169,97,0.05), transparent 22%),
      var(--bg);
    color: var(--text);
    font-family: 'Inter', 'Tajawal', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    min-height: 100vh;
    padding-bottom: 80px;
    letter-spacing: 0.1px;
  }

  /* LOGIN SCREEN */
  .login-screen {
    position: fixed; inset: 0; background: var(--bg); z-index: 999;
    display: flex; align-items: center; justify-content: center; padding: 24px;
  }
  .login-box {
    background: linear-gradient(165deg, #171b26, #10131b);
    border: 1px solid var(--border);
    border-radius: 22px;
    padding: 40px 28px 32px;
    width: 100%; max-width: 360px;
    text-align: center;
    box-shadow: 0 24px 60px rgba(0,0,0,0.5);
    position: relative;
  }
  .login-box::before {
    content: ''; position: absolute; top: 0; left: 50%; transform: translateX(-50%);
    width: 46px; height: 3px; background: linear-gradient(90deg, transparent, var(--accent), transparent);
    border-radius: 2px;
  }
  .login-logo { font-size: 40px; margin-bottom: 14px; filter: grayscale(0.3) sepia(0.3); }
  .login-title { font-size: 21px; font-weight: 700; color: #fff; margin-bottom: 5px; letter-spacing: 0.2px; }
  .login-sub { font-size: 12.5px; color: var(--text-muted); margin-bottom: 28px; letter-spacing: 0.3px; }
  .login-label { display: block; font-size: 11px; font-weight: 600; color: var(--accent2); text-align: right; margin-bottom: 7px; text-transform: uppercase; letter-spacing: 0.6px; }
  .login-input {
    width: 100%; background: var(--surface); border: 1px solid var(--border);
    border-radius: 10px; padding: 13px 14px; font-size: 15px; color: var(--text);
    outline: none; margin-bottom: 16px; text-align: left; direction: ltr;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .login-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(201,169,97,0.12); }
  .login-btn {
    width: 100%; background: linear-gradient(135deg, #d4b876, #b8923f); border: none; color: #171310;
    border-radius: 12px; padding: 14px; font-size: 15px; font-weight: 700;
    cursor: pointer; margin-bottom: 12px; transition: filter 0.2s; letter-spacing: 0.2px;
  }
  .login-btn:active { filter: brightness(0.92); }
  .login-error { color: var(--red); font-size: 13px; margin-top: 4px; display: none; }
  .login-viewer-btn {
    background: none; border: 1px dashed var(--border); color: var(--text-muted);
    border-radius: 12px; padding: 12px; font-size: 13.5px; cursor: pointer; width: 100%;
    transition: all 0.2s;
  }
  .login-viewer-btn:hover { border-color: var(--accent); color: var(--accent2); }

  /* ROLE BADGE */
  .role-badge {
    display: inline-flex; align-items: center; gap: 5px;
    padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: 700;
    cursor: pointer;
  }
  .role-admin { background: #2a2313; color: var(--accent2); border: 1px solid var(--accent); }
  .role-viewer { background: #16261c; color: var(--green); border: 1px solid var(--green); }

  /* HEADER */
  .header {
    background: linear-gradient(135deg, #0d0f16 0%, #171b26 50%, #12151f 100%);
    padding: 10px 14px 8px;
    position: sticky; top: 0; z-index: 100;
    border-bottom: 1px solid rgba(201,169,97,0.16);
    box-shadow: 0 12px 30px rgba(0,0,0,0.25);
  }
  .header-top { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }
  .logo-icon {
    width: 28px; height: 28px; background: linear-gradient(135deg, #d4b876, #a67c3d);
    border-radius: 8px; display: flex; align-items: center; justify-content: center;
    font-size: 13px; flex-shrink: 0;
  }
  .header-title { font-size: 15px; font-weight: 700; letter-spacing: 0.1px; color: #fff; }
  .header-sub { font-size: 10px; color: var(--text-muted); margin-top: 0px; }
  .header-right { margin-right: auto; display: flex; align-items: center; gap: 8px; }
  .add-btn {
    background: linear-gradient(135deg, #d4b876, #b8923f); color: #171310; border: none; border-radius: var(--radius-sm);
    padding: 6px 12px; font-size: 12px; font-weight: 700; cursor: pointer;
    display: flex; align-items: center; gap: 5px; transition: filter 0.2s; white-space: nowrap;
  }
  .add-btn:active { filter: brightness(0.9); }
  .trash-btn {
    position: relative; background: var(--surface); color: var(--text-muted); border: 1.5px solid var(--border);
    border-radius: var(--radius-sm); padding: 6px 8px; font-size: 13px; cursor: pointer;
    display: flex; align-items: center; justify-content: center; transition: all 0.2s;
  }
  .trash-btn:hover { border-color: var(--red); color: var(--red); }
  .requests-btn:hover { border-color: var(--accent); color: var(--accent2); }
  .temp-access-btn { position: relative; }
  .temp-access-btn.is-active { border-color: var(--accent); color: var(--accent2); background: linear-gradient(135deg, #2a2313, #1d1a0e); }
  .temp-access-dot {
    position: absolute; top: -4px; left: -4px; width: 10px; height: 10px; border-radius: 50%;
    background: var(--green); border: 2px solid var(--bg); display: none;
    animation: pulseDot 1.6s ease-in-out infinite;
  }
  .temp-access-btn.is-active .temp-access-dot { display: block; }
  @keyframes pulseDot { 0%,100% { box-shadow: 0 0 0 0 rgba(74,222,128,0.5); } 50% { box-shadow: 0 0 0 4px rgba(74,222,128,0); } }
  .temp-hint {
    position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
    background: #2a2313; border: 1px solid var(--accent); color: var(--accent2);
    padding: 9px 18px; border-radius: 20px; font-size: 12px; font-weight: 600;
    z-index: 50; display: flex; align-items: center; gap: 6px; white-space: nowrap;
  }
  .temp-modal-status {
    background: var(--surface); border: 1px solid var(--border); border-radius: 12px;
    padding: 12px 14px; margin-bottom: 16px; display: flex; align-items: center; justify-content: space-between; gap: 10px;
  }
  .temp-modal-status.on { border-color: var(--accent); background: #1d1a0e; }
  .temp-status-text { font-size: 13px; color: var(--text-muted); }
  .temp-status-text b { color: var(--accent2); font-family: 'Courier New', monospace; }
  .temp-end-btn { background: #2d1515; color: var(--red); border: none; border-radius: 8px; padding: 7px 12px; font-size: 12px; font-weight: 700; cursor: pointer; white-space: nowrap; }
  .temp-duration-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 14px; }
  .temp-duration-btn {
    background: var(--surface); border: 1.5px solid var(--border); color: var(--text);
    border-radius: 10px; padding: 12px 8px; font-size: 13px; font-weight: 600; cursor: pointer;
    transition: all 0.2s;
  }
  .temp-duration-btn:hover { border-color: var(--accent); color: var(--accent2); }
  .temp-custom-row { display: flex; gap: 8px; align-items: center; }
  .trash-badge {
    position: absolute; top: -6px; left: -6px; background: var(--red); color: #fff;
    font-size: 10px; font-weight: 700; min-width: 17px; height: 17px; border-radius: 9px;
    display: flex; align-items: center; justify-content: center; padding: 0 4px; line-height: 1;
  }
  .requests-badge { background: var(--accent); }
  .request-part-btn {
    background: linear-gradient(135deg, #232838, #1a1e2a); color: var(--accent2); border: 1px solid var(--border);
    border-radius: var(--radius-sm); padding: 6px 12px; font-size: 12px; font-weight: 600; cursor: pointer;
    display: flex; align-items: center; gap: 5px; white-space: nowrap; transition: all 0.2s;
  }
  .request-part-btn:hover { border-color: var(--accent); }
  .request-fab { background: linear-gradient(135deg, #4ade80, #22a35e); box-shadow: 0 8px 26px rgba(34,163,94,0.35); }
  .request-item {
    background: var(--surface); border: 1.5px solid var(--border); border-radius: 12px;
    padding: 12px; margin-bottom: 8px;
  }
  .request-item-name { font-size: 14px; font-weight: 700; color: var(--text); margin-bottom: 4px; }
  .request-item-meta { font-size: 11px; color: var(--text-dim); margin-bottom: 6px; }
  .request-item-note { font-size: 12px; color: var(--text-muted); background: #0f1a2e; border-radius: 8px; padding: 8px 10px; margin-bottom: 10px; line-height: 1.5; }
  .request-item-btns { display: flex; gap: 8px; }
  .request-accept-btn, .request-reject-btn {
    flex: 1; border: none; border-radius: 8px; padding: 10px; font-size: 13px; font-weight: 700; cursor: pointer;
    display: flex; align-items: center; justify-content: center; gap: 5px;
  }
  .request-accept-btn { background: #153022; color: var(--green); }
  .request-reject-btn { background: #2d1515; color: var(--red); }
  .requests-empty { text-align: center; padding: 40px 20px; color: var(--text-muted); }
  .trash-item {
    display: flex; align-items: center; gap: 10px; background: var(--surface);
    border: 1.5px solid var(--border); border-radius: 12px; padding: 10px 12px; margin-bottom: 8px;
  }
  .trash-item-info { flex: 1; min-width: 0; }
  .trash-item-name { font-size: 13px; font-weight: 600; color: var(--text); margin-bottom: 2px; }
  .trash-item-meta { font-size: 11px; color: var(--text-dim); }
  .trash-item-btns { display: flex; gap: 6px; flex-shrink: 0; }
  .trash-restore-btn, .trash-purge-btn {
    border: none; border-radius: 8px; padding: 8px 10px; font-size: 12px; font-weight: 600; cursor: pointer;
    display: flex; align-items: center; gap: 4px; white-space: nowrap;
  }
  .trash-restore-btn { background: #153022; color: var(--green); }
  .trash-purge-btn { background: #2d1515; color: var(--red); }
  .trash-empty { text-align: center; padding: 40px 20px; color: var(--text-muted); }

  /* SEARCH */
  .search-wrap { position: relative; margin-bottom: 6px; }
  .search-icon { position: absolute; right: 13px; top: 50%; transform: translateY(-50%); color: var(--text-muted); font-size: 14px; }
  .search-input {
    width: 100%; background: var(--surface); border: 1.5px solid var(--border);
    border-radius: 10px; padding: 9px 40px 9px 12px; font-size: 14px; color: var(--text);
    outline: none; transition: border-color 0.2s; direction: ltr; text-align: left;
  }
  .search-input::placeholder { color: var(--text-dim); }
  .search-input:focus { border-color: var(--accent); }
  .search-clear {
    position: absolute; left: 12px; top: 50%; transform: translateY(-50%);
    background: var(--border); border: none; color: var(--text-muted); width: 22px;
    height: 22px; border-radius: 50%; font-size: 12px; cursor: pointer;
    display: none; align-items: center; justify-content: center;
  }
  .search-clear.show { display: flex; }
  .stats-bar { font-size: 11px; color: var(--text-muted); margin-top: 4px; }
  .stats-bar span { color: var(--accent2); font-weight: 600; }

  /* FILTERS */
  .filters {
    display: flex; gap: 7px; overflow-x: auto; padding: 10px 16px; scrollbar-width: none;
    position: sticky; top: 104px; z-index: 55;
    background: linear-gradient(180deg, rgba(10,12,18,0.94), rgba(10,12,18,0.74));
    backdrop-filter: blur(10px);
    border-bottom: 1px solid rgba(201,169,97,0.08);
    scroll-snap-type: x proximity;
  }
  .filters::-webkit-scrollbar { display: none; }
  .filter-btn {
    flex-shrink: 0; background: var(--surface); border: 1.5px solid var(--border);
    color: var(--text-muted); border-radius: 20px; padding: 7px 14px; font-size: 12px;
    font-weight: 600; cursor: pointer; transition: all 0.2s; white-space: nowrap;
    scroll-snap-align: start;
  }
  .filter-btn:hover {
    border-color: var(--accent); color: var(--accent2); transform: translateY(-1px);
    box-shadow: 0 6px 16px rgba(201,169,97,0.12);
  }
  .filter-btn:active { transform: scale(0.98); }
  .filter-btn.active {
    background: linear-gradient(135deg, #d4b876, #a67c3d);
    border-color: #d4b876;
    color: #171310;
    box-shadow: 0 8px 20px rgba(201,169,97,0.28);
  }

  /* CARDS */
  .cards-container { padding: 12px 16px 0; }
  .card {
    background: linear-gradient(180deg, #171b26, #12151e);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 15px;
    margin-bottom: 10px;
    position: relative;
    transition: transform 0.18s ease, border-color 0.2s ease, box-shadow 0.18s ease;
    cursor: pointer;
    box-shadow: 0 4px 18px rgba(0,0,0,0.22);
  }
  .card:hover { transform: translateY(-2px); border-color: #4a3f28; box-shadow: 0 12px 32px rgba(0,0,0,0.32); }
  .card:active { transform: scale(0.99); }
  .card.expanded { border-color: var(--accent); box-shadow: 0 0 0 1px rgba(201,169,97,0.25), 0 12px 32px rgba(0,0,0,0.32); }
  .card-header { display: flex; align-items: flex-start; gap: 10px; direction: ltr; }
  .card-icon { width: 38px; height: 38px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }
  .card-image { width: 54px; height: 54px; border-radius: 14px; overflow: hidden; background: #0f1a2a; display: flex; align-items: center; justify-content: center; flex-shrink: 0; cursor: zoom-in; }
  .card-image img { width: 100%; height: 100%; object-fit: cover; display: block; }
  .card-side { display: flex; align-items: center; gap: 6px; flex-shrink: 0; }
  .card-thumb { width: 30px; height: 30px; border-radius: 8px; overflow: hidden; background: #0f1a2a; display: flex; align-items: center; justify-content: center; flex-shrink: 0; cursor: zoom-in; border: 1.5px solid var(--border); }
  .card-thumb img { width: 100%; height: 100%; object-fit: cover; display: block; }
  .card-info { flex: 1; min-width: 0; }
  .card-name { font-size: 14px; font-weight: 600; color: var(--text); line-height: 1.3; margin-bottom: 4px; }
  .card-code { font-size: 12px; color: var(--accent2); font-weight: 700; font-family: 'Courier New', monospace; }
  .card-actions { display: flex; gap: 6px; flex-shrink: 0; }
  .card-btn { width: 30px; height: 30px; border-radius: var(--radius-sm); border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 13px; transition: opacity 0.15s; }
  .card-btn:active { opacity: 0.7; }
  .btn-edit { background: var(--tag-bg); color: var(--accent2); }
  .btn-delete { background: #2d1515; color: var(--red); }
  .card-tags { display: flex; flex-wrap: wrap; gap: 5px; margin-top: 8px; }
  .tag { background: linear-gradient(135deg, #232838, #1a1e2a); color: var(--tag-text); border-radius: 7px; padding: 3px 8px; font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.3px; }
  .tag.location { background: linear-gradient(135deg, #16261c, #101c15); color: var(--green); }
  .tag.system { background: linear-gradient(135deg, #2e2612, #211b0d); color: var(--yellow); }
  .tag.viewer-added {
    background: transparent; border: 1px dashed var(--accent); color: var(--accent2);
    display: inline-flex; align-items: center; gap: 4px;
  }
  .card-arabic { margin-top: 8px; padding: 8px 10px; background: #0f1219; border-radius: 8px; border-right: 3px solid var(--accent); direction: rtl; text-align: right; }
  .arabic-row { display: flex; align-items: baseline; gap: 6px; margin-bottom: 3px; flex-direction: row-reverse; }
  .arabic-label { font-size: 10px; color: var(--text-muted); font-weight: 600; white-space: nowrap; flex-shrink: 0; }
  .arabic-value { font-size: 13px; color: var(--accent2); font-weight: 600; line-height: 1.4; font-family: 'Tajawal', sans-serif; }
  .arabic-phonetic { font-size: 12px; color: var(--text-dim); font-family: 'Tajawal', sans-serif; }
  .translate-btn {
    margin-top: 8px; background: none; border: 1px dashed var(--border); color: var(--text-muted);
    border-radius: 7px; padding: 5px 10px; font-size: 11px; cursor: pointer; width: 100%;
    transition: all 0.2s; display: flex; align-items: center; justify-content: center; gap: 5px;
  }
  .translate-btn:hover { border-color: var(--accent); color: var(--accent2); }
  .card-details { display: none; margin-top: 12px; padding-top: 12px; border-top: 1px solid var(--border); }
  .card.expanded .card-details { display: block; }
  .detail-row { display: flex; gap: 8px; margin-bottom: 8px; font-size: 13px; }
  .detail-label { color: var(--text-muted); min-width: 80px; font-size: 12px; }
  .detail-value { color: var(--text); font-weight: 500; }

  /* EMPTY */
  .empty { text-align: center; padding: 60px 20px; color: var(--text-muted); }
  .empty-icon { font-size: 48px; margin-bottom: 12px; }
  .empty-title { font-size: 16px; font-weight: 600; margin-bottom: 6px; color: var(--text-dim); }

  /* MODAL */
  .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 200; display: none; align-items: flex-end; backdrop-filter: blur(3px); }
  .modal-overlay.show { display: flex; }
  .modal { background: var(--card); border-radius: 20px 20px 0 0; border: 1px solid var(--border); padding: 20px 20px 40px; width: 100%; max-height: 85vh; overflow-y: auto; animation: slideUp 0.3s ease; }
  @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
  .modal-handle { width: 36px; height: 4px; background: var(--border); border-radius: 2px; margin: 0 auto 18px; }
  .modal-title { font-size: 17px; font-weight: 700; margin-bottom: 18px; color: var(--text); }
  .form-group { margin-bottom: 14px; }
  .form-label { display: block; font-size: 12px; font-weight: 600; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 6px; }
  .form-input, .form-select { width: 100%; background: var(--surface); border: 1.5px solid var(--border); border-radius: 10px; padding: 11px 13px; font-size: 14px; color: var(--text); outline: none; transition: border-color 0.2s; -webkit-appearance: none; direction: ltr; text-align: left; }
  .form-input:focus, .form-select:focus { border-color: var(--accent); }
  .form-input::placeholder { color: var(--text-dim); }
  .form-select option { background: var(--surface); }
  .modal-footer { display: flex; gap: 10px; margin-top: 20px; }
  .btn-cancel { flex: 1; background: var(--surface); border: 1px solid var(--border); color: var(--text-muted); border-radius: 12px; padding: 13px; font-size: 15px; font-weight: 600; cursor: pointer; }
  .btn-save { flex: 2; background: linear-gradient(135deg, #d4b876, #b8923f); border: none; color: #171310; border-radius: 12px; padding: 13px; font-size: 15px; font-weight: 700; cursor: pointer; }
  .btn-save:active { filter: brightness(0.9); }

  /* CONFIRM */
  .confirm-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.75); z-index: 300; display: none; align-items: center; justify-content: center; padding: 20px; }
  .confirm-overlay.show { display: flex; }
  .confirm-box { background: var(--card); border: 1px solid var(--border); border-radius: var(--radius); padding: 24px 20px 20px; max-width: 320px; width: 100%; text-align: center; animation: popIn 0.2s ease; }
  @keyframes popIn { from { transform: scale(0.92); opacity: 0; } to { transform: scale(1); opacity: 1; } }
  .confirm-icon { font-size: 40px; margin-bottom: 12px; }
  .confirm-title { font-size: 16px; font-weight: 700; margin-bottom: 8px; }
  .confirm-text { font-size: 13px; color: var(--text-muted); margin-bottom: 20px; }
  .confirm-btns { display: flex; gap: 10px; }
  .btn-no { flex: 1; background: var(--surface); border: 1.5px solid var(--border); color: var(--text); border-radius: 10px; padding: 12px; font-size: 14px; font-weight: 600; cursor: pointer; }
  .btn-yes { flex: 1; background: var(--red); border: none; color: #fff; border-radius: 10px; padding: 12px; font-size: 14px; font-weight: 700; cursor: pointer; }

  /* TOAST */
  .toast { position: fixed; bottom: 90px; left: 50%; transform: translateX(-50%) translateY(20px); background: #201a0d; border: 1px solid var(--accent); color: var(--accent2); padding: 10px 20px; border-radius: 20px; font-size: 13px; font-weight: 600; opacity: 0; transition: all 0.3s; z-index: 400; white-space: nowrap; }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
  .image-preview-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 1000; display: none; align-items: center; justify-content: center; padding: 20px; }
  .image-preview-overlay.show { display: flex; }
  .image-preview-frame { max-width: calc(100vw - 40px); max-height: calc(100vh - 40px); border-radius: 18px; overflow: hidden; box-shadow: 0 20px 60px rgba(0,0,0,0.45); border: 1px solid rgba(255,255,255,0.12); }
  .image-preview-frame img { display: block; width: 100%; height: auto; max-height: calc(100vh - 40px); }

  .fab { position: fixed; bottom: 20px; left: 16px; width: 54px; height: 54px; background: linear-gradient(135deg, #d4b876, #a67c3d); border-radius: 50%; border: none; color: #171310; font-size: 24px; cursor: pointer; box-shadow: 0 8px 26px rgba(201,169,97,0.4); display: flex; align-items: center; justify-content: center; z-index: 50; transition: transform 0.2s; }
  .fab:active { transform: scale(0.92); }

  /* Category colors */
  .cat-pantograph { background: #1c1f2e; color: #a5abd6; }
  .cat-electric { background: #16261c; color: #6fcf97; }
  .cat-doors { background: #2a2015; color: #dba36c; }
  .cat-hvac { background: #12262a; color: #6fc2cf; }
  .cat-eaa { background: #271620; color: #d68ab0; }
  .cat-motor { background: #26200f; color: var(--accent2); }
  .cat-illumination { background: #29220f; color: #e0c589; }
  .cat-brake { background: #291515; color: #e08080; }
  .cat-wc { background: #10221f; color: #7fd4c4; }
  .cat-interior { background: #1e1826; color: #b498d1; }
  .cat-default { background: #1a1e28; color: #97a0ad; }

  /* Viewer locked overlay hint */
  .viewer-hint {
    position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
    background: #16261c; border: 1px solid var(--green); color: var(--green);
    padding: 9px 18px; border-radius: 20px; font-size: 12px; font-weight: 600;
    z-index: 50; display: flex; align-items: center; gap: 6px;
  }
</style>
</head>
<body>

<!-- LOGIN SCREEN -->
<div class="login-screen" id="loginScreen">
  <div class="login-box">
    <div class="login-logo">🔧</div>
    <div class="login-title">Buscador de Fichas</div>
    <div class="login-sub">نظام إدارة قطع الغيار</div>
    <label class="login-label">🔐 كلمة سر المسؤول</label>
    <input type="password" class="login-input" id="pwdInput" placeholder="أدخل كلمة السر..." onkeydown="if(event.key==='Enter') tryLogin()">
    <div class="login-error" id="loginError">❌ كلمة السر غلط، حاول مرة ثانية</div>
    <button class="login-btn" onclick="tryLogin()">🔓 دخول كمسؤول</button>
    <button class="login-viewer-btn" onclick="enterViewer()">👁️ دخول للمشاهدة فقط</button>
  </div>
</div>

<!-- MAIN APP -->
<div id="mainApp" style="display:none">
  <div class="header">
    <div class="header-top">
      <div class="logo-icon">🔧</div>
      <div>
        <div class="header-title">Buscador de Fichas</div>
        <div class="header-sub">نظام إدارة قطع الغيار</div>
      </div>
      <div class="header-right">
        <span class="role-badge" id="roleBadge" onclick="logout()"></span>
        <button class="trash-btn requests-btn" id="requestsBtn" onclick="openRequests()" style="display:none">
          📥<span class="trash-badge requests-badge" id="requestsBadge" style="display:none">0</span>
        </button>
        <button class="trash-btn temp-access-btn" id="tempAccessBtn" onclick="openTempAccessModal()" style="display:none" title="وضع الإضافة المؤقت للمشاهدين">
          ⏱️<span class="temp-access-dot"></span>
        </button>
        <button class="trash-btn" id="trashBtn" onclick="openTrash()" style="display:none">
          🗑️<span class="trash-badge" id="trashBadge" style="display:none">0</span>
        </button>
        <button class="add-btn" id="addBtn" onclick="openModal()" style="display:none">+ Add</button>
        <button class="request-part-btn" id="requestPartBtn" onclick="openRequestModal()" style="display:none">📩 طلب قطعة</button>
      </div>
    </div>
    <div class="search-wrap">
      <span class="search-icon">🔍</span>
      <input type="text" class="search-input" id="searchInput" placeholder="Search part, code, location..." oninput="handleSearch(this.value)">
      <button class="search-clear" id="clearBtn" onclick="clearSearch()">✕</button>
    </div>
    <div class="stats-bar" id="statsBar">Loading...</div>
  </div>

  <div class="filters" id="filtersBar"></div>
  <div class="cards-container" id="cardsContainer"></div>

  <!-- FAB only for admin -->
  <button class="fab" id="fabBtn" onclick="openModal()" style="display:none">+</button>
  <!-- FAB for viewer to request a part -->
  <button class="fab request-fab" id="requestFabBtn" onclick="openRequestModal()" style="display:none">📩</button>

  <!-- Viewer hint -->
  <div class="viewer-hint" id="viewerHint" style="display:none">👁️ وضع المشاهدة — لا يمكن التعديل</div>
  <!-- Temp access hint for viewer -->
  <div class="temp-hint" id="tempHint" style="display:none">🔓 وضع الإضافة مفعّل — متبقي <span id="tempHintTime">--:--</span></div>
</div>

<!-- MODAL -->
<div class="modal-overlay" id="modalOverlay" onclick="handleOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title" id="modalTitle">➕ New Part</div>
    <div class="form-group"><label class="form-label">Part Name *</label><input type="text" class="form-input" id="fName" placeholder="e.g. PANTOGRAPH Sliding Strip"></div>
    <div class="form-group"><label class="form-label">Material Code</label><input type="text" class="form-input" id="fCode" placeholder="e.g. 10060691"></div>
    <div class="form-group"><label class="form-label">Location</label><input type="text" class="form-input" id="fLocation" placeholder="e.g. HM-5R"></div>
    <div class="form-group">
      <label class="form-label">Subsystem</label>
      <select class="form-select" id="fSystem">
        <option value="">-- Select --</option>
        <option>PANTOGRAPH / HIGH TENS.</option><option>ELECTRIC</option><option>DOORS</option>
        <option>HVAC</option><option>EAA</option><option>MOTOR</option><option>ILLUMINATION</option>
        <option>Brake</option><option>WC</option><option>INTERIOR / MECHANICAL</option>
        <option>GENSET</option><option>TOOLS</option><option>Water</option>
        <option>Interiors</option><option>Refrigerators</option><option>Security cameras</option>
      </select>
    </div>
    <div class="form-group"><label class="form-label">Utilization / Description</label><input type="text" class="form-input" id="fUtil" placeholder="e.g. High voltage, Cafeteria..."></div>
    <div class="form-group"><label class="form-label">Image URL</label><input type="text" class="form-input" id="fImage" placeholder="e.g. https://example.com/image.jpg"></div>
    <div class="form-group" style="display:flex;gap:10px;align-items:center;">
      <button type="button" class="btn-cancel" style="flex:1;min-width:0;" onclick="removeImage()">إزالة الصورة</button>
      <input type="file" class="form-input" id="fImageFile" accept="image/*" onchange="loadImageFile(event)" style="flex:2;min-width:0;" />
    </div>
    <div class="form-group"><label class="form-label">الترجمة العربية</label><input type="text" class="form-input" id="fTrans" placeholder="مثال: بيك أب DBZ" dir="rtl" style="text-align:right"></div>
    <div class="form-group"><label class="form-label">النطق</label><input type="text" class="form-input" id="fPhonetic" placeholder="مثال: دي بي زد" dir="rtl" style="text-align:right"></div>
    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeModal()">Cancel</button>
      <button class="btn-save" onclick="savePart()">💾 Save</button>
    </div>
  </div>
</div>

<!-- CONFIRM DELETE -->
<div class="confirm-overlay" id="confirmOverlay">
  <div class="confirm-box">
    <div class="confirm-icon">🗑️</div>
    <div class="confirm-title">Delete Part?</div>
    <div class="confirm-text" id="confirmText">This action cannot be undone.</div>
    <div class="confirm-btns">
      <button class="btn-no" onclick="closeConfirm()">Cancel</button>
      <button class="btn-yes" onclick="confirmDelete()">Delete</button>
    </div>
  </div>
</div>

<!-- TRASH LIST MODAL -->
<div class="modal-overlay" id="trashOverlay" onclick="handleTrashOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">🗑️ سلة المحذوفات</div>
    <div id="trashList"></div>
    <div class="modal-footer">
      <button class="btn-cancel" style="flex:1" onclick="closeTrash()">إغلاق</button>
    </div>
  </div>
</div>

<!-- CONFIRM PERMANENT DELETE -->
<div class="confirm-overlay" id="purgeConfirmOverlay">
  <div class="confirm-box">
    <div class="confirm-icon">⚠️</div>
    <div class="confirm-title">حذف نهائي؟</div>
    <div class="confirm-text" id="purgeConfirmText">لا يمكن التراجع عن هذا الإجراء.</div>
    <div class="confirm-btns">
      <button class="btn-no" onclick="closePurgeConfirm()">إلغاء</button>
      <button class="btn-yes" onclick="confirmPurge()">حذف نهائي</button>
    </div>
  </div>
</div>

<!-- REQUEST A PART (viewer) -->
<div class="modal-overlay" id="requestModalOverlay" onclick="handleRequestOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">📩 طلب إضافة قطعة</div>
    <div class="form-group"><label class="form-label">اسم القطعة *</label><input type="text" class="form-input" id="rName" placeholder="مثال: PANTOGRAPH Sliding Strip"></div>
    <div class="form-group"><label class="form-label">رقم القطعة (إن وجد)</label><input type="text" class="form-input" id="rCode" placeholder="مثال: 10060691" dir="rtl" style="text-align:right"></div>
    <div class="form-group"><label class="form-label">الموقع (إن وجد)</label><input type="text" class="form-input" id="rLocation" placeholder="مثال: HM-5R"></div>
    <div class="form-group">
      <label class="form-label">النظام الفرعي</label>
      <select class="form-select" id="rSystem">
        <option value="">-- اختر --</option>
        <option>PANTOGRAPH / HIGH TENS.</option><option>ELECTRIC</option><option>DOORS</option>
        <option>HVAC</option><option>EAA</option><option>MOTOR</option><option>ILLUMINATION</option>
        <option>Brake</option><option>WC</option><option>INTERIOR / MECHANICAL</option>
        <option>GENSET</option><option>TOOLS</option><option>Water</option>
        <option>Interiors</option><option>Refrigerators</option><option>Security cameras</option>
      </select>
    </div>
    <div class="form-group"><label class="form-label">ملاحظة / سبب الطلب</label><input type="text" class="form-input" id="rNote" placeholder="مثال: القطعة مطلوبة لصيانة..." dir="rtl" style="text-align:right"></div>
    <div class="form-group">
      <label class="form-label">إرفاق صورة (اختياري)</label>
      <input type="file" class="form-input" id="rImageFile" accept="image/*" onchange="loadRequestImageFile(event)">
      <div id="rImagePreviewWrap" style="display:none;margin-top:10px;align-items:center;gap:10px;">
        <img id="rImagePreview" src="" alt="معاينة" style="width:54px;height:54px;border-radius:12px;object-fit:cover;border:1.5px solid var(--border);">
        <button type="button" class="btn-cancel" style="flex:1;min-width:0;" onclick="removeRequestImage()">إزالة الصورة</button>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeRequestModal()">إلغاء</button>
      <button class="btn-save" onclick="submitRequest()">📨 إرسال الطلب</button>
    </div>
  </div>
</div>

<!-- TEMP ACCESS MODAL (admin only) -->
<div class="modal-overlay" id="tempAccessModalOverlay" onclick="handleTempAccessOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">⏱️ وضع الإضافة المؤقت للمشاهدين</div>
    <div style="font-size:12.5px;color:var(--text-muted);margin-top:-10px;margin-bottom:16px;line-height:1.6;">
      خلال الفترة المحددة، يقدر أي مشاهد يضيف قطعة مباشرة بدون ما ترسل كطلب للموافقة.
    </div>

    <div class="temp-modal-status" id="tempModalStatus">
      <span class="temp-status-text" id="tempModalStatusText">🔒 الوضع غير مفعّل حالياً</span>
      <button class="temp-end-btn" id="tempEndBtn" style="display:none" onclick="endTempAccessNow()">إنهاء الآن</button>
    </div>

    <div class="form-label" style="margin-bottom:10px;">اختر مدة التفعيل</div>
    <div class="temp-duration-grid">
      <button class="temp-duration-btn" onclick="activateTempAccess(15)">15 دقيقة</button>
      <button class="temp-duration-btn" onclick="activateTempAccess(30)">30 دقيقة</button>
      <button class="temp-duration-btn" onclick="activateTempAccess(60)">ساعة واحدة</button>
      <button class="temp-duration-btn" onclick="activateTempAccess(180)">3 ساعات</button>
      <button class="temp-duration-btn" onclick="activateTempAccess(480)">8 ساعات</button>
      <button class="temp-duration-btn" onclick="activateTempAccess(1440)">يوم كامل</button>
    </div>

    <div class="form-label">أو مدة مخصصة (بالدقائق)</div>
    <div class="temp-custom-row form-group">
      <input type="number" class="form-input" id="tempCustomMinutes" placeholder="مثال: 45" min="1" style="flex:1;">
      <button class="btn-save" style="flex:1;" onclick="activateTempAccessCustom()">✅ تفعيل</button>
    </div>

    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeTempAccessModal()">إغلاق</button>
    </div>
  </div>
</div>

<!-- REQUESTS LIST (admin) -->
<div class="modal-overlay" id="requestsOverlay" onclick="handleRequestsOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">📥 طلبات إضافة قطع</div>
    <div id="requestsList"></div>
    <div class="modal-footer">
      <button class="btn-cancel" style="flex:1" onclick="closeRequests()">إغلاق</button>
    </div>
  </div>
</div>

<!-- REQUEST DETAIL (full view, admin) -->
<div class="modal-overlay" id="requestDetailOverlay" onclick="handleRequestDetailOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">📩 تفاصيل الطلب</div>
    <div id="rdImageWrap" style="display:none;margin-bottom:16px;">
      <img id="rdImage" src="" alt="صورة الطلب" style="width:100%;max-height:260px;object-fit:cover;border-radius:14px;border:1.5px solid var(--border);cursor:zoom-in;" onclick="openImagePreview(document.getElementById('rdImage').src)">
    </div>
    <div id="rdName" style="font-size:18px;font-weight:700;color:var(--text);margin-bottom:10px;"></div>
    <div id="rdMetaRows" style="display:flex;flex-direction:column;gap:8px;margin-bottom:14px;"></div>
    <div id="rdNoteWrap" style="display:none;">
      <label class="form-label">ملاحظة / سبب الطلب</label>
      <div id="rdNote" class="request-item-note" style="margin-top:6px;"></div>
    </div>
    <div class="modal-footer" style="margin-top:18px;">
      <button class="request-reject-btn" style="flex:1;padding:12px;" onclick="askRejectRequest(rdCurrentId);closeRequestDetail()">❌ رفض</button>
      <button class="request-accept-btn" style="flex:1;padding:12px;" onclick="acceptRequest(rdCurrentId)">➕ إضافة</button>
    </div>
  </div>
</div>

<!-- CONFIRM REJECT REQUEST -->
<div class="confirm-overlay" id="rejectConfirmOverlay">
  <div class="confirm-box">
    <div class="confirm-icon">❌</div>
    <div class="confirm-title">رفض الطلب؟</div>
    <div class="confirm-text" id="rejectConfirmText">سيتم حذف هذا الطلب.</div>
    <div class="confirm-btns">
      <button class="btn-no" onclick="closeRejectConfirm()">إلغاء</button>
      <button class="btn-yes" onclick="confirmRejectRequest()">رفض الطلب</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>
<div class="image-preview-overlay" id="imagePreviewOverlay" onclick="closeImagePreview(event)">
  <div class="image-preview-frame"><img id="previewImage" src="" alt="Preview"></div>
</div>

<script>
const ADMIN_PASSWORD = 'ahmad@054';
const CATEGORIES = ['Todos','PANTOGRAPH / HIGH TENS.','ELECTRIC','DOORS','HVAC','EAA','MOTOR','ILLUMINATION','Brake','WC'];
const SYSTEM_ICONS = {
  'PANTOGRAPH / HIGH TENS.':{icon:'⚡',cls:'cat-pantograph'},
  'ELECTRIC':{icon:'🔌',cls:'cat-electric'},
  'DOORS':{icon:'🚪',cls:'cat-doors'},
  'HVAC':{icon:'❄️',cls:'cat-hvac'},
  'EAA':{icon:'🌀',cls:'cat-eaa'},
  'MOTOR':{icon:'⚙️',cls:'cat-motor'},
  'ILLUMINATION':{icon:'💡',cls:'cat-illumination'},
  'Brake':{icon:'🛑',cls:'cat-brake'},
  'WC':{icon:'🚿',cls:'cat-wc'},
  'INTERIOR / MECHANICAL':{icon:'🔩',cls:'cat-interior'},
  'Interiors':{icon:'🪑',cls:'cat-interior'},
  'GENSET':{icon:'🔋',cls:'cat-motor'},
  'TOOLS':{icon:'🛠️',cls:'cat-default'},
  'Water':{icon:'💧',cls:'cat-hvac'},
  'Refrigerators':{icon:'🧊',cls:'cat-hvac'},
  'Security cameras':{icon:'📷',cls:'cat-default'},
};

const INITIAL_DATA = [
  {name:"Useful alento pantograph",code:"5207186A",location:"D-1-2",system:"PANTOGRAPH / HIGH TENS.",util:"Aleron"},
  {name:"Ecometer",code:"10040722",location:"",system:"PANTOGRAPH / HIGH TENS.",util:"High voltage meter"},
  {name:"Spray tests smoke detectors",code:"10018889",location:"",system:"ELECTRIC",util:""},
  {name:"HMI Refrigerator Monitor",code:"10049409",location:"",system:"Refrigerators",util:"Car 5"},
  {name:"MCB control card",code:"10067179",location:"",system:"ELECTRIC",util:""},
  {name:"TAR car with cable",code:"5127401A",location:"M-4-2",system:"MOTOR",util:"TAR"},
  {name:"Wago 4 pole male",code:"416094",location:"",system:"ELECTRIC",util:""},
  {name:"Wago 4 pole female",code:"416095",location:"",system:"ELECTRIC",util:""},
  {name:"Kitchen access door set",code:"5142514A",location:"",system:"DOORS",util:"Cafeteria"},
  {name:"Remote box high voltage measuring",code:"10043746",location:"G-0",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Pole",code:"10053719",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Disturbance cable",code:"10053720",location:"G-2-1",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Distin catenary clamp",code:"10065652",location:"Q-1-1",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"230V enchife pictogram",code:"5164371A",location:"D-3-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"Pilot indicator light small for PHs desk",code:"10035109",location:"D-4-3",system:"ELECTRIC",util:""},
  {name:"Indicator button",code:"1008284",location:"",system:"ELECTRIC",util:""},
  {name:"Indicator pilot lamp",code:"10044265",location:"M-3-3",system:"ELECTRIC",util:""},
  {name:"Monitoring Station PC CCTV",code:"5136607A",location:"D-2-1",system:"Security cameras",util:"Camaras de seguridad"},
  {name:"Grease exterior doors",code:"10047662",location:"CH-R3",system:"DOORS",util:"Perfil elastomero"},
  {name:"Bar door bar handle",code:"10077757",location:"D-3-2",system:"DOORS",util:""},
  {name:"C05 Cafeteria door bar brake",code:"10077413",location:"",system:"DOORS",util:"Cafeteria"},
  {name:"PMR led light",code:"5007930A",location:"M-3-3",system:"ILLUMINATION",util:"Luminarias WC"},
  {name:"Led light cars",code:"10026486",location:"M-3-4",system:"ILLUMINATION",util:"Luminaria coches"},
  {name:"Trash Halls",code:"5124221A",location:"F-3-3",system:"Interiors",util:"Pepeleras de coches"},
  {name:"Paper bin",code:"5149142A",location:"",system:"Interiors",util:"Papeleras"},
  {name:"Soap pumping valve",code:"10065189",location:"D-2-3",system:"Interiors",util:"Jaboneras"},
  {name:"Right support blind sunshade",code:"5193490A",location:"A-1-3",system:"Interiors",util:"Estor coches"},
  {name:"Left support blind sunshade",code:"5193489A",location:"A-1-3",system:"Interiors",util:"Estor coches"},
  {name:"ABB battery charger temperature sensor",code:"10051443",location:"",system:"ELECTRIC",util:"ABB Battery Charger"},
  {name:"Earth braid door battery modules Ground",code:"5144195A",location:"C-2-3",system:"ELECTRIC",util:""},
  {name:"Speaker for Cars",code:"5125226A",location:"D-4-1",system:"ELECTRIC",util:""},
  {name:"Continuous Isolation Monitor",code:"10040507",location:"M-3-3",system:"ELECTRIC",util:""},
  {name:"Thermal paste",code:"123135",location:"CH-R-1",system:"TOOLS",util:""},
  {name:"Flange L 150mm",code:"10038037",location:"D-3-1",system:"ELECTRIC",util:""},
  {name:"Circular 7 pin connector",code:"10044084",location:"D-1-3",system:"ELECTRIC",util:""},
  {name:"Useful alento pantograph",code:"5207186B",location:"",system:"PANTOGRAPH / HIGH TENS.",util:"Aleron"},
  {name:"ELECTRONICS OUTDOOR DOORS DCU",code:"10048442",location:"R-4-2",system:"DOORS",util:"PUERTAS EXTERIORES"},
  {name:"KETA torica board",code:"10061283",location:"",system:"ELECTRIC",util:""},
  {name:"KETA complete distributor valve",code:"10040730",location:"H-0",system:"ELECTRIC",util:""},
  {name:"EP compact EPC-C without parking brake",code:"5125721A",location:"HM 1R",system:"Brake",util:"Coches con freno"},
  {name:"EP compact EPC-CP with parking brake",code:"5125720A",location:"HM 1R",system:"Brake",util:"Coches con freno"},
  {name:"External door silencer Filter",code:"10072501",location:"D-4-2",system:"DOORS",util:""},
  {name:"2P 10A switch",code:"10025613",location:"",system:"ELECTRIC",util:""},
  {name:"Flashlight Halogen lamp",code:"10024950",location:"D-4-2",system:"TOOLS",util:"PHs"},
  {name:"WC door",code:"5111676A",location:"",system:"WC",util:"DOORS"},
  {name:"Water shower pressure regulator bide wc tap",code:"10037180",location:"D-3-3",system:"WC",util:"Panel agua WC"},
  {name:"Manometer 0 to 10 bar",code:"610033",location:"Q-3-3",system:"Water",util:""},
  {name:"Water purge electrovalve valve",code:"610249",location:"M-3-1",system:"Water",util:""},
  {name:"Right rearview camera",code:"5130749A",location:"",system:"Security cameras",util:""},
  {name:"Left rearview camera",code:"5130748A",location:"G-3",system:"Security cameras",util:""},
  {name:"Riom-s car",code:"5127408A",location:"H-1",system:"ELECTRIC",util:""},
  {name:"WSP anti-lock",code:"5131889B",location:"W-2-1",system:"Brake",util:"Freno Motriz"},
  {name:"DNRA anti-lock",code:"5131889A",location:"H-0",system:"Brake",util:"Freno Motriz"},
  {name:"PH CRYSTAL PROTECTOR FILM",code:"10049980",location:"R-4-3",system:"INTERIOR / MECHANICAL",util:""},
  {name:"THERMAL MAGNETO Circuit Breaker 1.6 A",code:"10032600",location:"D-1-2",system:"ELECTRIC",util:"Hvac Evaporadores"},
  {name:"SENSOR INTERIOR DOORS S7",code:"5125901A",location:"M-3-3",system:"DOORS",util:""},
  {name:"COFFEE MAKER",code:"10055301",location:"",system:"Interiors",util:"Cafeteria"},
  {name:"1A FUSE",code:"412430",location:"D-2-3",system:"ELECTRIC",util:""},
  {name:"GENERATOR GRILLE",code:"5129645A",location:"HM-1R",system:"GENSET",util:""},
  {name:"Dopler radar connector",code:"5132593A",location:"F-2-2",system:"MOTOR",util:"ETCS ERTMS Radar"},
  {name:"ROLDANA DOOR WC",code:"10043997",location:"D-3-2",system:"WC",util:""},
  {name:"Wiper Motor",code:"5406179A",location:"F-2-2",system:"MOTOR",util:""},
  {name:"EXTERNAL DOOR COVER",code:"5134981A",location:"Q-8-3",system:"DOORS",util:""},
  {name:"POWER SUPPLY",code:"5125247A",location:"M-4-3",system:"ELECTRIC",util:"Power Supply for VoD"},
  {name:"INTERIOR DOORS BUTTON",code:"5125861",location:"M-3-3",system:"DOORS",util:""},
  {name:"PANTOGRAPH Sliding Strip",code:"10060691",location:"HM-5R",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"T5 8KW fluorescent tube",code:"442029",location:"Q-5-3",system:"ILLUMINATION",util:""},
  {name:"G13 18KW fluorescent tube",code:"442032",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"Ceiling hall Light VESTIB TALGO AVRIL G3",code:"5097681A",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"ROOF LIGHT",code:"5132208A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"5/2 VIAS PNEUMATIC VALVE For Damper",code:"10046463",location:"Q-6-3",system:"ELECTRIC",util:"Aire"},
  {name:"EXTRACTING UNIT",code:"5115927A",location:"HM-4R",system:"PANTOGRAPH / HIGH TENS.",util:"CABLES DE ALTA TENSION"},
  {name:"EXTRACTOR CYLINDER",code:"10046469",location:"Q-6-3",system:"ELECTRIC",util:"Cars Dampers"},
  {name:"HIGH MOTOR CABLE CEILING",code:"5132201A",location:"H-1-0",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"HIGH CABLE TRENCILLA",code:"5133431A",location:"Q-2-3",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"BATTERY CHARGER",code:"5109997A",location:"HM-4R",system:"ELECTRIC",util:""},
  {name:"ELECTRICAL CABINET CHARGER BATTERIES",code:"5128404A",location:"G-3",system:"ELECTRIC",util:""},
  {name:"EVAPORATOR MOTOR for HVAC",code:"10046471",location:"G-1-0",system:"HVAC",util:"Evaporadores, condensadores"},
  {name:"HIGH PRESSURE FAN NOISE DAMPER",code:"10004341",location:"",system:"HVAC",util:""},
  {name:"EAA EXTRACTOR MOTOR HVAC",code:"10046504",location:"H-3",system:"EAA",util:""},
  {name:"VCU CAR 1",code:"5127405A",location:"N-1-3",system:"ELECTRIC",util:""},
  {name:"OUTDOOR DOOR OPENING BUTTON",code:"5123805A",location:"Q-3-2",system:"DOORS",util:""},
  {name:"BATTERY CHARGER MODULE",code:"10051406",location:"HM-9R",system:"ELECTRIC",util:"ABB Battery Charger"},
  {name:"ASFA 110 V RELAY",code:"10038529",location:"M-3-3",system:"MOTOR",util:"Asfa"},
  {name:"WC DOOR HANDLE",code:"10044051",location:"Q-1-2",system:"WC",util:""},
  {name:"Hvac",code:"5115926A",location:"",system:"HVAC",util:""},
  {name:"HVAC gauge",code:"10064385",location:"Q-4-4",system:"HVAC",util:""},
  {name:"ROOM CONDENSER MOTOR",code:"10046477",location:"G-1-0",system:"HVAC",util:""},
  {name:"RETENTION VALVE WATER",code:"10003966",location:"D-1-2",system:"Water",util:""},
  {name:"FILTER TYPE Y WATER",code:"610282",location:"Q-1-1",system:"Water",util:""},
  {name:"CDU",code:"5127404A",location:"F-2-1",system:"ELECTRIC",util:""},
  {name:"MOTOR ROOM MACHINE FAN",code:"5125886A",location:"HM-3",system:"MOTOR",util:"Air Ventilacion sala de maquinas"},
  {name:"EAA DRAIN ELBOW for HVAC",code:"10049619",location:"M-3-4",system:"EAA",util:"Desague HVAC"},
  {name:"Standard PAPER GUIDE",code:"10049419",location:"D-1-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"EXTERNAL DOOR MECHANISM SH-815",code:"5130920A",location:"HM-1R",system:"DOORS",util:""},
  {name:"BRAKE FLUID",code:"10046364",location:"HM",system:"Brake",util:"Bogis"},
  {name:"WC Complete Seta bowl",code:"5141038A",location:"HM-1R",system:"WC",util:""},
  {name:"Cabin HVAC",code:"5116169A",location:"HM-3R",system:"HVAC",util:"PHS Cabin"},
  {name:"GPS ANTENNA",code:"10041230",location:"D-3-3",system:"ELECTRIC",util:""},
  {name:"PREF ARMCHAIR KEYBOARD",code:"5125248A",location:"Q-5-3",system:"INTERIOR / MECHANICAL",util:""},
  {name:"INDV MONITOR PREF VOD Seat monitor",code:"5125245A",location:"S-2-2",system:"ELECTRIC",util:"Monitors Group"},
  {name:"SHOWER-BIDET",code:"5125880A",location:"A-2-3",system:"WC",util:""},
  {name:"AIR PANEL",code:"5125890A",location:"S-2-2",system:"ELECTRIC",util:""},
  {name:"LLAVIN KEY for Train Doors",code:"10014266",location:"D-4-2",system:"DOORS",util:"Key for train"},
  {name:"PAU",code:"5125220A",location:"Q-3-4",system:"ELECTRIC",util:""},
  {name:"ETHERNET GREEN CABLE M12-RJ45",code:"10044024",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"TRACTION-BRAKE MANIPULATOR",code:"5130468A",location:"W-2-3",system:"MOTOR",util:"Manipulador de traccion"},
  {name:"COMPRESSOR OIL",code:"10014695",location:"HM-10",system:"MOTOR",util:"PHS Main Compressor"},
  {name:"CONVERTER CAR LIGHTS 110 35W",code:"5043034A",location:"F-2-1",system:"ELECTRIC",util:"Converter Motriz"},
  {name:"Circuit Breaker MCB 25KV",code:"5129151A",location:"HM-4",system:"PANTOGRAPH / HIGH TENS.",util:"HIGH VOLTAGE MOTRIZ"},
  {name:"Terminal panel electric air compressor",code:"10075845",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"Instant relay 4 switched 110VDC",code:"10013410",location:"F-4-1",system:"ELECTRIC",util:"Aire"},
  {name:"Three-pole contactor 48-130 VDC",code:"10032535",location:"M-2-4",system:"ELECTRIC",util:"Aire"},
  {name:"K1 AIR COMPRESSOR CONTACTOR",code:"10040254",location:"M-4-2",system:"ELECTRIC",util:"Aire"},
  {name:"COMPLETE PANTOGRAPH",code:"5126061A",location:"HM-8R",system:"PANTOGRAPH / HIGH TENS.",util:"Electrical"},
  {name:"MICRO DOOR WC",code:"10033111",location:"M-3-3",system:"WC",util:""},
  {name:"Fuse 2A",code:"10051521",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"5A fuse",code:"10051522",location:"D-2-3",system:"ELECTRIC",util:""},
  {name:"10A fuse",code:"10051523",location:"D-1-3",system:"ELECTRIC",util:""},
  {name:"CEILING LIGHT LED CARS",code:"5132262A",location:"Q-8-2",system:"ILLUMINATION",util:"Roof Ceiling LED lights"},
  {name:"GENSET BATTERY",code:"10060918",location:"H-0",system:"GENSET",util:"Gen-Set Generador"},
  {name:"K174 WIPER RELAY",code:"10015045",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"Cellar evaporator fan",code:"10073349",location:"R-3-2",system:"Refrigerators",util:"Cold Storage"},
  {name:"Cabinet Fridge",code:"5133843A",location:"N-1-2",system:"Refrigerators",util:"Fridge Cabin"},
  {name:"LEVEL SENSORS",code:"10027594",location:"D-3-2",system:"ELECTRIC",util:"Water System tank level"},
  {name:"EGRU",code:"5124861A",location:"W-3-2",system:"ELECTRIC",util:""},
  {name:"EXTINGUISHER",code:"10048634",location:"HM-4",system:"TOOLS",util:""},
  {name:"ADD PANTOGRAPH VALVE",code:"10061507",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"MENBRANA PANTOGRAPH",code:"10071835",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Microwave",code:"10066387",location:"G-0-2",system:"Interiors",util:"Car 05"},
  {name:"OVEN",code:"5142530A",location:"",system:"Interiors",util:"Car05"},
  {name:"WC BUTTON",code:"10033105",location:"Q-1-3",system:"WC",util:""},
  {name:"BATTERY CHARGER FAN",code:"10051430",location:"S-1-2",system:"ELECTRIC",util:"ABB Battery Charger"},
  {name:"EAA CAR CONTROL MODULE",code:"5115928A",location:"W-3-4",system:"EAA",util:"Control Card for HVAC"},
  {name:"EAA CAR CARD for HVAC Cars",code:"10046527",location:"F-1-2",system:"EAA",util:"Controal Card for HVAC"},
  {name:"EAA COMPLETE",code:"5115926",location:"",system:"EAA",util:""},
  {name:"DVR recorder Hard Disk",code:"5126589A",location:"S-1-1",system:"Security cameras",util:""},
  {name:"MOBILE VOICE RECORDER",code:"5146913A",location:"S-2-2",system:"Security cameras",util:""},
  {name:"CIP",code:"5125221A",location:"Q-4-4",system:"ELECTRIC",util:""},
  {name:"EAA MOTOR MODE SELECTOR",code:"10043962",location:"R-4-3",system:"EAA",util:""},
  {name:"EAA MOTOR EMERGENCY SELECTOR",code:"10043963",location:"R-4-3",system:"EAA",util:""},
  {name:"WC ELECTRONICS",code:"5125879A",location:"W-1-2",system:"WC",util:"Electronic Card for WC"},
  {name:"EMERGENCY LIGHT for seat Economy",code:"5140306B",location:"C-2-3",system:"ILLUMINATION",util:"Luces de emergencias"},
  {name:"WC Hand Dryer",code:"10041088",location:"D",system:"WC",util:""},
  {name:"EMERGENCY PUSH BUTTON WC Inside",code:"10032837",location:"Q-4-2",system:"WC",util:""},
  {name:"BCU1",code:"5131882A",location:"W-1-3",system:"ELECTRIC",util:""},
  {name:"BCU2",code:"5131882B",location:"F-2-3",system:"ELECTRIC",util:""},
  {name:"Exterior door motor",code:"10048432",location:"Q-7-2",system:"DOORS",util:"Exterior doors"},
  {name:"Interior Door Motor",code:"10065043",location:"",system:"DOORS",util:"Inner door"},
  {name:"INTERNAL DOOR MOTOR",code:"10051627",location:"Q-4-3",system:"DOORS",util:""},
  {name:"SH750 DOOR MECHANISM ASSEMBLY INT",code:"5130922A",location:"H-1",system:"DOORS",util:""},
  {name:"GENERATOR EXTINGUISHER",code:"5133188A",location:"M-4-2",system:"GENSET",util:""},
  {name:"Capacitor CONVERTER CONDENSER",code:"10063713",location:"",system:"ELECTRIC",util:"Capacitor"},
  {name:"CONDENSER CONVERTER Old",code:"10040558",location:"H-0",system:"ELECTRIC",util:"Capacitor"},
  {name:"EXTERIOR INFORMATION DISPLAY PANEL",code:"5148672A",location:"W-1-3",system:"ELECTRIC",util:""},
  {name:"WC TAP for Sink",code:"10033109",location:"F-3-2",system:"WC",util:""},
  {name:"LED STRIPE BOX",code:"5134532A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"PANTOGRAPH Neumatic PANEL",code:"5131338A",location:"H-1",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"CONDENSING UNIT",code:"10049407",location:"HM-1R",system:"HVAC",util:""},
  {name:"EAA PACKING UNIT",code:"5115739A",location:"",system:"EAA",util:""},
  {name:"CU-AG rod",code:"130008",location:"",system:"ELECTRIC",util:""},
  {name:"Swivel Pivot Bolt",code:"5216893A",location:"",system:"INTERIOR / MECHANICAL",util:"Mecanico Enganche morro"},
  {name:"Coolant converters",code:"10048496",location:"",system:"ELECTRIC",util:"Convertidores"},
  {name:"Antena GSMR-GPS ERTMS/ETCS",code:"10031619",location:"",system:"ELECTRIC",util:""},
  {name:"Fire Extinguisher",code:"10048634",location:"",system:"TOOLS",util:""},
  {name:"Boiler Heater",code:"10066388",location:"",system:"ELECTRIC",util:""},
  {name:"SCHUKO TYPE SOCKET XS2",code:"10037956",location:"",system:"ELECTRIC",util:""},
  {name:"Battery Charger Pump",code:"10051431",location:"",system:"ELECTRIC",util:"ABB Battery Charger"},
  {name:"Vagner Fire Protection",code:"5125832A",location:"",system:"ELECTRIC",util:"Wanger Fire Protection"},
  {name:"Roof Switch for roof line 25KV",code:"5129152A",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Non return valve for water system",code:"10003966",location:"Q-9-3",system:"Water",util:""},
  {name:"TEMPERATURE SENSOR for Boji",code:"10055578",location:"",system:"ELECTRIC",util:"PHs Temp Sensor"},
  {name:"RELAY TERMINAL GREEN Relay",code:"10018362",location:"",system:"ELECTRIC",util:""},
  {name:"SOLENOID VALVE-COIL 24V",code:"10046479",location:"",system:"ELECTRIC",util:""},
  {name:"CLEANER LUBRICANT for Electrical CRC",code:"121518",location:"",system:"TOOLS",util:"Cleaner for Elect. Contact"},
  {name:"TEMPERATURE SENSOR for Bogie GearBox",code:"10055582",location:"",system:"ELECTRIC",util:"PHs Gearbox"},
  {name:"DIRECT ROOF LIGHTING RIGHT/LEFT",code:"5136223A",location:"",system:"ILLUMINATION",util:"CARS DIRECT ROOF LIGHTING"},
  {name:"Power Head External Door Rubber SEALING PROFILE",code:"10025861",location:"",system:"DOORS",util:""},
  {name:"INSULATOR for Pantograph",code:"10061509",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"TEMPERATURE SENSOR STATOR",code:"10055585",location:"",system:"ELECTRIC",util:""},
  {name:"Aux Convertor Power Module",code:"5114676A",location:"",system:"ELECTRIC",util:"PHs Convertor BT / Alstom"},
  {name:"EVC ERTMS/ETCS",code:"5131642A",location:"",system:"ELECTRIC",util:"PHs ERTMS"},
  {name:"Fire Detector for Cars",code:"5125831A",location:"",system:"ELECTRIC",util:"CARS SMOKE DETECTOR"},
  {name:"CYCLONIC EXTRACTORS Fan",code:"5125885A",location:"",system:"MOTOR",util:""},
  {name:"Main Transformer Oil",code:"10050329",location:"",system:"MOTOR",util:"PHs"},
  {name:"CCTV HMI",code:"5126592A",location:"",system:"Security cameras",util:""},
  {name:"External Light front Head Top",code:"5134955A",location:"",system:"ILLUMINATION",util:"PHS Light"},
  {name:"Air dryer for Compressor",code:"Z5125715A",location:"",system:"MOTOR",util:"PHS Main Compressor"},
  {name:"IMPULSE SOLENOID VALVE air dryer",code:"10056396",location:"",system:"ELECTRIC",util:""},
  {name:"Battery new for trains",code:"10040502",location:"",system:"ELECTRIC",util:"Cars Batteries"},
  {name:"PMR DCU Door control Unit",code:"10050797",location:"",system:"DOORS",util:"PMR door"},
  {name:"Out door Transformer 25KV PT",code:"5130603A",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Electric Contact Grease",code:"10016943",location:"",system:"TOOLS",util:""},
  {name:"WIFI ACCESS POINT Indra",code:"5131397A",location:"",system:"ELECTRIC",util:"Car 04"},
  {name:"Under sink door",code:"5110176A",location:"",system:"WC",util:""},
  {name:"FRESH AIR MOTOR-FAN ASSEMBLY for HVAC",code:"10054650",location:"",system:"HVAC",util:""},
  {name:"Spring for Brake Pad",code:"10075660",location:"",system:"Brake",util:""},
  {name:"FRONT Sunshade for PH",code:"5238917A",location:"",system:"Interiors",util:"Sun shade PHs"},
  {name:"ELECTRIC MOTOR for sunshade for PH",code:"10082309",location:"",system:"ELECTRIC",util:"Estor PHs"},
  {name:"Welding rod",code:"135008",location:"",system:"TOOLS",util:""},
  {name:"Train batteries water",code:"133401",location:"",system:"Water",util:""},
  {name:"Generator Coolant",code:"10060392",location:"",system:"GENSET",util:""},
  {name:"COMPRESSOR WASHER after Oil fill",code:"10033358",location:"",system:"MOTOR",util:""},
  {name:"INTERNAL DOOR DCU",code:"10050797",location:"",system:"DOORS",util:""},
  {name:"CCTV Camera",code:"5126591A",location:"",system:"Security cameras",util:""},
  {name:"Push Button for Interior Door",code:"5154087A",location:"",system:"DOORS",util:""},
  {name:"Emergency Brake Valve Push button Brake",code:"5121989A",location:"",system:"Brake",util:"PHs Brake System"},
  {name:"Fuse 1250",code:"10050080",location:"",system:"ELECTRIC",util:"Cars Main Fuses"},
  {name:"DMI",code:"5131753A",location:"",system:"ELECTRIC",util:"PHs"},
  {name:"PMR TOILET LOCK SLAVE ASSEMBLY",code:"5150025A",location:"",system:"WC",util:"PMR door"},
  {name:"Loctite alfi quick",code:"126492",location:"",system:"TOOLS",util:""},
  {name:"OIL CONTROL UNIT",code:"10060613",location:"",system:"MOTOR",util:""},
  {name:"MAIN ELEMENT",code:"10081317",location:"",system:"MOTOR",util:""},
  {name:"O-RING",code:"10081279",location:"",system:"MOTOR",util:""},
  {name:"OIL FILT CARTRIDGE",code:"10048647",location:"",system:"MOTOR",util:""},
  {name:"THERMOSTAT",code:"10081278",location:"",system:"ELECTRIC",util:""},
  {name:"Coffee machine filter support",code:"10065350",location:"",system:"Interiors",util:"Car 05 Cafeteria"},
  {name:"180 OPENING HINGE Cold storage and cellar",code:"5078889A",location:"",system:"Refrigerators",util:""},
  {name:"Arm Rest for Business",code:"5140011A",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"External Door Limit switch S10",code:"10048451",location:"",system:"DOORS",util:""},
  {name:"HOURS COUNTER for compressor",code:"10071226",location:"",system:"MOTOR",util:""},
  {name:"Speed sensor Rudals IMPULSES SENSOR",code:"5143528A",location:"",system:"MOTOR",util:""},
  {name:"TORSION SPRING for coupler front locking door",code:"10083699",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"RADIATOR of ABB Battery Charger",code:"10051432",location:"",system:"ELECTRIC",util:"ABB Battery charger"},
  {name:"Non return valve for water system White",code:"10027449",location:"",system:"Water",util:""},
  {name:"Talgo rodal logo",code:"5006877A",location:"",system:"INTERIOR / MECHANICAL",util:"Rodadura"},
  {name:"Useful alento pantograph",code:"5207186A",location:"D-1-2",system:"PANTOGRAPH / HIGH TENS.",util:"Aleron"},
  {name:"Ecometer",code:"10040722",location:"",system:"",util:"Alta"},
  {name:"Spray tests smoke detectors",code:"10018889",location:"",system:"",util:""},
  {name:"HMI Refrigerator Monitor",code:"10049409",location:"",system:"",util:""},
  {name:"MCB control card",code:"10067179",location:"",system:"",util:"MCB"},
  {name:"TAR car with cable",code:"5127401A",location:"M-4-2",system:"",util:"Tracductor aceleración rodadura"},
  {name:"Wago 4 pole male",code:"416094",location:"",system:"",util:""},
  {name:"Wago 4 pole female",code:"416095",location:"",system:"",util:""},
  {name:"Kitchen access door set",code:"5142514A",location:"",system:"",util:""},
  {name:"Remote box, high voltage measuring equipment.",code:"10043746",location:"G-0",system:"",util:""},
  {name:"Pole",code:"10053719",location:"",system:"",util:""},
  {name:"Disturbance cable",code:"10053720",location:"G-2-1",system:"",util:""},
  {name:"Distin catenary clamp",code:"10065652",location:"Q-1-1",system:"",util:""},
  {name:"230V enchife pictogram",code:"5164371A",location:"D-3-1",system:"Interiors",util:""},
  {name:"Pilot indicator light small for PHs desk",code:"10035109",location:"D-4-3",system:"",util:"escritorio del maquinista"},
  {name:"Indicator button",code:"1008284",location:"",system:"",util:"Generador"},
  {name:"Indicator pilot lamp",code:"10044265",location:"M-3-3",system:"",util:""},
  {name:"Monitoring Station PC (security cameras) CCTV",code:"5136607A",location:"D-2-1",system:"Security cameras",util:"Camaras de seguridad"},
  {name:"Grease exterior doors",code:"10047662",location:"CH-R3",system:"",util:"Perfil elastomero"},
  {name:"Bar door bar handle",code:"10077757",location:"D-3-2",system:"Interiors",util:"Cafeteria"},
  {name:"C05 Cafeteria door bar brake",code:"10077413",location:"",system:"Interiors",util:"Cafeteria"},
  {name:"PMR led light",code:"5007930A",location:"M-3-3",system:"",util:"Luminarias WC"},
  {name:"Led light cars",code:"10026486",location:"M-3-4",system:"",util:"Luminaria coches"},
  {name:"Trash Halls",code:"5124221A",location:"F-3-3",system:"Interiors",util:"Pepeleras de coches"},
  {name:"Paper bin",code:"5149142A",location:"",system:"Interiors",util:"Papeleras"},
  {name:"Welded paper basket",code:"5149143A",location:"",system:"WC",util:"Papeleras WC"},
  {name:"Soap pumping valve",code:"10065189",location:"D-2-3",system:"Interiors",util:"Jaboneras"},
  {name:"Right support for blind sunshade",code:"5193490A",location:"A-1-3",system:"Interiors",util:"Estor coches"},
  {name:"Left support for blind sunshade",code:"5193489A",location:"A-1-3",system:"Interiors",util:"Estor coches"},
  {name:"ABB battery charger temperature sensor",code:"10051443",location:"",system:"",util:"ABB"},
  {name:"CU-AG rod",code:"130008",location:"",system:"",util:""},
  {name:"Earth braid door battery modules Ground",code:"5144195A",location:"C-2-3",system:"",util:""},
  {name:"Ground Braid",code:"5144195A",location:"",system:"",util:""},
  {name:"4 seater gathering table",code:"5110153A",location:"",system:"",util:""},
  {name:"Speaker for Cars",code:"5125226A",location:"D-4-1",system:"",util:""},
  {name:"Motor air duct",code:"5121651",location:"",system:"",util:""},
  {name:"Assembly table of gathering",code:"5107444A",location:"",system:"",util:""},
  {name:"Continuous Isolation Monitor",code:"10040507",location:"M-3-3",system:"MOTOR",util:""},
  {name:"Thermal paste",code:"123135",location:"CH-R-1",system:"",util:""},
  {name:"Battery + D4",code:"10040503",location:"",system:"",util:""},
  {name:"Battery + G4",code:"10040504",location:"",system:"",util:""},
  {name:"Flange L 150mm",code:"10038037",location:"D-3-1",system:"",util:""},
  {name:"Circular 7 pin connector.",code:"10044084",location:"D-1-3",system:"",util:""},
  {name:"Useful alento pantograph",code:"5207186B",location:"",system:"PANTOGRAPH / HIGH TENS.",util:"Aleron"},
  {name:"ELECTRONICS OUTDOOR DOORS DCU",code:"10048442",location:"R-4-2",system:"DOORS",util:"PUERTAS EXTERIORES"},
  {name:"KETA torica board",code:"10061283",location:"",system:"",util:"Coches con freno"},
  {name:"KETA complete distributor valve for Cars",code:"10040730",location:"H-0",system:"Brake",util:"Coches con freno"},
  {name:"EP compact EPC-C (without parking brake)",code:"5125721A",location:"HM 1R",system:"Brake",util:"Freno Motriz"},
  {name:"EP compact EPC-CP (with parking brake)",code:"5125720A",location:"HM 1R",system:"Brake",util:"Freno Motriz"},
  {name:"External door silencer Extrior door Filter",code:"10072501 / 10019400",location:"D-4-2",system:"DOORS",util:"Puertas Exteriores"},
  {name:"2P 10A switch",code:"10025613",location:"",system:"",util:"Cafetería Q8"},
  {name:"Flashlight, Halogen lamp (White light)",code:"10024950",location:"D-4-2",system:"MOTOR",util:""},
  {name:"Regulator",code:"610619",location:"F-4-2",system:"",util:"Racor 10040489, tuerca 353954, manometro 610033"},
  {name:"WC door",code:"5111676A",location:"",system:"Interiors",util:"Puerta WC"},
  {name:"Water shower pressure regulator bide, wc and tap",code:"10037180",location:"D-3-3",system:"",util:"Panel agua WC (2 por panel)"},
  {name:"Manometer from 0 to 10 bar",code:"610033",location:"Q-3-3",system:"Water",util:"Reguladoras de presión"},
  {name:"Water purge electrovalve valve",code:"610249",location:"M-3-1",system:"Water",util:"Agua coches 1, 2 y 13."},
  {name:"Right rearview camera",code:"5130749A",location:"",system:"ELECTRIC",util:"Motriz"},
  {name:"Left rearview camera",code:"5130748A",location:"G-3",system:"ELECTRIC",util:"Motriz"},
  {name:"K21",code:"10002500",location:"Q-6-3",system:"",util:"Motriz"},
  {name:"Riom-s car",code:"5127408A",location:"H-1",system:"",util:""},
  {name:"WSP anti-lock",code:"5131889B",location:"W-2-1",system:"Brake",util:"COCHES SIN FRENO"},
  {name:"DNRA anti-lock",code:"5131889A",location:"H-0",system:"Brake",util:"COCHES CON FRENO"},
  {name:"HIGH PLATFORM",code:"5126538",location:"PLANO",system:"",util:"MOTRIZ"},
  {name:"PH CRSTAL PROTECTOR FILM",code:"10049980",location:"R-4-3",system:"",util:"CRISTAL MOTRIZ"},
  {name:"THERMAL MAGNETO Circuit Breaker 1.6 A 2.5 a",code:"10032600",location:"D-1-2",system:"",util:"MOTIRZ Q84,"},
  {name:"SENSOR INTERIOR DOORS S7",code:"5125901A",location:"M-3-3",system:"",util:""},
  {name:"COFFEE MAKER",code:"10055301",location:"",system:"",util:""},
  {name:"1A FUSE",code:"412430",location:"D-2-3",system:"MOTOR",util:"Manipulador de tracción"},
  {name:"GENERATOR GRILLE",code:"5129645A",location:"HM-1R",system:"GENSET",util:""},
  {name:"CLEAN SELECTOR",code:"10047336",location:"C-2-3",system:"MOTOR",util:""},
  {name:"Dopler radar connector",code:"5132593A",location:"F-2-2",system:"",util:"ERTMS Radar"},
  {name:"CONNECTORS AFTER RODAPIE LIGHTS EMERGENCY MALE",code:"10023324",location:"D-4-2",system:"ILLUMINATION",util:""},
  {name:"CONNECTORS AFTER RODAPIE LIGHTS EMERGENCY FEMALE",code:"10023325",location:"M-3-1",system:"ILLUMINATION",util:""},
  {name:"CONNECTORS AFTER RODAPIE LIGHTS EMERGENCY CLIP",code:"10023328",location:"D-4-2",system:"ILLUMINATION",util:""},
  {name:"ROLDANA DOOR WC",code:"10043997",location:"D-3-2",system:"DOORS",util:""},
  {name:"Wiper Motor",code:"5406179A",location:"F-2-2",system:"MOTOR",util:""},
  {name:"EXTERNAL DOOR COVER",code:"5134981A",location:"Q-8-3",system:"DOORS",util:""},
  {name:"LIMIT SWITCH (S8)",code:"10048452",location:"",system:"",util:""},
  {name:"POWER SUPPLY",code:"5125247A",location:"M-4-3",system:"ELECTRIC",util:"Power Supply for Video on Demand  VoD"},
  {name:"INTERIOR DOORS BUTTON",code:"5125861",location:"M-3-3",system:"DOORS",util:""},
  {name:"WATER LOAD TRAMPILLA",code:"5162397A",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"PANTOGRAPH Saliding Strip",code:"10060691",location:"HM-5R",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"T5 8KW fluorescent tube",code:"442029",location:"Q-5-3",system:"ILLUMINATION",util:""},
  {name:"G13 18KW fluorescent tube",code:"442032",location:"S-1-1",system:"ILLUMINATION",util:"Luz sala de maquinas"},
  {name:"Ceiling hall Light (ROOF LIGHT.VESTIB. TALGO AVRIL G3)",code:"5097681A",location:"S-1-1",system:"ILLUMINATION",util:"Cars"},
  {name:"ROOF LIGHT",code:"5132208A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"Signs",code:"5133770A",location:"R-3-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"5/2 VIAS PNEUMATIC VALVE For Damper  Under frame",code:"10046463",location:"Q-6-3",system:"INTERIOR / MECHANICAL",util:"Cars Dampers"},
  {name:"EXTRACTING UNIT",code:"5115927A",location:"HM-4R",system:"EAA",util:""},
  {name:"EXTRACTOR CYLINDER",code:"10046469",location:"Q-6-3",system:"EAA",util:""},
  {name:"HIGH MOTOR CABLE (CEILING)",code:"5132201A",location:"H-1-0",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"HIGH CABLE TRENCILLA",code:"5133431A",location:"Q-2-3",system:"PANTOGRAPH / HIGH TENS.",util:"CABLES DE ALTA TENSION"},
  {name:"BATTERY CHARGER",code:"5109997A",location:"HM-4R",system:"ELECTRIC",util:""},
  {name:"ELECTRICAL CABINET CHARGER BATTERIES",code:"5128404A",location:"G-3",system:"ELECTRIC",util:""},
  {name:"EVAPORATOR MOTOR for HVAC",code:"10046471/10094848",location:"G-1-0",system:"HVAC",util:""},
  {name:"HIGH PRESSURE FAN NOISE DAMPER",code:"10004341",location:"",system:"",util:""},
  {name:"EAA EXTRACTOR MOTOR Under frame HVAC",code:"10046504",location:"H-3",system:"EAA",util:""},
  {name:"VCU (CAR 1)",code:"5127405A",location:"N-1-3",system:"ELECTRIC",util:""},
  {name:"OUTDOOR DOOR OPENING BUTTON",code:"5123805A",location:"Q-3-2",system:"DOORS",util:""},
  {name:"BATTERY CHARGER MODULE",code:"10051406",location:"HM-9R",system:"ELECTRIC",util:""},
  {name:"Instantane relay 4 switched 12VDC",code:"10044214",location:"D-4-3",system:"MOTOR",util:"Generador (k63,"},
  {name:"Timed relay",code:"10044334",location:"",system:"MOTOR",util:"Generador (k235,"},
  {name:"ASFA 110 V. RELAY",code:"10038529",location:"M-3-3",system:"MOTOR",util:"Asfa"},
  {name:"LIGHT POSITION SELECTOR",code:"10039933",location:"W-2-1",system:"MOTOR",util:""},
  {name:"WC DOOR HANDLE",code:"10044051",location:"Q-1-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"Hvac",code:"5115926A",location:"",system:"",util:""},
  {name:"MOTOR CONDENSER MOTOR for Cabin",code:"10046515",location:"W-1-1",system:"",util:""},
  {name:"HVAC gauge",code:"10064385",location:"Q-4-4",system:"EAA",util:""},
  {name:"ROOM CONDENSER MOTOR",code:"10046477",location:"G-1-0",system:"EAA",util:""},
  {name:"RETENTION VALVE (WATER)",code:"10003966",location:"D-1-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"FILTER TYPE Y (WATER)",code:"610282",location:"Q-1-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"CDU",code:"5127404A",location:"F-2-1",system:"ELECTRIC",util:""},
  {name:"Regulating valve filters self-cleaning machine room",code:"10053611",location:"",system:"",util:"Vintilación sala de maquinas"},
  {name:"MOTOR ROOM MACHINE FAN",code:"5125886A",location:"HM-3",system:"MOTOR",util:""},
  {name:"CLAMPS 34-40",code:"10006267",location:"C-2-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"SOAP HOLDER",code:"10041220",location:"S-1-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"EAA DRAIN ELBOW  for HVAC",code:"10049619",location:"M-3-4",system:"INTERIOR / MECHANICAL",util:"Desague HVAC"},
  {name:"Screw guides bins",code:"10034543",location:"A-1-2",system:"Interiors",util:"Papeleras"},
  {name:"PMR PAPER GUIDE",code:"10051308",location:"",system:"INTERIOR / MECHANICAL",util:"Papeleras"},
  {name:"Standard PAPER GUIDE",code:"10049419",location:"D-1-2",system:"INTERIOR / MECHANICAL",util:"Papeleras"},
  {name:"EXTERNAL DOOR MECHANISM SH-815",code:"R 5130920A/ L 10067732",location:"HM-1R",system:"DOORS",util:""},
  {name:"DBZ pickup",code:"631274",location:"S-1-1",system:"ELECTRIC",util:""},
  {name:"BRAKE FLUID",code:"10046364",location:"HM",system:"INTERIOR / MECHANICAL",util:""},
  {name:"WC Coumplete Seta bowl",code:"5141038A",location:"HM-1R",system:"ELECTRIC",util:""},
  {name:"Cabin HVAC",code:"5116169A",location:"HM-3R",system:"EAA",util:""},
  {name:"GPS ANTENNA",code:"10041230",location:"D-3-3",system:"MOTOR",util:""},
  {name:"TYPE AND 1/2 \"FILTER",code:"610282",location:"Q-1-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"LOGO TALGO STICKER",code:"5151627A",location:"D-3-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"MOTOR PUPITRE VOLTIMETER",code:"10034145",location:"",system:"MOTOR",util:""},
  {name:"LIMINISCENT PROFILE",code:"10020400",location:"ROOM UP",system:"INTERIOR / MECHANICAL",util:""},
  {name:"PREF. ARMCHAIR KEYBOARD",code:"5125248A",location:"Q-5-3",system:"ELECTRIC",util:""},
  {name:"INDV MONITOR PREF. VOD Seat  monitor",code:"5125245A",location:"S-2-2",system:"ELECTRIC",util:""},
  {name:"SHOWER-BIDET",code:"5125880A",location:"A-2-3",system:"INTERIOR / MECHANICAL",util:""},
  {name:"PUPITRE TEMPERATURE PROBE ASSEMBLY",code:"5116170A",location:"Q-1-1",system:"ELECTRIC",util:""},
  {name:"AIR PANEL",code:"5125890A",location:"S-2-2",system:"",util:""},
  {name:"VALVE",code:"1007103A",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"LLAVIN IN L  Kye for Train Doors",code:"10014266",location:"D-4-2",system:"",util:""},
  {name:"PAU",code:"5125220A",location:"Q-3-4",system:"ELECTRIC",util:""},
  {name:"External door diode support",code:"10056987",location:"",system:"",util:""},
  {name:"External DOOR PRESSURE REGULATOR valve",code:"10048454",location:"M-4-4",system:"DOORS",util:""},
  {name:"ETHERNET GREEN CABLE M12-RJ45",code:"10044024",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"GREEN CABLE ADAPTER",code:"5179904A",location:"Q-1-3",system:"ELECTRIC",util:""},
  {name:"TRACTION-BRAKE MANIPULATOR",code:"5130468A",location:"W-2-3",system:"ELECTRIC",util:""},
  {name:"GORRINILLO OIL",code:"10064764",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"COMPRESSOR OIL",code:"10014695",location:"HM-10",system:"INTERIOR / MECHANICAL",util:""},
  {name:"SPEAKERS",code:"5125226A",location:"D-4-1",system:"ELECTRIC",util:""},
  {name:"CONVERTER CAR LIGHTS MY5-L-110 / 35W",code:"5043034A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"Circuit Breaker MCB 25KV",code:"5129151A",location:"HM-4",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"Main air compressor electrical panel",code:"5125717A",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"Terminal panel electric air compressor",code:"10075845",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"Instant relay 4 switched 110VDC",code:"10013410",location:"F-4-1",system:"ELECTRIC",util:"Aire(K149)"},
  {name:"Three-pole contactor 48-130 VDC / VAC",code:"10032535",location:"M-2-4",system:"ELECTRIC",util:"Aire(K18)"},
  {name:"Auxiliary contacts block",code:"10032625",location:"D-3-2",system:"ELECTRIC",util:"Aire"},
  {name:"Contact 2 Q1 x air compressor",code:"10075838",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"Contact 1 Q1 x air compressor",code:"10075837",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"Hvac circuit breaker (evaporator, condenser)",code:"10032600",location:"D-1-2",system:"HVAC",util:"Evaporadores, condensadores"},
  {name:"Q1 air compressor circuit breaker",code:"10075836",location:"",system:"ELECTRIC",util:"Aire"},
  {name:"K1 Contactor Auxiliary Compressor",code:"10039456",location:"D-4-3",system:"ELECTRIC",util:"Gorrinillo"},
  {name:"K1 AIR COMPRESSOR CONTACTOR",code:"10040254",location:"M-4-2",system:"ELECTRIC",util:"Aire"},
  {name:"AIR CONNECTOR 2 POLE MEGAFONIA",code:"10041411",location:"D-1-3",system:"GENSET",util:""},
  {name:"CONNECTOR PIN",code:"10041039",location:"D-4-2",system:"ELECTRIC",util:""},
  {name:"MULTIPLE LLAVIN",code:"10013686",location:"D-4-2",system:"TOOLS",util:""},
  {name:"COMPLETE PANTOGRAPH",code:"5126061A",location:"HM-8R",system:"PANTOGRAPH / HIGH TENS.",util:"Electrical"},
  {name:"MICRO DOOR WC",code:"10033111",location:"M-3-3",system:"",util:""},
  {name:"Fuse 2A",code:"10051521",location:"D-1-2",system:"GENSET",util:"Generador"},
  {name:"5A fuse",code:"10051522",location:"D-2-3",system:"GENSET",util:"Generador"},
  {name:"10A fuse",code:"10051523",location:"D-1-3",system:"GENSET",util:"Generador"},
  {name:"F1 FUSE",code:"10051419",location:"M-3-2",system:"ELECTRIC",util:""},
  {name:"PNEUMATIC VALVE",code:"10046463",location:"Q-6-3",system:"INTERIOR / MECHANICAL",util:""},
  {name:"Driver's seat button",code:"10011386",location:"",system:"Interiors",util:"Motriz"},
  {name:"VALVE COIL",code:"10046456",location:"Q-6-3",system:"ELECTRIC",util:""},
  {name:"RED FLAG WITH HANDLE",code:"10010077",location:"",system:"",util:"BANDERAS DE MOTRIZ"},
  {name:"RELAY BASE",code:"10003461",location:"B-3-3",system:"ELECTRIC",util:""},
  {name:"CEILING LIGHT LED CARS",code:"5132262A",location:"Q-8-2",system:"ILLUMINATION",util:"Roof Ceiling LED lights"},
  {name:"50mm cable terminal of the generator battery",code:"10014840",location:"Q-1-1",system:"GENSET",util:""},
  {name:"GENSET BATTERY",code:"10060918 / 10099516 New",location:"H-0",system:"",util:""},
  {name:"CONVERTER 110 / 35W HALL LIGHTS",code:"5043034A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"K174 WIPER RELAY",code:"10015045",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"WATER PANEL",code:"5128605A",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"21 \"MONITOR",code:"5121191A",location:"N3",system:"ELECTRIC",util:"Monitors Group"},
  {name:"Cellar evaporator fan",code:"10073349",location:"R-3-2",system:"Refrigerators",util:""},
  {name:"Cellar evaporator fan",code:"10066359",location:"MH-6R",system:"Refrigerators",util:""},
  {name:"Cabien Fridge",code:"5133843A",location:"N-1-2",system:"",util:""},
  {name:"LEVEL SENSORS",code:"10027594",location:"D-3-2",system:"ELECTRIC",util:"Water System tank level Cars# 01,02 & 13"},
  {name:"MALE PIN XM01 / 743 MORRO",code:"10044289",location:"D-1-1",system:"MOTOR",util:""},
  {name:"Egru hard drive",code:"10064019",location:"",system:"ELECTRIC",util:""},
  {name:"EGRU",code:"5124861A",location:"W-3-2",system:"ELECTRIC",util:""},
  {name:"EXTINGUISHER",code:"10048634",location:"HM-4",system:"INTERIOR / MECHANICAL",util:""},
  {name:"ADD PANTOGRAPH VALVE",code:"10061507",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"MENBRANA PANTOGRAPH",code:"10071835",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"CONNECTION FITTINGS AND VALVE ADD PANTOGRA",code:"10061590",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"FERODOS \"VARIANTE A\"",code:"514591A",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"OUTDOOR CUSHION TEMPERATURE SENSOR CARS",code:"5138878A",location:"H-1-4",system:"ELECTRIC",util:""},
  {name:"SENSOR TEMPERATURE INDOOR CUSHIONS CARS",code:"5138878B",location:"H-1-4",system:"ELECTRIC",util:""},
  {name:"Microwave",code:"10066387",location:"G-0-2",system:"ELECTRIC",util:"Se cambia la ficha por la 10066387"},
  {name:"Pressure regulator ASSEMBLY Valve for HVAC Damper SV 3.2",code:"10090892",location:"",system:"",util:"CARS"},
  {name:"OVEN",code:"New 5142530A /10049405",location:"",system:"ELECTRIC",util:""},
  {name:"DIFFERENTIAL",code:"10047059",location:"",system:"ELECTRIC",util:""},
  {name:"AUTOMATIC DIF.",code:"10047056",location:"R-1-1",system:"ELECTRIC",util:""},
  {name:"PRESSURE WAVE SENSOR",code:"10006947 /5441965A",location:"Q-3-2",system:"ELECTRIC",util:""},
  {name:"WC BUTTON",code:"10033105",location:"Q-1-3",system:"ELECTRIC",util:""},
  {name:"BATTERY CHARGER FAN",code:"10051430",location:"S-1-2",system:"ELECTRIC",util:""},
  {name:"DOOR OPENING SWITCH PMR WC",code:"5130899A",location:"F-2-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"LED COVER HOLDER",code:"5134538C",location:"",system:"INTERIOR / MECHANICAL",util:""},
  {name:"EAA CAR CONTROL MODULE",code:"5115928A",location:"W-3-4",system:"EAA",util:""},
  {name:"EAA CAR CARD for HVAC Cars",code:"10046527",location:"F-1-2",system:"EAA",util:"Controal Card for HVAC"},
  {name:"AUTOMATIC F13 CAFETERIA",code:"10047053",location:"",system:"ELECTRIC",util:""},
  {name:"DIFFERENTIAL F13 CAFETERIA",code:"10047058",location:"D-1-1",system:"ELECTRIC",util:""},
  {name:"AUXILIARY CONTACT F13 CAFETERIA",code:"10032843",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"PIN FEMALE PRESSURE PRESSURE",code:"10032406",location:"",system:"ELECTRIC",util:""},
  {name:"LOCTITE FIXING THREADS",code:"126480",location:"TLROOM",system:"TOOLS",util:""},
  {name:"3/8 REGULATOR REDUCER",code:"610619",location:"F-4-1",system:"INTERIOR / MECHANICAL",util:""},
  {name:"Hand lamp FLASHLIGHT compete",code:"10034877",location:"Q-4-4",system:"",util:"REAR AND FRONT SIGNALING FLASHLIGHT"},
  {name:"Hand lamp Charger LANTERN CHARGER",code:"10010984",location:"",system:"",util:""},
  {name:"B04-B02 CAFETERIA RELAY",code:"10015045",location:"D-1-2",system:"ELECTRIC",util:""},
  {name:"LUMINAIRE T15 1X8W for machine room light",code:"5151893A",location:"",system:"",util:""},
  {name:"EAA SCREW",code:"5130904A",location:"HM-2",system:"EAA",util:"10044113,Tornillo largo"},
  {name:"EAA SAFETY WASHER",code:"10045002",location:"A-3-3",system:"EAA",util:""},
  {name:"NUT SECURITY",code:"10006033",location:"A-2-2",system:"EAA",util:""},
  {name:"MACHINE ROOM FILTER",code:"5134962A",location:"",system:"MOTOR",util:""},
  {name:"CABLE SIDE TAKE",code:"5157492B",location:"",system:"ELECTRIC",util:""},
  {name:"EXTERIOR IINFORMATION DISPLAY PANEL",code:"5148672A",location:"W-1-3",system:"ELECTRIC",util:""},
  {name:"BCU1",code:"5131882A",location:"W-1-3",system:"ELECTRIC",util:""},
  {name:"BCU2",code:"5131882B",location:"F-2-3",system:"ELECTRIC",util:""},
  {name:"Exterior door motor",code:"10048432",location:"Q-7-2",system:"",util:""},
  {name:"Motor + inner door support",code:"10051627",location:"Q-4-3",system:"",util:""},
  {name:"Interior Door Motor",code:"10065043",location:"",system:"DOORS",util:"Sin stok"},
  {name:"INTERNAL DOOR MOTOR",code:"10051627",location:"Q-4-3",system:"DOORS",util:""},
  {name:"SH750 DOOR MECHANISM ASSEMBLY. INT",code:"5130922A",location:"H-1",system:"DOORS",util:""},
  {name:"TRENCILLA WITH Z41 TERMINAL",code:"5037557C",location:"R-1-2",system:"ELECTRIC",util:""},
  {name:"TRENCILLA TIERRA PUERTA MOTRIZ",code:"10065933",location:"",system:"DOORS",util:""},
  {name:"GENERATOR EXTINGUISHER",code:"5133188A",location:"M-4-2",system:"INTERIOR / MECHANICAL",util:""},
  {name:"Capacitor CONVERTER CONDENSER (New)",code:"10063713",location:"",system:"ELECTRIC",util:"Convertidor Motriz"},
  {name:"CONDENSER CONVERTER (Old)",code:"10040558",location:"H-0",system:"ELECTRIC",util:"Convertidor Motriz"},
  {name:"NEW CAPACITOR NUT",code:"10064900",location:"",system:"",util:""},
  {name:"Round terminal new capacitor",code:"10066121",location:"",system:"ELECTRIC",util:"Capacitor"},
  {name:"New heat shrink condenser",code:"10009837",location:"",system:"ELECTRIC",util:"Capacitor"},
  {name:"Cable with new capacitor terminal",code:"5137136A",location:"",system:"ELECTRIC",util:"Capacitor"},
  {name:"New condenser nut",code:"10064900",location:"B-2-1",system:"ELECTRIC",util:"Capacitor"},
  {name:"New condenser spring washer",code:"10081289",location:"",system:"ELECTRIC",util:"Capacitor"},
  {name:"New condenser washer capaciter",code:"10064901",location:"A-3-2",system:"ELECTRIC",util:"Capacitor"},
  {name:"Termianal capacitor cable new",code:"10007087",location:"",system:"ELECTRIC",util:"120-150mm2"},
  {name:"CONDENSER CABLE TERMINAL Old",code:"10027114",location:"D-2-3",system:"ELECTRIC",util:""},
  {name:"Remote control set pin (ratchet bogis)",code:"10064474",location:"D-4-3",system:"Brake",util:"Bogis"},
  {name:"Remote control set (bug ratchet)",code:"10063653",location:"Q-4-2",system:"Brake",util:"Bogis"},
  {name:"OUTDOOR TEMPERATURE PROBE ASSEMBLY",code:"10046460",location:"Q-1-1",system:"ELECTRIC",util:""},
  {name:"PROBE",code:"5116170A",location:"Q-1-1",system:"ELECTRIC",util:""},
  {name:"MIO BT Green 8P LOCKING CONNECTOR",code:"10044330",location:"",system:"",util:""},
  {name:"WC TAP for Sink",code:"10033109",location:"F-3-2",system:"ELECTRIC",util:""},
  {name:"WC BINOPTIC SENSOR for Sink",code:"10062350",location:"",system:"WC",util:""},
  {name:"SOLENOID VALVE 5/2 for Sink",code:"10062349",location:"",system:"WC",util:""},
  {name:"GSMR FUSE",code:"10041576",location:"",system:"ELECTRIC",util:""},
  {name:"VOLTIMETER C.C. 150 V.",code:"435059",location:"Q-3-3",system:"MOTOR",util:""},
  {name:"LED STRIPE BOX",code:"5134532A",location:"F-2-1",system:"ILLUMINATION",util:""},
  {name:"PANTOGRAPH Neumatic PANEL",code:"5131338A",location:"H-1",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"PANTOGRAPH INSULATOR/SCREW/WASHER NOMEL",code:"10061509/10061298 /10061303",location:"",system:"PANTOGRAPH / HIGH TENS.",util:""},
  {name:"CARTRIDGE FILTERS ROOM MACHINES",code:"5156520A",location:"",system:"MOTOR",util:""},
  {name:"CONDENSING UNIT",code:"10049407",location:"HM-1R",system:"EAA",util:""},
  {name:"EAA COMPLETE",code:"5115926",location:"",system:"EAA",util:""},
  {name:"EAA PACKING UNIT",code:"5115739A",location:"",system:"EAA",util:""},
  {name:"EAA ELECTRONICS Cabin HVAC",code:"5141116A",location:"S-2-2",system:"EAA",util:""},
  {name:"DVR recorder / DVR Hard Disk",code:"5126589A / 10045951",location:"S-1-1",system:"Security cameras",util:""},
  {name:"MOBILE VOICE RECORDER",code:"5146913A",location:"S-2-2",system:"ELECTRIC",util:""},
  {name:"CIP",code:"5125221A",location:"Q-4-4",system:"ELECTRIC",util:""},
  {name:"LUBRI-LIMP",code:"121518",location:"",system:"TOOLS",util:""},
  {name:"K78 TRACTION COOLING MOTORS",code:"10039456",location:"",system:"ELECTRIC",util:""},
  {name:"K12 MACHINE ROOM REFRIGERATION CONTACTOR",code:"10041577",location:"D-1-2",system:"ELECTRIC",util:"K12"},
  {name:"EV + TAP COIL",code:"10062349",location:"",system:"ELECTRIC",util:""},
  {name:"READING LIGHT + PULSED + TRAFFIC",code:"10044270",location:"M-3-1",system:"ILLUMINATION",util:""},
  {name:"EAA MOTOR MODE SELECTOR",code:"10043962",location:"R-4-3",system:"MOTOR",util:""},
  {name:"EAA MOTOR EMERGENCY SELECTOR",code:"10043963",location:"R-4-3",system:"MOTOR",util:""},
  {name:"SIDE TAKE",code:"10041141",location:"D-4-3",system:"MOTOR",util:""},
  {name:"LED LIGHT BOX 13W / 1757mm",code:"10058938",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"LED LIGHT BOX 10W / 1347mm",code:"10058939",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"LED LIGHT BOX 6W / 835mm",code:"10058940",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"\"    \"",code:"10058939",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"\"     \"",code:"10058940",location:"S-1-1",system:"ILLUMINATION",util:""},
  {name:"ELECTRONICS TAP for WC Sink",code:"10062348",location:"C-2-3",system:"",util:"WC control card"},
  {name:"MULTILED DIODE",code:"464258",location:"A-2-3",system:"ELECTRIC",util:""},
  {name:"WC ELECTRONICS",code:"5125879A",location:"W-1-2",system:"ELECTRIC",util:""},
  {name:"K5 CONTACTOR",code:"10038831",location:"M-3-3",system:"ELECTRIC",util:""},
  {name:"PINNHEMBRA (INTERCOM. EMERGENCY",code:"10041042",location:"D-4-2",system:"ELECTRIC",util:""},
  {name:"Plastic Light emergency corridor tourist",code:"10068552/7",location:"",system:"ILLUMINATION",util:"Luces de emergencias pasillo turistas"},
  {name:"EMERGENCY LIGHT for seat Econemy",code:"5140306B/A",location:"C-2-3/S-1-1",system:"ILLUMINATION",util:""},
  {name:"Ultaviolet light UV Lamp",code:"100066366/10011215",location:"",system:"",util:"Agua, cafetería."},
  {name:"EMERGENCY LIGHT BUSSINESS",code:"10045465",location:"M-3-4",system:"ILLUMINATION",util:""},
  {name:"WC Hand Dry",code:"10041088",location:"D",system:"ELECTRIC",util:"WC"},
  {name:"250 / F4 FUSE",code:"412418",location:"D-4-2",system:"ELECTRIC",util:""},
  {name:"EMERGENCY PUSH BUTTON  WC Inside",code:"10032837",location:"Q-4-2",system:"ELECTRIC",util:""},
  {name:"OIL CONTROL UNIT",code:"10060613",location:"",system:"",util:""},
  {name:"OIL CONTROL BLOCK",code:"10081277",location:"",system:"",util:""},
  {name:"THERMOSTAT",code:"10081278",location:"",system:"",util:""},
  {name:"MAIN ELEMENT",code:"10081317",location:"",system:"",util:""},
  {name:"PISTON CONTROL",code:"10081318",location:"",system:"",util:""},
  {name:"COMPRESSION SPRING",code:"10081319",location:"",system:"",util:""},
  {name:"O-RING",code:"10081279",location:"",system:"",util:""},
  {name:"STOPPER",code:"10081280",location:"",system:"",util:""},
  {name:"RETAINING RING 38X1.5",code:"10081311",location:"",system:"",util:""},
  {name:"DOUBLE NIPPLE",code:"10081312",location:"",system:"",util:""},
  {name:"OIL FILT.CARTRIDGE",code:"10048647",location:"",system:"",util:""},
  {name:"O-RING",code:"10081313",location:"",system:"",util:""},
  {name:"STUD M8X30",code:"10081314",location:"",system:"",util:""},
  {name:"LOCKING RING A08",code:"10014814",location:"",system:"",util:""},
  {name:"HEXAGON NUT M8",code:"10068347",location:"",system:"",util:""},
  {name:"SEALING RING A17X21",code:"10081315",location:"",system:"",util:""},
  {name:"SCREW PLUG G3 / 8A",code:"10081316",location:"",system:"",util:""},
  {name:"FRONT Sunshade for PH",code:"5238917A",location:"Sun shade",system:"Interiors",util:"Estor PHs sun shade"},
  {name:"ELECTRIC MOTOR for sunshade for PH",code:"10082309",location:"Sun shade",system:"",util:"Estor PHs"},
  {name:"LOWER BAR SPRING",code:"10082303",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"TOP GUIDE COVER",code:"10082304",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"LOWER GUIDE COVER",code:"10082305",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"LOWER BAR COVER",code:"10082306",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"ROPE",code:"10082307",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"PULLEY",code:"10082308",location:"",system:"Interiors",util:"Estor PHs"},
  {name:"Welding rod",code:"135008",location:"",system:"",util:""},
  {name:"Borax for welding rods",code:"135401",location:"",system:"",util:""},
  {name:"Swivel Pivot, Bolt",code:"5216893A",location:"",system:"",util:"Enganche morro."},
  {name:"Coolant converters",code:"10048496",location:"",system:"",util:"Convertidores"},
  {name:"Talgo rodal logo",code:"5006877A",location:"",system:"",util:""},
  {name:"External door extrior Push Botton",code:"5123805A",location:"",system:"",util:""},
  {name:"Antena GSMR-GPS ERTMS/ETCS",code:"10031619",location:"",system:"",util:""},
  {name:"Grease exterior doors (grey)",code:"10041591",location:"",system:"",util:""},
  {name:"Grease exterior doors (white)",code:"10063686",location:"",system:"",util:""},
  {name:"Train batteries water",code:"133401",location:"",system:"",util:""},
  {name:"Generator Coolant",code:"10060392",location:"",system:"",util:""},
  {name:"Cabin Fridgfe",code:"5133843A",location:"",system:"",util:"PHs"},
  {name:"COMPRESSOR WASHER after Oil fill",code:"10033358",location:"",system:"",util:"washer"},
  {name:"INTERNAL DOOR DCU",code:"10050797",location:"",system:"",util:""},
  {name:"CCTV Camera",code:"5126591A",location:"",system:"",util:""},
  {name:"Push Botton for Intrior Door",code:"5154087A/5125861A",location:"",system:"",util:""},
  {name:"Fire  Extinguisher",code:"10048634",location:"",system:"",util:""},
  {name:"Keta DISTRIBUTOR VALVE COMPLETE POWER HEAD",code:"10039566",location:"",system:"",util:""},
  {name:"Boiler Heater",code:"10066388 / 5171888A",location:"",system:"",util:""},
  {name:"PRESSURE REGULATOR ASSEMBLY Valve for Ex. Door",code:"5178589A",location:"",system:"",util:""},
  {name:"Eur Obalise Antenna",code:"5132237A",location:"",system:"",util:"ERTMS EUROBALIZED ANTENNA"},
  {name:"SCHUKO TYPE SOCKET  ( Enchufe) XS2",code:"10037956",location:"",system:"",util:"Electriacl"},
  {name:"ZENER DIODE (D11)",code:"10048782",location:"",system:"",util:""},
  {name:"SPORT OF ZENER DIODE (D11)",code:"10056987",location:"",system:"",util:""},
  {name:"SOCKET FOR CFETERIA XS4",code:"10078196",location:"",system:"",util:""},
  {name:"PRESSURE TRANSDUCER",code:"10039575",location:"",system:"",util:""},
  {name:"Battery Charger Pump",code:"10051431",location:"",system:"",util:"Battery Charger"},
  {name:"Vagner Fire",code:"5125832A",location:"",system:"",util:"Fire Protection Machine"},
  {name:"ABB battery charger Contector KM1",code:"10051417",location:"",system:"",util:"ABB Battery Charger"},
  {name:"Transducer pressure of Cabin High Pressure",code:"10046481",location:"",system:"",util:""},
  {name:"Transducer pressure of Cabin High Pressure",code:"10046482",location:"",system:"",util:""},
  {name:"Cold Stroage Fan",code:"10073349",location:"",system:"",util:""},
  {name:"DRIVING CABIN HVAC CONTROL PANEL",code:"5141116A",location:"",system:"",util:"Cabin"},
  {name:"Main comp. Oil",code:"10014645",location:"",system:"",util:"Main Compressor"},
  {name:"Battery Charger (Gen-set Battery)",code:"10064505",location:"",system:"",util:"Machine room Genset"},
  {name:"Roof Switch for roof line (25KV)",code:"5129152A",location:"",system:"",util:"High Voltage"},
  {name:"Pressure Transducer for Brake panel Hydrolic",code:"10039363",location:"",system:"",util:"Brake system Hydralic system"},
  {name:"Non return valve for water system",code:"10003966",location:"Q-9-3",system:"",util:"Water System"},
  {name:"TEMPERATURE SENSOR for Boji",code:"10055578",location:"",system:"",util:"Temp Sensor"},
  {name:"Fuse Hloder  1A/2A",code:"10008994",location:"",system:"",util:"For card Z05 and Z06"},
  {name:"RELAY TERMINAL GREEN Relay",code:"10018362",location:"",system:"",util:""},
  {name:"Clamp for cable of antenna euro blace",code:"10051949",location:"",system:"",util:""},
  {name:"O-RING for Batteries filling line",code:"10075795",location:"",system:"",util:"Batteries for train"},
  {name:"4P LOCKING MALE CONNECTOR D",code:"10044116",location:"",system:"",util:"Sunshade male connector"},
  {name:"CONNECTOR FEMALE 4P",code:"10024422",location:"",system:"",util:"Sunshade female connector"},
  {name:"SOLENOID VALVE-COIL 24V",code:"10046479",location:"",system:"",util:"HVAC"},
  {name:"HMI MONITOR Car 5",code:"10049409",location:"",system:"",util:"Cafeteria"},
  {name:"CLEANER LUBRICANT for Electrical CRC",code:"121518",location:"",system:"",util:"Cleaner for Elect. Contect"},
  {name:"FEMALE TERMINAL complete isolate",code:"403665",location:"",system:"",util:"Legs"},
  {name:"EXTERIOR INFERIOR IZQDO/DRCHO Light",code:"5134954A",location:"",system:"",util:"EXTERIOR INFERIOR IZQDO/DRCHO"},
  {name:"Hand lamp only Lamp",code:"10024950",location:"",system:"",util:"Hand lamp only Lamp"},
  {name:"TEMPERATURE SENSOR for Bogie GearBox",code:"10055582",location:"",system:"",util:"Gearbox"},
  {name:"DIRECT ROOF LIGHTING (RIGHT)/DIRECT ROOF LIGHTING (LEIFT)",code:"5136223A/5132208A",location:"",system:"",util:"DIRECT ROOF LIGHTING (RIGHT)/DIRECT ROOF LIGHTING (LEIFT)"},
  {name:"COMPLETE BOTTLE FOR BRAKE FLUID Liquid",code:"100",location:"",system:"",util:""},
  {name:"LIGHT EXTERIOR INFERIOR IZQDO/DRCHO",code:"5134954A",location:"",system:"",util:""},
  {name:"Power Head External Door Ruber / SEALING PROFILE",code:"10025861 / 5148538A",location:"",system:"",util:""},
  {name:"Q1 for cafetaria panel",code:"10072582",location:"",system:"",util:""},
  {name:"INSULATOR for Pantograph",code:"10061509",location:"",system:"",util:""},
  {name:"Light for Cabin PHs roof cabin",code:"5042923A",location:"",system:"",util:""},
  {name:"TOILET SEAT with cover  for WC",code:"10043437",location:"",system:"",util:""},
  {name:"TEMPERATURE SENSOR STATOR",code:"10055585",location:"",system:"",util:""},
  {name:"Blue tube for WC",code:"10029470",location:"",system:"",util:""},
  {name:"CONVERTER  for reading light 110V  100W",code:"10023049",location:"",system:"",util:"12V 100W 110VDC 20KHZ"},
  {name:"Hand rest of Driver Seat Left\\Right",code:"10011380\\10011379",location:"",system:"",util:""},
  {name:"Aux Convertor Power Module",code:"5114676A",location:"",system:"",util:"BT / Alstom"},
  {name:"EVC ERTMS/ETCS",code:"5131642A",location:"",system:"",util:""},
  {name:"Rubber for water filling end",code:"505503",location:"",system:"",util:""},
  {name:"Water Pump Wiper",code:"3100194/ 10089672 New",location:"",system:"",util:""},
  {name:"Fire Detector for Cars",code:"5125831A",location:"",system:"",util:"SMOKE DETECTOR"},
  {name:"IMPULSES SENSOR  Speed Sensor",code:"5145781A",location:"",system:"",util:"External door"},
  {name:"OUTDOOR EMERGENCY DEVICE Open button from out Ex. Door",code:"5123800",location:"",system:"",util:""},
  {name:"CYCLONIC EXTRACTORS Fan",code:"5125885A",location:"",system:"",util:""},
  {name:"COTTER PIN for Brake Pad",code:"265269",location:"",system:"",util:""},
  {name:"Ball for Aux Brake handle",code:"10034729",location:"",system:"",util:"Brake System"},
  {name:"Aux Brake Handle with valve",code:"10004147",location:"",system:"",util:"Brake System"},
  {name:"Main Transformer Oil",code:"10050329",location:"",system:"",util:""},
  {name:"CCTV HMI",code:"5126592A",location:"",system:"",util:""},
  {name:"External Light front Head Top",code:"5134955A",location:"",system:"",util:"Light"},
  {name:"floricent light for Machine room, T8 1X18W LUMINAIRE",code:"5088230A",location:"",system:"",util:""},
  {name:"Sikaflex Black",code:"126425",location:"",system:"",util:""},
  {name:"Battery new for trains",code:"10040502",location:"",system:"",util:""},
  {name:"PMR DCU Door control Unit",code:"10050797",location:"",system:"",util:""},
  {name:"Micro switch for fridge door white",code:"10036629",location:"",system:"",util:"MICRO SWITCH"},
  {name:"Talgo type",code:"126821",location:"",system:"",util:""},
  {name:"Out door Transformer 25KV TENSION TRANSDUCER PT",code:"5130603A",location:"",system:"",util:""},
  {name:"Rodal CABLE 35 MM2 WITH TERMINALS Rudal Groud cable",code:"5161179A",location:"",system:"",util:""},
  {name:"O Ring for ECO meter / O Ring Lubrication",code:"10075743 / 10026521",location:"",system:"",util:""},
  {name:"Electric Contact Grease",code:"10016943",location:"",system:"",util:""},
  {name:"K2 relay for compressor control",code:"10075840",location:"",system:"",util:""},
  {name:"RED FLAG WITH HANDLE",code:"10010077",location:"",system:"",util:""},
  {name:"EKE S-Riom Card HSA2620A",code:"10049118",location:"",system:"",util:""},
  {name:"CONTENT SERVER CS",code:"5121193A",location:"",system:"",util:""},
  {name:"K1 AUXILIARY CONTACT BLOCK for main Copmressor",code:"10056496",location:"",system:"",util:"Main Compressor"},
  {name:"SPACER FOR PROTECTION PLATE for FUSE cover ( Long )",code:"5105459A",location:"",system:"",util:"Main Fuses"},
  {name:"SPACER FOR PROTECTION PLATE for FUSE cover ( Short )",code:"5172631A",location:"",system:"",util:"Main Fuses"},
  {name:"Leg Terminal for Capcitor filter 120-150 MM2;BORNAJE M16",code:"10007087",location:"",system:"",util:""},
  {name:"Cover for WC door lock angel white plastic",code:"5164797A",location:"",system:"",util:""},
  {name:"FOOTREST SUPPORT for Eco",code:"10083863",location:"",system:"",util:""},
  {name:"FRESH AIR SERVOMOTOR for HVAC Damper",code:"10046459",location:"",system:"",util:""},
  {name:"AUTOMATIC COFFEE MAKER",code:"10057186",location:"",system:"",util:""},
  {name:"Gaskit for ECO meter control connector",code:"10104208",location:"",system:"",util:""},
  {name:"Y4 Sloinde valve for External Door Black",code:"10048436",location:"",system:"",util:""},
  {name:"Table Hinge Eco",code:"5339314A New",location:"",system:"",util:""},
  {name:"Table Hinge Business",code:"5407500A New/ 5032855A Old",location:"",system:"",util:""},
  {name:"Emergency Brake Valve ( Push botton Brake)",code:"5121989A",location:"",system:"",util:""},
  {name:"Fuse 1250",code:"10050080",location:"",system:"",util:""},
  {name:"EUROBALISE AERIAL CONNECTOR",code:"5132595A",location:"",system:"",util:""},
  {name:"WINDSHIELD WASHER SELECTOR for wiper",code:"10047336",location:"",system:"",util:""},
  {name:"EUROBALISE AERIAL Groung Or Earthing",code:"5146567A",location:"",system:"",util:""},
  {name:"Micromesh Filter for Comp",code:"10083274",location:"",system:"",util:""},
  {name:"DMI",code:"5131753A",location:"",system:"",util:""},
  {name:"RADIATOR of ABB Battery Charger",code:"10051432",location:"",system:"",util:""},
  {name:"Cupler fron protection Cover",code:"5143039A",location:"",system:"",util:""},
  {name:"BUS Cupler REPEATER",code:"5127409A",location:"",system:"",util:""},
  {name:"Non return valve for water system White",code:"10027449",location:"",system:"",util:""},
  {name:"PMR TOILET LOCK SLAVE ASSEMBLY",code:"5150025A",location:"",system:"",util:""},
  {name:"ETCS cabint Door Lick",code:"10106658",location:"",system:"",util:""},
  {name:"Conector femail 6P for Eco Meter",code:"10037844",location:"",system:"",util:""},
  {name:"Fitting of Regulator valve (MALE FITTING)",code:"10044071",location:"",system:"",util:""},
  {name:"Pilo for PMR 2 Seats Fro specical",code:"5614060A",location:"",system:"",util:""},
  {name:"Air dryer for Compressor",code:"Z5125715A",location:"",system:"",util:""},
  {name:"IMPULSE SOLENOID VALVE air dryer",code:"10056396",location:"",system:"",util:""},
  {name:"ACELEROMETRO ERTMS/ETCS White Screw",code:"10076122",location:"",system:"",util:""},
  {name:"Talgo Logo on Rudal + Adhesive",code:"5006877A + 126433",location:"",system:"",util:""},
  {name:"Red valve for condensate tank Comp",code:"10030060",location:"",system:"",util:""},
  {name:"Orange Relay 480VAC VOLTAGE CONTROL RELAY",code:"10043713",location:"",system:"",util:""},
  {name:"Battery charger cooling fan KM12 KM13 Contactor",code:"10051429",location:"",system:"",util:""},
  {name:"RADIATOR MAIN CAP for Genset",code:"10083724",location:"",system:"",util:""},
  {name:"Cable ties  for heat area",code:"10038037",location:"",system:"",util:""},
  {name:"Insulation Tester  ( Magger)",code:"10015462",location:"",system:"",util:""},
  {name:"MVB Board for Battery charger",code:"10051414",location:"",system:"",util:""},
  {name:"Micro switch for Battery charger Fuse F1",code:"10052869",location:"",system:"",util:""},
  {name:"COMPLETE GRILL Small",code:"5141600A",location:"",system:"",util:""},
  {name:"COMPLETE GRILL large big",code:"5110094A",location:"",system:"",util:""},
  {name:"Check Valve for Brake side panel",code:"10029740",location:"",system:"",util:""},
  {name:"PANEL SP01A for SV Distribution Valve",code:"5125890A",location:"",system:"",util:""},
  {name:"WIFI ACCESS POINT Indra Wifi",code:"5131397A / 5131398A (Car 04)",location:"",system:"",util:""},
  {name:"THREE POLES CONTACTOR K1 in brake banel (230V) with Aux",code:"10041499 / 10048514 (Aux)",location:"",system:"",util:""},
  {name:"O-Ring for Keta Cars",code:"10061283",location:"",system:"",util:""},
  {name:"Under sink door",code:"5110176A",location:"",system:"",util:""},
  {name:"PNEUMATIC CILINDER under frame for HVAC",code:"10046469",location:"",system:"",util:""},
  {name:"FRESH AIR MOTOR-FAN ASSEMBLY for HVAC",code:"10054650",location:"",system:"",util:""},
  {name:"Spring for Brake Pad",code:"10075660",location:"",system:"",util:""},
  {name:"Gateway for Indra Wifi",code:"5131399A",location:"",system:"",util:""},
  {name:"Speed sensor Rudals IMPULSES SENSOR",code:"5143528A",location:"",system:"",util:""},
  {name:"Conector femail and male for under sink WC",code:"10034641/10034090",location:"",system:"",util:""},
  {name:"Pins Conector femail and male for under sink WC",code:"10013754/10032974",location:"",system:"",util:""},
  {name:"Z68 cabinet door HINGES FOR VX",code:"10105246",location:"",system:"",util:""},
  {name:"TORSION SPRING for cuppler front for locking door",code:"10083699",location:"",system:"",util:""},
  {name:"Coffee machine filter support or extention / Filter of coffee",code:"10065350/10065349",location:"",system:"",util:""},
  {name:"HOURS COUNTER for compressor",code:"10071226",location:"",system:"",util:""},
  {name:"180º OPENING HINGE for Cold stroage and celler",code:"5078889A",location:"",system:"",util:""},
  {name:"Arm Rest for Busuiness",code:"5140011A",location:"",system:"",util:""},
  {name:"Driver chair bottom air conector",code:"10050838",location:"",system:"",util:""},
  {name:"valve D03.01.23",code:"10050512",location:"",system:"",util:""},
  {name:"External Door Limit switch S10",code:"10048451",location:"",system:"",util:""},
  {name:"Transformer oil sample bottle",code:"10083597",location:"",system:"",util:""},
  {name:"ETCS Slector on Z68",code:"10054893",location:"",system:"",util:""},
  {name:"Air tube for pantograph  (Loctite 577)",code:"10095480 / 126399 (Loctite 577)",location:"",system:"",util:""},
  {name:"Loctite alfi quick",code:"126492",location:"",system:"",util:""},
];

let parts = JSON.parse(localStorage.getItem('fichas_parts') || 'null') ||
  INITIAL_DATA.map((p, i) => ({...p, id: Date.now() + i}));

let deletedParts = JSON.parse(localStorage.getItem('fichas_deleted') || 'null') || [];
let requests = JSON.parse(localStorage.getItem('fichas_requests') || 'null') || [];
let tempAccess = JSON.parse(localStorage.getItem('fichas_temp_access') || 'null'); // { expiresAt: number } | null

let currentRole = null; // 'admin' or 'viewer'
let activeFilter = 'Todos';
let searchQuery = '';
let editingId = null;
let deletingId = null;
let purgingId = null;
let rejectingId = null;
let rdCurrentId = null;
let fulfillingRequestId = null;
let expandedId = null;
let imageDataUrl = '';
let requestImageDataUrl = '';

function save() { localStorage.setItem('fichas_parts', JSON.stringify(parts)); }
function saveDeleted() { localStorage.setItem('fichas_deleted', JSON.stringify(deletedParts)); }
function saveRequests() { localStorage.setItem('fichas_requests', JSON.stringify(requests)); }
function saveTempAccess() { localStorage.setItem('fichas_temp_access', JSON.stringify(tempAccess)); }
function isTempAccessActive() { return !!(tempAccess && tempAccess.expiresAt > Date.now()); }

// AUTH
function tryLogin() {
  const pwd = document.getElementById('pwdInput').value;
  const err = document.getElementById('loginError');
  if (pwd === ADMIN_PASSWORD) {
    currentRole = 'admin';
    enterApp();
  } else {
    err.style.display = 'block';
    document.getElementById('pwdInput').value = '';
    document.getElementById('pwdInput').focus();
    setTimeout(() => err.style.display = 'none', 3000);
  }
}

function enterViewer() {
  currentRole = 'viewer';
  enterApp();
}

function enterApp() {
  document.getElementById('loginScreen').style.display = 'none';
  document.getElementById('mainApp').style.display = 'block';
  const badge = document.getElementById('roleBadge');
  if (currentRole === 'admin') {
    badge.textContent = '🔑 مسؤول';
    badge.className = 'role-badge role-admin';
    badge.title = 'اضغط للخروج';
    document.getElementById('addBtn').style.display = 'flex';
    document.getElementById('fabBtn').style.display = 'flex';
    document.getElementById('trashBtn').style.display = 'flex';
    document.getElementById('requestsBtn').style.display = 'flex';
    document.getElementById('tempAccessBtn').style.display = 'flex';
    document.getElementById('requestPartBtn').style.display = 'none';
    document.getElementById('requestFabBtn').style.display = 'none';
    document.getElementById('viewerHint').style.display = 'none';
  } else {
    badge.textContent = '👁️ مشاهد';
    badge.className = 'role-badge role-viewer';
    badge.title = 'اضغط للخروج';
    document.getElementById('trashBtn').style.display = 'none';
    document.getElementById('requestsBtn').style.display = 'none';
    document.getElementById('tempAccessBtn').style.display = 'none';
  }
  renderFilters();
  renderCards();
  updateTrashBadge();
  updateRequestsBadge();
  updateTempAccessUI();
}

function logout() {
  currentRole = null;
  document.getElementById('loginScreen').style.display = 'flex';
  document.getElementById('mainApp').style.display = 'none';
  document.getElementById('pwdInput').value = '';
}

// TEMP ACCESS (viewers can add parts directly for a limited time set by admin)
function formatCountdown(ms) {
  if (ms <= 0) return '00:00';
  const totalSec = Math.floor(ms / 1000);
  const h = Math.floor(totalSec / 3600);
  const m = Math.floor((totalSec % 3600) / 60);
  const s = totalSec % 60;
  const pad = n => String(n).padStart(2, '0');
  return h > 0 ? `${pad(h)}:${pad(m)}:${pad(s)}` : `${pad(m)}:${pad(s)}`;
}

function updateTempAccessUI() {
  const active = isTempAccessActive();
  if (currentRole === 'viewer') {
    document.getElementById('addBtn').style.display = active ? 'flex' : 'none';
    document.getElementById('fabBtn').style.display = active ? 'flex' : 'none';
    document.getElementById('requestPartBtn').style.display = active ? 'none' : 'flex';
    document.getElementById('requestFabBtn').style.display = active ? 'none' : 'flex';
    document.getElementById('viewerHint').style.display = active ? 'none' : 'flex';
    document.getElementById('tempHint').style.display = active ? 'flex' : 'none';
    if (active) document.getElementById('tempHintTime').textContent = formatCountdown(tempAccess.expiresAt - Date.now());
  } else if (currentRole === 'admin') {
    document.getElementById('tempAccessBtn').classList.toggle('is-active', active);
  }
  const statusBox = document.getElementById('tempModalStatus');
  if (statusBox) {
    const statusText = document.getElementById('tempModalStatusText');
    const endBtn = document.getElementById('tempEndBtn');
    if (active) {
      statusBox.classList.add('on');
      statusText.innerHTML = `🔓 مفعّل — متبقي <b>${formatCountdown(tempAccess.expiresAt - Date.now())}</b>`;
      endBtn.style.display = 'inline-block';
    } else {
      statusBox.classList.remove('on');
      statusText.textContent = '🔒 الوضع غير مفعّل حالياً';
      endBtn.style.display = 'none';
    }
  }
}

let _tempWasActive = isTempAccessActive();
setInterval(() => {
  const active = isTempAccessActive();
  if (_tempWasActive && !active && currentRole === 'viewer') showToast('⏱️ انتهت فترة الإضافة المؤقتة');
  _tempWasActive = active;
  if (currentRole) updateTempAccessUI();
}, 1000);

function openTempAccessModal() {
  if (currentRole !== 'admin') return;
  document.getElementById('tempCustomMinutes').value = '';
  document.getElementById('tempCustomMinutes').style.borderColor = '';
  updateTempAccessUI();
  document.getElementById('tempAccessModalOverlay').classList.add('show');
}
function closeTempAccessModal() { document.getElementById('tempAccessModalOverlay').classList.remove('show'); }
function handleTempAccessOverlayClick(e) { if (e.target === document.getElementById('tempAccessModalOverlay')) closeTempAccessModal(); }

function activateTempAccess(minutes) {
  if (currentRole !== 'admin' || !minutes || minutes <= 0) return;
  tempAccess = { expiresAt: Date.now() + minutes * 60000 };
  saveTempAccess();
  _tempWasActive = true;
  updateTempAccessUI();
  const label = minutes % 60 === 0 && minutes >= 60 ? `${minutes / 60} ساعة` : (minutes >= 1440 ? 'يوم كامل' : `${minutes} دقيقة`);
  showToast(`🔓 تم تفعيل الإضافة المؤقتة لمدة ${label}`);
  closeTempAccessModal();
}

function activateTempAccessCustom() {
  if (currentRole !== 'admin') return;
  const input = document.getElementById('tempCustomMinutes');
  const val = parseInt(input.value, 10);
  if (!val || val <= 0) { input.style.borderColor = 'var(--red)'; return; }
  input.style.borderColor = '';
  activateTempAccess(val);
}

function endTempAccessNow() {
  if (currentRole !== 'admin') return;
  tempAccess = null;
  saveTempAccess();
  _tempWasActive = false;
  updateTempAccessUI();
  showToast('🔒 تم إنهاء وضع الإضافة المؤقت');
}

// HELPERS
function getCatInfo(system) { return SYSTEM_ICONS[system] || {icon:'🔧',cls:'cat-default'}; }

function filterParts() {
  return parts.filter(p => {
    const q = searchQuery;
    const matchSearch = !q ||
      p.name.toLowerCase().includes(q) ||
      (p.code||'').toLowerCase().includes(q) ||
      (p.location||'').toLowerCase().includes(q) ||
      (p.system||'').toLowerCase().includes(q) ||
      (p.util||'').toLowerCase().includes(q);
    const matchFilter = activeFilter === 'Todos' || p.system === activeFilter;
    return matchSearch && matchFilter;
  });
}

function highlight(text) {
  if (!searchQuery || !text) return text || '';
  const re = new RegExp(`(${searchQuery.replace(/[.*+?^${}()|[\]\\]/g,'\\$&')})`, 'gi');
  return String(text).replace(re, '<mark style="background:#3b82f620;color:var(--accent2);border-radius:3px">$1</mark>');
}

function renderFilters() {
  const bar = document.getElementById('filtersBar');
  const allSystems = ['Todos', ...new Set(parts.map(p => p.system).filter(Boolean))];
  const cats = CATEGORIES.filter(c => allSystems.includes(c));
  bar.innerHTML = cats.map(c => `
    <button type="button" class="filter-btn ${activeFilter===c?'active':''}" data-category="${c}">
      ${c==='Todos'?'Todos':c}
    </button>`).join('');

  bar.querySelectorAll('.filter-btn').forEach(btn => {
    btn.addEventListener('click', () => setFilter(btn.dataset.category));
  });

  requestAnimationFrame(() => {
    const active = bar.querySelector('.filter-btn.active');
    active?.scrollIntoView({ behavior: 'smooth', inline: 'center', block: 'nearest' });
  });
}

function renderCards() {
  const filtered = filterParts();
  const container = document.getElementById('cardsContainer');
  document.getElementById('statsBar').innerHTML =
    `<span>${filtered.length}</span> result${filtered.length!==1?'s':''} of <span>${parts.length}</span> parts`;

  if (!filtered.length) {
    container.innerHTML = `<div class="empty"><div class="empty-icon">🔍</div><div class="empty-title">No results found</div></div>`;
    return;
  }

  const isAdmin = currentRole === 'admin';

  container.innerHTML = filtered.map(p => {
    const cat = getCatInfo(p.system);
    const exp = expandedId === p.id;
    return `
    <div class="card ${exp?'expanded':''}" id="card-${p.id}" onclick="toggleCard(${p.id},event)">
      <div class="card-header">
        <div class="card-icon ${cat.cls}">${cat.icon}</div>
        <div class="card-info">
          <div class="card-name">${highlight(p.name)}</div>
          ${p.code?`<div class="card-code"># ${highlight(p.code)}</div>`:''}
        </div>
        <div class="card-side">
          ${p.image ? `<div class="card-thumb" onclick="openImagePreview('${p.image.replace(/'/g, "\\'")}');event.stopPropagation()"><img src="${p.image}" alt="${p.name}"></div>` : ''}
          ${isAdmin ? `<div class="card-actions">
            <button class="card-btn btn-edit" onclick="editPart(${p.id},event)">✏️</button>
            <button class="card-btn btn-delete" onclick="askDelete(${p.id},event)">🗑</button>
          </div>` : ''}
        </div>
      </div>
      <div class="card-tags">
        ${p.system?`<span class="tag">${p.system}</span>`:''}
        ${p.location?`<span class="tag location">📍 ${highlight(p.location)}</span>`:''}
        ${p.util?`<span class="tag system">${p.util}</span>`:''}
        ${p.addedByViewer?`<span class="tag viewer-added" ${isAdmin?`onclick="verifyPart(${p.id},event)" title="اضغط للتأكيد كمسؤول" style="cursor:pointer"`:''}>👁️ أضافها مشاهد${isAdmin?' · تأكيد ✓':''}</span>`:''}
      </div>
      ${p.translation ? `
      <div class="card-arabic">
        <div class="arabic-row"><span class="arabic-label">الترجمة:</span><span class="arabic-value">${p.translation}</span></div>
        ${p.phonetic?`<div class="arabic-row"><span class="arabic-label">النطق:</span><span class="arabic-phonetic">${p.phonetic}</span></div>`:''}
      </div>` : (isAdmin ? `<button class="translate-btn" onclick="openModal(parts.find(x=>x.id===${p.id}),true);event.stopPropagation()">🌐 ترجمة للعربي</button>` : '')}
      <div class="card-details">
        ${p.code?`<div class="detail-row"><span class="detail-label">Code:</span><span class="detail-value" style="font-family:monospace;color:var(--accent2)">${p.code}</span></div>`:''}
        ${p.location?`<div class="detail-row"><span class="detail-label">Location:</span><span class="detail-value">${p.location}</span></div>`:''}
        ${p.system?`<div class="detail-row"><span class="detail-label">Subsystem:</span><span class="detail-value">${p.system}</span></div>`:''}
        ${p.util?`<div class="detail-row"><span class="detail-label">Util:</span><span class="detail-value">${p.util}</span></div>`:''}
      </div>
    </div>`;
  }).join('');
}

function toggleCard(id, e) {
  if (e.target.closest('.card-btn,.translate-btn')) return;
  expandedId = expandedId === id ? null : id;
  renderCards();
}

function setFilter(cat) { activeFilter = cat; renderFilters(); renderCards(); }

function handleSearch(val) {
  searchQuery = val.toLowerCase().trim();
  document.getElementById('clearBtn').classList.toggle('show', val.length > 0);
  renderCards();
}

function clearSearch() {
  const inp = document.getElementById('searchInput');
  inp.value = ''; handleSearch(''); inp.focus();
}

function loadImageFile(event) {
  const file = event.target.files[0];
  if (!file) {
    imageDataUrl = '';
    return;
  }
  const reader = new FileReader();
  reader.onload = () => {
    imageDataUrl = reader.result;
    document.getElementById('fImage').value = imageDataUrl;
  };
  reader.readAsDataURL(file);
}

function removeImage() {
  imageDataUrl = '';
  document.getElementById('fImage').value = '';
  document.getElementById('fImageFile').value = '';
}

function loadRequestImageFile(event) {
  const file = event.target.files[0];
  if (!file) { removeRequestImage(); return; }
  const reader = new FileReader();
  reader.onload = () => {
    requestImageDataUrl = reader.result;
    document.getElementById('rImagePreview').src = requestImageDataUrl;
    document.getElementById('rImagePreviewWrap').style.display = 'flex';
  };
  reader.readAsDataURL(file);
}

function removeRequestImage() {
  requestImageDataUrl = '';
  document.getElementById('rImageFile').value = '';
  document.getElementById('rImagePreview').src = '';
  document.getElementById('rImagePreviewWrap').style.display = 'none';
}

// MODAL (admin, or viewer during temp access — add only, never edit)
function openModal(part, scrollToTrans) {
  const canAdd = currentRole === 'admin' || (currentRole === 'viewer' && isTempAccessActive());
  if (!canAdd) return;
  if (part && currentRole !== 'admin') return;
  editingId = part ? part.id : null;
  document.getElementById('modalTitle').textContent = part ? '✏️ Edit Part' : '➕ New Part';
  document.getElementById('fName').value = part ? part.name : '';
  document.getElementById('fCode').value = part ? (part.code||'') : '';
  document.getElementById('fLocation').value = part ? (part.location||'') : '';
  document.getElementById('fSystem').value = part ? (part.system||'') : '';
  document.getElementById('fUtil').value = part ? (part.util||'') : '';
  document.getElementById('fTrans').value = part ? (part.translation||'') : '';
  document.getElementById('fPhonetic').value = part ? (part.phonetic||'') : '';
  document.getElementById('fImage').value = part ? (part.image||'') : '';
  document.getElementById('fImageFile').value = '';
  imageDataUrl = part ? (part.image||'') : '';
  document.getElementById('modalOverlay').classList.add('show');
  setTimeout(() => (scrollToTrans ? document.getElementById('fTrans') : document.getElementById('fName')).focus(), 300);
}

function closeModal() { document.getElementById('modalOverlay').classList.remove('show'); editingId = null; fulfillingRequestId = null; }
function handleOverlayClick(e) { if (e.target === document.getElementById('modalOverlay')) closeModal(); }

function savePart() {
  const canAdd = currentRole === 'admin' || (currentRole === 'viewer' && isTempAccessActive() && !editingId);
  if (!canAdd) return;
  const name = document.getElementById('fName').value.trim();
  if (!name) { document.getElementById('fName').style.borderColor = 'var(--red)'; return; }
  document.getElementById('fName').style.borderColor = '';
  const data = {
    name,
    code: document.getElementById('fCode').value.trim(),
    location: document.getElementById('fLocation').value.trim(),
    system: document.getElementById('fSystem').value,
    util: document.getElementById('fUtil').value.trim(),
    translation: document.getElementById('fTrans').value.trim(),
    phonetic: document.getElementById('fPhonetic').value.trim(),
    image: document.getElementById('fImage').value.trim(),
  };
  if (editingId) {
    parts = parts.map(p => p.id === editingId ? {...p,...data} : p);
    showToast('✅ Part updated');
  } else {
    if (currentRole === 'viewer') data.addedByViewer = true;
    parts.unshift({...data, id: Date.now()});
    if (fulfillingRequestId) {
      requests = requests.filter(r => r.id !== fulfillingRequestId);
      saveRequests(); updateRequestsBadge();
      showToast('✅ تمت إضافة القطعة وإغلاق الطلب');
    } else {
      showToast('✅ Part added');
    }
  }
  fulfillingRequestId = null;
  save(); closeModal(); renderFilters(); renderCards();
}

function editPart(id, e) {
  e.stopPropagation();
  if (currentRole !== 'admin') return;
  const part = parts.find(p => p.id === id);
  if (part) openModal(part);
}

function verifyPart(id, e) {
  e.stopPropagation();
  if (currentRole !== 'admin') return;
  parts = parts.map(p => p.id === id ? {...p, addedByViewer: false} : p);
  save(); renderCards();
  showToast('✅ تم تأكيد القطعة من المسؤول');
}

function askDelete(id, e) {
  e.stopPropagation();
  if (currentRole !== 'admin') return;
  deletingId = id;
  const part = parts.find(p => p.id === id);
  document.getElementById('confirmText').textContent = `سيتم نقل "${part?.name}" إلى سلة المحذوفات.`;
  document.getElementById('confirmOverlay').classList.add('show');
}

function closeConfirm() { document.getElementById('confirmOverlay').classList.remove('show'); deletingId = null; }

function confirmDelete() {
  if (!deletingId || currentRole !== 'admin') return;
  const part = parts.find(p => p.id === deletingId);
  if (part) {
    parts = parts.filter(p => p.id !== deletingId);
    deletedParts.unshift({...part, deletedAt: Date.now()});
    saveDeleted();
  }
  save(); closeConfirm();
  if (expandedId === deletingId) expandedId = null;
  renderFilters(); renderCards(); updateTrashBadge();
  showToast('🗑️ تم نقل القطعة لسلة المحذوفات');
  deletingId = null;
}

// TRASH BIN
function updateTrashBadge() {
  const badge = document.getElementById('trashBadge');
  if (!badge) return;
  if (deletedParts.length > 0) {
    badge.textContent = deletedParts.length > 99 ? '99+' : deletedParts.length;
    badge.style.display = 'flex';
  } else {
    badge.style.display = 'none';
  }
}

function openTrash() {
  if (currentRole !== 'admin') return;
  renderTrashList();
  document.getElementById('trashOverlay').classList.add('show');
}

function closeTrash() { document.getElementById('trashOverlay').classList.remove('show'); }

function handleTrashOverlayClick(e) { if (e.target === document.getElementById('trashOverlay')) closeTrash(); }

function renderTrashList() {
  const list = document.getElementById('trashList');
  if (!deletedParts.length) {
    list.innerHTML = `<div class="trash-empty">🗑️ سلة المحذوفات فارغة</div>`;
    return;
  }
  list.innerHTML = deletedParts.map(p => {
    const d = new Date(p.deletedAt);
    const dateStr = d.toLocaleDateString('ar', {day:'numeric', month:'short', year:'numeric'});
    return `
    <div class="trash-item">
      <div class="trash-item-info">
        <div class="trash-item-name">${p.name}</div>
        <div class="trash-item-meta">${p.code ? '# '+p.code+' · ' : ''}حُذفت في ${dateStr}</div>
      </div>
      <div class="trash-item-btns">
        <button class="trash-restore-btn" onclick="restorePart(${p.id})">↩️ استرجاع</button>
        <button class="trash-purge-btn" onclick="askPurge(${p.id})">🗑️ حذف نهائي</button>
      </div>
    </div>`;
  }).join('');
}

function restorePart(id) {
  if (currentRole !== 'admin') return;
  const part = deletedParts.find(p => p.id === id);
  if (!part) return;
  const { deletedAt, ...rest } = part;
  deletedParts = deletedParts.filter(p => p.id !== id);
  parts.unshift(rest);
  save(); saveDeleted();
  renderFilters(); renderCards(); renderTrashList(); updateTrashBadge();
  showToast('↩️ تم استرجاع القطعة');
}

function askPurge(id) {
  purgingId = id;
  const part = deletedParts.find(p => p.id === id);
  document.getElementById('purgeConfirmText').textContent = `سيتم حذف "${part?.name}" نهائيًا ولا يمكن استرجاعها.`;
  document.getElementById('purgeConfirmOverlay').classList.add('show');
}

function closePurgeConfirm() { document.getElementById('purgeConfirmOverlay').classList.remove('show'); purgingId = null; }

function confirmPurge() {
  if (!purgingId || currentRole !== 'admin') return;
  deletedParts = deletedParts.filter(p => p.id !== purgingId);
  saveDeleted(); closePurgeConfirm();
  renderTrashList(); updateTrashBadge();
  showToast('🗑️ تم الحذف النهائي');
  purgingId = null;
}

// PART REQUESTS (viewer submits, admin reviews)
function openRequestModal() {
  if (currentRole !== 'viewer') return;
  document.getElementById('rName').value = '';
  document.getElementById('rCode').value = '';
  document.getElementById('rLocation').value = '';
  document.getElementById('rSystem').value = '';
  document.getElementById('rNote').value = '';
  removeRequestImage();
  document.getElementById('requestModalOverlay').classList.add('show');
  setTimeout(() => document.getElementById('rName').focus(), 300);
}

function closeRequestModal() { document.getElementById('requestModalOverlay').classList.remove('show'); }
function handleRequestOverlayClick(e) { if (e.target === document.getElementById('requestModalOverlay')) closeRequestModal(); }

function submitRequest() {
  if (currentRole !== 'viewer') return;
  const name = document.getElementById('rName').value.trim();
  if (!name) { document.getElementById('rName').style.borderColor = 'var(--red)'; return; }
  document.getElementById('rName').style.borderColor = '';
  requests.unshift({
    id: Date.now(),
    name,
    code: document.getElementById('rCode').value.trim(),
    location: document.getElementById('rLocation').value.trim(),
    system: document.getElementById('rSystem').value,
    note: document.getElementById('rNote').value.trim(),
    image: requestImageDataUrl,
    requestedAt: Date.now(),
  });
  saveRequests(); closeRequestModal();
  showToast('📨 تم إرسال الطلب للمسؤول');
}

function updateRequestsBadge() {
  const badge = document.getElementById('requestsBadge');
  if (!badge) return;
  if (requests.length > 0) {
    badge.textContent = requests.length > 99 ? '99+' : requests.length;
    badge.style.display = 'flex';
  } else {
    badge.style.display = 'none';
  }
}

function openRequests() {
  if (currentRole !== 'admin') return;
  renderRequestsList();
  document.getElementById('requestsOverlay').classList.add('show');
}

function closeRequests() { document.getElementById('requestsOverlay').classList.remove('show'); }
function handleRequestsOverlayClick(e) { if (e.target === document.getElementById('requestsOverlay')) closeRequests(); }

function renderRequestsList() {
  const list = document.getElementById('requestsList');
  if (!requests.length) {
    list.innerHTML = `<div class="requests-empty">📥 لا توجد طلبات حالياً</div>`;
    return;
  }
  list.innerHTML = requests.map(r => {
    const d = new Date(r.requestedAt);
    const dateStr = d.toLocaleDateString('ar', {day:'numeric', month:'short', year:'numeric'});
    return `
    <div class="request-item" onclick="viewRequestDetail(${r.id})" style="cursor:pointer;">
      <div style="display:flex;align-items:center;gap:10px;">
        ${r.image ? `<div class="card-thumb" style="width:44px;height:44px;border-radius:12px;overflow:hidden;flex-shrink:0;"><img src="${r.image}" style="width:100%;height:100%;object-fit:cover;display:block;"></div>` : ''}
        <div style="flex:1;min-width:0;">
          <div class="request-item-name">${r.name}</div>
          <div class="request-item-meta">${r.code ? '# '+r.code+' · ' : ''}${r.system ? r.system+' · ' : ''}${r.location ? '📍 '+r.location+' · ' : ''}طُلبت في ${dateStr}</div>
        </div>
      </div>
      ${r.note ? `<div class="request-item-note">${r.note}</div>` : ''}
      <div class="request-item-btns">
        <button class="request-accept-btn" onclick="event.stopPropagation();acceptRequest(${r.id})">➕ إضافة</button>
        <button class="request-reject-btn" onclick="event.stopPropagation();askRejectRequest(${r.id})">❌ رفض</button>
      </div>
    </div>`;
  }).join('');
}

function viewRequestDetail(id) {
  if (currentRole !== 'admin') return;
  const r = requests.find(x => x.id === id);
  if (!r) return;
  rdCurrentId = id;
  const d = new Date(r.requestedAt);
  const dateStr = d.toLocaleDateString('ar', {day:'numeric', month:'short', year:'numeric'});

  const imgWrap = document.getElementById('rdImageWrap');
  if (r.image) { document.getElementById('rdImage').src = r.image; imgWrap.style.display = 'block'; }
  else { imgWrap.style.display = 'none'; }

  document.getElementById('rdName').textContent = r.name;

  const rows = [];
  if (r.code) rows.push(['🔢 رقم القطعة', r.code]);
  if (r.location) rows.push(['📍 الموقع', r.location]);
  if (r.system) rows.push(['⚙️ النظام الفرعي', r.system]);
  rows.push(['🗓️ طُلبت في', dateStr]);
  document.getElementById('rdMetaRows').innerHTML = rows.map(([label, val]) => `
    <div style="display:flex;justify-content:space-between;background:var(--surface);border:1.5px solid var(--border);border-radius:10px;padding:9px 12px;font-size:13px;">
      <span style="color:var(--text-muted);">${label}</span>
      <span style="color:var(--text);font-weight:600;">${val}</span>
    </div>`).join('');

  const noteWrap = document.getElementById('rdNoteWrap');
  if (r.note) { document.getElementById('rdNote').textContent = r.note; noteWrap.style.display = 'block'; }
  else { noteWrap.style.display = 'none'; }

  document.getElementById('requestDetailOverlay').classList.add('show');
}

function closeRequestDetail() { document.getElementById('requestDetailOverlay').classList.remove('show'); rdCurrentId = null; }
function handleRequestDetailOverlayClick(e) { if (e.target === document.getElementById('requestDetailOverlay')) closeRequestDetail(); }

function acceptRequest(id) {
  if (currentRole !== 'admin') return;
  const r = requests.find(x => x.id === id);
  if (!r) return;
  closeRequests();
  closeRequestDetail();
  fulfillingRequestId = id;
  openModal({ name: r.name, code: r.code, location: r.location, system: r.system, util: r.note, image: r.image });
  editingId = null; // ensure this is treated as a new part, not an edit
}

function askRejectRequest(id) {
  rejectingId = id;
  const r = requests.find(x => x.id === id);
  document.getElementById('rejectConfirmText').textContent = `سيتم حذف طلب "${r?.name}".`;
  document.getElementById('rejectConfirmOverlay').classList.add('show');
}

function closeRejectConfirm() { document.getElementById('rejectConfirmOverlay').classList.remove('show'); rejectingId = null; }

function confirmRejectRequest() {
  if (!rejectingId || currentRole !== 'admin') return;
  requests = requests.filter(r => r.id !== rejectingId);
  saveRequests(); closeRejectConfirm();
  renderRequestsList(); updateRequestsBadge();
  showToast('❌ تم رفض الطلب');
  rejectingId = null;
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

function openImagePreview(src) {
  const preview = document.getElementById('previewImage');
  preview.src = src;
  document.getElementById('imagePreviewOverlay').classList.add('show');
}

function closeImagePreview(event) {
  if (event.target.id !== 'imagePreviewOverlay') return;
  document.getElementById('imagePreviewOverlay').classList.remove('show');
}

document.addEventListener('keydown', e => { if (e.key==='Escape') { closeModal(); closeConfirm(); closeTrash(); closePurgeConfirm(); closeRequestModal(); closeRequests(); closeRequestDetail(); closeRejectConfirm(); closeTempAccessModal(); document.getElementById('imagePreviewOverlay').classList.remove('show'); } });
document.getElementById('pwdInput').addEventListener('keydown', e => { if (e.key==='Enter') tryLogin(); });
</script>
</body>
</html>
