# ClassBook — Classroom Booking System
### Complete Setup & Run Guide

---

## 📁 Project Structure

```
classbook/
├── backend/
│   ├── server.js        ← Main API server (all routes)
│   ├── database.js      ← In-memory data + timetable seed data
│   └── package.json     ← Backend dependencies
│
├── frontend/
│   ├── public/
│   │   ├── index.html   ← Main HTML page
│   │   ├── style.css    ← All styles
│   │   ├── api.js       ← Functions to call the backend API
│   │   └── app.js       ← All frontend logic
│   └── package.json
│
└── README.md            ← This file
```

---

## ✅ Prerequisites

Make sure you have **Node.js** installed. Download from: https://nodejs.org

Check if it's installed by opening Terminal / Command Prompt and typing:
```
node --version
```
You should see something like `v18.0.0` or higher.

---

## 🚀 Step-by-Step: How to Run

### STEP 1 — Set up the Backend

Open a **Terminal** (or Command Prompt on Windows).

```bash
# 1. Go into the backend folder
cd classbook/backend

# 2. Install all required packages
npm install

# 3. Start the backend server
node server.js
```

You should see:
```
✅ ClassBook backend running at http://localhost:5000
```

**Leave this terminal open!** The backend must keep running.

---

### STEP 2 — Set up the Frontend

Open a **NEW Terminal window** (don't close the first one).

```bash
# 1. Go into the frontend folder
cd classbook/frontend

# 2. Install serve (a simple web server)
npm install

# 3. Start the frontend
npx serve public -p 3000
```

You should see:
```
Serving!  Local: http://localhost:3000
```

---

### STEP 3 — Open the App

Open your browser and go to:
```
http://localhost:3000
```

That's it! The app is running.

---

## 🔗 How the Frontend and Backend Are Linked

The **frontend** (index.html, app.js) runs on **port 3000**.
The **backend** (server.js) runs on **port 5000**.

In `frontend/public/api.js`, there is this line:
```javascript
const BASE_URL = 'http://localhost:5000/api';
```

Every time the frontend needs data (rooms, bookings, login, etc.), it sends
an HTTP request to `http://localhost:5000/api/...` — this is your backend.

The backend processes the request and sends back JSON data.
The frontend reads the JSON and updates the page.

**Example flow:**
```
User clicks "Book Room"
       ↓
app.js calls Bookings.book(roomId, day, slot, subject)
       ↓
api.js sends: POST http://localhost:5000/api/bookings
       ↓
server.js receives the request, saves the booking, returns success
       ↓
app.js shows a success toast and refreshes the room list
```

---

## 👤 Demo Login Accounts

| Role    | Username     | Password |
|---------|--------------|----------|
| Teacher | prof_sharma  | pass123  |
| Teacher | prof_rao     | pass123  |
| Teacher | prof_gupta   | pass123  |
| Student | student1     | pass123  |
| Student | student2     | pass123  |

---

## 🌟 Features

### Teacher can:
- **Book a Room** — select day + time slot, see available rooms, book with subject name
- **Cancel Booking** — cancel their own extra lecture bookings
- **Notify Me** — join a waitlist; get notified when a booked room becomes free
- **My Bookings** — see all their booked rooms
- **Timetable** — view the full weekly schedule
- **Notifications** — see alerts when a waitlisted room is freed

### Student can:
- **Timetable** — view the weekly schedule per day
- **Room Status** — check which rooms are free for any day/slot

---

## 🔌 API Reference (Backend Routes)

| Method | Route | Description |
|--------|-------|-------------|
| POST | /api/auth/login | Login with username + password |
| GET | /api/auth/me | Get current logged-in user |
| GET | /api/rooms | List all rooms |
| GET | /api/rooms/availability?day=&slot= | Room availability for a slot |
| GET | /api/bookings | All bookings (optionally filter by day) |
| GET | /api/bookings/mine | My bookings (teacher) |
| POST | /api/bookings | Create a booking |
| DELETE | /api/bookings/:roomId/:day/:slot | Cancel a booking |
| POST | /api/waitlist | Add to waitlist |
| DELETE | /api/waitlist | Remove from waitlist |
| GET | /api/waitlist/mine | My waitlist entries |
| GET | /api/notifications | My notifications |
| PUT | /api/notifications/read-all | Mark all notifications as read |
| GET | /api/meta | Get available days and time slots |

---

## ⚠️ Common Issues & Fixes

**"Cannot connect to server" or rooms not loading:**
- Make sure the backend is running on port 5000
- Make sure both terminals are open
- Check there's no firewall blocking port 5000

**"npm not found":**
- Install Node.js from https://nodejs.org first

**Port already in use:**
```bash
# Kill whatever is using port 5000 (Mac/Linux)
lsof -ti:5000 | xargs kill

# Or change the port in server.js (line: const PORT = 5000)
# And update api.js (line: const BASE_URL = 'http://localhost:NEWPORT/api')
```

**Data resets when backend restarts:**
- This is expected! Data is stored in memory (RAM), not a database.
- For permanent storage, add a real database (see next section).

---

## 🏗️ Upgrading to a Real Database (Optional)

Currently data is stored in memory (it resets when you restart the server).
To save data permanently, you can add **SQLite** (easy, no setup needed):

```bash
cd classbook/backend
npm install better-sqlite3
```

Then replace the `bookings` object in `database.js` with SQLite tables.
Ask your professor or check the SQLite docs for help with this step.

---

## 📝 Timetable Customization

To change the pre-loaded timetable, edit `backend/database.js`.

Find the `seedTimetable()` function and modify the timetable array:
```javascript
const timetable = [
  { r: 'R101', d: 'Monday', s: '8:00-9:00', t: 'T1', tn: 'Prof. Sharma', sub: 'Mathematics' },
  // Add more entries like this...
];
```

Room IDs: R101, R102, R201, R202, R301, R302
Teacher IDs: T1 (Prof. Sharma), T2 (Prof. Rao), T3 (Prof. Gupta)
Days: Monday, Tuesday, Wednesday, Thursday, Friday
Slots: 8:00-9:00, 9:00-10:00, 10:00-11:00, 11:00-12:00, 12:00-1:00, 1:00-2:00, 2:00-3:00, 3:00-4:00

---

## 🎓 Good luck with your college project!
