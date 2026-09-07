<script>
  import { fade } from 'svelte/transition'
  let show = false

  function copy(tex: stringt) {
    navigator.clipboard.writeText(text)
  }
</script>

<div class="contact-float">
  <button class="contact-btn" on:click={() => show = !show}>
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>
    </svg>
    入驻联系我们
  </button>

  {#if show}
    <div class="contact-panel" transition:fade={{ duration: 200 }}>
      <div class="panel-title">入驻联系我们</div>
      <div class="contact-item">
        <span class="label">微信</span>
        <span class="value">mfujun</span>
        <button class="copy" on:click={() => copy('mfujun')}>复制</button>
      </div>
      <div class="contact-item">
        <span class="label">QQ</span>
        <span class="value">548827878</span>
        <button class="copy" on:click={() => copy('548827878')}>复制</button>
      </div>
      <div class="contact-item">
        <span class="label">邮箱</span>
        <span class="value">548827878@qq.com</span>
        <button class="copy" on:click={() => copy('548827878@qq.com')}>复制</button>
      </div>
    </div>
  {/if}
</div>

<style>
  .contact-float {
    position: fixed;
    right: 20px;
    bottom: 30px;
    z-index: 9999;
  }
  .contact-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 10px 18px;
    border: none;
    border-radius: 24px;
    background: #2563eb;
    color: #fff;
    font-size: 14px;
    cursor: pointer;
    box-shadow: 0 4px 14px rgba(37, 99, 235, 0.4);
    transition: transform 0.2s;
  }
  .contact-btn:hover { transform: translateY(-2px); }
  .contact-panel {
    position: absolute;
    right: 0;
    bottom: 56px;
    width: 300px;
    background: #1e293b;
    border: 1px solid #334155;
    border-radius: 12px;
    padding: 14px;
    box-shadow: 0 8px 30px rgba(0,0,0,0.3);
  }
  .panel-title {
    font-size: 14px;
    font-weight: 600;
    color: #f1f5f9;
    margin-bottom: 10px;
    padding-bottom: 8px;
    border-bottom: 1px solid #334155;
  }
  .contact-item {
    display: flex;
    align-items: center;
    padding: 8px 6px;
    border-bottom: 1px solid #334155;
  }
  .contact-item:last-child { border-bottom: none; }
  .label { width: 40px; color: #94a3b8; font-size: 13px; }
  .value { flex: 1; color: #e2e8f0; font-size: 13px; }
  .copy {
    padding: 3px 10px;
    border: 1px solid #475569;
    border-radius: 6px;
    background: transparent;
    color: #94a3b8;
    font-size: 12px;
    cursor: pointer;
  }
  .copy:hover { color: #fff; border-color: #2563eb; }
</style>
