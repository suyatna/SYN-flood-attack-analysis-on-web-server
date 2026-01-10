# SYN flood network attack analysis

## 📑 Table of contents

1. [Introduction](#introduction)
2. [Incident scenario](#scenario)
3. [Objective](#objective)
4. [Analyze network attack](#report)
5. [Conclusion](#conclusion)

---

## 👋 Introduction <a name="introduction">

Studi kasus ini dibuat sebagai bagian dari latihan cybersecurity yang berfokus pada pemahaman gangguan keamanan jaringan pada layanan website. Skenario yang digunakan menggambarkan situasi ketika pelanggan dan karyawan mengalami kesulitan mengakses website akibat aktivitas jaringan yang tidak berjalan normal.

Latihan ini disusun berdasarkan pembelajaran dalam program Google Cybersecurity Professional Certificate, khususnya pada course Connect and Protect: Networks and Network Security. Pembahasan difokuskan pada upaya mengenali jenis serangan jaringan yang mungkin terjadi, memahami dampaknya terhadap kinerja website, serta melihat pengaruhnya terhadap operasional organisasi secara keseluruhan.

---

## 💭 Incident scenario <a name="scenario">

Pada suatu sore, sistem pemantauan mendeteksi gangguan pada server web yang digunakan untuk menampilkan informasi penjualan dan promosi perjalanan. Beberapa karyawan melaporkan website tidak bisa dibuka dan browser menampilkan pesan connection timeout saat halaman diakses.

Sebagai analis keamanan, saya mencoba mengakses website untuk memastikan laporan tersebut dan menemukan kendala yang sama. Pemantauan jaringan menunjukkan lonjakan trafik menuju server web dalam waktu singkat. Pola ini tidak wajar dan berasal dari alamat IP yang tidak dikenal, membuat server kewalahan dan gagal merespons permintaan dari pengguna yang sah.

Kondisi ini langsung menghambat aktivitas karyawan yang mengandalkan website untuk melayani pelanggan. Temuan awal mengarah pada dugaan serangan jaringan yang menargetkan ketersediaan layanan web, sehingga diperlukan analisis lanjutan untuk memastikan jenis serangan dan dampaknya terhadap sistem.

Berikut adalah log TCP dan HTTP yang direkam menggunakan Wireshark:

<img width="824" height="1890" alt="image" src="https://github.com/user-attachments/assets/65f6341f-486d-41c5-ab11-8640dd2aac93" />

---

## 🎯 Objective <a name="objective">

Studi kasus ini disusun untuk memahami dan menganalisis gangguan jaringan yang berdampak langsung pada akses website. Fokus utama berada pada upaya menelusuri sumber masalah melalui pengamatan pola lalu lintas jaringan selama insiden berlangsung.

Tujuan analisis meliputi:
- Mengidentifikasi jenis serangan jaringan berdasarkan pola trafik TCP dan HTTP yang terpantau
- Menganalisis karakteristik lalu lintas jaringan yang tidak normal serta dampaknya terhadap kinerja server web
- Menjelaskan bagaimana serangan tersebut memicu gangguan akses website hingga terjadi koneksi timeout
- Memahami dampak serangan jaringan terhadap operasional organisasi dan aktivitas karyawan
- Menyusun dasar analisis untuk menentukan langkah penanganan dan pencegahan insiden di masa mendatang

---

## 📋 Analyze network attack <a name="report">

### Bagian 1: Identifikasi jenis serangan

Hasil pengamatan lalu lintas jaringan menunjukkan bahwa gangguan akses website yang ditandai dengan pesan connection timeout mengarah pada serangan Denial of Service (DoS). Trafik memperlihatkan lonjakan permintaan TCP SYN dalam jumlah besar yang datang dari alamat IP tidak dikenal dalam waktu singkat.

Permintaan koneksi tersebut tidak pernah diselesaikan melalui proses TCP secara normal. Server web terus menerima paket SYN tanpa adanya penyelesaian koneksi hingga tahap akhir, sehingga resource server terkuras. Pola ini sesuai dengan karakteristik serangan SYN Flood, di mana server dibanjiri permintaan koneksi palsu untuk mengganggu ketersediaan layanan.

### Bagian 2: Cara serangan menyebabkan gangguan website

Akses ke server web pada kondisi normal selalu diawali dengan proses TCP three-way handshake. Client mengirim paket SYN sebagai permintaan koneksi. Server membalas dengan SYN-ACK sebagai tanda persetujuan dan menyiapkan resource. Client kemudian mengirim ACK untuk menyelesaikan koneksi.
- Serangan SYN Flood terjadi saat penyerang mengirim paket SYN dalam jumlah besar tanpa melanjutkan proses handshake hingga selesai.
- Server tetap merespons setiap permintaan tersebut dengan menyediakan resource, meskipun koneksi tidak pernah terbentuk sepenuhnya.
- Kapasitas server pun cepat habis dan tidak mampu melayani koneksi baru.

Log jaringan menunjukkan server web akhirnya gagal merespons permintaan dari pengguna yang sah. Koneksi tidak pernah terbentuk dengan baik dan pengguna menerima pesan connection timeout saat mengakses website. Kondisi ini berdampak langsung pada ketersediaan layanan dan menghambat aktivitas operasional yang bergantung pada website.

---

## 🏁 Conclusion <a name="conclusion">

Hasil analisis lalu lintas jaringan menunjukkan bahwa gangguan akses website dipicu oleh serangan Denial of Service dengan metode SYN Flood. Server web dibanjiri permintaan SYN dalam jumlah besar hingga proses pembentukan koneksi TCP terganggu. Kondisi ini membuat resource server cepat habis dan tidak mampu melayani koneksi dari pengguna yang sah.

Insiden ini memperlihatkan bahwa lonjakan trafik yang tidak wajar dapat langsung memengaruhi ketersediaan layanan web dan aktivitas kerja karyawan. Pemahaman terhadap pola serangan melalui analisis trafik jaringan membantu mengidentifikasi jenis serangan beserta dampaknya. Temuan ini menjadi dasar bagi organisasi untuk memperkuat keamanan jaringan dan mencegah kejadian serupa di kemudian hari.
