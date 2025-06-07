<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>StudyMate AI</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f6fa;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .app-container {
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      width: 100%;
      max-width: 500px;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
    }

    .chat-box {
      border: 1px solid #ccc;
      padding: 10px;
      height: 300px;
      overflow-y: auto;
      background: #fafafa;
      border-radius: 8px;
      margin-bottom: 10px;
    }

    .chat-message {
      margin-bottom: 10px;
      padding: 8px;
      border-radius: 6px;
    }

    .chat-message.user {
      background-color: #dff9fb;
      text-align: right;
    }

    .chat-message.ai {
      background-color: #f1f2f6;
      text-align: left;
    }

    form {
      display: flex;
    }

    input[type="text"] {
      flex: 1;
      padding: 10px;
      font-size: 1em;
      border: 1px solid #ccc;
      border-radius: 6px;
      outline: none;
    }

    button {
      padding: 10px 15px;
      background-color: #2ecc71;
      border: none;
      color: white;
      font-weight: bold;
      margin-left: 10px;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background-color: #27ae60;
    }
  </style>
</head>
<body>
  <div class="app-container">
    <h1>StudyMate AI </h1>
    <div class="chat-box" id="chat-box"></div>
    <form id="chat-form">
      <input type="text" id="user-input" placeholder="Ask a question about your studies..." required />
      <button type="submit">Send</button>
    </form>
  </div>

  <script>
    const chatForm = document.getElementById("chat-form");
    const userInput = document.getElementById("user-input");
    const chatBox = document.getElementById("chat-box");

    // Replace with your OpenRouter API key
    const API_KEY = "your-openrouter-api-key";

    async function sendMessage(message) {
      const userMessage = `<div class="chat-message user">You: ${message}</div>`;
      chatBox.innerHTML += userMessage;
      chatBox.scrollTop = chatBox.scrollHeight;

      const response = await fetch("https://openrouter.ai/api/v1/chat/completions", {
        method: "POST",
        headers: {
          "Authorization": `Bearer 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`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          model: "openai/gpt-3.5-turbo",
          messages: [
            { role: "system", content: "You are StudyMate, a friendly and helpful study assistant for students. Answer clearly and help them understand their school subjects." },
            { role: "user", content: message }
          ]
        })
      });

      const data = await response.json();
      const reply = data.choices[0].message.content;
      const aiMessage = `<div class="chat-message ai">StudyMate: ${reply}</div>`;
      chatBox.innerHTML += aiMessage;
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    chatForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const message = userInput.value.trim();
      if (message) {
        sendMessage(message);
        userInput.value = "";
      }
    });
  </script>
</body>
</html>
