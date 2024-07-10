[<- Back](overview.md)

#### 4. Configuration

**Firebase Setup (`app.config.ts`):**

1. **Initialize Firebase in your Angular Project:**

   Open your `app.config.ts` file and add the following configuration to initialize Firebase. Replace the placeholder values with your actual Firebase project configuration details.

   ```typescript
   import { initializeApp } from 'firebase/app';
   import { getFirestore } from 'firebase/firestore';
   import { getAuth } from 'firebase/auth';

   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_AUTH_DOMAIN",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_STORAGE_BUCKET",
     messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
     appId: "YOUR_APP_ID"
   };

   const app = initializeApp(firebaseConfig);
   const db = getFirestore(app);
   const auth = getAuth(app);

   export { db, auth };
   ```

2. **Environment Variables:**

   Set up environment variables for different environments (development and production). This allows you to switch between configurations easily.

   - **src/environments/environment.ts** (Development)

     ```typescript
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

   - **src/environments/environment.prod.ts** (Production)

     ```typescript
     export const environment = {
       production: true,
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

3. **Angular Fire Module Setup:**

   Import Angular Fire modules in your `app.config.ts` file to integrate Firebase services.

   ```typescript
   import { provideFirebaseApp, initializeApp } from '@angular/fire/app';
   import { provideFirestore, getFirestore } from '@angular/fire/firestore';
   import { provideAuth, getAuth } from '@angular/fire/auth';
   import { environment } from '../environments/environment';

   const firebaseConfig = environment.firebaseConfig;

   @NgModule({
     imports: [
       provideFirebaseApp(() => initializeApp(firebaseConfig)),
       provideFirestore(() => getFirestore()),
       provideAuth(() => getAuth())
     ]
   })
   export class FirebaseConfigModule {}
   ```

4. **Main File Configuration (`main.ts`):**

   Ensure that the application is bootstrapped correctly in the `main.ts` file.

   ```typescript
   import { bootstrapApplication } from '@angular/platform-browser';
   import { appConfig } from './app/app.config';
   import { AppComponent } from './app/app.component';

   bootstrapApplication(AppComponent, appConfig)
     .catch((err) => console.error(err));
   ```

