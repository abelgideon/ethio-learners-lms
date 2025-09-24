# GEMINI.md

## Project Overview

This is a Learning Management System (LMS) built with Next.js, a React framework. The project uses a comprehensive stack of modern technologies to provide a robust and feature-rich platform for online learning.

**Main Technologies:**

*   **Framework:** [Next.js](https://nextjs.org/) (with Turbopack)
*   **Language:** [TypeScript](https://www.typescriptlang.org/)
*   **Database ORM:** [Prisma](https://www.prisma.io/)
*   **Database:** [PostgreSQL](https://www.postgresql.org/)
*   **Authentication:** [better-auth](https://www.npmjs.com/package/better-auth)
*   **UI Components:** [Radix UI](https://www.radix-ui.com/) and [shadcn/ui](https://ui.shadcn.com/)
*   **Styling:** [Tailwind CSS](https://tailwindcss.com/)
*   **Form Management:** [React Hook Form](https://react-hook-form.com/) and [Zod](https://zod.dev/) for validation
*   **File Storage:** [Amazon S3](https://aws.amazon.com/s3/) (or a compatible service)
*   **Rich Text Editing:** [Tiptap](https://tiptap.dev/)
*   **Email:** [Resend](https://resend.com/)

**Architecture:**

The application follows a standard Next.js project structure with the App Router. Key architectural features include:

*   **API Routes:** Backend logic is handled by API routes within the `app/api` directory.
*   **Database Schema:** The database schema is defined in `prisma/schema.prisma` and includes models for Users, Courses, Chapters, and Lessons.
*   **Authentication:** User authentication and session management are handled by the `better-auth` library.
*   **File Uploads:** The application is configured to upload files to an S3-compatible object storage service.
*   **Component-Based UI:** The user interface is built with React components, organized in the `components` directory.

## Building and Running

To get the project up and running, follow these steps:

1.  **Install Dependencies:**
    ```bash
    pnpm install
    ```

2.  **Set up Environment Variables:**
    Create a `.env` file in the root of the project and add the necessary environment variables, including the `DATABASE_URL`.

3.  **Run Database Migrations:**
    ```bash
    npx prisma migrate dev
    ```

4.  **Run the Development Server:**
    ```bash
    pnpm dev
    ```
    The application will be available at `http://localhost:3000`.

**Other Key Commands:**

*   **Build for Production:**
    ```bash
    pnpm build
    ```

*   **Start Production Server:**
    ```bash
    pnpm start
    ```

*   **Lint the Codebase:**
    ```bash
    pnpm lint
    ```

## Development Conventions

*   **Styling:** The project uses Tailwind CSS for styling. Utility classes should be used whenever possible.
*   **Components:** Reusable UI components are located in the `components` directory.
*   **State Management:** For complex state management, consider using a library like Zustand or React Context.
*   **API Routes:** Backend logic should be placed in API routes within the `app/api` directory.
*   **Database:** All database interactions should be done through the Prisma client.
*   **Validation:** Use Zod for data validation on both the client and server.
