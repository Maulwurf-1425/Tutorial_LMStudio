# 🧠 Hosting a Local AI with LM Studio

> A complete guide to running your own AI server on your home network – offline, private, and free.

---

## 📋 Table of Contents

1. [What is LM Studio?](#what-is-lm-studio)
2. [Installation](#installation)
3. [Models – Choosing by Device & Use Case](#models--choosing-by-device--use-case)
4. [System Prompts](#system-prompts)
5. [Starting the Local Server](#starting-the-local-server)
6. [Making It Available Across Your Wi-Fi](#making-it-available-across-your-wi-fi)
7. [Example Interface (HTML + JS)](#example-interface-html--js)
8. [Tips & Troubleshooting](#tips--troubleshooting)

---

## What is LM Studio?

[LM Studio](https://lmstudio.ai) is a free desktop app for Windows, macOS, and Linux that lets you run large language models (LLMs) entirely **locally** on your own machine. It offers:

- A graphical interface for downloading and managing models (via Hugging Face)
- A built-in **local API server** that emulates the OpenAI API
- Support for GPU acceleration (NVIDIA CUDA, Apple Metal, AMD ROCm)
- No internet connection needed for inference – complete privacy

---

## Installation

1. Go to [https://lmstudio.ai](https://lmstudio.ai) and download the version for your operating system.
2. Install LM Studio like any regular app.
3. On first launch, it will scan for GPU support – make sure your drivers are up to date.
4. Use the **"Discover"** tab to search for and download models directly from Hugging Face.

> **Model storage location:** By default at `~/.lmstudio/models/` (configurable in settings).

---

## Models – Choosing by Device & Use Case

The right model depends on your **RAM/VRAM**, your **GPU**, and your **use case**. As a rule of thumb: more parameters = smarter, but also more resource-hungry.

### Quantization Overview

Models are available in different quantization levels (e.g. `Q4_K_M`, `Q5_K_S`, `Q8_0`):

| Quantization | Quality | Memory Usage | Recommendation |
|---|---|---|---|
| Q2_K | ⭐⭐ | Very low | Only for very old devices |
| Q4_K_M | ⭐⭐⭐⭐ | Medium | **Best trade-off** |
| Q5_K_M | ⭐⭐⭐⭐½ | Medium-high | Good with 16 GB+ RAM |
| Q8_0 | ⭐⭐⭐⭐⭐ | High | Only for powerful hardware |

---

### 🖥️ Powerful PC / Workstation (32 GB+ RAM, dedicated GPU)

| Model | Size | Strengths | Quantization |
|---|---|---|---|
| **Llama 3.3 70B** | 70B | General purpose, very smart, coding | Q4_K_M |
| **Qwen2.5-72B** | 72B | Multilingual, reasoning, code | Q4_K_M |
| **DeepSeek-R1 70B** | 70B | Logic, math, reasoning | Q4_K_M |
| **Mixtral 8x22B** | ~140B MoE | Fast thanks to MoE architecture | Q3_K_M |

---

### 💻 Average Laptop / PC (16 GB RAM, integrated or weak GPU)

| Model | Size | Strengths | Quantization |
|---|---|---|---|
| **Llama 3.1 8B** | 8B | Versatile, fast, solid | Q4_K_M |
| **Mistral 7B v0.3** | 7B | Instruction following, text | Q4_K_M |
| **Qwen2.5 7B** | 7B | Multilingual, code, chat | Q4_K_M |
| **Gemma 2 9B** | 9B | All-rounder, by Google | Q4_K_M |
| **Phi-3.5 Mini** | 3.8B | Small but surprisingly capable | Q5_K_M |

---

### 📱 Low-End Hardware / Older Machine (8 GB RAM, no GPU)

| Model | Size | Strengths | Quantization |
|---|---|---|---|
| **Phi-3 Mini** | 3.8B | Efficient, surprisingly good | Q4_K_M |
| **Gemma 2 2B** | 2B | Very compact, usable | Q8_0 |
| **Qwen2.5 1.5B** | 1.5B | Ultra-small, for simple tasks | Q8_0 |
| **SmolLM2 1.7B** | 1.7B | Minimal footprint | Q8_0 |

---

### 🎯 Models by Use Case

| Purpose | Recommended Model |
|---|---|
| 💬 General chat & assistant | Llama 3.1 8B / Qwen2.5 7B |
| 💻 Writing & debugging code | DeepSeek-Coder-V2, Qwen2.5-Coder 7B |
| 🧮 Math & logic | DeepSeek-R1 (any size) |
| 📝 Writing text | Qwen2.5 7B / Llama 3.3 70B |
| 🔒 Maximum privacy & offline | Phi-3 Mini / Mistral 7B |
| 🚀 Best quality (powerful hardware) | Llama 3.3 70B / Qwen2.5 72B |

---

## System Prompts

A **system prompt** is a hidden instruction given to the model before the actual conversation begins. It defines the **personality**, **focus**, and **boundaries** of your AI assistant.

### Where to enter it?

In LM Studio: Under the Chat tab, there is a **"System Prompt"** field above the input box. When using the local server, it is sent via the API with each request (see example code below).

### Examples

**General-purpose assistant:**
```
You are a helpful, precise assistant. Be direct and clear in your answers.
If you are unsure about something, say so. Keep responses concise and accurate.
```

**Coding assistant:**
```
You are an experienced software developer focused on clean, well-documented code.
For coding questions, briefly explain your approach, then provide working code,
and point out any potential pitfalls. Prefer modern best practices.
```

**Strictly fact-based assistant:**
```
You are a fact-based assistant. Never make up information.
If you don't know something, clearly state "I don't know."
Keep answers concise and well-grounded.
```

**Tips for writing good system prompts:**
- Define the **role** ("You are a..."), **behavior** ("Always respond..."), and **limits** ("Stick to...")
- Write in the language you want the model to use
- Less is often more – a short, clear prompt often beats a long one
- Test different variations and adjust based on results

---

## Starting the Local Server

The built-in **Local Server** in LM Studio emulates the OpenAI API – meaning any software compatible with OpenAI also works with your local model.

### Step by Step

1. **Load a model:** In the **"Chat"** or **"Local Server"** tab, select a downloaded model and load it (green button).

2. **Open the Server tab:** Click the **`<>` icon** on the left sidebar (Local Server).

3. **Configure settings:**
   - **Port:** Default is `1234` (can be changed)
   - **CORS:** Enable for browser access
   - **"Serve on Local Network":** ✅ **Enable** so other devices on your Wi-Fi can connect

4. **Start the server:** Click **"Start Server"**.

The server is now running and accessible at:
```
http://<your-local-IP>:1234
```

### Finding Your Local IP

**Windows:**
```cmd
ipconfig
# Look for "IPv4 Address" under your Wi-Fi adapter
```

**macOS / Linux:**
```bash
ip addr show   # Linux
ifconfig       # macOS
# Look for "inet" on your Wi-Fi interface (wlan0, en0, etc.)
```

Typical local IP addresses: `192.168.1.x` or `192.168.0.x`

### Available API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/v1/models` | GET | List loaded models |
| `/v1/chat/completions` | POST | Chat requests (OpenAI-compatible) |
| `/v1/completions` | POST | Text completion (legacy) |
| `/v1/embeddings` | POST | Generate embeddings |

---

## Making It Available Across Your Wi-Fi

Once **"Serve on Local Network"** is enabled, all devices on the same Wi-Fi can access the server – smartphones, tablets, other PCs.

### Accessing from Other Devices

Replace `192.168.1.100` with your host machine's actual IP:

```
http://192.168.1.100:1234/v1/chat/completions
```

### Static IP Address Recommended

To prevent the IP from changing after every router restart, assign your host PC a **static local IP** in your router settings (DHCP reservation by MAC address – usually found under "DHCP" or "LAN" in your router's interface).

### Opening the Firewall (if needed)

**Windows:**
```powershell
# In PowerShell as Administrator:
New-NetFirewallRule -DisplayName "LM Studio Server" -Direction Inbound -Protocol TCP -LocalPort 1234 -Action Allow
```

**Linux (ufw):**
```bash
sudo ufw allow 1234/tcp
```

**macOS:** On first launch, macOS will automatically ask for permission.

> ⚠️ **Security note:** The server has no authentication. Only use it on your home network. **Never** expose this port to the public internet (no port forwarding on your router).

---

## Example Interface (HTML + JS)

This ready-to-use interface works in any browser and communicates directly with your local LM Studio server. Save it as `index.html` and open it from any device on your Wi-Fi.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Local AI Chat</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', system-ui, sans-serif;
      background: #0f0f13;
      color: #e8e8f0;
      height: 100dvh;
      display: flex;
      flex-direction: column;
    }

    header {
      padding: 1rem 1.5rem;
      background: #1a1a24;
      border-bottom: 1px solid #2a2a3a;
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }

    header h1 { font-size: 1.1rem; font-weight: 600; }
    .status-dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: #22c55e;
      box-shadow: 0 0 6px #22c55e;
      flex-shrink: 0;
    }
    .status-dot.offline { background: #ef4444; box-shadow: 0 0 6px #ef4444; }

    .config-bar {
      padding: 0.75rem 1.5rem;
      background: #15151f;
      border-bottom: 1px solid #2a2a3a;
      display: flex;
      gap: 0.75rem;
      flex-wrap: wrap;
      align-items: center;
    }

    .config-bar label { font-size: 0.8rem; color: #888; }

    .config-bar input, .config-bar textarea {
      background: #1a1a24;
      border: 1px solid #2a2a3a;
      border-radius: 6px;
      color: #e8e8f0;
      padding: 0.4rem 0.7rem;
      font-size: 0.85rem;
      outline: none;
    }
    .config-bar input:focus, .config-bar textarea:focus { border-color: #6366f1; }
    #serverUrl { width: 220px; }
    #systemPrompt { width: 100%; resize: vertical; min-height: 48px; }

    #chat {
      flex: 1;
      overflow-y: auto;
      padding: 1.5rem;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .message {
      max-width: 80%;
      padding: 0.75rem 1rem;
      border-radius: 12px;
      line-height: 1.6;
      font-size: 0.95rem;
      white-space: pre-wrap;
      word-break: break-word;
    }

    .message.user {
      background: #6366f1;
      align-self: flex-end;
      border-bottom-right-radius: 3px;
    }

    .message.assistant {
      background: #1e1e2e;
      align-self: flex-start;
      border: 1px solid #2a2a3a;
      border-bottom-left-radius: 3px;
    }

    .message.thinking {
      background: #1a1a24;
      border: 1px dashed #3a3a4a;
      color: #666;
      font-style: italic;
    }

    .input-area {
      padding: 1rem 1.5rem;
      background: #1a1a24;
      border-top: 1px solid #2a2a3a;
      display: flex;
      gap: 0.75rem;
    }

    #userInput {
      flex: 1;
      background: #0f0f13;
      border: 1px solid #2a2a3a;
      border-radius: 8px;
      color: #e8e8f0;
      padding: 0.65rem 1rem;
      font-size: 0.95rem;
      resize: none;
      outline: none;
      min-height: 44px;
      max-height: 150px;
    }
    #userInput:focus { border-color: #6366f1; }

    button {
      background: #6366f1;
      color: white;
      border: none;
      border-radius: 8px;
      padding: 0.65rem 1.25rem;
      font-size: 0.9rem;
      cursor: pointer;
      transition: background 0.2s;
      align-self: flex-end;
    }
    button:hover { background: #4f52d1; }
    button:disabled { background: #333; cursor: not-allowed; }

    #clearBtn {
      background: transparent;
      border: 1px solid #2a2a3a;
      color: #888;
      padding: 0.4rem 0.75rem;
      font-size: 0.8rem;
    }
    #clearBtn:hover { background: #1e1e2e; color: #ccc; }
  </style>
</head>
<body>

<header>
  <div class="status-dot" id="statusDot"></div>
  <h1>🧠 Local AI Chat</h1>
  <button id="clearBtn" style="margin-left:auto">Clear history</button>
</header>

<div class="config-bar">
  <div style="display:flex;gap:0.5rem;align-items:center">
    <label for="serverUrl">Server URL:</label>
    <input id="serverUrl" type="text" value="http://localhost:1234" placeholder="http://192.168.1.100:1234" />
  </div>
  <div style="flex:1;min-width:200px">
    <label for="systemPrompt">System Prompt:</label>
    <textarea id="systemPrompt" placeholder="You are a helpful assistant.">You are a helpful, precise assistant. Be direct and clear in your answers.</textarea>
  </div>
</div>

<div id="chat"></div>

<div class="input-area">
  <textarea id="userInput" placeholder="Type a message... (Enter = send, Shift+Enter = new line)" rows="1"></textarea>
  <button id="sendBtn">Send</button>
</div>

<script>
  const chat = document.getElementById('chat');
  const userInput = document.getElementById('userInput');
  const sendBtn = document.getElementById('sendBtn');
  const serverUrl = document.getElementById('serverUrl');
  const systemPromptEl = document.getElementById('systemPrompt');
  const statusDot = document.getElementById('statusDot');
  const clearBtn = document.getElementById('clearBtn');

  let history = [];

  async function checkConnection() {
    try {
      const res = await fetch(`${serverUrl.value}/v1/models`, { signal: AbortSignal.timeout(2000) });
      statusDot.className = res.ok ? 'status-dot' : 'status-dot offline';
    } catch {
      statusDot.className = 'status-dot offline';
    }
  }
  checkConnection();
  setInterval(checkConnection, 5000);
  serverUrl.addEventListener('change', checkConnection);

  function addMessage(role, content) {
    const div = document.createElement('div');
    div.className = `message ${role}`;
    div.textContent = content;
    chat.appendChild(div);
    chat.scrollTop = chat.scrollHeight;
    return div;
  }

  async function sendMessage() {
    const text = userInput.value.trim();
    if (!text) return;

    userInput.value = '';
    userInput.style.height = 'auto';
    sendBtn.disabled = true;

    addMessage('user', text);
    history.push({ role: 'user', content: text });

    const thinkingEl = addMessage('thinking', '⏳ Thinking...');

    const messages = [];
    const sp = systemPromptEl.value.trim();
    if (sp) messages.push({ role: 'system', content: sp });
    messages.push(...history);

    try {
      const res = await fetch(`${serverUrl.value}/v1/chat/completions`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          model: 'local-model', // LM Studio ignores this value
          messages,
          temperature: 0.7,
          max_tokens: 2048,
          stream: false
        })
      });

      if (!res.ok) throw new Error(`HTTP ${res.status}`);

      const data = await res.json();
      const reply = data.choices?.[0]?.message?.content ?? '(No response)';

      thinkingEl.remove();
      addMessage('assistant', reply);
      history.push({ role: 'assistant', content: reply });

    } catch (err) {
      thinkingEl.remove();
      addMessage('assistant', `❌ Error: ${err.message}\n\nMake sure LM Studio is running and the server is started.`);
    }

    sendBtn.disabled = false;
    userInput.focus();
  }

  sendBtn.addEventListener('click', sendMessage);

  userInput.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  });

  userInput.addEventListener('input', () => {
    userInput.style.height = 'auto';
    userInput.style.height = Math.min(userInput.scrollHeight, 150) + 'px';
  });

  clearBtn.addEventListener('click', () => {
    history = [];
    chat.innerHTML = '';
  });
</script>
</body>
</html>
```

### Using It Across Your Wi-Fi

1. Save the file as `index.html` on the host PC (or serve it via a local web server)
2. Change the **Server URL** in the interface to `http://192.168.1.100:1234` (your actual IP)
3. Open the file on another device on your Wi-Fi (e.g. a phone via browser)

> Alternatively, start a simple local web server to serve the file across the network:
> ```bash
> # Python (available everywhere):
> python3 -m http.server 8080
> # Then accessible at: http://192.168.1.100:8080
> ```

---

## Tips & Troubleshooting

### Improving Performance

- **GPU acceleration:** In LM Studio under *Settings → Hardware*, enable GPU offloading. The more layers on the GPU, the faster the responses.
- **Context length:** Lower values (e.g. 2048 instead of 8192) = less RAM usage and faster responses.
- **Threads:** Under *Settings → Inference*, set the thread count to match your CPU core count.

### Common Issues

| Problem | Solution |
|---|---|
| `Connection refused` | LM Studio server is not started, or wrong IP/port |
| Model won't load | Not enough RAM – choose a smaller model or higher quantization |
| Other devices can't connect | Enable "Serve on Local Network" in LM Studio + check firewall |
| Very slow responses | Enable GPU offloading or use a smaller model |
| CORS error in browser | Enable CORS in LM Studio's server settings |

### Useful Links

- 📥 [LM Studio Download](https://lmstudio.ai)
- 🤗 [Hugging Face Models](https://huggingface.co/models?library=gguf)
- 📖 [LM Studio Documentation](https://lmstudio.ai/docs)
- 🔍 [Browse GGUF Models](https://huggingface.co/models?search=gguf)

---

*Made with ❤️ for everyone who prefers to host their own AI.*
