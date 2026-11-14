# 📘 Student Guide — 14 พ.ย. 2026: ด่าน 4 (Level 16–20)

| ท่า | ใช้ที่ | หน้าตา |
|---|:-:|---|
| 🧹 คัดกรอง | 16 | `result = []` → `append` ตัวที่ผ่าน → `return result` หลัง loop |
| ⚖️ ตรวจความพร้อม | 17, 18 | `return ของที่มี >= ของที่ต้องใช้(...)` |
| 🔗 หลายเงื่อนไข | 19 | boolean ย่อย 2 ตัว → `return a and b` |
| 📋 รวบรวมเหตุผล | 20 | `reasons = []` → `if not ...: append` → `return {"ready": ..., "reasons": ...}` |

---

## 🧹 คัดกรอง → Level 16
```python
target = normalize_text(destination)
result = []
for package in mission["packages"]:
    if package.get("destination") == target:    # .get() กัน KeyError
        result.append(package)
return result
```

## ⚖️ ตรวจความพร้อม → Level 17–18
| Level | ของที่มี | ของที่ต้องใช้ |
|:-:|---|---|
| 17 | `mission["ship"]["fuel"]` | `calculate_required_fuel(mission)` |
| 18 | `mission["budget"]` | `total_shipping_cost(mission)` |

เท่ากันถือว่าพอ → `>=` · ไม่ต้อง `if ... return True else return False`

## 🔗 หลายเงื่อนไข → Level 19
```python
has_packages = len(mission["packages"]) > 0
within_capacity = total_cargo_weight(mission) <= mission["ship"]["capacity"]
return has_packages and within_capacity
```

## 📋 รวบรวมเหตุผล → Level 20
```python
reasons = []
if not mission["planet"]["is_open"]:
    reasons.append("สถานีปลายทางปิด")
if not fuel_is_ready(mission):
    reasons.append("เชื้อเพลิงไม่เพียงพอ")
# ... อีก 2 ข้อ
return {"ready": len(reasons) == 0, "reasons": reasons}
```
- ใช้ `if` แยกทุกข้อ **ไม่ใช้ `elif`** (ต้องได้เหตุผลครบทุกข้อ)
- ข้อความต้องตรงกับโจทย์ทุกตัวอักษร: `สถานีปลายทางปิด` · `เชื้อเพลิงไม่เพียงพอ` · `งบประมาณไม่เพียงพอ` · `ห้องเก็บของยังไม่พร้อม`

## ⭐ แสดงรายการ → Level 21
```python
if len(mission["packages"]) == 0:
    print("ยังไม่มีพัสดุ")
    return                              # return เปล่า = จบฟังก์ชัน
number = 1
for package in mission["packages"]:
    print(f"{number}. {package['name']} | ...")    # ใน f"..." ใช้ ['...']
    number += 1
```
- ฟังก์ชันแสดงผลใช้ `print` ไม่ต้อง return ค่า

## ⭐ แสดงผลจาก dict → Level 22
```python
summary = cargo_summary(mission)        # เรียกครั้งเดียว เก็บไว้
result = check_mission(mission)
print(f"พัสดุ: {summary['count']} กล่อง")
if result["ready"]:
    print("สถานะ: พร้อมออกเดินทาง")
else:
    for reason in result["reasons"]:
        print(f"- {reason}")
```

## 🆘 Error ที่เจอบ่อย
| Error / อาการ | สาเหตุ |
|---|---|
| `NameError` | ยังไม่ได้รัน cell 🔧 หรือ Level ก่อนหน้า |
| `KeyError: 'destination'` | ใช้ `[...]` กับพัสดุที่ไม่มี key → ใช้ `.get()` |
| Level 20 ได้เหตุผลแค่ข้อเดียว | ใช้ `elif` แทน `if` |
| Level 20 ได้ `None` | ลืม `return` dict |
| `SyntaxError` ใน f-string | ใช้ `"` ซ้อนใน `f"..."` → เปลี่ยนเป็น `['name']` |
| Level 21 แสดงลำดับเป็น 1 ทุกบรรทัด | ตั้ง `number = 1` ไว้ **ใน** loop |
