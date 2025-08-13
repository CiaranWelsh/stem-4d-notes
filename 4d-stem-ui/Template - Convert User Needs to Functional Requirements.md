> **Purpose** – A _minimum routine that converts **one user need** into a fully specified, test‑ready functional requirement (FR) set in ≤ 60 minutes.  
> Copy this note for every need (or run multiple needs in one note—your call).

---

### 1  Capture Context  _(≤ 5 min)_

-  List the **Need ID & summary**.
    
-  Paste any **sketch / mock‑up / domain diagram** or link to it.
    
-  Record known **external interfaces / systems** touched by this need.
    

---

### 2  Run the “4Q Drill‑down”  _(≤ 10 min)_

Write one short bullet per question:

1. **What?** – capability
    
2. **When/where?** – trigger & scope
    
3. **How well?** – measurable performance / quality
    
4. **How verified?** – test or inspection method
    

---

### 3  Draft Atomic “Shall” Statements  _(≤ 15 min)_

-  Break the answers above into **1‑n FRs**, each **one sentence**.
    
-  Each FR contains a **trigger/condition** _and_ a **measurable criterion**.
    
-  If a negative behaviour matters (“shall not …”), capture it too.
    

---

### 4  Attach Attributes & Test Stub  _(≤ 15 min)_

For _each_ FR add the **inline table** below:

|Field|Value|
|---|---|
|**Need**|N?|
|**Priority**|Must / Should / Could / Won’t|
|**Owner**|🔗 team / person|
|**Assumptions**|HW, lib, env…|
|**Verification**|Test ID + method|
|**Risk**|Low / Med / High|

Then create a **skeleton test** (Gherkin, pytest, etc.) with:

```text
# Requirement: FR‑?.?
# Expectation: <measurable pass/fail>
```

---

### 5  Trace & Review  _(≤ 15 min)_

-  **Link** FR IDs ↔ Need ID in your RTM table or back‑link.
    
-  **Peer walk‑through** (UX + dev + test) – 10 min time‑box.
    
-  Mark checklist items DONE ✅, file follow‑ups as tasks.
    

---

## 📄 Template Block (copy‑paste)

````markdown
### Need N? – <one‑line summary>
![[optional‑sketch.png]]

#### 4Q Drill‑down
- **What?**  
- **When/where?**  
- **How well?**  
- **How verified?**  

#### Functional Requirements
1. **FR‑?.1** The system shall …
2. **FR‑?.2** The system shall …

| Need | Priority | Owner | Assumptions | Verification | Risk |
|------|----------|-------|-------------|--------------|------|
| N? | Must | <name> | <list> | V‑?.1 (automated test) | Med |

```python
# tests/test_fr_?_?.py
def test_fr_xy():
    """Requirement: FR‑?.? – expectation…"""
    assert ...
````

---

## 🛠️ Worked Example (condensed)

### Need N3 – Real‑time visual feedback

![[ui‑preview‑sketch.png]]

**4Q**

- **What?** Live preview of scanned region
    
- **When/where?** Starts when _Arm Scan_ pressed; stops on pause/end
    
- **How well?** Latency ≤ 50 ms; ≥ 90 % frames at 512×512 / 1 µs dwell
    
- **How verified?** Loopback latency test @ 1 µs dwell
    

**FRs**

1. **FR‑3.1** The system shall stream a preview that updates ≤ 50 ms after each detector timestamp when dwell ≥ 1 µs.
    
2. **FR‑3.2** The preview shall maintain ≥ 90 % frame coverage for a 512×512 probe grid at 1 µs dwell.
    
3. **FR‑3.3** The preview shall start automatically on _Arm Scan_ and stop on scan end, pause, or abort.
    

|Need|Priority|Owner|Assumptions|Verification|Risk|
|---|---|---|---|---|---|
|N3|Must|UX Lead|GPU driver supports DMA|V‑3.1 latency CI test|High|

```python
@pytest.mark.parametrize("dwell_ns", [1000])
def test_preview_latency(device, loopback):
    """
    Requirement: FR‑3.1  
    Expectation: latency ≤ 50 ms
    """
    latency = loopback.measure_preview_latency(dwell_ns)
    assert latency <= 0.050
```

---

### 📌 Usage Tips

- Keep each checklist run < 1 hour; split big needs.
    
- Store every note under `SRS/Needs/` to exploit Obsidian backlinks.
    
- When the FR is implemented, link the **commit hash** or **Jira ticket** here.
    

---

_End of note_