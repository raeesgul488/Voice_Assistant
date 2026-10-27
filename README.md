<div align="center">

# 🎙️ Voice_Assistant

**A real-time, conversational voice agent built on [LiveKit Agents](https://docs.livekit.io/agents/).**
Speak naturally, get a spoken answer back, with low latency and clean audio.

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![LiveKit](https://img.shields.io/badge/LiveKit-Agents-0A0A0A)
![STT](https://img.shields.io/badge/STT-Deepgram%20Nova--3-13EF93)
![Package Manager](https://img.shields.io/badge/uv-supported-DE5FE9)

</div>

---

##  Overview

This project is an end-to-end voice assistant that joins a LiveKit room, listens to the user, understands what they said, and replies out loud. Audio travels over WebRTC for real-time streaming, while speech recognition, language generation, and voice synthesis are handled through LiveKit Inference.

The assistant is tuned for speech: short, friendly answers with no emojis, asterisks, or markdown that would sound awkward when read aloud.

## 🚀 Features

| Capability | Technology |
|---|---|
| 🎧 **Speech-to-Text** | Deepgram Nova-3 with multilingual support (`language="multi"`) |
| 🧠 **LLM** | Google Gemma (`google/gemma-4-31b-it`) |
| 🗣️ **Text-to-Speech** | Inworld TTS (`inworld/inworld-tts-2`, voice: *Ashley*) |
| 🔄 **Turn Detection** | LiveKit turn detector, so the agent knows when to listen and when to speak |
| 🔇 **Noise Cancellation** | ai-coustics audio enhancement (`QUAIL_VF_S`) |
| 👋 **Proactive Greeting** | The agent greets the user as soon as the session starts |

## 🧩 How It Works

```
 User's mic ──► WebRTC / LiveKit Room
                      │
                      ▼
          Noise cancellation (ai-coustics)
                      │
                      ▼
          Speech-to-Text (Deepgram Nova-3)
                      │
                      ▼
     LLM response (Gemma) + turn detection
                      │
                      ▼
          Text-to-Speech (Inworld TTS)
                      │
                      ▼
 User's speaker ◄── WebRTC / LiveKit Room
```

1. The agent registers with LiveKit under the name `my-agent` and joins a room when a session is dispatched.
2. Incoming audio is cleaned with noise cancellation, then transcribed.
3. The transcript goes to the LLM, guided by the assistant's instructions.
4. The reply is synthesized to speech and streamed back into the room.

## 📋 Prerequisites

- **Python** (see [`.python-version`](.python-version) for the version used in this project)
- A **[LiveKit Cloud](https://cloud.livekit.io/)** project, or a self-hosted LiveKit server
- LiveKit API credentials (URL, API key, API secret)
- [`uv`](https://docs.astral.sh/uv/) (recommended), or `pip`

## 🛠️ Installation

```bash
# 1. Clone the repository
git clone https://github.com/raeesgul488/Voice_Assistant.git
cd Voice_Assistant

# 2. Install dependencies (recommended: uv, which uses pyproject.toml and uv.lock)
uv sync
```

<details>
<summary>Prefer pip?</summary>

```bash
python -m venv .venv
source .venv/bin/activate        # On Windows: .venv\Scripts\activate
pip install -e .
```

</details>

## 🔐 Configuration

Copy the example environment file and fill in your credentials:

```bash
cp .env.example .env.local
```

```env
LIVEKIT_URL=wss://your-project.livekit.cloud
LIVEKIT_API_KEY=your-api-key
LIVEKIT_API_SECRET=your-api-secret
```

> ⚠️ **Never commit real secrets.** `.env.local` is loaded by the agent at startup and must stay out of version control. If a key has ever been pushed to a public repo, rotate it immediately in your LiveKit dashboard.

## ▶️ Running the Agent

**Talk to it directly in your terminal** (quickest way to test):

```bash
uv run Agent.py console
```

**Run in development mode** (connects to LiveKit and auto-reloads on changes):

```bash
uv run Agent.py dev
```

**Run in production mode:**

```bash
uv run Agent.py start
```

Once the agent is running, connect to a room using the [LiveKit Agents Playground](https://agents-playground.livekit.io/) or your own frontend, and start talking.

## 📁 Project Structure

```
.
├── Agent.py          # Agent definition, session setup, and entrypoint
├── src/voice_agent/  # Package source
├── pyproject.toml    # Project metadata and dependencies
├── uv.lock           # Locked dependency versions
├── .env.example      # Template for environment variables
├── .python-version   # Python version pin
└── README.md
```

## 🎛️ Customization

All of the main settings live in `Agent.py`:

- **Personality:** edit the `instructions` string in the `Assistant` class.
- **Language / STT model:** change the `inference.STT(...)` arguments, for example set a fixed `language` instead of `"multi"`.
- **LLM:** swap the model identifier in `inference.LLM(...)`.
- **Voice:** change the `voice` (and `model`) in `inference.TTS(...)`.
- **Noise cancellation:** replace `ai_coustics.EnhancerModel.QUAIL_VF_S` with another supported model.
- **Greeting:** adjust the instructions passed to `session.generate_reply(...)`.
- **Agent name:** change `agent_name="my-agent"` in `@server.rtc_session(...)`, and use the same name when dispatching the agent.

## 🧰 Tech Stack

- [LiveKit Agents](https://docs.livekit.io/agents/) and LiveKit Inference
- [Deepgram Nova-3](https://deepgram.com/) for speech recognition
- [Google Gemma](https://ai.google.dev/gemma) for language generation
- [Inworld TTS](https://inworld.ai/) for voice synthesis
- [ai-coustics](https://ai-coustics.com/) for noise cancellation
- [python-dotenv](https://pypi.org/project/python-dotenv/) for configuration

## 🗺️ Ideas for Next Steps

- [ ] Add function tools (weather, calendar, web search)
- [ ] Add a web or mobile frontend
- [ ] Support conversation memory across sessions
- [ ] Add Docker deployment

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the [MIT License](LICENSE). Add a `LICENSE` file to the repo, or swap in the license of your choice.

---

<div align="center">

Built with ❤️ by [raeesgul488](https://github.com/raeesgul488)

</div>