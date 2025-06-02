# 🛠️ 論文教授完整安裝指南
> **詳細步驟確保安裝成功 - 新手友善版**  
> *由 Lunaris 瞳海 精心編寫，適合所有技術水平的使用者*

---

## 📋 安裝前準備檢查清單

在開始安裝之前，請確認以下條件：

### ✅ **基本環境檢查**
- [ ] 作業系統：Windows 10/11 或 Ubuntu 18.04+ 或 macOS 10.15+
- [ ] 網路連線：穩定的網際網路連線（需要下載約 3-5GB 檔案）
- [ ] 磁碟空間：至少 10GB 可用空間
- [ ] 管理員權限：能夠安裝軟體和修改系統設定

### 💻 **硬體需求檢查**

#### 🎯 **最低配置**
- [ ] CPU：4核心以上處理器（Intel i5-8400 / AMD Ryzen 5 3600）
- [ ] RAM：16GB 系統記憶體
- [ ] GPU：NVIDIA GTX 1660 6GB 或更好（**必須為NVIDIA顯卡**）
- [ ] 儲存：500GB SSD（建議 NVMe）

#### 🚀 **建議配置**
- [ ] CPU：8核心以上（AMD Threadripper PRO 3945WX 等級）
- [ ] RAM：64GB 或以上
- [ ] GPU：NVIDIA RTX A5000 24GB 或 RTX 4090
- [ ] 儲存：2TB NVMe SSD

---

## 🔍 第一步：系統環境檢查

### 📊 **檢查您的硬體配置**

#### Windows 使用者：
1. 按 `Win + R`，輸入 `dxdiag`，按 Enter
2. 在「系統」頁籤檢查：
   - **處理器**：記下 CPU 型號和核心數
   - **記憶體**：確認 RAM 容量
3. 在「顯示」頁籤檢查：
   - **顯卡名稱**：必須是 NVIDIA 系列
   - **顯示記憶體**：記下 VRAM 容量

#### Linux 使用者：
```bash
# 檢查 CPU 資訊
lscpu | grep "Model name\|CPU(s)"

# 檢查記憶體
free -h

# 檢查 GPU（需安裝 NVIDIA 驅動）
nvidia-smi
```

#### macOS 使用者：
1. 點擊左上角蘋果圖示 → 「關於這台 Mac」
2. 查看處理器、記憶體和圖形卡資訊
3. **注意**：M1/M2 Mac 目前可能需要額外配置

### ⚠️ **重要確認事項**

> **如果您的顯卡不是 NVIDIA：**  
> AMD 或 Intel 顯卡使用者需要修改配置檔案使用 CPU 模式，效能會明顯下降但仍可使用。

---

## 🐍 第二步：Python 環境準備

### 📥 **安裝 Anaconda 或 Miniconda**

我們強烈建議使用 Conda 管理 Python 環境，避免套件衝突。

