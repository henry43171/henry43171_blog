+++
date = '2025-05-06T15:07:00+08:00'
draft = false
title = 'Pandas 練習紀錄'
+++
# Pandas 練習
## 基本語法
基本pandas語法練習，先宣告最基本的資料型態dataframe，這邊有刻意留一個None，方便後面檢查資料用。
```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Class": ["Class A", "Class B", "Class A"],
    "Math": [90, 85, 78],
    "English": [88, 92, 80],
    "Science": [85, 79, None]
}
```

建立 `DataFrame` 物件並印出全部資料，觀察整體格式與資料型別。
```python
df = pd.DataFrame(data)
print("\nAll data:\n", df)

"""
All data:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class A    78       80      NaN
"""
```

可以用不同的語法，來取得不同的資料格式，像是 `df.shape`，可以確認資料表的維度（列數、欄數）。
```python
print("\nData shape:\n", df.shape)

"""
Data shape:
 (3, 5)
"""
```

用 `df.head()` 可顯示前幾筆資料，可指定筆數，預設為5。
```python
print("\nFirst 2 rows:\n", df.head(2))

"""
First 2 rows:
     Name    Class  Math  English  Science
0  Alice  Class A    90       88     85.0
1    Bob  Class B    85       92     79.0
"""
```

用 `df.columns` 可查看欄位名稱（column labels）。
```python
print("\nData columns:\n", df.columns)

"""
Data columns:
 Index(['Name', 'Class', 'Math', 'English', 'Science'], dtype='object')
"""
```

用 `df.describe()` 可顯示資料的統計摘要（僅限數值欄位）。
```python
print("\nData describe:\n", df.describe())

"""
Data describe:
             Math    English    Science
count   3.000000   3.000000   2.000000
mean   84.333333  86.666667  82.000000
std     6.027714   6.110101   4.242641
min    78.000000  80.000000  79.000000
25%    81.500000  84.000000  80.500000
50%    85.000000  88.000000  82.000000
75%    87.500000  90.000000  83.500000
max    90.000000  92.000000  85.000000
"""
```

用 `df.info()` 可查看資料總覽（欄位型別、缺值狀況），因為他會直接輸出，就不另外寫print了。
```python
print("\nData information:")
df.info()

"""
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 5 columns):
 #   Column   Non-Null Count  Dtype  
---  ------   --------------  -----  
 0   Name     3 non-null      object 
 1   Class    3 non-null      object 
 2   Math     3 non-null      int64  
 3   English  3 non-null      int64  
 4   Science  2 non-null      float64
dtypes: float64(1), int64(2), object(2)
memory usage: 252.0+ bytes
"""
```

## 欄位操作
先宣告基本資料。
```python
import pandas as pd

data_expand = {
    "Name": ["Alice", "Bob", "Charlie", "Diana", "Ethan"],
    "Class": ["Class A", "Class B", "Class C", "Class A", "Class B"],
    "Math": [90, 85, 78, 92, 88],
    "English": [88, 92, 80, 85, 90],
    "Science": [85, 79, None, 90, 84]
}

df = pd.DataFrame(data_expand)
print("\n" + "--"*20 + "印出全資料" + "--"*20)
print("\nAll expand data:\n", df)
"""
All expand data:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83      NaN
7   Hannah  Class B    91       86     80.0
8      Ian  Class C    76       81     78.0
9    Julia  Class A    89       87     89.0
"""
```

### 選取欄位和重新命名

可選取多組欄位。
```python
# 選取欄位與重新命名
print("\n選取欄位")
print(df[["Math", "Science"]])
"""
   Math  Science
0    90     85.0
1    85     79.0
2    78      NaN
3    92     90.0
4    88     84.0
"""
```

可將欄位重新命名。
```python
print("\n重新命名")
print(df.rename(columns={"Math": "Mathematics"}))
"""
      Name    Class  Mathematics  English  Science
0    Alice  Class A           90       88     85.0
1      Bob  Class B           85       92     79.0
2  Charlie  Class C           78       80      NaN
3    Diana  Class A           92       85     90.0
4    Ethan  Class B           88       90     84.0
"""
```

