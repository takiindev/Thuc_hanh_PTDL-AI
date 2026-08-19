# PYTHON (PANDAS)

## HƯỚNG DẪN 50 HÀM & THUỘC TÍNH THƯỜNG DÙNG

*30 mục trong ảnh + 20 mục bổ sung thực tế*

| Mục | Nội dung |
|---|---|
| Đối tượng | Người mới học Pandas hoặc đang làm quen với phân tích dữ liệu bằng Python. |
| Cách đọc | Mỗi mục gồm: công dụng, cú pháp, ví dụ ngắn và lưu ý. |
| Thứ tự | Phần I giữ đúng 30 mục theo ảnh; Phần II là các hàm bổ sung nên biết. |
| Mục tiêu | Tra cứu nhanh, dễ đọc và có thể sao chép ví dụ để chạy thử. |

> Bố cục tối giản: ít khung, nhiều khoảng thở, code tách riêng để dễ đọc.

## 1. Chuẩn bị trước khi học

**Cài đặt:**

```bash
pip install pandas openpyxl pyarrow
```

```python
import pandas as pd

# DataFrame mẫu dùng xuyên suốt tài liệu
df = pd.DataFrame({
    "MSSV": ["SV01", "SV02", "SV03", "SV04", "SV05"],
    "Ten": ["An", "Binh", "Chi", "Dung", "Ha"],
    "Lop": ["CNTT1", "CNTT1", "CNTT2", "CNTT2", "CNTT1"],
    "Diem": [8.5, 7.0, None, 9.0, 6.5],
    "NgayDangKy": ["2026-08-01", "2026-08-02", "2026-08-03", "2026-08-03", "2026-08-05"]
})

# Bảng phụ cho ví dụ merge/join
df_lop = pd.DataFrame({
    "Lop": ["CNTT1", "CNTT2"],
    "CoVan": ["Thay A", "Co B"]
})
```

> Ghi nhớ: DataFrame có thể hình dung như một bảng Excel/SQL: mỗi cột có tên, mỗi dòng có chỉ số index.

> Quan trọng: shape, columns và dtypes là thuộc tính nên không có dấu ngoặc (). Các hàm như head(), info(), dropna()... là phương thức nên có ().

## 2. Cách dùng tài liệu

- Phần I: 30 hàm/thuộc tính xuất hiện trong ảnh - học theo đúng thứ tự 01 đến 30.

- Phần II: 20 hàm/thuộc tính bổ sung - hữu ích khi làm sạch, lọc, định hình và xuất dữ liệu.

- Phần III: 5 quy trình thực hành - kết hợp nhiều hàm thành một luồng xử lý hoàn chỉnh.

- Nếu cần tra cứu nhanh, chỉ cần nhìn tên hàm và các dòng Công dụng - Cú pháp - Ví dụ - Lưu ý.

## PHẦN I - 30 HÀM/THUỘC TÍNH TRONG ẢNH

> Giữ đúng thứ tự từ 01 đến 30 để đối chiếu trực tiếp với ảnh.

### 01. pd.read_csv()

**Dùng để:** Đọc file CSV vào DataFrame.

**Cú pháp thường dùng:** `pd.read_csv(path, sep=",", usecols=None, dtype=None, ...)`

```python
df_csv = pd.read_csv("students.csv")

# Chỉ đọc vài cột
df_csv = pd.read_csv("students.csv", usecols=["MSSV", "Ten", "Diem"])

# CSV dùng dấu chấm phẩy
df_csv = pd.read_csv("students.csv", sep=";")
```

**Kết quả / ý nghĩa:** Trả về một DataFrame. Đây là hàm phổ biến nhất để nhập dữ liệu dạng văn bản phân cách.

- Nếu lỗi font tiếng Việt, có thể thử encoding="utf-8" hoặc encoding phù hợp với file.

- dtype=... giúp ép kiểu ngay khi đọc.

### 02. pd.read_excel()

**Dùng để:** Đọc file Excel (.xlsx/.xls...) vào DataFrame.

**Cú pháp thường dùng:** `pd.read_excel(path, sheet_name=0, usecols=None, ...)`

```python
df_excel = pd.read_excel("students.xlsx")

# Đọc sheet theo tên
df_excel = pd.read_excel("students.xlsx", sheet_name="SinhVien")

# Đọc nhiều sheet
sheets = pd.read_excel("students.xlsx", sheet_name=None)
```

**Kết quả / ý nghĩa:** Mặc định trả về DataFrame; khi sheet_name=None trả về dict các DataFrame.

- Với .xlsx thường cần openpyxl.

- sheet_name có thể là tên sheet, số thứ tự hoặc danh sách.

