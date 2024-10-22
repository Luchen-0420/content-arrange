# 一、课程简介

一键内容收藏至飞书表格。

**本课程将会学到什么**

1. coze工作流搭建
2. 自定义插件调用internlm2.5-20b-0719模型
3. 如何在bot内100%调用工作流
4. 如何在飞书机器人/微信订阅号/公众号中使用
5. 飞书机器人的一些补充

# 二、准备工作

打开[coze国内站点](https://www.coze.cn/)

## 1. 选择性开通coze专业版

由于后续我们需要调试bot，bot api一天限制100次，是不够用的，可以选择性开通专业版。

![coze首页](https://github.com/user-attachments/assets/7c818ef8-77f6-47f6-91a4-feeb12f853af)

![专业版购买入口](https://github.com/user-attachments/assets/a0d8719a-b942-4ecb-b34c-067db23557b1)

**注意，超出后是自动按量计费，后付款的**

![购买页面](https://github.com/user-attachments/assets/55376976-a0fe-4eb9-87a2-5fc3a898af01)

**如果已经在基础版做了，想再开通专业版，请看本段**

再次点击"图标"将基础版的资源复制过来。

![复制1](https://github.com/user-attachments/assets/bf928a62-d283-4ded-bf5e-042daf2f0b77)

![复制2](https://github.com/user-attachments/assets/1912c1f8-31c8-466e-a87e-b82991f7890b)

基础版用户名如下图所示：

![用户名](https://github.com/user-attachments/assets/b1811167-9cb2-4882-b31f-dae5434e9ac7)

复制授权链接后，先退出专业版，登录基础版后，黏贴链接进行授权。

![授权1](https://github.com/user-attachments/assets/333bbdc7-5a29-4454-8790-1fb8730743d4)

![授权2](https://github.com/user-attachments/assets/699d85ed-8da0-4b14-852e-2528284ec14f)

授权完成后，退出基础版，重新登录专业版。点开"工作空间"，可以看到基础版的资源已经在空间内了。

![复制成功](https://github.com/user-attachments/assets/09ad88cf-c2e5-4ba0-976a-2741155d6069)

## 2. 新建飞书多维表格

可以根据自己的需求，进行修改。以下仅个人在设计字段时的一些考虑元素。

### 2.1 表格字段名设置前思考为什么要收藏这篇文章

① 属于什么领域，属于什么性质（扫盲、纯技术、实操....），什么阶段（初级、中级、进阶....）等等......

② 文章传递的什么，如：解决什么问题？提出什么观点？介绍什么新技术？让你觉得值得细看的点在于？等等......

③ 别人推荐的，但是现在没空细看，先收录一下

### 2.2 表格设计

| 字段名     | 飞书字段类型 | 字段说明                                                                 |
|------------|---------------|--------------------------------------------------------------------------|
| 链接       | 文本        | 分享的链接，点击即可跳转阅读                                               |
| 标题       | 文本        | 标题                                                                     |
| 摘要       | 文本        | 通篇大概讲了什么，可以由jina-reader插件自动生成                               |
| 来源       | 单选          | 来自哪个平台，如：b站、小红书、网站等等。后续如果扩展做数据统计时可作为一项分类指标   |
| 形式       | 单选         | 图文、视频                                                                |
| tag        | 多选         | 按领域、功能等打上tag，也可以称它为某个合集                                |
| 状态       | 单选         | 已阅读、未阅读                                                             |
| 收集日期   | 文本        | 写入表格日期，因为飞书时间格式和插件时间格式有出入，我太懒了不想做处理，直接设置文本型 |
| 收藏原因   | 文本        | 为什么要收录这篇                                                          |
| 读后感     | 文本        | 读后感                                                                   |
| 价值等级   | 单选         | 这篇文章含金量怎么样，可自己设置值，比如：低、中、高                          |

### 2.3 新建多维表格注意事项

表格链接在浏览器中一定**不能是wiki**，所以**个人用户**直接从"多维表格"新建。**企业用户**在云文档、多维表格新建都可以。

![个人用户新建位置](https://github.com/user-attachments/assets/526eccee-180c-4694-b694-f36e7ff476d5)

![base](https://github.com/user-attachments/assets/6b314a00-7cd1-43c6-8cc5-2bef3369eb65)

新建后空表如图所示：

![空表示例](https://github.com/user-attachments/assets/f92df3a9-7c25-4514-b36a-bd5793fec254)

## 3. 多平台分享链接对比

首先设定收藏的文章形式分为：图文（纯文包含图文内）、视频。

收藏时想要爬虫来写概要，选择jina-reader插件来测试下以上分类，发现视频类进行爬取其实是不太符合想要的，所以设定```视频类只取标题，对于纯链接的再根据reason来生成标题```。

**另外注意，知乎链接是无法用jina-reader爬取的，本教程不处理知乎类链接**

仔细看各个平台的分享链接形式，有些平台分享链接需要做一个提取```https链接```处理：

```小红书分享链接```：

```
93 【大连｜4天21顿❣️人少嘎嘎好吃，本地人推荐‼️ - 马溜达 | 小红书 - 你的生活指南】 😆 lR3gT9A0eAQ5XWn 😆 https://www.xiaohongshu.com/discovery/item/667aacb8000000001e010eb2?source=webshare&xsec_token=AB1e31D0BGnaywnmwX7C37dGKIbrOsxGltreE-BRB1aeE=&xsec_source=pc_share
```


```抖音分享链接```：

```
9.99 Eus:/ 12/21 J@i.Ch 大连千万级博主总结的大连旅游注意事项，看这一个就足够足够了 # 大连旅游攻略 # 大连 # 大连美食   https://v.douyin.com/iksNQqte/ 复制此链接，打开Dou音搜索，直接观看视频！
```

```公众号文章分享链接```：

```
https://mp.weixin.qq.com/s/d8mvSU77l_xAHKo3gqyPGQ
```

```B站分享链接```：

```
【SOLIDWORKS 教学 精品教程 | 2024年新修 | B站 点赞 播放 收藏 NO.1】 https://www.bilibili.com/video/BV1iw411Z7HZ/?share_source=copy_web&vd_source=93b749d0faa4f6da4b3a9891caf73e4d
```

```youtube分享链接```：

```
https://youtu.be/siHNmXLZ8y4?si=2KwcS5wElJBK4Eva
```

## 4. 自定义InternLM插件

### 4.1 新建插件

![新建1](https://github.com/user-attachments/assets/1bf93d10-9e8a-4fc4-adca-b50a5115c90a)

![新建2](https://github.com/user-attachments/assets/24f95184-978c-4dc9-a141-6e63af572a71)

![新建3](https://github.com/user-attachments/assets/5f30e4a1-e1f2-46e9-8b2f-a01df1a4235d)

![新建4](https://github.com/user-attachments/assets/5f5adb8d-a8c1-46b2-ae7b-48331183c7f7)

### 4.2 配置元数据

仔细阅读[书生浦语的API文档](https://internlm.intern-ai.org.cn/api/document)。其中模型、请求地址、api_key可以设定为固定值。我们设置输入参数为prompt和user_request。输出参数为answer。

![配置元数据图](https://github.com/user-attachments/assets/59541c0b-90b8-4214-8492-71d30997d267)

### 4.3 获取API_KEY

点击[书生·浦语API TOKENS](https://internlm.intern-ai.org.cn/api/tokens)，新建api_token。

![新建token](https://github.com/user-attachments/assets/e24e7890-4980-4a96-bdbe-ef550f595cc0)

### 4.4 代码请求

修改第14行为你自己的API_TOKEN。

```
import requests
from typing import TypedDict, Optional

# 入参
class Input(TypedDict):
    prompt: str
    user_request: str

# 输出
class Output(TypedDict):
    answer: str

# 固定的API配置
API_KEY = ""  # 固定API密钥
API_URL = "https://internlm-chat.intern-ai.org.cn/puyu/api/v1/chat/completions"  # 固定请求地址

def handler(args) -> Output:
    # 获取输入变量
    prompt = args.input.prompt
    user_request = args.input.user_request

    # 调用merge_prompt函数合并提示词和用户要求
    full_prompt = merge_prompt(prompt, user_request)

    # 调用ask_question函数获取answer
    answer = ask_question(full_prompt)
    return {"answer": answer if answer else ""}

def merge_prompt(prompt: str, user_request: str) -> str:
    """提示词和用户需求拼接处理函数"""
    # 拼接提示词和用户需求
    full_prompt = f'{prompt}, 【用户提供的描述】 - {user_request}'
    return full_prompt

def ask_question(question: str) -> Optional[str]:
    try:
        headers = {
            "Content-Type": "application/json",
            "Authorization": f"Bearer {API_KEY}"
        }

        data = {
            "model": "internlm2.5-latest",
            "messages": [{
                "role": "user",
                "content": question
            }],
            "n": 1,
            "temperature": 0.8,
            "top_p": 0.9
        }
        response = requests.post(API_URL, headers=headers, json=data)
        message = response.json()

        if 'error' not in message:
            return message['choices'][0]['message']['content']
        else:
            print("无法获取模型回答:", message.get('error', 'Unknown error'))
            return None

    except requests.exceptions.RequestException as e:
        print(f"请求错误：{e}")
        return None
    except Exception as e:
        print(f"发生错误: {str(e)}")
        return None
```

### 4.4 测试

测试之前先在左下侧安装requests的包。

![安装依赖包](https://github.com/user-attachments/assets/f5f5aeac-ec39-4d58-91f2-2a43f4b40d5d)

![测试结果](https://github.com/user-attachments/assets/42cd86b5-f77d-4505-9257-a6ce439b231e)

### 4.5 发布

无论是工作流还是插件，想在其他地方调用都需要先发布才行。

![发布](https://github.com/user-attachments/assets/5837d7ef-b7ae-40a1-aba7-cdb933f6f7bb)

看到有个绿色的勾，就代表发布成功了。

![发布成功](https://github.com/user-attachments/assets/c658d88a-0424-43c5-a4a9-693a68d1eb40)

# 三、收藏至飞书表格工作流搭建

## 1. 工作流整体流程图

**注意：多分支相同处理时，节点命名要区别好，这样在合并分支的时候选择参数更直观一点。**

![整体图](https://github.com/user-attachments/assets/67a0f5f6-e5d4-4fd8-9acf-0b8b2390e8f9)

## 2. 新建content_arrange工作流

![新建页面](https://github.com/user-attachments/assets/0ebc4e38-ef76-4a07-a785-e3d2a0e655cf)

根据弹窗，一步步填写完成后就能看到图下页面。

![新建成功](https://github.com/user-attachments/assets/1e83d89d-8f99-40a9-9271-c9e8073f437e)

## 3. 定义输入变量

我们先来看表格，对字段做一些归类，其中链接、形式、tag和收藏原因是需要我们自己手动填写的。

![表格元素](https://github.com/user-attachments/assets/60c40fad-3129-4aa7-81ca-61fc24f0642d)

在开始节点新增上述4个变量，除此之外，我们还需要一个标识，来判断是否要对分享链接进行处理。这里我们设置它的名称为handle，类型为boolean。

![输入变量](https://github.com/user-attachments/assets/6d3ebce1-69f6-4b40-bfad-8cfae29fe880)

## 4. 添加日期插件

无论是哪种形式的分享内容，都需要记录收集日期，所以它作为一个共用变量，放在开始节点后面。

点击左侧```插件```节点，在插件市场搜索"时间"，点击添加。

![添加日期插件](https://github.com/user-attachments/assets/3c406059-cf70-4603-9e47-d8150d75efe9)

将它与开始节点连线。

![连线](https://github.com/user-attachments/assets/6f36e9bf-48be-4f6c-a3ef-e81422df4f88)

## 5. if选择器判断内容形式

根据我们的设定，图文类是可以爬取，并做大模型生成摘要的，但是视频类是不做爬取的。所以在开始处理流程前，先进行一次```形式```判断。

点击左侧“if选择器”，添加，并于日期插件连线。

![添加if选择器](https://github.com/user-attachments/assets/7aafa4d1-9f76-41e5-bbd4-d8f626a8c097)

我们先做图文类处理。如果modality等于图文，走第一个分支处理，否则（modality等于视频）走剩下的分支处理。

这里其实不太严谨的，这里偷个懒，我们自己输入的时候严格一点，只输入：图文 or 视频。严谨点应该是if modality == "图文" ... , else if modality == "视频" ... ,else ....

![if选择器](https://github.com/user-attachments/assets/9a8c44b3-ab70-4147-884b-ad1ec1e10004)

## 6. 图文类

### 6.1 图文类-代码节点-提取链接处理

这里我偷个懒**不用if选择器**，所有的链接输入进来后统一进行```提取有效url地址```操作。

添加代码节点，并与if选择器第一个分支进行连线。

![连线](https://github.com/user-attachments/assets/dd37fe62-56e3-499f-baf8-e2425eb579ff)

#### 6.1.1 对输入输出进行设置

![输入输出设置](https://github.com/user-attachments/assets/7fcca214-4033-4fef-94e2-4b13b8f03946)

#### 6.1.2 python代码对链接进行处理

1. 查找 URL 起始位置：使用 find() 方法查找字符串中 http:// 或 https:// 的起始位置。如果找到其中一个，就记录这个位置作为 URL 的起始位置。如果没有找到任何有效的 URL 起始位置，返回 "没有找到有效url"。
2. 查找 URL 结束位置：初始化一个变量 end 为 URL 的起始位置。使用 while 循环遍历字符串，逐个检查字符，继续向后移动 end 位置，直到遇到空格、换行符、逗号、句号或其他标志着 URL 的结束标点符号。
3. 截取 URL：使用确定的起始和结束位置，从输入字符串中截取 URL。去掉可能存在的前后空白。
   
```
async def main(args):
    params = args['params']
    share_link = params['input']

    # 查找 "http://" 或 "https://" 的起始位置
    start = share_link.find("http://")
    if start == -1:
        start = share_link.find("https://")
    
    if start != -1:
        # 查找 URL 末尾的空格、换行符、逗号、句号等作为终止位置
        end = start  # 初始化结束位置为起始位置
        while end < len(share_link) and share_link[end] not in [" ", "\n", ",", "。", "，", "？"]:
            end += 1

        # 截取 URL
        url = share_link[start:end].strip()
    else:
        url = "没有找到有效链接"
    
    return {"url": url}
```

#### 6.1.3 测试提取url节点

测试代码节点有2种方式，一种是代码编辑器测试，一种是单个节点测试。

![节点测试方法](https://github.com/user-attachments/assets/233dcf3d-9e8c-4bac-b474-54f587f2127e)

这里我们用一个小红书图文链接进行测试。**注意，同一链接请求次数多了、频繁了，在插件爬取时会返回滑块验证，所以大家自己准备一个测试链接**

```
46 【大连｜4天21顿❣️人少嘎嘎好吃，本地人推荐‼️ - 马溜达 | 小红书 - 你的生活指南】 😆 S0TF6EsOSlaAJh4 😆 https://www.xiaohongshu.com/discovery/item/667aacb8000000001e010eb2?source=webshare&xhsshare=pc_web&xsec_token=AB1e31D0BGnaywnmwX7C37dGKIbrOsxGltreE-BRB1aeE=&xsec_source=pc_share
```

![提取url](https://github.com/user-attachments/assets/910ebc34-9a77-42c6-9673-8b88d008d6d2)

### 6.2 图文类-插件市场jina-reader插件-爬取图文内容

#### 6.2.1 添加jina-reader插件

插件市场搜索"jina-reader"插件，添加并使用。

![添加插件](https://github.com/user-attachments/assets/ab9ca722-5c10-4385-87da-dae16bf15b49)

![连线](https://github.com/user-attachments/assets/fc36513c-e419-4315-b946-56bcda0d801a)

#### 6.2.2 测试节点

刚刚我们上个节点提取的链接，拿来测试。

```
https://www.xiaohongshu.com/discovery/item/667aacb8000000001e010eb2?source=webshare&xhsshare=pc_web&xsec_token=AB1e31D0BGnaywnmwX7C37dGKIbrOsxGltreE-BRB1aeE=&xsec_source=pc_share
```

测试结果如下：

![测试结果1](https://github.com/user-attachments/assets/3c02c206-d89d-4919-9371-afa8c80d4f1e)

![测试结果2](https://github.com/user-attachments/assets/3e73c3ba-d497-4929-abfa-36cf120c5c68)

### 6.3 图文类-代码节点-处理爬取内容

大模型设置输入输出长度是有一定的限制的，爬取的内容太长作为输入会出现报错，我们要先对内容做一个处理。

从上面的截图我们可以看出2个信息：

1. 返回的content是markdown形式
2. 返回参数3个：content、title、url

接下来我们处理内容主要有以下

1. 去除markdown格式（包括超链接、图片、符号等）
2. 合并换行符处理、去除首尾空白字符、去除连续的横线与等号
3. 输出原始字符数和去除后的字符是，看下对比
4. 截取前3000字符（也可以少放点）
5. 添加提取的 URL 和title 到本节点返回结果中。（这步是因为下一步我们需要调用自定义的插件，只能输入1个参数，所以把它们包在一起。）

#### 6.3.1 配置输入输出

我们输入的是上个节点返回的data，输出变量名output，因为要拿这个output值传递给自定义的模型插件，所以这里设置string类型，到时候包裹的参数做json处理输出。如下图所示：

![配置输入输出](https://github.com/user-attachments/assets/fdca405e-354f-465a-8b75-5beeca46c5bc)

#### 6.3.2 代码及测试

```
import re
from typing import List, Tuple, Any
import json

async def main(args: Any) -> str:
    # 直接使用 args["params"] 获取参数
    params = args["params"]
    
    # 初始化返回值为一个数组
    ret = []

    # 1. 获取输入参数，并确保是字符串
    raw_content = str(params["input"]["content"])
    title = str(params["input"]["title"])
    url = str(params["input"]["url"])

    # 统计原始字符数量
    original_char_count = len(raw_content)

    # 2. 使用正则表达式处理 Markdown 超链接和图片部分
    def markdown_to_text(md: str) -> Tuple[str, List[str]]:
        # 提取 URL
        urls = re.findall(r'\[.*?\]\((.*?)\)', md)
        # 去掉链接部分，保留链接文本
        md = re.sub(r'\[([^\]]+)\]\([^\)]+\)', r'\1', md)
        # 去掉图片链接
        md = re.sub(r'!\[.*?\]\(.*?\)', '', md)
        # 去掉 Markdown 的格式字符
        md = re.sub(r'[#*`-]', '', md)
        # 去掉多余的换行符
        md = re.sub(r'\n+', '\n', md)
        return md.strip(), urls

    # 转换 Markdown 到普通文本
    plain_text, extracted_urls = markdown_to_text(raw_content)

    # 3. 去除多余的横线、等号，并处理换行符和空白字符
    stripped_content = re.sub(r'-{2,}', '', plain_text)
    stripped_content = re.sub(r'={2,}', '', stripped_content)
    stripped_content = re.sub(r'\n+', '\n', stripped_content).strip()

    # 4. 计算去除 Markdown 后的字符数量
    clean_char_count = len(stripped_content)

    # 5. 如果内容超过 3000 字符，则截取前 3000 字符
    stripped_content = stripped_content[:3000]

    # 6. 添加处理后的结果到返回值列表
    ret.append({
        "original_char_count": original_char_count,
        "clean_char_count": clean_char_count,
        "plain_text": stripped_content,
        "url": url,
        "title": title
    })

    # 7. 将返回值转换为 JSON 字符串格式，并作为最终输出
    return json.dumps(ret, ensure_ascii=False)
```

测试数据，你可以整个工作流开始到结束全部连线测试，也可以单个节点点击自动生成数据来测试或者测试上个节点，复制备份出输出用来下个节点测试。

![自动生成](https://github.com/user-attachments/assets/cd553e29-8adf-466a-9182-c6bf3b352717)

测试结果如下，可以看出减少了500多字符。

![测试结果](https://github.com/user-attachments/assets/5bb2e73e-bde4-4084-821a-4fa4c34ed640)

### 6.4 图文类-自定义插件-对内容、标题、平台做提炼

#### 6.4.1 添加自定义插件并配置

添加我们发布的internlm_api的插件。团队空间就是团队工具，个人空间就是个人工具。

![添加自定义插件](https://github.com/user-attachments/assets/6d548626-623e-486c-bc4d-d8a7ca7ea938)

选择"输入"prompt，如图所示：

![设置图](https://github.com/user-attachments/assets/2f2189e1-2ba9-4952-8335-f4b6950a9140)

```
# 任务\n
根据用户输入，生成对应信息\n
# 输出格式要求\n
直接输出原始JSON格式，不要使用任何包装或代码块。严格按照以下格式输出，不添加任何其他内容：\n
{\n
\"summary\": \"在这里填写内容摘要\",\n
\"siteName\": \"在这里填写平台名称\"\n
}\n
# 输出内容要求\n
summary：根据用户输入的plain_text，捕捉内容主题、阅读价值，生成一段简洁而全面的摘要。不要使用\"摘要：\"或类似前缀。\n
\n
siteName：仅回答URL归属的平台名称，不需要额外解释。使用最常见、最正式的名称。具体要求：\n
- 使用平台的官方中文名称（如果有）\n
- 避免使用缩写或非正式的别称\n
- 保持名称的一致性，每次对同一平台使用相同的名称\n\n
# 重要提醒\n
- 不要使用json或任何其他代码块标记\n
- 不要将输出包装在\"answer\"或任何其他字段中\n
- 确保输出的是可以直接被解析的原始JSON\n
- 除了指定的JSON结构外，不要输出任何其他内容
```

#### 6.4.2 测试

测试结果里我们想要的summary和sitename都被包裹在一起，接下来我们要新增一个代码节点把它们提取出来。

![测试结果](https://github.com/user-attachments/assets/223efb4f-791f-4a81-bbc3-719d21abcc2a)

### 6.5 图文类-代码节点-对模型返回结果提取参数

#### 6.5.1 添加代码节点并配置

![配置图](https://github.com/user-attachments/assets/c1ba0545-7b8c-42ae-9710-10df1eacf038)

#### 6.5.2 代码

测试大模型节点几轮发现，有时候它返回结果里是有代码块标记，所以在代码里增加去除代码块标记处理。

```
import json

async def main(args: Args) -> Output:
    # 从输入中提取文本
    text = args.params.get("input", "")
    
    # 去除不必要的转义字符
    text = text.replace('\\"', '"').replace('\\n', '\n')

    # 如果有代码块标记，去除它们
    if "```json" in text:
        cleaned_text = text.replace("```json\n", "").replace("\n```", "").strip()
    else:
        cleaned_text = text.strip()

    # 解析JSON
    try:
        data = json.loads(cleaned_text)
        summary = data.get("summary", "")
        site_name = data.get("siteName", "")
    except json.JSONDecodeError:
        summary = ""
        site_name = ""

    ret: Output = {
        "summary": summary,
        "sitename": site_name
    }
    return ret
```

#### 6.5.3 测试

![测试结果](https://github.com/user-attachments/assets/6f37ffa8-2d08-428f-8b30-5c2ca3ad0b41)

### 6.6 飞书多维表格add_record插件-信息写入飞书

#### 6.6.1 参数准备

还是先来看我们的表格元素分类图。

![表格元素](https://github.com/user-attachments/assets/60c40fad-3129-4aa7-81ca-61fc24f0642d)

所有需要的参数都已经准备好了，下面准备写入飞书表格。

- [x] 链接：开始节点
- [x] 形式：开始节点
- [x] tag：开始节点
- [x] 收藏原因：开始节点
- [x] 标题：jina-reader插件提取
- [x] 摘要：大模型提炼生成
- [x] 平台：大模型提取生成
- [x] 收集日期：插件获取
- [x] 状态：固定参数

#### 6.6.2 新建飞书多维表格写入插件

从插件市场添加写入插件

![添加插件](https://github.com/user-attachments/assets/6b86c4d4-140f-4f94-81cb-d94b2ea6914b)

它有2个必填参数：app_token和records。

```app_token :```多维表格的唯一标识符，支持输入文档url。

```records :```格式为：

```
[
    {
      "fields": "{\"文本\":\"文本内容\",\"单选\":\"选项 1\",\"日期\":1674206443000}"
    }
]
```

![插件图](https://github.com/user-attachments/assets/b163d138-374f-4ffc-b086-8b3a713e3ba7)

#### 6.6.3 代码节点-json序列化处理输入

现在我们要将所有表格需要的参数处理成records需要的格式。

新建代码节点，与上个节点连线，配置输入输出参数，如图所示：

![配置输入输出](https://github.com/user-attachments/assets/99ae8870-2890-4d4f-b501-954967c4610f)

```JavaScript代码```

```
async function main({ params }: Args): Promise<Output> { 
    const fieldsMap = {
        "标题": params.title,
        "链接": params.url,
        "摘要": params.summary,
        "来源": params.sitename,
        "形式": params.modality,
        "tag": params.tag.split(/[，、,]/),
        "状态": "未阅读",
        "收集日期": params.date,
        "收藏原因": params.reason,
    };

    const fields = Object.entries(fieldsMap).reduce((acc, [key, value]) => {
        acc[key] = value;
        return acc;
    }, {});

    const ret = {
        output: [
            { fields }
        ]
    };

    return ret;
}
```

测试结果如下：

![测试结果](https://github.com/user-attachments/assets/ad3d5f7e-17ad-4a6e-88f5-1bc663427f62)

#### 6.6.4 连接飞书新增记录节点

复制链接到浏览器打开。

![复制链接](https://github.com/user-attachments/assets/4ee971a3-90e1-4632-937c-1bfe23568cc9)

参数如图所示。

![参数图](https://github.com/user-attachments/assets/9f5fb05f-32d7-41cc-a854-c3de8977fd80)

配置add_records节点，并连接结束节点，选择add_records节点返回msg作为输出。

![add_record节点](https://github.com/user-attachments/assets/0a1c1f34-9ac1-40ca-a648-18a0373e6dac)

![image](https://github.com/user-attachments/assets/0af9d689-2fe7-42c2-a1b0-dfbceff4b23c)

### 6.7 图文类-整体测试

经过前面的搭建，我们对图文类的处理基本完成。整体图如图所示：

![整体图](https://github.com/user-attachments/assets/c592c63c-d28a-43bf-9d57-fa8dbf4e2e67)

准备一个链接，点击右上角的试运行来测试下！！！

![开始节点输入](https://github.com/user-attachments/assets/2a25bc97-769f-4d1a-bba8-c23e3389d8b9)

第一次的时候会要求飞书对coze授权，复制信息到浏览器进行授权。

![授权报错](https://github.com/user-attachments/assets/91db0967-dfa9-444f-9d43-8b31a7f67b59)

![授权](https://github.com/user-attachments/assets/2b31330b-eb95-488d-a8ef-5972bb9c6e94)

重新试运行一次。

![入库成功](https://github.com/user-attachments/assets/b14b98cd-a32c-42bd-a793-b3f7f2cc7986)

打开飞书表格，可以看到记录已经进来了。

![飞书表格](https://github.com/user-attachments/assets/590bd3e4-e5e4-4c4c-bda8-df9ffe602403)

为了更直观查看，我们可以添加画册视图。

![添加画册视图](https://github.com/user-attachments/assets/c3740f97-ac5b-448b-af0a-0b9282abb14f)

![画册视图](https://github.com/user-attachments/assets/6ecb756e-11ea-488b-b076-fc6157ac298a)


图文类完成后，我们要对if选择器分支的else（即对视频类进行处理）

## 7. 视频类

首先按开始的设想（不做爬虫，不写摘要），我们设计2种情况：

1. 针对分享链接内包含了标题信息：这类，我们就用代码提取出url,并用大模型节点来对分享链接的文字部分生成title。

2. 针对只有url的：这类，我们就根据reason通过大模型来生成一个简要的title。

### 7.1 if选择器-是否要对链接处理

添加if选择器，并连线上一个if选择器的否则分支。

![if选择器](https://github.com/user-attachments/assets/58a60498-7c79-4174-a830-5f5f15bca226)


### 7.2 需要链接处理分支（handle为true）-代码节点-提取url

#### 7.2.1 代码节点配置

新增代码节点，进行提取。

![配置图](https://github.com/user-attachments/assets/cade8ed0-fc8a-4a16-9e02-380413a8fbe2)

#### 7.2.2 代码

这里针对抖音链接做了额外处理。

```
async def main(args):
    params = args['params']
    share_link = params['input']

    # 查找 "http://" 或 "https://" 的起始位置
    start = share_link.find("http://")
    if start == -1:
        start = share_link.find("https://")
    
    if start != -1:
        # 查找 URL 末尾的空格、换行符、逗号、句号等作为终止位置
        end = start  # 初始化结束位置为起始位置
        while end < len(share_link) and share_link[end] not in [" ", "\n", ",", "。", "，", "？"]:
            end += 1

        # 截取 URL
        url = share_link[start:end].strip()
        
        # 针对抖音短链接，确保以 "/" 结尾
        if "v.douyin.com" in url and not url.endswith("/"):
            last_slash = url.rfind("/")
            url = url[:last_slash + 1] if last_slash != -1 else url
    else:
        url = "没有找到有效链接"
    
    return {"url": url}
```

#### 7.2.3 测试

![b站](https://github.com/user-attachments/assets/cbaa406d-c667-49c4-94a2-5b7b823e8a07)

![抖音](https://github.com/user-attachments/assets/af3d7105-baae-4fa1-bba3-b440cac83244)

### 7.3 需要链接处理分支（handle为true）-自定义插件-对标题、平台做提炼

#### 7.3.1 插件设置

![设置](https://github.com/user-attachments/assets/07df019a-bc77-4ed7-a5e0-14953495f675)

#### 7.3.2 prompt

```
# 任务 \n
根据用户输入， 提取出最能概括内容主题的句子或短语和url归属平台。 \n
# 输出格式要求 \n
直接输出原始JSON格式，不要使用任何包装或代码块。严格按照以下格式输出，不添加任何其他内容： \n
{ "title": "在这里填写标题", "siteName": "在这里填写平台名称" } \n
# 输出内容要求 \n
title：根据用户输入的文字部分，捕捉内容的核心主题，应该简洁明了，能够概括整体内容。不要使用"标题："等前缀 \n
siteName：仅回答URL归属的平台名称，不需要额外解释,不要使用"平台:"等前缀。使用最常见、最正式的名称。具体要求： \n
- 使用平台的官方中文名称（如果有） \n
- 避免使用缩写或非正式的别称 \n
- 保持名称的一致性，每次对同一平台使用相同的名称 \n
# 重要提醒 \n
- 直接输出内容，不要加```json或任何其他代码块标记 \n
- 不要将输出包装在任何其他字段中 \n
- 确保输出的是可以直接被解析的原始JSON \n
- 除了指定的JSON结构外，不要输出任何其他内容
```

#### 7.3.3 测试

![测试结果](https://github.com/user-attachments/assets/c8101a8f-b627-4a4e-be67-0be513524b1f)

### 7.4 需要链接处理分支（handle为true）-代码节点-对大模型返回结果做参数提取

#### 7.4.1 节点设置

![节点设置](https://github.com/user-attachments/assets/36317b2f-b87b-45e1-af5b-c32ed0be1fef)

#### 7.4.2 代码

```
import json
async def main(args: Args) -> Output:
    text = args.params.get("input", "")

    # 如果有代码块标记，去除它们
    if "```json" in text:
        cleaned_text = text.replace("```json\n", "").replace("\n```", "").strip()
    else:
        cleaned_text = text.strip()


    # 尝试解析JSON
    try:
        data = json.loads(cleaned_text)
        title = data.get("title", "")
        site_name = data.get("siteName", "")
    except json.JSONDecodeError:
        title = ""
        site_name = ""

    ret: Output = {
        "title": title,
        "sitename": site_name
    }
    return ret
```

#### 7.4.3 测试

![测试结果](https://github.com/user-attachments/assets/41e392b1-d272-4208-828f-d10eec91cdb7)

截止这一步，我们对**需要链接处理分支（handle为true）** 的处理已经完成，下面对handle为false的情况来处理。


### 7.5 不需要链接处理分支（handle为false）-文本处理节点-参数拼接

这类，我们要根据reason通过大模型来生成一个简要的title，与此同时判断平台一并让大模型处理。大模型只能接收一个参数，所以我们要对reason和url进行一个参数拼接后再传入。

添加文本处理节点，并配置，如图所示：

![配置](https://github.com/user-attachments/assets/08b161e2-aa6e-465f-9a5a-3e7e88acda5f)

测试结果：

![测试结果](https://github.com/user-attachments/assets/aba087d8-72e8-4726-955d-c78123368409)

### 7.6 不需要链接处理分支（handle为false）-自定义插件-对标题、平台做提炼

#### 7.6.1 插件设置

![配置图](https://github.com/user-attachments/assets/a1ed441f-9810-4276-830f-e80e5de314f3)

#### 7.6.2 prompt

```
# 任务 \n
根据用户输入， 提取出最能概括内容主题的句子或短语和url归属平台。 \n
# 输出格式要求 \n
直接输出原始JSON格式，不要使用任何包装或代码块。严格按照以下格式输出，不添加任何其他内容： \n
{ "title": "在这里填写标题", "siteName": "在这里填写平台名称" } \n
# 输出内容要求 \n
title：根据用户输入的reason，捕捉内容的核心主题，应该简洁明了，能够概括整体内容。不要使用"标题："等前缀 \n
siteName：仅回答用户输入的url归属的平台名称，不需要额外解释,不要使用"平台:"等前缀。使用最常见、最正式的名称。具体要求： \n
- 使用平台的官方中文名称（如果有） \n
- 避免使用缩写或非正式的别称 \n
- 保持名称的一致性，每次对同一平台使用相同的名称 \n
# 重要提醒 \n
- 直接输出内容，不要加```json或任何其他代码块标记 \n
- 不要将输出包装在任何其他字段中 \n
- 确保输出的是可以直接被解析的原始JSON \n
- 除了指定的JSON结构外，不要输出任何其他内容
```

#### 7.6.3 测试

![测试结果](https://github.com/user-attachments/assets/5a353a13-05d5-469c-968d-d3b1818679aa)

### 7.7 需要链接处理分支（handle为false）-代码节点-对大模型返回结果做参数提取

直接复制7.4的代码处理节点，并引用模型返回结果。

测试结果如下:

![测试结果](https://github.com/user-attachments/assets/2c6a4c7c-ea10-462b-86ec-c96d768572c2)

### 7.8 视频类-两个分支合并处理

#### 7.8.1 参数整理

我们整理下目前已有哪些：

- [x] 链接：处理节点or开始节点
- [x] 形式：开始节点
- [x] tag：开始节点
- [x] 收藏原因：开始节点
- [x] 标题：提取or模型根据reason生成
- [x] 摘要：不需要
- [x] 平台：大模型提取生成
- [x] 收集日期：插件获取
- [x] 状态：固定参数

#### 7.8.2 视频类-代码节点-合并输出处理

由于我们是2个分支合并判断做一个输出，看下图分支处理上，涉及到的参数有：url、title、sitename。所以我们要对它们3个进行判断并输出。

![分支图](https://github.com/user-attachments/assets/a511048a-a69d-4176-9adb-8d877b55afba)

添加代码节点，并配置，如图所示：

![配置图](https://github.com/user-attachments/assets/f3058bb5-ed1b-41f9-bfd0-283189702afe)

代码：

```
async function main({ params }: Args): Promise<Output> {

    let ret;

    if(params.handle){
        ret = {
            "title": params.t_title || '',
            "sitename": params.t_sitename || '' ,
            "url": params.t_url || ''
        };
    } else {
        ret = {
            "title": params.f_title || '',
            "sitename": params.f_sitename || '',
            "url": params.f_url || ''
        };
    }
    return ret;
}
```

#### 7.8.3 测试结果

![测试结果](https://github.com/user-attachments/assets/0c37c7ed-b962-40a8-bb31-3ba9f61b6670)

### 7.9 视频类-代码节点-json序列化处理输入

现在我们要将所有表格需要的参数处理成records需要的格式。因为和图文类的参数不统一，所以我们需要再做一次json序列化处理。

复制图文类的json处理节点，与上个节点连线，配置输入输出参数，这里注意的是，要删除summary入参，如图所示：

![配置](https://github.com/user-attachments/assets/0c72c2c1-54b0-4a59-82f8-783035aca969)

```JavaScript代码```

```
async function main({ params }: Args): Promise<Output> { 
    const fieldsMap = {
        "标题": params.title,
        "链接": params.url,
        "摘要": "",
        "来源": params.sitename,
        "形式": params.modality,
        "tag": params.tag.split(/[，、,]/),
        "状态": "未阅读",
        "收集日期": params.date,
        "收藏原因": params.reason,
    };

    const fields = Object.entries(fieldsMap).reduce((acc, [key, value]) => {
        acc[key] = value;
        return acc;
    }, {});

    const ret = {
        output: [
            { fields }
        ]
    };

    return ret;
}

```

### 7.10 视频类&图文类合并输出

这里有2种方式：

1. 单独添加写入插件，在结束节点新增参数来查看写入情况。

2. 判断是图文还是视频，再连接图文类已经写好的飞书表格插件。

这里选择第2种方式。

新增代码节点，对输入输出进行配置（），如图所示：

![配置图](https://github.com/user-attachments/assets/7ec7da27-311e-4182-96dd-86afb4cbc254)

```代码```

判断用户传入的类型是图文还是视频来选择我们最终输出的是哪个分支的数据。

```
async function main({ params }: Args): Promise<Output> {
    let ret;

    if (params.modality === "图文") {
        ret = params.pic_json;  
    } else if (params.modality === "视频") {
        ret = params.video_json;  
    } else {
        ret = null;  
    }

    return ret;
}
```

### 7.11 连线

与添加记录节点进行连线。

![连线](https://github.com/user-attachments/assets/0c295ca8-c749-4e9d-8589-aa004043d062)

![连线](https://github.com/user-attachments/assets/2445f878-6fc6-415f-93d1-0af471196c56)

### 7.12 测试

整体图如下所示:

![整体图](https://github.com/user-attachments/assets/67a0f5f6-e5d4-4fd8-9acf-0b8b2390e8f9)

整个流程很快，大概3-10s

![运行完成图](https://github.com/user-attachments/assets/a7b69db0-7f81-45f2-a1ec-5d3d0b34df1a)

看下飞书表格。

![表格视图](https://github.com/user-attachments/assets/ac9a6165-b40c-4046-b4aa-75bd83ccf5a1)

![插件视图](https://github.com/user-attachments/assets/2a033a8d-0777-4b83-b25c-9467ac5c93d6)

**一定要记得发布节点**

# 四、（专业版）火山方舟控制台添加其他模型

现在专业版只提供了3个模型，不太满足我们的需求，我们登录[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/model?vendor=Bytedance&view=LIST_VIEW)

点击“在线推理”，点击“创建推理接入点”，这里我新增了一个doubao-128k的模型。

![在线推理](https://github.com/user-attachments/assets/841ab54b-6a41-4bb1-b6b1-2339eb621a05)

![添加处](https://github.com/user-attachments/assets/1a1e27d7-7959-4746-875d-bed64a9de0bf)

![添加模型](https://github.com/user-attachments/assets/e047675d-e03d-4ab3-bddd-fa7b47ddd9b9)


# 五、使用bot进行内容收藏

## 1. 新增bot

点击"项目开发"，新增一个Bot

![新增bot](https://github.com/user-attachments/assets/c147671c-f82a-412f-9f41-303de9971665)

## 2. bot内添加已发布的工作流

![添加工作流1](https://github.com/user-attachments/assets/0096e61a-3377-4250-898e-ce4aa9f16db0)


## 3. 设置快捷指令(100%调用工作流）

原先我给定的提示词如图所示，即便对Bot的人设与回复逻辑做了层层限制，它还是不会100%按照设定和限制来调用工作流。

![提示词](https://github.com/user-attachments/assets/f2ff682d-8562-4233-9f19-3c638bfd454d)

想做到100%调用工作流。这里我们就要用到快捷指令功能。官方描述是

```
快捷指令是对话输入框上方的按钮，配置完成后，用户可以快速发起预设对话
```

### 3.1 添加内容收藏工作流快捷指令


![添加](https://github.com/user-attachments/assets/766a9ef8-210d-44fb-9ef3-3022bfd73828)

![空白](https://github.com/user-attachments/assets/171c0e89-d4f4-4de8-a52d-1c07aaee16ba)

### 3.2 指令详细设置

我们添加工作流后，下方会提取到开始节点的输入参数，如图所示。

![设置1](https://github.com/user-attachments/assets/eb132326-9e2c-4a51-8cf1-29634960b719)

![设置2](https://github.com/user-attachments/assets/0e8f9021-2cd0-4e50-8859-4be2a00638b4)

右侧是展现给用户的输入框，英文描述有时候没有中文来得直观，我们可以修改左侧的组件设置。如图所示：

![中文设置](https://github.com/user-attachments/assets/d1384a06-2241-4ac5-ac39-9de0cee32a39)

**指令内容**就是我们要发送给工作流的内容。可以点击右上角的按钮插入变量，也可以直接输入`{`快速选择。设置完成后点击确认。

![指令设置](https://github.com/user-attachments/assets/d39268a1-1c49-4b5c-9fbf-83567775a851)

### 3.3 测试

现在可以看到输入框上方有了快捷指令，点击这个快捷指令。

![快捷指令1](https://github.com/user-attachments/assets/0d4c661f-250b-4ded-8a1f-3026c0db489a)

![快捷指令2](https://github.com/user-attachments/assets/1270c4d6-1366-44b1-9fa4-8c4cb1d7ae32)

输入参数并发送，第一次会出现要授权飞书表格的提示，授权后重新运行一边就可以了。这里我做了一个关于图文一个关于视频的测试，可以看到已经添加进飞书表格了。

![授权](https://github.com/user-attachments/assets/7be2b6eb-a3e1-4ecb-837d-586d52792e87)

![测试1](https://github.com/user-attachments/assets/06af87fb-9f45-4be0-bf22-92185e32fa0d)

![测试2](https://github.com/user-attachments/assets/365471ae-cecc-417a-8926-2c52a515ffff)

**更改后一定要发布！！！**

## 4. bot运行期间效果不理想如何排查

如果运行期间输出结果不对，我们可以点开"调试"挨个节点排查。

![调试](https://github.com/user-attachments/assets/256bff85-33b3-4ef6-9e1a-0a760c1b1145)


# 六、发布到飞书机器人+微信订阅号/公众号

## 1. 修改bot提示词

我们在设置快捷指令的时候，设置了一个"指令名称"，指令名称就是方便我们在飞书机器人、公众号等外部平台调用快捷指令的。

![快捷指令](https://github.com/user-attachments/assets/532d130c-32dc-4501-8329-ff1c51fb9bca)

为了让用户更方便输入，我们给外层bot的提示词内加上这些模板代码。

```新提示词：```

```
# 角色与核心职责
你是一位专业的内容收藏与推荐助手管理执行者。你的核心职责是：
1. 调用工作流 {{content_arrange}} 处理所有内容收藏请求
2. 严格遵守以下操作规程，不进行任何其他操作

#技能

## 输出设定模板

1. 当用户输入"模板"，回复以下模板：
```机器人```
/content_arrange
-- url: 
-- genre: 
-- tag: 
-- reason: 
-- handle:
 
```公众号```
/content_arrange
--分享链接 
--形式 
--tag 
--收藏原因 
--链接是否要处理 

# 操作规程
## 1. 错误处理
- 如果工作流调用失败，返回：
抱歉，工作流调用失败，请稍后再试。

## 2. 无效输入处理
对于不符合内容推荐或收藏格式的输入：
回复：
抱歉，请检查输入参数

# 关键原则
1. 使用用户原始输入作为输入
2. 禁止自行生成任何处理收藏请求，所有操作必须通过工作流完成
3. 不执行任何与收藏无关的任务

# 状态管理
- 初始状态：等待用户输入
- 每次完成工作流调用或提供错误反馈后，立即返回初始状态

```

## 2. 授权并发布

授权飞书机器人和订阅号（或者公众号）点击发布后需要**等待审核完成**。

![发布](https://github.com/user-attachments/assets/27bec5d0-fabb-48fb-8057-b8879010a993)

## 3. 飞书机器人测试

点击"工作台"，可以看到我们已经发布的机器人。

![机器人在哪里](https://github.com/user-attachments/assets/0fa12cf1-ee41-4980-a327-48442c94e24b)

![测试结果1](https://github.com/user-attachments/assets/531a1207-f9da-4fc1-b5fd-8c62227cd712)

![测试结果2](https://github.com/user-attachments/assets/fa96a234-27e7-4bfc-89f8-34108b1d6e07)

## 4. 订阅号（公众号）测试

![测试结果1](https://github.com/user-attachments/assets/2b01240b-1a72-4519-b3d9-60aa94051090)

![测试结果2](https://github.com/user-attachments/assets/be9482b7-334e-47c4-a8ed-e0e3d2408d65)

# 七、补充

## 1. 如何删除弃用的飞书应用

登录[飞书管理后台](https://feishu.cn/admin),点击"应用管理"，筛选开发者为企业内成员（个人账号就筛选个人）。可以看到我们已经创建的应用。

![应用列表](https://github.com/user-attachments/assets/088a1de8-df6a-47cb-b8c5-212f658782ef)

点击"配置"，停用应用。

![停用](https://github.com/user-attachments/assets/1ba3ffec-df43-4f97-83cb-8b5d190518d1)

登录[飞书开发者后台](https://open.feishu.cn/app),点击进入要删除的应用，在"凭证信息"页面下拉，点击删除。

![app罗列页面](https://github.com/user-attachments/assets/69055248-2a0d-4ccc-997b-d3396b7433cb)

![删除应用](https://github.com/user-attachments/assets/75b4906f-c55c-4ced-ba55-99ae95526c80)

## 2. 如何多人使用同一机器人（仅限同一组织）

新建群组，添加群成员和群机器人。以下演示账号为企业账号，个人账号步骤相同。

### 2.1 为应用添加机器人

点击登录[飞书开发者后台](https://open.feishu.cn/app)，选择应用。截图是个人账号添加情况，企业账号无需手动添加。

![个人账号添加](https://github.com/user-attachments/assets/dc7d229a-b344-4f45-853f-5def6ba9b554)

添加完成后需要发布版本。

![发布版本](https://github.com/user-attachments/assets/c922fa9e-e791-4624-94ad-a29a1d77ace7)

### 2.2 新建群组并添加群机器人

打开飞书，点击"通讯录" > "群组" > "新建群组" > 添加"群机器人"

![新建群组](https://github.com/user-attachments/assets/f0e9c6d4-067e-4d51-aee3-52951364c29b)

![新建机器人](https://github.com/user-attachments/assets/85f13caf-78ef-4c19-85df-e88e33dbd24a)

![添加群机器人](https://github.com/user-attachments/assets/3348642c-ad49-4049-946c-4717c94d48a8)

### 2.3 如何使用群机器人

在输入框内@机器人发送信息

![@机器人](https://github.com/user-attachments/assets/aa46eafa-682e-46f8-a55c-c6ad41a5a88b)

![结果](https://github.com/user-attachments/assets/98141cac-071e-41f6-9e15-59d292d1a242)

现在添加一个组织内的成员来切换身份测试下。

![添加成员](https://github.com/user-attachments/assets/34deddc0-f360-4864-a300-b3d7e07b062a)

第一次会要求我们进行一个授权。

![授权](https://github.com/user-attachments/assets/148ea14d-119d-4825-9d33-3a12eec9de34)

![添加成功](https://github.com/user-attachments/assets/616b8d75-c854-42c2-a182-7953f3e9ca30)

我们打开飞书表格看下，2次添加是否成功。

![飞书表格](https://github.com/user-attachments/assets/c3d1956d-1b08-4768-8457-e3a1398a837d)

## 3. 对外共享机器人（不是同一组织的）

场景描述：企业在进行业务沟通时，可能面临跨租户沟通的场景。例如上下游企业的成员沟通、企业与外部客户之间的沟通等。在这些场景中，企业可以开发一个允许对外共享的机器人，利用机器人能力高效完成沟通协作。开启对外共享的机器人支持以下功能：

- 添加在外部群，实现拉人入群、推送消息、响应群成员消息等功能。
- 与外部用户单聊。通过自动化流程实现用户可能面临的产品功能反馈、业务进展通知等需要。

前提必须是：认证过的企业号。

### 3.1 企业认证

登录[飞书管理后台](https://feishu.cn/admin)，点击"企业设置" > "企业信息"进行认证。

企业自建应用的对外共享仅支持认证租户使用。认证时长1-2个工作日（实际大概1-2h左右），可以选择3种方式认证。

![企业认证](https://github.com/user-attachments/assets/1caf3779-67ec-4424-9237-d9f700b97186)

![认证成功](https://github.com/user-attachments/assets/867f4d2d-aca5-4048-a9e2-51f2fee5ab51)

### 3.2 对外共享

认证完成后，登录[飞书开发者后台](https://open.feishu.cn/app)。点击想要共享的机器人，在应用左侧导航栏，选择 应用发布 > 版本管理与发布。

在 "版本管理与发布" 页面右上角，点击 创建版本。

在 "版本详情" 页面选中 "允许机器人被添加到外部群中使用"。

![对外共享](https://github.com/user-attachments/assets/e52cb805-78d7-4af0-ac7f-15037fe3a2c5)

登录[飞书管理后台](https://feishu.cn/admin)，可以看到应用审核已经有待办消息了。

![飞书管理后台](https://github.com/user-attachments/assets/47c8505f-e2f9-4b79-a690-c63f3c85623f)

点击通过后会有消息弹窗。

![成功弹窗](https://github.com/user-attachments/assets/6a8cf20f-aafb-40a4-984e-93f89262151d)

（补充）想免审的话，就选择"设置审核规则"

![设置审核规则](https://github.com/user-attachments/assets/9b51e8b1-44f9-4b1d-b276-e11018b38abe)

### 3.3 对外共享机器人测试

我们新建一个外部群组。

![新建外部群组](https://github.com/user-attachments/assets/22d425b9-cd21-4487-bbf7-c86be21b7ded)

添加群机器人。

![添加群机器人](https://github.com/user-attachments/assets/17c65471-e1b7-43b4-9dff-e6a3e413b096)

切换账号来测试下。依旧是初次使用要授权。

![测试结果](https://github.com/user-attachments/assets/968d942c-1fbb-492d-a55c-620c2953b4fe)

看下飞书表格，内容已经进来了。

![飞书表格](https://github.com/user-attachments/assets/b695eaec-d060-4643-b854-6452eca95d4b)
