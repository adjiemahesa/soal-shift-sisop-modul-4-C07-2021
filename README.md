# soal-shift-sisop-modul-4-C07-2021

# Soal 1

Di suatu jurusan, terdapat admin lab baru yang super duper gabut, ia bernama Sin. Sin baru menjadi admin di lab tersebut selama 1 bulan. Selama sebulan tersebut ia bertemu orang-orang hebat di lab tersebut, salah satunya yaitu Sei. Sei dan Sin akhirnya berteman baik. Karena belakangan ini sedang ramai tentang kasus keamanan data, mereka berniat membuat filesystem dengan metode encode yang mutakhir.

a. Jika sebuah direktori dibuat dengan awalan “AtoZ_”, maka direktori tersebut akan menjadi direktori ter-encode.

b. Jika sebuah direktori di-rename dengan awalan “AtoZ_”, maka direktori tersebut akan menjadi direktori ter-encode.

c. Apabila direktori yang terenkripsi di-rename menjadi tidak ter-encode, maka isi direktori tersebut akan terdecode.

d. Setiap pembuatan direktori ter-encode (mkdir atau rename) akan tercatat ke sebuah log. Format : /home/[USER]/Downloads/[Nama Direktori] → /home/[USER]/Downloads/AtoZ_[Nama Direktori]

e. Metode encode pada suatu direktori juga berlaku terhadap direktori yang ada di dalamnya.(rekursif)


# jawaban soal 1

membuat fungsi untuk mengambil string tanpa ekstensi untuk dienkripsi menggunakan Atbash Cipher yang dipanggil saat readdir, berikut fungsi yag kami buat :
```
void encrypt1(char *str) //encrypt AtoZ
{
    if (strcmp(str, ".") == 0)
    return;
    if (strcmp(str, "..") == 0)
    return;

    printf("encrypting %s\n", str);

    int lastIdx = strlen(str);
    for (int i = lastIdx; i >= 0; i--)
    {
        if (str[i] == '/')
        {
            continue;
        }
        if (str[i] == '.')
        {
            lastIdx = i;
            break;
        }
    }
    mirrorFunc(str, lastIdx);
    printf("last %s\n", str);
}
```


fungsi akan melakukan print  untuk penanda pada terminal bahwa fungsi ini sedang melakukan encrypting. kemudian fungsi melakukan loop sepanjang string untuk mendapatkan nilai index terakhir sebelum ``.`` dan berhenti jika isi string adalah ```.``` lalu memasukannya kedalam variabel i

kemudian kami membuat fungsi untuk melakukan decrypting yang memiliki kegunaan yang hampir sama dengan fungsi encrypting sebelumnya, yaitu mengambil string tanpa ekstensi yang nantinya akan dipanggil saat getattr, readdir, unlink, dan saat rmdir untu melakukan dekripsi. berikut fungsi yag kami buat :
```
void decrypt1(char *str)
{
    if (strcmp(str, ".") == 0)
    return;
    if (strcmp(str, "..") == 0)
    return;
    if (strstr(str, "/") == NULL)
    return;

    char *nameFile = strstr(str, "/");

    printf("decrypting %s - %s\n", str, nameFile);

    int lastIdx = strlen(nameFile);
    for (int i = lastIdx; i >= 0; i--)
    {
        if (nameFile[i] == '/')
            break;
        if (nameFile[i] == '.')
        {
            lastIdx = i;
            break;
        }
    }
    mirrorFunc(nameFile + 1, lastIdx);
}

```

fungsi ini juga akan melakukan print untuk penanda pada terminal bahwa fungsi ini sedang melakukan decrypting. kemudian fungsi melakukan loop sepanjang string dari nama file untuk mendapatkan nilai index terakhir dari nama file dan berhenti jika isi string adalah ```.``` atau ```/```lalu memasukannya kedalam variabel i


selanjutnya kami membuat fungsi mirrorFunc yang melakukan pembalikan abjad untuk enkripsi sekaligus dekripsi fungsinya sebagai berikut :

```
void mirrorFunc(char *str, int safeIndex)
{
    for (int j = 0; j < safeIndex; j++)
    {
        int alphabet = 26;
        char start = str[j];

        if (str[j] >= 65 && str[j] <= 90)
        {
            str[j] = str[j] - 65 + 1;
            str[j] = alphabet - str[j];
            str[j] += 65;
        }
        else if (str[j] >= 97 && str[j] <= 122)
        {
            str[j] = str[j] - 97 + 1;
            str[j] = alphabet - str[j];
            str[j] += 97;
        }
        printf("%c menjadi %c\n", start, str[j]);
    }
}
```
fungsi di atas melakukan perulangan untuk membalik huruf alphabet sesuai dengan kode ASCII
```
if (str[j] >= 65 && str[j] <= 90)
        {
            str[j] = str[j] - 65 + 1;
            str[j] = alphabet - str[j];
            str[j] += 65;
        }
```
potongan diatas adalah untuk membalik huruf alphabet kapital

```
else if (str[j] >= 97 && str[j] <= 122)
        {
            str[j] = str[j] - 97 + 1;
            str[j] = alphabet - str[j];
            str[j] += 97;
        }
```
potongan diatas adalah untuk membalik huruf alphabet yang bukan kapital 

kemudian setiap folder yang dibuat atau direname dengan awalan AtoZ_ ditulist di dalam log menggunakan fungsi berikut :
```
void logAtoZ(const char *currPath, const char *newPath)
{
    FILE *logFile = fopen("/home/adjie/AtoZ.log", "a");
    fprintf(logFile, "%s → %s\n", currPath, newPath);

    fclose(logFile);
}
```



# Soal 2

Selain itu Sei mengusulkan untuk membuat metode enkripsi tambahan agar data pada komputer mereka semakin aman. Berikut rancangan metode enkripsi tambahan yang dirancang oleh Sei

