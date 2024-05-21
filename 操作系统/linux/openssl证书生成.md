# openSSL

======== 公私钥证书生成 ============

req -newkey rsa:2048 -nodes -keyout yjfboc.key -x509 -days 365 -out yjfboc.cer

pkcs12 -export -in yjfboc.cer -inkey yjfboc.key -out yjfboc.pfx