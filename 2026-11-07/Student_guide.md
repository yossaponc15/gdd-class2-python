# 📘 Student Guide — 7 พ.ย. 2026: 5 ท่าสำหรับ Level 13–18

| ท่า | ใช้ที่ | หน้าตา |
|---|:-:|---|
| 🔎 ค้นหา | 13 | วน → เจอ `return item` → นอก loop `return None` |
| ✏️ ค้นแล้วแก้ | 14 | `found = find(...)` → `if found is None: return False` → แก้ → `return True` |
| 🧩 ประสานงาน | 15 | `return {"k": ฟังก์ชันเดิม(...)}` |
| 🧹 คัดกรอง | 16 | `result = []` → `append` ตัวที่ผ่าน → `return result` หลัง loop |
| ⚖️ ตรวจความพร้อม | 17, 18 | `return ของที่มี >= ของที่ต้องใช้(...)` |

---

## 🔎 ค้นหา → Level 13
```python
target = normalize_text(name)          # จัดคำค้นก่อน loop
for item in items:
    if item["name"] == target:
        return item
return None                            # นอก loop!
```
> 🐞 `else: return None` ใน loop = ดูแค่ชิ้นแรก

## ✏️ ค้นแล้วแก้ → Level 14
```python
found = find_package(mission, name)
if found is None:
    return False
found["status"] = new_status           # แก้ตรงนี้ ใน list ก็เปลี่ยน
return True
```

## 🧩 ประสานงาน → Level 15
```python
return {
    "count": len(...),
    "weight": total_cargo_weight(mission),
    "cost": total_shipping_cost(mission),
}
```

## 🧹 คัดกรอง → Level 16
```python
result = []
for item in items:
    if item.get("destination") == target:    # .get() กัน KeyError
        result.append(item)
return result
```
| ค้นหา | คัดกรอง |
|---|---|
| ได้ **1 ตัว** (หรือ None) | ได้ **list** (อาจว่าง `[]`) |
| `return` ใน loop ได้ | `return` **หลัง** loop เท่านั้น |

## ⚖️ ตรวจความพร้อม → Level 17–18
```python
return mission["ship"]["fuel"] >= calculate_required_fuel(mission)
```
- เท่ากันถือว่าพอ → `>=`
- ไม่ต้อง `if ... return True else return False`

## 🆘 Error ที่เจอบ่อย
| Error | สาเหตุ |
|---|---|
| `NameError` | ยังไม่ได้รัน cell 🔧 หรือ Level ก่อนหน้า |
| `TypeError: 'NoneType' object is not subscriptable` | ค้นไม่เจอแล้วเอา `None` ไปใช้ `[...]` |
| `KeyError: 'destination'` | ใช้ `[...]` กับ dict ที่ไม่มี key → ใช้ `.get()` |