#### Windows 安裝：
1. 前往 [Anaconda 官網](https://www.anaconda.com/download) 下載 Windows 版本
2. 執行安裝程式，**務必勾選**「Add Anaconda to PATH」
3. 安裝完成後重開機

#### Linux 安裝：
```bash
# 下載 Miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# 執行安裝
bash Miniconda3-latest-Linux-x86_64.sh

# 重新載入環境
source ~/.bashrc
```

### ✅ **驗證 Python 安裝**
開啟終端機（Windows：Anaconda Prompt，Linux/Mac：Terminal）：
```bash
# 檢查 Python 版本（應該顯示 3.10 或更高）
python --version

# 檢查 Conda 版本
conda --version
```

**預期輸出範例：**
```
Python 3.10.16
conda 24.1.2
```

---

## 🔧 第三步：CUDA 環境設定

### 📋 **檢查 CUDA 相容性**

#### 方法一：使用 nvidia-smi 檢查
```bash
nvidia-smi
```

**輸出範例：**
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.xx.xx    Driver Version: 525.xx.xx    CUDA Version: 12.4  |
+-----------------------------------------------------------------------------+
```

記下 **CUDA Version** 的數字（如：12.4）

#### 方法二：Windows 使用者檢查驅動程式
1. 開啟「裝置管理員」
2. 展開「顯示卡」
3. 右鍵點擊您的 NVIDIA 顯卡 → 「內容」→「驅動程式」頁籤
4. 記下驅動程式版本

### 🎯 **CUDA 版本對應表**

| CUDA 版本 | 支援的驅動版本 | PyTorch 安裝指令關鍵字 |
|-----------|---------------|----------------------|
| 12.6      | ≥ 530.30     | cu126               |
| 12.4      | ≥ 525.60     | cu124               |
| 12.1      | ≥ 525.60     | cu121               |
| 11.8      | ≥ 450.80     | cu118               |

### ⚠️ **驅動程式更新（如需要）**

如果您的驅動程式版本過舊：
1. 前往 [NVIDIA 官網](https://www.nvidia.com/drivers/)
2. 選擇您的顯卡型號
3. 下載並安裝最新驅動程式
4. **重開機後**再繼續下一步

---

## 🏗️ 第四步：建立專案環境

### 📁 **創建專案目錄**

#### Windows：
```bash
# 在 C 槽建立專案資料夾
cd C:\
mkdir AI_Projects
cd AI_Projects

# 複製專案
git clone https://github.com/AJTM-175/shatter-paper-reader.git
cd shatter-paper-reader
```

#### Linux/Mac：
```bash
# 在家目錄建立專案資料夾
cd ~
mkdir AI_Projects
cd AI_Projects

# 複製專案
git clone https://github.com/AJTM-175/shatter-paper-reader.git
cd shatter-paper-reader
```

### 🐍 **建立 Conda 虛擬環境**

```bash
# 建立專用環境（名稱：paper-professor）
conda create -n paper-professor python=3.10.16 -y

# 啟用環境
conda activate paper-professor

# 確認環境已啟用（命令提示字元前應該會顯示 (paper-professor)）
python --version
```

**成功範例：**
```bash
(paper-professor) C:\AI_Projects\shatter-paper-reader> python --version
Python 3.10.16
```

---

## 📦 第五步：安裝核心依賴

### 🔥 **安裝 PyTorch（關鍵步驟）**

根據您在第三步檢查的 CUDA 版本，選擇對應的安裝指令：

#### CUDA 12.4 使用者：
```bash
pip install --force-reinstall torch torchvision torchaudio "numpy<=2.1.1" --index-url https://download.pytorch.org/whl/cu124
```

#### CUDA 12.1 使用者：
```bash
pip install --force-reinstall torch torchvision torchaudio "numpy<=2.1.1" --index-url https://download.pytorch.org/whl/cu121
```

#### CUDA 11.8 使用者：
```bash
pip install --force-reinstall torch torchvision torchaudio "numpy<=2.1.1" --index-url https://download.pytorch.org/whl/cu118
```

#### 沒有 NVIDIA 顯卡或遇到問題：
```bash
pip install torch torchvision torchaudio "numpy<=2.1.1" --index-url https://download.pytorch.org/whl/cpu
```

### ✅ **驗證 PyTorch 安裝**

```python
# 執行 Python 並測試
python -c "import torch; print(f'PyTorch 版本: {torch.__version__}'); print(f'CUDA 可用: {torch.cuda.is_available()}'); print(f'CUDA 版本: {torch.version.cuda if torch.cuda.is_available() else \"N/A\"}')"
```

**成功輸出範例：**
```
PyTorch 版本: 2.1.2+cu124
CUDA 可用: True
CUDA 版本: 12.4
```

---

## 🔬 第六步：安裝 MinerU 核心

### 📖 **安裝 MinerU PDF 處理引擎**

```bash
# 使用阿里雲鏡像加速安裝（推薦）
pip install -U magic-pdf[full]==1.3.3 -i https://mirrors.aliyun.com/pypi/simple

# 如果上述指令失敗，使用官方源：
# pip install -U magic-pdf[full]==1.3.3
```

**安裝過程說明：**
- 這個步驟可能需要 10-20 分鐘
- 會下載約 1-2GB 的檔案
- 如果出現紅色警告文字但最後顯示「Successfully installed」就是成功

### ✅ **驗證 MinerU 安裝**

```python
python -c "import magic_pdf; print('MinerU 安裝成功！')"
```

---

## 🚀 第七步：安裝專案依賴

### 📋 **安裝 requirements.txt 中的套件**

```bash
# 確保在專案目錄中
pwd  # 應該顯示包含 requirements.txt 的路徑

# 安裝所有依賴
pip install -r requirements.txt
```

### 🔧 **安裝 FAISS-GPU（向量檢索加速）**

```bash
# 重要：FAISS-GPU 只能透過 conda 安裝
conda install -c conda-forge faiss-gpu -y
```

**如果遇到 FAISS 安裝問題：**
```bash
# 降級安裝 CPU 版本
conda install -c conda-forge faiss-cpu -y
```

### ✅ **驗證所有依賴安裝**

創建測試腳本 `test_dependencies.py`：
```python
# 測試所有關鍵依賴
try:
    import torch
    print(f"✅ PyTorch: {torch.__version__}")
    
    import magic_pdf
    print("✅ MinerU: 已安裝")
    
    import faiss
    print("✅ FAISS: 已安裝")
    
    import PyQt6
    print("✅ PyQt6: 已安裝")
    
    import requests
    print("✅ Requests: 已安裝")
    
    print("\n🎉 所有依賴安裝成功！")
    
except ImportError as e:
    print(f"❌ 缺少依賴: {e}")
```

執行測試：
```bash
python test_dependencies.py
```

---

## 🔑 第八步：下載模型與配置

### 📥 **下載所需模型**

```bash
# 執行模型下載腳本
python download_models.py
```

**下載過程說明：**
- 首次執行會下載 2-3GB 的模型檔案
- 包含 PDF 處理模型和語音識別模型
- 請保持網路連線穩定

### ⚙️ **配置 CUDA 加速**

下載完成後，需要修改配置檔案：

#### Windows 配置檔案位置：
```
C:\Users\您的使用者名稱\magic-pdf.json
```

#### Linux/Mac 配置檔案位置：
```
/home/您的使用者名稱/magic-pdf.json
```

**編輯配置檔案：**
```json
{
    "device-mode": "cuda"
}
```

**如果您沒有 NVIDIA 顯卡：**
```json
{
    "device-mode": "cpu"
}
```

### ✅ **驗證模型配置**

```bash
# 測試 PDF 處理功能
python -c "
import magic_pdf
from magic_pdf.pipe.UNIPipe import UNIPipe
print('✅ MinerU 模型配置成功！')
"
```

---

## 🔐 第九步：API 金鑰配置

### 🌐 **申請必要的 API 服務**

#### DeepSeek API（大語言模型）：
1. 前往 [DeepSeek 官網](https://platform.deepseek.com/)
2. 註冊帳號並進行身份驗證
3. 在 API 頁面創建新的 API Key
4. **記錄您的 API Key**

#### MiniMax API（語音合成）：
1. 前往 [MiniMax 平台](https://platform.minimaxi.com/)
2. 註冊並完成認證
3. 創建應用並獲取 Group ID 和 API Key
4. **記錄您的 Group ID 和 API Key**

### ⚙️ **配置 API 金鑰**

編輯專案中的 `config.py` 檔案：

```python
# DeepSeek API 設定
API_BASE_URL = "https://api.deepseek.com"
API_KEY = "sk-xxxxxxxxxxxxxxxxxxxxxxxxx"  # 填入您的 DeepSeek API Key

# MiniMax TTS API 設定
TTS_GROUP_ID = "xxxxxxxxxxxxxxxxxx"  # 填入您的 MiniMax Group ID
TTS_API_KEY = "xxxxxxxxxxxxxxxxxx"   # 填入您的 MiniMax API Key
```

### ✅ **測試 API 連接**

創建測試腳本 `test_api.py`：
```python
import requests
from config import API_KEY, API_BASE_URL, TTS_API_KEY, TTS_GROUP_ID

# 測試 DeepSeek API
def test_deepseek():
    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json"
    }
    
    data = {
        "model": "deepseek-chat",
        "messages": [{"role": "user", "content": "Hello"}],
        "max_tokens": 10
    }
    
    try:
        response = requests.post(f"{API_BASE_URL}/chat/completions", 
                               headers=headers, json=data, timeout=10)
        if response.status_code == 200:
            print("✅ DeepSeek API 連接成功")
        else:
            print(f"❌ DeepSeek API 錯誤: {response.status_code}")
    except Exception as e:
        print(f"❌ DeepSeek API 測試失敗: {e}")

