Modul Pengenalan GNS3
===============

- [Modul Pengenalan GNS3](#modul-pengenalan-gns3)
  - [Apakah GNS3 itu?](#apakah-gns3-itu)
  - [Instalasi GNS3 VM](#instalasi-gns3-vm)
    - [Import Image di VirtualBox](#import-image-di-virtualbox)
    - [Import Image di VMWare](#import-image-di-vmware)
    - [Memasukkan Image Ubuntu ke GNS3](#memasukkan-image-ubuntu-ke-gns3)
  - [Instalasi GNS3 Client](#instalasi-gns3-client)
    - [Download GNS3 Client](#instalasi-gns3-client)
    - [Setup GNS3 Client](#instalasi-gns3-client)
  - [Penggunaan GNS3 Web](#penggunaan-gns3-web)
    - [Setup IP di Node](#setup-ip-di-node)
    - [Akses Sebuah Node ke Internet](#akses-sebuah-node-ke-internet)
    - [Membuat Topologi](#membuat-topologi)
  - [Penggunaan GNS3 Client](#penggunaan-gns3-client)
    - [Setup IP di Node GNS3 Client](#setup-ip-di-node-gns3-client)
    - [Akses Sebuah Node ke Internet di Client](#akses-sebuah-node-ke-internet-di-client)
    - [Membuat Topologi di Client](#membuat-topologi-di-client)
  - [Ketentuan](#ketentuan)
  - [Peringatan, Saran, Tips, dan Trik](#peringatan-saran-tips-dan-trik)
  - [Troubleshooting](#troubleshooting)
  - [Sumber](#sumber)

## Apakah GNS3 itu?
**GNS3 (Graphical Network Simulator-3)** adalah alat yang membantu Anda untuk bisa menjalankan sebuah simulasi dari topologi kecil yang hanya terdiri dari beberapa alat saja di komputer Anda sampai dengan topologi yang memiliki banyak alat yang di-hosting di beberapa server.

## Instalasi GNS3 VM
### Import Image di VirtualBox
1. Install VirtualBox
Silahkan mendownload dari link berikut [VirtualBox 7.0](https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html).

2. Download Image VM GNS3
Silahkan mendowload dari link berikut [GNS3 VM 3.0.5](https://github.com/GNS3/gns3-gui/releases/download/v3.0.5/GNS3.VM.VirtualBox.3.0.5.zip). Sehabis itu langsung saja extract.

3. Import file .ova ke VirtualBox

![import-ova](images/import-ova.jpg)

![import-ova-2](images/import-ova-2.jpg)

4.  Membuat host network adapter baru
  - Pilih File Menu -> Host Network Manager <br/>
![new-host-network-adapter](images/new-host-network-adapter-1.jpg)
  - Klik Create <br/>
![new-host-network-adapter-2](images/new-host-network-adapter-2.jpg)
  - Lalu setting agar IPv4 Address adalah `192.168.0.1`, dan IPv4 Network Mask `255.255.255.0` lalu klik apply
![new-host-network-adapter-4](images/new-host-network-adapter-4.jpg)
![new-host-network-adapter-5](images/new-host-network-adapter-5.jpg)

5. Ubah Network Adapter di VM
  - Pergi ke Settings -> Network
  - Ubah Adapter 1 ke Host-only Adapter dan sesuaikan dengan host network yang telah dibuat sebelumnya
![setting-network-vm-1](images/setting-network-vm-3.jpg)
  - Dan ubah Adapter 2 menjadi NAT <br/>
![setting-network-vm-3](images/setting-network-vm-2.jpg)
  - Lalu klik OK

6.  Jalankan VM
  - Maka VM seharusnya bisa menampilkan ini
![vm](images/new-vm-1.png)
  - Lalu buka alamat dengan keterangan "To launch the Web-UI" di browser
![vm-2](images/vm-2.jpg)

Setelah itu silahkan lanjutkan untuk mengimpor image Ubuntu ke GNS3 [disini](#memasukkan-image-ubuntu-ke-gns3)


### Import Image di VMWare
1. Install VMWare
Silahkan mendownload dari [VMware Workstation 17](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html).

2. Download Image VM GNS3
Silahkan mendowload dari [GNS3 VM 3.0.5](https://github.com/GNS3/gns3-gui/releases/download/v3.0.5/GNS3.VM.VMware.Workstation.3.0.5.zip). Sehabis itu langsung saja extract.

3. Import file .ova ke VMWare dan namai VM.

![import-ova](images/insert-image-vmware-1.png)

![import-ova-2](images/insert-image-vmware-2.png)

![import-ova-3](images/insert-image-vmware-3.png)

4. Sesuaikan settingan VMWare dengan klik `Edit virtual machine settings`.
- Pastikan settingan Network sudah sesuai.

![settingan-vmware-1](images/settingan-vmware-1.png)

- Jika ada error `Virtualized ... Not Supported on Platform` saat vm nanti dijalankan, coba disable virtualisasi di settingan processor

![settingan-vmware-2](images/settingan-vmware-2.png)

5.  Jalankan VM
  - Maka VM seharusnya bisa menampilkan ini
![vm](images/new-vm-2.png)
  - Lalu buka alamat dengan keterangan "To launch the Web-UI" di browser
![vm-2](images/new-vm-vmware-2.png)

Setelah itu silahkan lanjutkan untuk mengimpor image Ubuntu ke GNS3 [disini](#memasukkan-image-ubuntu-ke-gns3)

### Memasukkan Image Ubuntu ke GNS3

1. Import image ubuntu
  - Klik `Open menu` pada kiri atas <br/>
    ![insert-image-1](images/insert-image-1.png)
  - Pilih `Template preferences` <br/>
    ![insert-image-2](images/insert-image-2.png)
  - Pilih `Docker` <br/>
    ![insert-image-3](images/insert-image-3.png)
  - Klik `Add Docker container template` <br/>
    ![insert-image-4](images/insert-image-4.png)
  - `Controller type` pilih `Run this Docker container locally` <br/>
    ![insert-image-5](images/insert-image-5.png) 
  - Klik `Docker Virtual Machine`, pilih `New image` isikan `nevarre/gns3-debi:latest` di Image name <br/>
    ![insert-image-6](images/insert-image-6.png)
  - Klik `Container name` masukkan `ervn-debi` sebagai nama container <br/>
    ![insert-image-7](images/insert-image-7.png)
  - Klik `Network adapters` dan masukkan angka 4 <br/>
    ![insert-image-8](images/insert-image-8.png)
  - Biarkan bagian `Start command`, `Console type`, `Auxiliary console type`, dan `Environment`
  - Lalu klik tombol `Add template` di bawah sendiri <br/>
    ![insert-image-9](images/insert-image-9.png)

2. Coba image yang telah di-import
  - Klik `Open menu` di kiri atas <br/>
    ![insert-image-1](images/insert-image-1.png)
  - Pilih `Projects` <br/>
    ![test-image-1](images/test-image-1.png)
  - Klik `Add blank project` <br/>
    ![test-image-2](images/test-image-2.png)
  - Masukkan nama project (terserah)
  - Klik `Add project` <br/>
    ![test-image-3](images/test-image-3.png)
  - Klik tombol `Add a node` di atas <br/>
    ![test-image-4](images/test-image-4.png)
  - Lalu tarik `ervn-debi` ke area kosong di halaman <br/>
    ![test-image-5](images/test-image-5.png)
  - Tunggu sampai loading selesai
  - Jika berhasil akan menampilkan tampilan yang mirip dengan ini <br/>
    ![test-image-6](images/test-image-6.png)
  - Kita bisa start dengan klik kanan di node dan klik `Start` <br/>
    ![test-image-7](images/test-image-7.png)

3. Akses node
  - Bisa dilakukan dengan `Web console`  <br/>
    ![akses-node-1](images/akses-node-1.png)
  - Bisa dilakukan menggunakan command `telnet [IP VM] [Port node]` di terminal lokal pc kita, jika menggunakan contoh di gambar, maka commandnya adalah `telnet 10.15.43.32 5006` <br/>
    ![akses-node-2](images/akses-node-2.png)
  - Jika menggunakan telnet, hati-hati jika ingin keluar dari node. Gunakan `Ctrl + ]` lalu ketik quit untuk keluar dari node.
  - Jika command prompt tidak kunjung keluar, bisa klik enter berkali-kali sampai keluar

## Penggunaan GNS3 Web

### Setup IP di Node

1. Klik kanan pada node, buka `Configure` <br/>
   ![setup-ip-1](images/setup-ip-1.png)
2. Pada menu `General settings`, cari tombol `Edit network configuration` <br/>
   ![setup-ip-2](images/setup-ip-2.png)
3. Di situ kalian bisa setup IP sesuai dengan interface yang digunakan. Interface adalah sesuatu yang digunakan untuk menghubungkan dua device

### Akses Sebuah Node ke Internet
1. Buka menu Add a Node <br/>
   ![test-image-4](images/test-image-4.png)
2. Tarik NAT ke area kosong <br/>
  ![using-internet-1](images/using-internet-1.png)
3. Gunakan aktifkan menu `Add a Link` <br/>
  ![using-internet-2](images/using-internet-2.png)
4. Lalu klik node, pilih interface `eth0`, dan klik node NAT yang ditarik tadi <br/>
  ![using-internet-3](images/using-internet-3.png)
5. Lalu konfigurasi IP dari node ubuntu
  - Cari 2 line yang seperti ini
  ```
  # auto eth0
  # iface eth0 inet dhcp
  ```
  - Uncomment kedua line tersebut, lalu save
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
6. Start node <br/>
   ![test-image-7](images/test-image-7.png)
7. Akses console dari node, dan coba ping ke google, jika berhasil maka settingan Anda benar <br/>
   ![using-internet-4](images/using-internet-4.png)
8. Node ini akan nanti digunakan sebagai router untuk modul ini, ganti nama node ini menjadi `Eru` dengan fitur `Change hostname` di node, dan juga ganti symbol ke simbol router dengan fitur `Change symbol`
   ![using-internet-5](images/using-internet-5.png)

### Membuat Topologi
1. Tambahkan beberapa node ethernet switch dan ubuntu, lalu buat hubungan antar node dan nama-nama dari node hingga seperti di gambar <br/>
  ![create-topology-1](images/create-topology-1.jpg)
2. Gunakan fitur `Change hostname` untuk merubah nama-nama dari node
3. Lalu kita setting network masing-masing node dengan fitur `Edit network configuration` seperti yang ditunjukkan [disini](#setup-ip-di-node) sebelumnya, kita bisa menghapus semua settingnya dan mengisi dengan settingan di bawah
  - Foosha
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
  	address [Prefix IP].1.1
  	netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
  	address [Prefix IP].2.1
  	netmask 255.255.255.0
  ```
  - Loguetown
  ```
  auto eth0
  iface eth0 inet static
  	address [Prefix IP].1.2
  	netmask 255.255.255.0
  	gateway [Prefix IP].1.1
  ```
  - Alabasta
  ```
  auto eth0
  iface eth0 inet static
  	address [Prefix IP].1.3
  	netmask 255.255.255.0
  	gateway [Prefix IP].1.1
  ```
  - EniesLobby
  ```
  auto eth0
  iface eth0 inet static
  	address [Prefix IP].2.2
  	netmask 255.255.255.0
  	gateway [Prefix IP].2.1
  ```
  - Water7
  ```
  auto eth0
  iface eth0 inet static
  	address [Prefix IP].2.3
  	netmask 255.255.255.0
  	gateway [Prefix IP].2.1
  ```
**Penjelasan Pengertian**
- **Gateway**: Jalur pada jaringan yang harus dilewati paket-paket data untuk dapat masuk ke jaringan yang lain.

4. Restart semua node
5. Cek semua node ubuntu apakah sudah memiliki ip yang sesuai dengan settingan dengan command `ip a`. Berikut adalah contoh untuk node `Foosha` dengan Prefix IP `10.40`, sesuaikan dengan Prefix IP kelompok kalian masing-masing
![create-topology-2](images/create-topology-2.jpg)
6. Topologi yang dibuat sudah bisa berjalan secara lokal, tetapi kita belum bisa mengakses jaringan keluar. Maka kita perlu melakukan beberapa hal.
- Ketikkan **`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16`** pada router `Foosha`
**Keterangan:**
  - **iptables:** iptables merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data. Dengan iptables inilah kita akan mengatur semua lalu lintas dalam komputer, baik yang masuk, keluar, maupun yang sekadar melewati komputer kita. Untuk penjelasan lebih lanjut nanti akan dibahas pada Modul 5.
  - **NAT (Network Address Translation):** Suatu metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan menggunakan satu alamat IP.
  - **Masquerade:** Digunakan untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.
  - **-s (Source Address):** Spesifikasi pada source. Address bisa berupa nama jaringan, nama host, atau alamat IP.
- Ketikkan command `cat /etc/resolv.conf` di `Foosha` <br/>
![create-topology-3](images/create-topology-3.jpg)
- Ingat-ingat IP tersebut karena IP tersebut merupakan IP DNS, lalu ketikkan command ini di node ubuntu yang lain `echo nameserver [IP DNS] > /etc/resolv.conf`. Jika pada kasus contoh maka command-nya adalah `echo nameserver 192.168.122.1 > /etc/resolv.conf`.
- Semua node sekarang seharusnya sudah bisa melakukan ping ke google, yang artinya adalah sudah tersambung ke internet

## Ketentuan
- Praktikan **hanya** diperbolehkan menggunakan image docker `kuuhaku86/gns3-ubuntu`

## Peringatan, Saran, Tips, dan Trik
- Apa yang diinstal di node **tidak persisten**, artinya saat Anda mengerjakan project tersebut lagi Anda perlu menginstal aplikasi itu kembali
- Maka **selalu** simpan config di node ke directory `/root` sebelum keluar dari project
- Anda bisa memasukkan command yang ingin selalu dijalankan di node tersebut ke file `/root/.bashrc` di bagian paling bawah. (Contoh : command iptables dan echo nameserver tadi)</br>
![tips-trik-1](images/tips-trik-1.jpg)
- Anda bisa melakukan ekspor project jika bekerja secara tim dengan pergi ke menu `Project settings` -> `Export portable project`.</br>
![tips-trik-2](images/tips-trik-2.jpg)
- Jika mengerjakan menggunakan VM di local kalian sendiri. Kalian bisa mencegah hilangnya aplikasi atau file config dengan mematikan VM di mode save state.
- Manfaatkan bash scripting untuk install-install aplikasi yang diperlukan sehingga tidak perlu memasukkan command satu-satu, lalu save ke `/root`.
## Troubleshooting
- Ada sesuatu yang biasanya bisa tetapi tiba-tiba tidak bisa? Coba matikan dulu VM nya baru nyalakan kembali. Masih tidak bisa? Coba cara instal GNS3 yang lain dahulu sebelum bertanya ke asisten.
- Tidak bisa instal di satu metode? Coba cara instal yang lain dulu sebelum bertanya ke asisten.


## Sumber
- https://docs.gns3.com/docs/

