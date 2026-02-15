# CLAUDE.md — VideoForge

## Objectif

Atelier de création de vidéos courtes (YouTube Shorts, Instagram Reels) en français.
Le workflow est **collaboratif** : l'utilisateur brainstorm avec Claude sur chaque vidéo,
puis Claude utilise les outils installés pour la produire.

Pas de pipeline automatisé. Chaque vidéo est un projet unique discuté au cas par cas.

---

## Serveur & Projet

- **Serveur :** 46.224.49.147 (Hetzner VPS, CPU, 16GB RAM, Ubuntu)
- **User :** deploy
- **Répertoire :** /var/www/videoforge/
- **Repo :** git@github.com:OG-Elson-Private/VideoForge.git (branche: main)

Autres projets sur le même serveur :
- TradeHub : port 80
- CV Landing Page : port 3000
- Flyer Gigs : port 3001

---

## Outils à installer

### Système
```bash
sudo apt update && sudo apt install -y ffmpeg python3 python3-pip nodejs npm
```

### TTS (Text-to-Speech)

**Chatterbox (prioritaire)** — qualité supérieure, 23 langues, français natif :
```bash
pip install gradio_client --break-system-packages
```
Accès via HF Spaces (gratuit, pas de GPU local requis) :
```python
from gradio_client import Client
client = Client("ResembleAI/Chatterbox-Multilingual-TTS")
result = client.predict(text="...", language="fr", exaggeration=0.5, cfg_weight=0.5, api_name="/generate")
```

**Edge TTS (fallback)** — fiable, gratuit, génère SRT nativement :
```bash
pip install edge-tts --break-system-packages
```
Voix FR : `fr-FR-HenriNeural` (homme), `fr-FR-DeniseNeural` (femme),
`fr-BE-CharlineNeural`, `fr-BE-GerardNeural`, `fr-CA-AntoineNeural`, `fr-CA-SylvieNeural`

### Transcription (sous-titres SRT)
```bash
pip install openai-whisper --break-system-packages
```
Utilisé quand le TTS ne génère pas de SRT (Chatterbox). Edge TTS le fait nativement.

### Vidéo (composition & rendu)
```bash
cd /var/www/videoforge
npm init @revideo
npm install
```

---

## Format de sortie

- **Résolution :** 1080x1920 (vertical 9:16)
- **FPS :** 30
- **Codec :** H.264 / AAC
- **Format :** MP4

---

## Git

```bash
git add . && git commit -m "type: description" && git push origin main
```
- Commits en anglais, format `type: description`
- Pas de lignes d'attribution
- .gitignore : `node_modules/`, `tmp/`, `output/`, `*.mp3`, `*.mp4`, `*.wav`, `*.srt`, `__pycache__/`

---

## Communication

- **Avec l'utilisateur :** Français
- **Code & commits :** Anglais
- **Contenu vidéos :** Français