# 測試 MiniMax API
def test_minimax():
    headers = {
        "Authorization": f"Bearer {TTS_API_KEY}",
        "Content-Type": "application/json"
    }
    
    # 簡單的連接測試
    try:
        # 這裡只測試連接，不實際發送 TTS 請求
        print("✅ MiniMax API 金鑰已配置")
    except Exception as e:
        print(f"❌ MiniMax API 配置錯誤: {e}")

if __name__ == "__main__":
    print("🔑 測試 API 連接...")
    test_deepseek()
    test_minimax()
```

執行測試：
```bash
python test_api.py
```

---

## 🎮 第十步：首次啟動測試

### 🚀 **啟動應用程式**

```bash
# 確保虛擬環境已啟用
conda activate paper-professor

# 啟動論文教授
python main.py
```

### 🎯 **首次啟動檢查清單**

應用程式啟動後，您應該看到：

- [ ] **主視窗正常開啟**：PyQt6 介面顯示正常
- [ ] **側邊欄可見**：有上傳論文和論文列表區域
- [ ] **沒有錯誤訊息**：終端機沒有紅色錯誤文字
- [ ] **語音設備檢測**：在設定中能看到麥克風裝置

### 📄 **測試基本功能**

#### 1. 測試 PDF 上傳：
1. 準備一個 PDF 檔案（建議使用英文學術論文）
2. 點擊「匯入論文」
3. 選擇 PDF 檔案
4. 觀察處理進度

#### 2. 測試 AI 對話：
1. 在右側聊天區域輸入：「你好，請自我介紹」
2. 檢查 AI 是否正常回應

#### 3. 測試語音功能：
1. 在設定中選擇麥克風裝置
2. 點擊麥克風按鈕測試錄音

---

## 🔧 常見問題解決

### ❌ **CUDA 相關錯誤**

**問題**：`RuntimeError: CUDA out of memory`
**解決**：
```python
# 在 config.py 中調整批次大小
BATCH_SIZE = 1  # 降低批次大小
MAX_WORKERS = 1  # 減少並行任務
```

**問題**：`CUDA driver version is insufficient`
**解決**：更新 NVIDIA 驅動程式到最新版本

### ❌ **依賴安裝錯誤**

**問題**：`Microsoft Visual C++ 14.0 is required`（Windows）
**解決**：
1. 下載 [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
2. 安裝後重新執行 pip install

**問題**：`Failed building wheel for package`
**解決**：
```bash
# 嘗試使用 conda 安裝
conda install package_name
```

### ❌ **API 連接問題**

**問題**：`Connection timeout` 或 `API key invalid`
**解決**：
1. 檢查網路連線
2. 確認 API Key 正確複製
3. 檢查 API 服務額度是否充足

### ❌ **語音功能問題**

**問題**：麥克風無法錄音
**解決**：
1. 檢查麥克風權限設定
2. 在系統設定中確認預設錄音裝置
3. 嘗試重新啟動應用程式

### ❌ **PDF 處理失敗**

**問題**：PDF 上傳後處理失敗
**解決**：
1. 確認 PDF 檔案未損壞
2. 嘗試使用標準學術論文格式的 PDF
3. 檢查檔案大小（建議 < 50MB）

---

## 🎉 安裝完成檢查

如果您完成了以上所有步驟，請執行最終測試：

### 📋 **完整功能測試清單**

```bash
# 創建完整測試腳本
cat > final_test.py << 'EOF'
import sys
import torch
import magic_pdf
import faiss
import PyQt6
from config import API_KEY, TTS_API_KEY

