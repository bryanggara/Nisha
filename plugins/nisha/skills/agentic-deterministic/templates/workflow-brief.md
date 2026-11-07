# Brief Workflow / Loop

Isi sebelum menjalankan Workflow tool, subagent paralel, atau loop fix/retry.

1. **Scope**: <folder/file/sumber yang boleh disentuh; yang dilarang>
2. **Objective**: <hasil akhir dalam satu kalimat>
3. **Acceptance criteria (EARS)**:
   - Ketika ..., sistem harus ...
   - Jika ..., maka sistem harus ...
4. **Format output**: <mis. JSON {file, line, severity, bukti}, tabel, diff>
5. **Verifikasi**: <test/command/agent pembantah independen untuk temuan penting>
6. **Stop condition**:
   - done: <kriteria lulus + bukti>
   - stall: <mis. 2 ronde tanpa temuan baru / error sama 2x setelah fix>
   - exhausted: <maks 3 retry per masalah, maks N agent>
   - blocked: <butuh keputusan apa dari user>
7. **Batas keamanan**: <read-only? tanpa perintah destruktif, tanpa migrasi DB, tanpa push/deploy tanpa izin>
8. **Batas biaya**: <jumlah agent, model per fase, estimasi token/waktu>
9. **Slice uji**: <bagian kecil untuk percobaan pertama>
