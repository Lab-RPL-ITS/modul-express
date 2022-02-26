# Pertemuan 4

### Table of Contents

- [Pertemuan 4](#pertemuan-4)
  - [Table of Contents](#table-of-contents)
  - [Three Layer Architecture](#three-layer-architecture)
  - [Environment Variable](#environment-variable)
  - [Testing](#testing)
    - [Pengenalan](#pengenalan)
    - [Instalasi](#instalasi)
    - [Struktur Test](#struktur-test)
    - [Menulis Test](#menulis-test)
    - [Unit Testing](#unit-testing)
    - [Mock Function](#mock-function)
  - [Dokumentasi](#dokumentasi)
    - [Membuat Collection](#membuat-collection)
    - [Menambah Contoh Response](#menambah-contoh-response)
    - [Publish](#publish)
  - [Referensi](#referensi)

### Three Layer Architecture

Dalam pertemuan ketiga, kita menempatkan semua code kita dalam file routing. Sekarang, kita akan menerapkan three layer architecture berdasarkan prinsip Separation of Concern

![Untitled](Pertemuan%20%20c26f6/Untitled.png)

Dengan adanya three layer architecture, kita dapat memisahkan antara business logic dari API kita. Berikut alur dari aplikasi kita:

1. Controller menerima request, lalu memanggil service
2. Service yang berisi business logic memanggil data access layer
3. Data access melakukan query terhadap database
4. Hasil query diberikan ke service
5. Setelah business logic selesai dijalankan, hasilnya diberikan ke controlller
6. Controller memberi respond ke client

Apa perbedaan antara controller dan service? Hal ini bisa diibaratkan seperti manajer dan pekerja. Manajer biasanya memiliki tugas untuk:

- Mengelola pesanan yang masuk
- Memutuskan pekerja mana yang harus melayani pesanan tersebut
- Membagi pekerjaan

Sedangkan pekerja memiliki tugas:

- Menerima pesanan dari manajer
- Mengetahui langkah apa aja yang diperlukan untuk menyelesaikan pesanan
- Hanya memedulikan pekerjaannya
- Melaporkan hasil kerja ke manajer

Berdasar analogi diatas, maka controller bertugas untuk:

- Menerima HTTP request
- Memanggil service yang bertanggung jawab
- Meneruskan data yang diperlukan dari HTTP request ke service

Sedangkan service bertugas untuk:

- Menerima data dari controller
- Menjalankan algoritma dari business logic
- Mengembalikan hasil ke controller

Disini controller kita memanggil instance dari service. Tidak ada business logic yang dilakukan

```jsx
// controllers/Post/index.js

const PostService = require("../services/PostService");
const PostServiceInstance = new PostService();

async function getTask(req, res) {
  try {
    const { id } = req.params;

    const task = await taskServiceInstance.getById(id);

    return res.status(200).send({ success: true, data: task });
  } catch (error) {
    return next(error);
  }
}

module.exports = { createCord };
```

Service melakukan semua business logic dan memanggil data access layer (Mongoose) untuk berinteraksi dengan database

```jsx
const Task = require("../models/Task");

class TaskService {
  constructor() {
    this.taskModel = Task;
  }

  async getById(id) {
    const task = await this.taskModel.findById(id);
    return task;
  }
}

module.exports = TaskService;
```

### Environment Variable

Pada modul ketiga, kita telah mempelajari file **.env** yang berfungsi untuk menyimpan credential ataupun secret key dari apliklasi kita. Untuk mengakses variabel tersebut, kita dapat menggunakan package yang ada seperti **dotenv** lalu mengaksesnya menggunakan syntax **process.env** seperti contoh dibawah

```jsx
app.listen(process.env.PORT || 4000, () => {
  console.log(`Listening on http://localhost:${process.env.PORT || 4000}`);
});
```

Terdapat beberapa masalah ketika kita menggunakan cara diatas antara lain kita terkadang sering lupa nama variabelnya dan ada duplikasi ketika kita akan memberi default value. Agar lebih simpel, kita dapat membuat satu file yang menampung seluruh variabel tersebut

```jsx
// config/index.js

const dotenv = require("dotenv");
const envFound = dotenv.config();

if (envFound.error) {
  throw new Error("Couldn't find .env file  ");
}

const config = {
  dbUrl: process.env.MONGODB_URI,
  port: process.env.PORT || 4000,
};

module.exports = config;
```

Sekarang, untuk mengaksesnya kita sudah mendapatkan autocompletion apabila menggunakan VSCode. Jadi tidak perlu mengintip file **.env** lagi apabila lupa nama variabelnya

![Untitled](Pertemuan%20%20c26f6/Untitled%201.png)

## Testing

### Pengenalan

Testing adalah sebuah metode untuk memastikan bahwa produk yang kita buat sudah sesuai dengan requirement yang ada. Dengan adanya testing, kita menjadi lebih yakin bahwa software yang ada bekerja dengan baik dan tidak ada bug. Terdapat dua tipe testing yaitu manual testing dan automated testing. Manual testing biasanya dilakukan bersamaan dengan development itu sendiri. Dalam kasus pembuatan API kita dapat menggunakan Postman atau menggunakan logger

Pada automated testing, kita menulis test code yang menjelaskan behavior yang kita harapkan. Hal ini sangat berguna apabila aplikasi kita bertambah besar yang mengakibatkan manual testing menjadi tidak praktis. Untuk alur dari automated testing sendiri adalah sebagai berikut:

1. Import function/component yang akan dites
2. Beri input ke fungsi tersebut
3. Definisikan output yang diharapkan
4. Cek apakah fungsi memberikan output yang diharapkan

### Instalasi

Untuk melakukan testing di Javascript, kita dapat menggunakan package Jest. Jest adalah salah satu package testing terpopuler yang dibuat oleh Facebook. Untuk menginstall Jest, jalankan command berikut

```bash
npm install --save-dev jest
```

Setelah itu, tambahkan script untuk test pada package.json

```json
"scripts": {
  "start": "nodemon ./src/app.js",
  "test": "jest"
},
```

### Struktur Test

Pertama, kita dapat menggunakan **describe** block untuk mengelompokkan beberapa test menjadi satu test suite

```jsx
describe("Sum function", () => {
  // test code
});
```

Describe block menerima dua argumen. Yang pertama adalah string yang berisi nama dari test suite dan yang kedua adalah fungsi yang membungkus test kita.

Selanjutnya, ada **test** block yang berisi kode dari test kita

```jsx
describe("Sum function", () => {
  test("1 + 2 should return 3", () => {
    // actual test
  });
});
```

### Menulis Test

Misal kita mempunyai fungsi sum yang tugasnya menambahkan dua argumen yang kita berikan

```jsx
// utils/sum.js
const sum = (a, b) => a + b;
```

Untuk membuat test dari fungsi sum, buat file **sum.test.js**.

```jsx
// utils/sum.test.js
const sum = require("./sum");

describe("Sum function", () => {
  test("1 + 2 should return 3", () => {
    const result = sum(1, 2);
    expect(result).toEqual(3);
  });
});
```

Pertama, kita import fungsi sum, lalu memberikan input ke fungsi tersebut. Untuk melakukan pengecekan kita dapat menggunakan **expect**. Expect ini nanti akan mengembalikan expectation object yang dapat memanggil matchers untuk melakukan pengecekan. Disini kita menggunakan matchers **toEqual** yang membandingkan result dengan nilai 3. Kamu dapat melihat list dari matchers yang dapat digunakan dalam Jest [disini](https://jestjs.io/docs/expect)

### Unit Testing

![Untitled](Pertemuan%20%20c26f6/Untitled%202.png)

Terdapat beberapa jenis testing dalam pembuatan aplikasi seperti unit test, integration test, dan end-to-end test. Di modul ini kita hanya akan mempelajari unit testing. Pada unit testing, kita akan menguji bagian terkecil pada aplikasi kita dalam hal ini yaitu service

```jsx
const TaskService = require("./task.service");

describe("Task service", () => {
  const task = {
    name: "Soal shift sisop",
    isCompleted: false,
  };

  test("Create should return input data with id", async () => {
    const taskServiceInstance = new TaskService();

    const result = await taskServiceInstance.create(task);

    expect(result).toEqual({
      id: expect.any(String),
      ...task,
    });
  });
});
```

Pada code diatas, kita akan menguji apakah fungsi create akan mengembalikan data task lengkap dengan atribut id. Pada test block, kita akan membuat instance baru dari TaskService dan memanggil fungsi create dengan argumen variable task yang telah didefinisikan di describe block. Hasil kembalian dari fungsi create akan kita uji dengan object baru yang berisi data yang sama disertai id. Pada bagian id, karena isinya adalah random string, kita dapat menggunakan expect.any(String). Untuk melakukan pengujian jalankan command berikut

```bash
npm run test
```

Namun, saat menjalankan command tersebut, test kita masih gagal dan muncul error seperti ini

![Untitled](Pertemuan%20%20c26f6/Untitled%203.png)

Hal ini terjadi karena dalam TaskService, kita memanggil method Task.create() dari mongoose padahal kita belum menghubungkan mongoose dengan database MongoDB kita. Selain itu, dalam unit testing ini kita tidak perlu menyimpan data ke database sungguhan. Untuk mengatasi error tersebut, kita akan menggunakan mock function.

### Mock Function

Mock function membuat kita dapat menguji suatu hubungan antar code dengan menghapus/meniru implementasi dari suatu fungsi serta mencatat pemangglan ke suatu fungsi beserta parameternya. Tujuan dari mock function ini adalah mengganti hal yang berada di luar kendali kita seperti fungsi dari mongoose

Untuk membuat mock function, kita dapat menggunakan **jest.fn()**. Setelah itu kita dapat mengubah baik implementasi ataupun return value dari fungsi tersebut menggunakan **.mockImplementation()** atau **.mockReturnValue()**. Metode lain dapat kalian lihat [disini](https://jestjs.io/docs/mock-function-api)

```jsx
Task.create = jest.fn().mockResolvedValue({
  id: "123",
  ...task,
});
```

Pada code diatas, kita melakukan mocking pada fungsi **Task.create** dan mengubah resolved valuenya menjadi object dengan tambahan property id berisi string. Test block kita menjadi seperti berikut:

```jsx
test("Create should return input data with id", async () => {
  const taskServiceInstance = new TaskService();

  Task.create = jest.fn().mockResolvedValue({
    id: "123",
    ...task,
  });

  const result = await taskServiceInstance.create(task);

  expect(Task.create).toHaveBeenCalledTimes(1);
  expect(result).toEqual({
    id: expect.any(String),
    ...task,
  });
});
```

Disini kita menambahkan satu test lagi untuk memastikan bahwa fungsi **Task.create** hanya dipanggil satu kali saja. Jalankan test script lagi, sekarang akan terlihat pesan bahwa test berhasil dijalankan

![Untitled](Pertemuan%20%20c26f6/Untitled%204.png)

_const_ Task = require('../models/Task');

_const_ TaskService = require('./task.service');

describe('Task service', () => {

_const_ task = {

name: 'Soal shift sisop',

isCompleted: false,

};

test('Create should return input data with id', _async_ () => {

_const_ taskServiceInstance = new TaskService();

jest.spyOn(Task, 'create').mockImplementation((data) => ({

id: '123',

**...**data,

}));

_const_ result = _await_ taskServiceInstance.create(task);

expect(result).toEqual({

id: '123',

**...**task,

});

});

});

## Dokumentasi

Dokumentasi merupakan bagian yang penting dari sebuah API. Dengan adanya dokumentasi, orang bisa lebih memahami apa yang dilakukan suatu endpoint dan bagaimana cara melakukan request yang benar. Terdapat beberapa tools yang dapat kita gunakan untuk membuat API Documentation seperti Swagger dan Postman. Pada modul ini, kita akan menggunakan Postman

### Membuat Collection

Collection adalah kumpulan-kumpulan dari request yang kita buat. Untuk membuat collection baru, klik tombol plus seperti gambar dibawah

![Untitled](Pertemuan%20%20c26f6/Untitled%205.png)

Setelah itu beri nama pada collection yang dibuat

![Untitled](Pertemuan%20%20c26f6/Untitled%206.png)

Lalu tambahkan deskripsi dari API kalian di bagian kanan dengan menekan tombol dengan ikon pensil

![Untitled](Pertemuan%20%20c26f6/Untitled%207.png)

Buatlah beberapa request di collection tersebut dengan cara klik tombol dengan ikon titik tiga lalu “Add request”

![Untitled](Pertemuan%20%20c26f6/Untitled%208.png)

### Menambah Contoh Response

Response juga merupakan aspek yang penting dalam suatu API documentation karena dengan adanya hal tersebut, pengguna API dapat memahami data apa saja yang didapat dari endpoint tersebut dan bagaimana strukturnya. Untuk menambahkan response ke dokumentasi kita, kita dapat menggunakan tombol Save Response → Save as example

![Untitled](Pertemuan%20%20c26f6/Untitled%209.png)

### Publish

Terakhir, kita perlu mem-publish dokumentasi kita agar bisa diakses melalui browser. Pertama klik “View complete collection documentation” untuk melihat dokumentasi lengkap dari API kita

![Untitled](Pertemuan%20%20c26f6/Untitled%2010.png)

Setelah itu klik Publish

![Untitled](Pertemuan%20%20c26f6/Untitled%2011.png)

Kita akan diarahkan ke halaman web dimana kita dapat mengubah release tag, url, dan style dari dokumentasi kita

![Untitled](Pertemuan%20%20c26f6/Untitled%2012.png)

Setelah selesai mengubah pengatura, klik Publish di bawah halaman

![Untitled](Pertemuan%20%20c26f6/Untitled%2013.png)

Jika proses publish selesai, akan muncul halaman seperti berikut. Kunjungi URL yang tertera untuk melihat dokumentasi yang sudah dibuat

![Untitled](Pertemuan%20%20c26f6/Untitled%2014.png)

## Referensi

- [https://dev.to/santypk4/bulletproof-node-js-project-architecture-4epf](https://dev.to/santypk4/bulletproof-node-js-project-architecture-4epf)
- [https://jestjs.io/](https://jestjs.io/)
- [https://learning.postman.com/docs/publishing-your-api/documenting-your-api/](https://learning.postman.com/docs/publishing-your-api/documenting-your-api/)
