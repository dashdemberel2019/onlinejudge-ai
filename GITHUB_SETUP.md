# GitHub Repository Setup Guide

Танай судалгааг GitHub дээр байрлуулах заавар.

## 📋 Шаардлагатай зүйлс

- GitHub account
- Git суулгасан (эсвэл GitHub Desktop)
- Бэлэн файлууд

---

## 🚀 Алхам 1: GitHub дээр repository үүсгэх

### Web дээр:

1. GitHub-д нэвтэр: https://github.com
2. "+" дарж → "New repository"
3. Мэдээлэл оруул:
   ```
   Repository name: competitive-programming-research
   Description: Comprehensive research on competitive programming, AI detection, and contest management
   ✓ Public (эсвэл Private)
   ✗ Initialize with README (бид аль хэдийн бүтээсэн)
   License: None (бид аль хэдийн нэмсэн)
   ```
4. "Create repository" дар

---

## 💻 Алхам 2: Файлууд upload хийх

### Арга A: Web дээр upload (Хялбар)

1. Repository хуудас дээр "Add file" → "Upload files"
2. Бүх .md файлуудыг чирж оруул:
   ```
   README.md
   LICENSE
   CONTRIBUTING.md
   .gitignore
   AI_DETECTION_VERIFIED_2025.md
   ... (бусад бүх .md файлууд)
   ```
3. Commit message: "Initial commit: Research documents"
4. "Commit changes" дар

---

### Арга B: Git командаар (Мэргэжлийн)

#### Terminal/PowerShell дээр:

```bash
# 1. Repository clone хийх
git clone https://github.com/YOUR_USERNAME/competitive-programming-research.git
cd competitive-programming-research

# 2. Файлууд хуулах
# Бүх .md файлуудыг энэ folder-т хуул

# 3. Git add
git add .

# 4. Commit
git commit -m "Initial commit: Research documents"

# 5. Push
git push origin main
```

---

### Арга C: GitHub Desktop (Хамгийн хялбар)

1. GitHub Desktop татаж суулга
2. "Clone a repository" 
3. YOUR_USERNAME/competitive-programming-research сонго
4. Файлуудыг repository folder-т хуул
5. "Commit to main" дар
6. "Push origin" дар

---

## 📝 Алхам 3: README засварлах

### Repository-д нэмэх:

1. README.md-д нэмэх:
   ```markdown
   ## 👤 Author
   
   **Turbold** - Computer Science Student
   - Experience: 60+ programming olympiads organized
   - Focus: Contest management, AI detection, competitive programming
   
   ## 📧 Contact
   
   - GitHub: [@YOUR_USERNAME](https://github.com/YOUR_USERNAME)
   - Issues: For questions/discussions
   ```

2. Commit:
   ```bash
   git add README.md
   git commit -m "Add author information"
   git push
   ```

---

## 🎨 Алхам 4: Repository-г гоёх

### About section (Баруун дээд талд):

```
⚙️ Settings icon дар

Description: 
"Comprehensive research on competitive programming: AI detection methods, 
contest management (CMS), coding speed analysis. Based on 60+ olympiad 
organization experience. Mongolian & English."

Website: (хэрэв байвал)

Topics: 
competitive-programming
cms
ai-detection
olympiad
contest-management
education
mongolian
research
```

### README badges нэмэх (Optional):

```markdown
# Competitive Programming Research

![License](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)
![Status](https://img.shields.io/badge/Status-Active-success)
![Updated](https://img.shields.io/badge/Updated-January%202025-blue)
![Language](https://img.shields.io/badge/Language-Mongolian%20%7C%20English-orange)
```

---

## 📊 Алхам 5: GitHub Pages (Optional)

### Вебсайт болгох:

1. Settings → Pages
2. Source: main branch
3. Select: / (root)
4. Save
5. URL гарна: `https://YOUR_USERNAME.github.io/competitive-programming-research`

