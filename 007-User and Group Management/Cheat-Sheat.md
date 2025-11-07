# 👤 User and Group Management in Linux

<img src="./img/user_management.webp">

```bash
grep -v "#" /etc/login.defs | grep -v '^$'
```
```bash
MAIL_DIR	/var/spool/mail
UMASK		022
HOME_MODE	0700
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_WARN_AGE	7
UID_MIN                  1000
UID_MAX                 60000
SYS_UID_MIN               201
SYS_UID_MAX               999
SUB_UID_MIN		   100000
SUB_UID_MAX		600100000
SUB_UID_COUNT		    65536
GID_MIN                  1000
GID_MAX                 60000
SYS_GID_MIN               201
SYS_GID_MAX               999
SUB_GID_MIN		   100000
SUB_GID_MAX		600100000
SUB_GID_COUNT		    65536
ENCRYPT_METHOD SHA512
SHA_CRYPT_MAX_ROUNDS 100000
USERGROUPS_ENAB yes
CREATE_HOME	yes
HMAC_CRYPTO_ALGO SHA512
```