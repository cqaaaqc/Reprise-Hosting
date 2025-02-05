# OpenAI API 使用指南：从入门到实践

本文将详细介绍如何获取 OpenAI API Key、查询和充值额度，并通过 Python 进行 API 测试。无论你是新手还是有经验的开发者，都能快速上手 OpenAI 的强大功能。

## 一、获取 OpenAI API Key

### 1.1 注册 OpenAI 账号
如果你之前使用过 ChatGPT，那么你的 ChatGPT 账号就是 OpenAI 账号，直接登录即可。如果没有账号，可以前往 OpenAI 官网进行注册。需要注意的是，由于国内网络限制，注册过程可能较为复杂。你可以参考网上的教程，或直接购买现成的账号。

### 1.2 生成 API Key
登录 OpenAI 后，将鼠标移动到页面左侧，会弹出侧边栏。点击“API Keys”进入管理页面。

![获取 API Key](https://bbtdd.com/img/2742027202176066.webp)

在 API Keys 页面，点击“Create new secret key”创建新的 API Key。创建后，系统会弹出一个对话框，显示生成的 Key。务必立即保存，因为关闭对话框后将无法再次查看此 Key。

## 二、查询与充值 API 额度

### 2.1 查询使用额度
在侧边栏中点击“Usage”，进入使用页面。页面左侧显示每日花费，右侧显示当前额度。

![查询额度](https://bbtdd.com/img/884768083042.webp)

在 Credit Grants 区域，额度分为三种状态：灰色（未使用）、绿色（已使用）、红色（已过期）。只有未使用的额度（灰色状态）才能成功调用 API。

### 2.2 充值额度
在侧边栏中点击“Setting”下的“Billing”，进入账单页面。在此页面中，你可以管理支付方式并进行充值。

由于国内网络限制，部分国内 Visa 卡可能无法使用。建议使用国际信用卡或虚拟卡。👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)

添加支付方式后，返回 Overview 页面，点击“Add to credit balance”进行充值。充值完成后，可在 Usage 页面查看额度变化。

## 三、Python 环境下使用 OpenAI API

### 3.1 配置 Python 环境
确保你的 Python 版本为 3.7.1 或以上。建议使用 Anaconda 创建虚拟环境，以便管理依赖。

### 3.2 安装 OpenAI 库
通过 pip 安装 OpenAI 库：
bash
pip install openai


### 3.3 设置 API Key
OpenAI 默认会从环境变量中读取“OPENAI_API_KEY”。你可以通过以下两种方式设置 API Key：

- **全局设置**：在系统环境变量中添加 OPENAI_API_KEY。
- **项目设置**：在项目根目录下创建 `.env` 文件，并输入 `OPENAI_API_KEY=your_api_key`。

python
from openai import OpenAI
client = OpenAI()


### 3.4 发送请求测试
以下是一个简单的 GPT-3.5 聊天请求示例：

python
import os
import dotenv
from openai import OpenAI

dotenv.load_dotenv()

client = OpenAI(
    api_key=os.environ.get("OPENAI_API_KEY"),
)

response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a poetic assistant, skilled in explaining complex programming concepts with creative flair."},
        {"role": "user", "content": "Compose a poem that explains the concept of recursion in programming."}
    ]
)

print(response.choices[0].message.content)


发送请求后，你可以在 Usage 页面查看此次请求的花费及 token 数量（可能会有延迟）。

## 四、API 功能介绍（以 Python 为例）

### 4.1 文本生成
OpenAI 的文本生成功能支持多种模型，如 GPT-3.5 和 GPT-4。以下是生成对话的示例：

python
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Who won the world series in 2020?"},
        {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
        {"role": "user", "content": "Where was it played?"}
    ]
)


### 4.2 图像理解（GPT-4 Vision）
GPT-4 Vision 版本可以理解图像内容。你可以通过以下方式传入图像 URL：

python
response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {"role": "user", "content": [
            {"type": "text", "text": "Describe this image."},
            {"type": "image_url", "image_url": ""}
        ]}
    ]
)


### 4.3 图像生成
OpenAI 的图像生成功能可以根据文本描述生成图像。以下是示例：

python
response = client.images.generate(
    model="dall-e-3",
    prompt="A futuristic cityscape at sunset",
    n=1,
    size="1024x1024"
)

image_url = response.data[0].url


通过以上步骤，你可以快速上手 OpenAI API，并将其应用于各种场景，如文本生成、图像理解与生成等。