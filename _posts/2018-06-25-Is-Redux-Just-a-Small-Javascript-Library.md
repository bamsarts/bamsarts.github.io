---
layout:     post
title:      Is Redux Just a Small Javascript Library ?
subtitle:   
date:       2018-06-25
author:     Bambang S
header-img: img/library.jpg
catalog: false
tags:
    - frontend
    - redux
    - mvc
---

Redux is known as library for states management. There are so many guides, tutorials, and videos even his documentation is pretty good. Then why am I writing this ? The reason is simple, that is to help others who may not understand with redux despite reading many tutorials about redux. Believe me, Redux is not that difficult. 

Although you will not be using this library, but there are many lessons learned from paradigm and architecture of this library and can be implemented in others libraries or frameworks.

### Redux is like database on frontend

Redux is like database on frontend side. It seems like database, redux can perform database operations such as query, filter, insert, and delete. If you know MVC (Model View Controller) redux is similar like Model and Controller. Redux doesn't call it a database but a store and there is only one store in one app called _Single Source of Truth_.

### UI = f(data)

Redux is connected to the display or application UI, data changes in redux will change the UI automatically. So the principle is: _Data changed = UI changed_

### Redux as a place of business logic

Businnes logic such as validation, verification, authorization, asynchronous processes and others, mostly the implementation process placed in redux. It is not mandatory depending on the type of application.

![dada](https://bamsarts.github.io/img/banner-redux.png)

I hope my explanation of redux implementation in real application can be understood. And then, let's look redux in react

### React & Redux

![dada](https://bamsarts.github.io/img/react_redux.png)

<p align="center">source : https://onsen.io</p>

Whatever libraries or frameworks each developer needs two important things in the application that is displaying and managing data. Redux serves to manage data, while to display data you can use any library.
Redux is the most popular library in react, but actually redux can be mixed with other libraries such as vue, angular, even the legendary jquery.

The common challenge when creating app based component such as React, is handling communication between components. For example :

- How to tell component B if component A has been clicked ?
- How to change the contents of the table components, if there is a change in the filter component ?
- How to communicate component with different parent ?

Redux offers global state solutions. The way it works is simple, the state of each component is moved to a global state called _store_. This store will connect with components. And then, you only need to deal with their store, while redux that will handle communication between component and UI changes.

![redux](https://bamsarts.github.io/img/redux.png)

<p align="center">source : https://blog.codecentric.de</p>

### What is the state ?

Definition of state is context dependent, state is like the memory of application. This memory is used to store the status of the activity or execution of the application. For example :

- User interaction such as click, scroll etc (event state)
- Status of current page (location state)
- User login or not (session state)
- Data fetching from server
- etc

In the react component has a state. State makes application more dynamically. Changes in each component are generally done by changing their status.

### When is the right time to use redux on react ?

Based on my experience, you should not rush immediately to learn redux, it is recomended to focus on react first. For communication between components, can use _callback, refs, context, and high order componen_. If you feel managing the state with it way too complicated, most likely you need redux.

### How to learn Redux ?

Best way to  open your teks editor / IDE and try the code, open Redux documentation, and keep up trial & error. 