**Давуу тал:** 
- Markdown автоматаар HTML болно
- Гоё харагдана
- Хайлт хийхэд хялбар

---

## 🔧 Алхам 6: Repository Settings

### Recommended settings:

**General:**
```
✓ Allow merge commits
✓ Allow squash merging
✓ Allow rebase merging
✓ Automatically delete head branches
```

**Issues:**
```
✓ Issues enabled
Templates: Create issue templates
Labels: bug, enhancement, question, outdated
```

**Discussions:** (Optional)
```
✓ Enable discussions
Categories: General, Q&A, Ideas
```

---

## 📢 Алхам 7: Сурталчлах

### LinkedIn/Facebook post:

```
🎓 Шинэ судалгаа: AI Detection in Competitive Programming

60+ олимпиадын туршлагад суурилсан:
✓ AI илрүүлэх аргууд (90%+ accuracy)
✓ Contest Management System заавар
✓ Coding speed analysis
✓ Verified эх сурвалжууд

Үнэгүй, нээлттэй: [GitHub link]

#CompetitiveProgramming #Research #AI #Education
```

### Codeforces blog:

```
Title: AI Detection Research - 60+ Contest Experience

Content:
Hi Codeforces!

I've compiled comprehensive research on AI detection based on 
organizing 60+ programming olympiads. Includes:

- Verified detection methods (Moss, GPTSniffer, Proctoring)
- Practical recommendations for contest organizers  
- CMS setup guide
- Real case studies

Language: Mainly Mongolian, some English
Open source: [GitHub link]

Feedback welcome!
```

---

## 🔄 Алхам 8: Updates хийх

### Файл засварлах:

```bash
# 1. Өөрчлөлт хий
# 2. Add & commit
git add .
git commit -m "Update: AI detection statistics"

# 3. Push
git push
```

### Web дээр:

1. Файл дарах → ✏️ Edit
2. Өөрчлөлт хий
3. Commit changes

---

## 📈 Алхам 9: Stars цуглуулах

### Чанартай repository-н шинж:

- ✓ Detailed README
- ✓ Clear structure
- ✓ License
- ✓ Contributing guide
- ✓ Regular updates
- ✓ Respond to issues
- ✓ Good documentation

### Promotion:

- Share on social media
- Post in relevant communities
- Tag relevant topics
- Cross-reference in discussions

---

## ⚡ Quick Commands Cheat Sheet

```bash
# Status шалгах
git status

# Changes харах
git diff

# Бүх өөрчлөлт нэмэх
git add .

# Commit хийх
git commit -m "Your message"

# Push
git push

# Pull (updates татах)
git pull

# Branch үүсгэх
git checkout -b feature-name

# Branch солих
git checkout main
```

---

## 🆘 Common Issues

### Issue 1: Permission denied

```bash
# SSH key setup хийх
ssh-keygen -t ed25519 -C "your_email@example.com"
# GitHub Settings → SSH keys → Add SSH key
```

### Issue 2: Merge conflicts

```bash
# Файл засварлаад
git add .
git commit -m "Resolve merge conflict"
git push
```

### Issue 3: Forgot to pull

```bash
git pull --rebase
# Conflicts шийд
git push
```

---

## ✅ Success Checklist

- [ ] Repository үүссэн
- [ ] Бүх файл upload хийсэн  
- [ ] README засварласан
- [ ] License нэмсэн
- [ ] About section бөглөсөн
- [ ] Topics нэмсэн
- [ ] Эхний star авсан (өөрөө өг 😄)
- [ ] Social media дээр share хийсэн

---

## 🎉 Дууссан!

Одоо танай судалгаа GitHub дээр:
- Олон нийтэд нээлттэй
- Version control бүхий
- Collaboration боломжтой
- Professional-р харагдана

**Амжилт хүсье!** 🚀

---

**Асуулт байвал:** GitHub Issues ашигла эсвэл ChatGPT/Claude-д асуу! 😊
