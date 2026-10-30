# Ofrenda

Write a remembrance for someone you miss, with a model that runs on your own computer.

You give it a name, who they were to you, the foods and songs they loved, and one memory. A local Gemma model, served by Ollama, writes a short piece in your voice using only those details. A small altar lights a candle while it writes. Nothing is uploaded: the page talks to `localhost` and nowhere else, so it works with the internet off.

Built for the [Hacktoberfest Open-Source AI Challenge Week 4: Ghost in the Machine](https://dev.to/challenges/hacktoberfest-week4-2026-10-26).

## Run it

You need [Ollama](https://ollama.com) and Python (only to serve the page locally).

1. Pull a model:
   ```
   ollama pull gemma3
   ```
   On a machine with 8 GB of RAM or less, use `gemma3:1b`.

2. Start Ollama and allow the page to call it.

   Windows (PowerShell):
   ```
   $env:OLLAMA_ORIGINS="*"; ollama serve
   ```
   Mac / Linux:
   ```
   OLLAMA_ORIGINS="*" ollama serve
   ```
   If the Ollama tray app is already running, quit it first, or you'll get a "port already in use" error.

3. In a second terminal, from this folder:
   ```
   python -m http.server 8000 --bind 127.0.0.1
   ```
   Use `python3` on Mac / Linux.

4. Open <http://localhost:8000>. The status line should say "Connected" and show your model.

## How it works

- One file, `index.html`. No build step, no dependencies, no backend.
- The page calls Ollama's `/api/tags` to list your models (it prefers a Gemma) and `/api/chat` with streaming, so text appears as it is written.
- The system prompt tells the model to use only the facts you typed and not to add scenery, routines or feelings you didn't state. Temperature is set low (0.3) to keep it close to your words.

## Limits

- Small local models can still embellish. Early versions invented a photograph and a creaking door that nobody mentioned. The prompt now forbids this and it holds up much better, but read the result and edit anything that isn't true.
- The output is only as rich as what you give it. One true sentence of detail helps more than any setting.
- On CPU only, generation is slow (a minute or so for a short piece).

## Privacy

Your text is sent only to the Ollama server on your own machine. The page makes no other network requests. The last remembrance is saved in your browser's local storage and the Download button saves a `.txt` file; nothing else is stored.

## Built with

Gemma (open-weight), Ollama, and plain HTML, CSS and JavaScript. I used an AI assistant to help draft the code and the post, and tested and edited it myself.