### 03. df.head()

**Dùng để:** Xem nhanh các dòng đầu của DataFrame.

**Cú pháp thường dùng:** `df.head(n=5)`

```python
df.head()      # 5 dòng đầu
df.head(10)    # 10 dòng đầu
```

**Kết quả / ý nghĩa:** Giúp kiểm tra cấu trúc và dữ liệu mẫu ngay sau khi đọc file.

- Dùng head() ngay sau read_csv/read_excel là thói quen tốt.

### 04. df.tail()

**Dùng để:** Xem nhanh các dòng cuối của DataFrame.

**Cú pháp thường dùng:** `df.tail(n=5)`

```python
df.tail()      # 5 dòng cuối
df.tail(3)     # 3 dòng cuối
```

**Kết quả / ý nghĩa:** Hữu ích để kiểm tra phần cuối dữ liệu, đặc biệt khi file được nối thêm dữ liệu.

- Có thể phát hiện dòng tổng cộng hoặc dòng rỗng ở cuối file.

### 05. df.info()

**Dùng để:** Xem tổng quan số dòng, số cột, kiểu dữ liệu và số giá trị không rỗng.

**Cú pháp thường dùng:** `df.info()`

```python
df.info()
```

**Kết quả / ý nghĩa:** In ra thông tin tổng quan; rất hữu ích để phát hiện cột sai kiểu và cột có nhiều giá trị thiếu.

- info() in ra màn hình và thường không cần gán vào biến.

### 06. df.describe()

**Dùng để:** Tạo thống kê mô tả cho các cột.

**Cú pháp thường dùng:** `df.describe(include=None)`

```python
df.describe()                 # cột số
df.describe(include="all")    # nhiều kiểu dữ liệu hơn
```

**Kết quả / ý nghĩa:** Với cột số thường có count, mean, std, min, quartile và max.

- include="all" giúp xem cả cột chuỗi/phân loại.

### 07. df.shape

**Dùng để:** Lấy kích thước DataFrame.

**Cú pháp thường dùng:** `df.shape`

```python
rows, cols = df.shape
print(rows)  # số dòng
print(cols)  # số cột
```

**Kết quả / ý nghĩa:** Trả về tuple (số_dòng, số_cột).

- Đây là thuộc tính, KHÔNG viết df.shape().

### 08. df.columns

**Dùng để:** Lấy danh sách nhãn cột.

**Cú pháp thường dùng:** `df.columns`

```python
print(df.columns)

# Chuyển thành list Python
cols = df.columns.tolist()
```

**Kết quả / ý nghĩa:** Trả về đối tượng Index chứa tên các cột.

- Đây là thuộc tính, không có ().

- Có thể gán df.columns = [...] để đổi toàn bộ tên cột, nhưng rename() an toàn hơn khi chỉ đổi vài cột.

### 09. df.dtypes

**Dùng để:** Xem kiểu dữ liệu của từng cột.

**Cú pháp thường dùng:** `df.dtypes`

```python
print(df.dtypes)
```

**Kết quả / ý nghĩa:** Cho biết cột đang là int, float, string/object, datetime...

- Đây là thuộc tính, không có ().

- Nếu kiểu không đúng, thường dùng astype() hoặc pd.to_datetime().

### 10. df.rename()

**Dùng để:** Đổi tên cột hoặc index.

**Cú pháp thường dùng:** `df.rename(columns={...}, inplace=False)`

```python
df2 = df.rename(columns={
    "Ten": "HoTen",
    "Diem": "DiemTong"
})

# Sửa trực tiếp df
df.rename(columns={"Ten": "HoTen"}, inplace=True)
```

**Kết quả / ý nghĩa:** Trả về DataFrame đã đổi tên nếu inplace=False; hoặc sửa trực tiếp nếu inplace=True.

- Nên ưu tiên gán kết quả vào biến mới khi đang học để dễ kiểm soát.

### 11. df.isnull() / df.isna()

**Dùng để:** Phát hiện giá trị thiếu (NA/NaN/None).

**Cú pháp thường dùng:** `df.isna()`

```python
mask = df.isna()
print(mask)

# Đếm số giá trị thiếu mỗi cột
print(df.isna().sum())
```

**Kết quả / ý nghĩa:** Trả về bảng True/False cùng kích thước với DataFrame.

- isnull() là tên tương đương với isna().

- Chuỗi rỗng "" không mặc định được coi là NA.

### 12. df.notnull() / df.notna()

**Dùng để:** Kiểm tra giá trị KHÔNG bị thiếu.

**Cú pháp thường dùng:** `df.notna()`

```python
# Lấy các dòng có Điểm
df_co_diem = df[df["Diem"].notna()]
```

