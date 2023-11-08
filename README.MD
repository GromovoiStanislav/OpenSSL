# OpenSSL

### Create symmetric keys:

```bash
openssl rand -hex 32
#311c4f85cbef453a6c7db6d5ee203736045c5e87406281d1cc69b95235d4ecea

openssl rand -base64 32
#5irs9K697RuP7bnJycu66lYPlfRMO7fNgf03sLT+LC4=

#in file
openssl rand -base64 32 > symmetric_key

#encrypt file
openssl enc -aes-256-cbc -salt -pbkdf2 -in input.txt -out encrypted.bin -pass file:symmetric_key
#decrypt file
openssl enc -d -aes-256-cbc -pbkdf2 -in encrypted.bin -out decrypted.txt -pass file:symmetric_key
```

### Create asymmetric RSA keys:

```bash
openssl genrsa -out private.key 2048
# Извлечение публичного ключа из приватного ключа
openssl rsa -in private.key -out public.key -outform PEM -pubout

#or with PEM pass phrase
openssl genpkey -algorithm RSA -out private_key.pem -aes256
# Извлечение публичного ключа из приватного ключа
openssl rsa -pubout -in private_key.pem -out public_key.pem

#or without PEM pass phrase
openssl genpkey -algorithm RSA -out private_key.pem
# Извлечение публичного ключа из приватного ключа
openssl rsa -pubout -in private_key.pem -out public_key.pem

#encrypt file
openssl rsautl -encrypt -pubin -inkey public_key.pem -in input.txt -out encrypted.txt
#decrypt file
openssl rsautl -decrypt -inkey private_key.pem -in encrypted.txt -out decrypted.txt

#signature
openssl dgst -sha256 -sign private_key.pem -out signature.bin input.txt
#verify
openssl dgst -sha256 -verify public_key.pem -signature signature.bin input.txt
#verify in file
openssl dgst -sha256 -verify public_key.pem -signature signature.bin -out verified.txt input.txt
```

### Create asymmetric DSA keys:

```bash
openssl dsaparam -out dsaparam.pem 2048
openssl gendsa -out private_dsa.pem dsaparam.pem
# Извлечение публичного ключа из приватного ключа
openssl dsa -in private_dsa.pem -pubout -out public_dsa.pem

#signature
openssl dgst -sign private_dsa.pem -out signature.bin input.txt
#verify
openssl dgst -verify public_dsa.pem -signature signature.bin input.txt
#verify in file
openssl dgst -verify public_dsa.pem -signature signature.bin -out verified.txt input.txt
```

### Create asymmetric ECDSA keys:

```bash
openssl ecparam -name secp256k1 -genkey -out private_ecdsa.pem
# Извлечение публичного ключа из приватного ключа
openssl ec -in private_ecdsa.pem -pubout -outform PEM -out public_ecdsa.pem

#signature
openssl dgst -sha256 -sign private_ecdsa.pem -out signature.bin input.txt
#verify
openssl dgst -sha256 -verify public_ecdsa.pem -signature signature.bin input.txt
#verify in file
openssl dgst -sha256 -verify public_ecdsa.pem -signature signature.bin -out verified.txt input.txt
```

### Create hash:

```bash
openssl dgst -sha256 input.txt
# -md5 -sha1 -sha224 -sha256 -sha384 -sha512 -sha3-256 -blake2s256

#in file
openssl dgst -sha256 -out hash.txt input.txt
```
