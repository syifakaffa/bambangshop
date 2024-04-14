# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

Syifa Kaffa Billah
2206816430
Adpro-C

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
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
>1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam kasus BambangShop, apakah kita memerlukan sebuah interface (atau trait dalam Rust) untuk Subscriber tergantung pada kompleksitas perilaku yang diharapkan dari Subscriber tersebut. Jika semua Subscriber memiliki perilaku yang serupa atau hanya ada satu jenis Subscriber, cukup menggunakan satu Model struct untuk mewakili Subscriber. Namun, Jika terdapat berbagai jenis subsciber yang memiliki perilakunya masing-masing, maka kita dapat menggunakan trait/interface. Dalam kasus BambangShop, semua observers berperilaku serupa dan hanya memiliki satu kelas Subscriber saja, sehingga trait/interface tidak diperlukan, cukup dengan single model struct saja.

>2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Jika requirement program mengharusnya id Program dan url di Subscriber dibuat secara unik, maka menurut saya menggunakan DashMap (map/dictionary) lebih baik daripada vec (list) karena dapat memastikan bahwa id program dan URL subscriber yang harus unik dapat dijaga dengan efisien. Dengan menggunakan struktur data map, kita dapat dengan mudah mengelola dan memperoleh entri yang unik berdasarkan kunci (key) mereka, yang memudahkan operasi insert, pencarian, dan delete dengan kinerja yang optimal.

>3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Menurut saya, penggunaan DashMap tetap diperlukan untuk memastikan keamanan saat beroperasi secara concurency dan menjamin keamaan thread. Selain itu Singleton pattern sendiri sudah diterapkan di BambangShop, yaitu pada saat menggunakan *lazy_static*.


#### Reflection Publisher-2
>1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Pemisahan antara "Service" dan "Repository" dari "Model" dalam desain berbasis MVC bertujuan untuk memisahkan tanggung jawab yang berbeda agar aplikasi menjadi lebih modular, terpisah, dan mudah dipelihara dimana Service bertanggung jawab untuk logika bisnis, seperti validasi dan pengolahan data, sementara Repository bertanggung jawab untuk interaksi dengan penyimpanan data, seperti operasi CRUD ke database. Dengan kata lain, pemisahan ini akan membuat program kita memenuhi Single Responsibility Principle (SRP) dari SOLID Principles.

>2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika kita hanya menggunakan Model dalam desain pattern tanpa memisahkan Service dan Repository, maka seluruh logika bisnis dan interaksi dengan data akan diimplementasikan langsung di dalam Model. Hal ini dapat mengakibatkan kompleksitas kode yang tinggi di setiap model (Program, Subscriber, Notification) karena semua tanggung jawab akan bercampur aduk. Akibatnya, program akan sulit untuk di maintance dan diperluas karena tidak ada pemisahan yang jelas antara lapisan bisnis dan akses data. 


>3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Sebelumnya saya pernah menggunakan Postman untuk mendukung perkuliahan Pemrograman Berbasis Platform dimana Postman ini saya gunakan untuk melakukan testing API. Postman sendiri merupakan tools yang berguna untuk menguji fungsionalitas API dengan mengirimkan request HTTP sambil melihat dan menganalisis apakah response yang diberikan sesuai dengan harapan saya.


#### Reflection Publisher-3