**Kết quả / ý nghĩa:** Trả về True tại vị trí có dữ liệu hợp lệ và False tại vị trí thiếu.

- notnull() là tên tương đương với notna().

### 13. df.dropna()

**Dùng để:** Xóa dòng hoặc cột chứa giá trị thiếu.

**Cú pháp thường dùng:** `df.dropna(axis=0, how="any", subset=None, ...)`

```python
# Xóa mọi dòng có ít nhất 1 giá trị thiếu
df_clean = df.dropna()

# Chỉ xét cột Diem
df_clean = df.dropna(subset=["Diem"])

# Xóa cột có giá trị thiếu
df_cols = df.dropna(axis=1)
```

**Kết quả / ý nghĩa:** Trả về DataFrame đã loại bỏ dữ liệu thiếu theo điều kiện.

- Cẩn thận: dropna() có thể làm mất nhiều dòng. Nên kiểm tra df.isna().sum() trước.

### 14. df.fillna()

**Dùng để:** Điền giá trị thay thế vào ô bị thiếu.

**Cú pháp thường dùng:** `df.fillna(value)`

```python
# Điền 0 cho toàn bộ ô thiếu
df2 = df.fillna(0)

# Chỉ điền cột Diem bằng trung bình
df2 = df.copy()
df2["Diem"] = df2["Diem"].fillna(df2["Diem"].mean())
```

**Kết quả / ý nghĩa:** Giữ nguyên số dòng và thay các giá trị thiếu bằng giá trị chỉ định.

- Có thể dùng ffill()/bfill() khi cần lấy giá trị trước/sau theo thứ tự dữ liệu.

### 15. df.groupby()

**Dùng để:** Chia dữ liệu thành nhóm theo một hoặc nhiều cột để tính toán.

**Cú pháp thường dùng:** `df.groupby(by)[column].agg(...)`

```python
# Điểm trung bình theo lớp
avg = df.groupby("Lop")["Diem"].mean()

# Nhóm theo nhiều cột
result = df.groupby(["Lop"])["Diem"].count()
```

**Kết quả / ý nghĩa:** Tạo đối tượng GroupBy; thường kết hợp mean(), sum(), count(), agg()...

- groupby() giống ý tưởng GROUP BY trong SQL.

### 16. agg()

**Dùng để:** Áp dụng một hoặc nhiều phép tổng hợp.

**Cú pháp thường dùng:** `obj.agg(func) / groupby.agg({...})`

```python
summary = df.groupby("Lop").agg(
    DiemTB=("Diem", "mean"),
    DiemMax=("Diem", "max"),
    SoSV=("MSSV", "count")
)
```

**Kết quả / ý nghĩa:** Cho phép tạo nhiều chỉ số tổng hợp trong một lần.

- Cú pháp named aggregation như trên giúp tên cột kết quả rõ ràng.

### 17. df.sort_values()

**Dùng để:** Sắp xếp dòng theo giá trị của một hoặc nhiều cột.

**Cú pháp thường dùng:** `df.sort_values(by, ascending=True)`

```python
# Tăng dần theo điểm
df_sorted = df.sort_values("Diem")

# Giảm dần
df_sorted = df.sort_values("Diem", ascending=False)

# Nhiều cột
df_sorted = df.sort_values(["Lop", "Diem"], ascending=[True, False])
```

**Kết quả / ý nghĩa:** Trả về DataFrame được sắp xếp.

- NaN thường được đặt cuối; có thể điều chỉnh bằng na_position.

### 18. value_counts()

**Dùng để:** Đếm số lần xuất hiện của từng giá trị.

**Cú pháp thường dùng:** `series.value_counts(normalize=False)`

```python
# Số sinh viên mỗi lớp
counts = df["Lop"].value_counts()

# Tỉ lệ phần trăm
ratio = df["Lop"].value_counts(normalize=True) * 100
```

**Kết quả / ý nghĩa:** Trả về số lượng hoặc tỉ lệ của mỗi giá trị.

- Rất hữu ích với cột phân loại như lớp, giới tính, trạng thái.

### 19. merge()

**Dùng để:** Ghép hai DataFrame theo khóa, tương tự JOIN trong SQL.

**Cú pháp thường dùng:** `pd.merge(left, right, on=..., how="inner")`

```python
merged = pd.merge(df, df_lop, on="Lop", how="left")

# Cũng có thể dùng phương thức
merged2 = df.merge(df_lop, on="Lop", how="left")
```

**Kết quả / ý nghĩa:** Kết quả chứa các cột từ cả hai bảng theo khóa ghép.

- how thường dùng: inner, left, right, outer.

