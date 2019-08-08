## Training về mailserver


## commands thường dùng

- **APPEND**
- **CAPABILITY**
- **CHECK**
- **CLOSE**
- **COPY**
- **CREATE**
- **DELETE**
- **EXPUNGE**
- **FETCH**
- **LIST**
- **LOGIN**
- **LOGOUT**
- **LSUB**
- **NOOP**
- **RENAME**
- **SEARCH**
- **SELECT**
- **STARTTLS**
- **STATUS**
- **STORE**
- **SUBSCRIBE**
- **UID COPY**
- **UID STORE**
- **UNSUBSCRIBE**

Những commands mở rộng một vài server sẽ setting để support or not.

- **Conditional STORE** rfc4551 and **ENABLE**
- **Special-Use Mailboxes** rfc6154
- **ID extension** rfc2971
- **IDLE command** rfc2177
- **NAMESPACE** rfc2342 (hard coded single user namespace)
- **UNSELECT** rfc3691
- **AUTHENTICATE PLAIN** and **SASL-IR**

Connect to the server on port 993

    openssl s_client -crlf -connect 123flo.com:993

Khi connected use vinhtest1@123flo.com:123456 to log in. Server sẽ show những commands đã được support ví dụ **CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS SPECIAL-USE BINARY MOVE QUOTA**.

    < * OK test ready
    > A LOGIN vinhtest1@123flo.com 123456
    < A OK vinhtest1@123flo.com authenticated
    
