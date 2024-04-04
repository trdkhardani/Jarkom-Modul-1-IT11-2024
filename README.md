# Jarkom-Modul-1-IT11-2024

## Kelompok IT11
| NRP | Nama |
| ------ | ------ |
| 5027211049 | Tridiktya Hardani Putra |
| 5027221008 | Jeany Aurellia Putri Dewati |

## Daftar Isi
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
## ATM or ATP or FTP ?🤔
## Fuzz
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
## Secret
