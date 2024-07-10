[<- Back](overview.md)

#### 10. Error Handling

**Error Handling Strategies:**

1. **Global Error Handler:**

   Create a global error handler to catch and handle errors throughout the application. This can be done by implementing `ErrorHandler`.

   **global-error-handler.ts:**

   ```typescript
   import { ErrorHandler, Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root'
   })
   export class GlobalErrorHandler implements ErrorHandler {
     handleError(error: any): void {
       // Log the error to the console
       console.error('An error occurred:', error);
       // You can also send the error to a remote logging server if required
     }
   }
   ```

   Register the global error handler in your main application module.

   **main.ts:**

   ```typescript
   import { bootstrapApplication } from '@angular/platform-browser';
   import { provideRouter } from '@angular/router';
   import { AppComponent } from './app/app.component';
   import routes from './app/app.routes';
   import { GlobalErrorHandler } from './app/global-error-handler';
   import { ErrorHandler } from '@angular/core';

   bootstrapApplication(AppComponent, {
     providers: [
       provideRouter(routes),
       { provide: ErrorHandler, useClass: GlobalErrorHandler }
     ]
   }).catch(err => console.error(err));
   ```

2. **Service-Level Error Handling:**

   Handle errors at the service level using `try-catch` blocks and error logging.

   **firestore.service.ts:**

   ```typescript
   import { Injectable } from '@angular/core';
   import { Firestore, collection, doc, setDoc, getDoc, updateDoc, deleteDoc } from '@angular/fire/firestore';
   import { Observable } from 'rxjs';
   import { collectionData, docData } from 'rxfire/firestore';

   @Injectable({
     providedIn: 'root'
   })
   export class FirestoreService {
     constructor(private firestore: Firestore) {}

     // Add or Update Document
     async setDocument(collectionName: string, documentId: string, data: any): Promise<void> {
       try {
         const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
         await setDoc(docRef, data, { merge: true });
       } catch (error) {
         console.error('Error setting document:', error);
       }
     }

     // Get Document
     getDocument(collectionName: string, documentId: string): Observable<any> {
       const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
       return docData(docRef, { idField: 'id' });
     }

     // Get Collection
     getCollection(collectionName: string): Observable<any[]> {
       const collectionRef = collection(this.firestore, collectionName);
       return collectionData(collectionRef, { idField: 'id' });
     }

     // Update Document
     async updateDocument(collectionName: string, documentId: string, data: any): Promise<void> {
       try {
         const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
         await updateDoc(docRef, data);
       } catch (error) {
         console.error('Error updating document:', error);
       }
     }

     // Delete Document
     async deleteDocument(collectionName: string, documentId: string): Promise<void> {
       try {
         const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
         await deleteDoc(docRef);
       } catch (error) {
         console.error('Error deleting document:', error);
       }
     }
   }
   ```

3. **Component-Level Error Handling:**

   Handle errors at the component level to provide feedback to the user.

   **example.component.ts:**

   ```typescript
   import { Component } from '@angular/core';
   import { FirestoreService } from './services/firestore.service';

   @Component({
     selector: 'app-example',
     template: `
       <div *ngIf="error">{{ error }}</div>
       <div *ngFor="let item of items">
         {{ item.name }}
       </div>
       <button (click)="addItem()">Add Item</button>
     `
   })
   export class ExampleComponent {
     items: any[] = [];
     error: string | null = null;

     constructor(private firestoreService: FirestoreService) {}

     ngOnInit() {
       this.firestoreService.getCollection('items').subscribe(
         data => {
           this.items = data;
           this.error = null;
         },
         error => {
           this.error = 'Failed to load items.';
           console.error('Error loading items:', error);
         }
       );
     }

     addItem() {
       const newItem = { name: 'New Item' };
       this.firestoreService.setDocument('items', 'item-id', newItem).catch(error => {
         this.error = 'Failed to add item.';
         console.error('Error adding item:', error);
       });
     }
   }
   ```

4. **HTTP Error Handling:**

   Handle HTTP errors using `HttpInterceptor`.

   **http-error.interceptor.ts:**

   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest, HttpErrorResponse } from '@angular/common/http';
   import { Observable, throwError } from 'rxjs';
   import { catchError } from 'rxjs/operators';

   @Injectable()
   export class HttpErrorInterceptor implements HttpInterceptor {
     intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
       return next.handle(req).pipe(
         catchError((error: HttpErrorResponse) => {
           let errorMessage = '';
           if (error.error instanceof ErrorEvent) {
             // Client-side error
             errorMessage = `Error: ${error.error.message}`;
           } else {
             // Server-side error
             errorMessage = `Error Code: ${error.status}\nMessage: ${error.message}`;
           }
           console.error(errorMessage);
           return throwError(errorMessage);
         })
       );
     }
   }
   ```

   Register the interceptor in your main application module.

   **main.ts:**

   ```typescript
   import { bootstrapApplication } from '@angular/platform-browser';
   import { provideRouter } from '@angular/router';
   import { AppComponent } from './app/app.component';
   import routes from './app/app.routes';
   import { GlobalErrorHandler } from './app/global-error-handler';
   import { HTTP_INTERCEPTORS } from '@angular/common/http';
   import { HttpErrorInterceptor } from './app/http-error.interceptor';

   bootstrapApplication(AppComponent, {
     providers: [
       provideRouter(routes),
       { provide: ErrorHandler, useClass: GlobalErrorHandler },
       { provide: HTTP_INTERCEPTORS, useClass: HttpErrorInterceptor, multi: true }
     ]
   }).catch(err => console.error(err));
   ```
