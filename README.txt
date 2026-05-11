Jawaban Refleksi (Nomor 4) — Readme saja

1) Mengapa fungsi power() harus dipanggil di dalam term(), bukan sebaliknya? Jelaskan kaitannya dengan Operator Precedence.

Karena precedence operator ditentukan oleh struktur pemanggilan fungsi parser. Di kode ini:
- `expr()` memproses `+` dan `-` di level paling luar.
- `term()` memproses `*` dan `/`.
- Untuk membuat `^` memiliki prioritas lebih tinggi daripada `*` dan `/`, maka `term()` harus memulai parsing dari `power()`.

Secara konseptual, urutan prioritas dibentuk oleh hirarki:
`expr -> term -> power -> factor`
Sehingga ekspresi seperti `a ^ b * c` akan diparsing sebagai `(a ^ b) * c` (karena `term()` menerima hasil `power()` terlebih dahulu), bukan `a ^ (b * c)`.

2) Apa yang terjadi pada fase Analisis Semantik jika variabel z digunakan dalam kode sumber tetapi tidak ada di symbol_table?

Jika `z` tidak ada dalam `symbol_table`, maka fase analisis semantik (dalam parser pada bagian `factor()` yang mengecek definisi variabel) akan memunculkan error:
`ParserError(f"Semantic Error: Undefined variable 'z'")`
Artinya kompilasi dihentikan karena program menggunakan identifier yang tidak didefinisikan.

3) Jelaskan mengapa dalam TAC, instruksi untuk a ^ 2 harus muncul sebelum instruksi untuk +.

Karena `^` memiliki prioritas lebih tinggi daripada `+`. Saat pembentukan AST dan kemudian generasi TAC, subtree untuk `a ^ 2` diproses lebih dulu sehingga nilai hasil pangkat menjadi operand yang akan dijumlahkan.

Dengan urutan yang benar, TAC akan membentuk temporary untuk `a ^ 2` terlebih dahulu (mis. `t1 = a ^ 2`), baru kemudian menggunakan temporary itu pada operasi `+` (mis. `t3 = t1 + t2`). Ini memastikan evaluasi mengikuti urutan operasi yang benar sesuai precedence.

