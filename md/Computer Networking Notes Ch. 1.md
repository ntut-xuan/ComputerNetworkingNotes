# Computer Networking Notes Ch. 1

## Access network

- Cable-based access
  - 基於電纜連線
  - HFC，混合光纖同軸電纜
- 電話線撥接 Digital subscriber line
  - 基於現有電話線進行網路連線
- Wireless access networks
  - Wireless local area networks 區域無線網路 (WLANs)
    - 涵蓋於單一場所周圍(100ft內)
    - 802.11b/g/n/ac/ax
      - 11,54,450 Mbps......越新越快
  - Wide-area cellular access networks
    - 手機行動網路，基地臺大範圍覆蓋
    - 4G、5G 行動上網

## Physical media

- Twisted pair (雙絞線)
- Coaxial cable (同軸電纜)
- Fiber optic cable (光纖)
- Wireless radio (無線)

## Network core

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
  - 暫存器填滿時，封包可能會被丟棄 (dropped)，造成封包遺失
- 總共 n 個用戶，於指定時間內 k 個用戶同時傳輸的機率：
  - Binomial distribution
  - $P(\text{Total n users, at given time, k users transmitting simultaneously})=\displaystyle{C^n_k P^k(1-P)^{n-k}}$
- 總共 n 個用戶，於指定時間內 k 個用戶以上同時傳輸的機率：
  - $P(\text{Total n users, at given time, k users or more transmitting simultaneously})=\displaystyle{\sum^n_{i=k} C^n_i P^i(1-P)^{n-i}}$

### Circuit switching

- Unnecessary store-and-forward
- 端對端之間常時佔用，不共享頻寬
- 交換器沒有 queueing delay
- 當鏈路已滿時，拒絕新連線
- Frequency Division Multiplexing (FDM)
  - 利用光、電進行分頻，在該頻段下進行傳輸
- Time Division Multiplexing (TDM)
  - 分時段，各連線可在分配的時間內佔用範圍內之頻段進行傳輸

## Internet Protocal Stack

- 主要分成五層。
  - application：主要是應用程式的一些協定，例如：IMAP、SMTP、HTTP。
  - transport：主要是資料傳輸的一些協定，例如：TCP、UDP。
  - network：解決資料轉送與路由的問題，例如：IP。
  - link：解決硬體與軟體連接的問題：例如：乙太網路、802.11。
  - physical：主要是各種硬體，例如：數據機。


<img src="https://i.imgur.com/ROfdnT7.png" alt="image-20220928104435086" style="zoom:50%;" />

## ISO/OSI 七層模組

- 比 Internet protocal stack 多兩層，多了 presentation 與 session。
- 在 Internet protocal stack 中，將 presentation 與 session 放到 application layer 去做。
- 在 presentation 中，主要是讓應用程式做加解密、壓縮等問題。
- 在 session 中，主要是作用在校正、同步等問題

<img src="https://i.imgur.com/qq7pbfN.png" alt="image-20220928104008681" style="zoom:50%;" />



## Encapsulation / Decapsulation

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

​    

<img src="https://i.imgur.com/eepuz5p.png" alt="image-20220928105527026" style="zoom: 50%;" />
