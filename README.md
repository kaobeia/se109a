# 期中報告
## 說明：  
### 功能：  
    1. 具有用戶上線註冊和下線註銷功能  
    2. 具有群聊功能  
    3. 具有統計聊天室在線人數的功能  
      
### 思路：  
1. 模式：對於聊天室就是處理多個客戶端發送的請求與信息，從而需要壹個服務器端去處理這些客戶端請求與信息，故采用的是服務器端/客戶端模式。  
2. 客戶端與服務器端的連接：既然是多個客戶端發送的請求與信息從而交給服務器端處理，那麽客戶端與服務器端之間需要進行連接，在我的博客：Socket編程 中介紹到兩臺計算機之間如何通過使用套接字建立TCP連接，從而進行通信操作。稍後會通過分析以及代碼的方式進行詳細操作。
3. 客戶端方面：客戶端在與服務器端建立連接後，通過Socket對象獲取輸入輸出流從而與服務器端之間進行通信。  
4. 服務器端方面：服務器端的套接字ServerSocket對象在調用accept()方法偵聽客戶端的連接，當與客戶端成功建立連接後，返回Socket對象，從而利用該Socket對象獲取輸入輸出流從而與客戶端進行通信。  

### 實現：  
### 1.客戶端與服務器端建立連接：  
利用Socket編程，客戶端與服務器端建立連接的步驟如下：  

（1）服務器端通過java.net.ServerSocket類的構造方法實例化ServerSocket對象，選擇的構造方法如下：  
```public ServerSocket (int port) throws IOException```  
該構造方法需要傳入參數：端口號，從而創建綁定到指定端口的服務器套接字。  
（2）客戶端通過java.net.Socket類的構造方法創建一個流套接字並將其連接到指定主機上的指定端口號，該構造方法如下：  
```public Socket(String host,int port) throws UnknowHostException,IOException```  
（3）服務器端的ServerSocket對象調用accept()方法偵聽請求連接指定端口號的該服務器端的客戶端，並在接收到客戶端請求後返回服務器端的流套接字，即Socket對象，從而服務器端與客戶端成功建立連接。  
（4）客戶端與服務器端之間的通信操作，java.net.Socket類就是提供客戶端與服務器端相互通信的套接字。  
獲取Socket套接字的輸入流的方法為：  
```public InputStream getInputStream() throws IOException```  
獲取Socket套接字的輸出流的方法為：  
```public OutputStream getOutputStream() throws IOException```  
在獲取了服務器端與客戶端的輸入輸出流之後，進行信息輸入輸出即可進行通信操作。  
（5）服務器端與客戶端之間通信結束後，需要關閉套接字，調用close()方法即可。close()方法如下：  
```public void close() throws IOException```  
### 2.用戶註冊的功能：  
上線註冊：  
客戶端輸入格式為userName:用戶名時，服務器端識別到客戶端需要進行的是註冊用戶操作後，將客戶端輸入的用戶名保存起來，並將該客戶端的用戶名作為ConcurrentHashMap的key值，客戶端的Socket對像作為ConcurrentHashMap的value值，將其添加到全局變量的ConcurrentHashMap對像中。  
下線註銷：  
客戶端輸入格式為輸入信息中包含exit字符串時，服務器端識別到客戶端需要進行註銷用戶操作後，通過遍歷ConcurrentHashMap對象的key值，key值所對應的value值與當前客戶端的Socket對象相等時，則找到了需要註銷的客戶端的用戶名，從而利用其key值將客戶端從ConcurrentHashMap對像中刪除。  
### 3.群聊功能：  
客戶端輸入格式為G：群聊信息時，服務器端識別到客戶端需要進行的群聊操作，服務器端通過遍歷ConcurrentHashMap對象的value值，獲取客戶端的Socket對像從而利用Socket對象取得輸出流後將群聊信息發送給每一個客戶端。同樣為了客戶體驗，其他客戶端需要知道是誰發送了群聊信息，故當前客戶端在發送群聊信息的同時告訴其他客戶端自己的用戶名。  
### 4.統計聊天室在線人數功能：  
對於統計聊天室在線人數功能，用於服務器端將每一個註冊用戶名的客戶端保存在ConcurrentHashMap對像中，故若統計聊天室在線人數只需調用ConcurrentHashMap對象的size()方法即可。該功能模塊在客戶端用戶註冊時已經實現，故不再單獨實現。  
## 參考資料：  
https://www.itread01.com/content/1544634208.html