### 新增欄位與計算
新增欄位，可計算欄位結果。

#### 基本加總
有時候我們需要根據多個欄位的數值，算出總和或平均，這時就可以「開一條新欄」來放計算結果。

這種寫法是最直接的「欄與欄相加」，如果所有欄位都有值，沒問題；但一旦遇到 NaN（缺失值），結果就會整組報 NaN，像 Charlie 就「一分都拿不到」，因為他缺考了一科。
```python
# 新增欄位與計算欄位
print("\n新增欄位與計算欄位")
# 建立加總欄：
df["Total"] = df["Math"] + df["English"] + df["Science"]
print(df)

"""
新增欄位與計算欄位
      Name    Class  Math  English  Science  Total
0    Alice  Class A    90       88     85.0  263.0
1      Bob  Class B    85       92     79.0  256.0
2  Charlie  Class C    78       80      NaN    NaN
3    Diana  Class A    92       85     90.0  267.0
4    Ethan  Class B    88       90     84.0  262.0
"""
```

#### 寬容加總
這種寫法比較人性化，它會自動忽略 NaN 的欄位，只對有數值的欄位做加總。就像 Charlie 缺考一科，還是能加總剩下的兩科。
```python
print("\n新增欄位與計算欄位_2")
score_columns = ["Math", "English", "Science"]
df["Total_2"] = df[score_columns].sum(axis=1)
print(df)
"""
新增欄位與計算欄位_2
      Name    Class  Math  English  Science  Total  Total_2
0    Alice  Class A    90       88     85.0  263.0    263.0
1      Bob  Class B    85       92     79.0  256.0    256.0
2  Charlie  Class C    78       80      NaN    NaN    158.0
3    Diana  Class A    92       85     90.0  267.0    267.0
4    Ethan  Class B    88       90     84.0  262.0    262.0
"""
```

#### 嚴格加總
加入 min_count 機制。
如果要實現「只要缺一科就不算總分」的資料。min_count 可以設定至少幾個值要存在，才會進行加總；不滿足就直接給你 NaN。
```python
print("\n新增欄位與計算欄位_3")
df["Total_strict"] = df[score_columns].sum(axis=1, min_count=len(score_columns))
print(df)
"""
新增欄位與計算欄位_3
      Name    Class  Math  English  Science  Total  Total_2  Total_strict
0    Alice  Class A    90       88     85.0  263.0    263.0         263.0
1      Bob  Class B    85       92     79.0  256.0    256.0         256.0
2  Charlie  Class C    78       80      NaN    NaN    158.0           NaN
3    Diana  Class A    92       85     90.0  267.0    267.0         267.0
4    Ethan  Class B    88       90     84.0  262.0    262.0         262.0
"""
```

#### 建立平均
透過 `.mean(axis=1)` 可以針對每一列（row）計算多個欄位的平均值。這在需要針對多個特徵做統計時非常常見，不論是處理測試成績、銷售數據，還是感測器資料，都能派上用場。

- 第一種直接指定欄位名稱
- 第二種使用先前定義的欄位列表 score_columns，讓程式碼更有彈性與可維護性。

```python
# 建立平均欄：
print("\n平均欄位")
df["Average"] = df[["Math", "English", "Science"]].mean(axis=1)
df["Average_2"] = df[score_columns].mean(axis=1)
print(df)
"""
平均欄位
      Name    Class  Math  English  Science  Total  Total_2  Total_strict    Average  Average_2
0    Alice  Class A    90       88     85.0  263.0    263.0         263.0  87.666667  87.666667
1      Bob  Class B    85       92     79.0  256.0    256.0         256.0  85.333333  85.333333
2  Charlie  Class C    78       80      NaN    NaN    158.0           NaN  79.000000  79.000000
3    Diana  Class A    92       85     90.0  267.0    267.0         267.0  89.000000  89.000000
4    Ethan  Class B    88       90     84.0  262.0    262.0         262.0  87.333333  87.333333
"""
```

