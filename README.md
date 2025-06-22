### Laporan task 3

### Subtask A

![task3_a_code]

- Menggunakan sudo useradd untuk membuat user lokal
- Menggunakan sudo paswwd untuk membuat password masing masing user

**Hasil output**

![task3_a_output](images/task3_a_code.png)

### Subtask B

![task3_b_code](images/task3_b_code.png)

- Fungsi trl_getattr digunakan untuk memberikan metadata file/direktori. Pada ('/') diset sebagai direktori, Read-only untuk very_spicy_info.txt,   dan semua bisa baca tulis untuk upload.txt
- Fungsi trl_readdir, untuk setiap filler berturut-turut, Menambahkan entri untuk direktori saat ini (.), menambahkan entri untuk parent            direktori (..), menambahkan file file_very_spicy_info, dan file_upload
- Fungsi trl_opendir untuk mevalidasi akses saat membuka file
- Fungsi trl_read untuk membaca konten file
- Fungsi trl_write untuk menulis konten file, jika file yang di edit selain upload.txt maka tidak valid
- Fungsi trl_utimens untuk memperbaruhi timestamp dari file upload dan very_spicy_info
  
![task3_b_output](images/task3_b_output.png)

### Subtask C & D

![task3_cd_code](images/task3_cd_code.png)

- Fungsi is_trap_triggerd untuk mengembalikan access untuk membuka file trap
- Fungsi set_trap_triggered berguna untuk membuka file trap dengan akses write dan jika f true akan menclose file tersebut
- Fungsi trl_getattr mendapat tambahan kode, pada kedua file, jika is_trap_triggered mengubah sizenya sebanyak ascii_art_content dan jika tidak     mengubah sizenya tergantung perbandingan panjang content_daintotas dengan yang lain untuk very_spicy_info dan 0 untuk upload.txt
- Fungsi trl_read mendapat tambahan kode, jika is_trap_triggered mengubah isi_txt sama dengan ascii_art_content dan jika tidak isi_txt sama         dengan content_other untuk very_spicy_info dan "" untuk upload.txt. Kemudian pada if(offset<content_lent) mencopy konten ke buffer dengan         penanganan offset dan ukuran
- Fungsi trl_truncate untuk memotong ukuran file dari upload atau very_spicy_info
- Pada fuse_operation mengaitkan fungsi fungsi diatas dengan callback FUSE
- Pada fungsi main, unmask(0) untuk hak ases default dan return untuk memanggil fuse_main() dengan operasi yang telah didefinisikan

![task3_cd_ouput](images/task3_cd_output.png)

### Laporan task 4

### Subtask A

![task4_a_code](images/task4_a_code.png)

- Pada fungsi printString, menggunakan while untuk membuat loop, yaitu selama karakternya bukan NULL(\0), maka :
  - Menggunakan if untuk memeriksa jika karakter adalah newline(\n) akan menjalankan interrupt berisi '\r' untuk memindahkan kursor ke posisi awal     dan '\n' untuk memindahkan kursor ke baris berikutnya
  - menggunakan else yang berarti selain ketentuan if sebelumnya akan menjalankan interrupt berisi str[i], yaitu setiap karakter yang ada didalam      str, yang menampilkan isi dari string str
- Pada fungsi readString, menggunakan while untuk membuat loop selama benar, maka:
  - char c merupakan karakter yang diketik
  - Menggunakan if untuk memeriksan apakah c =='\r'(enter), '\b'(backspace), atau i<127.
    -Jika enter, buf[i](index terakhir) merupakan null(\0) kemudian loop berhenti
    -Jika backspace, index di decrement, kemudian menggunakan fungsi printSrting untuk menghilangkan isi dari index tadi
    -Jika i<127, buf[i] berisi dari c dan akan ditampilakn dilayar, jika melebihi maka yang lebih tidak akan disimpan ke buf[i] dan tidak akan          muncul
- Pada fungsi clearScreenm menggunakan interupt untuk mengscroll layar hingga kosong lalu mengembalikan posisi kursor di posis awal(0,0)

![task4_a_output](images/task4_a_output.png)

### Subtask B

![task4_b_std_lib.c](images/task4_b_code.png)

- Menggunakan #ifndef untuk mengecek apakah __STD_LIB_H__ sudah didefinisikan atau belum.
- Jika belum, akan mejalankan #define untuk mendefenisikan __STD_LIB_H__
- Menggunakan #include untuk mengimpor definisi data dari std_type.h untuk digunakan di std_lib.h
- Didalam berisi deklarasi fungsi seperti, div, mod, memcpy, dsb
- Menggunakan #endif untuk menyelenyasikan #ifndef