a. Jika sebuah direktori dibuat dengan awalan “RX_[Nama]”, maka direktori tersebut akan menjadi direktori terencode beserta isinya dengan perubahan nama isi sesuai kasus nomor 1 dengan algoritma tambahan ROT13 (Atbash + ROT13).

b. Jika sebuah direktori di-rename dengan awalan “RX_[Nama]”, maka direktori tersebut akan menjadi direktori terencode beserta isinya dengan perubahan nama isi sesuai dengan kasus nomor 1 dengan algoritma tambahan Vigenere Cipher dengan key “SISOP” (Case-sensitive, Atbash + Vigenere).

c. Apabila direktori yang terencode di-rename (Dihilangkan “RX_” nya), maka folder menjadi tidak terencode dan isi direktori tersebut akan terdecode berdasar nama aslinya.

d. Setiap pembuatan direktori terencode (mkdir atau rename) akan tercatat ke sebuah log file beserta methodnya (apakah itu mkdir atau rename).

e. Pada metode enkripsi ini, file-file pada direktori asli akan menjadi terpecah menjadi file-file kecil sebesar 1024 bytes, sementara jika diakses melalui filesystem rancangan Sin dan Sei akan menjadi normal. Sebagai contoh, Suatu_File.txt berukuran 3 kiloBytes pada directory asli akan menjadi 3 file kecil yakni:

Suatu_File.txt.0000

Suatu_File.txt.0001

Suatu_File.txt.0002

Ketika diakses melalui filesystem hanya akan muncul Suatu_File.txt


# Soal 3

Karena Sin masih super duper gabut akhirnya dia menambahkan sebuah fitur lagi pada filesystem mereka. 

a. Jika sebuah direktori dibuat dengan awalan “A_is_a_”, maka direktori tersebut akan menjadi sebuah direktori spesial.

b. Jika sebuah direktori di-rename dengan memberi awalan “A_is_a_”, maka direktori tersebut akan menjadi sebuah direktori spesial.

c. Apabila direktori yang terenkripsi di-rename dengan menghapus “A_is_a_” pada bagian awal nama folder maka direktori tersebut menjadi direktori normal.

d. Direktori spesial adalah direktori yang mengembalikan enkripsi/encoding pada direktori “AtoZ_” maupun “RX_” namun masing-masing aturan mereka tetap berjalan pada direktori di dalamnya (sifat recursive  “AtoZ_” dan “RX_” tetap berjalan pada subdirektori).

e. Pada direktori spesial semua nama file (tidak termasuk ekstensi) pada fuse akan berubah menjadi lowercase insensitive dan diberi ekstensi baru berupa nilai desimal dari binner perbedaan namanya.


Contohnya jika pada direktori asli nama filenya adalah “FiLe_CoNtoH.txt” maka pada fuse akan menjadi “file_contoh.txt.1321”. 1321 berasal dari biner 10100101001.


# soal 4

Untuk memudahkan dalam memonitor kegiatan pada filesystem mereka Sin dan Sei membuat sebuah log system dengan spesifikasi sebagai berikut.

1. Log system yang akan terbentuk bernama “SinSeiFS.log” pada direktori home pengguna (/home/[user]/SinSeiFS.log). Log system ini akan menyimpan daftar perintah system call yang telah dijalankan pada filesystem.

2. Karena Sin dan Sei suka kerapian maka log yang dibuat akan dibagi menjadi dua level, yaitu INFO dan WARNING.

3. Untuk log level WARNING, digunakan untuk mencatat syscall rmdir dan unlink.

4. Sisanya, akan dicatat pada level INFO.

5. Format untuk logging yaitu:


```[Level]::[dd][mm][yyyy]-[HH]:[MM]:[SS]:[CMD]::[DESC :: DESC]```<br>

```Level : Level logging, dd : 2 digit tanggal, mm : 2 digit bulan, yyyy : 4 digit tahun, HH : 2 digit jam (format 24 Jam),MM : 2 digit menit, SS : 2 digit detik, CMD : System Call yang terpanggil, DESC : informasi dan parameter tambahan```

```INFO::28052021-10:00:00:CREATE::/test.txt```<br>

```INFO::28052021-10:01:00:RENAME::/test.txt::/rename.txt```

PEMBAHASAN :

Untuk Menjawab dari soal nomor 4 disini kamu menggunakan Fungsi `logCreate` yang dimana pada fungsi ini kita mendeklarasikan file terlebih dahulu dengan nama `logFile`. Setelah itu nantinya akan di fopen dengan menggunakan parameter "a" yang di mana "a" sendiri sebagai penghubung atau `append`. di sini juga untuk penginfoan terdapat fungsi `if` dengan tujuan sebagai log fungsi syscall unlink dan rmdir dengan menampilkan `Warning` dan `Info` lalu kemudian menampilkan level INFO . 

```
void logCreate(char *c, int type)
{
    FILE *logFile = fopen("/home/adjie/SinSeiFS.log", "a");
    time_t currTime;
    struct tm *time_info;
    time(&currTime);
    time_info = localtime(&currTime);

    int year = time_info->tm_year + 1900;
    int month = time_info->tm_mon + 1;
    int day = time_info->tm_mday;
    int hour = time_info->tm_hour;
    int min = time_info->tm_min;
    int sec = time_info->tm_sec;

    if (type == 1)
    { 
        fprintf(logFile, "INFO::%02d%02d%d-%d:%d:%d:%s\n", day, month, year, hour, min, sec, c);
    }
    else if (type == 2)
    { 
        fprintf(logFile, "WARNING::%02d%02d%d-%d:%d:%d:%s\n", day, month, year, hour, min, sec, c);
    }
    fclose(logFile);
}
```



