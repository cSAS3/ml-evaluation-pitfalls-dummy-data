# ダミーデータで学ぶ機械学習評価の落とし穴

生命健康科学分野における機械学習・データサイエンス教育のための最小実験教材です。  
このリポジトリは，紀要原稿「ダミーデータで学ぶ機械学習評価の落とし穴―生命健康科学分野における教育用最小実験の設計―」で用いたダミーデータ生成コード，Pythonノートブック，図表，集計結果を再現するためのものです。

## Google Colab で実行

以下から実行できます。

https://colab.research.google.com/github/cSAS3/ml-evaluation-pitfalls-dummy-data/blob/main/ml_evaluation_pitfalls_dummy_data.ipynb

## 扱う3つの落とし穴

1. **被験者リーク**  
   同一被験者の繰り返し測定が学習集合と評価集合の両方に入ると，モデルが被験者固有の特徴を記憶し，高い精度に見えることを示します。

2. **ラベル・プロキシリーク**  
   正解ラベルにほぼ直結する特徴量が説明変数に混入すると，AUCが不自然に1.0近傍まで上昇することを示します。

3. **補間と外挿の差**  
   学習範囲内では妥当に見えるモデルが，学習範囲外では大きく破綻しうることを示します。

## 期待される集計結果

20個の乱数種で再現すると，概ね以下の値になります。

| 実験 | 条件 | 指標 | 平均 ± 標準偏差 |
|---|---|---:|---:|
| 被験者リーク | Sample-wise CV | Accuracy | 0.946 ± 0.013 |
| 被験者リーク | Subject-wise CV | Accuracy | 0.519 ± 0.057 |
| ラベル・プロキシリーク | Signal only | AUC | 0.769 ± 0.014 |
| ラベル・プロキシリーク | Signal + leaked proxy | AUC | 1.000 ± 0.000 |
| 補間・外挿 | Linear regression - Interpolation | RMSE | 0.793 ± 0.041 |
| 補間・外挿 | Linear regression - Extrapolation | RMSE | 0.822 ± 0.074 |
| 補間・外挿 | Random forest - Interpolation | RMSE | 0.964 ± 0.060 |
| 補間・外挿 | Random forest - Extrapolation | RMSE | 5.737 ± 0.530 |

## ローカルでの実行方法

```bash
git clone https://github.com/cSAS3/ml-evaluation-pitfalls-dummy-data.git
cd ml-evaluation-pitfalls-dummy-data
python -m venv .venv
source .venv/bin/activate  # Windowsでは .venv\Scripts\activate
pip install -r requirements.txt
python src/generate_dummy_data_and_figures.py
```

実行後，`data/`, `figures/`, `results/` に出力が保存されます。

## 構成

```text
ml-evaluation-pitfalls-dummy-data/
├── README.md
├── requirements.txt
├── CITATION.cff
├── data/
│   ├── subject_leakage_dummy_seed0.csv
│   ├── label_proxy_leakage_dummy_seed0.csv
│   ├── extrapolation_train_seed0.csv
│   ├── extrapolation_test_interpolation_seed0.csv
│   └── extrapolation_test_extrapolation_seed0.csv
├── figures/
│   ├── fig1_workflow.png
│   ├── fig2_subject_leakage.png
│   ├── fig3_target_leakage.png
│   └── fig4_extrapolation.png
├── notebooks/
│   └── ml_evaluation_pitfalls_dummy_data.ipynb
├── results/
│   ├── subject_leakage_scores_20seeds.csv
│   ├── label_proxy_leakage_scores_20seeds.csv
│   ├── extrapolation_scores_20seeds.csv
│   ├── summary_metrics_20seeds.csv
│   └── summary_metrics_20seeds.json
└── src/
    └── generate_dummy_data_and_figures.py
```

## 倫理・個人情報

収録データはすべて乱数から生成した教育用ダミーデータです。実在の被験者，患者，学生，動物に由来する情報は含みません。

## Citation

```text
Shintani SA. Learning Pitfalls in Machine-Learning Evaluation with Synthetic Data:
Design of Minimal Teaching Experiments for the Life and Health Sciences.
Chubu University Institute of Life and Health Sciences Bulletin, 2026.
```

## License
Apache-2.0 license
