<script setup lang="ts"></script>

<template>
  <div id="app">
    <main>
      <router-view />
    </main>
  </div>
</template>

<style>
:root {
  --primary-1: #E6498C;
  --primary-2: #E66D49;
  --primary-3: #E65149;
  --primary-4: #E349E6;
  --primary-5: #E68849;
  --primary-6: #E69A96;
  --surface: #ffffff;
  --surface-soft: #fff6fb;
  --surface-muted: #fff0f8;
  --text: #1e162b;
  --text-muted: #625f72;
  --border: rgba(230, 73, 140, 0.18);
  --shadow: 0 24px 60px rgba(230, 73, 140, 0.12);
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  min-height: 100vh;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  color: var(--text);
  background: radial-gradient(circle at top left, rgba(230, 73, 140, 0.14) 0%, transparent 24%),
    radial-gradient(circle at top right, rgba(230, 77, 230, 0.1) 0%, transparent 18%),
    linear-gradient(180deg, #ffffff 0%, #fcf3fb 100%);
}

#app {
  min-height: 100vh;
  background: transparent;
}

main {
  min-height: 100vh;
}

* {
  box-sizing: border-box;
}

button {
  font: inherit;
}

a {
  color: inherit;
  text-decoration: none;
}
</style>