![LOGIN](https://user-images.githubusercontent.com/6763141/62596052-ebe55e80-b90a-11e9-8ec7-20d564960758.png)
Lấy những folder special use như sent, inbox, .... Một số trường hợp nhiều mail server không có thư mục sent với special use \Sent. Nếu không xử lý chính xác có thể gây ra lỗi khi sent không lưu được vào chính xác sent folder. 

A XLIST "" "*" 
![image](https://user-images.githubusercontent.com/6763141/62614651-0b45b100-b936-11e9-8d9b-96a2e09a91dc.png)

## Querry cho để trả về tất cả các folder dưới dạng array or map.
A LIST "" "*"
![LIST](https://user-images.githubusercontent.com/6763141/62590249-e4678a80-b8f5-11e9-9a47-49116cba3106.png)

## Query để trả về tất cả folder mà một user đã subcribe.
A LSUB "" "*"
![unsubscribe](https://user-images.githubusercontent.com/6763141/62590403-6c4d9480-b8f6-11e9-9376-a99978398b51.png)

## Subcribe một mail box ví dụ rename_folder folder.
A SUBSCRIBE "rename_folder"
![subscribe](https://user-images.githubusercontent.com/6763141/62590470-ad45a900-b8f6-11e9-81b6-86aa48abc21c.png)

## UnSubcribe một mail box ví dụ rename_folder folder.
A UNSUBSCRIBE "rename_folder" 
![unsubscribe](https://user-images.githubusercontent.com/6763141/62590470-ad45a900-b8f6-11e9-81b6-86aa48abc21c.png)

## Tạo một mailbox để store emails đồng thời subscribe đến mailbox đó
A CREATE "testfolder"
![create_and_subcribe](https://user-images.githubusercontent.com/6763141/62590654-41b00b80-b8f7-11e9-89af-fe365df1c9c8.png)

## Đổi tên testfolder thành rename_folder
A RENAME "testfolder" "rename_folder
## Xóa testfolder thành rename_folder
DELETE "rename_folder"
## Examine vs Select
EXAMINE và select trả về chung một kết quả tuy nhiên nếu có nhiều client đang connect đến mailserver thì EXAMINE không làm ảnh hưởng đến \recent flag. Vì nếu có một message nhận bởi mail box có read/write sessions dùng select thì khả năng những sessions khác không được notify đã nhận messages.
![image](https://user-images.githubusercontent.com/6763141/62591557-2e526f80-b8fa-11e9-871a-e9b0f42aa788.png)

## Dùng để lấy status của mail box ví dụ bao nhiêu một chưa đọc, unseen này nọ. có thể dùng kết hợp với cả lệnh list để trả về status của tất cả những mail box. 
STATUS (X Y X)

![image](https://user-images.githubusercontent.com/6763141/62591170-ed0d9000-b8f8-11e9-9523-aa7eed16d89e.png)
# ví dụ về Lệnh SEARCH. Những lệnh này khá quan trọng trong khi chúng ta xử lý tìm kiếm trong mail client. Tôi đưa ra ví dụ để bạn build câu query phù hợp dùng mailcore, node-imap nhé .

## Chúng ta muốn search những messages với cờ \\Answered
A SEARCH ANSWERED, SEARCH ANSWERED, SEARCH DELETED, SEARCH DRAFT, SEARCH FLAGGED, SEARCH SEEN, SEARCH RECENT
![image](https://user-images.githubusercontent.com/6763141/62596381-459a5880-b90c-11e9-9d4b-eb0e43ee3194.png)
![image](https://user-images.githubusercontent.com/6763141/62597906-ba23c600-b911-11e9-99a8-f543a20fcb4b.png)
## A SEARCH RECENT 
Từ gmail mình gửi một message đến vinhtest1@123flo.com mình muốn tìm recent message. Bạn có thể thấy message vừa mới được gửi đến.

![image](https://user-images.githubusercontent.com/6763141/62598173-b17fbf80-b912-11e9-8608-99ed8cc8aa02.png)


## Tìm kiếm với thời gian đã chỉ định ví dụ vào SINCE 7-Aug-2019 mình đã nhận một message.

A SEARCH SENTBEFORE 12-Mar-2016
A SEARCH SENTON 12-Mar-2016
A SEARCH SENTSINCE 12-Mar-2016

A SEARCH SINCE 12-Mar-2016
A SEARCH ON 12-Mar-2016
A SEARCH BEFORE 12-Mar-2016
![image](https://user-images.githubusercontent.com/6763141/62599177-bb56f200-b915-11e9-8c84-e63cc35befe5.png)

Chúng ta có thể dùng để tìm kiếm một message đã gửi và lưu vào imap mailbox vào thời gian đã gửi đi 1h trước với OLDER và YOUNGER. Bạn có thể thấy trong 1h mình đã nhận một message từ gmail với **sequence number** 331. Ở đây tồi propose cách xử lý cho những giao thức bất đồng bộ. Hoặc là có thể khách hàng muốn xử lý những message đã gửi nhận trong 2 tuần cho cái contact vinhtest1@123flo.com.

A SEARCH OLDER 3600

![image](https://user-images.githubusercontent.com/6763141/62599225-df1a3800-b915-11e9-9310-98218eb29eaa.png)
## Tìm kiếm message với properties FROM, TO, BCC, CC, BODY, SUBJECT. Việc tìm kiếm cũng tốn khá nhiều thời gian và thỉnh thoảng chi phí tìm kiếm với subject cũng rất lớn với những complex header và complex query so với body. 

A SEARCH TO vinhtest1@123flo.com
A SEARCH FROM vinhtest1@123flo.com
A SEARCH CC vinhtest1@123flo.com
A SEARCH BCC vinhtest1@123flo.com
A SEARCH BODY test 
A SEARCH HEADER RECEIVED test
![image](https://user-images.githubusercontent.com/6763141/62599760-497fa800-b917-11e9-8fd8-c5576c9239cc.png)

## Thỉnh thoảng việc tìm kiếm khá phức tạp đòi hỏi phải build nhưng câu query vô cùng complex. Đòi hỏi câu query được build với nhiều criteria kết hợp lại với nhau.
#### A SEARCH FROM vinhtest@g123flo.com SINCE 13-May-2019
#### A SEARCH OR FROM vinhtest1@123flo.com FROM vinhtest2@123flo.com
#### A SEARCH OR (FROM vinhtest1@123flo.com) (FROM vinhtest2@123floc.om)
#### A SEARCH OR OR FROM vinhtest1@123flo.com FROM vinhtest2@123flo.com FROM vinhtest3@123flo.com **chú ý rằng khi tìm kiếm toán tử or chỉ nhận 2 input cho nên anh em chú ý cách build câu query cho phù hợp nhé**
#### A SEARCH OR (FROM vinhtest1@github.com SINCE 13-Mar-2016) FROM vinhtest2@123flo.com
#### A SEARCH NOT (OR (FROM vinhtest1@123flo.com) (BEFORE 13-May-2019))
#### A SEARCH NOT SEEN
#### A SEARCH UNSEEN
Để nhận  message với UIDs thì anh em bắt buộc phải thêm prefix UID nhé 

##### A UID SEARCH SINCE 13-May-2019
##### A UID SEARCH OR FROM vinhtest1@123flo.com FROM vinhtest2@123flo.com
##### A UID SEARCH TO vinhtest1@123flo.com

Tìm kiếm trả về UIDs thỉnh thoảng good hơn sequence no for 1 vài lý do để integrate với your system. last example có thể là 1 good strategy a for mailbox listener để xử lý tất cả UIDs after the last seen và tất cả unseen messages.

#### A UID SEARCH UID 1:*
#### A SEARCH UID 1:*
#### A UID SEARCH OR (UID 1:*) (UNSEEN)
# mailserver của flo có hỗ trợ ESEARCH nhé ví dụ

## Đêm UNSEEN messages và  trả về messageNo/UID đầu tiên.

#### A SEARCH RETURN (MIN COUNT) UNSEEN
#### A UID SEARCH RETURN (MIN COUNT) UNSEEN
![image](https://user-images.githubusercontent.com/6763141/62601732-433ffa80-b91c-11e9-8448-315ca4587654.png)

# 1. Fetch Ví dụ. Domail đóng vai trò là 1 forwarder. Send những commands to server thông qua actions của user và browser requests.  
Fetch the nội dung của  1 email message thông qua những commands:
Ví dụ fetch nội dung của một message thông qua sequence number 

A FETCH 1 BODY.PEEK[TEXT]
![image](https://user-images.githubusercontent.com/6763141/62609003-2b23a780-b92b-11e9-9a6e-ddf7f4140569.png)

#### Fetch the header of the message:

A FETCH 1 BODY.PEEK[HEADER]
![image](https://user-images.githubusercontent.com/6763141/62609052-42fb2b80-b92b-11e9-9e32-81d2db3220db.png)

#### Fetch header and contents of email message

A FETCH 1 BODY.PEEK[]
![image](https://user-images.githubusercontent.com/6763141/62609118-5c9c7300-b92b-11e9-8954-dfce41ea3ae2.png)

#### Fetch những phần cụ thể của 1 message sâu hơn chúng ta sẽ discuss về node-imap và cách parse body, header lẫn metadata.

A FETCH 1 BODY.PEEK[HEADER.FIELDS (Date From)]
![image](https://user-images.githubusercontent.com/6763141/62609609-5064e580-b92c-11e9-9ce9-cb35cd9b675b.png)

A FETCH 1 BODY.PEEK[HEADER.FIELDS.NOT (Date From)]
![image](https://user-images.githubusercontent.com/6763141/62609689-696d9680-b92c-11e9-8091-be27d785e7ff.png)

#### Chúng ta cũng có thể Fetch metadata bằng những command dưới đây.

A FETCH 1 FLAGS
A FETCH 1 ENVELOPE
A FETCH 1 INTERNALDATE
A FETCH 1 RFC822.SIZE
A FETCH 1 BODYSTRUCTURE.PEEK
A FETCH 1 UID

#### Fetches cũng có thể dùng kết hợp với metadata

A FETCH 1 (BODYSTRUCTURE.PEEK UID)
A FETCH 1 (BODYSTRUCTURE.PEEK UID RFC822.SIZE) **trong vài trường hợp mình chỉ đi lấy 1 part của mail. Ví dụ part đó chứa file mình đi đọc cái file size đó lên hoặc là đọc file size sau khi parse ra** 
Trong 1 vài trường hợp để improve performance một vài mail client dùng cả <start-index.length>

A FETCH 1 (BODYSTRUCTURE.PEEK BODY.PEEK[]<0.200>)
