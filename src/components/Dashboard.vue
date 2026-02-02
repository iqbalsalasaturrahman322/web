<template>
  <div class="dashboard">
    <header class="dashboard-header">
      <h2 class="dashboard-title">Dashboard</h2>
      <p class="welcome-text">
        Selamat datang, <strong class="username">{{ username }}</strong> 🎉
      </p>
    </header>

    <div class="cards-container">
      <div 
        v-for="card in cards" 
        :key="card.id"
        class="card"
      >
        <div class="card-icon">{{ card.icon }}</div>
        <h3 class="card-title">{{ card.title }}</h3>
        <p class="card-description">{{ card.description }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  props: {
    username: {
      type: String,
      required: true,
      default: 'User'
    }
  },
  emits: ['logout'],
  data() {
    return {
      cards: [
        {
          id: 1,
          icon: '✅',
          title: 'Status',
          description: 'Kamu berhasil login'
        },
        {
          id: 2,
          icon: 'ℹ️',
          title: 'Info',
          description: 'Ini adalah dashboard sederhana menggunakan Vue.js'
        },
        {
          id: 3,
          icon: '📊',
          title: 'Data',
          description: 'Lihat statistik dan laporan'
        },
        {
          id: 4,
          icon: '⚙️',
          title: 'Pengaturan',
          description: 'Kelola akun dan preferensi'
        }
      ]
    }
  },
  methods: {
    handleLogout() {
      this.$emit('logout')
    }
  }
}
</script>

<style scoped>
.dashboard {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
  text-align: center;
}

.dashboard-header {
  margin-bottom: 40px;
}

.dashboard-title {
  font-size: 2.5rem;
  color: #2c3e50;
  margin: 0 0 16px 0;
  font-weight: 700;
}

.welcome-text {
  font-size: 1.2rem;
  color: #555;
  margin: 0;
}

.username {
  color: #3498db;
  font-weight: 600;
}

.cards-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  margin-bottom: 40px;
}

.card {
  background: white;
  padding: 32px 24px;
  border-radius: 16px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  cursor: pointer;
  border: 2px solid transparent;
}

.card:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
  border-color: #3498db;
}

.card-icon {
  font-size: 3rem;
  margin-bottom: 16px;
  display: inline-block;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.card:hover .card-icon {
  animation: none;
  transform: scale(1.2);
  transition: transform 0.3s ease;
}

.card-title {
  font-size: 1.4rem;
  color: #2c3e50;
  margin: 0 0 12px 0;
  font-weight: 600;
}

.card-description {
  font-size: 1rem;
  color: #7f8c8d;
  margin: 0;
  line-height: 1.6;
}

/* Responsive */
@media (max-width: 1024px) {
  .cards-container {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  }
}

@media (max-width: 768px) {
  .dashboard {
    padding: 24px 16px;
  }

  .dashboard-title {
    font-size: 2rem;
  }

  .welcome-text {
    font-size: 1rem;
  }

  .cards-container {
    grid-template-columns: 1fr;
    gap: 16px;
  }

  .card {
    padding: 24px 20px;
  }

  .card-icon {
    font-size: 2.5rem;
  }

  .card-title {
    font-size: 1.2rem;
  }
}

@media (max-width: 480px) {
  .dashboard {
    padding: 20px 12px;
  }

  .dashboard-title {
    font-size: 1.75rem;
  }

  .welcome-text {
    font-size: 0.95rem;
  }
}
</style>