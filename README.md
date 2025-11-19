# football_shop

A new Flutter project.

## Jawaban atas Pertanyaan (Tugas 7)
- Widget tree itu adalah blueprint/deklarasi untuk bagian UI nya, nanti parent itu yang ngatur constraint, layout dan inheritance nya, hubungan parent-child itu ya hierarki untuk nantinya membangun menata dan mewarnai User Interface, perubahan terjadi kalau ada rebuild (pembuatan widget baru) dan flutter mengoptimalkan update lewat element/render tree

- widget kustom itu kayak myApp(root class), myHomePage(halaman utama), InfoCard (kartu kecil untuk npm,nama,kelas), itemcard untuk tiap opsi di grid. 

Widget bawaan ada banyak kayak:
materialApp(menyediakan tema, navigator, dll)
themeData/colorScheme(untuk material app. membantu mengonfigurasi tema aplikasi, seperti warna dll)
scaffold(appbar, body, drawer, dsb)
AppBar(toolbar di atas halaman, judul, action, buttons)
Text(menampilkan teks)
Column(mengatur child secara vertikal) dan Row(mengatur child secara horizontal)
Padding/EdgeInsets(memberi ruang di sekeliling widget)
SizedBox(memberi jarak atau kotak ukuran tetap)
Center(menempatkan child di tengah parent)
GridView.count(menampilkan grid dengan jumlah kolom tetap)
Card(kotak dengan elevation untuk item terpisah)
Container(kotak serbaguna untuk padding, ukuran, dekorasi)
MediaQuery.of(context).size(mengambil ukuran layar)
Icon/Icons.*(menampilkan ikon vector)
Material(widget visual material yang memberikan warna dan behavior ripple)
InkWell(menambahkan efek ketukan ripple dan callback onTap)
ScaffoldMessenger & SnackBar(menampilkan pesan sementara)
TextStyle(styling untuk Text seperti warna, size, fontWeight)
BorderRadius(membuat sudut melengkung)
const constructors(praktik membuat widget immutable yang bisa dioptimalkan)

- widget MaterialApp itu widget top level yang menyediakan Theme, menyediakan navigator bawaan untuk routing, menyediakan lokal(i18n), locacle dsb dan mengatur title aplikasi, debug banner dan behavior platform specific. Karena MaterialApp mengikat banyak konfigurasi global (tema, navigator, lokal) dia biasa digunakan sebagai widget root pada aplikasi yang pake material design. 

- StatelessWidget itu immutable ga nyimpen state internal, build() bergantung pada constructor dan buildContext, ini dipilih saat widget tidak perlu merubah tampilan berdasasr interaksi. StatefulWidget punya objek State yang terpisah untuk simpan data yang berubah (toggle, nilai input, hasil network), punya method setState() untuk memicu rebulid. Ini saat widget perlu simpan state seperti initState, dispose / didUpdateWidget. 
Overall Stateless sebisa mungkin karena performa lebih baik, Stateful digunakan kalau butuh mutable state lokal kayak form field yang berubah, animation controller dsb

- BuildContext itu object yang mewakili lokasi widget dalam widget tree. Handling ke elemen yang terkait. Penting karena context butuh akses ke info yang diwarisi oleh ancestor. Butuh tau scopenya lah biar tau ancestor/descendantnya. Penggunaan di build() itu kayak Widget build(BuildContext context) yang menggunakan context untuk baca tema, ukuran layar dan dependency dari ancestor. 

- Hot reload itu memasukkan perubahan kode ke running dart vm dan mempertahankan state aplikasi, ini bagus karena cepat, bagus untuk ubah layout, hasil, build tanpa kehilangan state(nilai form contohnya), dia ga aplikasiin perubahan pada kode yang memodifikasi global initialization atau static field yang hanya dieksekusi saat startup. Dia cepat dan mempertahankan state
Kalau hot restart itu memulai ulang aplikasi, restart dart vm sehingga all state kereset. Semua initState() dieksekusi lagi, bila butuh state awal yang bersih maka pake hot restart. Dia itu full reload + reset state.

