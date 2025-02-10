## 1. DNS

# Pengertian

Domain Name System atau DNS adalah sistem yang menerjemahkan nama domain seperti www.google.com menjadi IP address (contoh: 192.0.0.1) agar bisa dipahami oleh komputer saat Anda mengakses sebuah website menggunakan nama domain.

# Cara Kerja

DNS bekerja dalam beberapa langkah menggunakan proses yang disebut DNS lookup atau resolution.

Misalnya saya ingin membuka website tokopedia . Anda kemudian mengetikkan nama domain www.tokopedia.com ke kolom alamat web browser. Nah, di sini saya sedang melakukan proses yang disebut **DNS Request (Permintaan DNS)**.

Komputer saya lalu akan mengecek penyimpanan lokalnya untuk mencari apakah ada record (data) untuk domain tersebut. DNS record adalah IP address yang terkait dengan **FQDN**.

> FQDN adalah nama domain lengkap yang menentukan lokasi komputer atau host dalam jaringan, termasuk di internet maupun jaringan lokal. Struktur FQDN terdiri dari hostname dan nama domain.
>
> ![fqdn](/img/fqdn.webp)
>
> - Hostname. Label yang ditetapkan pada perangkat atau layanan di jaringan. Hostname merupakan bagian dari nama domain yang membuat alamat IP jadi mudah diingat. Contohnya, “www” adalah hostname untuk www.hostinger.co.id, dan “en” adalah hostname untuk en.wikipedia.org.
>
> - Subdomain. Bagian ini terletak di sisi kiri domain utama, dan terkadang menunjukkan bagian dari domain yang lebih besar. Misalnya, support.hostinger.com memiliki subdomain “support” yang merupakan bagian dari hostinger.com. Namun, tidak semua domain memiliki elemen ini.
>
> - Nama domain. Terdiri dari domain tingkat kedua dan top-level domain (TLD). Misalnya, di hostinger.co.id, “hostinger” adalah domain tingkat kedua, dan “.co.id” adalah TLD.

Selanjutnya komputer akan mencari dalam file host dan cache. File host adalah file teks biasa yang mengarahkan hostname ke IP address dalam sistem operasi, sedangkan cache adalah data sementara yang disimpan oleh hardware atau software.

begini gambaran kerja dns dalam mengakses website:

![alt](/img/cara-kerja-dns.jpg)

1. DNS Resolver

   perantara utama antara komputer Anda dan DNS server lainnya. Fungsinya adalah untuk meneruskan permintaan ke DNS server lainnya lalu mengirimkannya kembali setelah dipenuhi.

2. Root Nameserver

   Root nameserver atau root DNS server adalah server yang paling tinggi dalam alur kerja DNS. Fungsinya bisa diibaratkan seperti ruang arsip. Setelah menerima permintaan dari recursive DNS resolver, root nameserver akan memeriksa TLD milik domain yang Anda buka. Kemudian, recursive resolver akan diarahkan olehnya ke namaserver TLD yang tepat.

3. TLD Nameserver

   DNS server yang bertugas untuk menyimpan dan mengelola informasi domain yang menggunakan TLD tertentu. Top-level Domain atau TLD adalah bagian akhir domain, seperti .com, .org, .online, dan .net.

4. Authoritative Nameserver

   Authoritative name server atau authoritative DNS server adalah server terakhir dalam proses resolusi DNS. Server ini menyimpan semua informasi yang terkait dengan domain yang Anda kunjungi, termasuk alamat IP.

   Recursiver resolver kemudian akan memperoleh IP address milik domain yang Anda kunjungi, lalu mengirimkannya kembali ke komputer Anda sehingga website yang Anda akses akan terbuka.

   Terakhir, resolver akan melakukan DNS caching, yaitu menyimpan IP address yang berhasil diperolehnya dari authoritative name server sebagai data cache. Jadi, ketika Anda kembali website yang sama, prosesnya bisa lebih singkat karena record bisa langsung diambil dari cache.

# DNS dalam kubernetes

DNS (Domain Name System) dalam Kubernetes adalah fitur yang digunakan untuk memberikan resolusi nama kepada layanan dan pod dalam kluster Kubernetes.

DNS membuat aplikasi dalam kluster saling berkomunikasi menggunakan **nama yang mudah diingat**, bukan alamat IP yang bersifat dinamis.

## Fungsi DNS dalam kubernetes

