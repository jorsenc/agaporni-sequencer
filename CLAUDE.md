# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Agaporni Sequencer** is a browser-based music sequencer application for creating and layering lovebird (Agapornis) call sounds. It provides two implementations:

1. **agaporni-sequencer.html** — Synthesized lovebird sounds using Web Audio API oscillators and filters
2. **agaporni-sequencer-samples.html** — Real lovebird audio samples played back in the sequencer

Both files are fully self-contained HTML applications with no build process, dependencies, or server requirements.

## Architecture & Core Concepts

### Audio Engine

The application uses the **Web Audio API** to generate and playback sounds:

- **Sound Synthesis** (main version): Each of the 8 track types (chirp, trill, whistle, chatter, squawk, tweet, warble, peep) has a unique synthesis algorithm using oscillators, filters, and gain envelopes:
  - `playSound(trackIdx)` generates the audio for a single note based on frequency modulation, envelope shaping, and oscillator type
  - Each sound type uses different synthesis techniques (sine wave modulation, sawtooth filtering, LFO warbling, etc.)

- **Sample Playback** (samples version): Uses `<audio>` elements and the AudioContext to load and play pre-recorded bird clips from `lovebird_clips/` directory

### Sequencer Logic

The sequencer operates on a time-division-multiplexed grid:

- **Grid Structure**: `grid[trackIndex][stepIndex]` — Boolean array tracking which notes are active
- **Timing**: Uses a scheduler pattern with Web Audio's `audioCtx.currentTime` for precise scheduling
  - `scheduler()` function runs on a lookahead timer (`lookahead = 25ms`) to schedule notes ahead of playback
  - `nextStep()` advances the playhead and plays active notes for the current step
  - Tempo is adjustable (40–300 BPM) and converted to per-step duration

### Grid Configuration

Users can adjust:
- **Bars** (measures): 1, 2, 3, 4, 6, or 8
- **Subdivisions**: 1/2, 1/3, 1/4, 1/6, or 1/8 (steps per beat)
- **Total steps** = `numBars × 4 (beats/bar) × stepsPerBeat`

### Tracks

8 hardcoded lovebird sound tracks with metadata:
```javascript
{
  id: 'chirp',          // Unique identifier
  name: 'Chirrido',     // Localized name (Spanish)
  emoji: '🐤',          // Visual indicator
  color: '#00b894',     // UI color for the cell
  freq: 1800,           // Base frequency for synthesis
  type: 'chirp'         // Synthesis algorithm type
}
```

### UI & Interaction

- **Track Panel** (left): Mute toggles for each track (click track name or press 1–8)
- **Grid** (center): Click cells to toggle, drag to paint multiple cells
- **Beat Labels**: Shows measure and beat numbers; bar boundaries are highlighted
- **Playhead**: Vertical line showing current playback position
- **Transport**: Play/Pause, Stop, Tempo, Clear
- **Presets**: Chatter, Melody, Rhythm, Duet, Random patterns
- **Keyboard**: Space = Play/Pause, Esc = Stop, 1–8 = Mute tracks

## File Organization

```
AGAPORNIS/
├── agaporni-sequencer.html              # Synthesized sounds (main version)
├── agaporni-sequencer-samples.html      # Real samples version
├── lovebird_clips/                      # Audio samples
│   ├── chirp_1.mp3, chirp_2.mp3
│   ├── trill_1.mp3, trill_2.mp3
│   ├── whistle_1.mp3, whistle_2.mp3
│   ├── chatter_1.mp3, chatter_2.mp3
│   ├── squawk_1.mp3, squawk_2.mp3
│   ├── tweet_1.mp3, tweet_2.mp3
│   ├── warble_1.mp3, warble_2.mp3
│   └── peep_1.mp3, peep_2.mp3
└── CLAUDE.md                            # This file
```

## Development & Deployment

### Running Locally

No build or installation required. Simply open either HTML file in a modern browser:

```powershell
# Using PowerShell on Windows
start agaporni-sequencer.html

# Or drag the file to your browser
# Or use a local web server
python -m http.server 8000   # Then visit http://localhost:8000
```

### Browser Requirements

