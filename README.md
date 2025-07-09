<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Marufzoda AI - O'zbekcha Wikipedia Qidiruv</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #1a2a6c);
            color: white;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
        }
        
        .search-container {
            display: flex;
            margin: 30px 0;
            gap: 10px;
        }
        
        #searchInput {
            flex: 1;
            padding: 15px 20px;
            font-size: 1.1rem;
            border: none;
            border-radius: 50px;
            outline: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        #searchButton {
            padding: 15px 30px;
            font-size: 1.1rem;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        #searchButton:hover {
            background: #45a049;
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }
        
        .results-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .result-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
        }
        
        .result-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
            background: rgba(255, 255, 255, 0.15);
        }
        
        .result-title {
            font-size: 1.4rem;
            margin-bottom: 10px;
            color: #ffcc00;
        }
        
        .result-snippet {
            margin-bottom: 15px;
            line-height: 1.5;
            flex-grow: 1;
        }
        
        .result-link {
            display: inline-block;
            padding: 8px 15px;
            background: #2196F3;
            color: white;
            text-decoration: none;
            border-radius: 5px;
            transition: background 0.3s ease;
            text-align: center;
        }
        
        .result-link:hover {
            background: #0b7dda;
        }
        
        .loading {
            text-align: center;
            font-size: 1.2rem;
            margin: 30px 0;
            display: none;
        }
        
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 4px solid #ffffff;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .error {
            background: rgba(244, 67, 54, 0.2);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            margin: 20px 0;
            display: none;
        }
        
        .info-box {
            background: rgba(0, 0, 0, 0.2);
            padding: 15px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
        }
        
        .language-selector {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .lang-btn {
            padding: 8px 15px;
            background: rgba(255, 255, 255, 0.2);
            border: none;
            border-radius: 20px;
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .lang-btn.active {
            background: #2196F3;
            font-weight: bold;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
        }
        
        @media (max-width: 768px) {
            .results-container {
                grid-template-columns: 1fr;
            }
            
            .search-container {
                flex-direction: column;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>MARUFZODA AI</h1>
            <p class="subtitle">Maruf yaratgan AI bilan ma'lumot qidirish</p>
        </header>
        
        <div class="search-container">
            <input type="text" id="searchInput" placeholder="Serching">
            <button id="searchButton">Qidirish</button>
        </div>
        
        <div class="language-selector">
            <button class="lang-btn active" data-lang="uz">O'zbekcha</button>
            <button class="lang-btn" data-lang="ru">Русский</button>
            <button class="lang-btn" data-lang="en">English</button>
        </div>
        
        <div class="info-box">
            <p>Bu ilova Maruf tomonidan yaratilgan. Sizga turli mavzularda Wikipedia orqali ma'lumot topadi.</p>
        </div>
        
        <div class="loading" id="loading">
            <div class="spinner"></div>
            Ma'lumotlar yuklanmoqda...
        </div>
        
        <div class="error" id="error">
            Xatolik yuz berdi! Iltimos, qaytadan urunib ko'ring.
        </div>
        
        <div class="results-container" id="resultsContainer">
            <!-- Natijalar shu yerga joylashtiriladi -->
        </div>
        
        <footer>
            <p>© 2025 Marufzoda AI - Wikipedia Qidiruv Tizimi</p>
            <p>Wikipedia ma'lumotlaridan foydalanadi</p>
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const searchInput = document.getElementById('searchInput');
            const searchButton = document.getElementById('searchButton');
            const resultsContainer = document.getElementById('resultsContainer');
            const loadingElement = document.getElementById('loading');
            const errorElement = document.getElementById('error');
            const langButtons = document.querySelectorAll('.lang-btn');
            
            let currentLang = 'uz'; // Boshlang'ich til o'zbekcha
            
            // Tilni o'zgartirish
            langButtons.forEach(btn => {
                btn.addEventListener('click', function() {
                    // Faqat bitta tugma active bo'lishi uchun
                    langButtons.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    
                    currentLang = this.dataset.lang;
                    searchWikipedia();
                });
            });
            
            // Enter tugmasini qo'llab-quvvatlash
            searchInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    searchWikipedia();
                }
            });
            
            searchButton.addEventListener('click', searchWikipedia);
            
            function searchWikipedia() {
                const searchTerm = searchInput.value.trim();
                
                if (searchTerm === '') {
                    alert('Iltimos, qidiruv so\'zini kiriting!');
                    return;
                }
                
                // Oldingi natijalarni tozalash
                resultsContainer.innerHTML = '';
                errorElement.style.display = 'none';
                loadingElement.style.display = 'block';
                
                // Wikipedia API so'rovi - TILNI O'ZGARTIRDIK
                const apiUrl = `https://${currentLang}.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(searchTerm)}&format=json&origin=*`;
                
                fetch(apiUrl)
                    .then(response => response.json())
                    .then(data => {
                        loadingElement.style.display = 'none';
                        
                        if (data.query && data.query.search) {
                            displayResults(data.query.search);
                        } else {
                            showNoResults();
                        }
                    })
                    .catch(error => {
                        loadingElement.style.display = 'none';
                        errorElement.style.display = 'block';
                        console.error('Xatolik:', error);
                    });
            }
            
            function displayResults(results) {
                if (results.length === 0) {
                    showNoResults();
                    return;
                }
                
                results.forEach(result => {
                    // Snippetdan HTML teglarini olib tashlash
                    const snippet = result.snippet.replace(/<[^>]*>/g, '') + '...';
                    
                    const resultCard = document.createElement('div');
                    resultCard.className = 'result-card';
                    resultCard.innerHTML = `
                        <h3 class="result-title">${result.title}</h3>
                        <p class="result-snippet">${snippet}</p>
                        <a href="https://${currentLang}.wikipedia.org/?curid=${result.pageid}" class="result-link" target="_blank">To'liq maqolani o'qish</a>
                    `;
                    
                    resultsContainer.appendChild(resultCard);
                });
            }
            
            function showNoResults() {
                resultsContainer.innerHTML = `
                    <div class="no-results" style="grid-column: 1 / -1; text-align: center; padding: 30px;">
                        <h3>Hech qanday natija topilmadi 😔</h3>
                        <p>Iltimos, boshqa kalit so'zlar bilan urunib ko'ring yoki tilni o'zgartiring.</p>
                    </div>
                `;
            }
            
            // Dastlabki ma'lumotlarni yuklash
            window.onload = function() {
                searchInput.value = "O'zbekiston";
                searchWikipedia();
            };
        });
    </script>
</body>
</html>
