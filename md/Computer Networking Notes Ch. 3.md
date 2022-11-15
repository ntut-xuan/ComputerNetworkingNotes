# Computer Networking Notes Ch. 3

## 傳輸層提供的服務

### 傳輸層

- 提供邏輯上的通訊功能。
- 應用程式提供傳輸層提供的邏輯通訊發送訊息（Message），不需要考慮內部的物理細節。
- 傳輸層基本上分為 TCP 與 UDP，且通常只在邊緣系統上負責。



### 傳輸層的運作原理

- 傳輸層會從應用程式接收到的訊息（Message）轉成多個訊息區段（Segment）。
- 多個訊息區段（Segment）會將其封裝給網路層，並交由網路層發送到指定的目的地。
  - 網路層主要是負責兩個主機（Host）的邏輯通訊。
  - 傳輸層主要是負責兩個程序（Process）的邏輯通訊。



### TCP、UDP 與 IP

- TCP
  - 提供可靠的連接傳輸方式。
  - 提供可靠傳輸服務、壅塞控制。
  - 在 TCP 上的訊息分組稱為 Segment。
- UDP
  - 提供不可靠，無連接的傳輸方式。
  - 在 UDP 上的訊息分組稱為 Datagram。
- IP
  - 屬於網路層的範疇。
  - 盡力交付的服務：IP 會盡最大的努力在通訊的主機之間傳輸訊息區段。
  - 不可靠服務：IP 不做任何的確保，單純傳送資訊。



## 多路複用（Multiplexing）與多路分解（Demultiplexing）

### 多路複用與多路分解的概述

- 多路複用與多路分解

  - 網路層提供從主機到主機之間傳輸服務，延伸到應用程式從程序到程序之間的交付服務。

- 多路分解

  - 在目標主機上，將從網路層獲得到的訊息區段中的資料，交付到對應的 socket 中。

- 多路複用

  - 在來源主機上不同的 socket 上蒐集 segment，並在 segment 上封裝 header，

    多路分解可藉由 header 分解成多個 segment，並將 segment 傳送至網路層。



### Segment 的架構

- 都會有來源主機與目標主機的 IP、Port。
- 都會持有一個傳輸層的區段。
- 主機使用 IP 地址與 Port，來將訊息指向適合的 socket 上。

