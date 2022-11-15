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
