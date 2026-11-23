---
name: setup-web-game
description: >-
  Prepara e estrutura projetos de jogos web 2D (Phaser 3) e 3D (Babylon.js)
  com arquitetura modular orientada a eventos, mobile-first, alta performance,
  carregamento progressivo e placeholders geométricos configurados com triggers de áudio e animação.
---

# Setup Web Game Skill

Esta skill padroniza o scaffolding, a arquitetura e o desenvolvimento de jogos web 2D e 3D.

---

## 1. Diretrizes Fundamentais

- **Engines Oficiais**:
  - **2D**: Phaser 3.
  - **3D**: Babylon.js.
- **Idioma e Convenções**:
  - Código, nomes de arquivos, funções, variáveis e classes em **inglês**.
  - Comentários no código estritamente em **português (pt-BR)** e com critério (apenas quando o contexto for indispensável).
- **Clean Code & Boas Práticas**:
  - Funções pequenas com responsabilidade única.
  - Princípios KISS, DRY, alta coesão e baixo acoplamento.
  - Tratamento de erro explícito e preventivo.

---

## 2. Placeholders Visuais e Abstração de Assets

- **Sem geração prematura de assets**: Não buscar ou inventar sprites/modelos pesados sem solicitação.
- **Formas Geométricas Coloridas**:
  - 2D (Phaser): Retângulos, círculos e polígonos com cores sólidas distintas (ex.: Verde = Jogador, Vermelho = Inimigo, Amarelo = Item, Cinza = Plataforma).
  - 3D (Babylon): Cubos, esferas e cilindros com materiais básicos coloridos.
- **Camada de Desacoplamento (`AssetRegistry` / `SkinResolver`)**:
  - Manter um ponto único e centralizado para registro de elementos visuais.
  - Deixar mapeamento pronto para substituir geometrias por spritesheets, atlas ou modelos 3D (`.gltf` / `.glb`).
  - Triggers para animações (idle, walk, jump, attack, hurt) e eventos de áudio (SFX/BGM) já emitidos via EventBus.

---

## 3. Arquitetura Modular & Event-Driven (Observable)

- **Separação Rigorosa de Responsabilidades**:
  - `core/`: Inicialização da engine, loop principal e gerenciamento de ciclo de vida.
  - `events/`: EventBus / Observer central para comunicação entre módulos desvinculados.
  - `scenes/`: Telas de boot, loading, menu, gameplay e game over.
  - `entities/`: Representação lógica de entidades (comportamento desacoplado do render).
  - `input/`: Mapeamento de controles (teclado, toque/touch virtual).
  - `audio/`: Gerenciador de áudio escutando eventos do EventBus.
  - `ui/`: Camada de interface (HUD, botões, modais) desacoplada do jogo.
- **Comunicação por Eventos**:
  - Entidades não acessam a UI ou o Gerenciador de Áudio diretamente.
  - Disparam eventos no EventBus (ex.: `player:damaged`, `coin:collected`, `level:completed`).

---

## 4. Mobile-First & Responsividade

- **Viewport Adaptativo**:
  - Configurar Scale Manager (Phaser) ou Engine Resize listener (Babylon.js) para redimensionamento dinâmico sem distorção.
  - Suporte a orientação retrato (portrait) e paisagem (landscape).
- **Controles por Toque**:
  - Touch input nativo com suporte a multitouch.
  - Virtual joystick ou botões de toque na tela com feedback visual.
  - Prevenir comportamento padrão do navegador em gestos de toque (`touch-action: none`).

---

## 5. Performance e Otimização

- **Taxa de Quadros (Target: 60 FPS)**:
  - Evitar criação de objetos dentro do loop `update` (prevenir coletas frequentes do Garbage Collector).
  - Utilizar **Object Pooling** para projéteis, partículas e inimigos repetitivos.
- **Gestão de Memória**:
  - Destruir geometrias, materiais, texturas e listeners ao encerrar ou trocar cenas.
- **Carregamento Otimizado**:
  - Tela de boot com barra de carregamento inicial.
  - Carregamento progressivo (lazy loading de fases subsequentes ou pacotes de áudio/assets).
- **Latência de Entrada**:
  - Processamento imediato de comandos de input no início do ciclo do frame.

---

## 6. Estrutura Padrão de Diretórios

### Estrutura para Jogo 2D (Phaser 3)

```text
src/
├── config/
│   ├── gameConfig.js       # Resolução, escala, física e plugins
│   └── assetRegistry.js    # Mapeamento de cores, shapes e futuros assets
├── events/
│   └── EventBus.js         # Instância do EventEmitter / Observer central
├── scenes/
│   ├── BootScene.js        # Configurações iniciais mínimas
│   ├── PreloadScene.js     # Barra de progresso e carregamento
│   ├── MenuScene.js        # Menu principal
│   └── GameScene.js        # Loop de gameplay
├── entities/
│   ├── Player.js           # Lógica do jogador com representação geométrica
│   └── Enemy.js            # Lógica do inimigo com pool/geometria
├── systems/
│   ├── InputManager.js     # Mapeamento teclado e toque virtual
│   └── AudioManager.js     # Listener de eventos para SFX e BGM
└── index.js                # Ponto de entrada
```

### Estrutura para Jogo 3D (Babylon.js)

```text
src/
├── config/
│   ├── engineConfig.js     # Canvas, antialias, WebGL/WebGPU
│   └── assetRegistry.js    # Mapeamento de meshes primitivas e materiais
├── events/
│   └── EventBus.js         # Observable / EventEmitter central
├── scenes/
│   ├── LoadingScreen.js    # UI de carregamento e progresso
│   ├── BaseScene.js        # Setup de câmera, iluminação e sombras
│   └── GameScene.js        # Loop principal da cena 3D
├── entities/
│   ├── PlayerMesh.js       # Mesh geométrica, controles e animações lógicas
│   └── EnemyPool.js        # Pool de meshes primitivas reutilizáveis
├── systems/
│   ├── InputManager.js     # Entradas de toque/mouse/teclado
│   └── AudioManager.js     # Áudio espacial e listener de eventos
└── index.js                # Inicialização da engine e do loop
```

---

## 7. Referências e Exemplos de Código

- [Padrões Phaser 3 (2D)](./references/phaser2d-patterns.md)
- [Padrões Babylon.js (3D)](./references/babylon3d-patterns.md)

