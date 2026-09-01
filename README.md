# Retail Sales Analysis(小売売上分析プロジェクト)

## 概要

Kaggleで公開されている「Retail Sales Dataset」(シミュレーションデータ、CC0ライセンス)を用いて、PostgreSQLとSQLによるデータ分析、およびTableauによる可視化を行ったプロジェクトです。顧客・商品・店舗・取引の4テーブルをJOINし、利益率・地域・時系列・顧客セグメントなど多角的な視点から分析しました。

- **データソース**:[Retail Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/buharishehu/retail-sales-dataset)
- **使用技術**:PostgreSQL, SQL, Python(pandas, SQLAlchemy), Jupyter Notebook, Tableau
- **通貨単位について**:元データセットに通貨単位の明記はないため、本分析では数値をそのまま扱っています

## Tableauダッシュボード

**[Retail Sales Analysis Dashboard(Tableau Public)](https://public.tableau.com/views/RetailsalesanalysisprojectSQL/RetailSalesAnalysisDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)**

ナビゲーションボタンで行き来できる、3つのダッシュボードで構成されています。

1. **全体サマリー**:カテゴリ別の利益、地域別の利益、月次利益トレンド
2. **商品・取引分析**:商品別利益率ランキングTop10、割引の有無による利益比較
3. **顧客分析**:年代・性別の利益比較、支払い方法別分析、上位顧客トップ10

## データ構造

| テーブル | 内容 | 件数 |
|---|---|---|
| customers | 顧客の氏名・性別・生年月日・都市など | 200件 |
| products | 商品名・カテゴリ・単価・原価 | 50件 |
| stores | 店舗名・地域 | 5件 |
| transactions | 取引記録(ファクトテーブル)。顧客ID・商品ID・店舗ID・数量・割引・支払方法・日付 | 5,000件 |

customers・products・storesの3テーブルは、transactions(取引記録)を介して結びついており、これを軸にJOINすることで多面的な分析を行いました。

## 分析内容とインサイト

### 1. カテゴリ別の総利益
Electronics(2,152,078)とFashion(2,168,627)がほぼ同水準で、Groceries(678,879)の約3倍の利益を生んでいる。Groceriesは利益貢献度が相対的に低いカテゴリ。

### 2. 商品別の利益率ランキング
利益額の大きい商品は、単価が高い商品(家電・カメラなど)に偏る傾向。

### 3. 割引の有無による利益比較
| 区分 | 取引件数 | 平均利益/件 |
|---|---|---|
| 割引あり | 3,757件 | 1,001 |
| 定価 | 1,243件 | 996 |

割引ありの合計利益が大きいのは、主に取引件数が3倍多いことが要因。**1件あたりの利益効率はほぼ変わらず、割引は利益率を犠牲にせず取引数を増やす効果がある可能性**が見られる。

### 4. 性別による利益比較
男性(平均1,012/件、2,810件)と女性(平均985/件、2,190件)で、差はわずか(約2.7%)。明確な傾向差とは言い切れない。

### 5. 地域(店舗)別の利益
| 地域 | 合計利益 |
|---|---|
| East | 1,951,829 |
| West | 1,039,691 |
| North | 1,012,540 |
| South | 995,523 |

**East地域が他3地域の約2倍の利益**を上げている。店舗数・立地条件など、要因のさらなる調査価値あり。

### 6. 月次の利益トレンド(2023年9月〜2025年8月)
月ごとの利益は19万〜24万の範囲で安定して推移し、大きな季節変動は見られない。

### 7. 年代別の分析
20代〜60代以上では平均利益961〜1,036とほぼ横並びで、年代による大きな差は見られない。10代は平均1,404だが取引件数がわずか19件のため、統計的信頼性は低い。

### 8. 上位顧客トップ10
上位顧客の利益は34,821〜41,190の範囲に集中し、突出した1人はいない。取引件数と利益額の関係から、「高単価・低頻度」タイプと「低単価・高頻度」タイプの2種類の優良顧客像が存在する可能性が見える。

### 9. 支払い方法別の分析
| 支払い方法 | 取引件数 | 平均利益/件 |
|---|---|---|
| Bank Transfer | 1,159 | 1,027 |
| Credit Card | 1,281 | 1,003 |
| Cash | 1,284 | 994 |
| Mobile Money | 1,276 | 978 |

Bank Transferがわずかに平均利益が高いが、全体の差は小さく(978〜1,027)、決定的な傾向とは言えない。

## 総括

- 明確な差が見られた軸:**カテゴリ別**(Electronics/Fashion vs Groceries)、**地域別**(East vs その他)
- 差が小さく慎重な解釈が必要な軸:性別、年代、支払い方法、割引有無(1件あたりでは)
- 次のアクションとして有望な仮説:East地域の成功要因分析、割引施策による集客効果の検証、優良顧客の2タイプ別マーケティング施策の検討

## 使用したSQLの技術要素

- 複数テーブルのJOIN(4テーブル結合)
- 集計関数(SUM, COUNT, AVG)と GROUP BY
- 条件分岐(CASE WHEN)によるセグメント分類
- 日付操作(DATE_TRUNC, AGE, EXTRACT)
- 型変換(CAST/::numeric, ::date)
- 書式整形(to_char)

## 今後の展開

- Pythonを用いたETL処理の追加(データクレンジング、自動更新パイプライン)
- SQLのみでのダミーデータ生成(generate_series, random関数の活用)

---
---

# Retail Sales Analysis

## Overview

A data analysis project using PostgreSQL, SQL, and Tableau on a simulated retail sales dataset from Kaggle (CC0 license). Four tables (customers, products, stores, transactions) were joined to analyze profitability, regional performance, time trends, and customer segments from multiple angles.

- **Data source**:[Retail Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/buharishehu/retail-sales-dataset)
- **Tech stack**:PostgreSQL, SQL, Python(pandas, SQLAlchemy), Jupyter Notebook, Tableau
- **Note on currency**:The original dataset does not specify a currency unit, so all values are treated as raw numbers.

## Tableau Dashboard

**[Retail Sales Analysis Dashboard (Tableau Public)](https://public.tableau.com/views/RetailsalesanalysisprojectSQL/RetailSalesAnalysisDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)**

Consists of 3 dashboards, connected by navigation buttons.

1. **Overview**:Profit by category, profit by region, monthly profit trend
2. **Product & Transaction Analysis**:Top 10 products by profit margin, profit comparison by discount status
3. **Customer Analysis**:Profit by age group & gender, profit by payment method, top 10 customers by profit

## Data Structure

| Table | Content | Rows |
|---|---|---|
| customers | Name, gender, birth date, city, etc. | 200 |
| products | Product name, category, unit price, cost price | 50 |
| stores | Store name, region | 5 |
| transactions | Fact table: customer/product/store IDs, quantity, discount, payment method, date | 5,000 |

The customers, products, and stores tables are connected via the transactions table, which was used as the join key for all multi-table analysis.

## Analysis & Insights

### 1. Total Profit by Category
Electronics (2,152,078) and Fashion (2,168,627) are at a similar level, roughly 3x the profit of Groceries (678,879). Groceries contributes relatively little to overall profit.

### 2. Product Profit Margin Ranking
Products with the highest profit tend to be higher-priced items (electronics, cameras, etc.).

### 3. Profit Comparison by Discount Status
| Status | Transactions | Avg. Profit/Transaction |
|---|---|---|
| Discounted | 3,757 | 1,001 |
| Full Price | 1,243 | 996 |

The higher total profit for discounted transactions is mainly due to 3x more transactions. **Per-transaction profit efficiency is nearly unchanged, suggesting discounts may drive transaction volume without sacrificing margin.**

### 4. Profit Comparison by Gender
Male (avg 1,012/transaction, 2,810 transactions) vs. Female (avg 985/transaction, 2,190 transactions) — a difference of only about 2.7%, not a strong trend.

### 5. Profit by Region
| Region | Total Profit |
|---|---|
| East | 1,951,829 |
| West | 1,039,691 |
| North | 1,012,540 |
| South | 995,523 |

**East generates roughly 2x the profit of the other three regions.** Worth further investigation (store count, location factors, etc.).

### 6. Monthly Profit Trend (Sep 2023 – Aug 2025)
Monthly profit is stable, fluctuating between 190K and 240K, with no major seasonal trend.

### 7. Profit by Age Group
Ages 20s–60s+ are fairly flat (avg 961–1,036), showing no major difference by age. Teens show a higher average (1,404) but only 19 transactions, so this is not statistically reliable.

### 8. Top 10 Customers by Profit
Top customers' profit is concentrated between 34,821 and 41,190, with no single standout customer. Two distinct customer types emerge: "high-value, low-frequency" and "low-value, high-frequency."

### 9. Profit by Payment Method
| Method | Transactions | Avg. Profit/Transaction |
|---|---|---|
| Bank Transfer | 1,159 | 1,027 |
| Credit Card | 1,281 | 1,003 |
| Cash | 1,284 | 994 |
| Mobile Money | 1,276 | 978 |

Bank Transfer has a slightly higher average profit, but the overall spread (978–1,027) is small — not a decisive trend.

## Summary

- **Clear differences found in**:Category (Electronics/Fashion vs. Groceries), Region (East vs. others)
- **Small differences requiring careful interpretation**:Gender, age, payment method, discount status (per-transaction basis)
- **Promising next steps**:Investigate East region's success factors, validate discount strategy's effect on customer acquisition, explore targeted marketing for the two loyal-customer types

## SQL Techniques Used

- Multi-table JOINs (4 tables)
- Aggregate functions (SUM, COUNT, AVG) with GROUP BY
- Conditional logic (CASE WHEN) for segmentation
- Date functions (DATE_TRUNC, AGE, EXTRACT)
- Type casting (::numeric, ::date)
- Number formatting (to_char)

## Next Steps

- Add a Python-based ETL pipeline (data cleansing, automated refresh)
- Generate synthetic datasets using SQL only (generate_series, random functions)
