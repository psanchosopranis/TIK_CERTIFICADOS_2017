# Lab 02 DSA y EC Key Pairs 
##### Apuntes para TIK sobre certificados (Dic 2017)

### TOOLS

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

>[Openssl](https://www.openssl.org/)

>[DumpASN1 - ASN.1 object dump/syntax check program](http://manpages.ubuntu.com/manpages/xenial/man1/dumpasn1.1.html) del autor [Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/)

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

### Preparación del laboratorio

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv labs/lab02_dsa_ec
mkdir: se ha creado el directorio 'labs/lab02_dsa_ec'
$ cd labs/lab02_dsa_ec
$ pwd
/home/devel1/labs/lab02_dsa_ec

```
## FUENTES DE DOCUMENTACION:

* DSA - Digital Signature Algorithm:

    * [FIPS PUB 186-4 - Digital Signature Standard (DSS) - Ver Apartado 4 The Digital Signature Algorithm (DSA)](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf) de [NIST - National Institute of Standards And Technology](https://www.nist.gov/)

    * [DSA - Digital Signature Algorithm -  (es)](https://es.wikipedia.org/wiki/DSA)

    * [DSA - Digital Signature Algorithm - Wikipedia (en)](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm)


* ECC - Elliptic Curve Cryptography

    * [La Criptografía de Curva Elíptica (del inglés: Elliptic curve cryptography, ECC) - Wikipedia (es)](https://es.wikipedia.org/wiki/Criptograf%C3%ADa_de_curva_el%C3%ADptica)

    * [Elliptic-curve cryptography - Wikipedia (en)](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography)

    * Recommended Elliptic Curves for Federal Government Use [Ver: FIPS PUB 186-4 - Digital Signature Standard (DSS) - Appendix D: Recommended Elliptic Curves for Federal Government Use](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf) de [NIST - National Institute of Standards And Technology](https://www.nist.gov/)

    * [FIPS PUB 186-4 - Digital Signature Standard (DSS) - Apartado 6. The Elliptic Curve Digital Signature Algorithm (ECDSA)](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf) de [NIST - National Institute of Standards And Technology](https://www.nist.gov/)

    * [A (relatively easy to understand) primer on elliptic curve cryptography](https://arstechnica.com/information-technology/2013/10/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography/)

    * [ANDREA CORBELLINI - Elliptic Curve Cryptography: a gentle introduction](http://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/)

    * [An Elliptic Curve Cryptography (ECC) Primer. Why ECC is the next generation of public key cryptography. (The Certicom ‘Catch the Curve’ White Paper Series - June 2004)](https://www.certicom.com/content/dam/certicom/images/pdfs/WP-ECCprimer.pdf)

    * ECC en OpenSSL
        * [Elliptic Curve Cryptography](https://wiki.openssl.org/index.php/Elliptic_Curve_Cryptography)
        * [Command Line Elliptic Curve Operations](https://wiki.openssl.org/index.php/Command_Line_Elliptic_Curve_Operations)

### Sección 01. Algunos ejemplos `DSA` con `openssl`

#### 01.01 Generación de `parejas de claves` (Keypair) de `DSA`

La generación de `parejas de claves` (Keypair) de `DSA` transcurre en dos pasos:

#### 1. Generación de parámetros 

```
$ openssl dsaparam --help
unknown option --help
dsaparam [options] [bits] <infile >outfile
where options are
 -inform arg   input format - DER or PEM
 -outform arg  output format - DER or PEM
 -in arg       input file
 -out arg      output file
 -text         print as text
 -C            Output C code
 -noout        no output
 -genkey       generate a DSA key
 -rand         files to use for random number input
 -engine e     use engine e, possibly a hardware device.
 number        number of bits to use for generating private key


$ openssl dsaparam -out dsaparam_01.pem -outform PEM 2048
Generating DSA parameters, 2048 bit long prime
This could take some time
..+.+....+.......+.............+++++++++++++++++++++++++++++++++++++++++++++++++++*
....+........+......+...........+...............+.+....+...+..................+..+........+.......+.+.......+.............+.............+.....+...+.+.+.....+...............+........+......+........+.........+.+..........+..+...........+............................+.+............+..........................................+.+....+...+..+....+...........+......+.+..+.....+...............+....+....+.............+........................+..........+.......................+.+.....+.....+++++++++++++++++++++++++++++++++++++++++++++++++++*
```

* visualización:

![DSA Param](images/DSA_Params_01_image01.png "DSA Param")

```
$ openssl dsaparam -in dsaparam_01.pem -inform PEM -text
    P:   
        00:fc:0b:b0:76:17:17:f9:82:df:dd:9b:65:d0:5b:
        c1:44:37:0c:36:2e:9f:db:20:61:06:6f:6d:d8:5b:
        cb:23:b1:53:1e:6a:69:31:d5:60:46:c5:96:cd:01:
        05:ce:88:a4:79:5d:e4:7d:b5:53:98:f3:a1:31:15:
        b1:a4:6b:0f:3b:cf:a0:7d:c4:bc:ec:64:7f:77:04:
        83:88:26:fd:6e:2c:98:83:0e:4d:27:fb:7e:0f:14:
        d5:30:8e:11:36:40:f1:f7:32:0c:22:fa:91:a1:19:
        07:c6:26:b5:07:27:e6:13:98:7b:6d:08:28:42:cf:
        98:8f:8a:36:f8:c0:d6:59:f7:35:ba:4d:8d:27:f5:
        df:38:db:f1:47:c3:a1:ed:b4:fd:bf:e2:ba:0c:10:
        6f:e3:af:c4:1c:49:d0:d7:12:c4:4a:06:21:48:bc:
        20:cb:ca:8e:ce:d4:ba:c0:19:82:1c:0b:c4:01:dc:
        85:76:50:97:78:86:c7:ae:55:1c:4f:49:63:08:bc:
        9b:4e:36:b4:5d:06:95:1e:60:88:c4:2a:55:29:69:
        a1:2a:e4:68:bb:6d:b2:95:b1:18:92:2b:be:ae:75:
        57:b5:18:98:4d:7a:c1:4a:b5:a8:63:d9:1d:2c:a8:
        c0:23:9b:8d:2a:57:3a:77:ad:29:5e:15:1c:5f:1c:
        57:ab
    Q:   
        00:ec:8c:6e:20:61:de:d2:c0:8f:e0:c4:f6:bd:64:
        8d:d2:49:ad:77:7e:62:5b:4a:ae:53:e6:6b:d8:ed:
        59:6a:4f
    G:   
        6a:cc:d9:80:06:63:8e:85:d5:47:bd:b6:ca:41:d9:
        52:c1:cb:6b:39:54:51:ff:fd:f5:1e:e1:ab:ee:ea:
        9a:0c:a2:50:42:3a:0e:c3:e6:74:8b:fa:79:f6:ce:
        8d:ac:de:22:5b:a1:a8:32:3b:1d:06:06:32:0b:4f:
        80:50:c5:a1:9d:37:2e:34:9a:be:1a:cd:6f:19:66:
        8e:6a:3e:6a:90:ff:ea:c7:85:51:a1:c1:b2:b3:b2:
        6a:33:8e:39:ca:6f:c3:58:c2:20:44:02:02:3b:a6:
        64:72:78:a1:30:d3:5b:78:f4:21:a8:45:ce:dd:62:
        13:9f:07:b2:d1:4e:6a:ae:9c:78:ec:05:8b:54:22:
        6d:40:a7:10:d5:50:91:48:ea:9f:3f:b5:45:e1:42:
        6e:51:d0:62:96:9b:47:d4:4d:d1:d9:7f:44:b7:ce:
        15:d0:2e:dd:00:34:0a:93:17:0b:5b:4b:40:e5:a8:
        90:cb:82:68:47:d0:b7:cd:f3:4b:25:e0:1e:27:d0:
        ac:6e:4d:d3:6d:8d:d9:53:d4:6d:5a:fb:01:77:9a:
        dc:50:36:74:f2:51:d5:29:8e:6a:61:1e:f7:ed:d6:
        47:c3:7a:93:dd:fd:66:a2:fb:98:79:cb:d6:f1:d6:
        2b:73:85:52:4f:b0:1e:4e:55:ac:51:3b:2b:c7:60:
        b6
-----BEGIN DSA PARAMETERS-----
MIICLAKCAQEA/AuwdhcX+YLf3Ztl0FvBRDcMNi6f2yBhBm9t2FvLI7FTHmppMdVg
RsWWzQEFzoikeV3kfbVTmPOhMRWxpGsPO8+gfcS87GR/dwSDiCb9biyYgw5NJ/t+
DxTVMI4RNkDx9zIMIvqRoRkHxia1ByfmE5h7bQgoQs+Yj4o2+MDWWfc1uk2NJ/Xf
ONvxR8Oh7bT9v+K6DBBv46/EHEnQ1xLESgYhSLwgy8qOztS6wBmCHAvEAdyFdlCX
eIbHrlUcT0ljCLybTja0XQaVHmCIxCpVKWmhKuRou22ylbEYkiu+rnVXtRiYTXrB
SrWoY9kdLKjAI5uNKlc6d60pXhUcXxxXqwIhAOyMbiBh3tLAj+DE9r1kjdJJrXd+
YltKrlPma9jtWWpPAoIBAGrM2YAGY46F1Ue9tspB2VLBy2s5VFH//fUe4avu6poM
olBCOg7D5nSL+nn2zo2s3iJboagyOx0GBjILT4BQxaGdNy40mr4azW8ZZo5qPmqQ
/+rHhVGhwbKzsmozjjnKb8NYwiBEAgI7pmRyeKEw01t49CGoRc7dYhOfB7LRTmqu
nHjsBYtUIm1ApxDVUJFI6p8/tUXhQm5R0GKWm0fUTdHZf0S3zhXQLt0ANAqTFwtb
S0DlqJDLgmhH0LfN80sl4B4n0KxuTdNtjdlT1G1a+wF3mtxQNnTyUdUpjmphHvft
1kfDepPd/Wai+5h5y9bx1itzhVJPsB5OVaxROyvHYLY=
-----END DSA PARAMETERS-----


$ openssl asn1parse -i -dump -in dsaparam_01.pem -inform PEM
    0:d=0  hl=4 l= 556 cons: SEQUENCE          
    4:d=1  hl=4 l= 257 prim:  INTEGER           :FC0BB0761717F982DFDD9B65D05BC144370C362E9FDB2061066F6DD85BCB23B1531E6A6931D56046C596CD0105CE88A4795DE47DB55398F3A13115B1A46B0F3BCFA07DC4BCEC647F7704838826FD6E2C98830E4D27FB7E0F14D5308E113640F1F7320C22FA91A11907C626B50727E613987B6D082842CF988F8A36F8C0D659F735BA4D8D27F5DF38DBF147C3A1EDB4FDBFE2BA0C106FE3AFC41C49D0D712C44A062148BC20CBCA8ECED4BAC019821C0BC401DC857650977886C7AE551C4F496308BC9B4E36B45D06951E6088C42A552969A12AE468BB6DB295B118922BBEAE7557B518984D7AC14AB5A863D91D2CA8C0239B8D2A573A77AD295E151C5F1C57AB
  265:d=1  hl=2 l=  33 prim:  INTEGER           :EC8C6E2061DED2C08FE0C4F6BD648DD249AD777E625B4AAE53E66BD8ED596A4F
  300:d=1  hl=4 l= 256 prim:  INTEGER           :6ACCD98006638E85D547BDB6CA41D952C1CB6B395451FFFDF51EE1ABEEEA9A0CA250423A0EC3E6748BFA79F6CE8DACDE225BA1A8323B1D0606320B4F8050C5A19D372E349ABE1ACD6F19668E6A3E6A90FFEAC78551A1C1B2B3B26A338E39CA6FC358C2204402023BA6647278A130D35B78F421A845CEDD62139F07B2D14E6AAE9C78EC058B54226D40A710D5509148EA9F3FB545E1426E51D062969B47D44DD1D97F44B7CE15D02EDD00340A93170B5B4B40E5A890CB826847D0B7CDF34B25E01E27D0AC6E4DD36D8DD953D46D5AFB01779ADC503674F251D5298E6A611EF7EDD647C37A93DDFD66A2FB9879CBD6F1D62B7385524FB01E4E55AC513B2BC760B6

```

#### 2. Generación del `Keypair DSA` a partir de los parámetros generados en el paso anterior:

```
$ openssl gendsa --help
usage: gendsa [args] dsaparam-file
 -out file - output the key to 'file'
 -des      - encrypt the generated key with DES in cbc mode
 -des3     - encrypt the generated key with DES in ede cbc mode (168 bit key)
 -seed
                 encrypt PEM output with cbc seed
 -aes128, -aes192, -aes256
                 encrypt PEM output with cbc aes
 -camellia128, -camellia192, -camellia256
                 encrypt PEM output with cbc camellia
 -engine e - use engine e, possibly a hardware device.
 -rand file:file:...
           - load the file (or the files in the directory) into
             the random number generator
 dsaparam-file
           - a DSA parameter file as generated by the dsaparam command

$ openssl gendsa -des3 -out dsaprivatekey01.pem dsaparam_01.pem 
Generating DSA key, 2048 bits
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:


$ openssl gendsa -des3 -out dsaprivatekey01.pem dsaparam_01.pem -passout pass:changeit
Generating DSA key, 2048 bits
```

* visualización:

```
-----BEGIN DSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,7196577DBB333E27

+kCtYyrp161HMFAtigTIbCbwEtFcp7LEsHZjj1+ANaBX7qdLI5+Cod20F+kRaT5A
XfAlOmd5gTf3PUHcbxJ2rgR7se6YM6tIsT6IR31Qw0hyB2Q+1hpEuSg25b+7hMGW
CewC4LDopFzgOZNWMm20ivk9r4+3CJ1zV7ZuNA2ceRGJsqcrnqoW0VZCqoeJH/bG
E8N979OqgLkihMLLVvyAZkxlWImkhwRc0XOP7FcW6gdHoZjSUlS1FnNRcEtWaoFt
N4Qing+8EzZNwQBcEZ1M266ibXoTEa0fACxNrwcDLNoev0JkuJgVl5vkGpQmtejn
X8OOCPy5fKq0J/8VVVv5qFPYUrkHSRzAa1nrZe8hPGeg/RqWIIGIB8sbWcOnzcX+
b+Z2FHv1aJr8DX2cGPrh4CM2FD1NLVghQcaXKKkHK8OVMLmAcMw+WoMmM3tCPk1G
FMF/CdGwfpuWOCT3R8wJZA/6AEeJuuOzqWZvKkGtaLXpKBx9x9/lUxXsjPawETMs
ZxjckPkddJYBaE2lIR4dYJHsXwfMTqH0GRD0ANAKrY3zwmlKOVGoRrUOvnLXNAmf
Y/geD3NKOb+DQQvfdCM9v5W8JKWjd/2lTMsJb41V8Ksh55BFLmmRcpLVQvl3JcMI
UdAoRLXQ2ySx69nUGa7lyRSQUotbzFgngXa+aM1PVdyNsCneCUHxZIhoksNSVilo
RevdBTeeFMbNImswWCWSTv+Yrdt3c+g4nOmWLJouHnpj3t55TyX2wIUH/2Q/4/q/
KLHixAJYYyNMTT6YguQEDsYySjoZThdG74rSqMOo04CpHRqDC+niPoW13g6/q60N
XLr0/hxTqEHMckZqrvWSM+1HyPmvTCqfmoGQ4Tm/XmgdSPBzRrqO6Fon72ZlBOnE
ogxsZReBu1K+p4Vqo0u2iZZKPMkqp2IIqlEvHhgYNmeaEbnHefj9kjtFk0yblyYG
17eJcaz1IvA61yhDvuP/PAwG3ifPJNwSgcwfBPFFFfr343BdijHC54B7dAnm4/U5
ZaSPGqqgowbb5G16miBfUmtXwneuF5Ox9NqWK12VEL6YXFFf2/EgTrgizvN4fRaT
/BimFT3A0bYTuYaGQqU8vGVE/wSkDrZky7JksD5yJ7MhzDYqkxMjVeG5bGuGDYaP
-----END DSA PRIVATE KEY-----

$ openssl dsa -in dsaprivatekey01.pem -inform PEM -passin pass:changeit -text
read DSA key
Private-Key: (2048 bit)
priv:
    00:d2:75:ad:d7:95:a3:cc:66:15:ea:02:cf:05:30:
    55:b2:f5:11:f0:64:47:0f:68:37:ad:d6:9e:0e:b2:
    56:cf:1c
pub: 
    4f:7a:57:e0:44:46:ef:16:ef:16:48:a7:b2:f7:76:
    7c:a4:33:7f:46:45:2c:06:b9:af:52:88:91:29:f2:
    b9:25:a3:dd:03:c4:79:11:d5:3e:ea:50:8f:e5:72:
    7b:c7:a7:04:65:1a:b1:72:7e:a9:87:bb:47:06:f0:
    fd:b2:11:62:4b:2f:2c:d6:7f:dd:6c:65:f7:33:69:
    d9:dd:13:fa:9b:3d:b0:c6:3e:99:f7:87:d0:0f:ec:
    63:a2:80:38:03:d3:35:73:bb:86:62:7c:4e:42:a7:
    a5:bd:dc:b4:8f:80:86:71:20:40:87:f9:b3:51:1e:
    61:a3:ea:86:45:11:c3:75:b5:c4:31:3b:c6:0e:d0:
    ad:2e:a2:e1:7e:fc:11:b1:bc:e7:cd:f5:94:7c:8f:
    ed:98:e2:5c:26:0b:98:99:e6:dc:dd:8b:e4:b7:11:
    ec:da:ee:53:ef:28:2b:cc:3d:1b:45:5c:37:60:42:
    df:4a:79:0d:a7:96:11:9b:71:f2:55:0f:38:60:3a:
    e2:39:f5:48:c5:40:ec:d0:bd:cd:43:c7:fb:43:ff:
    ed:95:9e:66:90:7c:d4:6c:59:97:a7:8e:17:a0:9f:
    68:cc:ee:1b:7f:db:ba:47:00:23:ff:ca:c2:c9:16:
    4c:3e:0c:67:3d:76:a6:b2:da:24:0f:97:4c:49:a6:
    ec
P:   
    00:fc:0b:b0:76:17:17:f9:82:df:dd:9b:65:d0:5b:
    c1:44:37:0c:36:2e:9f:db:20:61:06:6f:6d:d8:5b:
    cb:23:b1:53:1e:6a:69:31:d5:60:46:c5:96:cd:01:
    05:ce:88:a4:79:5d:e4:7d:b5:53:98:f3:a1:31:15:
    b1:a4:6b:0f:3b:cf:a0:7d:c4:bc:ec:64:7f:77:04:
    83:88:26:fd:6e:2c:98:83:0e:4d:27:fb:7e:0f:14:
    d5:30:8e:11:36:40:f1:f7:32:0c:22:fa:91:a1:19:
    07:c6:26:b5:07:27:e6:13:98:7b:6d:08:28:42:cf:
    98:8f:8a:36:f8:c0:d6:59:f7:35:ba:4d:8d:27:f5:
    df:38:db:f1:47:c3:a1:ed:b4:fd:bf:e2:ba:0c:10:
    6f:e3:af:c4:1c:49:d0:d7:12:c4:4a:06:21:48:bc:
    20:cb:ca:8e:ce:d4:ba:c0:19:82:1c:0b:c4:01:dc:
    85:76:50:97:78:86:c7:ae:55:1c:4f:49:63:08:bc:
    9b:4e:36:b4:5d:06:95:1e:60:88:c4:2a:55:29:69:
    a1:2a:e4:68:bb:6d:b2:95:b1:18:92:2b:be:ae:75:
    57:b5:18:98:4d:7a:c1:4a:b5:a8:63:d9:1d:2c:a8:
    c0:23:9b:8d:2a:57:3a:77:ad:29:5e:15:1c:5f:1c:
    57:ab
Q:   
    00:ec:8c:6e:20:61:de:d2:c0:8f:e0:c4:f6:bd:64:
    8d:d2:49:ad:77:7e:62:5b:4a:ae:53:e6:6b:d8:ed:
    59:6a:4f
G:   
    6a:cc:d9:80:06:63:8e:85:d5:47:bd:b6:ca:41:d9:
    52:c1:cb:6b:39:54:51:ff:fd:f5:1e:e1:ab:ee:ea:
    9a:0c:a2:50:42:3a:0e:c3:e6:74:8b:fa:79:f6:ce:
    8d:ac:de:22:5b:a1:a8:32:3b:1d:06:06:32:0b:4f:
    80:50:c5:a1:9d:37:2e:34:9a:be:1a:cd:6f:19:66:
    8e:6a:3e:6a:90:ff:ea:c7:85:51:a1:c1:b2:b3:b2:
    6a:33:8e:39:ca:6f:c3:58:c2:20:44:02:02:3b:a6:
    64:72:78:a1:30:d3:5b:78:f4:21:a8:45:ce:dd:62:
    13:9f:07:b2:d1:4e:6a:ae:9c:78:ec:05:8b:54:22:
    6d:40:a7:10:d5:50:91:48:ea:9f:3f:b5:45:e1:42:
    6e:51:d0:62:96:9b:47:d4:4d:d1:d9:7f:44:b7:ce:
    15:d0:2e:dd:00:34:0a:93:17:0b:5b:4b:40:e5:a8:
    90:cb:82:68:47:d0:b7:cd:f3:4b:25:e0:1e:27:d0:
    ac:6e:4d:d3:6d:8d:d9:53:d4:6d:5a:fb:01:77:9a:
    dc:50:36:74:f2:51:d5:29:8e:6a:61:1e:f7:ed:d6:
    47:c3:7a:93:dd:fd:66:a2:fb:98:79:cb:d6:f1:d6:
    2b:73:85:52:4f:b0:1e:4e:55:ac:51:3b:2b:c7:60:
    b6
writing DSA key
-----BEGIN DSA PRIVATE KEY-----
MIIDVgIBAAKCAQEA/AuwdhcX+YLf3Ztl0FvBRDcMNi6f2yBhBm9t2FvLI7FTHmpp
MdVgRsWWzQEFzoikeV3kfbVTmPOhMRWxpGsPO8+gfcS87GR/dwSDiCb9biyYgw5N
J/t+DxTVMI4RNkDx9zIMIvqRoRkHxia1ByfmE5h7bQgoQs+Yj4o2+MDWWfc1uk2N
J/XfONvxR8Oh7bT9v+K6DBBv46/EHEnQ1xLESgYhSLwgy8qOztS6wBmCHAvEAdyF
dlCXeIbHrlUcT0ljCLybTja0XQaVHmCIxCpVKWmhKuRou22ylbEYkiu+rnVXtRiY
TXrBSrWoY9kdLKjAI5uNKlc6d60pXhUcXxxXqwIhAOyMbiBh3tLAj+DE9r1kjdJJ
rXd+YltKrlPma9jtWWpPAoIBAGrM2YAGY46F1Ue9tspB2VLBy2s5VFH//fUe4avu
6poMolBCOg7D5nSL+nn2zo2s3iJboagyOx0GBjILT4BQxaGdNy40mr4azW8ZZo5q
PmqQ/+rHhVGhwbKzsmozjjnKb8NYwiBEAgI7pmRyeKEw01t49CGoRc7dYhOfB7LR
TmqunHjsBYtUIm1ApxDVUJFI6p8/tUXhQm5R0GKWm0fUTdHZf0S3zhXQLt0ANAqT
FwtbS0DlqJDLgmhH0LfN80sl4B4n0KxuTdNtjdlT1G1a+wF3mtxQNnTyUdUpjmph
Hvft1kfDepPd/Wai+5h5y9bx1itzhVJPsB5OVaxROyvHYLYCggEAT3pX4ERG7xbv
Fkinsvd2fKQzf0ZFLAa5r1KIkSnyuSWj3QPEeRHVPupQj+Vye8enBGUasXJ+qYe7
Rwbw/bIRYksvLNZ/3Wxl9zNp2d0T+ps9sMY+mfeH0A/sY6KAOAPTNXO7hmJ8TkKn
pb3ctI+AhnEgQIf5s1EeYaPqhkURw3W1xDE7xg7QrS6i4X78EbG85831lHyP7Zji
XCYLmJnm3N2L5LcR7NruU+8oK8w9G0VcN2BC30p5DaeWEZtx8lUPOGA64jn1SMVA
7NC9zUPH+0P/7ZWeZpB81GxZl6eOF6CfaMzuG3/bukcAI//KwskWTD4MZz12prLa
JA+XTEmm7AIhANJ1rdeVo8xmFeoCzwUwVbL1EfBkRw9oN63Wng6yVs8c
-----END DSA PRIVATE KEY-----

```

#### 01.02 cambios de formato, extracción de la clave pública ...

* Obtención clave pública en formato DER:

```
$ openssl dsa -in dsaprivatekey01.pem -inform PEM -pubout -passin pass:changeit -out dsapublickey01.der -outform DER
```

* visualización:

```
$ openssl dsa -in dsapublickey01.der -inform DER -pubin 
read DSA key
writing DSA key
-----BEGIN PUBLIC KEY-----
MIIDRjCCAjkGByqGSM44BAEwggIsAoIBAQD8C7B2Fxf5gt/dm2XQW8FENww2Lp/b
IGEGb23YW8sjsVMeamkx1WBGxZbNAQXOiKR5XeR9tVOY86ExFbGkaw87z6B9xLzs
ZH93BIOIJv1uLJiDDk0n+34PFNUwjhE2QPH3Mgwi+pGhGQfGJrUHJ+YTmHttCChC
z5iPijb4wNZZ9zW6TY0n9d842/FHw6HttP2/4roMEG/jr8QcSdDXEsRKBiFIvCDL
yo7O1LrAGYIcC8QB3IV2UJd4hseuVRxPSWMIvJtONrRdBpUeYIjEKlUpaaEq5Gi7
bbKVsRiSK76udVe1GJhNesFKtahj2R0sqMAjm40qVzp3rSleFRxfHFerAiEA7Ixu
IGHe0sCP4MT2vWSN0kmtd35iW0quU+Zr2O1Zak8CggEAaszZgAZjjoXVR722ykHZ
UsHLazlUUf/99R7hq+7qmgyiUEI6DsPmdIv6efbOjazeIluhqDI7HQYGMgtPgFDF
oZ03LjSavhrNbxlmjmo+apD/6seFUaHBsrOyajOOOcpvw1jCIEQCAjumZHJ4oTDT
W3j0IahFzt1iE58HstFOaq6ceOwFi1QibUCnENVQkUjqnz+1ReFCblHQYpabR9RN
0dl/RLfOFdAu3QA0CpMXC1tLQOWokMuCaEfQt83zSyXgHifQrG5N022N2VPUbVr7
AXea3FA2dPJR1SmOamEe9+3WR8N6k939ZqL7mHnL1vHWK3OFUk+wHk5VrFE7K8dg
tgOCAQUAAoIBAE96V+BERu8W7xZIp7L3dnykM39GRSwGua9SiJEp8rklo90DxHkR
1T7qUI/lcnvHpwRlGrFyfqmHu0cG8P2yEWJLLyzWf91sZfczadndE/qbPbDGPpn3
h9AP7GOigDgD0zVzu4ZifE5Cp6W93LSPgIZxIECH+bNRHmGj6oZFEcN1tcQxO8YO
0K0uouF+/BGxvOfN9ZR8j+2Y4lwmC5iZ5tzdi+S3Eeza7lPvKCvMPRtFXDdgQt9K
eQ2nlhGbcfJVDzhgOuI59UjFQOzQvc1Dx/tD/+2VnmaQfNRsWZenjhegn2jM7ht/
27pHACP/ysLJFkw+DGc9dqay2iQPl0xJpuw=
-----END PUBLIC KEY-----
```

![DSA Public key](images/DSA_Pubkey_01_image01.png "DSA Public key")

>[Reference record for OID 1.2.840.10040.4.1](http://oidref.com/1.2.840.10040.4.1) _'Digital Signature Algorithm (DSA) subject public key. Defined in National Institute of Standards and Technology (NIST) FIPS 186: Digital Signature Standard. 19 May 1994. See also IETF RFC 3370, section 3.1.'_

>[Ver OID 1.2.840.113549.1.1.1 en oid-info.com](http://oid-info.com/get/1.2.840.10040.4.1)  _'Digital Signature Algorithm (DSA) subject public key. Defined in National Institute of Standards and Technology (NIST) FIPS 186: Digital Signature Standard. 19 May 1994. See also IETF RFC 3370, section 3.1.'_

* Cambio a formato estandar `PKCS#8` SIN PROTECCION DE CLAVE:

```
$ openssl pkcs8 -in dsaprivatekey01.pem -inform PEM -passin pass:changeit -topk8 -nocrypt -out dsaprivatekey01_pkcs8.der -outform DER
```
![DSA Private key PKCS#8](images/DSA_Privatekey_PKCS8_image01.png "DSA Private key PKCS#8")

* Cambio a formato estandar `PKCS#8` CON PROTECCION DE CLAVE:

```
$ openssl pkcs8 -in dsaprivatekey01.pem -inform PEM -passin pass:changeit -topk8 -out dsaprivatekey01_pkcs8.der -outform DER -passout pass:changeit
```

![DSA Private key PKCS#8 DES3+PBE Protected](images/DSA_Privatekey_PKCS8_PBE_image01.png "DES3+PBE Protected")

>[Reference record for OID 1.2.840.113549.1.5.3](http://oidref.com/1.2.840.113549.1.5.3) _'Password Based Encryption Algorithm. Uses Data Encryption Standard in Cipher Block Chaining Mode (DES-CBC). Uses MD5 to hash a password & salt to get Key and IV. Defined in RSA's PKCS#5'_

>[Ver OID 1.2.840.113549.1.5.3 en oid-info.com](http://oid-info.com/get/1.2.840.113549.1.5.3)  _'Password-based encryption algorithm using Data Encryption Standard in Cipher Block Chaining mode (DES-CBC) and Message Digest 5 (MD5). See IETF RFC 8018. '_

### Sección 02. Algunos ejemplos `EC - Elliptic curve`  con `openssl`

#### 02.01 Generación de `parejas de claves` (Keypair) de `EC`

La generación de `parejas de claves` (Keypair) de `EC` transcurre en dos pasos:

#### 1a. EC Parámetros: Listado de curvas 'semilla' disponibles: 

```
$ openssl ecparam -list_curves
  secp112r1 : SECG/WTLS curve over a 112 bit prime field
  secp112r2 : SECG curve over a 112 bit prime field
  secp128r1 : SECG curve over a 128 bit prime field
  secp128r2 : SECG curve over a 128 bit prime field
  secp160k1 : SECG curve over a 160 bit prime field
  secp160r1 : SECG curve over a 160 bit prime field
  secp160r2 : SECG/WTLS curve over a 160 bit prime field
  secp192k1 : SECG curve over a 192 bit prime field
  secp224k1 : SECG curve over a 224 bit prime field
  secp224r1 : NIST/SECG curve over a 224 bit prime field
  secp256k1 : SECG curve over a 256 bit prime field
  secp384r1 : NIST/SECG curve over a 384 bit prime field
  secp521r1 : NIST/SECG curve over a 521 bit prime field
  prime192v1: NIST/X9.62/SECG curve over a 192 bit prime field
  prime192v2: X9.62 curve over a 192 bit prime field
  prime192v3: X9.62 curve over a 192 bit prime field
  prime239v1: X9.62 curve over a 239 bit prime field
  prime239v2: X9.62 curve over a 239 bit prime field
  prime239v3: X9.62 curve over a 239 bit prime field
  prime256v1: X9.62/SECG curve over a 256 bit prime field
  sect113r1 : SECG curve over a 113 bit binary field
  sect113r2 : SECG curve over a 113 bit binary field
  sect131r1 : SECG/WTLS curve over a 131 bit binary field
  sect131r2 : SECG curve over a 131 bit binary field
  sect163k1 : NIST/SECG/WTLS curve over a 163 bit binary field
  sect163r1 : SECG curve over a 163 bit binary field
  sect163r2 : NIST/SECG curve over a 163 bit binary field
  sect193r1 : SECG curve over a 193 bit binary field
  sect193r2 : SECG curve over a 193 bit binary field
  sect233k1 : NIST/SECG/WTLS curve over a 233 bit binary field
  sect233r1 : NIST/SECG/WTLS curve over a 233 bit binary field
  sect239k1 : SECG curve over a 239 bit binary field
  sect283k1 : NIST/SECG curve over a 283 bit binary field
  sect283r1 : NIST/SECG curve over a 283 bit binary field
  sect409k1 : NIST/SECG curve over a 409 bit binary field
  sect409r1 : NIST/SECG curve over a 409 bit binary field
  sect571k1 : NIST/SECG curve over a 571 bit binary field
  sect571r1 : NIST/SECG curve over a 571 bit binary field
  c2pnb163v1: X9.62 curve over a 163 bit binary field
  c2pnb163v2: X9.62 curve over a 163 bit binary field
  c2pnb163v3: X9.62 curve over a 163 bit binary field
  c2pnb176v1: X9.62 curve over a 176 bit binary field
  c2tnb191v1: X9.62 curve over a 191 bit binary field
  c2tnb191v2: X9.62 curve over a 191 bit binary field
  c2tnb191v3: X9.62 curve over a 191 bit binary field
  c2pnb208w1: X9.62 curve over a 208 bit binary field
  c2tnb239v1: X9.62 curve over a 239 bit binary field
  c2tnb239v2: X9.62 curve over a 239 bit binary field
  c2tnb239v3: X9.62 curve over a 239 bit binary field
  c2pnb272w1: X9.62 curve over a 272 bit binary field
  c2pnb304w1: X9.62 curve over a 304 bit binary field
  c2tnb359v1: X9.62 curve over a 359 bit binary field
  c2pnb368w1: X9.62 curve over a 368 bit binary field
  c2tnb431r1: X9.62 curve over a 431 bit binary field
  wap-wsg-idm-ecid-wtls1: WTLS curve over a 113 bit binary field
  wap-wsg-idm-ecid-wtls3: NIST/SECG/WTLS curve over a 163 bit binary field
  wap-wsg-idm-ecid-wtls4: SECG curve over a 113 bit binary field
  wap-wsg-idm-ecid-wtls5: X9.62 curve over a 163 bit binary field
  wap-wsg-idm-ecid-wtls6: SECG/WTLS curve over a 112 bit prime field
  wap-wsg-idm-ecid-wtls7: SECG/WTLS curve over a 160 bit prime field
  wap-wsg-idm-ecid-wtls8: WTLS curve over a 112 bit prime field
  wap-wsg-idm-ecid-wtls9: WTLS curve over a 160 bit prime field
  wap-wsg-idm-ecid-wtls10: NIST/SECG/WTLS curve over a 233 bit binary field
  wap-wsg-idm-ecid-wtls11: NIST/SECG/WTLS curve over a 233 bit binary field
  wap-wsg-idm-ecid-wtls12: WTLS curvs over a 224 bit prime field
  Oakley-EC2N-3: 
	IPSec/IKE/Oakley curve #3 over a 155 bit binary field.
	Not suitable for ECDSA.
	Questionable extension field!
  Oakley-EC2N-4: 
	IPSec/IKE/Oakley curve #4 over a 185 bit binary field.
	Not suitable for ECDSA.
	Questionable extension field!
  brainpoolP160r1: RFC 5639 curve over a 160 bit prime field
  brainpoolP160t1: RFC 5639 curve over a 160 bit prime field
  brainpoolP192r1: RFC 5639 curve over a 192 bit prime field
  brainpoolP192t1: RFC 5639 curve over a 192 bit prime field
  brainpoolP224r1: RFC 5639 curve over a 224 bit prime field
  brainpoolP224t1: RFC 5639 curve over a 224 bit prime field
  brainpoolP256r1: RFC 5639 curve over a 256 bit prime field
  brainpoolP256t1: RFC 5639 curve over a 256 bit prime field
  brainpoolP320r1: RFC 5639 curve over a 320 bit prime field
  brainpoolP320t1: RFC 5639 curve over a 320 bit prime field
  brainpoolP384r1: RFC 5639 curve over a 384 bit prime field
  brainpoolP384t1: RFC 5639 curve over a 384 bit prime field
  brainpoolP512r1: RFC 5639 curve over a 512 bit prime field
  brainpoolP512t1: RFC 5639 curve over a 512 bit prime field
```

#### 1b. EC Parámetros: generación de `parámetros EC` (`EC parameters`) a partir de una de las `curvas semilla`:

* generación:

```
$ openssl ecparam -name secp256k1 -out ecparams_secp256k1.pem
```

* visualización:

![EC Params](images/EC_params_secp256K1_image01.png "EC Params")

>[Reference record for OID 1.3.132.0.10](http://oidref.com/1.3.132.0.10) _'"SEC 2" recommended elliptic curve domain - secp256k1. Covers "secp256k1", the elliptic curve domain listed in "SEC 2: Recommended Elliptic Curve Domain Parameters". The SEC (Standards for Efficient Cryptography) curves provide elliptic curve domain parameters at commonly required security levels for use by implementers of ECC standards like ANSI X9.62, ANSI X9.63, IEEE P1363, and other standards. Information by oid_info:
[http://www.secg.org/sec2-v2.pdf](http://www.secg.org/sec2-v2.pdf)'_

>[Ver OID 1.3.132.0.10 en oid-info.com](http://oid-info.com/get/1.3.132.0.10)  _'SEC 2" recommended elliptic curve domain "ansip256k1"  See ["SEC 2: Recommended Elliptic Curve Domain Parameters".](http://www.secg.org/sec2-v2.pdf)'_

```
$ openssl ecparam -in ecparams_secp256k1.pem -inform PEM -text
ASN1 OID: secp256k1
-----BEGIN EC PARAMETERS-----
BgUrgQQACg==
-----END EC PARAMETERS-----
```

```
$ openssl ecparam -in ecparams_secp256k1.pem -inform PEM -text -noout
ASN1 OID: secp256k1

$ openssl ecparam -in ecparams_secp256k1.pem -inform PEM -text -param_enc explicit -noout
Field Type: prime-field
Prime:
    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:fe:ff:
    ff:fc:2f
A:    0
B:    7 (0x7)
Generator (uncompressed):
    04:79:be:66:7e:f9:dc:bb:ac:55:a0:62:95:ce:87:
    0b:07:02:9b:fc:db:2d:ce:28:d9:59:f2:81:5b:16:
    f8:17:98:48:3a:da:77:26:a3:c4:65:5d:a4:fb:fc:
    0e:11:08:a8:fd:17:b4:48:a6:85:54:19:9c:47:d0:
    8f:fb:10:d4:b8
Order: 
    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:fe:ba:ae:dc:e6:af:48:a0:3b:bf:d2:5e:8c:d0:
    36:41:41
Cofactor:  1 (0x1)
```

#### 2. Creación de un `Keypair EC` apartir de los `EC Parámetros`: 

```
$ openssl ecparam -in ecparams_secp256k1.pem -genkey -noout -out ec_privatekey_secp256k1-key.pem -outform PEM
```

O bien **DIRECTAMENTE** sin el uso de los `EC Parámetros` sino utilizando directamente el _**ACRONIMO**_ de la `curva semilla`:

```
$ openssl ecparam -name secp256k1 -genkey -noout -out ec_privatekey_secp256k1-key.der -outform DER
```

* visualización:

![EC Private Key](images/EC_privatekey_secp256k1-key-image01.png "EC Private Key")

```
$ dumpasn1 -adhl ec_privatekey_secp256k1-key.der
    <30 74>
  0 116: SEQUENCE {
    <02 01>
  2   1: . INTEGER 1
    <04 20>
  5  32: . OCTET STRING
       : . . C8 01 7D F2 43 EB 29 BF 9D A0 15 C4 53 5C C9 A7
       : . . 66 44 13 E2 49 97 F1 D8 39 84 48 40 35 8F 74 60
    <A0 07>
 39   7: . [0] {
    <06 05>
 41   5: . . OBJECT IDENTIFIER secp256k1 (1 3 132 0 10)
       : . . . (SECG (Certicom) named elliptic curve)
       : . . }
    <A1 44>
 48  68: . [1] {
    <03 42>
 50  66: . . BIT STRING
       : . . . 04 85 86 6E F2 DD 0A C0 24 60 2B 78 91 48 53 0B
       : . . . EA FF 2D 1D 46 2B BB 91 88 6D 27 F8 A2 EE A1 01
       : . . . B3 8C F3 6B 98 0F CA 93 C0 4B E8 87 C8 E4 64 F8
       : . . . 78 5D 2D 92 A6 9C 8B E3 AF 52 3D 51 58 87 9A 51
       : . . . F3
       : . . }
       : . }

0 warnings, 0 errors.


$ openssl asn1parse -i -dump -in ec_privatekey_secp256k1-key.der -inform DER
    0:d=0  hl=2 l= 116 cons: SEQUENCE          
    2:d=1  hl=2 l=   1 prim:  INTEGER           :01
    5:d=1  hl=2 l=  32 prim:  OCTET STRING      
      0000 - c8 01 7d f2 43 eb 29 bf-9d a0 15 c4 53 5c c9 a7   ..}.C.).....S\..
      0010 - 66 44 13 e2 49 97 f1 d8-39 84 48 40 35 8f 74 60   fD..I...9.H@5.t`
   39:d=1  hl=2 l=   7 cons:  cont [ 0 ]        
   41:d=2  hl=2 l=   5 prim:   OBJECT            :secp256k1
   48:d=1  hl=2 l=  68 cons:  cont [ 1 ]        
   50:d=2  hl=2 l=  66 prim:   BIT STRING        
      0000 - 00 04 85 86 6e f2 dd 0a-c0 24 60 2b 78 91 48 53   ....n....$`+x.HS
      0010 - 0b ea ff 2d 1d 46 2b bb-91 88 6d 27 f8 a2 ee a1   ...-.F+...m'....
      0020 - 01 b3 8c f3 6b 98 0f ca-93 c0 4b e8 87 c8 e4 64   ....k.....K....d
      0030 - f8 78 5d 2d 92 a6 9c 8b-e3 af 52 3d 51 58 87 9a   .x]-......R=QX..
      0040 - 51 f3                  
```


#### 01.02 cambios de formato, extracción de la clave pública ...

* Obtención clave pública en formato DER:

```
$ openssl ec -in ec_privatekey_secp256k1-key.der -inform DER -pubout -out ec_publickey_secp256k1.der -outform DER
read EC key
writing EC key
```

* visualización:

```
$ openssl ec -in ec_publickey_secp256k1.der -inform DER -pubin -text
read EC key
Private-Key: (256 bit)
pub: 
    04:85:86:6e:f2:dd:0a:c0:24:60:2b:78:91:48:53:
    0b:ea:ff:2d:1d:46:2b:bb:91:88:6d:27:f8:a2:ee:
    a1:01:b3:8c:f3:6b:98:0f:ca:93:c0:4b:e8:87:c8:
    e4:64:f8:78:5d:2d:92:a6:9c:8b:e3:af:52:3d:51:
    58:87:9a:51:f3
ASN1 OID: secp256k1
writing EC key
-----BEGIN PUBLIC KEY-----
MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEhYZu8t0KwCRgK3iRSFML6v8tHUYru5GI
bSf4ou6hAbOM82uYD8qTwEvoh8jkZPh4XS2SppyL469SPVFYh5pR8w==
-----END PUBLIC KEY-----

$ dumpasn1 -adhl ec_publickey_secp256k1.der 
    <30 56>
  0  86: SEQUENCE {
    <30 10>
  2  16: . SEQUENCE {
    <06 07>
  4   7: . . OBJECT IDENTIFIER ecPublicKey (1 2 840 10045 2 1)
       : . . . (ANSI X9.62 public key type)
    <06 05>
 13   5: . . OBJECT IDENTIFIER secp256k1 (1 3 132 0 10)
       : . . . (SECG (Certicom) named elliptic curve)
       : . . }
    <03 42>
 20  66: . BIT STRING
       : . . 04 85 86 6E F2 DD 0A C0 24 60 2B 78 91 48 53 0B
       : . . EA FF 2D 1D 46 2B BB 91 88 6D 27 F8 A2 EE A1 01
       : . . B3 8C F3 6B 98 0F CA 93 C0 4B E8 87 C8 E4 64 F8
       : . . 78 5D 2D 92 A6 9C 8B E3 AF 52 3D 51 58 87 9A 51
       : . . F3
       : . }

0 warnings, 0 errors.
```

![EC Public Key](images/EC_PublcKey_PKCS8_image01.png "EC Public Key")

>[Reference record for OID 1.2.840.10045.2.1](http://oidref.com/1.2.840.10045.2.1) _'Elliptic curve public key cryptography. See IETF RFC 3279 and RFC 5753.'_

>[Ver OID 1.2.840.10045.2.1 en oid-info.com](http://oid-info.com/get/1.2.840.10045.2.1)  _'Elliptic curve public key cryptography. See IETF RFC 3279 and RFC 5753.'_


* Cambio a formato estandar `PKCS#8` SIN PROTECCION DE CLAVE:

```
$ openssl pkcs8 -in ec_privatekey_secp256k1-key.der -inform DER -topk8 -nocrypt -out ec_privatekey_secp256k1_pkcs8.PEM -outform PEM

$ cat ec_privatekey_secp256k1_pkcs8.PEM
-----BEGIN PRIVATE KEY-----
MIGEAgEAMBAGByqGSM49AgEGBSuBBAAKBG0wawIBAQQgyAF98kPrKb+doBXEU1zJ
p2ZEE+JJl/HYOYRIQDWPdGChRANCAASFhm7y3QrAJGAreJFIUwvq/y0dRiu7kYht
J/ii7qEBs4zza5gPypPAS+iHyORk+HhdLZKmnIvjr1I9UViHmlHz
-----END PRIVATE KEY-----
```

* Visualización:

```
$ openssl ec -in ec_privatekey_secp256k1_pkcs8.PEM -inform PEM -text
read EC key
Private-Key: (256 bit)
priv:
    00:c8:01:7d:f2:43:eb:29:bf:9d:a0:15:c4:53:5c:
    c9:a7:66:44:13:e2:49:97:f1:d8:39:84:48:40:35:
    8f:74:60
pub: 
    04:85:86:6e:f2:dd:0a:c0:24:60:2b:78:91:48:53:
    0b:ea:ff:2d:1d:46:2b:bb:91:88:6d:27:f8:a2:ee:
    a1:01:b3:8c:f3:6b:98:0f:ca:93:c0:4b:e8:87:c8:
    e4:64:f8:78:5d:2d:92:a6:9c:8b:e3:af:52:3d:51:
    58:87:9a:51:f3
ASN1 OID: secp256k1
writing EC key
-----BEGIN EC PRIVATE KEY-----
MHQCAQEEIMgBffJD6ym/naAVxFNcyadmRBPiSZfx2DmESEA1j3RgoAcGBSuBBAAK
oUQDQgAEhYZu8t0KwCRgK3iRSFML6v8tHUYru5GIbSf4ou6hAbOM82uYD8qTwEvo
h8jkZPh4XS2SppyL469SPVFYh5pR8w==
-----END EC PRIVATE KEY-----

$ openssl asn1parse -i -dump -in ec_privatekey_secp256k1_pkcs8.PEM -inform PEM
    0:d=0  hl=3 l= 132 cons: SEQUENCE          
    3:d=1  hl=2 l=   1 prim:  INTEGER           :00
    6:d=1  hl=2 l=  16 cons:  SEQUENCE          
    8:d=2  hl=2 l=   7 prim:   OBJECT            :id-ecPublicKey
   17:d=2  hl=2 l=   5 prim:   OBJECT            :secp256k1
   24:d=1  hl=2 l= 109 prim:  OCTET STRING      
      0000 - 30 6b 02 01 01 04 20 c8-01 7d f2 43 eb 29 bf 9d   0k.... ..}.C.)..
      0010 - a0 15 c4 53 5c c9 a7 66-44 13 e2 49 97 f1 d8 39   ...S\..fD..I...9
      0020 - 84 48 40 35 8f 74 60 a1-44 03 42 00 04 85 86 6e   .H@5.t`.D.B....n
      0030 - f2 dd 0a c0 24 60 2b 78-91 48 53 0b ea ff 2d 1d   ....$`+x.HS...-.
      0040 - 46 2b bb 91 88 6d 27 f8-a2 ee a1 01 b3 8c f3 6b   F+...m'........k
      0050 - 98 0f ca 93 c0 4b e8 87-c8 e4 64 f8 78 5d 2d 92   .....K....d.x]-.
      0060 - a6 9c 8b e3 af 52 3d 51-58 87 9a 51 f3            .....R=QX..Q.

```

![EC Private Key NOCRYPT](images/EC_privatekey_secp256k1-pkcs8-image01.png "EC Private Key NOCRYPT")

* Cambio a formato estandar `PKCS#8` CON PROTECCION DE CLAVE:


```
$ openssl pkcs8 -in ec_privatekey_secp256k1-key.der -inform DER -topk8 -out ec_privatekey_secp256k1_pkcs8_PBE.pem -outform PEM -passout pass:changeit

$ cat ec_privatekey_secp256k1_pkcs8_PBE.pem
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIGoMBsGCSqGSIb3DQEFAzAOBAiVpdGgK3VRmQICCAAEgYh/qLLuBieBnLj9kIW6
2qjyxVSbxrtJwjJ1Q96qNkd5EUjOFoMZP0vhc3rpk1JcytIWvqLsqjf341o3SdAR
vabpHjCiC/05Iv56//VSeMLqiNX2otIbwp8CRBw/ysvXXuGRJbcP7ZMNb0aWyeFS
maAw2oaQkW4U+SuOnynGwW6JWNbsrMQNQiT6
-----END ENCRYPTED PRIVATE KEY-----
```

* Visualización:

![EC Private Key DES3+PBE](images/EC_privatekey_secp256k1-pkcs8-PBE-image01.png "EC Private Key DES3+PBE")

>[Reference record for OID 1.2.840.113549.1.5.3](http://oidref.com/1.2.840.113549.1.5.3) _'Password Based Encryption Algorithm. Uses Data Encryption Standard in Cipher Block Chaining Mode (DES-CBC). Uses MD5 to hash a password & salt to get Key and IV. Defined in RSA's PKCS#5'_

>[Ver OID 1.2.840.113549.1.5.3 en oid-info.com](http://oid-info.com/get/1.2.840.113549.1.5.3)  _'Password-based encryption algorithm using Data Encryption Standard in Cipher Block Chaining mode (DES-CBC) and Message Digest 5 (MD5). See IETF RFC 8018. '_


* Cambio de protección a `AES-256` con `PBE`

Paso 1:
```
$ openssl ec -in ec_privatekey_secp256k1-key.der -inform DER -aes256 -out ec_privatekey_secp256k1_PBE.pem -outform PEM -passout pass:changeit
read EC key
writing EC key

$ cat ec_privatekey_secp256k1_PBE.pem
-----BEGIN EC PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,22B040A5D2EED2F665F15DD49916962D

UnOCUonibnPKZl2h/vLSgLbv+gCK6NwU9RvwmeBf2adJYrA/u6ZqfDB7pDzEM6mD
1qyYaZDNzqQlhLVOjDHvQi/qdAG38PQOX0yCq0CHq9q8X24MDB8ZV2JpbJ9A2oQJ
Mip0vTADK92qBmduzrTilqyzlvITj66IEa10U1pG3eY=
-----END EC PRIVATE KEY-----
```

Paso 2:
```
$ openssl pkcs8 -in ec_privatekey_secp256k1_PBE.pem -inform PEM -passin pass:changeit -topk8 -out ec_privatekey_secp256k1_pkcs8_aes256PBE.pem -outform PEM -passout pass:changeit

$ cat ec_privatekey_secp256k1_pkcs8_aes256PBE.pem
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIGoMBsGCSqGSIb3DQEFAzAOBAht6/iGII2mSAICCAAEgYhvjHAv9h0Zxx3kSNKu
/eid6FE5Vf9sZBlW9BAs+dlMBp8NxV/aJjhhkbkO1p+o/nyWY1uuGXQGci0aCVlr
kWZa5kOAxFmvltsqxr6T3M2dHjF19WjaXRNBKj/Me0UHQBPY+Up2BX2nm/s6Ag8/
wiuGRFqReliMZOyMQR02LC8N8blwav5cv6rU
-----END ENCRYPTED PRIVATE KEY-----
```

* Visualización:

![EC Private Key DES3+PBE](images/EC_privatekey_secp256k1-pkcs8-PBE-image02.png "EC Private Key DES3+PBE")

>NOTA: Al parecer `-topk8` ha modificado la protección a `DES + PBE` en lugar de preservar ÀES-256 + PBE`

```
$ openssl asn1parse -i -dump -in ec_privatekey_secp256k1_pkcs8_aes256PBE.pem -inform PEM
    0:d=0  hl=3 l= 168 cons: SEQUENCE          
    3:d=1  hl=2 l=  27 cons:  SEQUENCE          
    5:d=2  hl=2 l=   9 prim:   OBJECT            :pbeWithMD5AndDES-CBC
   16:d=2  hl=2 l=  14 cons:   SEQUENCE          
   18:d=3  hl=2 l=   8 prim:    OCTET STRING      
      0000 - 6d eb f8 86 20 8d a6 48-                          m... ..H
   28:d=3  hl=2 l=   2 prim:    INTEGER           :0800
   32:d=1  hl=3 l= 136 prim:  OCTET STRING      
      0000 - 6f 8c 70 2f f6 1d 19 c7-1d e4 48 d2 ae fd e8 9d   o.p/......H.....
      0010 - e8 51 39 55 ff 6c 64 19-56 f4 10 2c f9 d9 4c 06   .Q9U.ld.V..,..L.
      0020 - 9f 0d c5 5f da 26 38 61-91 b9 0e d6 9f a8 fe 7c   ..._.&8a.......|
      0030 - 96 63 5b ae 19 74 06 72-2d 1a 09 59 6b 91 66 5a   .c[..t.r-..Yk.fZ
      0040 - e6 43 80 c4 59 af 96 db-2a c6 be 93 dc cd 9d 1e   .C..Y...*.......
      0050 - 31 75 f5 68 da 5d 13 41-2a 3f cc 7b 45 07 40 13   1u.h.].A*?.{E.@.
      0060 - d8 f9 4a 76 05 7d a7 9b-fb 3a 02 0f 3f c2 2b 86   ..Jv.}...:..?.+.
      0070 - 44 5a 91 7a 58 8c 64 ec-8c 41 1d 36 2c 2f 0d f1   DZ.zX.d..A.6,/..
      0080 - b9 70 6a fe 5c bf aa d4-                          .pj.\...
```

### Sección 02. Ejemplos de generación y exportación de `Keypairs`  `DSA` y `EC` con `XCA`

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

#### Creación de un nuevo `Keypair` `DSA`: 

![XCA New DSA Key](images/XCA_NEW_KEYS.png "XCA New DSA Key")

![XCA New DSA Key](images/XCA_NEW_DSA_2048.png "XCA New DSA Key")

#### Visualización de las propiedades del nuevo `Keypair` `DSA` generado: 

![XCA New DSA Key Properties](images/XCA_NEW_DSA_2048_Details.png "XCA New DSA Key Properties")

#### Formatos de persistencia (_Export_) disponibles para el nuevo `Keypair` `DSA` generado: 

![XCA New Key Persistence modes](images/XCA_NEW_DSA_2048_Export.png "XCA New Key Persistence modes")

#### Creación de un nuevo `Keypair` `EC`:

![XCA New EC Key](images/"XCA New EC Key")

![XCA New EC Key](images/XCA_NEW_EC_Home.png "XCA New EC Key")

#### Selección de la curva `semilla` para la generación del nuevo `Keypair` `EC` a generar: 

![XCA New EC Key Curve Selection](images/XCA_NEW_EC_Curves.png "XCA New EC Key Curve Selection")

![XCA New EC Key Curve Selection](images/XCA_NEW_EC_Sample.png "XCA New EC Key Curve Selection")

![XCA New EC Key Curve Selection](images/XCC_NEW_EC_SampleOK.png "XCA New EC Key Curve Selection")

#### Visualización de las propiedades del nuevo `Keypair` `EC` generado: 
 
![XCA New EC Key Properties](images/XCA_NEW_EC_Details.png "XCA New EC Key Properties")

#### Formatos de persistencia (_Export_) disponibles para el nuevo `Keypair` `EC` generado: 

![XCA New Key Persistence modes](images/XCA_NEW_EC_Export.png "XCA New Key Persistence modes")