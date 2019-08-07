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

    openssl s_client -crlf -connect 123flo.com:993

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
## Xóa testfolder thành rename_folder
DELETE "rename_folder"
## Examine vs Select
EXAMINE và select trả về chung một kết quả tuy nhiên nếu có nhiều client đang connect đến 


The EXAMINE command is identical to SELECT and returns the same output; however, the selected mailbox is identified as read-only. No changes to the permanent state of the mailbox, including per-user state, are permitted; in particular, EXAMINE MUST NOT cause messages to lose the \Recent flag.

Why do we care about this the \Recent flag? What is it even for?

Message is “recently” arrived in this mailbox. This session is the first session to have been notified about this message; if the session is read-write, subsequent sessions will not see \Recent set for this message. This flag can not be altered by the client.

If there are multiple clients connecting to the same inbox, and if any of these clients rely on the \Recent flag, these clients may not be notified of new messages if one uses SELECT over EXAMINE

In reality, a client that relies on the \Recent flag being present is making too big of an assumption and should use other polling mechanisms such as searching for unread messages. I would still recommend using EXAMINE because it’s better to treat an object as immutable when the opportunity arises. A good example of this is the \Seen flag (a message without this will appear bold in your inbox, ie. unread), as when issuing a FETCH command, there are some parts of a message, that if retrieved, will implicitly mark the the message as read. This can have disastrous affects if someone is expecting unread messages to truly be unread, and an EXAMINE command may avoid this problem. The spec does not require this exact immutability behavior so check with the IMAP server before commiting.

STATUS (X Y X)


APPEND mailbox (flags) date message

STORE / UID STORE, updates flags for selected UIDs

EXPUNGE deletes all messages in selected mailbox marked with \Delete

COPY / UID COPY sequence mailbox

sends results to socket

