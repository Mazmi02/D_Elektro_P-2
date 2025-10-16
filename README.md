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


  { "en": "Apa Itu Stabilitas Sinyal Kecil (Small-Signal Stability)?", "id": "Kemampuan Sistem Stabil Setelah Gangguan Kecil." },
  { "en": "Apa Itu Bus Ayun (Swing Bus) Atau Slack Bus?", "id": "Bus Referensi Dalam Analisis Aliran Daya." },
  { "en": "Apa Fungsi Slack Bus?", "id": "Menyeimbangkan Pembangkitan Dengan Beban Dan Kerugian." },
  { "en": "Apa Itu Kurva Kemampuan Generator?", "id": "Menunjukkan Batas Operasi Aman Generator (MW, MVAR)." },
  { "en": "Apa Itu Zona Proteksi Relai?", "id": "Area Spesifik Dilindungi Oleh Relai Tertentu." },
  { "en": "Apa Itu FIFO (First-In-First-Out) Asinkron?", "id": "FIFO Transfer Data Antara Domain Clock Asinkron." },
  { "en": "Mengapa Kode Gray Digunakan Untuk CDC (Clock Domain Crossing)?", "id": "Hanya Satu Bit Berubah, Mengurangi Error." },
  { "en": "Apa Keuntungan Pengkodean One-Hot Untuk FSM?", "id": "Logika Sederhana, Potensi Kecepatan Lebih Tinggi." },
  { "en": "Kapan Dioda Schottky Lebih Baik Dari Dioda PN?", "id": "Aplikasi Switching Cepat Dan Tegangan Rendah." },
  { "en": "Apa Kepanjangan TFET (Tunneling Field-Effect Transistor)?", "id": "Transistor Efek Medan Terowongan." },
  { "en": "Apa Keuntungan Potensial TFET (Tunneling Field-Effect Transistor)?", "id": "Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Itu Lingkaran Resistansi Konstan Di Smith Chart?", "id": "Lingkaran Tempat Semua Titik Resistansi Sama." },
  { "en": "Apa Itu Lingkaran Reaktansi Konstan Di Smith Chart?", "id": "Busur Tempat Semua Titik Reaktansi Sama." },
  { "en": "Apa Itu Balun Guanella?", "id": "Jenis Balun Jalur Transmisi Rasio Impedansi 1:1." },
  { "en": "Apa Itu Balun Marchand?", "id": "Jenis Balun Planar Kompak Sirkuit Microwave." },
  { "en": "Apa Itu Metode Kurva Reaksi Ziegler-Nichols?", "id": "Metode Tuning PID Berdasarkan Respon Loop Terbuka." },
  { "en": "Apa Itu Anti-Windup Pada Kontroler PID?", "id": "Mencegah Akumulasi Berlebih Istilah Integral Saat Saturasi." },
  { "en": "Apa Itu Penjadwalan Penguatan (Gain Scheduling)?", "id": "Menyesuaikan Parameter Kontroler Berdasarkan Kondisi Operasi." },
  { "en": "Apa Itu Potensial Vektor Magnetik?", "id": "Besaran Vektor Yang Curl-nya Medan Magnet." },
  { "en": "Apa Itu Potensial Skalar Listrik?", "id": "Besaran Skalar Yang Gradiennya Medan Listrik." },
  { "en": "Bagaimana Propagasi Gelombang Di Media Lossless?", "id": "Amplitudo Tetap Konstan Seiring Jarak." },
  { "en": "Bagaimana Propagasi Gelombang Di Media Lossy?", "id": "Amplitudo Melemah (Atenuasi) Seiring Jarak." },
  { "en": "Apa Itu Amplifier Diferensial Penuh (Fully Differential)?", "id": "Amplifier Dengan Input Dan Output Diferensial." },
  { "en": "Apa Keuntungan Amplifier Diferensial Penuh?", "id": "Penolakan Derau Common-Mode Sangat Baik." },
  { "en": "Bagaimana Struktur Amplifier Instrumentasi 3-Op-Amp?", "id": "Dua Buffer Input Diikuti Amplifier Diferensial." },
  { "en": "Apa Itu Topologi Kapasitor Terbang (Flying Capacitor)?", "id": "Teknik Penyeimbangan Tegangan Inverter Multilevel." },
  { "en": "Bagaimana Cara Mengukur Derau Fasa (Phase Noise)?", "id": "Menggunakan Spectrum Analyzer Atau Phase Noise Analyzer." },
  { "en": "Apa Itu Jitter Periode (Period Jitter)?", "id": "Deviasi Periode Clock Dari Nilai Rata-ratanya." },
  { "en": "Apa Itu Jitter Siklus-ke-Siklus (Cycle-to-Cycle Jitter)?", "id": "Perbedaan Maksimum Antara Dua Periode Clock Berurutan." },
  { "en": "Apa Itu Time-Domain Reflectometry (TDR)?", "id": "Teknik Karakterisasi Impedansi Saluran Transmisi." },
  { "en": "Apa Itu Keselamatan Intrinsik (Intrinsic Safety)?", "id": "Teknik Desain Pencegah Percikan Api, Panas Berlebih." },
  { "en": "Di Mana Keselamatan Intrinsik (Intrinsic Safety) Penting?", "id": "Lingkungan Berbahaya (Minyak, Gas, Tambang)." },
  { "en": "Apa Beda Ferit MnZn Dan NiZn?", "id": "MnZn Frekuensi Rendah, NiZn Frekuensi Tinggi." },
  { "en": "Apa Itu Konduktivitas Termal?", "id": "Kemampuan Suatu Material Menghantarkan Panas." },
  { "en": "Apa Satuan Konduktivitas Termal?", "id": "Watt Per Meter-Kelvin (W/mK)." },
  { "en": "Apa Itu Koefisien Ekspansi Termal (CTE)?", "id": "Ukuran Perubahan Ukuran Material Terhadap Suhu." },
  { "en": "Mengapa CTE (Coefficient of Thermal Expansion) Penting Dalam PCB?", "id": "Perbedaan CTE Dapat Menyebabkan Stres Mekanis." },
  { "en": "Apa Itu Pensinyalan Single-Ended?", "id": "Sinyal Ditransmisikan Terhadap Referensi Ground Bersama." },
  { "en": "Apa Itu Low-Voltage Differential Signaling (LVDS)?", "id": "Standar Sinyal Diferensial Kecepatan Tinggi, Daya Rendah." },
  { "en": "Apa Itu Current Mode Logic (CML)?", "id": "Antarmuka Sinyal Diferensial Berbasis Arus." },
  { "en": "Apa Itu Emitter-Coupled Logic (ECL)?", "id": "Keluarga Logika Bipolar Kecepatan Sangat Tinggi." },
  { "en": "Apa Karakteristik Utama ECL (Emitter-Coupled Logic)?", "id": "Tidak Pernah Saturasi, Mengurangi Penundaan Switching." },
  { "en": "Apa Itu Pipelining?", "id": "Tumpang Tindih Eksekusi Instruksi Kinerja Lebih Tinggi." },
  { "en": "Apa Itu Arsitektur Superskalar?", "id": "Prosesor Dapat Mengeksekusi Lebih Satu Instruksi." },
  { "en": "Apa Itu Eksekusi Di Luar Urutan (Out-of-Order)?", "id": "Eksekusi Berdasarkan Ketersediaan Data, Bukan Urutan." },
  { "en": "Apa Itu Prediksi Percabangan (Branch Prediction)?", "id": "Teknik Menebak Arah Percabangan Efisiensi Pipeline." },
  { "en": "Apa Itu Eksekusi Spekulatif?", "id": "Mengeksekusi Instruksi Sebelum Diketahui Apakah Diperlukan." },
  { "en": "Apa Itu Very Long Instruction Word (VLIW)?", "id": "Arsitektur Prosesor Pengemas Beberapa Operasi." },
  { "en": "Apa Itu Single Instruction, Multiple Data (SIMD)?", "id": "Mengeksekusi Instruksi Sama Pada Banyak Data." },
  { "en": "Apa Itu Cache Miss?", "id": "Kondisi Data Diminta Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Hit?", "id": "Kondisi Data Diminta Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Line (Block)?", "id": "Unit Transfer Data Terkecil Antara Cache, Memori." },
  { "en": "Apa Itu Memory Management Unit (MMU)?", "id": "Menerjemahkan Alamat Virtual Ke Alamat Fisik." },
  { "en": "Apa Itu Memori Virtual?", "id": "Teknik Menggunakan Penyimpanan Sekunder Sebagai Memori Utama." },
  { "en": "Apa Itu Page Fault?", "id": "Terjadi Saat Program Mengakses Halaman Tidak Di Memori." },
  { "en": "Apa Itu Translation Lookaside Buffer (TLB)?", "id": "Cache Penyimpan Terjemahan Alamat Terbaru." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Byte Representasi Data Multi-byte." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Di Alamat Terendah." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Di Alamat Terendah." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Kode Deteksi Error Yang Umum Digunakan." },
  { "en": "Apa Itu Jarak Hamming?", "id": "Jumlah Posisi Bit Berbeda Antara Dua String." },
  { "en": "Apa Itu Kode Reed-Solomon?", "id": "Kode Koreksi Error Kuat Melawan Burst Error." },
  { "en": "Apa Itu Kode Turbo?", "id": "Kode Koreksi Error Kinerja Tinggi Mendekati Batas Shannon." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Koreksi Error Kinerja Tinggi Lainnya." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Protokol Kontrol Error Menggunakan Transmisi Ulang." },
  { "en": "Sebutkan Jenis Protokol ARQ?", "id": "Stop-and-Wait, Go-Back-N, Selective Repeat." },
  { "en": "Apa Itu Slotted ALOHA?", "id": "Protokol Akses Acak Terjadwal Untuk Jaringan." },
  { "en": "Apa Itu Carrier Sense Multiple Access (CSMA)?", "id": "Protokol Mendengarkan Kanal Sebelum Transmisi." },
  { "en": "Apa Itu CSMA/CD (Collision Detection)?", "id": "CSMA Dengan Kemampuan Mendeteksi Tabrakan (Ethernet)." },
  { "en": "Apa Itu CSMA/CA (Collision Avoidance)?", "id": "CSMA Dengan Kemampuan Menghindari Tabrakan (Wi-Fi)." },
  { "en": "Apa Itu Pengkodean Manchester?", "id": "Skema Pengkodean Garis Yang Menyertakan Sinyal Clock." },
  { "en": "Apa Itu Pengkodean Non-Return-to-Zero (NRZ)?", "id": "Skema Pengkodean Garis Sederhana." },
  { "en": "Apa Itu Pengkodean 4B/5B?", "id": "Mengkodekan 4 Bit Data Menjadi 5 Bit Kode." },
  { "en": "Apa Tujuan Pengkodean 4B/5B?", "id": "Menjamin Transisi Sinyal Untuk Pemulihan Clock." },
  { "en": "Apa Itu Diagram Mata?", "id": "Alat Visual Analisis Kualitas Sinyal Komunikasi Digital." },
  { "en": "Apa Itu Intersymbol Interference (ISI)?", "id": "Distorsi Simbol Akibat Simbol Sebelumnya." },
  { "en": "Apa Penyebab Intersymbol Interference (ISI)?", "id": "Respon Frekuensi Kanal Yang Tidak Ideal." },
  { "en": "Apa Itu Filter Raised-Cosine?", "id": "Filter Pembentuk Pulsa Untuk Mengurangi ISI." },
  { "en": "Apa Fungsi Manik Ferit (Ferrite Bead)?", "id": "Komponen Pasif Penekan Derau Frekuensi Tinggi." },
  { "en": "Bagaimana Cara Kerja Manik Ferit?", "id": "Bertindak Seperti Induktor Pada Frekuensi Tinggi." },
  { "en": "Apa Fungsi Common-Mode Choke?", "id": "Menekan Derau Common-Mode Tanpa Mempengaruhi Sinyal Diferensial." },
  { "en": "Apa Itu Kapasitor Y (Y-Capacitor)?", "id": "Kapasitor Terhubung Antara Jalur Fasa/Netral, Ground." },
  { "en": "Apa Itu Kapasitor X (X-Capacitor)?", "id": "Kapasitor Terhubung Antara Jalur Fasa, Netral." },
  { "en": "Apa Fungsi Kapasitor X Dan Y?", "id": "Menyaring Gangguan EMI Pada Jalur Listrik." },
  { "en": "Apa Itu Pembatas Arus Mula?", "id": "Rangkaian Pembatas Lonjakan Arus Saat Dinyalakan." },
  { "en": "Apa Tipe Umum Pembatas Arus Mula?", "id": "Termistor NTC Atau Relai Bypass." },
  { "en": "Apa Fungsi Rangkaian Snubber?", "id": "Melindungi Sakelar Elektronik Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Rangkaian Active Clamp?", "id": "Jenis Snubber Aktif Menggunakan Transistor." },
  { "en": "Apa Itu Konverter Kuasi-Resonan?", "id": "Konverter Switching Yang Beralih Dekat Titik Nol." },
  { "en": "Apa Itu Kontrol Histeresis?", "id": "Metode Kontrol Menjaga Variabel Dalam Batas." },
  { "en": "Apa Itu Kontrol Bang-Bang?", "id": "Jenis Kontroler Dua Keadaan (On/Off)." },
  { "en": "Apa Itu Sliding Mode Control (SMC)?", "id": "Teknik Kontrol Non-linear Yang Kuat." },
  { "en": "Apa Itu Saturasi (Saturasi) Pada Aktor?", "id": "Kondisi Output Aktor Mencapai Batas Maksimum." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Sinyal Naik 10% Hingga 90% Nilai Akhir." },
  { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Sinyal Turun 90% Hingga 10% Nilai Awal." },
  { "en": "Apa Itu Lewatan (Overshoot)?", "id": "Output Melebihi Nilai Akhir Yang Diinginkan." },
  { "en": "Apa Itu Undershoot?", "id": "Output Turun Di Bawah Nilai Akhir Yang Diinginkan." },
  { "en": "Apa Itu Deringan (Ringing)?", "id": "Osilasi Sinyal Di Sekitar Nilai Akhir." },
  { "en": "Apa Itu Waktu Tunak (Settling Time)?", "id": "Waktu Yang Dibutuhkan Sinyal Untuk Stabil." },
  { "en": "Apa Itu Penundaan Grup (Group Delay)?", "id": "Ukuran Penundaan Rata-rata Sinyal Melalui Sistem." },
  { "en": "Apa Itu Deteksi Islanding Pasif?", "id": "Mendeteksi Perubahan Parameter Jaringan Lokal." },
  { "en": "Apa Itu Deteksi Islanding Aktif?", "id": "Menginjeksikan Gangguan Kecil Untuk Mendeteksi Respon." },
  { "en": "Apa Fungsi STATCOM (Static Synchronous Compensator) Dalam Jaringan?", "id": "Menyediakan Kompensasi Daya Reaktif Dinamis Cepat." },
  { "en": "Apa Itu Algoritma Lokasi Gangguan?", "id": "Metode Menentukan Lokasi Fisik Gangguan Jaringan." },
  { "en": "Apa Itu Testbench Dalam Verifikasi Perangkat Keras?", "id": "Lingkungan Simulasi Untuk Menguji Desain." },
  { "en": "Apa Itu Assertion Dalam Verifikasi?", "id": "Pernyataan Properti Desain Yang Harus Selalu Benar." },
  { "en": "Apa Itu Verifikasi Formal?", "id": "Menggunakan Pembuktian Matematis Untuk Memverifikasi Desain." },
  { "en": "Apa Beda Simulasi Dan Sintesis?", "id": "Simulasi Memverifikasi, Sintesis Menciptakan Implementasi Gerbang." },
  { "en": "Apa Efek Injeksi Pembawa Panas (Hot Carrier Injection)?", "id": "Degradasi Kinerja Transistor Seiring Waktu." },
  { "en": "Apa Kepanjangan NBTI (Negative-Bias Temperature Instability)?", "id": "Ketidakstabilan Suhu Bias Negatif." },
  { "en": "Apa Sifat Unik Pembagi Daya Wilkinson?", "id": "Tiga Port Saling Sesuai Dan Terisolasi." },
  { "en": "Kapan Polarisasi Sirkular Digunakan?", "id": "Komunikasi Satelit, GPS, Mengurangi Efek Multipath." },
  { "en": "Apa Itu Aturan Lingkaran Plot Nyquist?", "id": "Menentukan Stabilitas Berdasarkan Lingkaran Titik -1." },
  { "en": "Apa Itu Penempatan Kutub (Pole Placement)?", "id": "Teknik Desain Kontroler Menempatkan Kutub Loop Tertutup." },
  { "en": "Apa Syarat Batas Medan E Di Antara Konduktor?", "id": "Komponen Tangensial Medan E Adalah Nol." },
  { "en": "Apa Syarat Batas Medan B Di Antara Konduktor?", "id": "Komponen Normal Medan B Adalah Nol." },
  { "en": "Apa Itu Potensial Skalar Magnetik?", "id": "Besaran Skalar Untuk Menghitung Medan Magnet." },
  { "en": "Apa Itu Difusi Magnetik?", "id": "Proses Penetrasi Medan Magnet Ke Dalam Konduktor." },
  { "en": "Apa Itu Umpan Balik Common-Mode (CMFB)?", "id": "Rangkaian Penstabil Tegangan Output Common-Mode." },
  { "en": "Di Mana CMFB (Common-Mode Feedback) Digunakan?", "id": "Dalam Amplifier Diferensial Penuh." },
  { "en": "Apa Itu Filter Kapasitor Bertukar?", "id": "Filter Aktif Menggunakan Kapasitor Bertukar, Op-Amp." },
  { "en": "Apa Keuntungan Filter Kapasitor Bertukar?", "id": "Konstanta Waktu Presisi, Cocok Untuk IC." },
  { "en": "Apa Beda TDR (Time-Domain Reflectometry) Dan TDT?", "id": "TDR Mengukur Pantulan, TDT Mengukur Transmisi." },
  { "en": "Kapan Logic Analyzer Lebih Baik Dari Osiloskop?", "id": "Menganalisis Banyak Sinyal Digital Secara Bersamaan." },
  { "en": "Kapan Network Analyzer Lebih Baik Dari Spectrum Analyzer?", "id": "Menganalisis Karakteristik Respon Frekuensi Lengkap." },
  { "en": "Apa Kepanjangan ASIL (Automotive Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan Otomotif." },
  { "en": "Apa Kelas Isolasi Termal Tertinggi?", "id": "Kelas C (Diatas 240°C)." },
  { "en": "Apa Itu Magnet Alnico?", "id": "Magnet Permanen Dari Aluminium, Nikel, Kobalt." },
  { "en": "Apa Itu Magnet Neodymium?", "id": "Magnet Tanah Jarang Paling Kuat Yang Tersedia." },
  { "en": "Apa Itu Magnet Samarium-Kobalt?", "id": "Magnet Tanah Jarang Tahan Suhu Tinggi." },
  { "en": "Apa Beda Ferit Keras Dan Lunak?", "id": "Keras Untuk Magnet Permanen, Lunak Inti Trafo." },
  { "en": "Apa Itu Koersivitas (Coercivity)?", "id": "Resistansi Material Magnetik Terhadap Demagnetisasi." },
  { "en": "Apa Itu Remanensi (Remanence)?", "id": "Sisa Magnetisasi Dalam Material Magnetik." },
  { "en": "Apa Itu Produk Energi Maksimum (BHmax)?", "id": "Ukuran Kekuatan Sebuah Magnet Permanen." },
  { "en": "Apa Itu Efek Barkhausen?", "id": "Derau Akibat Perubahan Diskret Domain Magnetik." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Daerah Kecil Dengan Momen Magnetik Seragam." },
  { "en": "Apa Itu Dinding Domain (Domain Wall)?", "id": "Batas Antara Domain Magnetik Yang Berdekatan." },
  { "en": "Apa Itu Magnetisasi Saturasi?", "id": "Kondisi Magnetisasi Maksimum Suatu Material." },
  { "en": "Apa Itu Permeabilitas Relatif?", "id": "Rasio Permeabilitas Material Terhadap Ruang Hampa." },
  { "en": "Apa Itu Konstanta Dielektrik Relatif?", "id": "Rasio Permitivitas Material Terhadap Ruang Hampa." },
  { "en": "Apa Fungsi Kontroler DMA (Direct Memory Access)?", "id": "Mengelola Transfer Data Langsung Antara Memori, Periferal." },
  { "en": "Apa Fungsi PIC (Programmable Interrupt Controller)?", "id": "Mengelola Interupsi Dari Beberapa Perangkat Ke CPU." },
  { "en": "Apa Fungsi PPI (Programmable Peripheral Interface)?", "id": "IC Untuk Antarmuka Paralel (Contoh: Intel 8255)." },
  { "en": "Apa Itu USART (Universal Synchronous/Asynchronous Receiver/Transmitter)?", "id": "Periferal Komunikasi Serial Fleksibel." },
  { "en": "Apa Itu Protokol SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron Empat Kawat." },
  { "en": "Apa Saja Sinyal Dalam SPI?", "id": "SCLK, MOSI, MISO, SS/CS." },
  { "en": "Apa Itu Protokol I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Serial Dua Kawat." },
  { "en": "Apa Saja Sinyal Dalam I2C?", "id": "SDA (Data) Dan SCL (Clock)." },
  { "en": "Apa Itu Debouncing Sakelar?", "id": "Proses Menghilangkan Transisi Palsu Sakelar Mekanis." },
  { "en": "Bagaimana Cara Debouncing Perangkat Keras?", "id": "Menggunakan Rangkaian RC Atau Schmitt Trigger." },
  { "en": "Bagaimana Cara Debouncing Perangkat Lunak?", "id": "Menggunakan Penundaan Dan Pemeriksaan Ulang." },
  { "en": "Apa Itu Sakelar Putar (Rotary Switch)?", "id": "Sakelar Banyak Posisi Dipilih Dengan Memutar." },
  { "en": "Apa Itu Papan Tombol Matriks (Matrix Keypad)?", "id": "Susunan Tombol Dalam Grid Baris, Kolom." },
  { "en": "Bagaimana Cara Membaca Papan Tombol Matriks?", "id": "Dengan Melakukan Scanning Baris, Membaca Kolom." },
  { "en": "Apa Itu Output Open-Drain?", "id": "Mirip Open-Collector Tapi Menggunakan MOSFET." },
  { "en": "Apa Itu Output Push-Pull?", "id": "Output Menggunakan Dua Transistor (Tinggi, Rendah)." },
  { "en": "Apa Keuntungan Output Push-Pull?", "id": "Dapat Secara Aktif Mendorong Sinyal Tinggi, Rendah." },
  { "en": "Apa Itu Antifuse?", "id": "Elemen Programmable Awalnya Terbuka, Lalu Terhubung." },
  { "en": "Di Mana Antifuse Digunakan?", "id": "Dalam Beberapa Jenis FPGA Dan PROM." },
  { "en": "Apa Itu Transistor Gerbang Mengambang?", "id": "Transistor Penyimpan Muatan Memori Non-Volatile." },
  { "en": "Apa Itu SONOS (Silicon-Oxide-Nitride-Oxide-Silicon)?", "id": "Teknologi Memori Flash Alternatif." },
  { "en": "Apa Itu Wear Leveling?", "id": "Teknik Distribusi Penulisan Merata Di Memori Flash." },
  { "en": "Apa Tujuan Wear Leveling?", "id": "Meningkatkan Umur Pakai Memori Flash." },
  { "en": "Apa Itu Garbage Collection Dalam SSD?", "id": "Proses Mengatur Ulang Blok Data Penulisan Baru." },
  { "en": "Apa Kepanjangan SSD (Solid-State Drive)?", "id": "Penggerak Keadaan Padat." },
  { "en": "Apa Itu Perintah Trim Dalam SSD?", "id": "Memberitahu Drive Blok Mana Tidak Lagi Digunakan." },
  { "en": "Apa Itu Over-Provisioning Dalam SSD?", "id": "Menyisihkan Kapasitas Ekstra Peningkatan Kinerja, Umur." },
  { "en": "Apa Itu Sistem Berkas (File System)?", "id": "Struktur Pengorganisasian Data Di Media Penyimpanan." },
  { "en": "Apa Itu Aliran Daya DC (Direct Current)?", "id": "Model Sederhana Aliran Daya Jaringan Listrik." },
  { "en": "Apa Itu Aliran Daya Optimal (OPF)?", "id": "Menemukan Pengoperasian Sistem Tenaga Paling Optimal." },
  { "en": "Apa Itu Estimasi Keadaan (State Estimation) Sistem Tenaga?", "id": "Mengestimasi Keadaan Jaringan Berdasarkan Pengukuran Berlebih." },
  { "en": "Apa Itu Analisis Kontingensi?", "id": "Menganalisis Dampak Kegagalan Komponen Sistem Tenaga." },
  { "en": "Apa Itu Sistem Proteksi Gardu Induk?", "id": "Relai, Pemutus Sirkuit Pelindung Peralatan." },
  { "en": "Apa Itu Proteksi Diferensial Bus?", "id": "Skema Proteksi Cepat Busbar Gardu Induk." },
  { "en": "Apa Itu Proteksi Diferensial Arus Saluran?", "id": "Skema Proteksi Utama Saluran Transmisi." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Relai Bekerja Saat Arus Melebihi Batas." },
  { "en": "Apa Itu Relai Berarah (Directional Relay)?", "id": "Relai Bekerja Berdasarkan Arah Aliran Gangguan." },
  { "en": "Apa Itu Relai Impedansi (Relai Jarak)?", "id": "Relai Pelindung Saluran Berdasarkan Pengukuran Impedansi." },
  { "en": "Apa Itu Sistem SCADA?", "id": "Sistem Kontrol Terpusat Jaringan Listrik." },
  { "en": "Apa Itu Energy Management System (EMS)?", "id": "Perangkat Lunak Pengoptimal Operasi Jaringan Listrik." },
  { "en": "Apa Itu Distribution Management System (DMS)?", "id": "EMS Khusus Jaringan Distribusi." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Menggunakan PMU Memantau Jaringan Skala Luas." },
  { "en": "Apa Itu Kemampuan Black Start?", "id": "Kemampuan Pembangkit Hidup Tanpa Daya Eksternal." },
  { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Kapasitas Pembangkit Sinkron Siap Naikkan Output." },
  { "en": "Apa Itu Cadangan Tidak Berputar (Non-Spinning Reserve)?", "id": "Kapasitas Pembangkit Offline Dapat Cepat Dihidupkan." },
  { "en": "Apa Itu Respon Frekuensi (Sistem Tenaga)?", "id": "Respon Alami Jaringan Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Kontrol Frekuensi Primer (Kontrol Governor)?", "id": "Respon Otomatis Pembangkit Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Kontrol Frekuensi Sekunder (AGC)?", "id": "Mengembalikan Frekuensi Ke Nilai Nominal Otomatis." },
  { "en": "Apa Itu Kontrol Frekuensi Tersier?", "id": "Penjadwalan Ulang Pembangkit Secara Manual/Otomatis." },
  { "en": "Apa Itu Islanding?", "id": "Bagian Jaringan Terisolasi Tapi Tetap Berenergi." },
  { "en": "Apa Itu Kontrol Droop P-f?", "id": "Menurunkan Frekuensi Referensi Saat Daya Aktif Naik." },
  { "en": "Apa Itu Kontrol Droop Q-V?", "id": "Menurunkan Tegangan Referensi Saat Daya Reaktif Naik." },
  { "en": "Apa Itu Synchronverter?", "id": "Inverter Yang Meniru Perilaku Generator Sinkron." },
  { "en": "Apa Itu Sistem Fasa Tunggal Tiga Kawat?", "id": "Sistem Distribusi Umum Untuk Perumahan." },
  { "en": "Apa Itu Sistem Daya Fasa Terpisah (Split-Phase)?", "id": "Nama Lain Sistem Fasa Tunggal Tiga Kawat." },
  { "en": "Apa Itu Sistem High-Leg Delta?", "id": "Sistem Tiga Fasa Satu Fasa Tegangan Tinggi." },
  { "en": "Apa Itu Sistem Corner-Grounded Delta?", "id": "Sistem Delta Salah Satu Fasa Ditanahkan." },
  { "en": "Apa Itu Sistem Tidak Ditanahkan (Ungrounded)?", "id": "Sistem Tenaga Tanpa Koneksi Intensional Ke Tanah." },
  { "en": "Apa Itu Sistem Ditanahkan Solid (Solidly Grounded)?", "id": "Titik Netral Terhubung Langsung Ke Tanah." },
  { "en": "Apa Itu Sistem Ditanahkan Tahanan (Resistance Grounded)?", "id": "Titik Netral Terhubung Tanah Melalui Resistor." },
  { "en": "Apa Itu Jaringan Urutan Positif?", "id": "Representasi Sistem Tenaga Komponen Urutan Positif." },
  { "en": "Apa Itu Jaringan Urutan Negatif?", "id": "Representasi Sistem Tenaga Komponen Urutan Negatif." },
  { "en": "Apa Itu Jaringan Urutan Nol?", "id": "Representasi Sistem Tenaga Komponen Urutan Nol." },
  { "en": "Apa Itu Impedansi Gangguan (Fault Impedance)?", "id": "Impedansi Di Titik Terjadinya Gangguan Listrik." },
  { "en": "Apa Itu Kapasitas Pemutusan (Interrupting Capacity) Pemutus Sirkuit?", "id": "Arus Gangguan Maksimum Dapat Diputuskan Aman." },
  { "en": "Apa Fungsi Grid Pentanahan (Grounding Grid) Gardu Induk?", "id": "Menyamakan Potensial Tanah Saat Terjadi Gangguan." },
  { "en": "Apa Kepanjangan MTBF (Mean Time Between Failures) Metastabilitas?", "id": "Waktu Rata-Rata Antar Kegagalan Akibat Metastabilitas." },
  { "en": "Apa Itu Penutupan Waktu (Timing Closure)?", "id": "Proses Memastikan Desain Digital Memenuhi Batasan Waktu." },
  { "en": "Apa Itu Jitter Deterministik?", "id": "Jitter Dengan Pola Berulang, Penyebab Teridentifikasi." },
  { "en": "Apa Itu Jitter Acak (Random Jitter)?", "id": "Jitter Tanpa Pola Akibat Proses Acak." },
  { "en": "Apa Itu Efek Kirk?", "id": "Perluasan Daerah Basis Penurun Penguatan Arus." },
  { "en": "Apa Itu Tembus-Suntik (Punch-through) Transistor Bipolar?", "id": "Daerah Deplesi Kolektor Mencapai Emitor." },
  { "en": "Apa Itu Ionisasi Tumbukan (Impact Ionization)?", "id": "Pembawa Muatan Berenergi Tinggi Hasilkan Pasangan Elektron-Lubang." },
  { "en": "Apa Itu Efisiensi Kuantum (Quantum Efficiency) Fotodetektor?", "id": "Rasio Elektron Terkumpul Terhadap Foton Masuk." },
  { "en": "Apa Hubungan Noise Figure Dan Noise Factor?", "id": "Noise Figure Adalah Noise Factor Dalam Decibel (dB)." },
  { "en": "Bagaimana Titik Intersep IP3 Bertingkat Dihitung?", "id": "Menggunakan Rumus Kombinasi Linearitas Bertingkat." },
  { "en": "Apa Keuntungan Stripline Dibanding Microstrip?", "id": "Isolasi Lebih Baik, Kebocoran Radiasi Rendah." },
  { "en": "Apa Itu Rasio Aksial (Axial Ratio) Antena?", "id": "Ukuran Kemurnian Polarisasi Sirkular Suatu Antena." },
  { "en": "Apa Syarat Stabilitas Kriteria Routh-Hurwitz?", "id": "Semua Elemen Kolom Pertama Tabel Routh Positif." },
  { "en": "Apa Fungsi Kompensator Lead?", "id": "Memperbaiki Respon Transien, Margin Fasa." },
  { "en": "Apa Fungsi Kompensator Lag?", "id": "Mengurangi Error Keadaan Tunak (Steady-State Error)." },
  { "en": "Apa Itu Sambungan Fluks (Flux Linkage)?", "id": "Total Fluks Magnet Yang Melingkupi Kumparan." },
  { "en": "Apa Satuan Sambungan Fluks?", "id": "Weber-Lilit (Weber-Turns)." },
  { "en": "Energi Apa Yang Tersimpan Dalam Induktor?", "id": "Energi Medan Magnet." },
  { "en": "Energi Apa Yang Tersimpan Dalam Kapasitor?", "id": "Energi Medan Listrik." },
  { "en": "Apa Masalah Stabilitas Amplifier Umpan Arus (CFA)?", "id": "Sensitif Terhadap Kapasitansi Umpan Balik." },
  { "en": "Apa Keuntungan Utama Amplifier Cascode?", "id": "Bandwidth Lebih Lebar Akibat Mengurangi Efek Miller." },
  { "en": "Apa Itu Cermin Arus Dengan Beban Aktif?", "id": "Pasangan Diferensial Menggunakan Cermin Arus Sebagai Beban." },
  { "en": "Apa Beda FFT Osiloskop Dan Spectrum Analyzer?", "id": "Spectrum Analyzer Memiliki Rentang Dinamis Lebih Baik." },
  { "en": "Mengapa Pengukuran Empat Kawat (Kelvin) Penting?", "id": "Akurat Untuk Mengukur Resistansi Sangat Rendah." },
  { "en": "Apa Beda Guarding Dan Shielding?", "id": "Shielding Memblokir, Guarding Mengalihkan Arus Bocor." },
  { "en": "Apa Itu Batas Busur Api (Arc Flash Boundary)?", "id": "Jarak Aman Potensi Luka Bakar Busur Api." },
  { "en": "Apa Itu Batas Pendekatan Terbatas (Limited Approach Boundary)?", "id": "Jarak Aman Untuk Orang Tanpa Kualifikasi." },
  { "en": "Apa Itu Batas Pendekatan Terlarang (Restricted Approach Boundary)?", "id": "Jarak Aman Hanya Untuk Orang Berkualifikasi." },
  { "en": "Berapa Konstanta Dielektrik Relatif Udara?", "id": "Sekitar Satu (1.0006)." },
  { "en": "Berapa Konstanta Dielektrik Relatif FR-4?", "id": "Sekitar Empat Hingga Lima (4-5)." },
  { "en": "Berapa Resistivitas Tembaga (Copper)?", "id": "Sekitar 1.68 x 10⁻⁸ Ohm-meter." },
  { "en": "Berapa Resistivitas Aluminium?", "id": "Sekitar 2.65 x 10⁻⁸ Ohm-meter." },
  { "en": "Apa Itu Sakelar SPDT (Single-Pole Double-Throw)?", "id": "Sakelar Dengan Satu Input, Dua Output." },
  { "en": "Apa Itu Sakelar DPDT (Double-Pole Double-Throw)?", "id": "Dua Sakelar SPDT Digerakkan Bersamaan." },
  { "en": "Apa Itu Sakelar Normal Terbuka (Normally Open)?", "id": "Sakelar Terbuka Saat Tidak Ditekan." },
  { "en": "Apa Itu Sakelar Normal Tertutup (Normally Closed)?", "id": "Sakelar Tertutup Saat Tidak Ditekan." },
  { "en": "Apa Itu Rangkaian Pengunci (Latching Circuit)?", "id": "Rangkaian Penahan Keadaan Setelah Pemicu Dihilangkan." },
  { "en": "Bagaimana Membuat Rangkaian Pengunci Dengan Relay?", "id": "Menggunakan Kontak Relay Menahan Koilnya Sendiri." },
  { "en": "Apa Itu Dioda Clamp?", "id": "Dioda Pencegah Tegangan Melebihi Level Tertentu." },
  { "en": "Apa Itu Regulator Shunt Dioda Zener?", "id": "Regulator Tegangan Sederhana Menggunakan Dioda Zener." },
  { "en": "Apa Itu Penyearah Jembatan (Bridge Rectifier)?", "id": "Empat Dioda Pengubah AC-DC Gelombang Penuh." },
  { "en": "Apa Itu Trafo Center-Tapped?", "id": "Transformator Lilitan Sekunder Memiliki Sambungan Tengah." },
  { "en": "Bagaimana Penyearah Gelombang Penuh Dengan Trafo Center-Tapped?", "id": "Hanya Membutuhkan Dua Dioda." },
  { "en": "Apa Itu Tegangan Riak (Ripple Voltage)?", "id": "Variasi Tegangan AC Kecil Pada Output DC." },
  { "en": "Apa Itu Faktor Riak (Ripple Factor)?", "id": "Ukuran Efektivitas Penyearah Dan Filter." },
  { "en": "Bagaimana Cara Mengurangi Tegangan Riak?", "id": "Menggunakan Kapasitor Filter Lebih Besar." },
  { "en": "Apa Itu Filter Input-Induktor?", "id": "Filter Penyearah Menggunakan Induktor Elemen Pertama." },
  { "en": "Apa Itu Filter Input-Choke?", "id": "Nama Lain Filter Input-Induktor." },
  { "en": "Apa Itu Filter Input-Kapasitor (Filter Pi)?", "id": "Filter Penyearah Dengan Kapasitor Di Inputnya." },
  { "en": "Apa Fungsi Resistor Bleeder?", "id": "Melepaskan Muatan Kapasitor Saat Daya Mati." },
  { "en": "Apa Fungsi Lain Resistor Bleeder?", "id": "Memperbaiki Regulasi Tegangan Pada Beban Rendah." },
  { "en": "Apa Itu Regulasi Tegangan?", "id": "Ukuran Seberapa Stabil Tegangan Output." },
  { "en": "Apa Itu Tegangan Tembus Dioda?", "id": "Tegangan Mundur Maksimum Sebelum Dioda Rusak." },
  { "en": "Apa Itu Rating Peak Inverse Voltage (PIV)?", "id": "Tegangan Mundur Puncak Maksimum Dioda." },
  { "en": "Apa Itu Penurunan Tegangan Maju?", "id": "Penurunan Tegangan Pada Dioda Saat Konduksi." },
  { "en": "Berapa Penurunan Tegangan Maju Dioda Silikon?", "id": "Sekitar 0.6 Hingga 0.7 Volt." },
  { "en": "Berapa Penurunan Tegangan Maju Dioda Schottky?", "id": "Sekitar 0.15 Hingga 0.45 Volt." },
  { "en": "Apa Itu Dioda Germanium?", "id": "Dioda Semikonduktor Penurunan Tegangan Maju Rendah." },
  { "en": "Berapa Penurunan Tegangan Maju Dioda Germanium?", "id": "Sekitar 0.3 Volt." },
  { "en": "Apa Itu Transistor Unijunction (UJT)?", "id": "Transistor Tiga Terminal Pemicu Thyristor." },
  { "en": "Apa Itu Programmable Unijunction Transistor (PUT)?", "id": "UJT Dengan Tegangan Puncak Dapat Diprogram." },
  { "en": "Apa Itu Opto-isolator?", "id": "Mengirim Sinyal Antar Sirkuit Terisolasi Cahaya." },
  { "en": "Apa Itu Current Transfer Ratio (CTR) Opto-isolator?", "id": "Rasio Arus Output Terhadap Arus Input." },
  { "en": "Apa Itu Sakelar Optik Berlubang?", "id": "Nama Lain Photointerrupter." },
  { "en": "Apa Itu Sensor Optik Reflektif?", "id": "Sensor Optik Pemancar, Penerima Berdampingan." },
  { "en": "Apa Contoh Sensor Optik Reflektif?", "id": "Pemindai Kode Batang (Barcode Scanner)." },
  { "en": "Apa Itu Photodarlington?", "id": "Phototransistor Konfigurasi Darlington Sensitivitas Tinggi." },
  { "en": "Dari Apa Tampilan Tujuh Segmen Dibuat?", "id": "Tujuh LED (Light Emitting Diode) Penampil Angka." },
  { "en": "Apa Itu Tampilan Matriks Titik?", "id": "Tampilan Dari Susunan LED Dalam Matriks." },
  { "en": "Apa Itu Liquid Crystal Display (LCD)?", "id": "Tampilan Menggunakan Kristal Cair Mempolarisasi Cahaya." },
  { "en": "Apa Itu Lampu Latar (Backlight) LCD?", "id": "Sumber Cahaya Di Belakang Panel Kristal Cair." },
  { "en": "Apa Beda LCD Transmisif Dan Reflektif?", "id": "Transmisif Butuh Lampu Latar, Reflektif Pakai Cahaya Sekitar." },
  { "en": "Apa Itu Vacuum Fluorescent Display (VFD)?", "id": "Tampilan Dengan Filamen, Grid, Anoda Berlapis Fosfor." },
  { "en": "Apa Itu Multiplexing Tampilan?", "id": "Mengemudikan Beberapa Digit Tampilan Secara Bergantian." },
  { "en": "Apa Keuntungan Multiplexing Tampilan?", "id": "Mengurangi Jumlah Pin I/O Yang Dibutuhkan." },
  { "en": "Apa Itu Phase Locked Loop (PLL)?", "id": "Sirkuit Umpan Balik Penyelaras Fasa Osilator." },
  { "en": "Apa Itu Lock Range PLL?", "id": "Rentang Frekuensi PLL Dapat Mempertahankan Kunci." },
  { "en": "Apa Itu Capture Range PLL?", "id": "Rentang Frekuensi PLL Dapat Memperoleh Kunci." },
  { "en": "Mana Yang Lebih Lebar, Lock Atau Capture Range?", "id": "Lock Range Selalu Lebih Lebar." },
  { "en": "Apa Itu Faktor Redaman (Damping Factor) PLL?", "id": "Menentukan Respon Transien Loop Saat Mengunci." },
  { "en": "Apa Itu Switched-Mode Power Supply (SMPS)?", "id": "Catu Daya Efisiensi Tinggi Menggunakan Sakelar Elektronik." },
  { "en": "Apa Keuntungan SMPS Dibanding Regulator Linear?", "id": "Efisiensi Jauh Lebih Tinggi, Ukuran Kecil." },
  { "en": "Apa Kerugian SMPS Dibanding Regulator Linear?", "id": "Lebih Kompleks, Menghasilkan Derau EMI." },
  { "en": "Apa Fungsi Filter Derau Pada Input SMPS?", "id": "Mencegah Derau Switching Kembali Ke Jalur Listrik." },
  { "en": "Apa Itu Topologi Inverting Buck-Boost?", "id": "Konverter Penghasil Tegangan Output Negatif." },
  { "en": "Apa Itu Topologi Konverter Ćuk?", "id": "Konverter Inverting Buck-Boost Dengan Riak Rendah." },
  { "en": "Apa Itu Penyearahan Sinkron?", "id": "Mengganti Dioda Penyearah Dengan MOSFET." },
  { "en": "Apa Keuntungan Penyearahan Sinkron?", "id": "Efisiensi Lebih Tinggi Akibat Penurunan Tegangan Rendah." },
  { "en": "Apa Itu Power Over Ethernet (PoE)?", "id": "Mengirimkan Daya Listrik Melalui Kabel Ethernet." },
  { "en": "Apa Itu Standar IEEE 802.3af?", "id": "Standar Awal Untuk Power Over Ethernet (PoE)." },
  { "en": "Apa Itu Power Sourcing Equipment (PSE)?", "id": "Perangkat Penyedia Daya Sistem PoE." },
  { "en": "Apa Itu Powered Device (PD)?", "id": "Perangkat Penerima Daya Sistem PoE." },
  { "en": "Apa Itu Resistansi Seri Ekuivalen (ESR) Kapasitor?", "id": "Resistansi Seri Efektif Internal Kapasitor." },
  { "en": "Apa Itu Induktansi Seri Ekuivalen (ESL) Kapasitor?", "id": "Induktansi Seri Efektif Internal Kapasitor." },
  { "en": "Apa Itu Tingkat Fermi (Fermi Level)?", "id": "Tingkat Energi Probabilitas Okupansi Elektron Setengah." },
  { "en": "Apa Itu Konsentrasi Pembawa Intrinsik?", "id": "Jumlah Elektron, Lubang Dalam Semikonduktor Murni." },
  { "en": "Apa Beda Semikonduktor Celah Pita Langsung, Tidak Langsung?", "id": "Langsung Memancarkan Cahaya Efisien (LED, Laser)." },
  { "en": "Sebutkan Contoh Semikonduktor Celah Pita Langsung?", "id": "Galium Arsenida (GaAs)." },
  { "en": "Sebutkan Contoh Semikonduktor Celah Pita Tidak Langsung?", "id": "Silikon (Si), Dan Germanium (Ge)." },
  { "en": "Apa Itu Amplifier Kelas D?", "id": "Amplifier Switching Efisiensi Tinggi Menggunakan PWM." },
  { "en": "Apa Fungsi Modulator Pada Amplifier Kelas D?", "id": "Mengubah Sinyal Audio Menjadi Sinyal PWM." },
  { "en": "Apa Fungsi Filter Output Pada Amplifier Kelas D?", "id": "Mengembalikan PWM Menjadi Sinyal Audio Analog." },
  { "en": "Apa Itu Pengali Pohon Wallace (Wallace Tree Multiplier)?", "id": "Skema Penjumlah Cepat Untuk Perkalian Biner." },
  { "en": "Apa Itu Penjumlah Simpan-Bawa (Carry-Save Adder)?", "id": "Penjumlah Yang Menunda Propagasi Bawaan (Carry)." },
  { "en": "Apa Fungsi Blok \"Always\" Dalam Verilog?", "id": "Menjelaskan Perilaku Sirkuit Kombinasional Atau Sekuensial." },
  { "en": "Apa Itu Mode Dominan TE10 Dalam Waveguide Persegi?", "id": "Mode Propagasi Frekuensi Terendah." },
  { "en": "Apa Itu Frekuensi Cutoff Waveguide?", "id": "Frekuensi Minimum Agar Gelombang Dapat Merambat." },
  { "en": "Apa Itu Stabilitas Lyapunov?", "id": "Metode Menganalisis Stabilitas Sistem Non-linear." },
  { "en": "Apa Itu Filter Kalman?", "id": "Algoritma Estimasi Keadaan Optimal Dari Pengukuran Berderau." },
  { "en": "Apa Bentuk Integral Hukum Induksi Faraday?", "id": "Integral Garis Medan Listrik Sama Dengan Laju Fluks." },
  { "en": "Apa Bentuk Diferensial Hukum Induksi Faraday?", "id": "Curl Medan Listrik Adalah Negatif Turunan Medan Magnet." },
  { "en": "Apa Impedansi Gelombang Dalam Medium?", "id": "Rasio Medan Listrik, Magnet Dalam Medium Dielektrik." },
  { "en": "Apa Itu Pasangan Ekor Panjang (Long-Tailed Pair)?", "id": "Nama Lain Rangkaian Pasangan Diferensial." },
  { "en": "Mengapa Disebut Ekor Panjang?", "id": "Menggunakan Sumber Arus (Ekor) Impedansi Tinggi." },
  { "en": "Apa Prinsip Kerja Mixer Sel Gilbert?", "id": "Pencampuran Sinyal Non-linear Menggunakan Pasangan Diferensial." },
  { "en": "Apa Itu Osiloskop Sampling?", "id": "Osiloskop Untuk Sinyal Periodik Frekuensi Sangat Tinggi." },
  { "en": "Apa Beda Osiloskop Sampling Dan Real-Time?", "id": "Real-Time Mengambil, Menampilkan Dalam Satu Akuisisi." },
  { "en": "Apa Kepanjangan RTD (Resistance Temperature Detector)?", "id": "Detektor Suhu Berbasis Resistansi." },
  { "en": "Apa Beda Termistor Dan RTD?", "id": "RTD Lebih Linear, Stabil, Termistor Lebih Sensitif." },
  { "en": "Bahan Apa Yang Umum Untuk RTD?", "id": "Platina (Platinum)." },
  { "en": "Mana Kapasitor Yang Memiliki Polaritas?", "id": "Kapasitor Elektrolit Dan Tantalum." },
  { "en": "Mana Kapasitor Yang Baik Untuk Frekuensi Tinggi?", "id": "Keramik, Mika, Dan Film." },
  { "en": "Mengapa Inti Udara Digunakan Untuk Induktor?", "id": "Tidak Ada Saturasi, Linearitas Tinggi." },
  { "en": "Apa Kepanjangan SELV (Safety Extra-Low Voltage)?", "id": "Tegangan Ekstra Rendah Keselamatan." },
  { "en": "Apa Karakteristik Sirkuit SELV?", "id": "Terisolasi Secara Aman Dari Tegangan Berbahaya." },
  { "en": "Apa Kepanjangan PELV (Protective Extra-Low Voltage)?", "id": "Tegangan Ekstra Rendah Protektif." },
  { "en": "Apa Beda SELV Dan PELV?", "id": "Sirkuit PELV Memiliki Koneksi Ke Ground." },
  { "en": "Apa Kepanjangan FELV (Functional Extra-Low Voltage)?", "id": "Tegangan Ekstra Rendah Fungsional." },
  { "en": "Apakah Sirkuit FELV Dianggap Aman?", "id": "Tidak, Karena Tidak Memiliki Isolasi Keselamatan." },
  { "en": "Apa Itu Lapisan Deplesi (Depletion Layer)?", "id": "Nama Lain Untuk Daerah Deplesi." },
  { "en": "Apa Itu Potensial Bawaan (Built-in Potential)?", "id": "Beda Potensial Simpangan P-N Tanpa Bias." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Memberi Tegangan Positif Ke Sisi P, Negatif Ke N." },
  { "en": "Apa Efek Dari Bias Maju?", "id": "Menurunkan Potensial Bawaan, Mengalirkan Arus." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Memberi Tegangan Negatif Ke Sisi P, Positif Ke N." },
  { "en": "Apa Efek Dari Bias Mundur?", "id": "Memperlebar Daerah Deplesi, Menghambat Arus." },
  { "en": "Apa Itu Arus Injeksi?", "id": "Arus Pembawa Minoritas Diinjeksikan Melintasi Simpangan." },
  { "en": "Apa Itu Rekombinasi Permukaan?", "id": "Rekombinasi Pembawa Muatan Di Permukaan Semikonduktor." },
  { "en": "Apa Itu Gettering?", "id": "Proses Menghilangkan Ketidakmurnian Daerah Aktif." },
  { "en": "Apa Itu Proses Pasivasi?", "id": "Menumbuhkan Lapisan Pelindung (Oksida/Nitrida) Pada Wafer." },
  { "en": "Apa Itu Local Oxidation of Silicon (LOCOS)?", "id": "Teknik Isolasi Antar Perangkat Pada IC." },
  { "en": "Apa Itu Shallow Trench Isolation (STI)?", "id": "Teknik Isolasi Modern Pengganti LOCOS." },
  { "en": "Apa Itu Doping Kompensasi?", "id": "Menambahkan Dopant Tipe Berlawanan Mengurangi Efek." },
  { "en": "Apa Itu Pembawa Mayoritas?", "id": "Jenis Pembawa Muatan Paling Banyak Semikonduktor." },
  { "en": "Apa Itu Pembawa Minoritas?", "id": "Jenis Pembawa Muatan Paling Sedikit Semikonduktor." },
  { "en": "Apa Pembawa Mayoritas Di Semikonduktor Tipe-N?", "id": "Elektron." },
  { "en": "Apa Pembawa Minoritas Di Semikonduktor Tipe-N?", "id": "Lubang (Hole)." },
  { "en": "Apa Pembawa Mayoritas Di Semikonduktor Tipe-P?", "id": "Lubang (Hole)." },
  { "en": "Apa Pembawa Minoritas Di Semikonduktor Tipe-P?", "id": "Elektron." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Emisi Elektron Material Saat Terkena Cahaya." },
  { "en": "Apa Itu Fungsi Kerja (Work Function)?", "id": "Energi Minimum Melepaskan Elektron Dari Permukaan." },
  { "en": "Apa Itu Kontak Ohmik?", "id": "Sambungan Logam-Semikonduktor Hambatan Sangat Rendah." },
  { "en": "Apa Itu Kontak Schottky (Penyearah)?", "id": "Sambungan Logam-Semikonduktor Yang Membentuk Dioda." },
  { "en": "Apa Itu MOSFET Kanal N (NMOS)?", "id": "MOSFET Dengan Kanal Elektron." },
  { "en": "Apa Itu MOSFET Kanal P (PMOS)?", "id": "MOSFET Dengan Kanal Lubang (Hole)." },
  { "en": "Apa Itu Logika CMOS?", "id": "Menggunakan Pasangan Transistor NMOS Dan PMOS." },
  { "en": "Apa Itu Saturasi Kecepatan (Velocity Saturation)?", "id": "Kondisi Kecepatan Pembawa Muatan Mencapai Batas Maksimum." },
  { "en": "Apa Itu Perangkap Muatan (Charge Trapping)?", "id": "Pembawa Muatan Terperangkap Cacat Dielektrik." },
  { "en": "Apa Efek Dari Perangkap Muatan?", "id": "Pergeseran Tegangan Treshold Seiring Waktu." },
  { "en": "Apa Itu Efek Terowongan Kuantum?", "id": "Partikel Menembus Penghalang Potensial." },
  { "en": "Kapan Efek Terowongan Kuantum Menjadi Masalah?", "id": "Saat Ketebalan Oksida Gerbang Sangat Tipis." },
  { "en": "Apa Itu Spintronik?", "id": "Elektronik Memanfaatkan Spin, Selain Muatan Elektron." },
  { "en": "Apa Kepanjangan GMR (Giant Magnetoresistance)?", "id": "Magnetoresistansi Raksasa." },
  { "en": "Di Mana GMR (Giant Magnetoresistance) Digunakan?", "id": "Kepala Pembaca Hard Disk, Sensor Magnetik." },
  { "en": "Apa Kepanjangan TMR (Tunnel Magnetoresistance)?", "id": "Magnetoresistansi Terowongan." },
  { "en": "Apa Itu Memristor?", "id": "Komponen Pasif Keempat (Selain R, L, C)." },
  { "en": "Apa Sifat Unik Memristor?", "id": "Resistansinya Tergantung Sejarah Arus Melaluinya." },
  { "en": "Apa Itu Rekayasa Neuromorfik?", "id": "Mendesain Sirkuit Meniru Struktur Otak Biologis." },
  { "en": "Apa Itu Fotonik Silikon?", "id": "Menggunakan Silikon Untuk Komponen Optik." },
  { "en": "Apa Keuntungan Fotonik Silikon?", "id": "Integrasi Optik, Elektronik Pada Satu Chip." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Buatan Sifat Elektromagnetik Tidak Biasa." },
  { "en": "Apa Itu Indeks Bias Negatif?", "id": "Sifat Metamaterial Membelokkan Cahaya Arah Berlawanan." },
  { "en": "Apa Itu Celah Terahertz (THz Gap)?", "id": "Rentang Frekuensi Antara Elektronik, Optik." },
  { "en": "Apa Itu Plasmonik?", "id": "Studi Interaksi Elektromagnetik Dengan Elektron Konduksi." },
  { "en": "Apa Itu Plasmon Permukaan?", "id": "Osilasi Elektron Di Permukaan Logam." },
  { "en": "Apa Itu Litografi Top-Down?", "id": "Metode Fabrikasi Dari Besar Ke Kecil." },
  { "en": "Apa Itu Perakitan Diri Bottom-Up?", "id": "Metode Fabrikasi Di Mana Struktur Tumbuh Sendiri." },
  { "en": "Apa Itu Sistem Siber-Fisik (Cyber-Physical System)?", "id": "Integrasi Erat Antara Komputasi, Jaringan, Proses Fisik." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Dengan Konektivitas Internet." },
  { "en": "Apa Itu Simpul Sensor (Sensor Node)?", "id": "Perangkat Kecil Jaringan Sensor Nirkabel." },
  { "en": "Apa Kepanjangan WSN (Wireless Sensor Network)?", "id": "Jaringan Sensor Nirkabel." },
  { "en": "Apa Tantangan Desain WSN?", "id": "Konsumsi Daya Rendah, Komunikasi Andal." },
  { "en": "Protokol Komunikasi Apa Digunakan Dalam IoT?", "id": "MQTT, CoAP, Bluetooth Low Energy (BLE)." },
  { "en": "Apa Kepanjangan MQTT (Message Queuing Telemetry Transport)?", "id": "Transportasi Telemetri Antrian Pesan." },
  { "en": "Apa Itu Model Publisher-Subscriber?", "id": "Pola Komunikasi Yang Digunakan Oleh MQTT." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth Hemat Daya Untuk IoT." },
  { "en": "Apa Itu Zigbee?", "id": "Standar Komunikasi Nirkabel Daya Rendah." },
  { "en": "Apa Itu Z-Wave?", "id": "Protokol Nirkabel Lain Untuk Otomasi Rumah." },
  { "en": "Apa Itu LoRaWAN?", "id": "Protokol Jaringan Area Luas Daya Rendah." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Memproses Data Dekat Sumber Data Dihasilkan." },
  { "en": "Apa Keuntungan Komputasi Tepi?", "id": "Latensi Rendah, Mengurangi Beban Jaringan." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menggunakan Jaringan Server Jarak Jauh Menyimpan, Mengelola Data." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Sistem Radio Komponennya Didefinisikan Perangkat Lunak." },
  { "en": "Apa Keuntungan SDR (Software-Defined Radio)?", "id": "Fleksibilitas Tinggi, Dapat Diprogram Ulang." },
  { "en": "Apa Itu Radio Kognitif?", "id": "Radio Cerdas Dapat Mengubah Parameternya." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Dengan Komunikasi Dua Arah." },
  { "en": "Apa Itu Respon Samping Otomatis (Demand Response)?", "id": "Program Penyesuaian Konsumsi Listrik Otomatis." },
  { "en": "Apa Itu Distributed Generation (DG)?", "id": "Pembangkitan Listrik Tersebar Dekat Titik Beban." },
  { "en": "Sebutkan Contoh Distributed Generation (DG)?", "id": "Panel Surya Atap, Turbin Angin Kecil." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Dapat Beroperasi Mandiri." },
  { "en": "Apa Itu Mode Terhubung Jaringan (Grid-Tied)?", "id": "Sistem Terhubung Dan Sinkron Dengan Jaringan Utama." },
  { "en": "Apa Itu Mode Pulau (Island Mode)?", "id": "Sistem Beroperasi Terpisah Dari Jaringan Utama." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Memberi Daya Kembali Ke Jaringan." },
  { "en": "Apa Itu Analisis Aliran Daya Stokastik?", "id": "Aliran Daya Memperhitungkan Ketidakpastian Pembangkitan." },
  { "en": "Apa Itu Stabilitas Tegangan?", "id": "Kemampuan Sistem Menjaga Tegangan Stabil." },
  { "en": "Apa Itu Runtuh Tegangan (Voltage Collapse)?", "id": "Penurunan Tegangan Drastis Tak Terkendali." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron Satu Sama Lain." },
  { "en": "Apa Itu Osilasi Daya (Power Oscillation)?", "id": "Fluktuasi Periodik Aliran Daya Antar Area." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Peredam Osilasi Daya Pada Generator." },
  { "en": "Apa Itu Verifikasi Berbasis Assertion (ABV)?", "id": "Verifikasi Menggunakan Properti Formal (Assertion)." },
  { "en": "Apa Itu Verifikasi Berbasis Cakupan (Coverage-Driven)?", "id": "Mengukur Kemajuan Verifikasi Berdasarkan Metrik Cakupan." },
  { "en": "Apa Itu Cakupan Kode (Code Coverage)?", "id": "Ukuran Baris Kode Dieksekusi Selama Simulasi." },
  { "en": "Apa Itu Cakupan Fungsional (Functional Coverage)?", "id": "Ukuran Skenario Fungsional Diuji." },
  { "en": "Apa Itu Universal Verification Methodology (UVM)?", "id": "Metodologi Standar Untuk Verifikasi IC." },
  { "en": "Apa Itu SystemVerilog?", "id": "Ekstensi Bahasa Verilog Untuk Desain, Verifikasi." },
  { "en": "Apa Itu Konstanta Propagasi?", "id": "Menggambarkan Perubahan Amplitudo, Fasa Gelombang." },
  { "en": "Apa Itu Konstanta Atenuasi?", "id": "Bagian Riil Konstanta Propagasi (Pelemahan)." },
  { "en": "Apa Itu Konstanta Fasa?", "id": "Bagian Imajiner Konstanta Propagasi (Pergeseran Fasa)." },
  { "en": "Apa Itu Polarisasi Gelombang Linear?", "id": "Vektor Medan Listrik Berosilasi Dalam Satu Garis." },
  { "en": "Apa Itu Polarisasi Gelombang Sirkular?", "id": "Ujung Vektor Medan Listrik Membentuk Lingkaran." },
  { "en": "Apa Beda Polarisasi Sirkular Kanan Dan Kiri?", "id": "Arah Rotasi Vektor Medan Listrik." },
  { "en": "Apa Itu Antena Array?", "id": "Beberapa Elemen Antena Bekerja Bersama." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Antena Array Dengan Arah Sorotan Dapat Diatur." },
  { "en": "Bagaimana Phased Array Mengatur Arah Sorotan?", "id": "Dengan Mengubah Fasa Sinyal Setiap Elemen." },
  { "en": "Apa Itu Beamforming?", "id": "Teknik Memfokuskan Sinyal Nirkabel Ke Arah Tertentu." },
  { "en": "Apa Itu Amplifier Kebisingan Rendah (LNA)?", "id": "Amplifier Pertama Di Rantai Penerima RF." },
  { "en": "Apa Karakteristik Penting LNA (Low Noise Amplifier)?", "id": "Noise Figure Sangat Rendah, Penguatan Cukup." },
  { "en": "Apa Itu Power Amplifier (PA)?", "id": "Amplifier Tahap Akhir Di Rantai Pemancar." },
  { "en": "Apa Karakteristik Penting PA (Power Amplifier)?", "id": "Efisiensi Daya Tinggi, Linearitas Baik." },
  { "en": "Apa Itu Persamaan Friis Untuk Suhu Derau?", "id": "Menghitung Total Suhu Derau Sistem Bertingkat." },
  { "en": "Apa Itu Pengamat Luenberger?", "id": "Jenis Pengamat Keadaan Untuk Sistem Linear." },
  { "en": "Apa Itu Pengontrol Optimal?", "id": "Pengontrol Yang Meminimalkan Fungsi Biaya Tertentu." },
  { "en": "Apa Itu Linear Quadratic Regulator (LQR)?", "id": "Jenis Pengontrol Optimal." },
  { "en": "Apa Itu Model Autoregressive (AR)?", "id": "Model Sinyal Berdasarkan Nilai Masa Lalunya." },
  { "en": "Apa Itu Model Moving Average (MA)?", "id": "Model Sinyal Berdasarkan Error Prediksi Masa Lalu." },
  { "en": "Apa Itu Model ARMA?", "id": "Gabungan Model Autoregressive Dan Moving Average." },
  { "en": "Apa Itu Koefisien Refleksi?", "id": "Rasio Amplitudo Gelombang Pantul Terhadap Gelombang Datang." },
  { "en": "Apa Itu Koefisien Transmisi?", "id": "Rasio Amplitudo Gelombang Transmisi Terhadap Gelombang Datang." },
  { "en": "Apa Itu Sudut Brewster?", "id": "Sudut Datang Tanpa Pantulan Untuk Polarisasi Tertentu." },
  { "en": "Apa Itu Refleksi Internal Total?", "id": "Fenomena Pantulan Total Di Antarmuka Medium." },
  { "en": "Apa Itu Sudut Kritis?", "id": "Sudut Datang Minimum Terjadinya Refleksi Internal Total." },
  { "en": "Bagaimana Prinsip Kerja Serat Optik?", "id": "Berdasarkan Fenomena Refleksi Internal Total." },
  { "en": "Apa Itu Mode Gelap (Dark Current) Fotodetektor?", "id": "Arus Kecil Saat Tidak Ada Cahaya." },
  { "en": "Apa Itu Responsivitas Fotodetektor?", "id": "Rasio Arus Foto Terhadap Daya Optik Masuk." },
  { "en": "Apa Itu Dioda Laser Fabry-Pérot?", "id": "Jenis Dioda Laser Paling Umum." },
  { "en": "Apa Itu Dioda Laser DFB (Distributed Feedback)?", "id": "Dioda Laser Dengan Emisi Satu Frekuensi." },
  { "en": "Apa Itu Amplifier Optik?", "id": "Menguatkan Sinyal Cahaya Langsung Tanpa Konversi." },
  { "en": "Apa Itu Erbium-Doped Fiber Amplifier (EDFA)?", "id": "Jenis Amplifier Optik Paling Umum." },
  { "en": "Apa Itu Wavelength Division Multiplexing (WDM)?", "id": "Mengirim Beberapa Sinyal Cahaya Beda Panjang Gelombang." },
  { "en": "Apa Beda WDM (Wavelength Division Multiplexing) Kasar Dan Padat?", "id": "Jarak Antar Saluran Panjang Gelombang." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Pelebaran Pulsa Optik Akibat Kecepatan Beda." },
  { "en": "Apa Itu Dispersi Mode Polarisasi (PMD)?", "id": "Pelebaran Pulsa Akibat Kecepatan Beda Polarisasi." },
  { "en": "Apa Itu Time-Division Duplexing (TDD)?", "id": "Menggunakan Slot Waktu Berbeda Untuk Uplink, Downlink." },
  { "en": "Apa Itu Frequency-Division Duplexing (FDD)?", "id": "Menggunakan Pita Frekuensi Berbeda Untuk Uplink, Downlink." },
  { "en": "Apa Itu Kanal Fading Rayleigh?", "id": "Model Kanal Nirkabel Tanpa Komponen Line-of-Sight." },
  { "en": "Apa Itu Kanal Fading Rician?", "id": "Model Kanal Nirkabel Dengan Komponen Line-of-Sight." },
  { "en": "Apa Itu Keanekaragaman (Diversity) Dalam Komunikasi?", "id": "Menggunakan Beberapa Jalur Meredam Efek Fading." },
  { "en": "Sebutkan Jenis Teknik Keanekaragaman?", "id": "Ruang, Waktu, Frekuensi, Dan Polarisasi." },
  { "en": "Apa Itu Multiple-Input Multiple-Output (MIMO)?", "id": "Menggunakan Banyak Antena Di Pemancar, Penerima." },
  { "en": "Apa Keuntungan MIMO (Multiple-Input Multiple-Output)?", "id": "Meningkatkan Kecepatan Data Dan Keandalan Tautan." },
  { "en": "Apa Itu Spatial Multiplexing?", "id": "Mengirim Beberapa Aliran Data Melalui Kanal MIMO." },
  { "en": "Apa Itu Beamforming Transmit?", "id": "Memfokuskan Energi Pancaran Ke Arah Penerima." },
  { "en": "Apa Itu Selular?", "id": "Konsep Membagi Area Layanan Menjadi Sel-sel." },
  { "en": "Apa Itu Penggunaan Ulang Frekuensi (Frequency Reuse)?", "id": "Menggunakan Frekuensi Sama Di Sel Tidak Berdekatan." },
  { "en": "Apa Itu Handoff (Handover)?", "id": "Proses Pengalihan Panggilan Dari Satu Sel Ke Lainnya." },
  { "en": "Apa Itu Interferensi Antar-Sel (Inter-Cell Interference)?", "id": "Gangguan Dari Sel Tetangga Menggunakan Frekuensi Sama." },
  { "en": "Apa Itu Orthogonal Frequency-Division Multiple Access (OFDMA)?", "id": "Skema Akses Ganda Berbasis OFDM." },
  { "en": "Apa Itu Single-Carrier FDMA (SC-FDMA)?", "id": "Modifikasi OFDMA Untuk Mengurangi PAPR." },
  { "en": "Apa Kepanjangan PAPR (Peak-to-Average Power Ratio)?", "id": "Rasio Daya Puncak Terhadap Rata-rata." },
  { "en": "Mengapa PAPR (Peak-to-Average Power Ratio) Tinggi Buruk?", "id": "Membutuhkan Power Amplifier Yang Sangat Linear." },
  { "en": "Apa Itu Orthogonality Factor?", "id": "Ukuran Seberapa Ortogonal Kode Dalam Sistem CDMA." },
  { "en": "Apa Itu Kontrol Daya (Power Control)?", "id": "Menyesuaikan Daya Pancar Mencegah Interferensi Berlebih." },
  { "en": "Apa Itu Near-Far Problem Dalam CDMA?", "id": "Sinyal Kuat Pengguna Dekat Menutupi Sinyal Lemah." },
  { "en": "Apa Itu Perambatan Line-of-Sight (LOS)?", "id": "Perambatan Gelombang Radio Langsung Tanpa Halangan." },
  { "en": "Apa Itu Perambatan Non-Line-of-Sight (NLOS)?", "id": "Perambatan Melalui Pantulan, Difraksi, Hamburan." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Gelombang Saat Melewati Tepi Penghalang." },
  { "en": "Apa Itu Hamburan (Scattering)?", "id": "Penyebaran Gelombang Akibat Objek Kecil." },
  { "en": "Apa Itu Kerugian Jalur Ruang Bebas?", "id": "Pelemahan Sinyal Alami Seiring Jarak." },
  { "en": "Apa Itu Zona Fresnel?", "id": "Daerah Elipsoid Antara Pemancar, Penerima." },
  { "en": "Mengapa Zona Fresnel Penting?", "id": "Halangan Di Zona Ini Menyebabkan Pelemahan Sinyal." },
  { "en": "Apa Itu Fade Margin?", "id": "Daya Ekstra Untuk Mengatasi Fluktuasi Sinyal (Fading)." },
  { "en": "Apa Itu Sistem Trunking Radio?", "id": "Berbagi Saluran Radio Secara Otomatis Antar Pengguna." },
  { "en": "Apa Itu Erlang?", "id": "Satuan Pengukuran Beban Lalu Lintas Telekomunikasi." },
  { "en": "Apa Itu Grade of Service (GoS)?", "id": "Probabilitas Panggilan Diblokir Dalam Jaringan." },
  { "en": "Apa Itu Pengkodean Entropi?", "id": "Jenis Kompresi Data Lossless (Huffman, Aritmetik)." },
  { "en": "Apa Itu Transform Coding?", "id": "Jenis Kompresi Data Lossy (DCT, Wavelet)." },
  { "en": "Apa Itu Kuantisasi Vektor?", "id": "Teknik Kuantisasi Kelompok Sampel Secara Bersamaan." },
  { "en": "Apa Itu PSNR (Peak Signal-to-Noise Ratio)?", "id": "Ukuran Kualitas Rekonstruksi Gambar/Video Lossy." },
  { "en": "Apa Itu Kode Hamming?", "id": "Jenis Kode Koreksi Error Blok Linear Sederhana." },
  { "en": "Apa Itu Kode BCH?", "id": "Kode Siklik Kuat Dapat Memperbaiki Banyak Error." },
  { "en": "Apa Itu Pemetaan Memori I/O (Input/Output)?", "id": "Perangkat I/O Diperlakukan Seperti Lokasi Memori." },
  { "en": "Apa Itu I/O (Input/Output) Terisolasi (Port-Mapped)?", "id": "Menggunakan Ruang Alamat Terpisah Untuk I/O." },
  { "en": "Apa Itu Sakelar Analog?", "id": "Sakelar Elektronik Untuk Melewatkan Sinyal Analog." },
  { "en": "Apa Komponen Utama Sakelar Analog?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)." },
  { "en": "Apa Itu Resistansi ON (On-Resistance) Sakelar Analog?", "id": "Resistansi Sakelar Saat Dalam Keadaan Tertutup." },
  { "en": "Apa Itu Kapasitansi OFF (Off-Capacitance) Sakelar Analog?", "id": "Kapasitansi Sakelar Saat Dalam Keadaan Terbuka." },
  { "en": "Apa Itu Injeksi Muatan (Charge Injection)?", "id": "Error Akibat Muatan Gerbang Pindah Ke Sinyal." },
  { "en": "Apa Itu Penguat Sampel-dan-Tahan (Sample-and-Hold)?", "id": "Mencuplik, Menahan Tegangan Analog Untuk Konversi." },
  { "en": "Apa Itu Waktu Akuisisi (Acquisition Time)?", "id": "Waktu Diperlukan Kapasitor Untuk Mengisi Tegangan." },
  { "en": "Apa Itu Droop Rate?", "id": "Laju Penurunan Tegangan Ditahan Akibat Bocor." },
  { "en": "Apa Itu Penguat Lock-In?", "id": "Mengekstrak Sinyal Dengan Frekuensi Referensi Dikenal." },
  { "en": "Apa Itu Deteksi Sinkron (Koheren)?", "id": "Prinsip Dasar Kerja Amplifier Lock-In." },
  { "en": "Apa Itu Jaringan Listrik Polifase?", "id": "Sistem AC Dengan Banyak Fasa (Umumnya Tiga)." },
  { "en": "Apa Itu Daya Kompleks?", "id": "Representasi Vektor Daya (Nyata Dan Reaktif)." },
  { "en": "Apa Itu Impedansi Urutan Nol?", "id": "Impedansi Jaringan Terhadap Arus Urutan Nol." },
  { "en": "Apa Itu Impedansi Urutan Positif?", "id": "Impedansi Jaringan Terhadap Arus Urutan Positif." },
  { "en": "Apa Itu Impedansi Urutan Negatif?", "id": "Impedansi Jaringan Terhadap Arus Urutan Negatif." },
  { "en": "Kapan Impedansi Urutan Positif, Negatif Berbeda?", "id": "Pada Mesin Berputar (Generator, Motor)." },
  { "en": "Apa Itu Sistem Tenaga Radial?", "id": "Sistem Distribusi Dengan Satu Arah Aliran Daya." },
  { "en": "Apa Itu Sistem Tenaga Cincin (Loop)?", "id": "Sistem Distribusi Membentuk Loop Tertutup." },
  { "en": "Apa Keuntungan Sistem Tenaga Cincin?", "id": "Keandalan Lebih Tinggi, Suplai Dari Dua Arah." },
  { "en": "Apa Itu Sistem Tenaga Saling Terhubung (Interconnected)?", "id": "Beberapa Jaringan Terhubung Untuk Keandalan." },
  { "en": "Apa Itu Osilasi Torsi Subsinkron (Subsynchronous Resonance)?", "id": "Interaksi Resonansi Antara Jaringan, Generator." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Menambahkan Kapasitor Seri Mengurangi Reaktansi Saluran." },
  { "en": "Apa Tujuan Kompensasi Seri?", "id": "Meningkatkan Kapasitas Transfer Daya Jarak Jauh." },
  { "en": "Apa Itu Kompensasi Shunt?", "id": "Menambahkan Reaktansi Paralel Untuk Mengatur Tegangan." },
  { "en": "Apa Itu Reaktor Shunt?", "id": "Induktor Shunt Mengkompensasi Efek Kapasitif Saluran." },
  { "en": "Kapan Reaktor Shunt Digunakan?", "id": "Saluran Transmisi Panjang, Beban Ringan." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Penerima Lebih Tinggi Dari Pengirim." },
  { "en": "Apa Penyebab Efek Ferranti?", "id": "Kapasitansi Saluran Pada Beban Sangat Ringan." },
  { "en": "Apa Itu Kapasitor Shunt?", "id": "Kapasitor Paralel Mengkompensasi Beban Induktif." },
  { "en": "Kapan Kapasitor Shunt Digunakan?", "id": "Memperbaiki Faktor Daya, Menaikkan Tegangan Lokal." },
  { "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?", "id": "Motor Sinkron Tanpa Beban, Penghasil VAR." },
  { "en": "Apa Itu Eksitasi Berlebih (Over-Excitation)?", "id": "Generator Menyuplai Daya Reaktif Ke Jaringan." },
  { "en": "Apa Itu Eksitasi Kurang (Under-Excitation)?", "id": "Generator Menyerap Daya Reaktif Dari Jaringan." },
  { "en": "Apa Itu Batas Stabilitas Keadaan Tunak?", "id": "Daya Maksimum Dapat Ditransfer Tanpa Kehilangan Sinkronisasi." },
  { "en": "Apa Itu Stabilitas Kritis?", "id": "Waktu Maksimum Gangguan Dapat Dihilangkan." },
  { "en": "Apa Itu Resistor Pentanahan Netral (NGR)?", "id": "Resistor Pembatas Arus Gangguan Tanah." },
  { "en": "Apa Itu Transformator Zig-Zag?", "id": "Trafo Tujuan Khusus Penyedia Jalur Pentanahan." },
  { "en": "Apa Itu Tegangan Langkah (Step Voltage)?", "id": "Beda Potensial Antara Dua Kaki Seseorang." },
  { "en": "Apa Itu Tegangan Sentuh (Touch Voltage)?", "id": "Beda Potensial Antara Tangan, Kaki Seseorang." },
  { "en": "Apa Itu Resistansi Kaki Manusia?", "id": "Resistansi Listrik Tubuh Manusia." },
  { "en": "Apa Itu Fibrilasi Ventrikel?", "id": "Kontraksi Jantung Tidak Terkoordinasi Akibat Sengatan." },
  { "en": "Apa Ambang Batas Arus Fibrilasi?", "id": "Beberapa Puluh Milliampere (mA)." },
  { "en": "Apa Itu Efek Let-Go?", "id": "Ketidakmampuan Melepaskan Konduktor Akibat Kontraksi Otot." },
  { "en": "Apa Itu Skema Proteksi Pilot?", "id": "Proteksi Saluran Cepat Menggunakan Kanal Komunikasi." },
  { "en": "Apa Itu Skema Pemblokiran Arah (Directional Blocking)?", "id": "Skema Proteksi Pilot Mengirim Sinyal Blok." },
  { "en": "Apa Itu Skema Pelepasan Izin (Permissive Tripping)?", "id": "Skema Proteksi Pilot Mengirim Sinyal Izin Trip." },
  { "en": "Apa Itu Sinkrofasor (Synchrophasor)?", "id": "Pengukuran Fasor Tersinkronisasi Waktu (GPS)." },
  { "en": "Apa Itu Model Garis Pi?", "id": "Model Saluran Transmisi Jarak Menengah." },
  { "en": "Apa Itu Model Garis Panjang?", "id": "Model Saluran Transmisi Menggunakan Parameter Terdistribusi." },
  { "en": "Apa Itu Konstanta Kompleks Propagasi?", "id": "Menggambarkan Atenuasi Dan Pergeseran Fasa Gelombang." },
  { "en": "Apa Itu Direct Memory Access (DMA)?", "id": "Transfer Data Memori Tanpa Intervensi CPU." },
  { "en": "Apa Itu Siklus Pencurian (Cycle Stealing)?", "id": "DMA Mengambil Alih Bus Dari CPU Sementara." },
  { "en": "Apa Itu Mode Transfer Burst DMA?", "id": "DMA Mengontrol Bus Sampai Seluruh Blok Ditransfer." },
  { "en": "Apa Itu Koprocessor (Coprocessor)?", "id": "Prosesor Tambahan Untuk Tugas Khusus (Matematika)." },
  { "en": "Apa Itu Instruksi Makro?", "id": "Satu Instruksi Merepresentasikan Urutan Instruksi Mikro." },
  { "en": "Apa Itu Mikroarsitektur?", "id": "Implementasi Internal Set Instruksi Prosesor." },
  { "en": "Apa Itu Hukum Amdahl?", "id": "Menjelaskan Batas Peningkatan Kinerja Sistem Paralel." },
  { "en": "Apa Itu Throughput?", "id": "Jumlah Tugas Selesai Per Satuan Waktu." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Total Menyelesaikan Satu Tugas." },
  { "en": "Apa Itu Sistem Operasi Waktu Nyata (RTOS)?", "id": "Sistem Operasi Dengan Penjadwalan Deterministik." },
  { "en": "Apa Itu Kernel Dalam Sistem Operasi?", "id": "Inti Dari Sistem Operasi." },
  { "en": "Apa Itu Penjadwal (Scheduler)?", "id": "Bagian Kernel Penentu Tugas Mana Dijalankan." },
  { "en": "Apa Itu Penjadwalan Preemptif?", "id": "Sistem Operasi Dapat Menghentikan Tugas Berjalan." },
  { "en": "Apa Itu Penjadwalan Non-Preemptif (Kooperatif)?", "id": "Tugas Berjalan Sampai Selesai Atau Menyerah." },
  { "en": "Apa Itu Prioritas Inversi?", "id": "Tugas Prioritas Rendah Menghalangi Tugas Prioritas Tinggi." },
  { "en": "Bagaimana Mengatasi Prioritas Inversi?", "id": "Menggunakan Protokol Pewarisan Prioritas (Priority Inheritance)." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Pencegah Akses Bersamaan Sumber Daya." },
  { "en": "Apa Itu Semaphore?", "id": "Variabel Sinkronisasi Mengontrol Akses Sumber Daya." },
  { "en": "Apa Itu Deadlock?", "id": "Dua Atau Lebih Proses Saling Menunggu." },
  { "en": "Apa Itu Kondisi Balapan (Race Condition)?", "id": "Hasil Bergantung Urutan Eksekusi Proses Tidak Terduga." },
  { "en": "Apa Itu Bagian Kritis (Critical Section)?", "id": "Bagian Kode Pengakses Sumber Daya Bersama." },
  { "en": "Apa Itu Operasi Atomik?", "id": "Operasi Yang Dijamin Selesai Tanpa Interupsi." },
  { "en": "Apa Itu Jitter Dalam RTOS?", "id": "Variasi Waktu Eksekusi Tugas Periodik." },
  { "en": "Apa Itu Penalaan (Tuning) Loop Kontrol?", "id": "Proses Penyesuaian Parameter Kontroler Kinerja Optimal." },
  { "en": "Apa Itu Plot Respon Frekuensi?", "id": "Grafik Penguatan, Fasa Terhadap Frekuensi." },
  { "en": "Apa Itu Diagram Bode?", "id": "Plot Logaritmik Respon Frekuensi." },
  { "en": "Apa Itu Dekade Dalam Diagram Bode?", "id": "Perubahan Frekuensi Faktor Sepuluh." },
  { "en": "Apa Itu Oktaf Dalam Diagram Bode?", "id": "Perubahan Frekuensi Faktor Dua." },
  { "en": "Apa Itu Kemiringan -20 dB/dekade?", "id": "Menunjukkan Adanya Satu Kutub (Pole) Riil." },
  { "en": "Apa Itu Kemiringan +20 dB/dekade?", "id": "Menunjukkan Adanya Satu Nol (Zero) Riil." },
  { "en": "Apa Itu Filter All-Pass?", "id": "Filter Pengubah Fasa Tanpa Mengubah Penguatan." },
  { "en": "Apa Itu Penundaan Fasa (Phase Delay)?", "id": "Penundaan Waktu Setiap Komponen Frekuensi Sinyal." },
  { "en": "Apa Beda Penundaan Fasa Dan Grup?", "id": "Penundaan Grup Terkait Dengan Selubung Sinyal." },
  { "en": "Apa Itu Distorsi Fasa?", "id": "Terjadi Saat Penundaan Fasa Tidak Linear." },
  { "en": "Apa Itu Distorsi Amplitudo?", "id": "Terjadi Saat Penguatan Berubah Terhadap Frekuensi." },
  { "en": "Apa Itu Model Small-Signal Transistor?", "id": "Model Linear Transistor Untuk Analisis AC." },
  { "en": "Apa Itu Model Hibrida-Pi?", "id": "Model Small-Signal Umum Transistor Bipolar." },
  { "en": "Apa Itu Frekuensi Transisi (fT)?", "id": "Frekuensi Penguatan Arus Common-Emitter Menjadi Satu." },
  { "en": "Apa Itu Kapasitansi Basis-Kolektor (Cbc)?", "id": "Kapasitansi Parasitik Antara Basis, Kolektor." },
  { "en": "Apa Itu Resistansi Output (ro)?", "id": "Resistansi Output Efektif Transistor (Efek Early)." },
  { "en": "Apa Itu Amplifier Umpan Balik?", "id": "Amplifier Menggunakan Sebagian Sinyal Outputnya." },
  { "en": "Sebutkan Empat Topologi Amplifier Umpan Balik?", "id": "Seri-Shunt, Shunt-Seri, Seri-Seri, Shunt-Shunt." },
  { "en": "Apa Pengaruh Umpan Balik Negatif?", "id": "Meningkatkan Stabilitas, Bandwidth, Mengurangi Penguatan." },
  { "en": "Apa Itu Desensitivitas Penguatan?", "id": "Umpan Balik Mengurangi Ketergantungan Pada Penguatan Asli." },
  { "en": "Apa Itu Perluasan Bandwidth?", "id": "Umpan Balik Meningkatkan Bandwidth Amplifier." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator Sinusoidal Populer Menggunakan Op-Amp." },
  { "en": "Apa Itu Osilator Pergeseran Fasa?", "id": "Osilator Sinusoidal Menggunakan Jaringan RC." },
  { "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Menggambarkan Tingkat Kegagalan Produk Seiring Waktu." },
  { "en": "Sebutkan Tiga Fase Kurva Bak Mandi?", "id": "Awal (Infant), Normal, Akhir (Wear-out)." },
  { "en": "Apa Itu Persamaan Arrhenius Dalam Keandalan?", "id": "Menghubungkan Tingkat Kegagalan Dengan Suhu." },
  { "en": "Apa Itu Highly Accelerated Life Test (HALT)?", "id": "Uji Percepatan Umur Menemukan Kelemahan Desain." },
  { "en": "Apa Itu Filter Eliptik (Cauer Filter)?", "id": "Filter Dengan Riak Di Passband, Stopband." },
  { "en": "Apa Keuntungan Filter Eliptik?", "id": "Transisi Paling Tajam Untuk Orde Tertentu." },
  { "en": "Apa Itu Filter Bessel-Thomson?", "id": "Filter Dengan Respon Penundaan Grup Paling Datar." },
  { "en": "Kapan Filter Bessel Digunakan?", "id": "Saat Menjaga Bentuk Gelombang Sangat Penting." },
  { "en": "Apa Itu Transformasi Tautologi (Constant-Resistance Network)?", "id": "Jaringan Dengan Impedansi Konstan Terhadap Frekuensi." },
  { "en": "Apa Itu Field-Oriented Control (FOC) Motor AC?", "id": "Metode Kontrol Torsi, Fluks Motor Independen." },
  { "en": "Apa Itu Direct Torque Control (DTC)?", "id": "Metode Kontrol Torsi Motor AC Tanpa Modulator." },
  { "en": "Apa Itu Enkoder Kuadratur?", "id": "Enkoder Dengan Dua Output (A, B) Beda Fasa." },
  { "en": "Apa Fungsi Sinyal Kuadratur Enkoder?", "id": "Menentukan Arah Putaran Serta Kecepatan." },
  { "en": "Apa Itu Sinyal Indeks (Z) Enkoder?", "id": "Satu Pulsa Per Putaran Untuk Referensi Posisi." },
  { "en": "Apa Itu Kontrol Vektor (Vector Control)?", "id": "Nama Lain Untuk Field-Oriented Control (FOC)." },
  { "en": "Apa Itu Magic Tee (Hybrid Tee)?", "id": "Pemandu Gelombang Empat Port Hibrida 180 Derajat." },
  { "en": "Apa Itu Power Added Efficiency (PAE)?", "id": "Ukuran Efisiensi Power Amplifier Frekuensi Radio." },
  { "en": "Apa Itu Resonator Cincin Celah (Split-Ring Resonator)?", "id": "Komponen Dasar Pembangun Metamaterial." },
  { "en": "Apa Itu Frekuensi Selektif Permukaan (FSS)?", "id": "Permukaan Periodik Reflektif/Transmisif Berdasarkan Frekuensi." },
  { "en": "Apa Itu Total Isotropic Sensitivity (TIS) Penerima?", "id": "Ukuran Kinerja Sensitivitas Penerima Tiga Dimensi." },
  { "en": "Apa Itu Total Radiated Power (TRP) Pemancar?", "id": "Total Daya RF Dipancarkan Antena Tiga Dimensi." },
  { "en": "Apa Itu Locational Marginal Pricing (LMP)?", "id": "Harga Listrik Berbeda Berdasarkan Lokasi Jaringan." },
  { "en": "Apa Itu Pasar Kapasitas (Capacity Market)?", "id": "Pasar Untuk Menjamin Ketersediaan Pembangkit Listrik." },
  { "en": "Apa Itu Jasa Tambahan (Ancillary Services)?", "id": "Jasa Pendukung Operasi Jaringan (Kontrol Frekuensi)." },
  { "en": "Apa Kepanjangan SCED (Security-Constrained Economic Dispatch)?", "id": "Dispatch Ekonomi Dibatasi Keamanan." },
  { "en": "Apa Itu Unit Commitment Dalam Sistem Tenaga?", "id": "Menentukan Pembangkit Mana Harus Dinyalakan/Dimatikan." },
  { "en": "Apa Itu IEEE 1588 Precision Time Protocol (PTP)?", "id": "Protokol Sinkronisasi Waktu Sangat Akurat Jaringan." },
  { "en": "Apa Itu Network Time Protocol (NTP)?", "id": "Protokol Sinkronisasi Waktu Jaringan Komputer." },
  { "en": "Apa Itu Efektivitas Pelindung (Shielding Effectiveness)?", "id": "Ukuran Kemampuan Pelindung Meredam Medan Elektromagnetik." },
  { "en": "Apa Itu Kerugian Penyerapan (Absorption Loss)?", "id": "Pelemahan Medan Saat Melewati Material Pelindung." },
  { "en": "Apa Itu Kerugian Pantulan (Reflection Loss)?", "id": "Pelemahan Akibat Ketidaksesuaian Impedansi." },
  { "en": "Apa Itu Via Jahitan (Stitching Vias)?", "id": "Rangkaian Via Penghubung Bidang Ground PCB." },
  { "en": "Apa Tujuan Via Jahitan (Stitching Vias)?", "id": "Memastikan Integritas Ground, Meningkatkan Pelindung EMI." },
  { "en": "Apa Itu Sensor Magnetoresistif Anisotropik (AMR)?", "id": "Sensor Medan Magnet Berbasis Perubahan Resistansi." },
  { "en": "Apa Itu Sensor Giant Magnetoresistive (GMR)?", "id": "Sensor Magnetik Lebih Sensitif Dari AMR." },
  { "en": "Apa Itu Sensor Tunnel Magnetoresistive (TMR)?", "id": "Sensor Magnetik Paling Sensitif Di Antaranya." },
  { "en": "Apa Itu Sensor Serat Optik?", "id": "Sensor Menggunakan Serat Optik Merasakan Besaran." },
  { "en": "Apa Itu Fiber Bragg Grating (FBG)?", "id": "Struktur Periodik Serat Optik Pemantul Panjang Gelombang." },
  { "en": "Bagaimana FBG (Fiber Bragg Grating) Bekerja Sebagai Sensor?", "id": "Regangan Atau Suhu Mengubah Panjang Gelombang Pantul." },
  { "en": "Apa Itu Giroskop Serat Optik (FOG)?", "id": "Giroskop Mengukur Rotasi Menggunakan Efek Sagnac." },
  { "en": "Apa Itu Efek Sagnac?", "id": "Pergeseran Fasa Antara Dua Berkas Cahaya." },
  { "en": "Apa Itu Sensor Efek Wiegand?", "id": "Sensor Magnetik Penghasil Pulsa Tegangan Tajam." },
  { "en": "Apa Itu Assignment Blocking Dalam Verilog?", "id": "Dieksekusi Berurutan Dalam Blok Prosedural." },
  { "en": "Apa Itu Assignment Non-blocking Dalam Verilog?", "id": "Dieksekusi Secara Paralel Dalam Blok Prosedural." },
  { "en": "Kapan Assignment Blocking Digunakan?", "id": "Untuk Logika Kombinasional." },
  { "en": "Kapan Assignment Non-blocking Digunakan?", "id": "Untuk Logika Sekuensial (Register, Flip-Flop)." },
  { "en": "Apa Itu Parameter Dalam Verilog?", "id": "Konstanta Yang Dapat Didefinisikan Ulang." },
  { "en": "Apa Itu Perintah `initial` Dalam Verilog?", "id": "Blok Kode Dieksekusi Sekali Di Awal Simulasi." },
  { "en": "Apa Itu Perintah `generate` Dalam Verilog?", "id": "Membuat Instance Perangkat Keras Secara Berulang." },
  { "en": "Apa Kepanjangan FPGA (Field-Programmable Gate Array)?", "id": "Larik Gerbang Dapat Diprogram Lapangan." },
  { "en": "Apa Kepanjangan CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Kompleks Dapat Diprogram." },
  { "en": "Apa Beda FPGA Dan CPLD?", "id": "FPGA Lebih Fleksibel, CPLD Waktu Propagasi Deterministik." },
  { "en": "Apa Itu Bitstream Konfigurasi?", "id": "Berkas Data Untuk Memprogram Perangkat FPGA." },
  { "en": "Apa Itu Memori Konfigurasi FPGA?", "id": "Memori Penyimpan Bitstream (SRAM Atau Flash)." },
  { "en": "Apa Itu Mode Master Serial Konfigurasi FPGA?", "id": "FPGA Mengontrol Proses Pemuatan Bitstream." },
  { "en": "Apa Itu Mode JTAG Konfigurasi FPGA?", "id": "Memprogram FPGA Melalui Antarmuka JTAG." },
  { "en": "Apa Itu Blok Logika Dapat Dikonfigurasi (CLB)?", "id": "Unit Logika Fungsional Dasar Dalam FPGA." },
  { "en": "Apa Itu Look-Up Table (LUT) Dalam FPGA?", "id": "Memori Kecil Pengimplementasi Fungsi Logika." },
  { "en": "Apa Itu Matriks Sakelar Dapat Diprogram?", "id": "Jaringan Interkoneksi Penghubung Blok-Blok Logika." },
  { "en": "Apa Itu Block RAM (BRAM)?", "id": "Blok Memori Khusus Di Dalam FPGA." },
  { "en": "Apa Itu Blok DSP (Digital Signal Processing) Dalam FPGA?", "id": "Perangkat Keras Khusus Operasi DSP." },
  { "en": "Apa Itu Transceiver Berkecepatan Tinggi?", "id": "Sirkuit Khusus Antarmuka Serial Cepat (SerDes)." },
  { "en": "Apa Kepanjangan SerDes (Serializer/Deserializer)?", "id": "Serializer/Deserializer." },
  { "en": "Apa Itu Soft Core Processor?", "id": "Prosesor Diimplementasikan Dalam Logika FPGA." },
  { "en": "Apa Itu Hard Core Processor?", "id": "Prosesor Fisik Terintegrasi Dalam Chip FPGA." },
  { "en": "Apa Itu Penskalaan Tegangan Dinamis?", "id": "Menyesuaikan Tegangan Operasi Prosesor Menghemat Daya." },
  { "en": "Apa Itu Penskalaan Frekuensi Dinamis?", "id": "Menyesuaikan Frekuensi Clock Prosesor Menghemat Daya." },
  { "en": "Apa Itu DVFS (Dynamic Voltage and Frequency Scaling)?", "id": "Kombinasi Penskalaan Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Rasio Energi Harmonik Terhadap Energi Fundamental." },
  { "en": "Apa Itu Total Demand Distortion (TDD)?", "id": "Rasio Arus Harmonik Terhadap Arus Beban Puncak." },
  { "en": "Apa Beda THD Dan TDD?", "id": "TDD Memberi Indikasi Lebih Baik Dampak Harmonik." },
  { "en": "Apa Itu Faktor-K Transformator?", "id": "Rating Trafo Tahan Panas Akibat Arus Harmonik." },
  { "en": "Apa Itu Ketidakseimbangan Tegangan (Voltage Unbalance)?", "id": "Kondisi Tegangan Tiga Fasa Tidak Sama." },
  { "en": "Apa Dampak Ketidakseimbangan Tegangan Pada Motor?", "id": "Pemanasan Berlebih, Menurunkan Efisiensi, Umur." },
  { "en": "Apa Itu Notching Dalam Kualitas Daya?", "id": "Gangguan Periodik Akibat Operasi Konverter Daya." },
  { "en": "Apa Itu Sistem Pentanahan IT?", "id": "Sistem Tanpa Koneksi Langsung Ke Tanah." },
  { "en": "Apa Itu Sistem Pentanahan TT?", "id": "Sumber Dan Beban Ditanahkan Secara Terpisah." },
  { "en": "Apa Itu Sistem Pentanahan TN?", "id": "Sumber Ditanahkan, Beban Dihubungkan Ke Netral." },
  { "en": "Apa Beda Sistem TN-C, TN-S, TN-C-S?", "id": "Cara Penanganan Konduktor Netral Dan Protektif." },
  { "en": "Apa Itu Sistem TN-S (Terre-Neutre-Séparé)?", "id": "Konduktor Netral Dan Protektif Terpisah." },
  { "en": "Apa Itu Sistem TN-C (Terre-Neutre-Commun)?", "id": "Fungsi Netral, Protektif Digabung Satu Konduktor." },
  { "en": "Apa Itu Konduktor PEN?", "id": "Konduktor Gabungan (Protective Earth and Neutral)." },
  { "en": "Apa Itu Sistem Jaringan Referensi Sinyal?", "id": "Jaringan Konduktif Referensi Sinyal Berkecepatan Tinggi." },
  { "en": "Apa Itu Perangkat Perlindungan Lonjakan (SPD)?", "id": "Nama Umum Untuk Perangkat Pelindung Lonjakan." },
  { "en": "Apa Itu Koordinasi SPD (Surge Protective Device)?", "id": "Memastikan SPD Bekerja Bersama Secara Efektif." },
  { "en": "Apa Itu Arus Follow-On?", "id": "Arus Jaringan Mengikuti Pelepasan Lonjakan SPD." },
  { "en": "Apa Itu Tegangan Clamping SPD?", "id": "Tegangan Sisa Saat SPD Mengalihkan Lonjakan." },
  { "en": "Apa Itu MOV (Metal Oxide Varistor)?", "id": "Komponen Umum Dalam SPD (Surge Protective Device)." },
  { "en": "Apa Itu Tabung Pelepasan Gas (GDT)?", "id": "Komponen SPD Kemampuan Energi Tinggi." },
  { "en": "Apa Itu Dioda Silikon Longsoran (SAD)?", "id": "Komponen SPD Respon Sangat Cepat." },
  { "en": "Apa Itu SPD Hibrida?", "id": "Menggabungkan Beberapa Teknologi Perlindungan Lonjakan." },
  { "en": "Apa Itu Ikatan Ekuipotensial?", "id": "Menghubungkan Semua Bagian Konduktif Menyamakan Potensial." },
  { "en": "Apa Itu Sistem Terminasi Udara (Air Terminal)?", "id": "Bagian Sistem Proteksi Petir Penangkap Sambaran." },
  { "en": "Apa Itu Konduktor Turun (Down Conductor)?", "id": "Menghubungkan Terminasi Udara Ke Elektroda Tanah." },
  { "en": "Apa Itu Sistem Elektroda Tanah?", "id": "Bagian Sistem Proteksi Petir Penyebar Arus." },
  { "en": "Apa Itu Metode Bola Guling (Rolling Sphere)?", "id": "Metode Penentuan Zona Proteksi Sistem Petir." },
  { "en": "Apa Itu Metode Sudut Proteksi?", "id": "Metode Lain Penentuan Zona Proteksi Petir." },
  { "en": "Apa Itu Arus Impuls Petir?", "id": "Arus Puncak Sangat Tinggi Durasi Sangat Singkat." },
  { "en": "Apa Bentuk Gelombang Standar Impuls Petir?", "id": "Bentuk Gelombang 1.2/50 Mikrosekon." },
  { "en": "Apa Bentuk Gelombang Standar Impuls Switching?", "id": "Bentuk Gelombang 250/2500 Mikrosekon." },
  { "en": "Apa Itu Metode Jacobi Dalam Analisis Aliran Daya?", "id": "Metode Iteratif Menyelesaikan Persamaan Aliran Daya." },
  { "en": "Apa Itu Metode Gauss-Seidel Dalam Aliran Daya?", "id": "Metode Iteratif Lebih Cepat Dari Jacobi." },
  { "en": "Apa Itu Metode Newton-Raphson Dalam Aliran Daya?", "id": "Metode Iteratif Paling Cepat, Paling Kompleks." },
  { "en": "Apa Itu Matriks Jacobian Dalam Aliran Daya?", "id": "Matriks Turunan Parsial Digunakan Newton-Raphson." },
  { "en": "Apa Itu Bus PV Dalam Aliran Daya?", "id": "Bus Generator (Daya Aktif, Tegangan Diketahui)." },
  { "en": "Apa Itu Bus PQ Dalam Aliran Daya?", "id": "Bus Beban (Daya Aktif, Reaktif Diketahui)." },
  { "en": "Apa Itu Kawat Tembaga Berenamel?", "id": "Kawat Tembaga Dengan Lapisan Isolasi Tipis." },
  { "en": "Untuk Apa Kawat Tembaga Berenamel Digunakan?", "id": "Membuat Lilitan Transformator, Induktor, Motor." },
  { "en": "Apa Itu Konstanta Waktu Termal?", "id": "Ukuran Seberapa Cepat Objek Merespon Perubahan Suhu." },
  { "en": "Apa Itu Kertas Isolasi Listrik?", "id": "Kertas Khusus Isolasi Antar Lilitan Trafo." },
  { "en": "Apa Itu Minyak Transformator?", "id": "Cairan Isolasi Dan Pendingin Dalam Transformator." },
  { "en": "Apa Fungsi Dehidrator Silika Gel Transformator?", "id": "Menyerap Kelembaban Dari Udara Masuk Konservator." },
  { "en": "Apa Itu Uji Rasio Transformasi (TTR)?", "id": "Memverifikasi Rasio Lilitan Primer Terhadap Sekunder." },
  { "en": "Apa Itu Uji Resistansi Lilitan Transformator?", "id": "Memeriksa Integritas Koneksi Lilitan Transformator." },
  { "en": "Apa Itu Uji Faktor Daya Isolasi?", "id": "Mengevaluasi Kualitas Sistem Isolasi Peralatan." },
  { "en": "Apa Itu Sudut Kerugian Dielektrik?", "id": "Ukuran Ketidakefisienan Material Dielektrik." },
  { "en": "Apa Itu Uji Sweep Frequency Response Analysis (SFRA)?", "id": "Mendeteksi Deformasi Mekanis Lilitan Transformator." },
  { "en": "Apa Itu Uji Partial Discharge (PD)?", "id": "Mendeteksi Pelepasan Muatan Lokal Dalam Isolasi." },
  { "en": "Apa Itu Motor Universal?", "id": "Motor Dapat Beroperasi Pada Daya AC, DC." },
  { "en": "Di Mana Motor Universal Digunakan?", "id": "Bor Tangan, Blender, Penyedot Debu." },
  { "en": "Apa Itu Motor Reluktansi (Reluctance Motor)?", "id": "Motor Sinkron Tanpa Lilitan Medan Di Rotor." },
  { "en": "Apa Itu Motor Histeresis (Hysteresis Motor)?", "id": "Motor Sinkron Menggunakan Kerugian Histeresis Rotor." },
  { "en": "Apa Itu Rem Arus Eddy?", "id": "Sistem Pengereman Non-kontak Menggunakan Arus Eddy." },
  { "en": "Apa Itu Kontrol Skalar Motor AC?", "id": "Metode Kontrol Sederhana Menjaga Rasio V/Hz." },
  { "en": "Apa Itu Sensor Efek Hall Linear?", "id": "Output Tegangan Proporsional Kekuatan Medan Magnet." },
  { "en": "Apa Itu Sensor Efek Hall Latch?", "id": "Tetap Dalam Satu Keadaan Sampai Medan Berlawanan." },
  { "en": "Apa Itu Sakelar Buluh (Reed Switch)?", "id": "Sakelar Dalam Tabung Kaca Diaktifkan Magnet." },
  { "en": "Apa Itu Filter Comb?", "id": "Filter Dengan Respon Frekuensi Mirip Sisir." },
  { "en": "Apa Efek Dari Filter Comb?", "id": "Menghasilkan Gema Atau Efek Flanging Audio." },
  { "en": "Apa Itu Interpolasi Linear?", "id": "Estimasi Nilai Di Antara Dua Titik Data." },
  { "en": "Apa Itu Ekstrapolasi?", "id": "Estimasi Nilai Di Luar Rentang Titik Data." },
  { "en": "Apa Itu Algoritma LMS (Least Mean Squares)?", "id": "Algoritma Adaptif Untuk Filter Adaptif." },
  { "en": "Apa Itu Filter Adaptif?", "id": "Filter Yang Koefisiennya Dapat Menyesuaikan Diri." },
  { "en": "Apa Itu Pengenal Sistem (System Identification)?", "id": "Membuat Model Matematis Sistem Dari Data." },
  { "en": "Apa Itu White-Box Modeling?", "id": "Pemodelan Berdasarkan Prinsip Pertama (Hukum Fisika)." },
  { "en": "Apa Itu Black-Box Modeling?", "id": "Pemodelan Tanpa Pengetahuan Internal Sistem." },
  { "en": "Apa Itu Grey-Box Modeling?", "id": "Gabungan Pemodelan White-Box Dan Black-Box." },
  { "en": "Apa Itu Zero-Crossing Rate (ZCR)?", "id": "Laju Sinyal Berubah Tanda Dari Positif-Negatif." },
  { "en": "Apa Itu Centroid Spektral?", "id": "Ukuran Pusat Massa Spektrum Sinyal." },
  { "en": "Apa Itu Rolloff Spektral?", "id": "Frekuensi Di Bawahnya Sebagian Besar Energi Spektral." },
  { "en": "Apa Itu Kode Hamming (7,4)?", "id": "Mengkodekan 4 Bit Data Menjadi 7 Bit." },
  { "en": "Berapa Banyak Error Dapat Dideteksi Kode Hamming (7,4)?", "id": "Dapat Mendeteksi Hingga Dua Bit Error." },
  { "en": "Berapa Banyak Error Dapat Dikoreksi Kode Hamming (7,4)?", "id": "Dapat Mengoreksi Satu Bit Error." },
  { "en": "Apa Itu Matriks Generator (G) Dalam Pengkodean?", "id": "Matriks Untuk Mengkodekan Pesan." },
  { "en": "Apa Itu Matriks Pengecekan Paritas (H)?", "id": "Matriks Untuk Memeriksa Validitas Kata Kode." },
  { "en": "Apa Itu Dekoding Keputusan Keras (Hard-Decision)?", "id": "Dekoder Menggunakan Nilai Biner (0 Atau 1)." },
  { "en": "Apa Itu Dekoding Keputusan Lunak (Soft-Decision)?", "id": "Dekoder Menggunakan Probabilitas Atau Keandalan Bit." },
  { "en": "Mana Yang Kinerjanya Lebih Baik?", "id": "Dekoding Keputusan Lunak (Soft-Decision)." },
  { "en": "Apa Itu Model Kanal AWGN?", "id": "Model Kanal Komunikasi Dengan Derau Putih Gaussian." },
  { "en": "Apa Itu Probabilitas Error Bit (BER)?", "id": "Rasio Bit Error Terhadap Total Bit Terkirim." },
  { "en": "Apa Itu Space-Time Coding (STC)?", "id": "Teknik Pengkodean Untuk Sistem MIMO." },
  { "en": "Apa Itu Kode Alamouti?", "id": "Skema Space-Time Block Code (STBC) Sederhana." },
  { "en": "Apa Itu Massive MIMO?", "id": "Sistem MIMO Dengan Jumlah Antena Sangat Besar." },
  { "en": "Apa Itu Sistem FDD (Frequency Division Duplexing)?", "id": "Menggunakan Frekuensi Berbeda Untuk Kirim, Terima." },
  { "en": "Apa Itu Sistem TDD (Time Division Duplexing)?", "id": "Menggunakan Slot Waktu Berbeda Kirim, Terima." },
  { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Karakteristik Kanal Berubah Lebih Lambat Dari Sinyal." },
  { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Karakteristik Kanal Berubah Lebih Cepat Dari Sinyal." },
  { "en": "Apa Itu Koherensi Waktu Kanal?", "id": "Ukuran Berapa Lama Kanal Tetap Konstan." },
  { "en": "Apa Itu Koherensi Bandwidth Kanal?", "id": "Rentang Frekuensi Di Mana Kanal Dianggap Datar." },
  { "en": "Apa Itu Fading Datar (Flat Fading)?", "id": "Bandwidth Sinyal Lebih Kecil Dari Koherensi Bandwidth." },
  { "en": "Apa Itu Fading Selektif Frekuensi?", "id": "Bandwidth Sinyal Lebih Besar Dari Koherensi Bandwidth." },
  { "en": "Apa Itu Mode Gelombang TEM (Transverse Electro-Magnetic)?", "id": "Medan Listrik, Magnet Tegak Lurus Arah Propagasi." },
  { "en": "Di Mana Mode TEM (Transverse Electro-Magnetic) Ada?", "id": "Kabel Koaksial Dan Saluran Transmisi Dua Kawat." },
  { "en": "Apa Itu Atenuasi Hujan?", "id": "Pelemahan Sinyal Gelombang Mikro Akibat Hujan." },
  { "en": "Apa Itu Scintillation Ionosfer?", "id": "Fluktuasi Cepat Amplitudo, Fasa Sinyal Satelit." },
  { "en": "Apa Itu Fading Multipath?", "id": "Interferensi Akibat Sinyal Tiba Lewat Jalur Ganda." },
  { "en": "Apa Itu Delay Spread?", "id": "Perbedaan Waktu Tiba Antara Jalur Multipath." },
  { "en": "Apa Efek Delay Spread?", "id": "Menyebabkan Intersymbol Interference (ISI)." },
  { "en": "Apa Itu Rake Receiver?", "id": "Penerima Menggabungkan Sinyal Dari Jalur Multipath." },
  { "en": "Apa Itu Antena Cerdas (Smart Antenna)?", "id": "Antena Array Dengan Pemrosesan Sinyal Cerdas." },
  { "en": "Apa Itu Keanekaragaman Makro (Macrodiversity)?", "id": "Keanekaragaman Menggunakan Base Station Di Lokasi Berbeda." },
  { "en": "Apa Itu Sistem Distribusi Antena (DAS)?", "id": "Jaringan Antena Terpisah Terhubung Sumber Bersama." },
  { "en": "Di Mana DAS (Distributed Antenna System) Digunakan?", "id": "Meningkatkan Cakupan Nirkabel Di Dalam Gedung." },
  { "en": "Apa Itu Small Cell?", "id": "Base Station Nirkabel Daya Rendah, Jangkauan Pendek." },
  { "en": "Apa Tujuan Penggunaan Small Cell?", "id": "Meningkatkan Kapasitas Jaringan Di Area Padat." },
  { "en": "Apa Itu HetNet (Heterogeneous Network)?", "id": "Jaringan Nirkabel Campuran Sel Makro, Kecil." },
  { "en": "Apa Itu Self-Organizing Network (SON)?", "id": "Jaringan Otomatis Mengkonfigurasi, Mengoptimalkan Diri." },
  { "en": "Apa Itu Pemodelan Kanal Radio?", "id": "Menciptakan Model Matematis Perambatan Gelombang Radio." },
  { "en": "Apa Itu Ray Tracing?", "id": "Teknik Pemodelan Kanal Menelusuri Jalur Sinyal." },
  { "en": "Apa Itu Model Empiris Kanal?", "id": "Model Berdasarkan Pengukuran Statistik Tanpa Teori Fisik." },
  { "en": "Sebutkan Contoh Model Empiris Kanal?", "id": "Okumura, Hata, Dan COST 231." },
  { "en": "Apa Itu Model Deterministik Kanal?", "id": "Model Berdasarkan Hukum Fisika Perambatan." },
  { "en": "Apa Itu Noise Figure Penerima?", "id": "Ukuran Penambahan Derau Oleh Penerima." },
  { "en": "Apa Itu Suhu Derau Sistem?", "id": "Ukuran Derau Total Sistem Direferensikan Ke Input." },
  { "en": "Apa Itu Kebisingan Atmosfer?", "id": "Derau Radio Yang Dihasilkan Oleh Proses Atmosfer." },
  { "en": "Apa Itu Kebisingan Galaksi?", "id": "Derau Radio Yang Berasal Dari Luar Angkasa." },
  { "en": "Apa Itu Kebisingan Buatan Manusia (Man-Made Noise)?", "id": "Derau Dari Peralatan Elektronik, Mesin, Listrik." },
  { "en": "Apa Itu Sinyal Jamming?", "id": "Sinyal Interferensi Disengaja Untuk Mengganggu Komunikasi." },
  { "en": "Apa Itu Standar IEEE 802.11?", "id": "Kumpulan Standar Untuk Jaringan Wi-Fi." },
  { "en": "Apa Itu Standar IEEE 802.15?", "id": "Kumpulan Standar Untuk Jaringan Personal (Bluetooth)." },
  { "en": "Apa Itu Standar IEEE 802.16?", "id": "Kumpulan Standar Untuk Jaringan WiMAX." },
  { "en": "Apa Itu BSS (Basic Service Set)?", "id": "Unit Dasar Jaringan Wi-Fi." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Unik Jaringan Nirkabel Wi-Fi." },
  { "en": "Apa Itu BSSID (Basic Service Set Identifier)?", "id": "Alamat MAC Dari Access Point Jaringan." },
  { "en": "Apa Itu Mode Ad Hoc Dalam Wi-Fi?", "id": "Perangkat Terhubung Langsung Tanpa Access Point." },
  { "en": "Apa Itu Mode Infrastruktur Dalam Wi-Fi?", "id": "Perangkat Terhubung Melalui Access Point Pusat." },
  { "en": "Apa Itu RTS/CTS (Request to Send/Clear to Send)?", "id": "Mekanisme Menghindari Tabrakan Tersembunyi Di Wi-Fi." },
  { "en": "Apa Itu Masalah Terminal Tersembunyi?", "id": "Dua Terminal Tak Dapat Mendengar, Mengirim Bersamaan." },
  { "en": "Apa Itu Masalah Terminal Terpapar?", "id": "Terminal Menunda Pengiriman Karena Mendengar Transmisi Lain." },
  { "en": "Apa Itu Pemodelan Kanal Stokastik?", "id": "Model Kanal Berdasarkan Proses Acak Statistik." },
  { "en": "Apa Itu Matriks Kanal Dalam Sistem MIMO?", "id": "Matriks Menggambarkan Jalur Antara Setiap Antena." },
  { "en": "Apa Itu Derajat Kebebasan Spasial?", "id": "Jumlah Aliran Data Independen Didukung Kanal MIMO." },
  { "en": "Apa Itu Pengkodean Dirty Paper (DPC)?", "id": "Teknik Pra-pembatalan Interferensi Dikenal Di Pemancar." },
  { "en": "Apa Itu Teorema Pemisahan Sumber-Kanal?", "id": "Pengkodean Sumber, Kanal Dapat Dioptimalkan Terpisah." },
  { "en": "Apa Itu Filter Wiener?", "id": "Filter Optimal Mengurangi Derau Mean-Square Error." },
  { "en": "Apa Itu Estimator Bayes?", "id": "Estimator Berbasis Teori Probabilitas Bayes." },
  { "en": "Apa Itu Estimator Kemungkinan Maksimum (MLE)?", "id": "Estimator Memaksimalkan Fungsi Kemungkinan." },
  { "en": "Apa Itu Batas Cramer-Rao?", "id": "Batas Bawah Varians Estimator Tak Bias." },
  { "en": "Apa Itu Detektor Korelasi?", "id": "Penerima Optimal Mendeteksi Sinyal Dikenal Dalam Derau." },
  { "en": "Apa Itu Transformasi Karhunen-Loève (KLT)?", "id": "Transformasi Optimal Untuk Representasi Sinyal Acak." },
  { "en": "Apa Itu Analisis Komponen Utama (PCA)?", "id": "Teknik Statistik Yang Terkait Erat Dengan KLT." },
  { "en": "Apa Itu Proses Stokastik Stasioner?", "id": "Proses Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Proses Ergodik?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
  { "en": "Apa Itu Rangkaian Resistor Diskrit?", "id": "Jaringan Resistor Dengan Nilai Individual." },
  { "en": "Apa Itu Penyesuaian Laser Resistor?", "id": "Memotong Material Resistor Laser Untuk Presisi." },
  { "en": "Apa Itu Jaringan R-2R?", "id": "Jaringan Resistor Digunakan Dalam DAC." },
  { "en": "Apa Itu Termometer Resistor Platinum (PRT)?", "id": "Nama Lain Untuk RTD (Resistance Temperature Detector) Platina." },
  { "en": "Apa Itu Persamaan Steinhart-Hart?", "id": "Model Matematis Untuk Resistansi Termistor." },
  { "en": "Apa Itu Efek Peltier?", "id": "Arus Listrik Menghasilkan Perbedaan Suhu." },
  { "en": "Apa Itu Efek Thomson?", "id": "Pembangkitan Panas Akibat Arus, Gradien Suhu." },
  { "en": "Apa Itu Angka Jasa (Figure of Merit) Termoelektrik?", "id": "Ukuran Efisiensi Material Termoelektrik." },
  { "en": "Apa Itu Penyearah Jembatan Terkendali Penuh?", "id": "Penyearah Jembatan Menggunakan Empat Thyristor." },
  { "en": "Apa Itu Penyearah Jembatan Setengah Terkendali?", "id": "Penyearah Campuran Dioda Dan Thyristor." },
  { "en": "Apa Itu Sudut Penundaan (Delay Angle) Thyristor?", "id": "Sudut Fasa Di Mana Thyristor Dipicu." },
  { "en": "Apa Itu Sudut Konduksi?", "id": "Durasi Sudut Di Mana Thyristor Menghantar." },
  { "en": "Apa Itu Sudut Padam (Extinction Angle)?", "id": "Sudut Fasa Di Mana Arus Menjadi Nol." },
  { "en": "Apa Itu Kegagalan Komutasi (Commutation Failure)?", "id": "Thyristor Gagal Mati Pada Waktu Yang Tepat." },
  { "en": "Apa Itu Komutasi Jalur (Line Commutation)?", "id": "Thyristor Dimatikan Oleh Polaritas Tegangan Jalur." },
  { "en": "Apa Itu Komutasi Paksa (Forced Commutation)?", "id": "Menggunakan Sirkuit Eksternal Untuk Mematikan Thyristor." },
  { "en": "Apa Itu Inverter Jembatan Tunggal?", "id": "Inverter DC-AC Menggunakan Empat Sakelar." },
  { "en": "Apa Itu Inverter Setengah Jembatan?", "id": "Inverter DC-AC Menggunakan Dua Sakelar." },
  { "en": "Apa Itu Dead Time Dalam Inverter?", "id": "Penundaan Mencegah Sakelar Atas-Bawah ON Bersamaan." },
  { "en": "Apa Itu Shoot-Through Dalam Inverter?", "id": "Kondisi Kedua Sakelar Kaki Sama ON." },
  { "en": "Apa Itu Metode Eliminasi Harmonik Selektif (SHE)?", "id": "Teknik PWM Menghilangkan Harmonik Tertentu." },
  { "en": "Apa Itu Metode Vektor Ruang (Space Vector)?", "id": "Teknik PWM Mengoptimalkan Penggunaan Tegangan DC." },
  { "en": "Apa Itu Inverter Z-Source?", "id": "Inverter Dapat Menaikkan Tegangan DC Input." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Langsung Tanpa Tautan DC." },
  { "en": "Apa Itu Soft Starter Motor?", "id": "Mengurangi Tegangan Motor Bertahap Saat Start." },
  { "en": "Apa Tujuan Soft Starter?", "id": "Membatasi Arus Mula Dan Stres Mekanis." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Pengontrol Kecepatan Motor AC Mengubah Frekuensi." },
  { "en": "Apa Itu Kontrol Pengereman DC Motor Induksi?", "id": "Menginjeksikan Arus DC Ke Stator Motor." },
  { "en": "Apa Itu Pengereman Dinamis?", "id": "Mengubah Energi Motor Menjadi Panas Resistor." },
  { "en": "Apa Itu Pengereman Anti-colokan (Plugging)?", "id": "Membalik Urutan Fasa Untuk Pengereman Cepat." },
  { "en": "Apa Itu Motor Sinkron Magnet Permanen (PMSM)?", "id": "Motor Sinkron Dengan Magnet Permanen Di Rotor." },
  { "en": "Apa Itu Motor Reluktansi Tersaklar (SRM)?", "id": "Motor Tanpa Magnet, Lilitan Rotor." },
  { "en": "Apa Itu Motor Brushless DC (BLDC)?", "id": "Motor PMSM Dengan Bentuk Gelombang Back-EMF Trapesium." },
  { "en": "Apa Itu Komutasi Enam Langkah (Six-Step) BLDC?", "id": "Metode Sederhana Mengemudikan Motor BLDC." },
  { "en": "Apa Itu Komutasi Sinusoidal Motor PMSM?", "id": "Menghasilkan Torsi Halus, Mengurangi Riak Torsi." },
  { "en": "Apa Itu Sensor Hall Dalam Motor BLDC?", "id": "Mendeteksi Posisi Rotor Untuk Komutasi." },
  { "en": "Apa Itu Pengoperasian Tanpa Sensor (Sensorless) Motor?", "id": "Mengestimasi Posisi Rotor Dari Back-EMF." },
  { "en": "Apa Itu Fluks Bocor (Leakage Flux) Mesin Listrik?", "id": "Fluks Magnet Tidak Berkontribusi Produksi Torsi." },
  { "en": "Apa Itu Reaksi Jangkar (Armature Reaction)?", "id": "Efek Medan Magnet Jangkar Terhadap Medan Utama." },
  { "en": "Apa Itu Kutub Interpole (Commutating Poles)?", "id": "Kutub Tambahan Mengurangi Percikan Api Di Komutator." },
  { "en": "Apa Itu Lilitan Kompensasi (Compensating Winding)?", "id": "Lilitan Di Muka Kutub Menetralkan Reaksi Jangkar." },
  { "en": "Apa Itu Uji Tanpa Beban Motor?", "id": "Mengukur Kerugian Rotasional Dan Magnetisasi." },
  { "en": "Apa Itu Uji Rotor Terkunci (Locked Rotor)?", "id": "Mengukur Impedansi Lilitan Dan Arus Mula." },
  { "en": "Apa Itu Diagram Lingkaran Motor Induksi?", "id": "Representasi Grafis Kinerja Motor Induksi." },
  { "en": "Apa Itu Torsi Pull-Out (Breakdown Torque)?", "id": "Torsi Maksimum Yang Dapat Dihasilkan Motor." },
  { "en": "Apa Itu Torsi Awal (Starting Torque)?", "id": "Torsi Yang Dihasilkan Motor Saat Start." },
  { "en": "Apa Itu Motor Sangkar Ganda (Double Cage)?", "id": "Rotor Dengan Dua Sangkar Torsi Awal Tinggi." },
  { "en": "Apa Itu Motor Rotor Lilit (Wound Rotor)?", "id": "Motor Induksi Resistansi Rotor Dapat Diubah." },
  { "en": "Apa Itu Kelas Isolasi Motor?", "id": "Menunjukkan Batas Suhu Operasi Aman Lilitan." },
  { "en": "Apa Itu Service Factor Motor?", "id": "Kemampuan Motor Beroperasi Di Atas Daya Nominal." },
  { "en": "Apa Itu Rangka (Frame) NEMA Motor?", "id": "Standar Dimensi Fisik Motor Listrik." },
  { "en": "Apa Itu Pemanasan Harmonik?", "id": "Pemanasan Tambahan Akibat Arus Harmonik." },
  { "en": "Apa Itu Sistem Tenaga Tak Ditanahkan?", "id": "Sistem Tanpa Koneksi Intensional Ke Tanah." },
  { "en": "Apa Keuntungan Sistem Tak Ditanahkan?", "id": "Kontinuitas Layanan Saat Gangguan Tanah Pertama." },
  { "en": "Apa Kerugian Sistem Tak Ditanahkan?", "id": "Sulit Mendeteksi Gangguan, Potensi Tegangan Lebih." },
  { "en": "Apa Itu Sistem Pentanahan Impedansi Tinggi?", "id": "Membatasi Arus Gangguan Tanah Level Rendah." },
  { "en": "Apa Itu Tegangan Lebih Transien?", "id": "Lonjakan Tegangan Durasi Sangat Singkat." },
  { "en": "Apa Penyebab Tegangan Lebih Transien?", "id": "Sambaran Petir, Operasi Switching." },
  { "en": "Apa Itu Gelombang Berjalan (Traveling Wave)?", "id": "Perambatan Gangguan Tegangan Sepanjang Saluran." },
  { "en": "Apa Itu Koefisien Refleksi Tegangan?", "id": "Fraksi Gelombang Tegangan Yang Dipantulkan." },
  { "en": "Apa Itu Koefisien Transmisi Tegangan?", "id": "Fraksi Gelombang Tegangan Yang Diteruskan." },
  { "en": "Apa Itu Lattice Diagram?", "id": "Diagram Waktu-Posisi Menganalisis Pantulan Gelombang." },
  { "en": "Apa Itu Beban Kritis Saluran?", "id": "Nama Lain Surge Impedance Loading (SIL)." },
  { "en": "Apa Itu Konstanta Atenuasi?", "id": "Ukuran Pelemahan Gelombang Seiring Jarak." },
  { "en": "Apa Itu Konstanta Fasa?", "id": "Ukuran Pergeseran Fasa Gelombang Per Satuan Jarak." },
  { "en": "Apa Itu Kecepatan Propagasi?", "id": "Kecepatan Perambatan Gelombang Di Saluran." },
  { "en": "Apa Itu Kabel Konsentris?", "id": "Kabel Dengan Konduktor Pusat Dikelilingi Pelindung." },
  { "en": "Apa Itu Kabel Berisolasi Mineral (MI)?", "id": "Kabel Tahan Api Dengan Isolasi Magnesium Oksida." },
  { "en": "Apa Itu Bus Duct (Busway)?", "id": "Sistem Batang Konduktor Dalam Selungkup Logam." },
  { "en": "Apa Itu Uji Kontinuitas Konduktor Protektif?", "id": "Memastikan Koneksi Ground Memiliki Resistansi Rendah." },
  { "en": "Apa Itu Polaritas (Instalasi Listrik)?", "id": "Memastikan Konduktor Fasa, Netral Terhubung Benar." },
  { "en": "Apa Itu Impedansi Loop Gangguan Tanah?", "id": "Total Impedansi Jalur Saat Terjadi Gangguan." },
  { "en": "Mengapa Pengukuran Impedansi Loop Penting?", "id": "Memastikan Perangkat Proteksi Bekerja Tepat Waktu." },
  { "en": "Apa Itu Uji RCD (Residual Current Device)?", "id": "Memverifikasi RCD Trip Pada Arus, Waktu Tepat." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Fasa Mencapai Puncak." },
  { "en": "Apa Itu Indikator Urutan Fasa?", "id": "Alat Untuk Memeriksa Urutan Fasa." },
  { "en": "Apa Itu Tegangan Nominal Sistem?", "id": "Tegangan Referensi Suatu Sistem Listrik." },
  { "en": "Apa Itu Frekuensi Nominal Sistem?", "id": "Frekuensi Referensi Suatu Sistem Listrik." },
  { "en": "Apa Itu Studi Hubung Singkat?", "id": "Analisis Menentukan Arus Gangguan Potensial." },
  { "en": "Apa Itu Studi Koordinasi Proteksi?", "id": "Memastikan Perangkat Proteksi Bekerja Selektif." },
  { "en": "Apa Itu Selektivitas (Diskriminasi)?", "id": "Hanya Perangkat Proteksi Terdekat Gangguan Bekerja." },
  { "en": "Apa Itu Proteksi Cadangan (Backup Protection)?", "id": "Proteksi Lapisan Kedua Jika Proteksi Utama Gagal." },
  { "en": "Apa Itu Kurva Waktu-Arus (Time-Current Curve)?", "id": "Grafik Karakteristik Operasi Perangkat Proteksi." },
  { "en": "Apa Itu Kurva Kerusakan Peralatan?", "id": "Menunjukkan Batas Tahan Arus Peralatan." },
  { "en": "Apa Itu Studi Aliran Beban?", "id": "Analisis Tegangan, Arus, Aliran Daya Jaringan." },
  { "en": "Apa Itu Studi Bahaya Busur Api?", "id": "Analisis Menentukan Energi Insiden Busur Api." },
  { "en": "Apa Itu Energi Insiden?", "id": "Energi Termal Busur Api Diukur Jarak Tertentu." },
  { "en": "Apa Itu Kode Bar (Barcode)?", "id": "Representasi Optik Data Dapat Dibaca Mesin." },
  { "en": "Apa Itu QR (Quick Response) Code?", "id": "Kode Matriks Dua Dimensi." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Identifikasi Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Tag RFID (Radio-Frequency Identification)?", "id": "Perangkat Kecil Penyimpan Data Identifikasi." },
  { "en": "Apa Beda Tag RFID (Radio-Frequency Identification) Aktif Dan Pasif?", "id": "Aktif Memiliki Baterai, Pasif Tidak." },
  { "en": "Apa Itu Pembaca RFID (Radio-Frequency Identification)?", "id": "Membaca Informasi Dari Tag RFID." },
  { "en": "Apa Itu Backscatter Dalam RFID?", "id": "Cara Tag Pasif Mengirim Data Kembali." },
  { "en": "Apa Itu Pemrosesan Sinyal Analog?", "id": "Memproses Sinyal Dalam Bentuk Kontinunya." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital?", "id": "Memproses Sinyal Dalam Bentuk Diskret." },
  { "en": "Apa Keuntungan Pemrosesan Sinyal Digital?", "id": "Fleksibilitas, Akurasi, Dan Tahan Derau." },
  { "en": "Apa Itu Finite Impulse Response (FIR) Filter?", "id": "Filter Digital Dengan Respon Impuls Terbatas." },
  { "en": "Apa Itu Infinite Impulse Response (IIR) Filter?", "id": "Filter Digital Dengan Respon Impuls Tak Terbatas." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Osilasi Dekat Diskontinuitas Dalam Rekonstruksi Sinyal." },
  { "en": "Apa Itu Zero-Padding Dalam FFT?", "id": "Menambahkan Nol Ke Sinyal Peningkatan Resolusi." },
  { "en": "Apa Itu Leakage Spektral?", "id": "Penyebaran Energi Frekuensi Akibat Sinyal Terpotong." },
  { "en": "Apa Itu Fungsi Jendela (Windowing Function)?", "id": "Mengurangi Efek Leakage Spektral Dalam Analisis." },
  { "en": "Apa Itu Efek Picket-Fence?", "id": "Kehilangan Informasi Spektral Antara Titik FFT." },
  { "en": "Apa Itu Downsampling (Desimasi)?", "id": "Mengurangi Laju Sampel Suatu Sinyal Digital." },
  { "en": "Apa Itu Upsampling (Interpolasi)?", "id": "Meningkatkan Laju Sampel Suatu Sinyal Digital." },
  { "en": "Apa Itu Aliasing?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Laju Sampel Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Pengkodean Diferensial?", "id": "Mengkodekan Perbedaan Antara Sampel Berurutan." },
  { "en": "Apa Itu Delta Modulation?", "id": "Bentuk Sederhana Pengkodean Diferensial." },
  { "en": "Apa Itu Pulse Code Modulation (PCM)?", "id": "Metode Standar Mengubah Sinyal Analog Digital." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Memetakan Nilai Kontinu Ke Set Nilai Diskret." },
  { "en": "Apa Itu Kesalahan Kuantisasi?", "id": "Error Yang Timbul Dari Proses Kuantisasi." },
  { "en": "Apa Itu Dithering?", "id": "Menambahkan Derau Mereduksi Error Kuantisasi." },
  { "en": "Apa Itu Audio Digital?", "id": "Representasi Digital Dari Sinyal Suara." },
  { "en": "Apa Itu Laju Bit (Bit Rate)?", "id": "Jumlah Bit Diproses Per Satuan Waktu." },
  { "en": "Apa Itu Kompresi Audio Lossless?", "id": "Mengurangi Ukuran Berkas Tanpa Kehilangan Informasi." },
  { "en": "Apa Itu Kompresi Audio Lossy?", "id": "Mengurangi Ukuran Berkas Dengan Membuang Informasi." },
  { "en": "Sebutkan Contoh Format Audio Lossless?", "id": "FLAC, ALAC, Dan WAV." },
  { "en": "Sebutkan Contoh Format Audio Lossy?", "id": "MP3, AAC, Dan OGG." },
  { "en": "Apa Itu Model Psikoakustik?", "id": "Model Persepsi Pendengaran Manusia." },
  { "en": "Apa Itu Masking Auditori?", "id": "Satu Suara Membuat Suara Lain Tak Terdengar." },
  { "en": "Apa Itu Citra Digital?", "id": "Representasi Digital Dari Gambar Visual." },
  { "en": "Apa Itu Piksel (Pixel)?", "id": "Elemen Gambar Terkecil Dalam Citra Digital." },
  { "en": "Apa Itu Resolusi Gambar?", "id": "Jumlah Total Piksel Dalam Sebuah Gambar." },
  { "en": "Apa Itu Kedalaman Warna (Color Depth)?", "id": "Jumlah Bit Digunakan Untuk Setiap Piksel." },
  { "en": "Apa Itu Model Warna RGB?", "id": "Merah, Hijau, Biru (Red, Green, Blue)." },
  { "en": "Apa Itu Model Warna CMYK?", "id": "Cyan, Magenta, Kuning, Hitam (Cyan, Magenta, Yellow, Key)." },
  { "en": "Di Mana Model CMYK Digunakan?", "id": "Dalam Industri Percetakan." },
  { "en": "Apa Itu Grayscale?", "id": "Citra Hanya Dengan Nuansa Abu-abu." },
  { "en": "Apa Itu Citra Biner?", "id": "Citra Hanya Dengan Piksel Hitam, Putih." },
  { "en": "Apa Itu Histogram Citra?", "id": "Grafik Distribusi Intensitas Piksel." },
  { "en": "Apa Itu Ekualisasi Histogram?", "id": "Teknik Meningkatkan Kontras Citra." },
  { "en": "Apa Itu Konvolusi Dalam Pemrosesan Citra?", "id": "Operasi Matematis Untuk Filtering Citra." },
  { "en": "Apa Itu Kernel (Filter Mask)?", "id": "Matriks Kecil Digunakan Dalam Operasi Konvolusi." },
  { "en": "Apa Itu Filter Gaussian Blur?", "id": "Menghaluskan Citra Menggunakan Fungsi Gaussian." },
  { "en": "Apa Itu Filter Penajaman (Sharpening)?", "id": "Meningkatkan Detail Tepi Dalam Sebuah Citra." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Mengidentifikasi Batas Objek Dalam Citra." },
  { "en": "Sebutkan Operator Deteksi Tepi?", "id": "Sobel, Prewitt, Canny, Dan Laplacian." },
  { "en": "Apa Itu Segmentasi Citra?", "id": "Membagi Citra Menjadi Beberapa Wilayah." },
  { "en": "Apa Itu Thresholding?", "id": "Metode Segmentasi Citra Sederhana." },
  { "en": "Apa Itu Morfologi Matematis?", "id": "Teknik Pemrosesan Citra Berbasis Bentuk." },
  { "en": "Apa Itu Operasi Erosi (Erosion)?", "id": "Mengecilkan Area Terang Dalam Citra." },
  { "en": "Apa Itu Operasi Dilasi (Dilation)?", "id": "Memperluas Area Terang Dalam Citra." },
  { "en": "Apa Itu Operasi Pembukaan (Opening)?", "id": "Erosi Diikuti Dengan Dilasi." },
  { "en": "Apa Itu Operasi Penutupan (Closing)?", "id": "Dilasi Diikuti Dengan Erosi." },
  { "en": "Apa Kegunaan Operasi Pembukaan?", "id": "Menghilangkan Derau Bintik Kecil (Noise)." },
  { "en": "Apa Kegunaan Operasi Penutupan?", "id": "Menutup Lubang Kecil Di Dalam Objek." },
  { "en": "Apa Itu Pengenalan Karakter Optik (OCR)?", "id": "Mengonversi Teks Gambar Ke Teks Mesin." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Bidang AI Pengajaran Komputer Memahami Citra." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Arsitektur Jaringan Saraf Untuk Pemrosesan Citra." },
  { "en": "Apa Itu Lapisan Konvolusional?", "id": "Lapisan Utama Dalam CNN Mengekstraksi Fitur." },
  { "en": "Apa Itu Lapisan Pooling?", "id": "Mengurangi Dimensi Peta Fitur Dalam CNN." },
  { "en": "Apa Itu Fungsi Aktivasi?", "id": "Memperkenalkan Non-Linearitas Dalam Jaringan Saraf." },
  { "en": "Sebutkan Contoh Fungsi Aktivasi?", "id": "ReLU, Sigmoid, Dan Tanh." },
  { "en": "Apa Itu Backpropagation?", "id": "Algoritma Pelatihan Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Sub-bidang Machine Learning Menggunakan Jaringan Dalam." },
  { "en": "Apa Itu Machine Learning?", "id": "Sistem Komputer Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Terawasi (Supervised Learning)?", "id": "Belajar Dari Data Berlabel." },
  { "en": "Apa Itu Pembelajaran Tak Terawasi (Unsupervised Learning)?", "id": "Belajar Dari Data Tak Berlabel." },
  { "en": "Apa Itu Reinforcement Learning?", "id": "Belajar Berdasarkan Umpan Balik (Reward/Punishment)." },
  { "en": "Apa Itu Klasifikasi Dalam Machine Learning?", "id": "Memprediksi Label Kategori (Contoh: Kucing/Anjing)." },
  { "en": "Apa Itu Regresi Dalam Machine Learning?", "id": "Memprediksi Nilai Kontinu (Contoh: Harga Rumah)." },
  { "en": "Apa Itu Clustering?", "id": "Mengelompokkan Data Serupa Secara Bersamaan." },
  { "en": "Apa Itu Overfitting?", "id": "Model Bekerja Baik Data Latih, Buruk Data Baru." },
  { "en": "Apa Itu Underfitting?", "id": "Model Terlalu Sederhana, Tidak Dapat Menangkap Pola." },
  { "en": "Apa Itu Set Pelatihan (Training Set)?", "id": "Data Digunakan Untuk Melatih Model." },
  { "en": "Apa Itu Set Pengujian (Test Set)?", "id": "Data Digunakan Mengevaluasi Kinerja Model." },
  { "en": "Apa Itu Set Validasi?", "id": "Data Digunakan Menyetel Parameter Model." },
  { "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Teknik Mengevaluasi Model Secara Kokoh." },
  { "en": "Apa Itu Pohon Keputusan (Decision Tree)?", "id": "Model Machine Learning Berbentuk Struktur Pohon." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Algoritma Klasifikasi Mencari Hyperplane Optimal." },
  { "en": "Apa Itu K-Nearest Neighbors (KNN)?", "id": "Algoritma Klasifikasi Berdasarkan Tetangga Terdekat." },
  { "en": "Apa Itu Pengurangan Dimensi?", "id": "Mengurangi Jumlah Variabel Dalam Data." },
  { "en": "Apa Itu Principal Component Analysis (PCA)?", "id": "Teknik Populer Untuk Pengurangan Dimensi." },
  { "en": "Apa Itu Gradient Descent?", "id": "Algoritma Optimisasi Menemukan Minimum Fungsi." },
  { "en": "Apa Itu Learning Rate?", "id": "Parameter Mengontrol Ukuran Langkah Gradient Descent." },
  { "en": "Apa Itu Loss Function (Cost Function)?", "id": "Mengukur Seberapa Baik Kinerja Model." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Interaksi Komputer Dengan Bahasa Manusia." },
  { "en": "Apa Itu Tokenization?", "id": "Memecah Teks Menjadi Unit (Kata, Kalimat)." },
  { "en": "Apa Itu Stemming?", "id": "Mengurangi Kata Ke Bentuk Dasarnya." },
  { "en": "Apa Itu Lemmatization?", "id": "Mengurangi Kata Ke Bentuk Kamusnya." },
  { "en": "Apa Itu Stop Words?", "id": "Kata Umum Disaring Sebelum Diproses." },
  { "en": "Apa Itu Bag-of-Words (BoW)?", "id": "Representasi Teks Berdasarkan Frekuensi Kata." },
  { "en": "Apa Itu TF-IDF?", "id": "Pembobotan Kata (Term Frequency-Inverse Document Frequency)." },
  { "en": "Apa Itu Word Embedding?", "id": "Representasi Vektor Kata Dalam Ruang." },
  { "en": "Apa Itu Word2Vec?", "id": "Model Populer Membuat Word Embedding." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Jaringan Saraf Untuk Data Sekuensial." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN Mengatasi Masalah Vanishing Gradient." },
  { "en": "Apa Itu Transformer (Model Arsitektur)?", "id": "Arsitektur Perhatian Populer Dalam NLP." },
  { "en": "Apa Itu Fan-in Dalam Gerbang Logika?", "id": "Jumlah Input Sebuah Gerbang Logika." },
  { "en": "Apa Itu Fan-out Dalam Gerbang Logika?", "id": "Jumlah Input Dapat Digerakkan Satu Output." },
  { "en": "Apa Itu Margin Derau (Noise Margin)?", "id": "Toleransi Sirkuit Terhadap Derau Sinyal." },
  { "en": "Apa Itu Batas Derau Tinggi (NMH)?", "id": "Noise Margin Saat Output Logika Tinggi." },
  { "en": "Apa Itu Batas Derau Rendah (NML)?", "id": "Noise Margin Saat Output Logika Rendah." },
  { "en": "Apa Itu Disipasi Daya Statis?", "id": "Konsumsi Daya Saat Sirkuit Tidak Berganti Keadaan." },
  { "en": "Apa Itu Disipasi Daya Dinamis?", "id": "Konsumsi Daya Saat Sirkuit Berganti Keadaan." },
  { "en": "Apa Penyebab Utama Disipasi Daya Dinamis?", "id": "Pengisian Dan Pengosongan Kapasitansi Beban." },
  { "en": "Apa Itu Penundaan Propagasi (Propagation Delay)?", "id": "Waktu Sinyal Merambat Dari Input Ke Output." },
  { "en": "Apa Itu Produk Penundaan-Daya (Power-Delay Product)?", "id": "Ukuran Kinerja Untuk Keluarga Gerbang Logika." },
  { "en": "Apa Itu Sistem Bilangan Oktal?", "id": "Sistem Bilangan Berbasis Delapan (0-7)." },
  { "en": "Apa Itu Pengkodean Biner-Ke-Gray?", "id": "Mengonversi Bilangan Biner Ke Kode Gray." },
  { "en": "Apa Itu Pengkodean Gray-Ke-Biner?", "id": "Mengonversi Kode Gray Ke Bilangan Biner." },
  { "en": "Apa Itu Rangkaian Aritmetika?", "id": "Sirkuit Digital Melakukan Operasi Matematika." },
  { "en": "Apa Itu Penjumlah Bawaan-Riak (Ripple-Carry Adder)?", "id": "Penjumlah Sederhana Dengan Propagasi Bawaan Berurutan." },
  { "en": "Apa Itu Penjumlah Lihat-Depan-Bawaan (Look-Ahead Carry)?", "id": "Penjumlah Cepat Dengan Logika Prediksi Bawaan." },
  { "en": "Apa Itu Pengurang (Subtractor)?", "id": "Rangkaian Digital Melakukan Operasi Pengurangan." },
  { "en": "Bagaimana Pengurangan Biner Dilakukan?", "id": "Menggunakan Penjumlahan Dengan Komplemen Dua." },
  { "en": "Apa Itu Komplemen Satu?", "id": "Membalik Semua Bit Dalam Bilangan Biner." },
  { "en": "Apa Itu Komplemen Dua?", "id": "Komplemen Satu Ditambah Dengan Satu." },
  { "en": "Apa Itu Unit Logika Aritmetika (ALU)?", "id": "Bagian CPU Melakukan Operasi Aritmetika, Logika." },
  { "en": "Apa Itu Sel Memori Statis (SRAM Cell)?", "id": "Sel Memori Menggunakan Flip-Flop Menyimpan Bit." },
  { "en": "Berapa Transistor Dibutuhkan Sel SRAM (Static Random-Access Memory) Tipikal?", "id": "Enam Transistor (6T)." },
  { "en": "Apa Itu Sel Memori Dinamis (DRAM Cell)?", "id": "Sel Memori Menggunakan Kapasitor Menyimpan Bit." },
  { "en": "Berapa Komponen Utama Sel DRAM (Dynamic Random-Access Memory)?", "id": "Satu Transistor Dan Satu Kapasitor (1T1C)." },
  { "en": "Mengapa DRAM (Dynamic Random-Access Memory) Perlu Penyegaran?", "id": "Muatan Kapasitor Bocor Seiring Waktu." },
  { "en": "Mana Yang Kepadatannya Lebih Tinggi?", "id": "DRAM (Dynamic Random-Access Memory)." },
  { "en": "Mana Yang Lebih Cepat?", "id": "SRAM (Static Random-Access Memory)." },
  { "en": "Apa Itu Programmable Logic Array (PLA)?", "id": "Perangkat Logika Dengan Bidang AND, OR Terprogram." },
  { "en": "Apa Itu Programmable Array Logic (PAL)?", "id": "Perangkat Logika Bidang AND Terprogram, OR Tetap." },
  { "en": "Apa Itu Generic Array Logic (GAL)?", "id": "Jenis PAL (Programmable Array Logic) Yang Dapat Dihapus." },
  { "en": "Apa Itu Antarmuka?", "id": "Batas Bersama Antara Dua Sistem Berbeda." },
  { "en": "Apa Itu Handshaking?", "id": "Proses Pertukaran Sinyal Kontrol (Contoh: REQ/ACK)." },
  { "en": "Apa Itu Penyangga Tiga Keadaan (Tri-State Buffer)?", "id": "Output Dapat Tinggi, Rendah, Impedansi Tinggi." },
  { "en": "Kapan Penyangga Tiga Keadaan Digunakan?", "id": "Saat Beberapa Perangkat Berbagi Bus Data." },
  { "en": "Apa Itu Efek Piroelektrik?", "id": "Material Menghasilkan Tegangan Akibat Perubahan Suhu." },
  { "en": "Di Mana Efek Piroelektrik Digunakan?", "id": "Sensor Gerak Inframerah Pasif (PIR)." },
  { "en": "Apa Itu Sensor Inframerah Pasif (PIR)?", "id": "Sensor Pendeteksi Radiasi Panas Dari Objek." },
  { "en": "Apa Itu Efek Elektro-Optik?", "id": "Perubahan Sifat Optik Material Akibat Medan Listrik." },
  { "en": "Apa Itu Efek Pockels?", "id": "Efek Elektro-Optik Linear." },
  { "en": "Apa Itu Efek Kerr?", "id": "Efek Elektro-Optik Kuadratik." },
  { "en": "Di Mana Efek Kerr Digunakan?", "id": "Shutter Kamera Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu Sel Kerr?", "id": "Perangkat Berbasis Efek Kerr Memodulasi Cahaya." },
  { "en": "Apa Itu Efek Akusto-Optik?", "id": "Interaksi Antara Gelombang Suara Dan Cahaya." },
  { "en": "Apa Itu Modulator Akusto-Optik?", "id": "Memodulasi Sinyal Cahaya Menggunakan Gelombang Suara." },
  { "en": "Apa Itu Efek Magneto-Optik (Faraday Effect)?", "id": "Rotasi Polarisasi Cahaya Akibat Medan Magnet." },
  { "en": "Apa Itu Isolator Faraday?", "id": "Perangkat Optik Melewatkan Cahaya Hanya Satu Arah." },
  { "en": "Apa Itu Sistem Kontrol Adaptif?", "id": "Pengontrol Otomatis Menyesuaikan Parameternya." },
  { "en": "Apa Itu Sistem Kontrol Kuat (Robust)?", "id": "Pengontrol Bekerja Baik Meskipun Ada Ketidakpastian." },
  { "en": "Apa Itu Diagram Bode?", "id": "Plot Logaritmik Penguatan, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Dekade (Decade) Dalam Diagram Bode?", "id": "Rentang Frekuensi Dengan Rasio Sepuluh." },
  { "en": "Apa Itu Oktaf (Octave) Dalam Diagram Bode?", "id": "Rentang Frekuensi Dengan Rasio Dua." },
  { "en": "Apa Itu Model Kawat (Wire Model)?", "id": "Model Sederhana Interkoneksi Tanpa Penundaan." },
  { "en": "Apa Itu Penundaan Interkoneksi (Interconnect Delay)?", "id": "Penundaan Sinyal Saat Merambat Melalui Kawat." },
  { "en": "Apa Itu Konstanta Waktu Elmore?", "id": "Aproksimasi Penundaan Sinyal Dalam Jaringan RC." },
  { "en": "Apa Itu Skema Pengkodean NRZ (Non-Return-to-Zero)?", "id": "Level Tegangan Konstan Selama Durasi Bit." },
  { "en": "Apa Itu Skema Pengkodean RZ (Return-to-Zero)?", "id": "Sinyal Kembali Ke Nol Di Tengah Bit." },
  { "en": "Apa Itu Pengkodean Manchester Diferensial?", "id": "Transisi Awal Menunjukkan 0, Tidak Ada Transisi 1." },
  { "en": "Apa Itu Tembaga Annealed?", "id": "Tembaga Yang Dilunakkan Untuk Fleksibilitas." },
  { "en": "Apa Itu Tembaga Hard-Drawn?", "id": "Tembaga Kekuatan Mekanis Tinggi." },
  { "en": "Di Mana Tembaga Hard-Drawn Digunakan?", "id": "Saluran Udara Transmisi Listrik." },
  { "en": "Apa Itu Aluminium Conductor Steel Reinforced (ACSR)?", "id": "Konduktor Dengan Inti Baja, Lapisan Aluminium." },
  { "en": "Mengapa ACSR (Aluminium Conductor Steel Reinforced) Digunakan?", "id": "Rasio Kekuatan-Berat Sangat Baik." },
  { "en": "Apa Itu Kawat Bundled?", "id": "Menggunakan Beberapa Konduktor Per Fasa Transmisi." },
  { "en": "Apa Tujuan Kawat Bundled?", "id": "Mengurangi Kerugian Korona Dan Reaktansi Saluran." },
  { "en": "Apa Itu Transposisi Saluran Transmisi?", "id": "Pertukaran Posisi Fisik Konduktor Fasa." },
  { "en": "Apa Tujuan Transposisi Saluran?", "id": "Menyeimbangkan Induktansi Dan Kapasitansi Antar Fasa." },
  { "en": "Apa Itu SAG Saluran Transmisi?", "id": "Lengkungan Ke Bawah Konduktor Akibat Gravitasi." },
  { "en": "Mengapa Perhitungan SAG Penting?", "id": "Menjaga Jarak Aman Ke Tanah." },
  { "en": "Apa Itu Menara Transmisi (Tower)?", "id": "Struktur Penopang Konduktor Saluran Udara." },
  { "en": "Apa Itu Isolator Rantai (String Insulator)?", "id": "Rangkaian Piringan Isolator Untuk Tegangan Tinggi." },
  { "en": "Apa Itu Efisiensi Rantai Isolator?", "id": "Rasio Tegangan Flashover Rantai Terhadap Jumlah." },
  { "en": "Apa Itu Cincin Pelindung (Guard Ring)?", "id": "Cincin Logam Menyamakan Distribusi Tegangan." },
  { "en": "Apa Itu Kawat Tanah (Ground Wire)?", "id": "Kawat Teratas Menara Melindungi Dari Petir." },
  { "en": "Apa Itu Optical Ground Wire (OPGW)?", "id": "Kawat Tanah Mengandung Serat Optik Komunikasi." },
  { "en": "Apa Itu Getaran Aeolian?", "id": "Getaran Frekuensi Tinggi Akibat Angin." },
  { "en": "Apa Itu Peredam Getaran (Vibration Damper)?", "id": "Perangkat Meredam Getaran Konduktor Akibat Angin." },
  { "en": "Apa Itu Kawat Litz?", "id": "Kawat Untaian Terisolasi Mengurangi Efek Kulit." },
  { "en": "Apa Itu Kabel Berisolasi Kertas Diresapi Minyak?", "id": "Jenis Kabel Bawah Tanah Tegangan Tinggi." },
  { "en": "Apa Itu Kabel XLPE (Cross-Linked Polyethylene)?", "id": "Kabel Isolasi Polimer Umum Tegangan Menengah." },
  { "en": "Apa Itu Pelapisan (Sheath) Kabel?", "id": "Lapisan Pelindung Luar Kabel Bawah Tanah." },
  { "en": "Apa Itu Perisai (Armouring) Kabel?", "id": "Lapisan Baja Pelindung Kabel Dari Kerusakan Mekanis." },
  { "en": "Apa Itu Grading Kabel?", "id": "Metode Menyamakan Stres Dielektrik Dalam Isolasi." },
  { "en": "Apa Itu Grading Kapasitansi?", "id": "Menggunakan Lapisan Dielektrik Berbeda Untuk Grading." },
  { "en": "Apa Itu Inter-Sheath Grading?", "id": "Menggunakan Selubung Konduktif Antara Lapisan Isolasi." },
  { "en": "Apa Itu Tegangan Tembus Dielektrik?", "id": "Kuat Medan Listrik Maksimum Sebelum Isolator Gagal." },
  { "en": "Apa Satuan Kekuatan Dielektrik?", "id": "Kilovolt Per Milimeter (kV/mm)." },
  { "en": "Apa Itu Penuaan Isolasi?", "id": "Degradasi Sifat Isolasi Seiring Waktu." },
  { "en": "Apa Penyebab Penuaan Isolasi?", "id": "Stres Termal, Listrik, Dan Mekanis." },
  { "en": "Apa Itu Pohon Listrik (Electrical Treeing)?", "id": "Jalur Kerusakan Permanen Dalam Isolasi Padat." },
  { "en": "Apa Itu Pohon Air (Water Treeing)?", "id": "Degradasi Isolasi Polimer Akibat Kehadiran Air." },
  { "en": "Apa Itu Frekuensi Sudut (ω)?", "id": "Frekuensi Dalam Radian Per Detik." },
  { "en": "Bagaimana Hubungan ω Dan f?", "id": "ω = 2πf." },
  { "en": "Apa Itu Suseptansi Kapasitif?", "id": "Bagian Imajiner Admitansi Dari Kapasitor." },
  { "en": "Apa Itu Suseptansi Induktif?", "id": "Bagian Imajiner Admitansi Dari Induktor." },
  { "en": "Apa Itu Penguat Umpan Balik Tegangan-Seri?", "id": "Topologi Penguat Transkonduktansi." },
  { "en": "Apa Itu Penguat Umpan Balik Arus-Seri?", "id": "Topologi Penguat Transresistansi." },
  { "en": "Apa Itu Penguat Umpan Balik Arus-Shunt?", "id": "Topologi Penguat Arus." },
  { "en": "Apa Itu Penguat Umpan Balik Tegangan-Shunt?", "id": "Topologi Penguat Tegangan." },
  { "en": "Apa Itu Kinematika Maju Robot?", "id": "Menghitung Posisi Ujung Lengan Dari Sudut Sendi." },
  { "en": "Apa Itu Kinematika Terbalik Robot?", "id": "Menghitung Sudut Sendi Untuk Posisi Ujung Lengan." },
  { "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Semua Titik Dapat Dicapai Ujung Lengan." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Desain, Konstruksi, Operasi Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen Yang Dimiliki Robot." },
  { "en": "Apa Itu End-Effector Robot?", "id": "Alat Di Ujung Lengan Robot (Gripper)." },
  { "en": "Apa Itu Aktuator Robot?", "id": "Komponen Penggerak Sendi Robot (Motor)." },
  { "en": "Apa Itu Sensor Proximity?", "id": "Sensor Pendeteksi Keberadaan Objek Tanpa Kontak." },
  { "en": "Apa Itu Sensor Visi (Vision Sensor)?", "id": "Sensor Menggunakan Kamera Untuk 'Melihat'." },
  { "en": "Apa Itu Sensor Gaya/Torsi?", "id": "Mengukur Gaya Dan Torsi Interaksi Robot." },
  { "en": "Apa Itu Robot Kartesian?", "id": "Robot Bergerak Dalam Tiga Sumbu Linear." },
  { "en": "Apa Itu Robot SCARA?", "id": "Lengan Robot Selektif Sesuai Rakitan." },
  { "en": "Apa Itu Robot Artikulasi (Articulated)?", "id": "Robot Dengan Sendi Putar Mirip Lengan Manusia." },
  { "en": "Apa Itu Trajektori Robot?", "id": "Jalur Yang Ditempuh End-Effector Robot." },
  { "en": "Apa Itu Ruang Sendi (Joint Space)?", "id": "Koordinat Berdasarkan Sudut Setiap Sendi Robot." },
  { "en": "Apa Itu Ruang Tugas (Task Space)?", "id": "Koordinat Berdasarkan Posisi End-Effector." },
  { "en": "Apa Itu Jacobian Dalam Robotika?", "id": "Matriks Hubungan Kecepatan Sendi Dan End-Effector." },
  { "en": "Apa Itu Singularitas Robot?", "id": "Konfigurasi Robot Kehilangan Derajat Kebebasan." },
  { "en": "Apa Itu Otomasi Industri?", "id": "Penggunaan Sistem Kontrol Mengoperasikan Proses Industri." },
  { "en": "Apa Itu Programmable Logic Controller (PLC)?", "id": "Komputer Digital Untuk Otomasi Industri." },
  { "en": "Apa Itu Bahasa Pemrograman Ladder Logic?", "id": "Bahasa Grafis Populer Untuk PLC." },
  { "en": "Apa Itu Rung Dalam Ladder Logic?", "id": "Garis Horizontal Merepresentasikan Satu Operasi Logika." },
  { "en": "Apa Itu Timer Dalam PLC?", "id": "Fungsi Logika Menghitung Interval Waktu." },
  { "en": "Apa Itu Counter Dalam PLC?", "id": "Fungsi Logika Menghitung Jumlah Kejadian." },
  { "en": "Apa Itu Modul Input/Output (I/O) PLC?", "id": "Antarmuka PLC Dengan Sensor Dan Aktuator." },
  { "en": "Apa Beda I/O Diskrit Dan Analog?", "id": "Diskrit On/Off, Analog Nilai Kontinu." },
  { "en": "Apa Itu Waktu Siklus (Scan Time) PLC?", "id": "Waktu Eksekusi Program PLC Satu Kali." },
  { "en": "Apa Itu Distributed Control System (DCS)?", "id": "Sistem Kontrol Terdistribusi Untuk Proses Skala Besar." },
  { "en": "Apa Beda PLC Dan DCS?", "id": "DCS Lebih Terintegrasi Untuk Kontrol Proses." },
  { "en": "Apa Itu Perangkat Lunak HMI (Human-Machine Interface)?", "id": "Antarmuka Grafis Bagi Operator." },
  { "en": "Apa Itu Perangkat Lunak SCADA?", "id": "Pengawasan, Kontrol, Akuisisi Data." },
  { "en": "Apa Itu Protokol Modbus?", "id": "Protokol Komunikasi Serial Umum Industri." },
  { "en": "Apa Itu Protokol Profibus?", "id": "Standar Fieldbus Digital Kecepatan Tinggi." },
  { "en": "Apa Itu Fieldbus?", "id": "Jaringan Digital Dua Arah Antar Instrumen Lapangan." },
  { "en": "Apa Itu Protokol HART?", "id": "Protokol Komunikasi Hibrida Analog-Digital." },
  { "en": "Apa Itu Pengalamatan IP (Internet Protocol)?", "id": "Alamat Numerik Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Subnet Mask?", "id": "Menentukan Bagian Jaringan, Host Dari Alamat IP." },
  { "en": "Apa Itu Gateway Default?", "id": "Router Jalan Keluar Jaringan Lokal." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberi Alamat IP Otomatis Ke Perangkat." },
  { "en": "Apa Itu Alamat MAC (Media Access Control)?", "id": "Alamat Fisik Unik Antarmuka Jaringan." },
  { "en": "Apa Itu Switch Jaringan?", "id": "Menghubungkan Perangkat Dalam Jaringan Lokal." },
  { "en": "Apa Itu Router Jaringan?", "id": "Menghubungkan Jaringan Berbeda, Meneruskan Paket." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Jaringan Mengontrol Lalu Lintas." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Konseptual Tujuh Lapisan Jaringan." },
  { "en": "Sebutkan Lapisan Fisik Model OSI?", "id": "Lapisan 1 (Physical Layer)." },
  { "en": "Sebutkan Lapisan Transport Model OSI?", "id": "Lapisan 4 (Transport Layer)." },
  { "en": "Apa Itu Protokol TCP (Transmission Control Protocol)?", "id": "Protokol Transport Andal Berorientasi Koneksi." },
  { "en": "Apa Itu Protokol UDP (User Datagram Protocol)?", "id": "Protokol Transport Cepat, Tidak Andal." },
  { "en": "Apa Itu Port Jaringan?", "id": "Titik Akhir Komunikasi Spesifik Aplikasi." },
  { "en": "Apa Itu Socket Jaringan?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Memetakan Alamat IP Privat Ke Publik." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Koneksi Aman Melalui Jaringan Publik." },
  { "en": "Apa Itu Bandwidth?", "id": "Kapasitas Maksimum Transfer Data Suatu Jaringan." },
  { "en": "Apa Itu Jitter Dalam Jaringan?", "id": "Variasi Penundaan Paket Data." },
  { "en": "Apa Itu Kehilangan Paket (Packet Loss)?", "id": "Paket Data Gagal Mencapai Tujuannya." },
  { "en": "Apa Itu Quality of Service (QoS)?", "id": "Manajemen Lalu Lintas Jaringan Prioritas Aplikasi." },
  { "en": "Apa Itu Kabel Serat Optik?", "id": "Kabel Mengirim Data Sebagai Pulsa Cahaya." },
  { "en": "Apa Itu Konektor LC Serat Optik?", "id": "Konektor Serat Optik Faktor Bentuk Kecil." },
  { "en": "Apa Itu Konektor SC Serat Optik?", "id": "Konektor Serat Optik Populer (Push-pull)." },
  { "en": "Apa Itu Splicing Serat Optik?", "id": "Menyambung Dua Serat Optik Secara Permanen." },
  { "en": "Apa Beda Fusion Splicing Dan Mekanis?", "id": "Fusion Melebur, Mekanis Menjajarkan Secara Presisi." },
  { "en": "Apa Itu OTDR (Optical Time-Domain Reflectometer)?", "id": "Alat Uji Karakterisasi Serat Optik." },
  { "en": "Apa Fungsi OTDR (Optical Time-Domain Reflectometer)?", "id": "Menemukan Putus, Sambungan, Kerugian Serat." },
  { "en": "Apa Itu Pengukur Daya Optik?", "id": "Mengukur Kekuatan Sinyal Cahaya Serat." },
  { "en": "Apa Itu Sumber Cahaya Optik?", "id": "Menghasilkan Sinyal Cahaya Stabil Pengujian." },
  { "en": "Apa Itu Kerugian Sisipan (Insertion Loss)?", "id": "Kehilangan Daya Akibat Penambahan Komponen." },
  { "en": "Apa Itu Kerugian Balik (Return Loss)?", "id": "Ukuran Cahaya Dipantulkan Kembali Komponen." },
  { "en": "Apa Itu Atenuator Serat Optik?", "id": "Perangkat Mengurangi Kekuatan Sinyal Optik." },
  { "en": "Apa Itu Multiplexer Optik?", "id": "Menggabungkan Beberapa Sinyal Optik Satu Serat." },
  { "en": "Apa Itu Demultiplexer Optik?", "id": "Memisahkan Sinyal Optik Gabungan." },
  { "en": "Apa Itu Sirkulator Optik?", "id": "Perangkat Mengarahkan Sinyal Antar Port Optik." },
  { "en": "Apa Itu Kode Barcode Linear?", "id": "Barcode Satu Dimensi (Contoh: UPC)." },
  { "en": "Apa Itu Kode Barcode 2D?", "id": "Barcode Dua Dimensi (Contoh: QR Code)." },
  { "en": "Apa Itu Kontras Cetak Barcode?", "id": "Perbedaan Reflektansi Antara Batang, Spasi." },
  { "en": "Apa Itu Zona Tenang (Quiet Zone) Barcode?", "id": "Area Kosong Di Sekitar Barcode." },
  { "en": "Apa Itu Simbologi Barcode?", "id": "Aturan Spesifik Pengkodean Karakter Dalam Barcode." },
  { "en": "Apa Itu Check Digit?", "id": "Digit Tambahan Untuk Verifikasi Kesalahan Pembacaan." },
  { "en": "Apa Itu Pencitra Linear (Linear Imager)?", "id": "Jenis Pemindai Barcode Menggunakan Sensor CCD." },
  { "en": "Apa Itu Pemindai Laser?", "id": "Jenis Pemindai Barcode Menggunakan Sinar Laser." },
  { "en": "Apa Itu Pembaca Barcode Berbasis Kamera?", "id": "Menggunakan Kamera Digital Untuk Membaca Barcode." },
  { "en": "Apa Itu Sistem Tenaga Tiga Fasa Seimbang?", "id": "Tegangan, Arus Setiap Fasa Sama Besar." },
  { "en": "Apa Itu Daya Rata-rata?", "id": "Daya Nyata Yang Dikonsumsi Beban." },
  { "en": "Apa Itu Daya Sesaat?", "id": "Nilai Daya Pada Waktu Tertentu." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Bagaimana Sistem Merespon Sinyal Frekuensi Berbeda." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Sistem Terhadap Input Impuls Singkat." },
  { "en": "Apa Itu Respon Langkah (Step Response)?", "id": "Output Sistem Terhadap Input Langkah." },
  { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Mencapai 50% Nilai Akhir." },
  { "en": "Apa Itu Waktu Puncak (Peak Time)?", "id": "Waktu Mencapai Puncak Lewatan Pertama." },
  { "en": "Apa Itu Persentase Lewatan (%OS)?", "id": "Ukuran Seberapa Jauh Lewatan Melebihi Nilai." },
  { "en": "Apa Itu Sistem Orde Pertama?", "id": "Sistem Dideskripsikan Persamaan Diferensial Orde Satu." },
  { "en": "Apa Itu Sistem Orde Kedua?", "id": "Sistem Dideskripsikan Persamaan Diferensial Orde Dua." },
  { "en": "Apa Itu Respon Teredam Kritis?", "id": "Respon Tercepat Tanpa Lewatan (Overshoot)." },
  { "en": "Apa Itu Respon Kurang Teredam?", "id": "Respon Dengan Osilasi Sebelum Mencapai Kestabilan." },
  { "en": "Apa Itu Respon Lebih Teredam?", "id": "Respon Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Respon Tidak Teredam?", "id": "Respon Berosilasi Terus-menerus Tanpa Henti." },
  { "en": "Apa Itu Rasio Redaman (Damping Ratio)?", "id": "Parameter Penentu Jenis Respon Orde Kedua." },
  { "en": "Apa Itu Frekuensi Alami (Natural Frequency)?", "id": "Frekuensi Osilasi Sistem Tanpa Redaman." },
  { "en": "Apa Itu Sistem Stabil?", "id": "Output Terbatas Untuk Setiap Input Terbatas." },
  { "en": "Apa Itu Sistem Tidak Stabil?", "id": "Output Tumbuh Tak Terbatas Seiring Waktu." },
  { "en": "Apa Itu Sistem Kontrol Diskrit?", "id": "Sistem Kontrol Yang Beroperasi Pada Waktu Diskret." },
  { "en": "Apa Itu Zero-Order Hold (ZOH)?", "id": "Mengubah Sinyal Diskret Menjadi Sinyal Tangga." },
  { "en": "Apa Itu Transformasi-Z Bilinear?", "id": "Metode Pemetaan Dari Domain-S Ke Domain-Z." },
  { "en": "Apa Itu Pemetaan Kutub-Nol (Pole-Zero Matching)?", "id": "Metode Lain Konversi Filter Analog Ke Digital." },
  { "en": "Apa Itu Kuantitasi Koefisien Filter?", "id": "Error Akibat Representasi Koefisien Dengan Bit Terbatas." },
  { "en": "Apa Itu Osilasi Batas (Limit Cycle)?", "id": "Osilasi Tak Diinginkan Dalam Filter Digital." },
  { "en": "Apa Itu Skala (Scaling) Dalam Filter Digital?", "id": "Menyesuaikan Level Sinyal Mencegah Overflow." },
  { "en": "Apa Itu Overflow Aritmetika?", "id": "Hasil Operasi Melebihi Kapasitas Register." },
  { "en": "Apa Itu Saturasi Aritmetika?", "id": "Membatasi Hasil Operasi Ke Nilai Maksimum." },
  { "en": "Apa Itu Wrap-Around Aritmetika?", "id": "Hasil Overflow Melipat Ke Nilai Negatif." },
  { "en": "Apa Itu Pemrosesan Sinyal Multirate?", "id": "Memproses Sinyal Dengan Laju Sampel Berbeda." },
  { "en": "Apa Itu Bank Filter (Filter Bank)?", "id": "Kumpulan Filter Lolos Pita Memecah Sinyal." },
  { "en": "Apa Itu Bank Filter Kuadratur Cermin (QMF)?", "id": "Bank Filter Dua Kanal Rekonstruksi Sempurna." },
  { "en": "Apa Itu Pemrosesan Sub-band?", "id": "Memproses Setiap Pita Frekuensi Sinyal Secara Independen." },
  { "en": "Apa Itu Kode QR Mikro?", "id": "Versi Lebih Kecil Dari Kode QR." },
  { "en": "Apa Itu Kode Aztec?", "id": "Kode Matriks Dua Dimensi Dengan Pola Pusat." },
  { "en": "Apa Itu Data Matrix?", "id": "Kode Matriks Dua Dimensi Efisiensi Tinggi." },
  { "en": "Apa Itu PDF417?", "id": "Simbologi Barcode Bertumpuk Kapasitas Data Tinggi." },
  { "en": "Di Mana PDF417 Sering Digunakan?", "id": "Surat Izin Mengemudi (SIM), Dokumen Identitas." },
  { "en": "Apa Itu Optical Character Recognition (OCR)?", "id": "Pengenalan Karakter Optik." },
  { "en": "Apa Itu Intelligent Character Recognition (ICR)?", "id": "Pengenalan Karakter Tulisan Tangan." },
  { "en": "Apa Itu Optical Mark Recognition (OMR)?", "id": "Mendeteksi Tanda Pada Formulir (Lembar Jawaban)." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor Dengan Sifat Optik Unik." },
  { "en": "Di Mana Titik Kuantum Digunakan?", "id": "Layar Tampilan (QLED), Pencitraan Biologis." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Lapisan Tunggal Atom Karbon." },
  { "en": "Apa Sifat Luar Biasa Grafena?", "id": "Sangat Kuat, Ringan, Konduktivitas Tinggi." },
  { "en": "Apa Itu Tabung Nano Karbon (Carbon Nanotube)?", "id": "Struktur Silinder Dari Atom Karbon." },
  { "en": "Apa Itu Sistem Mikroelektromekanis (MEMS)?", "id": "Perangkat Mekanis, Listrik Berukuran Mikro." },
  { "en": "Apa Itu Akselerometer MEMS (Microelectromechanical Systems)?", "id": "Sensor Percepatan Berbasis Teknologi MEMS." },
  { "en": "Apa Itu Giroskop MEMS (Microelectromechanical Systems)?", "id": "Sensor Rotasi Berbasis Teknologi MEMS." },
  { "en": "Apa Itu Digital Micromirror Device (DMD)?", "id": "Array Cermin Mikro Pengontrol Cahaya." },
  { "en": "Di Mana DMD (Digital Micromirror Device) Digunakan?", "id": "Proyektor DLP (Digital Light Processing)." },
  { "en": "Apa Itu Lab-on-a-Chip?", "id": "Perangkat Miniaturisasi Fungsi Laboratorium." },
  { "en": "Apa Itu Mikrofluida (Microfluidics)?", "id": "Ilmu Manipulasi Cairan Dalam Saluran Mikro." },
  { "en": "Apa Itu Piezoelektrik?", "id": "Sifat Material Menghasilkan Listrik Saat Ditekan." },
  { "en": "Apa Itu Aktuator Piezoelektrik?", "id": "Menghasilkan Gerakan Presisi Dari Sinyal Listrik." },
  { "en": "Apa Itu Transduser Ultrasonik?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Ultrasonik." },
  { "en": "Apa Itu Material Shape-Memory Alloy (SMA)?", "id": "Logam Yang 'Mengingat' Bentuk Aslinya." },
  { "en": "Bagaimana SMA (Shape-Memory Alloy) Diaktifkan?", "id": "Dengan Pemanasan Menggunakan Arus Listrik." },
  { "en": "Apa Itu Otot Buatan Polimer Elektroaktif (EAP)?", "id": "Material Polimer Berubah Bentuk Akibat Listrik." },
  { "en": "Apa Itu Ethernet Industri?", "id": "Penggunaan Protokol Ethernet Dalam Lingkungan Industri." },
  { "en": "Apa Itu Profinet?", "id": "Standar Ethernet Industri Terbuka." },
  { "en": "Apa Itu EtherCAT?", "id": "Protokol Ethernet Waktu Nyata Kinerja Tinggi." },
  { "en": "Apa Itu EtherNet/IP?", "id": "Protokol Ethernet Industri Lainnya." },
  { "en": "Apa Itu Ethernet Deterministik?", "id": "Ethernet Dengan Penundaan Terprediksi, Jaminan Waktu." },
  { "en": "Apa Itu Time-Sensitive Networking (TSN)?", "id": "Kumpulan Standar IEEE Untuk Ethernet Deterministik." },
  { "en": "Apa Itu Sinkronisasi?", "id": "Menyelaraskan Operasi Berdasarkan Referensi Waktu Bersama." },
  { "en": "Apa Itu Master Clock?", "id": "Sumber Waktu Utama Dalam Sistem Tersinkronisasi." },
  { "en": "Apa Itu PTP (Precision Time Protocol)?", "id": "Protokol IEEE 1588 Sinkronisasi Waktu Akurat." },
  { "en": "Apa Itu Mode Transparent Clock PTP?", "id": "Switch Mengukur, Mengoreksi Penundaan Residen." },
  { "en": "Apa Itu Mode Boundary Clock PTP?", "id": "Switch Bertindak Sebagai Slave, Lalu Master." },
  { "en": "Apa Itu CANopen?", "id": "Protokol Lapisan Aplikasi Di Atas CAN Bus." },
  { "en": "Apa Itu DeviceNet?", "id": "Protokol Lapisan Aplikasi Lain Berbasis CAN." },
  { "en": "Apa Itu Lapisan Fisik (PHY)?", "id": "Sirkuit Antarmuka Transmisi Sinyal Fisik." },
  { "en": "Apa Itu Lapisan Kontrol Akses Media (MAC)?", "id": "Mengontrol Akses Ke Media Jaringan Bersama." },
  { "en": "Apa Itu Jembatan (Bridge) Jaringan?", "id": "Menghubungkan Dua Segmen Jaringan Di Lapisan 2." },
  { "en": "Apa Itu VLAN (Virtual Local Area Network)?", "id": "Jaringan Logis Dibuat Dalam Switch Fisik." },
  { "en": "Apa Keuntungan VLAN (Virtual Local Area Network)?", "id": "Meningkatkan Keamanan Dan Fleksibilitas Jaringan." },
  { "en": "Apa Itu Trunking Dalam VLAN?", "id": "Membawa Lalu Lintas Beberapa VLAN Antar Switch." },
  { "en": "Apa Itu Protokol IEEE 802.1Q?", "id": "Standar Untuk Penandaan Frame VLAN (Tagging)." },
  { "en": "Apa Itu Spanning Tree Protocol (STP)?", "id": "Mencegah Loop Dalam Jaringan Ethernet Switch." },
  { "en": "Apa Itu Root Bridge Dalam STP?", "id": "Switch Referensi Pusat Dalam Topologi Spanning Tree." },
  { "en": "Apa Itu PortFast?", "id": "Fitur STP Mempercepat Koneksi Perangkat Ujung." },
  { "en": "Apa Itu BPDU (Bridge Protocol Data Unit)?", "id": "Pesan Yang Digunakan Oleh STP." },
  { "en": "Apa Itu Link Aggregation (Port Trunking)?", "id": "Menggabungkan Beberapa Link Fisik Menjadi Satu." },
  { "en": "Apa Itu LACP (Link Aggregation Control Protocol)?", "id": "Protokol Untuk Negosiasi Link Aggregation." },
  { "en": "Apa Itu Power Sourcing Equipment (PSE)?", "id": "Perangkat Penyedia Daya Dalam PoE." },
  { "en": "Apa Itu Powered Device (PD)?", "id": "Perangkat Penerima Daya Dalam PoE." },
  { "en": "Apa Itu Kelas Daya PoE (Power over Ethernet)?", "id": "Menentukan Alokasi Daya Maksimum Untuk PD." },
  { "en": "Apa Itu PoE (Power over Ethernet) Pasif?", "id": "Memberikan Daya Tanpa Proses Negosiasi." },
  { "en": "Apa Itu Mode A PoE (Power over Ethernet)?", "id": "Daya Dikirim Melalui Pasangan Data." },
  { "en": "Apa Itu Mode B PoE (Power over Ethernet)?", "id": "Daya Dikirim Melalui Pasangan Cadangan." },
  { "en": "Apa Itu Sakelar Terkelola (Managed Switch)?", "id": "Switch Dengan Fitur Konfigurasi, Pemantauan." },
  { "en": "Apa Itu Sakelar Tak Terkelola (Unmanaged Switch)?", "id": "Switch Sederhana Tanpa Opsi Konfigurasi." },
  { "en": "Apa Itu Port Mirroring (SPAN)?", "id": "Menyalin Lalu Lintas Satu Port Ke Port Lain." },
  { "en": "Apa Itu SNMP (Simple Network Management Protocol)?", "id": "Protokol Pemantauan, Pengelolaan Perangkat Jaringan." },
  { "en": "Apa Itu MIB (Management Information Base)?", "id": "Database Objek Dikelola Oleh SNMP." },
  { "en": "Apa Itu Perintah Ping?", "id": "Menguji Jangkauan, Latensi Perangkat Jaringan." },
  { "en": "Apa Itu Perintah Traceroute?", "id": "Menampilkan Jalur Paket Melalui Jaringan." },
  { "en": "Apa Itu Kabel Straight-Through?", "id": "Kabel Jaringan Pin Terhubung Lurus." },
  { "en": "Apa Itu Kabel Crossover?", "id": "Kabel Jaringan Pasangan Kirim, Terima Disilang." },
  { "en": "Kapan Kabel Crossover Digunakan?", "id": "Menghubungkan Dua Perangkat Sejenis (Komputer-Komputer)." },
  { "en": "Apa Itu Auto MDI-X?", "id": "Fitur Otomatis Deteksi Tipe Kabel." },
  { "en": "Apa Itu Skema Pengkabelan T568A?", "id": "Standar Urutan Warna Kabel Ethernet." },
  { "en": "Apa Itu Skema Pengkabelan T568B?", "id": "Standar Urutan Warna Kabel Ethernet Lainnya." },
  { "en": "Apa Itu Punch-Down Tool?", "id": "Alat Memasukkan Kawat Ke Blok Koneksi." },
  { "en": "Apa Itu Patch Panel?", "id": "Panel Koneksi Pengorganisasian Kabel Jaringan." },
  { "en": "Apa Itu Kabel Patch?", "id": "Kabel Pendek Penghubung Perangkat Ke Patch Panel." },
  { "en": "Apa Itu Kabel Backbone?", "id": "Kabel Utama Penghubung Antar Jaringan." },
  { "en": "Apa Itu Horizontal Cabling?", "id": "Pengkabelan Dari Ruang Telekomunikasi Ke Area Kerja." },
  { "en": "Apa Itu Jaringan Optik Pasif (PON)?", "id": "Jaringan Serat Optik Tanpa Komponen Aktif." },
  { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Titik Pusat Dalam Jaringan PON." },
  { "en": "Apa Itu ONU/ONT (Optical Network Unit/Terminal)?", "id": "Perangkat Di Sisi Pelanggan PON." },
  { "en": "Apa Itu Splitter Optik?", "id": "Membagi Sinyal Optik Dari Satu Serat." },
  { "en": "Apa Itu WDM-PON?", "id": "PON Menggunakan Panjang Gelombang Berbeda." },
  { "en": "Apa Itu TDM-PON?", "id": "PON Menggunakan Pembagian Waktu." },
  { "en": "Apa Itu Protokol TCP Tiga Arah Jabat Tangan?", "id": "Proses Pembentukan Koneksi TCP (SYN, SYN-ACK, ACK)." },
  { "en": "Apa Itu Windowing TCP?", "id": "Mekanisme Kontrol Aliran Dalam TCP." },
  { "en": "Apa Itu Kontrol Kemacetan TCP?", "id": "Mekanisme TCP Menghindari Kemacetan Jaringan." },
  { "en": "Apa Itu Slow Start TCP?", "id": "Fase Awal Kontrol Kemacetan TCP." },
  { "en": "Apa Itu Algoritma Nagle?", "id": "Meningkatkan Efisiensi TCP Dengan Menggabungkan Paket Kecil." },
  { "en": "Apa Itu Sistem Marginally Stable?", "id": "Sistem Dengan Kutub Di Sumbu Imajiner." },
  { "en": "Apa Itu Respon Frekuensi Nol (DC Gain)?", "id": "Penguatan Sistem Pada Frekuensi Nol." },
  { "en": "Apa Itu Waktu Tunda Transportasi (Transport Lag)?", "id": "Penundaan Waktu Murni Dalam Sistem." },
  { "en": "Apa Itu Model Dead Time?", "id": "Model Matematis Untuk Waktu Tunda Transportasi." },
  { "en": "Apa Itu Kontrol Rasio?", "id": "Menjaga Rasio Dua Variabel Proses Konstan." },
  { "en": "Apa Itu Kontrol Selektif?", "id": "Memilih Satu Sinyal Kontrol Dari Beberapa." },
  { "en": "Apa Itu Kontrol Split-Range?", "id": "Satu Output Kontroler Mengatur Dua Aktuator." },
  { "en": "Apa Itu Estimator Kalman?", "id": "Algoritma Optimal Estimasi Keadaan Sistem." },
  { "en": "Apa Itu Extended Kalman Filter (EKF)?", "id": "Filter Kalman Untuk Sistem Non-linear." },
  { "en": "Apa Itu Unscented Kalman Filter (UKF)?", "id": "Filter Kalman Non-linear Alternatif EKF." },
  { "en": "Apa Itu Particle Filter?", "id": "Metode Estimasi Berbasis Simulasi Monte Carlo." },
  { "en": "Apa Itu Proses Wiener?", "id": "Model Matematis Untuk Gerak Brown." },
  { "en": "Apa Itu Persamaan Diferensial Stokastik?", "id": "Persamaan Diferensial Dengan Komponen Acak." },
  { "en": "Apa Itu Jaringan Petri (Petri Net)?", "id": "Alat Pemodelan Grafis Sistem Kejadian Diskret." },
  { "en": "Apa Itu Otomata Berwaktu (Timed Automata)?", "id": "Otomata Hingga Dengan Variabel Jam Waktu Nyata." },
  { "en": "Apa Itu Logika Temporal?", "id": "Logika Formal Untuk Bernalar Tentang Waktu." },
  { "en": "Apa Itu Model Checking?", "id": "Verifikasi Otomatis Sistem Terhadap Spesifikasi Formal." },
  { "en": "Apa Itu Bisimulasi?", "id": "Hubungan Ekuivalensi Antara Sistem Transisi." },
  { "en": "Apa Itu Metode Bisection?", "id": "Algoritma Pencarian Akar Numerik." },
  { "en": "Apa Itu Metode Newton-Raphson?", "id": "Algoritma Pencarian Akar Cepat, Konvergen." },
  { "en": "Apa Itu Interpolasi Polinomial?", "id": "Menemukan Polinomial Melalui Sekumpulan Titik." },
  { "en": "Apa Itu Interpolasi Spline?", "id": "Interpolasi Menggunakan Polinomial Potongan." },
  { "en": "Apa Itu Regresi Linear?", "id": "Mencocokkan Garis Lurus Ke Sekumpulan Data." },
  { "en": "Apa Itu Metode Kuadrat Terkecil (Least Squares)?", "id": "Metode Menemukan Kurva Paling Cocok." },
  { "en": "Apa Itu Dekomposisi Nilai Singular (SVD)?", "id": "Faktorisasi Matriks Menjadi Tiga Matriks." },
  { "en": "Apa Itu Nilai Eigen Dan Vektor Eigen?", "id": "Karakteristik Skalar, Vektor Transformasi Linear." },
  { "en": "Apa Itu Ortogonalisasi Gram-Schmidt?", "id": "Proses Mengubah Set Vektor Menjadi Ortogonal." },
  { "en": "Apa Itu Ruang Vektor?", "id": "Kumpulan Vektor Dengan Operasi Penjumlahan, Perkalian." },
  { "en": "Apa Itu Basis Ruang Vektor?", "id": "Set Vektor Independen Linear Pembangun Ruang." },
  { "en": "Apa Itu Transformasi Linear?", "id": "Pemetaan Antar Ruang Vektor Menjaga Operasi." },
  { "en": "Apa Itu Ruang Nol (Kernel) Matriks?", "id": "Set Vektor Dipetakan Ke Vektor Nol." },
  { "en": "Apa Itu Ruang Kolom Matriks?", "id": "Rentang Vektor Kolom Suatu Matriks." },
  { "en": "Apa Itu Pangkat (Rank) Matriks?", "id": "Dimensi Ruang Kolom (Atau Baris)." },
  { "en": "Apa Itu Determinan Matriks?", "id": "Nilai Skalar Terkait Matriks Persegi." },
  { "en": "Apa Itu Matriks Invers?", "id": "Matriks Saat Dikalikan Menghasilkan Matriks Identitas." },
  { "en": "Apa Itu Adjoin Matriks?", "id": "Transpos Dari Matriks Kofaktor." },
  { "en": "Apa Itu Aturan Cramer?", "id": "Solusi Sistem Persamaan Linear Menggunakan Determinan." },
  { "en": "Apa Itu Eliminasi Gauss?", "id": "Algoritma Menyelesaikan Sistem Persamaan Linear." },
  { "en": "Apa Itu Dekomposisi LU?", "id": "Faktorisasi Matriks Menjadi Matriks Segitiga Bawah, Atas." },
  { "en": "Apa Itu Dekomposisi Cholesky?", "id": "Faktorisasi Matriks Simetris Definit Positif." },
  { "en": "Apa Itu Pivot Dalam Eliminasi Gauss?", "id": "Memilih Elemen Terbesar Untuk Kestabilan Numerik." },
  { "en": "Apa Itu Bilangan Kondisi Matriks?", "id": "Ukuran Sensitivitas Solusi Terhadap Error." },
  { "en": "Apa Itu Metode Iteratif?", "id": "Menemukan Solusi Dengan Aproksimasi Berulang." },
  { "en": "Apa Itu Konvergensi?", "id": "Urutan Aproksimasi Mendekati Solusi Sebenarnya." },
  { "en": "Apa Itu Kesalahan Pembulatan (Round-off Error)?", "id": "Error Akibat Representasi Bilangan Terbatas." },
  { "en": "Apa Itu Kesalahan Pemotongan (Truncation Error)?", "id": "Error Akibat Aproksimasi Proses Tak Terhingga." },
  { "en": "Apa Itu Aritmetika Titik Mengambang (Floating-Point)?", "id": "Representasi Komputer Untuk Bilangan Riil." },
  { "en": "Apa Itu Standar IEEE 754?", "id": "Standar Teknis Untuk Aritmetika Titik Mengambang." },
  { "en": "Apa Itu Presisi Tunggal (Single Precision)?", "id": "Format Titik Mengambang 32-bit." },
  { "en": "Apa Itu Presisi Ganda (Double Precision)?", "id": "Format Titik Mengambang 64-bit." },
  { "en": "Apa Itu Denormalized Number?", "id": "Bilangan Titik Mengambang Sangat Kecil." },
  { "en": "Apa Itu Not a Number (NaN)?", "id": "Nilai Aritmetika Mewakili Hasil Tak Terdefinisi." },
  { "en": "Apa Itu Infinity Dalam Komputer?", "id": "Nilai Mewakili Tak Terhingga Positif, Negatif." },
  { "en": "Apa Itu Underflow Aritmetika?", "id": "Hasil Terlalu Kecil Untuk Direpresentasikan." },
  { "en": "Apa Itu Analisis Numerik?", "id": "Studi Algoritma Menggunakan Aproksimasi Numerik." },
  { "en": "Apa Itu Derivasi Numerik?", "id": "Menghitung Aproksimasi Derivatif Fungsi." },
  { "en": "Apa Itu Integrasi Numerik (Quadrature)?", "id": "Menghitung Aproksimasi Integral Tentu." },
  { "en": "Apa Itu Aturan Trapesium?", "id": "Metode Sederhana Integrasi Numerik." },
  { "en": "Apa Itu Aturan Simpson?", "id": "Metode Integrasi Numerik Lebih Akurat." },
  { "en": "Apa Itu Metode Runge-Kutta?", "id": "Keluarga Metode Penyelesai Persamaan Diferensial." },
  { "en": "Apa Itu Masalah Nilai Batas?", "id": "Persamaan Diferensial Dengan Kondisi Di Batas." },
  { "en": "Apa Itu Masalah Nilai Awal?", "id": "Persamaan Diferensial Dengan Kondisi Awal." },
  { "en": "Apa Itu Metode Shooting?", "id": "Menyelesaikan Masalah Nilai Batas." },
  { "en": "Apa Itu Metode Beda Hingga?", "id": "Mengaproksimasi Derivatif Dengan Rumus Beda." },
  { "en": "Apa Itu Stabilitas Numerik?", "id": "Sifat Algoritma Di Mana Error Tidak Tumbuh." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Menguraikan Fungsi Menjadi Fungsi Sinusoidal." },
  { "en": "Apa Itu Konvergensi Seragam?", "id": "Jenis Konvergensi Lebih Kuat Dari Titik-demi-Titik." },
  { "en": "Apa Itu Fungsi Ortogonal?", "id": "Dua Fungsi Integral Hasil Kalinya Nol." },
  { "en": "Apa Itu Polinomial Legendre?", "id": "Keluarga Polinomial Ortogonal." },
  { "en": "Apa Itu Polinomial Chebyshev?", "id": "Keluarga Polinomial Ortogonal Terkait Trigonometri." },
  { "en": "Apa Itu Fungsi Bessel?", "id": "Solusi Persamaan Diferensial Bessel." },
  { "en": "Apa Itu Persamaan Gelombang?", "id": "Persamaan Diferensial Parsial Mendeskripsikan Gelombang." },
  { "en": "Apa Itu Persamaan Panas?", "id": "Mendeskripsikan Distribusi Panas Dalam Wilayah." },
  { "en": "Apa Itu Persamaan Laplace?", "id": "Mendeskripsikan Perilaku Potensial Listrik, Gravitasi." },
  { "en": "Apa Itu Persamaan Poisson?", "id": "Generalisasi Persamaan Laplace." },
  { "en": "Apa Itu Kondisi Batas Dirichlet?", "id": "Menentukan Nilai Fungsi Di Batas." },
  { "en": "Apa Itu Kondisi Batas Neumann?", "id": "Menentukan Turunan Normal Fungsi Di Batas." },
  { "en": "Apa Itu Vektor Satuan (Unit Vector)?", "id": "Vektor Dengan Panjang Tepat Satu." },
  { "en": "Apa Itu Perkalian Titik (Dot Product)?", "id": "Operasi Dua Vektor Menghasilkan Skalar." },
  { "en": "Apa Itu Perkalian Silang (Cross Product)?", "id": "Operasi Dua Vektor Menghasilkan Vektor." },
  { "en": "Apa Itu Gradien?", "id": "Operator Vektor Menunjukkan Laju Perubahan Maksimum." },
  { "en": "Apa Itu Divergensi?", "id": "Operator Vektor Ukuran Sumber Skalar." },
  { "en": "Apa Itu Curl?", "id": "Operator Vektor Ukuran Rotasi Vektor." },
  { "en": "Apa Itu Teorema Divergensi (Gauss)?", "id": "Menghubungkan Fluks Melalui Permukaan Dengan Divergensi." },
  { "en": "Apa Itu Teorema Stokes?", "id": "Menghubungkan Integral Garis Dengan Curl." },
  { "en": "Apa Itu Identitas Green?", "id": "Hubungan Integral Melibatkan Turunan Fungsi." },
  { "en": "Apa Itu Sistem Koordinat Kartesian?", "id": "Sistem Koordinat Menggunakan Sumbu Saling Tegak Lurus." },
  { "en": "Apa Itu Sistem Koordinat Polar?", "id": "Sistem Koordinat Berbasis Jarak, Sudut." },
  { "en": "Apa Itu Sistem Koordinat Silinder?", "id": "Ekstensi Tiga Dimensi Koordinat Polar." },
  { "en": "Apa Itu Sistem Koordinat Bola?", "id": "Sistem Koordinat Tiga Dimensi Berbasis Jarak, Sudut." },
  { "en": "Apa Itu Jacobian (Transformasi Koordinat)?", "id": "Faktor Penskalaan Dalam Perubahan Variabel Integral." },
  { "en": "Apa Itu Bilangan Kompleks?", "id": "Bilangan Bentuk a + bi." },
  { "en": "Apa Itu Konjugat Kompleks?", "id": "Mengubah Tanda Bagian Imajiner." },
  { "en": "Apa Itu Rumus Euler?", "id": "Menghubungkan Fungsi Eksponensial Kompleks, Trigonometri." },
  { "en": "Apa Itu Identitas Euler?", "id": "Kasus Khusus Rumus Euler (e^iπ + 1 = 0)." },
  { "en": "Apa Itu Bidang Kompleks?", "id": "Representasi Geometris Bilangan Kompleks." },
  { "en": "Apa Itu Analisis Kompleks?", "id": "Studi Fungsi Dengan Variabel Kompleks." },
  { "en": "Apa Itu Fungsi Holomorfik (Analitik)?", "id": "Fungsi Kompleks Dapat Diturunkan." },
  { "en": "Apa Itu Persamaan Cauchy-Riemann?", "id": "Syarat Fungsi Kompleks Menjadi Holomorfik." },
  { "en": "Apa Itu Teorema Integral Cauchy?", "id": "Integral Garis Fungsi Holomorfik Adalah Nol." },
  { "en": "Apa Itu Rumus Integral Cauchy?", "id": "Menghitung Nilai Fungsi Di Dalam Kontur." },
  { "en": "Apa Itu Deret Laurent?", "id": "Generalisasi Deret Taylor Untuk Fungsi Kompleks." },
  { "en": "Apa Itu Residu Dalam Analisis Kompleks?", "id": "Koefisien Suku (Z-Z₀)⁻¹ Deret Laurent." },
  { "en": "Apa Itu Teorema Residu?", "id": "Menghitung Integral Kontur Menggunakan Residu." },
  { "en": "Apa Itu Kutub (Pole) Fungsi Kompleks?", "id": "Singularitas Di Mana Fungsi Menuju Tak Terhingga." },
  { "en": "Apa Itu Nol (Zero) Fungsi Kompleks?", "id": "Titik Di Mana Nilai Fungsi Adalah Nol." },
  { "en": "Apa Itu Singularitas Esensial?", "id": "Singularitas Yang Bukan Kutub Atau Dapat Dihapus." },
  { "en": "Apa Itu Pemetaan Konformal?", "id": "Transformasi Kompleks Yang Mempertahankan Sudut." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Mengubah Fungsi Waktu Menjadi Fungsi Frekuensi Kompleks." },
  { "en": "Apa Itu Domain-S?", "id": "Domain Frekuensi Kompleks Dalam Transformasi Laplace." },
  { "en": "Apa Itu Transformasi Laplace Invers?", "id": "Mengubah Fungsi Domain-S Kembali Ke Domain Waktu." },
  { "en": "Apa Itu Teorema Nilai Awal?", "id": "Menentukan Nilai Awal Sinyal Dari Transformasi Laplace." },
  { "en": "Apa Itu Teorema Nilai Akhir?", "id": "Menentukan Nilai Akhir Sinyal Dari Transformasi Laplace." },
  { "en": "Apa Itu Teorema Konvolusi?", "id": "Konvolusi Di Domain Waktu Menjadi Perkalian." },
  { "en": "Apa Itu Fungsi Transfer Sistem?", "id": "Rasio Transformasi Laplace Output Terhadap Input." },
  { "en": "Apa Itu Probabilitas?", "id": "Ukuran Kemungkinan Terjadinya Suatu Peristiwa." },
  { "en": "Apa Itu Variabel Acak?", "id": "Variabel Yang Nilainya Hasil Eksperimen Acak." },
  { "en": "Apa Itu Fungsi Kepadatan Probabilitas (PDF)?", "id": "Mendeskripsikan Probabilitas Relatif Variabel Acak Kontinu." },
  { "en": "Apa Itu Fungsi Massa Probabilitas (PMF)?", "id": "Mendeskripsikan Probabilitas Variabel Acak Diskret." },
  { "en": "Apa Itu Fungsi Distribusi Kumulatif (CDF)?", "id": "Probabilitas Variabel Acak Kurang Dari Nilai Tertentu." },
  { "en": "Apa Itu Nilai Harapan (Expected Value)?", "id": "Rata-rata Jangka Panjang Suatu Variabel Acak." },
  { "en": "Apa Itu Varians?", "id": "Ukuran Sebaran Suatu Variabel Acak." },
  { "en": "Apa Itu Simpangan Baku (Standard Deviation)?", "id": "Akar Kuadrat Dari Varians." },
  { "en": "Apa Itu Distribusi Seragam (Uniform)?", "id": "Semua Hasil Memiliki Probabilitas Yang Sama." },
  { "en": "Apa Itu Distribusi Bernoulli?", "id": "Distribusi Percobaan Tunggal (Sukses/Gagal)." },
  { "en": "Apa Itu Distribusi Binomial?", "id": "Jumlah Sukses Dalam N Percobaan Bernoulli." },
  { "en": "Apa Itu Distribusi Poisson?", "id": "Probabilitas Jumlah Kejadian Dalam Interval." },
  { "en": "Apa Itu Distribusi Normal (Gaussian)?", "id": "Distribusi Berbentuk Lonceng Yang Umum." },
  { "en": "Apa Itu Teorema Batas Pusat (Central Limit Theorem)?", "id": "Jumlah Variabel Acak Cenderung Berdistribusi Normal." },
  { "en": "Apa Itu Kovarians?", "id": "Ukuran Hubungan Linear Antara Dua Variabel Acak." },
  { "en": "Apa Itu Koefisien Korelasi?", "id": "Kovarians Yang Dinormalisasi (-1 Hingga 1)." },
  { "en": "Apa Itu Independensi Statistik?", "id": "Satu Variabel Tidak Memberi Informasi Tentang Lainnya." },
  { "en": "Apa Itu Aturan Bayes?", "id": "Menghitung Probabilitas Bersyarat Terbalik." },
  { "en": "Apa Itu Proses Markov?", "id": "Proses Stokastik Dengan Sifat Tanpa Memori." },
  { "en": "Apa Itu Rantai Markov?", "id": "Proses Markov Dengan Ruang Keadaan Diskret." },
  { "en": "Apa Itu Matriks Transisi?", "id": "Probabilitas Transisi Antar Keadaan Rantai Markov." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Mengenai Antrian (Waiting Lines)." },
  { "en": "Apa Itu Notasi Kendall?", "id": "Notasi Standar Untuk Mendeskripsikan Model Antrian." },
  { "en": "Apa Itu Proses Kelahiran-Kematian?", "id": "Jenis Rantai Markov Untuk Model Antrian." },
  { "en": "Apa Itu Hukum Little?", "id": "Menghubungkan Jumlah Pelanggan, Laju Kedatangan, Waktu Tunggu." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Metode Komputasi Menggunakan Sampling Acak." },
  { "en": "Apa Itu Generator Bilangan Acak Semu?", "id": "Algoritma Penghasil Urutan Bilangan Tampak Acak." },
  { "en": "Apa Itu Benih (Seed) Generator Acak?", "id": "Nilai Awal Untuk Memulai Urutan Acak." },
  { "en": "Apa Itu Metode Transformasi Invers?", "id": "Menghasilkan Sampel Acak Dari Distribusi Apapun." },
  { "en": "Apa Itu Metode Penerimaan-Penolakan?", "id": "Metode Lain Untuk Menghasilkan Sampel Acak." },
  { "en": "Apa Itu Pengambilan Sampel Gibbs?", "id": "Algoritma MCMC Menghasilkan Urutan Sampel." },
  { "en": "Apa Itu Algoritma Metropolis-Hastings?", "id": "Algoritma MCMC (Markov Chain Monte Carlo) Umum." },
  { "en": "Apa Itu Bootstrap Statistik?", "id": "Metode Pengambilan Sampel Ulang Untuk Estimasi." },
  { "en": "Apa Itu Inferensi Bayes?", "id": "Metode Inferensi Statistik Berdasarkan Teorema Bayes." },
  { "en": "Apa Itu Distribusi Prior?", "id": "Keyakinan Awal Tentang Parameter." },
  { "en": "Apa Itu Distribusi Posterior?", "id": "Keyakinan Tentang Parameter Setelah Melihat Data." },
  { "en": "Apa Itu Uji Hipotesis?", "id": "Prosedur Statistik Memutuskan Antara Dua Hipotesis." },
  { "en": "Apa Itu Hipotesis Nol (H0)?", "id": "Pernyataan Default Yang Diuji." },
  { "en": "Apa Itu Hipotesis Alternatif (H1)?", "id": "Pernyataan Yang Diterima Jika H0 Ditolak." },
  { "en": "Apa Itu Kesalahan Tipe I?", "id": "Menolak Hipotesis Nol Yang Benar." },
  { "en": "Apa Itu Kesalahan Tipe II?", "id": "Menerima Hipotesis Nol Yang Salah." },
  { "en": "Apa Itu P-Value?", "id": "Probabilitas Mendapatkan Hasil Separah Yang Diamati." },
  { "en": "Apa Itu Tingkat Signifikansi (Alpha)?", "id": "Ambang Batas Untuk Menolak Hipotesis Nol." },
  { "en": "Apa Itu Interval Kepercayaan?", "id": "Rentang Estimasi Untuk Parameter Populasi." },
  { "en": "Apa Itu Uji-T (T-Test)?", "id": "Uji Hipotesis Statistik Berbasis Distribusi-T." },
  { "en": "Apa Itu Uji Chi-Kuadrat?", "id": "Uji Statistik Untuk Data Kategorikal." },
  { "en": "Apa Itu Analisis Varians (ANOVA)?", "id": "Membandingkan Rata-rata Tiga Atau Lebih Kelompok." },
  { "en": "Apa Itu Regresi Logistik?", "id": "Model Regresi Untuk Variabel Respon Biner." },
  { "en": "Apa Itu Entropi Informasi?", "id": "Ukuran Ketidakpastian Atau Kandungan Informasi." },
  { "en": "Apa Itu Pengkodean Huffman?", "id": "Algoritma Kompresi Data Lossless Optimal." },
  { "en": "Apa Itu Pengkodean Aritmetika?", "id": "Algoritma Kompresi Lossless Lainnya." },
  { "en": "Apa Itu Algoritma Lempel-Ziv (LZ)?", "id": "Keluarga Algoritma Kompresi Berbasis Kamus." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Kuantifikasi, Penyimpanan, Komunikasi Informasi." },
  { "en": "Apa Itu Kapasitas Kanal?", "id": "Laju Informasi Maksimum Dapat Ditransmisikan." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Komunikasi Aman." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengubah Plaintext Menjadi Ciphertext." },
  { "en": "Apa Itu Dekripsi?", "id": "Proses Mengubah Ciphertext Kembali Menjadi Plaintext." },
  { "en": "Apa Itu Kriptografi Kunci Simetris?", "id": "Menggunakan Kunci Sama Untuk Enkripsi, Dekripsi." },
  { "en": "Sebutkan Contoh Algoritma Kunci Simetris?", "id": "AES (Advanced Encryption Standard), DES (Data Encryption Standard)." },
  { "en": "Apa Itu Kriptografi Kunci Publik (Asimetris)?", "id": "Menggunakan Pasangan Kunci Publik Dan Privat." },
  { "en": "Apa Itu Kunci Publik?", "id": "Kunci Dapat Dibagikan Secara Bebas." },
  { "en": "Apa Itu Kunci Privat?", "id": "Kunci Yang Harus Dirahasiakan." },
  { "en": "Bagaimana Cara Kerja Enkripsi Kunci Publik?", "id": "Data Dienkripsi Kunci Publik, Didekripsi Privat." },
  { "en": "Sebutkan Contoh Algoritma Kunci Publik?", "id": "RSA (Rivest–Shamir–Adleman), Dan ECC (Elliptic Curve Cryptography)." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Mekanisme Kriptografis Untuk Otentikasi, Integritas." },
  { "en": "Bagaimana Cara Kerja Tanda Tangan Digital?", "id": "Hash Dienkripsi Kunci Privat, Diverifikasi Publik." },
  { "en": "Apa Itu Fungsi Hash Kriptografis?", "id": "Fungsi Satu Arah Menghasilkan Sidik Jari Data." },
  { "en": "Sebutkan Sifat Fungsi Hash?", "id": "Deterministik, Tahan Tumbukan, Efek Longsoran." },
  { "en": "Sebutkan Contoh Fungsi Hash?", "id": "SHA-256 (Secure Hash Algorithm 256-bit), MD5 (Message Digest 5)." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Mengikat Kunci Publik Dengan Identitas." },
  { "en": "Apa Itu Otoritas Sertifikat (CA)?", "id": "Entitas Terpercaya Yang Menerbitkan Sertifikat Digital." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Sistem Untuk Mengelola Sertifikat Digital." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Protokol Keamanan Komunikasi Jaringan." },
  { "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Penyerang Menyadap, Mengubah Komunikasi." },
  { "en": "Apa Itu Serangan Brute-Force?", "id": "Mencoba Semua Kemungkinan Kata Sandi." },
  { "en": "Apa Itu Serangan Kamus?", "id": "Serangan Brute-Force Menggunakan Daftar Kata Umum." },
  { "en": "Apa Itu Rekayasa Sosial (Social Engineering)?", "id": "Memanipulasi Orang Untuk Mendapatkan Informasi Rahasia." },
  { "en": "Apa Itu Phishing?", "id": "Upaya Mendapatkan Informasi Sensitif Menyamar." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Jahat (Virus, Worm, Trojan)." },
  { "en": "Apa Itu Virus Komputer?", "id": "Malware Mereplikasi Diri Menempel Program Lain." },
  { "en": "Apa Itu Worm Komputer?", "id": "Malware Mereplikasi Diri Mandiri Melalui Jaringan." },
  { "en": "Apa Itu Trojan Horse?", "id": "Malware Menyamar Sebagai Perangkat Lunak Sah." },
  { "en": "Apa Itu Ransomware?", "id": "Malware Mengenkripsi Data, Meminta Tebusan." },
  { "en": "Apa Itu Spyware?", "id": "Malware Mengumpulkan Informasi Tanpa Izin." },
  { "en": "Apa Itu Adware?", "id": "Perangkat Lunak Menampilkan Iklan Tak Diinginkan." },
  { "en": "Apa Itu Rootkit?", "id": "Malware Menyembunyikan Keberadaannya, Aktivitas Jahat." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Terinfeksi Dikendalikan Jarak Jauh." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membanjiri Sistem Hingga Tak Dapat Diakses." },
  { "en": "Apa Itu Serangan Distributed Denial-of-Service (DDoS)?", "id": "Serangan DoS Dari Banyak Sumber." },
  { "en": "Apa Itu SQL Injection?", "id": "Serangan Mengeksploitasi Celah Keamanan Database." },
  { "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Menyuntikkan Skrip Jahat Ke Situs Web." },
  { "en": "Apa Itu Cross-Site Request Forgery (CSRF)?", "id": "Memaksa Pengguna Melakukan Aksi Tak Diinginkan." },
  { "en": "Apa Itu Buffer Overflow?", "id": "Menulis Data Melebihi Batas Buffer." },
  { "en": "Apa Itu Heap Overflow?", "id": "Jenis Buffer Overflow Di Area Heap." },
  { "en": "Apa Itu Stack Overflow?", "id": "Jenis Buffer Overflow Di Area Stack." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Mendeteksi Aktivitas Jaringan Mencurigakan." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Sistem Mendeteksi Dan Memblokir Serangan." },
  { "en": "Apa Itu Honeypot?", "id": "Sistem Umpan Untuk Menarik Penyerang." },
  { "en": "Apa Itu Sandboxing?", "id": "Menjalankan Kode Di Lingkungan Terisolasi." },
  { "en": "Apa Itu Analisis Statis Kode?", "id": "Menganalisis Kode Sumber Tanpa Menjalankannya." },
  { "en": "Apa Itu Analisis Dinamis Kode?", "id": "Menganalisis Perilaku Kode Saat Dijalankan." },
  { "en": "Apa Itu Fuzzing?", "id": "Pengujian Perangkat Lunak Dengan Input Acak." },
  { "en": "Apa Itu Reverse Engineering?", "id": "Menganalisis Produk Untuk Memahami Cara Kerjanya." },
  { "en": "Apa Itu Disassembler?", "id": "Mengubah Kode Mesin Menjadi Bahasa Assembly." },
  { "en": "Apa Itu Decompiler?", "id": "Mengubah Kode Mesin Menjadi Kode Tingkat Tinggi." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Pesan Di Dalam Berkas Lain." },
  { "en": "Apa Itu Watermarking Digital?", "id": "Menanamkan Informasi Hak Cipta Dalam Media." },
  { "en": "Apa Itu Forensik Digital?", "id": "Ilmu Investigasi Kejahatan Digital." },
  { "en": "Apa Itu Chain of Custody?", "id": "Dokumentasi Penanganan Bukti Digital." },
  { "en": "Apa Itu Hash Collision?", "id": "Dua Input Berbeda Menghasilkan Hash Sama." },
  { "en": "Apa Itu Serangan Ulang Tahun (Birthday Attack)?", "id": "Mengeksploitasi Probabilitas Untuk Menemukan Tumbukan." },
  { "en": "Apa Itu Rainbow Table?", "id": "Tabel Hash Telah Dihitung Untuk Meretas." },
  { "en": "Apa Itu Salting Dalam Kata Sandi?", "id": "Menambahkan Data Acak Sebelum Melakukan Hash." },
  { "en": "Apa Tujuan Salting?", "id": "Melawan Serangan Menggunakan Rainbow Table." },
  { "en": "Apa Itu Key Stretching?", "id": "Membuat Proses Hashing Kata Sandi Lambat." },
  { "en": "Apa Itu Public Key Pinning?", "id": "Mekanisme Keamanan Mencegah Serangan MITM." },
  { "en": "Apa Itu Certificate Transparency?", "id": "Log Publik Untuk Memantau Penerbitan Sertifikat." },
  { "en": "Apa Itu Domain Name System Security Extensions (DNSSEC)?", "id": "Mengamankan DNS Dari Spoofing." },
  { "en": "Apa Itu Spoofing?", "id": "Memalsukan Data Untuk Menipu." },
  { "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Membutuhkan Dua Jenis Bukti Identitas." },
  { "en": "Apa Itu Biometrik?", "id": "Otentikasi Berdasarkan Karakteristik Fisik." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Serangan Mengeksploitasi Celah Keamanan Tak Dikenal." },
  { "en": "Apa Itu Patch Keamanan?", "id": "Pembaruan Perangkat Lunak Memperbaiki Celah Keamanan." },
  { "en": "Apa Itu Common Vulnerabilities and Exposures (CVE)?", "id": "Daftar Celah Keamanan Publik." },
  { "en": "Apa Itu Penetration Testing (Ethical Hacking)?", "id": "Mensimulasikan Serangan Menemukan Celah Keamanan." },
  { "en": "Apa Itu White Box Testing?", "id": "Pengujian Dengan Pengetahuan Penuh Sistem Internal." },
  { "en": "Apa Itu Black Box Testing?", "id": "Pengujian Tanpa Pengetahuan Sistem Internal." },
  { "en": "Apa Itu Grey Box Testing?", "id": "Pengujian Dengan Pengetahuan Parsial Sistem." },
  { "en": "Apa Itu Air Gap?", "id": "Mengisolasi Jaringan Secara Fisik Dari Internet." },
  { "en": "Apa Itu Demilitarized Zone (DMZ)?", "id": "Sub-jaringan Terisolasi Antara Jaringan Internal, Eksternal." },
  { "en": "Apa Itu Deep Packet Inspection (DPI)?", "id": "Memeriksa Isi Paket Data Secara Mendalam." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Aturan Izin Akses Jaringan." },
  { "en": "Apa Itu Network Segmentation?", "id": "Membagi Jaringan Menjadi Sub-jaringan Kecil." },
  { "en": "Apa Itu Microsegmentation?", "id": "Segmentasi Jaringan Pada Tingkat Workload Individual." },
  { "en": "Apa Itu Zero Trust Security?", "id": "Model Keamanan Tidak Pernah Mempercayai Apapun." },
  { "en": "Apa Itu Security Information and Event Management (SIEM)?", "id": "Manajemen Informasi Dan Peristiwa Keamanan." },
  { "en": "Apa Itu Teori Grafik?", "id": "Studi Grafik, Struktur Matematis." },
  { "en": "Apa Itu Simpul (Vertex/Node)?", "id": "Elemen Dasar Dalam Sebuah Grafik." },
  { "en": "Apa Itu Sisi (Edge)?", "id": "Koneksi Antara Dua Simpul." },
  { "en": "Apa Itu Grafik Berarah (Directed Graph)?", "id": "Grafik Dengan Sisi Memiliki Arah." },
  { "en": "Apa Itu Grafik Tak Berarah?", "id": "Grafik Dengan Sisi Tanpa Arah." },
  { "en": "Apa Itu Grafik Berbobot?", "id": "Grafik Sisi Memiliki Bobot Numerik." },
  { "en": "Apa Itu Derajat Simpul?", "id": "Jumlah Sisi Yang Terhubung Ke Simpul." },
  { "en": "Apa Itu Jalur (Path) Dalam Grafik?", "id": "Urutan Simpul Terhubung Oleh Sisi." },
  { "en": "Apa Itu Siklus (Cycle) Dalam Grafik?", "id": "Jalur Yang Dimulai, Berakhir Simpul Sama." },
  { "en": "Apa Itu Grafik Terhubung?", "id": "Terdapat Jalur Antara Setiap Pasang Simpul." },
  { "en": "Apa Itu Pohon (Tree)?", "id": "Grafik Terhubung Tanpa Siklus." },
  { "en": "Apa Itu Hutan (Forest)?", "id": "Kumpulan Pohon Yang Tidak Terhubung." },
  { "en": "Apa Itu Pohon Rentang (Spanning Tree)?", "id": "Subgraf Pohon Mencakup Semua Simpul." },
  { "en": "Apa Itu Pohon Rentang Minimum (MST)?", "id": "Pohon Rentang Total Bobot Sisi Minimum." },
  { "en": "Apa Itu Algoritma Prim?", "id": "Algoritma Serakah Menemukan Pohon Rentang Minimum." },
  { "en": "Apa Itu Algoritma Kruskal?", "id": "Algoritma Lain Menemukan Pohon Rentang Minimum." },
  { "en": "Apa Itu Algoritma Pencarian Jalur Terpendek?", "id": "Menemukan Jalur Total Bobot Minimum." },
  { "en": "Apa Itu Algoritma Dijkstra?", "id": "Menemukan Jalur Terpendek Dari Satu Sumber." },
  { "en": "Apa Itu Algoritma Bellman-Ford?", "id": "Menemukan Jalur Terpendek, Dapat Menangani Bobot Negatif." },
  { "en": "Apa Itu Algoritma Floyd-Warshall?", "id": "Menemukan Jalur Terpendek Antara Semua Pasang Simpul." },
  { "en": "Apa Itu Penelusuran Grafik (Graph Traversal)?", "id": "Proses Mengunjungi Semua Simpul Dalam Grafik." },
  { "en": "Apa Itu Breadth-First Search (BFS)?", "id": "Menelusuri Grafik Lapisan Demi Lapisan." },
  { "en": "Apa Itu Depth-First Search (DFS)?", "id": "Menelusuri Grafik Sejauh Mungkin Sebelum Mundur." },
  { "en": "Apa Itu Matriks Adjacency?", "id": "Representasi Grafik Menggunakan Matriks." },
  { "en": "Apa Itu Daftar Adjacency?", "id": "Representasi Grafik Menggunakan Daftar Simpul Tetangga." },
  { "en": "Apa Itu Aliran Maksimum (Maximum Flow)?", "id": "Jumlah Aliran Maksimum Dari Sumber Ke Tujuan." },
  { "en": "Apa Itu Teorema Max-Flow Min-Cut?", "id": "Aliran Maksimum Sama Dengan Kapasitas Potongan Minimum." },
  { "en": "Apa Itu Algoritma Ford-Fulkerson?", "id": "Metode Komputasi Untuk Masalah Aliran Maksimum." },
  { "en": "Apa Itu Pewarnaan Grafik?", "id": "Memberi Warna Simpul, Tetangga Tak Berwarna Sama." },
  { "en": "Apa Itu Bilangan Kromatik?", "id": "Jumlah Warna Minimum Untuk Pewarnaan Grafik." },
  { "en": "Apa Itu Grafik Bipartit?", "id": "Simpul Dapat Dibagi Dua Set Disjoin." },
  { "en": "Apa Itu Pencocokan (Matching) Dalam Grafik?", "id": "Set Sisi Tanpa Simpul Bersama." },
  { "en": "Apa Itu Pencocokan Maksimum?", "id": "Pencocokan Dengan Jumlah Sisi Paling Banyak." },
  { "en": "Apa Itu Siklus Hamiltonian?", "id": "Siklus Yang Mengunjungi Setiap Simpul Tepat Sekali." },
  { "en": "Apa Itu Jalur Hamiltonian?", "id": "Jalur Yang Mengunjungi Setiap Simpul Tepat Sekali." },
  { "en": "Apa Itu Masalah Pedagang Keliling (TSP)?", "id": "Menemukan Siklus Hamiltonian Dengan Bobot Minimum." },
  { "en": "Apa Itu Masalah NP-Complete?", "id": "Kelas Masalah Komputasi Sulit." },
  { "en": "Apa Itu Siklus Eulerian?", "id": "Siklus Yang Melintasi Setiap Sisi Tepat Sekali." },
  { "en": "Apa Itu Jalur Eulerian?", "id": "Jalur Yang Melintasi Setiap Sisi Tepat Sekali." },
  { "en": "Apa Itu Tujuh Jembatan Königsberg?", "id": "Masalah Sejarah Yang Melahirkan Teori Grafik." },
  { "en": "Apa Itu Isomorfisme Grafik?", "id": "Dua Grafik Sama Secara Struktural." },
  { "en": "Apa Itu Grafik Planar?", "id": "Grafik Dapat Digambar Tanpa Sisi Bersilangan." },
  { "en": "Apa Itu Rumus Euler Untuk Grafik Planar?", "id": "V - E + F = 2." },
  { "en": "Apa Itu Dualitas Grafik?", "id": "Konsep Grafik Dual Dari Grafik Planar." },
  { "en": "Apa Itu Klik (Clique) Dalam Grafik?", "id": "Subgraf Lengkap Di Mana Semua Simpul Terhubung." },
  { "en": "Apa Itu Himpunan Independen (Independent Set)?", "id": "Set Simpul Di Mana Tidak Ada Dua Terhubung." },
  { "en": "Apa Itu Penutup Simpul (Vertex Cover)?", "id": "Set Simpul Mencakup Semua Sisi Grafik." },
  { "en": "Apa Itu Teorema Turan?", "id": "Memberi Batas Jumlah Sisi Grafik." },
  { "en": "Apa Itu Teorema Ramsey?", "id": "Menjamin Keteraturan Dalam Grafik Cukup Besar." },
  { "en": "Apa Itu Grafik Acak?", "id": "Grafik Dihasilkan Oleh Proses Stokastik." },
  { "en": "Apa Itu Jaringan Dunia Kecil (Small-World)?", "id": "Grafik Jarak Rata-rata Simpul Pendek." },
  { "en": "Apa Itu Jaringan Bebas Skala (Scale-Free)?", "id": "Distribusi Derajat Mengikuti Hukum Pangkat." },
  { "en": "Apa Itu Centrality (Sentralitas)?", "id": "Ukuran Kepentingan Simpul Dalam Jaringan." },
  { "en": "Apa Itu Degree Centrality?", "id": "Sentralitas Berdasarkan Jumlah Koneksi." },
  { "en": "Apa Itu Betweenness Centrality?", "id": "Sentralitas Berdasarkan Frekuensi Simpul Di Jalur." },
  { "en": "Apa Itu Closeness Centrality?", "id": "Sentralitas Berdasarkan Jarak Rata-rata." },
  { "en": "Apa Itu PageRank?", "id": "Algoritma Sentralitas Digunakan Oleh Google." },
  { "en": "Apa Itu Komunitas Dalam Jaringan?", "id": "Kelompok Simpul Yang Terhubung Erat." },
  { "en": "Apa Itu Algoritma Deteksi Komunitas?", "id": "Menemukan Struktur Komunitas Dalam Grafik." },
  { "en": "Apa Itu Modularity?", "id": "Ukuran Kualitas Partisi Komunitas." },
  { "en": "Apa Itu Analisis Jaringan Sosial (SNA)?", "id": "Studi Struktur Sosial Menggunakan Teori Grafik." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Berurusan Dengan Kebenaran Parsial." },
  { "en": "Apa Itu Himpunan Fuzzy?", "id": "Himpunan Dengan Derajat Keanggotaan." },
  { "en": "Apa Itu Fungsi Keanggotaan?", "id": "Mendefinisikan Seberapa Besar Elemen Anggota Himpunan." },
  { "en": "Apa Itu Fuzzifikasi?", "id": "Proses Mengubah Input Crisp Menjadi Fuzzy." },
  { "en": "Apa Itu Mesin Inferensi Fuzzy?", "id": "Menerapkan Aturan Logika Fuzzy." },
  { "en": "Apa Itu Defuzzifikasi?", "id": "Proses Mengubah Output Fuzzy Menjadi Crisp." },
  { "en": "Apa Itu Metode Defuzzifikasi Centroid?", "id": "Menemukan Pusat Massa Himpunan Output." },
  { "en": "Apa Itu Kontroler Logika Fuzzy (FLC)?", "id": "Sistem Kontrol Berbasis Logika Fuzzy." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
  { "en": "Apa Itu Neuron (Simpul) Dalam ANN?", "id": "Unit Komputasi Dasar Dalam Jaringan." },
  { "en": "Apa Itu Bobot (Weight) Dalam ANN?", "id": "Parameter Kekuatan Koneksi Antar Neuron." },
  { "en": "Apa Itu Bias Dalam ANN?", "id": "Parameter Tambahan Menggeser Fungsi Aktivasi." },
  { "en": "Apa Itu Fungsi Aktivasi?", "id": "Memperkenalkan Non-Linearitas Ke Jaringan Saraf." },
  { "en": "Apa Itu Lapisan Input (Input Layer)?", "id": "Lapisan Pertama Menerima Data Input." },
  { "en": "Apa Itu Lapisan Tersembunyi (Hidden Layer)?", "id": "Lapisan Antara Input Dan Output." },
  { "en": "Apa Itu Lapisan Output (Output Layer)?", "id": "Lapisan Terakhir Menghasilkan Hasil." },
  { "en": "Apa Itu Jaringan Feedforward?", "id": "Informasi Bergerak Satu Arah, Maju." },
  { "en": "Apa Itu Perceptron?", "id": "Bentuk Paling Sederhana Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Jaringan Multilayer Perceptron (MLP)?", "id": "Jaringan Feedforward Dengan Lapisan Tersembunyi." },
  { "en": "Apa Itu Algoritma Backpropagation?", "id": "Metode Melatih Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Vanishing Gradient Problem?", "id": "Gradien Menjadi Sangat Kecil Saat Pelatihan." },
  { "en": "Apa Itu Exploding Gradient Problem?", "id": "Gradien Menjadi Sangat Besar Saat Pelatihan." },
  { "en": "Apa Itu Fungsi Aktivasi ReLU?", "id": "Rectified Linear Unit, Populer Dalam Deep Learning." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Jaringan Saraf Dengan Koneksi Umpan Balik." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN Mengatasi Vanishing Gradient." },
  { "en": "Apa Itu Gated Recurrent Unit (GRU)?", "id": "Varian LSTM Yang Lebih Sederhana." },
  { "en": "Apa Itu Peta Self-Organizing (SOM)?", "id": "Jenis Jaringan Saraf Tak Terawasi." },
  { "en": "Apa Itu Autoencoder?", "id": "Jaringan Saraf Untuk Pengurangan Dimensi." },
  { "en": "Apa Itu Generative Adversarial Network (GAN)?", "id": "Jaringan Saraf Untuk Menghasilkan Data Baru." },
  { "en": "Apa Itu Transfer Learning?", "id": "Menggunakan Model Telah Dilatih Untuk Tugas Baru." },
  { "en": "Apa Itu Fine-Tuning?", "id": "Menyesuaikan Bobot Model Telah Dilatih." },
  { "en": "Apa Itu Algoritma Genetika?", "id": "Algoritma Optimisasi Terinspirasi Evolusi." },
  { "en": "Apa Itu Kromosom Dalam Algoritma Genetika?", "id": "Representasi Calon Solusi." },
  { "en": "Apa Itu Fungsi Kebugaran (Fitness Function)?", "id": "Mengevaluasi Seberapa Baik Suatu Solusi." },
  { "en": "Apa Itu Seleksi Dalam Algoritma Genetika?", "id": "Memilih Individu Untuk Bereproduksi." },
  { "en": "Apa Itu Crossover (Pindah Silang)?", "id": "Menggabungkan Dua Induk Menghasilkan Keturunan." },
  { "en": "Apa Itu Mutasi Dalam Algoritma Genetika?", "id": "Perubahan Acak Dalam Kromosom." },
  { "en": "Apa Itu Komputasi Evolusioner?", "id": "Bidang Lebih Luas Mencakup Algoritma Genetika." },
  { "en": "Apa Itu Swarm Intelligence?", "id": "Perilaku Kolektif Sistem Terdesentralisasi." },
  { "en": "Apa Itu Particle Swarm Optimization (PSO)?", "id": "Algoritma Optimisasi Terinspirasi Perilaku Kawanan." },
  { "en": "Apa Itu Ant Colony Optimization (ACO)?", "id": "Algoritma Terinspirasi Perilaku Koloni Semut." },
  { "en": "Apa Itu Simulated Annealing?", "id": "Algoritma Optimisasi Probabilistik." },
  { "en": "Apa Itu Optimisasi?", "id": "Menemukan Solusi Terbaik Dari Alternatif." },
  { "en": "Apa Itu Fungsi Objektif?", "id": "Fungsi Yang Ingin Dimaksimalkan Atau Diminimalkan." },
  { "en": "Apa Itu Kendala (Constraint)?", "id": "Batasan Yang Harus Dipenuhi Solusi." },
  { "en": "Apa Itu Pemrograman Linear?", "id": "Optimisasi Dengan Fungsi Objektif, Kendala Linear." },
  { "en": "Apa Itu Metode Simplex?", "id": "Algoritma Klasik Untuk Pemrograman Linear." },
  { "en": "Apa Itu Pemrograman Non-linear?", "id": "Optimisasi Dengan Fungsi Objektif, Kendala Non-linear." },
  { "en": "Apa Itu Pemrograman Kuadratik?", "id": "Fungsi Objektif Kuadratik, Kendala Linear." },
  { "en": "Apa Itu Optimisasi Konveks?", "id": "Jenis Optimisasi Lebih Mudah Diselesaikan." },
  { "en": "Apa Itu Himpunan Konveks?", "id": "Himpunan Di Mana Garis Antara Dua Titik." },
  { "en": "Apa Itu Fungsi Konveks?", "id": "Fungsi Di Mana Segmen Garis." },
  { "en": "Apa Itu Optimisasi Kombinatorial?", "id": "Menemukan Objek Optimal Dari Himpunan Terbatas." },
  { "en": "Apa Itu Masalah Knapsack?", "id": "Contoh Klasik Optimisasi Kombinatorial." },
  { "en": "Apa Itu Pemrograman Dinamis?", "id": "Teknik Pemecahan Masalah Kompleks." },
  { "en": "Apa Itu Prinsip Optimalitas Bellman?", "id": "Prinsip Dasar Pemrograman Dinamis." },
  { "en": "Apa Itu Memoization?", "id": "Menyimpan Hasil Sub-masalah." },
  { "en": "Apa Itu Pencarian Heuristik?", "id": "Strategi Menemukan Solusi Cukup Baik." },
  { "en": "Apa Itu Algoritma Serakah (Greedy)?", "id": "Membuat Pilihan Optimal Lokal." },
  { "en": "Apa Itu Pemrograman Kendala (Constraint Programming)?", "id": "Paradigma Pemrograman Berbasis Kendala." },
  { "en": "Apa Itu Robotika Probabilistik?", "id": "Robotika Yang Memperhitungkan Ketidakpastian." },
  { "en": "Apa Itu Simultaneous Localization and Mapping (SLAM)?", "id": "Robot Membangun Peta, Melokalisasi Diri." },
  { "en": "Apa Itu Perencanaan Gerak (Motion Planning)?", "id": "Menemukan Jalur Bebas Hambatan Untuk Robot." },
  { "en": "Apa Itu Ruang Konfigurasi (Configuration Space)?", "id": "Ruang Semua Konfigurasi Robot." },
  { "en": "Apa Itu Hambatan Ruang Konfigurasi?", "id": "Daerah C-space Dilarang Untuk Robot." },
  { "en": "Apa Itu Probabilistic Roadmaps (PRM)?", "id": "Algoritma Perencanaan Gerak Berbasis Sampel." },
  { "en": "Apa Itu Rapidly-exploring Random Trees (RRT)?", "id": "Algoritma Perencanaan Gerak Lainnya." },
  { "en": "Apa Itu Dinamika Robot?", "id": "Studi Gerakan Robot Akibat Gaya." },
  { "en": "Apa Itu Persamaan Euler-Lagrange?", "id": "Metode Menurunkan Persamaan Gerak." },
  { "en": "Apa Itu Kontrol Torsi Robot?", "id": "Mengontrol Arus Motor Untuk Menghasilkan Torsi." },
  { "en": "Apa Itu Kontrol Posisi Robot?", "id": "Mengontrol Posisi Akhir Lengan Robot." },
  { "en": "Apa Itu Kontrol Kepatuhan (Compliance Control)?", "id": "Mengontrol Interaksi Robot Dengan Lingkungan." },
  { "en": "Apa Itu Kontrol Impedansi?", "id": "Jenis Kontrol Kepatuhan." },
  { "en": "Apa Itu Haptics?", "id": "Teknologi Umpan Balik Sentuhan." },
  { "en": "Apa Itu Teleoperasi?", "id": "Mengendalikan Robot Dari Jarak Jauh." },
  { "en": "Apa Itu Telerobotika?", "id": "Bidang Studi Teleoperasi Robot." },
  { "en": "Apa Itu Realitas Virtual (VR)?", "id": "Simulasi Lingkungan Tiga Dimensi." },
  { "en": "Apa Itu Realitas Tertambah (AR)?", "id": "Melapisi Informasi Digital Ke Dunia Nyata." },
  { "en": "Apa Itu Mixed Reality (MR)?", "id": "Gabungan Dunia Nyata Dan Virtual." },
  { "en": "Apa Itu Agent (Dalam AI)?", "id": "Entitas Otonom Bertindak Dalam Lingkungan." },
  { "en": "Apa Itu Sistem Multi-Agent?", "id": "Kumpulan Agent Berinteraksi." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Simulasi Kecerdasan Manusia Dalam Mesin." },
  { "en": "Apa Itu Strong AI?", "id": "AI Dengan Kesadaran Mirip Manusia." },
  { "en": "Apa Itu Weak AI (Narrow AI)?", "id": "AI Dirancang Untuk Tugas Tertentu." },
  { "en": "Apa Itu Uji Turing?", "id": "Uji Kemampuan Mesin Menunjukkan Kecerdasan." },
  { "en": "Apa Itu Sistem Pakar (Expert System)?", "id": "Program Komputer Meniru Kemampuan Ahli." },
  { "en": "Apa Itu Basis Pengetahuan?", "id": "Bagian Sistem Pakar Penyimpan Fakta, Aturan." },
  { "en": "Apa Itu Mesin Inferensi?", "id": "Menerapkan Aturan Ke Basis Pengetahuan." },
  { "en": "Apa Itu Forward Chaining?", "id": "Penalaran Dari Fakta Ke Kesimpulan." },
  { "en": "Apa Itu Backward Chaining?", "id": "Penalaran Dari Tujuan Ke Fakta." },
  { "en": "Apa Itu Ontologi Dalam Ilmu Komputer?", "id": "Representasi Formal Pengetahuan." },
  { "en": "Apa Itu Semantic Web?", "id": "Visi Web Dengan Makna Terbaca Mesin." },
  { "en": "Apa Itu Linked Data?", "id": "Prinsip Menerbitkan Data Terstruktur Di Web." },
  { "en": "Apa Itu Resource Description Framework (RDF)?", "id": "Model Data Untuk Pernyataan Web Semantik." },
  { "en": "Apa Itu SPARQL?", "id": "Bahasa Kueri Untuk Data RDF." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar, Kompleks." },
  { "en": "Apa Itu Tiga V Big Data?", "id": "Volume, Velocity, Dan Variety." },
  { "en": "Apa Itu Hadoop?", "id": "Kerangka Kerja Perangkat Lunak Terbuka." },
  { "en": "Apa Itu MapReduce?", "id": "Model Pemrograman Pemrosesan Big Data." },
  { "en": "Apa Itu Spark?", "id": "Mesin Analitik Terpadu Cepat." },
  { "en": "Apa Itu Data Mining?", "id": "Menemukan Pola Dalam Kumpulan Data Besar." },
  { "en": "Apa Itu Business Intelligence (BI)?", "id": "Mengubah Data Menjadi Wawasan Bisnis." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Penyimpanan Data Terpusat Untuk Analisis." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Memuat Data Ke Gudang." }



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
