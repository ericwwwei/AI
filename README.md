# AI-
AI小说生成器 v2.0.0 - 真正免费的AI创作工具
<img width="1373" height="713" alt="image" src="https://github.com/user-attachments/assets/a77d1500-1e85-4e3c-90c1-897e4b662617" />
应用简介
AI小说生成器是一款基于Electron开发的桌面应用，通过集成DeepSeek网页版实现完全免费的AI小说创作功能。无需购买API、无需高配置硬件，只需一个DeepSeek账号即可开始您的创作之旅。
# 下载链接
链接：https://pan.xunlei.com/s/VOiCWqJe4vrnS5_A9DutsnvHA1?pwd=rabr# 复制这段内容后打开「手机迅雷 App」即可获取。无需下载在线查看，视频原画享倍速播放
核心特性
完全免费使用DeepSeek网页版进行AI创作
智能章节大纲生成
自动化小说内容生成
角色状态与世界设定管理
实时生成进度监控
本地文件自动保存

技术架构
浏览器环境模拟
应用通过Electron的webview标签嵌入DeepSeek网页版，并通过JavaScript注入实现浏览器环境伪装，确保正常登录和使用。
// 消息发送核心逻辑

[b]async function sendMessageToAI(message) {
[b]    const inputElement = document.querySelector('textarea');
[b]    inputElement.value = message;
[b]    inputElement.dispatchEvent(new Event('input', { bubbles: true }));

[b]    // 模拟回车键提交
[b]    const enterEvent = new KeyboardEvent('keydown', {
[b]        key: 'Enter',
[b]        code: 'Enter',
[b]        keyCode: 13,
[b]        bubbles: true
[b]    });
[b]    inputElement.dispatchEvent(enterEvent);
[b]}
复制代码
内容捕获与段落保留
系统智能识别HTML结构，完整保留AI返回内容的段落格式和换行。

// HTML转文本并保留段落结构

[b]function htmlToText(element) {
[b]    let result = '';
[b]    for (let node of element.childNodes) {
[b]        if (node.nodeType === 3) {
[b]            result += node.textContent;
[b]        } else if (node.nodeType === 1) {
[b]            const tagName = node.tagName.toLowerCase();
[b]            if (tagName === 'br') {
[b]                result += '\n';
[b]            } else if (tagName === 'p' || tagName === 'div') {
[b]                if (result && !result.endsWith('\n')) {
[b]                    result += '\n';
[b]                }
[b]                result += htmlToText(node);
[b]                result += '\n';
[b]            } else {
[b]                result += htmlToText(node);
[b]            }
[b]        }
[b]    }
[b]    return result;
[b]}
复制代码
功能详解

1. 提示词管理

创建和管理小说项目，配置作者角色、创作规则等AI生成参数。支持多项目管理，每个项目独立配置。

2. 章节大纲生成

输入故事创意和主题，AI自动生成章节大纲。支持逐章生成，每次生成一章大纲，确保内容质量。

3. 小说内容生成

基于章节大纲，AI自动生成完整的小说内容。支持以下功能：


使用角色状态进行连贯创作
引用世界设定保持世界观一致
读取前面章节保持情节连贯
自动扩写达到目标字数
智能状态更新建议


4. 设定管理

管理角色状态和世界设定，AI可根据章节内容自动生成更新建议，确保小说世界观的连贯性。

5. 生成进度监控

实时查看AI创作日志，包括：


当前生成进度
章节字数统计
生成耗时记录
错误提示信息


6. 捕获进度查看

内嵌DeepSeek浏览器，可实时查看AI对话过程。添加透明遮罩防止误操作，确保自动化流程稳定运行。



使用指南

第一步：登录DeepSeek

点击左侧"捕获进度"菜单，在内嵌浏览器中登录DeepSeek账号，支持微信扫码登录。

第二步：配置提示词

在"提示词管理"页面创建项目，配置作者角色、创作规则等提示词模板。

第三步：生成章节大纲

在"章节大纲"页面输入故事创意，AI将自动生成章节大纲。

第四步：生成小说内容

在"小说生成"页面选择章节，AI将根据大纲生成完整小说内容。

第五步：查看生成进度

在"生成进度"页面实时查看AI创作日志，所有生成的内容自动保存到本地。



全局状态管理

应用实现了全局生成任务互斥机制，确保同一时间只有一个AI生成任务运行，避免资源冲突和数据混乱。
// 全局生成状态管理

[b]window.generationState = {
[b]    isGenerating: false,
[b]    currentTask: null,

[b]    startGeneration: function(taskType) {
[b]        if (this.isGenerating) {
[b]            return false;
[b]        }
[b]        this.isGenerating = true;
[b]        this.currentTask = taskType;
[b]        this.disableAllButtons();
[b]        return true;
[b]    },

[b]    stopGeneration: function() {
[b]        this.isGenerating = false;
[b]        this.currentTask = null;
[b]        this.enableAllButtons();
[b]    }
[b]};
复制代码


userdata/

└── projects/
    └── [项目名称]/
        ├── storylines/          # 章节大纲
        │   ├── 第1章大纲.json
        │   ├── 第2章大纲.json
        │   └── ...
        ├── chapters/            # 小说章节
        │   ├── [项目名]第1章.txt
        │   ├── [项目名]第2章.txt
        │   └── ...
        ├── state/               # 角色状态
        │   └── character_state.json
        ├── worldbible/          # 世界设定
        │   └── world_bible.json
        └── story_idea.txt       # 故事创意

注意事项


首次使用需要登录DeepSeek账号
不能点击深度思考、联网等选项，如果误选需要在浏览器中关闭后重启应用
生成过程中请勿关闭应用或切换账号
建议在网络稳定的环境下使用
由于使用网页版，生成速度会比API慢一些
偶尔可能出现捕获失败，重试即可




技术栈


Electron 28.1.0
Node.js
JavaScript ES6+
HTML5 / CSS3




开发者信息


开发者：52PJ ID:7631329
版本：2.0.0
开源协议：MIT
v2.0.0 (2025-01-09)
完全重构代码架构
移除API付费模式，改用DeepSeek网页版
实现浏览器环境模拟和自动化操作
优化内容捕获机制，完整保留段落格式
添加全局任务互斥管理
优化用户界面和交互体验
添加登录状态自动检测
修复多项已知问题