``
lcl_dev@lcl-Web:~/projects/companies/FlowOnlineProductNewRepository/wildduck$ openssl s_client -crlf -connect 123flo.com:993
CONNECTED(00000003)
depth=0 CN = mail.123flo.com
verify error:num=20:unable to get local issuer certificate
verify return:1
depth=0 CN = mail.123flo.com
verify error:num=21:unable to verify the first certificate
verify return:1
---
Certificate chain
 0 s:/CN=mail.123flo.com
   i:/C=US/O=Let's Encrypt/CN=Let's Encrypt Authority X3
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFVTCCBD2gAwIBAgISA31AiP43zVpkgvzcJRXKkX+sMA0GCSqGSIb3DQEBCwUA
MEoxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MSMwIQYDVQQD
ExpMZXQncyBFbmNyeXB0IEF1dGhvcml0eSBYMzAeFw0xOTA3MDEwMzAzNTFaFw0x
OTA5MjkwMzAzNTFaMBoxGDAWBgNVBAMTD21haWwuMTIzZmxvLmNvbTCCASIwDQYJ
KoZIhvcNAQEBBQADggEPADCCAQoCggEBAJqhIIu7yrhZuho3BAQI43eewkgu0HiZ
+PFbHqbZtSx6JZmmACq1aU2ykq+HQw9spRtkO+D7taDioH3FABySOT0u+KdxmvOd
80wOIHSI9Rje4Qru6e9KOTjQJUkRChlNNyRj9DILn+GAsfobnF5yiBCjwutvzqxZ
9XGigM1RvOraEXIQUQ7eHzSj+GYLVGeEVcB7ctwwWTX2oXHJ3T2JI/18mh1HU1y3
jdjFB4Fr53tnLgCK4h+qbANXyMnYEbFlMISvshtLEawLijT1nTBh3AHh8TjxBpm/
c+mGH3ow+Jfj8SqApFx8rNwNEuoczoGGj38yQtcWaOZFez420BCnCfUCAwEAAaOC
AmMwggJfMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYB
BQUHAwIwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQUU8261sry+SLL0g7ccc3Xn+qO
K2cwHwYDVR0jBBgwFoAUqEpqYwR93brm0Tm3pkVl7/Oo7KEwbwYIKwYBBQUHAQEE
YzBhMC4GCCsGAQUFBzABhiJodHRwOi8vb2NzcC5pbnQteDMubGV0c2VuY3J5cHQu
b3JnMC8GCCsGAQUFBzAChiNodHRwOi8vY2VydC5pbnQteDMubGV0c2VuY3J5cHQu
b3JnLzAaBgNVHREEEzARgg9tYWlsLjEyM2Zsby5jb20wTAYDVR0gBEUwQzAIBgZn
gQwBAgEwNwYLKwYBBAGC3xMBAQEwKDAmBggrBgEFBQcCARYaaHR0cDovL2Nwcy5s
ZXRzZW5jcnlwdC5vcmcwggEDBgorBgEEAdZ5AgQCBIH0BIHxAO8AdQDiaUuuJujp
QAnohhu2O4PUPuf+dIj7pI8okwGd3fHb/gAAAWurtAkoAAAEAwBGMEQCIFZ2cOJo
TofpqIevMYIFKq6uMYlqwFzr90VcFMNA+7G1AiB+lgkbf2J9WR27pUQWziDir5q+
944gFC61uP7f2JFfVwB2ACk8UZZUyDlluqpQ/FgH1Ldvv1h6KXLcpMMM9OVFR/R4
AAABa6u0CU8AAAQDAEcwRQIgA7WXSLg7QUPjli8jrLAIoH8nS/t2+SIGEfqgiekE
ItsCIQCyuyvm6SoUvJUpx0siRmrqpPZ5lY9FgJLBr4laWkm0RzANBgkqhkiG9w0B
AQsFAAOCAQEAhDQXpaViOkThz+elH8CPKsF3q7BTRpibuF3+vGAlMgND0cF18AHO
kwAtkWHo9JXgPkF8/XD3+/U+umPOqPQbiNcIpYZXcyNsD0FiJlb89vSOk/H0Genq
Zc1iWhsRSrkN6gWW71u8x8QKfvsgQq52iKJsNWC/nfCio7BetVwUlhxXlRGi4oT1
03WD4WdP7/cXqkxjzq/OpTeCoX8Z77+LP9aUNu9PcI2vuXKBBvCnf/tiAotMuwQy
Q8fvwmhlMT5lQ97eSzmnUNh8Oe5EIDerwMCbTO+i+flIIR7TNIX0JDr87IGrowqd
34FBpID/J0CH/6gcMJwvPlsQcUYShDT4Xg==
-----END CERTIFICATE-----
subject=/CN=mail.123flo.com
issuer=/C=US/O=Let's Encrypt/CN=Let's Encrypt Authority X3
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: ECDH, P-384, 384 bits
---
SSL handshake has read 2060 bytes and written 463 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 626725F1306CD1CF1B10B79696BAEC04A523CD1A72E4E91F43E6B4167C6BFC7D
    Session-ID-ctx: 
    Master-Key: E8C89A418AB6EF35E7BBEBA79F60F32B1F056EC7A7CC3BD77FF1ABF09856F982CEECB6A95BEF7C5EEE02A5874BD07901
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - 8a 5f f4 82 5e 03 b8 8f-d3 da 31 1c 5a 49 02 9a   ._..^.....1.ZI..
    0010 - 20 8d 49 ae 25 96 d1 5b-36 da 89 6a 22 cd a7 e7    .I.%..[6..j"...
    0020 - 25 a8 94 1d d4 79 10 c0-ca 2d 2d b1 38 17 9d 8d   %....y...--.8...
    0030 - 85 e9 67 2a d2 15 d2 69-cc db 44 86 8b 1a dd a3   ..g*...i..D.....
    0040 - f3 ea 43 ca 46 c8 22 a4-a9 78 71 9d 7b d5 f4 91   ..C.F."..xq.{...
    0050 - f1 18 aa 8e c5 0d f4 b0-62 3a f1 b1 e2 12 79 47   ........b:....yG
    0060 - 50 fa 1f 7b 70 1e 17 66-e2 e8 18 3d 6a ab b3 02   P..{p..f...=j...
    0070 - f6 1b 53 7d 8e 7a 2e 1d-d1 11 fb 56 70 2b ae fe   ..S}.z.....Vp+..
    0080 - 41 5b d4 a8 f4 0e 19 dd-25 c4 5d 7b 5d 93 81 7b   A[......%.]{]..{
    0090 - 56 65 d0 f9 a3 bb ba 81-73 a3 83 68 fc 90 14 0b   Ve......s..h....

    Start Time: 1565141919
    Timeout   : 300 (sec)
    Verify return code: 21 (unable to verify the first certificate)
---
* OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE AUTH=PLAIN AUTH=LOGIN] Dovecot (Ubuntu) ready.
A LOGIN vinhtest1@123flo.com 123456
A OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS SPECIAL-USE BINARY MOVE QUOTA] Logged in
LIST "" "*"
LIST BAD Error in IMAP command "": Unknown command.
A LIST "" "*"
* LIST (\HasNoChildren \Trash) "." Trash
* LIST (\HasNoChildren \Drafts) "." Drafts
* LIST (\HasNoChildren) "." Omni
* LIST (\HasNoChildren \Sent) "." Sent
* LIST (\HasNoChildren \Junk) "." Junk
* LIST (\HasNoChildren) "." "TEST IPAD"
* LIST (\HasNoChildren) "." Notes
* LIST (\HasNoChildren) "." INBOX
A OK List completed.
A UNSUBSCRIBE "Junk"                 
A OK Unsubscribe completed.
A CREATE testfolder
A OK Create completed.
A LIST "" "*"
* LIST (\HasNoChildren \Trash) "." Trash
* LIST (\HasNoChildren \Drafts) "." Drafts
* LIST (\HasNoChildren) "." Omni
* LIST (\HasNoChildren \Sent) "." Sent
* LIST (\HasNoChildren) "." testfolder
* LIST (\HasNoChildren \Junk) "." Junk
* LIST (\HasNoChildren) "." "TEST IPAD"
* LIST (\HasNoChildren) "." Notes
* LIST (\HasNoChildren) "." INBOX
A OK List completed.
A RENAME "testfolder" "rename_folder"
A OK Rename completed.
A SELECT "INBOX"
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft NonJunk Junk $Forwarded)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft NonJunk Junk $Forwarded \*)] Flags permitted.
* 330 EXISTS
* 0 RECENT
* OK [UNSEEN 20] First unseen.
* OK [UIDVALIDITY 1501640115] UIDs valid
* OK [UIDNEXT 7278] Predicted next UID
* OK [HIGHESTMODSEQ 12214] Highest
A OK [READ-WRITE] Select completed (0.067 secs).
A EXAMINE INBOX
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft NonJunk Junk $Forwarded)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 330 EXISTS
* 0 RECENT
* OK [UNSEEN 20] First unseen.
* OK [UIDVALIDITY 1501640115] UIDs valid
* OK [UIDNEXT 7278] Predicted next UID
* OK [HIGHESTMODSEQ 12214] Highest
A OK [READ-ONLY] Examine completed (0.000 secs).
A SELECT "INBOX"
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft NonJunk Junk $Forwarded)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft NonJunk Junk $Forwarded \*)] Flags permitted.
* 330 EXISTS
* 0 RECENT
* OK [UNSEEN 20] First unseen.
* OK [UIDVALIDITY 1501640115] UIDs valid
* OK [UIDNEXT 7278] Predicted next UID
* OK [HIGHESTMODSEQ 12214] Highest
A OK [READ-WRITE] Select completed (0.000 secs).
``

https://nbsoftsolutions.com/blog/introduction-to-imap