### 刪除欄位
資料整理過程中，常會出現一些「中途產物」——像是用來驗證語法的欄位、測試不同參數的欄位等。當這些欄位不再需要時，就可以使用 `drop()` 來清理。
```python
# 刪除欄位
print("\n刪除欄位")
df = df.drop(columns=["Total_2", "Total_strict", "Average_2"])
print(df)
"""
      Name    Class  Math  English  Science  Total    Average
0    Alice  Class A    90       88     85.0  263.0  87.666667
1      Bob  Class B    85       92     79.0  256.0  85.333333
2  Charlie  Class C    78       80      NaN    NaN  79.000000
3    Diana  Class A    92       85     90.0  267.0  89.000000
4    Ethan  Class B    88       90     84.0  262.0  87.333333
"""
```

### 條件篩選與邏輯運算
當我們想根據特定條件過濾資料時，可以使用布林運算符（例如 &, |, ~）來進行條件組合。
例如下面這段程式碼就是篩選出「Math > 80 且 English > 85」的資料列：
```python
# 條件篩選與邏輯運算
print("\n條件篩選與邏輯運算")
print(df[(df["Math"] > 80) & (df["English"] > 85)])
"""
    Name    Class  Math  English  Science  Total    Average
0  Alice  Class A    90       88     85.0  263.0  87.666667
1    Bob  Class B    85       92     79.0  256.0  85.333333
4  Ethan  Class B    88       90     84.0  262.0  87.333333
"""
```

### 排序與重排
`sort_values()` 可以讓你依照指定欄位進行排序，`ascending=False` 則是設定為降冪排列。
```python
# 排序與重排
print("\n排序與重排 'df.sort_values(by='Math', ascending=False)'")
df = df.sort_values(by="Math", ascending=False)
print(df)
"""
      Name    Class  Math  English  Science  Total    Average
3    Diana  Class A    92       85     90.0  267.0  89.000000
0    Alice  Class A    90       88     85.0  263.0  87.666667
4    Ethan  Class B    88       90     84.0  262.0  87.333333
1      Bob  Class B    85       92     79.0  256.0  85.333333
2  Charlie  Class C    78       80      NaN    NaN  79.000000
"""
```

排序後，原本的索引（index）會跟著資料順序跑掉，如果不重新編排的話，可能會造成後續處理混亂。這時就可以使用 `df.reset_index(drop=True)` 重新設定索引，這樣會重新設定索引，並移除原本的索引欄位，讓表格更乾淨整齊。
```python
print("\n排序與重排 'df.reset_index(drop=True)'")
df = df.reset_index(drop=True)
print(df)
"""
      Name    Class  Math  English  Science  Total    Average
0    Diana  Class A    92       85     90.0  267.0  89.000000
1    Alice  Class A    90       88     85.0  263.0  87.666667
2    Ethan  Class B    88       90     84.0  262.0  87.333333
3      Bob  Class B    85       92     79.0  256.0  85.333333
4  Charlie  Class C    78       80      NaN    NaN  79.000000
"""
```


## 資料篩選
Pandas 支援類似 Excel 的邏輯條件過濾功能，可以透過布林索引（Boolean Indexing）快速找到符合條件的資料。

為了更好的展示資料篩選的效果，我先把資料調整一下。
```python
data_expand = {
    "Name": ["Alice", "Bob", "Charlie", "Diana", "Ethan",
             "Fiona", "George", "Hannah", "Ian", "Julia"],
    "Class": ["Class A", "Class B", "Class C", "Class A", "Class B",
              "Class C", "Class A", "Class B", "Class C", "Class A"],
    "Math": [90, 85, 78, 92, 88, 79, 84, 91, 76, 89],
    "English": [88, 92, 80, 85, 90, 77, 83, 86, 81, 87],
    "Science": [85, 79, None, 90, 84, 82, None, 80, 78, 89]
}

df_e = pd.DataFrame(data_expand)
print("\nAll expand data:\n", df_e)

"""
All expand data:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83      NaN
7   Hannah  Class B    91       86     80.0
8      Ian  Class C    76       81     78.0
9    Julia  Class A    89       87     89.0
"""
```

