---
title:  "Angular 7 - Observables Tutorial for Components Communication"
categories: 
  - Javascript
tags:
  - Angular
comments: true
---

The best way to communicate between angular components is to use an **Observables**. I won't go too much into the details about how observables work here since it's a big subject. So will explain in simple terms and dive directly with the code. Before starting i hope you know what is components in angular, if not, this [article](https://www.toptal.com/angular/angular-components-overview-101) can help you to get started.

# Observables

Observables are lazy collections of muliple values over time. Let's take it in more simple way..

## Observables are lazy

You could think of observables as newsletters. For each subscriber a new newsletter is created. They are then only send to those people, and not to anyone else.

## Observables can have multiple values over time

Now if you keep that subscription to the newsletter open, you will get a new one every once and a while. The sender decides when you get it but all you have to do is just wait until it comes straight into your inbox.

# Lets Create an Observable

If you start using Angular you will probably encounter observables when setting up your HTTP requests. So let’s start there.

Below is an example service, which pass students data to the subscribers through observables. Here, instead making a Http call, i'm using local data to send.

## Creating observables
Creating observables is easy, just call the new Observable() and pass along one argument which represents the observer. Therefore i usually call it **observer** as well.

```javascript
// student.services.ts 

  import { Injectable } from '@angular/core';
  import { Observable } from 'rxjs';

  @Injectable({
    providedIn: 'root'
  })
  export class StudentService {

  students = [{
      id: 1,
      name: 'Prasanth',
      enrollmentnumber: 110201501126,
      college: 'SNS College of Engineering',
      university: 'AU'
  },
  {
      id: 2,
      name: 'Mohan',
      enrollmentnumber: 110470116027,
      college: 'SNS College of Engineering',
      university: 'AU'
  }];

  constructor() { }

  public getStudents(): any {
     const studentsObservable = new Observable(observer => {
            setTimeout(() => {
                observer.next(this.students);
            }, 1000);
     });

     return studentsObservable;
 }
}
```
When ```observer.next()``` is called in the observables the data passed inside the ```next()``` is sent to all the subscribers across the application. Now we have created an observable, lets subscribe to this observables in a component. 

## Subscribing to observables
Remember, observables are lazy. If you don’t subscribe nothing is going to happen. It’s good to know that when you subscribe to an observer, each call of subscribe() will trigger it’s own independent execution for that given observer. Subscribe calls are not shared among multiple subscribers to the same observable.

```javascript
  // app.component.ts

  import { Component, OnInit, OnDestroy } from '@angular/core';
  import { Subscription } from 'rxjs';
  import { StudentService } from './student.service';

  @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
  })
  export class AppComponent implements OnInit {

      students = [];
      studentsObservable: Subscription;

      constructor(private studentservice: StudentService) {}

      ngOnInit() {
          this.studentsObservable = this.studentservice.getStudents();
          studentsObservable.subscribe((studentsData) => {
              this.students = studentsData;
          });
      }
  }
```

## Unsubscribing to Observables
Unsubscribing is necessary, it is important that we unsubscribe from any subscriptions that we create in our Angular components when the component is destroyed to avoid memory leaks.

```javascript
  // app.component.ts

   ngOnDestroy() {
        // unsubscribe to ensure no memory leaks
        this.studentsObservable.unsubscribe();
    }
```

## Error handling
One of the most difficult tasks in asynchronous programming is dealing with errors. Unlike interactive style programming, we cannot simply use the try/catch/finally approach that we use when dealing with blocking code. So In the sense of Observables, you can handle the errors by specifying an error callback on the observer. 

```javascript
  // app.component.ts
  studentsObservable.subscribe({
    studentsData => console.log(studentsData),
    err => console.log(err)
  });
```
Producing an error also causes the observable to clean up subscriptions and stop producing values. An observable can either produce the values (calling the next callback), or it can complete, calling either the complete or error callback.
