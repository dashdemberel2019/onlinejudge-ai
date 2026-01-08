# TRAM Бодлогын Тестийн Багц

## 📋 Агуулга

### Үндсэн файлууд:
1. **tram_anti_ai_tests.md** - AI-д хүндрэлтэй тестүүдийн тайлбар
2. **gen_tram_test.py** - Тест генератор програм
3. **check_tram.py** - Гаралт шалгагч програм
4. **test1-4.in/out** - Бэлэн тест файлууд

## 🚀 Хэрэглэх заавар

### 1. Тест генератор ашиглах

```bash
# Энгийн тест (N=10, M=5)
python3 gen_tram_test.py simple 10 5 > my_test.in

# Арилгалттай тест (N=6, 5 орох, 2 гарах)
python3 gen_tram_test.py remove 6 5 2 > my_test.in

# Ээлжлэх тест
python3 gen_tram_test.py alternate 6 6 > my_test.in

# Stress тест (N=1000, M=100, 30% remove)
python3 gen_tram_test.py stress 1000 100 0.3 > my_test.in

# Симметр тест
python3 gen_tram_test.py symmetry > my_test.in

# Floating point precision тест
python3 gen_tram_test.py precision > my_test.in

# Maximum хязгаарын тест (N=150000, M=30000)
python3 gen_tram_test.py max > my_test.in
```

### 2. Шийдлээ тестлэх

```bash
# C++ код compile хийх
g++ -std=c++17 -O2 -o tram tram_final.cpp

# Тест ажиллуулах
./tram < test1.in > my_output.txt

# Гаралт шалгах
cat test1.in my_output.txt | python3 check_tram.py
```

### 3. Бүх тестийг автоматаар шалгах

```bash
#!/bin/bash
for i in 1 2 3 4; do
    echo "=== Test $i ==="
    ./tram < test${i}.in > output${i}.txt
    
    # Compare with expected
    if diff -q output${i}.txt test${i}.out > /dev/null; then
        echo "✓ PASS"
    else
        echo "✗ FAIL"
        echo "Expected:"
        cat test${i}.out
        echo "Got:"
        cat output${i}.txt
    fi
    echo
done
```

## 📝 Тестүүдийн тайлбар

### Test 1: Edge case (Minimum)
- N=1, M=1
- Хамгийн жижиг тохиолдол
- **AI алдаа:** Edge case алгасах

### Test 2: Column comparison
- N=5, M=5
- Багана 1 ба 2-ын харьцуулалт
- **AI алдаа:** Багана эрэмбэ буруу

### Test 3: Multiple removals
- N=4, M=10
- Олон L үйлдэл холилдсон
- **AI алдаа:** State tracking алдах

### Test 4: Symmetry breaking
- N=10, M=6
- Симметр тохиолдол
- **AI алдаа:** Tie-breaking rule буруу

## 🎯 AI-д хүндрэлтэй тохиолдлууд

### 1. Floating Point Precision
```cpp
// Буруу
if (dist1 == dist2)  // ❌

// Зөв
if (abs(dist1 - dist2) < 1e-9)  // ✓
```

### 2. Tie-breaking
```cpp
// Буруу: column эхэлж шалгах
if (col < best.col || (col == best.col && row < best.row))  // ❌

// Зөв: row эхэлж шалгах
if (row < best.row || (row == best.row && col < best.col))  // ✓
```

### 3. Index Confusion
```cpp
// Буруу: event номер vs passenger номер
map<int, pair<int,int>> seats;  // ❌ event index

// Зөв
for (int event = 1; event <= M; event++) {
    if (type == 'E') {
        passenger_seat[event] = best;  // ✓ event номер хадгална
    }
}
```

### 4. Output Count
```cpp
// Буруу: M удаа хэвлэх
for (int i = 0; i < M; i++)  // ❌

// Зөв: Зөвхөн E үйлдэлд хэвлэх
if (type == 'E') {
    cout << row << " " << col << "\n";  // ✓
}
```

## 📊 Онооны хязгаар

| Оноо | N хязгаар | M хязгаар | Цаг | Алгоритм |
|------|----------|----------|-----|----------|
| 25   | ≤ 150    | ≤ 150    | O(N·M²) | Brute force |
| 45   | ≤ 1500   | ≤ 1500   | O(N·M²) | Brute force |
| 65   | ≤ 150000 | ≤ 1500   | O(N·M²) | Brute force |
| 100  | ≤ 150000 | ≤ 30000  | O(M²logM) | Candidates + Set |

## 🔍 Checker ашиглах

Checker нь дараах зүйлийг шалгана:

1. ✓ Суудал хязгаарт багтаж байна уу (1 ≤ row ≤ N, 1 ≤ col ≤ 2)
2. ✓ Эзлэгдсэн суудал дахин сонгогдсон уу
3. ✓ Хоосон трам бол (1, 1) эсэх
4. ✓ Хамгийн их зайтай суудал эсэх
5. ✓ Тэнцүү зайтай бол tie-breaking зөв эсэх
6. ✓ L үйлдэл зөв ажиллаж байна уу
7. ✓ Output мөрийн тоо зөв эсэх

## 💡 Зөвлөмж

### Алдаа олоход:
1. Жижиг тест (test1) эхэлж шалгах
2. Print debug мэдээлэл нэмэх
3. Euclidean зай зөв тооцоолж байгаа эсэхийг шалгах
4. Tie-breaking логик шалгах
5. Edge cases-г анхаарах

### Код optimize хийхэд:
1. Brute force эхэлж бичих (65 оноо)
2. Candidate optimization нэмэх (100 оноо)
3. Set + binary search ашиглах
4. Memory-д анхаарах (Large N)

## 📚 Холбогдох материал

- **Бодлогын statement:** CEOI 2013 Day 1
- **Online Judge:** https://oj.uz/problem/view/CEOI13_tram
- **Editorials:** GitHub дээр олон шийдэл байдаг

---

**Амжилт хүсье! 🎓🚀**
