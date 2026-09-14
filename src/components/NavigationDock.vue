<script setup lang="ts">
const props = defineProps<{
  currentTab: "home" | "repos" | "blog" | "article" | "contact";
  theme: "light" | "dark";
  isMobile: boolean;
}>();

const emit = defineEmits<{
  (e: "navigate", tab: "home" | "repos" | "blog" | "contact"): void;
}>();

const navItems = [
  { id: "home", label: "Home", icon: "home" },
  { id: "repos", label: "GitHub", icon: "developer_mode" },
  { id: "blog", label: "Blog", icon: "article" },
  { id: "contact", label: "About", icon: "person" },
] as const;
</script>

<template>
  <!-- Desktop / Tablet Left Vertical Dock (Free Floating Squircles Matching Mockup) -->
  <aside v-if="!isMobile" class="tablet-launcher-dock" aria-label="Launcher Rail">
    <div class="dock-floating-column">
      <button
        v-for="item in navItems"
        :key="item.id"
        type="button"
        class="dock-launcher-btn"
        :class="{ active: currentTab === item.id || (item.id === 'blog' && currentTab === 'article') }"
        :title="item.label"
        @click="emit('navigate', item.id)"
      >
        <md-ripple></md-ripple>
        <span class="material-symbols-rounded dock-icon">{{ item.icon }}</span>
        <span class="dock-tooltip">{{ item.label }}</span>
      </button>
    </div>
  </aside>

  <!-- Google Pixel Mobile Material 3 Bottom Navigation Bar -->
  <nav v-else class="m3-bottom-nav-bar" aria-label="Material 3 Navigation Bar">
    <button
      v-for="item in navItems"
      :key="item.id"
      type="button"
      class="m3-nav-destination"
      :class="{ selected: currentTab === item.id || (item.id === 'blog' && currentTab === 'article') }"
      @click="emit('navigate', item.id)"
    >
      <div class="m3-nav-icon-container">
        <md-ripple></md-ripple>
        <span class="material-symbols-rounded m3-nav-icon">{{ item.icon }}</span>
      </div>
      <span class="m3-nav-label">{{ item.label }}</span>
    </button>
  </nav>
</template>

<style scoped>
/* =============================================================================
   FROSTED GLASS CONFIGURATION (EASILY ADJUSTABLE)
   These variables configure the frosted glass blur intensity, saturation,
   and translucent surface tinting for both desktop and mobile floating navs.
   You can adjust --nav-glass-blur here or in src/style/variables.css.
   ============================================================================= */
:root {
  --nav-glass-blur: 24px;              /* Adjust blur intensity (e.g. 12px, 20px, 32px) */
  --nav-glass-saturate: 180%;          /* Adjust saturation behind glass (100% - 200%) */
}

/* ==========================================================================
   DESKTOP / TABLET VERTICAL DOCK (MATCHING USER MOCKUP EXACTLY)
   ========================================================================== */
.tablet-launcher-dock {
  position: fixed;
  left: 2rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Background on nav buttons group with Frosted Glass */
.dock-floating-column {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.4rem;
  padding: 1.2rem 0.8rem;
  background: var(--nav-glass-bg, rgba(255, 248, 245, 0.72));
  backdrop-filter: blur(var(--nav-glass-blur, 24px)) saturate(var(--nav-glass-saturate, 180%));
  -webkit-backdrop-filter: blur(var(--nav-glass-blur, 24px)) saturate(var(--nav-glass-saturate, 180%));
  border: 1px solid var(--nav-glass-border, rgba(191, 96, 56, 0.18));
  border-radius: 9999px;
  box-shadow: var(--nav-glass-shadow, 0 10px 32px rgba(0, 0, 0, 0.10), 0 2px 8px rgba(191, 96, 56, 0.08), inset 0 1px 1px rgba(255, 255, 255, 0.6));
  transition: background-color 250ms ease, border-color 250ms ease, box-shadow 250ms ease;
}

[theme="dark"] .dock-floating-column {
  background: var(--nav-glass-bg, rgba(38, 27, 22, 0.72));
  border: 1px solid var(--nav-glass-border, rgba(255, 255, 255, 0.12));
  box-shadow: var(--nav-glass-shadow, 0 14px 40px rgba(0, 0, 0, 0.55), 0 2px 10px rgba(0, 0, 0, 0.35), inset 0 1px 1px rgba(255, 255, 255, 0.12));
}

/* Individual launcher buttons */
.dock-launcher-btn {
  position: relative;
  width: 4.4rem;
  height: 4.4rem;
  border-radius: 50%;
  border: none;
  background: var(--md-sys-color-dock-btn, #bf6038);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: #ffffff;
  transition:
    transform 280ms cubic-bezier(0.34, 1.56, 0.64, 1),
    border-radius 280ms cubic-bezier(0.34, 1.56, 0.64, 1),
    box-shadow 280ms ease,
    background-color 200ms ease;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.14);
}

.dock-launcher-btn:hover {
  transform: scale(1.12);
  filter: brightness(1.1);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.22);
}

