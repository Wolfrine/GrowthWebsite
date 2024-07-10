[<- Back](overview.md)

#### 6. Firestore Integration

**Storing and Retrieving Data:**

1. **Firestore Service (`firestore.service.ts`):**

   Create a service to handle Firestore operations.

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

2. **Component Integration:**

   Use the `FirestoreService` in your components to interact with Firestore.

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

3. **Collection and Document Structure:**

   - **Collection:** A collection is a container for documents. You can think of it as a table in a relational database.
   - **Document:** A document is a record in a collection, similar to a row in a relational database. Documents can contain subcollections.

   Example structure:
   ```
   registrations/
     ├── docId1/
     │   ├── subdomain: "subdomain1"
     │   └── ...
     ├── docId2/
     │   ├── subdomain: "subdomain2"
     │   └── ...
   institutes/
     ├── instituteId1/
     │   ├── name: "Institute Name"
     │   └── ...
     ├── instituteId2/
     │   ├── name: "Another Institute"
     │   └── ...
   ```

4. **Handling Errors:**

   Ensure to handle errors during Firestore operations.

   ```typescript
   async setDocument(collectionName: string, documentId: string, data: any): Promise<void> {
     try {
       const docRef = doc(this.firestore, `${collectionName}/${documentId}`);
       await setDoc(docRef, data, { merge: true });
     } catch (error) {
       console.error('Error setting document:', error);
     }
   }
   ```
