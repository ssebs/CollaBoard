# CollaBoard
> Excalidraw-Lite (Go + WebSockets)

A minimal collaborative whiteboard built with **Go** (backend) and **HTML/CSS/JavaScript** (frontend).  

Users can create a room via a shareable URL and draw together in real-time with undo/redo support.

NOTE: MVP should only be drawing together


## TODO / Planning
See [TODO.md](./TODO.md)

> The rest of this page is the general idea of the project.

---

## Tech Stack

- **Backend**: Go  
  - WebSocket server (`gorilla/websocket`
  - In-memory room manager + stack-based undo/redo  

- **Frontend**: Vanilla HTML/CSS/JS  
  - `<canvas>` for drawing  
  - WebSocket client for live collaboration  
  - Toolbar: select shape, undo, redo  

## 🎯 MVP Features

- Create a unique room with a shareable URL  
- Connect multiple clients to the same room via WebSockets  
- Draw basic shapes (freehand lines, rectangles, circles)
- Draw text  
- Stack-based undo/redo (per room, synced across all users)
- Real-time collaboration (every action is broadcast to all participants)

## 🔄 Data Flow

**1. Room Creation**
- Client requests `/create-room` → backend generates a new `roomID`  
- Redirect to `/board/:roomID`  
- Shareable URL: `https://yourapp.com/board/:roomID`

**2. WebSocket Connection**
- Client connects to `/ws/:roomID`  
- Backend sends current `room.Actions` (the stack of drawing actions)  

**3. Drawing**
- User draws on `<canvas>` → serialized into an `Action` JSON:
  ```json
  {
    "type": "line",
    "from": [10, 20],
    "to": [50, 100],
    "color": "#000"
  }
  ```

* Action sent to backend → appended to `room.Actions` → broadcast to all clients

**4. Undo/Redo**

* Undo: pop last action from `Actions` → push to `RedoStack` → broadcast
* Redo: pop from `RedoStack` → push to `Actions` → broadcast
* Clients clear canvas and redraw from `Actions`

---

## 🛠️ Future Enhancements

* User cursors with identifiers
* Export board to PNG/PDF
* Room expiration after inactivity
* Authentication for private boards

