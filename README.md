This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Tech Stack
Here is the Tech Stack used to create this project:
- Frontend: Next.js, React, Tailwind CSS, Typescript, JavaScript
- Backend: node.js, MongoDB, Prisma
- Other tools used: ClerkJS (User Authentication), Axios (Fetching Data from Database), Styled Components (Quick Styling), React-Toastify (Creating Notifications)

## Features
Here are some of the special features included in the web app: <br />
### User Authetication
To access the app, users must first sign up using their email to create an account. Following this, they can access their own personalized task manager and can sign in/sign out whenever they want. This was implemented using the Clerk, a cloud-based authetication service that is intergrated into Next.js

```javascript
import { ClerkProvider} from "@clerk/nextjs";
import { auth } from "@clerk/nextjs/server";
```
```html
<ClerkProvider>
      <html lang="en">
        <head>
          <link
            rel="stylesheet"
            href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css"
            integrity="sha512-z3gLpd7yknf1YoNbCzqRKc4qyor8gaKU1qmn+CShxbuBusANI9QpRohGBreCFkKxLhei6S9CQXFEbbKuqLg0DA=="
            crossOrigin="anonymous"
            referrerPolicy="no-referrer"
          />
        </head>
        <body className={nunito.className}>
          <NextTopLoader
            height={2}
            color="#27AE60"
            easing="cubic-bezier(0.53,0.21,0,1)"
          />
          <ContextProvider>
            <GlobalStyleProvider>
              {userId && <Sidebar />}
              <div className="w-full">{children}</div>
            </GlobalStyleProvider>
          </ContextProvider>
        </body>
      </html>
    </ClerkProvider>
```
### Pop-up Task Creation UI
Another feature of the app is the user's ability to create tasks using a pop-up modal, which can be seen in the CreateContent and Modal component <br />
Implementation:
```javascript
import CreateContent from "../Modals/CreateContent";
import Modal from "../Modals/Modal";
```
```javascript
const { theme, isLoading, openModal, modal } = useGlobalState();
{modal && <Modal content={<CreateContent />} />}
```
## Task Management Options
Users are able to management their tasks thanks to the Axios, a promised-based HTTP client that works with the Prisma client to management task data in the MongoDB database <br /> 
Implementation for Deleting a Task: 
```javascript
const deleteTask = async (id) => {
    try {
      const res = await axios.delete(`/api/tasks/${id}`);
      toast.success("Task deleted");

      allTasks();
    } catch (error) {
      console.log(error);
      toast.error("Something went wrong");
    }
  };
```
```javascript
export async function DELETE(
  req: Request,
  { params }: { params: { id: string } }
) {
  try {
    const { userId } = auth();
    const { id } = params;

    if (!userId) {
      return new NextResponse("Unauthorized", { status: 401 });
    }

    const task = await prisma.task.delete({
      where: {
        id,
      },
    });

    return NextResponse.json(task);
  } catch (error) {
    console.log("ERROR DELETING TASK: ", error);
    return NextResponse.json({ error: "Error deleting task", status: 500 });
  }
}
```
## To Host Your Own Web App
Make sure that you have a Clerk and MongoDB account and have installed all of the necessary packagaes (tech stack stated above). If I missed something, look through the imports in each code file to make sure that you have installed what you want to import. 

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.
