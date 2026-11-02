# DispatchIQ

## Project Description

DispatchIQ is a real-time emergency service coordination platform for managing incidents and dispatching response units. The application is designed for environments such as ambulance services, fire departments, campus emergency teams, or municipal response centers. Its purpose is to help multiple dispatchers and responders coordinate active incident responses in one shared system instead of relying on disconnected calls, notes, or spreadsheets.

A dispatcher can create an incident, assign a severity level and/or priority level, record a location, and dispatch one or more response units. Other authorized users connected to the same system will immediately see the new incident, the assigned units, and any updates to its status. As responders progress through their work, they can update statuses such as En Route, On Scene, Transporting, or Resolved, and those changes appear live to everyone.

The main feature that makes DispatchIQ useful is real-time synchronization. If one dispatcher assigns a unit to an emergency, all other connected dispatchers instantly see that assignment. If a responder marks themselves as On Scene, the incident dashboard updates immediately. The goal of the project is not to simulate real emergency communication infrastructure in full detail, but to build a web application with real-time coordination, multi-user collaboration, access control, and reliable state updates.

## Key Features for the Beta Version

- User accounts and authentication with registration, sign in, and sign out
- Authenticated users can create incidents with information such as title, type, description, location, priority level, and current status
- Authenticated users can create and manage response units, such as ambulances, fire trucks, or patrol teams, each with a name, type, and availability status
- Dispatchers can assign available units to incidents
- Assigned units will appear under the relevant incident, and their statuses can be updated by authorized users
  - For example, a responder may change their status from Available to En Route, then On Scene, and finally Available again once the incident is resolved.
- Support for real-time updated using WebSockets
  - When an incident is created, edited, assigned, or resolved, all connected users will see the change without refreshing the page. Likewise, unit status changes will be synchronized immediately.
- A basic dashboard view showing active incidents, available units, and assigned responders, along with persistent storage so all data remains after logout or page refresh.

The beta demonstration will show two or more users signed into separate browsers. One user creates an emergency incident and assigns a unit, while another user updates the responder status. All changes appear instantly on every connected client.

## Additional Features for the Final Version

- Live map view where incidents and units can be visualized geographically using markers. The map does not need full GPS accuracy and can use simulated or manually set locations, but it will provide a more visual and operational interface.
- Role-based permissions
  - Dispatchers will have authority to create incidents, assign units, and close incidents, w
  - Responders will be able to update only the units or incidents relevant to them
  - This helps make the system more realistic
- A priority queue system will be added so that urgent incidents are visually highlighted and sorted appropriately
- Warnings for dispatchers when attempting to assign a unit that is already busy or when multiple dispatchers attempt to allocate the same unit at the same time.
- Incident logs and activity history
  - Each incident will maintain a timeline of important actions, such as when it was created, when a unit was assigned, when the unit went on scene, and when the incident was resolved
  - This will help users understand how the response evolved over time.
- Live presence indicators to show which dispatchers are currently online and actively viewing the system
- IF TIME PERMITS, a lightweight messaging or note system attached to each incident so dispatchers can communicate contextual information in real time.

The final demonstration will show several users managing multiple incidents simultaneously, assigning units, updating statuses, observing map changes, and resolving emergencies while the interface remains synchronized for everyone.

## Technology Stack and Deployment

### Frontend

A single-page application using React, TypeScript, and Vite. This stack will allow us to create a responsive and interactive interface suitable for dashboards, incident panels, and live updates.

### Backend

Built with Node.js, Express, and TypeScript. It will expose a REST API for standard operations such as authentication, incident creation, unit management, and role-based access checks.

### Real-Time Communication

Implemented using Socket.IO. Socket.IO will be used to push live incident updates, unit status changes, dispatch assignments, presence information, and other shared events to connected users.

### Database

Persistent data will be stored in PostgreSQL, with Prisma used as the ORM for schema definition and database access. The database will store users, roles, incidents, units, assignments, and activity logs.

### Authentication

Authentication will support local email and password accounts, with passwords securely hashed before storage. GitHub OAuth will also be supported

### Sessions

Managed securely using cookies.

### Deployment

The application will be deployed on the course-provided virtual machine. Nginx can be used as a reverse proxy in front of the Node.js server, and PostgreSQL will run as the persistent database. The deployed version will include the frontend, backend, WebSocket communication, and database integration.

## Top Five Technical Challenges

### Handling concurrent dispatch actions safely

Two dispatchers might attempt to assign the same response unit to different incidents at nearly the same time. The system must prevent inconsistent results and ensure that a unit can only be assigned if it is still available at the moment the server accepts the request.

### Maintaining a reliable real-time shared state

Since multiple users may be updating incidents and units simultaneously, the interface must remain synchronized across clients without duplication, stale data, or missing updates. The server must remain authoritative, and the clients must reconcile incoming changes properly.

### Implementing role-based access control across both REST and WebSocket interactions.

It is not enough to restrict normal page actions; the system must also ensure that only authorized users can subscribe to sensitive incident updates or emit certain real-time events. This becomes more complicated when users have different roles such as dispatcher and responder.

### Building a live operational dashboard that remains clear and usable while many events happen at once

The frontend must visually organize active incidents, unit availability, assignments, and status changes in a way that is understandable under real-time conditions. This is a UI/UX challenge and a technical one.

### Supporting reconnection and state recovery

If a user temporarily loses connection while incidents continue to change, the system must restore a correct and current state once they reconnect. This requires careful synchronization between the live socket connection and the database-backed source of truth.

## Scope and Quality Priorities

The main priority of DispatchIQ is to focus on emergency incident management, unit dispatch, live updates, and access control instead of trying to simulate every aspect of a real 911 system.

Features such as real radio communication, external GIS services, automatic route optimization, or integration with actual emergency APIs are outside the planned scope. Keeping the scope focused will allow us to spend more time on the most important challenges: synchronization, concurrency, permissions, dashboard design, and reliability.

We will aim to complete a working end-to-end version early, then improve the map interface, permissions, logs, and conflict handling in the final stage.
