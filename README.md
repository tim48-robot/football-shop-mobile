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









