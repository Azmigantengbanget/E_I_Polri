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


  { "en": "Bunyi Hukum Ohm?", "id": "V = I x R." },
  { "en": "Satuan arus listrik?", "id": "Ampere (A)." },
  { "en": "Satuan tegangan listrik?", "id": "Volt (V)." },
  { "en": "Satuan hambatan listrik?", "id": "Ohm (Ω)." },
  { "en": "Fungsi utama resistor?", "id": "Menghambat aliran arus listrik." },
  { "en": "Fungsi utama kapasitor?", "id": "Menyimpan muatan listrik sementara." },
  { "en": "Satuan kapasitansi?", "id": "Farad (F)." },
  { "en": "Fungsi utama induktor?", "id": "Menyimpan energi dalam medan magnet." },
  { "en": "Satuan induktansi?", "id": "Henry (H)." },
  { "en": "Tiga komponen pasif utama?", "id": "Resistor, Kapasitor, dan Induktor." },
  { "en": "Fungsi utama dioda?", "id": "Menyearahkan arus listrik." },
  { "en": "Fungsi utama transistor?", "id": "Sebagai saklar atau penguat sinyal." },
  { "en": "Kepanjangan IC (Integrated Circuit)?", "id": "Integrated Circuit." },
  { "en": "Apa itu IC (Integrated Circuit)?", "id": "Kumpulan komponen dalam satu chip." },
  { "en": "Definisi rangkaian seri?", "id": "Komponen dihubungkan secara berderet." },
  { "en": "Definisi rangkaian paralel?", "id": "Komponen dihubungkan secara berjajar." },
  { "en": "Definisi instrumentasi?", "id": "Ilmu dan seni pengukuran/kontrol." },
  { "en": "Alat ukur tegangan, arus, hambatan?", "id": "Multimeter." },
  { "en": "Alat ukur visualisasi sinyal?", "id": "Osiloskop (Oscilloscope)." },
  { "en": "Apa itu akurasi pengukuran?", "id": "Kedekatan hasil ukur ke nilai sebenarnya." },
  { "en": "Apa itu presisi pengukuran?", "id": "Kedekatan hasil ukur yang berulang." },
  { "en": "Apa itu sensor?", "id": "Perangkat pendeteksi perubahan fisik/kimia." },
  { "en": "Apa itu transduser?", "id": "Mengubah satu bentuk energi ke lain." },
  { "en": "Hubungan sensor dan transduser?", "id": "Sensor adalah jenis dari transduser." },
  { "en": "Sistem bilangan komputer?", "id": "Biner (basis 2)." },
  { "en": "Angka dalam sistem biner?", "id": "Hanya 0 dan 1." },
  { "en": "Tiga gerbang logika dasar?", "id": "AND, OR, NOT." },
  { "en": "Output gerbang AND?", "id": "1 jika semua input bernilai 1." },
  { "en": "Output gerbang OR?", "id": "1 jika salah satu input 1." },
  { "en": "Fungsi gerbang NOT?", "id": "Membalik nilai input (inverter)." },
  { "en": "Apa itu flip-flop?", "id": "Rangkaian penyimpan satu bit data." },
  { "en": "Fungsi catu daya (power supply)?", "id": "Menyediakan tegangan listrik untuk perangkat." },
  { "en": "Proses ubah AC ke DC?", "id": "Penyearahan (rectification)." },
  { "en": "Komponen utama penyearah?", "id": "Dioda atau jembatan dioda." },
  { "en": "Fungsi transformator (trafo)?", "id": "Menaikkan atau menurunkan tegangan AC." },
  { "en": "Alat untuk menyambung komponen?", "id": "Solder." },
  { "en": "Bahan penyambung saat menyolder?", "id": "Timah solder (tenol)." },
  { "en": "Fungsi 'breadboard'?", "id": "Papan untuk merangkai sirkuit sementara." },
  { "en": "Kepanjangan PCB (Printed Circuit Board)?", "id": "Printed Circuit Board." },
  { "en": "Fungsi PCB (Printed Circuit Board)?", "id": "Papan sirkuit permanen untuk komponen." },
  { "en": "Cara ukur tegangan DC (Direct Current)?", "id": "Selektor ke VDC, probe paralel." },
  { "en": "Cara ukur arus DC (Direct Current)?", "id": "Selektor ke ADC, probe seri." },
  { "en": "Cara cek kontinuitas kabel?", "id": "Gunakan mode ohmmeter atau buzzer." },
  { "en": "Apa itu 'short circuit'?", "id": "Hubungan singkat, resistansi sangat rendah." },
  { "en": "Apa itu 'open circuit'?", "id": "Rangkaian terbuka, tidak ada aliran arus." },
  { "en": "Fungsi 'heat sink'?", "id": "Menyerap dan membuang panas komponen." },
  { "en": "Komponen yang butuh 'heat sink'?", "id": "Transistor daya, IC regulator." },
  { "en": "Kepanjangan LED (Light Emitting Diode)?", "id": "Light Emitting Diode." },
  { "en": "Fungsi LED (Light Emitting Diode)?", "id": "Dioda yang memancarkan cahaya." },
  { "en": "Apa itu 'voltage regulator'?", "id": "Komponen penstabil tegangan output." },
  { "en": "Contoh IC (Integrated Circuit) regulator tegangan?", "id": "LM7805, LM7905." },
  { "en": "Fungsi filter pada catu daya?", "id": "Meratakan tegangan DC setelah penyearah." },
  { "en": "Komponen untuk filter catu daya?", "id": "Kapasitor elektrolit (elco)." },
  { "en": "Apa itu sinyal AC (Alternating Current)?", "id": "Arus yang arahnya bolak-balik." },
  { "en": "Apa itu sinyal DC (Direct Current)?", "id": "Arus yang arahnya searah." },
  { "en": "Frekuensi listrik PLN di Indonesia?", "id": "50 Hertz (Hz)." },
  { "en": "Tegangan listrik rumah di Indonesia?", "id": "220 Volt (V)." },
  { "en": "Alat untuk membaca kode warna resistor?", "id": "Tabel kode warna atau multimeter." },
  { "en": "Apa itu potensiometer?", "id": "Resistor yang nilainya bisa diubah." },
  { "en": "Apa itu 'relay'?", "id": "Saklar yang dikendalikan secara elektromagnetik." },
  { "en": "Fungsi utama 'relay'?", "id": "Mengontrol sirkuit tegangan tinggi." },
  { "en": "Apa itu 'operational amplifier' (Op-Amp)?", "id": "Penguat diferensial tegangan tinggi." },
  { "en": "Kepanjangan Op-Amp?", "id": "Operational Amplifier." },
  { "en": "Contoh IC (Integrated Circuit) Op-Amp populer?", "id": "IC 741." },
  { "en": "Bentuk gelombang pada osiloskop?", "id": "Sinus, kotak, segitiga, gigi gergaji." },
  { "en": "Sumbu horizontal osiloskop menunjukkan?", "id": "Waktu (Time/Div)." },
  { "en": "Sumbu vertikal osiloskop menunjukkan?", "id": "Tegangan (Volt/Div)." },
  { "en": "Apa itu 'ground'?", "id": "Titik referensi tegangan nol." },
  { "en": "Apa itu frekuensi?", "id": "Jumlah siklus per detik." },
  { "en": "Apa itu periode?", "id": "Waktu untuk satu siklus lengkap." },
  { "en": "Hubungan frekuensi dan periode?", "id": "Frekuensi = 1 / Periode." },
  { "en": "Kepanjangan ESD (Electrostatic Discharge)?", "id": "Electrostatic Discharge." },
  { "en": "Bahaya ESD (Electrostatic Discharge)?", "id": "Dapat merusak komponen elektronik sensitif." },
  { "en": "Cara mencegah ESD (Electrostatic Discharge)?", "id": "Gunakan gelang anti-statis." },
  { "en": "Apa itu 'soldering iron'?", "id": "Alat pemanas untuk melelehkan timah." },
  { "en": "Apa itu 'desoldering pump'?", "id": "Alat untuk menyedot timah cair." },
  { "en": "Apa itu 'flux'?", "id": "Bahan kimia pembersih saat menyolder." },
  { "en": "Tujuan penggunaan 'flux'?", "id": "Agar timah menempel dengan sempurna." },
  { "en": "Apa itu 'multivibrator'?", "id": "Rangkaian untuk menghasilkan gelombang kotak." },
  { "en": "Tipe 'multivibrator'?", "id": "Astable, monostable, bistable." },
  { "en": "Multivibrator 'astable' menghasilkan?", "id": "Sinyal osilasi terus-menerus." },
  { "en": "Apa itu osilator?", "id": "Rangkaian pembangkit sinyal periodik." },
  { "en": "Fungsi osilator pada jam?", "id": "Menghasilkan detak waktu yang presisi." },
  { "en": "Komponen utama osilator kristal?", "id": "Kristal kuarsa." },
  { "en": "Apa itu mikrokontroler?", "id": "Komputer kecil dalam satu chip." },
  { "en": "Contoh mikrokontroler populer?", "id": "Arduino, Raspberry Pi." },
  { "en": "Bahasa pemrograman untuk Arduino?", "id": "C++." },
  { "en": "Bahasa pemrograman untuk Raspberry Pi?", "id": "Python." },
  { "en": "Apa itu 'input device'?", "id": "Perangkat masukan (contoh: tombol)." },
  { "en": "Apa itu 'output device'?", "id": "Perangkat keluaran (contoh: LED, motor)." },
  { "en": "Fungsi ADC (Analog-to-Digital Converter) di mikrokontroler?", "id": "Membaca data dari sensor analog." },
  { "en": "Fungsi PWM (Pulse Width Modulation) di mikrokontroler?", "id": "Mengontrol kecerahan LED, kecepatan motor." },
  { "en": "Kepanjangan PWM?", "id": "Pulse Width Modulation." },
  { "en": "Sensor untuk mengukur suhu?", "id": "Termistor, LM35." },
  { "en": "Sensor untuk mengukur cahaya?", "id": "LDR (Light Dependent Resistor)." },
  { "en": "Sensor untuk mengukur jarak?", "id": "Sensor ultrasonik, inframerah." },
  { "en": "Beda filter aktif dan pasif?", "id": "Aktif pakai komponen aktif (Op-Amp)." },
  { "en": "Keuntungan filter aktif?", "id": "Bisa ada penguatan sinyal (gain)." },
  { "en": "Tipe respons filter umum?", "id": "Butterworth, Chebyshev, Bessel." },
  { "en": "Karakteristik filter Butterworth?", "id": "Respons paling datar di passband." },
  { "en": "Karakteristik filter Chebyshev?", "id": "Riap (ripple) di passband, roll-off tajam." },
  { "en": "Konfigurasi penguat transistor dasar?", "id": "Common Emitter, Collector, Base." },
  { "en": "Penguat 'Common Emitter' punya?", "id": "Penguatan tegangan dan arus tinggi." },
  { "en": "Penguat 'Common Collector' disebut juga?", "id": "Emitter follower." },
  { "en": "Fungsi utama 'emitter follower'?", "id": "Sebagai 'buffer' atau penyangga impedansi." },
  { "en": "Jenis osilator Jembatan Wien?", "id": "Menghasilkan gelombang sinus murni." },
  { "en": "Jenis osilator Colpitts?", "id": "Menggunakan dua kapasitor dan satu induktor." },
  { "en": "Apa itu 'strain gauge'?", "id": "Sensor pengukur regangan mekanis." },
  { "en": "Apa itu 'load cell'?", "id": "Sensor untuk mengukur berat atau gaya." },
  { "en": "Prinsip kerja 'load cell'?", "id": "Menggunakan beberapa 'strain gauge'." },
  { "en": "Apa itu 'accelerometer'?", "id": "Sensor untuk mengukur percepatan." },
  { "en": "Kepanjangan DAQ (Data Acquisition)?", "id": "Data Acquisition." },
  { "en": "Fungsi sistem DAQ (Data Acquisition)?", "id": "Mengumpulkan dan merekam data dari sensor." },
  { "en": "Apa itu 'signal conditioning'?", "id": "Memproses sinyal agar sesuai untuk diukur." },
  { "en": "Contoh 'signal conditioning'?", "id": "Amplifikasi, filtering, linearisasi." },
  { "en": "Apa itu kalibrasi instrumen?", "id": "Membandingkan alat ukur dengan standar." },
  { "en": "Tujuan utama kalibrasi?", "id": "Memastikan akurasi hasil pengukuran." },
  { "en": "Kepanjangan SMPS (Switch-Mode Power Supply)?", "id": "Switch-Mode Power Supply." },
  { "en": "Keuntungan SMPS (Switch-Mode Power Supply)?", "id": "Efisiensi tinggi, ukuran lebih kecil." },
  { "en": "Kelemahan SMPS (Switch-Mode Power Supply)?", "id": "Menghasilkan noise frekuensi tinggi." },
  { "en": "Apa itu 'buck converter'?", "id": "Konverter DC-DC penurun tegangan." },
  { "en": "Apa itu 'boost converter'?", "id": "Konverter DC-DC penaik tegangan." },
  { "en": "Kepanjangan BMS (Battery Management System)?", "id": "Battery Management System." },
  { "en": "Fungsi BMS (Battery Management System)?", "id": "Melindungi baterai dari overcharge/discharge." },
  { "en": "Jenis baterai isi ulang umum?", "id": "Lithium-ion (Li-ion), Ni-MH." },
  { "en": "Prinsip kerja metal detector?", "id": "Induksi elektromagnetik." },
  { "en": "Komponen utama metal detector?", "id": "Koil pemancar dan koil penerima." },
  { "en": "Prinsip kerja 'speed gun'?", "id": "Efek Doppler pada gelombang radio/laser." },
  { "en": "Apa itu efek Doppler?", "id": "Perubahan frekuensi akibat gerakan relatif." },
  { "en": "Prinsip kerja 'night vision'?", "id": "Menguatkan cahaya minim (intensifier tube)." },
  { "en": "Warna dominan 'night vision'?", "id": "Hijau, karena mata manusia sensitif." },
  { "en": "Prinsip kerja 'breathalyzer'?", "id": "Reaksi kimia atau sensor semikonduktor." },
  { "en": "Apa itu 'thermal camera'?", "id": "Kamera yang mendeteksi radiasi panas." },
  { "en": "Beda 'thermal' & 'night vision'?", "id": "Thermal deteksi panas, night vision cahaya." },
  { "en": "Apa itu 'function generator'?", "id": "Alat pembangkit berbagai bentuk gelombang." },
  { "en": "Apa itu 'logic analyzer'?", "id": "Alat untuk menganalisa banyak sinyal digital." },
  { "en": "Beda osiloskop & 'logic analyzer'?", "id": "Osiloskop analog, logic analyzer digital." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Alat ukur sinyal di domain frekuensi." },
  { "en": "Kepanjangan LCR (Inductance, Capacitance, and Resistance) Meter?", "id": "Inductance, Capacitance, and Resistance Meter." },
  { "en": "Fungsi LCR (Inductance, Capacitance, and Resistance) Meter?", "id": "Mengukur nilai L, C, dan R." },
  { "en": "Apa itu 'amplifier class'?", "id": "Klasifikasi penguat berdasarkan karakteristiknya." },
  { "en": "Contoh 'amplifier class'?", "id": "Class A, B, AB, C, D." },
  { "en": "Class penguat paling efisien?", "id": "Class D." },
  { "en": "Penguat 'push-pull' menggunakan?", "id": "Dua transistor (NPN dan PNP)." },
  { "en": "Apa itu 'crossover distortion'?", "id": "Distorsi pada penguat Class B." },
  { "en": "Apa itu 'negative feedback'?", "id": "Umpan balik untuk stabilkan penguatan." },
  { "en": "Manfaat 'negative feedback'?", "id": "Mengurangi distorsi, meningkatkan stabilitas." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Rangkaian pembanding dengan histeresis." },
  { "en": "Fungsi 'Schmitt trigger'?", "id": "Mengubah sinyal berisik jadi bersih." },
  { "en": "Kepanjangan DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
  { "en": "Apa itu resolusi DAC (Digital-to-Analog Converter)?", "id": "Jumlah bit input yang diolah." },
  { "en": "Apa itu 'sampling rate' ADC (Analog-to-Digital Converter)?", "id": "Seberapa sering sinyal analog diukur." },
  { "en": "Satuan 'sampling rate'?", "id": "Hertz (Hz) atau samples per second." },
  { "en": "Teorema sampling dasar?", "id": "Teorema Nyquist-Shannon." },
  { "en": "Bunyi teorema Nyquist?", "id": "Sampling rate > 2x frekuensi maks." },
  { "en": "Efek jika sampling rate kurang?", "id": "Terjadi 'aliasing'." },
  { "en": "Apa itu 'aliasing'?", "id": "Sinyal frekuensi tinggi tampak rendah." },
  { "en": "Apa itu 'anti-aliasing filter'?", "id": "Low-pass filter sebelum proses sampling." },
  { "en": "Sensor untuk mengukur kelembaban?", "id": "Sensor higrometer, DHT11, DHT22." },
  { "en": "Sensor untuk mengukur tekanan udara?", "id": "Sensor barometer, BMP180." },
  { "en": "Sensor untuk deteksi gas?", "id": "Sensor gas MQ series." },
  { "en": "Sensor untuk mengukur denyut jantung?", "id": "Sensor PPG (Photoplethysmography)." },
  { "en": "Apa itu 'piezoelectric sensor'?", "id": "Sensor yang hasilkan listrik saat ditekan." },
  { "en": "Contoh aplikasi 'piezoelectric'?", "id": "Sensor getar, mikrofon, pemantik api." },
  { "en": "Apa itu 'Hall effect sensor'?", "id": "Sensor pendeteksi medan magnet." },
  { "en": "Aplikasi 'Hall effect sensor'?", "id": "Sensor kecepatan, sensor posisi." },
  { "en": "Apa itu 'optocoupler'?", "id": "Isolator rangkaian menggunakan cahaya." },
  { "en": "Fungsi utama 'optocoupler'?", "id": "Mengisolasi sirkuit tegangan tinggi-rendah." },
  { "en": "Gerbang logika universal?", "id": "NAND dan NOR." },
  { "en": "Kenapa disebut gerbang universal?", "id": "Bisa membentuk semua gerbang lain." },
  { "en": "Apa itu 'encoder'?", "id": "Mengubah banyak input menjadi kode biner." },
  { "en": "Apa itu 'decoder'?", "id": "Mengubah kode biner menjadi banyak output." },
  { "en": "Apa itu 'multiplexer' (MUX)?", "id": "Memilih satu dari banyak input." },
  { "en": "Apa itu 'demultiplexer' (DEMUX)?", "id": "Meneruskan satu input ke banyak output." },
  { "en": "Rangkaian penjumlah biner?", "id": "Adder (Half Adder, Full Adder)." },
  { "en": "Apa itu 'shift register'?", "id": "Register yang bisa menggeser data." },
  { "en": "Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Universal Asynchronous Receiver-Transmitter." },
  { "en": "Fungsi UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi serial asinkron." },
  { "en": "Kepanjangan SPI (Serial Peripheral Interface)?", "id": "Serial Peripheral Interface." },
  { "en": "Karakteristik SPI (Serial Peripheral Interface)?", "id": "Komunikasi serial sinkron, lebih cepat." },
  { "en": "Kepanjangan I2C (Inter-Integrated Circuit)?", "id": "Inter-Integrated Circuit." },
  { "en": "Karakteristik I2C (Inter-Integrated Circuit)?", "id": "Hanya butuh dua kabel komunikasi." },
  { "en": "Apa itu 'datasheet'?", "id": "Dokumen spesifikasi teknis komponen." },
  { "en": "Pentingnya membaca 'datasheet'?", "id": "Mengetahui cara kerja dan batasan komponen." },
  { "en": "Apa itu 'breadthalyzer' inframerah?", "id": "Mengukur alkohol dari serapan inframerah." },
  { "en": "Apa itu 'signal conditioning'?", "id": "Pengkondisian sinyal dari sensor." },
  { "en": "Contoh 'signal conditioning'?", "id": "Amplifikasi, filtering, isolasi." },
  { "en": "Apa itu 'cold junction compensation'?", "id": "Kompensasi pada sensor termokopel." },
  { "en": "Apa itu 'ground loop'?", "id": "Masalah noise akibat banyak jalur ground." },
  { "en": "Cara atasi 'ground loop'?", "id": "Gunakan isolasi atau ground terpusat." },
  { "en": "Apa itu 'Faraday cage'?", "id": "Sangkar pelindung dari medan elektromagnetik." },
  { "en": "Beda utama mikroprosesor & mikrokontroler?", "id": "Mikroprosesor butuh komponen eksternal." },
  { "en": "Mikrokontroler sering disebut?", "id": "Komputer dalam satu chip." },
  { "en": "Kepanjangan RAM (Random Access Memory)?", "id": "Random Access Memory." },
  { "en": "Sifat memori RAM (Random Access Memory)?", "id": "Volatile, data hilang saat mati." },
  { "en": "Beda SRAM (Static RAM) dan DRAM (Dynamic RAM)?", "id": "SRAM lebih cepat, DRAM lebih padat." },
  { "en": "Kepanjangan SRAM (Static RAM)?", "id": "Static Random Access Memory." },
  { "en": "Kepanjangan DRAM (Dynamic RAM)?", "id": "Dynamic Random Access Memory." },
  { "en": "Kepanjangan ROM (Read-Only Memory)?", "id": "Read-Only Memory." },
  { "en": "Sifat memori ROM (Read-Only Memory)?", "id": "Non-volatile, data tetap ada." },
  { "en": "Kepanjangan EPROM (Erasable PROM)?", "id": "Erasable Programmable Read-Only Memory." },
  { "en": "Cara hapus data EPROM (Erasable PROM)?", "id": "Menggunakan sinar ultraviolet." },
  { "en": "Kepanjangan EEPROM (Electrically Erasable PROM)?", "id": "Electrically Erasable Programmable Read-Only Memory." },
  { "en": "Apa itu 'flash memory'?", "id": "Jenis EEPROM yang lebih cepat." },
  { "en": "Penggunaan 'flash memory'?", "id": "USB drive, SSD, kartu memori." },
  { "en": "Apa itu sistem tertanam (embedded system)?", "id": "Sistem komputer khusus dalam perangkat." },
  { "en": "Contoh sistem tertanam?", "id": "Sistem di mobil, HT, GPS." },
  { "en": "Karakteristik sistem tertanam?", "id": "Spesifik, handal, seringkali real-time." },
  { "en": "Kepanjangan RTOS (Real-Time Operating System)?", "id": "Real-Time Operating System." },
  { "en": "Fungsi RTOS (Real-Time Operating System)?", "id": "Menjamin tugas selesai dalam waktu pasti." },
  { "en": "Apa itu sensor cerdas (smart sensor)?", "id": "Sensor dengan pemrosesan data terintegrasi." },
  { "en": "Kepanjangan HART (Highway Addressable Remote Transducer) Protocol?", "id": "Highway Addressable Remote Transducer Protocol." },
  { "en": "Fungsi protokol HART?", "id": "Komunikasi digital di atas sinyal analog." },
  { "en": "Apa itu 'fieldbus'?", "id": "Jaringan digital untuk instrumentasi industri." },
  { "en": "Jenis-jenis 'noise' elektronik?", "id": "Thermal, shot, flicker, burst." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise akibat agitasi termal elektron." },
  { "en": "Nama lain 'thermal noise'?", "id": "Johnson-Nyquist noise." },
  { "en": "Apa itu 'shielding'?", "id": "Melindungi sirkuit dari interferensi." },
  { "en": "Bahan untuk 'shielding'?", "id": "Logam konduktif seperti tembaga." },
  { "en": "Apa itu 'grounding'?", "id": "Menghubungkan sirkuit ke titik ground." },
  { "en": "Tujuan utama 'grounding'?", "id": "Keamanan dan stabilitas sinyal." },
  { "en": "Apa itu 'hardware trojan'?", "id": "Sirkuit berbahaya disisipkan ke IC." },
  { "en": "Apa itu 'side-channel attack'?", "id": "Serangan berbasis informasi sampingan." },
  { "en": "Contoh 'side-channel attack'?", "id": "Analisa konsumsi daya, waktu, emisi." },
  { "en": "Apa itu 'fault injection'?", "id": "Memaksa kesalahan untuk dapatkan informasi." },
  { "en": "Apa itu 'chip-off' forensics?", "id": "Melepas chip memori untuk dibaca langsung." },
  { "en": "Apa itu 'doping' semikonduktor?", "id": "Menambah ketidakmurnian untuk ubah sifat." },
  { "en": "Doping tipe-n menghasilkan?", "id": "Semikonduktor dengan kelebihan elektron." },
  { "en": "Doping tipe-p menghasilkan?", "id": "Semikonduktor dengan kelebihan 'hole'." },
  { "en": "Daerah deplesi di P-N junction?", "id": "Area tanpa pembawa muatan bebas." },
  { "en": "Apa itu 'forward bias'?", "id": "Memberi tegangan positif ke tipe-p." },
  { "en": "Apa itu 'reverse bias'?", "id": "Memberi tegangan negatif ke tipe-p." },
  { "en": "Kepanjangan PFC (Power Factor Correction)?", "id": "Power Factor Correction." },
  { "en": "Tujuan PFC (Power Factor Correction)?", "id": "Meningkatkan efisiensi penggunaan daya listrik." },
  { "en": "Apa itu 'standby power'?", "id": "Daya yang dikonsumsi saat mati." },
  { "en": "Nama lain 'standby power'?", "id": "Vampire power atau phantom load." },
  { "en": "Apa itu 'energy harvesting'?", "id": "Memanen energi dari lingkungan sekitar." },
  { "en": "Contoh 'energy harvesting'?", "id": "Panel surya, piezoelektrik, termoelektrik." },
  { "en": "Kepanjangan MEMS (Micro-Electro-Mechanical Systems)?", "id": "Micro-Electro-Mechanical Systems." },
  { "en": "Contoh sensor MEMS?", "id": "Accelerometer dan giroskop di HP." },
  { "en": "Apa itu giroskop?", "id": "Sensor pengukur orientasi dan kecepatan angular." },
  { "en": "Apa itu 'Inertial Measurement Unit' (IMU)?", "id": "Gabungan accelerometer dan giroskop." },
  { "en": "Kepanjangan IMU?", "id": "Inertial Measurement Unit." },
  { "en": "Aplikasi IMU (Inertial Measurement Unit)?", "id": "Navigasi drone, stabilisasi gambar." },
  { "en": "Apa itu magnetometer?", "id": "Sensor pengukur medan magnet." },
  { "en": "Fungsi magnetometer di HP?", "id": "Sebagai kompas digital." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Tujuan 'sensor fusion'?", "id": "Mendapatkan hasil yang lebih akurat." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi dan prediksi." },
  { "en": "Fungsi 'Kalman filter'?", "id": "Mengurangi noise dan estimasi keadaan." },
  { "en": "Apa itu 'interfacing'?", "id": "Proses menghubungkan dua sirkuit berbeda." },
  { "en": "Apa itu 'level shifter'?", "id": "Menyesuaikan level tegangan logika." },
  { "en": "Apa itu 'bus' dalam elektronika?", "id": "Jalur komunikasi bersama." },
  { "en": "Jenis 'bus'?", "id": "Address bus, data bus, control bus." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Timer untuk reset sistem hang." },
  { "en": "Apa itu 'brown-out detector'?", "id": "Mendeteksi jika tegangan suplai turun." },
  { "en": "Sistem kontrol loop terbuka?", "id": "Output tidak mempengaruhi input." },
  { "en": "Sistem kontrol loop tertutup?", "id": "Output digunakan sebagai umpan balik." },
  { "en": "Nama lain loop tertutup?", "id": "Feedback control system." },
  { "en": "Kepanjangan PID (Proportional-Integral-Derivative) controller?", "id": "Proportional-Integral-Derivative controller." },
  { "en": "Fungsi PID (Proportional-Integral-Derivative) controller?", "id": "Sistem kontrol umpan balik akurat." },
  { "en": "Apa itu 'transfer function'?", "id": "Representasi matematis dari sistem." },
  { "en": "Apa itu 'Bode plot'?", "id": "Grafik respons frekuensi suatu sistem." },
  { "en": "Apa itu 'settling time'?", "id": "Waktu sistem mencapai kondisi stabil." },
  { "en": "Apa itu 'overshoot'?", "id": "Output melampaui nilai targetnya." },
  { "en": "Apa itu 'rise time'?", "id": "Waktu output naik dari 10% ke 90%." },
  { "en": "Apa itu 'Lissajous figure'?", "id": "Pola osiloskop dari dua sinyal sinus." },
  { "en": "Fungsi 'Lissajous figure'?", "id": "Mengukur beda fasa dan frekuensi." },
  { "en": "Apa itu 'time-domain reflectometer' (TDR)?", "id": "Alat untuk mencari kerusakan kabel." },
  { "en": "Kepanjangan TDR?", "id": "Time-Domain Reflectometer." },
  { "en": "Prinsip kerja TDR (Time-Domain Reflectometer)?", "id": "Mengirim pulsa dan menganalisa pantulannya." },
  { "en": "Apa itu 'bit-error rate tester' (BERT)?", "id": "Alat penguji tingkat kesalahan transmisi." },
  { "en": "Kepanjangan BERT?", "id": "Bit-Error Rate Tester." },
  { "en": "Apa itu 'signal integrity'?", "id": "Ukuran kualitas sinyal listrik." },
  { "en": "Faktor perusak 'signal integrity'?", "id": "Noise, distorsi, timing error." },
  { "en": "Apa itu 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan perangkat berfungsi tanpa interferensi." },
  { "en": "Kepanjangan EMC?", "id": "Electromagnetic Compatibility." },
  { "en": "Tes dalam EMC (Electromagnetic Compatibility)?", "id": "Tes emisi dan tes imunitas." },
  { "en": "Kepanjangan EMI (Electromagnetic Interference)?", "id": "Electromagnetic Interference." },
  { "en": "Kepanjangan EMS (Electromagnetic Susceptibility)?", "id": "Electromagnetic Susceptibility." },
  { "en": "Apa itu 'anechoic chamber'?", "id": "Ruangan bebas gema elektromagnetik." },
  { "en": "Fungsi 'anechoic chamber'?", "id": "Untuk pengujian antena dan EMC." },
  { "en": "Apa itu 'optoelectronics'?", "id": "Elektronika berbasis interaksi cahaya." },
  { "en": "Kepanjangan LASER?", "id": "Light Amplification by Stimulated Emission of Radiation." },
  { "en": "Apa itu 'photodiode'?", "id": "Dioda yang mendeteksi cahaya." },
  { "en": "Mode operasi 'photodiode'?", "id": "Photovoltaic dan photoconductive." },
  { "en": "Apa itu 'fiber optic sensor'?", "id": "Sensor yang menggunakan serat optik." },
  { "en": "Keuntungan 'fiber optic sensor'?", "id": "Tahan interferensi elektromagnetik (EMI)." },
  { "en": "Prinsip kerja LiDAR (Light Detection and Ranging)?", "id": "Mengukur jarak dengan pulsa laser." },
  { "en": "Apa itu thyristor?", "id": "Komponen semikonduktor sebagai saklar." },
  { "en": "Contoh thyristor?", "id": "SCR (Silicon-Controlled Rectifier), TRIAC." },
  { "en": "Kepanjangan SCR (Silicon-Controlled Rectifier)?", "id": "Silicon-Controlled Rectifier." },
  { "en": "Fungsi utama SCR (Silicon-Controlled Rectifier)?", "id": "Saklar daya DC terkendali." },
  { "en": "Kepanjangan TRIAC (Triode for Alternating Current)?", "id": "Triode for Alternating Current." },
  { "en": "Fungsi utama TRIAC (Triode for Alternating Current)?", "id": "Saklar daya AC terkendali." },
  { "en": "Kepanjangan IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Insulated-Gate Bipolar Transistor." },
  { "en": "Keuntungan IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Kombinasi kecepatan MOSFET dan daya BJT." },
  { "en": "Apa itu 'inverter'?", "id": "Rangkaian pengubah DC ke AC." },
  { "en": "Apa itu 'rectifier'?", "id": "Rangkaian pengubah AC ke DC." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Rangkaian untuk meredam lonjakan tegangan." },
  { "en": "Standar antarmuka serial lama?", "id": "RS-232." },
  { "en": "Kelemahan RS-232?", "id": "Jarak pendek, rentan noise." },
  { "en": "Standar antarmuka serial industri?", "id": "RS-485." },
  { "en": "Kelebihan RS-485?", "id": "Jarak jauh, bisa multi-drop." },
  { "en": "Kepanjangan CAN (Controller Area Network) bus?", "id": "Controller Area Network bus." },
  { "en": "Penggunaan CAN (Controller Area Network) bus?", "id": "Jaringan di dalam otomotif." },
  { "en": "Apa itu Modbus?", "id": "Protokol komunikasi untuk instrumentasi." },
  { "en": "Kepanjangan DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
  { "en": "Beda DSP (Digital Signal Processor) & CPU (Central Processing Unit)?", "id": "DSP dioptimalkan untuk operasi sinyal." },
  { "en": "Apa itu filter FIR (Finite Impulse Response)?", "id": "Filter digital tanpa umpan balik." },
  { "en": "Kepanjangan FIR (Finite Impulse Response)?", "id": "Finite Impulse Response." },
  { "en": "Apa itu filter IIR (Infinite Impulse Response)?", "id": "Filter digital dengan umpan balik." },
  { "en": "Kepanjangan IIR (Infinite Impulse Response)?", "id": "Infinite Impulse Response." },
  { "en": "Mana lebih stabil, FIR/IIR?", "id": "FIR (Finite Impulse Response) selalu stabil." },
  { "en": "Apa itu 'decimation'?", "id": "Proses mengurangi sampling rate sinyal." },
  { "en": "Apa itu 'interpolation' sinyal?", "id": "Proses meningkatkan sampling rate sinyal." },
  { "en": "Algoritma hitung DFT (Discrete Fourier Transform) cepat?", "id": "FFT (Fast Fourier Transform)." },
  { "en": "Apa itu 'PCB layout'?", "id": "Proses desain tata letak komponen." },
  { "en": "Apa itu 'routing' PCB?", "id": "Proses desain jalur tembaga." },
  { "en": "Apa itu 'via' pada PCB?", "id": "Lubang penghubung antar lapisan." },
  { "en": "Kepanjangan SMT (Surface-Mount Technology)?", "id": "Surface-Mount Technology." },
  { "en": "Beda SMT (Surface-Mount Technology) & 'through-hole'?", "id": "Komponen SMT dipasang di permukaan." },
  { "en": "Apa itu 'trace' pada PCB?", "id": "Jalur konduktif tembaga." },
  { "en": "Apa itu 'solder mask'?", "id": "Lapisan pelindung pada PCB." },
  { "en": "Apa itu 'silkscreen'?", "id": "Lapisan tulisan dan simbol di PCB." },
  { "en": "Apa itu 'traceability' dalam kalibrasi?", "id": "Kemampuan melacak pengukuran ke standar." },
  { "en": "Apa itu 'standard uncertainty'?", "id": "Ketidakpastian dari standar referensi." },
  { "en": "Tipe standar kalibrasi?", "id": "Primer, sekunder, dan kerja." },
  { "en": "Apa itu 'guard banding'?", "id": "Memperketat batas toleransi saat kalibrasi." },
  { "en": "Apa itu 'linearity' instrumen?", "id": "Kemampuan memberi output proporsional." },
  { "en": "Apa itu 'hysteresis'?", "id": "Perbedaan output saat input naik/turun." },
  { "en": "Apa itu 'sensitivity' sensor?", "id": "Perubahan output per perubahan input." },
  { "en": "Apa itu 'resolution' sensor?", "id": "Perubahan terkecil yang bisa dideteksi." },
  { "en": "Apa itu 'response time'?", "id": "Waktu sensor merespon perubahan." },
  { "en": "Sensor untuk getaran?", "id": "Accelerometer atau sensor piezoelektrik." },
  { "en": "Sensor untuk aliran fluida?", "id": "Flow meter." },
  { "en": "Sensor untuk level ketinggian cairan?", "id": "Level sensor." },
  { "en": "Apa itu ' Wheatstone bridge'?", "id": "Rangkaian untuk mengukur resistansi." },
  { "en": "Penggunaan 'Wheatstone bridge'?", "id": "Pada strain gauge dan load cell." },
  { "en": "Apa itu 'thermocouple'?", "id": "Sensor suhu dari dua logam berbeda." },
  { "en": "Prinsip kerja 'thermocouple'?", "id": "Efek Seebeck." },
  { "en": "Apa itu efek Seebeck?", "id": "Tegangan muncul akibat beda suhu." },
  { "en": "Apa itu 'RTD (Resistance Temperature Detector)'?", "id": "Sensor suhu berbasis resistansi logam." },
  { "en": "Bahan untuk RTD (Resistance Temperature Detector)?", "id": "Platinum (Pt100)." },
  { "en": "Apa itu 'thermistor'?", "id": "Resistor yang peka terhadap suhu." },
  { "en": "Tipe 'thermistor'?", "id": "NTC (Negative) dan PTC (Positive)." },
  { "en": "Arti NTC (Negative Temperature Coefficient)?", "id": "Resistansi turun jika suhu naik." },
  { "en": "Apa itu 'instrumentation amplifier' (In-Amp)?", "id": "Penguat khusus untuk sinyal sensor." },
  { "en": "Keunggulan In-Amp?", "id": "CMRR tinggi, gain bisa diatur." },
  { "en": "Kepanjangan CMRR (Common-Mode Rejection Ratio)?", "id": "Common-Mode Rejection Ratio." },
  { "en": "Fungsi CMRR (Common-Mode Rejection Ratio) tinggi?", "id": "Sangat baik dalam menolak noise." },
  { "en": "Apa itu 'DAQ card'?", "id": "Kartu akuisisi data untuk komputer." },
  { "en": "Protokol instrumentasi nirkabel?", "id": "WirelessHART, ISA100.11a." },
  { "en": "Apa itu 'data logger'?", "id": "Perangkat perekam data dari waktu ke waktu." },
  { "en": "Apa itu 'virtual instrumentation'?", "id": "Instrumentasi berbasis software (misal: LabVIEW)." },
  { "en": "Apa itu 'human-machine interface' (HMI)?", "id": "Antarmuka grafis antara manusia-mesin." },
  { "en": "Kepanjangan HMI?", "id": "Human-Machine Interface." },
  { "en": "Kepanjangan PLC (Programmable Logic Controller)?", "id": "Programmable Logic Controller." },
  { "en": "Fungsi PLC (Programmable Logic Controller)?", "id": "Kontroler terprogram untuk otomasi industri." },
  { "en": "Kepanjangan SCADA (Supervisory Control and Data Acquisition)?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Fungsi SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem monitoring dan kontrol skala besar." },
  { "en": "Bahasa pemrograman PLC (Programmable Logic Controller)?", "id": "Ladder diagram." },
  { "en": "Apa itu 'noise shaping'?", "id": "Teknik memindahkan noise ke frekuensi lain." },
  { "en": "Apa itu 'dithering'?", "id": "Menambah noise untuk perbaiki resolusi." },
  { "en": "Fungsi 'dithering'?", "id": "Mengurangi 'quantization error' yang terlihat." },
  { "en": "Apa itu 'windowing function'?", "id": "Fungsi untuk analisa sinyal FFT." },
  { "en": "Contoh 'windowing function'?", "id": "Hamming, Hanning, Blackman." },
  { "en": "Apa itu 'Gibbs phenomenon'?", "id": "Osilasi pada aproksimasi sinyal kotak." },
  { "en": "Apa itu 'z-transform'?", "id": "Transformasi matematis untuk sinyal diskrit." },
  { "en": "Kaitan 'z-transform' & 'Fourier transform'?", "id": "Fourier adalah kasus khusus z-transform." },
  { "en": "Apa itu 'Laplace transform'?", "id": "Transformasi matematis untuk sinyal kontinu." },
  { "en": "Apa itu 'impulse response'?", "id": "Output sistem saat diberi input impuls." },
  { "en": "Apa itu sistem LTI (Linear Time-Invariant)?", "id": "Sistem linear yang tak berubah waktu." },
  { "en": "Fungsi 'autocorrelation'?", "id": "Mengukur kemiripan sinyal dengan dirinya." },
  { "en": "Fungsi 'cross-correlation'?", "id": "Mengukur kemiripan dua sinyal berbeda." },
  { "en": "Aplikasi 'cross-correlation'?", "id": "Mendeteksi sinyal, mengukur waktu tunda." },
  { "en": "Apa itu 'downconversion' pada mixer?", "id": "Mengubah frekuensi tinggi ke rendah." },
  { "en": "Apa itu 'upconversion' pada mixer?", "id": "Mengubah frekuensi rendah ke tinggi." },
  { "en": "Produk sampingan dari mixer?", "id": "Frekuensi gambar (image frequency)." },
  { "en": "Apa itu 'image rejection ratio' (IRR)?", "id": "Kemampuan menolak frekuensi gambar." },
  { "en": "Apa itu 'local oscillator' (LO)?", "id": "Osilator lokal untuk proses mixing." },
  { "en": "Kepanjangan BAW (Bulk Acoustic Wave) filter?", "id": "Bulk Acoustic Wave filter." },
  { "en": "Keuntungan filter BAW/SAW?", "id": "Ukuran kecil, performa frekuensi tinggi." },
  { "en": "Apa itu 'insertion loss'?", "id": "Pelemahan sinyal akibat suatu komponen." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran sinyal yang dipantulkan kembali." },
  { "en": "Apa itu S-parameters?", "id": "Parameter untuk karakterisasi jaringan RF." },
  { "en": "Parameter S11 merepresentasikan apa?", "id": "Koefisien pantulan port 1 (input)." },
  { "en": "Parameter S21 merepresentasikan apa?", "id": "Gain atau loss dari port 1 ke 2." },
  { "en": "Apa itu 'shielding effectiveness'?", "id": "Ukuran kemampuan perisai meredam medan." },
  { "en": "Apa itu 'star grounding'?", "id": "Teknik grounding terpusat satu titik." },
  { "en": "Apa itu 'ferrite bead'?", "id": "Komponen pasif untuk menekan noise HF." },
  { "en": "Bahan semikonduktor paling umum?", "id": "Silikon (Si)." },
  { "en": "Keunggulan Gallium Nitride (GaN)?", "id": "Efisien di daya dan frekuensi tinggi." },
  { "en": "Apa itu 'wafer' silikon?", "id": "Lempengan tipis bahan semikonduktor." },
  { "en": "Apa itu 'die' dalam IC?", "id": "Satu unit sirkuit di wafer." },
  { "en": "Bahan substrat PCB (Printed Circuit Board) umum?", "id": "FR-4." },
  { "en": "Apa itu 'traceability' kalibrasi?", "id": "Kemampuan melacak ukur ke standar nasional." },
  { "en": "Apa itu 'calibration uncertainty'?", "id": "Rentang keraguan pada hasil kalibrasi." },
  { "en": "Apa itu 'inter-laboratory comparison'?", "id": "Perbandingan hasil ukur antar laboratorium." },
  { "en": "Kepanjangan HSM (Hardware Security Module)?", "id": "Hardware Security Module." },
  { "en": "Fungsi HSM (Hardware Security Module)?", "id": "Modul aman untuk operasi kriptografi." },
  { "en": "Apa itu 'tamper resistance'?", "id": "Kemampuan perangkat melawan modifikasi fisik." },
  { "en": "Contoh 'tamper resistance'?", "id": "Sirkuit akan rusak jika dibuka paksa." },
  { "en": "Apa itu 'power analysis attack'?", "id": "Serangan berbasis analisa konsumsi daya." },
  { "en": "Kepanjangan FPGA (Field-Programmable Gate Array)?", "id": "Field-Programmable Gate Array." },
  { "en": "Apa itu FPGA (Field-Programmable Gate Array)?", "id": "IC yang logikanya bisa diprogram." },
  { "en": "Kepanjangan ASIC (Application-Specific Integrated Circuit)?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Beda FPGA (Field-Programmable Gate Array) dan ASIC (Application-Specific Integrated Circuit)?", "id": "FPGA fleksibel, ASIC performa tinggi." },
  { "en": "Apa itu 'JTAG (Joint Test Action Group)'?", "id": "Standar untuk debugging dan tes hardware." },
  { "en": "Apa itu 'boundary scan'?", "id": "Teknik tes koneksi pin IC." },
  { "en": "Apa itu 'glitching'?", "id": "Mengganggu sinyal clock atau daya." },
  { "en": "Tujuan serangan 'glitching'?", "id": "Memaksa sistem melakukan kesalahan." },
  { "en": "Apa itu 'gain-bandwidth product'?", "id": "Hasil kali gain dan bandwidth Op-Amp." },
  { "en": "Apa itu 'slew rate'?", "id": "Kecepatan perubahan output Op-Amp." },
  { "en": "Apa itu 'common-mode voltage'?", "id": "Tegangan rata-rata pada input diferensial." },
  { "en": "Apa itu 'input offset voltage'?", "id": "Tegangan input untuk output nol." },
  { "en": "Apa itu 'input bias current'?", "id": "Arus kecil yang masuk ke input Op-Amp." },
  { "en": "Apa itu 'instrumentation amplifier' (In-Amp)?", "id": "Penguat khusus untuk sinyal sensor." },
  { "en": "Apa itu 'isolation amplifier'?", "id": "Penguat dengan isolasi galvanik." },
  { "en": "Tujuan 'isolation amplifier'?", "id": "Keamanan dan memutus ground loop." },
  { "en": "Apa itu 'active rectifier'?", "id": "Penyearah presisi menggunakan Op-Amp." },
  { "en": "Apa itu 'analog multiplier'?", "id": "Sirkuit yang mengalikan dua sinyal analog." },
  { "en": "Apa itu 'sample and hold' circuit?", "id": "Sirkuit untuk mengambil dan menahan sampel." },
  { "en": "Apa itu 'analog switch'?", "id": "Saklar elektronik untuk sinyal analog." },
  { "en": "Apa itu 'delta-sigma' ADC (Analog-to-Digital Converter)?", "id": "ADC resolusi tinggi dengan oversampling." },
  { "en": "Apa itu 'flash' ADC (Analog-to-Digital Converter)?", "id": "ADC paralel yang sangat cepat." },
  { "en": "Apa itu 'successive approximation' ADC?", "id": "ADC dengan keseimbangan kecepatan-resolusi." },
  { "en": "Apa itu 'R-2R ladder' DAC (Digital-to-Analog Converter)?", "id": "DAC yang hanya gunakan dua nilai resistor." },
  { "en": "Apa itu 'glitch energy' pada DAC?", "id": "Lonjakan energi saat DAC berganti nilai." },
  { "en": "Apa itu 'monotonicity' DAC?", "id": "Output selalu naik seiring input naik." },
  { "en": "Apa itu 'differential nonlinearity' (DNL)?", "id": "Ukuran kesalahan antar step ADC/DAC." },
  { "en": "Apa itu 'integral nonlinearity' (INL)?", "id": "Ukuran deviasi dari garis lurus ideal." },
  { "en": "Apa itu 'spurious-free dynamic range' (SFDR)?", "id": "Rentang dinamis bebas dari sinyal palsu." },
  { "en": "Apa itu 'total harmonic distortion' (THD)?", "id": "Ukuran distorsi harmonik pada sinyal." },
  { "en": "Apa itu 'Q factor'?", "id": "Ukuran kualitas rangkaian resonator." },
  { "en": "Apa itu 'skin effect'?", "id": "Arus cenderung mengalir di permukaan konduktor." },
  { "en": "Frekuensi berapa 'skin effect' signifikan?", "id": "Pada frekuensi radio (RF) tinggi." },
  { "en": "Apa itu 'dielectric loss'?", "id": "Kehilangan energi pada bahan dielektrik." },
  { "en": "Apa itu 'transmission line'?", "id": "Struktur pemandu gelombang elektromagnetik." },
  { "en": "Contoh 'transmission line'?", "id": "Kabel koaksial, microstrip di PCB." },
  { "en": "Apa itu 'characteristic impedance'?", "id": "Impedansi khas sebuah transmission line." },
  { "en": "Apa itu 'smith chart'?", "id": "Grafik untuk analisa 'transmission line'." },
  { "en": "Fungsi 'smith chart'?", "id": "Membantu proses 'impedance matching'." },
  { "en": "Apa itu 'balun'?", "id": "Komponen penghubung balanced-unbalanced." },
  { "en": "Contoh aplikasi 'balun'?", "id": "Menghubungkan antena dipole ke kabel koaksial." },
  { "en": "Apa itu 'directional coupler'?", "id": "Komponen untuk mengambil sampel sinyal." },
  { "en": "Apa itu 'power divider/combiner'?", "id": "Membagi atau menggabungkan daya sinyal." },
  { "en": "Apa itu 'phase shifter'?", "id": "Sirkuit untuk menggeser fasa sinyal." },
  { "en": "Aplikasi 'phase shifter'?", "id": "Pada sistem antena 'phased array'." },
  { "en": "Apa itu 'Verilog' atau 'VHDL'?", "id": "Bahasa deskripsi perangkat keras (HDL)." },
  { "en": "Fungsi HDL (Hardware Description Language)?", "id": "Mendesain sirkuit digital untuk FPGA/ASIC." },
  { "en": "Apa itu 'synthesis'?", "id": "Proses mengubah kode HDL jadi sirkuit." },
  { "en": "Apa itu 'place and route'?", "id": "Proses penempatan dan penyambungan sirkuit." },
  { "en": "Apa itu 'system-on-a-chip' (SoC)?", "id": "Seluruh sistem dalam satu chip IC." },
  { "en": "Contoh SoC (System-on-a-Chip)?", "id": "Prosesor pada smartphone modern." },
  { "en": "Apa itu 'application-specific instruction-set processor' (ASIP)?", "id": "Prosesor dengan instruksi khusus." },
  { "en": "Apa itu 'gate array'?", "id": "IC semi-kustom dengan gerbang pre-fabrikasi." },
  { "en": "Apa itu 'standard cell'?", "id": "Desain ASIC berbasis pustaka sel standar." },
  { "en": "Apa itu 'full custom' design?", "id": "Desain IC manual untuk performa maksimal." },
  { "en": "Apa itu 'photolithography'?", "id": "Proses pencetakan sirkuit ke wafer." },
  { "en": "Apa itu 'etching'?", "id": "Proses menghilangkan material dari wafer." },
  { "en": "Apa itu 'cleanroom'?", "id": "Ruangan sangat bersih untuk fabrikasi IC." },
  { "en": "Apa itu 'Moore's Law'?", "id": "Jumlah transistor berlipat ganda." },
  { "en": "Periode 'Moore's Law'?", "id": "Kira-kira setiap dua tahun." },
  { "en": "Apa itu 'yield' dalam fabrikasi?", "id": "Persentase chip yang berfungsi baik." },
  { "en": "Apa itu 'binning'?", "id": "Mengelompokkan chip berdasarkan performa." },
  { "en": "Apa itu 'thermal design power' (TDP)?", "id": "Jumlah panas maksimal yang dihasilkan." },
  { "en": "Kepanjangan TDP?", "id": "Thermal Design Power." }



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
