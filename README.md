[README.md](https://github.com/user-attachments/files/32999920/README.md)
# Emergency Resource Command Center

A production-style demo application for **Problem 90 — Emergency Resource Matching**.

The system helps an emergency command center manage emergencies and available resources, calculate suitable matches, prioritize urgent requests, and manually book resources.

## Main Features

- Emergency request management
- Resource management
- Priority Queue for emergency ordering
- Greedy resource selection
- Emergency-to-resource matching
- Distance-based matching
- Resource capacity and availability tracking
- Manual resource booking
- Match score and rationale
- Simulation mode
- Analytics dashboard
- Priority Queue visualization
- Bipartite graph / network visualization
- Map visualization
- Weight tuning for the matching formula
- Manual Test Mode for creating emergencies and resources
- SQLite database for local development
- React + TypeScript frontend
- Flask + SQLAlchemy backend

## Project Structure

```text
EmergencyResourceCC/
│
├── backend/
│   ├── algorithms/
│   ├── app/
│   ├── models/
│   │   ├── db.py
│   │   └── models.py
│   ├── routes/
│   │   └── api.py
│   ├── services/
│   ├── utils/
│   └── run.py
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Panels.tsx
│   │   │   └── Viz.tsx
│   │   ├── App.tsx
│   │   ├── api.ts
│   │   ├── main.tsx
│   │   ├── styles.css
│   │   └── types.ts
│   ├── package.json
│   └── vite.config.ts
│
├── data/
├── scripts/
├── tests/
├── requirements.txt
├── README.md
└── start-dev.ps1
```

## Technologies Used

### Frontend
- React
- TypeScript
- Vite
- CSS
- Visualization components

### Backend
- Python
- Flask
- SQLAlchemy
- SQLite

### Algorithms

The core matching logic is implemented directly in the project rather than relying on a black-box matching library.

1. **Priority Queue**
   - Orders emergencies according to urgency/severity and waiting time.

2. **Greedy Selection**
   - Selects the best currently available resource according to the calculated score.

3. **Matching**
   - Compares emergency requirements with resource type, equipment, availability and geographic distance.

4. **Scoring**
   - A weighted score combines factors such as:
     - urgency
     - distance
     - suitability
     - availability

## How to Run the Project

### Step 1 — Open the project

Open the `EmergencyResourceCC` folder in VS Code.

### Step 2 — Start the Backend

Open a terminal in VS Code:

```text
Terminal → New Terminal
```

Go to the backend:

```powershell
cd backend
```

Activate the virtual environment if required:

```powershell
..\ .venv\Scripts\Activate.ps1
```

If the command above gives an error because of the space, use:

```powershell
..\.venv\Scripts\Activate.ps1
```

Then start Flask:

```powershell
python run.py
```

You should see something similar to:

```text
* Running on http://127.0.0.1:5000
```

**Keep this terminal running.**

Do not press `Ctrl + C` while using the application.

---

### Step 3 — Start the Frontend

Open another VS Code terminal.

Keep the backend terminal running and open a second terminal using:

```text
Terminal → New Terminal
```

Go to the frontend:

```powershell
cd frontend
```

Install dependencies if this is the first run:

```powershell
npm install
```

Then start Vite:

```powershell
npm run dev
```

You should see something similar to:

```text
Local: http://localhost:5173/
```

---

## How to View the Application

Open your browser and visit:

```text
http://localhost:5173
```

The Emergency Resource Command Center should appear.

You normally need **two terminals running**:

### Terminal 1 — Backend

```powershell
cd backend
python run.py
```

Running at:

```text
http://127.0.0.1:5000
```

### Terminal 2 — Frontend

```powershell
cd frontend
npm run dev
```

Running at:

```text
http://localhost:5173
```

## Important: Do Not Close the Backend

If the frontend opens but buttons such as **BOOK RESOURCE**, matching, analytics or simulation fail, first check that the backend terminal is still running.

The frontend communicates with the Flask backend through API endpoints.

## Manual Test Mode

The application includes a **Manual Test Mode**.

Use it to create test data without modifying the database manually.

### Create an Emergency

Enter:

- Title
- Severity
- Resource type
- Equipment
- Latitude
- Longitude

Then click:

```text
+ PUSH TO QUEUE
```

The emergency will be sent to the backend and added to the queue.

### Create a Resource

Enter:

- Resource name
- Resource type
- Equipment
- Capacity

Then click:

```text
+ ADD RESOURCE
```

The resource becomes available to the matching system.

## Booking a Resource

The **BOOK RESOURCE** section allows you to select:

```text
Emergency
+
Resource
```

Then click:

```text
BOOK RESOURCE
```

The backend creates an assignment and decreases the resource's available capacity.

When the available capacity reaches zero, the resource is marked as busy.

## If You See "The Requested URL Was Not Found"

If the interface displays:

```text
The requested URL was not found on the server.
```

check the Flask backend route in:

```text
backend/routes/api.py
```

The frontend booking request is expected to call:

```text
/api/book
```

The backend must have a matching Flask route, for example:

```python
@app.route("/api/book", methods=["POST"])
def book_resource():
    ...
```

If the backend uses a different URL, the frontend request in:

```text
frontend/src/components/Panels.tsx
```

must use the same endpoint.

## Checking the Backend

With Flask running, the terminal should show requests such as:

```text
GET /api/analytics
GET /api/simulation
GET /api/match/status
```

with HTTP status:

```text
200
```

A `200` response means the endpoint responded successfully.

## Typical Development Workflow

Start both servers:

```powershell
# Terminal 1
cd backend
python run.py
```

```powershell
# Terminal 2
cd frontend
npm run dev
```

Then open:

```text
http://localhost:5173
```

Test the application in this order:

1. Check the dashboard.
2. Check existing emergencies.
3. Check existing resources.
4. Run matching.
5. Inspect the Priority Queue.
6. Inspect the match results.
7. Create a test emergency.
8. Create a test resource.
9. Select an emergency and resource.
10. Book the resource.
11. Verify that resource capacity/status changes.
12. Check analytics and simulation.

## Stopping the Application

In each terminal press:

```text
Ctrl + C
```

Stop the backend and frontend separately.

## Troubleshooting

### `python` is not recognized

Make sure Python is installed and the virtual environment is activated.

### `npm` is not recognized

Install Node.js and restart VS Code.

### Frontend does not start

Run:

```powershell
cd frontend
npm install
npm run dev
```

### Backend does not start

Run:

```powershell
cd backend
python run.py
```

Check the error printed in the terminal.

### API requests fail

Make sure:

```text
Backend → http://127.0.0.1:5000
Frontend → http://localhost:5173
```

are both running.

### Port already in use

If port `5000` or `5173` is already being used, stop the previous development server or use the port shown by the terminal.

## Demonstrating the Algorithm

For a project/demo presentation, demonstrate the system like this:

```text
Emergency Created
       ↓
Priority Queue
       ↓
Highest Priority Emergency
       ↓
Find Compatible Resources
       ↓
Calculate Match Scores
       ↓
Greedy Selection
       ↓
Best Available Resource
       ↓
Assignment
       ↓
Capacity Updated
       ↓
Resource Status Updated
```

## Project Goal

The Emergency Resource Command Center demonstrates how algorithmic resource allocation can be used in an emergency-management scenario.

Instead of manually checking every resource, the system:

- prioritizes emergencies,
- filters incompatible resources,
- considers distance and availability,
- calculates match scores,
- selects a suitable resource,
- records the assignment,
- and updates resource capacity.

This provides a practical demonstration of **Priority Queue + Greedy Selection + Matching** algorithms in a full-stack application.

## Development Note

This is a demonstration/development application. The Flask development server should not be used as the production server. For production deployment, use an appropriate WSGI server and production database/configuration.
