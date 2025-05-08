# Image Captioning Tool - 使用说明

## 工具简介

这是一个基于OpenAI API调用212服务端的JoyCaptionV2的图像标注工具，可以帮助您为图像生成文字描述或标签。工具提供三种模式：
1. **标签模式(Tag)** - 生成Booru风格的标签列表，适用于SDXL模型的tag标签
2. **描述模式(Des)** - 生成自然语言描述的图像说明，适用于Flux模型的tag标签
3. **自定义模式(Custom)** - 根据您的自定义提示词生成内容

## 环境要求

- Python 3.6+
- 安装依赖包：`pip install openai`

## 快速开始

### 1. 基本使用方法

将工具放在您的图像文件夹中，然后通过命令行运行：

#### 标签模式（生成Booru风格标签）
```
python lablel_image.py --input "您的图像文件夹路径" --mode tag
```

#### 描述模式（生成自然语言描述）
```
python lablel_image.py --input "您的图像文件夹路径" --mode des
```

#### 自定义模式（使用自定义提示词）
```
python lablel_image.py --input "您的图像文件夹路径" --mode custom --custom_prompt "您想要的提示词"
```

### 2. 参数说明

| 参数 | 说明 | 示例 |
|------|------|------|
| `--input` | **必需** 输入图像文件夹路径 | `--input "C:\images"` |
| `--mode` | **必需** 选择模式：tag/des/custom | `--mode tag` |
| `--custom_prompt` | 自定义提示词（仅custom模式需要） | `--custom_prompt "描述这张图片中的主要元素"` |
| `--api_key` | OpenAI API密钥（默认已设置） | `--api_key "sk-..."` |
| `--base_url` | API基础URL（默认已设置） | `--base_url "http://192.168.5.212:8000/v1"` |
| `--max_retries` | 最大重试次数（默认3） | `--max_retries 5` |

## 使用示例

### 示例1：为文件夹中所有图像生成标签
```
python lablel_image.py --input "D:\动漫图片" --mode tag
```

### 示例2：为图像生成详细描述
```
python lablel_image.py --input "/home/user/pictures" --mode des
```

### 示例3：使用自定义提示词
```
python lablel_image.py --input "E:\dataset" --mode custom --custom_prompt "Write a very short descriptive caption for this image in a casual tone, do not describe the style of image, such as Photograph, cartoon, anime. ONLY describe the most important elements of the image."
```

## 输出结果

- 工具会在每个图像文件旁边生成同名的`.txt`文件
- 例如：`image1.jpg`会生成`image1.txt`
- 处理日志会同时显示在屏幕和`image_captioning.log`文件中

## 注意事项

1. 支持的图像格式：`.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.webp`
2. 确保网络连接正常，能够访问API服务器
3. 大量图像处理可能需要较长时间，请耐心等待
4. 如果处理失败，工具会自动重试（默认最多3次）


## 高级提示词控制

### 1. 字数控制功能

您可以通过在自定义提示词中加入特定占位符来控制输出内容的长度：

#### 基本字数控制
- 使用 `{word_count}` 指定具体字数：
  ```
  --custom_prompt "Write a description within {word_count} words"
  ```
  示例（限制50字）：
  ```
  --custom_prompt "Write a Booru tag list within 50 words"
  ```

#### 相对长度控制
- 使用 `{length}` 指定相对长度（从very short到very long）：
  ```
  --custom_prompt "Write a {length} description"
  ```
  可用长度选项：
  ```
  "very short", "short", "medium-length", "long", "very long"
  ```
  示例（中等长度描述）：
  ```
  --custom_prompt "Write a medium-length descriptive caption"
  ```

#### 预设字数选项
您可以直接使用这些预设字数（单位：字）：
```
20, 30, 40, 50, 60, 70, 80, 90, 100,
110, 120, 130, 140, 150, 160, 170, 180,
190, 200, 210, 220, 230, 240, 250, 260
```
示例（精确到100字）：
```
--custom_prompt "Write a product listing description within 100 words"
```

### 2. 内容控制功能

#### 风格控制
- **正式/非正式语气**：
  ```
  "Write in a formal tone..."
  "Write in a casual tone..."
  ```

- **特定平台风格**：
  ```
  "Write a MidJourney prompt..."
  "Write a Stable Diffusion prompt..."
  "Write a social media post caption..."
  ```

#### 元素排除
禁止描述某些内容：
```
"Do NOT describe the style of image (photograph/cartoon/anime)"
"Do NOT mention image resolution"
"Do NOT include anything sexual; keep it PG"
```

#### 专业分析
艺术评论风格：
```
"Analyze like an art critic (composition, color, symbolism)..."
"Specify depth of field and lighting conditions..."
```

#### 实用商业场景
产品列表描述：
```
"Write as though it were a product listing..."
```

### 3. 组合使用示例

#### 示例1：精确控制的商业描述
```
--custom_prompt "Write a 100-word product listing in formal tone. Do NOT mention image quality. Include lighting and camera angle info."
```

#### 示例2：艺术分析
```
--custom_prompt "Write a long art critic analysis. Discuss composition, color theory and likely artistic movement."
```

#### 示例3：安全的内容标签
```
--custom_prompt "Write a medium-length Booru tag list. Do NOT include style tags. Mark if content is sfw/suggestive."
```

## 完整预设模板参考

您可以直接使用这些经过优化的预设模板：

### 描述类
```
"Write a {length} descriptive caption in {formal/casual} tone"
"Write a descriptive caption within {word_count} words, focusing on key elements only"
```

### 标签类
```
"Write a {length} list of Booru-like tags, excluding style/type tags"
"Generate 50-word Booru tag list with aesthetic quality rating"
```

### 专业类
```
"Analyze this image's composition using rule of thirds, {length}"
"Write a camera spec report (aperture/ISO/shutter speed) in 80 words"
```

### 商业类
```
"Create a {length} social media caption with emojis"
"Write a 120-word product description highlighting key features"
```

## 使用建议

1. **先测试后批量**：先用单张图片测试提示词效果
2. **逐步添加限制**：从简单提示开始，逐步添加控制条件
3. **检查日志**：`image_captioning.log`会记录所有生成内容
4. **组合功能**：可以同时使用字数控制和内容控制

## 注意事项

- 过于复杂的提示词可能导致响应时间增加
- 某些限制条件可能会冲突（如同时要求"very short"和包含大量细节）
- 实际输出字数可能有±10%的波动

通过灵活组合这些控制参数，您可以精确获得符合项目需求的图像描述输出！

## 常见问题

**Q: 处理失败怎么办？**
A: 检查日志文件`image_captioning.log`中的错误信息，确认图像格式是否正确，网络是否通畅

**Q: 如何修改提示词模板？**
A: 可以编辑代码中的`generate_prompt`函数，或使用自定义模式

**Q: 输出文件在哪里？**
A: 默认与原始图像在同一文件夹下，文件名相同但扩展名为`.txt`

如有其他问题，请联系lsy技术支持。