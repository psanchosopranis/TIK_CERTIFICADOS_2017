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
## INTRODUCCION (breve) a `CSR - Certificate Signing Request`

### PKI - Public key infrastructure y Certificate Enrollment Process

>[Public key infrastructure - Wikipedia (en)](https://en.wikipedia.org/wiki/Public_key_infrastructure)

>[Infraestructura de clave pública - Wikipedia (es)](https://es.wikipedia.org/wiki/Infraestructura_de_clave_p%C3%BAblica)

>[What is PKI (public key infrastructure)? - Definition from WhatIs.com](http://searchsecurity.techtarget.com/definition/PKI)

>[NetContractor Blog » Certificate Enrollment Process](http://www.netcontractor.pl/blog/wp-content/uploads/2014/11/Certificate-Enrollment-Process.ppsx)

### Certificate Signing Request

>[Certificate signing request Wikipedia (en)](https://en.wikipedia.org/wiki/Certificate_signing_request)

>[GlobalSign Blog: What is a Certificate Signing Request (CSR)?](https://www.globalsign.com/en/blog/what-is-a-certificate-signing-request-csr/)

>[Global Sign: Certificate Signing Request (CSR) - Overview](https://support.globalsign.com/customer/portal/articles/1229769)

>[SSL Shopper - What is a CSR (Certificate Signing Request)?](https://www.sslshopper.com/what-is-a-csr-certificate-signing-request.html)

>[PKCS #10: Certification Request Syntax Standard - ARCHIVE](https://web.archive.org/web/20061210143303/http://www.rsasecurity.com/rsalabs/node.asp?id=2132)

>[RFC 2986 - PKCS #10: Certification Request Syntax Specification Version 1.7](https://datatracker.ietf.org/doc/rfc2986/?include_text=1)

# Seccion 01 - Ejemplos de utilización de `openssl` para generar `CSR - Certificate Signing Requests'

## 01.01 Archivo de Configuración de Openssl 

Openssl dispone de un `archivo de configuración` para todas sus operaciones relativas a la Infraestructura PKI (Public key infrastructure). Entre estas operaciones se encuentran las que permiten realizar el tratamiento de `CSR`.

En sistemas Linux habitualmente dicho archivo puede encontrarse en la ruta `/etc/ssl/openssl.cnf`

>Nota: consultar la documentación de la distribución Linux que se esté utilizando para verificar la ubicación por defecto del archivo `openssl.cnf`

En una distribución Linux Ubuntu 17.10 (la que se está utilizando en los laboratorios de ejemplo) tiene este contenido:

```
#
# OpenSSL example configuration file.
# This is mostly being used for generation of certificate requests.
#

# This definition stops the following lines choking if HOME isn't
# defined.
HOME			= .
RANDFILE		= $ENV::HOME/.rnd

# Extra OBJECT IDENTIFIER info:
#oid_file		= $ENV::HOME/.oid
oid_section		= new_oids

# To use this configuration file with the "-extfile" option of the
# "openssl x509" utility, name here the section containing the
# X.509v3 extensions to use:
# extensions		= 
# (Alternatively, use a configuration file that has only
# X.509v3 extensions in its main [= default] section.)

[ new_oids ]

# We can add new OIDs in here for use by 'ca', 'req' and 'ts'.
# Add a simple OID like this:
# testoid1=1.2.3.4
# Or use config file substitution like this:
# testoid2=${testoid1}.5.6

# Policies used by the TSA examples.
tsa_policy1 = 1.2.3.4.1
tsa_policy2 = 1.2.3.4.5.6
tsa_policy3 = 1.2.3.4.5.7

####################################################################
[ ca ]
default_ca	= CA_default		# The default ca section

####################################################################
[ CA_default ]

dir		= ./demoCA		# Where everything is kept
certs		= $dir/certs		# Where the issued certs are kept
crl_dir		= $dir/crl		# Where the issued crl are kept
database	= $dir/index.txt	# database index file.
#unique_subject	= no			# Set to 'no' to allow creation of
					# several ctificates with same subject.
new_certs_dir	= $dir/newcerts		# default place for new certs.

certificate	= $dir/cacert.pem 	# The CA certificate
serial		= $dir/serial 		# The current serial number
crlnumber	= $dir/crlnumber	# the current crl number
					# must be commented out to leave a V1 CRL
crl		= $dir/crl.pem 		# The current CRL
private_key	= $dir/private/cakey.pem# The private key
RANDFILE	= $dir/private/.rand	# private random number file

x509_extensions	= usr_cert		# The extentions to add to the cert

# Comment out the following two lines for the "traditional"
# (and highly broken) format.
name_opt 	= ca_default		# Subject Name options
cert_opt 	= ca_default		# Certificate field options

# Extension copying option: use with caution.
# copy_extensions = copy

# Extensions to add to a CRL. Note: Netscape communicator chokes on V2 CRLs
# so this is commented out by default to leave a V1 CRL.
# crlnumber must also be commented out to leave a V1 CRL.
# crl_extensions	= crl_ext

default_days	= 365			# how long to certify for
default_crl_days= 30			# how long before next CRL
default_md	= default		# use public key default MD
preserve	= no			# keep passed DN ordering

# A few difference way of specifying how similar the request should look
# For type CA, the listed attributes must be the same, and the optional
# and supplied fields are just that :-)
policy		= policy_match

# For the CA policy
[ policy_match ]
countryName		= match
stateOrProvinceName	= match
organizationName	= match
organizationalUnitName	= optional
commonName		= supplied
emailAddress		= optional

# For the 'anything' policy
# At this point in time, you must list all acceptable 'object'
# types.
[ policy_anything ]
countryName		= optional
stateOrProvinceName	= optional
localityName		= optional
organizationName	= optional
organizationalUnitName	= optional
commonName		= supplied
emailAddress		= optional

####################################################################
[ req ]
default_bits		= 2048
default_keyfile 	= privkey.pem
distinguished_name	= req_distinguished_name
attributes		= req_attributes
x509_extensions	= v3_ca	# The extentions to add to the self signed cert

# Passwords for private keys if not present they will be prompted for
# input_password = secret
# output_password = secret

# This sets a mask for permitted string types. There are several options. 
# default: PrintableString, T61String, BMPString.
# pkix	 : PrintableString, BMPString (PKIX recommendation before 2004)
# utf8only: only UTF8Strings (PKIX recommendation after 2004).
# nombstr : PrintableString, T61String (no BMPStrings or UTF8Strings).
# MASK:XXXX a literal mask value.
# WARNING: ancient versions of Netscape crash on BMPStrings or UTF8Strings.
string_mask = utf8only

# req_extensions = v3_req # The extensions to add to a certificate request

[ req_distinguished_name ]
countryName			= Country Name (2 letter code)
countryName_default		= AU
countryName_min			= 2
countryName_max			= 2

stateOrProvinceName		= State or Province Name (full name)
stateOrProvinceName_default	= Some-State

localityName			= Locality Name (eg, city)

0.organizationName		= Organization Name (eg, company)
0.organizationName_default	= Internet Widgits Pty Ltd

# we can do this but it is not needed normally :-)
#1.organizationName		= Second Organization Name (eg, company)
#1.organizationName_default	= World Wide Web Pty Ltd

organizationalUnitName		= Organizational Unit Name (eg, section)
#organizationalUnitName_default	=

commonName			= Common Name (e.g. server FQDN or YOUR name)
commonName_max			= 64

emailAddress			= Email Address
emailAddress_max		= 64

# SET-ex3			= SET extension number 3

[ req_attributes ]
challengePassword		= A challenge password
challengePassword_min		= 4
challengePassword_max		= 20

unstructuredName		= An optional company name

[ usr_cert ]

# These extensions are added when 'ca' signs a request.

# This goes against PKIX guidelines but some CAs do it and some software
# requires this to avoid interpreting an end user certificate as a CA.

basicConstraints=CA:FALSE

# Here are some examples of the usage of nsCertType. If it is omitted
# the certificate can be used for anything *except* object signing.

# This is OK for an SSL server.
# nsCertType			= server

# For an object signing certificate this would be used.
# nsCertType = objsign

# For normal client use this is typical
# nsCertType = client, email

# and for everything including object signing:
# nsCertType = client, email, objsign

# This is typical in keyUsage for a client certificate.
# keyUsage = nonRepudiation, digitalSignature, keyEncipherment

# This will be displayed in Netscape's comment listbox.
nsComment			= "OpenSSL Generated Certificate"

# PKIX recommendations harmless if included in all certificates.
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid,issuer

# This stuff is for subjectAltName and issuerAltname.
# Import the email address.
# subjectAltName=email:copy
# An alternative to produce certificates that aren't
# deprecated according to PKIX.
# subjectAltName=email:move

# Copy subject details
# issuerAltName=issuer:copy

#nsCaRevocationUrl		= http://www.domain.dom/ca-crl.pem
#nsBaseUrl
#nsRevocationUrl
#nsRenewalUrl
#nsCaPolicyUrl
#nsSslServerName

# This is required for TSA certificates.
# extendedKeyUsage = critical,timeStamping

[ v3_req ]

# Extensions to add to a certificate request

basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment

[ v3_ca ]


# Extensions for a typical CA


# PKIX recommendation.

subjectKeyIdentifier=hash

authorityKeyIdentifier=keyid:always,issuer

# This is what PKIX recommends but some broken software chokes on critical
# extensions.
#basicConstraints = critical,CA:true
# So we do this instead.
basicConstraints = CA:true

# Key usage: this is typical for a CA certificate. However since it will
# prevent it being used as an test self-signed certificate it is best
# left out by default.
# keyUsage = cRLSign, keyCertSign

# Some might want this also
# nsCertType = sslCA, emailCA

# Include email address in subject alt name: another PKIX recommendation
# subjectAltName=email:copy
# Copy issuer details
# issuerAltName=issuer:copy

# DER hex encoding of an extension: beware experts only!
# obj=DER:02:03
# Where 'obj' is a standard or added object
# You can even override a supported extension:
# basicConstraints= critical, DER:30:03:01:01:FF

[ crl_ext ]

# CRL extensions.
# Only issuerAltName and authorityKeyIdentifier make any sense in a CRL.

# issuerAltName=issuer:copy
authorityKeyIdentifier=keyid:always

[ proxy_cert_ext ]
# These extensions should be added when creating a proxy certificate

# This goes against PKIX guidelines but some CAs do it and some software
# requires this to avoid interpreting an end user certificate as a CA.

basicConstraints=CA:FALSE

# Here are some examples of the usage of nsCertType. If it is omitted
# the certificate can be used for anything *except* object signing.

# This is OK for an SSL server.
# nsCertType			= server

# For an object signing certificate this would be used.
# nsCertType = objsign

# For normal client use this is typical
# nsCertType = client, email

# and for everything including object signing:
# nsCertType = client, email, objsign

# This is typical in keyUsage for a client certificate.
# keyUsage = nonRepudiation, digitalSignature, keyEncipherment

# This will be displayed in Netscape's comment listbox.
nsComment			= "OpenSSL Generated Certificate"

# PKIX recommendations harmless if included in all certificates.
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid,issuer

# This stuff is for subjectAltName and issuerAltname.
# Import the email address.
# subjectAltName=email:copy
# An alternative to produce certificates that aren't
# deprecated according to PKIX.
# subjectAltName=email:move

# Copy subject details
# issuerAltName=issuer:copy

#nsCaRevocationUrl		= http://www.domain.dom/ca-crl.pem
#nsBaseUrl
#nsRevocationUrl
#nsRenewalUrl
#nsCaPolicyUrl
#nsSslServerName

# This really needs to be in place for it to be a proxy certificate.
proxyCertInfo=critical,language:id-ppl-anyLanguage,pathlen:3,policy:foo

####################################################################
[ tsa ]

default_tsa = tsa_config1	# the default TSA section

[ tsa_config1 ]

# These are used by the TSA reply generation only.
dir		= ./demoCA		# TSA root directory
serial		= $dir/tsaserial	# The current serial number (mandatory)
crypto_device	= builtin		# OpenSSL engine to use for signing
signer_cert	= $dir/tsacert.pem 	# The TSA signing certificate
					# (optional)
certs		= $dir/cacert.pem	# Certificate chain to include in reply
					# (optional)
signer_key	= $dir/private/tsakey.pem # The TSA private key (optional)

default_policy	= tsa_policy1		# Policy if request did not specify it
					# (optional)
other_policies	= tsa_policy2, tsa_policy3	# acceptable policies (optional)
digests		= md5, sha1		# Acceptable message digests (mandatory)
accuracy	= secs:1, millisecs:500, microsecs:100	# (optional)
clock_precision_digits  = 0	# number of digits after dot. (optional)
ordering		= yes	# Is ordering defined for timestamps?
				# (optional, default: no)
tsa_name		= yes	# Must the TSA name be included in the reply?
				# (optional, default: no)
ess_cert_id_chain	= no	# Must the ESS cert id chain be included?
				# (optional, default: no)
```

## 01.02 `CSR` de `Usuario`  

Cuando nos referimos a `CSR de Usuario` nos estamos refiriendo en realidad a un `CSR` que identifica a una `Persona` en sus diferentes posibilidades de `rol` o `tipo de identidad` que se desea acreditar:

Tipos de certificados: 
>(fuente: [Wikipedia (es) >> Infraestructura de clave pública: Tipos de Certificado](https://es.wikipedia.org/wiki/Infraestructura_de_clave_pública) )

Existen diferentes tipos de certificado digital, en función de la información que contiene cada uno y a nombre de quién se emite el certificado:
* **Certificado personal**, que acredita la identidad del titular.
* **Certificado de pertenencia a empresa**, que además de la identidad del titular acredita su vinculación con la entidad para la que trabaja.
* **Certificado de representante**, que además de la pertenencia a empresa acredita también los poderes de representación que el titular tiene sobre la misma.
* **Certificado de persona jurídica**, que identifica una empresa o sociedad como tal a la hora de realizar trámites ante las administraciones o instituciones.
* **Certificado de atributo**, el cual permite identificar una cualidad, estado o situación. Este tipo de certificado va asociado al certificado personal. (p.ej. Médico, Director, Casado, Apoderado de..., etc.).

### Paso 1 Generación del `Key Pair` (O utilización de uno que haya sido generado/almacenado prevamente)

En este ejemplo seleccionaremos un 'Key Pair' `RSA` (podríamos haber utilizado igualmente `DSA` o `EC`)

>Nota: Es conveniente asegurarse de cúales son los requerimientos/especificaciones de la CA (Certification Authority) (ver siguientes laboratorios) sobre los tipos de estandares aceptados y sus parámetros (longitud de clave, curva elíptica semilla, ...)

```
$ openssl genrsa -out rsakeypair01.pem -des3 -passout pass:changeit 2048 
Generating RSA private key, 2048 bit long modulus
....................................................................+++
.......................+++
e is 65537 (0x10001)

$ cat rsakeypair01.pem 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,992913A292987415

wtvGjbDbKjFkGj93O4i2Q0mUkqSaC9d/2PtCE78224v/Ib/e5HSHYD6cX4g1Ys55
Dm57SbqZ7eMW6MTuF8KXaSY3RdurWGzuH/xSOB22lrMOugK6sUKyZKQu76IDMtj3
VRSUBQqp9/6AxG+HEI/kL/tYT1Mywnlo9C2APhdKrO2/2lY6C+9A680GrcPeKFeT
04t3ND9gBOKsyU8m7jzWtWH6kZ8IRz3Tg3tTVAWN/gWUjZw44AGm7mBjZxgJdmKD
9rpxGjXmwDQjBfJu4v0NV/f5wMzjv8ZiefF3vSYqeKv/NlVwyxjktoPvtRPoTcKy
kDWL3SlPZ5+BQftTpHnB7+s4LDmQRsdm530nHQ2DYQ1H4djbdjYzujjrrVWyQPsa
1ILQUaOtGzlQ2MP4TmB0mwgsWzgj5YdWPR2/4GPpDjUM/cwQHCfCBEig1cIEThzA
f3ogAbUnYkb9gZw6TTvBXqKuCIZwsT0uRUmYfTehs/KbmT9EHiT8OumAbs7DR4dQ
FknvSI0YWU9Y8RJG7/rcX6olKjxWNe6xkjztMKcpkVj8sBz40Ebr7LEVTGWUj7Hv
lkGDBDE65F9eLVWG55FzivbWt2xl0+lB7SK8nMtlShq3Y4z1UVHigQXn8RAz2PXJ
+F4jFgSIGAD01XD11idTK3UE4GFe8W7IGEJoAbqNiwANnX1HMKP6BwhJHS45hZEM
lmEzzDent0cu3u4V2J+ILK8+CxD+ZCmcIDvG07C+G+Rk05D7dmNM4t0nByiD5Y3q
g1ulzcK9MOoDv3KKKpJSAI96q/RGeBrFxFvVCaTyvzaKK26+Ndw5Q2gJmjjm4pmo
6/V+pGwYdipg5yjlDM95J9QaCF/FwE+slO2I82S8HO/QJ8yjDZYQiw+EggCYPbSC
TJJRCElepHfIonPU3icGdR+sZvZ57FFEwg5Cap/oKsm9V2+bsWER7wpdaEx/AbAh
7T6ciALZz3RGmL4z99J7Yj5HUU5AwuwAMQzv+itrHu4nYSrFlVHKOOA5vHnA7p8L
QQilu0q0O2xAnPcw9+B88Y5m9VyDCNNNfR+w3HINlGqQJLxfMO4+pgMapTQ6B2d7
7kog0ZWOq4F8SAMwDL2LtoHaMz6HG4bJw5Bik2iWhqE8NDd66HdZtng16XhDDYaL
uIizkaHtIIhx3LVgR74aeZH/jtZGy2qooBSEJCLNelSag4rOtawszI8Z32HoCAYU
SKg8qWU0AmjcsDY1fgr8MEd0ZQ8R6iZbAgYPfyUs8MA91EWKuylVdZp2RyDfVwQq
lPIYXRuxnBkww2B81BalSlJM48vjvuND0iXOItz7jQ8MVFYggKzE5Q+dfX/q6FGO
Lrg0J1EOYuzsPJ2lZcLEqldD5n2lrvGVm0ohabJN+/cuLpSSbN9IjRoEHUxUG1Fp
aKlSi9IjSZkD2hUGAb+mPFnvKZ7mXOzfsI2/9D3jOmG3l9ntJUnfpsqD1Lj2YQrK
RLaB6NGwPLAEXXB4clRHYqd6ieU8tx5elDpYTQV2txDH+3Nu2l80i0fTGPeaMpq+
et1ycxOHvDzJbqdEhjBRFZSWNXfERUaIWPHI1jpJtuoUw/W8MrjDF8HwxQyLVkaF
-----END RSA PRIVATE KEY-----
```

### Paso 2. Generación del `CSR - Certificate Signing Request`

* Ayuda de `openssl req`

```
$ openssl req -help
unknown option -help
req [options] <infile >outfile
where options  are
 -inform arg    input format - DER or PEM
 -outform arg   output format - DER or PEM
 -in arg        input file
 -out arg       output file
 -text          text form of request
 -pubkey        output public key
 -noout         do not output REQ
 -verify        verify signature on REQ
 -modulus       RSA modulus
 -nodes         don't encrypt the output key
 -engine e      use engine e, possibly a hardware device
 -subject       output the request's subject
 -passin        private key password source
 -key file      use the private key contained in file
 -keyform arg   key file format
 -keyout arg    file to send the key to
 -rand file:file:...
                load the file (or the files in the directory) into
                the random number generator
 -newkey rsa:bits generate a new RSA key of 'bits' in size
 -newkey dsa:file generate a new DSA key, parameters taken from CA in 'file'
 -newkey ec:file generate a new EC key, parameters taken from CA in 'file'
 -[digest]      Digest to sign with (md5, sha1, md2, mdc2, md4)
 -config file   request template file.
 -subj arg      set or modify request subject
 -multivalue-rdn enable support for multivalued RDNs
 -new           new request.
 -batch         do not ask anything during request generation
 -x509          output a x509 structure instead of a cert. req.
 -days          number of days a certificate generated by -x509 is valid for.
 -set_serial    serial number to use for a certificate generated by -x509.
 -newhdr        output "NEW" in the header lines
 -asn1-kludge   Output the 'request' in a format that is wrong but some CA's
                have been reported as requiring
 -extensions .. specify certificate extension section (override value in config file)
 -reqexts ..    specify request extension section (override value in config file)
 -utf8          input characters are UTF8 (default ASCII)
 -nameopt arg    - various certificate name options
 -reqopt arg    - various request text options
```


* Creación del CSR

```
$ openssl req -new -utf8 -sha256 -key rsakeypair01.pem -passin pass:changeit -out CSR_User01.pem 
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Madrid
Locality Name (eg, city) []:Madrid
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Empresa Ejemplo, S.A.          
Organizational Unit Name (eg, section) []:Administración PKI
Common Name (e.g. server FQDN or YOUR name) []:Juan Pérez Martínez
Email Address []:juan.perez.martinez@empresa.ejemplo.es 

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:changeit
An optional company name []:Ejemplo
```

>IMPORTANTE: no olvidar usar la opción `-utf8` _(input characters are UTF8 (default ASCII))_ cuando se van a introducir caracteres `especiales` (acentos, caracteres en alfabeto griego, cirílico, ...) como en el ejemplo superior `Common Name (e.g. server FQDN or YOUR name) []:Juan Pérez Martínez`

* Visualización del CSR con `grc-viewer`

```
$ gcr-viewer CSR_User01.pem
```

![gcr-viewer CSR_User01.pem](images/gcr-viewer-CSR_User01_image01.png "gcr-viewer CSR_User01.pem")

* Visualización con la herramienta en-línea >[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder01_img01.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder01_img02.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder01_img03.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder01_img04.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder01_img05.png "https://redkestrel.co.uk/products/decoder/")

* Visualización del CSR con `openssl asn1parse`

```
$ openssl asn1parse -i -dump -inform PEM -in CSR_User01.pem 
    0:d=0  hl=4 l= 827 cons: SEQUENCE          
    4:d=1  hl=4 l= 547 cons:  SEQUENCE          
    8:d=2  hl=2 l=   1 prim:   INTEGER           :00
   11:d=2  hl=3 l= 196 cons:   SEQUENCE          
   14:d=3  hl=2 l=  11 cons:    SET               
   16:d=4  hl=2 l=   9 cons:     SEQUENCE          
   18:d=5  hl=2 l=   3 prim:      OBJECT            :countryName
   23:d=5  hl=2 l=   2 prim:      PRINTABLESTRING   :ES
   27:d=3  hl=2 l=  15 cons:    SET               
   29:d=4  hl=2 l=  13 cons:     SEQUENCE          
   31:d=5  hl=2 l=   3 prim:      OBJECT            :stateOrProvinceName
   36:d=5  hl=2 l=   6 prim:      UTF8STRING        :Madrid
   44:d=3  hl=2 l=  15 cons:    SET               
   46:d=4  hl=2 l=  13 cons:     SEQUENCE          
   48:d=5  hl=2 l=   3 prim:      OBJECT            :localityName
   53:d=5  hl=2 l=   6 prim:      UTF8STRING        :Madrid
   61:d=3  hl=2 l=  30 cons:    SET               
   63:d=4  hl=2 l=  28 cons:     SEQUENCE          
   65:d=5  hl=2 l=   3 prim:      OBJECT            :organizationName
   70:d=5  hl=2 l=  21 prim:      UTF8STRING        :Empresa Ejemplo, S.A.
   93:d=3  hl=2 l=  28 cons:    SET               
   95:d=4  hl=2 l=  26 cons:     SEQUENCE          
   97:d=5  hl=2 l=   3 prim:      OBJECT            :organizationalUnitName
  102:d=5  hl=2 l=  19 prim:      UTF8STRING        :Administración PKI
  123:d=3  hl=2 l=  30 cons:    SET               
  125:d=4  hl=2 l=  28 cons:     SEQUENCE          
  127:d=5  hl=2 l=   3 prim:      OBJECT            :commonName
  132:d=5  hl=2 l=  21 prim:      UTF8STRING        :Juan Pérez Martínez
  155:d=3  hl=2 l=  53 cons:    SET               
  157:d=4  hl=2 l=  51 cons:     SEQUENCE          
  159:d=5  hl=2 l=   9 prim:      OBJECT            :emailAddress
  170:d=5  hl=2 l=  38 prim:      IA5STRING         :juan.perez.martinez@empresa.ejemplo.es
  210:d=2  hl=4 l= 290 cons:   SEQUENCE          
  214:d=3  hl=2 l=  13 cons:    SEQUENCE          
  216:d=4  hl=2 l=   9 prim:     OBJECT            :rsaEncryption
  227:d=4  hl=2 l=   0 prim:     NULL              
  229:d=3  hl=4 l= 271 prim:    BIT STRING        
      0000 - 00 30 82 01 0a 02 82 01-01 00 b6 a1 f6 4c 6c 6e   .0...........Lln
      0010 - 4f 87 49 53 83 48 44 81-39 b6 af 4a 3e 14 81 a5   O.IS.HD.9..J>...
      0020 - 3a a7 ff 7b da 2e b5 09-6f 45 d0 3d ac c8 47 f9   :..{....oE.=..G.
      0030 - 99 f9 48 56 cc fd f8 ff-9d 74 4c 07 1d 9d a7 05   ..HV.....tL.....
      0040 - f9 19 94 f0 fc ca eb 74-b1 c1 a3 4d 8e a7 fc f1   .......t...M....
      0050 - 22 47 e5 ec c2 d5 e3 5a-44 87 d9 3b c7 3d 08 57   "G.....ZD..;.=.W
      0060 - eb f3 c3 0f 03 aa 05 f6-6e 68 6b ea a5 2a 7e 1c   ........nhk..*~.
      0070 - d4 06 00 c0 df 02 69 10-62 7f 42 6e d5 a0 99 4c   ......i.b.Bn...L
      0080 - f6 6b 55 a1 1a 4b bd 3a-de 4d 56 5b 14 42 ad b1   .kU..K.:.MV[.B..
      0090 - 09 dd 74 82 2d 8d 74 27-1e c3 62 95 9a 9e 46 da   ..t.-.t'..b...F.
      00a0 - 7b 2e 08 fc b6 29 5d 4b-60 37 b7 d1 73 9b c6 3e   {....)]K`7..s..>
      00b0 - 59 e8 14 90 25 b9 af fc-b2 3a 9d bf e9 8e 42 61   Y...%....:....Ba
      00c0 - 08 c7 86 92 8a 17 f9 ac-0e ac 2e 15 9e 30 1b 9c   .............0..
      00d0 - 71 18 0e bd 9c b9 34 72-48 6d c3 4c ae 55 66 5f   q.....4rHm.L.Uf_
      00e0 - d8 d3 e7 2c c4 0c 7d 67-c4 54 bd 95 15 e6 7d 16   ...,..}g.T....}.
      00f0 - ef a1 c5 ed 14 92 90 28-95 7f c3 78 c0 65 d5 05   .......(...x.e..
      0100 - 2f 7e ca ae 3d 42 0f 3f-cc 53 02 03 01 00 01      /~..=B.?.S.....
  504:d=2  hl=2 l=  49 cons:   cont [ 0 ]        
  506:d=3  hl=2 l=  22 cons:    SEQUENCE          
  508:d=4  hl=2 l=   9 prim:     OBJECT            :unstructuredName
  519:d=4  hl=2 l=   9 cons:     SET               
  521:d=5  hl=2 l=   7 prim:      UTF8STRING        :Ejemplo
  530:d=3  hl=2 l=  23 cons:    SEQUENCE          
  532:d=4  hl=2 l=   9 prim:     OBJECT            :challengePassword
  543:d=4  hl=2 l=  10 cons:     SET               
  545:d=5  hl=2 l=   8 prim:      UTF8STRING        :changeit
  555:d=1  hl=2 l=  13 cons:  SEQUENCE          
  557:d=2  hl=2 l=   9 prim:   OBJECT            :sha256WithRSAEncryption
  568:d=2  hl=2 l=   0 prim:   NULL              
  570:d=1  hl=4 l= 257 prim:  BIT STRING        
      0000 - 00 13 53 5d 71 de 74 5d-cb fc 92 ca 09 05 db 04   ..S]q.t]........
      0010 - cb 26 26 0a 90 f8 51 e0-ee 7a cc 02 81 09 42 38   .&&...Q..z....B8
      0020 - 6c 65 1d 31 21 e7 37 58-22 35 f6 ac 1a 99 e9 2c   le.1!.7X"5.....,
      0030 - 21 55 69 0b 46 03 42 ed-3b 82 c7 93 6f e4 db 8f   !Ui.F.B.;...o...
      0040 - 4c ff 16 0b db b9 1b 82-dc fe f6 7e 14 d4 ab fe   L..........~....
      0050 - 55 cc 45 48 b3 1f b4 86-bf 0f f5 a5 64 20 d8 80   U.EH........d ..
      0060 - 7e 7a 9d f2 2a fd ed 94-89 e4 11 85 03 28 0f 11   ~z..*........(..
      0070 - 08 e5 af 7c 50 d3 94 c1-df 76 d5 2a 1c c9 2e 99   ...|P....v.*....
      0080 - aa 22 d6 01 44 94 91 56-fa a2 6d 9f 74 24 cc 54   ."..D..V..m.t$.T
      0090 - a2 64 d3 05 5a ff 11 1b-3a c2 dc a3 68 9d 80 28   .d..Z...:...h..(
      00a0 - 3e 01 af 33 e6 d8 fd 72-c0 02 d0 20 a0 7e a4 15   >..3...r... .~..
      00b0 - 02 58 2f 6a a8 23 b0 04-09 ca 75 0d a1 53 c4 68   .X/j.#....u..S.h
      00c0 - 62 72 a9 a1 15 73 bb d1-6b 04 ad cc dc 99 06 90   br...s..k.......
      00d0 - c4 c7 8a b6 84 6a 3c 51-97 87 e5 61 c5 2b 24 bf   .....j<Q...a.+$.
      00e0 - c7 fb 7d b1 b5 cf 08 6a-ec d2 98 51 1d ae b4 9a   ..}....j...Q....
      00f0 - 9b 03 20 16 f0 e3 c4 df-51 e0 6d 40 49 10 7f f8   .. .....Q.m@I...
      0100 - 5d                               
```

## 01.03 `CSR` de `Dominio` (de `Servidor`)  


## 01.04 `CSR` de `Usuario` EXTENDIDO 


## 01.05 `CSR` de `Dominio` (de `Servidor`) EXTENDIDO 


# Seccion 02 - Ejemplos de utilización de `Java Keytool` para generar `CSR - Certificate Signing Requests'


## 02.02 `CSR` de `Usuario`  


## 02.03 `CSR` de `Dominio` (de `Servidor`)  


## 02.04 `CSR` de `Usuario` EXTENDIDO 


## 02.05 `CSR` de `Dominio` (de `Servidor`) EXTENDIDO 

# Seccion 03 - Ejemplos de utilización de `Java Keytool` para generar `CSR - Certificate Signing Requests'


## 03.02 `CSR` de `Usuario`  


## 03.03 `CSR` de `Dominio` (de `Servidor`)  