先來看看一個簡單的條件式，會回傳一個布林值的 Series，這個結果代表只有第三筆資料（索引值為 2）符合條件，英文成績小於等於 85。
```python
print(df["English"] <= 85)

"""
0    False
1    False
2     True
3     True
4    False
5     True
6     True
7    False
8     True
9    False
Name: English, dtype: bool
"""
```

### 篩選英文成績小於等於 85 的學生  
可以將這個布林條件直接套用在 DataFrame 上，過濾出符合的列：
```python
# 篩選英文成績小於等於 85 的學生
print("\nEnglish <= 85:\n", df[df["English"] <= 85])

"""
English <= 85:
       Name    Class  Math  English  Science
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83      NaN
8      Ian  Class C    76       81     78.0
"""
```

### 篩選班級為 Class A 的學生  
同理，也可以用文字欄位做條件篩選，像是找出「Class A」的學生：
```python
# 篩選班級為 Class A 的學生
print("\nClass A only:\n", df[df["Class"] == "Class A"])

"""
Class A only:
      Name    Class  Math  English  Science
0   Alice  Class A    90       88     85.0
3   Diana  Class A    92       85     90.0
6  George  Class A    84       83      NaN
9   Julia  Class A    89       87     89.0
"""
```

### 多條件篩選 
若要同時滿足多個條件，必須用 `&` 搭配括號來包住每個條件，否則容易出錯。以下例子會篩選出「Class A」中自然成績大於 80 的學生：
```python
# 同時滿足多個條件（使用 & 和括號）
print("\nClass A and Science > 80:\n", df[(df["Class"] == "Class A") & (df["Science"] > 80)])

"""
Class A and Science > 80:
     Name    Class  Math  English  Science
0  Alice  Class A    90       88     85.0
3  Diana  Class A    92       85     90.0
9  Julia  Class A    89       87     89.0
"""
```

### 篩選多個值(使用 isin())
如果需要篩選出欄位值為多個可能的情況（像是 Class A 和 Class B），就可以使用 `isin()` 函數，它會回傳布林 Series，判斷哪些列的值存在於指定的列表中。
```python
# 篩選多個值(使用 isin())
print("\nClass A and B:\n", df_e[df_e["Class"].isin(["Class A", "Class B"])])
"""
Class A and B:
      Name    Class  Math  English  Science
0   Alice  Class A    90       88     85.0
1     Bob  Class B    85       92     79.0
3   Diana  Class A    92       85     90.0
4   Ethan  Class B    88       90     84.0
6  George  Class A    84       83      NaN
7  Hannah  Class B    91       86     80.0
9   Julia  Class A    89       87     89.0
"""
```

### 名字有包含li，模糊搜尋
若想針對文字欄位進行部分比對（像是名字中含有「li」），可以搭配 `str.contains()` 使用，它會針對每個字串進行關鍵字搜尋。
```python
# 名字有包含li，模糊搜尋
print("\nName contains 'li':\n", df_e[df_e["Name"].str.contains("li")])
"""
Name contains 'li':
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
2  Charlie  Class C    78       80      NaN
9    Julia  Class A    89       87     89.0
"""
```

## 缺失值
在進行資料分析時，經常會遇到資料缺失的情況，例如感應器故障、問卷未填寫、資料紀錄中斷等。這些缺失值如果沒有妥善處理，可能會影響整體統計結果與模型表現。因此，理解如何檢查、篩選、刪除與填補缺失值，是資料處理中的重要步驟。

### 檢查缺失值
第一步是檢查資料中哪些欄位或列包含缺失值。在 Pandas 中，可以使用 `isnull()` 搭配 `sum()` 快速掌握缺失狀況。


