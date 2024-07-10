[<- Back](overview.md)

## 2. Getting Started

**Prerequisites:**

Before setting up the project, ensure you have the following installed:

- **Node.js** (version 14.x or higher)
- **Angular CLI** (version 12.x or higher)
- **Firebase CLI**
- **Docker** (for deployment)
- **Git** (for version control)

**Setup Instructions:**

1. **Clone the Repository:**
   ```sh
   git clone https://github.com/your-repo/gt-gurukul.git
   cd gt-gurukul
   ```

2. **Install Dependencies:**
   ```sh
   npm install
   ```
3. **Setup Firebase:**
- Install Firebase CLI if not already installed:  
  ```sh
  npm install -g firebase-tools
  ```
- Initialize Firebase in your project:
  ```sh
  firebase init
  ```

- Select Firestore, Functions, and Hosting during the setup.

4. **Configure Firebase:**
- Open src/environments/environment.ts and add your Firebase project configuration:
  ```ts
  export const environment = {
    production: false,
    firebaseConfig: {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_STORAGE_BUCKET",
      messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
      appId: "YOUR_APP_ID"
    }
  };
  ```

5. **Run the Application:**
   ```sh
   ng serve
  ```
The application should now be running on http://localhost:4200.
