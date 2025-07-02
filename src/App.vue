<script setup>
import { ref, onMounted } from 'vue'

const currentSection = ref('home')
const isMenuOpen = ref(false)
const timeWorking = ref('')

// Функция для расчета времени работы чата
function calculateWorkingTime() {
  const startDate = new Date('2023-10-01')
  const now = new Date()
  const diffTime = Math.abs(now - startDate)
  const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24))
  const diffMonths = Math.floor(diffDays / 30)
  const remainingDays = diffDays % 30
  
  if (diffMonths > 0) {
    timeWorking.value = `${diffMonths} мес. ${remainingDays} дн.`
  } else {
    timeWorking.value = `${diffDays} дней`
  }
}

const sections = {
  home: 'Главная',
  admin: 'Администрация', 
  rules: 'Правила'
}

const adminTeam = [
  {
    id: 1,
    name: 'Kaneki Ken',
    role: 'Основатель',
    description: 'Основатель чата. Занимается вливанием новичков, слежкой за чатом и разработкой бота.',
    gradient: 'linear-gradient(135deg, #ff1744, #ad1457, #6a1b9a)',
    image: '/kaneki2.png'
  },
  {
    id: 2,
    name: 'Satoru Gojo',
    role: 'Владелец',
    description: 'Владелец чата. Ищет и принимает новичков, следит за чатом, занимается чисткой и монтажом.',
    gradient: 'linear-gradient(135deg, #9c27b0, #673ab7, #3f51b5)',
    image: '/gojo.png'
  },
  {
    id: 3,
    name: 'Giorno Giovanna',
    role: 'Совладелец',
    description: 'Совладелец и помощник Годжо.<br>Ищет новичков, помогает с чисткой и набором в админы.',
    gradient: 'linear-gradient(135deg, #ff5722, #ff9800, #ffc107)',
    image: '/djorno.png'
  },
  {
    id: 4,
    name: 'Komako Semenovich',
    role: 'Старший Админ',
    description: 'Старший администратор.<br>Занимается монтажом видео, поддерживает активность и следит за чатом.',
    gradient: 'linear-gradient(135deg, #666666, #888888, #aaaaaa)',
    image: '/komako.png'
  },
  {
    id: 5,
    name: 'Kisuke Urahara',
    role: 'Старший Админ',
    description: 'Монтаж видео и слежка за чатом.<br>(Старший админ потому что слишком пиздато и часто монтирует)',
    gradient: 'linear-gradient(135deg, #2e7d32, #388e3c, #4caf50)',
    image: '/urahara.png'
  }
]

const rulesCards = [
  {
    id: 1,
    title: 'Основные правила',
    describe: 'Правила поведения в флуд чате',
    rules: [
      '1. Если меняете юз, предупреждайте @kishimoro (варн за нарушение)',
      '2. Запрещен шокирующий и интимный контент (мут 2 часа)',
      '3. Запрещена коммерческая деятельность (варн)',
      '4. Не спамить в любом виде (варн)',
      '5. При выходе напишите админу своего персонажа',
      '6. Сраться по хуйне нельзя (варн)',
      '7. Неактивен более 4 дней (бан)',
      '8. Оскорбление администрации (варн)',
      '9. Разговоры только на русском (мут 30 мин)',
      '10. Выбирайте нормальные роли из фандомов',
      '11. Во время чистки нельзя писать (варн)',
      '12. Запрещена пропаганда нацизма, фашизма (варн)',
      '13. Обсуждение политики с негативом запрещено (мут 1 час)',
      '14. Рекламные ссылки запрещены (варн)',
      '15. Не переходить на личную жизнь (мут 5 часов)',
      '16. Можно писать влд для опроса об исключении',
      '17. Видите нарушение - пишите влд/адм',
      '18. Добавлять участников без разрешения запрещено (варн)',
      '19. Калл, закрепление, опросы только с разрешения (варн)',
      '20. Администрация выбирает меру пресечения',
      '21. Запрещено угрожать сватом, доксом (мут/бан)',
      '22. Не отпугивать нью и не оскорблять (варн)',
      '23. Незнание правил не освобождает от ответственности',
      '24. Не брать рест за день до чистки',
      '25. Возрастное ограничение с 13 лет',
      '26. Не ссориться из-за несогласия с правилами (варн)'
    ]
  },
  {
    id: 2,
    title: 'Голосовые чаты',
    describe: 'Правила поведения в ГЧ',
    rules: [
      '1. Запрещено перебивать участников и мешать общению',
      '2. Включать громкие звуки без разрешения (мут 3 часа)',
      '3. Оскорбление участников и их голоса (варн + мут в гч)',
      '4. Шокирующий контент (мут)'
    ]
  },
  {
    id: 3,
    title: 'Дополнительно',
    describe: 'Важная информация',
    rules: [
      'Норма сообщений: 150 в неделю',
      'Нью от 4 дней не банят',
      'Чистку проводит Годжо',
      'Помогает ему Джорно',
      'При нарушении обращайтесь к админам',
      'Соблюдайте правила и наслаждайтесь общением!'
    ]
  }
]

