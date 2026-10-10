<!DOCTYPE html>  
<html lang="zh-CN">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
<title>**梦角** **·** **字卡传讯**</title>  
<style>  
  * { margin:0; padding:0; box-sizing:border-box; }  
  html, body { height: 100%; }  
  body {  
    background: #ededed;  
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;  
    color: #1a1a1a;  
    -webkit-font-smoothing: antialiased;  
    display: block;  
    overflow: hidden;  
  }  
  .chat-container {  
    width: 100%; max-width: 900px; height: 100vh;  
    margin: 0 auto; background: #ededed;  
    display: flex; flex-direction: column; position: relative;  
  }  
  .chat-header {  
    padding: 14px 22px;  
    display: flex; align-items: center; justify-content: space-between;  
    background: #f7f7f7;  
    border-bottom: 1px solid #e2e2e2;  
    flex-shrink: 0; position: relative; z-index: 100;  
  }  
  .header-left { display: flex; align-items: center; gap: 12px; }  
  .header-avatar {  
    width: 38px; height: 38px; border-radius: 6px;  
    background: #e0e0e0; display: flex; align-items: center; justify-content: center;  
    font-size: 17px; overflow: hidden; flex-shrink: 0;  
    user-select: none; cursor: pointer; position: relative; transition: opacity 0.15s;  
  }  
  .header-avatar:hover { opacity: 0.85; }  
  .header-avatar::after {  
    content: '📷'; position: absolute; inset: 0;  
    background: rgba(0,0,0,0.35); color: #fff; font-size: 13px;  
    display: flex; align-items: center; justify-content: center;  
    opacity: 0; transition: opacity 0.15s;  
  }  
  .header-avatar:hover::after { opacity: 1; }  
  .header-avatar img, .avatar img { width:100%; height:100%; object-fit:cover; display:block; }  
  .title-group { display: flex; flex-direction: column; gap: 1px; min-width: 0; }  
  .title-group .name {  
    font-size: 16px; font-weight: 600; color: #1a1a1a; line-height: 1.25;  
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;  
  }  
  .title-group .sub {  
    font-size: 11.5px; color: #b0b0b0; letter-spacing: 0.3px;  
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 240px;  
  }  
  .header-right { display: flex; align-items: center; gap: 8px; position: relative; }  
  .private-tag {  
    font-size: 11px; color: #b0b0b0; background: #fff;  
    padding: 4px 10px; border-radius: 20px; border: 1px solid #ececec; user-select: none;  
  }  
  .menu-btn {  
    width: 34px; height: 34px; border-radius: 8px;  
    background: transparent; border: 1px solid transparent; color: #666;  
    font-size: 20px; line-height: 1; cursor: pointer;  
    display: flex; align-items: center; justify-content: center;  
    transition: all 0.15s; font-family: inherit; padding-bottom: 6px;  
  }  
  .menu-btn:hover { background: #ededed; }  
  .menu-btn:active { background: #e0e0e0; }  
  .dropdown {  
    position: absolute; top: calc(100% + 8px); right: 0;  
    background: #ffffff; border-radius: 10px;  
    box-shadow: 0 8px 28px rgba(0,0,0,0.14), 0 0 0 1px rgba(0,0,0,0.04);  
    padding: 6px; min-width: 220px;  
    display: none; flex-direction: column; gap: 2px;  
    z-index: 200;  
  }  
  .dropdown.active { display: flex; }  
  .dropdown-item {  
    display: flex; align-items: center; gap: 10px;  
    padding: 10px 12px; border-radius: 7px;  
    font-size: 13.5px; color: #333; cursor: pointer;  
    transition: background 0.12s; font-family: inherit;  
    background: none; border: none; text-align: left; width: 100%; letter-spacing: 0.1px;  
  }  
  .dropdown-item:hover { background: #f5f5f5; }  
  .dropdown-item .icon { font-size: 14px; width: 20px; text-align: center; opacity: 0.7; }  
  .dropdown-divider { height: 1px; background: #f0f0f0; margin: 4px 8px; }  
  
  .messages {  
    flex: 1; overflow-y: auto; padding: 24px 24px 14px;  
    display: flex; flex-direction: column; gap: 18px;  
    background: #ededed; scroll-behavior: smooth;  
  }  
  .messages::-webkit-scrollbar { width: 6px; }  
  .messages::-webkit-scrollbar-thumb { background: #d5d5d5; border-radius: 4px; }  
  .msg-row {  
    display: flex; gap: 12px; align-items: flex-start;  
    max-width: 100%; min-width: 0; animation: appear 0.2s ease-out;  
  }  
  .msg-row.dreamer { align-self: flex-start; flex-direction: row; }  
  .msg-row.user    { align-self: flex-end;   flex-direction: row-reverse; }  
  @keyframes appear {  
    from { opacity:0; transform: translateY(6px); }  
    to { opacity:1; transform: translateY(0); }  
  }  
  .avatar {  
    width: 42px; height: 42px; border-radius: 6px; background: #fff;  
    display: flex; align-items: center; justify-content: center; font-size: 18px;  
    overflow: hidden; flex-shrink: 0; position: relative;  
    box-shadow: 0 1px 2px rgba(0,0,0,0.06); user-select: none;  
  }  
  .avatar.dreamer-avatar { background: #dfe6f0; }  
  .avatar.user-avatar    { background: #d8e4f5; }  
  .bubble {  
    width: fit-content; max-width: min(560px, 68%);  
    padding: 11px 15px; border-radius: 6px;  
    font-size: 15px; line-height: 1.55;  
    overflow-wrap: break-word; word-break: normal;  
    letter-spacing: 0.1px; position: relative;  
    box-shadow: 0 1px 1px rgba(0,0,0,0.03);  
  }  
  .bubble.dreamer { background: #fff;    color: #1a1a1a; }  
  .bubble.user    { background: #95ec69; color: #1a1a1a; }  
  .bubble.dreamer::before {  
    content: ''; position: absolute; left: -6px; top: 13px;  
    border-top: 6px solid transparent; border-bottom: 6px solid transparent;  
    border-right: 6px solid #fff;  
  }  
  .bubble.user::before {  
    content: ''; position: absolute; right: -6px; top: 13px;  
    border-top: 6px solid transparent; border-bottom: 6px solid transparent;  
    border-left: 6px solid #95ec69;  
  }  
  .input-bar {  
    padding: 12px 22px 18px; background: #f7f7f7;  
    display: flex; gap: 12px; align-items: center;  
    border-top: 1px solid #e2e2e2; flex-shrink: 0;  
  }  
  .input-bar input {  
    flex: 1; background: #fff; border: 1px solid #e0e0e0; border-radius: 8px;  
    padding: 12px 16px; font-size: 16px; color: #1a1a1a; outline: none;  
    transition: border-color 0.15s; font-family: inherit;  
  }  
  .input-bar input::placeholder { color: #bbb; }  
  .input-bar input:focus { border-color: #c8c8c8; }  
  .send-btn {  
    height: 42px; padding: 0 24px; border-radius: 8px;  
    background: #07c160; border: none; color: #fff;  
    font-size: 14.5px; font-weight: 500; cursor: pointer;  
    transition: all 0.15s; flex-shrink: 0; font-family: inherit;  
  }  
  .send-btn:hover  { background: #06ad56; }  
  .send-btn:active { transform: scale(0.97); background: #059c4d; }  
  .overlay {  
    position: fixed; inset: 0; background: rgba(0,0,0,0.4);  
    display: flex; align-items: center; justify-content: center;  
    z-index: 500; opacity: 0; visibility: hidden;  
    transition: opacity 0.2s, visibility 0.2s; padding: 20px;  
  }  
  .overlay.active { opacity: 1; visibility: visible; }  
  .modal {  
    background: #fff; width: 100%; max-width: 540px;  
    border-radius: 14px; box-shadow: 0 12px 40px rgba(0,0,0,0.2);  
    padding: 22px 22px 18px;  
    display: flex; flex-direction: column; gap: 12px;  
    max-height: 86vh; overflow: hidden;  
  }  
  .modal h2 { font-size: 16px; font-weight: 600; color: #1a1a1a; }  
  .modal .desc { font-size: 12.5px; color: #999; margin-top: -4px; line-height: 1.6; }  
  .modal textarea {  
    width: 100%; height: 220px; background: #fafafa;  
    border: 1px solid #e8e8e8; border-radius: 8px;  
    padding: 12px 14px; font-family: inherit; font-size: 15px; color: #333;  
    resize: vertical; outline: none; line-height: 1.7;  
    transition: border-color 0.15s, background 0.15s;  
  }  
  .modal textarea::placeholder { color: #ccc; }  
  .modal textarea:focus { border-color: #c8c8c8; background: #fff; }  
  .modal input**[**type="text"**]**, .modal input**[**type="number"**]** {  
    width: 100%; background: #fafafa;  
    border: 1px solid #e8e8e8; border-radius: 8px;  
    padding: 12px 14px; font-family: inherit; font-size: 16px; color: #333;  
    outline: none; transition: border-color 0.15s, background 0.15s;  
  }  
  .modal input:focus { border-color: #c8c8c8; background: #fff; }  
  .modal .hint { font-size: 11.5px; color: #bbb; margin-top: -4px; }  
  .modal-actions {  
    display: flex; gap: 10px; justify-content: flex-end; margin-top: 4px;  
  }  
  .modal-actions button {  
    padding: 9px 22px; border-radius: 8px;  
    font-size: 14px; font-weight: 500;  
    cursor: pointer; transition: all 0.15s;  
    font-family: inherit; border: none;  
  }  
  .btn-cancel { background: #f2f2f2; color: #666; }  
  .btn-cancel:hover { background: #e8e8e8; }  
  .btn-save { background: #07c160; color: #fff; }  
  .btn-save:hover  { background: #06ad56; }  
  .btn-save:active { transform: scale(0.97); }  
  .form-row { display: flex; flex-direction: column; gap: 6px; }  
  .form-row label { font-size: 13px; color: #555; font-weight: 500; }  
  .form-row .hint { font-size: 11.5px; color: #b0b0b0; margin-top: 0; }  
  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }  
  .group-list {  
    overflow-y: auto; max-height: 52vh;  
    display: flex; flex-direction: column; gap: 8px; padding: 2px;  
  }  
  .group-list::-webkit-scrollbar { width: 5px; }  
  .group-list::-webkit-scrollbar-thumb { background: #ddd; border-radius: 3px; }  
  .group-card {  
    display: flex; align-items: center; justify-content: space-between;  
    background: #fafafa; border: 1px solid #eee; border-radius: 10px;  
    padding: 14px 16px; cursor: pointer; transition: all 0.15s;  
  }  
  .group-card:hover { background: #f3f3f3; border-color: #e0e0e0; }  
  .group-card-left { display: flex; flex-direction: column; gap: 3px; min-width: 0; }  
  .group-card-name {  
    font-size: 14.5px; font-weight: 500; color: #1a1a1a;  
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;  
  }  
  .group-card-count { font-size: 12px; color: #aaa; }  
  .group-card-right { display: flex; align-items: center; gap: 6px; flex-shrink: 0; }  
  .icon-btn {  
    background: none; border: none; padding: 5px 8px; border-radius: 6px;  
    cursor: pointer; color: #999; font-size: 14px;  
    transition: all 0.12s; font-family: inherit;  
  }  
  .icon-btn:hover { background: #ececec; color: #555; }  
  .icon-btn.danger:hover { background: #ffecec; color: #e33; }  
  .add-group-btn {  
    width: 100%; padding: 13px; border-radius: 10px;  
    background: #f7f7f7; border: 1px dashed #ddd; color: #666;  
    font-size: 14px; cursor: pointer; font-family: inherit;  
    transition: all 0.15s; margin-top: 4px;  
  }  
  .add-group-btn:hover { background: #f0f0f0; border-color: #ccc; color: #333; }  
  .panel-header { display: flex; align-items: center; gap: 10px; padding-bottom: 4px; }  
  .back-btn {  
    background: none; border: none; padding: 6px 10px; border-radius: 6px;  
    cursor: pointer; color: #666; font-size: 13px;  
    font-family: inherit; transition: background 0.12s;  
  }  
  .back-btn:hover { background: #f0f0f0; }  
  .panel-title {  
    font-size: 15px; font-weight: 600; color: #1a1a1a;  
    flex: 1; min-width: 0;  
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;  
  }  
  .hidden-file { display: none; }  
  @media (max-width: 600px) {  
    .chat-header { padding: 12px 14px; }  
    .messages { padding: 16px 12px 10px; gap: 16px; }  
    .input-bar { padding: 8px 12px 14px; }  
    .bubble { max-width: 78%; font-size: 15px; }  
    .avatar { width: 38px; height: 38px; }  
    .private-tag { display: none; }  
    .send-btn { padding: 0 18px; }  
    .dropdown { min-width: 190px; }  
    .title-group .sub { max-width: 140px; }  
    .modal { padding: 18px 16px 14px; }  
    .form-grid { grid-template-columns: 1fr; }  
  }  
</style>  
</head>  
<body>  
  
<div class="chat-container">  
  <div class="chat-header">  
    <div class="header-left">  
      <div class="header-avatar" id="headerAvatar" title="**点击更换梦角头像**">✦</div>  
      <div class="title-group">  
        <span class="name" id="titleName">**梦角**</span>  
        <span class="sub" id="titleStatus">**在线**</span>  
      </div>  
    </div>  
    <div class="header-right">  
      <span class="private-tag">**私密**</span>  
      <button class="menu-btn" id="menuBtn" title="**设置**">⋮</button>  
      <div class="dropdown" id="dropdown">  
        <button class="dropdown-item" id="changeDreamerAvatar"><span class="icon">✦</span> **更换梦角头像**</button>  
        <button class="dropdown-item" id="changeUserAvatar"><span class="icon">👤</span> **更换我的头像**</button>  
        <div class="dropdown-divider"></div>  
        <button class="dropdown-item" id="changeDreamerName"><span class="icon">✎</span> **修改昵称**</button>  
        <button class="dropdown-item" id="manageStatus"><span class="icon">💬</span> **管理状态**</button>  
        <button class="dropdown-item" id="replySettings"><span class="icon">⏱</span> **回复设置**</button>  
        <div class="dropdown-divider"></div>  
        <button class="dropdown-item" id="editCards"><span class="icon">📇</span> **字卡库**</button>  
        <button class="dropdown-item" id="clearChat"><span class="icon">🗑</span> **清空聊天记录**</button>  
      </div>  
    </div>  
  </div>  
  
  <div class="messages" id="messages"></div>  
  
  <div class="input-bar">  
    <input type="text" id="msgInput" placeholder="**说些什么**……" autocomplete="off">  
    <button class="send-btn" id="sendBtn">**发送**</button>  
  </div>  
</div>  
  
<!-- **字卡库** -->  
<div class="overlay" id="cardsOverlay">  
  <div class="modal">  
    <div id="groupListView">  
      <h2 style="margin-bottom:6px">**字卡库**</h2>  
      <p class="desc" style="margin-bottom:10px">**点进一个分组编辑字卡，一行一张**</p>  
      <div class="group-list" id="groupList"></div>  
      <button class="add-group-btn" id="addGroupBtn">+ **新建分组**</button>  
      <div class="modal-actions" style="margin-top:12px">  
        <button class="btn-cancel" id="cardsCloseBtn">**关闭**</button>  
      </div>  
    </div>  
    <div id="groupEditView" style="display:none">  
      <div class="panel-header">  
        <button class="back-btn" id="backToGroups">‹ **返回**</button>  
        <div class="panel-title" id="editGroupName">**分组**</div>  
        <button class="icon-btn" id="renameGroupBtn" title="**重命名**">✎</button>  
      </div>  
      <p class="desc" style="margin-bottom:6px">**一行一张字卡，空行自动忽略**</p>  
      <textarea id="groupCardsInput" placeholder="**今天也想念你**&#10;**我一直在你身边**&#10;**你的眼睛真好看**"></textarea>  
      <div class="modal-actions" style="margin-top:10px">  
        <button class="btn-cancel" id="groupCancelBtn">**取消**</button>  
        <button class="btn-save" id="groupSaveBtn">**保存**</button>  
      </div>  
    </div>  
  </div>  
</div>  
  
<!-- **昵称** -->  
<div class="overlay" id="nameOverlay">  
  <div class="modal" style="max-width:420px">  
    <h2>**修改昵称**</h2>  
    <p class="desc">**想叫他**/**她什么，都可以**</p>  
    <input type="text" id="nameInput" placeholder="**例如：他、阿澈**……" maxlength="20">  
    <div class="modal-actions">  
      <button class="btn-cancel" id="nameCancelBtn">**取消**</button>  
      <button class="btn-save" id="nameSaveBtn">**保存**</button>  
    </div>  
  </div>  
</div>  
  
<!-- **状态** -->  
<div class="overlay" id="statusOverlay">  
  <div class="modal" style="max-width:460px">  
    <h2>**状态**</h2>  
    <p class="desc">**一行一个。梦角回你消息时会随机换一个**</p>  
    <textarea id="statusInput" style="height:180px" placeholder="**在线**&#10;**在想你**&#10;**发呆中**&#10;**有点困**&#10;**在看书**"></textarea>  
    <div class="modal-actions">  
      <button class="btn-cancel" id="statusCancelBtn">**取消**</button>  
      <button class="btn-save" id="statusSaveBtn">**保存**</button>  
    </div>  
  </div>  
</div>  
  
<!-- **回复设置** -->  
<div class="overlay" id="replyOverlay">  
  <div class="modal" style="max-width:460px">  
    <h2>**回复设置**</h2>  
    <p class="desc">**控制梦角回消息的节奏**</p>  
    <div class="form-grid">  
      <div class="form-row">  
        <label>**延迟最短（毫秒）**</label>  
        <input type="number" id="delayMin" min="0" max="20000" step="100">  
        <span class="hint">**例如** 400</span>  
      </div>  
      <div class="form-row">  
        <label>**延迟最长（毫秒）**</label>  
        <input type="number" id="delayMax" min="0" max="20000" step="100">  
        <span class="hint">**例如** 1500</span>  
      </div>  
    </div>  
    <div class="form-grid">  
      <div class="form-row">  
        <label>**最少回复条数**</label>  
        <input type="number" id="countMin" min="1" max="10" step="1">  
      </div>  
      <div class="form-row">  
        <label>**最多回复条数**</label>  
        <input type="number" id="countMax" min="1" max="10" step="1">  
      </div>  
    </div>  
    <div class="form-row">  
      <label>**连续回复的间隔（毫秒）**</label>  
      <input type="number" id="betweenDelay" min="0" max="10000" step="100">  
      <span class="hint">**当回复多条时，每条之间的间隔**</span>  
    </div>  
    <div class="modal-actions">  
      <button class="btn-cancel" id="replyCancelBtn">**取消**</button>  
      <button class="btn-save" id="replySaveBtn">**保存**</button>  
    </div>  
  </div>  
</div>  
  
<input type="file" accept="image/*" class="hidden-file" id="fileDreamer">  
<input type="file" accept="image/*" class="hidden-file" id="fileUser">  
  
<script>  
(function(){  
  function $(id) { return document.getElementById(id); }  
  function randBetween(min, max) {  
    if (max < min) { const t = min; min = max; max = t; }  
    return Math.floor(Math.random() * (max - min + 1)) + min;  
  }  
  
  // ---------------- **数据** ----------------  
  let groups = **[**  
    { name: '**温柔**', cards: **[**'**今天也想念你**','**我一直在你身边**','**你的眼睛真好看**','**抱抱你，辛苦啦**'**]** },  
    { name: '**日常**', cards: **[**'**要一起散步吗**','**悄悄牵住你的手**','**风把你带来我身边**','**今晚月色真美**'**]** },  
    { name: '**心动**', cards: **[**'**你是我最温柔的梦**','**想听你再多说一点**','**只对你一个人这样**','**你笑起来像星星**'**]** }  
  **]**;  
  function loadGroups() {  
    try {  
      const s = localStorage.getItem('dream_groups_v5');  
      if (s) { const p = JSON.parse(s); if (Array.isArray(p) && p.length) groups = p; }  
    } catch(e) {}  
  }  
  function persistGroups() {  
    try { localStorage.setItem('dream_groups_v5', JSON.stringify(groups)); } catch(e) {}  
  }  
  function getAllCards() { return groups.flatMap(g => g.cards); }  
  function randomCard() {  
    const all = getAllCards();  
    if (!all.length) return '……**（字卡库空了，从** ⋮ → **字卡库添加一些吧）**';  
    return all**[**Math.floor(Math.random() * all.length)**]**;  
  }  
  
  let avatarDreamer = '', avatarUser = '';  
  function loadAvatars() {  
    try {  
      avatarDreamer = localStorage.getItem('dream_avatar_dreamer') || '';  
      avatarUser = localStorage.getItem('dream_avatar_user') || '';  
    } catch(e) {}  
  }  
  function persistAvatars() {  
    try {  
      localStorage.setItem('dream_avatar_dreamer', avatarDreamer);  
      localStorage.setItem('dream_avatar_user', avatarUser);  
    } catch(e) {}  
  }  
  
  let dreamerName = '**梦角**';  
  function loadName() {  
    try { const n = localStorage.getItem('dream_name'); if (n && n.trim()) dreamerName = n; } catch(e) {}  
  }  
  function persistName() { try { localStorage.setItem('dream_name', dreamerName); } catch(e) {} }  
  
  let statusList = **[**'**在线**', '**在想你**', '**发呆中**', '**有点困**', '**在看书**', '**刚睡醒**'**]**;  
  let currentStatus = '**在线**';  
  function loadStatusList() {  
    try {  
      const l = localStorage.getItem('dream_status_list');  
      if (l) { const p = JSON.parse(l); if (Array.isArray(p) && p.length) statusList = p; }  
      const c = localStorage.getItem('dream_status_current');  
      if (c) currentStatus = c;  
    } catch(e) {}  
    if (!statusList.length) statusList = **[**'**在线**'**]**;  
    if (!currentStatus) currentStatus = statusList**[**0**]**;  
  }  
  function persistStatusList() {  
    try {  
      localStorage.setItem('dream_status_list', JSON.stringify(statusList));  
      localStorage.setItem('dream_status_current', currentStatus);  
    } catch(e) {}  
  }  
  function rollStatus() {  
    if (!statusList.length) return;  
    if (statusList.length === 1) { currentStatus = statusList**[**0**]**; refreshTitle(); return; }  
    let next;  
    do { next = statusList**[**Math.floor(Math.random() * statusList.length)**]**; }  
    while (next === currentStatus);  
    currentStatus = next;  
    refreshTitle();  
    persistStatusList();  
  }  
  
  let replySettings = { delayMin: 400, delayMax: 1500, countMin: 1, countMax: 3, betweenDelay: 500 };  
  function loadReplySettings() {  
    try {  
      const s = localStorage.getItem('dream_reply_settings');  
      if (s) { const p = JSON.parse(s); if (p && typeof p === 'object') Object.assign(replySettings, p); }  
    } catch(e) {}  
  }  
  function persistReplySettings() {  
    try { localStorage.setItem('dream_reply_settings', JSON.stringify(replySettings)); } catch(e) {}  
  }  
  
  // ---------------- DOM ----------------  
  const messagesEl = $('messages'), msgInput = $('msgInput'), sendBtn = $('sendBtn');  
  const headerAvatar = $('headerAvatar'), titleName = $('titleName'), titleStatus = $('titleStatus');  
  const fileDreamer = $('fileDreamer'), fileUser = $('fileUser');  
  const menuBtn = $('menuBtn'), dropdown = $('dropdown');  
  const changeDreamerAvatar = $('changeDreamerAvatar');  
  const changeUserAvatar = $('changeUserAvatar');  
  const changeDreamerName = $('changeDreamerName');  
  const manageStatus = $('manageStatus');  
  const replySettingsBtn = $('replySettings');  
  const editCardsBtn = $('editCards');  
  const clearChatBtn = $('clearChat');  
  
  const cardsOverlay = $('cardsOverlay');  
  const groupListView = $('groupListView');  
  const groupEditView = $('groupEditView');  
  const groupList = $('groupList');  
  const addGroupBtn = $('addGroupBtn');  
  const cardsCloseBtn = $('cardsCloseBtn');  
  const backToGroups = $('backToGroups');  
  const editGroupName = $('editGroupName');  
  const renameGroupBtn = $('renameGroupBtn');  
  const groupCardsInput = $('groupCardsInput');  
  const groupCancelBtn = $('groupCancelBtn');  
  const groupSaveBtn = $('groupSaveBtn');  
  
  const nameOverlay = $('nameOverlay'), nameInput = $('nameInput');  
  const nameCancelBtn = $('nameCancelBtn'), nameSaveBtn = $('nameSaveBtn');  
  
  const statusOverlay = $('statusOverlay'), statusInput = $('statusInput');  
  const statusCancelBtn = $('statusCancelBtn'), statusSaveBtn = $('statusSaveBtn');  
  
  const replyOverlay = $('replyOverlay');  
  const delayMinEl = $('delayMin'), delayMaxEl = $('delayMax');  
  const countMinEl = $('countMin'), countMaxEl = $('countMax');  
  const betweenDelayEl = $('betweenDelay');  
  const replyCancelBtn = $('replyCancelBtn'), replySaveBtn = $('replySaveBtn');  
  
  // ---------------- **渲染** ----------------  
  function renderAvatar(el, dataUrl, fallback) {  
    el.innerHTML = '';  
    if (dataUrl) {  
      const img = document.createElement('img');  
      img.src = dataUrl; img.alt = '';  
      el.appendChild(img);  
    } else {  
      el.textContent = fallback;  
    }  
  }  
  function refreshHeaderAvatar() { renderAvatar(headerAvatar, avatarDreamer, '✦'); }  
  function refreshTitle() {  
    titleName.textContent = dreamerName;  
    titleStatus.textContent = currentStatus || '';  
    titleStatus.style.display = currentStatus ? '' : 'none';  
    document.title = dreamerName + ' **·** **字卡传讯**';  
  }  
  
  function appendMessage(text, isUser) {  
    const row = document.createElement('div');  
    row.className = 'msg-row ' + (isUser ? 'user' : 'dreamer');  
  
    const av = document.createElement('div');  
    av.className = 'avatar ' + (isUser ? 'user-avatar' : 'dreamer-avatar');  
    renderAvatar(av, isUser ? avatarUser : avatarDreamer, isUser ? '**我**' : '✦');  
  
    const bubble = document.createElement('div');  
    bubble.className = 'bubble ' + (isUser ? 'user' : 'dreamer');  
    bubble.textContent = text;  
  
    row.appendChild(av);  
    row.appendChild(bubble);  
    messagesEl.appendChild(row);  
    messagesEl.scrollTop = messagesEl.scrollHeight;  
  }  
  
  // ---------------- **发送** / **回复** ----------------  
  let replying = false;  
  
  function send() {  
    const text = msgInput.value.trim();  
    if (!text) return;  
    if (replying) return;  
    appendMessage(text, true);  
    msgInput.value = '';  
    msgInput.focus();  
    scheduleReply();  
  }  
  
  function scheduleReply() {  
    replying = true;  
    let count = randBetween(replySettings.countMin, replySettings.countMax);  
    if (count < 1) count = 1;  
    const firstDelay = randBetween(replySettings.delayMin, replySettings.delayMax);  
  
    let i = 0;  
    function replyOnce() {  
      try {  
        if (i >= count) return;  
        const all = getAllCards();  
        if (!all.length) {  
          appendMessage('……**（字卡库空了，从** ⋮ → **字卡库添加一些吧）**', false);  
          return;  
        }  
        appendMessage(randomCard(), false);  
        rollStatus();  
        i++;  
        if (i < count) {  
          const gap = randBetween(replySettings.betweenDelay, replySettings.betweenDelay + 300);  
          setTimeout(replyOnce, gap);  
        }  
      } catch (err) {  
        console.error('**回复出错：**', err);  
      } finally {  
        if (i >= count) replying = false;  
      }  
    }  
    setTimeout(replyOnce, firstDelay);  
  }  
  
  // ---------------- **字卡库** ----------------  
  let editingGroupIndex = -1;  
  
  function openCardsModal() { showGroupList(); cardsOverlay.classList.add('active'); }  
  function closeCardsModal() { cardsOverlay.classList.remove('active'); }  
  
  function showGroupList() {  
    groupListView.style.display = '';  
    groupEditView.style.display = 'none';  
    renderGroupList();  
  }  
  function showGroupEdit(index) {  
    editingGroupIndex = index;  
    groupListView.style.display = 'none';  
    groupEditView.style.display = '';  
    editGroupName.textContent = groups**[**index**]**.name;  
    groupCardsInput.value = groups**[**index**]**.cards.join('\n');  
    setTimeout(function() { groupCardsInput.focus(); }, 100);  
  }  
  
  function renderGroupList() {  
    groupList.innerHTML = '';  
    if (!groups.length) {  
      const empty = document.createElement('div');  
      empty.style.cssText = 'text-align:center;color:#bbb;font-size:13px;padding:24px 0';  
      empty.textContent = '**还没有分组，点下面新建一个吧**';  
      groupList.appendChild(empty);  
      return;  
    }  
    groups.forEach(function(g, i) {  
      const card = document.createElement('div');  
      card.className = 'group-card';  
  
      const left = document.createElement('div');  
      left.className = 'group-card-left';  
      const nameEl = document.createElement('div');  
      nameEl.className = 'group-card-name';  
      nameEl.textContent = g.name;  
      const countEl = document.createElement('div');  
      countEl.className = 'group-card-count';  
      countEl.textContent = g.cards.length + ' **张字卡**';  
      left.appendChild(nameEl);  
      left.appendChild(countEl);  
  
      const right = document.createElement('div');  
      right.className = 'group-card-right';  
  
      const renameBtn = document.createElement('button');  
      renameBtn.className = 'icon-btn';  
      renameBtn.title = '**重命名**';  
      renameBtn.textContent = '✎';  
      renameBtn.addEventListener('click', function(e) {  
        e.stopPropagation();  
        const newName = prompt('**重命名分组：**', g.name);  
        if (newName === null) return;  
        const trimmed = newName.trim();  
        if (!trimmed) { alert('**分组名不能为空**'); return; }  
        groups**[**i**]**.name = trimmed;  
        persistGroups();  
        renderGroupList();  
      });  
  
      const delBtn = document.createElement('button');  
      delBtn.className = 'icon-btn danger';  
      delBtn.title = '**删除分组**';  
      delBtn.textContent = '🗑';  
      delBtn.addEventListener('click', function(e) {  
        e.stopPropagation();  
        if (!confirm('**删除分组「**' + g.name + '**」？里面的字卡也会一起删掉。**')) return;  
        groups.splice(i, 1);  
        persistGroups();  
        renderGroupList();  
      });  
  
      right.appendChild(renameBtn);  
      right.appendChild(delBtn);  
  
      card.appendChild(left);  
      card.appendChild(right);  
      card.addEventListener('click', function() { showGroupEdit(i); });  
      groupList.appendChild(card);  
    });  
  }  
  
  addGroupBtn.addEventListener('click', function() {  
    const name = prompt('**新分组名字：**', '');  
    if (name === null) return;  
    const trimmed = name.trim();  
    if (!trimmed) { alert('**分组名不能为空**'); return; }  
    groups.push({ name: trimmed, cards: **[]** });  
    persistGroups();  
    renderGroupList();  
    showGroupEdit(groups.length - 1);  
  });  
  
  backToGroups.addEventListener('click', function() {  
    if (editingGroupIndex >= 0) {  
      const lines = groupCardsInput.value.split(/\r?\n/).map(l => l.trim()).filter(l => l !== '');  
      groups**[**editingGroupIndex**]**.cards = lines;  
      persistGroups();  
    }  
    showGroupList();  
  });  
  
  renameGroupBtn.addEventListener('click', function() {  
    if (editingGroupIndex < 0) return;  
    const cur = groups**[**editingGroupIndex**]**.name;  
    const newName = prompt('**重命名分组：**', cur);  
    if (newName === null) return;  
    const trimmed = newName.trim();  
    if (!trimmed) { alert('**分组名不能为空**'); return; }  
    groups**[**editingGroupIndex**]**.name = trimmed;  
    editGroupName.textContent = trimmed;  
    persistGroups();  
  });  
  
  groupCancelBtn.addEventListener('click', function() { showGroupList(); });  
  
  groupSaveBtn.addEventListener('click', function() {  
    if (editingGroupIndex < 0) return;  
    const lines = groupCardsInput.value.split(/\r?\n/).map(l => l.trim()).filter(l => l !== '');  
    groups**[**editingGroupIndex**]**.cards = lines;  
    persistGroups();  
    showGroupList();  
  });  
  
  cardsCloseBtn.addEventListener('click', closeCardsModal);  
  cardsOverlay.addEventListener('click', function(e) { if (e.target === cardsOverlay) closeCardsModal(); });  
  
  // ---------------- **昵称** ----------------  
  function openNameModal() {  
    nameInput.value = dreamerName;  
    nameOverlay.classList.add('active');  
    setTimeout(function() { nameInput.focus(); nameInput.select(); }, 120);  
  }  
  function closeNameModal() { nameOverlay.classList.remove('active'); }  
  function saveNameModal() {  
    const v = nameInput.value.trim();  
    if (!v) { alert('**昵称不能为空**'); return; }  
    dreamerName = v;  
    persistName();  
    refreshTitle();  
    closeNameModal();  
  }  
  
  // ---------------- **状态** ----------------  
  function openStatusModal() {  
    statusInput.value = statusList.join('\n');  
    statusOverlay.classList.add('active');  
    setTimeout(function() { statusInput.focus(); }, 120);  
  }  
  function closeStatusModal() { statusOverlay.classList.remove('active'); }  
  function saveStatusModal() {  
    const lines = statusInput.value.split(/\r?\n/).map(l => l.trim()).filter(l => l !== '');  
    if (!lines.length) { alert('**至少保留一个状态**'); return; }  
    statusList = lines;  
    if (statusList.indexOf(currentStatus) === -1) {  
      currentStatus = statusList**[**Math.floor(Math.random() * statusList.length)**]**;  
    }  
    persistStatusList();  
    refreshTitle();  
    closeStatusModal();  
  }  
  
  // ---------------- **回复设置** ----------------  
  function openReplyModal() {  
    delayMinEl.value = replySettings.delayMin;  
    delayMaxEl.value = replySettings.delayMax;  
    countMinEl.value = replySettings.countMin;  
    countMaxEl.value = replySettings.countMax;  
    betweenDelayEl.value = replySettings.betweenDelay;  
    replyOverlay.classList.add('active');  
  }  
  function closeReplyModal() { replyOverlay.classList.remove('active'); }  
  function saveReplyModal() {  
    let dm = parseInt(delayMinEl.value, 10); if (isNaN(dm) || dm < 0) dm = 0;  
    let dM = parseInt(delayMaxEl.value, 10); if (isNaN(dM) || dM < 0) dM = 0;  
    let cm = parseInt(countMinEl.value, 10); if (isNaN(cm) || cm < 1) cm = 1;  
    let cM = parseInt(countMaxEl.value, 10); if (isNaN(cM) || cM < 1) cM = 1;  
    let bd = parseInt(betweenDelayEl.value, 10); if (isNaN(bd) || bd < 0) bd = 0;  
    if (dm > dM) { const t = dm; dm = dM; dM = t; }  
    if (cm > cM) { const t = cm; cm = cM; cM = t; }  
    replySettings = { delayMin: dm, delayMax: dM, countMin: cm, countMax: cM, betweenDelay: bd };  
    persistReplySettings();  
    closeReplyModal();  
  }  
  
  // ---------------- **头像上传** ----------------  
  function handleFile(input, isUser) {  
    const file = input.files && input.files**[**0**]**;  
    if (!file) return;  
    if (!file.type.startsWith('image/')) { alert('**请选择图片文件**'); input.value = ''; return; }  
    const reader = new FileReader();  
    reader.onload = function(e) {  
      const dataUrl = e.target.result;  
      if (isUser) { avatarUser = dataUrl; persistAvatars(); }  
      else { avatarDreamer = dataUrl; persistAvatars(); refreshHeaderAvatar(); }  
    };  
    reader.readAsDataURL(file);  
    input.value = '';  
  }  
  
  fileDreamer.addEventListener('change', function() { handleFile(fileDreamer, false); });  
  fileUser.addEventListener('change', function() { handleFile(fileUser, true); });  
  headerAvatar.addEventListener('click', function() { fileDreamer.click(); });  
  
  // ---------------- **下拉菜单（用** document **监听关菜单，不遮挡）** ----------------  
  function openDropdown() { dropdown.classList.add('active'); }  
  function closeDropdown() { dropdown.classList.remove('active'); }  
  
  menuBtn.addEventListener('click', function(e) {  
    e.stopPropagation();  
    if (dropdown.classList.contains('active')) closeDropdown();  
    else openDropdown();  
  });  
  
  // **点任何地方都先关菜单（但下拉菜单和菜单按钮的点击会** stopPropagation**，不受影响）**  
  document.addEventListener('click', function() {  
    closeDropdown();  
  });  
  
  // **让下拉菜单内部的点击不冒泡到** document**，从而不会被自己关掉**  
  dropdown.addEventListener('click', function(e) {  
    e.stopPropagation();  
  });  
  
  changeDreamerAvatar.addEventListener('click', function() { closeDropdown(); fileDreamer.click(); });  
  changeUserAvatar.addEventListener('click', function() { closeDropdown(); fileUser.click(); });  
  changeDreamerName.addEventListener('click', function() { closeDropdown(); openNameModal(); });  
  manageStatus.addEventListener('click', function() { closeDropdown(); openStatusModal(); });  
  replySettingsBtn.addEventListener('click', function() { closeDropdown(); openReplyModal(); });  
  editCardsBtn.addEventListener('click', function() { closeDropdown(); openCardsModal(); });  
  clearChatBtn.addEventListener('click', function() {  
    closeDropdown();  
    if (!confirm('**清空全部聊天记录？此操作不可恢复。**')) return;  
    messagesEl.innerHTML = '';  
    replying = false;  
  });  
  
  // ---------------- **通用事件** ----------------  
  sendBtn.addEventListener('click', send);  
  msgInput.addEventListener('keydown', function(e) {  
    if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); send(); }  
  });  
  
  nameCancelBtn.addEventListener('click', closeNameModal);  
  nameSaveBtn.addEventListener('click', saveNameModal);  
  nameOverlay.addEventListener('click', function(e) { if (e.target === nameOverlay) closeNameModal(); });  
  nameInput.addEventListener('keydown', function(e) {  
    if (e.key === 'Enter') { e.preventDefault(); saveNameModal(); }  
  });  
  
  statusCancelBtn.addEventListener('click', closeStatusModal);  
  statusSaveBtn.addEventListener('click', saveStatusModal);  
  statusOverlay.addEventListener('click', function(e) { if (e.target === statusOverlay) closeStatusModal(); });  
  
  replyCancelBtn.addEventListener('click', closeReplyModal);  
  replySaveBtn.addEventListener('click', saveReplyModal);  
  replyOverlay.addEventListener('click', function(e) { if (e.target === replyOverlay) closeReplyModal(); });  
  
  document.addEventListener('keydown', function(e) {  
    if (e.key === 'Escape') {  
      if (cardsOverlay.classList.contains('active')) {  
        if (groupEditView.style.display !== 'none') showGroupList();  
        else closeCardsModal();  
      }  
      else if (nameOverlay.classList.contains('active')) closeNameModal();  
      else if (statusOverlay.classList.contains('active')) closeStatusModal();  
      else if (replyOverlay.classList.contains('active')) closeReplyModal();  
      else closeDropdown();  
    }  
  });  
  
  // ---------------- **初始化** ----------------  
  loadGroups();  
  loadAvatars();  
  loadName();  
  loadStatusList();  
  loadReplySettings();  
  refreshHeaderAvatar();  
  refreshTitle();  
  window.addEventListener('load', function() { msgInput.focus(); });  
})();  
</script>  
</body>  
</html>  
