# No-Code Architects Toolkit - Zeabur 一键部署模板

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/00M7JS?referralCode=xiangyugongzuoliu)

> 强大的媒体处理 API 工具包，提供 50+ 个 API 端点，支持视频字幕生成、音视频转录、FFmpeg 处理等功能。

## ✨ 核心功能

- 🎬 **自动视频字幕生成** - 支持简体中文、繁体中文等多语言
- 🎤 **AI 音视频转录** - 基于 OpenAI Whisper 的高质量转录
- 🎞️ **FFmpeg 音视频处理** - 格式转换、剪辑、拼接等操作
- ☁️ **云存储集成** - 支持 Cloudflare R2、AWS S3、GCP Storage
- 📦 **二进制文件上传** - 直接上传大文件到云存储
- 🌏 **完整中文字体支持** - 简体、繁体中文渲染

## 🚀 快速开始

### 1. 点击上方 Deploy 按钮

点击 "Deploy on Zeabur" 按钮开始部署。

### 2. 配置必需的环境变量

#### 基础配置（必填）

- **API_KEY**: 你的 API 密钥，用于保护所有端点（建议使用 16 位以上强密码）

#### 云存储配置（三选一）

**选项 A: Cloudflare R2（推荐，零出口费用）**
- `S3_ENDPOINT_URL`: 你的 R2 端点（如 `https://abc123.r2.cloudflarestorage.com`）
- `S3_ACCESS_KEY`: R2 Access Key ID
- `S3_SECRET_KEY`: R2 Secret Access Key
- `S3_BUCKET_NAME`: R2 Bucket 名称
- `S3_REGION`: 填写 `auto`
- `R2_PUBLIC_DOMAIN`: （可选）自定义域名

**选项 B: AWS S3 或兼容存储**
- `S3_ENDPOINT_URL`: S3 端点 URL
- `S3_ACCESS_KEY`: Access Key ID
- `S3_SECRET_KEY`: Secret Access Key
- `S3_BUCKET_NAME`: Bucket 名称
- `S3_REGION`: 区域（如 `us-east-1`）

**选项 C: Google Cloud Storage**
- `GCP_BUCKET_NAME`: GCS Bucket 名称
- `GCP_SA_CREDENTIALS`: 服务账号 JSON 凭证（完整 JSON 字符串）

### 3. 等待部署完成

部署通常需要 2-3 分钟。完成后：
- 访问根路径 `/` 查看项目首页和完整文档
- 使用 `/v1/*` 路径调用 API 端点

## 📖 使用示例

### 测试 API 连接

```bash
curl -X GET https://your-domain.zeabur.app/v1/toolkit/test \
  -H "X-API-Key: YOUR_API_KEY"
```

### 音视频转录

```bash
curl -X POST https://your-domain.zeabur.app/v1/media/transcribe \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "media_url": "https://example.com/audio.mp3",
    "task": "transcribe",
    "language": "zh"
  }'
```

### 视频添加字幕

```bash
curl -X POST https://your-domain.zeabur.app/v1/video/caption \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://example.com/video.mp4",
    "subtitles": [
      {"start": 0, "end": 2, "text": "你好"},
      {"start": 2, "end": 4, "text": "世界"}
    ],
    "response_type": "cloud"
  }'
```

## 🔧 性能调优（可选）

根据你的使用场景，可以调整以下参数：

- **GUNICORN_WORKERS**: Worker 进程数（推荐 2-4）
- **GUNICORN_TIMEOUT**: 请求超时时间（秒，推荐 300-600）
- **MAX_QUEUE_LENGTH**: 队列最大长度（0 表示无限制）

## 📚 完整 API 文档

部署后访问 `https://your-domain.zeabur.app/` 查看：
- 完整的 API 端点列表（50+ 个）
- 详细的使用指南
- 代码示例
- 技术栈说明

---

Fork 自 [stephengpope/no-code-architects-toolkit](https://github.com/stephengpope/no-code-architects-toolkit)

**维护者**: [翔宇工作流](https://xiangyugongzuoliu.com/)
