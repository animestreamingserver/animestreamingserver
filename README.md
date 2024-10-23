- 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ANIFLIX +</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        body {
            background-color: black;
            position: relative;
            color: white;
            margin: 0;
            font-family: Arial Black;
        }

        .video-section {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 80vh; /* Adjust as needed */
            margin-top: 20px;
            position: relative;
        }

        video {
            width: 90%; /* Maintain aspect ratio */
            height: auto; /* Maintain aspect ratio */
        }

        /* Audio controls at 30% */
        video::-webkit-media-controls-volume-slider {
            width: 70%;
        }

        video::-webkit-media-controls-volume-slider-container {
            width: 70%;
        }

        .next-video-button {
            position: absolute;
            top: 50%;
            right: 20px;
            transform: translateY(-50%);
            width: 50px;
            height: 50px;
            background-color: #fff;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
        }

        .next-video-button img {
            width: 30px;
            height: 30px;
        }

        .prev-video-button {
            position: absolute;
            top: 50%;
            left: 20px;
            transform: translateY(-50%);
            width: 50px;
            height: 50px;
            background-color: #fff;
            border-radius: 50%;
            display: none; /* Initially hidden */
            justify-content: center;
            align-items: center;
            cursor: pointer;
        }

        .prev-video-button img {
            width: 30px;
            height: 30px;
        }

        /* Tooltip */
        .tooltip {
            position: absolute;
            background-color: rgba(0, 0, 0, 0.7);
            color: #fff;
            padding: 10px;
            border-radius: 5px;
            z-index: 10;
            bottom: 0;
            left: 0;
            right: 0;
            text-align: center;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 16px;
        }

        .row-posters {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 10px;
            white-space: nowrap;
        }

        .row-posters a {
            position: relative;
            display: inline-block;
            cursor: pointer;
        }

        .row-posters a:hover .tooltip {
            opacity: 1;
        }

        .search-bar-container {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 100;
        }

        .search-bar {
            background-color: #000;
            color: #fff;
            border: none;
            padding: 10px;
            border-radius: 5px;
            width: 200px;
            transition: width 0.3s;
        }

        .search-bar:focus {
            width: 300px;
            outline: none;
        }

        .search-button {
            background-color: red ;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            margin-left: 5px;
            transition: background-color 0.3s;
        }

        .search-button:hover {
            background-color: #0056b3;
        }

        /* Title and description */
        .video-info {
            position: absolute;
            top: 50%;
            left: 5%; /* Positioned to the left side */
            transform: translateY(-50%); /* Center vertically */
            text-align: left; /* Left-align text */
            animation: fadeOut 10s forwards; /* Fade out after 10 seconds */
        }

        .video-title {
            font-size: 48px; /* Twice as big */
            margin-bottom: 10px;
        }

        .video-description {
            font-size: 16px;
        }

        @keyframes fadeOut {
            0% { opacity: 1; }
            100% { opacity: 0; }
        }

        .hero {
            position: absolute;
            top: 70px; /* Lowered the position */
            left: 20px;
            z-index: 101; /* Ensure the hero content is above the video */
            color: white;
        }

        .hero-content button {
            background-color: red; /* Changed the button color to red */
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }

        .scroll-down {
            position: fixed;
            bottom: 20px;
            right: 40px; /* Moved more to the right */
            cursor: pointer;
           
            z-index: 9999; /* Ensure the button is on top of everything */
        }

        .scroll-down:hover {
            animation-play-state: paused; /* Pause animation on hover */
        }

        @keyframes pop {
            0% {
                transform: scale(1); /* Normal scale */
            }
            100% {
                transform: scale(1.1); /* Slightly larger scale */
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="logo"></div>
        <div class="menu-icon" onclick="toggleSidebar()">&#9776;</div>
        <div class="search-bar-container">
            <input type="text" class="search-bar" placeholder="Search...">
            <button class="search-button">Search</button>
        </div>
    </header>
    <main>
        <section class="hero">
            <div class="hero-content">
                <h1>Streamed</h1>
                <p>Watched</p>
                <button onclick="openBetaPage()">More Info</button>
            </div>
        </section>
        <section class="video-section">
            <h2></h2>
            <video id="videoPlayer" autoplay onended="playNextVideo()">
                <source src="king3.mp4" type="video/mp4">
                Your browser does not support the video tag.
            </video>
            <div class="video-info" id="videoInfo">
                <div class="video-title" id="videoTitle">Sample Title</div>
                <div class="video-description" id="videoDescription">This is a sample description of the video that will disappear after 10 seconds.</div>
            </div>
            <div class="next-video-button" onclick="playNextVideo()">
                <img src="arrow_icon.png" alt="Next Video">
            </div>
            <div class="prev-video-button" onclick="playPreviousVideo()">
                <img src="arrow_icon.png" alt="Previous Video">
            </div>
        </section>
    </main>
    <script>
        let currentVideoIndex = 0;
        const videoSources = ["king3.mp4", "solo leveling.mp4", "jjk.mp4", "cote.mp4", "ds.mp4", "Spy x Family.mp4", "liar.mp4", "shadow.mp4", "Oshi no Ko.mp4", "MASHLE.mp4", "Dr.STONE.mp4", "for the glory.mp4"];
        const videoTitles = [
            "King's Avatar 3",
            "Solo Leveling",
            "Jujutsu Kaisen",
            "Classroom of the Elite",
            "Demon Slayer",
            "Spy x Family",
            "Liar Liar",
            "The Eminence in Shadow",
            "Oshi no Ko",
            "MASHLE",
            "Dr.STONE",
            "The King's Avatar: For the Glory"
        ];
        const videoDescriptions = [
            "The epic conclusion of the King trilogy.",
            "The world's weakest becomes the strongest hunter.",
            "Sorcery and martial arts collide in this intense battle.",
            "Students compete in a high stakes academic competition.",
            "A young demon hunter's quest for vengeance.",
            "A spy's mission to create a fake family for a crucial mission.",
            "A psychological game where deception is the key to victory.",
            "A hero emerges from the shadows to save the world.",
            "Doctor and a girl reincarnated as a idol's kids uncover dark secrets of entertainment industry and there mother's death.",
            "A young man without magical abilities in a world where magic is everything.",
            "A genius scientist, aims to rebuild civilization from scratch after humanity is mysteriously petrified for thousands of years.",
            "A tale of bravery and sacrifice for the ultimate prize."
        ];

        const videoPlayer = document.getElementById('videoPlayer');
        const videoInfo = document.getElementById('videoInfo');
        const videoTitle = document.getElementById('videoTitle');
        const videoDescription = document.getElementById('videoDescription');

        videoPlayer.addEventListener('play', function() {
            showVideoInfo();
            if (videoSources[currentVideoIndex] === "MASHLE.mp4") {
                increaseVolume();
            }
        });

        function showVideoInfo() {
            const currentVideo = videoSources[currentVideoIndex];
            videoTitle.textContent = videoTitles[currentVideoIndex];
            videoDescription.textContent = videoDescriptions[currentVideoIndex];
            videoInfo.style.display = 'block';
            videoInfo.style.animation = 'none'; // Reset animation
            void videoInfo.offsetWidth; // Trigger reflow
            videoInfo.style.animation = 'fadeOut 10s forwards'; // Start fade-out animation
        }

        function playNextVideo() {
            currentVideoIndex = (currentVideoIndex + 1) % videoSources.length;
            loadVideo(currentVideoIndex);
        }

        function playPreviousVideo() {
            currentVideoIndex = (currentVideoIndex - 1 + videoSources.length) % videoSources.length;
            loadVideo(currentVideoIndex);
        }

        function loadVideo(index) {
            videoPlayer.src = videoSources[index];
            showVideoInfo();
            videoPlayer.play();
        }

        document.addEventListener('DOMContentLoaded', () => {
            loadVideo(currentVideoIndex);
        });

        function toggleSidebar() {
            const sidebar = document.getElementById('sidebar');
            sidebar.classList.toggle('active');
        }

        function openBetaPage() {
            window.location.href = "k.html";
        }

        function searchAndPlayYouTube(query) {
            const apiKey = 'YOUR_API_KEY'; // Replace with your actual YouTube Data API v3 key
            const apiUrl = `https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&q=${encodeURIComponent(query)}&key=${apiKey}`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    const videoId = data.items[0].id.videoId;
                    const videoUrl = `https://www.youtube.com/watch?v=${videoId}`;
                    window.open(videoUrl, '_blank');
                })
                .catch(error => console.error('Error fetching YouTube videos:', error));
        }

        function scrollPageDown() {
            window.scrollBy({ top: window.innerHeight, behavior: 'smooth' });
        }

        function increaseVolume() {
            videoPlayer.volume = 1.0; // Max volume
        }
    </script>
