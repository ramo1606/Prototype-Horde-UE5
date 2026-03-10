**TPS Survival Game**

Plan de Implementación

UE5 · Kay Lousberg Assets · Mobile / Switch 2 / PC · 2 hs/día

**Resumen de Fases**

| Fase | Nombre | Duración | Horas |
| :---- | :---- | :---- | :---- |
| FASE 0 | Fundación del Proyecto | 2 semanas | \~28 hs |
| FASE 1 | Personaje y Cámara | 2 semanas | \~28 hs |
| FASE 2 | Combate Base | 2.5 semanas | \~35 hs |
| FASE 3 | Enemy AI | 3 semanas | \~42 hs |
| FASE 4 | Wave System y Game Loop | 2 semanas | \~28 hs |
| FASE 5 | Armas Adicionales y Contenido | 2 semanas | \~28 hs |
| FASE 6 | Mapa y Arte Final | 1.5 semanas | \~21 hs |
| FASE 7 | Polish y Optimización | 3 semanas | \~42 hs |
| **TOTAL** |  | **18 semanas** | **\~252 hs** |

**FASE 0  Fundación del Proyecto**

**2 semanas · \~28 hs**

*Proyecto configurado correctamente antes de escribir lógica de juego. Mobile-first desde el primer día.*

| Tarea | Estimado |
| :---- | :---- |
| Crear proyecto Blank (no Third Person template) | 2 días |
| Configurar Android SDK, conectar dispositivo target para preview en vivo | 1 día |
| Deshabilitar Lumen, VSM, Motion Blur, Bloom · Activar Mobile Forward Renderer | 1 día |
| Desactivar plugins innecesarios (Chaos Vehicles, Water, Foliage, PCG, Movie Render Queue) | 0.5 días |
| Configurar Scalability Groups para Mobile / Switch 2 / PC en DefaultScalability.ini | 1 día |
| Enhanced Input: 4 acciones base (mover, rotar cámara, apuntar, disparar) | 1 día |
| Master Material de cel shading (NdotL cuantizado en 2 niveles \+ gradient atlas de Kay) | 2 días |
| Post Process Volume: deshabilitar defaults, agregar outline via Custom Depth stencil | 2 días |
| 3 materiales de prueba con la paleta de colores definitiva del juego | 1 día |

*→ Checkpoint: Cubo con cel shading \+ outline corriendo a 60fps estables en el dispositivo target.*

**FASE 1  Personaje y Cámara**

**2 semanas · \~28 hs**

*El núcleo de feel del juego. Todo lo demás depende de que esto se sienta bien.*

| Tarea | Estimado |
| :---- | :---- |
| Importar FBX de Prototype Bits · Confirmar Rig\_Medium · Generar Skeleton Asset | 1 día |
| Configurar ACharacter \+ CharacterMovementComponent (velocidad, aceleración, fricción) | 1 día |
| Animation Blueprint base: Idle, Walk, Run con blendspace 1D por velocidad | 2 días |
| Spring arm modo exploración: lag, longitud \~400 units, colisión con mundo | 1 día |
| Spring arm modo aim: shoulder offset (X:30 Y:60), arm \~150 units, FoV reducido | 1 día |
| Transición suave entre estados (interpolación de parámetros en Tick, \~0.2s) | 2 días |
| Orientación del personaje: locomotion-relative en exploración · aim-relative al apuntar | 1 día |
| Aim Offset: blendspace 2D pitch/yaw usando anims ranged de Kay (aim\_up/center/down) | 2 días |
| Tuning en dispositivo: dead zones de stick, sensibilidad, cambio de hombro L/R | 1 día |

*→ Checkpoint: Personaje moviéndose, cámara transicionando entre exploración y aim, sintiéndose bien en gamepad.*

**FASE 2  Combate Base**

**2.5 semanas · \~35 hs**

*La pistola se siente satisfactoria. Puedo moverme, apuntar, disparar y matar.*

| Tarea | Estimado |
| :---- | :---- |
| Definir Collision Channels: Player, Enemy, Projectile, WorldStatic | 0.5 días |
| Clase base AWeapon con interfaz de disparo (cadencia, munición, recarga) | 1 día |
| Pistola: hitscan con LineTraceSingleByChannel, cadencia, munición | 1 día |
| Socket en personaje, attachment de arma · Animación de disparo con anims Kay | 1 día |
| UHealthComponent reutilizable: recibir daño, muerte, eventos Blueprint | 1 día |
| Feedback de impacto: decal de bala en superficie, hit flash en enemigo | 1 día |
| Enemigo placeholder: mesh Kay \+ HealthComponent, puede recibir daño y morir | 1 día |
| Recoil: camera shake leve \+ spread incremental por disparo sostenido | 1 día |
| HUD mínimo en UMG: crosshair, vida, ammo actual / ammo total | 1 día |
| Animaciones de recarga usando anims ranged de Kay | 1 día |

*→ Checkpoint: Puedo disparar y matar enemigos estáticos. La pistola tiene feedback convincente.*

**FASE 3  Enemy AI**

