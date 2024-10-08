# Instalasi TrueNAS Scale di Proxmox VE 8.2

Panduan ini menjelaskan langkah-langkah untuk menginstal TrueNAS Scale sebagai VM di Proxmox VE versi 8.2.

## Prasyarat

- Server Proxmox yang sudah diinstal dan dikonfigurasi.
- File ISO TrueNAS Scale.
- Akses ke antarmuka web Proxmox.

## Langkah-langkah Instalasi TrueNAS Scale

### 1. Unduh ISO TrueNAS Scale ke Proxmox

1. Kunjungi [situs web TrueNAS](https://www.truenas.com/download-truenas-scale/) dan copy link unduhan trueNAS Scale terbaru.
2. Masuk ke antarmuka web Proxmox.
3. Buka tab `Datacenter` > `Server Anda` > `Local` > `ISO Images`.
4. Klik tombol `Download from URL` dan tunggu muncul pop-up windownya.
   
   ![Unduh ISO](download-from-url.png)

5. Paste link unduhan trueNAS Scale yang sudah anda copy sebelumnya.

   ![Unduh ISO](download-from-url-popup.png)

6. Pilih `Query URL` untuk mengambil nama file menggunakan link dari URL.

   ![Unduh ISO](download-from-url-popup-query-url.png)

7. Pilih `Download` untuk mengunduh file dan menyimpannya di local.

   ![Unduh ISO](download-from-url-popup-download.png)

8. Jika sudah selesai maka file ISO akan muncul di halaman.

   ![Unduh ISO](download-from-url-finish.png)

### 3. Buat VM Baru

1. Klik tombol `Create VM` di antarmuka web Proxmox.
   
   ![Create VM](create-vm.png)
   
2. Isi `VM ID` dan `Name` sesuai keinginan Anda.
   
   ![Create VM](create-vm-general.png)

3. Pada bagian `OS`, pilih `Use CD/DVD disc image file (iso)`, pilih `ISO File` yang telah Anda upload kemudian pilih `Next`.

   ![Create VM](create-vm-iso.png)
   
   ![Create VM](create-vm-iso-next.png)
   
4. Pilih `SCSI` sebagai `Bus/Device` untuk `Hard Disk`.

   ![Create VM](create-vm-disk.png)

5. Pada bagian `CPU`, pilih jumlah `cores` yang sesuai.

   ![Create VM](create-vm-cpu.png)
    
6. Pada bagian `Memory`, alokasikan jumlah RAM yang diperlukan (misalnya, 8GB).

   ![Create VM](create-vm-memory.png)
    
7. Pada bagian `Network`, pastikan menggunakan `VirtIO`.

    ![Create VM](create-vm-network.png)

8. Periksa kembali konfigurasi yang sudah dipilih, jika sudah sesuai maka pilih `Finish`

    ![Create VM](create-vm-finish.png)

### 4. Konfigurasi Disk

1. Buka tab `Datacenter` > `Server Anda` > `Shell`.

   ![Disk Configuration](proxmox-shell.png)
   
3. Identifikasi hard disk yang ingin Anda tambahkan ke Proxmox, dengan menggunakan perintah di bawah.
   ```
   lsblk -o +MODEL,SERIAL
   ls /dev/disk/by-id
   ```
4. Pilih hardisk yang akan di mount ke trueNAS, sebagai contoh:
   
   ![Disk Configuration](hardisk-mount.png)
   
5. Ketik perintah di bawah ini untuk melakukan mount sesuai hardisk yang dipilih.
   ```
   qm set [VM number] -scsi[number] /dev/disk/by-id/[hardisk]
   ```
7. Ulang perintah di atas jika terdapat dua atau lebih hardisk yang akan dipasang.
   
### 5. Mulai VM dan Instal TrueNAS Scale

1. Setelah VM dibuat, pilih VM tersebut dan klik tombol `Start`.

   ![Console](vm-console-start.png)
   
2. Buka `Console` untuk VM tersebut.
   
   ![Console](vm-console-start-novnc.png)

3. Setelah server boot dari drive, Anda akan melihat menu installer TrueNAS Scale.
   
   ![Menu Boot TrueNAS Scale](1.jpg)
    
4. Pilih "Install/Upgrade" dan tekan Enter.

   ![Menu Installer TrueNAS Scale](2.jpg)
      
5. Ikuti petunjuk di layar untuk menginstal TrueNAS Scale. Ini termasuk memilih drive target untuk instalasi (catatan: semua data pada drive ini akan dihapus).

   ![Menu Boot TrueNAS Scale](3.jpg)

6. Pastikan data yang ada di dalam drive tersebut sudah di backup, jika sudah aman pilih `Yes`.

   ![Menu Boot TrueNAS Scale](4.jpg)

7. Pilih `Administrative user (admin)` untuk mulai membuat akun admin yang baru.

   ![Menu Boot TrueNAS Scale](5.jpg)

8. Masukkan password untuk user `admin` dan pastikan anda tidak lupa dengan password yang sudah dimasukkan.

   ![Menu Boot TrueNAS Scale](6.jpg)

9. Pilih `OK` untuk melanjutkan ke proses selanjutnya.

   ![Menu Boot TrueNAS Scale](7.jpg)

10. Pilih `Yes` jika sudah menggunakan hardware yang baru lalu boot dengan EFI.

   ![Menu Boot TrueNAS Scale](8.jpg)

11. Proses instalasi dimulai.

   ![Menu Boot TrueNAS Scale](9.jpg)
   
12. Setelah instalasi selesai, lepaskan USB drive dan reboot server.

   ![Menu Boot TrueNAS Scale](10.jpg)

13. Pilih `Reboot System` kemudian pilih `OK`.

   ![Menu Boot TrueNAS Scale](11.jpg)

### 6. Konfigurasi Awal

1. Setelah server reboot, TrueNAS Scale akan mulai. Konsol akan menampilkan alamat IP untuk mengakses antarmuka web TrueNAS.
   
   ![Alamat IP Konsol](12.png)
   
3. Buka browser web di komputer yang terhubung ke jaringan yang sama dan masukkan alamat IP tersebut.
4. Ikuti wizard konfigurasi awal untuk mengatur pengaturan jaringan, membuat pool penyimpanan, mengatur pengguna, dll.
   
   ![Wizard Konfigurasi](15.jpg)

### 7. Pasca Instalasi

1. Perbarui TrueNAS Scale ke versi terbaru melalui antarmuka web.
2. Konfigurasikan layanan dan aplikasi tambahan sesuai kebutuhan.

Dengan mengikuti langkah-langkah ini, Anda seharusnya dapat menginstal dan mengkonfigurasi TrueNAS Scale di server Anda. Jika Anda mengalami masalah atau memerlukan bantuan lebih lanjut, jangan ragu untuk bertanya!
