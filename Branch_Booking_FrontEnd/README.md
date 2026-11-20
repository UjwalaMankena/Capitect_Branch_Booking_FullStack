
# Booking Frontend

An Angular-based banking appointment booking application that allows customers to authenticate, select banking services, and schedule appointments at their preferred bank branch.

## Technology Stack

| Technology | Version |
|---|---|
| Angular | 20.3.35 |
| Angular CLI | 20.3.43 |
| TypeScript | ~5.8.0 |
| RxJS | ~7.8.1 |
| Bootstrap | 5.3.3 |
| Bootstrap Icons | 1.11.3 |
| ng-bootstrap | 19.0.1 |
| Node.js | Compatible with Angular 20 |
| npm | Compatible with Node.js |

## Prerequisites

Ensure the following are installed:

- Node.js (Angular 20 compatible version)
- npm
- Angular CLI 20
- Java Spring Boot backend (for API integration)

Install Angular CLI:

```bash
npm install -g @angular/cli@20.3.43
```

Verify installations:

```bash
node -v
npm -v
ng version
```

## Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
```

### 2. Navigate to Project

```bash
cd booking-frontend
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Start Development Server

```bash
npm start
```

Alternatively:

```bash
ng serve
```

Open the application:

http://localhost:4200/

## Available NPM Commands

| Command | Description |
|---|---|
| `npm start` | Starts Angular development server |
| `npm run build` | Creates production build |
| `npm run watch` | Builds continuously in development mode |
| `npm run ng` | Executes Angular CLI commands |

### Production Build

```bash
npm run build
```

Build output is generated under:

```text
dist/
```

## Application Architecture

The application uses Angular services to separate API communication and business logic from UI components.

```text
src/
 └── app/
     ├── services/
     │   ├── auth.service.ts
     │   └── booking.service.ts
     │
     ├── components/
     │   ├── login/
     │   └── booking/
     │
     ├── app.component.ts
     ├── app.config.ts
     └── app.routes.ts
```

## Authentication

The application uses JWT-based authentication.

### Login API

```http
POST /api/auth/login
```

Request:

```json
{
  "email": "customer@example.com",
  "password": "password"
}
```

The authentication service stores the JWT token and username in browser local storage.

Storage keys:

```text
access_token
username
```

The token is used for authenticated API requests.

### Authorization Header

Protected API requests use:

```http
Authorization: Bearer <JWT_TOKEN>
```

## Appointment Booking

Customers can schedule appointments by selecting:

- Banking service / purpose
- Preferred branch
- Appointment date
- Appointment time
- Customer contact details

### Create Appointment API

```http
POST /api/appointments
```

Authorization:

```http
Authorization: Bearer <JWT_TOKEN>
```

Content-Type:

```http
Content-Type: application/json
```

The booking service handles API communication and returns the appointment confirmation or an error.

## API Configuration

The frontend communicates with the Spring Boot backend through an Angular development proxy.

### proxy.conf.json

Create this file in the project root:

```json
{
  "/api": {
    "target": "http://localhost:8080",
    "secure": false,
    "changeOrigin": true,
    "logLevel": "debug"
  }
}
```

Configure the Angular development server to use the proxy:

```json
"serve": {
  "options": {
    "proxyConfig": "proxy.conf.json"
  }
}
```

Frontend API URLs should use relative paths:

```typescript
private apiUrl = '/api/auth';
```

```typescript
private apiUrl = '/api/appointments';
```

The Angular proxy forwards these requests to:

```text
http://localhost:8080
```

**Note:** The Angular development proxy is intended for local development. Production deployments require an appropriate reverse proxy or backend CORS configuration.

## Error Handling

The application handles common HTTP errors:

| HTTP Status | Description |
|---|---|
| 0 | Network or connectivity issue |
| 400 | Invalid request |
| 401 | Unauthorized / Invalid JWT |
| 403 | Forbidden |
| 404 | API endpoint not found |
| 500 | Internal server error |

Users receive an appropriate error message when API requests fail.

## Running the Application

Ensure the following services are running:

| Application | URL |
|---|---|
| Angular Frontend | http://localhost:4200 |
| Spring Boot Backend | http://localhost:8080 |

Start the backend first, then start the Angular application:

```bash
npm start
```

## Troubleshooting

### 1. CORS Error

If you encounter CORS errors:

- Verify that `proxy.conf.json` exists.
- Verify that Angular is configured to use the proxy.
- Restart the Angular development server.
- Ensure the Spring Boot backend is running.

### 2. 401 Unauthorized

- Verify that the user is logged in.
- Check whether `access_token` exists in local storage.
- Verify that the JWT is being sent in the Authorization header.
- Check token expiration.

### 3. API Connection Failed

Verify that the Spring Boot backend is accessible:

```text
http://localhost:8080
```

Check the browser Network tab and backend logs for additional details.

## Security Considerations

- Do not hardcode credentials or JWT secrets in Angular.
- Use HTTPS in production.
- Validate JWT tokens on the backend.
- Protect sensitive APIs using Spring Security.
- Avoid storing sensitive customer information in local storage.
- Consider a more secure token storage strategy for production.

## Development Notes

- Angular services manage API communication.
- Reactive Forms handle customer input and validation.
- JWT authentication protects secured API endpoints.
- Angular signals maintain authentication state.
- RxJS handles asynchronous HTTP operations.

---

**Project:** Booking Frontend  
**Framework:** Angular 20  
**Backend:** Spring Boot REST API  
**Authentication:** JWT
