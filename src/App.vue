<template>
  <div class="app">
    <header class="header">
      <h1>♻️ WasteGame - Спаси планету!</h1>
      <p class="subtitle">Замени одноразовые предметы на экологичные альтернативы</p>
    </header>

    <main class="main">
      <!-- Стартовый экран -->
      <div v-if="!gameStore.gameStarted && !gameStore.gameOver" class="start-screen">
        <div class="welcome-box">
          <h2>Добро пожаловать в WasteGame! 🌍</h2>
          <p>У тебя есть 2 минуты, чтобы заменить 10 одноразовых предметов на экологичные альтернативы.</p>
          <p><strong>+1 балл</strong> за каждый правильный выбор!</p>
          <button @click="gameStore.startGame" class="btn btn-primary">Начать игру</button>
        </div>
        <Leaderboard v-if="gameStore.leaderboard.length > 0" />
      </div>

      <!-- Игровой процесс -->
      <div v-if="gameStore.gameStarted" class="game-container">
        <div class="game-header">
          <div class="stats">
            <div class="stat">
              <span class="stat-label">Балл:</span>
              <span class="stat-value">{{ gameStore.score }}/{{ gameStore.gameItems.length }}</span>
            </div>
            <Timer />
            <div class="stat">
              <span class="stat-label">Сэкономлено:</span>
              <span class="stat-value">{{ gameStore.totalSavings }}г</span>
            </div>
          </div>
        </div>

        <div class="game-content">
          <GameBoard />
        </div>
      </div>

      <!-- Экран завершения -->
      <div v-if="gameStore.gameOver" class="end-screen">
        <div class="result-box">
          <h2>Игра завершена! 🎉</h2>
          <div class="result-stats">
            <div class="result-stat">
              <span class="result-label">Итоговый балл:</span>
              <span class="result-value">{{ gameStore.score }}/{{ gameStore.gameItems.length }}</span>
            </div>
            <div class="result-stat">
              <span class="result-label">Ты спас:</span>
              <span class="result-value">{{ gameStore.totalSavings }}г пластика в год!</span>
            </div>
          </div>
          <button @click="gameStore.startGame" class="btn btn-primary">Играть ещё раз</button>
        </div>

        <Leaderboard v-if="gameStore.leaderboard.length > 0" />
      </div>
    </main>
  </div>
</template>

<script setup>
import { useGameStore } from './stores/gameStore';
import GameBoard from './components/GameBoard.vue';
import Timer from './components/Timer.vue';
import Leaderboard from './components/Leaderboard.vue';

const gameStore = useGameStore();
</script>

<style scoped>
.app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #333;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header {
  background: rgba(255, 255, 255, 0.1);
  padding: 2rem;
  text-align: center;
  color: white;
  border-bottom: 2px solid rgba(255, 255, 255, 0.2);
}

.header h1 {
  margin: 0;
  font-size: 2.5rem;
}

.subtitle {
  margin: 0.5rem 0 0 0;
  font-size: 1.1rem;
  opacity: 0.9;
}

.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.start-screen,
.end-screen {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.welcome-box,
.result-box {
  background: white;
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  text-align: center;
}

.welcome-box h2,
.result-box h2 {
  font-size: 2rem;
  color: #667eea;
  margin-top: 0;
}

.welcome-box p,
.result-box p {
  font-size: 1.1rem;
  line-height: 1.6;
  color: #555;
}

.btn {
  padding: 0.8rem 2rem;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
  margin-top: 1rem;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
}

.game-container {
  background: white;
  border-radius: 15px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.game-header {
  margin-bottom: 2rem;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 1rem;
}

.stats {
  display: flex;
  justify-content: space-around;
  gap: 1rem;
}

.stat {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-label {
  font-size: 0.9rem;
  color: #999;
  text-transform: uppercase;
}

.stat-value {
  font-size: 1.8rem;
  font-weight: bold;
  color: #667eea;
}

.result-stats {
  margin: 1.5rem 0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.result-stat {
  padding: 1rem;
  background: #f5f5f5;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.result-label {
  font-size: 1.1rem;
  font-weight: bold;
}

.result-value {
  font-size: 1.5rem;
  color: #667eea;
  font-weight: bold;
}

@media (max-width: 768px) {
  .header h1 {
    font-size: 1.8rem;
  }

  .stats {
    flex-direction: column;
  }

  .welcome-box,
  .result-box {
    padding: 1.5rem;
  }

  .welcome-box h2,
  .result-box h2 {
    font-size: 1.5rem;
  }
}
</style>
