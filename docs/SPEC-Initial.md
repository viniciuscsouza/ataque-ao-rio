# Especificações - Clone River Raid

## 1. Cenário e Visualização

### Canvas Principal
- Dimensões: 800x600 pixels
- Cor de fundo: azul escuro (#1a4d7a) representando o rio
- Orientação: visão vertical top-down
- Rolagem: automática de cima para baixo (velocidade: 2-3 px/frame)

### Rio
- **Margens**: faixas verdes (#2d5016) à esquerda e direita
  - Largura variável: 150-250px cada margem
  - Desenho: retângulos sólidos
  - Sinuosidade: curvas suaves a cada 300-500px
  
- **Pontes**:
  - Cor: cinza (#666666)
  - Dimensões: largura do rio × 40px altura
  - Espaçamento: aparecem a cada 800-1200px
  - Gap central: 120-150px para passagem obrigatória

## 2. Nave do Jogador

### Aparência
- Formato: triângulo isósceles apontando para cima
- Dimensões: 30px largura × 40px altura
- Cores:
  - Corpo: vermelho (#cc0000)
  - Asas laterais: cinza claro (#aaaaaa) - triângulos menores nas laterais
  - Cockpit: amarelo (#ffff00) - círculo 8px no topo
- Posição inicial: centro-inferior da tela (y = 500px)

### Desenho Detalhado
```
Corpo principal: triângulo (15, 0) - (0, 40) - (30, 40)
Asa esquerda: triângulo (-10, 20) - (0, 25) - (0, 35)
Asa direita: triângulo (30, 25) - (40, 20) - (30, 35)
Cockpit: círculo em (15, 12) raio 4px
```

## 3. Inimigos

### Helicópteros
- Forma: retângulo 35×20px (corpo) + círculo 25px (rotor acima)
- Cores: corpo verde militar (#4a5f3a), rotor cinza translúcido
- Movimento: horizontal esquerda-direita
- Velocidade: 1-2 px/frame
- Desenho:
  - Corpo: retângulo arredondado
  - Cauda: linha fina 15px para trás
  - Rotor: elipse horizontal girando

### Tanques
- Forma: retângulo 40×25px + canhão 20×5px
- Cor: verde escuro (#2d4a2b)
- Posição: fixos nas margens
- Desenho:
  - Base: retângulo com cantos arredondados
  - Rodas: 4 círculos pequenos (6px) embaixo
  - Canhão: retângulo apontando para o centro do rio
  - Torre: retângulo menor 20×15px no centro

### Barcos
- Forma: trapézio 45×20px
- Cor: cinza escuro (#4a4a4a)
- Movimento: vertical lento (0.5-1 px/frame)
- Desenho:
  - Casco: trapézio - base maior embaixo
  - Cabine: retângulo 15×10px no centro
  - Detalhes: 2-3 janelas pequenas (quadrados 3px)

## 4. Elementos Interativos

### Combustível (Power-ups)
- Forma: retângulo vertical 20×30px
- Cor: vermelho brilhante (#ff0000)
- Borda: amarela (#ffff00) 2px
- Posição: flutuando no rio
- Desenho:
  - Corpo: retângulo com label "F"
  - Brilho: pulsa opacidade 0.7-1.0

### Projéteis do Jogador
- Forma: retângulo vertical 4×15px
- Cor: amarelo brilhante (#ffff00)
- Velocidade: 8 px/frame para cima
- Rastro: leve desfoque motion blur

### Projéteis Inimigos
- Forma: círculo 6px
- Cor: laranja (#ff6600)
- Velocidade: 4 px/frame em direção ao jogador

## 5. Explosões

### Animação
- Frames: 5 quadros
- Duração: 300ms total
- Frames:
  1. Círculo amarelo 20px
  2. Círculo laranja 35px com partículas
  3. Círculo vermelho 45px + mais partículas
  4. Círculo escuro 40px fragmentado
  5. Fumaça cinza dissipando

### Partículas
- Quantidade: 8-12 por explosão
- Forma: círculos 3-6px
- Cores: amarelo, laranja, vermelho
- Movimento: radial para fora

## 6. Interface (HUD)

### Barra de Combustível
- Posição: canto superior esquerdo
- Dimensões: 200×20px
- Desenho:
  - Fundo: cinza escuro (#333333)
  - Preenchimento: gradiente verde-amarelo-vermelho
  - Borda: branca 2px
  - Label: "FUEL" em branco

### Pontuação
- Posição: canto superior direito
- Fonte: monospace, 24px
- Cor: branca (#ffffff)
- Formato: "SCORE: 00000"

### Vidas
- Posição: abaixo da pontuação
- Desenho: 3 ícones pequenos da nave (15px)
- Cor: branca

## 7. Mecânicas de Colisão

### Hitboxes
- Nave jogador: 80% do tamanho visual
- Inimigos: 90% do tamanho visual
- Margens/pontes: hitbox exata
- Combustível: hitbox exata

### Detecção
- Método: retângulos (AABB)
- Verificação: a cada frame
- Prioridade: margens > inimigos > projéteis > combustível

## 8. Parâmetros de Gameplay

- **Combustível inicial**: 100 unidades
- **Consumo**: 0.2 unidades/frame
- **Recarga por power-up**: 30 unidades
- **Velocidade de scroll**: inicia 2px/frame, aumenta 0.1 a cada 1000 pontos
- **Spawn de inimigos**: 1 a cada 200-400px de scroll
- **Pontuação**: helicóptero 100pts, tanque 150pts, barco 200pts

---

**Paleta de Cores Principal:**
- Rio: #1a4d7a
- Margens: #2d5016
- Pontes: #666666
- Jogador: #cc0000, #aaaaaa, #ffff00
- Inimigos: #4a5f3a, #2d4a2b, #4a4a4a
- UI: #ffffff, #333333
- Efeitos: #ffff00, #ff6600, #ff0000

---

# Especificações Detalhadas - Interface (HUD)

## 1. Layout Geral

### Estrutura
- **Camada**: sobreposta ao jogo (z-index superior)
- **Fundo**: transparente, não interfere na visibilidade do jogo
- **Padding**: 15px das bordas do canvas
- **Opacidade**: 95% para leve transparência

## 2. Barra de Combustível

### Posicionamento
- **Localização**: canto superior esquerdo
- **Coordenadas**: x=15px, y=15px
- **Espaçamento**: 10px de margem interna

### Dimensões
- **Container total**: 220×50px
- **Barra interna**: 200×25px
- **Borda**: 3px sólida

### Estrutura Visual
```
┌─────────────────────────┐
│ FUEL                    │
│ ┌───────────────────┐   │
│ │█████████████░░░░░░│ 65%│
│ └───────────────────┘   │
└─────────────────────────┘
```

### Desenho Detalhado
**Container externo:**
- Retângulo: 220×50px
- Fundo: preto semi-transparente (#000000, 70% opacidade)
- Borda: branca (#ffffff) 2px
- Cantos: arredondados 5px

**Label "FUEL":**
- Posição: x=10px, y=8px (relativo ao container)
- Fonte: Arial Bold, 14px
- Cor: branco (#ffffff)
- Sombra: preta 1px para legibilidade

**Barra de progresso:**
- Posição: x=10px, y=25px (relativo ao container)
- Fundo da barra: cinza escuro (#2a2a2a)
- Borda interna: cinza médio (#555555) 1px

**Preenchimento (fuel atual):**
- Altura: 25px
- Largura: proporcional ao combustível (0-200px)
- Gradiente horizontal:
  - 100-70%: verde (#00ff00) → verde-amarelo (#88ff00)
  - 70-30%: amarelo (#ffff00) → laranja (#ffaa00)
  - 30-0%: laranja (#ff6600) → vermelho (#ff0000)
- Efeito: brilho sutil (glow) quando < 30%

**Percentual numérico:**
- Posição: x=205px, y=32px
- Fonte: Arial Bold, 12px
- Cor: branco (#ffffff)
- Formato: "65%"

### Animações
- **Consumo**: transição suave 0.1s
- **Recarga**: efeito de pulso + som
- **Crítico (<20%)**: piscar vermelho 0.5s intervalo
- **Vazio (0%)**: piscar rápido vermelho 0.2s + alerta sonoro

## 3. Painel de Pontuação

### Posicionamento
- **Localização**: canto superior direito
- **Coordenadas**: x=canvas.width-235px, y=15px

### Dimensões
- **Container**: 220×40px

### Estrutura Visual
```
┌─────────────────┐
│ SCORE: 0045200  │
└─────────────────┘
```

### Desenho Detalhado
**Container:**
- Retângulo: 220×40px
- Fundo: preto semi-transparente (#000000, 70% opacidade)
- Borda: branca (#ffffff) 2px
- Cantos: arredondados 5px

**Label "SCORE:":**
- Posição: x=10px, y=23px (centralizado verticalmente)
- Fonte: Arial Bold, 18px
- Cor: amarelo (#ffff00)

**Valor da pontuação:**
- Posição: alinhado à direita, x=205px
- Fonte: Courier New Bold, 18px (monospace)
- Cor: branco (#ffffff)
- Formato: 7 dígitos com zeros à esquerda "0000000"
- Sombra: verde (#00ff00) 1px para efeito retrô

### Animações
- **Incremento**: dígitos rolam para cima (slot machine)
- **Duração**: 0.3s por incremento
- **Efeito especial**: pulso amarelo ao atingir múltiplos de 10.000

## 4. Indicador de Vidas

### Posicionamento
- **Localização**: abaixo do painel de pontuação
- **Coordenadas**: x=canvas.width-235px, y=65px

### Dimensões
- **Container**: 220×50px
- **Espaçamento entre ícones**: 15px

### Estrutura Visual
```
┌─────────────────┐
│ LIVES:  ▲ ▲ ▲  │
└─────────────────┘
```

### Desenho Detalhado
**Container:**
- Retângulo: 220×50px
- Fundo: preto semi-transparente (#000000, 70% opacidade)
- Borda: branca (#ffffff) 2px
- Cantos: arredondados 5px

**Label "LIVES:":**
- Posição: x=10px, y=28px
- Fonte: Arial Bold, 14px
- Cor: branco (#ffffff)

**Ícones de naves:**
- Posição inicial: x=80px, y=15px
- Dimensões por ícone: 25×30px
- Desenho: miniatura da nave do jogador
  - Corpo: triângulo vermelho (#cc0000)
  - Asas: cinza (#aaaaaa)
  - Cockpit: ponto amarelo 3px
- Espaçamento: 35px entre cada ícone (centro a centro)
- Quantidade máxima exibida: 5 vidas

**Estados:**
- **Vida ativa**: cores normais, opacidade 100%
- **Vida perdida**: fade out para cinza (#666666), opacidade 30%

### Animações
- **Perda de vida**: ícone pisca 3x antes de escurecer (0.5s total)
- **Vida extra**: novo ícone surge com scale 0→1.2→1 (0.4s)

## 5. Barra de Nível/Progresso

### Posicionamento
- **Localização**: centro superior
- **Coordenadas**: x=canvas.width/2-100px, y=15px

### Dimensões
- **Container**: 200×30px

### Estrutura Visual
```
┌─────────────────┐
│ STAGE 3  ▓▓▓░░ │
└─────────────────┘
```

### Desenho Detalhado
**Container:**
- Retângulo: 200×30px
- Fundo: preto semi-transparente (#000000, 70% opacidade)
- Borda: ciano (#00ffff) 2px
- Cantos: arredondados 5px

**Label de estágio:**
- Posição: x=10px, y=18px
- Fonte: Arial Bold, 12px
- Cor: ciano (#00ffff)
- Formato: "STAGE X"

**Barra de progresso:**
- Posição: x=90px, y=10px
- Dimensões: 100×10px
- Segmentos: 5 blocos de 18px cada + 2px espaço
- Cor preenchida: verde (#00ff00)
- Cor vazia: cinza escuro (#333333)

## 6. Mensagens de Alerta

### Alerta de Combustível Baixo

**Posicionamento:**
- Centro da tela: x=canvas.width/2, y=canvas.height/2-100px

**Container:**
- Dimensões: 300×80px (centralizado)
- Fundo: vermelho semi-transparente (#ff0000, 80% opacidade)
- Borda: amarela (#ffff00) 4px piscante

**Texto:**
- "⚠ LOW FUEL ⚠"
- Fonte: Arial Black, 28px
- Cor: amarelo (#ffff00)
- Alinhamento: centralizado
- Animação: pisca 0.3s on/off

**Duração:** exibido quando fuel < 20%, some quando reabastece

### Game Over

**Posicionamento:**
- Centro absoluto da tela

**Container:**
- Dimensões: 400×200px
- Fundo: preto sólido (#000000, 90% opacidade)
- Borda: vermelha (#ff0000) 5px

**Texto principal:**
- "GAME OVER"
- Fonte: Arial Black, 48px
- Cor: vermelho (#ff0000)
- Sombra: preta 3px
- Posição: centralizado, y=60px (relativo ao container)

**Pontuação final:**
- "FINAL SCORE: XXXXXXX"
- Fonte: Arial Bold, 24px
- Cor: branco (#ffffff)
- Posição: centralizado, y=110px

**Instruções:**
- "PRESS SPACE TO RESTART"
- Fonte: Arial, 16px
- Cor: amarelo (#ffff00)
- Posição: centralizado, y=160px
- Animação: pisca lentamente 1s on/off

### Pausa

**Overlay completo:**
- Fundo: preto semi-transparente (#000000, 60% opacidade)
- Cobre todo o canvas

**Texto:**
- "PAUSED"
- Fonte: Arial Black, 64px
- Cor: branco (#ffffff) com borda ciano (#00ffff) 3px
- Posição: centro absoluto
- Sombra: preta 4px

**Subtexto:**
- "Press P to continue"
- Fonte: Arial, 20px
- Cor: cinza claro (#cccccc)
- Posição: 40px abaixo do texto principal

## 7. Indicador de Combo/Multiplicador

### Posicionamento
- **Localização**: centro-esquerdo da tela
- **Coordenadas**: x=15px, y=canvas.height/2-50px

### Dimensões
- **Container**: 120×80px (aparece apenas quando ativo)

### Desenho Detalhado
**Container:**
- Fundo: gradiente diagonal amarelo-laranja (#ffff00 → #ff8800)
- Borda: branca (#ffffff) 3px
- Cantos: arredondados 8px
- Brilho: glow amarelo externo

**Texto "COMBO":**
- Posição: topo do container
- Fonte: Arial Bold, 14px
- Cor: vermelho escuro (#aa0000)

**Multiplicador:**
- Formato: "×N" (ex: ×3, ×5)
- Fonte: Arial Black, 36px
- Cor: branco (#ffffff)
- Sombra: vermelha (#ff0000) 2px
- Animação: pulsa scale 1.0→1.2→1.0 a cada hit

**Condição de exibição:**
- Aparece: ao destruir 3+ inimigos em 2 segundos
- Desaparece: após 3 segundos sem kills (fade out 0.5s)

## 8. Mini-mapa (Opcional)

### Posicionamento
- **Localização**: canto inferior direito
- **Coordenadas**: x=canvas.width-135px, y=canvas.height-135px

### Dimensões
- **Container**: 120×120px

### Desenho Detalhado
**Container:**
- Retângulo: 120×120px
- Fundo: preto (#000000, 80% opacidade)
- Borda: verde (#00ff00) 2px

**Elementos do mapa:**
- **Rio**: linha azul vertical central (3px)
- **Margens**: linhas verdes laterais (2px)
- **Jogador**: ponto vermelho pulsante (5px)
- **Inimigos**: pontos amarelos (3px)
- **Combustível**: pontos verdes (3px)

**Escala:**
- Representa 1000px verticais do jogo
- Atualiza em tempo real

## 9. Paleta de Cores - Interface

```
Backgrounds:      #000000 (70-90% opacidade)
Bordas primárias: #ffffff
Bordas especiais: #00ffff, #ffff00, #ff0000
Textos principais: #ffffff
Textos destaque:   #ffff00
Alertas:          #ff0000
Positivos:        #00ff00
Fuel gradient:    #00ff00 → #ffff00 → #ff6600 → #ff0000
Shadows:          #000000, #00ff00 (retrô)
```

## 10. Responsividade

- Todos os elementos escalam proporcionalmente se canvas < 800×600
- Fontes reduzem 20% em telas pequenas
- Mantém legibilidade mínima de 12px
- HUD nunca sobrepõe área crítica de gameplay (centro)