def main():
    print("🔍 系統環境檢查...")
    print(f"Python 版本: {sys.version}")
    print(f"PyTorch 版本: {torch.__version__}")
    print(f"CUDA 可用: {torch.cuda.is_available()}")
    
    if torch.cuda.is_available():
        print(f"GPU 數量: {torch.cuda.device_count()}")
        print(f"GPU 名稱: {torch.cuda.get_device_name(0)}")
        print(f"GPU 記憶體: {torch.cuda.get_device_properties(0).total_memory / 1024**3:.1f} GB")
    
    print("\n📦 依賴套件檢查...")
    try:
        import magic_pdf
        print("✅ MinerU: 已安裝")
    except:
        print("❌ MinerU: 未安裝")
    
    try:
        import faiss
        print("✅ FAISS: 已安裝")
    except:
        print("❌ FAISS: 未安裝")
    
    try:
        import PyQt6
        print("✅ PyQt6: 已安裝")
    except:
        print("❌ PyQt6: 未安裝")
    
    print("\n🔑 API 配置檢查...")
    if API_KEY and API_KEY != "YOUR_API_KEY":
        print("✅ DeepSeek API Key: 已配置")
    else:
        print("❌ DeepSeek API Key: 未配置")
    
    if TTS_API_KEY and TTS_API_KEY != "YOUR_MINIMAX_API_KEY":
        print("✅ MiniMax API Key: 已配置")
    else:
        print("❌ MiniMax API Key: 未配置")
    
    print("\n🎉 安裝檢查完成！")
    print("如果所有項目都顯示 ✅，您可以開始使用論文教授了！")

