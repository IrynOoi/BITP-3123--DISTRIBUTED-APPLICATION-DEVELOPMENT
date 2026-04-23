
## Team Member Contribution

**WONG JIA XI** B032310495  
**CHAI JIAN JIE** B032310296  
**YAP ZHAN HONG** B032310386  
**OOI XIEN XIEN** B032310183  
**LOO WEN CHUAN** B032310334  
**NG CHUN KEAT** B032310265



Note: The technical documentation should be provided on Github as a Markdown readme file located at the root of your repository. The contents may include the following:  
## Introduction  

The EventGo application is a desktop-based event management and attendance tracking system designed to support the end-to-end process of organizing, managing, and monitoring events. Developed using Java Swing within the Eclipse IDE, the application provides a graphical user interface (GUI) for both administrators and users to interact with the system. It integrates with Firebase Authentication for secure login access and utilizes MySQL or Firebase Firestore for backend data storage, ensuring a reliable and scalable architecture.
The system is structured to manage six key business processes: event creation, user registration, registration review, QR code generation, on-site check-in, and attendance reporting. Admin users can log in via Firebase and access an interface to perform full CRUD operations on events. This allows them to create, edit, view, and delete event information directly through the application, ensuring flexible event management capabilities.
Users can register for events using a built-in form, which captures and stores their registration with an initial status of "Pending Approval". This registration data is stored in the backend and awaits review by a designated reviewer or administrator. The review process enables authorized personnel to assess registrations and approve or reject them based on predefined criteria. The user-friendly interface streamlines this process and updates the registration status accordingly.
For registrations that are approved, the system automatically generates a QR Code using the ZXing library. This QR code encodes the unique combination of userId and eventId, serving as a digital ticket for event check-in. The code is displayed on the confirmation screen or made available for download, enabling seamless check-in at the event venue.
During on-site check-in, staff members can scan QR codes using a webcam or scanning tool integrated into the system. The scanned code is matched against the backend records to validate the registration, and the system then records the check-in time. This process ensures accurate attendance tracking and prevents unauthorized access to the event.
Finally, the EventGo application supports real-time attendance monitoring and reporting. Administrators can generate statistics such as total registrants, actual attendance count, and event attendance rates. Reports can be exported in CSV or JSON formats, offering flexibility for analysis or archival purposes. Overall, EventGo provides a robust and efficient solution for event administration, user participation, and data-driven decision-making.  

## Project Overview  

EventGo is a desktop application built with Java Swing that simplifies event management and attendance tracking. It allows admins to create events, review registrations, and generate QR codes for approved participants. Users can register through the system, and staff can scan QR codes on-site to record attendance.
The system uses Firebase for authentication and MySQL or Firestore for data storage. It solves common issues like manual registration, slow check-in, and poor attendance tracking by automating these processes. EventGo is ideal for schools, tuition centers, and small organizations looking for an efficient event management solution.

## Project Objectives
To automate and simplify the end-to-end event management workflow, including event creation, user registration, and approval, thereby reducing manual administrative tasks and increasing operational efficiency.


To implement a secure and accurate QR code-based check-in system, ensuring reliable attendance tracking and reducing the possibility of fraud or manual error during participant verification.


To provide real-time attendance data and exportable reports in formats such as CSV or JSON, enabling event organizers to analyze participation trends and make informed decisions for future planning.


## Commercial Value / Third-Party Integration:   

The EventGo application demonstrates strong potential for real-world commercial deployment, especially in educational institutions, training centers, corporate seminars, and community event organizations. Its modular design and desktop interface make it suitable for in-house deployment in environments where centralized control and offline accessibility are important. By automating processes such as registration approval, QR-based check-in, and attendance tracking, the system reduces administrative burden and human error, while improving participant experience and data accuracy.
To ensure security, scalability, and ease of integration, EventGo leverages trusted third-party services, most notably Firebase Authentication and Firestore Database, both from Google Firebase.
Firebase Authentication is integrated to handle secure student and staff login. It provides a reliable and scalable method for authenticating users using email-password credentials, ensuring that only authorized individuals can access sensitive features like event registration or attendance monitoring. This is particularly important for educational or official settings, where privacy and account control are critical.


Firestore Database serves as a backup storage for check-in data. In addition to any primary storage mechanism (such as MySQL), Firestore offers real-time synchronization, cloud-based access, and high availability. This backup layer ensures that attendance data remains intact even if local databases fail, and it can be used to sync real-time check-in statuses across devices or locations.


The use of these external APIs not only accelerates development by handling complex backend tasks (authentication, syncing, cloud storage) but also provides industry-standard reliability and security. This makes EventGo highly adaptable for institutions looking for a low-maintenance, high-performance solution for event management.


## System Architecture

High-Level Diagram: A visual representation (e.g., a block diagram) showing how all components interact: the two frontend apps, the backend server, the database, and any external services. This provides a clear overview of the distributed system.

