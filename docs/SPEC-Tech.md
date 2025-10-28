# Tecnologias Recomendadas para River Raid Clone + Firebase

## 1. Stack Principal Recomendada

### Opção A: Vanilla JavaScript + HTML5 Canvas (RECOMENDADO)
**Por que escolher:**
- ✅ Simplicidade e controle total sobre o jogo
- ✅ Performance excelente para jogos 2D
- ✅ Sem dependências externas ou build process
- ✅ Deploy direto no Firebase Hosting
- ✅ Melhor para aprendizado de game dev

**Estrutura do projeto:**
```
river-raid/
├── public/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   ├── game.js (loop principal)
│   │   ├── player.js
│   │   ├── enemy.js
│   │   ├── collision.js
│   │   ├── ui.js
│   │   ├── audio.js
│   │   └── utils.js
│   ├── assets/
│   │   ├── sounds/
│   │   └── sprites/ (opcional)
│   └── firebase-config.js
├── .firebaserc
├── firebase.json
└── README.md
```

### Opção B: Phaser.js Framework
**Por que escolher:**
- ✅ Framework game engine completo
- ✅ Sistema de física integrado
- ✅ Gerenciamento de assets facilitado
- ✅ Grande comunidade e documentação
- ⚠️ Curva de aprendizado maior
- ⚠️ Overhead para projeto simples

### Opção C: PixiJS (Renderização 2D)
**Por que escolher:**
- ✅ Performance superior com WebGL
- ✅ Bom para muitas sprites/partículas
- ✅ Mais leve que Phaser
- ⚠️ Requer mais código manual para game logic

## 2. Firebase - Serviços Necessários

### Firebase Hosting
**Para hospedar o jogo:**
```bash
# Instalar Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Inicializar projeto
firebase init hosting

# Deploy
firebase deploy
```

**Configuração firebase.json:**
```json
{
  "hosting": {
    "public": "public",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

### Firebase Realtime Database (para Leaderboard)
**Estrutura de dados:**
```json
{
  "leaderboard": {
    "easy": {
      "userId1": {
        "name": "ACE_PILOT",
        "score": 125400,
        "date": 1698528000000,
        "combo": 10
      }
    },
    "normal": { ... },
    "hard": { ... }
  },
  "players": {
    "userId1": {
      "name": "ACE_PILOT",
      "achievements": [1, 2, 5, 8],
      "totalGames": 45,
      "bestScore": 125400
    }
  }
}
```

**Regras de segurança:**
```json
{
  "rules": {
    "leaderboard": {
      "$difficulty": {
        ".read": true,
        "$userId": {
          ".write": "auth != null && auth.uid == $userId",
          ".validate": "newData.hasChildren(['name', 'score', 'date'])"
        }
      }
    }
  }
}
```

### Firebase Authentication (Opcional)
**Para identificar jogadores:**
- Anonymous Auth (mais simples - recomendado)
- Email/Password
- Google Sign-In

### Firestore (Alternativa ao Realtime Database)
**Vantagens:**
- Queries mais complexas
- Melhor escalabilidade
- Estrutura mais organizada

**Quando usar:**
- Se precisar de filtros avançados
- Leaderboards por região/país
- Histórico detalhado de partidas

## 3. Setup do Projeto - Passo a Passo

### Passo 1: Criar estrutura HTML básica
```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>River Raid Clone</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div id="game-container">
        <canvas id="gameCanvas" width="800" height="600"></canvas>
        <div id="ui-overlay">
            <!-- HUD elements aqui -->
        </div>
    </div>

    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-database-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth-compat.js"></script>
    
    <!-- Seu código -->
    <script src="firebase-config.js"></script>
    <script src="js/game.js"></script>
</body>
</html>
```

### Passo 2: Configurar Firebase
```javascript
// public/firebase-config.js
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "seu-projeto.firebaseapp.com",
  databaseURL: "https://seu-projeto.firebaseio.com",
  projectId: "seu-projeto",
  storageBucket: "seu-projeto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};

// Inicializar Firebase
firebase.initializeApp(firebaseConfig);

// Referências
const database = firebase.database();
const auth = firebase.auth();

// Auth anônimo (automático)
auth.signInAnonymously().catch(error => {
  console.error("Erro no auth:", error);
});
```

### Passo 3: Implementar Leaderboard
```javascript
// js/leaderboard.js
class Leaderboard {
  constructor() {
    this.database = firebase.database();
    this.currentDifficulty = 'normal';
  }

  async submitScore(playerName, score, difficulty) {
    const userId = firebase.auth().currentUser.uid;
    const timestamp = Date.now();
    
    try {
      await this.database.ref(`leaderboard/${difficulty}/${userId}`).set({
        name: playerName,
        score: score,
        date: timestamp,
        difficulty: difficulty
      });
      
      return true;
    } catch (error) {
      console.error("Erro ao enviar score:", error);
      return false;
    }
  }

  async getTopScores(difficulty, limit = 100) {
    try {
      const snapshot = await this.database
        .ref(`leaderboard/${difficulty}`)
        .orderByChild('score')
        .limitToLast(limit)
        .once('value');
      
      const scores = [];
      snapshot.forEach(child => {
        scores.push({
          userId: child.key,
          ...child.val()
        });
      });
      
      // Ordenar descendente
      return scores.reverse();
    } catch (error) {
      console.error("Erro ao buscar scores:", error);
      return [];
    }
  }

