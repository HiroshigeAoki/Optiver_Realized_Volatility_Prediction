# Dataについて

## book_[train/test].parquet:
- 株の銘柄毎に最も競争力のある上位2つの売買注文のデータがはいっている。
- Data parquetファイルの読み込み方法: parquet file  

## stock_id 
- 株の銘柄 (どの株か) 銘柄は0から126まである。
- 読み込んだ後、カテゴリカルデータになってしまうので、int型に変えたほうがいいかも

## time_id 
- 取引の時間 (submissionファイルのtime_idと連動している)
- time_idは、連続値ではないが、全ての銘柄で統一されている。

## Seconds_in_bucket
- time_idの中で，バケットが始まって0からスタートして何秒後か．おそらく予測するのは10分後なのSecconds_in_bucketは最大600．

## bid_price[1/2] 
- 株の買値の希望値を正規化した価格．出された買値の希望値を正規化し，1番多い値と2番目に多い値．
- Normalized prices of the most/second most competitive buy level.

## ask_price[1/2] 
- 株の売値の希望値の1番目と2番目．出された売値の希望値を正規化し，1番多い値と2番目に多い値．
- Normalized prices of the most/second most competitive sell level.

## bid_size[1/2] 
- 買うのを希望している側の競争力のある1番目と2番目の株式の数
- The number of shares on the most/second most competitive buy level.

ask_size[1/2] 
- 売るのを希望している側の競争力のある1番目と2番目の株式の数．1番目と2番目の株式の数
- The number of shares on the most/second most competitive sell level.

## trade_[train/test].parquet: 
- 実際に行われた売買のデータが入っている。parquet fileの読み込み方法： parquet file 

## stock_id 
- 同上．
- Same as above.

## time_id 
- 同上．
- Same as above.

## seconds_in_bucket 
- time_idの中で，バケットが始まって0からスタートして何後か．
- Same as above. Note that since trade and book data are taken from the same time window and trade data is more sparse in general, this field is not necessarily starting from 0.

## price 
- 1秒間で行われた取引の平均価格
- 価格は正規化され、価格の平均はそれぞれのトランザクションで取引された株式の数で重み付される。
- The average price of executed transactions happening in one second. Prices have been normalized and the average has been weighted by the number of shares traded in each transaction.
　
## size 
- 取引された株式の合計
- The sum number of shares traded.

## order_count 
- 固有の取引の数
- The number of unique trade orders taking place.

# train.csv The ground truth values for the training set.

## stock_id 
- 株の銘柄 (どの株か) 銘柄は0から126まである。

## time_id 
- 取引の時間 (submissionファイルのtime_idと連動している)
- time_idは、連続値ではないが、全ての銘柄で統一されている。

## target 
- 株式の変動性
- 同じstock_id・time_idの株に対して、10分単位で計算される。
- tutorial notebook.
- The realized volatility computed over the 10 minute window following the feature data under the same stock/time_id. There is no overlap between feature and target data. You can find more info in our 

# test.csv
### テストデータは3つ
<br>
Provides the mapping between the other data files and the submission file. As with other test files, most of the data is only available to your notebook upon submission with just the first few rows available for download.

## stock_id 
- Same as above.

## time_id
- Same as above.

## row_id 
- Submission.csvでの行番号。

## stock_id 
- 例: stock_id: 0, time_id:4 ⇒ 0-4

# sample_submission.csv 
### A sample submission file in the correct format.

## row_id 
- Submission.csvでの行番号。

## stock_id
- 例: stock_id: 0, time_id:4 ⇒ 0-4
- Same as in test.csv.

## target 
- 株の変動性
- Same definition as in train.csv. The benchmark is using the median target value from train.csv.



## tutorial notebookのmemo
- 取引は、売りての言い値(ask price)と買い手の掛け値(bid price)が同じときでないと発生しない．
- 株式市場では、流動性が大事
## Order bookの統計量

## bid/ask speedの計算方法
![image](https://user-images.githubusercontent.com/79825066/128728034-323cb94b-614a-4ca7-b459-68c756a8c27d.png)

- Best offer, best bidはどうやって求めるの？

## Weighted average priceの計算方法
![image](https://user-images.githubusercontent.com/79825066/128728271-01b960d7-4ba0-4bcf-a75c-01b6631afcc1.png)