先列出原始資料。
```python
data_expand = {
    "Name": ["Alice", "Bob", "Charlie", "Diana", "Ethan",
             "Fiona", "George", "Hannah", "Ian", "Julia"],
    "Class": ["Class A", "Class B", "Class C", "Class A", "Class B",
              "Class C", "Class A", "Class B", "Class C", "Class A"],
    "Math": [90, 85, 78, 92, 88, 79, 84, 91, 76, 89],
    "English": [88, 92, 80, 85, 90, 77, 83, 86, 81, 87],
    "Science": [85, 79, None, 90, 84, 82, None, 80, 78, 89]
}

df = pd.DataFrame(data_expand)
print("\nAll expand data:\n", df)
"""
All expand data:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83      NaN
7   Hannah  Class B    91       86     80.0
8      Ian  Class C    76       81     78.0
9    Julia  Class A    89       87     89.0
"""
```

使用 `isnull()` ，會返回布林值。
```python
print("\nCheck if each value is null:\n", df.isnull())            # 顯示每個欄位是否為缺失值（True/False）
"""
Check if each value is null:
     Name  Class   Math  English  Science
0  False  False  False    False    False
1  False  False  False    False    False
2  False  False  False    False     True
3  False  False  False    False    False
4  False  False  False    False    False
5  False  False  False    False    False
6  False  False  False    False     True
7  False  False  False    False    False
8  False  False  False    False    False
9  False  False  False    False    False
"""
```

使用 `isnull()` 和 `sum()` ，確認資料缺失狀況。
```python
print("\nCount of null values in each column:\n",df.isnull().sum())      # 計算每個欄位中缺失值的數量
"""
Count of null values in each column:
 Name       0
Class      0
Math       0
English    0
Science    2
dtype: int64
"""
```

### 篩選缺失值
當需要進一步了解缺失值的分布或針對有缺失的資料進行觀察時，我們可以使用布林條件來篩選。例如，可以找出某欄位為空的列，或是任何欄位有缺失值的列。這個步驟常用來觀察資料是否有集中缺失、或僅是零星缺漏。

使用條件篩選，找出特定欄位中為缺失值的資料。
```python
print("\nRows where Science is null\n", df[df["Science"].isnull()])        # 找出 Science 欄位為缺失值的列
"""
Rows where Science is null
       Name    Class  Math  English  Science
2  Charlie  Class C    78       80      NaN
6   George  Class A    84       83      NaN
"""
```

篩選出任何一個欄位有缺失值的資料列。
```python
print("\nRows with any null value:\n", df[df.isnull().any(axis=1)])       # 找出任何欄位為缺失值的列
"""
Rows with any null value:
       Name    Class  Math  English  Science
2  Charlie  Class C    78       80      NaN
6   George  Class A    84       83      NaN
"""
```


### 刪除缺失值
若缺失資料過於零散，或該欄位對分析影響不大，可以選擇直接刪除含有缺失值的列。這是最簡單的處理方式，但也會導致資料量減少，需評估是否會影響分析結果的代表性。

當資料的缺失值無法合理補足時，可以考慮直接刪除，常見於少數缺失且樣本量足夠的情況。這行程式碼會刪除所有包含缺失值的列，但不會改變原始資料（除非加上 `inplace=True`）。刪除後的資料會較乾淨，適合進行統計分析，但也可能會損失部分資訊。
```python
print("\nDrop rows with any missing values:\n", df.dropna()) # 刪除任何缺失值的列
# df.dropna(inplace=True)      # 直接修改原本的 df
"""
Drop rows with any missing values:
      Name    Class  Math  English  Science
0   Alice  Class A    90       88     85.0
1     Bob  Class B    85       92     79.0
3   Diana  Class A    92       85     90.0
4   Ethan  Class B    88       90     84.0
5   Fiona  Class C    79       77     82.0
7  Hannah  Class B    91       86     80.0
8     Ian  Class C    76       81     78.0
9   Julia  Class A    89       87     89.0
"""
```

### 填補缺失值
若刪除資料會導致樣本不足，則可考慮以「補值」方式處理。補值策略應根據資料的特性與分析目的選擇，常見方式包括：
- 補 0
- 補平均值
- 向前填補（`ffill`）或向後填補（`bfill`）

