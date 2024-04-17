# BambangShop Publisher App

Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project

In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:

1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment

1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable | type | description |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)

- [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
- **STAGE 1: Implement models and repositories**
  - [x] Commit: `Create Subscriber model struct.`
  - [x] Commit: `Create Notification model struct.`
  - [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - [x] Commit: `Implement add function in Subscriber repository.`
  - [x] Commit: `Implement list_all function in Subscriber repository.`
  - [x] Commit: `Implement delete function in Subscriber repository.`
  - [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - [x] Commit: `Create Notification service struct skeleton.`
  - [x] Commit: `Implement subscribe function in Notification service.`
  - [x] Commit: `Implement subscribe function in Notification controller.`
  - [x] Commit: `Implement unsubscribe function in Notification service.`
  - [x] Commit: `Implement unsubscribe function in Notification controller.`
  - [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - [x] Commit: `Implement publish function in Program service and Program controller.`
  - [x] Commit: `Edit Product service methods to call notify after create/delete.`
  - [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. For this case, a single Subscriber model struct should be sufficient. We don't necessarily need to define a separate trait or interface. The Subscriber struct already encapsulates the data and behavior (URL) required for the Observer pattern implementation. Since Rust doesn't have traditional inheritance, we can use composition to achieve the same effect as an interface, if needed.

2. Using DashMap is a better choice than Vec because both id and url need to be unique. With a DashMap, we can use the id or url as the key, ensuring uniqueness, and store the corresponding Program or Subscriber struct as the value. A Vec would not guarantee uniqueness and would require additional checks to prevent duplicates.

3. Using the DashMap library is a better choice than implementing the Singleton pattern in this case. The Singleton pattern is primarily used to ensure that a class has only one instance and provides a global point of access to that instance. However, in the case of the List of Subscribers, we need to store multiple Subscriber instances, and the DashMap provides a thread-safe way to manage and access this collection concurrently. Implementing the Singleton pattern would not address the thread-safety concerns for managing multiple Subscriber instances.

#### Reflection Publisher-2

1.Separating the "Service" and "Repository" layers from the Model provides better separation of concerns and promotes code organization and maintainability. The main reasons for this separation are:

- The Model should focus on representing the data structure and enforcing data integrity rules. It should not be concerned with how the data is persisted or how business logic is implemented.
- The Repository layer abstracts the data access logic, making it easier to swap out the underlying data storage - mechanism (e.g., database, file system) without affecting the rest of the application.
- The Service layer encapsulates the business logic and orchestrates the interactions between different components, such as Models, Repositories, and external services.

This separation of concerns makes the code more modular, testable, and easier to maintain and extend in the long run.

2.If we only use the Model and combine all the responsibilities (data storage, business logic, and interactions between models) within each model class, it would lead to tightly coupled and monolithic code. The code complexity and maintenance overhead would increase significantly as the application grows. Here are some potential issues:

- Each Model would become bloated with business logic, data access code, and code to interact with other Models, making it difficult to understand and maintain.
- Changes to the data storage mechanism or business logic would require modifications in multiple Model classes, increasing the risk of introducing bugs.
- Testing would become more difficult as Models would have multiple responsibilities, and it would be harder to isolate and mock dependencies.
- Code reuse would be limited, as the logic would be tightly coupled with specific Model implementations.

By separating concerns into different layers (Model, Repository, Service), the code becomes more modular, easier to reason about, and easier to test and maintain in the long run.

3.Postman is a powerful tool for testing and interacting with APIs. In the context of this tutorial, Postman helps by providing a convenient interface to send HTTP requests to the application and inspect the responses. Some useful features of Postman include:

- Creating collections of requests, which can be organized into folders and shared with team members.
- Saving request and response data, making it easier to reproduce and debug issues.
- Enabling environment variables, which can be used to switch between different configurations (e.g., development, staging, production).
- Allowing scripting using JavaScript for more advanced testing scenarios.
- Providing a built-in runner for automated testing and continuous integration.

For my group project, Postman can be particularly useful for end-to-end testing of our API endpoints, ensuring that the application behaves as expected across different scenarios. The ability to save and share collections can facilitate collaboration within the team, and the scripting capabilities can help us create more complex test cases.

In future software engineering projects, especially those involving APIs or microservices, Postman will be a valuable tool for testing, debugging, and documenting the application's endpoints and expected behaviors.

#### Reflection Publisher-3