/* Active tab button morphs into rounded squircle */
.dock-launcher-btn.active {
  border-radius: 1.4rem;
  background: var(--md-sys-color-dock-btn-active, #a64d26);
  transform: scale(1.06);
  box-shadow: 0 6px 18px rgba(191, 96, 56, 0.38);
}

.dock-icon {
  font-size: 2.2rem;
  color: #ffffff;
  transition: transform 250ms ease;
}

.dock-launcher-btn.active .dock-icon {
  font-variation-settings: "FILL" 1, "wght" 500;
  transform: scale(1.05);
}

/* Tooltip on hover */
.dock-tooltip {
  position: absolute;
  left: calc(100% + 1.2rem);
  top: 50%;
  transform: translateY(-50%) translateX(-8px);
  padding: 0.5rem 1.1rem;
  background: var(--md-sys-color-surface-container-high, #f2d8c7);
  color: var(--md-sys-color-on-surface, #221a16);
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.2rem;
  font-weight: 600;
  border-radius: 0.8rem;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
  pointer-events: none;
  white-space: nowrap;
  opacity: 0;
  transition: opacity 200ms ease, transform 200ms ease;
  z-index: 1000;
}

.dock-launcher-btn:hover .dock-tooltip {
  opacity: 1;
  transform: translateY(-50%) translateX(0);
}

/* ==========================================================================
   MOBILE FLOATING MATERIAL 3 NAVIGATION DOCK
   Floats centered above the bottom edge with a pill-shaped frosted glass shell,
   matching the desktop floating launcher dock aesthetic.
   ========================================================================== */
.m3-bottom-nav-bar {
  position: fixed;
  bottom: calc(1.4rem + env(safe-area-inset-bottom, 0px));
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 3.2rem);
  max-width: 42rem;
  height: 6.8rem;
  padding: 0 0.6rem;
  border-radius: 9999px;
  background: var(--nav-glass-bg, rgba(255, 248, 245, 0.72));
  backdrop-filter: blur(var(--nav-glass-blur, 24px)) saturate(var(--nav-glass-saturate, 180%));
  -webkit-backdrop-filter: blur(var(--nav-glass-blur, 24px)) saturate(var(--nav-glass-saturate, 180%));
  border: 1px solid var(--nav-glass-border, rgba(191, 96, 56, 0.18));
  box-shadow: var(--nav-glass-shadow, 0 10px 32px rgba(0, 0, 0, 0.10), 0 2px 8px rgba(191, 96, 56, 0.08), inset 0 1px 1px rgba(255, 255, 255, 0.6));
  display: flex;
  align-items: center;
  justify-content: space-around;
  z-index: 100;
  user-select: none;
  transition: transform 250ms cubic-bezier(0.2, 0, 0, 1),
              background-color 250ms ease,
              border-color 250ms ease,
              box-shadow 250ms ease;
}

[theme="dark"] .m3-bottom-nav-bar {
  background: var(--nav-glass-bg, rgba(38, 27, 22, 0.72));
  border: 1px solid var(--nav-glass-border, rgba(255, 255, 255, 0.12));
  box-shadow: var(--nav-glass-shadow, 0 14px 40px rgba(0, 0, 0, 0.55), 0 2px 10px rgba(0, 0, 0, 0.35), inset 0 1px 1px rgba(255, 255, 255, 0.12));
}

.m3-nav-destination {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0.4rem 0.6rem;
  color: var(--md-sys-color-on-surface-variant, #52443d);
  transition: color 200ms ease, transform 150ms ease;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  flex: 1;
  max-width: 9.5rem;
}

.m3-nav-destination:active {
  transform: scale(0.94);
}

.m3-nav-icon-container {
  position: relative;
  width: 5.6rem;
  height: 3rem;
  border-radius: 9999px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 250ms cubic-bezier(0.34, 1.56, 0.64, 1),
              box-shadow 250ms ease,
              transform 200ms ease;
  overflow: hidden;
}

.m3-nav-icon {
  font-size: 2.2rem;
  transition: font-variation-settings 200ms ease, transform 200ms ease, color 200ms ease;
}

.m3-nav-label {
  font-family: var(--font-sans, "Google Sans Flex", "Inter", sans-serif);
  font-size: 1.15rem;
  font-weight: 500;
  letter-spacing: 0.02em;
  transition: font-weight 200ms ease, color 200ms ease;
  white-space: nowrap;
}

/* Active destination state */
.m3-nav-destination.selected {
  color: var(--md-sys-color-on-surface, #221a16);
}

.m3-nav-destination.selected .m3-nav-icon-container {
  background-color: #ffd2b8;
  box-shadow: 0 2px 6px rgba(191, 96, 56, 0.22);
}

[theme="dark"] .m3-nav-destination.selected .m3-nav-icon-container {
  background-color: #723214;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.45);
}

.m3-nav-destination.selected .m3-nav-icon {
  color: #3b1404;
  font-variation-settings: "FILL" 1, "wght" 600;
  transform: scale(1.05);
}

[theme="dark"] .m3-nav-destination.selected .m3-nav-icon {
  color: #ffdccf;
  font-variation-settings: "FILL" 1, "wght" 600;
  transform: scale(1.05);
}

.m3-nav-destination.selected .m3-nav-label {
  font-weight: 700;
  color: var(--md-sys-color-on-surface, #221a16);
}

[theme="dark"] .m3-nav-destination.selected .m3-nav-label {
  color: #ede0db;
}

@media (max-width: 360px) {
  .m3-bottom-nav-bar {
    width: calc(100% - 2rem);
    height: 6.4rem;
    padding: 0 0.4rem;
  }
  .m3-nav-icon-container {
    width: 4.8rem;
    height: 2.8rem;
  }
  .m3-nav-icon {
    font-size: 2rem;
  }
  .m3-nav-label {
    font-size: 1.05rem;
  }
}
</style>