#### 補 0
適用於像是成績、銷售等情境，缺席可視為「沒有表現」。
```python
print("\nFill missing values with 0:\n", df.fillna(0))
"""
Fill missing values with 0:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      0.0
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83      0.0
7   Hannah  Class B    91       86     80.0
8      Ian  Class C    76       81     78.0
9    Julia  Class A    89       87     89.0
"""
```

#### 補平均值
適合資料波動不大的情況，如氣溫、濕度等連續型資料。
```python
print("\nFill missing values in 'Science' with the mean:\n", df.fillna({"Science": df["Science"].mean()}))
"""
Fill missing values in 'Science' with the mean:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88   85.000
1      Bob  Class B    85       92   79.000
2  Charlie  Class C    78       80   83.375
3    Diana  Class A    92       85   90.000
4    Ethan  Class B    88       90   84.000
5    Fiona  Class C    79       77   82.000
6   George  Class A    84       83   83.375
7   Hannah  Class B    91       86   80.000
8      Ian  Class C    76       81   78.000
9    Julia  Class A    89       87   89.000
"""
```

#### 向前填補（`ffill`）或向後填補（`bfill`）
特別適合時間序列資料，像是感測器每分鐘紀錄一次的數據中，短時間內缺失可以用前後的資料推估補上。

```python
print("\nFill missing values using forward fill (ffill):\n", df.ffill())
"""
Fill missing values in 'Science' with the mean:
       Name    Class  Math  English  Science
0    Alice  Class A    90       88   85.000
1      Bob  Class B    85       92   79.000
2  Charlie  Class C    78       80   83.375
3    Diana  Class A    92       85   90.000
4    Ethan  Class B    88       90   84.000
5    Fiona  Class C    79       77   82.000
6   George  Class A    84       83   83.375
7   Hannah  Class B    91       86   80.000
8      Ian  Class C    76       81   78.000
9    Julia  Class A    89       87   89.000
"""

print("\nFill missing values using backward fill (bfill):\n", df.bfill())
"""
Fill missing values using forward fill (ffill):
       Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80     79.0
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    79       77     82.0
6   George  Class A    84       83     82.0
7   Hannah  Class B    91       86     80.0
8      Ian  Class C    76       81     78.0
9    Julia  Class A    89       87     89.0
"""
```


## 讀取檔案
在實際處理資料時，我們通常會從外部檔案讀取資料，例如 `.csv` 或 `.json`。Pandas 提供了多種方便的函式來幫助我們讀取這些常見格式。

以下介紹兩種常用的讀取方式：

- pd.read_csv()：用來讀取 CSV 檔案，是最常見的資料格式之一。
- pd.read_json()：用來讀取 JSON 格式的資料，也非常常見於 API 回傳的資料內容。

讀取後會得到一個 DataFrame，我們可以用 `df.head()` 來查看前幾筆資料內容，確認格式是否正確。


下面是實際讀入 CSV 的範例。
```python
import pandas as pd

df = pd.read_csv("./pandas_practice/data.csv")  # 檔名可以是相對路徑或絕對路徑
print(df)
"""
      Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    95       89     91.0
6   George  Class A    70       75     72.0
7   Hannah  Class B    83       87     85.0
8      Ian  Class C    60       65     62.0
9    Julia  Class A    88       90     89.0
"""
```

`df.head()` 預設是載入5筆資料，也可以依據需求來顯示不同數量的資料。
```python
print(df.head())  # 顯示前 5 筆資料
# print(df.head(3))  # 顯示前 3 筆資料
"""
      Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
"""
```

下面是實際讀入 JSON 的範例。
```python
df = pd.read_json("./pandas_practice/data.json")
print(df)
"""
      Name    Class  Math  English  Science
0    Alice  Class A    90       88     85.0
1      Bob  Class B    85       92     79.0
2  Charlie  Class C    78       80      NaN
3    Diana  Class A    92       85     90.0
4    Ethan  Class B    88       90     84.0
5    Fiona  Class C    91       89     88.0
6   George  Class A    76       81     70.0
7   Hannah  Class C    83       87     82.0
8      Ian  Class B    89       86     85.0
9    Julia  Class A    80       79     76.0
"""
```
