# 1. DNS (Domain Name System)

## 1.1 Teori

### 1.1.A Pengertian

DNS (_Domain Name System_) adalah sistem penamaan untuk semua device (smartphone, computer, atau
network) yang terhubung dengan internet. DNS Server berfungsi menerjemahkan nama domain menjadi alamat IP. DNS dibuat guna untuk menggantikan sistem penggunaan file host yang dirasa tidak efisien.

### 1.1.B Cara Kerja

![DNS](images/1.jpg)

Client akan meminta alamt IP dari suatu domain ke DNS server. Jika pada DNS server data alamat IP dari DNS server tersebut ada maka akan di return alamat IP nya kembali menuju client. Jika DNS server tersebut tidak memiliki alamat IP dari domain tersebut maka dia akan bertanya kepada DNS server yang lain sampai alamat domain itu ditemukan.

### 1.1.C Aplikasi DNS Server

Untuk praktikum jarkom kita menggunakan aplikasi BIND9 sebagai DNS server, karena BIND(Berkley Internet Naming Daemon) adalah DNS server yang paling banyak digunakan dan juga memiliki fitur-fitur yang cukup lengkap.

### 1.1.D List DNS Record
| Tipe          | Deskripsi                     |
| ------------- |:-----------------------------|
| A             | Memetakan nama domain ke alamat IP (IPv4) dari komputer hosting domain|
| AAAA          | AAAA record hampir mirip A record, tapi mengarahkan domain ke alamat Ipv6|
| CNAME         | Alias ​​dari satu nama ke nama lain: pencarian DNS akan dilanjutkan dengan mencoba lagi pencarian dengan nama baru|
| NS            | Delegasikan zona DNS untuk menggunakan authoritative name servers yang diberikan|
| PTR           | Digunakan untuk Reverse DNS (Domain Name System) lookup|
| SOA           | Mengacu server DNS yang mengediakan otorisasi informasi tentang sebuah domain Internet|
| TXT           | Mengijinkan administrator untuk memasukan data acak ke dalam catatan DNS, catatan ini juga digunakan di spesifikasi Sender Policy Framework|

### 1.1.E SOA (Start of Authority)

Adalah informasi yang dimiliki oleh suatu DNS zone.

| Nama          | Deskripsi                     |
| ------------- |:-----------------------------|
| Serial        | Jumlah revisi dari file zona ini. Kenaikan nomor ini setiap kali file zone diubah sehingga perubahannya akan didistribusikan ke server DNS sekunder manapun|
| Refresh       | Jumlah waktu dalam detik bahwa nameserver sekunder harus menunggu untuk memeriksa salinan baru dari zona DNS dari nameserver utama domain. Jika file zona telah berubah maka server DNS sekunder akan memperbarui salinan zona tersebut agar sesuai dengan zona server DNS utama|
| Retry         | Jumlah waktu dalam hitungan detik bahwa nameserver utama domain (atau server) harus menunggu jika upaya refresh oleh nameserver sekunder gagal sebelum mencoba refresh zona domain dengan nameserver sekunder itu lagi|
| Expire        | Jumlah waktu dalam hitungan detik bahwa nameserver sekunder (atau server) akan menahan zona sebelum tidak lagi mempunyai otoritas|
| Minimum       | Jumlah waktu dalam hitungan detik bahwa catatan sumber daya domain valid. Ini juga dikenal sebagai TTL minimum, dan dapat diganti oleh TTL catatan sumber daya individu|
| TTL           | (waktu untuk tinggal) - Jumlah detik nama domain di-cache secara lokal sebelum kadaluarsa dan kembali ke nameserver otoritatif untuk informasi terbaru|



------





## 1.2 Praktik