function setSection(section) {
  currentSection.value = section
  isMenuOpen.value = false
}

// Функция для обработки клика по кнопке с задержкой
function handleJoinClick(event) {
  event.preventDefault()
  
  // Добавляем класс загрузки
  const button = event.currentTarget
  button.classList.add('loading')
  
  // Ждем 1 секунду, затем переходим по ссылке
  setTimeout(() => {
    window.open('https://t.me/kishimoro', '_blank')
    button.classList.remove('loading')
  }, 1000)
}

onMounted(() => {
  // Добавляем класс для предотвращения зума на iOS
  document.documentElement.style.setProperty('--vh', `${window.innerHeight * 0.01}px`)
  
  window.addEventListener('resize', () => {
    document.documentElement.style.setProperty('--vh', `${window.innerHeight * 0.01}px`)
  })
  
  // Рассчитываем время работы чата
  calculateWorkingTime()
  
  // Обновляем каждый день
  setInterval(calculateWorkingTime, 24 * 60 * 60 * 1000)
})
</script>

<template>
  <div class="app">
    <!-- Навигация -->
    <nav class="navbar">
      <div class="nav-content">
        <div class="logo">
          <img src="/bow.png" alt="logo" class="logo-icon">
          <span class="logo-text">Nyashki Kaneki</span>
        </div>
        
        <div class="burger-container">
          <input 
            type="checkbox" 
            id="burger-checkbox"
            class="burger-checkbox"
            v-model="isMenuOpen"
          >
          <label for="burger-checkbox" class="burger"></label>
        </div>
      </div>
      
      <div class="nav-menu" :class="{ open: isMenuOpen }">
        <button 
          v-for="(label, key) in sections" 
          :key="key"
          @click="setSection(key)"
          class="nav-item"
          :class="{ active: currentSection === key }"
        >
          {{ label }}
        </button>
      </div>
    </nav>

    <!-- Основной контент -->
    <main class="main-content">
      <!-- Главная страница -->
      <section v-if="currentSection === 'home'" class="section home-section">
        <div class="hero">
          <div class="hero-content">
            <h1 class="hero-title">
              Добро пожаловать в 
              <span class="gradient-text">Nyashki Kaneki</span>
            </h1>
            <p class="hero-subtitle">
              Самый пиздатый флуд в Telegram
            </p>
            <div class="working-time">
              <div class="time-badge">
                <span class="time-label">Работаем уже</span>
                <span class="time-value">{{ timeWorking }}</span>
                <span class="time-since">с октября 2023</span>
              </div>
            </div>
            <div class="join-action">
              <div class="button-wrapper">
                <a href="https://t.me/kishimoro" target="_blank" class="spiderverse-button" @click="handleJoinClick">
                  <span class="glitch-text">Вступить в чат</span>
                  <div class="glitch-layers">
                    <div class="glitch-layer layer-1">Вступить в чат</div>
                    <div class="glitch-layer layer-2">Вступить в чат</div>
                  </div>
                  <div class="noise"></div>
                  <div class="glitch-slice"></div>
                </a>
              </div>
            </div>
          </div>
          <div class="hero-visual">
          </div>
        </div>
      </section>

      <!-- Администрация -->
      <section v-if="currentSection === 'admin'" class="section admin-section">
        <h2 class="section-title">
          Администрация
        </h2>
        <p class="section-subtitle">Наша команда делает чат лучше каждый день</p>
        
        <div class="admin-grid">
          <div 
            v-for="admin in adminTeam" 
            :key="admin.id"
            class="admin-card"
            :style="{ background: admin.gradient }"
          >
            <!-- Основная информация -->
            <div class="admin-header">
              <div class="admin-initials">{{ admin.name.split(' ').map(n => n[0]).join('') }}</div>
              <p class="admin-role-text">{{ admin.role }}</p>
            </div>
            
            <!-- Имя и описание -->
            <div class="admin-info">
              <h3 class="admin-name">{{ admin.name }}</h3>
              <p class="admin-description" v-html="admin.description"></p>
            </div>
            
            <!-- Кнопка действия -->
            <button class="admin-contact-btn">
              Связаться
              <svg class="contact-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M8 12H16M12 8V16M22 12C22 17.5228 17.5228 22 12 22C6.47715 22 2 17.5228 2 12C2 6.47715 6.47715 2 12 2C17.5228 2 22 6.47715 22 12Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </button>
            
            <!-- Изображение персонажа -->
            <img 
              :src="admin.image" 
              :alt="admin.name"
              class="admin-character-image scale-on-hover"
            />
          </div>
    </div>
      </section>

      <!-- Правила -->
      <section v-if="currentSection === 'rules'" class="section rules-section">
        <h2 class="section-title">
          Правила чата
        </h2>
        <p class="section-subtitle">Заходя в флуд, вы соглашаетесь со всеми правилами</p>
        
        <div class="container scroll-1">
          <div 
            v-for="card in rulesCards" 
            :key="card.id"
            class="card"
          >
            <div class="card__image"></div>
            <div class="card__content">
              <h3 class="card__title">{{ card.title }}</h3>
              <p class="card__describe">{{ card.describe }}</p>
              <div class="rules-list">
                <div 
                  v-for="(rule, index) in card.rules" 
                  :key="index"
                  class="rule-item"
                >
                  {{ rule }}
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <div class="scroll-hint">
          <span>← Свайпните для просмотра всех правил →</span>
        </div>
      </section>


  </main>
  </div>
