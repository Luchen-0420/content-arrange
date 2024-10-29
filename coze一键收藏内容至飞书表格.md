# 一、课程简介

## 1.教程简介

实现收藏内容至飞书表格。

## 2.本课程将会学到什么

1. coze工作流搭建
2. 自定义插件调用internlm2.5-20b-0719模型
3. 如何在bot内100%调用工作流
4. 如何在飞书机器人/微信订阅号/公众号中使用
5. （进阶教程）循环节点的使用
6. 一些关于飞书的补充

# 二、准备工作

## 1. 开通coze专业版

打开[coze国内站点](https://www.coze.cn/)

由于后续我们需要调试bot，bot api一天限制100次，是不够用的，可以选择性开通专业版。本次课程的团队空间皆为专业版团队空间。

![coze首页](https://github.com/user-attachments/assets/7c818ef8-77f6-47f6-91a4-feeb12f853af)

![专业版购买入口](https://github.com/user-attachments/assets/901a9aba-0ab2-4ddd-99ee-d97fac2ac6d0)

**注意，超出后是自动按量计费，后付款的**

![购买页面](https://github.com/user-attachments/assets/2e322381-a2ac-4bdd-99da-6c9f92aa366f)

**如果已经在基础版做了，想再开通专业版，请看本段**

再次点击"图标"将基础版的资源复制过来。

![复制1](https://github.com/user-attachments/assets/8a5b4c60-876d-45f3-bf83-42bf3b19da66)

![复制2](https://github.com/user-attachments/assets/c9e4eff5-ff55-4936-9779-38b7ebf853c3)

基础版用户名如下图所示：

![基础版用户名](https://github.com/user-attachments/assets/723c3e77-fac5-44de-b598-243d04a47201)

复制授权链接后，先退出专业版，登录基础版后，黏贴链接进行授权。

![授权1](https://github.com/user-attachments/assets/6917927d-c31b-42e3-8eb3-a7d47923f93e)

![授权2](https://github.com/user-attachments/assets/ae94b0c1-6812-4ed8-a0cf-5564ac420297)

授权完成后，退出基础版，重新登录专业版。点开"工作空间"，可以看到基础版的资源已经在空间内了。

![复制成功](https://github.com/user-attachments/assets/a50438ae-b057-468d-9140-e68cad2ccdcf)

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

![个人用户新建位置](https://github.com/user-attachments/assets/e7f228f4-59b6-46b3-a0d4-1e18e157deb8)

![表格注意事项](https://github.com/user-attachments/assets/13ece857-0405-4351-b93b-2f6ec631bf38)

新建后空表如图所示：

![空表示例](https://github.com/user-attachments/assets/82183037-9b8e-45dc-b1b6-af8397aad289)

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

邀请码请私聊小助手 ~

### 4.1 新建插件

![插件新建1](https://github.com/user-attachments/assets/eedfcbd5-3fc8-453c-9cf1-3c1e5e3849d2)

![插件新建2](https://github.com/user-attachments/assets/33241343-2282-4962-8337-6568d2c4128a)

![插件新建3](https://github.com/user-attachments/assets/6fd0ece5-146b-4f37-a7dd-6a3f313395c3)

![插件新建4](https://github.com/user-attachments/assets/52a8d245-82c3-4963-9b5d-b9da30c74452)

### 4.2 配置元数据

仔细阅读[书生浦语的API文档](https://internlm.intern-ai.org.cn/api/document)。其中模型、请求地址、api_key可以设定为固定值。我们设置输入参数为prompt和user_request。输出参数为answer。

![配置元数据图](https://github.com/user-attachments/assets/d1213ed9-7839-4733-bf8d-361f5759089b)

### 4.3 获取API_KEY

点击[书生·浦语API TOKENS](https://internlm.intern-ai.org.cn/api/tokens)，新建api_token。

![新建token](https://github.com/user-attachments/assets/2060ac1c-9029-4917-b371-2b7d421788fa)


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

![安装依赖包](https://github.com/user-attachments/assets/4ef0ba1e-df4e-4148-b574-00e442dc62fd)

![自定义插件测试结果](https://github.com/user-attachments/assets/8b0465a7-5f97-4ee1-a139-2fafeddf8412)

### 4.5 发布

无论是工作流还是插件，想在其他地方调用都需要先发布才行。

![发布插件](https://github.com/user-attachments/assets/5509e184-6e47-4e33-9c50-247bcf788602)

看到有个绿色的勾，就代表发布成功了。

![发布成功](https://github.com/user-attachments/assets/bb0d5172-5332-43f4-a158-84dbc3705d0b)

# 三、收藏至飞书表格工作流

## 1. 工作流整体流程图

**注意：多分支相同功能处理时，节点命名要区别好，这样在合并分支的时候选择参数更直观一点。比如图文类和视频类都需要进行序列化处理，节点名称命名时可以采用 v_xx p_xx 等等**

![整体图](https://github.com/user-attachments/assets/3a333993-c9d2-442f-acdf-858c00fc4ff8)

## 2. 新建content_arrange工作流

![新建工作流](https://github.com/user-attachments/assets/bc965df6-3f21-4aea-a124-3522a449ee7f)

根据弹窗，一步步填写完成后就能看到图下页面。这里分几个功能区域。

![新建成功](https://github.com/user-attachments/assets/1e83d89d-8f99-40a9-9271-c9e8073f437e)

**注意：教程编写的时候coze工作流页面未改版，以下截图与最新有细微差别，不过影响不大，最新版的将会通过视频教程来演示和降解**

## 3. 定义输入变量

我们先来看表格，对字段做一些归类，其中链接、形式、tag和收藏原因是需要我们自己手动填写的。

![表格元素](https://github.com/user-attachments/assets/758c496b-8c15-4187-ba8a-79d74553fb9a)

在开始节点新增上述4个变量，除此之外，我们还需要一个标识，来判断是否要对分享链接进行处理。这里我们设置它的名称为handle，类型为boolean。

![新增变量](https://github.com/user-attachments/assets/9d77f4c2-0a97-478e-a422-0312f2b6c935)

## 4. 添加日期插件

无论是哪种形式的分享内容，都需要记录收集日期，所以它作为一个共用变量，放在开始节点后面。

点击左侧```插件```节点，在插件市场搜索"时间"，点击添加。

![添加日期插件](https://github.com/user-attachments/assets/07d29f4b-47c9-4f4d-8ab4-d5c597c5b799)

将它与开始节点连线。

![连线](https://github.com/user-attachments/assets/456fcd81-a4ad-41ce-9aee-2cbe2b3244e9)

## 5. if选择器判断内容形式

根据我们的设定，图文类是可以爬取，并做大模型生成摘要的，但是视频类是不做爬取的。所以在开始处理流程前，先进行一次```形式```判断。

点击左侧“if选择器”，添加，并于日期插件连线。

![添加If选择器](https://github.com/user-attachments/assets/f3d30839-26cc-4e4a-a355-c6d4548ed144)

我们先做图文类处理。如果modality等于图文，走第一个分支处理，否则（modality等于视频）走剩下的分支处理。

这里其实不太严谨的，主要是偷个懒，我们自己输入的时候严格一点，只输入：图文 or 视频。严谨点应该是if modality == "图文" ... , else if modality == "视频" ... ,else ....

![if选择器配置](https://github.com/user-attachments/assets/69989ab0-dfea-4e7e-bdc1-5d4ee77a7f70)

## 6. 图文类

### 6.1 图文类-代码节点-提取链接处理

这里我偷个懒**不用if选择器**，所有的链接输入进来后统一进行```提取有效url地址```操作。

添加代码节点，并与if选择器第一个分支进行连线。

![连线](https://github.com/user-attachments/assets/8e6e0a6e-7673-48c1-9c76-9e0897779860)

#### 6.1.1 对输入输出进行设置

![输入输出设置](https://github.com/user-attachments/assets/ba6ed391-9839-47e6-8469-e7ab662ed8f4)

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

![节点测试方式](https://github.com/user-attachments/assets/fd6e4622-f314-45a2-a7e1-fa187eb8141f)

这里我们用一个小红书图文链接进行测试。**注意，同一链接请求次数多了、频繁了，在插件爬取时会返回滑块验证，所以大家自己准备一个测试链接**

```
46 【大连｜4天21顿❣️人少嘎嘎好吃，本地人推荐‼️ - 马溜达 | 小红书 - 你的生活指南】 😆 S0TF6EsOSlaAJh4 😆 https://www.xiaohongshu.com/discovery/item/667aacb8000000001e010eb2?source=webshare&xhsshare=pc_web&xsec_token=AB1e31D0BGnaywnmwX7C37dGKIbrOsxGltreE-BRB1aeE=&xsec_source=pc_share
```

![提取代码测试结果](https://github.com/user-attachments/assets/5825ccc2-4189-48b0-bbcf-56eed0cf11e9)

### 6.2 图文类-插件市场jina-reader插件-爬取图文内容

#### 6.2.1 添加jina-reader插件

插件市场搜索"jina-reader"插件，添加并使用。

![添加jinareader插件](https://github.com/user-attachments/assets/455c4530-a128-4a0f-9cfa-636d0cdd590e)

![连线](https://github.com/user-attachments/assets/dd8dac6c-8008-4142-8120-a02df2d1edc9)

#### 6.2.2 测试节点

刚刚我们上个节点提取的链接，拿来测试。

```
https://www.xiaohongshu.com/discovery/item/667aacb8000000001e010eb2?source=webshare&xhsshare=pc_web&xsec_token=AB1e31D0BGnaywnmwX7C37dGKIbrOsxGltreE-BRB1aeE=&xsec_source=pc_share
```

测试结果如下：

![测试结果1](https://github.com/user-attachments/assets/0a5afd3a-24b2-47f9-8d28-18110a1f600e)

![测试结果2](https://github.com/user-attachments/assets/e24c1106-42be-46ad-b169-3d613c80cdc6)

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

![配置输入输出](https://github.com/user-attachments/assets/a61e8d99-f552-41ee-8c31-a6dafdc3e333)

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

![自动生成测试数据](https://github.com/user-attachments/assets/9c01f211-21ee-47d7-9775-97cb633618d7)

测试结果如下，可以看出减少了500多字符。

![测试结果](https://github.com/user-attachments/assets/5bb2e73e-bde4-4084-821a-4fa4c34ed640)

### 6.4 图文类-自定义插件-对内容、标题、平台做提炼

#### 6.4.1 添加自定义插件并配置

添加我们发布的internlm_api的插件。团队空间就是团队工具，个人空间就是个人工具。

![添加自定义插件](https://github.com/user-attachments/assets/30acac2a-caa5-4af8-8d33-032ea3994984)

选择"输入"prompt，如图所示：

![设置图](https://github.com/user-attachments/assets/976da6c1-8df5-4d66-971c-507ffb472792)

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

![测试结果](https://github.com/user-attachments/assets/697a9d87-403f-4cbd-b81e-5cdef08b2c07)

### 6.5 图文类-代码节点-对模型返回结果提取参数

#### 6.5.1 添加代码节点并配置

![代码节点配置图](https://github.com/user-attachments/assets/3f95859c-f0e8-4354-a2d1-41466cbb7a22)

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

![代码节点测试结果](https://github.com/user-attachments/assets/7eb994d1-a464-4df1-b166-167a8ef2d8f3)

### 6.6 飞书多维表格add_record插件-信息写入飞书

#### 6.6.1 参数准备

还是先来看我们的表格元素分类图。

![表格元素分类图](https://github.com/user-attachments/assets/ccd05afa-4ffc-47c3-af65-c9dd96780bcf)

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

![添加插件](https://github.com/user-attachments/assets/8147deae-b668-4cd7-aaec-e7c3a0a1aa1c)

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

![插件图](https://github.com/user-attachments/assets/61793485-bc8c-450d-8a4a-94f86d84c6bf)

#### 6.6.3 代码节点-json序列化处理输入

现在我们要将所有表格需要的参数处理成records需要的格式。

新建代码节点，与上个节点连线，配置输入输出参数，如图所示：

![配置输入输出](https://github.com/user-attachments/assets/12ba6f00-1b62-4f71-946e-6ecfb27bf007)

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

![测试结果图](https://github.com/user-attachments/assets/0d2a545e-12ff-4130-bb94-0492febab552)

#### 6.6.4 连接飞书新增记录节点

复制链接到浏览器打开。

![复制链接](https://github.com/user-attachments/assets/d15fd91b-a501-44ad-b236-26306e0e14fb)

参数如图所示。

![表格链接参数](https://github.com/user-attachments/assets/13ff9c63-c1c8-4156-93ac-dbe835df4d14)

配置add_records节点，并连接结束节点，选择add_records节点返回msg作为输出。

![连线](https://github.com/user-attachments/assets/0661ead5-ce4f-4c67-8632-cba681294205)

![结束节点设置](https://github.com/user-attachments/assets/59e729c4-ae9e-45f2-b3a3-fb7a201749a7)

### 6.7 图文类-整体测试

经过前面的搭建，我们对图文类的处理基本完成。整体图如图所示：

![图文类整体图](https://github.com/user-attachments/assets/7e6018e1-618e-4e34-aed0-ac4f77423c85)

准备一个链接，点击右上角的试运行来测试下！！！

![开始节点输入](https://github.com/user-attachments/assets/af71abe8-71a2-4a34-a70b-0367fbc0744c)

第一次的时候会要求飞书对coze授权，复制信息到浏览器进行授权或者点击授权按钮。

![第一次运行授权问题](https://github.com/user-attachments/assets/da9291af-b6a0-4a6a-9c27-720416504d24)

![授权](https://github.com/user-attachments/assets/ac2e7879-59a0-44c4-aee8-58c0809c5fc1)

重新试运行一次。

![工作流运行结果](https://github.com/user-attachments/assets/b992db0e-2fa1-4e23-ba5b-e2a7d0500ba0)

打开飞书表格，可以看到记录已经进来了。

![飞书表格记录](https://github.com/user-attachments/assets/611e095f-eeae-42e4-b311-eda7c463b5fd)

为了更直观查看，我们可以添加画册视图。

![添加画册视图](https://github.com/user-attachments/assets/cbf051a9-ee7b-45d6-906a-8ef7860376ec)

![画册视图](https://github.com/user-attachments/assets/7c3addd9-906b-442f-b49d-c1436a4a12c4)

图文类完成后，我们要对if选择器分支的else（即对视频类进行处理）

## 7. 视频类

首先按开始的设想（不做爬虫，不写摘要），我们设计2种情况：

1. 针对分享链接内包含了标题信息：这类，我们就用代码提取出url,并用大模型节点来对分享链接的文字部分生成title。

2. 针对只有url的：这类，我们就根据reason通过大模型来生成一个简要的title。

### 7.1 if选择器-是否要对链接处理

添加if选择器，并连线上一个if选择器的否则分支。

![添加if选择器](https://github.com/user-attachments/assets/cbc59f7d-f476-4f49-a6d3-e1eff42e7203)

### 7.2 需要链接处理分支（handle为true）-代码节点-提取url

#### 7.2.1 代码节点配置

新增代码节点，进行提取。

![配置图](https://github.com/user-attachments/assets/bc50c918-1639-4444-a7e1-2cee09cdb3af)

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

![b站链接测试](https://github.com/user-attachments/assets/7c6a0a6f-2c47-4f04-86d6-9eca5ec92aaa)

![抖音链接测试](https://github.com/user-attachments/assets/a6d1c602-fdbd-4164-957c-281b72b06a73)

### 7.3 需要链接处理分支（handle为true）-自定义插件-对标题、平台做提炼

#### 7.3.1 插件设置

![插件设置](https://github.com/user-attachments/assets/1f700a67-eafa-4cdb-bd5a-48f49b000ca8)

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

![测试结果](https://github.com/user-attachments/assets/bedc206e-a328-4060-9618-ec748a578486)

### 7.4 需要链接处理分支（handle为true）-代码节点-对大模型返回结果做参数提取

#### 7.4.1 节点设置

![节点设置](https://github.com/user-attachments/assets/701fa4fc-814e-4d68-8991-667bec0d9709)

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

![测试结果](https://github.com/user-attachments/assets/33e27e2c-8fbc-46ae-9562-81e38dbf0365)

截止这一步，我们对**需要链接处理分支（handle为true）** 的处理已经完成，下面对handle为false的情况来处理。

### 7.5 不需要链接处理分支（handle为false）-文本处理节点-参数拼接

这类，我们要根据reason通过大模型来生成一个简要的title，与此同时判断平台一并让大模型处理。大模型只能接收一个参数，所以我们要对reason和url进行一个参数拼接后再传入。

添加文本处理节点，并配置，如图所示：

![配置](https://github.com/user-attachments/assets/19613dc1-fa77-4812-993a-2a402c5eccef)

测试结果：

![测试结果](https://github.com/user-attachments/assets/6fc0f80c-7def-46c5-89f5-dd3fd6a40e50)

### 7.6 不需要链接处理分支（handle为false）-自定义插件-对标题、平台做提炼

#### 7.6.1 插件设置

![配置图](https://github.com/user-attachments/assets/408637a4-2ae4-4f9f-abe8-5a7df18f7a04)

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

![测试结果](https://github.com/user-attachments/assets/4a98c668-4acd-4c86-b2e8-0d505d4c0492)

### 7.7 需要链接处理分支（handle为false）-代码节点-对大模型返回结果做参数提取

直接复制7.4的代码处理节点，并引用模型返回结果。

测试结果如下:

![测试结果](https://github.com/user-attachments/assets/095550fe-4d2e-4a30-937d-b7cf765dee70)

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

![分支图](https://github.com/user-attachments/assets/4037fcea-f2ed-4e94-af0d-e136dcbab3e4)

添加代码节点，并配置，如图所示：

![配置图](https://github.com/user-attachments/assets/0a402ccc-ace5-4d3e-b712-c2698641cad5)

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

![测试结果](https://github.com/user-attachments/assets/4608a1cc-7622-4231-88af-98123b11bb3a)

### 7.9 视频类-代码节点-json序列化处理输入

现在我们要将所有表格需要的参数处理成records需要的格式。因为和图文类的参数不统一，所以我们需要再做一次json序列化处理。

复制图文类的json处理节点，与上个节点连线，配置输入输出参数，这里注意的是，要删除summary入参，如图所示：

![配置](https://github.com/user-attachments/assets/c58186ee-9eab-4a7e-9d55-bcefd7c7bcbb)

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

![配置图](https://github.com/user-attachments/assets/0957125f-2ca2-4a23-940a-4eae60ec6ee3)

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

![代码节点与添加记录连线](https://github.com/user-attachments/assets/bc5c65a8-2799-442e-8d94-507b7c102337)

![添加记录与结束节点连线](https://github.com/user-attachments/assets/ec292362-4138-4e93-a6a7-26d0455ea765)

### 7.12 测试

整体图如下所示:

![整体图](https://github.com/user-attachments/assets/67a0f5f6-e5d4-4fd8-9acf-0b8b2390e8f9)

整个流程很快，大概3-10s

![工作流运行完成](https://github.com/user-attachments/assets/a09ff4fb-8bcc-4878-a008-1fda0b7e6a8a)

看下飞书表格。

![表格视图](https://github.com/user-attachments/assets/d8e114c3-3228-4b76-b88f-40c47d48c40b)

![画册视图](https://github.com/user-attachments/assets/4b913631-3577-44a2-9352-cd0c11a19841)

**一定要记得发布节点**

# 四、（专业版）火山方舟控制台添加其他模型

现在专业版只提供了3个模型，不太满足我们的需求，我们登录[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/model?vendor=Bytedance&view=LIST_VIEW)

点击“在线推理”，点击“创建推理接入点”，这里我新增了一个doubao-128k的模型。

![在线推理](https://github.com/user-attachments/assets/27f56ee9-34ef-4aef-8d88-558b7347e27e)

![添加处](https://github.com/user-attachments/assets/0dda43f5-6e56-4878-87fd-4d9eabc71fc6)

![添加模型](https://github.com/user-attachments/assets/69f7a80d-ddf1-443d-9936-579da25345d9)

# 五、使用bot进行内容收藏

## 1. 新增bot

点击"项目开发"，新增一个智能体

![新增智能体](https://github.com/user-attachments/assets/b55eaf95-f81b-4a92-b19b-d3fb7f4c44f6)

## 2. 智能体内添加已发布的工作流

![添加工作流](https://github.com/user-attachments/assets/c6945d52-5540-4e89-9886-66cfd3115b4a)

## 3. 设置快捷指令(100%调用工作流）

原先我给定的提示词如图所示，即便对Bot的人设与回复逻辑做了层层限制，它还是不会100%按照设定和限制来调用工作流。

![原先人设](https://github.com/user-attachments/assets/b6ae0b21-81b7-4e73-93d5-b27c14250680)

想做到100%调用工作流。这里我们就要用到**快捷指令功能**。官方描述是

```
快捷指令是对话输入框上方的按钮，配置完成后，用户可以快速发起预设对话
```

### 3.1 添加内容收藏工作流快捷指令

![添加快捷指令位置](https://github.com/user-attachments/assets/9c38f5c0-a2cf-4300-bcbe-11d75bbb3b70)

![空白快捷指令](https://github.com/user-attachments/assets/047563e1-1e81-4627-82cc-eb8fc84c60bd)

### 3.2 指令详细设置

我们添加工作流后，下方会提取到开始节点的输入参数，如图所示。

![快捷指令设置1](https://github.com/user-attachments/assets/8876a5d3-1ff8-4d21-ba6d-24da62e594eb)

![快捷指令设置2](https://github.com/user-attachments/assets/6aa01f64-e0ea-430f-95c0-0b7c0a4b56ad)

右侧是展现给用户的输入框，英文描述有时候没有中文来得直观，我们可以修改左侧的组件设置。如图所示：

![中文设置](https://github.com/user-attachments/assets/d5d1a7d9-e63a-4590-a4e9-d4ddef2e710d)

**指令内容**就是我们要发送给工作流的内容。可以点击右上角的按钮插入变量，也可以直接输入`{`快速选择。设置完成后点击确认。

![指令设置](https://github.com/user-attachments/assets/1474deb9-de08-46cd-bb73-f6ebdd0d5127)

### 3.3 测试

现在可以看到输入框上方有了快捷指令，点击这个快捷指令。

![快捷指令位置](https://github.com/user-attachments/assets/7ee95295-60fd-4679-9298-d701dc92a7d6)

![快捷指令展开](https://github.com/user-attachments/assets/16a69730-188a-43d6-94e9-a00125b525db)

输入参数并发送，第一次会出现要授权飞书表格的提示，授权后重新运行一边就可以了。这里我做了一个关于图文一个关于视频的测试，可以看到已经添加进飞书表格了。

![授权](https://github.com/user-attachments/assets/44ab9112-6d4e-491b-9ac2-93b7bec5aab3)

![测试结果](https://github.com/user-attachments/assets/eef70255-bddc-48be-8896-a70ba7f86a38)

![测试结果画册视图](https://github.com/user-attachments/assets/bf6b0f11-e712-402a-8a90-eee8b6b01bfb)

## 4. bot运行期间效果不理想如何排查

如果运行期间输出结果不对，我们可以点开"调试"挨个节点排查。

![调试](https://github.com/user-attachments/assets/bb9dddf9-0e2a-4a27-a497-7d7ad1b352ab)

# 六、发布到飞书机器人+微信订阅号/公众号

## 1. 设置智能体提示词

我们在设置快捷指令的时候，设置了一个"指令名称"，指令名称就是方便我们在飞书机器人、公众号等外部平台调用快捷指令的。

![快捷指令名称](https://github.com/user-attachments/assets/45cc88c0-6c2b-4ed4-909f-3863a6b3f29d)

为了让用户更方便输入，我们给外层bot的提示词内加上模板。

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

不想公开在coze商店的话就取消勾选。

根据页面提示授权飞书机器人和订阅号（或者公众号）。

点击发布后需要**等待审核完成**。

![发布页面](https://github.com/user-attachments/assets/e52619ad-83cc-44e7-8e17-5f213fbf232b)

## 3. 飞书机器人测试

点击"工作台"，可以看到我们已经发布的机器人。

![飞书机器人位置](https://github.com/user-attachments/assets/946dbac9-57d5-48cd-a4c9-c76069be5ec6)

![飞书机器人测试结果](https://github.com/user-attachments/assets/bcbc74c6-67f8-4849-8a8f-ed0cdc2ddc2c)

![测试结果画册视图](https://github.com/user-attachments/assets/39fb6483-e69f-462e-8078-5ca1ee702ca4)

## 4. 订阅号（公众号）测试

![订阅号测试](https://github.com/user-attachments/assets/15a4d344-51c6-4ea2-9af0-aafcc3ef2f41)

![订阅号画册视图](https://github.com/user-attachments/assets/8b2f634c-8a46-41b7-ac0f-b8e3e93c159f)


# 七、（进阶教程）获取github trending 并每周定时自动添加进飞书表格

## 1. 新建一张数据表

我们只需要存储：日期、url、简介、标题

| 字段名     | 飞书字段类型 | 字段说明                                                                 |
|------------|---------------|--------------------------------------------------------------------------|
| 日期       | 文本        | 日期                                              |
| 标题       | 文本        | 标题                                              |
| 简介       | 文本        | 通篇大概讲了什么，由大模型生成                       |
| 地址       | 文本        | 地址                                              |
| language   | 单选        | language标签                                              |

![空表示例](https://github.com/user-attachments/assets/7d6f14ba-de4b-40f7-8cdb-ff9c3e81bfbd)

## 2. 添加变量

前面一个工作流我们使用的是在插件内输入变量。稍微有点麻烦的情况是，当你修改完表格链接后需要先发布工作流再发布bot。

![直接输入链接](https://github.com/user-attachments/assets/c722cea0-ba6f-4661-8e53-351aba87f407)

这里我们采用一种更便捷的方式：变量，来方便后续如果要对表格链接做改动。这样就只需要对Bot进行重新发布就可以了。

进入我们的bot，点击添加变量，输入变量名称，输入默认值（我们的表格链接）保存。

注意：这里表格链接一定要准确，不要设置成内容收藏表格的里链接。

![表格链接](https://github.com/user-attachments/assets/262c972e-eb3f-4b12-9571-b164d861329c)

![添加变量1](https://github.com/user-attachments/assets/e28c344e-374a-454f-95b1-997c90fe28ee)

![添加变量2](https://github.com/user-attachments/assets/b1d944a3-1325-4851-9841-bb05f6219c3b)

## 3. 新建获取github每日热点工作流

### 3.1 添加时间插件和变量节点

我们可以把这两个并排放到开始节点后方。获取日期插件和上一个工作流一样。点击"变量"节点。

![新增变量节点](https://github.com/user-attachments/assets/834d3c2e-cfeb-4fc0-88b3-50c56e767da0)

选择"从智能体获取变量值"，key值和我们在bot中设置的要一致。

![变量节点设置](https://github.com/user-attachments/assets/14aa5a75-686b-46b5-b62d-69954cd890c5)

![连线图](https://github.com/user-attachments/assets/2ac9ee68-2552-49e1-88cc-88804a3cd801)

### 3.2 添加热榜插件

#### 3.2.1 添加热榜插件直接插件市场添加别人现成的

插件市场里添加热榜插件。

![添加插件](https://github.com/user-attachments/assets/27d9e94e-ed9d-41fd-b8c1-a8633595342f)

点击"测试该节点"，可以看到我们返回参数有如左半边所示。

![测试结果](https://github.com/user-attachments/assets/0ca1af77-5c5d-4063-8f20-858b57258b27)

#### 3.2.2 自定义插件

自定义插件放到工作流中有时候会网络超时，教程演示使用的插件市场现成的。

新建插件。

![新建插件1](https://github.com/user-attachments/assets/85cebe89-f299-4147-b5ed-062a2a91fd9c)

插件代码如下：

```
from runtime import Args
from typings.get_github_trending.get_github_trending import Input, Output
import requests
from bs4 import BeautifulSoup
from datetime import datetime
import json


def get_trending_repos():
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
        'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
        'Connection': 'keep-alive'
    }
    params = {
        'spoken_language_code': ''
    }
    
    url = 'https://github.com/trending'
    response = requests.get(url, headers=headers, params=params)
    print(response)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    repos_data = []
    repositories = soup.select('article.Box-row')
    
    for repo in repositories:
        try:

            # 获取仓库完整名称（包含作者和仓库名）
            link_element = repo.select_one('h2.h3 a')
            if link_element:
                full_name = link_element.get_text(strip=True).replace('\n', '').replace(' ', '')
                repo_url = f"https://github.com{link_element['href']}"
            else:
                full_name = None  # 或者处理缺少链接的情况
                repo_url = None  # 根据需要进行处理

            # 分离作者和仓库名
            author, repo_name = full_name.split('/')
            
            # 获取描述
            description = repo.select_one('p')
            description = description.get_text(strip=True) if description else ''
            
            # 获取language
            language_span = repo.select_one('span[itemprop="programmingLanguage"]')
            language = language_span.get_text(strip=True) if language_span else 'Unknown'            
            
            # 获取统计信息
            stats = repo.select('a.Link--muted')
            stars = stats[0].get_text(strip=True).replace(',', '') if len(stats) > 0 else '0'
            
            # 获取今日star数量
            today_stars_elem = repo.select_one('span.d-inline-block.float-sm-right')
            today_stars = ''
            if today_stars_elem:
                today_stars = today_stars_elem.get_text(strip=True).split()[0].replace(',', '')
            
            
            repo_info = {
               "description":description,
               "language":language,
               "name":repo_name,
               "stars":stars,
               "today_stars":today_stars,
               "url":repo_url
            }
            
            repos_data.append(repo_info)
            
        except Exception as e:
            print(f"处理仓库时出错: {str(e)}")
            continue
    
    return repos_data

def handler(args: Args[Input])->Output:
    repos_data = get_trending_repos()
    return {"result": repos_data}
```

插件依赖包配置：

![插件依赖包配置](https://github.com/user-attachments/assets/54f29cea-f355-4b92-b971-a8c045784181)

插件元数据配置：

![插件元数据配置](https://github.com/user-attachments/assets/cb996e55-dc8e-456e-9675-5996dded9a4e)

测试结果：

![榜单](https://github.com/user-attachments/assets/24828701-99f8-42a3-b04f-10fc03fb7fbc)

![测试结果](https://github.com/user-attachments/assets/f0abda12-1dfb-4e18-91a2-aa989bb89a50)

![工作流内测试结果](https://github.com/user-attachments/assets/41a8cf9b-ccae-4c87-9d36-e8e551946cec)

**测试后记得发布**

### 3.2 工作流思路

1. 由插件来获取飞书表格所需字段值
2. （循环节点内）爬虫爬取页面信息，并做处理
3. （循环节点内）大模型对爬取内容进行英译中
4. （循环节点内）序列化处理，写入飞书表格

### 3.3 循环翻译并输出写入飞书表格

除了使用循环节点外，还有一种做法就是用官方的大模型节点，选择批处理。

本教程演示的是自定义的InternLM模型调用插件，能否和官方一样可以通过自定义实现批处理我不知道，反正我不会写这块代码

#### 3.3.1 新建循环节点

点击新增循环节点。

![新建循环节点](https://github.com/user-attachments/assets/2a05a327-7708-4fa5-9b5a-832c528d0810)

进行一些变量配置。

![配置页面](https://github.com/user-attachments/assets/aafd4a19-7107-4aec-958c-32aafbf7c39a)

#### 3.3.2 循环体内添加jina-reader插件来爬取内容

点击循环体节点，蓝色框代表已经选中，在选中的情况下，再去点击添加其他插件。

![循环体添加插件](https://github.com/user-attachments/assets/8ff241a4-e0da-4a78-9fc6-c4ceb3a9a5a9)

插件配置如图所示,url输入的是"循环"节点内的值：

![配置](https://github.com/user-attachments/assets/ea1f13e6-87e2-4f69-b272-5ef090144ac7)

#### 3.3.3 爬取内容处理

和内容收藏工作流一样，我们要对爬取内容做去除简单markdown格式处理和截取文字。

点击"循环体"让它呈选中状态再点击添加"代码"节点，配置如图所示：

![配置图](https://github.com/user-attachments/assets/fa019dcf-ded3-4128-8907-8cdc0733bfce)

```python代码```

```
import re
from typing import List, Tuple, Any
import json

async def main(args: Any) -> str:
    try:
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

        # 2. 使用正则表达式处理 Markdown 超链接、图片和格式字符
        def markdown_to_text(md: str) -> Tuple[str, List[str]]:
            # 提取 URL
            urls = re.findall(r'\[.*?\]\((.*?)\)', md)
            # 去掉链接部分，保留链接文本，去掉图片链接，去掉 Markdown 的格式字符，去掉多余的换行符
            md = re.sub(r'!\[.*?\]\(.*?\)|\[([^\]]+)\]\([^\)]+\)|[#*`-]|\n+', lambda m: '' if m.group(0).startswith('![') or m.group(0).startswith('[[') else (m.group(1) + ' ') if m.group(1) else '\n', md)
            return md.strip(), urls

        # 转换 Markdown 到普通文本
        plain_text, extracted_urls = markdown_to_text(raw_content)

        # 3. 去除多余的横线、等号，并处理换行符和空白字符
        stripped_content = re.sub(r'-{2,}|={2,}', '', plain_text)
        stripped_content = re.sub(r'\n+', '\n', stripped_content).strip()

        # 4. 计算去除 Markdown 后的字符数量
        clean_char_count = len(stripped_content)

        # 5. 如果内容超过 1500 字符，则截取前 1500 字符
        stripped_content = stripped_content[:1500]

        # 6. 添加处理后的结果到返回值列表
        ret.append({
            "plain_text": stripped_content
        })

        # 7. 将返回值转换为 JSON 字符串格式，并作为最终输出
        return json.dumps({"output": ret}, ensure_ascii=False)

    except Exception as e:
        return json.dumps({"error": str(e)}, ensure_ascii=False)
```

#### 3.3.4 大模型进行逐条翻译

点击"循环体"呈现选中状态，添加我们自定义的大模型插件。输入提示词和引入我们需要翻译的值。

![配置图](https://github.com/user-attachments/assets/86243639-8b4c-4845-9065-6dcc72630454)

```提示词```

```
# 任务\n 根据用户输入，进行翻译\n
# 输出格式要求\n
直接输出原始JSON格式，不要使用任何包装或代码块。 严格按照以下格式输出，不添加任何其他内容：\n {\n \"summary\": \"在这里填写内容摘要\" }\n
# 输出内容要求\n summary：根据用户输入的<plain_text>，捕捉内容主题、阅读价值，生成一段简洁而全面的摘要。不要使用\"摘要：\"或类似前缀。使用最常见、最正式的名称。
# 重要提醒\n
- 不要使用json或任何其他代码块标记\n
- 不要将输出包装在\"answer\"或任何其他字段中\n
- 确保输出的是可以直接被解析的原始JSON\n
- 除了指定的JSON结构外，不要输出任何其他内容
```

#### 3.3.5 序列化准备写入飞书表格

和内容收藏工作流一样，我们要对写入飞书表格的数据进行一个序列化处理。与上个工作流不同的是，此处代码，增加了去除"summary"字符处理。

![配置图1](https://github.com/user-attachments/assets/9bbe12fb-1ab5-4ec7-a4a7-5ecd242168a3)

![配置图2](https://github.com/user-attachments/assets/e17959d5-370d-479f-88e8-4cda0d04f731)

```js代码```

```
async function main({ params }: Args): Promise<Output> { 
    const contentMatch = params.content.match(/"summary":\s*"([^"]+)"/);
    const summary = contentMatch ? contentMatch[1] : ""; 

    const fields = {
        "标题":params.input["name"],
        "简介":summary,
        "日期":params.time,
        "地址":params.input["url"],
        "language":params.input["language"]

    };
    const ret = {
        output: [
            { fields }
        ]
    };

    return ret;
}

```

#### 3.3.6 写入飞书表格

点击"循环体"让它呈选中状态再点击添加"写入飞书多维表格"插件，配置如图所示：

![配置图](https://github.com/user-attachments/assets/6f1466a4-f657-440c-b8e8-17126562d08b)

#### 3.3.7 配置输出

给循环节点配置一个输出变量，主要方便查看是否写入成功。

![输出配置1](https://github.com/user-attachments/assets/276af147-b069-4402-9d2a-b32edbfc74ae)

![输出配置2](https://github.com/user-attachments/assets/70352811-8a68-42ac-88cf-04670a7cc7c9)

#### 3.3.8 测试结果

![测试结果](https://github.com/user-attachments/assets/d4a1c410-8025-4932-b554-284a81330482)

### 3.4 定时处理工作流并发送机器人

#### 3.4.1 触发器设置

点击进入bot页面，首先添加我们刚刚发布的工作流，然后点击"新建触发器"

![新建定时器](https://github.com/user-attachments/assets/deae2602-56d6-4263-bcb3-6bc0cad95434)

我想要的是每周一八点给我进行录入和推送，大家可以自行选择什么时间段，为了测试看结果，先选择一个就近时间时间，要考虑加入审核时间，大概选择1-2h后差不多。

这里我们的任务执行选择工作流。设置如图所示。

![触发器设置](https://github.com/user-attachments/assets/cb6e5539-555a-499d-8261-5b5dcf2bbce7)

#### 3.4.2 发布

设置完成后一定要点击发布。

![发布](https://github.com/user-attachments/assets/8a67e412-a57d-40bc-ad72-80970ddf2027)

#### 3.4.3 测试结果

触发成功，虽然机器人回复有问题，但是依旧成功写入飞书表格

![成功结果](https://github.com/user-attachments/assets/3ae3d6d2-407c-4af7-a571-6ab8ea593faf)


# 八、飞书应用、机器人相关补充

## 1. 如何删除弃用的飞书应用

登录[飞书管理后台](https://feishu.cn/admin),点击"应用管理"，筛选开发者为企业内成员（个人账号就筛选个人）。可以看到我们已经创建的应用。

![飞书应用列表](https://github.com/user-attachments/assets/789c7f78-6f4d-4438-bc83-f391d7d5b845)

点击"配置"，停用应用。

![停用应用](https://github.com/user-attachments/assets/f1363c70-595a-4dd0-8b2f-94a69ad3e561)

登录[飞书开发者后台](https://open.feishu.cn/app),点击进入要删除的应用，在"凭证信息"页面下拉，点击删除。

![停用部分应用后列表](https://github.com/user-attachments/assets/fb48ba63-6bbd-4434-b0f6-40037d48eb4e)

![删除应用](https://github.com/user-attachments/assets/9ad0e338-7144-4e9b-859e-7cb74970c476)

## 2. 如何多人使用同一机器人（仅限同一组织）

新建群组，添加群成员和群机器人。以下演示账号为企业账号，个人账号步骤相同。

### 2.1 为应用添加机器人

点击登录[飞书开发者后台](https://open.feishu.cn/app)，选择应用。截图是个人账号添加情况，企业账号无需手动添加。

![个人账号添加机器人](https://github.com/user-attachments/assets/2c33c771-2f79-4e22-81c6-371210f5eb46)

添加完成后需要发布版本。

![发布版本](https://github.com/user-attachments/assets/00285964-3104-405b-b308-689fbe9242fe)

### 2.2 新建群组并添加群机器人

打开飞书，点击"通讯录" > "群组" > "新建群组" > 添加"群机器人"

![新建群组](https://github.com/user-attachments/assets/02ca8de2-7127-4cef-b347-3152fbac883e)

![新建群机器人位置](https://github.com/user-attachments/assets/eb696bbb-bd80-44c6-a146-0a7a4b61e251)

![添加群机器人](https://github.com/user-attachments/assets/09f83143-789b-4b7b-ba21-110c830fa6e8)

### 2.3 如何使用群机器人

#### 2.3.1 在输入框内@机器人发送信息

![@机器人](https://github.com/user-attachments/assets/e2b59951-2155-4b63-bc35-da951146b3f4)

![测试结果](https://github.com/user-attachments/assets/c61db1f6-df4f-438b-a469-a6dd074574c2)

现在添加一个组织内的成员来切换身份测试下。

![添加组织内成员测试](https://github.com/user-attachments/assets/0478b2d6-f801-4484-869e-ffc2c39323a4)

第一次会要求我们进行一个授权。

![授权](https://github.com/user-attachments/assets/027e8608-2143-4c4e-95ec-270e79d1a5c9)

![添加成功](https://github.com/user-attachments/assets/c74619e7-e9fa-4020-81b2-967a412362a0)

我们打开飞书表格看下，2次添加是否成功。

![飞书表格验证](https://github.com/user-attachments/assets/6362c208-c699-42bb-8377-80857f63e918)

#### 2.3.2 点击头像私聊

![点击头像私聊](https://github.com/user-attachments/assets/0ad7d523-bd78-4fbd-adcd-002c08c2653b)

## 3. 对外共享机器人（不是同一组织的）

场景描述：企业在进行业务沟通时，可能面临跨租户沟通的场景。例如上下游企业的成员沟通、企业与外部客户之间的沟通等。在这些场景中，企业可以开发一个允许对外共享的机器人，利用机器人能力高效完成沟通协作。开启对外共享的机器人支持以下功能：

- 添加在外部群，实现拉人入群、推送消息、响应群成员消息等功能。
- 与外部用户单聊。通过自动化流程实现用户可能面临的产品功能反馈、业务进展通知等需要。

前提必须是：认证过的企业号。

### 3.1 企业认证

登录[飞书管理后台](https://feishu.cn/admin)，点击"企业设置" > "企业信息"进行认证。

企业自建应用的对外共享仅支持认证租户使用。认证时长1-2个工作日（实际大概1-2h左右），可以选择3种方式认证。

![企业认证方式](https://github.com/user-attachments/assets/cb113d4e-6a04-4c47-be65-e3a646931a43)

![认证成功](https://github.com/user-attachments/assets/8fd72f5f-7dc4-499b-aa9d-8dc212f2dedf)

### 3.2 对外共享

认证完成后，登录[飞书开发者后台](https://open.feishu.cn/app)。点击想要共享的机器人，在应用左侧导航栏，选择 应用发布 > 版本管理与发布。

在 "版本管理与发布" 页面右上角，点击 创建版本。

在 "版本详情" 页面选中 "允许机器人被添加到外部群中使用"。

![对外共享](https://github.com/user-attachments/assets/33f90c6d-4555-4ffe-bc83-574a22e286ce)

登录[飞书管理后台](https://feishu.cn/admin)，可以看到应用审核已经有待办消息了。

![飞书管理后台](https://github.com/user-attachments/assets/95e74c37-5a34-4d70-be3d-bc7c36f00a74)

点击通过后会有消息弹窗。

![授权成功弹窗](https://github.com/user-attachments/assets/9136d407-2115-4615-8501-2279a12f7d3e)

（补充）想免审的话，就选择"设置审核规则"

![设置审核规则](https://github.com/user-attachments/assets/70072056-ecbb-4164-b6fb-575b2f70c06a)

### 3.3 对外共享机器人测试

我们新建一个外部群组。

![新建外部群组](https://github.com/user-attachments/assets/35e0cc6f-3ef7-444d-a9e3-8750b6f1f99e)

添加群机器人。

![添加群机器人](https://github.com/user-attachments/assets/2da7a74f-66f9-4447-b510-b7a8d1b64cc1)

切换账号来测试下。依旧是初次使用要授权。

![测试结果](https://github.com/user-attachments/assets/b0f81c80-919a-446f-822b-268141e2c1c5)

看下飞书表格，内容已经进来了。

![飞书表格](https://github.com/user-attachments/assets/b695eaec-d060-4643-b854-6452eca95d4b)

