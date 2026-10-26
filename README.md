<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>L3-6與學測112-115 (GSAT Vocabulary App)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        // Suppress Tailwind CDN production warning in console
        const origWarn = console.warn;
        console.warn = function(...args) {
            if (args[0] && typeof args[0] === 'string' && args[0].includes('cdn.tailwindcss.com')) return;
            origWarn.apply(console, args);
        };
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Custom Styles for 3D Card Flip -->
    <style>
        .perspective-1000 {
            perspective: 1000px;
        }
        .transform-style-3d {
            transform-style: preserve-3d;
        }
        .backface-hidden {
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }
        .card-inner {
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .card-container.flipped .card-inner {
            transform: rotateY(180deg);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-100 text-slate-800 font-sans min-h-screen flex flex-col">

    <!-- Header Navigation Bar -->
    <header class="bg-indigo-700 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex flex-col md:flex-row justify-between items-center gap-3">
            <div class="flex items-center space-x-3">
                <i class="fa-solid me-2 fa-graduation-cap text-2xl text-yellow-300"></i>
                <h1 class="text-xl md:text-2xl font-bold tracking-wide">🐰L3-6＋學測112-115🐰</h1>
            </div>
            <div class="flex items-center space-x-2 text-xs md:text-sm bg-indigo-800/60 px-3 py-1.5 rounded-lg border border-indigo-500/30">
                <span id="tts-status"><i class="fa-solid fa-volume-high text-emerald-400"></i> 語音引擎預備完成</span>
            </div>
        </div>

        <!-- Navigation Tabs -->
        <nav class="bg-indigo-800 text-indigo-100 overflow-x-auto scrollbar-none border-t border-indigo-600/50">
            <div class="max-w-7xl mx-auto flex px-2 space-x-1 whitespace-nowrap text-sm font-medium py-1" id="tab-container">
                <button onclick="switchTab('L3')" id="tab-L3" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">L3 單字</button>
                <button onclick="switchTab('L4')" id="tab-L4" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">L4 單字</button>
                <button onclick="switchTab('L5')" id="tab-L5" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">L5 單字</button>
                <button onclick="switchTab('L6')" id="tab-L6" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">L6 單字</button>
                <button onclick="switchTab('112')" id="tab-112" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">112 學測</button>
                <button onclick="switchTab('113')" id="tab-113" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">113 學測</button>
                <button onclick="switchTab('114')" id="tab-114" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">114 學測</button>
                <button onclick="switchTab('115')" id="tab-115" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition">115 學測</button>
                <button onclick="switchTab('AZ')" id="tab-AZ" class="tab-btn px-4 py-2 rounded-md hover:bg-indigo-700 transition"><i class="fa-solid fa-list-ul mr-1"></i> A-Z 全部單字總表</button>
                <button onclick="switchTab('QUIZ')" id="tab-QUIZ" class="tab-btn px-4 py-2 rounded-md bg-amber-500 text-slate-900 font-bold hover:bg-amber-400 transition"><i class="fa-solid fa-pen-to-square mr-1"></i> 50題選擇題測驗</button>
            </div>
        </nav>
    </header>

    <!-- Main Content Body -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6">

        <!-- FLASHCARD VIEW (Used for tabs L3-L6, 112-115) -->
        <div id="view-flashcard" class="space-y-6">
            <!-- Deck Info Bar -->
            <div class="flex flex-wrap justify-between items-center bg-white p-4 rounded-xl shadow-sm border border-slate-200 gap-4">
                <div>
                    <h2 id="deck-title" class="text-xl font-bold text-slate-800">L3 高中基礎必備單字</h2>
                    <p id="deck-subtitle" class="text-sm text-slate-500">點擊卡片即可翻面查看英文及詳細例句</p>
                </div>
                <div class="flex items-center space-x-3">
                    <span id="card-counter" class="text-sm font-semibold bg-indigo-50 text-indigo-700 px-3 py-1.5 rounded-full border border-indigo-200">
                        1 / 10
                    </span>
                    <button onclick="shuffleCurrentDeck()" class="px-3 py-1.5 text-sm bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium rounded-lg border border-slate-300 transition flex items-center space-x-1">
                        <i class="fa-solid fa-shuffle text-indigo-600"></i>
                        <span>隨機排序</span>
                    </button>
                </div>
            </div>

            <!-- 3D Flashcard Section -->
            <div class="flex justify-center my-6">
                <div id="flashcard-container" onclick="toggleCardFlip()" class="card-container perspective-1000 w-full max-w-2xl h-[450px] md:h-[430px] cursor-pointer">
                    <div id="card-inner" class="card-inner relative w-full h-full transform-style-3d shadow-xl rounded-2xl border border-slate-200 bg-white">
                        
                        <!-- FRONT OF CARD (Chinese Meaning Very Large & POS First) -->
                        <div class="card-front absolute inset-0 backface-hidden p-8 flex flex-col justify-between items-center text-center bg-gradient-to-br from-white via-indigo-50/30 to-indigo-100/40 rounded-2xl">
                            <div class="w-full flex justify-between items-center text-xs text-slate-400 font-medium">
                                <span><i class="fa-regular fa-lightbulb text-amber-500 mr-1"></i>卡片正面 (中文 & 詞性)</span>
                                <span>點擊翻面 <i class="fa-solid fa-rotate text-indigo-500"></i></span>
                            </div>

                            <div class="my-auto space-y-4">
                                <span id="card-front-pos" class="inline-block px-4 py-1.5 rounded-full bg-indigo-600 text-white text-base md:text-xl font-bold tracking-wide shadow-sm">
                                    v.
                                </span>
                                <div id="card-front-chinese" class="text-4xl md:text-6xl font-black text-slate-800 tracking-wider">
                                    定居；解決
                                </div>
                            </div>

                            <div class="text-xs text-slate-400">
                                翻面查看英文單字、音標與完整大字體英文例句
                            </div>
                        </div>

                        <!-- BACK OF CARD (English Word, IPA, Audio, Meaning, Large Examples) -->
                        <div class="card-back absolute inset-0 backface-hidden rotate-y-180 p-6 md:p-8 flex flex-col justify-between bg-slate-900 text-white rounded-2xl overflow-y-auto">
                            <!-- Top Info & Audio Button -->
                            <div class="flex justify-between items-start border-b border-slate-800 pb-3">
                                <div>
                                    <div class="flex items-baseline space-x-3">
                                        <h3 id="card-back-word" class="text-3xl md:text-4xl font-extrabold text-indigo-400 tracking-wide">
                                            settle
                                        </h3>
                                        <span id="card-back-pos" class="text-base font-semibold text-amber-400 italic">
                                            (v.)
                                        </span>
                                    </div>
                                    <span id="card-back-phonetic" class="text-sm text-slate-400 font-mono">
                                        /ˈset.əl/
                                    </span>
                                </div>
                                <button onclick="event.stopPropagation(); speakWord();" title="播放發音" class="p-3 bg-indigo-600 hover:bg-indigo-500 active:scale-95 text-white rounded-full shadow-lg transition flex items-center justify-center">
                                    <i class="fa-solid fa-volume-high text-xl"></i>
                                </button>
                            </div>

                            <!-- Chinese Meaning Ref -->
                            <div class="my-2">
                                <p class="text-xs text-slate-400 uppercase tracking-wider font-semibold">中文解釋</p>
                                <p id="card-back-chinese" class="text-xl md:text-2xl font-bold text-slate-100">定居；安頓；解決（爭端）</p>
                            </div>

                            <!-- LARGE Clear Bilingual Examples -->
                            <div class="bg-slate-800/90 p-5 rounded-xl border border-slate-700/80 my-2 space-y-3">
                                <p class="text-xs text-indigo-300 font-bold uppercase tracking-wider"><i class="fa-solid fa-quote-left mr-1"></i> 實用雙語例句</p>
                                <p id="card-back-ex-en" class="text-xl md:text-2xl font-bold text-amber-300 leading-relaxed">
                                    "They decided to settle in a quiet town after living in the big city for ten years."
                                </p>
                                <p id="card-back-ex-zh" class="text-base md:text-lg text-indigo-200">
                                    在大城市生活了十年之後，他們決定在一個安靜的小鎮定居下來。
                                </p>
                            </div>

                            <div class="text-right text-xs text-slate-500">
                                點擊任意處翻回正面
                            </div>
                        </div>

                    </div>
                </div>
            </div>

            <!-- Controls (Next, Previous, TTS Pronunciation) -->
            <div class="flex justify-center items-center space-x-4 max-w-md mx-auto">
                <button onclick="prevCard()" class="flex-1 py-3 px-4 bg-white hover:bg-slate-50 text-slate-700 font-bold rounded-xl shadow-md border border-slate-200 transition active:scale-95 flex items-center justify-center space-x-2">
                    <i class="fa-solid fa-chevron-left"></i>
                    <span>上一張</span>
                </button>
                
                <button onclick="speakWord()" class="py-3 px-5 bg-indigo-100 hover:bg-indigo-200 text-indigo-700 font-bold rounded-xl shadow-sm border border-indigo-200 transition active:scale-95 flex items-center justify-center" title="朗讀英文單字">
                    <i class="fa-solid fa-volume-high text-xl"></i>
                </button>

                <button onclick="nextCard()" class="flex-1 py-3 px-4 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl shadow-md transition active:scale-95 flex items-center justify-center space-x-2">
                    <span>下一張</span>
                    <i class="fa-solid fa-chevron-right"></i>
                </button>
            </div>
        </div>

        <!-- TAB 9: A-Z ALL WORDS MASTER LIST VIEW -->
        <div id="view-az" class="hidden space-y-6">
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 mb-6">
                    <div>
                        <h2 class="text-2xl font-bold text-slate-800">A-Z 全部單字總表</h2>
                        <p class="text-sm text-slate-500">包含所有 Level 3-6 及 112-115 學測高頻核心單字，可搜尋、篩選與點擊發音</p>
                    </div>
                    <div class="flex items-center space-x-2">
                        <span id="az-count-badge" class="bg-indigo-100 text-indigo-800 text-xs font-bold px-3 py-1.5 rounded-full">
                            共 0 個單字
                        </span>
                    </div>
                </div>

                <!-- Search and Filters -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                    <div class="relative">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-3.5 text-slate-400"></i>
                        <input type="text" id="az-search-input" oninput="filterAZList()" placeholder="搜尋英文單字或中文意思..." class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm">
                    </div>
                    <div>
                        <select id="az-level-filter" onchange="filterAZList()" class="w-full py-2.5 px-3 rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                            <option value="ALL">全部範圍 / 年份</option>
                            <option value="L3">L3</option>
                            <option value="L4">L4</option>
                            <option value="L5">L5</option>
                            <option value="L6">L6</option>
                            <option value="112">112 學測</option>
                            <option value="113">113 學測</option>
                            <option value="114">114 學測</option>
                            <option value="115">115 預測</option>
                        </select>
                    </div>
                    <div>
                        <select id="az-sort-select" onchange="filterAZList()" class="w-full py-2.5 px-3 rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                            <option value="az">按 A-Z 字母排序</option>
                            <option value="za">按 Z-A 字母排序</option>
                            <option value="level">按範圍排序</option>
                        </select>
                    </div>
                </div>

                <!-- Table Container -->
                <div class="overflow-x-auto rounded-xl border border-slate-200">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-slate-600 text-xs uppercase font-bold border-b border-slate-200">
                                <th class="p-3 md:p-4">單字 / 音標</th>
                                <th class="p-3 md:p-4">詞性</th>
                                <th class="p-3 md:p-4">中文意思</th>
                                <th class="p-3 md:p-4">分類標籤</th>
                                <th class="p-3 md:p-4 text-center">發音</th>
                            </tr>
                        </thead>
                        <tbody id="az-table-body" class="divide-y divide-slate-200 text-sm">
                            <!-- Rows will be injected by JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- TAB 10: 50-QUESTION GSAT MULTIPLE CHOICE QUIZ VIEW -->
        <div id="view-quiz" class="hidden space-y-6">
            <!-- Quiz Start / Overview Banner -->
            <div id="quiz-start-screen" class="bg-white p-8 rounded-2xl shadow-sm border border-slate-200 text-center space-y-6">
                <div class="inline-flex p-4 rounded-full bg-amber-100 text-amber-600 mb-2">
                    <i class="fa-solid fa-award text-4xl"></i>
                </div>
                <h2 class="text-3xl font-extrabold text-slate-800">50 題學測英文單字模擬測驗</h2>
                <p class="text-slate-600 max-w-2xl mx-auto">
                    測驗題目自 L3-L6 及 112-115 歷年學測高頻詞彙隨機抽樣，每題包含四選一選擇題與詳細語境例句。答題後即時提供解析與語音發音。
                </p>
                <div class="flex justify-center items-center space-x-6 text-sm text-slate-500">
                    <span><i class="fa-regular fa-clock text-indigo-600 mr-1"></i> 題數：50 題</span>
                    <span><i class="fa-solid fa-shuffle text-emerald-600 mr-1"></i> 隨機選項與題號</span>
                    <span><i class="fa-solid fa-volume-high text-amber-600 mr-1"></i> 支援語音朗讀</span>
                </div>
                <div>
                    <button onclick="startQuiz()" class="px-8 py-4 bg-amber-500 hover:bg-amber-400 text-slate-900 font-bold text-lg rounded-xl shadow-lg hover:shadow-xl transition transform active:scale-95">
                        <i class="fa-solid fa-play mr-2"></i> 開始模擬測驗
                    </button>
                </div>
            </div>

            <!-- Active Quiz Box -->
            <div id="quiz-active-screen" class="hidden space-y-6">
                <!-- Quiz Progress Top Bar -->
                <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex flex-wrap justify-between items-center gap-4">
                    <div class="flex items-center space-x-3">
                        <span id="quiz-question-number" class="text-lg font-bold text-indigo-700">第 1 / 50 題</span>
                        <span id="quiz-category-tag" class="text-xs bg-slate-100 text-slate-600 font-semibold px-2.5 py-1 rounded">L3</span>
                    </div>
                    <div class="flex items-center space-x-4">
                        <div class="text-sm font-semibold text-slate-600">
                            得分: <span id="quiz-live-score" class="text-emerald-600 font-bold">0</span>
                        </div>
                        <button onclick="restartQuizPrompt()" class="text-xs text-slate-500 hover:text-red-600 underline">
                            重新開始
                        </button>
                    </div>
                </div>

                <!-- Progress Bar -->
                <div class="w-full bg-slate-200 h-2 rounded-full overflow-hidden">
                    <div id="quiz-progress-bar" class="bg-indigo-600 h-full w-0 transition-all duration-300"></div>
                </div>

                <!-- Question Card -->
                <div class="bg-white p-6 md:p-8 rounded-2xl shadow-sm border border-slate-200 space-y-6">
                    <div class="space-y-3">
                        <div class="flex justify-between items-start">
                            <span class="text-xs font-bold uppercase tracking-wider text-indigo-600 bg-indigo-50 px-3 py-1 rounded-full">選擇題 (單字語境測驗)</span>
                            <button onclick="speakQuizWord()" class="p-2 text-indigo-600 hover:bg-indigo-50 rounded-lg transition" title="朗讀題目目標單字">
                                <i class="fa-solid fa-volume-high text-lg"></i>
                            </button>
                        </div>
                        <p id="quiz-question-text" class="text-xl md:text-2xl font-bold text-slate-800 leading-relaxed">
                            題目加載中...
                        </p>
                    </div>

                    <!-- Multiple Choice Options -->
                    <div id="quiz-options-container" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <!-- 4 Options populated dynamically -->
                    </div>

                    <!-- Instant Explanation Box (Hidden until answer selected) -->
                    <div id="quiz-explanation-box" class="hidden p-4 rounded-xl border bg-slate-50 space-y-2">
                        <div id="quiz-result-badge" class="font-bold text-sm flex items-center space-x-2">
                            <!-- Correct/Incorrect badge -->
                        </div>
                        <p id="quiz-explanation-text" class="text-sm text-slate-700">
                            <!-- Details and definition -->
                        </p>
                    </div>

                    <!-- Next Question Button Bar -->
                    <div class="flex justify-end pt-2">
                        <button id="quiz-next-btn" onclick="nextQuizQuestion()" disabled class="px-6 py-3 bg-slate-300 text-slate-500 font-bold rounded-xl shadow transition flex items-center space-x-2">
                            <span>下一題</span>
                            <i class="fa-solid fa-arrow-right"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Quiz Result Summary Screen -->
            <div id="quiz-result-screen" class="hidden bg-white p-8 rounded-2xl shadow-sm border border-slate-200 text-center space-y-6">
                <div class="inline-flex p-4 rounded-full bg-indigo-100 text-indigo-600 mb-2">
                    <i class="fa-solid fa-trophy text-5xl"></i>
                </div>
                <h2 class="text-3xl font-extrabold text-slate-800">測驗完成！</h2>
                
                <div class="p-6 bg-slate-50 rounded-2xl border border-slate-200 max-w-md mx-auto space-y-2">
                    <p class="text-sm text-slate-500 font-semibold uppercase">你的最終得分</p>
                    <div id="final-score" class="text-5xl font-black text-indigo-600">
                        80 / 100
                    </div>
                    <p id="final-eval" class="text-sm font-medium text-slate-600 pt-2">
                        表現優異！持續複習能讓記憶更牢固！
                    </p>
                </div>

                <div class="flex justify-center space-x-4">
                    <button onclick="startQuiz()" class="px-6 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl shadow transition">
                        <i class="fa-solid fa-rotate-right mr-2"></i> 再測驗一次
                    </button>
                    <button onclick="switchTab('AZ')" class="px-6 py-3 bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold rounded-xl border border-slate-300 transition">
                        <i class="fa-solid fa-book-open mr-2"></i> 查看 A-Z 單字總表
                    </button>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-800 text-slate-400 text-xs py-4 text-center mt-auto">
        <p>高中英文單字卡 & 學測 50 題模擬測驗系統 &copy; 2026</p>
    </footer>

    <!-- Application JavaScript Logic -->
    <script>
        /* ==========================================================================
           Vocabulary Dataset (ALL WORDS FROM FULL INPUT LIST)
           ========================================================================== */
        const ALL_WORDS = [
            // L3
            { word: "differ", phonetic: "/ˈdɪf.ɚ/", pos: "v.", chinese: "不同；相異", level: "L3", exEn: "Although they are sisters, Jamie differs from Tina in personality and appearance.", exZh: "雖然她們是姐妹，但潔咪在性格和外貌上都與蒂娜不同。" },
            { word: "differentiate", phonetic: "/ˌdɪf.əˈren.ʃi.eɪt/", pos: "v.", chinese: "區分；辨別", level: "L3", exEn: "Young children sometimes cannot differentiate fiction from reality.", exZh: "幼童有時無法區分虛構與現實。" },
            { word: "crew", phonetic: "/kruː/", pos: "n.", chinese: "全體工作人員；船員", level: "L3", exEn: "After the captain welcomed the passengers, members of the flight crew demonstrated safety procedures.", exZh: "機長歡迎乘客登機後，機組人員示範了各項安全程序。" },
            { word: "sailor", phonetic: "/ˈseɪ.lɚ/", pos: "n.", chinese: "水手；海員", level: "L3", exEn: "As a young boy, Steven loved the ocean and dreamed of becoming a sailor.", exZh: "史蒂芬從小就熱愛海洋，夢想著有一天能成為一名水手。" },
            { word: "sail", phonetic: "/seɪl/", pos: "v./n.", chinese: "航行；帆", level: "L3", exEn: "In 2019, Greta Thunberg sailed across the Atlantic Ocean.", exZh: "2019年，葛蕾塔·童貝里橫渡大西洋。" },
            { word: "settle", phonetic: "/ˈset.əl/", pos: "v.", chinese: "定居；解決", level: "L3", exEn: "Linda decided to settle in Spain after moving from her home country.", exZh: "琳達離開家鄉後決定在西班牙定居。" },
            { word: "settlement", phonetic: "/ˈset.əl.mənt/", pos: "n.", chinese: "協議；定居點", level: "L3", exEn: "The workers reached a settlement with their employer regarding pay raises.", exZh: "工人與雇主就加薪問題達成了一致協議。" },
            { word: "settler", phonetic: "/ˈset.lɚ/", pos: "n.", chinese: "移民；殖民者", level: "L3", exEn: "European settlers established communities in North America during the 17th century.", exZh: "十七世紀時，歐洲拓荒者在北美建立了社區。" },
            { word: "possess", phonetic: "/pəˈzes/", pos: "v.", chinese: "具有；擁有", level: "L3", exEn: "There is no right to possess firearms under local laws without a legal license.", exZh: "在當地法律下，未取得合法執照不得持有槍枝。" },
            { word: "possession", phonetic: "/pəˈzeʃ.ən/", pos: "n.", chinese: "擁有；個人財產", level: "L3", exEn: "Park officials found a flashlight among the lost hiker's personal possessions.", exZh: "公園管理人員在失蹤登山客的個人物品中發現了一支手電筒。" },
            { word: "exceptional", phonetic: "/ɪkˈsep.ʃən.əl/", pos: "adj.", chinese: "傑出的；特別的", level: "L3", exEn: "The child demonstrated an exceptional talent for playing the piano.", exZh: "這名孩子展現出了非凡的彈鋼琴天賦。" },
            { word: "exception", phonetic: "/ɪkˈsep.ʃən/", pos: "n.", chinese: "例外", level: "L3", exEn: "Most museums ban flash photography inside galleries without exception.", exZh: "大多數博物館都嚴格禁止在展覽館內使用閃光燈，概無例外。" },
            { word: "strategic", phonetic: "/strəˈtiː.dʒɪk/", pos: "adj.", chinese: "戰略的；戰略性的", level: "L3", exEn: "Thanks to its strategic position, rival powers frequently tried to control the port.", exZh: "由於其戰略位置重要，競爭強國經常試圖控制該港口。" },
            { word: "strategy", phonetic: "/ˈstræt.ə.dʒi/", pos: "n.", chinese: "策略", level: "L3", exEn: "The government adopted a long-term strategy for reducing unemployment.", exZh: "政府採取了降低失業率的長期策略。" },
            { word: "opposite", phonetic: "/ˈɑː.pə.zɪt/", pos: "adj./prep.", chinese: "相反的；對面的", level: "L3", exEn: "Our hotel is on the opposite side of town, so it takes time to get there.", exZh: "我們的飯店在城鎮的另一邊，所以去那裡需要一些時間。" },
            { word: "core", phonetic: "/kɔːr/", pos: "n./adj.", chinese: "核心；果核", level: "L3", exEn: "At the core of this politician's policies is a deep desire for equality.", exZh: "這位政治家政策的核心是對平等的渴望。" },
            { word: "considerably", phonetic: "/kənˈsɪd.ɚ.ə.bli/", pos: "adv.", chinese: "相當大（或多）地", level: "L3", exEn: "The baby has grown considerably since we last saw her three months ago.", exZh: "自從我們三個月前見到她以來，這嬰兒長大了許多。" },
            { word: "considerable", phonetic: "/kənˈsɪd.ɚ.ə.bəl/", pos: "adj.", chinese: "相當大的；相當多的", level: "L3", exEn: "The tiger's considerable size and strength impressed all the zoo visitors.", exZh: "老虎巨大的體型和力量給所有動物園遊客留下了深刻印象。" },
            { word: "densely", phonetic: "/ˈdens.li/", pos: "adv.", chinese: "密集地", level: "L3", exEn: "The park contains densely planted trees that create cool shade during summer.", exZh: "公園裡樹木茂密，在炎熱的夏天提供了涼爽的樹蔭。" },
            { word: "dense", phonetic: "/dens/", pos: "adj.", chinese: "密集的；濃重的", level: "L3", exEn: "Taipei has a very dense population with thousands of people per square kilometer.", exZh: "台北市人口非常稠密，每平方公里有數千人。" },
            { word: "density", phonetic: "/ˈden.sə.t̬i/", pos: "n.", chinese: "密度；濃度", level: "L3", exEn: "The density of water changes depending on temperature and salt level.", exZh: "水的密度會隨著溫度和鹽度的變化而改變。" },
            { word: "majestic", phonetic: "/məˈdʒes.tɪk/", pos: "adj.", chinese: "雄偉的；壯觀的", level: "L3", exEn: "The majestic castle sits atop a steep hill overlooking the whole city.", exZh: "雄偉的城堡座落在山頂，俯瞰整個城市。" },
            { word: "majesty", phonetic: "/ˈmædʒ.ə.sti/", pos: "n.", chinese: "雄偉壯觀；莊嚴", level: "L3", exEn: "Explorers were struck dumb by the sheer majesty of the enormous waterfall.", exZh: "探險家們被這座巨大瀑布的雄偉壯觀驚呆了。" },
            { word: "soar", phonetic: "/sɔːr/", pos: "v.", chinese: "高聳；急升", level: "L3", exEn: "The company's stock soared to new heights after releasing the breakthrough product.", exZh: "在發布突破性產品後，該公司的股票暴漲至新高。" },
            { word: "valley", phonetic: "/ˈvæl.i/", pos: "n.", chinese: "山谷；溪谷", level: "L3", exEn: "Even from down in the valley, climbers could see the majestic snow peaks.", exZh: "即使從山谷低處，登山者也能看到雄偉的雪峰。" },
            { word: "stream", phonetic: "/striːm/", pos: "n./v.", chinese: "溪流；流動", level: "L3", exEn: "There are plenty of fish swimming in the small streams running through this forest.", exZh: "穿過這片森林的小溪裡有許多魚在游動。" },
            { word: "stark", phonetic: "/stɑːrk/", pos: "adj.", chinese: "明顯的；簡陋的", level: "L3", exEn: "Dan noticed a stark difference between his health before and after the treatment.", exZh: "丹注意到治療前後健康狀況有著鮮明的差異。" },
            { word: "lively", phonetic: "/ˈlaɪv.li/", pos: "adj.", chinese: "熱烈的；充滿活力的", level: "L3", exEn: "The professor's question prompted a lively discussion among the students.", exZh: "教授提出的問題引發了學生們熱烈的討論。" },
            { word: "cattle", phonetic: "/ˈkæt.əl/", pos: "n.", chinese: "牛（複數）", level: "L3", exEn: "The farmer led a large herd of cattle across the country road.", exZh: "農夫帶領著一大群牛穿過鄉間小路。" },
            { word: "agricultural", phonetic: "/ˌæɡ.rəˈkʌl.tʃɚ.əl/", pos: "adj.", chinese: "農業的", level: "L3", exEn: "Thanks to its rich fertile land, the region has a lively agricultural trade.", exZh: "得益於肥沃的土地，該地區農業貿易十分活躍。" },
            { word: "exceed", phonetic: "/ɪkˈsiːd/", pos: "v.", chinese: "超過；超出", level: "L3", exEn: "Billy exceeded his parents' expectations by earning an A on his final exam.", exZh: "比利在期末考試中獲得A，超出了父母的預期。" },
            { word: "gorgeous", phonetic: "/ˈɡɔːr.dʒəs/", pos: "adj.", chinese: "絢麗的；美麗的", level: "L3", exEn: "Jane bought a gorgeous evening dress for the upcoming dance performance.", exZh: "珍為即將到來的舞蹈表演買了一件絢麗的晚禮服。" },
            { word: "lush", phonetic: "/lʌʃ/", pos: "adj.", chinese: "茂盛的；繁茂的", level: "L3", exEn: "The island's lush vegetation is a result of abundant rain throughout the year.", exZh: "該島茂盛的植被是全年降雨充足的結果。" },
            { word: "bustling", phonetic: "/ˈbʌs.lɪŋ/", pos: "adj.", chinese: "繁忙的；熙熙攘攘的", level: "L3", exEn: "On weekends, the city market is bustling with food vendors and shoppers.", exZh: "每逢週末，城市市場上攤販和購物者絡繹不絕，十分熱鬧。" },
            { word: "bustle", phonetic: "/ˈbʌs.əl/", pos: "v.", chinese: "忙碌；奔忙", level: "L3", exEn: "With so much to prepare, the assistants bustled around the hall before the show.", exZh: "表演前有許多準備工作，助理們在大廳裡忙個不停。" },
            { word: "mighty", phonetic: "/ˈmaɪ.t̬i/", pos: "adj.", chinese: "巨大的；強而有力的", level: "L3", exEn: "The mighty oak tree towered above all other plants in the ancient forest.", exZh: "那棵巨大雄偉的橡樹矗立在古老森林的所有植物之上。" },
            { word: "mountainous", phonetic: "/ˈmaʊn.tə.nəs/", pos: "adj.", chinese: "多山的", level: "L3", exEn: "This is a highly mountainous region, making road construction very difficult.", exZh: "這是一個多山地區，使得道路建設非常困難。" },
            { word: "take a look at", phonetic: "/teɪk ə lʊk æt/", pos: "phr.", chinese: "檢視；瞧一瞧", level: "L3", exEn: "Let me take a look at the report before we send it to the manager.", exZh: "在我們發送給經理之前，讓我先瞧一瞧這份報告。" },
            { word: "be worlds apart", phonetic: "/bi wɝːldz əˈpɑːrt/", pos: "phr.", chinese: "截然不同；天壤之別", level: "L3", exEn: "The two brothers are worlds apart in terms of personality and hobbies.", exZh: "兄弟倆在性格和興趣嗜好方面天壤之別。" },
            { word: "by contrast", phonetic: "/baɪ ˈkɑːn.træst/", pos: "phr.", chinese: "相比之下", level: "L3", exEn: "Tom is very introverted; by contrast, his brother is extremely outgoing.", exZh: "湯姆很內向；相比之下，他的弟弟非常外向。" },
            { word: "make up", phonetic: "/meɪk ʌp/", pos: "phr.", chinese: "形成；構成", level: "L3", exEn: "Eleven players make up a full team on the soccer field.", exZh: "足球場上由十一名球員組成一支完整的球隊。" },
            { word: "from far and wide", phonetic: "/frɑːm fɑːr ænd waɪd/", pos: "phr.", chinese: "從四處；從各處", level: "L3", exEn: "Music lovers traveled from far and wide to attend the annual rock festival.", exZh: "音樂愛好者從四面八方前來參加一年一度的搖滾音樂節。" },
            { word: "make for", phonetic: "/meɪk fɔːr/", pos: "phr.", chinese: "促成；造成", level: "L3", exEn: "The warm sunshine and ocean breeze made for a delightful afternoon at the beach.", exZh: "溫暖的陽光和海風促成了在海灘上度過的一個愉快的下午。" },

            // L4
            { word: "primitive", phonetic: "/ˈprɪm.ə.t̬ɪv/", pos: "adj.", chinese: "原始的；早期的", level: "L4", exEn: "Thousands of years ago, primitive humans made tools out of stone and bones.", exZh: "幾千年前，原始人類用石頭和骨頭製作工具。" },
            { word: "fiber", phonetic: "/ˈfaɪ.bɚ/", pos: "n.", chinese: "纖維", level: "L4", exEn: "Clothing made of natural fibers like cotton feels much more comfortable.", exZh: "由棉花等天然纖維製成的衣服感覺舒服得多。" },
            { word: "tissue", phonetic: "/ˈtɪʃ.uː/", pos: "n.", chinese: "組織；紙巾", level: "L4", exEn: "In the accident, Mary suffered minor damage to her muscle tissue.", exZh: "在事故中，瑪麗的肌肉組織受到了輕微損傷。" },
            { word: "provoke", phonetic: "/prəˈvoʊk/", pos: "v.", chinese: "激起；引起", level: "L4", exEn: "He says mean things because he enjoys provoking unnecessary arguments.", exZh: "他說話很刻薄，因為他喜歡挑起不必要的爭端。" },
            { word: "recent", phonetic: "/ˈriː.sənt/", pos: "adj.", chinese: "最近的；近來的", level: "L4", exEn: "John's recent visit to the clinic led him to change his daily diet.", exZh: "約翰最近去診所就診促使他改變了日常飲食。" },
            { word: "substantial", phonetic: "/səbˈstæn.ʃəl/", pos: "adj.", chinese: "大量的；重大的", level: "L4", exEn: "Gold and diamonds are valuable materials of substantial market worth.", exZh: "黃金和鑽石是具有重大市場價值的昂貴材料。" },
            { word: "unethical", phonetic: "/ʌnˈeθ.ɪ.kəl/", pos: "adj.", chinese: "不道德的", level: "L4", exEn: "It is highly unethical to perform experiments on humans without permission.", exZh: "未經許可對人類進行實驗是非常不道德的。" },
            { word: "ethical", phonetic: "/ˈeθ.ɪ.kəl/", pos: "adj.", chinese: "道德的；合乎倫理的", level: "L4", exEn: "This medical company holds high ethical standards for drug research.", exZh: "這家醫療公司在藥物研究方面保持著高度的道德標準。" },
            { word: "ethics", phonetic: "/ˈeθ.ɪks/", pos: "n.", chinese: "道德準則；倫理學", level: "L4", exEn: "Many people question the business ethics of selling misleading goods.", exZh: "許多人質疑銷售誤導性商品的商業道德。" },
            { word: "torment", phonetic: "/ˈtɔːr.ment/", pos: "n./v.", chinese: "折磨；痛苦", level: "L4", exEn: "During the funeral, he sank to his knees in severe mental torment.", exZh: "在葬禮期間，他陷入了巨大的精神折磨並跪倒在地。" },
            { word: "livestock", phonetic: "/ˈlaɪv.stɑːk/", pos: "n.", chinese: "牲畜；家畜", level: "L4", exEn: "The agricultural show features a livestock market for cattle and sheep.", exZh: "農業展覽會設有牛羊等家畜市場。" },
            { word: "toll", phonetic: "/toʊl/", pos: "n.", chinese: "傷亡人數；通行費", level: "L4", exEn: "After the earthquake, rescue workers tried to figure out the exact death toll.", exZh: "地震發生後，救援人員努力釐清確切的死亡人數。" },
            { word: "estimate", phonetic: "/ˈes.tə.meɪt/", pos: "v./n.", chinese: "估計；估算", level: "L4", exEn: "Engineers estimate that the new bridge project will cost fifty million dollars.", exZh: "工程師估計這項新橋樑工程將耗資五千萬美元。" },
            { word: "underestimate", phonetic: "/ˌʌn.dɚˈes.tə.meɪt/", pos: "v.", chinese: "低估", level: "L4", exEn: "Never underestimate the importance of regular exercise for your long-term health.", exZh: "永遠不要低估規律運動對長期健康的重要性。" },
            { word: "switch", phonetic: "/swɪtʃ/", pos: "v./n.", chinese: "轉變；開關", level: "L4", exEn: "Tom switched his mobile phone to silent mode during the official meeting.", exZh: "湯姆在正式會議期間將手機切換為靜音模式。" },
            { word: "vitamin", phonetic: "/ˈvaɪ.t̬ə.mɪn/", pos: "n.", chinese: "維他命；維生素", level: "L4", exEn: "Fresh oranges and lemons are rich sources of essential vitamin C.", exZh: "新鮮的柳橙和檸檬富含人體必需的維生素C。" },
            { word: "mineral", phonetic: "/ˈmɪn.ər.əl/", pos: "n.", chinese: "礦物質", level: "L4", exEn: "Spring water contains healthy minerals that are beneficial to your body.", exZh: "泉水含有對身體有益的健康礦物質。" },
            { word: "concern", phonetic: "/kənˈsɝːn/", pos: "n./v.", chinese: "擔心；關心", level: "L4", exEn: "The mayor expressed deep concern over rising traffic safety issues.", exZh: "市長對日益嚴重的交通安全問題表示深切關切。" },
            { word: "concerning", phonetic: "/kənˈsɝː.nɪŋ/", pos: "prep.", chinese: "關於；涉及", level: "L4", exEn: "The police asked for any information concerning the missing child.", exZh: "警方徵求任何有關失蹤兒童的線索。" },
            { word: "infrastructure", phonetic: "/ˈɪn.frəˌstrʌk.tʃɚ/", pos: "n.", chinese: "基礎建設", level: "L4", exEn: "Investing in transportation infrastructure helps boost the whole national economy.", exZh: "投資交通基礎建設有助於促進整個國家經濟。" },
            { word: "quantity", phonetic: "/ˈkwɑːn.t̬ə.t̬i/", pos: "n.", chinese: "數量", level: "L4", exEn: "Buying groceries in large quantities at warehouse stores can save money.", exZh: "在倉儲式賣場大量購買食品雜貨可以省錢。" },
            { word: "genetically", phonetic: "/dʒəˈnet̬.ɪ.kəl.i/", pos: "adv.", chinese: "由基因決定地", level: "L4", exEn: "An organism's innate traits are genetically determined at birth.", exZh: "生物的先天特徵在出生時就由基因決定了。" },
            { word: "genetic", phonetic: "/dʒəˈnet̬.ɪk/", pos: "adj.", chinese: "基因的；遺傳的", level: "L4", exEn: "Certain diseases are caused by complex genetic factors passed down in families.", exZh: "某些疾病是由家族遺傳的複雜基因因素引起的。" },
            { word: "gene", phonetic: "/dʒiːn/", pos: "n.", chinese: "基因", level: "L4", exEn: "Parents pass on copies of their genes to their biological children.", exZh: "父母將自己的基因複製傳給親生子女。" },
            { word: "latter", phonetic: "/ˈlæt̬.ɚ/", pos: "n./adj.", chinese: "後者；後者的", level: "L4", exEn: "Given a choice between tea and coffee, I prefer the latter.", exZh: "如果要在茶和咖啡之間選擇，我更喜歡後者。" },
            { word: "induce", phonetic: "/ɪnˈduːs/", pos: "v.", chinese: "引起；導致", level: "L4", exEn: "Some people drink warm milk before sleeping to induce relaxation.", exZh: "有些人睡前喝熱牛奶來誘導放鬆睡意。" },
            { word: "allergic", phonetic: "/əˈlɝː.dʒɪk/", pos: "adj.", chinese: "過敏的", level: "L4", exEn: "People who are allergic to peanuts must check food labels carefully.", exZh: "對花生過敏的人必須仔細檢查食品標籤。" },
            { word: "allergy", phonetic: "/ˈæl.ɚ.dʒi/", pos: "n.", chinese: "過敏反應", level: "L4", exEn: "She developed a severe allergy to cat fur during her childhood.", exZh: "她在童年時期對貓毛產生了嚴重的過敏反應。" },
            { word: "reaction", phonetic: "/riˈæk.ʃən/", pos: "n.", chinese: "生理反應；回應", level: "L4", exEn: "Tom's immediate reaction to the good news was pure joy and excitement.", exZh: "湯姆對這個好消息的第一反應是純粹的喜悅與興奮。" },
            { word: "react", phonetic: "/riˈækt/", pos: "v.", chinese: "起反應；回應", level: "L4", exEn: "How did the audience react to the director's surprise announcement?", exZh: "觀眾對導演的突如其來的宣布有何反應？" },
            { word: "somewhat", phonetic: "/ˈsʌm.hwɑːt/", pos: "adv.", chinese: "有點；有幾分", level: "L4", exEn: "Although James apologized, Mary still felt somewhat hurt by his words.", exZh: "雖然詹姆士道歉了，瑪麗對他的話依然感到有點受傷。" },
            { word: "alter", phonetic: "/ˈɑːl.tɚ/", pos: "v.", chinese: "改變；更動", level: "L4", exEn: "David usually alters his fashion style when entering a new environment.", exZh: "大衛進入新環境時通常會改變自己的時尚風格。" },
            { word: "suit", phonetic: "/suːt/", pos: "v.", chinese: "適合；合心意", level: "L4", exEn: "Working remote flexible hours suits her lifestyle as a busy mother.", exZh: "遠距彈性工作時間非常適合她作為忙碌母親的生活方式。" },
            { word: "suitable", phonetic: "/ˈsuː.t̬ə.bəl/", pos: "adj.", chinese: "合適的；適宜的", level: "L4", exEn: "Wearing jeans and sports shoes is not suitable for a formal evening banquet.", exZh: "穿牛仔褲和運動鞋不適合出席正式的晚宴。" },
            { word: "scale", phonetic: "/skeɪl/", pos: "n.", chinese: "規模；比例", level: "L4", exEn: "The factory manufactures vehicles on an impressively large scale.", exZh: "這家工廠以令人印象深刻的大規模製造車輛。" },
            { word: "comprehensive", phonetic: "/ˌkɑːm.prəˈhen.sɪv/", pos: "adj.", chinese: "全面的；詳盡的", level: "L4", exEn: "Researchers conducted a comprehensive survey covering thousands of participants.", exZh: "研究人員進行了一項涵蓋數千名參與者的全面調查。" },
            { word: "have at heart", phonetic: "/hæv æt hɑːrt/", pos: "phr.", chinese: "在意；關心", level: "L4", exEn: "Good teachers always have their students' educational success at heart.", exZh: "優秀的老師總是關心學生的學習成功。" },
            { word: "rest assured", phonetic: "/rest əˈʃʊrd/", pos: "phr.", chinese: "儘管放心", level: "L4", exEn: "Rest assured that we will deliver the products on time without delay.", exZh: "請儘管放心，我們會準時交付產品，絕不拖延。" },
            { word: "better still", phonetic: "/ˈbet.ɚ stɪl/", pos: "phr.", chinese: "更好的是", level: "L4", exEn: "Call him to explain, or better still, meet him in person to talk it over.", exZh: "打電話給他解釋，或者更好的是，當面找他談談。" },
            { word: "take a toll", phonetic: "/teɪk ə toʊl/", pos: "phr.", chinese: "造成惡果；造成傷害", level: "L4", exEn: "Working night shifts constantly will take a severe toll on your health.", exZh: "長期上夜班會對你的健康造成嚴重的傷害。" },
            { word: "in theory", phonetic: "/ɪn ˈθɪr.i/", pos: "phr.", chinese: "理論上", level: "L4", exEn: "In theory, the plan should work smoothly, but unexpected issues may arise.", exZh: "理論上這個計劃應該順利，但可能會出現意想不到的問題。" },
            { word: "at a price", phonetic: "/æt ə praɪs/", pos: "phr.", chinese: "付出代價", level: "L4", exEn: "Achieving top performance came at a price, as he sacrificed rest time.", exZh: "取得頂尖成績是有代價的，因為他犧牲了休息時間。" },
            { word: "jump to conclusions", phonetic: "/dʒʌmp tuː kənˈkluː.ʒənz/", pos: "phr.", chinese: "貿然斷定；下結論", level: "L4", exEn: "Don't jump to conclusions before collecting all facts and evidence.", exZh: "在收集到所有事實和證據之前，不要輕率下結論。" },
            { word: "keep an eye on", phonetic: "/kiːp æn aɪ ɑːn/", pos: "phr.", chinese: "照看；留意", level: "L4", exEn: "Please keep an eye on my luggage while I go to buy a water bottle.", exZh: "我去買瓶水時，請幫我照看我的行李。" },

            // L5
            { word: "prominent", phonetic: "/ˈprɑː.mə.nənt/", pos: "adj.", chinese: "著名的；重要的", level: "L5", exEn: "Kevin played a prominent role in founding the international technology company.", exZh: "凱文在創立這家國際科技公司中發揮了重要作用。" },
            { word: "commerce", phonetic: "/ˈkɑː.mɝːs/", pos: "n.", chinese: "貿易；商業", level: "L5", exEn: "Electronic commerce has expanded globally with convenient online shopping.", exZh: "隨著便捷的線上購物，電子商務在全球範圍內迅速擴張。" },
            { word: "tranquil", phonetic: "/ˈtræŋ.kwɪl/", pos: "adj.", chinese: "平靜的；安寧的", level: "L5", exEn: "They spent a relaxing vacation in a tranquil mountain village.", exZh: "他們在一個平靜寧靜的山村度過了放鬆的假期。" },
            { word: "vast", phonetic: "/væst/", pos: "adj.", chinese: "龐大的；廣闊的", level: "L5", exEn: "The desert stretches across a vast territory with few human settlements.", exZh: "沙漠延伸過一片廣袤無垠的土地，幾乎沒有人類定居。" },
            { word: "literally", phonetic: "/ˈlɪt̬.ɚ.əl.i/", pos: "adv.", chinese: "字面上地；真正地", level: "L5", exEn: "The phrase 'photography' literally means 'drawing with light' in Greek.", exZh: "「攝影」這個詞在希臘語中字面意思是「用光繪畫」。" },
            { word: "literal", phonetic: "/ˈlɪt̬.ɚ.əl/", pos: "adj.", chinese: "字面意義的", level: "L5", exEn: "Translate the passage naturally rather than giving a strict literal translation.", exZh: "自然地翻譯該段落，而不是給出嚴格的字面直譯。" },
            { word: "tide", phonetic: "/taɪd/", pos: "n.", chinese: "潮汐；潮水", level: "L5", exEn: "Walking along the shore at low tide is a pleasant evening activity.", exZh: "退潮時沿著海岸散步是一項令人愉快的傍晚活動。" },
            { word: "surge", phonetic: "/sɝːdʒ/", pos: "v./n.", chinese: "湧動；急速上升", level: "L5", exEn: "When the gates opened, excited crowds surged forward to see the singer.", exZh: "大門一開，興奮的人群便蜂擁向前去看那位歌手。" },
            { word: "likelihood", phonetic: "/ˈlaɪ.kli.hʊd/", pos: "n.", chinese: "可能性", level: "L5", exEn: "In all likelihood, it will rain soon as dark storm clouds gather above.", exZh: "十有八九很快就會下雨，因為烏雲已在上方聚集。" },
            { word: "solid", phonetic: "/ˈsɑː.lɪd/", pos: "adj.", chinese: "堅固的；固體的", level: "L5", exEn: "The house was built with a solid foundation made of concrete and iron.", exZh: "這棟房子建在由混凝土和鐵製成的堅固地基上。" },
            { word: "shift", phonetic: "/ʃɪft/", pos: "v./n.", chinese: "移動；改變", level: "L5", exEn: "There was a dramatic shift in public attitude towards solar energy.", exZh: "公眾對太陽能的態度發生了巨大轉變。" },
            { word: "beneath", phonetic: "/bɪˈniːθ/", pos: "prep.", chinese: "在……下方", level: "L5", exEn: "Around ninety percent of an iceberg's total volume lies hidden beneath the water.", exZh: "冰山總體積的大約百分之九十隱藏在水面下方。" },
            { word: "elevation", phonetic: "/ˌel.əˈveɪ.ʃən/", pos: "n.", chinese: "海拔；高度", level: "L5", exEn: "Air temperature decreases steadily as elevation above sea level increases.", exZh: "氣溫隨著海拔高度的增加而穩定下降。" },
            { word: "elevate", phonetic: "/ˈel.ə.veɪt/", pos: "v.", chinese: "提升；提拔", level: "L5", exEn: "Hard work and dedication helped elevate him to senior executive positions.", exZh: "努力工作和奉獻精神幫助他升至高階主管職位。" },
            { word: "elevator", phonetic: "/ˈel.ə.veɪ.t̬ɚ/", pos: "n.", chinese: "電梯", level: "L5", exEn: "Take the elevator to the eighth floor to find the administrative office.", exZh: "搭電梯到八樓即可找到行政辦公室。" },
            { word: "sustain", phonetic: "/səˈsteɪn/", pos: "v.", chinese: "維持；供養", level: "L5", exEn: "The thrilling storyline sustained the audience's interest until the end.", exZh: "扣人心弦的故事情節維持了觀眾的興趣直到最後。" },
            { word: "accelerate", phonetic: "/əkˈsel.ɚ.eɪt/", pos: "v.", chinese: "加速；促進", level: "L5", exEn: "The car accelerated rapidly when the driver pressed down on the pedal.", exZh: "當駕駛踩下油門時，汽車迅速加速。" },
            { word: "acceleration", phonetic: "/əkˌsel.ɚˈeɪ.ʃən/", pos: "n.", chinese: "加速；加速度", level: "L5", exEn: "There has been a sharp acceleration in technology adoption recently.", exZh: "最近科技應用的普及速度大幅加快。" },
            { word: "contend", phonetic: "/kənˈtend/", pos: "v.", chinese: "競爭；主張", level: "L5", exEn: "Ten top teams are contending for the national basketball championship.", exZh: "十支頂尖球隊正在爭奪全國籃球錦標賽冠軍。" },
            { word: "fuel", phonetic: "/ˈfjuː.əl/", pos: "n./v.", chinese: "燃料；加燃料", level: "L5", exEn: "The car ran out of fuel on the highway, forcing us to call towing.", exZh: "車子在公路上用光了燃料，迫使我們打電話叫拖車。" },
            { word: "excessive", phonetic: "/ɪkˈses.ɪv/", pos: "adj.", chinese: "過量的；過度的", level: "L5", exEn: "Excessive consumption of sugar can lead to health problems like diabetes.", exZh: "攝取過量的糖分會引發糖尿病等健康問題。" },
            { word: "melt", phonetic: "/melt/", pos: "v.", chinese: "融化；熔化", level: "L5", exEn: "The summer heat caused the ice in the lemonade glass to melt fast.", exZh: "夏日的炎熱使檸檬水玻璃杯中的冰塊迅速融化。" },
            { word: "polar", phonetic: "/ˈpoʊ.lɚ/", pos: "adj.", chinese: "極地的", level: "L5", exEn: "Polar bears rely on sea ice platforms to hunt for food in Arctic.", exZh: "北極熊依賴海冰平台在北極獵取食物。" },
            { word: "pole", phonetic: "/poʊl/", pos: "n.", chinese: "柱子；極點", level: "L5", exEn: "A wooden telephone pole was knocked down during the typhoon storm.", exZh: "颱風暴雨期間，一根木製電線桿被倒吹。" },
            { word: "overall", phonetic: "/ˌoʊ.vɚˈɑːl/", pos: "adj./adv.", chinese: "整體的；大體上", level: "L5", exEn: "Overall, the event was a huge success despite minor logistical delays.", exZh: "大體而言，儘管有一些小的後勤延誤，活動取得了巨大的成功。" },
            { word: "confirm", phonetic: "/kənˈfɝːm/", pos: "v.", chinese: "證實；確認", level: "L5", exEn: "New evidence confirmed the police's suspicion regarding the case.", exZh: "新證據證實了警方對該案的懷疑。" },
            { word: "incidence", phonetic: "/ˈɪn.sɪ.dəns/", pos: "n.", chinese: "發生率", level: "L5", exEn: "A reduced incidence of disease was reported following vaccination.", exZh: "據報導接種疫苗後疾病發生率有所下降。" },
            { word: "sweep", phonetic: "/swiːp/", pos: "v.", chinese: "席捲；打掃", level: "L5", exEn: "The hurricane swept across the coast, damaging thousands of homes.", exZh: "颶風席捲海岸，毀壞了數千棟房屋。" },
            { word: "extraordinary", phonetic: "/ɪkˈstrɔːr.də me.ri/", pos: "adj.", chinese: "非凡的；意想不到的", level: "L5", exEn: "The young musician demonstrated an extraordinary talent for composition.", exZh: "這名年輕音樂家展現了非凡的作曲天賦。" },
            { word: "outlook", phonetic: "/ˈaʊt.lʊk/", pos: "n.", chinese: "前景；展望", level: "L5", exEn: "The economic outlook for the next quarter remains promising.", exZh: "下季度的經濟前景依然看好。" },
            { word: "joint", phonetic: "/dʒɔɪnt/", pos: "adj./n.", chinese: "聯合的；接合處", level: "L5", exEn: "The two countries launched a joint space research initiative.", exZh: "兩國啟動了一項聯合太空研究計畫。" },
            { word: "curb", phonetic: "/kɝːb/", pos: "v./n.", chinese: "遏止；控制", level: "L5", exEn: "The government took measures to curb rising inflation prices.", exZh: "政府採取措施控制日益增長通膨物價。" },
            { word: "acknowledge", phonetic: "/əkˈnɑː.lɪdʒ/", pos: "v.", chinese: "承認；感謝", level: "L5", exEn: "He acknowledged his mistakes and offered a heartfelt public apology.", exZh: "他承認了自己的錯誤並公開誠摯道歉。" },
            { word: "acknowledgement", phonetic: "/əkˈnɑː.lɪdʒ.mənt/", pos: "n.", chinese: "承認；致謝", level: "L5", exEn: "The author wrote an acknowledgement to thank her editors.", exZh: "作者寫了一篇致謝詞感謝她的編輯們。" },
            { word: "generation", phonetic: "/ˌdʒen.əˈreɪ.ʃən/", pos: "n.", chinese: "世代；一代人", level: "L5", exEn: "We must preserve nature for the wellbeing of future generations.", exZh: "我們必須為了後代子孫的福祉而保護自然。" },
            { word: "set apart", phonetic: "/set əˈpɑːrt/", pos: "phr.", chinese: "使與眾不同", level: "L5", exEn: "Her exceptional discipline sets her apart from all other candidates.", exZh: "她卓越的自律使她從所有其他應徵者中脫穎而出。" },
            { word: "in consequence", phonetic: "/ɪn ˈkɑːn.sə.kwəns/", pos: "phr.", chinese: "結果；因此", level: "L5", exEn: "He broke traffic safety rules and was fined in consequence.", exZh: "他違反了交通規則，結果受到了罰款。" },
            { word: "contend with", phonetic: "/kənˈtend wɪð/", pos: "phr.", chinese: "應付；處理", level: "L5", exEn: "The new manager had many challenging problems to contend with.", exZh: "新經理有許多棘手的問題需要處理。" },
            { word: "result in", phonetic: "/rɪˈzʌlt ɪn/", pos: "phr.", chinese: "導致；造成", level: "L5", exEn: "Careless driving often results in serious road accidents.", exZh: "粗心駕駛往往導致嚴重的道路事故。" },
            { word: "as a matter of fact", phonetic: "/æz ə ˈmæt̬.ɚ ʌv fækt/", pos: "phr.", chinese: "事實上", level: "L5", exEn: "As a matter of fact, I have visited that town twice before.", exZh: "事實上，我以前去過那個小鎮兩次。" },
            { word: "owing to", phonetic: "/ˈoʊ.ɪŋ tuː/", pos: "phr.", chinese: "因為；由於", level: "L5", exEn: "The match was postponed owing to heavy thunderstorm rain.", exZh: "比賽由於雷暴大雨而延期。" },
            { word: "far from", phonetic: "/fɑːr frɑːm/", pos: "phr.", chinese: "完全不……", level: "L5", exEn: "The current test result is far from satisfactory for the teacher.", exZh: "目前的測試結果對老師來說完全不滿意。" },

            // L6
            { word: "march", phonetic: "/mɑːrtʃ/", pos: "v./n.", chinese: "遊行示威；行進", level: "L6", exEn: "Thousands of citizens marched through city streets to protest corruption.", exZh: "數千名市民在城市街頭遊行抗議腐敗。" },
            { word: "memorial", phonetic: "/məˈmɔːr.i.əl/", pos: "n./adj.", chinese: "紀念物；紀念的", level: "L6", exEn: "A magnificent statue was built as a memorial to war heroes.", exZh: "建造了一座雄偉的雕像作為戰鬥英雄的紀念碑。" },
            { word: "outline", phonetic: "/ˈaʊt.laɪn/", pos: "v./n.", chinese: "概述；大綱", level: "L6", exEn: "The director outlined the main objectives of the research project.", exZh: "主任概述了該研究項目的主要目標。" },
            { word: "racism", phonetic: "/ˈreɪ.sɪ.zəm/", pos: "n.", chinese: "種族歧視", level: "L6", exEn: "Societies worldwide continue fighting against systemic racism and bias.", exZh: "全世界社會繼續與系統性種族主義和偏見作鬥爭。" },
            { word: "racist", phonetic: "/ˈreɪ.sɪst/", pos: "n./adj.", chinese: "種族主義者", level: "L6", exEn: "The organization strictly bans making offensive racist comments online.", exZh: "該組織嚴格禁止在網絡上發表具冒犯性的種族主義言論。" },
            { word: "racial", phonetic: "/ˈreɪ.ʃəl/", pos: "adj.", chinese: "種族的", level: "L6", exEn: "The civil rights movement aimed to eliminate racial discrimination laws.", exZh: "民權運動旨在消除種族歧視法律。" },
            { word: "decisive", phonetic: "/dɪˈsaɪ.sɪv/", pos: "adj.", chinese: "決定性的；果斷的", level: "L6", exEn: "His late goal proved to be the decisive factor in winning the match.", exZh: "他在最後時刻的進球證明是贏得比賽的決定性因素。" },
            { word: "civil", phonetic: "/ˈsɪv.əl/", pos: "adj.", chinese: "公民的；民事的", level: "L6", exEn: "Citizens are entitled to fundamental civil rights under federal law.", exZh: "根據聯邦法律，公民享有基本民權。" },
            { word: "grant", phonetic: "/ɡrænt/", pos: "v./n.", chinese: "授予；補助金", level: "L6", exEn: "The university granted him a research scholarship to study abroad.", exZh: "大學授予他出國留學的研究獎學金。" },
            { word: "flame", phonetic: "/fleɪm/", pos: "n.", chinese: "火焰", level: "L6", exEn: "The engine caught fire and burst into bright orange flames.", exZh: "引擎著火並爆發出亮橘色的火焰。" },
            { word: "wither", phonetic: "/ˈwɪð.ɚ/", pos: "v.", chinese: "枯萎；衰退", level: "L6", exEn: "Without sufficient water, flower petals wited under scorching sun.", exZh: "沒有足夠的水，花瓣在烈日下枯萎了。" },
            { word: "captivity", phonetic: "/kæpˈtɪv.ə.t̬i/", pos: "n.", chinese: "監禁；囚禁", level: "L6", exEn: "Wild animals raised in captivity often lose natural survival instincts.", exZh: "在圈養環境下長大的野生動物往往會失去天然生存本能。" },
            { word: "captive", phonetic: "/ˈkæp.tɪv/", pos: "n./adj.", chinese: "俘虜；被囚禁的", level: "L6", exEn: "The soldiers were taken captive during midnight enemy raid.", exZh: "士兵們在深夜敵人的襲擊中被俘虜。" },
            { word: "cripple", phonetic: "/ˈkrɪp.əl/", pos: "v.", chinese: "使殘廢；重創", level: "L6", exEn: "A severe knee injury crippled his professional football career permanently.", exZh: "嚴重的膝傷永久重創了他的職業足球生涯。" },
            { word: "segregation", phonetic: "/ˌseɡ.rəˈɡeɪ.ʃən/", pos: "n.", chinese: "隔離；分開", level: "L6", exEn: "The civil rights movement fought to abolish racial segregation.", exZh: "民權運動致力於廢除種族隔離制度。" },
            { word: "segregate", phonetic: "/ˈseɡ.rə.ɡeɪt/", pos: "v.", chinese: "隔離；劃分", level: "L6", exEn: "Schools used to segregate students based on race in past decades.", exZh: "在過去的幾十年裡，學校曾根據種族隔離學生。" },
            { word: "midst", phonetic: "/mɪdst/", pos: "n.", chinese: "中間；當中", level: "L6", exEn: "In the midst of the crisis, she remained calm and focused.", exZh: "在危機之中，她依然保持冷靜和專注。" },
            { word: "exile", phonetic: "/ˈek.saɪl/", pos: "n./v.", chinese: "流亡；放逐", level: "L6", exEn: "The political leader lived in exile for over twenty years overseas.", exZh: "這位政治領袖在海外流亡了二十多年。" },
            { word: "shameful", phonetic: "/ˈʃeɪm.fəl/", pos: "adj.", chinese: "可恥的", level: "L6", exEn: "Cheating on academic exams is considered shameful behavior.", exZh: "在學術考試中作弊被認為是可恥的行為。" },
            { word: "shame", phonetic: "/ʃeɪm/", pos: "n.", chinese: "羞恥；遺憾", level: "L6", exEn: "He felt deep shame after being caught fabricating survey data.", exZh: "他在被發現捏造調查數據後感到深深的羞恥。" },
            { word: "fatigue", phonetic: "/fəˈtiːɡ/", pos: "n.", chinese: "疲勞；倦怠", level: "L6", exEn: "Extreme fatigue forced the marathon runner to withdraw midway.", exZh: "極度疲勞迫使馬拉松選手在中途退出。" },
            { word: "mobility", phonetic: "/moʊˈbɪl.ə.t̬i/", pos: "n.", chinese: "流動性；活動力", level: "L6", exEn: "Higher education improves career mobility for young graduates.", exZh: "高等教育提高了年輕畢業生的職業流動性。" },
            { word: "mobile", phonetic: "/ˈmoʊ.bəl/", pos: "adj.", chinese: "可移動的", level: "L6", exEn: "Mobile devices allow workers to access email anywhere easily.", exZh: "行動裝置讓工作人員隨時隨地輕鬆閱讀電子郵件。" },
            { word: "rob", phonetic: "/rɑːb/", pos: "v.", chinese: "剝奪；搶劫", level: "L6", exEn: "The unexpected injury robbed the athlete of competing in Olympics.", exZh: "意外的受傷剝奪了這位運動員參加奧運會的機會。" },
            { word: "robber", phonetic: "/ˈrɑː.bɚ/", pos: "n.", chinese: "搶匪", level: "L6", exEn: "Armed robbers raided the bank and escaped in a black car.", exZh: "持槍搶匪襲擊了銀行並乘坐一輛黑色汽車逃跑。" },
            { word: "robbery", phonetic: "/ˈrɑː.bɚ.i/", pos: "n.", chinese: "搶劫案", level: "L6", exEn: "Police are investigating a street robbery that occurred last night.", exZh: "警方正在調查昨晚發生的一起街頭搶劫案。" },
            { word: "dignity", phonetic: "/ˈdɪɡ.nə.t̬i/", pos: "n.", chinese: "尊嚴；自尊", level: "L6", exEn: "Everyone deserves to be treated with respect and basic human dignity.", exZh: "每個人都值得被尊重和擁有基本的人類尊嚴。" },
            { word: "oppression", phonetic: "/əˈpreʃ.ən/", pos: "n.", chinese: "壓迫；暴政", level: "L6", exEn: "Citizens celebrated their newfound freedom after decades of oppression.", exZh: "經過數十年的壓迫，公民們慶祝新獲得的自由。" },
            { word: "oppress", phonetic: "/əˈpres/", pos: "v.", chinese: "壓迫；虐待", level: "L6", exEn: "Rulers who oppress people eventually face public rebellion.", exZh: "壓迫人民的統治者最終會面臨大眾的反抗。" },
            { word: "vicious", phonetic: "/ˈvɪʃ.əs/", pos: "adj.", chinese: "邪惡的；兇殘的", level: "L6", exEn: "The vicious attack drew immediate condemnation from global leaders.", exZh: "這起兇殘的襲擊立即遭到全球領袖的譴責。" },
            { word: "symphony", phonetic: "/ˈsɪm.fə.ni/", pos: "n.", chinese: "交響樂", level: "L6", exEn: "The orchestra performed Beethoven's famous Ninth Symphony beautifully.", exZh: "管弦樂團完美演繹了貝多芬著名的第九交響曲。" },
            { word: "eloquence", phonetic: "/ˈel.ə.kwəns/", pos: "n.", chinese: "雄辯；口才", level: "L6", exEn: "The lawyer's eloquence convinced jury members of the client's innocence.", exZh: "律師的雄辯說服了陪審團相信客戶的清白。" },
            { word: "eloquent", phonetic: "/ˈel.ə.kwənt/", pos: "adj.", chinese: "雄辯的；有說服力的", level: "L6", exEn: "She delivered an eloquent speech appealing for global climate action.", exZh: "她發表了一篇呼籲全球採取氣候行動的精彩演說。" },
            { word: "rhetorical", phonetic: "/rɪˈtɔːr.ɪ.kəl/", pos: "adj.", chinese: "修辭的", level: "L6", exEn: "He asked a rhetorical question that required no formal reply.", exZh: "他提出了一個不需要正式回答的反問句。" },
            { word: "rhetoric", phonetic: "/ˈret.ər.ɪk/", pos: "n.", chinese: "修辭學；修辭", level: "L6", exEn: "Political speeches are often filled with persuasive rhetoric.", exZh: "政治演說往往充斥著說服性的修辭。" },
            { word: "conscience", phonetic: "/ˈkɑːn.ʃəns/", pos: "n.", chinese: "良心；良知", level: "L6", exEn: "His clear conscience allowed him to sleep peacefully despite criticism.", exZh: "儘管受到批評，問心無愧讓他能安然入睡。" },
            { word: "warrior", phonetic: "/ˈwɔːr.i.ɚ/", pos: "n.", chinese: "戰士", level: "L6", exEn: "Brave warriors defended their kingdom against invading enemy forces.", exZh: "勇敢的戰士們保衛王國抵抗入侵的敵軍。" },
            { word: "go down in history", phonetic: "/ɡoʊ daʊn ɪn ˈhɪs.t̬ɚ.i/", pos: "phr.", chinese: "載入史冊", level: "L6", exEn: "This medical discovery will go down in history as a major breakthrough.", exZh: "這項醫學發現作為一項重大突破將載入史冊。" },
            { word: "as long as", phonetic: "/æz lɑːŋ æz/", pos: "phr.", chinese: "只要", level: "L6", exEn: "You can succeed as long as you maintain determination and persistence.", exZh: "只要你保持決心和堅持，你就能成功。" },
            { word: "be heavy with", phonetic: "/bi ˈhev.i wɪð/", pos: "phr.", chinese: "充滿（情緒/物）", level: "L6", exEn: "Her voice was heavy with emotion as she thanked her supporters.", exZh: "當她向支持者致謝時，聲音裡充滿了情緒。" },
            { word: "live out", phonetic: "/lɪv aʊt/", pos: "phr.", chinese: "實現（夢想）", level: "L6", exEn: "She worked tirelessly to live out her dream of becoming a doctor.", exZh: "她不知疲倦地工作，以實現自己成為醫生的夢想。" },
            { word: "join hands", phonetic: "/dʒɔɪn hændz/", pos: "phr.", chinese: "攜手合作", level: "L6", exEn: "Nations must join hands to battle global poverty and environmental risks.", exZh: "各國必須攜手合作抗擊全球貧困與環境風險。" },
            { word: "have yet to", phonetic: "/hæv jet tuː/", pos: "phr.", chinese: "尚未；還沒有", level: "L6", exEn: "Scientists have yet to find a cure for this rare biological condition.", exZh: "科學家尚未找到治療這種罕見生物狀況的方法。" },

            // 112 學測
            { word: "sticky", phonetic: "/ˈstɪk.i/", pos: "adj.", chinese: "黏的；棘手的", level: "112", exEn: "The hot summer weather made my skin feel uncomfortably sticky.", exZh: "炎熱的夏日天氣讓我的皮膚感到不舒服的黏膩。" },
            { word: "greasy", phonetic: "/ˈɡriː.si/", pos: "adj.", chinese: "油膩的", level: "112", exEn: "Eating too much greasy fast food is not good for your heart.", exZh: "吃太多油膩的速食對你的心臟不好。" },
            { word: "clumsy", phonetic: "/ˈklʌm.zi/", pos: "adj.", chinese: "笨拙的", level: "112", exEn: "The clumsy waiter accidentally dropped a tray full of drink glasses.", exZh: "笨拙的服務生不小心打破了一托盤的飲料玻璃杯。" },
            { word: "mighty", phonetic: "/ˈmaɪ.t̬i/", pos: "adj.", chinese: "強大的", level: "112", exEn: "The mighty river rushed through the deep rocky canyon.", exZh: "強大的河流奔騰穿過深邃的岩石峽谷。" },
            { word: "clap", phonetic: "/klæp/", pos: "v.", chinese: "拍手；鼓掌", level: "112", exEn: "The audience began to clap enthusiastically after the show ended.", exZh: "表演結束後觀眾開始熱烈鼓掌。" },
            { word: "toss", phonetic: "/tɑːs/", pos: "v.", chinese: "拋；扔", level: "112", exEn: "He decided to toss the old worn coin into the wishing fountain.", exZh: "他決定把那枚磨損的舊硬幣扔進許願池。" },
            { word: "pose", phonetic: "/poʊz/", pos: "v./n.", chinese: "擺姿勢；提出", level: "112", exEn: "Models posed for photographers on the red carpet event.", exZh: "模特兒們在紅毯活動上擺出姿勢讓攝影師拍照。" },
            { word: "snap", phonetic: "/snæp/", pos: "v.", chinese: "咔嚓折斷；快照", level: "112", exEn: "Dry tree branches snap easily under strong pressure force.", exZh: "乾樹枝在大力的施壓下很容易咔嚓折斷。" },
            { word: "siblings", phonetic: "/ˈsɪb.lɪŋz/", pos: "n.", chinese: "手足；兄弟姊妹", level: "112", exEn: "She gets along very well with all three of her siblings.", exZh: "她和她的三個兄弟姊妹都相處得非常好。" },
            { word: "bare", phonetic: "/ber/", pos: "adj.", chinese: "裸露的；赤裸的", level: "112", exEn: "Walking with bare feet on sandy warm beaches feels relaxing.", exZh: "赤腳走在溫暖的沙灘上感覺很放鬆。" },
            { word: "ancestors", phonetic: "/ˈæn.ses.tɚz/", pos: "n.", chinese: "祖先", level: "112", exEn: "Our ancestors built strong stone monuments thousands of years ago.", exZh: "我們的祖先在幾千年前建造了堅固的石碑。" },
            { word: "instructors", phonetic: "/ɪnˈstrʌk.tɚz/", pos: "n.", chinese: "教練；講師", level: "112", exEn: "Ski instructors guided beginners safely down snow slopes.", exZh: "滑雪教練安全地引導初學者滑下雪坡。" },
            { word: "blank", phonetic: "/blæŋk/", pos: "adj./n.", chinese: "空白 look", level: "112", exEn: "Fill in the blank spaces with appropriate word choices.", exZh: "請在空白處填入適當的詞彙選項。" },
            { word: "hollow", phonetic: "/ˈhɑː.loʊ/", pos: "adj.", chinese: "中空的；虛偽的", level: "112", exEn: "Old oak trees often have hollow trunks inside.", exZh: "老橡樹的樹幹內部往往是中空的。" },
            { word: "queer", phonetic: "/kwɪr/", pos: "adj.", chinese: "古怪的；奇特的", level: "112", exEn: "He noticed a queer smell coming from the deserted house basement.", exZh: "他注意到廢棄房屋地地下室傳出一股古怪的氣味。" },
            { word: "liability", phonetic: "/ˌlaɪ.əˈbɪl.ə.t̬i/", pos: "n.", chinese: "法律責任；劣勢", level: "112", exEn: "Company managers must minimize legal liability risks.", exZh: "公司管理者必須儘量減少法律責任風險。" },
            { word: "generosity", phonetic: "/ˌdʒen.əˈrɑː.sə.t̬i/", pos: "n.", chinese: "慷慨；大方", level: "112", exEn: "Her generosity helped build local community medical centers.", exZh: "她的慷慨解囊幫助籌建了當地社區醫療中心。" },
            { word: "integrity", phonetic: "/ɪnˈteɡ.rə.t̬i/", pos: "n.", chinese: "正直；誠實", level: "112", exEn: "Journalists should report news with uncompromising moral integrity.", exZh: "新聞記者報導新聞應保持不妥協的道德正直。" },
            { word: "sincerity", phonetic: "/sɪnˈser.ə.t̬i/", pos: "n.", chinese: "真誠；誠意", level: "112", exEn: "She expressed deep sincerity while offering an apology.", exZh: "她在道歉時表達了深刻的真誠。" },
            { word: "fraction", phonetic: "/ˈfræk.ʃən/", pos: "n.", chinese: "小部分；分數", level: "112", exEn: "Only a small fraction of applicants gained admission to college.", exZh: "只有一小部分申請者獲得了大考錄取。" },
            { word: "privilege", phonetic: "/ˈprɪv.əl.ɪdʒ/", pos: "n.", chinese: "特權；榮幸", level: "112", exEn: "It was a great privilege to interview the Nobel Peace Prize winner.", exZh: "專訪諾貝爾和平獎得主是一項極大的榮幸。" },
            { word: "recall", phonetic: "/rɪˈkɑːl/", pos: "v.", chinese: "回想；召回", level: "112", exEn: "I cannot recall where I parked my bicycle yesterday evening.", exZh: "我想不起來昨晚把自行車停在哪裡了。" },
            { word: "provoke", phonetic: "/prəˈvoʊk/", pos: "v.", chinese: "激怒；引發", level: "112", exEn: "Unfair treatment provoked public anger and peaceful protest.", exZh: "不公平的待遇引發了大眾的憤怒和平抗議。" },
            { word: "counter", phonetic: "/ˈkaʊn.t̬ɚ/", pos: "v./n.", chinese: "反駁；櫃檯", level: "112", exEn: "The speaker countered criticism with solid scientific data.", exZh: "演講者用紮實的科學數據反駁了批評。" },
            { word: "expose", phonetic: "/ɪkˈspoʊz/", pos: "v.", chinese: "暴露；揭露", level: "112", exEn: "Don't expose delicate skin to direct midday sunlight.", exZh: "不要將嬌嫩的皮膚暴露在中午的陽光直射下。" },
            { word: "convert", phonetic: "/kənˈvɝːt/", pos: "v.", chinese: "轉化；改變", level: "112", exEn: "Solar panels convert sunshine energy into electricity power.", exZh: "太陽能電池板將陽光能轉化為電能。" },
            { word: "crippled", phonetic: "/ˈkrɪp.əld/", pos: "adj.", chinese: "受創的；殘疾的", level: "112", exEn: "A storm crippled the regional transport network system.", exZh: "風暴重創了區域交通網路系統。" },
            { word: "accelerated", phonetic: "/əkˈsel.ɚ.eɪ.t̬ɪd/", pos: "adj.", chinese: "加速的", level: "112", exEn: "The project moved forward at an accelerated pace recently.", exZh: "該項目最近以加速的步伐推進。" },
            { word: "rendered", phonetic: "/ˈren.dɚd/", pos: "adj.", chinese: "致使；呈現的", level: "112", exEn: "An unexpected power outage rendered computers useless.", exZh: "突如其來的停電使電腦無法使用。" },
            { word: "ventured", phonetic: "/ˈven.tʃɚd/", pos: "v.", chinese: "冒險前往", level: "112", exEn: "Explorers ventured deep into uncharted dark forest caves.", exZh: "探險家們冒險進入未知的黑暗森林洞穴。" },
            { word: "choked", phonetic: "/tʃoʊkt/", pos: "v./adj.", chinese: "噎住；堵塞的", level: "112", exEn: "Thick black smoke choked the entire burning building hallway.", exZh: "濃濃的黑煙堵塞了整棟著火大樓的走廊。" },
            { word: "enclosed", phonetic: "/ɪnˈkloʊzd/", pos: "adj.", chinese: "隨信附上；封閉的", level: "112", exEn: "Please find the official document enclosed in the envelope.", exZh: "請查收信封內附上的正式文件。" },
            { word: "injected", phonetic: "/ɪnˈdʒek.tɪd/", pos: "v.", chinese: "注射；注入", level: "112", exEn: "Doctors injected medicine to ease the patient's acute pain.", exZh: "醫生注射了藥物以緩解患者的急性疼痛。" },
            { word: "undoubtedly", phonetic: "/ʌnˈdaʊ.t̬ɪd.li/", pos: "adv.", chinese: "毫無疑問地", level: "112", exEn: "She is undoubtedly one of the most talented authors of this era.", exZh: "毫無疑問她是這個時代最優秀的作者之一。" },
            { word: "roughly", phonetic: "/ˈrʌf.li/", pos: "adv.", chinese: "大約；粗略地", level: "112", exEn: "The trip will take roughly three hours by highway drive.", exZh: "走高速公路這趟旅程大約需要三個小時。" },
            { word: "understandably", phonetic: "/ˌʌn.dɚˈstæn.də.bli/", pos: "adv.", chinese: "合乎情理地", level: "112", exEn: "Parents were understandably worried about school safety issues.", exZh: "家長們對學校安全問題表示擔憂是可以理解的。" },
            { word: "supposedly", phonetic: "/səˈpoʊ.zɪd.li/", pos: "adv.", chinese: "據說；據假定", level: "112", exEn: "The new device is supposedly waterproof under ocean depth.", exZh: "據說這種新裝置在深海下具有防水功能。" },

            // 113 學測
            { word: "spicy", phonetic: "/ˈspaɪ.si/", pos: "adj.", chinese: "辛辣的", level: "113", exEn: "Thai food is famous for spicy and flavorful herbs.", exZh: "泰國美食以辛辣和美味的香草而聞名。" },
            { word: "slender", phonetic: "/ˈslen.dɚ/", pos: "adj.", chinese: "苗條的；修長的", level: "113", exEn: "People who desire a slender figure should exercise regularly.", exZh: "渴望苗條身材的人應該定期運動。" },
            { word: "slight", phonetic: "/slaɪt/", pos: "adj.", chinese: "輕微的", level: "113", exEn: "There was a slight delay in train arrival times.", exZh: "火車到達時間略有延誤。" },
            { word: "slippery", phonetic: "/ˈslɪp.ɚ.i/", pos: "adj.", chinese: "滑的", level: "113", exEn: "Wet tiles after rain can become very slippery.", exZh: "雨後濕滑的地磚會變得非常滑。" },
            { word: "emerging", phonetic: "/ɪˈmɝː.dʒɪŋ/", pos: "v./adj.", chinese: "浮現；新興的", level: "113", exEn: "Watching sun emerging from sea clouds is a famous activity.", exZh: "觀賞太陽從雲海中升起是一項著名的活動。" },
            { word: "flashing", phonetic: "/ˈflæʃ.ɪŋ/", pos: "v.", chinese: "閃爍", level: "113", exEn: "Emergency lights were flashing brightly in night street.", exZh: "夜間街頭緊急燈閃爍個不停。" },
            { word: "rushing", phonetic: "/ˈrʌʃ.ɪŋ/", pos: "v.", chinese: "急衝", level: "113", exEn: "Water was rushing down mountain stream paths.", exZh: "水流沿著山間溪流急衝而下。" },
            { word: "floating", phonetic: "/ˈfloʊ.t̬ɪŋ/", pos: "v.", chinese: "漂浮", level: "113", exEn: "White clouds were floating peacefully in blue sky.", exZh: "朵朵白雲在藍天中平靜地飄浮。" },
            { word: "apt", phonetic: "/æpt/", pos: "adj.", chinese: "易於……的；恰當的", level: "113", exEn: "Careless drivers are apt to make mistakes in heavy traffic.", exZh: "粗心的駕駛在交通繁忙時容易出錯。" },
            { word: "due", phonetic: "/duː/", pos: "adj.", chinese: "預期的；到期的", level: "113", exEn: "Do you know what time the next bus is due to arrive?", exZh: "你知道下一班公車預計什麼時候到達嗎？" },
            { word: "bound", phonetic: "/baʊnd/", pos: "adj.", chinese: "必然的；前往的", level: "113", exEn: "With hard practice, you are bound to pass the exam.", exZh: "經過刻苦的練習，你一定能通過考試。" },
            { word: "docked", phonetic: "/dɑːkt/", pos: "v.", chinese: "靠港；停靠", level: "113", exEn: "The ship docked safely at harbor bay yesterday.", exZh: "這艘船昨天安全停靠在港灣。" },
            { word: "density", phonetic: "/ˈden.sə.t̬i/", pos: "n.", chinese: "密度", level: "113", exEn: "High population density challenges urban planning design.", exZh: "高人口密度對城市規劃設計提出挑戰。" },
            { word: "humidity", phonetic: "/hjuːˈmɪd.ə.t̬i/", pos: "n.", chinese: "濕度", level: "113", exEn: "High humidity makes summer heat feel much stickier.", exZh: "高濕度使夏季炎熱感覺更加黏膩。" },
            { word: "circulation", phonetic: "/ˌsɝː.kjəˈleɪ.ʃən/", pos: "n.", chinese: "血液循環；流通", level: "113", exEn: "Regular walking exercise improves blood circulation in legs.", exZh: "定期步行運動可以改善腿部的血液循環。" },
            { word: "atmosphere", phonetic: "/ˈæt.məs.fɪr/", pos: "n.", chinese: "大氣；氣氛", level: "113", exEn: "The Earth's atmosphere protects surface life from radiation.", exZh: "地球的大氣層保護表面生命免受輻射。" },
            { word: "stay", phonetic: "/steɪ/", pos: "v.", chinese: "停留", level: "113", exEn: "Stay focused on goal tasks during class hours.", exZh: "在課堂時間保持專注於目標任務。" },
            { word: "take", phonetic: "/teɪk/", pos: "v.", chinese: "拿取；採取", level: "113", exEn: "Take safety precautions before operating heavy machinery.", exZh: "操作重型機械前採取安全預防措施。" },
            { word: "serve", phonetic: "/sɝːv/", pos: "v.", chinese: "服務；充當", level: "113", exEn: "Restaurants serve delicious traditional meals daily.", exZh: "餐廳每天供應美味的傳統菜餚。" },
            { word: "stand", phonetic: "/stænd/", pos: "v.", chinese: "站立；經受得起", level: "113", exEn: "Great artwork will no doubt stand the test of time.", exZh: "偉大的藝術作品無疑將經受住時間的考驗。" },
            { word: "forceful", phonetic: "/ˈfɔːrs.fəl/", pos: "adj.", chinese: "強有力的", level: "113", exEn: "She gave a forceful argument defending environmental rights.", exZh: "她發表了維護環境權利的強有力論據。" },
            { word: "realistic", phonetic: "/ˌriː.əˈlɪs.tɪk/", pos: "adj.", chinese: "現實的；寫實的", level: "113", exEn: "Set realistic goals when planning personal study schedules.", exZh: "在規劃個人學習進度表時要制定現實的目標。" },
            { word: "compulsory", phonetic: "/kəmˈpʌl.sɚ.i/", pos: "adj.", chinese: "強制性的；必修的", level: "113", exEn: "Military service is compulsory for adult young men in some countries.", exZh: "在某些國家成年年輕男子服兵役是強制性的。" },
            { word: "distinctive", phonetic: "/dɪˈstɪŋk.tɪv/", pos: "adj.", chinese: "獨特的；有特色的", level: "113", exEn: "The bird has a distinctive blue feather color pattern.", exZh: "這種鳥具有獨特的藍色羽毛顏色圖案。" },
            { word: "advantage", phonetic: "/ədˈvæn.t̬ɪdʒ/", pos: "n.", chinese: "優勢；利益", level: "113", exEn: "Knowing foreign languages gives job market advantages.", exZh: "掌握外語能提供就業市場優勢。" },
            { word: "revenge", phonetic: "/rɪˈvenʒ/", pos: "n.", chinese: "報復；復仇", level: "113", exEn: "Seeking personal revenge will not solve long conflicts.", exZh: "尋求個人報復並不能解決長期的衝突。" },
            { word: "remedy", phonetic: "/ˈrem.ə.di/", pos: "n.", chinese: "補救方法；藥方", level: "113", exEn: "Lemon honey tea is a common natural cold remedy.", exZh: "檸檬蜂蜜茶是常見的天然感冒偏方。" },
            { word: "credit", phonetic: "/ˈkred.ɪt/", pos: "n.", chinese: "功勞；信用", level: "113", exEn: "The leader took credit for all team work efforts.", exZh: "領導將所有團隊努力的功勞歸功於自己。" },
            { word: "proposes", phonetic: "/prəˈpoʊ.zɪz/", pos: "v.", chinese: "提議；建議", level: "113", exEn: "The mayor proposes building new urban parks.", exZh: "市長提議興建新的城市公園。" },
            { word: "contains", phonetic: "/kənˈteɪnz/", pos: "v.", chinese: "包含", level: "113", exEn: "The report contains important financial statistics data.", exZh: "該報告包含重要的財務統計數據。" },
            { word: "promises", phonetic: "/ˈprɑː.mɪ.sɪz/", pos: "v.", chinese: "承諾；有潛力", level: "113", exEn: "The hotel promises great service and transport convenience.", exZh: "這家飯店保證良好的服務和交通便利。" },
            { word: "confirms", phonetic: "/kənˈfɝːmz/", pos: "v.", chinese: "證實；確認", level: "113", exEn: "The test result confirms original medical diagnoses.", exZh: "測試結果證實了最初的醫療診斷。" },
            { word: "appeal", phonetic: "/əˈpiːl/", pos: "n./v.", chinese: "呼籲；吸引力", level: "113", exEn: "Hospitals make an urgent appeal for blood donations.", exZh: "醫院緊急呼籲大眾捐血。" },
            { word: "approach", phonetic: "/əˈproʊtʃ/", pos: "n./v.", chinese: "方法；靠近", level: "113", exEn: "We need a innovative approach to solve environmental problems.", exZh: "我們需要創新的方法來解決環境問題。" },
            { word: "operation", phonetic: "/ˌɑː.pəˈreɪ.ʃən/", pos: "n.", chinese: "手術；運作", level: "113", exEn: "The surgical operation was performed successfully by doctors.", exZh: "外科手術由醫生成功完成。" },
            { word: "observation", phonetic: "/ˌɑːb.zɚˈveɪ.ʃən/", pos: "n.", chinese: "觀察", level: "113", exEn: "Scientific scientific laws are derived from careful observation.", exZh: "科學規律源於仔細的觀察。" },
            { word: "eligibly", phonetic: "/ˈel.ɪ.dʒə.bli/", pos: "adv.", chinese: "合格地", level: "113", exEn: "Applicants who registered eligibly entered prize draws.", exZh: "符合條件註冊的申請者進入了抽獎。" },
            { word: "randomly", phonetic: "/ˈræn.dəm.li/", pos: "adv.", chinese: "隨機地", level: "113", exEn: "Students were randomly placed into study project groups.", exZh: "學生被隨機分配到學習小組中。" },
            { word: "apparently", phonetic: "/əˈper.ənt.li/", pos: "adv.", chinese: "顯然地", level: "113", exEn: "He was apparently unaware of safety rule changes.", exZh: "他顯然沒有意識到安全規則的改變。" },
            { word: "consequently", phonetic: "/ˈkɑːn.sə.kwənt.li/", pos: "adv.", chinese: "結果；因此", level: "113", exEn: "He missed alarm and consequently arrived late for work.", exZh: "他錯過了鬧鐘，結果上班遲到了。" },

            // 114 學測
            { word: "border", phonetic: "/ˈbɔːr.dɚ/", pos: "n.", chinese: "邊界；邊框", level: "114", exEn: "Guards check passports at the international border line crossing.", exZh: "警衛在國際邊境過境處檢查護照。" },
            { word: "timer", phonetic: "/ˈtaɪ.mɚ/", pos: "n.", chinese: "計時器", level: "114", exEn: "Set a kitchen timer when baking cookies in oven.", exZh: "在烤箱烤餅乾時設置廚房計時器。" },
            { word: "container", phonetic: "/kənˈteɪ.nɚ/", pos: "n.", chinese: "容器", level: "114", exEn: "Put a container under a leaking faucet to collect water drops.", exZh: "在漏水的水頭下放一個容器來收集水滴。" },
            { word: "marker", phonetic: "/ˈmɑːr.kɚ/", pos: "n.", chinese: "標記；馬克筆", level: "114", exEn: "Use a red marker to highlight key textbook terms.", exZh: "使用紅色馬克筆突出顯示教科書關鍵術語。" },
            { word: "produce", phonetic: "/ˈproʊ.duːs/", pos: "n.", chinese: "農產品", level: "114", exEn: "Farmers market offers fresh seasonal produce directly to customers.", exZh: "農民市場直接向客戶提供新鮮的季節性農產品。" },
            { word: "fashion", phonetic: "/ˈfæʃ.ən/", pos: "n.", chinese: "時尚；方式", level: "114", exEn: "Trends in clothing fashion change constantly each year.", exZh: "服裝時尚趨勢每年都在不斷變化。" },
            { word: "brand", phonetic: "/brænd/", pos: "n.", chinese: "品牌", level: "114", exEn: "They launched a new eco-friendly electronics product brand.", exZh: "他們推出了一個新的環保電子產品品牌。" },
            { word: "trend", phonetic: "/trend/", pos: "n.", chinese: "趨勢", level: "114", exEn: "There is a growing trend towards remote working online.", exZh: "遠距線上工作的趨勢日益增長。" },
            { word: "blurring", phonetic: "/blɝː.ɪŋ/", pos: "v.", chinese: "模糊", level: "114", exEn: "Childhood memories are blurring as years pass by.", exZh: "隨著歲月的流逝童年的記憶正在變得模糊。" },
            { word: "trimming", phonetic: "/ˈtrɪm.ɪŋ/", pos: "v.", chinese: "修剪", level: "114", exEn: "Gardener was trimming tree hedge branches in park.", exZh: "園丁正在公園修剪樹籬枝條。" },
            { word: "draining", phonetic: "/ˈdreɪ.nɪŋ/", pos: "v.", chinese: "排出；消耗", level: "114", exEn: "Heavy mental stress was draining her physical energy.", exZh: "沉重的精神壓力正在消耗她的體力。" },
            { word: "glaring", phonetic: "/ˈɡler.ɪŋ/", pos: "adj.", chinese: "耀眼的；明顯的", level: "114", exEn: "Bright sunlight caused a glaring reflection on screen.", exZh: "明烈的陽光在螢幕上造成了刺眼的反射。" },
            { word: "excessive", phonetic: "/ɪkˈses.ɪv/", pos: "adj.", chinese: "過度的", level: "114", exEn: "Avoid excessive alcohol drink consumption for organ health.", exZh: "為了器官健康避免過度飲酒。" },
            { word: "furious", phonetic: "/ˈfjʊr.i.əs/", pos: "adj.", chinese: "狂怒的", level: "114", exEn: "He was furious when discovering his laptop broken.", exZh: "當發現筆記型電腦壞了時他怒不可遏。" },
            { word: "offensive", phonetic: "/əˈfen.sɪv/", pos: "adj.", chinese: "冒犯的；無禮的", level: "114", exEn: "Racist remarks are by nature offensive and hurtful.", exZh: "種族主義言論本質上是冒犯性和具傷害性的。" },
            { word: "stubborn", phonetic: "/ˈstʌb.ɚn/", pos: "adj.", chinese: "固執的", level: "114", exEn: "The stubborn boy refused to listen to advice.", exZh: "那個固執的男孩拒絕聽取建議。" },
            { word: "text", phonetic: "/tekst/", pos: "n.", chinese: "文本；正文", level: "114", exEn: "Read textbook lesson text carefully before taking quiz.", exZh: "小考前仔細閱讀課本課文正文。" },
            { word: "brush", phonetic: "/brʌʃ/", pos: "n.", chinese: "刷子；畫筆", level: "114", exEn: "Painter cleaned her paint brush with fresh water.", exZh: "畫家用水清洗了她的畫筆。" },
            { word: "draft", phonetic: "/dræft/", pos: "n.", chinese: "草稿", level: "114", exEn: "Mary revised the first draft of her essay several times.", exZh: "瑪麗將論文第一稿修改了數次。" },
            { word: "plot", phonetic: "/plɑːt/", pos: "n.", chinese: "情節；陰謀", level: "114", exEn: "The novel novel plot had unexpected turns and mystery.", exZh: "小說情節出現意想不到的轉折與謎團。" },
            { word: "casual", phonetic: "/ˈkæʒ.u.əl/", pos: "adj.", chinese: "便裝的；隨意的", level: "114", exEn: "Wear casual comfortable clothes for weekend outdoor picnics.", exZh: "週末戶外野餐請穿著休閒舒適的衣服。" },
            { word: "fragile", phonetic: "/ˈfrædʒ.əl/", pos: "adj.", chinese: "易碎的", level: "114", exEn: "Handle fragile antique glass vases with extreme care.", exZh: "小心處理易碎的古董玻璃花瓶。" },
            { word: "remote", phonetic: "/rɪˈmoʊt/", pos: "adj.", chinese: "偏遠的；遙遠的", level: "114", exEn: "They lived in a remote mountain village without internet.", exZh: "他們生活在一個沒有網路的偏遠山村。" },
            { word: "vacant", phonetic: "/ˈveɪ.kənt/", pos: "adj.", chinese: "空置的；空缺的", level: "114", exEn: "Left vacant for years, the old house was filled with dust.", exZh: "廢棄多年的老房子落滿了厚厚的灰塵。" },
            { word: "gigantic", phonetic: "/dʒaɪˈɡæn.t̬ɪk/", pos: "adj.", chinese: "巨大的", level: "114", exEn: "A gigantic ocean whale swam alongside peaceful boat.", exZh: "一頭巨大的海洋鯨魚沿著平靜的船隻游動。" },
            { word: "exclusive", phonetic: "/ɪkˈskluː.sɪv/", pos: "adj.", chinese: "獨家的；專屬的", level: "114", exEn: "The news reporter secured an exclusive interview story.", exZh: "新聞記者獲得了獨家採訪報導。" },
            { word: "multiple", phonetic: "/ˈmʌl.tə.pəl/", pos: "adj.", chinese: "多重的；多個的", level: "114", exEn: "The project choice question offers multiple answer options.", exZh: "該項目選擇問題提供多個答案選項。" },
            { word: "enormous", phonetic: "/ɪˈnɔːr.məs/", pos: "adj.", chinese: "巨大的；龐大的", level: "114", exEn: "The student showed enormous courage saving old man.", exZh: "學生救老人在火災中展現巨大勇氣。" },
            { word: "halted", phonetic: "/hɑːl.tɪd/", pos: "v.", chinese: "停止；中斷", level: "114", exEn: "Construction projects were halted due to budget cuts.", exZh: "建設項目由於預算縮減而中斷。" },
            { word: "hatched", phonetic: "/hætʃt/", pos: "v.", chinese: "孵化", level: "114", exEn: "Baby birds hatched safely inside tree nest eggs.", exZh: "幼鳥在樹巢蛋中安全孵化。" },
            { word: "possessed", phonetic: "/pəˈzest/", pos: "v.", chinese: "擁有", level: "114", exEn: "He possessed great athletic talent since young age.", exZh: "他從小就擁有非凡的運動天賦。" },
            { word: "reinforced", phonetic: "/ˌriː.ɪnˈfɔːrst/", pos: "v.", chinese: "強化；加固", level: "114", exEn: "Engineers reinforced sea walls against violent waves.", exZh: "工程師加固了海堤抵禦猛烈的浪花。" },
            { word: "praised", phonetic: "/preɪzd/", pos: "v.", chinese: "稱讚", level: "114", exEn: "Teachers praised student diligence work performance.", exZh: "老師讚揚了學生的勤奮工作表現。" },
            { word: "graced", phonetic: "/ɡreɪst/", pos: "v.", chinese: "使增光；親臨", level: "114", exEn: "The President graced graduation ceremony with presence.", exZh: "總統親臨畢業典禮使典禮增光彩。" },
            { word: "addressed", phonetic: "/əˈdrest/", pos: "v.", chinese: "演說；處理", level: "114", exEn: "The speaker addressed audience on climate issues.", exZh: "演講者就氣候問題向觀眾發表演說。" },
            { word: "credited", phonetic: "/ˈkred.ɪ.tɪd/", pos: "v.", chinese: "歸功於", level: "114", exEn: "He was credited with inventing breakthrough solar cells.", exZh: "發明突破性太陽能電池歸功於他。" },
            { word: "verbally", phonetic: "/ˈvɝː.bəl.i/", pos: "adv.", chinese: "言語上地", level: "114", exEn: "He was accused of verbally abusing coworkers.", exZh: "他被指控在言語上虐待同事。" },
            { word: "dominantly", phonetic: "/ˈdɑː.mə.nənt.li/", pos: "adv.", chinese: "占主導地位地", level: "114", exEn: "The team played dominantly throughout whole tournament.", exZh: "球隊在整個錦標賽中佔據主導地位。" },
            { word: "legitimately", phonetic: "/ləˈdʒɪt̬.ə.mət.li/", pos: "adv.", chinese: "合法地；合理地", level: "114", exEn: "He legitimately earned prize rewards through hard work.", exZh: "他通過努力合法地獲得獎品回報。" },
            { word: "relevantly", phonetic: "/ˈrel.ə.vənt.li/", pos: "adv.", chinese: "相關地", level: "114", exEn: "Answer interview questions relevantly and clearly.", exZh: "相關且清晰地回答面試問題。" },

            // 115 學測
            { word: "hasty", phonetic: "/ˈheɪ.sti/", pos: "adj.", chinese: "倉促的；草率的", level: "115", exEn: "Don't make hasty decisions without full careful thought.", exZh: "未經周密思考不要做出草率的決定。" },
            { word: "tight", phonetic: "/taɪt/", pos: "adj.", chinese: "緊湊的；緊密的", level: "115", exEn: "The mayor has such a tight schedule that interview takes weeks.", exZh: "市長日程安排非常緊湊，安排採訪需要數週。" },
            { word: "diligent", phonetic: "/ˈdɪl.ə.dʒənt/", pos: "adj.", chinese: "勤奮的", level: "115", exEn: "Diligent students review daily study vocabulary regular.", exZh: "勤奮的學生每天複習日常學習詞彙。" },
            { word: "routine", phonetic: "/ruːˈtiːn/", pos: "n./adj.", chinese: "常規；例行的", level: "115", exEn: "Morning exercise is part of her health routine life.", exZh: "晨練是她健康常規生活的一部分。" },
            { word: "official", phonetic: "/əˈfɪʃ.əl/", pos: "adj.", chinese: "官方的；正式的", level: "115", exEn: "The ministry released official government news statement.", exZh: "部會發表了政府官方新聞聲明。" },
            { word: "instant", phonetic: "/ˈɪn.stənt/", pos: "adj.", chinese: "立即的；速食的", level: "115", exEn: "Online shopping provides instant payment confirmation.", exZh: "網路購物提供即時付款確認。" },
            { word: "amateur", phonetic: "/ˈæm.ə.tʃɚ/", pos: "adj./n.", chinese: "業餘的；業餘愛好者", level: "115", exEn: "Jane started as an amateur artist before owning studio.", exZh: "珍在擁有工作室之前是一位業餘藝術家。" },
            { word: "elementary", phonetic: "/ˌel.əˈmen.t̬ɚ.i/", pos: "adj.", chinese: "初級的；基礎的", level: "115", exEn: "Children learn reading basics in elementary school.", exZh: "孩子們在小學學習閱讀基礎知識。" },
            { word: "career", phonetic: "/kəˈrɪr/", pos: "n.", chinese: "事業；生涯", level: "115", exEn: "She built a successful professional career in law.", exZh: "她在法律領域建立了成功的專業事業。" },
            { word: "vacancy", phonetic: "/ˈveɪ.kən.si/", pos: "n.", chinese: "空缺；空房", level: "115", exEn: "Teaching vacancy attracted many qualified applicants.", exZh: "教師職位空缺吸引了許多合格的申請人。" },
            { word: "expectation", phonetic: "/ˌek.spekˈteɪ.ʃən/", pos: "n.", chinese: "期望；預期", level: "115", exEn: "The movie performance exceeded audience high expectation.", exZh: "電影表演超出了觀眾的高期望。" },
            { word: "inspiration", phonetic: "/ˌɪn.spəˈreɪ.ʃən/", pos: "n.", chinese: "靈感", level: "115", exEn: "Nature beauty provided endless creative inspiration art.", exZh: "自然美景提供了源源不絕的藝術創作靈感。" },
            { word: "initially", phonetic: "/ɪˈnɪʃ.əl.i/", pos: "adv.", chinese: "最初地", level: "115", exEn: "Product created far more profit than initially expected.", exZh: "產品創造的利潤遠高於最初的預期。" },
            { word: "genuinely", phonetic: "/ˈdʒen.ju.ɪn.li/", pos: "adv.", chinese: "真誠地；真地", level: "115", exEn: "She was genuinely happy for friend competition win.", exZh: "她真心地為朋友獲勝感到高興。" },
            { word: "alternatively", phonetic: "/ɑːlˈtɝː.nə.t̬ɪv.li/", pos: "adv.", chinese: "或者；要不然", level: "115", exEn: "You can email us or alternatively call office direct.", exZh: "您可以給我們發送電子郵件或打電話給辦公室。" },
            { word: "fundamentally", phonetic: "/ˌfʌn.dəˈmen.t̬əl.i/", pos: "adv.", chinese: "根本地", level: "115", exEn: "Technology has fundamentally changed global communications.", exZh: "科技從根本上改變了全球通訊方式。" },
            { word: "dimension", phonetic: "/ˌdaɪˈmen.ʃən/", pos: "n.", chinese: "維度；尺寸", level: "115", exEn: "Measure length height dimension of room furniture.", exZh: "測量房間家具的長度高度尺寸。" },
            { word: "integration", phonetic: "/ˌɪn.t̬əˈɡreɪ.ʃən/", pos: "n.", chinese: "整合；融合", level: "115", exEn: "System integration improves office operational efficiency.", exZh: "系統整合提高了辦公室營運效率。" },
            { word: "provision", phonetic: "/prəˈvɪʒ.ən/", pos: "n.", chinese: "供應；條款", level: "115", exEn: "Emergency food supply provision saved storm victims.", exZh: "緊急食品供應救助了風暴災民。" },
            { word: "consumption", phonetic: "/kənˈsʌmp.ʃən/", pos: "n.", chinese: "消耗；食用", level: "115", exEn: "Balanced consumption of vegetables promotes health.", exZh: "平衡食用蔬菜促進健康。" },
            { word: "dreads", phonetic: "/dredz/", pos: "v.", chinese: "害怕；畏懼", level: "115", exEn: "Timmy dreads thought of speaking before large audience.", exZh: "提米害怕在大庭廣眾面前發言。" },
            { word: "stresses", phonetic: "/stres.ɪz/", pos: "v.", chinese: "強調", level: "115", exEn: "Teacher stresses importance of honest exam work.", exZh: "老師強調誠實考試的重要性。" },
            { word: "wanders", phonetic: "/ˈwɑːn.dɚz/", pos: "v.", chinese: "漫遊；漫步", level: "115", exEn: "He wanders along quiet garden paths in evening.", exZh: "他傍晚在安靜的花園小徑上漫步。" },
            { word: "escapes", phonetic: "/ɪˈskeɪps/", pos: "v.", chinese: "逃脫；逃避", level: "115", exEn: "Prisoner escapes from jail house during storm night.", exZh: "囚犯在暴風雨之夜逃脫監獄。" },
            { word: "resource", phonetic: "/ˈriː.sɔːrs/", pos: "n.", chinese: "資源", level: "115", exEn: "Water is precious natural resource essential for life.", exZh: "水是生命不可或缺的寶貴自然資源。" },
            { word: "impact", phonetic: "/ˈɪm.pækt/", pos: "n./v.", chinese: "影響；衝擊", level: "115", exEn: "Climate change has severe impact on global weather.", exZh: "氣候變遷對全球天氣產生嚴重影響。" },
            { word: "passion", phonetic: "/ˈpæʃ.ən/", pos: "n.", chinese: "熱情", level: "115", exEn: "Music is his life passion devoted whole heartedly.", exZh: "音樂是他全心全意奉獻的全心熱情。" },
            { word: "emphasis", phonetic: "/ˈem.fə.sɪs/", pos: "n.", chinese: "強調；重點", level: "115", exEn: "School places emphasis on practical language skills.", exZh: "學校強調實用語言技能。" },
            { word: "shattered", phonetic: "/ˈʃæt̬.ɚd/", pos: "v./adj.", chinese: "粉碎的", level: "115", exEn: "Broken window glass shattered across room floor.", exZh: "打破的窗戶玻璃碎落滿地。" },
            { word: "assaulted", phonetic: "/əˈsɑːl.tɪd/", pos: "v.", chinese: "襲擊；攻擊", level: "115", exEn: "Drunk driver assaulted passers by with bottles.", exZh: "醉酒司機用瓶子襲擊路人。" },
            { word: "overturned", phonetic: "/ˌoʊ.vɚˈtɝːnd/", pos: "v.", chinese: "翻覆", level: "115", exEn: "Truck overturned on highway blocking morning traffic.", exZh: "卡車在公路上翻覆阻礙了早晨交通。" },
            { word: "condemned", phonetic: "/kənˈdemd/", pos: "v.", chinese: "譴責", level: "115", exEn: "Public strongly condemned violent street behavior.", exZh: "公眾強烈譴責暴力街頭行為。" },
            { word: "crash", phonetic: "/kræʃ/", pos: "v./n.", chinese: "碰撞；墜毀", level: "115", exEn: "Car crash happened at busy traffic light intersection.", exZh: "汽車相撞事故發生在繁忙的紅綠燈路口。" },
            { word: "tumble", phonetic: "/ˈtʌm.bəl/", pos: "v.", chinese: "跌倒；滾落", level: "115", exEn: "Rocks tumble down steep mountain slope path.", exZh: "岩石從陡峭的山坡路徑上滾落下來。" },
            { word: "elbow", phonetic: "/ˈel.boʊ/", pos: "v./n.", chinese: "用肘推擠；肘部", level: "115", exEn: "I had to elbow my way through crowded bus station.", exZh: "我不得不擠過擁擠的公車站。" },
            { word: "struggle", phonetic: "/ˈstrʌɡ.əl/", pos: "v./n.", chinese: "掙扎；奮鬥", level: "115", exEn: "Students struggle to master complex math problems.", exZh: "學生們努力掌握複雜的數學問題。" },
            { word: "swift", phonetic: "/swɪft/", pos: "adj.", chinese: "迅速的；敏捷的", level: "115", exEn: "Police made swift response action catching suspect.", exZh: "警方採取迅速反應行動逮捕嫌犯。" },
            { word: "brutal", phonetic: "/ˈbruː.t̬əl/", pos: "adj.", chinese: "殘忍的；嚴酷的", level: "115", exEn: "The city survived brutal winter blizzard weather conditions.", exZh: "這座城市倖存於殘酷的冬日暴風雪天氣條。" },
            { word: "harsh", phonetic: "/hɑːrʃ/", pos: "adj.", chinese: "嚴厲的；惡劣的", level: "115", exEn: "Don't judge people with harsh critical words.", exZh: "不要用嚴厲批評的言語批判他人。" },
            { word: "grave", phonetic: "/ɡreɪv/", pos: "adj./n.", chinese: "嚴重的；墓穴", level: "115", exEn: "Shootings raised grave concerns about campus safety.", exZh: "槍擊案引發對校園安全的嚴重關切。" }
        ];

        /* ==========================================================================
           App State Management
           ========================================================================== */
        let currentTab = 'L3';
        let currentDeck = [];
        let currentCardIndex = 0;
        let isFlipped = false;

        // Quiz State
        let quizQuestions = [];
        let currentQuizIndex = 0;
        let quizScore = 0;
        let currentQuizAnswered = false;

        /* ==========================================================================
           Init & Tab Switching
           ========================================================================== */
        window.addEventListener('DOMContentLoaded', () => {
            switchTab('L3');
        });

        function switchTab(tabKey) {
            currentTab = tabKey;
            
            // Update tab button UI styles
            const buttons = document.querySelectorAll('.tab-btn');
            buttons.forEach(btn => {
                btn.classList.remove('bg-indigo-600', 'text-white', 'shadow-inner');
            });
            const activeBtn = document.getElementById(`tab-${tabKey}`);
            if (activeBtn && tabKey !== 'QUIZ') {
                activeBtn.classList.add('bg-indigo-600', 'text-white', 'shadow-inner');
            }

            // Hide all views
            const viewFlashcard = document.getElementById('view-flashcard');
            const viewAZ = document.getElementById('view-az');
            const viewQuiz = document.getElementById('view-quiz');

            if (viewFlashcard) viewFlashcard.classList.add('hidden');
            if (viewAZ) viewAZ.classList.add('hidden');
            if (viewQuiz) viewQuiz.classList.add('hidden');

            if (tabKey === 'AZ') {
                if (viewAZ) viewAZ.classList.remove('hidden');
                renderAZTable();
            } else if (tabKey === 'QUIZ') {
                if (viewQuiz) viewQuiz.classList.remove('hidden');
                showQuizStartScreen();
            } else {
                if (viewFlashcard) viewFlashcard.classList.remove('hidden');
                loadDeck(tabKey);
            }
        }

        /* ==========================================================================
           Flashcard Logic (Tabs L3 - 115)
           ========================================================================== */
        function loadDeck(levelKey) {
            currentDeck = ALL_WORDS.filter(item => item.level === levelKey);
            currentCardIndex = 0;
            isFlipped = false;

            const titles = {
                'L3': 'L3 單字',
                'L4': 'L4 單字',
                'L5': 'L5 單字',
                'L6': 'L6 單字',
                '112': '112 學測單字',
                '113': '113 學測單字',
                '114': '114 學測單字',
                '115': '115 學測單字'
            };

            const titleEl = document.getElementById('deck-title');
            if (titleEl) titleEl.innerText = titles[levelKey] || `${levelKey} 單字`;
            
            renderCard();
        }

        function renderCard() {
            if (!currentDeck || currentDeck.length === 0) return;
            const card = currentDeck[currentCardIndex];
            
            // Card reset orientation
            isFlipped = false;
            const container = document.getElementById('flashcard-container');
            if (container) container.classList.remove('flipped');

            // Counter
            const counter = document.getElementById('card-counter');
            if (counter) counter.innerText = `${currentCardIndex + 1} / ${currentDeck.length}`;

            // Populate Front
            const frontPos = document.getElementById('card-front-pos');
            const frontChinese = document.getElementById('card-front-chinese');
            if (frontPos) frontPos.innerText = card.pos;
            if (frontChinese) frontChinese.innerText = card.chinese;

            // Populate Back
            const backWord = document.getElementById('card-back-word');
            const backPos = document.getElementById('card-back-pos');
            const backPhonetic = document.getElementById('card-back-phonetic');
            const backChinese = document.getElementById('card-back-chinese');
            const backExEn = document.getElementById('card-back-ex-en');
            const backExZh = document.getElementById('card-back-ex-zh');

            if (backWord) backWord.innerText = card.word;
            if (backPos) backPos.innerText = `(${card.pos})`;
            if (backPhonetic) backPhonetic.innerText = card.phonetic;
            if (backChinese) backChinese.innerText = card.chinese;
            if (backExEn) backExEn.innerText = `"${card.exEn}"`;
            if (backExZh) backExZh.innerText = card.exZh;
        }

        function toggleCardFlip() {
            isFlipped = !isFlipped;
            const container = document.getElementById('flashcard-container');
            if (container) {
                if (isFlipped) {
                    container.classList.add('flipped');
                } else {
                    container.classList.remove('flipped');
                }
            }
        }

        function nextCard() {
            if (currentDeck.length === 0) return;
            currentCardIndex = (currentCardIndex + 1) % currentDeck.length;
            renderCard();
        }

        function prevCard() {
            if (currentDeck.length === 0) return;
            currentCardIndex = (currentCardIndex - 1 + currentDeck.length) % currentDeck.length;
            renderCard();
        }

        function shuffleCurrentDeck() {
            for (let i = currentDeck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [currentDeck[i], currentDeck[j]] = [currentDeck[j], currentDeck[i]];
            }
            currentCardIndex = 0;
            renderCard();
        }

        /* ==========================================================================
           Text-to-Speech (TTS)
           ========================================================================== */
        function speakWord(customText) {
            if (!('speechSynthesis' in window)) {
                alert('您的瀏覽器不支援語音合成功能');
                return;
            }
            window.speechSynthesis.cancel(); // Stop ongoing speech

            let textToSpeak = customText;
            if (!textToSpeak && currentDeck && currentDeck[currentCardIndex]) {
                textToSpeak = currentDeck[currentCardIndex].word;
            }
            if (!textToSpeak) return;

            const utterance = new SpeechSynthesisUtterance(textToSpeak);
            utterance.lang = 'en-US';
            utterance.rate = 0.85; // Slightly slower for clarity
            window.speechSynthesis.speak(utterance);
        }

        /* ==========================================================================
           Tab 9: A-Z All Words Master List
           ========================================================================== */
        function renderAZTable() {
            filterAZList();
        }

        function filterAZList() {
            const searchInput = document.getElementById('az-search-input');
            const levelFilter = document.getElementById('az-level-filter');
            const sortSelect = document.getElementById('az-sort-select');
            
            const query = searchInput ? searchInput.value.toLowerCase().trim() : '';
            const level = levelFilter ? levelFilter.value : 'ALL';
            const sortOrder = sortSelect ? sortSelect.value : 'az';

            let filtered = ALL_WORDS.filter(item => {
                const matchQuery = item.word.toLowerCase().includes(query) || item.chinese.includes(query);
                const matchLevel = (level === 'ALL') || (item.level === level);
                return matchQuery && matchLevel;
            });

            // Sorting
            if (sortOrder === 'az') {
                filtered.sort((a, b) => a.word.localeCompare(b.word));
            } else if (sortOrder === 'za') {
                filtered.sort((a, b) => b.word.localeCompare(a.word));
            } else if (sortOrder === 'level') {
                filtered.sort((a, b) => a.level.localeCompare(b.level));
            }

            // Update badge count
            const countBadge = document.getElementById('az-count-badge');
            if (countBadge) countBadge.innerText = `共 ${filtered.length} 個單字`;

            // Render Rows
            const tbody = document.getElementById('az-table-body');
            if (!tbody) return;
            tbody.innerHTML = '';

            filtered.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 transition";
                tr.innerHTML = `
                    <td class="p-3 md:p-4">
                        <div class="font-bold text-indigo-700 text-base">${item.word}</div>
                        <div class="text-xs text-slate-400 font-mono">${item.phonetic}</div>
                    </td>
                    <td class="p-3 md:p-4">
                        <span class="px-2.5 py-1 rounded-full bg-slate-100 text-slate-700 text-xs font-semibold">${item.pos}</span>
                    </td>
                    <td class="p-3 md:p-4 font-medium text-slate-800">${item.chinese}</td>
                    <td class="p-3 md:p-4">
                        <span class="px-2.5 py-1 rounded bg-indigo-50 text-indigo-700 text-xs font-bold border border-indigo-200">${item.level}</span>
                    </td>
                    <td class="p-3 md:p-4 text-center">
                        <button onclick="speakWord('${item.word.replace(/'/g, "\\'")}')" class="p-2 text-indigo-600 hover:bg-indigo-100 rounded-full transition" title="發音">
                            <i class="fa-solid fa-volume-high"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        /* ==========================================================================
           Tab 10: 50-Question GSAT Multiple Choice Quiz Engine
           ========================================================================== */
        function showQuizStartScreen() {
            const startScreen = document.getElementById('quiz-start-screen');
            const activeScreen = document.getElementById('quiz-active-screen');
            const resultScreen = document.getElementById('quiz-result-screen');

            if (startScreen) startScreen.classList.remove('hidden');
            if (activeScreen) activeScreen.classList.add('hidden');
            if (resultScreen) resultScreen.classList.add('hidden');
        }

        function startQuiz() {
            // Generate 50 unique questions sampled randomly from ALL_WORDS
            const pool = [...ALL_WORDS];
            // Shuffle pool
            for (let i = pool.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [pool[i], pool[j]] = [pool[j], pool[i]];
            }

            const targetWords = pool.slice(0, 50);
            quizQuestions = targetWords.map(target => {
                // Generate 3 distractors
                const distractors = ALL_WORDS.filter(item => item.word !== target.word);
                for (let i = distractors.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [distractors[i], distractors[j]] = [distractors[j], distractors[i]];
                }
                const options = [target, ...distractors.slice(0, 3)];
                // Shuffle options
                for (let i = options.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [options[i], options[j]] = [options[j], options[i]];
                }

                // Create masked example question sentence
                const maskedSentence = target.exEn.replace(new RegExp(target.word, 'gi'), '______');

                return {
                    target: target,
                    options: options,
                    questionText: maskedSentence
                };
            });

            currentQuizIndex = 0;
            quizScore = 0;
            currentQuizAnswered = false;

            const startScreen = document.getElementById('quiz-start-screen');
            const activeScreen = document.getElementById('quiz-active-screen');
            const resultScreen = document.getElementById('quiz-result-screen');

            if (startScreen) startScreen.classList.add('hidden');
            if (resultScreen) resultScreen.classList.add('hidden');
            if (activeScreen) activeScreen.classList.remove('hidden');

            renderQuizQuestion();
        }

        function renderQuizQuestion() {
            if (currentQuizIndex >= quizQuestions.length) {
                showQuizResults();
                return;
            }

            currentQuizAnswered = false;
            const q = quizQuestions[currentQuizIndex];

            // Progress Top Bar
            const qNum = document.getElementById('quiz-question-number');
            const qCat = document.getElementById('quiz-category-tag');
            const liveScore = document.getElementById('quiz-live-score');
            const pBar = document.getElementById('quiz-progress-bar');

            if (qNum) qNum.innerText = `第 ${currentQuizIndex + 1} / 50 題`;
            if (qCat) qCat.innerText = q.target.level;
            if (liveScore) liveScore.innerText = quizScore;
            if (pBar) pBar.style.width = `${((currentQuizIndex) / 50) * 100}%`;

            // Question Text
            const qText = document.getElementById('quiz-question-text');
            if (qText) qText.innerText = q.questionText;

            // Options Container
            const optsContainer = document.getElementById('quiz-options-container');
            if (optsContainer) {
                optsContainer.innerHTML = '';
                const labels = ['(A)', '(B)', '(C)', '(D)'];

                q.options.forEach((opt, idx) => {
                    const btn = document.createElement('button');
                    btn.className = "quiz-opt-btn p-4 rounded-xl border border-slate-200 hover:border-indigo-500 hover:bg-indigo-50/50 transition font-semibold text-left text-slate-800 flex items-center space-x-3 w-full bg-white";
                    btn.onclick = () => selectQuizAnswer(opt.word, btn);
                    btn.innerHTML = `
                        <span class="text-indigo-600 font-bold">${labels[idx]}</span>
                        <span class="text-lg">${opt.word}</span>
                    `;
                    optsContainer.appendChild(btn);
                });
            }

            // Reset Explanation box and Next button
            const expBox = document.getElementById('quiz-explanation-box');
            if (expBox) expBox.classList.add('hidden');

            const nextBtn = document.getElementById('quiz-next-btn');
            if (nextBtn) {
                nextBtn.disabled = true;
                nextBtn.className = "px-6 py-3 bg-slate-300 text-slate-500 font-bold rounded-xl shadow transition flex items-center space-x-2 cursor-not-allowed";
            }
        }

        function selectQuizAnswer(selectedWord, clickedBtn) {
            if (currentQuizAnswered) return;
            currentQuizAnswered = true;

            const q = quizQuestions[currentQuizIndex];
            const isCorrect = (selectedWord === q.target.word);

            if (isCorrect) {
                quizScore += 2; // 50 questions * 2 points = 100 max
                clickedBtn.classList.remove('border-slate-200', 'hover:border-indigo-500');
                clickedBtn.classList.add('bg-emerald-100', 'border-emerald-500', 'text-emerald-900');
            } else {
                clickedBtn.classList.remove('border-slate-200', 'hover:border-indigo-500');
                clickedBtn.classList.add('bg-rose-100', 'border-rose-500', 'text-rose-900');

                // Highlight correct option
                const allOptBtns = document.querySelectorAll('.quiz-opt-btn');
                allOptBtns.forEach(btn => {
                    if (btn.innerText.includes(q.target.word)) {
                        btn.classList.add('bg-emerald-100', 'border-emerald-500', 'text-emerald-900');
                    }
                });
            }

            // Live score update
            const liveScore = document.getElementById('quiz-live-score');
            if (liveScore) liveScore.innerText = quizScore;

            // Explanation box
            const expBox = document.getElementById('quiz-explanation-box');
            const badge = document.getElementById('quiz-result-badge');
            const expText = document.getElementById('quiz-explanation-text');

            if (expBox) expBox.classList.remove('hidden');
            if (badge) {
                if (isCorrect) {
                    badge.innerHTML = `<span class="text-emerald-600"><i class="fa-solid fa-circle-check mr-1"></i> 回答正確！ (+2分)</span>`;
                } else {
                    badge.innerHTML = `<span class="text-rose-600"><i class="fa-solid fa-circle-xmark mr-1"></i> 回答錯誤！正確答案是：${q.target.word}</span>`;
                }
            }

            if (expText) {
                expText.innerHTML = `
                    <div class="mt-1">
                        <strong>${q.target.word}</strong> (${q.target.pos}): ${q.target.chinese}
                    </div>
                    <div class="text-slate-500 mt-1 italic">"${q.target.exZh}"</div>
                `;
            }

            // Enable Next Button
            const nextBtn = document.getElementById('quiz-next-btn');
            if (nextBtn) {
                nextBtn.disabled = false;
                nextBtn.className = "px-6 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl shadow transition flex items-center space-x-2 cursor-pointer";
            }

            // Speak Target Word
            speakWord(q.target.word);
        }

        function speakQuizWord() {
            if (quizQuestions[currentQuizIndex]) {
                speakWord(quizQuestions[currentQuizIndex].target.word);
            }
        }

        function nextQuizQuestion() {
            currentQuizIndex++;
            renderQuizQuestion();
        }

        function restartQuizPrompt() {
            if (confirm("確定要放棄當前測驗進度並重新開始嗎？")) {
                startQuiz();
            }
        }

        function showQuizResults() {
            const activeScreen = document.getElementById('quiz-active-screen');
            const resultScreen = document.getElementById('quiz-result-screen');

            if (activeScreen) activeScreen.classList.add('hidden');
            if (resultScreen) resultScreen.classList.remove('hidden');

            const scoreEl = document.getElementById('final-score');
            const evalEl = document.getElementById('final-eval');

            if (scoreEl) scoreEl.innerText = `${quizScore} / 100`;

            if (evalEl) {
                if (quizScore >= 90) {
                    evalEl.innerText = "🌟 太棒了！你的英文單字實力已具備頂標水準！";
                } else if (quizScore >= 70) {
                    evalEl.innerText = "👍 表現優異！掌握了絕大多數高頻學測單字！";
                } else if (quizScore >= 50) {
                    evalEl.innerText = "📚 良好！再多複習幾次單字卡，分數肯定會更高！";
                } else {
                    evalEl.innerText = "💪 加油！建議多利用 L3-L6 分頁加強基礎單字記憶！";
                }
            }
        }
    </script>
    <footer>
  <p>資料來源：龍騰高中英文第五冊｜© 2026＠Maxine</p>
</footer>
</body>
</html>
