<script setup lang="ts">
import {
  IconHome,
  IconHomeFilled,
  IconBrandGithub,
  IconBrandGithubFilled,
  IconArticle,
  IconArticleFilled,
  IconUser,
  IconUserFilled,
} from "@tabler/icons-vue";

const props = defineProps<{
  currentTab: "home" | "repos" | "blog" | "article" | "contact" | "support";
  theme: "light" | "dark";
  isMobile: boolean;
}>();

const emit = defineEmits<{
  (e: "navigate", tab: "home" | "repos" | "blog" | "contact"): void;
}>();

const navItems = [
  {
    id: "home" as const,
    label: "Home",
    activeIcon: IconHomeFilled,
    inactiveIcon: IconHome,
  },
  {
    id: "repos" as const,
    label: "GitHub",
    activeIcon: IconBrandGithubFilled,
    inactiveIcon: IconBrandGithub,
  },
  {
    id: "blog" as const,
    label: "Blog",
    activeIcon: IconArticleFilled,
    inactiveIcon: IconArticle,
  },
  {
    id: "contact" as const,
    label: "About",
    activeIcon: IconUserFilled,
    inactiveIcon: IconUser,
  },
];

function isTabActive(id: string) {
  return (
    props.currentTab === id || (id === "blog" && props.currentTab === "article")
  );
}
</script>

<template>
  <!-- Material Design 3 Expressive Floating Navigation Dock -->
  <nav class="m3-expressive-dock" aria-label="Navigation Dock">
    <button
      v-for="item in navItems"
      :key="item.id"
      type="button"
      class="m3-dock-item"
      :class="{ active: isTabActive(item.id) }"
      :title="item.label"
      :aria-label="item.label"
      @click="emit('navigate', item.id)"
    >
      <md-ripple></md-ripple>
      <component
        :is="isTabActive(item.id) ? item.activeIcon : item.inactiveIcon"
        class="m3-dock-icon"
        :size="24"
        :stroke-width="1.8"
      />
    </button>
  </nav>
</template>

<style scoped>
/* =============================================================================
   FROSTED GLASS CONFIGURATION (EASILY ADJUSTABLE)
   ============================================================================= */
:root {
  --nav-glass-blur: 24px;
  --nav-glass-saturate: 180%;
}

/* =============================================================================
   MATERIAL DESIGN 3 EXPRESSIVE FLOATING DOCK
   Mobile Viewport: Floating horizontal pill dock at the bottom.
   Desktop Viewport: Floating vertical pill dock on the left side.
   ============================================================================= */
.m3-expressive-dock {
  position: fixed;
  bottom: calc(1.4rem + env(safe-area-inset-bottom, 0px));
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  padding: 0.6rem 0.8rem;
  height: 5.6rem;
  border-radius: 9999px;
  background: var(--nav-glass-bg, rgba(255, 248, 245, 0.76));
  backdrop-filter: blur(var(--nav-glass-blur, 24px))
    saturate(var(--nav-glass-saturate, 180%));
  -webkit-backdrop-filter: blur(var(--nav-glass-blur, 24px))
    saturate(var(--nav-glass-saturate, 180%));
  border: none;
  outline: none;
  box-shadow: var(
    --nav-glass-shadow,
    0 10px 32px rgba(0, 0, 0, 0.1),
    0 2px 8px rgba(191, 96, 56, 0.08)
  );
  z-index: 100;
  user-select: none;
  box-sizing: border-box;
  transition:
    transform 250ms cubic-bezier(0.2, 0, 0, 1),
    background-color 250ms ease,
    box-shadow 250ms ease;
}

[theme="dark"] .m3-expressive-dock {
  background: var(--nav-glass-bg, rgba(38, 27, 22, 0.76));
  border: none;
  outline: none;
  box-shadow: var(
    --nav-glass-shadow,
    0 14px 40px rgba(0, 0, 0, 0.55),
    0 2px 10px rgba(0, 0, 0, 0.35)
  );
}

.m3-expressive-dock:focus,
.m3-expressive-dock:focus-visible {
  outline: none;
}

/* =============================================================================
   M3 EXPRESSIVE DOCK ITEMS (ALL PILL SHAPED, NO HOVER EFFECTS)
   Both inactive and active backgrounds are 100% pill shaped (border-radius: 9999px).
   Hover effects are completely disabled.
   ============================================================================= */
.m3-dock-item {
  position: relative;
  width: 5rem;
  height: 4.2rem;
  border-radius: 9999px;
  border: none;
  outline: none;
  background: rgba(191, 96, 56, 0.11);
  color: var(--md-sys-color-on-surface-variant, #52443d);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  overflow: hidden;
  -webkit-tap-highlight-color: transparent;
  flex-shrink: 0;
  transition:
    width 320ms cubic-bezier(0.34, 1.56, 0.64, 1),
    height 320ms cubic-bezier(0.34, 1.56, 0.64, 1),
    background-color 250ms ease,
    box-shadow 250ms ease,
    color 200ms ease;
}

[theme="dark"] .m3-dock-item {
  background: rgba(255, 255, 255, 0.08);
  color: var(--md-sys-color-on-surface-variant, #d7c2b8);
}

/* Active touch/click feedback */
.m3-dock-item:active {
  transform: scale(0.94);
}

/* Active tab: Pill shaped (border-radius: 9999px) with expanded width on mobile */
.m3-dock-item.active {
  width: 7.8rem;
  height: 4.2rem;
  border-radius: 9999px;
  background-color: var(--md-sys-color-primary, #bf6038);
  color: #ffffff;
  box-shadow: 0 4px 16px rgba(191, 96, 56, 0.38);
}

[theme="dark"] .m3-dock-item.active {
  background-color: #c8683f;
  color: #ffffff;
  box-shadow: 0 4px 18px rgba(0, 0, 0, 0.45);
}

/* Icon styling */
.m3-dock-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 250ms ease;
}

.m3-dock-item.active .m3-dock-icon {
  transform: scale(1.06);
}

/* =============================================================================
   DESKTOP VIEWPORT (ORIGINAL LEFT-SIDE VERTICAL DOCK POSITION)
   Dock is anchored to the left: 2rem and vertically centered.
   ============================================================================= */
@media (min-width: 769px) {
  .m3-expressive-dock {
    left: 2rem;
    top: 50%;
    bottom: auto;
    transform: translateY(-50%);
    flex-direction: column;
    width: auto;
    height: auto;
    padding: 0.8rem 0.6rem;
    gap: 0.8rem;
    border-radius: 9999px;
  }

  .m3-dock-item {
    width: 4.6rem;
    height: 4.6rem;
    border-radius: 9999px;
  }

  /* Active tab on desktop expands vertically as a pill */
  .m3-dock-item.active {
    width: 4.6rem;
    height: 7rem;
    border-radius: 9999px;
  }
}

/* =============================================================================
   COMPACT MOBILE (< 360PX)
   ============================================================================= */
@media (max-width: 360px) {
  .m3-expressive-dock {
    gap: 0.5rem;
    padding: 0.5rem 0.6rem;
    height: 5.2rem;
  }

  .m3-dock-item {
    width: 4.4rem;
    height: 3.8rem;
  }

  .m3-dock-item.active {
    width: 6.8rem;
    height: 3.8rem;
  }
}
</style>
