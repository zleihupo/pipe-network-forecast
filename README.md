# pipe-network-forecast

基于多源管网时序数据的液位预测、异常识别与成因分析。完整方法与结果见 [`report/report.pdf`](report/report.pdf)。

## 项目内容

| 任务 | 方法 | 结论 |
|---|---|---|
| 液位预测（Q1） | SARIMAX(Fourier) vs LightGBM，含 ADF 检验、滞后相关、昼夜节律特征 | 梯度提升模型在准确性、波动率、漂移三项指标上均优于传统时序模型 |
| 异常识别（Q2） | 基于历史昼夜节律（hourly mean/std + z-score）+ 连续异常点判定 | 对非正常工作状态的异常数据区分能力好 |
| 多变量预测与成因（inflow loading） | Ridge / LightGBM 多输出回归 + SHAP 解释 | 高值阶段各观测变量最主要的影响采集点稳定且取值一致 |

## 目录结构

```
├── notebooks/
│   ├── 01_load_sta_met.ipynb            # 数据加载与训练/预测期切分
│   ├── 02_waterlevel_forecast_Q1.ipynb  # 液位预测：SARIMAX vs LightGBM
│   ├── 03_anomaly_detection_Q2.ipynb    # 异常识别：昼夜节律基线 + z-score
│   └── 04_inflow_loading_analysis.ipynb # 多变量预测 + 特征相关性 + SHAP
├── report/
│   ├── report.pdf                       # 完整报告（19 页）
│   └── figures/                         # 报告插图
├── data/                                # 数据文件说明（不含数据）
└── requirements.txt
```

## 运行

```bash
pip install -r requirements.txt
jupyter lab notebooks/02_waterlevel_forecast_Q1.ipynb
```

notebook 最初在 Google Colab 上开发（数据经 `files.upload()` 上传），本地运行时把数据文件放到 `data/` 并自行调整读取路径即可。

## 环境

Python 3.9，依赖见 `requirements.txt`。

## 说明

- 仓库不含任何数据文件，数据准备见 [`data/README.md`](data/README.md)。
- 报告中的指标（波动率 / Spike Ratio / Drift）定义见 notebook 02 末尾三个函数。