</template>

<style>
/* Глобальные стили для удаления белой рамки */
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  background: #1a0b2e;
  overflow-x: hidden;
}

#app {
  margin: 0;
  padding: 0;
  min-height: 100vh;
}
</style>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  min-height: 100vh;
  width: 100vw;
  background: 
    linear-gradient(135deg, rgba(75, 0, 130, 0.0) 0%, rgba(106, 27, 154, 0.20) 25%, rgba(138, 43, 226, 0.20) 50%, rgba(156, 39, 176, 0.20) 75%, rgba(123, 31, 162, 0.20) 100%),
    url('/mainwallpaper.jpeg');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  background-repeat: no-repeat;
  color: white;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  overflow-x: hidden;
  position: relative;
}

/* Навигация */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: rgba(26, 11, 46, 0.95);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  max-width: 1200px;
  margin: 0 auto;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 700;
  font-size: 1.2rem;
}

.logo-icon {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

.logo-text {
  background: linear-gradient(45deg, #ff6b9d, #c44dff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.burger-container {
  display: flex;
  align-items: center;
}

.burger-checkbox {
  position: absolute;
  visibility: hidden;
}

.burger {
  cursor: pointer;
  display: block;
  position: relative;
  border: none;
  background: transparent;
  width: 40px;
  height: 26px;
  margin: 0;
}

.burger::before,
.burger::after {
  content: '';
  left: 0;
  position: absolute;
  display: block;
  width: 100%;
  height: 4px;
  border-radius: 10px;
  background: #fff;
}

.burger::before {
  top: 0;
  box-shadow: 0 11px 0 #fff;
  transition: box-shadow .3s .15s, top .3s .15s, transform .3s;
}

.burger::after {
  bottom: 0;
  transition: bottom .3s .15s, transform .3s;
}

.burger-checkbox:checked + .burger::before {
  top: 11px;
  transform: rotate(45deg);
  box-shadow: 0 6px 0 rgba(255,255,255,0);
  transition: box-shadow .15s, top .3s, transform .3s .15s;
}

.burger-checkbox:checked + .burger::after {
  bottom: 11px;
  transform: rotate(-45deg);
  transition: bottom .3s, transform .3s .15s;
}

.nav-menu {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: rgba(26, 11, 46, 0.98);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  transform: translateY(-100%);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.nav-menu.open {
  transform: translateY(0);
  opacity: 1;
  visibility: visible;
}

.nav-item {
  padding: 1rem;
  background: none;
  border: none;
  color: white;
  font-size: 1.1rem;
  cursor: pointer;
  text-align: left;
  transition: all 0.3s ease;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.nav-item:hover,
.nav-item.active {
  background: linear-gradient(90deg, rgba(255, 107, 157, 0.2), rgba(196, 77, 255, 0.2));
  color: #ff6b9d;
}

/* Основной контент */
.main-content {
  margin-top: 80px;
  min-height: calc(100vh - 80px);
}

.section {
  padding: 2rem 1rem;
  max-width: 1200px;
  margin: 0 auto;
}

/* Главная страница */
.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 3rem 0;
  position: relative;
}

.hero-content {
  position: relative;
}

.hero-title {
  font-size: 2.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
  line-height: 1.2;
}

.gradient-text {
  background: linear-gradient(45deg, #ff6b9d, #c44dff, #4dabf7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: gradientShift 3s ease-in-out infinite;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.hero-subtitle {
  font-size: 1.2rem;
  opacity: 0.9;
  margin-bottom: 1.5rem;
}

.working-time {
  margin-bottom: 2rem;
}

.time-badge {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 16px;
  padding: 1.5rem 2rem;
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
  transition: all 0.3s ease;
  animation: glow 3s ease-in-out infinite alternate;
}

.time-badge:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.4);
}

@keyframes glow {
  from {
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3), 0 0 20px rgba(255, 107, 157, 0.3);
  }
  to {
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3), 0 0 30px rgba(196, 77, 255, 0.4);
  }
}

