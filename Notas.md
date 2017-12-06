### notas TIK sobre certificados (Dic 2017)

Incluir imagen:


![alt text](images/image01.png "Title")

## TOOLS

>[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

>[CSR Decoder And Certificate Decoder(Red Krestel)(old)](https://certlogik.com/decoder/)

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)


## Generacion de Claves RSA con OpenSSL

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv labs/lab01_rsa
mkdir: se ha creado el directorio 'labs'
mkdir: se ha creado el directorio 'labs/lab01_rsa'
$ cd labs/lab01_rsa/
$ pwd
/home/devel1/labs/lab01_rsa

```

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

### Ejemplo 01: Generar clave RSA en formato PEM sin cifrar (PKCS#1)

>[Public-Key Cryptography Standards (PKCS)](https://web.archive.org/web/20061209135809/http://www.rsasecurity.com/rsalabs/node.asp?id=2124)

>[02 - PKCS #1: RSA Cryptography Standard](https://web.archive.org/web/20061210143154/http://www.rsasecurity.com/rsalabs/node.asp?id=2125)

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