### 1.2.A Buat Topologi
Pada praktik kali ini, kita akan membangun sebuah dunia dalam jagat **First Age**. **Eru** berperan sebagai penghubung **(router)** yang menjaga alur kehidupan antar-dunia. Sedangkan kekuatan para **Valar** dibagi: **Manwë** akan menjadi penguasa domain (**DNS Master**), sementara **Melkor** menjadi bayangan yang hanya meniru (**DNS Slave**). Klien seperti **Varda** dan **Ulmo** akan menjadi penghuni yang menggunakan layanan ini.

Buat topologi seperti di [pengenalan GNS3](https://github.com/lab-kcks/Modul-Komdat-Jarkom/blob/main/Modul-GNS3#Membuat-Topologi) kemarin.

![topologi](images/topologi-new.png)

Kita akan membuat node `Manwe` sebagai DNS server.

### 1.2.A Instalasi bind

- Buka *Manwe* dan update package lists dengan menjalankan command:

	```
	apt-get update
	```

- Setalah melakukan update silahkan install aplikasi bind9 pada *Manwe* dengan perintah:

	```
	apt-get install bind9 -y
	```
  ![install bind9](images/1-new.png)

- Setelah install jalankan command berikut:
  ```bash
  ln -s /etc/init.d/named /etc/init.d/bind9
  ```
### 1.2.B Pembuatan Domain
Dalam legenda Tolkien, dunia disebut **Arda**. Maka pada tutorial ini kita akan membuat sebuah domain bernama `arda.fa` (Arda First Age), dengan `Manwe` sebagai pusat pengelolaannya (**DNS Master**)..

Jadi kita akan membuat domain **arda.fa**.

- Lakukan perintah pada *Manwe*. Isikan seperti berikut:

  ```
  nano /etc/bind/named.conf.local
  ```

- Isikan configurasi domain **arda.fa** sesuai dengan syntax berikut:

  ```
  zone "arda.fa" {
  	type master;
  	file "/etc/bind/arda/arda.fa";
  };
  ```
  ![config arda.fa](images/2-2-new.png)
<!-- ![config jarkom2022.com](images/2-2.png) -->

- Buat folder **arda** di dalam **/etc/bind**

  ```
  mkdir /etc/bind/arda
  ```

- Dibawah ini Jalankan urut dari A ke B.
  - A. buat file baru bernama **zone.template** pada direktori `/etc/bind/`.
    ```
    nano /etc/bind/zone.template
    ```
    Isi dengan isi berikut:
    ```
    $TTL    604800          ; Waktu cache default (detik)
    @       IN      SOA     localhost. root.localhost. (
                            2025100401 ; Serial (format YYYYMMDDXX)
                            604800     ; Refresh (1 minggu)
                            86400      ; Retry (1 hari)
                            2419200    ; Expire (4 minggu)
                            604800 )   ; Negative Cache TTL
    ;
    
    @       IN      NS      localhost.
    @       IN      A       127.0.0.1
    ```
  
  - B. Copy file `/etc/bind/zone.template` dan rename file hasil copy menjadi `/etc/bind/arda/arda.fa`
    ```
    cp /etc/bind/zone.template /etc/bind/arda/arda.fa
    ``` 

- Kemudian buka file **arda.fa** dan edit seperti gambar berikut dengan IP *Manwe* masing-masing kelompok:

  ```
  nano /etc/bind/arda/arda.fa
  ```
  ![konfig arda.fa](images/3-1-new.png)

- Restart bind9 dengan perintah 

  ```
  service bind9 restart
  
  ATAU
  
  named -g //Bisa digunakan untuk restart sekaligus debugging
  ```



### 1.2.C Setting nameserver pada client

Setiap penghuni Arda (**Varda dan Ulmo**) harus menghadap kepada **Manwe** untuk **bertanya arah**

Domain yang kita buat tidak akan langsung dikenali oleh client oleh sebab itu kita harus merubah settingan nameserver yang ada  kepada client.

- Pada client *Varda* dan *Ulmo* arahkan nameserver menuju IP *Manwe* dengan mengedit file _resolv.conf_ dengan mengetikkan perintah 

	```
	nano /etc/resolv.conf
	```
  ![isi resolv.conf di client](images/4-1-new.png)
<!-- ![ping](images/4-1.png) -->

- Untuk mencoba koneksi DNS, lakukan ping domain **arda.fa** dengan melakukan  perintah berikut pada client *Varda* dan *Ulmo*

  ```
  ping arda.fa -c 5
  ```
  ![ping](images/5-1-new.png)

<!-- ![ping](images/5-1-new.png) -->


### 1.2.D Reverse DNS (Record PTR)

Agar para penghuni mengenal asal mereka, kita membuat catatan balik (**PTR**) untuk setiap entitas: **Eru** (router), **Melkor**, dan **Manwe**. Dengan begitu, siapa pun yang menyebut alamat akan tahu nama tokoh yang sesungguhnya.

Jika pada pembuatan domain sebelumnya DNS server kita bekerja menerjemahkan string domain **arda.fa** kedalam alamat IP agar dapat dibuka, maka Reverse DNS atau Record PTR digunakan untuk menerjemahkan alamat IP ke alamat domain yang sudah diterjemahkan sebelumnya.

- Edit file **/etc/bind/named.conf.local** pada *Manwe*

  ```
  nano /etc/bind/named.conf.local
  ```

- Lalu tambahkan konfigurasi berikut ke dalam file **named.conf.local**. Tambahkan **reverse dari 3 byte awal** dari IP yang ingin dilakukan Reverse DNS. Karena di contoh saya menggunakan IP `10.40.1` untuk IP dari records, maka reversenya adalah `1.40.10` 

  ```
  zone "1.40.10.in-addr.arpa" {
      type master;
      file "/etc/bind/arda/1.40.10.in-addr.arpa";
  };
  ```
  ![zone file reverse](images/6-1-new.png)

- Copykan file **zone.template** pada path **/etc/bind** ke dalam folder **arda** yang baru saja dibuat dan ubah namanya menjadi **1.40.10.in-addr.arpa**

  ```
  cp /etc/bind/zone.template /etc/bind/arda/1.40.10.in-addr.arpa
  ```

  *Keterangan 1.40.10 adalah 3 byte pertama IP Manwe yang dibalik urutan penulisannya*

- Edit file **1.40.10.in-addr.arpa** menjadi seperti gambar di bawah ini

  ![konfig reverse](images/7-1-new.png)
<!-- ![konfig](images/7-1.png) -->

- Kemudian restart bind9 dengan perintah 

  ```
  service bind9 restart
  ```

- Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client *Varda* 

  ```
  // Install package dnsutils
  // Pastikan nameserver di /etc/resolv.conf telah dikembalikan sama dengan nameserver dari Foosha
  apt-get update
  apt-get install dnsutils
  
  //Kembalikan nameserver agar tersambung dengan Minwe
  host -t PTR "IP Manwe"
  ```
  ![host](images/8-1-new.png)
  



### 1.2.E Record CNAME
Kita menambahkan alias agar www.arda.fa tetap menunjuk ke dunia yang sama. Sama seperti dalam mitos, satu dunia bisa dikenal dengan banyak nama, tetapi tetap merujuk pada Arda.

Record CNAME adalah sebuah record yang membuat alias name dan mengarahkan domain ke alamat/domain yang lain.

Langkah-langkah membuat record CNAME:

- Buka file **arda.fa** pada server *Manwe* dan tambahkan konfigurasi seperti pada gambar berikut:


  ![cname](images/9-1-new.png)



- Kemudian restart bind9 dengan perintah

  ```
  service bind9 restart
  ```

- Lalu cek dengan melakukan `host -t CNAME www.arda.fa` atau `ping www.arda.fa -c 5`. Hasilnya harus mengarah ke host dengan IP *Manwe*.

  ![dns](images/10-1-new.png)




### 1.2.F Membuat DNS Slave

DNS Slave adalah DNS cadangan yang akan diakses jika server DNS utama mengalami kegagalan. Kita akan menjadikan server *Melkor* sebagai DNS slave dan server *Manwe* sebagai DNS masternya.

#### I. Konfigurasi Pada Server Manwe

- Edit file **/etc/bind/named.conf.local** dan sesuaikan dengan syntax berikut

  ```
  zone "arda.fa" {
      type master;
      notify yes;
      also-notify { "IP Melkor"; }; // Masukan IP Melkor tanpa tanda petik
      allow-transfer { "IP Melkor"; }; // Masukan IP Melkor tanpa tanda petik
      file "/etc/bind/arda/arda.fa";
  };
  ```

  ![dns master setting](images/11-2-new.png)




- Lakukan restart bind9

  ```
  service bind9 restart
  ```



#### II. Konfigurasi Pada Server Melkor

- Buka *Melkor* dan update package lists dengan menjalankan command:

  ```
  apt-get update
  ```

- Setalah melakukan update silahkan install aplikasi bind9 pada *Melkor* dengan perintah:

  ```
  apt-get install bind9 -y
  ```

- Kemudian buka file **/etc/bind/named.conf.local** pada *Melkor* dan tambahkan syntax berikut:

  ```
  zone "arda.fa" {
      type slave;
      masters { "IP Manwe"; }; // Masukan IP Manwe tanpa tanda petik
      file "/etc/bind/arda/arda.fa;
  };
  ```
  ![dns slave](images/12-2-new.png)

- Jalankan command ini agar service bisa di jalankan:
  ```
  ln -s /etc/init.d/named /etc/init.d/bind9
  ```

- Lakukan restart bind9

  ```
  service bind9 restart
  ```



#### III. Testing

- Pada server *Manwe* silahkan matikan service bind9

  ```
  service bind9 stop
  ```

- Pada client *Varda* pastikan pengaturan nameserver mengarah ke IP *Manwe* dan IP *Melkor*

  ![testing pt.1](images/13-1-new.png)


- Lakukan ping ke **arda.fa** pada client *Varda*. Jika ping berhasil maka konfigurasi DNS slave telah berhasil

  ![testing pt.2](images/14-1-new.png)


### 1.2.G Membuat Subdomain

Dalam **dunia Arda**, terdapat banyak wilayah. Salah satunya adalah **Angband**, benteng kelam milik Melkor. Kita buat subdomain **angband.arda.fa** yang mengarah ke **Melkor**.

Subdomain adalah bagian dari sebuah nama domain induk. Subdomain umumnya mengacu ke suatu alamat fisik di sebuah situs contohnya: **arda.fa** merupakan sebuah domain induk. Sedangkan **angband.arda.fa** merupakan sebuah subdomain.

- Pada *Manwe*, edit file **/etc/bind/arda/arda.fa** lalu tambahkan subdomain untuk **arda.fa** yang mengarah ke IP *Melkor*.

  ```
  nano /etc/bind/arda/arda.fa
  ```

- Tambahkan konfigurasi seperti pada gambar ke dalam file **arda.fa**.

  ![subdomain](images/15-1-new.png)

- Restart service bind  

  ```
  service bind9 restart
  ```

- Coba ping ke subdomain dengan perintah berikut dari client *Varda*

  ```
  ping angband.arda.fa -c 5
  
  ATAU
  
  host -t A angband.arda.fa
  ```
  ![subdomain testing](images/16-1-new.png)



### 1.2.H Delegasi Subdomain

Kita juga bisa mendelegasikan wilayah **Beleriand**. Segala urusan Beleriand akan langsung ditangani oleh **Manwe**, namun di dalamnya bisa ada negeri lain seperti **Doriath** (dalam contoh diarahkan ke Varda).

Delegasi subdomain adalah pemberian wewenang atas sebuah subdomain kepada DNS baru.

#### I. Konfigurasi Pada Server *Manwe*

- Pada *Manwe*, edit file **/etc/bind/arda/arda.fa** dan ubah menjadi seperti di bawah ini sesuai dengan pembagian IP *Manwe* kelompok masing-masing.

  ```
  nano /etc/bind/arda/arda.fa
  ```
  ![delegasi subdomain zone file](images/17-1-new.png)


- Kemudian edit file **/etc/bind/named.conf.options** pada *Manwe*.

  ```
  nano /etc/bind/named.conf.options
  ```

- Tambahkan baris berikut pada **/etc/bind/named.conf.options**

  ```
  allow-query{any;};
  auth-nxdomain no;
  listen-on-v6 { any; }
  ```
  ![isi dari conf options](images/18-new.png)


- Kemudian edit file **/etc/bind/named.conf.local** menjadi seperti gambar di bawah:

  ```
  zone "arda.fa" {
      type master;
      file "/etc/bind/arda/arda.fa";
      allow-transfer { "IP Melkor"; }; // Masukan IP Melkor tanpa tanda petik
  };
  ```
  ![delegasi subdomain manwe](images/19-1-new.png)

- Setelah itu restart bind9

  ```
  service bind9 restart
  ```

#### II. Konfigurasi Pada Server *Melkor*

- Pada *Melkor* edit file **/etc/bind/named.conf.options**

  ```
  nano /etc/bind/named.conf.options
  ```

- tambahkan baris berikut pada **/etc/bind/named.conf.options**

  ```
  allow-query{any;};
  auth-nxdomain no;
  listen-on-v6 { any; }
  ```
  ![isi dari conf options](images/18-new.png)

- Lalu edit file **/etc/bind/named.conf.local** menjadi seperti gambar di bawah:

  ![doriath zone file](images/21-2-new.png)

- Kemudian buat direktori dengan nama **delegasi** 
  ```
  mkdir /etc/bind/delegasi
  ```

- Dibawah ini Jalankan urut dari A ke B.
  - A. buat file baru bernama **zone.template** pada direktori `/etc/bind/`.
    ```
    nano /etc/bind/zone.template
    ```
    Isi dengan isi berikut:
    ```
    $TTL    604800          ; Waktu cache default (detik)
    @       IN      SOA     localhost. root.localhost. (
                            2025100401 ; Serial (format YYYYMMDDXX)
                            604800     ; Refresh (1 minggu)
                            86400      ; Retry (1 hari)
                            2419200    ; Expire (4 minggu)
                            604800 )   ; Negative Cache TTL
    ;
    
    @       IN      NS      localhost.
    @       IN      A       127.0.0.1
    ```
  
  - B. Copy file `/etc/bind/zone.template` dan rename file hasil copy menjadi `/etc/bind/delegasi/doriath.arda.fa`
    ```
    cp /etc/bind/zone.template /etc/bind/delegasi/doriath.arda.fa
    ``` 
- Copy **zone.template** ke direktori tersebut dan edit namanya menjadi **doriath.arda.fa** 

  ```
  cp /etc/bind/zone.template /etc/bind/delegasi/doriath.arda.fa
  ```

- Kemudian edit file **doriath.arda.fa** menjadi seperti dibawah ini

  ![doriath zone fil pt.2](images/22-1-new.png)

- Restart bind9

  ```
  service bind9 restart
  ```

#### III. Testing

- Lakukan ping ke domain **doriath.arda.fa** dan **beleriand.doriath.arda.fa** dari client *Ulmo*

  ![testing delegasi subdomain](images/23-1-new.png)


### 1.2.I DNS Forwarder

Akhirnya, agar para penghuni Arda bisa melihat dunia luar (internet), **Manwe** sebagai **Master** akan menggunakan jalur Eru (router) untuk meneruskan pertanyaan ke **DNS upstream**.

DNS Forwarder digunakan untuk mengarahkan DNS Server ke IP yang ingin dituju.

- Edit file **/etc/bind/named.conf.options** pada server *Manwe*
- tambahkan bagian ini

```
forwarders {
    "IP nameserver dari Eru";
};
```
- Dan tambahkan

```
dnssec-validation no;
allow-query{any;};
auth-nxdomain no;
listen-on-v6 { any; }
```
![dns forwarder](images/24-1-new.png)

- Restart bind9

  ```
  service bind9 restart
  ```

- Harusnya jika nameserver pada file **/etc/resolv.conf** di client diubah menjadi IP **Manwe** maka akan di forward ke IP DNS GNS3 yaitu IP nameserver yang ada di **Eru** dan bisa mendapatkan koneksi.
- Coba ping google.com pada **Ulmo**, kalau benar maka tetap bisa mendapatkan respon dari google

  ![testing dns forward](images/25-new.png)


### 1.3 Keterangan Configurasi Zone file

1. #### Penulisan Serial

   Ditulis dengan format YYYYMMDDXX. Serial di increment setiap melakukan perubahan pada file zone.

   ```
   YYYY adalah tahun
   MM adalah bulan
   DD adalah tanggal
   XX adalah counter
   ```

   Contoh:

   ![DNS](images/17-1-new.png)

2. #### Penggunaan Titik

   ![delegasi subdomain zone file](images/17-1-new.png)

   Pada salah satu contoh di atas, dapat kita amati pada kolom keempat terdapat record yang menggunakan titik pada akhir kata dan ada yang tidak. Penggunaan titik berfungsi sebagai penentu FQDN (Fully-Qualified Domain Name) suatu domain.

   Contohnya jika "**arda.fa.**" di akhiri dengan titik maka akan dianggap sebagai FQDN dan akan dibaca sebagai "**arda.fa**" , sedangkan ns1 di atas tidak menggunakan titik sehingga dia tidak terbaca sebagai FQDN. Maka ns1 akan di tambahkan di depan terhadap nilai $ORIGIN sehinga ns1 akan terbaca sebagai "**ns1.arda.fa**" . Nilai $ORIGIN diambil dari penamaan zone yang terdapat pada  */etc/bind/named.conf.local*.

3. #### Penulisan Name Server (NS) record

   Salah satu aturan penulisan NS record adalah dia harus menuju A record., bukan CNAME. 



## Latihan Modul 2 DNS - Dunia Arda
> Pada latihan kali ini, kalian akan berperan sebagai para Ainur yang mengatur keseimbangan di dunia Arda. Manwë menjadi penguasa domain (DNS Master), Melkor menjadi bayangan (DNS Slave), dan Varda serta Ulmo adalah penghuni dunia yang akan memanfaatkan layanan DNS (Client).

1. Buatlah agar bila kita mengecek IP milik **Manwe** menggunakan dnsutils (`host -t PTR “IP Manwe”`), hasilnya menunjukkan bahwa IP tersebut dimiliki oleh domain `arda.fa`!
2. Buatlah subdomain **angband.arda.fa**, **beleriand.arda.fa**, dan **valinor.arda.fa** yang semuanya mengarah ke IP milik **Melkor**!
3. Buatlah subdomain **ulmo.arda.fa**. Lalu buatlah subdomain dalam subdomain dalam subdomain **yyy.laut.dalam.ulmo.arda.fa** yang mengarah ke IP milik **Manwe**! (yyy = nama kelompok)
4. Buatlah record `CNAME` **megah.arda.fa** dan **semangat.yyy.arda.fa** yang mengarah ke **arda.fa**! (yyy = nama kelompok)
5. Delegasikan subdomain **yyy.ulmo.arda.fa** dan **asyik.yyy.ulmo.arda.fa** dari **Manwe ke Melkor**! (yyy = nama kelompok)

## References
* https://computer.howstuffworks.com/dns.htm
* http://knowledgelayer.softlayer.com/faq/what-does-serial-refresh-retry-expire-minimum-and-ttl-mean
* https://en.wikipedia.org/wiki/List_of_DNS_record_types
* https://kb.indowebsite.id/knowledge-base/pengertian-catatan-dns-atau-record-dns/
