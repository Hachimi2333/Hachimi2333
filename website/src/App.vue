<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import { MoonIcon, SunIcon } from '@lucide/vue'
import { Button } from '@/components/ui/button'

type Theme = 'light' | 'dark'
const theme = ref<Theme>('light')
let preference: MediaQueryList | undefined
let hasSavedPreference = false

function applyTheme(value: Theme) {
  theme.value = value
  document.documentElement.dataset.theme = value
}

function onSystemThemeChange(event: MediaQueryListEvent) {
  if (!hasSavedPreference) applyTheme(event.matches ? 'dark' : 'light')
}

function toggleTheme() {
  const next = theme.value === 'dark' ? 'light' : 'dark'
  hasSavedPreference = true
  applyTheme(next)
  try {
    localStorage.setItem('theme', next)
  } catch {
    // The toggle still works for this visit when storage is unavailable.
  }
}

onMounted(() => {
  preference = window.matchMedia('(prefers-color-scheme: dark)')
  let saved: string | null = null
  try {
    saved = localStorage.getItem('theme')
  } catch {
    // Fall back to the system preference when storage is unavailable.
  }
  hasSavedPreference = saved === 'light' || saved === 'dark'
  applyTheme(hasSavedPreference ? saved as Theme : preference.matches ? 'dark' : 'light')
  preference.addEventListener('change', onSystemThemeChange)
})

onUnmounted(() => preference?.removeEventListener('change', onSystemThemeChange))
</script>

<template>
  <div class="page">
    <header class="header">
      <a class="identity" href="/" aria-label="Hachimi2333 home">
        <img src="https://static.hachimi2333.top/avatar.webp" alt="" width="40" height="40" />
      </a>

      <nav class="navigation" aria-label="Main navigation">
        <Button as-child variant="secondary" size="icon" class="nav-button">
          <a href="https://github.com/Hachimi2333" aria-label="GitHub" title="GitHub">
            <svg aria-hidden="true" viewBox="0 0 24 24" fill="currentColor"><path d="M12 .8a11.2 11.2 0 0 0-3.54 21.83c.56.1.76-.24.76-.54v-2.17c-3.11.68-3.77-1.32-3.77-1.32-.51-1.29-1.24-1.63-1.24-1.63-1.02-.7.08-.68.08-.68 1.13.08 1.72 1.16 1.72 1.16 1 1.72 2.62 1.22 3.26.93.1-.73.39-1.22.71-1.5-2.48-.28-5.1-1.24-5.1-5.54 0-1.23.44-2.23 1.16-3.02-.12-.28-.5-1.42.11-2.97 0 0 .95-.31 3.08 1.15A10.7 10.7 0 0 1 12 6.66c.95 0 1.91.13 2.8.38 2.14-1.46 3.08-1.15 3.08-1.15.61 1.55.23 2.69.11 2.97.72.79 1.16 1.79 1.16 3.02 0 4.31-2.62 5.26-5.12 5.54.4.35.76 1.03.76 2.07v2.6c0 .3.2.65.77.54A11.2 11.2 0 0 0 12 .8Z" /></svg>
          </a>
        </Button>
        <Button
          variant="secondary"
          size="icon"
          class="nav-button"
          type="button"
          :aria-label="theme === 'dark' ? 'Switch to light mode' : 'Switch to dark mode'"
          :title="theme === 'dark' ? 'Switch to light mode' : 'Switch to dark mode'"
          @click="toggleTheme"
        >
          <SunIcon v-if="theme === 'dark'" aria-hidden="true" :stroke-width="1.6" />
          <MoonIcon v-else aria-hidden="true" :stroke-width="1.6" />
        </Button>
      </nav>
    </header>

    <main class="main">
      <div class="intro">
        <h1>Hachimi2333</h1>
        <p class="bio">A student interested in programming, Linux, and systems development.</p>
      </div>
    </main>

    <footer class="footer">
      <span>© Hachimi2333</span>
      <div class="registrations" lang="zh-CN">
        <a href="https://beian.miit.gov.cn/">粤ICP备2026003234号-1</a>
        <a href="https://beian.mps.gov.cn/#/query/webSearch?code=44030002010010">粤公网安备44030002010010号</a>
      </div>
    </footer>
  </div>
</template>
