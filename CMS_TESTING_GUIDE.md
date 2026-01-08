# CMS дээр бодлого шалгах заавар

## 📋 Агуулга

1. CMS-ийн тухай
2. Бодлого нэмэх
3. Test data үүсгэх
4. Checker/Grader бичих
5. Шалгах процесс
6. Алдаа засах
7. Tips & Tricks

---

## 🎯 1. CMS-ийн тухай

### CMS гэж юу вэ?

**Contest Management System** - Италийн IOI-гийн бүтээсэн:
- Олимпиад зохион байгуулах систем
- IOI, APIO, CEOI зэрэгт ашигладаг
- GitHub: https://github.com/cms-dev/cms

### Гол бүрэлдэхүүн:

```
AdminWebServer (AWS)  → Contest admin panel
ContestWebServer (CWS) → Participant interface  
EvaluationService     → Judge code
Worker               → Run tests
Database             → PostgreSQL
```

---

## 🔧 2. Бодлого нэмэх

### Арга 1: Web Interface ашиглах

#### Step 1: Login AWS
```bash
# AWS нээх
http://localhost:8889

# Login: admin / admin (default)
```

#### Step 2: Task үүсгэх
```
AWS → Tasks → Add Task

Мэдээлэл оруулах:
- Name: task_name (англи үсэг, underscore)
- Title: "Бодлогын нэр"
- Time limit: 1.0 (секунд)
- Memory limit: 256 (МБ)
- Task type: Batch (эсвэл Communication)
- Submission format: task_name.%l (C++: .cpp, Python: .py)
```

#### Step 3: Statement оруулах
```
→ Statements tab
→ Upload PDF (Монгол/English)
→ Primary language сонгох
```

#### Step 4: Attachments (хэрэв байвал)
```
→ Attachments tab
→ Жишээ: sample input/output
→ Upload files
```

### Арга 2: Command line ашиглах

```bash
# Task үүсгэх
cmsImportTask /path/to/task/

# Task folder бүтэц:
task_name/
├── task.yaml          # Configuration
├── statement/
│   ├── statement.pdf
│   └── statement_en.pdf
├── input/
│   ├── input0.txt
│   ├── input1.txt
│   └── ...
├── output/
│   ├── output0.txt
│   ├── output1.txt
│   └── ...
├── gen/             # Test generator (optional)
├── check/           # Checker (optional)
└── sol/             # Solutions (optional)
```

---

## 📝 3. Test Data үүсгэх

### Batch Task (ердийн бодлого)

#### Арга 1: Manual upload

```bash
# AWS дээр Task нээх
→ Testcases tab
→ Add testcase

Нэг testcase бүрт:
- Input file: input0.txt
- Output file: output0.txt
- Public: ✓ (sample) эсвэл ✗ (hidden)
```

#### Арга 2: Bulk import

```bash
# input/ output/ folder үүсгэх
mkdir -p task/input task/output

# Test files үүсгэх
echo "3 5" > task/input/input0.txt
echo "8" > task/output/output0.txt

echo "10 20" > task/input/input1.txt
echo "30" > task/output/output1.txt

# Import
cmsImportTask task/
```

### Test Generator ашиглах

```python
#!/usr/bin/env python3
# gen/generator.py

import random
import sys

def generate(seed, n):
    random.seed(seed)
    a = random.randint(1, n)
    b = random.randint(1, n)
    print(f"{a} {b}")

if __name__ == "__main__":
    seed = int(sys.argv[1])
    n = int(sys.argv[2])
    generate(seed, n)
```

```bash
# Testcase үүсгэх
python3 gen/generator.py 42 100 > input/input0.txt
./solution < input/input0.txt > output/output0.txt
```

### Subtask үүсгэх

```yaml
# task.yaml
score_type: GroupMin  # Subtask scoring

testcases:
  - input: input0.txt
    output: output0.txt
    public: true
    
  # Subtask 1: N ≤ 100
  - input: input1.txt
    output: output1.txt
    points: 30
    
  - input: input2.txt
    output: output2.txt
    points: 30
    
  # Subtask 2: N ≤ 10000
  - input: input3.txt
    output: output3.txt
    points: 70
```

---

## ⚙️ 4. Checker/Grader бичих

### Default Checker (Diff)

```
Batch task ихэнх тохиолдолд:
→ Output файл яг таарах ёстой
→ Whitespace ignore хийнэ
→ Extra checker шаардлагагүй
```

