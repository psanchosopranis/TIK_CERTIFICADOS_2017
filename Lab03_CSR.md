# Lab 03 CSR Certificate Signing Request 
##### Apuntes para TIK sobre certificados (Dic 2017)

### TOOLS

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

>[Openssl](https://www.openssl.org/)

>[DumpASN1 - ASN.1 object dump/syntax check program](http://manpages.ubuntu.com/manpages/xenial/man1/dumpasn1.1.html) del autor [Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/)

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

Dos nuevas herramientas:

>[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

>[CSR Decoder And Certificate Decoder(Red Krestel)(old)](https://certlogik.com/decoder/)

### Preparación del laboratorio

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv $HOME/labs/lab03_CSR
mkdir: se ha creado el directorio '/home/devel1/labs/lab03_CSR'
$ cd $HOME/labs/lab03_CSR
$ pwd
/home/devel1/labs/lab03_CSR
```
## INTRODUCCION (breve) a `RSA Public Key scheme`

>[RSA Wikipedia (es)](https://es.wikipedia.org/wiki/RSA)