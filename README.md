Hi, Welcome to My Telegram AP-project
This project was developed as part of the Advanced Programming (AP) course at Shahid Beheshti University (SBU).
We built a backend system inspired by Telegram, focusing on scalability and real-time communication.
The server was implemented entirely in Java, using socket programming with JSON/Gson for communication.
Our team of three students collaborated, and I was primarily responsible for the backend API and protocol design.
The system supports user registration, login, and session handling with secure password hashing.
We implemented messaging, private chats, groups, channels, and chat lists with pagination and unread counters.
Bonus features include folders, archive chats, read receipts, pinned messages, and contact management.
The architecture follows a layered approach with services, protocol, and socket layers.
PostgreSQL integration was added, with in-memory fallbacks to ensure reliability during development.
This project demonstrates teamwork, backend development, and protocol design skills at a production-like level.


# 📱 Telegram Backend (Java, Socket-Based)

A backend implementation of a **Telegram-inspired messenger** built with **Java, PostgreSQL, and socket-based JSON APIs**.  
This project was developed as part of an Advanced Programming course and extended with several **bonus features** to match real-world production design.

---

## 🚀 Features

### 🔐 Authentication
- User **registration** with password hashing (SHA-256).
- User **login** with session handling and online/offline tracking.

### 💬 Messaging
- **Send messages** (`MESSAGE_SEND`) with clientTempId → server assigned ID.
- **Message delivery events** (`EVENT: MESSAGE_NEW`).
- **Message status** (`READ`, `DELIVERED`) with per-message updates.
- **Pin/Unpin messages** in a chat.
- Support for **reply messages** (threading by replyId).
- **Edit/Delete flags** on messages.

### 👥 Chats
- **Private chats (PV)** created automatically on first message.
- **Groups** with owner/admin/member roles.
- **Channels** for broadcasting.
- **Chat list API** with pagination, unread counters, muted/archive flags.
- **Pin chats** (`CHAT_PIN`).
- **Archive chats** (`SET_CHAT_ARCHIVED`).

### 📂 Folders (Bonus)
- CRUD for custom folders: create, update, delete, reorder.
- Add/remove chats to folders.
- Events when a chat’s folder membership changes.
- API for listing chats inside a folder (supports `archived` as a virtual folder).

### 👁️ Read Receipts (Bonus)
- `CHAT_READ` → server marks messages up to `upto` as read.
- Server broadcasts **per-message read events** to message owners.
- Supports **bulk marking** (upto N messages).

### 📇 Contacts (Bonus)
- **List contacts** (`contacts_list`).
- **Add contacts** (`contacts_add`) with error handling:
  - `CONTACT_ALREADY_EXISTS`
  - `PHONE_NOT_REGISTERED`
- Output includes `userId`, `displayName`, `username`, `lastSeen`, etc.

### 📌 Other Bonus Features
- **Client Registry** to push events to online users.
- **Socket-based events** for login/logout, new messages, read receipts, chat updates.
- **In-memory fallback** for all services when DB is not reachable.
- **Scalable architecture** to later extend to media sharing, reactions, and presence indicators.

---

## 🏗️ Architecture

- **Java 17**
- **Socket-based server**: TCP sockets with JSON messages (using Gson).
- **PostgreSQL** as DB (services implemented, fallback in-memory for some).
- **Services layer** (UserService, GroupService, PVService, MessageService, ContactService).
- **Protocol layer**: CommandProcessor + SocketProtocol for routing JSON commands.
- **Sockets layer**: ClientHandler, ClientRegistry, SocketServer.

---

## 📂 Project Structure

```
telegramserver/
 ├── models/        # User, Message, etc.
 ├── services/      # UserService, GroupService, MessageService, PVService, ContactService
 ├── protocol/      # SocketProtocol, CommandProcessor
 ├── sockets/       # SocketServer, ClientHandler, ClientRegistry
 └── MainServer.java
```

---

## 🧩 Protocol Example (Socket JSON)

**Send Message Request**
```json
{
  "type": "MESSAGE_SEND",
  "id": "msg-uuid",
  "chat_id": "c_101",
  "msg": {
    "kind": "text",
    "text": "Hello 👋",
    "clientTempId": "tmp-uuid"
  }
}
```

**Send Message Response**
```json
{
  "type": "MESSAGE_SEND_OK",
  "id": "msg-uuid",
  "server_time": 1757257033254
}
```

**New Message Event**
```json
{
  "type": "EVENT",
  "event": "MESSAGE_NEW",
  "chat_id": "c_101",
  "message": {
    "id": "srv-1001",
    "from": "u_42",
    "kind": "text",
    "text": "Hello 👋"
  }
}
```

---

## 🛠️ Setup & Run

1. **Clone repo**
   ```bash
   git clone https://github.com/your-username/telegram-backend.git
   cd telegram-backend
   ```

2. **Configure DB**
   - PostgreSQL with database `Telegram`.
   - Update DB credentials in `services/*.java`.

3. **Run server**
   ```bash
   javac -d out $(find src -name "*.java")
   java -cp out telegramserver.MainServer
   ```

4. **Connect with client**
   - Clients communicate via **TCP socket**.
   - Messages are newline-terminated JSON.

---

## ✨ Bonus Features Implemented
- Folders (create, update, delete, reorder).
- Archive chats.
- Read receipts (bulk + per-message).
- Pin messages.
- Contact list + add contact API.
- Client registry for event push.
- In-memory fallback services.
- Socket event system.

---

## 🧑‍💻 Author
- **Role:** Backend Developer (Socket API & Protocol Design)  
- **Focus:** Java, PostgreSQL, Socket programming, JSON protocols.  
- Extended beyond course requirements to include **production-inspired features** (folders, contacts, archive, read receipts).  

---

## 📌 Notes
- Some services currently use **in-memory fallback** (e.g., contacts, chat states).  
- DB integration points are documented with comments (`TODO: Replace with SQL`).  
- Ready for extension with mobile/desktop clients.
