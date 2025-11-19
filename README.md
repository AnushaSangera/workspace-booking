# Complete Project
# Workspace Booking System
    This project is a small workspace/meeting-room booking system where users can book rooms by the hour.
    It includes basic conflict checks, pricing logic, cancellation rules, and an analytics API for admins.

# How to Run the Project
    # Backend

        Go to the backend folder:

        cd backend


        Install packages:

        npm install


        Create a .env file with:

        PORT=5000
        MONGO_URI=your_mongodb_connection_string

    Start the server:

        npm start


        The backend should run on:
        http://localhost:5000

    Frontend

        Go to the frontend folder:

        cd frontend


    Install packages:

        npm install


    Run the app:

    npm run dev


    Frontend will run on:
        http://localhost:5173 (or whichever port uses)

        If needed, update the API URL in:

        frontend/src/api/httpClient.js

# API Examples
    Create a New Booking

    POST /api/bookings

    {
    "roomId": "101",
    "userName": "Priya",
    "startTime": "2025-11-20T10:00:00.000Z",
    "endTime": "2025-11-20T12:30:00.000Z"
    }

    Conflict Response Example
    {
    "error": "Room already booked from 10:30 AM to 11:30 AM"
    }

    Cancel a Booking

    POST /api/bookings/:id/cancel

    Example error:

    {
    "error": "Cannot cancel within 2 hours of start time"
    }

    Analytics API

    GET /api/analytics?from=2025-11-01&to=2025-11-30

    Response:

    [
    {
        "roomId": "101",
        "roomName": "Cabin 1",
        "totalHours": 15.5,
        "totalRevenue": 5250
    }
    ]

# Deployment Links

    Add your links here after deploying:

    Frontend: https://your-frontend-url.com

    Backend: https://your-backend-url.com