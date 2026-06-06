<script setup>
import { onBeforeUnmount, watch } from 'vue'

const props = defineProps({
  visible: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: '系統通知'
  },
  message: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['close'])

function closeModal() {
  emit('close')
}

function onKeydown(event) {
  if (event.key === 'Escape' && props.visible) {
    closeModal()
  }
}

// 顯示 Modal 時鎖定頁面滾動，避免背景誤操作。
watch(
  () => props.visible,
  (isVisible) => {
    document.body.style.overflow = isVisible ? 'hidden' : ''
  }
)

window.addEventListener('keydown', onKeydown)

onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKeydown)
  document.body.style.overflow = ''
})
</script>

<template>
  <transition name="modal-fade">
    <div v-if="visible" class="overlay" @click.self="closeModal">
      <div class="modal-card" role="dialog" aria-modal="true" aria-label="status modal">
        <div class="modal-header">
          <h3>{{ title }}</h3>
          <button type="button" class="icon-btn" @click="closeModal" aria-label="close modal">X</button>
        </div>

        <p class="modal-message">{{ message }}</p>

        <button type="button" class="confirm-btn" @click="closeModal">關閉</button>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.32);
  display: grid;
  place-items: center;
  padding: 20px;
  z-index: 1000;
}

.modal-card {
  width: min(420px, 100%);
  background: #ffffff;
  border-radius: 6px;
  border: 1px solid #dddddd;
  box-shadow: 0 16px 32px rgba(0, 0, 0, 0.16);
  padding: 16px;
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

h3 {
  margin: 0;
  font-size: 18px;
  color: #2b2f34;
}

.icon-btn {
  border: 1px solid #d7d7d7;
  background: #fff;
  color: #7d8187;
  border-radius: 4px;
  width: 28px;
  height: 28px;
  cursor: pointer;
  font-size: 12px;
}

.modal-message {
  margin: 14px 0 16px;
  line-height: 1.6;
  color: #525860;
  font-size: 14px;
}

.confirm-btn {
  width: 100%;
  border: none;
  border-radius: 4px;
  height: 36px;
  font-weight: 700;
  color: #fff;
  background: #3d97ea;
  cursor: pointer;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.25s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>