.time-label {
  font-size: 0.9rem;
  opacity: 0.8;
  margin-bottom: 0.5rem;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.time-value {
  font-size: 2rem;
  font-weight: 700;
  background: linear-gradient(45deg, #ff6b9d, #c44dff, #4dabf7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.3rem;
}

.time-since {
  font-size: 0.8rem;
  opacity: 0.7;
}

.join-action {
  margin-top: 2rem;
}

/* Glitch button styles */
.button-wrapper {
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.2s ease;
  padding: 40px;
}

.spiderverse-button {
  position: relative;
  padding: 15px 30px;
  font-size: 18px;
  font-weight: 900;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  background: #fff;
  color: #000;
  text-transform: uppercase;
  letter-spacing: 2px;
  transform-style: preserve-3d;
  transition: all 0.15s ease;
  font-family: 'Inter', Arial, sans-serif;
  text-decoration: none;
  display: inline-block;
  text-shadow:
    -1px -1px 0 #000,
    1px -1px 0 #000,
    -1px 1px 0 #000,
    1px 1px 0 #000;
}

.glitch-text {
  position: relative;
  display: inline-block;
}

.glitch-layers {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.glitch-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  border-radius: 50px;
  opacity: 0;
  transition: all 0.15s ease;
}

.layer-1 {
  color: #0ff;
  transform-origin: center;
}

.layer-2 {
  color: #f0f;
  transform-origin: center;
}

.button-wrapper:hover .layer-1 {
  opacity: 1;
  animation: glitchLayer1 0.4s steps(2) infinite;
}

.button-wrapper:hover .layer-2 {
  opacity: 1;
  animation: glitchLayer2 0.4s steps(2) infinite;
}

.button-wrapper:hover .spiderverse-button {
  animation: buttonGlitch 0.3s steps(2) infinite;
  box-shadow:
    0 0 20px rgba(255, 255, 255, 0.5),
    0 0 30px rgba(0, 255, 255, 0.5),
    0 0 40px rgba(255, 0, 255, 0.5);
}

.noise {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: repeating-radial-gradient(
    circle at 50% 50%,
    transparent 0,
    rgba(0, 0, 0, 0.1) 1px,
    transparent 2px
  );
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s;
  animation: noise 0.2s steps(2) infinite;
}

.button-wrapper:hover .noise {
  opacity: 1;
}

/* Loading state styles */
.spiderverse-button.loading {
  animation: buttonGlitch 0.2s steps(2) infinite;
  box-shadow:
    0 0 25px rgba(255, 255, 255, 0.8),
    0 0 35px rgba(0, 255, 255, 0.8),
    0 0 45px rgba(255, 0, 255, 0.8);
  pointer-events: none;
}

.spiderverse-button.loading .layer-1 {
  opacity: 1;
  animation: glitchLayer1 0.3s steps(2) infinite;
}

.spiderverse-button.loading .layer-2 {
  opacity: 1;
  animation: glitchLayer2 0.3s steps(2) infinite;
}

.spiderverse-button.loading .noise {
  opacity: 1;
}

@keyframes buttonGlitch {
  0% {
    transform: translate(0) scale(1.1);
  }
  25% {
    transform: translate(-10px, 5px) scale(1.15) skew(-5deg);
  }
  50% {
    transform: translate(10px, -5px) scale(1.1) skew(5deg);
  }
  75% {
    transform: translate(-15px, -5px) scale(1.05) skew(-3deg);
  }
  100% {
    transform: translate(0) scale(1.1);
  }
}

@keyframes glitchLayer1 {
  0% {
    transform: translate(-20px, -10px) scale(1.1) skew(-10deg);
    clip-path: polygon(0 20%, 100% 20%, 100% 50%, 0 50%);
  }
  25% {
    transform: translate(20px, 10px) scale(1.2) skew(10deg);
    clip-path: polygon(0 30%, 100% 30%, 100% 60%, 0 60%);
  }
  50% {
    transform: translate(-15px, 5px) scale(0.9) skew(-5deg);
    clip-path: polygon(0 10%, 100% 10%, 100% 40%, 0 40%);
  }
  75% {
    transform: translate(15px, -5px) scale(1.3) skew(5deg);
    clip-path: polygon(0 40%, 100% 40%, 100% 70%, 0 70%);
  }
  100% {
    transform: translate(-20px, -10px) scale(1.1) skew(-10deg);
    clip-path: polygon(0 20%, 100% 20%, 100% 50%, 0 50%);
  }
}

@keyframes glitchLayer2 {
  0% {
    transform: translate(20px, 10px) scale(1.1) skew(10deg);
    clip-path: polygon(0 50%, 100% 50%, 100% 80%, 0 80%);
  }
  25% {
    transform: translate(-20px, -10px) scale(1.2) skew(-10deg);
    clip-path: polygon(0 60%, 100% 60%, 100% 90%, 0 90%);
  }
  50% {
    transform: translate(15px, -5px) scale(0.9) skew(5deg);
    clip-path: polygon(0 40%, 100% 40%, 100% 70%, 0 70%);
  }
  75% {
    transform: translate(-15px, 5px) scale(1.3) skew(-5deg);
    clip-path: polygon(0 70%, 100% 70%, 100% 100%, 0 100%);
  }
  100% {
    transform: translate(20px, 10px) scale(1.1) skew(10deg);
    clip-path: polygon(0 50%, 100% 50%, 100% 80%, 0 80%);
  }
}

@keyframes noise {
  0% {
    transform: translate(0, 0);
  }
  10% {
    transform: translate(-5%, -5%);
  }
  20% {
    transform: translate(10%, 5%);
  }
  30% {
    transform: translate(-5%, 10%);
  }
  40% {
    transform: translate(15%, -5%);
  }
  50% {
    transform: translate(-10%, 15%);
  }
  60% {
    transform: translate(5%, -10%);
  }
  70% {
    transform: translate(-15%, 5%);
  }
  80% {
    transform: translate(10%, 10%);
  }
  90% {
    transform: translate(-5%, 15%);
  }
  100% {
    transform: translate(0, 0);
  }
}

.glitch-slice {
  position: absolute;
  width: 120%;
  height: 5px;
  background: #fff;
  opacity: 0;
  animation: slice 3s linear infinite;
}

@keyframes slice {
  0% {
    top: -10%;
    opacity: 0;
  }
  1% {
    opacity: 0.5;
  }
  3% {
    opacity: 0;
  }
  50% {
    top: 110%;
  }
  100% {
    top: 110%;
  }
}



.hero-visual {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
}

/* Заголовки секций */
.section-title {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 1rem;
  text-align: center;
}



.section-subtitle {
  text-align: center;
  opacity: 0.8;
  margin-bottom: 3rem;
  font-size: 1.1rem;
}

/* Администрация */
.admin-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem 0;
}

.admin-card {
  position: relative;
  width: 288px;
  height: 208px;
  border-radius: 16px;
  border: 4px solid rgba(255, 255, 255, 0.15);
  overflow: hidden;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 16px;
  transition: all 0.3s ease;
  transform: rotate(-12deg);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  margin: 30px auto;
}

.admin-card:hover {
  transform: rotate(0deg);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
}

/* Верхняя часть - инициалы и роль */
.admin-header {
  color: rgba(255, 255, 255, 0.9);
  z-index: 10;
}

.admin-initials {
  font-weight: 700;
  font-size: 3rem;
  line-height: 1;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  margin-bottom: 4px;
}

.admin-role-text {
  font-size: 0.75rem;
  opacity: 0.9;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin: 0;
}

/* Основная информация */
.admin-info {
  flex-grow: 1;
  z-index: 10;
}

  .admin-name {
    font-size: 1.1rem;
    font-weight: 600;
    color: white;
    margin: 0 0 8px 0;
    text-shadow: 2px 2px 6px rgba(0, 0, 0, 0.9), 
                 1px 1px 3px rgba(0, 0, 0, 0.8),
                 0 0 8px rgba(0, 0, 0, 0.6);
    -webkit-text-stroke: 0.5px rgba(0, 0, 0, 0.4);
  }
  
  .admin-description {
    font-size: 0.8rem;
    line-height: 1.4;
    color: rgba(255, 255, 255, 0.95);
    margin: 0;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.9),
                 1px 1px 2px rgba(0, 0, 0, 0.7),
                 0 0 6px rgba(0, 0, 0, 0.5);
    font-weight: 500;
  }

