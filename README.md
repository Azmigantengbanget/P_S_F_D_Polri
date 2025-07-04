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


  { "en": "Definisi Forensik Digital?", "id": "Ilmu investigasi barang bukti digital." },
  { "en": "Tujuan utama Forensik Digital?", "id": "Mengungkap fakta dari bukti digital." },
  { "en": "Empat tahap forensik digital?", "id": "Collection, Examination, Analysis, Reporting." },
  { "en": "Apa itu 'Chain of Custody'?", "id": "Dokumentasi riwayat penanganan barang bukti." },
  { "en": "Pentingnya 'Chain of Custody'?", "id": "Menjaga keaslian dan legalitas bukti." },
  { "en": "Apa itu 'hashing' forensik?", "id": "Membuat sidik jari digital unik." },
  { "en": "Fungsi 'hashing' dalam forensik?", "id": "Memastikan integritas dan keaslian data." },
  { "en": "Algoritma hash yang umum?", "id": "SHA-256, MD5." },
  { "en": "Apa itu 'imaging' forensik?", "id": "Membuat salinan bit-per-bit identik." },
  { "en": "Kenapa menggunakan 'imaging'?", "id": "Agar bukti asli tidak berubah." },
  { "en": "Perangkat pencegah penulisan data?", "id": "Write Blocker." },
  { "en": "Bukti yang mudah hilang?", "id": "Data Volatile." },
  { "en": "Contoh data 'volatile'?", "id": "Data di RAM, register CPU." },
  { "en": "Bukti yang tersimpan permanen?", "id": "Data Non-Volatile." },
  { "en": "Contoh data 'non-volatile'?", "id": "Data di Hard Disk Drive (HDD)." },
  { "en": "Urutan pengumpulan bukti?", "id": "Dari paling volatile ke non-volatile." },
  { "en": "Jejak digital yang ditinggalkan?", "id": "Artefak digital (Digital Artifact)." },
  { "en": "Definisi sinyal?", "id": "Besaran fisik pembawa informasi." },
  { "en": "Beda sinyal analog & digital?", "id": "Analog kontinu, digital diskrit." },
  { "en": "Proses ubah sinyal analog-digital?", "id": "Sampling dan Quantization." },
  { "en": "Proses sampling?", "id": "Mengambil sampel sinyal secara periodik." },
  { "en": "Proses quantization?", "id": "Memberi nilai digital pada sampel." },
  { "en": "Alat konversi analog-digital (ADC)?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Alat konversi digital-analog (DAC)?", "id": "DAC (Digital-to-Analog Converter)." },
  { "en": "Apa itu 'noise' pada sinyal?", "id": "Gangguan acak yang tidak diinginkan." },
  { "en": "Proses menghilangkan 'noise'?", "id": "Filtering (Penyaringan)." },
  { "en": "Contoh filter dasar?", "id": "Low-pass filter, high-pass filter." },
  { "en": "Penerapan filter di forensik?", "id": "Membersihkan rekaman audio atau video." },
  { "en": "Apa itu 'frequency' sinyal?", "id": "Jumlah getaran per detik (Hertz)." },
  { "en": "Apa itu 'amplitude' sinyal?", "id": "Kekuatan atau intensitas sebuah sinyal." },
  { "en": "Apa itu 'bandwidth' sinyal?", "id": "Rentang frekuensi yang ditempati sinyal." },
  { "en": "Dasar hukum bukti elektronik?", "id": "UU ITE (Informasi dan Transaksi Elektronik)." },
  { "en": "Apa itu forensik audio?", "id": "Analisa bukti suara untuk investigasi." },
  { "en": "Tujuan forensik audio?", "id": "Otentikasi, perbaikan, identifikasi suara." },
  { "en": "Apa itu forensik video?", "id": "Analisa bukti video untuk investigasi." },
  { "en": "Apa itu 'frame' video?", "id": "Satu gambar tunggal dalam video." },
  { "en": "Apa itu 'frame rate' (fps)?", "id": "Jumlah frame per detik." },
  { "en": "Apa itu kompresi data?", "id": "Proses memperkecil ukuran file." },
  { "en": "Kompresi 'lossy' vs 'lossless'?", "id": "Lossy buang data, lossless tidak." },
  { "en": "Risiko kompresi 'lossy' forensik?", "id": "Potensi kehilangan detail bukti penting." },
  { "en": "Apa itu 'metadata'?", "id": "Informasi tentang suatu data." },
  { "en": "Contoh metadata foto?", "id": "Waktu, tanggal, model kamera, GPS." },
  { "en": "Apa itu 'timestamp'?", "id": "Cap waktu digital suatu kejadian." },
  { "en": "Pentingnya 'timestamp' forensik?", "id": "Membangun kronologi kejadian yang akurat." },
  { "en": "Apa itu 'file system'?", "id": "Struktur penyimpanan file di media." },
  { "en": "Contoh 'file system'?", "id": "NTFS, FAT32, ext4." },
  { "en": "Apa itu 'unallocated space'?", "id": "Area disk tidak terpakai sistem." },
  { "en": "Pentingnya 'unallocated space'?", "id": "Bisa berisi sisa file terhapus." },
  { "en": "Apa itu 'file carving'?", "id": "Memulihkan file dari data mentah." },
  { "en": "Peralatan utama investigator digital?", "id": "Forensic Workstation." },
  { "en": "Sistem operasi untuk forensik?", "id": "Windows, Linux (distro khusus)." },
  { "en": "Contoh distro Linux forensik?", "id": "Kali Linux, SANS SIFT, Paladin." },
  { "en": "Apa itu forensik jaringan?", "id": "Analisa lalu lintas jaringan." },
  { "en": "Tujuan forensik jaringan?", "id": "Lacak penyusupan, identifikasi sumber serangan." },
  { "en": "Rekaman paket data jaringan (PCAP)?", "id": "File PCAP (Packet Capture)." },
  { "en": "Software analisa file PCAP (Packet Capture)?", "id": "Wireshark." },
  { "en": "Apa itu forensik seluler?", "id": "Forensik pada perangkat mobile." },
  { "en": "Bukti dari forensik seluler?", "id": "Panggilan, SMS, chat, lokasi GPS." },
  { "en": "Apa itu 'logical acquisition'?", "id": "Menyalin file yang terlihat pengguna." },
  { "en": "Apa itu 'physical acquisition'?", "id": "Menyalin seluruh isi memori fisik." },
  { "en": "Mana akuisisi lebih lengkap?", "id": "Physical acquisition." },
  { "en": "Apa itu 'SIM card cloning'?", "id": "Membuat duplikat kartu SIM." },
  { "en": "Apa itu 'Faraday bag'?", "id": "Kantung pemblokir sinyal nirkabel." },
  { "en": "Fungsi 'Faraday bag'?", "id": "Mencegah penghapusan data jarak jauh." },
  { "en": "Apa itu 'domain' sinyal?", "id": "Cara merepresentasikan sinyal (waktu/frekuensi)." },
  { "en": "Apa itu 'time domain'?", "id": "Representasi sinyal terhadap waktu." },
  { "en": "Apa itu 'frequency domain'?", "id": "Representasi sinyal terhadap frekuensi." },
  { "en": "Alat ubah domain waktu-frekuensi?", "id": "Transformasi Fourier." },
  { "en": "Kepanjangan FFT (Fast Fourier Transform)?", "id": "Fast Fourier Transform." },
  { "en": "Penerapan FFT (Fast Fourier Transform) dalam forensik?", "id": "Analisa frekuensi suara atau sinyal." },
  { "en": "Apa itu 'spectrogram'?", "id": "Visualisasi frekuensi sinyal terhadap waktu." },
  { "en": "Kegunaan 'spectrogram' audio?", "id": "Identifikasi suara, deteksi editan." },
  { "en": "Apa itu 'speaker recognition'?", "id": "Identifikasi orang dari karakteristik suaranya." },
  { "en": "Apa itu 'voiceprint'?", "id": "Ciri khas unik dari suara seseorang." },
  { "en": "Apa itu 'enhancement' audio?", "id": "Proses menjernihkan rekaman suara." },
  { "en": "Apa itu 'authentication' audio?", "id": "Memastikan rekaman suara tidak dimanipulasi." },
  { "en": "Apa itu 'Electric Network Frequency' (ENF)?", "id": "Analisa frekuensi listrik pada rekaman." },
  { "en": "Fungsi analisa ENF (Electric Network Frequency)?", "id": "Memverifikasi waktu dan keaslian rekaman." },
  { "en": "Apa itu 'image forensics'?", "id": "Forensik pada bukti gambar digital." },
  { "en": "Apa itu 'Error Level Analysis' (ELA)?", "id": "Mendeteksi area manipulasi pada gambar." },
  { "en": "Cara kerja ELA (Error Level Analysis)?", "id": "Analisa perbedaan tingkat kompresi JPEG." },
  { "en": "Apa itu 'metadata analysis'?", "id": "Menganalisa data EXIF sebuah gambar." },
  { "en": "Apa itu 'cloning' gambar?", "id": "Menyalin bagian gambar ke bagian lain." },
  { "en": "Teknik deteksi 'cloning'?", "id": "Analisa pola noise, pencocokan blok." },
  { "en": "Apa itu 'splicing' gambar?", "id": "Menggabungkan dua gambar berbeda." },
  { "en": "Apa itu 'anti-forensics'?", "id": "Teknik untuk menghalangi investigasi digital." },
  { "en": "Contoh 'anti-forensics'?", "id": "Enkripsi, data wiping, steganografi." },
  { "en": "Apa itu 'steganography'?", "id": "Menyembunyikan pesan dalam file lain." },
  { "en": "Alat untuk 'steganography'?", "id": "Steghide, OpenStego." },
  { "en": "Apa itu 'data wiping'?", "id": "Menghapus data secara permanen." },
  { "en": "Beda 'delete' dan 'wipe'?", "id": "Delete sembunyikan, wipe timpa data." },
  { "en": "Apa itu 'live forensics'?", "id": "Analisa pada sistem yang menyala." },
  { "en": "Apa itu 'post-mortem forensics'?", "id": "Analisa pada sistem yang mati." },
  { "en": "Tahap pertama di TKP (Tempat Kejadian Perkara) digital?", "id": "Amankan lokasi dan identifikasi bukti." },
  { "en": "Apa itu 'digital evidence bag'?", "id": "Kantong khusus untuk barang bukti." },
  { "en": "Pentingnya dokumentasi TKP (Tempat Kejadian Perkara)?", "id": "Mencatat kondisi awal bukti." },
  { "en": "Dasar hukum bukti video?", "id": "UU ITE dan KUHAP." },
  { "en": "Aturan pemusnahan bukti digital?", "id": "Sesuai standar manajemen barang bukti." },
  { "en": "Syarat bukti digital di pengadilan?", "id": "Asli, otentik, dan tidak dimanipulasi." },
  { "en": "Tantangan etis 'face recognition'?", "id": "Potensi bias dan pengawasan massal." },
  { "en": "Hak subjek data yang terekam?", "id": "Hak atas privasi sesuai aturan." },
  { "en": "Rumus dasar kebutuhan storage?", "id": "Bitrate x waktu x jumlah kamera." },
  { "en": "Rumus dasar kebutuhan bandwidth?", "id": "Bitrate per kamera x jumlah kamera." },
  { "en": "Jaringan ideal untuk lab forensik?", "id": "Jaringan terisolasi (air-gapped)." },
  { "en": "Tujuan jaringan terisolasi?", "id": "Mencegah kontaminasi bukti digital." },
  { "en": "Cara amankan workstation forensik?", "id": "Hanya instal software forensik, batasi akses." },
  { "en": "Kenapa enkripsi bukti penting?", "id": "Melindungi data saat transit dan disimpan." },
  { "en": "Tujuan manajemen pengguna VMS (Video Management System)?", "id": "Memberi hak akses sesuai peran." },
  { "en": "Contoh peran pengguna VMS (Video Management System)?", "id": "Admin, operator, investigator." },
  { "en": "SOP (Standard Operating Procedure) permintaan bukti digital?", "id": "Melalui surat permintaan resmi." },
  { "en": "Kepanjangan BWC (Body Worn Camera)?", "id": "Body Worn Camera." },
  { "en": "Fungsi utama BWC (Body Worn Camera)?", "id": "Merekam interaksi petugas dengan publik." },
  { "en": "Tujuan BWC (Body Worn Camera) bagi Polri?", "id": "Transparansi, akuntabilitas, bukti hukum." },
  { "en": "Prosedur aktivasi BWC (Body Worn Camera)?", "id": "Aktifkan sebelum interaksi dengan publik." },
  { "en": "Penyimpanan data BWC (Body Worn Camera)?", "id": "Diunggah ke server terpusat aman." },
  { "en": "Prosedur pelabelan bukti video?", "id": "Catat waktu, tanggal, lokasi, hash." },
  { "en": "Fungsi penyegelan barang bukti?", "id": "Menjamin bukti tidak diakses sembarangan." },
  { "en": "Penggunaan sinyal dalam pengintaian?", "id": "Penyadapan sinyal komunikasi (SIGINT)." },
  { "en": "Peran analisa sinyal di rekonstruksi?", "id": "Memvalidasi kronologi dari bukti audio/video." },
  { "en": "Fungsi 'color correction'?", "id": "Menyesuaikan keseimbangan warna video." },
  { "en": "Fungsi 'deblurring'?", "id": "Mengurangi efek blur pada gambar." },
  { "en": "Apa itu 'Gaussian noise'?", "id": "Noise acak dengan distribusi normal." },
  { "en": "Apa itu 'salt-and-pepper noise'?", "id": "Noise berupa bintik hitam putih." },
  { "en": "Kepanjangan SNR (Signal-to-Noise Ratio)?", "id": "Signal-to-Noise Ratio." },
  { "en": "Arti SNR (Signal-to-Noise Ratio) tinggi?", "id": "Sinyal kuat dibandingkan dengan noise." },
  { "en": "Apa itu '3D reconstruction' forensik?", "id": "Membangun model 3D TKP dari gambar." },
  { "en": "Syarat '3D reconstruction'?", "id": "Butuh rekaman dari beberapa sudut." },
  { "en": "Apa itu 'data integrity'?", "id": "Keaslian dan keutuhan data." },
  { "en": "Cara jaga 'data integrity' file?", "id": "Gunakan hashing (SHA-256)." },
  { "en": "Apa itu 'file signature analysis'?", "id": "Memverifikasi tipe file dari headernya." },
  { "en": "Apa itu 'codec analysis'?", "id": "Menganalisa jenis kompresi yang digunakan." },
  { "en": "Apa itu 'forensic video authentication'?", "id": "Proses pembuktian keaslian video." },
  { "en": "Apa itu 'video redaction'?", "id": "Menyunting bagian sensitif dari video." },
  { "en": "Contoh 'redaction'?", "id": "Memblur wajah saksi atau korban." },
  { "en": "Apa itu 'biometric data'?", "id": "Data dari ciri fisik (wajah, suara)." },
  { "en": "Isu privasi 'biometric data'?", "id": "Risiko penyalahgunaan data identitas." },
  { "en": "Pentingnya 'audit trail' software forensik?", "id": "Mencatat semua langkah analisa." },
  { "en": "Apa itu 'video loss'?", "id": "Sinyal video dari kamera terputus." },
  { "en": "Penyebab 'video loss'?", "id": "Kabel putus, kamera mati, konektor." },
  { "en": "Apa itu 'network jitter'?", "id": "Variasi waktu tunda pengiriman paket." },
  { "en": "Dampak 'jitter' pada video?", "id": "Gambar patah-patah atau terdistorsi." },
  { "en": "Apa itu 'packet loss'?", "id": "Hilangnya paket data saat transmisi." },
  { "en": "Dampak 'packet loss'?", "id": "Gambar rusak atau hilang sebagian." },
  { "en": "Apa itu 'centralized evidence storage'?", "id": "Penyimpanan bukti digital terpusat." },
  { "en": "Apa itu 'edge storage' forensik?", "id": "Penyimpanan di memori perangkat bukti." },
  { "en": "Fungsi 'edge storage' bukti?", "id": "Data asli sebelum diakuisisi." },
  { "en": "Apa itu 'failover' sistem forensik?", "id": "Sistem cadangan mengambil alih otomatis." },
  { "en": "Apa itu 'load balancing' forensik?", "id": "Membagi beban kerja analisa." },
  { "en": "Apa itu 'image flicker'?", "id": "Gambar berkedip akibat frekuensi listrik." },
  { "en": "Apa itu 'anti-flicker' mode?", "id": "Mengurangi efek kedipan pada video." },
  { "en": "Apa itu 'video wall'?", "id": "Gabungan banyak monitor jadi satu." },
  { "en": "Fungsi 'video wall' forensik?", "id": "Tampilan analisa terpusat saat investigasi." },
  { "en": "Apa itu 'digital watermark' forensik?", "id": "Menandai bukti digital yang diolah." },
  { "en": "Apa itu 'case management' forensik?", "id": "Fitur mengelola bukti per kasus." },
  { "en": "Apa itu 'exporting' bukti?", "id": "Menyimpan klip bukti sebagai file." },
  { "en": "Format video untuk pengadilan?", "id": "Format asli atau format standar." },
  { "en": "Apa itu 'native format'?", "id": "Format asli dari perangkat perekam." },
  { "en": "Pentingnya 'native format'?", "id": "Menjaga metadata dan kualitas asli." },
  { "en": "Apa itu 'forensic video player'?", "id": "Player khusus untuk analisa video." },
  { "en": "Tugas operator 'live monitoring'?", "id": "Mengamati, merespon, melaporkan kejadian." },
  { "en": "Apa itu 'situational awareness'?", "id": "Pemahaman kondisi lingkungan sekitar." },
  { "en": "Peran sinyal tingkatkan 'awareness'?", "id": "Memberi 'mata dan telinga' tambahan." },
  { "en": "Apa itu 'common operational picture'?", "id": "Tampilan situasi terpadu untuk semua." },
  { "en": "Apa itu 'image sensor size'?", "id": "Ukuran fisik dari sensor gambar." },
  { "en": "Pengaruh 'sensor size'?", "id": "Sensor besar, performa low-light baik." },
  { "en": "Apa itu 'F-number' atau 'F-stop'?", "id": "Ukuran bukaan diafragma lensa." },
  { "en": "Pengaruh 'F-number' kecil?", "id": "Lebih banyak cahaya masuk." },
  { "en": "Apa itu 'chromatic aberration'?", "id": "Distorsi warna pada tepi objek." },
  { "en": "Apa itu 'vignetting'?", "id": "Efek gambar lebih gelap di sudut." },
  { "en": "Apa itu 'convolution'?", "id": "Operasi matematis untuk filtering gambar." },
  { "en": "Apa itu 'kernel' dalam filtering?", "id": "Matriks kecil untuk operasi konvolusi." },
  { "en": "Apa itu 'median filter'?", "id": "Filter untuk hilangkan 'salt-and-pepper noise'." },
  { "en": "Apa itu 'thresholding'?", "id": "Mengubah gambar grayscale menjadi hitam-putih." },
  { "en": "Apa itu 'morphological operations'?", "id": "Operasi pada bentuk objek gambar." },
  { "en": "Contoh 'morphological operations'?", "id": "Erosion dan Dilation." },
  { "en": "Apa itu 'object permanence'?", "id": "Masalah analitik saat objek terhalang." },
  { "en": "Apa itu 'occlusion'?", "id": "Objek terhalang oleh objek lain." },
  { "en": "Apa itu 'predictive policing'?", "id": "Prediksi kejahatan berbasis analisa data." },
  { "en": "Peran sinyal di dalamnya?", "id": "Sebagai salah satu sumber data analisa." },
  { "en": "Apa itu 'geospatial intelligence' (GEOINT)?", "id": "Intelijen dari data geospasial." },
  { "en": "Kepanjangan GEOINT (Geospatial Intelligence)?", "id": "Geospatial Intelligence." },
  { "en": "Kaitan GEOINT (Geospatial Intelligence) dan bukti digital?", "id": "Memetakan dan mengkorelasikan lokasi bukti." },
  { "en": "Apa itu 'Windows Registry'?", "id": "Database konfigurasi sistem operasi Windows." },
  { "en": "Fungsi forensik 'Registry'?", "id": "Melacak aktivitas pengguna dan program." },
  { "en": "Apa itu 'prefetch files'?", "id": "File berisi jejak eksekusi program." },
  { "en": "Fungsi forensik 'prefetch files'?", "id": "Mengetahui program apa saja dijalankan." },
  { "en": "Apa itu 'jumplists'?", "id": "Daftar file yang baru diakses." },
  { "en": "Apa itu 'shellbags'?", "id": "Jejak folder yang pernah dibuka." },
  { "en": "Apa itu 'hibernation file'?", "id": "File berisi salinan isi RAM." },
  { "en": "Fungsi 'hibernation file' forensik?", "id": "Analisa memori dari sistem mati." },
  { "en": "Apa itu 'pagefile' atau 'swap file'?", "id": "Memori virtual di hard disk." },
  { "en": "Apa itu 'database forensics'?", "id": "Forensik pada sistem database." },
  { "en": "Apa itu 'log analysis'?", "id": "Proses memeriksa file log sistem." },
  { "en": "Tujuan utama 'log analysis'?", "id": "Membangun kronologi dan menemukan anomali." },
  { "en": "Apa itu 'Master File Table' (MFT)?", "id": "Indeks semua file di partisi NTFS." },
  { "en": "Fungsi MFT (Master File Table) dalam forensik?", "id": "Menemukan informasi file yang dihapus." },
  { "en": "Langkah 'hardening' NVR (Network Video Recorder)?", "id": "Ubah port, matikan service, update." },
  { "en": "Ancaman botnet Mirai?", "id": "Menyerang perangkat IoT, termasuk CCTV." },
  { "en": "Apa itu enkripsi 'end-to-end' (E2EE)?", "id": "Enkripsi dari kamera hingga monitor." },
  { "en": "Tujuan VLAN (Virtual LAN) untuk bukti?", "id": "Mengisolasi trafik forensik dari jaringan lain." },
  { "en": "Pentingnya manajemen 'firmware'?", "id": "Untuk menambal celah keamanan terbaru." },
  { "en": "Kamera 'multi-sensor panoramic'?", "id": "Gabungan beberapa kamera jadi satu." },
  { "en": "Keuntungan kamera 'panoramic'?", "id": "Cakupan area sangat luas." },
  { "en": "Teknologi 'laser night vision'?", "id": "Gunakan laser untuk penglihatan malam." },
  { "en": "Keunggulan 'laser night vision'?", "id": "Jarak pandang malam sangat jauh." },
  { "en": "Apa itu sensor LiDAR?", "id": "Deteksi jarak menggunakan pulsa laser." },
  { "en": "Beda LiDAR dan video forensik?", "id": "LiDAR ukur jarak, video rekam gambar." },
  { "en": "Apa itu 'AI on the edge'?", "id": "Proses analitik AI langsung di perangkat." },
  { "en": "Keuntungan 'AI on the edge'?", "id": "Mengurangi beban server dan bandwidth." },
  { "en": "Apa itu 'kalibrasi kamera'?", "id": "Mengoreksi parameter internal lensa kamera." },
  { "en": "Tujuan 'kalibrasi' untuk forensik?", "id": "Akurasi pengukuran (photogrammetry)." },
  { "en": "Apa itu 'tiered storage'?", "id": "Penyimpanan berjenjang sesuai prioritas." },
  { "en": "Contoh 'tiered storage' forensik?", "id": "Kasus aktif di SSD, arsip di LTO." },
  { "en": "Kepanjangan VSaaS?", "id": "Video Surveillance as a Service." },
  { "en": "Model bisnis VSaaS?", "id": "Berlangganan layanan CCTV via cloud." },
  { "en": "Definisi RAID 0?", "id": "Striping, menggabung disk tanpa redundansi." },
  { "en": "Definisi RAID 1?", "id": "Mirroring, duplikasi data ke disk lain." },
  { "en": "Definisi RAID 5?", "id": "Striping dengan satu paritas terdistribusi." },
  { "en": "Definisi RAID 6?", "id": "Striping dengan dua paritas terdistribusi." },
  { "en": "Definisi RAID 10?", "id": "Gabungan dari mirroring dan striping." },
  { "en": "Apa itu 'write endurance' SSD?", "id": "Batas siklus tulis sebuah SSD." },
  { "en": "Pentingnya 'write endurance' forensik?", "id": "Akuisisi data butuh daya tahan tulis." },
  { "en": "Syarat video 'enhancement' di pengadilan?", "id": "Proses terdokumentasi, tidak mengubah fakta." },
  { "en": "Dokumentasi 'enhancement' berisi?", "id": "Software, filter, dan parameter yang dipakai." },
  { "en": "Beda 'clarification' & 'manipulation'?", "id": "Clarification menjernihkan, manipulasi mengubah." },
  { "en": "Peran saksi ahli forensik video?", "id": "Menjelaskan proses teknis di pengadilan." },
  { "en": "Korelasi bukti video & BTS (Base Transceiver Station)?", "id": "Mencocokkan rekaman dengan lokasi HP." },
  { "en": "Tantangan Big Data di forensik?", "id": "Volume data besar, kecepatan proses." },
  { "en": "Apa itu 'real-time processing'?", "id": "Data diolah saat itu juga." },
  { "en": "Apa itu 'batch processing'?", "id": "Data dikumpulkan dulu, baru diolah." },
  { "en": "Apa itu 'false positive' analitik?", "id": "Alarm palsu dari deteksi keliru." },
  { "en": "Apa itu 'false negative' analitik?", "id": "Kejadian penting gagal terdeteksi." },
  { "en": "Cara mengurangi 'false positive'?", "id": "Kalibrasi, atur sensitivitas, gunakan AI." },
  { "en": "Apa itu 'optical character recognition' (OCR)?", "id": "Membaca teks dari dalam gambar." },
  { "en": "Kepanjangan OCR?", "id": "Optical Character Recognition." },
  { "en": "Penerapan OCR (Optical Character Recognition) forensik?", "id": "Membaca dokumen, plat nomor." },
  { "en": "Apa itu 'video stitching'?", "id": "Menggabungkan video dari banyak kamera." },
  { "en": "Apa itu 'defog'?", "id": "Fitur digital mengurangi efek kabut." },
  { "en": "Apa itu 'electronic image stabilization' (EIS)?", "id": "Stabilisasi gambar secara digital." },
  { "en": "Kepanjangan EIS?", "id": "Electronic Image Stabilization." },
  { "en": "Apa itu 'optical image stabilization' (OIS)?", "id": "Stabilisasi gambar menggunakan hardware lensa." },
  { "en": "Kepanjangan OIS?", "id": "Optical Image Stabilization." },
  { "en": "Mana lebih efektif, EIS (Electronic Image Stabilization) / OIS (Optical Image Stabilization)?", "id": "OIS lebih efektif." },
  { "en": "Apa itu 'digital signature' bukti?", "id": "Memastikan bukti digital tidak diubah." },
  { "en": "Apa itu 'secure boot' workstation?", "id": "Memastikan hanya OS asli yang jalan." },
  { "en": "Apa itu API (Application Programming Interface) forensik?", "id": "Antarmuka untuk integrasi software forensik." },
  { "en": "Apa itu SDK (Software Development Kit) forensik?", "id": "Alat untuk mengembangkan fitur forensik." },
  { "en": "Kepanjangan SDK?", "id": "Software Development Kit." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan dalam transmisi data." },
  { "en": "Kepanjangan BER?", "id": "Bit Error Rate." },
  { "en": "Apa itu 'gamma correction'?", "id": "Menyesuaikan kecerahan gambar non-linear." },
  { "en": "Apa itu 'dynamic DNS' (DDNS)?", "id": "Layanan untuk IP address dinamis." },
  { "en": "Kepanjangan DDNS?", "id": "Dynamic Domain Name System." },
  { "en": "Fungsi DDNS (Dynamic Domain Name System) untuk investigasi?", "id": "Melacak server dengan IP berubah." },
  { "en": "Apa itu 'port forwarding'?", "id": "Mengarahkan trafik port ke IP lokal." },
  { "en": "Risiko 'port forwarding' keamanan?", "id": "Membuka celah keamanan jika salah." },
  { "en": "Alternatif 'port forwarding'?", "id": "Gunakan layanan P2P atau cloud." },
  { "en": "Apa itu P2P (Peer-to-Peer) streaming?", "id": "Koneksi video langsung antar perangkat." },
  { "en": "Apa itu UPS (Uninterruptible Power Supply)?", "id": "Uninterruptible Power Supply." },
  { "en": "Fungsi UPS (Uninterruptible Power Supply) untuk workstation forensik?", "id": "Cegah mati mendadak saat akuisisi." },
  { "en": "Apa itu 'surge protector'?", "id": "Pelindung dari lonjakan listrik." },
  { "en": "Pentingnya 'surge protector' forensik?", "id": "Mencegah kerusakan barang bukti elektronik." },
  { "en": "Apa itu 'data center' forensik?", "id": "Pusat penyimpanan bukti digital." },
  { "en": "Syarat 'data center' forensik?", "id": "Aman, akses terbatas, suhu terjaga." },
  { "en": "Apa itu 'hot-swappable' HDD?", "id": "HDD bisa diganti tanpa matikan sistem." },
  { "en": "Apa itu 'Network Time Security' (NTS)?", "id": "Versi aman dari NTP." },
  { "en": "Kepanjangan NTS?", "id": "Network Time Security." },
  { "en": "Apa itu 'data lifecycle management'?", "id": "Manajemen data dari dibuat-dihapus." },
  { "en": "Tahap 'data lifecycle'?", "id": "Create, store, use, share, archive, destroy." },
  { "en": "Apa itu 'archiving' bukti?", "id": "Menyimpan bukti jangka panjang." },
  { "en": "Media untuk 'archiving'?", "id": "Tape LTO, cloud storage dingin." },
  { "en": "Apa itu 'data classification'?", "id": "Mengelompokkan data berdasarkan sensitivitas." },
  { "en": "Tujuan 'data classification'?", "id": "Menerapkan kontrol keamanan yang sesuai." },
  { "en": "Apa itu 'Gaussian blur'?", "id": "Filter untuk menghaluskan gambar." },
  { "en": "Apa itu 'Unsharp Mask'?", "id": "Filter untuk mempertajam detail gambar." },
  { "en": "Apa itu 'histogram of oriented gradients' (HOG)?", "id": "Metode deteksi objek (misal: manusia)." },
  { "en": "Kepanjangan HOG?", "id": "Histogram of Oriented Gradients." },
  { "en": "Apa itu 'machine learning'?", "id": "Sistem yang belajar dari data." },
  { "en": "Beda AI (Artificial Intelligence) & Machine Learning?", "id": "Machine Learning adalah sub-bidang AI." },
  { "en": "Apa itu 'training data'?", "id": "Data untuk melatih model AI." },
  { "en": "Apa itu 'inference' AI?", "id": "Proses AI membuat prediksi." },
  { "en": "Apa itu 'false match rate' (FMR)?", "id": "Tingkat kesalahan identifikasi positif palsu." },
  { "en": "Apa itu 'false non-match rate' (FNMR)?", "id": "Tingkat kegagalan sistem mengenali." },
  { "en": "Apa itu 'equal error rate' (EER)?", "id": "Titik saat FMR dan FNMR sama." },
  { "en": "Kepanjangan EER?", "id": "Equal Error Rate." },
  { "en": "Apa itu 'privacy impact assessment' (PIA)?", "id": "Penilaian dampak proyek terhadap privasi." },
  { "en": "Kepanjangan PIA?", "id": "Privacy Impact Assessment." },
  { "en": "Kapan PIA (Privacy Impact Assessment) diperlukan?", "id": "Sebelum implementasi sistem pengawasan baru." },
  { "en": "Apa itu 'motion compensation'?", "id": "Teknik kompresi video antar frame." },
  { "en": "Apa itu 'I-frame'?", "id": "Intra-coded frame (frame gambar utuh)." },
  { "en": "Apa itu 'P-frame'?", "id": "Predicted frame (berdasar frame sebelumnya)." },
  { "en": "Apa itu 'B-frame'?", "id": "Bi-directional predicted frame." },
  { "en": "Langkah 'hardening' NVR (Network Video Recorder)?", "id": "Ubah port, matikan service, update." },
  { "en": "Ancaman botnet Mirai?", "id": "Menyerang perangkat IoT, termasuk CCTV." },
  { "en": "Apa itu enkripsi 'end-to-end' (E2EE)?", "id": "Enkripsi dari kamera hingga monitor." },
  { "en": "Tujuan VLAN (Virtual LAN) untuk bukti?", "id": "Mengisolasi trafik forensik dari jaringan lain." },
  { "en": "Pentingnya manajemen 'firmware'?", "id": "Untuk menambal celah keamanan terbaru." },
  { "en": "Kamera 'multi-sensor panoramic'?", "id": "Gabungan beberapa kamera jadi satu." },
  { "en": "Keuntungan kamera 'panoramic'?", "id": "Cakupan area sangat luas." },
  { "en": "Teknologi 'laser night vision'?", "id": "Gunakan laser untuk penglihatan malam." },
  { "en": "Keunggulan 'laser night vision'?", "id": "Jarak pandang malam sangat jauh." },
  { "en": "Apa itu sensor LiDAR (Light Detection and Ranging)?", "id": "Deteksi jarak menggunakan pulsa laser." },
  { "en": "Beda LiDAR (Light Detection and Ranging) dan video forensik?", "id": "LiDAR ukur jarak, video rekam gambar." },
  { "en": "Apa itu 'AI (Artificial Intelligence) on the edge'?", "id": "Proses analitik AI langsung di perangkat." },
  { "en": "Keuntungan 'AI (Artificial Intelligence) on the edge'?", "id": "Mengurangi beban server dan bandwidth." },
  { "en": "Apa itu 'kalibrasi kamera'?", "id": "Mengoreksi parameter internal lensa kamera." },
  { "en": "Tujuan 'kalibrasi' untuk forensik?", "id": "Akurasi pengukuran (photogrammetry)." },
  { "en": "Apa itu 'tiered storage'?", "id": "Penyimpanan berjenjang sesuai prioritas." },
  { "en": "Contoh 'tiered storage' forensik?", "id": "Kasus aktif di SSD, arsip di LTO." },
  { "en": "Kepanjangan VSaaS (Video Surveillance as a Service)?", "id": "Video Surveillance as a Service." },
  { "en": "Model bisnis VSaaS (Video Surveillance as a Service)?", "id": "Berlangganan layanan CCTV via cloud." },
  { "en": "Definisi RAID 0 (Redundant Array of Independent Disks)?", "id": "Striping, menggabung disk tanpa redundansi." },
  { "en": "Definisi RAID 1 (Redundant Array of Independent Disks)?", "id": "Mirroring, duplikasi data ke disk lain." },
  { "en": "Definisi RAID 5 (Redundant Array of Independent Disks)?", "id": "Striping dengan satu paritas terdistribusi." },
  { "en": "Definisi RAID 6 (Redundant Array of Independent Disks)?", "id": "Striping dengan dua paritas terdistribusi." },
  { "en": "Definisi RAID 10 (Redundant Array of Independent Disks)?", "id": "Gabungan dari mirroring dan striping." },
  { "en": "Apa itu 'write endurance' SSD (Solid State Drive)?", "id": "Batas siklus tulis sebuah SSD." },
  { "en": "Pentingnya 'write endurance' forensik?", "id": "Akuisisi data butuh daya tahan tulis." },
  { "en": "Syarat video 'enhancement' di pengadilan?", "id": "Proses terdokumentasi, tidak mengubah fakta." },
  { "en": "Dokumentasi 'enhancement' berisi?", "id": "Software, filter, dan parameter yang dipakai." },
  { "en": "Beda 'clarification' & 'manipulation'?", "id": "Clarification menjernihkan, manipulasi mengubah." },
  { "en": "Peran saksi ahli video?", "id": "Menjelaskan proses forensik di pengadilan." },
  { "en": "Korelasi bukti video & BTS (Base Transceiver Station)?", "id": "Mencocokkan rekaman dengan lokasi HP." },
  { "en": "Tantangan Big Data di forensik?", "id": "Volume data besar, kecepatan proses." },
  { "en": "Apa itu 'real-time processing'?", "id": "Data diolah saat itu juga." },
  { "en": "Apa itu 'batch processing'?", "id": "Data dikumpulkan dulu, baru diolah." },
  { "en": "Apa itu 'false positive' analitik?", "id": "Alarm palsu dari deteksi keliru." },
  { "en": "Apa itu 'false negative' analitik?", "id": "Kejadian penting gagal terdeteksi." },
  { "en": "Cara mengurangi 'false positive'?", "id": "Kalibrasi, atur sensitivitas, gunakan AI." },
  { "en": "Apa itu 'optical character recognition' (OCR)?", "id": "Membaca teks dari dalam gambar." },
  { "en": "Kepanjangan OCR (Optical Character Recognition)?", "id": "Optical Character Recognition." },
  { "en": "Penerapan OCR (Optical Character Recognition) forensik?", "id": "Membaca dokumen, plat nomor." },
  { "en": "Apa itu 'video stitching'?", "id": "Menggabungkan video dari banyak kamera." },
  { "en": "Apa itu 'defog'?", "id": "Fitur digital mengurangi efek kabut." },
  { "en": "Apa itu 'electronic image stabilization' (EIS)?", "id": "Stabilisasi gambar secara digital." },
  { "en": "Kepanjangan EIS (Electronic Image Stabilization)?", "id": "Electronic Image Stabilization." },
  { "en": "Apa itu 'optical image stabilization' (OIS)?", "id": "Stabilisasi gambar menggunakan hardware lensa." },
  { "en": "Kepanjangan OIS (Optical Image Stabilization)?", "id": "Optical Image Stabilization." },
  { "en": "Mana lebih efektif, EIS (Electronic Image Stabilization) / OIS (Optical Image Stabilization)?", "id": "OIS lebih efektif." },
  { "en": "Apa itu 'digital signature' bukti?", "id": "Memastikan bukti digital tidak diubah." },
  { "en": "Apa itu 'secure boot' NVR (Network Video Recorder)?", "id": "Memastikan hanya firmware asli yang jalan." },
  { "en": "Apa itu API (Application Programming Interface) VMS?", "id": "Antarmuka untuk integrasi dengan sistem lain." },
  { "en": "Apa itu SDK (Software Development Kit) VMS?", "id": "Alat untuk mengembangkan fitur tambahan." },
  { "en": "Kepanjangan SDK (Software Development Kit)?", "id": "Software Development Kit." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan dalam transmisi data." },
  { "en": "Kepanjangan BER (Bit Error Rate)?", "id": "Bit Error Rate." },
  { "en": "Apa itu 'gamma correction'?", "id": "Menyesuaikan kecerahan gambar non-linear." },
  { "en": "Apa itu 'dynamic DNS' (DDNS)?", "id": "Layanan untuk IP address dinamis." },
  { "en": "Kepanjangan DDNS (Dynamic Domain Name System)?", "id": "Dynamic Domain Name System." },
  { "en": "Fungsi DDNS (Dynamic Domain Name System) untuk investigasi?", "id": "Melacak server dengan IP berubah." },
  { "en": "Apa itu 'port forwarding'?", "id": "Mengarahkan trafik port ke IP lokal." },
  { "en": "Risiko 'port forwarding' keamanan?", "id": "Membuka celah keamanan jika salah." },
  { "en": "Alternatif 'port forwarding'?", "id": "Gunakan layanan P2P atau cloud." },
  { "en": "Apa itu P2P (Peer-to-Peer) CCTV?", "id": "Koneksi langsung tanpa port forwarding." },
  { "en": "Apa itu UPS (Uninterruptible Power Supply)?", "id": "Uninterruptible Power Supply." },
  { "en": "Fungsi UPS (Uninterruptible Power Supply) untuk NVR?", "id": "Memberi daya saat listrik padam." },
  { "en": "Apa itu 'surge protector'?", "id": "Pelindung dari lonjakan listrik." },
  { "en": "Pentingnya 'surge protector' forensik?", "id": "Mencegah kerusakan barang bukti elektronik." },
  { "en": "Apa itu 'data center' CCTV?", "id": "Pusat penyimpanan dan pengolahan data." },
  { "en": "Syarat 'data center' CCTV?", "id": "Aman, dingin, daya listrik stabil." },
  { "en": "Apa itu 'hot-swappable' HDD (Hard Disk Drive)?", "id": "HDD bisa diganti tanpa matikan NVR." },
  { "en": "Apa itu 'Network Time Security' (NTS)?", "id": "Versi aman dari NTP." },
  { "en": "Kepanjangan NTS (Network Time Security)?", "id": "Network Time Security." },
  { "en": "Apa itu 'data lifecycle management'?", "id": "Manajemen data dari dibuat-dihapus." },
  { "en": "Tahap 'data lifecycle'?", "id": "Create, store, use, share, archive, destroy." },
  { "en": "Apa itu 'archiving' bukti?", "id": "Menyimpan bukti jangka panjang." },
  { "en": "Media untuk 'archiving'?", "id": "Tape LTO, cloud storage dingin." },
  { "en": "Apa itu 'data classification'?", "id": "Mengelompokkan data berdasarkan sensitivitas." },
  { "en": "Tujuan 'data classification'?", "id": "Menerapkan kontrol keamanan yang sesuai." },
  { "en": "Apa itu 'Gaussian blur'?", "id": "Filter untuk menghaluskan gambar." },
  { "en": "Apa itu 'Unsharp Mask'?", "id": "Filter untuk mempertajam detail gambar." },
  { "en": "Apa itu 'histogram of oriented gradients' (HOG)?", "id": "Metode deteksi objek (misal: manusia)." },
  { "en": "Kepanjangan HOG (Histogram of Oriented Gradients)?", "id": "Histogram of Oriented Gradients." },
  { "en": "Apa itu 'machine learning'?", "id": "Sistem yang belajar dari data." },
  { "en": "Beda AI (Artificial Intelligence) & Machine Learning?", "id": "Machine Learning adalah sub-bidang AI." },
  { "en": "Apa itu 'training data'?", "id": "Data untuk melatih model AI." },
  { "en": "Apa itu 'inference' AI (Artificial Intelligence)?", "id": "Proses AI membuat prediksi." },
  { "en": "Apa itu 'false match rate' (FMR)?", "id": "Tingkat kesalahan identifikasi positif palsu." },
  { "en": "Apa itu 'false non-match rate' (FNMR)?", "id": "Tingkat kegagalan sistem mengenali." },
  { "en": "Apa itu 'equal error rate' (EER)?", "id": "Titik saat FMR dan FNMR sama." },
  { "en": "Kepanjangan EER (Equal Error Rate)?", "id": "Equal Error Rate." },
  { "en": "Apa itu 'privacy impact assessment' (PIA)?", "id": "Penilaian dampak proyek terhadap privasi." },
  { "en": "Kepanjangan PIA (Privacy Impact Assessment)?", "id": "Privacy Impact Assessment." },
  { "en": "Kapan PIA (Privacy Impact Assessment) diperlukan?", "id": "Sebelum implementasi sistem pengawasan baru." },
  { "en": "Apa itu 'motion compensation'?", "id": "Teknik kompresi video antar frame." },
  { "en": "Apa itu 'I-frame'?", "id": "Intra-coded frame (frame gambar utuh)." },
  { "en": "Apa itu 'P-frame'?", "id": "Predicted frame (berdasar frame sebelumnya)." },
  { "en": "Apa itu 'B-frame'?", "id": "Bi-directional predicted frame." },
  { "en": "Beda 'Wavelet' & 'Fourier Transform'?", "id": "Wavelet analisa waktu dan frekuensi." },
  { "en": "Keunggulan 'Wavelet Transform'?", "id": "Baik untuk sinyal non-stasioner." },
  { "en": "Apa itu 'frequency domain'?", "id": "Representasi sinyal berdasarkan frekuensinya." },
  { "en": "Apa itu 'Point Spread Function' (PSF)?", "id": "Respon sistem optik terhadap titik." },
  { "en": "Kepanjangan PSF (Point Spread Function)?", "id": "Point Spread Function." },
  { "en": "Apa itu 'deconvolution'?", "id": "Proses membalikkan distorsi (blur)." },
  { "en": "Apa itu 'blind deconvolution'?", "id": "Deconvolution tanpa mengetahui PSF." },
  { "en": "Ruang warna standar internasional?", "id": "CIE 1931 XYZ." },
  { "en": "Fungsi metadata EXIF (Exchangeable image file format)?", "id": "Menyimpan informasi teknis sebuah gambar." },
  { "en": "Analisa artefak kompresi?", "id": "Mendeteksi manipulasi dari pola blok." },
  { "en": "Cara deteksi 'video splicing'?", "id": "Analisa noise, timestamp, dan kontinuitas." },
  { "en": "Apa itu 'frame duplication'?", "id": "Menyalin frame untuk menutupi aksi." },
  { "en": "Apa itu 'federated VMS (Video Management System)'?", "id": "Gabungan sistem VMS independen." },
  { "en": "Tujuan 'disaster recovery drill'?", "id": "Menguji prosedur pemulihan bencana." },
  { "en": "Apa itu 'capacity planning'?", "id": "Merencanakan kebutuhan sistem di masa depan." },
  { "en": "Apa itu 'Total Cost of Ownership' (TCO)?", "id": "Total biaya kepemilikan sistem." },
  { "en": "Komponen TCO (Total Cost of Ownership) bukti digital?", "id": "Hardware, software, listrik, maintenance." },
  { "en": "Masalah 'black box' pada AI (Artificial Intelligence)?", "id": "Sulit menjelaskan cara AI mengambil keputusan." },
  { "en": "Tantangan hukum AI (Artificial Intelligence) 'black box'?", "id": "Sulit dipertanggungjawabkan di pengadilan." },
  { "en": "Ancaman 'deepfake' sebagai bukti?", "id": "Potensi bukti palsu yang meyakinkan." },
  { "en": "Apa itu 'augmented reality' forensik?", "id": "Menampilkan info digital di atas bukti." },
  { "en": "SOP (Standard Operating Procedure) permintaan 'video redaction'?", "id": "Dasar hukum jelas, proses terdokumentasi." },
  { "en": "Konsekuensi 'chain of custody' rusak?", "id": "Bukti video bisa ditolak pengadilan." },
  { "en": "Beda malfungsi & serangan siber?", "id": "Analisa log, pola, dan IOC." },
  { "en": "Apa itu 'aperture' lensa?", "id": "Bukaan diafragma yang mengontrol cahaya." },
  { "en": "Apa itu ISO pada kamera?", "id": "Tingkat sensitivitas sensor terhadap cahaya." },
  { "en": "Efek ISO tinggi?", "id": "Gambar terang, tapi noise meningkat." },
  { "en": "Apa itu 'exposure triangle'?", "id": "Hubungan antara aperture, ISO, shutter speed." },
  { "en": "Apa itu 'pixel binning'?", "id": "Menggabungkan piksel untuk tingkatkan sensitivitas." },
  { "en": "Apa itu 'back-illuminated sensor' (BSI)?", "id": "Sensor dengan sensitivitas cahaya lebih baik." },
  { "en": "Kepanjangan BSI (Back-Side Illuminated) sensor?", "id": "Back-Side Illuminated." },
  { "en": "Apa itu 'dynamic range'?", "id": "Rasio antara bagian terterang-tergelap." },
  { "en": "Satuan 'dynamic range'?", "id": "Decibels (dB) atau stops." },
  { "en": "Apa itu 'tonal range'?", "id": "Distribusi kecerahan dalam sebuah gambar." },
  { "en": "Apa itu 'color gamut'?", "id": "Rentang warna yang bisa ditampilkan." },
  { "en": "Standar 'color gamut' umum?", "id": "sRGB, Adobe RGB, DCI-P3." },
  { "en": "Apa itu 'color depth'?", "id": "Jumlah bit untuk merepresentasikan warna." },
  { "en": "Contoh 'color depth'?", "id": "8-bit, 10-bit, 12-bit." },
  { "en": "Efek 'color depth' tinggi?", "id": "Gradasi warna lebih halus." },
  { "en": "Apa itu 'aliasing'?", "id": "Distorsi pada sinyal frekuensi tinggi." },
  { "en": "Contoh 'aliasing' pada video?", "id": "Moire pattern pada garis rapat." },
  { "en": "Fungsi 'anti-aliasing filter'?", "id": "Mengurangi efek aliasing." },
  { "en": "Apa itu 'Nyquist theorem'?", "id": "Prinsip dasar dalam sampling sinyal." },
  { "en": "Apa itu 'interpolation'?", "id": "Memperkirakan nilai di antara data." },
  { "en": "Fungsi 'interpolation' pada gambar?", "id": "Mengubah ukuran gambar (resizing)." },
  { "en": "Apa itu 'extrapolation'?", "id": "Memperkirakan nilai di luar data." },
  { "en": "Apa itu 'convolutional kernel'?", "id": "Matriks untuk operasi filter gambar." },
  { "en": "Apa itu 'image registration'?", "id": "Menyelaraskan beberapa gambar menjadi satu." },
  { "en": "Apa itu 'optical flow'?", "id": "Pola pergerakan objek antar frame." },
  { "en": "Fungsi 'optical flow'?", "id": "Estimasi kecepatan dan arah gerakan." },
  { "en": "Apa itu 'background subtraction'?", "id": "Memisahkan objek bergerak dari latar." },
  { "en": "Tantangan 'background subtraction'?", "id": "Perubahan cahaya, latar belakang dinamis." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Analisa sinyal dalam domain frekuensi." },
  { "en": "Apa itu 'power spectrum'?", "id": "Distribusi kekuatan sinyal per frekuensi." },
  { "en": "Apa itu 'phase correlation'?", "id": "Metode untuk mengukur pergeseran gambar." },
  { "en": "Apa itu 'feature matching'?", "id": "Mencocokkan fitur unik antar gambar." },
  { "en": "Algoritma 'feature matching'?", "id": "SIFT, SURF, ORB." },
  { "en": "Apa itu 'affine transformation'?", "id": "Transformasi geometri (rotasi, skala)." },
  { "en": "Apa itu 'perspective transformation'?", "id": "Transformasi untuk memperbaiki sudut pandang." },
  { "en": "Apa itu 'epipolar geometry'?", "id": "Geometri dari sistem dua kamera." },
  { "en": "Apa itu 'stereo vision'?", "id": "Membuat persepsi kedalaman dari dua kamera." },
  { "en": "Fungsi 'stereo vision'?", "id": "Mengukur jarak objek secara akurat." },
  { "en": "Apa itu 'structured light'?", "id": "Memproyeksikan pola cahaya untuk scan 3D." },
  { "en": "Apa itu 'time-of-flight' (ToF) camera?", "id": "Kamera yang mengukur jarak." },
  { "en": "Kepanjangan ToF (Time-of-Flight) camera?", "id": "Time-of-Flight camera." },
  { "en": "Prinsip kerja ToF (Time-of-Flight)?", "id": "Mengukur waktu pantul cahaya inframerah." },
  { "en": "Apa itu 'camera trapping'?", "id": "Penggunaan kamera otomatis di alam liar." },
  { "en": "Kaitan GEOINT (Geospatial Intelligence) dan forensik?", "id": "Memetakan dan mengkorelasikan lokasi bukti." },
  { "en": "Apa itu 'pattern recognition'?", "id": "Pengenalan pola dalam data." },
  { "en": "Apa itu 'anomaly detection'?", "id": "Deteksi kejadian yang menyimpang." },
  { "en": "Penerapan 'anomaly detection' forensik?", "id": "Deteksi aktivitas mencurigakan di log." },
  { "en": "Apa itu 'edge computing'?", "id": "Komputasi yang dilakukan dekat sumber data." },
  { "en": "Contoh 'edge computing' forensik?", "id": "Akuisisi data awal di perangkat." },
  { "en": "Apa itu 'fog computing'?", "id": "Lapisan komputasi antara edge-cloud." },
  { "en": "Apa itu 'video transcoding'?", "id": "Mengubah format atau bitrate video." },
  { "en": "Apa itu 'adaptive bitrate streaming'?", "id": "Menyesuaikan kualitas video dengan bandwidth." },
  { "en": "Apa itu 'jitter buffer'?", "id": "Buffer untuk mengatasi variasi waktu tunda." },
  { "en": "Apa itu 'peer-to-peer' (P2P) streaming?", "id": "Streaming langsung antar perangkat." },
  { "en": "Apa itu 'WebRTC' (Web Real-Time Communication)?", "id": "Standar komunikasi video realtime di web." },
  { "en": "Kepanjangan WebRTC (Web Real-Time Communication)?", "id": "Web Real-Time Communication." },
  { "en": "Apa itu 'gimbal'?", "id": "Alat mekanis untuk stabilisasi kamera." },
  { "en": "Fungsi 'gimbal' pada drone forensik?", "id": "Menjaga rekaman TKP tetap stabil." },
  { "en": "Apa itu 'rolling shutter compensation'?", "id": "Software untuk perbaiki efek rolling shutter." },
  { "en": "Apa itu 'generative adversarial network' (GAN)?", "id": "Arsitektur AI untuk membuat data sintetis." },
  { "en": "Kepanjangan GAN (Generative Adversarial Network)?", "id": "Generative Adversarial Network." },
  { "en": "Penggunaan GAN (Generative Adversarial Network) untuk kejahatan?", "id": "Membuat deepfake video atau gambar." },
  { "en": "Penggunaan GAN (Generative Adversarial Network) untuk kebaikan?", "id": "Meningkatkan resolusi, membuat data latih." },
  { "en": "Apa itu 'Bayesian filtering'?", "id": "Filter statistik berbasis probabilitas." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Filter untuk estimasi dan prediksi." },
  { "en": "Fungsi 'Kalman filter' forensik?", "id": "Melacak pergerakan objek secara akurat." },
  { "en": "Dasar 'camera forensics'?", "id": "Analisa PRNU, artefak lensa." },
  { "en": "Apa itu 'video game forensics'?", "id": "Forensik pada konsol atau game." },
  { "en": "Apa itu 'drone forensics'?", "id": "Forensik pada pesawat tanpa awak." },
  { "en": "Data dari 'drone forensics'?", "id": "Log penerbangan, video, data GPS." },
  { "en": "Apa itu 'vehicle forensics'?", "id": "Forensik pada sistem infotainment mobil." },
  { "en": "Apa itu 'IoT forensics'?", "id": "Forensik pada perangkat Internet of Things." },
  { "en": "Tantangan 'IoT forensics'?", "id": "Keragaman perangkat, data volatile." },
  { "en": "Apa itu 'wearable forensics'?", "id": "Forensik pada perangkat wearable." },
  { "en": "Contoh perangkat 'wearable'?", "id": "Smartwatch, fitness tracker." },
  { "en": "Jelaskan prinsip 'Daubert Standard' pada bukti forensik?", "id": "Standar ilmiah untuk penerimaan bukti ahli." },
  { "en": "Bagaimana 'hash collision' dapat melemahkan bukti digital?", "id": "Dua file berbeda punya hash sama." },
  { "en": "Apa itu 'side-channel attack' pada perangkat forensik?", "id": "Serangan via konsumsi daya atau emisi." },
  { "en": "Fungsi 'Secure Element' (SE) di smartphone?", "id": "Chip aman untuk simpan data kriptografi." },
  { "en": "Tantangan forensik pada 'File-Based Encryption' (FBE)?", "id": "Setiap file dienkripsi dengan kunci berbeda." },
  { "en": "Apa itu 'memory remanence'?", "id": "Sisa data di memori setelah dimatikan." },
  { "en": "Teknik eksploitasi 'memory remanence'?", "id": "Cold boot attack." },
  { "en": "Peran 'digital twin' dalam investigasi?", "id": "Simulasi digital dari sistem atau TKP." },
  { "en": "Apa itu 'probabilistic data structure'?", "id": "Struktur data efisien tapi tidak 100% akurat." },
  { "en": "Contoh 'probabilistic data structure'?", "id": "Bloom filter." },
  { "en": "Apa itu 'homomorphic encryption' untuk investigasi?", "id": "Analisa data terenkripsi tanpa dekripsi." },
  { "en": "Implikasi 'quantum computing' pada kriptografi?", "id": "Dapat memecahkan algoritma RSA dan ECC." },
  { "en": "Solusi menghadapi ancaman kuantum?", "id": "Post-quantum cryptography (PQC)." },
  { "en": "Bagaimana 'zero-knowledge proof' (ZKP) diterapkan?", "id": "Verifikasi identitas tanpa serahkan data." },
  { "en": "Dilema etis 'predictive policing'?", "id": "Potensi bias dan diskriminasi." },
  { "en": "Apa itu 'adversarial example' pada AI (Artificial Intelligence)?", "id": "Input yang sengaja dibuat menipu AI." },
  { "en": "Apa itu 'model inversion attack'?", "id": "Serangan untuk merekonstruksi data latih AI." },
  { "en": "Fungsi 'neuron' dalam 'neural network'?", "id": "Unit pemrosesan informasi dasar." },
  { "en": "Apa itu 'activation function'?", "id": "Fungsi yang menentukan output neuron." },
  { "en": "Apa itu 'backpropagation'?", "id": "Metode melatih jaringan saraf tiruan." },
  { "en": "Analisa sinyal pada sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Mendeteksi anomali pada sistem industri." },
  { "en": "Apa itu 'JTAG (Joint Test Action Group)' Forensics?", "id": "Akses level rendah ke chip perangkat." },
  { "en": "Apa itu 'chip-off' forensics?", "id": "Melepas chip memori untuk dibaca langsung." },
  { "en": "Kapan 'chip-off' forensics dilakukan?", "id": "Sebagai usaha terakhir jika perangkat rusak." },
  { "en": "Beda 'regular expression' dan 'keyword search'?", "id": "Regex mencari pola, keyword kata pasti." }



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
