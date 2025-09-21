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


  { "en": "Apa Itu Transformasi Laplace?", "id": "Mengubah Fungsi Waktu Ke Fungsi Frekuensi." },
  { "en": "Siapa Penemu Transformasi Laplace?", "id": "Pierre-Simon Laplace Seorang Matematikawan Perancis." },
  { "en": "Simbol Transformasi Laplace?", "id": "Biasanya Disimbolkan Dengan Huruf L." },
  { "en": "Variabel Domain Waktu?", "id": "Variabel t Mewakili Waktu Nyata." },
  { "en": "Variabel Domain Frekuensi?", "id": "Variabel s Adalah Frekuensi Kompleks." },
  { "en": "Bentuk Variabel s?", "id": "s Sama Dengan Sigma Ditambah j Omega." },
  { "en": "Fungsi Awal Disebut Apa?", "id": "Fungsi Dalam Domain Waktu Atau f(t)." },
  { "en": "Hasil Transformasi Disebut Apa?", "id": "Fungsi Dalam Domain Frekuensi Atau F(s)." },
  { "en": "Definisi Integral Transformasi Laplace?", "id": "Integral f(t) Dikali e Pangkat -st." },
  { "en": "Batas Integral Definisi?", "id": "Dari Nol Sampai Tak Hingga." },
  { "en": "Kenapa Batasnya Dari Nol?", "id": "Karena Menganalisis Sistem Untuk Waktu Positif." },
  { "en": "Apa Kegunaan Utama?", "id": "Menyelesaikan Persamaan Diferensial Biasa Linier." },
  { "en": "Bidang Ilmu Yang Menggunakan?", "id": "Teknik Elektro Fisika Dan Teknik Mesin." },
  { "en": "Apa Itu Transformasi Balik?", "id": "Inverse Laplace Transform Mengembalikan Ke Domain Waktu." },
  { "en": "Simbol Transformasi Balik?", "id": "Simbol L Pangkat Negatif Satu." },
  { "en": "Sifat Paling Dasar?", "id": "Sifat Linearitas Transformasi Laplace." },
  { "en": "Apa Maksud Sifat Linearitas?", "id": "Transformasi Jumlah Sama Dengan Jumlah Transformasi." },
  { "en": "Apa Transformasi Dari Konstanta C?", "id": "Hasilnya Adalah C Dibagi Dengan s." },
  { "en": "Apa Transformasi Fungsi Step?", "id": "Satu Dibagi Dengan Variabel Kompleks s." },
  { "en": "Nama Lain Fungsi Step?", "id": "Fungsi Heaviside Atau Unit Step Function." },
  { "en": "Apa Transformasi Fungsi Eksponensial?", "id": "Satu Dibagi Dengan s Dikurangi a." },
  { "en": "Bentuk Fungsi Eksponensial?", "id": "e Pangkat a Dikali Dengan t." },
  { "en": "Apa Transformasi Fungsi Sinus?", "id": "Omega Dibagi s Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Apa Transformasi Fungsi Cosinus?", "id": "s Dibagi s Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Apa Itu Sifat Pergeseran Frekuensi?", "id": "Mengalikan f(t) Dengan Eksponensial Menggeser F(s)." },
  { "en": "Apa Itu Sifat Pergeseran Waktu?", "id": "Menggeser f(t) Mengalikan F(s) Dengan Eksponensial." },
  { "en": "Transformasi Dari Turunan Pertama?", "id": "s Dikali F(s) Dikurangi f(0)." },
  { "en": "f(0) Mewakili Apa?", "id": "Kondisi Awal Sistem Pada Waktu Nol." },
  { "en": "Transformasi Dari Turunan Kedua?", "id": "s Kuadrat F(s) Dikurangi s f(0)." },
  { "en": "Transformasi Dari Integral?", "id": "F(s) Dibagi Dengan Variabel Kompleks s." },
  { "en": "Apa Itu Teorema Nilai Awal?", "id": "Mencari Nilai Awal f(0) Dari F(s)." },
  { "en": "Rumus Teorema Nilai Awal?", "id": "Limit s Menuju Tak Hingga sF(s)." },
  { "en": "Apa Itu Teorema Nilai Akhir?", "id": "Mencari Nilai f(t) Saat t Tak Hingga." },
  { "en": "Rumus Teorema Nilai Akhir?", "id": "Limit s Menuju Nol Dari sF(s)." },
  { "en": "Syarat Teorema Nilai Akhir?", "id": "Semua Pole Harus Di Sisi Kiri." },
  { "en": "Apa Itu Pole?", "id": "Nilai s Yang Membuat F(s) Tak Hingga." },
  { "en": "Apa Itu Zero?", "id": "Nilai s Yang Membuat F(s) Nol." },
  { "en": "Di Mana Letak Pole?", "id": "Akar-Akar Dari Bagian Penyebut Fungsi F(s)." },
  { "en": "Di Mana Letak Zero?", "id": "Akar-Akar Dari Bagian Pembilang Fungsi F(s)." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Transformasi Output Terhadap Input." },
  { "en": "Simbol Fungsi Transfer?", "id": "Biasanya Disimbolkan Dengan Huruf G(s) atau H(s)." },
  { "en": "Fungsi Transfer Menggambarkan Apa?", "id": "Karakteristik Dinamis Dari Sebuah Sistem Linear." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Pada Dua Fungsi (f*g)." },
  { "en": "Teorema Konvolusi Domain Waktu?", "id": "Konvolusi Di Waktu Menjadi Perkalian Di Frekuensi." },
  { "en": "Rumus Teorema Konvolusi?", "id": "Transformasi f(t)*g(t) Adalah F(s)G(s)." },
  { "en": "Apa Itu Fungsi Impuls?", "id": "Fungsi Dengan Luas Satu Durasi Sangat Singkat." },
  { "en": "Nama Lain Fungsi Impuls?", "id": "Fungsi Delta Dirac Atau Unit Impulse." },
  { "en": "Transformasi Fungsi Impuls?", "id": "Hasilnya Adalah Konstanta Bernilai Satu." },
  { "en": "Apa Itu Fungsi Ramp?", "id": "Fungsi Yang Naik Linier Terhadap Waktu." },
  { "en": "Bentuk Fungsi Ramp?", "id": "Fungsi f(t) Sama Dengan Variabel t." },
  { "en": "Transformasi Fungsi Ramp?", "id": "Satu Dibagi Dengan Variabel s Kuadrat." },
  { "en": "Apa Itu Region Of Convergence (ROC)?", "id": "Daerah Dimana Integral Transformasi Konvergen." },
  { "en": "Pentingnya ROC?", "id": "Menjamin Keunikan Hasil Transformasi Balik." },
  { "en": "ROC Untuk Sistem Kausal?", "id": "Terletak Di Sebelah Kanan Pole Paling Kanan." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Sistem Tidak Mendahului Inputnya." },
  { "en": "Bagaimana Laplace Menyederhanakan Masalah?", "id": "Mengubah Persamaan Diferensial Menjadi Persamaan Aljabar." },
  { "en": "Langkah Menyelesaikan Persamaan Diferensial?", "id": "Transformasi Selesaikan Dengan Aljabar Lalu Transformasi Balik." },
  { "en": "Apa Itu Ekspansi Fraksi Parsial?", "id": "Metode Untuk Memudahkan Proses Transformasi Balik." },
  { "en": "Tujuan Fraksi Parsial?", "id": "Memecah F(s) Kompleks Menjadi Bentuk Sederhana." },
  { "en": "Bentuk Sederhana Yang Diakui?", "id": "Bentuk Yang Ada Di Tabel Transformasi Laplace." },
  { "en": "Apa Itu Tabel Transformasi Laplace?", "id": "Daftar Pasangan Transformasi Fungsi Yang Umum." },
  { "en": "Apa Itu Sifat Penskalaan Waktu?", "id": "Mengubah Skala Waktu Pada Fungsi f(t)." },
  { "en": "Transformasi f(at)?", "id": "Satu Per a Dikali F(s/a)." },
  { "en": "Transformasi Sinus Hiperbolik (sinh)?", "id": "a Dibagi s Kuadrat Dikurangi a Kuadrat." },
  { "en": "Transformasi Cosinus Hiperbolik (cosh)?", "id": "s Dibagi s Kuadrat Dikurangi a Kuadrat." },
  { "en": "Sifat Diferensiasi Frekuensi?", "id": "Mengalikan f(t) Dengan -t." },
  { "en": "Hasil Diferensiasi Frekuensi?", "id": "Menghasilkan Turunan Pertama Dari Fungsi F(s)." },
  { "en": "Sifat Integrasi Frekuensi?", "id": "Membagi f(t) Dengan Variabel Waktu t." },
  { "en": "Hubungan Dengan Transformasi Fourier?", "id": "Transformasi Fourier Kasus Khusus Transformasi Laplace." },
  { "en": "Kapan Laplace Sama Dengan Fourier?", "id": "Ketika Bagian Riil s Sama Dengan Nol." },
  { "en": "Apa Kelebihan Laplace?", "id": "Bisa Menganalisis Fungsi Yang Tidak Stabil." },
  { "en": "Apa Itu Sistem Stabil?", "id": "Output Terbatas Untuk Input Yang Terbatas." },
  { "en": "Syarat Stabilitas Di Domain s?", "id": "Semua Pole Harus Berada Di Sisi Kiri." },
  { "en": "Apa Itu Bidang-s (s-Plane)?", "id": "Representasi Grafis Dari Domain Frekuensi Kompleks." },
  { "en": "Sumbu Horizontal Bidang-s?", "id": "Sumbu Riil Atau Sigma (σ)." },
  { "en": "Sumbu Vertikal Bidang-s?", "id": "Sumbu Imajiner Atau Omega (ω)." },
  { "en": "Sisi Kiri Bidang-s?", "id": "Daerah Dimana Bagian Riil s Negatif." },
  { "en": "Sisi Kanan Bidang-s?", "id": "Daerah Dimana Bagian Riil s Positif." },
  { "en": "Apa Itu Residu?", "id": "Konstanta Dalam Perhitungan Ekspansi Fraksi Parsial." },
  { "en": "Metode Menghitung Residu?", "id": "Metode Heaviside Cover-Up Adalah Salah Satunya." },
  { "en": "Aplikasi Dalam Rangkaian Listrik?", "id": "Menganalisis Rangkaian RLC Dalam Keadaan Transien." },
  { "en": "Impedansi Kapasitor Di Domain s?", "id": "Satu Dibagi Dengan s Dikali C." },
  { "en": "Impedansi Induktor Di Domain s?", "id": "s Dikali Dengan Induktansi L." },
  { "en": "Impedansi Resistor Di Domain s?", "id": "Tetap Sama Yaitu Sebesar R." },
  { "en": "Apa Itu Orde Sistem?", "id": "Pangkat Tertinggi s Di Penyebut Fungsi Transfer." },
  { "en": "Orde Sistem Menunjukkan Apa?", "id": "Jumlah Komponen Penyimpan Energi Dalam Sistem." },
  { "en": "Apa Itu Respon Transien?", "id": "Respon Sistem Sesaat Setelah Diberi Input." },
  { "en": "Apa Itu Respon Keadaan Tunak?", "id": "Perilaku Sistem Setelah Waktu Yang Sangat Lama." },
  { "en": "Sifat Periodisitas?", "id": "Transformasi Laplace Untuk Fungsi Yang Periodik." },
  { "en": "Apakah Transformasi Ini Unik?", "id": "Ya Unik Untuk Fungsi Yang Diberikan." },
  { "en": "Transformasi Bilateral?", "id": "Integral Dari Minus Tak Hingga Sampai Hingga." },
  { "en": "Transformasi Unilateral?", "id": "Integral Dari Nol Sampai Tak Hingga." },
  { "en": "Yang Umum Digunakan?", "id": "Transformasi Unilateral Untuk Sistem Dunia Nyata." },
  { "en": "Tipe Pole?", "id": "Pole Riil Pole Kompleks Dan Pole Berulang." },
  { "en": "Pole Kompleks Menghasilkan Respon?", "id": "Respon Berosilasi Atau Bergetar Dalam Sistem." },
  { "en": "Pole Riil Menghasilkan Respon?", "id": "Respon Eksponensial Naik Atau Turun." },
  { "en": "Pole Di Sumbu Imajiner?", "id": "Menghasilkan Osilasi Berkelanjutan Tanpa Redaman." },
  { "en": "Pole Di Sisi Kanan?", "id": "Menghasilkan Respon Yang Tidak Stabil Dan Tumbuh." },
  { "en": "Apa Transformasi Dari t Pangkat n?", "id": "n Faktorial Dibagi s Pangkat n+1." },
  { "en": "Syarat Untuk t Pangkat n?", "id": "n Harus Merupakan Bilangan Bulat Positif." },
  { "en": "Apa Transformasi Dari Fungsi Waktu f(t)?", "id": "Fungsi Frekuensi Kompleks Bernama F(s)." },
  { "en": "Apa Transformasi Balik Dari F(s)?", "id": "Fungsi Waktu Semula Yaitu f(t)." },
  { "en": "Tujuan Utama Transformasi?", "id": "Menyederhanakan Proses Analisis Dan Perhitungan." },
  { "en": "Persamaan Diferensial Menjadi Apa?", "id": "Persamaan Aljabar Yang Lebih Mudah Diselesaikan." },
  { "en": "Operasi Integral Menjadi Apa?", "id": "Operasi Pembagian Sederhana Dengan Variabel s." },
  { "en": "Operasi Turunan Menjadi Apa?", "id": "Operasi Perkalian Sederhana Dengan Variabel s." },
  { "en": "Apa Itu Pasangan Transformasi?", "id": "Sepasang Fungsi f(t) Dan F(s)." },
  { "en": "Fungsi Apa Yang Bisa Ditransformasi?", "id": "Fungsi Yang Kontinu Bagian Demi Bagian." },
  { "en": "Syarat Lain Fungsi f(t)?", "id": "Harus Memiliki Orde Eksponensial Terbatas." },
  { "en": "Apa Arti Orde Eksponensial?", "id": "Fungsi Tidak Tumbuh Lebih Cepat Eksponensial." },
  { "en": "Apa Itu Sifat Linearitas Bagian Satu?", "id": "Transformasi a f(t) Adalah a F(s)." },
  { "en": "Apa Itu Sifat Linearitas Bagian Dua?", "id": "Transformasi f(t)+g(t) Adalah F(s)+G(s)." },
  { "en": "Transformasi Dari Nol?", "id": "Hasilnya Adalah Nol Di Domain Frekuensi." },
  { "en": "Bagaimana Kondisi Awal Digunakan?", "id": "Dimasukkan Langsung Ke Dalam Persamaan Transformasi." },
  { "en": "Apa Keuntungan Menggunakan Kondisi Awal?", "id": "Solusi Lengkap Diperoleh Dalam Satu Langkah." },
  { "en": "Apa Itu Respon Impuls Sistem?", "id": "Output Sistem Ketika Inputnya Fungsi Impuls." },
  { "en": "Transformasi Respon Impuls?", "id": "Sama Dengan Fungsi Transfer Sistem H(s)." },
  { "en": "Apa Itu Respon Step Sistem?", "id": "Output Sistem Ketika Inputnya Fungsi Step." },
  { "en": "Bagaimana Mencari Respon Step?", "id": "Fungsi Transfer Dikalikan Dengan 1/s." },
  { "en": "Apa Itu Pole Dan Zero?", "id": "Frekuensi Kompleks Yang Mencirikan Perilaku Sistem." },
  { "en": "Pengaruh Pole Terhadap Sistem?", "id": "Menentukan Stabilitas Dan Karakteristik Respon Sistem." },
  { "en": "Pengaruh Zero Terhadap Sistem?", "id": "Mempengaruhi Amplitudo Dan Fasa Respon Sistem." },
  { "en": "Apa Itu Diagram Pole-Zero?", "id": "Plot Lokasi Pole Dan Zero Di Bidang-s." },
  { "en": "Simbol Pole Di Diagram?", "id": "Tanda Silang (x) Pada Bidang-s." },
  { "en": "Simbol Zero Di Diagram?", "id": "Tanda Lingkaran (o) Pada Bidang-s." },
  { "en": "Stabilitas Terlihat Dari Mana?", "id": "Dari Lokasi Pole-Pole Sistem." },
  { "en": "Sistem Stabil Jika?", "id": "Semua Pole Di Setengah Bidang Kiri." },
  { "en": "Sistem Tidak Stabil Jika?", "id": "Ada Pole Di Setengah Bidang Kanan." },
  { "en": "Sistem Stabil Marjinal Jika?", "id": "Pole Di Sumbu Imajiner Tanpa Berulang." },
  { "en": "Apa Itu Pole Dominan?", "id": "Pole Paling Dekat Dengan Sumbu Imajiner." },
  { "en": "Pengaruh Pole Dominan?", "id": "Sangat Mempengaruhi Perilaku Respon Transien." },
  { "en": "Transformasi Balik F(s-a)?", "id": "e Pangkat at Dikali Dengan f(t)." },
  { "en": "Transformasi Balik e Pangkat -as F(s)?", "id": "f(t-a) Dikali Dengan Fungsi u(t-a)." },
  { "en": "Apa Itu Fungsi u(t-a)?", "id": "Fungsi Step Yang Bergeser Sejauh a." },
  { "en": "Apa Arti Perkalian Dengan s?", "id": "Operasi Diferensiasi Dalam Domain Waktu." },
  { "en": "Apa Arti Pembagian Dengan s?", "id": "Operasi Integrasi Dalam Domain Waktu." },
  { "en": "Metode Populer Transformasi Balik?", "id": "Menggunakan Tabel Dan Ekspansi Fraksi Parsial." },
  { "en": "Kapan Fraksi Parsial Digunakan?", "id": "Ketika F(s) Adalah Fungsi Rasional Polinomial." },
  { "en": "Apa Itu Fungsi Rasional?", "id": "Fungsi Berbentuk Pembagian Dua Polinomial." },
  { "en": "Apa Itu Polinomial Proper?", "id": "Derajat Pembilang Lebih Kecil Dari Penyebut." },
  { "en": "Bagaimana Jika Polinomial Improper?", "id": "Lakukan Pembagian Panjang Terlebih Dahulu." },
  { "en": "Jenis Akar Penyebut?", "id": "Akar Riil Berbeda Kompleks Dan Berulang." },
  { "en": "Akar Kompleks Selalu Muncul?", "id": "Selalu Muncul Berpasangan Konjugat Kompleks." },
  { "en": "Pasangan Konjugat Kompleks?", "id": "a + jb Dan a - jb." },
  { "en": "Akar Kompleks Menghasilkan Fungsi Apa?", "id": "Fungsi Sinusoidal Yang Teredam Atau Tumbuh." },
  { "en": "Faktor Redaman Terkait Dengan?", "id": "Bagian Riil Dari Lokasi Pole Kompleks." },
  { "en": "Frekuensi Osilasi Terkait Dengan?", "id": "Bagian Imajiner Dari Lokasi Pole Kompleks." },
  { "en": "Apa Itu Transformasi Laplace Dua Sisi?", "id": "Sama Dengan Transformasi Bilateral Yang Sudah Disebut." },
  { "en": "Apa Itu Sifat Perkalian Dengan t?", "id": "Minus Turunan Pertama Dari Fungsi F(s)." },
  { "en": "Transformasi t dikali f(t)?", "id": "Hasilnya Adalah -d/ds Dari F(s)." },
  { "en": "Aplikasi Dalam Teori Kontrol?", "id": "Menganalisis Stabilitas Dan Kinerja Sistem Kontrol." },
  { "en": "Aplikasi Dalam Pemrosesan Sinyal?", "id": "Mendesain Dan Menganalisis Filter Analog." },
  { "en": "Apa Transformasi Dari f(t)/t?", "id": "Integral F(s) Dari s Hingga Tak Hingga." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Bagaimana Mendapatkan Respon Frekuensi?", "id": "Mengganti s Dengan j Omega Di Fungsi Transfer." },
  { "en": "Variabel Omega (ω) Mewakili Apa?", "id": "Frekuensi Sudut Dalam Radian Per Detik." },
  { "en": "Laplace Mengubah Kalkulus Menjadi?", "id": "Aljabar Sederhana Untuk Dipecahkan." },
  { "en": "Kondisi Awal Nol Berarti?", "id": "Sistem Dalam Keadaan Diam Sebelum Input." },
  { "en": "Apa Itu Persamaan Karakteristik?", "id": "Penyebut Dari Fungsi Transfer Disamakan Nol." },
  { "en": "Akar Persamaan Karakteristik?", "id": "Adalah Pole-Pole Dari Sistem Tersebut." },
  { "en": "Apa Itu Fungsi Jendela (Window Function)?", "id": "Fungsi Yang Bernilai Nol Di Luar Interval." },
  { "en": "Contoh Fungsi Jendela?", "id": "Fungsi Kotak Atau Rectangular Pulse Function." },
  { "en": "Transformasi Fungsi Kotak?", "id": "Kombinasi Dari Dua Fungsi Step Bergeser." },
  { "en": "Apakah Transformasi Laplace Linier?", "id": "Ya Transformasi Ini Memenuhi Sifat Linearitas." },
  { "en": "Bagaimana Tabel Laplace Dibuat?", "id": "Dengan Menghitung Integral Definisi Untuk Fungsi Umum." },
  { "en": "Apa Batasan Transformasi Laplace?", "id": "Tidak Bisa Untuk Sistem Non-Linier." },
  { "en": "Apa Itu Sistem Linier?", "id": "Sistem Yang Memenuhi Prinsip Superposisi." },
  { "en": "Prinsip Superposisi Adalah?", "id": "Respon Jumlah Input Adalah Jumlah Respon Individual." },
  { "en": "Apa Itu Sistem Invarian Waktu?", "id": "Perilaku Sistem Tidak Berubah Seiring Waktu." },
  { "en": "Laplace Digunakan Untuk Sistem?", "id": "Sistem Linier Dan Invarian Waktu (LTI)." },
  { "en": "Apa Beda Dengan Transformasi-Z?", "id": "Transformasi-Z Untuk Sinyal Dan Sistem Waktu Diskrit." },
  { "en": "Laplace Untuk Sistem Waktu?", "id": "Sistem Waktu Kontinu Atau Sistem Analog." },
  { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Yang Berulang Setiap Periode T." },
  { "en": "Bagaimana Transformasi Sinyal Periodik?", "id": "Transformasi Satu Periode Dibagi Faktor Tertentu." },
  { "en": "Faktor Pembagi Sinyal Periodik?", "id": "Satu Dikurangi e Pangkat -sT." },
  { "en": "Apa Itu Teorema Konvolusi Frekuensi?", "id": "Perkalian Di Waktu Menjadi Konvolusi Di Frekuensi." },
  { "en": "Kenapa Konvolusi Frekuensi Jarang Dipakai?", "id": "Karena Perhitungannya Lebih Rumit Daripada Perkalian." },
  { "en": "Apa Itu Fungsi Error (erf)?", "id": "Fungsi Khusus Terkait Dengan Probabilitas." },
  { "en": "Apakah Semua Fungsi Punya Transformasi?", "id": "Tidak Semua Fungsi Memenuhi Syarat Konvergensi." },
  { "en": "Contoh Fungsi Tanpa Transformasi?", "id": "Fungsi e Pangkat t Kuadrat." },
  { "en": "Mengapa Fungsi Itu Tidak Punya?", "id": "Karena Tumbuh Terlalu Cepat Melebihi Orde Eksponensial." },
  { "en": "Bagian Riil s (Sigma) Menentukan?", "id": "Tingkat Pertumbuhan Atau Peluruhan Eksponensial." },
  { "en": "Bagian Imajiner s (Omega) Menentukan?", "id": "Frekuensi Osilasi Dari Sinyal Tersebut." },
  { "en": "Jika Pole Di Titik Asal?", "id": "Sistem Memiliki Sifat Integrator Murni." },
  { "en": "Respon Sistem Integrator?", "id": "Outputnya Adalah Integral Dari Sinyal Input." },
  { "en": "Jika Zero Di Titik Asal?", "id": "Sistem Memiliki Sifat Diferensiator Murni." },
  { "en": "Respon Sistem Diferensiator?", "id": "Outputnya Adalah Turunan Dari Sinyal Input." },
  { "en": "Pole Ganda Di Sumbu Imajiner?", "id": "Menyebabkan Respon Tidak Stabil Amplitudo Tumbuh." },
  { "en": "Apa Itu Analisis Rangkaian AC?", "id": "Menggunakan Fasor Yang Merupakan Bagian Laplace." },
  { "en": "Apa Itu Solusi Homogen?", "id": "Respon Alami Sistem Tanpa Input Eksternal." },
  { "en": "Apa Itu Solusi Partikular?", "id": "Respon Paksa Sistem Akibat Adanya Input." },
  { "en": "Laplace Memberikan Solusi Apa?", "id": "Memberikan Solusi Total (Homogen Plus Partikular)." },
  { "en": "Apa Itu Fungsi Bessel?", "id": "Solusi Untuk Persamaan Diferensial Bessel Tertentu." },
  { "en": "Apa Notasi Lain f(t)?", "id": "Sinyal Dalam Domain Waktu Atau Time Domain." },
  { "en": "Apa Notasi Lain F(s)?", "id": "Sinyal Dalam Domain Frekuensi Atau s-Domain." },
  { "en": "Transformasi Laplace Adalah Transformasi?", "id": "Transformasi Integral Untuk Fungsi Waktu Kontinu." },
  { "en": "Apa Fungsi Kernel Transformasi?", "id": "Fungsi Eksponensial e Pangkat Negatif st." },
  { "en": "Apa Itu Analisis Bidang-s?", "id": "Menganalisis Sistem Menggunakan Posisi Pole Zero." },
  { "en": "Bagaimana Sifat Linearitas Membantu?", "id": "Memecah Fungsi Kompleks Menjadi Bagian Sederhana." },
  { "en": "Transformasi Dari t Dikalikan Cosinus?", "id": "Menggunakan Sifat Diferensiasi Dalam Frekuensi." },
  { "en": "Transformasi Dari t Dikalikan Sinus?", "id": "Juga Menggunakan Sifat Diferensiasi Dalam Frekuensi." },
  { "en": "Apa Itu Solusi Keadaan Nol?", "id": "Respon Sistem Dengan Kondisi Awal Nol." },
  { "en": "Apa Itu Solusi Input Nol?", "id": "Respon Sistem Hanya Karena Kondisi Awal." },
  { "en": "Solusi Lengkap Adalah Jumlah Dari?", "id": "Solusi Keadaan Nol Dan Solusi Input Nol." },
  { "en": "Kenapa Disebut Frekuensi Kompleks?", "id": "Karena Variabel s Terdiri Dari Bagian Riil Imajiner." },
  { "en": "Apa Peran Bagian Riil (Sigma)?", "id": "Menentukan Redaman Atau Pertumbuhan Amplitudo Sinyal." },
  { "en": "Apa Peran Bagian Imajiner (Omega)?", "id": "Menentukan Frekuensi Osilasi Sinyal sinusoidal." },
  { "en": "Jika Sigma Negatif?", "id": "Amplitudo Sinyal Akan Meredam Seiring Waktu." },
  { "en": "Jika Sigma Positif?", "id": "Amplitudo Sinyal Akan Tumbuh Tak Terbatas." },
  { "en": "Jika Sigma Nol?", "id": "Amplitudo Sinyal Tetap Konstan Tidak Berubah." },
  { "en": "Apa Itu Jalur Integrasi Balik?", "id": "Garis Vertikal Di Dalam Region Konvergensi." },
  { "en": "Nama Jalur Integrasi Balik?", "id": "Kontur Bromwich Atau Integral Bromwich." },
  { "en": "Apakah Integral Balik Sering Digunakan?", "id": "Tidak Biasanya Menggunakan Tabel Dan Fraksi Parsial." },
  { "en": "Apa Itu Fungsi Transfer Loop Terbuka?", "id": "Fungsi Transfer Tanpa Umpan Balik (Feedback)." },
  { "en": "Apa Itu Fungsi Transfer Loop Tertutup?", "id": "Fungsi Transfer Dengan Jalur Umpan Balik." },
  { "en": "Bagaimana Umpan Balik Mempengaruhi Pole?", "id": "Posisi Pole Loop Tertutup Akan Berubah." },
  { "en": "Tujuan Analisis Umpan Balik?", "id": "Menstabilkan Sistem Atau Memperbaiki Kinerja." },
  { "en": "Transformasi Balik Dari Konstanta K?", "id": "K Dikalikan Dengan Fungsi Impuls Delta." },
  { "en": "Bagaimana Membuktikan Sifat Transformasi?", "id": "Dengan Menerapkan Definisi Integral Transformasi Laplace." },
  { "en": "Apa Itu Fungsi Transfer All-Pass?", "id": "Fungsi Transfer Yang Hanya Mengubah Fasa." },
  { "en": "Magnitudo Fungsi All-Pass?", "id": "Selalu Bernilai Satu Untuk Semua Frekuensi." },
  { "en": "Hubungan Zero Dan Pole All-Pass?", "id": "Zero Adalah Cerminan Negatif Dari Pole." },
  { "en": "Apa Itu Sistem Minimum Phase?", "id": "Sistem Stabil Dengan Invers Yang Stabil." },
  { "en": "Ciri Sistem Minimum Phase?", "id": "Semua Pole Dan Zero Di Sisi Kiri." },
  { "en": "Apa Itu Sistem Non-Minimum Phase?", "id": "Sistem Dengan Zero Di Sisi Kanan." },
  { "en": "Pengaruh Zero Di Sisi Kanan?", "id": "Menyebabkan Respon Awal Yang Berlawanan Arah." },
  { "en": "Apa Itu Overshoot?", "id": "Respon Sistem Melebihi Nilai Akhir Sesaat." },
  { "en": "Overshoot Terkait Dengan?", "id": "Adanya Pole Kompleks Konjugat Dalam Sistem." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Dari Sepuluh Ke Sembilan Puluh Persen." },
  { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Hingga Respon Mencapai Setengah Nilai Akhir." },
  { "en": "Apa Itu Waktu Tunak (Settling Time)?", "id": "Waktu Hingga Respon Masuk Dalam Batas Toleransi." },
  { "en": "Parameter Kinerja Ini Diukur Dari?", "id": "Respon Step Dari Sistem Orde Dua." },
  { "en": "Apa Itu Redaman Kritis?", "id": "Kondisi Respon Paling Cepat Tanpa Overshoot." },
  { "en": "Apa Itu Underdamped?", "id": "Sistem Berosilasi Sebelum Mencapai Keadaan Tunak." },
  { "en": "Penyebab Underdamped?", "id": "Pole Berupa Pasangan Kompleks Konjugat." },
  { "en": "Apa Itu Overdamped?", "id": "Sistem Merespon Lambat Tanpa Adanya Osilasi." },
  { "en": "Penyebab Overdamped?", "id": "Pole Berupa Dua Akar Riil Berbeda." },
  { "en": "Penyebab Redaman Kritis?", "id": "Pole Berupa Dua Akar Riil Yang Sama." },
  { "en": "Apa Itu Rasio Redaman (Zeta)?", "id": "Parameter Yang Menentukan Tipe Redaman Sistem." },
  { "en": "Jika Zeta Antara Nol Dan Satu?", "id": "Sistem Underdamped Atau Kurang Teredam." },
  { "en": "Jika Zeta Sama Dengan Satu?", "id": "Sistem Teredam Kritis (Critically Damped)." },
  { "en": "Jika Zeta Lebih Dari Satu?", "id": "Sistem Teredam Lebih (Overdamped)." },
  { "en": "Jika Zeta Sama Dengan Nol?", "id": "Sistem Tidak Teredam Berosilasi Terus Menerus." },
  { "en": "Apa Itu Frekuensi Alami (Omega_n)?", "id": "Frekuensi Osilasi Sistem Tanpa Adanya Redaman." },
  { "en": "Hubungan Pole Dan Parameter?", "id": "Lokasi Pole Ditentukan Oleh Zeta Dan Omega_n." },
  { "en": "Transformasi Fungsi Tangga Satuan?", "id": "Sama Dengan Transformasi Fungsi Step (1/s)." },
  { "en": "Untuk Apa Analisis Respon Transien?", "id": "Melihat Perilaku Sistem Saat Terjadi Perubahan." },
  { "en": "Untuk Apa Analisis Keadaan Tunak?", "id": "Melihat Perilaku Sistem Setelah Lama Beroperasi." },
  { "en": "Bagian Transien Terkait Dengan?", "id": "Pole Dari Fungsi Transfer Sistem." },
  { "en": "Bagian Tunak Terkait Dengan?", "id": "Pole Dari Fungsi Input Yang Diberikan." },
  { "en": "Kapan Respon Transien Hilang?", "id": "Ketika Waktu Mendekati Tak Hingga Untuk Sistem Stabil." },
  { "en": "Contoh Sistem Orde Pertama?", "id": "Rangkaian RC Sederhana Atau Termometer." },
  { "en": "Fungsi Transfer Orde Pertama?", "id": "Memiliki Satu Pole Di Penyebutnya." },
  { "en": "Contoh Sistem Orde Kedua?", "id": "Rangkaian RLC Atau Sistem Pegas-Massa-Peredam." },
  { "en": "Fungsi Transfer Orde Kedua?", "id": "Memiliki Polinomial Orde Dua Di Penyebut." },
  { "en": "Adakah Transformasi Untuk Produk f(t)g(t)?", "id": "Ada Tapi Melibatkan Integral Konvolusi Kompleks." },
  { "en": "Apa Itu Sifat Dualitas?", "id": "Tidak Ada Sifat Dualitas Sederhana Seperti Fourier." },
  { "en": "Apa Akar Kata Laplace?", "id": "Nama Keluarga Matematikawan Pierre-Simon Laplace." },
  { "en": "Apakah Transformasi Ini Reversibel?", "id": "Ya Selalu Bisa Kembali Ke Domain Waktu." },
  { "en": "Bagaimana Jika Kondisi Awal Tak Nol?", "id": "Suku Tambahan Muncul Di Persamaan Aljabar." },
  { "en": "Laplace Mengubah Persamaan Integro-Diferensial?", "id": "Ya Menjadi Persamaan Aljabar Biasa." },
  { "en": "Apa Itu Persamaan Integro-Diferensial?", "id": "Persamaan Yang Mengandung Turunan Dan Integral." },
  { "en": "Apa Itu Domain Waktu Diskrit?", "id": "Sinyal Hanya Didefinisikan Pada Waktu Tertentu." },
  { "en": "Apa Itu Domain Waktu Kontinu?", "id": "Sinyal Didefinisikan Untuk Setiap Saat." },
  { "en": "Laplace Bekerja Di Domain?", "id": "Domain Waktu Kontinu Untuk Fungsi f(t)." },
  { "en": "Penyebut F(s) Disebut Juga?", "id": "Polinomial Karakteristik Dari Sistem Tersebut." },
  { "en": "Pembilang F(s) Berkaitan Dengan?", "id": "Bagaimana Input Dan Turunannya Mempengaruhi Sistem." },
  { "en": "Apa Itu Sistem SISO?", "id": "Single-Input Single-Output System." },
  { "en": "Laplace Umumnya Untuk Sistem?", "id": "Sistem SISO Linier Dan Invarian Waktu." },
  { "en": "Apa Itu Sistem MIMO?", "id": "Multiple-Input Multiple-Output System." },
  { "en": "Analisis Sistem MIMO?", "id": "Menggunakan Matriks Fungsi Transfer (State-Space)." },
  { "en": "Hubungan Dengan Ruang Keadaan?", "id": "Fungsi Transfer Bisa Diturunkan Dari Matriks State-Space." },
  { "en": "Apa Keuntungan Tabel Transformasi?", "id": "Menghindari Perhitungan Integral Yang Berulang-ulang." },
  { "en": "Kapan Teorema Nilai Akhir Gagal?", "id": "Jika Ada Pole Di Sisi Kanan." },
  { "en": "Kenapa Teorema Nilai Akhir Gagal?", "id": "Karena Sistem Tidak Pernah Mencapai Keadaan Tunak." },
  { "en": "Kapan Teorema Nilai Awal Berguna?", "id": "Memverifikasi Hasil Transformasi Tanpa Transformasi Balik." },
  { "en": "Efek Penambahan Pole?", "id": "Cenderung Membuat Sistem Merespon Lebih Lambat." },
  { "en": "Efek Penambahan Zero?", "id": "Cenderung Membuat Sistem Merespon Lebih Cepat." },
  { "en": "Zero Di Sisi Kiri Menyebabkan?", "id": "Overshoot Lebih Besar Pada Respon Sistem." },
  { "en": "Apa Itu Pembatalan Pole-Zero?", "id": "Ketika Pole Dan Zero Berada Di Lokasi Sama." },
  { "en": "Efek Pembatalan Pole-Zero?", "id": "Mode Terkait Pole Tersebut Tidak Terlihat." },
  { "en": "Apakah Pembatalan Selalu Sempurna?", "id": "Tidak Selalu Sempurna Dalam Sistem Nyata." },
  { "en": "Apa Itu Sistem Tak Kausal?", "id": "Output Mendahului Input (Tidak Fisik)." },
  { "en": "Apa Itu Sistem Anti Kausal?", "id": "Output Hanya Bergantung Pada Input Masa Depan." },
  { "en": "ROC Untuk Sistem Tak Kausal?", "id": "Berada Di Sebelah Kiri Pole Paling Kiri." },
  { "en": "ROC Untuk Sinyal Dua Sisi?", "id": "Berbentuk Cincin Antara Dua Pole." },
  { "en": "Apakah ROC Mencakup Pole?", "id": "Tidak ROC Tidak Pernah Mengandung Pole." },
  { "en": "Bagaimana Sifat Transformasi Diturunkan?", "id": "Berasal Langsung Dari Definisi Integral." },
  { "en": "Contoh Turunan Sifat Pergeseran?", "id": "Substitusi Variabel Langsung Ke Dalam Integral." },
  { "en": "Apa Itu Respons Natural?", "id": "Sama Dengan Solusi Input Nol." },
  { "en": "Apa Itu Respons Paksa?", "id": "Sama Dengan Solusi Keadaan Nol." },
  { "en": "Apa Fungsi Utama Bidang-s?", "id": "Alat Visual Untuk Menganalisis Perilaku Sistem." },
  { "en": "Bagaimana Cara Kerja Transformasi?", "id": "Memetakan Fungsi Dari Satu Domain Ke Domain Lain." },
  { "en": "Domain Asal Adalah?", "id": "Domain Waktu (t) Yang Berupa Riil." },
  { "en": "Domain Tujuan Adalah?", "id": "Domain Frekuensi (s) Yang Berupa Kompleks." },
  { "en": "Apa Sifat Turunan Di Domain Waktu?", "id": "Menjadi Perkalian Dengan s Di Domain Frekuensi." },
  { "en": "Apa Sifat Integral Di Domain Waktu?", "id": "Menjadi Pembagian Dengan s Di Domain Frekuensi." },
  { "en": "Apa Transformasi Dari Fungsi Delta Dirac?", "id": "Hasilnya Adalah Angka Satu (1)." },
  { "en": "Apa Transformasi Dari Fungsi Step Heaviside?", "id": "Hasilnya Adalah Satu Dibagi s (1/s)." },
  { "en": "Apa Arti Fisis Dari Pole?", "id": "Mode Alami Atau Frekuensi Resonansi Sistem." },
  { "en": "Apa Arti Fisis Dari Zero?", "id": "Frekuensi Dimana Respon Sistem Menjadi Nol." },
  { "en": "Apa Itu Sistem LTI?", "id": "Sistem Linier Invarian Waktu (Linear Time-Invariant)." },
  { "en": "Laplace Adalah Alat Untuk?", "id": "Menganalisis Sistem LTI Dengan Sangat Efektif." },
  { "en": "Apa Itu Kondisi Awal f(0)?", "id": "Nilai Fungsi Tepat Sebelum Waktu Nol." },
  { "en": "Mengapa Kondisi Awal Penting?", "id": "Menentukan Respon Lengkap Dari Suatu Sistem." },
  { "en": "Apa Transformasi Dari e Pangkat -at?", "id": "Satu Dibagi Dengan s Ditambah a." },
  { "en": "Apa Transformasi sin(ωt)?", "id": "Omega Dibagi s Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Apa Transformasi cos(ωt)?", "id": "s Dibagi s Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Teorema Nilai Awal Mencari?", "id": "Nilai f(t) Pada Saat t=0+." },
  { "en": "Teorema Nilai Akhir Mencari?", "id": "Nilai f(t) Saat t Mendekati Tak Hingga." },
  { "en": "Kapan Respon Sistem Stabil?", "id": "Ketika Semua Pole Di Bidang Kiri." },
  { "en": "Apa Artinya Bidang Kiri?", "id": "Bagian Riil Dari s Bernilai Negatif." },
  { "en": "Kapan Respon Sistem Tidak Stabil?", "id": "Jika Ada Pole Di Bidang Kanan." },
  { "en": "Apa Artinya Bidang Kanan?", "id": "Bagian Riil Dari s Bernilai Positif." },
  { "en": "Respon Karena Pole Di Sumbu Imajiner?", "id": "Menghasilkan Osilasi Yang Tak Pernah Berhenti." },
  { "en": "Bagaimana Mendapatkan Fungsi Transfer?", "id": "Transformasi Laplace Dari Persamaan Diferensial Sistem." },
  { "en": "Fungsi Transfer Adalah Rasio?", "id": "Transformasi Output Dibagi Transformasi Input." },
  { "en": "Syarat Fungsi Transfer?", "id": "Kondisi Awal Sistem Harus Bernilai Nol." },
  { "en": "Apa Nama Lain Fungsi Transfer?", "id": "Fungsi Sistem Atau Network Function." },
  { "en": "Apa Itu Konvolusi Di Domain Waktu?", "id": "Operasi Integral Yang Cukup Rumit." },
  { "en": "Konvolusi Di Waktu Menjadi Apa?", "id": "Perkalian Sederhana Di Domain Frekuensi." },
  { "en": "Sifat Ini Disebut Apa?", "id": "Teorema Konvolusi Yang Sangat Berguna." },
  { "en": "Perkalian Di Waktu Menjadi Apa?", "id": "Konvolusi Kompleks Di Domain Frekuensi." },
  { "en": "Bagaimana Sifat Pergeseran Waktu Membantu?", "id": "Menganalisis Sinyal Yang Mengalami Penundaan." },
  { "en": "Bentuk Sinyal Tunda?", "id": "f(t-a)u(t-a)." },
  { "en": "Transformasi Sinyal Tunda?", "id": "e Pangkat -as Dikalikan Dengan F(s)." },
  { "en": "Bagaimana Sifat Pergeseran Frekuensi Bekerja?", "id": "Mengalikan f(t) Dengan Sinyal Eksponensial." },
  { "en": "Bentuk Pengali Di Domain Waktu?", "id": "e Pangkat -at Dikalikan Dengan f(t)." },
  { "en": "Hasil Transformasinya Adalah?", "id": "Fungsi F(s) Bergeser Menjadi F(s+a)." },
  { "en": "Apa Itu Residu Pole?", "id": "Koefisien Yang Dihitung Untuk Fraksi Parsial." },
  { "en": "Metode Heaviside Untuk Apa?", "id": "Cara Cepat Menghitung Residu Untuk Pole Riil." },
  { "en": "Ekspansi Fraksi Parsial Untuk Apa?", "id": "Menyederhanakan F(s) Sebelum Transformasi Balik." },
  { "en": "Apa Itu Pole Sederhana?", "id": "Pole Yang Hanya Muncul Satu Kali." },
  { "en": "Apa Itu Pole Berulang?", "id": "Pole Yang Muncul Lebih Dari Satu Kali." },
  { "en": "Ekspansi Untuk Pole Berulang?", "id": "Memerlukan Beberapa Suku Dengan Pangkat Meningkat." },
  { "en": "Apa Itu Pole Kompleks Konjugat?", "id": "Sepasang Pole Dengan Bagian Imajiner Berlawanan." },
  { "en": "Hasil Transformasi Balik Pole Kompleks?", "id": "Fungsi Sinusoidal Teredam Dalam Domain Waktu." },
  { "en": "Bagian Riil Pole Kompleks Menentukan?", "id": "Tingkat Redaman Eksponensial Dari Osilasi." },
  { "en": "Bagian Imajiner Pole Kompleks Menentukan?", "id": "Frekuensi Osilasi Dari Sinyal Sinusoidal." },
  { "en": "Apa Itu Orde Polinomial?", "id": "Pangkat Tertinggi Dari Variabel Polinomial." },
  { "en": "Syarat Fraksi Parsial?", "id": "Orde Pembilang Harus Kurang Dari Penyebut." },
  { "en": "Jika Orde Tidak Memenuhi?", "id": "Lakukan Pembagian Polinomial Panjang Terlebih Dahulu." },
  { "en": "Sinyal Kausal Berarti?", "id": "Bernilai Nol Untuk Waktu t Negatif." },
  { "en": "Transformasi Unilateral Untuk Sinyal?", "id": "Sinyal Kausal Yang Umum Ditemui." },
  { "en": "Adakah Transformasi Untuk Logaritma?", "id": "Tidak Ada Bentuk Sederhana Tertutup." },
  { "en": "Apa Transformasi Dari Fungsi Periodik?", "id": "Melibatkan Transformasi Satu Periode Saja." },
  { "en": "Bagaimana Rumus Fungsi Periodik?", "id": "Transformasi Satu Periode Dibagi (1 - e^-sT)." },
  { "en": "Huruf T Mewakili Apa?", "id": "Periode Dari Sinyal Yang Berulang." },
  { "en": "Bagaimana Laplace Menganalisis Rangkaian?", "id": "Mengubah Komponen RLC Ke Impedansi-s." },
  { "en": "Impedansi-s Untuk Resistor?", "id": "Tetap R Tidak Berubah Sama Sekali." },
  { "en": "Impedansi-s Untuk Induktor?", "id": "Menjadi sL (Sudah Termasuk Kondisi Awal)." },
  { "en": "Impedansi-s Untuk Kapasitor?", "id": "Menjadi 1/sC (Sudah Termasuk Kondisi Awal)." },
  { "en": "Hukum Rangkaian Di Domain-s?", "id": "Hukum Ohm Dan Kirchhoff Tetap Berlaku." },
  { "en": "Analisis Rangkaian Menjadi?", "id": "Analisis Aljabar Sederhana Dengan Impedansi." },
  { "en": "Apa Itu Admittance?", "id": "Kebalikan Dari Impedansi (Y(s) = 1/Z(s))." },
  { "en": "Apa Itu Sistem Orde Nol?", "id": "Sistem Penguatan Konstan Tanpa Dinamika." },
  { "en": "Fungsi Transfer Sistem Orde Nol?", "id": "Hanya Berupa Konstanta Penguatan (Gain) K." },
  { "en": "Apa Itu Time Constant?", "id": "Ukuran Kecepatan Respon Sistem Orde Pertama." },
  { "en": "Hubungan Time Constant Dan Pole?", "id": "Kebalikan Dari Lokasi Pole Riil Negatif." },
  { "en": "Sistem Cepat Memiliki Time Constant?", "id": "Time Constant Yang Sangat Kecil." },
  { "en": "Sistem Lambat Memiliki Time Constant?", "id": "Time Constant Yang Sangat Besar." },
  { "en": "Berapa Lama Respon Mencapai 63%?", "id": "Satu Kali Nilai Time Constant." },
  { "en": "Berapa Lama Respon Mencapai Tunak?", "id": "Sekitar Lima Kali Nilai Time Constant." },
  { "en": "Frekuensi Sudut Teredam (Omega_d)?", "id": "Frekuensi Osilasi Sistem Orde Dua Underdamped." },
  { "en": "Hubungan Omega_d Dan Omega_n?", "id": "Selalu Lebih Kecil Dari Frekuensi Alami." },
  { "en": "Kapan Omega_d Sama Dengan Nol?", "id": "Saat Sistem Teredam Kritis (Zeta = 1)." },
  { "en": "Jika f(t) Genap?", "id": "Tidak Ada Penyederhanaan Khusus Untuk Laplace." },
  { "en": "Jika f(t) Ganjil?", "id": "Tidak Ada Penyederhanaan Khusus Untuk Laplace." },
  { "en": "Apakah Hasil F(s) Umumnya Riil?", "id": "Tidak F(s) Adalah Fungsi Bernilai Kompleks." },
  { "en": "Apa Itu Magnitudo F(s)?", "id": "Besar Atau Amplitudo Dari Bilangan Kompleks." },
  { "en": "Apa Itu Fasa F(s)?", "id": "Sudut Dari Bilangan Kompleks Tersebut." },
  { "en": "Magnitudo Dan Fasa Membentuk?", "id": "Respon Frekuensi Ketika s = j Omega." },
  { "en": "Apa Itu Plot Bode?", "id": "Plot Magnitudo Dan Fasa Respon Frekuensi." },
  { "en": "Plot Bode Berguna Untuk?", "id": "Menganalisis Stabilitas Dan Kinerja Sistem Kontrol." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Seberapa Jauh Sistem Dari Ketidakstabilan." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Lain Untuk Kestabilan Relatif Sistem." },
  { "en": "Analisis Stabilitas Routh-Hurwitz?", "id": "Metode Tanpa Mencari Akar Polinomial Karakteristik." },
  { "en": "Routh-Hurwitz Menganalisis Apa?", "id": "Koefisien Polinomial Untuk Cek Stabilitas." },
  { "en": "Apa Itu Root Locus?", "id": "Plot Pergerakan Pole Loop Tertutup." },
  { "en": "Root Locus Diplot Terhadap?", "id": "Perubahan Penguatan Sistem (Gain K)." },
  { "en": "Tujuan Root Locus?", "id": "Mendesain Kontroler Untuk Performa Yang Diinginkan." },
  { "en": "Apa Transformasi Dari Turunan ke-n?", "id": "Melibatkan Pangkat s Sampai n Dan Kondisi Awal." },
  { "en": "Apa Itu Fungsi Gamma?", "id": "Perampatan Dari Fungsi Faktorial Untuk Bilangan Riil." },
  { "en": "Hubungan Dengan Transformasi t Pangkat a?", "id": "Hasilnya Melibatkan Fungsi Gamma Dari a+1." },
  { "en": "Apakah Laplace Operator Injeksi?", "id": "Ya Setiap f(t) Punya F(s) Unik." },
  { "en": "Apakah Laplace Operator Surjeksi?", "id": "Tidak Semua Fungsi F(s) Punya Transformasi Balik." },
  { "en": "Contoh F(s) Tanpa Transformasi Balik?", "id": "Fungsi Yang Tidak Mendekati Nol Saat s Besar." },
  { "en": "Apa Itu Final Value Theorem?", "id": "Sama Dengan Teorema Nilai Akhir." },
  { "en": "Apa Nama Lengkap Transformasi Laplace?", "id": "Transformasi Integral Pierre-Simon Laplace." },
  { "en": "Domain Waktu Sering Disebut?", "id": "Ruang Waktu Atau Time Space." },
  { "en": "Domain Frekuensi Sering Disebut?", "id": "Ruang Frekuensi Atau s-Space." },
  { "en": "Variabel s Adalah Variabel?", "id": "Variabel Kompleks Dummy Untuk Analisis Matematis." },
  { "en": "Apa Syarat Konvergensi Integral?", "id": "Integral Harus Menghasilkan Nilai Yang Terbatas." },
  { "en": "Apa Itu Transformasi Invers?", "id": "Sama Dengan Transformasi Balik (Inverse Transform)." },
  { "en": "Notasi Untuk Transformasi Invers?", "id": "L Pangkat Minus Satu Kurung F(s)." },
  { "en": "Apa Sifat Paling Berguna?", "id": "Mengubah Operasi Kalkulus Menjadi Operasi Aljabar." },
  { "en": "Sifat Turunan Memerlukan Apa?", "id": "Informasi Mengenai Kondisi Awal Sistem." },
  { "en": "Apa Yang Terjadi Pada Kondisi Awal?", "id": "Menjadi Bagian Dari Persamaan Aljabar." },
  { "en": "Apa Transformasi Dari u(t)?", "id": "Satu Dibagi Dengan Variabel s (1/s)." },
  { "en": "Apa Transformasi Dari δ(t)?", "id": "Konstanta Satu (1) Tanpa Unit." },
  { "en": "Apa Transformasi Dari Fungsi Ramp r(t)?", "id": "Satu Dibagi s Kuadrat (1/s²)." },
  { "en": "Apa Hubungan Fungsi-Fungsi Ini?", "id": "Ramp Adalah Integral Step Adalah Integral Impuls." },
  { "en": "Hubungan Turunannya?", "id": "Step Turunan Ramp Impuls Turunan Step." },
  { "en": "Transformasi e Pangkat at Dikalikan sin(ωt)?", "id": "Menggunakan Sifat Pergeseran Dalam Frekuensi." },
  { "en": "Hasil Transformasi e Pangkat at sin(ωt)?", "id": "Omega Dibagi (s-a) Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Transformasi e Pangkat at Dikalikan cos(ωt)?", "id": "Juga Menggunakan Sifat Pergeseran Dalam Frekuensi." },
  { "en": "Hasil Transformasi e Pangkat at cos(ωt)?", "id": "s-a Dibagi (s-a) Kuadrat Ditambah Omega Kuadrat." },
  { "en": "Apa Itu Persamaan Diferensial Homogen?", "id": "Persamaan Dengan Sisi Kanan Sama Dengan Nol." },
  { "en": "Apa Itu Persamaan Diferensial Non-Homogen?", "id": "Persamaan Dengan Fungsi Input Di Sisi Kanan." },
  { "en": "Laplace Menyelesaikan Jenis Persamaan Apa?", "id": "Keduanya Homogen Dan Juga Non-Homogen." },
  { "en": "Apa Itu Akar Karakteristik?", "id": "Sama Dengan Pole Dari Fungsi Transfer." },
  { "en": "Akar Karakteristik Menentukan?", "id": "Respon Natural Atau Respon Transien Sistem." },
  { "en": "Fungsi Input Menentukan?", "id": "Respon Paksa Atau Respon Keadaan Tunak." },
  { "en": "Apa Itu Respon Total?", "id": "Jumlah Dari Respon Natural Dan Respon Paksa." },
  { "en": "Bagaimana Laplace Memberikan Respon Total?", "id": "Secara Otomatis Dalam Satu Kali Perhitungan." },
  { "en": "Apa Itu Osilasi Teredam?", "id": "Osilasi Yang Amplitudonya Mengecil Seiring Waktu." },
  { "en": "Osilasi Teredam Disebabkan Oleh?", "id": "Pole Kompleks Di Sisi Kiri Bidang-s." },
  { "en": "Apa Itu Pertumbuhan Eksponensial?", "id": "Sinyal Yang Amplitudonya Membesar Tanpa Batas." },
  { "en": "Pertumbuhan Eksponensial Disebabkan Oleh?", "id": "Pole Riil Di Sisi Kanan Bidang-s." },
  { "en": "Apa Itu Diagram Blok?", "id": "Representasi Grafis Dari Fungsi-Fungsi Sistem." },
  { "en": "Fungsi Transfer Dalam Diagram Blok?", "id": "Ditulis Di Dalam Kotak Atau Blok." },
  { "en": "Apa Itu Titik Penjumlahan?", "id": "Lingkaran Yang Menjumlahkan Atau Mengurangkan Sinyal." },
  { "en": "Apa Itu Titik Percabangan?", "id": "Titik Dimana Sinyal Dibagi Ke Beberapa Jalur." },
  { "en": "Diagram Blok Berguna Untuk?", "id": "Menganalisis Sistem Kompleks Dengan Umpan Balik." },
  { "en": "Apa Itu Metode Fraksi Parsial?", "id": "Teknik Aljabar Untuk Memecah Fungsi Rasional." },
  { "en": "Tujuan Metode Ini?", "id": "Agar Setiap Suku Mudah Dicari Transformasi Baliknya." },
  { "en": "Apa Itu Koefisien Fraksi Parsial?", "id": "Sama Dengan Residu Yang Dicari Nilainya." },
  { "en": "Bagaimana Mencari Koefisien Pole Kompleks?", "id": "Bisa Menggunakan Aljabar Kompleks Atau Metode Lain." },
  { "en": "Apa Itu Respon Frekuensi H(jω)?", "id": "Fungsi Transfer H(s) Dengan s=jω." },
  { "en": "Respon Frekuensi Memberikan Informasi?", "id": "Bagaimana Sistem Merespon Input Sinusoidal." },
  { "en": "Magnitudo Respon Frekuensi?", "id": "Perbandingan Amplitudo Output Terhadap Input." },
  { "en": "Fasa Respon Frekuensi?", "id": "Pergeseran Fasa Antara Sinyal Output Dan Input." },
  { "en": "Jika Pole Dekat Sumbu jω?", "id": "Akan Terjadi Puncak Resonansi Pada Respon Frekuensi." },
  { "en": "Apa Itu Bandwidth Sistem?", "id": "Rentang Frekuensi Dimana Sistem Bekerja Efektif." },
  { "en": "Definisi Umum Bandwidth?", "id": "Frekuensi Saat Gain Turun Tiga Desibel." },
  { "en": "Sistem Orde Lebih Tinggi?", "id": "Memiliki Lebih Dari Dua Pole Di Penyebut." },
  { "en": "Respon Sistem Orde Tinggi?", "id": "Bisa Diaproksimasi Oleh Pole-Pole Dominan." },
  { "en": "Apa Syarat Aproksimasi Pole Dominan?", "id": "Pole Lain Harus Jauh Di Kiri." },
  { "en": "Kenapa Pole Jauh Di Kiri?", "id": "Karena Responnya Meredam Jauh Lebih Cepat." },
  { "en": "Sifat Diferensiasi Terhadap Parameter?", "id": "Teknik Untuk Mencari Transformasi Bentuk Tertentu." },
  { "en": "Bagaimana Teorema Nilai Akhir Bekerja?", "id": "Menghubungkan Perilaku Asimtotik Di Dua Domain." },
  { "en": "Perilaku s Mendekati Nol?", "id": "Berhubungan Dengan Perilaku t Mendekati Tak Hingga." },
  { "en": "Bagaimana Teorema Nilai Awal Bekerja?", "id": "Juga Menghubungkan Perilaku Asimtotik Di Dua Domain." },
  { "en": "Perilaku s Mendekati Tak Hingga?", "id": "Berhubungan Dengan Perilaku t Mendekati Nol." },
  { "en": "Apakah Sifat-Sifat Ini Selalu Berlaku?", "id": "Hanya Jika Syarat-Syarat Tertentu Terpenuhi." },
  { "en": "ROC Untuk Transformasi Bilateral?", "id": "Penting Untuk Menentukan Jenis Sinyal." },
  { "en": "ROC Mendefinisikan Apa?", "id": "Rentang s Dimana Transformasi Laplace Ada." },
  { "en": "ROC Untuk Sinyal Sisi Kanan?", "id": "Setengah Bidang Di Sebelah Kanan Pole." },
  { "en": "ROC Untuk Sinyal Sisi Kiri?", "id": "Setengah Bidang Di Sebelah Kiri Pole." },
  { "en": "Analisis Rangkaian Dengan Sumber Tak Nol?", "id": "Sumber Tegangan Atau Arus Awal Diperlakukan." },
  { "en": "Bagaimana Sumber Awal Diperlakukan?", "id": "Sebagai Sumber Tambahan Di Domain-s." },
  { "en": "Sumber Awal Untuk Kapasitor?", "id": "Sumber Tegangan V(0)/s Secara Seri." },
  { "en": "Sumber Awal Untuk Induktor?", "id": "Sumber Arus I(0)/s Secara Paralel." },
  { "en": "Metode Ini Menyederhanakan?", "id": "Analisis Rangkaian Dengan Kondisi Awal." },
  { "en": "Apa Itu Jaringan Dua Port?", "id": "Jaringan Listrik Dengan Satu Input Satu Output." },
  { "en": "Analisis Jaringan Dua Port?", "id": "Menggunakan Parameter-z -y -h atau -ABCD." },
  { "en": "Parameter Ini Didefinisikan Di?", "id": "Domain Frekuensi-s Setelah Transformasi Laplace." },
  { "en": "Apa Itu Persamaan Keadaan (State-Space)?", "id": "Representasi Sistem Menggunakan Variabel Keadaan." },
  { "en": "Keuntungan Persamaan Keadaan?", "id": "Bisa Untuk Sistem MIMO Dan Non-Linier." },
  { "en": "Bagaimana Hubungan Dengan Fungsi Transfer?", "id": "Fungsi Transfer Bisa Dicari Dari Matriks Keadaan." },
  { "en": "Rumus Fungsi Transfer Dari State-Space?", "id": "C Kali (sI-A) Invers Kali B Ditambah D." },
  { "en": "A B C D Adalah?", "id": "Matriks-Matriks Yang Mendefinisikan Sistem Dinamis." },
  { "en": "Apa Itu Matriks A?", "id": "Matriks Sistem Yang Menentukan Lokasi Pole." },
  { "en": "Nilai Eigen Matriks A?", "id": "Sama Dengan Pole-Pole Dari Fungsi Transfer." },
  { "en": "Apa Itu Sinyal Eksponensial Kompleks?", "id": "e Pangkat st Adalah Basisnya Laplace." },
  { "en": "Kenapa Eksponensial Kompleks Digunakan?", "id": "Merupakan Fungsi Eigen Dari Sistem LTI." },
  { "en": "Apa Artinya Fungsi Eigen?", "id": "Bentuk Fungsi Tidak Berubah Setelah Melewati Sistem." },
  { "en": "Apa Yang Berubah?", "id": "Hanya Amplitudo Dan Fasa Yang Berubah." },
  { "en": "Perubahan Ini Dinyatakan Oleh?", "id": "Nilai Fungsi Transfer Pada Frekuensi Itu." },
  { "en": "Apakah Laplace Satu-Satunya Transformasi?", "id": "Tidak Ada Fourier Mellin Z-Transform Dll." },
  { "en": "Kenapa Laplace Sangat Populer Di Teknik?", "id": "Sangat Efektif Untuk Masalah Nilai Awal." },
  { "en": "Contoh Masalah Nilai Awal?", "id": "Menyelesaikan Persamaan Diferensial Dengan Kondisi Awal." },
  { "en": "Apa Itu Persamaan Beda (Difference Equation)?", "id": "Persamaan Untuk Sistem Waktu Diskrit." },
  { "en": "Alat Untuk Menyelesaikan Persamaan Beda?", "id": "Menggunakan Transformasi-Z Yang Mirip Laplace." },
  { "en": "Apa Itu Sumbu Imajiner?", "id": "Garis Vertikal Dimana Bagian Riil s Nol." },
  { "en": "Sumbu Imajiner Memisahkan Apa?", "id": "Daerah Stabil Dan Daerah Tidak Stabil." },
  { "en": "Apa Itu Setengah Bidang Kiri Terbuka?", "id": "Daerah Re(s) < 0 Tidak Termasuk Sumbu." },
  { "en": "Apa Itu Setengah Bidang Kiri Tertutup?", "id": "Daerah Re(s) <= 0 Termasuk Sumbu Imajiner." },
  { "en": "Syarat Stabilitas Asimtotik?", "id": "Semua Pole Di Setengah Bidang Kiri Terbuka." },
  { "en": "Apa Itu Stabilitas BIBO?", "id": "Bounded-Input Bounded-Output Stability." },
  { "en": "Arti Stabilitas BIBO?", "id": "Input Terbatas Menghasilkan Output Yang Terbatas." },
  { "en": "Syarat Stabilitas BIBO?", "id": "Sama Dengan Stabilitas Asimtotik Untuk Sistem LTI." },
  { "en": "Sistem Dengan Pole Di Sumbu Imajiner?", "id": "Tidak Stabil Secara Asimtotik Tapi Mungkin BIBO." },
  { "en": "Kapan Pole Di Sumbu Imajiner Stabil BIBO?", "id": "Jika Pole Tidak Berulang (Sederhana)." },
  { "en": "Apa Itu Respon Osilasi Tumbuh?", "id": "Disebabkan Pole Kompleks Di Sisi Kanan." },
  { "en": "Apa Itu Titik Cabang (Branch Point)?", "id": "Titik Singularitas Untuk Fungsi Bernilai Banyak." },
  { "en": "Laplace Berguna Untuk Sistem Deterministik?", "id": "Ya Sistem Yang Perilakunya Dapat Diprediksi." },
  { "en": "Bagaimana Dengan Sistem Stokastik?", "id": "Analisis Memerlukan Metode Probabilistik Yang Berbeda." },
  { "en": "Apa Nama Lain Sumbu Riil Bidang-s?", "id": "Sumbu Neper Atau Sumbu Redaman Sistem." },
  { "en": "Apa Nama Lain Sumbu Imajiner?", "id": "Sumbu Frekuensi Riil Atau Frekuensi Sudut." },
  { "en": "Transformasi Laplace Adalah Generalisasi Dari?", "id": "Transformasi Fourier Untuk Analisis Sistem Dinamis." },
  { "en": "Apa Yang Diwakili Oleh Pole?", "id": "Mode Respon Natural Dari Suatu Sistem." },
  { "en": "Apa Yang Diwakili Oleh Zero?", "id": "Mode Pemblokiran Sinyal Pada Frekuensi Tertentu." },
  { "en": "Bagaimana Zero Memblokir Sinyal?", "id": "Membuat Gain Sistem Menjadi Nol." },
  { "en": "Apa Itu Fungsi Transfer Unit Feedback?", "id": "Sistem Umpan Balik Dengan H(s) = 1." },
  { "en": "Fungsi Transfer Loop Tertutupnya?", "id": "G(s) Dibagi Satu Ditambah G(s)." },
  { "en": "Apa Itu Gain DC Sistem?", "id": "Gain Sistem Pada Frekuensi Nol." },
  { "en": "Bagaimana Menghitung Gain DC?", "id": "Mengevaluasi H(s) Pada Saat s=0." },
  { "en": "Apa Arti Gain DC Tak Hingga?", "id": "Sistem Memiliki Integrator (Pole Di Nol)." },
  { "en": "Apa Arti Gain DC Nol?", "id": "Sistem Memiliki Diferensiator (Zero Di Nol)." },
  { "en": "Sifat Penskalaan Dalam Domain-s?", "id": "Tidak Ada Sifat Penskalaan Sederhana." },
  { "en": "Apa Transformasi Dari u(t)-u(t-a)?", "id": "Mewakili Sebuah Pulsa Persegi (Rectangular Pulse)." },
  { "en": "Hasil Transformasi Pulsa Persegi?", "id": "Satu Dikurang e Pangkat -as Dibagi s." },
  { "en": "Apa Itu Teorema Pergeseran Kompleks?", "id": "Sama Dengan Sifat Pergeseran Dalam Frekuensi." },
  { "en": "Apa Itu Teorema Pergeseran Riil?", "id": "Sama Dengan Sifat Pergeseran Dalam Waktu." },
  { "en": "Apakah Kondisi Awal Mempengaruhi Stabilitas?", "id": "Tidak Stabilitas Hanya Tergantung Posisi Pole." },
  { "en": "Kondisi Awal Mempengaruhi Apa?", "id": "Hanya Amplitudo Dari Respon Total Sistem." },
  { "en": "Dapatkah Sistem Stabil Punya Respon Tak Terbatas?", "id": "Ya Jika Diberi Input Yang Tak Terbatas." },
  { "en": "Dapatkah Sistem Tidak Stabil Punya Respon Terbatas?", "id": "Ya Untuk Input Tertentu Yang Membatalkan Pole." },
  { "en": "Apa Itu Pembatalan Pole-Zero Tak Sempurna?", "id": "Pole Dan Zero Sangat Dekat Tapi Tidak Sama." },
  { "en": "Efek Pembatalan Tak Sempurna?", "id": "Menghasilkan Respon Lambat Dengan Amplitudo Kecil." },
  { "en": "Apa Transformasi Dari Fungsi Tangen?", "id": "Tidak Ada Bentuk Sederhana Yang Tertutup." },
  { "en": "Apa Itu Analisis Sirkuit Transien?", "id": "Menganalisis Perilaku Rangkaian Sesaat Setelah Switching." },
  { "en": "Kenapa Laplace Cocok Untuk Transien?", "id": "Karena Secara Alami Memasukkan Kondisi Awal." },
  { "en": "Apa Itu Analisis Sirkuit AC Steady State?", "id": "Menganalisis Rangkaian Setelah Semua Transien Hilang." },
  { "en": "Alat Untuk Analisis AC Steady State?", "id": "Metode Fasor Yang Jauh Lebih Sederhana." },
  { "en": "Fasor Adalah Representasi?", "id": "Sinusoid Sebagai Bilangan Kompleks Statis." },
  { "en": "Hubungan Fasor Dan Laplace?", "id": "Fasor Adalah Kasus Khusus Dari Laplace." },
  { "en": "Apa Itu Error Keadaan Tunak?", "id": "Perbedaan Antara Input Dan Output Referensi." },
  { "en": "Kapan Error Keadaan Tunak Dihitung?", "id": "Saat Waktu Mendekati Tak Hingga." },
  { "en": "Bagaimana Menghitung Error Keadaan Tunak?", "id": "Menggunakan Teorema Nilai Akhir." },
  { "en": "Tipe Sistem (Type Number)?", "id": "Jumlah Pole Di Titik Asal Loop Terbuka." },
  { "en": "Tipe Sistem Menentukan?", "id": "Kemampuan Sistem Melacak Sinyal Input Tertentu." },
  { "en": "Sistem Tipe 0 Punya Error Untuk?", "id": "Input Step Dengan Error Yang Konstan." },
  { "en": "Sistem Tipe 1 Punya Error Untuk?", "id": "Input Ramp Dengan Error Yang Konstan." },
  { "en": "Sistem Tipe 2 Punya Error Untuk?", "id": "Input Parabola Dengan Error Yang Konstan." },
  { "en": "Sistem Tipe 1 Melacak Step?", "id": "Dengan Error Keadaan Tunak Sama Dengan Nol." },
  { "en": "Sistem Tipe 2 Melacak Ramp?", "id": "Dengan Error Keadaan Tunak Sama Dengan Nol." },
  { "en": "Bagaimana Laplace Menangani Sinyal Terputus?", "id": "Direpresentasikan Sebagai Kombinasi Fungsi Step." },
  { "en": "Apa Itu Fungsi Gigi Gergaji (Sawtooth)?", "id": "Sinyal Periodik Yang Naik Linier Lalu Jatuh." },
  { "en": "Bagaimana Transformasi Fungsi Gigi Gergaji?", "id": "Menggunakan Rumus Untuk Sinyal Periodik." },
  { "en": "Apa Itu Fungsi Gelombang Segitiga?", "id": "Sinyal Periodik Yang Naik Dan Turun Linier." },
  { "en": "Apa Itu Fungsi Gelombang Kotak?", "id": "Sinyal Periodik Yang Bernilai Tinggi Dan Rendah." },
  { "en": "Deret Fourier Terkait Dengan?", "id": "Dekomposisi Sinyal Periodik Menjadi Sinusoid." },
  { "en": "Hubungan Deret Fourier Dan Laplace?", "id": "Koefisien Deret Fourier Adalah Residu Pole." },
  { "en": "Pole Terletak Di Mana?", "id": "Di Sumbu Imajiner Pada Frekuensi Harmonik." },
  { "en": "Apa Itu Initial Condition Theorem?", "id": "Sama Dengan Teorema Nilai Awal." },
  { "en": "Apa Arti Fisis Teorema Konvolusi?", "id": "Respon Sistem Adalah Konvolusi Input Dan Respon Impuls." },
  { "en": "Respon Impuls Adalah?", "id": "Karakteristik Murni Dari Sistem LTI." },
  { "en": "Notasi Respon Impuls?", "id": "Biasanya Ditulis Sebagai h(t) Di Domain Waktu." },
  { "en": "Transformasi h(t) Adalah?", "id": "Fungsi Transfer Sistem H(s)." },
  { "en": "Apa Itu Dekonvolusi?", "id": "Proses Untuk Mencari Input Dari Output." },
  { "en": "Bagaimana Dekonvolusi Dilakukan Di Domain-s?", "id": "Pembagian Sederhana X(s) = Y(s) / H(s)." },
  { "en": "Apa Itu Filter Ideal?", "id": "Filter Yang Melewatkan Frekuensi Tertentu Sempurna." },
  { "en": "Apakah Filter Ideal Bisa Dibuat?", "id": "Tidak Karena Filter Ideal Tidak Kausal." },
  { "en": "Contoh Filter Ideal?", "id": "Filter Low-Pass Ideal Dengan Batas Tajam." },
  { "en": "Respon Impuls Filter Ideal?", "id": "Berbentuk Fungsi Sinc Yang Non-Kausal." },
  { "en": "Apa Itu Sistem Aljabar Diferensial?", "id": "Gabungan Persamaan Diferensial Dan Persamaan Aljabar." },
  { "en": "Laplace Bisa Menyelesaikan Sistem Ini?", "id": "Ya Dengan Mentransformasi Seluruh Sistem Persamaan." },
  { "en": "Apa Itu Transformasi Laplace Fraksional?", "id": "Generalisasi Untuk Orde Turunan Non-Integer." },
  { "en": "Apa Itu Konstanta Gain K?", "id": "Faktor Pengali Pada Fungsi Transfer." },
  { "en": "Efek Menaikkan Gain K?", "id": "Umumnya Membuat Sistem Lebih Cepat Tapi Kurang Stabil." },
  { "en": "Berapa Jumlah Pole Sistem?", "id": "Sama Dengan Orde Dari Sistem Tersebut." },
  { "en": "Berapa Jumlah Zero Sistem?", "id": "Bisa Berapa Saja Tidak Tergantung Orde." },
  { "en": "Apa Itu Sistem Proper?", "id": "Jumlah Pole Lebih Besar Sama Dengan Zero." },
  { "en": "Apa Itu Sistem Strictly Proper?", "id": "Jumlah Pole Lebih Besar Dari Jumlah Zero." },
  { "en": "Sistem Fisik Biasanya Adalah?", "id": "Sistem Strictly Proper Karena Ada Inersia." },
  { "en": "Efek Zero Di Kanan Pada Respon Step?", "id": "Menyebabkan Undershoot Awal Sebelum Naik." },
  { "en": "Nama Lain Undershoot?", "id": "Initial Inverse Response Atau Respon Balik Awal." },
  { "en": "Apa Itu Waktu Tunda Transportasi?", "id": "Penundaan Waktu Murni Akibat Jarak Fisik." },
  { "en": "Model Matematika Waktu Tunda?", "id": "Operator e Pangkat Minus s Kali Tunda." },
  { "en": "Efek Waktu Tunda Pada Stabilitas?", "id": "Sangat Mengurangi Stabilitas Sistem Umpan Balik." },
  { "en": "Apa Itu Aproksimasi Padé?", "id": "Aproksimasi Fungsi Tunda Menjadi Fungsi Rasional." },
  { "en": "Tujuan Aproksimasi Padé?", "id": "Memudahkan Analisis Menggunakan Metode Klasik." },
  { "en": "Apakah Laplace Bekerja Untuk Vektor?", "id": "Ya Bisa Diterapkan Elemen Demi Elemen." },
  { "en": "Apakah Laplace Bekerja Untuk Matriks?", "id": "Ya Sama Diterapkan Elemen Demi Elemen." },
  { "en": "Transformasi Balik Dari 1?", "id": "Fungsi Impuls Dirac δ(t)." },
  { "en": "Transformasi Balik Dari s?", "id": "Turunan Dari Fungsi Impuls Dirac." },
  { "en": "Apa Itu Fungsi Singularity?", "id": "Keluarga Fungsi Seperti Impuls Step Ramp." },
  { "en": "Apa Itu Fungsi Doublet?", "id": "Turunan Dari Fungsi Impuls Dirac." },
  { "en": "Apa Itu Respon Memori Sistem?", "id": "Respon Impuls h(t) Mencirikan Memori Sistem." },
  { "en": "Sistem Tanpa Memori (Memoryless)?", "id": "Output Hanya Bergantung Pada Input Saat Ini." },
  { "en": "Respon Impuls Sistem Tanpa Memori?", "id": "h(t) Berupa Impuls Dikalikan Konstanta." },
  { "en": "Apa Itu Sifat Kausalitas?", "id": "Output Tidak Bergantung Pada Input Masa Depan." },
  { "en": "Syarat Kausalitas Untuk h(t)?", "id": "h(t) Harus Nol Untuk t Negatif." },
  { "en": "Apa Itu Solusi Unik?", "id": "Hanya Ada Satu Solusi Untuk Masalah Tertentu." },
  { "en": "Laplace Memberikan Solusi Unik?", "id": "Ya Jika ROC Didefinisikan Dengan Benar." },
  { "en": "Untuk Sistem Kausal ROC Selalu?", "id": "Menuju Ke Arah Kanan Tak Hingga." },
  { "en": "Apa Itu Teknik Kontrol Klasik?", "id": "Menggunakan Fungsi Transfer Dan Analisis Frekuensi." },
  { "en": "Apa Itu Teknik Kontrol Modern?", "id": "Menggunakan Representasi Ruang Keadaan (State-Space)." },
  { "en": "Laplace Adalah Fondasi Untuk?", "id": "Teknik Kontrol Klasik Yang Banyak Dipakai." },
  { "en": "Apa Itu Plot Nyquist?", "id": "Plot Respon Frekuensi Di Bidang Kompleks." },
  { "en": "Kriteria Stabilitas Nyquist?", "id": "Menentukan Stabilitas Loop Tertutup Dari Plot Loop Terbuka." },
  { "en": "Kelebihan Kriteria Nyquist?", "id": "Bisa Untuk Sistem Dengan Waktu Tunda." },
  { "en": "Apa Arti Integral Konvergen?", "id": "Luas Di Bawah Kurva Memiliki Nilai Terbatas." },
  { "en": "Apa Arti Integral Divergen?", "id": "Luas Di Bawah Kurva Menuju Tak Hingga." },
  { "en": "Laplace Membutuhkan Integral Yang?", "id": "Konvergen Agar Hasilnya Dapat Didefinisikan." },
  { "en": "Apa Itu Domain Transformasi?", "id": "Domain Tujuan Hasil Transformasi (Domain-s)." },
  { "en": "Apa Nama Lain Sifat Superposisi?", "id": "Sifat Linearitas Dari Transformasi Laplace." },
  { "en": "Transformasi Dari Fungsi Tangga Bergeser?", "id": "e Pangkat -as Dibagi Dengan s." },
  { "en": "Bentuk Fungsi Tangga Bergeser?", "id": "Fungsi u(t-a) Dalam Domain Waktu." },
  { "en": "Apa Sifat Turunan Di Domain Frekuensi?", "id": "Perkalian Dengan -t Di Domain Waktu." },
  { "en": "Apa Sifat Integral Di Domain Frekuensi?", "id": "Pembagian Dengan t Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Sisi Kanan?", "id": "Sinyal Yang Nol Untuk t < T." },
  { "en": "Sinyal Kausal Adalah Sinyal?", "id": "Sinyal Sisi Kanan Dengan T=0." },
  { "en": "Apa Itu Sinyal Sisi Kiri?", "id": "Sinyal Yang Nol Untuk t > T." },
  { "en": "Apa Itu Sinyal Durasi Terbatas?", "id": "Sinyal Nol Di Luar Interval Waktu." },
  { "en": "ROC Sinyal Durasi Terbatas?", "id": "Mencakup Seluruh Bidang-s Kecuali Mungkin s=0." },
  { "en": "ROC Untuk Sinyal Sisi Kanan?", "id": "Setengah Bidang Kanan Dari Pole Terluar." },
  { "en": "ROC Untuk Sinyal Sisi Kiri?", "id": "Setengah Bidang Kiri Dari Pole Terdalam." },
  { "en": "Bagaimana ROC Sinyal Gabungan?", "id": "Irisan Dari Masing-Masing ROC Sinyal." },
  { "en": "Apakah ROC Bisa Kosong?", "id": "Ya Jika Tidak Ada Irisan Antar ROC." },
  { "en": "Apa Arti ROC Kosong?", "id": "Sinyal Gabungan Tidak Memiliki Transformasi Laplace." },
  { "en": "Apa Arti Fisis Sumbu Imajiner?", "id": "Mewakili Frekuensi Sinusoidal Murni Tanpa Redaman." },
  { "en": "Apa Arti Titik Asal (Origin)?", "id": "Mewakili Frekuensi Nol Atau Sinyal DC." },
  { "en": "Pole Jauh Di Kiri Berarti?", "id": "Respon Transien Meredam Dengan Sangat Cepat." },
  { "en": "Zero Jauh Di Kiri Berarti?", "id": "Pengaruhnya Terhadap Respon Sistem Sangat Kecil." },
  { "en": "Fungsi Eksponensial e Pangkat -at cos(ωt)?", "id": "Contoh Sinyal Sinusoidal Yang Teredam." },
  { "en": "Pole Dari Sinyal Ini?", "id": "Terletak Di -a ± jω." },
  { "en": "Jarak Pole Dari Sumbu Imajiner?", "id": "Menentukan Laju Redaman (a)." },
  { "en": "Jarak Pole Dari Sumbu Riil?", "id": "Menentukan Frekuensi Osilasi Teredam (ω)." },
  { "en": "Jika Pole Bergerak Menjauhi Sumbu Riil?", "id": "Frekuensi Osilasi Akan Semakin Meningkat." },
  { "en": "Jika Pole Bergerak Ke Kiri?", "id": "Sistem Akan Meredam Dengan Lebih Cepat." },
  { "en": "Apa Itu Konstanta Waktu Dominan?", "id": "Konstanta Waktu Terbesar Dari Semua Pole." },
  { "en": "Peran Konstanta Waktu Dominan?", "id": "Menentukan Kecepatan Respon Sistem Secara Keseluruhan." },
  { "en": "Apa Yang Terjadi Jika Pole Berimpit?", "id": "Terjadi Kondisi Teredam Kritis (Critically Damped)." },
  { "en": "Respon Akibat Pole Berulang?", "id": "Memiliki Suku t Dikalikan Eksponensial." },
  { "en": "Apa Beda Dengan Transformasi Mellin?", "id": "Kernel Mellin Berbentuk x Pangkat s-1." },
  { "en": "Laplace Cocok Untuk Sistem?", "id": "Sistem Invarian Waktu Bukan Invarian Skala." },
  { "en": "Mellin Cocok Untuk Sistem?", "id": "Sistem Invarian Skala Yang Umum Di Geometri." },
  { "en": "Apa Itu Persamaan Diferensial Orde-n?", "id": "Persamaan Dengan Turunan Tertinggi Orde n." },
  { "en": "Jumlah Kondisi Awal Yang Dibutuhkan?", "id": "Sebanyak n Kondisi Awal (f(0) f'(0)...)." },
  { "en": "Apa Itu Masalah Batas (Boundary Problem)?", "id": "Kondisi Diberikan Pada Batas Waktu Berbeda." },
  { "en": "Apakah Laplace Cocok Untuk Masalah Batas?", "id": "Tidak Cocok Karena Didesain Untuk Nilai Awal." },
  { "en": "Apa Arti Analitis Sebuah Fungsi?", "id": "Fungsi Memiliki Turunan Di Suatu Daerah." },
  { "en": "F(s) Adalah Fungsi?", "id": "Fungsi Analitis Di Dalam Region Konvergensi." },
  { "en": "Pole Adalah Titik?", "id": "Titik Singularitas Dimana Fungsi Tidak Analitis." },
  { "en": "Apa Itu Sinyal Kotak Satuan?", "id": "Sinyal Bernilai Satu Antara Nol Dan Satu." },
  { "en": "Transformasi Sinyal Kotak Satuan?", "id": "(1 - e Pangkat -s) Dibagi s." },
  { "en": "Apa Itu Fungsi Transfer Tegangan?", "id": "Rasio Tegangan Output Terhadap Tegangan Input." },
  { "en": "Apa Itu Fungsi Transfer Arus?", "id": "Rasio Arus Output Terhadap Arus Input." },
  { "en": "Apa Itu Transimpedansi?", "id": "Rasio Tegangan Output Terhadap Arus Input." },
  { "en": "Apa Itu Transadmitansi?", "id": "Rasio Arus Output Terhadap Tegangan Input." },
  { "en": "Keempat Fungsi Ini Adalah?", "id": "Jenis-Jenis Fungsi Jaringan Atau Fungsi Transfer." },
  { "en": "Bagaimana Konvolusi Bekerja?", "id": "Membalik Satu Fungsi Lalu Menggeser Dan Mengalikan." },
  { "en": "Hasil Akhir Konvolusi?", "id": "Integral Dari Hasil Perkalian Setiap Saat." },
  { "en": "Teorema Konvolusi Menyederhanakan?", "id": "Perhitungan Respon Sistem LTI Secara Drastis." },
  { "en": "Apa Arti Fisis Dari Respon Impuls?", "id": "Respon Sistem Terhadap 'Tendangan' Sangat Singkat." },
  { "en": "Bagaimana Input Lain Dilihat?", "id": "Sebagai Jumlah Impuls-Impuls Berbobot Yang Bergeser." },
  { "en": "Respon Total Adalah?", "id": "Superposisi Respon Terhadap Setiap Impuls Input." },
  { "en": "Proses Superposisi Ini Adalah?", "id": "Definisi Matematis Dari Integral Konvolusi." },
  { "en": "Apa Itu Persamaan Volterra?", "id": "Persamaan Integral Yang Sering Muncul." },
  { "en": "Bagaimana Laplace Menyelesaikan Persamaan Volterra?", "id": "Mengubahnya Menjadi Persamaan Aljabar Yang Mudah." },
  { "en": "Apa Sifat Komutatif Konvolusi?", "id": "f Konvolusi g Sama Dengan g Konvolusi f." },
  { "en": "Apa Sifat Asosiatif Konvolusi?", "id": "Urutan Konvolusi Tiga Sinyal Tidak Penting." },
  { "en": "Apa Sifat Distributif Konvolusi?", "id": "Konvolusi Dengan Penjumlahan Bisa Didistribusikan." },
  { "en": "Sifat-Sifat Ini Memudahkan?", "id": "Analisis Sistem Kompleks Yang Terhubung Seri Paralel." },
  { "en": "Sistem Terhubung Seri?", "id": "Fungsi Transfer Total Adalah Perkalian Masing-Masing." },
  { "en": "Sistem Terhubung Paralel?", "id": "Fungsi Transfer Total Adalah Penjumlahan Masing-Masing." },
  { "en": "Apa Itu Metode Analisis Node?", "id": "Metode Analisis Rangkaian Berbasis Hukum Arus Kirchhoff." },
  { "en": "Analisis Node Di Domain-s?", "id": "Tegangan Node Diselesaikan Menggunakan Aljabar." },
  { "en": "Apa Itu Metode Analisis Mesh?", "id": "Metode Berbasis Hukum Tegangan Kirchhoff." },
  { "en": "Analisis Mesh Di Domain-s?", "id": "Arus Mesh Diselesaikan Menggunakan Aljabar." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Linier Menjadi Sumber Tegangan." },
  { "en": "Teorema Thevenin Di Domain-s?", "id": "Berlaku Dengan Impedansi Thevenin Z_th(s)." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
  { "en": "Teorema Norton Di Domain-s?", "id": "Berlaku Dengan Impedansi Norton Z_n(s)." },
  { "en": "Apa Itu Plot Bidang Kompleks?", "id": "Sama Dengan Bidang-s (s-Plane)." },
  { "en": "Bagaimana Menemukan Lokasi Pole?", "id": "Mencari Akar Dari Polinomial Penyebut H(s)." },
  { "en": "Bagaimana Menemukan Lokasi Zero?", "id": "Mencari Akar Dari Polinomial Pembilang H(s)." },
  { "en": "Apa Itu Error Tipe?", "id": "Error Keadaan Tunak Untuk Input Polinomial." },
  { "en": "Apa Itu Konstanta Error Posisi?", "id": "Konstanta Terkait Error Untuk Input Step." },
  { "en": "Apa Itu Konstanta Error Kecepatan?", "id": "Konstanta Terkait Error Untuk Input Ramp." },
  { "en": "Apa Itu Konstanta Error Akselerasi?", "id": "Konstanta Terkait Error Untuk Input Parabola." },
  { "en": "Konstanta Error Ini Tergantung?", "id": "Tipe Sistem (Jumlah Integrator Di Loop)." },
  { "en": "Apakah Laplace Berguna Untuk PDE?", "id": "Ya Untuk Persamaan Diferensial Parsial Tertentu." },
  { "en": "Bagaimana Laplace Bekerja Untuk PDE?", "id": "Mengeliminasi Turunan Terhadap Satu Variabel (Waktu)." },
  { "en": "Hasilnya Adalah Persamaan?", "id": "Persamaan Diferensial Biasa Dalam Variabel Spasial." },
  { "en": "Contoh PDE Yang Diselesaikan?", "id": "Persamaan Gelombang Dan Persamaan Panas." },
  { "en": "Apa Itu Sifat Konjugasi Kompleks?", "id": "Transformasi f*(t) Adalah F*(s*)." },
  { "en": "Sifat Ini Berguna Jika?", "id": "Sinyal f(t) Adalah Sinyal Bernilai Riil." },
  { "en": "Jika f(t) Riil Maka?", "id": "F(s) Memiliki Simetri Konjugat Kompleks." },
  { "en": "Arti Simetri Konjugat?", "id": "F(s) Sama Dengan F*(s*) Konjugat." },
  { "en": "Implikasi Simetri Konjugat?", "id": "Plot Pole-Zero Selalu Simetris Terhadap Sumbu Riil." },
  { "en": "Apa Itu Transformasi Invers Numerik?", "id": "Metode Komputer Untuk Mencari f(t) Dari F(s)." },
  { "en": "Kapan Invers Numerik Digunakan?", "id": "Ketika Bentuk Analitik Sulit Atau Tidak Ditemukan." },
  { "en": "Apa Itu Fungsi Transfer Rasional?", "id": "Fungsi Transfer Berupa Rasio Dua Polinomial." },
  { "en": "Sistem LTI Lumped Selalu?", "id": "Memiliki Fungsi Transfer Yang Rasional." },
  { "en": "Apa Itu Sistem Terdistribusi?", "id": "Sistem Dimana Parameter Tersebar (Saluran Transmisi)." },
  { "en": "Fungsi Transfer Sistem Terdistribusi?", "id": "Bukan Fungsi Rasional (Contohnya e Pangkat -s)." },
  { "en": "Apa Itu Operator Diferensial D?", "id": "Notasi Singkat Untuk d/dt." },
  { "en": "Bagaimana Hubungan D Dengan s?", "id": "Secara Informal D Bisa Diganti Dengan s." },
  { "en": "Penggantian Ini Berlaku Jika?", "id": "Semua Kondisi Awal Sistem Adalah Nol." },
  { "en": "Apa Itu Metode Aljabar Operator?", "id": "Metode Heaviside Yang Mendahului Analisis Laplace." },
  { "en": "Laplace Memberikan Landasan Matematis?", "id": "Rigor Untuk Metode Operator Heaviside." },
  { "en": "Apa Itu Fungsi Beta?", "id": "Fungsi Khusus Lain Yang Terkait Integral." },
  { "en": "Hubungan Laplace Dan Probabilitas?", "id": "Fungsi Pembangkit Momen Adalah Bentuk Laplace." },
  { "en": "Apa Itu Operator Laplace?", "id": "Operator Integral Yang Memetakan f(t) Ke F(s)." },
  { "en": "Apakah Operator Laplace Aditif?", "id": "Ya L[f+g] Sama Dengan L[f]+L[g]." },
  { "en": "Apakah Operator Laplace Homogen?", "id": "Ya L[cf] Sama Dengan c L[f]." },
  { "en": "Kedua Sifat Ini Membentuk?", "id": "Sifat Fundamental Linearitas Transformasi Laplace." },
  { "en": "Apa Nama Lain Bidang-s?", "id": "Bidang Frekuensi Kompleks Atau Complex Frequency Plane." },
  { "en": "Apa Efek Konvolusi Dengan Impuls?", "id": "Menghasilkan Kembali Fungsi Asli (f*δ = f)." },
  { "en": "Apa Efek Konvolusi Dengan Impuls Bergeser?", "id": "Menggeser Fungsi Asli Sebesar Pergeseran Impuls." },
  { "en": "Apa Itu Fungsi Transfer Tak Stabil?", "id": "Fungsi Transfer Dengan Setidaknya Satu Pole Kanan." },
  { "en": "Apa Itu Fungsi Transfer Stabil Marjinal?", "id": "Fungsi Dengan Pole Sederhana Di Sumbu Imajiner." },
  { "en": "Bagaimana Dengan Pole Ganda Di Sumbu Imajiner?", "id": "Menyebabkan Respon Amplitudo Tumbuh Secara Linear." },
  { "en": "Apakah Sistem Ini Stabil BIBO?", "id": "Tidak Karena Input Sinusoidal Bisa Beresonansi." },
  { "en": "Apa Itu Resonansi?", "id": "Input Memicu Mode Natural Sistem Menyebabkan Pertumbuhan." },
  { "en": "Frekuensi Resonansi Terjadi Pada?", "id": "Frekuensi Yang Sama Dengan Lokasi Pole." },
  { "en": "Bagaimana Cara Mengubah Respon Sistem?", "id": "Dengan Mengubah Posisi Pole Dan Zero." },
  { "en": "Cara Mengubah Posisi Pole?", "id": "Dengan Menambahkan Kontroler Umpan Balik (Feedback)." },
  { "en": "Apa Itu Kontroler Proporsional (P)?", "id": "Kontroler Yang Menguatkan Sinyal Error." },
  { "en": "Apa Itu Kontroler Integral (I)?", "id": "Kontroler Yang Menghilangkan Error Keadaan Tunak." },
  { "en": "Apa Itu Kontroler Derivatif (D)?", "id": "Kontroler Yang Mempercepat Respon Dan Redaman." },
  { "en": "Gabungan Ketiganya Adalah?", "id": "Kontroler PID (Proporsional-Integral-Derivatif) Yang Populer." },
  { "en": "Analisis Kontroler PID Dilakukan Di?", "id": "Domain-s Menggunakan Perkakas Transformasi Laplace." },
  { "en": "Apa Itu Fungsi Pembangkit?", "id": "Alat Matematis Dalam Teori Probabilitas." },
  { "en": "Hubungan Dengan Laplace?", "id": "Fungsi Pembangkit Momen Adalah Bentuk Laplace." },
  { "en": "Variabel Acak Apa Yang Terkait?", "id": "Variabel Acak Kontinu Non-Negatif." },
  { "en": "Apa Itu Teorema Tauberian?", "id": "Kebalikan Parsial Dari Teorema Nilai Akhir." },
  { "en": "Teorema Tauberian Menghubungkan?", "id": "Perilaku Asimtotik Fungsi Dengan Transformasinya." },
  { "en": "Apa Arti Asimtotik?", "id": "Perilaku Fungsi Saat Variabel Menuju Ekstrem." },
  { "en": "Apa Batasan Notasi 's'?", "id": "Hanya Simbol Bisa Diganti Huruf Lain." },
  { "en": "Huruf Lain Yang Kadang Dipakai?", "id": "Huruf p Sering Dipakai Dalam Literatur Lama." },
  { "en": "Apa Transformasi Dari Fungsi Konstan Satu?", "id": "Satu Dibagi Dengan s Sama Dengan Fungsi Step." },
  { "en": "Kenapa Hasilnya Sama?", "id": "Karena Unilateral Mengasumsikan Fungsi Nol Sebelum t=0." },
  { "en": "Fungsi 1 dan u(t) Sama?", "id": "Sama Untuk t >= 0." },
  { "en": "Apa Itu Sistem Disipatif?", "id": "Sistem Yang Membuang Energi Biasanya Sebagai Panas." },
  { "en": "Hubungan Dengan Stabilitas?", "id": "Sistem Disipatif Cenderung Stabil Secara Alami." },
  { "en": "Pole Sistem Disipatif?", "id": "Berada Di Setengah Bidang Kiri." },
  { "en": "Apa Itu Sistem Konservatif?", "id": "Sistem Yang Tidak Kehilangan Energi (Ideal)." },
  { "en": "Pole Sistem Konservatif?", "id": "Berada Tepat Di Sumbu Imajiner." },
  { "en": "Contoh Sistem Konservatif?", "id": "Rangkaian LC Ideal Tanpa Adanya Resistor." },
  { "en": "Apa Itu Mode Sistem?", "id": "Setiap Pole Menghasilkan Satu Mode Respon." },
  { "en": "Mode Dari Pole Riil?", "id": "Mode Eksponensial Yang Meredam Atau Tumbuh." },
  { "en": "Mode Dari Pole Kompleks?", "id": "Mode Sinusoidal Teredam Atau Tumbuh." },
  { "en": "Apa Itu Keterkontrolan (Controllability)?", "id": "Kemampuan Input Untuk Mengontrol Semua Keadaan." },
  { "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Output Untuk Merefleksikan Semua Keadaan." },
  { "en": "Pembatalan Pole-Zero Mempengaruhi?", "id": "Keterkontrolan Dan Keteramatan Mode Tersebut." },
  { "en": "Mode Yang Dibatalkan Menjadi?", "id": "Tidak Terkontrol Atau Tidak Teramati." },
  { "en": "Apa Itu Realisasi Sistem?", "id": "Mencari Representasi State-Space Dari Fungsi Transfer." },
  { "en": "Apakah Realisasi Sistem Unik?", "id": "Tidak Ada Banyak Realisasi Yang Mungkin." },
  { "en": "Apa Itu Realisasi Minimal?", "id": "Realisasi Dengan Jumlah Keadaan Paling Sedikit." },
  { "en": "Orde Realisasi Minimal?", "id": "Sama Dengan Orde Fungsi Transfer." },
  { "en": "Transformasi Balik Dari F(s/a)?", "id": "a Dikalikan Dengan f(at)." },
  { "en": "Sifat Ini Adalah Bentuk Lain?", "id": "Sifat Penskalaan Waktu (Time Scaling)." },
  { "en": "Apa Itu Transformasi Laplace Diskrit?", "id": "Istilah Yang Kadang Salah Digunakan Untuk Transformasi-Z." },
  { "en": "Apa Itu Persamaan Diferensial Kaku (Stiff)?", "id": "Persamaan Dengan Konstanta Waktu Sangat Berbeda." },
  { "en": "Apakah Laplace Peduli Dengan Kekakuan?", "id": "Tidak Metode Analitik Tidak Terpengaruh." },
  { "en": "Kekakuan Mempengaruhi Metode Apa?", "id": "Metode Numerik Untuk Simulasi Di Domain Waktu." },
  { "en": "Apa Itu Sifat Modulasi?", "id": "Sama Dengan Teorema Pergeseran Frekuensi." },
  { "en": "Kenapa Disebut Modulasi?", "id": "Karena Perkalian Dengan Sinusoid Adalah Modulasi." },
  { "en": "Transformasi f(t) cos(ω₀t)?", "id": "Setengah [F(s-jω₀) + F(s+jω₀)]." },
  { "en": "Apa Itu Identitas Euler?", "id": "Menghubungkan Eksponensial Kompleks Dengan Sinus Dan Cosinus." },
  { "en": "Identitas Euler Berguna Untuk?", "id": "Menurunkan Banyak Pasangan Dan Sifat Transformasi." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Bernilai Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Transformasi Hilbert Terkait Dengan?", "id": "Hubungan Bagian Riil Dan Imajiner Sinyal Analitik." },
  { "en": "Apa Itu Frekuensi Kompleks s = σ + jω?", "id": "Titik Pada Bidang-s Yang Kompleks." },
  { "en": "Apa Interpretasi f(t)e^-σt?", "id": "Sinyal f(t) Yang Diberi Pembobotan Eksponensial." },
  { "en": "Transformasi Laplace Adalah Transformasi?", "id": "Transformasi Fourier Dari Sinyal Terbobot Ini." },
  { "en": "Jadi Laplace Mencakup Fourier?", "id": "Ya Sebagai Kasus Khusus Saat σ=0." },
  { "en": "Syarat Fourier Konvergen?", "id": "Integral Absolut f(t) Harus Terbatas." },
  { "en": "Syarat Laplace Lebih Longgar?", "id": "Ya Faktor e^-σt Membantu Integral Konvergen." },
  { "en": "Contoh Fungsi Dengan Transformasi Laplace Tapi Tidak Fourier?", "id": "Fungsi Step u(t) Atau Ramp t." },
  { "en": "Apa Itu Fungsi Transfer Tegangan Terbuka?", "id": "Rasio Tegangan Output Tanpa Beban Terhadap Input." },
  { "en": "Apa Itu Arus Hubung Singkat?", "id": "Arus Output Ketika Terminal Output Dihubung Singkat." },
  { "en": "Impedansi Input Z_in(s)?", "id": "Rasio V_in(s) Terhadap I_in(s)." },
  { "en": "Impedansi Output Z_out(s)?", "id": "Impedansi Thevenin Dilihat Dari Terminal Output." },
  { "en": "Bagaimana Menghitung Z_out(s)?", "id": "Mematikan Sumber Input Independen Dan Mencari Impedansi." },
  { "en": "Apa Maksud Mematikan Sumber Tegangan?", "id": "Menggantinya Dengan Hubungan Singkat (Short Circuit)." },
  { "en": "Apa Maksud Mematikan Sumber Arus?", "id": "Menggantinya Dengan Hubungan Terbuka (Open Circuit)." },
  { "en": "Apa Itu Persamaan Diferensial Linier?", "id": "Persamaan Dimana Variabel Tidak Dikuadratkan Dll." },
  { "en": "Apa Itu Koefisien Konstan?", "id": "Koefisien Persamaan Diferensial Tidak Berubah." },
  { "en": "Laplace Efektif Untuk Persamaan?", "id": "Persamaan Diferensial Linier Dengan Koefisien Konstan." },
  { "en": "Bagaimana Dengan Koefisien Variabel?", "id": "Penyelesaian Menjadi Jauh Lebih Rumit." },
  { "en": "Apa Itu Persamaan Bessel Dan Legendre?", "id": "Contoh Persamaan Dengan Koefisien Variabel." },
  { "en": "Apa Itu Kondisi Diam (At Rest)?", "id": "Berarti Semua Kondisi Awal Adalah Nol." },
  { "en": "Jika Sistem Diam Maka?", "id": "Y(s) Sama Dengan H(s) Dikali X(s)." },
  { "en": "Apa Itu Sumbu Real Negatif?", "id": "Tempat Pole Sistem Overdamped Orde Pertama." },
  { "en": "Di Mana Pole Sistem Undamped?", "id": "Tepat Di Sumbu Imajiner (Bukan Di Asal)." },
  { "en": "Di Mana Pole Sistem Tidak Stabil Osilasi?", "id": "Di Sisi Kanan Bidang-s (Kompleks Konjugat)." },
  { "en": "Apa Nama Lain Teorema Nilai Awal/Akhir?", "id": "Initial/Final Value Property Of Laplace Transform." },
  { "en": "Kapan Hasil Transformasi Riil?", "id": "Sangat Jarang Terjadi Dalam Praktiknya." },
  { "en": "Apa Itu Transformasi Balik Residu?", "id": "Metode Formal Menggunakan Integral Kontur Kompleks." },
  { "en": "Teorema Residu Cauchy?", "id": "Dasar Matematis Untuk Metode Transformasi Balik Ini." },
  { "en": "Fraksi Parsial Adalah Bentuk Sederhana?", "id": "Penerapan Praktis Dari Teorema Residu Cauchy." },
  { "en": "Apa Itu Sistem Kontinu?", "id": "Sinyal Dan Sistem Didefinisikan Untuk Semua Waktu." },
  { "en": "Apa Itu Sistem Diskrit?", "id": "Sinyal Didefinisikan Hanya Pada Interval Waktu Tertentu." },
  { "en": "Laplace Adalah Alat Utama Untuk?", "id": "Analisis Sistem Waktu Kontinu Atau Analog." },
  { "en": "Transformasi-Z Adalah Alat Utama?", "id": "Analisis Sistem Waktu Diskrit Atau Digital." },
  { "en": "Apakah Ada Hubungan Keduanya?", "id": "Ya Transformasi-Z Bisa Dilihat Sebagai Aproksimasi Laplace." },
  { "en": "Apa Itu Aproksimasi Tustin?", "id": "Metode Populer Untuk Mendiskritkan Sistem Kontinu." },
  { "en": "Apa Itu Analisis Fundamental?", "id": "Memahami Konsep Dasar Tanpa Detail Berlebihan." },
  { "en": "Apakah Batch Ini Cukup Fundamental?", "id": "Ya Fokus Pada Konsep Inti Laplace." },
  { "en": "Apa Langkah Pertama Solusi Persamaan Diferensial?", "id": "Transformasi Laplace Pada Setiap Suku Persamaan." },
  { "en": "Apa Langkah Kedua Solusi Persamaan Diferensial?", "id": "Masukkan Semua Kondisi Awal Yang Diberikan." },
  { "en": "Apa Langkah Ketiga Solusi Persamaan Diferensial?", "id": "Selesaikan Persamaan Aljabar Untuk Fungsi Y(s)." },
  { "en": "Apa Langkah Terakhir Solusi Persamaan Diferensial?", "id": "Cari Transformasi Balik Untuk Mendapatkan y(t)." },
  { "en": "Apa Nama Lain Respon Natural?", "id": "Respon Input Nol Atau Solusi Homogen." },
  { "en": "Respon Natural Bergantung Pada?", "id": "Hanya Karakteristik Internal Dari Sistem." },
  { "en": "Apa Nama Lain Respon Paksa?", "id": "Respon Keadaan Nol Atau Solusi Partikular." },
  { "en": "Respon Paksa Bergantung Pada?", "id": "Hanya Fungsi Input Yang Diberikan." },
  { "en": "Bergerak Horizontal Ke Kiri Di Bidang-s?", "id": "Meningkatkan Tingkat Redaman Eksponensial Sistem." },
  { "en": "Bergerak Horizontal Ke Kanan Di Bidang-s?", "id": "Mengurangi Tingkat Redaman Menuju Ketidakstabilan." },
  { "en": "Bergerak Vertikal Menjauhi Sumbu Riil?", "id": "Meningkatkan Frekuensi Osilasi Respon Sistem." },
  { "en": "Apa Turunan Dari Fungsi Step?", "id": "Fungsi Impuls Dirac Yang Sangat Tajam." },
  { "en": "Apa Integral Dari Fungsi Impuls?", "id": "Fungsi Step Satuan (Unit Step Function)." },
  { "en": "Apa Turunan Dari Fungsi Ramp?", "id": "Fungsi Step Satuan Dengan Amplitudo Konstan." },
  { "en": "Apa Integral Dari Fungsi Ramp?", "id": "Fungsi Parabola (t Kuadrat Dibagi Dua)." },
  { "en": "Apa Itu Keluarga Fungsi Singularity?", "id": "Impuls Step Ramp Parabola Dan Seterusnya." },
  { "en": "Hubungan Antar Fungsi Singularity?", "id": "Setiap Fungsi Adalah Integral Dari Sebelumnya." },
  { "en": "Apa Analogi Sistem Mekanis Untuk Resistor?", "id": "Peredam Kejut Atau Dashpot (Gesekan)." },
  { "en": "Apa Analogi Sistem Mekanis Untuk Kapasitor?", "id": "Pegas Yang Menyimpan Energi Potensial." },
  { "en": "Apa Analogi Sistem Mekanis Untuk Induktor?", "id": "Massa Yang Menyimpan Energi Kinetik." },
  { "en": "Apa Analogi Untuk Tegangan?", "id": "Gaya Yang Diberikan Pada Sistem Mekanis." },
  { "en": "Apa Analogi Untuk Arus?", "id": "Kecepatan Gerak Dari Massa Sistem." },
  { "en": "Sistem Analog Ini Memiliki?", "id": "Persamaan Diferensial Dengan Bentuk Yang Sama." },
  { "en": "Apa Itu Sistem Underdamped?", "id": "Sistem Yang Berosilasi Sebelum Mencapai Kestabilan." },
  { "en": "Apa Itu Sistem Overdamped?", "id": "Sistem Lambat Yang Tidak Mengalami Osilasi." },
  { "en": "Apa Itu Sistem Critically Damped?", "id": "Respon Tercepat Yang Mungkin Tanpa Osilasi." },
  { "en": "Kondisi Ini Ditentukan Oleh?", "id": "Lokasi Pole-Pole Dari Fungsi Transfer." },
  { "en": "Lokasi Pole Untuk Underdamped?", "id": "Sepasang Pole Kompleks Konjugat Di Kiri." },
  { "en": "Lokasi Pole Untuk Overdamped?", "id": "Dua Pole Riil Berbeda Di Kiri." },
  { "en": "Lokasi Pole Untuk Critically Damped?", "id": "Dua Pole Riil Sama (Berulang) Di Kiri." },
  { "en": "Apa Itu Gelombang Sinus Teredam?", "id": "Gelombang Sinus Dikalikan Fungsi Eksponensial Turun." },
  { "en": "Transformasi Gelombang Sinus Teredam?", "id": "Memiliki Penyebut Dengan Suku (s+a)²." },
  { "en": "Apa Itu Zero Blocking Property?", "id": "Zero Dapat Memblokir Respon Frekuensi Tertentu." },
  { "en": "Jika Input Punya Frekuensi Sama Dengan Zero?", "id": "Maka Output Sistem Akan Bernilai Nol." },
  { "en": "Apa Itu Fungsi Transfer All-Pass?", "id": "Hanya Mengubah Fasa Tanpa Mengubah Amplitudo." },
  { "en": "Magnitudo Respon Frekuensi All-Pass?", "id": "Selalu Sama Dengan Satu (0 dB)." },
  { "en": "Apa Itu Error Offset?", "id": "Sama Dengan Error Keadaan Tunak Konstan." },
  { "en": "Bagaimana Kontroler Integral Menghilangkan Error?", "id": "Dengan Terus Menjumlahkan Error Seiring Waktu." },
  { "en": "Apa Itu Sinyal Pembawa (Carrier)?", "id": "Sinyal Frekuensi Tinggi Dalam Proses Modulasi." },
  { "en": "Apa Itu Sinyal Modulasi?", "id": "Sinyal Informasi Frekuensi Rendah Yang Dikirim." },
  { "en": "Proses Modulasi Adalah?", "id": "Perkalian Dua Sinyal Dalam Domain Waktu." },
  { "en": "Hasilnya Di Domain Frekuensi?", "id": "Konvolusi Spektrum Kedua Sinyal Tersebut." },
  { "en": "Apa Itu Teorema Parseval?", "id": "Menghubungkan Energi Sinyal Di Domain Waktu Frekuensi." },
  { "en": "Apakah Ada Teorema Parseval Untuk Laplace?", "id": "Ada Tapi Lebih Kompleks Dari Versi Fourier." },
  { "en": "Apa Itu Sistem Invertible?", "id": "Sistem Yang Inputnya Bisa Dipulihkan Dari Output." },
  { "en": "Syarat Sistem Invertible?", "id": "Ada Fungsi Transfer Invers H⁻¹(s)." },
  { "en": "Fungsi Transfer Invers H⁻¹(s)?", "id": "Sama Dengan 1 Dibagi Dengan H(s)." },
  { "en": "Kapan Sistem Invers Stabil?", "id": "Jika Semua Zero Sistem Asli Di Kiri." },
  { "en": "Sistem Dengan Zero Di Kiri Disebut?", "id": "Sistem Minimum Phase Yang Sudah Dibahas." },
  { "en": "Apa Itu Aproksimasi Orde Rendah?", "id": "Menyederhanakan Sistem Orde Tinggi Menjadi Orde Rendah." },
  { "en": "Tujuan Aproksimasi Orde Rendah?", "id": "Memudahkan Analisis Dan Desain Sistem Kontrol." },
  { "en": "Metode Aproksimasi Paling Sederhana?", "id": "Mengabaikan Pole Dan Zero Yang Tidak Dominan." },
  { "en": "Apa Itu Pole Tidak Dominan?", "id": "Pole Yang Sangat Jauh Di Kiri." },
  { "en": "Apa Arti Fisis Time Constant?", "id": "Ukuran 'Kelambatan' Atau Inersia Suatu Sistem." },
  { "en": "Sistem Dengan Time Constant Besar?", "id": "Merespon Sangat Lambat Terhadap Perubahan Input." },
  { "en": "Sistem Dengan Time Constant Kecil?", "id": "Merespon Sangat Cepat Terhadap Perubahan Input." },
  { "en": "Apa Efek Penambahan Zero Di Kiri?", "id": "Meningkatkan Kecepatan Respon Dan Juga Overshoot." },
  { "en": "Apa Efek Penambahan Pole Di Kiri?", "id": "Memperlambat Respon Sistem Secara Keseluruhan." },
  { "en": "Apa Itu Analisis Sensitivitas?", "id": "Studi Bagaimana Output Berubah Akibat Parameter." },
  { "en": "Sensitivitas Terhadap Apa?", "id": "Perubahan Kecil Pada Parameter Sistem." },
  { "en": "Laplace Memudahkan Analisis Sensitivitas?", "id": "Ya Dengan Menganalisis Fungsi Transfer H(s)." },
  { "en": "Apa Itu Spektrum Sinyal?", "id": "Representasi Sinyal Dalam Domain Frekuensi." },
  { "en": "Magnitudo |F(s)| Disebut?", "id": "Spektrum Magnitudo Dari Sinyal f(t)." },
  { "en": "Sudut ∠F(s) Disebut?", "id": "Spektrum Fasa Dari Sinyal f(t)." },
  { "en": "Kernel Transformasi e^-st Terdiri Dari?", "id": "Bagian Riil e^-σt Dan Imajiner e^-jωt." },
  { "en": "Bagian Riil Kernel?", "id": "Fungsi Redaman Eksponensial Yang Riil." },
  { "en": "Bagian Imajiner Kernel?", "id": "Fungsi Sinusoidal Kompleks (Sinus Dan Cosinus)." },
  { "en": "Jadi Laplace Mencari Proyeksi?", "id": "Sinyal f(t) Pada Basis Eksponensial Teredam." },
  { "en": "Bagaimana Transformasi Invers Dihitung Secara Formal?", "id": "Menggunakan Integral Kontur Di Bidang Kompleks." },
  { "en": "Apakah Perhitungan Ini Praktis?", "id": "Tidak Sangat Rumit Dan Jarang Digunakan." },
  { "en": "Alternatif Praktisnya Adalah?", "id": "Penggunaan Tabel Pasangan Transformasi Yang Ada." },
  { "en": "Apa Itu Plot Nichols?", "id": "Plot Lain Untuk Menganalisis Respon Frekuensi." },
  { "en": "Plot Nichols Memetakan Apa?", "id": "Gain (dB) Melawan Fasa (Derajat)." },
  { "en": "Kestabilan Di Plot Nichols?", "id": "Dilihat Dari Jarak Ke Titik Kritis." },
  { "en": "Titik Kritis Di Plot Nichols?", "id": "Nol dB Dan Minus 180 Derajat." },
  { "en": "Apa Itu Transformasi Laplace Dua Dimensi?", "id": "Generalisasi Untuk Fungsi Dengan Dua Variabel." },
  { "en": "Aplikasi Transformasi Laplace 2D?", "id": "Pemrosesan Gambar Dan Analisis Sistem Spasial." },
  { "en": "Apa Beda Dengan Transformasi Fourier 2D?", "id": "Sama Laplace 2D Bisa Menganalisis Sinyal Tumbuh." },
  { "en": "Apa Itu Error Tetesan (Droop)?", "id": "Penurunan Amplitudo Pada Respon Pulsa." },
  { "en": "Error Tetesan Disebabkan Oleh?", "id": "Efek Diferensiator Atau Zero Di Titik Asal." },
  { "en": "Apa Itu Waktu Tunda Grup?", "id": "Ukuran Distorsi Fasa Pada Sinyal." },
  { "en": "Waktu Tunda Grup Dihitung Dari?", "id": "Turunan Negatif Dari Fasa Respon Frekuensi." },
  { "en": "Filter Dengan Fasa Linier?", "id": "Memiliki Waktu Tunda Grup Yang Konstan." },
  { "en": "Apa Efek Waktu Tunda Grup Konstan?", "id": "Semua Frekuensi Tertunda Sama Tidak Ada Distorsi." },
  { "en": "Apa Itu Sinyal Pita Dasar (Baseband)?", "id": "Sinyal Yang Spektrumnya Terpusat Di Sekitar Nol." },
  { "en": "Apa Itu Sinyal Lolos Pita (Passband)?", "id": "Sinyal Yang Spektrumnya Terpusat Di Frekuensi Pembawa." },
  { "en": "Analisis Sinyal Lolos Pita?", "id": " sering Dilakukan Menggunakan Representasi Baseband Kompleks." },
  { "en": "Apa Itu Sistem All-Pole?", "id": "Sistem Yang Tidak Memiliki Zero Terbatas." },
  { "en": "Contoh Sistem All-Pole?", "id": "Filter Butterworth Dan Filter Chebyshev Tipe I." },
  { "en": "Apa Itu Sistem All-Zero?", "id": "Sistem Yang Tidak Memiliki Pole Terbatas." },
  { "en": "Contoh Sistem All-Zero?", "id": "Filter FIR (Finite Impulse Response)." },
  { "en": "Apakah Laplace Berguna Untuk Filter FIR?", "id": "Tidak Transformasi-Z Lebih Cocok Untuk FIR." },
  { "en": "Kenapa Laplace Tidak Cocok Untuk FIR?", "id": "Karena Respon Impulsnya Berdurasi Terbatas." },
  { "en": "Apa Itu Sistem IIR?", "id": "Infinite Impulse Response Filter." },
  { "en": "Laplace Cocok Untuk Filter?", "id": "Filter IIR Analog Yang Dideskripsikan Persamaan Diferensial." },
  { "en": "Apa Esensi Transformasi Laplace?", "id": "Jembatan Antara Dunia Diferensial Dan Aljabar." },
  { "en": "Apakah Laplace Hanya Untuk Teknik Elektro?", "id": "Tidak Sangat Luas Dipakai Di Berbagai Bidang." },
  { "en": "Contoh Bidang Lain?", "id": "Ekonomi (Model Dinamis) Biologi (Farmakokinetik)." },
  { "en": "Apa Itu Farmakokinetik?", "id": "Studi Pergerakan Obat Di Dalam Tubuh." },
  { "en": "Model Farmakokinetik Menggunakan?", "id": "Persamaan Diferensial Yang Diselesaikan Dengan Laplace." },
  { "en": "Apa Perbedaan Utama Pole Dan Zero?", "id": "Pole Di Penyebut Zero Di Pembilang." },
  { "en": "Apa Perbedaan Efek Pole Dan Zero?", "id": "Pole Tentukan Stabilitas Zero Tentukan Bentuk Respon." },
  { "en": "Apa Interpretasi Sumbu Riil Negatif?", "id": "Lokasi Mode-Mode Eksponensial Yang Meredam." },
  { "en": "Apa Interpretasi Titik Asal (Origin)?", "id": "Mode Integrator Atau Frekuensi DC (Nol)." },
  { "en": "Apa Tujuan Utama Pembagian Polinomial?", "id": "Memastikan Fungsi Rasional Menjadi Bentuk Proper." },
  { "en": "Kapan Pembagian Polinomial Diperlukan?", "id": "Saat Derajat Pembilang Lebih Besar Sama Penyebut." },
  { "en": "Apa Definisi Formal Sistem Kausal?", "id": "Respon Impuls h(t) Nol Untuk t<0." },
  { "en": "Laplace Memetakan Ruang Fungsi Waktu Ke?", "id": "Ruang Fungsi Analitis Di Bidang Kompleks." },
  { "en": "Osilasi Murni Di Domain Waktu Berarti?", "id": "Adanya Pole Di Sumbu Imajiner." },
  { "en": "Peluruhan Murni Di Domain Waktu Berarti?", "id": "Adanya Pole Di Sumbu Riil Negatif." },
  { "en": "Apa Beda Utama Laplace Dan Fourier?", "id": "Laplace Memiliki Faktor Konvergensi e Pangkat -σt." },
  { "en": "Faktor Ini Memungkinkan Transformasi?", "id": "Fungsi Yang Tumbuh Seperti Fungsi Eksponensial." },
  { "en": "Apa Arti Fisik Redaman Kritis?", "id": "Kembali Ke Posisi Setimbang Tercepat Mungkin." },
  { "en": "Apa Arti Fisik Respon Underdamped?", "id": "Berayun Melewati Titik Setimbang Sebelum Berhenti." },
  { "en": "Apa Arti Fisik Respon Overdamped?", "id": "Kembali Ke Setimbang Secara Lambat Tanpa Berayun." },
  { "en": "Kenapa ROC Penting Untuk Transformasi Balik?", "id": "Menjamin Bahwa Hasil Transformasi Balik Unik." },
  { "en": "Tanpa ROC Apa Yang Terjadi?", "id": "Satu F(s) Bisa Punya Banyak f(t)." },
  { "en": "Apa Esensi Dari Prinsip Superposisi?", "id": "Respon Terhadap Jumlah Input Adalah Jumlah Respon." },
  { "en": "Sistem Linier Harus Memenuhi?", "id": "Sifat Aditivitas Dan Sifat Homogenitas." },
  { "en": "Apa Itu Sifat Aditivitas?", "id": "Respon f1+f2 Adalah Respon f1 + Respon f2." },
  { "en": "Apa Itu Sifat Homogenitas?", "id": "Respon kf Adalah k Dikalikan Respon f." },
  { "en": "Apa Itu Domain Analisis?", "id": "Domain Frekuensi-s Tempat Analisis Dilakukan." },
  { "en": "Apa Itu Domain Sintesis?", "id": "Domain Waktu Tempat Sinyal Direalisasikan." },
  { "en": "Apa Hubungan Antara Step Dan Impuls?", "id": "Step Adalah Integral Dari Fungsi Impuls." },
  { "en": "Apa Hubungan Antara Ramp Dan Step?", "id": "Ramp Adalah Integral Dari Fungsi Step." },
  { "en": "Hubungan Ini Tercermin Di Domain-s?", "id": "Sebagai Faktor Pembagian Dengan Variabel s." },
  { "en": "Apa Itu Fungsi Transfer Loop Gain?", "id": "Produk Semua Fungsi Transfer Dalam Satu Loop." },
  { "en": "Kapan Sistem Loop Tertutup Stabil?", "id": "Akar Dari 1+G(s)H(s) Di Kiri." },
  { "en": "Penyebut Fungsi Transfer Loop Tertutup?", "id": "Disebut Juga Persamaan Karakteristik Loop Tertutup." },
  { "en": "Apa Itu Kondisi Awal Turunan?", "id": "Nilai f'(0) f''(0) Dan Seterusnya." },
  { "en": "Laplace Membutuhkan Kondisi Awal Apa Saja?", "id": "Semua Turunan Sampai Orde n-1." },
  { "en": "Apa Arti Fisis Kondisi Awal Kapasitor?", "id": "Tegangan Awal Yang Tersimpan Di Kapasitor." },
  { "en": "Apa Arti Fisis Kondisi Awal Induktor?", "id": "Arus Awal Yang Mengalir Melalui Induktor." },
  { "en": "Bagaimana Jika Kondisi Awal Tidak Diketahui?", "id": "Dapat Dibiarkan Sebagai Variabel Dalam Solusi." },
  { "en": "Apa Itu Solusi Umum?", "id": "Solusi Yang Masih Mengandung Konstanta Tak Diketahui." },
  { "en": "Apa Itu Solusi Khusus?", "id": "Solusi Dimana Konstanta Sudah Ditentukan." },
  { "en": "Laplace Selalu Memberikan Solusi?", "id": "Solusi Khusus Karena Memakai Kondisi Awal." },
  { "en": "Apa Itu Sinyal Terbatas (Bounded)?", "id": "Sinyal Yang Amplitudonya Tidak Pernah Tak Hingga." },
  { "en": "Definisi Stabilitas BIBO Memerlukan?", "id": "Setiap Input Terbatas Hasilkan Output Terbatas." },
  { "en": "Sistem Dengan Pole Di Kanan?", "id": "Bisa Hasilkan Output Tak Terbatas Dari Input Terbatas." },
  { "en": "Bagaimana Pole Di Kanan Menyebabkan Ini?", "id": "Mode Natural Tumbuh Eksponensial Tanpa Batas." },
  { "en": "Apa Itu Absis Konvergensi?", "id": "Batas Vertikal Paling Kiri Dari ROC." },
  { "en": "Absis Konvergensi Ditentukan Oleh?", "id": "Bagian Riil Dari Pole Paling Kanan." },
  { "en": "Jika Sinyal Kausal Maka ROC?", "id": "Re(s) > σ_c (Absis Konvergensi)." },
  { "en": "Jika Sinyal Anti-Kausal Maka ROC?", "id": "Re(s) < σ_c (Pole Paling Kiri)." },
  { "en": "Apa Itu Sinyal Anti-Kausal?", "id": "Sinyal Yang Bernilai Nol Untuk t > 0." },
  { "en": "Apakah Sinyal Anti-Kausal Fisis?", "id": "Tidak Karena Mendahului Rangsangan Atau Input." },
  { "en": "Apa Itu Sinyal Non-Kausal?", "id": "Sinyal Yang Ada Untuk Waktu Positif Negatif." },
  { "en": "ROC Sinyal Non-Kausal?", "id": "Berbentuk Pita Vertikal Di Bidang-s." },
  { "en": "Pita Vertikal Ini Dibatasi Oleh?", "id": "Dua Pole Dari Sinyal Tersebut." },
  { "en": "Apa Maksud Pole Di Tak Hingga?", "id": "Sistem Strictly Proper Punya Zero Di Tak Hingga." },
  { "en": "Jumlah Pole Dan Zero Terbatas?", "id": "Sama Untuk Fungsi Transfer Rasional." },
  { "en": "Apa Itu Deret Laurent?", "id": "Ekspansi Fungsi Di Sekitar Titik Singularitas." },
  { "en": "Hubungan Dengan Fraksi Parsial?", "id": "Fraksi Parsial Adalah Kasus Khusus Deret Laurent." },
  { "en": "Apa Itu Teorema Sisa (Residue Theorem)?", "id": "Teorema Kunci Dalam Analisis Fungsi Kompleks." },
  { "en": "Bagaimana Teorema Ini Dipakai?", "id": "Untuk Menghitung Integral Kontur Transformasi Balik." },
  { "en": "Apa Itu Kontur Integrasi?", "id": "Jalur Tertutup Di Bidang Kompleks." },
  { "en": "Apa Itu Singularitas Esensial?", "id": "Tipe Singularitas Kompleks Selain Pole." },
  { "en": "Fungsi Transfer Sistem Fisik?", "id": "Biasanya Hanya Memiliki Singularitas Berupa Pole." },
  { "en": "Contoh Fungsi Dengan Singularitas Esensial?", "id": "Fungsi e Pangkat 1/s Di s=0." },
  { "en": "Apakah Solusi Persamaan Diferensial Selalu Unik?", "id": "Ya Jika Kondisi Awal Diberikan Lengkap." },
  { "en": "Apa Itu Teorema Eksistensi Dan Keunikan?", "id": "Menjamin Adanya Solusi Unik Untuk Persamaan Diferensial." },
  { "en": "Laplace Secara Implisit Mengasumsikan?", "id": "Teorema Eksistensi Dan Keunikan Ini Berlaku." },
  { "en": "Apa Itu Respon Frekuensi Negatif?", "id": "Secara Matematis Ada Tapi Tidak Fisis." },
  { "en": "Untuk Sinyal Riil Spektrum?", "id": "Memiliki Simetri Konjugat |H(-jω)| = |H(jω)|." },
  { "en": "Apa Arti Simetri Magnitudo?", "id": "Plot Magnitudo Adalah Fungsi Genap." },
  { "en": "Simetri Untuk Fasa?", "id": "Plot Fasa Adalah Fungsi Ganjil." },
  { "en": "Bagaimana Menangani Fungsi Dengan Lompatan?", "id": "Direpresentasikan Menggunakan Kombinasi Fungsi Step." },
  { "en": "Bagaimana Menangani Fungsi Dengan Sudut Tajam?", "id": "Direpresentasikan Menggunakan Kombinasi Fungsi Ramp." },
  { "en": "Apa Itu Aproksimasi Butterworth?", "id": "Desain Filter Dengan Respon Frekuensi Paling Datar." },
  { "en": "Apa Itu Aproksimasi Chebyshev?", "id": "Desain Filter Dengan Transisi Lebih Tajam." },
  { "en": "Kekurangan Filter Chebyshev?", "id": "Memiliki Riak (Ripple) Di Passband." },
  { "en": "Pole Filter Butterworth Terletak?", "id": "Pada Setengah Lingkaran Di Sisi Kiri." },
  { "en": "Pole Filter Chebyshev Terletak?", "id": "Pada Setengah Elips Di Sisi Kiri." },
  { "en": "Desain Filter Ini Dilakukan Di?", "id": "Domain-s Menggunakan Posisi Pole Dan Zero." },
  { "en": "Apa Itu Filter Notch?", "id": "Filter Yang Menolak Satu Frekuensi Spesifik." },
  { "en": "Bagaimana Desain Filter Notch?", "id": "Menempatkan Sepasang Zero Di Sumbu Imajiner." },
  { "en": "Lokasi Zero Menentukan?", "id": "Frekuensi Yang Akan Ditolak Atau Diblokir." },
  { "en": "Apa Itu Konstanta Waktu Rangkaian RC?", "id": "Produk Dari Resistansi Dan Kapasitansi (RC)." },
  { "en": "Apa Itu Konstanta Waktu Rangkaian RL?", "id": "Induktansi Dibagi Dengan Resistansi (L/R)." },
  { "en": "Pole Rangkaian RC?", "id": "Terletak Di s = -1/RC." },
  { "en": "Pole Rangkaian RL?", "id": "Terletak Di s = -R/L." },
  { "en": "Apa Itu Sinyal Kuantisasi?", "id": "Sinyal Kontinu Yang Diubah Menjadi Level Diskrit." },
  { "en": "Apakah Laplace Menganalisis Kuantisasi?", "id": "Tidak Itu Adalah Proses Non-Linier." },
  { "en": "Apa Itu Sinyal Sampling?", "id": "Sinyal Kontinu Diubah Menjadi Sinyal Diskrit." },
  { "en": "Transformasi Apa Untuk Sinyal Sampling?", "id": "Transformasi-Z Lebih Tepat Daripada Laplace." },
  { "en": "Apa Beda Sistem Orde 1 Dan 2?", "id": "Sistem Orde 2 Bisa Berosilasi Orde 1 Tidak." },
  { "en": "Kenapa Sistem Orde 1 Tidak Berosilasi?", "id": "Karena Hanya Memiliki Satu Pole Riil." },
  { "en": "Osilasi Memerlukan Setidaknya?", "id": "Dua Komponen Penyimpan Energi (Orde 2)." },
  { "en": "Apa Itu Model Ideal?", "id": "Penyederhanaan Sistem Nyata Untuk Analisis." },
  { "en": "Komponen RLC Adalah?", "id": "Model Ideal Untuk Komponen Rangkaian Sebenarnya." },
  { "en": "Transformasi Laplace Bekerja Dengan?", "id": "Model Matematis Ideal Dari Sistem Fisik." },
  { "en": "Validitas Hasil Tergantung Pada?", "id": "Seberapa Baik Model Mewakili Sistem Nyata." },
  { "en": "Apa Itu Analisis Asimtotik?", "id": "Mempelajari Perilaku Fungsi Pada Nilai Ekstrem." },
  { "en": "Teorema Nilai Awal/Akhir Adalah?", "id": "Contoh Alat Untuk Analisis Asimtotik." },
  { "en": "Perilaku s -> 0 Merepresentasikan?", "id": "Perilaku Frekuensi Rendah Atau Jangka Panjang." },
  { "en": "Perilaku s -> ∞ Merepresentasikan?", "id": "Perilaku Frekuensi Tinggi Atau Jangka Pendek." },
  { "en": "Apa Nama Lain Sifat Aditif?", "id": "Respon Jumlah Input Adalah Jumlah Respon." },
  { "en": "Apa Nama Lain Sifat Homogen?", "id": "Penskalaan Input Akan Menskalakan Output." },
  { "en": "Kedua Sifat Ini Adalah Definisi?", "id": "Sistem Linier Yang Bisa Dianalisis." },
  { "en": "Kenapa Kernel e^-st Begitu Penting?", "id": "Karena Merupakan Fungsi Eigen Sistem LTI." },
  { "en": "Apa Arti Fisis Eigenfunction?", "id": "Bentuk Sinyal Tidak Berubah Oleh Sistem." },
  { "en": "Apa Yang Berubah Pada Eigenfunction?", "id": "Hanya Amplitudo Dan Fasa Sinyal." },
  { "en": "Perubahan Ini Dikuantifikasi Oleh?", "id": "Fungsi Transfer H(s) Pada Nilai s." },
  { "en": "Kapan Sebaiknya Teorema Nilai Akhir Dipakai?", "id": "Saat Hanya Butuh Nilai Tunak Cepat." },
  { "en": "Apa Kelemahan Teorema Nilai Akhir?", "id": "Tidak Memberi Informasi Respon Transien." },
  { "en": "Bagaimana Rasio Redaman Mempengaruhi Overshoot?", "id": "Rasio Redaman Kecil Sebabkan Overshoot Besar." },
  { "en": "Bagaimana Frekuensi Natural Mempengaruhi Kecepatan?", "id": "Frekuensi Natural Besar Respon Lebih Cepat." },
  { "en": "Garis Horizontal Di Bidang-s Artinya?", "id": "Sekumpulan Sistem Dengan Redaman Yang Sama." },
  { "en": "Garis Vertikal Di Bidang-s Artinya?", "id": "Sekumpulan Sistem Dengan Frekuensi Osilasi Sama." },
  { "en": "Garis Radial Dari Origin Artinya?", "id": "Sekumpulan Sistem Dengan Rasio Redaman Sama." },
  { "en": "Bisakah Laplace Menyelesaikan Persamaan Non-Linier?", "id": "Tidak Hanya Untuk Sistem Atau Persamaan Linier." },
  { "en": "Bisakah Laplace Untuk Sistem Berubah Waktu?", "id": "Tidak Hanya Untuk Sistem Invarian Waktu." },
  { "en": "Apa Itu Sistem Invarian Waktu?", "id": "Perilakunya Tidak Berubah Jika Waktu Bergeser." },
  { "en": "Apa Itu Linearitas Terhadap Transformasi?", "id": "Transformasi Dari Jumlah Adalah Jumlah Transformasi." },
  { "en": "Apa Beda Dengan Linearitas Sistem?", "id": "Linearitas Sistem Tentang Perilaku Input-Output." },
  { "en": "Apa Yang Terjadi Jika Pole Di Origin?", "id": "Sistem Akan Berintegrasi (Menjumlahkan) Input." },
  { "en": "Apa Yang Terjadi Jika Zero Di Origin?", "id": "Sistem Akan Berdiferensiasi (Mencari Laju Perubahan)." },
  { "en": "Respon Impuls Dari Integrator?", "id": "Fungsi Step Satuan u(t)." },
  { "en": "Respon Impuls Dari Diferensiator?", "id": "Fungsi Doublet (Turunan Dari Impuls)." },
  { "en": "Apakah Solusi Total Selalu Ada?", "id": "Ya Selama Sistemnya Linier Dan LTI." },
  { "en": "Apa Itu Mode Tersembunyi (Hidden Mode)?", "id": "Mode Akibat Pembatalan Pole-Zero." },
  { "en": "Apakah Mode Tersembunyi Masih Ada?", "id": "Ya Masih Ada Di Dalam Sistem." },
  { "en": "Kenapa Mode Tersembunyi Tidak Terlihat?", "id": "Karena Tidak Terpicu Input Atau Terlihat Output." },
  { "en": "Apa Bahaya Mode Tersembunyi?", "id": "Mode Tak Stabil Bisa Tetap Ada Tersembunyi." },
  { "en": "Apa Itu Stabilitas Internal?", "id": "Semua Mode Internal Sistem Harus Stabil." },
  { "en": "Stabilitas BIBO Terkait Dengan?", "id": "Hanya Hubungan Antara Input Dan Output." },
  { "en": "Stabilitas Internal Lebih Kuat?", "id": "Ya Menjamin Tidak Ada Perilaku Buruk Tersembunyi." },
  { "en": "Bagaimana Melihat Stabilitas Internal?", "id": "Dari Pole Sebelum Adanya Pembatalan Pole-Zero." },
  { "en": "Apa Transformasi Dari Fungsi Konstan K?", "id": "K Dibagi Dengan Variabel s (K/s)." },
  { "en": "Apa Transformasi Dari Sinyal K*δ(t)?", "id": "Konstanta K Di Domain Frekuensi." },
  { "en": "Apa Esensi Dari Analisis Frekuensi?", "id": "Memecah Sinyal Kompleks Menjadi Komponen Sinusoidal." },
  { "en": "Laplace Adalah Perluasan Dari Analisis?", "id": "Analisis Fourier Dengan Memasukkan Faktor Redaman." },
  { "en": "Apa Itu Sinyal Kausalitas Kuat?", "id": "Sistem Kausal Yang Responnya Kontinu." },
  { "en": "Syarat Kausalitas Kuat?", "id": "Derajat Penyebut Minimal Satu Lebih Besar." },
  { "en": "Apa Itu Sinyal Energi?", "id": "Sinyal Dengan Energi Total Yang Terbatas." },
  { "en": "Apa Itu Sinyal Daya?", "id": "Sinyal Dengan Daya Rata-Rata Yang Terbatas." },
  { "en": "Fungsi Step Adalah Sinyal?", "id": "Sinyal Daya Bukan Sinyal Energi." },
  { "en": "Pulsa Persegi Adalah Sinyal?", "id": "Sinyal Energi Bukan Sinyal Daya." },
  { "en": "Fourier Lebih Cocok Untuk?", "id": "Analisis Sinyal Daya (Steady-State)." },
  { "en": "Laplace Lebih Cocok Untuk?", "id": "Analisis Sinyal Energi (Transien)." },
  { "en": "Apa Itu Ruang Vektor?", "id": "Kumpulan Objek (Vektor) Dengan Aturan Penjumlahan." },
  { "en": "Fungsi f(t) Bisa Dilihat Sebagai?", "id": "Vektor Dalam Ruang Fungsi Berdimensi Tak Hingga." },
  { "en": "Transformasi Laplace Adalah Pemetaan?", "id": "Pemetaan Linier Antara Dua Ruang Fungsi." },
  { "en": "Apa Efek Memindahkan Pole Ke Kiri?", "id": "Membuat Respon Transien Meredam Lebih Cepat." },
  { "en": "Apa Efek Memindahkan Zero Ke Kiri?", "id": "Mengurangi Overshoot Atau Efek Diferensial." },
  { "en": "Apa Itu Aksi Kontrol Proporsional?", "id": "Output Kontroler Sebanding Dengan Error." },
  { "en": "Apa Itu Aksi Kontrol Integral?", "id": "Output Kontroler Sebanding Integral Error." },
  { "en": "Apa Itu Aksi Kontrol Derivatif?", "id": "Output Kontroler Sebanding Turunan Error." },
  { "en": "Setiap Aksi Kontrol Ini Mempengaruhi?", "id": "Posisi Pole Dan Zero Dari Loop Tertutup." },
  { "en": "Tujuan Desain Kontroler?", "id": "Menempatkan Pole Loop Tertutup Di Lokasi Diinginkan." },
  { "en": "Lokasi Diinginkan Ditentukan Oleh?", "id": "Spesifikasi Kinerja Seperti Overshoot Rise Time." },
  { "en": "Apa Itu Respon Impuls h(t) = 0?", "id": "Sistem Nol Yang Tidak Merespon Apapun." },
  { "en": "Fungsi Transfer Untuk h(t) = 0?", "id": "H(s) = 0 Untuk Semua s." },
  { "en": "Apa Itu Respon Impuls h(t) = δ(t)?", "id": "Sistem Identitas Output Sama Dengan Input." },
  { "en": "Fungsi Transfer Untuk h(t) = δ(t)?", "id": "H(s) = 1 Untuk Semua s." },
  { "en": "Apa Itu Sistem Statis?", "id": "Sistem Tanpa Memori (Memoryless System)." },
  { "en": "Apa Itu Sistem Dinamis?", "id": "Sistem Dengan Memori (Memiliki Komponen Penyimpan Energi)." },
  { "en": "Laplace Digunakan Untuk Menganalisis?", "id": "Sistem Dinamis Linier Invarian Waktu." },
  { "en": "Apa Itu Orde Relatif?", "id": "Selisih Antara Derajat Penyebut Dan Pembilang." },
  { "en": "Orde Relatif Menentukan?", "id": "Perilaku Frekuensi Tinggi Dan Kausalitas Kuat." },
  { "en": "Orde Relatif Harus Berapa Agar Fisis?", "id": "Harus Lebih Besar Atau Sama Dengan Nol." },
  { "en": "Apa Itu Matriks Transisi Keadaan?", "id": "Matriks e Pangkat At Dalam Analisis State-Space." },
  { "en": "Transformasi Laplace Matriks Ini?", "id": "Menghasilkan Matriks (sI - A) Invers." },
  { "en": "Ini Menghubungkan State-Space Dengan?", "id": "Metode Fungsi Transfer Menggunakan Transformasi Laplace." },
  { "en": "Apa Syarat Awal Untuk Fungsi Transfer?", "id": "Sistem Harus Dalam Keadaan Diam (At Rest)." },
  { "en": "Keadaan Diam Berarti Kondisi Awal?", "id": "Semua Kondisi Awal Bernilai Nol." },
  { "en": "Apa Itu Dekomposisi Fraksi Parsial?", "id": "Sama Dengan Ekspansi Fraksi Parsial." },
  { "en": "Metode Cover-Up Heaviside Untuk?", "id": "Mencari Koefisien Pole Sederhana Riil." },
  { "en": "Apa Itu Konstanta Gain Statis?", "id": "Sama Dengan Gain DC Dari Sistem." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi Menahan Frekuensi Rendah." },
  { "en": "Ciri Filter High-Pass?", "id": "Memiliki Zero Di Titik Asal (s=0)." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Frekuensi Dalam Pita Tertentu." },
  { "en": "Ciri Filter Band-Pass?", "id": "Memiliki Pole Kompleks Dan Zero Di Origin." },
  { "en": "Apa Itu Filter Band-Stop?", "id": "Menolak Frekuensi Dalam Pita Tertentu." },
  { "en": "Ciri Filter Band-Stop?", "id": "Memiliki Zero Kompleks Di Sumbu Imajiner." },
  { "en": "Apa Itu Sinyal Deterministik?", "id": "Sinyal Yang Perilakunya Bisa Diprediksi Tepat." },
  { "en": "Apa Itu Sinyal Acak (Stokastik)?", "id": "Sinyal Yang Perilakunya Memiliki Ketidakpastian." },
  { "en": "Laplace Digunakan Untuk Sinyal?", "id": "Sinyal Deterministik Yang Memiliki Bentuk Matematis." },
  { "en": "Analisis Sinyal Acak Menggunakan?", "id": "Statistik Seperti Rata-Rata Dan Variansi." },
  { "en": "Apa Itu Fungsi Autokorelasi?", "id": "Mengukur Kemiripan Sinyal Dengan Versi Gesernya." },
  { "en": "Teorema Wiener-Khinchin Menghubungkan?", "id": "Autokorelasi Dengan Kerapatan Spektral Daya." },
  { "en": "Analisis Ini Termasuk Dalam?", "id": "Pemrosesan Sinyal Statistik Yang Lebih Maju." },
  { "en": "Apa Itu Titik Setimbang (Equilibrium Point)?", "id": "Keadaan Dimana Sistem Akan Berdiam." },
  { "en": "Stabilitas Terkait Dengan Perilaku?", "id": "Sistem Di Sekitar Titik Setimbangnya." },
  { "en": "Sistem Stabil Akan?", "id": "Kembali Ke Titik Setimbang Setelah Diganggu." },
  { "en": "Sistem Tidak Stabil Akan?", "id": "Menjauh Dari Titik Setimbang Setelah Diganggu." },
  { "en": "Apa Itu Linearized System?", "id": "Aproksimasi Linier Dari Sistem Non-Linier." },
  { "en": "Di Mana Aproksimasi Ini Dilakukan?", "id": "Di Sekitar Titik Operasi Atau Titik Setimbang." },
  { "en": "Setelah Dilinearisasi Apa Yang Dilakukan?", "id": "Analisis Menggunakan Transformasi Laplace Bisa Diterapkan." },
  { "en": "Jadi Laplace Bisa Untuk Non-Linier?", "id": "Secara Tidak Langsung Pada Model Linearisasinya." },
  { "en": "Apa Itu Bidang Fasa (Phase Plane)?", "id": "Alat Analisis Untuk Sistem Non-Linier Orde Dua." },
  { "en": "Apakah Bidang Fasa Sama Dengan Bidang-s?", "id": "Tidak Sama Sekali Keduanya Konsep Berbeda." },
  { "en": "Bidang Fasa Memplot Apa?", "id": "Variabel Keadaan Satu Terhadap Yang Lain." },
  { "en": "Bidang-s Memplot Apa?", "id": "Lokasi Pole Dan Zero Di Domain Kompleks." },
  { "en": "Apa Beda Respon Impuls Dan Respon Step?", "id": "Impuls Uji Karakter Step Uji Perilaku." },
  { "en": "Apa Beda Stabilitas BIBO Dan Stabilitas Asimtotik?", "id": "BIBO Tentang I/O Asimtotik Tentang Mode Internal." },
  { "en": "Apa Beda Rasio Redaman Dan Frekuensi Natural?", "id": "Rasio Redaman Bentuk Frekuensi Natural Kecepatan." },
  { "en": "Apa Beda Teorema Nilai Awal Dan Akhir?", "id": "Awal Untuk t=0 Akhir Untuk t=Tak Hingga." },
  { "en": "Apa Beda Transformasi Laplace Dan Deret Fourier?", "id": "Laplace Untuk Transien Fourier Untuk Sinyal Periodik." },
  { "en": "Apa Beda Sistem Waktu Kontinu Dan Diskrit?", "id": "Kontinu Untuk Semua Waktu Diskrit Interval Tertentu." },
  { "en": "Apa Beda Fungsi Transfer Dan Respon Impuls?", "id": "Fungsi Transfer Adalah Transformasi Respon Impuls." },
  { "en": "Apa Beda Persamaan Diferensial Dan Aljabar?", "id": "Diferensial Libatkan Turunan Aljabar Tidak." },
  { "en": "Apa Beda Pole Riil Dan Pole Kompleks?", "id": "Riil Untuk Peluruhan Kompleks Untuk Osilasi." },
  { "en": "Apa Beda Sumbu Riil Dan Sumbu Imajiner?", "id": "Riil Untuk Redaman Imajiner Untuk Osilasi." },
  { "en": "Kenapa Kondisi Awal Turunan Diperlukan?", "id": "Untuk Mendefinisikan Keadaan Sistem Secara Lengkap." },
  { "en": "Kenapa Penyebut Fungsi Transfer Sangat Penting?", "id": "Karena Menentukan Semua Pole Dan Stabilitas Sistem." },
  { "en": "Kenapa Pembilang Fungsi Transfer Penting?", "id": "Karena Menentukan Bagaimana Input Mempengaruhi Sistem." },
  { "en": "Kenapa Kita Membutuhkan Transformasi Balik?", "id": "Untuk Mengembalikan Solusi Ke Domain Waktu Nyata." },
  { "en": "Kenapa Metode Fraksi Parsial Dibutuhkan?", "id": "Agar F(s) Cocok Dengan Bentuk Tabel." },
  { "en": "Kenapa Sistem Fisik Harus Kausal?", "id": "Karena Akibat Tidak Bisa Mendahului Penyebab." },
  { "en": "Kenapa Stabilitas Sangat Penting Dalam Desain?", "id": "Agar Sistem Tidak Tumbuh Menjadi Tak Terkendali." },
  { "en": "Kenapa Integral Definisi Dari Nol?", "id": "Karena Menganalisis Perilaku Sistem Setelah Dinyalakan." },
  { "en": "Kenapa Teorema Konvolusi Sangat Kuat?", "id": "Mengubah Operasi Integral Rumit Menjadi Perkalian." },
  { "en": "Kenapa Loop Umpan Balik Digunakan?", "id": "Untuk Menstabilkan Sistem Dan Mengurangi Error." },
  { "en": "Apa Itu Domain Waktu?", "id": "Domain Dimana Sinyal Adalah Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Domain Dimana Sinyal Adalah Fungsi Frekuensi." },
  { "en": "Apa Hubungan Fundamental Keduanya?", "id": "Dihubungkan Oleh Transformasi Laplace Dan Baliknya." },
  { "en": "Apa Itu Linear Time-Invariant System?", "id": "Sistem Linier Yang Sifatnya Tidak Berubah." },
  { "en": "Kapan Suatu Sistem Disebut Linier?", "id": "Jika Memenuhi Prinsip Superposisi (Aditif Homogen)." },
  { "en": "Kapan Sistem Disebut Invarian Waktu?", "id": "Input Yang Ditunda Hasilkan Output Tertunda Sama." },
  { "en": "Bagaimana Laplace Merepresentasikan 'Memori' Sistem?", "id": "Melalui Operator Integral (1/s)." },
  { "en": "Bagaimana Laplace Merepresentasikan 'Antisipasi' Sistem?", "id": "Melalui Operator Turunan (s)." },
  { "en": "Apa Itu Solusi Analitik?", "id": "Solusi Dalam Bentuk Persamaan Matematika Tepat." },
  { "en": "Apa Itu Solusi Numerik?", "id": "Solusi Dalam Bentuk Aproksimasi Angka-Angka." },
  { "en": "Laplace Memberikan Solusi Jenis Apa?", "id": "Solusi Analitik Untuk Sistem LTI." },
  { "en": "Apa Yang Dimaksud Dengan Orde Sistem?", "id": "Jumlah Elemen Penyimpan Energi Independen." },
  { "en": "Contoh Elemen Penyimpan Energi Listrik?", "id": "Induktor (Energi Magnetik) Kapasitor (Energi Listrik)." },
  { "en": "Contoh Elemen Penyimpan Energi Mekanis?", "id": "Massa (Energi Kinetik) Pegas (Energi Potensial)." },
  { "en": "Orde Sistem Sama Dengan?", "id": "Pangkat Tertinggi Polinomial Karakteristik." },
  { "en": "Apa Itu Zero Finite?", "id": "Zero Yang Terletak Di Lokasi Terbatas." },
  { "en": "Apa Itu Zero Infinite?", "id": "Zero Yang Terletak Di Tak Hingga." },
  { "en": "Jika Derajat Penyebut > Pembilang?", "id": "Sistem Memiliki Zero Di Tak Hingga." },
  { "en": "Apa Makna Fisis Dari 'Frekuensi'?", "id": "Ukuran Seberapa Cepat Sesuatu Berosilasi." },
  { "en": "Apa Makna Fisis Dari 'Redaman'?", "id": "Ukuran Seberapa Cepat Osilasi Menghilang." },
  { "en": "Apa Itu Mode Tak Teramati?", "id": "Mode Sistem Yang Tidak Muncul Di Output." },
  { "en": "Apa Itu Mode Tak Terkontrol?", "id": "Mode Sistem Yang Tidak Bisa Dipengaruhi Input." },
  { "en": "Penyebab Mode Tak Teramati/Terkontrol?", "id": "Pembatalan Sempurna Antara Pole Dan Zero." },
  { "en": "Apa Itu Sistem SISO Dan MIMO?", "id": "Single-Input/Output Dan Multiple-Input/Output." },
  { "en": "Laplace Secara Tradisional Untuk Sistem?", "id": "Sistem SISO Yang Paling Umum." },
  { "en": "Apa Itu Kopling Antar Variabel?", "id": "Input Satu Mempengaruhi Output Yang Lain." },
  { "en": "Analisis Sistem MIMO Menggunakan?", "id": "Matriks Fungsi Transfer Di Domain Laplace." },
  { "en": "Setiap Elemen Matriks Adalah?", "id": "Fungsi Transfer Dari Satu Input Ke Output." },
  { "en": "Apa Itu Dekomposisi Modal?", "id": "Memecah Respon Sistem Menjadi Jumlah Modenya." },
  { "en": "Setiap Mode Terkait Dengan?", "id": "Satu Pole (Atau Sepasang Pole) Sistem." },
  { "en": "Apa Itu Koefisien Partisipasi?", "id": "Menunjukkan Seberapa Besar Kontribusi Setiap Mode." },
  { "en": "Koefisien Ini Dihitung Saat?", "id": "Melakukan Ekspansi Fraksi Parsial (Residu)." },
  { "en": "Apa Itu Fungsi Transfer Bipoler?", "id": "Fungsi Transfer Dengan Pole Dan Zero." },
  { "en": "Apa Itu Respon Impuls Hingga (FIR)?", "id": "Respon Impuls Menjadi Nol Setelah Waktu Tertentu." },
  { "en": "Sistem FIR Biasanya Adalah?", "id": "Sistem Waktu Diskrit (Digital)." },
  { "en": "Apa Itu Respon Impuls Tak Hingga (IIR)?", "id": "Respon Impuls Berlanjut Selamanya (Meredam)." },
  { "en": "Sistem Analog (Kontinu) Biasanya?", "id": "Sistem IIR Dideskripsikan Persamaan Diferensial." },
  { "en": "Apa Arti Fisis Teorema Nilai Awal?", "id": "Perilaku Rangkaian Sesaat Setelah Saklar Ditutup." },
  { "en": "Perilaku Awal Kapasitor?", "id": "Seperti Hubungan Singkat (Short Circuit)." },
  { "en": "Perilaku Awal Induktor?", "id": "Seperti Hubungan Terbuka (Open Circuit)." },
  { "en": "Apa Arti Fisis Teorema Nilai Akhir?", "id": "Perilaku Rangkaian Setelah Waktu Sangat Lama." },
  { "en": "Perilaku Akhir Kapasitor (DC)?", "id": "Seperti Hubungan Terbuka (Open Circuit)." },
  { "en": "Perilaku Akhir Induktor (DC)?", "id": "Seperti Hubungan Singkat (Short Circuit)." },
  { "en": "Apa Itu Konstanta Gain?", "id": "Faktor Pengali Yang Menskalakan Fungsi Transfer." },
  { "en": "Apa Itu Bentuk Kanonik?", "id": "Bentuk Standar Untuk Representasi Sistem." },
  { "en": "Contoh Bentuk Kanonik Kontroler?", "id": "Bentuk Kanonik Terkontrol Atau Teramati." },
  { "en": "Apa Arti Transformasi f(t-a)?", "id": "Ini Bukan Notasi Transformasi Laplace." },
  { "en": "Notasi Yang Benar Adalah?", "id": "L{f(t-a)u(t-a)}." },
  { "en": "Kenapa u(t-a) Diperlukan?", "id": "Untuk Memastikan Fungsi Nol Sebelum Pergeseran." },
  { "en": "Transformasi Dari sin(t)u(t-π)?", "id": "Menggunakan Sifat Pergeseran Dan Identitas Trigonometri." },
  { "en": "Apa Itu Spektrum Amplitudo?", "id": "Sama Dengan Spektrum Magnitudo." },
  { "en": "Apa Itu Spektrum Fasa?", "id": "Plot Sudut Fasa Terhadap Frekuensi." },
  { "en": "Apa Itu Distorsi Amplitudo?", "id": "Saat Gain Sistem Tidak Sama Untuk Semua Frekuensi." },
  { "en": "Apa Itu Distorsi Fasa?", "id": "Saat Pergeseran Fasa Tidak Linier Dengan Frekuensi." },
  { "en": "Sistem Tanpa Distorsi?", "id": "Memiliki Gain Konstan Dan Fasa Linier." },
  { "en": "Respon Impuls Sistem Tanpa Distorsi?", "id": "Impuls Yang Tertunda Dan Terskalakan." },
  { "en": "Apa Itu Konvergensi Absolut?", "id": "Integral Dari Nilai Absolut Fungsi Konvergen." },
  { "en": "Jika Integral Konvergen Absolut?", "id": "Maka Integral Asli Pasti Akan Konvergen." },
  { "en": "ROC Selalu Berbentuk?", "id": "Pita Vertikal Atau Setengah Bidang." },
  { "en": "Batas ROC Ditentukan Oleh?", "id": "Garis Vertikal Yang Melewati Pole." },
  { "en": "Apakah ROC Bagian Dari Transformasi?", "id": "Ya F(s) Tanpa ROC Tidak Lengkap." },
  { "en": "Apa Itu Sinyal Real-Valued?", "id": "Sinyal Yang Nilainya Selalu Bilangan Riil." },
  { "en": "Apa Itu Sinyal Complex-Valued?", "id": "Sinyal Yang Nilainya Bisa Bilangan Kompleks." },
  { "en": "Di Dunia Nyata Sinyal Selalu?", "id": "Sinyal Real-Valued Yang Bisa Diukur." },
  { "en": "Sinyal Kompleks Digunakan Untuk?", "id": "Penyederhanaan Analisis Matematis (Contohnya Fasor)." },
  { "en": "Pole-Zero Plot Adalah?", "id": "Sidik Jari Unik Dari Suatu Sistem." },
  { "en": "Dari Plot Pole-Zero Kita Tahu?", "id": "Stabilitas Tipe Respon Dan Karakteristik Lainnya." },
  { "en": "Apa Itu Sistem Kanan?", "id": "Sistem Kausal Yang Memiliki Sisi Kanan." },
  { "en": "Apa Itu Sistem Kiri?", "id": "Sistem Anti-Kausal Yang Memiliki Sisi Kiri." },
  { "en": "Apa Itu Teori Rangkaian?", "id": "Aplikasi Utama Pertama Dari Transformasi Laplace." },
  { "en": "Apa Itu Teori Kontrol?", "id": "Bidang Lainnya Yang Sangat Bergantung Pada Laplace." },
  { "en": "Apa Itu Pemrosesan Sinyal?", "id": "Bidang Dimana Laplace Dan Fourier Digunakan." },
  { "en": "Apa Itu Model Matematika?", "id": "Persamaan (Diferensial) Yang Mewakili Sistem Fisik." },
  { "en": "Langkah Pertama Dalam Analisis Sistem?", "id": "Menurunkan Model Matematika Yang Akurat." },
  { "en": "Langkah Selanjutnya Setelah Model?", "id": "Menganalisis Model Menggunakan Transformasi Laplace." },
  { "en": "Langkah Terakhir Setelah Analisis?", "id": "Interpretasi Hasil Dan Verifikasi Dengan Realitas." },
  { "en": "Apa Itu Simbol 's'?", "id": "Variabel Kompleks s = σ + jω." },
  { "en": "Apa Beda Respon Natural Dan Paksa?", "id": "Natural Dari Sistem Paksa Dari Input." },
  { "en": "Apa Beda Respon Transien Dan Tunak?", "id": "Transien Lenyap Tunak Bertahan Lama." },
  { "en": "Apa Beda Pole Di Kiri Dan Kanan?", "id": "Kiri Stabil Kanan Tidak Stabil." },
  { "en": "Apa Beda Sifat Turunan Waktu Dan Frekuensi?", "id": "Waktu Jadi 's' Frekuensi Jadi '-t'." },
  { "en": "Apa Beda Sifat Geser Waktu Dan Frekuensi?", "id": "Waktu Jadi e^-as Frekuensi Jadi F(s-a)." },
  { "en": "Apa Beda Orde Sistem Dan Tipe Sistem?", "id": "Orde Jumlah Pole Tipe Jumlah Integrator." },
  { "en": "Apa Beda Sistem Kausal Dan Non-Kausal?", "id": "Kausal Tergantung Masa Lalu Non-Kausal Masa Depan." },
  { "en": "Apa Beda Sistem Stabil Dan Stabil Marjinal?", "id": "Stabil Meredam Marjinal Osilasi Terus." },
  { "en": "Apa Beda Fungsi Transfer Dan Persamaan Keadaan?", "id": "Fungsi Transfer I/O State-Space Internal." },
  { "en": "Apa Beda Analisis Node Dan Mesh?", "id": "Node Berbasis Arus Mesh Berbasis Tegangan." },
  { "en": "Kapan Sistem Dikatakan Strictly Proper?", "id": "Saat Derajat Penyebut Lebih Besar Pembilang." },
  { "en": "Kapan Sistem Dikatakan Biproper?", "id": "Saat Derajat Penyebut Sama Dengan Pembilang." },
  { "en": "Apa Itu Gain Frekuensi Tinggi?", "id": "Gain Sistem Saat s Menuju Tak Hingga." },
  { "en": "Apa Itu Gain Frekuensi Rendah?", "id": "Sama Dengan Gain DC Saat s=0." },
  { "en": "Bagaimana Sifat Linearitas Digunakan?", "id": "Memecah Masalah Kompleks Menjadi Bagian Sederhana." },
  { "en": "Bagaimana Sifat Pergeseran Waktu Digunakan?", "id": "Untuk Menganalisis Sinyal Dengan Penundaan Waktu." },
  { "en": "Bagaimana Sifat Turunan Digunakan?", "id": "Mengubah Persamaan Diferensial Menjadi Aljabar." },
  { "en": "Bagaimana Sifat Integral Digunakan?", "id": "Mengubah Persamaan Integral Menjadi Aljabar." },
  { "en": "Bagaimana Teorema Konvolusi Digunakan?", "id": "Menghitung Respon Sistem Tanpa Integral Sulit." },
  { "en": "Bagaimana Fraksi Parsial Digunakan?", "id": "Untuk Mencocokkan F(s) Dengan Tabel Transformasi." },
  { "en": "Apa Arti Fisis Dari 's'?", "id": "Frekuensi Kompleks Mencakup Pertumbuhan Dan Osilasi." },
  { "en": "Apa Arti Fisis Dari σ (Sigma)?", "id": "Laju Pertumbuhan Atau Peluruhan Eksponensial." },
  { "en": "Apa Arti Fisis Dari ω (Omega)?", "id": "Frekuensi Osilasi Sinusoidal Dalam Radian." },
  { "en": "Apa Arti Fisis Dari Fungsi Transfer?", "id": "DNA Atau Karakteristik Bawaan Sistem LTI." },
  { "en": "Apa Arti Fisis Dari Respon Impuls?", "id": "Bagaimana Sistem 'Menendang Balik' Saat Dipukul." },
  { "en": "Apa Arti Fisis Dari Respon Step?", "id": "Bagaimana Sistem Bereaksi Terhadap Perubahan Mendadak." },
  { "en": "Apa Arti Fisis Dari Pole?", "id": "Frekuensi Getaran Alami Internal Sistem." },
  { "en": "Apa Arti Fisis Dari Zero?", "id": "Frekuensi Dimana Sistem Memblokir Transmisi Sinyal." },
  { "en": "Apa Arti Fisis Dari Stabilitas?", "id": "Sistem Tidak Akan Meledak Dengan Sendirinya." },
  { "en": "Apa Arti Fisis Dari Umpan Balik?", "id": "Menggunakan Output Untuk Mengoreksi Perilaku Sistem." },
  { "en": "Bagaimana Laplace Merepresentasikan Energi Kinetik?", "id": "Melalui Impedansi Induktor Sebesar sL." },
  { "en": "Bagaimana Laplace Merepresentasikan Energi Potensial?", "id": "Melalui Impedansi Kapasitor Sebesar 1/sC." },
  { "en": "Bagaimana Laplace Merepresentasikan Disipasi Energi?", "id": "Melalui Impedansi Resistor Sebesar R." },
  { "en": "Apa Itu Sistem Minimum Phase?", "id": "Semua Pole Dan Zero Di Sisi Kiri." },
  { "en": "Apa Sifat Sistem Minimum Phase?", "id": "Respon Fasa Paling Minimal Untuk Gain Tertentu." },
  { "en": "Apa Itu Sistem Non-Minimum Phase?", "id": "Memiliki Zero Di Sisi Kanan Bidang-s." },
  { "en": "Apa Sifat Sistem Non-Minimum Phase?", "id": "Menunjukkan Respon Awal Yang Berlawanan (Undershoot)." },
  { "en": "Apa Itu Sistem Band-Limited?", "id": "Sinyal Dengan Kandungan Frekuensi Terbatas." },
  { "en": "Apa Itu Aliasing?", "id": "Kesalahan Akibat Sampling Terlalu Lambat." },
  { "en": "Laplace Terkait Dengan Aliasing?", "id": "Tidak Aliasing Isu Dalam Sistem Diskrit." },
  { "en": "Fungsi Transfer Menghubungkan Apa?", "id": "Output Y(s) Dengan Input X(s)." },
  { "en": "Y(s) Sama Dengan?", "id": "H(s) Dikalikan Dengan X(s)." },
  { "en": "X(s) Sama Dengan?", "id": "Y(s) Dibagi Dengan H(s)." },
  { "en": "H(s) Sama Dengan?", "id": "Y(s) Dibagi Dengan X(s)." },
  { "en": "Hubungan Ini Berlaku Jika?", "id": "Semua Kondisi Awal Sistem Adalah Nol." },
  { "en": "Apa Itu Persamaan Diferensial Biasa (ODE)?", "id": "Persamaan Diferensial Dengan Satu Variabel Independen." },
  { "en": "Laplace Biasanya Untuk Menyelesaikan?", "id": "Persamaan Diferensial Biasa Linier Koefisien Konstan." },
  { "en": "Apa Itu Persamaan Diferensial Parsial (PDE)?", "id": "Persamaan Dengan Beberapa Variabel Independen." },
  { "en": "Laplace Bisa Untuk PDE?", "id": "Ya Mengubah Satu Variabel Menjadi Aljabar." },
  { "en": "Apa Itu Respon Frekuensi Kompleks?", "id": "Nilai H(s) Untuk Nilai s Tertentu." },
  { "en": "Apa Itu Respon Frekuensi Sinusoidal?", "id": "Nilai H(s) Saat s = jω." },
  { "en": "Magnitudo H(jω) Menunjukkan?", "id": "Penguatan Amplitudo Pada Frekuensi Omega." },
  { "en": "Fasa H(jω) Menunjukkan?", "id": "Pergeseran Fasa Pada Frekuensi Omega." },
  { "en": "Apa Itu Diagram Argand?", "id": "Sama Dengan Bidang Kompleks Atau Bidang-s." },
  { "en": "Bagaimana Cara Memvisualisasikan Fungsi Transfer?", "id": "Dengan Permukaan 3D Di Atas Bidang-s." },
  { "en": "Ketinggian Permukaan 3D Adalah?", "id": "Magnitudo Dari Fungsi Transfer |H(s)|." },
  { "en": "Di Lokasi Pole Permukaan?", "id": "Menjulang Tinggi Menuju Tak Terhingga." },
  { "en": "Di Lokasi Zero Permukaan?", "id": "Menyentuh Bidang-s (Bernilai Nol)." },
  { "en": "Respon Frekuensi Adalah Irisan?", "id": "Permukaan 3D Ini Sepanjang Sumbu Imajiner." },
  { "en": "Apa Itu Sinyal Abadi (Eternal Signal)?", "id": "Sinyal Yang Ada Dari Minus Tak Hingga." },
  { "en": "Laplace Bilateral Untuk Sinyal?", "id": "Sinyal Abadi Atau Sinyal Non-Kausal." },
  { "en": "Laplace Unilateral Untuk Sinyal?", "id": "Sinyal Kausal Yang Mulai Di Waktu Nol." },
  { "en": "Apa Itu Teorema Pergeseran Real?", "id": "Nama Lain Untuk Sifat Pergeseran Waktu." },
  { "en": "Apa Itu Teorema Pergeseran Kompleks?", "id": "Nama Lain Sifat Pergeseran Frekuensi." },
  { "en": "Apakah Laplace Bisa Untuk Fungsi Periodik?", "id": "Ya Dengan Rumus Khusus Yang Melibatkan Periode." },
  { "en": "Rumus Melibatkan Transformasi Dari?", "id": "Hanya Satu Siklus Pertama Dari Sinyal." },
  { "en": "Lalu Dibagi Dengan Faktor?", "id": "Satu Dikurangi e Pangkat Negatif sT." },
  { "en": "Faktor Ini Berasal Dari?", "id": "Penjumlahan Deret Geometri Tak Terhingga." },
  { "en": "Apa Syarat Awal f(0⁻)?", "id": "Nilai Fungsi Sesaat Sebelum Waktu Nol." },
  { "en": "Apa Syarat Awal f(0⁺)?", "id": "Nilai Fungsi Sesaat Setelah Waktu Nol." },
  { "en": "Yang Digunakan Di Definisi Turunan?", "id": "Nilai f(0⁻) Sesaat Sebelum Perubahan." },
  { "en": "Kenapa f(0⁻) Dipakai?", "id": "Karena Mewakili Energi Tersimpan Sebelum Saklar Aktif." },
  { "en": "Apa Itu Sistem Lossless?", "id": "Sistem Ideal Tanpa Disipasi Energi." },
  { "en": "Semua Pole Sistem Lossless?", "id": "Berada Tepat Di Sumbu Imajiner." },
  { "en": "Contoh Sistem Lossless?", "id": "Rangkaian LC Ideal Atau Pendulum Tanpa Gesekan." },
  { "en": "Apa Itu Respon Alami?", "id": "Perilaku Sistem Jika Dibiarkan Sendiri." },
  { "en": "Respon Alami Ditentukan Oleh?", "id": "Lokasi Pole-Pole Dari Sistem." },
  { "en": "Apa Itu Respon Paksa?", "id": "Perilaku Sistem Akibat Diberi Gaya Eksternal." },
  { "en": "Bentuk Respon Paksa Ditentukan Oleh?", "id": "Bentuk Sinyal Input Yang Diberikan." },
  { "en": "Apa Itu Impedansi Titik Penggerak?", "id": "Impedansi Dilihat Dari Terminal Input." },
  { "en": "Apa Itu Fungsi Transfer Titik Penggerak?", "id": "Fungsi Dimana Input Output Di Tempat Sama." },
  { "en": "Apa Itu Jaringan Pasif?", "id": "Jaringan Yang Hanya Terdiri Dari R L C." },
  { "en": "Pole Jaringan Pasif?", "id": "Selalu Berada Di Sisi Kiri Bidang-s." },
  { "en": "Jaringan Pasif Selalu Stabil?", "id": "Ya Selalu Stabil Secara Internal." },
  { "en": "Apa Itu Jaringan Aktif?", "id": "Jaringan Yang Mengandung Sumber Energi (Op-Amp)." },
  { "en": "Jaringan Aktif Bisa Tidak Stabil?", "id": "Ya Bisa Didesain Untuk Jadi Osilator." },
  { "en": "Osilator Adalah Sistem?", "id": "Sistem Tidak Stabil Marjinal Dengan Pole Imajiner." },
  { "en": "Apa Beda Transformasi Dan Deret?", "id": "Transformasi Untuk Sinyal Non-Periodik Deret Periodik." },
  { "en": "Deret Fourier Adalah Representasi?", "id": "Sinyal Periodik Sebagai Jumlah Sinusoid Harmonik." },
  { "en": "Transformasi Fourier Adalah Representasi?", "id": "Sinyal Non-Periodik Sebagai Integral Sinusoid." },
  { "en": "Laplace Adalah Representasi?", "id": "Sinyal Sebagai Integral Sinusoid Teredam." },
  { "en": "Faktor Redaman σ Memungkinkan Analisis?", "id": "Sinyal Yang Tumbuh Dan Meredam (Transien)." },
  { "en": "Sumbu σ Mewakili?", "id": "Dunia Peluruhan Eksponensial Dalam Sistem." },
  { "en": "Sumbu jω Mewakili?", "id": "Dunia Osilasi Murni (Dunia Fourier)." },
  { "en": "Bidang-s Menggabungkan Kedua Dunia?", "id": "Ya Menggabungkan Analisis Peluruhan Dan Osilasi." },
  { "en": "Apa Itu Solusi Lengkap?", "id": "Solusi Yang Mencakup Komponen Transien Dan Tunak." },
  { "en": "Laplace Secara Otomatis Memberikan?", "id": "Solusi Lengkap Dalam Satu Kali Perhitungan." },
  { "en": "Apa Arti Kata 'Unilateral'?", "id": "Transformasi Yang Memandang Satu Sisi (Waktu Positif)." },
  { "en": "Apa Arti Kata 'Bilateral'?", "id": "Transformasi Yang Memandang Dua Sisi Waktu." },
  { "en": "Apa Arti Fungsi 'Proper' Rasional?", "id": "Derajat Atas Lebih Kecil Sama Dengan Bawah." },
  { "en": "Apa Arti Fungsi 'Strictly Proper'?", "id": "Derajat Atas Lebih Kecil Dari Bawah." },
  { "en": "Pole Di Kanan Menyebabkan Respon?", "id": "Respon Tumbuh Secara Eksponensial Tanpa Batas." },
  { "en": "Respon Tak Terbatas Menyebabkan Sistem?", "id": "Menjadi Gagal Rusak Atau Tidak Berguna." },
  { "en": "Zero Di Kanan Menyebabkan Respon?", "id": "Undershoot Atau Respon Awal Yang Aneh." },
  { "en": "Respon Undershoot Membuat Kontrol?", "id": "Menjadi Lebih Sulit Dan Kurang Intuitif." },
  { "en": "Apa Peran 'j' Dalam s=σ+jω?", "id": "Penanda Satuan Imajiner Untuk Komponen Osilasi." },
  { "en": "Apa Peran 'dt' Dalam Integral Laplace?", "id": "Elemen Diferensial Waktu Yang Sangat Kecil." },
  { "en": "Fungsi Waktu Apa Dengan Transformasi 1/s²?", "id": "Fungsi Ramp t Atau r(t)." },
  { "en": "Fungsi Waktu Apa Dengan Transformasi 1/(s-a)?", "id": "Fungsi Eksponensial e Pangkat at." },
  { "en": "Apa Hubungan Pole Dengan Nilai Eigen?", "id": "Pole Adalah Nilai Eigen Matriks Dinamika Sistem." },
  { "en": "Apa Hubungan Respon Impuls Dengan Fungsi Green?", "id": "Respon Impuls Adalah Contoh Fungsi Green." },
  { "en": "Berapa Orde Sistem Rangkaian RC?", "id": "Sistem Orde Pertama Karena Satu Penyimpan Energi." },
  { "en": "Berapa Orde Sistem Rangkaian RLC?", "id": "Sistem Orde Kedua Dua Penyimpan Energi." },
  { "en": "Berapa Orde Sistem Massa-Pegas?", "id": "Sistem Orde Kedua (Energi Kinetik Potensial)." },
  { "en": "Apa Efek Penambahan Redaman?", "id": "Memindahkan Pole Kompleks Mendekati Sumbu Riil." },
  { "en": "Apa Efek Pengurangan Massa?", "id": "Meningkatkan Frekuensi Natural Sistem." },
  { "en": "Apa Itu Identitas Sistem?", "id": "Proses Menemukan Model Matematis Dari Data." },
  { "en": "Bagaimana Identitas Sistem Dilakukan?", "id": "Memberi Input Mengukur Output Mencari H(s)." },
  { "en": "Apa Itu Kurva Respon Frekuensi?", "id": "Plot Gain Dan Fasa Terhadap Frekuensi." },
  { "en": "Dari Kurva Ini Bisakah H(s) Ditemukan?", "id": "Ya Prosesnya Disebut Sintesis Jaringan." },
  { "en": "Apa Itu Logika Dasar Umpan Balik?", "id": "Mengukur Membandingkan Dan Mengoreksi Perilaku." },
  { "en": "Apa Yang Diukur Dalam Umpan Balik?", "id": "Sinyal Output Aktual Dari Sistem." },
  { "en": "Apa Yang Dibandingkan Dalam Umpan Balik?", "id": "Output Aktual Dengan Sinyal Referensi Diinginkan." },
  { "en": "Apa Hasil Perbandingan Ini?", "id": "Sinyal Error Atau Kesalahan Yang Terjadi." },
  { "en": "Apa Yang Dikoreksi Oleh Kontroler?", "id": "Kontroler Beraksi Berdasarkan Sinyal Error." },
  { "en": "Seluruh Analisis Ini Dilakukan Di?", "id": "Domain-s Menggunakan Aljabar Fungsi Transfer." },
  { "en": "Apa Kelemahan Transformasi Laplace?", "id": "Tidak Bisa Digunakan Untuk Sistem Non-Linier." },
  { "en": "Contoh Sistem Non-Linier?", "id": "Sistem Dengan Saturasi Gesekan Atau Histeresis." },
  { "en": "Apa Itu Saturasi?", "id": "Output Terbatas Walaupun Input Terus Bertambah." },
  { "en": "Bagaimana Menganalisis Sistem Non-Linier?", "id": "Menggunakan Linearisasi Atau Simulasi Domain Waktu." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Dalam Waktu Dan Amplitudo." },
  { "en": "Laplace Adalah Alat Untuk Dunia?", "id": "Dunia Analog Atau Sinyal Waktu Kontinu." },
  { "en": "Transformasi-Z Adalah Alat Untuk Dunia?", "id": "Dunia Digital Atau Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Konverter Analog-ke-Digital (ADC)?", "id": "Jembatan Antara Dunia Analog Dan Digital." },
  { "en": "Apa Itu Konverter Digital-ke-Analog (DAC)?", "id": "Jembatan Dari Dunia Digital Kembali Ke Analog." },
  { "en": "Apa Arti Respon Impuls Meredam?", "id": "Menandakan Bahwa Sistem Tersebut Stabil." },
  { "en": "Apa Arti Respon Impuls Tumbuh?", "id": "Menandakan Bahwa Sistem Tersebut Tidak Stabil." },
  { "en": "Apa Arti Respon Impuls Osilasi Konstan?", "id": "Menandakan Sistem Stabil Marjinal." },
  { "en": "Jadi h(t) Mencerminkan Stabilitas?", "id": "Ya Perilaku h(t) Menunjukkan Stabilitas Sistem." },
  { "en": "Hubungan h(t) Dan Pole?", "id": "Bentuk h(t) Ditentukan Oleh Pole H(s)." },
  { "en": "Apa Itu Teorema Konvolusi Grafis?", "id": "Metode Visual Untuk Menghitung Integral Konvolusi." },
  { "en": "Langkah-Langkah Konvolusi Grafis?", "id": "Membalik Menggeser Mengalikan Dan Mengintegralkan." },
  { "en": "Apa Itu Respon Sistem Terhadap Dirinya Sendiri?", "id": "Tidak Ada Konsep Seperti Itu." },
  { "en": "Sistem Merespon Terhadap Apa?", "id": "Input Eksternal Dan Kondisi Awal Internal." },
  { "en": "Apa Beda Model Fisik Dan Matematis?", "id": "Fisik Benda Nyata Matematis Adalah Persamaan." },
  { "en": "Laplace Bekerja Pada Level Apa?", "id": "Bekerja Pada Model Matematika Sistem." },
  { "en": "Apa Itu Parameter Terpusat (Lumped)?", "id": "Asumsi Komponen Ideal Di Satu Titik." },
  { "en": "Contoh Parameter Terpusat?", "id": "Resistor Kapasitor Induktor Ideal." },
  { "en": "Apa Itu Parameter Terdistribusi?", "id": "Parameter Tersebar Di Sepanjang Benda Fisik." },
  { "en": "Contoh Parameter Terdistribusi?", "id": "Resistansi Dan Kapasitansi Saluran Transmisi." },
  { "en": "Laplace Lebih Mudah Untuk Sistem?", "id": "Sistem Parameter Terpusat Dengan ODE." },
  { "en": "Analisis Sistem Terdistribusi Menghasilkan?", "id": "Fungsi Transfer Irasional (Akar Kuadrat s)." },
  { "en": "Apa Sifat Unik Transformasi Laplace?", "id": "Korespondensi Satu-ke-Satu Antara f(t) Dan F(s)." },
  { "en": "Korespondensi Ini Memerlukan Spesifikasi?", "id": "Region Of Convergence (ROC) Yang Tepat." },
  { "en": "Bagaimana Cara Cek Hasil Transformasi Balik?", "id": "Transformasikan Kembali Hasilnya Harus Kembali Ke F(s)." },
  { "en": "Bagaimana Cara Cek Hasil Transformasi?", "id": "Gunakan Teorema Nilai Awal Atau Akhir." },
  { "en": "Apa Arti Fisis Dari Residu?", "id": "Amplitudo Setiap Mode Dalam Respon Sistem." },
  { "en": "Residu Besar Menandakan?", "id": "Mode Tersebut Sangat Dominan Dalam Respon." },
  { "en": "Residu Kecil Menandakan?", "id": "Mode Tersebut Kurang Berpengaruh Pada Respon." },
  { "en": "Apa Itu Error Pembulatan (Round-off Error)?", "id": "Error Akibat Keterbatasan Presisi Komputer." },
  { "en": "Error Ini Mempengaruhi Analisis?", "id": "Analisis Numerik Bukan Analisis Simbolik Laplace." },
  { "en": "Apa Itu Analisis Simbolik?", "id": "Bekerja Dengan Variabel Matematika Bukan Angka." },
  { "en": "Laplace Adalah Bentuk Dari?", "id": "Analisis Simbolik Yang Sangat Kuat." },
  { "en": "Apa Itu Sinyal Kiri-Sided?", "id": "Sama Dengan Sinyal Sisi Kiri." },
  { "en": "Apa Itu Sinyal Kanan-Sided?", "id": "Sama Dengan Sinyal Sisi Kanan." },
  { "en": "Apa Itu Titik Singularitas Terisolasi?", "id": "Titik Singularitas Yang Tidak Punya Tetangga." },
  { "en": "Pole Adalah Contoh?", "id": "Singularitas Terisolasi Dari Fungsi Transfer." },
  { "en": "Apa Itu Analisis Kompleks?", "id": "Cabang Matematika Yang Mempelajari Fungsi Kompleks." },
  { "en": "Transformasi Laplace Sangat Bergantung Pada?", "id": "Hasil Dan Teorema Dari Analisis Kompleks." },
  { "en": "Apa Itu Pemetaan Konformal?", "id": "Transformasi Yang Mempertahankan Sudut Antar Kurva." },
  { "en": "Apakah Transformasi Laplace Konformal?", "id": "Ya Di Daerah Dimana Turunannya Tidak Nol." },
  { "en": "Apa Itu Jalur Integrasi Bromwich?", "id": "Jalur Vertikal Untuk Integral Transformasi Balik." },
  { "en": "Jalur Ini Harus Berada Di?", "id": "Dalam Region Of Convergence (ROC)." },
  { "en": "Apa Itu Kondisi Lipschitz?", "id": "Kondisi Matematis Untuk Keunikan Solusi ODE." },
  { "en": "Apa Itu Domain Hardy?", "id": "Ruang Fungsi Analitis Yang Penting." },
  { "en": "Apa Arti Fisis 'Linier'?", "id": "Hubungan Sebab-Akibat Yang Proporsional." },
  { "en": "Dua Kali Gaya Menghasilkan?", "id": "Dua Kali Perpindahan Dalam Sistem Linier." },
  { "en": "Apa Arti Fisis 'Invarian Waktu'?", "id": "Eksperimen Hari Ini Dan Besok Hasilnya Sama." },
  { "en": "Karakteristik Sistem Tidak Berubah?", "id": "Seiring Dengan Berjalannya Waktu." },
  { "en": "Sistem Nyata Selalu Invarian?", "id": "Tidak Komponen Bisa Menua Dan Berubah." },
  { "en": "Asumsi Invarian Waktu Adalah?", "id": "Penyederhanaan Yang Baik Untuk Banyak Kasus." },
  { "en": "Apa Itu Respon Steady-State?", "id": "Sama Dengan Respon Keadaan Tunak." },
  { "en": "Respon Steady-State Berbentuk Seperti?", "id": "Fungsi Input Yang Menggerakkan Sistem." },
  { "en": "Input Sinusoidal Menghasilkan Output?", "id": "Output Sinusoidal Dengan Frekuensi Sama." },
  { "en": "Apa Yang Berubah Pada Output Sinusoidal?", "id": "Amplitudo Dan Fasa Sesuai Fungsi Transfer." },
  { "en": "Ini Adalah Konsep Dasar?", "id": "Analisis Rangkaian AC Menggunakan Metode Fasor." },
  { "en": "Jadi Fasor Adalah Dunia?", "id": "Dunia Steady-State Dari Transformasi Laplace." },
  { "en": "Bagaimana Dengan Respon Transien?", "id": "Fasor Tidak Memberi Informasi Tentang Transien." },
  { "en": "Laplace Memberi Keduanya?", "id": "Ya Memberi Cerita Lengkap Transien Dan Tunak." },
  { "en": "Apa Itu Fungsi Transfer Termodinamika?", "id": "Menganalisis Dinamika Perpindahan Panas." },
  { "en": "Apa Itu Fungsi Transfer Ekonomi?", "id": "Menganalisis Model Dinamis Ekonomi (Ekonometrika)." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sama Dengan Sinyal Pita Dasar." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sama Dengan Sinyal Lolos Pita." },
  { "en": "Apa Jika Pole Tepat Di Origin?", "id": "Sistem Berperilaku Seperti Sebuah Integrator." },
  { "en": "Apa Jika Input Adalah e^at Dan 'a' Pole?", "id": "Terjadi Resonansi Respon Tumbuh Tak Terbatas." },
  { "en": "Apa Jika Zero Membatalkan Pole Tak Stabil?", "id": "Sistem Stabil BIBO Tapi Tidak Stabil Internal." },
  { "en": "Apa Jika Zero Membatalkan Pole Stabil?", "id": "Mode Stabil Tersebut Menjadi Tersembunyi." },
  { "en": "Apa Jika Dua Pole Sangat Berdekatan?", "id": "Perilaku Mirip Pole Ganda Teredam Kritis." },
  { "en": "Apa Jika Pole Dan Zero Sangat Dekat?", "id": "Residu Mode Tersebut Akan Sangat Kecil." },
  { "en": "Apa Jika Semua Pole Sangat Jauh Kiri?", "id": "Sistem Merespon Dengan Sangat Cepat Sekali." },
  { "en": "Apa Jika Ada Pole Sangat Dekat Origin?", "id": "Sistem Merespon Dengan Sangat Lambat Sekali." },
  { "en": "Apa Jika Input Adalah DC Ke Sistem HPF?", "id": "Output Akan Nol Setelah Transien Selesai." },
  { "en": "Apa Jika Input Frekuensi Tinggi Ke Sistem LPF?", "id": "Output Akan Sangat Kecil Atau Nol." },
  { "en": "Suku e^-at Dalam Solusi Berasal Dari?", "id": "Pole Riil Sederhana Di s = -a." },
  { "en": "Suku t*e^-at Dalam Solusi Berasal Dari?", "id": "Pole Riil Ganda Di s = -a." },
  { "en": "Suku Cosinus Murni Dalam Solusi Berasal Dari?", "id": "Pole Kompleks Di Sumbu Imajiner." },
  { "en": "Suku Sinus Teredam Dalam Solusi Berasal Dari?", "id": "Sepasang Pole Kompleks Di Bidang Kiri." },
  { "en": "Suku Konstanta Dalam Solusi Berasal Dari?", "id": "Pole Di Origin Dari Fungsi Input." },
  { "en": "Amplitudo Setiap Suku Ditentukan Oleh?", "id": "Residu Pole Yang Sesuai (Koefisien Fraksi)." },
  { "en": "Apa Kesalahan Umum Teorema Nilai Akhir?", "id": "Menggunakannya Untuk Sistem Yang Tidak Stabil." },
  { "en": "Apa Kesalahan Umum Fraksi Parsial?", "id": "Lupa Melakukan Pembagian Panjang Dulu." },
  { "en": "Apa Asumsi Dibalik Model Orde Dua?", "id": "Sistem Didominasi Oleh Dua Penyimpan Energi." },
  { "en": "Apa Asumsi Dibalik Analisis Laplace?", "id": "Sistem Harus Linier Dan Invarian Waktu." },
  { "en": "Apa Peran Laplace Dalam Desain?", "id": "Memprediksi Perilaku Sistem Sebelum Dibangun." },
  { "en": "Apa Peran Laplace Dalam Analisis?", "id": "Menjelaskan Perilaku Sistem Yang Sudah Ada." },
  { "en": "Apa Peran Laplace Dalam Pemecahan Masalah?", "id": "Mendiagnosis Masalah Stabilitas Atau Kinerja." },
  { "en": "Apa Arti Fisis Dari Bilangan Kompleks?", "id": "Mewakili Amplitudo Dan Fasa Secara Bersamaan." },
  { "en": "Apa Bagian Riil Bilangan Kompleks?", "id": "Proyeksi Pada Sumbu Horizontal (Amplitudo Cosinus)." },
  { "en": "Apa Bagian Imajiner Bilangan Kompleks?", "id": "Proyeksi Pada Sumbu Vertikal (Amplitudo Sinus)." },
  { "en": "Apa Beda Konvergensi Mutlak Dan Bersyarat?", "id": "Mutlak Lebih Kuat Daripada Konvergensi Bersyarat." },
  { "en": "Integral Laplace Membutuhkan Konvergensi?", "id": "Konvergensi Mutlak Di Dalam Daerah ROC." },
  { "en": "Apa Itu 'Kondisi Baik' (Well-posed)?", "id": "Masalah Yang Punya Solusi Unik Stabil." },
  { "en": "Masalah Nilai Awal ODE Adalah?", "id": "Contoh Masalah Yang Biasanya 'Kondisi Baik'." },
  { "en": "Apa Itu Sinyal Waktu-Terbatas?", "id": "Sama Dengan Sinyal Durasi Terbatas." },
  { "en": "Apa Transformasi Sinyal Waktu-Terbatas?", "id": "F(s) Analitis Di Seluruh Bidang-s." },
  { "en": "Artinya Tidak Punya Pole?", "id": "Tidak Memiliki Pole Di Lokasi Terbatas." },
  { "en": "Apa Hubungan Ketidakpastian Heisenberg?", "id": "Durasi Sinyal Dan Bandwidth Berbanding Terbalik." },
  { "en": "Sinyal Sangat Singkat Di Waktu?", "id": "Memiliki Spektrum Sangat Lebar Di Frekuensi." },
  { "en": "Sinyal Sangat Sempit Di Frekuensi?", "id": "Memiliki Durasi Sangat Panjang Di Waktu." },
  { "en": "Impuls Dirac Adalah Contoh Ekstrem?", "id": "Durasi Nol Tapi Spektrum Tak Terhingga." },
  { "en": "Apa Arti Dari s→0?", "id": "Melihat Perilaku Jangka Panjang Atau DC." },
  { "en": "Apa Arti Dari s→∞?", "id": "Melihat Perilaku Jangka Pendek Atau Awal." },
  { "en": "Fungsi Transfer Sistem Fisik Selalu?", "id": "Menuju Nol Saat s Menuju Tak Hingga." },
  { "en": "Kenapa Harus Menuju Nol?", "id": "Karena Sistem Fisik Tidak Bisa Merespon Instan." },
  { "en": "Ini Terkait Dengan Sifat?", "id": "Sistem Strictly Proper (Orde Relatif Positif)." },
  { "en": "Bagaimana Laplace Menangani Impuls Bergeser?", "id": "Sifat Pergeseran Waktu Tetap Berlaku." },
  { "en": "Transformasi δ(t-a)?", "id": "e Pangkat Negatif as." },
  { "en": "Bagaimana Laplace Menangani Ramp Bergeser?", "id": "Juga Menggunakan Sifat Pergeseran Waktu." },
  { "en": "Transformasi r(t-a)?", "id": "e Pangkat -as Dibagi s Kuadrat." },
  { "en": "Apa Itu Sifat Scaling?", "id": "Sama Dengan Sifat Penskalaan Waktu." },
  { "en": "Memampatkan Sinyal Di Waktu?", "id": "Melebarkan Spektrumnya Di Domain Frekuensi." },
  { "en": "Melebarkan Sinyal Di Waktu?", "id": "Memampatkan Spektrumnya Di Domain Frekuensi." },
  { "en": "Apa Itu Sistem Invers?", "id": "Sistem Yang Membatalkan Efek Sistem Asli." },
  { "en": "Fungsi Transfer Sistem Invers?", "id": "Satu Dibagi Fungsi Transfer Asli (1/H(s))." },
  { "en": "Pole Sistem Invers?", "id": "Adalah Zero Dari Sistem Asli." },
  { "en": "Zero Sistem Invers?", "id": "Adalah Pole Dari Sistem Asli." },
  { "en": "Sistem Invers Stabil Jika?", "id": "Semua Zero Sistem Asli Di Kiri." },
  { "en": "Bagaimana Desain Filter Analog?", "id": "Menentukan H(s) Lalu Merealisasikannya Dengan RLC Op-Amp." },
  { "en": "Apa Itu Tahap Realisasi?", "id": "Mengubah Persamaan Matematika Menjadi Rangkaian Fisik." },
  { "en": "Apa Itu Sintesis Jaringan?", "id": "Proses Formal Untuk Melakukan Tahap Realisasi." },
  { "en": "Apa Itu Fungsi Positif-Riil?", "id": "Fungsi Kompleks Yang Penting Dalam Sintesis." },
  { "en": "Impedansi Jaringan Pasif Harus?", "id": "Fungsi Positif-Riil Dari Variabel s." },
  { "en": "Apa Itu Plot Akar (Root Locus)?", "id": "Plot Lintasan Pole Loop Tertutup." },
  { "en": "Lintasan Diplot Sebagai Fungsi?", "id": "Perubahan Gain Loop Terbuka (K)." },
  { "en": "Root Locus Dimulai Dari?", "id": "Pole Loop Terbuka (Saat K=0)." },
  { "en": "Root Locus Berakhir Di?", "id": "Zero Loop Terbuka (Saat K=Tak Hingga)." },
  { "en": "Root Locus Sangat Berguna Untuk?", "id": "Desain Kontroler Umpan Balik Secara Grafis." },
  { "en": "Apa Itu Asimtot Root Locus?", "id": "Garis Lurus Arah Pergerakan Pole." },
  { "en": "Apa Itu Titik Pecah (Breakaway Point)?", "id": "Titik Dimana Locus Meninggalkan Sumbu Riil." },
  { "en": "Apa Itu Sudut Keberangkatan/Kedatangan?", "id": "Sudut Locus Saat Meninggalkan Pole Kompleks." },
  { "en": "Semua Aturan Plot Root Locus?", "id": "Berasal Dari Analisis Persamaan Karakteristik." },
  { "en": "Apa Itu 'Solusi Kuadratik'?", "id": "Rumus ABC Untuk Mencari Akar Polinomial Orde 2." },
  { "en": "Rumus Ini Berguna Untuk?", "id": "Mencari Pole Sistem Orde Kedua." },
  { "en": "Apa Itu Diskriminan?", "id": "Bagian b²-4ac Dalam Rumus Kuadratik." },
  { "en": "Tanda Diskriminan Menentukan?", "id": "Jenis Pole (Riil Atau Kompleks)." },
  { "en": "Diskriminan Positif Berarti?", "id": "Dua Pole Riil Berbeda (Overdamped)." },
  { "en": "Diskriminan Nol Berarti?", "id": "Dua Pole Riil Sama (Critically Damped)." },
  { "en": "Diskriminan Negatif Berarti?", "id": "Dua Pole Kompleks Konjugat (Underdamped)." },
  { "en": "Apa Itu Konvergensi Seragam?", "id": "Jenis Konvergensi Yang Lebih Kuat." },
  { "en": "Integral Laplace Konvergen Secara?", "id": "Seragam Di Dalam Sub-Daerah ROC." },
  { "en": "Apa Itu Teorema Tauberian Abelian?", "id": "Teorema Yang Menghubungkan Nilai Akhir." },
  { "en": "Apa Itu Fungsi Transfer Transenden?", "id": "Fungsi Transfer Yang Bukan Rasional." },
  { "en": "Contoh Fungsi Transenden?", "id": "Fungsi e^-sT Untuk Sistem Waktu Tunda." },
  { "en": "Sistem Waktu Tunda Memiliki?", "id": "Jumlah Pole Tak Terhingga." },
  { "en": "Apa Itu Aproksimasi Orde Pertama Plus Tunda?", "id": "Model Sederhana Untuk Sistem Orde Tinggi." },
  { "en": "Model Ini Populer Di?", "id": "Industri Kontrol Proses Kimia." },
  { "en": "Parameter Model Ini Adalah?", "id": "Gain Konstanta Waktu Dan Waktu Tunda." },
  { "en": "Bagaimana Parameter Ini Ditemukan?", "id": "Dari Kurva Reaksi Proses Respon Step." },
  { "en": "Apa Itu Kurva Reaksi Proses?", "id": "Respon Step Eksperimental Dari Sistem Nyata." },
  { "en": "Laplace Adalah Alat Matematis?", "id": "Abstrak Namun Dengan Interpretasi Fisik Kuat." },
  { "en": "Menguasai Laplace Membutuhkan Latihan?", "id": "Ya Latihan Mengerjakan Soal Sangat Penting." },
  { "en": "Apa Itu Deret Taylor?", "id": "Ekspansi Fungsi Sebagai Polinomial Tak Hingga." },
  { "en": "Apa Itu Deret Maclaurin?", "id": "Kasus Khusus Deret Taylor Di Sekitar Nol." },
  { "en": "Hubungan Dengan Laplace?", "id": "Digunakan Dalam Analisis Lanjutan Dan Aproksimasi." },
  { "en": "Apa Itu 'Kekakuan' Sistem?", "id": "Saat Konstanta Waktu Sangat Jauh Berbeda." },
  { "en": "Kekakuan Menyulitkan Simulasi?", "id": "Simulasi Numerik Bukan Analisis Simbolik Laplace." },
  { "en": "Apa Esensi Dari Laplace?", "id": "Mengubah Masalah Sulit Menjadi Masalah Mudah." },
  { "en": "Masalah Sulitnya Adalah?", "id": "Persamaan Diferensial Dalam Domain Waktu." },
  { "en": "Masalah Mudahnya Adalah?", "id": "Persamaan Aljabar Dalam Domain Frekuensi." },
  { "en": "Hubungan Stabilitas BIBO Dan Respon Impuls?", "id": "Sistem Stabil Jika Respon Impuls Terintegralkan Mutlak." },
  { "en": "Hubungan Teorema Nilai Akhir Dan Tipe Sistem?", "id": "Tipe Sistem Menentukan Error Nilai Akhir." },
  { "en": "Hubungan ROC Dan Kausalitas?", "id": "ROC Di Kanan Implikasikan Sistem Kausal." },
  { "en": "Kenapa Laplace Fundamental Dalam Teknik?", "id": "Karena Banyak Sistem Fisik Bersifat Linier." },
  { "en": "Apa Abstraksi Utama Yang Dibuat Laplace?", "id": "Mengubah Fungsi Waktu Menjadi Titik Frekuensi." },
  { "en": "Apa Itu 'Domain' Dalam Matematika?", "id": "Kumpulan Nilai Input Untuk Sebuah Fungsi." },
  { "en": "Apa Itu 'Range' Dalam Matematika?", "id": "Kumpulan Nilai Output Dari Sebuah Fungsi." },
  { "en": "Apa Itu 'Operator' Linier?", "id": "Pemetaan Yang Memenuhi Prinsip Superposisi." },
  { "en": "Apakah Pole Sama Dengan Frekuensi Natural?", "id": "Jarak Pole Ke Origin Adalah Frekuensi Natural." },
  { "en": "Apakah Zero Selalu Mengurangi Magnitudo?", "id": "Ya Pada Frekuensi Yang Sama Dengannya." },
  { "en": "Apa Arti Jarak Dari Origin Di Bidang-s?", "id": "Mewakili Frekuensi Natural Tak Teredam (ωn)." },
  { "en": "Apa Arti Sudut Dari Sumbu Riil Negatif?", "id": "Terkait Dengan Rasio Redaman Zeta." },
  { "en": "Apa Itu Sistem Invariansi Skala?", "id": "Perilaku Sama Pada Skala Berbeda." },
  { "en": "Laplace Untuk Sistem Invarian?", "id": "Invarian Waktu Bukan Invarian Skala." },
  { "en": "Apa Beda Solusi Dan Fungsi?", "id": "Solusi Adalah Fungsi Yang Penuhi Persamaan." },
  { "en": "F(s) Adalah Solusi Atau Fungsi?", "id": "Fungsi Hasil Transformasi Belum Solusi Akhir." },
  { "en": "y(t) Adalah Solusi Atau Fungsi?", "id": "Solusi Akhir Dari Persamaan Diferensial." },
  { "en": "Apa Itu Ruang Hilbert?", "id": "Ruang Vektor Abstrak Yang Penting." },
  { "en": "Apa Itu Ruang Banach?", "id": "Jenis Lain Ruang Vektor Abstrak." },
  { "en": "Apa Itu Fungsi Analitik?", "id": "Fungsi Kompleks Yang Dapat Diturunkan." },
  { "en": "F(s) Biasanya Adalah Fungsi?", "id": "Fungsi Analitik Di Dalam Daerah ROC." },
  { "en": "Apa Arti 'Sinyal Well-Behaved'?", "id": "Sinyal Yang Memenuhi Syarat Transformasi Laplace." },
  { "en": "Contoh Sinyal 'Ill-Behaved'?", "id": "Sinyal e^(t²) Yang Tumbuh Terlalu Cepat." },
  { "en": "Apa Itu Teorema Residue?", "id": "Alat Utama Untuk Menghitung Integral Kontur." },
  { "en": "Fraksi Parsial Adalah Aplikasi?", "id": "Aplikasi Praktis Dari Teorema Residue." },
  { "en": "Apa Itu Titik Cabang (Branch Point)?", "id": "Singularitas Fungsi Multi-Nilai Seperti Akar Kuadrat." },
  { "en": "Fungsi Transfer Rasional Punya Titik Cabang?", "id": "Tidak Hanya Memiliki Singularitas Berupa Pole." },
  { "en": "Apa Yang Terjadi Jika Input Periodik?", "id": "Output Terdiri Dari Transien Dan Tunak Periodik." },
  { "en": "Bagian Transien Akan?", "id": "Lenyap Seiring Waktu Untuk Sistem Stabil." },
  { "en": "Bagian Tunak Akan?", "id": "Berbentuk Sama Dengan Input Tapi Beda Amplitudo Fasa." },
  { "en": "Apa Itu Sifat Unik Laplace?", "id": "Setiap f(t) Punya Satu F(s) Unik." },
  { "en": "Sifat Ini Disebut Juga?", "id": "Teorema Lerch Yang Menjamin Keunikan." },
  { "en": "Apa Itu Transformasi Invers Laplace?", "id": "Proses Kembali Dari Domain-s Ke Domain-t." },
  { "en": "Apakah Prosesnya Selalu Mudah?", "id": "Tidak Bisa Sangat Sulit Tanpa Fraksi Parsial." },
  { "en": "Apa Itu Konstanta Proporsionalitas?", "id": "Faktor Pengali Yang Menghubungkan Dua Kuantitas." },
  { "en": "Gain Sistem Adalah?", "id": "Konstanta Proporsionalitas Antara Input Dan Output." },
  { "en": "Apa Itu Respon Impuls Satuan?", "id": "Respon Terhadap Impuls Dengan Luas Satu." },
  { "en": "Apa Itu Respon Step Satuan?", "id": "Respon Terhadap Step Dengan Amplitudo Satu." },
  { "en": "Normalisasi Ini Berguna Untuk?", "id": "Membandingkan Kinerja Antar Sistem Yang Berbeda." },
  { "en": "Apa Itu Sistem Non-Kausal?", "id": "Sistem Yang Responnya Mendahului Input." },
  { "en": "Respon Impuls Sistem Non-Kausal?", "id": "Bernilai Tidak Nol Untuk Waktu t < 0." },
  { "en": "Sistem Non-Kausal Fisis?", "id": "Tidak Ada Sistem Fisik Yang Non-Kausal." },
  { "en": "Kapan Sistem Non-Kausal Dianalisis?", "id": "Dalam Pemrosesan Sinyal Offline (Data Rekaman)." },
  { "en": "Apa Beda Persamaan Linear Dan Afin?", "id": "Afin Adalah Linear Ditambah Sebuah Konstanta." },
  { "en": "Laplace Bisa Untuk Sistem Afin?", "id": "Ya Dengan Menggunakan Sifat Superposisi." },
  { "en": "Apa Itu 'Peredam' Dalam Sistem?", "id": "Komponen Yang Menghilangkan Energi Dari Sistem." },
  { "en": "Peredam Di Rangkaian Listrik?", "id": "Resistor Yang Menghasilkan Panas." },
  { "en": "Peredam Di Sistem Mekanis?", "id": "Dashpot Atau Gesekan Fluida." },
  { "en": "Efek Peredam Pada Pole?", "id": "Menggerakkan Pole Ke Arah Kiri Bidang-s." },
  { "en": "Lebih Banyak Redaman Berarti?", "id": "Pole Bergerak Lebih Jauh Ke Kiri." },
  { "en": "Apa Itu 'Pegas' Dalam Sistem?", "id": "Komponen Yang Menyimpan Dan Melepas Energi." },
  { "en": "Efek Pegas Pada Pole?", "id": "Menggerakkan Pole Menjauhi Sumbu Riil." },
  { "en": "Pegas Lebih Kaku Berarti?", "id": "Pole Bergerak Lebih Jauh Secara Vertikal." },
  { "en": "Apa Itu 'Massa' Dalam Sistem?", "id": "Komponen Yang Menolak Perubahan Kecepatan (Inersia)." },
  { "en": "Gabungan Ketiganya Membentuk?", "id": "Sistem Orde Dua Yang Fundamental." },
  { "en": "Pole-Zeronya Mencerminkan Fisik?", "id": "Ya Interaksi Antara Massa Pegas Dan Peredam." },
  { "en": "Apa Itu Teorema Konvolusi Real?", "id": "Konvolusi Dalam Waktu Adalah Perkalian Dalam Frekuensi." },
  { "en": "Apa Itu Teorema Konvolusi Kompleks?", "id": "Perkalian Dalam Waktu Adalah Konvolusi Dalam Frekuensi." },
  { "en": "Mana Yang Lebih Sering Digunakan?", "id": "Teorema Konvolusi Real Jauh Lebih Berguna." },
  { "en": "Apa Esensi Teorema Nilai Awal?", "id": "Perilaku Awal Ditentukan Oleh Frekuensi Tinggi." },
  { "en": "Apa Esensi Teorema Nilai Akhir?", "id": "Perilaku Akhir Ditentukan Oleh Frekuensi Rendah." },
  { "en": "Apa Itu Gelombang Kotak?", "id": "Sama Dengan Fungsi Gelombang Kotak (Square Wave)." },
  { "en": "Transformasi Gelombang Kotak?", "id": "Memiliki Zero Pada Frekuensi Harmonik Genap." },
  { "en": "Spektrumnya Terdiri Dari?", "id": "Frekuensi Dasar Dan Harmonik Ganjil." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Fenomena Overshoot Dekat Diskontinuitas." },
  { "en": "Apa Itu Sistem Orde Pecahan?", "id": "Sistem Dideskripsikan Turunan Orde Non-Integer." },
  { "en": "Analisisnya Menggunakan?", "id": "Transformasi Laplace Fraksional Yang Lebih Maju." },
  { "en": "Apa Itu Konvergensi Titik Demi Titik?", "id": "Konvergensi Pada Setiap Titik Dalam Domain." },
  { "en": "Apa Itu Konvergensi Dalam Norma?", "id": "Konvergensi 'Rata-Rata' Di Seluruh Domain." },
  { "en": "Apa Beda Model Dan Sistem?", "id": "Sistem Adalah Benda Nyata Model Adalah Deskripsinya." },
  { "en": "Akurasi Analisis Laplace Bergantung?", "id": "Pada Akurasi Model Matematika Yang Digunakan." },
  { "en": "Apa Itu Linearitas Lokal?", "id": "Perilaku Sistem Non-Linier Dekat Titik Operasi." },
  { "en": "Model Linearisasi Bergantung Pada?", "id": "Konsep Linearitas Lokal Ini." },
  { "en": "Apa Itu Feedback Positif?", "id": "Sinyal Output Ditambahkan Kembali Ke Input." },
  { "en": "Efek Feedback Positif?", "id": "Umumnya Menyebabkan Ketidakstabilan Yang Cepat." },
  { "en": "Contoh Feedback Positif?", "id": "Efek Larsen (Dengung) Pada Sistem Audio." },
  { "en": "Apa Itu Feedback Negatif?", "id": "Sinyal Output Dikurangkan Dari Input." },
  { "en": "Efek Feedback Negatif?", "id": "Umumnya Menstabilkan Sistem Dan Meningkatkan Kinerja." },
  { "en": "Sistem Kontrol Hampir Selalu Menggunakan?", "id": "Umpan Balik Negatif Untuk Kestabilan." },
  { "en": "Apa Itu Respon Fana?", "id": "Nama Lain Untuk Respon Transien." },
  { "en": "Apa Itu Respon Mantap?", "id": "Nama Lain Untuk Respon Keadaan Tunak." },
  { "en": "Apa Itu Fungsi Transfer Aljabar?", "id": "Fungsi Rasional Dari Variabel Kompleks s." },
  { "en": "Apa Itu Fungsi Waktu?", "id": "Fungsi Riil Dari Variabel Waktu t." },
  { "en": "Jembatan Antara Keduanya Adalah?", "id": "Transformasi Laplace Dan Transformasi Baliknya." },
  { "en": "Apa Itu Sinyal Kausatif?", "id": "Sama Dengan Sinyal Kausal." },
  { "en": "Bagaimana Laplace Dulu Dihitung?", "id": "Secara Manual Menggunakan Tabel Dan Aturan." },
  { "en": "Bagaimana Laplace Sekarang Dihitung?", "id": "Menggunakan Perangkat Lunak Komputasi Simbolik." },
  { "en": "Contoh Perangkat Lunak?", "id": "MATLAB Mathematica Maple Dan Lainnya." },
  { "en": "Apakah Pemahaman Dasar Tetap Penting?", "id": "Ya Untuk Interpretasi Hasil Yang Benar." },
  { "en": "Tanpa Dasar Perangkat Lunak Adalah?", "id": "Hanya Kotak Hitam Yang Tidak Bisa Dipercaya." },
  { "en": "Apa Itu Kondisi Awal Nol?", "id": "Asumsi Bahwa Sistem Dalam Keadaan Rileks." },
  { "en": "Kapan Asumsi Ini Valid?", "id": "Saat Menganalisis Fungsi Transfer Murni." },
  { "en": "Untuk Respon Total Apa Yang Diperlukan?", "id": "Informasi Lengkap Mengenai Kondisi Awal." },
  { "en": "Apa Itu Diagram Bode?", "id": "Sama Dengan Plot Bode (Magnitudo Dan Fasa)." },
  { "en": "Apa Itu Plot Polar?", "id": "Sama Dengan Plot Nyquist." },
  { "en": "Ketiga Plot Ini Menampilkan?", "id": "Informasi Yang Sama Dalam Format Berbeda." },
  { "en": "Setiap Plot Punya Kelebihan?", "id": "Ya Untuk Menganalisis Aspek Kinerja Berbeda." },
  { "en": "Bode Baik Untuk Desain?", "id": "Desain Kontroler Berbasis Frekuensi." },
  { "en": "Nyquist Baik Untuk Analisis?", "id": "Stabilitas Sistem Dengan Waktu Tunda." },
  { "en": "Dasar Dari Semua Analisis Ini?", "id": "Konsep Fungsi Transfer H(s) Laplace." }



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
