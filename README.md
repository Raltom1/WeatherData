# Weather Data Analysis — README (CSE413)

## STEP 1: Gumawa ng weather_data.csv

1. Buksan mo ang **Jupyter** (o kahit anong file manager sa loob ng Jupyter).
2. Click **File → New → Text File**
3. I-paste mo itong data sa loob:

```
Date,Temperature
2026-01-01,28
2026-01-02,29
2026-01-03,30
2026-01-04,31
2026-01-05,30
2026-01-06,29
2026-01-07,32
2026-01-08,33
2026-01-09,31
2026-01-10,34
2026-01-11,35
2026-01-12,34
2026-01-13,36
2026-01-14,35
```

4. I-save mo (Ctrl+S o File → Save).
5. I-rename mo yung file (right-click → Rename, o click yung filename sa taas) at palitan ang extension mula `.txt` papuntang **`weather_data.csv`**
   - Halimbawa: `untitled.txt` → `weather_data.csv`
6. Siguraduhin na nasa **parehong folder** ito ng notebook mo (yung `.ipynb` file).

---

## STEP 2: Gumawa ng bagong Notebook

1. Click **File → New → Notebook**
2. Piliin ang Python kernel (Python 3 o conda:base)

---

## STEP 3: I-type isa-isa ang code sa bawat cell

Bawat code block sa baba = **isang cell**. I-type mo sa notebook, tapos pindutin **Shift + Enter** para i-run bago pumunta sa susunod na cell.

### Cell 1 — Load Weather Data
```python
#Load Weather Data#
import pandas as pd
data = pd.read_csv("weather_data.csv")
print("\nWEATHER DATA")
print(data)
```
> Ito ang naglo-load ng CSV papunta sa variable na `data`. LAHAT ng susunod na cells ay umaasa dito — kaya siguraduhin na successful ito bago tumuloy.

---

### Cell 2 — Compute Mean Temperature
```python
#Compute Mean Temperature #
mean_temp = data["Temperature"].mean()
print("\nMean Temperature:")
print(f"{mean_temp:.2f} °C")
```

---

### Cell 3 — Summary Statistics
```python
#Analyze Weather Data#
print("\nSummary Statistics:")
print(data["Temperature"].describe())
```

---

### Cell 4 — Hottest and Coldest Day
```python
hottest = data.loc[data["Temperature"].idxmax()]
coldest = data.loc[data["Temperature"].idxmin()]

print("\nHottest day: ")
print(f"date: {hottest['Date']}")
print(f"Temperature: {hottest['Temperature']} °C")

print("\nColdest Day:")
print(f"Date: {coldest['Date']}")
print(f"Temperature: {coldest['Temperature']} °C")
```

---

### Cell 5 — Simple Linear Regression
```python
#Simple Linear Regression#
# Convert dates into numerical values#
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
import numpy as np
data["Day_Number"] = np.arange(len(data))

X = data[["Day_Number"]]
y = data["Temperature"]

model = LinearRegression()
model.fit(X, y)
```

---

### Cell 6 — Predict Next 7 Days
```python
# STEP 6: PREDICT NEXT 7 DAYS
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
import numpy as np

future_days = np.arange(len(data), len(data)+7).reshape(-1,1)

predictions = model.predict(future_days)

print("\nPredicted Temperatures for Next 7 Days")

for i, temp in enumerate(predictions, start=1):
    print(f"Day +{i}: {temp:.2f} °C")
```

---

### Cell 7 — Temperature Trend Graph
```python
#Create Temperature Trend Graph#
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
plt.figure(figsize=(10,5))
plt.plot(data["Date"], data["Temperature"], marker='o')
plt.title("Temperature Trend")
plt.xlabel("Date")
plt.ylabel("Temperature (°C)")
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
```

---

## TROUBLESHOOTING

**Problema:** `NameError: name 'data' is not defined`
**Dahilan:** Hindi mo pa na-run yung Cell 1 (Load Weather Data) sa kasalukuyang session.
**Solusyon:** Click **Kernel → Restart Kernel and Run All Cells...**  ito ay auto magru-run ng lahat ng cells mula taas hanggang baba, sunod-sunod.

**Problema:** `UserWarning: X does not have valid feature names...` (sa Cell 6)
**Dahilan:** Warning lang ito, hindi error — gumana pa rin nang tama ang code.
**Solusyon:** Pwede mong balewalain.

**Problema:** `FileNotFoundError: weather_data.csv`
**Dahilan:** Hindi magkasabay ang lokasyon ng notebook at ng csv file.
**Solusyon:** Siguraduhin parehong folder, o i-upload ulit ang csv gamit ang upload icon sa file browser.
