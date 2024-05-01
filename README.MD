# Tutorial 9 - Advanced Programming - High Level Networking
## Refleksi

### Nomor 1
> Apa beda utama antara unary, server streaming, dan bidirectional streaming dalam Remote Procedure Call (RPC), dan kapan sebaiknya masing-masing digunakan?

#### Unary RPC
Unary RPC melibatkan pengiriman satu request dan menerima satu response dari server. Kelebihannya terletak pada kesederhanaan dan kemudahan implementasi. Namun, tidak efisien untuk pengiriman data besar atau streaming yang berkelanjutan. Biasanya digunakan untuk mengambil data spesifik atau melakukan otentikasi pengguna.

#### Server Streaming RPC
Pada server streaming RPC, server mengirim serangkaian response kepada client sebagai respon atas satu request. Cocok untuk pengiriman data besar atau streaming yang berkelanjutan. Namun, client hanya dapat mengirimkan satu request pada awalnya. Misalnya digunakan untuk streaming data seperti streaming video atau pembaruan cuaca.

#### Client Streaming RPC
Dalam client streaming RPC, client mengirim serangkaian request ke server, kemudian server mengirim satu response setelah semua pesan diterima. Ini memungkinkan client untuk terus-menerus mengirim data dalam satu sesi. Namun, server harus menunggu semua data diterima sebelum mengirim respons. Berguna ketika client perlu mengirim data dalam jumlah besar secara bertahap, seperti upload file besar.

#### Bidirectional Streaming RPC
Pada tipe ini, baik client maupun server dapat saling mengirim serangkaian pesan secara bergantian. Memungkinkan komunikasi dua arah yang interaktif dan real-time. Namun, lebih kompleks dalam implementasi dan manajemennya. Digunakan dalam aplikasi chat, analitik real-time, dan permainan interaktif.
### Nomor 2
> Apa saja yang perlu diperhatikan dalam implementasi layanan gRPC di Rust, terutama terkait dengan keamanan seperti otentikasi, otorisasi, dan enkripsi data?

Pertama, pastikan koneksi antara client dan server terjamin aman dengan menggunakan TLS untuk mengenkripsi data saat transit. Kedua, terapkan mekanisme otentikasi, seperti token JWT, untuk memverifikasi identitas pengguna. Ketiga, otorisasi perlu diterapkan untuk mengontrol akses pengguna terhadap sumber daya yang tepat. Terakhir, pastikan semua data yang dikirim antara client dan server dienkripsi untuk menjaga kerahasiaan dan integritas data.

### Nomor 3
> Apa tantangan yang mungkin muncul saat menangani streaming bidirectional dalam gRPC Rust, khususnya dalam skenario seperti aplikasi obrolan?

Tantangannya termasuk sinkronisasi pesan antara client dan server untuk mencegah kekacauan atau putusnya koneksi saat mentransmisikan data. Selain itu, manajemen buffering juga penting; penggunaan buffer yang terlalu banyak dapat menyebabkan penundaan yang tidak diinginkan, sementara penggunaan buffer yang terlalu sedikit dapat membebani kinerja server. Pengelolaan koneksi dari banyak client yang berbeda juga menjadi masalah, terutama dalam aplikasi obrolan yang sibuk. Terakhir, penanganan kesalahan koneksi yang tidak terduga juga penting untuk menjaga keamanan dan integritas data.

### Nomor 4
> Apa kelebihan dan kekurangan menggunakan `tokio_stream::wrappers::ReceiverStream` untuk streaming respons dalam layanan gRPC Rust?

**Kelebihan:**
- Fleksibilitas dalam mengonversi `tokio::sync::mpsc::Receiver` menjadi stream yang dapat digunakan dalam implementasi gRPC.
- Kemudahan penggunaan dan integrasi dengan fitur dan alat lain dari `tokio`.

**Kekurangan:**
- Implementasi yang memblokir operasi dapat mempengaruhi kinerja keseluruhan aplikasi.
- Tidak cocok untuk throughput yang tinggi karena keterbatasan dalam menangani throughput yang tinggi.
- Meskipun mudah digunakan, kompleks bagi pengembang yang tidak terbiasa dengan `tokio`.

