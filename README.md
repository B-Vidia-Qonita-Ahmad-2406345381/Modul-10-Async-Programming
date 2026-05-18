## Experiment 1.2: Understanding How It Works

![hasil](timer-tutorial/static/exp1-2.png)

"hey hey!" muncul pertama meskipun ditulis setelah `spawner.spawn(...)`.
Ini terjadi karena `spawn()` hanya mendaftarkan task ke antrian, belum
mengeksekusinya. Eksekusi baru terjadi saat `executor.run()` dipanggil.
Jadi urutan sebenarnya: spawn task -> cetak "hey hey!" -> drop -> executor
jalan -> "howdy!" -> tunggu 2 detik -> "done!".
