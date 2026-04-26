EMDR Light Bar Mage – Bilateral Stimulation Session Tool
EMDR Light Bar Mage is an advanced web application for EMDR (Eye Movement Desensitization and Reprocessing) bilateral stimulation. It combines a visual light bar, synthesised tones, haptic/midi/serial extensions, session recording, and real‑time synchronisation over WebSocket. The interface is designed to be fully customisable, supporting therapeutic bilateral stimulation in a clinical or home‑practice setting.

Table of Contents
What is EMDR & Bilateral Stimulation?
Key Features
Requirements & Browser Support
Getting Started
User Interface Guide
Top HUD (Status Bar)
Main Canvas – Light Bar & Oscilloscope
Control Panel Modules
How to Use for EMDR Sessions
Device Integration (BLE, MIDI, Serial, Haptics)
WebSocket Sync – Multi‑User / Co‑Therapy
Session Recording & Replay
Presets & Customisation
Keyboard Shortcuts
Technical Notes (Audio / Canvas)
Troubleshooting
Clinical Disclaimer

What is EMDR & Bilateral Stimulation?
EMDR is a psychotherapy technique that helps process traumatic memories. One of its core components is bilateral stimulation – rhythmic left‑right sensory input (visual, auditory, or tactile). This app provides:
Visual: a luminous dot / bar moving left‑right along a track.
Auditory: alternating tones panned left/right, with adjustable pitch, envelope, filter and effects.
Tactile: optional vibration via phone haptics, BLE vibrators, MIDI or serial devices.
The user follows the moving light with their eyes while the brain processes the targeted memory. The therapist (or user) controls the speed, intensity, sound design and external devices.

Key Features
Fully adjustable bilateral light bar – speed (0.2 – 5 Hz), brightness (up to 200%), colour / hue, glow, bar height, saturation.
Real‑time oscilloscope – shows the audio waveform; can be set to follow the light’s vertical position (wave follow mode).
Sophisticated audio engine – low‑pass filter with envelope, LFO modulation, delay, reverb, chorus, stereo width, and multiple waveforms (sine, square, triangle, sawtooth).
External device support – Bluetooth LE vibrator, MIDI output, USB Serial vibrator, and browser haptic (vibrate).
Session recording & replay – record every parameter change and play back the entire session (useful for supervision or client review).
Multi‑user synchronisation – over WebSocket (any room name) – all connected clients share the exact same state (speed, colour, sound, phase).
Presets – three built‑in (pulse, click, deep drone) plus unlimited custom presets saved to local storage.
Edge flashes & visual feedback – when the light reaches the edges, a dramatic coloured flash occurs, triggering auditory and haptic pulses.
Fully responsive – works on desktop and mobile (though external devices require appropriate APIs).

Requirements & Browser Support
Feature
Browser Requirement
Core visual + audio
Any modern browser (Chrome, Edge, Firefox, Safari) with WebAudio support.
WebSocket sync
Any browser (WebSocket API).
Bluetooth LE vibrator
Chrome / Edge on desktop or Android (requires HTTPS or localhost).
MIDI output
Chrome / Edge (WebMIDI).
USB Serial vibrator
Chrome / Edge (WebSerial, requires HTTPS / localhost).
Haptics (vibrate)
Most mobile browsers, some desktop (navigator.vibrate).

Important: For BLE / Serial / MIDI, you must serve the page via https:// or localhost:// because these APIs are restricted to secure contexts. Open the .html file directly from a local web server (e.g. npx serve, python -m http.server, or VS Code Live Server).

Getting Started
Download the HTML file (or clone the repository).
Serve it locally:
bash
npx serve .
# or
python -m http.server 8000
Open http://localhost:8000 (or your custom address).
Click anywhere on the page to enable audio (it will resume the AudioContext).
Press the Play button (⏯) in the top HUD to start the bilateral movement.
All parameters are editable in real‑time – the light and sound will update immediately.

User Interface Guide
Top HUD (Status Bar)
Section
Controls
Sync
WebSocket server URL, room name, Link / Cut, resync button. Status LED and VFD show connection.
Devices
Status chips (haptic, BLE, MIDI, serial). BLE, MIDI, USB buttons connect external vibrators. Test sends a test left+right pulse.
Global
⏯ Play/Pause, 🔇 Mute, ⛶ Fullscreen, Tips toggle (shows tooltips on hover).

