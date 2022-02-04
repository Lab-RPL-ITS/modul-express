# Pertemuan

# Intro

Apa itu NodeJS? NodeJS adalah environment untuk menjalankan Javascript di luar browser. Awalnya, Javascript hanya dapat dijalankan pada browser (pada console). Kemudian, NodeJS dibuat dengan menggunakan Chrome V8 JS Engine (mesin untuk menjalankan Javascript pada Google Chrome).

Node mempunyai komunitas yang besar. Dengan Node kita juga dapat membuat aplikasi full stack, yaitu membuat aplikasi full stack dengan bahasa Javascript.

## Requirement

Sebelum mempelajari course Node ini, kamu diharapkan memahami materi di bawah:

- HTML, CSS, Javascript, ES6
- Callbacks, Promises, Async-Await

## Perbedaan Browser dan NodeJS

Berikut adalah perbedaan menggunakan Javascript dengan browser dan Node.

1. Pada browser, kita dapat mengakses DOM. Pada Node, kita tidak dapat mengakses DOM. Karena pada browser, terdapat DOM (ada elemen-elemennya) sedangkan di Node tidak ada.
2. Pada browser, ada object Window. Pada Node, tidak ada. Jadi kita tidak dapat mengakses API yang disediakan object Window pada Node. Misalkan: API `Window.open()`.
3. Pada browser, Javascript digunakan untuk membuat aplikasi interaktif (dapat bergerak), sedangkan pada Node, Javascript digunakan untuk membuat server side apps.
4. Pada browser, tidak dapat mengakses filesystem (file pada storage komputer). Sedangkan dengan Node, bisa.

# Node Fundamentals

## Install Node

Kunjungi [http://nodejs.org](http://nodejs.org) . Download versi LTS. LTS artinya long term support (dimana itu adalah versi yang paling stabil untuk production). Kemudian install seperti biasa dengan selalu memilih yes pada instalasi.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled.png>)

Cara mengecek apakah node sudah terinstall adalah dengan ketikkan command pada terminal: `node` . Jika outputnya berupa versi node, maka Anda telah berhasil menginstall node.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%201.png>)

## Menjalankan Node

Kamu dapat menjalankan Node dengan 2 cara:

1. Repl

   Pada terminal, ketikkan command `node`, kemudian setelah simbol `>` kamu dapat memasukkan statement Javascript yang ingin kamu jalankan. Namun, metode repl kurang digunakan karena statement tidak dapat disimpan.

   ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%202.png>)

2. CLI

   Buat folder yang berisikan file **app.js**. Isi file dengan statement Javascript, kemudian run file tersebut dengan command

   `node <path file>`

   ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%203.png>)

## Globals Object

Globals Object adalah object yang tersedia pada seluruh module. Berikut beberapa object yang merupakan globals object:

1. `__dirname`

   return path dari current directory.

   ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%204.png>)

2. `__filename`

   return file name.

3. `require`

   adalah function untuk menggunakan modules (dengan CommonJS).

4. `module`

   return info mengenai current module (file).

5. `process`

   return info mengenai env dimana program dieksekusi.

## Modules

Buat file **module.js** dengan isi code sebagai berikut.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%205.png>)

Kemudian file **app.js** dengan isi code sebagai berikut. Jalankan **app.js**.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%206.png>)

Jadi pada **module.js**, kita mengexport object berisi properti **hasna** dan **thifa.** Pada **app.js**, kita dapat mengakses object yang diexport dengan `require` dan file-nya (dapat ditulis tanpa ekstensi .js). Kemudian pada `const names`, akan berisi object yang diexport pada file **module.js.**

### Syntax Alternatif

Pada **module.js**, kamu dapat juga mengexport dengan cara:

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%207.png>)

Pada course ini yang akan lebih digunakan adalah cara yang pertama.

Kamu juga dapat require suatu module tanpa meng-assign nya pada variable tertentu. Pada contoh di bawah, buat file **suatu module.js** dan isi dengan code di bawah.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%208.png>)

Kemudian pada **app.js** import module tersebut, dan jalankan app.js. Fungsi pada **suatumodule.js** ter-invoke karena fungsinya diinvoke pada module **suatumodule.js**.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%209.png>)

