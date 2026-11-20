# 🌌 Voyager — Constellation Game (v2.0)

> *"The cosmos is within us. We are made of star-stuff."* — Carl Sagan

**Voyager** es un videojuego web interactivo de astronomía y rompecabezas construido con **HTML5 Canvas** y **Vanilla JavaScript**, sin dependencias externas pesadas. El jugador explora el firmamento, conecta las estrellas de constelaciones reales y descubre la historia y mitología que la humanidad ha proyectado en el cielo durante milenios.

---

## 🎮 Modos de Juego y Mecánicas

### 1. Modo Travesía (Modo Principal)
- **8 Constelaciones históricas**: Orión, Cassiopeia, Osa Mayor, Leo, Escorpio, Cruz del Sur, Lyra y Cisne.
- **Identificación Estelar**: Hover en cualquier estrella para consultar su nombre astronómico real (Betelgeuse, Rigel, Antares, Vega, Deneb, etc.).
- **Validación de Conexiones**: Conexión correcta dibuja líneas lumínicas con estela azulada; error produce vibración reactiva, destello y penalización de puntaje.
- **Sistema de Puntuación**: Puntos base, bonificación por tiempo de resolución y multiplicadores de dificultad.

### 2. Modo Libre (Sandbox Creativo)
- Permite hacer clic en cualquier punto del lienzo para crear nuevas estrellas.
- Conecta estrellas a voluntad para forjar constelaciones personalizadas.
- Doble clic en una estrella para eliminarla.
- Asigna un nombre a tu constelación y descárgala en **PNG** en alta resolución.

---

## ✨ Características Destacadas (v2.0 Futurista)

| Característica | Implementación Técnica |
|---|---|
| 🔊 **Audio Procedural** | Síntesis en tiempo real con **Web Audio API** (tonos de conexión armónicos, acordes de victoria, drones ambientales espaciales; 0 KB de archivos externos). |
| 🌌 **Nebulosas Dinámicas** | Renderizado multicapa con gradientes radiales procedurales temáticos para cada constelación. |
| 🌠 **Lore & Mitología** | Panel contextual post-victoria con narrativa histórica y mitológica de la constelación completada. |
| 📸 **Exportación Fotográfica** | Captura combinada de canvas (`toDataURL`) con marca de agua y branding estilizado listo para compartir. |
| 🏆 **Sistema de Logros** | 7 logros desbloqueables con notificaciones animadas en tiempo real. |
| 📊 **Marcador Local (Leaderboard)** | Registro persistente con iniciales estilo Arcade mediante `localStorage`. |
| ⏸️ **Pausa & Menú** | Pausa instantánea con tecla `ESC` o botón en interfaz, congelando timers y animaciones. |
| 📱 **Soporte Táctil Completo** | Optimizado para pantallas táctiles y dispositivos móviles. |

---

## 🗂️ Estructura del Proyecto

```
voyager/
├── index.html                  # Juego completo y autónomo (Canvas, VFX, Audio, UI)
├── README.md                   # Documentación principal
├── AGENTS.md                   # Guía de colaboración y estándares para Agentes IA
├── docs/
│   ├── game-design-doc.md      # Game Design Document (GDD) detallado
│   └── qa-checklist.md         # Checklist de pruebas QA exhaustivo
└── .agents/
    └── skills/
        └── voyager-game/
            ├── SKILL.md        # Definición de roles y contratos de agentes
            └── constellation-data.json # Base de datos astronómica estructurada
```

---

## 🚀 Cómo Ejecutar

No requiere compilación, servidores node ni instalación de paquetes:
1. Haz doble clic en [`index.html`](./index.html) o ábrelo en cualquier navegador web moderno (Chrome, Edge, Firefox, Safari).
2. Haz clic en **INICIAR VIAJE** o prueba el **MODO LIBRE**.

---

*Desarrollado como proyecto avanzado con arquitectura agéntica en Google Antigravity.*