- Nếu tên khóa khác nhau, dùng left_on=... và right_on=....

### 20. df.join()

**Dùng để:** Ghép DataFrame chủ yếu dựa trên index hoặc cột chỉ định.

**Cú pháp thường dùng:** `df.join(other, on=None, how="left")`

```python
left = df.set_index("Lop")
right = df_lop.set_index("Lop")
joined = left.join(right, how="left")
```

**Kết quả / ý nghĩa:** Ghép nhanh khi index của hai bảng đã được chuẩn bị phù hợp.

- merge() linh hoạt và dễ hiểu hơn khi ghép theo cột khóa; join() tiện khi ghép theo index.

### 21. pd.concat()

**Dùng để:** Nối nhiều DataFrame theo dòng hoặc theo cột.

**Cú pháp thường dùng:** `pd.concat(objs, axis=0, ignore_index=False)`

```python
df_a = df.iloc[:3]
df_b = df.iloc[3:]

# Nối theo dòng
all_rows = pd.concat([df_a, df_b], ignore_index=True)

# Nối theo cột
side_by_side = pd.concat([df_a.reset_index(drop=True), df_b.reset_index(drop=True)], axis=1)
```

**Kết quả / ý nghĩa:** axis=0 nối dọc theo dòng; axis=1 nối ngang theo cột.

- concat() không phải merge theo khóa; nó ghép theo trục/index.

### 22. apply()

**Dùng để:** Áp dụng một hàm lên Series hoặc theo hàng/cột của DataFrame.

**Cú pháp thường dùng:** `series.apply(func) / df.apply(func, axis=...)`

```python
# Series: biến điểm thành chuỗi
df2 = df.copy()
df2["XepLoai"] = df2["Diem"].apply(
    lambda x: "Dat" if pd.notna(x) and x >= 5 else "Chua xet"
)

# DataFrame: tính trên từng dòng
df2["MoTa"] = df2.apply(lambda row: f"{row['Ten']} - {row['Lop']}", axis=1)
```

**Kết quả / ý nghĩa:** Linh hoạt khi logic xử lý không có sẵn bằng vector hóa.

- Ưu tiên phép toán vector hóa có sẵn của Pandas khi có thể vì thường nhanh và rõ hơn apply().

### 23. map()

**Dùng để:** Ánh xạ giá trị theo dict hoặc hàm; với DataFrame mới, map() có thể áp dụng hàm theo từng ô.

**Cú pháp thường dùng:** `series.map(mapping_or_func) / df.map(func)`

```python
# Series.map(): đổi mã lớp thành tên đầy đủ
mapping = {"CNTT1": "Cong nghe thong tin 1", "CNTT2": "Cong nghe thong tin 2"}
df2 = df.copy()
df2["TenLop"] = df2["Lop"].map(mapping)

# DataFrame.map(): xử lý từng phần tử số/chuỗi phù hợp
small = pd.DataFrame({"A": [1, 2], "B": [3, 4]})
small2 = small.map(lambda x: x * 10)
```

**Kết quả / ý nghĩa:** Series.map() rất tiện để thay mã bằng nhãn; DataFrame.map() áp dụng hàm nhận/trả về từng giá trị.

- Trong pandas hiện đại, DataFrame.applymap() đã được đổi tên/deprecate thành DataFrame.map(); tài liệu này ưu tiên map().

### 24. pivot_table()

**Dùng để:** Tạo bảng tổng hợp kiểu PivotTable của Excel.

**Cú pháp thường dùng:** `df.pivot_table(values=..., index=..., columns=..., aggfunc=...)`

```python
pivot = df.pivot_table(
    values="Diem",
    index="Lop",
    aggfunc="mean"
)
```

**Kết quả / ý nghĩa:** Tạo bảng tổng hợp đa chiều, tự xử lý nhóm và phép tổng hợp.

- Có thể dùng aggfunc=["mean", "max", "count"] để lấy nhiều thống kê.

### 25. pd.crosstab()

**Dùng để:** Tạo bảng chéo tần suất giữa hai hoặc nhiều biến phân loại.

**Cú pháp thường dùng:** `pd.crosstab(index, columns, normalize=False)`

```python
# Tạo thêm cột trạng thái minh họa
df2 = df.copy()
df2["Dat"] = df2["Diem"].fillna(0).ge(5)

ct = pd.crosstab(df2["Lop"], df2["Dat"])
ct_pct = pd.crosstab(df2["Lop"], df2["Dat"], normalize="index") * 100
```

**Kết quả / ý nghĩa:** Cho biết số lượng hoặc tỉ lệ ở từng giao điểm hàng-cột.

- Phù hợp cho phân tích biến phân loại và bảng tần suất.