### Built-in Modules

Built-in Modules adalah modules yang sudah tersedia pada Node, yang dapat kita import untuk memudahkan kita melakukan berbagai task. Di sini tidak akan dibahas seluruh jenis built-in modules, hanya modules yang mungkin sering dipakai saja. Untuk modules lebih lengkapnya kamu dapat membacanya di dokumentasi NodeJS, [Index | Node.js v17.3.0 Documentation (nodejs.org)](https://nodejs.org/api/). Ada beberapa macam built-in modules pada Node:

1. OS

   Module OS adalah module yang menyediakan utility (property dan method) berhubungan
   dengan operating system.

   ```jsx
   // import module OS
   const os = require("os");

   // info about current user
   const user = os.userInfo();
   console.log(user);

   // method returns the system uptime in seconds
   console.log(`The System Uptime is ${os.uptime()} seconds`);

   const currentOS = {
     name: os.type(), // operating system name
     release: os.release(),
     totalMem: os.totalmem(), // total amount of system memory in bytes as an integer
     freeMem: os.freemem(), // amount of free system memory in bytes as an integer
   };
   console.log(currentOS);
   ```

   ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2010.png>)

2. Path

   Module Path menyediakan utility yang berhubungan dengan file dan directory path.

   ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2011.png>)

   Pada contoh di atas,

   - `path.sep` mereturn `\` sebagai separator tiap segmen dalam path.
   - `path.join` menggabungkan beberapa segmen menjadi satu path.
   - `path.basename` mereturn last portion dalam suatu path.
   - `path.resolve` mereturn absolute path.

3. FS

   Module yang memudahkan kita untuk berinteraksi dengan file system. Kita dapat write, read, dan sebagainya pada suatu file. Method pada module File System dibagi menjadi dua:

   1. FS Synchronous

      ```jsx
      const { readFileSync, writeFileSync } = require("fs");
      console.log("start");

      /* membaca file first.txt dengan encoding utf8. utf8 ini adalah jenis karakter
      pada first.txt agar komputer dapat membaca file. ada beberapa jenis encode yang lain */
      const first = readFileSync("./content/first.txt", "utf8");
      const second = readFileSync("./content/second.txt", "utf8");

      /* write pada result-sync.txt dengan hasil dari membaca first.txt dan second.txt */
      writeFileSync(
        "./content/result-sync.txt",
        `Here is the result : ${first}, ${second}`,
        { flag: "a" } // opsi flag, 'a' untuk Open file for appending. The file is created if it does not exist.
      );
      console.log("done with this task");
      console.log("starting the next one");
      ```

      ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2012.png>)

   2. FS Asynchronous

      ```jsx
      const { readFile, writeFile } = require("fs");

      console.log("start");
      readFile("./content/first.txt", "utf8", (err, result) => {
        if (err) {
          console.log(err);
          return;
        }
        const first = result;
        readFile("./content/second.txt", "utf8", (err, result) => {
          if (err) {
            console.log(err);
            return;
          }
          const second = result;
          writeFile(
            "./content/result-async.txt",
            `Here is the result : ${first}, ${second}`,
            (err, result) => {
              if (err) {
                console.log(err);
                return;
              }
              console.log("done with this task");
            }
          );
        });
      });
      console.log("starting next task");
      ```

      ![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2013.png>)

   Pada read file asynchronous first.txt, kita memanggil callback untuk melakukan read file second.txt. Jadi pada asynchronous, kita harus memakai callback untuk melakukan task tepat sehabis fungsi asynchronous. Karena, program akan menjalankan task berikutnya diluar callback. Perhatikan output `console.log('starting next task')`, pada metode synchronous, next task akan dijalankan setelah read/write file selesai. Sedangkan pada asynchronous, next task akan dijalankan sebelum read/write file selesai.

   Agak sulit untuk membaca callback di dalam callback, maka di section berikutnya kita akan memakai Promises atau Async Await untuk handle fungsi asynchronous.

## NPM

### Install

NPM (Node Package Manager) adalah seperti semacam library/package/modules (istilah-istilah ini akan digunakan pada modul ini) yang disediakan oleh orang lain yang baik hati mempublish code nya pada NPM, sehingga bisa kita gunakan dan tidak perlu capek-capek membuatnya dari awal. Kamu bisa mengecek [npm (npmjs.com)](https://www.npmjs.com/) untuk melihat library apa saja yang tersedia.

Kamu bisa mengecek versi npm dengan command berikut (Node sudah otomatis terdapat npm didalamnya).

```bash
npm --version
```

Untuk menginstall suatu package, dapat dilakukan dengan command:

```jsx
npm i <packageName> // untuk menginstall package pada current project saja

