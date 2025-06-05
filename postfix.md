 Please refer the below steps for creating canonical_maps:

1. Modifying /etc/postfix/main.cf

 `vi /etc/postfix/main.cf`

 `sender_canonical_maps = hash:/etc/postfix/canonical  <- add`

2. Creating /etc/postfix/canonical with including the following

`vi /etc/postfix/canonical`

`user@bbb.co.jp user@aaa.co.jp`

3. Reflecting canonical.db to get the change work

`postmap /etc/postfix/canonical`

4.Restarting postfix service

`service postfix restart (or reload)`