### 26. nunique()

**Dùng để:** Đếm số giá trị khác nhau.

**Cú pháp thường dùng:** `series.nunique(dropna=True) / df.nunique()`

```python
# Có bao nhiêu lớp khác nhau?
print(df["Lop"].nunique())

# Số giá trị khác nhau ở từng cột
print(df.nunique())
```

**Kết quả / ý nghĩa:** Trả về số lượng giá trị duy nhất.

- Khác unique(): nunique() trả về SỐ LƯỢNG, unique() trả về CÁC GIÁ TRỊ.

### 27. unique()

**Dùng để:** Lấy danh sách các giá trị duy nhất của một Series.

**Cú pháp thường dùng:** `series.unique()`

```python
classes = df["Lop"].unique()
print(classes)
```

**Kết quả / ý nghĩa:** Trả về mảng chứa các giá trị không trùng lặp theo thứ tự xuất hiện.

- Thường dùng trên một cột: df["Lop"].unique().

### 28. astype()

**Dùng để:** Ép kiểu dữ liệu cho Series/DataFrame.

**Cú pháp thường dùng:** `obj.astype(dtype)`

```python
df2 = df.copy()
df2["MSSV"] = df2["MSSV"].astype("string")

# Ép nhiều cột
df2 = df2.astype({"MSSV": "string", "Lop": "string"})
```

**Kết quả / ý nghĩa:** Trả về đối tượng với kiểu dữ liệu được chuyển đổi.

- Nếu dữ liệu bẩn (ví dụ "abc" trong cột số), astype(float) có thể lỗi; khi đó nên làm sạch trước.

### 29. pd.to_datetime()

**Dùng để:** Chuyển chuỗi/cột sang kiểu ngày giờ.

**Cú pháp thường dùng:** `pd.to_datetime(arg, errors="raise", format=None)`

```python
df2 = df.copy()
df2["NgayDangKy"] = pd.to_datetime(df2["NgayDangKy"])

# Dữ liệu lỗi -> NaT thay vì dừng chương trình
df2["NgayDangKy"] = pd.to_datetime(df2["NgayDangKy"], errors="coerce")
```

**Kết quả / ý nghĩa:** Sau khi chuyển, có thể dùng .dt.year, .dt.month, .dt.day...

- errors="coerce" biến giá trị không parse được thành NaT.

### 30. df.to_csv()

**Dùng để:** Ghi DataFrame ra file CSV.

**Cú pháp thường dùng:** `df.to_csv(path, index=False, encoding=...)`

```python
df.to_csv("ket_qua.csv", index=False, encoding="utf-8-sig")
```

**Kết quả / ý nghĩa:** Tạo file CSV từ DataFrame.

- index=False thường được dùng để không ghi cột index ra file.

- utf-8-sig giúp Excel trên Windows nhận tiếng Việt tốt trong nhiều trường hợp.

## PHẦN II - 20 HÀM/THUỘC TÍNH BỔ SUNG

> Các mục này thường gặp trong xử lý dữ liệu thực tế và giúp hoàn thiện bộ công cụ Pandas cơ bản.

### 31. df.to_excel()

**Dùng để:** Ghi DataFrame ra file Excel.

**Cú pháp thường dùng:** `df.to_excel(path, index=False, sheet_name="Sheet1")`

```python
df.to_excel("ket_qua.xlsx", index=False, sheet_name="SinhVien")
```

**Kết quả / ý nghĩa:** Tạo file Excel từ DataFrame.

- Với .xlsx thường dùng engine openpyxl.

### 32. df.sample()

**Dùng để:** Lấy ngẫu nhiên một số dòng để kiểm tra dữ liệu.

**Cú pháp thường dùng:** `df.sample(n=None, frac=None, random_state=None)`

```python
df.sample(3, random_state=42)

# Lấy 20% số dòng
df.sample(frac=0.2, random_state=42)
```

**Kết quả / ý nghĩa:** Trả về mẫu ngẫu nhiên của dữ liệu.

- random_state giúp kết quả ngẫu nhiên lặp lại được.

### 33. df.loc[]

**Dùng để:** Chọn dữ liệu theo nhãn index/cột và điều kiện boolean.

**Cú pháp thường dùng:** `df.loc[rows, columns]`

```python
# Lấy các dòng điểm >= 8, chỉ chọn Ten và Diem
high = df.loc[df["Diem"] >= 8, ["Ten", "Diem"]]

# Gán giá trị an toàn theo điều kiện
df2 = df.copy()
df2.loc[df2["Diem"].isna(), "Diem"] = 0
```

**Kết quả / ý nghĩa:** loc rất phù hợp khi lọc theo điều kiện hoặc làm việc theo tên cột.

