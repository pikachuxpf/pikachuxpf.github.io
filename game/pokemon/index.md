---
title: 宝可梦记忆游戏
date: 2024-02-15 19:00:00
type: "game"
layout: "page"
---

<div class="pokemon-memory-game">
  <div class="game-header">
    <h1 class="game-title">宝可梦记忆游戏</h1>
    <div class="game-stats">
      <div class="stat-item">
        <span class="stat-label">步数:</span>
        <span id="moves" class="stat-value">0</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">时间:</span>
        <span id="timer" class="stat-value">00:00</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">最佳:</span>
        <span id="best-score" class="stat-value">--</span>
      </div>
    </div>
  </div>

  <div class="game-controls">
    <button id="new-game" class="btn btn-primary">新游戏</button>
    <select id="difficulty" class="difficulty-select">
      <option value="easy">简单 (4x3)</option>
      <option value="medium" selected>中等 (4x4)</option>
      <option value="hard">困难 (6x4)</option>
    </select>
  </div>

  <div class="game-board" id="game-board">
    <!-- 游戏卡片将通过JavaScript动态生成 -->
  </div>

  <div class="game-modal" id="win-modal">
    <div class="modal-content">
      <h2>恭喜你赢了！</h2>
      <div class="modal-stats">
        <p>步数: <span id="final-moves">0</span></p>
        <p>用时: <span id="final-time">00:00</span></p>
      </div>
      <button id="play-again" class="btn btn-primary">再玩一次</button>
    </div>
  </div>
</div>

