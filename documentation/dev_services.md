[<- Back](overview.md)

#### 8. Services

**Overview of Services:**

Services in Angular are used to share data and functionality across components. They help keep your components clean and focused on presentation logic. Below are some key services implemented in the GT Gurukul application:

1. **AuthService:**
   Handles user authentication and authorization using Firebase.

2. **FirestoreService:**
   Manages Firestore operations like adding, retrieving, updating, and deleting documents.

3. **SyllabusService:**
   Fetches syllabus-related data such as boards, standards, and subjects from Firestore.

**Example Usage:**

1. **AuthService:**

   **auth.service.ts:**

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

   **Using AuthService in a Component:**

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

2. **FirestoreService:**

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
       const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
       await setDoc(docRef, data, { merge: true });
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
       const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
       await updateDoc(docRef, data);
     }

     // Delete Document
     async deleteDocument(collectionName: string, documentId: string): Promise<void> {
       const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
       await deleteDoc(docRef);
     }
   }
   ```

   **Using FirestoreService in a Component:**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { FirestoreService } from './services/firestore.service';
   import { Observable } from 'rxjs';

   @Component({
     selector: 'app-example',
     template: `
       <div *ngFor="let item of items$ | async">
         {{ item.name }}
       </div>
       <button (click)="addItem()">Add Item</button>
     `
   })
   export class ExampleComponent implements OnInit {
     items$: Observable<any[]>;

     constructor(private firestoreService: FirestoreService) {}

     ngOnInit() {
       this.items$ = this.firestoreService.getCollection('items');
     }

     addItem() {
       const newItem = { name: 'New Item' };
       this.firestoreService.setDocument('items', 'item-id', newItem);
     }
   }
   ```

3. **SyllabusService:**

   **syllabus.service.ts:**

   ```typescript
   import { Injectable } from '@angular/core';
   import { FirestoreService } from './firestore.service';
   import { Observable } from 'rxjs';

   @Injectable({
     providedIn: 'root'
   })
   export class SyllabusService {
     constructor(private firestoreService: FirestoreService) {}

     // Get Boards
     getBoards(): Observable<any[]> {
       return this.firestoreService.getCollection('boards');
     }

     // Get Standards
     getStandards(): Observable<any[]> {
       return this.firestoreService.getCollection('standards');
     }

     // Get Subjects
     getSubjects(): Observable<any[]> {
       return this.firestoreService.getCollection('subjects');
     }
   }
   ```

   **Using SyllabusService in a Component:**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { SyllabusService } from './services/syllabus.service';
   import { Observable } from 'rxjs';

   @Component({
     selector: 'app-syllabus',
     template: `
       <div>
         <h3>Boards</h3>
         <ul>
           <li *ngFor="let board of boards$ | async">{{ board.name }}</li>
         </ul>
       </div>
       <div>
         <h3>Standards</h3>
         <ul>
           <li *ngFor="let standard of standards$ | async">{{ standard.name }}</li>
         </ul>
       </div>
       <div>
         <h3>Subjects</h3>
         <ul>
           <li *ngFor="let subject of subjects$ | async">{{ subject.name }}</li>
         </ul>
       </div>
     `
   })
   export class SyllabusComponent implements OnInit {
     boards$: Observable<any[]>;
     standards$: Observable<any[]>;
     subjects$: Observable<any[]>;

     constructor(private syllabusService: SyllabusService) {}

     ngOnInit() {
       this.boards$ = this.syllabusService.getBoards();
       this.standards$ = this.syllabusService.getStandards();
       this.subjects$ = this.syllabusService.getSubjects();
     }
   }
   ```
