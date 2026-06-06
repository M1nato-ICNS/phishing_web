<script setup>
import { ref } from 'vue'

const emit = defineEmits(['submit-login'])

const account = ref('')
const password = ref('')
const accountError = ref('')
const passwordError = ref('')
const isSubmitting = ref(false)

function validateForm() {
  accountError.value = ''
  passwordError.value = ''

  // 帳號支援 email 或英數底線組成的員編格式。
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  const employeeIdPattern = /^[a-zA-Z0-9_]{4,20}$/

  if (!account.value.trim()) {
    accountError.value = '請輸入帳號。'
  } else if (!emailPattern.test(account.value) && !employeeIdPattern.test(account.value)) {
    accountError.value = '帳號格式不正確，請輸入 Email 或 4-20 碼英數字。'
  }

  // 密碼只做格式檢核，實際安全驗證仍應由後端負責。
  if (!password.value) {
    passwordError.value = '請輸入密碼。'
  } else if (password.value.length < 8) {
    passwordError.value = '密碼長度需至少 8 碼。'
  }

  return !accountError.value && !passwordError.value
}

async function handleSubmit() {
  if (!validateForm()) {
    return
  }

  isSubmitting.value = true

  const payload = {
    account: account.value.trim(),
    passwordLength: password.value.length,
    submittedAt: new Date().toISOString()
  }

  // 模擬 API 請求延遲，並以 console.log 呈現送出行為。
  console.log('[LoginForm] 模擬 API 送出:', payload)
  await new Promise((resolve) => setTimeout(resolve, 800))

  emit('submit-login', payload)
  isSubmitting.value = false
}
</script>

<template>
  <div class="login-form-card">
    <h2>登入CLIP STUDIO</h2>
    <p class="subtitle">請輸入持有的 CLIP STUDIO 帳號資訊。</p>

    <form class="form-grid" @submit.prevent="handleSubmit" novalidate>
      <label class="field">
        <span>電子郵件地址</span>
        <input
          v-model="account"
          type="text"
          name="account"
          autocomplete="username"
          placeholder=""
        >
        <small v-if="accountError" class="error">{{ accountError }}</small>
      </label>

      <label class="field">
        <span>密碼</span>
        <input
          v-model="password"
          type="password"
          name="password"
          autocomplete="current-password"
          placeholder="請輸入至少 8 碼密碼"
        >
        <small v-if="passwordError" class="error">{{ passwordError }}</small>
      </label>

      <a class="forgot-link" href="#" @click.prevent>忘記密碼的帳戶</a>

      <button class="submit-btn" type="submit" :disabled="isSubmitting">
        {{ isSubmitting ? '登入中...' : '登入' }}
      </button>
    </form>

    <div class="separator" aria-hidden="true">
      <span></span>
      <em>或</em>
      <span></span>
    </div>

    <button type="button" class="social-btn" @click="console.log('[LoginForm] Apple login clicked')">
      <span class="social-icon apple"></span>
      以Apple繼續
    </button>

    <button type="button" class="social-btn" @click="console.log('[LoginForm] Google login clicked')">
      <span class="social-icon google">G</span>
      以Google繼續
    </button>

    <div class="separator register-divider" aria-hidden="true"><span></span></div>

    <p class="register-text">初次的用戶請由此</p>
    <button type="button" class="register-btn" @click="console.log('[LoginForm] Register clicked')">註冊帳號</button>
  </div>
</template>

<style scoped>
.login-form-card {
  width: 100%;
  max-width: 360px;
  min-height: 560px;
}

h2 {
  margin: 16px 0 14px;
  font-size: 35px;
  font-weight: 500;
  color: #31363d;
}

.subtitle {
  margin: 0 0 16px;
  color: #5f656d;
  line-height: 1.5;
  font-size: 12px;
}

.form-grid {
  display: grid;
  gap: 12px;
}

.field {
  display: grid;
  gap: 6px;
}

.field span {
  font-size: 11px;
  color: #5e656f;
}

input {
  border: 1px solid #dbdbdb;
  border-radius: 4px;
  height: 40px;
  padding: 0 10px;
  font-size: 14px;
  background: #fff;
  transition: border-color 0.2s, box-shadow 0.2s;
}

input:focus {
  outline: none;
  border-color: #6bb2f0;
  box-shadow: 0 0 0 2px rgba(63, 152, 227, 0.12);
}

.error {
  color: #c0392b;
  font-size: 11px;
}

.forgot-link {
  text-align: right;
  font-size: 11px;
  color: #a2a7ae;
  text-decoration: none;
}

.submit-btn {
  margin-top: 0;
  border: none;
  border-radius: 4px;
  height: 40px;
  font-size: 14px;
  font-weight: 700;
  color: #fff;
  background: #3d97ea;
  cursor: pointer;
  transition: opacity 0.2s ease;
}

.submit-btn:hover:enabled {
  opacity: 0.92;
}

.submit-btn:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

.separator {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 18px 0 10px;
}

.separator span {
  flex: 1;
  border-top: 1px solid #dfdfdf;
}

.separator em {
  font-style: normal;
  color: #8a9096;
  font-size: 11px;
}

.social-btn {
  width: 100%;
  height: 32px;
  border: 1px solid #d8d8d8;
  background: #fff;
  border-radius: 4px;
  margin-top: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 7px;
  color: #474c52;
  font-size: 14px;
  cursor: pointer;
}

.social-icon {
  display: inline-flex;
  width: 16px;
  justify-content: center;
  font-size: 16px;
  line-height: 1;
}

.social-icon.apple {
  color: #121212;
}

.social-icon.google {
  color: #ea4335;
  font-weight: 700;
  font-size: 15px;
}

.register-divider {
  margin-top: 16px;
  margin-bottom: 10px;
}

.register-text {
  margin: 0 0 8px;
  text-align: center;
  color: #555b62;
  font-size: 14px;
}

.register-btn {
  width: 100%;
  height: 32px;
  border: 1px solid #91c4f4;
  border-radius: 4px;
  color: #2f8ddd;
  background: #fff;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
}

@media (max-width: 480px) {
  h2 {
    font-size: 31px;
  }
}
</style>