<template>
    <div class="chatbot-container">
      <div class="header">
        <h1>Chat</h1>
      </div>
      <div class="chat-window" ref="chatWindow">
        <div v-for="(message, index) in messages" :key="index" :class="`message ${message.sender}`">
          <div class="avatar">
            <img :src="message.sender === 'user' ? 'user-avatar.png' : 'bot-avatar.png'" :alt="`${message.sender} Avatar`">
          </div>
          <div class="message-content">
            <p>{{ message.text }}</p>
          </div>
          <div v-if="message.sender === 'user'" class="bubble-tail user-tail"></div>
          <div v-else class="bubble-tail bot-tail"></div>
        </div>
      </div>
      <div class="input-area">
        <div v-show="isTyping" class="typing-indicator">
          <span></span>
          <span></span>
          <span></span>
        </div>
        <input
          type="text"
          placeholder="Type your message..."
          v-model="userInput"
          @keyup.enter="sendMessage"
        >
        <button @click="sendMessage">
          <i class="fas fa-paper-plane"></i>
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        messages: [
          { sender: 'bot', text: 'Hello, how can I assist you today?' },
        ],
        userInput: '',
        isTyping: false,
      };
    },
    methods: {
      sendMessage() {
        if (this.userInput.trim()) {
          this.messages.push({ sender: 'user', text: this.userInput.trim() });
          this.scrollToBottom();
          this.userInput = '';
          this.isTyping = true;
  
          setTimeout(() => {
            this.isTyping = false;
            const botResponse = this.getBotResponse(this.userInput.trim());
            this.messages.push({ sender: 'bot', text: botResponse });
            this.scrollToBottom();
          }, 1000);
        }
      },
      getBotResponse(userMessage) {
        // Replace this with your actual chatbot logic
        const responses = [
          'Hello!',
          'How can I assist you today?',
          "I'm afraid I don't understand. Can you rephrase your question?",
          'Thank you for your message. Let me think about it.',
        ];
        return responses[Math.floor(Math.random() * responses.length)];
      },
      scrollToBottom() {
        this.$nextTick(() => {
          const chatWindow = this.$refs.chatWindow;
          chatWindow.scrollTop = chatWindow.scrollHeight;
        });
      },
    },
  };
  </script>
  
  <style>
  body {
    font-family: 'Roboto', sans-serif;
    margin: 0;
    padding: 0;
    background: linear-gradient(135deg, #667eea, #764ba2);
    background-size: 400% 400%;
    animation: gradientAnimation 15s ease infinite;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  @keyframes gradientAnimation {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }
  
  .chatbot-container {
    max-width: 500px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    overflow: hidden;
  }
  
  .header {
    background-color: #4caf50;
    color: #fff;
    padding: 15px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
  }
  
  .chat-window {
    height: 400px;
    overflow-y: scroll;
    padding: 20px;
    background-color: #fff;
    display: flex;
    flex-direction: column;
  }
  
  .input-area {
    display: flex;
    align-items: center;
    padding: 10px;
    background-color: #f5f5f5;
  }
  
  input[type='text'] {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 20px;
    outline: none;
    font-size: 16px;
  }
  
  button {
    margin-left: 10px;
    padding: 10px 15px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  .message {
    display: flex;
    align-items: flex-start;
    margin-bottom: 10px;
    position: relative;
  }
  
  .user {
    flex-direction: row-reverse;
    align-self: flex-end;
  }
  
  .bot-message {
    align-self: flex-start;
  }
  
  .avatar {
    width: 40px;
    height: 40px;
    margin-right: 10px;
  }
  
  .user .avatar {
    margin-right: 0;
    margin-left: 10px;
  }
  
  .avatar img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
  }
  
  .message-content {
    padding: 10px 15px;
    border-radius: 20px;
    max-width: 70%;
  }
  
  .user .message-content {
    background-color: #e2f3eb;
    color: #333;
    border-radius: 20px 20px 0 20px;
  }
  
  .bot-message .message-content {
    background-color: #f1f0f0;
    color: #333;
    border-radius: 20px 20px 20px 0;
  }
  
  .bubble-tail {
    position: absolute;
    width: 0;
    height: 0;
    border-style: solid;
  }
  
  .user-tail {
    right: -10px;
    top: 10px;
    border-color: transparent transparent transparent #e2f3eb;
    border-width: 10px;
  }
  
  .bot-tail {
    left: -10px;
    top: 10px;
    border-color: transparent #f1f0f0 transparent transparent;
    border-width: 10px;
  }
  
  .typing-indicator {
    display: flex;
    margin-right: 10px;
  }
  
  .typing-indicator span {
    display: inline-block;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: #aaa;
    animation: typing 1s infinite;
  }
  
  .typing-indicator span:nth-child(2) {
    animation-delay: 0.2s;
  }
  
  .typing-indicator span:nth-child(3) {
    animation-delay: 0.4s;
  }
  
  @keyframes typing {
    0% {
      transform: translateY(0);
      opacity: 0.5;
    }
    50% {
      transform: translateY(-5px);
      opacity: 1;
    }
    100% {
      transform: translateY(0);
      opacity: 0.5;
    }
  }
  </style>