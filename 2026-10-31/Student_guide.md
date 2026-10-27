# 📘 Student Guide — 31 ต.ค. 2026: Level 13–18

> เปิดดูระหว่างทำ notebook ได้เลย · แต่ละหัวข้อบอกว่าใช้กับ **Level ไหน**

---

## 🔁 ก่อนเริ่ม
เปิด notebook หลัก → **Run All** (หรือรันทีละ cell จาก Level 1) ถ้าเจอ `NameError` แปลว่ายังไม่ได้รัน cell ก่อนหน้า

---

## 1. ค้นหา: เจอคืน dict · ไม่เจอคืน `None` → Level 13
```python
target = normalize_text(package_name)      # ① จัดคำค้นครั้งเดียว
for package in mission["packages"]:
    if package["name"] == target:
        return package                     # ② เจอ → คืนทั้ง dict
return None                                # ③ นอก loop!
```
| พลาดบ่อย | อาการ |
|---|---|
| `else: return None` อยู่ใน loop | ค้นได้แค่ชิ้นแรก |
| ลืม `normalize_text` คำค้น | ค้น `"  MEDICINE "` ไม่เจอ |
| คืน `package["name"]` | โจทย์ให้คืน **dict ทั้งก้อน** |

---

## 2. ใช้ผลค้นหาแล้วแก้ค่า → Level 14
```python
found = find_package(mission, package_name)
if found is None:
    return False
found["status"] = new_status
return True
```
> dict ที่ค้นเจอ **คือตัวเดียวกับใน list** แก้ที่ `found` = แก้ใน mission ด้วย

---

## 3. คืน dict สรุป → Level 15 (Checkpoint 3)
```python
return {
    "count": len(mission["packages"]),
    "weight": total_cargo_weight(mission),
    "cost": total_shipping_cost(mission),
}
```
> ห้ามเขียน loop รวมใหม่ ใช้ฟังก์ชันที่มีอยู่แล้ว

---

## 4. คัดกรอง list → Level 16
```python
target = normalize_text(destination)
result = []
for package in mission["packages"]:
    if package.get("destination") == target:
        result.append(package)
return result
```
- ใช้ `.get("destination")` เพราะพัสดุบางชิ้น **ไม่มี** key นี้ → ได้ `None` แทน `KeyError`
- คืน **list ใหม่** (ไม่ใช่ print)

---

## 5. ฟังก์ชันตรวจความพร้อม → Level 17–18
รูปแบบเดียวกันทั้งคู่:
```python
return ของที่มี >= ฟังก์ชันที่คำนวณของที่ต้องใช้(mission)
```
| Level | ของที่มี | ของที่ต้องใช้ |
|:-:|---|---|
| 17 | `mission["ship"]["fuel"]` | `calculate_required_fuel(mission)` |
| 18 | `mission["budget"]` | `total_shipping_cost(mission)` |

- "เท่ากันถือว่าพอ" → `>=`
- ไม่ต้อง `if ... return True else return False`

---

## 🗂️ แผนที่ข้อมูล mission
```text
mission
├── "pilot"    → {"name", "callsign"}
├── "ship"     → {"name", "fuel", "capacity"}
├── "planet"   → {"name", "distance", "is_open"}
├── "budget"   → ตัวเลข
├── "packages" → [ {"name", "weight", "type", "status", ("destination")}, ... ]
└── "launched" → True / False
```
> ไม่แน่ใจว่ามี key อะไร → `print(mission)` ดูก่อน

## 🆘 Error ที่เจอบ่อย
| Error | สาเหตุ |
|---|---|
| `NameError` | ยังไม่ได้รัน cell ก่อนหน้า → Run All |
| `KeyError: 'destination'` | ใช้ `p["destination"]` กับพัสดุที่ไม่มี key → ใช้ `p.get("destination")` |
| `TypeError: 'NoneType' object is not subscriptable` | ค้นไม่เจอแล้วเอา `None` ไปใช้ `[...]` หรือฟังก์ชันลืม `return` |
