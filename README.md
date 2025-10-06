<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Definisi Dasar Dari Telekomunikasi?", "id": "Komunikasi Jarak Jauh Melalui Media Elektronik." },
  { "en": "Sebutkan Tiga Komponen Utama Sistem Telekomunikasi?", "id": "Pengirim, Media Transmisi, Dan Juga Penerima." },
  { "en": "Apa Fungsi Utama Dari Pengirim (Transmitter)?", "id": "Mengubah Informasi Menjadi Sinyal Siap Kirim." },
  { "en": "Apa Fungsi Utama Dari Penerima (Receiver)?", "id": "Mengubah Sinyal Kembali Menjadi Informasi Asli." },
  { "en": "Apa Yang Dimaksud Dengan Media Transmisi?", "id": "Saluran Fisik Atau Non-Fisik Pengiriman Sinyal." },
  { "en": "Sebutkan Contoh Media Transmisi Fisik?", "id": "Kabel Tembaga, Koaksial, Dan Serat Optik." },
  { "en": "Sebutkan Contoh Media Transmisi Non-Fisik?", "id": "Gelombang Radio, Mikro, Dan Sinar Inframerah." },
  { "en": "Siapakah Penemu Telegraf Listrik Yang Terkenal?", "id": "Samuel Morse Adalah Penemu Telegraf Listrik." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Telepon?", "id": "Alexander Graham Bell Penemu Alat Telepon." },
  { "en": "Apa Perbedaan Utama Sinyal Analog Digital?", "id": "Analog Kontinu, Sedangkan Digital Adalah Diskret." },
  { "en": "Apa Satuan Untuk Mengukur Frekuensi Sinyal?", "id": "Satuan Frekuensi Adalah Hertz Atau Hz." },
  { "en": "Apa Yang Dimaksud Dengan Bandwidth (Lebar Pita)?", "id": "Rentang Frekuensi Dalam Saluran Komunikasi Tertentu." },
  { "en": "Apa Kepanjangan Dari Istilah AM Radio?", "id": "AM Adalah Singkatan Amplitude Modulation Radio." },
  { "en": "Apa Kepanjangan Dari Istilah FM Radio?", "id": "FM Adalah Singkatan Frequency Modulation Radio." },
  { "en": "Apa Itu Proses Modulasi Dalam Telekomunikasi?", "id": "Proses Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Tujuan Utama Dilakukannya Proses Modulasi?", "id": "Agar Sinyal Dapat Dikirim Secara Efisien." },
  { "en": "Sinyal Apa Yang Membawa Informasi Asli?", "id": "Sinyal Modulasi Atau Baseband Membawa Informasi." },
  { "en": "Sinyal Apa Yang Digunakan Sebagai Pembawa?", "id": "Sinyal Carrier Dengan Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Demodulasi Di Sisi Penerima?", "id": "Proses Memisahkan Sinyal Informasi Dari Pembawa." },
  { "en": "Apa Itu Noise Dalam Sistem Komunikasi?", "id": "Gangguan Yang Tidak Diinginkan Pada Sinyal." },
  { "en": "Bagaimana Noise Mempengaruhi Kualitas Sinyal Telekomunikasi?", "id": "Dapat Merusak Dan Mengubah Informasi Asli." },
  { "en": "Apa Yang Dimaksud Dengan Atenuasi Sinyal?", "id": "Pelemahan Kekuatan Sinyal Selama Proses Transmisi." },
  { "en": "Apa Fungsi Amplifier Dalam Sistem Telekomunikasi?", "id": "Menguatkan Kembali Sinyal Yang Telah Melemah." },
  { "en": "Sebutkan Tipe Komunikasi Berdasarkan Arah Transmisi?", "id": "Simplex, Half-Duplex, Dan Juga Full-Duplex." },
  { "en": "Jelaskan Apa Itu Mode Komunikasi Simplex?", "id": "Komunikasi Hanya Bisa Dilakukan Satu Arah." },
  { "en": "Berikan Contoh Nyata Dari Komunikasi Simplex?", "id": "Siaran Radio Atau Siaran Televisi Konvensional." },
  { "en": "Jelaskan Apa Itu Mode Komunikasi Half-Duplex?", "id": "Komunikasi Dua Arah Tetapi Secara Bergantian." },
  { "en": "Berikan Contoh Nyata Dari Komunikasi Half-Duplex?", "id": "Contohnya Adalah Penggunaan Walkie-Talkie Atau HT." },
  { "en": "Jelaskan Apa Itu Mode Komunikasi Full-Duplex?", "id": "Komunikasi Dua Arah Secara Bersamaan Terjadi." },
  { "en": "Berikan Contoh Nyata Dari Komunikasi Full-Duplex?", "id": "Contoh Paling Umum Adalah Percakapan Telepon." },
  { "en": "Apa Bahan Utama Dari Kabel Serat Optik?", "id": "Terbuat Dari Serat Kaca Atau Plastik." },
  { "en": "Apa Keunggulan Utama Penggunaan Kabel Serat Optik?", "id": "Bandwidth Besar, Kecepatan Tinggi, Tahan Interferensi." },
  { "en": "Apa Kerugian Utama Dari Kabel Tembaga?", "id": "Rentan Terhadap Interferensi Dan Juga Atenuasi." },
  { "en": "Apa Itu Gelombang Elektromagnetik Dalam Telekomunikasi?", "id": "Gelombang Yang Digunakan Untuk Transmisi Nirkabel." },
  { "en": "Apa Itu Spektrum Frekuensi Elektromagnetik Terpakai?", "id": "Rentang Semua Frekuensi Gelombang Elektromagnetik Mungkin." },
  { "en": "Mengapa Spektrum Frekuensi Perlu Diatur Ketat?", "id": "Untuk Menghindari Terjadinya Interferensi Antar Pengguna." },
  { "en": "Lembaga Apa Yang Mengatur Frekuensi Di Indonesia?", "id": "Kementerian Komunikasi Dan Informatika Republik Indonesia." },
  { "en": "Apa Satuan Dasar Informasi Digital Sekarang?", "id": "Satuan Dasar Informasi Adalah Bit (Binary Digit)." },
  { "en": "Berapa Nilai Yang Dimiliki Oleh Satu Bit?", "id": "Hanya Memiliki Dua Nilai, 0 Atau 1." },
  { "en": "Apa Itu Proses Konversi Analog Ke Digital?", "id": "Mengubah Sinyal Analog Menjadi Deretan Bit." },
  { "en": "Apa Nama Alat Untuk Konversi Analog-Digital?", "id": "Alatnya Bernama ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Nama Alat Untuk Konversi Digital-Analog?", "id": "Alatnya Bernama DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Keuntungan Utama Dari Sistem Komunikasi Digital?", "id": "Lebih Tahan Noise Dan Mudah Diproses." },
  { "en": "Apa Itu Multiplexing Dalam Dunia Telekomunikasi?", "id": "Menggabungkan Beberapa Sinyal Menjadi Satu Saluran." },
  { "en": "Apa Tujuan Utama Dari Teknik Multiplexing?", "id": "Meningkatkan Efisiensi Penggunaan Saluran Komunikasi." },
  { "en": "Sebutkan Jenis Multiplexing Yang Paling Umum?", "id": "FDM (Frequency Division Multiplexing), TDM (Time Division Multiplexing)." },
  { "en": "Bagaimana Cara Kerja FDM (Frequency Division Multiplexing)?", "id": "Membagi Saluran Berdasarkan Pita Frekuensi Berbeda." },
  { "en": "Bagaimana Cara Kerja TDM (Time Division Multiplexing)?", "id": "Membagi Saluran Berdasarkan Slot Waktu Bergantian." },
  { "en": "Apa Itu Jaringan Telekomunikasi Secara Umum?", "id": "Kumpulan Perangkat Untuk Komunikasi Jarak Jauh." },
  { "en": "Sebutkan Contoh Jaringan Telekomunikasi Publik?", "id": "PSTN (Public Switched Telephone Network) Adalah Contoh." },
  { "en": "Apa Fungsi Dari Switching Dalam Jaringan?", "id": "Menghubungkan Pengirim Dan Penerima Secara Tepat." },
  { "en": "Apa Itu Antena Dalam Komunikasi Nirkabel?", "id": "Perangkat Untuk Memancarkan Menerima Gelombang Radio." },
  { "en": "Apa Fungsi Utama Dari Sebuah Router Jaringan?", "id": "Meneruskan Paket Data Antar Jaringan Berbeda." },
  { "en": "Apa Itu Protokol Dalam Jaringan Komputer?", "id": "Aturan Yang Mengatur Komunikasi Antar Perangkat." },
  { "en": "Sebutkan Contoh Protokol Jaringan Yang Populer?", "id": "TCP/IP (Transmission Control Protocol/Internet Protocol)." },
  { "en": "Apa Yang Dimaksud Dengan Topologi Jaringan?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Sebutkan Beberapa Contoh Topologi Jaringan Fisik?", "id": "Bus, Star, Ring, Dan Juga Mesh." },
  { "en": "Apa Karakteristik Utama Dari Topologi Star?", "id": "Semua Perangkat Terhubung Ke Sebuah Hub/Switch." },
  { "en": "Apa Karakteristik Utama Dari Topologi Bus?", "id": "Semua Perangkat Terhubung Pada Satu Kabel." },
  { "en": "Apa Itu Internet Secara Definisi Sederhana?", "id": "Jaringan Komputer Global Yang Saling Terhubung." },
  { "en": "Apa Kepanjangan Dari WWW Dalam Internet?", "id": "WWW Adalah Singkatan Dari World Wide Web." },
  { "en": "Apa Fungsi Dari ISP (Internet Service Provider)?", "id": "Menyediakan Layanan Akses Internet Kepada Pengguna." },
  { "en": "Apa Itu Latency Dalam Jaringan Komunikasi?", "id": "Waktu Tunda Pengiriman Data Dari Sumber." },
  { "en": "Apa Satuan Yang Digunakan Untuk Latency?", "id": "Biasanya Diukur Dalam Milidetik Atau ms." },
  { "en": "Apa Itu Jitter Dalam Jaringan Digital?", "id": "Variasi Waktu Tiba Paket Data Diterima." },
  { "en": "Apa Itu Packet Loss Dalam Jaringan?", "id": "Kegagalan Paket Data Sampai Ke Tujuannya." },
  { "en": "Apa Perbedaan Antara Data Dan Informasi?", "id": "Data Mentah, Informasi Adalah Data Bermakna." },
  { "en": "Apa Fungsi Dari Modem (Modulator-Demodulator)?", "id": "Mengubah Sinyal Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Komunikasi Satelit Dalam Telekomunikasi?", "id": "Menggunakan Satelit Sebagai Stasiun Relay Di Angkasa." },
  { "en": "Sebutkan Keuntungan Utama Komunikasi Via Satelit?", "id": "Jangkauan Sangat Luas Mencakup Seluruh Dunia." },
  { "en": "Apa Itu Propagasi Gelombang Radio Di Udara?", "id": "Cara Gelombang Radio Merambat Dari Pemancar." },
  { "en": "Sebutkan Tiga Jenis Propagasi Gelombang Radio?", "id": "Ground Wave, Sky Wave, Space Wave." },
  { "en": "Bagaimana Cara Propagasi Ground Wave Merambat?", "id": "Mengikuti Kontur Atau Lekukan Permukaan Bumi." },
  { "en": "Bagaimana Cara Propagasi Sky Wave Merambat?", "id": "Dipantulkan Kembali Ke Bumi Oleh Ionosfer." },
  { "en": "Bagaimana Cara Propagasi Space Wave Merambat?", "id": "Merambat Dalam Garis Lurus (Line of Sight)." },
  { "en": "Frekuensi Apa Yang Menggunakan Propagasi Sky Wave?", "id": "Frekuensi Tinggi Atau HF (High Frequency)." },
  { "en": "Frekuensi Apa Yang Menggunakan Propagasi Space Wave?", "id": "Frekuensi Sangat Tinggi VHF, UHF, SHF." },
  { "en": "Apa Itu Line of Sight (LOS) Propagation?", "id": "Tidak Ada Halangan Antara Pemancar Penerima." },
  { "en": "Apa Itu Sistem Komunikasi Seluler Modern?", "id": "Jaringan Nirkabel Terbagi Dalam Wilayah Sel." },
  { "en": "Apa Fungsi Dari BTS (Base Transceiver Station)?", "id": "Menghubungkan Perangkat Seluler Dengan Jaringan Utama." },
  { "en": "Apa Kepanjangan Dari Teknologi LTE Telekomunikasi?", "id": "LTE Adalah Singkatan Long-Term Evolution." },
  { "en": "Apa Generasi Jaringan Seluler Saat Ini?", "id": "Generasi Kelima Atau Dikenal Sebagai 5G." },
  { "en": "Apa Keunggulan Utama Dari Jaringan 5G?", "id": "Kecepatan Sangat Tinggi Dan Latensi Rendah." },
  { "en": "Apa Itu Kode Morse Dalam Telegrafi?", "id": "Sistem Representasi Huruf Angka Dengan Titik." },
  { "en": "Apa Itu Throughput Dalam Sebuah Jaringan?", "id": "Laju Transfer Data Aktual Yang Berhasil." },
  { "en": "Apa Perbedaan Throughput Dengan Bandwidth Jaringan?", "id": "Bandwidth Teoritis, Throughput Adalah Kecepatan Aktual." },
  { "en": "Apa Itu Pengkodean Saluran (Channel Coding)?", "id": "Teknik Menambahkan Redundansi Untuk Deteksi Error." },
  { "en": "Apa Tujuan Utama Dari Channel Coding?", "id": "Meningkatkan Keandalan Transmisi Data Di Kanal." },
  { "en": "Apa Itu Error Detection Dalam Komunikasi?", "id": "Proses Mendeteksi Apakah Ada Error Data." },
  { "en": "Apa Itu Error Correction Dalam Komunikasi?", "id": "Proses Memperbaiki Error Data Yang Ditemukan." },
  { "en": "Sebutkan Contoh Teknik Deteksi Error Sederhana?", "id": "Parity Check Adalah Salah Satu Contohnya." },
  { "en": "Apa Itu Interferensi Dalam Komunikasi Nirkabel?", "id": "Gangguan Dari Sinyal Lain Yang Tidak." },
  { "en": "Sebutkan Dua Tipe Utama Dari Interferensi?", "id": "Co-channel Interference Dan Adjacent Channel Interference." },
  { "en": "Apa Itu Fading Dalam Komunikasi Nirkabel?", "id": "Variasi Kekuatan Sinyal Diterima Oleh Penerima." },
  { "en": "Apa Itu Sinyal Baseband Dalam Telekomunikasi?", "id": "Sinyal Informasi Asli Sebelum Proses Modulasi." },
  { "en": "Apa Itu Sinyal Passband Dalam Telekomunikasi?", "id": "Sinyal Yang Telah Dimodulasi Ke Frekuensi." },
  { "en": "Apa Itu Sistem Trunking Radio Profesional?", "id": "Berbagi Sedikit Saluran Untuk Banyak Pengguna." },
  { "en": "Apa Kepanjangan Dari LAN Dalam Jaringan Komputer?", "id": "LAN Adalah Singkatan Local Area Network." },
  { "en": "Apa Cakupan Geografis Dari Jaringan LAN?", "id": "Area Lokal Terbatas, Seperti Satu Gedung." },
  { "en": "Apa Kepanjangan Dari WAN Dalam Jaringan Komputer?", "id": "WAN Adalah Singkatan Wide Area Network." },
  { "en": "Apa Cakupan Geografis Dari Jaringan WAN?", "id": "Area Sangat Luas, Antar Kota, Negara." },
  { "en": "Apa Fungsi Dari IP (Internet Protocol) Address?", "id": "Alamat Unik Perangkat Dalam Suatu Jaringan." },
  { "en": "Apa Kepanjangan Dari DNS Dalam Internet?", "id": "DNS Adalah Domain Name System Server." },
  { "en": "Apa Fungsi Utama Dari Server DNS?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP." },
  { "en": "Apa Kepanjangan Dari HTTP Yang Sering Digunakan?", "id": "HTTP Adalah Hypertext Transfer Protocol." },
  { "en": "Apa Fungsi Dari Protokol HTTP Di Internet?", "id": "Mengatur Komunikasi Antara Web Browser Server." },
  { "en": "Apa Kepanjangan Dari FTP Dalam Transfer Data?", "id": "FTP Adalah Singkatan File Transfer Protocol." },
  { "en": "Apa Kegunaan Utama Dari Protokol FTP?", "id": "Untuk Mengirim Dan Menerima File Komputer." },
  { "en": "Perangkat Apa Yang Menghubungkan Jaringan LAN Berbeda?", "id": "Router Adalah Perangkat Untuk Menghubungkan Jaringan." },
  { "en": "Apa Fungsi Dari Switch Dalam Jaringan LAN?", "id": "Menghubungkan Perangkat Dalam Satu Jaringan Lokal." },
  { "en": "Apa Perbedaan Utama Antara Hub Dan Switch?", "id": "Switch Lebih Cerdas, Mengirim Data Tertuju." },
  { "en": "Apa Itu Firewall Dalam Keamanan Jaringan Komputer?", "id": "Sistem Keamanan Yang Melindungi Jaringan Internal." },
  { "en": "Apa Itu Bandwidth Dalam Konteks Jaringan Digital?", "id": "Kapasitas Maksimal Transfer Data Suatu Jalur." },
  { "en": "Apa Satuan Umum Untuk Mengukur Kecepatan Internet?", "id": "Megabit Per Detik Atau Disingkat Mbps." },
  { "en": "Apa Kepanjangan Dari Wi-Fi Teknologi Nirkabel?", "id": "Wi-Fi Adalah Wireless Fidelity, Sebuah Merek." },
  { "en": "Standar IEEE Apa Yang Digunakan Untuk Wi-Fi?", "id": "Standar Yang Digunakan Adalah IEEE 802.11." },
  { "en": "Apa Fungsi Dari Access Point (AP)?", "id": "Memancarkan Sinyal Wi-Fi Untuk Perangkat Terhubung." },
  { "en": "Apa Itu Teknologi Komunikasi Nirkabel Bluetooth?", "id": "Standar Nirkabel Untuk Pertukaran Data Dekat." },
  { "en": "Apa Jangkauan Maksimal Efektif Dari Sinyal Bluetooth?", "id": "Jangkauan Jarak Pendek, Sekitar Sepuluh Meter." },
  { "en": "Apa Teknologi Utama Jaringan Seluler Generasi Pertama?", "id": "Menggunakan Teknologi Sistem Komunikasi Sinyal Analog." },
  { "en": "Apa Fitur Utama Jaringan Seluler Generasi Kedua?", "id": "Transisi Ke Teknologi Digital, SMS, MMS." },
  { "en": "Apa Kepanjangan Dari GSM Jaringan Seluler 2G?", "id": "GSM Adalah Global System for Mobiles." },
  { "en": "Apa Peningkatan Utama Jaringan Seluler Generasi Ketiga?", "id": "Kecepatan Data Lebih Tinggi, Akses Internet." },
  { "en": "Apa Peningkatan Signifikan Jaringan Seluler Generasi Keempat?", "id": "Kecepatan Sangat Tinggi, Streaming Video HD." },
  { "en": "Apa Yang Dimaksud Handoff Dalam Komunikasi Seluler?", "id": "Proses Perpindahan Panggilan Antar Sel Berbeda." },
  { "en": "Apa Itu SIM (Subscriber Identity Module) Card?", "id": "Kartu Untuk Mengidentifikasi Pelanggan Jaringan Seluler." },
  { "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor Unik Untuk Mengidentifikasi Perangkat Seluler." },
  { "en": "Apa Itu Sampling Dalam Konversi Sinyal Analog?", "id": "Proses Mengambil Sampel Amplitudo Sinyal Analog." },
  { "en": "Apa Itu Teorema Sampling Nyquist-Shannon?", "id": "Frekuensi Sampling Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Kuantisasi (Quantization) Sinyal Digital?", "id": "Proses Memetakan Sampel Ke Level Tertentu." },
  { "en": "Apa Itu Error Kuantisasi Dalam Proses ADC?", "id": "Selisih Antara Nilai Sampel Dan Kuantisasi." },
  { "en": "Apa Kepanjangan Dari PCM Sistem Transmisi Digital?", "id": "PCM Adalah Singkatan Pulse Code Modulation." },
  { "en": "Sebutkan Tiga Proses Utama Dalam Teknik PCM?", "id": "Sampling, Kuantisasi, Dan Juga Proses Encoding." },
  { "en": "Apa Itu Line Coding Dalam Transmisi Digital?", "id": "Metode Merepresentasikan Data Digital Menjadi Sinyal." },
  { "en": "Sebutkan Contoh Metode Line Coding Populer?", "id": "NRZ (Non-Return-to-Zero), Manchester, Dan Bipolar." },
  { "en": "Apa Keuntungan Dari Pengkodean Manchester Dibanding NRZ?", "id": "Memiliki Sinkronisasi Clock Yang Jauh Lebih Baik." },
  { "en": "Apa Itu Bit Rate Dalam Komunikasi Digital?", "id": "Jumlah Bit Yang Dikirim Per Satuan Detik." },
  { "en": "Apa Itu Baud Rate Dalam Sistem Komunikasi?", "id": "Jumlah Perubahan Sinyal Atau Simbol Perdetik." },
  { "en": "Apakah Bit Rate Selalu Sama Dengan Baud?", "id": "Tidak Selalu, Tergantung Pada Teknik Modulasi." },
  { "en": "Apa Itu ISI (Inter-Symbol Interference) Digital?", "id": "Distorsi Sinyal Dimana Simbol Saling Mengganggu." },
  { "en": "Apa Penyebab Utama Terjadinya Inter-Symbol Interference?", "id": "Bandwidth Kanal Yang Terbatas Atau Tidak." },
  { "en": "Apa Itu Eye Diagram Atau Pola Mata?", "id": "Alat Untuk Menganalisis Kualitas Sinyal Digital." },
  { "en": "Informasi Apa Yang Didapat Dari Eye Diagram?", "id": "Tingkat Noise, Jitter, Dan Juga ISI." },
  { "en": "Apa Itu Gain Atau Penguatan Sebuah Antena?", "id": "Kemampuan Antena Memfokuskan Energi Arah Tertentu." },
  { "en": "Apa Satuan Umum Untuk Mengukur Gain Antena?", "id": "Satuan Pengukuran Adalah Desibel Isotropik (dBi)." },
  { "en": "Apa Itu Antena Isotropik Sebagai Sebuah Referensi?", "id": "Antena Ideal Memancarkan Energi Sama Rata." },
  { "en": "Apa Itu Polarisasi Gelombang Elektromagnetik Radio?", "id": "Orientasi Medan Listrik Gelombang Radio Terpancar." },
  { "en": "Sebutkan Dua Jenis Polarisasi Linear Utama?", "id": "Polarisasi Vertikal Dan Juga Polarisasi Horizontal." },
  { "en": "Apa Itu Direktovitas (Directivity) Sebuah Antena?", "id": "Ukuran Kemampuan Antena Mengarahkan Energi Pancar." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern) Antena?", "id": "Representasi Grafis Kekuatan Sinyal Dari Antena." },
  { "en": "Apa Bentuk Paling Sederhana Dari Antena?", "id": "Antena Dipole Setengah Panjang Gelombang Listrik." },
  { "en": "Apa Karakteristik Utama Dari Antena Yagi-Uda?", "id": "Memiliki Gain Tinggi Dan Sangat Direktif." },
  { "en": "Dimana Antena Yagi-Uda Sering Digunakan Manusia?", "id": "Umumnya Digunakan Untuk Penerima Siaran Televisi." },
  { "en": "Apa Karakteristik Utama Dari Antena Parabola?", "id": "Gain Sangat Tinggi, Untuk Komunikasi Jarak." },
  { "en": "Apa Fungsi Reflektor Pada Antena Parabola?", "id": "Memfokuskan Gelombang Elektromagnetik Ke Satu Titik." },
  { "en": "Apa Itu Impedansi Input Dari Sebuah Antena?", "id": "Ukuran Hambatan Antena Pada Frekuensi Operasi." },
  { "en": "Mengapa Impedansi Antena Harus Sesuai Transmisi?", "id": "Untuk Transfer Daya Maksimal, Menghindari Pantulan." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Ketidaksesuaian Impedansi Antara Saluran Antena." },
  { "en": "Berapa Nilai VSWR Yang Dianggap Ideal?", "id": "Nilai Ideal Adalah 1:1, Tanpa Pantulan." },
  { "en": "Apa Itu Komunikasi Serat Optik (Fiber Optic)?", "id": "Transmisi Informasi Sebagai Pulsa Cahaya Laser." },
  { "en": "Sebutkan Tiga Bagian Utama Kabel Serat Optik?", "id": "Core (Inti), Cladding (Selubung), dan Jacket." },
  { "en": "Bagaimana Cahaya Merambat Di Dalam Serat Optik?", "id": "Melalui Prinsip Total Internal Reflection (Pemantulan)." },
  { "en": "Sebutkan Dua Jenis Utama Kabel Serat Optik?", "id": "Single-Mode Fiber (SMF) Dan Multi-Mode Fiber." },
  { "en": "Apa Perbedaan Utama Antara SMF Dan MMF?", "id": "Diameter Core SMF Jauh Lebih Kecil." },
  { "en": "Manakah Yang Lebih Baik Untuk Jarak Jauh?", "id": "Single-Mode Fiber Lebih Baik Jarak Jauh." },
  { "en": "Apa Itu Dispersi Dalam Serat Optik?", "id": "Penyebaran Pulsa Cahaya Saat Merambat Jauh." },
  { "en": "Apa Dampak Dari Dispersi Pada Sinyal?", "id": "Membatasi Bandwidth Dan Jarak Transmisi Maksimal." },
  { "en": "Apa Kepanjangan Dari SONET Dalam Jaringan Optik?", "id": "SONET Adalah Synchronous Optical Networking." },
  { "en": "Apa Kepanjangan Dari SDH Dalam Jaringan Optik?", "id": "SDH Adalah Synchronous Digital Hierarchy." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Mengirim Beberapa Sinyal Cahaya Berbeda Warna." },
  { "en": "Apa Keuntungan Utama Dari Teknologi WDM?", "id": "Meningkatkan Kapasitas Serat Optik Secara Drastis." },
  { "en": "Apa Itu Sinyal Analog Secara Definisi?", "id": "Sinyal Kontinu Yang Mewakili Besaran Fisik." },
  { "en": "Apa Itu Sinyal Digital Secara Definisi?", "id": "Sinyal Diskret Yang Diwakili Oleh Angka." },
  { "en": "Apa Itu Amplitudo Dari Sebuah Sinyal?", "id": "Besaran Maksimum Sinyal Diukur Dari Nol." },
  { "en": "Apa Itu Frekuensi Dari Sebuah Sinyal?", "id": "Jumlah Siklus Gelombang Lengkap Per Detik." },
  { "en": "Apa Itu Fasa Dari Sebuah Sinyal?", "id": "Posisi Relatif Sinyal Pada Suatu Waktu." },
  { "en": "Apa Itu Decibel (dB) Dalam Ilmu Telekomunikasi?", "id": "Satuan Logaritmik Untuk Mengukur Rasio Daya." },
  { "en": "Apa Kepanjangan Dari SNR Dalam Kualitas Sinyal?", "id": "SNR Adalah Signal-to-Noise Ratio." },
  { "en": "Apa Arti Dari Nilai SNR Yang Tinggi?", "id": "Sinyal Jauh Lebih Kuat Daripada Noise." },
  { "en": "Apa Itu Fourier Transform Dalam Analisis Sinyal?", "id": "Memecah Sinyal Ke Dalam Komponen Frekuensinya." },
  { "en": "Domain Apa Yang Dihasilkan Transformasi Fourier?", "id": "Dari Domain Waktu Ke Domain Frekuensi." },
  { "en": "Apa Itu Filter Dalam Pemrosesan Sinyal?", "id": "Sirkuit Yang Melewatkan Frekuensi Tertentu Saja." },
  { "en": "Sebutkan Jenis-Jenis Dasar Filter Elektronik?", "id": "Low-Pass, High-Pass, Band-Pass, Band-Stop Filter." },
  { "en": "Apa Fungsi Dari Low-Pass Filter (LPF)?", "id": "Melewatkan Frekuensi Rendah, Memblokir Frekuensi Tinggi." },
  { "en": "Apa Fungsi Dari High-Pass Filter (HPF)?", "id": "Melewatkan Frekuensi Tinggi, Memblokir Frekuensi Rendah." },
  { "en": "Apa Itu Band-Pass Filter (BPF)?", "id": "Melewatkan Rentang Frekuensi Tertentu Saja." },
  { "en": "Apa Itu Band-Stop Filter Atau Notch Filter?", "id": "Memblokir Rentang Frekuensi Tertentu Yang Dipilih." },
  { "en": "Apa Itu Bandwidth Efektif Dari Suatu Antena?", "id": "Rentang Frekuensi Dimana Antena Bekerja Efektif." },
  { "en": "Apa Itu Beamwidth Dari Sebuah Antena Direktif?", "id": "Sudut Pancaran Utama Dari Lobe Antena." },
  { "en": "Apa Itu Side Lobe Pada Pola Radiasi?", "id": "Pancaran Radiasi Kecil Ke Arah Samping." },
  { "en": "Apa Modulasi Digital Yang Mengubah Amplitudo Carrier?", "id": "ASK (Amplitude Shift Keying) Modulasi Digital." },
  { "en": "Bagaimana Cara Kerja Modulasi ASK (Amplitude Shift Keying)?", "id": "Merepresentasikan Bit Dengan Level Amplitudo Berbeda." },
  { "en": "Apa Kerentanan Utama Dari Teknik Modulasi ASK?", "id": "Sangat Rentan Terhadap Gangguan Noise Amplitudo." },
  { "en": "Apa Modulasi Digital Yang Mengubah Frekuensi Carrier?", "id": "FSK (Frequency Shift Keying) Modulasi Digital." },
  { "en": "Bagaimana Cara Kerja Modulasi FSK (Frequency Shift Keying)?", "id": "Merepresentasikan Bit Dengan Frekuensi Sinyal Berbeda." },
  { "en": "Apa Modulasi Digital Yang Mengubah Fasa Carrier?", "id": "PSK (Phase Shift Keying) Modulasi Digital." },
  { "en": "Bagaimana Cara Kerja Modulasi BPSK (Binary PSK)?", "id": "Menggunakan Dua Fasa Untuk Representasi Dua Bit." },
  { "en": "Berapa Pergeseran Fasa Pada Modulasi BPSK?", "id": "Pergeseran Fasa Sebesar 180 Derajat Celcius." },
  { "en": "Apa Kepanjangan Dari Modulasi Digital QPSK?", "id": "QPSK Adalah Quadrature Phase Shift Keying." },
  { "en": "Berapa Banyak Titik Sinyal Pada Konstelasi QPSK?", "id": "Memiliki Empat Titik Sinyal Atau Simbol." },
  { "en": "Berapa Bit Yang Diwakili Setiap Simbol QPSK?", "id": "Setiap Simbol Dapat Mewakili Dua Buah Bit." },
  { "en": "Apa Keuntungan QPSK Dibandingkan Dengan Modulasi BPSK?", "id": "Efisiensi Spektral Dua Kali Lebih Baik." },
  { "en": "Apa Kepanjangan Dari Modulasi Digital QAM?", "id": "QAM Adalah Quadrature Amplitude Modulation." },
  { "en": "Parameter Sinyal Apa Yang Diubah Oleh QAM?", "id": "Mengubah Amplitudo Dan Juga Fasa Sekaligus." },
  { "en": "Apa Itu Diagram Konstelasi Dalam Modulasi Digital?", "id": "Representasi Grafis Titik Sinyal Pada Bidang." },
  { "en": "Apa Yang Direpresentasikan Sumbu Horizontal Dan Vertikal?", "id": "Komponen In-Phase (I) Dan Quadrature (Q)." },
  { "en": "Apa Arti Jarak Antar Titik Konstelasi?", "id": "Semakin Jauh Jaraknya, Semakin Tahan Noise." },
  { "en": "Teknik Modulasi Mana Yang Digunakan Wi-Fi Modern?", "id": "Menggunakan Varian QAM (Contohnya 256-QAM)." },
  { "en": "Apa Itu Bit Error Rate (BER) Telekomunikasi?", "id": "Rasio Jumlah Bit Error Terhadap Total." },
  { "en": "Apa Arti Nilai BER Yang Semakin Rendah?", "id": "Kualitas Transmisi Semakin Baik, Error Sedikit." },
  { "en": "Apa Hubungan Modulasi Orde Tinggi Dengan BER?", "id": "Orde Tinggi Lebih Rentan Error (BER Tinggi)." },
  { "en": "Apa Hubungan Modulasi Orde Tinggi Dengan Kecepatan?", "id": "Orde Tinggi Menawarkan Kecepatan Data Lebih." },
  { "en": "Apa Itu Differential PSK (DPSK) Modulasi Digital?", "id": "Fasa Direferensikan Terhadap Fasa Simbol Sebelumnya." },
  { "en": "Apa Itu Entropi Dalam Teori Informasi Shannon?", "id": "Ukuran Ketidakpastian Atau Informasi Rata-Rata." },
  { "en": "Siapa Bapak Teori Informasi Dunia Modern?", "id": "Claude Shannon, Seorang Matematikawan Dan Insinyur." },
  { "en": "Apa Itu Teorema Kapasitas Kanal Shannon-Hartley?", "id": "Menentukan Laju Data Maksimum Bebas Error." },
  { "en": "Faktor Apa Yang Membatasi Kapasitas Kanal?", "id": "Bandwidth Kanal Dan Juga Signal-to-Noise Ratio." },
  { "en": "Apa Itu Source Coding (Pengkodean Sumber)?", "id": "Proses Untuk Menghilangkan Redundansi Dari Data." },
  { "en": "Apa Tujuan Utama Dari Source Coding?", "id": "Untuk Kompresi Data Agar Lebih Efisien." },
  { "en": "Sebutkan Contoh Algoritma Source Coding Populer?", "id": "Huffman Coding Dan Lempel-Ziv Adalah Contoh." },
  { "en": "Apa Itu Redundansi Dalam Konteks Data?", "id": "Informasi Berlebih Yang Sebenarnya Tidak Diperlukan." },
  { "en": "Apa Satuan Kapasitas Kanal Menurut Teori Shannon?", "id": "Kapasitas Kanal Diukur Dalam Bit Perdetik." },
  { "en": "Apa Orbit Satelit GEO (Geostationary Equatorial Orbit)?", "id": "Orbit Di Atas Ekuator, Periode 24 Jam." },
  { "en": "Bagaimana Posisi Satelit GEO Dari Pengamat Bumi?", "id": "Tampak Diam Di Satu Titik Langit." },
  { "en": "Berapa Ketinggian Rata-Rata Orbit Satelit GEO?", "id": "Sekitar 35,786 Kilometer Di Atas Bumi." },
  { "en": "Apa Keuntungan Utama Satelit Berjenis GEO?", "id": "Cakupan Sangat Luas, Antena Bumi Tetap." },
  { "en": "Apa Kerugian Utama Dari Satelit GEO?", "id": "Latensi Atau Delay Propagasi Sangat Tinggi." },
  { "en": "Apa Orbit Satelit LEO (Low Earth Orbit)?", "id": "Orbit Rendah Dengan Ketinggian Di Bawah." },
  { "en": "Berapa Ketinggian Rata-Rata Orbit Satelit LEO?", "id": "Antara 160 Hingga 2,000 Kilometer Saja." },
  { "en": "Apa Keuntungan Utama Dari Satelit LEO?", "id": "Latensi Sangat Rendah Dibandingkan Satelit GEO." },
  { "en": "Apa Kerugian Utama Dari Satelit LEO?", "id": "Cakupan Sempit, Butuh Banyak Satelit (Konstelasi)." },
  { "en": "Apa Orbit Satelit MEO (Medium Earth Orbit)?", "id": "Orbit Antara LEO Dan Juga Satelit GEO." },
  { "en": "Sistem Apa Yang Umumnya Menggunakan Satelit MEO?", "id": "Sistem Navigasi Seperti GPS (Global Positioning System)." },
  { "en": "Apa Itu Transponder Pada Sebuah Satelit Komunikasi?", "id": "Perangkat Yang Menerima, Menguatkan, Memancarkan Sinyal." },
  { "en": "Apa Itu Uplink Dalam Komunikasi Satelit?", "id": "Sinyal Dikirim Dari Stasiun Bumi Ke." },
  { "en": "Apa Itu Downlink Dalam Komunikasi Satelit?", "id": "Sinyal Dikirim Dari Satelit Ke Stasiun." },
  { "en": "Apa Itu Stasiun Bumi (Earth Station)?", "id": "Fasilitas Di Bumi Untuk Komunikasi Satelit." },
  { "en": "Apa Itu Footprint Dari Sebuah Satelit?", "id": "Area Di Permukaan Bumi Yang Dicakup." },
  { "en": "Apa Itu Atenuasi Hujan (Rain Fade)?", "id": "Pelemahan Sinyal Satelit Akibat Air Hujan." },
  { "en": "Frekuensi Mana Yang Rentan Atenuasi Hujan?", "id": "Frekuensi Lebih Tinggi, Seperti Ka-Band, Ku-Band." },
  { "en": "Apa Kepanjangan Dari VSAT Jaringan Satelit?", "id": "VSAT Adalah Very Small Aperture Terminal." },
  { "en": "Apa Fungsi Dari Jaringan Komunikasi VSAT?", "id": "Untuk Komunikasi Data, Internet, Suara Privat." },
  { "en": "Apa Perbedaan Circuit Switching Dan Packet Switching?", "id": "Circuit Mendedikasikan Jalur, Packet Membagi Data." },
  { "en": "Jaringan Apa Yang Menggunakan Metode Circuit Switching?", "id": "Jaringan Telepon Tradisional Atau PSTN." },
  { "en": "Apa Keuntungan Dari Metode Circuit Switching?", "id": "Kualitas Layanan Terjamin, Delay Tetap Rendah." },
  { "en": "Jaringan Apa Yang Menggunakan Metode Packet Switching?", "id": "Jaringan Internet Modern (Protokol TCP/IP)." },
  { "en": "Apa Keuntungan Dari Metode Packet Switching?", "id": "Penggunaan Bandwidth Sangat Efisien Dan Fleksibel." },
  { "en": "Apa Itu Paket (Packet) Dalam Jaringan?", "id": "Potongan Kecil Data Yang Dikirim Terpisah." },
  { "en": "Informasi Apa Yang Ada Di Header Paket?", "id": "Alamat Sumber, Tujuan, Dan Informasi Kontrol." },
  { "en": "Apa Kepanjangan Dari MPLS Dalam Jaringan?", "id": "MPLS Adalah Multiprotocol Label Switching." },
  { "en": "Apa Fungsi Utama Dari Teknologi MPLS?", "id": "Mempercepat Pengiriman Paket Berdasarkan Label Pendek." },
  { "en": "Apa Kepanjangan Dari VoIP Teknologi Suara?", "id": "VoIP Adalah Voice over Internet Protocol." },
  { "en": "Bagaimana Cara Kerja Teknologi Canggih VoIP?", "id": "Mengirimkan Suara Dalam Bentuk Paket Data." },
  { "en": "Apa Keuntungan Utama Menggunakan Teknologi VoIP?", "id": "Biaya Jauh Lebih Murah Daripada Telepon." },
  { "en": "Apa Itu QoS (Quality of Service)?", "id": "Kemampuan Jaringan Memberikan Layanan Prioritas Tertentu." },
  { "en": "Mengapa QoS Penting Untuk Aplikasi VoIP?", "id": "Untuk Memastikan Kualitas Suara Tetap Jernih." },
  { "en": "Apa Itu Kodek (Codec) Dalam Aplikasi VoIP?", "id": "Mengkompres Dan Mendekompres Data Suara Digital." },
  { "en": "Apa Kepanjangan Dari CDMA Teknologi Seluler?", "id": "CDMA Adalah Code Division Multiple Access." },
  { "en": "Bagaimana Prinsip Kerja Dari Teknologi CDMA?", "id": "Pengguna Berbagi Frekuensi, Dibedakan Dengan Kode." },
  { "en": "Apa Kepanjangan Dari TDMA Teknologi Seluler?", "id": "TDMA Adalah Time Division Multiple Access." },
  { "en": "Bagaimana Prinsip Kerja Dari Teknologi TDMA?", "id": "Pengguna Berbagi Frekuensi Yang Sama, Slot." },
  { "en": "Apa Kepanjangan Dari FDMA Teknologi Seluler?", "id": "FDMA Adalah Frequency Division Multiple Access." },
  { "en": "Bagaimana Prinsip Kerja Dari Teknologi FDMA?", "id": "Setiap Pengguna Mendapatkan Alokasi Kanal Frekuensi." },
  { "en": "Apa Itu Orthogonality Dalam Teknik Multiple Access?", "id": "Sinyal Pengguna Tidak Saling Menginterferensi Satu." },
  { "en": "Apa Kepanjangan Dari OFDM Modulasi Digital?", "id": "OFDM Adalah Orthogonal Frequency-Division Multiplexing." },
  { "en": "Di Mana Teknologi OFDM Banyak Digunakan?", "id": "Pada Jaringan Wi-Fi, LTE, Dan 5G." },
  { "en": "Apa Keuntungan Utama Dari Teknologi OFDM?", "id": "Sangat Tangguh Terhadap Masalah Multipath Fading." },
  { "en": "Apa Itu Multipath Fading Komunikasi Nirkabel?", "id": "Sinyal Tiba Di Penerima Melalui Lintasan." },
  { "en": "Apa Efek Dari Multipath Fading Tersebut?", "id": "Dapat Menyebabkan Interferensi Konstruktif Atau Destruktif." },
  { "en": "Apa Itu Spreading Spectrum Dalam Komunikasi?", "id": "Menyebarkan Sinyal Pada Bandwidth Yang Lebar." },
  { "en": "Apa Tujuan Utama Dari Spreading Spectrum?", "id": "Untuk Keamanan Dan Ketahanan Terhadap Interferensi." },
  { "en": "Sebutkan Dua Jenis Teknik Spreading Spectrum?", "id": "Direct Sequence (DS) Dan Frequency Hopping." },
  { "en": "Bagaimana Cara Kerja Frequency Hopping (FHSS)?", "id": "Frekuensi Carrier Berpindah-Pindah Secara Cepat." },
  { "en": "Bagaimana Cara Kerja Direct Sequence (DSSS)?", "id": "Sinyal Data Dikalikan Dengan Kode Pseudo-Noise." },
  { "en": "Apa Itu Kode Pseudo-Noise (PN Code)?", "id": "Urutan Biner Yang Tampak Acak Tapi." },
  { "en": "Apa Itu GPRS (General Packet Radio Service)?", "id": "Teknologi Peningkatan Jaringan GSM Untuk Data." },
  { "en": "Apa Itu EDGE (Enhanced Data rates for GSM)?", "id": "Peningkatan Lebih Lanjut Dari Teknologi GPRS." },
  { "en": "Apa Istilah Lain Untuk Jaringan EDGE?", "id": "Dikenal Juga Dengan Sebutan Jaringan Seluler 2.75G." },
  { "en": "Apa Itu MIMO (Multiple-Input Multiple-Output) Nirkabel?", "id": "Menggunakan Banyak Antena Pemancar Dan Penerima." },
  { "en": "Apa Keuntungan Utama Menggunakan Teknologi MIMO?", "id": "Meningkatkan Kecepatan Data Dan Keandalan Sinyal." },
  { "en": "Apa Itu Beamforming Pada Antena Array?", "id": "Memfokuskan Sinyal Nirkabel Ke Arah Pengguna." },
  { "en": "Apa Itu Sel (Cell) Dalam Jaringan Seluler?", "id": "Area geografis yang dilayani satu BTS." },
  { "en": "Apa Itu Frekuensi Reuse Dalam Jaringan Seluler?", "id": "Penggunaan Kembali Frekuensi Yang Sama Disel." },
  { "en": "Mengapa Perlu Dilakukan Frekuensi Reuse Planning?", "id": "Untuk Menghindari Co-Channel Interference Antar Sel." },
  { "en": "Apa Itu Cluster Dalam Perencanaan Jaringan Seluler?", "id": "Sekelompok Sel Dimana Setiap Frekuensi Unik." },
  { "en": "Apa Itu Kabel Twisted Pair Dalam Jaringan?", "id": "Sepasang Konduktor Tembaga Yang Saling Dililitkan." },
  { "en": "Mengapa Kabel Tembaga Di Dalamnya Saling Dililitkan?", "id": "Untuk Mengurangi Interferensi Elektromagnetik Atau Crosstalk." },
  { "en": "Apa Kepanjangan Dari UTP Kategori Kabel Jaringan?", "id": "UTP Adalah Unshielded Twisted Pair." },
  { "en": "Apa Ciri Khas Utama Dari Kabel UTP?", "id": "Tidak Memiliki Lapisan Pelindung Tambahan (Shield)." },
  { "en": "Apa Kepanjangan Dari STP Kategori Kabel Jaringan?", "id": "STP Adalah Shielded Twisted Pair." },
  { "en": "Apa Keuntungan Kabel STP Dibandingkan Kabel UTP?", "id": "Lebih Tahan Terhadap Interferensi Frekuensi Tinggi." },
  { "en": "Sebutkan Bagian Utama Dari Kabel Koaksial?", "id": "Konduktor Pusat, Dielektrik, Shield, Dan Jaket." },
  { "en": "Di Mana Kabel Koaksial Umumnya Digunakan?", "id": "Untuk Antena Televisi Dan Jaringan Kabel." },
  { "en": "Apa Itu Crosstalk Atau Saluran Silang?", "id": "Gangguan Sinyal Antara Pasangan Kabel Berdekatan." },
  { "en": "Apa Itu Bumbung Gelombang (Waveguide) Media Transmisi?", "id": "Pipa Logam Berongga Untuk Memandu Gelombang." },
  { "en": "Frekuensi Apa Yang Dilewatkan Oleh Bumbung Gelombang?", "id": "Frekuensi Sangat Tinggi, Seperti Gelombang Mikro." },
  { "en": "Apa Kerugian Transmisi Melalui Udara Bebas?", "id": "Rentan Fading, Interferensi, Dan Masalah Keamanan." },
  { "en": "Media Transmisi Apa Dengan Keamanan Paling Tinggi?", "id": "Serat Optik Karena Sulit Untuk Disadap." },
  { "en": "Media Transmisi Apa Dengan Bandwidth Paling Lebar?", "id": "Serat Optik Menawarkan Kapasitas Bandwidth Terbesar." },
  { "en": "Apa Itu Skin Effect Pada Konduktor Listrik?", "id": "Arus AC Cenderung Mengalir Di Permukaan." },
  { "en": "Apa Kepanjangan Dari DSP Dalam Teknik Elektro?", "id": "DSP Adalah Digital Signal Processing." },
  { "en": "Apa Tujuan Utama Dari Digital Signal Processing?", "id": "Memanipulasi Sinyal Digital Untuk Tujuan Tertentu." },
  { "en": "Sebutkan Contoh Aplikasi DSP Sehari-Hari Kita?", "id": "Kompresi Audio MP3, Peredam Noise Headphone." },
  { "en": "Apa Itu Operasi Konvolusi Dalam Dunia DSP?", "id": "Operasi Matematis Menggabungkan Dua Sinyal Bersama." },
  { "en": "Apa Kegunaan Konvolusi Dalam Pemrosesan Sinyal?", "id": "Untuk Menerapkan Efek Filter Pada Sinyal." },
  { "en": "Apa Kepanjangan Dari FFT Dalam Analisis Sinyal?", "id": "FFT Adalah Singkatan Fast Fourier Transform." },
  { "en": "Apa Fungsi Utama Dari Algoritma FFT?", "id": "Menghitung Transformasi Fourier Secara Sangat Efisien." },
  { "en": "Apa Itu FIR (Finite Impulse Response) Filter?", "id": "Filter Digital Tanpa Menggunakan Umpan Balik." },
  { "en": "Apa Karakteristik Utama Dari Filter FIR?", "id": "Selalu Stabil, Memiliki Fasa Yang Linear." },
  { "en": "Apa Itu IIR (Infinite Impulse Response) Filter?", "id": "Filter Digital Yang Menggunakan Umpan Balik." },
  { "en": "Apa Keuntungan Filter IIR Dibandingkan FIR?", "id": "Jauh Lebih Efisien Secara Komputasi (Cepat)." },
  { "en": "Apa Itu Aliasing Dalam Proses Sampling Sinyal?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Bagaimana Cara Mencegah Terjadinya Efek Aliasing?", "id": "Gunakan Filter Anti-Aliasing Sebelum Proses Sampling." },
  { "en": "Apa Fungsi Dari Windowing Dalam Analisis FFT?", "id": "Mengurangi Kebocoran Spektral (Spectral Leakage) Sinyal." },
  { "en": "Apa Itu Downsampling Dalam Pengolahan Sinyal Digital?", "id": "Proses Untuk Mengurangi Laju Sampling Sinyal." },
  { "en": "Apa Itu Upsampling Dalam Pengolahan Sinyal Digital?", "id": "Proses Untuk Meningkatkan Laju Sampling Sinyal." },
  { "en": "Apa Kepanjangan Dari ITU Badan Standardisasi Dunia?", "id": "ITU Adalah International Telecommunication Union." },
  { "en": "Apa Peran Utama Dari Badan Standardisasi ITU?", "id": "Mengatur Spektrum Radio Dan Telekomunikasi Global." },
  { "en": "Sektor Apa Di ITU Yang Mengatur Spektrum Radio?", "id": "Sektor Radiokomunikasi Atau Dikenal Dengan ITU-R." },
  { "en": "Sektor Apa Di ITU Standardisasi Telekomunikasi?", "id": "Sektor Standardisasi Telekomunikasi Atau ITU-T." },
  { "en": "Apa Kepanjangan Dari IEEE Badan Profesional Teknik?", "id": "IEEE Adalah Institute of Electrical and Electronics." },
  { "en": "Sebutkan Contoh Standar Populer Buatan IEEE?", "id": "Standar IEEE 802.11 Untuk Jaringan Wi-Fi." },
  { "en": "Apa Kepanjangan Dari IETF Dalam Dunia Internet?", "id": "IETF Adalah Internet Engineering Task Force." },
  { "en": "Apa Peran Utama Dari Badan Standardisasi IETF?", "id": "Mengembangkan Dan Mempromosikan Standar Jaringan Internet." },
  { "en": "Dokumen Apa Yang Diterbitkan Oleh IETF?", "id": "Request for Comments Atau Disingkat RFC." },
  { "en": "Apa Kepanjangan Dari 3GPP Dalam Telekomunikasi Seluler?", "id": "3GPP Adalah 3rd Generation Partnership Project." },
  { "en": "Apa Fokus Utama Dari Badan Standardisasi 3GPP?", "id": "Mengembangkan Spesifikasi Untuk Teknologi Jaringan Seluler." },
  { "en": "Apa Kepanjangan Dari ETSI Badan Standardisasi Eropa?", "id": "ETSI Adalah European Telecommunications Standards Institute." },
  { "en": "Mengapa Standardisasi Sangat Penting Dalam Telekomunikasi?", "id": "Memastikan Interoperabilitas Antar Perangkat Dan Vendor." },
  { "en": "Apa Itu Interoperabilitas Dalam Konteks Teknologi?", "id": "Kemampuan Sistem Berbeda Bekerja Sama Efektif." },
  { "en": "Apa Itu Standar De Jure Secara Definisi?", "id": "Standar Yang Disahkan Oleh Badan Resmi." },
  { "en": "Apa Itu Standar De Facto Secara Definisi?", "id": "Standar Yang Diterima Luas Karena Dominasi." },
  { "en": "Apa Kepanjangan Dari IoT Dalam Dunia Teknologi?", "id": "IoT Adalah Singkatan Internet of Things." },
  { "en": "Apa Konsep Dasar Dari Internet of Things?", "id": "Menghubungkan Objek Fisik Sehari-Hari Ke Internet." },
  { "en": "Sebutkan Contoh Penerapan IoT Di Sekitar?", "id": "Smart Home, Wearable Device, Mobil Terhubung." },
  { "en": "Apa Itu Komunikasi M2M (Machine-to-Machine)?", "id": "Komunikasi Langsung Antara Dua Mesin Cerdas." },
  { "en": "Apa Perbedaan M2M Dengan Teknologi IoT?", "id": "IoT Adalah Konsep Lebih Luas Dari M2M." },
  { "en": "Sebutkan Contoh Teknologi Komunikasi Nirkabel IoT?", "id": "LoRaWAN, NB-IoT, Dan Juga Sigfox." },
  { "en": "Apa Karakteristik Teknologi Komunikasi Untuk IoT?", "id": "Konsumsi Daya Rendah Dan Jangkauan Cukup Luas." },
  { "en": "Apa Kepanjangan Dari SDN Dalam Arsitektur Jaringan?", "id": "SDN Adalah Software-Defined Networking." },
  { "en": "Apa Ide Utama Dari Arsitektur SDN?", "id": "Memisahkan Control Plane Dari Data Plane." },
  { "en": "Apa Itu Control Plane Dalam Perangkat Jaringan?", "id": "Bagian Yang Membuat Keputusan Rute Lalu." },
  { "en": "Apa Itu Data Plane Dalam Perangkat Jaringan?", "id": "Bagian Yang Meneruskan Lalu Lintas Data." },
  { "en": "Apa Keuntungan Utama Dari Arsitektur Jaringan SDN?", "id": "Jaringan Menjadi Lebih Fleksibel Dan Terpusat." },
  { "en": "Apa Kepanjangan Dari NFV Teknologi Jaringan?", "id": "NFV Adalah Network Functions Virtualization." },
  { "en": "Apa Konsep Utama Dari Teknologi Canggih NFV?", "id": "Virtualisasi Fungsi Jaringan Seperti Firewall Router." },
  { "en": "Apa Keuntungan Utama Dari Penerapan Teknologi NFV?", "id": "Mengurangi Ketergantungan Pada Perangkat Keras Khusus." },
  { "en": "Apa Itu Jaringan Ad Hoc Nirkabel?", "id": "Jaringan Nirkabel Terdesentralisasi Tanpa Infrastruktur Tetap." },
  { "en": "Sebutkan Contoh Penerapan Jaringan Ad Hoc?", "id": "Komunikasi Darurat Bencana Atau Area Terpencil." },
  { "en": "Apa Kepanjangan Dari MANET Dalam Jaringan Nirkabel?", "id": "MANET Adalah Mobile Ad Hoc Network." },
  { "en": "Apa Itu Cognitive Radio (Radio Kognitif)?", "id": "Radio Pintar Yang Bisa Mendeteksi Spektrum." },
  { "en": "Apa Tujuan Utama Dari Teknologi Cognitive Radio?", "id": "Meningkatkan Efisiensi Penggunaan Spektrum Frekuensi Radio." },
  { "en": "Apa Itu Spektrum Frekuensi Yang Tidak Berlisensi?", "id": "Pita Frekuensi Bebas Digunakan Tanpa Izin." },
  { "en": "Berikan Contoh Pita Frekuensi Tidak Berlisensi?", "id": "Pita ISM (Industrial, Scientific, Medical) 2.4 GHz." },
  { "en": "Apa Itu Power Line Communication (PLC)?", "id": "Menggunakan Jaringan Listrik Untuk Transmisi Data." },
  { "en": "Apa Itu Visible Light Communication (VLC)?", "id": "Menggunakan Cahaya Tampak Untuk Transmisi Data." },
  { "en": "Apa Kepanjangan Dari Teknologi Li-Fi Modern?", "id": "Li-Fi Adalah Singkatan Light Fidelity." },
  { "en": "Apa Keunggulan Teknologi Li-Fi Dibanding Wi-Fi?", "id": "Kecepatan Sangat Tinggi Dan Jauh Aman." },
  { "en": "Apa Kerugian Utama Teknologi Komunikasi Li-Fi?", "id": "Tidak Dapat Menembus Dinding Atau Penghalang." },
  { "en": "Apa Itu Kuantum Komunikasi (Quantum Communication)?", "id": "Menggunakan Prinsip Mekanika Kuantum Untuk Komunikasi." },
  { "en": "Apa Janji Utama Teknologi Kuantum Komunikasi?", "id": "Tingkat Keamanan Komunikasi Yang Sangat Tinggi." },
  { "en": "Apa Itu QKD (Quantum Key Distribution)?", "id": "Metode Aman Berbagi Kunci Enkripsi Kuantum." },
  { "en": "Apa Itu Passive Optical Network (PON)?", "id": "Jaringan Serat Optik Titik Ke Multipoint." },
  { "en": "Di Mana Teknologi PON Umumnya Digunakan?", "id": "Untuk Layanan Fiber-to-the-Home Atau FTTH." },
  { "en": "Apa Kepanjangan Dari FTTH Dalam Akses Internet?", "id": "FTTH Adalah Fiber-to-the-Home." },
  { "en": "Apa Itu Optical Splitter Dalam Jaringan PON?", "id": "Membagi Sinyal Optik Dari Satu Serat." },
  { "en": "Apa Kepanjangan Dari OLT Dalam Jaringan PON?", "id": "OLT Adalah Optical Line Terminal." },
  { "en": "Di Mana Perangkat OLT Biasanya Ditempatkan?", "id": "Terletak Di Kantor Pusat Penyedia Layanan." },
  { "en": "Apa Kepanjangan Dari ONT Dalam Jaringan PON?", "id": "ONT Adalah Optical Network Terminal." },
  { "en": "Di Mana Perangkat ONT Biasanya Ditempatkan?", "id": "Terletak Di Sisi Atau Rumah Pelanggan." },
  { "en": "Apa Itu Free-Space Optical (FSO) Communication?", "id": "Komunikasi Optik Nirkabel Melalui Udara Bebas." },
  { "en": "Apa Keuntungan Dari Free-Space Optical Communication?", "id": "Instalasi Cepat Dan Bandwidth Sangat Tinggi." },
  { "en": "Apa Kelemahan Dari Free-Space Optical Communication?", "id": "Rentan Terhadap Kondisi Cuaca Seperti Kabut." },
  { "en": "Apa Itu Network Latency Atau Keterlambatan Jaringan?", "id": "Total Waktu Tunda Data Melintasi Jaringan." },
  { "en": "Sebutkan Komponen Utama Dari Network Latency?", "id": "Delay Propagasi, Transmisi, Antrian, Dan Pemrosesan." },
  { "en": "Apa Itu Delay Propagasi (Propagation Delay)?", "id": "Waktu Sinyal Merambat Dari Sumber Tujuan." },
  { "en": "Apa Itu Delay Transmisi (Transmission Delay)?", "id": "Waktu Mendorong Semua Bit Paket Ke." },
  { "en": "Apa Itu Delay Antrian (Queuing Delay)?", "id": "Waktu Paket Menunggu Di Antrian Router." },
  { "en": "Faktor Apa Yang Mempengaruhi Delay Propagasi?", "id": "Jarak Fisik Antara Pengirim Dan Penerima." },
  { "en": "Faktor Apa Yang Mempengaruhi Delay Transmisi?", "id": "Ukuran Paket Dan Juga Bandwidth Saluran." },
  { "en": "Apa Kepanjangan Dari OSI Model Referensi Jaringan?", "id": "OSI Adalah Open Systems Interconnection." },
  { "en": "Berapa Jumlah Lapisan Pada Model Referensi OSI?", "id": "Model OSI Terdiri Dari Tujuh Lapisan." },
  { "en": "Sebutkan Lapisan Pertama Atau Terbawah Model OSI?", "id": "Lapisan Pertama Adalah Lapisan Fisik (Physical)." },
  { "en": "Apa Fungsi Utama Dari Lapisan Fisik OSI?", "id": "Mengirimkan Bit Mentah Melalui Media Fisik." },
  { "en": "Sebutkan Lapisan Kedua Pada Model Referensi OSI?", "id": "Lapisan Kedua Adalah Lapisan Data Link." },
  { "en": "Apa Fungsi Utama Lapisan Data Link OSI?", "id": "Mengatur Akses Media Dan Deteksi Error." },
  { "en": "Apa Itu MAC (Media Access Control) Address?", "id": "Alamat Fisik Unik Kartu Antarmuka Jaringan." },
  { "en": "Pada Lapisan OSI Mana MAC Address Beroperasi?", "id": "MAC Address Beroperasi Pada Lapisan Data Link." },
  { "en": "Sebutkan Lapisan Ketiga Pada Model Referensi OSI?", "id": "Lapisan Ketiga Adalah Lapisan Jaringan (Network)." },
  { "en": "Apa Fungsi Utama Lapisan Jaringan Model OSI?", "id": "Menentukan Rute Terbaik Untuk Pengiriman Paket." },
  { "en": "Pada Lapisan OSI Mana Alamat IP Beroperasi?", "id": "Alamat IP Beroperasi Pada Lapisan Jaringan." },
  { "en": "Sebutkan Lapisan Keempat Pada Model Referensi OSI?", "id": "Lapisan Keempat Adalah Lapisan Transport (Transport)." },
  { "en": "Apa Fungsi Utama Lapisan Transport Model OSI?", "id": "Menyediakan Koneksi Antar Ujung Yang Andal." },
  { "en": "Sebutkan Dua Protokol Utama Di Lapisan Transport?", "id": "TCP (Transmission Control Protocol) Dan UDP (User Datagram Protocol)." },
  { "en": "Apa Perbedaan Utama Antara Protokol TCP UDP?", "id": "TCP Andal Berorientasi Koneksi, UDP Tidak." },
  { "en": "Sebutkan Lapisan Kelima Pada Model Referensi OSI?", "id": "Lapisan Kelima Adalah Lapisan Sesi (Session)." },
  { "en": "Apa Fungsi Utama Lapisan Sesi Model OSI?", "id": "Membangun, Mengelola, Dan Mengakhiri Sesi Komunikasi." },
  { "en": "Sebutkan Lapisan Keenam Pada Model Referensi OSI?", "id": "Lapisan Keenam Adalah Lapisan Presentasi (Presentation)." },
  { "en": "Apa Fungsi Utama Lapisan Presentasi Model OSI?", "id": "Translasi, Enkripsi, Dan Kompresi Data Digital." },
  { "en": "Sebutkan Lapisan Ketujuh Atau Puncak Model OSI?", "id": "Lapisan Ketujuh Adalah Lapisan Aplikasi (Application)." },
  { "en": "Apa Fungsi Utama Lapisan Aplikasi Model OSI?", "id": "Menyediakan Layanan Jaringan Langsung Ke Pengguna." },
  { "en": "Berapa Lapisan Yang Ada Di Model TCP/IP?", "id": "Model TCP/IP Umumnya Memiliki Empat Lapisan." },
  { "en": "Lapisan OSI Mana Yang Digabung Di TCP/IP?", "id": "Aplikasi, Presentasi, Sesi Menjadi Satu Lapisan." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) Dalam Jaringan Komputer?", "id": "Proses Menambahkan Header Pada Data Tiap Lapisan." },
  { "en": "Apa Istilah Untuk Unit Data Di Lapisan Transport?", "id": "Unit Data Di Lapisan Ini Segmen." },
  { "en": "Apa Istilah Untuk Unit Data Di Lapisan Jaringan?", "id": "Unit Data Di Lapisan Ini Paket." },
  { "en": "Apa Istilah Untuk Unit Data Di Lapisan Data Link?", "id": "Unit Data Di Lapisan Ini Frame." },
  { "en": "Apa Itu Kriptografi Dalam Keamanan Informasi Jaringan?", "id": "Ilmu Menyandikan Dan Memecahkan Kode Pesan." },
  { "en": "Apa Itu Enkripsi Dalam Konteks Keamanan Data?", "id": "Proses Mengubah Plaintext Menjadi Ciphertext Yang Aman." },
  { "en": "Apa Itu Dekripsi Dalam Konteks Keamanan Data?", "id": "Proses Mengembalikan Ciphertext Menjadi Plaintext Kembali." },
  { "en": "Apa Itu Plaintext Dalam Proses Enkripsi?", "id": "Pesan Asli Yang Dapat Dibaca Manusia." },
  { "en": "Apa Itu Ciphertext Dalam Proses Enkripsi?", "id": "Pesan Tersandi Yang Tidak Dapat Dibaca." },
  { "en": "Apa Itu Enkripsi Simetris Atau Kunci Privat?", "id": "Menggunakan Kunci Yang Sama Untuk Enkripsi Dekripsi." },
  { "en": "Sebutkan Contoh Algoritma Enkripsi Simetris Terkenal?", "id": "AES (Advanced Encryption Standard) Adalah Contoh." },
  { "en": "Apa Itu Enkripsi Asimetris Atau Kunci Publik?", "id": "Menggunakan Sepasang Kunci, Publik Dan Privat." },
  { "en": "Bagaimana Cara Kerja Enkripsi Kunci Asimetris?", "id": "Kunci Publik Enkripsi, Kunci Privat Dekripsi." },
  { "en": "Sebutkan Contoh Algoritma Enkripsi Asimetris Terkenal?", "id": "RSA (Rivest–Shamir–Adleman) Adalah Contoh Populer." },
  { "en": "Apa Itu Otentikasi (Authentication) Dalam Keamanan?", "id": "Proses Verifikasi Identitas Pengguna Atau Perangkat." },
  { "en": "Apa Itu Integritas Data (Data Integrity)?", "id": "Memastikan Data Tidak Diubah Selama Transmisi." },
  { "en": "Apa Itu Non-repudiation Dalam Keamanan Digital?", "id": "Pencegahan Penyangkalan Atas Tindakan Yang Dilakukan." },
  { "en": "Apa Fungsi Dari Digital Signature (Tanda Tangan)?", "id": "Menjamin Otentikasi, Integritas, Dan Non-repudiasi." },
  { "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Pihak Ketiga Menyadap Komunikasi Dua Pihak." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membanjiri Server Hingga Tidak Dapat Melayani." },
  { "en": "Apa Kepanjangan Dari VPN Dalam Keamanan Jaringan?", "id": "VPN Adalah Singkatan Virtual Private Network." },
  { "en": "Apa Fungsi Utama Dari Virtual Private Network?", "id": "Menciptakan Koneksi Aman Di Jaringan Publik." },
  { "en": "Apa Itu Komunikasi Gelombang Mikro (Microwave)?", "id": "Komunikasi Menggunakan Gelombang Radio Frekuensi Tinggi." },
  { "en": "Apa Karakteristik Propagasi Gelombang Mikro Terestrial?", "id": "Membutuhkan Jalur Line-of-Sight (LOS) Bebas." },
  { "en": "Mengapa Komunikasi Gelombang Mikro Memerlukan Line-of-Sight?", "id": "Karena Sinyal Tidak Dapat Menembus Penghalang." },
  { "en": "Apa Kegunaan Utama Tautan Gelombang Mikro?", "id": "Untuk Komunikasi Jarak Jauh Point-to-Point." },
  { "en": "Apa Itu Zona Fresnel (Fresnel Zone)?", "id": "Area Elipsoid Antara Antena Pemancar Penerima." },
  { "en": "Mengapa Zona Fresnel Harus Bebas Dari Hambatan?", "id": "Hambatan Dapat Menyebabkan Refleksi Dan Atenuasi." },
  { "en": "Berapa Persen Zona Fresnel Pertama Harus Bersih?", "id": "Minimal 60 Persen Harus Bebas Hambatan." },
  { "en": "Apa Itu Repeater Dalam Tautan Gelombang Mikro?", "id": "Stasiun Untuk Menerima Dan Memancarkan Ulang." },
  { "en": "Kapan Sebuah Repeater Gelombang Mikro Diperlukan?", "id": "Ketika Jarak Terlalu Jauh Atau Terhalang." },
  { "en": "Sebutkan Keuntungan Komunikasi Gelombang Mikro Darat?", "id": "Bandwidth Tinggi Dibandingkan Dengan Frekuensi Rendah." },
  { "en": "Sebutkan Kerugian Komunikasi Gelombang Mikro Darat?", "id": "Membutuhkan Tower Tinggi Dan Rentan Cuaca." },
  { "en": "Apa Kepanjangan Dari RADAR Dalam Navigasi?", "id": "RADAR Adalah Radio Detection and Ranging." },
  { "en": "Bagaimana Prinsip Kerja Dasar Dari Sistem Radar?", "id": "Memancarkan Gelombang Radio, Menganalisis Pantulannya Kembali." },
  { "en": "Informasi Apa Yang Diperoleh Dari Sistem Radar?", "id": "Jarak, Kecepatan, Dan Arah Objek Target." },
  { "en": "Bagaimana Radar Menentukan Jarak Suatu Objek?", "id": "Mengukur Waktu Tunda Pulsa Kembali Diterima." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Perubahan Frekuensi Akibat Gerakan Sumber Penerima." },
  { "en": "Bagaimana Radar Menggunakan Efek Doppler Tersebut?", "id": "Untuk Mengukur Kecepatan Radial Suatu Objek." },
  { "en": "Apa Itu Radar Pulsa (Pulsed Radar)?", "id": "Radar Yang Memancarkan Sinyal Dalam Pulsa." },
  { "en": "Apa Itu Radar Gelombang Kontinu (Continuous Wave)?", "id": "Radar Yang Memancarkan Sinyal Secara Terus-menerus." },
  { "en": "Sebutkan Aplikasi Umum Dari Teknologi Radar?", "id": "Navigasi Pesawat, Prakiraan Cuaca, Kontrol Lalu." },
  { "en": "Apa Itu Penampang Lintang Radar (Radar Cross-Section)?", "id": "Ukuran Seberapa Mudah Objek Didateksi Radar." },
  { "en": "Apa Itu Clutter Dalam Istilah Sistem Radar?", "id": "Gema Atau Pantulan Yang Tidak Diinginkan." },
  { "en": "Apa Kepanjangan Dari SNMP Manajemen Jaringan?", "id": "SNMP Adalah Simple Network Management Protocol." },
  { "en": "Apa Fungsi Utama Dari Protokol Canggih SNMP?", "id": "Untuk Memantau Dan Mengelola Perangkat Jaringan." },
  { "en": "Sebutkan Tiga Komponen Utama Dalam Arsitektur SNMP?", "id": "Manager, Agent, Dan Juga Database MIB." },
  { "en": "Apa Itu MIB (Management Information Base)?", "id": "Database Informasi Yang Dikelola Oleh Agent." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Varian Bluetooth Untuk Konsumsi Daya Rendah." },
  { "en": "Dimana Teknologi BLE Umumnya Banyak Digunakan?", "id": "Perangkat IoT Dan Wearable Yang Hemat Baterai." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Pendek Sekali." },
  { "en": "Berapa Jangkauan Maksimal Teknologi Komunikasi NFC?", "id": "Hanya Beberapa Sentimeter Saja Jarak Maksimalnya." },
  { "en": "Sebutkan Aplikasi Populer Teknologi Komunikasi NFC?", "id": "Pembayaran Nirkontak Dan Pemasangan Perangkat Cepat." },
  { "en": "Apa Itu Zigbee Dalam Komunikasi Nirkabel?", "id": "Standar Nirkabel Daya Rendah Untuk Jaringan." },
  { "en": "Apa Jenis Topologi Jaringan Yang Didukung Zigbee?", "id": "Mendukung Topologi Star, Tree, Dan Mesh." },
  { "en": "Apa Itu Z-Wave Dalam Otomasi Rumah?", "id": "Protokol Komunikasi Nirkabel Untuk Smart Home." },
  { "en": "Apa Itu Buffer Dalam Konteks Telekomunikasi?", "id": "Area Penyimpanan Sementara Untuk Data Digital." },
  { "en": "Apa Itu Buffer Overflow Dalam Keamanan?", "id": "Data Melebihi Kapasitas Buffer, Menimpa Memori." },
  { "en": "Apa Itu Handshake Dalam Protokol Komunikasi?", "id": "Proses Negosiasi Parameter Sebelum Transfer Data." },
  { "en": "Apa Contoh Proses Handshake Yang Umum?", "id": "TCP Three-Way Handshake (SYN, SYN-ACK, ACK)." },
  { "en": "Apa Itu Flow Control Dalam Lapisan Transport?", "id": "Mekanisme Mengatur Laju Pengiriman Data Optimal." },
  { "en": "Apa Itu Congestion Control Dalam Jaringan Komputer?", "id": "Mekanisme Mencegah Kemacetan Lalu Lintas Jaringan." },
  { "en": "Apa Perbedaan Antara Flow Dan Congestion Control?", "id": "Flow Antar Dua Titik, Congestion Keseluruhan." },
  { "en": "Apa Itu Port Number Di Lapisan Transport?", "id": "Alamat Untuk Mengidentifikasi Aplikasi Di Host." },
  { "en": "Berapa Rentang Well-Known Ports Yang Ada?", "id": "Dari 0 Hingga 1023 Untuk Layanan." },
  { "en": "Port Berapa Yang Digunakan Protokol HTTP?", "id": "HTTP Umumnya Menggunakan Port Nomor 80." },
  { "en": "Port Berapa Yang Digunakan Protokol HTTPS?", "id": "HTTPS Umumnya Menggunakan Port Nomor 443." },
  { "en": "Apa Itu Socket Dalam Pemrograman Jaringan?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Kepanjangan Dari API Dalam Pengembangan Perangkat?", "id": "API Adalah Application Programming Interface." },
  { "en": "Apa Itu Jitter Buffer Dalam Aplikasi VoIP?", "id": "Menyimpan Sementara Paket Suara Untuk Atasi." },
  { "en": "Apa Itu Mean Opinion Score (MOS)?", "id": "Ukuran Kualitas Subjektif Suara Dalam VoIP." },
  { "en": "Berapa Skala Nilai Mean Opinion Score?", "id": "Skala Dari 1 (Buruk) Hingga 5." },
  { "en": "Apa Itu Forward Error Correction (FEC)?", "id": "Mengirim Data Redundan Untuk Memperbaiki Error." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Meminta Pengiriman Ulang Paket Yang Rusak." },
  { "en": "Apa Fungsi Osiloskop Dalam Pengukuran Elektronik?", "id": "Menampilkan Bentuk Gelombang Sinyal Terhadap Waktu." },
  { "en": "Sumbu Vertikal Pada Osiloskop Mewakili Besaran Apa?", "id": "Sumbu Vertikal Menunjukkan Amplitudo Atau Tegangan." },
  { "en": "Sumbu Horizontal Pada Osiloskop Mewakili Besaran Apa?", "id": "Sumbu Horizontal Menunjukkan Besaran Satuan Waktu." },
  { "en": "Apa Fungsi Dari Spectrum Analyzer (Penganalisis Spektrum)?", "id": "Menampilkan Kekuatan Sinyal Pada Domain Frekuensi." },
  { "en": "Apa Fungsi Dari Vector Network Analyzer (VNA)?", "id": "Mengukur Parameter S-parameter Perangkat Frekuensi Radio." },
  { "en": "Apa Itu S-parameters (Scattering Parameters) RF?", "id": "Parameter Yang Menggambarkan Perilaku Jaringan RF." },
  { "en": "Apa Satuan Daya Absolut Yang Umum Digunakan?", "id": "Satuan Watt Atau Miliwatt (mW) Umumnya." },
  { "en": "Apa Itu dBm (Decibel-milliwatts) Dalam Pengukuran Daya?", "id": "Rasio Daya Dalam Desibel Referensi 1mW." },
  { "en": "Berapa Nilai 0 dBm Dalam Satuan Miliwatt?", "id": "Setara Dengan Daya Sebesar 1 Miliwatt." },
  { "en": "Apa Itu Power Meter Dalam Pengukuran RF?", "id": "Alat Untuk Mengukur Daya Sinyal Radio." },
  { "en": "Apa Fungsi Dari Signal Generator (Pembangkit Sinyal)?", "id": "Menghasilkan Sinyal Uji Dengan Karakteristik Tertentu." },
  { "en": "Apa Itu BER Tester (Bit Error Rate)?", "id": "Alat Mengukur Tingkat Kesalahan Bit Transmisi." },
  { "en": "Apa Fungsi Time-Domain Reflectometer (TDR)?", "id": "Mendeteksi Lokasi Gangguan Pada Saluran Kabel." },
  { "en": "Bagaimana Cara Kerja Dari Sebuah TDR?", "id": "Mengirim Pulsa, Mengukur Waktu Pantulan Kembali." },
  { "en": "Apa Fungsi Optical Time-Domain Reflectometer (OTDR)?", "id": "Mendeteksi Lokasi Gangguan Pada Kabel Optik." },
  { "en": "Apa Perbedaan LED Dengan Laser Sumber Cahaya?", "id": "Laser Menghasilkan Cahaya Koheren, LED Tidak." },
  { "en": "Apa Itu Cahaya Koheren Yang Dihasilkan Laser?", "id": "Gelombang Cahaya Memiliki Fasa Yang Sama." },
  { "en": "Sumber Cahaya Apa Dipakai Jarak Jauh SMF?", "id": "Laser Dioda Dipakai Untuk Jarak Sangat Jauh." },
  { "en": "Sumber Cahaya Apa Dipakai Jarak Pendek MMF?", "id": "LED (Light Emitting Diode) Jarak Pendek." },
  { "en": "Apa Fungsi Detektor Optik Sistem Komunikasi?", "id": "Mengubah Sinyal Cahaya Menjadi Sinyal Listrik." },
  { "en": "Sebutkan Jenis Detektor Optik Yang Umum?", "id": "PIN Photodiode Dan APD (Avalanche Photodiode)." },
  { "en": "Apa Kelebihan APD Dibandingkan PIN Photodiode?", "id": "APD Memiliki Penguatan Internal, Lebih Sensitif." },
  { "en": "Apa Kepanjangan Dari EDFA Penguat Optik?", "id": "EDFA Adalah Erbium-Doped Fiber Amplifier." },
  { "en": "Apa Fungsi Utama Dari Amplifier EDFA?", "id": "Menguatkan Sinyal Optik Secara Langsung Terus." },
  { "en": "Bagaimana Cara Kerja Penguat Optik Raman?", "id": "Menggunakan Serat Transmisi Sebagai Media Penguatan." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Penyebaran Pulsa Akibat Kecepatan Warna Berbeda." },
  { "en": "Apa Itu Dispersi Modus Polarisasi (Polarization Mode)?", "id": "Penyebaran Pulsa Akibat Polarisasi Berbeda Merambat." },
  { "en": "Apa Itu Soliton Dalam Komunikasi Serat Optik?", "id": "Pulsa Optik Yang Menjaga Bentuknya Merambat." },
  { "en": "Apa Itu Attenuator Optik Dalam Sistem Optik?", "id": "Perangkat Untuk Melemahkan Daya Sinyal Optik." },
  { "en": "Apa Itu Konektor Dan Sambungan (Splice) Optik?", "id": "Menghubungkan Dua Ujung Serat Optik Bersama." },
  { "en": "Apa Kepanjangan MSC Jaringan Inti Seluler?", "id": "MSC Adalah Mobile Switching Center." },
  { "en": "Apa Fungsi Utama Dari Mobile Switching Center?", "id": "Melakukan Switching Panggilan Antar Pengguna Jaringan." },
  { "en": "Apa Kepanjangan HLR Jaringan Inti Seluler?", "id": "HLR Adalah Home Location Register." },
  { "en": "Data Apa Yang Disimpan Dalam Database HLR?", "id": "Informasi Permanen Pelanggan Seperti Nomor Telepon." },
  { "en": "Apa Kepanjangan VLR Jaringan Inti Seluler?", "id": "VLR Adalah Visitor Location Register." },
  { "en": "Data Apa Yang Disimpan Dalam Database VLR?", "id": "Informasi Temporer Pelanggan Yang Sedang Berkunjung." },
  { "en": "Apa Fungsi Dari AuC (Authentication Center)?", "id": "Menyimpan Data Rahasia Untuk Otentikasi Pelanggan." },
  { "en": "Apa Fungsi Dari EIR (Equipment Identity Register)?", "id": "Memvalidasi Perangkat Seluler Yang Digunakan Pengguna." },
  { "en": "Apa Itu Proses Roaming Dalam Jaringan Seluler?", "id": "Menggunakan Layanan Jaringan Operator Lain Berbeda." },
  { "en": "Apa Itu Proses Registrasi Lokasi (Location Update)?", "id": "Proses Ponsel Memberi Tahu Lokasi Selnya." },
  { "en": "Apa Itu Kanal Kontrol (Control Channel)?", "id": "Kanal Untuk Sinyal Kontrol, Bukan Suara." },
  { "en": "Apa Itu Kanal Lalu Lintas (Traffic Channel)?", "id": "Kanal Untuk Membawa Suara Atau Data." },
  { "en": "Apa Itu Paging Dalam Sistem Komunikasi Seluler?", "id": "Proses Jaringan Mencari Lokasi Ponsel Dipanggil." },
  { "en": "Apa Itu IMSI (International Mobile Subscriber Identity)?", "id": "Nomor Unik Untuk Mengidentifikasi Pelanggan Global." },
  { "en": "Apa Itu TMSI (Temporary Mobile Subscriber Identity)?", "id": "Nomor Identitas Sementara Untuk Keamanan Privasi." },
  { "en": "Apa Peran Dari BSC (Base Station Controller)?", "id": "Mengontrol Beberapa BTS Dalam Jaringan GSM." },
  { "en": "Apa Itu Kode Konvolusi (Convolutional Code)?", "id": "Jenis Kode Koreksi Error Menggunakan Konvolusi." },
  { "en": "Apa Itu Turbo Codes Dalam Channel Coding?", "id": "Kode Koreksi Error Dengan Performa Sangat." },
  { "en": "Apa Itu Kode Reed-Solomon (RS Codes)?", "id": "Kode Blok Non-Biner Untuk Koreksi Error." },
  { "en": "Di Mana Kode Reed-Solomon Sering Digunakan?", "id": "Penyimpanan Digital Seperti CD, DVD, QR." },
  { "en": "Apa Itu Interleaving Dalam Koreksi Error?", "id": "Mengubah Urutan Data Untuk Atasi Burst." },
  { "en": "Apa Itu Burst Error Dalam Komunikasi?", "id": "Error Yang Terjadi Berurutan Dalam Blok." },
  { "en": "Apa Itu Teori Antrian (Queueing Theory)?", "id": "Studi Matematis Tentang Garis Tunggu Antrian." },
  { "en": "Mengapa Teori Antrian Penting Dalam Telekomunikasi?", "id": "Untuk Menganalisis Dan Mengoptimalkan Performa Jaringan." },
  { "en": "Apa Itu Model Antrian M/M/1?", "id": "Model Dasar: Kedatangan Poisson, Layanan Eksponensial." },
  { "en": "Apa Itu Hukum Little Dalam Teori Antrian?", "id": "Menghubungkan Jumlah Pelanggan, Tingkat Kedatangan, Waktu." },
  { "en": "Apa Itu Blocking Dalam Jaringan Circuit-Switched?", "id": "Panggilan Ditolak Karena Semua Sirkuit Sibuk." },
  { "en": "Apa Satuan Yang Mengukur Lalu Lintas Telekomunikasi?", "id": "Satuan Pengukuran Lalu Lintas Adalah Erlang." },
  { "en": "Apa Itu Grade of Service (GoS)?", "id": "Probabilitas Sebuah Panggilan Diblokir Oleh Jaringan." },
  { "en": "Apa Itu Network Slicing (Pemotongan Jaringan)?", "id": "Membuat Beberapa Jaringan Virtual Diatas Infrastruktur." },
  { "en": "Di Jaringan Mana Konsep Network Slicing Kunci?", "id": "Merupakan Konsep Kunci Dalam Arsitektur 5G." },
  { "en": "Apa Itu Massive MIMO Dalam Teknologi 5G?", "id": "Sistem MIMO Dengan Jumlah Antena Sangat." },
  { "en": "Apa Itu Small Cell Dalam Jaringan Seluler?", "id": "BTS Daya Rendah Untuk Area Kecil." },
  { "en": "Mengapa Small Cell Penting Untuk Jaringan 5G?", "id": "Meningkatkan Kapasitas Dan Cakupan Di Area." },
  { "en": "Apa Itu Carrier Aggregation (Agregasi Operator)?", "id": "Menggabungkan Beberapa Pita Frekuensi Secara Bersamaan." },
  { "en": "Apa Tujuan Utama Dari Carrier Aggregation?", "id": "Untuk Meningkatkan Kecepatan Data Puncak Pengguna." },
  { "en": "Apa Itu Self-Organizing Network (SON)?", "id": "Jaringan Yang Dapat Mengkonfigurasi Diri Otomatis." },
  { "en": "Apa Itu Device-to-Device (D2D) Communication?", "id": "Komunikasi Langsung Antara Perangkat Tanpa BTS." },
  { "en": "Apa Itu Mobile Edge Computing (MEC)?", "id": "Memindahkan Komputasi Lebih Dekat Ke Pengguna." },
  { "en": "Apa Keuntungan Dari Mobile Edge Computing?", "id": "Mengurangi Latensi Dan Beban Jaringan Inti." },
  { "en": "Apa Itu Dupleks FDD (Frequency Division Duplex)?", "id": "Uplink Downlink Menggunakan Frekuensi Pita Berbeda." },
  { "en": "Apa Itu Dupleks TDD (Time Division Duplex)?", "id": "Uplink Downlink Berbagi Frekuensi, Slot Waktu." },
  { "en": "Apa Itu Radio Over Fiber (RoF)?", "id": "Mengirim Sinyal Radio Melalui Kabel Optik." },
  { "en": "Apa Itu Return Loss Dalam Pengukuran RF?", "id": "Ukuran Daya Sinyal Yang Dipantulkan Kembali." },
  { "en": "Apa Itu Insertion Loss Dalam Pengukuran RF?", "id": "Kehilangan Daya Sinyal Akibat Penyisipan Perangkat." },
  { "en": "Nilai Return Loss Yang Baik Adalah Besar?", "id": "Semakin Besar Nilainya Semakin Baik Performanya." },
  { "en": "Nilai Insertion Loss Yang Baik Adalah Kecil?", "id": "Semakin Kecil Nilainya Semakin Baik Performanya." },
  { "en": "Apa Itu Impedansi Karakteristik Saluran Transmisi?", "id": "Impedansi Yang Dilihat Gelombang Merambat Sepanjang." },
  { "en": "Berapa Impedansi Karakteristik Umum Kabel Koaksial?", "id": "Biasanya 50 Ohm Atau 75 Ohm." },
  { "en": "Apa Itu Smith Chart Dalam Teknik RF?", "id": "Alat Grafis Untuk Menyelesaikan Masalah Impedansi." },
  { "en": "Apa Itu Noise Figure (Angka Derau)?", "id": "Ukuran Penurunan SNR Akibat Komponen Elektronik." },
  { "en": "Nilai Noise Figure Yang Baik Adalah Kecil?", "id": "Semakin Kecil Angka Derau Semakin Baik." },
  { "en": "Apa Itu Phase Noise Dalam Osilator?", "id": "Fluktuasi Fasa Yang Tidak Diinginkan Pada." },
  { "en": "Apa Itu Amplifier Linearitas (Amplifier Linearity)?", "id": "Kemampuan Amplifier Menguatkan Sinyal Tanpa Distorsi." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Titik Dimana Gain Amplifier Turun 1." },
  { "en": "Apa Itu Intermodulation Distortion (IMD)?", "id": "Produk Frekuensi Baru Akibat Non-Linearitas Perangkat." },
  { "en": "Apa Itu Third-Order Intercept Point (IP3)?", "id": "Ukuran Teoritis Linearitas Suatu Perangkat Elektronik." },
  { "en": "Apa Itu Channel Sounding Dalam Komunikasi Nirkabel?", "id": "Proses Mengukur Karakteristik Kanal Komunikasi Nirkabel." },
  { "en": "Apa Itu Hamming Code Dalam Channel Coding?", "id": "Jenis Kode Blok Linear Untuk Koreksi." },
  { "en": "Apa Itu Viterbi Algorithm Dalam Teknik Komunikasi?", "id": "Algoritma Untuk Mendekode Kode Konvolusi Secara." },
  { "en": "Apa Itu Trellis Diagram Dalam Pengkodean?", "id": "Diagram State Untuk Merepresentasikan Kode Konvolusi." },
  { "en": "Apa Itu Nyquist Filter Dalam Komunikasi Digital?", "id": "Filter Untuk Mengeliminasi Inter-Symbol Interference (ISI)." },
  { "en": "Apa Itu Raised-Cosine Filter Dalam Teknik Komunikasi?", "id": "Jenis Nyquist Filter Yang Praktis Digunakan." },
  { "en": "Apa Itu Equalizer Dalam Sistem Komunikasi?", "id": "Filter Untuk Mengkompensasi Distorsi Kanal Komunikasi." },
  { "en": "Apa Itu Adaptive Equalizer Dalam Sistem Komunikasi?", "id": "Equalizer Yang Dapat Menyesuaikan Diri Otomatis." },
  { "en": "Apa Kepanjangan WSN Dalam Teknologi Jaringan?", "id": "WSN Adalah Wireless Sensor Network." },
  { "en": "Apa Komponen Utama Dari Wireless Sensor Network?", "id": "Banyak Sensor Node Dan Satu Sink." },
  { "en": "Apa Fungsi Dari Sensor Node Dalam WSN?", "id": "Mengumpulkan Data Dari Lingkungan Sekitar Fisik." },
  { "en": "Apa Fungsi Dari Sink Node (Gateway) WSN?", "id": "Mengumpulkan Data Dari Semua Sensor Node." },
  { "en": "Apa Tantangan Utama Dalam Desain Jaringan WSN?", "id": "Konsumsi Energi Yang Sangat Rendah (Hemat)." },
  { "en": "Sebutkan Contoh Aplikasi Dari Jaringan WSN?", "id": "Pemantauan Lingkungan, Pertanian Presisi, Kesehatan Medis." },
  { "en": "Apa Itu Protokol Routing LEACH Untuk WSN?", "id": "Protokol Hemat Energi Berbasis Cluster Hierarkis." },
  { "en": "Apa Itu Difraksi (Diffraction) Gelombang Radio?", "id": "Pembelokan Gelombang Saat Melewati Ujung Tajam." },
  { "en": "Kapan Efek Difraksi Menjadi Sangat Signifikan?", "id": "Ketika Ukuran Hambatan Sebanding Panjang Gelombang." },
  { "en": "Apa Itu Refraksi (Refraction) Gelombang Radio?", "id": "Pembelokan Gelombang Saat Melintasi Media Berbeda." },
  { "en": "Apa Penyebab Refraksi Di Atmosfer Bumi?", "id": "Perubahan Kepadatan Dan Suhu Lapisan Atmosfer." },
  { "en": "Apa Itu Hamburan (Scattering) Gelombang Radio?", "id": "Pancaran Ulang Energi Gelombang Arah Acak." },
  { "en": "Apa Yang Menyebabkan Hamburan Gelombang Radio?", "id": "Objek Kecil Dibandingkan Panjang Gelombang Radio." },
  { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Fluktuasi Sinyal Cepat Akibat Efek Multipath." },
  { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Fluktuasi Sinyal Lambat Akibat Shadowing Hambatan." },
  { "en": "Apa Itu Shadowing Dalam Propagasi Nirkabel?", "id": "Pelemahan Sinyal Akibat Hambatan Besar Fisik." },
  { "en": "Sebutkan Contoh Model Propagasi Kanal Nirkabel?", "id": "Model Okumura, Hata, Dan Juga Cost-231." },
  { "en": "Apa Itu Link Budget Dalam Desain Nirkabel?", "id": "Perhitungan Semua Penguatan Kerugian Sistem Komunikasi." },
  { "en": "Apa Tujuan Dari Perhitungan Link Budget?", "id": "Memastikan Daya Terima Cukup Untuk Komunikasi." },
  { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Total Yang Dipancarkan Dari Antena." },
  { "en": "Apa Itu Kabel Laut (Submarine Cable) Telekomunikasi?", "id": "Kabel Serat Optik Diletakkan Di Dasar." },
  { "en": "Apa Fungsi Utama Dari Kabel Laut Internasional?", "id": "Menghubungkan Jaringan Internet Dan Telekomunikasi Antarbenua." },
  { "en": "Apa Keuntungan Komunikasi Menggunakan Kabel Bawah Laut?", "id": "Kapasitas Sangat Besar Dan Latensi Rendah." },
  { "en": "Apa Standar ITU-T Untuk Penomoran Telepon Internasional?", "id": "Rekomendasi E.164 Dari Badan Standardisasi ITU-T." },
  { "en": "Apa Komponen Utama Dari Nomor E.164?", "id": "Kode Negara, Kode Jaringan, Nomor Pelanggan." },
  { "en": "Apa Itu DTH (Direct-to-Home) Broadcasting Satelit?", "id": "Siaran Televisi Langsung Dari Satelit Ke." },
  { "en": "Satelit Orbit Apa Yang Digunakan Untuk DTH?", "id": "Umumnya Menggunakan Satelit Geostasioner Atau GEO." },
  { "en": "Apa Itu Transmisi SCPC (Single Channel Per Carrier)?", "id": "Satu Sinyal Saluran Memodulasi Satu Carrier Satelit." },
  { "en": "Apa Itu Transmisi MCPC (Multiple Channel Per Carrier)?", "id": "Beberapa Saluran Sinyal Digabung Memodulasi Carrier." },
  { "en": "Apa Itu Antena Cerdas (Smart Antenna)?", "id": "Antena Yang Dapat Mengubah Pola Radiasinya." },
  { "en": "Apa Tujuan Utama Dari Penggunaan Antena Cerdas?", "id": "Meningkatkan Kualitas Sinyal, Mengurangi Interferensi Sekitar." },
  { "en": "Apa Itu Sistem Antena Switched Beam?", "id": "Memilih Salah Satu Beam Tetap Arah." },
  { "en": "Apa Itu Sistem Antena Adaptive Array?", "id": "Secara Otomatis Mengarahkan Beam Ke Pengguna." },
  { "en": "Apa Itu Null Steering Dalam Antena Cerdas?", "id": "Mengarahkan Null Pola Radiasi Ke Interferer." },
  { "en": "Apa Itu Interkoneksi Dalam Dunia Regulasi Telekomunikasi?", "id": "Hubungan Antara Jaringan Dua Operator Berbeda." },
  { "en": "Mengapa Interkoneksi Sangat Penting Bagi Industri?", "id": "Memungkinkan Pelanggan Operator Berbeda Saling Berkomunikasi." },
  { "en": "Apa Itu Biaya Terminasi (Termination Charge)?", "id": "Biaya Dibayar Operator Asal Ke Tujuan." },
  { "en": "Apa Itu USO (Universal Service Obligation)?", "id": "Kewajiban Operator Menyediakan Layanan Di Daerah." },
  { "en": "Siapa Yang Menetapkan Aturan USO Di Indonesia?", "id": "Pemerintah, Melalui Kementerian Komunikasi Dan Informatika." },
  { "en": "Apa Itu LRIC (Long-Run Incremental Cost)?", "id": "Model Perhitungan Biaya Untuk Menentukan Tarif." },
  { "en": "Apa Itu Local Loop Unbundling (LLU)?", "id": "Operator Lain Menyewa Jaringan Akses Operator." },
  { "en": "Apa Itu Mobile Number Portability (MNP)?", "id": "Memungkinkan Pelanggan Pindah Operator Tanpa Ganti." },
  { "en": "Apa Itu Gray Code Dalam Pengkodean Digital?", "id": "Urutan Kode Dimana Dua Nilai Berurutan." },
  { "en": "Apa Keuntungan Utama Menggunakan Gray Code?", "id": "Mengurangi Kemungkinan Error Dalam Sistem Transisi." },
  { "en": "Di Mana Gray Code Sering Digunakan?", "id": "Dalam Modulasi QAM Dan Rotary Encoder." },
  { "en": "Apa Itu Scrambler Dalam Komunikasi Digital?", "id": "Membuat Urutan Bit Tampak Lebih Acak." },
  { "en": "Mengapa Proses Scrambling Diperlukan Dalam Transmisi?", "id": "Menghindari Deretan Bit Panjang, Membantu Sinkronisasi." },
  { "en": "Apa Itu Network Address Translation (NAT)?", "id": "Mengubah Alamat IP Privat Menjadi IP." },
  { "en": "Apa Tujuan Utama Dari Penerapan Teknologi NAT?", "id": "Menghemat Penggunaan Alamat IPv4 Yang Terbatas." },
  { "en": "Apa Itu Port Address Translation (PAT)?", "id": "Bentuk NAT Dinamis Memetakan Banyak Alamat." },
  { "en": "Apa Itu Dynamic Host Configuration Protocol (DHCP)?", "id": "Protokol Memberikan Alamat IP Secara Otomatis." },
  { "en": "Apa Manfaat Menggunakan Server DHCP Di Jaringan?", "id": "Memudahkan Manajemen Dan Alokasi Alamat IP." },
  { "en": "Apa Itu Protokol ARP (Address Resolution Protocol)?", "id": "Memetakan Alamat IP Ke Alamat Fisik." },
  { "en": "Apa Itu Proxy Server Dalam Jaringan Komputer?", "id": "Server Bertindak Sebagai Perantara Permintaan Klien." },
  { "en": "Apa Itu Caching Dalam Konteks Web Proxy?", "id": "Menyimpan Salinan Konten Web Yang Sering." },
  { "en": "Apa Keuntungan Menggunakan Web Cache Proxy?", "id": "Mempercepat Waktu Akses Dan Menghemat Bandwidth." },
  { "en": "Apa Itu Reverse Proxy Server Dalam Jaringan?", "id": "Perantara Yang Menerima Permintaan Klien, Meneruskan." },
  { "en": "Apa Fungsi Umum Dari Reverse Proxy?", "id": "Load Balancing, Keamanan, Dan Juga Caching." },
  { "en": "Apa Itu Load Balancer Dalam Arsitektur Server?", "id": "Mendistribusikan Beban Kerja Ke Beberapa Server." },
  { "en": "Apa Tujuan Utama Dari Load Balancing?", "id": "Meningkatkan Responsivitas Dan Ketersediaan Layanan Aplikasi." },
  { "en": "Apa Itu Content Delivery Network (CDN)?", "id": "Jaringan Server Terdistribusi Menyimpan Konten Dekat." },
  { "en": "Apa Manfaat Utama Menggunakan Layanan CDN?", "id": "Mempercepat Pengiriman Konten Web Secara Global." },
  { "en": "Apa Itu Jitter Dalam Transmisi Suara IP?", "id": "Variasi Waktu Kedatangan Paket Suara Digital." },
  { "en": "Bagaimana Jitter Mempengaruhi Kualitas Panggilan VoIP?", "id": "Menyebabkan Suara Terdengar Terputus-Putus Atau Kacau." },
  { "en": "Apa Itu Deep Packet Inspection (DPI)?", "id": "Teknik Menganalisis Konten Paket Data Jaringan." },
  { "en": "Apa Itu Wavelength-Selective Switch (WSS)?", "id": "Perangkat Mengalihkan Sinyal Optik Berdasarkan Panjang." },
  { "en": "Apa Itu Reconfigurable Optical Add-Drop Multiplexer (ROADM)?", "id": "Perangkat Untuk Menambah Atau Mengambil Sinyal." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Elektronik Untuk Sinkronisasi Sinyal Frekuensi." },
  { "en": "Apa Fungsi Utama PLL Dalam Penerima Radio?", "id": "Untuk Demodulasi Sinyal Dan Sintesis Frekuensi." },
  { "en": "Apa Itu Mixer Dalam Sirkuit Frekuensi Radio?", "id": "Sirkuit Non-Linear Yang Mencampur Dua Sinyal." },
  { "en": "Apa Hasil Dari Proses Mixing Dua Sinyal?", "id": "Menghasilkan Frekuensi Penjumlahan Dan Juga Pengurangan." },
  { "en": "Apa Itu Penerima Superheterodyne Dalam Sistem Radio?", "id": "Arsitektur Penerima Mengubah Frekuensi RF Ke." },
  { "en": "Apa Itu Frekuensi Antara (Intermediate Frequency)?", "id": "Frekuensi Tetap Hasil Proses Mixing Penerima." },
  { "en": "Apa Keuntungan Arsitektur Penerima Superheterodyne?", "id": "Selektivitas Dan Sensitivitas Yang Sangat Baik." },
  { "en": "Apa Itu Direct Conversion Receiver (Zero-IF)?", "id": "Arsitektur Penerima Langsung Mengubah Sinyal RF." },
  { "en": "Apa Itu Doppler Spread Dalam Kanal Nirkabel?", "id": "Penyebaran Spektral Sinyal Akibat Gerakan Pengguna." },
  { "en": "Apa Itu Coherence Time Dari Kanal Nirkabel?", "id": "Durasi Waktu Dimana Respons Kanal Konstan." },
  { "en": "Apa Hubungan Coherence Time Dan Doppler Spread?", "id": "Berbanding Terbalik Satu Sama Lainnya Keduanya." },
  { "en": "Apa Itu Coherence Bandwidth Dari Suatu Kanal?", "id": "Rentang Frekuensi Dimana Kanal Dianggap Flat." },
  { "en": "Apa Itu Delay Spread Dalam Kanal Multipath?", "id": "Perbedaan Waktu Tiba Antara Lintasan Pertama." },
  { "en": "Apa Itu Flat Fading Dalam Komunikasi?", "id": "Semua Komponen Frekuensi Sinyal Mengalami Fading." },
  { "en": "Apa Itu Frequency Selective Fading Komunikasi?", "id": "Hanya Sebagian Komponen Frekuensi Mengalami Fading." },
  { "en": "Apa Itu Rake Receiver Dalam Komunikasi CDMA?", "id": "Penerima Yang Memanfaatkan Sinyal Multipath Berbeda." },
  { "en": "Bagaimana Rake Receiver Meningkatkan Kinerja Sistem?", "id": "Dengan Menggabungkan Energi Dari Berbagai Lintasan." },
  { "en": "Apa Itu Near-Far Problem Dalam Sistem CDMA?", "id": "Sinyal Kuat Pengguna Dekat Mengalahkan Sinyal." },
  { "en": "Bagaimana Cara Mengatasi Near-Far Problem tersebut?", "id": "Menggunakan Kontrol Daya (Power Control) Ketat." },
  { "en": "Apa Itu Soft Handover Dalam Jaringan CDMA?", "id": "Perangkat Terhubung Ke Dua BTS Sekaligus." },
  { "en": "Apa Keuntungan Dari Soft Handover Dibanding Hard?", "id": "Transisi Lebih Mulus Dan Koneksi Lebih." },
  { "en": "Apa Itu Orthogonal Variable Spreading Factor (OVSF)?", "id": "Kode Yang Digunakan Dalam Jaringan WCDMA." },
  { "en": "Apa Itu Network Time Protocol (NTP)?", "id": "Protokol Untuk Sinkronisasi Waktu Antar Komputer." },
  { "en": "Mengapa Sinkronisasi Waktu Penting Dalam Jaringan?", "id": "Penting Untuk Keamanan, Logging, Dan Transaksi." },
  { "en": "Apa Itu Session Initiation Protocol (SIP)?", "id": "Protokol Sinyal Untuk Sesi Komunikasi Multimedia." },
  { "en": "Dimana Protokol SIP Umumnya Sering Digunakan?", "id": "Digunakan Secara Luas Dalam Sistem VoIP." },
  { "en": "Apa Itu Real-time Transport Protocol (RTP)?", "id": "Protokol Untuk Mengirimkan Data Audio Video." },
  { "en": "Apa Fungsi RTCP (RTP Control Protocol)?", "id": "Memberikan Umpan Balik Kualitas Layanan RTP." },
  { "en": "Apa Itu Cyclic Prefix Dalam Modulasi OFDM?", "id": "Menyalin Akhir Simbol Ke Bagian Awal." },
  { "en": "Apa Tujuan Menambahkan Cyclic Prefix Di OFDM?", "id": "Mengatasi Inter-Symbol Interference Akibat Efek Multipath." },
  { "en": "Apa Kepanjangan Dari DSL Teknologi Internet Kabel?", "id": "DSL Adalah Digital Subscriber Line." },
  { "en": "Media Apa Yang Digunakan Teknologi Koneksi DSL?", "id": "Menggunakan Saluran Kabel Telepon Tembaga Biasa." },
  { "en": "Apa Kepanjangan Dari ADSL Jenis Koneksi DSL?", "id": "ADSL Adalah Asymmetric Digital Subscriber Line." },
  { "en": "Mengapa ADSL Disebut Sebagai Asymmetric (Asimetris)?", "id": "Kecepatan Unduh Lebih Cepat Dari Unggah." },
  { "en": "Apa Kepanjangan Dari VDSL Jenis Koneksi DSL?", "id": "VDSL Adalah Very-high-bit-rate Digital Subscriber Line." },
  { "en": "Apa Kelebihan VDSL Dibandingkan Teknologi ADSL?", "id": "Menawarkan Kecepatan Jauh Lebih Tinggi Cepat." },
  { "en": "Apa Kepanjangan HFC Jaringan TV Kabel?", "id": "HFC Adalah Hybrid Fiber-Coaxial." },
  { "en": "Bagaimana Arsitektur Dasar Jaringan Modern HFC?", "id": "Kombinasi Serat Optik Dan Kabel Koaksial." },
  { "en": "Apa Kepanjangan FTTx Dalam Jaringan Akses Optik?", "id": "FTTx Adalah Fiber to the x." },
  { "en": "Apa Kepanjangan Dari FTTN Dalam Arsitektur FTTx?", "id": "FTTN Adalah Fiber-to-the-Node." },
  { "en": "Di Mana Titik Akhir Serat Optik FTTN?", "id": "Berakhir Di Sebuah Kabinet Jalan (Node)." },
  { "en": "Apa Kepanjangan Dari FTTC Dalam Arsitektur FTTx?", "id": "FTTC Adalah Fiber-to-the-Curb." },
  { "en": "Di Mana Titik Akhir Serat Optik FTTC?", "id": "Lebih Dekat Ke Pelanggan Daripada FTTN." },
  { "en": "Apa Kepanjangan Dari FTTB Dalam Arsitektur FTTx?", "id": "FTTB Adalah Fiber-to-the-Building." },
  { "en": "Di Mana Titik Akhir Serat Optik FTTB?", "id": "Berakhir Di Dalam Gedung Atau Apartemen." },
  { "en": "Apa Itu Routing Dalam Jaringan Komputer?", "id": "Proses Memilih Jalur Terbaik Untuk Data." },
  { "en": "Apa Itu Routing Statis (Static Routing)?", "id": "Rute Dikonfigurasi Secara Manual Oleh Administrator." },
  { "en": "Apa Keuntungan Utama Dari Penggunaan Routing Statis?", "id": "Sangat Aman Dan Beban CPU Rendah." },
  { "en": "Apa Itu Routing Dinamis (Dynamic Routing)?", "id": "Router Saling Bertukar Informasi Secara Otomatis." },
  { "en": "Apa Keuntungan Utama Dari Penggunaan Routing Dinamis?", "id": "Skalabilitas Baik Dan Adaptif Terhadap Perubahan." },
  { "en": "Apa Itu Algoritma Routing Distance Vector?", "id": "Router Hanya Tahu Jarak Ke Tetangga." },
  { "en": "Sebutkan Contoh Protokol Routing Distance Vector?", "id": "RIP (Routing Information Protocol) Adalah Contoh." },
  { "en": "Apa Masalah Utama Algoritma Distance Vector?", "id": "Masalah Loops Dan Konvergensi Yang Lambat." },
  { "en": "Apa Itu Algoritma Routing Link State?", "id": "Setiap Router Memiliki Peta Seluruh Jaringan." },
  { "en": "Sebutkan Contoh Protokol Routing Link State?", "id": "OSPF (Open Shortest Path First) Adalah Contoh." },
  { "en": "Apa Itu Alokasi Spektrum Frekuensi Radio?", "id": "Proses Menetapkan Pita Frekuensi Untuk Layanan." },
  { "en": "Apa Itu Lisensi Spektrum Frekuensi Radio?", "id": "Izin Resmi Untuk Menggunakan Pita Frekuensi." },
  { "en": "Apa Itu Lelang Spektrum (Spectrum Auction)?", "id": "Metode Pemerintah Menjual Hak Penggunaan Spektrum." },
  { "en": "Apa Itu Spectrum Refarming Dalam Dunia Telekomunikasi?", "id": "Mengalokasikan Ulang Spektrum Untuk Teknologi Baru." },
  { "en": "Apa Itu DSA (Dynamic Spectrum Access)?", "id": "Teknik Mengizinkan Pengguna Sekunder Mengakses Spektrum." },
  { "en": "Apa Itu Statistical TDM (Time Division Multiplexing)?", "id": "Alokasi Slot Waktu Dinamis Berdasarkan Permintaan." },
  { "en": "Apa Keuntungan Statistical TDM Dibanding TDM Sinkron?", "id": "Pemanfaatan Bandwidth Jauh Lebih Efisien Sekali." },
  { "en": "Apa Kepanjangan CWDM Dalam Multiplexing Optik?", "id": "CWDM Adalah Coarse Wavelength Division Multiplexing." },
  { "en": "Apa Kepanjangan DWDM Dalam Multiplexing Optik?", "id": "DWDM Adalah Dense Wavelength Division Multiplexing." },
  { "en": "Apa Perbedaan Utama Antara CWDM Dan DWDM?", "id": "Jarak Antar Panjang Gelombang DWDM Sempit." },
  { "en": "Mana Yang Memiliki Kapasitas Kanal Lebih Banyak?", "id": "DWDM Menawarkan Kapasitas Kanal Jauh Lebih." },
  { "en": "Apa Itu Orthogonalitas Dalam Modulasi Teknik OFDM?", "id": "Subcarrier Tidak Saling Menginterferensi Satu Sama." },
  { "en": "Apa Itu PAPR (Peak-to-Average Power Ratio)?", "id": "Rasio Daya Puncak Terhadap Daya Rata-Rata." },
  { "en": "Apa Masalah Utama PAPR Tinggi Di OFDM?", "id": "Menyebabkan Distorsi Pada Power Amplifier Non-Linear." },
  { "en": "Apa Itu Klasifikasi Paket Dalam Mekanisme QoS?", "id": "Mengidentifikasi Dan Mengelompokkan Lalu Lintas Data." },
  { "en": "Apa Itu Penandaan Paket (Packet Marking) QoS?", "id": "Memberi Tanda Paket Untuk Perlakuan Prioritas." },
  { "en": "Sebutkan Contoh Metode Penandaan Paket Jaringan?", "id": "DSCP (Differentiated Services Code Point) Adalah Contoh." },
  { "en": "Apa Itu Antrian (Queuing) Dalam Manajemen Jaringan?", "id": "Menahan Paket Ketika Jaringan Sedang Sibuk." },
  { "en": "Bagaimana Cara Kerja Antrian FIFO (First-In First-Out)?", "id": "Paket Yang Datang Pertama Dilayani Dahulu." },
  { "en": "Bagaimana Cara Kerja WFQ (Weighted Fair Queuing)?", "id": "Membagi Bandwidth Secara Adil Antar Aliran." },
  { "en": "Apa Itu Policing Dalam Mekanisme Jaringan QoS?", "id": "Membuang Paket Yang Melebihi Batas Tertentu." },
  { "en": "Apa Itu Shaping Dalam Mekanisme Jaringan QoS?", "id": "Menunda Paket Yang Melebihi Batas Tertentu." },
  { "en": "Apa Perbedaan Utama Policing Dan Shaping?", "id": "Policing Membuang Paket, Shaping Menunda Paket." },
  { "en": "Apa Itu Availability Atau Ketersediaan Jaringan?", "id": "Persentase Waktu Jaringan Beroperasi Secara Normal." },
  { "en": "Bagaimana Availability Umumnya Dinyatakan Atau Diukur?", "id": "Dalam Persentase, Contohnya 'Five Nines' (99.999%)." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antara Dua Kegagalan Sistem." },
  { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu Rata-Rata Untuk Memperbaiki Sistem Rusak." },
  { "en": "Bagaimana Menghitung Availability Menggunakan MTBF MTTR?", "id": "Availability Adalah MTBF Dibagi (MTBF + MTTR)." },
  { "en": "Apa Itu Redundansi Dalam Desain Jaringan?", "id": "Menyediakan Komponen Cadangan Untuk Mengambil Alih." },
  { "en": "Apa Tujuan Utama Dari Redundansi Jaringan?", "id": "Untuk Meningkatkan Ketersediaan Dan Keandalan Jaringan." },
  { "en": "Apa Itu Hot Standby Dalam Sistem Redundansi?", "id": "Sistem Cadangan Menyala Dan Siap Mengambil." },
  { "en": "Apa Itu Cold Standby Dalam Sistem Redundansi?", "id": "Sistem Cadangan Mati, Perlu Dinyalakan Manual." },
  { "en": "Apa Itu Protokol BGP (Border Gateway Protocol)?", "id": "Protokol Routing Yang Digunakan Antar Autonomous System." },
  { "en": "Apa Itu Autonomous System (AS) Di Internet?", "id": "Sekumpulan Jaringan IP Di Bawah Satu." },
  { "en": "Apa Itu Interior Gateway Protocol (IGP)?", "id": "Protokol Routing Digunakan Di Dalam Satu." },
  { "en": "Apa Itu Exterior Gateway Protocol (EGP)?", "id": "Protokol Routing Digunakan Antar Autonomous System." },
  { "en": "Apakah BGP Termasuk Kategori IGP Atau EGP?", "id": "BGP Adalah Contoh Protokol EGP." },
  { "en": "Apa Itu Metrik Dalam Protokol Routing?", "id": "Nilai Yang Digunakan Untuk Mengukur Kebaikan." },
  { "en": "Metrik Apa Yang Digunakan Oleh Protokol RIP?", "id": "RIP Menggunakan Hop Count Sebagai Metriknya." },
  { "en": "Metrik Apa Yang Digunakan Oleh Protokol OSPF?", "id": "OSPF Menggunakan Cost Berdasarkan Bandwidth Jalur." },
  { "en": "Apa Itu Administrative Distance Dalam Routing?", "id": "Ukuran Kepercayaan Terhadap Sumber Informasi Rute." },
  { "en": "Apa Itu Route Summarization Atau Agregasi?", "id": "Menggabungkan Beberapa Rute Menjadi Satu Rute." },
  { "en": "Apa Tujuan Dari Melakukan Route Summarization?", "id": "Mengurangi Ukuran Tabel Routing Pada Router." },
  { "en": "Apa Itu Equal-Cost Multi-Path (ECMP) Routing?", "id": "Mendistribusikan Lalu Lintas Ke Beberapa Jalur." },
  { "en": "Apa Itu Policy-Based Routing (PBR)?", "id": "Meneruskan Paket Berdasarkan Kebijakan Selain Tujuan." },
  { "en": "Apa Itu Multicast Dalam Komunikasi Jaringan?", "id": "Mengirim Paket Ke Sekelompok Penerima Tertentu." },
  { "en": "Apa Perbedaan Unicast, Multicast, Dan Broadcast?", "id": "Unicast Satu-ke-Satu, Multicast Satu-ke-Banyak, Broadcast Satu-ke-Semua." },
  { "en": "Apa Fungsi Protokol IGMP (Internet Group Management)?", "id": "Host Memberi Tahu Router Keanggotaan Grup." },
  { "en": "Apa Itu Anycast Dalam Komunikasi Jaringan?", "id": "Mengirim Paket Ke Anggota Terdekat Grup." },
  { "en": "Apa Itu Tunneling Dalam Jaringan Komputer?", "id": "Membungkus Satu Protokol Jaringan Di Dalam." },
  { "en": "Apa Itu Generic Routing Encapsulation (GRE)?", "id": "Protokol Tunneling Yang Dikembangkan Oleh Cisco." },
  { "en": "Apa Itu Kode Hamming Dalam Teori Pengkodean?", "id": "Jenis Kode Blok Linear Pendeteksi Koreksi." },
  { "en": "Apa Itu Jarak Hamming (Hamming Distance)?", "id": "Jumlah Posisi Bit Yang Berbeda Antara." },
  { "en": "Apa Itu Parity Bit Dalam Deteksi Error?", "id": "Bit Tambahan Untuk Membuat Jumlah Bit." },
  { "en": "Apa Itu Checksum Dalam Deteksi Error?", "id": "Nilai Dihitung Dari Blok Data Untuk." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Teknik Deteksi Error Yang Lebih Kuat." },
  { "en": "Apa Itu Polar Code Dalam Teori Pengkodean?", "id": "Jenis Kode Koreksi Error Modern Terbukti." },
  { "en": "Di Mana Polar Code Digunakan Saat Ini?", "id": "Digunakan Pada Kanal Kontrol Jaringan 5G." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Koreksi Error Dengan Performa Mendekati." },
  { "en": "Di Mana LDPC Code Banyak Digunakan?", "id": "Wi-Fi (802.11n/ac), DVB-S2, Dan 5G." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Sirkuit Yang Otomatis Menyesuaikan Penguatan Amplifier." },
  { "en": "Apa Tujuan Utama Dari Sirkuit AGC?", "id": "Menjaga Level Sinyal Output Tetap Konstan." },
  { "en": "Apa Itu Space-Time Coding (STC)?", "id": "Teknik Mengirim Data Di Ruang Waktu." },
  { "en": "Apa Itu Space-Frequency Coding (SFC)?", "id": "Teknik Mengirim Data Di Ruang Frekuensi." },
  { "en": "Apa Itu Diversity Dalam Komunikasi Nirkabel?", "id": "Teknik Mengirim Sinyal Lewat Kanal Independen." },
  { "en": "Apa Tujuan Utama Dari Teknik Diversity?", "id": "Untuk Melawan Efek Fading Dan Meningkatkan." },
  { "en": "Sebutkan Beberapa Jenis Teknik Diversity Nirkabel?", "id": "Space, Frequency, Time, Dan Polarization Diversity." },
  { "en": "Apa Itu Space Diversity (Keanekaragaman Ruang)?", "id": "Menggunakan Beberapa Antena Yang Terpisah Jarak." },
  { "en": "Apa Itu Frequency Diversity (Keanekaragaman Frekuensi)?", "id": "Mengirim Sinyal Yang Sama Pada Frekuensi." },
  { "en": "Apa Itu Time Diversity (Keanekaragaman Waktu)?", "id": "Mengirim Sinyal Yang Sama Pada Slot." },
  { "en": "Apa Itu Combining Technique Dalam Sistem Diversity?", "id": "Metode Menggabungkan Sinyal Dari Berbagai Cabang." },
  { "en": "Sebutkan Teknik Combining Yang Paling Umum?", "id": "Selection, Equal Gain, Dan Maximal-Ratio Combining." },
  { "en": "Apa Itu IPv6 (Internet Protocol version 6)?", "id": "Versi Terbaru Protokol Internet Pengganti IPv4." },
  { "en": "Berapa Panjang Alamat Dari Protokol IPv6?", "id": "Alamat IPv6 Memiliki Panjang 128-bit." },
  { "en": "Berapa Panjang Alamat Dari Protokol IPv4?", "id": "Alamat IPv4 Memiliki Panjang 32-bit." },
  { "en": "Mengapa Dunia Membutuhkan Protokol Internet IPv6?", "id": "Karena Alamat IPv4 Sudah Hampir Habis." },
  { "en": "Bagaimana Alamat IPv6 Ditulis Secara Konvensional?", "id": "Dalam Delapan Grup Angka Heksadesimal Dipisah." },
  { "en": "Apa Keuntungan Utama Dari Protokol IPv6?", "id": "Ruang Alamat Sangat Besar, Keamanan Bawaan." },
  { "en": "Apa Itu IPsec (Internet Protocol Security)?", "id": "Fitur Keamanan Yang Wajib Ada IPv6." },
  { "en": "Apa Itu SLAAC Dalam Konfigurasi IPv6?", "id": "Stateless Address Autoconfiguration, Konfigurasi Alamat Otomatis." },
  { "en": "Apa Itu Dual Stack Dalam Transisi IPv6?", "id": "Perangkat Menjalankan IPv4 Dan IPv6 Bersamaan." },
  { "en": "Apa Itu Tunneling 6to4 Mekanisme Transisi?", "id": "Membungkus Paket IPv6 Di Dalam Paket." },
  { "en": "Apa Jenis Alamat Dalam Protokol IPv6?", "id": "Unicast, Multicast, Dan Juga Jenis Anycast." },
  { "en": "Apakah Terdapat Alamat Broadcast Di IPv6?", "id": "Tidak, Fungsinya Digantikan Oleh Alamat Multicast." },
  { "en": "Apa Itu Alamat Link-Local Di IPv6?", "id": "Alamat Yang Hanya Valid Di Link." },
  { "en": "Apa Awalan Alamat Link-Local IPv6 Selalu?", "id": "Alamat Selalu Dimulai Dengan Awalan FE80." },
  { "en": "Apa Itu Unique Local Address (ULA)?", "id": "Alamat Mirip IP Privat Untuk Jaringan." },
  { "en": "Apa Itu Global Unicast Address (GUA)?", "id": "Alamat Unik Global Yang Dapat Dirutekan." },
  { "en": "Apa Kepanjangan WEP Keamanan Jaringan Nirkabel?", "id": "WEP Adalah Wired Equivalent Privacy." },
  { "en": "Mengapa Standar Keamanan WEP Tidak Lagi Aman?", "id": "Memiliki Celah Kriptografi Yang Sangat Serius." },
  { "en": "Apa Kepanjangan WPA Keamanan Jaringan Nirkabel?", "id": "WPA Adalah Wi-Fi Protected Access." },
  { "en": "Apa Peningkatan WPA Dibandingkan Dengan Standar WEP?", "id": "Menggunakan TKIP (Temporal Key Integrity Protocol)." },
  { "en": "Apa Kepanjangan WPA2 Keamanan Jaringan Nirkabel?", "id": "WPA2 Adalah Wi-Fi Protected Access II." },
  { "en": "Standar Enkripsi Apa Yang Digunakan Oleh WPA2?", "id": "Menggunakan AES (Advanced Encryption Standard) Kuat." },
  { "en": "Apa Kepanjangan WPA3 Keamanan Jaringan Nirkabel?", "id": "WPA3 Adalah Wi-Fi Protected Access III." },
  { "en": "Apa Peningkatan Utama Pada Standar WPA3?", "id": "Keamanan Lebih Baik Untuk Jaringan Wi-Fi." },
  { "en": "Apa Itu Standar Keamanan IEEE 802.1X?", "id": "Standar Otentikasi Berbasis Port Untuk Jaringan." },
  { "en": "Dimana Standar 802.1X Umumnya Banyak Digunakan?", "id": "Di Jaringan Wi-Fi Perusahaan (Enterprise Mode)." },
  { "en": "Apa Itu RADIUS (Remote Authentication Dial-In User)?", "id": "Server Untuk Otentikasi, Otorisasi, Akuntansi Terpusat." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Unik Jaringan Nirkabel Wi-Fi." },
  { "en": "Apa Itu Serangan Evil Twin Jaringan Wi-Fi?", "id": "Access Point Palsu Meniru Jaringan Asli." },
  { "en": "Apa Itu Teori Teletraffic Atau Lalu Lintas?", "id": "Aplikasi Teori Probabilitas Pada Jaringan Telekomunikasi." },
  { "en": "Apa Itu Proses Kedatangan Panggilan (Call Arrival)?", "id": "Model Statistik Bagaimana Panggilan Masuk Jaringan." },
  { "en": "Distribusi Probabilitas Apa Memodelkan Kedatangan Panggilan Acak?", "id": "Distribusi Poisson Sering Digunakan Sebagai Model." },
  { "en": "Apa Itu Waktu Layanan (Holding Time)?", "id": "Durasi Waktu Panggilan Menggunakan Sebuah Sirkuit." },
  { "en": "Distribusi Apa Memodelkan Waktu Layanan Panggilan?", "id": "Distribusi Eksponensial Sering Digunakan Sebagai Model." },
  { "en": "Apa Itu Formula Erlang B Dalam Teori?", "id": "Menghitung Probabilitas Blocking Dalam Sistem Loss." },
  { "en": "Apa Asumsi Utama Dalam Sistem Erlang B?", "id": "Panggilan Yang Diblokir Akan Langsung Hilang." },
  { "en": "Apa Itu Formula Erlang C Dalam Teori?", "id": "Menghitung Probabilitas Panggilan Harus Menunggu Antrian." },
  { "en": "Apa Asumsi Utama Dalam Sistem Erlang C?", "id": "Panggilan Yang Datang Akan Menunggu Antrian." },
  { "en": "Apa Itu Busy Hour Dalam Analisis Trafik?", "id": "Periode Satu Jam Dengan Beban Trafik." },
  { "en": "Apa Kepanjangan DVB Standar Penyiaran Digital?", "id": "DVB Adalah Digital Video Broadcasting." },
  { "en": "Di Mana Standar DVB Banyak Digunakan?", "id": "Digunakan Secara Luas Di Eropa, Asia." },
  { "en": "Apa Kepanjangan ATSC Standar Penyiaran Digital?", "id": "ATSC Adalah Advanced Television Systems Committee." },
  { "en": "Di Mana Standar ATSC Banyak Digunakan?", "id": "Digunakan Di Amerika Utara Dan Korea." },
  { "en": "Apa Kepanjangan ISDB Standar Penyiaran Digital?", "id": "ISDB Adalah Integrated Services Digital Broadcasting." },
  { "en": "Di Mana Standar ISDB Banyak Digunakan?", "id": "Digunakan Di Jepang Dan Amerika Selatan." },
  { "en": "Apa Itu TV Terestrial (Terrestrial Television)?", "id": "Siaran TV Menggunakan Pemancar Di Darat." },
  { "en": "Apa Keuntungan Utama Penyiaran TV Digital?", "id": "Kualitas Gambar Suara Lebih Baik, Efisiensi." },
  { "en": "Apa Itu Set-Top Box (STB)?", "id": "Perangkat Untuk Mengubah Sinyal Digital Ke." },
  { "en": "Apa Kepanjangan DAB Standar Radio Digital?", "id": "DAB Adalah Digital Audio Broadcasting." },
  { "en": "Apa Itu HD Radio Standar Radio Digital?", "id": "Standar Radio Digital Hibrida Dari iBiquity." },
  { "en": "Bagaimana Perilaku Resistor Pada Frekuensi Tinggi?", "id": "Memiliki Induktansi Dan Kapasitansi Parasitik Juga." },
  { "en": "Bagaimana Perilaku Kapasitor Pada Frekuensi Tinggi?", "id": "Bisa Berperilaku Seperti Induktor Di Atas." },
  { "en": "Bagaimana Perilaku Induktor Pada Frekuensi Tinggi?", "id": "Bisa Berperilaku Seperti Kapasitor Di Atas." },
  { "en": "Apa Itu Self-Resonant Frequency (SRF)?", "id": "Frekuensi Dimana Komponen Mencapai Resonansi Sendiri." },
  { "en": "Apa Itu Dioda Varactor (Varicap Diode)?", "id": "Dioda Yang Kapasitansinya Bervariasi Dengan Tegangan." },
  { "en": "Apa Fungsi Dioda PIN Dalam Sirkuit RF?", "id": "Digunakan Sebagai Saklar Atau Attenuator RF." },
  { "en": "Apa Itu Dioda Schottky Dalam Sirkuit RF?", "id": "Memiliki Waktu Switching Cepat, Tegangan Rendah." },
  { "en": "Apa Fungsi Dioda Gunn Dalam Frekuensi Mikro?", "id": "Digunakan Sebagai Osilator Pada Frekuensi Mikro." },
  { "en": "Apa Kepanjangan HEMT Transistor Frekuensi Tinggi?", "id": "HEMT Adalah High-Electron-Mobility Transistor." },
  { "en": "Apa Itu Amplifier Derau Rendah (Low-Noise Amplifier)?", "id": "Amplifier Tahap Pertama Pada Penerima Radio." },
  { "en": "Apa Fungsi Utama Dari Sebuah LNA?", "id": "Menguatkan Sinyal Lemah Tanpa Menambah Noise." },
  { "en": "Apa Itu Amplifier Daya (Power Amplifier)?", "id": "Amplifier Tahap Terakhir Pada Sebuah Pemancar." },
  { "en": "Apa Fungsi Utama Dari Sebuah PA?", "id": "Menguatkan Sinyal Hingga Daya Cukup Dipancarkan." },
  { "en": "Apa Itu Osilator Dalam Elektronika Komunikasi?", "id": "Sirkuit Yang Menghasilkan Sinyal Periodik Stabil." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Frekuensi Osilasi Dikontrol Oleh Tegangan Input." },
  { "en": "Apa Itu Isolator Dalam Sirkuit Frekuensi Mikro?", "id": "Perangkat Pasif Memungkinkan Sinyal Lewat Satu." },
  { "en": "Apa Itu Sirkulator Dalam Sirkuit Frekuensi Mikro?", "id": "Perangkat Tiga Port Mengarahkan Sinyal Sirkular." },
  { "en": "Apa Itu Directional Coupler Dalam Sirkuit RF?", "id": "Mencuplik Sebagian Kecil Daya Sinyal RF." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter Kompak Dengan Selektivitas Sangat Tinggi." },
  { "en": "Apa Itu Duplexer Dalam Sistem Komunikasi Radio?", "id": "Memungkinkan Pemancar Penerima Berbagi Satu Antena." },
  { "en": "Apa Itu Diplexer Dalam Sistem Komunikasi Radio?", "id": "Menggabungkan Atau Memisahkan Sinyal Frekuensi Berbeda." },
  { "en": "Apa Itu Balanced-to-Unbalanced Transformer (Balun)?", "id": "Menghubungkan Saluran Transmisi Seimbang Tidak Seimbang." },
  { "en": "Apa Itu Saluran Transmisi Seimbang (Balanced)?", "id": "Dua Konduktor Membawa Sinyal Sama Berlawanan." },
  { "en": "Apa Itu Saluran Transmisi Tidak Seimbang (Unbalanced)?", "id": "Satu Konduktor Bawa Sinyal, Satu Ground." },
  { "en": "Contoh Saluran Transmisi Seimbang Yang Umum?", "id": "Kabel Twisted Pair Adalah Saluran Seimbang." },
  { "en": "Contoh Saluran Transmisi Tidak Seimbang Umum?", "id": "Kabel Koaksial Adalah Saluran Tidak Seimbang." },
  { "en": "Apa Itu Microstrip Line Saluran Transmisi?", "id": "Saluran Transmisi Planar Di Atas Papan." },
  { "en": "Apa Itu Stripline Saluran Transmisi Planar?", "id": "Konduktor Diapit Antara Dua Lapisan Ground." },
  { "en": "Apa Itu Coplanar Waveguide (CPW)?", "id": "Konduktor Pusat Dikelilingi Dua Ground Coplanar." },
  { "en": "Apa Itu Permittivity Dalam Material Dielektrik?", "id": "Ukuran Kemampuan Material Menyimpan Energi Listrik." },
  { "en": "Apa Itu Permeability Dalam Material Magnetik?", "id": "Ukuran Kemampuan Material Mendukung Medan Magnet." },
  { "en": "Apa Itu Loss Tangent (Tan δ)?", "id": "Ukuran Kehilangan Energi Dalam Material Dielektrik." },
  { "en": "Apa Itu Quality Factor (Q Factor)?", "id": "Ukuran Kualitas Resonator Atau Komponen Reaktif." },
  { "en": "Apa Arti Dari Nilai Q Factor Tinggi?", "id": "Kehilangan Energi Rendah, Osilasi Sangat Stabil." },
  { "en": "Apa Itu White Noise (Derau Putih)?", "id": "Derau Dengan Kerapatan Spektral Daya Datar." },
  { "en": "Apa Itu Pink Noise (Derau Merah Jambu)?", "id": "Kerapatan Spektral Daya Turun 3 dB." },
  { "en": "Apa Itu Thermal Noise (Johnson-Nyquist Noise)?", "id": "Derau Akibat Agitasi Termal Elektron Konduktor." },
  { "en": "Apa Itu Shot Noise Dalam Perangkat Elektronik?", "id": "Derau Akibat Sifat Diskret Aliran Elektron." },
  { "en": "Apa Itu Flicker Noise (1/f Noise)?", "id": "Derau Yang Dominan Pada Frekuensi Sangat." },
  { "en": "Apa Itu Channel Capacity Dalam Teori Informasi?", "id": "Laju Data Maksimum Teoretis Melalui Kanal." },
  { "en": "Apa Itu Water-Filling Algorithm Dalam Komunikasi MIMO?", "id": "Algoritma Alokasi Daya Optimal Subkanal Berbeda." },
  { "en": "Apa Itu Go-Back-N ARQ Protokol?", "id": "Protokol Mengirim Ulang Paket Error Semua." },
  { "en": "Apa Itu Selective Repeat ARQ Protokol?", "id": "Protokol Hanya Mengirim Ulang Paket Yang." },
  { "en": "Apa Itu Slotted ALOHA Protokol Akses Media?", "id": "Pengguna Mengirim Data Hanya Awal Slot." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Penyediaan Layanan Komputasi Melalui Jaringan Internet." },
  { "en": "Apa Kepanjangan IaaS Dalam Layanan Komputasi Awan?", "id": "IaaS Adalah Infrastructure as a Service." },
  { "en": "Layanan Apa Yang Disediakan Oleh Model IaaS?", "id": "Menyediakan Infrastruktur Komputasi Virtual Seperti Server." },
  { "en": "Apa Kepanjangan PaaS Dalam Layanan Komputasi Awan?", "id": "PaaS Adalah Platform as a Service." },
  { "en": "Layanan Apa Yang Disediakan Oleh Model PaaS?", "id": "Menyediakan Platform Untuk Mengembangkan Menyebarkan Aplikasi." },
  { "en": "Apa Kepanjangan SaaS Dalam Layanan Komputasi Awan?", "id": "SaaS Adalah Software as a Service." },
  { "en": "Layanan Apa Yang Disediakan Oleh Model SaaS?", "id": "Menyediakan Aplikasi Perangkat Lunak Siap Pakai." },
  { "en": "Apa Itu Virtualisasi Dalam Dunia Komputasi?", "id": "Membuat Versi Virtual Dari Sesuatu Fisik." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine)?", "id": "Emulasi Sistem Komputer Lengkap Diatas Host." },
  { "en": "Apa Itu Hypervisor Dalam Teknologi Virtualisasi?", "id": "Perangkat Lunak Yang Membuat Menjalankan Mesin." },
  { "en": "Apa Itu Container Dalam Teknologi Virtualisasi?", "id": "Metode Virtualisasi Tingkat Sistem Operasi Ringan." },
  { "en": "Apa Perbedaan Utama Antara VM Dan Container?", "id": "Container Berbagi Kernel OS, Lebih Ringan." },
  { "en": "Apa Kepanjangan JPEG Standar Kompresi Gambar?", "id": "JPEG Adalah Joint Photographic Experts Group." },
  { "en": "Apa Jenis Kompresi Yang Digunakan Oleh JPEG?", "id": "Menggunakan Metode Kompresi Lossy (Ada Kehilangan)." },
  { "en": "Apa Kepanjangan MPEG Standar Kompresi Video?", "id": "MPEG Adalah Moving Picture Experts Group." },
  { "en": "Sebutkan Contoh Standar Kompresi Video MPEG?", "id": "MPEG-2 (DVD), MPEG-4 (Internet Streaming)." },
  { "en": "Apa Itu Standar Kompresi Video H.264?", "id": "Dikenal Juga Sebagai AVC (Advanced Video Coding)." },
  { "en": "Apa Itu Standar Kompresi Video H.265?", "id": "Dikenal Juga Sebagai HEVC (High Efficiency Video)." },
  { "en": "Mana Yang Lebih Efisien, H.264 Atau H.265?", "id": "H.265 Sekitar Dua Kali Lebih Efisien." },
  { "en": "Apa Itu Kodek (Codec) Dalam Multimedia?", "id": "Perangkat Keras Atau Lunak Mengkompres Mendekompres." },
  { "en": "Apa Frekuensi Sampling Standar Untuk Audio CD?", "id": "Standar Frekuensi Sampling Adalah 44.1 kHz." },
  { "en": "Apa Itu Format Kontainer Multimedia (Container Format)?", "id": "Berkas Yang Menyimpan Audio, Video, Subtitle." },
  { "en": "Sebutkan Contoh Format Kontainer Yang Populer?", "id": "MP4, MKV (Matroska), Dan Juga AVI." },
  { "en": "Apa Kepanjangan GNSS Untuk Sistem Navigasi?", "id": "GNSS Adalah Global Navigation Satellite System." },
  { "en": "Apa Kepanjangan GPS Sistem Navigasi Amerika Serikat?", "id": "GPS Adalah Global Positioning System." },
  { "en": "Sistem Navigasi Satelit Apa Milik Negara Rusia?", "id": "Rusia Mengoperasikan Sistem Navigasi Satelit GLONASS." },
  { "en": "Sistem Navigasi Satelit Apa Milik Uni Eropa?", "id": "Uni Eropa Mengembangkan Sistem Navigasi Galileo." },
  { "en": "Sistem Navigasi Satelit Apa Milik Negara Tiongkok?", "id": "Tiongkok Mengoperasikan Sistem Navigasi Satelit BeiDou." },
  { "en": "Bagaimana Prinsip Dasar Penentuan Posisi GNSS?", "id": "Menggunakan Prinsip Trilaterasi Dari Sinyal Satelit." },
  { "en": "Berapa Jumlah Minimum Satelit Untuk Posisi 3D?", "id": "Dibutuhkan Minimal Empat Sinyal Satelit Terlihat." },
  { "en": "Informasi Apa Yang Dipancarkan Oleh Satelit GPS?", "id": "Waktu Tepat, Posisi Satelit (Data Ephemeris)." },
  { "en": "Apa Itu Data Almanak Dalam Sinyal GPS?", "id": "Informasi Status Kasar Semua Satelit Konstelasi." },
  { "en": "Sebutkan Sumber Error Dalam Penentuan Posisi GPS?", "id": "Delay Ionosfer, Troposfer, Error Jam Satelit." },
  { "en": "Apa Itu Differential GPS (DGPS)?", "id": "Teknik Meningkatkan Akurasi GPS Menggunakan Stasiun." },
  { "en": "Apa Itu Assisted GPS (A-GPS)?", "id": "Menggunakan Bantuan Jaringan Seluler Mempercepat Pencarian." },
  { "en": "Apa Itu Four-Wave Mixing (FWM) Optik?", "id": "Efek Non-Linear Dimana Frekuensi Baru Tercipta." },
  { "en": "Apa Dampak Negatif FWM Dalam Sistem WDM?", "id": "Dapat Menyebabkan Crosstalk Antar Kanal Berdekatan." },
  { "en": "Apa Kepanjangan SOA Penguat Optik Semikonduktor?", "id": "SOA Adalah Semiconductor Optical Amplifier." },
  { "en": "Apa Keuntungan SOA Dibandingkan Dengan Amplifier EDFA?", "id": "Ukuran Lebih Kecil, Dapat Diintegrasikan Mudah." },
  { "en": "Apa Itu Jaringan Optik Elastis (Elastic Optical)?", "id": "Jaringan Mengalokasikan Spektrum Optik Secara Fleksibel." },
  { "en": "Apa Itu Carrier-Sense Multiple Access (CSMA)?", "id": "Protokol Mendengarkan Media Sebelum Mengirim Data." },
  { "en": "Apa Kepanjangan CSMA/CD Protokol Akses Media?", "id": "CSMA with Collision Detection." },
  { "en": "Bagaimana Cara Kerja Mekanisme Collision Detection?", "id": "Berhenti Mengirim Ketika Terdeteksi Adanya Tabrakan." },
  { "en": "Di Jaringan Mana CSMA/CD Digunakan Dahulu?", "id": "Digunakan Pada Jaringan Ethernet Lama (Half-Duplex)." },
  { "en": "Apa Kepanjangan CSMA/CA Protokol Akses Media?", "id": "CSMA with Collision Avoidance." },
  { "en": "Bagaimana Cara Kerja Mekanisme Collision Avoidance?", "id": "Menggunakan Mekanisme RTS/CTS Untuk Hindari Tabrakan." },
  { "en": "Di Jaringan Mana CSMA/CA Umumnya Digunakan?", "id": "Digunakan Pada Jaringan Nirkabel Wi-Fi." },
  { "en": "Apa Itu Token Ring Dalam Topologi Jaringan?", "id": "Topologi Jaringan Dimana Token Mengontrol Akses." },
  { "en": "Apa Itu Circuit Breaker Dalam Sistem Listrik?", "id": "Saklar Otomatis Pelindung Sirkuit Dari Beban." },
  { "en": "Apa Itu Grounding Atau Pembumian Dalam Elektronika?", "id": "Titik Referensi Tegangan Nol Dalam Sirkuit." },
  { "en": "Apa Tujuan Utama Dari Sistem Grounding?", "id": "Untuk Keselamatan Dan Stabilitas Sinyal Listrik." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Amplifier Menolak Sinyal Common-Mode." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Sirkuit Menolak Noise Dari Catu." },
  { "en": "Apa Itu Slew Rate Dari Sebuah Amplifier?", "id": "Laju Perubahan Maksimum Tegangan Output Amplifier." },
  { "en": "Apa Itu Gain-Bandwidth Product (GBW)?", "id": "Hasil Kali Antara Gain Dan Bandwidth." },
  { "en": "Apa Itu Shannon Limit Dalam Teori Informasi?", "id": "Batas Atas Teoritis Laju Data Efisien." },
  { "en": "Apa Itu Polaritas Dalam Konteks Kelistrikan?", "id": "Menunjukkan Arah Aliran Arus (Positif/Negatif)." },
  { "en": "Apa Itu Kode BCD (Binary-Coded Decimal)?", "id": "Setiap Digit Desimal Diwakili Empat Bit." },
  { "en": "Apa Itu American Standard Code for Information Interchange?", "id": "Standar Pengkodean Karakter Berbasis Alfabet Latin." },
  { "en": "Apa Itu Unicode Dalam Pengkodean Karakter?", "id": "Standar Pengkodean Karakter Global Mencakup Semua." },
  { "en": "Apa Perbedaan Antara Serial Dan Paralel Transmisi?", "id": "Serial Mengirim Bit Satu-persatu, Paralel Sekaligus." },
  { "en": "Mana Yang Lebih Cepat, Serial Atau Paralel?", "id": "Transmisi Paralel Secara Teori Lebih Cepat." },
  { "en": "Apa Itu Universal Synchronous/Asynchronous Receiver/Transmitter (USART)?", "id": "Perangkat Keras Untuk Komunikasi Serial Fleksibel." },
  { "en": "Apa Itu Open-Source Software (Perangkat Lunak Terbuka)?", "id": "Perangkat Lunak Dengan Kode Sumber Terbuka." },
  { "en": "Apa Itu Firmware Dalam Sistem Elektronik?", "id": "Perangkat Lunak Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu Central Processing Unit (CPU)?", "id": "Otak Komputer Yang Menjalankan Instruksi Program." },
  { "en": "Apa Itu Random-Access Memory (RAM)?", "id": "Memori Komputer Volatil Untuk Penyimpanan Sementara." },
  { "en": "Apa Itu Read-Only Memory (ROM)?", "id": "Memori Non-Volatil Berisi Data Tidak Berubah." },
  { "en": "Apa Itu Cache Memory Dalam Sistem Komputer?", "id": "Memori Kecil Cepat Menyimpan Data Sering." },
  { "en": "Apa Itu Bus Dalam Arsitektur Komputer?", "id": "Jalur Komunikasi Yang Menghubungkan Komponen Komputer." },
  { "en": "Apa Itu Laten Dalam Konteks Memori?", "id": "Waktu Tunda Sebelum Transfer Data Dimulai." },
  { "en": "Apa Itu Throughput Dalam Kinerja Sistem?", "id": "Jumlah Pekerjaan Yang Selesai Per Satuan." },
  { "en": "Apa Itu Scalability Atau Skalabilitas Sistem?", "id": "Kemampuan Sistem Menangani Peningkatan Beban Kerja." },
  { "en": "Apa Itu Failover Dalam Sistem Ketersediaan Tinggi?", "id": "Proses Pengalihan Otomatis Ke Sistem Cadangan." },
  { "en": "Apa Itu Hot Swapping Dalam Perangkat Keras?", "id": "Mengganti Komponen Tanpa Mematikan Sistem Utama." },
  { "en": "Apa Itu Command Line Interface (CLI)?", "id": "Antarmuka Pengguna Berbasis Teks Untuk Perintah." },
  { "en": "Apa Itu Graphical User Interface (GUI)?", "id": "Antarmuka Pengguna Berbasis Grafis Dan Ikon." },
  { "en": "Apa Itu Application Specific Integrated Circuit (ASIC)?", "id": "Sirkuit Terpadu Didesain Untuk Tujuan Khusus." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "Sirkuit Terpadu Dapat Diprogram Ulang Pengguna." },
  { "en": "Apa Perbedaan Antara ASIC Dan FPGA?", "id": "ASIC Tetap, FPGA Fleksibel Tapi Kurang." },
  { "en": "Apa Itu Very Large-Scale Integration (VLSI)?", "id": "Proses Menggabungkan Jutaan Transistor Dalam Chip." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Dengan Batas Waktu Ketat." },
  { "en": "Dimana RTOS Umumnya Banyak Sekali Digunakan?", "id": "Sistem Tertanam, Robotika, Dan Kontrol Industri." },
  { "en": "Apa Itu Watchdog Timer Dalam Sistem Tertanam?", "id": "Timer Pencegah Sistem Macet (Hang) Bekerja." },
  { "en": "Apa Itu Cross-Compiler Dalam Pengembangan Perangkat Lunak?", "id": "Compiler Berjalan Di Satu Platform, Menghasilkan." },
  { "en": "Apa Itu Debugging Dalam Proses Pemrograman?", "id": "Proses Menemukan Dan Memperbaiki Bug Kode." },
  { "en": "Apa Itu Linker Dalam Proses Kompilasi?", "id": "Program Menggabungkan File Objek Menjadi Eksekutabel." },
  { "en": "Apa Itu Library Dalam Pemrograman Komputer?", "id": "Kumpulan Kode Pra-Tulis Untuk Digunakan Kembali." },
  { "en": "Apa Itu Static Linking Dalam Pemrograman?", "id": "Menyalin Kode Library Ke Dalam Program." },
  { "en": "Apa Itu Dynamic Linking Dalam Pemrograman?", "id": "Program Memuat Kode Library Saat Dijalankan." },
  { "en": "Apa Itu Quality of Experience (QoE)?", "id": "Ukuran Kepuasan Subjektif Pengguna Terhadap Layanan." },
  { "en": "Apa Perbedaan Antara QoS Dan QoE?", "id": "QoS Teknis Jaringan, QoE Persepsi Pengguna." },
  { "en": "Apa Itu Network Neutrality (Netralitas Jaringan)?", "id": "Prinsip Penyedia Internet Memperlakukan Semua Data." },
  { "en": "Apa Itu Big Data Dalam Dunia Teknologi?", "id": "Kumpulan Data Sangat Besar Kompleks Dianalisis." },
  { "en": "Apa Itu Machine Learning (Pembelajaran Mesin)?", "id": "Cabang AI Dimana Komputer Belajar Data." },
  { "en": "Apa Kepanjangan SGSN (Serving GPRS Support Node)?", "id": "SGSN Adalah Serving GPRS Support Node." },
  { "en": "Apa Fungsi Utama Dari Perangkat SGSN?", "id": "Mengelola Sesi Data Mobilitas Pengguna GPRS." },
  { "en": "Apa Kepanjangan GGSN (Gateway GPRS Support Node)?", "id": "GGSN Adalah Gateway GPRS Support Node." },
  { "en": "Apa Fungsi Utama Dari Perangkat GGSN?", "id": "Gerbang Antara Jaringan GPRS (General Packet Radio Service) Internet." },
  { "en": "Apa Kepanjangan UMTS (Universal Mobile Telecommunications System)?", "id": "UMTS Adalah Universal Mobile Telecommunications System." },
  { "en": "Apa Jaringan Akses Radio Pada UMTS?", "id": "UTRAN (UMTS Terrestrial Radio Access Network)." },
  { "en": "Apa Kepanjangan RNC (Radio Network Controller) UTRAN?", "id": "RNC Adalah Singkatan Radio Network Controller." },
  { "en": "Apa Fungsi Utama Radio Network Controller (RNC)?", "id": "Mengontrol Beberapa Node B Dalam Jaringan." },
  { "en": "Apa Itu Node B Dalam Jaringan UMTS?", "id": "Setara Dengan BTS (Base Transceiver Station)." },
  { "en": "Apa Kepanjangan LTE (Long Term Evolution)?", "id": "LTE Adalah Singkatan Long Term Evolution." },
  { "en": "Apa Jaringan Akses Radio Pada Sistem LTE?", "id": "E-UTRAN (Evolved UMTS Terrestrial Radio Access)." },
  { "en": "Apa Jaringan Inti Pada Arsitektur LTE?", "id": "EPC (Evolved Packet Core) Adalah Jaringannya." },
  { "en": "Apa Kepanjangan eNodeB (Evolved Node B)?", "id": "eNodeB Adalah Singkatan Evolved Node B." },
  { "en": "Apa Fungsi Dari eNodeB Dalam Jaringan LTE?", "id": "Menggabungkan Fungsi RNC Dan Juga Node B." },
  { "en": "Apa Kepanjangan MME (Mobility Management Entity)?", "id": "MME Adalah Mobility Management Entity." },
  { "en": "Apa Fungsi Utama Dari Perangkat Keras MME?", "id": "Mengelola Mobilitas Dan Sesi Pengguna LTE." },
  { "en": "Apa Itu Serving Gateway (S-GW) EPC?", "id": "Merutekan Meneruskan Paket Data Milik Pengguna." },
  { "en": "Apa Itu Packet Data Network Gateway (P-GW)?", "id": "Menghubungkan Jaringan EPC Dengan Jaringan Eksternal." },
  { "en": "Apa Itu HSS (Home Subscriber Server)?", "id": "Database Gabungan HLR (Home Location Register) AuC." },
  { "en": "Apa Itu VoLTE (Voice over LTE)?", "id": "Layanan Suara Dikirim Melalui Jaringan LTE." },
  { "en": "Siapa Perumus Empat Persamaan Medan Elektromagnetik?", "id": "James Clerk Maxwell Merumuskan Persamaan Tersebut." },
  { "en": "Apa Itu Gelombang Bidang (Plane Wave)?", "id": "Model Gelombang Elektromagnetik Ideal Medan Seragam." },
  { "en": "Apa Itu Impedansi Intrinsik Suatu Media?", "id": "Rasio Amplitudo Medan Listrik Terhadap Magnetik." },
  { "en": "Berapa Nilai Impedansi Intrinsik Ruang Hampa?", "id": "Sekitar 377 Ohm Atau 120π Ohm." },
  { "en": "Apa Itu Vektor Poynting Teori Elektromagnetik?", "id": "Representasi Kerapatan Daya Arah Aliran Energi." },
  { "en": "Apa Itu Antena Array (Larik Antena)?", "id": "Sekelompok Elemen Antena Bekerja Secara Bersama." },
  { "en": "Apa Tujuan Membuat Susunan Antena Array?", "id": "Meningkatkan Gain, Mengarahkan Pola Radiasi Antena." },
  { "en": "Apa Itu Phased Array Antenna (Antena Larik)?", "id": "Antena Array Dengan Arah Beam Diprogram." },
  { "en": "Bagaimana Mengubah Arah Beam Phased Array?", "id": "Dengan Mengubah Fasa Sinyal Setiap Elemen." },
  { "en": "Apa Itu Balun (Balanced-to-Unbalanced Transformer)?", "id": "Transformator Impedansi Antara Saluran Seimbang Seimbang." },
  { "en": "Apa Itu MANET (Mobile Ad Hoc Network)?", "id": "Jaringan Ad Hoc Nirkabel Perangkat Bergerak." },
  { "en": "Apa Karakteristik Utama Dari Jaringan MANET?", "id": "Tanpa Infrastruktur, Topologi Dinamis, Perangkat Bergerak." },
  { "en": "Apa Itu VANET (Vehicular Ad Hoc Network)?", "id": "Jenis MANET Khusus Untuk Komunikasi Kendaraan." },
  { "en": "Sebutkan Aplikasi Utama Dari Jaringan VANET?", "id": "Peningkatan Keselamatan Lalu Lintas, Manajemen Trafik." },
  { "en": "Apa Itu Protokol Routing Proaktif Ad Hoc?", "id": "Memelihara Rute Ke Semua Tujuan Setiap." },
  { "en": "Apa Itu Protokol Routing Reaktif Ad Hoc?", "id": "Mencari Rute Hanya Ketika Diperlukan Saja." },
  { "en": "Apakah DSDV (Destination-Sequenced Distance-Vector) Proaktif Reaktif?", "id": "DSDV Adalah Contoh Protokol Routing Proaktif." },
  { "en": "Apa Kepanjangan AODV (Ad Hoc On-Demand Distance)?", "id": "AODV Adalah Ad Hoc On-Demand Distance." },
  { "en": "Apakah AODV Termasuk Proaktif Atau Reaktif?", "id": "AODV Adalah Contoh Protokol Routing Reaktif." },
  { "en": "Apa Kepanjangan DSR (Dynamic Source Routing)?", "id": "DSR Adalah Dynamic Source Routing." },
  { "en": "Apa Itu Fungsi Hash Dalam Kriptografi?", "id": "Memetakan Data Ukuran Variabel Ke Ukuran." },
  { "en": "Sebutkan Sifat Penting Fungsi Hash Kriptografis?", "id": "Pre-image Resistance, Second Pre-image, Collision Resistance." },
  { "en": "Apa Itu Message Digest Fungsi Hash?", "id": "Nama Lain Untuk Nilai Output Hash." },
  { "en": "Sebutkan Contoh Algoritma Hash Yang Populer?", "id": "SHA-256 (Secure Hash Algorithm 256-bit)." },
  { "en": "Apa Kepanjangan MAC (Message Authentication Code)?", "id": "MAC Adalah Message Authentication Code." },
  { "en": "Apa Fungsi Utama Kode Otentikasi Pesan?", "id": "Memverifikasi Integritas Dan Otentisitas Suatu Pesan." },
  { "en": "Apa Itu HMAC (Hash-based MAC)?", "id": "Jenis MAC Dihitung Menggunakan Fungsi Hash." },
  { "en": "Apa Itu Sertifikat Digital (Digital Certificate)?", "id": "Berkas Elektronik Mengikat Kunci Publik Identitas." },
  { "en": "Siapa Yang Menerbitkan Sertifikat Digital Tersebut?", "id": "Diterbitkan Oleh CA (Certificate Authority)." },
  { "en": "Apa Kepanjangan PKI (Public Key Infrastructure)?", "id": "PKI Adalah Public Key Infrastructure." },
  { "en": "Apa Itu Proses Acak (Random Process)?", "id": "Kumpulan Sinyal Acak Yang Diindeks Waktu." },
  { "en": "Apa Itu Sinyal Stasioner (Stationary Signal)?", "id": "Proses Acak Statistiknya Tidak Berubah Waktu." },
  { "en": "Apa Itu Proses Ergodik (Ergodic Process)?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
  { "en": "Apa Itu Fungsi Autokorelasi (Autocorrelation Function)?", "id": "Mengukur Kemiripan Sinyal Dengan Versi Tundanya." },
  { "en": "Apa Itu Fungsi Korelasi Silang (Cross-correlation)?", "id": "Mengukur Kemiripan Antara Dua Sinyal Berbeda." },
  { "en": "Apa Itu Power Spectral Density (PSD)?", "id": "Distribusi Daya Sinyal Pada Berbagai Frekuensi." },
  { "en": "Apa Itu Teorema Wiener-Khinchin Dalam Teori?", "id": "Menghubungkan Autokorelasi Dengan Kerapatan Spektral Daya." },
  { "en": "Apa Itu Matched Filter Pemrosesan Sinyal?", "id": "Filter Optimal Maksimalkan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu SDR (Software-Defined Radio)?", "id": "Sistem Radio Komponennya Diimplementasikan Perangkat Lunak." },
  { "en": "Apa Keuntungan Utama Dari Teknologi SDR?", "id": "Sangat Fleksibel Dan Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Itu NFV (Network Function Virtualization)?", "id": "Virtualisasi Fungsi Jaringan Menjadi Perangkat Lunak." },
  { "en": "Apa Itu Data Mining (Penambangan Data)?", "id": "Proses Menemukan Pola Berguna Dalam Data." },
  { "en": "Apa Itu Deep Learning (Pembelajaran Mendalam)?", "id": "Sub-bidang ML (Machine Learning) Menggunakan ANN." },
  { "en": "Apa Itu ANN (Artificial Neural Network)?", "id": "Model Komputasi Terinspirasi Dari Otak Biologis." },
  { "en": "Apa Itu Bluetooth Piconet Komunikasi Nirkabel?", "id": "Jaringan Kecil Satu Master Tujuh Slave." },
  { "en": "Apa Itu Bluetooth Scatternet Komunikasi Nirkabel?", "id": "Gabungan Beberapa Jaringan Piconet Yang Terhubung." },
  { "en": "Apa Itu WiMAX (Worldwide Interoperability for Microwave)?", "id": "Standar Komunikasi Nirkabel Untuk Akses Broadband." },
  { "en": "Standar IEEE Apa Yang Digunakan WiMAX?", "id": "Menggunakan Standar IEEE (Institute of Electrical and Electronics Engineers) 802.16." },
  { "en": "Apa Itu PAN (Personal Area Network)?", "id": "Jaringan Komputer Untuk Komunikasi Antar Perangkat." },
  { "en": "Apa Itu MAN (Metropolitan Area Network)?", "id": "Jaringan Komputer Mencakup Area Sebuah Kota." },
  { "en": "Apa Itu ICMP (Internet Control Message Protocol)?", "id": "Digunakan Untuk Mengirim Pesan Error Operasional." },
  { "en": "Utilitas Apa Yang Menggunakan Protokol ICMP?", "id": "Perintah Ping Dan Traceroute Menggunakan ICMP." },
  { "en": "Apa Itu TTL (Time to Live)?", "id": "Mekanisme Membatasi Umur Paket Di Jaringan." },
  { "en": "Apa Fungsi Dari Perintah Jaringan Ping?", "id": "Menguji Jangkauan Dan Latensi Ke Host." },
  { "en": "Apa Fungsi Dari Perintah Jaringan Traceroute?", "id": "Menampilkan Rute Yang Dilewati Paket Ke." },
  { "en": "Apa Itu Port Scanning Keamanan Jaringan?", "id": "Proses Memindai Port Terbuka Suatu Host." },
  { "en": "Apa Itu IDS (Intrusion Detection System)?", "id": "Sistem Memantau Jaringan Mencari Aktivitas Mencurigakan." },
  { "en": "Apa Itu IPS (Intrusion Prevention System)?", "id": "Sistem IDS Yang Juga Dapat Memblokir." },
  { "en": "Apa Itu Honeypot Keamanan Jaringan Siber?", "id": "Sistem Umpan Untuk Menarik Perhatian Penyerang." },
  { "en": "Apa Itu DMZ (Demilitarized Zone)?", "id": "Sub-jaringan Antara Jaringan Internal Dan Eksternal." },
  { "en": "Server Apa Biasanya Ditempatkan Di DMZ?", "id": "Server Web, Email, DNS (Domain Name System), FTP." },
  { "en": "Apa Itu SMF (Single-Mode Fiber)?", "id": "Serat Optik Dengan Inti Sangat Kecil." },
  { "en": "Apa Itu MMF (Multi-Mode Fiber)?", "id": "Serat Optik Dengan Inti Lebih Besar." },
  { "en": "Apa Itu Dispersi Modal Serat Optik?", "id": "Penyebaran Pulsa Akibat Mode Berbeda Merambat." },
  { "en": "Di Jenis Serat Mana Dispersi Modal Terjadi?", "id": "Hanya Terjadi Pada Serat Optik MMF." },
  { "en": "Apa Itu NA (Numerical Aperture)?", "id": "Ukuran Kemampuan Serat Optik Menangkap Cahaya." },
  { "en": "Apa Itu Optical Circulator (Sirkulator Optik)?", "id": "Komponen Optik Mengarahkan Cahaya Dari Port." },
  { "en": "Apa Itu Isolator Faraday (Faraday Rotator)?", "id": "Komponen Optik Memutar Polarisasi Medan Magnet." },
  { "en": "Apa Itu FBG (Fiber Bragg Grating)?", "id": "Jenis Reflektor Terdistribusi Dalam Serat Optik." },
  { "en": "Apa Aplikasi Utama Fiber Bragg Grating?", "id": "Sebagai Filter Optik Dan Sensor Suhu." },
  { "en": "Apa Itu Zero-Dispersion Wavelength Serat Optik?", "id": "Panjang Gelombang Dimana Dispersi Kromatik Nol." },
  { "en": "Apa Itu DSF (Dispersion-Shifted Fiber)?", "id": "Serat Dirancang Memindahkan Zero-Dispersion Ke 1550." },
  { "en": "Apa Itu NZDSF (Non-Zero Dispersion-Shifted Fiber)?", "id": "Serat Dengan Sedikit Dispersi Di Jendela." },
  { "en": "Mengapa NZDSF Dikembangkan Untuk Sistem DWDM?", "id": "Untuk Menekan Efek FWM (Four-Wave Mixing)." },
  { "en": "Apa Arsitektur Jaringan Inti Pada Sistem 5G?", "id": "SBA (Service-Based Architecture) Adalah Arsitekturnya." },
  { "en": "Apa Konsep Utama Dari Arsitektur Berbasis Layanan?", "id": "Fungsi Jaringan Berinteraksi Sebagai Layanan (Service)." },
  { "en": "Apa Kepanjangan AMF (Access and Mobility Management Function)?", "id": "AMF Adalah Access and Mobility Management." },
  { "en": "Apa Kepanjangan SMF (Session Management Function)?", "id": "SMF Adalah Session Management Function." },
  { "en": "Apa Kepanjangan UPF (User Plane Function)?", "id": "UPF Adalah Singkatan User Plane Function." },
  { "en": "Apa Fungsi Utama Dari UPF Dalam 5G?", "id": "Menangani Lalu Lintas Data Milik Pengguna." },
  { "en": "Apa Kepanjangan NEF (Network Exposure Function)?", "id": "NEF Adalah Network Exposure Function." },
  { "en": "Apa Fungsi Utama Dari NEF Dalam 5G?", "id": "Mengekspos Kemampuan Jaringan Ke Pihak Ketiga." },
  { "en": "Apa Kepanjangan NRF (Network Repository Function)?", "id": "NRF Adalah Network Repository Function." },
  { "en": "Apa Itu eMBB (Enhanced Mobile Broadband)?", "id": "Kasus Penggunaan 5G Untuk Kecepatan Sangat." },
  { "en": "Apa Itu URLLC (Ultra-Reliable Low-Latency Communication)?", "id": "Kasus Penggunaan 5G Untuk Komunikasi Andal." },
  { "en": "Apa Itu mMTC (Massive Machine-Type Communications)?", "id": "Kasus Penggunaan 5G Untuk Jutaan Perangkat." },
  { "en": "Apa Itu New Radio (NR) Dalam 5G?", "id": "Standar Antarmuka Udara Baru Untuk Jaringan." },
  { "en": "Pita Frekuensi Apa Yang Digunakan Oleh 5G?", "id": "Sub-6 GHz Dan Gelombang Milimeter (mmWave)." },
  { "en": "Apa Keuntungan Menggunakan Frekuensi Gelombang Milimeter?", "id": "Menawarkan Bandwidth Sangat Besar, Kecepatan Tinggi." },
  { "en": "Apa Kerugian Menggunakan Frekuensi Gelombang Milimeter?", "id": "Jangkauan Pendek, Mudah Terhalang Benda Fisik." },
  { "en": "Apa Itu Phase Shifter (Penggeser Fasa) RF?", "id": "Komponen Mengubah Fasa Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Attenuator (Pelemah) RF Tetap?", "id": "Komponen Pasif Mengurangi Daya Sinyal RF." },
  { "en": "Apa Itu Variable Attenuator (Pelemah Variabel)?", "id": "Pelemah Dimana Tingkat Atenuasi Dapat Diatur." },
  { "en": "Apa Itu Power Divider (Pembagi Daya)?", "id": "Membagi Sinyal Input Menjadi Beberapa Output." },
  { "en": "Apa Itu Power Combiner (Penggabung Daya)?", "id": "Menggabungkan Beberapa Sinyal Input Menjadi Satu." },
  { "en": "Sebutkan Jenis Power Divider/Combiner Yang Umum?", "id": "Wilkinson Power Divider Dan Resistive Power." },
  { "en": "Apa Karakteristik Filter Butterworth Respon Frekuensi?", "id": "Respon Sangat Datar Pada Passband (Maximally)." },
  { "en": "Apa Karakteristik Filter Chebyshev Respon Frekuensi?", "id": "Memiliki Ripple Di Passband, Roll-off Tajam." },
  { "en": "Apa Karakteristik Filter Bessel Respon Fasa?", "id": "Memiliki Respon Fasa Linear, Tunda Grup." },
  { "en": "Filter Mana Yang Paling Baik Untuk Sinyal?", "id": "Filter Bessel Menjaga Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Filter Eliptik (Elliptic Filter)?", "id": "Memiliki Ripple Di Passband Dan Stopband." },
  { "en": "Apa Itu Resonator Dielektrik (Dielectric Resonator)?", "id": "Material Keramik Resonansi Pada Frekuensi Mikro." },
  { "en": "Apa Fungsi Dari Stasiun Bumi (Earth Station)?", "id": "Terminal Di Bumi Untuk Komunikasi Satelit." },
  { "en": "Apa Itu HPA (High-Power Amplifier) Stasiun Bumi?", "id": "Menguatkan Sinyal Sebelum Dipancarkan Ke Satelit." },
  { "en": "Apa Itu LNB (Low-Noise Block Downconverter)?", "id": "Menerima, Menguatkan, Menurunkan Frekuensi Sinyal Satelit." },
  { "en": "Apa Parameter Kinerja G/T Sebuah Penerima?", "id": "Rasio Gain Antena Terhadap Temperatur Noise." },
  { "en": "Apa Arti Nilai G/T Yang Tinggi?", "id": "Menunjukkan Kinerja Penerima Yang Sangat Baik." },
  { "en": "Apa Itu Pointing Error Dalam Antena Satelit?", "id": "Kesalahan Arah Antena Tidak Tepat Sasaran." },
  { "en": "Apa Itu Payload Pada Sebuah Satelit?", "id": "Peralatan Misi Utama Yang Dibawa Satelit." },
  { "en": "Sebutkan Contoh Payload Satelit Komunikasi?", "id": "Transponder, Antena, Dan Juga Peralatan Terkait." },
  { "en": "Apa Itu TT&C (Telemetry, Tracking, and Command)?", "id": "Sistem Untuk Memonitor Dan Mengontrol Satelit." },
  { "en": "Apa Itu Telemetry Dalam Sistem Kontrol Satelit?", "id": "Pengiriman Data Status Kesehatan Satelit Bumi." },
  { "en": "Apa Itu Sun Outage Dalam Komunikasi Satelit?", "id": "Gangguan Sinyal Saat Matahari Dibelakang Satelit." },
  { "en": "Apa Kepanjangan MQTT (Message Queuing Telemetry Transport)?", "id": "MQTT Adalah Message Queuing Telemetry Transport." },
  { "en": "Bagaimana Model Komunikasi Protokol Canggih MQTT?", "id": "Menggunakan Model Publish-Subscribe Dengan Seorang Broker." },
  { "en": "Apa Itu Broker Dalam Protokol Komunikasi MQTT?", "id": "Server Pusat Menerima Mengirim Pesan Antar." },
  { "en": "Apa Kepanjangan CoAP (Constrained Application Protocol)?", "id": "CoAP Adalah Constrained Application Protocol." },
  { "en": "Untuk Perangkat Apa Protokol CoAP Dirancang?", "id": "Dirancang Untuk Perangkat IoT (Internet of Things) Terbatas." },
  { "en": "Protokol Apa Yang Mirip Dengan CoAP?", "id": "Didasarkan Pada Model REST (Representational State Transfer) HTTP." },
  { "en": "Apa Itu Arsitektur Tiga Lapis IoT?", "id": "Perception (Sensor), Network (Jaringan), Application (Aplikasi)." },
  { "en": "Apa Itu Platform IoT (Internet of Things)?", "id": "Perangkat Lunak Mengelola Menghubungkan Perangkat IoT." },
  { "en": "Apa Itu Digital Twin (Kembaran Digital)?", "id": "Representasi Virtual Dari Objek Atau Sistem." },
  { "en": "Apa Manfaat Dari Teknologi Digital Twin?", "id": "Simulasi, Pemantauan, Dan Optimalisasi Aset Fisik." },
  { "en": "Apa Itu Edge Computing Dalam Arsitektur IoT?", "id": "Memproses Data Lebih Dekat Ke Sumbernya." },
  { "en": "Apa Itu Fog Computing Dalam Arsitektur IoT?", "id": "Lapisan Komputasi Antara Edge Dan Cloud." },
  { "en": "Apa Kepanjangan LPWAN (Low-Power Wide-Area Network)?", "id": "LPWAN Adalah Low-Power Wide-Area Network." },
  { "en": "Sebutkan Contoh Teknologi Jaringan LPWAN?", "id": "LoRaWAN, Sigfox, Dan Juga NB-IoT (Narrowband-IoT)." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area)?", "id": "Protokol LPWAN Berbasis Teknik Modulasi LoRa." },
  { "en": "Apa Itu Telemetry Dalam Manajemen Jaringan?", "id": "Pengumpulan Data Kinerja Jaringan Secara Real-time." },
  { "en": "Apa Kepanjangan ZTP (Zero-Touch Provisioning)?", "id": "ZTP Adalah Singkatan Zero-Touch Provisioning." },
  { "en": "Apa Konsep Utama Dari Zero-Touch Provisioning?", "id": "Konfigurasi Perangkat Jaringan Otomatis Tanpa Intervensi." },
  { "en": "Apa Itu Orkestrasi Jaringan (Network Orchestration)?", "id": "Otomatisasi Koordinasi Layanan Jaringan Secara Menyeluruh." },
  { "en": "Apa Itu Intent-Based Networking (IBN)?", "id": "Jaringan Mengelola Diri Sendiri Berdasarkan Tujuan." },
  { "en": "Apa Itu Data Plane Dalam Perangkat Jaringan?", "id": "Bagian Meneruskan Paket (Forwarding) Lalu Lintas." },
  { "en": "Apa Itu Control Plane Dalam Perangkat Jaringan?", "id": "Bagian Membuat Keputusan Routing (Berpikir) Jaringan." },
  { "en": "Apa Itu Management Plane Perangkat Jaringan?", "id": "Bagian Untuk Mengkonfigurasi Memantau Perangkat Jaringan." },
  { "en": "Apa Itu OpenFlow Dalam Jaringan SDN?", "id": "Protokol Komunikasi Antara Controller Dan Switch." },
  { "en": "Apa Itu Southbound API Dalam Arsitektur SDN?", "id": "API (Application Programming Interface) Antara Controller Switch." },
  { "en": "Apa Itu Northbound API Dalam Arsitektur SDN?", "id": "API Antara Controller Dan Lapisan Aplikasi." },
  { "en": "Apa Itu Scattering Dalam Propagasi Gelombang?", "id": "Penyebaran Gelombang Akibat Interaksi Objek Kecil." },
  { "en": "Apa Itu Rayleigh Scattering Dalam Atmosfer?", "id": "Hamburan Cahaya Oleh Partikel Lebih Kecil." },
  { "en": "Mengapa Langit Terlihat Berwarna Biru Siang Hari?", "id": "Karena Hamburan Rayleigh Lebih Efektif Cahaya." },
  { "en": "Apa Itu Mie Scattering Dalam Atmosfer?", "id": "Hamburan Cahaya Partikel Seukuran Panjang Gelombang." },
  { "en": "Mengapa Awan Terlihat Berwarna Putih Atau Abu-abu?", "id": "Karena Hamburan Mie Menghamburkan Semua Warna." },
  { "en": "Apa Itu Tropospheric Ducting Fenomena Propagasi?", "id": "Gelombang Radio Terperangkap Lapisan Atmosfer Troposfer." },
  { "en": "Apa Itu Faraday Rotation Efek Propagasi?", "id": "Rotasi Polarisasi Gelombang Melewati Media Termagnetisasi." },
  { "en": "Apa Itu Atmosferik Lensing Fenomena Propagasi?", "id": "Pembelokan Gelombang Radio Oleh Variasi Indeks." },
  { "en": "Apa Itu Spread Spectrum Clocking (SSC)?", "id": "Teknik Mengurangi EMI (Electromagnetic Interference) Dengan Menyebarkan." },
  { "en": "Apa Itu Signal Integrity (Integritas Sinyal)?", "id": "Ukuran Kualitas Sinyal Listrik Dalam Transmisi." },
  { "en": "Sebutkan Masalah Umum Dalam Signal Integrity?", "id": "Refleksi, Crosstalk, Dan Juga Atenuasi Sinyal." },
  { "en": "Apa Itu Termination Resistor Saluran Transmisi?", "id": "Resistor Di Ujung Saluran Mencegah Refleksi." },
  { "en": "Nilai Resistor Terminasi Harus Sama Dengan Apa?", "id": "Harus Sesuai Dengan Impedansi Karakteristik Saluran." },
  { "en": "Apa Itu Ground Bounce Dalam Sirkuit Digital?", "id": "Fluktuasi Tegangan Referensi Ground Akibat Switching." },
  { "en": "Apa Itu Decoupling Capacitor Dalam Sirkuit Elektronik?", "id": "Kapasitor Menstabilkan Tegangan Catu Daya Lokal." },
  { "en": "Apa Itu Larmor Frequency Dalam Fisika?", "id": "Frekuensi Presesi Momen Magnetik Di Medan." },
  { "en": "Apa Itu Backscatter Communication Dalam Teknologi?", "id": "Perangkat Berkomunikasi Dengan Memantulkan Sinyal Radio." },
  { "en": "Di Mana Teknologi Backscatter Umumnya Digunakan?", "id": "Pada Tag RFID (Radio-Frequency Identification) Pasif." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Pasif?", "id": "Tag Tanpa Baterai Mendapat Daya Sinyal." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Aktif?", "id": "Tag Dengan Baterai Sendiri, Jangkauan Jauh." },
  { "en": "Apa Itu Wireless Power Transfer (WPT)?", "id": "Transmisi Energi Listrik Tanpa Menggunakan Kabel." },
  { "en": "Apa Itu Inductive Coupling Dalam Teknologi WPT?", "id": "Transfer Energi Melalui Medan Magnet Antar." },
  { "en": "Apa Itu Quality Factor (Faktor Q) Resonator?", "id": "Ukuran Kualitas Resonator, Rasio Energi Disimpan." },
  { "en": "Apa Itu Spurious-Free Dynamic Range (SFDR)?", "id": "Rentang Antara Sinyal Fundamental Dan Spurious." },
  { "en": "Apa Itu Sinyal Spurious Dalam Analisis Spektrum?", "id": "Komponen Frekuensi Tidak Diinginkan Dihasilkan Sistem." },
  { "en": "Apa Itu Harmonics Dalam Sinyal Non-Linear?", "id": "Komponen Frekuensi Kelipatan Bulat Frekuensi Fundamental." },
  { "en": "Apa Itu Phase Jitter Dalam Sinyal Digital?", "id": "Penyimpangan Tepi Sinyal Dari Posisi Idealnya." },
  { "en": "Apa Kepanjangan SMTP (Simple Mail Transfer Protocol)?", "id": "SMTP Adalah Simple Mail Transfer Protocol." },
  { "en": "Apa Fungsi Utama Dari Protokol Canggih SMTP?", "id": "Untuk Mengirimkan Pesan Email Antar Server." },
  { "en": "Port Berapa Yang Digunakan Oleh Protokol SMTP?", "id": "Umumnya Menggunakan Port Nomor 25 Atau 587." },
  { "en": "Apa Kepanjangan POP3 (Post Office Protocol version 3)?", "id": "POP3 Adalah Post Office Protocol version 3." },
  { "en": "Apa Fungsi Utama Dari Protokol POP3?", "id": "Untuk Mengunduh Email Dari Server Ke Klien." },
  { "en": "Apa Karakteristik Utama Dari Cara Kerja POP3?", "id": "Email Biasanya Dihapus Dari Server Setelah Diunduh." },
  { "en": "Apa Kepanjangan IMAP (Internet Message Access Protocol)?", "id": "IMAP Adalah Internet Message Access Protocol." },
  { "en": "Apa Fungsi Utama Dari Protokol Canggih IMAP?", "id": "Mengakses Email Yang Tersimpan Di Server." },
  { "en": "Apa Keuntungan IMAP Dibandingkan Dengan Protokol POP3?", "id": "Email Tersinkronisasi Di Beberapa Perangkat Berbeda." },
  { "en": "Apa Itu Protokol Jaringan Telnet?", "id": "Protokol Untuk Akses Remote Terminal Berbasis Teks." },
  { "en": "Mengapa Telnet Dianggap Tidak Aman Untuk Digunakan?", "id": "Karena Semua Data Dikirim Tanpa Enkripsi." },
  { "en": "Apa Kepanjangan SSH (Secure Shell) Protokol Jaringan?", "id": "SSH Adalah Protokol Jaringan Secure Shell." },
  { "en": "Apa Keuntungan Utama SSH Dibandingkan Telnet?", "id": "Menyediakan Koneksi Remote Yang Aman Terenkripsi." },
  { "en": "Port Berapa Yang Digunakan Oleh Protokol SSH?", "id": "Protokol SSH Umumnya Menggunakan Port 22." },
  { "en": "Apa Itu TCM (Trellis Coded Modulation)?", "id": "Menggabungkan Pengkodean Konvolusi Dengan Proses Modulasi." },
  { "en": "Apa Tujuan Utama Dari Teknik Canggih TCM?", "id": "Meningkatkan Kinerja Error Tanpa Mengurangi Laju." },
  { "en": "Apa Itu Hard-Decision Decoding Dalam Suatu Sistem?", "id": "Penerima Membuat Keputusan Biner (0/1) Langsung." },
  { "en": "Apa Itu Soft-Decision Decoding Dalam Suatu Sistem?", "id": "Penerima Menggunakan Informasi Probabilitas Bit Diterima." },
  { "en": "Mana Yang Memberikan Performa Lebih Baik?", "id": "Soft-Decision Decoding Memberikan Performa Jauh Baik." },
  { "en": "Apa Itu CSI (Channel State Information)?", "id": "Informasi Karakteristik Kanal Propagasi Komunikasi Nirkabel." },
  { "en": "Bagaimana CSI Dapat Diperoleh Oleh Penerima?", "id": "Melalui Pengiriman Sinyal Pilot Atau Referensi." },
  { "en": "Mengapa CSI Penting Dalam Komunikasi Nirkabel?", "id": "Untuk Adaptasi Transmisi Dan Peningkatan Performa." },
  { "en": "Apa Kepanjangan SAR (Synthetic Aperture Radar)?", "id": "SAR Adalah Singkatan Synthetic Aperture Radar." },
  { "en": "Bagaimana Prinsip Kerja Dasar Dari SAR?", "id": "Menggunakan Gerakan Platform Untuk Membuat Apertur." },
  { "en": "Apa Keuntungan Utama Dari Teknologi SAR?", "id": "Menghasilkan Gambar Resolusi Tinggi, Tembus Awan." },
  { "en": "Apa Kepanjangan ISAR (Inverse Synthetic Aperture Radar)?", "id": "ISAR Adalah Inverse Synthetic Aperture Radar." },
  { "en": "Apa Perbedaan Utama Antara SAR Dan ISAR?", "id": "SAR Platform Bergerak, ISAR Target Bergerak." },
  { "en": "Apa Kepanjangan INS (Inertial Navigation System)?", "id": "INS Adalah Sistem Navigasi Inersia." },
  { "en": "Sensor Apa Yang Digunakan Oleh Sistem INS?", "id": "Menggunakan Akselerometer Dan Juga Sensor Giroskop." },
  { "en": "Apa Kelemahan Utama Dari Sistem Navigasi INS?", "id": "Error Cenderung Terakumulasi Seiring Berjalannya Waktu." },
  { "en": "Apa Itu Kalman Filter Dalam Sistem Navigasi?", "id": "Algoritma Menggabungkan Data Sensor Berbeda Optimal." },
  { "en": "Apa Itu SPM (Self-Phase Modulation)?", "id": "Pergeseran Fasa Pulsa Optik Akibat Intensitasnya." },
  { "en": "Apa Itu XPM (Cross-Phase Modulation)?", "id": "Pergeseran Fasa Pulsa Akibat Interaksi Pulsa." },
  { "en": "Apa Kepanjangan SBS (Stimulated Brillouin Scattering)?", "id": "SBS Adalah Stimulated Brillouin Scattering." },
  { "en": "Apa Dampak Negatif SBS Dalam Serat Optik?", "id": "Menghamburkan Daya Sinyal Kembali Ke Arah." },
  { "en": "Apa Kepanjangan SRS (Stimulated Raman Scattering)?", "id": "SRS Adalah Stimulated Raman Scattering." },
  { "en": "Apa Dampak SRS Dalam Sistem Komunikasi WDM?", "id": "Mentransfer Daya Dari Kanal Frekuensi Tinggi." },
  { "en": "Apa Itu Fault Tolerance Dalam Desain Sistem?", "id": "Kemampuan Sistem Tetap Beroperasi Meski Gagal." },
  { "en": "Apa Kepanjangan UPS (Uninterruptible Power Supply)?", "id": "UPS Adalah Uninterruptible Power Supply." },
  { "en": "Apa Fungsi Utama Dari Perangkat Keras UPS?", "id": "Menyediakan Daya Cadangan Jangka Pendek Saat." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Hard Disk Untuk Redundansi." },
  { "en": "Apa Tujuan Utama Dari Sistem Pendingin Server?", "id": "Menjaga Suhu Operasi Optimal Perangkat Elektronik." },
  { "en": "Apa Itu Sistem Kontrol Akses Fisik?", "id": "Membatasi Siapa Saja Yang Boleh Masuk." },
  { "en": "Apa Itu Denial of Service (DoS) Attack?", "id": "Serangan Membuat Layanan Tidak Tersedia Bagi." },
  { "en": "Apa Itu Distributed DoS (DDoS) Attack?", "id": "Serangan DoS Dilakukan Dari Banyak Sumber." },
  { "en": "Apa Itu Botnet Dalam Keamanan Siber?", "id": "Jaringan Komputer Terinfeksi Malware Di Bawah." },
  { "en": "Apa Itu Phishing Dalam Keamanan Siber?", "id": "Upaya Memperoleh Informasi Sensitif Dengan Menyamar." },
  { "en": "Apa Itu Malware (Malicious Software)?", "id": "Perangkat Lunak Jahat Yang Dirancang Merusak." },
  { "en": "Sebutkan Jenis-Jenis Malware Yang Paling Umum?", "id": "Virus, Worm, Trojan Horse, Dan Ransomware." },
  { "en": "Apa Itu Virus Komputer Secara Definisi?", "id": "Program Jahat Mereplikasi Diri Menyisipkan Kode." },
  { "en": "Apa Itu Worm (Cacing) Komputer?", "id": "Malware Mandiri Menyebar Melalui Jaringan Komputer." },
  { "en": "Apa Itu Trojan Horse Dalam Dunia Malware?", "id": "Malware Menyamar Sebagai Perangkat Lunak Yang." },
  { "en": "Apa Itu Ransomware Dalam Keamanan Data?", "id": "Malware Mengenkripsi Data Korban Meminta Tebusan." },
  { "en": "Apa Itu Spyware Dalam Keamanan Data?", "id": "Malware Mengumpulkan Informasi Pengguna Tanpa Izin." },
  { "en": "Apa Itu Adware Dalam Dunia Perangkat Lunak?", "id": "Perangkat Lunak Menampilkan Iklan Tidak Diinginkan." },
  { "en": "Apa Itu Social Engineering (Rekayasa Sosial)?", "id": "Manipulasi Psikologis Untuk Memperoleh Informasi Rahasia." },
  { "en": "Apa Itu Two-Factor Authentication (2FA)?", "id": "Metode Keamanan Membutuhkan Dua Bentuk Verifikasi." },
  { "en": "Apa Itu Biometrik Dalam Sistem Otentikasi?", "id": "Menggunakan Karakteristik Fisik Unik (Sidik Jari)." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Izin Menentukan Siapa Akses Sumber." },
  { "en": "Apa Itu Network Segmentation (Segmentasi Jaringan)?", "id": "Membagi Jaringan Menjadi Sub-jaringan Lebih Kecil." },
  { "en": "Apa Manfaat Dari Melakukan Network Segmentation?", "id": "Meningkatkan Keamanan Dan Performa Kinerja Jaringan." },
  { "en": "Apa Itu Principle of Least Privilege (PoLP)?", "id": "Memberikan Hak Akses Minimum Yang Dibutuhkan." },
  { "en": "Apa Itu Zero-Day Exploit Dalam Keamanan?", "id": "Serangan Mengeksploitasi Celah Belum Diketahui Vendor." },
  { "en": "Apa Itu Patch Management Dalam Sistem Operasi?", "id": "Proses Mengelola Pembaruan Perangkat Lunak Sistem." },
  { "en": "Apa Itu Penetration Testing (Uji Penetrasi)?", "id": "Simulasi Serangan Siber Untuk Menemukan Celah." },
  { "en": "Apa Itu Ethical Hacking (Peretasan Etis)?", "id": "Aktivitas Peretasan Resmi Untuk Tujuan Keamanan." },
  { "en": "Apa Itu Kerberos Dalam Sistem Otentikasi?", "id": "Protokol Otentikasi Jaringan Menggunakan Kriptografi Simetris." },
  { "en": "Apa Itu Transport Layer Security (TLS)?", "id": "Protokol Kriptografi Pengganti SSL (Secure Sockets Layer)." },
  { "en": "Apa Fungsi Utama Dari Protokol Canggih TLS?", "id": "Memberikan Komunikasi Aman Melalui Jaringan Komputer." },
  { "en": "Apa Itu Open Systems Interconnection (OSI) Model?", "id": "Model Konseptual Tujuh Lapisan Standardisasi Fungsi." },
  { "en": "Apa Itu Data Encapsulation Dalam Model OSI?", "id": "Proses Menambahkan Header Kontrol Setiap Lapisan." },
  { "en": "Apa Itu Protocol Data Unit (PDU)?", "id": "Istilah Untuk Paket Data Di Lapisan." },
  { "en": "Apa Nama PDU Untuk Lapisan Fisik?", "id": "Lapisan Fisik Menggunakan Bit Sebagai PDU-nya." },
  { "en": "Apa Nama PDU Untuk Lapisan Data Link?", "id": "Lapisan Data Link Menggunakan Frame PDU." },
  { "en": "Apa Nama PDU Untuk Lapisan Jaringan?", "id": "Lapisan Jaringan Menggunakan Packet Sebagai PDU." },
  { "en": "Apa Nama PDU Untuk Lapisan Transport?", "id": "Lapisan Transport Menggunakan Segment (TCP) Datagram." },
  { "en": "Apa Nama PDU Untuk Tiga Lapisan Atas?", "id": "Tiga Lapisan Atas Menggunakan Data PDU." },
  { "en": "Apa Itu Collision Domain Dalam Jaringan Ethernet?", "id": "Segmen Jaringan Dimana Tabrakan Paket Bisa." },
  { "en": "Perangkat Apa Yang Beroperasi Di Collision Domain?", "id": "Hub Dan Repeater Beroperasi Dalam Collision." },
  { "en": "Perangkat Apa Yang Memisahkan Collision Domain?", "id": "Switch, Bridge, Dan Router Memisahkan Domain." },
  { "en": "Apa Itu Broadcast Domain Dalam Jaringan?", "id": "Area Jaringan Dimana Frame Broadcast Diteruskan." },
  { "en": "Perangkat Apa Yang Memisahkan Broadcast Domain?", "id": "Router Adalah Perangkat Memisahkan Broadcast Domain." },
  { "en": "Apa Itu VLAN (Virtual Local Area Network)?", "id": "Jaringan LAN Logis Terpisah Secara Fisik." },
  { "en": "Apa Keuntungan Menggunakan Teknologi Canggih VLAN?", "id": "Fleksibilitas, Keamanan, Dan Manajemen Jaringan Mudah." },
  { "en": "Apa Itu Trunking Dalam Konteks Jaringan VLAN?", "id": "Link Yang Membawa Lalu Lintas Beberapa." },
  { "en": "Protokol Apa Yang Digunakan Untuk VLAN Trunking?", "id": "Protokol IEEE (Institute of Electrical and Electronics Engineers) 802.1Q." },
  { "en": "Apa Itu Spanning Tree Protocol (STP)?", "id": "Protokol Jaringan Mencegah Terjadinya Looping Bridge." },
  { "en": "Apa Itu Looping Bridge Dalam Jaringan?", "id": "Kondisi Dimana Ada Beberapa Jalur Aktif." },
  { "en": "Apa Dampak Negatif Dari Looping Bridge?", "id": "Menyebabkan Broadcast Storm, Ketidakstabilan Tabel MAC." },
  { "en": "Apa Itu Broadcast Storm Dalam Jaringan?", "id": "Lalu Lintas Broadcast Berlebihan Membanjiri Jaringan." },
  { "en": "Apa Itu PortFast Dalam Konfigurasi Switch?", "id": "Fitur STP (Spanning Tree Protocol) Mempercepat Konektivitas." },
  { "en": "Apa Itu EtherChannel Dalam Teknologi Switching Cisco?", "id": "Menggabungkan Beberapa Link Fisik Menjadi Satu." },
  { "en": "Apa Manfaat Utama Dari Menggunakan EtherChannel?", "id": "Meningkatkan Bandwidth Dan Redundansi Antar Switch." },
  { "en": "Apa Itu Power over Ethernet (PoE)?", "id": "Teknologi Mengirimkan Daya Listrik Melalui Kabel." },
  { "en": "Perangkat Apa Yang Sering Menggunakan Teknologi PoE?", "id": "Telepon IP, Access Point, Kamera Keamanan." },
  { "en": "Apa Itu Routing on a Stick?", "id": "Metode Inter-VLAN Routing Menggunakan Satu Antarmuka." },
  { "en": "Apa Itu Switched Virtual Interface (SVI)?", "id": "Antarmuka VLAN Virtual Pada Switch Layer-3." },
  { "en": "Apa Itu Switch Layer-3 (Multilayer Switch)?", "id": "Switch Yang Juga Dapat Melakukan Fungsi." },
  { "en": "Apa Kepanjangan OSPF (Open Shortest Path First)?", "id": "OSPF (Open Shortest Path First) Adalah Jawabannya." },
  { "en": "Termasuk Algoritma Apa Protokol OSPF (Open Shortest Path First)?", "id": "OSPF (Open Shortest Path First) Menggunakan Link-State." },
  { "en": "Apa Itu Area Dalam Protokol OSPF (Open Shortest Path First)?", "id": "Sekelompok Router Berbagi Database Topologi Sama." },
  { "en": "Apa Nama Area Utama Desain OSPF (Open Shortest Path First)?", "id": "Area Tulang Punggung Atau Dikenal Area 0." },
  { "en": "Apa Itu Router ID Protokol OSPF (Open Shortest Path First)?", "id": "Alamat IP (Internet Protocol) Unik Identifikasi Router." },
  { "en": "Apa Itu DR (Designated Router) Di OSPF (Open Shortest Path First)?", "id": "Router Dipilih Mewakili Jaringan Multiakses Fisik." },
  { "en": "Apa Itu BDR (Backup Designated Router) OSPF (Open Shortest Path First)?", "id": "Router Cadangan Jika DR (Designated Router) Gagal." },
  { "en": "Mengapa Jaringan OSPF (Open Shortest Path First) Perlu DR Dan BDR?", "id": "Mengurangi Lalu Lintas Pertukaran Informasi Routing." },
  { "en": "Apa Kepanjangan EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "EIGRP (Enhanced Interior Gateway Routing Protocol) Adalah." },
  { "en": "Protokol Apa Yang Dikembangkan Perusahaan Cisco?", "id": "EIGRP (Enhanced Interior Gateway Routing Protocol) Miliknya." },
  { "en": "Algoritma Apa Digunakan Protokol EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Menggunakan DUAL (Diffusing Update Algorithm) Canggih." },
  { "en": "Sebutkan Metrik Digunakan Protokol EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Bandwidth, Delay, Load, Reliability, MTU (Maximum Transmission Unit)." },
  { "en": "Apa Itu Successor Protokol EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Rute Terbaik Untuk Mencapai Jaringan Tujuan." },
  { "en": "Apa Itu Feasible Successor (FS) EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Rute Cadangan Bebas Loop Ke Jaringan." },
  { "en": "Apa Kepanjangan BGP (Border Gateway Protocol)?", "id": "BGP (Border Gateway Protocol) Adalah Protokolnya." },
  { "en": "Protokol Apa Menjalankan Jaringan Internet Global?", "id": "BGP (Border Gateway Protocol) Protokol Inti Internet." },
  { "en": "Termasuk Jenis Apa Protokol Routing BGP (Border Gateway Protocol)?", "id": "BGP Adalah Contoh EGP (Exterior Gateway Protocol)." },
  { "en": "Apa Itu Path Attribute (Atribut Jalur) BGP (Border Gateway Protocol)?", "id": "Parameter Digunakan BGP Untuk Memilih Rute." },
  { "en": "Sebutkan Contoh Atribut Jalur BGP (Border Gateway Protocol)?", "id": "AS-Path, Next-Hop, Dan Local Preference." },
  { "en": "Apa Itu Sesi Peering Protokol BGP (Border Gateway Protocol)?", "id": "Koneksi TCP (Transmission Control Protocol) Antar Router." },
  { "en": "Apa Itu Route Map Konfigurasi Router?", "id": "Alat Mengontrol Dan Memodifikasi Informasi Rute." },
  { "en": "Apa Itu Prefix List Konfigurasi Router?", "id": "Mekanisme Mencocokkan Awalan Alamat IP (Internet Protocol)." },
  { "en": "Apa Kepanjangan VPN (Virtual Private Network)?", "id": "VPN (Virtual Private Network) Adalah Namanya." },
  { "en": "Apa Kepanjangan IPsec (Internet Protocol Security)?", "id": "IPsec (Internet Protocol Security) Adalah Jawabannya." },
  { "en": "Pada Lapisan OSI (Open Systems Interconnection) Mana IPsec Beroperasi?", "id": "IPsec (Internet Protocol Security) Beroperasi Di Network." },
  { "en": "Apa Itu AH (Authentication Header) Dalam IPsec (Internet Protocol Security)?", "id": "Menyediakan Integritas Data Dan Juga Otentikasi." },
  { "en": "Apa Itu ESP (Encapsulating Security Payload) IPsec (Internet Protocol Security)?", "id": "Menyediakan Enkripsi, Integritas, Dan Juga Otentikasi." },
  { "en": "Apa Itu SSL (Secure Sockets Layer) VPN (Virtual Private Network)?", "id": "Jenis VPN Menggunakan Protokol TLS/SSL." },
  { "en": "Apa Keuntungan Utama Dari SSL (Secure Sockets Layer) VPN?", "id": "Tidak Membutuhkan Instalasi Perangkat Lunak Khusus." },
  { "en": "Apa Kepanjangan MPLS (Multiprotocol Label Switching)?", "id": "MPLS (Multiprotocol Label Switching) Adalah Jawabannya." },
  { "en": "Apa Itu L2VPN (Layer 2 VPN)?", "id": "Konektivitas Layer 2 Melalui MPLS (Multiprotocol Label Switching)." },
  { "en": "Apa Itu L3VPN (Layer 3 VPN)?", "id": "Konektivitas Layer 3 Melalui MPLS (Multiprotocol Label Switching)." },
  { "en": "Apa Model Layanan QoS (Quality of Service) Best-Effort?", "id": "Jaringan Tidak Memberikan Jaminan Pengiriman Apapun." },
  { "en": "Apa Model Layanan QoS (Quality of Service) IntServ?", "id": "Aplikasi Meminta Reservasi Sumber Daya Jaringan." },
  { "en": "Apa Model Layanan QoS (Quality of Service) DiffServ?", "id": "Memberikan Perlakuan Berbeda Berdasarkan Kelas Layanan." },
  { "en": "Apa Itu Priority Queuing (PQ) Mekanisme Antrian?", "id": "Selalu Melayani Antrian Prioritas Tertinggi Dahulu." },
  { "en": "Apa Risiko Menggunakan Mekanisme Antrian PQ (Priority Queuing)?", "id": "Menyebabkan Starvation (Kelaparan) Antrian Prioritas Rendah." },
  { "en": "Apa Itu Custom Queuing (CQ) Mekanisme Antrian?", "id": "Alokasi Persentase Bandwidth Tetap Tiap Antrian." },
  { "en": "Apa Itu Weighted Fair Queuing (WFQ)?", "id": "Membagi Bandwidth Secara Adil Antar Aliran." },
  { "en": "Apa Itu Unicast Dalam Pengiriman Data?", "id": "Pengiriman Data Dari Satu Sumber Satu." },
  { "en": "Apa Itu Broadcast Dalam Pengiriman Data?", "id": "Pengiriman Data Dari Satu Sumber Semua." },
  { "en": "Apa Itu Multicast Dalam Pengiriman Data?", "id": "Pengiriman Data Satu Sumber Grup Tertentu." },
  { "en": "Apa Itu Jitter Dalam Transmisi Jaringan?", "id": "Variasi Dalam Waktu Tiba Paket Data." },
  { "en": "Apa Itu Latency Dalam Jaringan Komputer?", "id": "Waktu Tunda Data Dari Sumber Tujuan." },
  { "en": "Apa Itu RTT (Round-Trip Time)?", "id": "Waktu Tunda Paket Pergi Dan Kembali." },
  { "en": "Apa Itu BDP (Bandwidth-Delay Product)?", "id": "Ukuran Kapasitas Data Maksimum Suatu Link." },
  { "en": "Apa Itu Simplex Communication Dalam Suatu Sistem?", "id": "Komunikasi Yang Terjadi Hanya Satu Arah." },
  { "en": "Apa Itu Half-Duplex Communication Suatu Sistem?", "id": "Komunikasi Dua Arah, Tetapi Secara Bergantian." },
  { "en": "Apa Itu Full-Duplex Communication Suatu Sistem?", "id": "Komunikasi Dua Arah Yang Terjadi Bersamaan." },
  { "en": "Apa Itu FHRP (First Hop Redundancy Protocol)?", "id": "Protokol Menyediakan Default Gateway Yang Redundan." },
  { "en": "Sebutkan Contoh Protokol FHRP (First Hop Redundancy Protocol)?", "id": "HSRP (Hot Standby Router Protocol) Contohnya." },
  { "en": "Apa Kepanjangan HSRP (Hot Standby Router Protocol)?", "id": "HSRP (Hot Standby Router Protocol) Adalah Namanya." },
  { "en": "Apa Kepanjangan VRRP (Virtual Router Redundancy Protocol)?", "id": "VRRP (Virtual Router Redundancy Protocol) Adalah Jawabannya." },
  { "en": "Apa Kepanjangan GLBP (Gateway Load Balancing Protocol)?", "id": "GLBP (Gateway Load Balancing Protocol) Adalah Namanya." },
  { "en": "Protokol Mana Standar Terbuka, HSRP Atau VRRP?", "id": "VRRP (Virtual Router Redundancy Protocol) Standar Terbuka." },
  { "en": "Protokol FHRP (First Hop Redundancy Protocol) Mana Mendukung Load Balancing?", "id": "GLBP (Gateway Load Balancing Protocol) Mendukungnya." },
  { "en": "Apa Itu Wildcard Mask Konfigurasi Jaringan?", "id": "Kebalikan Subnet Mask Mencocokkan Alamat IP." },
  { "en": "Apa Itu NAPT (Network Address Port Translation)?", "id": "Nama Lain PAT (Port Address Translation)." },
  { "en": "Apa Itu TFTP (Trivial File Transfer Protocol)?", "id": "Protokol Transfer File Sederhana Tanpa Otentikasi." },
  { "en": "Port UDP (User Datagram Protocol) Berapa Digunakan TFTP?", "id": "TFTP (Trivial File Transfer Protocol) Menggunakan Port 69." },
  { "en": "Apa Itu NNTP (Network News Transfer Protocol)?", "id": "Protokol Membaca Mengirim Artikel Grup Usenet." },
  { "en": "Apa Itu LACP (Link Aggregation Control Protocol)?", "id": "Protokol Standar Menggabungkan Link (EtherChannel)." },
  { "en": "Apa Itu PAgP (Port Aggregation Protocol)?", "id": "Protokol Milik Cisco Menggabungkan Link EtherChannel." },
  { "en": "Apa Itu CoS (Class of Service)?", "id": "Metode Penandaan Prioritas Pada Frame Layer-2." },
  { "en": "Apa Itu DSCP (Differentiated Services Code Point)?", "id": "Metode Penandaan Prioritas Pada Paket Layer-3." },
  { "en": "Apa Itu Congestion Avoidance Manajemen Jaringan?", "id": "Mekanisme Mendeteksi Kemacetan Sebelum Terjadi Parah." },
  { "en": "Sebutkan Contoh Algoritma Congestion Avoidance?", "id": "RED (Random Early Detection) Adalah Contohnya." },
  { "en": "Apa Itu RED (Random Early Detection)?", "id": "Acak Membuang Paket Ketika Antrian Penuh." },
  { "en": "Apa Itu Traffic Policing Manajemen QoS (Quality of Service)?", "id": "Membatasi Lalu Lintas Masuk Sesuai Kebijakan." },
  { "en": "Apa Itu Traffic Shaping Manajemen QoS (Quality of Service)?", "id": "Menahan Paket Berlebih Melancarkan Lalu Lintas." },
  { "en": "Apa Itu LLQ (Low Latency Queuing)?", "id": "Kombinasi PQ (Priority Queuing) Dan CBWFQ." },
  { "en": "Apa Itu CIR (Committed Information Rate)?", "id": "Tingkat Bandwidth Dijamin Oleh Penyedia Layanan." },
  { "en": "Apa Itu Egress Dan Ingress Traffic?", "id": "Egress Keluar, Ingress Adalah Lalu Lintas." },
  { "en": "Apa Itu ASN (Autonomous System Number)?", "id": "Nomor Unik Mengidentifikasi AS (Autonomous System)." },
  { "en": "Siapa Yang Mengelola Alokasi ASN (Autonomous System Number) Global?", "id": "IANA (Internet Assigned Numbers Authority) Dan RIR." },
  { "en": "Apa Itu RIR (Regional Internet Registry)?", "id": "Organisasi Mengelola Alokasi IP (Internet Protocol) Regional." },
  { "en": "Apa RIR (Regional Internet Registry) Wilayah Asia Pasifik?", "id": "APNIC (Asia-Pacific Network Information Centre) Wilayahnya." },
  { "en": "Apa Itu Route Flap Dampening BGP (Border Gateway Protocol)?", "id": "Mekanisme Menekan Rute Jaringan Tidak Stabil." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Community Attribute?", "id": "Atribut Menandai Grup Rute Untuk Kebijakan." },
  { "en": "Apa Itu iBGP (Internal BGP) Peering?", "id": "Sesi Peering BGP (Border Gateway Protocol) Dalam AS." },
  { "en": "Apa Itu eBGP (External BGP) Peering?", "id": "Sesi Peering BGP (Border Gateway Protocol) Antar AS." },
  { "en": "Apa Aturan BGP (Border Gateway Protocol) Split Horizon?", "id": "Mencegah Looping Rute Dalam Sesi iBGP (Internal BGP)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Reflector?", "id": "Solusi Mengurangi Jumlah Sesi iBGP (Internal BGP)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Confederation?", "id": "Membagi AS (Autonomous System) Besar Menjadi Sub-AS." },
  { "en": "Apa Itu Tunnel Mode Protokol IPsec (Internet Protocol Security)?", "id": "Mengenkapsulasi Seluruh Paket IP (Internet Protocol) Asli." },
  { "en": "Apa Itu Transport Mode Protokol IPsec (Internet Protocol Security)?", "id": "Hanya Mengenkripsi Payload Dari Paket IP." },
  { "en": "Apa Itu IKE (Internet Key Exchange)?", "id": "Protokol Negosiasi Kunci Keamanan Di IPsec (Internet Protocol Security)." },
  { "en": "Apa Itu SA (Security Association) IPsec (Internet Protocol Security)?", "id": "Kumpulan Parameter Keamanan Antara Dua Pihak." },
  { "en": "Apa Itu DMVPN (Dynamic Multipoint VPN)?", "id": "Solusi VPN (Virtual Private Network) Skalabel Cisco." },
  { "en": "Apa Itu GRE (Generic Routing Encapsulation)?", "id": "Protokol Tunneling Dapat Membungkus Berbagai Protokol." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type?", "id": "Menentukan Bagaimana OSPF Beroperasi Di Link." },
  { "en": "Sebutkan Jenis Network Type OSPF (Open Shortest Path First)?", "id": "Broadcast, Point-to-Point, NBMA (Non-Broadcast Multi-Access)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Stub Area?", "id": "Area Yang Tidak Menerima Rute Eksternal." },
  { "en": "Apa Itu LSA (Link-State Advertisement)?", "id": "Paket Data OSPF (Open Shortest Path First)." },
  { "en": "Apa Itu Dijkstra's Algorithm Dunia Routing?", "id": "Algoritma OSPF (Open Shortest Path First) Hitung." },
  { "en": "Apa Itu Loopback Interface Sebuah Router?", "id": "Antarmuka Virtual Selalu Aktif Di Router." },
  { "en": "Apa Kegunaan Loopback Interface OSPF (Open Shortest Path First)?", "id": "Sering Digunakan Sebagai Router ID Stabil." },
  { "en": "Apa Itu Passive Interface Konfigurasi Routing?", "id": "Mencegah Protokol Routing Mengirim Update Keluar." },
  { "en": "Apa Itu Sistem Operasi Jaringan (Network Operating System)?", "id": "Sistem Operasi Didesain Khusus Manajemen Jaringan." },
  { "en": "Sebutkan Contoh Sistem Operasi Jaringan Populer?", "id": "Windows Server, Linux, Dan Juga Cisco IOS (Internetwork Operating System)." },
  { "en": "Apa Kepanjangan IOS (Internetwork Operating System)?", "id": "IOS Adalah Singkatan Internetwork Operating System." },
  { "en": "Apa Itu Antarmuka Baris Perintah (Command-Line Interface)?", "id": "Antarmuka Pengguna Berbasis Teks Input Perintah." },
  { "en": "Apa Itu Antarmuka Pengguna Grafis (Graphical User Interface)?", "id": "Antarmuka Berbasis Elemen Grafis Dan Ikon." },
  { "en": "Mode Apa Yang Paling Dasar CLI (Command-Line Interface) Cisco?", "id": "Mode User EXEC (Executive) Adalah Dasarnya." },
  { "en": "Mode Apa Yang Memberikan Hak Akses Penuh?", "id": "Mode Privileged EXEC (Executive) Memberikan Hak." },
  { "en": "Perintah Apa Untuk Masuk Ke Mode Privileged?", "id": "Perintah 'enable' Digunakan Untuk Masuk Mode." },
  { "en": "Perintah Apa Untuk Masuk Mode Konfigurasi Global?", "id": "Perintah 'configure terminal' Untuk Masuk Mode." },
  { "en": "Apa Fungsi Dari Perintah 'show running-config'?", "id": "Menampilkan Konfigurasi Yang Sedang Berjalan Aktif." },
  { "en": "Apa Fungsi Dari Perintah 'show startup-config'?", "id": "Menampilkan Konfigurasi Yang Disimpan Di NVRAM." },
  { "en": "Di Memori Apa Konfigurasi Berjalan Disimpan?", "id": "Konfigurasi Berjalan Disimpan Di RAM (Random-Access Memory)." },
  { "en": "Di Memori Apa Konfigurasi Startup Disimpan?", "id": "Konfigurasi Startup Disimpan Di NVRAM (Non-Volatile Random-Access Memory)." },
  { "en": "Perintah Apa Untuk Menyimpan Konfigurasi Berjalan?", "id": "Perintah 'copy running-config startup-config' Digunakan." },
  { "en": "Apa Itu Banner MOTD (Message of the Day)?", "id": "Pesan Ditampilkan Saat Pengguna Login Perangkat." },
  { "en": "Apa Fungsi Dari Perintah 'show ip interface brief'?", "id": "Tampilkan Status Singkat Semua Antarmuka IP (Internet Protocol)." },
  { "en": "Apa Fungsi Perintah 'no shutdown' Antarmuka?", "id": "Untuk Mengaktifkan Antarmuka Secara Administratif (Up)." },
  { "en": "Apa Itu Clock Rate Pada Antarmuka Serial?", "id": "Menentukan Kecepatan Transmisi Sisi DCE (Data Communications Equipment)." },
  { "en": "Sisi Mana Dari Kabel Serial Membutuhkan Clock?", "id": "Sisi DCE (Data Communications Equipment) Membutuhkannya." },
  { "en": "Apa Itu DTE (Data Terminal Equipment)?", "id": "Perangkat Seperti Router Atau Komputer Pengguna." },
  { "en": "Apa Itu DCE (Data Communications Equipment)?", "id": "Perangkat Seperti Modem Menyediakan Sinyal Clock." },
  { "en": "Apa Itu Collision Detection (CD) Di Ethernet?", "id": "Mekanisme CSMA/CD (Carrier-Sense Multiple Access with Collision Detection) Deteksi Tabrakan." },
  { "en": "Apa Kepanjangan CDP (Cisco Discovery Protocol)?", "id": "CDP Adalah Singkatan Cisco Discovery Protocol." },
  { "en": "Apa Fungsi Utama Dari Protokol CDP?", "id": "Mengumpulkan Informasi Tentang Perangkat Cisco Terhubung." },
  { "en": "Pada Lapisan OSI (Open Systems Interconnection) Mana CDP Bekerja?", "id": "CDP (Cisco Discovery Protocol) Bekerja Di Layer-2." },
  { "en": "Apa Kepanjangan LLDP (Link Layer Discovery Protocol)?", "id": "LLDP Adalah Singkatan Link Layer Discovery Protocol." },
  { "en": "Apa Perbedaan Utama Antara CDP Dan LLDP?", "id": "LLDP (Link Layer Discovery Protocol) Standar Terbuka." },
  { "en": "Apa Itu File System Flash Pada Router?", "id": "Memori Non-Volatil Menyimpan Image IOS (Internetwork Operating System)." },
  { "en": "Apa Itu Proses Boot-up Sebuah Router?", "id": "Urutan Langkah Router Saat Pertama Dinyalakan." },
  { "en": "Apa Langkah Pertama Dalam Proses Boot Router?", "id": "Menjalankan POST (Power-On Self-Test) Diagnostik Hardware." },
  { "en": "Dari Mana Bootstrap Loader Mencari IOS?", "id": "Mencari IOS (Internetwork Operating System) Di Flash." },
  { "en": "Apa Itu Configuration Register Pada Sebuah Router?", "id": "Mengontrol Bagaimana Router Melakukan Proses Booting." },
  { "en": "Nilai Configuration Register Mana Untuk Password Recovery?", "id": "Nilai 0x2142 Digunakan Untuk Proses Recovery." },
  { "en": "Apa Itu ROMMON (ROM Monitor) Mode?", "id": "Mode Perawatan Dasar Jika IOS Gagal." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol) Snooping?", "id": "Fitur Keamanan Mencegah Server DHCP (Dynamic Host Configuration Protocol) Palsu." },
  { "en": "Apa Itu Dynamic ARP (Address Resolution Protocol) Inspection?", "id": "Fitur Keamanan Memvalidasi Paket ARP (Address Resolution Protocol)." },
  { "en": "Apa Itu Port Security Pada Switch?", "id": "Fitur Membatasi Jumlah MAC (Media Access Control) Address." },
  { "en": "Apa Tiga Mode Pelanggaran (Violation) Port Security?", "id": "Shutdown, Protect, Dan Juga Mode Restrict." },
  { "en": "Apa Yang Terjadi Pada Mode Violation Shutdown?", "id": "Port Dimatikan Dan Pesan Log Dikirim." },
  { "en": "Apa Itu Sticky MAC Address Learning?", "id": "Secara Dinamis Mempelajari MAC (Media Access Control) Address." },
  { "en": "Apa Itu Network Time Protocol (NTP)?", "id": "Protokol Untuk Sinkronisasi Waktu Di Jaringan." },
  { "en": "Apa Itu Stratum Dalam Hierarki Protokol NTP?", "id": "Level Keakuratan Sumber Waktu Jaringan NTP." },
  { "en": "Stratum Berapa Yang Paling Akurat Dalam NTP?", "id": "Stratum 0 (Jam Atom) Adalah Terakurat." },
  { "en": "Apa Itu Syslog Dalam Manajemen Jaringan?", "id": "Protokol Standar Untuk Mengirim Pesan Log." },
  { "en": "Sebutkan Tingkat Keparahan (Severity Level) Syslog?", "id": "Dari 0 (Emergency) Hingga 7 (Debugging)." },
  { "en": "Tingkat Keparahan Syslog Mana Paling Kritis?", "id": "Level 0 (Emergency) Adalah Paling Kritis." },
  { "en": "Apa Kepanjangan SNMP (Simple Network Management Protocol)?", "id": "SNMP Adalah Simple Network Management Protocol." },
  { "en": "Apa Fungsi Perintah 'debug' Di Cisco IOS?", "id": "Menampilkan Output Real-time Proses Jaringan Detail." },
  { "en": "Mengapa Perintah 'debug' Harus Hati-hati Digunakan?", "id": "Dapat Menyebabkan Beban CPU (Central Processing Unit) Tinggi." },
  { "en": "Apa Itu Floating Static Route (Rute Statis)?", "id": "Rute Statis Cadangan Dengan Administrative Distance." },
  { "en": "Apa Itu Administrative Distance (AD)?", "id": "Ukuran Kepercayaan Router Terhadap Sumber Rute." },
  { "en": "Nilai AD (Administrative Distance) Mana Lebih Dipercaya?", "id": "Nilai Yang Lebih Rendah Lebih Dipercaya." },
  { "en": "Berapa Nilai AD (Administrative Distance) Rute Statis?", "id": "Nilai Defaultnya Adalah 1 Untuk Rute." },
  { "en": "Berapa Nilai AD (Administrative Distance) Protokol OSPF?", "id": "Nilai Defaultnya Adalah 110 Untuk OSPF." },
  { "en": "Berapa Nilai AD (Administrative Distance) Internal EIGRP?", "id": "Nilai Defaultnya Adalah 90 Untuk Internal." },
  { "en": "Apa Itu Recursive Lookup Dalam Tabel Routing?", "id": "Router Mencari Rute Lagi Untuk Capai." },
  { "en": "Apa Itu Classless Inter-Domain Routing (CIDR)?", "id": "Metode Alokasi IP (Internet Protocol) Mengabaikan Kelas." },
  { "en": "Apa Itu Variable Length Subnet Mask (VLSM)?", "id": "Memungkinkan Penggunaan Subnet Mask Berbeda Jaringan." },
  { "en": "Apa Manfaat Utama Dari Penggunaan Teknik VLSM?", "id": "Menghemat Alamat IP (Internet Protocol) Secara Efisien." },
  { "en": "Apa Itu Route Aggregation Atau Supernetting?", "id": "Menggabungkan Beberapa Jaringan Menjadi Satu Rute." },
  { "en": "Apa Itu Default Route (Rute Default)?", "id": "Rute Digunakan Jika Tidak Ada Rute." },
  { "en": "Bagaimana Notasi Rute Default Dituliskan Jelas?", "id": "0.0.0.0/0 Adalah Notasi Rute Default." },
  { "en": "Apa Itu Split Horizon Dalam Protokol Routing?", "id": "Mencegah Rute Dikirim Kembali Ke Antarmuka." },
  { "en": "Apa Itu Poison Reverse Dalam Protokol Routing?", "id": "Mengirim Rute Dengan Metrik Tak Terhingga." },
  { "en": "Apa Itu Holddown Timer Dalam Protokol Routing?", "id": "Mencegah Rute Tidak Stabil Diterima Sementara." },
  { "en": "Apa Itu Multicast Address Untuk Protokol EIGRP?", "id": "Menggunakan Alamat 224.0.0.10 Untuk Update." },
  { "en": "Apa Itu Multicast Address Untuk Protokol OSPF?", "id": "224.0.0.5 (Semua Router) 224.0.0.6 (DR/BDR)." },
  { "en": "Apa Itu Authentication Dalam Protokol Routing?", "id": "Memastikan Update Routing Datang Dari Sumber." },
  { "en": "Metode Otentikasi Apa Yang Didukung OSPF?", "id": "Plain Text Dan Juga MD5 (Message Digest 5)." },
  { "en": "Apa Itu Passive-Interface Dalam Konfigurasi Routing?", "id": "Mencegah Antarmuka Mengirim Dan Menerima Update." },
  { "en": "Apa Itu Link-State Database (LSDB) OSPF?", "id": "Database Berisi Semua LSA (Link-State Advertisement) Area." },
  { "en": "Apa Itu OSPF Hello Packet (Paket Hello)?", "id": "Digunakan Untuk Menemukan Memelihara Hubungan Tetangga." },
  { "en": "Apa Itu OSPF Dead Interval (Interval Mati)?", "id": "Waktu Router Menunggu Sebelum Anggap Tetangga." },
  { "en": "Berapa Nilai Hello Dan Dead Interval Default?", "id": "Broadcast: Hello 10 Detik, Dead 40." },
  { "en": "Apa Itu Area Border Router (ABR)?", "id": "Router Yang Terhubung Ke Beberapa Area." },
  { "en": "Apa Itu Autonomous System Boundary Router (ASBR)?", "id": "Router OSPF (Open Shortest Path First) Terhubung Jaringan." },
  { "en": "Sebutkan Jenis LSA (Link-State Advertisement) OSPF Dasar?", "id": "LSA (Link-State Advertisement) Tipe 1, 2, 3, 4, 5." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 1?", "id": "Router LSA, Dihasilkan Setiap Router OSPF." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 2?", "id": "Network LSA, Dihasilkan Oleh DR (Designated Router)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 3?", "id": "Summary LSA, Dihasilkan Oleh ABR (Area Border Router)." },
  { "en": "Apa Itu Virtual Link Dalam Protokol OSPF?", "id": "Link Logis Menghubungkan Area Terputus Backbone." },
  { "en": "Apa Itu Access Control List (ACL) Standar?", "id": "Memfilter Lalu Lintas Hanya Berdasarkan Alamat." },
  { "en": "Berapa Rentang Nomor Untuk ACL (Access Control List) Standar?", "id": "Rentang Nomor Dari 1 Hingga 99." },
  { "en": "Apa Itu Access Control List (ACL) Extended?", "id": "Memfilter Berdasarkan Alamat Sumber, Tujuan, Port." },
  { "en": "Berapa Rentang Nomor Untuk ACL (Access Control List) Extended?", "id": "Rentang Nomor Dari 100 Hingga 199." },
  { "en": "Di Mana Sebaiknya ACL (Access Control List) Standar Ditempatkan?", "id": "Ditempatkan Sedekat Mungkin Dengan Jaringan Tujuan." },
  { "en": "Di Mana Sebaiknya ACL (Access Control List) Extended Ditempatkan?", "id": "Ditempatkan Sedekat Mungkin Dengan Jaringan Sumber." },
  { "en": "Apa Aturan Terakhir Implisit Dalam Setiap ACL?", "id": "Aturan 'deny any' (Tolak Semua) Tersembunyi." },
  { "en": "Perintah Apa Untuk Menerapkan ACL (Access Control List) Antarmuka?", "id": "Perintah 'ip access-group' Diterapkan Di Antarmuka." },
  { "en": "Apa Itu Named ACL (Access Control List)?", "id": "ACL (Access Control List) Yang Diidentifikasi Nama." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol) Relay Agent?", "id": "Meneruskan Permintaan DHCP (Dynamic Host Configuration Protocol) Antar Jaringan." },
  { "en": "Perintah Apa Untuk Mengkonfigurasi DHCP (Dynamic Host Configuration Protocol) Relay?", "id": "Perintah 'ip helper-address' Untuk Mengkonfigurasi Relay." },
  { "en": "Apa Itu SLAAC (Stateless Address Autoconfiguration) IPv6?", "id": "Metode Perangkat Konfigurasi Alamat IPv6 (Internet Protocol version 6) Otomatis." },
  { "en": "Protokol Apa Yang Digunakan Dalam Proses SLAAC?", "id": "Menggunakan ICMPv6 (Internet Control Message Protocol for IPv6) Router Advertisement." },
  { "en": "Apa Itu EUI-64 (Extended Unique Identifier) IPv6?", "id": "Proses Membuat Interface ID Dari MAC (Media Access Control)." },
  { "en": "Apa Itu Anycast Address Dalam Protokol IPv6?", "id": "Alamat Unicast Ditetapkan Ke Beberapa Antarmuka." },
  { "en": "Apa Itu Unique Local Address (ULA) IPv6?", "id": "Alamat Mirip RFC (Request for Comments) 1918 IPv4." },
  { "en": "Apa Itu Global Unicast Address (GUA) IPv6?", "id": "Alamat IPv6 (Internet Protocol version 6) Unik Global." },
  { "en": "Apa Itu Tunneling Dalam Jaringan IPv6 (Internet Protocol version 6)?", "id": "Membungkus Paket IPv6 Dalam Paket IPv4." },
  { "en": "Sebutkan Contoh Mekanisme Tunneling IPv6 Otomatis?", "id": "6to4 Tunneling Dan Juga ISATAP Tunneling." },
  { "en": "Apa Kepanjangan ISATAP (Intra-Site Automatic Tunnel Addressing)?", "id": "ISATAP Adalah Intra-Site Automatic Tunnel Addressing." },
  { "en": "Apa Itu NAT64 (Network Address Translation 64)?", "id": "Mekanisme Transisi Menerjemahkan Alamat IPv6 IPv4." },
  { "en": "Apa Fungsi Dari DNS64 (Domain Name System 64)?", "id": "Mensintesis Record AAAA Dari Record A." },
  { "en": "Apa Itu Neighbor Discovery Protocol (NDP) IPv6?", "id": "Protokol Menemukan Perangkat Lain Di Link." },
  { "en": "Protokol Apa Yang Digantikan Oleh NDP?", "id": "Menggantikan Fungsi ARP (Address Resolution Protocol) Di IPv4." },
  { "en": "Sebutkan Pesan ICMPv6 (Internet Control Message Protocol for IPv6) Digunakan NDP?", "id": "Router Solicitation, Router Advertisement, Neighbor Solicitation." },
  { "en": "Apa Itu Router Solicitation (RS) Message?", "id": "Pesan Host Meminta Router Advertisement Segera." },
  { "en": "Apa Itu Router Advertisement (RA) Message?", "id": "Pesan Router Mengiklankan Kehadirannya Di Link." },
  { "en": "Apa Itu Neighbor Solicitation (NS) Message?", "id": "Pesan Untuk Menentukan Alamat Link-Layer Tetangga." },
  { "en": "Apa Itu HSRP (Hot Standby Router Protocol) State?", "id": "Initial, Listen, Speak, Standby, Dan Active." },
  { "en": "Siapa Yang Dipilih Sebagai Active Router HSRP?", "id": "Router Dengan Prioritas Tertinggi Dipilih Aktif." },
  { "en": "Apa Yang Terjadi Jika Prioritas HSRP Sama?", "id": "Router Dengan Alamat IP (Internet Protocol) Tertinggi." },
  { "en": "Apa Itu Virtual MAC (Media Access Control) Address HSRP?", "id": "Alamat MAC Virtual Yang Digunakan Grup." },
  { "en": "Apa Itu Preemption Dalam Protokol HSRP?", "id": "Memungkinkan Router Prioritas Tinggi Mengambil Alih." },
  { "en": "Apa Itu Object Tracking Dalam Protokol HSRP?", "id": "Memantau Status Antarmuka Mengubah Prioritas HSRP." },
  { "en": "Apa Perbedaan Utama Antara HSRP Dan VRRP?", "id": "VRRP (Virtual Router Redundancy Protocol) Adalah Standar Terbuka." },
  { "en": "Istilah Apa Yang Digunakan VRRP Untuk Active?", "id": "VRRP (Virtual Router Redundancy Protocol) Menggunakan Istilah Master." },
  { "en": "Apa Itu PortFast BPDU (Bridge Protocol Data Unit) Guard?", "id": "Mematikan Port Jika Menerima Paket BPDU." },
  { "en": "Apa Itu Root Guard Dalam STP (Spanning Tree Protocol)?", "id": "Mencegah Switch Lain Menjadi Root Bridge." },
  { "en": "Apa Itu UDLD (UniDirectional Link Detection)?", "id": "Protokol Mendeteksi Link Searah Sebelum STP." },
  { "en": "Apa Itu Flex Links Dalam Switching Cisco?", "id": "Solusi Redundansi Link Alternatif Untuk STP." },
  { "en": "Apa Itu VTP (VLAN Trunking Protocol)?", "id": "Protokol Cisco Menyebarkan Informasi VLAN (Virtual Local Area Network)." },
  { "en": "Sebutkan Tiga Mode Operasi Protokol VTP?", "id": "Server, Client, Dan Juga Mode Transparent." },
  { "en": "Apa Fungsi VTP (VLAN Trunking Protocol) Pruning?", "id": "Mencegah Lalu Lintas VLAN (Virtual Local Area Network) Tidak." },
  { "en": "Apa Itu DTP (Dynamic Trunking Protocol)?", "id": "Protokol Cisco Negosiasi Trunking Secara Otomatis." },
  { "en": "Sebutkan Mode Operasi Protokol DTP?", "id": "Access, Trunk, Dynamic Auto, Dynamic Desirable." },
  { "en": "Mode DTP (Dynamic Trunking Protocol) Mana Memulai Negosiasi?", "id": "Mode Dynamic Desirable Secara Aktif Memulai." },
  { "en": "Apa Itu Native VLAN Dalam Trunking?", "id": "VLAN (Virtual Local Area Network) Tidak Diberi Tag." },
  { "en": "Apa Itu Rapid Spanning Tree Protocol (RSTP)?", "id": "Evolusi STP (Spanning Tree Protocol) Dengan Konvergensi." },
  { "en": "Standar IEEE (Institute of Electrical and Electronics Engineers) Mana Mendefinisikan RSTP?", "id": "IEEE (Institute of Electrical and Electronics Engineers) 802.1w Mendefinisikannya." },
  { "en": "Apa Itu Port States Dalam Protokol RSTP?", "id": "Discarding, Learning, Dan Juga Forwarding State." },
  { "en": "Apa Itu Multiple Spanning Tree Protocol (MSTP)?", "id": "Memetakan Beberapa VLAN (Virtual Local Area Network) Ke Instance." },
  { "en": "Apa Itu Per-VLAN Spanning Tree Plus (PVST+)?", "id": "Varian STP (Spanning Tree Protocol) Cisco Satu." },
  { "en": "Apa Itu Private VLAN (PVLAN)?", "id": "Teknik Segmentasi Port Dalam VLAN (Virtual Local Area Network) Sama." },
  { "en": "Sebutkan Tiga Jenis Port Dalam PVLAN?", "id": "Promiscuous, Isolated, Dan Juga Port Community." },
  { "en": "Apa Itu Storm Control Pada Perangkat Switch?", "id": "Membatasi Lalu Lintas Broadcast, Multicast, Unicast." },
  { "en": "Apa Itu SPAN (Switched Port Analyzer)?", "id": "Menyalin Lalu Lintas Satu Port Ke." },
  { "en": "Apa Nama Lain Untuk Teknologi Port SPAN?", "id": "Dikenal Juga Dengan Sebutan Port Mirroring." },
  { "en": "Apa Itu RSPAN (Remote SPAN)?", "id": "Memantau Port Sumber Di Switch Berbeda." },
  { "en": "Apa Itu StackWise Technology Dalam Switch Cisco?", "id": "Teknologi Menumpuk Beberapa Switch Menjadi Satu." },
  { "en": "Apa Itu Virtual Switching System (VSS)?", "id": "Menggabungkan Dua Switch Fisik Satu Switch." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Memungkinkan Aplikasi Berkomunikasi Satu Sama." },
  { "en": "Apa Itu REST (Representational State Transfer) API?", "id": "Gaya Arsitektur API (Application Programming Interface) Populer." },
  { "en": "Metode HTTP (Hypertext Transfer Protocol) Apa Digunakan REST?", "id": "GET, POST, PUT, DELETE, Dan Lainnya." },
  { "en": "Format Data Apa Umumnya Digunakan REST API?", "id": "JSON (JavaScript Object Notation) Format Umumnya." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan Mudah Dibaca." },
  { "en": "Apa Itu YAML (YAML Ain't Markup Language)?", "id": "Format Serialisasi Data Mudah Dibaca Manusia." },
  { "en": "Apa Itu Ansible Dalam Otomatisasi Jaringan?", "id": "Alat Otomatisasi Agentless Menggunakan SSH (Secure Shell)." },
  { "en": "Apa Itu Puppet Dalam Otomatisasi Jaringan?", "id": "Alat Manajemen Konfigurasi Berbasis Model Deklaratif." },
  { "en": "Apa Itu Chef Dalam Otomatisasi Jaringan?", "id": "Kerangka Otomatisasi Menggunakan 'Recipe' Dan 'Cookbook'." },
  { "en": "Apa Itu Git Dalam Kontrol Versi?", "id": "Sistem Kontrol Versi Terdistribusi Sangat Populer." },
  { "en": "Apa Itu Repository Dalam Konteks Teknologi Git?", "id": "Lokasi Penyimpanan Proyek Git Dan Riwayatnya." },
  { "en": "Apa Perintah Untuk Mengkloning Repository Git?", "id": "Perintah 'git clone' Digunakan Untuk Mengkloning." },
  { "en": "Apa Itu 'commit' Dalam Sistem Version Control?", "id": "Menyimpan Perubahan Ke Repository Lokal Anda." },
  { "en": "Apa Itu 'branch' Dalam Sistem Version Control?", "id": "Versi Independen Dari Proyek Sedang Dikerjakan." },
  { "en": "Apa Itu Zero Trust Security Model?", "id": "Model Keamanan 'Never Trust, Always Verify'." },
  { "en": "Apa Itu SASE (Secure Access Service Edge)?", "id": "Arsitektur Cloud Menggabungkan Jaringan Keamanan Terpadu." },
  { "en": "Apa Itu SD-WAN (Software-Defined Wide Area Network)?", "id": "Pendekatan Virtualisasi Mengelola Jaringan WAN (Wide Area Network)." },
  { "en": "Apa Keuntungan Utama Dari Teknologi SD-WAN?", "id": "Manajemen Terpusat, Efisiensi Biaya, Performa Aplikasi." },
  { "en": "Apa Itu Underlay Network Dalam SD-WAN?", "id": "Infrastruktur Jaringan Fisik (Contoh: MPLS, Internet)." },
  { "en": "Apa Itu Overlay Network Dalam SD-WAN?", "id": "Jaringan Virtual Dibangun Diatas Jaringan Underlay." },
  { "en": "Apa Itu Data Center (Pusat Data)?", "id": "Fasilitas Terpusat Menyimpan Mengelola Aset Komputasi." },
  { "en": "Apa Itu Arsitektur Spine-Leaf Data Center?", "id": "Topologi Jaringan Dua Lapis Modern Terukur." },
  { "en": "Apa Itu VXLAN (Virtual Extensible LAN)?", "id": "Teknologi Overlay Enkapsulasi Frame Layer-2 Layer-3." },
  { "en": "Apa Tujuan Utama Dari Teknologi VXLAN?", "id": "Mengatasi Keterbatasan Skalabilitas VLAN (Virtual Local Area Network) tradisional." },
  { "en": "Apa Itu Border Gateway Multicast Protocol (BGMP)?", "id": "Protokol Routing Multicast Antar Domain Skalabel." },
  { "en": "Apa Itu Protocol Independent Multicast (PIM)?", "id": "Keluarga Protokol Routing Multicast Populer Digunakan." },
  { "en": "Sebutkan Dua Mode Operasi Utama PIM?", "id": "PIM Dense Mode (PIM-DM), PIM Sparse Mode." },
  { "en": "Apa Itu PIM (Protocol Independent Multicast) Dense Mode?", "id": "Menggunakan Pendekatan Push, Membanjiri Lalu Lintas." },
  { "en": "Apa Itu PIM (Protocol Independent Multicast) Sparse Mode?", "id": "Menggunakan Pendekatan Pull, Hanya Jika Diminta." },
  { "en": "Apa Itu Rendezvous Point (RP) PIM-SM?", "id": "Router Titik Pertemuan Untuk Sumber Penerima." },
  { "en": "Apa Itu IGMP (Internet Group Management Protocol) Snooping?", "id": "Fitur Switch Meneruskan Lalu Lintas Multicast." },
  { "en": "Apa Itu Authentication, Authorization, and Accounting (AAA)?", "id": "Kerangka Keamanan Mengontrol Akses Mengaudit Tindakan." },
  { "en": "Protokol Apa Yang Sering Digunakan AAA?", "id": "RADIUS (Remote Authentication Dial-In User Service) dan TACACS+." },
  { "en": "Apa Kepanjangan TACACS+ (Terminal Access Controller Access-Control)?", "id": "TACACS+ Adalah Terminal Access Controller Access-Control System." },
  { "en": "Apa Perbedaan Utama RADIUS Dan TACACS+?", "id": "TACACS+ Mengenkripsi Seluruh Paket, Memisahkan Fungsi." },
  { "en": "Apa Itu Kernel Dalam Sebuah Sistem Operasi?", "id": "Inti Sistem Operasi, Menjembatani Hardware Software." },
  { "en": "Apa Itu Shell Dalam Sebuah Sistem Operasi?", "id": "Program Antarmuka Pengguna Ke Layanan Kernel." },
  { "en": "Apa Itu Process ID (PID)?", "id": "Nomor Unik Diberikan Sistem Operasi Proses." },
  { "en": "Apa Itu Thread Dalam Komputasi Paralel?", "id": "Unit Eksekusi Terkecil Dalam Suatu Proses." },
  { "en": "Apa Itu Deadlock Dalam Sistem Operasi?", "id": "Dua Proses Saling Menunggu Sumber Daya." },
  { "en": "Apa Itu Semaphore Dalam Sinkronisasi Proses?", "id": "Variabel Untuk Mengontrol Akses Sumber Daya." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Sinkronisasi Memastikan Satu Thread Akses." },
  { "en": "Apa Itu Virtual Memory (Memori Virtual)?", "id": "Teknik Manajemen Memori Memberi Ilusi Memori." },
  { "en": "Apa Itu Page Fault Dalam Virtual Memory?", "id": "Interupsi Saat Program Mengakses Halaman Tidak." },
  { "en": "Apa Itu Internet Control Message Protocol (ICMP)?", "id": "Protokol Pesan Kontrol Untuk Jaringan IP." },
  { "en": "Apa Itu End-to-End Principle Desain Jaringan?", "id": "Fungsi Spesifik Aplikasi Diletakkan Di Ujung." },
  { "en": "Apa Itu Application Layer Gateway (ALG)?", "id": "Komponen Memodifikasi Payload Aplikasi Untuk NAT." },
  { "en": "Apa Itu Next-Generation Firewall (NGFW)?", "id": "Firewall Dengan Fitur Inspeksi Mendalam Kesadaran." },
  { "en": "Apa Itu Unified Threat Management (UTM)?", "id": "Perangkat Keamanan Tunggal Menggabungkan Beberapa Fungsi." },
  { "en": "Apa Itu Security Information and Event Management (SIEM)?", "id": "Sistem Mengumpulkan Menganalisis Data Log Keamanan." },
  { "en": "Apa Itu Honeypot Dalam Keamanan Jaringan?", "id": "Sistem Umpan Menarik Mengalihkan Perhatian Penyerang." },
  { "en": "Apa Itu Sandbox Dalam Keamanan Malware?", "id": "Lingkungan Terisolasi Untuk Menjalankan Kode Mencurigakan." },
  { "en": "Apa Itu Threat Intelligence (Intelijen Ancaman)?", "id": "Informasi Tentang Ancaman Siber Yang Terkini." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
