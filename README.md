<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>طلبات البيت</title>
    <style>
        /* --- الألوان والأساسيات --- */
        :root { 
            --primary: #25D366; 
            --dark: #075E54; 
            --bg: #f2f4f7; 
            --card-bg: #ffffff;
            --text: #333333;
            --border: #e0e0e0;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body { 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; 
            background: var(--bg); 
            margin: 0; 
            padding: 0; 
            padding-bottom: 120px; /* مسافة أمان عشان زرار الواتس ميغطيش على القائمة */
            color: var(--text);
        }

        /* --- الهيدر الثابت --- */
        .app-header { 
            background: var(--dark); 
            color: white; 
            padding: 15px 0; 
            text-align: center; 
            position: sticky; 
            top: 0; 
            z-index: 1000; 
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .app-header h1 { margin: 0; font-size: 1.2rem; font-weight: 700; }

        .container { 
            max-width: 600px; 
            margin: auto; 
            padding: 15px; 
        }

        /* --- 1. داش بورد (الشاشة الرئيسية) --- */
        #dashboard-view { animation: fadeIn 0.3s ease-in-out; }

        /* شريط البحث الكبير */
        .search-wrapper { position: relative; margin-bottom: 20px; }
        .global-search-input { 
            width: 100%; 
            padding: 15px 45px 15px 15px; 
            border-radius: 12px; 
            border: 1px solid transparent; 
            background: white;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05); 
            font-size: 16px; /* مهم للموبايل */
            transition: all 0.3s;
        }
        .global-search-input:focus { 
            border-color: var(--primary); 
            box-shadow: 0 4px 20px rgba(37, 211, 102, 0.2); 
            outline: none;
        }
        .search-icon { position: absolute; right: 15px; top: 50%; transform: translateY(-50%); color: #999; font-size: 1.2rem; pointer-events: none; }

        /* شبكة الأقسام */
        .grid-container { 
            display: grid; 
            grid-template-columns: repeat(2, 1fr); /* عمودين للموبايل */
            gap: 12px; 
        }
        
        .cat-card { 
            background: var(--card-bg); 
            padding: 15px; 
            border-radius: 16px; 
            text-align: center; 
            cursor: pointer; 
            box-shadow: 0 2px 5px rgba(0,0,0,0.03); 
            border: 1px solid transparent;
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            justify-content: center;
            height: 110px;
            transition: transform 0.1s, background 0.2s;
        }
        
        .cat-card:active { transform: scale(0.96); background-color: #f0fff4; border-color: var(--primary); }
        .cat-icon { font-size: 2.2rem; margin-bottom: 8px; }
        .cat-card h3 { margin: 0; font-size: 1rem; color: #444; font-weight: 600; }

        /* --- 2. صفحة القائمة (التفاصيل) --- */
        #tabs-view { display: none; animation: slideUp 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94); }
        
        /* شريط التنقل العلوي */
        .nav-bar { 
            display: flex; 
            align-items: center; 
            justify-content: space-between; 
            margin-bottom: 15px; 
            background: white;
            padding: 10px 15px;
            border-radius: 12px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.03);
        }
        .back-btn { 
            background: #f0f2f5; 
            border: none; 
            padding: 8px 15px;
            border-radius: 8px;
            color: var(--dark); 
            font-weight: bold; 
            font-size: 0.9rem;
            cursor: pointer;
            display: flex; align-items: center; gap: 5px;
        }
        .cat-title-display { font-size: 1.1rem; font-weight: bold; color: var(--dark); }

        .local-search-input { 
            width: 100%; 
            padding: 12px; 
            border: 1px solid var(--border); 
            border-radius: 10px; 
            margin-bottom: 10px; 
            font-size: 16px; 
            outline: none;
        }
        .local-search-input:focus { border-color: var(--primary); }

        /* --- 3. تصميم الصفوف والعناصر --- */
        .item-container { 
            background: white; 
            margin-bottom: 8px; 
            padding: 12px; 
            border-radius: 12px; 
            box-shadow: 0 1px 3px rgba(0,0,0,0.02);
            border: 1px solid #eee;
        }

        .main-row { display: flex; align-items: center; min-height: 30px; }
        
        /* تكبير الـ Label عشان اللمس */
        .item-label { 
            display: flex; 
            align-items: center; 
            width: 100%; 
            cursor: pointer; 
            padding: 5px 0; /* مساحة لمس إضافية */
        }
        
        /* تخصيص الـ Checkbox */
        input[type="checkbox"] { 
            appearance: none; 
            -webkit-appearance: none;
            width: 24px; 
            height: 24px; 
            border: 2px solid #ccc; 
            border-radius: 6px; 
            margin-left: 12px; 
            display: grid; 
            place-content: center;
            flex-shrink: 0;
            transition: 0.2s;
        }
        input[type="checkbox"]::before {
            content: "";
            width: 12px;
            height: 12px;
            transform: scale(0);
            background-color: white;
            border-radius: 2px;
            transition: 0.1s;
        }
        input[type="checkbox"]:checked {
            background-color: var(--primary);
            border-color: var(--primary);
        }
        input[type="checkbox"]:checked::before { transform: scale(1); }
        
        .item-text { font-size: 1.05rem; color: #444; }
        input[type="checkbox"]:checked + .item-text { color: var(--dark); font-weight: 600; }

        /* التفاصيل (الكمية والملاحظة) */
        .details-row { 
            display: none; 
            margin-top: 12px; 
            padding-top: 10px;
            border-top: 1px dashed #eee;
            gap: 10px; 
            animation: fadeIn 0.2s;
        }
        .details-row.show { display: flex; }
        
        .qty-input { 
            width: 60px; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 8px; 
            text-align: center; 
            font-weight: bold; 
            font-size: 16px; /* منع الزووم */
        }
        .note-input { 
            flex: 1; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 8px; 
            font-size: 16px; /* منع الزووم */
            background: #fafafa;
        }
        .qty-input:focus, .note-input:focus { border-color: var(--primary); background: white; outline: none; }

        /* إضافة عنصر جديد */
        .add-new-box { 
            margin-top: 20px; 
            background: #fff; 
            padding: 10px; 
            border-radius: 10px; 
            border: 1px dashed #ccc; 
            display: flex; gap: 8px; 
        }
        .add-new-box input { 
            flex: 1; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 6px; 
            font-size: 16px;
            outline: none;
        }
        .add-btn { background: #333; color: white; border: none; padding: 0 15px; border-radius: 6px; font-weight: bold; }

        /* --- زر الإرسال --- */
        .send-container { 
            position: fixed; 
            bottom: 25px; 
            left: 20px; 
            right: 20px; 
            z-index: 999; 
            display: flex;
            justify-content: center;
            pointer-events: none; /* عشان نسمح باللمس حواليه لو الزرار مش واخد الشاشة كلها */
        }
        .send-btn { 
            pointer-events: auto;
            width: 100%;
            max-width: 400px;
            background: var(--primary); 
            color: white; 
            border: none; 
            padding: 16px; 
            font-size: 1.1rem; 
            font-weight: bold; 
            border-radius: 16px; 
            cursor: pointer; 
            box-shadow: 0 8px 20px rgba(37, 211, 102, 0.3); 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            gap: 10px; 
            transition: transform 0.1s; 
        }
        .send-btn:active { transform: scale(0.97); }

        /* --- Media Queries للتظبيط الدقيق --- */
        @media (max-width: 380px) {
            /* للشاشات الصغيرة جداً */
            .grid-container { grid-template-columns: repeat(2, 1fr); gap: 10px; }
            .cat-card { padding: 10px; height: 100px; }
            .cat-icon { font-size: 1.8rem; }
            .cat-card h3 { font-size: 0.9rem; }
        }

        /* أنيميشن */
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        
        /* نتائج البحث */
        .search-group-title { font-size: 0.9rem; color: #777; margin: 15px 5px 5px 0; font-weight: bold; }
        .no-results { text-align: center; color: #999; margin-top: 30px; font-size: 1.1rem; }

    </style>
</head>
<body>

<div class="app-header">
    <h1>🛒 طلبات البيت</h1>
</div>

<div class="container">
    
    <div id="dashboard-view">
        <div class="search-wrapper">
            <span class="search-icon">🔍</span>
            <input type="text" class="global-search-input" id="globalSearch" placeholder="دوري على أي حاجة هنا..." onkeyup="handleGlobalSearch()">
        </div>

        <div class="grid-container" id="cards-container">
            </div>

        <div id="global-results" style="display: none;"></div>
    </div>

    <div id="tabs-view">
        <div class="nav-bar">
            <button class="back-btn" onclick="showDashboard()">
                <span>➜</span> القائمة الرئيسية
            </button>
            <span class="cat-title-display" id="current-cat-title"></span>
        </div>

        <input type="search" class="local-search-input" id="localSearch" placeholder="بحث في القسم ده..." onkeyup="handleLocalSearch()">

        <div id="items-list-container"></div>
    </div>

</div>

<div class="send-container">
    <button type="button" class="send-btn" onclick="sendOrder()">
         إرسال الطلبات واتساب 📤
    </button>
</div>

<script>
    // ---------------------------------------------------------
    //  البيانات (نفس بياناتك السابقة)
    // ---------------------------------------------------------
    const defaultData = {
        veg: { title: "خضروات وفاكهة", icon: "🥬", items: ["طماطم", "خيار", "بصل", "بطاطس", "فلفل", "رومي", "ليمون", "جزر", "كوسة", "باذنجان", "ثوم", "تفاح", "برتقال", "موز", "جوافة", "منجه", "بطيخ", "كسبره", "بقدونس", "شبت"] },
        supermarket: { title: "سوبر ماركت", icon: "🛒", items: ["أرز", "مكرونة اسباكتي", "مكرونة ريش", "زيت", "سكر", "شاي", "بن", "ملح", "خل", "دقيق", "علبه تونه", "علبه ذره", "كاتشب", "مايوناز", "علبه مشروم", "برطمان صلصة", "علب صلصة", "زجاجه دبس رومان", "زجاجه باربكيو", "زجاجه صويا صوص", "زجاجه مسطرده", "كياس خمريه", "كيس شعريه", "علبه مرقة دجاج", "بسكوت شوفان", "بيض", "لبان", "مربى", "حلاوة", "عسل أبيض", "عسل اسود", "سمنة", "اوكسي بودر", "ببسي", "سيفين"] },
        attar: { title: "العطار", icon: "🏺", items: ["كمون", "كسبره ناشفة", "كبيبه صيني", "ديمجلاس", "زجاجه جوزه الطيب", "كركوم", "كاري", "بودره زعفران", "توابل محشي", "رز بسمتي"] },
        meat: { title: "لحوم وفراخ", icon: "🍗", items: ["لحمة مفرومة", "لحمة قطع", "بوفتيك", "بانيه", "وراك فراخ", "فرخة كاملة", "كبد وقوانص", "سجق"] },
        breakfast: { title: "فطار وعشاء", icon: "🍳", items: ["علبه فول", "ساندوتش فول", "طعمية", "ساندوتش طعمية", "طبق بطاطس صوابع", "كيس شيبسي", "بتنجان مقلي", "بتنجان مخلل", "فطيره", "عيش", "فينو"] },
        dairy: { title: "ألبان وجبن", icon: "🧀", items: ["لبن", "علبه لبن", "جبنه بيضاء", "جبنه رومي", "جبنه فلامنك", "جبنه شيدر", "تركي مدخن", "لانشون", "زبادي عادي", "زبادي لايت", "قشطة"] },
        bakery: { title: "الفرن", icon: "🥖", items: ["فينو عادي", "فينو سن", "بقسماط", "دوناتس"] },
        soap: { title: "منظفات", icon: "🧽", items: ["صابون اصفر", "صابون اخضر", "ديتول", "دوني", "كلور الوان", "برسيل جل", "عصايه خشب للمقاشه", "أكياس زباله كبيره", "اكياس بلاستك", "بلادج", "جلانس", "سلك ناعم", "بريل", "باكت مناديل", "باكت مناديل مطبخ", "مناديل تواليت", "صابون وش", "شامبو"] },
        others: { title: "أخرى", icon: "✨", items: [] }
    };

    let appData = {};
    let currentCategoryKey = '';

    function loadData() {
        const saved = localStorage.getItem('myGroceryData_v5');
        appData = saved ? JSON.parse(saved) : JSON.parse(JSON.stringify(defaultData));
    }
    function saveData() { localStorage.setItem('myGroceryData_v5', JSON.stringify(appData)); }

    // --- DOM Rendering ---
    function renderDashboard() {
        const grid = document.getElementById('cards-container');
        grid.innerHTML = '';
        Object.keys(appData).forEach(key => {
            const cat = appData[key];
            const card = document.createElement('div');
            card.className = 'cat-card';
            card.onclick = () => openCategory(key);
            card.innerHTML = `<div class="cat-icon">${cat.icon}</div><h3>${cat.title}</h3>`;
            grid.appendChild(card);
        });
    }

    function showDashboard() {
        document.getElementById('tabs-view').style.display = 'none';
        document.getElementById('dashboard-view').style.display = 'block';
        document.getElementById('globalSearch').value = '';
        document.getElementById('global-results').style.display = 'none';
        document.getElementById('cards-container').style.display = 'grid';
        window.scrollTo(0,0);
    }

    function openCategory(key) {
        currentCategoryKey = key;
        document.getElementById('dashboard-view').style.display = 'none';
        document.getElementById('tabs-view').style.display = 'block';
        document.getElementById('current-cat-title').textContent = appData[key].title;
        document.getElementById('localSearch').value = '';
        renderItemsList(key);
        window.scrollTo(0,0);
    }

    function renderItemsList(key, filterText = '') {
        const container = document.getElementById('items-list-container');
        container.innerHTML = '';
        const items = appData[key].items;
        
        items.forEach(item => {
            if (item.includes(filterText)) {
                container.appendChild(createItemElement(item, key));
            }
        });

        // Add New Box
        const addBox = document.createElement('div');
        addBox.className = 'add-new-box';
        addBox.innerHTML = `
            <input type="text" id="new-item-input" placeholder="ضيفي حاجة جديدة...">
            <button type="button" class="add-btn" onclick="addNewItem()">+</button>
        `;
        container.appendChild(addBox);
    }

    function createItemElement(itemName, categoryKey) {
        const div = document.createElement('div');
        div.className = 'item-container';
        div.innerHTML = `
            <div class="main-row">
                <label class="item-label">
                    <input type="checkbox" name="${itemName}" data-cat="${categoryKey}" onchange="toggleDetails(this)">
                    <span class="item-text">${itemName}</span>
                </label>
            </div>
            <div class="details-row">
                <input type="number" class="qty-input" placeholder="العدد" value="1" onclick="this.select()">
                <input type="text" class="note-input" placeholder="ملحوظة...">
            </div>
        `;
        return div;
    }

    function toggleDetails(checkbox) {
        const detailsRow = checkbox.closest('.item-container').querySelector('.details-row');
        if (checkbox.checked) {
            detailsRow.classList.add('show');
            // Timeout to allow UI animation before focus
            setTimeout(() => detailsRow.querySelector('.qty-input').focus(), 100);
        } else {
            detailsRow.classList.remove('show');
            detailsRow.querySelector('.qty-input').value = '1';
            detailsRow.querySelector('.note-input').value = '';
        }
    }

    function addNewItem() {
        const input = document.getElementById('new-item-input');
        const val = input.value.trim();
        if (val) {
            appData[currentCategoryKey].items.push(val);
            saveData();
            renderItemsList(currentCategoryKey);
        }
    }

    // --- Search Logic ---
    function handleLocalSearch() {
        renderItemsList(currentCategoryKey, document.getElementById('localSearch').value.trim());
    }

    function handleGlobalSearch() {
        const query = document.getElementById('globalSearch').value.trim();
        const resultsContainer = document.getElementById('global-results');
        const cardsContainer = document.getElementById('cards-container');

        if (query === '') {
            resultsContainer.style.display = 'none';
            cardsContainer.style.display = 'grid';
            return;
        }

        cardsContainer.style.display = 'none';
        resultsContainer.style.display = 'block';
        resultsContainer.innerHTML = '';
        let foundAny = false;

        Object.keys(appData).forEach(key => {
            const catItems = appData[key].items.filter(item => item.includes(query));
            if (catItems.length > 0) {
                foundAny = true;
                const title = document.createElement('div');
                title.className = 'search-group-title';
                title.textContent = appData[key].title;
                resultsContainer.appendChild(title);
                catItems.forEach(item => resultsContainer.appendChild(createItemElement(item, key)));
            }
        });
        
        if(!foundAny) resultsContainer.innerHTML = '<div class="no-results">مفيش حاجة بالاسم ده 🤷‍♂️</div>';
    }

    // --- Send WhatsApp ---
    function sendOrder() {
        const checkedBoxes = document.querySelectorAll('input[type="checkbox"]:checked');
        if (checkedBoxes.length === 0) {
            alert("القائمة فاضية يا ست الكل! اختاري الأول 😄");
            return;
        }

        let orderSummary = {};
        checkedBoxes.forEach(chk => {
            const catKey = chk.getAttribute('data-cat');
            const name = chk.name;
            const row = chk.closest('.item-container');
            const qty = row.querySelector('.qty-input').value || "1";
            const note = row.querySelector('.note-input').value.trim();
            
            if (!orderSummary[catKey]) orderSummary[catKey] = [];
            let line = `▫️ ${name}: ${qty}`;
            if(note) line += ` (${note})`;
            orderSummary[catKey].push(line);
        });

        let message = "*🛒 طلبات البيت:* %0a";
        Object.keys(orderSummary).forEach(catKey => {
            message += `%0a*━━ ${appData[catKey].title} ━━*%0a`;
            orderSummary[catKey].forEach(l => message += l + "%0a");
        });

        const myNumber = "201112233512"; 
        window.open(`https://wa.me/${myNumber}?text=${message}`, '_blank');
    }

    // Init
    loadData();
    renderDashboard();
</script>

</body>
</html>
