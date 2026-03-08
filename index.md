---
layout: default
---

[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)
![GitHub License](https://img.shields.io/github/license/surveychat/surveychat)

`surveychat` is an open-source web application that enables researchers to administer surveys and conduct randomized experiments involving large language model (LLM)-based conversational agents, without the need to develop custom web application code. The system supports two primary operational modes: (i) **survey mode**, in which all participants interact with an identical chatbot configuration, and (ii) **experiment mode**, in which participants are randomly assigned to one of multiple chatbot conditions, each defined by a researcher-specified persona and language model. Upon completion of the interaction, participants receive an anonymized JSON transcript that contains only the role, content, and timestamp of each message. This transcript can be copied back into the parent survey platform (such as Qualtrics), within which the chatbot interface itself can also be directly embedded. The frontend of `surveychat` is implemented using Streamlit, and the entire application is configured via a single Python file. The system does not persist conversation data on its server and is compatible with any chat-completions-compatible API endpoint - including locally hosted models - thereby allowing researchers to retain full control over model selection, API usage, data jurisdiction, and adherence to ethical and regulatory requirements.

**[Try the demo](https://surveychat.invisible.info)** [passcodes: `ALPHA` (neutral bot) or `BETA` (empathetic bot)]

**[View code on GitHub](https://github.com/surveychat/surveychat)**

<table>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/surveychat/surveychat/main/paper/surveychat-interface-2.png" alt="Chat interface" style="max-width:100%;" />
      <br/><em>Participants chat with the AI model</em>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/surveychat/surveychat/main/paper/surveychat-interface-3.png" alt="Transcript export" style="max-width:100%;" />
      <br/><em>Participants paste their conversation transcript into your survey (e.g., Qualtrics)</em>
    </td>
  </tr>
</table>

<br/>

**Contents:** 
- [Before you start](#before-you-start)
- [Run locally](#run-locally)
- [Deploy online](#deploy-online-streamlit-community-cloud)
- [Help and documentation](#help-and-documentation)

<br/>

## Before you start

You will need:

- **Python 3.10 or newer** - check with `python3 --version` in your terminal. Download from [python.org](https://www.python.org/downloads/) if needed.
- **An API key** - from a provider such as OpenAI, Azure, Hugging Face, or OpenRouter.
- **A terminal** - Terminal on macOS/Linux, or Command Prompt / PowerShell on Windows.


<br/>

## Run locally

Good for testing on your own computer before sharing with participants.

**Step 1: Fork and clone the repo**

Click **Fork** on the [GitHub page](https://github.com/surveychat/surveychat) to create your own copy, then clone it:

```bash
git clone https://github.com/YOUR_USERNAME/surveychat.git
cd surveychat
pip install -r requirements.txt
cp .env.example .env
```

> Don't have git? Download it from [git-scm.com](https://git-scm.com/downloads). On macOS it may already be installed - check with `git --version`.

**Step 2: Add your API key**

Open the `.env` file in any text editor and replace `your-key-here` with your actual API key:

```
OPENAI_API_KEY=sk-...
```

**Step 3: Configure the chatbot**

Open `app.py` in a text editor. Find the block that starts with:

```
# ╔══════════ RESEARCHER CONFIGURATION ═══════════╗
```

Set `N_CONDITIONS = 1` for a single chatbot (survey mode), or a higher number for an experiment with multiple chatbot versions. Then edit the `CONDITIONS` list to write your chatbot's instructions:

```python
CONDITIONS = [
    {
        "name":          "Interview bot",
        "system_prompt": "You are a friendly research interviewer. Ask one open-ended question at a time about the participant's social media habits. After 5-6 exchanges, thank them and let them know they can click End this chat.",
        "model":         "gpt-4o",
    },
]
```

**Step 4: Run the app**

```bash
streamlit run app.py
```

Your browser will open automatically at [http://localhost:8501](http://localhost:8501). This URL only works on your own computer.

<br/>

## Deploy online (Streamlit Community Cloud)

This gives you a permanent public URL you can share with participants - free for public repos, no server to manage.

1. Push your modified repo to GitHub (your `.env` file is automatically excluded from the upload).
2. Go to [share.streamlit.io](https://share.streamlit.io) and sign in with your GitHub account.
3. Click **Create app**, then select your repo and `app.py` as the main file.
4. Under **Advanced settings → Secrets**, add your API key exactly as shown:
   ```
   OPENAI_API_KEY = "sk-..."
   ```
5. Click **Deploy**. You will get a public URL to embed inside Qualtrics, or share directly with participants.

<br/>

## Deploy online (other platforms)

You can also deploy on platforms like Azure, AWS, or DigitalOcean. The process is different for each platform and may require additional configuration (e.g. for environment variables, dependencies, and web server setup). Refer to the platform's documentation for deploying Streamlit web apps.

<br/>

## Help and documentation

See the [full documentation](https://github.com/surveychat/surveychat#readme) on GitHub for configuration options and troubleshooting.