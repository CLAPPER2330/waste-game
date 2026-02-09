# ♻️ WasteGame - Спаси планету!

Увлекательная игра на Vue 3 для экологичного образования. Замени одноразовые предметы на экологичные альтернативы и сэкономь планете!
eco-game/
├── src/
│   ├── components/
│   │   ├── GameBoard.vue
│   │   ├── TrashBin.vue
│   │   ├── AlternativesList.vue
│   │   ├── Timer.vue
│   │   └── Leaderboard.vue
│   ├── stores/
│   │   └── gameStore.js (Pinia)
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── package.json
└── vite.config.js{
  "name": "eco-game",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.3.0",
    "pinia": "^2.1.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.0.0",
    "vite": "^4.0.0"
  }
}import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useGameStore = defineStore('game', () => {
  // Game state
  const score = ref(0);
  const timeLeft = ref(120); // 2 minutes
  const gameStarted = ref(false);
  const gameOver = ref(false);
  const selectedAlternative = ref(null);
  const completedItems = ref([]);
  const leaderboard = ref(JSON.parse(localStorage.getItem('ecoLeaderboard')) || []);

  // Game items with alternatives
  const gameItems = [
    {
      id: 1,
      name: 'Пластиковый стаканчик',
      icon: '🥤',
      waste: 50,
      alternatives: [
        { id: 'a1', name: 'Кружка из нержавейки', icon: '☕', correct: true, savings: 300 },
        { id: 'a2', name: 'Бумажный стаканчик', icon: '📄', correct: false, savings: 0 },
        { id: 'a3', name: 'Керамическая чашка', icon: '🍵', correct: true, savings: 280 }
      ]
    },
    {
      id: 2,
      name: 'Мини-шампунь',
      icon: '🧴',
      waste: 30,
      alternatives: [
        { id: 'b1', name: 'Твёрдый шампунь в бумаге', icon: '📦', correct: true, savings: 200 },
        { id: 'b2', name: 'Шампунь в пластике', icon: '🥤', correct: false, savings: 0 },
        { id: 'b3', name: 'Натуральный шампунь', icon: '🌿', correct: true, savings: 180 }
      ]
    },
    {
      id: 3,
      name: 'Полиэтиленовый пакет',
      icon: '🛍️',
      waste: 40,
      alternatives: [
        { id: 'c1', name: 'Тканевая сумка-шоппер', icon: '👜', correct: true, savings: 150 },
        { id: 'c2', name: 'Бумажная сумка', icon: '📪', correct: true, savings: 100 },
        { id: 'c3', name: 'Пакет из биопластика', icon: '♻️', correct: false, savings: 0 }
      ]
    },
    {
      id: 4,
      name: 'Одноразовая расчёска',
      icon: '💇',
      waste: 20,
      alternatives: [
        { id: 'd1', name: 'Деревянная расчёска', icon: '🪵', correct: true, savings: 120 },
        { id: 'd2', name: 'Расчёска из кости', icon: '🦴', correct: true, savings: 110 },
        { id: 'd3', name: 'Пластиковая расчёска', icon: '🧬', correct: false, savings: 0 }
      ]
    },
    {
      id: 5,
      name: 'Одноразовая зубная щётка',
      icon: '🪥',
      waste: 15,
      alternatives: [
        { id: 'e1', name: 'Бамбуковая зубная щётка', icon: '🎋', correct: true, savings: 90 },
        { id: 'e2', name: 'Электрическая щётка (долговечная)', icon: '⚡', correct: true, savings: 100 },
        { id: 'e3', name: 'Пластиковая щётка', icon: '🧬', correct: false, savings: 0 }
      ]
    },
    {
      id: 6,
      name: 'Влажные салфетки',
      icon: '🧻',
      waste: 25,
      alternatives: [
        { id: 'f1', name: 'Тканевые салфетки', icon: '🧣', correct: true, savings: 140 },
        { id: 'f2', name: 'Платочки из микрофибры', icon: '👃', correct: true, savings: 130 },
        { id: 'f3', name: 'Одноразовые салфетки', icon: '📋', correct: false, savings: 0 }
      ]
    },
    {
      id: 7,
      name: 'Пластиковая бутылка',
      icon: '🍾',
      waste: 45,
      alternatives: [
        { id: 'g1', name: 'Металлическая фляга', icon: '🫖', correct: true, savings: 250 },
        { id: 'g2', name: 'Стеклянная бутылка', icon: '🥛', correct: true, savings: 220 },
        { id: 'g3', name: 'Биопластиковая бутылка', icon: '♻️', correct: false, savings: 0 }
      ]
    },
    {
      id: 8,
      name: 'Одноразовые контактные линзы',
      icon: '👁️',
      waste: 10,
      alternatives: [
        { id: 'h1', name: 'Многоразовые очки', icon: '👓', correct: true, savings: 80 },
        { id: 'h2', name: 'Переиспользуемые линзы', icon: '💧', correct: true, savings: 70 },
        { id: 'h3', name: 'Линзы в пластике', icon: '🥤', correct: false, savings: 0 }
      ]
    },
    {
      id: 9,
      name: 'Одноразовая посуда',
      icon: '🍽️',
      waste: 35,
      alternatives: [
        { id: 'i1', name: 'Фарфоровая посуда', icon: '🍴', correct: true, savings: 200 },
        { id: 'i2', name: 'Бамбуковая посуда', icon: '🎋', correct: true, savings: 180 },
        { id: 'i3', name: 'Пластиковая посуда', icon: '🧬', correct: false, savings: 0 }
      ]
    },
    {
      id: 10,
      name: 'Пластиковая соломинка',
      icon: '🥤',
      waste: 5,
      alternatives: [
        { id: 'j1', name: 'Стеклянная соломинка', icon: '🥽', correct: true, savings: 50 },
        { id: 'j2', name: 'Бамбуковая соломинка', icon: '🎋', correct: true, savings: 45 },
        { id: 'j3', name: 'Металлическая соломинка', icon: '🪛', correct: true, savings: 60 }
      ]
    }
  ];

  const currentItems = ref([...gameItems].sort(() => Math.random() - 0.5));

  // Computed
  const totalSavings = computed(() => {
    return completedItems.value.reduce((sum, item) => sum + item.savings, 0);
  });

  const completedCount = computed(() => completedItems.value.length);

  const isGameComplete = computed(() => completedItems.value.length === gameItems.length);

  // Methods
  const startGame = () => {
    score.value = 0;
    timeLeft.value = 120;
    gameStarted.value = true;
    gameOver.value = false;
    completedItems.value = [];
    selectedAlternative.value = null;
    currentItems.value = [...gameItems].sort(() => Math.random() - 0.5);
  };

  const selectAlternative = (alternative) => {
    selectedAlternative.value = alternative;
  };

  const confirmChoice = (itemId) => {
    if (!selectedAlternative.value) return;

    if (selectedAlternative.value.correct) {
      score.value += 1;
      completedItems.value.push({
        itemId,
        alternative: selectedAlternative.value.name,
        savings: selectedAlternative.value.savings
      });

      if (isGameComplete.value) {
        endGame();
      }
    }
    selectedAlternative.value = null;
  };

  const endGame = () => {
    gameOver.value = true;
    gameStarted.value = false;
    saveToLeaderboard();
  };

  const saveToLeaderboard = () => {
    const entry = {
      player: 'Player ' + Math.floor(Math.random() * 10000),
      score: score.value,
      savings: totalSavings.value,
      date: new Date().toLocaleString('ru-RU')
    };

    leaderboard.value.push(entry);
    leaderboard.value.sort((a, b) => b.score - a.score);
    leaderboard.value = leaderboard.value.slice(0, 10);
    localStorage.setItem('ecoLeaderboard', JSON.stringify(leaderboard.value));
  };

  const decrementTime = () => {
    if (timeLeft.value > 0) {
      timeLeft.value--;
    } else if (gameStarted.value) {
      endGame();
    }
  };

  return {
    score,
    timeLeft,
    gameStarted,
    gameOver,
    selectedAlternative,
    completedItems,
    currentItems,
    leaderboard,
    totalSavings,
    completedCount,
    isGameComplete,
    startGame,
    selectAlternative,
    confirmChoice,
    decrementTime,
    endGame,
    gameItems
  };
});<template>
  <div class="app">
    <header class="header">
      <h1>♻️ EcoGame - Спаси планету!</h1>
      <p class="subtitle">Замени одноразовые предметы на экологичные альтернативы</p>
    </header>

    <main class="main">
      <!-- Стартовый экран -->
      <div v-if="!gameStore.gameStarted && !gameStore.gameOver" class="start-screen">
        <div class="welcome-box">
          <h2>Добро пожаловать в EcoGame! 🌍</h2>
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
</style><template>
  <div class="game-board">
    <div class="items-container">
      <div class="item-card" v-for="item in gameStore.currentItems" :key="item.id">
        <div v-if="!isCompleted(item.id)" class="item-content">
          <div class="item-icon">{{ item.icon }}</div>
          <div class="item-name">{{ item.name }}</div>
          <button @click="selectItem(item)" class="btn-select">Выбрать</button>
        </div>
        <div v-else class="item-completed">
          <span class="checkmark">✓</span>
          <p>Готово!</p>
        </div>
      </div>
    </div>

    <!-- Модальное окно выбора альтернативы -->
    <div v-if="selectedItem" class="modal-overlay" @click.self="closeModal">
      <div class="modal">
        <button class="close-btn" @click="closeModal">✕</button>
        <h3>Замени {{ selectedItem.name }}</h3>
        <div class="icon-large">{{ selectedItem.icon }}</div>
        
        <div class="alternatives">
          <div
            v-for="alt in selectedItem.alternatives"
            :key="alt.id"
            class="alternative"
            :class="{ selected: selectedAlternative?.id === alt.id }"
            @click="gameStore.selectAlternative(alt)"
          >
            <span class="alt-icon">{{ alt.icon }}</span>
            <span class="alt-name">{{ alt.name }}</span>
          </div>
        </div>

        <div v-if="selectedAlternative" class="feedback">
          <p v-if="selectedAlternative.correct" class="correct">
            ✓ Отлично! Ты сэкономишь {{ selectedAlternative.savings }}г пластика в год!
          </p>
          <p v-else class="incorrect">
            ✗ Это не экологично. Попробуй снова!
          </p>
        </div>

        <button 
          @click="confirm" 
          class="btn btn-confirm"
          :disabled="!selectedAlternative"
        >
          Подтвердить
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useGameStore } from '../stores/gameStore';

