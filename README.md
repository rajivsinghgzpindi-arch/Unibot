


OPENAI_API_KEY=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
[index2.html](https://github.com/user-attachments/files/22688811/index2.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Campus GPT 🤖</title>

<style>
/* ===== Body & Container ===== */
body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #0d0d0d, #1a1a1a);
  overflow: hidden;
  color: #fff;
}

.container {
  width: 450px;
  background: #111;
  padding: 25px;
  border-radius: 20px;
  box-shadow: 0 15px 40px rgba(0,0,0,0.8);
  position: relative;
  z-index: 2;
}

/* ===== Floating Neon Particles ===== */
.floating-cartoon {
  position: absolute;
  pointer-events: none;
  font-size: 18px;
  opacity: 0.15;
  animation: float 6s linear infinite;
  color: rgb(151, 252, 0);
}
@keyframes float {
  0% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-25px) rotate(20deg); }
  100% { transform: translateY(0) rotate(0deg); }
}

/* ===== Chatbox ===== */
#chatbox {
  height: 360px;
  padding: 12px;
  overflow-y: auto;
  margin-bottom: 12px;
  background-color: #1a1a2b;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  position: relative;
}
#chatbox::-webkit-scrollbar { width: 8px; }
#chatbox::-webkit-scrollbar-thumb { background: rgba(0,255,254,0.5); border-radius: 10px; }

/* ===== Messages ===== */
.message {
  margin: 8px 0;
  padding: 12px 16px;
  border-radius: 20px;
  max-width: 75%;
  line-height: 1.5;
  word-wrap: break-word;
  display: flex;
  align-items: center;
  gap: 8px;
}
.user { background: linear-gradient(135deg, rgb(200, 255, 0), rgb(200, 255, 0)); color: #111; align-self: flex-end; }
.bot { background: linear-gradient(135deg, #3333ff, #6600ff); color: #fff; align-self: flex-start; }

/* ===== Input & Button ===== */
.search-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}
.search-wrapper::before {
  content: '';
  position: absolute;
  top: -5px; left: -5px; right: -5px; bottom: -5px;
  border-radius: 15px;
  border: 2px solid #d9dbe4;
  box-shadow: 0 0 10px #b261b5, 0 0 20px #b261b5, 0 0 40px #b261b5;
  pointer-events: none;
  z-index: 0;
}
.search-wrapper input, .search-wrapper button { position: relative; z-index: 1; }

#userInput {
  width: 70%;
  padding: 16px;
  border-radius: 10px;
  border: 1px solid #444;
  outline: none;
  background: transparent;
  color: #fff;
  font-size: 16px;
}
#sendBtn {
  width: 25%;
  padding: 16px;
  border: none;
  background: linear-gradient(45deg, rgb(200, 255, 0), #9ce706);
  color: black;
  font-weight: bold;
  border-radius: 10px;
  cursor: pointer;
  margin-left: 5%;
}
#sendBtn:hover { transform: scale(1.05); box-shadow: 0 4px 12px rgba(161, 167, 167, 0.5); }
</style>
</head>
<body>

<!-- Floating Background Images -->
<img src="image.jpg" class="bg-stable" style="top:5%; left:5%" width="500">



<div class="container">
  <div id="chatbox">
    <!-- Optional initial bot image -->
    <img src="image.jpg" alt="UniBot" style="width:60px; display:block; margin:10px auto;">
  </div>

  <div class="search-wrapper">
    <input type="text" id="userInput" placeholder="Type your message..."/>
    <button id="sendBtn">Send</button>
  </div>
</div>

<script>
// ===== Floating Neon Particles =====
const container = document.querySelector('.container');
const emojis = ['🤖','✨','💡','🧠','🌟','🐱','🦄'];
for(let i=0; i<30; i++){
  const span = document.createElement('span');
  span.classList.add('floating-cartoon');
  span.textContent = emojis[Math.floor(Math.random()*emojis.length)];
  span.style.top = Math.random() * (container.clientHeight - 20) + 'px';
  span.style.left = Math.random() * (container.clientWidth - 20) + 'px';
  span.style.fontSize = 12 + Math.random()*22 + 'px';
  span.style.animationDuration = 4 + Math.random()*5 + 's';
  container.appendChild(span);
}

// ===== Chat Functionality =====
const chatbox = document.getElementById('chatbox');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');

function addMessage(sender, message, type){
  const div = document.createElement('div');
  div.classList.add('message', type || 'bot');
  div.innerHTML = `<strong>${sender}:</strong> ${message}`;
  chatbox.appendChild(div);
  chatbox.scrollTop = chatbox.scrollHeight;
}

async function sendMessage() {
  const message = userInput.value.trim();
  if(!message) return;

  addMessage('You', message, 'user');
  userInput.value = '';

  const typingMsg = document.createElement('p');
  typingMsg.innerHTML = `<strong>Campus GPT:</strong> Typing...`;
  chatbox.appendChild(typingMsg);
  chatbox.scrollTop = chatbox.scrollHeight;

  try {
    const res = await fetch('/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message })
    });
    const data = await res.json();
    typingMsg.innerHTML = `<strong>Campus GPT:</strong> ${data.reply}`;
  } catch(err) {
    console.error(err);
    typingMsg.innerHTML = `<strong>Campus GPT:</strong> (Demo) I received your message but could not contact AI.`;
  }
}

