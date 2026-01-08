# OJ.UZ БҮГДИЙГ ХЭРХЭН ТАТАХ ВЭ?

Би танд 4 арга зааж өгье - хамгийн хялбараас эхлээд:

---

## 🥇 АРГА 1: GitHub-аас бэлэн датаг татах (ХАМГИЙН ХЯЛБАР!)

Аль хэдийн бэлэн байгаа OI Checklist repositories-уудаас татаж авах.

### Алхам 1: JSON файл татах

Терминал дээр дараах командуудыг ажиллуул:

```bash
# Аль нэгийг сонгоно уу:

# Сонголт 1: labs-asterisk version
wget https://raw.githubusercontent.com/labs-asterisk/oichecklist/main/src/data/problem_data.json

# Сонголт 2: avighnac version (илүү шинэ)
# Энэ repository нь илүү олон мэдээлэлтэй
```

### Алхам 2: JSON-оос oj.uz бодлогуудыг гаргах

```bash
# Зөвхөн oj.uz холбоосууд гаргах
cat problem_data.json | grep '"link"' | grep 'oj.uz' | sed 's/.*view\///' | sed 's/".*//' > oj_uz_aliases.txt

# Эсвэл Python ашиглах:
python3 << 'EOF'
import json

with open('problem_data.json', 'r') as f:
    data = json.load(f)

aliases = []

# JSON structure-ээс бодлогууд түүх
for section in data:
    if 'years' in section:
        for year in section['years']:
            if 'problems' in year:
                for prob in year['problems']:
                    if 'link' in prob and 'oj.uz' in prob['link']:
                        alias = prob['link'].split('/')[-1]
                        aliases.append(alias)

# Файлд хадгалах
with open('oj_uz_all_aliases.txt', 'w') as f:
    for alias in aliases:
        f.write(f"{alias}\n")

print(f"✅ {len(aliases)} бодлого хадгалагдлаа!")
EOF
```

**Давуу тал:** Хамгийн хялбар, удаан үргэлжлэхгүй  
**Сул тал:** Зарим шинэ бодлогууд байхгүй байж болно

---

## 🥈 АРГА 2: Browser Console ашиглах (Хурдан!)

### Алхам 1: Browser нээх

1. Chrome эсвэл Firefox дээр `https://oj.uz/problems` нээ
2. `F12` дарж Developer Tools нээ
3. **Console** таб руу оч

### Алхам 2: JavaScript код ажиллуулах

Доорх кодыг Console-д붙여넣기хийж Enter дар:

```javascript
// Нэг хуудасны бодлогууд цуглуулах
let problems = [];
document.querySelectorAll('table tbody tr').forEach(row => {
    let cells = row.querySelectorAll('td');
    if (cells.length > 0) {
        let alias = cells[0].textContent.trim();
        if (alias && !alias.includes('Alias')) {
            problems.push(alias);
        }
    }
});

// Үр дүн харуулах
console.log('✅ ' + problems.length + ' бодлого олдлоо:');
console.log(problems.join('\n'));

// Clipboard-руу автоматаар хуулах
copy(problems.join('\n'));
alert('✅ ' + problems.length + ' бодлого clipboard-руу хуулагдлаа!\nCtrl+V дарж файлд хадгална уу.');
```

### Алхам 3: Бүх хуудсуудыг давтах

Дараах кодыг ажиллуулж **БҮХ** хуудсыг автоматаар давтана:

```javascript
// БҮХ хуудсыг автоматаар scrape хийх
async function scrapeAllPages() {
    let allProblems = [];
    let currentPage = 1;
    let maxPages = 57; // oj.uz дээр ~57 хуудас байдаг
    
    console.log('🔄 Бүх хуудас scrape хийж байна...');
    
    for (let page = 1; page <= maxPages; page++) {
        try {
            let url = page === 1 ? '/problems' : `/problems?page=${page}`;
            
            // Fetch page
            let response = await fetch(url);
            let html = await response.text();
            let parser = new DOMParser();
            let doc = parser.parseFromString(html, 'text/html');
            
            // Extract aliases
            let rows = doc.querySelectorAll('table tbody tr');
            rows.forEach(row => {
                let cells = row.querySelectorAll('td');
                if (cells.length > 0) {
                    let alias = cells[0].textContent.trim();
                    if (alias && !alias.includes('Alias')) {
                        allProblems.push(alias);
                    }
                }
            });
            
            console.log(`✓ Хуудас ${page}/${maxPages}: ${rows.length} бодлого`);
            
            // Be nice to the server
            await new Promise(resolve => setTimeout(resolve, 500));
            
        } catch (error) {
            console.error(`✗ Хуудас ${page} дээр алдаа:`, error);
        }
    }
    
    console.log('\n' + '='.repeat(60));
    console.log(`✅ ДУУССАН! Нийт ${allProblems.length} бодлого`);
    console.log('='.repeat(60));
    console.log(allProblems.join('\n'));
    
    // Clipboard-руу хуулах
    copy(allProblems.join('\n'));
    
    // File татах (browser дээр)
    let blob = new Blob([allProblems.join('\n')], {type: 'text/plain'});
    let link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'oj_uz_all_problems.txt';
    link.click();
    
    alert('✅ Дууслаа! ' + allProblems.length + ' бодлого\nФайл татагдаж байна...');
    
    return allProblems;
}

// Ажиллуулах
scrapeAllPages();
```

