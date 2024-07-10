[<- Back](overview.md)

#### 7. UI Components

**Angular Standalone Components:**

1. **Creating a Standalone Component:**

   Create a new standalone component using Angular CLI.

   ```sh
   ng generate component example --standalone
   ```

   This will create a new component `ExampleComponent` with the following structure:

   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-example',
     templateUrl: './example.component.html',
     styleUrls: ['./example.component.scss'],
     standalone: true
   })
   export class ExampleComponent {}
   ```

2. **Using Standalone Components:**

   To use a standalone component, import it in the module where it will be used.

   ```typescript
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { AppComponent } from './app.component';
   import { ExampleComponent } from './example/example.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       ExampleComponent
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

**Using Angular Material for UI:**

1. **Install Angular Material:**

   Install Angular Material, Angular CDK, and Angular Animations.

   ```sh
   ng add @angular/material
   ```

2. **Import Material Modules:**

   Import the necessary Material modules in your application. Create a separate module for Material components if necessary.

   ```typescript
   import { NgModule } from '@angular/core';
   import { MatButtonModule } from '@angular/material/button';
   import { MatInputModule } from '@angular/material/input';
   import { MatCardModule } from '@angular/material/card';
   import { MatToolbarModule } from '@angular/material/toolbar';

   @NgModule({
     exports: [
       MatButtonModule,
       MatInputModule,
       MatCardModule,
       MatToolbarModule
     ]
   })
   export class MaterialModule { }
   ```

3. **Using Material Components:**

   Use Material components in your templates.

   ```html
   <mat-toolbar color="primary">
     <span>GT Gurukul</span>
   </mat-toolbar>

   <mat-card>
     <mat-card-title>Example Card</mat-card-title>
     <mat-card-content>
       <p>This is an example card using Angular Material.</p>
     </mat-card-content>
     <mat-card-actions>
       <button mat-button>Action</button>
     </mat-card-actions>
   </mat-card>
   ```

**Styling with SCSS:**

1. **Global Styles:**

   Define global styles in `styles.scss`.

   ```scss
   // styles.scss

   @import '~@angular/material/theming';
   @import 'src/theme.scss';

   // Include the common styles for Angular Material
   @include mat-core();

   // Define your custom theme
   $primary: mat-palette($mat-indigo);
   $accent: mat-palette($mat-pink, A200, A100, A400);
   $warn: mat-palette($mat-red);

   // Create the theme object
   $theme: mat-light-theme(
     (
       color: (
         primary: $primary,
         accent: $accent,
         warn: $warn
       )
     )
   );

   // Include the Angular Material theme styles
   @include angular-material-theme($theme);
   ```

2. **Component Styles:**

   Use SCSS in your component styles.

   ```scss
   /* example.component.scss */

   .example-card {
     margin: 20px;
     padding: 20px;
     background-color: #f0f0f0;

     mat-card-title {
       font-size: 1.5em;
       font-weight: bold;
     }

     mat-card-content {
       font-size: 1.2em;
     }

     mat-card-actions {
       display: flex;
       justify-content: flex-end;
     }
   }
   ```

3. **Theming:**

   Define custom themes and use them in your application.

   ```scss
   // theme.scss

   @import '~@angular/material/theming';

   // Define your custom theme
   $primary: mat-palette($mat-indigo);
   $accent: mat-palette($mat-pink, A200, A100, A400);
   $warn: mat-palette($mat-red);

   $theme: mat-light-theme(
     (
       color: (
         primary: $primary,
         accent: $accent,
         warn: $warn
       )
     )
   );

   // Include the theme styles
   @include angular-material-theme($theme);
   ```
