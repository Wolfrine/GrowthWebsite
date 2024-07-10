[<- Back](overview.md)

#### 9. Routing

**Overview of Routes:**

1. **Defining Routes (`app.routes.ts`):**

   In your Angular application, define routes in a dedicated routing file. This file will contain all the route configurations for your application.

   ```typescript
   import { Routes } from '@angular/router';
   import { LoginComponent } from './login/login.component';
   import { DashboardComponent } from './dashboard/dashboard.component';
   import { RegistrationComponent } from './registration/registration.component';
   import { AuthGuard } from './services/auth.guard';

   const routes: Routes = [
     { path: 'login', component: LoginComponent },
     { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
     { path: 'register/new', component: RegistrationComponent },
     { path: '', redirectTo: '/login', pathMatch: 'full' }
   ];

   export default routes;
   ```

2. **Configuring the App (`main.ts`):**

   Ensure that your routes are configured in the main application bootstrap file.

   ```typescript
   import { bootstrapApplication } from '@angular/platform-browser';
   import { provideRouter } from '@angular/router';
   import { AppComponent } from './app/app.component';
   import routes from './app/app.routes';

   bootstrapApplication(AppComponent, {
     providers: [provideRouter(routes)]
   }).catch(err => console.error(err));
   ```

3. **Route Guards:**

   Use route guards to protect certain routes. For example, the `AuthGuard` ensures that only authenticated users can access the dashboard.

   **auth.guard.ts:**

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

4. **Navigating Between Routes:**

   Use the Angular Router to navigate between routes programmatically.

   **navigation.component.ts:**

   ```typescript
   import { Component } from '@angular/core';
   import { Router } from '@angular/router';

   @Component({
     selector: 'app-navigation',
     template: `
       <button (click)="goToDashboard()">Go to Dashboard</button>
       <button (click)="goToRegister()">Register</button>
     `
   })
   export class NavigationComponent {
     constructor(private router: Router) {}

     goToDashboard() {
       this.router.navigate(['/dashboard']);
     }

     goToRegister() {
       this.router.navigate(['/register/new']);
     }
   }
   ```

5. **Route Parameters:**

   Access route parameters using the ActivatedRoute service.

   **example.component.ts:**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { ActivatedRoute } from '@angular/router';

   @Component({
     selector: 'app-example',
     template: `
       <p>Parameter ID: {{ id }}</p>
     `
   })
   export class ExampleComponent implements OnInit {
     id: string | null = null;

     constructor(private route: ActivatedRoute) {}

     ngOnInit() {
       this.id = this.route.snapshot.paramMap.get('id');
     }
   }
   ```

6. **Lazy Loading Modules:**

   Implement lazy loading for feature modules to improve performance.

   **app.routes.ts:**

   ```typescript
   const routes: Routes = [
     { path: 'login', component: LoginComponent },
     { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
     { path: 'register/new', component: RegistrationComponent },
     { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) },
     { path: '', redirectTo: '/login', pathMatch: 'full' }
   ];
   ```
