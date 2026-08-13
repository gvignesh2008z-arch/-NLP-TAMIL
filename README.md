# நம்ம ஊரு AI · Namma Ooru AI

A Tamil-first, voice-first assistant concept for rural and elderly Tamil-speaking
users. Built with React + Vite.

## Run it

```bash
npm install
npm run dev
```

Open the printed local URL in **Chrome** (best Web Speech API support).
Microphone access requires **https** or **localhost** — it will not work
over a plain `http://` LAN address.

## What's implemented

- **Speech-to-text**: `src/hooks/useSpeechRecognition.js` wraps the browser's
  `SpeechRecognition` API set to `ta-IN`, with explicit mic-permission
  requests and Tamil error messages (no support / permission denied /
  no speech / network / no mic).
- **Intent detection**: `src/lib/intentEngine.js` — keyword matching across
  Tamil script and Tanglish, covering the six categories: today's info,
  weather, agriculture, government schemes, public services, general.
- **AI response**: `src/lib/aiClient.js` — **this is the one file to swap
  when you connect a real backend.** Right now it returns honest
  placeholder text (it explicitly says "not connected yet" rather than
  inventing a fake weather number or scheme name). The file has inline
  comments showing the exact `fetch()` shape to drop in, plus suggested
  real data sources per category.
- **Text-to-speech**: `src/hooks/useSpeechSynthesis.js` — reads AI replies
  aloud automatically, with a manual replay button per message, and
  detects whether the device actually has a Tamil voice installed.
- **Conversation UI**: `src/components/ChatPanel.jsx` — "நீங்கள் சொன்னது"
  (You said) / "AI பதில்" (AI response) bubbles, typing-dots loading state.
- **Category shortcuts**: `src/components/CategoryCards.jsx` — tapping a
  card sends a natural sample question for that category, so the flow is
  demoable without speaking every time.
- **Error/permission handling**: `src/components/StatusBanner.jsx`,
  surfaced from the speech hooks.

## Design concept

The visual identity is built around the **kolam** — the rice-flour pattern
drawn each dawn on a swept-earth threshold. The hero section is the dark
"swept ground," the mic button sits inside an animated kolam ring (dots =
the traditional pulli grid; the looping line draws itself while listening),
and the page opens into warm daylight tones below for the everyday content.
Type pairing: **Catamaran** for display/UI chrome, **Baloo Thambi 2** for
body Tamil text — chosen for generous, rounded letterforms that stay legible
at large sizes for elderly readers.

## Wiring up real data (next steps)

Everything routes through `getAIResponse(userText, category)` in
`src/lib/aiClient.js`. To go live:

1. Stand up a backend endpoint (or call provider APIs directly with a
   server-side proxy to keep keys off the client).
2. Replace the body of `getAIResponse` with a real `fetch()` call — the
   calling code in `App.jsx` doesn't need to change.
3. Suggested sources per category are listed as comments in that file
   (weather API, Agmarknet/e-NAM for agriculture, a maintained schemes
   database, local civic directories, a news feed, and a Tamil-capable
   LLM for general queries).
