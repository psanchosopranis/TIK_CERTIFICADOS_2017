# Lab 01 RSA Key Pairs 
##### Apuntes para TIK sobre certificados (Dic 2017)

### TOOLS

>[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

>[CSR Decoder And Certificate Decoder(Red Krestel)(old)](https://certlogik.com/decoder/)

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)


### Preparación del laboratorio

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv labs/lab01_rsa
mkdir: se ha creado el directorio 'labs'
mkdir: se ha creado el directorio 'labs/lab01_rsa'
$ cd labs/lab01_rsa/
$ pwd
/home/devel1/labs/lab01_rsa

```

## SECCION 01: Generación y manejo de claves RSA con `openssl`

Ayuda de openssl para `genrsa`:

```
$ openssl genrsa --help
usage: genrsa [args] [numbits]
 -des            encrypt the generated key with DES in cbc mode
 -des3           encrypt the generated key with DES in ede cbc mode (168 bit key)
 -seed
                 encrypt PEM output with cbc seed
 -aes128, -aes192, -aes256
                 encrypt PEM output with cbc aes
 -camellia128, -camellia192, -camellia256
                 encrypt PEM output with cbc camellia
 -out file       output the key to 'file
 -passout arg    output file pass phrase source
 -f4             use F4 (0x10001) for the E value
 -3              use 3 for the E value
 -engine e       use engine e, possibly a hardware device.
 -rand file:file:...
                 load the file (or the files in the directory) into
                 the random number generator

```

### Ejemplo 01.01: Generar clave RSA en formato `PEM` sin cifrar `(PKCS#1)`

>[Public-Key Cryptography Standards `(PKCS)` (RSA Laboratories - _ARCHIVO_)](https://web.archive.org/web/20061209135809/http://www.rsasecurity.com/rsalabs/node.asp?id=2124)

>[02 - PKCS #1: RSA Cryptography Standard (RSA Laboratories - _ARCHIVO_)](https://web.archive.org/web/20061210143154/http://www.rsasecurity.com/rsalabs/node.asp?id=2125)

>[Wikipedia (ES) PKCS](https://es.wikipedia.org/wiki/PKCS)

>[Wikipedia (EN) PKCS](https://en.wikipedia.org/wiki/PKCS)


```
$ openssl genrsa -out rsakeypair01.pem 
Generating RSA private key, 2048 bit long modulus
...+++
..............+++
e is 65537 (0x10001)
$ cat rsakeypair01.pem 
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAq0ljou3ka2xX1yt4g6dg9sm8irnNajmPA26s9GbbYbJ9Agy/
8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+jTvYQiPJJKrzclxM6PPlNDejiAGub
Hhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizTruNFkg4y8mBEtiqTBbLyqAULzhKI
oJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5YMfcskiI0yden2046X+zbpv0auT8
SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATlszbSGP5IZKh7JKzUv60ccaAgIIYX
8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM5QIDAQABAoIBAAQuhoAzzp/QxVQ7
e3W7YGKmCjRY1OsC/LrYuOA+opx//w1NwmHixKunzUiHD78y0ODG4gX3UT4R1ugi
Zu9wPkxvNXgicibY9Ym6rnFOpNLyHJJmDiNuADvyTGv9aADspjx1j5WoCf8XVL08
KM5YydI7OjeWoSfJsePAplY9SC9R5frkCqgLo3zBY6JXnHCIG/5+ePezBMTPhS7+
I3zR00xukfE4TKZXD+M8c9S01FkFh8Ed/8FyTIv9CvVDWKTNQxnr3b9A0nAlvcK/
cHmJYtBxGKm37fU8WsRjzFXWNtb+8GmhiHWcilHDfXZcYRCgUg/62G2TdXrcCcMx
0ghRUwECgYEA3HOeSGFjOmaKAcYPTYkV1wR9X6XcshCX8danoZ9Zwg0CLVJbOswh
83rmEJil1uJFj8bBD2f10oAEF7kbPp7hsW1pTkRzCsOLUwrzJI5sIeKX+MjHk+Cu
6P8UQbtuQufg/cliG169aR3elN51mB3VDbRdlnN2rCZMidrR7y8h2HMCgYEAxug1
7GSWNUMoynqTdmscFeDGU85s28cuU5k2Dpr+hGf0BnLEr4ZtYdCiL893IgMTtX9h
OcY/b+QQXOYPBcxxhMFfg1nseHkclV6wDo0jlrj0bp96mM8f+G2BaxMylN1NTwyW
2iUJsc3HiKilm5SK0P1IQHUkNst24mZTheNO50cCgYBSQEEaOFvRelibeM5U24Tu
iJpOiY/iUBahALnir5XJtRjO2B95vJgeRAh6wLl/h0T08+8sVFl/hIwCXeowXw9O
J8hWj2ts5LCi9z7osMrfia+x/xeXsQkRnbptHAVzqRhlGDImjB6XjbFyUd4GN3s6
dcVFUSdB67g65w3U8/zoyQKBgHX745B3EcpCLf38u1+wpRYtIDcx6Mxs14PrC2+a
bIJpjwwMI7LiEBvHP40QiN/550Tva+JzP8nFBBe2tw5/eI6AjYFCY8wKOvQ3GENp
YMTUrNi5bGUb5yDbA7tZxdUbd/H9y7VV5uw63bKoJqOkdrsEokjpszN1eO1OORjf
/judAoGBALtlrg0ME3VD7cTY84syl1cc7Xdpk+l4QHeSgChX0s8uzbsYfD82Knk6
PKVbAbG4kY1vkJEuKPsSph/mEjFZ4+TuJoyzHATGNpfzDpcTl3YpB+h+Ho3nPnLL
PyFsxxarmKbJ46VX43pytw3Mx/mBPqU8Zbrx7SMr4bBcQ8dZpD29
-----END RSA PRIVATE KEY-----

```

#### _INTERLUDIO_: `ASN.1` Abstract Syntax Notation One
* [What is ASN.1 (Abstract Syntax Notation One)?](http://whatis.techtarget.com/definition/ASN1-Abstract-Syntax-Notation-One)
* [Wikipedia (ES) ASN.1](https://es.wikipedia.org/wiki/ASN.1)
* [Wikipedia (EN) ASN.1](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One)
* [International Telecommunication Union (ITU) - Introduction to ASN.1​](http://www.itu.int/en/ITU-T/asn1/Pages/introduction.aspx)
* [Reglas de codificación básicas `BER` (Basic Encoding Rules) (Wikipedia ES)](https://es.wikipedia.org/wiki/Reglas_de_codificaci%C3%B3n_b%C3%A1sicas)
* [The standard ASN.1 encoding rules (Wikipedia EN) `BER`, `CER`, `DER`, `XER`, ...](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One#Encodings)
* [`PEM` - Privacy-enhanced Electronic Mail (Wikipedia EN)](https://en.wikipedia.org/wiki/Privacy-enhanced_Electronic_Mail)
* [ITU - X.690 : Information technology - ASN.1 encoding rules: Specification of Basic Encoding Rules (BER), Canonical Encoding Rules (CER) and Distinguished Encoding Rules (DER)](https://www.itu.int/rec/T-REC-X.690/en)


#### Visualizacion del contenido del archivo de clave generado con `openssl rsa`:

* Ayuda de `openssl rsa`

```
$ openssl rsa --help
unknown option --help
rsa [options] <infile >outfile
where options are
 -inform arg     input format - one of DER NET PEM
 -outform arg    output format - one of DER NET PEM
 -in arg         input file
 -sgckey         Use IIS SGC key format
 -passin arg     input file pass phrase source
 -out arg        output file
 -passout arg    output file pass phrase source
 -des            encrypt PEM output with cbc des
 -des3           encrypt PEM output with ede cbc des using 168 bit key
 -seed           encrypt PEM output with cbc seed
 -aes128, -aes192, -aes256
                 encrypt PEM output with cbc aes
 -camellia128, -camellia192, -camellia256
                 encrypt PEM output with cbc camellia
 -text           print the key in text
 -noout          don't print key out
 -modulus        print the RSA key modulus
 -check          verify key consistency
 -pubin          expect a public key in input file
 -pubout         output a public key
 -engine e       use engine e, possibly a hardware device.
 ```

>Nota: En realidad la opción `--help` no es una opción válida `( unknown option --help )`pero sirve de _truco_ para obtener la ayuda de los diferentes _comandos_ `( commands )` de OpenSSL 

* Utilización de `openssl rsa` para visualizar el archivo:

```
$ openssl rsa -in rsakeypair01.pem -inform PEM -text
Private-Key: (2048 bit)
modulus:
    00:ab:49:63:a2:ed:e4:6b:6c:57:d7:2b:78:83:a7:
    60:f6:c9:bc:8a:b9:cd:6a:39:8f:03:6e:ac:f4:66:
    db:61:b2:7d:02:0c:bf:f1:76:cf:1b:98:78:5f:6a:
    48:ae:7d:73:c6:5c:9b:a7:e4:fe:4d:4b:09:46:9c:
    02:da:bf:a3:4e:f6:10:88:f2:49:2a:bc:dc:97:13:
    3a:3c:f9:4d:0d:e8:e2:00:6b:9b:1e:1a:2f:ab:ec:
    db:e2:da:3d:2c:43:b4:a8:49:9b:65:0f:4d:f0:da:
    36:7d:95:72:52:2c:d3:ae:e3:45:92:0e:32:f2:60:
    44:b6:2a:93:05:b2:f2:a8:05:0b:ce:12:88:a0:94:
    e1:a5:5b:d1:f8:31:ee:37:7b:9c:08:38:18:48:20:
    07:6a:61:78:66:28:53:42:df:f9:60:c7:dc:b2:48:
    88:d3:27:5e:9f:6d:38:e9:7f:b3:6e:9b:f4:6a:e4:
    fc:4a:b2:60:d6:41:16:45:4b:36:5c:e8:f4:c5:a8:
    c4:09:aa:05:f3:b9:7e:40:3a:c1:58:04:e5:b3:36:
    d2:18:fe:48:64:a8:7b:24:ac:d4:bf:ad:1c:71:a0:
    20:20:86:17:f0:19:03:9b:f0:3b:43:8f:97:0e:c1:
    94:80:83:58:41:8c:03:f4:90:f6:5f:28:da:88:bc:
    cc:e5
publicExponent: 65537 (0x10001)
privateExponent:
    04:2e:86:80:33:ce:9f:d0:c5:54:3b:7b:75:bb:60:
    62:a6:0a:34:58:d4:eb:02:fc:ba:d8:b8:e0:3e:a2:
    9c:7f:ff:0d:4d:c2:61:e2:c4:ab:a7:cd:48:87:0f:
    bf:32:d0:e0:c6:e2:05:f7:51:3e:11:d6:e8:22:66:
    ef:70:3e:4c:6f:35:78:22:72:26:d8:f5:89:ba:ae:
    71:4e:a4:d2:f2:1c:92:66:0e:23:6e:00:3b:f2:4c:
    6b:fd:68:00:ec:a6:3c:75:8f:95:a8:09:ff:17:54:
    bd:3c:28:ce:58:c9:d2:3b:3a:37:96:a1:27:c9:b1:
    e3:c0:a6:56:3d:48:2f:51:e5:fa:e4:0a:a8:0b:a3:
    7c:c1:63:a2:57:9c:70:88:1b:fe:7e:78:f7:b3:04:
    c4:cf:85:2e:fe:23:7c:d1:d3:4c:6e:91:f1:38:4c:
    a6:57:0f:e3:3c:73:d4:b4:d4:59:05:87:c1:1d:ff:
    c1:72:4c:8b:fd:0a:f5:43:58:a4:cd:43:19:eb:dd:
    bf:40:d2:70:25:bd:c2:bf:70:79:89:62:d0:71:18:
    a9:b7:ed:f5:3c:5a:c4:63:cc:55:d6:36:d6:fe:f0:
    69:a1:88:75:9c:8a:51:c3:7d:76:5c:61:10:a0:52:
    0f:fa:d8:6d:93:75:7a:dc:09:c3:31:d2:08:51:53:
    01
prime1:
    00:dc:73:9e:48:61:63:3a:66:8a:01:c6:0f:4d:89:
    15:d7:04:7d:5f:a5:dc:b2:10:97:f1:d6:a7:a1:9f:
    59:c2:0d:02:2d:52:5b:3a:cc:21:f3:7a:e6:10:98:
    a5:d6:e2:45:8f:c6:c1:0f:67:f5:d2:80:04:17:b9:
    1b:3e:9e:e1:b1:6d:69:4e:44:73:0a:c3:8b:53:0a:
    f3:24:8e:6c:21:e2:97:f8:c8:c7:93:e0:ae:e8:ff:
    14:41:bb:6e:42:e7:e0:fd:c9:62:1b:5e:bd:69:1d:
    de:94:de:75:98:1d:d5:0d:b4:5d:96:73:76:ac:26:
    4c:89:da:d1:ef:2f:21:d8:73
prime2:
    00:c6:e8:35:ec:64:96:35:43:28:ca:7a:93:76:6b:
    1c:15:e0:c6:53:ce:6c:db:c7:2e:53:99:36:0e:9a:
    fe:84:67:f4:06:72:c4:af:86:6d:61:d0:a2:2f:cf:
    77:22:03:13:b5:7f:61:39:c6:3f:6f:e4:10:5c:e6:
    0f:05:cc:71:84:c1:5f:83:59:ec:78:79:1c:95:5e:
    b0:0e:8d:23:96:b8:f4:6e:9f:7a:98:cf:1f:f8:6d:
    81:6b:13:32:94:dd:4d:4f:0c:96:da:25:09:b1:cd:
    c7:88:a8:a5:9b:94:8a:d0:fd:48:40:75:24:36:cb:
    76:e2:66:53:85:e3:4e:e7:47
exponent1:
    52:40:41:1a:38:5b:d1:7a:58:9b:78:ce:54:db:84:
    ee:88:9a:4e:89:8f:e2:50:16:a1:00:b9:e2:af:95:
    c9:b5:18:ce:d8:1f:79:bc:98:1e:44:08:7a:c0:b9:
    7f:87:44:f4:f3:ef:2c:54:59:7f:84:8c:02:5d:ea:
    30:5f:0f:4e:27:c8:56:8f:6b:6c:e4:b0:a2:f7:3e:
    e8:b0:ca:df:89:af:b1:ff:17:97:b1:09:11:9d:ba:
    6d:1c:05:73:a9:18:65:18:32:26:8c:1e:97:8d:b1:
    72:51:de:06:37:7b:3a:75:c5:45:51:27:41:eb:b8:
    3a:e7:0d:d4:f3:fc:e8:c9
exponent2:
    75:fb:e3:90:77:11:ca:42:2d:fd:fc:bb:5f:b0:a5:
    16:2d:20:37:31:e8:cc:6c:d7:83:eb:0b:6f:9a:6c:
    82:69:8f:0c:0c:23:b2:e2:10:1b:c7:3f:8d:10:88:
    df:f9:e7:44:ef:6b:e2:73:3f:c9:c5:04:17:b6:b7:
    0e:7f:78:8e:80:8d:81:42:63:cc:0a:3a:f4:37:18:
    43:69:60:c4:d4:ac:d8:b9:6c:65:1b:e7:20:db:03:
    bb:59:c5:d5:1b:77:f1:fd:cb:b5:55:e6:ec:3a:dd:
    b2:a8:26:a3:a4:76:bb:04:a2:48:e9:b3:33:75:78:
    ed:4e:39:18:df:fe:3b:9d
coefficient:
    00:bb:65:ae:0d:0c:13:75:43:ed:c4:d8:f3:8b:32:
    97:57:1c:ed:77:69:93:e9:78:40:77:92:80:28:57:
    d2:cf:2e:cd:bb:18:7c:3f:36:2a:79:3a:3c:a5:5b:
    01:b1:b8:91:8d:6f:90:91:2e:28:fb:12:a6:1f:e6:
    12:31:59:e3:e4:ee:26:8c:b3:1c:04:c6:36:97:f3:
    0e:97:13:97:76:29:07:e8:7e:1e:8d:e7:3e:72:cb:
    3f:21:6c:c7:16:ab:98:a6:c9:e3:a5:57:e3:7a:72:
    b7:0d:cc:c7:f9:81:3e:a5:3c:65:ba:f1:ed:23:2b:
    e1:b0:5c:43:c7:59:a4:3d:bd
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAq0ljou3ka2xX1yt4g6dg9sm8irnNajmPA26s9GbbYbJ9Agy/
8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+jTvYQiPJJKrzclxM6PPlNDejiAGub
Hhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizTruNFkg4y8mBEtiqTBbLyqAULzhKI
oJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5YMfcskiI0yden2046X+zbpv0auT8
SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATlszbSGP5IZKh7JKzUv60ccaAgIIYX
8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM5QIDAQABAoIBAAQuhoAzzp/QxVQ7
e3W7YGKmCjRY1OsC/LrYuOA+opx//w1NwmHixKunzUiHD78y0ODG4gX3UT4R1ugi
Zu9wPkxvNXgicibY9Ym6rnFOpNLyHJJmDiNuADvyTGv9aADspjx1j5WoCf8XVL08
KM5YydI7OjeWoSfJsePAplY9SC9R5frkCqgLo3zBY6JXnHCIG/5+ePezBMTPhS7+
I3zR00xukfE4TKZXD+M8c9S01FkFh8Ed/8FyTIv9CvVDWKTNQxnr3b9A0nAlvcK/
cHmJYtBxGKm37fU8WsRjzFXWNtb+8GmhiHWcilHDfXZcYRCgUg/62G2TdXrcCcMx
0ghRUwECgYEA3HOeSGFjOmaKAcYPTYkV1wR9X6XcshCX8danoZ9Zwg0CLVJbOswh
83rmEJil1uJFj8bBD2f10oAEF7kbPp7hsW1pTkRzCsOLUwrzJI5sIeKX+MjHk+Cu
6P8UQbtuQufg/cliG169aR3elN51mB3VDbRdlnN2rCZMidrR7y8h2HMCgYEAxug1
7GSWNUMoynqTdmscFeDGU85s28cuU5k2Dpr+hGf0BnLEr4ZtYdCiL893IgMTtX9h
OcY/b+QQXOYPBcxxhMFfg1nseHkclV6wDo0jlrj0bp96mM8f+G2BaxMylN1NTwyW
2iUJsc3HiKilm5SK0P1IQHUkNst24mZTheNO50cCgYBSQEEaOFvRelibeM5U24Tu
iJpOiY/iUBahALnir5XJtRjO2B95vJgeRAh6wLl/h0T08+8sVFl/hIwCXeowXw9O
J8hWj2ts5LCi9z7osMrfia+x/xeXsQkRnbptHAVzqRhlGDImjB6XjbFyUd4GN3s6
dcVFUSdB67g65w3U8/zoyQKBgHX745B3EcpCLf38u1+wpRYtIDcx6Mxs14PrC2+a
bIJpjwwMI7LiEBvHP40QiN/550Tva+JzP8nFBBe2tw5/eI6AjYFCY8wKOvQ3GENp
YMTUrNi5bGUb5yDbA7tZxdUbd/H9y7VV5uw63bKoJqOkdrsEokjpszN1eO1OORjf
/judAoGBALtlrg0ME3VD7cTY84syl1cc7Xdpk+l4QHeSgChX0s8uzbsYfD82Knk6
PKVbAbG4kY1vkJEuKPsSph/mEjFZ4+TuJoyzHATGNpfzDpcTl3YpB+h+Ho3nPnLL
PyFsxxarmKbJ46VX43pytw3Mx/mBPqU8Zbrx7SMr4bBcQ8dZpD29
-----END RSA PRIVATE KEY-----
```

#### Visualizacion del contenido del archivo de clave generado con `openssl asn1parse`:

* Ayuda de `openssl asn1parse`

```
$ openssl asn1parse --help
unknown option --help
asn1parse [options] <infile
where options are
 -inform arg   input format - one of DER PEM
 -in arg       input file
 -out arg      output file (output format is always DER
 -noout arg    don't produce any output
 -offset arg   offset into file
 -length arg   length of section in file
 -i            indent entries
 -dump         dump unknown data in hex form
 -dlimit arg   dump the first arg bytes of unknown data in hex form
 -oid file     file of extra oid definitions
 -strparse offset
               a series of these can be used to 'dig' into multiple
               ASN1 blob wrappings
 -genstr str   string to generate ASN1 structure from
 -genconf file file to generate ASN1 structure from
 ```

* Utilización de `openssl asn1parse` para visualizar el archivo:

```
$ openssl asn1parse -in rsakeypair01.pem -inform PEM -i -dump 
    0:d=0  hl=4 l=1187 cons: SEQUENCE          
    4:d=1  hl=2 l=   1 prim:  INTEGER           :00
    7:d=1  hl=4 l= 257 prim:  INTEGER           :AB4963A2EDE46B6C57D72B7883A760F6C9BC8AB9CD6A398F036EACF466DB61B27D020CBFF176CF1B98785F6A48AE7D73C65C9BA7E4FE4D4B09469C02DABFA34EF61088F2492ABCDC97133A3CF94D0DE8E2006B9B1E1A2FABECDBE2DA3D2C43B4A8499B650F4DF0DA367D9572522CD3AEE345920E32F26044B62A9305B2F2A8050BCE1288A094E1A55BD1F831EE377B9C0838184820076A617866285342DFF960C7DCB24888D3275E9F6D38E97FB36E9BF46AE4FC4AB260D64116454B365CE8F4C5A8C409AA05F3B97E403AC15804E5B336D218FE4864A87B24ACD4BFAD1C71A020208617F019039BF03B438F970EC194808358418C03F490F65F28DA88BCCCE5
  268:d=1  hl=2 l=   3 prim:  INTEGER           :010001
  273:d=1  hl=4 l= 256 prim:  INTEGER           :042E868033CE9FD0C5543B7B75BB6062A60A3458D4EB02FCBAD8B8E03EA29C7FFF0D4DC261E2C4ABA7CD48870FBF32D0E0C6E205F7513E11D6E82266EF703E4C6F3578227226D8F589BAAE714EA4D2F21C92660E236E003BF24C6BFD6800ECA63C758F95A809FF1754BD3C28CE58C9D23B3A3796A127C9B1E3C0A6563D482F51E5FAE40AA80BA37CC163A2579C70881BFE7E78F7B304C4CF852EFE237CD1D34C6E91F1384CA6570FE33C73D4B4D4590587C11DFFC1724C8BFD0AF54358A4CD4319EBDDBF40D27025BDC2BF70798962D07118A9B7EDF53C5AC463CC55D636D6FEF069A188759C8A51C37D765C6110A0520FFAD86D93757ADC09C331D208515301
  533:d=1  hl=3 l= 129 prim:  INTEGER           :DC739E4861633A668A01C60F4D8915D7047D5FA5DCB21097F1D6A7A19F59C20D022D525B3ACC21F37AE61098A5D6E2458FC6C10F67F5D2800417B91B3E9EE1B16D694E44730AC38B530AF3248E6C21E297F8C8C793E0AEE8FF1441BB6E42E7E0FDC9621B5EBD691DDE94DE75981DD50DB45D967376AC264C89DAD1EF2F21D873
  665:d=1  hl=3 l= 129 prim:  INTEGER           :C6E835EC6496354328CA7A93766B1C15E0C653CE6CDBC72E5399360E9AFE8467F40672C4AF866D61D0A22FCF77220313B57F6139C63F6FE4105CE60F05CC7184C15F8359EC78791C955EB00E8D2396B8F46E9F7A98CF1FF86D816B133294DD4D4F0C96DA2509B1CDC788A8A59B948AD0FD4840752436CB76E2665385E34EE747
  797:d=1  hl=3 l= 128 prim:  INTEGER           :5240411A385BD17A589B78CE54DB84EE889A4E898FE25016A100B9E2AF95C9B518CED81F79BC981E44087AC0B97F8744F4F3EF2C54597F848C025DEA305F0F4E27C8568F6B6CE4B0A2F73EE8B0CADF89AFB1FF1797B109119DBA6D1C0573A918651832268C1E978DB17251DE06377B3A75C545512741EBB83AE70DD4F3FCE8C9
  928:d=1  hl=3 l= 128 prim:  INTEGER           :75FBE3907711CA422DFDFCBB5FB0A5162D203731E8CC6CD783EB0B6F9A6C82698F0C0C23B2E2101BC73F8D1088DFF9E744EF6BE2733FC9C50417B6B70E7F788E808D814263CC0A3AF43718436960C4D4ACD8B96C651BE720DB03BB59C5D51B77F1FDCBB555E6EC3ADDB2A826A3A476BB04A248E9B3337578ED4E3918DFFE3B9D
 1059:d=1  hl=3 l= 129 prim:  INTEGER           :BB65AE0D0C137543EDC4D8F38B3297571CED776993E978407792802857D2CF2ECDBB187C3F362A793A3CA55B01B1B8918D6F90912E28FB12A61FE6123159E3E4EE268CB31C04C63697F30E971397762907E87E1E8DE73E72CB3F216CC716AB98A6C9E3A557E37A72B70DCCC7F9813EA53C65BAF1ED232BE1B05C43C759A43DB
 ```
 
* Ayuda de `dumpasn1` [DumpASN1 - ASN.1 object dump/syntax check program](http://manpages.ubuntu.com/manpages/xenial/man1/dumpasn1.1.html) del autor [Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/)

```
$ dumpasn1 --help
DumpASN1 - ASN.1 object dump/syntax check program.
Copyright Peter Gutmann 1997 - 2012.  Last updated 8 August 2015.

Usage: dumpasn1 [-acdefghilmoprstuvwxz] <file>
  Input options:
       - = Take input from stdin (some options may not work properly)
       -<number> = Start <number> bytes into the file
       -- = End of arg list
       -c<file> = Read Object Identifier info from alternate config file
            (values will override equivalents in global config file)

  Output options:
       -f<file> = Dump object at offset -<number> to file (allows data to be
            extracted from encapsulating objects)
       -w<number> = Set width of output, default = 80 columns

  Display options:
       -a = Print all data in long data blocks, not just the first 128 bytes
       -d = Print dots to show column alignment
       -g = Display ASN.1 structure outline only (no primitive objects)
       -h = Hex dump object header (tag+length) before the decoded output
       -hh = Same as -h but display more of the object as hex data
       -i = Use shallow indenting, for deeply-nested objects
       -l = Long format, display extra info about Object Identifiers
       -m<number>  = Maximum nesting level for which to display content
       -p = Pure ASN.1 output without encoding information
       -t = Display text values next to hex dump of data
       -v = Verbose mode, equivalent to -ahlt

  Format options:
       -e = Don't print encapsulated data inside OCTET/BIT STRINGs
       -r = Print bits in BIT STRING as encoded in reverse order
       -u = Don't format UTCTime/GeneralizedTime string data
       -x = Display size and offset in hex not decimal

  Checking options:
       -o = Don't check validity of character strings hidden in octet strings
       -s = Syntax check only, don't dump ASN.1 structures
       -z = Allow zero-length items

Warnings generated by deprecated OIDs require the use of '-l' to be displayed.
Program return code is the number of errors found or EXIT_SUCCESS.

```
* Utilización de `dumpasn1` para visualizar el archivo:

```
$ dumpasn1 -adhl rsakeypair01.pem 
    <2D 2D>
  0  45: Unknown (Reserved) {
    <2D 2D>
  2  45: . Unknown (Reserved) {
    <2D 42>
  4  66: . . Unknown (Reserved) {
    <45 47>
  6  71: . . . [APPLICATION 5]
       : . . . . 'IN RSA PRIVATE KEY-----.MIIEowIBAAKCAQEAq0ljou3k'
       : . . . . 'a2xX1yt4g6dg9sm8irnNajm'
       : . . . . Error: IA5String contains illegal character(s).
Error: Inconsistent object length, 7 bytes difference.
       : . . . }
Error: Inconsistent object length, 30 bytes difference.
       : . . }
Error: Inconsistent object length, 32 bytes difference.
       : . }
Warning: Further data follows ASN.1 data at position 79.

1 warning, 4 errors.
```
>Nota: la visualización del archivo FALLA pues `dumpasn1` necesita que el archivo a visualizar se encuentre en `DER encoded format`


* Visualización con la utilidad en linea [ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

![ASN.1 JavaScript decoder](images/rsakeypair01_image01.png "ASN.1 JavaScript decoder")

* Visualización con la utilidad de GNOME `File Viewer: gcr-viewer`

```
$ gcr-viewer rsakeypair01.pem 
```

![$ gcr-viewer rsakeypair01.pem](images/rsakeypair01_image02.png "$ gcr-viewer rsakeypair01.pem")

### Ejemplo 01.02: Obtener la clave RSA generada en el ejemplo anterior codificada en `DER  (Distinguished Encoding Rules)` sin cifrar `(PKCS#1)` utilizando `openssl rsa`

´´´
$ openssl rsa -in rsakeypair01.pem -inform PEM -out rsakeypair01.der -outform DER
writing RSA key
´´´

#### Visualizando el archivo obtenido:

>Nota: El archivo obtenido se encuentra en formato binario y no es adecuado visualizarlo como texto (por ejemplo con `cat`)

* Visualizándolo en Hexadecimal (_en línea de comando -- CLI --_) con `xxd` ('make a hexdump or do the reverse.')
```
$ xxd -c 16 rsakeypair01.der 
00000000: 3082 04a3 0201 0002 8201 0100 ab49 63a2  0............Ic.
00000010: ede4 6b6c 57d7 2b78 83a7 60f6 c9bc 8ab9  ..klW.+x..`.....
00000020: cd6a 398f 036e acf4 66db 61b2 7d02 0cbf  .j9..n..f.a.}...
00000030: f176 cf1b 9878 5f6a 48ae 7d73 c65c 9ba7  .v...x_jH.}s.\..
00000040: e4fe 4d4b 0946 9c02 dabf a34e f610 88f2  ..MK.F.....N....
00000050: 492a bcdc 9713 3a3c f94d 0de8 e200 6b9b  I*....:<.M....k.
00000060: 1e1a 2fab ecdb e2da 3d2c 43b4 a849 9b65  ../.....=,C..I.e
00000070: 0f4d f0da 367d 9572 522c d3ae e345 920e  .M..6}.rR,...E..
00000080: 32f2 6044 b62a 9305 b2f2 a805 0bce 1288  2.`D.*..........
00000090: a094 e1a5 5bd1 f831 ee37 7b9c 0838 1848  ....[..1.7{..8.H
000000a0: 2007 6a61 7866 2853 42df f960 c7dc b248   .jaxf(SB..`...H
000000b0: 88d3 275e 9f6d 38e9 7fb3 6e9b f46a e4fc  ..'^.m8...n..j..
000000c0: 4ab2 60d6 4116 454b 365c e8f4 c5a8 c409  J.`.A.EK6\......
000000d0: aa05 f3b9 7e40 3ac1 5804 e5b3 36d2 18fe  ....~@:.X...6...
000000e0: 4864 a87b 24ac d4bf ad1c 71a0 2020 8617  Hd.{$.....q.  ..
000000f0: f019 039b f03b 438f 970e c194 8083 5841  .....;C.......XA
00000100: 8c03 f490 f65f 28da 88bc cce5 0203 0100  ....._(.........
00000110: 0102 8201 0004 2e86 8033 ce9f d0c5 543b  .........3....T;
00000120: 7b75 bb60 62a6 0a34 58d4 eb02 fcba d8b8  {u.`b..4X.......
00000130: e03e a29c 7fff 0d4d c261 e2c4 aba7 cd48  .>.....M.a.....H
00000140: 870f bf32 d0e0 c6e2 05f7 513e 11d6 e822  ...2......Q>..."
00000150: 66ef 703e 4c6f 3578 2272 26d8 f589 baae  f.p>Lo5x"r&.....
00000160: 714e a4d2 f21c 9266 0e23 6e00 3bf2 4c6b  qN.....f.#n.;.Lk
00000170: fd68 00ec a63c 758f 95a8 09ff 1754 bd3c  .h...<u......T.<
00000180: 28ce 58c9 d23b 3a37 96a1 27c9 b1e3 c0a6  (.X..;:7..'.....
00000190: 563d 482f 51e5 fae4 0aa8 0ba3 7cc1 63a2  V=H/Q.......|.c.
000001a0: 579c 7088 1bfe 7e78 f7b3 04c4 cf85 2efe  W.p...~x........
000001b0: 237c d1d3 4c6e 91f1 384c a657 0fe3 3c73  #|..Ln..8L.W..<s
000001c0: d4b4 d459 0587 c11d ffc1 724c 8bfd 0af5  ...Y......rL....
000001d0: 4358 a4cd 4319 ebdd bf40 d270 25bd c2bf  CX..C....@.p%...
000001e0: 7079 8962 d071 18a9 b7ed f53c 5ac4 63cc  py.b.q.....<Z.c.
000001f0: 55d6 36d6 fef0 69a1 8875 9c8a 51c3 7d76  U.6...i..u..Q.}v
00000200: 5c61 10a0 520f fad8 6d93 757a dc09 c331  \a..R...m.uz...1
00000210: d208 5153 0102 8181 00dc 739e 4861 633a  ..QS......s.Hac:
00000220: 668a 01c6 0f4d 8915 d704 7d5f a5dc b210  f....M....}_....
00000230: 97f1 d6a7 a19f 59c2 0d02 2d52 5b3a cc21  ......Y...-R[:.!
00000240: f37a e610 98a5 d6e2 458f c6c1 0f67 f5d2  .z......E....g..
00000250: 8004 17b9 1b3e 9ee1 b16d 694e 4473 0ac3  .....>...miNDs..
00000260: 8b53 0af3 248e 6c21 e297 f8c8 c793 e0ae  .S..$.l!........
00000270: e8ff 1441 bb6e 42e7 e0fd c962 1b5e bd69  ...A.nB....b.^.i
00000280: 1dde 94de 7598 1dd5 0db4 5d96 7376 ac26  ....u.....].sv.&
00000290: 4c89 dad1 ef2f 21d8 7302 8181 00c6 e835  L..../!.s......5
000002a0: ec64 9635 4328 ca7a 9376 6b1c 15e0 c653  .d.5C(.z.vk....S
000002b0: ce6c dbc7 2e53 9936 0e9a fe84 67f4 0672  .l...S.6....g..r
000002c0: c4af 866d 61d0 a22f cf77 2203 13b5 7f61  ...ma../.w"....a
000002d0: 39c6 3f6f e410 5ce6 0f05 cc71 84c1 5f83  9.?o..\....q.._.
000002e0: 59ec 7879 1c95 5eb0 0e8d 2396 b8f4 6e9f  Y.xy..^...#...n.
000002f0: 7a98 cf1f f86d 816b 1332 94dd 4d4f 0c96  z....m.k.2..MO..
00000300: da25 09b1 cdc7 88a8 a59b 948a d0fd 4840  .%............H@
00000310: 7524 36cb 76e2 6653 85e3 4ee7 4702 8180  u$6.v.fS..N.G...
00000320: 5240 411a 385b d17a 589b 78ce 54db 84ee  R@A.8[.zX.x.T...
00000330: 889a 4e89 8fe2 5016 a100 b9e2 af95 c9b5  ..N...P.........
00000340: 18ce d81f 79bc 981e 4408 7ac0 b97f 8744  ....y...D.z....D
00000350: f4f3 ef2c 5459 7f84 8c02 5dea 305f 0f4e  ...,TY....].0_.N
00000360: 27c8 568f 6b6c e4b0 a2f7 3ee8 b0ca df89  '.V.kl....>.....
00000370: afb1 ff17 97b1 0911 9dba 6d1c 0573 a918  ..........m..s..
00000380: 6518 3226 8c1e 978d b172 51de 0637 7b3a  e.2&.....rQ..7{:
00000390: 75c5 4551 2741 ebb8 3ae7 0dd4 f3fc e8c9  u.EQ'A..:.......
000003a0: 0281 8075 fbe3 9077 11ca 422d fdfc bb5f  ...u...w..B-..._
000003b0: b0a5 162d 2037 31e8 cc6c d783 eb0b 6f9a  ...- 71..l....o.
000003c0: 6c82 698f 0c0c 23b2 e210 1bc7 3f8d 1088  l.i...#.....?...
000003d0: dff9 e744 ef6b e273 3fc9 c504 17b6 b70e  ...D.k.s?.......
000003e0: 7f78 8e80 8d81 4263 cc0a 3af4 3718 4369  .x....Bc..:.7.Ci
000003f0: 60c4 d4ac d8b9 6c65 1be7 20db 03bb 59c5  `.....le.. ...Y.
00000400: d51b 77f1 fdcb b555 e6ec 3add b2a8 26a3  ..w....U..:...&.
00000410: a476 bb04 a248 e9b3 3375 78ed 4e39 18df  .v...H..3ux.N9..
00000420: fe3b 9d02 8181 00bb 65ae 0d0c 1375 43ed  .;......e....uC.
00000430: c4d8 f38b 3297 571c ed77 6993 e978 4077  ....2.W..wi..x@w
00000440: 9280 2857 d2cf 2ecd bb18 7c3f 362a 793a  ..(W......|?6*y:
00000450: 3ca5 5b01 b1b8 918d 6f90 912e 28fb 12a6  <.[.....o...(...
00000460: 1fe6 1231 59e3 e4ee 268c b31c 04c6 3697  ...1Y...&.....6.
00000470: f30e 9713 9776 2907 e87e 1e8d e73e 72cb  .....v)..~...>r.
00000480: 3f21 6cc7 16ab 98a6 c9e3 a557 e37a 72b7  ?!l........W.zr.
00000490: 0dcc c7f9 813e a53c 65ba f1ed 232b e1b0  .....>.<e...#+..
000004a0: 5c43 c759 a43d bd                        \C.Y.=.
```

>Nota: adviértase el _significativo_ comienzo por `0x8032` _delator_ de la presencia de un posible archivo codificado en `ASN.1` en `DER format` comenzando por una `SEQUENCE`

* Visualizándolo en Hexadecimal (_en entorno gráfico_) con `ghex`

```
$ ghex rsakeypair01.der
```

![ASN.1 JavaScript decoder](images/rsakeypair01_image03_GHex.png "ASN.1 JavaScript decoder")

* Visualización con las mismas herramientas utilizadas en el ejemplo anterior producirán el mismo resultado:
    * `$ openssl rsa -in rsakeypair01.der -inform DER -text`
    * `$ openssl asn1parse -in rsakeypair01.der -inform DER -i -dump`

>Nota: No olvidar informar correctamente que el archivo se encuentra en encoding format `DER ( -inform DER )`

* La visualización con `dumpasn1` esta vez sí obtendrá el resultado esperado al hallarse el archivo codificado en `DER` encoding format:

```
$ dumpasn1 -adhl rsakeypair01.der
    <30 82 04 A3>
   0 1187: SEQUENCE {
    <02 01>
   4    1: . INTEGER 0
    <02 82 01 01>
   7  257: . INTEGER
         : . . 00 AB 49 63 A2 ED E4 6B 6C 57 D7 2B 78 83 A7 60
         : . . F6 C9 BC 8A B9 CD 6A 39 8F 03 6E AC F4 66 DB 61
         : . . B2 7D 02 0C BF F1 76 CF 1B 98 78 5F 6A 48 AE 7D
         : . . 73 C6 5C 9B A7 E4 FE 4D 4B 09 46 9C 02 DA BF A3
         : . . 4E F6 10 88 F2 49 2A BC DC 97 13 3A 3C F9 4D 0D
         : . . E8 E2 00 6B 9B 1E 1A 2F AB EC DB E2 DA 3D 2C 43
         : . . B4 A8 49 9B 65 0F 4D F0 DA 36 7D 95 72 52 2C D3
         : . . AE E3 45 92 0E 32 F2 60 44 B6 2A 93 05 B2 F2 A8
         : . . 05 0B CE 12 88 A0 94 E1 A5 5B D1 F8 31 EE 37 7B
         : . . 9C 08 38 18 48 20 07 6A 61 78 66 28 53 42 DF F9
         : . . 60 C7 DC B2 48 88 D3 27 5E 9F 6D 38 E9 7F B3 6E
         : . . 9B F4 6A E4 FC 4A B2 60 D6 41 16 45 4B 36 5C E8
         : . . F4 C5 A8 C4 09 AA 05 F3 B9 7E 40 3A C1 58 04 E5
         : . . B3 36 D2 18 FE 48 64 A8 7B 24 AC D4 BF AD 1C 71
         : . . A0 20 20 86 17 F0 19 03 9B F0 3B 43 8F 97 0E C1
         : . . 94 80 83 58 41 8C 03 F4 90 F6 5F 28 DA 88 BC CC
         : . . E5
    <02 03>
 268    3: . INTEGER 65537
    <02 82 01 00>
 273  256: . INTEGER
         : . . 04 2E 86 80 33 CE 9F D0 C5 54 3B 7B 75 BB 60 62
         : . . A6 0A 34 58 D4 EB 02 FC BA D8 B8 E0 3E A2 9C 7F
         : . . FF 0D 4D C2 61 E2 C4 AB A7 CD 48 87 0F BF 32 D0
         : . . E0 C6 E2 05 F7 51 3E 11 D6 E8 22 66 EF 70 3E 4C
         : . . 6F 35 78 22 72 26 D8 F5 89 BA AE 71 4E A4 D2 F2
         : . . 1C 92 66 0E 23 6E 00 3B F2 4C 6B FD 68 00 EC A6
         : . . 3C 75 8F 95 A8 09 FF 17 54 BD 3C 28 CE 58 C9 D2
         : . . 3B 3A 37 96 A1 27 C9 B1 E3 C0 A6 56 3D 48 2F 51
         : . . E5 FA E4 0A A8 0B A3 7C C1 63 A2 57 9C 70 88 1B
         : . . FE 7E 78 F7 B3 04 C4 CF 85 2E FE 23 7C D1 D3 4C
         : . . 6E 91 F1 38 4C A6 57 0F E3 3C 73 D4 B4 D4 59 05
         : . . 87 C1 1D FF C1 72 4C 8B FD 0A F5 43 58 A4 CD 43
         : . . 19 EB DD BF 40 D2 70 25 BD C2 BF 70 79 89 62 D0
         : . . 71 18 A9 B7 ED F5 3C 5A C4 63 CC 55 D6 36 D6 FE
         : . . F0 69 A1 88 75 9C 8A 51 C3 7D 76 5C 61 10 A0 52
         : . . 0F FA D8 6D 93 75 7A DC 09 C3 31 D2 08 51 53 01
    <02 81 81>
 533  129: . INTEGER
         : . . 00 DC 73 9E 48 61 63 3A 66 8A 01 C6 0F 4D 89 15
         : . . D7 04 7D 5F A5 DC B2 10 97 F1 D6 A7 A1 9F 59 C2
         : . . 0D 02 2D 52 5B 3A CC 21 F3 7A E6 10 98 A5 D6 E2
         : . . 45 8F C6 C1 0F 67 F5 D2 80 04 17 B9 1B 3E 9E E1
         : . . B1 6D 69 4E 44 73 0A C3 8B 53 0A F3 24 8E 6C 21
         : . . E2 97 F8 C8 C7 93 E0 AE E8 FF 14 41 BB 6E 42 E7
         : . . E0 FD C9 62 1B 5E BD 69 1D DE 94 DE 75 98 1D D5
         : . . 0D B4 5D 96 73 76 AC 26 4C 89 DA D1 EF 2F 21 D8
         : . . 73
    <02 81 81>
 665  129: . INTEGER
         : . . 00 C6 E8 35 EC 64 96 35 43 28 CA 7A 93 76 6B 1C
         : . . 15 E0 C6 53 CE 6C DB C7 2E 53 99 36 0E 9A FE 84
         : . . 67 F4 06 72 C4 AF 86 6D 61 D0 A2 2F CF 77 22 03
         : . . 13 B5 7F 61 39 C6 3F 6F E4 10 5C E6 0F 05 CC 71
         : . . 84 C1 5F 83 59 EC 78 79 1C 95 5E B0 0E 8D 23 96
         : . . B8 F4 6E 9F 7A 98 CF 1F F8 6D 81 6B 13 32 94 DD
         : . . 4D 4F 0C 96 DA 25 09 B1 CD C7 88 A8 A5 9B 94 8A
         : . . D0 FD 48 40 75 24 36 CB 76 E2 66 53 85 E3 4E E7
         : . . 47
    <02 81 80>
 797  128: . INTEGER
         : . . 52 40 41 1A 38 5B D1 7A 58 9B 78 CE 54 DB 84 EE
         : . . 88 9A 4E 89 8F E2 50 16 A1 00 B9 E2 AF 95 C9 B5
         : . . 18 CE D8 1F 79 BC 98 1E 44 08 7A C0 B9 7F 87 44
         : . . F4 F3 EF 2C 54 59 7F 84 8C 02 5D EA 30 5F 0F 4E
         : . . 27 C8 56 8F 6B 6C E4 B0 A2 F7 3E E8 B0 CA DF 89
         : . . AF B1 FF 17 97 B1 09 11 9D BA 6D 1C 05 73 A9 18
         : . . 65 18 32 26 8C 1E 97 8D B1 72 51 DE 06 37 7B 3A
         : . . 75 C5 45 51 27 41 EB B8 3A E7 0D D4 F3 FC E8 C9
    <02 81 80>
 928  128: . INTEGER
         : . . 75 FB E3 90 77 11 CA 42 2D FD FC BB 5F B0 A5 16
         : . . 2D 20 37 31 E8 CC 6C D7 83 EB 0B 6F 9A 6C 82 69
         : . . 8F 0C 0C 23 B2 E2 10 1B C7 3F 8D 10 88 DF F9 E7
         : . . 44 EF 6B E2 73 3F C9 C5 04 17 B6 B7 0E 7F 78 8E
         : . . 80 8D 81 42 63 CC 0A 3A F4 37 18 43 69 60 C4 D4
         : . . AC D8 B9 6C 65 1B E7 20 DB 03 BB 59 C5 D5 1B 77
         : . . F1 FD CB B5 55 E6 EC 3A DD B2 A8 26 A3 A4 76 BB
         : . . 04 A2 48 E9 B3 33 75 78 ED 4E 39 18 DF FE 3B 9D
    <02 81 81>
1059  129: . INTEGER
         : . . 00 BB 65 AE 0D 0C 13 75 43 ED C4 D8 F3 8B 32 97
         : . . 57 1C ED 77 69 93 E9 78 40 77 92 80 28 57 D2 CF
         : . . 2E CD BB 18 7C 3F 36 2A 79 3A 3C A5 5B 01 B1 B8
         : . . 91 8D 6F 90 91 2E 28 FB 12 A6 1F E6 12 31 59 E3
         : . . E4 EE 26 8C B3 1C 04 C6 36 97 F3 0E 97 13 97 76
         : . . 29 07 E8 7E 1E 8D E7 3E 72 CB 3F 21 6C C7 16 AB
         : . . 98 A6 C9 E3 A5 57 E3 7A 72 B7 0D CC C7 F9 81 3E
         : . . A5 3C 65 BA F1 ED 23 2B E1 B0 5C 43 C7 59 A4 3D
         : . . BD
         : . }

0 warnings, 0 errors.

```