- Cú pháp gán với loc giúp tránh nhiều lỗi liên quan tới chained assignment.

### 34. df.iloc[]

**Dùng để:** Chọn dữ liệu theo vị trí số nguyên.

**Cú pháp thường dùng:** `df.iloc[row_positions, col_positions]`

```python
# 3 dòng đầu, 2 cột đầu
part = df.iloc[:3, :2]

# Dòng thứ 2, cột thứ 4 (đếm từ 0)
value = df.iloc[1, 3]
```

**Kết quả / ý nghĩa:** Hoạt động giống slicing theo vị trí, bắt đầu từ 0.

- loc dùng nhãn; iloc dùng vị trí.

### 35. df.query()

**Dùng để:** Lọc các dòng bằng biểu thức dạng chuỗi dễ đọc.

**Cú pháp thường dùng:** `df.query("expression")`

```python
high = df.query("Diem >= 8")

lop1 = df.query("Lop == 'CNTT1'")

min_score = 7
filtered = df.query("Diem >= @min_score")
```

**Kết quả / ý nghĩa:** Trả về DataFrame thỏa điều kiện.

- Dùng @ten_bien để tham chiếu biến Python bên ngoài biểu thức.

### 36. df.drop()

**Dùng để:** Xóa dòng hoặc cột theo nhãn.

**Cú pháp thường dùng:** `df.drop(labels=..., axis=...) / df.drop(columns=[...])`

```python
# Xóa cột
df2 = df.drop(columns=["NgayDangKy"])

# Xóa dòng theo index
df3 = df.drop(index=[0, 1])
```

**Kết quả / ý nghĩa:** Trả về DataFrame bỏ đi các dòng/cột được chỉ định.

- Khác dropna(): drop() xóa theo tên/index, còn dropna() xóa dựa trên giá trị thiếu.

### 37. df.replace()

**Dùng để:** Thay một hoặc nhiều giá trị bằng giá trị khác.

**Cú pháp thường dùng:** `df.replace(to_replace, value)`

```python
df2 = df.replace({"CNTT1": "KTPM", "CNTT2": "HTTT"})

# Trên một cột
df2["Lop"] = df2["Lop"].replace({"CNTT1": "Lop 1"})
```

**Kết quả / ý nghĩa:** Thay giá trị theo quy tắc trực tiếp.

- map() thường dùng để tạo/ánh xạ một Series; replace() tiện để thay giá trị hiện có.

### 38. df.duplicated()

**Dùng để:** Đánh dấu các dòng bị trùng lặp.

**Cú pháp thường dùng:** `df.duplicated(subset=None, keep="first")`

```python
# True ở các dòng bị xem là trùng
dup_mask = df.duplicated()

# Trùng theo MSSV
dup_mssv = df.duplicated(subset=["MSSV"], keep=False)
```

**Kết quả / ý nghĩa:** Trả về Series True/False.

- keep=False đánh dấu tất cả bản ghi thuộc nhóm trùng.

### 39. df.drop_duplicates()

**Dùng để:** Xóa các dòng trùng lặp.

**Cú pháp thường dùng:** `df.drop_duplicates(subset=None, keep="first")`

```python
unique_rows = df.drop_duplicates()

# Chỉ xét MSSV, giữ bản ghi cuối
unique_mssv = df.drop_duplicates(subset=["MSSV"], keep="last")
```

**Kết quả / ý nghĩa:** Trả về DataFrame đã loại trùng.

- Nên kiểm tra duplicated() trước khi xóa để biết dữ liệu nào sẽ bị ảnh hưởng.

### 40. df.assign()

**Dùng để:** Tạo thêm một hoặc nhiều cột và trả về DataFrame mới.

**Cú pháp thường dùng:** `df.assign(NewCol=...)`

```python
df2 = df.assign(
    DiemCong=lambda x: x["Diem"].fillna(0) + 0.5,
    Dat=lambda x: x["Diem"].fillna(0) >= 5
)
```

**Kết quả / ý nghĩa:** Tiện cho pipeline vì không cần sửa trực tiếp DataFrame gốc.

- Lambda trong assign có thể tham chiếu DataFrame đang được xây dựng.

### 41. df.reset_index()

**Dùng để:** Đưa index trở về 0,1,2,... và tùy chọn giữ index cũ thành cột.

**Cú pháp thường dùng:** `df.reset_index(drop=False)`

```python
df_indexed = df.set_index("MSSV")

# Đưa MSSV từ index về lại cột
back = df_indexed.reset_index()

# Bỏ hẳn index cũ
clean = df_indexed.reset_index(drop=True)
```