npm install -g <packageName> // menginstall package agar dapat digunakan di seluruh project
```

Sebelum menginstall suatu package pada current project, kamu harus menginisiasi project agar menggunakan npm. Dapat menggunakan command `npm init` kemudian kamu harus memilih opsi-opsi (dengan klik enter) atau `npm init -y` untuk menggunakan opsi default.

Setelah `npm init`, pada current project akan terbuat file **package.json**, ini adalah manifest file yang berisi tentang project-mu.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2014.png>)

Sekarang kita coba menginstall suatu package. Jalankan command `npm i lodash`. Maka akan muncul key **dependencies** pada **package.json,** dan ada folder **node_modules** berisi folder **lodash.** Folder **node_modules** akan berisi library-library yang diinstall dengan npm.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2015.png>)

Setelah itu, coba:

```jsx
const _ = require("lodash");
const items = [1, [2, [3, [4]]]];
const newItems = _.flattenDeep(items);
console.log(newItems);
```

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2016.png>)

### Share Code

Bicara mengenai sharing code, kamu dapat push repository kamu tanpa folder **node_modules**. Folder **node_modules** akan memakan banyak resource, sehingga merupakan hal yang baik kita tidak perlu mem-push/pull nya pada git (jangan lupa menuliskannya pada .gitignore). Dengan mem-push file **package.json** dan **package-lock.json**, temanmu dapat mendapatkan **node_modules** yang sama dengan menggunakan command `npm install` (setelah pull repository). **Package-lock.json** digunakan untuk men-lock agar library yang diinstall adalah versi yang ditentukan. Karena jika berbeda versi ditakutkan ada perbedaan behavior.

### Package Nodemon

Jalankan `npm i nodemon --save-dev`. Artinya package ini hanya akan diinstall pada envirotment development. Package nodemon adalah salah satu contoh package yang dapat kamu install secara global. Setelah menginstall package ini, kamu dapat menjalankan app.js dengan `nodemon app.js`, kemudian setiap ada perubahan pada project maka terminal akan langsung mengeluarkan output yang terupdate.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2017.png>)

Pada package.json, kamu dapat menambahkan key dan value pada key **“scripts”**. Pada contoh di bawah, jika kamu `npm start`, maka akan menjalankan `nodemon app.js`.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2018.png>)

### Uninstall

Kamu dapat uninstall package dengan `npm uninstall <packageName>`, otomatis library pada node_modules dan package.json akan terhapus. Dapat juga dengan menghapus code pada package.json, kemudian `npm install`.

## Event Loop

Sebelum masuk event loop, kamu harus memahami sinkron asinkronus pada javascript.

Untuk lebih memahami tentang sinkron asinkronus, pahami di bagian concurrency modul LBE 2021 lalu di [modul-lbe-rpl-2021/modul-3 at main · rizqitsani/modul-lbe-rpl-2021 · GitHub](https://github.com/rizqitsani/modul-lbe-rpl-2021/tree/main/modul-3#concurrency).

Event loop adalah salah satu aspek penting yang perlu dipahami dalam Node JS.

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2019.png>)

Perhatikan contoh gambaran event loop di atas, di sini alurnya adalah para user membuat request ke server, setiap ada request masuk, maka akan meregister callback untuk menjalankan logic dengan mengakses database, network dan lain-lain. Nah proses logic ini dapat memakan waktu. Kita sebut saja logic ini adalah logic A. Bayangkan jika prosesnya adalah synchronous, maka 1 user harus menyelesaikan logic A sampai selesai, baru user lain yang merequest logic A dapat menjalankan logic A tersebut. Maka register callback dilakukan agar proses logic A asynchronous, setelah user request dan register callback, event loop tetap dapat menerima request dari user lain. Setelah logic A selesai secara asynchronous, hasil response nya akan dibalikkan ke pengaju request (execute callback).

Untuk memahami lebih lanjut, perhatikan call stack di bawah ini.

```jsx
const bar = () => console.log("bar");

