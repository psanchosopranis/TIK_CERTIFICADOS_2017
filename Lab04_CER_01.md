# Lab 03 CSR Certificate Signing Request 
##### Apuntes para TIK sobre certificados (Dic 2017)

### TOOLS

>[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

>[Openssl](https://www.openssl.org/)

>[DumpASN1 - ASN.1 object dump/syntax check program](http://manpages.ubuntu.com/manpages/xenial/man1/dumpasn1.1.html) del autor [Peter Gutmann](https://www.cs.auckland.ac.nz/~pgut001/)

>[XCA - X Certificate and key management](http://xca.sourceforge.net/) (c) by Christian Hohnstädt, christian@hohnstaedt.de]

>[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

>[CSR Decoder And Certificate Decoder(Red Krestel)(old)](https://certlogik.com/decoder/)

>[MMC - Microsoft Management Console](https://msdn.microsoft.com/en-us/library/bb742441.aspx)

### Preparación del laboratorio

Creacion del directorio de trabajo del lab:

```
$ mkdir -pv $HOME/labs/lab04_CER_01
mkdir: se ha creado el directorio '/home/devel1/labs/lab04_CER_01'
$ cd $HOME/labs/lab04_CER_01
$ pwd
/home/devel1/labs/lab04_CER_01
```
## INTRODUCCION (breve) a Certificados

### Certificados

>[Public key certificate - Wikipedia (en)](https://en.wikipedia.org/wiki/Public_key_certificate)

>In cryptography, a **public key certificate**, also known as a **digital certificate** or **identity certificate**, is an electronic document used to prove the ownership of a public key.
> 
>The **certificate includes information** about:
>* the key, 
>* information about the identity of its owner (called the subject), 
>* and the digital signature of an entity that has verified the certificate's contents (called the issuer). 
>
>If the signature is valid, and the software examining the certificate trusts the issuer, then it can use that key to communicate securely with the certificate's subject.
>* In **email encryption**, **code signing**, and **e-signature systems**, a certificate's subject is typically a **person** or **organization**. 
>* However, in **Transport Layer Security (TLS)** a certificate's subject is typically a **computer** or **other device**, though TLS certificates may identify organizations or individuals in addition to their core role in identifying devices. **TLS**, sometimes called by its older name **Secure Sockets Layer (SSL)**, is notable for being a part of **HTTPS**, a protocol for securely browsing the web.
>
>In a typical **public-key infrastructure (PKI) scheme**, the **certificate issuer** is a **certificate authority (CA)**, usually a company that charges customers to issue certificates for them. 
>
>By contrast, in a web of trust scheme, individuals sign each other's keys directly, in a format that performs a similar function to a public key certificate.
>
>The **most common format for public key certificates** is defined by **X.509**. Because X.509 is very general, the format is **further constrained by profiles** defined for certain use cases, such as **Public Key Infrastructure (X.509) as defined in RFC 5280**.

>[X.509 - Structure_of_a_certificate - Wikipedia (en)](https://en.wikipedia.org/wiki/X.509#Structure_of_a_certificate)

>**Certificate filename extensions** (tomado de [X.509 - Structure_of_a_certificate - Wikipedia (en)](https://en.wikipedia.org/wiki/X.509#Structure_of_a_certificate) ):
>
>There are several commonly used filename extensions for X.509 certificates. Unfortunately, some of these extensions are also used for other data such as private keys.
>* **.pem** – `(Privacy-enhanced Electronic Mail)` Base64 encoded DER certificate, enclosed between "-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----"
>* **.cer, .crt, .der** – usually in binary `DER` form, but Base64-encoded certificates are common too (see .pem above)
>* **.p7b, .p7c** – `PKCS#7 SignedData structure` without data, just certificate(s) or CRL(s)
>* **.p12** – `PKCS#12`, may contain certificate(s) (public) and private keys (password protected)
>* **.pfx – PFX**, predecessor of PKCS#12 (usually contains data in PKCS#12 format, e.g., with PFX files generated in IIS)
>
>**PKCS#7** is a standard for signing or encrypting (officially called "enveloping") data. Since the certificate is needed to verify signed data, it is possible to include them in the SignedData structure. A `.P7C` file is a degenerated SignedData structure, without any data to sign.
>
>**PKCS#12** evolved from the personal information exchange `(PFX)` standard and is used to exchange public and private objects in a single file


>**Tipos de certificados**: [ver Infraestructura de clave pública - Wikipedia (es)](https://es.wikipedia.org/wiki/Infraestructura_de_clave_p%C3%BAblica)
>
>Existen diferentes **tipos** de **certificado digital**, en función de la información que contiene cada uno y a nombre de quién se emite el certificado:
>
>* **Certificado personal**, que acredita la identidad del titular.
>* **Certificado de pertenencia a empresa**, que además de la identidad del titular acredita su vinculación con la entidad para la que trabaja.
>* **Certificado de representante**, que además de la pertenencia a empresa acredita también los poderes de representación que el titular tiene sobre la misma.
>* **Certificado de persona jurídica**, que identifica una empresa o sociedad como tal a la hora de realizar trámites ante las administraciones o instituciones.
>* **Certificado de atributo** , el cual permite identificar una cualidad, estado o situación. Este tipo de certificado va asociado al certificado personal. (p.ej. Médico, Director, Casado, Apoderado de..., etc.).
>
>Además, existen otros tipos de certificado digital utilizados en entornos más técnicos:
>* **Certificado de servidor seguro**, utilizado en los servidores web que quieren proteger ante terceros el intercambio de información con los usuarios.
Certificado de firma de código, para garantizar la autoría y la no modificación del código de aplicaciones informáticas.

### PKI - Public key infrastructure y Certificate Enrollment Process

>[PKI - Infraestructura de clave pública - Wikipedia (es)](https://es.wikipedia.org/wiki/Infraestructura_de_clave_p%C3%BAblica)

>**Componentes de una Infraestructura de clave pública** (tomado de: [PKI - Infraestructura de clave pública - Wikipedia (es)](https://es.wikipedia.org/wiki/Infraestructura_de_clave_p%C3%BAblica) ):
>
>Los componentes más habituales de una infraestructura de clave pública son:
>* La **autoridad de certificación** (o, en inglés, **CA**, **Certificate Authority**): es la encargada de emitir y revocar certificados. Es la entidad de confianza que da legitimidad a la relación de una clave pública con la identidad de un usuario o servicio.
>* La **autoridad de registro** (o, en inglés, **RA**, **Registration Authority**): es la responsable de verificar el enlace entre los certificados (concretamente, entre la clave pública del certificado) y la identidad de sus titulares.
>* Los **repositorios**: son las estructuras encargadas de almacenar la información relativa a la PKI. Los dos repositorios más importantes son:
>   * el **repositorio de certificados** y 
>   * el **repositorio de listas de revocación de certificados**. En una lista de revocación de certificados (o, en inglés, **CRL**, **Certificate Revocation List**) se incluyen todos aquellos certificados que por algún motivo han dejado de ser válidos antes de la fecha establecida dentro del mismo certificado.
>* La **autoridad de validación** (o, en inglés, **VA**, **Validation Authority)**: es la encargada de comprobar la validez de los certificados digitales.
>* La **autoridad de sellado de tiempo** (o, en inglés, **TSA**, **TimeStamp Authority**): es la encargada de firmar documentos con la finalidad de probar que existían antes de un determinado instante de tiempo.
>* Los **usuarios** y **entidades finales** son aquellos que poseen un **par de claves (pública y privada)** y un **certificado** asociado a su **clave pública**. Utilizan un conjunto de aplicaciones que hacen uso de la tecnología **PKI** (para **validar firmas digitales**, **cifrar documentos** para otros usuarios, etc.)

>[Public key infrastructure - Wikipedia (en)](https://en.wikipedia.org/wiki/Public_key_infrastructure)

>[What is PKI (public key infrastructure)? - Definition from WhatIs.com](http://searchsecurity.techtarget.com/definition/PKI)

>[NetContractor Blog » Certificate Enrollment Process](http://www.netcontractor.pl/blog/wp-content/uploads/2014/11/Certificate-Enrollment-Process.ppsx)

>[IETF - RFC5280 Internet X.509 Public Key Infrastructure Certificate - and Certificate Revocation List (CRL) Profile](https://tools.ietf.org/html/rfc5280)

## PKCS

>[Wikipedia (ES) PKCS](https://es.wikipedia.org/wiki/PKCS)

>[Wikipedia (EN) PKCS](https://en.wikipedia.org/wiki/PKCS)

>[Public-Key Cryptography Standards `(PKCS)` (RSA Laboratories - _ARCHIVO_)](https://web.archive.org/web/20061209135809/http://www.rsasecurity.com/rsalabs/node.asp?id=2124)

>[PKCS #7: PKCS #7: Cryptographic Message Syntax Standard (RSA Laboratories - _ARCHIVO_)](https://web.archive.org/web/20061210143318/http://www.rsasecurity.com/rsalabs/node.asp?id=2129)

>[PKCS #12: PKCS #12: Personal Information Exchange Syntax Standard (RSA Laboratories - _ARCHIVO_)](https://web.archive.org/web/20061210143421/http://www.rsasecurity.com/rsalabs/node.asp?id=2138)


# Seccion 01 - Ejemplos de utilización de `openssl` para generar `CSR - Certificate Signing Requests'

## 01.01 Archivo de Configuración de Openssl 


![XCA_CSR_Server_step09.png](images/XCA_CSR_Server_step09.png "XCA_CSR_Server_step09.png")