/* Кнопка связаться */
.admin-contact-btn {
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.3);
  background: rgba(255, 255, 255, 0.9);
  font-weight: 600;
  color: #1a0b2e;
  padding: 8px 12px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.8rem;
  cursor: pointer;
  z-index: 10;
  position: relative;
}

.admin-contact-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  transform: translateY(-2px);
}

.contact-icon {
  width: 16px;
  height: 16px;
  transition: transform 0.3s ease;
}

.admin-contact-btn:hover .contact-icon {
  transform: rotate(90deg);
}

/* Изображения персонажей */
.admin-character-image {
  position: absolute;
  bottom: -10px;
  right: -20px;
  width: 140px;
  height: 140px;
  object-fit: cover;
  object-position: center top;
  border-radius: 50% 50% 20% 20%;
  z-index: 1;
  transition: all 0.3s ease;
  pointer-events: none;
  opacity: 0.8;
  filter: brightness(1.1) contrast(1.1);
}

.scale-on-hover {
  transition: transform 0.3s ease;
}

.admin-card:hover .scale-on-hover {
  transform: scale(1.15) translateY(-5px);
  opacity: 1;
  filter: brightness(1.2) contrast(1.2);
}

/* Правила */
.scroll-1::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

.scroll-1::-webkit-scrollbar-thumb {
  border-radius: 20px;
  background: #888;
}

