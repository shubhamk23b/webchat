# 💬 WebChat — Real-Time Chat Application

> A lightweight **real-time web chat app** built with **Node.js, Express, EJS and Socket.IO**. Messages travel over persistent **WebSocket** connections, so they appear instantly for everyone — no page refresh needed.

![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.21-000000?logo=express&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-4.8-010101?logo=socketdotio&logoColor=white)
![EJS](https://img.shields.io/badge/EJS-3.1-B4CA65)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#️-tech-stack)
- [System Architecture](#️-system-architecture)
- [How Real-Time Messaging Works](#-how-real-time-messaging-works)
- [Project Structure](#-project-structure)
- [Getting Started](#️-getting-started)
- [Scripts](#-scripts)
- [Repository Notes](#-repository-notes)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🎯 Overview

**WebChat** (package name `web-sockets`) is a learning project that demonstrates two-way, real-time communication between browsers and a server. **Express** serves the pages, **EJS** renders the chat view, and **Socket.IO** keeps a live connection open so messages are pushed to connected users the moment they are sent.

---

## 🚀 Features

- ⚡ **Instant messaging** over WebSockets (with automatic fallback handled by Socket.IO)
- 👥 **Multiple users** can chat at the same time
- 🖥️ **Server-rendered UI** with EJS templates
- 🔄 **Automatic reconnection** provided by the Socket.IO client
- 🪶 **Minimal dependencies** — just Express, EJS and Socket.IO
- 🧩 Simple structure that is easy to read and extend

<!-- TODO: confirm against app.js and views/ — e.g. usernames, join/leave notices, typing indicator, rooms. -->

---

## 🛠️ Tech Stack

| Category | Technology |
| -------- | ---------- |
| Runtime | Node.js |
| Web framework | Express `^4.21` |
| Real-time engine | Socket.IO `^4.8` |
| Templating | EJS `^3.1` |
| Frontend | HTML / CSS / JavaScript (Socket.IO client) |
| Package manager | npm |

---

## 🏗️ System Architecture

```text
┌───────────────┐                                   ┌───────────────┐
│   Browser A   │                                   │   Browser B   │
│ Socket.IO     │                                   │ Socket.IO     │
│ client        │                                   │ client        │
└───────┬───────┘                                   └───────▲───────┘
        │  1. HTTP GET /  (load page)                       │
        │  2. WebSocket handshake (persistent link)         │
        │  3. emit message                                  │ 5. push message
        ▼                                                   │
┌─────────────────────────────────────────────────────────────────────┐
│                           Node.js server                            │
│                                                                     │
│   ┌───────────────┐   serves   ┌──────────────────┐                 │
│   │   Express     │──────────► │ views/ (EJS)     │                 │
│   │   (app.js)    │            └──────────────────┘                 │
│   └───────┬───────┘                                                 │
│           │ attached to the same HTTP server                        │
│           ▼                                                         │
│   ┌───────────────────────────────────────────┐                     │
│   │            Socket.IO server               │                     │
│   │  4. receives message → broadcasts to all  │                     │
│   └───────────────────────────────────────────┘                     │
└─────────────────────────────────────────────────────────────────────┘
```

The same Node.js process handles both normal **HTTP requests** (Express) and **real-time events** (Socket.IO).

---

## 🔌 How Real-Time Messaging Works

1. A user opens the site; **Express** renders the chat page from an **EJS** view.
2. The page loads the **Socket.IO client**, which opens a persistent connection to the server.
3. When the user sends a message, the client **emits an event** to the server.
4. The server's Socket.IO handler **receives the event** and **broadcasts** it to the connected clients.
5. Every client's listener fires and **appends the message** to the chat window immediately.

```text
 Client A ── emit("message") ──►  Server ── broadcast ──►  Client A, B, C ...
```

Because the connection stays open, there is no polling and no page reload.

---

## 📁 Project Structure

```text
webchat/
│
├── app.js              # Express app + HTTP server + Socket.IO event handling
├── package.json        # Dependencies and metadata
├── package-lock.json   # Locked dependency versions
├── .gitignore
│
├── views/              # EJS templates for the chat page
└── node_modules/       # Installed dependencies (should not be committed)
```

| File / Folder | Responsibility |
| ------------- | -------------- |
| `app.js` | Creates the Express app and HTTP server, attaches Socket.IO, defines routes and socket event listeners |
| `views/` | EJS templates, including the chat UI and the client-side Socket.IO script |
| `package.json` | Declares `express`, `ejs` and `socket.io` |

---

## ⚙️ Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18 or newer
- npm (bundled with Node.js)

### 1. Clone the repository

```bash
git clone https://github.com/shubhamk23b/webchat.git
cd webchat
```

### 2. Install dependencies

```bash
npm install
```

### 3. Run the app

```bash
node app.js
```

Open the address printed in the terminal (check the `listen` port in `app.js`), e.g. `http://localhost:3000`.

### 4. Try it out

Open the app in **two browser windows** side by side and send a message from one — it appears in the other instantly.

> 💡 For auto-restart while developing: `npx nodemon app.js`

---

## 📜 Scripts

`package.json` currently has only a placeholder `test` script. Add:

```json
"scripts": {
  "start": "node app.js",
  "dev": "nodemon app.js"
}
```

Then run `npm start` (or `npm run dev` with nodemon installed).

---

## 📝 Repository Notes

- **`node_modules/` is committed** to the repository. Remove it from Git and ignore it:

  ```bash
  git rm -r --cached node_modules
  echo "node_modules/" >> .gitignore
  git commit -m "Stop tracking node_modules"
  ```
- Anyone cloning the project only needs `npm install` to restore dependencies.

---

## 🚀 Future Improvements

- [ ] Usernames and user authentication
- [ ] Chat rooms / private messaging
- [ ] "User is typing…" indicator
- [ ] Online users list and join/leave notifications
- [ ] Message history stored in a database (MongoDB / SQLite)
- [ ] Input sanitisation to prevent XSS in messages
- [ ] Timestamps and message formatting
- [ ] Deployment (Render / Railway) with environment-based port config

---

## 👨‍💻 Author

**Shubham Kanojiya**
AI/ML & Backend Developer

GitHub: [@shubhamk23b](https://github.com/shubhamk23b)

---

⭐ If you find this project useful, consider giving it a star on GitHub!
