<script lang="ts">
  import { fade } from 'svelte/transition'

  let show = false
  let submitting = false
  let message = ''

  const form = {
    company: '',
    website: '',
    contact: '',
    phone: ''
  }

  function resetForm(): void {
    form.company = ''
    form.website = ''
    form.contact = ''
    form.phone = ''
    message = ''
  }

  function closeModal(): void {
    show = false
    resetForm()
  }

  async function submitForm(): Promise<void> {
    if (!form.company || !form.website || !form.contact || !form.phone) {
      message = '请填写公司名称、网址、联系人和联系电话'
      return
    }

    submitting = true
    message = ''

    try {
      const content = '公司名称：' + form.company + '\n网址：' + (form.website || '未填写') + '\n联系人：' + form.contact + '\n联系电话：' + form.phone

      const response = await fetch('https://www.pushplus.plus/send', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          token: '37af69b210224201a5c4ff5a973d36c1',
          title: '新客户入驻申请',
          content: content,
          template: 'txt'
        })
      })

      const data = await response.json() as { code: number }

      if (data.code === 200) {
        message = '提交成功，我们会尽快联系您！'
        setTimeout(() => {
          closeModal()
        }, 2000)
      } else {
        message = '提交失败，请稍后重试'
      }
    } catch (error) {
      message = '网络错误，请稍后重试'
    } finally {
      submitting = false
    }
  }
</script>

<div class="contact-float">
  <button class="contact-btn" on:click={() => show = !show}>
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>
    </svg>
    入驻联系我们
  </button>
</div>

{#if show}
  <div class="modal-overlay" on:click={closeModal}>
    <div class="modal-content" transition:fade on:click|stopPropagation>
      <div class="modal-header">
        <h3>入驻申请</h3>
        <button class="close-btn" on:click={closeModal}>×</button>
      </div>
      <form on:submit|preventDefault={submitForm}>
        <div class="form-group">
          <label for="company">公司名称 <span class="required">*</span></label>
          <input id="company" type="text" bind:value={form.company} placeholder="请输入公司名称" />
        </div>
        <div class="form-group">
          <label for="website">网址 <span class="required">*</span></label>
          <input id="website" type="text" bind:value={form.website} placeholder="请输入公司网址" />
        </div>
        <div class="form-group">
          <label for="contact">联系人 <span class="required">*</span></label>
          <input id="contact" type="text" bind:value={form.contact} placeholder="请输入联系人姓名" />
        </div>
        <div class="form-group">
          <label for="phone">联系电话 <span class="required">*</span></label>
          <input id="phone" type="tel" bind:value={form.phone} placeholder="请输入联系电话" />
        </div>
        {#if message}
          <div class="form-message">{message}</div>
        {/if}
        <button type="submit" class="submit-btn" disabled={submitting}>
          {submitting ? '提交中...' : '提交申请'}
        </button>
      </form>
    </div>
  </div>
{/if}

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
  }
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10000;
  }
  .modal-content {
    background: #1e293b;
    border: 1px solid #334155;
    border-radius: 12px;
    padding: 24px;
    width: 90%;
    max-width: 420px;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
  }
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  .modal-header h3 {
    margin: 0;
    color: #f1f5f9;
    font-size: 18px;
  }
  .close-btn {
    background: none;
    border: none;
    color: #94a3b8;
    font-size: 24px;
    cursor: pointer;
    padding: 0;
    line-height: 1;
  }
  .close-btn:hover {
    color: #fff;
  }
  .form-group {
    margin-bottom: 16px;
  }
  .form-group label {
    display: block;
    color: #94a3b8;
    font-size: 13px;
    margin-bottom: 6px;
  }
  .required {
    color: #ef4444;
  }
  .form-group input {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #334155;
    border-radius: 8px;
    background: #0f172a;
    color: #e2e8f0;
    font-size: 14px;
    box-sizing: border-box;
  }
  .form-group input:focus {
    outline: none;
    border-color: #2563eb;
  }
  .form-message {
    padding: 10px 12px;
    border-radius: 8px;
    font-size: 13px;
    margin-bottom: 16px;
    background: #1e3a5f;
    color: #93c5fd;
  }
  .submit-btn {
    width: 100%;
    padding: 12px;
    border: none;
    border-radius: 8px;
    background: #2563eb;
    color: #fff;
    font-size: 15px;
    cursor: pointer;
  }
  .submit-btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
</style>
