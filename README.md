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

Once connected use vinhtest1@123flo.com:123456 to log in

    < * OK test ready
    > A LOGIN vinhtest1@123flo.com 123456
    < A OK testuser authenticated
    
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