1. sebagai Resolusi service name

   DNS Kubernetes memungkinkan layanan diakses melalui nama domain seperti my-service.my-namespace.svc.cluster.local, tanpa perlu mengetahui IP address-nya. Nama domain ini mempermudah komunikasi antar-layanan dalam kluster.

2. Resolusi Nama Pod

   Kubernetes DNS juga dapat digunakan untuk mengakses pod secara langsung, terutama jika pod memiliki hostname atau subdomain yang telah dikonfigurasi.

3. Komunikasi Antar-Namespace

   Dengan DNS, Anda dapat mengakses layanan di namespace yang berbeda menggunakan format:

   ```bash
   <service-name>.<namespace>.svc.cluster.local
   ```

<br>
<br>

# Peran DNS dalam Security System

Domain Name System (DNS) adalah sistem yang menerjemahkan nama domain menjadi alamat IP. Dalam konteks keamanan, DNS memiliki peran penting dalam melindungi sistem dari ancaman siber, mencegah akses ke situs berbahaya, dan memastikan komunikasi jaringan yang aman. peran DNS dalam security sistem

## **DNS Filtering**

DNS Filtering adalah proses pemblokiran atau pengizinan akses ke suatu domain berdasarkan aturan tertentu yang diterapkan pada sistem resolusi DNS. Proses ini sering digunakan untuk keamanan siber, kontrol akses, atau pemblokiran konten berbahaya. Berikut adalah tahapan utama dalam proses DNS filtering:

1.  Saat pengguna mencoba mengakses sebuah situs web (misalnya, example.com), perangkat mereka akan mengirimkan permintaan DNS (DNS query) ke server DNS untuk menerjemahkan nama domain menjadi alamat IP.

2.  Permintaan DNS dikirim ke DNS resolver, yang biasanya merupakan server DNS dari ISP atau layanan DNS pihak ketiga seperti Google DNS (8.8.8.8), Cloudflare DNS (1.1.1.1), atau OpenDNS.

3.  Server DNS memiliki daftar aturan atau database yang berisi domain yang diperbolehkan (allow list) dan diblokir (block list).

4.  Jika domain diblokir, resolver akan:

    - Mengembalikan IP palsu yang mengarah ke halaman peringatan (misalnya, "Website ini telah diblokir").
    - Tidak memberikan jawaban (mengembalikan NXDOMAIN atau "domain not found").

    Jika domain diizinkan, resolver akan meneruskan permintaan ke DNS autoritatif dan mengembalikan alamat IP yang benar.

Filtering bisa dilakukan dengan berbagai metode:

- **Blacklisting**: Domain yang berbahaya atau tidak diinginkan diblokir berdasarkan daftar hitam (contoh: situs malware, phishing, atau pornografi).
- **Whitelisting**: Hanya domain tertentu yang diperbolehkan, sementara yang lainnya diblokir.
- **Category-based Filtering**: Menyaring berdasarkan kategori, misalnya memblokir situs perjudian atau media sosial.
- **Threat Intelligence Filtering**: Menggunakan database ancaman terbaru untuk mencegah akses ke domain yang mencurigakan.

DNS Filtering bekerja dengan menyaring permintaan DNS berdasarkan kebijakan keamanan tertentu. Metode ini efektif untuk meningkatkan keamanan, mengurangi ancaman malware, dan mengontrol akses ke konten tertentu, baik di lingkungan rumah, kantor, atau jaringan besar.

## DNS Firewall dan Threat Intelligence

DNS Firewall dan Threat Intelligence adalah dua mekanisme keamanan yang digunakan untuk melindungi jaringan dari ancaman siber dengan cara mencegah akses ke domain berbahaya. Berikut adalah penjelasan cara kerja keduanya:

### Cara Kerja DNS Firewall

DNS Firewall adalah sistem keamanan yang berfungsi untuk memfilter dan memblokir permintaan DNS ke domain yang dianggap berbahaya atau tidak diizinkan. Berikut langkah-langkah cara kerjanya:

1. Pengguna Mengajukan Permintaan DNS

   Saat pengguna mencoba mengakses sebuah situs web, perangkat mereka mengirimkan permintaan DNS ke server resolver untuk menerjemahkan domain menjadi alamat IP.

2. DNS Firewall Mengecek Database Keamanan

   DNS Firewall memiliki database domain berbahaya (blacklist) yang terus diperbarui.
   Database ini bisa berasal dari berbagai sumber, termasuk Threat Intelligence Feeds.
   DNS Firewall akan membandingkan permintaan domain dengan daftar yang ada.