- **Modern browser** with Web Audio API support (Chrome, Firefox, Safari, Edge)
- **CORS**: If serving locally with samples version, use a local server (file:// URLs may not load audio due to CORS)
- **Audio permission**: Browser will request microphone/audio permissions on first play

### Deployment

Since there's no build process:

1. **Static hosting**: Upload the HTML files and `lovebird_clips/` folder to any static host (GitHub Pages, Netlify, Vercel, AWS S3, etc.)
2. **File structure**: Keep `lovebird_clips/` in the same directory as the HTML files or update the path in the script
3. **MIME types**: Ensure audio files are served with correct MIME type (`audio/mpeg` for .mp3)

## Key Functions & Patterns

### State Management

- `grid[trackIdx][stepIdx]` — Tracks which cells are active
- `muted[trackIdx]` — Tracks which tracks are muted
- `isPlaying`, `currentStep`, `tempo`, `nextStepTime` — Playback state

### UI Rendering Functions

- `buildGrid()` — Rebuilds the grid DOM when bar/subdivision settings change
- `buildTrackPanel()` — Renders the track list with mute controls
- `updatePlayhead()` — Updates the visual playhead position (CSS left %)

### Audio & Timing

- `initAudio()` — Initializes AudioContext (called on first user interaction)
- `scheduler()` — Main timing loop; schedules notes ahead of current time
- `nextStep()` — Advances playhead and plays active notes
- `playSound(trackIdx)` — Synthesizes and plays a single note for a track

### Presets

`applyPreset(pattern)` fills the grid with predefined patterns:
- **chatter** — Rapid chirps and chatter
- **melody** — Melodic whistles with supporting trills
- **rhythm** — Steady beat with hi-hat-like peeps
- **duet** — Two complementary voices alternating
- **random** — Random notes across all tracks

## Extending the Application

### Adding New Sounds

To add a new lovebird sound type:

1. **Add track to TRACKS array**:
   ```javascript
   { id: 'new-sound', name: 'Name', emoji: '🐦', color: '#abc123', freq: 1200, type: 'new-sound' }
   ```

2. **Add synthesis algorithm in `playSound()`**:
   ```javascript
   case 'new-sound': {
     osc.type = 'sine';
     osc.frequency.setValueAtTime(t.freq, now);
     gainNode.gain.setValueAtTime(0.2, now);
     gainNode.gain.exponentialRampToValueAtTime(0.01, now + dur);
     osc.start(now); osc.stop(now + dur);
     break;
   }
   ```

3. **Update presets** in `applyPreset()` to include the new track if desired

### Using Real Samples (Samples Version)

The samples version loads audio clips from `lovebird_clips/`. To modify:

1. Replace clips in `lovebird_clips/` with different bird sounds
2. Update the `TRACKS` array if track types change
3. Adjust file paths in the JavaScript if folder structure changes

### Styling Customization

CSS variables in `:root` control the entire color scheme:
- `--bg`, `--surface`, `--surface2` — Background colors
- `--accent`, `--accent2` — Primary UI colors
- `--cell-on`, `--cell-on-alt` — Active cell colors
- `--text`, `--text2` — Text colors

### Localization

The UI is in Spanish. To change to another language, search for all Spanish text (e.g., "Reproducir", "Compases") and replace with the target language.

## Performance Considerations

- **Polyphony**: All 8 tracks can play simultaneously without issues on modern browsers
- **Scheduling**: Uses a lookahead timer pattern to avoid missed notes due to main-thread blocking
- **Memory**: Grid size is `8 tracks × (numBars × 4 × stepsPerBeat) steps` — even with 8 bars and 8 subdivisions (~256 cells per track), memory usage is negligible
- **Rendering**: DOM updates only occur when grid configuration changes, not during playback

## Testing

Since this is a single-file app with no automated tests:

1. **Manual testing** in browser DevTools (F12 → Console)
2. **Check logs**: The script logs initialization to console
3. **Test audio**: Verify sound synthesis/playback works by toggling cells and adjusting tempo
4. **Browser compatibility**: Test in multiple browsers (Chrome, Firefox, Safari, Edge)
5. **Mobile**: Responsive design adapts to smaller screens (tablet/mobile)

## Known Limitations & Future Improvements

- No save/load functionality (sequences are lost on page refresh)
- No MIDI input/output support
- Samples version requires CORS-compliant hosting for local development
- No audio recording or export capability
- Synthesis algorithms are stylized for character, not acoustic accuracy

## Debugging Tips

### No Sound?

1. Check browser console for errors (F12)
2. Verify AudioContext state: `console.log(audioCtx.state)` — should be 'running' after clicking Play
3. Confirm browser allows audio playback (not muted globally)
4. Test in a different browser

### Playback Timing Issues?

1. Check tempo value (40–300 BPM)
2. Verify lookahead timer is running: `console.log(schedulerTimer)`
3. Try adjusting `lookahead` constant (currently 25ms) if notes are missed

### Grid Not Rendering?

1. Clear browser cache and reload
2. Check that TRACKS array and grid dimensions match
3. Verify no JavaScript errors in console

---

Last updated: 2026-07-06