  async getPlayerRank(userId, difficulty) {
    const allScores = await this.getTopScores(difficulty, 1000);
    const index = allScores.findIndex(s => s.userId === userId);
    return index >= 0 ? index + 1 : -1;
  }
}
```

### Passo 4: Sistema de Achievements (localStorage)
```javascript
// js/achievements.js
class AchievementManager {
  constructor() {
    this.achievements = this.loadAchievements();
  }

  loadAchievements() {
    const saved = localStorage.getItem('river-raid-achievements');
    return saved ? JSON.parse(saved) : {};
  }

  saveAchievements() {
    localStorage.setItem('river-raid-achievements', 
      JSON.stringify(this.achievements));
  }

  unlock(achievementId) {
    if (!this.achievements[achievementId]) {
      this.achievements[achievementId] = {
        unlocked: true,
        date: Date.now()
      };
      this.saveAchievements();
      this.showNotification(achievementId);
      return true;
    }
    return false;
  }

  showNotification(achievementId) {
    // Implementar UI de notificação
    console.log(`Achievement unlocked: ${achievementId}`);
  }

  isUnlocked(achievementId) {
    return this.achievements[achievementId]?.unlocked || false;
  }

  getProgress() {
    const total = 30; // Total de achievements
    const unlocked = Object.keys(this.achievements).length;
    return { unlocked, total, percentage: (unlocked / total) * 100 };
  }
}
```

## 4. Bibliotecas Auxiliares Recomendadas

### Para Áudio
```html
<!-- Howler.js - Gerenciamento de som -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/howler/2.2.3/howler.min.js"></script>
```

```javascript
// Exemplo de uso
const sounds = {
  shoot: new Howl({ src: ['assets/sounds/shoot.mp3'] }),
  explosion: new Howl({ src: ['assets/sounds/explosion.mp3'] }),
  music: new Howl({ 
    src: ['assets/sounds/music.mp3'],
    loop: true,
    volume: 0.5
  })
};

sounds.shoot.play();
```

### Para Efeitos Visuais
```javascript
// Particles.js (opcional)
// Para sistemas de partículas mais complexos
```

### Para Detecção de Colisão
```javascript
// SAT.js (Separating Axis Theorem)
// Se precisar de colisões complexas
// Para River Raid, AABB simples é suficiente
```

## 5. Build e Deploy

### Comandos essenciais:
```bash
# Desenvolvimento local
firebase serve

# Deploy para produção
firebase deploy

# Deploy apenas hosting
firebase deploy --only hosting

# Ver logs
firebase hosting:logs
```

### Otimizações pré-deploy:
```bash
# Minificar JS (opcional)
npm install -g terser
terser js/game.js -o js/game.min.js

# Otimizar imagens
npm install -g imagemin-cli
imagemin assets/sprites/* --out-dir=assets/sprites/optimized
```

## 6. Estrutura Recomendada Final

```
river-raid-clone/
├── public/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   ├── core/
│   │   │   ├── game.js
│   │   │   ├── engine.js
│   │   │   └── constants.js
│   │   ├── entities/
│   │   │   ├── player.js
│   │   │   ├── enemy.js
│   │   │   ├── projectile.js
│   │   │   └── powerup.js
│   │   ├── systems/
│   │   │   ├── collision.js
│   │   │   ├── spawner.js
│   │   │   └── particles.js
│   │   ├── ui/
│   │   │   ├── hud.js
│   │   │   ├── menu.js
│   │   │   └── leaderboard.js
│   │   ├── managers/
│   │   │   ├── audio-manager.js
│   │   │   ├── achievement-manager.js
│   │   │   └── game-state.js
│   │   └── utils/
│   │       ├── math.js
│   │       └── helpers.js
│   ├── assets/
│   │   ├── sounds/
│   │   └── sprites/
│   └── firebase-config.js
├── .firebaserc
├── firebase.json
├── .gitignore
└── README.md
```

## 7. Alternativas ao Firebase

### Opção 1: Vercel
- Deploy gratuito e simples
- Edge functions para backend
- Sem banco de dados integrado (usar Supabase separado)

### Opção 2: Netlify
- Similar ao Vercel
- Netlify Functions para backend
- Identity para autenticação

### Opção 3: GitHub Pages
- Totalmente gratuito
- Apenas arquivos estáticos
- Sem backend (usar Firebase apenas para dados)

## Recomendação Final

**Para seu projeto River Raid Clone:**

✅ **Frontend:** Vanilla JavaScript + HTML5 Canvas  
✅ **Hosting:** Firebase Hosting  
✅ **Leaderboard:** Firebase Realtime Database  
✅ **Auth:** Firebase Anonymous Auth  
✅ **Achievements:** localStorage (dados locais)  
✅ **Audio:** Howler.js  
✅ **Deploy:** Firebase CLI  

**Cusas estimados:** Grátis no plano Spark do Firebase (até limites generosos para um jogo indie)

Quer que eu crie um template inicial do projeto com essa estrutura?