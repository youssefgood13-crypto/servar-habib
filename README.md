<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مكتبة حبيب قدوس</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Amiri:wght@700&family=Aref+Ruqaa:wght@700&display=swap');
        body, html { margin: 0; padding: 0; min-height: 100vh; background-color: #000; font-family: 'Segoe UI', sans-serif; color: white; overflow-x: hidden; }
        .bg-img { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-image: url('https://upload.wikimedia.org/wikipedia/commons/e/e4/Christ_the_Consoler.jpg'); background-size: cover; background-position: center; z-index: -2; }
        .overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.7); z-index: -1; }
        .cross { position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); font-size: 60vh; color: rgba(255, 255, 255, 0.05); z-index: 0; pointer-events: none; }
        .content { position: relative; z-index: 10; display: flex; flex-direction: column; align-items: center; padding: 50px 20px; text-align: center; }
        h1 { font-family: 'Aref Ruqaa', serif; color: #ffcc00; font-size: clamp(35px, 8vw, 65px); margin-bottom: 25px; text-shadow: 2px 2px 10px #000; }
        #search { width: 100%; max-width: 500px; padding: 15px; border: 3px solid #ffcc00; border-radius: 50px; font-size: 18px; outline: none; text-align: center; background: #fff; color: #000; font-weight: bold; }
        #status { margin: 20px; font-size: 18px; color: #ffcc00; font-weight: bold; }
        .results { width: 100%; max-width: 600px; padding: 0; list-style: none; }
        .file-item { background: rgba(255, 255, 255, 0.95); color: #000; margin: 10px 0; padding: 15px; border-radius: 12px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
        .file-item a { background: #8b0000; color: #fff; padding: 10px 20px; text-decoration: none; border-radius: 8px; font-weight: bold; }
        .footer { margin-top: 50px; font-family: 'Amiri', serif; font-size: clamp(28px, 6vw, 45px); text-shadow: 2px 2px 10px #000; }
    </style>
</head>
<body>
    <div class="bg-img"></div>
    <div class="overlay"></div>
    <div class="cross">†</div>
    <div class="content">
        <h1>الكتاب المقدس كامل</h1>
        <input type="text" id="search" placeholder="اكتب اسم السفر هنا للبحث..." oninput="doSearch()">
        <div id="status">جاري الاتصال بالمكتبة...</div>
        <ul id="list" class="results"></ul>
        <div class="footer">مكتبة حبيب قدوس</div>
    </div>
    <script>
        const USER = 'youssefgood13-crypto';
        const REPO = 'servar-habib';
        let allFiles = [];

        async function loadFiles() {
            const status = document.getElementById('status');
            try {
                // محاولة جلب الملفات من API الخاص بـ GitHub
                const response = await fetch(`https://api.github.com/repos/${USER}/${REPO}/contents/`);
                const data = await response.json();
                
                if (Array.isArray(data)) {
                    // تصفية الملفات (استبعاد index.html وأي فولدرات)
                    allFiles = data.filter(f => f.type === 'file' && f.name !== 'index.html');
                    status.innerHTML = `✅ تم العثور على (${allFiles.length}) سفر جاهز للقراءة`;
                } else {
                    status.innerHTML = "❌ المستودع فاضي! ارفع ملفات الأسفار الأول.";
                }
            } catch (err) {
                status.innerHTML = "❌ خطأ في الاتصال! تأكد إن المستودع Public.";
            }
        }

        function doSearch() {
            const query = document.getElementById('search').value.toLowerCase();
            const list = document.getElementById('list');
            list.innerHTML = '';

            if (query.length < 1) return;

            const matches = allFiles.filter(f => f.name.toLowerCase().includes(query));
            matches.forEach(f => {
                const li = document.createElement('li');
                li.className = 'file-item';
                // رابط السفر المباشر
                const url = `https://${USER}.github.io/${REPO}/${f.name}`;
                li.innerHTML = `<b>📖 ${f.name.split('.')[0]}</b> <a href="${url}" target="_blank">فتح السفر</a>`;
                list.appendChild(li);
            });

            document.getElementById('status').innerText = matches.length > 0 ? `✨ لقينا ${matches.length} نتيجة` : "🤷 مفيش ملف بالاسم ده";
        }

        loadFiles();
    </script>
</body>
</html>
