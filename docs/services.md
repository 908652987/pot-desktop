# 服务配置

各服务都需要到对应服务商的官方平台申请凭据，然后把凭据填入应用的 **设置 → 服务设置**。具体字段以设置页提示为准。

| 服务 | 类型 | 官方入口 |
|---|---|---|
| 百度翻译/OCR | 翻译、识别 | https://fanyi-api.baidu.com/ 、https://cloud.baidu.com/product/ocr |
| 腾讯翻译/OCR | 翻译、识别 | https://cloud.tencent.com/product/tmt 、https://cloud.tencent.com/product/ocr |
| 讯飞 OCR | 识别 | https://www.xfyun.cn/ |
| 火山引擎翻译/OCR | 翻译、识别 | https://www.volcengine.com/product/machine-translation |
| 阿里翻译 | 翻译 | https://www.aliyun.com/product/machine-translation |
| 有道翻译 | 翻译 | https://ai.youdao.com/ |
| DeepL | 翻译 | https://www.deepl.com/pro-api |
| OpenAI | 翻译 | https://platform.openai.com/api-keys |
| Gemini | 翻译 | https://ai.google.dev/ |
| ChatGLM | 翻译 | https://open.bigmodel.cn/ |
| Ollama | 翻译 | https://ollama.com/ |
| 彩云小译 | 翻译 | https://dashboard.caiyunapp.com/ |
| 小牛翻译 | 翻译 | https://niutrans.com/ |
| Transmart | 翻译 | https://transmart.qq.com/ |
| Anki | 生词本 | https://foosoft.net/projects/anki-connect/ |
| 欧路词典 | 生词本 | https://dict.eudic.net/ |
| 简单 LaTeX | 识别 | 本地识别，无需申请 |
| Lingva | 翻译、语音 | 自建或使用可用的公开实例 |

## 说明

- 需要 API Key、Secret、App ID 的服务，请在服务商控制台创建应用后填写。
- Lingva、ECDICT 与阿里云备份不再默认启用；如仍需要，可在服务设置中手动添加或自行部署对应服务。
