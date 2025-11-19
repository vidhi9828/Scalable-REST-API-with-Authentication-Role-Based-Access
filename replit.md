# Backend Developer Assignment Platform

## Overview

A full-stack REST API platform built as a backend developer intern assignment. The application provides JWT-based authentication, role-based access control (User/Admin), and task management capabilities. Built with Express.js backend, React frontend using shadcn/ui components, and Drizzle ORM for database management.

The platform demonstrates core backend development competencies including secure authentication, RESTful API design, database schema design, and role-based permissions.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- React 18 with TypeScript
- Vite as build tool and development server
- Wouter for client-side routing (lightweight alternative to React Router)
- TanStack Query (React Query) for server state management
- shadcn/ui component library built on Radix UI primitives
- Tailwind CSS for styling with custom design tokens

**Design System:**
- Material Design approach for data-heavy interfaces
- Inter font family for optimal readability
- Custom color system using HSL values with CSS variables
- Consistent spacing using Tailwind's 2/4/6/8 unit scale
- Elevation system using opacity-based shadows

**State Management:**
- AuthContext for global authentication state
- TanStack Query for API data caching and synchronization
- Local component state using React hooks

**Component Organization:**
- Page components in `client/src/pages/`
- Reusable UI components in `client/src/components/`
- shadcn/ui primitives in `client/src/components/ui/`
- Shared types in `shared/schema.ts`

### Backend Architecture

**Technology Stack:**
- Express.js for HTTP server
- TypeScript for type safety
- JWT (jsonwebtoken) for authentication tokens
- bcryptjs for password hashing
- Drizzle ORM for database interactions

**API Design:**
- RESTful API following versioning pattern (`/api/v1`)
- JWT Bearer token authentication
- Zod schemas for request validation
- Centralized error handling middleware

**Authentication & Authorization:**
- JWT tokens stored in localStorage on client
- Password hashing using bcrypt (10 rounds)
- Role-based access control with "user" and "admin" roles
- Middleware chain: `authenticateToken` → `requireAdmin` for protected routes

**Storage Layer:**
- Abstract `IStorage` interface allows swapping implementations
- In-memory `MemStorage` implementation for development
- Database implementation uses Drizzle ORM with PostgreSQL schema
- UUID-based primary keys using `gen_random_uuid()`

**API Endpoints:**
- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/login` - User authentication
- `GET /api/v1/tasks` - List tasks (filtered by role)
- `GET /api/v1/tasks/:id` - Get single task
- `POST /api/v1/tasks` - Create task (authenticated users)
- `PUT /api/v1/tasks/:id` - Update task (owner or admin)
- `DELETE /api/v1/tasks/:id` - Delete task (owner or admin)

### Database Schema

**Users Table:**
- `id` (varchar, primary key, UUID)
- `username` (text, unique, not null)
- `password` (text, not null) - bcrypt hashed
- `role` (text, default: "user") - enum: "user" | "admin"

**Tasks Table:**
- `id` (varchar, primary key, UUID)
- `title` (text, not null)
- `description` (text, not null)
- `status` (text, default: "pending") - enum: "pending" | "completed"
- `userId` (varchar, not null) - foreign key reference
- `createdAt` (timestamp, default: now())

**Validation:**
- Zod schemas derived from Drizzle table definitions
- Client and server share validation schemas from `shared/schema.ts`
- Type-safe insertions and updates using TypeScript inference

### Build & Deployment

**Development Mode:**
- Vite dev server with HMR for frontend
- tsx for running TypeScript backend with hot reload
- Concurrent frontend/backend development

**Production Build:**
- Frontend: Vite builds to `dist/public`
- Backend: esbuild bundles server to `dist/index.js`
- Static file serving from Express in production

**Environment Configuration:**
- `NODE_ENV` for environment detection
- `DATABASE_URL` for PostgreSQL connection
- `JWT_SECRET` for token signing (defaults provided for development)

## External Dependencies

**Database:**
- PostgreSQL via Neon serverless driver (`@neondatabase/serverless`)
- Drizzle ORM for schema definition and query building
- Drizzle Kit for migrations

**UI Component Libraries:**
- Radix UI primitives for accessible component foundations
- shadcn/ui as pre-styled component collection
- Lucide React for icons

**API Documentation:**
- Swagger/OpenAPI via swagger-jsdoc and swagger-ui-express
- Interactive API docs available at `/api-docs`
- JSDoc comments in route files generate OpenAPI specs

**Development Tools:**
- Replit-specific plugins for error overlay and dev banner
- TypeScript for full-stack type safety
- Path aliases configured in tsconfig and vite config

**Authentication & Security:**
- jsonwebtoken for JWT token generation/verification
- bcryptjs for secure password hashing
- CORS middleware for cross-origin requests

**Form Handling:**
- react-hook-form with Zod resolvers for type-safe forms
- Shared validation schemas between client and server