.container {
  display: flex;
  overflow-x: scroll;
  padding: 24px;
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  scroll-snap-type: x mandatory;
  scroll-padding-top: 24px;
  border-radius: 8px;
  gap: 20px;
}

.container * {
  box-sizing: border-box;
  color: #fff;
}

.container .card {
  flex: 0 0 300px;
  overflow: hidden;
  border-radius: 8px;
  background-color: #141414;
  scroll-snap-align: start;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.container .card:hover {
  transform: translateY(-5px);
  border-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.card .card__image {
  flex: 1;
  height: 100px;
}

.card:nth-child(1) .card__image {
  background-image: linear-gradient(
      to right top,
      #051937, 
      #004d7a, 
      #008793, 
      #00bf72, 
      #a8eb12
  );
}

.card:nth-child(2) .card__image {
  background-image: linear-gradient(
    to right top, 
    #dc09a5, 
    #ce00b4, 
    #ba00c5, 
    #9c00d8, 
    #6f12eb
  );
}

.card:nth-child(3) .card__image {
  background-image: linear-gradient(
    to right top, 
    #dc1009, 
    #e55f00, 
    #e49200, 
    #dac000, 
    #c7eb12
  );
}

.card .card__content {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding: 20px;
  max-height: 400px;
  overflow-y: auto;
}

.card .card__content::-webkit-scrollbar {
  width: 4px;
}

.card .card__content::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
}

.card .card__content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
}

