# Jarkom-Modul-1-IT11-2024

## Kelompok IT11
| NRP | Nama |
| ------ | ------ |
| 5027211049 | Tridiktya Hardani Putra |
| 5027221008 | Jeany Aurellia Putri Dewati |

## Daftar Isi
- [Jarkom-Modul-1-IT11-2024](#jarkom-modul-1-it11-2024)
  - [Kelompok IT11](#kelompok-it11)
  - [Daftar Isi](#daftar-isi)
  - [Creds](#creds)
  - [ATM or ATP or FTP ?🤔](#atm-or-atp-or-ftp-)
  - [Fuzz](#fuzz)
  - [How Many Packets?](#how-many-packets)
  - [Trace Him](#trace-him)
  - [Malwleowleo](#malwleowleo)
  - [Evidence](#evidence)
  - [Whoami](#whoami)
  - [Secret](#secret)

## Creds
Pertama-tama, kami membuka file evidence.pcap dengan Wireshark. Kemudian, memasukkan **ftp** sebagai keyword pada display filter.

![CR 1](./image/creds1.png)
Kami scroll ke bawah sampai menemukan response “Login successful”. Sebenarnya menggunakan filter **ftp.response.code == 230** merupakan cara yang lebih tepat dan cepat untuk menemukan respons login successful karena 230 response code dalam FTP artinya login berhasil. Setelah itu, kami follow TCP Stream dari packet yang mengandung info tersebut.

![CR 2](./image/creds2.png)

Dan ditemukan USER dan PASS yang digunakan untuk menjawab pertanyaan yang ada pada netcat 10.15.40.20 10007.

![CR 3](./image/creds3.png)
## ATM or ATP or FTP ?🤔
Langkah-langkah penyelesaian soal ini sangat mirip dengan solusi soal “creds”. Pertama-tama, kami membuka file evidence.pcap dengan Wireshark. Kemudian, memasukkan “ftp” sebagai keyword pada display filter.

![AAF 1](./image/atmatpftp1.png)

Kami scroll sampai bawah sendiri, dan menemukan response “Login successfull”. Atau dengan cara yang lebih cepat dengan display filter yang digunakan untuk menyelesaikan soal [Creds](#creds). Setelah itu, kami follow TCP Stream.

![AAF 2](./image/atmatpftp2.png)

Dan ditemukan PASS yang digunakan untuk menjawab pertanyaan yang ada pada netcat 10.15.40.20 10004.

![AAF 3](./image/atmatpftp3.png)
## Fuzz
Pertama-tama, kami membuka file capture.pcap dengan Wireshark. Kemudian, memasukkan “http” sebagai keyword pada display filter.

![FUZZ 1](./image/fuzz1.png)

Kami menemukan kumpulan requests di bagian yang paling atas, diketahui dari **Request Header POST / HTTP/1.1 dan application/x-www-form-urlencoded**. IP Source request tersebut merupakan jawaban untuk soal 1 pada netcat. Untuk jawaban soal 2, diketahui dari port **HTTP by default, yaitu 80**. Untuk jawaban soal 3, diketahui dari endpoint yang tertera pada request **(‘/’)**.

Untuk jawaban soal 4, diketahui dari follow HTTP Stream salah satu dari requests tersebut, tetapi berupa singkatan dari software tertera, yaitu **ffuf**.

![FUZZ 2](./image/fuzz2.png)

Terakhir, jawaban soal 5, diketahui dengan cara memasukkan keywords **“ip.src == 172.20.0.2 && http”** supaya hasil menunjukkan response dari host (172.20.0.2) dan ber-protokol HTTP.

![FUZZ 3](./image/fuzz3.png)

Kami scroll sampai menemukan response yang paling berbeda di antara semuanya. Dan kami menemukan **HTTP Response 302 Found**. Kemudian, kami follow HTTP stream dari response tersebut.

![FUZZ 4](./image/fuzz4.png)

Di dalam stream ini, kami melakukan pencarian spesifik (Find) dengan keyword **“302 f”** untuk menemukan HTTP Response 302 Found guna mengetahui kredensial yang benar, sebagai jawaban dari soal 5.

![FUZZ 5](./image/fuzz5.png)
## How Many Packets?
Pada filter dimasukkan keyword **ftp.response.code == 331** untuk melakukan list semua response yang membutuhkan password untuk melanjutkan code 331. Dan didapatkan terdapat **934** packet yang muncul dari hasil response.
![HMP 1](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/hom%20many%201.png)

Hasil ini kemudian kami masukkan pada nc yang ada pada soal dan didapatkan flag sebagai berikut.
![HMP 2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/hom%20many%202.png)
## Trace Him
Kami mendapatkan ip attacker dengan melihat kolom destination pada bagian response. Didapatkan IP address attacker yaitu **10.15.40.20**.
![TH 1](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/tracehim%201.png)

Hasil ini kemudian kami masukkan pada nc dan didapatkan flag sebagai berikut.
![TH 2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/tracehim%202.png)
## Malwleowleo
Pertama kami melakukan filter **tcp.stream eq (nomor)** dan kemudian melakukan follow. Ketika mencapai **tcp.stream eq 2 kami** menemukan command **STOR m4L1c10us_W4re.c**.
![Mal 1](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/malwle1.png)

Kemudian file **m4L1c10us_W4re.c** kami masukkan pada nc dan didapatkan flag sebagai berikut.
![Mal 2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/malwle2.png)
## Evidence
Kami mendapatkan domain dari korban dengan melakukan filter dengan keyword **http**, kemudian setelah melakukan scroll, pada **tcp.stream eq 1011** didapatkan domain korban yaitu **nanomate-solution.com**
![Evi 1](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/eviden1.png)

Pertanyaan selanjutnya dari nc adalah web server dari korban. Setelah menemukan domain korban, kemudian kami mencoba melakukan filter menggunakan keyword **http.host = “nanomate-solutions.com”**. Setelah itu kami menemukan pada **tcp.stream eq 1026** web server dari korban beserta versinya yaitu **Apache/2.4.56**.
![Evi 2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/eviden2.png)

Pertanyaan selanjutnya adalah mencari endpoint. Untuk mencarinya kami melakukan filter dengan keyword **http.request.method** dan setelah dilakukan scroll kami mendapatkan **/app/includes/process_login.php** sebagai hasilnya.
![Evi 3](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/eviden3.png)

Pertanyaan terakhir yaitu email dan password yang dapat melakukan login. Kami melakukan filter keyword **http** kemudian kami cari info yang memiliki metode post dan hasil yang bertuliskan "Login Succesful". Kemudian didapatkan **email=tareq@gmail.com&password=tareq@nanomate**.
![Evi 4](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/eviden4.png)

Semua jawaban ini, kami masukkan pada nc dan didapatkan hasil flag seperti berikut.
![Evi 5](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/eviden5.png)
## Whoami
Kami menemukan nama dari attacker setelah melakukan filter **tcp.stream(nomor)** dan menemukannya pada **tcp.stream eq 7**, tapi masih terenkripsi
![whoami](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/attacker1.png)

Setelah itu kami melakukan decode menggunakan base64 dan didapatkan nama dari attacker yaitu **Paul Atreides**.
![whoami2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/attacker2.png)

Dan kemudian kami masukkan nama tersebut pada nc dan didapatkan hasil flag seperti berikut.
![whoami3](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/attacker3.png)
## Secret
Kami menemukan pesan tersembunyi dari attacker dengan melakukan **export object**.
![secret1](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/secret%201.png)

Didapatkan 2 file seperti berikut.
![secret2](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/secret%201_2.png)

Kemudian kami save file yang bertuliskan **mirza.jpg** dan didapatkan gambar yang berisi pesan seperti dibawah ini. 
![secret3](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/mirza.jpg)

Kata **MIO MIRZA** kami inputkan pada nc dan didapatkan hasil flag seperti berikut ini.
![secret4](https://github.com/trdkhardani/Jarkom-Modul-1-IT11-2024/blob/main/image/secret%202.png)
