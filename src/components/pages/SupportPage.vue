<script setup lang="ts">
import { ref, markRaw } from "vue";
import {
  IconHeartHandshake,
  IconWallet,
  IconBrandTelegram,
  IconCurrencyTether,
  IconDiamond,
  IconBrandBinance,
  IconCopy,
  IconCheck,
  IconShieldCheck,
  IconInfoCircle,
  IconBrandGithub,
  IconArrowUpRight,
  IconStar,
  IconSparkles,
} from "@tabler/icons-vue";

interface CryptoWallet {
  id: string;
  name: string;
  symbol: string;
  network: string;
  badge: string;
  address: string;
  color: string;
  icon: any;
  highlight?: string;
  label?: string;
  actionText?: string;
  copiedText?: string;
}

const copiedId = ref<string | null>(null);

function fallbackCopyText(text: string) {
  try {
    const textArea = document.createElement("textarea");
    textArea.value = text;
    textArea.style.position = "fixed";
    textArea.style.left = "-9999px";
    textArea.style.top = "0";
    document.body.appendChild(textArea);
    textArea.focus();
    textArea.select();
    document.execCommand("copy");
    document.body.removeChild(textArea);
  } catch {
    // Ignore fallback errors
  }
}

async function copyAddress(wallet: CryptoWallet) {
  try {
    if (navigator.clipboard && navigator.clipboard.writeText) {
      await navigator.clipboard.writeText(wallet.address);
    } else {
      fallbackCopyText(wallet.address);
    }
  } catch {
    fallbackCopyText(wallet.address);
  }
  copiedId.value = wallet.id;
  setTimeout(() => {
    if (copiedId.value === wallet.id) {
      copiedId.value = null;
    }
  }, 2500);
}

const wallets: CryptoWallet[] = [
  {
    id: "ton",
    name: "GRAM",
    symbol: "TON",
    network: "The Open Network (TON)",
    badge: "Telegram Native",
    address: "UQC6Y5b5Yy3Vlr6XveRnz2_MCArjxTaUrJXfQseH4thgJwQf",
    color: "#0088cc",
    icon: markRaw(IconBrandTelegram),
    highlight: "Recommended",
    label: "Wallet Address:",
    actionText: "Copy Address",
    copiedText: "Address Copied!",
  },
  {
    id: "trx",
    name: "TRON",
    symbol: "TRX",
    network: "TRON Mainnet (TRC20)",
    badge: "TRX / TRC20",
    address: "TE7qfYGZRK6h5LQrWbTHFDUMwWwwT66FMQ",
    color: "#ef0027",
    icon: markRaw(IconDiamond),
    highlight: "Fast & Low Fee",
    label: "Wallet Address:",
    actionText: "Copy Address",
    copiedText: "Address Copied!",
  },
  {
    id: "usdt-bep20",
    name: "Tether USD",
    symbol: "USDT",
    network: "BEP20 (BNB Smart Chain)",
    badge: "BEP20 / BSC",
    address: "0x2d826f9a4B68842F1B93643CdE6cBDf3a0e5dC0C",
    color: "#26a17b",
    icon: markRaw(IconCurrencyTether),
    highlight: "Low Network Fee",
    label: "Wallet Address:",
    actionText: "Copy Address",
    copiedText: "Address Copied!",
  },
  {
    id: "binance-pay",
    name: "Binance Pay",
    symbol: "Pay ID",
    network: "Binance App / Instant Transfer",
    badge: "Instant 0% Fees",
    address: "67192565",
    color: "#f0b90b",
    icon: markRaw(IconBrandBinance),
    highlight: "0% Transfer Fee",
    label: "Binance Pay ID / Binance ID:",
    actionText: "Copy Pay ID",
    copiedText: "Pay ID Copied!",
  },
];
</script>

