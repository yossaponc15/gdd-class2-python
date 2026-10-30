# 📘 Student Guide — 17 ต.ค. 2026: Function × List × Dict

> เปิดดูระหว่างทำ notebook ได้เลย · แต่ละหัวข้อบอกว่าใช้กับ **Level ไหน**

---

## 1. `return` คืออะไร → ทุก Level

```python
def fuel(distance):
    return (distance + 9) // 10     # ส่งค่ากลับ

f = fuel(240)        # f = 24 เอาไปใช้ต่อได้
```

| | `print` | `return` |
|---|---|---|
| ทำอะไร | แสดงบนจอ | ส่งค่ากลับให้คนเรียก |
| ค่าที่ได้ | `None` | ค่าที่ return |

- โจทย์เขียนว่า **"คืนค่า"** → ใช้ `return`
- เจอ `return` ฟังก์ชัน **จบทันที** บรรทัดหลังจากนั้นไม่ทำงาน

---

## 2. เรียกฟังก์ชันเดิม อย่าเขียนสูตรซ้ำ → ทุก Level

```python
def create_package(name, weight, package_type):
    return {"name": normalize_text(name), ...}     # ใช้ของ Level 1
```

> 💡 โจทย์บอก "ต้องเรียก ...()" = ห้ามเขียน logic นั้นใหม่เอง

---

## 3. ฟังก์ชันที่คืน True / False → Level 10

```python
def is_ok(x):
    cond1 = ...                 # boolean ย่อยตัวที่ 1
    cond2 = ...                 # boolean ย่อยตัวที่ 2
    return cond1 and cond2      # จริงทุกข้อถึงจะ True
```

| อยากตรวจว่า | เขียน |
|---|---|
| ไม่เกิน 20 | `x <= 20` |
| ไม่ใช่ของในรายการ | `t not in ["Weapon", "Explosive"]` |
| รวมแล้วไม่เกินความจุ | `ตอนนี้ + ของใหม่ <= ความจุ` |

> ⚠️ ไม่ต้องเขียน `if ok: return True else: return False` → `return ok` ได้เลย

---

## 4. list ของ dict + ตัวสะสม → Level 9, 12, 15

```python
total = 0                          # ① ก่อน loop
for package in mission["packages"]:
    total += package["weight"]     # ② ในแต่ละรอบ package คือ dict 1 ตัว
return total                       # ③ หลัง loop (ย่อหน้าเท่ากับ for)
```

เรียกฟังก์ชันใน loop (Level 12):
```python
total += calculate_package_cost(package)
```

> ⚠️ `return` ใน loop → ได้แค่ชิ้นแรก

---

## 5. เพิ่มของลง list → Level 11

```python
if is_package_allowed(mission, package):
    package["status"] = "พร้อมส่ง"
    mission["packages"].append(package)
    return True
return False
```

---

## 6. ค้นหา: เจอคืน dict · ไม่เจอคืน None → Level 13

```python
target = normalize_text(name)      # จัดคำค้นครั้งเดียว
for p in mission["packages"]:
    if p["name"] == target:
        return p                   # เจอ → จบเลย
return None                        # นอก loop!
```

---

## 7. ใช้ผลค้นหาแล้วแก้ไข → Level 14

```python
found = find_package(mission, name)
if found is None:
    return False
found["status"] = new_status       # แก้ตรงนี้ ใน list ก็เปลี่ยนด้วย
return True
```

---

## 8. ฟังก์ชันคืน dict สรุปผล → Level 15

```python
return {
    "count": len(mission["packages"]),
    "weight": total_cargo_weight(mission),     # เรียกฟังก์ชันเดิม
    "cost": total_shipping_cost(mission),
}
```

---

## 🆘 Error ที่เจอบ่อย

| Error | สาเหตุ |
|---|---|
| `NameError: name 'xxx' is not defined` | ยังไม่ได้รัน cell ของ Level ก่อนหน้า → รันตั้งแต่บน หรือใช้ Recovery Cell |
| `KeyError: 'weight'` | สะกด key ผิด หรือ dict ไม่มี key นั้น |
| `TypeError: 'NoneType' object is not subscriptable` | ฟังก์ชันลืม `return` หรือค้นหาไม่เจอแล้วเอา `None` ไปใช้ `[...]` |
| `TypeError: unsupported operand ... NoneType` | ฟังก์ชันที่เรียกไม่ได้ `return` ค่า |
