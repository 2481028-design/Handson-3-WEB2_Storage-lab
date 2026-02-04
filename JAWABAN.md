Eksperimen 1:
    - cookies: masih ada
    - localstorage: masih ada
    - session storage: masih ada

Eksperimen 2: Tab isolation Catat: Berapa nilai counter di tab baru: 5

Mengapa session token TIDAK boleh disimpan di LocalStorage?
Session token tidak boleh disimpan di LocalStorage karena LocalStorage sepenuhnya dapat diakses oleh JavaScript yang berjalan di halaman tersebut. Jika aplikasi memiliki celah keamanan seperti Cross-Site Scripting (XSS), penyerang dapat menyuntikkan skrip berbahaya dan dengan mudah membaca, menyalin, lalu mengirim session token ke server mereka sendiri. Ketika token ini dicuri, penyerang bisa melakukan session hijacking, yaitu mengambil alih sesi pengguna tanpa perlu mengetahui username dan password.
Selain itu, LocalStorage tidak memiliki mekanisme keamanan bawaan seperti HttpOnly, sehingga tidak ada cara untuk mencegah JavaScript mengakses data sensitif di dalamnya. Token yang tersimpan di LocalStorage juga bersifat persisten sampai dihapus secara manual, sehingga risiko kebocoran tetap ada meskipun browser ditutup dan dibuka kembali. Karena alasan-alasan tersebut, menyimpan session token di LocalStorage dianggap praktik yang tidak aman untuk data autentikasi.

Apa keuntungan SessionStorage untuk multi-step form dibanding LocalStorage?
SessionStorage memiliki keunggulan utama berupa isolasi per tab dan masa hidup yang terbatas pada satu sesi tab browser. Artinya, data yang disimpan di SessionStorage hanya tersedia di tab tempat data tersebut dibuat dan akan otomatis terhapus ketika tab tersebut ditutup.
Dalam konteks multi-step form (misalnya form pendaftaran atau checkout), perilaku ini sangat ideal karena data hanya dibutuhkan sementara selama pengguna menyelesaikan langkah-langkah form. Jika pengguna menutup tab atau membuka form di tab lain, data tidak tercampur atau tertimpa, sehingga mengurangi potensi bug dan kebingungan state.
Sebaliknya, jika menggunakan LocalStorage, data form bisa tetap tersimpan lama dan terbuka di banyak tab sekaligus, yang dapat menyebabkan inkonsistensi data, kebocoran informasi, atau pengalaman pengguna yang membingungkan. Oleh karena itu, SessionStorage lebih tepat untuk state sementara yang tidak perlu bertahan lama.

Jika kamu membuat aplikasi todo list offline-first, storage mana yang akan kamu gunakan dan mengapa?
Untuk aplikasi todo list dengan pendekatan offline-first, pilihan terbaik adalah IndexedDB. IndexedDB dirancang sebagai database di sisi klien yang mampu menyimpan data dalam jumlah besar, terstruktur, dan kompleks (misalnya objek, array, relasi sederhana).
IndexedDB juga bersifat asynchronous, sehingga tidak memblokir thread utama JavaScript, yang sangat penting untuk menjaga performa aplikasi tetap responsif. Dalam aplikasi offline-first, data todo dapat disimpan secara lokal, dimodifikasi tanpa koneksi internet, lalu disinkronkan ke server ketika koneksi kembali tersedia.
Dibandingkan LocalStorage atau SessionStorage yang hanya mendukung penyimpanan key–value sederhana dan memiliki keterbatasan kapasitas, IndexedDB jauh lebih skalabel dan cocok untuk kebutuhan cache, sinkronisasi, serta manajemen data jangka panjang di aplikasi modern.

Bonus – Ringkasan Penggunaan Storage yang Tepat

Cookie sebaiknya digunakan ketika data perlu otomatis ikut terkirim ke server, seperti session login atau preferensi ringan. Cookie menyediakan kontrol keamanan penting seperti HttpOnly, Secure, dan SameSite yang membantu mengurangi risiko XSS dan CSRF. Namun, ukurannya terbatas dan tetap berisiko jika tidak dikonfigurasi dengan benar, sehingga isinya harus sekecil dan sesensitif mungkin.

LocalStorage cocok untuk menyimpan data non-sensitif yang perlu bertahan lama di sisi klien, seperti tema UI, bahasa pilihan, atau cache ringan. Karena mudah diakses JavaScript dan tidak memiliki proteksi khusus, LocalStorage tidak boleh digunakan untuk menyimpan data rahasia.

SessionStorage ideal untuk data yang hanya relevan selama satu sesi atau satu tab browser, seperti state sementara form, wizard langkah-langkah, atau data UI sementara. Data akan otomatis hilang saat tab ditutup, sehingga lebih aman untuk konteks jangka pendek.

IndexedDB paling tepat untuk data besar, terstruktur, dan aplikasi berbasis offline-first atau cache kompleks. Meskipun lebih kuat, prinsip keamanannya tetap sama: hindari menyimpan rahasia atau token autentikasi di storage yang dapat diakses JavaScript.