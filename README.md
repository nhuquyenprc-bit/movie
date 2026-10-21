[movie.html](https://github.com/user-attachments/files/33751110/movie.html)
<!DOCTYPE html>
<html lang="vi" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CineFlix - Xem Phim Trực Tuyến</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        netflix: {
                            red: '#E50914',
                            dark: '#141414',
                            gray: '#333333',
                            lightgray: '#808080'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Font Inter & FontAwesome -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #141414;
            color: #ffffff;
            overflow-x: hidden;
        }
        /* Custom scrollbar for horizontal movie rows */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        .movie-card {
            transition: transform 0.3s cubic-bezier(0.25, 0.8, 0.25, 1), box-shadow 0.3s ease;
        }
        .movie-card:hover {
            transform: scale(1.08);
            z-index: 30;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.8);
        }
        .hero-gradient {
            background: linear-gradient(77deg, rgba(0,0,0,0.9) 15%, rgba(0,0,0,0.5) 45%, rgba(0,0,0,0) 80%);
        }
        .hero-bottom-gradient {
            background: linear-gradient(180deg, transparent 0%, rgba(20,20,20,0.8) 70%, #141414 100%);
        }
    </style>
</head>
<body class="bg-netflix-dark text-white selection:bg-netflix-red selection:text-white">

    <!-- Navbar -->
    <nav id="navbar" class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 bg-gradient-to-b from-black/80 to-transparent px-4 md:px-12 py-4 flex items-center justify-between">
        <div class="flex items-center space-x-8">
            <a href="#" class="text-netflix-red font-extrabold text-2xl md:text-3xl tracking-wider uppercase">CineFlix</a>
            <div class="hidden md:flex items-center space-x-6 text-sm font-medium">
                <button onclick="filterCategory('all')" class="nav-link text-white font-semibold hover:text-gray-300 transition" data-cat="all">Trang chủ</button>
                <button onclick="filterCategory('movie')" class="nav-link text-gray-300 hover:text-white transition" data-cat="movie">Phim lẻ</button>
                <button onclick="filterCategory('series')" class="nav-link text-gray-300 hover:text-white transition" data-cat="series">Phim bộ</button>
                <button onclick="filterCategory('anime')" class="nav-link text-gray-300 hover:text-white transition" data-cat="anime">Anime</button>
                <button onclick="toggleWatchlistModal()" class="nav-link text-gray-300 hover:text-white transition flex items-center gap-1">
                    <i class="fa-solid fa-heart text-netflix-red"></i> Yêu thích <span id="watchlist-count" class="bg-netflix-red text-xs px-1.5 py-0.5 rounded-full">0</span>
                </button>
            </div>
        </div>
        
        <div class="flex items-center space-x-4">
            <!-- Search bar -->
            <div class="relative flex items-center">
                <input type="text" id="search-input" placeholder="Tìm kiếm phim, diễn viên..." 
                    class="bg-black/60 border border-gray-700 text-sm rounded-full pl-9 pr-4 py-1.5 w-48 md:w-64 focus:w-72 transition-all duration-300 focus:outline-none focus:border-white text-white placeholder-gray-400">
                <i class="fa-solid fa-magnifying-glass absolute left-3 text-gray-400 text-sm"></i>
            </div>
            <!-- User avatar -->
            <div class="w-8 h-8 rounded bg-netflix-red flex items-center justify-center font-bold text-sm cursor-pointer overflow-hidden">
                <img src="https://placehold.co/100x100/E50914/ffffff?text=CF" alt="User" class="w-full h-full object-cover">
            </div>
        </div>
    </nav>

    <!-- Mobile Navigation bar bottom -->
    <div class="md:hidden fixed bottom-0 left-0 right-0 z-40 bg-black/90 backdrop-blur border-t border-gray-800 flex justify-around py-3 px-2 text-xs">
        <button onclick="filterCategory('all')" class="flex flex-col items-center text-netflix-red"><i class="fa-solid fa-house text-lg"></i><span>Trang chủ</span></button>
        <button onclick="filterCategory('movie')" class="flex flex-col items-center text-gray-400"><i class="fa-solid fa-film text-lg"></i><span>Phim lẻ</span></button>
        <button onclick="filterCategory('series')" class="flex flex-col items-center text-gray-400"><i class="fa-solid fa-tv text-lg"></i><span>Phim bộ</span></button>
        <button onclick="filterCategory('anime')" class="flex flex-col items-center text-gray-400"><i class="fa-solid fa-bolt text-lg"></i><span>Anime</span></button>
        <button onclick="toggleWatchlistModal()" class="flex flex-col items-center text-gray-400"><i class="fa-solid fa-heart text-lg"></i><span>Yêu thích</span></button>
    </div>

    <!-- Hero Banner Section -->
    <header id="hero-section" class="relative h-[85vh] w-full flex items-center overflow-hidden">
        <div class="absolute inset-0 z-0">
            <img id="hero-bg" src="" alt="Hero Background" class="w-full h-full object-cover">
            <div class="absolute inset-0 hero-gradient"></div>
            <div class="absolute inset-0 hero-bottom-gradient"></div>
        </div>

        <div class="relative z-10 px-4 md:px-12 max-w-2xl mt-16 space-y-4">
            <div class="flex items-center space-x-2">
                <span id="hero-badge" class="bg-netflix-red text-xs font-bold px-2 py-0.5 rounded uppercase tracking-wider">Thịnh hành</span>
                <span id="hero-rating" class="text-green-400 font-bold text-sm"><i class="fa-solid fa-star"></i> 9.2/10</span>
                <span id="hero-year" class="text-gray-300 text-sm">2026</span>
            </div>
            <h1 id="hero-title" class="text-4xl md:text-6xl font-extrabold tracking-tight drop-shadow-md">Tên Phim Hot</h1>
            <p id="hero-desc" class="text-gray-300 text-sm md:text-base line-clamp-3 drop-shadow">Mô tả ngắn gọn về bộ phim siêu phẩm đang làm mưa làm gió trên bảng xếp hạng thế giới với kỹ xảo tuyệt đỉnh và cốt truyện lôi cuốn.</p>
            <div class="flex items-center space-x-4 pt-2">
                <button id="hero-play-btn" class="bg-white hover:bg-gray-200 text-black font-semibold px-6 py-2.5 rounded flex items-center space-x-2 transition shadow-lg">
                    <i class="fa-solid fa-play text-black text-lg"></i>
                    <span>Xem ngay</span>
                </button>
                <button id="hero-more-btn" class="bg-gray-600/70 hover:bg-gray-600/90 text-white font-semibold px-6 py-2.5 rounded flex items-center space-x-2 backdrop-blur transition shadow-lg border border-gray-500/30">
                    <i class="fa-solid fa-circle-info text-lg"></i>
                    <span>Chi tiết</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Rows -->
    <main class="px-4 md:px-12 pb-24 -mt-24 relative z-20 space-y-10">
        
        <!-- Search Results Container (Hidden by default) -->
        <section id="search-results-section" class="hidden space-y-4 pt-12">
            <h2 class="text-xl md:text-2xl font-bold flex items-center gap-2">
                <i class="fa-solid fa-search text-netflix-red"></i> Kết quả tìm kiếm cho: "<span id="search-keyword"></span>"
            </h2>
            <div id="search-results-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4">
                <!-- Dynamic Search Cards -->
            </div>
        </section>

        <!-- Categories Rows Container -->
        <div id="main-catalog" class="space-y-10">
            <!-- Row 1: Thịnh hành -->
            <section class="movie-row space-y-3">
                <div class="flex items-center justify-between">
                    <h2 class="text-lg md:text-xl font-bold flex items-center gap-2">
                        <span class="w-1.5 h-6 bg-netflix-red rounded-full"></span> Phim Thịnh Hành Hôm Nay
                    </h2>
                </div>
                <div class="relative group">
                    <button onclick="scrollRow('row-trending', -1)" class="absolute left-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-r flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-left text-xl"></i>
                    </button>
                    <div id="row-trending" class="flex space-x-4 overflow-x-auto no-scrollbar py-4 px-1 scroll-smooth">
                        <!-- Movies injected by JS -->
                    </div>
                    <button onclick="scrollRow('row-trending', 1)" class="absolute right-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-l flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-right text-xl"></i>
                    </button>
                </div>
            </section>

            <!-- Row 2: Hành động -->
            <section class="movie-row space-y-3">
                <div class="flex items-center justify-between">
                    <h2 class="text-lg md:text-xl font-bold flex items-center gap-2">
                        <span class="w-1.5 h-6 bg-netflix-red rounded-full"></span> Phim Hành Động Đỉnh Cao
                    </h2>
                </div>
                <div class="relative group">
                    <button onclick="scrollRow('row-action', -1)" class="absolute left-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-r flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-left text-xl"></i>
                    </button>
                    <div id="row-action" class="flex space-x-4 overflow-x-auto no-scrollbar py-4 px-1 scroll-smooth">
                        <!-- Movies injected by JS -->
                    </div>
                    <button onclick="scrollRow('row-action', 1)" class="absolute right-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-l flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-right text-xl"></i>
                    </button>
                </div>
            </section>

            <!-- Row 3: Khoa học viễn tưởng -->
            <section class="movie-row space-y-3">
                <div class="flex items-center justify-between">
                    <h2 class="text-lg md:text-xl font-bold flex items-center gap-2">
                        <span class="w-1.5 h-6 bg-netflix-red rounded-full"></span> Khoa Học Viễn Tưởng & Không Gian
                    </h2>
                </div>
                <div class="relative group">
                    <button onclick="scrollRow('row-scifi', -1)" class="absolute left-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-r flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-left text-xl"></i>
                    </button>
                    <div id="row-scifi" class="flex space-x-4 overflow-x-auto no-scrollbar py-4 px-1 scroll-smooth">
                        <!-- Movies injected by JS -->
                    </div>
                    <button onclick="scrollRow('row-scifi', 1)" class="absolute right-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-l flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-right text-xl"></i>
                    </button>
                </div>
            </section>

            <!-- Row 4: Anime / Hoạt hình -->
            <section class="movie-row space-y-3">
                <div class="flex items-center justify-between">
                    <h2 class="text-lg md:text-xl font-bold flex items-center gap-2">
                        <span class="w-1.5 h-6 bg-netflix-red rounded-full"></span> Thế Giới Anime Đặc Sắc
                    </h2>
                </div>
                <div class="relative group">
                    <button onclick="scrollRow('row-anime', -1)" class="absolute left-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-r flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-left text-xl"></i>
                    </button>
                    <div id="row-anime" class="flex space-x-4 overflow-x-auto no-scrollbar py-4 px-1 scroll-smooth">
                        <!-- Movies injected by JS -->
                    </div>
                    <button onclick="scrollRow('row-anime', 1)" class="absolute right-0 top-1/2 -translate-y-1/2 z-30 bg-black/70 hover:bg-black/90 text-white w-10 h-28 rounded-l flex items-center justify-center opacity-0 group-hover:opacity-100 transition shadow-2xl">
                        <i class="fa-solid fa-chevron-right text-xl"></i>
                    </button>
                </div>
            </section>
        </div>
    </main>

    <!-- Movie Detail / Watch Modal -->
    <div id="movie-modal" class="fixed inset-0 z-50 flex items-center justify-center p-2 md:p-6 bg-black/80 backdrop-blur-md hidden">
        <div class="bg-[#181818] w-full max-w-4xl rounded-xl overflow-hidden shadow-2xl border border-gray-800 relative max-h-[90vh] flex flex-col">
            <!-- Close Button -->
            <button onclick="closeMovieModal()" class="absolute top-4 right-4 z-20 bg-black/60 hover:bg-netflix-red w-10 h-10 rounded-full flex items-center justify-center text-white transition">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>

            <!-- Modal Header / Player Container -->
            <div id="modal-player-container" class="relative w-full aspect-video bg-black flex items-center justify-center">
                <!-- Poster or Video iframe -->
                <img id="modal-player-poster" src="" alt="Poster" class="w-full h-full object-cover">
                <div id="modal-player-overlay" class="absolute inset-0 bg-black/40 flex items-center justify-center">
                    <button onclick="playMovieVideo()" class="bg-netflix-red hover:bg-red-700 text-white w-16 h-16 rounded-full flex items-center justify-center shadow-2xl transition transform hover:scale-110">
                        <i class="fa-solid fa-play text-2xl ml-1"></i>
                    </button>
                </div>
                <!-- Simulated video iframe element -->
                <div id="video-frame-wrap" class="absolute inset-0 hidden">
                    <iframe id="youtube-iframe" class="w-full h-full" src="" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                </div>
            </div>

            <!-- Modal Content Info -->
            <div class="p-6 overflow-y-auto space-y-4">
                <div class="flex flex-wrap items-center justify-between gap-4">
                    <div class="space-y-1">
                        <div class="flex items-center space-x-3 text-sm">
                            <span id="modal-rating" class="text-green-400 font-bold"><i class="fa-solid fa-star"></i> 8.5/10</span>
                            <span id="modal-year" class="text-gray-400">2025</span>
                            <span id="modal-duration" class="text-gray-400">120 phút</span>
                            <span id="modal-category-badge" class="bg-gray-800 text-xs px-2 py-0.5 rounded text-gray-300">Phim Lẻ</span>
                        </div>
                        <h2 id="modal-title" class="text-2xl md:text-3xl font-extrabold">Tiêu đề phim</h2>
                    </div>
                    <!-- Action buttons -->
                    <div class="flex items-center space-x-3">
                        <button id="modal-watchlist-btn" onclick="toggleCurrentWatchlist()" class="bg-gray-800 hover:bg-gray-700 border border-gray-600 px-4 py-2 rounded-lg flex items-center space-x-2 text-sm font-semibold transition">
                            <i class="fa-solid fa-plus text-netflix-red" id="modal-watchlist-icon"></i>
                            <span id="modal-watchlist-text">Yêu thích</span>
                        </button>
                        <button onclick="playMovieVideo()" class="bg-netflix-red hover:bg-red-700 px-5 py-2 rounded-lg flex items-center space-x-2 text-sm font-bold transition">
                            <i class="fa-solid fa-play"></i>
                            <span>Xem Phim</span>
                        </button>
                    </div>
                </div>

                <p id="modal-desc" class="text-gray-300 text-sm md:text-base leading-relaxed">
                    Nội dung chi tiết của bộ phim sẽ hiển thị ở đây với cốt truyện hấp dẫn và nhiều tình tiết bất ngờ.
                </p>

                <div class="border-t border-gray-800 pt-4 flex flex-wrap gap-4 text-xs text-gray-400">
                    <div><span class="text-gray-500 font-semibold">Thể loại:</span> <span id="modal-genres" class="text-white">Hành động, Viễn tưởng</span></div>
                    <div><span class="text-gray-500 font-semibold">Đạo diễn:</span> <span id="modal-director" class="text-white">Christopher Nolan</span></div>
                    <div><span class="text-gray-500 font-semibold">Diễn viên:</span> <span id="modal-cast" class="text-white">Tom Hardy, Cillian Murphy</span></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Watchlist Drawer / Modal -->
    <div id="watchlist-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/80 backdrop-blur-md hidden">
        <div class="bg-[#181818] w-full max-w-2xl rounded-xl overflow-hidden shadow-2xl border border-gray-800 flex flex-col max-h-[85vh]">
            <div class="p-4 border-b border-gray-800 flex items-center justify-between">
                <h3 class="text-xl font-bold flex items-center gap-2">
                    <i class="fa-solid fa-heart text-netflix-red"></i> Danh sách phim yêu thích của bạn
                </h3>
                <button onclick="toggleWatchlistModal()" class="text-gray-400 hover:text-white text-lg"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div id="watchlist-items-container" class="p-4 overflow-y-auto space-y-3 flex-1">
                <!-- Watchlist items injected here -->
            </div>
        </div>
    </div>

    <!-- Notification Toast -->
    <div id="toast" class="fixed bottom-16 md:bottom-6 right-6 z-50 bg-neutral-900 border border-gray-700 text-white px-4 py-3 rounded-lg shadow-2xl flex items-center space-x-3 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none">
        <i id="toast-icon" class="fa-solid fa-circle-check text-green-500 text-lg"></i>
        <span id="toast-msg" class="text-sm font-medium">Thông báo hệ thống</span>
    </div>

    <!-- Footer -->
    <footer class="bg-black text-gray-500 text-xs py-10 px-4 md:px-12 border-t border-gray-900 space-y-4">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4">
            <p>&copy; 2026 CineFlix Vietnam. Ứng dụng xem phim giả lập giải trí trực tuyến cao cấp.</p>
            <div class="flex space-x-4 text-base">
                <a href="#" class="hover:text-white transition"><i class="fa-brands fa-facebook"></i></a>
                <a href="#" class="hover:text-white transition"><i class="fa-brands fa-instagram"></i></a>
                <a href="#" class="hover:text-white transition"><i class="fa-brands fa-twitter"></i></a>
                <a href="#" class="hover:text-white transition"><i class="fa-brands fa-youtube"></i></a>
            </div>
        </div>
    </footer>

    <!-- Script Application Logic -->
    <script>
        // Mock Database of Movies
        const moviesData = [
            {
                id: 1,
                title: "Interstellar: Hố Đen Tử Thần",
                category: "movie",
                genreRow: "scifi",
                rating: 9.4,
                year: 2014,
                duration: "169 phút",
                genres: "Khoa học viễn tưởng, Phiêu lưu, Tâm lý",
                director: "Christopher Nolan",
                cast: "Matthew McConaughey, Anne Hathaway, Jessica Chastain",
                poster: "https://images.unsplash.com/photo-1506703719100-a0f3a48c0f86?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1451187580459-43490279c0fa?auto=format&fit=crop&q=80&w=1600",
                desc: "Khi Trái Đất dần trở nên cạn kiệt, một nhóm nhà thám hiểm không gian đã đi qua một hố đen mới được phát hiện để tìm kiếm một hành tinh có thể duy trì sự sống cho loài người.",
                trailerId: "zSWdZVtXT7E"
            },
            {
                id: 2,
                title: "Cyberpunk: Edgerunners",
                category: "anime",
                genreRow: "anime",
                rating: 9.1,
                year: 2022,
                duration: "10 tập",
                genres: "Anime, Hành động, Khoa học viễn tưởng",
                director: "Hiroyuki Imaishi",
                cast: "KENN, Aoi Yuuki, Hiroki Touchi",
                poster: "https://images.unsplash.com/photo-1563089145-599997674d42?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?auto=format&fit=crop&q=80&w=1600",
                desc: "Trong một thành phố tương lai bị ám ảnh bởi công nghệ và sự biến đổi cơ thể, một đứa trẻ đường phố nỗ lực sinh tồn bằng cách trở thành một edgerunner - lính đánh thuê ngoài vòng pháp luật.",
                trailerId: "JtqIas3bYhg"
            },
            {
                id: 3,
                title: "John Wick: Chapter 4",
                category: "movie",
                genreRow: "action",
                rating: 8.8,
                year: 2023,
                duration: "169 phút",
                genres: "Hành động, Hình sự, Giật gân",
                director: "Chad Stahelski",
                cast: "Keanu Reeves, Donnie Yen, Bill Skarsgård",
                poster: "https://images.unsplash.com/photo-1536440136628-849c177e76a1?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1518709268805-4e9042af9f23?auto=format&fit=crop&q=80&w=1600",
                desc: "John Wick khám phá ra con đường đánh bại High Table, nhưng trước khi có được sự tự do, Wick phải đối mặt với một kẻ thù mới với những liên minh hùng mạnh trên toàn cầu.",
                trailerId: "qEVUtrk8_B4"
            },
            {
                id: 4,
                title: "Stranger Things: Mùa 5",
                category: "series",
                genreRow: "scifi",
                rating: 9.2,
                year: 2025,
                duration: "8 tập",
                genres: "Kinh dị, Viễn tưởng, Giả tưởng",
                director: "The Duffer Brothers",
                cast: "Millie Bobby Brown, Finn Wolfhard, Winona Ryder",
                poster: "https://images.unsplash.com/photo-1509198397868-475647b2a1e5?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1518709268805-4e9042af9f23?auto=format&fit=crop&q=80&w=1600",
                desc: "Trận chiến cuối cùng tại thị trấn Hawkins khi ranh giới giữa thế giới thực và Upside Down sụp đổ hoàn toàn, đe dọa sự sống còn của nhân loại.",
                trailerId: "b9EkMc79ZSU"
            },
            {
                id: 5,
                title: "Attack on Titan: Final",
                category: "anime",
                genreRow: "anime",
                rating: 9.6,
                year: 2023,
                duration: "Phim đặc biệt",
                genres: "Anime, Hành động, Giả tưởng tối",
                director: "Yuichiro Hayashi",
                cast: "Yuki Kaji, Yui Ishikawa, Marina Inoue",
                poster: "https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1579783902614-a3fb3927b675?auto=format&fit=crop&q=80&w=1600",
                desc: "Eren Jaeger giải phóng sức mạnh tối thượng của Titans để thực hiện lý tưởng cứu lấy tộc người Eldia, đặt cả thế giới trước bờ vực diệt vong.",
                trailerId: "M_OauHnAFc4"
            },
            {
                id: 6,
                title: "Mad Max: Fury Road",
                category: "movie",
                genreRow: "action",
                rating: 8.9,
                year: 2015,
                duration: "120 phút",
                genres: "Hành động, Viễn tưởng, Phiêu lưu",
                director: "George Miller",
                cast: "Tom Hardy, Charlize Theron, Nicholas Hoult",
                poster: "https://images.unsplash.com/photo-1568605117036-5fe5e7bab0b7?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1508739773434-c26b3d09e071?auto=format&fit=crop&q=80&w=1600",
                desc: "Trong một vùng đất hoang tận thế, Imperator Furiosa lên kế hoạch trốn chạy khỏi bạo chúa Immortan Joe cùng sự trợ giúp của người cô độc Max Rockatansky.",
                trailerId: "hEJnMQG9ev8"
            },
            {
                id: 7,
                title: "Spider-Man: Across the Spider-Verse",
                category: "movie",
                genreRow: "anime",
                rating: 9.3,
                year: 2023,
                duration: "140 phút",
                genres: "Hoạt hình, Hành động, Siêu anh hùng",
                director: "Joaquim Dos Santos, Kemp Powers",
                cast: "Shameik Moore, Hailee Steinfeld, Oscar Isaac",
                poster: "https://images.unsplash.com/photo-1635863138275-d9b33299680b?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?auto=format&fit=crop&q=80&w=1600",
                desc: "Miles Morales du hành qua các đa vũ trụ và đối đầu với một liên minh các Người Nhện khác về cách xử lý một thảm họa tầm vóc vũ trụ.",
                trailerId: "cqGjhVJWtEg"
            },
            {
                id: 8,
                title: "The Batman",
                category: "movie",
                genreRow: "action",
                rating: 8.5,
                year: 2022,
                duration: "176 phút",
                genres: "Hành động, Hình sự, Trinh thám",
                director: "Matt Reeves",
                cast: "Robert Pattinson, Zoë Kravitz, Paul Dano",
                poster: "https://images.unsplash.com/photo-1509198397868-475647b2a1e5?auto=format&fit=crop&q=80&w=800",
                banner: "https://images.unsplash.com/photo-1536440136628-849c177e76a1?auto=format&fit=crop&q=80&w=1600",
                desc: "Batman điều tra tên sát nhân hàng loạt Riddler đang nhắm vào giới tinh hoa tham nhũng tại thành phố Gotham, khám phá ra những bí mật động trời của gia tộc Wayne.",
                trailerId: "mqqft2x_G4g"
            }
        ];

        let currentActiveMovie = null;
        let watchlist = JSON.parse(localStorage.getItem('cineflix_watchlist')) || [];

        // Initialize App on Window Load
        window.onload = function() {
            renderHero(moviesData[0]);
            renderRows();
            updateWatchlistCount();
            setupSearchListener();
            window.addEventListener('scroll', handleNavbarScroll);
        };

        // Handle Navbar background transition on scroll
        function handleNavbarScroll() {
            const navbar = document.getElementById('navbar');
            if (window.scrollY > 50) {
                navbar.classList.add('bg-netflix-dark', 'shadow-lg');
                navbar.classList.remove('bg-gradient-to-b', 'from-black/80');
            } else {
                navbar.classList.remove('bg-netflix-dark', 'shadow-lg');
                navbar.classList.add('bg-gradient-to-b', 'from-black/80');
            }
        }

        // Render Hero Section
        function renderHero(movie) {
            currentActiveMovie = movie;
            document.getElementById('hero-bg').src = movie.banner;
            document.getElementById('hero-title').innerText = movie.title;
            document.getElementById('hero-desc').innerText = movie.desc;
            document.getElementById('hero-rating').innerHTML = `<i class="fa-solid fa-star"></i> ${movie.rating}/10`;
            document.getElementById('hero-year').innerText = movie.year;
            
            document.getElementById('hero-play-btn').onclick = () => openMovieModal(movie.id);
            document.getElementById('hero-more-btn').onclick = () => openMovieModal(movie.id);
        }

        // Render Movie Cards into Rows
        function renderRows() {
            const trendingContainer = document.getElementById('row-trending');
            const actionContainer = document.getElementById('row-action');
            const scifiContainer = document.getElementById('row-scifi');
            const animeContainer = document.getElementById('row-anime');

            trendingContainer.innerHTML = '';
            actionContainer.innerHTML = '';
            scifiContainer.innerHTML = '';
            animeContainer.innerHTML = '';

            moviesData.forEach(movie => {
                const cardHTML = createMovieCardHTML(movie);
                trendingContainer.innerHTML += cardHTML;
                
                if (movie.genreRow === 'action') actionContainer.innerHTML += cardHTML;
                if (movie.genreRow === 'scifi') scifiContainer.innerHTML += cardHTML;
                if (movie.genreRow === 'anime') animeContainer.innerHTML += cardHTML;
            });
        }

        // Create HTML string for individual movie card
        function createMovieCardHTML(movie) {
            const isFav = watchlist.some(item => item.id === movie.id);
            return `
                <div onclick="openMovieModal(${movie.id})" class="movie-card flex-shrink-0 w-44 md:w-56 bg-zinc-900 rounded-lg overflow-hidden shadow-lg cursor-pointer relative group">
                    <div class="w-full aspect-[2/3] relative">
                        <img src="${movie.poster}" alt="${movie.title}" class="w-full h-full object-cover" onerror="this.src='https://placehold.co/400x600/333333/ffffff?text=CineFlix'">
                        <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-transparent opacity-0 group-hover:opacity-100 transition-opacity flex flex-col justify-end p-3">
                            <span class="text-xs font-bold text-green-400 mb-1"><i class="fa-solid fa-star"></i> ${movie.rating} | ${movie.year}</span>
                            <h4 class="font-bold text-sm text-white line-clamp-1">${movie.title}</h4>
                            <span class="text-[10px] text-gray-300 line-clamp-1">${movie.genres}</span>
                        </div>
                    </div>
                </div>
            `;
        }

        // Horizontal scrolling helper
        function scrollRow(rowId, direction) {
            const container = document.getElementById(rowId);
            const scrollAmount = 400 * direction;
            container.scrollBy({ left: scrollAmount, behavior: 'smooth' });
        }

        // Category filter buttons
        function filterCategory(category) {
            // Update active state in nav
            document.querySelectorAll('.nav-link').forEach(btn => btn.classList.remove('text-netflix-red', 'font-semibold'));
            event.currentTarget.classList.add('text-netflix-red', 'font-semibold');

            const searchSection = document.getElementById('search-results-section');
            const mainCatalog = document.getElementById('main-catalog');

            if (category === 'all') {
                searchSection.classList.add('hidden');
                mainCatalog.classList.remove('hidden');
                renderRows();
            } else {
                searchSection.classList.remove('hidden');
                mainCatalog.classList.add('hidden');
                document.getElementById('search-keyword').innerText = category === 'movie' ? 'Phim Lẻ' : category === 'series' ? 'Phim Bộ' : 'Anime';
                
                const filtered = moviesData.filter(m => m.category === category);
                const grid = document.getElementById('search-results-grid');
                grid.innerHTML = filtered.map(m => createMovieCardHTML(m)).join('') || '<p class="text-gray-400 col-span-full text-center py-12">Không tìm thấy phim phù hợp.</p>';
            }
        }

        // Search functionality
        function setupSearchListener() {
            const searchInput = document.getElementById('search-input');
            searchInput.addEventListener('input', (e) => {
                const keyword = e.target.value.toLowerCase().trim();
                const searchSection = document.getElementById('search-results-section');
                const mainCatalog = document.getElementById('main-catalog');

                if (keyword.length > 0) {
                    searchSection.classList.remove('hidden');
                    mainCatalog.classList.add('hidden');
                    document.getElementById('search-keyword').innerText = keyword;

                    const results = moviesData.filter(m => m.title.toLowerCase().includes(keyword) || m.genres.toLowerCase().includes(keyword));
                    const grid = document.getElementById('search-results-grid');
                    grid.innerHTML = results.length > 0 ? results.map(m => createMovieCardHTML(m)).join('') : '<p class="text-gray-400 col-span-full text-center py-12">Không tìm thấy phim nào khớp với từ khóa của bạn.</p>';
                } else {
                    searchSection.classList.add('hidden');
                    mainCatalog.classList.remove('hidden');
                }
            });
        }

        // Modal Controls
        function openMovieModal(movieId) {
            const movie = moviesData.find(m => m.id === movieId);
            if (!movie) return;
            currentActiveMovie = movie;

            document.getElementById('modal-title').innerText = movie.title;
            document.getElementById('modal-rating').innerHTML = `<i class="fa-solid fa-star"></i> ${movie.rating}/10`;
            document.getElementById('modal-year').innerText = movie.year;
            document.getElementById('modal-duration').innerText = movie.duration;
            document.getElementById('modal-desc').innerText = movie.desc;
            document.getElementById('modal-genres').innerText = movie.genres;
            document.getElementById('modal-director').innerText = movie.director;
            document.getElementById('modal-cast').innerText = movie.cast;
            document.getElementById('modal-category-badge').innerText = movie.category === 'movie' ? 'Phim Lẻ' : movie.category === 'series' ? 'Phim Bộ' : 'Anime';

            // Reset player view
            document.getElementById('modal-player-poster').src = movie.banner;
            document.getElementById('modal-player-poster').classList.remove('hidden');
            document.getElementById('modal-player-overlay').classList.remove('hidden');
            document.getElementById('video-frame-wrap').classList.add('hidden');
            document.getElementById('youtube-iframe').src = '';

            updateModalWatchlistButtonState();

            document.getElementById('movie-modal').classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }

        function closeMovieModal() {
            document.getElementById('movie-modal').classList.add('hidden');
            document.getElementById('youtube-iframe').src = ''; // Stop video
            document.body.style.overflow = 'auto';
        }

        // Play movie simulated video via YouTube embed
        function playMovieVideo() {
            if (!currentActiveMovie) return;
            document.getElementById('modal-player-poster').classList.add('hidden');
            document.getElementById('modal-player-overlay').classList.add('hidden');
            
            const iframeWrap = document.getElementById('video-frame-wrap');
            iframeWrap.classList.remove('hidden');
            document.getElementById('youtube-iframe').src = `https://www.youtube.com/embed/${currentActiveMovie.trailerId}?autoplay=1`;
        }

        // Watchlist Management with LocalStorage
        function toggleCurrentWatchlist() {
            if (!currentActiveMovie) return;
            const index = watchlist.findIndex(item => item.id === currentActiveMovie.id);
            
            if (index > -1) {
                watchlist.splice(index, 1);
                showToast("Đã xóa khỏi danh sách yêu thích", "fa-circle-minus", "text-yellow-500");
            } else {
                watchlist.push(currentActiveMovie);
                showToast("Đã thêm vào danh sách yêu thích thành công!", "fa-circle-check", "text-green-500");
            }

            localStorage.setItem('cineflix_watchlist', JSON.stringify(watchlist));
            updateWatchlistCount();
            updateModalWatchlistButtonState();
        }

        function updateModalWatchlistButtonState() {
            if (!currentActiveMovie) return;
            const isFav = watchlist.some(item => item.id === currentActiveMovie.id);
            const icon = document.getElementById('modal-watchlist-icon');
            const text = document.getElementById('modal-watchlist-text');

            if (isFav) {
                icon.className = "fa-solid fa-check text-green-500";
                text.innerText = "Đã thích";
            } else {
                icon.className = "fa-solid fa-plus text-netflix-red";
                text.innerText = "Yêu thích";
            }
        }

        function updateWatchlistCount() {
            document.getElementById('watchlist-count').innerText = watchlist.length;
        }

        function toggleWatchlistModal() {
            const modal = document.getElementById('watchlist-modal');
            const container = document.getElementById('watchlist-items-container');
            
            if (modal.classList.contains('hidden')) {
                // Render list
                if (watchlist.length === 0) {
                    container.innerHTML = `<p class="text-gray-400 text-center py-8">Danh sách yêu thích của bạn đang trống.</p>`;
                } else {
                    container.innerHTML = watchlist.map(movie => `
                        <div class="flex items-center justify-between bg-zinc-900 p-3 rounded-lg hover:bg-zinc-800 transition">
                            <div class="flex items-center space-x-3 cursor-pointer" onclick="closeWatchlistModal(); openMovieModal(${movie.id});">
                                <img src="${movie.poster}" alt="${movie.title}" class="w-12 h-16 object-cover rounded">
                                <div>
                                    <h4 class="font-bold text-sm text-white">${movie.title}</h4>
                                    <p class="text-xs text-gray-400">${movie.year} &bull; ${movie.genres}</p>
                                </div>
                            </div>
                            <button onclick="removeFromWatchlist(event, ${movie.id})" class="text-gray-400 hover:text-netflix-red p-2 transition">
                                <i class="fa-solid fa-trash-can"></i>
                            </button>
                        </div>
                    `).join('');
                }
                modal.classList.remove('hidden');
                document.body.style.overflow = 'hidden';
            } else {
                modal.classList.add('hidden');
                document.body.style.overflow = 'auto';
            }
        }

        function closeWatchlistModal() {
            document.getElementById('watchlist-modal').classList.add('hidden');
            document.body.style.overflow = 'auto';
        }

        function removeFromWatchlist(event, movieId) {
            event.stopPropagation();
            watchlist = watchlist.filter(item => item.id !== movieId);
            localStorage.setItem('cineflix_watchlist', JSON.stringify(watchlist));
            updateWatchlistCount();
            toggleWatchlistModal(); // Re-render list
            toggleWatchlistModal();
            showToast("Đã xóa phim khỏi danh sách", "fa-circle-info", "text-yellow-500");
        }

        // Toast notification helper
        function showToast(message, iconClass = "fa-circle-check", iconColor = "text-green-500") {
            const toast = document.getElementById('toast');
            const msg = document.getElementById('toast-msg');
            const icon = document.getElementById('toast-icon');

            msg.innerText = message;
            icon.className = `fa-solid ${iconClass} ${iconColor} text-lg`;

            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }
    </script>
</body>
</html>
