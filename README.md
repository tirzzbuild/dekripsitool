# Dekripsi Engine v3 — Auto Discovery

Tool dekripsi client-side dengan UI **neo-brutalism**. Tidak ada input
manual untuk kunci maupun nama array — semuanya dideteksi otomatis.

## Auto-Discovery

### 7 pola string-array
1. `var/let/const NAME = ['..', '..']`
2. `function NAME(){ const A=[..]; return NAME(); }`
3. IIFE: `(function(){ const A=[..]; })()`
4. Object map: `const NAME = { a:'..', b:'..' }`
5. Array of arrays: `NAME = [['..','..'],['..']]`
6. `NAME = "a|b|c".split("|")`
7. `NAME = ['..'].concat(['..'])`

### 12 strategi kunci
1. Argumen literal `decode('kunci')`
2. Parameter fungsi + var key
3. `var key = '...'` / secret / pass / kunci / token
4. `rc4('kunci', ...)` / xor / decrypt
5. `atob('...')` → decode base64 sebagai kunci
6. IIFE argumen `}('kunci', ...)`
7. `['kunci'][0]`
8. Concat multi string `'a'+'b'`
9. Array join `['a','b'].join('')`
10. Reverse string `'...'.split('').reverse().join('')`
11. Hex string panjang
12. Fallback string literal panjang

### Scoring otomatis
Untuk setiap kombinasi array × kunci, decode semua item, hitung skor
teks (huruf=3, spasi=2, angka=1, simbol kode=2, kontrol=-5). Kombinasi
dengan skor tertinggi dipilih.

## Pipeline
Scan Array → Find Keys → Rotasi → Decode → Cache → Replace → Nested → Score

## Deploy
1. `npm i -g vercel`
2. `cd dekripsi-v3`
3. `vercel`
4. Atau drag-and-drop ke https://vercel.com/new

## Catatan
100% client-side. Tidak ada data dikirim ke server.