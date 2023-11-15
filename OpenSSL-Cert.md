```
openssl x509 -in certificat.com.crt -text -noout

# This will create the request for file.csr
openssl x509 -x509toreq -conf openss-public.cnf -signkey CERT-public.key -in CERT-public.crt -out CERT-public.csr

```


``` # CSR : "Certificate Signing Request" - "Pedido de Assinatura de Certificado" É em CSR é um bloco de texto codificado em ASN.1 (Abstract Syntax 

Notation One) que contém informações sobre a organização e a chave pública a ser incluída em um certificado SSL" É gerado pelo servidor e 

enviado para CA para emissao do certificado

``` 

``` # .PEM : Um arquivo com extensão .pem geralmente contém dados codificados em formato PEM (Privacy Enhanced Mail). No contexto de certificados 

digitais e chaves criptográficas, os arquivos .pem são comumente usados para armazenar certificados X.509 e chaves privadas. É importante 

ressaltar que um arquivo .pem pode conter tanto um certificado quanto uma chave privada, dependendo do conteúdo entre os cabeçalhos e rodapés. 

Em alguns casos, você pode encontrar arquivos .pem que contêm ambos.
```


