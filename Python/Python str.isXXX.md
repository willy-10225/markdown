# ✅ Python `str.isXXX()` 家族總表

## 一、常見字串判斷方法

| 方法            | 功能說明                                   | 範例                      | 結果     |
| --------------- | ------------------------------------------ | ------------------------- | -------- |
| `isalnum()`     | 是否全為英文字母或數字                     | `'abc123'.isalnum()`      | ✅ True  |
| `isalpha()`     | 是否全為英文字母                           | `'Hello'.isalpha()`       | ✅ True  |
| `isdigit()`     | 是否全為數字（0–9）                        | `'12345'.isdigit()`       | ✅ True  |
| `isdecimal()`   | 是否全為十進位數字（0–9，嚴格於 isdigit）  | `'⑤'.isdecimal()`         | ❌ False |
| `isnumeric()`   | 是否為任意數值字元（含中文數字、羅馬數字） | `'Ⅷ'.isnumeric()`         | ✅ True  |
| `islower()`     | 是否全為小寫字母                           | `'abc'.islower()`         | ✅ True  |
| `isupper()`     | 是否全為大寫字母                           | `'ABC'.isupper()`         | ✅ True  |
| `istitle()`     | 是否為標題格式（每個字首大寫）             | `'Hello World'.istitle()` | ✅ True  |
| `isspace()`     | 是否全為空白（含換行、tab）                | `' \n\t'.isspace()`       | ✅ True  |
| `isprintable()` | 是否全為可列印字元（非控制字元）           | `'abc\n'.isprintable()`   | ❌ False |
| `isascii()`     | 是否全為 ASCII 字元（0–127）               | `'Hello'.isascii()`       | ✅ True  |

## 二、三個數字相關方法差異（容易搞混）

| 方法          | 判斷範圍                     | `'②'` | `'Ⅷ'` | `'9'` |
| ------------- | ---------------------------- | ----- | ----- | ----- |
| `isdecimal()` | 僅限 0–9                     | ❌    | ❌    | ✅    |
| `isdigit()`   | 十進位與部分特殊數字         | ✅    | ❌    | ✅    |
| `isnumeric()` | 所有數字型態（含中文、羅馬） | ✅    | ✅    | ✅    |