**Kết quả / ý nghĩa:** Khôi phục RangeIndex mặc định.

- drop=True bỏ index cũ thay vì tạo thành cột.

### 42. df.set_index()

**Dùng để:** Chọn một hoặc nhiều cột làm index.

**Cú pháp thường dùng:** `df.set_index(keys)`

```python
df_by_id = df.set_index("MSSV")

# MultiIndex
df_multi = df.set_index(["Lop", "MSSV"])
```

**Kết quả / ý nghĩa:** DataFrame có index mang ý nghĩa nghiệp vụ hơn.

- Hữu ích khi join theo index hoặc tra cứu theo khóa.

### 43. melt()

**Dùng để:** Chuyển bảng dạng rộng (wide) thành dạng dài (long).

**Cú pháp thường dùng:** `df.melt(id_vars=..., value_vars=..., var_name=..., value_name=...)`

```python
wide = pd.DataFrame({
    "MSSV": ["SV01", "SV02"],
    "Toan": [8, 7],
    "Ly": [9, 6]
})

long = wide.melt(
    id_vars="MSSV",
    var_name="Mon",
    value_name="Diem"
)
```

**Kết quả / ý nghĩa:** Các cột môn học được gom thành một cột Mon và một cột Diem.

- melt() rất hữu ích trước khi vẽ biểu đồ hoặc phân tích theo biến.

### 44. df.explode()

**Dùng để:** Tách phần tử list-like trong một ô thành nhiều dòng.

**Cú pháp thường dùng:** `df.explode(column)`

```python
x = pd.DataFrame({
    "MSSV": ["SV01", "SV02"],
    "KyNang": [["Python", "SQL"], ["Java"]]
})

x2 = x.explode("KyNang", ignore_index=True)
```

**Kết quả / ý nghĩa:** Mỗi phần tử trong danh sách trở thành một dòng riêng.

- Rất hữu ích với dữ liệu JSON hoặc cột chứa list.

### 45. .str accessor

**Dùng để:** Áp dụng thao tác chuỗi theo kiểu vector hóa cho Series.

**Cú pháp thường dùng:** `series.str.<method>()`

```python
df2 = df.copy()
df2["TenUpper"] = df2["Ten"].str.upper()
df2["TenLength"] = df2["Ten"].str.len()
df2["BatDauB"] = df2["Ten"].str.startswith("B")
```

**Kết quả / ý nghĩa:** Cho phép xử lý cả cột chuỗi mà không cần viết vòng lặp Python.

- Một số hàm hay dùng: lower(), upper(), strip(), contains(), replace(), split().

### 46. .dt accessor

**Dùng để:** Trích xuất/thao tác thành phần ngày giờ trên Series datetime.

**Cú pháp thường dùng:** `series.dt.<property/method>`

```python
df2 = df.copy()
df2["NgayDangKy"] = pd.to_datetime(df2["NgayDangKy"])
df2["Nam"] = df2["NgayDangKy"].dt.year
df2["Thang"] = df2["NgayDangKy"].dt.month
df2["Thu"] = df2["NgayDangKy"].dt.day_name()
```

**Kết quả / ý nghĩa:** Tạo các cột thời gian phục vụ phân tích theo ngày/tháng/năm.

- Cột phải là kiểu datetime trước khi dùng .dt.

### 47. transform()

**Dùng để:** Tính theo nhóm nhưng trả kết quả có cùng số dòng với dữ liệu gốc.

**Cú pháp thường dùng:** `groupby.transform(func)`

```python
df2 = df.copy()
df2["DiemTB_Lop"] = df2.groupby("Lop")["Diem"].transform("mean")
df2["LechSoVoiTB"] = df2["Diem"] - df2["DiemTB_Lop"]
```

**Kết quả / ý nghĩa:** Mỗi dòng nhận được thống kê của chính nhóm mà nó thuộc về.

- Khác agg(): agg() thường làm giảm số dòng; transform() giữ số dòng để gắn kết quả ngược vào bảng gốc.

### 48. round()

**Dùng để:** Làm tròn số theo số chữ số thập phân.

**Cú pháp thường dùng:** `df.round(decimals)`

```python
df2 = df.copy()
df2["Diem"] = df2["Diem"].round(1)

# Nhiều cột với số chữ số khác nhau
# df2.round({"Diem": 1, "TyLe": 2})
```

**Kết quả / ý nghĩa:** Trả về giá trị số đã được làm tròn theo yêu cầu.

- Hữu ích trước khi trình bày báo cáo hoặc xuất file.

### 49. pd.read_json()

**Dùng để:** Đọc dữ liệu JSON vào DataFrame.

**Cú pháp thường dùng:** `pd.read_json(path_or_buf, ...)`

