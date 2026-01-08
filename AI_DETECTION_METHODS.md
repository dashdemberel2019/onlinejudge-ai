# AI Detection: Онлайн системд AI илрүүлэх аргууд

## 🔍 Хураангуй

**Хариулт:** ✓ Тийм, аргууд байна, **ГЭХДЭЭ** 100% найдвартай биш.

```
AI Detection difficulty:
  Easy to detect:    ★☆☆☆☆ (Copy-paste from ChatGPT)
  Moderate:          ★★★☆☆ (With modifications)
  Hard:              ★★★★☆ (Experienced user)
  Nearly impossible: ★★★★★ (Expert + AI collaboration)
```

---

## 📊 Detection аргууд

### 1. **Code Pattern Analysis** (Кодын pattern шинжилгээ)

#### AI-ийн шинж тэмдэгүүд:

```cpp
// ChatGPT/Claude typical patterns:

// 1. Over-commented код
// This function calculates the maximum value
// Parameters: arr - input array, n - size
// Returns: maximum value in the array
int findMax(vector<int>& arr, int n) {
    // Initialize max with first element
    int maxVal = arr[0];
    
    // Iterate through remaining elements
    for (int i = 1; i < n; i++) {
        // Update max if current element is larger
        if (arr[i] > maxVal) {
            maxVal = arr[i];
        }
    }
    
    // Return the maximum value
    return maxVal;
}

// ❌ SUSPICIOUS: Хэт олон comment
// ✓ Human код: Бага comment эсвэл comment үгүй
```

```cpp
// 2. Verbose variable names
// AI typical:
int numberOfElementsInArray = n;
double averageValueOfAllElements = sum / count;
bool isValidInputForCalculation = true;

// Human typical:
int n;
double avg = sum / cnt;
bool valid = true;

// ❌ SUSPICIOUS: Хэт урт, тодорхой нэр
```

```cpp
// 3. Overly structured код
// AI typical:
class Solution {
public:
    int solve(vector<int>& nums) {
        // Validate input
        if (nums.empty()) {
            return 0;
        }
        
        // Initialize variables
        int result = 0;
        int n = nums.size();
        
        // Main algorithm
        for (int i = 0; i < n; ++i) {
            result += nums[i];
        }
        
        // Return result
        return result;
    }
};

// Human typical (competitive programming):
int main() {
    int n; cin >> n;
    int sum = 0;
    for (int i = 0; i < n; i++) {
        int x; cin >> x;
        sum += x;
    }
    cout << sum;
}

// ❌ SUSPICIOUS: Хэт бүтэцтэй, class ашигласан
```

```cpp
// 4. Consistent formatting
// AI: Always perfect indentation, spacing
if (condition) {
    doSomething();
} else {
    doOtherThing();
}

// Human: Sometimes messy
if(condition){doSomething();}
else{doOtherThing();}

// ❌ SUSPICIOUS: Хэт цэвэрхэн
```

```cpp
// 5. American English spelling in comments
// AI (trained on English):
// This function calculates the color value
int getColor() { ... }

// Mongolian/Russian programmer:
// Функц өнгийн утгыг тооцоолно
int getColor() { ... }

// ❌ SUSPICIOUS: Англи хэл дээрх professional comments
```

---

### 2. **Code Similarity Detection** (Plagiarism)

#### Moss (Measure of Software Similarity):

```bash
# Stanford-ийн систем
# Олон submission-г харьцуулна

Example:
→ 50 submissions
→ 2 код 95% ижил
→ 🚨 ALERT: Possible plagiarism

Tools:
→ Moss: https://theory.stanford.edu/~aiken/moss/
→ JPlag: https://jplag.ipd.kit.edu/
→ Sherlock: Sheffield detector
```

#### Pattern:

```
AI код-ын тусгай шинж:
→ Identical variable names across users
→ Same comment style
→ Same algorithm structure
→ Same edge case handling

Example:
User A: int numberOfElements = n;
User B: int numberOfElements = n;
User C: int numberOfElements = n;

🚨 ALERT: Бүгд ChatGPT-ээс авсан байж болно!
```

---

### 3. **Submission Timing Analysis**

#### Pattern 1: Too fast submission

```
Problem released: 00:00
User submission:  00:03 (3 минутын дараа)

Problem complexity: Codeforces Div2D (Hard)
Expected time: 30-60+ минут

Analysis:
→ 3 минут = Read (1 мин) + Think (0) + Code (2 мин)
→ Impossible to think + implement in 2 минут
→ 🚨 SUSPICIOUS: Likely AI usage

Exception:
→ Very easy problems
→ Expert users (but still suspicious)
```

