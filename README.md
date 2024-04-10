# PDF处理脚本

此脚本提供了一系列功能来处理PDF文件，包括裁剪页眉页脚、提取文本、过滤表格等。以下是各个功能的详细说明。

## 功能

### `crop_header_footer`

裁剪PDF文件的页眉和页脚。

#### 参数:
- `input_pdf`: 输入PDF文件的路径。
- `output_pdf`: 输出PDF文件的路径。

#### 返回:
- 无。

### `extract_covered_text`

提取PDF中被覆盖的页眉页脚区域的文本内容，并保存到文件中。

#### 参数:
- `input_pdf`: 输入PDF文件的路径。
- `output_path`: 输出文本文件的路径。

#### 返回:
- 无。

### `not_within_bboxes`

检查对象是否位于任何边界框内。

#### 参数:
- `obj`: PDF对象。
- `bboxes`: 边界框列表。

#### 返回:
- 如果对象不在任何边界框内则返回True，否则返回False。

### `extract_text_except_tables`

从PDF中提取除表格以外的文本内容，并保存到文件中。

#### 参数:
- `output_pdf`: 输出PDF文件的路径。
- `output_path`: 输出文本文件的路径。

#### 返回:
- 无。

### `remove_covered_text`

从输入文本中删除被覆盖的页眉页脚区域的内容，并保存到文件中。

#### 参数:
- `input_text_path`: 输入文本文件的路径。
- `covered_text_path`: 覆盖文本文件的路径。
- `output_path`: 输出文本文件的路径。

#### 返回:
- 无。

### `process_pdf`

处理PDF文件。

#### 参数:
- `input_pdf`: 输入PDF文件的路径。
- `output_folder`: 输出文件夹的路径。

#### 返回:
- 无。

## 使用方法

```python
def main():
    input_folder = r"C:\Users\hnkfl\Desktop\ocr\111"  # 输入文件夹路径
    output_folder = r"C:\Users\hnkfl\Desktop\ocr\111"  # 输出文件夹路径

    # 如果输出文件夹不存在，则创建它
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    # 遍历输入文件夹中的所有PDF文件
    for file_name in os.listdir(input_folder):
        if file_name.endswith('.pdf'):
            input_pdf = os.path.join(input_folder, file_name)
            process_pdf(input_pdf, output_folder)

if __name__ == "__main__":
    main()
```