- Cara saya menambahkan navigasi untuk pindah layar bisa pake navigator 1.0 built in dan materialpageroute, bisa langsung push ke halaman baru gini misalnya
Navigator.of(context).push(
  MaterialPageRoute(builder: (context) => DetailPage(someData)),
);
atau mengembalikan nilai dadri route pake await 
final result = await Navigator.of(context).push(
  MaterialPageRoute(builder: (c) => InputPage()),
);

kalo pake navigator 2.0 / route api itu untuk use-case yang kompleks kayak url, deep link / web. mereka lebiih fleksibel karena declarative routing. 


## Jawaban atas Pertanyaan (Tugas 8)
- beda navigator.push dan navigator.pushreplacement: 
Navigator.push: nambah halaman ke atas stack, bisa balik lagi. bisa buat buka detail produk, lihat gambar, atau form sementara.
Navigator.pushReplacement: ganti halaman sekarang, gak bisa balik. bisa buat setelah login, selesai checkout, atau splash → home.

- cara memanfaatkan hierarchy widget: 
Scaffold itu kerangka utama. taro AppBar di atas untuk judul/aksi, body untuk konten, dan drawer buat navigasi samping.
Taro style AppBar sama setiap halaman supaya tampilan konsisten. Masukin LeftDrawer di Scaffold.drawer biar semua halaman punya menu yang sama.
Nanti tampilan akan rapi dan navigasi seragam.

- kelebihan menggunakan layoutwidget 
Padding: kasih jarak antar field supaya rapi.
SingleChildScrollView: bungkus form yang panjang biar gak overflow waktu keyboard muncul.
ListView: lebih cocok untuk daftar panjang karena lebih efisien (lazy).
Contoh:
Form -> SingleChildScrollView -> Column -> Padding(TextFormField)

- cara menyesuaikan tema: 
Set di MaterialApp lewat theme: ThemeData(colorScheme: ColorScheme.fromSeed(seedColor: Color(0xFF...))).
Pakai Theme.of(context).colorScheme.primary untuk AppBar, dan colorScheme.secondary untuk tombol supaya konsisten.


## Jawaban atas Pertanyaan (Tugas 9)

- model Dart itu kayak blueprint atau template buat data yang kita ambil dari API. Misalnya, kita punya data produk dari Django yang berbentuk JSON, kita bikin class Product di Dart buat representasinya. Tanpa model, data cuma Map<String, dynamic>, jadi kita gak tau tipe datanya. Misal, price harus double, tapi kalau langsung pake map, bisa aja jadi String atau null tanpa error. Dengan model, kita bisa define double price.  Dart punya null-safety, tapi kalau pake map langsung, kita harus selalu cek data['price'] != null manual. Kita juga buat kode jadi lebih rapi dan mudah diubah
Konsekuensinya kalau langsung pake Map<String, dynamic> tanpa model:
Kode jadi berantakan, sulit debug karena error runtime (bukan compile-time).
Gak aman null-safety, bisa crash kalau data kosong.
Susah maintain, kalau API berubah, harus ubah banyak tempat.
Lebih rentan typo, misal data['prce'] alih-alih data['price'], gak ketahuan sampe runtime.

- Package http: Ini library dasar di Dart buat bikin HTTP request ke server. Kita pake http.get(), http.post(), dll., buat kirim data ke Django. Tanpa ini, gak bisa komunikasi sama server. Di tugas ini, kita pake http buat fetch data produk, login, dll.

CookieRequest: Ini dari package pbp_django_auth, yang extend dari http tapi khusus buat handle cookie otomatis. Django pake session-based auth, jadi cookie penting buat maintain login state. CookieRequest otomatis simpan dan kirim cookie di setiap request, jadi kita gak perlu handle manual.

Perbedaan peran:

http: Basic HTTP client, kita handle semua sendiri (header, cookie, dll.). Cocok buat request sederhana tanpa auth.
CookieRequest: Lebih advanced, fokus ke auth. Dia handle cookie otomatis, jadi cocok buat app yang butuh login/logout. Di tugas ini, kita pake CookieRequest karena butuh session auth dari Django.

- Karena CookieRequest itu yang handle session dan cookie, jadi harus sama di seluruh app biar login state konsisten. Kalau setiap screen bikin instance baru, cookie gak tersimpan, jadi user harus login ulang tiap ganti halaman. 