</body>
</html>


<p style="color: black;">This text is black.</p>
<p style="color: black;">This text is black.</p>

<h2>Watched</h2>
            <div class="row-posters">
                <a href="https://anix.to/anime/the-kings-avatar-6nx4/ep-1">
                    <div class="tooltip">The King's Avatar</div>
                    <img src="S86475ce3fb89455dbb141d1f7363f97a0.png" alt="The King's Avatar" width="200" height="300" onclick="openLink('https://anix.to/anime/the-kings-avatar-6nx4/ep-1')">
                </a>
                <a href="https://anix.to/anime/solo-leveling-3rpv2/ep-1">
                    <div class="tooltip">Solo Leveling</div>
                    <img src="359624ee690b1744a5a978cd251f5401.png" alt="Solo Leveling" width="200" height="300" onclick="openLink('https://anix.to/anime/solo-leveling-3rpv2/ep-1')">
                </a>
                <a href="https://anix.to/anime/kaijuu-8-gou-llrk6/ep-1">
                    <div class="tooltip">Kaiju No.8</div>
                    <img src="8.png" alt="Kaiju No.8" width="200" height="300" onclick="openLink('https://anix.to/anime/kaijuu-8-gou-llrk6/ep-1')">
                </a>
                <a href="https://anix.to/anime/wind-breaker-ll1x3/ep-1">
                    <div class="tooltip">Wind Breaker</div>
                    <img src="wind.png" alt="Wind Breaker" width="200" height="300" onclick="openLink('https://anix.to/anime/wind-breaker-ll1x3/ep-1')">
                </a>
                <a href="https://anix.to/anime/the-eminence-in-shadow-4ylx/ep-1">
                    <div class="tooltip">The Eminence in Shadow</div>
                    <img src="shadow.png" alt="The Eminence in Shadow" width="200" height="300" ondblclick="openImage('shadow.png')">
                </a>
                <a href="https://anix.to/anime/classroom-of-the-elite-07o9/ep-1">
                    <div class="tooltip">Classroom of the Elite</div>
                    <img src="class.png" alt="Classroom of the Elite" width="200" height="300" ondblclick="openImage('class.png')">
                </a>
<a href="https://anix.to/anime/maou-no-ore-ga-dorei-elf-wo-yome-ni-shitanda-ga-dou-medereba-ii-kwxxw/ep-1">
                    <div class="tooltip">An Archdemon's Dilemma: How to Love Your Elf Bride</div>
                    <img src="ELF.png" alt=" width="200" height="300" ondblclick="openImage('ELF.png')">
                </a>
 <a href="https://anix.to/anime/one-punch-man-928/ep-1">
                    <div class="tooltip">One-Punch Man</div>
                    <img src="punch.png" alt=" width="200" height="300" ondblclick="openImage('punch.png')">
                </a>
 <a href="https://anix.to/anime/mushoku-tensei-jobless-reincarnation-k57v/ep-1">
                    <div class="tooltip">Mushoku Tensei: Jobless Reincarnation</div>
                    <img src="jobless.png" alt=" width="200" height="300" ondblclick="openImage('jobless.png')">
                </a>
<a href="https://anix.to/anime/mashle-magic-and-muscles-7j2zj">
                    <div class="tooltip">MASHLE: MAGIC AND MUSCLES</div>
                    <img src="Mashle.png" alt=" width="200" height="300" ondblclick="openImage('Mashle.png')">
                </a>
<a href="https://anix.to/anime/oshi-no-ko2-4q1jm">
                    <div class="tooltip">Oshi No Ko</div>
                    <img src="oshi.jpg" alt=" width="200" height="300" ondblclick="openImage('oshi.jpg')">
                </a>

 <a href="https://anix.to/anime/my-dress-up-darling-7jjw6/ep-1">
                    <div class="tooltip">My Dress-Up Darling</div>
                    <img src="My Dress-Up Darling.png" alt=My Dress-Up Darling" width="200" height="300" ondblclick="openImage('My Dress-Up Darling.png')">
                </a>
                <a href="https://anix.to/anime/tensei-shitara-dai-nana-ouji-dattanode-kimamani-majutsu-wo-kiwamemasu-52wwq/ep-1">
                    <div class="tooltip">I Was Reincarnated as the 7th Prince So I Can Take My Time Perfecting My Magical Ability</div>
                    <img src="7.png" alt="I Was Reincarnated as the 7th Prince" width="200" height="300" ondblclick="openImage('7.png')">
                </a>
 <a href="https://anix.to/anime/megami-no-cafe-terrace-ojxmz/ep-1">
                    <div class="tooltip">The Café Terrace and Its Goddesses</div>
                    <img src="The Café Terrace and Its Goddesses.png" alt="" width="200" height="300" ondblclick="openImage('The Café Terrace and Its Goddesses.png')">    
</a>
 <a href="https://anix.to/anime/the-detective-is-already-dead-mx2v/ep-1">
                    <div class="tooltip">The Detective Is Already Dead</div>
                    <img src="dead.png" alt="The Detective Is Already Dead" width="200" height="300" onclick="openLink('https://anix.to/anime/the-detective-is-already-dead-mx2v/ep-1')">
</a>

<a href="https://anix.to/anime/the-worlds-finest-assassin-gets-reincarnated-in-another-world-as-an-aristocrat-z0qm/ep-1">
                    <div class="tooltip">The World's Finest Assassin Gets Reincarnated in Another World as an Aristocrat</div>
                    <img src="world.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-worlds-finest-assassin-gets-reincarnated-in-another-world-as-an-aristocrat-z0qm/ep-1')">
</a>

<a href="https://anix.to/anime/the-daily-life-of-the-immortal-king-chinese-dub-v6n4/ep-1">
                    <div class="tooltip">The Daily Life of the Immortal King</div>
                    <img src="daily.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-daily-life-of-the-immortal-king-chinese-dub-v6n4/ep-1')">
</a>

<a href="https://anix.to/anime/haikyu-rjqn/ep-1">
                    <div class="tooltip">HAIKYU</div>
                    <img src="HAIKYU.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/haikyu-rjqn/ep-1')">
</a>

<a href="https://anix.to/anime/the-greatest-demon-lord-is-reborn-as-a-typical-nobody-4qqmx/ep-1">
                    <div class="tooltip">The Greatest Demon Lord Is Reborn as a Typical Nobody</div>
                    <img src="demon.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-greatest-demon-lord-is-reborn-as-a-typical-nobody-4qqmx/ep-1')">
</a>
<a href="https://anix.to/anime/haikyu-lev-appears-83po/ep-1">
                    <div class="tooltip">Lev Genzan</div>
                    <img src="Lev Genzan.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/haikyu-lev-appears-83po/ep-1')">
</a>
<a href="https:https://anix.to/anime/demon-slayer-kimetsu-no-yaiba-6q67/ep-1">
                    <div class="tooltip">Demon Slayer</div>
                    <img src="Demon Slayer.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/demon-slayer-kimetsu-no-yaiba-6q67/ep-1')">
</a>
<a href="https://anix.to/anime/blue-lock-2o2mq/ep-1">
                    <div class="tooltip">BLUELOCK</div>
                    <img src="BLUELOCK.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/blue-lock-2o2mq/ep-1')">
