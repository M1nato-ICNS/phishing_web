<script setup>
import { ref } from 'vue'

const languages = ['繁體中文', 'English']
const activeLanguage = ref(languages[0])

function switchLanguage(language) {
  activeLanguage.value = language
  console.log('[Navbar] 切換語系:', language)
}
</script>

<template>
  <header class="navbar">
    <div class="brand-title">
      <span class="link-icon">🔗</span>
      <span>CLIP STUDIO ACCOUNT</span>
    </div>

    <div class="lang-switch" aria-label="language switcher">
      <button type="button" class="lang-btn" @click="switchLanguage(activeLanguage === languages[0] ? languages[1] : languages[0])">
        {{ activeLanguage }} ▼
      </button>
    </div>
  </header>
</template>

<style scoped>
.navbar {
  width: min(800px, 100%);
  margin: 6px auto 0;
  padding: 8px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
}

.brand-title {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.3px;
}

.lang-switch {
  margin-left: auto;
}

.lang-btn {
  border: 0;
  background: transparent;
  padding: 2px 0;
  cursor: pointer;
  color: #2a84d2;
  font-size: 11px;
}

@media (max-width: 640px) {
  .navbar {
    width: calc(100% - 24px);
  }
}
</style>