// Event listeners
sendBtn.addEventListener('click', sendMessage);
userInput.addEventListener('keypress', e => { if(e.key === 'Enter') sendMessage(); });
</script>

</body>
</html>




[server.js](https://github.com/user-attachments/files/22688815/server.js)
// server.js
import express from "express";
import cors from "cors";
import path from "path";
import dotenv from "dotenv";
import { fileURLToPath } from "url";

dotenv.config(); // load .env

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();
app.use(cors());
app.use(express.json());

// Serve static files (images, css, js)
app.use(express.static(__dirname));

console.log("Loaded OpenAI key:", process.env.OPENAI_API_KEY ? "YES" : "NO");

// Serve index2.html at root
app.get("/", (req, res) => {
  const filePath = path.join(__dirname, "index2.html");
  res.sendFile(filePath, (err) => {
    if (err) {
      console.error("Error sending index2.html:", err);
      res.status(500).send("❌ Could not load the page.");
    }
  });
});

// Chat endpoint
app.post("/chat", async (req, res) => {
  const { message } = req.body;
  if (!message || !message.trim()) {
    return res.status(400).json({ reply: "Please type a message." });
  }

  const apiKey = process.env.OPENAI_API_KEY;

  try {
    if (apiKey) {
      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${apiKey}`,
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [{ role: "user", content: message }],
          max_tokens: 500,
        }),
      });

      const data = await response.json();
      console.log("OpenAI response:", JSON.stringify(data, null, 2));

      if (data?.choices?.[0]?.message?.content) {
        return res.json({ reply: data.choices[0].message.content.trim() });
      }

      console.warn("OpenAI error, falling back to demo:", data?.error);
    } else {
      console.warn("No API key found, using demo mode.");
    }
  } catch (err) {
    console.error("Error contacting OpenAI:", err);
  }

  // Always safe fallback
  return res.json({ reply: demoReply(message) });
});

// Demo reply generator
function demoReply(userMessage) {
  const m = (userMessage || "").toString().toLowerCase();

  if (m.includes("library")) {
    return "📚 The campus library is open Mon–Fri 8:00–22:00, Sat–Sun 9:00–18:00. Study rooms can be booked via the student portal.";
  }
  if (m.includes("canteen") || m.includes("food") || m.includes("mess")) {
    return "🍴 The canteen serves breakfast, lunch, and dinner. Peak lunch hours are 12:30–14:00, so try to come early!";
  }
  if (m.includes("hostel") || m.includes("accommodation")) {
    return "🏠 Hostel allocations are posted on the student services page. For urgent help, email hostel-support@campus.edu.";
  }
  if (m.includes("placement") || m.includes("internship") || m.includes("jobs")) {
    return "💼 The Placement Cell updates openings on the Careers Portal. Weekly mock interviews are held every Friday.";
  }
  if (m.includes("wifi") || m.includes("internet")) {
    return "🌐 Campus Wi-Fi SSID: CampusNet. Log in with your student ID and password. For issues, contact IT support.";
  }
  if (m.includes("exam") || m.includes("timetable")) {
    return "📝 Exam timetables are published 2–3 weeks before exams on the Academic Calendar page. Check your department notice board too.";
  }
  if (m.includes("sports") || m.includes("gym")) {
    return "⚽ The Sports Complex is open 6:00–22:00. Gym membership is required, but day passes are also available.";
  }

  return `🤖 (Demo mode) I received your message: "${userMessage}". I can help with campus timings, facilities, events, or contacts.`;
}

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`✅ Server running on http://localhost:${PORT}`);
});


[script.js](https://github.com/user-attachments/files/22688816/script.js)
const chatbox = document.getElementById('chatbox');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');

sendBtn.addEventListener('click', sendMessage);
userInput.addEventListener('keypress', (e) => { if(e.key === "Enter") sendMessage(); });

function addMessage(sender, message) {
    const p = document.createElement('p');
    p.innerHTML = `<strong>${sender}:</strong> ${message}`;
    chatbox.appendChild(p);
    chatbox.scrollTop = chatbox.scrollHeight;
}

async function sendMessage() {
    const message = userInput.value.trim();
    if (!message) return;

    addMessage('You', message);
    userInput.value = '';

    const typingMsg = document.createElement('p');
    typingMsg.innerHTML = `<strong>Campus GPT:</strong> Typing...`;
    chatbox.appendChild(typingMsg);
    chatbox.scrollTop = chatbox.scrollHeight;

    try {
        const res = await fetch('/chat', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ message })
        });
        const data = await res.json();
        typingMsg.innerHTML = `<strong>Campus GPT:</strong> ${data.reply}`;
    } catch (err) {
        console.error(err);
        typingMsg.innerHTML = `<strong>Campus GPT:</strong> ⚠️ Error connecting to server`;
    }
}

{
  "name": "hackathon",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "type": "module",
  "scripts": {
    "start": "node server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^17.2.3",
    "express": "^5.1.0",
    "node-fetch": "^3.3.2"
  }
}
![image](https://github.com/user-attachments/assets/419acdc1-d3b7-4d64-be22-98382e3de05e)