const baz = () => console.log("baz");

const foo = () => {
  console.log("foo");
  bar();
  baz();
};

foo();
```

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2020.png>)

Diagram call stack nya akan seperti ini:

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2021.png>)

Eksekusi per iterasi:

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2022.png>)

Terlihat normal bukan?

### Message Queue

Perhatikan contoh call stack lain:

```jsx
const bar = () => console.log("bar");

const baz = () => console.log("baz");

const foo = () => {
  console.log("foo");
  setTimeout(bar, 0);
  baz();
};

foo();
```

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2023.png>)

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2024.png>)

Eksekusi per iterasi:

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2025.png>)

Ketika `setTimeout` dipanggil, browser atau NodeJS akan start timer. Ketika timer habis (pada contoh ini 0 sebagai timeout), callback function akan diletakkan pada Message Queue. Message Queue juga adalah event-event yang diantrikan untuk diproses nantinya.

Loop akan memberi prioritas pada callstack, akan memproses semua yang ada di call stack, setelah call stack habis, akan memproses event-event pada message queue.

Kita tidak perlu menunggu fungsi `setTimeout` karena fungsi tersebut ‘hidup’ pada thread-nya sendiri. Atau dalam bahasa lain, jika kita men-set timeout pada `setTimeout` adalah 2, kita tidak perlu menunggu 2 detik, proses menunggu akan terjadi pada tempat yang lain.

### Job Queue

ECMAScript 2015 mengenalkan konsep Job Queue, yang dipakai oleh Promises. Ini adalah cara untuk mengeksekusi hasil dari async function secepatnya, tidak seperti message queue yang mengeksekusi setelah call stack habis. Promise yang sudah resolve sebelum fungsi berakhir akan dieksekusi setelah fungsi yang sekarang sedang dijalankan.

```jsx
const bar = () => console.log("bar");

const baz = () => console.log("baz");

const foo = () => {
  console.log("foo");
  setTimeout(bar, 0);
  new Promise((resolve, reject) =>
    resolve("should be right after baz, before bar")
  ).then((resolve) => console.log(resolve));
  baz();
};

foo();
```

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2026.png>)

Diagram call stack nya:

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2027.png>)

Ada perbedaan antara promises, async await (yang di-build berdasar promises), dan fungsi javascript asynchronous tradisional seperti setTimeout.

## Event Emitter

```jsx
const EventEmitter = require("events");

const customEmitter = new EventEmitter();

customEmitter.on("response", (name, id) => {
  // di sini menerima argumen 'john' dan 34
  console.log(`data recieved user ${name} with id:${id}`);
});

customEmitter.on("response", () => {
  console.log("some other logic here");
});

customEmitter.emit("response", "john", 34); // di sini passing argumen 'john' dan 34
```

Sesuai dengan namanya event emitter di sini kita membuat event. Pertama import module **events**. Buat instance dari class **EventEmitter** (class dari built in module). Class EventEmitter mempunyai method built in **on** dan **emit.** Pada method **on**, akan mendengarkan event ‘response’, jika terdengar, maka callback akan dijalankan. Pada method **emit**, kita munculkan event ‘response’.

Contoh event emitter built in :

```jsx
const http = require("http"); // module built in http

const server = http.createServer();

server.on("request", (req, res) => {
  res.end("Welcome");
});