![task4_b_std_lib.h](images/task4_b_code1.png)

- Menggunakan #include " " untuk mengimpor deklarasi fungsi, tipe data, dan macro yang didefinisikan di file header std_lib.h.
- Selanjutnya berisi kode dari setiap fungsi :
  - div untuk pembagian dengan hasil bulat dengan mengabaikan sisa
  - mod untuk sisa pembagian
  - memcpy untuk menyalin data
  - strlen untuk menghitung panjang string
  - strcmp untuk membandingkan string
  - strcpy untuk menyalin string
  - clear untuk mengosongkan buffer
 
### Subtask C & D

![task4_cd_code](images/task4_cd_code.png)

- Pada fungsi handleCommand, berfungsi untuk menyimpan perintah(maks 10), ketika menerima command menggunakan while dan if untuk memeriksa dan       mengubah ('|') menjadi null ('\0'), sehingga kita bisa menghitung jumlah perintah yang diberikan. Kemudian menjalankan fungsi executePipe
- Pada fungsi executePipe :
  - Menggunakan clear(pipe_buffer1,128) untuk menginisialisasi buffer tersebut
  - Menggunakan loop untuk setiap perintah yang diberikan sebelumnya. Didalam loop berisi:
    - pada if(i>0), selama i>0, maka menjalankan kode untuk mengganti isi input_buffer menjadi ouput_buffer dan mengosongkan isi dari ouput_buffer
    - Menjalankan fungsi splitCommand (akan dijelaskan nanti)
    - Menggunakan if dan strcmp untuk membandingkan command yang diterima apakah salah satu dari echo, grep, atau wc.
      - Jika echo, maka menjalankan fungsi strcpy
      - Jika grep, result berisi hasil fungsi strstr(akan dijelaskan nanti), jika result true, menjalankan fungsi strcpy untuk menyalin hasil              kedalam output_buffer, jika false, bershikan isi output_buffer
      - Jika wc, menggunakan loop untuk menghitung jumlah word, strlen untuk jmlh chars dan lines. Menggunakan fungsi  itoa(akan dijelaskan nanti)         untuk lines, word, dan chars. Menggunakan fungsi strcpy untuk menyalin isi line_str ke ouput_buffer lalu strcat(dijelaskan nanti) untuk            menggabungkan yang didalam kurung(  ,  );
      - Jika bukan ketiganya, akan menampilkan Unknown command
  - Setelah loop, menampilkan isi dari ouput_buffer menggunakan printString.

 ![task4_helper_code](images/task4_helper_code.png)

- Pada fungsi trim, berguna untuk menghapus whitespace(spasi, newline, tab)
- Pada fungsi splitCommand, berfungsi untuk mengambil kata pertama sebagai command dan kata selanjutnya menjadi argumen atau string
- Pada fungsi strstr, berfungsi untuk mencari substring didalam string
- Pada fungsi strcat, berfungsi untuk menggabungkan kedua string
- Pada fungsi reverse, membalikkan string yang diterima
- Pada fungsi itoa, menghuba integer ke string

![task4_cd_ouput](images/task4_cd_output.png)

### Subtask E

![task4_e_code](images/task4_e_code.png)

- Pada build :
  - prepare : Membuat floppy disk image kosong
  - bootloader :Kompilasi bootloader
  - stdlib : Kompilasi library standar
  - kernel : Kompilasi kernel (C + Assembly)
  - link : Linking semua komponen dan menulis ke floppy image
- Pada run :
  - -f bochsrc.txt -q : Menggunakan konfigurasi Bochs.(-q(dilatar belakang))
- Pada clean : Menghapus semua file hasil build di direktori bin/
- Pada prepare   : Membuat floppy disk image kosong
  - if=/dev/zero : Mengisi dengan byte 0x00
  - bs=512       : Block size 512 byte (ukuran sector)
  - count=2880   : Total sector (512 * 2880 = 1.44 MB)
- Pada bootloader : Mengompilasi bootloader (Assembly) ke format flat binary
- Pada stdlib :  Mengompilasi library standar (C) ke object file
  - -ansi     : Standar ANSI C
  - -c        : Hanya compile (tanpa linking)
  - -Iinclude : Menambahkan direktori header
- Pada kernel :  Mengompilasi kernel (bagian C + Assembly)
- Pada link : Menggabungkan semua object file (kernel.o, kernel_asm.o, std_lib.o) menjadi kernel.sys

![task4_e_ouput](images/task4_e_output.png)
