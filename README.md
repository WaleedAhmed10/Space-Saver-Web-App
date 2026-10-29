# Space Saver — Car Parking Management System (Web)

A full-stack parking management web app built as a **Database Systems (DBMS)** course project. Customers register their vehicles and get an automatically assigned parking slot. On exit, the fee is calculated and recorded. Admins can see revenue, parked vehicles and generated reports.

> Looking for the Java/JavaFX desktop version of this idea? See the companion OOP project: **space-saver-parking-java**.

#Developed by:
- Waleed Ahmed (01-131232-093)
- Amir Ali (01-131232-011)

Dept of Software Engineering, BUIC

## Features

**Customer**
- Register / log in (passwords hashed with bcrypt)
- Register a vehicle and get a free slot assigned automatically
- View available slots, notifications and payment history
- Exit a vehicle by license plate; the fee is charged automatically

**Admin**
- View total revenue
- View currently parked vehicles
- Generate and view reports

**Database logic (the DBMS focus)**
- 8 tables: `Users`, `Vehicles`, `ParkingSlots`, `Bookings`, `EntryExitLogs`, `Payments`, `Notifications`, `Reports`, with foreign keys and indexes
- View `AvailableSlots`
- Stored procedures `RegisterVehicle` and `ExitVehicle` (fee = duration × hourly rate, minimum 1 hour)
- Function `TotalRevenue()`
- Trigger `TriggerLongParking`: sends a notification when a vehicle stays more than 2 hours

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, Vite, Tailwind CSS |
| Backend | Node.js, Express 5, mysql2, bcrypt, cors |
| Database | MySQL |

## Project Structure

```
Space-Saver-DBMS/
├── frontend/          # React + Vite app (runs on :5173)
├── backend/
│   ├── server.js      # Express REST API (runs on :3001)
│   └── hashPassword.js# One-time script to hash the seeded demo passwords
└── Space Saver.sql    # Database schema, procedures, trigger, seed data
```

## Getting Started

### Prerequisites
- Node.js 18+
- MySQL 8+

### 1. Set up the database
Run the SQL script in MySQL (Workbench or CLI):

```bash
mysql -u root -p < "Space Saver.sql"
```

This creates the `space_saver` database, all tables, procedures, the trigger, and two demo users plus two parking slots.

### 2. Configure and start the backend
Open `backend/server.js` and `backend/hashPassword.js` and set your own MySQL password in the `mysql.createPool({...})` block.

```bash
cd backend
npm install
node hashPassword.js   # hashes the demo users' plain-text passwords (run once)
node server.js         # API on http://localhost:3001
```

### 3. Start the frontend

```bash
cd frontend
npm install
npm run dev            # App on http://localhost:5173
```

### Demo accounts
| Role | Email | Password |
|---|---|---|
| Customer | `Amir@gmail.com` | `1234567` |
| Admin | `Waleed2002@gmail.com` | `12345678` |

> These work only after running `hashPassword.js`. Change or remove them before any real deployment.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/register` | Create a customer account |
| POST | `/api/login` | Log in |
| GET | `/api/slots/available` | List available slots |
| POST | `/api/vehicles/register` | Register a vehicle and assign a slot |
| POST | `/api/vehicles/exit` | Exit a vehicle and record payment |
| GET | `/api/vehicles/parked` | Currently parked vehicles |
| GET | `/api/notifications/:userID` | A user's notifications |
| GET | `/api/payments/:userID` | A user's payments |
| GET | `/api/revenue` | Total revenue (admin) |
| POST | `/api/reports/generate` | Generate a report (admin) |
| GET | `/api/reports` | List reports |

## Known Limitations
- No session tokens/JWT: the logged-in user lives in frontend state only, so endpoints are not truly protected.
- Database credentials are set directly in the source files (an `.env` file would be better).
- API URL (`http://localhost:3001`) is hardcoded in the frontend.
