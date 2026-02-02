<template>
  <aside class="sidebar">
    <nav>
      <ul class="sidebar-menu">
        <li 
          v-for="item in menuItems" 
          :key="item.id"
          class="menu-item"
          :class="{ active: activeMenu === item.id }"
          @click="handleMenuClick(item.id)"
        >
          <span class="menu-icon">{{ item.icon }}</span>
          <span class="menu-text">{{ item.label }}</span>
        </li>
      </ul>
    </nav>
  </aside>
</template>

<script>
export default {
  name: 'Sidebar',
  emits: ['menu-change'],
  data() {
    return {
      activeMenu: 'dashboard',
      menuItems: [
        { id: 'dashboard', label: 'Dashboard', icon: '📊' },
        { id: 'profile', label: 'Profile', icon: '👤' },
        { id: 'settings', label: 'Settings', icon: '⚙️' }
      ]
    }
  },
  methods: {
    handleMenuClick(menuId) {
      this.activeMenu = menuId
      this.$emit('menu-change', menuId)
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 200px;
  background: #34495e;
  color: white;
  height: calc(100vh - 50px);
  padding-top: 20px;
  overflow-y: auto;
}

.sidebar-menu {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-item {
  padding: 12px 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 12px;
  transition: background 0.2s ease;
  user-select: none;
}

.menu-item:hover {
  background: #2c3e50;
}

.menu-item.active {
  background: #2c3e50;
  border-left: 3px solid #3498db;
  padding-left: 17px;
}

.menu-icon {
  font-size: 1.2rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 24px;
}

.menu-text {
  font-size: 0.95rem;
  font-weight: 500;
}

/* Scrollbar styling */
.sidebar::-webkit-scrollbar {
  width: 6px;
}

.sidebar::-webkit-scrollbar-track {
  background: #2c3e50;
}

.sidebar::-webkit-scrollbar-thumb {
  background: #555;
  border-radius: 3px;
}

.sidebar::-webkit-scrollbar-thumb:hover {
  background: #777;
}
</style>