# imap_smtp_training

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

    openssl s_client -crlf -connect mail.flomail.net:993

Once connected use vinhtest1@123flo.com:123456 to log in

    < * OK test ready
    > A LOGIN vinhtest1@123flo.com 123456
    < A OK testuser authenticated
    
## Querry cho để trả về tất cả các folder dưới dạng array or map.
A LIST "" "*"
## Query để trả về tất cả folder mà một user đã subcribe.
A LSUB "" "*"
## Subcribe một mail box ví dụ Junk folder.
A SUBSCRIBE "junk"
## UnSubcribe một mail box ví dụ Junk folder.
A UNSUBSCRIBE "Junk" 
## Tạo một mailbox để store emails
A CREATE "testfolder"
## Đổi tên testfolder thành rename_folder
A RENAME "testfolder" "rename_folder

DELETE "path/to/mailbox"

SELECT/EXAMINE
STATUS (X Y X)


APPEND mailbox (flags) date message

STORE / UID STORE, updates flags for selected UIDs

EXPUNGE deletes all messages in selected mailbox marked with \Delete

COPY / UID COPY sequence mailbox

sends results to socket