</a>
<a href="https://anix.to/anime/saikyou-onmyouji-no-isekai-tenseiki-n3p0m/ep-1">
                    <div class="tooltip">The Reincarnation of the Strongest Exorcist in Another World</div>
                    <img src="The Reincarnation of the Strongest Exorcist in Another World.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/saikyou-onmyouji-no-isekai-tenseiki-n3p0m/ep-1')">
</a>

<a href="https://anix.to/anime/jujutsu-kaisen-32n8/ep-1">
                    <div class="tooltip">Jujutsu Kaisen</div>
                    <img src="Jujutsu Kaisen.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/jujutsu-kaisen-32n8/ep-1')">
</a>

<a href="https://anix.to/anime/jujutsu-kaisen-0-3w5y/ep-1">
                    <div class="tooltip">Jujutsu Kaisen 0 Movie</div>
                    <img src="Jujutsu Kaisen 0 Movie.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/jujutsu-kaisen-0-3w5y/ep-1')">
</a>

<a href="https://anix.to/anime/dragon-ball-super-7jly/ep-1">
                    <div class="tooltip">Dragon Ball Super</div>
                    <img src="Dragon Ball Super.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/dragon-ball-super-7jly/ep-1')">
</a>

<a href="https://anix.to/anime/sword-art-online-5y9/ep-1">
                    <div class="tooltip">Sword Art Online</div>
                    <img src="Sword Art Online.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/sword-art-online-5y9/ep-1')">
</a>

<a href="https://anix.to/anime/tensei-kizoku-no-isekai-boukenroku-jichou-wo-shiranai-kamigami-no-shito2-qxz23/ep-1">
                    <div class="tooltip">The Aristocrat's Otherworldly Adventure: Serving Gods Who Go Too Far</div>
                    <img src="Far.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/tensei-kizoku-no-isekai-boukenroku-jichou-wo-shiranai-kamigami-no-shito2-qxz23/ep-1')">
</a>

<a href="https://anix.to/anime/isekai-de-cheat-skill-wo-te-ni-shita-ore-wa-genjitsu-sekai-wo-mo-musou-suru-level-up-wa-jinsei-wo-kaeta-vv9r6/ep-1">
                    <div class="tooltip">I Got a Cheat Skill in Another World and Became Unrivaled in The Real World, Too</div>
                    <img src="real.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/isekai-de-cheat-skill-wo-te-ni-shita-ore-wa-genjitsu-sekai-wo-mo-musou-suru-level-up-wa-jinsei-wo-kaeta-vv9r6/ep-1')">
</a>

<a href="https://hianime.to/watch/berserk-of-gluttony-18593?ep=107283">
                    <div class="tooltip">Berserk of Gluttony</div>
                    <img src="Berserk of Gluttony.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/berserk-of-gluttony-18593?ep=107283 ')">
</a>
<a href="https://hianime.to/watch/spirit-chronicles-17333?ep=78673">
                    <div class="tooltip">Spirit Chronicles</div>
                    <img src="Spirit Chronicles.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/spirit-chronicles-17333?ep=78673 ')">
</a><a href="https://hianime.to/watch/makeine-too-many-losing-heroines-19235?ep=126169">
                    <div class="tooltip">Makeine: Too Many Losing Heroines!</div>
                    <img src="Makeine.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/makeine-too-many-losing-heroines-19235?ep=126169 ')">
</a><a href="https://hianime.to/watch/why-does-nobody-remember-me-in-this-world-19240?ep=126174">
                    <div class="tooltip">Why Does Nobody Remember Me in This World?</div>
                    <img src="Why Does Nobody Remember Me in This World.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/why-does-nobody-remember-me-in-this-world-19240?ep=126174 ')">

</a><a href="https://hianime.to/watch/wistoria-wand-and-sword-19239?ep=125863">
                    <div class="tooltip">Wistoria: Wand and Sword</div>
                    <img src="Wistoria Wand and Sword.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/wistoria-wand-and-sword-19239?ep=125863 ')">

</a><a href="https://hianime.to/watch/is-it-wrong-to-try-to-pick-up-girls-in-a-dungeon-1115?ep=17380">
                    <div class="tooltip">Is It Wrong to Try to Pick Up Girls in a Dungeon?</div>
                    <img src="Is It Wrong to Try to Pick Up Girls in a Dungeon.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/is-it-wrong-to-try-to-pick-up-girls-in-a-dungeon-1115?ep=17380 ')">

</a><a href="https://hianime.to/watch/the-kingdoms-of-ruin-18586?ep=107882">
                    <div class="tooltip">The Kingdoms of Ruin</div>
                    <img src="The Kingdoms of Ruin.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/the-kingdoms-of-ruin-18586?ep=107882 ')">
</a><a href="https://hianime.to/watch/the-promised-neverland-55?ep=1389">
                    <div class="tooltip">The Promised Neverland</div>
                    <img src="The Promised Neverland.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/the-promised-neverland-55?ep=1389 ')">
</a><a href="https://hianime.to/watch/our-last-crusade-or-the-rise-of-a-new-world-3181?ep=52632">
                    <div class="tooltip">Our Last Crusade or the Rise of a New World</div>
                    <img src="Our Last Crusade or the Rise of a New World.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/our-last-crusade-or-the-rise-of-a-new-world-3181?ep=52632 ')">
</a><a href="https://hianime.to/watch/vtuber-legend-how-i-went-viral-after-forgetting-to-turn-off-my-stream-19225?ep=125865">
                    <div class="tooltip">VTuber Legend: How I Went Viral after Forgetting to Turn Off My Stream</div>
                    <img src="VTuber Legend How I Went Viral after Forgetting to Turn Off My Stream.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/vtuber-legend-how-i-went-viral-after-forgetting-to-turn-off-my-stream-19225?ep=125865 ')">
</a><a href="https://hianime.to/watch/komi-san-wa-comyushou-desu-2nd-season-17975?ep=89391">
                    <div class="tooltip">Komi-san wa, Comyushou desu. 2nd Season</div>
                    <img src="Komi-san wa Comyushou desu. 2nd Season.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/komi-san-wa-comyushou-desu-2nd-season-17975?ep=89391 ')">
</a>
</a><a href="https://hianime.to/watch/komi-cant-communicate-17906?ep=83660">
                    <div class="tooltip">Komi Can't Communicate</div>
                    <img src="Komi Can't Communicate.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/komi-cant-communicate-17906?ep=83660 ')">
</a>
</a><a href="https://hianime.to/watch/my-instant-death-ability-is-so-overpowered-no-one-in-this-other-world-stands-a-chance-against-me-18847?ep=114682">
                    <div class="tooltip">My Instant Death Ability is So Overpowered, No One in This Other World Stands a Chance Against Me!</div>
                    <img src="My Instant Death Ability is So Overpowered, No One in This Other World Stands a Chance Against Me!.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/my-instant-death-ability-is-so-overpowered-no-one-in-this-other-world-stands-a-chance-against-me-18847?ep=114682 ')">
</a>
</a><a href="https://hianime.to/watch/bungo-stray-dogs-858?ep=14933">
                    <div class="tooltip">Bungo Stray Dogs</div>
                    <img src="Bungo Stray Dogs.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/bungo-stray-dogs-858?ep=14933 ')">
</a>
</a><a href="https://hianime.to/watch/bungou-stray-dogs-wan-15670?ep=51353">
                    <div class="tooltip">Bungo Stray Dogs Wan!</div>
                    <img src="Bungo Stray Dogs Wan!.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/bungou-stray-dogs-wan-15670?ep=51353 ')">
</a>
</a><a href="https://hianime.to/watch/alya-sometimes-hides-her-feelings-in-russian-19254?ep=125794">
                    <div class="tooltip">Alya Sometimes Hides Her Feelings in Russian</div>
                    <img src="Alya Sometimes Hides Her Feelings in Russian.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/alya-sometimes-hides-her-feelings-in-russian-19254?ep=125794 ')">
</a>
</a><a href="https://hianime.to/watch/campfire-cooking-in-another-world-with-my-absurd-skill-18293?ep=97131">
                    <div class="tooltip">Campfire Cooking in Another World with My Absurd Skill</div>
                    <img src="Campfire Cooking in Another World with My Absurd Skill.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/campfire-cooking-in-another-world-with-my-absurd-skill-18293?ep=97131 ')">