server.listen(5000);
```

Darimana kita bisa tahu variabel **server** bisa menerima event ‘request’ ? Dari dokumentasi: [HTTP | Node.js v15.14.0 Documentation (nodejs.org)](https://nodejs.org/docs/latest-v15.x/api/http.html#http_event_request)

## HTTP

### HTTP Cycle

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2028.png>)

Saat kita mengunjungi sebuah web, kita melakukan request terhadap suatu server. Server adalah komputer yang berisi resource sebuah aplikasi. Setelah request diterima server, server akan mengirim response kepada kita. Request atau response adalah pesan yang menggunakan protokol HTTP, makanya request atau response juga disebut pesan HTTP.

1. Request Message

Terdiri dari method get, url, headers, dan body. Method adalah jenis pesan, misalkan jika get digunakan jika data dapat terlihat di url, sedangkan post digunakan jika data tidak terlihat di url. Headers berisi tentang meta information mengenai pesan. Kita dapat mengatur beberapa informasi dalam headers jika diinginkan, misalkan mengatur apakah memakai mode cache atau tidak. Body berisi data yang dikirim bersama pesan tersebut. Misalkan request dengan metode post, akan mengirim suatu data untuk disimpan di database aplikasi.

1. Response Message

Terdiri dari status code, status text, headers, body. Status code lebih baik dihafalkan, misalkan 200 artinya OK, 400 artinya error. Pada headers, kita dapat mengatur misalkan _content-type_, yaitu tipe konten data yang kita kirim. Pada body, contohnya kita mengirim file html yang akan dikirim sebagai response.

### HTTP Methods

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2029.png>)

Kamu dapat mengeksplor developer tools bagian network untuk mengetahui informasi mengenai request dan response yang sedang terjadi pada browser.

### HTTP Status

Pada pesan HTTP berupda response, terdapat response status. Response status merupakan salah satu bagian dari respons yang berisikan tentang informasi apakah sebuah request berhasil atau gagal dilakukan. Status yang diberikan berupa kode (status code) dan pesan dari kode tersebut dalam bentuk teks (status message).

Indikasi keberhasilan request client ditentukan oleh response status code yang dikirim oleh server. Karena itu, tentu nilai status code tak bisa sembarang kita tetapkan. Status code haruslah bernilai 3 digit angka dengan ketentuan berikut:

- 100-199 : informational responses.
- **200 - 299 : successful responses.**
- 300-399 : redirect.
- **400-499 : client error.**
- **500-599 : server errors.**

Fokus terhadap poin yang ditebalkan yah karena poin itu akan sering digunakan. Silakan eksplorasi lebih detail mengenai status code pada [halaman MDN mengenai HTTP Status](https://developer.mozilla.org/id/docs/Web/HTTP/Status).

Cara menset status code dari response:

```jsx
const requestListener = (request, response) => {
  response.statusCode = 404;

  // 404 defaultnya adalah 'not found'
  response.statusMessage = "User is not found";
};
```

### HTTP Headers

Header adalah dimana kita dapat men-set beberapa informasi mengenai pesan HTTP. Contoh properti header yang bisa diatur adalah content-type. Properti header lain dapat diakses pada [HTTP headers - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

```jsx
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "content-type": "text/html" });
  res.write("<h1>home page</h1>");
  res.end();
});

server.listen(5000);
```

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2030.png>)

Coba diganti `'content-type': 'text/plain'` :

![Untitled](<Pertemuan%201%20(Revisi)%2074286a1877c94927801f0c1fb2a24761/Untitled%2031.png>)

Kita sebaiknya menulis status code pada response untuk memberi tahu browser apa yang sedang terjadi.

Beberapa properti object req yang dapat digunakan adalah **req.method** dan **req.url**.

Contoh lain:

```jsx
const http = require("http");

const server = http.createServer((req, res) => {
  const url = req.url;
  // home page
  if (url === "/") {
    res.writeHead(200, { "content-type": "text/html" });
    res.write("<h1>home page</h1>");
    res.end();
  }
  // about page
  else if (url === "/about") {
    res.writeHead(200, { "content-type": "text/html" });
    res.write("<h1>about page</h1>");
    res.end();
  }
  // 404
  else {
    res.writeHead(404, { "content-type": "text/html" });
    res.write("<h1>page not found</h1>");
    res.end();
  }
});

server.listen(5000);
```

# Referensi

[Coding Addict - YouTube](https://www.youtube.com/channel/UCMZFwxv5l-XtKi693qMJptA)

[Run JavaScript Everywhere. (nodejs.dev)](https://nodejs.dev/)

[Documentation | Node.js (nodejs.org)](https://nodejs.org/en/docs/)