#### Pattern 2: Consistent timing

```
User history:
Problem 1: 3 минут
Problem 2: 4 минут  
Problem 3: 3 минут
Problem 4: 3 минут
Problem 5: 25 минут ← SUDDENLY SLOW

Analysis:
→ First 4: AI-generated instantly
→ Problem 5: AI failed, user had to think
→ 🚨 SUSPICIOUS: Timing inconsistency
```

#### Pattern 3: Multiple attempts pattern

```
Normal user:
Submit 1: Wrong Answer (small bug)
Submit 2: Wrong Answer (logic error)
Submit 3: Accepted (fixed)

AI user:
Submit 1: Accepted (perfect first try)
OR
Submit 1: Compilation Error (copy-paste issue)
Submit 2: Accepted (fixed syntax)

🚨 SUSPICIOUS: Too perfect or specific error patterns
```

---

### 4. **Behavioral Analysis** (Хэрэглэгчийн зан)

#### Keyboard/Mouse tracking:

```
Contest with proctoring:
→ Keystroke dynamics (typing rhythm)
→ Mouse movement patterns
→ Copy-paste events detection
→ Tab switching detection

AI user patterns:
→ Long pause (asking AI)
→ Sudden burst of typing (paste code)
→ No gradual code buildup
→ Minimal corrections

Normal user patterns:
→ Gradual typing
→ Many small corrections
→ Trial-and-error visible
→ Consistent rhythm
```

#### Example timeline:

```
NORMAL USER:
00:00 - Start typing main()
00:30 - Write input reading
02:00 - Think about algorithm
05:00 - Start implementing
10:00 - Fix bugs gradually
15:00 - Submit

AI USER:
00:00 - Long pause (copy problem)
02:00 - Another pause (wait for AI)
03:00 - BURST typing (paste code)
03:30 - Submit

🚨 SUSPICIOUS: Abnormal pattern
```

---

### 5. **Code Fingerprinting** (Кодын хурууны хээ)

#### GPTZero-style detection:

```python
# AI-generated text/code байдлаар шинжлэх

Metrics:
→ Perplexity: AI код бага perplexity
→ Burstiness: Human илүү variable
→ Predictability: AI илүү predictable

Example:
Human code perplexity: 45
AI code perplexity: 12 ← Too low!

🚨 SUSPICIOUS: AI-generated
```

#### Code structure analysis:

```cpp
// AI typical structure:
1. Input validation
2. Variable initialization  
3. Main algorithm with comments
4. Edge case handling
5. Return statement with comment

// Human typical structure:
1. Quick variable declarations
2. Algorithm (maybe messy)
3. Output
4. (Edge cases forgotten 😅)

AI code: Too "textbook-like"
```

---

### 6. **Language Model Detection**

#### CodeBERT / GPT-Detector:

```python
# Machine Learning model
# Trained on AI vs Human code

Input: Source code
Output: Probability of AI-generated

Example:
Code A: 87% likely AI-generated 🚨
Code B: 15% likely AI-generated ✓

Tools:
→ GPTZero
→ AI Text Classifier (OpenAI)
→ Originality.AI
→ Copyleaks
```

#### Limitations:

```
❌ False positives possible
❌ Can be fooled with modifications
❌ New AI models evade detection
✓ Works on obvious cases
✓ Useful as supporting evidence
```

---

### 7. **Interview/Explanation Test**

#### Post-contest verification:

```
Suspected users:
→ Ask to explain algorithm
→ Ask to solve similar problem live
→ Code review session

Red flags:
❌ Cannot explain own code
❌ Cannot solve simpler variant
❌ Different coding style in live session
❌ Much slower in live session

Example:
Contest: Solved Div2D in 5 минут
Live test: Cannot solve Div2B in 30 минут

🚨 CONFIRMED: AI usage
```

---

## 🛡️ Platform-specific tools

### Codeforces:

```
Current:
→ Moss plagiarism detection
→ Manual review of suspicious submissions
→ Community reports
→ Timing analysis

Future (rumored):
→ Code fingerprinting
→ Submission pattern analysis
→ Browser monitoring (for online rounds)
```

### LeetCode (Hiring assessments):

```
Tools:
✓ Webcam proctoring
✓ Screen recording
✓ Browser lockdown
✓ Keystroke analysis
✓ Mouse tracking
✓ Tab switching detection
✓ Copy-paste detection

Result:
→ Hard to use AI with proctoring
→ But not impossible (second device)
```

### HackerRank:

```
Tools:
✓ Plagiarism detection (Moss-like)
✓ Code similarity analysis
✓ Timing analysis
✓ Proctoring (optional)

Features:
→ Code playback (watch typing)
→ Environment screenshot
→ Webcam snapshot
```

### Codility:

```
Tools:
✓ Advanced proctoring
✓ AI detection algorithms
✓ Code similarity
✓ Browser restrictions

Known to be strict!
```

---

## 📈 Detection accuracy

### By method:

| Method | Accuracy | False Positive |
|--------|----------|----------------|
| Manual code review | 60-70% | Medium |
| Plagiarism detection | 80-90% | Low |
| Timing analysis | 50-60% | High |
| Proctoring | 85-95% | Low |
| Code fingerprinting | 70-80% | Medium |
| ML detection | 75-85% | Medium |
| Explanation test | 95-99% | Very Low |

### Combined approach:

```
Single method: 60-80% reliable
Multiple methods: 90-95% reliable

Example pipeline:
1. Timing analysis: 🚨 Suspicious
2. Code pattern: 🚨 Suspicious  
3. ML detector: 🚨 87% AI
4. Interview: ❌ Cannot explain

Result: 99% confident - AI used
```

---

## 💡 AI ашигласныг илрүүлэх практик шинж:

### Хялбар илрүүлэх (95%+):

```cpp
// 1. Direct ChatGPT output
// Certainly! Here's a solution to your problem:
int solve() {
    // Let me explain the approach:
    ...
}

🚨 OBVIOUS: Introductory text left in code
```

```cpp
// 2. Multiple language artifacts
def solution():  # Python
    pass

// Then switched to C++
int main() { ... }

🚨 OBVIOUS: Copy-paste from different prompts
```

```cpp
// 3. Incomplete edits
// <THOUGHT>This approach uses DP</THOUGHT>
int dp[1000];

🚨 OBVIOUS: AI thinking tags visible
```

### Дунд зэргийн илрүүлэх (70-80%):

```cpp
// 1. Over-engineered for simple problem
// Problem: Print sum of two numbers
class Calculator {
    private:
        int operand1;
        int operand2;
    public:
        Calculator(int a, int b) : operand1(a), operand2(b) {}
        int calculateSum() { return operand1 + operand2; }
};

🚨 SUSPICIOUS: Too complex for simple task
```

```cpp
// 2. Inconsistent style with past submissions
// User's past code: Messy, no comments
int main(){int n;cin>>n;cout<<n*2;}

// Suspicious submission: Clean, commented
/**
 * This program calculates twice the input value
 * Input: integer n
 * Output: 2*n
 */
int main() {
    int inputNumber;
    std::cin >> inputNumber;
    std::cout << inputNumber * 2 << std::endl;
    return 0;
}

🚨 SUSPICIOUS: Style changed dramatically
```

### Хүндрэлтэй илрүүлэх (30-50%):

```cpp
// Experienced AI user:
// 1. AI-ээс авсан
// 2. Comment арилгасан
// 3. Variable нэр өөрчилсөн
// 4. Style өөрчилсөн
// 5. Жижиг алдаа нэмсэн (natural looking)

// Original AI:
int numberOfElements = n;
vector<int> inputArray(numberOfElements);

// Modified:
int n;
vector<int> a(n);

Result: Hard to detect ⚠️
```

---

## 🎯 Практик жишээнүүд

### Case Study 1: Codeforces Round

```
Situation:
→ User rated 1200 (Blue)
→ Solved Div1D in 4 minutes
→ First submission: Accepted
→ Code: Professional, well-commented

Red flags:
🚨 Rating inconsistent (should struggle with Div1D)
🚨 Too fast (experts need 20+ minutes)
🚨 Perfect first try (no debugging)
🚨 Code style different from past

Investigation:
→ Check code pattern: AI-like ✓
→ Check timing: Impossible ✓
→ Check past submissions: Different style ✓
→ Run ML detector: 89% AI ✓

Action:
→ Submission skipped
→ User warned/banned
```

---

### Case Study 2: Online Interview

```
Situation:
→ Candidate solved 5/5 problems
→ Average time: 8 minutes per problem
→ Code: Clean, efficient

Proctoring data:
→ Long pauses before typing
→ Burst typing (paste events detected)
→ Tab switching to browser
→ Copy event detected

Live interview:
→ Cannot explain algorithm
→ Cannot solve simpler problem
→ Much slower coding

Result:
→ 100% confident: AI used
→ Candidate rejected
```

---

## 🔐 Platforms-ын countermeasures

### Current (2024-2025):