</a>
</a><a href="https://hianime.to/watch/reign-of-the-seven-spellblades-18488?ep=102869&ep=102869">
                    <div class="tooltip">Reign of the Seven Spellblades</div>
                    <img src="Reign of the Seven Spellblades.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/reign-of-the-seven-spellblades-18488?ep=102869&ep=102869 ')">
</a>
</a><a href="https://hianime.to/watch/the-wrong-way-to-use-healing-magic-18838?ep=114700">
                    <div class="tooltip">The Wrong Way to Use Healing Magic</div>
                    <img src="The Wrong Way to Use Healing Magic.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/the-wrong-way-to-use-healing-magic-18838?ep=114700 ')">
</a>
</a><a href="https://hianime.to/watch/parallel-world-pharmacy-18087?ep=92700">
                    <div class="tooltip">Parallel World Pharmacy</div>
                    <img src="Parallel World Pharmacy.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/parallel-world-pharmacy-18087?ep=92700 ')">
</a>
</a><a href="https://hianime.to/watch/the-cafe-terrace-and-its-goddesses-season-2-19244?ep=125816">
                    <div class="tooltip">The Café Terrace and Its Goddesses Season 2</div>
                    <img src="The Café Terrace and Its Goddesses Season 2.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/the-cafe-terrace-and-its-goddesses-season-2-19244?ep=125816 ')">
</a>
</a><a href="https://hianime.to/watch/spy-x-family-code-white-19291?ep=126907">
                    <div class="tooltip">Spy x Family Code: White</div>
                    <img src="Spy x Family Code White.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/spy-x-family-code-white-19291?ep=126907 ')">
</a>
</a><a href="https://hianime.to/watch/the-rising-of-the-shield-hero-524?ep=10509">
                    <div class="tooltip">The Rising of the Shield Hero</div>
                    <img src="The Rising of the Shield Hero.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/the-rising-of-the-shield-hero-524?ep=10509 ')">
</a>
</a><a href="https://hianime.to/watch/a-journey-through-another-world-raising-kids-while-adventuring-19227?ep=125869">
                    <div class="tooltip">A Journey Through Another World: Raising Kids While Adventuring</div>
                    <img src="A Journey Through Another World Raising Kids While Adventuring.png" alt=" width="200" height="300" onclick="openLink('https://hianime.to/watch/a-journey-through-another-world-raising-kids-while-adventuring-19227?ep=125869 ')">
</a></a><a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink(' ')">
</a>

</a><a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink(' ')">
</a>
</a><a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink(' ')">
</a>
</a><a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink(' ')">
</a>
</a><a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink(' ')">
</a>

<a href="https://anix.to/anime/jitsu-wa-ore-saikyou-deshita-kw35r/ep-1">
                    <div class="tooltip">Am I Actually the Strongest?</div>
                    <img src="Am I Actually the Strongest.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/jitsu-wa-ore-saikyou-deshita-kw35r/ep-1')">
</a>

<a href="https://anix.to/anime/dr-stone-lo6q/ep-1">
                    <div class="tooltip">Dr. Stone</div>
                    <img src="Dr. Stone.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/dr-stone-lo6q/ep-1')">
</a>

<a href="https://anix.to/anime/spy-x-family-6ll19/ep-1">
                    <div class="tooltip">Spy X Family</div>
                    <img src="Spy X Family.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/spy-x-family-6ll19/ep-1')">
</a>

<a href="https://anix.to/anime/my-hero-academia-jvl2/ep-1">
                    <div class="tooltip">My Hero Academia</div>
                    <img src="My Hero Academia.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/my-hero-academia-jvl2/ep-1')">
</a>

<a href="https://anix.to/anime/beyblade-burst-turbo-67k7/ep-1">
                    <div class="tooltip">BeyBlade Burst Turbo</div>
                    <img src="BeyBlade Burst Turbo.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/beyblade-burst-turbo-67k7/ep-1')">
</a>

<a href="https://anix.to/anime/beyblade-burst-surge-0z3r/ep-1">
                    <div class="tooltip">Beyblade Burst Surge</div>
                    <img src="Beyblade Burst Surge.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/beyblade-burst-surge-0z3r/ep-1')">
</a>

<a href="https://anix.to/anime/beyblade-burst-evolution-vx14/ep-1">
                    <div class="tooltip">Beyblade Burst Evolution</div>
                    <img src="Beyblade Burst Evolution.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/beyblade-burst-evolution-vx14/ep-1')">
</a>


<a href="https://anix.to/anime/beyblade-burst-gt-nqz8/ep-1">
                    <div class="tooltip">Beyblade Burst Rise</div>
                    <img src="Beyblade Burst Rise.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/beyblade-burst-gt-nqz8/ep-1')">
</a>


<a href="https://anix.to/anime/black-clover-v2k6/ep-1">
                    <div class="tooltip">Black Clover</div>
                    <img src="Black Clover.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/black-clover-v2k6/ep-1')">
</a>

<a href="https://anix.to/anime/black-clover-the-all-magic-knights-thanksgiving-festa-3o39/ep-1">
                    <div class="tooltip">Black Clover: The All Magic Knights Thanksgiving Festa</div>
                    <img src="Magic.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/black-clover-the-all-magic-knights-thanksgiving-festa-3o39/ep-1')">
</a>


<a href="https://anix.to/anime/black-clover-movie-lv9q/ep-1">
                    <div class="tooltip">Black Clover: Sword of the Wizard King</div>
                    <img src="King.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/black-clover-movie-lv9q/ep-1')">
</a>
<a href="https://anix.to/anime/squishy-black-clover-7q66/ep-1">
                    <div class="tooltip">Squishy! Black Clover</div>
                    <img src="Squishy! Black Clover.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/squishy-black-clover-7q66/ep-1')">
</a>
<a href="https://anix.to/anime/the-hidden-dungeon-only-i-can-enter-nyml/ep-1">
                    <div class="tooltip">The Hidden Dungeon Only I Can Enter</div>
                    <img src="The Hidden Dungeon Only I Can Enter.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-hidden-dungeon-only-i-can-enter-nyml/ep-1')">
</a>
<a href="https://anix.to/anime/inu-ni-nattara-suki-na-hito-ni-hirowareta-ova-llx0m/ep-1-uncen">
                    <div class="tooltip">My Life as Inukai-san's Dog OVA</div>
                    <img src="My Life as Inukai-san's Dog OVA.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/inu-ni-nattara-suki-na-hito-ni-hirowareta-ova-llx0m/ep-1-uncen')">
</a>
<a href="https://kaido.to/watch/overflow-uncensored-17884?ep=79462">
                    <div class="tooltip">Overflow</div>
                    <img src="overflow.png" alt=" width="200" height="300" onclick="openLink('https://kaido.to/watch/overflow-uncensored-17884?ep=79462')">
</a>
<a href="https://kaido.to/watch/isekai-onsen-paradise-uncensored-18982?ep=116995">
                    <div class="tooltip">Isekai Onsen Paradise</div>
                    <img src="hot.png" alt=" width="200" height="300" onclick="openLink('https://kaido.to/watch/isekai-onsen-paradise-uncensored-18982?ep=116995')">
</a>
<a href="https://kaido.to/watch/my-life-as-inukai-sans-dog-uncensored-18314?ep=97274">
                    <div class="tooltip">My Life as Inukai-san's Dog</div>
                    <img src="My Life as Inukai-san's Dog.png" alt=" width="200" height="300" onclick="openLink('https://kaido.to/watch/my-life-as-inukai-sans-dog-uncensored-18314?ep=97274')">
</a>
<a href="https://kaido.to/watch/mother-of-the-goddess-dormitory-uncensored-17883?ep=79328">
                    <div class="tooltip">Mother of the Goddess' Dormitory </div>
                    <img src="Mother of the Goddess' Dormitory .png" alt=" width="200" height="300" onclick="openLink('https://kaido.to/watch/mother-of-the-goddess-dormitory-uncensored-17883?ep=79328')">