3. Tindakan yang Dilakukan oleh DNS Firewall
   Jika domain terdeteksi sebagai ancaman, maka:

   - Memblokir akses dengan mengembalikan respons NXDOMAIN (domain tidak ditemukan).
   - Mengalihkan pengguna ke halaman peringatan (misalnya, “Situs ini diblokir”).
   - Melakukan logging dan notifikasi ke administrator untuk analisis lebih lanjut.
   - Jika domain tidak berbahaya, DNS resolver akan meneruskan permintaan ke DNS autoritatif untuk mendapatkan alamat IP situs tujuan.

### Cara Kerja Threat Intelligence dalam DNS Firewall

Threat Intelligence berperan dalam mendeteksi dan memblokir ancaman berdasarkan analisis data real-time. Berikut cara kerjanya:

1. Pengumpulan Data Ancaman

   Threat Intelligence mengumpulkan informasi dari berbagai sumber, seperti:

   - Reputasi Domain & IP (misalnya, domain yang sering digunakan untuk phishing).
   - Laporan serangan siber global (DDoS, malware, ransomware).
   - Threat Feeds dari organisasi keamanan (Cisco Talos, FireEye, CrowdStrike, dll.).
   - Honeypot dan Machine Learning untuk mendeteksi pola ancaman baru.

2. Analisis dan Klasifikasi Ancaman

   Data yang dikumpulkan akan dianalisis dengan teknik seperti:

   - Signature-based detection (mencocokkan domain/IP dengan daftar blacklist).
   - Behavioral analysis (mendeteksi pola aktivitas mencurigakan).
   - AI & Machine Learning untuk menemukan ancaman zero-day.

3. Penyebaran Database ke DNS Firewall

   Setelah dianalisis, data ancaman dikirim ke DNS Firewall dalam bentuk Threat Feeds.

   Database ini terus diperbarui secara otomatis
   untuk menangani ancaman terbaru.

4. Pencegahan Ancaman Secara Proaktif

   Saat ada permintaan DNS ke domain berbahaya, DNS Firewall langsung memblokir akses berdasarkan data dari Threat Intelligence.

   Administrator dapat menganalisis log serangan untuk mengidentifikasi pola serangan yang berulang.


## 2. Firewall

# Video Konsep Dasar Firewall

https://www.youtube.com/watch?v=fET2PYkckLo

# Pengertian Firewall

Firewall adalah sistem keamanan jaringan yang berfungsi sebagai penghalang antara jaringan internal (misalnya, jaringan perusahaan atau rumah) dan jaringan eksternal (seperti internet). Firewall bekerja dengan memantau, menyaring, dan mengontrol lalu lintas data berdasarkan aturan keamanan yang telah ditentukan.

## Fungsi Utama Firewall:

1. Mencegah Akses Tidak Sah – Firewall dapat memblokir akses dari pihak yang tidak berwenang ke jaringan internal.
2. Menyaring Lalu Lintas Data – Memeriksa paket data yang masuk dan keluar, serta mengizinkan atau menolak berdasarkan aturan keamanan.
3. Mencegah Serangan Berbahaya – Melindungi jaringan dari serangan seperti DDoS (Distributed Denial of Service), malware, dan hacking.
4. Membantu Manajemen Bandwidth – Dapat digunakan untuk membatasi penggunaan bandwidth untuk aplikasi tertentu.
5. Menerapkan Kebijakan Keamanan – Bisa digunakan untuk mengatur akses internet bagi pengguna dalam suatu jaringan.

# Jenis-Jenis Firewall:

1. Packet Filtering Firewall – Menyaring paket data berdasarkan alamat IP, port, dan protokol.
2. Stateful Inspection Firewall – Melakukan pemeriksaan yang lebih dalam terhadap koneksi yang sedang berlangsung.
3. Proxy Firewall – Bertindak sebagai perantara antara pengguna dan internet untuk menyembunyikan identitas jaringan internal.
4. Next-Generation Firewall (NGFW) – Menggabungkan fitur firewall tradisional dengan kemampuan deteksi ancaman modern seperti Intrusion Prevention System (IPS).

# Cara Kerja Firewall (General)

Firewall bekerja dengan membatasi akses ke jaringan dan sistem berdasarkan kriteria tertentu. Ini dilakukan dengan mengatur aturan dan kebijakan yang menentukan siapa yang dapat mengakses jaringan dan sistem, dan jenis data apa yang diperbolehkan masuk dan keluar dari jaringan.

