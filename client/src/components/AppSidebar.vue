<template>
  <aside class="sidebar" :class="{ collapsed: isCollapsed }">
    <!-- Brand -->
    <div class="sidebar-brand">
      <span class="brand-icon">F</span>
      <div class="brand-text" v-show="!isCollapsed">
        <span class="brand-name">FactoryOps</span>
        <span class="brand-sub">Inventory</span>
      </div>
      <button class="sidebar-toggle" @click="toggle" :title="isCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <polyline v-if="isCollapsed" points="9 18 15 12 9 6"/>
          <polyline v-else points="15 18 9 12 15 6"/>
        </svg>
      </button>
    </div>

    <!-- Nav section label -->
    <div class="nav-section-label" v-show="!isCollapsed">Navigation</div>

    <!-- Nav links -->
    <nav class="sidebar-nav">
      <router-link to="/" active-class="" exact-active-class="nav-link-active" class="nav-link" data-tooltip="Dashboard">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">{{ t('nav.overview') }}</span>
      </router-link>

      <router-link to="/inventory" class="nav-link" data-tooltip="Inventory">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/><polyline points="3.27 6.96 12 12.01 20.73 6.96"/><line x1="12" y1="22.08" x2="12" y2="12"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">{{ t('nav.inventory') }}</span>
      </router-link>

      <router-link to="/orders" class="nav-link" data-tooltip="Orders">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">{{ t('nav.orders') }}</span>
      </router-link>

      <router-link to="/demand" class="nav-link" data-tooltip="Demand Forecast">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/><polyline points="17 6 23 6 23 12"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">{{ t('nav.demandForecast') }}</span>
      </router-link>

      <router-link to="/spending" class="nav-link" data-tooltip="Finance">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">{{ t('nav.finance') }}</span>
      </router-link>

      <router-link to="/reports" class="nav-link" data-tooltip="Reports">
        <span class="nav-icon">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/><line x1="2" y1="20" x2="22" y2="20"/>
          </svg>
        </span>
        <span class="nav-label" v-show="!isCollapsed">Reports</span>
      </router-link>
    </nav>

    <!-- Bottom area -->
    <div class="sidebar-bottom">
      <div v-show="!isCollapsed">
        <LanguageSwitcher />
      </div>
      <div v-show="!isCollapsed">
        <ProfileMenu
          @show-profile-details="$emit('show-profile-details')"
          @show-tasks="$emit('show-tasks')"
        />
      </div>
    </div>
  </aside>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'
import { useAuth } from '../composables/useAuth'
import { useI18n } from '../composables/useI18n'
import LanguageSwitcher from './LanguageSwitcher.vue'
import ProfileMenu from './ProfileMenu.vue'

export default {
  name: 'AppSidebar',
  components: {
    LanguageSwitcher,
    ProfileMenu
  },
  emits: ['show-profile-details', 'show-tasks', 'collapse-change'],
  setup(props, { emit }) {
    const { currentUser } = useAuth()
    const { t } = useI18n()

    const isCollapsed = ref(window.innerWidth < 1024)

    const toggle = () => {
      isCollapsed.value = !isCollapsed.value
      emit('collapse-change', isCollapsed.value)
    }

    const handleResize = () => {
      if (window.innerWidth < 1024) {
        isCollapsed.value = true
      }
    }

    onMounted(() => {
      window.addEventListener('resize', handleResize)
    })

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize)
    })

    return {
      currentUser,
      t,
      isCollapsed,
      toggle
    }
  }
}
</script>

<style scoped>
.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--sidebar-bg);
  display: flex;
  flex-direction: column;
  z-index: 100;
  border-right: 1px solid var(--sidebar-border);
  overflow-y: auto;
  transition: width 0.25s ease;
  overflow: visible;
}

.sidebar.collapsed {
  width: var(--sidebar-collapsed-width);
}

.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px 16px;
  border-bottom: 1px solid var(--sidebar-border);
  flex-shrink: 0;
  position: relative;
}

.brand-icon {
  width: 32px;
  height: 32px;
  background: var(--accent);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.875rem;
  color: white;
  flex-shrink: 0;
}

.brand-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: var(--sidebar-text-active);
  display: block;
  letter-spacing: -0.01em;
}

.brand-sub {
  font-size: 0.6875rem;
  color: var(--sidebar-text);
  display: block;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.nav-section-label {
  font-size: 0.625rem;
  font-weight: 700;
  color: var(--sidebar-text);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  padding: 20px 16px 8px;
  opacity: 0.5;
  overflow: hidden;
  transition: opacity 0.15s;
}

.sidebar-nav {
  flex: 1;
  padding: 4px 8px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 9px 10px;
  border-radius: 7px;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s, color 0.15s;
}

.nav-link:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.nav-link-active,
.nav-link.router-link-active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-text-active);
}

.nav-icon {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-label {
  flex: 1;
  overflow: hidden;
  white-space: nowrap;
  transition: opacity 0.2s;
}

.sidebar-bottom {
  flex-shrink: 0;
  border-top: 1px solid var(--sidebar-border);
  padding: 12px 8px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

/* Toggle button: floats at the right edge of the sidebar */
.sidebar-toggle {
  position: absolute;
  right: -12px;
  top: 50%;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #1e293b;
  border: 1px solid rgba(255, 255, 255, 0.12);
  color: #94a3b8;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
  transition: background 0.15s, color 0.15s, box-shadow 0.15s;
  flex-shrink: 0;
}

.sidebar-toggle:hover {
  background: #334155;
  color: #f1f5f9;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

/* In collapsed mode, center nav links */
.sidebar.collapsed .nav-link {
  justify-content: center;
  padding: 9px 0;
}

/* CSS tooltip that appears to the right of icons in collapsed mode */
.sidebar.collapsed .nav-link {
  position: relative;
}

.sidebar.collapsed .nav-link::after {
  content: attr(data-tooltip);
  position: absolute;
  left: calc(100% + 14px);
  top: 50%;
  transform: translateY(-50%);
  background: #1e293b;
  color: #f1f5f9;
  font-size: 0.75rem;
  font-weight: 500;
  padding: 5px 10px;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.sidebar.collapsed .nav-link:hover::after {
  opacity: 1;
}

/* Center brand icon when collapsed */
.sidebar.collapsed .sidebar-brand {
  justify-content: center;
  padding: 20px 0;
}
</style>
