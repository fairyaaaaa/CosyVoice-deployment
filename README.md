# CosyVoice-deployment
# CosyVoice 部署说明

本项目旨在展示如何在 AutoDL 云平台上部署并运行 CosyVoice

## 🧾 部署环境

- 平台：AutoDL 容器
- 操作系统：Ubuntu 22.04
- CUDA 版本：11.8
- GPU：NVIDIA RTX 3090 (24GB)
- CPU：20 vCPU AMD EPYC 7642

---

## 📦 环境搭建

### 1. 克隆项目仓库
```bash
cd ~/autodl-tmp
git clone --recursive https://github.com/FunAudioLLM/CosyVoice.git
cd cosyvoice
```

### 2. 创建并激活 Conda 虚拟环境
```bash
conda create -n cosyvoice python=3.10 -y
conda activate cosyvoice
```
#如果遇见activate不成功可以试试
```bash
source activate
conda deactivate
```

### 3. 安装依赖项
```bash
# 核心依赖
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host=mirrors.aliyun.com

# 安装 pynini（语音相关工具包）
conda install -y -c conda-forge pynini==2.1.5

```

### 4. 下载CosyVoice2-0.5B模型
使用 ModelScope 平台提供的 snapshot_download 工具，将模型下载至本地：
、、、bash
from modelscope import snapshot_download

snapshot_download(
    'iic/CosyVoice2-0.5B',
    local_dir='pretrained_models/CosyVoice2-0.5B'
)
、、、
下载后模型将保存在 pretrained_models/CosyVoice2-0.5B 路径下。

### 5. 下载预训练音色
https://github.com/user-attachments/files/18149385/spk2info.zip
解压后复制spk2info.pt，并放在CosyVoice2-0.5B 路径下

---

## 🔧 启动服务

运行网页演示页面：
```bash
python webui.py --port 50000 --model_dir pretrained_models/CosyVoice2-0.5B
```

---

## ✅ 功能验证

部署完成后，我使用了如下文本进行语音合成测试：
> “我是通义实验室语音团队全新推出的生成式语音大模型，提供舒适自然的语音合成能力。”

系统顺利生成语音，响应时间正常，音频播放无误。
此外，我还体验了 3 秒极速复刻模式，录制了个人声音，并合成了一段自我介绍音频，结果自然流畅，整体表现良好。该音频已保存至项目目录下的 audio/ 文件夹中供参考。

---

## 📌 注意事项

- 如遇 `ffprobe` 错误，请执行：
```bash
apt update && apt install -y ffmpeg
```

---

