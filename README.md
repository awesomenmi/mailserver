# Почта: SMTP, IMAP, POP3 

Отправка почты с помощью telnet:
```
[root@localhost homew31]# telnet 127.0.0.1 20025
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
220 mailserver.local ESMTP Postfix
ehlo mailserver.local
250-mailserver.local
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
mail from: username@example.com
250 2.1.0 Ok
rcpt to: vagrant@mailserver.local
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
Subject: hello
Greetings
.
250 2.0.0 Ok: queued as 1D7FE400A4C9
```

Содержимое лога /var/log/maillog:
```
Jul 15 16:29:33 localhost postfix/smtpd[29241]: connect from gateway[10.0.2.2]
Jul 15 16:31:48 localhost postfix/smtpd[29241]: 1D7FE400A4C9: client=gateway[10.0.2.2]
Jul 15 16:33:11 localhost postfix/cleanup[29245]: 1D7FE400A4C9: message-id=<>
Jul 15 16:33:11 localhost postfix/qmgr[29235]: 1D7FE400A4C9: from=<username@example.com>, size=214, nrcpt=1 (queue active)
Jul 15 16:33:11 localhost postfix/local[29246]: 1D7FE400A4C9: to=<vagrant@mailserver.local>, relay=local, delay=107, delays=107/0.01
/0/0, dsn=2.0.0, status=sent (delivered to mailbox)
Jul 15 16:33:11 localhost postfix/qmgr[29235]: 1D7FE400A4C9: removed
Jul 15 16:33:42 localhost postfix/smtpd[29241]: disconnect from gateway[10.0.2.2]
```

Для просмотра письма в почтовом ящике подключимся через telnet по IMAP:
```
[root@localhost homew31]# telnet localhost 20143
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
* OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE AUTH=PLAIN] Dovecot ready.
a login vagrant password
a OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE SNIPPET=FUZZY SPECIAL-USE] Logged in
b select inbox
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 1 EXISTS
* 1 RECENT
* OK [UNSEEN 1] First unseen.
* OK [UIDVALIDITY 1594831015] UIDs valid
* OK [UIDNEXT 2] Predicted next UID
b OK [READ-WRITE] Select completed (0.002 + 0.000 + 0.002 secs).
b UID SEARCH *
* SEARCH 1
b OK Search completed (0.001 + 0.000 secs).
a UID FETCH 1 (BODY.PEEK[1.MIME])
* 1 FETCH (UID 1 BODY[1.MIME] {321}
Return-Path: <username@example.com>
X-Original-To: vagrant@mailserver.local
Delivered-To: vagrant@mailserver.local
Received: from mailserver.local (gateway [10.0.2.2])
	by mailserver.local (Postfix) with ESMTP id 1D7FE400A4C9
	for <vagrant@mailserver.local>; Wed, 15 Jul 2020 16:31:24 +0000 (UTC)
Subject: hello

)
a OK Fetch completed (0.001 + 0.000 secs).
```

Подключение с помощью Thunderbird:

![alt-текст](https://github.com/awesomenmi/mailserver/blob/master/Screenshot%20from%202020-07-15%2019-49-27.png)

![alt-текст](https://github.com/awesomenmi/mailserver/blob/master/Screenshot%20from%202020-07-15%2019-49-55.png)