- Tambah 10.0.2.2 ke ALLOWED_HOSTS di Django: Karena emulator Android pake IP khusus 10.0.2.2 buat akses localhost host machine. Tanpa ini, Django tolak request dari emulator karena IP gak di-allow, jadi error 400 atau forbidden.

Aktifkan CORS: Cross-Origin Resource Sharing. Flutter (web atau app) beda origin sama Django server. Tanpa CORS, browser blok request, jadi gak bisa fetch data. Kita install django-cors-headers dan allow semua origin atau specific.

Pengaturan SameSite/Cookie: Cookie di Django harus set SameSite=None atau Lax biar bisa dikirim cross-origin. Kalau gak, cookie gak ikut dikirim, jadi auth gagal.

Izin Akses Internet di Android: Di android/app/src/main/AndroidManifest.xml, tambah <uses-permission android:name="android.permission.INTERNET" />. Tanpa ini, app Android gak bisa akses internet.

- User input data di form (misal, nama produk di product_form.dart).
Form validasi input, kirim ke create_product_flutter di Django via CookieRequest.post().
Django terima data, validasi, simpan ke DB, return success JSON.
Flutter terima response, kalau success, update UI (misal, refresh list produk).
Data dari DB ditarik via show_json, di-parse jadi model Product, ditampilkan di ListView.

- Register: User isi form di Flutter, kirim ke /auth/register/ Django. Django cek username unik, bikin user baru, return success. Flutter navigate ke login.
Login: User isi username/password, kirim ke /auth/login/ Django. Django authenticate, kalau bener set session cookie, return user data. Flutter simpan cookie via CookieRequest, navigate ke menu.
Session Maintenance: CookieRequest kirim cookie di setiap request, jadi Django tau user logged in.
Logout: User klik logout, kirim ke /auth/logout/ Django. Django clear session, delete cookie. Flutter clear state, back ke login

- 1. Memastikan Deployment Django Berjalan Baik
Clone repo football-ronaldo dari GitHub.
Install dependencies: pip install -r requirements.txt.
Run migrations: python manage.py migrate.
Start server: python manage.py runserver.
Test endpoint /json/ di browser atau Postman, pastikan return JSON produk.
2. Implementasi Fitur Registrasi Akun di Flutter
Bikin screen register.dart dengan form fields: username, password1, password2.
Pake CookieRequest.post() ke /auth/register/ Django.
Handle response: kalau success, navigate ke login; kalau error, tampilkan pesan.
3. Membuat Halaman Login di Flutter
Bikin screen login.dart dengan form: username, password.
Kirim POST ke /auth/login/ Django via CookieRequest.
Kalau success, simpan cookie dan navigate ke menu; kalau gagal, tampil error.
4. Mengintegrasikan Sistem Autentikasi Django dengan Flutter
Pake package pbp_django_auth buat CookieRequest, yang handle session cookie otomatis.
Wrap app dengan Provider buat share CookieRequest instance.
Di setiap request, cookie dikirim otomatis, jadi Django tau user logged in.
Tambah logout: POST ke /auth/logout/, clear cookie, back ke login.
5. Membuat Model Kustom Sesuai Django
Bikin class Product di models/product.dart dengan fields: id, name, price, description, thumbnail, category, is_featured, user.
Tambah factory fromJson() buat parse JSON dari Django.
Tambah toJson() buat kirim data balik.
6-7. Membuat Halaman Daftar Item dari Endpoint JSON
Bikin screen product_list.dart dengan FutureBuilder buat fetch data.
Fetch dari /json/ Django, parse jadi List<Product>.
Tampilkan di ListView dengan ProductCard, show: name, price, description, thumbnail, category, is_featured.
8-11. Membuat Halaman Detail Item
Bikin screen product_detail.dart, terima Product object dari navigation.
Tampilkan semua atribut: id, name, price, dll.
Tambah ElevatedButton buat back ke list.
12. Filter Item Berdasarkan User Login
Di fetchProducts(), setelah fetch user dari /auth/user/, filter listProduct.where((p) => p.user == userId).
Jadi cuma tampil produk milik user yang login.