const gameStore = useGameStore();
const selectedItem = ref(null);

const selectItem = (item) => {
  selectedItem.value = item;
  gameStore.selectAlternative(null);
};

const closeModal = () => {
  selectedItem.value = null;
  gameStore.selectAlternative(null);
};

const isCompleted = (itemId) => {
  return gameStore.completedItems.some(item => item.itemId === itemId);
};

const confirm = () => {
  if (gameStore.selectedAlternative?.correct) {
    gameStore.confirmChoice(selectedItem.value.id);
    closeModal();
  }
};

const selectedAlternative = () => gameStore.selectedAlternative;
</script>

<style scoped>
.game-board {
  width: 100%;
}

.items-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.item-card {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  padding: 1.5rem;
  text-align: center;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  min-height: 180px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.item-card:hover:not(.completed) {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
}

.item-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.8rem;
  width: 100%;
}

.item-icon {
  font-size: 3rem;
}

.item-name {
  font-weight: bold;
  font-size: 0.95rem;
  line-height: 1.3;
}

.btn-select {
  background: white;
  color: #667eea;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
  width: 100%;
}

.btn-select:hover {
  transform: scale(1.05);
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
}

.item-completed {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
}

.checkmark {
  font-size: 2.5rem;
  font-weight: bold;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.modal {
  background: white;
  border-radius: 15px;
  padding: 2rem;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  position: relative;
}

.close-btn {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: #999;
}

.modal h3 {
  color: #667eea;
  font-size: 1.5rem;
  margin-top: 0;
  text-align: center;
}

.icon-large {
  font-size: 4rem;
  text-align: center;
  margin: 1rem 0;
}

.alternatives {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  margin: 1.5rem 0;
}

.alternative {
  padding: 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.alternative:hover {
  border-color: #667eea;
  background: rgba(102, 126, 234, 0.05);
}

.alternative.selected {
  border-color: #667eea;
  background: rgba(102, 126, 234, 0.15);
}

.alt-icon {
  font-size: 1.5rem;
}

.alt-name {
  flex: 1;
  font-weight: 500;
}

.feedback {
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  text-align: center;
  font-weight: bold;
}

.correct {
  background: #e8f5e9;
  color: #2e7d32;
}

.incorrect {
  background: #ffebee;
  color: #c62828;
}

.btn-confirm {
  width: 100%;
  padding: 0.8rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-confirm:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
}

.btn-confirm:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .items-container {
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    gap: 0.8rem;
  }

  .modal {
    padding: 1.5rem;
  }

  .icon-large {
    font-size: 3rem;
  }
}
</style><template>
  <div class="timer">
    <div class="time-display">{{ formatTime }}</div>
  </div>
</template>

<script setup>
import { computed, onMounted, onUnmounted } from 'vue';
import { useGameStore } from '../stores/gameStore';

const gameStore = useGameStore();

const formatTime = computed(() => {
  const minutes = Math.floor(gameStore.timeLeft / 60);
  const seconds = gameStore.timeLeft % 60;
  return `${minutes}:${seconds.toString().padStart(2, '0')}`;
});

let interval;

onMounted(() => {
  interval = setInterval(() => {
    gameStore.decrementTime();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<style scoped>
.timer {
  display: flex;
  align-items: center;
  justify-content: center;
}

.time-display {
  font-size: 1.8rem;
  font-weight: bold;
  color: #667eea;
  background: #f5f5f5;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  min-width: 80px;
  text-align: center;
}
</style><template>
  <div class="leaderboard">
    <h3>🏆 Топ дня</h3>
    <table class="leaderboard-table">
      <thead>
        <tr>
          <th>Место</th>
          <th>Игрок</th>
          <th>Балл</th>
          <th>Сэкономлено</th>
          <th>Время</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(entry, index) in gameStore.leaderboard" :key="index">
          <td class="rank">{{ index + 1 }}</td>
          <td>{{ entry.player }}</td>
          <td class="score">{{ entry.score }}</td>
          <td>{{ entry.savings }}г</td>
          <td class="date">{{ entry.date }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { useGameStore } from '../stores/gameStore';

const gameStore = useGameStore();
</script>

<style scoped>
.leaderboard {
  background: white;
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  margin-top: 2rem;
}

.leaderboard h3 {
  color: #667eea;
  font-size: 1.5rem;
  margin-top: 0;
  text-align: center;
}

.leaderboard-table {
  width: 100%;
  border-collapse: collapse;
}

.leaderboard-table th {
  background: #f5f5f5;
  padding: 1rem;
  text-align: left;
  font-weight: bold;
  color: #667eea;
  border-bottom: 2px solid #e0e0e0;
}

.leaderboard-table td {
  padding: 0.8rem 1rem;
  border-bottom: 1px solid #f0f0f0;
}

.leaderboard-table tr:hover {
  background: #f9f9f9;
}

.rank {
  font-weight: bold;
  color: #667eea;
}

.score {
  font-weight: bold;
  font-size: 1.1rem;
}

.date {
  font-size: 0.9rem;
  color: #999;
}

@media (max-width: 768px) {
  .leaderboard {
    padding: 1rem;
  }

  .leaderboard-table {
    font-size: 0.9rem;
  }

  .leaderboard-table th,
  .leaderboard-table td {
    padding: 0.5rem;
  }

  .date {
    display: none;
  }
}
</style>import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)

app.use(createPinia())
app.mount('#app')import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
})<!DOCTYPE html>
<html lang="ru">
  <head>
    <meta charset="UTF-8">
    <link rel="icon" href="/favicon.ico">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoGame - Спаси планету!</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
