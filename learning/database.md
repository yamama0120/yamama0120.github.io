資料庫概念
==
## DB
* 欄=屬性attribute,
* 每列row=紀錄record
* 外鍵(foreign key 可為空)去參考 主鍵(primary不可為空) 
#### 完整性限制
* 哪3個?  
1. 實體完整性：PK具備唯一性且不可為空  
2. 參考完整性：用以建立兩table間的關聯(不能沒有對應的data)  
3. 定義域完整性：每個屬性的值皆符合其範圍
* 刪的解決方法3個  
1. 限制刪除：不允許刪除  
2. 連帶刪除：若欲刪的資料有被其他紀錄所參考,則刪除帶有相同參考外來鍵之紀錄。  
3. 空值化：” ，將該筆紀錄之相同參考外來鍵設為null
#### 關聯式資料庫綱要relational databases schema 
* 表名稱(a,b,c,d) 
* 外鍵寫法：地址 參考 c.編號

#### ERD實體關聯圖
* 以圖形方式表示關聯資料庫的概念化模型,用以描述真實世界中物件與物件之間的關聯性  
* 圖示：實體(方) 關係(菱) 屬性(圓) 鍵值(圓+底線) 基數比(11.1M.MN)
* 屬性：一般、多值、複合(切成更小/\)、衍生(u相乘得總量)

#### 正規化(將資料拆成數個具有關聯的資料表 )
* 目的：
  1.減少重複性 2.避免data不一致 3.避免更新異常
* 正規化
	1. 1NF:多值
	2. 2NF:部分相依(abc但b可決定c)  
	3. 3NF:遞移相依(非pk 決定非pk)  (若無:新增要等key,刪到其他的,修改相關的) 
	4. BCNF:非pk 決定pk  
	5. 4NF:多值相依  

* 無損分解：分解過程中無任何元素消失.且舊關聯表中所有功能相依性完整保留於新關聯表  
* 反正規化：過度正規化時,需查詢具有了大量關聯的資料表  
	1. 易降低執行效率  
	2. 分割過程易造成資料遺失  
	因此可適當做反項正規化(從目前的正規化退回上一個步驟)


## SQL

#### DDL definition(針對table的語法):Create/Alter/Drop  
索引：Create Index A on 表名(欄位)

#### DML manipulation操作 (針對table內容):增 刪 改 查   
增：insert into  表名+(屬性,,,)+values+(‘值’,900)  
改：update 表名+set  屬性=值,,, +where…  
刪：delete from表名+where…。不加全刪 

查：
* 只能指定一個欄位-例:去除重複的(B)
```SQL
SELECT DISTINCT B , A , C FROM TABLE
條件可加：in ('值') .is null. and. or
```

* 查詢相符字元  
```SQL
where name like '鐵%'
where name like '鐵_ _'
```
* 照順序排 限制筆數  

```SQL
order by欄位 +遞增ASC(預設)/DESC  limit 1 
```
* 分成各公司計算:
```SQL
group by 公司，having條件
```


* 計算：總和.平均.字串長  
```SQL
SUM,AVG,MAX,MIN,LENGTH  
實例:Select SUM (name) from…
```	
* 計算非空數:  
```SQL
此紀錄幾筆非空/ 此屬性幾筆非空  
count(*)/count(屬性)/count(distinct 屬性)  
至少1欄位不為空  count(*) 就算一筆紀錄
```
 

* 別名&連結
```SQL
SELECT A1.Store_Name AS Store,  
SUM(A1.Sales) AS 'Total Sales'   ->有分開就要引號!
FROM Store_Information AS A1
```

* Join
```SQL
內部 Inner join (會依條件,預設)
外部 left/right/full outer join (沒有的自動補null) 
```

* 進階
時間差 種類:
```SQL
Where datediff (“yyyy”,A,B)  (OTHER:季q 月m 天d 時h)
之間:Where 屬性 Between a and b
```
* OTHER :
```SQL
挑出字元輸出:SUBSTR (屬性,從哪始,長度):
合併:CONCAT
取代:Replace (屬性,’b’,’c’)  ->b改c 

```


* Union聯集
```SQL
union連結兩結果,不重複
union all 全列,可重複
(交集Intersect )
```

* 巢狀查詢 (效率較join好) 有時就不用join
```SQL
<.>.=.not.<= ,like 常搭配in/any/all (>all  >any)

標準子查詢、關聯子查詢(2表以上)
Where EXISTS ( ) 測試內查詢有無產任何結果  
	有->系統執行外查詢中,不理會內查詢條件
	沒有->整個 SQL 語句不會產生任何結果
```

* Case
<img src="https://github.com/yamama0120/yamama0120.github.io/blob/main/image/case1.png" width="500px">

#### DCL control(針對權限做控管):Grant Revoke
``` SQL
grant 權 on 表名(屬性) to 誰 identified by "密"
revoke 權 on表名(屬性) from誰
權：INSERT.UPDATE.DELETE.UPDATE.SELECT.index.execute.usage
```

## 交易&資安
#### 資訊安全
* SQL injection：  
程式設計不良，輸入時夾帶的惡意指令，被認為是正常指令而執行，遭受破壞和入侵  

