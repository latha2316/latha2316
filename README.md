npx create-react-app react-firebase-table
cd react-firebase-table
// firebase.js
import firebase from "firebase/app";
import "firebase/firestore";
import "firebase/auth";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID",
};

firebase.initializeApp(firebaseConfig);

export const auth = firebase.auth();
export const firestore = firebase.firestore();
// Table.js
import React, { useEffect, useState } from "react";
import { firestore } from "./firebase";

const Table = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    const fetchData = async () => {
      const snapshot = await firestore.collection("your_collection").get();
      const items = snapshot.docs.map((doc) => ({
        id: doc.id,
        ...doc.data(),
      }));
      setData(items);
    };

    fetchData();
  }, []);

  return (
    <div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            {/* Add other columns as needed */}
          </tr>
        </thead>
        <tbody>
          {data.map((item) => (
            <tr key={item.id}>
              <td>{item.id}</td>
              <td>{item.name}</td>
              {/* Add other cells as needed */}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default Table;
// App.js
import React from "react";
import Table from "./Table";

function App() {
  return (
    <div>
      <h1>Firebase React Table</h1>
      <Table />
    </div>
  );
}

export default App;
npm start
npm install -g firebase-tools
firebase login
firebase init
# Choose Hosting, select your Firebase project, set public directory to "build", configure as a single-page app
npm run build
firebase deploy
ng new firebase-angular-table
cd firebase-angular-table
ng add @angular/fire
export const environment = {
  production: false,
  firebaseConfig: {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID",
  },
};
// table.component.ts
import { Component, OnInit } from '@angular/core';
import { AngularFirestore } from '@angular/fire/firestore';

@Component({
  selector: 'app-table',
  template: `
    <div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <!-- Add other columns as needed -->
          </tr>
        </thead>
        <tbody>
          <tr *ngFor="let item of data">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <!-- Add other cells as needed -->
          </tr>
        </tbody>
      </table>
    </div>
  `,
})
export class TableComponent implements OnInit {
  data: any[] = [];

  constructor(private firestore: AngularFirestore) {}

  ngOnInit(): void {
    this.fetchData();
  }

  fetchData(): void {
    this.firestore
      .collection('your_collection')
      .get()
      .subscribe((querySnapshot) => {
        this.data = querySnapshot.docs.map((doc) => ({
          id: doc.id,
          ...doc.data(),
        }));
      });
  }
}
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h1>Firebase Angular Table</h1>
    <app-table></app-table>
  `,
})
export class AppComponent {}
ng serve
ng build --prod
firebase login
firebase init
# Choose Hosting, select your Firebase project, set public directory to "dist/firebase-angular-table"
firebase deploy
