# RaceDay-Part2
RaceDay – Event Management REST API
Project Overview
RaceDay is an event-management API for the South African road-running, walking and cycling community. It allows event organisers to manage events and categories, participants to enrol in events, and organisers to record results.
This project was developed as part of a RESTful API development assignment using ASP.NET Core Web API, C#, Entity Framework Core and SQL Server.
Main Features
- Participant registration and login.
- Password hashing.
- JWT-based authentication.
- Role-based access for Participant and Organiser accounts.
- View event listings and event details.
- Organiser-only event creation, updating and deletion, with ownership checks.
- View event types.
- View and manage event categories.
- Participant enrolment in an event category.
- Participants view their own enrolments and results.
- Organisers view enrolments and results for events they own.
- Organisers record finishing times and positions.
Implementation note: Public registration is restricted to the Participant role. Organiser accounts must be created through a controlled process; do not enable unrestricted public Organiser registration.
Technologies
- C#
- ASP.NET Core Web API (.NET 8)
- Entity Framework Core 8
- Microsoft SQL Server
- JWT Bearer authentication
- Swagger / OpenAPI
Database
The API uses a SQL Server database named RaceDay and contains six main tables:
Table	Purpose
USERS	Participant and organiser profiles, password hashes and roles.
EVENTTYPES	Event types such as Run, Walk and Cycle.
EVENTS	Event details and the organiser responsible.
CATEGORIES	Event categories and age ranges.
ENROLMENTS	Links participants to events and selected categories.
RESULTS	Finishing times and positions linked to enrolments.


The database includes relationships and constraints to help maintain data integrity, including unique email addresses, one enrolment per participant per event, and a result linked to an enrolment.
Prerequisites
- Visual Studio with the ASP.NET and web development workload.
- .NET 8 SDK.
- SQL Server or SQL Server Express.
- A RaceDay database with the required tables and relationships.
Configuration
Update appsettings.json to match your local SQL Server instance. Example:
{
  "ConnectionStrings": {
    "RaceDayConnection": "Server=YOUR_SERVER_NAME;Database=RaceDay;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Jwt": {
    "Key": "REPLACE_WITH_A_LONG_RANDOM_DEVELOPMENT_KEY",
    "Issuer": "RaceDayAPI",
    "Audience": "RaceDayUsers"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
Replace YOUR_SERVER_NAME with your SQL Server instance. Never commit real secrets or production keys to a public repository. For deployment, use environment variables or a secure secret store.
Run the API
1. Open the solution in Visual Studio.
2. Check the connection string and JWT settings in appsettings.json.
3. Ensure SQL Server is running and the RaceDay database is available.
4. Select Build → Build Solution.
5. Run the project with Ctrl + F5 or the Visual Studio run button.
6. Open the Swagger URL shown by Visual Studio, usually ending in /swagger.
Swagger lets you inspect and send requests to the API.
API Endpoints
This is the intended endpoint list. Confirm it against the final Swagger display before submission.
Users
Method	Route	Purpose
POST	/api/users/register	Register a Participant account.
POST	/api/users/login	Verify credentials and return a JWT.
GET	/api/users/me	View the signed-in user's profile.
PUT	/api/users/me	Update the signed-in user's profile.


Events
Method	Route	Purpose
GET	/api/events	View all events.
GET	/api/events/{id}	View one event.
POST	/api/events	Create an event (Organiser).
PUT	/api/events/{id}	Update an event owned by the Organiser.
DELETE	/api/events/{id}	Delete an event owned by the Organiser.


Event Types
Method	Route	Purpose
GET	/api/eventtypes	View available event types.


Categories
Method	Route	Purpose
GET	/api/events/{eventId}/categories	View categories for an event.
GET	/api/categories/{id}	View one category.
POST	/api/events/{eventId}/categories	Create a category (Organiser).
PUT	/api/categories/{id}	Update a category (owning Organiser).
DELETE	/api/categories/{id}	Delete a category (owning Organiser).


Enrolments
Method	Route	Purpose
POST	/api/events/{eventId}/enrolments	Enrol in an event using a category (Participant).
GET	/api/enrolments/me	View the signed-in Participant's enrolments.
GET	/api/events/{eventId}/enrolments	View enrolments for an event owned by the Organiser.


Results
Method	Route	Purpose
POST	/api/enrolments/{enrolmentId}/results	Record a result (owning Organiser).
GET	/api/results/me	View the signed-in Participant's results.
GET	/api/events/{eventId}/results	View results for an event owned by the Organiser.


Authentication in Swagger
1. Call POST /api/users/login with a registered account's email and password.
2. Copy the returned JWT token.
3. Click Authorize in Swagger.
4. Enter Bearer  followed by the token if the dialog expects the full header. If Swagger automatically adds the prefix, enter only the token.
5. Submit requests that require authentication.
A Participant token cannot access Organiser-only endpoints. Organiser endpoints also check that the signed-in organiser owns the event.
Suggested Test Checklist
- [ ] Participant registration returns 201 Created.
- [ ] Duplicate email registration is rejected.
- [ ] Login succeeds and returns a JWT.
- [ ] Incorrect login credentials are rejected.
- [ ] The signed-in user can view and update their profile.
- [ ] Events and event types can be viewed.
- [ ] Unauthenticated protected requests are rejected.
- [ ] Participants cannot access Organiser-only operations.
- [ ] Organisers cannot edit another organiser's event or categories.
- [ ] Participants can enrol using a category belonging to that event.
- [ ] Duplicate enrolment is rejected.
- [ ] Participants can view only their own enrolments and results.
- [ ] Organisers can view enrolments/results only for their own events.
- [ ] Invalid finishing times or positions are rejected.
- [ ] The project builds successfully.
Project Status
The API has been built and run locally, and its controller groups have appeared in Swagger during development. Verify each endpoint with real requests before describing it as fully tested.
Remaining Submission Checks
Check the assignment brief and complete any requirements not yet finished:
- Automated unit tests and test evidence.
- GitHub repository with source code and a clear commit history.
- GitHub Actions workflow for build/test continuous integration.
- README matched to the final implementation.
- Required screenshots, including CI/CD evidence.
- Required unlisted video demonstration.
- Final security review of configuration and organiser-account creation.
Author
Student project – RaceDay RESTful API.
