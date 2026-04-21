# 測試指南

本檔案說明 Claude How To 的測試基礎設施。

## 概覽

專案使用 GitHub Actions 在每次 push 和 pull request 時自動執行測試。測試覆蓋：

- **單元測試**：使用 pytest 的 Python 測試
- **程式碼質量**：使用 Ruff 做 lint 和格式化
- **安全**：使用 Bandit 做漏洞掃描
- **型別檢查**：使用 mypy 做靜態型別分析
- **構建驗證**：EPUB 生成測試

## 在本地執行測試

### 前置條件

```bash
# 安裝 uv（快速 Python 包管理器）
pip install uv

# 或者在 macOS 上使用 Homebrew
brew install uv
```

### 配置環境

```bash
# 克隆倉庫
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 建立虛擬環境
uv venv

# 啟用虛擬環境
source .venv/bin/activate  # macOS/Linux
# 或者
.venv\Scripts\activate     # Windows

# 安裝開發依賴
uv pip install -r requirements-dev.txt
```

### 執行測試

```bash
# 執行所有單元測試
pytest scripts/tests/ -v

# 執行帶覆蓋率的測試
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行指定測試檔案
pytest scripts/tests/test_build_epub.py -v

# 執行指定測試函式
pytest scripts/tests/test_build_epub.py::test_function_name -v

# 以 watch 模式執行測試（需要 pytest-watch）
ptw scripts/tests/
```

### 執行 lint

```bash
# 檢查程式碼格式
ruff format --check scripts/

# 自動修復格式問題
ruff format scripts/

# 執行 lint
ruff check scripts/

# 自動修復 lint 問題
ruff check --fix scripts/
```

### 執行安全掃描

```bash
# 執行 Bandit 安全掃描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 生成 JSON 報告
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/ -f json -o bandit-report.json
```

### 執行型別檢查

```bash
# 使用 mypy 檢查型別
mypy scripts/ --ignore-missing-imports --no-implicit-optional
```

## GitHub Actions 工作流

### 觸發條件

- 推送到 `main` 或 `develop` 分支（當 `scripts` 有變更時）
- 向 `main` 提交 Pull Request（當 `scripts` 有變更時）
- 手動觸發 workflow

### 作業

#### 1. 單元測試（pytest）

- **執行環境**：Ubuntu latest
- **Python 版本**：3.10、3.11、3.12
- **執行內容**：
  - 從 `requirements-dev.txt` 安裝依賴
  - 執行 pytest 並生成覆蓋率報告
  - 將覆蓋率上傳到 Codecov
  - 歸檔測試結果和 HTML 覆蓋率報告

**結果**：如果任何測試失敗，工作流失敗（關鍵）

#### 2. 程式碼質量（Ruff）

- **執行環境**：Ubuntu latest
- **Python 版本**：3.11
- **執行內容**：
  - 使用 `ruff format` 檢查格式
  - 使用 `ruff check` 執行 lint
  - 報告問題，但不會讓整個工作流失敗

**結果**：非阻塞（僅警告）

#### 3. 安全掃描（Bandit）

- **執行環境**：Ubuntu latest
- **Python 版本**：3.11
- **執行內容**：
  - 掃描安全漏洞
  - 生成 JSON 報告
  - 將報告作為 artifact 上傳

**結果**：非阻塞（僅警告）

#### 4. 型別檢查（mypy）

- **執行環境**：Ubuntu latest
- **Python 版本**：3.11
- **執行內容**：
  - 執行靜態型別分析
  - 報告型別不匹配
  - 幫助儘早發現 bug

**結果**：非阻塞（僅警告）

#### 5. 構建 EPUB

- **執行環境**：Ubuntu latest
- **依賴**：pytest、lint、安全掃描（都必須透過）
- **執行內容**：
  - 使用 `scripts/build_epub.py` 構建 EPUB
  - 驗證 EPUB 是否成功生成
  - 將 EPUB 作為 artifact 上傳

**結果**：如果構建失敗，工作流失敗（關鍵）

#### 6. 總結

- **執行環境**：Ubuntu latest
- **依賴**：所有其他作業
- **執行內容**：
  - 生成工作流總結
  - 列出所有 artifacts
  - 彙總總體狀態

## 編寫測試

### 測試結構

測試應放在 `scripts/tests/` 中，檔名形如 `test_*.py`：

