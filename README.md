# 🍌 Nano Banana Studio

A **creative playground** combining **FastAPI + Streamlit** to experiment with **Google’s Gemini image generation & editing (“nano banana”) capabilities**. Build, edit, merge, restore, or virtually try-on images—all from a simple UI.

---

## Features

* **Text-to-Image Generation** – Create images from prompts.
* **Prompt-Based Image Editing** – Transform existing images.
* **Virtual Try-On** – Composite products onto people.
* **Ad Variations Generator** – Create multiple image variations for ads.
* **Image Merging** – Combine up to 5 images with prompt guidance.
* **Scene Extensions / Reinterpretation** – Generate 3 variations of a scene.
* **Old Photo Restoration** – Restore old images.
* **Randomized System Prompts** – Adds variety automatically.

---

## Tech Stack

* Python 3.11+
* **FastAPI** (backend)
* **Streamlit** (frontend)
* **uv** – Dependency & runtime management
* **Uvicorn** ASGI server

---

## Prerequisites

* Python 3.11 installed
* A valid Gemini API key (set in the UI each session)

---

## Installing `uv`

### **Windows**

1. Open **PowerShell** as Administrator.
2. Run:

```powershell
iwr https://astral.sh/uv/install.ps1 -useb | iex
```

3. Confirm installation:

```powershell
uv --version
```

### **macOS**

```bash
brew install uv
```

### **Linux / WSL**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

## Quick Start

```bash
# Clone project
git clone https://github.com/1DS22AI022Minakshi/BananaVision-AI.git
cd BananaVision-AI

# Sync dependencies
uv sync

# Run backend
uv run backend.py
# Backend API at http://localhost:8000

# In a second terminal, run frontend
uv run streamlit run frontend.py
# Open URL (default http://localhost:8501) and enter Gemini API key
```

---

## Environment Variables (Optional)

| Variable               | Purpose                                     | Default |
| ---------------------- | ------------------------------------------- | ------- |
| `MAX_AD_VARIATIONS`    | Upper bound for ad images                   | 3       |
| `MAX_SCENE_VARIATIONS` | Maximum scene variations (currently forced) | 3       |

```bash
export MAX_AD_VARIATIONS=3
export MAX_SCENE_VARIATIONS=3
```

---

## API Overview

All endpoints expect a form field `api_key` with your Gemini key.

1. **Generate Image**
   `POST /generate` – Fields: `api_key`, `prompt` → Returns base64 image

2. **Edit Image**
   `POST /edit` – Fields: `api_key`, `prompt` + image file

3. **Virtual Try-On**
   `POST /virtual_try_on` – Files: product + person, optional prompt

4. **Create Ads**
   `POST /create_ads` – Files: model + product, optional prompt → Returns JSON of up to 3 image variations

5. **Merge Images**
   `POST /merge_images` – Multiple files (up to 5), optional prompt

6. **Generate Scenes**
   `POST /generate_scenes` – Scene file + optional prompt → Up to 3 variations

7. **Restore Old Image**
   `POST /restore_old_image` – File + optional prompt

---

## Frontend Usage

1. Start backend
2. Start Streamlit app
3. Enter backend URL (default `http://localhost:8000`)
4. Paste Gemini API key
5. Pick mode and interact

---

## Project Structure

```
backend.py      # FastAPI backend endpoints
frontend.py     # Streamlit UI
prompts.py      # Prompt templates
pyproject.toml  # Dependencies & metadata
movie/          # Sample media assets
```

---

## Development Notes

* Prompt banks are in `prompts.py` keyed by mode.
* Scene & ad variants are clamped to prevent overuse.
* Responses return **base64 images directly** (no temp files).

---

## Troubleshooting

| Issue                         | Fix                                             |
| ----------------------------- | ----------------------------------------------- |
| 401/403 or quota errors       | Check API key & usage limits                    |
| Empty results array           | Retry with a different prompt                   |
| Streamlit can't reach backend | Confirm backend running on port 8000            |
| Slow responses                | Model rate limiting – automatic backoff applied |

---

## Future Ideas

* Local caching for identical prompts
* Download button for generated images
* Async endpoints for parallel generation