</a>
<a href="https://anix.to/anime/liar-liar-802yo">
                    <div class="tooltip">Liar Liar</div>
                    <img src="Liar Liar.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/liar-liar-802yo')">
</a>
<a href="https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1">
                    <div class="tooltip">The Demon Sword Master of Excalibur Academy </div>
                    <img src="The Demon Sword Master of Excalibur Academy .png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1')">
</a>
<a href="https://anix.to/anime/watashi-no-shiawase-na-kekkon-zl1vl/ep-1">
                    <div class="tooltip">My Happy Marriage </div>
                    <img src="My Happy Marriage .png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/watashi-no-shiawase-na-kekkon-zl1vl/ep-1')">
</a>
<a href="https://anix.to/anime/okashi-na-tensei-vv9xl/ep-1">
                    <div class="tooltip">Sweet Reincarnation</div>
                    <img src="Sweet Reincarnation.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/okashi-na-tensei-vv9xl/ep-1')">
</a>
<a href="https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm">
                    <div class="tooltip">Akashic Records of Bastard Magic Instructor</div>
                    <img src="akashic.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm')">
</a>
<a href="https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1">
                    <div class="tooltip">The Strongest Sage with the Weakest Crest</div>
                    <img src="Shikkakumon no Saikyou Kenja.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1')">
</a>
<a href="https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1">
                    <div class="tooltip">The Iceblade Sorcerer Shall Rule the World</div>
                    <img src="The Iceblade Sorcerer Shall Rule the World 2.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1')">
</a>
<a href="https://anix.to/watch/violet-evergarden-wny6/ep-1">
                    <div class="tooltip">Violet Evergarden</div>
                    <img src="violet evergarden.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/watch/violet-evergarden-wny6/ep-1')">
</a>
<a href="https://anix.to/watch/a-galaxy-next-door-p5l4w/ep-1">
                    <div class="tooltip">A Galaxy Next Door</div>
                    <img src="A Galaxy Next Door.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/watch/a-galaxy-next-door-p5l4w/ep-1')">
</a>
<a href="https://anix.to/watch/lv2-kara-cheat-datta-moto-yuusha-kouho-no-mattari-isekai-life-p5gg5/ep-1">
                    <div class="tooltip">Chillin' in Another World with Level 2 Super Cheat Powers</div>
                    <img src="Chillin' in Another World with Level 2 Super Cheat Powers.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/watch/lv2-kara-cheat-datta-moto-yuusha-kouho-no-mattari-isekai-life-p5gg5/ep-1')">
</a>
<a href="https://anix.to/watch/konosuba-gods-blessing-on-this-wonderful-world-k833/ep-1">
                    <div class="tooltip">KONOSUBA</div>
                    <img src="KONOSUBA.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/watch/konosuba-gods-blessing-on-this-wonderful-world-k833/ep-1')">
</a>
<a href="https://anix.to/watch/wise-mans-grandchild-gv6d/ep-1">
                    <div class="tooltip">Wise Man’s Grandchild</div>
                    <img src="Wise Man’s Grandchild.png" alt=" width="200" height="300" onclick="openLink('https://anix.to/watch/wise-mans-grandchild-gv6d/ep-1')">
</a>
<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink('')">
</a>
<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink('')">
</a>
<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink('')">
</a>
                <!-- Add more clickable movie thumbnails as needed -->
 </div>          
</section>
        <section class="row">
            <h2> Webtoons</h2>
            <div class="row-posters">

 <!-- New Row of 100x200 posters -->
  </a>
                <a href="https://w6.sololevelingmanga.org/manga/solo-leveling-chapter-1/">
                    <div class="tooltip">Solo Leveling</div>
                    <img src="359624ee690b1744a5a978cd251f5401.png" alt="Solo Leveling" width="200" height="300" ondblclick="openImage('359624ee690b1744a5a978cd251f5401.png')">
                </a>
</a>
                <a href="https://imnotthatkindoftalent.us/">
                    <div class="tooltip">I’m Not That Kind of Talent</div>
                    <img src="I’m Not That Kind of Talent.png" alt="Solo Leveling" width="200" height="300" ondblclick="openImage('359624ee690b1744a5a978cd251f5401.png')">
                </a>
<a href="https://www.webtoons.com/en/romance/the-mafia-nanny/episode-1/viewer?title_no=5879&episode_no=1">
                    <div class="tooltip">The Mafia Nanny</div>
                    <img src="The Mafia Nanny.png" alt=" width="200" height="300" onclick="openLink('https://www.webtoons.com/en/romance/the-mafia-nanny/episode-1/viewer?title_no=5879&episode_no=1')">
</a>
<a href="https://www.webtoons.com/en/fantasy/the-exiled-hero/episode-1/viewer?title_no=6487&episode_no=1">
                    <div class="tooltip">The Exiled Hero</div>
                    <img src="The Exiled Hero.png" alt=" width="200" height="300" onclick="openLink('https://www.webtoons.com/en/fantasy/the-exiled-hero/episode-1/viewer?title_no=6487&episode_no=1')">
</a>
<a href="https://www.webtoons.com/en/action/weapon-creator/prologue/viewer?title_no=5956&episode_no=1">
                    <div class="tooltip">Weapon Creator</div>
                    <img src="Weapon Creator.png" alt=" width="200" height="300" onclick="openLink('https://www.webtoons.com/en/action/weapon-creator/prologue/viewer?title_no=5956&episode_no=1')">
</a>
<a href="https://www.webtoons.com/en/action/reincarnator/ep-0-the-return/viewer?title_no=6297&episode_no=1">
                    <div class="tooltip">Reincarnator</div>
                    <img src="Reincarnator.png" alt=" width="200" height="300" onclick="openLink('https://www.webtoons.com/en/action/reincarnator/ep-0-the-return/viewer?title_no=6297&episode_no=1')">
</a>
<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink('')">
</a>
<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt=" width="200" height="300" onclick="openLink('')">
</a>
 </div>          
</section>
        <section class="row">
            <h2>Manhwa Adaptation</h2>
            <div class="row-posters">

 <!-- New Row of 100x200 posters -->
  </a>
                <a href="https://anix.to/anime/solo-leveling-3rpv2/ep-1">
                    <div class="tooltip">Solo Leveling</div>
                    <img src="monarch.png" alt="Solo Leveling" width="250" height="150" ondblclick="openImage('monarch.png')">
                </a>
 </div>          
</section>
        <section class="row">
            <h2>Sports</h2>
            <div class="row-posters">

 <!-- New Row of 100x200 posters -->
  </a>
                <a href="https://anix.to/anime/blue-lock-2o2mq/ep-1">
                    <div class="tooltip">BLUELOCK</div>
                    <img src="BLUELOCK 2.png" alt="BLUELOCK" width="250" height="150" ondblclick="openImage('BLUELOCK 2.png')">
                </a>
 </a>
                <a href="https://anix.to/anime/haikyu-rjqn/ep-1">
                    <div class="tooltip">HAIKYU</div>
                    <img src="HAIKYU 2.png" alt="HAIKYU" width="250" height="150" ondblclick="openImage('HAIKYU 2.png')">
                </a>
<a href="https://anix.to/anime/haikyu-lev-appears-83po/ep-1">
                    <div class="tooltip">Lev Genzan</div>
                    <img src="Lev Genzan 2.png" alt=" width="250" height="150" onclick="openImage('Lev Genzan 2.png')">
</a>
 </div>          
</section>
        <section class="row">
            <h2>School</h2>
            <div class="row-posters">

 <!-- New Row of 100x200 posters -->
  </a>
                <a href="https://anix.to/anime/classroom-of-the-elite-07o9/ep-1">
                    <div class="tooltip">Classroom of the Elite</div>
                    <img src="Classroom of the Elite 2.png" alt="" width="250" height="150" ondblclick="openImage('Classroom of the Elite 2.png')">
                </a>