```
✓ Plagiarism detection (Moss, etc.)
✓ Manual review
✓ Community reporting
~ Proctoring (some platforms)
~ Code pattern analysis (limited)
~ Timing analysis (limited)

✗ Advanced AI detection (not yet)
✗ Real-time monitoring (rare)
✗ Mandatory live verification
```

### Future (2026+):

```
Likely:
→ Mandatory proctoring
→ AI detection algorithms standard
→ Post-contest verification
→ Live coding rounds only

Possible:
→ Onsite contests emphasized
→ AI-assisted category separate
→ Blockchain verification
```

---

## 💭 AI илрүүлэх боломж байгаа эсэх?

### ✅ Илрүүлж болно:

```
1. Careless AI usage
   → Direct copy-paste
   → Obvious patterns
   → Detection rate: 95%+

2. Moderate AI usage
   → Some modifications
   → Timing suspicious
   → Detection rate: 70-80%

3. With proctoring
   → Screen/webcam/keyboard
   → Hard to hide AI usage
   → Detection rate: 85-95%
```

### ❌ Илрүүлэх хүндрэлтэй:

```
1. Expert + AI collaboration
   → AI generates idea
   → Human implements
   → Detection rate: 30-50%

2. Carefully modified code
   → Style adjusted
   → Comments removed
   → Detection rate: 40-60%

3. Without proctoring
   → Second device usage
   → Careful copy-paste
   → Detection rate: 50-70%
```

### ⚠️ Агаарын алсын хараа (Technical cat-and-mouse):

```
Detection improves → AI evasion improves
Better tools → Better hiding methods
More monitoring → More workarounds

Conclusion: 100% detection impossible
But: Most cases detectable with multiple methods
```

---

## 🎓 Platforms нарт зөвлөмж:

### Essential measures:

```
1. ✓ Implement Moss/plagiarism detection
2. ✓ Manual review of suspicious submissions
3. ✓ Timing analysis alerts
4. ✓ Code pattern analysis
5. ✓ Community reporting system
```

### Recommended:

```
6. ✓ ML-based AI detection
7. ✓ Post-contest verification (random)
8. ✓ Style consistency checking
9. ✓ Proctoring for important contests
10. ✓ Live coding verification
```

### Advanced:

```
11. Keystroke dynamics analysis
12. Code playback system
13. Browser behavior monitoring
14. Multi-factor verification
15. Blockchain proof-of-work
```

---

## 📚 Нөөц

### AI Detection Tools:

```
Code:
→ GPTZero: https://gptzero.me
→ Copyleaks: https://copyleaks.com
→ Originality.AI: https://originality.ai
→ CodeBERT models

Plagiarism:
→ Moss: https://theory.stanford.edu/~aiken/moss/
→ JPlag: https://jplag.ipd.kit.edu/

Proctoring:
→ ProctorU
→ Proctorio
→ HonorLock
→ Respondus
```

### Research:

```
Papers:
→ "Detecting AI-Generated Code"
→ "Keystroke Dynamics Analysis"
→ "Code Authorship Attribution"

Communities:
→ r/competitiveprogramming
→ Codeforces blogs
→ Academic conferences (ICPC, IOI)
```

---

## ✅ Дүгнэлт

### Хариулт: **Илрүүлж болно, гэхдээ...**

```
✓ Careless AI usage: 90%+ илрүүлэх
✓ Moderate AI usage: 70-80% илрүүлэх
~ Careful AI usage: 40-60% илрүүлэх
✗ Expert + AI: 30-50% илрүүлэх

Combined methods:
→ Code pattern ✓
→ Timing analysis ✓
→ Plagiarism check ✓
→ ML detection ✓
→ Proctoring ✓
→ Verification interview ✓

Result: 90-95% detection rate possible
```

### Гол санаа:

```
AI detection = Arms race

Current state:
→ Basic AI usage: Easy to detect
→ Careful AI usage: Hard to detect
→ 100% accuracy: Impossible

Best approach:
→ Multiple detection methods
→ Random verification
→ Emphasis on understanding
→ Onsite contests for important events
```

### Хувийн зөвлөмж:

```
Contest organizers:
✓ Use multiple detection methods
✓ Random post-contest interviews
✓ Community reporting
✓ Proctoring for high-stakes

Participants:
✓ Don't use AI in contests (ethical)
✓ Use AI for learning (appropriate)
✓ Understand vs memorize
✓ Build genuine skills
```

---

**Эцсийн үг:** AI detection технологи хөгжиж байна, гэхдээ 100% найдвартай биш. Хамгийн сайн шийдэл бол залилах биш, үнэнч чанартай contest-д оролцож, AI-г зөвхөн **сургалтын хэрэгсэл** болгон ашиглах юм. 🎯
