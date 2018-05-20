---
title: 用python pandas讀取大檔案
date: 2018-05-21 01:30:00
tags: 
- Python
categories:
- Python
---
有時候會需要用python來讀取一些比較大的檔案來對其做操作，我們可以使用pandas這個python來分塊讀取檔案，可以使用`pd.read_csv` 中的 `chunksize` 來設定一次讀入多少數據。

```python
import  numpy as np
filename = r'./[filename].csv'
chunksize = 10 ** 6

def process(df):
  # 你想要對檔案做的事情 ｡:.ﾟヽ(*´∀`)ﾉﾟ.:｡

data = pd.read_csv(filename, chunksize=chunksize)

# 運用chunksize來讀取檔案會使其建立 TextFileReader 因此要用下面方式來讀取文件內容
for df in data:
  process(df)
```

## Reference
- [pandas.read_csv API Reference](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)
- [How to read a 6 GB csv file with pandas](https://stackoverflow.com/questions/25962114/how-to-read-a-6-gb-csv-file-with-pandas)
- [Why the object, which I read a csv file using pandas from, is TextFileReader object](https://stackoverflow.com/questions/41844485/why-the-object-which-i-read-a-csv-file-using-pandas-from-is-textfilereader-obj)
