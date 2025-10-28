# Especificações - Sistema de Combo (Fase 14)

## 1. Mecânica de Combo

### Condições de Ativação
- **Trigger inicial**: destruir 3 ou mais inimigos consecutivos
- **Janela de tempo**: 2.5 segundos entre cada kill para manter o combo
- **Máximo de combo**: 10x multiplicador
- **Reset**: após 3 segundos sem destruir inimigos

### Níveis de Combo
```
3 kills  → Combo x2  (pontos × 2)
5 kills  → Combo x3  (pontos × 3)
7 kills  → Combo x4  (pontos × 4)
10 kills → Combo x5  (pontos × 5)
15 kills → Combo x7  (pontos × 7)
20 kills → Combo x10 (pontos × 10)
```

### Sistema de Pontuação
- **Pontos base mantidos**: helicóptero 100pts, tanque 150pts, barco 200pts
- **Aplicação do multiplicador**: pontos_base × multiplicador_combo
- **Bônus de quebra**: ao perder combo ≥5x, ganhar 500 pontos extras
- **Super combo (≥10x)**: cada kill adicional +1000 pontos fixos

## 2. Indicador Visual de Combo

### Posicionamento Dinâmico
- **Posição inicial**: x=15px, y=canvas.height/2-50px (esquerda-centro)
- **Comportamento**: segue levemente a nave do jogador (offset Y suave)
- **Aparecimento**: slide-in da esquerda (0.3s) ao ativar combo
- **Desaparecimento**: fade-out + scale down (0.5s) ao resetar

### Container Principal

**Dimensões base:**
- Largura: 140px
- Altura: 100px (escala com nível de combo)