<img width="940" height="520" alt="image" src="https://github.com/user-attachments/assets/da6f96ea-c499-4c02-ab71-d78fdda7f7a5" />

## Backend Application

Technology Stack: 

<img width="757" height="681" alt="image" src="https://github.com/user-attachments/assets/485b2571-8329-4cf7-99ff-150e911126a0" />


## API Documentation: 

**1.Security**  

All endpoints requiring user authentication use Firebase ID Token:
Header: Authorization: Bearer <Firebase_ID_Token>
The token is validated using Firebase Admin SDK to verify identity.  

**2. User API**  

2.1 POST /api/user/register
Function: Register a new user.
Request Body:
{
  "userId": "U001",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "student"
}  

Success Response:  

Status: 200  

Body: "User registered successfully"  

Error Response:  

Status: 500  

Body: "Failed to register account: <error_message>"  

2.2 POST /api/user/email  

Function: Retrieve user info by email.  

Request Body:
{
  "email": "john@example.com"
}  

Success Response:
{
  "userId": "U001",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "student",
  "createdAt": "2024-01-01T12:00:00"
}  

Error Response:  

Status: 404  

Body: (no content)


**3. Registration API**  

3.1 POST /api/registration/{eventId} 

Function: Register a user to an event.   

Path Variable:  

eventId (integer)  

**Headers:**
Authorization: Bearer <Firebase_ID_Token>  

Success Response:  

Status: 200  

Body: "Registered user <userId> to event <eventId>"  

Error Responses:  

404: User not found  

409: Already registered  

500: Internal error


3.2 POST /api/registration/update-status  

Function: Update a user's registration status.
Request Body:
{
  "registrationId": "12",
  "status": "approved"
}  

Success Response:  

Status: 200  

Body: "Status updated"

3.3 GET /api/registration/registered  

Function: Retrieve all events the user has registered for.  

Headers:  

Authorization: Bearer <Firebase_ID_Token>  

Success Response:
[
  {
    "eventId": 1,
    "eventTitle": "Hackathon",
    "eventDate": "2025-07-25"
  }
]

3.4 GET /api/registration/byEvent?eventId=1  

Function: Retrieve list of registrations for a specific event.  

Query Parameter: eventId (integer)  

Success Response:
[
  {
    "userId": "U001",
    "name": "John Doe",
    "status": "approved"
  }
]

**4. Event API**  

4.1 GET /api/events  

Function: Get all published events.  

Success Response:
[
  {
    "eventId": 1,
    "title": "Tech Fest",
    "description": "Annual festival",
    "date": "2025-08-01"
  }
]

4.2 GET /api/events/QRCode?eventId=1  

Function: Get QR code PNG image for the event.  

Success Response:  

Status: 200  

Content-Type: image/png  

Error Response:  

404 or 500 with error message in text/plain

4.3 GET /api/events/attendanceStats?event_id=1
Function: Get attendance stats for a specific event.  

Success Response:
{
  "totalRegistered": 50,
  "totalCheckedIn": 35
}

4.4 POST /api/events/publish  

Function: Publish a new event.  

Request Body:
{
  "title": "AI Conference",
  "description": "A conference on AI",
  "date": "2025-09-10"
}  

Success Response:  

Status: 200  

Body: "Activity published successfully"  

Error Response:  

Status: 500  

Body: "Failed to publish activity"

4.5 GET /api/events/available  

Function: List events open for registration.

**5. Check-in API**
5.1 POST /api/checkin?qrToken=abc123  

Function: Check in a user to an event using QR token.  

Headers:  

Authorization: Bearer <Firebase_ID_Token>  

Success Response:  

Status: 200
Body: "✅ Check-in successful"  

Error Responses:  

404: Not found  

403: Rejected/Pending registration  

500: Server error  

5.2 GET /api/checkin/signIn?event_id=1  

Function: Get a list of users who have checked in for the event.  

Success Response:
[
  {
    "student_id": "U001",
    "name": "John Doe",
    "sign_in_time": "2025-07-18 10:30:00"
  }
]


## Frontend Applications

## Admin Frontend Application:

**Purpose:**   
This application is designed for university administrators. It allows them to publish new activities, generate QR codes for sign-in, manage participant registrations, monitor attendance, and view statistics. The goal is to simplify event management within the university environment.

**Technology Stack:** 
Language: Java
UI Framework: Java Swing
Charting Library: JFreeChart (for visualizing attendance statistics)
Others: Java Standard Library, ImageIcon (for QR display)

API Integration: The Java Swing application communicates with the Firebase backend via HTTP requests using HttpURLConnection. Each action (e.g., publishing activity, fetching QR code, retrieving attendance data) triggers an API call to a Firebase Cloud Function endpoint, which interacts with Firestore. The app parses JSON responses and updates the UI accordingly.


## Student Frontend Application:

