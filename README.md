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


  { "en": "Pulau Paskah Adalah Wilayah Teritorial Dari Negara Mana?", "id": "Chili." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Will to Power'?", "id": "Friedrich Nietzsche." },
  { "en": "Apa Nama Lapisan Terluar Dari Kulit Manusia?", "id": "Epidermis." },
  { "en": "Apa Ciri Utama Dari Zaman Perunggu?", "id": "Penggunaan Peralatan Dari Perunggu." },
  { "en": "Harpsichord Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Papan Tuts." },
  { "en": "Siapa Psikolog Yang Mengusulkan Konsep 'Pikiran Bawah Sadar'?", "id": "Sigmund Freud." },
  { "en": "Kota Apa Yang Dijuluki 'The City of Light'?", "id": "Paris." },
  { "en": "Apa Rasa Dominan Pada Masakan Khas Gudeg?", "id": "Manis." },
  { "en": "Kingdom Apa Dalam Klasifikasi Biologis Yang Mencakup Semua Hewan?", "id": "Animalia." },
  { "en": "Bulan Jupiter Manakah Yang Terkenal Dengan Potensi Samudra?", "id": "Europa." },
  { "en": "Kepulauan Galapagos Adalah Bagian Dari Negara Mana?", "id": "Ekuador." },
  { "en": "Siapa Filsuf Yang Dikenal Dengan Konsep 'Dialektika Materialisme'?", "id": "Karl Marx." },
  { "en": "Apa Nama Lapisan Kulit Di Bawah Epidermis?", "id": "Dermis." },
  { "en": "Apa Ciri Utama Dari Zaman Besi?", "id": "Penggunaan Peralatan Dari Besi." },
  { "en": "Oboe Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan Konsep 'Arketipe'?", "id": "Carl Jung." },
  { "en": "Kota Apa Yang Dijuluki 'The Big Apple'?", "id": "New York." },
  { "en": "Apa Ciri Khas Rasa Pada Masakan Arsik?", "id": "Asam Dan Pedas." },
  { "en": "Kingdom Apa Dalam Klasifikasi Biologis Yang Mencakup Tumbuhan?", "id": "Plantae." },
  { "en": "Bulan Saturnus Manakah Yang Permukaannya Mirip Bumi Purba?", "id": "Titan." },
  { "en": "Pulau Sardinia Adalah Bagian Dari Negara Mana?", "id": "Italia." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Ungkapan 'Tuhan Sudah Mati'?", "id": "Friedrich Nietzsche." },
  { "en": "Pigmen Apa Yang Memberi Warna Pada Kulit Manusia?", "id": "Melanin." },
  { "en": "Apa Nama Zaman Prasejarah Sebelum Zaman Logam?", "id": "Zaman Batu." },
  { "en": "French Horn Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Logam." },
  { "en": "Siapa Psikolog Yang Mengembangkan Teori Hierarki Kebutuhan?", "id": "Abraham Maslow." },
  { "en": "Kota Apa Yang Dijuluki 'The Windy City'?", "id": "Chicago." },
  { "en": "Apa Bahan Utama Yang Membuat Masakan Pempek Kenyal?", "id": "Sagu." },
  { "en": "Kingdom Apa Dalam Klasifikasi Biologis Yang Mencakup Jamur?", "id": "Fungi." },
  { "en": "Bulan Mars Manakah Yang Berukuran Lebih Besar?", "id": "Fobos." },
  { "en": "Pulau Sisilia Adalah Wilayah Otonom Dari Negara Mana?", "id": "Italia." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan 'Paradoks Achilles Dan Kura-Kura'?", "id": "Zeno dari Elea." },
  { "en": "Apa Nama Lapisan Terdalam Dari Kulit Manusia?", "id": "Hipodermis." },
  { "en": "Zaman Batu Dibagi Menjadi Tiga Periode Apa Saja?", "id": "Paleolitikum, Mesolitikum, Neolitikum." },
  { "en": "Bassoon Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan Eksperimen Anjingnya?", "id": "Ivan Pavlov." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Abadi' (The Eternal City)?", "id": "Roma." },
  { "en": "Rasa Pedas Pada Ayam Taliwang Berasal Dari Apa?", "id": "Cabai Kering." },
  { "en": "Organisme Bersel Tunggal Tanpa Inti Termasuk Kingdom Apa?", "id": "Monera (Archaea, Bacteria)." },
  { "en": "Bulan Neptunus Manakah Yang Memiliki Orbit Mundur?", "id": "Triton." },
  { "en": "Kepulauan Canary Adalah Bagian Dari Negara Mana?", "id": "Spanyol." },
  { "en": "Siapa Filsuf Yang Mengusulkan Konsep 'Negara Ideal'?", "id": "Plato." },
  { "en": "Protein Apa Yang Memberi Kekuatan Pada Kulit Dan Rambut?", "id": "Keratin." },
  { "en": "Zaman Apa Yang Ditandai Dengan Munculnya Pertanian?", "id": "Zaman Neolitikum." },
  { "en": "Xylophone Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan Konsep 'Id, Ego, Superego'?", "id": "Sigmund Freud." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Seribu Menara'?", "id": "Praha." },
  { "en": "Apa Yang Memberi Warna Hitam Pada Masakan Rawon?", "id": "Kluwek." },
  { "en": "Ganggang Atau Alga Termasuk Dalam Kingdom Apa?", "id": "Protista." },
  { "en": "Bulan Uranus Manakah Yang Memiliki Ngarai Terbesar?", "id": "Miranda." },
  { "en": "Pulau Kreta Adalah Bagian Dari Negara Mana?", "id": "Yunani." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Empirisme?", "id": "John Locke." },
  { "en": "Kelenjar Apa Yang Menghasilkan Minyak Di Kulit?", "id": "Kelenjar Sebasea." },
  { "en": "Zaman Apa Yang Ditandai Dengan Peralatan Batu Kasar?", "id": "Zaman Paleolitikum." },
  { "en": "Timpani Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Siapa Psikolog Yang Mengembangkan Konsep 'Conditioning Klasik'?", "id": "Ivan Pavlov." },
  { "en": "Kota Apa Yang Dijuluki 'Mutiara Dari Danube'?", "id": "Budapest." },
  { "en": "Rasa Asam Segar Pada Soto Banjar Berasal Dari Apa?", "id": "Jeruk Nipis." },
  { "en": "Amoeba Dan Paramecium Termasuk Dalam Kingdom Apa?", "id": "Protista." },
  { "en": "Bulan Saturnus Manakah Yang Memiliki Kawah Raksasa?", "id": "Mimas." },
  { "en": "Pulau Corsica Adalah Wilayah Dari Negara Mana?", "id": "Perancis." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Rasionalisme Modern?", "id": "René Descartes." },
  { "en": "Apa Lapisan Kulit Yang Terdiri Dari Sel-Sel Mati?", "id": "Stratum Korneum." },
  { "en": "Zaman Apa Yang Menjadi Transisi Antara Paleolitikum Dan Neolitikum?", "id": "Zaman Mesolitikum." },
  { "en": "Violin Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Gesek." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Operant Conditioning'?", "id": "B.F. Skinner." },
  { "en": "Kota Apa Yang Dijuluki 'Venesia Dari Utara'?", "id": "Amsterdam Atau Stockholm." },
  { "en": "Warna Kuning Pada Masakan Soto Lamongan Berasal Dari Apa?", "id": "Kunyit." },
  { "en": "Bakteri Termasuk Dalam Kingdom Apa?", "id": "Eubacteria (Bacteria)." },
  { "en": "Bulan Jupiter Manakah Yang Paling Dekat Dengan Planetnya?", "id": "Metis." },
  { "en": "Kepulauan Azores Adalah Wilayah Otonom Dari Negara Mana?", "id": "Portugal." },
  { "en": "Siapa Filsuf Yang Dikenal Dengan Konsep 'Kategorikal Imperatif'?", "id": "Immanuel Kant." },
  { "en": "Apa Fungsi Utama Dari Kelenjar Keringat?", "id": "Mendinginkan Tubuh." },
  { "en": "Apa Peristiwa Yang Menandai Awal Zaman Sejarah?", "id": "Penemuan Tulisan." },
  { "en": "Cello Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Gesek." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan Eksperimen Penjara Stanford?", "id": "Philip Zimbardo." },
  { "en": "Kota Apa Yang Dijuluki 'Ibukota Dunia Selatan'?", "id": "São Paulo." },
  { "en": "Apa Yang Memberi Aroma Khas Pada Coto Makassar?", "id": "Rempah Tauco." },
  { "en": "Organisme Yang Hidup Di Lingkungan Ekstrem Termasuk Kingdom Apa?", "id": "Archaebacteria (Archaea)." },
  { "en": "Bulan Pluto Yang Terbesar Bernama Apa?", "id": "Charon." },
  { "en": "Pulau Tasmania Adalah Bagian Dari Negara Mana?", "id": "Australia." },
  { "en": "Siapa Filsuf Yang Mengatakan 'Manusia Adalah Binatang Politik'?", "id": "Aristoteles." },
  { "en": "Lapisan Kulit Mana Yang Mengandung Pembuluh Darah?", "id": "Dermis." },
  { "en": "Revolusi Apa Yang Menandai Awal Pertanian?", "id": "Revolusi Neolitik." },
  { "en": "Klarinet Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan Teori Perkembangan Kognitif?", "id": "Jean Piaget." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Singa'?", "id": "Singapura." },
  { "en": "Rasa Khas Pada Sayur Asem Berasal Dari Apa?", "id": "Asam Jawa." },
  { "en": "Jamur Roti Termasuk Dalam Kingdom Apa?", "id": "Fungi." },
  { "en": "Bulan Jupiter Manakah Yang Paling Aktif Secara Vulkanik?", "id": "Io." },
  { "en": "Laut Karibia Terletak Di Antara Benua Apa?", "id": "Amerika Utara Dan Selatan." },
  { "en": "Apa Hasil Utama Dari Perjanjian Ghent (1814)?", "id": "Mengakhiri Perang Tahun 1812." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Clavicula'?", "id": "Tulang Selangka." },
  { "en": "Di Kota Kuno Manakah Letak Taman Gantung Babilonia?", "id": "Babilonia." },
  { "en": "Apa Nama Majas Yang Mengulang Bunyi Konsonan Awal?", "id": "Aliterasi." },
  { "en": "Prinsip Bernoulli Menjelaskan Hubungan Antara Kecepatan Dan Apa?", "id": "Tekanan Fluida." },
  { "en": "Di Negara Manakah Letak Kota Florence (Firenze)?", "id": "Italia." },
  { "en": "Panting Adalah Alat Musik Petik Dari Provinsi Mana?", "id": "Kalimantan Selatan." },
  { "en": "Paus Biru Termasuk Dalam Kelas Hewan Apa?", "id": "Mamalia." },
  { "en": "Dalam Mitologi Yunani, Rasi Bintang Orion Adalah Seorang?", "id": "Pemburu." },
  { "en": "Laut Mediterania Menghubungkan Tiga Benua Apa Saja?", "id": "Eropa, Afrika, Asia." },
  { "en": "Apa Hasil Utama Dari Perjanjian Nanking (1842)?", "id": "Menyerahkan Hong Kong Ke Inggris." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Scapula'?", "id": "Tulang Belikat." },
  { "en": "Di Kota Kuno Manakah Letak Patung Zeus Di Olympia?", "id": "Olympia, Yunani." },
  { "en": "Apa Nama Majas Yang Menggunakan Kata Meniru Bunyi?", "id": "Onomatope." },
  { "en": "Prinsip Siapakah Yang Menjelaskan Gaya Apung Benda?", "id": "Prinsip Archimedes." },
  { "en": "Di Negara Manakah Letak Kota Kyoto?", "id": "Jepang." },
  { "en": "Japen Adalah Alat Musik Petik Dari Provinsi Mana?", "id": "Kalimantan Tengah." },
  { "en": "Buaya Termasuk Dalam Kelas Hewan Apa?", "id": "Reptilia." },
  { "en": "Dalam Mitologi Yunani, Rasi Bintang Taurus Melambangkan Apa?", "id": "Banteng." },
  { "en": "Laut Tiongkok Selatan Berada Di Benua Mana?", "id": "Asia." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Camp David (1978)?", "id": "Perdamaian Israel Dan Mesir." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Patella'?", "id": "Tempurung Lutut." },
  { "en": "Di Kota Kuno Manakah Letak Kuil Artemis Di Ephesus?", "id": "Ephesus." },
  { "en": "Apa Nama Majas Yang Menggunakan Pertentangan Makna?", "id": "Antitesis." },
  { "en": "Hukum Siapakah Yang Menjelaskan Perambatan Tekanan Fluida?", "id": "Hukum Pascal." },
  { "en": "Di Negara Manakah Letak Kota Barcelona?", "id": "Spanyol." },
  { "en": "Garantung Adalah Alat Musik Dari Suku Mana?", "id": "Suku Batak." },
  { "en": "Pinguin Termasuk Dalam Kelas Hewan Apa?", "id": "Aves (Burung)." },
  { "en": "Dalam Mitologi Yunani, Rasi Bintang Leo Melambangkan Apa?", "id": "Singa Nemea." },
  { "en": "Laut Arab Adalah Bagian Dari Samudra Mana?", "id": "Samudra Hindia." },
  { "en": "Apa Tujuan Utama Perjanjian Tordesillas (1494)?", "id": "Membagi Wilayah Penjelajahan." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Femur'?", "id": "Tulang Paha." },
  { "en": "Di Kota Kuno Manakah Letak Mausoleum Di Halicarnassus?", "id": "Halicarnassus." },
  { "en": "Apa Nama Majas Yang Bermakna Kebalikan Dari Sebenarnya?", "id": "Ironi." },
  { "en": "Efek Doppler Menjelaskan Perubahan Apa Pada Gelombang?", "id": "Perubahan Frekuensi." },
  { "en": "Di Negara Manakah Letak Kota Munich?", "id": "Jerman." },
  { "en": "Keso Adalah Alat Musik Gesek Dari Suku Mana?", "id": "Suku Makassar." },
  { "en": "Ikan Hiu Termasuk Dalam Kelas Hewan Apa?", "id": "Chondrichthyes (Ikan Bertulang Rawan)." },
  { "en": "Dalam Mitologi Yunani, Rasi Bintang Gemini Melambangkan Apa?", "id": "Anak Kembar Castor Dan Pollux." },
  { "en": "Laut Bering Terletak Di Antara Benua Apa?", "id": "Asia Dan Amerika Utara." },
  { "en": "Apa Hasil Utama Dari Perjanjian Paris (1973)?", "id": "Mengakhiri Perang Vietnam." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Tibia'?", "id": "Tulang Kering." },
  { "en": "Di Kota Kuno Manakah Letak Mercusuar Alexandria?", "id": "Alexandria." },
  { "en": "Apa Nama Majas Yang Menggantikan Kata Kasar?", "id": "Eufemisme." },
  { "en": "Efek Siapakah Yang Menjelaskan Pembelokan Benda Berputar?", "id": "Efek Coriolis." },
  { "en": "Di Negara Manakah Letak Kota Milan?", "id": "Italia." },
  { "en": "Foy Doa Adalah Alat Musik Tiup Dari Pulau Mana?", "id": "Flores." },
  { "en": "Katak Termasuk Dalam Kelas Hewan Apa?", "id": "Amfibia." },
  { "en": "Dalam Mitologi Yunani, Rasi Bintang Virgo Melambangkan Apa?", "id": "Dewi Keadilan, Astraea." },
  { "en": "Laut Tasman Memisahkan Australia Dengan Negara Apa?", "id": "Selandia Baru." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Maastricht (1992)?", "id": "Membentuk Uni Eropa." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Humerus'?", "id": "Tulang Lengan Atas." },
  { "en": "Di Kota Kuno Manakah Letak Kolosus Di Rodos?", "id": "Rodos." },
  { "en": "Apa Nama Majas Yang Menggunakan Kiasan Berlebihan?", "id": "Hiperbola." },
  { "en": "Asas Siapakah Yang Menjelaskan Keterapungan Benda?", "id": "Asas Archimedes." },
  { "en": "Di Negara Manakah Letak Kota Istanbul?", "id": "Turki." },
  { "en": "Sampe Adalah Alat Musik Petik Dari Suku Mana?", "id": "Suku Dayak." },
  { "en": "Kura-Kura Termasuk Dalam Kelas Hewan Apa?", "id": "Reptilia." },
  { "en": "Dalam Mitologi Yunani, Ursa Major Melambangkan Apa?", "id": "Beruang Besar." },
  { "en": "Laut Hitam Terhubung Ke Laut Mediterania Melalui Selat Apa?", "id": "Selat Bosporus." },
  { "en": "Apa Hasil Utama Dari Konferensi Meja Bundar?", "id": "Penyerahan Kedaulatan Indonesia." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Sternum'?", "id": "Tulang Dada." },
  { "en": "Piramida Agung Giza Adalah Makam Untuk Firaun Siapa?", "id": "Firaun Khufu." },
  { "en": "Apa Nama Majas Yang Menggunakan Pengulangan Kata?", "id": "Repetisi." },
  { "en": "Hukum Siapakah Yang Menyatakan Orbit Planet Berbentuk Elips?", "id": "Hukum Kepler Pertama." },
  { "en": "Di Negara Manakah Letak Kota Lisbon?", "id": "Portugal." },
  { "en": "Gendang Melayu Adalah Alat Musik Khas Dari Daerah Mana?", "id": "Sumatra Dan Semenanjung Malaya." },
  { "en": "Laba-Laba Termasuk Dalam Kelas Hewan Apa?", "id": "Arachnida." },
  { "en": "Rasi Bintang Crux Dikenal Dengan Nama Populer Apa?", "id": "Salib Selatan." },
  { "en": "Laut Merah Terletak Di Antara Benua Apa?", "id": "Afrika Dan Asia." },
  { "en": "Apa Tujuan Dari Perjanjian Adams-Onís (1819)?", "id": "Menyerahkan Florida Ke AS." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Mandibula'?", "id": "Tulang Rahang Bawah." },
  { "en": "Kuil Artemis Di Ephesus Didedikasikan Untuk Dewi Apa?", "id": "Dewi Artemis." },
  { "en": "Apa Nama Majas Yang Membandingkan Dua Hal Berbeda?", "id": "Metafora." },
  { "en": "Teori Siapakah Yang Mengatakan Alam Semesta Mengembang?", "id": "Edwin Hubble." },
  { "en": "Di Negara Manakah Letak Kota Wina (Vienna)?", "id": "Austria." },
  { "en": "Serunai Adalah Alat Musik Tiup Dari Suku Apa?", "id": "Suku Minang." },
  { "en": "Kepiting Termasuk Dalam Kelas Hewan Apa?", "id": "Crustacea." },
  { "en": "Dalam Mitologi Yunani, Ursa Minor Melambangkan Apa?", "id": "Beruang Kecil." },
  { "en": "Laut Baltik Terletak Di Benua Mana?", "id": "Eropa." },
  { "en": "Apa Tujuan Utama Dari Perjanjian Brest-Litovsk?", "id": "Rusia Keluar Dari PD I." },
  { "en": "Apa Nama Umum Untuk Tulang Ilmiah 'Karpal'?", "id": "Tulang Pergelangan Tangan." },
  { "en": "Mausoleum Halicarnassus Dibangun Untuk Siapa?", "id": "Mausolus." },
  { "en": "Apa Nama Majas Yang Menggunakan Kata Kiasan?", "id": "Kiasan." },
  { "en": "Prinsip Siapakah Yang Terkait Dengan Mekanika Kuantum?", "id": "Prinsip Ketidakpastian Heisenberg." },
  { "en": "Di Negara Manakah Letak Kota Praha?", "id": "Ceko." },
  { "en": "Celempung Adalah Alat Musik Petik Dari Daerah Mana?", "id": "Jawa (Sunda)." },
  { "en": "Cacing Tanah Termasuk Dalam Filum Hewan Apa?", "id": "Annelida." },
  { "en": "Rasi Bintang Apa Yang Digunakan Untuk Menemukan Bintang Utara?", "id": "Ursa Minor." },
  { "en": "Kanal Suez Menghubungkan Laut Mediterania Dengan Laut Apa?", "id": "Laut Merah." },
  { "en": "Pada Abad Berapa Julius Caesar Hidup?", "id": "Abad Ke-1 SM." },
  { "en": "Belahan Otak Mana Yang Umumnya Berhubungan Dengan Kreativitas?", "id": "Otak Kanan." },
  { "en": "Peradaban Mesopotamia Kuno Terletak Di Negara Modern Mana?", "id": "Irak." },
  { "en": "Subgenre Fiksi Ilmiah Apa Yang Bertema Futuristik Kelam?", "id": "Cyberpunk." },
  { "en": "Siapa Matematikawan Yunani Yang Terkenal Dengan Teoremanya?", "id": "Pythagoras." },
  { "en": "Apa Simbol Mata Uang Untuk Pound Sterling Inggris?", "id": "£." },
  { "en": "Tari Legong Adalah Tarian Klasik Dari Daerah Mana?", "id": "Bali." },
  { "en": "Dalam Rantai Makanan, Tumbuhan Disebut Sebagai Apa?", "id": "Produsen." },
  { "en": "Galaksi Bima Sakti Termasuk Dalam Jenis Galaksi Apa?", "id": "Spiral Berpalang." },
  { "en": "Kanal Panama Menghubungkan Samudra Atlantik Dengan Samudra Apa?", "id": "Samudra Pasifik." },
  { "en": "Pada Abad Berapa Ratu Elizabeth I Memerintah Inggris?", "id": "Abad Ke-16." },
  { "en": "Belahan Otak Mana Yang Umumnya Berhubungan Dengan Bahasa?", "id": "Otak Kiri." },
  { "en": "Peradaban Aztec Kuno Terletak Di Negara Modern Mana?", "id": "Meksiko." },
  { "en": "Subgenre Fantasi Apa Yang Berlatar Di Dunia Fiksi?", "id": "Fantasi Tinggi (High Fantasy)." },
  { "en": "Siapa Matematikawan Yang Dianggap Sebagai Bapak Geometri?", "id": "Euclid." },
  { "en": "Apa Simbol Mata Uang Untuk Euro?", "id": "€." },
  { "en": "Tari Golek Menak Adalah Tarian Dari Keraton Mana?", "id": "Keraton Yogyakarta." },
  { "en": "Organisme Yang Memakan Tumbuhan Disebut Apa?", "id": "Konsumen Primer (Herbivora)." },
  { "en": "Apa Jenis Galaksi Yang Tidak Memiliki Bentuk Teratur?", "id": "Galaksi Tak Beraturan." },
  { "en": "Selat Malaka Memisahkan Semenanjung Malaya Dengan Pulau Apa?", "id": "Pulau Sumatra." },
  { "en": "Pada Abad Berapa Napoleon Bonaparte Berkuasa Di Perancis?", "id": "Awal Abad Ke-19." },
  { "en": "Fungsi Logika Dan Analisis Umumnya Diatur Oleh Otak Mana?", "id": "Otak Kiri." },
  { "en": "Peradaban Inka Kuno Berpusat Di Negara Modern Mana?", "id": "Peru." },
  { "en": "Subgenre Horor Apa Yang Menggunakan Ketegangan Psikologis?", "id": "Horor Psikologis." },
  { "en": "Siapa Matematikawan Yang Mengembangkan Kalkulus Secara Independen?", "id": "Newton Dan Leibniz." },
  { "en": "Apa Simbol Mata Uang Untuk Yen Jepang?", "id": "¥." },
  { "en": "Tari Pakarena Adalah Tarian Tradisional Dari Suku Apa?", "id": "Suku Makassar." },
  { "en": "Organisme Yang Memakan Herbivora Disebut Apa?", "id": "Konsumen Sekunder (Karnivora)." },
  { "en": "Apa Jenis Galaksi Yang Berbentuk Bulat Atau Elips?", "id": "Galaksi Elips." },
  { "en": "Selat Gibraltar Memisahkan Spanyol Dengan Negara Apa?", "id": "Maroko." },
  { "en": "Pada Abad Berapa Leonardo Da Vinci Hidup?", "id": "Abad Ke-15 Hingga Ke-16." },
  { "en": "Imajinasi Spasial Dan Artistik Umumnya Diatur Oleh Otak Mana?", "id": "Otak Kanan." },
  { "en": "Peradaban Mesir Kuno Terletak Di Benua Mana?", "id": "Afrika." },
  { "en": "Subgenre Fiksi Kriminal Apa Yang Fokus Pada Detektif?", "id": "Whodunit." },
  { "en": "Siapa Matematikawan Perancis Yang Terkenal Dengan Segitiganya?", "id": "Blaise Pascal." },
  { "en": "Apa Simbol Mata Uang Untuk Dolar Amerika Serikat?", "id": "$." },
  { "en": "Tari Perang Adalah Tarian Tradisional Dari Suku Mana?", "id": "Suku Dayak." },
  { "en": "Organisme Yang Memakan Karnivora Disebut Apa?", "id": "Konsumen Tersier." },
  { "en": "Apa Nama Jenis Galaksi Yang Paling Umum?", "id": "Galaksi Spiral." },
  { "en": "Selat Bosporus Memisahkan Sisi Eropa Dan Asia Kota Mana?", "id": "Istanbul." },
  { "en": "Pada Abad Berapa Genghis Khan Menyatukan Bangsa Mongol?", "id": "Abad Ke-13." },
  { "en": "Pengenalan Wajah Adalah Fungsi Dominan Dari Otak Mana?", "id": "Otak Kanan." },
  { "en": "Peradaban Maya Kuno Berkembang Di Wilayah Modern Mana?", "id": "Meksiko Dan Amerika Tengah." },
  { "en": "Subgenre Fantasi Apa Yang Terjadi Di Dunia Nyata?", "id": "Fantasi Urban." },
  { "en": "Siapa Matematikawan Yang Merumuskan Teori Probabilitas?", "id": "Pascal Dan Fermat." },
  { "en": "Apa Simbol Mata Uang Untuk Franc Swiss?", "id": "CHF." },
  { "en": "Tari Zapin Adalah Tarian Yang Dipengaruhi Budaya Apa?", "id": "Budaya Arab." },
  { "en": "Jamur Dan Bakteri Berperan Sebagai Apa Dalam Ekosistem?", "id": "Dekomposer (Pengurai)." },
  { "en": "Galaksi Andromeda Termasuk Dalam Jenis Galaksi Apa?", "id": "Spiral." },
  { "en": "Kanal Kiel Menghubungkan Laut Utara Dengan Laut Apa?", "id": "Laut Baltik." },
  { "en": "Pada Abad Berapa Ratu Victoria Memerintah Inggris?", "id": "Abad Ke-19." },
  { "en": "Kemampuan Matematika Umumnya Terletak Di Belahan Otak Mana?", "id": "Otak Kiri." },
  { "en": "Peradaban Persia Kuno Terletak Di Negara Modern Mana?", "id": "Iran." },
  { "en": "Subgenre Fiksi Ilmiah Apa Yang Bertema Perjalanan Waktu?", "id": "Time Travel Fiction." },
  { "en": "Siapa Matematikawan Yang Dikenal Dengan Teori Bilangannya?", "id": "Pierre de Fermat." },
  { "en": "Apa Simbol Mata Uang Untuk Rupee India?", "id": "₹." },
  { "en": "Tari Cokek Adalah Tarian Pergaulan Dari Budaya Apa?", "id": "Betawi-Tionghoa." },
  { "en": "Tingkat Paling Atas Dalam Piramida Makanan Disebut Apa?", "id": "Predator Puncak." },
  { "en": "Awan Magellan Termasuk Dalam Jenis Galaksi Apa?", "id": "Galaksi Tak Beraturan." },
  { "en": "Selat Bering Memisahkan Rusia Dengan Negara Mana?", "id": "Amerika Serikat (Alaska)." },
  { "en": "Pada Abad Berapa Christopher Columbus Melakukan Pelayarannya?", "id": "Akhir Abad Ke-15." },
  { "en": "Interpretasi Musik Adalah Fungsi Dominan Dari Otak Mana?", "id": "Otak Kanan." },
  { "en": "Peradaban Romawi Kuno Berpusat Di Kota Apa?", "id": "Roma." },
  { "en": "Subgenre Fiksi Apa Yang Menceritakan Sejarah Alternatif?", "id": "Alternate History." },
  { "en": "Siapa Matematikawan Yang Membuat Sumbu Koordinat Kartesius?", "id": "René Descartes." },
  { "en": "Apa Simbol Mata Uang Untuk Rubel Rusia?", "id": "₽." },
  { "en": "Tari Gandrung Berasal Dari Suku Apa?", "id": "Suku Osing, Banyuwangi." },
  { "en": "Manusia Yang Memakan Tumbuhan Dan Hewan Disebut Apa?", "id": "Omnivora." },
  { "en": "Galaksi Sombrero Termasuk Dalam Jenis Galaksi Apa?", "id": "Spiral." },
  { "en": "Selat Sunda Memisahkan Pulau Jawa Dengan Pulau Apa?", "id": "Pulau Sumatra." },
  { "en": "Pada Abad Berapa Martin Luther Memulai Reformasi?", "id": "Abad Ke-16." },
  { "en": "Pemikiran Kritis Dan Logis Adalah Fungsi Otak Mana?", "id": "Otak Kiri." },
  { "en": "Peradaban Yunani Kuno Berpusat Di Negara Modern Mana?", "id": "Yunani." },
  { "en": "Subgenre Fiksi Apa Yang Berlatar Luar Angkasa?", "id": "Space Opera." },
  { "en": "Siapa Matematikawan Yang Merumuskan Teorema Terakhirnya?", "id": "Pierre de Fermat." },
  { "en": "Apa Simbol Mata Uang Untuk Yuan Tiongkok?", "id": "¥." },
  { "en": "Tari Barong Adalah Pertunjukan Mitos Dari Pulau Mana?", "id": "Bali." },
  { "en": "Apa Sebutan Untuk Aliran Energi Dalam Rantai Makanan?", "id": "Aliran Energi." },
  { "en": "Apa Karakteristik Utama Dari Galaksi Elips?", "id": "Berisi Bintang-Bintang Tua." },
  { "en": "Selat Hormuz Menghubungkan Teluk Persia Dengan Teluk Apa?", "id": "Teluk Oman." },
  { "en": "Pada Abad Berapa Perang Dunia I Terjadi?", "id": "Awal Abad Ke-20." },
  { "en": "Kesadaran Spasial Adalah Fungsi Dominan Otak Mana?", "id": "Otak Kanan." },
  { "en": "Peradaban Ottoman Berpusat Di Negara Modern Mana?", "id": "Turki." },
  { "en": "Subgenre Fantasi Apa Yang Menggunakan Unsur Mitologi?", "id": "Mythic Fantasy." },
  { "en": "Siapa Matematikawan Yang Dikenal Dengan Simbol Tak Hingga?", "id": "John Wallis." },
  { "en": "Apa Simbol Mata Uang Untuk Won Korea Selatan?", "id": "₩." },
  { "en": "Tari Hudoq Adalah Ritual Dari Suku Mana?", "id": "Suku Dayak." },
  { "en": "Apa Posisi Terendah Dalam Rantai Makanan?", "id": "Produsen." },
  { "en": "Apa Karakteristik Utama Dari Galaksi Tak Beraturan?", "id": "Banyak Gas Dan Debu." },
  { "en": "Laut Hitam Terletak Di Antara Benua Apa?", "id": "Eropa Dan Asia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1986?", "id": "Elie Wiesel." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Penciuman?", "id": "Olfaksi." },
  { "en": "Kaisar Augustus Memulai Periode Damai Yang Dikenal Sebagai?", "id": "Pax Romana." },
  { "en": "Cor Anglais Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Hukum Avogadro Menghubungkan Volume Gas Dengan Apa?", "id": "Jumlah Mol Zat." },
  { "en": "Dari Negara Manakah Asal Penemu Telepon?", "id": "Skotlandia." },
  { "en": "Sate Lilit Adalah Hidangan Khas Dari Pulau Mana?", "id": "Bali." },
  { "en": "Serangga Dan Laba-Laba Termasuk Dalam Filum Hewan Apa?", "id": "Arthropoda." },
  { "en": "Apa Istilah Pergeseran Spektrum Cahaya Ke Arah Merah?", "id": "Pergeseran Merah (Redshift)." },
  { "en": "Laut Kaspia Adalah Danau Air Asin Terbesar Di Dunia?", "id": "Benar." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1946?", "id": "Hermann Hesse." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Pengecap?", "id": "Gustasi." },
  { "en": "Siapa Kaisar Romawi Yang Memformalkan Kekristenan?", "id": "Theodosius I." },
  { "en": "Glockenspiel Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Hukum Faraday Tentang Induksi Berhubungan Dengan Apa?", "id": "Medan Magnet Dan Arus Listrik." },
  { "en": "Dari Negara Manakah Asal Penemu Mesin Diesel?", "id": "Jerman." },
  { "en": "Gado-Gado Adalah Makanan Khas Dari Daerah Mana?", "id": "Betawi (Jakarta)." },
  { "en": "Siput Dan Kerang Termasuk Dalam Filum Hewan Apa?", "id": "Mollusca." },
  { "en": "Apa Istilah Pergeseran Spektrum Cahaya Ke Arah Biru?", "id": "Pergeseran Biru (Blueshift)." },
  { "en": "Laut Merah Memisahkan Benua Afrika Dengan Semenanjung Apa?", "id": "Semenanjung Arab." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1922?", "id": "Niels Bohr." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Peraba?", "id": "Somatosensori." },
  { "en": "Siapa Raja Inggris Yang Terkenal Dalam Perang Salib?", "id": "Richard I (The Lionheart)." },
  { "en": "Contrabassoon Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup Kayu." },
  { "en": "Hukum Coulomb Menjelaskan Gaya Antara Dua Apa?", "id": "Muatan Listrik." },
  { "en": "Dari Negara Manakah Asal Penemu World Wide Web?", "id": "Inggris." },
  { "en": "Sate Madura Menggunakan Bumbu Utama Apa?", "id": "Kacang Tanah." },
  { "en": "Bintang Laut Dan Teripang Termasuk Dalam Filum Hewan Apa?", "id": "Echinodermata." },
  { "en": "Apa Radiasi Latar Belakang Sisa Dari Big Bang?", "id": "Cosmic Microwave Background (CMB)." },
  { "en": "Laut Jepang Terletak Di Antara Jepang Dan Benua Apa?", "id": "Asia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1990?", "id": "Mikhail Gorbachev." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Pendengaran?", "id": "Audisi." },
  { "en": "Siapa Sultan Mesir Yang Mengalahkan Pasukan Salib?", "id": "Salahuddin Al-Ayyubi." },
  { "en": "Celesta Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Hukum Joule Menjelaskan Hubungan Antara Panas Dan Apa?", "id": "Arus Listrik." },
  { "en": "Dari Negara Manakah Asal Penemu Kertas?", "id": "Tiongkok." },
  { "en": "Nasi Liwet Adalah Makanan Khas Dari Kota Mana?", "id": "Solo (Surakarta)." },
  { "en": "Ubur-Ubur Dan Anemon Laut Termasuk Dalam Filum Hewan Apa?", "id": "Cnidaria." },
  { "en": "Apa Materi Hipotetis Yang Tidak Memancarkan Cahaya?", "id": "Materi Gelap (Dark Matter)." },
  { "en": "Laut Utara Adalah Bagian Dari Samudra Apa?", "id": "Samudra Atlantik." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1962?", "id": "Perutz Dan Kendrew." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Penglihatan?", "id": "Visi." },
  { "en": "Siapa Ratu Spanyol Yang Mendanai Pelayaran Columbus?", "id": "Isabella I dari Kastila." },
  { "en": "Vibraphone Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Perkusi." },
  { "en": "Hukum Ampère Menghubungkan Arus Listrik Dengan Apa?", "id": "Medan Magnet." },
  { "en": "Dari Negara Manakah Asal Penemu Mesin Cetak?", "id": "Jerman." },
  { "en": "Lumpia Semarang Menggunakan Isian Utama Apa?", "id": "Rebung." },
  { "en": "Cacing Pipih Termasuk Dalam Filum Hewan Apa?", "id": "Platyhelminthes." },
  { "en": "Apa Energi Hipotetis Penyebab Perluasan Alam Semesta?", "id": "Energi Gelap (Dark Energy)." },
  { "en": "Laut Karibia Adalah Bagian Dari Samudra Mana?", "id": "Samudra Atlantik." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1953?", "id": "Krebs Dan Lipmann." },
  { "en": "Indra Keseimbangan Terletak Di Organ Apa?", "id": "Telinga Dalam." },
  { "en": "Siapa Kaisar Romawi Suci Yang Paling Terkenal?", "id": "Charlemagne." },
  { "en": "Lute Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Petik." },
  { "en": "Hukum Snellius Menjelaskan Tentang Apa?", "id": "Pembiasan Cahaya." },
  { "en": "Dari Negara Manakah Asal Penemu Mikroskop?", "id": "Belanda." },
  { "en": "Asinan Bogor Menggunakan Bahan Utama Apa?", "id": "Buah Dan Sayuran." },
  { "en": "Spons Laut Termasuk Dalam Filum Hewan Apa?", "id": "Porifera." },
  { "en": "Apa Fenomena Lensa Gravitasi Pada Cahaya Bintang?", "id": "Gravitational Lensing." },
  { "en": "Laut Okhotsk Terletak Di Sebelah Barat Samudra Apa?", "id": "Samudra Pasifik." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1913?", "id": "Rabindranath Tagore." },
  { "en": "Apa Nama Ilmiah Untuk Indra Keseimbangan?", "id": "Vestibular." },
  { "en": "Siapa Kaisar Mongol Yang Mendirikan Dinasti Yuan?", "id": "Kubilai Khan." },
  { "en": "Didgeridoo Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup." },
  { "en": "Hukum Planck Menjelaskan Radiasi Dari Objek Apa?", "id": "Benda Hitam." },
  { "en": "Dari Negara Manakah Asal Penemu Teleskop?", "id": "Belanda." },
  { "en": "Tahu Gejrot Adalah Makanan Khas Dari Kota Mana?", "id": "Cirebon." },
  { "en": "Manusia Termasuk Dalam Filum Hewan Apa?", "id": "Chordata." },
  { "en": "Apa Peristiwa Teoretis Awal Mula Alam Semesta?", "id": "Ledakan Dahsyat (Big Bang)." },
  { "en": "Laut Adriatik Memisahkan Semenanjung Italia Dengan Semenanjung Apa?", "id": "Semenanjung Balkan." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1975?", "id": "Andrei Sakharov." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Perasa Sakit?", "id": "Nosisepsi." },
  { "en": "Siapa Raja Prusia Yang Terkenal Sebagai 'Raja Prajurit'?", "id": "Frederick William I." },
  { "en": "Theremin Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Elektronik." },
  { "en": "Hukum Stefan-Boltzmann Menjelaskan Hubungan Antara Suhu Dan Apa?", "id": "Energi Radiasi." },
  { "en": "Dari Negara Manakah Asal Penemu Semen?", "id": "Inggris." },
  { "en": "Karedok Adalah Makanan Khas Dari Daerah Mana?", "id": "Jawa Barat (Sunda)." },
  { "en": "Cacing Gelang Termasuk Dalam Filum Hewan Apa?", "id": "Nematoda." },
  { "en": "Apa Istilah Untuk Titik Di Langit Tepat Di Atas?", "id": "Zenit." },
  { "en": "Laut Aegea Terletak Di Antara Negara Mana?", "id": "Yunani Dan Turki." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1918?", "id": "Fritz Haber." },
  { "en": "Apa Istilah Ilmiah Untuk Indra Suhu?", "id": "Termosepsi." },
  { "en": "Siapa Ratu Inggris Yang Terkenal Dengan 'Armada Spanyol'?", "id": "Ratu Elizabeth I." },
  { "en": "Bagpipe Termasuk Dalam Keluarga Alat Musik Apa?", "id": "Alat Musik Tiup." },
  { "en": "Hukum Fick Menjelaskan Tentang Proses Apa?", "id": "Difusi." },
  { "en": "Dari Negara Manakah Asal Penemu Penisilin?", "id": "Skotlandia." },
  { "en": "Mie Kocok Adalah Makanan Khas Dari Kota Mana?", "id": "Bandung." },
  { "en": "Semua Hewan Bertulang Belakang Termasuk Subfilum Apa?", "id": "Vertebrata." },
  { "en": "Apa Istilah Untuk Titik Di Langit Tepat Di Bawah?", "id": "Nadir." },
  { "en": "Di Negara Manakah Letak Kota Casablanca?", "id": "Maroko." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 2007?", "id": "Al Gore Dan IPCC." },
  { "en": "Apa Nama Otot Terkuat Di Tubuh Manusia Berdasarkan Berat?", "id": "Otot Masseter (Rahang)." },
  { "en": "Apa Hasil Signifikan Dari Pertempuran Trafalgar?", "id": "Supremasi Angkatan Laut Inggris." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Blues?", "id": "Pola 12 Bar Dan Nada Sedih." },
  { "en": "Dari Negara Manakah Asal Penemu Dinamit, Alfred Nobel?", "id": "Swedia." },
  { "en": "Di Negara Manakah Letak Patung Kristus Penebus?", "id": "Brasil." },
  { "en": "Wayang Kulit Secara Tradisional Terbuat Dari Bahan Apa?", "id": "Kulit Kerbau." },
  { "en": "Apa Nama Formasi Batuan Runcing Di Langit-Langit Gua?", "id": "Stalaktit." },
  { "en": "Apa Sebutan Untuk Planet Yang Mengorbit Bintang Lain?", "id": "Eksoplanet." },
  { "en": "Di Negara Manakah Letak Kota Auckland?", "id": "Selandia Baru." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1999?", "id": "Günter Grass." },
  { "en": "Apa Nama Otot Terpanjang Di Tubuh Manusia?", "id": "Otot Sartorius." },
  { "en": "Siapa Pemenang Perang Peloponnesia Antara Athena Dan Sparta?", "id": "Sparta." },
  { "en": "Dari Wilayah Mana Musik Country Berasal?", "id": "Amerika Serikat Bagian Selatan." },
  { "en": "Dari Negara Manakah Asal Penemu Telepon, Alexander Graham Bell?", "id": "Skotlandia." },
  { "en": "Di Kota Manakah Letak Gedung Opera Sydney?", "id": "Sydney." },
  { "en": "Apa Tujuan Utama Dari Upacara Adat Ngaben Di Bali?", "id": "Kremasi Jenazah." },
  { "en": "Apa Nama Formasi Batuan Yang Tumbuh Dari Dasar Gua?", "id": "Stalagmit." },
  { "en": "Apa Sebutan Untuk Awan Gas Dan Debu Antarbintang?", "id": "Nebula." },
  { "en": "Di Negara Manakah Letak Kota Munich (München)?", "id": "Jerman." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1935?", "id": "James Chadwick." },
  { "en": "Apa Nama Otot Besar Di Bagian Paha Depan?", "id": "Quadriceps Femoris." },
  { "en": "Apa Hasil Akhir Dari Perang Saudara Amerika?", "id": "Kemenangan Union." },
  { "en": "Apa Ciri Khas Ritme Musik Reggae?", "id": "Aksen Off-Beat." },
  { "en": "Dari Negara Manakah Asal Penemu Baterai, Alessandro Volta?", "id": "Italia." },
  { "en": "Di Negara Manakah Letak Tembok Besar Tiongkok?", "id": "Tiongkok." },
  { "en": "Apa Bahan Pewarna Alami Pada Batik Sogan?", "id": "Kayu Soga." },
  { "en": "Apa Nama Lembah Curam Yang Terbentuk Oleh Sungai?", "id": "Ngarai (Canyon)." },
  { "en": "Apa Sebutan Untuk Bintang Yang Baru Lahir?", "id": "Protobintang." },
  { "en": "Di Negara Manakah Letak Kota Jenewa (Geneva)?", "id": "Swiss." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1934?", "id": "Harold Urey." },
  { "en": "Apa Nama Otot Betis Di Bagian Belakang Kaki?", "id": "Gastrocnemius." },
  { "en": "Apa Akibat Dari Perang Candu Bagi Tiongkok?", "id": "Penyerahan Hong Kong." },
  { "en": "Apa Instrumen Utama Dalam Musik Rock and Roll?", "id": "Gitar Listrik, Drum, Bass." },
  { "en": "Dari Negara Manakah Asal Penemu Mesin Cetak, Gutenberg?", "id": "Jerman." },
  { "en": "Di Negara Manakah Letak Situs Kuno Machu Picchu?", "id": "Peru." },
  { "en": "Apa Fungsi Utama Dari Ritual Peusijuek Di Aceh?", "id": "Memberi Berkah Dan Doa." },
  { "en": "Apa Nama Dataran Tinggi Luas Dengan Sisi Curam?", "id": "Mesa." },
  { "en": "Apa Sebutan Untuk Bintang Yang Gagal Menyala?", "id": "Katai Coklat." },
  { "en": "Di Negara Manakah Letak Kota Lagos?", "id": "Nigeria." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1959?", "id": "Ochoa Dan Kornberg." },
  { "en": "Otot Apa Yang Memompa Darah Ke Seluruh Tubuh?", "id": "Otot Jantung (Miokardium)." },
  { "en": "Apa Hasil Dari Pertempuran Hastings (1066)?", "id": "Penaklukan Norman Atas Inggris." },
  { "en": "Apa Ciri Khas Dari Musik Klasik Era Barok?", "id": "Ornamen Kompleks Dan Kontrapung." },
  { "en": "Dari Negara Manakah Asal Penemu Penisilin, Alexander Fleming?", "id": "Skotlandia." },
  { "en": "Di Negara Manakah Letak Kota Kuno Petra?", "id": "Yordania." },
  { "en": "Apa Tujuan Utama Dari Upacara Adat Tiwah?", "id": "Mengantar Arwah Ke Surga." },
  { "en": "Apa Nama Lembah Sempit Dan Dalam Yang Dibuat Gletser?", "id": "Fjord." },
  { "en": "Apa Sebutan Untuk Sisa Ledakan Bintang Raksasa?", "id": "Sisa Supernova." },
  { "en": "Di Negara Manakah Letak Kota Kyoto?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1991?", "id": "Aung San Suu Kyi." },
  { "en": "Otot Apa Yang Berfungsi Untuk Mengunyah?", "id": "Otot Masseter Dan Temporalis." },
  { "en": "Siapa Pemenang Dalam Perang Seratus Tahun?", "id": "Perancis." },
  { "en": "Apa Ciri Khas Dari Musik Klasik Era Romantik?", "id": "Ekspresi Emosi Yang Kuat." },
  { "en": "Dari Negara Manakah Asal Penemu Kertas, Cai Lun?", "id": "Tiongkok." },
  { "en": "Di Negara Manakah Letak Angkor Wat?", "id": "Kamboja." },
  { "en": "Kerajinan Perak Celuk Berasal Dari Desa Di Provinsi Mana?", "id": "Bali." },
  { "en": "Apa Nama Kolam Air Panas Yang Menyembur Periodik?", "id": "Geyser." },
  { "en": "Apa Sebutan Untuk Bintang Mati Yang Sangat Padat?", "id": "Bintang Neutron." },
  { "en": "Di Negara Manakah Letak Kota Alexandria?", "id": "Mesir." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1953?", "id": "Winston Churchill." },
  { "en": "Otot Apa Yang Berfungsi Untuk Mengangkat Bahu?", "id": "Otot Trapezius." },
  { "en": "Apa Hasil Akhir Dari Perang Korea (1950-1953)?", "id": "Gencatan Senjata (Stalemate)." },
  { "en": "Musik Hip Hop Lahir Di Kota Mana?", "id": "New York (The Bronx)." },
  { "en": "Dari Negara Manakah Asal Penemu Semen Portland?", "id": "Inggris." },
  { "en": "Di Negara Manakah Letak Patung Moai?", "id": "Chili (Pulau Paskah)." },
  { "en": "Apa Bahan Dasar Pembuatan Kerajinan Gerabah?", "id": "Tanah Liat." },
  { "en": "Apa Nama Gunung Berapi Yang Membentuk Danau Kawah?", "id": "Kaldera." },
  { "en": "Apa Sebutan Untuk Lubang Hitam Di Pusat Galaksi?", "id": "Lubang Hitam Supermasif." },
  { "en": "Di Negara Manakah Letak Kota St. Petersburg?", "id": "Rusia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1903?", "id": "Becquerel, Pierre Dan Marie Curie." },
  { "en": "Otot Apa Yang Berfungsi Menekuk Siku?", "id": "Otot Bisep." },
  { "en": "Apa Hasil Dari Perang Enam Hari (1967)?", "id": "Kemenangan Israel." },
  { "en": "Apa Ciri Khas Musik Jazz?", "id": "Improvisasi Dan Sinkopasi." },
  { "en": "Dari Negara Manakah Asal Penemu Mesin Uap, James Watt?", "id": "Skotlandia." },
  { "en": "Di Negara Manakah Letak Stonehenge?", "id": "Inggris." },
  { "en": "Apa Tujuan Utama Upacara Seren Taun Suku Sunda?", "id": "Syukuran Atas Hasil Panen." },
  { "en": "Apa Nama Depresi Melingkar Akibat Tumbukan Meteorit?", "id": "Kawah." },
  { "en": "Apa Sebutan Untuk Gugus Bintang Yang Sangat Padat?", "id": "Gugus Bola (Globular Cluster)." },
  { "en": "Di Negara Manakah Letak Kota Vancouver?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1979?", "id": "Bunda Teresa." },
  { "en": "Otot Apa Yang Berfungsi Meluruskan Kaki?", "id": "Otot Quadriceps." },
  { "en": "Apa Akibat Utama Dari Perjanjian Trianon?", "id": "Hungaria Kehilangan Banyak Wilayah." },
  { "en": "Apa Ciri Khas Musik Gamelan?", "id": "Tangga Nada Pentatonik." },
  { "en": "Dari Negara Manakah Asal Penemu World Wide Web?", "id": "Inggris." },
  { "en": "Di Negara Manakah Letak Acropolis Athena?", "id": "Yunani." },
  { "en": "Apa Makna Simbolis Dari Corak Batik Parang?", "id": "Kekuatan Dan Kekuasaan." },
  { "en": "Apa Nama Titik Tertinggi Di Permukaan Bumi?", "id": "Puncak Gunung." },
  { "en": "Apa Sebutan Untuk Galaksi Yang Sedang Bertabrakan?", "id": "Galaksi Interaktif." },
  { "en": "Selat Apa Yang Memisahkan Italia Dan Albania?", "id": "Selat Otranto." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1901?", "id": "Emil von Behring." },
  { "en": "Apa Fungsi Utama Dari Saraf Optik?", "id": "Mengirim Informasi Visual Ke Otak." },
  { "en": "Apa Hasil Dari Perjanjian Guadalupe Hidalgo?", "id": "Mengakhiri Perang Meksiko-Amerika." },
  { "en": "Dari Negara Manakah Asal Pelukis Rembrandt?", "id": "Belanda." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Oksigen?", "id": "Joseph Priestley." },
  { "en": "Di Negara Manakah Letak Gunung Fuji?", "id": "Jepang." },
  { "en": "Apa Arti Semboyan Negara Indonesia 'Bhinneka Tunggal Ika'?", "id": "Berbeda-Beda Tetapi Tetap Satu." },
  { "en": "Satu-Satunya Mamalia Yang Bisa Terbang Adalah?", "id": "Kelelawar." },
  { "en": "Periode Apa Yang Merupakan Awal Dari Zaman Dinosaurus?", "id": "Periode Trias." },
  { "en": "Selat Apa Yang Memisahkan Semenanjung Korea Dari Tiongkok?", "id": "Teluk Korea." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1913?", "id": "Rabindranath Tagore." },
  { "en": "Apa Bagian Neuron Yang Menerima Sinyal?", "id": "Dendrit." },
  { "en": "Apa Hasil Dari Perjanjian Portsmouth (1905)?", "id": "Mengakhiri Perang Rusia-Jepang." },
  { "en": "Dari Negara Manakah Asal Pelukis Vincent Van Gogh?", "id": "Belanda." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Radium?", "id": "Marie Dan Pierre Curie." },
  { "en": "Di Negara Manakah Letak Gunung Kilimanjaro?", "id": "Tanzania." },
  { "en": "Keris Adalah Senjata Tradisional Warisan Budaya Dari Mana?", "id": "Indonesia." },
  { "en": "Hewan Apa Yang Diketahui Memiliki Tiga Jantung?", "id": "Gurita Dan Cumi-Cumi." },
  { "en": "Periode Apa Yang Menjadi Puncak Kejayaan Dinosaurus?", "id": "Periode Jura." },
  { "en": "Selat Apa Yang Memisahkan Tasmania Dari Australia?", "id": "Selat Bass." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1905?", "id": "Philipp Lenard." },
  { "en": "Apa Bagian Neuron Yang Mengirim Sinyal Ke Sel Lain?", "id": "Akson." },
  { "en": "Apa Hasil Dari Perjanjian Brest-Litovsk (1918)?", "id": "Rusia Mundur Dari Perang Dunia I." },
  { "en": "Dari Negara Manakah Asal Pelukis Leonardo Da Vinci?", "id": "Italia." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Polonium?", "id": "Marie Curie." },
  { "en": "Di Negara Manakah Letak Gunung Aconcagua?", "id": "Argentina." },
  { "en": "Apa Nama Filosofi Gotong Royong Khas Indonesia?", "id": "Gotong Royong." },
  { "en": "Hewan Apa Yang Tidak Memiliki Otak?", "id": "Bintang Laut, Ubur-Ubur." },
  { "en": "Periode Apa Yang Menjadi Akhir Dari Zaman Dinosaurus?", "id": "Periode Kapur." },
  { "en": "Selat Apa Yang Menghubungkan Samudra Pasifik Dan Arktik?", "id": "Selat Bering." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1901?", "id": "Jacobus Henricus van 't Hoff." },
  { "en": "Apa Nama Selubung Pelindung Yang Menyelimuti Akson?", "id": "Selubung Mielin." },
  { "en": "Apa Hasil Dari Perjanjian Trianon (1920)?", "id": "Membubarkan Kerajaan Hungaria." },
  { "en": "Dari Negara Manakah Asal Pelukis Pablo Picasso?", "id": "Spanyol." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Hidrogen?", "id": "Henry Cavendish." },
  { "en": "Di Negara Manakah Letak Gunung McKinley (Denali)?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Sistem Irigasi Tradisional Di Bali?", "id": "Subak." },
  { "en": "Mamalia Darat Terbesar Saat Ini Adalah?", "id": "Gajah Semak Afrika." },
  { "en": "Era Apa Yang Mengikuti Zaman Dinosaurus?", "id": "Era Kenozoikum." },
  { "en": "Selat Apa Yang Memisahkan Inggris Dan Irlandia?", "id": "Laut Irlandia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1923?", "id": "William Butler Yeats." },
  { "en": "Apa Nama Celah Antara Dua Neuron?", "id": "Sinapsis." },
  { "en": "Apa Hasil Dari Perjanjian Versailles (1919)?", "id": "Mengakhiri Perang Dunia I." },
  { "en": "Dari Negara Manakah Asal Pelukis Salvador Dalí?", "id": "Spanyol." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Nitrogen?", "id": "Daniel Rutherford." },
  { "en": "Di Negara Manakah Letak Gunung Matterhorn?", "id": "Swiss Dan Italia." },
  { "en": "Musyawarah Untuk Mufakat Adalah Prinsip Demokrasi Khas Mana?", "id": "Pancasila (Indonesia)." },
  { "en": "Hewan Apa Yang Dapat Tidur Selama Tiga Tahun?", "id": "Siput." },
  { "en": "Periode Apa Yang Ditandai Munculnya Manusia Pertama?", "id": "Periode Pleistosen." },
  { "en": "Selat Apa Yang Menghubungkan Laut Hitam Dan Laut Marmara?", "id": "Selat Bosporus." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1930?", "id": "Karl Landsteiner." },
  { "en": "Apa Fungsi Utama Dari Saraf Motorik?", "id": "Mengirim Sinyal Dari Otak Ke Otot." },
  { "en": "Apa Hasil Dari Perjanjian Paris (1783)?", "id": "Mengakhiri Perang Revolusi Amerika." },
  { "en": "Dari Negara Manakah Asal Pelukis Claude Monet?", "id": "Perancis." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Fluorin?", "id": "Henri Moissan." },
  { "en": "Di Negara Manakah Letak Gunung Mont Blanc?", "id": "Perancis Dan Italia." },
  { "en": "Apa Nama Rumah Panggung Tradisional Suku Bugis?", "id": "Rumah Saoraja." },
  { "en": "Hewan Apa Yang Diketahui Tidak Pernah Minum Air?", "id": "Tikus Kanguru." },
  { "en": "Zaman Apa Yang Kita Jalani Saat Ini?", "id": "Zaman Holosen." },
  { "en": "Selat Apa Yang Menghubungkan Samudra Hindia Dan Pasifik?", "id": "Selat Malaka." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1962?", "id": "Linus Pauling." },
  { "en": "Apa Fungsi Utama Dari Saraf Sensorik?", "id": "Mengirim Sinyal Dari Indra Ke Otak." },
  { "en": "Apa Tujuan Utama Deklarasi Balfour (1917)?", "id": "Mendukung Tanah Air Yahudi." },
  { "en": "Dari Negara Manakah Asal Pelukis Frida Kahlo?", "id": "Meksiko." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Argon?", "id": "Lord Rayleigh Dan William Ramsay." },
  { "en": "Di Negara Manakah Letak Gunung Elbrus?", "id": "Rusia." },
  { "en": "Apa Nama Seni Bela Diri Khas Indonesia?", "id": "Pencak Silat." },
  { "en": "Hewan Apa Yang Memiliki Sidik Jari Mirip Manusia?", "id": "Koala." },
  { "en": "Zaman Apa Yang Dikenal Sebagai 'Zaman Es'?", "id": "Zaman Pleistosen." },
  { "en": "Selat Apa Yang Memisahkan Florida Dari Kuba?", "id": "Selat Florida." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1921?", "id": "Frederick Soddy." },
  { "en": "Sistem Saraf Apa Yang Mengontrol Fungsi Otomatis Tubuh?", "id": "Sistem Saraf Otonom." },
  { "en": "Apa Hasil Dari Perjanjian San Francisco (1951)?", "id": "Mengakhiri Perang Dengan Jepang." },
  { "en": "Dari Negara Manakah Asal Pelukis Edvard Munch?", "id": "Norwegia." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Neon?", "id": "William Ramsay Dan Morris Travers." },
  { "en": "Di Negara Manakah Letak Gunung Logan?", "id": "Kanada." },
  { "en": "Apa Nama Perahu Tradisional Dengan Cadik Khas Indonesia?", "id": "Perahu Cadik." },
  { "en": "Satu-Satunya Burung Yang Bisa Terbang Mundur Adalah?", "id": "Burung Kolibri." },
  { "en": "Periode Apa Yang Ditandai Munculnya Bunga Pertama?", "id": "Periode Kapur." },
  { "en": "Selat Apa Yang Memisahkan Spanyol Dari Maroko?", "id": "Selat Gibraltar." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1945?", "id": "Gabriela Mistral." },
  { "en": "Sistem Saraf Apa Yang Mengontrol Gerakan Sadar?", "id": "Sistem Saraf Somatik." },
  { "en": "Apa Hasil Dari Perjanjian Maastricht (1992)?", "id": "Membentuk Uni Eropa." },
  { "en": "Dari Negara Manakah Asal Pelukis Gustav Klimt?", "id": "Austria." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Natrium?", "id": "Humphry Davy." },
  { "en": "Di Negara Manakah Letak Gunung Vinson Massif?", "id": "Antartika." },
  { "en": "Noken Adalah Tas Tradisional Dari Serat Kayu Khas Mana?", "id": "Papua." },
  { "en": "Hewan Apa Yang Memiliki Darah Berwarna Biru?", "id": "Kepiting, Laba-Laba, Gurita." },
  { "en": "Periode Apa Yang Ditandai Dominasi Amfibi?", "id": "Periode Karbon." },
  { "en": "Sungai Mekong Melintasi Negara Asia Tenggara Mana Saja?", "id": "Tiongkok, Myanmar, Laos, Thailand." },
  { "en": "Pada Abad Keberapa Martin Luther Memulai Reformasi?", "id": "Abad Ke-16." },
  { "en": "Apa Fungsi Utama Dari Sel Darah Putih?", "id": "Melawan Infeksi Dan Penyakit." },
  { "en": "Apa Pemicu Utama Terjadinya Perang Dunia I?", "id": "Pembunuhan Franz Ferdinand." },
  { "en": "Di Museum Manakah Lukisan 'Mona Lisa' Dipajang?", "id": "Museum Louvre, Paris." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Stetoskop?", "id": "René Laennec." },
  { "en": "Universitas Harvard Terletak Di Dekat Kota Mana?", "id": "Boston (Cambridge), Amerika Serikat." },
  { "en": "Apa Arti Filosofi Bali 'Tri Hita Karana'?", "id": "Tiga Penyebab Kebahagiaan." },
  { "en": "Apa Istilah Untuk Pergerakan Lempeng-Lempeng Bumi?", "id": "Tektonika Lempeng." },
  { "en": "Apa Nama Fase Bulan Saat Terlihat Penuh?", "id": "Bulan Purnama." },
  { "en": "Sungai Rhein Melintasi Negara Eropa Mana Saja?", "id": "Swiss, Jerman, Perancis, Belanda." },
  { "en": "Pada Abad Keberapa Joan of Arc Hidup?", "id": "Abad Ke-15." },
  { "en": "Apa Fungsi Utama Dari Sel Darah Merah?", "id": "Mengangkut Oksigen." },
  { "en": "Apa Penyebab Utama Terjadinya Perang Dunia II?", "id": "Invasi Jerman Ke Polandia." },
  { "en": "Di Museum Manakah Lukisan 'The Starry Night' Dipajang?", "id": "MoMA, New York." },
  { "en": "Siapakah Yang Menemukan Termometer Air Raksa?", "id": "Daniel Gabriel Fahrenheit." },
  { "en": "Universitas Oxford Terletak Di Kota Mana?", "id": "Oxford, Inggris." },
  { "en": "Apa Arti Dari Filosofi Jawa 'Memayu Hayuning Bawana'?", "id": "Memperindah Keindahan Dunia." },
  { "en": "Apa Nama Batuan Cair Panas Di Bawah Kerak Bumi?", "id": "Magma." },
  { "en": "Apa Nama Fase Bulan Saat Tidak Terlihat Dari Bumi?", "id": "Bulan Baru." },
  { "en": "Sungai Danube Melintasi Berapa Negara Di Eropa?", "id": "10 Negara." },
  { "en": "Pada Abad Keberapa Ratu Victoria Memerintah Inggris?", "id": "Abad Ke-19." },
  { "en": "Apa Nama Cairan Kuning Dalam Darah?", "id": "Plasma Darah." },
  { "en": "Apa Penyebab Utama Perang Dingin?", "id": "Persaingan Ideologi AS-Soviet." },
  { "en": "Di Museum Manakah Lukisan 'The Persistence of Memory' Dipajang?", "id": "MoMA, New York." },
  { "en": "Siapakah Yang Menemukan Mikroskop Majemuk?", "id": "Zacharias Janssen." },
  { "en": "Universitas Cambridge Terletak Di Kota Mana?", "id": "Cambridge, Inggris." },
  { "en": "Apa Arti Dari Filosofi Bugis 'Sipakatau, Sipakalebbi, Sipakainge'?", "id": "Saling Menghormati Dan Mengingatkan." },
  { "en": "Apa Nama Batuan Cair Panas Yang Keluar Dari Gunung Api?", "id": "Lava." },
  { "en": "Apa Nama Fase Bulan Saat Terlihat Separuh?", "id": "Kuartal Pertama Atau Ketiga." },
  { "en": "Sungai Zambezi Terkenal Dengan Air Terjun Apa?", "id": "Air Terjun Victoria." },
  { "en": "Pada Abad Keberapa Christopher Columbus Hidup?", "id": "Abad Ke-15 Hingga Ke-16." },
  { "en": "Apa Komponen Darah Yang Berperan Dalam Pembekuan?", "id": "Trombosit (Keping Darah)." },
  { "en": "Apa Penyebab Utama Perang Revolusi Amerika?", "id": "Pajak Tanpa Perwakilan." },
  { "en": "Di Museum Manakah Lukisan 'Guernica' Dipajang?", "id": "Museo Reina Sofía, Madrid." },
  { "en": "Siapakah Yang Menemukan Barometer Air Raksa?", "id": "Evangelista Torricelli." },
  { "en": "Universitas Stanford Terletak Di Negara Bagian Mana?", "id": "California, Amerika Serikat." },
  { "en": "Apa Inti Dari Ajaran 'Tat Twam Asi'?", "id": "Aku Adalah Engkau." },
  { "en": "Apa Nama Proses Batuan Mengalami Perubahan Bentuk?", "id": "Metamorfisme." },
  { "en": "Apa Nama Fase Bulan Yang Berbentuk Sabit?", "id": "Bulan Sabit." },
  { "en": "Sungai Amazon Adalah Sungai Terbesar Di Benua Mana?", "id": "Amerika Selatan." },
  { "en": "Pada Abad Keberapa William Shakespeare Menulis Dramanya?", "id": "Akhir Abad Ke-16." },
  { "en": "Protein Apa Yang Membawa Oksigen Dalam Darah?", "id": "Hemoglobin." },
  { "en": "Apa Penyebab Utama Perang Saudara Amerika?", "id": "Perbudakan Dan Hak Negara Bagian." },
  { "en": "Di Galeri Manakah Patung 'David' Karya Michelangelo?", "id": "Galleria dell'Accademia, Florence." },
  { "en": "Siapakah Yang Menemukan Teleskop Reflektor?", "id": "Isaac Newton." },
  { "en": "Universitas Sorbonne Terletak Di Kota Mana?", "id": "Paris, Perancis." },
  { "en": "Apa Arti Dari Prinsip 'Cipta, Rasa, Karsa'?", "id": "Pikiran, Perasaan, Kehendak." },
  { "en": "Apa Nama Getaran Di Permukaan Bumi Akibat Pelepasan Energi?", "id": "Gempa Bumi." },
  { "en": "Apa Nama Daerah Gelap Di Permukaan Bulan?", "id": "Maria." },
  { "en": "Sungai Nil Adalah Sungai Terpanjang Di Benua Mana?", "id": "Afrika." },
  { "en": "Pada Abad Keberapa Galileo Galilei Hidup?", "id": "Abad Ke-16 Hingga Ke-17." },
  { "en": "Di Mana Sel Darah Merah Diproduksi?", "id": "Sumsum Tulang." },
  { "en": "Apa Pemicu Langsung Perang Vietnam?", "id": "Insiden Teluk Tonkin." },
  { "en": "Di Museum Manakah Lukisan 'The Night Watch' Dipajang?", "id": "Rijksmuseum, Amsterdam." },
  { "en": "Siapakah Yang Menemukan Elektromagnetisme?", "id": "Hans Christian Ørsted." },
  { "en": "Universitas Bologna, Universitas Tertua, Di Negara Mana?", "id": "Italia." },
  { "en": "Apa Nama Falsafah Hidup Masyarakat Jawa?", "id": "Nrimo Ing Pandum." },
  { "en": "Apa Nama Lapisan Terluar Bumi Yang Padat?", "id": "Litosfer." },
  { "en": "Apa Fenomena Saat Bulan Menutupi Matahari?", "id": "Gerhana Matahari." },
  { "en": "Sungai Yangtze Adalah Sungai Terpanjang Di Benua Mana?", "id": "Asia." },
  { "en": "Pada Abad Keberapa Isaac Newton Merumuskan Hukumnya?", "id": "Abad Ke-17." },
  { "en": "Apa Umur Rata-Rata Sel Darah Merah?", "id": "120 Hari." },
  { "en": "Apa Penyebab Utama Dari Perang Korea?", "id": "Invasi Korea Utara Ke Selatan." },
  { "en": "Di Museum Manakah Lukisan 'Las Meninas' Dipajang?", "id": "Museum Prado, Madrid." },
  { "en": "Siapakah Yang Menemukan Generator Listrik (Dinamo)?", "id": "Michael Faraday." },
  { "en": "Massachusetts Institute of Technology (MIT) Terletak Di Kota Mana?", "id": "Cambridge, Amerika Serikat." },
  { "en": "Apa Arti Semboyan Pelaut Bugis 'Sekali Layar Terkembang'?", "id": "Pantang Surut Kembali." },
  { "en": "Apa Nama Proses Pecahnya Batuan Akibat Cuaca?", "id": "Pelapukan." },
  { "en": "Apa Fenomena Saat Bumi Menutupi Bulan?", "id": "Gerhana Bulan." },
  { "en": "Sungai Murray Adalah Sungai Terpanjang Di Benua Mana?", "id": "Australia." },
  { "en": "Pada Abad Keberapa Albert Einstein Mengemukakan Relativitas?", "id": "Awal Abad Ke-20." },
  { "en": "Apa Nama Kondisi Kekurangan Sel Darah Merah?", "id": "Anemia." },
  { "en": "Apa Penyebab Utama Perang Tiga Puluh Tahun?", "id": "Konflik Agama Dan Politik." },
  { "en": "Di Museum Manakah Patung 'Venus de Milo' Berada?", "id": "Museum Louvre, Paris." },
  { "en": "Siapakah Yang Menemukan Mesin Cetak Modern?", "id": "Johannes Gutenberg." },
  { "en": "Universitas Al-Azhar, Salah Satu Yang Tertua, Di Kota Mana?", "id": "Kairo, Mesir." },
  { "en": "Apa Falsafah Hidup Orang Sunda 'Silih Asah, Silih Asih'?", "id": "Saling Mengajari Dan Menyayangi." },
  { "en": "Apa Nama Proses Pergerakan Benua Di Bumi?", "id": "Pergeseran Benua." },
  { "en": "Apa Nama Fase Bulan Antara Kuartal Dan Purnama?", "id": "Bulan Cembung." },
  { "en": "Sungai Mississippi Adalah Sungai Terpanjang Di Benua Mana?", "id": "Amerika Utara." },
  { "en": "Pada Abad Keberapa Terjadi Puncak Zaman Renaisans?", "id": "Abad Ke-15 Hingga Ke-16." },
  { "en": "Apa Peran Utama Dari Plasma Darah?", "id": "Mengangkut Sel Dan Nutrisi." },
  { "en": "Apa Penyebab Perang Napoleon Di Eropa?", "id": "Ambisi Ekspansi Napoleon." },
  { "en": "Di Galeri Manakah 'The Birth of Venus' Dipajang?", "id": "Galeri Uffizi, Florence." },
  { "en": "Siapakah Yang Menemukan Baja Tahan Karat?", "id": "Harry Brearley." },
  { "en": "Universitas Heidelberg, Tertua Di Jerman, Di Kota Mana?", "id": "Heidelberg." },
  { "en": "Apa Falsafah Toraja 'Misa' Kada Dipotuo, Pantan Kada Dipomate'?", "id": "Kesatuan Dalam Kehidupan." },
  { "en": "Apa Nama Patahan Terkenal Di California?", "id": "Patahan San Andreas." },
  { "en": "Apa Nama Bayangan Tergelap Saat Gerhana?", "id": "Umbra." },
  { "en": "Di Negara Manakah Letak Kepulauan Lofoten?", "id": "Norwegia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1969?", "id": "Samuel Beckett." },
  { "en": "Apa Nama Tulang Di Lengan Bawah Sisi Ibu Jari?", "id": "Tulang Radius." },
  { "en": "Raja Louis XIV Dari Perancis Memerintah Selama Era Apa?", "id": "Era Absolutisme." },
  { "en": "Dari Negara Manakah Asal Komponis Johann Sebastian Bach?", "id": "Jerman." },
  { "en": "Siapakah Yang Menemukan Bakteri Penyebab Tuberkulosis?", "id": "Robert Koch." },
  { "en": "Peso Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Meksiko, Filipina, Argentina." },
  { "en": "Apa Nama Hukum Adat Yang Berlaku Di Minangkabau?", "id": "Adat Perpatih." },
  { "en": "Apa Nama Superbenua Purba Yang Mencakup Semua Daratan?", "id": "Pangea." },
  { "en": "Prinsip Apa Yang Menyatakan Benda Mempertahankan Geraknya?", "id": "Hukum Inersia." },
  { "en": "Di Negara Manakah Letak Kepulauan Azores?", "id": "Portugal." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1971?", "id": "Willy Brandt." },
  { "en": "Apa Nama Tulang Di Lengan Bawah Sisi Kelingking?", "id": "Tulang Ulna." },
  { "en": "Ratu Elizabeth I Dari Inggris Memerintah Selama Era Apa?", "id": "Era Elizabethan." },
  { "en": "Dari Negara Manakah Asal Komponis Wolfgang Amadeus Mozart?", "id": "Austria." },
  { "en": "Siapakah Yang Mengembangkan Vaksin Rabies Pertama?", "id": "Louis Pasteur." },
  { "en": "Dolar Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Amerika Serikat, Kanada, Australia." },
  { "en": "Apa Nama Sistem Kekerabatan Matrilineal Dari Minangkabau?", "id": "Matrilineal." },
  { "en": "Apa Nama Superbenua Purba Di Belahan Bumi Selatan?", "id": "Gondwana." },
  { "en": "Hukum Newton Kedua Menghubungkan Gaya, Massa, Dan Apa?", "id": "Percepatan." },
  { "en": "Di Negara Manakah Letak Kepulauan Canary?", "id": "Spanyol." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1901?", "id": "Wilhelm Röntgen." },
  { "en": "Apa Nama Tulang Yang Membentuk Pergelangan Tangan?", "id": "Tulang Karpal." },
  { "en": "Kaisar Meiji Dari Jepang Memerintah Selama Periode Apa?", "id": "Restorasi Meiji." },
  { "en": "Dari Negara Manakah Asal Komponis Ludwig van Beethoven?", "id": "Jerman." },
  { "en": "Siapakah Yang Menemukan Struktur Heliks Ganda DNA?", "id": "Watson Dan Crick." },
  { "en": "Franc Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Swiss." },
  { "en": "Apa Nama Sistem Irigasi Tradisional Khas Bali?", "id": "Subak." },
  { "en": "Apa Nama Superbenua Purba Di Belahan Bumi Utara?", "id": "Laurasia." },
  { "en": "Hukum Newton Ketiga Dikenal Sebagai Prinsip Apa?", "id": "Aksi-Reaksi." },
  { "en": "Di Negara Manakah Letak Kepulauan Faroe?", "id": "Denmark." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1908?", "id": "Ernest Rutherford." },
  { "en": "Apa Nama Tulang Yang Membentuk Telapak Tangan?", "id": "Tulang Metakarpal." },
  { "en": "Suleiman Agung Memerintah Selama Puncak Kekuasaan Kekaisaran Apa?", "id": "Kekaisaran Ottoman." },
  { "en": "Dari Negara Manakah Asal Komponis Pyotr Tchaikovsky?", "id": "Rusia." },
  { "en": "Siapakah Yang Mengemukakan Prinsip Ketidakpastian?", "id": "Werner Heisenberg." },
  { "en": "Lira Pernah Menjadi Mata Uang Resmi Negara Mana?", "id": "Italia." },
  { "en": "Apa Nama Falsafah Hidup Rukun Masyarakat Jawa?", "id": "Guyub Rukun." },
  { "en": "Apa Nama Proses Terpisahnya Lempeng Tektonik?", "id": "Pemekaran Benua (Rifting)." },
  { "en": "Hukum Apa Yang Menjelaskan Tarikan Antar Planet?", "id": "Hukum Gravitasi Universal." },
  { "en": "Di Negara Manakah Letak Kepulauan Galapagos?", "id": "Ekuador." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1906?", "id": "Golgi Dan Cajal." },
  { "en": "Apa Nama Tulang Yang Membentuk Pergelangan Kaki?", "id": "Tulang Tarsal." },
  { "en": "Raja Hammurabi Terkenal Karena Hukumnya Dari Peradaban Mana?", "id": "Babilonia." },
  { "en": "Dari Negara Manakah Asal Komponis Antonio Vivaldi?", "id": "Italia." },
  { "en": "Siapakah Yang Mengklasifikasikan Golongan Darah A, B, O?", "id": "Karl Landsteiner." },
  { "en": "Dirham Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Uni Emirat Arab, Maroko." },
  { "en": "Apa Nama Upacara Adat Untuk Menyambut Tamu Di Papua?", "id": "Bakar Batu." },
  { "en": "Apa Nama Proses Lempeng Samudra Menyelusup Ke Bawah Benua?", "id": "Subduksi." },
  { "en": "Prinsip Siapakah Yang Menjadi Dasar Hidrostatika?", "id": "Prinsip Archimedes." },
  { "en": "Di Negara Manakah Letak Kepulauan Aleutian?", "id": "Amerika Serikat (Alaska)." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1989?", "id": "Dalai Lama Ke-14." },
  { "en": "Apa Nama Tulang Yang Membentuk Telapak Kaki?", "id": "Tulang Metatarsal." },
  { "en": "Akbar Agung Adalah Kaisar Terkenal Dari Kekaisaran Mana?", "id": "Kekaisaran Mughal." },
  { "en": "Dari Negara Manakah Asal Komponis Frédéric Chopin?", "id": "Polandia." },
  { "en": "Siapakah Yang Menemukan Hubungan Antara Listrik Dan Magnet?", "id": "Hans Christian Ørsted." },
  { "en": "Dinar Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Kuwait, Yordania, Aljazair." },
  { "en": "Apa Nama Sidang Adat Masyarakat Melayu?", "id": "Musyawarah." },
  { "en": "Apa Nama Titik Pertemuan Tiga Lempeng Tektonik?", "id": "Triple Junction." },
  { "en": "Hukum Apa Yang Menjelaskan Perilaku Gas Ideal?", "id": "Hukum Gas Ideal." },
  { "en": "Di Negara Manakah Letak Kepulauan Shetland?", "id": "Skotlandia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1949?", "id": "William Faulkner." },
  { "en": "Apa Nama Tulang Terbesar Dan Terkuat Di Tubuh?", "id": "Tulang Paha (Femur)." },
  { "en": "Justinian I Adalah Kaisar Terkenal Dari Kekaisaran Mana?", "id": "Kekaisaran Bizantium." },
  { "en": "Dari Negara Manakah Asal Komponis Franz Liszt?", "id": "Hungaria." },
  { "en": "Siapakah Yang Merumuskan Hukum Elektrolisis?", "id": "Michael Faraday." },
  { "en": "Rupiah Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Indonesia." },
  { "en": "Apa Arti Dari Falsafah Sunda 'Cageur, Bageur, Bener'?", "id": "Sehat, Baik, Benar." },
  { "en": "Apa Nama Proses Terbentuknya Gunung Berapi?", "id": "Vulkanisme." },
  { "en": "Prinsip Apa Yang Menjelaskan Efek Doppler?", "id": "Perubahan Frekuensi Gelombang." },
  { "en": "Di Negara Manakah Letak Kepulauan Hebrides?", "id": "Skotlandia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1918?", "id": "Max Planck." },
  { "en": "Apa Nama Tulang Yang Membentuk Jari Tangan Dan Kaki?", "id": "Falang." },
  { "en": "Charlemagne Adalah Kaisar Pertama Dari Kekaisaran Mana?", "id": "Kekaisaran Romawi Suci." },
  { "en": "Dari Negara Manakah Asal Komponis Giuseppe Verdi?", "id": "Italia." },
  { "en": "Siapakah Yang Mengemukakan Teori Kuantum Elektrodinamika?", "id": "Richard Feynman." },
  { "en": "Ringgit Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Malaysia." },
  { "en": "Apa Nama Tradisi Lisan Bercerita Di Sumatera Barat?", "id": "Kaba." },
  { "en": "Apa Nama Proses Pengikisan Pantai Oleh Gelombang?", "id": "Abrasi." },
  { "en": "Hukum Apa Yang Menjelaskan Hubungan Tekanan Dan Kedalaman?", "id": "Hukum Hidrostatik." },
  { "en": "Di Negara Manakah Letak Kepulauan Svalbard?", "id": "Norwegia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1957?", "id": "Albert Camus." },
  { "en": "Apa Nama Tulang Yang Paling Kecil Di Tubuh Manusia?", "id": "Tulang Sangurdi (Stapes)." },
  { "en": "Kaisar Qin Shi Huang Adalah Pendiri Dinasti Mana?", "id": "Dinasti Qin." },
  { "en": "Dari Negara Manakah Asal Komponis Sergei Rachmaninoff?", "id": "Rusia." },
  { "en": "Siapakah Yang Menemukan Bahwa Alam Semesta Mengembang?", "id": "Edwin Hubble." },
  { "en": "Yen Adalah Nama Mata Uang Resmi Negara Mana?", "id": "Jepang." },
  { "en": "Apa Nama Sistem Kepercayaan Lokal Suku Jawa?", "id": "Kejawen." },
  { "en": "Apa Nama Daerah Di Dasar Laut Yang Sangat Dalam?", "id": "Palung." },
  { "en": "Prinsip Apa Yang Terkait Dengan Kacamata Dan Lensa?", "id": "Pembiasan Cahaya." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Skandinavia?", "id": "Norwegia Dan Swedia." },
  { "en": "Siapa Pemenang Nobel Ekonomi Pada Tahun 2002?", "id": "Daniel Kahneman." },
  { "en": "Apa Nama Ilmiah Untuk Gendang Telinga?", "id": "Membran Timpani." },
  { "en": "Siapakah Yang Dianggap Pendiri Sejati Dinasti Qing?", "id": "Hong Taiji." },
  { "en": "Apa Arti Istilah Tempo 'Andante' Dalam Musik?", "id": "Tempo Berjalan Perlahan." },
  { "en": "Siapakah Yang Membuat Termometer Praktis Pertama?", "id": "Daniel Gabriel Fahrenheit." },
  { "en": "Kota Wina Di Austria Terletak Di Tepi Sungai Apa?", "id": "Sungai Danube." },
  { "en": "Apa Fungsi Utama Noken Bagi Masyarakat Papua?", "id": "Tas Tradisional Serbaguna." },
  { "en": "Bintang Laut Termasuk Dalam Kelas Hewan Apa?", "id": "Asteroidea." },
  { "en": "Apa Nama Bintik-Bintik Gelap Di Permukaan Matahari?", "id": "Bintik Matahari." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Iberia?", "id": "Spanyol Dan Portugal." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1953?", "id": "Winston Churchill." },
  { "en": "Apa Nama Tiga Tulang Kecil Di Telinga Tengah?", "id": "Maleus, Inkus, Stapes." },
  { "en": "Siapakah Pendiri Dinasti Mughal Di India?", "id": "Babur." },
  { "en": "Apa Arti Istilah Tempo 'Allegro' Dalam Musik?", "id": "Cepat Dan Riang." },
  { "en": "Siapakah Yang Menemukan Hukum Gerak Planet?", "id": "Johannes Kepler." },
  { "en": "Kota Budapest Di Hungaria Terletak Di Tepi Sungai Apa?", "id": "Sungai Danube." },
  { "en": "Apa Fungsi Utama Dari Ritual Rambu Solo'?", "id": "Upacara Pemakaman." },
  { "en": "Teripang Termasuk Dalam Kelas Hewan Apa?", "id": "Holothuroidea." },
  { "en": "Apa Nama Lapisan Matahari Yang Terlihat Dari Bumi?", "id": "Fotosfer." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Korea?", "id": "Korea Utara Dan Korea Selatan." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1906?", "id": "J. J. Thomson." },
  { "en": "Apa Bagian Telinga Dalam Yang Berfungsi Untuk Keseimbangan?", "id": "Kanal Semisirkularis." },
  { "en": "Siapakah Pendiri Dinasti Achaemenid Di Persia?", "id": "Koresh Agung." },
  { "en": "Apa Arti Istilah Tempo 'Presto' Dalam Musik?", "id": "Sangat Cepat." },
  { "en": "Siapakah Yang Mengembangkan Teori Kuantum?", "id": "Max Planck." },
  { "en": "Kota Praha Di Ceko Terletak Di Tepi Sungai Apa?", "id": "Sungai Vltava." },
  { "en": "Apa Tujuan Utama Dari Upacara Ngaben Di Bali?", "id": "Mengembalikan Roh Ke Alam Asal." },
  { "en": "Bulu Babi Termasuk Dalam Kelas Hewan Apa?", "id": "Echinoidea." },
  { "en": "Apa Nama Lapisan Atmosfer Matahari Di Atas Fotosfer?", "id": "Kromosfer." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Arab?", "id": "Arab Saudi, Yaman, Oman, UEA." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1911?", "id": "Marie Curie." },
  { "en": "Apa Nama Saraf Yang Menghubungkan Telinga Ke Otak?", "id": "Saraf Auditori." },
  { "en": "Siapakah Pendiri Dinasti Maurya Di India?", "id": "Chandragupta Maurya." },
  { "en": "Apa Arti Istilah Dinamika 'Piano' (p) Dalam Musik?", "id": "Lembut." },
  { "en": "Siapakah Yang Menemukan Struktur DNA?", "id": "Watson Dan Crick." },
  { "en": "Kota London Di Inggris Terletak Di Tepi Sungai Apa?", "id": "Sungai Thames." },
  { "en": "Apa Tujuan Utama Dari Upacara Sekaten?", "id": "Memperingati Maulid Nabi Muhammad." },
  { "en": "Lili Laut Termasuk Dalam Kelas Hewan Apa?", "id": "Crinoidea." },
  { "en": "Apa Fenomena Lontaran Materi Dari Matahari?", "id": "Lidah Api Matahari (Solar Flare)." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Balkan?", "id": "Yunani, Albania, Bulgaria, Dll." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1923?", "id": "Banting Dan Macleod." },
  { "en": "Apa Nama Lensa Transparan Di Bagian Depan Mata?", "id": "Kornea." },
  { "en": "Siapakah Pendiri Dinasti Han Di Tiongkok?", "id": "Liu Bang." },
  { "en": "Apa Arti Istilah Dinamika 'Forte' (f) Dalam Musik?", "id": "Keras." },
  { "en": "Siapakah Yang Mengemukakan Teori Relativitas Umum?", "id": "Albert Einstein." },
  { "en": "Kota Paris Di Perancis Terletak Di Tepi Sungai Apa?", "id": "Sungai Seine." },
  { "en": "Apa Tujuan Utama Dari Tradisi Lompat Batu Nias?", "id": "Ujian Kedewasaan Pria." },
  { "en": "Cacing Pipih Termasuk Dalam Filum Hewan Apa?", "id": "Platyhelminthes." },
  { "en": "Apa Sebutan Untuk Angin Yang Berasal Dari Matahari?", "id": "Angin Surya." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Italia?", "id": "Italia, San Marino, Vatikan." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1925?", "id": "George Bernard Shaw." },
  { "en": "Apa Cairan Bening Yang Mengisi Bagian Depan Mata?", "id": "Aqueous Humour." },
  { "en": "Siapakah Pendiri Dinasti Utsmaniyah (Ottoman)?", "id": "Osman I." },
  { "en": "Apa Arti Istilah Tempo 'Largo' Dalam Musik?", "id": "Sangat Lambat." },
  { "en": "Siapakah Yang Menemukan Penisilin?", "id": "Alexander Fleming." },
  { "en": "Kota Roma Di Italia Terletak Di Tepi Sungai Apa?", "id": "Sungai Tiber." },
  { "en": "Apa Tujuan Utama Dari Tradisi Pasola Di Sumba?", "id": "Ritual Memohon Kesuburan." },
  { "en": "Ubur-Ubur Termasuk Dalam Filum Hewan Apa?", "id": "Cnidaria." },
  { "en": "Apa Nama Lapisan Terluar Atmosfer Matahari?", "id": "Korona." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Malaya?", "id": "Malaysia, Thailand, Myanmar." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1964?", "id": "Martin Luther King Jr." },
  { "en": "Apa Gel Bening Yang Mengisi Bola Mata?", "id": "Vitreous Humour." },
  { "en": "Siapakah Pendiri Dinasti Tudor Di Inggris?", "id": "Henry VII." },
  { "en": "Apa Arti Istilah Dinamika 'Mezzo-Forte' (mf)?", "id": "Agak Keras." },
  { "en": "Siapakah Yang Mengembangkan Vaksin Cacar?", "id": "Edward Jenner." },
  { "en": "Kota Kairo Di Mesir Terletak Di Tepi Sungai Apa?", "id": "Sungai Nil." },
  { "en": "Apa Tujuan Utama Dari Kesenian Wayang Kulit?", "id": "Hiburan Dan Pendidikan Moral." },
  { "en": "Spons Termasuk Dalam Filum Hewan Apa?", "id": "Porifera." },
  { "en": "Badai Matahari Disebabkan Oleh Ledakan Besar Apa?", "id": "Lidah Api Matahari." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Indochina?", "id": "Vietnam, Laos, Kamboja." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1918?", "id": "Fritz Haber." },
  { "en": "Apa Bukaan Di Tengah Iris Yang Mengatur Cahaya?", "id": "Pupil." },
  { "en": "Siapakah Pendiri Dinasti Bourbon Di Perancis?", "id": "Henry IV." },
  { "en": "Apa Arti Istilah Dinamika 'Mezzo-Piano' (mp)?", "id": "Agak Lembut." },
  { "en": "Siapakah Yang Menemukan Hukum Gravitasi Universal?", "id": "Isaac Newton." },
  { "en": "Kota Baghdad Di Irak Terletak Di Tepi Sungai Apa?", "id": "Sungai Tigris." },
  { "en": "Apa Fungsi Dari Kesenian Angklung?", "id": "Alat Musik Dan Pendidikan." },
  { "en": "Cacing Tanah Termasuk Dalam Filum Hewan Apa?", "id": "Annelida." },
  { "en": "Apa Nama Siklus Aktivitas Matahari?", "id": "Siklus Matahari (11 Tahun)." },
  { "en": "Negara Apa Saja Yang Ada Di Semenanjung Jutland?", "id": "Denmark Dan Jerman." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1946?", "id": "Hermann Hesse." },
  { "en": "Otot Apa Yang Mengontrol Ukuran Pupil Mata?", "id": "Otot Iris." },
  { "en": "Siapakah Pendiri Dinasti Safawi Di Persia?", "id": "Ismail I." },
  { "en": "Apa Arti Istilah 'Accelerando' Dalam Musik?", "id": "Tempo Semakin Cepat." },
  { "en": "Siapakah Yang Menemukan Teori Evolusi Melalui Seleksi Alam?", "id": "Charles Darwin." },
  { "en": "Kota Moskow Di Rusia Terletak Di Tepi Sungai Apa?", "id": "Sungai Moskva." },
  { "en": "Apa Tujuan Dari Upacara Kasada Suku Tengger?", "id": "Persembahan Kepada Dewa Gunung." },
  { "en": "Siput Termasuk Dalam Filum Hewan Apa?", "id": "Mollusca." },
  { "en": "Apa Yang Terjadi Pada Bintik Matahari Secara Berkala?", "id": "Meningkat Dan Menurun." },
  { "en": "Pulau Gotland Adalah Pulau Terbesar Milik Negara Mana?", "id": "Swedia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1968?", "id": "Yasunari Kawabata." },
  { "en": "Apa Fungsi Utama Hormon Insulin Dalam Tubuh?", "id": "Menurunkan Kadar Gula Darah." },
  { "en": "Perjanjian Apa Yang Menjadi Dasar Hukum Humaniter Internasional?", "id": "Konvensi Jenewa." },
  { "en": "Dari Negara Manakah Asal Pelukis Hieronymus Bosch?", "id": "Belanda." },
  { "en": "Siapa Fisikawan Yang Mempopulerkan Istilah 'Lubang Hitam'?", "id": "John Wheeler." },
  { "en": "Hulu Sungai Nil Berasal Dari Danau Mana?", "id": "Danau Victoria." },
  { "en": "Apa Fungsi Utama Dari Sistem Subak Di Bali?", "id": "Sistem Manajemen Irigasi." },
  { "en": "Apa Nama Pori-Pori Kecil Di Permukaan Daun Tumbuhan?", "id": "Stomata." },
  { "en": "Planet Apa Di Tata Surya Yang Berotasi Miring?", "id": "Uranus." },
  { "en": "Pulau Sakhalin Terletak Di Lepas Pantai Negara Mana?", "id": "Rusia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1923?", "id": "Robert A. Millikan." },
  { "en": "Apa Fungsi Utama Hormon Glukagon Dalam Tubuh?", "id": "Meningkatkan Kadar Gula Darah." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Spanyol-Amerika?", "id": "Perjanjian Paris (1898)." },
  { "en": "Dari Negara Manakah Asal Pelukis Pieter Bruegel the Elder?", "id": "Belanda." },
  { "en": "Siapa Ahli Kimia Yang Mengembangkan Skala Keasaman pH?", "id": "Søren Peder Lauritz Sørensen." },
  { "en": "Hulu Sungai Amazon Berasal Dari Pegunungan Mana?", "id": "Pegunungan Andes." },
  { "en": "Apa Fungsi Utama Dari Upacara Adat Tedak Siten?", "id": "Ritual Turun Tanah Bayi." },
  { "en": "Jaringan Apa Yang Mengangkut Air Di Dalam Tumbuhan?", "id": "Jaringan Xilem." },
  { "en": "Planet Apa Di Tata Surya Yang Berotasi Terbalik?", "id": "Venus." },
  { "en": "Pulau Hokkaido Adalah Bagian Dari Negara Mana?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1935?", "id": "Frédéric dan Irène Joliot-Curie." },
  { "en": "Apa Fungsi Utama Hormon Adrenalin?", "id": "Merespons Stres (Fight-or-Flight)." },
  { "en": "Perjanjian Apa Yang Menyatukan Kerajaan Inggris Dan Skotlandia?", "id": "Acts of Union 1707." },
  { "en": "Dari Negara Manakah Asal Pelukis Edvard Munch?", "id": "Norwegia." },
  { "en": "Siapa Ahli Biologi Yang Mengusulkan Sistem Tiga Domain?", "id": "Carl Woese." },
  { "en": "Hulu Sungai Mississippi Berasal Dari Danau Mana?", "id": "Danau Itasca." },
  { "en": "Apa Fungsi Utama Dari Kesenian Debus Banten?", "id": "Seni Pertunjukan Kekebalan Tubuh." },
  { "en": "Jaringan Apa Yang Mengangkut Makanan Di Tumbuhan?", "id": "Jaringan Floem." },
  { "en": "Atmosfer Planet Manakah Yang Sebagian Besar Karbon Dioksida?", "id": "Venus Dan Mars." },
  { "en": "Pulau Hainan Adalah Provinsi Paling Selatan Negara Mana?", "id": "Tiongkok." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1973?", "id": "Henry Kissinger, Lê Đức Thọ." },
  { "en": "Apa Fungsi Utama Hormon Testosteron?", "id": "Mengembangkan Karakteristik Seksual Pria." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Meksiko-Amerika?", "id": "Perjanjian Guadalupe Hidalgo." },
  { "en": "Dari Negara Manakah Asal Pelukis Francisco Goya?", "id": "Spanyol." },
  { "en": "Siapa Penemu Struktur Benzena Yang Terkenal?", "id": "August Kekulé." },
  { "en": "Hulu Sungai Yangtze Berasal Dari Dataran Tinggi Mana?", "id": "Dataran Tinggi Tibet." },
  { "en": "Apa Fungsi Utama Dari Upacara Adat Seren Taun?", "id": "Ungkapan Syukur Atas Panen." },
  { "en": "Apa Nama Pigmen Yang Memberi Warna Merah Atau Kuning?", "id": "Karotenoid." },
  { "en": "Planet Apa Yang Memiliki Hari Terpanjang?", "id": "Venus." },
  { "en": "Pulau Newfoundland Adalah Bagian Dari Provinsi Mana?", "id": "Newfoundland dan Labrador, Kanada." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1971?", "id": "Pablo Neruda." },
  { "en": "Apa Fungsi Utama Hormon Estrogen?", "id": "Mengatur Siklus Menstruasi Wanita." },
  { "en": "Perjanjian Apa Yang Membagi Kekaisaran Karoling?", "id": "Perjanjian Verdun." },
  { "en": "Dari Negara Manakah Asal Pelukis René Magritte?", "id": "Belgia." },
  { "en": "Siapa Yang Pertama Kali Mengamati Gerak Brown?", "id": "Robert Brown." },
  { "en": "Hulu Sungai Gangga Berasal Dari Gletser Mana?", "id": "Gletser Gangotri." },
  { "en": "Apa Fungsi Utama Dari Kesenian Reog Ponorogo?", "id": "Seni Pertunjukan Dan Ritual." },
  { "en": "Apa Proses Tumbuhan Kehilangan Air Dalam Bentuk Uap?", "id": "Transpirasi." },
  { "en": "Planet Apa Yang Memiliki Badai Debu Terbesar?", "id": "Mars." },
  { "en": "Pulau Prince Edward Adalah Provinsi Terkecil Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1958?", "id": "Beadle, Tatum, Lederberg." },
  { "en": "Apa Fungsi Utama Hormon Kortisol?", "id": "Mengatur Metabolisme Dan Stres." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Falkland?", "id": "Tidak Ada Perjanjian Formal." },
  { "en": "Dari Negara Manakah Asal Pelukis Wassily Kandinsky?", "id": "Rusia." },
  { "en": "Siapa Yang Menciptakan Skala Suhu Celsius?", "id": "Anders Celsius." },
  { "en": "Hulu Sungai Volga Berasal Dari Perbukitan Mana?", "id": "Perbukitan Valdai." },
  { "en": "Apa Nama Sistem Kepercayaan Asli Nusantara?", "id": "Animisme Dan Dinamisme." },
  { "en": "Apa Zat Kayu Keras Yang Menyusun Dinding Sel?", "id": "Lignin." },
  { "en": "Planet Apa Yang Memiliki Suhu Permukaan Paling Panas?", "id": "Venus." },
  { "en": "Pulau Vancouver Adalah Bagian Dari Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Ekonomi Pada Tahun 1976?", "id": "Milton Friedman." },
  { "en": "Apa Fungsi Utama Hormon Melatonin?", "id": "Mengatur Siklus Tidur." },
  { "en": "Perjanjian Apa Yang Menjual Alaska Ke Amerika Serikat?", "id": "Pembelian Alaska." },
  { "en": "Dari Negara Manakah Asal Pelukis Gustav Klimt?", "id": "Austria." },
  { "en": "Siapa Yang Menciptakan Skala Suhu Fahrenheit?", "id": "Daniel Gabriel Fahrenheit." },
  { "en": "Hulu Sungai Danube Berasal Dari Wilayah Mana?", "id": "Hutan Hitam, Jerman." },
  { "en": "Apa Makna Dari Ritual 'Metatah' Di Bali?", "id": "Upacara Potong Gigi." },
  { "en": "Apa Nama Lapisan Lilin Pelindung Pada Daun?", "id": "Kutikula." },
  { "en": "Planet Apa Yang Paling Dekat Dengan Matahari?", "id": "Merkurius." },
  { "en": "Pulau Ellesmere Adalah Bagian Dari Wilayah Mana?", "id": "Nunavut, Kanada." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1983?", "id": "Lech Wałęsa." },
  { "en": "Apa Fungsi Utama Hormon Progesteron?", "id": "Menjaga Kehamilan." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Napoleon?", "id": "Kongres Wina." },
  { "en": "Dari Negara Manakah Asal Pelukis Piet Mondrian?", "id": "Belanda." },
  { "en": "Siapa Yang Menciptakan Skala Suhu Kelvin?", "id": "William Thomson (Lord Kelvin)." },
  { "en": "Hulu Sungai Indus Berasal Dari Dataran Tinggi Mana?", "id": "Dataran Tinggi Tibet." },
  { "en": "Apa Nama Falsafah Bahari Suku Bugis-Makassar?", "id": "Pinisi." },
  { "en": "Apa Nama Gerak Tumbuhan Menuju Rangsang Cahaya?", "id": "Fototropisme." },
  { "en": "Planet Apa Yang Memiliki Suhu Paling Dingin?", "id": "Uranus." },
  { "en": "Pulau Irlandia Terbagi Antara Dua Negara Apa?", "id": "Republik Irlandia Dan Inggris." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1982?", "id": "Gabriel García Márquez." },
  { "en": "Apa Fungsi Utama Hormon Pertumbuhan (GH)?", "id": "Merangsang Pertumbuhan Tubuh." },
  { "en": "Perjanjian Apa Yang Mengakhiri Perang Tujuh Tahun?", "id": "Perjanjian Paris (1763)." },
  { "en": "Dari Negara Manakah Asal Pelukis Jackson Pollock?", "id": "Amerika Serikat." },
  { "en": "Siapa Yang Pertama Kali Mengusulkan Teori Atom?", "id": "Democritus." },
  { "en": "Hulu Sungai Mekong Berasal Dari Dataran Tinggi Mana?", "id": "Dataran Tinggi Tibet." },
  { "en": "Apa Nama Sistem Gotong Royong Pertanian Di Sunda?", "id": "Liliuran." },
  { "en": "Apa Nama Proses Tumbuhan Mengubah Energi Cahaya?", "id": "Fotosintesis." },
  { "en": "Planet Apa Yang Memiliki Kecepatan Angin Tertinggi?", "id": "Neptunus." },
  { "en": "Di Benua Manakah Letak Pegunungan Pyrenees?", "id": "Eropa." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1962?", "id": "John Steinbeck." },
  { "en": "Arteri Apa Yang Membawa Darah Beroksigen Ke Otak?", "id": "Arteri Karotid." },
  { "en": "Pada Tahun Berapakah Perjanjian Westphalia Ditandatangani?", "id": "Tahun 1648." },
  { "en": "Apa Istilah Untuk Musik Dengan Satu Melodi Tunggal?", "id": "Monofoni." },
  { "en": "Unsur Apa Yang Digunakan Dalam Termometer Tradisional?", "id": "Raksa (Merkuri)." },
  { "en": "Sungai Yangtze Di Tiongkok Melintasi Kota Besar Mana?", "id": "Shanghai, Chongqing." },
  { "en": "Apa Nama Rumah Adat Tradisional Suku Batak Toba?", "id": "Rumah Bolon." },
  { "en": "Meskipun Namanya Kuda, Kuda Laut Termasuk Jenis Hewan Apa?", "id": "Ikan." },
  { "en": "Planet Apa Yang Dikenal Memiliki Badai Heksagonal?", "id": "Saturnus." },
  { "en": "Di Benua Manakah Letak Pegunungan Apennine?", "id": "Eropa." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1929?", "id": "Louis de Broglie." },
  { "en": "Vena Apa Yang Membawa Darah Dari Kepala Ke Jantung?", "id": "Vena Jugularis." },
  { "en": "Pada Tahun Berapakah Perjanjian Nanking Ditandatangani?", "id": "Tahun 1842." },
  { "en": "Apa Istilah Untuk Musik Dengan Beberapa Melodi Independen?", "id": "Polifoni." },
  { "en": "Unsur Apa Yang Menjadi Filamen Dalam Bola Lampu Pijar?", "id": "Tungsten." },
  { "en": "Sungai Indus Melintasi Negara Mana Saja?", "id": "Tiongkok, India, Pakistan." },
  { "en": "Apa Nama Rumah Adat Suku Toraja?", "id": "Tongkonan." },
  { "en": "Hiu Putih Besar Termasuk Dalam Kelas Ikan Apa?", "id": "Ikan Bertulang Rawan." },
  { "en": "Planet Apa Yang Terkenal Dengan 'Lembah Mariner'?", "id": "Mars." },
  { "en": "Di Benua Manakah Letak Pegunungan Skandinavia?", "id": "Eropa." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1945?", "id": "Artturi Virtanen." },
  { "en": "Arteri Apa Yang Membawa Darah Ke Ginjal?", "id": "Arteri Renalis." },
  { "en": "Pada Tahun Berapakah Perjanjian Paris (1783) Ditandatangani?", "id": "Tahun 1783." },
  { "en": "Apa Istilah Untuk Musik Dengan Melodi Utama Dan Iringan?", "id": "Homofoni." },
  { "en": "Unsur Radioaktif Apa Yang Digunakan Dalam Pembangkit Listrik Tenaga Nuklir?", "id": "Uranium." },
  { "en": "Sungai Tigris Dan Efrat Melintasi Negara Modern Mana?", "id": "Irak, Suriah, Turki." },
  { "en": "Apa Nama Rumah Adat Panggung Khas Jambi?", "id": "Rumah Panggung Kajang Lako." },
  { "en": "Salamander Raksasa Termasuk Dalam Kelas Hewan Apa?", "id": "Amfibia." },
  { "en": "Planet Apa Yang Memiliki Gunung Berapi Tertinggi?", "id": "Mars (Olympus Mons)." },
  { "en": "Di Benua Manakah Letak Pegunungan Altai?", "id": "Asia." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1946?", "id": "Hermann Joseph Muller." },
  { "en": "Vena Apa Yang Membawa Darah Dari Lengan?", "id": "Vena Sefalika Dan Basilis." },
  { "en": "Pada Tahun Berapakah Perjanjian Camp David Ditandatangani?", "id": "Tahun 1978." },
  { "en": "Apa Istilah Untuk Bentuk Atau Struktur Komposisi Musik?", "id": "Forma Musikal." },
  { "en": "Unsur Apa Yang Digunakan Dalam Baterai Isi Ulang?", "id": "Litium." },
  { "en": "Sungai Gangga Adalah Sungai Suci Di Negara Mana?", "id": "India." },
  { "en": "Apa Nama Rumah Adat Khas Provinsi Riau?", "id": "Rumah Melayu Selaso Jatuh." },
  { "en": "Penyu Belimbing Termasuk Dalam Kelas Hewan Apa?", "id": "Reptilia." },
  { "en": "Planet Apa Yang Memiliki 'Bintik Gelap Besar'?", "id": "Neptunus." },
  { "en": "Di Benua Manakah Letak Pegunungan Carpathia?", "id": "Eropa." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1948?", "id": "T. S. Eliot." },
  { "en": "Arteri Apa Yang Membawa Darah Ke Kaki?", "id": "Arteri Femoralis." },
  { "en": "Pada Tahun Berapakah Perjanjian Maastricht Ditandatangani?", "id": "Tahun 1992." },
  { "en": "Apa Istilah Untuk Bagian Instrumental Di Awal Lagu?", "id": "Intro." },
  { "en": "Unsur Gas Mulia Apa Yang Paling Melimpah Di Udara?", "id": "Argon." },
  { "en": "Sungai Thames Adalah Sungai Terkenal Di Kota Mana?", "id": "London." },
  { "en": "Apa Nama Rumah Adat Panggung Khas Lampung?", "id": "Nowou Sesat." },
  { "en": "Tuna Sirip Biru Termasuk Dalam Kelas Ikan Apa?", "id": "Ikan Bertulang Keras." },
  { "en": "Planet Apa Yang Memiliki Medan Magnet Paling Kuat?", "id": "Jupiter." },
  { "en": "Di Benua Manakah Letak Pegunungan Tian Shan?", "id": "Asia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1978?", "id": "Sadat Dan Begin." },
  { "en": "Vena Apa Yang Merupakan Vena Terpanjang Di Tubuh?", "id": "Vena Safena Besar." },
  { "en": "Pada Tahun Berapakah Perjanjian Tordesillas Ditandatangani?", "id": "Tahun 1494." },
  { "en": "Apa Istilah Untuk Bagian Instrumental Di Tengah Lagu?", "id": "Interlude Atau Solo." },
  { "en": "Unsur Apa Yang Memberi Warna Merah Pada Kembang Api?", "id": "Stronsium." },
  { "en": "Sungai Seine Adalah Sungai Terkenal Di Kota Mana?", "id": "Paris." },
  { "en": "Apa Nama Rumah Adat Panggung Khas Bengkulu?", "id": "Rumah Bubungan Lima." },
  { "en": "Iguana Laut Termasuk Dalam Kelas Hewan Apa?", "id": "Reptilia." },
  { "en": "Planet Apa Yang Memiliki Kemiringan Sumbu Paling Ekstrem?", "id": "Uranus." },
  { "en": "Di Benua Manakah Letak Pegunungan Sierra Nevada?", "id": "Amerika Utara." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1956?", "id": "Shockley, Bardeen, Brattain." },
  { "en": "Apa Nama Arteri Utama Yang Memasok Darah Ke Jantung?", "id": "Arteri Koroner." },
  { "en": "Pada Tahun Berapakah Perjanjian Guadalupe Hidalgo Ditandatangani?", "id": "Tahun 1848." },
  { "en": "Apa Istilah Untuk Bagian Akhir Dari Sebuah Lagu?", "id": "Outro Atau Coda." },
  { "en": "Unsur Apa Yang Memberi Warna Hijau Pada Kembang Api?", "id": "Barium." },
  { "en": "Sungai Tiber Adalah Sungai Terkenal Di Kota Mana?", "id": "Roma." },
  { "en": "Apa Nama Rumah Adat Khas Suku Sasak Di Lombok?", "id": "Bale Tani." },
  { "en": "Ikan Pari Manta Termasuk Dalam Kelas Ikan Apa?", "id": "Ikan Bertulang Rawan." },
  { "en": "Planet Apa Yang Memiliki Awan Asam Sulfat?", "id": "Venus." },
  { "en": "Di Benua Manakah Letak Pegunungan Great Dividing Range?", "id": "Australia." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1964?", "id": "Dorothy Hodgkin." },
  { "en": "Apa Nama Vena Terbesar Yang Masuk Ke Jantung?", "id": "Vena Cava." },
  { "en": "Pada Tahun Berapakah Perjanjian Brest-Litovsk Ditandatangani?", "id": "Tahun 1918." },
  { "en": "Apa Istilah Untuk Pengulangan Bagian Musik?", "id": "Refrain Atau Chorus." },
  { "en": "Unsur Apa Yang Digunakan Untuk Membuat Balon Udara?", "id": "Helium." },
  { "en": "Sungai Chao Phraya Terletak Di Negara Mana?", "id": "Thailand." },
  { "en": "Apa Nama Rumah Adat Khas Nusa Tenggara Timur?", "id": "Rumah Musalaki." },
  { "en": "Kecebong Adalah Tahap Larva Dari Hewan Apa?", "id": "Katak (Amfibia)." },
  { "en": "Atmosfer Planet Manakah Yang Paling Padat?", "id": "Venus." },
  { "en": "Di Benua Manakah Letak Pegunungan Zagros?", "id": "Asia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1958?", "id": "Boris Pasternak." },
  { "en": "Di Mana Letak Katup Aorta Dalam Jantung?", "id": "Antara Ventrikel Kiri Dan Aorta." },
  { "en": "Pada Tahun Berapakah Perjanjian Ghent Ditandatangani?", "id": "Tahun 1814." },
  { "en": "Apa Istilah Untuk Satu Baris Lirik Dalam Lagu?", "id": "Verse." },
  { "en": "Unsur Apa Yang Diperlukan Untuk Membuat Baja?", "id": "Besi Dan Karbon." },
  { "en": "Sungai Spree Adalah Sungai Terkenal Di Kota Mana?", "id": "Berlin." },
  { "en": "Apa Nama Rumah Adat Khas Provinsi Maluku Utara?", "id": "Rumah Sasadu." },
  { "en": "Belut Listrik Sebenarnya Termasuk Jenis Hewan Apa?", "id": "Ikan." },
  { "en": "Planet Apa Yang Memiliki Rotasi Paling Cepat?", "id": "Jupiter." },
  { "en": "Selat Gibraltar Menghubungkan Samudra Atlantik Dengan Laut Apa?", "id": "Laut Mediterania." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1961?", "id": "Ivo Andrić." },
  { "en": "Apa Bagian Otak Yang Mengontrol Emosi Seperti Rasa Takut?", "id": "Amigdala." },
  { "en": "Sultan Mehmed II Dikenal Karena Menaklukkan Kota Apa?", "id": "Konstantinopel." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Mengukur Panjang?", "id": "Meter." },
  { "en": "Apa Nama Unsur Logam Paling Melimpah Di Kerak Bumi?", "id": "Aluminium." },
  { "en": "Apa Istilah Latin Untuk Prinsip Hukum 'Mata Ganti Mata'?", "id": "Lex Talionis." },
  { "en": "Apa Nama Kain Tenun Tradisional Khas Suku Baduy?", "id": "Tenun Baduy." },
  { "en": "Apa Pigmen Yang Memberi Warna Oranye Pada Wortel?", "id": "Beta-Karoten." },
  { "en": "Era Geologis Apa Yang Terjadi Sebelum Era Paleozoikum?", "id": "Era Prakambrium." },
  { "en": "Selat Magelhaens Terletak Di Ujung Selatan Benua Mana?", "id": "Amerika Selatan." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1913?", "id": "Heike Kamerlingh Onnes." },
  { "en": "Apa Bagian Otak Yang Berperan Penting Dalam Pembentukan Memori?", "id": "Hipokampus." },
  { "en": "Raja Richard I Dari Inggris Dikenal Dengan Julukan Apa?", "id": "Richard Si Hati Singa." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Mengukur Massa?", "id": "Kilogram." },
  { "en": "Apa Unsur Non-Logam Paling Melimpah Di Kerak Bumi?", "id": "Oksigen." },
  { "en": "Apa Prinsip Hukum Yang Berarti 'Tidak Ada Yang Boleh Dihukum Tanpa Undang-Undang'?", "id": "Nullum Crimen Sine Lege." },
  { "en": "Apa Nama Kain Batik Dari Pekalongan Dengan Motif Cerah?", "id": "Batik Pesisir." },
  { "en": "Apa Pigmen Yang Memberi Warna Merah Pada Tomat?", "id": "Likopen." },
  { "en": "Era Geologis Apa Yang Merupakan Bagian Dari Zaman Prakambrium?", "id": "Era Arkean Dan Proterozoikum." },
  { "en": "Selat Hormuz Adalah Jalur Air Strategis Di Teluk Apa?", "id": "Teluk Persia." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1903?", "id": "Svante Arrhenius." },
  { "en": "Apa Bagian Batang Otak Yang Mengatur Pernapasan?", "id": "Medula Oblongata." },
  { "en": "Raja Hammurabi Terkenal Karena Menciptakan Apa?", "id": "Kode Hukum Tertulis." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Mengukur Waktu?", "id": "Detik (Sekon)." },
  { "en": "Apa Unsur Yang Menjadi Komponen Utama Inti Bumi?", "id": "Besi Dan Nikel." },
  { "en": "Apa Prinsip Hukum Yang Berarti 'Seseorang Dianggap Tidak Bersalah Sampai Terbukti Sebaliknya'?", "id": "Asas Praduga Tak Bersalah." },
  { "en": "Apa Nama Topi Tradisional Pria Dari Yogyakarta?", "id": "Blangkon." },
  { "en": "Apa Pigmen Yang Memberi Warna Ungu Pada Terong?", "id": "Antosianin." },
  { "en": "Era Paleozoikum Sering Disebut Sebagai Zaman Apa?", "id": "Zaman Kehidupan Purba." },
  { "en": "Selat Dardanella Menghubungkan Laut Marmara Dengan Laut Apa?", "id": "Laut Aegea." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1961?", "id": "Dag Hammarskjöld." },
  { "en": "Apa Bagian Otak Yang Berfungsi Sebagai Pusat Kendali Suhu?", "id": "Hipotalamus." },
  { "en": "Ratu Victoria Dari Inggris Memerintah Selama Revolusi Apa?", "id": "Revolusi Industri." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Arus Listrik?", "id": "Ampere." },
  { "en": "Unsur Apa Yang Paling Banyak Terdapat Di Atmosfer Bumi?", "id": "Nitrogen." },
  { "en": "Apa Prinsip Hukum Yang Melarang Hukuman Ganda?", "id": "Ne Bis In Idem." },
  { "en": "Apa Nama Belati Melengkung Khas Dari Madura?", "id": "Celurit." },
  { "en": "Apa Nama Gula Alami Yang Terdapat Dalam Buah?", "id": "Fruktosa." },
  { "en": "Era Mesozoikum Terdiri Dari Tiga Periode Apa Saja?", "id": "Trias, Jura, Kapur." },
  { "en": "Selat Taiwan Memisahkan Tiongkok Dari Pulau Apa?", "id": "Taiwan." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1902?", "id": "Ronald Ross." },
  { "en": "Apa Bagian Otak Yang Memproses Informasi Visual?", "id": "Korteks Visual." },
  { "en": "Kaisar Augustus Terkenal Karena Memulai Periode Apa?", "id": "Pax Romana (Perdamaian Romawi)." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Suhu?", "id": "Kelvin." },
  { "en": "Unsur Apa Yang Paling Melimpah Di Alam Semesta?", "id": "Hidrogen." },
  { "en": "Apa Istilah Latin Untuk 'Tindakan Tuhan' Dalam Hukum?", "id": "Act of God." },
  { "en": "Apa Nama Senjata Tradisional Khas Suku Sunda?", "id": "Kujang." },
  { "en": "Apa Nama Gula Yang Terdapat Dalam Susu?", "id": "Laktosa." },
  { "en": "Era Kenozoikum Sering Disebut Sebagai Zaman Apa?", "id": "Zaman Kehidupan Baru." },
  { "en": "Selat Cook Memisahkan Dua Pulau Utama Negara Mana?", "id": "Selandia Baru." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1957?", "id": "Albert Camus." },
  { "en": "Apa Bagian Otak Yang Mengatur Gerakan Motorik Halus?", "id": "Serebelum." },
  { "en": "Firaun Tutankhamun Terkenal Karena Apa?", "id": "Makamnya Yang Utuh." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Jumlah Zat?", "id": "Mol." },
  { "en": "Unsur Apa Yang Membuat Darah Berwarna Merah?", "id": "Besi (Dalam Hemoglobin)." },
  { "en": "Apa Istilah Hukum Untuk Bukti Yang Tak Terbantahkan?", "id": "Smoking Gun." },
  { "en": "Apa Nama Kerajinan Perak Bakar Dari Yogyakarta?", "id": "Perak Kotagede." },
  { "en": "Apa Nama Zat Pahit Dalam Kopi?", "id": "Kafein." },
  { "en": "Periode Apa Yang Dimulai Sekitar 2,6 Juta Tahun Lalu?", "id": "Periode Kuarter." },
  { "en": "Selat Denmark Memisahkan Greenland Dari Pulau Apa?", "id": "Islandia." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1922?", "id": "Francis Aston." },
  { "en": "Korteks Serebral Adalah Lapisan Terluar Dari Bagian Otak Mana?", "id": "Otak Besar." },
  { "en": "Cleopatra Adalah Ratu Terakhir Dari Dinasti Apa?", "id": "Dinasti Ptolemaik." },
  { "en": "Apa Satuan Standar Internasional (SI) Untuk Intensitas Cahaya?", "id": "Candela." },
  { "en": "Unsur Apa Yang Terkandung Dalam Klorofil?", "id": "Magnesium." },
  { "en": "Apa Istilah Latin Untuk 'Demi Kebaikan Publik'?", "id": "Pro Bono Publico." },
  { "en": "Apa Nama Kain Tenun Khas Dari Nusa Tenggara Timur?", "id": "Tenun Ikat." },
  { "en": "Apa Senyawa Yang Memberi Rasa Pedas Pada Cabai?", "id": "Capsaicin." },
  { "en": "Periode Apa Yang Ditandai Dengan Munculnya Mamalia Besar?", "id": "Periode Paleogen." },
  { "en": "Selat Mozambique Memisahkan Madagaskar Dari Benua Mana?", "id": "Afrika." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1963?", "id": "Palang Merah Internasional." },
  { "en": "Apa Bagian Sistem Saraf Yang Mengontrol Refleks?", "id": "Sumsum Tulang Belakang." },
  { "en": "Julius Caesar Adalah Diktator Dari Republik Apa?", "id": "Republik Romawi." },
  { "en": "Apa Sebutan Untuk Tujuh Satuan Dasar Dalam Fisika?", "id": "Satuan Pokok SI." },
  { "en": "Unsur Apa Yang Digunakan Sebagai Semikonduktor Dalam Chip?", "id": "Silikon." },
  { "en": "Apa Prinsip Hukum Yang Menyatakan Perjanjian Harus Ditepati?", "id": "Pacta Sunt Servanda." },
  { "en": "Apa Nama Seni Pertunjukan Boneka Dari Jawa Barat?", "id": "Wayang Golek." },
  { "en": "Apa Nama Zat Manis Yang Diproduksi Lebah?", "id": "Madu." },
  { "en": "Periode Apa Yang Ditandai Dengan Munculnya Ikan Pertama?", "id": "Periode Ordovisium." },
  { "en": "Selat Davis Memisahkan Greenland Dari Pulau Apa?", "id": "Pulau Baffin." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1930?", "id": "C. V. Raman." },
  { "en": "Apa Sistem Saraf Yang Terdiri Dari Otak Dan Sumsum?", "id": "Sistem Saraf Pusat." },
  { "en": "Raja Ashoka Agung Adalah Pemimpin Dari Kekaisaran Mana?", "id": "Kekaisaran Maurya." },
  { "en": "Apa Contoh Satuan Turunan Untuk Kecepatan?", "id": "Meter Per Detik." },
  { "en": "Unsur Apa Yang Merupakan Komponen Utama Udara?", "id": "Nitrogen Dan Oksigen." },
  { "en": "Apa Istilah Latin Untuk 'Status Saat Ini'?", "id": "Status Quo." },
  { "en": "Apa Nama Alat Musik Bambu Yang Digoyangkan?", "id": "Angklung." },
  { "en": "Apa Zat Yang Memberi Rasa Pahit Pada Kulit Jeruk?", "id": "Limonen." },
  { "en": "Periode Apa Yang Ditandai Munculnya Tumbuhan Darat Pertama?", "id": "Periode Silur." },
  { "en": "Teluk Meksiko Berbatasan Dengan Tiga Negara Apa Saja?", "id": "Amerika Serikat, Meksiko, Kuba." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1915?", "id": "William Henry Dan Lawrence Bragg." },
  { "en": "Enzim Apa Di Air Liur Yang Memulai Pencernaan Karbohidrat?", "id": "Amilase (Ptyalin)." },
  { "en": "Siapa Penjelajah Spanyol Yang Mencari 'Fountain of Youth'?", "id": "Juan Ponce de León." },
  { "en": "Siapakah Yang Memahat Patung 'The Burghers of Calais'?", "id": "Auguste Rodin." },
  { "en": "Apa Nama Unsur Yang Paling Elektronegatif?", "id": "Fluorin." },
  { "en": "Sungai Rhine (Rhein) Mengalir Melalui Negara Mana Saja?", "id": "Swiss, Jerman, Perancis, Belanda." },
  { "en": "Apa Nama Perahu Layar Tradisional Dari Suku Bugis?", "id": "Perahu Pinisi." },
  { "en": "Kelelawar Termasuk Dalam Ordo Hewan Apa?", "id": "Chiroptera." },
  { "en": "Berapa Jumlah Bulan Yang Diketahui Mengorbit Mars?", "id": "Dua (Fobos Dan Deimos)." },
  { "en": "Teluk Persia Merupakan Perpanjangan Dari Samudra Mana?", "id": "Samudra Hindia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1936?", "id": "Eugene O'Neill." },
  { "en": "Enzim Apa Di Lambung Yang Memecah Protein?", "id": "Pepsin." },
  { "en": "Siapa Penjelajah Inggris Yang Mengelilingi Dunia?", "id": "Sir Francis Drake." },
  { "en": "Siapakah Yang Memahat Patung 'Apollo and Daphne'?", "id": "Gian Lorenzo Bernini." },
  { "en": "Apa Nama Unsur Gas Mulia Yang Paling Ringan?", "id": "Helium." },
  { "en": "Sungai Volga, Terpanjang Di Eropa, Terletak Di Negara Mana?", "id": "Rusia." },
  { "en": "Apa Nama Upacara Adat Bakar Batu Di Papua?", "id": "Barapen." },
  { "en": "Paus Dan Lumba-Lumba Termasuk Dalam Ordo Hewan Apa?", "id": "Cetacea." },
  { "en": "Berapa Jumlah Bulan Yang Diketahui Mengorbit Jupiter?", "id": "Lebih Dari 90." },
  { "en": "Teluk Benggala Terletak Di Bagian Timur Negara Mana?", "id": "India." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1936?", "id": "Peter Debye." },
  { "en": "Zat Apa Yang Dihasilkan Hati Untuk Membantu Pencernaan Lemak?", "id": "Cairan Empedu." },
  { "en": "Siapa Penjelajah Portugis Yang Pertama Mencapai Brasil?", "id": "Pedro Álvares Cabral." },
  { "en": "Siapakah Yang Memahat Patung 'The Kiss'?", "id": "Auguste Rodin." },
  { "en": "Apa Unsur Halogen Yang Berwujud Cair Pada Suhu Kamar?", "id": "Bromin." },
  { "en": "Sungai Mackenzie Adalah Sungai Terpanjang Di Negara Mana?", "id": "Kanada." },
  { "en": "Apa Nama Alat Musik Bambu Dari Sulawesi Utara?", "id": "Kolintang." },
  { "en": "Kucing Dan Harimau Termasuk Dalam Famili Hewan Apa?", "id": "Felidae." },
  { "en": "Berapa Jumlah Bulan Yang Diketahui Mengorbit Saturnus?", "id": "Lebih Dari 140." },
  { "en": "Teluk Hudson Adalah Perairan Besar Di Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1933?", "id": "Thomas Hunt Morgan." },
  { "en": "Di Bagian Mana Dari Usus Halus Penyerapan Nutrisi Terjadi?", "id": "Jejunum Dan Ileum." },
  { "en": "Siapa Penjelajah Yang Memberi Nama Samudra Pasifik?", "id": "Ferdinand Magellan." },
  { "en": "Siapakah Yang Memahat Patung 'Perseus with the Head of Medusa'?", "id": "Benvenuto Cellini." },
  { "en": "Apa Nama Unsur Logam Alkali Yang Paling Reaktif?", "id": "Fransium." },
  { "en": "Sungai Murray Adalah Sungai Terpanjang Di Negara Mana?", "id": "Australia." },
  { "en": "Apa Nama Seni Ukir Kayu Khas Jepara?", "id": "Ukir Jepara." },
  { "en": "Anjing Dan Serigala Termasuk Dalam Famili Hewan Apa?", "id": "Canidae." },
  { "en": "Berapa Jumlah Bulan Yang Diketahui Mengorbit Uranus?", "id": "27." },
  { "en": "Teluk Carpentaria Terletak Di Sebelah Utara Benua Mana?", "id": "Australia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1947?", "id": "André Gide." },
  { "en": "Apa Fungsi Utama Dari Enzim Lipase?", "id": "Memecah Lemak." },
  { "en": "Siapa Penjelajah Viking Yang Mencapai Amerika Utara?", "id": "Leif Erikson." },
  { "en": "Siapakah Yang Memahat Patung 'The Ecstasy of Saint Teresa'?", "id": "Gian Lorenzo Bernini." },
  { "en": "Apa Unsur Paling Padat Yang Ditemukan Secara Alami?", "id": "Osmium." },
  { "en": "Sungai Parana Mengalir Melalui Tiga Negara Apa Saja?", "id": "Brasil, Paraguay, Argentina." },
  { "en": "Apa Nama Topeng Kayu Dalam Pertunjukan Drama Tari Bali?", "id": "Topeng Sidakarya." },
  { "en": "Beruang Dan Panda Termasuk Dalam Famili Hewan Apa?", "id": "Ursidae." },
  { "en": "Berapa Jumlah Bulan Yang Diketahui Mengorbit Neptunus?", "id": "14." },
  { "en": "Teluk Guinea Terletak Di Lepas Pantai Benua Mana?", "id": "Afrika." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1927?", "id": "Arthur Compton Dan C.T.R. Wilson." },
  { "en": "Di Mana Cairan Empedu Disimpan Sebelum Dilepaskan?", "id": "Kantung Empedu." },
  { "en": "Siapa Penjelajah Yang Mencari Kota Emas 'El Dorado'?", "id": "Francisco de Orellana." },
  { "en": "Siapakah Yang Memahat Patung 'Moses'?", "id": "Michelangelo." },
  { "en": "Apa Nama Unsur Yang Memiliki Titik Lebur Tertinggi?", "id": "Tungsten." },
  { "en": "Sungai Orinoco Sebagian Besar Mengalir Di Negara Mana?", "id": "Venezuela." },
  { "en": "Apa Nama Teater Boneka Kayu Tradisional Dari Jawa Barat?", "id": "Wayang Golek." },
  { "en": "Kuda Dan Zebra Termasuk Dalam Famili Hewan Apa?", "id": "Equidae." },
  { "en": "Berapa Jumlah Planet Kerdil Yang Diakui Di Tata Surya?", "id": "Lima." },
  { "en": "Teluk Alaska Adalah Perpanjangan Dari Samudra Mana?", "id": "Samudra Pasifik." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1952?", "id": "Albert Schweitzer." },
  { "en": "Apa Fungsi Utama Dari Vili Di Usus Halus?", "id": "Memperluas Area Penyerapan." },
  { "en": "Siapa Penjelajah Yang Mencari Jalur Barat Laut (Northwest Passage)?", "id": "Henry Hudson." },
  { "en": "Siapakah Yang Membuat Patung Gerbang Surga (Gates of Paradise)?", "id": "Lorenzo Ghiberti." },
  { "en": "Apa Unsur Yang Paling Banyak Terdapat Dalam Tubuh Manusia?", "id": "Oksigen." },
  { "en": "Sungai Yukon Mengalir Melalui Kanada Dan Negara Bagian Mana?", "id": "Alaska." },
  { "en": "Apa Nama Tarian Perang Dari Suku Dayak?", "id": "Tari Kancet Papatai." },
  { "en": "Simpanse Dan Gorila Termasuk Dalam Famili Hewan Apa?", "id": "Hominidae." },
  { "en": "Planet Kerdil Terbesar Di Sabuk Asteroid Adalah?", "id": "Ceres." },
  { "en": "Teluk Baffin Memisahkan Greenland Dari Pulau Apa?", "id": "Pulau Baffin." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1901?", "id": "Jacobus Henricus van 't Hoff." },
  { "en": "Apa Fungsi Utama Dari Bakteri Di Usus Besar?", "id": "Membantu Pencernaan Serat." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kekaisaran Aztec?", "id": "Hernán Cortés." },
  { "en": "Siapakah Yang Membuat Patung 'Winged Victory of Samothrace'?", "id": "Seniman Yunani Kuno." },
  { "en": "Apa Unsur Yang Penting Untuk Kesehatan Tulang?", "id": "Kalsium." },
  { "en": "Sungai Colorado Terkenal Karena Membentuk Ngarai Apa?", "id": "Grand Canyon." },
  { "en": "Apa Nama Kesenian Drama Tari Dari Jawa Timur?", "id": "Ludruk." },
  { "en": "Rusa Dan Jerapah Termasuk Dalam Ordo Hewan Apa?", "id": "Artiodactyla." },
  { "en": "Planet Kerdil Apa Yang Paling Jauh Dari Matahari?", "id": "Eris." },
  { "en": "Teluk Biscay Terletak Di Lepas Pantai Negara Mana?", "id": "Perancis Dan Spanyol." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1929?", "id": "Thomas Mann." },
  { "en": "Apa Nama Bagian Akhir Dari Usus Halus?", "id": "Ileum." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kekaisaran Inka?", "id": "Francisco Pizarro." },
  { "en": "Siapakah Yang Memahat Air Mancur Trevi (Trevi Fountain)?", "id": "Nicola Salvi." },
  { "en": "Apa Unsur Yang Memberi Warna Biru Pada Safir?", "id": "Besi Dan Titanium." },
  { "en": "Sungai Rio Grande Membentuk Perbatasan Antara AS Dan Negara Mana?", "id": "Meksiko." },
  { "en": "Apa Nama Teater Tradisional Rakyat Dari Jawa Tengah?", "id": "Ketoprak." },
  { "en": "Tikus Dan Tupai Termasuk Dalam Ordo Hewan Apa?", "id": "Rodentia." },
  { "en": "Apa Planet Kerdil Yang Mengorbit Di Luar Neptunus?", "id": "Pluto, Eris, Makemake, Haumea." },
  { "en": "Pulau Madagaskar Terletak Di Lepas Pantai Benua Mana?", "id": "Afrika." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1965?", "id": "Mikhail Sholokhov." },
  { "en": "Hormon Apa Yang Mengatur Kadar Kalsium Dalam Darah?", "id": "Hormon Paratiroid (PTH)." },
  { "en": "Pada Tahun Berapakah Konvensi Jenewa Pertama Ditandatangani?", "id": "Tahun 1864." },
  { "en": "Johann Sebastian Bach Adalah Komposer Dari Era Musik Apa?", "id": "Era Barok." },
  { "en": "Apa Simbol Kimia Untuk Unsur Stronsium?", "id": "Sr." },
  { "en": "Sungai Tigris Mengalir Melintasi Negara Mana Saja?", "id": "Turki, Suriah, Irak." },
  { "en": "Apa Nama Sistem Organisasi Kemasyarakatan Tradisional Di Bali?", "id": "Banjar." },
  { "en": "Kadal Dan Ular Termasuk Dalam Kelas Hewan Apa?", "id": "Reptilia." },
  { "en": "Planet Apa Di Tata Surya Yang Memiliki Satelit Terbanyak?", "id": "Saturnus." },
  { "en": "Pulau Greenland Terletak Di Antara Samudra Apa Saja?", "id": "Atlantik Dan Arktik." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1914?", "id": "Max von Laue." },
  { "en": "Hormon Apa Yang Dilepaskan Saat Merasa Bahagia?", "id": "Endorfin Dan Serotonin." },
  { "en": "Pada Tahun Berapakah Perjanjian Trianon Ditandatangani?", "id": "Tahun 1920." },
  { "en": "Wolfgang Amadeus Mozart Adalah Komposer Dari Era Musik Apa?", "id": "Era Klasik." },
  { "en": "Apa Simbol Kimia Untuk Unsur Zirkonium?", "id": "Zr." },
  { "en": "Sungai Efrat Mengalir Melintasi Negara Mana Saja?", "id": "Turki, Suriah, Irak." },
  { "en": "Apa Nama Upacara Adat Potong Gigi Di Bali?", "id": "Metatah (Mepandes)." },
  { "en": "Kupu-Kupu Dan Ngengat Termasuk Dalam Ordo Serangga Apa?", "id": "Lepidoptera." },
  { "en": "Planet Apa Yang Memiliki Gunung Berapi Tertinggi?", "id": "Mars (Olympus Mons)." },
  { "en": "Pulau Baffin Adalah Pulau Terbesar Di Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1920?", "id": "Walther Nernst." },
  { "en": "Hormon Apa Yang Merangsang Produksi Sel Darah Merah?", "id": "Eritropoietin (EPO)." },
  { "en": "Pada Tahun Berapakah Perjanjian Portsmouth Ditandatangani?", "id": "Tahun 1905." },
  { "en": "Ludwig van Beethoven Adalah Komposer Transisi Antara Era Apa?", "id": "Klasik Dan Romantik." },
  { "en": "Apa Simbol Kimia Untuk Unsur Molibdenum?", "id": "Mo." },
  { "en": "Sungai Amu Darya Mengalir Melintasi Negara Mana Saja?", "id": "Asia Tengah." },
  { "en": "Apa Nama Senjata Tradisional Mirip Keris Dari Bali?", "id": "Keris Bali." },
  { "en": "Kumbang Termasuk Dalam Ordo Serangga Apa?", "id": "Coleoptera." },
  { "en": "Planet Apa Yang Terkenal Dengan 'Great Red Spot'?", "id": "Jupiter." },
  { "en": "Pulau Sumatra Adalah Pulau Terbesar Keberapa Di Dunia?", "id": "Keenam." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1920?", "id": "August Krogh." },
  { "en": "Hormon Apa Yang Merangsang Kontraksi Rahim Saat Melahirkan?", "id": "Oksitosin." },
  { "en": "Pada Tahun Berapakah Piagam Atlantik Dideklarasikan?", "id": "Tahun 1941." },
  { "en": "Frédéric Chopin Adalah Komposer Dari Era Musik Apa?", "id": "Era Romantik." },
  { "en": "Apa Simbol Kimia Untuk Unsur Paladium?", "id": "Pd." },
  { "en": "Sungai Syr Darya Mengalir Melintasi Negara Mana Saja?", "id": "Asia Tengah." },
  { "en": "Apa Nama Falsafah Hidup Gotong Royong Orang Indonesia?", "id": "Gotong Royong." },
  { "en": "Semut Dan Lebah Termasuk Dalam Ordo Serangga Apa?", "id": "Hymenoptera." },
  { "en": "Planet Apa Yang Terkenal Dengan Cincin Esnya?", "id": "Saturnus." },
  { "en": "Pulau Honshu Adalah Pulau Utama Di Negara Mana?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1947?", "id": "André Gide." },
  { "en": "Hormon Apa Yang Mengatur Rasa Haus?", "id": "Hormon Antidiuretik (ADH)." },
  { "en": "Pada Tahun Berapakah Perjanjian Adams-Onís Ditandatangani?", "id": "Tahun 1819." },
  { "en": "Franz Schubert Adalah Komposer Dari Era Musik Apa?", "id": "Era Romantik Awal." },
  { "en": "Apa Simbol Kimia Untuk Unsur Kadmium?", "id": "Cd." },
  { "en": "Sungai Ural Membentuk Batas Antara Dua Benua Apa?", "id": "Eropa Dan Asia." },
  { "en": "Apa Nama Sistem Musyawarah Untuk Mencapai Kesepakatan?", "id": "Musyawarah Mufakat." },
  { "en": "Lalat Dan Nyamuk Termasuk Dalam Ordo Serangga Apa?", "id": "Diptera." },
  { "en": "Planet Apa Yang Memiliki Hari Terpendek?", "id": "Jupiter." },
  { "en": "Pulau Britania Raya Terdiri Dari Tiga Negara Apa Saja?", "id": "Inggris, Skotlandia, Wales." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1969?", "id": "International Labour Organization (ILO)." },
  { "en": "Hormon Apa Yang Mengatur Metabolisme Karbohidrat?", "id": "Insulin Dan Glukagon." },
  { "en": "Pada Tahun Berapakah Perjanjian Giyanti Ditandatangani?", "id": "Tahun 1755." },
  { "en": "Pyotr Ilyich Tchaikovsky Adalah Komposer Dari Era Musik Apa?", "id": "Era Romantik." },
  { "en": "Apa Simbol Kimia Untuk Unsur Antimon?", "id": "Sb." },
  { "en": "Sungai Irawadi Adalah Sungai Utama Di Negara Mana?", "id": "Myanmar." },
  { "en": "Apa Nama Tradisi Adu Cepat Perahu Di Riau?", "id": "Pacu Jalur." },
  { "en": "Capung Termasuk Dalam Ordo Serangga Apa?", "id": "Odonata." },
  { "en": "Planet Apa Yang Terkenal Dengan Atmosfer Bergaris?", "id": "Jupiter." },
  { "en": "Pulau Victoria Adalah Pulau Terbesar Kedua Di Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1921?", "id": "Albert Einstein." },
  { "en": "Hormon Apa Yang Mengatur Siklus Bangun Dan Tidur?", "id": "Melatonin." },
  { "en": "Perjanjian Giyanti Membagi Kerajaan Apa?", "id": "Kerajaan Mataram." },
  { "en": "Igor Stravinsky Adalah Komposer Dari Era Musik Apa?", "id": "Era Modern." },
  { "en": "Apa Simbol Kimia Untuk Unsur Telurium?", "id": "Te." },
  { "en": "Sungai Kuning (Huang He) Terletak Di Negara Mana?", "id": "Tiongkok." },
  { "en": "Apa Nama Upacara Pemakaman Raja-Raja Jawa?", "id": "Pitra Yadnya." },
  { "en": "Belalang Sembah Termasuk Dalam Ordo Serangga Apa?", "id": "Mantodea." },
  { "en": "Planet Apa Yang Memiliki Warna Biru Karena Metana?", "id": "Neptunus Dan Uranus." },
  { "en": "Pulau Ellesmere Adalah Pulau Terbesar Ketiga Di Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1911?", "id": "Marie Curie." },
  { "en": "Hormon Apa Yang Dilepaskan Saat Seseorang Jatuh Cinta?", "id": "Oksitosin Dan Dopamin." },
  { "en": "Perjanjian Saragosa Mengatur Batas Wilayah Antara Siapa?", "id": "Spanyol Dan Portugal." },
  { "en": "Claude Debussy Adalah Komposer Dari Era Musik Apa?", "id": "Era Impresionis." },
  { "en": "Apa Simbol Kimia Untuk Unsur Bismut?", "id": "Bi." },
  { "en": "Sungai Lena Adalah Salah Satu Sungai Terpanjang Di Mana?", "id": "Siberia, Rusia." },
  { "en": "Apa Nama Permainan Tradisional Menggunakan Bambu Panjang?", "id": "Egrang." },
  { "en": "Kecoa Termasuk Dalam Ordo Serangga Apa?", "id": "Blattodea." },
  { "en": "Planet Apa Yang Memiliki Satelit Bernama Triton?", "id": "Neptunus." },
  { "en": "Pulau Sulawesi Adalah Pulau Terbesar Keberapa Di Dunia?", "id": "Kesebelas." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1978?", "id": "Anwar Sadat Dan Menachem Begin." },
  { "en": "Hormon Apa Yang Berperan Dalam Respon 'Lawan Atau Lari'?", "id": "Adrenalin." },
  { "en": "Perjanjian Bongaya Adalah Perjanjian Antara VOC Dan Kerajaan Mana?", "id": "Kerajaan Gowa." },
  { "en": "Arnold Schoenberg Adalah Komposer Dari Era Musik Apa?", "id": "Era Modern (Ekspresionis)." },
  { "en": "Apa Simbol Kimia Untuk Unsur Polonium?", "id": "Po." },
  { "en": "Sungai Ob Adalah Sungai Besar Di Wilayah Mana?", "id": "Siberia, Rusia." },
  { "en": "Apa Nama Permainan Tradisional Menggunakan Gundu?", "id": "Kelereng." },
  { "en": "Jangkrik Termasuk Dalam Ordo Serangga Apa?", "id": "Orthoptera." },
  { "en": "Planet Apa Yang Memiliki Satelit Bernama Titan?", "id": "Saturnus." },
  { "en": "Apa Nama Danau Tertinggi Di Dunia Yang Dapat Dilayari?", "id": "Danau Titicaca." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1911?", "id": "Wilhelm Wien." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Belakang Tengkorak?", "id": "Tulang Oksipital." },
  { "en": "Revolusi Bunga (Carnation Revolution) Terjadi Di Negara Mana?", "id": "Portugal." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Ska?", "id": "Ritme Off-Beat Yang Cepat." },
  { "en": "Apa Unsur Kedua Paling Melimpah Di Alam Semesta?", "id": "Helium." },
  { "en": "Sungai Nil Mengalir Melalui Berapa Negara?", "id": "11 Negara." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Pulau Sumba?", "id": "Rumah Adat Uma Mbatangu." },
  { "en": "Hewan Apa Yang Memiliki Periode Kehamilan Paling Lama?", "id": "Gajah." },
  { "en": "Berapa Lama Waktu Cahaya Matahari Mencapai Bumi?", "id": "Sekitar 8 Menit 20 Detik." },
  { "en": "Apa Nama Danau Air Tawar Terdalam Di Amerika Utara?", "id": "Great Slave Lake." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1950?", "id": "Bertrand Russell." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Samping Tengkorak?", "id": "Tulang Temporal." },
  { "en": "Revolusi Beludru (Velvet Revolution) Terjadi Di Negara Mana?", "id": "Cekoslowakia." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Funk?", "id": "Ritme Sinkopasi Dan Bassline." },
  { "en": "Apa Unsur Logam Yang Paling Ringan?", "id": "Litium." },
  { "en": "Sungai Amazon Sebagian Besar Mengalir Di Negara Mana?", "id": "Brasil Dan Peru." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Suku Dani?", "id": "Rumah Honai." },
  { "en": "Hewan Apa Yang Jantungnya Berdetak Paling Lambat?", "id": "Paus Biru." },
  { "en": "Apa Sebutan Untuk Batas Luar Tata Surya?", "id": "Awan Oort." },
  { "en": "Apa Nama Danau Terbesar Di Benua Amerika Selatan?", "id": "Danau Maracaibo." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1907?", "id": "Eduard Buchner." },
  { "en": "Apa Nama Tulang Yang Membentuk Dahi Manusia?", "id": "Tulang Frontal." },
  { "en": "Revolusi Oranye (Orange Revolution) Terjadi Di Negara Mana?", "id": "Ukraina." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Disko?", "id": "Ritme Four-On-The-Floor." },
  { "en": "Apa Unsur Yang Paling Banyak Terdapat Di Inti Bumi?", "id": "Besi." },
  { "en": "Sungai Yangtze Adalah Sungai Terpanjang Di Negara Mana?", "id": "Tiongkok." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Aceh?", "id": "Rumoh Aceh." },
  { "en": "Hewan Apa Yang Tidak Memiliki Pita Suara?", "id": "Jerapah." },
  { "en": "Apa Fenomena Saat Planet Melintas Di Depan Bintang?", "id": "Transit." },
  { "en": "Apa Nama Danau Paling Asin Di Dunia?", "id": "Don Juan Pond, Antartika." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1908?", "id": "Metchnikoff Dan Ehrlich." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Atas Tengkorak?", "id": "Tulang Parietal." },
  { "en": "Revolusi Mawar (Rose Revolution) Terjadi Di Negara Mana?", "id": "Georgia." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Reggae?", "id": "Aksen Pada Ketukan Kedua Dan Keempat." },
  { "en": "Apa Unsur Yang Digunakan Untuk Membuat Kaca?", "id": "Silikon." },
  { "en": "Sungai Mississippi Adalah Sungai Terpanjang Di Negara Mana?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Rumah Adat Tradisional Suku Minahasa?", "id": "Rumah Pewaris." },
  { "en": "Hewan Apa Yang Memiliki Gigitan Terkuat?", "id": "Buaya Air Asin." },
  { "en": "Apa Sebutan Untuk Jarak Terdekat Bumi Ke Matahari?", "id": "Perihelion." },
  { "en": "Apa Nama Danau Terbesar Sepenuhnya Di Dalam Kanada?", "id": "Great Bear Lake." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1932?", "id": "John Galsworthy." },
  { "en": "Apa Nama Tulang Yang Membentuk Pipi Manusia?", "id": "Tulang Zigomatik." },
  { "en": "Revolusi Tulip (Tulip Revolution) Terjadi Di Negara Mana?", "id": "Kirgistan." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Heavy Metal?", "id": "Distorsi Gitar Yang Keras." },
  { "en": "Apa Unsur Yang Dibutuhkan Untuk Pembekuan Darah?", "id": "Kalsium." },
  { "en": "Sungai Mekong Adalah Sungai Terpanjang Di Asia Mana?", "id": "Asia Tenggara." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Riau?", "id": "Rumah Selaso Jatuh Kembar." },
  { "en": "Hewan Apa Yang Dapat Melihat Sinar Inframerah?", "id": "Ular." },
  { "en": "Apa Sebutan Untuk Jarak Terjauh Bumi Dari Matahari?", "id": "Aphelion." },
  { "en": "Apa Nama Danau Vulkanik Terbesar Di Dunia?", "id": "Danau Toba." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1968?", "id": "René Cassin." },
  { "en": "Apa Nama Tulang Yang Membentuk Batang Hidung?", "id": "Tulang Hidung (Nasal)." },
  { "en": "Revolusi Anyelir (Carnation Revolution) Terjadi Di Negara Mana?", "id": "Portugal." },
  { "en": "Apa Ciri Khas Utama Dari Genre Musik Punk Rock?", "id": "Tempo Cepat Dan Vokal Agresif." },
  { "en": "Apa Unsur Yang Memberi Warna Hijau Pada Tumbuhan?", "id": "Magnesium (Dalam Klorofil)." },
  { "en": "Sungai Kongo Adalah Sungai Terdalam Di Dunia?", "id": "Benar." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Suku Sasak?", "id": "Bale Lumbung." },
  { "en": "Berapa Jumlah Lengan Yang Dimiliki Bintang Laut?", "id": "Umumnya Lima." },
  { "en": "Apa Sebutan Untuk Keselarasan Tiga Benda Langit?", "id": "Syzygy." },
  { "en": "Apa Nama Danau Kawah Berwarna Biru Di Oregon, AS?", "id": "Crater Lake." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1902?", "id": "Lorentz Dan Zeeman." },
  { "en": "Apa Nama Tulang Yang Membentuk Rahang Atas?", "id": "Maksila." },
  { "en": "Revolusi EDSA Terjadi Di Negara Mana?", "id": "Filipina." },
  { "en": "Apa Ciri Khas Utama Dari Musik Klasik Era Mozart?", "id": "Keseimbangan Dan Kejelasan." },
  { "en": "Apa Unsur Yang Terkandung Dalam Garam Dapur?", "id": "Natrium Dan Klorin." },
  { "en": "Sungai Volga Adalah Sungai Terpanjang Di Benua Mana?", "id": "Eropa." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Suku Nias?", "id": "Omo Sebua." },
  { "en": "Hewan Apa Yang Menghasilkan Mutiara?", "id": "Tiram." },
  { "en": "Apa Fenomena Saat Bulan Berada Di Titik Terdekat Bumi?", "id": "Perigee (Supermoon)." },
  { "en": "Apa Nama Danau Terendah Di Permukaan Bumi?", "id": "Laut Mati." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1902?", "id": "Emil Fischer." },
  { "en": "Apa Nama Tulang Paling Kecil Di Tubuh Manusia?", "id": "Stapes (Tulang Sangurdi)." },
  { "en": "Revolusi Sandinista Terjadi Di Negara Mana?", "id": "Nikaragua." },
  { "en": "Apa Ciri Khas Utama Dari Musik Era Barok?", "id": "Ornamen Rumit Dan Kontras." },
  { "en": "Unsur Apa Yang Terkandung Dalam Intan?", "id": "Karbon." },
  { "en": "Sungai Zambezi Terletak Di Benua Mana?", "id": "Afrika." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Jambi?", "id": "Kajang Lako." },
  { "en": "Hewan Apa Yang Dapat Menyemprotkan Tinta Sebagai Pertahanan?", "id": "Cumi-Cumi." },
  { "en": "Apa Fenomena Saat Bulan Berada Di Titik Terjauh Bumi?", "id": "Apogee." },
  { "en": "Apa Nama Danau Terbesar Di Amerika Tengah?", "id": "Danau Nikaragua." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1921?", "id": "Anatole France." },
  { "en": "Apa Nama Tulang Yang Membentuk Langit-Langit Mulut?", "id": "Tulang Palatina." },
  { "en": "Revolusi Rumania Tahun 1989 Menggulingkan Pemimpin Siapa?", "id": "Nicolae Ceaușescu." },
  { "en": "Apa Ciri Khas Utama Dari Musik Era Romantik?", "id": "Ekspresi Emosional Yang Kuat." },
  { "en": "Unsur Apa Yang Memberi Warna Merah Pada Batu Rubi?", "id": "Kromium." },
  { "en": "Sungai Niger Adalah Sungai Utama Di Benua Mana?", "id": "Afrika." },
  { "en": "Apa Nama Rumah Adat Tradisional Dari Bengkulu?", "id": "Bubungan Lima." },
  { "en": "Hewan Apa Yang Memiliki Leher Paling Panjang?", "id": "Jerapah." },
  { "en": "Apa Sebutan Untuk Jalur Semu Tahunan Matahari Di Langit?", "id": "Ekliptika." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Cinta'?", "id": "Paris." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1904?", "id": "Ivan Pavlov." },
  { "en": "Berapa Jenis Reseptor Rasa Dasar Yang Ada Di Lidah?", "id": "Lima." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Televisi Mekanis?", "id": "John Logie Baird." },
  { "en": "Jembatan Golden Gate Adalah Tipe Jembatan Apa?", "id": "Jembatan Gantung (Suspension)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Air?", "id": "H₂O." },
  { "en": "Museum Louvre Terletak Di Kota Mana?", "id": "Paris, Perancis." },
  { "en": "Apa Nama Upacara Adat Lompat Batu Di Pulau Nias?", "id": "Fahombo." },
  { "en": "Apa Istilah Biologis Untuk Hubungan Saling Menguntungkan?", "id": "Simbiosis Mutualisme." },
  { "en": "Apa Nama Pilar Batu Saat Stalaktit Dan Stalagmit Bertemu?", "id": "Kolom Gua." },
  { "en": "Kota Apa Yang Dijuluki 'Ibu Kota Dunia'?", "id": "New York." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1934?", "id": "Luigi Pirandello." },
  { "en": "Apa Lima Rasa Dasar Yang Dikenali Lidah?", "id": "Manis, Asin, Asam, Pahit, Umami." },
  { "en": "Siapakah Yang Menemukan Proses Vulkanisasi Karet?", "id": "Charles Goodyear." },
  { "en": "Jembatan Brooklyn Di New York Adalah Tipe Jembatan Apa?", "id": "Jembatan Hibrida (Gantung/Kabel)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Karbon Dioksida?", "id": "CO₂." },
  { "en": "Museum British Terletak Di Kota Mana?", "id": "London, Inggris." },
  { "en": "Apa Nama Upacara Adat Kematian Suku Toraja?", "id": "Rambu Solo'." },
  { "en": "Apa Istilah Biologis Untuk Hubungan Satu Untung, Satu Rugi?", "id": "Simbiosis Parasitisme." },
  { "en": "Apa Nama Tirai Batu Yang Terbentuk Di Dinding Gua?", "id": "Drapery Atau Tirai Gua." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Apung'?", "id": "Venesia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1904?", "id": "Lord Rayleigh." },
  { "en": "Rasa Umami Sering Digambarkan Seperti Rasa Apa?", "id": "Gurih Atau Lezat." },
  { "en": "Siapakah Yang Menemukan Rem Angin Kereta Api?", "id": "George Westinghouse." },
  { "en": "Jembatan Sydney Harbour Adalah Tipe Jembatan Apa?", "id": "Jembatan Pelengkung (Arch Bridge)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Garam Dapur?", "id": "NaCl." },
  { "en": "Museum Prado Terletak Di Kota Mana?", "id": "Madrid, Spanyol." },
  { "en": "Apa Nama Upacara Adat Bakar Batu Di Papua?", "id": "Barapen." },
  { "en": "Apa Istilah Biologis Untuk Hubungan Satu Untung, Satu Netral?", "id": "Simbiosis Komensalisme." },
  { "en": "Apa Nama Kolam Air Di Dalam Gua?", "id": "Danau Gua." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Dosa' (Sin City)?", "id": "Las Vegas." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1904?", "id": "Sir William Ramsay." },
  { "en": "Reseptor Rasa Untuk Umami Mendeteksi Senyawa Apa?", "id": "Glutamat." },
  { "en": "Siapakah Penemu Mesin Jahit Praktis Pertama?", "id": "Isaac Singer." },
  { "en": "Jembatan Millau Viaduct Di Perancis Adalah Tipe Jembatan Apa?", "id": "Jembatan Kabel (Cable-Stayed)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Metana?", "id": "CH₄." },
  { "en": "Museum Hermitage Negara Terletak Di Kota Mana?", "id": "St. Petersburg, Rusia." },
  { "en": "Apa Nama Upacara Adat Pemakaman Suku Dayak Ngaju?", "id": "Tiwah." },
  { "en": "Hewan Yang Berburu Hewan Lain Disebut Apa?", "id": "Predator." },
  { "en": "Apa Nama Aliran Air Di Dalam Gua?", "id": "Sungai Bawah Tanah." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Malaikat'?", "id": "Los Angeles." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1915?", "id": "Romain Rolland." },
  { "en": "Reseptor Rasa Untuk Asam Mendeteksi Ion Apa?", "id": "Ion Hidrogen." },
  { "en": "Siapakah Yang Menemukan Lift Dengan Rem Pengaman?", "id": "Elisha Otis." },
  { "en": "Jembatan Akashi Kaikyō Di Jepang Adalah Tipe Jembatan Apa?", "id": "Jembatan Gantung (Suspension)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Oksigen?", "id": "O₂." },
  { "en": "Museum Metropolitan of Art Terletak Di Kota Mana?", "id": "New York, Amerika Serikat." },
  { "en": "Apa Nama Tradisi Perang Tombak Di Sumba?", "id": "Pasola." },
  { "en": "Hewan Yang Diburu Oleh Predator Disebut Apa?", "id": "Mangsa (Prey)." },
  { "en": "Apa Nama Mutiara Yang Terbentuk Di Dalam Gua?", "id": "Mutiara Gua (Cave Pearl)." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Kucing'?", "id": "Kuching, Malaysia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1960?", "id": "Albert Lutuli." },
  { "en": "Apa Nama Tonjolan Kecil Di Permukaan Lidah?", "id": "Papila." },
  { "en": "Siapakah Yang Dianggap Penemu Fonograf?", "id": "Thomas Edison." },
  { "en": "Jembatan Emas (Golden Bridge) Di Vietnam Adalah Tipe Jembatan Apa?", "id": "Jembatan Penopang (Girder)." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Asam Sulfat?", "id": "H₂SO₄." },
  { "en": "Galeri Uffizi Terletak Di Kota Mana?", "id": "Florence, Italia." },
  { "en": "Apa Nama Sistem Kepercayaan Animisme Khas Sunda?", "id": "Sunda Wiwitan." },
  { "en": "Hubungan Antara Lebah Dan Bunga Adalah Contoh Simbiosis Apa?", "id": "Mutualisme." },
  { "en": "Apa Nama Terowongan Yang Dibuat Oleh Air Di Dalam Gua?", "id": "Lorong Gua." },
  { "en": "Kota Apa Yang Dijuluki 'Pengantin Laut' (Bride of the Sea)?", "id": "Venesia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1908?", "id": "Gabriel Lippmann." },
  { "en": "Apa Indera Yang Bergantung Pada Reseptor Kimia?", "id": "Penciuman Dan Pengecap." },
  { "en": "Siapakah Yang Menemukan Dinamit?", "id": "Alfred Nobel." },
  { "en": "Jembatan Rialto Di Venesia Adalah Tipe Jembatan Apa?", "id": "Jembatan Pelengkung Batu." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Amonia?", "id": "NH₃." },
  { "en": "Rijksmuseum Terletak Di Kota Mana?", "id": "Amsterdam, Belanda." },
  { "en": "Apa Nama Ritual Pemanggilan Hujan Khas Jawa?", "id": "Ujungan." },
  { "en": "Hubungan Antara Ikan Badut Dan Anemon Adalah Contoh Apa?", "id": "Komensalisme." },
  { "en": "Apa Nama Ruangan Besar Terbuka Di Dalam Gua?", "id": "Ruang Gua (Chamber)." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Angin'?", "id": "Chicago." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1903?", "id": "Niels Ryberg Finsen." },
  { "en": "Apa Nama Saraf Yang Terhubung Ke Indra Penciuman?", "id": "Saraf Olfaktori." },
  { "en": "Siapakah Yang Menemukan Bakelit, Plastik Sintetis Pertama?", "id": "Leo Baekeland." },
  { "en": "Jembatan Si-o-se-pol Di Iran Adalah Tipe Jembatan Apa?", "id": "Jembatan Pelengkung." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Glukosa?", "id": "C₆H₁₂O₆." },
  { "en": "Museum Nasional Tiongkok Terletak Di Kota Mana?", "id": "Beijing, Tiongkok." },
  { "en": "Apa Nama Tradisi Lisan Dan Sastra Suku Minangkabau?", "id": "Kaba." },
  { "en": "Hubungan Antara Nyamuk Dan Manusia Adalah Contoh Apa?", "id": "Parasitisme." },
  { "en": "Apa Nama Formasi Es Permanen Di Dalam Gua?", "id": "Gletser Gua." },
  { "en": "Kota Apa Yang Dijuluki 'Kota Terlarang'?", "id": "Beijing." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1912?", "id": "Gerhart Hauptmann." },
  { "en": "Apa Indera Yang Dapat Mendeteksi Gelombang Suara?", "id": "Pendengaran." },
  { "en": "Siapakah Yang Menemukan Telegraf Listrik?", "id": "Samuel Morse." },
  { "en": "Jembatan Charles Di Praha Adalah Tipe Jembatan Apa?", "id": "Jembatan Pelengkung Batu." },
  { "en": "Apa Rumus Kimia Sederhana Untuk Asam Klorida?", "id": "HCl." },
  { "en": "Museum Nasional Korea Terletak Di Kota Mana?", "id": "Seoul, Korea Selatan." },
  { "en": "Apa Nama Upacara Syukuran Panen Suku Dayak?", "id": "Gawai Dayak." },
  { "en": "Organisme Yang Hidup Dengan Memakan Sisa Organik Disebut?", "id": "Saprofit." },
  { "en": "Apa Nama Batuan Yang Umum Ditemukan Di Gua?", "id": "Batu Gamping." },
  { "en": "Teluk Persia Terletak Di Antara Jazirah Arab Dan Negara Apa?", "id": "Iran." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1912?", "id": "Gustaf Dalén." },
  { "en": "Tipe Darah Apa Yang Dikenal Sebagai Donor Universal?", "id": "Tipe O Negatif." },
  { "en": "Marco Polo Adalah Penjelajah Yang Terkenal Menjelajahi Benua Apa?", "id": "Asia." },
  { "en": "Kelompok Musik Empat Pemain Gesek Disebut Apa?", "id": "Kuartet Gesek." },
  { "en": "Siapakah Ilmuwan Yang Pertama Kali Mengisolasi Unsur Fosfor?", "id": "Hennig Brand." },
  { "en": "Gunung Vinson Massif Adalah Puncak Tertinggi Di Benua Mana?", "id": "Antartika." },
  { "en": "Apa Nama Kerajinan Perak Terkenal Dari Bali?", "id": "Perak Celuk." },
  { "en": "Hewan Apa Yang Lidahnya Bisa Lebih Panjang Dari Tubuhnya?", "id": "Bunglon." },
  { "en": "Periode Apa Yang Dikenal Dengan 'Ledakan Kambrium'?", "id": "Periode Kambrium." },
  { "en": "Teluk Aden Menghubungkan Laut Merah Dengan Samudra Apa?", "id": "Samudra Hindia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1930?", "id": "Sinclair Lewis." },
  { "en": "Tipe Darah Apa Yang Dikenal Sebagai Resipien Universal?", "id": "Tipe AB Positif." },
  { "en": "James Cook Adalah Penjelajah Yang Memetakan Samudra Apa?", "id": "Samudra Pasifik." },
  { "en": "Kelompok Musik Tiga Pemain Disebut Apa?", "id": "Trio." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Kalium?", "id": "Humphry Davy." },
  { "en": "Gunung Kosciuszko Adalah Puncak Tertinggi Di Benua Mana?", "id": "Australia." },
  { "en": "Apa Nama Kerajinan Ukir Kayu Khas Jepara?", "id": "Ukir Jepara." },
  { "en": "Hewan Apa Yang Merupakan Mamalia Tertinggi?", "id": "Jerapah." },
  { "en": "Periode Apa Yang Mengakhiri Era Paleozoikum?", "id": "Periode Permian." },
  { "en": "Teluk Biscay Berada Di Lepas Pantai Negara Mana?", "id": "Perancis Dan Spanyol." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1906?", "id": "Henri Moissan." },
  { "en": "Sistem Golongan Darah ABO Ditemukan Oleh Siapa?", "id": "Karl Landsteiner." },
  { "en": "Vasco Da Gama Adalah Penjelajah Pertama Yang Mencapai Mana?", "id": "India Melalui Laut." },
  { "en": "Kelompok Musik Lima Pemain Tiup Logam Disebut Apa?", "id": "Kuintet Kuningan." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Natrium?", "id": "Humphry Davy." },
  { "en": "Gunung Elbrus Adalah Puncak Tertinggi Di Benua Mana?", "id": "Eropa." },
  { "en": "Apa Nama Kerajinan Tenun Khas Suku Dayak?", "id": "Tenun Ikat Dayak." },
  { "en": "Hewan Apa Yang Tidak Memiliki Tulang Belakang?", "id": "Invertebrata." },
  { "en": "Periode Apa Yang Ditandai Dengan Munculnya Ikan Pertama?", "id": "Periode Ordovisium." },
  { "en": "Teluk Bothnia Terletak Di Antara Negara Mana?", "id": "Swedia Dan Finlandia." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1904?", "id": "Ivan Pavlov." },
  { "en": "Faktor Rhesus (Rh) Dalam Darah Dinamai Dari Hewan Apa?", "id": "Monyet Rhesus." },
  { "en": "Ferdinand Magellan Terkenal Karena Memimpin Ekspedisi Apa?", "id": "Mengelilingi Dunia Pertama." },
  { "en": "Ansambel Musik Besar Yang Memainkan Karya Klasik Disebut Apa?", "id": "Orkestra Simfoni." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Barium?", "id": "Humphry Davy." },
  { "en": "Gunung Logan Adalah Puncak Tertinggi Di Negara Mana?", "id": "Kanada." },
  { "en": "Apa Nama Kerajinan Logam Khas Kotagede?", "id": "Kerajinan Perak Bakar." },
  { "en": "Hewan Apa Yang Diketahui Memiliki Mata Terbesar?", "id": "Cumi-Cumi Raksasa." },
  { "en": "Periode Apa Yang Ditandai Dengan Kemunculan Amfibi?", "id": "Periode Devon." },
  { "en": "Teluk Finlandia Menghubungkan Finlandia Dengan Kota Apa?", "id": "St. Petersburg, Rusia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1926?", "id": "Grazia Deledda." },
  { "en": "Tipe Darah Manakah Yang Paling Jarang Ditemukan?", "id": "Tipe AB Negatif." },
  { "en": "Christopher Columbus Adalah Penjelajah Yang Mencapai Benua Apa?", "id": "Benua Amerika." },
  { "en": "Kelompok Penyanyi Tanpa Instrumen Disebut Apa?", "id": "Akapela." },
  { "en": "Siapakah Ilmuwan Yang Mengisolasi Unsur Aluminium?", "id": "Hans Christian Ørsted." },
  { "en": "Gunung Denali Adalah Nama Lain Dari Gunung Apa?", "id": "Gunung McKinley." },
  { "en": "Apa Nama Seni Membuat Pola Pada Kain Dengan Lilin?", "id": "Batik." },
  { "en": "Hewan Apa Yang Dapat Meregenerasi Bagian Tubuhnya?", "id": "Bintang Laut, Cacing Planaria." },
  { "en": "Periode Apa Yang Dikenal Sebagai Zaman Batu Bara?", "id": "Periode Karbon." },
  { "en": "Teluk St. Lawrence Adalah Muara Dari Sungai Apa?", "id": "Sungai St. Lawrence." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1965?", "id": "UNICEF." },
  { "en": "Apa Yang Terjadi Jika Darah Donor Dan Resipien Tidak Cocok?", "id": "Aglutinasi (Penggumpalan)." },
  { "en": "John Cabot Adalah Penjelajah Yang Mengklaim Tanah Untuk Siapa?", "id": "Inggris Di Amerika Utara." },
  { "en": "Grup Musik Jazz Kecil Biasanya Disebut Apa?", "id": "Combo Jazz." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Magnesium?", "id": "Joseph Black." },
  { "en": "Puncak Jaya Adalah Gunung Tertinggi Di Negara Mana?", "id": "Indonesia." },
  { "en": "Apa Nama Seni Lipat Kertas Dari Jepang?", "id": "Origami." },
  { "en": "Hewan Apa Yang Dapat Menyemprotkan Cairan Berbau Busuk?", "id": "Sigung." },
  { "en": "Periode Apa Yang Ditandai Dengan Munculnya Reptil Pertama?", "id": "Periode Karbon." },
  { "en": "Teluk Thailand Berbatasan Dengan Negara Mana Saja?", "id": "Thailand, Kamboja, Vietnam." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1905?", "id": "Philipp Lenard." },
  { "en": "Antigen Apa Yang Terdapat Pada Sel Darah Merah Tipe A?", "id": "Antigen A." },
  { "en": "Amerigo Vespucci Adalah Penjelajah Yang Namanya Digunakan Untuk Apa?", "id": "Benua Amerika." },
  { "en": "Grup Penyanyi Pop Biasanya Disebut Apa?", "id": "Boy Band Atau Girl Group." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Klorin?", "id": "Carl Wilhelm Scheele." },
  { "en": "Gunung Matterhorn Terletak Di Perbatasan Negara Mana?", "id": "Swiss Dan Italia." },
  { "en": "Apa Nama Seni Menulis Indah Khas Arab?", "id": "Kaligrafi." },
  { "en": "Hewan Apa Yang Memiliki Tiga Kelopak Mata?", "id": "Unta." },
  { "en": "Periode Apa Yang Mengawali Era Mesozoikum?", "id": "Periode Trias." },
  { "en": "Teluk Tonkin Terkenal Karena Insiden Perang Apa?", "id": "Perang Vietnam." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1904?", "id": "Sir William Ramsay." },
  { "en": "Antibodi Apa Yang Terdapat Dalam Plasma Darah Tipe B?", "id": "Antibodi Anti-A." },
  { "en": "Hernán Cortés Adalah Penakluk Kekaisaran Apa?", "id": "Kekaisaran Aztec." },
  { "en": "Grup Musisi Yang Memainkan Musik Rock Disebut Apa?", "id": "Band Rock." },
  { "en": "Siapakah Ilmuwan Yang Menemukan Unsur Iodin?", "id": "Bernard Courtois." },
  { "en": "Gunung Fuji Adalah Gunung Tertinggi Di Negara Mana?", "id": "Jepang." },
  { "en": "Apa Nama Seni Merangkai Bunga Dari Jepang?", "id": "Ikebana." },
  { "en": "Hewan Apa Yang Dapat Hidup Di Darat Dan Air?", "id": "Amfibi." },
  { "en": "Periode Apa Yang Menjadi Puncak Zaman Dinosaurus?", "id": "Periode Jura." },
  { "en": "Teluk Aqaba Adalah Perpanjangan Dari Laut Mana?", "id": "Laut Merah." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1923?", "id": "William Butler Yeats." },
  { "en": "Tipe Darah Apa Yang Tidak Memiliki Antigen A Atau B?", "id": "Tipe O." },
  { "en": "Francisco Pizarro Adalah Penakluk Kekaisaran Apa?", "id": "Kekaisaran Inka." },
  { "en": "Grup Musisi Yang Memainkan Musik Tradisional Disebut Apa?", "id": "Ansambel." },
  { "en": "Siapakah Ilmuwan Yang Mengisolasi Unsur Fluorin?", "id": "Henri Moissan." },
  { "en": "Gunung Mont Blanc Adalah Puncak Tertinggi Di Pegunungan Mana?", "id": "Pegunungan Alpen." },
  { "en": "Apa Nama Seni Teater Boneka Khas Jawa?", "id": "Wayang." },
  { "en": "Hewan Apa Yang Memiliki Metabolisme Paling Lambat?", "id": "Kungkang." },
  { "en": "Periode Apa Yang Mengakhiri Era Mesozoikum?", "id": "Periode Kapur." },
  { "en": "Apa Nama Garis Lintang 23.5° Di Sebelah Utara Khatulistiwa?", "id": "Garis Balik Utara." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1907?", "id": "Albert A. Michelson." },
  { "en": "Apa Nama Tulang Yang Melindungi Otak Manusia?", "id": "Tulang Kranium." },
  { "en": "Siapakah Yang Diakui Sebagai Penemu Dinamometer?", "id": "Edmé Régnier." },
  { "en": "Museum Vatikan Terletak Di Kota Sekaligus Negara Mana?", "id": "Kota Vatikan." },
  { "en": "Apa Rumus Kimia Untuk Asam Asetat (Cuka)?", "id": "CH₃COOH." },
  { "en": "Gunung St. Helens Terletak Di Negara Bagian Mana?", "id": "Washington, Amerika Serikat." },
  { "en": "Apa Nama Kain Lurik Khas Dari Wilayah Klaten?", "id": "Lurik ATBM." },
  { "en": "Apa Istilah Untuk Organisme Yang Memakan Organisme Lain?", "id": "Heterotrof." },
  { "en": "Apa Nama Bukit Pasir Yang Dibentuk Oleh Angin?", "id": "Gumuk Pasir." },
  { "en": "Apa Nama Garis Lintang 23.5° Di Sebelah Selatan Khatulistiwa?", "id": "Garis Balik Selatan." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1916?", "id": "Verner von Heidenstam." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Atas Kepala?", "id": "Tulang Parietal." },
  { "en": "Siapakah Yang Menemukan Mesin Rajut Pertama?", "id": "William Lee." },
  { "en": "Museum Seni Modern (MoMA) Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Apa Rumus Kimia Untuk Gas Tawa (Nitrous Oxide)?", "id": "N₂O." },
  { "en": "Gunung Krakatau Terletak Di Selat Mana?", "id": "Selat Sunda." },
  { "en": "Apa Nama Seni Pertunjukan Lenong Dari Betawi?", "id": "Lenong." },
  { "en": "Apa Istilah Untuk Organisme Yang Menghasilkan Makanan Sendiri?", "id": "Autotrof." },
  { "en": "Apa Nama Formasi Tanah Subur Di Muara Sungai?", "id": "Delta." },
  { "en": "Apa Nama Garis Lintang 66.5° Di Sebelah Utara?", "id": "Lingkar Arktik." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1905?", "id": "Adolf von Baeyer." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Depan Kepala?", "id": "Tulang Frontal." },
  { "en": "Siapakah Penemu Balon Udara Panas Pertama?", "id": "Montgolfier Bersaudara." },
  { "en": "Museum Nasional Sejarah Alam Terletak Di Kota Mana?", "id": "Washington, D.C." },
  { "en": "Apa Rumus Kimia Untuk Hidrogen Peroksida?", "id": "H₂O₂." },
  { "en": "Gunung Pinatubo Terletak Di Negara Mana?", "id": "Filipina." },
  { "en": "Apa Nama Tradisi Karapan Sapi Berasal Dari Pulau Mana?", "id": "Madura." },
  { "en": "Apa Istilah Untuk Organisme Yang Memakan Tumbuhan?", "id": "Herbivora." },
  { "en": "Apa Nama Formasi Tanah Yang Sangat Subur Berwarna Hitam?", "id": "Chernozem." },
  { "en": "Apa Nama Garis Lintang 66.5° Di Sebelah Selatan?", "id": "Lingkar Antartika." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1907?", "id": "Charles Louis Alphonse Laveran." },
  { "en": "Apa Nama Tulang Yang Membentuk Bagian Samping Bawah Kepala?", "id": "Tulang Temporal." },
  { "en": "Siapakah Penemu Kapal Uap Komersial Pertama?", "id": "Robert Fulton." },
  { "en": "Tate Modern Adalah Galeri Seni Di Kota Mana?", "id": "London." },
  { "en": "Apa Rumus Kimia Untuk Etanol (Alkohol)?", "id": "C₂H₅OH." },
  { "en": "Gunung Fuji Adalah Gunung Berapi Di Negara Mana?", "id": "Jepang." },
  { "en": "Apa Nama Seni Pertunjukan Khas Dari Jawa Timur?", "id": "Ludruk." },
  { "en": "Apa Istilah Untuk Organisme Yang Memakan Daging?", "id": "Karnivora." },
  { "en": "Apa Nama Cekungan Besar Yang Dihasilkan Oleh Angin?", "id": "Cekungan Deflasi." },
  { "en": "Apa Nama Garis Bujur 180 Derajat?", "id": "Garis Tanggal Internasional." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1902?", "id": "Theodor Mommsen." },
  { "en": "Apa Nama Tulang Yang Menghubungkan Tulang Rusuk Dan Panggul?", "id": "Tulang Belakang." },
  { "en": "Siapakah Penemu Sepeda Pertama Kali?", "id": "Karl von Drais." },
  { "en": "Museum Guggenheim Di Spanyol Terletak Di Kota Mana?", "id": "Bilbao." },
  { "en": "Apa Rumus Kimia Untuk Formaldehida (Formalin)?", "id": "CH₂O." },
  { "en": "Gunung Vesuvius Terletak Di Dekat Kota Mana?", "id": "Napoli, Italia." },
  { "en": "Apa Nama Drama Tari Tradisional Khas Bali?", "id": "Gambuh." },
  { "en": "Apa Istilah Untuk Organisme Yang Memakan Segalanya?", "id": "Omnivora." },
  { "en": "Apa Nama Dataran Rendah Luas Di Pesisir Pantai?", "id": "Dataran Pesisir." },
  { "en": "Apa Nama Garis Imajiner Yang Menjadi Poros Rotasi Bumi?", "id": "Sumbu Bumi." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1917?", "id": "Komite Internasional Palang Merah." },
  { "en": "Apa Nama Tulang Yang Membentuk Sendi Lutut?", "id": "Femur, Tibia, Patela." },
  { "en": "Siapakah Yang Menemukan Mesin Pintal (Spinning Jenny)?", "id": "James Hargreaves." },
  { "en": "Museum Sejarah Alam Amerika Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Apa Rumus Kimia Untuk Gula Pasir (Sukrosa)?", "id": "C₁₂H₂₂O₁₁." },
  { "en": "Gunung Rainier Terletak Di Negara Bagian Mana?", "id": "Washington, Amerika Serikat." },
  { "en": "Apa Nama Seni Tutur Dengan Iringan Musik Dari Riau?", "id": "Syair." },
  { "en": "Apa Istilah Untuk Organisme Yang Memakan Sisa Organik?", "id": "Detritivor." },
  { "en": "Apa Nama Daratan Yang Menjorok Ke Laut?", "id": "Tanjung." },
  { "en": "Apa Nama Garis Yang Membagi Bumi Menjadi Belahan Utara Dan Selatan?", "id": "Khatulistiwa." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1904?", "id": "Lord Rayleigh." },
  { "en": "Apa Nama Tulang Yang Membentuk Sendi Siku?", "id": "Humerus, Radius, Ulna." },
  { "en": "Siapakah Yang Menemukan Mesin Tenun Bertenaga Uap?", "id": "Edmund Cartwright." },
  { "en": "Museum Seni Philadelphia Terkenal Dengan Tangga Di Film Apa?", "id": "Rocky." },
  { "en": "Apa Rumus Kimia Untuk Gas Ozon?", "id": "O₃." },
  { "en": "Gunung Etna Adalah Gunung Berapi Aktif Di Pulau Mana?", "id": "Sisilia, Italia." },
  { "en": "Apa Nama Seni Pertunjukan Komedi Khas Jawa Tengah?", "id": "Ketoprak." },
  { "en": "Apa Istilah Untuk Organisme Yang Mendapat Makanan Dari Inang?", "id": "Parasit." },
  { "en": "Apa Nama Formasi Tanah Beku Permanen Di Kutub?", "id": "Permafrost." },
  { "en": "Apa Nama Garis Yang Menghubungkan Kutub Utara Dan Selatan?", "id": "Garis Bujur (Meridian)." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1903?", "id": "Svante Arrhenius." },
  { "en": "Apa Nama Tulang Yang Membentuk Pergelangan Tangan?", "id": "Tulang Karpal." },
  { "en": "Siapakah Yang Menemukan Proses Pembuatan Baja Murah?", "id": "Henry Bessemer." },
  { "en": "Institut Seni Chicago Terletak Di Kota Mana?", "id": "Chicago." },
  { "en": "Apa Rumus Kimia Untuk Asam Asetat?", "id": "CH₃COOH." },
  { "en": "Gunung Kilimanjaro Terletak Di Negara Mana?", "id": "Tanzania." },
  { "en": "Apa Nama Seni Pertunjukan Khas Masyarakat Betawi?", "id": "Ondel-Ondel." },
  { "en": "Apa Istilah Untuk Organisme Tempat Parasit Hidup?", "id": "Inang (Host)." },
  { "en": "Apa Nama Lapisan Tanah Paling Atas Yang Subur?", "id": "Topsoil." },
  { "en": "Apa Nama Jalur Semu Matahari Di Langit?", "id": "Ekliptika." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1904?", "id": "Mistral Dan Echegaray." },
  { "en": "Apa Nama Tulang Yang Membentuk Pergelangan Kaki?", "id": "Tulang Tarsal." },
  { "en": "Siapakah Yang Menemukan Kaca Pengaman?", "id": "Édouard Bénédictus." },
  { "en": "Museum Nasional Tokyo Terletak Di Kota Mana?", "id": "Tokyo, Jepang." },
  { "en": "Apa Rumus Kimia Untuk Natrium Bikarbonat (Soda Kue)?", "id": "NaHCO₃." },
  { "en": "Gunung Aconcagua Terletak Di Pegunungan Mana?", "id": "Pegunungan Andes." },
  { "en": "Apa Nama Kesenian Musik Patrol Saat Bulan Puasa?", "id": "Musik Patrol." },
  { "en": "Apa Istilah Untuk Rantai Makanan Yang Saling Berhubungan?", "id": "Jaring-Jaring Makanan." },
  { "en": "Apa Nama Lapisan Batuan Dasar Di Bawah Tanah?", "id": "Batuan Dasar (Bedrock)." }



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
