## Experiment 1.2: Understanding How It Works

![hasil](timer-tutorial/static/exp1-2.png)

"hey hey!" muncul pertama meskipun ditulis setelah `spawner.spawn(...)`.
Ini terjadi karena `spawn()` hanya mendaftarkan task ke antrian, belum
mengeksekusinya. Eksekusi baru terjadi saat `executor.run()` dipanggil.
Jadi urutan sebenarnya: spawn task -> cetak "hey hey!" -> drop -> executor
jalan -> "howdy!" -> tunggu 2 detik -> "done!".

## Experiment 1.3: Multiple Spawn and Removing Drop

### Multiple Spawn
![multiple spawn](timer-tutorial/static/exp1-3a.png)

Dengan 3 spawn, executor menjalankan ketiga task secara interleaved.
Semua "howdy" muncul duluan, lalu setelah timer selesai semua "done"
muncul. Satu thread bisa menangani banyak task tanpa membuat thread baru.

### Tanpa drop(spawner)
![tanpa drop](timer-tutorial/static/exp1-3b.png)

Program hang dan tidak selesai. `drop(spawner)` berfungsi sebagai sinyal
bahwa tidak ada task baru lagi. Tanpa itu, executor terus menunggu di
`recv()` selamanya karena channel masih terbuka.

## Experiment 2.1: Original Code

### Cara Menjalankan
```bash
# Terminal 1
cargo run --bin server

# Terminal 2, 3, 4 (masing-masing)
cargo run --bin client
```

![hasil](chat/static/exp2-1.png)

Ketika salah satu client mengetik pesan, server menerima lalu
mem-broadcast ke semua client yang terhubung. Setiap koneksi
ditangani oleh `tokio::spawn` tersendiri sehingga server bisa
melayani banyak client sekaligus tanpa blocking.