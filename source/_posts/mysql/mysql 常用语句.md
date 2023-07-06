### mysql 加解密

```sql
# AES
SELECT TO_BASE64(AES_ENCRYPT(crypt_str, key_str));
SELECT AES_DECRYPT(FROM_base64(crypt_str),key_str);
```