**3 semanas · \~42 hs**

*Enemigos que detectan, persiguen y atacan. El juego ya es jugable en su forma más básica.*

| Tarea | Estimado |
| :---- | :---- |
| Configurar NavMesh en mapa de prueba · Ajustar parámetros para mobile | 1 día |
| AIController base \+ Blackboard (TargetActor, CanSeePlayer, DistanceToTarget) | 1 día |
| Perception System: vista con radio/ángulo · Detección del jugador | 1 día |
| Behavior Tree \- Estado Idle/Patrol: deambular entre patrol points | 1.5 días |
| Behavior Tree \- Estado Chase: perseguir al jugador por NavMesh | 1.5 días |
| Behavior Tree \- Estado Attack: llegar a rango y ejecutar ataque | 1 día |
| Animaciones dirigidas por AI: walk, run, attack, death con anims Kay | 2 días |
| Hit reaction: stagger leve al recibir daño (interrupt del behavior tree) | 1 día |
| Tuning de pathfinding para mobile: frecuencia de update, max paths simultáneos | 1 día |
| Balanceo de stats del enemigo (velocidad, daño, radio de detección) | 1 día |

*→ Checkpoint: Enemigos con tres estados funcionales. El combate tiene tensión real.*

**FASE 4  Wave System y Game Loop**

**2 semanas · \~28 hs**

*Loop completo. Wave empieza, enemigos spawnean, puedo morir o completar la wave.*

| Tarea | Estimado |
| :---- | :---- |
| AWaveManager: clase que controla secuencia de waves con FTimerHandle | 1 día |
| Data Asset de wave: cantidad de enemigos, tipos, cadencia, spawn points | 1.5 días |
| Spawn points distribuidos en el mapa · Lógica de activación por wave | 1 día |
| Sistema de score con combo multiplier (ventana de tiempo entre kills) | 1.5 días |
| Condición de derrota (jugador muere) · Condición de victoria (wave completada) | 1 día |
| Escalado de dificultad: curva de stats por wave con UCurveFloat | 1 día |
| HUD completo: vida, ammo, número de wave, score, combo multiplier, contador de enemigos | 2 días |
| Pantallas de Game Over y Wave Complete en UMG | 1 día |

*→ Checkpoint: El juego tiene principio, medio y fin. Score y condición de derrota funcionan.*

**FASE 5  Armas Adicionales y Contenido**

**2 semanas · \~28 hs**

*El juego tiene variedad real. Tres armas, dos tipos de enemigos, dificultad que escala con sentido.*

| Tarea | Estimado |
| :---- | :---- |
| Escopeta: hitscan multi-pellet con spread, daño por pellet, distancia corta | 1.5 días |
| Ametralladora: disparo automático, alta cadencia, spread incremental | 1.5 días |
| Sistema de pickup de armas: spawneables en el mapa, switch automático o manual | 1 día |
| Segundo tipo de enemigo con comportamiento distinto (rápido/débil o lento/tanque) | 2 días |
| Mix de tipos de enemigos por wave · Ajuste de dificultad con ambos tipos | 1 día |
| Animaciones del segundo enemigo con anims Kay (Large rig si aplica) | 1 día |

*→ Checkpoint: Sesiones de juego con decisiones de arma y escalado de dificultad perceptible.*

**FASE 6  Mapa y Arte Final**

**1.5 semanas · \~21 hs**

*El juego se ve como el juego. El mapa tiene personalidad y fluidez de movimiento.*

| Tarea | Estimado |
| :---- | :---- |
| Diseño del mapa en papel: layout de arena, flujo de movimiento, puntos de cobertura | 0.5 días |
| Construcción modular con props de Prototype Bits de Kay | 3 días |
| Iluminación baked con Lightmass · Lightmap resolution por mesh · Reflection Captures | 2 días |
| Ajuste del NavMesh con geometría final del mapa | 0.5 días |
| Aplicar master material y paleta a todos los assets en escena | 1 día |
| Revisión visual completa: consistencia de estilo, paleta, shader en todo el mapa | 1 día |

*→ Checkpoint: El mapa final está construido, iluminado y funciona con el NavMesh. Todo sigue el estilo.*

**FASE 7  Polish y Optimización**

**3 semanas · \~42 hs**

*Gold candidate. Jugable de principio a fin, se ve bien, corre estable en los tres targets.*

| Tarea | Estimado |
| :---- | :---- |
| Camera shake al disparar (leve) y al recibir daño (más pronunciado) | 0.5 días |
| Sonidos base: disparo x3, impacto en superficie, hit en enemigo, death, UI | 1.5 días |
| Partículas mínimas: muzzle flash, hit spark (muy bajo tricount, mobile-safe) | 1 día |
| Profiling en dispositivo con RenderDoc · Identificar draw call y GPU hotspots | 2 días |
| LODs automáticos en todos los meshes principales · Revisión manual de los críticos | 1.5 días |
| Occlusion culling audit · Instance Static Mesh para props repetidos | 1 día |
| Verificación de Scalability Groups en Switch 2 target y PC | 1 día |
| Memory audit: textures, meshes, audio · Comprimir atlas de Kay a 128x128 en mobile | 1 día |
| Playtesting completo: balance de waves, timing de spawn, curva de dificultad | 2 días |
| Bug fixing y pulido final | 2 días |

