[<- Back](overview.md)

#### 5. Authentication

**Google Sign-In and Other Authentication Methods:**

1. **Setting Up Firebase Authentication:**

   Ensure that Firebase Authentication is enabled in your Firebase console. Go to the Firebase Console > Authentication > Sign-in method and enable Google.

2. **Authentication Service (`auth.service.ts`):**

   Create an authentication service to handle the sign-in and sign-out processes.

   ```typescript
   import { Injectable } from '@angular/core';
   import { GoogleAuthProvider, signInWithPopup, signOut } from 'firebase/auth';
   import { auth } from '../app.config'; // Ensure you import the auth instance from your configuration file

   @Injectable({
     providedIn: 'root'
   })
   export class AuthService {
     constructor() {}

     // Google Sign-In
     async googleSignIn(): Promise<void> {
       const provider = new GoogleAuthProvider();
       try {
         await signInWithPopup(auth, provider);
       } catch (error) {
         console.error('Error during Google sign-in', error);
       }
     }

     // Sign-Out
     async signOut(): Promise<void> {
       try {
         await signOut(auth);
       } catch (error) {
         console.error('Error during sign-out', error);
       }
     }

     // Get Current User
     getCurrentUser() {
       return auth.currentUser;
     }
   }
   ```

3. **Component Integration:**

   Use the `AuthService` in your components to handle user authentication.

   ```typescript
   import { Component } from '@angular/core';
   import { AuthService } from './services/auth.service';

   @Component({
     selector: 'app-login',
     template: `
       <button (click)="login()">Login with Google</button>
       <button (click)="logout()">Logout</button>
     `
   })
   export class LoginComponent {
     constructor(private authService: AuthService) {}

     login() {
       this.authService.googleSignIn();
     }

     logout() {
       this.authService.signOut();
     }
   }
   ```

4. **Protecting Routes with Guards (`auth.guard.ts`):**

   Create a guard to protect routes that require authentication.

   ```typescript
   import { Injectable } from '@angular/core';
   import { CanActivate, Router } from '@angular/router';
   import { AuthService } from './services/auth.service';

   @Injectable({
     providedIn: 'root'
   })
   export class AuthGuard implements CanActivate {
     constructor(private authService: AuthService, private router: Router) {}

     canActivate(): boolean {
       const user = this.authService.getCurrentUser();
       if (user) {
         return true;
       } else {
         this.router.navigate(['/login']);
         return false;
       }
     }
   }
   ```

5. **Adding Guard to Routes (`app.routes.ts`):**

   Apply the `AuthGuard` to routes that should be protected.

   ```typescript
   import { Routes } from '@angular/router';
   import { LoginComponent } from './login/login.component';
   import { DashboardComponent } from './dashboard/dashboard.component';
   import { AuthGuard } from './services/auth.guard';

   const routes: Routes = [
     { path: 'login', component: LoginComponent },
     { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
     { path: '', redirectTo: '/login', pathMatch: 'full' }
   ];
   ```