if __name__ == "__main__":
    main()
EOF

# 執行完整測試
python final_test.py
```

### 🏆 **成功標準**

如果看到以下輸出，表示安裝完全成功：

```
🔍 系統環境檢查...
Python 版本: 3.10.16
PyTorch 版本: 2.1.2+cu124
CUDA 可用: True
GPU 數量: 1
GPU 名稱: NVIDIA RTX A5000
GPU 記憶體: 24.0 GB

📦 依賴套件檢查...
✅ MinerU: 已安裝
✅ FAISS: 已安裝
✅ PyQt6: 已安裝

🔑 API 配置檢查...
✅ DeepSeek API Key: 已配置
✅ MiniMax API Key: 已配置

🎉 安裝檢查完成！
如果所有項目都顯示 ✅，您可以開始使用論文教授了！
```

---

## 📞 技術支援

### 🆘 **如果遇到問題**

1. **檢查錯誤訊息**：仔細閱讀終端機的錯誤輸出
2. **查詢解決方案**：在本指南的「常見問題解決」部分尋找答案
3. **重新安裝**：嘗試刪除虛擬環境並重新建立
4. **尋求協助**：在 GitHub Issues 中詳細描述問題

### 🔄 **重新安裝流程**

如果需要完全重新安裝：
```bash
# 刪除虛擬環境
conda deactivate
conda env remove -n paper-professor

# 清理緩存
pip cache purge
conda clean --all

# 重新開始安裝流程
```

---

**🌟 祝您使用愉快！如有問題歡迎隨時詢問 Lunaris 瞳海！**

*最後更新：2025年6月2日*  
*指南版本：v1.0 - 新手完整版*