<a href="https://anix.to/anime/the-eminence-in-shadow-4ylx/ep-1">
                    <div class="tooltip">The Eminence in Shadow</div>
                    <img src="The Eminence in Shadow.png" alt="" width="250" height="150" ondblclick="openImage('The Eminence in Shadow.png')">
                </a>               
 <a href="https://anix.to/anime/my-dress-up-darling-7jjw6/ep-1">
                    <div class="tooltip">My Dress-Up Darling</div>
                    <img src="My Dress-Up Darling 2.png" alt="" width="250" height="150" ondblclick="openImage('My Dress-Up Darling 2.png')">
             </a>
 <a href="https://anix.to/anime/spy-x-family-6ll19/ep-1">
                    <div class="tooltip">Spy X Family</div>
                    <img src="Spy X Family 2.png" alt="" width="250" height="150" ondblclick="openImage('Spy X Family 2.png')">    
</a>
<a href="https://anix.to/anime/mushoku-tensei-jobless-reincarnation-k57v/ep-1">
                    <div class="tooltip">Mushoku Tensei: Jobless Reincarnation</div>
                    <img src="jobless 2 .png" alt="" width="250" height="150" ondblclick="openImage('jobless 2 .png')">    
</a>
 <a href="https://anix.to/anime/megami-no-cafe-terrace-ojxmz/ep-1">
                    <div class="tooltip">The Café Terrace and Its Goddesses</div>
                    <img src="The Café Terrace and Its Goddesses 2.jpg" alt="" width="250" height="150" ondblclick="openImage('The Café Terrace and Its Goddesses 2.png')">    
</a>
                <a href="https://anix.to/anime/wind-breaker-ll1x3/ep-1">
                    <div class="tooltip">WIND BREAKER</div>
                    <img src="WIND BREAKER 2.png" alt="" width="250" height="150" ondblclick="openImage('WIND BREAKER 2.png')">
                </a>
<a href="https://anix.to/anime/my-hero-academia-jvl2/ep-1">
                    <div class="tooltip">My Hero Academia</div>
                    <img src="My Hero Academia 2.png" alt=" width="250" height="150" onclick="openImage('My Hero Academia 2.png')">
</a>

<a href="https://anix.to/anime/jitsu-wa-ore-saikyou-deshita-kw35r/ep-1">
                    <div class="tooltip">Am I Actually the Strongest?</div>
                    <img src="Am I Actually the Strongest 2.png" alt=" width="250" height="150" onclick="openImage('Am I Actually the Strongest 2.png')">
</a>
<a href="https://anix.to/anime/mashle-magic-and-muscles-7j2zj/ep-1">
                    <div class="tooltip">MASHLE: MAGIC AND MUSCLES</div>
                    <img src="ma.png" alt=" width="250" height="150" onclick="openImage('ma.png')">
</a>
<a href="https://anix.to/anime/oshi-no-ko2-4q1jm/ep-1">
                    <div class="tooltip">Oshi No Ko</div>
                    <img src="o.png" alt=" width="250" height="150" onclick="openImage('o.png')">
</a>
 <a href="https://anix.to/anime/haikyu-rjqn/ep-1">
                    <div class="tooltip">HAIKYU</div>
                    <img src="HAIKYU 2.png" alt="HAIKYU" width="250" height="150" ondblclick="openImage('HAIKYU 2.png')">
                </a>
<a href="https://anix.to/anime/haikyu-lev-appears-83po/ep-1">
                    <div class="tooltip">Lev Genzan</div>
                    <img src="Lev Genzan 2.png" alt=" width="250" height="150" onclick="openImage('Lev Genzan 2.png')">
</a>
<a href="https://anix.to/anime/the-greatest-demon-lord-is-reborn-as-a-typical-nobody-4qqmx/ep-1">
                    <div class="tooltip">The Greatest Demon Lord Is Reborn as a Typical Nobody</div>
                    <img src="demon 2.png" alt=" width="250" height="150" onclick="openImage('demon 2.png')">
</a>
<a href="https://anix.to/anime/the-daily-life-of-the-immortal-king-chinese-dub-v6n4/ep-1">
                    <div class="tooltip">The Daily Life of the Immortal King</div>
                    <img src="daily 2.png" alt=" width="250" height="150" onclick="openImage('daily 2.png')">
</a>

<a href="https://anix.to/anime/jujutsu-kaisen-32n8/ep-1">
                    <div class="tooltip">Jujutsu Kaisen</div>
                    <img src="jjk.png" alt=" width="250" height="150" onclick="openImage('jjk.png')">
