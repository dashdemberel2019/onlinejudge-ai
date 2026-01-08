# CEOI 2013 - TRAM Бодлого

## Бодлогын тодорхойлолт

Загребын шинэ трамд N мөр (1-ээс N хүртэл) болон 2 багана (1, 2) байна. Зорчигчид суудал сонгохдоо:

1. **Хоосон бол:** (1, 1) суудалд суух
2. **Эзлэгдсэн бол:** Эзлэгдсэн суудлуудаас хамгийн хол зайтай чөлөөтэй суудалд суух
3. **Тэнцүү зайтай бол:** Бага мөр → Бага багана гэж эрэмбэлэх

**Зай:** Euclidean зай = √((row₁-row₂)² + (col₁-col₂)²)

## Оролт

```
N M                  # Мөрийн тоо, үйл явдлын тоо
E | L P              # E = орж ирэх, L P = P-р зорчигч гарах
...
```

## Гаралт

E үйл явдал бүрт сонгосон суудлын row, column хэвлэх.

## Жишээ 1

**Оролт:**
```
3 7
E      # Зорчигч 1 орж ирэв
E      # Зорчигч 2 орж ирэв
E      # Зорчигч 3 орж ирэв
L 2    # Зорчигч 2 гарсан
E      # Зорчигч 4 орж ирэв
L 1    # Зорчигч 1 гарсан
E      # Зорчигч 5 орж ирэв
```

**Гаралт:**
```
1 1    # Зорчигч 1: хоосон → (1,1)
3 2    # Зорчигч 2: (1,1)-ээс хол → (3,2)
1 2    # Зорчигч 3: (1,1), (3,2)-оос хол → (1,2)
3 1    # Зорчигч 4: 2 гарсан, (1,1), (1,2)-оос хол → (3,1)
1 1    # Зорчигч 5: 1 гарсан, (1,2), (3,1)-ээс хол → (1,1)
```

**Дүрслэл:**
```
      Багана 1    Багана 2
Мөр 1:   [5]         [3]
Мөр 2:   [ ]         [ ]
Мөр 3:   [4]         [X]  (X = арилсан)
```

## Алгоритм

### Энгийн шийдэл (O(N·M²)) - 65 оноо

```cpp
for бүх чөлөөтэй суудал (row, col):
    min_dist = INF
    for бүх эзлэгдсэн суудал (r, c):
        dist = sqrt((row-r)² + (col-c)²)
        min_dist = min(min_dist, dist)
    
    if (min_dist > best_dist):
        best = (row, col)
    else if (min_dist == best_dist):
        if (row < best.row) or (row == best.row && col < best.col):
            best = (row, col)
```

### Оновчтой шийдэл (100 оноо)

**Гол санаа:** Бүх N×2 суудлыг шалгах шаардлагагүй. Зөвхөн **candidate** суудлуудыг шалгах хангалттай:

1. **Хязгаарын цэгүүд:** (1,1), (1,2), (N,1), (N,2)
2. **Эзлэгдсэн суудлын ойролцоо:** Эзлэгдсэн мөрийн өмнө/дараа
3. **Interval дундын цэгүүд:** Эзлэгдсэн мөрүүдийн дунд

**Candidate тоо:** O(M) ширхэг  
**Ойрхон суудал олох:** Set + binary search = O(log M)  
**Нийт цаг:** O(M² log M)

```cpp
set<int> col[2];  // col[0]=багана1, col[1]=багана2

// Candidate цэгүүд
candidates = {1, N}  // хязгаар
for c in [0, 1]:
    for r in col[c]:
        candidates += {r-1, r+1}  // ойролцоо
    
    // Interval дунд
    for i in range(len(col[c])-1):
        mid = (col[c][i] + col[c][i+1]) / 2
        candidates += {mid}

// Бүх candidate-ийг шалгах
for row in candidates:
    for col in [0, 1]:
        if эзлэгдээгүй(row, col):
            // Binary search ашиглан ойрхон олох
            lower = col[c].lower_bound(row)
            upper = col[c].upper_bound(row)
            
            dist = min(abs(row - *lower), abs(row - *upper))
            ...
```

## Код

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int N, M;
    cin >> N >> M;
    
    set<pair<int, int>> occupied;
    map<int, pair<int, int>> passenger_seat;
    
    for (int i = 1; i <= M; i++) {
        char type;
        cin >> type;
        
        if (type == 'E') {
            pair<int, int> best = {1, 1};
            
            if (occupied.empty()) {
                best = {1, 1};
            } else {
                double max_min_dist = -1;
                
                for (int row = 1; row <= N; row++) {
                    for (int col = 1; col <= 2; col++) {
                        if (occupied.count({row, col})) continue;
                        
                        double min_dist = 1e18;
                        for (auto [r, c] : occupied) {
                            double dr = row - r;
                            double dc = col - c;
                            min_dist = min(min_dist, sqrt(dr*dr + dc*dc));
                        }
                        
                        if (min_dist > max_min_dist + 1e-9) {
                            max_min_dist = min_dist;
                            best = {row, col};
                        } else if (abs(min_dist - max_min_dist) < 1e-9) {
                            if (row < best.first || 
                                (row == best.first && col < best.second)) {
                                best = {row, col};
                            }
                        }
                    }
                }
            }
            
            occupied.insert(best);
            passenger_seat[i] = best;
            cout << best.first << " " << best.second << "\n";
            
        } else {
            int P;
            cin >> P;
            occupied.erase(passenger_seat[P]);
            passenger_seat.erase(P);
        }
    }
    
    return 0;
}
```

## Онооны хуваарилалт

- **25 оноо:** N ≤ 150, M ≤ 150
- **45 оноо:** N ≤ 1500, M ≤ 1500  
- **65 оноо:** N ≤ 150000, M ≤ 1500 (O(N·M²) шийдэл)
- **100 оноо:** N ≤ 150000, M ≤ 30000 (Оновчтой candidate шийдэл)

## Анхаарах зүйлс

1. **Floating point харьцуулалт:** 1e-9 epsilon ашиглах
2. **Эрэмбэлэх:** Тэнцүү зайтай үед row → column
3. **Set ашиглалт:** Binary search-д хурдан
4. **Edge cases:** Хоосон трам, ганц суудал үлдсэн

## Холбоос

- **OJ.uz:** https://oj.uz/problem/view/CEOI13_tram
- **CEOI 2013:** Day 1, Task 1
- **Difficulty:** Medium-Hard
