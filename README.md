# A股 AI 选股模型（Qlib + LightGBM）

这是一个基于 [Microsoft Qlib](https://github.com/microsoft/qlib) 的 A 股机器学习选股示例。

## 模型做什么

- 股票池：沪深 300（CSI300）
- 特征：Qlib Alpha158
- 模型：LightGBM
- 目标：预测股票未来收益的相对排序
- 策略：选择预测分数最高的 50 只股票，并按日替换其中 5 只
- 输出：预测分数、信号分析和历史回测结果

> 本项目仅用于学习和研究，不构成任何投资建议。历史回测不代表未来收益。

## 环境

建议使用 Python 3.10–3.12，并在独立虚拟环境中运行。

```bash
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## 准备中国市场数据

Qlib 官方说明目前推荐使用社区数据：

```bash
wget https://github.com/chenditc/investment_data/releases/latest/download/qlib_bin.tar.gz
mkdir -p ~/.qlib/qlib_data/cn_data
tar -zxvf qlib_bin.tar.gz -C ~/.qlib/qlib_data/cn_data --strip-components=1
```

Windows 用户可手动下载并解压到：

```text
%USERPROFILE%\.qlib\qlib_data\cn_data
```

## 训练与回测

```bash
qrun workflow_config_lightgbm_Alpha158.yaml
```

Qlib 会训练 LightGBM 模型、生成每只股票的预测分数，并对 Top-K 选股策略进行回测。实验记录默认由 MLflow 管理。

## 调整参数

编辑 `workflow_config_lightgbm_Alpha158.yaml`：

- `market`：股票池
- `topk`：每天持有的股票数量
- `n_drop`：每天替换的股票数量
- `train / valid / test`：训练、验证和测试区间
- `model.kwargs`：LightGBM 参数

## 项目来源

工作流配置基于 Microsoft Qlib 的官方 LightGBM + Alpha158 示例，按照 MIT 许可证使用。详情见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