Main Canvas – Light Bar & Oscilloscope
Moving light – follows sine‑wave motion left‑right. When it hits an edge, a coloured flash occurs, a tone is played, and external vibrators fire (if connected).
Oscilloscope – shows the real‑time audio waveform (top‑left toggle scope).
Spectrum Mode – when enabled, the light’s colour cycles through hues based on position (overrides manual hue).
Wave Follow – light’s vertical position follows the oscilloscope’s current amplitude (creating a dancing trail).
Control Panel Modules
The bottom panel is organised into scrollable blocks:
Module
Parameters (knobs)
Speed
main speed (Hz) – large knob.
Visual
Brightness, Hue, Glow, Bar stretch, Saturation, Edge intensity, Scope height, Scope brightness. Toggles for scope, spectrum, wave follow, plus background colour picker.
Synth
Volume, Pitch (tone freq), Attack, Decay, Harmonics (overtones), Filter cutoff, Filter Q, Filter envelope (filter follows note envelope). Waveform selector (sine/triangle/square/sawtooth).
Effects
Echo delay time & feedback & mix, Reverb mix, Chorus mix & rate & depth, Stereo width. LFO rate & depth & destination (cutoff, volume, pan) and LFO waveform.
Preset
Dropdown for built‑in / custom presets; Rand random pitch generator; 💾 save current as custom; ✕ delete custom.
Session
Record, Stop, Play, Save (to localStorage), Load (from localStorage), session timer.


How to Use for EMDR Sessions
Prepare the environment – sit in a quiet room, either as a therapist guiding a client or as a self‑practitioner.
Set a comfortable speed – start slow (e.g. 0.5 Hz) and gradually increase according to the client’s tolerance.
Adjust light intensity – set brightness and bar stretch so the movement is clearly visible without being overwhelming.
Enable audio – unmute and adjust volume / pitch. Many EMDR protocols use alternating tones that match the visual rhythm. The synthesised ping is triggered exactly at each edge.
Activate external devices (optional) – connect a BLE vibrator (e.g. Lovense, or any device with a write characteristic) or a USB serial vibrator that responds to l and r commands. This adds tactile bilateral stimulation.
Start the session – press ⏯ Play. The client tracks the light with their eyes while the therapist may adjust speed or sound dynamically.
Record the session (optional) – hit ● rec before starting. Every parameter change will be saved. After the session, you can ● rec again to stop, then ▶ play to review the exact sequence – very useful for supervision.
Multi‑client synchronisation – if multiple devices are in the same room (e.g. two screens for a client and a therapist), connect them to the same WebSocket server and room. All adjustments will be mirrored live.

Device Integration (BLE, MIDI, Serial, Haptics)
Haptics (Phone Vibration)
If the browser supports navigator.vibrate, the chip shows on. No extra configuration – every time the light reaches an edge, the device will vibrate for 50 ms.
Bluetooth LE Vibrator
Click BLE – a browser picker opens.
Choose a BLE device that has a writable characteristic.
The app expects a simple protocol:
Left edge → write [1, 80, 0, 80]
Right edge → write [2, 80, 0, 80]
(These bytes simulate a vibrator intensity; you may need to adapt the code for your specific device.)
MIDI Output
Click MIDI – request access.
Select the target MIDI output.
Left edge sends note 55 (velocity 100), right edge sends note 60 (both with note‑off after 60 ms).
USB Serial Vibrator
Click USB – choose a serial port with baud rate 9600.
Left edge sends "l\n", right edge sends "r\n".
(You can build a simple Arduino vibrator that listens for these commands.)
Test Button
The test ⚡ button sends a quick left‑right pulse (audio + all connected devices) – useful for verifying everything works before a session.

WebSocket Sync – Multi‑User / Co‑Therapy
The app can synchronise multiple browser tabs or different computers over a WebSocket server.
Default server: ws://localhost:8080 (you must run your own WebSocket server).
Room concept: any string (e.g. emdr-1). Clients in the same room share state.
Minimal WebSocket server (Node.js)
javascript
const WebSocket = require('ws');
const server = new WebSocket.Server({ port: 8080 });
const rooms = new Map();

