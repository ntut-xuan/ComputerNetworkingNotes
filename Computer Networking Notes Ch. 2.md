# Computer Networking Notes Ch. 2

## How to create a network app?

- 軟體主要是開發在終端上面，也就是 application layer 相互運作。
- 在開發網路軟體時，不需要考慮到網路設備。



## Architecture

### Client - Server architecture

- Server
  - 向 client 提供資料。
  - 永遠都在開機狀態。
  - 具有固定的 IP Address。
- Client
  - 向伺服器做聯繫並取得資料。
  - 不會與其他 client 端做聯繫。



### P2P architecture

- 去中心化，client 也有可能是 server，也有可能兩者都是。
- 一個 peers 會像其他的 peers 請求資訊，因此會從其他的 peers 取得資料。
- peers 的 IP 是動態的。
- 最常使用的是 P2P File Sharing。



