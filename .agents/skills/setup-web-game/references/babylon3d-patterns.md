# Padrões e Referências para Babylon.js (3D)

## 1. Asset Registry com Primitivas Geométricas

Gera meshes nativas coloridas e reserva os slots de carregamento de GLTF/GLB:

```javascript
// src/config/assetRegistry.js
import * as BABYLON from '@babylonjs/core';

export const MESH_TYPES = {
  PLAYER: 'player',
  ENEMY: 'enemy',
  GROUND: 'ground',
};

export function createPrimitiveMesh(scene, type) {
  let mesh;
  const mat = new BABYLON.StandardMaterial(`${type}Mat`, scene);

  switch (type) {
    case MESH_TYPES.PLAYER:
      mesh = BABYLON.MeshBuilder.CreateBox('playerMesh', { size: 1 }, scene);
      mat.diffuseColor = new BABYLON.Color3(0.13, 0.77, 0.36); // Verde
      break;
    case MESH_TYPES.ENEMY:
      mesh = BABYLON.MeshBuilder.CreateSphere('enemyMesh', { diameter: 1 }, scene);
      mat.diffuseColor = new BABYLON.Color3(0.93, 0.26, 0.26); // Vermelho
      break;
    default:
      mesh = BABYLON.MeshBuilder.CreatePlane('defaultMesh', { size: 1 }, scene);
      mat.diffuseColor = new BABYLON.Color3(0.5, 0.5, 0.5);
  }

  mesh.material = mat;
  return mesh;
}
```

---

## 2. EventBus com Observable do Babylon.js

```javascript
// src/events/EventBus.js
import { Observable } from '@babylonjs/core';

export const EventBus = {
  playerDamaged: new Observable(),
  itemCollected: new Observable(),
  triggerSfx: new Observable(),
  triggerAnimation: new Observable(),
};
```

---

## 3. Otimização de Performance e Mobile

- Ajustar escala de resolução para dispositivos móveis (`engine.setHardwareScalingLevel(1 / window.devicePixelRatio)` ou valor fixo em aparelhos de baixo desempenho).
- Desconectar listeners de redimensionamento e liberar memória com `scene.dispose()` e `engine.dispose()`.
- Utilizar instancing (`mesh.createInstance()`) para múltiplos objetos idênticos.