<style>
.pokemon-memory-game {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.game-header {
  text-align: center;
  margin-bottom: 30px;
}

.game-title {
  color: var(--post_heading_color);
  margin-bottom: 20px;
  font-size: 2.5rem;
  position: relative;
  display: inline-block;
}

.game-title::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 100%;
  height: 3px;
  background: linear-gradient(90deg, #ffcc00, #ff9900);
  border-radius: 3px;
}

.game-stats {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-bottom: 20px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: var(--board-color);
  padding: 10px 15px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.stat-label {
  font-size: 0.9rem;
  color: var(--sec_text_color);
  margin-bottom: 5px;
}

.stat-value {
  font-size: 1.2rem;
  font-weight: bold;
  color: var(--post_heading_color);
}

.game-controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  margin-bottom: 30px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.btn-primary {
  background: linear-gradient(45deg, #ff9900, #ffcc00);
  color: white;
  box-shadow: 0 3px 10px rgba(255, 153, 0, 0.3);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(255, 153, 0, 0.4);
}

.difficulty-select {
  padding: 10px 15px;
  border: 2px solid var(--line_color);
  border-radius: 5px;
  font-size: 1rem;
  background: var(--board-color);
  color: var(--post_text_color);
  cursor: pointer;
}

.game-board {
  display: grid;
  gap: 15px;
  margin: 0 auto;
  perspective: 1000px;
}

.card {
  position: relative;
  height: 120px;
  cursor: pointer;
  transform-style: preserve-3d;
  transition: transform 0.6s;
}

.card.flipped {
  transform: rotateY(180deg);
}

.card.matched {
  animation: matchAnimation 0.6s ease;
  pointer-events: none;
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.card-front {
  background: linear-gradient(45deg, #ff9900, #ffcc00);
  border: 2px solid #ff9900;
}

.card-front::before {
  content: '?';
  font-size: 3rem;
  color: white;
  font-weight: bold;
}

.card-back {
  background: white;
  border: 2px solid #ffcc00;
  transform: rotateY(180deg);
  padding: 10px;
}

.card-back img {
  max-width: 80%;
  max-height: 80%;
  object-fit: contain;
}

.card:hover .card-front {
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.game-modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 1000;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: var(--board-color);
  padding: 30px;
  border-radius: 15px;
  text-align: center;
  max-width: 400px;
  box-shadow: 0 5px 25px rgba(0, 0, 0, 0.2);
}

.modal-content h2 {
  color: var(--post_heading_color);
  margin-top: 0;
  margin-bottom: 20px;
}

.modal-stats {
  margin-bottom: 20px;
}

.modal-stats p {
  margin: 10px 0;
  font-size: 1.1rem;
  color: var(--post_text_color);
}

@keyframes matchAnimation {
  0% { transform: rotateY(180deg) scale(1); }
  50% { transform: rotateY(180deg) scale(1.1); }
  100% { transform: rotateY(180deg) scale(1); }
}

@media (max-width: 768px) {
  .game-stats {
    flex-wrap: wrap;
    gap: 15px;
  }
  
  .game-controls {
    flex-direction: column;
  }
  
  .card {
    height: 80px;
  }
  
  .card-front::before {
    font-size: 2rem;
  }
  
  .game-board {
    gap: 10px;
  }
}
</style>

<script>
document.addEventListener('DOMContentLoaded', () => {
  // 游戏状态
  let cards = [];
  let flippedCards = [];
  let matchedPairs = 0;
  let moves = 0;
  let timer = 0;
  let timerInterval = null;
  let gameStarted = false;
  let currentDifficulty = 'medium';
  
  // 游戏配置
  const difficulties = {
    easy: { rows: 3, cols: 4 },
    medium: { rows: 4, cols: 4 },
    hard: { rows: 4, cols: 6 }
  };
  
  // 宝可梦图片
  const pokemonImages = [
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/25.png', // 皮卡丘
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/1.png',  // 妙蛙种子
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/4.png',  // 小火龙
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/7.png',  // 杰尼龟
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/150.png', // 超梦
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/143.png', // 卡比兽
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/131.png', // 拉达
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/133.png', // 伊布
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/149.png', // 快龙
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/94.png',  // 耿鬼
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/65.png',  // 阿柏蛇
    'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/132.png'  // 百变怪
  ];
  
  // DOM 元素
  const gameBoard = document.getElementById('game-board');
  const movesElement = document.getElementById('moves');
  const timerElement = document.getElementById('timer');
  const bestScoreElement = document.getElementById('best-score');
  const newGameButton = document.getElementById('new-game');
  const difficultySelect = document.getElementById('difficulty');
  const winModal = document.getElementById('win-modal');
  const playAgainButton = document.getElementById('play-again');
  const finalMovesElement = document.getElementById('final-moves');
  const finalTimeElement = document.getElementById('final-time');
  
  // 初始化游戏
  function initGame() {
    const { rows, cols } = difficulties[currentDifficulty];
    const totalCards = rows * cols;
    const pairs = totalCards / 2;
    
    // 清空游戏板
    gameBoard.innerHTML = '';
    gameBoard.style.gridTemplateColumns = `repeat(${cols}, 1fr)`;
    
    // 重置游戏状态
    cards = [];
    flippedCards = [];
    matchedPairs = 0;
    moves = 0;
    timer = 0;
    gameStarted = false;
    
    // 更新显示
    movesElement.textContent = moves;
    timerElement.textContent = formatTime(timer);
    
    // 清除计时器
    if (timerInterval) {
      clearInterval(timerInterval);
      timerInterval = null;
    }
    
    // 创建卡片数组
    const selectedPokemon = pokemonImages.slice(0, pairs);
    const cardPairs = [...selectedPokemon, ...selectedPokemon];
    
    // 洗牌
    for (let i = cardPairs.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [cardPairs[i], cardPairs[j]] = [cardPairs[j], cardPairs[i]];
    }
    
    // 创建卡片DOM
    cardPairs.forEach((pokemon, index) => {
      const card = createCard(pokemon, index);
      cards.push(card);
      gameBoard.appendChild(card.element);
    });
    
    // 加载最佳分数
    loadBestScore();
  }
  
  // 创建卡片
  function createCard(pokemon, index) {
    const cardElement = document.createElement('div');
    cardElement.className = 'card';
    cardElement.dataset.index = index;
    cardElement.dataset.pokemon = pokemon;
    
    const cardFront = document.createElement('div');
    cardFront.className = 'card-face card-front';
    
    const cardBack = document.createElement('div');
    cardBack.className = 'card-face card-back';
    
    const img = document.createElement('img');
    img.src = pokemon;
    img.alt = 'Pokemon';
    img.loading = 'lazy';
    
    cardBack.appendChild(img);
    cardElement.appendChild(cardFront);
    cardElement.appendChild(cardBack);
    
    // 添加点击事件
    cardElement.addEventListener('click', () => flipCard(cardElement));
    
    return {
      element: cardElement,
      pokemon: pokemon,
      isFlipped: false,
      isMatched: false
    };
  }
  
  // 翻转卡片
  function flipCard(cardElement) {
    // 如果已经翻开或已匹配，则返回
    if (cardElement.classList.contains('flipped') || 
        cardElement.classList.contains('matched') || 
        flippedCards.length >= 2) {
      return;
    }
    
    // 开始游戏计时
    if (!gameStarted) {
      startTimer();
      gameStarted = true;
    }
    
    // 翻转卡片
    cardElement.classList.add('flipped');
    flippedCards.push(cardElement);
    
    // 如果翻开了两张卡片，检查是否匹配
    if (flippedCards.length === 2) {
      moves++;
      movesElement.textContent = moves;
      checkForMatch();
    }
  }
  
  // 检查是否匹配
  function checkForMatch() {
    const [card1, card2] = flippedCards;
    const pokemon1 = card1.dataset.pokemon;
    const pokemon2 = card2.dataset.pokemon;
    
    if (pokemon1 === pokemon2) {
      // 匹配成功
      setTimeout(() => {
        card1.classList.add('matched');
        card2.classList.add('matched');
        matchedPairs++;
        
        // 检查是否获胜
        if (matchedPairs === cards.length / 2) {
          endGame();
        }
        
        flippedCards = [];
      }, 500);
    } else {
      // 匹配失败
      setTimeout(() => {
        card1.classList.remove('flipped');
        card2.classList.remove('flipped');
        flippedCards = [];
      }, 1000);
    }
  }
  
  // 开始计时
  function startTimer() {
    timerInterval = setInterval(() => {
      timer++;
      timerElement.textContent = formatTime(timer);
    }, 1000);
  }
  
  // 格式化时间
  function formatTime(seconds) {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  }
  
  // 结束游戏
  function endGame() {
    clearInterval(timerInterval);
    
    // 更新最终统计
    finalMovesElement.textContent = moves;
    finalTimeElement.textContent = formatTime(timer);
    
    // 保存最佳分数
    saveBestScore();
    
    // 显示胜利弹窗
    setTimeout(() => {
      winModal.style.display = 'flex';
    }, 500);
  }
  
  // 保存最佳分数
  function saveBestScore() {
    const key = `pokemon_memory_best_${currentDifficulty}`;
    const currentBest = localStorage.getItem(key);
    
    if (!currentBest || moves < parseInt(currentBest)) {
      localStorage.setItem(key, moves.toString());
      bestScoreElement.textContent = moves;
    }
  }
  
  // 加载最佳分数
  function loadBestScore() {
    const key = `pokemon_memory_best_${currentDifficulty}`;
    const best = localStorage.getItem(key);
    bestScoreElement.textContent = best || '--';
  }
  
  // 事件监听器
  newGameButton.addEventListener('click', initGame);
  
  difficultySelect.addEventListener('change', (e) => {
    currentDifficulty = e.target.value;
    initGame();
  });
  
  playAgainButton.addEventListener('click', () => {
    winModal.style.display = 'none';
    initGame();
  });
  
  // 初始化游戏
  initGame();
});
</script>