<template>
  <div class="about-page-view support-page-view">
    <div class="minimal-about-container">
      <!-- 1. Hero Header Card -->
      <section class="minimal-profile-card support-hero-card">
        <div class="support-header-layout">
          <div class="support-badge-row">
            <span class="support-pill-badge">
              <IconHeartHandshake :size="16" :stroke-width="2.2" />
              Direct Support
            </span>
          </div>

          <div class="support-hero-title-group">
            <h1 class="support-hero-title">Support FAgram Desktop</h1>
            <p class="support-hero-subtitle">
              FAgram Desktop is open-source and free to use. If you enjoy using
              it and want to support ongoing development, server costs, and new
              features, donations are greatly appreciated!
            </p>
          </div>

          <!-- Network Caution Notice -->
          <div class="support-notice-box">
            <IconInfoCircle
              class="support-notice-icon"
              :size="20"
              :stroke-width="2"
            />
            <div class="support-notice-text">
              <span>
                Please ensure you transfer assets exclusively via the specified
                network listed on each card to prevent loss of funds. For
                Binance Pay, you can send directly using the Pay ID in your
                Binance app with zero fees.
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- 2. Crypto & Payment Section -->
      <section class="minimal-projects-section support-wallets-section">
        <div class="wallets-header-row">
          <h2 class="section-heading">
            <IconWallet class="heading-icon" :size="20" :stroke-width="2" />
            Crypto & Payment Methods
          </h2>
          <span class="wallets-count-badge">
            {{ wallets.length }} Options
          </span>
        </div>

        <div class="wallets-grid">
          <div
            v-for="wallet in wallets"
            :key="wallet.id"
            class="wallet-card"
            :class="{ 'is-copied': copiedId === wallet.id }"
          >
            <!-- Card Header: Icon + Name + Badge -->
            <div class="wallet-card-header">
              <div
                class="wallet-icon-box"
                :style="{
                  '--coin-color': wallet.color,
                }"
              >
                <component :is="wallet.icon" :size="24" :stroke-width="1.8" />
              </div>

              <div class="wallet-identity-col">
                <div class="wallet-title-row">
                  <span class="wallet-name">{{ wallet.name }}</span>
                  <span class="wallet-symbol">{{ wallet.symbol }}</span>
                </div>
                <div class="wallet-network-row">
                  <span class="wallet-network-label">{{ wallet.network }}</span>
                </div>
              </div>

              <div class="wallet-badges-col">
                <span v-if="wallet.highlight" class="wallet-highlight-badge">
                  {{ wallet.highlight }}
                </span>
                <span class="wallet-network-badge">
                  {{ wallet.badge }}
                </span>
              </div>
            </div>

            <!-- Address Display Area -->
            <div class="wallet-address-wrapper">
              <span class="wallet-address-label">{{
                wallet.label || "Wallet Address:"
              }}</span>
              <div
                class="wallet-address-box"
                :title="'Click to copy ' + wallet.name"
                @click="copyAddress(wallet)"
              >
                <code class="wallet-address-code">{{ wallet.address }}</code>
              </div>
            </div>

            <!-- Action Button -->
            <div class="wallet-actions-row">
              <button
                type="button"
                class="copy-wallet-btn"
                :class="{ 'btn-copied': copiedId === wallet.id }"
                @click="copyAddress(wallet)"
              >
                <md-ripple></md-ripple>
                <IconCheck
                  v-if="copiedId === wallet.id"
                  class="btn-icon"
                  :size="18"
                  :stroke-width="2.2"
                />
                <IconCopy
                  v-else
                  class="btn-icon"
                  :size="18"
                  :stroke-width="2"
                />
                <span>{{
                  copiedId === wallet.id
                    ? wallet.copiedText || "Address Copied!"
                    : wallet.actionText || "Copy Address"
                }}</span>
              </button>
            </div>
          </div>
        </div>
        <!-- Support Badge Reward Section -->
        <div class="support-badge-claim-container">
          <div class="support-badge-claim-row">
            <div class="badge-claim-icon-wrapper">
              <IconSparkles :size="20" :stroke-width="2.2" />
            </div>
            <p class="badge-claim-text">
              Send a DM with proof of transaction to claim your exclusive
              <strong>SupportBadge</strong>
            </p>
          </div>
          <div class="support-badge-action-row">
            <a
              href="https://t.me/sidawakens"
              target="_blank"
              rel="noopener noreferrer"
              class="connect-btn badge-claim-btn"
            >
              <md-ripple></md-ripple>
              <IconBrandTelegram
                class="btn-icon"
                :size="20"
                :stroke-width="2"
              />
              <span>Send DM to Claim SupportBadge</span>
              <IconArrowUpRight
                class="arrow-icon"
                :size="16"
                :stroke-width="2"
              />
            </a>
          </div>
        </div>
      </section>

      <!-- 3. Other Ways to Support -->
      <section class="minimal-connect-section support-other-section">
        <h2 class="section-heading">
          <IconStar class="heading-icon" :size="20" :stroke-width="2" />
          Other Ways to Support
        </h2>
        <p class="other-support-desc">
          Beyond financial contributions, starring the repository, sharing with
          other Telegram power users, and reporting bugs helps immensely.
        </p>

        <div class="connect-links-row">
          <a
            href="https://github.com/fagramdesktop/fagram-desktop"
            target="_blank"
            rel="noopener noreferrer"
            class="connect-btn"
          >
            <md-ripple></md-ripple>
            <IconBrandGithub class="btn-icon" :size="20" :stroke-width="2" />
            <span>Star FAgram Desktop on GitHub</span>
            <IconArrowUpRight class="arrow-icon" :size="16" :stroke-width="2" />
          </a>

          <a
            href="https://t.me/fagramdesktop"
            target="_blank"
            rel="noopener noreferrer"
            class="connect-btn"
          >
            <md-ripple></md-ripple>
            <IconBrandTelegram class="btn-icon" :size="20" :stroke-width="2" />
            <span>Join Telegram Community</span>
            <IconArrowUpRight class="arrow-icon" :size="16" :stroke-width="2" />
          </a>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
