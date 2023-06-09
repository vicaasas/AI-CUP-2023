# NCKU-AICUP2023-TEAM-3575

## 運行環境
### 使用 Anaconda 建立環境
```bash
conda create -n Contest python=3.8
```
### python 套件
```bash
pip install -r requirements.txt
```
## Checkpoint
|        Class         | Description                          
| :------------------: | :----------------------------------- 
|     `Part2`          | [BERT label predict](https://drive.google.com/file/d/14g1haykEVEspO62SMqWVHZg6Ua-UY7gG/view?usp=drive_link)<br> [Evidence Reranking](https://drive.google.com/file/d/1w_xZEenaYmJdaBJzvu8RhszejcRJjcwO/view?usp=drive_link) 
|     `Part3`          | [Mask predict](https://drive.google.com/file/d/1mT_6Y2Bplq0ullOx8NgMXlAdSUXXM0-G/view?usp=drive_link)

## 各 part 分數
### part1
origin
|                        | Top1    |  Top2    | Top3    | Top4    | Top5  |
| :------------------: | :------------------ |------------------ |------------------ |------------------ |------------------ |
|          `Precision`   |  0.247 |0.238 |0.214 |0.197 |0.178|
|           `Recall`     |  0.224 |0.433 | 0.584 | 0.713 |0.807 |

Adaptive Cosine Similarity
|                        | Top1    |  Top2    | Top3    | Top4    | Top5  | Top6  | Top7  | Top8  | Top9  | Top10 | 
| :------------------: | :------------------ |------------------ |------------------ |------------------ |------------------ |------------------ |------------------ |------------------ |------------------ |------------------ |
|          `Precision`   | 0.698   |	0.428   |	0.309 |0.241	|0.197	|0.167	|0.145	|0.128	|0.114	| 0.104 |
|           `Recall`     | 0.653   |	0.791	| 0.847	  |0.875	|0.892	|0.905	|0.913	|0.920	|0.926	| 0.931 |

### part2
`Train data`
|              Precision          | Top1    |  Top2    | Top3    | Top4    | Top5  |
| :------------------: | :------------------ |------------------ |------------------ |------------------ |------------------ |
|          `origin`   | 0.783	|0.577	|0.441	|0.353	|0.293|
|           `rerank`     | 0.903	|0.632	|0.461	|0.358	|0.293|

|              Recall          | Top1    |  Top2    | Top3    | Top4    | Top5  |
| :------------------: | :------------------ |------------------ |------------------ |------------------ |------------------ |
|          `origin`   | 0.586	| 0.774	| 0.846| 	0.876| 	0.891| 
|           `rerank`     | 0.689	|0.851	|0.885	|0.891|	0.891|

### part3
`Train data Accuracy:`
|                       | origin    |  Prompt    | Rerank & Prompt    |
| :------------------: | :------------------ |------------------ |------------------ |
|          `Val_acc`   | 0.4345	|0.7146	|0.7223	|
|           `loss`     | 1.2353	|0.8876	|0.9030	|

## 檔案說明
我們將 public_train.jsonl 跟 public_train_0522.jsonl 合併成 `public_train_all.jsonl`
以及public_test.jsonl 跟 private_test_data.jsonl 合併成 `public_private_combine_test_data.jsonl`
<br>而有兩個檔案太大[train_doc5_all_page_other_predictor.jsonl](https://drive.google.com/file/d/17171E3S6HRH9tFwtnhItrS-Vhms7Ce9O/view?usp=drive_link)以及[combine_test_doc5_all_page_with_sentences.jsonl](https://drive.google.com/file/d/1Jrs2aQVZX4JVqGHpfMGzmUIq7BSPkYuj/view?usp=drive_link)請自行下載
* **`hanlp_con_results.pkl:`** 為 public_train_all.jsonl 的 hanlp_results

* **`hanlp_con_combine_test_results_imp.pkl:`** 為 public_private_combine_test_data.jsonl 的 hanlp_results
* **`train_doc5_all_page_other_predictor.jsonl:`** 為包含所有 wikipedia search 的結果以及其對應的內容的 train data
* **`combine_test_doc5_all_page_with_sentences.jsonl:`** 為包含所有 wikipedia search 的結果以及其對應的內容的 test data
* **`train_doc10_all_method.jsonl:`** 為Part1 public_train_all.jsonl 的經過 Adaptive Cosine Similarity取相似度前10的predicted page
* **`combine_test_doc10_all.jsonl:`** 為Part1 public_private_combine_test_data.jsonl 的經過 Adaptive Cosine Similarity取相似度前10的predicted page
* **`train_doc10sent5_all_ext.jsonl:`** 為Part2 將 train_doc10_all_method.jsonl 經過 BERT label predict 後取前五個 evidence的結果
* **`train_doc10sent5_all_rerank_ext.jsonl:`** 為 train_doc10sent5_all_ext.jsonl 經過 Evidence Reranking 的結果
* **`combine_test_doc10sent5_ext.jsonl:`** 為Part2 將 combine_test_doc10_all.jsonl 經過 BERT label predict 後取前五個 evidence的結果
* **`combine_test_doc10sent5_rerank_ext.jsonl:`** 為 combine_test_doc10sent5_ext.jsonl 經過 Evidence Reranking 的結果

## 檔案格式

train_doc5_all_page_other_predictor.jsonl:
```python3
{
    'id': '...',
    'label': '...',
    'claim': '...',
    'evidence': '...', 
    'hanlp_results': '...',
    'all_page': '...', //全部的wiki page
    'all_sentences': '...', //wiki page 對應的相關內容
}
```
combine_test_doc5_all_page_with_sentences.jsonl:
```python3
{
    'id': '...',
    'claim': '...',
    'hanlp_results': '...',
    'all_page': '...', //全部的wiki page
    'all_sentences': '...', //wiki page 對應的相關內容
}
```
train_doc10_all_method.jsonl:
```python3
{
    'id': '...',
    'label': '...',
    'claim': '...',
    'evidence': '...',
    'predicted_pages': '...',
}
```
combine_test_doc10_all.jsonl:
```python3
{
    'id': '...',
    'claim': '...',
    'predicted_pages': '...',
}
```
train_doc10sent5_all_ext.jsonl:
```python3
{
    'id': '...',
    'label': '...',
    'claim': '...',
    'evidence': '...',
    'predicted_pages': '...',
    "predicted_evidence": '...'
}
```
combine_test_doc10sent5_ext.jsonl:
```python3
{
    'id': '...',
    'claim': '...',
    'predicted_pages': '...',
    "predicted_evidence": '...'
}
```
train_doc10sent5_all_rerank_ext.jsonl:
```python3
{
    'id': '...',
    'label': '...',
    'claim': '...',
    'evidence': '...',
    'predicted_pages': '...',
    "predicted_evidence": '...' // 這裡是 reranking 後的結果
```
combine_test_doc10sent5_rerank_ext.jsonl:
```python3
{
    'id': '...',
    'claim': '...',
    'predicted_pages': '...',
    "predicted_evidence": '...' // 這裡是 reranking 後的結果
}
```
