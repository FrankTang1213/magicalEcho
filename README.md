# 音频字幕

把 MP3、M4A 等录音转成带时间戳的字幕。网页会自动识别录音语言，默认翻译成简体中文，也可以改成其他语言或保留原文。

识别在本机完成。把字幕译成另一种语言时需要联网。

## 环境

- Python 3.11 及以上
- 能访问网络，用于首次下载识别模型，以及翻译字幕

```powershell
python -m venv .venv
.\.venv\Scripts\python -m pip install -r requirements.txt
```

## 启动网页

```powershell
.\.venv\Scripts\python server.py
```

浏览器打开 http://127.0.0.1:8765 。服务默认只监听本机。需要改地址或端口时：

```powershell
.\.venv\Scripts\python server.py --host 127.0.0.1 --port 8765
```

## 使用

1. 填写本机音频路径，或上传一个音频文件。两者都填时，以本机路径为准。
2. 音频语言保持“自动检测”，也可以手动指定。
3. 字幕语言默认是中文（简体）。可以改成其他语言，或选择“保留原语言”。
4. 选择 SRT 或 VTT，然后点击“生成字幕”。
5. 完成后可以下载字幕。输出目录留空时，文件保存在音频同一目录。

文件名示例：

| 音频 | 字幕语言 | 结果 |
|---|---|---|
| `C:\audio\speech.mp3` | 中文（简体） | `C:\audio\speech.zh.srt` |
| `C:\audio\speech.mp3` | 英语 | `C:\audio\speech.en.srt` |
| `C:\audio\speech.m4a` | 保留原语言 | `C:\audio\speech.srt` |

只上传、不填本机路径时，音频和字幕会放在 `workspace/jobs/` 下面，页面仍提供下载。单个上传文件上限是 512 MB。

支持的音频扩展名：`.mp3`、`.m4a`、`.wav`、`.flac`、`.ogg`、`.opus`、`.aac`、`.wma`、`.mp4`、`.webm`、`.aiff`、`.aif`、`.caf`。M4A 里常见的 AAC 和 ALAC 都可以解码。

## 识别与翻译

默认识别模型是 `small`。可选 `tiny`、`base`、`small`、`medium`、`large-v3`。模型越大越慢，也越准确。某个模型第一次使用时会自动下载。

`tiny`、`base`、`small`、`medium` 能识别 99 种语言。`large-v3` 另外支持粤语（`yue`）。常见语言里，中文、英语、日语、韩语，以及西班牙语、法语、德语、葡萄牙语、俄语的效果更好。纯音乐或没有人声的录音不会得到有效字幕。

识别语言和字幕语言相同时，直接使用原文，不再翻译。

## 命令行

命令行只做原语言听写，不翻译。

```powershell
.\.venv\Scripts\python transcribe_audio.py speech.mp3
.\.venv\Scripts\python transcribe_audio.py speech.mp3 -o speech.srt --language zh --model small
.\.venv\Scripts\python transcribe_audio.py speech.mp3 --format vtt
```

不写 `-o` 时，字幕与音频同名，扩展名换成 `.srt`。`-o -` 把字幕打印到终端。

## 测试

```powershell
.\.venv\Scripts\python -m unittest test_transcribe_audio.py test_subtitle_service.py
```
