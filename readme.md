

# Guide по просмотору сертификата и его проверке по sha-256


## Просмотреть всю открытую часть сертификата

```
openssl x509 -in cert.pem -text -noout
```

## Просмотреть fingerpint

```
openssl x509 -in tls.crt -fingerprint -noout
```

## Просмотреть дату создания и его окончания срока действия

```
openssl x509 -in tls.crt -dates -noout
```

## Проверить, кто выдал сертификат

```
openssl x509 -in tls.crt -issuer -noout
```

## Просмотр subject

```
openssl x509 -in tls.crt -subject -noout
```

## Просмотр SubjectAltName

```
openssl x509 -in tls.crt -ext subjectAltName -noout
```

## Просмотр Serial Number в hex-формате

```
openssl x509 -in tls.crt -serial -noout
```

## Просмотр Serial Number как в выводе -dates -noout

```
openssl x509 -in tls.crt -serial -noout | cut -d= -f2 | sed 's/../&:/g; s/:$//'
```

## Если больше одного файла, и tls.crt также содержит например Intermediate + RootCA

```
openssl crl2pkcs7 -nocrl -certfile tls.crt | openssl pkcs7 -print_certs -text -noout | grep "Issuer:"
```

## Проверка sha256 суммы для открытой части сертификата

```
openssl x509 -in tls.crt -pubkey -noout | openssl pkey -pubin -outform DER | openssl dgst -sha256
```

## Проверка sha256 суммы для закртой части сертификата, если начало сертификата начинается с -----BEGIN RSA PRIVATE KEY-----


```
openssl rsa -in tls.key -pubout -outform DER | openssl dgst -sha256
```

## Проверка sha256 суммы для закртой части сертификата, если начало сертификата начинается с -----BEGIN EC PRIVATE KEY-----

```
openssl ec -in tls.key -pubout -outform DER | openssl dgst -sha256
```
________________
## В ходе проверки принадлежности открытой части сертификата и закрытой sha-256 должны быть одинаковы!
