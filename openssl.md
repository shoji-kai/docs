openssl
=======

* 自己証明書の作り方
  ```
  % mkdir registry_certs
  % sudo openssl req -newkey rsa:4096 -nodes -sha256 -keyout registry_certs/domain.key -x509 -days 365 -out registry_certs/domain.crt
  ```