.about-page-view {
  display: flex;
  justify-content: center;
  width: 100%;
  max-width: 96rem;
  margin: 0 auto;
  padding: 2rem 2rem 6rem 9rem;
  box-sizing: border-box;
}

.minimal-about-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
}

/* =============================================================================
   Card Containers (Consistent with MD3 Surface Cards)
   ============================================================================= */
.minimal-profile-card,
.minimal-projects-section,
.minimal-connect-section {
  background: var(--md-sys-color-surface-container, rgba(255, 255, 255, 0.7));
  backdrop-filter: blur(24px) saturate(180%);
  -webkit-backdrop-filter: blur(24px) saturate(180%);
  border: 1px solid var(--md-sys-color-outline-variant, rgba(191, 96, 56, 0.14));
  border-radius: 28px;
  padding: 2.8rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transition:
    border-color 250ms ease,
    box-shadow 250ms ease;
}

[theme="dark"] .minimal-profile-card,
[theme="dark"] .minimal-projects-section,
[theme="dark"] .minimal-connect-section {
  background: var(--md-sys-color-surface-container, rgba(40, 30, 26, 0.7));
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 8px 28px rgba(0, 0, 0, 0.35);
}

/* =============================================================================
   Hero Header
   ============================================================================= */
.support-header-layout {
  display: flex;
  flex-direction: column;
  gap: 1.6rem;
}

.support-badge-row {
  display: flex;
  align-items: center;
}

.support-pill-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--md-sys-color-primary, #bf6038);
  background: rgba(191, 96, 56, 0.12);
  padding: 0.35rem 1rem;
  border-radius: 9999px;
  border: 1px solid rgba(191, 96, 56, 0.2);
}

