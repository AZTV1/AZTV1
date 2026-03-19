<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منصة AZTV1 الذهبية</title>
    <style>
        /* إعدادات الألوان الأساسية */
        body { 
            background-color: #000000; /* خلفية سوداء بالكامل */
            color: #ffffff; 
            font-family: 'Segoe UI', Tahoma, sans-serif; 
            margin: 0; 
            text-align: center; 
        }

        header { 
            background: rgba(0, 0, 0, 0.95); 
            padding: 20px; 
            border-bottom: 2px solid #333;
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        /* شعار AZTV1 باللون الذهبي */
        .logo { 
            color: #FFD700; /* لون ذهبي */
            font-size: 35px; 
            font-weight: bold; 
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); /* وهج ذهبي خفيف */
            letter-spacing: 3px;
        }

        .container { padding: 20px; }

        /* الأقسام (العناوين) باللون الأحمر */
        h1, h2 { 
            color: #ff0000; /* لون أحمر صريح */
            margin-bottom: 20px;
        }

        .upload-section { 
            background: #0a0a0a; 
            padding: 40px; 
            border-radius: 15px; 
            margin-bottom: 30px; 
            border: 1px solid #ff0000; /* إطار أحمر خفيف للقسم */
        }

        .btn-upload { 
            background: #ff0000; /* زر أحمر */
            color: white; 
            padding: 15px 35px; 
            border-radius: 5px; 
            cursor: pointer; 
            font-weight: bold; 
            font-size: 18px; 
            display: inline-block; 
            transition: 0.3s;
        }

        .btn-upload:hover {
            background: #cc0000;
            transform: scale(1.05);
        }

        input[type="file"] { display: none; }
        
        /* تفاصيل الفيلم */
        #movie-details { 
            display: none; 
            background: #050505; 
            padding: 25px; 
            border-radius: 15px; 
            margin-top: 20px; 
            text-align: right; 
            border: 1px solid #444; 
        }

        #movie-details img { 
            width: 160px; 
            float: right; 
            margin-left: 20px; 
            border-radius: 10px;
            border: 2px solid #ff0000; /* إطار أحمر حول البوستر */
        }

        .info-text h2 { color: #FFD700; } /* اسم الفيلم يظهر بالذهبي عند البحث */
        
        video { 
            width: 100%; 
            max-width: 900px; 
            border-radius: 10px; 
            margin-top: 25px; 
            border: 2px solid #ff0000; 
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
        }

        .trailer-link { 
            display: inline-block; 
            margin-top: 15px; 
            color: #FFD700; 
            text-decoration: underline;
            font-weight: bold; 
        }
    </style>
</head>
<body>

<header>
    <div class="logo">AZTV1</div>
</header>

<div class="container">
    <div class="upload-section">
        <h1>قسم رفع الأفلام</h1>
        <p>قم باختيار فيلمك المفضل وسنقوم بجلب البيانات لك فوراً</p>
        <label for="movie-file" class="btn-upload">🎬 اختر الفيلم الآن</label>
        <input type="file" id="movie-file" accept="video/*" onchange="handleFileUpload(event)">
    </div>

    <div id="movie-details">
        <img id="movie-poster" src="" alt="بوستر الفيلم">
        <div class="info-text">
            <h2 id="movie-title">اسم الفيلم</h2>
            <p id="movie-story" style="line-height: 1.6;">قصة الفيلم...</p>
            <p style="color: #FFD700;">⭐ التقييم: <span id="movie-rating" style="color: #fff;"></span></p>
            <a id="movie-trailer" class="trailer-link" target="_blank">📺 المقطع الدعائي الرسمي</a>
        </div>
        <video id="main-player" controls></video>
    </div>
</div>

<script>
    const API_KEY = '862024251648a8043632b7337397e5d8';

    async function handleFileUpload(event) {
        const file = event.target.files[0];
        if (!file) return;

        let fileName = file.name.replace(/\.[^/.]+$/, "").replace(/[._-]/g, " ");
        
        const player = document.getElementById('main-player');
        player.src = URL.createObjectURL(file);
        document.getElementById('movie-details').style.display = 'block';

        try {
            const response = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&query=${encodeURIComponent(fileName)}&language=ar`);
            const data = await response.json();

            if (data.results && data.results.length > 0) {
                const movie = data.results[0];
                document.getElementById('movie-title').innerText = movie.title;
                document.getElementById('movie-story').innerText = movie.overview || "الوصف غير متوفر حالياً.";
                document.getElementById('movie-poster').src = `https://image.tmdb.org/t/p/w500${movie.poster_path}`;
                document.getElementById('movie-rating').innerText = movie.vote_average + " / 10";
                
                const trailerRes = await fetch(`https://api.themoviedb.org/3/movie/${movie.id}/videos?api_key=${API_KEY}`);
                const trailerData = await trailerRes.json();
                const trailer = trailerData.results.find(v => v.type === 'Trailer');
                if (trailer) {
                    document.getElementById('movie-trailer').href = `https://www.youtube.com/watch?v=${trailer.key}`;
                }
            }
        } catch (error) {
            console.error("خطأ:", error);
        }
    }
</script>

</body>
</html>
