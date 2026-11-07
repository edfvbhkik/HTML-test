
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NAYMO AI MINI 🌱</title>

    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #101a16;
            color: white;
            display: flex;
            flex-direction: column;
            height: 100vh;
        }

        header {
            padding: 20px;
            text-align: center;
            background: #193b2b;
            font-size: 24px;
            font-weight: bold;
        }

        #messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
        }

        .message {
            padding: 12px;
            margin: 10px 0;
            border-radius: 12px;
            max-width: 80%;
            white-space: pre-wrap;
        }

        .user {
            background: #26734d;
            margin-left: auto;
        }

        .bot {
            background: #263b30;
        }

        footer {
            display: flex;
            padding: 15px;
            gap: 10px;
            background: #193b2b;
        }

        input {
            flex: 1;
            padding: 14px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
        }

        button {
            padding: 14px 20px;
            border: none;
            border-radius: 10px;
            background: #58c788;
            font-weight: bold;
            cursor: pointer;
        }
    </style>
</head>

<body>
    <header>🌱 NAYMO AI MINI</header>

    <div id="messages">
        <div class="message bot">
            Hey! I'm Naymo AI 🌱
            Ask me anything!
        </div>
    </div>

    <footer>
        <input id="input" placeholder="Talk to Naymo AI...">
        <button onclick="sendMessage()">Send</button>
    </footer>

    <script>
        async function sendMessage() {
            const input = document.getElementById("input");
            const messages = document.getElementById("messages");
            const text = input.value.trim();

            if (!text) return;

            const userMessage = document.createElement("div");
            userMessage.className = "message user";
            userMessage.textContent = text;
            messages.appendChild(userMessage);

            input.value = "";

            const botMessage = document.createElement("div");
            botMessage.className = "message bot";
            botMessage.textContent = "Naymo is thinking...";
            messages.appendChild(botMessage);

            messages.scrollTop = messages.scrollHeight;

            try {
                const response = await fetch("/chat", {
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    body: JSON.stringify({message: text})
                });

                const data = await response.json();

                botMessage.textContent = data.reply || data.error || "No reply received.";
            } catch (error) {
                botMessage.textContent = "Couldn't connect to Naymo's server yet!";
            }

            messages.scrollTop = messages.scrollHeight;
        }

        document.getElementById("input").addEventListener("keydown", function(event) {
            if (event.key === "Enter") sendMessage();
        });
    </script>
</body>
</html>
