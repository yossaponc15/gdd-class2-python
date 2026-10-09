# 🐉 I08: TOI-Zero A2 — Arrays + Functions + Boss Stage

> **หลักสูตร 2: Python Intermediate · ครั้งที่ 8 / 8 (ครั้งสุดท้าย!)**
> หัวข้อ: การใช้ arrays (list) และ functions แก้โจทย์, การจัดการข้อมูลซับซ้อน, สอบท้ายคอร์ส

🏟️ Galactic Delivery จัด **การแข่งขันโปรแกรมเมอร์ประจำกาแล็กซี!** โจทย์ระดับ A2 ต้องใช้ทั้ง **list**, **dict** และ **function** ร่วมกัน และต้อง **เร็วพอ** กับข้อมูลขนาดใหญ่

---

## 🎯 เป้าหมายวันนี้
- [ ] รับข้อมูลเป็น list / list 2 มิติ ได้คล่อง
- [ ] ใช้เทคนิค: นับด้วย list/dict, prefix sum, sliding window, เรียงหลายเงื่อนไข
- [ ] แยกโจทย์เป็น function ย่อย ทดสอบทีละส่วน
- [ ] เข้าใจว่าทำไมบางวิธี "ช้าเกินไป"
- [ ] ผ่านสอบท้ายคอร์ส 🐉

---

## 1. Template รับข้อมูลสำหรับ A2 📥

```python
n = int(input())
arr = list(map(int, input().split()))            # N ตัวในบรรทัดเดียว

n, m = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]   # ตาราง n แถว

words = [input() for _ in range(n)]              # n บรรทัด บรรทัดละคำ
```

> 💡 `_` คือชื่อตัวแปรที่บอกว่า "ไม่ได้ใช้ค่านี้"

## 2. แยกเป็น function 🧩

```python
def read_input():
    n = int(input())
    return list(map(int, input().split()))

def solve(arr):
    return max(arr) - min(arr)

arr = read_input()
print(solve(arr))
```

ข้อดี: ทดสอบ `solve` ด้วยข้อมูลเองได้โดยไม่ต้องพิมพ์ input ทุกครั้ง
```python
assert solve([3, 9, 1]) == 8     # ถ้าผิด จะ AssertionError ทันที
```

## 3. ความเร็วสำคัญ! ⏱️

Grader มีเวลาจำกัด (ประมาณ 1 วินาที) Python ทำได้ราว **10 ล้านคำสั่งต่อวินาที**

| N | วน 1 ชั้น (N) | วน 2 ชั้น (N²) |
|---:|---:|---:|
| 1,000 | 1 พัน ✅ | 1 ล้าน ✅ |
| 100,000 | 1 แสน ✅ | 1 หมื่นล้าน ❌ ช้าเกิน! |

> 💡 อ่าน **ขอบเขต N** ในโจทย์ก่อนเลือกวิธี! (Big O จะเรียนจริงจังในหลักสูตร Advanced)

## 4. เทคนิคยอดนิยม 🛠️

### 4.1 นับความถี่ (dict หรือ list)
```python
count = {}
for x in arr:
    count[x] = count.get(x, 0) + 1

# ถ้าค่าอยู่ในช่วงเล็ก ๆ เช่น 0–100 ใช้ list เร็วกว่า
count = [0] * 101
for x in arr:
    count[x] += 1
```

### 4.2 Prefix Sum — ถามผลรวมช่วงได้ทันที
```python
arr = [3, 1, 4, 1, 5]
pre = [0]
for x in arr:
    pre.append(pre[-1] + x)      # pre = [0, 3, 4, 8, 9, 14]

# ผลรวมตำแหน่ง l ถึง r (นับจาก 1) = pre[r] - pre[l-1]
print(pre[4] - pre[1])           # 1+4+1 = 6
```
ถาม Q ครั้ง: แบบวนบวกใหม่ทุกครั้ง = N×Q ❌ · prefix sum = N+Q ✅

### 4.3 Sliding Window — ช่วงยาว K ที่ดีที่สุด
```python
k = 3
window = sum(arr[:k])
best = window
for i in range(k, len(arr)):
    window += arr[i] - arr[i - k]    # เลื่อนหน้าต่าง: เพิ่มตัวใหม่ ลบตัวเก่า
    best = max(best, window)
```

### 4.4 เรียงหลายเงื่อนไข
```python
# คะแนนมาก→น้อย ถ้าเท่ากัน ชื่อ A→Z
players.sort(key=lambda p: (-p[1], p[0]))
```

### 4.5 ตาราง 2 มิติ + เพื่อนบ้าน
```python
dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]     # บน ล่าง ซ้าย ขวา
for dr, dc in dirs:
    nr, nc = r + dr, c + dc
    if 0 <= nr < n and 0 <= nc < m:          # เช็กไม่ออกนอกตาราง!
        ...
```

## 5. Checklist ก่อนส่ง ✅
- [ ] input ไม่มีข้อความ · ไม่มี print debug
- [ ] output ตรงรูปแบบ
- [ ] ทดสอบค่าขอบ: N = 1, ค่าเท่ากันหมด, ค่าติดลบ, ค่าสูงสุด
- [ ] ดูขอบเขต N → วิธีเร็วพอไหม?

---

## 🎓 สรุปหลักสูตร Python Intermediate

| ครั้ง | เรียนอะไร |
|:-:|---|
| I01 | List + comprehension |
| I02 | Tuple + Set |
| I03 | Dictionary |
| I04–05 | Functions: parameters, return, scope, lambda |
| I06 | Exception + File I/O + CSV |
| I07 | Modules, Packages, pip, venv |
| I08 | แก้โจทย์ A2 |

➡️ **หลักสูตรต่อไป: Python Advanced** 🤖 — OOP, Decorators, Algorithms และอีกมากมาย!