Contohnya, jika sebuah organisasi ingin memblokir akses ke situs web yang berbahaya atau berisi konten yang tidak pantas, maka firewall dapat diperintahkan untuk memblokir semua akses ke situs web tersebut. Firewall juga dapat membatasi jenis data yang diperbolehkan masuk dan keluar dari jaringan, seperti email atau file yang terkait dengan bisnis organisasi.

Selain itu, firewall dapat memonitor lalu lintas jaringan dan melaporkan aktivitas yang mencurigakan. Jika ada aktivitas yang mencurigakan seperti serangan siber, firewall akan memberikan peringatan dan memblokir akses ke sistem atau jaringan untuk melindungi informasi dan data organisasi.

Firewall dapat diimplementasikan dengan menggunakan perangkat lunak atau perangkat keras, atau kombinasi dari keduanya. Firewall perangkat keras adalah perangkat yang berdiri sendiri dan terhubung ke jaringan, sedangkan firewall perangkat lunak adalah program yang diinstal pada komputer atau server.

# Cara Kerja Firewall (Kubernetes)

Dalam Kubernetes, firewall berfungsi untuk mengontrol lalu lintas jaringan antara **pod, node, dan klien eksternal**. Firewall dapat diterapkan di berbagai level, seperti di tingkat Cloud Provider, Node (host), atau Kubernetes itu sendiri.

1. Firewall di Tingkat Infrastruktur (Cloud Provider / Network Layer)

   Jika Kubernetes berjalan di cloud (misalnya AWS, GCP, atau Azure), biasanya cloud provider menyediakan firewall dalam bentuk Security Groups (AWS), Network Security Groups (Azure), atau VPC Firewall Rules (GCP).

   Firewall ini bekerja dengan menentukan aturan ingress (masuk) dan egress (keluar).

   - Bisa digunakan untuk membatasi akses ke kube-apiserver, worker nodes, dan load balancer.
     Contoh aturan firewall di AWS Security Group:

   - Izinkan port 6443 untuk akses ke kube-apiserver hanya dari IP tertentu.
   - Izinkan hanya lalu lintas internal antar worker nodes.

2. Firewall di Tingkat Node (Host Firewall)

   Setiap node Kubernetes adalah server (VM atau bare metal) yang memiliki firewall bawaan, seperti:

   iptables (Linux)
   firewalld (Linux)
   Windows Defender Firewall (Windows Node)

   Cara kerja:

   - Kubernetes menggunakan kube-proxy yang mengatur iptables untuk mengelola akses antar pod dan layanan.
   - Administrator bisa menambahkan aturan tambahan untuk membatasi akses SSH, kubelet API, atau service tertentu.

   Contoh aturan iptables untuk membatasi akses SSH hanya dari IP tertentu:

   ```bash
   iptables -A INPUT -p tcp --dport 22 -s 192.168.1.100 -j ACCEPT
   iptables -A INPUT -p tcp --dport 22 -j DROP
   ```

3. Firewall di Tingkat Kubernetes (Network Policies)

   Di dalam Kubernetes sendiri, firewall dapat diterapkan dengan Network Policy.

   Cara kerja:

   - Menggunakan CNI (Container Network Interface) seperti Calico, Cilium, atau WeaveNet untuk menerapkan aturan firewall.
   - Network Policy mengontrol komunikasi antar pod berdasarkan label, namespace, IP, atau port.

   Contoh Network Policy untuk membatasi akses hanya dari pod tertentu:

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
   name: allow-from-app
   namespace: default
   spec:
   podSelector:
       matchLabels:
       app: database
   policyTypes:
       - Ingress
   ingress:
       - from:
           - podSelector:
               matchLabels:
               app: backend
       ports:
           - protocol: TCP
           port: 5432
   ```

   Penjelasan:

   - Hanya pod dengan label app=backend yang bisa mengakses pod app=database pada port 5432 (PostgreSQL).
   - Pod lain akan diblokir.

4. Firewall di Tingkat Service (Ingress/Egress Rules)

   Untuk layanan yang diekspos ke publik, Kubernetes menyediakan Ingress Controller dan Egress Rules.

   - Ingress Firewall: Mengontrol akses masuk ke dalam cluster melalui Ingress Controller (misal: Nginx Ingress, Traefik).

   - Egress Firewall: Mengontrol akses keluar dari pod ke internet atau layanan lain.

   Contoh cara kerja Ingress Controller dengan firewall di Kubernetes:

   - Menggunakan Nginx Ingress untuk hanya mengizinkan akses ke domain tertentu.
   - Menggunakan Calico Egress Rules untuk memblokir akses pod ke internet kecuali ke API eksternal tertentu.
