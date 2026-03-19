<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منصتي الشخصية للأفلام</title>
    <style>
        /* تصميم نتفليكس الاحترافي */
        body { background-color: #111; color: white; font-family: 'Segoe UI', Tahoma, sans-serif; margin: 0; padding: 0; overflow-x: hidden; }
        header { background: linear-gradient(to bottom, rgba(0,0,0,0.9), transparent); padding: 20px; position: fixed; width: 100%; z-index: 1000; display: flex; justify-content: space-between; align-items: center; }
        .logo { color: #E50914; font-size: 30px; font-weight: bold; letter-spacing: 2px; margin-right: 20px; }
        
        /* قسم رفع الفيديو */
        .hero { height: 70vh; display: flex; flex-direction: column; justify-content: center; align-items: center; background: url('https://assets.nflxext.com/ffe/siteui/vlv3/f841d4c7-10e1-40af-bca1-0741f90210c1/ae43105a-52f9-467a-9a99-b31c1722650d/SA-en-20220502-popsignuptwoweeks-perspective_alpha_website_large.jpg'); background-size: cover; box-shadow: inset 0 0 100px #000; text-align: center; border-bottom: 5px solid #222; }
        .upload-card { background: rgba(0,0,0,0.75); padding: 40px; border-radius: 15px; border: 1px solid #333; }
        .hero h1 { font-size: 35px; margin-bottom: 20px; }
        input[type="file"] { display: none; }
        .btn-upload { background-color: #E50914; color: white; padding: 15px 30px; border-radius: 5px; cursor: pointer; font-size: 18px; font-weight: bold; transition: 0.3s; display: inline-block; }
        .btn-upload:hover { background-color: #b20710; transform: scale(1.05); }

        /* مشغل الفيديو */
        video { width: 90%; max-width: 900px; margin: 20px auto; border-radius: 10px; display: none; border: 2px solid #333; box-shadow: 0 0 50px #000; }

        /* صف الأفلام التلقائية */
        .row { padding: 40px 20px; }
        .row h2 { margin-bottom: 20px; font-size: 24px; color: #fff; }
        .posters { display: flex; overflow-y: hidden; overflow-x: scroll; gap: 15px; padding: 10px 0; }
        .posters::-webkit-scrollbar { display: none; }
        .poster { width: 180px; border-radius: 5px; transition: transform 0.3s; cursor: pointer; }
        .poster:hover { transform: scale(1.1); box-shadow: 0 0 15px rgba(255,255,255,0.2); }
    </style>
</head>
<body>

<header>
    <div class="logo">AZTV1</div>
</header>

<div class="hero">
    <div class="upload-card">
        <h1>مرحباً بك في منصتك الخاصة</h1>
        <p>اختر فيلماً من هاتفك واستمتع بالمشاهدة فوراً</p>
        <label for="movie-input" class="btn-upload">
            🎬 اختر الفيلم من الاستوديو
        </label>
        <input type="file" id="movie-input" accept="video/*" onchange="startPlayer(event)">
    </div>
    <video id="videoPlayer" controls></video>
</div>

<div class="row">
    <h2>أفلام مقترحة (Trending)</h2>
    <div class="posters" id="trending-row">
        <!-- سيتم جلب الأفلام هنا تلقائياً -->
    </div>
</div>

<script>
    // 1. وظيفة تشغيل الفيديو المرفوع من الهاتف
    function startPlayer(event) {
        const file = event.target.files[0];
        const player = document.getElementById('videoPlayer');
        if (file) {
            const url = URL.createObjectURL(file);
            player.src = url;
            player.style.display = 'block';
            player.scrollIntoView({ behavior: 'smooth' });
            player.play();
        }
    }

    // 2. جلب أفلام التريند تلقائياً لإعطاء مظهر احترافي
    const API_KEY = '862024251648a8043632b7337397e5d8'; 
    const IMG_URL = 'https://image.tmdb.org/t/p/w500';

    async function fetchMovies() {
        try {
            const res = await fetch(`https://api.themoviedb.org/3/trending/movie/week?api_key=${API_KEY}&language=ar`);
            const data = await res.json();
            const row = document.getElementById('trending-row');
            
            data.results.forEach(movie => {
                const img = document.createElement('img');
                img.src = IMG_URL + movie.poster_path;
                img.className = 'poster';
                img.title = movie.title;
                img.onclick = () => alert("اسم الفيلم: " + movie.title + "\nالقصة: " + movie.overview);
                row.appendChild(img);
            });
        } catch (err) {
            console.log("خطأ في جلب الأفلام");
        }
    }

    fetchMovies();
</script>

</body>
</html>
