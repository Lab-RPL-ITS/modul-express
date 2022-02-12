# Pertemuan 2

# Table of contents

- [Pertemuan 2](#pertemuan-2)
- [Table of contents](#table-of-contents)
- [Express](#express)
  - [Penggunaan](#penggunaan)
  - [Hello World](#hello-world)
  - [Routing](#routing)
  - [Middleware](#middleware)
    - [Menggunakan Middleware](#menggunakan-middleware)
    - [Next Function](#next-function)
  - [Static Files](#static-files)
  - [Request](#request)
    - [Route Parameters](#route-parameters)
    - [Query String](#query-string)
    - [Body](#body)
  - [Response](#response)
    - [Status Code](#status-code)
    - [JSON](#json)
  - [Express Router](#express-router)
- [Referensi](#referensi)

# Express

Express adalah framework untuk membuat web application dalam NodeJS. Dengan Express, kita dapat membuat API dengan sangat cepat dibandingkan cara lama yang pure menggunakan NodeJS saja.

Kamu dapat menambahkan package express pada project mu dengan:

```jsx
npm install express --save
```

## Penggunaan

Express dapat digunakan untuk API atau SSR (server side rendering).

1. API
   1. Mengirim data berupa JSON
   2. Return berupa `res.json()`
2. SSR
   1. Merender template sebuah halaman
   2. Return berupa `res.render()`

## Hello World

```jsx
const express = require("express");
const app = express();

/* Mengatur response jika browser mengakses url '/' dengan method GET */
app.get("/", (req, res) => {
  console.log("User mengakses home page");
  res.status(200).send("Home Page");
});

app.listen(5000, () => {
  console.log("server is listening on port 5000...");
});
```

Untuk membuat aplikasi express, kita bisa memanggil fungsi `express()` dan menyimpan kembaliannya ke variabel dalam contoh ini `app`.

Pada contoh diatas, jika ada request ke route `/` dengan method `GET`, maka akan mengembalikan response “Home Page”. Terakhir, untuk menjalankan server dan mendengarkan port tertentu, kita dapat menggunakan method `listen()`.

## Routing

Routing disini adalah menentukan bagaimana aplikasi kita akan merespon request ke endpoint tertentu yang terdiri dari HTTP Method (GET, POST, PUT) dan URI (’/’, ‘/about’). Kita dapat mendefinisikan sebuah route dengan struktur seperti berikut:

```jsx
app.METHOD(PATH, HANDLER);
```

- app adalah instance dari aplikasi express (didapat dari `express()`)
- METHOD adalah HTTP Method yang ditulis dengan huruf kecil
- PATH adalah path dari route pada server
- HANDLER adalah fungsi yang akan dijalankan jika ada request ke endpoint tersebut

Contoh:

```jsx
app.get("/", (req, res) => {
  res.send("Request GET pada '/'");
});

app.post("/users", (req, res) => {
  res.send("Request POST pada '/users'");
});
```

## Middleware

![Untitled](<Pertemuan%202%20(Revisi)%20444fd0c49ea34e6f91c2bf587cfd08b5/Untitled.png>)

Middleware adalah fungsi yang dijalankan setelah menerima request dan sebelum memberikan response. Misalkan kita memiliki middleware `logger()` yang berfungsi untuk melakukan log setiap ada request ke server

```jsx
const logger = (req, res, next) => {
  const { method, url } = req;
  const time = new Date().toLocaleString("us");

  console.log(method, url, time);

  next();
};
```

### Menggunakan Middleware

Untuk menggunakan middleware, kita dapat menggunakan routing biasa

```jsx
app.get("/", logger, (req, res) => {
  res.send("Home");
});
```

Apabila kita ingin menggunakan middleware pada semua route, gunakan fungsi `app.use()`

```jsx
app.use(logger);
```

### Next Function

Pada fungsi middleware, kita memiliki parameter `next` yang berupa fungsi. Fungsi next ini dapat kita panggil untuk memanggil middleware berikutnya

```jsx
app.use((req, res, next) => {
  console.log("Halo");
});

// Request tidak akan mencapai fungsi ini
app.get("/", (req, res) => {
  res.send("Home");
});
```

Pada contoh di atas, terdapat fungsi **logger**. Kemudian fungsi **logger** (middleware) akan digunakan pada response route `/` dan `/about`. Parameter **req, res, next** pada **logger** sudah otomatis dari express. Next harus dipakai di akhir agar saat menjalankan fungsi (middleware) tersebut, dapat menjalankan fungsi (middleware) selanjutnya.

```jsx
const express = require("express");
const app = express();

const logger = (req, res, next) => {
  const method = req.method;
  const url = req.url;
  const time = new Date().getFullYear();
  console.log(method, url, time);
  next();
};

app.use(logger);

app.get("/", logger, (req, res) => {
  res.send("Home");
});
app.get("/about", logger, (req, res) => {
  res.send("About");
});

app.listen(5000, () => {
  console.log("Server is listening on port 5000....");
});
```

## Static Files

```jsx
const express = require("express");
const path = require("path");

const app = express();

app.use(express.static("./public"));

app.all("*", (req, res) => {
  res.status(404).send("resource not found");
});

app.listen(5000, () => {
  console.log("server is listening on port 5000....");
});
```

Express menyediakan middleware untuk menggunakan asset statis (seperti html, css, dll) yaitu `express.static()`. Pada contoh diatas, kita dapat menyimpan asset di folder **public**. Misal kita memiliki file /public/gambar.jpg, kita dapat mengaksesnya pada `http://localhost:5000/gambar.jpg`

## Request

Object request pada parameter suatu middleware dapat kita gunakan untuk beberapa hal dibawah ini

### Route Parameters

Pada API, kita sering menggunakan route yang dinamis seperti `/products/1`. Untuk mendefinisikan sebuah dynamic route kita dapat menggunakan `:` diikuti dengan nama parameternya seperti berikut

```jsx
app.get("/users/:id", logger, (req, res) => {
  const { id } = req.params;
  res.send(`Melihat user dengan id ${id}`);
});
```

Saat kita mengakses `http://localhost:5000/users/1`, req.params akan berisi

```json
{
  "id": "1"
}
```

### Query String

Untuk mengakses query string seperti `/products?search=keyboard`, kita dapat menggunakan `req.query`

```jsx
app.get("/products", logger, (req, res) => {
  if (req.query.search) {
    return res.send(`Cari produk ${req.query.search}`);
  }

  res.send("Lihat semua produk");
});
```

Saat kita mengakses `http://localhost:5000/product?search=keyboard`, req.params akan berisi

```json
{
  "search": "keyboard"
}
```

### Body

Untuk mengakses request body, kita memerlukan config tambahan pada aplikasi kita untuk melakukan parsing. Setelah itu, body bisa diakses melalui `req.body`

```jsx
const express = require("express");

const app = express();

// Untuk header application/json
app.use(express.json());
// Untuk header application/x-www-form-urlencoded
app.use(express.urlencoded({ extended: false }));

app.post("/", (req, res) => {
  res.send(req.body);
});

app.listen(5000, () => {
  console.log("Server is listening on port 5000....");
});
```

## Response

### Status Code

Untuk mengembalikan status gunakan `res.status()`

```jsx
app.get("/health", (req, res) => {
  res.status(200).end();
});

app.put("/users/1", (req, res) => {
  res.status(401).end();
});
```

### JSON

Misal kita menyimpan data produk sebuah toko dalam bentuk JSON pada `data.js`

```jsx
const products = [
  {
    id: 1,
    name: "albany sofa",
    image:
      "https://dl.airtable.com/.attachments/6ac7f7b55d505057317534722e5a9f03/9183491e/product-3.jpg",
    price: 39.95,
    desc: `I'm baby direct trade farm-to-table hell of, YOLO readymade raw denim venmo whatever organic gluten-free kitsch schlitz irony af flexitarian.`,
  },
  {
    id: 2,
    name: "entertainment center",
    image:
      "https://dl.airtable.com/.attachments/da5e17fd71f50578d525dd5f596e407e/d5e88ac8/product-2.jpg",
    price: 29.98,
    desc: `I'm baby direct trade farm-to-table hell of, YOLO readymade raw denim venmo whatever organic gluten-free kitsch schlitz irony af flexitarian.`,
  },
];

module.exports = products;
```

Untuk mengembalikan data tersebut dalam bentuk JSON, kita dapat menggunakan `res.json()`

```jsx
const express = require("express");
const app = express();
const products = require("./data");

app.get("/", (req, res) => {
  res.json(products);
});

app.listen(5000, () => {
  console.log("Server is listening on port 5000....");
});
```

![Untitled](<Pertemuan%202%20(Revisi)%20444fd0c49ea34e6f91c2bf587cfd08b5/Untitled%201.png>)

Kamu bisa memakai extension chrome json viewer untuk mempermudah saat membaca JSON.

## Express Router

Kita dapat memisahkan file routing kita menjadi beberapa file agar lebih modular. Mari kita buat file-file seperti pada gambar di bawah

![Untitled](<Pertemuan%202%20(Revisi)%20444fd0c49ea34e6f91c2bf587cfd08b5/Untitled%202.png>)

Untuk membuat routing, selain menggunakan instance dari aplikasi, kita juga dapat menggunakan Express Router dengan memanggil `express.Router()`

**routes/blog.js**

```jsx
const express = require("express");

const router = express.Router();

router.get("/", function (req, res) {
  res.send("All Blogs");
});

router.get("/:id", function (req, res) {
  res.send("View Blogs" + req.params.id);
});

module.exports = router;
```

**routes/dashboard.js**

```jsx
const express = require("express");

const router = express.Router();

router.get("/", function (req, res) {
  res.send("Dashboard Page");
});

module.exports = router;
```

**routes/settings.js**

```jsx
const express = require("express");

const router = express.Router();

router.get("/", function (req, res) {
  res.send("Settings Page");
});

router.get("/profile", function (req, res) {
  res.send("Profile Page");
});

module.exports = router;
```

Kemudian, pada **app.js.**

```jsx
var express = require("express");
var app = express();
const settings = require("./routes/settings");
const dashborad = require("./routes/dashboard");
const blogs = require("./routes/blogs");

app.use("/", dashboard);
app.use("/blogs", blogs);
app.use("/settings", settings);

app.listen(5000, function () {
  console.log("Listening to Port 5000");
});
```

Dengan pengorganisasian seperti ini, kita menghindari menulis prefix berulang-ulang, seperti pada prefix `blogs`, dapat digunakan untuk route `/blogs/:id` tanpa ditulis prefix blogs pada file **blogs.js.**

# Referensi

[Coding Addict - YouTube](https://www.youtube.com/channel/UCMZFwxv5l-XtKi693qMJptA)

[Node JS — Router and Routes. Hello all. In this blog, I am… by Sjlouji | Joan Louji | The Startup (medium.com)](https://medium.com/swlh/node-js-router-and-routes-a4a3cfced5c4)
