<template>
  <div id="app">
    <Navbar v-if="isLoggedIn" @logout="handleLogout" />
    
    <div class="main-layout" v-if="isLoggedIn">
      <Sidebar @menu-change="handleMenuChange" />
      
      <main class="content">
        <Dashboard
          :username="formData.username"
          @logout="handleLogout"
        />
      </main>
    </div>

    <!-- LOGIN FORM -->
    <div v-else class="login-container">
      <div class="login-card">
        <div class="login-header">
          <h2 class="login-title">Selamat Datang</h2>
          <p class="login-subtitle">Silakan login untuk melanjutkan</p>
        </div>

        <form @submit.prevent="handleLogin" class="login-form">
          <div class="form-group">
            <label for="username" class="form-label">Username</label>
            <input
              id="username"
              v-model.trim="formData.username"
              type="text"
              class="form-input"
              placeholder="Masukkan username"
              required
              autocomplete="username"
            />
          </div>

          <div class="form-group">
            <label for="password" class="form-label">Password</label>
            <input
              id="password"
              v-model="formData.password"
              type="password"
              class="form-input"
              placeholder="Masukkan password"
              required
              autocomplete="current-password"
            />
          </div>

          <button 
            type="submit" 
            class="login-btn"
            :disabled="isLoading"
          >
            <span v-if="!isLoading">Login</span>
            <span v-else>Loading...</span>
          </button>

          <transition name="fade">
            <div v-if="error" class="error-message">
              <span class="error-icon">⚠️</span>
              {{ error }}
            </div>
          </transition>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Dashboard from './components/Dashboard.vue'
import Navbar from './components/Navbar.vue'
import Sidebar from './components/Sidebar.vue'

export default {
  name: 'App',
  components: {
    Dashboard,
    Navbar,
    Sidebar
  },
  data() {
    return {
      formData: {
        username: '',
        password: ''
      },
      isLoggedIn: false,
      isLoading: false,
      error: '',
      currentMenu: 'dashboard'
    }
  },
  methods: {
    async handleLogin() {
      this.error = ''
      this.isLoading = true

      await new Promise(resolve => setTimeout(resolve, 500))

      if (!this.formData.username || !this.formData.password) {
        this.error = 'Username dan password harus diisi!'
        this.isLoading = false
        return
      }

      if (this.formData.username === 'admin' && this.formData.password === '1234') {
        this.isLoggedIn = true
        this.error = ''
        
        localStorage.setItem('isLoggedIn', 'true')
        localStorage.setItem('username', this.formData.username)
      } else {
        this.error = 'Username atau password salah!'
      }

      this.isLoading = false
    },

    handleLogout() {
      this.isLoggedIn = false
      this.formData.username = ''
      this.formData.password = ''
      this.error = ''
      this.currentMenu = 'dashboard'
      
      localStorage.removeItem('isLoggedIn')
      localStorage.removeItem('username')
    },

    handleMenuChange(menuId) {
      this.currentMenu = menuId
      console.log('Menu changed to:', menuId)
    }
  },

  mounted() {
    const savedLogin = localStorage.getItem('isLoggedIn')
    const savedUsername = localStorage.getItem('username')
    
    if (savedLogin === 'true' && savedUsername) {
      this.isLoggedIn = true
      this.formData.username = savedUsername
    }
  }
}
</script>

<style>
/* Global Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  height: 100%;
  overflow-x: hidden;
}

#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  height: 100%;
  background: #ecf0f1;
}

/* Main Layout - PERBAIKAN UTAMA */
.main-layout {
  display: flex;
  height: calc(100vh - 50px);
  width: 100%;
  overflow: hidden;
}

.content {
  flex: 1;
  overflow-y: auto;
  background: #ecf0f1;
  width: 100%;
}

/* Login Container */
.login-container {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.login-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  padding: 40px;
  width: 100%;
  max-width: 420px;
  animation: slideUp 0.4s ease-out;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.login-header {
  text-align: center;
  margin-bottom: 32px;
}

.login-title {
  font-size: 1.8rem;
  color: #2c3e50;
  margin-bottom: 8px;
  font-weight: 600;
}

.login-subtitle {
  color: #7f8c8d;
  font-size: 0.95rem;
}

/* Form Styles */
.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-size: 0.9rem;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 4px;
}

.form-input {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 1rem;
  transition: all 0.3s ease;
  outline: none;
}

.form-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-input::placeholder {
  color: #bdc3c7;
}

.login-btn {
  width: 100%;
  padding: 14px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 8px;
}

.login-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(102, 126, 234, 0.4);
}

.login-btn:active:not(:disabled) {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* Error Message */
.error-message {
  background: #fee;
  color: #c33;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 8px;
  border-left: 4px solid #e74c3c;
}

.error-icon {
  font-size: 1.2rem;
}

/* Fade Transition */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

/* Login Hint */
.login-hint {
  text-align: center;
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid #ecf0f1;
}

.login-hint p {
  font-size: 0.85rem;
  color: #7f8c8d;
}

.login-hint code {
  background: #ecf0f1;
  padding: 2px 6px;
  border-radius: 4px;
  font-family: 'Courier New', monospace;
  color: #667eea;
  font-weight: 600;
}

/* Responsive */
@media (max-width: 768px) {
  .login-card {
    padding: 30px 24px;
  }

  .login-title {
    font-size: 1.5rem;
  }

  .main-layout {
    flex-direction: column;
    height: auto;
  }
  
  .content {
    min-height: calc(100vh - 50px);
  }
}
</style>