![image-20221115145520066](https://i.imgur.com/L291ISR.png)



### 無連接的多路複用與多路分解

> 待補



### 連接的多路複用與多路分解

> 待補



## User Datagram Protocol

### UDP 的簡介

- UDP 的簡介
  - 沒有多餘的裝飾、極簡的網路傳輸協定
  - 盡力而為服務：可能會 loss 或者傳輸亂序的資料給應用程式。
  - 無連接協定：不須 handshaking，UDP Segment 
- Why use UDP?
  - 不用 handshaking。
  - 不需要在 sender、receiver 紀錄狀態。
  - 極小的 header size。
  - 不用壅塞控制。
- Where to use UDP?
  - 影音串流平台
  - DNS
  - SNMP
  - HTTP/3（通常會新增為了可靠傳輸所需要的特性，以及新增壅塞控制到應用層上）。



### UDP 的傳輸層動作

- UDP Sender 的動作
- UDP Receiver 的動作
  - 從 IP 取得 segment
  - 使用 checksum 驗證 segment 的資料



### UDP Checksum

- 發送方對 segment 的訊息區段內所有 16bits 的數值進行總和，將總和的值進行反運算當作驗證碼。
- 接收方接收到 segment，總和後加上驗證碼，若出現 0 則代表 segment 有誤。



### UDP 的比較

- UDP 提供錯誤偵測，但不包含錯誤修復，僅能放棄訊息區段或者發出警告。
- TCP 提供了可靠傳輸，先見「可靠資料傳輸原理」後見 TCP 傳輸原理。



## 可靠資料傳輸原理

### 可靠資料傳輸原理

- 期望提供的功能：
  - 資料能夠藉由一條可靠的通道進行傳輸。
- 實際上功能上的實作：
  - 利用可靠資料傳輸協定（Reliable Data Transfer Protocol），將資料藉由不可靠的通道（例如 IP）進行傳輸。

|                            期望                             |                            實作                             |
| :---------------------------------------------------------: | :---------------------------------------------------------: |
| ![image-20221115211524339](https://i.imgur.com/EtykjMS.png) | ![image-20221115211531431](https://i.imgur.com/cnE3ZXA.png) |



### rdt 1.0: Reliable transfer over a Reliable channel

#### 介紹 rdt 1.0

- 考慮底層通道是完美的
  - 沒有 bit 錯誤的問題
  - 沒有掉封包的問題
- 發送端與接收端可以形成一個有限的狀態機
  - sender 等待上層指示，製作 packet 與發送 packet 到底層通道內。
  - receiver 等待下層指示，接收 packet 與傳遞資料至上層。



### rdt 2.0: Channel with bit errors

#### 介紹 rdt 2.0

- 底層通道較趨近真實的情況，訊息區段是有可能在傳輸過程受損的。
- 接收方得讓發送方知道哪些內容正確被接收，哪些內容需要重傳。
  - 這樣的想法使用自動重傳請求 ARQ (Automatic Repeat reQuest) 協議來實踐。
- 使用 ARQ 之外，還需要三種協議功能來處理 bit error 的問題：
  - 錯誤檢測
  - 接收方反饋：往接收方發送 ACK (0) 與 NAK (1)。
  - 重傳：接收方收到有錯誤的區段時，發送端重新傳輸該訊息區段。



因此，rdt 2.0 可以畫成以下的狀態機

- sender：等待上層指示發送資訊至底層通道，等待 ACK 或 NAK 資訊，根據資訊決定是否傳輸或者重傳。
- receiver：等待下層指示，如果收到了資訊，但資訊是錯誤的，發送 NAK，否則發送 ACK 並且傳遞資料給上層。

|                           sender                            |                          receiver                           |
| :---------------------------------------------------------: | :---------------------------------------------------------: |
| ![image-20221115213337667](https://i.imgur.com/jBLyNQ9.png) | ![image-20221115213345441](https://i.imgur.com/ozAbGsX.png) |

rdt 2.0 這樣的協議也被稱作停等（stop-and-wait）協議。



#### rdt 2.0 的問題

- 回傳的 ACK & NAK 同時也可能是錯的，也就是 ACK 變成了 NAK，或者 NAK 變成了 ACK。
  - 不太可能再次重傳，可能會 duplicate。
- 解決問題的簡單方式
  - 目前假設的問題：只有 bit 會錯誤，但不會掉封包。
  - 因此讓封包帶有序號，檢查序號即可知道是否為重傳。



### rdt 2.1

Sender 可以分成以下的狀態機：

1. 等待上層傳送 0 信號，接著傳送序號為 0 的 segment
2. 等待剛剛傳送序號為 0 的封包回傳 ACK 或者 NAK
   - 如果收到了 NAK 或者封包錯誤，那麼重傳
3. 等待上層傳送 1 信號，接著傳送序號為 1 的 segment
4. 等待剛剛傳送序號為 1 的封包回傳 ACK 或者 NAK
   - 如果收到了 NAK 或者封包錯誤，那麼重傳

<img src="https://i.imgur.com/4YO92u1.png" alt="image-20221115215246987" style="zoom:67%;" />



Receiver 可以分成以下的狀態機：

1. 接收到了信號 0，確認 segment 是序號 0 且沒有問題，將資料傳至上層並傳送 ACK 的 packet。
   - 若接收到了 segment 且 segment 是錯的，傳送 NAK 的 packet。
   - 若接收到了 segment 且 segment 是對的，但 segment 有序號 1，傳送 ACK。
2. 接收到了信號 1，確認 segment 是序號 1 且沒有問題，將資料傳至上層並傳送 ACK 的 packet。
   - 若接收到了 segment 且 segment 是錯的，傳送 NAK 的 packet。
   - 若接收到了 segment 且 segment 是對了，但 segment 有序號 0，傳送 ACK。

<img src="https://i.imgur.com/ypv4CHG.png" alt="image-20221115215619780" style="zoom:67%;" />

### rdt 2.2

與 rdt 2.1 不同的地方：由於發送端只要收到了兩個同組的 ACK 後，確定該封包為冗餘封包，就可以知道接收方沒有正確接收到分組。

因此 rdt 2.2 拋棄了發送 NAK 的特性，如下面的發送方狀態圖。

<img src="https://i.imgur.com/NlOagVy.png" alt="image-20221115221716299" style="zoom:67%;" />



### rdt 3.0

- 底層通道較趨近真實的情況，訊息區段是有可能在傳輸過程受損之外，也有可能掉封包。
- 讓發送方負責檢測和恢復掉封包的動作，如果發送方可以等待足夠長的時間來確定封包已丟失，那麼他只需要重傳即可。
  - 等多久呢？讓發送方選一個適當的時間值。
  - 冗餘封包？rdt 2.2 處理掉了這個問題。
- 實現等待足夠長的時間來處理掉封包的問題，需要一個倒數計時器，且滿足以下的需求
  - 每次發送一個 segment 時，啟動一個定時器
  - 收到 response 的時候，定時器採取適當的措施
  - 可以終止計時器



下圖呈現了 rdt 3.0 的發送與接收狀態圖

<img src="https://i.imgur.com/0Ogdeii.png" alt="image-20221115223505589" style="zoom:67%;" />

<img src="https://gaia.cs.umass.edu/kurose_ross/interactive/rdt_30_receiver.png" alt="Interactive Problems, Computer Networking: A Top Down Approach" style="zoom:67%;" />



rdt 3.0 可以使用以下的圖，來呈現遇到問題時解決的方式

|                                                              |                                                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://i.imgur.com/HS2HnvP.png" alt="image-20221115223940577" style="zoom:67%;" /> | <img src="https://i.imgur.com/zfR7E2y.png" alt="image-20221115223951170" style="zoom:67%;" /> |
| <img src="https://i.imgur.com/ZaytQmU.png" alt="image-20221115224000456" style="zoom:67%;" /> | <img src="https://i.imgur.com/7kjgYLB.png" alt="image-20221115224014566" style="zoom:67%;" /> |



### 流水線可靠資料傳輸協定

- 停等協定的效能很糟糕，原因是考慮 RTT 的影響後，發送封包到等待封包回來的時間太長，利用率過低
- 所以我們應該不停地發送封包而不等待確認，使用流水線技術。

<img src="https://i.imgur.com/HtAkywZ.png" alt="image-20221115224635511" style="zoom:67%;" />

- 同時需要處理以下的事項：
  - 增加序號範圍，必須保證送出去的封包序號不一樣，避免衝突。
  - 發送方與接收方也許沒辦法暫存 segment，因為會一次性地吃很多個 segment，所以遇到正確的就要儲存。
  - 利用回退 N 步（Go-Back-N, GBN）以及選擇重傳（Selective Repeat, SR）來解決丟失、損壞與超時的問題。



### Go-Back-N

- 我們有好幾個 segment 等著發送，我們可以使用流水線的方式進行發送。
- 我們使用 slide windows 的方式進行發送，若有 10 個 segment，我們一開始發送 [0, 1, 2, 3]，接著在收到 ACK 0 的時候發送 [1, 2, 3, 4] 等等，其中決定一次發送幾個 segment 的準則，來自於 Go-Back-N 所指定的 N 的數量。
- GBN 必須要，也處理了以下的事項：
  - 發送：發送時先確定 windows 是否已滿，滿了就等待，沒滿就發送。
  - 收到一個 ACK：滑動窗口，發送下一個 segment。
  - 超時事件：確定哪個 segment 超時，接著將該 base 超時的 segment 的 slide windows 全部重送。
    - 例如 2 超時了，且 N=4，接收端拋棄 [3, 4, 5] 已經傳送成功的封包，[2, 3, 4, 5] 都會再重送。
- GBN 有以下的優點：
  - 解決了需要按序交付的問題。
  - 不需要暫存失序的分組。
- GBN 同時隱含以下的問題：
  - 丟棄所有的失序分組有點浪費，可能會造成需要重傳多次的問題。

<img src="https://i.imgur.com/cyJfhdJ.png" alt="image-20221115232025581" style="zoom:67%;" />

<img src="https://i.imgur.com/gkZMIY6.png" alt="image-20221115232008109" style="zoom:67%;" />



### Selective Repeat

- 我們有好幾個 segment 等著發送，我們可以使用流水線的方式進行發送。
- 我們使用 slide windows 的方式進行發送，若有 10 個 segment，我們一開始發送 [0, 1, 2, 3]，接著在收到 ACK 0 的時候發送 [1, 2, 3, 4] 等等，其中決定一次發送幾個 segment 的準則。
- 比起 GBN，SR 改變了以下的事情：
  - 超時事件：確定哪個 segment 超時，接著將該 base 超時的 segment 重送。
    - 例如已經傳送 [2, 3, 4, 5]，2 超時了，就會再次傳送 2 這個 segment，接著傳送 6, 7...
- SR 有以下的優點：
  - 利用暫存解決掉 GBN 浪費的問題
- SR 有以下的問題：
  - 當今天序號的範圍是有限的（例如 0 1 2 3 循環）時，若沒有適當的選取範圍，會導致因為 Receiver 不知道 Sender 傳了什麼，而導致誤解發生。
    - 窗口長度必須要小於等於序號空間大小的一半。

|                           正常運作                           |                        不當選取範圍                         |
| :----------------------------------------------------------: | :---------------------------------------------------------: |
| <img src="https://i.imgur.com/w0xsMpC.png" alt="image-20221115232925136" style="zoom: 80%;" /> | ![image-20221115233702212](https://i.imgur.com/gJttpqQ.png) |



## 連接導向的傳輸方式：TCP

### TCP

- 點對點傳輸：One sender, one receiver.
- 可依賴的，照順序的 byte stream
- 

### TCP Segment 結構

<img src="https://i.imgur.com/Z9WBiFV.png" alt="image-20221116003149994" style="zoom:67%;" />



- Source port：來源端口號
- Destination port：目標端口號
- Sequence number：序號 (32 bits)
- Acknowledgment number：確認號 (32 bits)
- header length field：標頭長度欄位 (4bits - 20 bits)
- flag field：標誌欄位 (6bits)
  - ACK 用於確認 segment 的值是有效的
  - RST、SYN、FIN 用於連接建立與拆除
  - 壅塞告知使用 CWR 與 ECE 的欄位
  - PSH 表示接收方應立即將資料給上層
  - UGR 表示指示資料是緊急的，且會在 Ugent data pointer 指出。



#### 序號與確認號

- 當我們有大資料的時候，會將大資料切成小的 segment，每個 segment 都有一組序號。
- 確認號通常意指在這個序號接收成功後，應該要接收到的序號。
  - 例如第一筆資料傳送了 0~535 的資訊，第二筆應是 536~899 的資料，這時候確認號會填上 536。
- TCP 會回傳目前傳送成功的最小 bits，稱為累積確認。
  - 例如第一筆資料傳送了 0~535 的資訊，第二筆應是 536~899 的資料，但沒傳成功，第三筆 900 至 1000 的資料傳成功了。



### Telnet

> 待補



### 往返時間的估計與超時

#### 估計往返時間

- SampleRTT：某 segment 被發出到 segment 被接收的時間量。

  - SampleRTT 不為重送的 segment 進行計算。

- EstimatedRTT：由於 SampleRTT 是浮動的，所以我們考慮 SampleRTT 的平均值，每當獲得一個新的 SampleRTT 的時候，就會用以下的算式進行更新。
  $$
  \text{EstimatedRTT} = 0.875 \times \text{EstimatedRTT} + 0.125 \times \text{SampleRTT}
  $$

- 使用 SampleRTT 後，估計 RTT 與樣本 RTT 可以看出明顯的差別，估計 RTT 變得更加趨緩了

<img src="https://i.imgur.com/T4gVeu0.png" alt="image-20221116025825195" style="zoom:67%;" />

- DevRTT：用來估算 SampleRTT 偏離 EstimatedRTT 的程度
  $$
  \text{DevRTT} = (1-\beta) \times \text{DevRTT} + \beta \times |\text{SampleRTT} - \text{EstimatedRTT}|
  $$
  $\beta$ 值推薦 0.25。



#### 設置與管理重傳超時間隔

- 什麼時候該重傳？

  - 大於 EstimatedRTT 時重傳，但如果大於 EstimatedRTT 太久又怕延遲過大，所以需要設置間隔。

- TimeoutInterval 可以由以下的公式定義
  $$
  \text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}
  $$
  初始值建議 1 秒，出現超時則 TimeoutInterval 加倍。



### 可靠資料傳輸

#### TCP 傳送事件

- TCP 是一個可靠資料傳輸的協定。
- TCP 發送的簡化三大事件
  - 從上面應用程式接收到資料 $e$
    - 封裝到一個 segment 中，把 segment 交給 IP
    - 如果定時器沒開啟則開啟定時器。
  - 定時器超時
    - 重傳最小序號但還沒被回應的 segment
    - 啟動定時器。
  - 收到 ACK，ACK 回傳序號 $y$
    - 定義 sendbase 為最早未被確認的 segment 序號
    - 如果 $y$ 大於 sendbase，則 sendbase 賦值為 $y$。
    - 如果當前沒有任何 segment 的回應，重啟定時器。



### 流量控制

若某應用程式讀取資料的速度太慢，但傳送速度太快，就會導致大量暫存溢出，因此 TCP 需要提供流量控制，使用了壅塞控制。

TCP 提供了一個接口稱為 receive window 的變量來提供流量控制，其定義如下：
$$
\text{Rev Buffer} \ge \text{LastByteRcvd - LastByteRead}$ \\
\text{rwnd} = \text{Rev Buffer} - [\text{LastByteRcvd} - \text{LastByteRead}]
$$
若 $\text{rwnd} = 0$ 時，接收方透過 ACK 來告訴發送方不能再發送資料（Buffer 為 0），否則則可以發送資料（Buffer 不為 0）。



### TCP 連接管理

- 建立連線：三次握手（three-way handshake）
  - 第一步：發送帶有 SYN=1、seq=client_isn 的特殊 TCP segment
  - 第二步：若接收端同意了你的連線請求，發送 SYN=1、seq=server_isn、ack=client_isn+1 的特殊 TCP segment
  - 第三步：發送端發送 SYN=0、seq=client_isn+1、ack=server_isn+1
  - 由此，連接就開始了。

<img src="https://i.imgur.com/72d4xLS.png" alt="image-20221116033320411" style="zoom:67%;" />

- 結束連線：發送一個帶有 FIN 的標頭，接收端發送一個 FIN 的標頭，接著連接的資源就會全部釋放。

<img src="https://i.imgur.com/4bCxD8Q.png" alt="image-20221116033327173" style="zoom:67%;" />



## TCP 壅塞控制

TCP 使用了三種不同的 algorithm 來控制 TCP 的壅塞情況，以避免壅塞情況發生導致暫存溢出，瘋狂掉包的問題。

1. 慢啟動
   - TCP 會設置一個較小的 MSS，然後開始指數增長得一次性發送越來越多的 segment，直到出現超時的現象。
2. 壅塞避免
   - 將 cwnd 的值設成遇到壅塞的值的一半，開始以線性增長的方式發送 segment。
3. 快速恢復

以此類推我們就能慢慢控制 ssthresh，來讓其封包的壅塞控制逐漸穩定。

<img src="https://i.imgur.com/RmxbDKx.png" alt="image-20221116035319344" style="zoom:67%;" />