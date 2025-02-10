# Tool_Study

Repository for scratch code to study Tech Stacks for project

---

### Why New Repository?

25.02.10 I realize I definitely need to know how to use MUI, MUI-Toolpad-Core, Firebase, Next.js, Typescript, FastAPI, and Typescript for one of the College-Industry Related Project to build or come up with the solution. 

Initially I thought I could do the project using ChatGPT but that was not the case. Without the foundational knowledges to tools, I cannot review the code and check which part is right or wrong. I could not understand how each tools work so using tools without understanding required lots of extra time and effort and minimum understanding about the tools. 

Therefore, I decided to dedecate myself some time to follow official documentation for each tools and get a grasp of knowledges to use the tools wisely and effectively.

### Will this approach work?

Not sure, ChatGPT is not always correct but I have some understanding on web developement, so I will figure it out. Also, I am not worried about ChatGPT providing wrong response becasue I think I know which part is wrong in the response and ask question to clarify it. 

---

## Fullstack Project Setup Guide

This guide will walk you through setting up a fullstack project using Material UI (MUI), MUI Toolpad Core, Firebase, FastAPI, and Next.js. It includes steps for setting up VSCode, pyenv, and configuring both backend and frontend.

---

## **1. Setting Up VSCode**

### **Install VSCode Extensions:**
1. **Recommended Extensions:**
   - **ESLint**: For linting JavaScript/TypeScript code.
   - **Prettier - Code Formatter**: For consistent formatting.
   - **Python**: For FastAPI development.
   - **vscode-icons**: For better folder visuals.
   - **Material Icon Theme**: For UI improvements.
   - **Firebase Tools**: Optional, for Firebase integration.

### **Enable Useful Settings:**
- `Editor: Format on Save`: Enables auto-formatting.
- `Python: Linting`: Enable linting for Python files.

---

## **2. Setting Up the Project Structure**

```bash
mkdir my-fullstack-project
cd my-fullstack-project
mkdir backend frontend
```

---

## **3. Setting Up Backend (FastAPI + pyenv)**

### **Install pyenv**
1. Install `pyenv` by following the [installation guide](https://github.com/pyenv/pyenv#installation). After installation, run the following commands:
   ```bash
   pyenv install 3.10.6
   pyenv virtualenv 3.10.6 fastapi-env
   pyenv local fastapi-env
   ```

2. **Create a FastAPI Project:**
   ```bash
   cd backend
   pip install --upgrade pip
   pip install fastapi uvicorn pydantic
   pip freeze > requirements.txt
   ```

3. **Create a FastAPI Entry Point (`main.py`):**
   ```python
   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/")
   def read_root():
       return {"message": "Hello, World!"}
   ```

4. **Run FastAPI:**
   ```bash
   uvicorn main:app --reload
   ```
   Visit: `http://127.0.0.1:8000`.

---

## **4. Setting Up Frontend (Next.js + MUI + Firebase)**

### **Step 1: Initialize a Next.js App**

1. **Install Node.js:** Use [nvm](https://github.com/nvm-sh/nvm) to manage Node.js versions.
   ```bash
   nvm install --lts
   nvm use --lts
   ```

2. **Create a Next.js App:**
   ```bash
   cd ../frontend
   npx create-next-app@latest .
   ```

3. **Install Dependencies:**
   ```bash
   npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
   npm install firebase
   ```

### **Step 2: Connect Firebase**

1. **Create a Firebase Project:**
   - Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
   - Enable Firestore or Realtime Database.

2. **Add Firebase Config:**
   Create a file `firebase.js` in the `frontend` directory:
   ```javascript
   import { initializeApp } from "firebase/app";

   const firebaseConfig = {
       apiKey: "your-api-key",
       authDomain: "your-auth-domain",
       projectId: "your-project-id",
       storageBucket: "your-storage-bucket",
       messagingSenderId: "your-messaging-sender-id",
       appId: "your-app-id"
   };

   const app = initializeApp(firebaseConfig);
   export default app;
   ```

### **Step 3: Add MUI Components**

1. **Wrap `pages/_app.js` with MUI Theme Provider:**
   ```javascript
   import { ThemeProvider, createTheme } from '@mui/material/styles';
   import CssBaseline from '@mui/material/CssBaseline';

   const theme = createTheme();

   export default function MyApp({ Component, pageProps }) {
       return (
           <ThemeProvider theme={theme}>
               <CssBaseline />
               <Component {...pageProps} />
           </ThemeProvider>
       );
   }
   ```

2. **Add a Simple MUI Component in `pages/index.js`:**
   ```javascript
   import { Button, Typography } from '@mui/material';

   export default function Home() {
       return (
           <div>
               <Typography variant="h1">Hello, Material UI!</Typography>
               <Button variant="contained" color="primary">
                   Click Me
               </Button>
           </div>
       );
   }
   ```

---

## **5. Install and Use MUI Toolpad Core**

1. **Install the Toolpad CLI:**
   ```bash
   npm install @mui/toolpad-cli --save-dev
   ```

2. **Start Toolpad:**
   ```bash
   npx toolpad dev
   ```

3. **Access the Toolpad Editor:** Visit `http://localhost:3000/_toolpad`.

---

## **6. Connect Backend to Frontend**

1. **Update FastAPI to Allow CORS:** Install `fastapi-cors`:
   ```bash
   pip install fastapi[all] python-multipart
   ```

   Update `main.py`:
   ```python
   from fastapi.middleware.cors import CORSMiddleware

   app.add_middleware(
       CORSMiddleware,
       allow_origins=["http://localhost:3000"],
       allow_credentials=True,
       allow_methods=["*"],
       allow_headers=["*"],
   )
   ```

2. **Create an API Route in Frontend:**
   ```javascript
   export async function fetchData() {
       const res = await fetch("http://127.0.0.1:8000/");
       const data = await res.json();
       return data;
   }
   ```

3. **Use API Data in Frontend:**
   ```javascript
   import { useEffect, useState } from 'react';

   export default function Home() {
       const [message, setMessage] = useState("");

       useEffect(() => {
           fetch("http://127.0.0.1:8000/")
               .then(res => res.json())
               .then(data => setMessage(data.message));
       }, []);

       return <h1>{message}</h1>;
   }
   ```

---

## **7. Debugging in VSCode**

- **Debug FastAPI:** Add `.vscode/launch.json`:
   ```json
   {
       "version": "0.2.0",
       "configurations": [
           {
               "name": "Python: FastAPI",
               "type": "python",
               "request": "launch",
               "program": "${workspaceFolder}/backend/main.py",
               "console": "integratedTerminal",
               "args": ["--reload"]
           }
       ]
   }
   ```

- **Debug Frontend:** Run the Next.js app in dev mode:
   ```bash
   npm run dev
   ```

---

This guide should help you set up your fullstack project. Feel free to reach out if you need assistance with specific parts!