*→ Checkpoint: 30fps estables en mobile · 60fps en Switch 2 y PC · Memory budget dentro del target.*

**Fases Post-Launch (Opcionales)**

*A implementar una vez que el juego base esté lanzado y estable.*

**FASE A  Split-Screen Local (2-4 Jugadores)**

**1.5 semanas · \~21 hs**

*Coop o VS local sin tocar la arquitectura base. Feature aditiva de bajo riesgo.*

| Tarea | Estimado |
| :---- | :---- |
| Configurar múltiples PlayerControllers y viewport splitting en UGameViewportClient | 1 día |
| Cámara independiente por jugador (spring arm propio por PlayerController) | 2 días |
| Input routing: Enhanced Input mapeado por PlayerController | 1 día |
| HUD por jugador: instanciar UMG por viewport independientemente | 2 días |
| Spawn y respawn de jugadores en el mapa con posiciones diferenciadas | 1 día |
| Escalar cantidad de enemigos en waves según cantidad de jugadores activos | 1 día |
| Lógica condicional: 2p en mobile · 2-4p en Switch 2 y PC | 0.5 días |
| Testing y ajuste de edge cases (jugador muere, rejoin, fin de wave) | 2 días |

*→ Checkpoint: 2 jugadores en split-screen en PC y Switch 2 · 2 jugadores en mobile si el framerate lo permite.*

⚠ Limitar a 2 jugadores en mobile (Adreno 610 no tiene headroom para 4 viewports).

⚠ 4 jugadores habilitado solo en Switch 2 y PC vía detección de plataforma.

⚠ No requiere refactorización del código base — es puramente aditivo.

**FASE B  Multijugador Online Simple (2-4 Jugadores)**

**4 semanas · \~56 hs**

*Coop o VS online con arquitectura server-authoritative. Requiere refactorización de sistemas core.*

| Tarea | Estimado |
| :---- | :---- |
| Refactorizar HealthComponent: replicación de variable HP \+ RPC de daño | 1 día |
| Refactorizar WeaponSystem: disparo server-authoritative · efectos visuales solo en cliente | 3 días |
| Wave Manager replicado: estado de wave y spawn visibles a todos los clientes | 2 días |
| Game State y Score replicados en AGameStateBase | 1 día |
| Lobby simple: host / join · lista de jugadores · ready up | 3 días |
| Online Subsystem Steam: sesiones, matchmaking básico, invites para PC | 2 días |
| Online Subsystem Nintendo: integración con Nintendo Switch Online para Switch 2 | 1 día |
| Latency compensation básica para hitscan (client-side prediction del trace) | 3 días |
| Testing en red real: latencia, packet loss, desync edge cases | 4 días |

*→ Checkpoint: Sesión online estable con 2-4 jugadores en PC (Steam) y Switch 2 (Nintendo Online).*

⚠ ADVERTENCIA: online no es una feature, es una refactorización. Todo sistema de juego necesita ser replication-aware.

⚠ Implementar siempre después del juego base lanzado — subestimar este scope es el error más común en dev solo.

⚠ El framework de replicación de UE5 es excelente, pero el debugging en red real es costoso en tiempo.

⚠ Latency compensation para hitscan es el ítem más complejo — en coop puede simplificarse con lag acceptance generoso.

**Comparativa de Fases Post-Launch**

| Feature | Costo | Impacto en código base | Viabilidad mobile |
| :---- | :---- | :---- | :---- |
| Split-screen 2p | \~1 semana | Bajo — aditivo | Viable con ajustes |
| Split-screen 4p | \~1.5 semanas | Bajo — aditivo | Solo PC / Switch 2 |
| Online simple 2-4p | \~4 semanas | Alto — refactorización | Viable, latencia peor |
| Ambos combinados | \~6 semanas | Alto | Online split no recomendado en mobile |

**Notas del Plan**

1. Los estimados asumen 2 horas diarias de trabajo enfocado e incluyen overhead de compilaciones de shader y setup de UE5.

2. Nunca más de una semana sin algo jugable — cada fase termina con un checkpoint verificable.

3. Mobile target (Adreno 610\) es el piso en todo momento. Switch 2 y PC son mejoras aditivas vía Scalability Groups.

4. Los assets de Kay Lousberg (Prototype Bits \+ Character Animations) están presentes desde Fase 0, eliminando \~2 semanas del timeline original.

5. El rig de Kay (Rig\_Medium) se usa directamente sin retargeting al Mannequin de UE5 para evitar fricción.

6. La gradient atlas de Prototype Bits (128x128 en mobile) permite batching agresivo de draw calls con Instanced Static Mesh.

7. Riesgo principal: las compilaciones de shader de UE5 son lentas. Trabajar siempre con el dispositivo conectado, no confiar solo en el editor.