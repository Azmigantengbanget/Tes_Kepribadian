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



  { "en": "Saat menghadapi tugas sulit, saya .........", "id": "Mengerjakannya sedikit demi sedikit." },
  { "en": "Saya lebih suka bekerja .........", "id": "Sendirian agar lebih fokus." },
  { "en": "Ketika orang lain meminta bantuan, saya .........", "id": "Membantu jika saya mampu." },
  { "en": "Saya sering merasa khawatir tentang hal kecil. .........", "id": "Tidak setuju, saya cukup santai." },
  { "en": "Rutinitas sehari-hari membuat saya merasa .........", "id": "Aman dan teratur." },
  { "en": "Di sebuah pesta, saya cenderung .........", "id": "Berbicara dengan beberapa orang saja." },
  { "en": "Jika rencana saya gagal, saya .........", "id": "Segera mencari rencana alternatif." },
  { "en": "Mengkritik orang lain adalah hal yang .........", "id": "Saya hindari jika tidak perlu." },
  { "en": "Saya membuat keputusan secara .........", "id": "Cepat berdasarkan intuisi." },
  { "en": "Perubahan mendadak membuat saya .........", "id": "Merasa sedikit tidak nyaman." },
  { "en": "Saya menggambarkan diri saya sebagai orang yang .........", "id": "Penuh energi dan antusias." },
  { "en": "Menepati janji bagi saya adalah .........", "id": "Sebuah keharusan yang mutlak." },
  { "en": "Saya mudah merasa simpati pada orang lain. .........", "id": "Setuju, saya orang yang empatik." },
  { "en": "Saya sering memikirkan kembali kesalahan masa lalu. .........", "id": "Tidak setuju, saya fokus ke depan." },
  { "en": "Mencoba makanan baru adalah sesuatu yang .........", "id": "Sangat saya sukai." },
  { "en": "Berbicara di depan umum membuat saya .........", "id": "Merasa sangat gugup." },
  { "en": "Tujuan jangka panjang penting bagi saya. .........", "id": "Setuju, saya selalu punya tujuan." },
  { "en": "Saya lebih suka mengikuti daripada memimpin. .........", "id": "Setuju, saya pemain tim yang baik." },
  { "en": "Suasana hati saya mudah berubah. .........", "id": "Tidak setuju, emosi saya stabil." },
  { "en": "Saya menikmati diskusi filosofis yang mendalam. .........", "id": "Setuju, saya suka berpikir." },
  { "en": "Menjadi pusat perhatian membuat saya .........", "id": "Merasa tidak nyaman." },
  { "en": "Saya selalu menyelesaikan apa yang saya mulai. .........", "id": "Setuju, saya orang yang persisten." },
  { "en": "Saya percaya bahwa orang pada dasarnya baik. .........", "id": "Setuju, saya mudah percaya." },
  { "en": "Saya jarang merasa puas dengan pencapaian saya. .........", "id": "Tidak setuju, saya menghargai usaha." },
  { "en": "Seni abstrak membuat saya .........", "id": "Merasa tertarik dan penasaran." },
  { "en": "Saya lebih suka mendengarkan daripada berbicara. .........", "id": "Setuju, saya pendengar yang baik." },
  { "en": "Saya orang yang sangat terorganisir. .........", "id": "Setuju, semua hal saya rencanakan." },
  { "en": "Saya sulit berkata 'tidak' pada orang lain. .........", "id": "Setuju, saya tidak mau mengecewakan." },
  { "en": "Saya sering merasa stres karena pekerjaan. .........", "id": "Setuju, tuntutan pekerjaan tinggi." },
  { "en": "Saya tidak tertarik pada teori-teori rumit. .........", "id": "Tidak setuju, saya menyukai ide kompleks." },
  { "en": "Menghabiskan akhir pekan sendirian adalah .........", "id": "Sesuatu yang saya nikmati." },
  { "en": "Saya memperhatikan detail-detail kecil. .........", "id": "Setuju, saya orang yang teliti." },
  { "en": "Saya suka berdebat untuk mempertahankan pendapat. .........", "id": "Tidak setuju, saya menghindari konflik." },
  { "en": "Saya orang yang santai dan tidak mudah panik. .........", "id": "Setuju, saya bisa tetap tenang." },
  { "en": "Saya lebih suka hal-hal yang praktis. .........", "id": "Setuju, saya fokus pada hasil." },
  { "en": "Saya mudah memulai percakapan dengan orang asing. .........", "id": "Tidak setuju, saya agak pemalu." },
  { "en": "Saya sering menunda-nunda pekerjaan. .........", "id": "Tidak setuju, saya disiplin waktu." },
  { "en": "Kepentingan kelompok lebih penting dari kepentingan pribadi. .........", "id": "Setuju, kerjasama adalah kunci." },
  { "en": "Saya mudah tersinggung oleh perkataan orang. .........", "id": "Tidak setuju, saya tidak terlalu sensitif." },
  { "en": "Saya memiliki imajinasi yang sangat aktif. .........", "id": "Setuju, saya suka berkhayal." },
  { "en": "Saya lebih suka bekerja dalam tim. .........", "id": "Setuju, banyak ide lebih baik." },
  { "en": "Saya mengikuti jadwal yang saya buat. .........", "id": "Setuju, saya sangat terstruktur." },
  { "en": "Saya cenderung curiga pada niat orang lain. .........", "id": "Tidak setuju, saya berpikir positif." },
  { "en": "Saya jarang merasa cemas atau gelisah. .........", "id": "Setuju, saya merasa cukup aman." },
  { "en": "Saya suka mengunjungi tempat-tempat baru. .........", "id": "Setuju, saya suka petualangan." },
  { "en": "Saya tidak banyak bicara saat rapat. .........", "id": "Setuju, saya lebih banyak mengamati." },
  { "en": "Saya selalu siap sedia sebelum tenggat waktu. .........", "id": "Setuju, saya tidak suka terburu-buru." },
  { "en": "Saya senang melihat orang lain sukses. .........", "id": "Setuju, saya ikut merasa bahagia." },
  { "en": "Saya sering merasa tidak bersemangat. .........", "id": "Tidak setuju, saya punya banyak energi." },
  { "en": "Saya tidak suka perubahan dalam rencana. .........", "id": "Setuju, saya suka kepastian." },
  { "en": "Saya adalah orang yang memulai obrolan. .........", "id": "Setuju, saya suka bersosialisasi." },
  { "en": "Saya suka lingkungan kerja yang rapi. .........", "id": "Setuju, kerapian membantu saya fokus." },
  { "en": "Saya lebih suka bekerja sama daripada bersaing. .........", "id": "Setuju, kolaborasi lebih produktif." },
  { "en": "Saya bisa mengatasi kemunduran dengan cepat. .........", "id": "Setuju, saya orang yang tangguh." },
  { "en": "Saya lebih suka menonton film yang familier. .........", "id": "Setuju, saya suka kenyamanan." },
  { "en": "Saya merasa lelah setelah bersosialisasi. .........", "id": "Setuju, saya butuh waktu sendiri." },
  { "en": "Saya selalu berpikir sebelum bertindak. .........", "id": "Setuju, saya berhati-hati." },
  { "en": "Saya sangat peduli pada perasaan orang lain. .........", "id": "Setuju, saya sangat empatik." },
  { "en": "Saya sering merasa sedih tanpa alasan jelas. .........", "id": "Tidak setuju, perasaan saya stabil." },
  { "en": "Saya menikmati karya seni dan alam. .........", "id": "Setuju, saya menghargai keindahan." },
  { "en": "Saya lebih suka kelompok pertemanan yang kecil. .........", "id": "Setuju, lebih akrab dan mendalam." },
  { "en": "Saya suka membuat daftar pekerjaan. .........", "id": "Setuju, itu membuat saya terorganisir." },
  { "en": "Saya akan mengutarakan pendapat walau tidak populer. .........", "id": "Setuju, kebenaran lebih penting." },
  { "en": "Saya tidak mudah marah. .........", "id": "Setuju, saya orang yang sabar." },
  { "en": "Saya penasaran tentang banyak hal. .........", "id": "Setuju, saya suka belajar." },
  { "en": "Saya merasa canggung dalam keramaian. .........", "id": "Setuju, saya lebih nyaman sendiri." },
  { "en": "Saya dapat diandalkan oleh teman-teman saya. .........", "id": "Setuju, saya bertanggung jawab." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya lebih suka di belakang." },
  { "en": "Saya jarang merasa tegang. .........", "id": "Setuju, saya bisa rileks." },
  { "en": "Saya terbuka terhadap ide-ide baru. .........", "id": "Setuju, saya tidak kaku." },
  { "en": "Saya suka acara sosial yang besar. .........", "id": "Tidak setuju, terlalu ramai." },
  { "en": "Saya adalah orang yang ambisius. .........", "id": "Setuju, saya punya target tinggi." },
  { "en": "Saya sering memuji orang lain. .........", "id": "Setuju, saya suka memberi apresiasi." },
  { "en": "Saya merasa puas dengan diri saya. .........", "id": "Setuju, saya menerima diri sendiri." },
  { "en": "Saya lebih suka fakta daripada imajinasi. .........", "id": "Setuju, saya orang yang realistis." },
  { "en": "Saya sering memulai proyek baru. .........", "id": "Setuju, saya penuh inisiatif." },
  { "en": "Saya selalu menepati aturan. .........", "id": "Setuju, aturan dibuat untuk dipatuhi." },
  { "en": "Saya sabar menghadapi orang yang sulit. .........", "id": "Setuju, saya mencoba memahami." },
  { "en": "Saya jarang merasa kesepian. .........", "id": "Setuju, saya nyaman dengan diri sendiri." },
  { "en": "Saya suka memecahkan masalah yang rumit. .........", "id": "Setuju, itu sebuah tantangan." },
  { "en": "Saya merasa percaya diri di sekitar orang lain. .........", "id": "Setuju, saya nyaman bersosialisasi." },
  { "en": "Saya bekerja keras untuk mencapai tujuan. .........", "id": "Setuju, usaha tidak mengkhianati hasil." },
  { "en": "Saya tidak suka pamer pencapaian saya. .........", "id": "Setuju, saya orang yang rendah hati." },
  { "en": "Saya bisa menangani stres dengan baik. .........", "id": "Setuju, saya punya cara tersendiri." },
  { "en": "Saya memiliki minat yang luas. .........", "id": "Setuju, saya suka banyak hal." },
  { "en": "Saya tidak suka obrolan ringan. .........", "id": "Setuju, saya suka percakapan mendalam." },
  { "en": "Saya adalah orang yang efisien. .........", "id": "Setuju, saya tidak suka membuang waktu." },
  { "en": "Saya suka membantu orang lain tanpa pamrih. .........", "id": "Setuju, itu memberi kepuasan batin." },
  { "en": "Saya jarang merasa takut. .........", "id": "Tidak setuju, rasa takut itu wajar." },
  { "en": "Saya suka rutinitas yang monoton. .........", "id": "Tidak setuju, saya butuh variasi." },
  { "en": "Saya mudah berteman. .........", "id": "Setuju, saya orang yang terbuka." },
  { "en": "Saya suka merencanakan perjalanan secara detail. .........", "id": "Setuju, persiapan itu penting." },
  { "en": "Saya menghindari konfrontasi langsung. .........", "id": "Setuju, saya lebih suka diplomasi." },
  { "en": "Saya adalah orang yang optimis. .........", "id": "Setuju, saya melihat sisi baiknya." },
  { "en": "Saya suka berpikir di luar kotak. .........", "id": "Setuju, saya suka ide orisinal." },
  { "en": "Ketika bekerja dalam tim, saya cenderung .........", "id": "Mengambil peran sebagai pemimpin." },
  { "en": "Saya merasa tidak nyaman jika menjadi sorotan. .........", "id": "Setuju, saya lebih suka di belakang layar." },
  { "en": "Menurut teman-teman, saya adalah orang yang .........", "id": "Sangat bisa diandalkan." },
  { "en": "Menghadapi kritik membuat saya .........", "id": "Menerimanya sebagai masukan berharga." },
  { "en": "Saya lebih suka membicarakan ide-ide abstrak. .........", "id": "Setuju, saya suka teori." },
  { "en": "Saat membuat rencana perjalanan, saya .........", "id": "Merencanakannya secara sangat detail." },
  { "en": "Saya mudah merasa iri dengan keberhasilan orang. .........", "id": "Tidak setuju, saya ikut senang." },
  { "en": "Saat marah, saya biasanya .........", "id": "Memendamnya dan diam." },
  { "en": "Mengikuti aturan yang tidak masuk akal .........", "id": "Membuat saya merasa frustrasi." },
  { "en": "Saya adalah orang yang spontan. .........", "id": "Setuju, saya tidak suka banyak rencana." },
  { "en": "Berkenalan dengan orang baru di sebuah acara .........", "id": "Adalah hal yang mudah bagi saya." },
  { "en": "Saya mempersiapkan pakaian saya malam sebelumnya. .........", "id": "Setuju, agar pagi lebih efisien." },
  { "en": "Saya percaya setiap orang punya sisi baik. .........", "id": "Setuju, saya mencoba melihat hal positif." },
  { "en": "Kegagalan kecil bisa merusak suasana hati saya. .........", "id": "Tidak setuju, saya tidak terlalu memikirkannya." },
  { "en": "Saya sering merenungkan makna hidup. .........", "id": "Setuju, saya orang yang reflektif." },
  { "en": "Saya merasa tidak nyaman di lingkungan asing. .........", "id": "Setuju, saya butuh waktu beradaptasi." },
  { "en": "Saya selalu membayar tagihan tepat waktu. .........", "id": "Setuju, saya sangat disiplin." },
  { "en": "Saya tidak ragu mengungkapkan ketidaksetujuan saya. .........", "id": "Setuju, saya asertif dan jujur." },
  { "en": "Saya sering membandingkan diri dengan orang lain. .........", "id": "Setuju, itu memotivasi saya." },
  { "en": "Saya menikmati musik klasik atau jazz. .........", "id": "Setuju, saya menyukai alunan kompleks." },
  { "en": "Saat berbincang, saya lebih banyak .........", "id": "Mengajukan pertanyaan daripada bercerita." },
  { "en": "Kamar tidur saya selalu dalam keadaan rapi. .........", "id": "Setuju, saya suka kebersihan." },
  { "en": "Memberi kesempatan kedua itu penting. .........", "id": "Setuju, semua orang bisa salah." },
  { "en": "Saya sering merasa lelah secara mental. .........", "id": "Tidak setuju, saya punya stamina mental." },
  { "en": "Saya tidak suka pekerjaan yang repetitif. .........", "id": "Setuju, saya cepat merasa bosan." },
  { "en": "Saya suka menjadi tuan rumah sebuah acara. .........", "id": "Setuju, saya suka menghibur orang." },
  { "en": "Saya langsung membalas email atau pesan. .........", "id": "Setuju, saya responsif." },
  { "en": "Saya lebih suka mengalah untuk menjaga kedamaian. .........", "id": "Setuju, harmoni sangat penting." },
  { "en": "Saya sulit tidur jika terlalu banyak pikiran. .........", "id": "Setuju, pikiran saya sangat aktif." },
  { "en": "Saya lebih suka film dengan alur cerita rumit. .........", "id": "Setuju, saya suka menganalisis." },
  { "en": "Memiliki banyak teman adalah hal yang .........", "id": "Sangat penting dalam hidup saya." },
  { "en": "Saya memeriksa pekerjaan saya berulang kali. .........", "id": "Setuju, untuk memastikan tidak ada kesalahan." },
  { "en": "Saya tidak suka meminta bantuan orang lain. .........", "id": "Setuju, saya lebih suka mandiri." },
  { "en": "Saya adalah orang yang sangat sabar. .........", "id": "Setuju, saya tidak mudah terpancing." },
  { "en": "Belajar hal baru adalah hobi saya. .........", "id": "Setuju, saya pembelajar seumur hidup." },
  { "en": "Saya sering merasa kikuk dalam interaksi sosial. .........", "id": "Tidak setuju, saya merasa cukup luwes." },
  { "en": "Saya membuat daftar pro dan kontra sebelum memutuskan. .........", "id": "Setuju, saya orang yang analitis." },
  { "en": "Saya mudah percaya pada apa yang orang katakan. .........", "id": "Tidak setuju, saya agak skeptis." },
  { "en": "Saya tidak terlalu memikirkan pendapat orang lain. .........", "id": "Setuju, saya percaya diri." },
  { "en": "Saya tertarik pada budaya yang berbeda. .........", "id": "Setuju, saya suka keberagaman." },
  { "en": "Saya lebih suka akhir pekan yang tenang di rumah. .........", "id": "Setuju, untuk mengisi ulang energi." },
  { "en": "Saya adalah orang yang tepat waktu. .........", "id": "Setuju, saya menghargai waktu." },
  { "en": "Saya sering menawarkan bantuan kepada orang lain. .........", "id": "Setuju, saya suka menolong." },
  { "en": "Saya cenderung melihat sisi terburuk dari situasi. .........", "id": "Tidak setuju, saya seorang optimis." },
  { "en": "Saya sering bosan dengan rutinitas. .........", "id": "Setuju, saya butuh hal baru." },
  { "en": "Saya merasa nyaman berbicara dalam kelompok besar. .........", "id": "Setuju, saya suka berbagi ide." },
  { "en": "Saya tidak suka meninggalkan pekerjaan setengah jadi. .........", "id": "Setuju, harus selesai sampai tuntas." },
  { "en": "Saya merasa nyaman berbagi perasaan saya. .........", "id": "Tidak setuju, saya orang yang privat." },
  { "en": "Saya jarang merasa cemburu. .........", "id": "Setuju, saya punya rasa percaya." },
  { "en": "Saya suka mencoba resep masakan baru. .........", "id": "Setuju, saya suka bereksperimen." },
  { "en": "Saya merasa lebih produktif di malam hari. .........", "id": "Setuju, saya adalah 'night owl'." },
  { "en": "Saya adalah orang yang berhati-hati. .........", "id": "Setuju, saya selalu mempertimbangkan risiko." },
  { "en": "Saya akan mengakui jika saya berbuat salah. .........", "id": "Setuju, kejujuran itu penting." },
  { "en": "Saya sering merasa tegang di bahu atau leher. .........", "id": "Setuju, saya mudah merasa stres." },
  { "en": "Saya suka berdiskusi tentang berbagai topik. .........", "id": "Setuju, saya menikmati bertukar pikiran." },
  { "en": "Saya memilih untuk diam saat rapat. .........", "id": "Tidak setuju, saya aktif berpartisipasi." },
  { "en": "Saya suka merapikan barang setelah digunakan. .........", "id": "Setuju, saya tidak suka berantakan." },
  { "en": "Saya mencoba untuk tidak menghakimi orang lain. .........", "id": "Setuju, setiap orang punya cerita." },
  { "en": "Saya puas dengan kehidupan saya saat ini. .........", "id": "Setuju, saya bersyukur." },
  { "en": "Peraturan dibuat untuk dilanggar. .........", "id": "Tidak setuju, aturan menjaga ketertiban." },
  { "en": "Saya mudah bergaul dengan siapa saja. .........", "id": "Setuju, saya orang yang fleksibel." },
  { "en": "Saya lebih suka bekerja dengan target yang jelas. .........", "id": "Setuju, itu memberi saya arah." },
  { "en": "Saya adalah pendengar yang baik. .........", "id": "Setuju, teman sering curhat pada saya." },
  { "en": "Saya mudah merasa kesal karena hal sepele. .........", "id": "Tidak setuju, saya cukup sabar." },
  { "en": "Saya suka membaca buku non-fiksi. .........", "id": "Setuju, saya suka menambah pengetahuan." },
  { "en": "Saya sering menjadi orang terakhir yang pulang. .........", "id": "Tidak setuju, saya suka pulang cepat." },
  { "en": "Saya adalah orang yang pekerja keras. .........", "id": "Setuju, saya berdedikasi pada pekerjaan." },
  { "en": "Saya percaya pada kerja sama tim. .........", "id": "Setuju, bersama kita lebih kuat." },
  { "en": "Saya adalah orang yang tenang dan terkendali. .........", "id": "Setuju, saya jarang menunjukkan emosi." },
  { "en": "Saya suka mencari cara-cara baru untuk melakukan sesuatu. .........", "id": "Setuju, saya suka inovasi." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya lebih nyaman di balik layar." },
  { "en": "Saya orang yang praktis dan membumi. .........", "id": "Setuju, saya fokus pada kenyataan." },
  { "en": "Saya akan membela teman saya. .........", "id": "Setuju, saya adalah teman setia." },
  { "en": "Saya jarang khawatir tentang masa depan. .........", "id": "Setuju, saya hidup di saat ini." },
  { "en": "Saya terbuka pada kritik yang membangun. .........", "id": "Setuju, itu membantu saya berkembang." },
  { "en": "Saya merasa paling hidup saat berpetualang. .........", "id": "Setuju, saya suka mengambil risiko." },
  { "en": "Saya orang yang berorientasi pada detail. .........", "id": "Setuju, saya teliti dalam bekerja." },
  { "en": "Saya sangat menghargai kejujuran. .........", "id": "Setuju, itu adalah nilai utama." },
  { "en": "Saya sering merasa ragu pada kemampuan diri. .........", "id": "Tidak setuju, saya percaya diri." },
  { "en": "Saya suka melakukan sesuatu secara spontan. .........", "id": "Setuju, itu membuat hidup seru." },
  { "en": "Saya suka berada di tengah keramaian. .........", "id": "Setuju, saya suka energi orang banyak." },
  { "en": "Saya tidak suka perubahan pada menit terakhir. .........", "id": "Setuju, itu mengganggu rencana saya." },
  { "en": "Saya mudah berempati dengan karakter fiksi. .........", "id": "Setuju, saya mudah terhanyut." },
  { "en": "Saya merasa optimis tentang masa depan. .........", "id": "Setuju, saya punya harapan besar." },
  { "en": "Saya suka mencari pengalaman baru. .........", "id": "Setuju, saya tidak suka stagnan." },
  { "en": "Saya lebih suka berbicara langsung daripada lewat pesan. .........", "id": "Setuju, lebih personal dan jelas." },
  { "en": "Saya adalah orang yang telaten. .........", "id": "Setuju, saya sabar dalam mengerjakan sesuatu." },
  { "en": "Saya sangat menghargai kesopanan. .........", "id": "Setuju, itu menunjukkan rasa hormat." },
  { "en": "Saya jarang merasa terganggu oleh kebisingan. .........", "id": "Tidak setuju, saya butuh ketenangan." },
  { "en": "Saya percaya ada lebih dari satu cara benar. .........", "id": "Setuju, saya berpikiran terbuka." },
  { "en": "Saya suka percakapan yang mendalam dan berarti. .........", "id": "Setuju, obrolan ringan membosankan." },
  { "en": "Saya adalah perencana yang baik. .........", "id": "Setuju, saya selalu berpikir ke depan." },
  { "en": "Saya lebih suka memberi daripada menerima. .........", "id": "Setuju, itu lebih memuaskan." },
  { "en": "Saya bisa mengabaikan perasaan negatif dengan cepat. .........", "id": "Setuju, saya tidak suka berlarut-larut." },
  { "en": "Saya suka mengubah dekorasi rumah saya. .........", "id": "Setuju, saya suka suasana baru." },
  { "en": "Saat dihadapkan pada ketidakadilan, saya .........", "id": "Merasa harus melakukan sesuatu." },
  { "en": "Saya percaya bahwa keberuntungan itu .........", "id": "Hasil dari kerja keras." },
  { "en": "Menunggu sesuatu tanpa kepastian membuat saya .........", "id": "Merasa sangat tidak sabar." },
  { "en": "Menurut saya, tradisi adalah sesuatu yang .........", "id": "Penting untuk dilestarikan." },
  { "en": "Jika saya punya waktu luang, saya memilih .........", "id": "Melakukan hobi yang produktif." },
  { "en": "Saya lebih termotivasi oleh .........", "id": "Pujian dan pengakuan." },
  { "en": "Menyimpan rahasia adalah hal yang .........", "id": "Mudah bagi saya." },
  { "en": "Saya sering mengambil peran sebagai penengah. .........", "id": "Setuju, saya suka mencari solusi." },
  { "en": "Saya merasa 'FOMO' (takut ketinggalan). .........", "id": "Tidak setuju, saya nyaman sendiri." },
  { "en": "Saya suka hal-hal yang terbukti secara ilmiah. .........", "id": "Setuju, saya mempercayai data." },
  { "en": "Berada di alam membuat saya merasa .........", "id": "Tenang dan damai." },
  { "en": "Saya membuat keputusan berdasarkan logika, bukan perasaan. .........", "id": "Setuju, saya orang yang rasional." },
  { "en": "Saya cepat bosan dengan satu jenis pekerjaan. .........", "id": "Setuju, saya butuh variasi." },
  { "en": "Saya lebih suka film yang membuat berpikir. .........", "id": "Setuju, saya suka alur kompleks." },
  { "en": "Kesalahan orang lain membuat saya .........", "id": "Mencoba untuk memakluminya." },
  { "en": "Saya mudah terpengaruh oleh suasana hati orang lain. .........", "id": "Setuju, saya cukup empatik." },
  { "en": "Memulai proyek baru membuat saya merasa .........", "id": "Sangat bersemangat dan tertantang." },
  { "en": "Saya adalah orang yang taat aturan. .........", "id": "Setuju, aturan menjaga ketertiban." },
  { "en": "Saya merasa sulit untuk rileks. .........", "id": "Tidak setuju, saya mudah bersantai." },
  { "en": "Saya lebih suka percakapan tatap muka. .........", "id": "Setuju, lebih personal dan efektif." },
  { "en": "Saya tidak suka menjadi sorotan publik. .........", "id": "Setuju, saya lebih suka privasi." },
  { "en": "Saya selalu mencari makna di balik sesuatu. .........", "id": "Setuju, saya orang yang mendalam." },
  { "en": "Saya percaya pada kesempatan kedua. .........", "id": "Setuju, semua orang bisa berubah." },
  { "en": "Saya tidak mudah menyerah pada kesulitan. .........", "id": "Setuju, saya orang yang gigih." },
  { "en": "Saya lebih suka mendengarkan musik instrumental. .........", "id": "Setuju, membantu saya fokus." },
  { "en": "Saya merasa bertanggung jawab atas kebahagiaan saya. .........", "id": "Setuju, kebahagiaan adalah pilihan." },
  { "en": "Saya tidak suka basa-basi. .........", "id": "Setuju, saya lebih suka to the point." },
  { "en": "Saya adalah orang yang sangat kompetitif. .........", "id": "Setuju, saya suka tantangan menang." },
  { "en": "Saya sering merasa nostalgia. .........", "id": "Setuju, saya suka mengenang masa lalu." },
  { "en": "Saya suka pekerjaan yang membutuhkan ketelitian tinggi. .........", "id": "Setuju, saya menikmati detail." },
  { "en": "Saya merasa lebih baik setelah berolahraga. .........", "id": "Setuju, itu meningkatkan mood saya." },
  { "en": "Saya adalah orang yang pemaaf. .........", "id": "Setuju, saya tidak suka menyimpan dendam." },
  { "en": "Saya sering merasa cemas saat menunggu. .........", "id": "Tidak setuju, saya bisa menunggu dengan sabar." },
  { "en": "Saya lebih suka buku fiksi daripada non-fiksi. .........", "id": "Setuju, saya suka imajinasi." },
  { "en": "Saya tidak ragu untuk meminta maaf terlebih dahulu. .........", "id": "Setuju, itu menunjukkan kedewasaan." },
  { "en": "Saya adalah orang yang terus terang. .........", "id": "Setuju, saya mengatakan apa adanya." },
  { "en": "Saya lebih suka memimpin daripada dipimpin. .........", "id": "Setuju, saya punya visi sendiri." },
  { "en": "Saya merasa gelisah jika tidak melakukan apa-apa. .........", "id": "Setuju, saya harus tetap produktif." },
  { "en": "Saya menikmati kesendirian. .........", "id": "Setuju, itu waktu untuk refleksi." },
  { "en": "Saya suka mempelajari bahasa baru. .........", "id": "Setuju, itu membuka wawasan baru." },
  { "en": "Saya adalah orang yang sangat setia kawan. .........", "id": "Setuju, teman adalah keluarga." },
  { "en": "Saya tidak suka meminjam uang. .........", "id": "Setuju, saya lebih suka mandiri." },
  { "en": "Saya mudah beradaptasi dengan lingkungan baru. .........", "id": "Setuju, saya orang yang fleksibel." },
  { "en": "Saya percaya pada intuisi saya. .........", "id": "Setuju, intuisi seringkali benar." },
  { "en": "Saya suka seni pertunjukan seperti teater. .........", "id": "Setuju, saya menghargai seni peran." },
  { "en": "Saya adalah orang yang hemat. .........", "id": "Setuju, saya selalu menabung." },
  { "en": "Saya tidak suka menjadi bagian dari gosip. .........", "id": "Setuju, saya menghindari hal negatif." },
  { "en": "Saya bisa tetap fokus dalam lingkungan bising. .........", "id": "Tidak setuju, saya butuh ketenangan." },
  { "en": "Saya suka memberi hadiah kepada orang lain. .........", "id": "Setuju, itu membuat saya bahagia." },
  { "en": "Saya tidak takut mengambil risiko yang diperhitungkan. .........", "id": "Setuju, risiko bagian dari pertumbuhan." },
  { "en": "Saya adalah orang yang sederhana. .........", "id": "Setuju, saya tidak suka kemewahan." },
  { "en": "Saya mudah terharu oleh sebuah cerita. .........", "id": "Setuju, saya cukup sentimental." },
  { "en": "Saya tidak suka pusat perbelanjaan yang ramai. .........", "id": "Setuju, terlalu bising dan padat." },
  { "en": "Saya lebih suka kualitas daripada kuantitas. .........", "id": "Setuju, dalam pertemanan dan barang." },
  { "en": "Saya suka mengikuti berita dan isu terkini. .........", "id": "Setuju, saya suka tetap terinformasi." },
  { "en": "Saya adalah orang yang spiritual. .........", "id": "Setuju, saya percaya ada kekuatan lebih besar." },
  { "en": "Saya merasa sulit untuk meminta bantuan. .........", "id": "Setuju, saya enggan merepotkan orang lain." },
  { "en": "Saya adalah orang yang apa adanya. .........", "id": "Setuju, saya tidak suka berpura-pura." },
  { "en": "Saya sering meragukan keputusan saya sendiri. .........", "id": "Tidak setuju, saya cukup yakin." },
  { "en": "Saya suka aktivitas yang memacu adrenalin. .........", "id": "Setuju, saya suka tantangan ekstrem." },
  { "en": "Saya adalah pendengar yang sabar. .........", "id": "Setuju, saya memberi orang waktu bicara." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya lebih suka mengamati." },
  { "en": "Saya adalah orang yang sangat disiplin. .........", "id": "Setuju, saya taat pada jadwal." },
  { "en": "Saya suka rumah yang penuh dengan tamu. .........", "id": "Tidak setuju, saya lebih suka ketenangan." },
  { "en": "Saya menghargai seni lebih dari sains. .........", "id": "Tidak setuju, keduanya sama pentingnya." },
  { "en": "Saya adalah orang yang sangat mandiri. .........", "id": "Setuju, saya bisa mengurus diri sendiri." },
  { "en": "Saya tidak mudah terintimidasi. .........", "id": "Setuju, saya punya pendirian kuat." },
  { "en": "Saya sering melamun. .........", "id": "Setuju, imajinasi saya sangat aktif." },
  { "en": "Saya lebih suka bekerja untuk tujuan sosial. .........", "id": "Setuju, memberi dampak lebih penting." },
  { "en": "Saya adalah orang yang sangat terencana. .........", "id": "Setuju, saya tidak suka kejutan." },
  { "en": "Saya merasa kesal jika menunggu terlalu lama. .........", "id": "Setuju, saya tidak sabaran." },
  { "en": "Saya suka melakukan pekerjaan sukarela. .........", "id": "Setuju, membantu orang itu memuaskan." },
  { "en": "Saya adalah orang yang metodis. .........", "id": "Setuju, saya suka langkah-langkah terstruktur." },
  { "en": "Saya tidak suka membuang-buang waktu. .........", "id": "Setuju, waktu sangat berharga." },
  { "en": "Saya lebih suka film sedih daripada lucu. .........", "id": "Tidak setuju, saya suka hiburan ringan." },
  { "en": "Saya adalah orang yang konsisten. .........", "id": "Setuju, perkataan dan tindakan saya sejalan." },
  { "en": "Saya mudah memaafkan diri sendiri. .........", "id": "Setuju, semua orang membuat kesalahan." },
  { "en": "Saya suka pekerjaan yang dinamis dan berubah-ubah. .........", "id": "Setuju, saya tidak suka monoton." },
  { "en": "Saya adalah orang yang sangat ekspresif. .........", "id": "Setuju, perasaan saya mudah terlihat." },
  { "en": "Saya tidak suka menjadi bagian dari kerumunan. .........", "id": "Setuju, saya lebih suka ruang pribadi." },
  { "en": "Saya adalah orang yang berprinsip. .........", "id": "Setuju, saya punya nilai yang dipegang." },
  { "en": "Saya mudah merasa bersalah. .........", "id": "Tidak setuju, saya bertanggung jawab." },
  { "en": "Saya suka permainan strategi seperti catur. .........", "id": "Setuju, saya suka berpikir ke depan." },
  { "en": "Saya adalah orang yang sangat logis. .........", "id": "Setuju, saya mengandalkan fakta." },
  { "en": "Saya tidak suka menjadi sorotan. .........", "id": "Setuju, saya lebih suka di belakang layar." },
  { "en": "Saya adalah orang yang sangat rapi. .........", "id": "Setuju, saya suka keteraturan." },
  { "en": "Saya sering merasa lelah setelah bekerja. .........", "id": "Setuju, pekerjaan saya menuntut energi." },
  { "en": "Saya suka menulis jurnal atau buku harian. .........", "id": "Setuju, itu membantu saya merefleksikan diri." },
  { "en": "Saya adalah orang yang sangat realistis. .........", "id": "Setuju, saya melihat dunia apa adanya." },
  { "en": "Saya tidak suka berhutang budi. .........", "id": "Setuju, saya lebih suka memberi." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya tidak mudah panik." },
  { "en": "Saya suka belajar tentang sejarah. .........", "id": "Setuju, masa lalu memberi pelajaran." },
  { "en": "Saya adalah orang yang sangat blak-blakan. .........", "id": "Setuju, saya tidak suka basa-basi." },
  { "en": "Saya tidak suka membuang makanan. .........", "id": "Setuju, saya menghargai makanan." },
  { "en": "Saya adalah orang yang sangat teliti. .........", "id": "Setuju, saya memperhatikan detail kecil." },
  { "en": "Saya suka bekerja dengan angka dan data. .........", "id": "Setuju, saya suka hal yang pasti." },
  { "en": "Saya adalah orang yang sangat positif. .........", "id": "Setuju, saya selalu melihat sisi baik." },
  { "en": "Saya tidak suka kejutan. .........", "id": "Setuju, saya lebih suka persiapan." },
  { "en": "Ketika teman saya sedih, saya .........", "id": "Menjadi pendengar yang baik untuknya." },
  { "en": "Saya lebih termotivasi oleh .........", "id": "Kepuasan kerja daripada uang." },
  { "en": "Saya tidak merasa perlu untuk menyenangkan semua orang. .........", "id": "Setuju, saya fokus pada prinsip." },
  { "en": "Jika saya melihat rekan kerja berbuat curang, saya .........", "id": "Akan melaporkannya pada atasan." },
  { "en": "Setelah mengalami kegagalan besar, saya .........", "id": "Mencoba belajar dari kesalahan." },
  { "en": "Kekalahan dalam permainan membuat saya .........", "id": "Merasa sedikit kesal dan tertantang." },
  { "en": "Saya suka membuat orang lain tertawa. .........", "id": "Setuju, humor itu penting." },
  { "en": "Saya merasa sulit untuk memulai kebiasaan baru. .........", "id": "Tidak setuju, saya mudah beradaptasi." },
  { "en": "Saya percaya pada kerja keras mengalahkan bakat. .........", "id": "Setuju, usaha adalah kunci utama." },
  { "en": "Saya lebih suka kota besar daripada pedesaan. .........", "id": "Setuju, saya suka dinamika kota." },
  { "en": "Saya menghargai seni yang sulit dipahami. .........", "id": "Setuju, saya suka menafsirkannya." },
  { "en": "Saya tidak suka menjadi bagian dari drama. .........", "id": "Setuju, saya menghindari konflik." },
  { "en": "Saya merasa paling produktif saat .........", "id": "Bekerja di bawah tekanan." },
  { "en": "Saya seringkali menjadi orang yang mengalah. .........", "id": "Setuju, demi menjaga keharmonisan." },
  { "en": "Saya mudah bosan dengan orang. .........", "id": "Tidak setuju, saya menghargai hubungan." },
  { "en": "Saya suka merakit atau memperbaiki barang. .........", "id": "Setuju, saya menikmati pekerjaan tangan." },
  { "en": "Saya lebih suka liburan yang santai. .........", "id": "Setuju, saya butuh istirahat total." },
  { "en": "Saya adalah orang yang visioner. .........", "id": "Setuju, saya suka memikirkan masa depan." },
  { "en": "Saya tidak suka membagikan barang pribadi saya. .........", "id": "Setuju, saya menjaga barang saya." },
  { "en": "Saya merasa tenang saat hujan turun. .........", "id": "Setuju, suaranya menenangkan." },
  { "en": "Saya suka menghadiri seminar atau lokakarya. .........", "id": "Setuju, saya suka mengembangkan diri." },
  { "en": "Saya lebih suka berbicara daripada menulis. .........", "id": "Setuju, lebih ekspresif dan cepat." },
  { "en": "Saya orang yang sangat berhati-hati saat berkendara. .........", "id": "Setuju, keselamatan nomor satu." },
  { "en": "Saya tidak keberatan bekerja di akhir pekan. .........", "id": "Setuju, jika pekerjaan menuntut." },
  { "en": "Saya merasa canggung menerima pujian. .........", "id": "Setuju, saya merasa sedikit malu." },
  { "en": "Saya suka mengatur dan mengkategorikan barang. .........", "id": "Setuju, saya suka semuanya teratur." },
  { "en": "Saya adalah orang yang sangat idealis. .........", "id": "Setuju, saya punya standar tinggi." },
  { "en": "Saya tidak mudah teralihkan saat bekerja. .........", "id": "Setuju, saya bisa sangat fokus." },
  { "en": "Saya merasa penting untuk mengikuti tradisi keluarga. .........", "id": "Setuju, itu bagian dari identitas." },
  { "en": "Saya suka tampil beda dari orang lain. .........", "id": "Setuju, saya suka mengekspresikan diri." },
  { "en": "Saya adalah orang yang sangat intuitif. .........", "id": "Setuju, saya sering mengikuti kata hati." },
  { "en": "Saya tidak suka perubahan dalam kepemimpinan. .........", "id": "Tidak setuju, perubahan bisa baik." },
  { "en": "Saya merasa sulit untuk meminta kenaikan gaji. .........", "id": "Setuju, saya merasa tidak enak." },
  { "en": "Saya suka menyelesaikan teka-teki silang atau sudoku. .........", "id": "Setuju, itu mengasah otak saya." },
  { "en": "Saya adalah orang yang sangat lugas. .........", "id": "Setuju, saya tidak suka berbelit-belit." },
  { "en": "Saya tidak suka menjadi orang yang bertanggung jawab. .........", "id": "Tidak setuju, saya suka memegang kendali." },
  { "en": "Saya mudah merasa bersimpati pada hewan. .........", "id": "Setuju, saya seorang penyayang binatang." },
  { "en": "Saya adalah orang yang pendiam di lingkungan baru. .........", "id": "Setuju, saya perlu waktu untuk observasi." },
  { "en": "Saya lebih suka film aksi daripada drama. .........", "id": "Setuju, saya suka yang menegangkan." },
  { "en": "Saya adalah orang yang sangat tekun. .........", "id": "Setuju, saya tidak mudah menyerah." },
  { "en": "Saya tidak suka menjadi bagian dari grup besar. .........", "id": "Setuju, saya lebih suka grup kecil." },
  { "en": "Saya merasa tidak nyaman dengan keheningan. .........", "id": "Tidak setuju, saya menikmati ketenangan." },
  { "en": "Saya suka melakukan sesuatu sesuai prosedur. .........", "id": "Setuju, itu meminimalisir kesalahan." },
  { "en": "Saya adalah orang yang sangat penyayang. .........", "id": "Setuju, saya peduli pada orang lain." },
  { "en": "Saya sering merasa perlu menyendiri. .........", "id": "Setuju, untuk mengisi ulang energi." },
  { "en": "Saya suka membuat rencana cadangan. .........", "id": "Setuju, untuk mengantisipasi masalah." },
  { "en": "Saya tidak suka membicarakan diri sendiri. .........", "id": "Setuju, saya lebih suka mendengarkan." },
  { "en": "Saya mudah bergaul dengan orang yang lebih tua. .........", "id": "Setuju, saya menghormati pengalaman mereka." },
  { "en": "Saya adalah orang yang sangat ingin tahu. .........", "id": "Setuju, saya selalu bertanya 'mengapa'." },
  { "en": "Saya lebih suka liburan yang penuh petualangan. .........", "id": "Setuju, saya suka mencoba hal baru." },
  { "en": "Saya tidak suka meminjamkan barang saya. .........", "id": "Setuju, saya takut barangnya rusak." },
  { "en": "Saya adalah orang yang sangat gigih. .........", "id": "Setuju, saya akan terus mencoba." },
  { "en": "Saya tidak suka berbicara di telepon. .........", "id": "Setuju, saya lebih suka pesan teks." },
  { "en": "Saya mudah merasa gelisah. .........", "id": "Tidak setuju, saya orang yang tenang." },
  { "en": "Saya suka pekerjaan yang menantang secara intelektual. .........", "id": "Setuju, saya suka memecahkan masalah." },
  { "en": "Saya lebih suka menabung daripada berbelanja. .........", "id": "Setuju, saya memikirkan masa depan." },
  { "en": "Saya adalah orang yang sangat bersemangat. .........", "id": "Setuju, saya punya banyak energi." },
  { "en": "Saya tidak keberatan dikritik di depan umum. .........", "id": "Tidak setuju, itu membuat tidak nyaman." },
  { "en": "Saya adalah orang yang sangat romantis. .........", "id": "Setuju, saya suka gestur-gestur kecil." },
  { "en": "Saya lebih suka bekerja di belakang layar. .........", "id": "Setuju, saya tidak butuh pengakuan." },
  { "en": "Saya adalah orang yang sangat independen. .........", "id": "Setuju, saya tidak suka bergantung." },
  { "en": "Saya tidak suka ketidakpastian. .........", "id": "Setuju, saya suka hal yang jelas." },
  { "en": "Saya mudah terinspirasi oleh hal-hal di sekitar. .........", "id": "Setuju, saya menemukan ide di mana saja." },
  { "en": "Saya adalah orang yang sangat loyal. .........", "id": "Setuju, saya setia pada teman." },
  { "en": "Saya tidak suka berada di tempat yang kotor. .........", "id": "Setuju, saya menyukai kebersihan." },
  { "en": "Saya merasa lebih hidup di malam hari. .........", "id": "Setuju, saya seorang 'night owl'." },
  { "en": "Saya suka mengikuti aturan dan jadwal. .........", "id": "Setuju, itu membuat hidup teratur." },
  { "en": "Saya adalah orang yang sangat murah hati. .........", "id": "Setuju, saya suka berbagi." },
  { "en": "Saya sering merasa gugup saat bertemu orang baru. .........", "id": "Tidak setuju, saya merasa antusias." },
  { "en": "Saya suka mempelajari cara kerja sesuatu. .........", "id": "Setuju, saya punya rasa ingin tahu." },
  { "en": "Saya lebih suka percakapan yang ringan dan lucu. .........", "id": "Setuju, saya suka suasana santai." },
  { "en": "Saya adalah orang yang sangat efisien dalam bekerja. .........", "id": "Setuju, saya tidak suka bertele-tele." },
  { "en": "Saya tidak suka menjadi minoritas dalam sebuah grup. .........", "id": "Tidak setuju, saya berani berbeda." },
  { "en": "Saya adalah orang yang sangat bersyukur. .........", "id": "Setuju, saya menghargai apa yang saya miliki." },
  { "en": "Saya suka mendengarkan musik dengan lirik puitis. .........", "id": "Setuju, saya menikmati makna kata." },
  { "en": "Saya lebih suka bekerja dengan cepat. .........", "id": "Setuju, saya tidak suka menunda." },
  { "en": "Saya tidak suka membuang waktu untuk hal sepele. .........", "id": "Setuju, saya fokus pada hal penting." },
  { "en": "Saya adalah orang yang sangat jujur. .........", "id": "Setuju, bahkan jika menyakitkan." },
  { "en": "Saya sering merasa lelah tanpa sebab yang jelas. .........", "id": "Tidak setuju, energi saya stabil." },
  { "en": "Saya suka mencari solusi kreatif untuk masalah. .........", "id": "Setuju, saya suka berpikir 'out of the box'." },
  { "en": "Saya adalah orang yang sangat terbuka. .........", "id": "Setuju, saya mudah berbagi cerita." },
  { "en": "Saya tidak suka diminta melakukan sesuatu mendadak. .........", "id": "Setuju, saya butuh waktu untuk bersiap." },
  { "en": "Saya adalah orang yang sangat suportif terhadap teman. .........", "id": "Setuju, saya selalu ada untuk mereka." },
  { "en": "Saya merasa sulit untuk rileks setelah hari yang sibuk. .........", "id": "Setuju, pikiran saya terus bekerja." },
  { "en": "Saya suka membaca buku tentang pengembangan diri. .........", "id": "Setuju, saya ingin menjadi lebih baik." },
  { "en": "Saya lebih suka bekerja di lingkungan yang kompetitif. .........", "id": "Setuju, itu mendorong saya." },
  { "en": "Saya adalah orang yang sangat berorientasi pada tujuan. .........", "id": "Setuju, saya fokus pada target." },
  { "en": "Saya tidak suka menjadi pemimpin. .........", "id": "Setuju, saya lebih nyaman sebagai pengikut." },
  { "en": "Saya merasa tenang ketika berada di dekat air. .........", "id": "Setuju, itu sangat menenangkan." },
  { "en": "Saya suka melakukan banyak hal sekaligus. .........", "id": "Tidak setuju, saya fokus pada satu hal." },
  { "en": "Saya lebih suka menjadi spesialis daripada generalis. .........", "id": "Setuju, saya suka mendalami satu bidang." },
  { "en": "Saya adalah orang yang sangat menghargai privasi. .........", "id": "Setuju, saya tidak suka mengumbar." },
  { "en": "Saya tidak suka dinilai oleh orang lain. .........", "id": "Setuju, itu membuat saya tidak nyaman." },
  { "en": "Saya merasa bersemangat saat mempelajari keterampilan baru. .........", "id": "Setuju, saya suka tantangan belajar." },
  { "en": "Saya adalah orang yang sangat diplomatis. .........", "id": "Setuju, saya bisa menengahi konflik." },
  { "en": "Saya tidak suka memikirkan masa lalu. .........", "id": "Setuju, saya fokus pada masa kini." },
  { "en": "Saya merasa lebih baik setelah menuliskan pikiran saya. .........", "id": "Setuju, itu membantu menjernihkan pikiran." },
  { "en": "Saya adalah orang yang cenderung mengambil inisiatif. .........", "id": "Setuju, saya tidak suka menunggu." },
  { "en": "Saya tidak keberatan melakukan tugas yang repetitif. .........", "id": "Setuju, saya menemukan ritme di dalamnya." },
  { "en": "Saya lebih suka film dengan akhir yang bahagia. .........", "id": "Setuju, saya suka cerita positif." },
  { "en": "Ketika saya berjanji, saya .........", "id": "Selalu berusaha untuk menepatinya." },
  { "en": "Saya merasa sulit untuk fokus pada satu hal. .........", "id": "Tidak setuju, saya bisa fokus mendalam." },
  { "en": "Saya mudah memercayai orang yang baru saya temui. .........", "id": "Tidak setuju, saya perlu waktu." },
  { "en": "Saya sering merasa energi saya terkuras. .........", "id": "Tidak setuju, saya cukup berenergi." },
  { "en": "Saya lebih suka liburan ke tempat yang sama. .........", "id": "Setuju, saya menyukai yang familier." },
  { "en": "Saya merasa nyaman menjadi bagian dari kelompok. .........", "id": "Setuju, saya suka rasa kebersamaan." },
  { "en": "Saya adalah orang yang sangat teliti. .........", "id": "Setuju, tidak ada detail yang terlewat." },
  { "en": "Saya akan mengorbankan waktu saya untuk teman. .........", "id": "Setuju, teman sangat berarti." },
  { "en": "Saya jarang memikirkan hal-hal yang tidak menyenangkan. .........", "id": "Setuju, saya fokus pada hal positif." },
  { "en": "Saya suka mengunjungi museum atau galeri seni. .........", "id": "Setuju, saya menghargai karya seni." },
  { "en": "Saya lebih suka percakapan empat mata. .........", "id": "Setuju, terasa lebih personal." },
  { "en": "Saya tidak suka jika barang saya dipinjam. .........", "id": "Setuju, saya sangat menjaga barang." },
  { "en": "Saya melihat kritik sebagai serangan pribadi. .........", "id": "Tidak setuju, itu adalah masukan." },
  { "en": "Saya adalah orang yang sangat ekspresif secara verbal. .........", "id": "Setuju, saya suka bercerita." },
  { "en": "Saya suka hal-hal yang teratur dan dapat diprediksi. .........", "id": "Setuju, saya tidak suka kejutan." },
  { "en": "Saya suka bekerja di lingkungan yang serba cepat. .........", "id": "Setuju, saya suka tantangan dinamis." },
  { "en": "Saya selalu berusaha melihat dari sudut pandang lain. .........", "id": "Setuju, untuk memahami lebih baik." },
  { "en": "Saya mudah merasa bersalah atas hal kecil. .........", "id": "Tidak setuju, saya mudah memaafkan diri." },
  { "en": "Saya tidak tertarik pada politik. .........", "id": "Tidak setuju, saya suka mengikuti isu." },
  { "en": "Saya merasa sulit untuk duduk diam tanpa melakukan apa-apa. .........", "id": "Setuju, saya harus selalu bergerak." },
  { "en": "Saya adalah orang yang taat pada tenggat waktu. .........", "id": "Setuju, saya sangat menghargai waktu." },
  { "en": "Saya lebih suka memuji di depan umum. .........", "id": "Setuju, apresiasi itu penting." },
  { "en": "Saya merasa optimis bahkan saat situasi sulit. .........", "id": "Setuju, selalu ada harapan." },
  { "en": "Saya lebih suka bekerja dengan instruksi yang jelas. .........", "id": "Setuju, saya butuh arahan." },
  { "en": "Saya tidak suka menjadi pembicara dalam sebuah grup. .........", "id": "Setuju, saya lebih suka mendengarkan." },
  { "en": "Saya adalah orang yang sangat berhati-hati. .........", "id": "Setuju, saya selalu pikirkan konsekuensinya." },
  { "en": "Saya percaya pada kerja keras dan dedikasi. .........", "id": "Setuju, itu adalah kunci sukses." },
  { "en": "Saya merasa tidak nyaman dengan perubahan besar. .........", "id": "Setuju, saya suka stabilitas." },
  { "en": "Saya suka mencoba berbagai jenis musik. .........", "id": "Setuju, saya terbuka pada genre baru." },
  { "en": "Saya merasa energik di pagi hari. .........", "id": "Setuju, saya adalah 'morning person'." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab. .........", "id": "Setuju, saya menyelesaikan tugas saya." },
  { "en": "Saya tidak suka bersaing dengan teman. .........", "id": "Setuju, persahabatan lebih penting." },
  { "en": "Saya jarang merasa kecewa. .........", "id": "Tidak setuju, kecewa itu perasaan wajar." },
  { "en": "Saya suka membaca buku fiksi ilmiah. .........", "id": "Setuju, saya suka imajinasi masa depan." },
  { "en": "Saya lebih suka menghabiskan uang untuk pengalaman. .........", "id": "Setuju, kenangan lebih berharga." },
  { "en": "Saya adalah orang yang sangat metodis. .........", "id": "Setuju, saya bekerja secara terstruktur." },
  { "en": "Saya tidak suka membiarkan orang lain menunggu. .........", "id": "Setuju, saya sangat tepat waktu." },
  { "en": "Saya jarang merasa marah. .........", "id": "Setuju, saya bisa mengontrol emosi." },
  { "en": "Saya suka mencari tahu cara kerja sesuatu. .........", "id": "Setuju, saya punya rasa ingin tahu." },
  { "en": "Saya merasa nyaman di antara orang-orang yang saya kenal. .........", "id": "Setuju, saya suka lingkungan familier." },
  { "en": "Saya adalah orang yang fokus pada detail. .........", "id": "Setuju, saya tidak suka kesalahan kecil." },
  { "en": "Saya suka memberi nasihat kepada teman-teman. .........", "id": "Setuju, saya senang bisa membantu." },
  { "en": "Saya tidak mudah terpengaruh oleh stres. .........", "id": "Setuju, saya punya daya tahan baik." },
  { "en": "Saya percaya bahwa setiap masalah ada solusinya. .........", "id": "Setuju, saya selalu mencari jalan keluar." },
  { "en": "Saya suka menjadi bagian dari suatu komunitas. .........", "id": "Setuju, saya suka rasa memiliki." },
  { "en": "Saya adalah orang yang sangat disiplin diri. .........", "id": "Setuju, saya bisa mengontrol diri." },
  { "en": "Saya menghindari menjadi pusat perhatian. .........", "id": "Setuju, saya lebih suka di belakang layar." },
  { "en": "Saya sering merenungkan kesalahan saya. .........", "id": "Tidak setuju, saya fokus pada perbaikan." },
  { "en": "Saya tidak suka pekerjaan dengan banyak aturan. .........", "id": "Setuju, saya suka kebebasan berkreasi." },
  { "en": "Saya adalah orang yang sangat talkative. .........", "id": "Setuju, saya suka mengobrol." },
  { "en": "Saya tidak suka menunda keputusan. .........", "id": "Setuju, saya orang yang tegas." },
  { "en": "Saya merasa penting untuk bersikap sopan. .........", "id": "Setuju, itu mencerminkan diri kita." },
  { "en": "Saya jarang merasa cemas akan hal-hal baru. .........", "id": "Setuju, saya melihatnya sebagai petualangan." },
  { "en": "Saya suka mengikuti perkembangan teknologi. .........", "id": "Setuju, saya suka inovasi." },
  { "en": "Saya lebih suka bekerja di lingkungan yang tenang. .........", "id": "Setuju, itu membantu saya berkonsentrasi." },
  { "en": "Saya adalah orang yang sangat gigih. .........", "id": "Setuju, saya tidak mudah putus asa." },
  { "en": "Saya tidak suka membicarakan orang lain. .........", "id": "Setuju, saya lebih suka membicarakan ide." },
  { "en": "Saya adalah orang yang sangat stabil secara emosional. .........", "id": "Setuju, suasana hati saya konsisten." },
  { "en": "Saya lebih suka seni yang realistis. .........", "id": "Setuju, saya suka yang jelas." },
  { "en": "Saya suka memimpin sebuah proyek. .........", "id": "Setuju, saya suka mengambil tanggung jawab." },
  { "en": "Saya adalah orang yang sangat teratur. .........", "id": "Setuju, semua ada tempatnya." },
  { "en": "Saya tidak suka mengecewakan orang lain. .........", "id": "Setuju, saya berusaha menepati ekspektasi." },
  { "en": "Saya adalah orang yang sangat positif. .........", "id": "Setuju, saya melihat gelas setengah penuh." },
  { "en": "Saya suka melakukan hal-hal yang tidak terduga. .........", "id": "Setuju, saya suka memberi kejutan." },
  { "en": "Saya tidak banyak bicara dalam kelompok baru. .........", "id": "Setuju, saya butuh waktu untuk terbuka." },
  { "en": "Saya selalu menyelesaikan tugas sebelum tenggat waktu. .........", "id": "Setuju, saya suka punya waktu ekstra." },
  { "en": "Saya adalah orang yang sangat koperatif. .........", "id": "Setuju, saya bisa bekerja dengan siapa saja." },
  { "en": "Saya tidak mudah tersinggung. .........", "id": "Setuju, saya tidak menganggap serius semuanya." },
  { "en": "Saya suka berdebat tentang topik yang saya minati. .........", "id": "Setuju, itu cara bertukar ide." },
  { "en": "Saya merasa berenergi saat berada di luar ruangan. .........", "id": "Setuju, saya suka udara segar." },
  { "en": "Saya adalah orang yang dapat dipercaya. .........", "id": "Setuju, saya bisa menjaga rahasia." },
  { "en": "Saya tidak suka ketidakadilan dalam bentuk apapun. .........", "id": "Setuju, saya menjunjung tinggi keadilan." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya tidak mudah terhasut." },
  { "en": "Saya suka mencoba aktivitas baru sendirian. .........", "id": "Setuju, saya menikmati waktu sendiri." },
  { "en": "Saya lebih suka pekerjaan yang stabil. .........", "id": "Setuju, saya menyukai keamanan." },
  { "en": "Saya adalah orang yang sangat efisien. .........", "id": "Setuju, saya mencari cara tercepat." },
  { "en": "Saya tidak suka menjadi beban bagi orang lain. .........", "id": "Setuju, saya berusaha mandiri." },
  { "en": "Saya adalah orang yang sangat bersemangat. .........", "id": "Setuju, saya mudah antusias." },
  { "en": "Saya suka merencanakan masa depan. .........", "id": "Setuju, saya suka memiliki tujuan." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya lebih suka di belakang panggung." },
  { "en": "Saya adalah orang yang sangat patuh. .........", "id": "Setuju, saya mengikuti arahan dengan baik." },
  { "en": "Saya tidak suka memikirkan masalah terlalu lama. .........", "id": "Setuju, saya lebih suka mencari solusi." },
  { "en": "Saya adalah orang yang sangat tangguh. .........", "id": "Setuju, saya bisa bangkit dari kegagalan." },
  { "en": "Saya suka tantangan intelektual. .........", "id": "Setuju, saya suka mengasah pikiran." },
  { "en": "Saya lebih suka berbicara dengan beberapa teman dekat. .........", "id": "Setuju, kualitas lebih penting dari kuantitas." },
  { "en": "Saya adalah orang yang sangat sadar diri. .........", "id": "Setuju, saya tahu kelebihan dan kekurangan." },
  { "en": "Saya tidak suka berkonflik dengan atasan. .........", "id": "Setuju, saya menjaga hubungan profesional." },
  { "en": "Saya jarang merasa bosan. .........", "id": "Setuju, saya selalu menemukan sesuatu." },
  { "en": "Saya suka hal-hal yang memiliki makna mendalam. .........", "id": "Setuju, saya tidak suka yang dangkal." },
  { "en": "Saya merasa paling kreatif saat .........", "id": "Bekerja sendirian dalam ketenangan." },
  { "en": "Saya adalah orang yang sangat logis. .........", "id": "Setuju, saya berdasarkan fakta dan data." },
  { "en": "Saya tidak suka pamer. .........", "id": "Setuju, saya lebih suka rendah hati." },
  { "en": "Saya adalah orang yang sangat tabah. .........", "id": "Setuju, saya bisa menahan kesulitan." },
  { "en": "Ketika ada konflik dalam tim, saya .........", "id": "Berusaha menjadi penengah." },
  { "en": "Saya lebih suka pekerjaan yang memberi dampak sosial. .........", "id": "Setuju, kepuasan batin itu penting." },
  { "en": "Saya tidak suka jika orang terlambat. .........", "id": "Setuju, saya sangat menghargai waktu." },
  { "en": "Saya merasa sulit mengungkapkan perasaan saya. .........", "id": "Setuju, saya orang yang agak tertutup." },
  { "en": "Menghadapi kegagalan, saya .........", "id": "Cepat bangkit dan mencoba lagi." },
  { "en": "Saya lebih suka mengikuti intuisi daripada data. .........", "id": "Tidak setuju, saya butuh fakta." },
  { "en": "Saya menikmati menjadi mentor bagi orang lain. .........", "id": "Setuju, saya suka berbagi ilmu." },
  { "en": "Saya merasa tidak nyaman jika tidak produktif. .........", "id": "Setuju, saya harus selalu aktif." },
  { "en": "Saya percaya setiap orang berhak atas pendapatnya. .........", "id": "Setuju, saya menghargai perbedaan." },
  { "en": "Saya lebih suka liburan yang terencana matang. .........", "id": "Setuju, saya tidak suka ketidakpastian." },
  { "en": "Saya adalah orang yang sangat kompetitif. .........", "id": "Tidak setuju, saya fokus pada diri sendiri." },
  { "en": "Saya tidak suka meminjam barang dari orang lain. .........", "id": "Setuju, saya tidak mau merepotkan." },
  { "en": "Saya merasa kesal jika rencana saya terganggu. .........", "id": "Setuju, saya suka semuanya teratur." },
  { "en": "Saya suka membuat lelucon untuk mencairkan suasana. .........", "id": "Setuju, saya punya selera humor." },
  { "en": "Saya lebih suka bekerja dengan target jangka pendek. .........", "id": "Setuju, terasa lebih cepat tercapai." },
  { "en": "Saya adalah orang yang sangat pemaaf. .........", "id": "Setuju, saya tidak suka menyimpan amarah." },
  { "en": "Saya tidak mudah terpengaruh oleh tren. .........", "id": "Setuju, saya punya gaya sendiri." },
  { "en": "Saya merasa perlu mendapat persetujuan orang lain. .........", "id": "Tidak setuju, saya percaya diri." },
  { "en": "Saya suka menghadiri acara-acara kebudayaan. .........", "id": "Setuju, saya suka memperluas wawasan." },
  { "en": "Saya lebih suka bekerja di lingkungan yang kolaboratif. .........", "id": "Setuju, ide bersama lebih baik." },
  { "en": "Saya adalah orang yang sangat bersemangat. .........", "id": "Setuju, saya punya antusiasme tinggi." },
  { "en": "Saya tidak suka pekerjaan yang terlalu banyak detail. .........", "id": "Tidak setuju, saya menikmati ketelitian." },
  { "en": "Saya merasa penting untuk menjaga hubungan baik. .........", "id": "Setuju, jaringan itu aset." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya tidak mudah gelisah." },
  { "en": "Saya lebih suka belajar dari pengalaman. .........", "id": "Setuju, praktik lebih baik dari teori." },
  { "en": "Saya tidak suka berada dalam situasi yang canggung. .........", "id": "Setuju, itu membuat saya tidak nyaman." },
  { "en": "Saya selalu berusaha menyelesaikan pekerjaan lebih awal. .........", "id": "Setuju, untuk menghindari keterlambatan." },
  { "en": "Saya akan membela seseorang yang diperlakukan tidak adil. .........", "id": "Setuju, saya menjunjung tinggi keadilan." },
  { "en": "Saya sering memikirkan makna dari segala sesuatu. .........", "id": "Setuju, saya orang yang reflektif." },
  { "en": "Saya tidak suka mengambil risiko finansial. .........", "id": "Setuju, saya lebih suka bermain aman." },
  { "en": "Saya merasa nyaman berbicara di depan audiens. .........", "id": "Setuju, saya suka berbagi pengetahuan." },
  { "en": "Saya adalah orang yang sangat terstruktur. .........", "id": "Setuju, saya suka mengikuti rencana." },
  { "en": "Saya tidak suka menjadi pusat gosip. .........", "id": "Setuju, saya menjaga nama baik." },
  { "en": "Saya jarang merasa pesimis. .........", "id": "Setuju, saya selalu melihat harapan." },
  { "en": "Saya suka pekerjaan yang memungkinkan saya bepergian. .........", "id": "Setuju, saya suka melihat tempat baru." },
  { "en": "Saya lebih suka memiliki sedikit teman dekat. .........", "id": "Setuju, kualitas mengalahkan kuantitas." },
  { "en": "Saya adalah orang yang sangat tepat janji. .........", "id": "Setuju, saya menghargai komitmen." },
  { "en": "Saya tidak suka diperintah. .........", "id": "Tidak setuju, saya menghormati otoritas." },
  { "en": "Saya merasa sulit untuk rileks sepenuhnya. .........", "id": "Setuju, pikiran saya selalu aktif." },
  { "en": "Saya suka mencari solusi yang tidak biasa. .........", "id": "Setuju, saya suka berpikir kreatif." },
  { "en": "Saya tidak suka menjadi bagian dari keramaian. .........", "id": "Setuju, saya lebih suka ketenangan." },
  { "en": "Saya adalah orang yang sangat berorientasi pada hasil. .........", "id": "Setuju, tujuan akhir itu penting." },
  { "en": "Saya mencoba untuk tidak menghakimi pilihan orang lain. .........", "id": "Setuju, setiap orang berhak memilih." },
  { "en": "Saya adalah orang yang sangat tabah menghadapi masalah. .........", "id": "Setuju, saya tidak mudah hancur." },
  { "en": "Saya suka membaca berbagai jenis genre buku. .........", "id": "Setuju, saya suka perspektif yang berbeda." },
  { "en": "Saya merasa sulit untuk menolak ajakan teman. .........", "id": "Setuju, saya tidak ingin mengecewakan." },
  { "en": "Saya adalah orang yang sangat konsisten. .........", "id": "Setuju, tindakan saya sesuai perkataan." },
  { "en": "Saya tidak suka ketidakpastian dalam hidup. .........", "id": "Setuju, saya menyukai rencana yang jelas." },
  { "en": "Saya mudah merasa bosan. .........", "id": "Tidak setuju, saya selalu menemukan kesibukan." },
  { "en": "Saya suka membantu orang lain mencapai tujuan mereka. .........", "id": "Setuju, kesuksesan bersama lebih baik." },
  { "en": "Saya adalah orang yang sangat mandiri. .........", "id": "Setuju, saya bisa diandalkan sendiri." },
  { "en": "Saya tidak suka jika ada yang menyentuh barang saya. .........", "id": "Setuju, saya menjaga barang pribadi." },
  { "en": "Saya jarang merasa iri hati. .........", "id": "Setuju, saya fokus pada jalan saya." },
  { "en": "Saya suka merencanakan acara atau pertemuan. .........", "id": "Setuju, saya menikmati prosesnya." },
  { "en": "Saya lebih suka bekerja dengan tangan saya sendiri. .........", "id": "Setuju, saya menikmati membuat sesuatu." },
  { "en": "Saya adalah orang yang sangat jujur, kadang terlalu jujur. .........", "id": "Setuju, saya sulit berbohong." },
  { "en": "Saya tidak suka menjadi orang terakhir yang tahu. .........", "id": "Setuju, saya suka tetap terinformasi." },
  { "en": "Saya adalah orang yang sangat tenang dalam krisis. .........", "id": "Setuju, saya bisa berpikir jernih." },
  { "en": "Saya suka belajar tentang budaya lain. .........", "id": "Setuju, dunia ini sangat beragam." },
  { "en": "Saya lebih suka liburan aktif daripada santai. .........", "id": "Setuju, saya suka tantangan fisik." },
  { "en": "Saya adalah orang yang sangat telaten dan sabar. .........", "id": "Setuju, saya menikmati proses yang lambat." },
  { "en": "Saya tidak suka membuang-buang uang. .........", "id": "Setuju, saya sangat hemat." },
  { "en": "Saya mudah merasa khawatir akan keselamatan orang lain. .........", "id": "Setuju, saya sangat peduli." },
  { "en": "Saya suka tantangan baru di tempat kerja. .........", "id": "Setuju, itu membuat saya berkembang." },
  { "en": "Saya tidak suka berada di sekitar orang negatif. .........", "id": "Setuju, itu menguras energi saya." },
  { "en": "Saya adalah orang yang sangat setia. .........", "id": "Setuju, baik pada pasangan maupun teman." },
  { "en": "Saya tidak suka jika rutinitas saya diganggu. .........", "id": "Setuju, saya suka konsistensi." },
  { "en": "Saya merasa paling bahagia saat .........", "id": "Dikelilingi oleh orang yang saya cintai." },
  { "en": "Saya suka memecahkan masalah yang logis. .........", "id": "Setuju, saya menikmati menggunakan akal." },
  { "en": "Saya lebih suka menulis daripada berbicara. .........", "id": "Setuju, saya bisa menyusun kata lebih baik." },
  { "en": "Saya adalah orang yang sangat efisien. .........", "id": "Setuju, saya mencari cara tercepat." },
  { "en": "Saya tidak suka menjadi pusat konflik. .........", "id": "Setuju, saya adalah pembawa damai." },
  { "en": "Saya merasa sulit untuk meminta maaf. .........", "id": "Tidak setuju, saya mengakui kesalahan." },
  { "en": "Saya suka mempelajari keterampilan baru. .........", "id": "Setuju, saya suka terus belajar." },
  { "en": "Saya tidak suka berada di tempat yang ramai. .........", "id": "Setuju, saya mudah merasa pusing." },
  { "en": "Saya adalah orang yang sangat ambisius. .........", "id": "Setuju, saya punya tujuan besar." },
  { "en": "Saya tidak suka menonjolkan diri. .........", "id": "Setuju, saya lebih suka rendah hati." },
  { "en": "Saya jarang merasa sedih. .........", "id": "Tidak setuju, sedih adalah emosi wajar." },
  { "en": "Saya suka mencoba hal-hal yang belum pernah saya lakukan. .........", "id": "Setuju, hidup adalah petualangan." },
  { "en": "Saya lebih suka bekerja di lingkungan yang terstruktur. .........", "id": "Setuju, saya butuh kejelasan." },
  { "en": "Saya adalah orang yang sangat berhati-hati. .........", "id": "Setuju, saya selalu waspada." },
  { "en": "Saya tidak suka membiarkan masalah berlarut-larut. .........", "id": "Setuju, saya suka penyelesaian cepat." },
  { "en": "Saya merasa mudah lelah. .........", "id": "Tidak setuju, saya punya banyak energi." },
  { "en": "Saya suka memikirkan konsep-konsep filosofis. .........", "id": "Setuju, saya suka berpikir mendalam." },
  { "en": "Saya tidak suka berbicara tentang perasaan saya. .........", "id": "Setuju, saya adalah orang yang privat." },
  { "en": "Saya adalah orang yang sangat bisa diandalkan. .........", "id": "Setuju, saya selalu menepati janji." },
  { "en": "Saya tidak suka dikejar tenggat waktu. .........", "id": "Setuju, itu membuat saya stres." },
  { "en": "Saya merasa puas ketika membantu orang lain. .........", "id": "Setuju, itu adalah kebahagiaan." },
  { "en": "Saya adalah orang yang sangat terorganisir. .........", "id": "Setuju, semua ada tempatnya." },
  { "en": "Saya suka mengikuti perkembangan mode. .........", "id": "Tidak setuju, saya punya gaya sendiri." },
  { "en": "Saya adalah orang yang sangat percaya diri. .........", "id": "Setuju, saya yakin dengan kemampuan saya." },
  { "en": "Saya tidak suka membuang waktu. .........", "id": "Setuju, saya sangat menghargai waktu." },
  { "en": "Saya adalah orang yang sangat ramah. .........", "id": "Setuju, saya mudah tersenyum." },
  { "en": "Saya jarang mengubah pendapat saya. .........", "id": "Tidak setuju, saya terbuka untuk berubah." },
  { "en": "Saya suka melakukan riset sebelum membuat keputusan. .........", "id": "Setuju, saya butuh informasi lengkap." },
  { "en": "Saya adalah orang yang sangat mudah bergaul. .........", "id": "Setuju, saya bisa masuk ke lingkungan manapun." },
  { "en": "Saya tidak suka memikirkan hal-hal yang rumit. .........", "id": "Tidak setuju, saya suka tantangan." },
  { "en": "Saya adalah orang yang sangat menghargai seni. .........", "id": "Setuju, seni itu memperkaya jiwa." },
  { "en": "Saya lebih suka merencanakan secara spontan. .........", "id": "Setuju, saya suka fleksibilitas." },
  { "en": "Ketika diberi tanggung jawab, saya merasa .........", "id": "Tertantang dan bersemangat." },
  { "en": "Saya tidak suka jika orang lain ikut campur. .........", "id": "Setuju, saya suka bekerja mandiri." },
  { "en": "Saya merasa sulit untuk rileks saat liburan. .........", "id": "Tidak setuju, saya bisa menikmati waktu luang." },
  { "en": "Saya percaya pada kekuatan pikiran positif. .........", "id": "Setuju, pikiran membentuk realitas." },
  { "en": "Saya lebih suka mendiskusikan perasaan saya. .........", "id": "Setuju, itu membuat saya lega." },
  { "en": "Saya adalah orang yang sangat praktis. .........", "id": "Setuju, saya fokus pada solusi nyata." },
  { "en": "Saya tidak suka menjadi bagian dari kompetisi. .........", "id": "Setuju, saya lebih suka kolaborasi." },
  { "en": "Saya merasa cemas jika tidak memeriksa ponsel. .........", "id": "Tidak setuju, saya bisa lepas dari gawai." },
  { "en": "Saya suka membaca biografi orang sukses. .........", "id": "Setuju, itu sangat menginspirasi." },
  { "en": "Saya tidak suka obrolan ringan yang panjang. .........", "id": "Setuju, saya lebih suka percakapan bermakna." },
  { "en": "Saya adalah orang yang sangat berhati-hati. .........", "id": "Setuju, saya memikirkan setiap langkah." },
  { "en": "Saya tidak keberatan mengakui kekurangan saya. .........", "id": "Setuju, itu adalah bagian dari diri." },
  { "en": "Saya sering merasa kewalahan oleh tugas. .........", "id": "Tidak setuju, saya bisa mengatur prioritas." },
  { "en": "Saya tertarik pada gagasan-gagasan futuristik. .........", "id": "Setuju, saya suka membayangkan masa depan." },
  { "en": "Saya lebih suka lingkungan kerja yang tenang. .........", "id": "Setuju, kebisingan mengganggu konsentrasi." },
  { "en": "Saya adalah orang yang menepati janji. .........", "id": "Setuju, kata-kata saya bisa dipegang." },
  { "en": "Saya tidak suka jika orang lain menyentuh barang saya. .........", "id": "Setuju, saya menjaga barang pribadi." },
  { "en": "Saya jarang merasa iri pada orang lain. .........", "id": "Setuju, saya fokus pada perkembangan diri." },
  { "en": "Saya suka membuat sesuatu dari awal. .........", "id": "Setuju, saya menikmati proses kreatif." },
  { "en": "Saya merasa lebih nyaman dalam kelompok kecil. .........", "id": "Setuju, interaksi lebih mendalam." },
  { "en": "Saya adalah orang yang sangat terorganisir. .........", "id": "Setuju, semuanya ada pada tempatnya." },
  { "en": "Saya mencoba memahami sudut pandang yang berbeda. .........", "id": "Setuju, saya berpikiran terbuka." },
  { "en": "Saya jarang merasa stres. .........", "id": "Setuju, saya punya manajemen stres yang baik." },
  { "en": "Saya lebih suka seni yang mudah dimengerti. .........", "id": "Setuju, saya suka pesan yang jelas." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya lebih nyaman di balik layar." },
  { "en": "Saya adalah orang yang sangat persisten. .........", "id": "Setuju, saya tidak mudah menyerah." },
  { "en": "Saya tidak suka menghakimi orang lain. .........", "id": "Setuju, setiap orang punya perjuangan." },
  { "en": "Saya adalah orang yang sangat optimis. .........", "id": "Setuju, saya selalu melihat sisi terang." },
  { "en": "Saya suka pekerjaan yang melibatkan pemecahan masalah. .........", "id": "Setuju, itu menantang pikiran saya." },
  { "en": "Saya lebih suka menghabiskan waktu sendirian. .........", "id": "Setuju, saya butuh waktu untuk diri sendiri." },
  { "en": "Saya adalah orang yang sangat disiplin. .........", "id": "Setuju, saya mengikuti aturan yang saya buat." },
  { "en": "Saya tidak suka perubahan rencana yang mendadak. .........", "id": "Setuju, itu mengganggu ritme saya." },
  { "en": "Saya merasa bahagia ketika orang di sekitar saya bahagia. .........", "id": "Setuju, kebahagiaan itu menular." },
  { "en": "Saya suka mempelajari hal-hal baru. .........", "id": "Setuju, saya adalah pembelajar seumur hidup." },
  { "en": "Saya adalah orang yang sangat blak-blakan. .........", "id": "Setuju, saya mengatakan apa adanya." },
  { "en": "Saya tidak suka pekerjaan di belakang meja. .........", "id": "Setuju, saya suka pekerjaan lapangan." },
  { "en": "Saya merasa sulit untuk mengatakan 'tidak'. .........", "id": "Setuju, saya tidak ingin mengecewakan." },
  { "en": "Saya jarang merasa cemas tentang masa depan. .........", "id": "Setuju, saya fokus pada saat ini." },
  { "en": "Saya suka melakukan hal-hal secara spontan. .........", "id": "Setuju, itu membuat hidup lebih seru." },
  { "en": "Saya lebih suka bekerja dalam tim daripada sendirian. .........", "id": "Setuju, kolaborasi menghasilkan ide lebih baik." },
  { "en": "Saya adalah orang yang sangat teliti. .........", "id": "Setuju, saya memperhatikan setiap detail." },
  { "en": "Saya tidak suka membuang-buang waktu. .........", "id": "Setuju, waktu adalah sumber daya berharga." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya tidak mudah panik." },
  { "en": "Saya suka merenungkan ide-ide yang kompleks. .........", "id": "Setuju, saya menikmati tantangan intelektual." },
  { "en": "Saya merasa nyaman mengekspresikan diri. .........", "id": "Setuju, saya tidak takut menjadi diri sendiri." },
  { "en": "Saya tidak suka meninggalkan pekerjaan belum selesai. .........", "id": "Setuju, saya harus menyelesaikannya." },
  { "en": "Saya adalah orang yang sangat sopan. .........", "id": "Setuju, sopan santun itu penting." },
  { "en": "Saya jarang merasa kesal atau marah. .........", "id": "Setuju, saya bisa mengendalikan emosi." },
  { "en": "Saya suka rutinitas yang terstruktur. .........", "id": "Setuju, itu memberi saya kepastian." },
  { "en": "Saya adalah orang yang sangat mudah bergaul. .........", "id": "Setuju, saya bisa beradaptasi di mana saja." },
  { "en": "Saya tidak suka memikirkan kegagalan masa lalu. .........", "id": "Setuju, saya fokus pada masa depan." },
  { "en": "Saya adalah orang yang sangat bisa dipercaya. .........", "id": "Setuju, saya menjaga kepercayaan orang." },
  { "en": "Saya tidak suka berada dalam situasi konflik. .........", "id": "Setuju, saya lebih suka kedamaian." },
  { "en": "Saya adalah orang yang sangat ceria. .........", "id": "Setuju, saya suka menyebarkan energi positif." },
  { "en": "Saya suka mencoba berbagai jenis makanan. .........", "id": "Setuju, saya suka petualangan kuliner." },
  { "en": "Saya lebih suka percakapan yang mendalam. .........", "id": "Setuju, obrolan ringan kadang membosankan." },
  { "en": "Saya adalah orang yang sangat terencana. .........", "id": "Setuju, saya selalu punya rencana cadangan." },
  { "en": "Saya tidak suka mengecewakan harapan orang tua. .........", "id": "Setuju, saya ingin membuat mereka bangga." },
  { "en": "Saya adalah orang yang sangat sabar. .........", "id": "Setuju, saya bisa menunggu." },
  { "en": "Saya suka pekerjaan yang melibatkan kreativitas. .........", "id": "Setuju, saya suka menghasilkan ide baru." },
  { "en": "Saya merasa tidak nyaman dengan keheningan yang lama. .........", "id": "Setuju, saya lebih suka ada suara." },
  { "en": "Saya adalah orang yang sangat efisien. .........", "id": "Setuju, saya mencari cara paling efektif." },
  { "en": "Saya tidak suka meminta pertolongan. .........", "id": "Setuju, saya berusaha mandiri." },
  { "en": "Saya jarang merasa cemburu dalam hubungan. .........", "id": "Setuju, saya punya kepercayaan penuh." },
  { "en": "Saya suka hal-hal yang berkaitan dengan sejarah. .........", "id": "Setuju, belajar dari masa lalu itu penting." },
  { "en": "Saya lebih suka bekerja di lingkungan yang suportif. .........", "id": "Setuju, itu meningkatkan produktivitas." },
  { "en": "Saya adalah orang yang sangat konsisten. .........", "id": "Setuju, tindakan dan nilai saya sejalan." },
  { "en": "Saya tidak suka ketidakjelasan atau ambiguitas. .........", "id": "Setuju, saya butuh instruksi yang jelas." },
  { "en": "Saya adalah orang yang sangat berempati. .........", "id": "Setuju, saya bisa merasakan perasaan orang." },
  { "en": "Saya suka membaca berita setiap hari. .........", "id": "Setuju, saya suka tetap terinformasi." },
  { "en": "Saya lebih suka menjadi pendengar yang aktif. .........", "id": "Setuju, saya menyerap informasi dengan baik." },
  { "en": "Saya adalah orang yang sangat berhati-hati dalam mengambil keputusan. .........", "id": "Setuju, saya mempertimbangkan semua opsi." },
  { "en": "Saya tidak suka menjadi pusat perhatian dalam sebuah argumen. .........", "id": "Setuju, saya menghindari drama." },
  { "en": "Saya adalah orang yang sangat bersemangat tentang hobi saya. .........", "id": "Setuju, hobi adalah gairah saya." },
  { "en": "Saya suka tantangan yang menguji batas kemampuan saya. .........", "id": "Setuju, itu cara saya untuk tumbuh." },
  { "en": "Saya lebih suka lingkungan yang stabil dan aman. .........", "id": "Setuju, saya tidak suka risiko." },
  { "en": "Saya adalah orang yang sangat metodis dalam bekerja. .........", "id": "Setuju, saya mengikuti langkah demi langkah." },
  { "en": "Saya tidak suka jika orang lain mengkritik teman saya. .........", "id": "Setuju, saya sangat protektif." },
  { "en": "Saya jarang merasa bosan jika sendirian. .........", "id": "Setuju, saya menikmati waktu berkualitas." },
  { "en": "Saya suka pekerjaan yang memiliki hasil nyata. .........", "id": "Setuju, saya suka melihat dampaknya." },
  { "en": "Saya lebih suka berbicara langsung ke intinya. .........", "id": "Setuju, saya tidak suka bertele-tele." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab atas tindakan saya. .........", "id": "Setuju, saya mengakui kesalahan saya." },
  { "en": "Saya tidak suka berada di sekitar orang yang pesimis. .........", "id": "Setuju, itu mempengaruhi suasana hati." },
  { "en": "Saya adalah orang yang sangat tangguh secara mental. .........", "id": "Setuju, saya tidak mudah jatuh." },
  { "en": "Saya suka memikirkan solusi untuk masalah global. .........", "id": "Setuju, saya peduli pada dunia." },
  { "en": "Saya lebih suka berinteraksi dengan sedikit orang. .........", "id": "Setuju, saya menghargai percakapan mendalam." },
  { "en": "Saya adalah orang yang sangat berdedikasi. .........", "id": "Setuju, saya memberikan yang terbaik." },
  { "en": "Saya tidak suka jika orang lain tidak menepati janji. .........", "id": "Setuju, saya sangat menghargai komitmen." },
  { "en": "Saya adalah orang yang sangat stabil. .........", "id": "Setuju, saya tidak mudah goyah." },
  { "en": "Saya suka membongkar barang untuk melihat cara kerjanya. .........", "id": "Setuju, saya punya rasa ingin tahu teknis." },
  { "en": "Saya lebih suka bekerja dengan jadwal yang fleksibel. .........", "id": "Setuju, saya suka kebebasan mengatur waktu." },
  { "en": "Saya adalah orang yang sangat berprinsip. .........", "id": "Setuju, saya berpegang pada nilai-nilai saya." },
  { "en": "Saya tidak suka berada dalam utang. .........", "id": "Setuju, saya suka kebebasan finansial." },
  { "en": "Saya adalah orang yang sangat mudah bergaul. .........", "id": "Setuju, saya bisa memulai percakapan." },
  { "en": "Saya suka membuat orang lain merasa nyaman. .........", "id": "Setuju, saya adalah tuan rumah yang baik." },
  { "en": "Saya adalah orang yang sangat visioner. .........", "id": "Setuju, saya memikirkan gambaran besar." },
  { "en": "Saya lebih suka belajar dari buku daripada video. .........", "id": "Setuju, saya lebih fokus saat membaca." },
  { "en": "Ketika menghadapi masalah, langkah pertama saya adalah .........", "id": "Menganalisis situasinya terlebih dahulu." },
  { "en": "Saya tidak suka jika orang lain mengatur hidup saya. .........", "id": "Setuju, saya menghargai kebebasan pribadi." },
  { "en": "Saya sering merasa perlu untuk membuktikan diri. .........", "id": "Tidak setuju, saya nyaman dengan diri sendiri." },
  { "en": "Saya percaya bahwa seni bisa mengubah dunia. .........", "id": "Setuju, seni punya kekuatan besar." },
  { "en": "Saya lebih suka memberi kritik secara langsung. .........", "id": "Setuju, agar jelas dan tidak salah paham." },
  { "en": "Saya adalah orang yang sangat bersemangat. .........", "id": "Setuju, saya punya banyak energi positif." },
  { "en": "Saya tidak suka membuang barang lama. .........", "id": "Setuju, saya punya ikatan emosional." },
  { "en": "Saya merasa cemas jika tidak punya rencana. .........", "id": "Setuju, saya butuh struktur yang jelas." },
  { "en": "Saya suka mendengarkan podcast tentang topik baru. .........", "id": "Setuju, saya suka memperluas pengetahuan." },
  { "en": "Saya tidak suka menjadi orang yang memulai percakapan. .........", "id": "Setuju, saya lebih suka merespons." },
  { "en": "Saya adalah orang yang sangat fokus pada tujuan. .........", "id": "Setuju, saya tidak mudah teralihkan." },
  { "en": "Saya tidak keberatan berbagi makanan saya. .........", "id": "Setuju, berbagi itu menyenangkan." },
  { "en": "Saya jarang memikirkan apa yang orang lain pikirkan. .........", "id": "Setuju, saya fokus pada diri sendiri." },
  { "en": "Saya tertarik pada konsep spiritualitas. .........", "id": "Setuju, saya mencari makna lebih dalam." },
  { "en": "Saya lebih suka bekerja dengan data dan angka. .........", "id": "Setuju, saya menyukai objektivitas." },
  { "en": "Saya adalah orang yang selalu menepati janji. .........", "id": "Setuju, integritas adalah segalanya." },
  { "en": "Saya tidak suka jika seseorang terlalu banyak bertanya. .........", "id": "Tidak setuju, saya suka rasa ingin tahu." },
  { "en": "Saya jarang merasa bersalah. .........", "id": "Tidak setuju, saya sering merefleksikan tindakan." },
  { "en": "Saya suka membuat sesuatu dengan tangan saya. .........", "id": "Setuju, itu sangat memuaskan." },
  { "en": "Saya merasa lebih berenergi setelah berolahraga. .........", "id": "Setuju, itu meningkatkan semangat saya." },
  { "en": "Saya adalah orang yang sangat rapi. .........", "id": "Setuju, saya tidak tahan berantakan." },
  { "en": "Saya mencoba untuk tidak membuat asumsi tentang orang. .........", "id": "Setuju, saya lebih suka bertanya." },
  { "en": "Saya jarang merasa gugup sebelum presentasi. .........", "id": "Setuju, saya sudah mempersiapkan diri." },
  { "en": "Saya lebih suka film dokumenter daripada fiksi. .........", "id": "Setuju, saya suka belajar dari kenyataan." },
  { "en": "Saya tidak suka berada dalam situasi formal. .........", "id": "Setuju, saya lebih suka suasana santai." },
  { "en": "Saya adalah orang yang sangat gigih. .........", "id": "Setuju, saya tidak akan berhenti mencoba." },
  { "en": "Saya tidak suka jika orang lain menyela pembicaraan saya. .........", "id": "Setuju, saya menghargai kesempatan berbicara." },
  { "en": "Saya adalah orang yang sangat stabil. .........", "id": "Setuju, saya tidak mudah terombang-ambing." },
  { "en": "Saya suka memecahkan teka-teki logika. .........", "id": "Setuju, itu melatih pikiran saya." },
  { "en": "Saya lebih suka menghabiskan malam Jumat di rumah. .........", "id": "Setuju, saya butuh istirahat." },
  { "en": "Saya adalah orang yang sangat berorientasi pada detail. .........", "id": "Setuju, saya tidak melewatkan hal kecil." },
  { "en": "Saya tidak suka menonjol dalam sebuah kelompok. .........", "id": "Setuju, saya lebih nyaman sebagai bagian tim." },
  { "en": "Saya jarang merasa kewalahan. .........", "id": "Setuju, saya bisa mengelola beban kerja." },
  { "en": "Saya suka mencoba rute baru saat bepergian. .........", "id": "Setuju, saya suka eksplorasi." },
  { "en": "Saya adalah orang yang sangat ekspresif. .........", "id": "Setuju, emosi saya mudah terlihat." },
  { "en": "Saya tidak suka pekerjaan yang tidak memiliki akhir yang jelas. .........", "id": "Setuju, saya butuh penyelesaian." },
  { "en": "Saya adalah orang yang sangat murah hati. .........", "id": "Setuju, saya suka memberi." },
  { "en": "Saya jarang merasa cemas atau khawatir. .........", "id": "Setuju, saya memiliki pandangan positif." },
  { "en": "Saya suka melakukan sesuatu yang berbeda dari biasanya. .........", "id": "Setuju, saya tidak suka kebosanan." },
  { "en": "Saya lebih suka bekerja di lingkungan yang dinamis. .........", "id": "Setuju, saya suka tantangan yang berubah." },
  { "en": "Saya adalah orang yang sangat teliti dalam keuangan. .........", "id": "Setuju, saya membuat anggaran." },
  { "en": "Saya tidak suka mengkritik pekerjaan orang lain. .........", "id": "Setuju, saya lebih suka memberi masukan." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya jarang merasa tertekan." },
  { "en": "Saya suka mempelajari tentang penemuan ilmiah baru. .........", "id": "Setuju, itu sangat menarik." },
  { "en": "Saya merasa nyaman memulai percakapan dengan orang asing. .........", "id": "Setuju, saya suka bertemu orang baru." },
  { "en": "Saya tidak suka jika ada barang yang tidak pada tempatnya. .........", "id": "Setuju, saya suka keteraturan." },
  { "en": "Saya adalah orang yang sangat setia. .........", "id": "Setuju, saya sangat menghargai hubungan." },
  { "en": "Saya jarang merasa pesimis. .........", "id": "Setuju, saya selalu mencari sisi baiknya." },
  { "en": "Saya suka menghadiri konser musik live. .........", "id": "Setuju, energinya luar biasa." },
  { "en": "Saya lebih suka berdiskusi dalam kelompok kecil. .........", "id": "Setuju, semua orang bisa berbicara." },
  { "en": "Saya adalah orang yang sangat berhati-hati. .........", "id": "Setuju, saya menghindari risiko yang tidak perlu." },
  { "en": "Saya tidak suka menjadi pusat perhatian. .........", "id": "Setuju, saya merasa malu." },
  { "en": "Saya adalah orang yang sangat sabar dalam antrean. .........", "id": "Setuju, saya bisa menunggu giliran." },
  { "en": "Saya suka membaca buku-buku tebal. .........", "id": "Setuju, saya suka cerita yang mendalam." },
  { "en": "Saya lebih suka liburan yang menenangkan. .........", "id": "Setuju, untuk melepaskan stres." },
  { "en": "Saya adalah orang yang sangat efisien. .........", "id": "Setuju, saya tidak suka pemborosan." },
  { "en": "Saya tidak suka jika orang lain membuat keputusan untuk saya. .........", "id": "Setuju, saya ingin kontrol sendiri." },
  { "en": "Saya jarang merasa marah pada orang lain. .........", "id": "Setuju, saya mencoba untuk memahami." },
  { "en": "Saya suka mencoba resep baru di dapur. .........", "id": "Setuju, saya suka bereksperimen." },
  { "en": "Saya merasa lebih produktif bekerja sendirian. .........", "id": "Setuju, saya tidak mudah terganggu." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab. .........", "id": "Setuju, saya menyelesaikan apa yang saya mulai." },
  { "en": "Saya tidak suka membicarakan masalah pribadi saya. .........", "id": "Setuju, saya adalah orang yang privat." },
  { "en": "Saya jarang merasa terancam oleh perubahan. .........", "id": "Setuju, saya melihatnya sebagai peluang." },
  { "en": "Saya suka melakukan sesuatu yang kreatif. .........", "id": "Setuju, itu cara saya berekspresi." },
  { "en": "Saya lebih suka pekerjaan dengan jam kerja yang teratur. .........", "id": "Setuju, saya suka rutinitas." },
  { "en": "Saya adalah orang yang sangat terorganisir. .........", "id": "Setuju, saya punya sistem untuk semuanya." },
  { "en": "Saya tidak suka mengecewakan orang lain. .........", "id": "Setuju, saya berusaha memenuhi harapan." },
  { "en": "Saya adalah orang yang sangat positif. .........", "id": "Setuju, saya percaya pada hal baik." },
  { "en": "Saya suka kejutan yang menyenangkan. .........", "id": "Setuju, itu membuat hari lebih cerah." },
  { "en": "Saya lebih suka mendengarkan daripada menjadi pembicara utama. .........", "id": "Setuju, saya adalah pengamat yang baik." },
  { "en": "Saya adalah orang yang sangat dapat diandalkan. .........", "id": "Setuju, saya selalu ada saat dibutuhkan." },
  { "en": "Saya tidak suka berada di sekitar orang yang banyak mengeluh. .........", "id": "Setuju, itu sangat negatif." },
  { "en": "Saya adalah orang yang sangat tangguh. .........", "id": "Setuju, saya bisa mengatasi kesulitan." },
  { "en": "Saya suka permainan yang membutuhkan strategi. .........", "id": "Setuju, saya suka berpikir beberapa langkah ke depan." },
  { "en": "Saya lebih suka komunikasi yang jelas dan langsung. .........", "id": "Setuju, saya tidak suka kode-kode." },
  { "en": "Saya adalah orang yang sangat teliti. .........", "id": "Setuju, saya memeriksa pekerjaan saya dua kali." },
  { "en": "Saya tidak suka menjadi beban bagi keluarga saya. .........", "id": "Setuju, saya ingin mandiri." },
  { "en": "Saya jarang merasa sedih untuk waktu yang lama. .........", "id": "Setuju, saya cepat pulih." },
  { "en": "Saya suka merencanakan kegiatan untuk akhir pekan. .........", "id": "Setuju, saya suka memaksimalkan waktu." },
  { "en": "Saya lebih suka pekerjaan yang memberi saya otonomi. .........", "id": "Setuju, saya suka kebebasan dalam bekerja." },
  { "en": "Saya adalah orang yang sangat peduli. .........", "id": "Setuju, saya memperhatikan kesejahteraan orang lain." },
  { "en": "Saya tidak suka berada di tengah-tengah argumen. .........", "id": "Setuju, saya lebih suka mencari jalan tengah." },
  { "en": "Saya jarang merasa bosan. .........", "id": "Setuju, saya selalu menemukan sesuatu untuk dilakukan." },
  { "en": "Saya suka melakukan hal-hal dengan cara yang benar. .........", "id": "Setuju, saya mengikuti prosedur." },
  { "en": "Saya lebih suka bekerja dengan orang lain. .........", "id": "Setuju, saya adalah pemain tim." },
  { "en": "Saya adalah orang yang sangat teratur. .........", "id":- "Setuju, saya tidak suka kekacauan." },
  { "en": "Saya tidak suka jika orang lain tidak jujur. .........", "id": "Setuju, kejujuran adalah hal utama." },
  { "en": "Saya adalah orang yang sangat tenang. .........", "id": "Setuju, saya tidak mudah stres." },
  { "en": "Saya suka belajar tentang berbagai topik. .........", "id": "Setuju, saya memiliki banyak minat." },
  { "en": "Saya lebih suka hubungan yang mendalam dan bermakna. .........", "id": "Setuju, kualitas lebih penting." },
  { "en": "Saya adalah orang yang sangat disiplin. .........", "id": "Setuju, saya bisa mengendalikan diri." },
  { "en": "Saya tidak suka jika rencana saya berubah. .........", "id": "Setuju, saya suka kepastian." },
  { "en": "Saya adalah orang yang sangat suportif. .........", "id": "Setuju, saya selalu mendukung teman-teman saya." },
  { "en": "Saya suka tantangan yang membuat saya berpikir. .........", "id": "Setuju, saya suka memecahkan masalah." },
  { "en": "Saya lebih suka lingkungan kerja yang formal. .........", "id": "Tidak setuju, saya suka suasana santai." },
  { "en": "Saya adalah orang yang sangat berprinsip. .........", "id": "Setuju, saya memiliki nilai-nilai yang kuat." },
  { "en": "Saya tidak suka memikirkan hal yang sama berulang-ulang. .........", "id": "Setuju, saya cepat move on." },
  { "en": "Saya adalah orang yang sangat ceria. .........", "id": "Setuju, saya mudah tertawa." },
  { "en": "Saya suka melakukan sesuatu yang bermanfaat bagi masyarakat. .........", "id": "Setuju, saya suka berkontribusi." },
  { "en": "Saya lebih suka memikirkan masa kini daripada masa depan. .........", "id": "Setuju, saya fokus pada saat ini." },
  { "en": "Ketika saya membuat kesalahan, saya .........", "id": "Segera mengakuinya dan memperbaikinya." },
  { "en": "Saya tidak suka jika orang lain menyalin pekerjaan saya. .........", "id": "Setuju, saya menghargai orisinalitas." },
  { "en": "Saya sering merasa perlu menyendiri untuk berpikir. .........", "id": "Setuju, saya butuh ketenangan." },
  { "en": "Saya percaya bahwa batasan ada untuk diuji. .........", "id": "Tidak setuju, saya menghormati aturan." },
  { "en": "Saya lebih suka memberi kritik yang membangun. .........", "id": "Setuju, tujuannya untuk perbaikan." },
  { "en": "Saya adalah orang yang sangat mudah beradaptasi. .........", "id": "Setuju, saya fleksibel dengan perubahan." },
  { "en": "Saya tidak suka membuang-buang makanan. .........", "id": "Setuju, saya menghargai sumber daya." },
  { "en": "Saya merasa cemas saat berada di tempat tinggi. .........", "id": "Setuju, saya memiliki sedikit fobia." },
  { "en": "Saya suka menonton film dari berbagai negara. .........", "id": "Setuju, saya suka perspektif budaya beda." },
  { "en": "Saya tidak suka menjadi orang yang memulai perdebatan. .........", "id": "Setuju, saya menghindari konfrontasi." },
  { "en": "Saya adalah orang yang sangat berorientasi pada proses. .........", "id": "Setuju, proses yang baik hasil baik." },
  { "en": "Saya tidak keberatan bekerja lembur jika diperlukan. .........", "id": "Setuju, saya punya dedikasi tinggi." },
  { "en": "Saya jarang merasa iri dengan kehidupan orang lain. .........", "id": "Setuju, saya fokus pada jalan hidup saya." },
  { "en": "Saya tertarik pada cara kerja pikiran manusia. .........", "id": "Setuju, psikologi itu menarik." },
  { "en": "Saya lebih suka bekerja di lingkungan yang non-kompetitif. .........", "id": "Setuju, saya menyukai kerja sama tim." },
  { "en": "Saya adalah orang yang selalu menepati perkataan. .........", "id": "Setuju, integritas itu sangat penting." },
  { "en": "Saya tidak suka jika seseorang berbicara terlalu keras. .........", "id": "Setuju, saya menyukai ketenangan." },
  { "en": "Saya jarang merasa bersalah tentang keputusan masa lalu. .........", "id": "Setuju, saya belajar dan move on." },
  { "en": "Saya suka membuat kerajinan tangan. .........", "id": "Setuju, itu melatih kreativitas saya." },
  { "en": "Saya merasa lebih bersemangat setelah minum kopi. .........", "id": "Setuju, itu meningkatkan energi saya." },
  { "en": "Saya adalah orang yang sangat teratur dalam keuangan. .........", "id": "Setuju, saya mencatat pengeluaran." },
  { "en": "Saya mencoba untuk tidak memotong pembicaraan orang. .........", "id": "Setuju, saya adalah pendengar yang sopan." },
  { "en": "Saya jarang merasa gugup saat berbicara dengan atasan. .........", "id": "Setuju, saya merasa percaya diri." },
  { "en": "Saya lebih suka buku dengan akhir yang tak terduga. .........", "id": "Setuju, saya menyukai kejutan." },
  { "en": "Saya tidak suka berada dalam kelompok yang terlalu besar. .........", "id": "Setuju, saya lebih nyaman di kelompok kecil." },
  { "en": "Saya adalah orang yang sangat gigih mencapai impian. .........", "id": "Setuju, saya tidak pernah menyerah." },
  { "en": "Saya tidak suka jika orang lain tidak menghargai waktu saya. .........", "id": "Setuju, waktu sangat berharga." },
  { "en": "Saya adalah orang yang sangat stabil emosinya. .........", "id": "Setuju, saya tidak mudah marah atau sedih." },
  { "en": "Saya suka memecahkan masalah yang kompleks. .........", "id": "Setuju, itu menantang bagi saya." },
  { "en": "Saya lebih suka menghabiskan waktu berkualitas bersama keluarga. .........", "id": "Setuju, keluarga adalah prioritas." },
  { "en": "Saya adalah orang yang sangat berorientasi pada detail. .........", "id": "Setuju, saya memastikan semua sempurna." },
  { "en": "Saya tidak suka menonjolkan pencapaian saya. .........", "id": "Setuju, saya lebih suka rendah hati." },
  { "en": "Saya jarang merasa kewalahan oleh tanggung jawab. .........", "id": "Setuju, saya bisa mengelolanya dengan baik." },
  { "en": "Saya suka mencoba hal-hal di luar zona nyaman. .........", "id": "Setuju, itu cara untuk berkembang." },
  { "en": "Saya adalah orang yang sangat ekspresif secara non-verbal. .........", "id": "Setuju, gerak tubuh saya berbicara." },
  { "en": "Saya tidak suka pekerjaan yang monoton. .........", "id": "Setuju, saya membutuhkan variasi dan tantangan." },
  { "en": "Saya adalah orang yang sangat dermawan. .........", "id": "Setuju, saya suka berbagi dengan sesama." },
  { "en": "Saya jarang merasa cemas akan penilaian orang. .........", "id": "Setuju, saya percaya pada diri sendiri." },
  { "en": "Saya suka melakukan sesuatu yang berbeda setiap hari. .........", "id": "Setuju, rutinitas itu membosankan." },
  { "en": "Saya lebih suka bekerja di lingkungan yang kreatif. .........", "id": "Setuju, saya suka ide-ide baru." },
  { "en": "Saya adalah orang yang sangat hemat. .........", "id": "Setuju, saya selalu memikirkan pengeluaran." },
  { "en": "Saya tidak suka mengkritik orang di depan umum. .........", "id": "Setuju, saya lebih suka bicara pribadi." },
  { "en": "Saya adalah orang yang sangat tenang di bawah tekanan. .........", "id": "Setuju, saya bisa berpikir jernih." },
  { "en": "Saya suka mempelajari tentang teknologi baru. .........", "id": "Setuju, saya suka mengikuti perkembangan." },
  { "en": "Saya merasa nyaman menjadi diri sendiri. .........", "id": "Setuju, saya tidak perlu berpura-pura." },
  { "en": "Saya tidak suka jika ada barang yang berantakan. .........", "id": "Setuju, saya menyukai kerapian." },
  { "en": "Saya adalah orang yang sangat setia pada pasangan. .........", "id": "Setuju, kesetiaan adalah segalanya." },
  { "en": "Saya jarang merasa pesimis tentang masa depan. .........", "id": "Setuju, saya selalu punya harapan." },
  { "en": "Saya suka menghadiri acara olahraga. .........", "id": "Setuju, saya suka semangat kompetisinya." },
  { "en": "Saya lebih suka berdiskusi untuk mencari solusi. .........", "id": "Setuju, dua kepala lebih baik." },
  { "en": "Saya adalah orang yang sangat berhati-hati dalam bertindak. .........", "id": "Setuju, saya selalu memikirkan risikonya." },
  { "en": "Saya tidak suka menjadi pusat perdebatan. .........", "id": "Setuju, saya lebih suka menjadi penonton." },
  { "en": "Saya adalah orang yang sangat sabar menunggu sesuatu. .........", "id": "Setuju, saya memahami proses butuh waktu." },
  { "en": "Saya suka membaca buku tentang petualangan. .........", "id":
"Setuju, itu membangkitkan imajinasi saya." },
  { "en": "Saya lebih suka liburan yang mendidik. .........", "id": "Setuju, saya suka belajar sambil jalan." },
  { "en": "Saya adalah orang yang sangat efisien dalam mengatur waktu. .........", "id": "Setuju, saya membuat jadwal harian." },
  { "en": "Saya tidak suka jika orang lain membuat keputusan untuk saya. .........", "id": "Setuju, saya ingin memegang kendali." },
  { "en": "Saya jarang marah karena hal-hal sepele. .........", "id": "Setuju, saya memilih untuk tenang." },
  { "en": "Saya suka mencoba makanan jalanan. .........", "id": "Setuju, saya suka petualangan rasa." },
  { "en": "Saya merasa lebih produktif saat mendengarkan musik. .........", "id": "Setuju, itu membantu saya fokus." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab. .........", "id": "Setuju, saya menepati kewajiban saya." },
  { "en": "Saya tidak suka membicarakan kelemahan saya. .........", "id": "Setuju, saya lebih suka menunjukkannya." },
  { "en": "Saya jarang merasa terancam oleh ide-ide baru. .........", "id": "Setuju, saya melihatnya sebagai peluang belajar." },
  { "en": "Saya suka melakukan sesuatu yang kreatif di waktu luang. .........", "id": "Setuju, itu adalah cara saya berekspresi." },
  { "en": "Saya lebih suka pekerjaan dengan hasil yang terukur. .........", "id": "Setuju, saya suka melihat kemajuan." },
  { "en": "Saya adalah orang yang sangat terorganisir. .........", "id": "Setuju, saya merencanakan segalanya." },
  { "en": "Saya tidak suka mengecewakan tim saya. .........", "id": "Setuju, saya adalah pemain tim." },
  { "en": "Saya adalah orang yang sangat positif. .........", "id": "Setuju, saya melihat kebaikan di setiap situasi." },
  { "en": "Saya suka memberikan kejutan kecil untuk teman. .........", "id": "Setuju, itu cara menunjukkan perhatian." },
  { "en": "Saya lebih suka mengamati sebelum bertindak. .........", "id": "Setuju, saya adalah seorang perencana." },
  { "en": "Saya adalah orang yang sangat dapat dipercaya. .........", "id": "Setuju, saya memegang teguh amanah." },
  { "en": "Saya tidak suka berada di sekitar orang yang arogan. .........", "id": "Setuju, kerendahan hati itu penting." },
  { "en": "Saya adalah orang yang sangat tangguh menghadapi kritik. .........", "id": "Setuju, saya menjadikannya sebagai motivasi." },
  { "en": "Saya suka permainan yang menguji pengetahuan umum. .........", "id": "Setuju, saya suka belajar hal baru." },
  { "en": "Saya lebih suka komunikasi yang jujur dan terbuka. .........", "id": "Setuju, saya tidak suka ada rahasia." },
  { "en": "Saya adalah orang yang sangat teliti dalam bekerja. .........", "id": "Setuju, saya memastikan tidak ada kesalahan." },
  { "en": "Saya tidak suka menjadi beban bagi orang tua. .........", "id": "Setuju, saya ingin membuat mereka bangga." },
  { "en": "Saya jarang merasa sedih berkepanjangan. .........", "id": "Setuju, saya bisa mengelola emosi saya." },
  { "en": "Saya suka merencanakan acara sosial. .........", "id": "Setuju, saya menikmati mengorganisir." },
  { "en": "Saya lebih suka pekerjaan yang memberi saya kebebasan. .........", "id": "Setuju, saya suka otonomi dalam bekerja." },
  { "en": "Saya adalah orang yang sangat peduli pada lingkungan. .........", "id": "Setuju, saya melakukan daur ulang." },
  { "en": "Saya tidak suka berada di tengah konflik keluarga. .........", "id": "Setuju, saya lebih suka menjadi pendamai." },
  { "en": "Saya jarang merasa bosan saat sendirian. .........", "id": "Setuju, saya menikmati waktu dengan diri sendiri." },
  { "en": "Saya suka melakukan hal-hal yang benar menurut aturan. .........", "id": "Setuju, saya mengikuti prosedur yang ada." },
  { "en": "Saya lebih suka bekerja dengan orang yang berpengalaman. .........", "id": "Setuju, saya bisa belajar banyak." },
  { "en": "Saya adalah orang yang sangat teratur. .........", "id": "Setuju, saya tidak suka kekacauan." },
  { "en": "Saya tidak suka jika orang lain tidak menepati janji. .........", "id": "Setuju, komitmen adalah hal yang penting." },
  { "en": "Saya adalah orang yang sangat tenang dan stabil. .........", "id": "Setuju, saya tidak mudah terpengaruh." },
  { "en": "Saya suka belajar tentang budaya dan tradisi. .........", "id": "Setuju, itu memperkaya wawasan saya." },
  { "en": "Saya lebih suka hubungan yang tulus dan mendalam. .........", "id": "Setuju, saya tidak suka kepura-puraan." },
  { "en": "Saya adalah orang yang sangat disiplin dalam berolahraga. .........", "id": "Setuju, kesehatan adalah investasi." },
  { "en": "Saya tidak suka jika rencana saya diganggu. .........", "id": "Setuju, saya suka hal yang terencana." },
  { "en": "Saya adalah orang yang sangat suportif terhadap tujuan teman. .........", "id": "Setuju, saya senang melihat mereka berhasil." },
  { "en": "Saya suka tantangan yang membutuhkan pemikiran kritis. .........", "id": "Setuju, saya suka menganalisis masalah." },
  { "en": "Saya lebih suka lingkungan kerja yang santai. .........", "id": "Setuju, saya lebih produktif tanpa tekanan." },
  { "en": "Saya adalah orang yang sangat berprinsip. .........", "id": "Setuju, saya memegang teguh nilai-nilai saya." },
  { "en": "Saya tidak suka memikirkan masalah yang sudah berlalu. .........", "id": "Setuju, saya fokus pada solusi masa depan." },
  { "en": "Saya adalah orang yang sangat ceria dan optimis. .........", "id": "Setuju, saya menyebarkan aura positif." },
  { "en": "Saya suka melakukan sesuatu yang bermanfaat bagi orang lain. .........", "id": "Setuju, saya suka memberi kontribusi." },
  { "en": "Saya lebih suka menyelesaikan satu tugas sebelum memulai yang lain. .........", "id": "Setuju, saya bekerja secara sekuensial." },
  { "en": "Ketika dihadapkan pada aturan baru, saya .........", "id": "Mencoba memahami alasannya terlebih dahulu." },
  { "en": "Saya tidak suka jika orang lain terlalu bergantung pada saya. .........", "id": "Setuju, saya mendorong kemandirian." },
  { "en": "Saya sering merasa perlu menyendiri setelah acara sosial. .........", "id": "Setuju, untuk mengisi kembali energi saya." },
  { "en": "Saya percaya bahwa kegagalan adalah guru terbaik. .........", "id": "Setuju, saya belajar dari setiap kesalahan." },
  { "en": "Saya lebih suka memberikan hadiah buatan sendiri. .........", "id": "Setuju, terasa lebih personal dan tulus." },
  { "en": "Saya adalah orang yang sangat sadar akan lingkungan sekitar. .........", "id": "Setuju, saya peka terhadap detail." },
  { "en": "Saya tidak suka membuang waktu dalam antrean. .........", "id": "Setuju, saya orang yang tidak sabaran." },
  { "en": "Saya merasa cemas jika tidak memegang kendali situasi. .........", "id": "Tidak setuju, saya bisa mengikuti alur." },
  { "en": "Saya suka menonton film dokumenter alam. .........", "id": "Setuju, saya mengagumi keindahan alam." },
  { "en": "Saya tidak suka menjadi orang yang harus memutuskan. .........", "id": "Setuju, saya lebih suka mengikuti." },
  { "en": "Saya adalah orang yang sangat berorientasi pada detail. .........", "id": "Setuju, saya memastikan semuanya akurat." },
  { "en": "Saya tidak keberatan mengubah rutinitas saya untuk orang lain. .........", "id": "Setuju, saya orang yang fleksibel." },
  { "en": "Saya jarang merasa tertekan oleh tenggat waktu. .........", "id": "Setuju, saya bekerja dengan baik di bawah tekanan." },
  { "en": "Saya tertarik pada cara kerja mesin yang kompleks. .........", "id": "Setuju, saya suka bidang teknik." },
  { "en": "Saya lebih suka bekerja di lingkungan yang stabil. .........", "id": "Setuju, saya tidak suka banyak perubahan." },
  { "en": "Saya adalah orang yang selalu menepati janji pada diri sendiri. .........", "id": "Setuju, saya punya disiplin diri." },
  { "en": "Saya tidak suka jika seseorang terlalu banyak bicara. .........", "id": "Setuju, saya menghargai keheningan." },
  { "en": "Saya jarang menyesali keputusan yang telah saya buat. .........", "id": "Setuju, saya yakin dengan pilihan saya." },
  { "en": "Saya suka merancang atau mendekorasi ruangan. .........", "id": "Setuju, saya punya selera estetika." },
  { "en": "Saya merasa lebih waspada di lingkungan baru. .........", "id": "Setuju, saya selalu mengamati sekitar." },
  { "en": "Saya adalah orang yang sangat rapi dan bersih. .........", "id": "Setuju, kebersihan adalah sebagian dari iman." },
  { "en": "Saya mencoba untuk tidak membuat janji yang tidak bisa saya tepati. .........", "id": "Setuju, saya realistis dengan komitmen." },
  { "en": "Saya jarang merasa gugup saat tampil di depan umum. .........", "id": "Setuju, saya menikmati menjadi pusat perhatian." },
  { "en": "Saya lebih suka buku yang memiliki pesan moral. .........", "id": "Setuju, saya suka cerita yang mendidik." },
  { "en": "Saya tidak suka berada dalam situasi yang tidak terduga. .........", "id": "Setuju, saya lebih suka persiapan." },
  { "en": "Saya adalah orang yang sangat gigih dalam mengejar cita-cita. .........", "id": "Setuju, saya pantang menyerah." },
  { "en": "Saya tidak suka jika orang lain tidak mendengarkan pendapat saya. .........", "id": "Setuju, saya ingin merasa dihargai." },
  { "en": "Saya adalah orang yang sangat stabil dan dapat diandalkan. .........", "id": "Setuju, teman-teman mengandalkan saya." },
  { "en": "Saya suka memecahkan masalah yang membutuhkan kreativitas. .........", "id": "Setuju, saya suka berpikir di luar kotak." },
  { "en": "Saya lebih suka menghabiskan waktu dengan teman-teman. .........", "id": "Setuju, saya adalah makhluk sosial." },
  { "en": "Saya adalah orang yang sangat berorientasi pada target. .........", "id": "Setuju, saya fokus pada pencapaian." },
  { "en": "Saya tidak suka menonjolkan diri dalam keramaian. .........", "id": "Setuju, saya lebih suka menjadi pengamat." },
  { "en": "Saya jarang merasa kewalahan oleh informasi baru. .........", "id": "Setuju, saya adalah pembelajar cepat." },
  { "en": "Saya suka mencoba hal-hal yang menantang adrenalin. .........", "id": "Setuju, saya suka petualangan." },
  { "en": "Saya adalah orang yang sangat ekspresif secara emosional. .........", "id": "Setuju, perasaan saya mudah terbaca." },
  { "en": "Saya tidak suka pekerjaan yang membutuhkan banyak interaksi. .........", "id": "Setuju, saya lebih suka bekerja sendiri." },
  { "en": "Saya adalah orang yang sangat murah hati dengan waktu saya. .........", "id": "Setuju, saya suka membantu orang lain." },
  { "en": "Saya jarang merasa cemas akan hal yang belum terjadi. .........", "id": "Setuju, saya fokus pada saat ini." },
  { "en": "Saya suka melakukan sesuatu yang berbeda setiap akhir pekan. .........", "id": "Setuju, saya tidak suka rutinitas." },
  { "en": "Saya lebih suka bekerja di lingkungan yang kompetitif. .........", "id": "Setuju, itu memotivasi saya untuk lebih baik." },
  { "en": "Saya adalah orang yang sangat hemat dalam berbelanja. .........", "id": "Setuju, saya selalu mencari diskon." },
  { "en": "Saya tidak suka mengkritik orang secara langsung. .........", "id": "Setuju, saya memilih kata-kata dengan hati-hati." },
  { "en": "Saya adalah orang yang sangat tenang dan sabar. .........", "id": "Setuju, saya tidak mudah terprovokasi." },
  { "en": "Saya suka mempelajari tentang luar angkasa. .........", "id": "Setuju, alam semesta sangat menakjubkan." },
  { "en": "Saya merasa nyaman menjadi pemimpin dalam sebuah grup. .........", "id": "Setuju, saya suka mengambil tanggung jawab." },
  { "en": "Saya tidak suka jika ada barang yang tidak simetris. .........", "id": "Setuju, saya menyukai keseimbangan." },
  { "en": "Saya adalah orang yang sangat setia pada merek tertentu. .........", "id": "Setuju, saya menghargai kualitas." },
  { "en": "Saya jarang merasa pesimis bahkan saat gagal. .........", "id": "Setuju, saya melihatnya sebagai pelajaran." },
  { "en": "Saya suka menghadiri pameran teknologi. .........", "id": "Setuju, saya suka melihat inovasi terbaru." },
  { "en": "Saya lebih suka berdiskusi dengan orang yang punya pendapat beda. .........", "id": "Setuju, itu memperluas wawasan saya." },
  { "en": "Saya adalah orang yang sangat berhati-hati dalam berbicara. .........", "id": "Setuju, saya memikirkan dampak kata-kata." },
  { "en": "Saya tidak suka menjadi pusat perhatian negatif. .........", "id": "Setuju, saya menjaga reputasi saya." },
  { "en": "Saya adalah orang yang sangat sabar dalam mengajar. .........", "id": "Setuju, saya memahami setiap orang berbeda." },
  { "en": "Saya suka membaca buku-buku sejarah. .........", "id": "Setuju, saya suka belajar dari masa lalu." },
  { "en": "Saya lebih suka liburan yang penuh relaksasi. .........", "id": "Setuju, saya perlu mengisi ulang energi." },
  { "en": "Saya adalah orang yang sangat efisien dalam menyelesaikan tugas. .........", "id": "Setuju, saya tidak suka menunda." },
  { "en": "Saya tidak suka jika orang lain ikut campur dalam urusan pribadi saya. .........", "id": "Setuju, saya menghargai privasi." },
  { "en": "Saya jarang marah, tetapi sekali marah bisa sangat intens. .........", "id": "Setuju, saya menahan sampai batasnya." },
  { "en": "Saya suka mencoba makanan dari berbagai budaya. .........", "id": "Setuju, saya suka petualangan rasa." },
  { "en": "Saya merasa lebih produktif di pagi hari. .........", "id": "Setuju, saya adalah 'morning person'." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab pada keluarga. .........", "id": "Setuju, keluarga adalah prioritas utama." },
  { "en": "Saya tidak suka membicarakan pencapaian saya. .........", "id": "Setuju, saya lebih suka karya saya yang berbicara." },
  { "en": "Saya jarang merasa terancam oleh kompetisi. .........", "id": "Setuju, saya percaya pada kemampuan saya." },
  { "en": "Saya suka melakukan sesuatu yang menantang secara fisik. .........", "id": "Setuju, saya suka menguji batas saya." },
  { "en": "Saya lebih suka pekerjaan dengan jam kerja yang pasti. .........", "id": "Setuju, saya suka struktur dan rutinitas." },
  { "en": "Saya adalah orang yang sangat terorganisir dalam perjalanan. .........", "id": "Setuju, saya merencanakan setiap detail." },
  { "en": "Saya tidak suka mengecewakan teman-teman saya. .........", "id": "Setuju, saya adalah teman yang baik." },
  { "en": "Saya adalah orang yang sangat positif dalam menghadapi masalah. .........", "id": "Setuju, saya selalu mencari solusi." },
  { "en": "Saya suka memberikan hadiah yang bermanfaat. .........", "id": "Setuju, saya memikirkan kebutuhan mereka." },
  { "en": "Saya lebih suka mengamati dinamika sosial dalam kelompok. .........", "id": "Setuju, saya adalah seorang pengamat." },
  { "en": "Saya adalah orang yang sangat dapat diandalkan dalam krisis. .........", "id": "Setuju, saya tetap tenang dan fokus." },
  { "en": "Saya tidak suka berada di sekitar orang yang tidak tulus. .........", "id": "Setuju, saya menghargai kejujuran." },
  { "en": "Saya adalah orang yang sangat tangguh menghadapi kesulitan. .........", "id": "Setuju, saya tidak mudah patah semangat." },
  { "en": "Saya suka permainan yang membutuhkan kerja sama tim. .........", "id": "Setuju, saya suka berkolaborasi." },
  { "en": "Saya lebih suka komunikasi yang efisien dan to the point. .........", "id": "Setuju, saya tidak suka basa-basi." },
  { "en": "Saya adalah orang yang sangat teliti dalam merawat barang. .........", "id": "Setuju, saya menjaga barang-barang saya." },
  { "en": "Saya tidak suka menjadi beban pikiran orang lain. .........", "id": "Setuju, saya menyelesaikan masalah saya sendiri." },
  { "en": "Saya jarang merasa sedih atau murung. .........", "id": "Setuju, saya memiliki pandangan hidup ceria." },
  { "en": "Saya suka merencanakan pesta atau acara kumpul-kumpul. .........", "id": "Setuju, saya menikmati mengorganisir." },
  { "en": "Saya lebih suka pekerjaan yang memberi saya kebebasan berekspresi. .........", "id": "Setuju, saya suka kreativitas." },
  { "en": "Saya adalah orang yang sangat peduli pada keadilan sosial. .........", "id": "Setuju, saya membela yang lemah." },
  { "en": "Saya tidak suka berada di tengah perdebatan sengit. .........", "id": "Setuju, saya lebih suka suasana damai." },
  { "en": "Saya jarang merasa bosan jika ada buku untuk dibaca. .........", "id": "Setuju, buku adalah teman terbaik." },
  { "en": "Saya suka melakukan hal-hal yang benar meskipun sulit. .........", "id": "Setuju, saya berpegang pada prinsip." },
  { "en": "Saya lebih suka bekerja dengan orang yang cerdas. .........", "id": "Setuju, saya suka terstimulasi secara intelektual." },
  { "en": "Saya adalah orang yang sangat teratur dalam menyimpan file. .........", "id": "Setuju, saya tidak suka kehilangan data." },
  { "en": "Saya tidak suka jika orang lain mengambil keuntungan dari saya. .........", "id": "Setuju, saya menghargai keadilan." },
  { "en": "Saya adalah orang yang sangat tenang dan tidak mudah terkejut. .........", "id": "Setuju, saya memiliki kontrol diri yang baik." },
  { "en": "Saya suka belajar tentang filsafat dan ide-ide besar. .........", "id": "Setuju, itu memperluas cara pandang saya." },
  { "en": "Saya lebih suka hubungan yang stabil dan jangka panjang. .........", "id": "Setuju, saya mencari komitmen." },
  { "en": "Saya adalah orang yang sangat disiplin dalam hal diet. .........", "id": "Setuju, saya menjaga kesehatan saya." },
  { "en": "Saya tidak suka jika rencana yang sudah matang diubah. .........", "id": "Setuju, saya menghargai persiapan." },
  { "en": "Saya adalah orang yang sangat suportif terhadap impian orang lain. .........", "id": "Setuju, saya senang melihat orang berhasil." },
  { "en": "Saya suka tantangan yang membutuhkan daya tahan mental. .........", "id": "Setuju, saya suka menguji batas pikiran saya." },
  { "en": "Saya lebih suka lingkungan kerja yang informal. .........", "id": "Setuju, saya lebih santai dan kreatif." },
  { "en": "Saya adalah orang yang sangat berprinsip dan konsisten. .........", "id": "Setuju, saya berpegang teguh pada nilai." },
  { "en": "Saya tidak suka memikirkan kembali kegagalan. .........", "id": "Setuju, saya lebih baik menatap ke depan." },
  { "en": "Saya adalah orang yang sangat ceria dan suka humor. .........", "id": "Setuju, tawa adalah obat terbaik." },
  { "en": "Saya suka melakukan sesuatu yang berdampak positif bagi banyak orang. .........", "id": "Setuju, saya ingin membuat perbedaan." },
  { "en": "Saya lebih suka menyelesaikan konflik secara langsung. .........", "id": "Setuju, agar masalah cepat selesai." },
  { "en": "Ketika saya memiliki tujuan, saya .........", "id": "Membuat rencana langkah demi langkah." },
  { "en": "Saya tidak suka jika orang lain membuat keputusan untuk saya. .........", "id": "Setuju, saya suka memegang kendali." },
  { "en": "Saya sering merasa perlu menyendiri setelah hari yang sibuk. .........", "id": "Setuju, untuk memulihkan energi saya." },
  { "en": "Saya percaya bahwa pengalaman adalah guru yang paling berharga. .........", "id": "Setuju, saya belajar dari tindakan." },
  { "en": "Saya lebih suka memberi hadiah yang praktis dan bermanfaat. .........", "id": "Setuju, fungsionalitas itu penting." },
  { "en": "Saya adalah orang yang sangat sadar akan bahasa tubuh. .........", "id": "Setuju, saya bisa membaca situasi." },
  { "en": "Saya tidak suka membuang waktu untuk hal yang tidak penting. .........", "id": "Setuju, saya sangat efisien." },
  { "en": "Saya merasa cemas jika masa depan tidak pasti. .........", "id": "Setuju, saya butuh kepastian." },
  { "en": "Saya suka menonton video tutorial untuk belajar hal baru. .........", "id": "Setuju, saya adalah pembelajar visual." },
  { "en": "Saya tidak suka menjadi orang pertama yang berbicara. .........", "id": "Setuju, saya lebih suka mengamati dulu." },
  { "en": "Saya adalah orang yang sangat berorientasi pada hasil. .........", "id": "Setuju, hasil akhir adalah yang utama." },
  { "en": "Saya tidak keberatan mengorbankan kepentingan saya demi tim. .........", "id": "Setuju, kesuksesan tim lebih penting." },
  { "en": "Saya jarang merasa terganggu oleh kritik. .........", "id": "Setuju, saya menganggapnya sebagai masukan." },
  { "en": "Saya tertarik pada ide-ide yang menentang status quo. .........", "id": "Setuju, saya suka pemikiran progresif." },
  { "en": "Saya lebih suka bekerja di lingkungan yang terprediksi. .........", "id": "Setuju, saya tidak suka kejutan." },
  { "en": "Saya adalah orang yang selalu memegang teguh komitmen. .........", "id": "Setuju, kata-kata saya adalah janji." },
  { "en": "Saya tidak suka jika seseorang terlalu formal. .........", "id": "Setuju, saya lebih suka suasana santai." },
  { "en": "Saya jarang menyesali masa lalu. .........", "id": "Setuju, saya fokus pada masa depan." },
  { "en": "Saya suka merawat tanaman atau hewan peliharaan. .........", "id": "Setuju, itu mengajarkan tanggung jawab." },
  { "en": "Saya merasa lebih kreatif ketika bekerja di bawah tekanan. .........", "id": "Tidak setuju, tekanan menghambat kreativitas." },
  { "en": "Saya adalah orang yang sangat bersih dan higienis. .........", "id": "Setuju, kebersihan sangat penting." },
  { "en": "Saya mencoba untuk tidak memihak dalam sebuah konflik. .........", "id": "Setuju, saya berusaha objektif." },
  { "en": "Saya jarang merasa gugup saat menghadapi tantangan. .........", "id": "Setuju, saya melihatnya sebagai peluang." },
  { "en": "Saya lebih suka buku yang membuat saya tertawa. .........", "id": "Setuju, saya suka humor." },
  { "en": "Saya tidak suka berada dalam situasi yang penuh drama. .........", "id": "Setuju, saya menghindari konflik yang tidak perlu." },
  { "en": "Saya adalah orang yang sangat gigih dalam menyelesaikan masalah. .........", "id": "Setuju, saya tidak berhenti sampai tuntas." },
  { "en": "Saya tidak suka jika orang lain tidak menghargai usaha saya. .........", "id": "Setuju, pengakuan itu penting." },
  { "en": "Saya adalah orang yang sangat stabil dan tenang. .........", "id": "Setuju, saya sulit dibuat panik." },
  { "en": "Saya suka memecahkan masalah dengan cara yang logis. .........", "id": "Setuju, saya mengandalkan penalaran." },
  { "en": "Saya lebih suka menghabiskan waktu dengan sedikit teman. .........", "id": "Setuju, saya menghargai kualitas hubungan." },
  { "en": "Saya adalah orang yang sangat berorientasi pada detail. .........", "id": "Setuju, saya tidak suka ada yang terlewat." },
  { "en": "Saya tidak suka menjadi pusat perhatian dalam sebuah kemenangan. .........", "id": "Setuju, itu adalah usaha tim." },
  { "en": "Saya jarang merasa kewalahan oleh pekerjaan. .........", "id": "Setuju, saya punya manajemen waktu yang baik." },
  { "en": "Saya suka mencoba hal-hal yang sedikit menakutkan. .........", "id": "Setuju, saya suka memacu adrenalin." },
  { "en": "Saya adalah orang yang sangat terbuka dengan perasaan saya. .........", "id": "Setuju, saya tidak suka memendam." },
  { "en": "Saya tidak suka pekerjaan yang terlalu banyak aturan. .........", "id": "Setuju, saya suka fleksibilitas." },
  { "en": "Saya adalah orang yang sangat murah hati dengan pengetahuan. .........", "id": "Setuju, saya suka berbagi ilmu." },
  { "en": "Saya jarang merasa cemas tentang hal-hal di luar kendali. .........", "id": "Setuju, saya menerima apa adanya." },
  { "en": "Saya suka melakukan sesuatu yang berbeda dari orang lain. .........", "id": "Setuju, saya suka menjadi unik." },
  { "en": "Saya lebih suka bekerja di lingkungan yang mendukung. .........", "id": "Setuju, itu meningkatkan moral saya." },
  { "en": "Saya adalah orang yang sangat hemat. .........", "id": "Setuju, saya merencanakan keuangan dengan baik." },
  { "en": "Saya tidak suka mengkritik ide orang lain secara terbuka. .........", "id": "Setuju, saya memberikan masukan pribadi." },
  { "en": "Saya adalah orang yang sangat tenang dalam menghadapi masalah. .........", "id": "Setuju, saya fokus pada solusi." },
  { "en": "Saya suka mempelajari sejarah seni. .........", "id": "Setuju, itu memperkaya perspektif saya." },
  { "en": "Saya merasa nyaman memimpin sebuah diskusi. .........", "id": "Setuju, saya suka mengarahkan percakapan." },
  { "en": "Saya tidak suka jika ada barang yang letaknya berubah. .........", "id": "Setuju, saya suka keteraturan." },
  { "en": "Saya adalah orang yang sangat setia pada nilai-nilai saya. .........", "id": "Setuju, saya tidak mudah terpengaruh." },
  { "en": "Saya jarang merasa pesimis tentang kemampuan saya. .........", "id": "Setuju, saya percaya pada diri sendiri." },
  { "en": "Saya suka menghadiri lokakarya keterampilan. .........", "id": "Setuju, saya suka belajar hal praktis." },
  { "en": "Saya lebih suka berdiskusi untuk mencapai mufakat. .........", "id": "Setuju, kompromi itu penting." },
  { "en": "Saya adalah orang yang sangat berhati-hati dalam mempercayai orang. .........", "id": "Setuju, kepercayaan harus didapatkan." },
  { "en": "Saya tidak suka menjadi pusat perhatian saat gagal. .........", "id": "Setuju, itu membuat saya malu." },
  { "en": "Saya adalah orang yang sangat sabar dalam mencapai tujuan. .........", "id": "Setuju, saya percaya pada proses." },
  { "en": "Saya suka membaca buku-buku filsafat. .........", "id": "Setuju, saya suka pertanyaan mendalam." },
  { "en": "Saya lebih suka liburan yang aktif dan penuh kegiatan. .........", "id": "Setuju, saya tidak suka diam." },
  { "en": "Saya adalah orang yang sangat efisien dalam berkomunikasi. .........", "id": "Setuju, saya langsung ke intinya." },
  { "en": "Saya tidak suka jika orang lain tidak bertanggung jawab. .........", "id": "Setuju, semua orang harus punya akuntabilitas." },
  { "en": "Saya jarang marah karena lalu lintas macet. .........", "id": "Setuju, saya menerimanya sebagai bagian kota." },
  { "en": "Saya suka mencoba aplikasi atau perangkat lunak baru. .........", "id": "Setuju, saya suka teknologi." },
  { "en": "Saya merasa lebih produktif saat bekerja dari rumah. .........", "id": "Setuju, saya bisa lebih fokus." },
  { "en": "Saya adalah orang yang sangat bertanggung jawab atas tim saya. .........", "id": "Setuju, kesuksesan mereka adalah tanggung jawab saya." },
  { "en": "Saya tidak suka membicarakan kekayaan saya. .........", "id": "Setuju, saya lebih suka tetap sederhana." },
  { "en": "Saya jarang merasa terancam oleh orang sukses. .........", "id": "Setuju, saya menjadikannya inspirasi." },
  { "en": "Saya suka melakukan sesuatu yang menantang kreativitas saya. .........", "id": "Setuju, saya suka menghasilkan karya." },
  { "en": "Saya lebih suka pekerjaan dengan tujuan yang jelas. .........", "id": "Setuju, saya butuh arah yang pasti." },
  { "en": "Saya adalah orang yang sangat terorganisir dalam kehidupan pribadi. .........", "id": "Setuju, saya merencanakan segalanya." },
  { "en": "Saya tidak suka mengecewakan atasan saya. .........", "id": "Setuju, saya menghormati hierarki." },
  { "en": "Saya adalah orang yang sangat positif dan jarang mengeluh. .........", "id": "Setuju, saya fokus pada solusi." },
  { "en": "Saya suka memberikan hadiah yang dipersonalisasi. .........", "id": "Setuju, itu menunjukkan perhatian lebih." },
  { "en": "Saya lebih suka mengamati dalam diam. .........", "id": "Setuju, saya belajar banyak dari observasi." },
  { "en": "Saya adalah orang yang sangat dapat diandalkan oleh keluarga. .........", "id": "Setuju, saya adalah sandaran mereka." },
  { "en": "Saya tidak suka berada di sekitar orang yang suka pamer. .........", "id": "Setuju, saya menghargai kerendahan hati." },
  { "en": "Saya adalah orang yang sangat tangguh menghadapi perubahan. .........", "id": "Setuju, saya mudah beradaptasi." },
  { "en": "Saya suka permainan yang membutuhkan strategi tim. .........", "id": "Setuju, saya suka berkolaborasi untuk menang." },
  { "en": "Saya lebih suka komunikasi yang jujur meski menyakitkan. .........", "id": "Setuju, kebenaran itu penting." },
  { "en": "Saya adalah orang yang sangat teliti dalam mengelola uang. .........", "id": "Setuju, saya mencatat setiap pengeluaran." },
  { "en": "Saya tidak suka menjadi beban dalam sebuah hubungan. .........", "id": "Setuju, saya ingin menjadi pasangan setara." },
  { "en": "Saya jarang merasa sedih tentang hal-hal di luar kendali. .........", "id": "Setuju, saya menerima apa yang tidak bisa diubah." },
  { "en": "Saya suka merencanakan masa depan keuangan saya. .........", "id": "Setuju, saya suka keamanan finansial." },
  { "en": "Saya lebih suka pekerjaan yang memberi saya kesempatan belajar. .........", "id": "Setuju, saya menghargai pertumbuhan." },
  { "en": "Saya adalah orang yang sangat peduli pada detail kecil. .........", "id": "Setuju, detail membuat perbedaan." },
  { "en": "Saya tidak suka berada di tengah perdebatan politik. .........", "id": "Setuju, saya lebih suka menjaga netralitas." },
  { "en": "Saya jarang merasa bosan karena saya punya banyak hobi. .........", "id": "Setuju, saya selalu punya kegiatan." },
  { "en": "Saya suka melakukan hal-hal yang benar secara etis. .........", "id": "Setuju, saya berpegang pada moral." },
  { "en": "Saya lebih suka bekerja dengan orang yang memiliki etos kerja sama. .........", "id": "Setuju, semangat tim itu penting." },
  { "en": "Saya adalah orang yang sangat teratur dalam rutinitas harian. .........", "id": "Setuju, saya suka jadwal yang konsisten." },
  { "en": "Saya tidak suka jika orang lain meremehkan kemampuan saya. .........", "id": "Setuju, itu memotivasi saya untuk membuktikan." },
  { "en": "Saya adalah orang yang sangat tenang dan jarang cemas. .........", "id": "Setuju, saya bisa mengelola pikiran saya." },
  { "en": "Saya suka belajar tentang cara kerja alam semesta. .........", "id": "Setuju, saya suka ilmu pengetahuan." },
  { "en": "Saya lebih suka hubungan yang didasarkan pada kepercayaan. .........", "id": "Setuju, kepercayaan adalah fondasi." },
  { "en": "Saya adalah orang yang sangat disiplin dalam mencapai tujuan. .........", "id": "Setuju, saya fokus dan konsisten." },
  { "en": "Saya tidak suka jika rencana liburan saya berantakan. .........", "id": "Setuju, saya menghargai persiapan." },
  { "en": "Saya adalah orang yang sangat suportif terhadap keputusan teman. .........", "id": "Setuju, saya mendukung pilihan mereka." },
  { "en": "Saya suka tantangan yang membutuhkan pemecahan masalah visual. .........", "id": "Setuju, saya suka teka-teki gambar." },
  { "en": "Saya lebih suka lingkungan kerja yang beragam. .........", "id": "Setuju, saya belajar dari berbagai perspektif." },
  { "en": "Saya adalah orang yang sangat berprinsip. .........", "id": "Setuju, saya tidak mudah goyah." },
  { "en": "Saya tidak suka memikirkan kembali kesalahan kecil. .........", "id": "Setuju, saya fokus pada gambaran besar." },
  { "en": "Saya adalah orang yang sangat ceria dan mudah berteman. .........", "id": "Setuju, saya adalah seorang ekstrovert." },
  { "en": "Saya suka melakukan sesuatu yang berdampak jangka panjang. .........", "id": "Setuju, saya memikirkan warisan." }


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