### Custom Checker

#### Хэзээ ашиглах вэ?
- Олон зөв хариулт байх үед
- Float утга харьцуулах (epsilon)
- Special format шалгах

#### Checker бичих:

```cpp
// check/checker.cpp
#include <iostream>
#include <fstream>
using namespace std;

int main(int argc, char *argv[]) {
    // argv[1] = input file
    // argv[2] = expected output (judge)
    // argv[3] = contestant output
    
    ifstream fin(argv[1]);
    ifstream fexp(argv[2]);
    ifstream fout(argv[3]);
    
    int expected, output;
    fexp >> expected;
    
    if (!(fout >> output)) {
        cout << 0.0 << endl;  // Wrong format
        return 0;
    }
    
    if (output == expected) {
        cout << 1.0 << endl;  // Correct
    } else {
        cout << 0.0 << endl;  // Wrong answer
    }
    
    return 0;
}
```

```bash
# Compile
g++ -std=c++17 -O2 -o checker check/checker.cpp

# Test
./checker input0.txt output0.txt contestant_output.txt
# Output: 1.0 (зөв) эсвэл 0.0 (буруу)
```

#### CMS-д нэмэх:

```yaml
# task.yaml
checker: checker  # checker executable
```

```bash
# AWS дээр:
→ Task Settings
→ Output evaluation: Comparator
→ Upload checker executable
```

### Communication Task (Interactive)

```cpp
// graders/grader.cpp
#include <iostream>
using namespace std;

int main() {
    int secret = 42;
    int queries = 0;
    
    while (queries < 10) {
        int guess;
        cin >> guess;
        queries++;
        
        if (guess == secret) {
            cout << "correct" << endl;
            return 0;
        } else if (guess < secret) {
            cout << "higher" << endl;
        } else {
            cout << "lower" << endl;
        }
    }
    
    cout << "failed" << endl;
    return 1;
}
```

---

## 🧪 5. Шалгах процесс

### User solution submit хийх

#### CWS дээр (Participant interface):

```
1. Login: http://localhost:8888
2. User: test_user
3. Task сонгох
4. Submit:
   - File upload: solution.cpp
   - Language: C++17
   - Submit дарах
```

#### File submit хийх:

```bash
# Direct file submit (testing)
cmsSubmitFiles -c contest_name -u username -t task_name solution.cpp
```

### Submission үр дүн харах

```
CWS дээр:
→ Submissions tab
→ Status:
  - Compiling
  - Evaluating
  - Evaluated

→ Score: 0-100
→ Details: Test-by-test results
```

### Evaluation details:

```
Test 0: ✓ Correct (0.01s, 2MB) - 10 points
Test 1: ✗ Wrong Answer (0.02s, 3MB) - 0 points
Test 2: ✗ Time Limit (1.00s, 5MB) - 0 points
Test 3: ✓ Correct (0.15s, 10MB) - 15 points

Total: 25/100
```

---

## 🔍 6. Алдаа засах

### Compilation Error

```cpp
// Алдаа:
#include <bits/stdc++.h>  // ❌ CMS дээр ажиллахгүй байж болно

// Зөв:
#include <iostream>
#include <vector>
#include <algorithm>
```

**Шалгах:**
```bash
# AWS → Task → Compilation parameters
→ Check C++ version
→ Check compilation flags
```

### Wrong Answer

```bash
# Manual test:
g++ -std=c++17 -O2 solution.cpp -o solution
./solution < input0.txt > my_output.txt
diff my_output.txt output0.txt
```

### Time Limit Exceeded

```python
# Check time limit
→ AWS → Task → Time limit: 1.0s

# Profile code:
time ./solution < large_input.txt
```

### Memory Limit Exceeded

```bash
# Check memory limit
→ AWS → Task → Memory limit: 256MB

# Monitor memory:
valgrind ./solution < input.txt
```

### Runtime Error

```bash
# Test locally:
./solution < input0.txt

# Common issues:
- Array out of bounds
- Division by zero
- Stack overflow (too deep recursion)
```

### Judge Error (System issue)

```
Reasons:
- Worker crashed
- Database connection lost
- Disk full

Solution:
→ AWS → Reevaluate submission
→ Restart services: sudo systemctl restart cms*
```

---

## 💡 7. Tips & Tricks