### Nomor 5
> Bagaimana struktur kode gRPC Rust dapat disusun untuk memfasilitasi penggunaan kembali dan modularitas, serta meningkatkan kemudahan pemeliharaan dan ekstensibilitas dari waktu ke waktu?

Pertama, pisahkan definisi protokol gRPC dan implementasi layanan gRPC menggunakan protobuf. Ini memungkinkan antarmuka yang jelas antara client dan server, sehingga meningkatkan penggunaan kembali kode. Selanjutnya, pisahkan kode ke dalam modul yang berbeda, misalnya, modul untuk definisi layanan dan modul untuk logika bisnis terkait. Ini akan meningkatkan modularitas dan kemudahan pemeliharaan karena perubahan dapat dibatasi hanya pada bagian tertentu dari kode.

### Nomor 6
> Apa langkah tambahan yang mungkin diperlukan dalam implementasi MyPaymentService untuk menangani logika pemrosesan pembayaran yang lebih kompleks?

Langkah tambahan mungkin meliputi validasi data yang lebih ketat, implementasi logika bisnis yang lebih kompleks seperti manajemen inventaris dan perhitungan harga yang dinamis, serta manajemen transaksi yang lebih canggih seperti pengiriman data secara bertahap melalui streaming gRPC.

### Nomor 7
> Bagaimana penggunaan gRPC sebagai protokol komunikasi mempengaruhi arsitektur dan desain sistem terdistribusi secara keseluruhan, terutama dalam hal interoperabilitas dengan teknologi dan platform lain?

P

enggunaan gRPC memungkinkan komunikasi antara layanan yang ditulis dalam bahasa pemrograman yang berbeda melalui protobuf. Ini memfasilitasi interoperabilitas antar platform dan teknologi, karena protokol komunikasi yang sama digunakan di seluruh sistem. Selain itu, gRPC dapat mempermudah integrasi dengan platform lain yang mendukung protobuf, seperti Android dan JavaScript, tanpa memerlukan penyesuaian yang rumit.

### Nomor 8
> Apa keuntungan dan kerugian menggunakan HTTP/2, protokol yang mendasari gRPC, dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk API REST?

**Keuntungan:**
- Dukungan untuk multiplexing dan streaming data dalam dua arah, meningkatkan efisiensi dan responsivitas.
- Header compression mengurangi overhead jaringan dan mempercepat waktu muat halaman.
- Mendukung penggunaan Protocol Buffers untuk serialisasi data, menawarkan format data yang lebih efisien.

**Kerugian:**
- Lebih kompleks untuk diimplementasi daripada HTTP/1.1.
- Tidak semua server mendukungnya sepenuhnya.
- Kompatibilitas dengan perangkat atau browser lama mungkin terbatas.
- Fitur yang disediakan mungkin terlalu berlebihan untuk aplikasi sederhana.

### Nomor 9
> Bagaimana model permintaan-respons dari API REST berbeda dengan kemampuan streaming bidireksional gRPC dalam hal komunikasi real-time dan responsivitas?

Dengan gRPC, baik client maupun server dapat mengirim dan menerima pesan secara asinkron, meningkatkan responsivitas dalam komunikasi real-time seperti aplikasi obrolan. Sementara itu, API REST menggunakan model permintaan-respons yang lebih sederhana, yang mungkin tidak seefisien gRPC untuk komunikasi real-time karena harus menunggu respons dari setiap permintaan.

### Nomor 10
> Apa implikasi dari pendekatan berbasis skema gRPC menggunakan Protocol Buffers, dibandingkan dengan sifat yang lebih fleksibel, tanpa skema JSON dalam muatan API REST?

Pendekatan berbasis skema gRPC dengan Protocol Buffers memastikan konsistensi dan validasi data yang ketat, karena skema data didefinisikan di awal. Di sisi lain, JSON dalam API REST lebih fleksibel dan tidak memerlukan skema yang ketat, memungkinkan perubahan struktur data tanpa perlu memperbarui skema. Meskipun fleksibel, ini dapat menyebabkan inkonsistensi dalam format data antara client dan server.