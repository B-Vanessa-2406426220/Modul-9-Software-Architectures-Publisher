# Reflection

> How much data your publisher program will send to the message broker in one run?

Dalam satu kali eksekusi, program publisher ini akan mengirimkan 5 pesan (events) ke message broker. Hal ini terlihat di dalam fungsi main(), di mana pemanggilan fungsi p.publish_event(...) dilakukan sebanyak 5 kali secara berurutan. Pada setiap pemanggilannya, program tidak sekadar mengirimkan sinyal kosong, melainkan menginisialisasi dan mengirimkan objek UserCreatedEventMessage yang memuat state data unik untuk setiap penggunanya. Masing-masing objek membawa data state yang unik dan spesifik, dimulai dari ID 1 hingga 5, yang merepresentasikan entitas pengguna yang berbeda (yaitu Amir, Budi, Cica, Dira, dan Emir). Sebelum ditransmisikan ke dalam jaringan menuju antrean RabbitMQ, setiap data pengguna tersebut terlebih dahulu diserialisasi menggunakan library Borsh agar diubah menjadi format byte array yang standar. Rangkaian proses ini secara jelas mendemonstrasikan bagaimana aplikasi pengirim dapat terus-menerus memproduksi dan mendelegasikan workload pesan tanpa harus menunggu konfirmasi pemrosesan dari sisi penerima.

> The url of: "amqp://guest:guest@localhost:5672" is the same as in the subscriber program, what does it mean?

Penggunaan URL amqp://guest:guest@localhost:5672 yang sama persis pada publisher dan subscriber adalah syarat agar komunikasi asinkron ini berhasil. Dalam arsitektur message queuing, pengirim dan penerima pesan bekerja secara terpisah dan tidak saling berinteraksi secara langsung. Oleh karena itu, URL tersebut berfungsi sebagai titik temu terpusat yang disepakati oleh kedua program. Alamat ini memastikan keduanya terhubung ke server RabbitMQ lokal yang sama melalui port standar 5672. Publisher menggunakan alamat ini sebagai tempat tujuan untuk menitipkan pesan-pesannya ke dalam antrean broker. Sebagai pasangannya, subscriber wajib memantau alamat yang sama persis agar bisa mengambil pesan yang telah dititipkan tersebut. Jika URL-nya sedikit saja berbeda, pesan dari publisher hanya akan menumpuk di antrean broker karena subscriber memantau di tempat yang salah.

## Running RabbitMQ
![/running-RabbitMQ](./assets/running-RabbitMQ.jpg)