**Estilo visual:**
- Fundo: gradiente radial do centro
  - Combo 2-3x: amarelo (#ffff00) → laranja (#ff8800)
  - Combo 4-6x: laranja (#ff8800) → vermelho (#ff0000)
  - Combo 7-10x: vermelho (#ff0000) → roxo (#aa00ff)
- Borda: branca (#ffffff) 4px
- Brilho externo (glow): cor do gradiente, 10px blur
- Cantos: arredondados 12px
- Pulso: escala 1.0→1.05→1.0 (0.5s loop)

### Elementos do Indicador

**Label "COMBO" (topo):**
- Posição: x=70px (centro), y=15px
- Fonte: Arial Black, 16px
- Cor: branco (#ffffff)
- Sombra: preta (#000000) 2px
- Efeito: leve balanço horizontal (-2px ↔ +2px)

**Multiplicador principal:**
- Formato: "×N"
- Posição: centralizado, y=45px
- Fonte: Impact, 48px
- Cor: branco (#ffffff)
- Contorno: preto (#000000) 3px
- Sombra: cor do gradiente, 4px
- Animação a cada kill:
  - Scale 1.0 → 1.4 → 1.0 (0.3s)
  - Rotação -5° → 0° → 5° → 0° (0.3s)

**Contador de kills:**
- Formato: "X kills"
- Posição: centralizado, y=75px
- Fonte: Arial Bold, 14px
- Cor: amarelo claro (#ffff88)
- Atualiza instantaneamente

**Barra de tempo restante:**
- Posição: fundo do container, y=90px
- Dimensões: 120×6px (centralizada)
- Fundo: cinza escuro (#333333)
- Preenchimento: gradiente igual ao container
- Animação: depleta da direita para esquerda em 2.5s
- Efeito crítico (<0.5s): pisca vermelho

## 3. Efeitos Visuais Especiais

### Ao Ativar Combo (3 kills)
- Flash branco em toda tela (0.1s, 30% opacidade)
- Onda expansiva circular da nave (amarelo)
- Som: "whoosh" ascendente
- Texto flutuante: "COMBO START!" (fade up & out, 1s)

### Ao Aumentar Nível de Combo
- Partículas coloridas explodem do indicador (8-12 partículas)
- Partículas:
  - Tamanho: 4-8px
  - Cor: mesma do gradiente atual
  - Velocidade: radial, 150-250px/s
  - Vida: 0.8s (fade out)
- Screen shake leve: 3px magnitude, 0.2s
- Som: "ding" mais agudo a cada nível

### Combo Crítico (≥7x)
- **Aura na nave do jogador:**
  - Círculo pulsante ao redor
  - Cor: corresponde ao gradiente
  - Raio: 50-65px (pulso)
  - Opacidade: 40%
  - Partículas trail: deixa rastro colorido
  
- **Estrelas de fundo:**
  - Spawn de 3-5 estrelas decorativas por segundo
  - Forma: estrelas 5 pontas, 10-15px
  - Cor: branco-amarelo brilhante
  - Movimento: parallax lento
  - Fade out após 2s

### Ao Perder Combo (≥5x)
- Flash vermelho na tela (0.2s, 20% opacidade)
- Indicador "quebra" em 3-4 fragmentos
- Fragmentos caem com gravidade e fade out (0.6s)
- Texto flutuante: "COMBO BROKEN!" (vermelho, 1.2s)
- Som: "crash" descendente triste
- Se combo era ≥5x: "+500 BONUS" em amarelo aparece

### Super Combo (10x - Máximo)
- **Fundo do jogo:**
  - Overlay de scan lines horizontais (efeito CRT)
  - Leve chromatic aberration (separação RGB)
  - Pulsos de luz sincronizados com música
  
- **Indicador especial:**
  - Substitui container por versão "premium"
  - Fundo: arco-íris animado girando
  - Estrelas pequenas orbitando (8 estrelas)
  - Texto: "MAX COMBO!" acima do multiplicador
  - Tamanho aumentado: 160×120px

- **Projéteis aprimorados:**
  - Cor: arco-íris gradient
  - Rastro de partículas
  - Largura aumentada 30%

## 4. Sistema de Áudio para Combo

### Sons Base
- **Combo ativado (3 kills)**: synth ascendente, 0.3s, frequência 440Hz→880Hz
- **Level up (+2/3 kills)**: "blip" agudo, 0.1s, pitch aumenta com nível
- **Combo quebrado**: som descendente "wah-wah", 0.5s
- **Max combo (10x)**: fanfarra épica, 1s, múltiplas camadas

### Música Adaptativa
- **Normal (sem combo)**: música base em 100% volume
- **Combo ativo (3-6x)**: adiciona camada de percussão (+20% volume)
- **Combo alto (7-9x)**: adiciona camada de synth melódico (+30% volume)
- **Max combo (10x)**: música full mix com todos os layers (150% volume)
- **Transição**: crossfade suave 0.5s entre estados

---

# Especificações - Extras e Power-ups (Fase 16)

## 1. Sistema de Power-ups

### Tipos de Power-ups

#### 1.1 Escudo (Shield)
**Aparência:**
- Forma: hexágono 28×28px
- Cor: ciano brilhante (#00ffff)
- Borda: branca (#ffffff) 2px
- Ícone interno: "S" estilizado
- Animação idle: rotação lenta 360° em 4s

**Efeito:**
- Duração: 8 segundos
- Proteção: 1 hit de inimigo ou obstáculo
- Visual na nave: dome semi-transparente ciano (60px raio)
- Animação do dome: pulso suave + shimmer effect
- Ao receber dano: escudo pisca 3x e desaparece

**Spawn:**
- Frequência: 1 a cada 1200-1800px de scroll
- Posição: aleatória no rio (evita margens)

#### 1.2 Arma Dupla (Double Shot)
**Aparência:**
- Forma: dois retângulos paralelos 20×12px
- Cor: laranja (#ff8800)
- Borda: amarela (#ffff00) 2px
- Ícone: duas setas para cima "⇈"
- Animação idle: pulso de brilho

**Efeito:**
- Duração: 10 segundos
- Disparo: 2 projéteis simultâneos (espaçamento 20px)
- Projéteis: mesmas características, trajetórias paralelas
- Visual na nave: canhões duplos visíveis nas asas
- Indicador: "2×" pequeno acima da nave

**Spawn:**
- Frequência: 1 a cada 1500-2000px
- Posição: centro do rio preferencialmente

#### 1.3 Arma Tripla (Triple Shot)
**Aparência:**
- Forma: três triângulos em leque 24×24px
- Cor: roxo (#aa00ff)
- Borda: rosa (#ff00ff) 2px
- Ícone: três setas divergentes "⬉⬆⬈"
- Animação idle: rotação pulsante

**Efeito:**
- Duração: 8 segundos
- Disparo: 3 projéteis (centro + ±25° ângulo)
- Cobertura: área mais ampla
- Visual na nave: canhão triplo central
- Indicador: "3×" acima da nave

**Spawn:**
- Frequência: 1 a cada 2500-3500px (raro)
- Posição: após destruir 5+ inimigos sem dano

#### 1.4 Velocidade Aumentada (Speed Boost)
**Aparência:**
- Forma: raio/relâmpago 22×30px estilizado
- Cor: amarelo elétrico (#ffff00)
- Borda: branca com glow
- Animação idle: faíscas elétricas ao redor

**Efeito:**
- Duração: 6 segundos
- Velocidade nave: +60% horizontal e vertical
- Visual: rastro de partículas amarelas atrás da nave
- Projéteis: velocidade +40%
- Indicador: chevrons "⟫" animados na nave

**Spawn:**
- Frequência: 1 a cada 1800-2500px
- Posição: próximo a áreas densas de inimigos

#### 1.5 Bomba/Smart Bomb
**Aparência:**
- Forma: círculo 26px com cruz interna
- Cor: vermelho (#ff0000)
- Borda: amarela piscante
- Ícone: explosão estilizada
- Animação idle: pulso de alerta

**Efeito:**
- Uso: ativação manual (tecla B)
- Quantidade: 1 uso por coleta (acumula até 3)
- Ação: destrói TODOS inimigos na tela
- Animação: onda expansiva branca desde a nave
- Pontuação: 50% dos pontos normais por inimigo
- Screen shake: 8px magnitude, 0.5s

**Spawn:**
- Frequência: 1 a cada 3000-4000px (muito raro)
- Posição: após seções muito difíceis

#### 1.6 Combustível Extra (Mega Fuel)
**Aparência:**
- Forma: retângulo 24×36px
- Cor: verde neon (#00ff00)
- Borda: amarela (#ffff00) 3px
- Ícone: "F+" grande
- Animação: brilho intenso pulsante

**Efeito:**
- Recarga: 60 unidades (vs 30 do normal)
- Visual: explosão verde de partículas ao coletar
- Bônus: +200 pontos extras

**Spawn:**
- Frequência: 1 a cada 2000-3000px
- Substitui 30% dos fuel normais

### Indicador de Power-ups Ativos

**Posicionamento:**
- Localização: canto inferior esquerdo
- Coordenadas: x=15px, y=canvas.height-130px

**Container:**
- Dimensões: 180×110px
- Fundo: preto (#000000, 75% opacidade)
- Borda: branca (#ffffff) 2px
- Cantos arredondados: 5px

**Layout interno:**
```
┌──────────────────┐
│ ACTIVE POWER-UPS │
├──────────────────┤
│ [🛡] Shield  5.2s│
│ [⇈]  Double  7.8s│
│ [💣] Bombs: ×2   │
└──────────────────┘
```

**Elementos:**
- Label superior: "ACTIVE POWER-UPS", Arial Bold 12px
- Ícones miniatura: 16×16px, cor do power-up
- Timer: contagem regressiva em segundos, 1 decimal
- Cor do timer:
  - Verde: >5s
  - Amarelo: 3-5s
  - Vermelho piscante: <3s
- Bombas: contador sem timer

## 2. Sistema de Chefes

### Boss 1: Helicóptero de Ataque Pesado

**Aparência:**
- Dimensões: 120×80px (4x tamanho normal)
- Cor: verde militar escuro (#3a4f2a)
- Detalhes:
  - Dois rotores principais (50px cada)
  - 4 canhões laterais visíveis
  - Cockpit blindado (vidro vermelho)
  - Placa de armadura cinza
  - Luzes piscantes vermelhas

**Comportamento:**
- HP: 50 hits
- Entrada: desce lentamente do topo (3s)
- Movimento: padrão senoidal horizontal
- Fases de ataque:
  1. **Fase 1 (HP 50-30)**: disparo simples a cada 1s
  2. **Fase 2 (HP 30-15)**: disparo em rajada de 3, mais rápido
  3. **Fase 3 (HP 15-0)**: disparo caótico + movimento errático

**Ataques especiais:**
- **Mísseis teleguiados** (Fase 2+): 
  - Dispara 2 mísseis que seguem o jogador
  - Cor: laranja com rastro de fumaça
  - Velocidade: 3px/frame
  - Duração: 5s ou até colisão
  
- **Carpet bomb** (Fase 3): 
  - Solta 8 bombas em linha
  - Caem verticalmente
  - Explosão 40px raio ao tocar rio

**Derrota:**
- Animação: múltiplas explosões sequenciais (2s)
- Fragmentos: 10-15 pedaços caindo
- Pontuação: 5000 pontos
- Recompensa: 3 power-ups aleatórios
- Recarga total de combustível

### Boss 2: Navio de Guerra Encouraçado

**Aparência:**
- Dimensões: 140×180px (vertical)
- Cor: cinza naval (#555555)
- Detalhes:
  - 3 torres de canhão (rotativas)
  - Ponte de comando elevada
  - Convés com detalhes
  - 6 canhões antiaéreos laterais
  - Âncora decorativa

**Comportamento:**
- HP: 75 hits
- Posição: move lentamente pelo rio (vertical)
- Largura: ocupa 70% do rio
- Fases:
  1. **Fase 1 (HP 75-50)**: canhões principais disparam alternados
  2. **Fase 2 (HP 50-25)**: todos canhões ativos
  3. **Fase 3 (HP 25-0)**: antiaéreos + torpedos

**Ataques:**
- **Canhões principais**: projéteis grandes (12px), lentos, alto dano
- **Antiaéreos**: rajadas rápidas, projéteis pequenos
- **Torpedos** (Fase 3): 
  - Largados lateralmente
  - Movimento: horizontal cruzando rio
  - Velocidade: 2px/frame

**Pontos fracos:**
- Torres de canhão: 3× dano
- Ponte de comando: 5× dano (pequeno alvo)

**Derrota:**
- Afunda lentamente (3s)
- Explosões em cascata
- Pontuação: 8000 pontos
- Recompensa: Escudo + Arma Tripla + Mega Fuel

### Boss 3: Fortaleza Aérea (Final Boss)

**Aparência:**
- Dimensões: 200×150px
- Forma: plataforma voadora futurística
- Cor: preto metálico (#1a1a1a) com detalhes vermelhos
- Detalhes:
  - 4 turbinas (animadas)
  - Torre central com laser
  - 8 torretas defensivas
  - Escudo energético visível (ciano translúcido)

**Comportamento:**
- HP: 120 hits (escudo) + 80 hits (core)
- Movimento: diagonal complexo, imprevisível
- Fases:
  1. **Fase Escudo**: escudo ativo, dano reduzido 50%
  2. **Fase Transição**: escudo quebra, vulnerável 3s
  3. **Fase Core**: ataque total, sem defesa
  4. **Fase Final** (HP <20): kamikaze desesperado

**Ataques:**
- **Laser contínuo**: beam 400px comprimento, rastreia jogador
- **Drones**: spawna 4 mini-helicópteros
- **Bombardeio orbital**: projéteis caem do topo da tela
- **Onda de choque**: pulso circular expansivo

**Derrota:**
- Explosão nuclear (tela branca 1s)
- Fragmentos massivos
- Screen shake 10px, 3s
- Pontuação: 15000 pontos
- Cutscene de vitória

## 3. Tipos de Terreno Variados

### Terreno 1: Rio Padrão
- Já implementado
- Cor água: #1a4d7a
- Margens: #2d5016

### Terreno 2: Deserto/Canyon
**Características:**
- Água: marrom-amarelada (#8b7355)
- Margens: areia clara (#d4a76a)
- Decorações: cactos (10-20px) espaçados
- Pedras: círculos cinza aleatórios
- Inimigos especiais: escorpiões mecânicos (novos)

### Terreno 3: Neve/Ártico
**Características:**
- Água: azul gelo (#a8d8ea)
- Margens: branco neve (#f0f0f0)
- Decorações: pinheiros (triângulos verdes)
- Icebergs flutuantes (obstáculos móveis)
- Inimigos: tanques brancos (camuflados)
- Efeito visual: partículas de neve caindo

### Terreno 4: Vulcão/Lava
**Características:**
- Água: laranja-vermelho lava (#ff4400)
- Margens: rocha preta (#2a2a2a)
- Decorações: crateras com brilho
- Lava bubbles: círculos que sobem e explodem
- Inimigos: drones de fogo (vermelhos)
- Efeito: heat distortion (ondulação)
- Dano: lava causa dano contínuo se tocar

### Terreno 5: Cidade/Urbano
**Características:**
- Água: rio urbano cinza (#4a5f6a)
- Margens: asfalto (#333333) com prédios
- Decorações: arranha-céus de fundo, pontes complexas
- Carros nas margens (animados)
- Inimigos: drones urbanos, helicópteros policiais
- Iluminação: janelas acesas nos prédios

### Terreno 6: Espaço Sideral
**Características:**
- Água: espaço profundo (#0a0a1a)
- Margens: asteroides (#666666)
- Decorações: estrelas, nebulosas coloridas
- Sem gravidade: movimento mais fluido
- Inimigos: naves alienígenas
- Efeito: parallax de estrelas em 3 camadas

### Transição entre Terrenos
- Duração: 500px de scroll
- Gradiente suave entre cores
- Fade in/out de decorações
- Mensagem: "ENTERING [TERRAIN NAME]"

## 4. Sistema de Achievements

### Lista de Conquistas

**Básicas:**
- "First Blood" - Destruir primeiro inimigo (10pts)
- "Survivor" - Completar primeiro nível (50pts)
- "Ace Pilot" - Atingir 10.000 pontos (100pts)
- "Fuel Master" - Coletar 50 combustíveis (75pts)

**Combate:**
- "Sharpshooter" - 90% acurácia em um nível (150pts)
- "Tank Destroyer" - Destruir 50 tanques (100pts)
- "Air Superiority" - Destruir 100 helicópteros (100pts)
- "Naval Commander" - Destruir 30 barcos (100pts)

**Combo:**
- "Combo Rookie" - Atingir combo 5x (50pts)
- "Combo Master" - Atingir combo 10x (200pts)
- "Unstoppable" - Manter combo 10x por 15s (500pts)

**Sobrevivência:**
- "Close Call" - Sobreviver com <5% fuel (100pts)
- "Untouchable" - Completar nível sem dano (250pts)
- "Marathon" - Jogar por 10 minutos contínuos (150pts)

**Bosses:**
- "Boss Hunter" - Derrotar primeiro boss (200pts)
- "Fleet Admiral" - Derrotar boss navio (300pts)
- "Savior" - Derrotar boss final (500pts)
- "Flawless Victory" - Derrotar boss sem tomar dano (1000pts)

**Especiais:**
- "Pacifist" - Completar 500px sem disparar (150pts)
- "Rampage" - Destruir 20 inimigos em 10s (300pts)
- "Collector" - Coletar todos tipos de power-up (200pts)
- "Perfect Run" - 100% acurácia em nível (500pts)

### Interface de Achievements

**Posicionamento:**
- Notificação popup: topo-centro da tela
- Menu de achievements: tela separada (tecla A)

**Notificação de desbloqueio:**
```
┌─────────────────────────────┐
│  🏆 ACHIEVEMENT UNLOCKED!   │
│                             │
│     "First Blood"           │
│  Destruir primeiro inimigo  │
│         +10 pts             │
└─────────────────────────────┘
```

**Estilo:**
- Dimensões: 350×120px
- Fundo: gradiente dourado (#ffd700 → #ffaa00)
- Borda: dourada brilhante 4px
- Animação entrada: slide down + bounce (0.5s)
- Permanência: 3s
- Animação saída: fade up (0.5s)
- Som: fanfarra curta

**Menu de achievements:**
- Grid 3 colunas
- Cada achievement: 100×100px card
- Desbloqueado: colorido + ícone
- Bloqueado: cinza + silhueta + "???"
- Progress bar para achievements progressivos
- Total: X/30 (porcentagem embaixo)

## 5. Modos de Dificuldade

### Easy Mode
- Combustível inicial: 150 unidades
- Consumo: 0.15 unidades/frame (-25%)
- HP da nave: 2 hits antes de morrer
- Velocidade inimigos: -20%
- Frequência de power-ups: +50%
- Spawn de inimigos: -30%
- Pontuação: 70% dos pontos normais

### Normal Mode
- Configurações padrão (já especificadas)
- Pontuação: 100%

### Hard Mode
- Combustível inicial: 80 unidades
- Consumo: 0.25 unidades/frame (+25%)
- HP da nave: 1 hit (padrão)
- Velocidade inimigos: +30%
- Frequência de power-ups: -40%
- Spawn de inimigos: +50%
- Inimigos atiram mais frequentemente
- Pontuação: 150% dos pontos normais

### Nightmare Mode (Desbloqueável)
- Combustível inicial: 60 unidades
- Consumo: 0.3 unidades/frame
- Velocidade scroll: +50%
- Inimigos: HP dobrado
- Chefes: HP triplicado + novos ataques
- Sem power-ups de vida/escudo
- Rio mais estreito (margens +30% largura)
- Pontuação: 300% dos pontos normais
- Desbloqueia após completar Hard Mode

### Seleção de Dificuldade

**Tela de seleção:**
```
┌────────────────────────────┐
│    SELECT DIFFICULTY       │
│                            │
│  [ EASY ]   ⭐            │
│  [ NORMAL ] ⭐⭐          │
│  [ HARD ]   ⭐⭐⭐        │
│  [ 🔒 NIGHTMARE ] ⭐⭐⭐⭐ │
│                            │
│  Best Score: 45,200        │
└────────────────────────────┘
```

## 6. Leaderboard Online

### Estrutura de Dados
```javascript
{
  rank: 1,
  playerName: "ACE_PILOT",
  score: 125400,
  difficulty: "HARD",
  date: "2025-10-28",
  achievements: 28,
  combo_max: 10
}
```

### Interface

**Posicionamento:**
- Tela dedicada (tecla L)
- Acessível do menu principal

**Layout:**
```
┌─────────────────────────────────────────┐
│         GLOBAL LEADERBOARD              │
│         [EASY][NORMAL][HARD][ALL]       │
├────┬────────────┬─────────┬─────────────┤
│ #  │   NAME     │  SCORE  │  DIFFICULTY │
├────┼────────────┼─────────┼─────────────┤
│ 1  │ ACE_PILOT  │ 125,400 │    HARD     │
│ 2  │ SKY_QUEEN  │ 98,750  │   NORMAL    │
│ 3  │ RIVER_KING │ 87,300  │    HARD     │
│... │     ...    │   ...   │     ...     │
│ 45 │ [YOU] ▶    │ 45,200  │   NORMAL    │
└────┴────────────┴─────────┴─────────────┘
```

**Funcionalidades:**
- Filtro por dificuldade
- Top 100 jogadores
- Destaque para posição do jogador
- Atualização: ao fim de cada partida
- Sincronização: tempo real ou ao abrir tela
- Nome do jogador: 12 caracteres máximo
- Validação: anti-cheat básico (score máximo possível)

### API Endpoints (Conceitual)
- GET /leaderboard?difficulty=normal&limit=100
- POST /leaderboard (score, difficulty, playerName)
- GET /leaderboard/player/{id}

---

**Notas de implementação:**
- Fase 14 deve ser implementada ANTES da 16
- Power-ups da Fase 16 são independentes entre si
- Chefes requerem sistema de HP (não implementado ainda)
- Terrenos podem ser adicionados gradualmente
- Achievements requerem sistema de persistência (localStorage)
- Leaderboard requer backend (pode usar serviço terceiro como Firebase)