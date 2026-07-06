# 🐦 Agaporni Sequencer

Una aplicación web interactiva para crear y secuenciar sonidos de loros agapornis (inseparables). Una herramienta lúdica y musical para explorar los cantos naturales de estas aves.

**[🎵 Abre la aplicación aquí](https://agaporni-sequencer.vercel.app)** | [Versión con muestras de audio real](https://agaporni-sequencer.vercel.app/agaporni-sequencer-samples.html)

## ¿Qué es?

Agaporni Sequencer es un secuenciador de 8 pistas basado en cuadrícula donde cada pista representa un tipo diferente de canto del loro agapornis:

- 🐤 **Chirrido** (Chirp)
- 🎶 **Trino** (Trill)
- 🎵 **Silbido** (Whistle)
- 🗣 **Parloteo** (Chatter)
- 🦜 **Graznido** (Squawk)
- 🐣 **Pío suave** (Tweet)
- 🎼 **Gorjeo** (Warble)
- 🔔 **Pitido** (Peep)

## Características

✨ **Fácil de usar**: Interfaz intuitiva basada en clics y arrastres
🎵 **Sonido realista**: Síntesis de audio Web Audio API que emula los cantos naturales
⚡ **En tiempo real**: Reproduccción sincronizada a tempo ajustable (40-300 BPM)
🎨 **Presets musicales**: Patrones predefinidos (Parloteo, Melodía, Ritmo, Dúo, Aleatorio)
📱 **Responsive**: Funciona en desktop, tablet y móvil
🎹 **Controles por teclado**: Atajos rápidos para reproducción y silenciado de pistas
💾 **Sin dependencias**: 100% HTML/CSS/JavaScript, sin build process

## Cómo usar

### Reproducción básica

1. Abre la aplicación en tu navegador
2. **Haz clic en las celdas** del grid para activar notas
3. Presiona **Play** (o **Espacio**) para reproducir
4. Ajusta el **tempo (BPM)** para cambiar la velocidad

### Edición avanzada

- **Arrastrar para pintar**: Mantén presionado y arrastra para activar varias celdas
- **Silenciar pistas**: Haz clic en el número de pista (1-8) o en el botón 🔊
- **Cambiar compases**: Selecciona 1, 2, 3, 4, 6 u 8 compases
- **Subdivisiones**: Cambia entre 1/2, 1/3, 1/4, 1/6 o 1/8 de nota
- **Presets**: Aplica patrones predefinidos con los botones de preset

### Atajos de teclado

| Tecla | Función |
|-------|---------|
| **Espacio** | Play / Pause |
| **Esc** | Stop |
| **1-8** | Silenciar pista |

## Tecnología

- **Web Audio API** — Síntesis de sonido en tiempo real
- **Vanilla JavaScript** — Sin frameworks, sin dependencias
- **CSS Grid & Flexbox** — Diseño responsive
- **HTML5** — Semántica moderna

## Dos versiones

### 1. **agaporni-sequencer.html** (recomendado)
Sonidos sintetizados usando osciladores de Web Audio API. Cada tipo de canto tiene su propio algoritmo de síntesis que emula las características acústicas del ave.

### 2. **agaporni-sequencer-samples.html**
Versión con muestras de audio real de loros agapornis. Usa archivos MP3 pregrabados de `lovebird_clips/`.

## Instalación local

No requiere instalación. Solo necesitas un navegador moderno:

```bash
# Opción 1: Abre el archivo directamente
open agaporni-sequencer.html

# Opción 2: Usa un servidor local (recomendado para la versión de samples)
python -m http.server 8000
# Luego visita: http://localhost:8000
```

**Requisitos:**
- Navegador con soporte Web Audio API (Chrome, Firefox, Safari, Edge)
- Para la versión con samples: servidor local debido a CORS

## Desarrollo

Ver [CLAUDE.md](CLAUDE.md) para guía de arquitectura, patrones de desarrollo y cómo extender la aplicación.

## Agregar nuevos sonidos

Para crear síntesis personalizadas:

1. Añade una entrada a la constante `TRACKS`
2. Implementa el algoritmo de síntesis en la función `playSound()`
3. (Opcional) Añade patrones para el nuevo sonido en `applyPreset()`

Ver sección "Extending the Application" en CLAUDE.md.

## Limitaciones conocidas

- Sin función de guardar/cargar (los datos se pierden al recargar la página)
- Sin soporte MIDI
- Sin grabación o exportación de audio
- Los algoritmos de síntesis son artísticos, no acústicamente precisos

## Posibles mejoras

- 💾 Guardar/cargar secuencias en JSON o localStorage
- 🎛 MIDI input para control externo
- 📹 Grabación y exportación de audio (WAV/MP3)
- 🎨 Selector visual de presets
- 📊 Visualizador de espectro
- 🌍 Más idiomas

## Licencia

MIT License — Libre para usar, modificar y distribuir.

## Créditos

Inspirado en la belleza de los cantos del loro agapornis y en la tradición de los secuenciadores de música electrónica.

---

**¿Preguntas? ¿Sugerencias?**  
Abre un issue o contacta al desarrollador.

Hecho con 🐦 y amor por los inseparables.