</a>
<a href="https://kaido.to/watch/my-life-as-inukai-sans-dog-uncensored-18314?ep=97274">
                    <div class="tooltip">My Life as Inukai-san's Dog </div>
                    <img src="My Life as Inukai-san's Dog 2.png" alt=" width="250" height="150" onclick="openImage('My Life as Inukai-san's Dog 2.png')">
</a>
<a href="https://kaido.to/watch/overflow-uncensored-17884?ep=79462">
                    <div class="tooltip">Overflow </div>
                    <img src="overflow 2.png" alt=" width="250" height="150" onclick="openImage('overflow 2.png')">
</a>
<a href="https://anix.to/anime/inu-ni-nattara-suki-na-hito-ni-hirowareta-ova-llx0m/ep-1-uncen">
                    <div class="tooltip">My Life as Inukai-san's Dog OVA </div>
                    <img src="My Life as Inukai-san's Dog OVA 2.png" alt=" width="250" height="150" onclick="openImage('My Life as Inukai-san's Dog OVA 2.png')">

</a>
<a href="https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1">
                    <div class="tooltip">The Iceblade Sorcerer Shall Rule the World</div>
                    <img src="The Iceblade Sorcerer Shall Rule the World.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1')">
</a>
<a href="https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1">
                    <div class="tooltip">The Strongest Sage with the Weakest Crest</div>
                    <img src="Shikkakumon no Saikyou Kenja 2.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1')">
</a>
<a href="https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm">
                    <div class="tooltip">Akashic Records of Bastard Magic Instructor</div>
                    <img src="akashic 2.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm')">
</a>
<a href="https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1">
                    <div class="tooltip">The Demon Sword Master of Excalibur Academy </div>
                    <img src="The Demon Sword Master of Excalibur Academy 2.png" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1')">
</a>
<a href="https://anix.to/anime/liar-liar-802yo">
                    <div class="tooltip">Liar Liar</div>
                    <img src="Liar Liar2.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/liar-liar-802yo')">
</a>
<a href="https://kaido.to/watch/mother-of-the-goddess-dormitory-uncensored-17883?ep=79328">
                    <div class="tooltip">Mother of the Goddess' Dormitory </div>
                    <img src="Mother of the Goddess' Dormitory 2.png" alt=" width="250" height="150" onclick="openLink('https://kaido.to/watch/mother-of-the-goddess-dormitory-uncensored-17883?ep=79328')">
</a> 

<a href=" https://anix.to/anime/tensei-kizoku-no-isekai-boukenroku-jichou-wo-shiranai-kamigami-no-shito2-qxz23/ep-1">
                    <div class="tooltip"> The Aristocrat’s Otherworldly Adventure: Serving Gods Who Go Too Far </div>
                    <img src="god.png" alt="" width="250" height="150" ondblclick="openImage(' god.png')">
                </a>

<a href=" https://anix.to/anime/saikyou-onmyouji-no-isekai-tenseiki-n3p0m/ep-1 ">
                    <div class="tooltip"> The Reincarnation of the Strongest Exorcist in Another World </div>
                    <img src=" world 2.png " alt="" width="250" height="150" ondblclick="openImage(' world 2.png)">
                </a>

<a href=" https://anix.to/anime/isekai-de-cheat-skill-wo-te-ni-shita-ore-wa-genjitsu-sekai-wo-mo-musou-suru-level-up-wa-jinsei-wo-kaeta-vv9r6/ep-1 ">
                    <div class="tooltip"> I Got a Cheat Skill in Another World and Became Unrivaled in The Real World, Too
 </div>
                    <img src="I.png" alt="" width="250" height="150" ondblclick="openImage(' I.png')">
                </a>
</div>
</section>
        <section class="row">
            <h2>Magic</h2>
            <div class="row-posters">

 <!-- New Row of 100x200 posters -->
<a href="https://anix.to/anime/my-hero-academia-jvl2/ep-1">
                    <div class="tooltip">My Hero Academia</div>
                    <img src="My Hero Academia 2.png" alt=" width="250" height="150" onclick="openImage('My Hero Academia 2.png')">
</a>
<a href="https://anix.to/anime/mushoku-tensei-jobless-reincarnation-k57v/ep-1">
                    <div class="tooltip">Mushoku Tensei: Jobless Reincarnation</div>
                    <img src="jobless 2 .png" alt="" width="250" height="150" ondblclick="openImage('jobless 2 .png')">    
</a>

<a href="https://anix.to/anime/the-eminence-in-shadow-4ylx/ep-1">
                    <div class="tooltip">The Eminence in Shadow</div>
                    <img src="The Eminence in Shadow.png" alt="" width="250" height="150" ondblclick="openImage('The Eminence in Shadow.png')">
                </a>         
<a href="">
                    <div class="tooltip">An Archdemon's Dilemma: How to Love Your Elf Bride</div>
                    <img src="bride.png" alt="An Archdemon's Dilemma: How to Love Your Elf Bride" width="250" height="150" ondblclick="openImage('bride.png')">
                </a>
                <a href="https://anix.to/anime/solo-leveling-3rpv2/ep-1">
                    <div class="tooltip">Solo Leveling</div>
                    <img src="monarch.png" alt="Solo Leveling" width="250" height="150" ondblclick="openImage('monarch.png')">
                </a>
<a href="https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1">
                    <div class="tooltip">The Iceblade Sorcerer Shall Rule the World</div>
                    <img src="The Iceblade Sorcerer Shall Rule the World.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/hyouken-no-majutsushi-ga-sekai-wo-suberu-sekai-saikyou-no-majutsushi-de-aru-shounen-wa-majutsu-gakuin-ni-nyuugaku-suru-m2qrp/ep-1')">
</a>
<a href="https://anix.to/anime/jitsu-wa-ore-saikyou-deshita-kw35r/ep-1">
                    <div class="tooltip">Am I Actually the Strongest?</div>
                    <img src="Am I Actually the Strongest 2.png" alt=" width="250" height="150" onclick="openImage('Am I Actually the Strongest 2.png')">
</a>

<a href="https://anix.to/anime/the-daily-life-of-the-immortal-king-chinese-dub-v6n4/ep-1">
                    <div class="tooltip">The Daily Life of the Immortal King</div>
                    <img src="daily 2.png" alt=" width="250" height="150" onclick="openImage('daily 2.png')">
</a>

<a href="https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1">
                    <div class="tooltip">The Strongest Sage with the Weakest Crest</div>
                    <img src="Shikkakumon no Saikyou Kenja 2.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/the-strongest-sage-with-the-weakest-crest-kqx9/ep-1')">
</a>
<a href="https://anix.to/anime/mashle-magic-and-muscles-7j2zj/ep-1">
                    <div class="tooltip">MASHLE: MAGIC AND MUSCLES</div>
                    <img src="ma.png" alt=" width="250" height="150" onclick="openImage('ma.png')">
</a>
<a href="https://anix.to/anime/the-greatest-demon-lord-is-reborn-as-a-typical-nobody-4qqmx/ep-1">
                    <div class="tooltip">The Greatest Demon Lord Is Reborn as a Typical Nobody</div>
                    <img src="demon 2.png" alt=" width="250" height="150" onclick="openImage('demon 2.png')">
</a>
<a href="https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm">
                    <div class="tooltip">Akashic Records of Bastard Magic Instructor</div>
                    <img src="akashic 2.jpg" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/akashic-records-of-bastard-magic-instructor-n8vm')">
</a>
<a href="https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1">
                    <div class="tooltip">The Demon Sword Master of Excalibur Academy </div>
                    <img src="The Demon Sword Master of Excalibur Academy 2.png" alt=" width="250" height="150" onclick="openLink('https://anix.to/anime/the-demon-sword-master-of-excalibur-academy-vvv92/ep-1')">
</a>
<a href="https://anix.to/anime/tensei-shitara-dai-nana-ouji-dattanode-kimamani-majutsu-wo-kiwamemasu-52wwq/ep-1 ">
                    <div class="tooltip">I Was Reincarnated as the 7th Prince So I Can Take My Time Perfecting My Magical Ability </div>
                    <img src="I Was Reincarnated as the 7th Prince So I Can Take My Time Perfecting My Magical Ability.png" alt="" width="250" height="150" ondblclick="openImage(' I Was Reincarnated as the 7th Prince So I Can Take My Time Perfecting My Magical Ability.png')">
                </a>
<a href=" https://anix.to/anime/tensei-kizoku-no-isekai-boukenroku-jichou-wo-shiranai-kamigami-no-shito2-qxz23/ep-1">
                    <div class="tooltip"> The Aristocrat’s Otherworldly Adventure: Serving Gods Who Go Too Far </div>
                    <img src="god.png" alt="" width="250" height="150" ondblclick="openImage(' god.png')">
                </a>

<a href=" https://anix.to/anime/saikyou-onmyouji-no-isekai-tenseiki-n3p0m/ep-1 ">
                    <div class="tooltip"> The Reincarnation of the Strongest Exorcist in Another World </div>
                    <img src=" world 2.png " alt="" width="250" height="150" ondblclick="openImage(' world 2.png)">
                </a>

<a href=" https://anix.to/anime/isekai-de-cheat-skill-wo-te-ni-shita-ore-wa-genjitsu-sekai-wo-mo-musou-suru-level-up-wa-jinsei-wo-kaeta-vv9r6/ep-1 ">
                    <div class="tooltip"> I Got a Cheat Skill in Another World and Became Unrivaled in The Real World, Too
 </div>
                    <img src="I.png" alt="" width="250" height="150" ondblclick="openImage(' I.png')">
                </a>

<a href=" https://anix.to/anime/black-clover-v2k6/ep-1">
                    <div class="tooltip">Black Clover
 </div>
                    <img src="Black Clover 2.png" alt="" width="250" height="150" ondblclick="openImage('Black Clover 2.png')">
                </a>
<a href=" https://anix.to/anime/black-clover-the-all-magic-knights-thanksgiving-festa-3o39/ep-1 ">
                    <div class="tooltip"> Black Clover: The All Magic Knights Thanksgiving Festa
PG 13 HD 
1 
 </div>
                    <img src=" black_clover_-_jump_festa_2018_special_8316.png" alt="" width="250" height="150" ondblclick="openImage(' black_clover_-_jump_festa_2018_special_8316.png')">
                </a>
<a href=" https://anix.to/anime/black-clover-movie-lv9q/ep-1">
                    <div class="tooltip"> Black Clover: Sword of the Wizard King
</div>
                    <img src=" Wizard King.png " alt="" width="250" height="150" ondblclick="openImage(' Wizard King.png ')">
                </a>
<a href=" https://anix.to/anime/squishy-black-clover-7q66/ep-1">
                    <div class="tooltip"> Squishy! Black Clover
 </div>
                    <img src=" Squishy! Black Clover 2
.png" alt="" width="250" height="150" ondblclick="openImage(' Squishy! Black Clover
 2.png')">
                </a>
<a href=" https://anix.to/anime/the-hidden-dungeon-only-i-can-enter-nyml/ep-1">
                    <div class="tooltip"> The Hidden Dungeon Only I Can Enter
 </div>
                    <img src=" w.jpg" alt="" width="250" height="150" ondblclick="openImage(' w.jpg')">
                </a>
<a href=" https://anix.to/anime/watashi-no-shiawase-na-kekkon-zl1vl/ep-1 ">
                    <div class="tooltip"> My Happy Marriage
 </div>
                    <img src=" My Happy Marriage 2.png" alt="" width="250" height="150" ondblclick="openImage(' My Happy Marriage 2.png')">
                </a>

</a>
                <a href="">
                    <div class="tooltip">Solo Leveling</div>
                    <img src="monarch.png" alt="Solo Leveling" width="250" height="150" ondblclick="openImage('monarch.png')">
                </a>
</a>
                <a href="">
                    <div class="tooltip">The King's Avatar</div>
                    <img src="kings.png" alt="g" width="250" height="150" ondblclick="openImage('kings.png')">
                </a>
 <a href="">
                    <div class="tooltip">I Was Reincarnated as the 7th Prince So I Can Take My Time Perfecting My Magical Ability</div>
                    <img src="0d2ef03e553960bc16bc2853da20458c.png" alt="g" width="250" height="150" ondblclick="openImage('0d2ef03e553960bc16bc2853da20458c.png')">
                </a>
 <a href="">
                    <div class="tooltip">Jujutsu Kaisen</div>
                    <img src="jj.png" alt="g" width="250" height="150" ondblclick="openImage('jj.png')">
                </a>
 <a href="">
                    <div class="tooltip">Jujutsu Kaisen 0 Movie</div>
                    <img src="jj0.png" alt="g" width="250" height="150" ondblclick="openImage('jj0.png')">
                </a>
 <a href="">
                    <div class="tooltip">Sword Art Online</div>
                    <img src="soa.png" alt="g" width="250" height="150" ondblclick="openImage('soa.png')">
                </a>
 <a href="">
                    <div class="tooltip">Spirit Chronicles
</div>
                    <img src="Spirit Chroniclesh.png" alt="g" width="250" height="150" ondblclick="openImage('Spirit Chroniclesh.png')">
                </a>
 <a href="">
                    <div class="tooltip">Wistoria: Wand and Sword</div>
                    <img src="Wistoriaf Wand and Sword.png" alt="g" width="250" height="150" ondblclick="openImage('Wistoriaf Wand and Sword.png')">
                </a>
 <a href="">
                    <div class="tooltip">Is It Wrong to Try to Pick Up Girls in a Dungeon?</div>
                    <img src="Is It Wrong to Try to Pick Up Girls in a Dungeonh.png" alt="g" width="250" height="150" ondblclick="openImage('Is It Wrong to Try to Pick Up Girls in a Dungeonh.png')">
                </a>
 <a href="">
                    <div class="tooltip">The Kingdoms of Ruin</div>
                    <img src="The Kingdoms of Ruing.png" alt="g" width="250" height="150" ondblclick="openImage('The Kingdoms of Ruing.png')">
                </a>
 <a href="">
                    <div class="tooltip">The Rising of the Shield Hero</div>
                    <img src="rsh.png" alt="g" width="250" height="150" ondblclick="openImage('rsh.png')">
                </a>

<a href="">
                    <div class="tooltip">Reign of the Seven Spellblades</div>
                    <img src="r7s.png" alt="g" width="250" height="150" ondblclick="openImage('r7s.png')">
                </a>

<a href="">
                    <div class="tooltip">The Wrong Way to Use Healing Magic</div>
                    <img src="The Wrong Way to Use Healing Magicg.png" alt="g" width="250" height="150" ondblclick="openImage('The Wrong Way to Use Healing Magich.png')">
                </a>
<a href="">
                    <div class="tooltip">My Instant Death Ability is So Overpowered, No One in This Other World Stands a Chance Against Me!
</div>
                    <img src="Instant Death Ability.png" alt="g" width="250" height="150" ondblclick="openImage('.png')">
                </a>
<a href="">
                    <div class="tooltip">The Strongest Sage with the Weakest Crest </div>
                    <img src="The Strongest Sage with the Weakest Crest .png" alt="g" width="250" height="150" ondblclick="openImage('The Strongest Sage with the Weakest Crest .png')">
                </a>
<a href="">
                    <div class="tooltip">KONOSUBA </div>
                    <img src="KONOSUBgA.png" alt="g" width="250" height="150" ondblclick="openImage('KONOSUBgA.png')">
                </a>
<a href="">
                    <div class="tooltip">Wise Man’s Grandchild </div>
                    <img src="Wise Man’s Grandchildh.png" alt="g" width="250" height="150" ondblclick="openImage('Wise Man’s Grandchildh.png')">
                </a>

<a href="">
                    <div class="tooltip"></div>
                    <img src=".png" alt="g" width="250" height="150" ondblclick="openImage('.png')">
                </a>






</div>
     </section>
    </main>
    <script src="script.js"></script>
    <script>
        let currentVideoIndex = 0;
        const videoSources = ["king3.mp4", "solo leveling.mp4", "jjk.mp4", "cote.mp4","ds.mp4","Spy x Family.mp4","liar.mp4","shadow.mp4","for the glory.mp4"];

        function playNextVideo() {
            currentVideoIndex = (currentVideoIndex + 1) % videoSources.length;
            loadVideo(currentVideoIndex);
        }

        function playPreviousVideo() {
            currentVideoIndex = (currentVideoIndex - 1 + videoSources.length) % videoSources.length;
            loadVideo(currentVideoIndex);
        }

        function loadVideo(index) {
            const videoPlayer = document.getElementById('videoPlayer');
            videoPlayer.src = videoSources[index];
            if (videoSources[index] === "king3.mp4") {
                videoPlayer.classList.add('king3');
            } else {
                videoPlayer.classList.remove('king3');
            }
            document.querySelector('.video-section h2').textContent = videoTitles[index];
            videoPlayer.play();
        }

        document.addEventListener('DOMContentLoaded', () => {
            loadVideo(currentVideoIndex);
        });

        function toggleSidebar() {
            const sidebar = document.getElementById('sidebar');
            sidebar.classList.toggle('active');
        }

        function openBetaPage() {
            window.location.href = "beta.html";
        }

        function searchAndPlayYouTube(query) {
            const apiKey = 'YOUR_API_KEY'; // Replace with your actual YouTube Data API v3 key
            const apiUrl = `https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&q=${encodeURIComponent(query)}&key=${apiKey}`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    const videoId = data.items[0].id.videoId;
                    const videoUrl = `https://www.youtube.com/watch?v=${videoId}`;
                    window.open(videoUrl, '_blank');
                })
                .catch(error => console.error('Error fetching YouTube videos:', error));
        }
    </script>
</body>
</html>



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anime Quiz Popup</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        body {
            background-color: black;
            color: white;
            font-family: Arial Black;
            margin: 0;
        }

        .quiz-button {
            background-color: red;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
        }

        .quiz-popup {
            display: none;
            position: fixed;
            bottom: 80px;
            right: 20px;
            background-color: #333;
            padding: 20px;
            border-radius: 5px;
            z-index: 1000;
            width: 300px;
        }

        .quiz-popup input[type="text"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: none;
            border-radius: 5px;
        }

        .quiz-popup button {
            background-color: red;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-right: 5px;
        }

        .quiz-popup button:hover {
            background-color: #0056b3;
        }

        .close-btn {
            background-color: #555;
            float: right;
        }
    </style>
</head>
<body>
    <button class="quiz-button" onclick="toggleQuizPopup()">Ask Anime Questions</button>

    <div class="quiz-popup" id="quizPopup">
        <button class="close-btn" onclick="toggleQuizPopup()">X</button>
        <h2>Anime Questions</h2>
        <input type="text" id="animeQuestion" placeholder="Type your question...">
        <button onclick="submitQuestion()">Submit</button>
    </div>

    <script>
        function toggleQuizPopup() {
            const quizPopup = document.getElementById('quizPopup');
            if (quizPopup.style.display === 'none' || quizPopup.style.display === '') {
                quizPopup.style.display = 'block';
            } else {
                quizPopup.style.display = 'none';
            }
        }

        function submitQuestion() {
            const question = document.getElementById('animeQuestion').value;
            if (question.trim() !== '') {
                alert('Your question: ' + question);
                document.getElementById('animeQuestion').value = ''; // Clear input field
                toggleQuizPopup(); // Close popup after submitting
            } else {
                alert('Please enter a question.');
            }
        }
    </script>
</body>
</html>