```python
# scripts/tests/test_example.py
import pytest
from scripts.example_module import some_function

def test_basic_functionality():
    """測試 some_function 是否正常工作。"""
    result = some_function("input")
    assert result == "expected_output"

def test_error_handling():
    """測試 some_function 是否能優雅處理錯誤。"""
    with pytest.raises(ValueError):
        some_function("invalid_input")

@pytest.mark.asyncio
async def test_async_function():
    """測試非同步函式。"""
    result = await async_function()
    assert result is not None
```

### 測試最佳實踐

- **使用有描述性的名稱**：例如 `test_function_returns_correct_value()`
- **儘量每個測試只做一個斷言**：更容易排查失敗原因
- **使用 fixture** 複用初始化邏輯：見 `scripts/tests/conftest.py`
- **Mock 外部服務**：使用 `unittest.mock` 或 `pytest-mock`
- **測試邊界情況**：空輸入、None 值、錯誤情況
- **保持測試快速**：避免 `sleep()` 和外部 I/O
- **使用 pytest 標記**：例如 `@pytest.mark.slow` 標記慢測試

### Fixtures

常用 fixture 定義在 `scripts/tests/conftest.py`：

```python
# 在測試中使用 fixture
def test_something(tmp_path):
    """tmp_path fixture 提供臨時目錄。"""
    test_file = tmp_path / "test.txt"
    test_file.write_text("content")
    assert test_file.read_text() == "content"
```

## 覆蓋率報告

### 本地覆蓋率

```bash
# 生成覆蓋率報告
pytest scripts/tests/ --cov=scripts --cov-report=html

# 在瀏覽器中開啟覆蓋率報告
open htmlcov/index.html
```

### 覆蓋率目標

- **最低覆蓋率**：80%
- **分支覆蓋率**：啟用
- **重點區域**：核心功能和錯誤路徑

## Pre-commit Hooks

專案使用 pre-commit hooks 在每次提交前自動執行檢查：

```bash
# 安裝 pre-commit hooks
pre-commit install

# 手動執行 hooks
pre-commit run --all-files

# 跳過某次提交的 hooks（不推薦）
git commit --no-verify
```

在 `.pre-commit-config.yaml` 中配置的 hooks：
- Ruff formatter
- Ruff linter
- Bandit security scanner
- YAML validation
- 檔案大小檢查
- 合併衝突檢測

## 排障

### 本地測試透過，但 CI 失敗

常見原因：
1. **Python 版本差異**：CI 使用 3.10、3.11、3.12
2. **依賴缺失**：更新 `requirements-dev.txt`
3. **平臺差異**：路徑分隔符、環境變數
4. **測試不穩定**：依賴時序或執行順序的測試

解決方案：
```bash
# 使用相同的 Python 版本測試
uv python install 3.10 3.11 3.12

# 使用乾淨環境測試
rm -rf .venv
uv venv
uv pip install -r requirements-dev.txt
pytest scripts/tests/
```

### Bandit 報告誤報

某些安全警告可能是誤報。可在 `pyproject.toml` 中配置：

```toml
[tool.bandit]
exclude_dirs = ["scripts/tests"]
skips = ["B101"]  # 跳過 assert_used 警告
```

### 型別檢查太嚴格

對特定檔案放寬型別檢查：

```python
# 放在檔案頂部
# type: ignore

# 或者針對特定行
some_dynamic_code()  # type: ignore
```

## 持續整合最佳實踐

1. **保持測試快速**：每個測試最好在 1 秒內完成
2. **不要測試外部 API**：用 mock 替代外部服務
3. **測試要隔離**：每個測試都應獨立
4. **斷言要清晰**：寫 `assert x == 5`，不要寫 `assert x`
5. **處理非同步測試**：使用 `@pytest.mark.asyncio`
6. **生成報告**：覆蓋率、安全掃描、型別檢查

## 資源

- [pytest Documentation](https://docs.pytest.org/)
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [Bandit Documentation](https://bandit.readthedocs.io/)
- [mypy Documentation](https://mypy.readthedocs.io/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 貢獻測試

提交 PR 時：

1. **為新功能編寫測試**
2. **本地執行測試**：`pytest scripts/tests/ -v`
3. **檢查覆蓋率**：`pytest scripts/tests/ --cov=scripts`
4. **執行 lint**：`ruff check scripts/`
5. **安全掃描**：`bandit -r scripts/ --exclude scripts/tests/`
6. **如果測試變化，更新檔案**

所有 PR 都必須包含測試！🧪

---

如果你對測試有問題或疑問，請在 GitHub 上建立 issue 或 discussion。
