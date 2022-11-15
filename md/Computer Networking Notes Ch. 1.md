# Computer Networking Notes Ch. 1

## Internet 的簡介

- 世界規模的電腦網路，可以與世界數十億的電子產品進行網路互動。
- 使用有線或者無線的方式進行連線，包括汽車、手機、筆電等等都需要 Internet。

<img src="https://i.imgur.com/JXTDrX8.png" alt="image-20221114174054685" style="zoom:75%;" />



## Protocol 的簡介

- 概念上即為「與人進行互動」的類比。
- 與人詢問問題會先打招呼→詢問問題→得到問題，而網路協議就如同類似的東西。

<img src="https://i.imgur.com/tmimKWF.png" alt="image-20221114174349422" style="zoom:67%;" />



## 邊緣網路

- 與網路連接的電腦或設備，稱為邊緣系統，也稱為主機（host）。



### 存取網路的方法

- 考慮到主機到網路的物理連接方法，也就是存取網路的方法。
- 我們可以分成以下幾種物理連接方法：
  - 一般家庭網路連接的方式：使用數據機（DSL）、電纜（Cable）、FTTH、撥號（Dial-Up）或衛星（Satellite）。
  - 企業網路連接的方式：Ethernet 或 Wi-Fi。
  - 廣域網路連接的方式：3G、LTE。



## 網路核心

### Packet switching

- Store-and-forward
  - 封包須完整送達路由器，才會再傳輸至下一節點。
- Transmission rate
  - $R = \text{Transmission rate} (\frac{\text{bits}}{\text{sec}} \text{ or bps})$
  - 單位時間內可傳送的資料大小
- Transmission delay
  - $L = \text{Packet size} (\text{bits})$
  - $\displaystyle{d_{trans} = \frac{L}{R}}\ (\text{sec})$
- Queueing delay & packet loss
  - 資料到達速度大於資料送出的速度時，封包將會在路由器的暫存器中排隊
  - 暫存器填滿時，封包無法加入等待而被丟棄（dropped），造成封包遺失（Packet loss）
- 總共 $n$ 個用戶，於指定時間內 $k$ 個用戶同時傳輸的機率：
  - 使用 Binomial distribution 進行運算
  - $P(\text{Total n users, at given time, k users transmitting simultaneously})=\displaystyle{C^n_k P^k(1-P)^{n-k}}$
- 總共 $n$ 個用戶，於指定時間內 $k$ 個用戶以上同時傳輸的機率：
  - 可理解為多個 Binomial distruibution 的總和
  - $P(\text{Total n users, at given time, k users or more transmitting simultaneously})=\displaystyle{\sum^n_{i=k} C^n_i P^i(1-P)^{n-i}}$



### Circuit switching

- 不必要進行 Store-and-forward
- 端對端之間常時佔用，不共享頻寬
- 交換器沒有 Queueing Delay
- 當鏈路（Link Capacity）已滿時，拒絕新連線
- Frequency Division Multiplexing（FDM）
  - 各連線僅獨佔其所屬頻段進行傳輸（不同的連線用不同的載波）
- Time Division Multiplexing（TDM）
  - 各連線僅在其分配的時間內獨佔電路進行傳輸



## Packet Switch 的時延、掉封包與吞吐量

### Packet delay

$d_{total} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$

1. processing delay（$d_{proc}$）：分析 packet 中的資訊（如錯誤檢查）與決定下個傳送目的地
2. queueing delay（$d_{queue}$）：因為 router 一次只能傳送一個 packet，還沒輪到的在 queue（buffer）中等待
3. transmission delay（$d_{trans}$）：將 packet 送出到 link 上需要時間（$\frac{L}{R}$），取決於 router 的頻寬
4. propagation delay（$d_{prop}$）：在 link 上傳輸的時間（$\frac{d}{s}$），距離越長延遲越久



### Queueing delay

當 packet 送入的速率越接近從 router 送出的速率，在 queue 中等待的時間越長，呈指數成長；在送入大於送出後，發生 packet loss，queueing delay $\rightarrow \infty$。



## 協定層次與服務模型

### Internet Protocal Stack

- 主要分成五層。
  - application：主要是應用程式的一些協定，例如：IMAP、SMTP、HTTP。
  - transport：主要是資料傳輸的一些協定，例如：TCP、UDP。
  - network：解決資料轉送與路由的問題，例如：IP。
  - link：解決硬體與軟體連接的問題：例如：乙太網路、802.11。
  - physical：主要是各種硬體，例如：數據機。


<img src="https://i.imgur.com/ROfdnT7.png" alt="image-20220928104435086" style="zoom:50%;" />

### ISO/OSI 七層模組

- 比 Internet protocal stack 多兩層，多了 presentation 與 session。
- 在 Internet protocal stack 中，將 presentation 與 session 放到 application layer 去做。
- 在 presentation 中，主要是讓應用程式做加解密、壓縮等問題。
- 在 session 中，主要是作用在校正、同步等問題

<img src="https://i.imgur.com/qq7pbfN.png" alt="image-20220928104008681" style="zoom:50%;" />



### Encapsulation / Decapsulation

> Need more infomation (NMI)

- 對於資料的傳輸，會從任何模組的最上層加標頭（header）加到最下層，再進行傳輸（Encapsulation）。
- 裝置與裝置之間的傳輸，目標裝置會解發送裝置提供的標頭，來得知資訊，這個動作叫做解封裝（Decapsulation）。
- 每一層的標頭分別是：

|    layer    |  header  |
| :---------: | :------: |
| application | message  |
|  transport  | segment  |
|   network   | datagram |
|    link     |  frame   |

<img src="https://i.imgur.com/eepuz5p.png" alt="image-20220928105527026" style="zoom: 50%;" />
