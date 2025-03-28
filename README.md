# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. In this tutorial, we used `RwLock<>` to synchronise the use of **Vec of Notifications**. Explain why it is necessary for this case, and explain why we do not use `Mutex<>` instead?

In this tutorial, `RwLock<>` is used to allow multiple threads to read the notifications concurrently, but ensures that only one thread can write (modify) the notifications at a time. This is necessary because we want to enable multiple threads to access the list of notifications for reading, which can happen frequently without causing issues. On the other hand, `Mutex<>` would allow only one thread to access the data at any given time, which would severely limit concurrent read access and reduce performance. Therefore, `RwLock<>` is preferred to allow more efficient reading and still ensure safe writes.

2. In this tutorial, we used the `lazy_static` external library to define `Vec` and `DashMap` as a **“static”** variable. Compared to Java where we can mutate the content of a `static` variable via a `static` function, why did Rust not allow us to do so?

Rust enforces strict ownership and borrowing rules to ensure memory safety, which is why it doesn’t allow direct mutation of static variables without a safe synchronization mechanism. In Java, static variables can be mutated directly, but Rust’s memory safety guarantees prevent this to avoid race conditions and data corruption. The use of lazy_static ensures that the static variables are initialized safely, and synchronization mechanisms like RwLock<> or Mutex<> are needed to mutate them in a thread-safe way.

#### Reflection Subscriber-2

1. Have you explored things outside of the steps in the tutorial, for example: `src/lib.rs`? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

I have primarily focused on the steps within the tutorial, but I did briefly explore src/lib.rs to understand the broader structure of the application. I learned how different components are connected in the codebase, particularly the interconnections between services, controllers, and models. Understanding the modular structure helped in organizing my own implementations and how to interact with other parts of the codebase. I didn’t explore deeply outside of the tutorial steps because I wanted to ensure I completed the necessary tasks as instructed.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

The Observer pattern significantly simplifies adding more subscribers to the system. Since the Publisher only needs to notify all subscribed receivers, adding new subscribers is as simple as registering them with the Publisher. This decouples the Publisher from individual subscribers, making the system scalable. Additionally, spawning multiple instances of the Main app is straightforward because the system is designed to handle multiple receivers with unique URLs. Each instance of the Receiver app will be able to receive notifications, making it easy to add more instances as needed.

3. Have you tried to make your own Tests, or enhance documentation on your **Postman collection**? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

I have used Postman extensively to test the API endpoints, and I found it to be incredibly useful in validating the functionality of the notification system. I’ve also added custom tests in Postman to simulate different scenarios, such as subscribing to a product type and checking if the notifications are properly received by the Receiver app. This testing process is invaluable for ensuring that the notification system works as expected. Enhancing the documentation in Postman also helps streamline communication within the team, especially when sharing API specifications.

