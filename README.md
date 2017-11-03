# Tugas 2 PKSJ

| NRP         | Nama                     |
|-------------|--------------------------|
| 5114100001  | M. Nezar Mahardika       |
| 5114100040  | Rizky Fenaldo M          |
| 5114100171  | Glleen Allan M           |

#### Penjelasan Tugas
*Install Wordpress pada sebuah virutal OS
*Install plugin yang vulnerable terhadap SQL injection
*Install/gunakan tool untuk uji SQL injection pada Wordpress


## Pendahuluan
## Dasar Teori
**OS yang digunakan**

* **Ubuntu 16.04** adalah 
Ubuntu merupakan salah satu distribusi Linux yang berbasiskan Debian dan didistribusikan sebagai perangkat lunak bebas. Nama Ubuntu berasal dari filosofi dari Afrika Selatan yang berarti "kemanusiaan kepada sesama". Ubuntu dirancang untuk kepentingan penggunaan pribadi, namun versi server Ubuntu juga tersedia, dan telah dipakai secara luas. (https://id.wikipedia.org/wiki/Ubuntu)

* **Ubuntu Server** adalah 
suatu desain ubuntu yang digunakan untuk diinstall di lingkungan enterprise atau perusahaan untuk keperluan seperti web server ataupun router, secara default versi server ini tidak menyertakan antarmuka GUI, yang ada hanya shell alias Command line,aplikasi bawaan dari ubuntu server sekedar info buat anda berupa aplikasi serveri webserver, DNS server, DHCP server, firewall, openSSH, dan applikasi yang berhubungan dengan server, teknologi yang dibenamkan diserver juga umumnya hanya dipakai oleh orang yang benar benar advanced di Linux. (https://etix.wordpress.com/2010/01/28/perbedaan-ubuntu-server-dan-desktop/)

**Tools yang digunakan**

* **Wordpress** adalah 
Sebuah aplikasi sumber terbuka (open source) yang sangat populer digunakan sebagai mesin blog (blog engine). WordPress dibangun dengan bahasa pemrograman PHP dan basis data (database) MySQL. PHP dan MySQL, keduanya merupakan perangkat lunak sumber terbuka (open source software). Selain sebagai blog, WordPress juga mulai digunakan sebagai sebuah CMS (Content Management System) karena kemampuannya untuk dimodifikasi dan disesuaikan dengan kebutuhan penggunanya. WordPress adalah penerus resmi dari b2/cafelog yang dikembangkan oleh Michel Valdrighi. Nama WordPress diusulkan oleh Christine Selleck, teman Matt Mullenweg. WordPress saat ini menjadi platform content management system (CMS) bagi beberapa situs web ternama seperti CNN, Reuters, The New York Times, TechCrunch, dan lainnya. (https://id.wikipedia.org/wiki/WordPress)

*Wordpress Plugin*

* **Video Player**, 
adalah plugin Wordpress yang menyediakan fitur untuk memasukkan video ke website Wordpress.
Video Player ini dapat diunduh di halaman ini (https://wordpress.org/plugins/player/developers/)

* **League Manager**,
Plugin ini digunakan untuk menampilkan hasil dari pertandingan sepak bola pada website wordpress.
Plugin ini dapat diunduh pada halaman berikut (https://wordpress.org/plugins/leaguemanager/developers/).

*Tools*

* **Sql Map** 
tools opensource yang mendeteksi dan melakukan exploit pada bug SQL injection secara otomatis. dengan melakukan serangan SQL injection seorang attacker dapat mengambil alih serta memanipulasi sebuah database di dalam sebuah server.
SQLMap dapat diunduh pada link berikut (https://github.com/sqlmapproject/sqlmap/zipball/master)

* **Wpscan** 
merupakan tools vulnerability scanner untuk CMS Wordpress yang ditulis dengan menggunakan bahasa pemrograman ruby, WPScan mampu mendeteksi kerentanan umum serta daftar semua plugin dan themes yang digunakan oleh sebuah website yang menggunakan CMS Wordpress.
WPScan dapat diunduh pada link berikut (https://github.com/wpscanteam/wpscan/zipball/master)

## Persiapan

#### 1. Langkah Instalasi Wordpress

  1. Pastikan LAMP (Linux, Apache, MySQL, and PHP) stack sudah terinstall
  2. Unduh versi terbaru dari Wordpress
  $ cd /tmp
  $ wget http://wordpress.org/latest.tar.gz
  
  3. Ekstrak file yang telah diunduh, ke dalam folder `/var/www/html`
  
  $ tar -xvzf latest.tar.gz -C /var/www/html

  4. Login ke MySQL milik user `root`
  
  $ mysql -u root -p
  
  5. Buat database yang akan digunakan Wordpress
  
  mysql> CREATE DATABASE wordpress

  6. Buat user yang digunakan untuk mengoperasikan database Wordpress
  
  mysql> CREATE USER wordpress@localhost IDENTIFIED BY "pksj";
  

  7. Berikan hak akses ke database `wordpress` untuk user yang baru dibuat
  
  mysql> GRANT ALL ON wordpress.* TO wordpress@localhost;
  

  8. Flush aturan hak akses yang telah ditetapkan agar MySQL yang sedang berjalan dapat mengetahui perubahan aturan hak akses, kemudian keluar dari MySQL
  
  mysql> FLUSH PRIVILEGES;
  mysql> exit;
  
  9. Pindah ke direktori `/var/www/html/wordpress`, kemudian salin `wp-config-sample.php` ke file `wp-config.php`
  
  $ cd /var/www/html/wordpress;
  $ cp wp-config-sample.php wp-config.php;
  

  10. Buka file `wp-config.php` menggunakan teks editor, kemudian ubah isi variable dari `DB_NAME`, `DB_USER`dan `DB_PASSWORD` sesuai dengan nilai yang telah anda tentukan pada langkah 5 dan 6
  
  $ vi wp-config.php
  
   11. Buka web browser, kemudian masukkan alamat Wordpress anda (misal `http://localhost`. Jika Wordpress berjalan maka akan muncul tampilan seperti di gambar.
   
   ![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/1.JPG)
    
   12. Masukkan informasi yang akan digunakan untuk membuat akun admin  Wordpress. Setelah selesai, klik tombol `Install Wordpress`
   
   13. Login ke Wordpress anda menggunakan akun admin yang telah dibuat
   ![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/2.jpg)
    
   #### 2. Install Wordpress Plugin
   1. Unduh plugin Wordpress Video Player di https://wordpress.org/plugins/player/developers)
   
   2. Login ke wordpress, lalu pilih menu **Add New** pada navigasi **Plugins**
   ![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/3.JPG)
   ![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/4.JPG)
   
   3. Unggah file yang diunduh, kemudian pilih Install Now untuk menginstall plugin
   ![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/5.JPG)
   
   4. Setelah berhasil diinstal, klik activate plugin
    
#### Uji Sql Injection dengan Wpscan

Pada tahap ini, kami melakukan sebuah skenario uji sql injection, yaitu :
1. Memindai semua vulnability yang terdapat dalam wordpress plugin yang telah di install

-  Menggunakan *Wpscan* untuk melakukan pindai yang terdapat vulnability dalam wordpress plugin dengan cara :
ruby wpscan.rb -u 10.151.36.166/wordpress --enumerate vp

akan menghasilkan

![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/hasil%20wpscan.JPG)

#### Uji Sql Injection terhadap plugin wordpress leaguemanager

Pada tahap ini, kami melakukan 2 skenario uji sql injection terhadap leaguemanager plugin, yaitu :
1. melakukan brute force sql injection yang terdapat dalam leaguemanager yang telah diinstall

melakukan brute force sql injection yang terdapat dalam leaguemanager yang telah diinstall untuk dari match

-  Menggunakan *Sql Map* untuk melakukan sql injection terhadap leaguemanager untuk menggambil semua tables yang ada.

python sqlmap.py -u "http://10.151.36.166/?match=1" --dbms mysql --level 5 --risk 3 --tables

![Hasil](https://github.com/glleena/tugaspksj2/blob/master/hasiltugas2/hasil%20sqlmap.JPG)