* 對資料庫的危害
	1. 資料庫機密訊息竊取
	2. 網頁竄改
	3. 資料庫被惡意操作(刪 改管理帳密)
	4. 獲取權限劫持系統

#### 交易管理四大特性ACID
* 單元性atomicity：交易不可再分割(全執行/全不執行)
* 一致性consistency：儲存資料在交易前後，均滿足完整性限制，並有正確結果
* 孤立性isolation：交易執行直到commit前，不可被同時執行的其他交易干擾
* 永久性durability：交易執行且經commit後，對資料庫所做的變更永遠有效

#### 交易並行問題
* 死結 (圖:o交易,索資源指向擁有的)  
	當兩個以上的運算單元，雙方都在等待對方停止執行以取得系統資源，但沒有一方提前結束。
* 死結前提四條件  
	1. 互斥:一個資源只能給一個交易，不可共用
	2. 不可搶奪:在交易執行過程中，不可搶奪其他交易資源
	3. 持有與等待:某交易T1正佔用R1資源,又在等待T2佔用的R2資源
	4. 循環等待:會形成一個相互循環的環狀過程


#### 並行控制問題
* **更新遺失**：  
多交易交錯執行->針對item更新或存取；  
可能因執行順序不同->item的內容值不正確  
* **未確認相依(髒讀)**： a做 b搞事  
交易進行中，交易A更新但尚未commit，此data被交易b讀取到；但交易A因故rollback->交易B讀到錯誤的值
* **不一致分析**：算c 你改c  
某交易正對資料做聚合運算，but有項目被其他交易更改->計算結果不正確
* **幻讀**：  
查詢過程中，因另一筆交易更新，造成讀取筆數錯誤


## NoSQL:
* **水平擴充能力**：
  只要新增伺服器節點，就可不斷擴充資料庫系統容量。低價c即可
* **打破schema**：
採用key-value模式(將一筆資料簡化為只有1個key值,對應一個value值)，每筆資料間無關聯性->故可任意切割或調整，解決大量資料異動困難,資料高動態.即時
* **型態多元**：
  資料量大時，可處理多種類資料(圖片.影音…) 迎合市場需求

#### 簡要說明4主流NoSQL 存資料 應用？
* 鍵值資料庫key-value 
* 欄族資料庫column 
* 文件資料庫documents 
* 圖形資料庫graph
<img src="https://github.com/yamama0120/yamama0120.github.io/blob/main/image/database1.jpg" width="500px">
<img src="https://github.com/yamama0120/yamama0120.github.io/blob/main/image/database2.jpg" width="500px">




#### CAP(分散環境下p必滿足)
* **C一致性**：任何讀取操作下,總能讀到正確結果  
NoSQL採最終一致性(分散data暫時讀不到最終結果,一段時間後必須會讀到更新後的資料)
* **A可用性**：可在有限時間內返回操作結果,保證每個請求不論成功或失敗皆有回應
* **P分區容忍性**：任意資訊的遺失或節點失敗,不影響整體系統運作

## 資料倉儲
#### 資料倉儲定義:  
組織中大型資料庫->用ETL程序整合、集中儲存，再以管理者資料分析工具(olap data mining )->動態即時檢視,分析有價值決策    
ETL:資料清理過程(萃取、轉置、載入)
#### 4大特性
* 整合性：資料from組織內外,不同時間來源型態->標準化,一致性格式
* 不變動性：資料一旦確認寫入後是不會被刪除的
* 時間差異性：累積歷史性資料->反映隨時間變化的資料->趨勢分析
* 主題導向：和OLTP比,架構著重於按意義歸類至不同主題(data可重組for特定Q)
#### 三層架構
* 傳遞 -視覺化
* 儲存-倉儲.超市.(成本低.短期佳)
* 獲得內外data .ETL
<img src="https://github.com/yamama0120/yamama0120.github.io/blob/main/image/database3.jpg" width="500px">


#### 資料方塊
定義：倉儲建的資料集合，維度和量值定義的多維度結構->for user 快.複雜的查
* Drill down 彙總_深入明細_維度增
* Roll up
* Slice針對某一維度限制
* Dice。2個以上維度
* Pivot:變換維度方向,重新安排表格中維度的放置,user可從不同角度檢視資料

#### OLAP多維度方式分析for 大規模統計數據分析
* 事實表格:  
反應過去事實的data,存放以大量交易為中心,經量化的數值資料
* 維度表格:  
為加速查詢決策者在該維度中有興趣的data,所建立的索引表格
* 星狀綱要：  
每個維度都儲存在單一資料表中。一個中心事實表格有大量不重複的資料，較小的附屬維度表格。
* 雪花綱要：  
是星狀網要的變形，部份維度表格經正規化分裂成新的維度表格，容易維護及節省儲存空間。

## B-tree
* 分枝:取上標［m/2］~m  ，鍵值:取degree的各-1  
3-tree：degree 2-3 ，鍵1-2
* 插入：加入後滿了上拉/ 葉記得分開
* 刪除：葉-取左大or右小 做rotate/取左大or右小補 







