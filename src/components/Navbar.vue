<template>
  <nav class="navbar">
    <h3 class="navbar-title">website sederhana</h3>
    <button class="logout-btn" @click="handleLogout">Logout</button>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  emits: ['logout'],
  methods: {
    handleLogout() {
      this.$emit('logout')
    }
  }
}
</script>

<style scoped>
.navbar {
  height: 50px;
  background: #2c3e50;
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
}

.navbar-title {
  margin: 0;
  font-size: 1.5rem;
}

.logout-btn {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 8px 16px;
  cursor: pointer;
  border-radius: 4px;
  font-size: 0.9rem;
  transition: background 0.3s ease;
}

.logout-btn:hover {
  background: #c0392b;
}

.logout-btn:active {
  transform: scale(0.98);
}
</style>