server.on('connection', ws => {
  ws.on('message', data => {
    const msg = JSON.parse(data);
    if (msg.type === 'join') ws.room = msg.room;
    if (msg.room) {
      server.clients.forEach(client => {
        if (client !== ws && client.room === msg.room && client.readyState === WebSocket.OPEN) {
          client.send(JSON.stringify(msg));
        }
      });
    }
  });
});
console.log('WebSocket relay running on ws://localhost:8080');
After starting the server, set the URL (e.g. ws://localhost:8080), enter a room, and click Link. The sync-led turns green and the VFD shows linked. Any parameter change on any client will propagate to all others, including play/pause and phase.
Resync button forces a phase sync to avoid drift between clients.

Session Recording & Replay
All control changes (knobs, buttons, toggles, presets, even random mode) are automatically recorded while recording is active.
Press ● rec – the timer starts.
Perform the session – adjust speed, sound, visual parameters, etc.
Press ■ stop – recording ends.
Press ▶ play – the entire sequence replays exactly as it happened (including the exact timing of state changes).
💾 save – stores the session in the browser’s localStorage under a name you provide.
📂 load – loads a previously saved session so that it can be replayed again.
Replay will automatically pause the current movement and apply the recorded timeline. You can stop replay at any time.

Presets & Customisation
Built‑in Presets
pulse – soft, warm bass pulse (61 Hz, sine wave, gentle decay).
click – sharp, high‑pitched click for distinct bilateral cues.
deep drone – low sawtooth with reverb and LFO modulation, sustained feel.
Saving a Custom Preset
Dial in your desired sound and visual parameters.
Click 💾, enter a name.
The preset appears in the dropdown with a star (★).
To delete a custom preset, select it and press ✕.
Random Pitch Mode
Click the Rand button (it lights up). Whenever the light hits an edge, the pitch (toneFreq) randomly jumps between 80 Hz and 1200 Hz (logarithmic). This can be used in creative / exposure‑based protocols.

Keyboard Shortcuts
Key
Action
Space
Play / Pause
M
Mute / Unmute
F
Toggle Fullscreen
Arrow Up / Right
Increase value of currently focused knob (or speed by default)
Arrow Down / Left
Decrease value of currently focused knob
Tab
Move focus between knobs / controls

Hold any knob’s label area to see tooltips when Tips is enabled.

Technical Notes (Audio / Canvas)
The audio engine uses the Web Audio API with a custom ping synthesiser: multiple partials with independent envelopes, a low‑pass filter that can be modulated by an envelope follower and an LFO.
LFO can modulate filter cutoff, master volume, or stereo pan. Its waveform can be sine, triangle, square, sawtooth, or random (random is implemented as a step‑function using a sine LFO with a noise‑modulated frequency – may be updated in future versions).
The canvas draws the light bar track, edge flashes, and oscilloscope at 60 fps. The wavefollow mode reads the analyser data in real time to shift the light vertically.
All state is encoded in JSON; WebSocket sync uses full_state messages. Phase sync messages are separate to keep visual motion aligned.

Troubleshooting
Problem
Likely Cause & Solution
No sound / audio not playing
Click anywhere on the page – browsers require a user gesture to start AudioContext.
Bluetooth / MIDI / Serial buttons do nothing
You must use HTTPS or localhost. Open the page with a proper web server, not via file://.
WebSocket connection fails
No server running at ws://localhost:8080. Start the relay script, or change the URL to a running server.
Session recording does not save
localStorage is full or disabled. Try clearing some space.
Light moves erratically / jumps
Check speed value – very high speeds (>3 Hz) may cause aliasing on low refresh rates. Keep within therapeutic range (0.5‑2 Hz recommended).
Vibrator / BLE does not respond
The device may need a different characteristic format. You can modify the blePulse() function inside the HTML to match your device’s protocol.


Clinical Disclaimer
This application is a tool to assist bilateral stimulation, not a replacement for professional EMDR therapy. EMDR should be practiced under the guidance of a trained therapist, especially when processing traumatic memories. The developers are not responsible for any misuse or clinical outcomes arising from the use of this software. Always follow established EMDR protocols and ensure that the client is stable and has appropriate resources before starting bilateral stimulation.

License & Credits
Code: MIT – free to use, modify, and distribute.
Fonts: Google Fonts (Nunito, Quicksand, Poppins, Share Tech Mono).
Inspiration: Classic EMDR light bars and bilateral sound generators.
For issues, feature requests, or contributions, please open an issue on the repository (or contact the maintainer).
Enjoy your EMDR sessions – with rhythm, light, and science.

