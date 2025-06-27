🧠 iBot - Smart Python Assistant with GUI (Math + Wiki + DuckDuckGo)
iBot is a Python-based desktop assistant with a modern dark-themed GUI that can:

🧮 Solve math problems (like 2x + 5 = 15, log(100), sqrt(25))

📘 Answer general knowledge questions using Wikipedia

🔍 Provide fallback web responses using the DuckDuckGo Instant Answer API

💬 All within a responsive GUI — no browser redirection needed

🚀 Features
🖥️ Modern GUI built with Tkinter (dark mode)

🧠 Math Engine powered by SymPy

🌐 Wikipedia Lookup for factual knowledge

🦆 DuckDuckGo Fallback for web-like answers without leaving the app

🤖 Smart Fallback Chain:
Math → Wikipedia → DuckDuckGo → Friendly response

✅ Clean and styled chat interface with hover effects and colored tags

📸 Screenshot
(Add screenshot image here if you have one)

🧪 Example Queries
User Input	Bot Response
2x + 3 = 9	x = [3]
log(100)	The answer is: 4.605...
What is AI?	Wikipedia summary on Artificial Intelligence
How to build a chatbot?	DuckDuckGo short answer (if available)

🛠️ Installation
1. Clone the repository
bash
Copy
Edit
git clone https://github.com/your-username/ibot-assistant.git
cd ibot-assistant
2. Install dependencies
bash
Copy
Edit
pip install sympy wikipedia requests
3. Run the bot
bash
Copy
Edit
python ibot.py
📁 Project Structure
bash
Copy
Edit
ibot-assistant/
│
├── ibot.py               # Main bot script with GUI
├── README.md             # This file
└── assets/               # (Optional) Screenshots or assets
🎨 GUI Preview
Dark mode UI

Chat-style layout (user vs bot)

Input bar and stylized "Ask" button

Answers stay inside the window — no external browser

💡 Future Ideas
🗣️ Voice input and output (speech recognition + TTS)

🌐 GPT fallback using OpenAI API

📊 History/log export

🌗 Theme toggle (dark/light)

📄 License
MIT License

🙌 Acknowledgements
sympy

wikipedia

DuckDuckGo API

Inspired by the idea of combining local intelligence with online knowledge










