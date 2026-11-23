# Padrões e Referências para Phaser 3 (2D)

## 1. Asset Registry com Placeholders e Triggers

Centraliza a geração de gráficos primitivos e mapeia onde futuros assets e animações serão injetados:

```javascript
// src/config/assetRegistry.js
export const ASSET_TYPES = {
  PLAYER: 'player',
  ENEMY: 'enemy',
  ITEM: 'item',
};

export const COLOR_PLACEHOLDERS = {
  [ASSET_TYPES.PLAYER]: 0x22c55e, // Verde
  [ASSET_TYPES.ENEMY]: 0xef4444,  // Vermelho
  [ASSET_TYPES.ITEM]: 0xeab308,   // Amarelo
};

export function createGeometricTexture(scene, key, color, width = 32, height = 32) {
  // Cria textura sólida em memória para evitar requisições externas
  const graphics = scene.make.graphics({ x: 0, y: 0, add: false });
  graphics.fillStyle(color, 1);
  graphics.fillRect(0, 0, width, height);
  graphics.generateTexture(key, width, height);
  graphics.destroy();
}
```

---

## 2. EventBus (Observable / PubSub)

```javascript
// src/events/EventBus.js
import Phaser from 'phaser';

// Instância compartilhada de EventEmitter para comunicação desacoplada
export const EventBus = new Phaser.Events.EventEmitter();

export const GAME_EVENTS = {
  PLAYER_MOVED: 'player:moved',
  PLAYER_DAMAGED: 'player:damaged',
  ITEM_COLLECTED: 'item:collected',
  TRIGGER_SFX: 'audio:sfx',
  TRIGGER_ANIMATION: 'anim:trigger',
};
```

---

## 3. Configuração Mobile-First

```javascript
// src/config/gameConfig.js
import Phaser from 'phaser';

export const gameConfig = {
  type: Phaser.AUTO,
  scale: {
    mode: Phaser.Scale.FIT,
    autoCenter: Phaser.Scale.CENTER_BOTH,
    width: 360,
    height: 640, // Base portrait para mobile
  },
  physics: {
    default: 'arcade',
    arcade: { gravity: { y: 600 }, debug: false },
  },
  input: {
    activePointers: 3, // Suporte a múltiplos toques simultâneos
  },
};
```
