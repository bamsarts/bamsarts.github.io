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