**Purpose:** This frontend targets students and participants. It allows users to browse events, register, check registration status, and scan QR codes to sign in at events. The goal is to make the registration and attendance process smooth and digital.

**Technology Stack:** 
Language: Java
UI Framework: Java Swing
Others: Java Standard Library, Webcam/QR scanning libraries (e.g., ZXing for Java)

API Integration: Similar to the admin app, the student frontend communicates with the Firebase backend using HTTP requests (via HttpURLConnection). It allows students to fetch a list of available activities, register for an activity and submit QR scan results for attendance tracking. The app handles JSON responses and provides relevant status messages or updates to the UI accordingly.




## Database Design

Entity-Relationship Diagram (ERD): A diagram showing the database tables, their columns (with data types), and the relationships between them (one-to-one, one-to-many).

<img width="780" height="626" alt="image" src="https://github.com/user-attachments/assets/3812d1e9-187c-48a9-a82d-142f0094c280" />



**Schema Justification:**

The database schema for eventgo_db is structured to efficiently support an event registration and attendance system, emphasizing clear relationships, referential integrity, and extensibility. The key design decisions are as follows:  

**Core Tables and Design Rationale:**

<img width="766" height="554" alt="image" src="https://github.com/user-attachments/assets/c715d449-e94f-4af1-b42b-82f48abefb47" />

**Relationships:**  

<img width="755" height="327" alt="image" src="https://github.com/user-attachments/assets/c68a552f-87e2-40d8-a22c-e351a79e2216" />

These relationships ensure clean, normalized data and easy querying of participation, attendance stats, and user-event history.

**View and Purpose:**

<img width="786" height="404" alt="image" src="https://github.com/user-attachments/assets/42a017d7-ffcd-44b5-b87a-8d7416d32636" />

Views are used to reduce complex JOIN queries in the backend logic, improving code clarity and performance.

**Security and Data Integrity**

Foreign Keys: Enforced on all major entity relationships (user_id, event_id, registration_id).


Authentication Mapping: user_id maps to Firebase UID or student ID.


QR Check-in System: Ensures check-ins are tied to event-specific tokens and valid registrations.

Business Logic and Data Validation

Use Case Diagrams/Flowcharts: Illustrate the main user flows, such as "selecting a book," "borrowing a book," and "returning a book." This visually demonstrates the business logic.

**Use Case diagram**

The Use Case Diagram provides a high-level overview of the interactions between different types of users (actors) and the core functionalities (use cases) of the EventGo application. It helps stakeholders understand who can perform what actions within the system.

**Actors Involved:**

Admin: Responsible for managing events, reviewing registrations, generating QR codes, and analyzing attendance.  

Student: Can view events, register for them, and check in using QR codes.   

<img width="464" height="969" alt="image" src="https://github.com/user-attachments/assets/8339156c-1ade-4e5c-ba75-3a46eb673461" />



## Data Validation: 
**Frontend:**
1)Empty Field Checks:

-Required input fields such as Activity Title, Venue, Date & Time, Student Name, and Email are checked before submission.


-If any mandatory field is left blank, a warning JOptionPane message is shown and the data is not submitted.


2)Format Validation:


-Email addresses are validated with regular expressions to ensure they follow the correct format (e.g., example@student.utem.edu.my).


-Numeric fields like Maximum Participants are checked to allow only digits.


-Date/Time input via JSpinner ensures the user selects valid date values.


3)Dropdown Selections:


-For combo boxes (e.g., activity categories or event filters), validation ensures that users do not proceed with the default or invalid selection.


4)QR Code Attendance:


-In the student app, before marking attendance via QR scan, the app validates that the student has already registered for that activity.

**Backend:**
1)Email Uniqueness:


-When a student registers for an activity, the backend checks Firestore to ensure the same email has not already registered for the same event.


2)Activity Capacity Check:


-Before allowing a new registration, the backend checks whether the number of current participants has reached the maximum set in the activity's max_participants field.


3）Data Consistency & Integrity:


-Cloud Functions validate that activity documents exist before writing registration or attendance data to avoid orphan records.


-Attendance submissions are validated against valid QR codes (matched with backend records) to prevent spoofing or duplicate attendance.


4）Authentication Verification (optional):


-If Firebase Authentication is integrated, backend functions also verify user tokens before accepting sensitive write operations.



## Output :

<img width="281" height="317" alt="image" src="https://github.com/user-attachments/assets/ec510baf-1cce-455e-b775-f577509cfd02" />

<br><br>

<img width="396" height="397" alt="image" src="https://github.com/user-attachments/assets/0ca8807b-7e01-4560-a4de-e1b1775ff567" />

<br><br>

<img width="394" height="390" alt="image" src="https://github.com/user-attachments/assets/5cb18c1b-da54-4a32-b26a-55c76bd0bb36" />

<br><br>

<img width="495" height="354" alt="image" src="https://github.com/user-attachments/assets/f76d58eb-b57a-4b66-bbd3-d83be375bbcf" />








