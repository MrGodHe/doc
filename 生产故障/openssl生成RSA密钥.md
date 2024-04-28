1、生成密钥

输入“genrsa -out rsa_private_key.pem 1024”命令,回车后,在当前 bin 文件目 录中会新增一个 rsa_private_key.pem 文件,

2、生成公钥

输入“rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem”命令回车 后,在当前 bin 文件目录中会新增一个 rsa_public_key.pem 文件

3、将原始私钥转换为pkcs8格式（Java用户需要转，php,.net等用户不用转）
openssl pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt  xxx_key_pkcs

