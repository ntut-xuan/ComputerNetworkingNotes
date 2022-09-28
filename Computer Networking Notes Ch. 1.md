# Computer Networking Notes Ch. 1

## Internet Protocal Stack

- 主要分成五層。
  - application: 主要是應用程式的一些協定，例如：IMAP、SMTP、HTTP。
  - transport: 主要是資料傳輸的一些協定，例如：TCP、UDP。
  - network: 解決資料轉送與路由的問題，例如：IP。
  - link: 解決硬體與軟體連接的問題：例如：乙太網路、802.11。
  - physical: 主要是各種硬體，例如：數據機。


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