```python
df_json = pd.read_json("students.json")
```

**Kết quả / ý nghĩa:** Biến dữ liệu JSON phù hợp thành DataFrame.

- Với JSON lồng sâu, có thể cần pd.json_normalize() để làm phẳng trước khi phân tích.

### 50. pd.read_parquet()

**Dùng để:** Đọc file Parquet - định dạng cột hiệu quả cho dữ liệu lớn.

**Cú pháp thường dùng:** `pd.read_parquet(path, columns=None, ...)`

```python
df_parquet = pd.read_parquet("students.parquet")

# Chỉ đọc vài cột
df_parquet = pd.read_parquet("students.parquet", columns=["MSSV", "Diem"])
```

**Kết quả / ý nghĩa:** Trả về DataFrame; thường nhanh và giữ kiểu dữ liệu tốt hơn CSV trong pipeline dữ liệu.

- Thường cần pyarrow hoặc engine Parquet tương thích.

## PHẦN III - 5 QUY TRÌNH THỰC HÀNH

> Thay vì học từng hàm rời rạc, có thể luyện theo 5 luồng xử lý dưới đây.

### Quy trình 1 - Mở file và kiểm tra nhanh

```python
df = pd.read_csv("data.csv")
print(df.shape)
print(df.columns)
df.info()
print(df.head())
```

### Quy trình 2 - Kiểm tra và xử lý dữ liệu thiếu

```python
print(df.isna().sum())

df["Diem"] = df["Diem"].fillna(df["Diem"].mean())
df = df.dropna(subset=["MSSV"])
```

### Quy trình 3 - Lọc, sắp xếp, chọn cột

```python
result = (
    df.loc[df["Diem"] >= 7, ["MSSV", "Ten", "Lop", "Diem"]]
      .sort_values("Diem", ascending=False)
)
```

### Quy trình 4 - Tổng hợp theo nhóm

```python
summary = (
    df.groupby("Lop")
      .agg(SoSV=("MSSV", "count"), DiemTB=("Diem", "mean"))
      .reset_index()
      .sort_values("DiemTB", ascending=False)
)
```

### Quy trình 5 - Ghép bảng rồi xuất Excel

```python
result = (
    df.merge(df_lop, on="Lop", how="left")
      .sort_values(["Lop", "Diem"], ascending=[True, False])
)

result.to_excel("bao_cao.xlsx", index=False)
```

## Tra cứu nhanh: cần làm gì thì dùng hàm nào?

| Nhu cầu | Hàm nên dùng |
|---|---|
| Đọc CSV | pd.read_csv() |
| Đọc Excel | pd.read_excel() |
| Xem số dòng / số cột | df.shape |
| Xem kiểu dữ liệu | df.dtypes hoặc df.info() |
| Kiểm tra dữ liệu thiếu | df.isna().sum() |
| Xóa / điền dữ liệu thiếu | df.dropna() / df.fillna() |
| Lọc dòng | df.loc[...] hoặc df.query() |
| Sắp xếp | df.sort_values() |
| Đếm tần suất | value_counts() |
| Nhóm và tổng hợp | groupby() + agg() |
| Ghép hai bảng theo khóa | merge() |
| Nối nhiều bảng | pd.concat() |
| Đổi kiểu dữ liệu | astype() / pd.to_datetime() |
| Tạo bảng tổng hợp | pivot_table() / pd.crosstab() |
| Xuất file | to_csv() / to_excel() |

## Những lỗi người mới thường gặp

- Viết df.shape(): sai vì shape là thuộc tính. Đúng: df.shape.

- Nhầm loc và iloc: loc dùng nhãn/tên; iloc dùng vị trí số nguyên.

- Dùng dropna() quá sớm: có thể làm mất nhiều dữ liệu. Nên xem df.isna().sum() trước.

- Ép astype(int) khi còn NaN: có thể lỗi. Xử lý dữ liệu thiếu trước hoặc dùng kiểu nullable phù hợp.

- merge() với khóa bị trùng: có thể làm số dòng tăng mạnh. Nên kiểm tra unique() hoặc value_counts() của khóa.

- Lạm dụng apply(): nhiều tác vụ có thể viết rõ hơn bằng .str, .dt, phép toán vector hóa hoặc map().

- Quên index=False khi xuất file: CSV/Excel có thể xuất thêm cột index 0, 1, 2... ngoài ý muốn.

## Gợi ý học nhanh

> Ưu tiên học chắc: read_csv, head, info, describe, shape, columns, dtypes, isna, fillna, dropna, loc, sort_values, groupby, agg và merge. Sau đó mở rộng sang pivot_table, melt, explode, .str và .dt.
