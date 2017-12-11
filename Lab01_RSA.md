# Lab 01 RSA Key Pairs 
##### Apuntes para TIK sobre certificados (Dic 2017)

### TOOLS

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

>[Openssl](https://www.openssl.org/)

>[DumpASN1 - ASN.1 object dump/syntax check program](http://manpages.ubuntu.com/manpages/xenial/man1/dumpasn1.1.html) del autor [Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/)

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

### Preparación del laboratorio

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv $HOME/labs/lab01_rsa
mkdir: se ha creado el directorio '/home/devel1/labs'
mkdir: se ha creado el directorio '/home/devel1/labs/lab01_rsa'
$ cd $HOME/labs/lab01_rsa/
$ pwd
/home/devel1/labs/lab01_rsa
```
## INTRODUCCION (breve) a `RSA Public Key scheme`

>[RSA Wikipedia (es)](https://es.wikipedia.org/wiki/RSA)

>[RSA (cryptosystem) Wikipedia (en)](https://en.wikipedia.org/wiki/RSA_(cryptosystem))

>[RSA Calculator](https://www.cs.drexel.edu/~jpopyack/IntroCS/HW/RSAWorksheet.html) 

Ejemplos de generación de un `Keypair RSA` y cifrado/descifrado de un mensaje con dicho Keypair:

* Step 1

![RSA Calculator - Step 1](images/RSA_Calulator_step1_image01.png "RSA Calculator - Step 1")


* Step 2

![RSA Calculator - Step 2](images/RSA_Calulator_step2_image01.png "RSA Calculator - Step 2")


* Step 3

![RSA Calculator - Step 3](images/RSA_Calulator_step3_image01.png "RSA Calculator - Step 3")


* Step 4

![RSA Calculator - Step 4](images/RSA_Calulator_step4_image01.png "RSA Calculator - Step 4")


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
$ openssl genrsa -out rsakeypair01.pem 2048
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
* [ASN.1 key structures in DER and PEM](https://tls.mbed.org/kb/cryptography/asn1-key-structures-in-der-and-pem)

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

```
$ openssl rsa -in rsakeypair01.pem -inform PEM -out rsakeypair01.der -outform DER
writing RSA key
```

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
    * `$ gcr-viewer rsakeypair01.der`

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

### Ejemplo 01.03: Generar clave RSA en formato `(PKCS#1)` `PEM` y `DER` con diferentes protecciones de cifrado `(PKCS#1)`

>Nota: ver [PKCS #5: Password-Based Cryptography Standard](https://web.archive.org/web/20061210143350/http://www.rsasecurity.com/rsalabs/node.asp?id=2127)

#### En formato `PEM` con `PBE - Password Based Encription` y `des3 - 'Triple DES`:

```
$ openssl genrsa -out rsakeypair02_des3.pem -des3 -passout pass:changeit
Generating RSA private key, 2048 bit long modulus
..............................+++
.........+++
e is 65537 (0x10001)
```

* Visualización:

```
$ cat rsakeypair02_des3.pem 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,4B71D975CC60A4FC

TF60GPXKPaHd3YvBX8HY+oKFs0580y0BDFKn4EkZ7NfROYVT+urP7VcTpnQfWODu
Knf0whwbWZHh2blqiS5sGgl9SEpBMMdAHAJi/vjnkBatFoJuzhlJkQ+8zDNbooZ4
aHuGN9+4JTIFmwM2PeUpbIgk4epuot3CXCkkdDN13i28MY48aL94e0qL9Hw9HxpE
ZAjHgelFKDVhig6Qdi/o4x92ulmurQbp7r8J78rUG2cF81ou9/FafgOrquOR4GNC
7UrR4WqJlI2NMLndeGAXksEGK8HpiSukvFirOg2OmYwuch7eMxAUuHu8AvyToK7o
s2Aknn66RF9U/dQCVNxaBzwqI+HCK+QugosjjppwahjGgaRkcHqYoUKOdDYT2N/6
EQjz91gaCa4ZNaydQvm04Bnj2vEq9QZVFH8j7hIHLjlODAXxxiIalL49PIgWvwTe
gE7uPt80BQeTSLw5Aw15POoXNyPMreXHIhHtPD/Er6UXW6MsEoiX2FjpebU2Haeh
yipcdc7Bre61MyfPv/ScA8/34WWV+MC7MRksq9DvdunWylCh4osdsjHrBWqaxPmk
v9fRt6n1S0/tgsQX9vKs4jDl4w1q6VVqz4hkdaI16KRBGkXU476pVysBmo2zDqWf
ukyRTVQL7OOYPesiIgpWKWM4jfrijlgMH6JXXyXlfgWOsSBdCaTF/xr/JIcz+/p7
MmM6WvDHPm7bqdTDwHOLB8xlCFcQEf1ZlgY0k6Tg4Zh+q2BfQQev3hidG7LEjU9p
2Ns2mUayOI0NNA2w1NzKCEjbBcHbDVpiHdN6ncMJjtNPrsg49sFNsm0cxL2AQmg1
mQc67yV0d9h7sVAZY3SsQVpz61odWDkYtQOxwkS5Qa56VEs7rJ0Ba0iRC1WtIEd7
d8VUUnM7C6iBByp037wFCqTFhrp5RQ8QlJiWAtBdwLawZaWzPwuJctBqzC3GmneD
YddhHBQ80aqqqKPtfqjlFnmjnNGLuYtBgVA3jYztZrTOhafQ8wIeqG5nl21JJCte
HoqYeDodSDESYuQsxuBZ4lK8+YV/CnWk6sxy4vDrjyoyFDkzV6/n1BP3uheB+GYp
CKKQSfVB7c18dKbBa6+sIqJXQPDx5JASWrHGDs7pOgxM2hQVHlr4sZBChGrCNNes
mLdG7JmJIxtrQpQKzT4S+04MOO65ZhfEXvyVQ3KQrNOQzfwgw7HuNywCYlTN/Hc2
bEAldW3V/AhSPyjWi0bTfWqarEF0NjgPn0oCc5gPFMUef9HBfZ+6K5RFP8fbsfsQ
v4cfpjuUVkYYrngLT8mX3e4rLwolppKlMmqaZsMviKmHE8cAh/r469yqKbW+aTjT
b9tedHyjI0xQbTCrYK2nx5Xbtu0cZB9BBaeRFueaVXIRXcd7wO++sGtcJQJ3p+Gl
3R7FQ7NHKr9NRJF+B6EyRpwBPw5MQCyd+PcNGd+ftpNNTR7R8uZfAKPXcEZLbhYx
lZEzW70hd1RKZevF1sdpHAT+j4YOhl09+lfiiJL8TxXqlddUn+lzc8R/tdizSYnU
e3huBhNS7xPU/6PvG48snWhTxaqXx2LR3Fi2dk6iRsiGWwc1GYalTFTXFhodS7MY
-----END RSA PRIVATE KEY-----
```

>Nota: la visualización con `ASN.1 JavaScript decoder` y `openssl asn1parse -in rsakeypair02_des3.pem -inform PEM -i -dump` fallarán al estar cifrado el archivo.

```
$ openssl rsa -in rsakeypair02_des3.pem -inform PEM -text -passin pass:changeit
Private-Key: (2048 bit)
modulus:
    00:d8:44:a6:b7:57:28:35:c9:72:0a:ef:8b:ed:8f:
    2a:33:85:a3:0a:8b:b6:de:59:82:22:66:b3:8b:3b:
    d9:25:da:f3:3c:29:81:43:62:0a:36:44:36:05:82:
    4b:76:0b:08:40:0f:bf:ce:8a:84:d7:32:4e:70:50:
    f2:c7:e7:38:6c:18:ce:78:1d:41:42:28:e7:d3:2a:
    e9:51:d1:51:09:f2:94:df:ac:c5:9e:91:26:5c:46:
    8b:e2:04:ad:ae:56:b7:e2:c1:e0:34:79:ac:ce:da:
    0b:7e:98:e7:05:69:75:bb:fe:4a:65:eb:cc:97:e7:
    b9:0a:f4:aa:9c:63:bb:54:ef:11:dd:cc:39:a5:5b:
    e1:9f:e2:c6:6f:5a:3d:7b:9c:18:a0:28:c3:fe:de:
    bb:ff:d4:04:d6:2c:90:6c:bb:14:7f:bd:b3:8a:1b:
    b4:bf:06:42:a2:1a:1a:c1:7d:26:7e:6d:a1:5d:f5:
    74:81:f1:f6:75:d8:37:ec:49:18:7e:9b:81:f5:b2:
    8f:ec:29:13:88:de:07:80:bf:5b:2e:6d:92:95:4c:
    64:06:bd:fc:6a:d3:18:bc:04:b8:57:24:43:f7:02:
    55:4d:df:f5:96:1f:7f:05:d4:02:1b:0b:ce:c5:d9:
    64:f0:9d:fc:b4:a6:b5:fb:3d:cb:76:2c:4c:85:d9:
    12:21
publicExponent: 65537 (0x10001)
privateExponent:
    00:ca:5e:6b:16:92:42:f9:19:b8:52:4d:57:45:83:
    3a:bd:1d:66:b4:3e:8a:6e:c3:99:07:31:cc:fe:13:
    91:9d:5f:a5:4d:7d:95:0c:85:98:02:b1:8f:4f:56:
    5d:e0:66:70:60:e9:9b:d4:dc:3f:73:d3:0a:1d:05:
    52:39:06:bf:00:3f:b2:2c:29:4b:e0:69:c0:8c:13:
    d2:62:12:18:df:d3:13:c5:bd:b8:ea:28:ee:99:58:
    16:1a:f7:bf:c1:ac:39:c3:c1:21:58:59:27:00:24:
    7e:af:06:ca:de:80:91:7f:c9:72:ba:ae:8c:32:f4:
    cd:6d:58:06:b1:f2:8c:df:ea:5f:96:bb:2e:63:44:
    c3:71:48:15:6c:52:64:0e:13:ed:ab:e9:ea:c9:fc:
    67:f2:07:29:bf:0f:01:4f:18:0d:49:19:1d:ae:2e:
    b5:2f:d1:6b:8c:12:e4:82:36:a4:7f:f2:5b:a8:36:
    0e:5e:7e:ea:32:86:1e:08:9b:87:3c:64:fc:da:f1:
    3f:60:a4:a2:4e:6a:cf:cc:83:9a:4e:4b:b1:98:98:
    7a:ca:46:8f:5a:74:39:19:97:b2:a2:71:d8:f4:68:
    83:cc:7a:4d:ff:12:4d:0c:7f:5b:1c:1e:61:13:ae:
    a3:5a:f7:24:eb:f6:7e:71:82:fc:05:38:78:3f:57:
    6a:85
prime1:
    00:ff:7f:e9:7f:f8:22:53:17:3a:47:6a:27:d6:a6:
    07:96:cb:bd:ff:58:b8:84:59:c0:4e:6e:fe:d4:bf:
    00:7e:b9:cd:7c:40:86:56:90:f2:6f:81:a8:e7:1c:
    f4:1b:29:5d:fd:8c:1b:ce:5c:15:a5:d7:c3:9f:69:
    74:1a:5c:19:05:cc:26:8a:28:37:9a:fe:ec:a9:fa:
    de:d1:ab:29:f7:7d:c5:b9:11:2c:43:b6:df:d7:73:
    90:8d:b4:a6:7c:ec:1f:5d:cb:15:f0:e7:71:5b:ee:
    b5:8e:1a:c9:fb:cd:81:ac:02:77:4e:82:17:d4:d9:
    8d:3a:8a:57:aa:12:2b:7a:5b
prime2:
    00:d8:b1:12:4c:13:f6:5b:71:32:06:91:72:9a:87:
    73:5a:be:a8:3f:39:85:28:6a:0c:2a:7e:91:bd:56:
    f3:d1:dc:2b:81:cc:f8:2d:21:bf:67:15:be:ce:9b:
    79:e0:1a:62:9b:ff:a0:f5:07:de:b5:4a:a6:f3:60:
    cc:9e:8e:64:df:d7:40:36:6e:94:49:cf:3a:f9:18:
    9b:c0:36:1d:92:95:46:0a:83:9d:f2:27:ff:94:76:
    e8:82:23:bc:42:6c:22:21:4a:04:09:93:d3:15:bb:
    99:a2:a5:08:ca:a2:26:31:28:2e:c1:1e:b5:fe:1b:
    4d:0c:66:c7:8e:e3:09:b6:33
exponent1:
    00:9d:06:aa:25:42:05:26:78:02:13:5e:04:ba:01:
    34:6c:d3:2f:24:3e:0c:b9:1a:c7:fb:ec:6f:a5:a4:
    f5:d2:ec:b2:83:88:cc:87:6b:71:f4:62:05:c2:74:
    a2:fa:06:84:83:8d:96:91:4f:3a:e6:c0:22:b4:58:
    85:48:a6:0d:a2:e8:83:4f:29:c4:72:f7:f5:cf:ca:
    25:23:4b:44:f1:9c:54:e6:96:cd:53:98:93:1d:c1:
    22:b8:54:ac:75:de:50:b0:be:5d:ef:be:ee:86:87:
    c1:01:63:29:7c:1e:23:71:e9:da:15:60:12:f1:77:
    fb:3a:5d:47:df:78:67:4c:3d
exponent2:
    07:5c:2c:54:ed:30:a6:f6:77:4e:85:ce:30:b5:7d:
    13:8d:fa:44:fb:7d:2c:5c:3d:c1:8b:bf:03:81:5a:
    92:bc:39:b6:14:f2:31:06:3e:6c:0f:ef:05:31:7e:
    b8:3f:ed:2d:83:47:42:4b:5c:23:28:3f:f7:70:48:
    42:f2:6f:36:b3:26:f9:32:54:53:15:78:d9:51:1e:
    a9:2e:ad:5e:77:37:83:cf:e8:a3:7a:fc:51:5c:a0:
    95:1b:05:9f:f3:f0:4c:d5:28:c1:f2:85:6e:ed:8a:
    50:5d:ee:a8:83:4d:6c:ca:02:9c:88:a0:81:db:07:
    88:ee:4c:c4:eb:ad:3e:c5
coefficient:
    15:99:5e:f9:ab:d5:83:62:97:bb:7e:35:9a:d2:cf:
    eb:92:2f:2e:0c:7f:5f:4d:65:78:be:00:b2:f2:fb:
    38:ca:44:55:c5:36:74:3a:ec:0c:1b:88:ea:36:e5:
    bb:f4:06:af:60:1f:5b:ca:58:24:8a:33:3c:c0:4d:
    83:79:ba:be:59:5d:b1:0c:59:f7:bd:d8:8e:ff:bf:
    18:88:2f:3e:60:86:0c:19:44:7b:5c:26:0e:8b:b7:
    09:37:ec:a4:58:84:08:16:00:5f:56:90:b2:45:68:
    8c:5f:4d:66:5a:fd:85:de:25:15:ac:d6:2a:5c:33:
    ce:9a:9c:20:a9:ed:b5:48
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA2ESmt1coNclyCu+L7Y8qM4WjCou23lmCImazizvZJdrzPCmB
Q2IKNkQ2BYJLdgsIQA+/zoqE1zJOcFDyx+c4bBjOeB1BQijn0yrpUdFRCfKU36zF
npEmXEaL4gStrla34sHgNHmsztoLfpjnBWl1u/5KZevMl+e5CvSqnGO7VO8R3cw5
pVvhn+LGb1o9e5wYoCjD/t67/9QE1iyQbLsUf72zihu0vwZCohoawX0mfm2hXfV0
gfH2ddg37EkYfpuB9bKP7CkTiN4HgL9bLm2SlUxkBr38atMYvAS4VyRD9wJVTd/1
lh9/BdQCGwvOxdlk8J38tKa1+z3LdixMhdkSIQIDAQABAoIBAQDKXmsWkkL5GbhS
TVdFgzq9HWa0Popuw5kHMcz+E5GdX6VNfZUMhZgCsY9PVl3gZnBg6ZvU3D9z0wod
BVI5Br8AP7IsKUvgacCME9JiEhjf0xPFvbjqKO6ZWBYa97/BrDnDwSFYWScAJH6v
BsregJF/yXK6rowy9M1tWAax8ozf6l+Wuy5jRMNxSBVsUmQOE+2r6erJ/GfyBym/
DwFPGA1JGR2uLrUv0WuMEuSCNqR/8luoNg5efuoyhh4Im4c8ZPza8T9gpKJOas/M
g5pOS7GYmHrKRo9adDkZl7Kicdj0aIPMek3/Ek0Mf1scHmETrqNa9yTr9n5xgvwF
OHg/V2qFAoGBAP9/6X/4IlMXOkdqJ9amB5bLvf9YuIRZwE5u/tS/AH65zXxAhlaQ
8m+BqOcc9BspXf2MG85cFaXXw59pdBpcGQXMJoooN5r+7Kn63tGrKfd9xbkRLEO2
39dzkI20pnzsH13LFfDncVvutY4ayfvNgawCd06CF9TZjTqKV6oSK3pbAoGBANix
EkwT9ltxMgaRcpqHc1q+qD85hShqDCp+kb1W89HcK4HM+C0hv2cVvs6beeAaYpv/
oPUH3rVKpvNgzJ6OZN/XQDZulEnPOvkYm8A2HZKVRgqDnfIn/5R26IIjvEJsIiFK
BAmT0xW7maKlCMqiJjEoLsEetf4bTQxmx47jCbYzAoGBAJ0GqiVCBSZ4AhNeBLoB
NGzTLyQ+DLkax/vsb6Wk9dLssoOIzIdrcfRiBcJ0ovoGhIONlpFPOubAIrRYhUim
DaLog08pxHL39c/KJSNLRPGcVOaWzVOYkx3BIrhUrHXeULC+Xe++7oaHwQFjKXwe
I3Hp2hVgEvF3+zpdR994Z0w9AoGAB1wsVO0wpvZ3ToXOMLV9E436RPt9LFw9wYu/
A4Fakrw5thTyMQY+bA/vBTF+uD/tLYNHQktcIyg/93BIQvJvNrMm+TJUUxV42VEe
qS6tXnc3g8/oo3r8UVyglRsFn/PwTNUowfKFbu2KUF3uqINNbMoCnIiggdsHiO5M
xOutPsUCgYAVmV75q9WDYpe7fjWa0s/rki8uDH9fTWV4vgCy8vs4ykRVxTZ0OuwM
G4jqNuW79AavYB9bylgkijM8wE2Debq+WV2xDFn3vdiO/78YiC8+YIYMGUR7XCYO
i7cJN+ykWIQIFgBfVpCyRWiMX01mWv2F3iUVrNYqXDPOmpwgqe21SA==
-----END RSA PRIVATE KEY-----
```
>Nota: Adviértase la presencia del parámetro `-passin pass:changeit` para indicar la password (`PBE - Password Based Encription`) con la que está cifrada la clave RSA.

#### Convertida a formato `DER` manteniendo el cifrado con `PBE - Password Based Encription` y `des3 - 'Triple DES`:

```
$ openssl rsa -in rsakeypair02_des3.pem -inform PEM -out rsakeypair02_des3.der -outform DER -des3 -passin pass:changeit -passout pass:changeit
writing RSA key
```

>ADVERTENCIA: Aunque APARENTEMENTE debería haber generado una CLAVE RSA protegida por clave, SIN EMBARGO, OpenSSL la ha generado SIN PROTECCION (puede apreciarse en que NO solicita la password) 

* Visualización:

```
$ openssl rsa -in rsakeypair02_des3.der -inform DER -text
Private-Key: (2048 bit)
modulus:
    00:d8:44:a6:b7:57:28:35:c9:72:0a:ef:8b:ed:8f:
    2a:33:85:a3:0a:8b:b6:de:59:82:22:66:b3:8b:3b:
    d9:25:da:f3:3c:29:81:43:62:0a:36:44:36:05:82:
    4b:76:0b:08:40:0f:bf:ce:8a:84:d7:32:4e:70:50:
    f2:c7:e7:38:6c:18:ce:78:1d:41:42:28:e7:d3:2a:
    e9:51:d1:51:09:f2:94:df:ac:c5:9e:91:26:5c:46:
    8b:e2:04:ad:ae:56:b7:e2:c1:e0:34:79:ac:ce:da:
    0b:7e:98:e7:05:69:75:bb:fe:4a:65:eb:cc:97:e7:
    b9:0a:f4:aa:9c:63:bb:54:ef:11:dd:cc:39:a5:5b:
    e1:9f:e2:c6:6f:5a:3d:7b:9c:18:a0:28:c3:fe:de:
    bb:ff:d4:04:d6:2c:90:6c:bb:14:7f:bd:b3:8a:1b:
    b4:bf:06:42:a2:1a:1a:c1:7d:26:7e:6d:a1:5d:f5:
    74:81:f1:f6:75:d8:37:ec:49:18:7e:9b:81:f5:b2:
    8f:ec:29:13:88:de:07:80:bf:5b:2e:6d:92:95:4c:
    64:06:bd:fc:6a:d3:18:bc:04:b8:57:24:43:f7:02:
    55:4d:df:f5:96:1f:7f:05:d4:02:1b:0b:ce:c5:d9:
    64:f0:9d:fc:b4:a6:b5:fb:3d:cb:76:2c:4c:85:d9:
    12:21
publicExponent: 65537 (0x10001)
privateExponent:
    00:ca:5e:6b:16:92:42:f9:19:b8:52:4d:57:45:83:
    3a:bd:1d:66:b4:3e:8a:6e:c3:99:07:31:cc:fe:13:
    91:9d:5f:a5:4d:7d:95:0c:85:98:02:b1:8f:4f:56:
    5d:e0:66:70:60:e9:9b:d4:dc:3f:73:d3:0a:1d:05:
    52:39:06:bf:00:3f:b2:2c:29:4b:e0:69:c0:8c:13:
    d2:62:12:18:df:d3:13:c5:bd:b8:ea:28:ee:99:58:
    16:1a:f7:bf:c1:ac:39:c3:c1:21:58:59:27:00:24:
    7e:af:06:ca:de:80:91:7f:c9:72:ba:ae:8c:32:f4:
    cd:6d:58:06:b1:f2:8c:df:ea:5f:96:bb:2e:63:44:
    c3:71:48:15:6c:52:64:0e:13:ed:ab:e9:ea:c9:fc:
    67:f2:07:29:bf:0f:01:4f:18:0d:49:19:1d:ae:2e:
    b5:2f:d1:6b:8c:12:e4:82:36:a4:7f:f2:5b:a8:36:
    0e:5e:7e:ea:32:86:1e:08:9b:87:3c:64:fc:da:f1:
    3f:60:a4:a2:4e:6a:cf:cc:83:9a:4e:4b:b1:98:98:
    7a:ca:46:8f:5a:74:39:19:97:b2:a2:71:d8:f4:68:
    83:cc:7a:4d:ff:12:4d:0c:7f:5b:1c:1e:61:13:ae:
    a3:5a:f7:24:eb:f6:7e:71:82:fc:05:38:78:3f:57:
    6a:85
prime1:
    00:ff:7f:e9:7f:f8:22:53:17:3a:47:6a:27:d6:a6:
    07:96:cb:bd:ff:58:b8:84:59:c0:4e:6e:fe:d4:bf:
    00:7e:b9:cd:7c:40:86:56:90:f2:6f:81:a8:e7:1c:
    f4:1b:29:5d:fd:8c:1b:ce:5c:15:a5:d7:c3:9f:69:
    74:1a:5c:19:05:cc:26:8a:28:37:9a:fe:ec:a9:fa:
    de:d1:ab:29:f7:7d:c5:b9:11:2c:43:b6:df:d7:73:
    90:8d:b4:a6:7c:ec:1f:5d:cb:15:f0:e7:71:5b:ee:
    b5:8e:1a:c9:fb:cd:81:ac:02:77:4e:82:17:d4:d9:
    8d:3a:8a:57:aa:12:2b:7a:5b
prime2:
    00:d8:b1:12:4c:13:f6:5b:71:32:06:91:72:9a:87:
    73:5a:be:a8:3f:39:85:28:6a:0c:2a:7e:91:bd:56:
    f3:d1:dc:2b:81:cc:f8:2d:21:bf:67:15:be:ce:9b:
    79:e0:1a:62:9b:ff:a0:f5:07:de:b5:4a:a6:f3:60:
    cc:9e:8e:64:df:d7:40:36:6e:94:49:cf:3a:f9:18:
    9b:c0:36:1d:92:95:46:0a:83:9d:f2:27:ff:94:76:
    e8:82:23:bc:42:6c:22:21:4a:04:09:93:d3:15:bb:
    99:a2:a5:08:ca:a2:26:31:28:2e:c1:1e:b5:fe:1b:
    4d:0c:66:c7:8e:e3:09:b6:33
exponent1:
    00:9d:06:aa:25:42:05:26:78:02:13:5e:04:ba:01:
    34:6c:d3:2f:24:3e:0c:b9:1a:c7:fb:ec:6f:a5:a4:
    f5:d2:ec:b2:83:88:cc:87:6b:71:f4:62:05:c2:74:
    a2:fa:06:84:83:8d:96:91:4f:3a:e6:c0:22:b4:58:
    85:48:a6:0d:a2:e8:83:4f:29:c4:72:f7:f5:cf:ca:
    25:23:4b:44:f1:9c:54:e6:96:cd:53:98:93:1d:c1:
    22:b8:54:ac:75:de:50:b0:be:5d:ef:be:ee:86:87:
    c1:01:63:29:7c:1e:23:71:e9:da:15:60:12:f1:77:
    fb:3a:5d:47:df:78:67:4c:3d
exponent2:
    07:5c:2c:54:ed:30:a6:f6:77:4e:85:ce:30:b5:7d:
    13:8d:fa:44:fb:7d:2c:5c:3d:c1:8b:bf:03:81:5a:
    92:bc:39:b6:14:f2:31:06:3e:6c:0f:ef:05:31:7e:
    b8:3f:ed:2d:83:47:42:4b:5c:23:28:3f:f7:70:48:
    42:f2:6f:36:b3:26:f9:32:54:53:15:78:d9:51:1e:
    a9:2e:ad:5e:77:37:83:cf:e8:a3:7a:fc:51:5c:a0:
    95:1b:05:9f:f3:f0:4c:d5:28:c1:f2:85:6e:ed:8a:
    50:5d:ee:a8:83:4d:6c:ca:02:9c:88:a0:81:db:07:
    88:ee:4c:c4:eb:ad:3e:c5
coefficient:
    15:99:5e:f9:ab:d5:83:62:97:bb:7e:35:9a:d2:cf:
    eb:92:2f:2e:0c:7f:5f:4d:65:78:be:00:b2:f2:fb:
    38:ca:44:55:c5:36:74:3a:ec:0c:1b:88:ea:36:e5:
    bb:f4:06:af:60:1f:5b:ca:58:24:8a:33:3c:c0:4d:
    83:79:ba:be:59:5d:b1:0c:59:f7:bd:d8:8e:ff:bf:
    18:88:2f:3e:60:86:0c:19:44:7b:5c:26:0e:8b:b7:
    09:37:ec:a4:58:84:08:16:00:5f:56:90:b2:45:68:
    8c:5f:4d:66:5a:fd:85:de:25:15:ac:d6:2a:5c:33:
    ce:9a:9c:20:a9:ed:b5:48
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA2ESmt1coNclyCu+L7Y8qM4WjCou23lmCImazizvZJdrzPCmB
Q2IKNkQ2BYJLdgsIQA+/zoqE1zJOcFDyx+c4bBjOeB1BQijn0yrpUdFRCfKU36zF
npEmXEaL4gStrla34sHgNHmsztoLfpjnBWl1u/5KZevMl+e5CvSqnGO7VO8R3cw5
pVvhn+LGb1o9e5wYoCjD/t67/9QE1iyQbLsUf72zihu0vwZCohoawX0mfm2hXfV0
gfH2ddg37EkYfpuB9bKP7CkTiN4HgL9bLm2SlUxkBr38atMYvAS4VyRD9wJVTd/1
lh9/BdQCGwvOxdlk8J38tKa1+z3LdixMhdkSIQIDAQABAoIBAQDKXmsWkkL5GbhS
TVdFgzq9HWa0Popuw5kHMcz+E5GdX6VNfZUMhZgCsY9PVl3gZnBg6ZvU3D9z0wod
BVI5Br8AP7IsKUvgacCME9JiEhjf0xPFvbjqKO6ZWBYa97/BrDnDwSFYWScAJH6v
BsregJF/yXK6rowy9M1tWAax8ozf6l+Wuy5jRMNxSBVsUmQOE+2r6erJ/GfyBym/
DwFPGA1JGR2uLrUv0WuMEuSCNqR/8luoNg5efuoyhh4Im4c8ZPza8T9gpKJOas/M
g5pOS7GYmHrKRo9adDkZl7Kicdj0aIPMek3/Ek0Mf1scHmETrqNa9yTr9n5xgvwF
OHg/V2qFAoGBAP9/6X/4IlMXOkdqJ9amB5bLvf9YuIRZwE5u/tS/AH65zXxAhlaQ
8m+BqOcc9BspXf2MG85cFaXXw59pdBpcGQXMJoooN5r+7Kn63tGrKfd9xbkRLEO2
39dzkI20pnzsH13LFfDncVvutY4ayfvNgawCd06CF9TZjTqKV6oSK3pbAoGBANix
EkwT9ltxMgaRcpqHc1q+qD85hShqDCp+kb1W89HcK4HM+C0hv2cVvs6beeAaYpv/
oPUH3rVKpvNgzJ6OZN/XQDZulEnPOvkYm8A2HZKVRgqDnfIn/5R26IIjvEJsIiFK
BAmT0xW7maKlCMqiJjEoLsEetf4bTQxmx47jCbYzAoGBAJ0GqiVCBSZ4AhNeBLoB
NGzTLyQ+DLkax/vsb6Wk9dLssoOIzIdrcfRiBcJ0ovoGhIONlpFPOubAIrRYhUim
DaLog08pxHL39c/KJSNLRPGcVOaWzVOYkx3BIrhUrHXeULC+Xe++7oaHwQFjKXwe
I3Hp2hVgEvF3+zpdR994Z0w9AoGAB1wsVO0wpvZ3ToXOMLV9E436RPt9LFw9wYu/
A4Fakrw5thTyMQY+bA/vBTF+uD/tLYNHQktcIyg/93BIQvJvNrMm+TJUUxV42VEe
qS6tXnc3g8/oo3r8UVyglRsFn/PwTNUowfKFbu2KUF3uqINNbMoCnIiggdsHiO5M
xOutPsUCgYAVmV75q9WDYpe7fjWa0s/rki8uDH9fTWV4vgCy8vs4ykRVxTZ0OuwM
G4jqNuW79AavYB9bylgkijM8wE2Debq+WV2xDFn3vdiO/78YiC8+YIYMGUR7XCYO
i7cJN+ykWIQIFgBfVpCyRWiMX01mWv2F3iUVrNYqXDPOmpwgqe21SA==
-----END RSA PRIVATE KEY-----
```

Para evitar este problema podríamos hacerlo mediante manipulación directa:

Editamos el archivo (por ejemplo con `vim` en linea de comando o con `gedit` en entorno gráfico) y eliminamos las lineas de _cabecera_:

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,4B71D975CC60A4FC
```
...  y _pié_:

```
-----END RSA PRIVATE KEY-----
```
```
$ cp rsakeypair02_des3.pem rsakeypair02_des3.b64 && vim rsakeypair02_des3.b64 && cat rsakeypair02_des3.b64
TF60GPXKPaHd3YvBX8HY+oKFs0580y0BDFKn4EkZ7NfROYVT+urP7VcTpnQfWODu
Knf0whwbWZHh2blqiS5sGgl9SEpBMMdAHAJi/vjnkBatFoJuzhlJkQ+8zDNbooZ4
aHuGN9+4JTIFmwM2PeUpbIgk4epuot3CXCkkdDN13i28MY48aL94e0qL9Hw9HxpE
ZAjHgelFKDVhig6Qdi/o4x92ulmurQbp7r8J78rUG2cF81ou9/FafgOrquOR4GNC
7UrR4WqJlI2NMLndeGAXksEGK8HpiSukvFirOg2OmYwuch7eMxAUuHu8AvyToK7o
s2Aknn66RF9U/dQCVNxaBzwqI+HCK+QugosjjppwahjGgaRkcHqYoUKOdDYT2N/6
EQjz91gaCa4ZNaydQvm04Bnj2vEq9QZVFH8j7hIHLjlODAXxxiIalL49PIgWvwTe
gE7uPt80BQeTSLw5Aw15POoXNyPMreXHIhHtPD/Er6UXW6MsEoiX2FjpebU2Haeh
yipcdc7Bre61MyfPv/ScA8/34WWV+MC7MRksq9DvdunWylCh4osdsjHrBWqaxPmk
v9fRt6n1S0/tgsQX9vKs4jDl4w1q6VVqz4hkdaI16KRBGkXU476pVysBmo2zDqWf
ukyRTVQL7OOYPesiIgpWKWM4jfrijlgMH6JXXyXlfgWOsSBdCaTF/xr/JIcz+/p7
MmM6WvDHPm7bqdTDwHOLB8xlCFcQEf1ZlgY0k6Tg4Zh+q2BfQQev3hidG7LEjU9p
2Ns2mUayOI0NNA2w1NzKCEjbBcHbDVpiHdN6ncMJjtNPrsg49sFNsm0cxL2AQmg1
mQc67yV0d9h7sVAZY3SsQVpz61odWDkYtQOxwkS5Qa56VEs7rJ0Ba0iRC1WtIEd7
d8VUUnM7C6iBByp037wFCqTFhrp5RQ8QlJiWAtBdwLawZaWzPwuJctBqzC3GmneD
YddhHBQ80aqqqKPtfqjlFnmjnNGLuYtBgVA3jYztZrTOhafQ8wIeqG5nl21JJCte
HoqYeDodSDESYuQsxuBZ4lK8+YV/CnWk6sxy4vDrjyoyFDkzV6/n1BP3uheB+GYp
CKKQSfVB7c18dKbBa6+sIqJXQPDx5JASWrHGDs7pOgxM2hQVHlr4sZBChGrCNNes
mLdG7JmJIxtrQpQKzT4S+04MOO65ZhfEXvyVQ3KQrNOQzfwgw7HuNywCYlTN/Hc2
bEAldW3V/AhSPyjWi0bTfWqarEF0NjgPn0oCc5gPFMUef9HBfZ+6K5RFP8fbsfsQ
v4cfpjuUVkYYrngLT8mX3e4rLwolppKlMmqaZsMviKmHE8cAh/r469yqKbW+aTjT
b9tedHyjI0xQbTCrYK2nx5Xbtu0cZB9BBaeRFueaVXIRXcd7wO++sGtcJQJ3p+Gl
3R7FQ7NHKr9NRJF+B6EyRpwBPw5MQCyd+PcNGd+ftpNNTR7R8uZfAKPXcEZLbhYx
lZEzW70hd1RKZevF1sdpHAT+j4YOhl09+lfiiJL8TxXqlddUn+lzc8R/tdizSYnU
e3huBhNS7xPU/6PvG48snWhTxaqXx2LR3Fi2dk6iRsiGWwc1GYalTFTXFhodS7MY
```

a continuación con `openssl enc` podemos decodificar el contenido en `Base 64` y obtener su equivalente binario, que constituye el contenido en `DER encoded`:

>[Base 64 - Wikipedia (es)](https://es.wikipedia.org/wiki/Base64)

>[Base 64 - Wikipedia (en)](https://en.wikipedia.org/wiki/Base64)

```
$ openssl enc -d -base64 -in rsakeypair02_des3.b64 -out rsakeypair02_des3.der
```

```
$ xxd -c 16 rsakeypair02_des3.der
00000000: 4c5e b418 f5ca 3da1 dddd 8bc1 5fc1 d8fa  L^....=....._...
00000010: 8285 b34e 7cd3 2d01 0c52 a7e0 4919 ecd7  ...N|.-..R..I...
00000020: d139 8553 faea cfed 5713 a674 1f58 e0ee  .9.S....W..t.X..
00000030: 2a77 f4c2 1c1b 5991 e1d9 b96a 892e 6c1a  *w....Y....j..l.
00000040: 097d 484a 4130 c740 1c02 62fe f8e7 9016  .}HJA0.@..b.....
00000050: ad16 826e ce19 4991 0fbc cc33 5ba2 8678  ...n..I....3[..x
00000060: 687b 8637 dfb8 2532 059b 0336 3de5 296c  h{.7..%2...6=.)l
00000070: 8824 e1ea 6ea2 ddc2 5c29 2474 3375 de2d  .$..n...\)$t3u.-
00000080: bc31 8e3c 68bf 787b 4a8b f47c 3d1f 1a44  .1.<h.x{J..|=..D
00000090: 6408 c781 e945 2835 618a 0e90 762f e8e3  d....E(5a...v/..
000000a0: 1f76 ba59 aead 06e9 eebf 09ef cad4 1b67  .v.Y...........g
000000b0: 05f3 5a2e f7f1 5a7e 03ab aae3 91e0 6342  ..Z...Z~......cB
000000c0: ed4a d1e1 6a89 948d 8d30 b9dd 7860 1792  .J..j....0..x`..
000000d0: c106 2bc1 e989 2ba4 bc58 ab3a 0d8e 998c  ..+...+..X.:....
000000e0: 2e72 1ede 3310 14b8 7bbc 02fc 93a0 aee8  .r..3...{.......
000000f0: b360 249e 7eba 445f 54fd d402 54dc 5a07  .`$.~.D_T...T.Z.
00000100: 3c2a 23e1 c22b e42e 828b 238e 9a70 6a18  <*#..+....#..pj.
00000110: c681 a464 707a 98a1 428e 7436 13d8 dffa  ...dpz..B.t6....
00000120: 1108 f3f7 581a 09ae 1935 ac9d 42f9 b4e0  ....X....5..B...
00000130: 19e3 daf1 2af5 0655 147f 23ee 1207 2e39  ....*..U..#....9
00000140: 4e0c 05f1 c622 1a94 be3d 3c88 16bf 04de  N...."...=<.....
00000150: 804e ee3e df34 0507 9348 bc39 030d 793c  .N.>.4...H.9..y<
00000160: ea17 3723 ccad e5c7 2211 ed3c 3fc4 afa5  ..7#...."..<?...
00000170: 175b a32c 1288 97d8 58e9 79b5 361d a7a1  .[.,....X.y.6...
00000180: ca2a 5c75 cec1 adee b533 27cf bff4 9c03  .*\u.....3'.....
00000190: cff7 e165 95f8 c0bb 3119 2cab d0ef 76e9  ...e....1.,...v.
000001a0: d6ca 50a1 e28b 1db2 31eb 056a 9ac4 f9a4  ..P.....1..j....
000001b0: bfd7 d1b7 a9f5 4b4f ed82 c417 f6f2 ace2  ......KO........
000001c0: 30e5 e30d 6ae9 556a cf88 6475 a235 e8a4  0...j.Uj..du.5..
000001d0: 411a 45d4 e3be a957 2b01 9a8d b30e a59f  A.E....W+.......
000001e0: ba4c 914d 540b ece3 983d eb22 220a 5629  .L.MT....=."".V)
000001f0: 6338 8dfa e28e 580c 1fa2 575f 25e5 7e05  c8....X...W_%.~.
00000200: 8eb1 205d 09a4 c5ff 1aff 2487 33fb fa7b  .. ]......$.3..{
00000210: 3263 3a5a f0c7 3e6e dba9 d4c3 c073 8b07  2c:Z..>n.....s..
00000220: cc65 0857 1011 fd59 9606 3493 a4e0 e198  .e.W...Y..4.....
00000230: 7eab 605f 4107 afde 189d 1bb2 c48d 4f69  ~.`_A.........Oi
00000240: d8db 3699 46b2 388d 0d34 0db0 d4dc ca08  ..6.F.8..4......
00000250: 48db 05c1 db0d 5a62 1dd3 7a9d c309 8ed3  H.....Zb..z.....
00000260: 4fae c838 f6c1 4db2 6d1c c4bd 8042 6835  O..8..M.m....Bh5
00000270: 9907 3aef 2574 77d8 7bb1 5019 6374 ac41  ..:.%tw.{.P.ct.A
00000280: 5a73 eb5a 1d58 3918 b503 b1c2 44b9 41ae  Zs.Z.X9.....D.A.
00000290: 7a54 4b3b ac9d 016b 4891 0b55 ad20 477b  zTK;...kH..U. G{
000002a0: 77c5 5452 733b 0ba8 8107 2a74 dfbc 050a  w.TRs;....*t....
000002b0: a4c5 86ba 7945 0f10 9498 9602 d05d c0b6  ....yE.......]..
000002c0: b065 a5b3 3f0b 8972 d06a cc2d c69a 7783  .e..?..r.j.-..w.
000002d0: 61d7 611c 143c d1aa aaa8 a3ed 7ea8 e516  a.a..<......~...
000002e0: 79a3 9cd1 8bb9 8b41 8150 378d 8ced 66b4  y......A.P7...f.
000002f0: ce85 a7d0 f302 1ea8 6e67 976d 4924 2b5e  ........ng.mI$+^
00000300: 1e8a 9878 3a1d 4831 1262 e42c c6e0 59e2  ...x:.H1.b.,..Y.
00000310: 52bc f985 7f0a 75a4 eacc 72e2 f0eb 8f2a  R.....u...r....*
00000320: 3214 3933 57af e7d4 13f7 ba17 81f8 6629  2.93W.........f)
00000330: 08a2 9049 f541 edcd 7c74 a6c1 6baf ac22  ...I.A..|t..k.."
00000340: a257 40f0 f1e4 9012 5ab1 c60e cee9 3a0c  .W@.....Z.....:.
00000350: 4cda 1415 1e5a f8b1 9042 846a c234 d7ac  L....Z...B.j.4..
00000360: 98b7 46ec 9989 231b 6b42 940a cd3e 12fb  ..F...#.kB...>..
00000370: 4e0c 38ee b966 17c4 5efc 9543 7290 acd3  N.8..f..^..Cr...
00000380: 90cd fc20 c3b1 ee37 2c02 6254 cdfc 7736  ... ...7,.bT..w6
00000390: 6c40 2575 6dd5 fc08 523f 28d6 8b46 d37d  l@%um...R?(..F.}
000003a0: 6a9a ac41 7436 380f 9f4a 0273 980f 14c5  j..At68..J.s....
000003b0: 1e7f d1c1 7d9f ba2b 9445 3fc7 dbb1 fb10  ....}..+.E?.....
000003c0: bf87 1fa6 3b94 5646 18ae 780b 4fc9 97dd  ....;.VF..x.O...
000003d0: ee2b 2f0a 25a6 92a5 326a 9a66 c32f 88a9  .+/.%...2j.f./..
000003e0: 8713 c700 87fa f8eb dcaa 29b5 be69 38d3  ..........)..i8.
000003f0: 6fdb 5e74 7ca3 234c 506d 30ab 60ad a7c7  o.^t|.#LPm0.`...
00000400: 95db b6ed 1c64 1f41 05a7 9116 e79a 5572  .....d.A......Ur
00000410: 115d c77b c0ef beb0 6b5c 2502 77a7 e1a5  .].{....k\%.w...
00000420: dd1e c543 b347 2abf 4d44 917e 07a1 3246  ...C.G*.MD.~..2F
00000430: 9c01 3f0e 4c40 2c9d f8f7 0d19 df9f b693  ..?.L@,.........
00000440: 4d4d 1ed1 f2e6 5f00 a3d7 7046 4b6e 1631  MM...._...pFKn.1
00000450: 9591 335b bd21 7754 4a65 ebc5 d6c7 691c  ..3[.!wTJe....i.
00000460: 04fe 8f86 0e86 5d3d fa57 e288 92fc 4f15  ......]=.W....O.
00000470: ea95 d754 9fe9 7373 c47f b5d8 b349 89d4  ...T..ss.....I..
00000480: 7b78 6e06 1352 ef13 d4ff a3ef 1b8f 2c9d  {xn..R........,.
00000490: 6853 c5aa 97c7 62d1 dc58 b676 4ea2 46c8  hS....b..X.vN.F.
000004a0: 865b 0735 1986 a54c 54d7 161a 1d4b b318  .[.5...LT....K..
```

>Nota: Se trata de un archivo ASN.1 cifrado en triple DES y por lo tanto NO legible directamente como ASN.1 en claro. Veremos la diferencia al utilizar más adelante el formato `PKCS#8`

#### Cambio del algoritmo de cifrado desde `des3 - 'Triple DES` a un cifrado `aes256 - 'AES 256'` (ambos con `PBE - Password Based Encription`)

```
$ openssl rsa -in rsakeypair02_des3.pem -inform PEM -out rsakeypair02_aes256.pem -outform PEM -aes256 -passin pass:changeit -passout pass:changeit
writing RSA key
$ cat rsakeypair02_aes256.pem
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,A0D1824B3EEF78FAE3B13EDED8431A3B

NGMd91eVchYPB4vn9VZfVJmA2vnuexMGTAX97Vm6UlrCsTysAkjl4FYCzClmoFv3
0W7D4YG2mavPITxlwl5WfpeUBQzcW/1ZRGh2JyHIf+gmFx20OTpKgbscjPM6hGux
ZOK6sV7VxaUawpBcMLDe1avELfjAte4SZQvw0VilWJmeX3VLQDPW2vQtx6Fv17bx
dmPiWLg2Wja3WqIXK1n2GuuWYtdov+uTWtVD8Wa8oRpXobQZlV+VIjCMkEIHsZg7
EeMXLdc2FfJkfRUGHEDbBCvdbSUslloRc0RciyUpOGjkvfXyEbKgI43Fp9G4o1oM
AnQoRJpxC507yGg2WlwlIyfGCfcJZ2VI8pjNxfHLktA8NCn5E5SaXOOY3HmsR8Yq
xemushrwVv3z2rWDkkNkCr4zHexlRoqBjUZSWGtKR+w6hYLZxrRT7QNLjTe2kwAI
QhxjbXyFPev4QtfpXj8OwP636YGXHt+6H3cq+089Nv+gacrCa9JleLsQW3PFIlpV
VXld44pyyo9bmuScuIOFXQm/Ut3AxSUMHk2fxNuiZf+RDPtJDJ/2NYSwrIoJcYK9
vBJx4w7zio8wiSACXbnGfftm7MbcCmRWP0M2DQvib2el3UeKu83rppkpGp41AnH0
2i6NAAUsz2Si0KRcSA6E5si8JFROzdet/zlcmt+TLW6l6KqZ4fu0FLrqtL+2IW13
CM7cojAkSDTi81jMKHGCtw+JvBPbHp3tNqhicc1LqT+IzY9rjW1Gchma0vNaOMZ6
1XZNrXS22A5rUvzKPuTbRCpgwziNC+NG5Y6HXhF5yyT1GMhrT7gfpuEqX/Xi06x+
pNDAb9KjShvYNzqimXsfxsUVxWEOZqAA5a6NZC3+5zwCukasy/CyNBozBRBlCVXz
41bmNlhZ3h7DCGVqR4PwpDkjYMPCRccVrVw9k6buxe2opI3N0IyMlW7PV6mDTy3f
XdJ+W4dw4OA6iWf1qE2lpjiijEiKxZ7eTA6FhnOapN/JstnbC2TTX5+gSqo1aLST
jGGAXQpNUVP60yihdogM+VdnM9aC2P4+qbFqbgd26VkbRYowKf5lDPuC23aSGY/D
8x2CiCuNZD4UICned7XMbrf0qhDUy02w8ZDpRvX5A106WDcenOBLUe7j5hnuBgjn
K+OiO1BwasMw89qs9qzXfdnUN1jzthGHeKIP8nykelV0T6LXx6vrpEvPYcfFPBJA
IJbRyK4wrEMFMNpLPRzelJFX/A5qcsSqF6IMl5ni6MHPC7Zr8b22MalKoFfSjMfq
qZldnGVxF9dP5vw9yeql2B+Z9CWIKlHJ9yoz1cDUTQ16p6OfRQiTyauO1RhKIFuR
qgObyZI/kegp+GV/QBsj+H5I7bhbdCJYkkxsg6O9swzKH3/YDr1Gg9kBE3mK9VkZ
h08zPmzB08CwdLqFQge/oB4DDjPrAU0GK2Mc4XdvFWWr2gjoZjC+QiHm3pNMncEL
V+XpeWBtOEHO3yWlSOrTYPj+ti3kH+MyVAHLj2pNZjZ9c5m6DE7sBxiQHdczSsD7
Azszqi4YUWyy5qH0T+fsR2qNOis+VTBlzTRV+3n5WPJm+KKmDYsrR/OjucdDDUwW
-----END RSA PRIVATE KEY-----
$ openssl rsa -in rsakeypair02_aes256.pem -inform PEM -text -passin pass:changeit
Private-Key: (2048 bit)
modulus:
    00:d8:44:a6:b7:57:28:35:c9:72:0a:ef:8b:ed:8f:
    2a:33:85:a3:0a:8b:b6:de:59:82:22:66:b3:8b:3b:
    d9:25:da:f3:3c:29:81:43:62:0a:36:44:36:05:82:
    4b:76:0b:08:40:0f:bf:ce:8a:84:d7:32:4e:70:50:
    f2:c7:e7:38:6c:18:ce:78:1d:41:42:28:e7:d3:2a:
    e9:51:d1:51:09:f2:94:df:ac:c5:9e:91:26:5c:46:
    8b:e2:04:ad:ae:56:b7:e2:c1:e0:34:79:ac:ce:da:
    0b:7e:98:e7:05:69:75:bb:fe:4a:65:eb:cc:97:e7:
    b9:0a:f4:aa:9c:63:bb:54:ef:11:dd:cc:39:a5:5b:
    e1:9f:e2:c6:6f:5a:3d:7b:9c:18:a0:28:c3:fe:de:
    bb:ff:d4:04:d6:2c:90:6c:bb:14:7f:bd:b3:8a:1b:
    b4:bf:06:42:a2:1a:1a:c1:7d:26:7e:6d:a1:5d:f5:
    74:81:f1:f6:75:d8:37:ec:49:18:7e:9b:81:f5:b2:
    8f:ec:29:13:88:de:07:80:bf:5b:2e:6d:92:95:4c:
    64:06:bd:fc:6a:d3:18:bc:04:b8:57:24:43:f7:02:
    55:4d:df:f5:96:1f:7f:05:d4:02:1b:0b:ce:c5:d9:
    64:f0:9d:fc:b4:a6:b5:fb:3d:cb:76:2c:4c:85:d9:
    12:21
publicExponent: 65537 (0x10001)
privateExponent:
    00:ca:5e:6b:16:92:42:f9:19:b8:52:4d:57:45:83:
    3a:bd:1d:66:b4:3e:8a:6e:c3:99:07:31:cc:fe:13:
    91:9d:5f:a5:4d:7d:95:0c:85:98:02:b1:8f:4f:56:
    5d:e0:66:70:60:e9:9b:d4:dc:3f:73:d3:0a:1d:05:
    52:39:06:bf:00:3f:b2:2c:29:4b:e0:69:c0:8c:13:
    d2:62:12:18:df:d3:13:c5:bd:b8:ea:28:ee:99:58:
    16:1a:f7:bf:c1:ac:39:c3:c1:21:58:59:27:00:24:
    7e:af:06:ca:de:80:91:7f:c9:72:ba:ae:8c:32:f4:
    cd:6d:58:06:b1:f2:8c:df:ea:5f:96:bb:2e:63:44:
    c3:71:48:15:6c:52:64:0e:13:ed:ab:e9:ea:c9:fc:
    67:f2:07:29:bf:0f:01:4f:18:0d:49:19:1d:ae:2e:
    b5:2f:d1:6b:8c:12:e4:82:36:a4:7f:f2:5b:a8:36:
    0e:5e:7e:ea:32:86:1e:08:9b:87:3c:64:fc:da:f1:
    3f:60:a4:a2:4e:6a:cf:cc:83:9a:4e:4b:b1:98:98:
    7a:ca:46:8f:5a:74:39:19:97:b2:a2:71:d8:f4:68:
    83:cc:7a:4d:ff:12:4d:0c:7f:5b:1c:1e:61:13:ae:
    a3:5a:f7:24:eb:f6:7e:71:82:fc:05:38:78:3f:57:
    6a:85
prime1:
    00:ff:7f:e9:7f:f8:22:53:17:3a:47:6a:27:d6:a6:
    07:96:cb:bd:ff:58:b8:84:59:c0:4e:6e:fe:d4:bf:
    00:7e:b9:cd:7c:40:86:56:90:f2:6f:81:a8:e7:1c:
    f4:1b:29:5d:fd:8c:1b:ce:5c:15:a5:d7:c3:9f:69:
    74:1a:5c:19:05:cc:26:8a:28:37:9a:fe:ec:a9:fa:
    de:d1:ab:29:f7:7d:c5:b9:11:2c:43:b6:df:d7:73:
    90:8d:b4:a6:7c:ec:1f:5d:cb:15:f0:e7:71:5b:ee:
    b5:8e:1a:c9:fb:cd:81:ac:02:77:4e:82:17:d4:d9:
    8d:3a:8a:57:aa:12:2b:7a:5b
prime2:
    00:d8:b1:12:4c:13:f6:5b:71:32:06:91:72:9a:87:
    73:5a:be:a8:3f:39:85:28:6a:0c:2a:7e:91:bd:56:
    f3:d1:dc:2b:81:cc:f8:2d:21:bf:67:15:be:ce:9b:
    79:e0:1a:62:9b:ff:a0:f5:07:de:b5:4a:a6:f3:60:
    cc:9e:8e:64:df:d7:40:36:6e:94:49:cf:3a:f9:18:
    9b:c0:36:1d:92:95:46:0a:83:9d:f2:27:ff:94:76:
    e8:82:23:bc:42:6c:22:21:4a:04:09:93:d3:15:bb:
    99:a2:a5:08:ca:a2:26:31:28:2e:c1:1e:b5:fe:1b:
    4d:0c:66:c7:8e:e3:09:b6:33
exponent1:
    00:9d:06:aa:25:42:05:26:78:02:13:5e:04:ba:01:
    34:6c:d3:2f:24:3e:0c:b9:1a:c7:fb:ec:6f:a5:a4:
    f5:d2:ec:b2:83:88:cc:87:6b:71:f4:62:05:c2:74:
    a2:fa:06:84:83:8d:96:91:4f:3a:e6:c0:22:b4:58:
    85:48:a6:0d:a2:e8:83:4f:29:c4:72:f7:f5:cf:ca:
    25:23:4b:44:f1:9c:54:e6:96:cd:53:98:93:1d:c1:
    22:b8:54:ac:75:de:50:b0:be:5d:ef:be:ee:86:87:
    c1:01:63:29:7c:1e:23:71:e9:da:15:60:12:f1:77:
    fb:3a:5d:47:df:78:67:4c:3d
exponent2:
    07:5c:2c:54:ed:30:a6:f6:77:4e:85:ce:30:b5:7d:
    13:8d:fa:44:fb:7d:2c:5c:3d:c1:8b:bf:03:81:5a:
    92:bc:39:b6:14:f2:31:06:3e:6c:0f:ef:05:31:7e:
    b8:3f:ed:2d:83:47:42:4b:5c:23:28:3f:f7:70:48:
    42:f2:6f:36:b3:26:f9:32:54:53:15:78:d9:51:1e:
    a9:2e:ad:5e:77:37:83:cf:e8:a3:7a:fc:51:5c:a0:
    95:1b:05:9f:f3:f0:4c:d5:28:c1:f2:85:6e:ed:8a:
    50:5d:ee:a8:83:4d:6c:ca:02:9c:88:a0:81:db:07:
    88:ee:4c:c4:eb:ad:3e:c5
coefficient:
    15:99:5e:f9:ab:d5:83:62:97:bb:7e:35:9a:d2:cf:
    eb:92:2f:2e:0c:7f:5f:4d:65:78:be:00:b2:f2:fb:
    38:ca:44:55:c5:36:74:3a:ec:0c:1b:88:ea:36:e5:
    bb:f4:06:af:60:1f:5b:ca:58:24:8a:33:3c:c0:4d:
    83:79:ba:be:59:5d:b1:0c:59:f7:bd:d8:8e:ff:bf:
    18:88:2f:3e:60:86:0c:19:44:7b:5c:26:0e:8b:b7:
    09:37:ec:a4:58:84:08:16:00:5f:56:90:b2:45:68:
    8c:5f:4d:66:5a:fd:85:de:25:15:ac:d6:2a:5c:33:
    ce:9a:9c:20:a9:ed:b5:48
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA2ESmt1coNclyCu+L7Y8qM4WjCou23lmCImazizvZJdrzPCmB
Q2IKNkQ2BYJLdgsIQA+/zoqE1zJOcFDyx+c4bBjOeB1BQijn0yrpUdFRCfKU36zF
npEmXEaL4gStrla34sHgNHmsztoLfpjnBWl1u/5KZevMl+e5CvSqnGO7VO8R3cw5
pVvhn+LGb1o9e5wYoCjD/t67/9QE1iyQbLsUf72zihu0vwZCohoawX0mfm2hXfV0
gfH2ddg37EkYfpuB9bKP7CkTiN4HgL9bLm2SlUxkBr38atMYvAS4VyRD9wJVTd/1
lh9/BdQCGwvOxdlk8J38tKa1+z3LdixMhdkSIQIDAQABAoIBAQDKXmsWkkL5GbhS
TVdFgzq9HWa0Popuw5kHMcz+E5GdX6VNfZUMhZgCsY9PVl3gZnBg6ZvU3D9z0wod
BVI5Br8AP7IsKUvgacCME9JiEhjf0xPFvbjqKO6ZWBYa97/BrDnDwSFYWScAJH6v
BsregJF/yXK6rowy9M1tWAax8ozf6l+Wuy5jRMNxSBVsUmQOE+2r6erJ/GfyBym/
DwFPGA1JGR2uLrUv0WuMEuSCNqR/8luoNg5efuoyhh4Im4c8ZPza8T9gpKJOas/M
g5pOS7GYmHrKRo9adDkZl7Kicdj0aIPMek3/Ek0Mf1scHmETrqNa9yTr9n5xgvwF
OHg/V2qFAoGBAP9/6X/4IlMXOkdqJ9amB5bLvf9YuIRZwE5u/tS/AH65zXxAhlaQ
8m+BqOcc9BspXf2MG85cFaXXw59pdBpcGQXMJoooN5r+7Kn63tGrKfd9xbkRLEO2
39dzkI20pnzsH13LFfDncVvutY4ayfvNgawCd06CF9TZjTqKV6oSK3pbAoGBANix
EkwT9ltxMgaRcpqHc1q+qD85hShqDCp+kb1W89HcK4HM+C0hv2cVvs6beeAaYpv/
oPUH3rVKpvNgzJ6OZN/XQDZulEnPOvkYm8A2HZKVRgqDnfIn/5R26IIjvEJsIiFK
BAmT0xW7maKlCMqiJjEoLsEetf4bTQxmx47jCbYzAoGBAJ0GqiVCBSZ4AhNeBLoB
NGzTLyQ+DLkax/vsb6Wk9dLssoOIzIdrcfRiBcJ0ovoGhIONlpFPOubAIrRYhUim
DaLog08pxHL39c/KJSNLRPGcVOaWzVOYkx3BIrhUrHXeULC+Xe++7oaHwQFjKXwe
I3Hp2hVgEvF3+zpdR994Z0w9AoGAB1wsVO0wpvZ3ToXOMLV9E436RPt9LFw9wYu/
A4Fakrw5thTyMQY+bA/vBTF+uD/tLYNHQktcIyg/93BIQvJvNrMm+TJUUxV42VEe
qS6tXnc3g8/oo3r8UVyglRsFn/PwTNUowfKFbu2KUF3uqINNbMoCnIiggdsHiO5M
xOutPsUCgYAVmV75q9WDYpe7fjWa0s/rki8uDH9fTWV4vgCy8vs4ykRVxTZ0OuwM
G4jqNuW79AavYB9bylgkijM8wE2Debq+WV2xDFn3vdiO/78YiC8+YIYMGUR7XCYO
i7cJN+ykWIQIFgBfVpCyRWiMX01mWv2F3iUVrNYqXDPOmpwgqe21SA==
-----END RSA PRIVATE KEY-----

```

### Ejemplo 01.04: Utilización del formato estandar `PKCS#8`

>[PKCS #8: Private-Key Information Syntax Standard](https://web.archive.org/web/20061210143206/http://www.rsasecurity.com/rsalabs/node.asp?id=2130)

>[PKCS#8 - Wikipedia (en)](https://en.wikipedia.org/wiki/PKCS_8)

>[Openssl Manual:Pkcs8(1)](https://wiki.openssl.org/index.php/Manual:Pkcs8(1))

Cuando se requiere que los archivos contenedores de parejas de claves (`keypairs`) se encuentren en el formato que define el estandar `PKCS#8`, `openssl` cuenta con el comando `pkcs8` para ello. 

#### Ayuda de `openssl pkcs8`

```
$ openssl pkcs8 --help
Usage pkcs8 [options]
where options are
-in file        input file
-inform X       input format (DER or PEM)
-passin arg     input file pass phrase source
-outform X      output format (DER or PEM)
-out file       output file
-passout arg    output file pass phrase source
-topk8          output PKCS8 file
-nooct          use (nonstandard) no octet format
-embed          use (nonstandard) embedded DSA parameters format
-nsdb           use (nonstandard) DSA Netscape DB format
-noiter         use 1 as iteration count
-nocrypt        use or expect unencrypted private key
-v2 alg         use PKCS#5 v2.0 and cipher "alg"
-v1 obj         use PKCS#5 v1.5 and cipher "alg"
 -engine e       use engine e, possibly a hardware device.
``` 

#### Convertir un `keypair` en formato `PKCS#1` a formato `PKCS#8` (con el archivo resultante NO PROTEGIDO por clave) 

>Notese la opción `-nocrypt`

```
openssl pkcs8 -topk8 -in rsakeypair01.pem -inform PEM -out rsakeypair01_pkcs8.pem -outform PEM -nocrypt
```

* ¿ Cómo son las cabeceras de ambos archivos `PEM` ?:

```
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
$ cat rsakeypair01_pkcs8.pem
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCrSWOi7eRrbFfX
K3iDp2D2ybyKuc1qOY8Dbqz0Ztthsn0CDL/xds8bmHhfakiufXPGXJun5P5NSwlG
nALav6NO9hCI8kkqvNyXEzo8+U0N6OIAa5seGi+r7Nvi2j0sQ7SoSZtlD03w2jZ9
lXJSLNOu40WSDjLyYES2KpMFsvKoBQvOEoiglOGlW9H4Me43e5wIOBhIIAdqYXhm
KFNC3/lgx9yySIjTJ16fbTjpf7Num/Rq5PxKsmDWQRZFSzZc6PTFqMQJqgXzuX5A
OsFYBOWzNtIY/khkqHskrNS/rRxxoCAghhfwGQOb8DtDj5cOwZSAg1hBjAP0kPZf
KNqIvMzlAgMBAAECggEABC6GgDPOn9DFVDt7dbtgYqYKNFjU6wL8uti44D6inH//
DU3CYeLEq6fNSIcPvzLQ4MbiBfdRPhHW6CJm73A+TG81eCJyJtj1ibqucU6k0vIc
kmYOI24AO/JMa/1oAOymPHWPlagJ/xdUvTwozljJ0js6N5ahJ8mx48CmVj1IL1Hl
+uQKqAujfMFjoleccIgb/n5497MExM+FLv4jfNHTTG6R8ThMplcP4zxz1LTUWQWH
wR3/wXJMi/0K9UNYpM1DGevdv0DScCW9wr9weYli0HEYqbft9TxaxGPMVdY21v7w
aaGIdZyKUcN9dlxhEKBSD/rYbZN1etwJwzHSCFFTAQKBgQDcc55IYWM6ZooBxg9N
iRXXBH1fpdyyEJfx1qehn1nCDQItUls6zCHzeuYQmKXW4kWPxsEPZ/XSgAQXuRs+
nuGxbWlORHMKw4tTCvMkjmwh4pf4yMeT4K7o/xRBu25C5+D9yWIbXr1pHd6U3nWY
HdUNtF2Wc3asJkyJ2tHvLyHYcwKBgQDG6DXsZJY1QyjKepN2axwV4MZTzmzbxy5T
mTYOmv6EZ/QGcsSvhm1h0KIvz3ciAxO1f2E5xj9v5BBc5g8FzHGEwV+DWex4eRyV
XrAOjSOWuPRun3qYzx/4bYFrEzKU3U1PDJbaJQmxzceIqKWblIrQ/UhAdSQ2y3bi
ZlOF407nRwKBgFJAQRo4W9F6WJt4zlTbhO6Imk6Jj+JQFqEAueKvlcm1GM7YH3m8
mB5ECHrAuX+HRPTz7yxUWX+EjAJd6jBfD04nyFaPa2zksKL3Puiwyt+Jr7H/F5ex
CRGdum0cBXOpGGUYMiaMHpeNsXJR3gY3ezp1xUVRJ0HruDrnDdTz/OjJAoGAdfvj
kHcRykIt/fy7X7ClFi0gNzHozGzXg+sLb5psgmmPDAwjsuIQG8c/jRCI3/nnRO9r
4nM/ycUEF7a3Dn94joCNgUJjzAo69DcYQ2lgxNSs2LlsZRvnINsDu1nF1Rt38f3L
tVXm7Drdsqgmo6R2uwSiSOmzM3V47U45GN/+O50CgYEAu2WuDQwTdUPtxNjzizKX
Vxztd2mT6XhAd5KAKFfSzy7Nuxh8PzYqeTo8pVsBsbiRjW+QkS4o+xKmH+YSMVnj
5O4mjLMcBMY2l/MOlxOXdikH6H4ejec+css/IWzHFquYpsnjpVfjenK3DczH+YE+
pTxluvHtIyvhsFxDx1mkPb0=
-----END PRIVATE KEY-----
```

* Se observan diferentes cabeceras:
    * PKCS#1 ==> `-----BEGIN RSA PRIVATE KEY-----`
    * PKCS#8 ==> `-----BEGIN PRIVATE KEY-----`

Puede observarse que el estandar `PKCS#1` es específico para `RSA` mientras que el estandar `PKCS#8` prevé la persistencia de otros tipos/algoritmos de claves asimétricas que se verán en siguinetes laboratorios (`DSA`, `EC`, ...) 

* ¿ Cómo es la estructura interna `ASN.1` de ambos archivos ?:

![PKCS#1 vs. PKCS#8](images/PKCS1vsPKCS8_image01.png "PKCS#1 vs. PKCS#8")

Puede observarse en la estructura interna `ASN.1` del archivo en formato  estandar `PKCS#8` indica **expresamanente** el tipo/algoritmo de clave asimétrica que contiene mediante la inclusión de un `OBJECT IDENTIFIER (oid)` específico: `OBJECT IDENTIFIER 1.2.840.113549.1.1.1 rsaEncryption (PKCS #1)`

>[What is an OID?](http://www.oid-info.com/#oid)

>[Object Identifier (OID) Repository](http://www.oid-info.com/)

>[Reference record for OID 1.2.840.113549.1.1.1](http://oidref.com/1.2.840.113549.1.1.1) Rivest, Shamir and Adleman (RSA) encryption (and signing).

>[Ver OID 1.2.840.113549.1.1.1 en oid-info.com](http://oid-info.com/get/1.2.840.113549.1.1.1) _'Description: Rivest, Shamir and Adleman (RSA) encryption (and signing) Information: Defined in IETF RFC 2313, IETF RFC 2437. See also IETF RFC 3370. See also the equivalent but deprecated OID [{joint-iso-itu-t(2) ds(5) algorithm(8) encryptionAlgorithm(1) rsa(1)}](http://oid-info.com/get/2.5.8.1.1).'_ 

>[RedIRIS - Registro de OIDs - RID](https://www.rediris.es/rid/oid/)


#### Convertir un `keypair` en formato `PKCS#1` a formato `PKCS#8`, esta vez en formato `DER`y con el archivo resultante PROTEGIDO por clave

```
$ openssl pkcs8 -topk8 -in rsakeypair01.pem -inform PEM -out rsakeypair01_pkcs8.der -outform DER -passout pass:changeit
``` 

* visualización:

![rsakeypair01_pkcs8.der](images/rsakeypair01_pkcs8_der_image01.png "rsakeypair01_pkcs8.der")

>[Reference record for OID 1.2.840.113549.1.5.3](http://oidref.com/1.2.840.113549.1.5.3) _'Password Based Encryption Algorithm. 
Uses Data Encryption Standard in Cipher Block Chaining Mode (DES-CBC). 
Uses MD5 to hash a password & salt to get Key and IV.'_ [Ver OID 1.2.840.113549.1.5.3 en oid-info.com](http://oid-info.com/get/1.2.840.113549.1.5.3)

```
$ dumpasn1 -adhl rsakeypair01_pkcs8.der
    <30 82 04 E9>
   0 1257: SEQUENCE {
    <30 1B>
   4   27: . SEQUENCE {
    <06 09>
   6    9: . . OBJECT IDENTIFIER pbeWithMD5AndDES-CBC (1 2 840 113549 1 5 3)
         : . . . (PKCS #5)
    <30 0E>
  17   14: . . SEQUENCE {
    <04 08>
  19    8: . . . OCTET STRING 4B 8F 6F 58 02 13 39 7B
    <02 02>
  29    2: . . . INTEGER 2048
         : . . . }
         : . . }
    <04 82 04 C8>
  33 1224: . OCTET STRING
         : . . 4A 40 D9 4A 22 91 0D 5D 88 DE 15 D8 EE 59 E0 D6
         : . . B0 50 02 92 67 B9 15 39 B8 B1 60 C2 8B 53 89 83
         : . . 42 A1 B5 39 F2 C5 42 A5 88 6D 24 87 0B 82 FD DA
         : . . 3F 00 1C 7D B5 15 91 9A B3 A6 14 67 3A A5 CD 75
         : . . 5D 50 34 B4 82 98 B5 F1 8E E4 EB AD 2A D6 F9 0D
         : . . 1A 84 3E B7 AB 78 E7 AB 05 11 FD 74 F8 3F 59 14
         : . . 23 7D 4C 5E 20 B8 C6 FF 38 FD 15 7E D7 67 11 2B
         : . . BE ED AC BD 6C 0C D5 0E 07 D1 11 AD E6 BF 6D 27
         : . . 3D 99 B6 62 10 6F FC 0F 78 F3 1D 8A 3B 92 56 24
         : . . 72 26 C8 77 D1 17 02 B1 81 C1 33 B8 66 F9 EC 44
         : . . E2 72 97 27 80 34 28 CA F5 4C E6 58 97 BD BA 46
         : . . 35 21 17 56 47 68 32 88 AF 35 A9 9A BF 52 E0 A7
         : . . B1 4F BB 90 2A 91 7D B1 77 82 79 A7 E2 A2 A1 75
         : . . 9E 0B E0 36 1C DF 14 5E 63 C7 1C F7 7B F9 D7 AC
         : . . 03 00 1A 1A 73 14 5A 77 43 31 F6 86 A3 38 74 33
         : . . CA 56 04 D9 5C E6 97 08 F3 50 E6 5E 0C 5A 3B E5
         : . . 40 8A 39 4A 9D F3 B6 23 23 26 EA F6 4D 10 F7 6B
         : . . 30 1E C6 07 B9 F5 DE 39 4E 80 C8 13 AA 2C 0E 00
         : . . C1 3C F3 82 97 F5 CF E4 02 85 CE E4 8B 99 59 D9
         : . . A1 85 3B 84 3C 7A F7 F3 C9 7E 96 31 AE 99 79 07
         : . . C6 81 8B D6 6B A6 EA CA D5 50 72 4D 1F 35 AA 39
         : . . 90 D6 19 2B FA AD D2 45 1E 0E 3E C0 9F C1 3E 6A
         : . . 0E D7 76 87 75 5F DE C6 70 4F 13 F5 EF 96 00 6F
         : . . AD C2 33 57 85 13 90 5F D2 B0 6E 62 36 3B 21 F3
         : . . 48 DE 66 4E DD 05 87 2A FC B1 F4 EC CA 94 CB FE
         : . . 21 E2 20 86 63 B8 98 87 39 12 72 A6 45 3A D7 E9
         : . . AD 58 5F 8A 7C E6 45 75 2D 76 CF 22 3A 6E 86 29
         : . . B2 7E 28 C6 B3 BF 42 95 DB 75 AD 47 F4 D7 63 07
         : . . 69 97 30 9D 9A 3E F7 30 5D 2D DC 36 A2 A0 6C C0
         : . . 74 47 73 79 30 EE 96 63 8E 56 5C B7 3E 03 26 B8
         : . . 9D 4B 1D 29 F0 E0 DF C3 D9 5A CF B4 74 C4 7E 22
         : . . EA D0 A2 07 C4 9F 61 27 49 CF F2 C3 94 21 24 2B
         : . . 9B 61 92 9C FF F7 0A BB E5 78 F5 41 45 29 0C 1D
         : . . C6 2F 92 19 79 2B 13 4A C4 1A F7 38 79 6A 8A 7A
         : . . 91 1B A5 C6 04 FD AA 34 D0 62 81 18 3D 14 A0 05
         : . . 74 5B AC 35 F4 70 91 31 B5 95 BF 47 B4 11 6E 4C
         : . . 1D 23 65 73 4A A2 F5 BB 4B 24 1A 33 D1 11 0F 73
         : . . D0 AC 81 6C DF 03 60 6D 29 ED 6F 4F 20 F6 50 F3
         : . . 54 F0 51 BA EC 0F 1F BA F1 41 AC 14 C1 95 50 24
         : . . AC 7F 0C 68 CD 51 7C 13 C3 8F 9B AC DC F1 AF 1B
         : . . 5E A0 DB C9 0A 16 23 2B 60 30 FE 9F 3B E3 28 45
         : . . 26 4D D3 58 C5 B7 C0 9D 31 70 28 1F 8A FE B7 C1
         : . . 17 90 0F DE 21 BD 21 27 F8 16 84 BE 78 2D 73 03
         : . . 98 16 DF 90 11 B1 61 E5 2B 67 A0 E0 77 FA 6B AF
         : . . 9D 14 C5 03 48 C6 00 F7 1B 0E 19 C3 8F 27 3F D4
         : . . E7 D8 E0 F9 FC 76 40 46 A9 51 98 95 FA 87 A7 B0
         : . . C8 AA F0 43 8E 08 4A 40 D4 24 23 77 DD CD 07 E6
         : . . F9 E9 89 9D 0D FE 19 3B 7B 52 A1 51 5A 12 38 B4
         : . . 6B CE 0F 0B DE 48 84 55 90 98 35 8F 10 5B BA 92
         : . . 68 D3 78 5A EF D1 62 42 C3 6A 36 91 A0 8B D7 53
         : . . E3 93 AD C4 5D EB 7D F7 6A 68 79 C1 2F 68 9B C5
         : . . D4 42 AF 98 F3 6C A9 A1 BA 70 BC A0 13 89 D3 42
         : . . B4 36 1F 7B 46 35 56 AA C9 A0 0A 85 33 99 C4 6B
         : . . 30 DD C1 B1 14 2C 0A 12 A5 27 78 8D 3F A1 7D 73
         : . . 4C 6C 8E 36 96 C2 15 43 23 D1 09 40 BE 2C 64 A3
         : . . 91 88 DD 9D 6D FC 02 80 58 31 B6 A2 85 84 5F 6D
         : . . 92 05 93 42 C8 83 B6 B0 DB DB FA F0 D6 86 E2 E7
         : . . A1 4B 08 44 24 5B 39 89 12 ED AE 84 F8 2C 89 C2
         : . . 8C F2 9D 9C E6 4D 37 ED 49 C8 0E FD A2 6B D5 15
         : . . D3 EF 20 A3 98 B2 DE D4 F2 B2 7B E8 FF F8 C4 19
         : . . E2 C2 FE 9F 57 90 73 4D EA 75 29 28 39 A0 4C 25
         : . . FE 16 11 D4 16 F6 F1 E6 F3 C3 72 3F F0 CE 57 C7
         : . . 94 C4 0F 30 73 CF 45 E6 4A F7 A6 F5 48 F4 35 66
         : . . 35 AA 37 C8 FE 17 5B 01 71 8D C5 62 26 41 C3 92
         : . . CD 8D 4E 9B 95 F5 8F 53 65 41 1F 38 97 1F 29 FC
         : . . 44 29 4B F1 71 E5 2D BA 79 4A 47 37 C5 09 A0 4D
         : . . A6 74 DE DB ED 73 15 74 AE AE 91 DB 2C F8 5D 66
         : . . 28 E9 1B 1D 83 07 5A BF 79 8F 5F F2 9F 0B 93 9C
         : . . 96 2A 0C 0C 0C 0D 98 5B EE 66 A6 9E 7C 20 B4 4D
         : . . 79 20 B2 0C 12 D7 AC E2 F5 6A 21 7E E9 EA FB CA
         : . . F5 E5 C6 6A F8 C3 09 64 14 63 DB 8A 26 5A E6 49
         : . . BF 69 5D 78 6E E8 0C D1 A3 7E 1C 39 5B AC CF 02
         : . . 4E DC 55 AB DA E5 65 6D FE CE 31 EB 18 C2 63 6D
         : . . 40 80 97 67 16 2A 41 61 53 97 25 BE 17 53 32 D5
         : . . D9 34 B7 E0 A7 06 FF 4A 20 52 15 5B 36 84 CF 3C
         : . . 98 53 9C 29 F5 EB 47 D4 66 F4 87 BF E7 59 3B CC
         : . . 4C 0B 3F 16 01 00 B2 E2
         : . }

```

### Ejemplo 01.05: Extracción de la Clave Pública RSA en diferentes formatos:

#### Formato `PEM` encoded y estandard `PKCS#8` desde un Keypair `RSA` en formato `PEM` encoded y estandard `PKCS#1`:
```
$ openssl rsa -in rsakeypair01.pem -inform PEM -pubout -out rsapubkey01_pkcs8.pem -outform PEM
writing RSA key
$ cat rsapubkey01_pkcs8.pem
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg
9sm8irnNajmPA26s9GbbYbJ9Agy/8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+j
TvYQiPJJKrzclxM6PPlNDejiAGubHhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizT
ruNFkg4y8mBEtiqTBbLyqAULzhKIoJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5
YMfcskiI0yden2046X+zbpv0auT8SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATl
szbSGP5IZKh7JKzUv60ccaAgIIYX8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM
5QIDAQAB
-----END PUBLIC KEY-----

```

>Nota: adviértase que estamos hemos estado utilizando `openssl rsa`

* visualización:

![PKCS#8 Private Key vs Public Key](images/rsapubkey01_image02.png "PKCS#8 Private Key vs Public Key")


#### Formato `DER` encoded:
```
$ openssl rsa -in rsakeypair01.pem -inform PEM -pubout -out rsapubkey01_pkcs8.der -outform DER
writing RSA key
$ dumpasn1 -adhl rsapubkey01_pkcs8.der
    <30 82 01 22>
  0 290: SEQUENCE {
    <30 0D>
  4  13: . SEQUENCE {
    <06 09>
  6   9: . . OBJECT IDENTIFIER rsaEncryption (1 2 840 113549 1 1 1)
       : . . . (PKCS #1)
    <05 00>
 17   0: . . NULL
       : . . }
    <03 82 01 0F>
 19 271: . BIT STRING, encapsulates {
    <30 82 01 0A>
 24 266: . . SEQUENCE {
    <02 82 01 01>
 28 257: . . . INTEGER
       : . . . . 00 AB 49 63 A2 ED E4 6B 6C 57 D7 2B 78 83 A7 60
       : . . . . F6 C9 BC 8A B9 CD 6A 39 8F 03 6E AC F4 66 DB 61
       : . . . . B2 7D 02 0C BF F1 76 CF 1B 98 78 5F 6A 48 AE 7D
       : . . . . 73 C6 5C 9B A7 E4 FE 4D 4B 09 46 9C 02 DA BF A3
       : . . . . 4E F6 10 88 F2 49 2A BC DC 97 13 3A 3C F9 4D 0D
       : . . . . E8 E2 00 6B 9B 1E 1A 2F AB EC DB E2 DA 3D 2C 43
       : . . . . B4 A8 49 9B 65 0F 4D F0 DA 36 7D 95 72 52 2C D3
       : . . . . AE E3 45 92 0E 32 F2 60 44 B6 2A 93 05 B2 F2 A8
       : . . . . 05 0B CE 12 88 A0 94 E1 A5 5B D1 F8 31 EE 37 7B
       : . . . . 9C 08 38 18 48 20 07 6A 61 78 66 28 53 42 DF F9
       : . . . . 60 C7 DC B2 48 88 D3 27 5E 9F 6D 38 E9 7F B3 6E
       : . . . . 9B F4 6A E4 FC 4A B2 60 D6 41 16 45 4B 36 5C E8
       : . . . . F4 C5 A8 C4 09 AA 05 F3 B9 7E 40 3A C1 58 04 E5
       : . . . . B3 36 D2 18 FE 48 64 A8 7B 24 AC D4 BF AD 1C 71
       : . . . . A0 20 20 86 17 F0 19 03 9B F0 3B 43 8F 97 0E C1
       : . . . . 94 80 83 58 41 8C 03 F4 90 F6 5F 28 DA 88 BC CC
       : . . . . E5
    <02 03>
289   3: . . . INTEGER 65537
       : . . . }
       : . . }
       : . }

0 warnings, 0 errors.

```

#### Conversion de la clave `RSA pública` obtenida desde el estandar `PKCS#8` a estandard `PKCS#1`:

```
$ openssl rsa -pubin -in rsapubkey01_pkcs8.pem -inform PEM -RSAPublicKey_out
writing RSA key
-----BEGIN RSA PUBLIC KEY-----
MIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg9sm8irnNajmPA26s9GbbYbJ9Agy/8XbP
G5h4X2pIrn1zxlybp+T+TUsJRpwC2r+jTvYQiPJJKrzclxM6PPlNDejiAGubHhov
q+zb4to9LEO0qEmbZQ9N8No2fZVyUizTruNFkg4y8mBEtiqTBbLyqAULzhKIoJTh
pVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5YMfcskiI0yden2046X+zbpv0auT8SrJg
1kEWRUs2XOj0xajECaoF87l+QDrBWATlszbSGP5IZKh7JKzUv60ccaAgIIYX8BkD
m/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM5QIDAQAB
-----END RSA PUBLIC KEY-----
```

>Nota: obsérvese la cabecera `-----BEGIN RSA PUBLIC KEY-----`

```
$ openssl rsa -pubin -in rsapubkey01_pkcs8.pem -inform PEM -RSAPublicKey_out -out rsapubkey01_pkcs1.pem -outform PEM
writing RSA key
$ cat rsapubkey01_pkcs1.pem
-----BEGIN RSA PUBLIC KEY-----
MIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg9sm8irnNajmPA26s9GbbYbJ9Agy/8XbP
G5h4X2pIrn1zxlybp+T+TUsJRpwC2r+jTvYQiPJJKrzclxM6PPlNDejiAGubHhov
q+zb4to9LEO0qEmbZQ9N8No2fZVyUizTruNFkg4y8mBEtiqTBbLyqAULzhKIoJTh
pVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5YMfcskiI0yden2046X+zbpv0auT8SrJg
1kEWRUs2XOj0xajECaoF87l+QDrBWATlszbSGP5IZKh7JKzUv60ccaAgIIYX8BkD
m/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM5QIDAQAB
-----END RSA PUBLIC KEY-----
```

>Nota: obsérvese la cabecera `-----BEGIN RSA PUBLIC KEY-----`

```
$ openssl rsa -pubin -in rsapubkey01_pkcs8.pem -inform PEM -RSAPublicKey_out -out rsapubkey01_pkcs1.der -outform DER
writing RSA key
$ dumpasn1 -adhl rsapubkey01_pkcs1.der
    <30 82 01 0A>
  0 266: SEQUENCE {
    <02 82 01 01>
  4 257: . INTEGER
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
265   3: . INTEGER 65537
       : . }

0 warnings, 0 errors.	
```

>Nota: obsérvese la **AUSENCIA** del `OBJECT IDENTIFIER rsaEncryption (1 2 840 113549 1 1 1)`

![PKCS#8 Public Key vs PKCS#1 Public Key](images/PKCS8vsPKCS1_image02.png "PKCS#8 Private Key vs Public Key")


#### **DIRECTAMENTE** en estandard `PKCS#1` desde un **Keypair** `RSA` en estandard `PKCS#8`:

* Archivo de origen NO PROTEGIDO con clave:

```
$ openssl rsa -pubout -in rsakeypair01_pkcs8.pem -inform PEM -RSAPublicKey_out -out rsapubkey01b_pkcs1.pem -outform PEM
writing RSA key
$ cat rsapubkey01b_pkcs1.pem
-----BEGIN RSA PUBLIC KEY-----
MIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg9sm8irnNajmPA26s9GbbYbJ9Agy/8XbP
G5h4X2pIrn1zxlybp+T+TUsJRpwC2r+jTvYQiPJJKrzclxM6PPlNDejiAGubHhov
q+zb4to9LEO0qEmbZQ9N8No2fZVyUizTruNFkg4y8mBEtiqTBbLyqAULzhKIoJTh
pVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5YMfcskiI0yden2046X+zbpv0auT8SrJg
1kEWRUs2XOj0xajECaoF87l+QDrBWATlszbSGP5IZKh7JKzUv60ccaAgIIYX8BkD
m/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM5QIDAQAB
-----END RSA PUBLIC KEY-----
```

```
$ openssl rsa -pubout -in rsakeypair01_pkcs8.pem -inform PEM -RSAPublicKey_out -out rsapubkey01b_pkcs1.der -outform DER
writing RSA key
$ dumpasn1 -adhl rsapubkey01b_pkcs1.der
    <30 82 01 0A>
  0 266: SEQUENCE {
    <02 82 01 01>
  4 257: . INTEGER
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
265   3: . INTEGER 65537
       : . }

0 warnings, 0 errors.
```

* Archivo de origen PROTEGIDO con clave:

>Adviértase el parámetro `-passin`

```
$ openssl rsa -pubout -in rsakeypair02_aes256.pem -inform PEM -passin pass:changeit -RSAPublicKey_out -out rsapubkey01c_pkcs1.der -outform DER
writing RSA key
$ dumpasn1 -adhl rsapubkey01c_pkcs1.der
    <30 82 01 0A>
  0 266: SEQUENCE {
    <02 82 01 01>
  4 257: . INTEGER
       : . . 00 D8 44 A6 B7 57 28 35 C9 72 0A EF 8B ED 8F 2A
       : . . 33 85 A3 0A 8B B6 DE 59 82 22 66 B3 8B 3B D9 25
       : . . DA F3 3C 29 81 43 62 0A 36 44 36 05 82 4B 76 0B
       : . . 08 40 0F BF CE 8A 84 D7 32 4E 70 50 F2 C7 E7 38
       : . . 6C 18 CE 78 1D 41 42 28 E7 D3 2A E9 51 D1 51 09
       : . . F2 94 DF AC C5 9E 91 26 5C 46 8B E2 04 AD AE 56
       : . . B7 E2 C1 E0 34 79 AC CE DA 0B 7E 98 E7 05 69 75
       : . . BB FE 4A 65 EB CC 97 E7 B9 0A F4 AA 9C 63 BB 54
       : . . EF 11 DD CC 39 A5 5B E1 9F E2 C6 6F 5A 3D 7B 9C
       : . . 18 A0 28 C3 FE DE BB FF D4 04 D6 2C 90 6C BB 14
       : . . 7F BD B3 8A 1B B4 BF 06 42 A2 1A 1A C1 7D 26 7E
       : . . 6D A1 5D F5 74 81 F1 F6 75 D8 37 EC 49 18 7E 9B
       : . . 81 F5 B2 8F EC 29 13 88 DE 07 80 BF 5B 2E 6D 92
       : . . 95 4C 64 06 BD FC 6A D3 18 BC 04 B8 57 24 43 F7
       : . . 02 55 4D DF F5 96 1F 7F 05 D4 02 1B 0B CE C5 D9
       : . . 64 F0 9D FC B4 A6 B5 FB 3D CB 76 2C 4C 85 D9 12
       : . . 21
    <02 03>
265   3: . INTEGER 65537
       : . }

0 warnings, 0 errors.
```

#### Conversión de una `clave pública RSA` en estandard `PKCS#1` a estandard `PKCS#8`:


```
$ openssl rsa -RSAPublicKey_in -in rsapubkey01b_pkcs1.pem -inform PEM -pubout
writing RSA key
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg
9sm8irnNajmPA26s9GbbYbJ9Agy/8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+j
TvYQiPJJKrzclxM6PPlNDejiAGubHhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizT
ruNFkg4y8mBEtiqTBbLyqAULzhKIoJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5
YMfcskiI0yden2046X+zbpv0auT8SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATl
szbSGP5IZKh7JKzUv60ccaAgIIYX8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM
5QIDAQAB
-----END PUBLIC KEY-----
$ openssl rsa -RSAPublicKey_in -in rsapubkey01b_pkcs1.der -inform DER -pubout
writing RSA key
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg
9sm8irnNajmPA26s9GbbYbJ9Agy/8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+j
TvYQiPJJKrzclxM6PPlNDejiAGubHhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizT
ruNFkg4y8mBEtiqTBbLyqAULzhKIoJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5
YMfcskiI0yden2046X+zbpv0auT8SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATl
szbSGP5IZKh7JKzUv60ccaAgIIYX8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM
5QIDAQAB
-----END PUBLIC KEY-----
```

>Nota: obsérvese la cabecera `-----BEGIN PUBLIC KEY-----` **(PKCS#8)** en lugar de `-----BEGIN RSA PUBLIC KEY-----` **(PKCS#1)**

```
$ openssl rsa -RSAPublicKey_in -in rsapubkey01b_pkcs1.pem -inform PEM -pubout -out rsapubkey01b_pkcs8.pem -outform PEM
writing RSA key
$ cat rsapubkey01b_pkcs8.pem
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq0ljou3ka2xX1yt4g6dg
9sm8irnNajmPA26s9GbbYbJ9Agy/8XbPG5h4X2pIrn1zxlybp+T+TUsJRpwC2r+j
TvYQiPJJKrzclxM6PPlNDejiAGubHhovq+zb4to9LEO0qEmbZQ9N8No2fZVyUizT
ruNFkg4y8mBEtiqTBbLyqAULzhKIoJThpVvR+DHuN3ucCDgYSCAHamF4ZihTQt/5
YMfcskiI0yden2046X+zbpv0auT8SrJg1kEWRUs2XOj0xajECaoF87l+QDrBWATl
szbSGP5IZKh7JKzUv60ccaAgIIYX8BkDm/A7Q4+XDsGUgINYQYwD9JD2XyjaiLzM
5QIDAQAB
-----END PUBLIC KEY-----
```
>Nota: obsérvese la cabecera `-----BEGIN PUBLIC KEY-----` **(PKCS#8)** en lugar de `-----BEGIN RSA PUBLIC KEY-----` **(PKCS#1)**
```
$ openssl rsa -RSAPublicKey_in -in rsapubkey01b_pkcs1.pem -inform PEM -pubout -out rsapubkey01b_pkcs8.der -outform DER
writing RSA key
$ dumpasn1 -adhl rsapubkey01b_pkcs8.der
    <30 82 01 22>
  0 290: SEQUENCE {
    <30 0D>
  4  13: . SEQUENCE {
    <06 09>
  6   9: . . OBJECT IDENTIFIER rsaEncryption (1 2 840 113549 1 1 1)
       : . . . (PKCS #1)
    <05 00>
 17   0: . . NULL
       : . . }
    <03 82 01 0F>
 19 271: . BIT STRING, encapsulates {
    <30 82 01 0A>
 24 266: . . SEQUENCE {
    <02 82 01 01>
 28 257: . . . INTEGER
       : . . . . 00 AB 49 63 A2 ED E4 6B 6C 57 D7 2B 78 83 A7 60
       : . . . . F6 C9 BC 8A B9 CD 6A 39 8F 03 6E AC F4 66 DB 61
       : . . . . B2 7D 02 0C BF F1 76 CF 1B 98 78 5F 6A 48 AE 7D
       : . . . . 73 C6 5C 9B A7 E4 FE 4D 4B 09 46 9C 02 DA BF A3
       : . . . . 4E F6 10 88 F2 49 2A BC DC 97 13 3A 3C F9 4D 0D
       : . . . . E8 E2 00 6B 9B 1E 1A 2F AB EC DB E2 DA 3D 2C 43
       : . . . . B4 A8 49 9B 65 0F 4D F0 DA 36 7D 95 72 52 2C D3
       : . . . . AE E3 45 92 0E 32 F2 60 44 B6 2A 93 05 B2 F2 A8
       : . . . . 05 0B CE 12 88 A0 94 E1 A5 5B D1 F8 31 EE 37 7B
       : . . . . 9C 08 38 18 48 20 07 6A 61 78 66 28 53 42 DF F9
       : . . . . 60 C7 DC B2 48 88 D3 27 5E 9F 6D 38 E9 7F B3 6E
       : . . . . 9B F4 6A E4 FC 4A B2 60 D6 41 16 45 4B 36 5C E8
       : . . . . F4 C5 A8 C4 09 AA 05 F3 B9 7E 40 3A C1 58 04 E5
       : . . . . B3 36 D2 18 FE 48 64 A8 7B 24 AC D4 BF AD 1C 71
       : . . . . A0 20 20 86 17 F0 19 03 9B F0 3B 43 8F 97 0E C1
       : . . . . 94 80 83 58 41 8C 03 F4 90 F6 5F 28 DA 88 BC CC
       : . . . . E5
    <02 03>
289   3: . . . INTEGER 65537
       : . . . }
       : . . }
       : . }

0 warnings, 0 errors.
```

>Nota: obsérvese la **PRESENCIA** del `OBJECT IDENTIFIER rsaEncryption (1 2 840 113549 1 1 1)`

>--------------------------------------------------------------------------
>RESUMEN DE CABECERAS `PEM`: 
>    * PKCS#1 Private Key ==> `-----BEGIN RSA PRIVATE KEY-----`
>    * PKCS#1 Public Key ==> `-----BEGIN RSA PUBLIC KEY-----`
>    * PKCS#8 Private Key ==> `-----BEGIN PRIVATE KEY-----`
>    * PKCS#8 Public Key ==> `-----BEGIN PUBLIC KEY-----`
>--------------------------------------------------------------------------
>[Véase 'ASN.1 key structures in DER and PEM'](https://tls.mbed.org/kb/cryptography/asn1-key-structures-in-der-and-pem)
>
>--------------------------------------------------------------------------

## SECCION 02: Generación y manejo de claves RSA con la herramienta `xca` 

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

* Creación Paso 1:

![Generacion RSA Keypair con XCA - Paso 1](images/XCA_RSA_key_step1.png "Generacion RSA Keypair con XCA - Paso 1")

* Creación Paso 2: 

![Generacion RSA Keypair con XCA - Paso 2](images/XCA_RSA_key_step2.png "Generacion RSA Keypair con XCA - Paso 2")

* Creación Paso 3: 

![Generacion RSA Keypair con XCA - Paso 3](images/XCA_RSA_key_step3.png "Generacion RSA Keypair con XCA - Paso 3")

* Exportación de las claves `privada` y `pública` en diferentes formatos y estándares:

![Exportacion claves - 1](images/XCA_RSA_key_export_step1.png "Exportacion claves - 1")

![Exportacion claves - 2](images/XCA_RSA_key_export_step2.png "Exportacion claves - 2")

* Ejemplo 01: Exportación de la clave privada en `DER` encoding SIN PROTECCION DE CLAVE

![Exportacion claves RSA DER - 1](images/XCA_RSA_key_export_step3.png "Exportacion claves RSA DER - 1")

![Exportacion claves RSA DER - 1](images/XCA_RSA_key_export_step4.png "Exportacion claves RSA DER - 1")

* Ejemplo 02: Exportación de la clave privada en `PEM` encoding CON PROTECCION DE CIFRADO y CLAVE `PBE - Password Based Encription`

![Exportacion claves RSA PEM PKCS8 - 1](images/XCA_RSA_key_export_step5.png "Exportacion claves RSA PEM PKCS8 - 1")

![Exportacion claves RSA PEM PKCS8 - PBE](images/XCA_RSA_key_export_step5a.png "Exportacion claves RSA PEM PKCS8 - PBE")

```
$ cat /home/devel1/tmp/sample/Nueva_clave_RSA.pk8 
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIFDjBABgkqhkiG9w0BBQ0wMzAbBgkqhkiG9w0BBQwwDgQIAqwaAP8ysPACAggA
MBQGCCqGSIb3DQMHBAg+YJjZ8cTDCASCBMi0XkbjgXR4j2eQOtdHBY9l0HGoxLHN
H+YsyPaVtfZfjWVcU3Bz9It/tNETvUqr8aVp7uCVUTDLTQSZnvh+EYZO7z2Lmlks
ZRWyq8NXm151bR9qDMyxNiQQZ/2njuwILXkvqKAgHEOXM+VlW2RQxSvoycmfAmdG
rfLyBUdXXiQI9T3xkU3edRGICjzwAljpMwL5N5qV8PmTr6haS9Lxx2agO65b3DkM
Kw83IPdPLdsMTP+5MI/OeXMK08z/QRf44kOE0p/G94VzQD0WcAGDPhgSnCDI+IRz
/UJfikJg+CnoKNMlhTYoYSEJgEA/0PgnaoWI+LA9HwFhlqVZaSo/V9jwkAGpTe9H
fld+BFsbYmSCu9mq8xTo8K8AGPBpmZZg0yIjDsbATDxL6u276FPwXgPJp47Q0/gw
UxDClXMY6JxBPCtcD/ZrlUKVfSr9u48dkrcqPiKFrYmc3EH47G75pdkXCFPPauQV
NA9bi1I3mixla0DWHyv8+PpuiOVdnjeY3NNQTwoJWflkhvRmOaOB0OrNjQlndiRF
TWUofWyLB4nK5btuYeIZqSTHWB2vSS+EdhQ0Ww4HnJy1P5gH5A3MGZ3nPIgpQ9ux
3RXBFG1I+5vsTPCjNmfGnt3jRRvwC2Osa4cQwVoPjcUN6VNiJhypr/f3DxKFNUVJ
V1BjV3DdnAKNzjzyv5O0BUVFriufkLnjQ781L8ut2xhnn6wUTdaVYFiJn55obzaA
eJ5l7P6Olpl8glDVwUl7iFR4Ibi++Nhsd3NoW/I1t7XbEgmBvyZYvNoysXYWf2VJ
o9eYHlZpzXeSeseYDc0QvbRTHNk/GT4aWYCCGkCXnpJREshiPrmcyahIg/G26Y/A
eC3b81yWtgIh5asii+K9CHWBZnwLjJabcvxsDDUYiQu+hoOV6rDSw1Ve1KHiwER3
qNaW2Pkh+nOyMIh1gbhBrC5489TJmJgLfrac7CFlbm8HgiIrzFqRDpL7/Lo2IlYd
sImpPVOhanxCreTC6CbQAahLf28r4PKlbMsjVCmQkrxLegi3DaGcoqIk/qTixwK7
5OK5D/Uj0w//np5OMoKK20E9IWFADW7La8E6P8mJh4HfE9/BcKhAcn+OFBv2QUMF
heq/8mCudBii2Jqm/QgRZJlL01EwIYaCoa3vjyxOyUK4pyTUN+VHciutrUJbQR0s
gwo+vlG3AbVGHH7Hi4bJwzNDKRoY3nGPxqWLHfTrutHpG5pjnpYHTLGK7IG5QJk9
SkEfEhV3Mjpyo0x67WxOE9/Tl3L1D7t4W4vO7mW10EBahD1fvy9oJub2oVQoKdDq
dPocKt10K6bxcZblN+G5DY3ogs2mH0gZLDgUCo1r2p/OYyLXwI3SFg+/RuynP5sp
ZFyXgCWiAS3Zt1Lrdb1d9+Sx9tcQXF9zDQz5dysCAXzUng7Rv3oENs1mXHg6pVSq
X0l3gQwyL0Z+bQjHFCIDOBckj0qih2C4T3GSQaOx24VeMbFnnEPOcE88LFv0H3IE
3XcozHGHpUTvFqM5hozZ6dqfo6Kl6aPCCQnPT1ep+SkZy9OHnMj2v2VIwhRr98ET
jCe9ePhhgxZVQJ5k3mbUOmQcYC7UQi9OhliOno3dqj4PjNPwWq2elCHYFG2a8h8C
3DI=
-----END ENCRYPTED PRIVATE KEY-----
```

![Exportacion claves RSA PEM PKCS8 - 2](images/XCA_RSA_key_export_step6.png "Exportacion claves RSA PEM PKCS8 - 2")

>[Reference record for OID 1.2.840.113549.1.5.13](http://oidref.com/1.2.840.113549.1.5.13) _'PBES2 encryption scheme (This OID describes a key derivation Algorithm and an encryption Scheme for private keys different from old identifiers)'_

>[Ver OID 1.2.840.113549.1.5.13 en oid-info.com](http://oid-info.com/get/1.2.840.113549.1.5.13) _'Password-Based Encryption Scheme 2 (PBES2). See IETF RFC 8018.'_ 

>[PKCS #5: Password-Based Cryptography Specification - Version 2.1](https://tools.ietf.org/html/rfc8018)

>[Passphrase Based Key Derived Key Function Scheme 2 (PBKDF2): Ver PKCS #5: Password-Based Cryptography Specification - Version 2.1](https://tools.ietf.org/html/rfc8018#section-5.2)

>[Password-Based Encryption Scheme 2 (PBES2): Ver PKCS #5: Password-Based Cryptography Specification - Version 2.1](https://tools.ietf.org/html/rfc8018#section-6.2) _'PBES2 combines a password-based key derivation function, which shall be PBKDF2 (Section 5.2) for this version of PKCS #5, with an underlying encryption scheme.  The key length and any other parameters for the underlying encryption scheme depend on the scheme. PBES2 is recommended for new applications.'_

>[PBKDF2 (Password-Based Key Derivation Function 2) Wikipedia (en)](https://en.wikipedia.org/wiki/PBKDF2)

## SECCION 03: Ejemplos de cifrado y descifrado mediante el uso de `Keypairs RSA` con `openssl rsautl`

>[Public – Private key encryption using OpenSSL](https://www.devco.net/archives/2006/02/13/public_-_private_key_encryption_using_openssl.php)

### Ayuda de `openssl rsautl`

```
$ openssl rsautl --help
Usage: rsautl [options]
-in file        input file
-out file       output file
-inkey file     input key
-keyform arg    private key format - default PEM
-pubin          input is an RSA public
-certin         input is a certificate carrying an RSA public key
-ssl            use SSL v2 padding
-raw            use no padding
-pkcs           use PKCS#1 v1.5 padding (default)
-oaep           use PKCS#1 OAEP
-sign           sign with private key
-verify         verify with public key
-encrypt        encrypt with public key
-decrypt        decrypt with private key
-hexdump        hex dump output
-engine e       use engine e, possibly a hardware device.
-passin arg    pass phrase source

```

### Cifrado y descifrado: Vamos a crear un archivo de texto, calcular su hash SHA256 y vamos a tomar como ejemplo el cifrado y descifrado de dicho hash con `openssl rsautl`

* Tomamos un archivo de texto de ejemplo tomando como contenido cinco párrafos de `'Loem Ipsum ...'`:

 

>[Lorem Ipsum: ¿Que es? y Generador](https://es.lipsum.com/)

```
$ vim loremipsum5.txt
$ cat loremipsum5.txt 
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam at erat nibh. Aliquam ante felis, vestibulum eget est sit amet, eleifend porta risus. Sed sit amet fermentum mi. Nulla pharetra dignissim nunc a placerat. Suspendisse eget sapien mi. Sed aliquet elementum sapien, non porta lectus elementum nec. Vivamus euismod venenatis augue. Nullam vulputate ornare dui eu ultrices. Aliquam erat volutpat. Aenean gravida mi vitae urna maximus, ac semper ligula malesuada.

Pellentesque varius ex justo, vel volutpat erat feugiat ultricies. Etiam sagittis, elit nec mattis bibendum, dolor mauris sollicitudin sapien, id ullamcorper magna purus quis lorem. Nam in convallis velit. Nulla pellentesque turpis a erat lobortis, a semper augue egestas. Suspendisse at mauris sem. Nullam eleifend eros sed finibus fringilla. Quisque dictum orci eget ex sollicitudin, eget fermentum ipsum ullamcorper. Morbi semper tortor sit amet mauris consequat, non faucibus ante consequat. Sed sit amet lectus at lacus accumsan imperdiet ac vestibulum magna. Duis convallis lorem sit amet justo vestibulum finibus. Aliquam id eleifend massa, vel fringilla nisl. Nam eget pulvinar nunc, vel tristique nulla. Cras eu sem consectetur risus malesuada fermentum sed a diam. Nam arcu justo, congue sed ultrices quis, commodo eget tortor. Donec luctus leo vel elit vehicula vulputate.

Suspendisse efficitur sem ut ipsum euismod, in commodo risus mollis. Donec at egestas dui. Aliquam erat risus, tristique ac arcu sit amet, tincidunt vulputate metus. Quisque sodales scelerisque pretium. Donec quis est et nulla mattis ultricies ac sed dui. Aenean mollis condimentum arcu vel vestibulum. Nunc vitae augue vitae nunc iaculis porttitor. Aliquam sagittis nisl at risus elementum, nec tempor augue interdum. Integer tempor est tortor, vitae volutpat diam tristique vel. Fusce eu vehicula velit, eget aliquam orci. Etiam vulputate turpis eget interdum gravida. Nunc porttitor massa ac semper tempus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent non consequat ex, at dictum nisl. Quisque ligula felis, pretium vitae justo ac, dictum pretium mi. Ut in leo dui.

Fusce finibus elementum sapien. Maecenas in justo consequat, tristique nulla id, malesuada eros. Donec viverra massa purus, at laoreet augue malesuada vel. Praesent congue, eros vehicula tristique varius, libero risus mollis nulla, vel laoreet dolor velit quis dolor. Proin cursus viverra dolor id interdum. Praesent vel est ac massa finibus fringilla. Aliquam quis mauris quis leo viverra consectetur. Fusce non aliquam ex. Mauris suscipit sollicitudin ligula. Fusce fringilla augue a faucibus rhoncus. Vivamus nec placerat erat. Nullam mattis risus nec urna rhoncus tempus.

Integer et sagittis elit. Nunc ex justo, vulputate id massa vel, rhoncus ultrices felis. Morbi semper, dui quis consequat accumsan, massa eros scelerisque magna, vel tincidunt lectus urna vitae nibh. Donec dolor erat, vulputate nec ex ultrices, porttitor efficitur risus. Quisque in massa vitae est placerat euismod eu a nulla. Curabitur malesuada condimentum dui, nec pellentesque ligula euismod in. Duis ac leo nec erat elementum bibendum eu sit amet odio. Quisque feugiat tempor tellus quis fermentum. Mauris aliquam laoreet sollicitudin.
```

* calculamos su `huella digital` como `Hash SHA-256`:

Alternativa 1:
```
$ sha256sum loremipsum5.txt | awk '{print $1}' 1>loremipsum5.sha256 2>/dev/null
$ cat loremipsum5.sha256 
9ef5c1ad6b4f48aa6115c36640890fe2e604547949b5dc82927a341dc8c301ab
```

Alternativa 2:
```
$ openssl dgst -sha256 -hex loremipsum5.txt | awk '{print $2}' 1>loremipsum5.sha256 2>/dev/null
$ cat loremipsum5.sha256 
9ef5c1ad6b4f48aa6115c36640890fe2e604547949b5dc82927a341dc8c301ab
```


* Ciframos con la `clave publica RSA` usando `openssl rsautl` y luego desciframos con la `clave privada RSA`:

```
$ openssl rsautl -encrypt -inkey rsapubkey01b_pkcs8.pem -pubin -in loremipsum5.sha256 -out loremipsum5_sha256.crypted
$ xxd -c 16 loremipsum5_sha256.crypted 
00000000: 243b c09d 97e7 16fd 0161 ae45 ea98 02bb  $;.......a.E....
00000010: 5445 cef7 6dc5 38f2 73d8 a0f7 06bc bf55  TE..m.8.s......U
00000020: 2d8e 0e2c a2a9 0ac4 d8f6 6d00 909e 683c  -..,......m...h<
00000030: 2ef8 7d96 55d2 ddbd b627 6d45 790e 88b0  ..}.U....'mEy...
00000040: ccc9 7d48 fcd0 72e6 5c0a adaf 013c 1328  ..}H..r.\....<.(
00000050: fa87 d2f1 f8a3 42ea 4930 6f27 a900 de26  ......B.I0o'...&
00000060: 7630 3fe6 32a1 31aa 7cad 8be6 b779 064c  v0?.2.1.|....y.L
00000070: 9d6b 7c0e 1926 8f92 ae6c dd05 9841 c18e  .k|..&...l...A..
00000080: 111e 622f 3f6c 46eb 8415 923a 0956 d7fa  ..b/?lF....:.V..
00000090: 808d 30e8 3557 6b91 a100 1b25 8b6d eb15  ..0.5Wk....%.m..
000000a0: d205 fdc1 d37a ea8e 4dd7 d962 d4c9 27d3  .....z..M..b..'.
000000b0: e439 e252 f927 a44c d9f4 3f24 9c2d 7521  .9.R.'.L..?$.-u!
000000c0: cd06 7a05 8aad 2f7d 145d 7a60 4e2e 4e2f  ..z.../}.]z`N.N/
000000d0: 3918 1d04 baf7 1c04 06cf b55f 07b8 9d63  9.........._...c
000000e0: 088c 3232 5ebc cdf2 51fa 55ce bca2 953a  ..22^...Q.U....:
000000f0: 19f7 7d2d ff7b e469 bc0d c336 6d8a a856  ..}-.{.i...6m..V
$ openssl rsautl -decrypt -inkey rsakeypair01.pem -in loremipsum5_sha256.crypted -out loremipsum5_sha256.clear
$ cat loremipsum5_sha256.clear 
9ef5c1ad6b4f48aa6115c36640890fe2e604547949b5dc82927a341dc8c301ab
$ cat loremipsum5.sha256
9ef5c1ad6b4f48aa6115c36640890fe2e604547949b5dc82927a341dc8c301ab
```


