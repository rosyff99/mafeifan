
# PDF Processing Scripts

这些脚本提供了一系列功能来处理PDF文件，包括裁剪页眉页脚、提取文本、过滤表格等。下面是各个功能及其算法逻辑和示例代码的详细说明。

## 功能和示例

### `crop_header_footer`

**功能**: 裁剪PDF文件的页眉和页脚。

**算法逻辑**:
1. 打开PDF文件。
2. 遍历每一页，对于每一页:
   - 定义页眉和页脚的矩形区域。
   - 使用白色矩形覆盖页眉和页脚区域。
3. 保存更改并关闭PDF文档。

**代码示例**:
```python
def crop_header_footer(input_pdf, output_pdf):
    pdf_document = fitz.open(input_pdf)
    for page_num in range(pdf_document.page_count):
        page = pdf_document[page_num]
        header_rect = fitz.Rect(0, 0, page.rect.width, 70)
        footer_rect = fitz.Rect(0, page.rect.height - 70, page.rect.width, page.rect.height)
        page.draw_rect(header_rect, fill=(1, 1, 1))
        page.draw_rect(footer_rect, fill=(1, 1, 1))
    pdf_document.save(output_pdf)
    pdf_document.close()
```

### `extract_covered_text`

**功能**: 提取PDF中被覆盖的页眉页脚区域的文本内容，并保存到文件中。

**算法逻辑**:
1. 打开PDF文件。
2. 遍历每一页，对于每一页:
   - 定义页眉和页脚区域的边界框。
   - 提取这些区域内的文本。
3. 将所有提取的文本保存到文件中。

**代码示例**:
```python
def extract_covered_text(input_pdf, output_path):
    covered_text = ""
    with pdfplumber.open(input_pdf) as pdf:
        for page in tqdm(pdf.pages):
            header_rects = [fitz.Rect(0, 0, page.width, 70)]
            footer_rects = [fitz.Rect(0, page.height - 70, page.width, page.height)]
            bboxes = header_rects + footer_rects
            for bbox in bboxes:
                text_instances = page.within_bbox(bbox).extract_text()
                covered_text += text_instances + '
'
    with open(output_path, 'w', encoding='utf-8') as f:
        f.write(covered_text)
```

(类似的方式添加其他函数的详细说明...)

### `is_scanned_page`

**功能**: 检查页面是否是扫描页。

**算法逻辑**:
1. 获取页面的文本内容。
2. 如果文本内容非常少（如少于50个字符），则认为页面是扫描页。

**代码示例**:
```python
def is_scanned_page(page):
    text = page.get_text()
    return len(text.strip()) < 50
```

### `analyze_pdf`

**功能**: 分析给定PDF文件是否包含扫描页，并将包含扫描页的文件移动到目标文件夹。

**算法逻辑**:
1. 打开PDF文件。
2. 遍历每一页，检查是否为扫描页。
3. 如果找到扫描页，关闭文件并返回True。
4. 如果没有扫描页，关闭文件并返回False。

**代码示例**:
```python
def analyze_pdf(file_path, destination_folder):
    doc = fitz.open(file_path)
    for page in doc:
        if is_scanned_page(page):
            doc.close()
            return True
    doc.close()
    return False
```

### `move_scanned_pdfs`

**功能**: 将源文件夹中包含扫描页的PDF文件移动到目标文件夹。

**算法逻辑**:
1. 遍历源文件夹中的所有PDF文件。
2. 使用`analyze_pdf`分析每个文件。
3. 如果文件包含扫描页，则将其移动到目标文件夹。

**代码示例**:
```python
def move_scanned_pdfs(source_folder, destination_folder):
    if not os.path.exists(destination_folder):
        os.makedirs(destination_folder)
    for root, dirs, files in os.walk(source_folder):
        for file in files:
            if file.lower().endswith('.pdf'):
                file_path = os.path.join(root, file)
                if analyze_pdf(file_path, destination_folder):
                    shutil.move(file_path, os.path.join(destination_folder, file))
```
