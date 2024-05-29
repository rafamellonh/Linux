```
Output the current certificate information (for future validation): It's possible check the info about the certificat
openssl x509 -in certificat.com.crt -text -noout

```

```

# This will create the request for file.csr
openssl x509 -x509toreq -conf openss-public.cnf -signkey CERT-public.key -in CERT-public.crt -out CERT-public.csr

openssl req -out foundation-dev.ent.org.com.csr -config /etc/pki/tls/openssl.cnf -key etc/pki/tls/private/localhost.key -new

openssl req -new -key stratus-nonprod-public.key -out stratus-nonprod-public.csr -config openssl-public.cnf


```

```
check public key between .csr and .crt, if it is the same, the .csr is good.

* CRT :  openssl x509 -noout -modulus -in /etc/pki/tls/certs/certificat.crt | openssl md5

* CSR :  openssl req -noout -modulus -in /etc/pki/tls/private/certificat.csr | openssl md5

* KEY : sudo openssl rsa -noout -modulus -in /etc/pki/tls/private/localhost.key | openssl md5

```

![Cert.](/images/cert.jpg "This is a sample image.")




* CSR : "Certificate Signing Request" - "Pedido de Assinatura de Certificado" É em CSR é um bloco de texto codificado em ASN.1 (Abstract Syntax Notation One) que contém informações sobre a organização e a chave pública a ser incluída em um certificado SSL" 
É gerado pelo servidor e enviado para CA para emissao do certificado

 

* .PEM : Um arquivo com extensão .pem geralmente contém dados codificados em formato PEM (Privacy Enhanced Mail). No contexto de certificados 
digitais e chaves criptográficas, os arquivos .pem são comumente usados para armazenar certificados X.509 e chaves privadas. 
É importante ressaltar que um arquivo .pem pode conter tanto um certificado quanto uma chave privada, dependendo do conteúdo 
entre os cabeçalhos e rodapés. Em alguns casos, você pode encontrar arquivos .pem que contêm ambos.



* se o certificado usa Bundle, trocar so a primeira parte do arquivo e comparar as outras duas com o arquivo ChainBundle2 que vai receber.

 -----BEGIN CERTIFICATE-----
MIIDITCCAoqgAwIBAgIUD9AYnC0NWojQ.... (certificado)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIDITCCAoqgAwIBAgIUD9AYnC0NWojQ.... (certificado intermediário ou certificado de autoridade intermediária)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIEpAIBAAKCAQEAy3FVHEEosFX1OJU5.... (certificados raiz)
-----END CERTIFICATE-----






```
Create an OpenSSL configuration file named san.cnf using the following information.
[ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = req_ext
[ req_distinguished_name ]
countryName         = Country Name (2 letter code)
stateOrProvinceName = State or Province Name (full name)
localityName        = Locality Name (eg, city)
organizationName    = Organization Name (eg, company)
commonName          = Common Name (e.g. server FQDN or YOUR name)
[ req_ext ]
subjectAltName = @alt_names
[alt_names]
DNS.1 = domain.com
DNS.2 = *.domain2.com
DNS.3 = *.domain3.com
DNS.4 = *.domain4.com



```