[theme="dark"] .support-pill-badge {
  background: rgba(255, 181, 157, 0.15);
  color: var(--md-sys-color-primary, #ffb59d);
  border-color: rgba(255, 181, 157, 0.25);
}

.support-hero-title-group {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

.support-hero-title {
  font-family: "Lexend Deca", sans-serif;
  font-size: 2.8rem;
  font-weight: 800;
  color: var(--md-sys-color-on-surface, #221a16);
  line-height: 1.15;
  margin: 0;
}

.support-hero-subtitle {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.45rem;
  line-height: 1.6;
  color: var(--md-sys-color-on-surface-variant, #52443d);
  margin: 0;
  max-width: 75ch;
}

[theme="dark"] .support-hero-subtitle {
  color: var(--md-sys-color-on-surface-variant, #d7c2b9);
}

/* =============================================================================
   Notice Box
   ============================================================================= */
.support-notice-box {
  display: flex;
  align-items: flex-start;
  gap: 1.2rem;
  padding: 1.2rem 1.6rem;
  border-radius: 16px;
  background: rgba(191, 96, 56, 0.08);
  border: 1px solid rgba(191, 96, 56, 0.18);
  color: var(--md-sys-color-on-surface, #221a16);
}

[theme="dark"] .support-notice-box {
  background: rgba(255, 181, 157, 0.08);
  border-color: rgba(255, 181, 157, 0.18);
  color: var(--md-sys-color-on-surface, #ede0db);
}

.support-notice-icon {
  color: var(--md-sys-color-primary, #bf6038);
  flex-shrink: 0;
  margin-top: 0.2rem;
}

[theme="dark"] .support-notice-icon {
  color: var(--md-sys-color-primary, #ffb59d);
}

.support-notice-text {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.25rem;
  line-height: 1.5;
  font-weight: 500;
}

/* =============================================================================
   Wallets Grid
   ============================================================================= */
.wallets-header-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1.6rem;
}

.section-heading {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.6rem;
  font-weight: 700;
  color: var(--md-sys-color-on-surface, #221a16);
  margin: 0;
}

.heading-icon {
  font-size: 2rem;
  color: var(--md-sys-color-primary, #bf6038);
}

[theme="dark"] .heading-icon {
  color: var(--md-sys-color-primary, #ffb59d);
}

.wallets-count-badge {
  font-family: "JetBrains Mono", monospace;
  font-size: 1.15rem;
  font-weight: 600;
  color: var(--md-sys-color-on-surface-variant, #52443d);
  background: var(--md-sys-color-surface-container-high, rgba(0, 0, 0, 0.04));
  padding: 0.3rem 0.8rem;
  border-radius: 9999px;
  border: 1px solid var(--md-sys-color-outline-variant, rgba(0, 0, 0, 0.08));
}

[theme="dark"] .wallets-count-badge {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.1);
  color: var(--md-sys-color-on-surface-variant, #d7c2b9);
}

.wallets-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(38rem, 1fr));
  gap: 1.6rem;
}

.wallet-card {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
  padding: 2rem;
  border-radius: 20px;
  background: var(--md-sys-color-surface-container-high, rgba(0, 0, 0, 0.03));
  border: 1px solid var(--md-sys-color-outline-variant, rgba(191, 96, 56, 0.12));
  transition:
    transform 200ms ease,
    box-shadow 200ms ease,
    border-color 200ms ease,
    background 200ms ease;
}

[theme="dark"] .wallet-card {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(255, 255, 255, 0.08);
}

.wallet-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  border-color: var(--md-sys-color-primary, #bf6038);
}

.wallet-card.is-copied {
  border-color: #26a17b;
}

.wallet-card-header {
  display: flex;
  align-items: center;
  gap: 1.4rem;
}

.wallet-icon-box {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 4.4rem;
  height: 4.4rem;
  border-radius: 14px;
  background: color-mix(in srgb, var(--coin-color, #bf6038) 12%, transparent);
  color: var(--coin-color, #bf6038);
  border: 1px solid
    color-mix(in srgb, var(--coin-color, #bf6038) 25%, transparent);
  flex-shrink: 0;
}

.wallet-identity-col {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex: 1;
  min-width: 0;
}

.wallet-title-row {
  display: flex;
  align-items: baseline;
  gap: 0.6rem;
}

.wallet-name {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--md-sys-color-on-surface, #221a16);
}

.wallet-symbol {
  font-family: "JetBrains Mono", monospace;
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--md-sys-color-primary, #bf6038);
}

[theme="dark"] .wallet-symbol {
  color: var(--md-sys-color-primary, #ffb59d);
}

.wallet-network-row {
  display: flex;
  align-items: center;
}

.wallet-network-label {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.2rem;
  color: var(--md-sys-color-on-surface-variant, #52443d);
}

[theme="dark"] .wallet-network-label {
  color: var(--md-sys-color-on-surface-variant, #d7c2b9);
}

.wallet-badges-col {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.4rem;
  flex-shrink: 0;
}

.wallet-highlight-badge {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1rem;
  font-weight: 700;
  color: #0088cc;
  background: rgba(0, 136, 204, 0.12);
  padding: 0.2rem 0.6rem;
  border-radius: 9999px;
  white-space: nowrap;
}

[theme="dark"] .wallet-highlight-badge {
  color: #38bdf8;
  background: rgba(56, 189, 248, 0.15);
}

.wallet-network-badge {
  font-family: "JetBrains Mono", monospace;
  font-size: 1.05rem;
  font-weight: 600;
  padding: 0.2rem 0.65rem;
  border-radius: 6px;
  background: var(--md-sys-color-surface-container, rgba(0, 0, 0, 0.05));
  border: 1px solid var(--md-sys-color-outline-variant, rgba(0, 0, 0, 0.1));
  color: var(--md-sys-color-on-surface, #221a16);
  white-space: nowrap;
}

[theme="dark"] .wallet-network-badge {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.12);
  color: var(--md-sys-color-on-surface, #ede0db);
}

/* =============================================================================
   Address Box
   ============================================================================= */
.wallet-address-wrapper {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.wallet-address-label {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--md-sys-color-on-surface-variant, #52443d);
}

[theme="dark"] .wallet-address-label {
  color: var(--md-sys-color-on-surface-variant, #d7c2b9);
}

.wallet-address-box {
  background: var(--md-sys-color-surface, rgba(255, 255, 255, 0.6));
  border: 1px solid var(--md-sys-color-outline-variant, rgba(191, 96, 56, 0.15));
  border-radius: 12px;
  padding: 0.9rem 1.2rem;
  cursor: pointer;
  transition:
    background 180ms ease,
    border-color 180ms ease;
  overflow: hidden;
}

[theme="dark"] .wallet-address-box {
  background: rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 255, 255, 0.1);
}

.wallet-address-box:hover {
  background: var(--md-sys-color-surface-container-high, rgba(0, 0, 0, 0.04));
  border-color: var(--md-sys-color-primary, #bf6038);
}

.wallet-address-code {
  font-family: "JetBrains Mono", monospace;
  font-size: 1.2rem;
  line-height: 1.4;
  color: var(--md-sys-color-on-surface, #221a16);
  word-break: break-all;
  display: block;
}

[theme="dark"] .wallet-address-code {
  color: var(--md-sys-color-on-surface, #ede0db);
}

/* =============================================================================
   Action Buttons
   ============================================================================= */
.wallet-actions-row {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.copy-wallet-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.7rem;
  width: 100%;
  padding: 0.9rem 1.4rem;
  border-radius: 12px;
  border: 1px solid var(--md-sys-color-primary, #bf6038);
  background: rgba(191, 96, 56, 0.08);
  color: var(--md-sys-color-primary, #bf6038);
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.25rem;
  font-weight: 700;
  cursor: pointer;
  overflow: hidden;
  transition:
    background 180ms ease,
    color 180ms ease,
    border-color 180ms ease,
    transform 150ms ease;
}

[theme="dark"] .copy-wallet-btn {
  border-color: var(--md-sys-color-primary, #ffb59d);
  background: rgba(255, 181, 157, 0.1);
  color: var(--md-sys-color-primary, #ffb59d);
}

.copy-wallet-btn:hover {
  background: var(--md-sys-color-primary, #bf6038);
  color: #ffffff;
  transform: translateY(-1px);
}

[theme="dark"] .copy-wallet-btn:hover {
  background: var(--md-sys-color-primary, #ffb59d);
  color: #221a16;
}

.copy-wallet-btn.btn-copied {
  background: #26a17b;
  border-color: #26a17b;
  color: #ffffff;
}

/* =============================================================================
   Other Support Links
   ============================================================================= */
.other-support-desc {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.35rem;
  line-height: 1.6;
  color: var(--md-sys-color-on-surface-variant, #52443d);
  margin: 0 0 1.6rem;
  max-width: 70ch;
}

[theme="dark"] .other-support-desc {
  color: var(--md-sys-color-on-surface-variant, #d7c2b9);
}

.connect-links-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1.2rem;
}

.connect-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 1rem;
  padding: 1.2rem 2rem;
  border-radius: 16px;
  background: var(--md-sys-color-surface-container-high, rgba(0, 0, 0, 0.04));
  border: 1px solid var(--md-sys-color-outline-variant, rgba(191, 96, 56, 0.15));
  color: var(--md-sys-color-on-surface, #221a16);
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.3rem;
  font-weight: 600;
  text-decoration: none;
  overflow: hidden;
  cursor: pointer;
  transition:
    background 200ms ease,
    border-color 200ms ease,
    transform 200ms ease;
}

[theme="dark"] .connect-btn {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(255, 255, 255, 0.08);
  color: var(--md-sys-color-on-surface, #ede0db);
}

.connect-btn:hover {
  background: rgba(191, 96, 56, 0.08);
  border-color: var(--md-sys-color-primary, #bf6038);
  transform: translateY(-2px);
}

[theme="dark"] .connect-btn:hover {
  background: rgba(255, 181, 157, 0.1);
  border-color: var(--md-sys-color-primary, #ffb59d);
}

.connect-btn .btn-icon {
  color: var(--md-sys-color-primary, #bf6038);
}

[theme="dark"] .connect-btn .btn-icon {
  color: var(--md-sys-color-primary, #ffb59d);
}

.connect-btn .arrow-icon {
  color: var(--md-sys-color-on-surface-variant, #52443d);
  opacity: 0.7;
  transition:
    transform 200ms ease,
    opacity 200ms ease;
}

.connect-btn:hover .arrow-icon {
  transform: translate(2px, -2px);
  opacity: 1;
}

/* =============================================================================
   Support Badge Claim (Centered)
   ============================================================================= */
.support-badge-claim-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: 1.4rem;
  margin-top: 2.4rem;
  padding: 2.2rem 2rem;
  border-radius: 20px;
  background: var(--md-sys-color-surface-container-high, rgba(0, 0, 0, 0.03));
  border: 1px dashed
    var(--md-sys-color-outline-variant, rgba(191, 96, 56, 0.25));
  width: 100%;
  box-sizing: border-box;
}

[theme="dark"] .support-badge-claim-container {
  background: rgba(255, 255, 255, 0.03);
  border-color: rgba(255, 255, 255, 0.14);
}

.support-badge-claim-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
  text-align: center;
}

.badge-claim-icon-wrapper {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: var(--md-sys-color-primary, #bf6038);
  flex-shrink: 0;
}

[theme="dark"] .badge-claim-icon-wrapper {
  color: var(--md-sys-color-primary, #ffb59d);
}

.badge-claim-text {
  font-family: "Lexend Deca", sans-serif;
  font-size: 1.35rem;
  line-height: 1.5;
  color: var(--md-sys-color-on-surface, #221a16);
  margin: 0;
  text-align: center;
}

[theme="dark"] .badge-claim-text {
  color: var(--md-sys-color-on-surface, #ede0db);
}

.badge-claim-text strong {
  color: var(--md-sys-color-primary, #bf6038);
  font-weight: 700;
}

[theme="dark"] .badge-claim-text strong {
  color: var(--md-sys-color-primary, #ffb59d);
}

.support-badge-action-row {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.badge-claim-btn {
  justify-content: center !important;
  text-align: center;
}

/* =============================================================================
   Responsive Media Queries
   ============================================================================= */
@media (max-width: 768px) {
  .about-page-view {
    padding: 1rem 1.6rem 2.4rem 1.6rem;
  }

  .minimal-profile-card,
  .minimal-projects-section,
  .minimal-connect-section {
    padding: 2rem 1.8rem;
    border-radius: 28px;
  }

  .support-hero-title {
    font-size: 2.3rem;
  }

  .wallets-grid {
    grid-template-columns: 1fr;
    gap: 1.4rem;
  }

  .wallet-card {
    padding: 1.6rem;
  }

  .support-badge-claim-container {
    padding: 1.8rem 1.4rem;
    margin-top: 1.8rem;
  }

  .badge-claim-btn {
    width: 100%;
    justify-content: center !important;
  }

  .connect-links-row {
    flex-direction: column;
    gap: 1rem;
  }

  .connect-btn {
    width: 100%;
    justify-content: flex-start;
  }

  .connect-btn .arrow-icon {
    margin-left: auto;
  }
}

@media (max-width: 400px) {
  .minimal-profile-card,
  .minimal-projects-section,
  .minimal-connect-section {
    padding: 1.8rem 1.4rem;
  }

  .support-hero-title {
    font-size: 2rem;
  }

  .wallet-card-header {
    flex-wrap: wrap;
  }

  .wallet-badges-col {
    align-items: flex-start;
    width: 100%;
    flex-direction: row;
    margin-top: 0.4rem;
  }
}
</style>