### Test Data санал:

```
✓ Sample test (public): 1-2 ширхэг
✓ Edge cases: Empty, max, min values  
✓ Subtask tests: Grouped by constraints
✓ Random tests: Large random inputs
✓ Stress tests: Maximum constraints
✓ Corner cases: Special patterns

Нийт: 10-30 testcase
```

### Subtask scoring:

```yaml
# task.yaml
score_type: GroupMin  # Subtask-аас хамгийн багаа авна

testcases:
  # Group 0: Samples (0 оноо)
  - {input: in0.txt, output: out0.txt, points: 0, public: true}
  
  # Group 1: N ≤ 100 (30 оноо)
  - {input: in1.txt, output: out1.txt, points: 30}
  - {input: in2.txt, output: out2.txt, points: 30}
  
  # Group 2: N ≤ 10000 (70 оноо)
  - {input: in3.txt, output: out3.txt, points: 70}
  - {input: in4.txt, output: out4.txt, points: 70}
```

### Checker examples:

#### Float comparison:
```cpp
const double EPS = 1e-6;
if (abs(output - expected) < EPS) {
    cout << 1.0 << endl;  // Correct
}
```

#### Multiple correct answers:
```cpp
if (is_valid_solution(output)) {
    cout << 1.0 << endl;
} else {
    cout << 0.0 << endl;
}
```

#### Partial scores:
```cpp
double score = compute_score(output);
cout << score << endl;  // 0.0 to 1.0
```

### Testing workflow:

```bash
# 1. Бодлого setup
cmsImportTask task/

# 2. Test data үүсгэх
./generator.sh

# 3. Solution шалгах
./test_solution.sh

# 4. CMS дээр import
cmsReimportTask task/

# 5. Manual test submit
cmsSubmitFiles -c contest -u test -t task solution.cpp

# 6. Results шалгах
→ AWS → Submissions
```

### Common mistakes:

```
❌ input/output files буруу нэр (input0.txt, input1.txt, ...)
❌ Testcase-ийн newline үгүй
❌ Large files git-д commit хийх
❌ Checker буруу exit code
❌ Time limit хэт бага
❌ Sample test hidden байх
```

---

## 📚 Нэмэлт материал

### CMS Documentation:
```
→ https://cms.readthedocs.io/
→ Task types
→ Scoring types
→ API reference
```

### Example tasks:
```
→ CMS GitHub: cms-dev/cms
→ Example contests folder
→ IOI tasks: ioi-tasks repository
```

### Tools:

```bash
# CMS commands:
cmsAddAdmin          # Admin нэмэх
cmsAddUser           # User нэмэх
cmsAddContest        # Contest үүсгэх
cmsAddTask           # Task нэмэх
cmsImportTask        # Task import
cmsReimportTask      # Task дахин import
cmsSubmitFiles       # Submit code

# Service commands:
sudo systemctl start cms*     # Бүх service эхлүүлэх
sudo systemctl stop cms*      # Зогсоох
sudo systemctl restart cms*   # Дахин эхлүүлэх
sudo systemctl status cms*    # Status харах
```

---

## 🎯 Практик жишээ: Бүтэн workflow

### 1. Task folder үүсгэх:

```bash
mkdir -p tram/{input,output,sol,check,statement}
cd tram
```

### 2. task.yaml бичих:

```yaml
name: tram
title: Tram
time_limit: 1.0
memory_limit: 256
task_type: Batch
submission_format: [tram.%l]

score_type: Sum
score_parameters: 100

infile: ""
outfile: ""
```

### 3. Tests үүсгэх:

```bash
# Sample
echo "3 7
E
E
E
L 2
E
L 1
E" > input/input0.txt

echo "1 1
3 2
1 2
3 1
1 1" > output/output0.txt
```

### 4. Solution бичих:

```bash
vim sol/solution.cpp
# (Бодлого бичих)

# Test
g++ -o sol/solution sol/solution.cpp
./sol/solution < input/input0.txt
```

### 5. Import:

```bash
cmsImportTask .
```

### 6. Submit & Test:

```bash
# Web дээр submit эсвэл:
cmsSubmitFiles -c mycontest -u test -t tram sol/solution.cpp
```

---

**Амжилт хүсье!** 🚀 CMS нь эхэндээ төвөгтэй санагдана, гэхдээ олон удаа ашигласны дараа маш хялбар болно!
