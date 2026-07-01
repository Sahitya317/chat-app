# 💬 Chat App — Real-Time Messaging (MERN + Socket.io)

A full-stack real-time chat application built with the **MERN stack** (MongoDB, Express, React, Node.js) and **Socket.io** for instant, bidirectional communication. Users can sign up, chat one-on-one in real time, see who's online, share images, and manage their profile.

---

## ✨ Features

- 🔐 **Authentication** — Secure signup/login with JWT and bcrypt password hashing
- ⚡ **Real-time messaging** — Instant message delivery using WebSockets (Socket.io)
- 🟢 **Online/offline status** — Live indicator of which users are currently active
- 🖼️ **Image sharing** — Send images in chat, stored via Cloudinary
- 🔔 **Unseen message counter** — Per-user unread message badge in the sidebar
- 🔍 **User search** — Quickly filter/search users to start a conversation
- 👤 **Profile management** — Update name, bio, and profile picture
- 📱 **Responsive UI** — Built with Tailwind CSS for a clean, mobile-friendly layout

---

##  Tech Stack

**Frontend**
- React 
- React Router v7
- Tailwind CSS v4
- Axios
- Socket.io-client
- React Hot Toast

**Backend**
- Node.js + Express 5
- MongoDB with Mongoose
- Socket.io
- JWT (jsonwebtoken) for authentication
- bcryptjs for password hashing
- Cloudinary for image storage

**Deployment**
- Vercel (both client and server)

---

## Architecture

```
[React Client] ── REST API (Axios) ──► [Express Server] ──► [MongoDB]
       │                                       │
       └────────── Socket.io (WebSocket) ──────┘
                                                │
                                          [Cloudinary]
```

- Regular data operations (login, fetching chat history, updating profile) go through REST APIs.
- Real-time events (new messages, online status updates) are pushed instantly via Socket.io, running on the same HTTP server as Express.
- Images are converted to base64 on the client, uploaded to Cloudinary by the backend, and only the returned URL is stored in MongoDB.

---

## Project Structure

```
chat-app/
├── client/                     # React frontend
│   ├── context/
│   │   ├── AuthContext.jsx     # Auth state, login/logout, socket connection
│   │   └── ChatContext.jsx     # Messages, users, chat state
│   ├── src/
│   │   ├── components/         # Sidebar, ChatContainer, RightSidebar
│   │   ├── pages/               # LoginPage, HomePage, ProfilePage
│   │   └── App.jsx
│   └── package.json
│
├── server/                     # Express backend
│   ├── controllers/            # userControllers.js, messageController.js
│   ├── models/                 # User.js, Message.js (Mongoose schemas)
│   ├── routes/                 # userRoutes.js, messageRoutes.js
│   ├── middleware/
│   │   └── auth.js             # JWT route protection
│   ├── lib/                    # db.js, utils.js, cloudinary.js
│   └── server.js               # Entry point (Express + Socket.io)
│
└── README.md
```

---

## 🔌 API Reference

### Auth Routes — `/api/auth`

| Method | Endpoint | Protected | Description |
|--------|----------|:---------:|-------------|
| POST | `/SignUp` | ❌ | Register a new user |
| POST | `/login` | ❌ | Log in an existing user |
| GET | `/check` | ✅ | Verify token / auto-login on refresh |
| PUT | `/update-profile` | ✅ | Update name, bio, or profile picture |

### Message Routes — `/api/messages`

| Method | Endpoint | Protected | Description |
|--------|----------|:---------:|-------------|
| GET | `/users` | ✅ | Get all users + unseen message counts |
| GET | `/:id` | ✅ | Get full conversation with a specific user |
| PUT | `/mark/:id` | ✅ | Mark a specific message as seen |
| POST | `/send/:id` | ✅ | Send a text/image message to a user |

> All protected routes require a valid JWT sent in the `token` request header.

---

##  Getting Started

### Prerequisites
- Node.js (v18+ recommended)
- A MongoDB database (local or [MongoDB Atlas](https://www.mongodb.com/atlas))
- A [Cloudinary](https://cloudinary.com/) account (for image uploads)

### 1. Clone the repository
```bash
git clone https://github.com/Sahitya317/chat-app.git
cd chat-app
```

### 2. Set up the backend
```bash
cd server
npm install
```

Create a `.env` file inside `server/` with:
```env
MONGODB_URI=your_mongodb_connection_string
PORT=5000
JWT_SECRET=your_jwt_secret
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

Run the server:
```bash
npm run server   # development (with nodemon)
# or
npm start         # production
```

### 3. Set up the frontend
```bash
cd ../client
npm install
```

Create a `.env` file inside `client/` with:
```env
VITE_BACKEND_URL=http://localhost:5000
```

Run the client:
```bash
npm run dev
```

The app should now be running at `http://localhost:5173` (client) and `http://localhost:5000` (server).

---

##  Security Notes

- Passwords are hashed using bcrypt before being stored — plain-text passwords are never saved.
- JWT tokens are used for stateless authentication and expire after 7 days.
- Protected routes verify the JWT via middleware before granting access to any user-specific data.
- Sensitive fields (like `password`) are always excluded from API responses.

---

## 🚀 Future Improvements

- Group chat support
- Typing indicators
- Message delivery/read receipts (double-tick style)
- Redis adapter for Socket.io to support horizontal scaling
- Push notifications
- Unit and integration tests

---

## 👤 Author

**Sahitya**
GitHub: [@Sahitya317](https://github.com/Sahitya317)

---

