<html>
  <head>
    <title>AI Chatbot</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.min.js"></script>
    <style>
      .chatlog {
        height: 300px;
        overflow-y: scroll;
      }

      .clear-button {
        position: fixed;
        bottom: 20px;
        right: 20px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>AI Chatbot</h1>
      <div class="row">
        <div class="col-md-8 offset-md-2">
          <div class="card">
            <div class="card-body">
              <div class="chatlog">
                <p class="text-center">Welcome to our AI Chatbot. How can I assist you today?</p>
              </div>
              <hr>
              <div class="input-group mb-3">
                <input type="text" class="form-control" placeholder="Type your message here" aria-label="Type your message here" aria-describedby="button-addon2">
                <button class="btn btn-outline-secondary" type="button" id="button-addon2">Send</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button class="btn btn-danger clear-button">Clear Chat Log</button>
    </div>
    <script>
      const chatlog = document.querySelector(".chatlog");
      const inputField = document.querySelector("input");
      const sendButton = document.querySelector("#button-addon2");
      const clearButton = document.querySelector(".clear-button");

      sendButton.addEventListener("click", function () {
        const message = inputField.value;
        sendMessage(message);
        inputField.value = "";
      });

      clearButton.addEventListener("click", function () {
        chatlog.innerHTML = '<p class="text-center">Welcome to our AI Chatbot. How can I assist you today?</p>';
      });

      function sendMessage(message) {
        const userMessage = document.createElement("p");
        userMessage.innerText = `You: ${message}`;
        chatlog.appendChild(userMessage);

        const loader = document.createElement("div");
        loader.classList.add("spinner-border", "spinner-border-sm", "text-primary", "align-self-end");
        loader.setAttribute("role", "status");
        const botMessage = document.createElement("p");
        botMessage.classList.add("text-end");
        botMessage.appendChild(loader);
        chatlog.appendChild(botMessage);

        fetch("https://www.literallyanything.io/api/integrations/chatgpt", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            "systemPrompt": "How can I assist you today?",
            "prompts": [{ "role": "user", "content": message }]
          })
        })
          .then(res => res.json())
          .then(data => {
            botMessage.innerText = `Bot: ${data.response}`;
          })
          .catch(err => {
            botMessage.innerText = `Bot: Sorry, I'm unable to process your request at the moment. Please try again later.`;
            console.log(err);
          });
      }
    </script>
  </body>
</html>
