# Stable Diffusion WebUI Docker 部署指南

## 系統需求

### 硬體
- NVIDIA GPU (推薦 6GB 以上顯存)
- 至少 16GB 內存
- 50GB 可用磁碟空間

### 軟體
- Windows 10/11 或 Linux
- Docker Desktop
- NVIDIA Container Toolkit
- Git

## 安裝步驟

### 1. 安裝前提條件

#### Windows
1. 安裝 NVIDIA GPU 驅動
2. 安裝 Docker Desktop
   - 下載地址：https://www.docker.com/products/docker-desktop
3. 啟用 WSL 2
4. 安裝 NVIDIA Container Toolkit

#### Linux
```bash
# Ubuntu 範例
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

### 2. 克隆倉庫

```bash
git clone https://github.com/your-username/sd-webui-docker.git
cd sd-webui-docker
```

### 3. 配置環境

1. 複製環境變量模板
```bash
cp .env.example .env
```

2. 編輯 `.env` 文件
```ini
# 自定義端口
WEB_PORT=7860

# 模型和輸出目錄
MODEL_VOLUME=./models
OUTPUT_VOLUME=./outputs

# GPU 配置
NVIDIA_VISIBLE_DEVICES=all
```

### 4. 啟動服務

```bash
# 構建並啟動
docker-compose up --build

# 後台運行
docker-compose up -d --build
```

## 使用指南

### 訪問 WebUI
- 瀏覽器打開：`http://localhost:7860`

### 模型管理

1. 模型存儲位置：`models/` 目錄
2. 支持的模型格式：
   - `.ckpt`
   - `.safetensors`
   - `.pt`

### 常見模型下載網站
- Civitai: https://civitai.com/
- HuggingFace: https://huggingface.co/
- Stability AI: https://stability.ai/

### 性能優化建議
- 使用 `.safetensors` 格式
- 選擇適合顯卡的模型
- 調整 WebUI 設置中的顯存優化選項

## 高級配置

### 自定義啟動參數

在 `.env` 中修改 `EXTRA_ARGS`：
```ini
EXTRA_ARGS=--precision full --no-half --xformers
```

### 常用啟動參數
- `--precision full`：提高精度
- `--no-half`：禁用半精度
- `--xformers`：啟用 xformers 加速
- `--medvram`：顯存優化
- `--lowvram`：低顯存模式

## 常見問題排查

1. 映像無法構建
   - 檢查網絡連接
   - 更新 Docker 和 NVIDIA 驅動
   
2. WebUI 無法啟動
   - 檢查 GPU 驅動
   - 確認 NVIDIA Container Toolkit 正確安裝
   
3. 顯存不足
   - 使用 `--medvram` 或 `--lowvram`
   - 關閉不必要的擴展
   - 減小批次大小

## 安全與隱私

- 不要在公開環境長期暴露 WebUI
- 使用防火牆限制訪問
- 定期更新模型和 Docker 映像

## 許可證

本項目基於 [選擇合適的開源許可證]，遵循上游倉庫的許可條款。

## 社區與支持

- GitHub Issues: [倉庫地址]/issues
- Discord: [Stable Diffusion 社區邀請連結]

## 貢獻指南

1. Fork 倉庫
2. 創建特性分支 `git checkout -b feature/amazing-feature`
3. 提交更改 `git commit -m '增加了某些特性'`
4. 推送到分支 `git push origin feature/amazing-feature`
5. 提交 Pull Request

---

**免責聲明**：本項目僅供學習和研究使用，請遵守相關法律法規，尊重版權和倫理。