**⏱️ Хугацаа:** ~30 секунд (57 хуудас × 0.5 сек/хуудас)  
**Давуу тал:** Хурдан, бүрэн жагсаалт  
**Сул тал:** Browser нээлттэй байх хэрэгтэй

---

## 🥉 АРГА 3: Python Script (Программчлалын арга)

### Алхам 1: Dependencies суулгах

```bash
pip install beautifulsoup4 requests
```

### Алхам 2: Script үүсгэх

`scrape_ojuz.py` файл үүсгээд доорх кодыг хуул:

```python
#!/usr/bin/env python3
import requests
from bs4 import BeautifulSoup
import time

def scrape_all_ojuz():
    all_problems = []
    base_url = "https://oj.uz/problems"
    
    print("🔄 Scraping oj.uz...")
    
    # Нийт хуудасны тоог олох
    response = requests.get(base_url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Pagination links шалгах
    max_page = 57  # Default
    
    # Бүх хуудсыг давтах
    for page in range(1, max_page + 1):
        url = f"{base_url}?page={page}" if page > 1 else base_url
        
        try:
            print(f"  Хуудас {page}/{max_page}...", end=" ")
            response = requests.get(url, timeout=10)
            soup = BeautifulSoup(response.text, 'html.parser')
            
            table = soup.find('table')
            if table:
                rows = table.find_all('tr')[1:]  # Skip header
                count = 0
                
                for row in rows:
                    cols = row.find_all('td')
                    if len(cols) >= 1:
                        alias = cols[0].text.strip()
                        if alias:
                            all_problems.append(alias)
                            count += 1
                
                print(f"✓ {count} бодлого")
            
            time.sleep(0.5)  # Be nice to server
            
        except Exception as e:
            print(f"✗ Алдаа: {e}")
    
    return all_problems

if __name__ == "__main__":
    problems = scrape_all_ojuz()
    
    # Файлд хадгалах
    with open('oj_uz_all_problems.txt', 'w') as f:
        for p in problems:
            f.write(f"{p}\n")
    
    print(f"\n✅ {len(problems)} бодлого хадгалагдлаа!")
    print("📄 Файл: oj_uz_all_problems.txt")
```

### Алхам 3: Ажиллуулах

```bash
python3 scrape_ojuz.py
```

**⏱️ Хугацаа:** ~30-60 секунд  
**Давуу тал:** Автоматжуулалт, давтаж ажиллуулж болно  
**Сул тал:** Python dependencies хэрэгтэй

---

## 🏅 АРГА 4: Manual хуулах (Хамгийн найдвартай)

### Хэрэв бүх автомат арга ажиллахгүй бол:

1. **oj.uz/problems** руу оч
2. Хуудас тус бүрээр нээ (page=1, page=2, ...)
3. Бодлогуудын нэрийг text editor дээр хуулах
4. 57 хуудас хүртэл давтах

**Эсвэл:**

- OI Checklist webapp ашиглах: https://checklist.spoi.org.in/
- Manual export хийх

---

## 🎯 Та юуг хүсэж байна?

Би танд **одоо** яг юуг хийж өгөхийг хүсч байна вэ?

**A)** GitHub-аас бэлэн датаг татаж өгөх (5 сек)  
**B)** Browser console код өгөх (1 минут)  
**C)** Python script өгөх (бэлэн болсон)  
**D)** Би өөрөө scrape хийж өгөх (30+ минут)

Сонголтоо хэлээрэй! 🚀