.card .card__content .card__title {
  font-size: 24px;
  color: #fff;
  text-transform: capitalize;
  font-weight: 700;
}

.card .card__content .card__describe {
  color: #ccc;
  font-size: 14px;
  margin-bottom: 10px;
}

.rules-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.rule-item {
  font-size: 12px;
  line-height: 1.4;
  color: #e0e0e0;
  padding: 6px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.rule-item:last-child {
  border-bottom: none;
}

.scroll-hint {
  text-align: center;
  margin-top: 1rem;
  opacity: 0.7;
  font-size: 0.9rem;
  font-style: italic;
}



/* Адаптивность */
@media (min-width: 768px) {
  .nav-menu {
    position: static;
    transform: none;
    opacity: 1;
    visibility: visible;
    background: none;
    border: none;
    flex-direction: row;
    gap: 1rem;
  }
  
  .burger-container {
    display: none;
  }
  
  .nav-item {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 25px;
  }
  
  .hero-title {
    font-size: 3.5rem;
  }
  
  .time-badge {
    padding: 2rem 2.5rem;
  }
  
  .time-value {
    font-size: 2.5rem;
  }
  
  .spiderverse-button {
    padding: 18px 35px;
    font-size: 20px;
  }
  
  .button-wrapper {
    padding: 30px;
  }
  
  .admin-grid {
    grid-template-columns: 1fr;
    padding: 1rem 0;
  }
  
  .admin-card {
    width: 100%;
    max-width: 320px;
    margin: 20px auto;
  }
  
  .container {
    max-width: 100%;
    padding: 20px;
  }
  
  .container .card {
    flex: 0 0 280px;
  }
  
  .rule-item {
    font-size: 13px;
  }
}

@media (min-width: 1024px) {
  .section {
    padding: 3rem 2rem;
  }
  
  .hero {
    padding: 4rem 0;
  }
  
  .admin-grid {
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    padding: 2rem 0;
  }
  
  .admin-card {
    width: 288px;
    margin: 30px auto;
  }
}

/* Плавные переходы для мобильных устройств */
@media (max-width: 767px) {
  .app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  .spiderverse-button {
    touch-action: manipulation;
    -webkit-tap-highlight-color: transparent;
    padding: 12px 24px;
    font-size: 16px;
  }
  
  .button-wrapper {
    padding: 20px;
  }
  
  .container {
    padding: 16px;
    gap: 15px;
  }
  
  .container .card {
    flex: 0 0 250px;
  }
  
  .card .card__content {
    padding: 15px;
    max-height: 350px;
  }
  
  .card .card__content .card__title {
    font-size: 20px;
  }
  
  .rule-item {
    font-size: 11px;
  }
}
</style>
