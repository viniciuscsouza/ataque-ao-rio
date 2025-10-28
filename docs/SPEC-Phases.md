# Ordem de Implementação Recomendada - Clone River Raid

## Fase 1: Fundação (Core Engine)
- Configurar canvas HTML5 e contexto 2D de renderização
- Implementar loop principal do jogo (requestAnimationFrame)
- Criar sistema de coordenadas e grid base
- Implementar sistema de input (teclado: setas, espaço, P)
- Configurar estrutura de estados do jogo (menu, jogando, pausado, game over)

## Fase 2: Cenário Básico
- Desenhar fundo do rio (retângulo azul fixo)
- Implementar margens verdes estáticas (esquerda e direita)
- Criar sistema de scroll vertical automático
- Adicionar sinuosidade básica às margens (curvas suaves)
- Implementar geração procedural de segmentos de rio

## Fase 3: Nave do Jogador
- Desenhar nave do jogador (triângulo + asas + cockpit)
- Implementar movimentação horizontal (limites da tela)
- Implementar movimentação vertical (limitada)
- Adicionar sistema de disparo (tecla espaço)
- Criar projéteis do jogador (movimento e renderização)

## Fase 4: Sistema de Colisão
- Implementar detecção de colisão AABB (retângulos)
- Criar hitboxes para todos os elementos
- Detectar colisão nave vs margens
- Detectar colisão nave vs pontes
- Detectar colisão projéteis vs inimigos
- Implementar sistema de destruição de objetos

## Fase 5: Inimigos Básicos
- Implementar helicópteros (desenho e movimento horizontal)
- Implementar tanques nas margens (estáticos com canhão)
- Implementar barcos no rio (movimento vertical lento)
- Criar sistema de spawn de inimigos (aleatório)
- Adicionar lógica de tiro inimigo (projéteis básicos)

## Fase 6: Pontes
- Desenhar pontes atravessando o rio
- Implementar gap central para passagem
- Adicionar colisão ponte vs nave
- Criar sistema de geração periódica de pontes
- Implementar colisão projéteis vs pontes

## Fase 7: Sistema de Combustível
- Implementar contador de combustível (variável)
- Adicionar consumo constante por frame
- Criar power-ups de combustível (desenho e spawn)
- Implementar coleta de combustível (colisão + recarga)
- Adicionar lógica de game over por falta de combustível

## Fase 8: Interface - HUD Essencial
- Implementar barra de combustível (visual e funcional)
- Adicionar display de pontuação (canto superior direito)
- Criar indicador de vidas (ícones de naves)
- Implementar sistema de pontos (acumular ao destruir inimigos)
- Adicionar sistema de vidas (perder ao colidir)

## Fase 9: Efeitos Visuais
- Criar animação de explosão (5 frames)
- Adicionar sistema de partículas para explosões
- Implementar efeito de brilho nos projéteis
- Adicionar feedback visual ao coletar combustível
- Criar transição de fade para game over

## Fase 10: Áudio (Placeholder)
- Adicionar som de disparo (efeito simples)
- Implementar som de explosão
- Adicionar som de coleta de combustível
- Criar som de alerta de combustível baixo
- Implementar música de fundo (loop simples)

## Fase 11: Interface - Alertas e Mensagens
- Implementar alerta de combustível baixo (<20%)
- Criar tela de Game Over (texto e pontuação final)
- Adicionar tela de pausa (overlay + texto)
- Implementar botão de restart (tecla espaço)
- Criar transições entre estados do jogo

## Fase 12: Progressão e Dificuldade
- Implementar sistema de níveis/estágios
- Adicionar barra de progresso de nível
- Aumentar velocidade de scroll progressivamente
- Incrementar frequência de spawn de inimigos
- Balancear dificuldade (consumo combustível, HP inimigos)

## Fase 13: Melhorias Visuais
- Refinar desenho dos inimigos (detalhes extras)
- Adicionar animações de movimento (rotor helicóptero)
- Implementar efeitos de hover nos elementos
- Adicionar sombras e brilhos para profundidade
- Criar variações visuais de elementos (cores, tamanhos)

## Fase 14: Sistema de Combo (Opcional)
- Implementar detector de múltiplos kills rápidos
- Criar indicador visual de combo
- Adicionar multiplicador de pontos
- Implementar timer de combo
- Adicionar efeitos especiais para combos altos

## Fase 15: Polish Final
- Otimizar performance (garbage collection, pooling de objetos)
- Adicionar menu inicial (start, instruções)
- Implementar sistema de high score (localStorage)
- Criar tela de instruções/controles
- Adicionar mini-mapa (opcional)
- Testar e balancear gameplay
- Ajustar feedback visual e sonoro
- Correção de bugs finais

## Fase 16: Extras (Se houver tempo)
- Power-ups adicionais (escudo, arma dupla, velocidade)
- Chefes de fim de nível
- Diferentes tipos de terreno
- Sistema de achievements
- Modo de dificuldade selecionável
- Leaderboard online

---

**Nota:** Cada fase deve ser testada e validada antes de avançar para a próxima. A progressão é incremental, permitindo ter um jogo jogável desde a Fase 7.