<script setup>
const currentYear = new Date().getFullYear()
</script>

<template>
  <footer class="site-footer">
    <div class="footer-content">
      <nav class="footer-links" aria-label="footer links">
        <a href="#" @click.prevent>使用條款</a>
        <span>|</span>
        <a href="#" @click.prevent>隱私權政策</a>
      </nav>

      <p class="symbol">へへへ</p>
      <p class="brand">CELSYS</p>
      <p class="copyright">© {{ currentYear }} CELSYS, Inc.</p>
    </div>
  </footer>
</template>

<style scoped>
.site-footer {
  margin-top: auto;
  padding: 90px 0 10px;
}

.footer-content {
  width: min(360px, 100%);
  margin: 0 auto;
  display: grid;
  justify-items: center;
  gap: 7px;
}

.footer-links {
  margin: 0;
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 11px;
}

.footer-links span {
  color: #afafaf;
}

.footer-links a {
  color: #5fa5e8;
  font-size: 11px;
  text-decoration: none;
}

.symbol {
  margin: 4px 0 0;
  color: #424242;
  letter-spacing: 2px;
}

.brand {
  margin: 0;
  color: #3c434a;
  font-size: 24px;
  letter-spacing: 1px;
}

.copyright {
  margin: 0;
  color: #a0a5ab;
  font-size: 12px;
}

@media (max-width: 480px) {
  .footer-content {
    width: calc(100% - 24px);
  }
}
</style>