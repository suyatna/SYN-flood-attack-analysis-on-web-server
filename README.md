# SYN flood attack analysis on web server

## 📌 Table of contents

1. [Background](#background)
2. [Incident overview](#scenario)
3. [Objective of analysis](#objective)
4. [Detailed attack analysis](#analysis)
5. [Insights and lessons learned](#insight)

---

## 🧠 Background <a name="background">

Studi kasus ini saya buat sebagai latihan cybersecurity dengan fokus memahami gangguan keamanan jaringan pada layanan website. Skenario yang digunakan menggambarkan kondisi saat pelanggan dan karyawan kesulitan mengakses website karena aktivitas jaringan tidak berjalan normal.

Latihan ini disusun berdasarkan materi di program Google Cybersecurity Professional Certificate, khususnya pada course Connect and Protect: Networks and Network Security. Pembahasan dimulai dengan mengenali jenis serangan jaringan yang mungkin terjadi, memahami dampaknya ke performa website, dan melihat pengaruhnya ke operasional organisasi secara keseluruhan.

---

## 🚨 Incident overview <a name="scenario">

Saya bertugas sebagai analis keamanan siber di sebuah agen perjalanan yang mengandalkan website perusahaan untuk promosi dan paket liburan. Website ini dipakai setiap hari oleh karyawan untuk membantu pelanggan memilih layanan. Suatu sore, sistem monitoring mendeteksi gangguan di server web. Website tidak bisa diakses dan browser menampilkan pesan connection timeout. Kondisi ini mulai menghambat aktivitas operasional.

Peninjauan trafik jaringan menunjukkan lonjakan permintaan TCP SYN dalam jumlah tidak wajar yang berasal dari alamat IP eksternal tidak dikenal. Server web kewalahan menangani permintaan tersebut hingga gagal merespons koneksi normal. Server web dimatikan sementara untuk menstabilkan sistem. Firewall lalu dikonfigurasi untuk memblokir sumber permintaan berlebih. Langkah ini bersifat sementara dan langsung dilaporkan ke manajer untuk menentukan penanganan lanjutan.

Berikut ringkasan log TCP dan HTTP selama insiden:

<img width="824" height="1700" alt="image" src="https://github.com/user-attachments/assets/65f6341f-486d-41c5-ab11-8640dd2aac93" />

---

## 🎯 Objective of analysis <a name="objective">

Studi kasus ini disusun untuk memahami dan menganalisis gangguan jaringan yang berdampak langsung pada akses website. Fokus utama berada pada proses menelusuri sumber masalah lewat pengamatan pola lalu lintas jaringan saat insiden terjadi.

Tujuan analisis meliputi:
- Mengidentifikasi jenis serangan jaringan berdasarkan pola trafik TCP dan HTTP yang terpantau
- Menganalisis ciri lalu lintas jaringan yang tidak normal dan dampaknya terhadap kinerja server web
- Menjelaskan cara serangan tersebut memicu gangguan akses website hingga muncul koneksi timeout
- Memahami dampak serangan jaringan terhadap operasional organisasi dan aktivitas karyawan
- Menyusun dasar analisis untuk menentukan langkah penanganan dan pencegahan insiden ke depan

---

## 🔍 Detailed attack analysis <a name="analysis">

### a. Attack identification

Hasil pengamatan lalu lintas jaringan menunjukkan gangguan akses website dengan pesan connection timeout mengarah ke serangan Denial of Service (DoS). Trafik terlihat melonjak dengan banyak permintaan TCP SYN yang masuk dalam waktu singkat. Permintaan tersebut berasal dari alamat IP tidak dikenal.

Permintaan koneksi tidak pernah diselesaikan secara normal. Server terus menerima paket SYN tanpa kelanjutan proses TCP sampai selesai. Resource server terkuras karena koneksi dibiarkan setengah terbuka. Pola ini sesuai dengan karakteristik serangan SYN flood, di mana server dibanjiri permintaan koneksi palsu sampai layanan tidak tersedia.

### b. Impact on server

Akses normal ke server web dimulai dengan proses TCP three-way handshake. Client mengirim SYN sebagai permintaan koneksi. Server membalas dengan SYN-ACK sambil menyiapkan resource. Client kemudian mengirim ACK untuk menyelesaikan koneksi.
- Serangan SYN flood terjadi saat penyerang mengirim paket SYN dalam jumlah besar tanpa melanjutkan handshake.
- Server tetap merespons setiap permintaan dan mengalokasikan resource meskipun koneksi tidak pernah terbentuk penuh.
- Kapasitas server cepat habis dan tidak mampu menerima koneksi baru.

Log jaringan menunjukkan server web akhirnya gagal merespons permintaan pengguna yang sah. Koneksi tidak pernah terbentuk dengan baik dan pengguna menerima pesan connection timeout saat mengakses website. Kondisi ini langsung berdampak pada ketersediaan layanan dan mengganggu aktivitas operasional yang bergantung pada website.

---

## 💡 Insights and lessons learned <a name="insight">

Studi kasus ini menunjukkan serangan SYN flood bisa langsung mengganggu ketersediaan layanan web tanpa menyentuh sisi aplikasi. Pola trafik berisi paket SYN yang tidak pernah menyelesaikan three way handshake menjadi tanda kuat adanya serangan Denial of Service (DoS).

Analisis lalu lintas jaringan memperlihatkan serangan memanfaatkan keterbatasan server dalam menangani koneksi setengah terbuka atau half open connection. Penumpukan koneksi ini membuat resource server cepat habis dan tidak mampu merespons permintaan pengguna yang sah. Layanan terasa lambat bahkan tidak bisa diakses sama sekali.

Kasus ini menunjukkan pentingnya network monitoring dan analisis log sebagai langkah awal mendeteksi serangan berbasis trafik. Pemahaman pola TCP yang normal dan menyimpang membantu insiden dikenali lebih cepat sebelum dampaknya melebar ke sistem lain.

Hasil analisis menegaskan serangan terhadap availability tidak selalu rumit. Dampaknya bisa besar jika tidak diantisipasi. Mitigasi seperti konfigurasi firewall yang tepat, penerapan rate limiting, dan penggunaan intrusion detection system menjadi hal yang perlu diperhatikan.
