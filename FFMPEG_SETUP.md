# FFmpeg 设置说明

## 概述
本项目已将 FFmpeg 配置为使用项目内的相对路径，无需系统安装。

## 目录结构
```
项目根目录/
├── ffmpeg/
│   ├── ffmpeg.exe    # FFmpeg 主程序
│   └── ffprobe.exe   # 媒体信息探测工具
└── .env              # 配置文件
```

## 配置说明

### 环境变量配置 (.env)
```bash
FFMPEG_PATH=./ffmpeg/ffmpeg.exe
```

### 代码中的路径配置
所有服务都已更新为使用相对路径：
- `app/services/video_processor.py`
- `app/services/tencent_asr_sdk.py`
- `app/services/xunfei_asr_service.py`
- `app/utils/subtitle_burner.py`
- `app/utils/beautify_basic.py`
- `app/pipeline/v2_pipeline.py`

## 部署说明

### 新环境部署
1. 克隆项目代码
2. 复制 FFmpeg 文件到 `ffmpeg/` 目录：
   ```bash
   mkdir ffmpeg
   # 复制 ffmpeg.exe 和 ffprobe.exe 到 ffmpeg/ 目录
   ```
3. 确保 `.env` 文件中的路径配置正确

### FFmpeg 文件获取
- Windows: 从 https://ffmpeg.org/download.html 下载
- 需要的文件：`ffmpeg.exe`, `ffprobe.exe`
- 版本：推荐使用 FFmpeg 8.0 或更高版本

## 注意事项

1. **Git 忽略**: FFmpeg 二进制文件已添加到 `.gitignore`，不会提交到版本控制
2. **路径兼容**: 代码支持跨平台，Windows 使用项目路径，Linux/Mac 使用系统路径
3. **文件大小**: FFmpeg 文件较大（约100MB），部署时需要单独处理

## 验证安装
```bash
# 测试 FFmpeg 是否正常工作
./ffmpeg/ffmpeg.exe -version

# 测试配置是否正确加载
python -c "import os; from dotenv import load_dotenv; load_dotenv(); print('FFMPEG_PATH:', os.getenv('FFMPEG_PATH'))"
```

## 故障排除

### 常见问题
1. **文件不存在错误**: 确保 `ffmpeg.exe` 在正确位置
2. **权限问题**: 确保 FFmpeg 文件有执行权限
3. **路径问题**: 检查 `.env` 文件中的路径配置

### 日志检查
字幕处理过程中会生成详细日志，包括：
- 音频提取状态
- 语音识别进度
- 字幕烧录结果