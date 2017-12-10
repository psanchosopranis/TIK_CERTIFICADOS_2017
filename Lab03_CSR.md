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

```
$ cat CSR_User01.pem 
-----BEGIN CERTIFICATE REQUEST-----
MIIDOzCCAiMCAQAwgcQxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEeMBwGA1UECgwVRW1wcmVzYSBFamVtcGxvLCBTLkEuMRww
GgYDVQQLDBNBZG1pbmlzdHJhY2nDs24gUEtJMR4wHAYDVQQDDBVKdWFuIFDDqXJl
eiBNYXJ0w61uZXoxNTAzBgkqhkiG9w0BCQEWJmp1YW4ucGVyZXoubWFydGluZXpA
ZW1wcmVzYS5lamVtcGxvLmVzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAtqH2TGxuT4dJU4NIRIE5tq9KPhSBpTqn/3vaLrUJb0XQPazIR/mZ+UhWzP34
/510TAcdnacF+RmU8PzK63SxwaNNjqf88SJH5ezC1eNaRIfZO8c9CFfr88MPA6oF
9m5oa+qlKn4c1AYAwN8CaRBif0Ju1aCZTPZrVaEaS7063k1WWxRCrbEJ3XSCLY10
Jx7DYpWankbaey4I/LYpXUtgN7fRc5vGPlnoFJAlua/8sjqdv+mOQmEIx4aSihf5
rA6sLhWeMBuccRgOvZy5NHJIbcNMrlVmX9jT5yzEDH1nxFS9lRXmfRbvocXtFJKQ
KJV/w3jAZdUFL37Krj1CDz/MUwIDAQABoDEwFgYJKoZIhvcNAQkCMQkMB0VqZW1w
bG8wFwYJKoZIhvcNAQkHMQoMCGNoYW5nZWl0MA0GCSqGSIb3DQEBCwUAA4IBAQAT
U11x3nRdy/ySygkF2wTLJiYKkPhR4O56zAKBCUI4bGUdMSHnN1giNfasGpnpLCFV
aQtGA0LtO4LHk2/k249M/xYL27kbgtz+9n4U1Kv+VcxFSLMftIa/D/WlZCDYgH56
nfIq/e2UieQRhQMoDxEI5a98UNOUwd921SocyS6ZqiLWAUSUkVb6om2fdCTMVKJk
0wVa/xEbOsLco2idgCg+Aa8z5tj9csAC0CCgfqQVAlgvaqgjsAQJynUNoVPEaGJy
qaEVc7vRawStzNyZBpDEx4q2hGo8UZeH5WHFKyS/x/t9sbXPCGrs0phRHa60mpsD
IBbw48TfUeBtQEkQf/hd
-----END CERTIFICATE REQUEST-----
```

>**IMPORTANTE**: no olvidar usar la opción `-utf8` _(input characters are UTF8 (default ASCII))_ cuando se van a introducir caracteres `especiales` (acentos, caracteres en alfabeto griego, cirílico, ...) como en el ejemplo superior `Common Name (e.g. server FQDN or YOUR name) []:Juan Pérez Martínez`

* Visualización utilizando el mismo comando openssl req con la opción `-text` en lugar de `-new`:

```
$ openssl req -text -in CSR_User01.pem -inform PEM
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=ES, ST=Madrid, L=Madrid, O=Empresa Ejemplo, S.A., OU=Administraci\xC3\xB3n PKI, CN=Juan P\xC3\xA9rez Mart\xC3\xADnez/emailAddress=juan.perez.martinez@empresa.ejemplo.es
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:a1:f6:4c:6c:6e:4f:87:49:53:83:48:44:81:
                    39:b6:af:4a:3e:14:81:a5:3a:a7:ff:7b:da:2e:b5:
                    09:6f:45:d0:3d:ac:c8:47:f9:99:f9:48:56:cc:fd:
                    f8:ff:9d:74:4c:07:1d:9d:a7:05:f9:19:94:f0:fc:
                    ca:eb:74:b1:c1:a3:4d:8e:a7:fc:f1:22:47:e5:ec:
                    c2:d5:e3:5a:44:87:d9:3b:c7:3d:08:57:eb:f3:c3:
                    0f:03:aa:05:f6:6e:68:6b:ea:a5:2a:7e:1c:d4:06:
                    00:c0:df:02:69:10:62:7f:42:6e:d5:a0:99:4c:f6:
                    6b:55:a1:1a:4b:bd:3a:de:4d:56:5b:14:42:ad:b1:
                    09:dd:74:82:2d:8d:74:27:1e:c3:62:95:9a:9e:46:
                    da:7b:2e:08:fc:b6:29:5d:4b:60:37:b7:d1:73:9b:
                    c6:3e:59:e8:14:90:25:b9:af:fc:b2:3a:9d:bf:e9:
                    8e:42:61:08:c7:86:92:8a:17:f9:ac:0e:ac:2e:15:
                    9e:30:1b:9c:71:18:0e:bd:9c:b9:34:72:48:6d:c3:
                    4c:ae:55:66:5f:d8:d3:e7:2c:c4:0c:7d:67:c4:54:
                    bd:95:15:e6:7d:16:ef:a1:c5:ed:14:92:90:28:95:
                    7f:c3:78:c0:65:d5:05:2f:7e:ca:ae:3d:42:0f:3f:
                    cc:53
                Exponent: 65537 (0x10001)
        Attributes:
            unstructuredName         :unable to print attribute
            challengePassword        :unable to print attribute
    Signature Algorithm: sha256WithRSAEncryption
         13:53:5d:71:de:74:5d:cb:fc:92:ca:09:05:db:04:cb:26:26:
         0a:90:f8:51:e0:ee:7a:cc:02:81:09:42:38:6c:65:1d:31:21:
         e7:37:58:22:35:f6:ac:1a:99:e9:2c:21:55:69:0b:46:03:42:
         ed:3b:82:c7:93:6f:e4:db:8f:4c:ff:16:0b:db:b9:1b:82:dc:
         fe:f6:7e:14:d4:ab:fe:55:cc:45:48:b3:1f:b4:86:bf:0f:f5:
         a5:64:20:d8:80:7e:7a:9d:f2:2a:fd:ed:94:89:e4:11:85:03:
         28:0f:11:08:e5:af:7c:50:d3:94:c1:df:76:d5:2a:1c:c9:2e:
         99:aa:22:d6:01:44:94:91:56:fa:a2:6d:9f:74:24:cc:54:a2:
         64:d3:05:5a:ff:11:1b:3a:c2:dc:a3:68:9d:80:28:3e:01:af:
         33:e6:d8:fd:72:c0:02:d0:20:a0:7e:a4:15:02:58:2f:6a:a8:
         23:b0:04:09:ca:75:0d:a1:53:c4:68:62:72:a9:a1:15:73:bb:
         d1:6b:04:ad:cc:dc:99:06:90:c4:c7:8a:b6:84:6a:3c:51:97:
         87:e5:61:c5:2b:24:bf:c7:fb:7d:b1:b5:cf:08:6a:ec:d2:98:
         51:1d:ae:b4:9a:9b:03:20:16:f0:e3:c4:df:51:e0:6d:40:49:
         10:7f:f8:5d
-----BEGIN CERTIFICATE REQUEST-----
MIIDOzCCAiMCAQAwgcQxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEeMBwGA1UECgwVRW1wcmVzYSBFamVtcGxvLCBTLkEuMRww
GgYDVQQLDBNBZG1pbmlzdHJhY2nDs24gUEtJMR4wHAYDVQQDDBVKdWFuIFDDqXJl
eiBNYXJ0w61uZXoxNTAzBgkqhkiG9w0BCQEWJmp1YW4ucGVyZXoubWFydGluZXpA
ZW1wcmVzYS5lamVtcGxvLmVzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAtqH2TGxuT4dJU4NIRIE5tq9KPhSBpTqn/3vaLrUJb0XQPazIR/mZ+UhWzP34
/510TAcdnacF+RmU8PzK63SxwaNNjqf88SJH5ezC1eNaRIfZO8c9CFfr88MPA6oF
9m5oa+qlKn4c1AYAwN8CaRBif0Ju1aCZTPZrVaEaS7063k1WWxRCrbEJ3XSCLY10
Jx7DYpWankbaey4I/LYpXUtgN7fRc5vGPlnoFJAlua/8sjqdv+mOQmEIx4aSihf5
rA6sLhWeMBuccRgOvZy5NHJIbcNMrlVmX9jT5yzEDH1nxFS9lRXmfRbvocXtFJKQ
KJV/w3jAZdUFL37Krj1CDz/MUwIDAQABoDEwFgYJKoZIhvcNAQkCMQkMB0VqZW1w
bG8wFwYJKoZIhvcNAQkHMQoMCGNoYW5nZWl0MA0GCSqGSIb3DQEBCwUAA4IBAQAT
U11x3nRdy/ySygkF2wTLJiYKkPhR4O56zAKBCUI4bGUdMSHnN1giNfasGpnpLCFV
aQtGA0LtO4LHk2/k249M/xYL27kbgtz+9n4U1Kv+VcxFSLMftIa/D/WlZCDYgH56
nfIq/e2UieQRhQMoDxEI5a98UNOUwd921SocyS6ZqiLWAUSUkVb6om2fdCTMVKJk
0wVa/xEbOsLco2idgCg+Aa8z5tj9csAC0CCgfqQVAlgvaqgjsAQJynUNoVPEaGJy
qaEVc7vRawStzNyZBpDEx4q2hGo8UZeH5WHFKyS/x/t9sbXPCGrs0phRHa60mpsD
IBbw48TfUeBtQEkQf/hd
-----END CERTIFICATE REQUEST-----
```

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

Cuando nos referimos a `CSR de Dominio` nos estamos refiriendo en realidad a un `CSR` que identifica a un `Dominio DNS`, habitualmente en la forma de la dirección `DNS` de un servidor `HTTPS` ó `FTPS`, y en general un servidor que expone su acceso a través de `TLS/SSL`:

Tipos de certificados: 
>(fuente: [Wikipedia (es) >> Infraestructura de clave pública: Tipos de Certificado]

Además, existen otros tipos de certificado digital utilizados en entornos más técnicos:

* Certificado de servidor seguro, utilizado en los servidores web que quieren proteger ante terceros el intercambio de información con los usuarios.
* Certificado de firma de código, para garantizar la autoría y la no modificación del código de aplicaciones informáticas.

### Paso 1. Generación del `Key Pair` (O utilización de uno que haya sido generado/almacenado prevamente)

>Nota: usaremos el mismo `Key Pair RSA` que el generado en el ejemplo `01.01` anterior

### Paso 2. Generación del `CSR - Certificate Signing Request`

* Generación del CSR:

```
$ openssl req -new -utf8 -sha256 -key rsakeypair01.pem -passin pass:changeit -out CSR_labtik122017.pem
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
Organization Name (eg, company) [Internet Widgits Pty Ltd]:TIK Techedge Institute of Knowledge
Organizational Unit Name (eg, section) []:Admin Lab Demo Servers
Common Name (e.g. server FQDN or YOUR name) []:labtik122017.techedgegroup.es
Email Address []:admin.tik.labs@tik.techedgegroup.es

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:changeit
An optional company name []:Techedge Institute of Knowledge
```

```
$ cat CSR_labtik122017.pem 
-----BEGIN CERTIFICATE REQUEST-----
MIIDaTCCAlECAQAwgdoxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEsMCoGA1UECgwjVElLIFRlY2hlZGdlIEluc3RpdHV0ZSBv
ZiBLbm93bGVkZ2UxHzAdBgNVBAsMFkFkbWluIExhYiBEZW1vIFNlcnZlcnMxJjAk
BgNVBAMMHWxhYnRpazEyMjAxNy50ZWNoZWRnZWdyb3VwLmVzMTIwMAYJKoZIhvcN
AQkBFiNhZG1pbi50aWsubGFic0B0aWsudGVjaGVkZ2Vncm91cC5lczCCASIwDQYJ
KoZIhvcNAQEBBQADggEPADCCAQoCggEBALah9kxsbk+HSVODSESBObavSj4UgaU6
p/972i61CW9F0D2syEf5mflIVsz9+P+ddEwHHZ2nBfkZlPD8yut0scGjTY6n/PEi
R+XswtXjWkSH2TvHPQhX6/PDDwOqBfZuaGvqpSp+HNQGAMDfAmkQYn9CbtWgmUz2
a1WhGku9Ot5NVlsUQq2xCd10gi2NdCcew2KVmp5G2nsuCPy2KV1LYDe30XObxj5Z
6BSQJbmv/LI6nb/pjkJhCMeGkooX+awOrC4VnjAbnHEYDr2cuTRySG3DTK5VZl/Y
0+csxAx9Z8RUvZUV5n0W76HF7RSSkCiVf8N4wGXVBS9+yq49Qg8/zFMCAwEAAaBJ
MBcGCSqGSIb3DQEJBzEKDAhjaGFuZ2VpdDAuBgkqhkiG9w0BCQIxIQwfVGVjaGVk
Z2UgSW5zdGl0dXRlIG9mIEtub3dsZWRnZTANBgkqhkiG9w0BAQsFAAOCAQEAhPR3
Ks+bAFQUfV5e8n41LyKCD5cJIxwF1cwpoXNtqYTEYlsDXT/uyNh04OjiZ/o+EeM2
ho5JinSx9iAMekQtSc3uMdHJ8BdOoBecRRn6bDhfLyoMhj98UPM6ntz3DuORxvFi
i4A7L2V/l4yIyED7JkHleWMEmSYpAVzdsdGLR5psJwFh8pPVQnB862ZNq2TFrdnM
TWRRdi/MheowUnEuPhpkkbVNtlPzKByCJZvrc72l2DTew4+mYmewt2VjXY0TQYBa
JlhloWNFlNbGCqTxtw0tUj7zXk7jt3pLAGwkl+kLqW7MND3ys0YuGg6jHW60wogA
aUc5NK+IGpGRx5+f5w==
-----END CERTIFICATE REQUEST-----
```

* Visualización utilizando el mismo comando openssl req con la opción `-text` en lugar de `-new`:

```
$ openssl req -text -in CSR_labtik122017.pem -inform PEM
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=ES, ST=Madrid, L=Madrid, O=TIK Techedge Institute of Knowledge, OU=Admin Lab Demo Servers, CN=labtik122017.techedgegroup.es/emailAddress=admin.tik.labs@tik.techedgegroup.es
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:a1:f6:4c:6c:6e:4f:87:49:53:83:48:44:81:
                    39:b6:af:4a:3e:14:81:a5:3a:a7:ff:7b:da:2e:b5:
                    09:6f:45:d0:3d:ac:c8:47:f9:99:f9:48:56:cc:fd:
                    f8:ff:9d:74:4c:07:1d:9d:a7:05:f9:19:94:f0:fc:
                    ca:eb:74:b1:c1:a3:4d:8e:a7:fc:f1:22:47:e5:ec:
                    c2:d5:e3:5a:44:87:d9:3b:c7:3d:08:57:eb:f3:c3:
                    0f:03:aa:05:f6:6e:68:6b:ea:a5:2a:7e:1c:d4:06:
                    00:c0:df:02:69:10:62:7f:42:6e:d5:a0:99:4c:f6:
                    6b:55:a1:1a:4b:bd:3a:de:4d:56:5b:14:42:ad:b1:
                    09:dd:74:82:2d:8d:74:27:1e:c3:62:95:9a:9e:46:
                    da:7b:2e:08:fc:b6:29:5d:4b:60:37:b7:d1:73:9b:
                    c6:3e:59:e8:14:90:25:b9:af:fc:b2:3a:9d:bf:e9:
                    8e:42:61:08:c7:86:92:8a:17:f9:ac:0e:ac:2e:15:
                    9e:30:1b:9c:71:18:0e:bd:9c:b9:34:72:48:6d:c3:
                    4c:ae:55:66:5f:d8:d3:e7:2c:c4:0c:7d:67:c4:54:
                    bd:95:15:e6:7d:16:ef:a1:c5:ed:14:92:90:28:95:
                    7f:c3:78:c0:65:d5:05:2f:7e:ca:ae:3d:42:0f:3f:
                    cc:53
                Exponent: 65537 (0x10001)
        Attributes:
            challengePassword        :unable to print attribute
            unstructuredName         :unable to print attribute
    Signature Algorithm: sha256WithRSAEncryption
         84:f4:77:2a:cf:9b:00:54:14:7d:5e:5e:f2:7e:35:2f:22:82:
         0f:97:09:23:1c:05:d5:cc:29:a1:73:6d:a9:84:c4:62:5b:03:
         5d:3f:ee:c8:d8:74:e0:e8:e2:67:fa:3e:11:e3:36:86:8e:49:
         8a:74:b1:f6:20:0c:7a:44:2d:49:cd:ee:31:d1:c9:f0:17:4e:
         a0:17:9c:45:19:fa:6c:38:5f:2f:2a:0c:86:3f:7c:50:f3:3a:
         9e:dc:f7:0e:e3:91:c6:f1:62:8b:80:3b:2f:65:7f:97:8c:88:
         c8:40:fb:26:41:e5:79:63:04:99:26:29:01:5c:dd:b1:d1:8b:
         47:9a:6c:27:01:61:f2:93:d5:42:70:7c:eb:66:4d:ab:64:c5:
         ad:d9:cc:4d:64:51:76:2f:cc:85:ea:30:52:71:2e:3e:1a:64:
         91:b5:4d:b6:53:f3:28:1c:82:25:9b:eb:73:bd:a5:d8:34:de:
         c3:8f:a6:62:67:b0:b7:65:63:5d:8d:13:41:80:5a:26:58:65:
         a1:63:45:94:d6:c6:0a:a4:f1:b7:0d:2d:52:3e:f3:5e:4e:e3:
         b7:7a:4b:00:6c:24:97:e9:0b:a9:6e:cc:34:3d:f2:b3:46:2e:
         1a:0e:a3:1d:6e:b4:c2:88:00:69:47:39:34:af:88:1a:91:91:
         c7:9f:9f:e7
-----BEGIN CERTIFICATE REQUEST-----
MIIDaTCCAlECAQAwgdoxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEsMCoGA1UECgwjVElLIFRlY2hlZGdlIEluc3RpdHV0ZSBv
ZiBLbm93bGVkZ2UxHzAdBgNVBAsMFkFkbWluIExhYiBEZW1vIFNlcnZlcnMxJjAk
BgNVBAMMHWxhYnRpazEyMjAxNy50ZWNoZWRnZWdyb3VwLmVzMTIwMAYJKoZIhvcN
AQkBFiNhZG1pbi50aWsubGFic0B0aWsudGVjaGVkZ2Vncm91cC5lczCCASIwDQYJ
KoZIhvcNAQEBBQADggEPADCCAQoCggEBALah9kxsbk+HSVODSESBObavSj4UgaU6
p/972i61CW9F0D2syEf5mflIVsz9+P+ddEwHHZ2nBfkZlPD8yut0scGjTY6n/PEi
R+XswtXjWkSH2TvHPQhX6/PDDwOqBfZuaGvqpSp+HNQGAMDfAmkQYn9CbtWgmUz2
a1WhGku9Ot5NVlsUQq2xCd10gi2NdCcew2KVmp5G2nsuCPy2KV1LYDe30XObxj5Z
6BSQJbmv/LI6nb/pjkJhCMeGkooX+awOrC4VnjAbnHEYDr2cuTRySG3DTK5VZl/Y
0+csxAx9Z8RUvZUV5n0W76HF7RSSkCiVf8N4wGXVBS9+yq49Qg8/zFMCAwEAAaBJ
MBcGCSqGSIb3DQEJBzEKDAhjaGFuZ2VpdDAuBgkqhkiG9w0BCQIxIQwfVGVjaGVk
Z2UgSW5zdGl0dXRlIG9mIEtub3dsZWRnZTANBgkqhkiG9w0BAQsFAAOCAQEAhPR3
Ks+bAFQUfV5e8n41LyKCD5cJIxwF1cwpoXNtqYTEYlsDXT/uyNh04OjiZ/o+EeM2
ho5JinSx9iAMekQtSc3uMdHJ8BdOoBecRRn6bDhfLyoMhj98UPM6ntz3DuORxvFi
i4A7L2V/l4yIyED7JkHleWMEmSYpAVzdsdGLR5psJwFh8pPVQnB862ZNq2TFrdnM
TWRRdi/MheowUnEuPhpkkbVNtlPzKByCJZvrc72l2DTew4+mYmewt2VjXY0TQYBa
JlhloWNFlNbGCqTxtw0tUj7zXk7jt3pLAGwkl+kLqW7MND3ys0YuGg6jHW60wogA
aUc5NK+IGpGRx5+f5w==
-----END CERTIFICATE REQUEST-----
```

* Visualización del CSR con `grc-viewer`

```
$ gcr-viewer CSR_labtik122017.pem
```

![gcr-viewer CSR_labtik122017.pem](images/gcr-viewer-CSR_Server01_image01.png "gcr-viewer CSR_labtik122017.pem")

* Visualización con la herramienta en-línea >[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder02_img01.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder02_img02.png "https://redkestrel.co.uk/products/decoder/")


* Visualización del CSR con `openssl asn1parse`

```
$ openssl asn1parse -i -dump -in CSR_labtik122017.pem -inform PEM
    0:d=0  hl=4 l= 873 cons: SEQUENCE          
    4:d=1  hl=4 l= 593 cons:  SEQUENCE          
    8:d=2  hl=2 l=   1 prim:   INTEGER           :00
   11:d=2  hl=3 l= 218 cons:   SEQUENCE          
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
   61:d=3  hl=2 l=  44 cons:    SET               
   63:d=4  hl=2 l=  42 cons:     SEQUENCE          
   65:d=5  hl=2 l=   3 prim:      OBJECT            :organizationName
   70:d=5  hl=2 l=  35 prim:      UTF8STRING        :TIK Techedge Institute of Knowledge
  107:d=3  hl=2 l=  31 cons:    SET               
  109:d=4  hl=2 l=  29 cons:     SEQUENCE          
  111:d=5  hl=2 l=   3 prim:      OBJECT            :organizationalUnitName
  116:d=5  hl=2 l=  22 prim:      UTF8STRING        :Admin Lab Demo Servers
  140:d=3  hl=2 l=  38 cons:    SET               
  142:d=4  hl=2 l=  36 cons:     SEQUENCE          
  144:d=5  hl=2 l=   3 prim:      OBJECT            :commonName
  149:d=5  hl=2 l=  29 prim:      UTF8STRING        :labtik122017.techedgegroup.es
  180:d=3  hl=2 l=  50 cons:    SET               
  182:d=4  hl=2 l=  48 cons:     SEQUENCE          
  184:d=5  hl=2 l=   9 prim:      OBJECT            :emailAddress
  195:d=5  hl=2 l=  35 prim:      IA5STRING         :admin.tik.labs@tik.techedgegroup.es
  232:d=2  hl=4 l= 290 cons:   SEQUENCE          
  236:d=3  hl=2 l=  13 cons:    SEQUENCE          
  238:d=4  hl=2 l=   9 prim:     OBJECT            :rsaEncryption
  249:d=4  hl=2 l=   0 prim:     NULL              
  251:d=3  hl=4 l= 271 prim:    BIT STRING        
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
  526:d=2  hl=2 l=  73 cons:   cont [ 0 ]        
  528:d=3  hl=2 l=  23 cons:    SEQUENCE          
  530:d=4  hl=2 l=   9 prim:     OBJECT            :challengePassword
  541:d=4  hl=2 l=  10 cons:     SET               
  543:d=5  hl=2 l=   8 prim:      UTF8STRING        :changeit
  553:d=3  hl=2 l=  46 cons:    SEQUENCE          
  555:d=4  hl=2 l=   9 prim:     OBJECT            :unstructuredName
  566:d=4  hl=2 l=  33 cons:     SET               
  568:d=5  hl=2 l=  31 prim:      UTF8STRING        :Techedge Institute of Knowledge
  601:d=1  hl=2 l=  13 cons:  SEQUENCE          
  603:d=2  hl=2 l=   9 prim:   OBJECT            :sha256WithRSAEncryption
  614:d=2  hl=2 l=   0 prim:   NULL              
  616:d=1  hl=4 l= 257 prim:  BIT STRING        
      0000 - 00 84 f4 77 2a cf 9b 00-54 14 7d 5e 5e f2 7e 35   ...w*...T.}^^.~5
      0010 - 2f 22 82 0f 97 09 23 1c-05 d5 cc 29 a1 73 6d a9   /"....#....).sm.
      0020 - 84 c4 62 5b 03 5d 3f ee-c8 d8 74 e0 e8 e2 67 fa   ..b[.]?...t...g.
      0030 - 3e 11 e3 36 86 8e 49 8a-74 b1 f6 20 0c 7a 44 2d   >..6..I.t.. .zD-
      0040 - 49 cd ee 31 d1 c9 f0 17-4e a0 17 9c 45 19 fa 6c   I..1....N...E..l
      0050 - 38 5f 2f 2a 0c 86 3f 7c-50 f3 3a 9e dc f7 0e e3   8_/*..?|P.:.....
      0060 - 91 c6 f1 62 8b 80 3b 2f-65 7f 97 8c 88 c8 40 fb   ...b..;/e.....@.
      0070 - 26 41 e5 79 63 04 99 26-29 01 5c dd b1 d1 8b 47   &A.yc..&).\....G
      0080 - 9a 6c 27 01 61 f2 93 d5-42 70 7c eb 66 4d ab 64   .l'.a...Bp|.fM.d
      0090 - c5 ad d9 cc 4d 64 51 76-2f cc 85 ea 30 52 71 2e   ....MdQv/...0Rq.
      00a0 - 3e 1a 64 91 b5 4d b6 53-f3 28 1c 82 25 9b eb 73   >.d..M.S.(..%..s
      00b0 - bd a5 d8 34 de c3 8f a6-62 67 b0 b7 65 63 5d 8d   ...4....bg..ec].
      00c0 - 13 41 80 5a 26 58 65 a1-63 45 94 d6 c6 0a a4 f1   .A.Z&Xe.cE......
      00d0 - b7 0d 2d 52 3e f3 5e 4e-e3 b7 7a 4b 00 6c 24 97   ..-R>.^N..zK.l$.
      00e0 - e9 0b a9 6e cc 34 3d f2-b3 46 2e 1a 0e a3 1d 6e   ...n.4=..F.....n
      00f0 - b4 c2 88 00 69 47 39 34-af 88 1a 91 91 c7 9f 9f   ....iG94........
      0100 - e7                                                .

```

## 01.04 `CSR` de `Usuario` EXTENDIDO 

### Paso 1. Generación del `Key Pair` (O utilización de uno que haya sido generado/almacenado prevamente)

>Nota: usaremos el mismo `Key Pair RSA` que el generado en el ejemplo `01.01` anterior

### Paso 2. Generación del `CSR - Certificate Signing Request`

* Adaptación del archivo de configuración de OpenSSL

Cuando se requiere _ampliar_ el numero de elementos del `DN - Distinguish Name` para que el `prompt` de `openssl req` lo incluya es preciso _ampliar_ la sección `[ req_distinguished_name ]` del archivo de configuración `/etc/ssl/openssl.cnf`  

```
$ sudo vim /etc/ssl/openssl.cnf

... ... ... ... ...
# req_extensions = v3_req # The extensions to add to a certificate request

[ req_distinguished_name ]
countryName                     = Country Name (2 letter code)
countryName_default             = AU
countryName_min                 = 2
countryName_max                 = 2

stateOrProvinceName             = State or Province Name (full name)
stateOrProvinceName_default     = Some-State

localityName                    = Locality Name (eg, city)

0.organizationName              = Organization Name (eg, company)
0.organizationName_default      = Internet Widgits Pty Ltd

# we can do this but it is not needed normally :-)
#1.organizationName             = Second Organization Name (eg, company)
#1.organizationName_default     = World Wide Web Pty Ltd

organizationalUnitName          = Organizational Unit Name (eg, section)
#organizationalUnitName_default =

commonName                      = Common Name (e.g. server FQDN or YOUR name)
commonName_max                  = 64

emailAddress                    = Email Address
emailAddress_max                = 64

serialNumber                    = Serial Number (NIF)
serialNumber_max                = 10

... ... ... ... ...
```

Nótese específicamente las dos líneas incluidas en la sección:
```
serialNumber                    = Serial Number (NIF)
serialNumber_max                = 10
```

* Generación del CSR con el elemento `Serial Number` incluido en el `DN - Distinguished Name` que utilizaremos pra almacenar el `NIF` de la Persona/Usuario Subject Alternative Names`

>**OBSERVESE** el efecto sobre el `prompt` ampliado de `openssl req` al _ampliar_ la sección `[ req_distinguished_name ]` del archivo de configuración `/etc/ssl/openssl.cnf`

```
$ openssl req -new -utf8 -sha256 -key rsakeypair01.pem -passin pass:changeit -out CSR_User01_NIF.pem
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
Serial Number (NIF) []:123456789X

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:changeit
An optional company name []:Ejemplo, S.A.
```

NOTESE la nueva línea en el  `prompt` ampliado de `openssl req`:
```
Serial Number (NIF) []:123456789X
```

* Visualización utilizando el mismo comando openssl req con la opción `-text` en lugar de `-new`:

```
$ openssl req -text -in CSR_User01_NIF.pem -inform PEM
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=ES, ST=Madrid, L=Madrid, O=Empresa Ejemplo, S.A., OU=Administraci\xC3\xB3n PKI, CN=Juan P\xC3\xA9rez Mart\xC3\xADnez/emailAddress=juan.perez.martinez@empresa.ejemplo.es/serialNumber=123456789X
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:a1:f6:4c:6c:6e:4f:87:49:53:83:48:44:81:
                    39:b6:af:4a:3e:14:81:a5:3a:a7:ff:7b:da:2e:b5:
                    09:6f:45:d0:3d:ac:c8:47:f9:99:f9:48:56:cc:fd:
                    f8:ff:9d:74:4c:07:1d:9d:a7:05:f9:19:94:f0:fc:
                    ca:eb:74:b1:c1:a3:4d:8e:a7:fc:f1:22:47:e5:ec:
                    c2:d5:e3:5a:44:87:d9:3b:c7:3d:08:57:eb:f3:c3:
                    0f:03:aa:05:f6:6e:68:6b:ea:a5:2a:7e:1c:d4:06:
                    00:c0:df:02:69:10:62:7f:42:6e:d5:a0:99:4c:f6:
                    6b:55:a1:1a:4b:bd:3a:de:4d:56:5b:14:42:ad:b1:
                    09:dd:74:82:2d:8d:74:27:1e:c3:62:95:9a:9e:46:
                    da:7b:2e:08:fc:b6:29:5d:4b:60:37:b7:d1:73:9b:
                    c6:3e:59:e8:14:90:25:b9:af:fc:b2:3a:9d:bf:e9:
                    8e:42:61:08:c7:86:92:8a:17:f9:ac:0e:ac:2e:15:
                    9e:30:1b:9c:71:18:0e:bd:9c:b9:34:72:48:6d:c3:
                    4c:ae:55:66:5f:d8:d3:e7:2c:c4:0c:7d:67:c4:54:
                    bd:95:15:e6:7d:16:ef:a1:c5:ed:14:92:90:28:95:
                    7f:c3:78:c0:65:d5:05:2f:7e:ca:ae:3d:42:0f:3f:
                    cc:53
                Exponent: 65537 (0x10001)
        Attributes:
            challengePassword        :unable to print attribute
            unstructuredName         :unable to print attribute
    Signature Algorithm: sha256WithRSAEncryption
         95:56:8e:35:fa:0d:2c:24:25:90:12:55:b6:70:fb:7c:89:c6:
         bf:b2:9b:f1:c5:99:0c:09:dd:42:b1:df:12:65:63:02:34:44:
         3c:48:38:3a:1d:6e:ca:79:5d:90:14:3b:27:8e:ad:3a:d2:54:
         76:38:94:47:5d:34:72:71:03:a0:b6:aa:a5:22:50:60:7b:c9:
         cd:91:0c:0b:15:c5:87:06:b6:36:a2:18:fc:a2:82:5d:cd:ba:
         5f:e7:b0:79:9f:59:e0:fc:0f:8e:08:7b:26:69:a0:06:42:84:
         85:68:7c:19:5f:4a:9d:f2:5a:0e:44:3e:2e:23:74:99:79:80:
         d7:98:a4:36:11:43:43:0d:32:a9:cf:f1:30:bb:3e:c4:35:6d:
         51:e5:ff:c2:4f:e4:cd:4e:1e:91:0b:b9:32:bc:77:44:bd:8b:
         61:70:4b:c7:36:41:7f:b4:6a:8d:b3:87:5f:e6:85:8a:91:d9:
         cd:0e:e4:4b:46:8d:27:5a:6a:fa:8a:60:09:bc:37:dc:fa:9a:
         0d:e8:7b:51:fd:d7:8e:37:9f:ed:ee:ab:7b:1d:d8:42:d9:95:
         78:a7:3b:07:a8:47:fb:3e:37:ad:1b:81:26:8d:b6:47:6d:65:
         19:76:c1:14:89:0e:26:0a:c7:81:5b:12:4c:a1:e7:d8:62:3d:
         a1:f2:34:b8
-----BEGIN CERTIFICATE REQUEST-----
MIIDVjCCAj4CAQAwgdkxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEeMBwGA1UECgwVRW1wcmVzYSBFamVtcGxvLCBTLkEuMRww
GgYDVQQLDBNBZG1pbmlzdHJhY2nDs24gUEtJMR4wHAYDVQQDDBVKdWFuIFDDqXJl
eiBNYXJ0w61uZXoxNTAzBgkqhkiG9w0BCQEWJmp1YW4ucGVyZXoubWFydGluZXpA
ZW1wcmVzYS5lamVtcGxvLmVzMRMwEQYDVQQFEwoxMjM0NTY3ODlYMIIBIjANBgkq
hkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtqH2TGxuT4dJU4NIRIE5tq9KPhSBpTqn
/3vaLrUJb0XQPazIR/mZ+UhWzP34/510TAcdnacF+RmU8PzK63SxwaNNjqf88SJH
5ezC1eNaRIfZO8c9CFfr88MPA6oF9m5oa+qlKn4c1AYAwN8CaRBif0Ju1aCZTPZr
VaEaS7063k1WWxRCrbEJ3XSCLY10Jx7DYpWankbaey4I/LYpXUtgN7fRc5vGPlno
FJAlua/8sjqdv+mOQmEIx4aSihf5rA6sLhWeMBuccRgOvZy5NHJIbcNMrlVmX9jT
5yzEDH1nxFS9lRXmfRbvocXtFJKQKJV/w3jAZdUFL37Krj1CDz/MUwIDAQABoDcw
FwYJKoZIhvcNAQkHMQoMCGNoYW5nZWl0MBwGCSqGSIb3DQEJAjEPDA1FamVtcGxv
LCBTLkEuMA0GCSqGSIb3DQEBCwUAA4IBAQCVVo41+g0sJCWQElW2cPt8ica/spvx
xZkMCd1Csd8SZWMCNEQ8SDg6HW7KeV2QFDsnjq060lR2OJRHXTRycQOgtqqlIlBg
e8nNkQwLFcWHBrY2ohj8ooJdzbpf57B5n1ng/A+OCHsmaaAGQoSFaHwZX0qd8loO
RD4uI3SZeYDXmKQ2EUNDDTKpz/Ewuz7ENW1R5f/CT+TNTh6RC7kyvHdEvYthcEvH
NkF/tGqNs4df5oWKkdnNDuRLRo0nWmr6imAJvDfc+poN6HtR/deON5/t7qt7HdhC
2ZV4pzsHqEf7PjetG4EmjbZHbWUZdsEUiQ4mCseBWxJMoefYYj2h8jS4
-----END CERTIFICATE REQUEST-----
```
>**ADVIERTASE** el fragmento `ejemplo.es/serialNumber=123456789X`

* Visualización del CSR con `grc-viewer`

```
$ gcr-viewer CSR_User01_NIF.pem
```

![gcr-viewer CSR_User01_NIF.pem](images/gcr-viewer-CSR_UserNIF_image01.png "gcr-viewer CSR_User01_NIF.pem")

* Visualización con la herramienta en-línea >[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder03_img01.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder03_img02.png "https://redkestrel.co.uk/products/decoder/")

### INTERLUDIO: ATRIBUTOS POSIBLES EN UN `DN - Distinguished Name`

>Ver [LDAP DNs and RDNs](https://www.ldap.com/ldap-dns-and-rdns)

>Ver [Oracle - Understanding the LDAP Binding Component](https://docs.oracle.com/cd/E19182-01/820-6573/ghusi/index.html)

>[Reference record for OID 2.5.4](http://oidref.com/2.5.4)  _'Attribute type. For more information, see Rec. ITU-T X.501 | ISO/IEC 9594-2.'_

>[Ver OID 2.5.4 en oid-info.com](http://oid-info.com/get/2.5.4) _'Attribute type. For more information, see Rec. ITU-T X.501 | ISO/IEC 9594-2.'_

|OID|Name|Description|
|---|----|-----------|
|2.5.4.0|objectClass|Object classes|
|2.5.4.1|aliasedEntryName|Attribute type "Aliased entry name"|
|2.5.4.2|knowledgeInformation|knowledgeInformation attribute type|
|2.5.4.3|commonName|Common name|
|2.5.4.4|surname|Attribute "surname"|
|2.5.4.5|serialNumber|Serial number attribute type|
|2.5.4.6|countryName|Country name|
|2.5.4.7|localityName|Locality Name|
|2.5.4.8|stateOrProvinceName|State or Province name|
|2.5.4.9|streetAddress|Street address|
|2.5.4.10|organizationName|Organization name|
|2.5.4.11|organizationUnitName|Organization unit name|
|2.5.4.12|title|Title attribute type|
|2.5.4.13|description|Description attribute type|
|2.5.4.14|searchGuide|Search guide attribute type|
|2.5.4.15|businessCategory|Business category attribute type|
|2.5.4.16|postalAddress|Postal address attribute type|
|2.5.4.17|postalCode|Postal code attribute type|
|2.5.4.18|postOfficeBox|Post office box attribute type|
|2.5.4.19|physicalDeliveryOfficeName|physicalDeliveryOfficeName attribute type|
|2.5.4.20|telephoneNumber|Telephone number attribute type|
|2.5.4.21|telexNumber|Telex number attribute type|
|2.5.4.22|teletexTerminalIdentifier|Teletex terminal identifier attribute type|
|2.5.4.23|facsimileTelephoneNumber|Facsimile telephone number attribute type|
|2.5.4.24|x121Address|X121 address attribute type|
|2.5.4.25|internationalISDNNumber|International ISDN (Integrated Services Digital Network) number attribute type|
|2.5.4.26|registeredAddress|Registered address attribute type|
|2.5.4.27|destinationIndicator|Destination indicator attribute type|
|2.5.4.28|preferredDeliveryMethod|Preferred delivery method attribute type|
|2.5.4.29|presentationAddress|Presentation address attribute type|
|2.5.4.30|supportedApplicationContext|Supported application context attribute type|
|2.5.4.31|member|Member attribute type|
|2.5.4.32|owner|Owner attribute type|
|2.5.4.33|roleOccupant|Role occupant attribute type|
|2.5.4.34|seeAlso|seeAlso attribute type|
|2.5.4.35|userPassword|userPassword attribute type|
|2.5.4.36|userCertificate|userCertificate attribute type|
|2.5.4.37|cACertificate|cAcertificate attribute type|
|2.5.4.38|authorityRevocationList|authorityRevocationList attribute type|
|2.5.4.39|certificateRevocationList|certificateRevocationList attribute type|
|2.5.4.40|crossCertificatePair|crossCertificatePair attribute type|
|2.5.4.41|name|Name attribute type|
|2.5.4.42|givenName|Given name attribute type|
|2.5.4.43|initials|Initials attribute type|
|2.5.4.44|generationQualifier|generationQualifier attribute type|
|2.5.4.45|uniqueIdentifier|uniqueIdentifier attribute type|
|2.5.4.46|dnQualifier|dnQualifier attribute type|
|2.5.4.47|enhancedSearchGuide|enhancedSearchGuide attribute type|
|2.5.4.48|protocolInformation|protocolInformation attribute type|
|2.5.4.49|distinguishedName|distinguishedName attribute type|
|2.5.4.50|uniqueMember|uniqueMember attribute type|
|2.5.4.51|houseIdentifier|houseIdentifier attribute type|
|2.5.4.52|supportedAlgorithms|supportedAlgorithms attribute type|
|2.5.4.53|deltaRevocationList|deltaRevocationList attribute type|
|2.5.4.54|dmdName|DMD Name attribute type|
|2.5.4.55|clearance|Attribute type "Clearance"|
|2.5.4.56|defaultDirQop|Attribute type "Default Dir Qop"|
|2.5.4.57|attributeIntegrityInfo|Attribute type "Attribute integrity info"|
|2.5.4.58|attributeCertificate|attributeCertificate attribute type|
|2.5.4.59|attributeCertificateRevocationList|attributeCertificateRevocationList attribute type|
|2.5.4.60|confKeyInfo|Attribute type "Conf key info"|
|2.5.4.61|aACertificate|aACertificate attribute type|
|2.5.4.62|attributeDescriptorCertificate|attributeDescriptorCertificate attribute type|
|2.5.4.63|attributeAuthorityRevocationList|attributeAuthorityRevocationList attribute type|
|2.5.4.64|family-information|Family-information attribute type|
|2.5.4.65|pseudonym|Pseudonym attribute type|
|2.5.4.66|communicationsService|communicationsService attribute type|
|2.5.4.67|communicationsNetwork|communicationsNetwork attribute type|
|2.5.4.68|certificationPracticeStmt|certificationPracticeStmt attribute type (Certification practice statement attribute)|
|2.5.4.69|certificatePolicy|certificatePolicy attribute type|
|2.5.4.70|pkiPath|pkiPath attribute type|
|2.5.4.71|privPolicy|privPolicy attribute type|
|2.5.4.72|role|role attribute type|
|2.5.4.73|delegationPath|delegationPath attribute type|
|2.5.4.74|protPrivPolicy|protPrivPolicy ATTRIBUTE ::= {|
|2.5.4.75|xMLPrivilegeInfo|xmlPrivilegeInfo ATTRIBUTE ::= {|
|2.5.4.76|xmlPrivPolicy|None|
|2.5.4.77|uuidpair|uUIDPair|
|2.5.4.78|tagOid|tagOid ATTRIBUTE ::= {|
|2.5.4.79|uiiFormat|uiiFormat ATTRIBUTE ::= {|
|2.5.4.80|uiiInUrh|uiiInUrn ATTRIBUTE ::= {|
|2.5.4.81|contentUrl|contentUrl ATTRIBUTE ::= {|
|2.5.4.82|permission|permission ATTRIBUTE ::= {|
|2.5.4.83|uri|uri ATTRIBUTE ::= {|
|2.5.4.84|pwdAttribute|pwdAttribute ATTRIBUTE ::= {|
|2.5.4.85|userPwd|userPwd ATTRIBUTE ::= {|
|2.5.4.86|urn|urn ATTRIBUTE ::= {|
|2.5.4.87|url|url ATTRIBUTE ::= {|
|2.5.4.88|utmCoordinates|utmCoordinates ATTRIBUTE ::= {|
|2.5.4.89|urnC|urnC ATTRIBUTE ::= {|
|2.5.4.90|uii|uii ATTRIBUTE ::= {|
|2.5.4.91|epc|epc ATTRIBUTE ::= {|
|2.5.4.92|tagAfi|tagAfi ATTRIBUTE ::= {|
|2.5.4.93|epcFormat|epcFormat ATTRIBUTE ::= {|
|2.5.4.94|epcInUrn|epcInUrn ATTRIBUTE ::= {|
|2.5.4.95|ldapUrl|ldapUrl ATTRIBUTE ::= {|
|2.5.4.96|ldapUrl|tagLocation ATTRIBUTE ::= {|
|2.5.4.97|organizationIdentifier|organizationIdentifier ATTRIBUTE ::= {|


## 01.05 `CSR` de `Dominio` (de `Servidor`) EXTENDIDO 

### Paso 1. Generación del `Key Pair` (O utilización de uno que haya sido generado/almacenado prevamente)

>Nota: usaremos el mismo `Key Pair RSA` que el generado en el ejemplo `01.01` anterior

### Paso 2. Generación del `CSR - Certificate Signing Request`

En ocasiones necesitamos incluir en el CSR algunos `atributos extendidos` como es el caso de un `Subject Alternative Names` para poder indicar `Nombres Alternativos` del dominio/servidor que se desean queden cubiertos en el futuro Certificado X.509.

* Generación del CSR con `Subject Alternative Names`

```
$ printf '[SAN]\nsubjectAltName=DNS:labtik122017,DNS:labtik122017.techedgegroup.es\n' > config_san_local.cnf && cat config_san_local.cnf 
[SAN]
subjectAltName=DNS:labtik122017,DNS:labtik122017.techedgegroup.es

$ cat /etc/ssl/openssl.cnf config_san_local.cnf > openssl_config_san_local.cnf && rm config_san_local.cnf

$ tail -5 openssl_config_san_local.cnf
				# (optional, default: no)
ess_cert_id_chain	= no	# Must the ESS cert id chain be included?
				# (optional, default: no)
[SAN]
subjectAltName=DNS:labtik122017,DNS:labtik122017.techedgegroup.es

$ openssl req -new -utf8 -sha256 \
> -key rsakeypair01.pem -passin pass:changeit \
> -subj "/C=ES/ST=Madrid/L=Madrid/O=TIK Techedge Institute of Knowledge/OU=Admin Lab Demo Servers/CN=labtik122017.techedgegroup.es" \
> -reqexts SAN -extensions SAN -config openssl_config_san_local.cnf \
> -out CSR_labtik122017_SAN.pem

$ cat CSR_labtik122017_SAN.pem
-----BEGIN CERTIFICATE REQUEST-----
MIIDNTCCAh0CAQAwgaYxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEsMCoGA1UECgwjVElLIFRlY2hlZGdlIEluc3RpdHV0ZSBv
ZiBLbm93bGVkZ2UxHzAdBgNVBAsMFkFkbWluIExhYiBEZW1vIFNlcnZlcnMxJjAk
BgNVBAMMHWxhYnRpazEyMjAxNy50ZWNoZWRnZWdyb3VwLmVzMIIBIjANBgkqhkiG
9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtqH2TGxuT4dJU4NIRIE5tq9KPhSBpTqn/3va
LrUJb0XQPazIR/mZ+UhWzP34/510TAcdnacF+RmU8PzK63SxwaNNjqf88SJH5ezC
1eNaRIfZO8c9CFfr88MPA6oF9m5oa+qlKn4c1AYAwN8CaRBif0Ju1aCZTPZrVaEa
S7063k1WWxRCrbEJ3XSCLY10Jx7DYpWankbaey4I/LYpXUtgN7fRc5vGPlnoFJAl
ua/8sjqdv+mOQmEIx4aSihf5rA6sLhWeMBuccRgOvZy5NHJIbcNMrlVmX9jT5yzE
DH1nxFS9lRXmfRbvocXtFJKQKJV/w3jAZdUFL37Krj1CDz/MUwIDAQABoEkwRwYJ
KoZIhvcNAQkOMTowODA2BgNVHREELzAtggxsYWJ0aWsxMjIwMTeCHWxhYnRpazEy
MjAxNy50ZWNoZWRnZWdyb3VwLmVzMA0GCSqGSIb3DQEBCwUAA4IBAQAi7pu4PMgx
YCtp0VwA1hXfk554M25RoDzSoY4a49ypN/9LQn2ecXhHwdYZdVY8XPOw3UQWKDf0
naJ0r14C2JfeCDv9ZyLnunTTovhzHTNLPm89+tSz63IhF4MdqWzKoH1dfNyJYbEW
p8Iv7tOXWjem+0KpwWtEprPVlAKv6Z6J4cL4dD3Hy9Hpse9xJiRjqYULl40sAyc+
sw613JDwIO1PamDGkDLebITEXZbhTyvguXu8ZLT758SvJGmc43p7WySG5ewUnMDJ
SXQwVKpcFUwp+7UIij99CnWDEVN+TDE276VCOH2Ee2tF/1JaLDTSsZ5rOk+78u9c
s2eJSzX601UB
-----END CERTIFICATE REQUEST-----
```

* Visualización utilizando el mismo comando `openssl req` con la opción `-text` en lugar de `-new`:

```
$ openssl req -text -in CSR_labtik122017_SAN.pem -inform PEM
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=ES, ST=Madrid, L=Madrid, O=TIK Techedge Institute of Knowledge, OU=Admin Lab Demo Servers, CN=labtik122017.techedgegroup.es
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:a1:f6:4c:6c:6e:4f:87:49:53:83:48:44:81:
                    39:b6:af:4a:3e:14:81:a5:3a:a7:ff:7b:da:2e:b5:
                    09:6f:45:d0:3d:ac:c8:47:f9:99:f9:48:56:cc:fd:
                    f8:ff:9d:74:4c:07:1d:9d:a7:05:f9:19:94:f0:fc:
                    ca:eb:74:b1:c1:a3:4d:8e:a7:fc:f1:22:47:e5:ec:
                    c2:d5:e3:5a:44:87:d9:3b:c7:3d:08:57:eb:f3:c3:
                    0f:03:aa:05:f6:6e:68:6b:ea:a5:2a:7e:1c:d4:06:
                    00:c0:df:02:69:10:62:7f:42:6e:d5:a0:99:4c:f6:
                    6b:55:a1:1a:4b:bd:3a:de:4d:56:5b:14:42:ad:b1:
                    09:dd:74:82:2d:8d:74:27:1e:c3:62:95:9a:9e:46:
                    da:7b:2e:08:fc:b6:29:5d:4b:60:37:b7:d1:73:9b:
                    c6:3e:59:e8:14:90:25:b9:af:fc:b2:3a:9d:bf:e9:
                    8e:42:61:08:c7:86:92:8a:17:f9:ac:0e:ac:2e:15:
                    9e:30:1b:9c:71:18:0e:bd:9c:b9:34:72:48:6d:c3:
                    4c:ae:55:66:5f:d8:d3:e7:2c:c4:0c:7d:67:c4:54:
                    bd:95:15:e6:7d:16:ef:a1:c5:ed:14:92:90:28:95:
                    7f:c3:78:c0:65:d5:05:2f:7e:ca:ae:3d:42:0f:3f:
                    cc:53
                Exponent: 65537 (0x10001)
        Attributes:
        Requested Extensions:
            X509v3 Subject Alternative Name: 
                DNS:labtik122017, DNS:labtik122017.techedgegroup.es
    Signature Algorithm: sha256WithRSAEncryption
         22:ee:9b:b8:3c:c8:31:60:2b:69:d1:5c:00:d6:15:df:93:9e:
         78:33:6e:51:a0:3c:d2:a1:8e:1a:e3:dc:a9:37:ff:4b:42:7d:
         9e:71:78:47:c1:d6:19:75:56:3c:5c:f3:b0:dd:44:16:28:37:
         f4:9d:a2:74:af:5e:02:d8:97:de:08:3b:fd:67:22:e7:ba:74:
         d3:a2:f8:73:1d:33:4b:3e:6f:3d:fa:d4:b3:eb:72:21:17:83:
         1d:a9:6c:ca:a0:7d:5d:7c:dc:89:61:b1:16:a7:c2:2f:ee:d3:
         97:5a:37:a6:fb:42:a9:c1:6b:44:a6:b3:d5:94:02:af:e9:9e:
         89:e1:c2:f8:74:3d:c7:cb:d1:e9:b1:ef:71:26:24:63:a9:85:
         0b:97:8d:2c:03:27:3e:b3:0e:b5:dc:90:f0:20:ed:4f:6a:60:
         c6:90:32:de:6c:84:c4:5d:96:e1:4f:2b:e0:b9:7b:bc:64:b4:
         fb:e7:c4:af:24:69:9c:e3:7a:7b:5b:24:86:e5:ec:14:9c:c0:
         c9:49:74:30:54:aa:5c:15:4c:29:fb:b5:08:8a:3f:7d:0a:75:
         83:11:53:7e:4c:31:36:ef:a5:42:38:7d:84:7b:6b:45:ff:52:
         5a:2c:34:d2:b1:9e:6b:3a:4f:bb:f2:ef:5c:b3:67:89:4b:35:
         fa:d3:55:01
-----BEGIN CERTIFICATE REQUEST-----
MIIDNTCCAh0CAQAwgaYxCzAJBgNVBAYTAkVTMQ8wDQYDVQQIDAZNYWRyaWQxDzAN
BgNVBAcMBk1hZHJpZDEsMCoGA1UECgwjVElLIFRlY2hlZGdlIEluc3RpdHV0ZSBv
ZiBLbm93bGVkZ2UxHzAdBgNVBAsMFkFkbWluIExhYiBEZW1vIFNlcnZlcnMxJjAk
BgNVBAMMHWxhYnRpazEyMjAxNy50ZWNoZWRnZWdyb3VwLmVzMIIBIjANBgkqhkiG
9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtqH2TGxuT4dJU4NIRIE5tq9KPhSBpTqn/3va
LrUJb0XQPazIR/mZ+UhWzP34/510TAcdnacF+RmU8PzK63SxwaNNjqf88SJH5ezC
1eNaRIfZO8c9CFfr88MPA6oF9m5oa+qlKn4c1AYAwN8CaRBif0Ju1aCZTPZrVaEa
S7063k1WWxRCrbEJ3XSCLY10Jx7DYpWankbaey4I/LYpXUtgN7fRc5vGPlnoFJAl
ua/8sjqdv+mOQmEIx4aSihf5rA6sLhWeMBuccRgOvZy5NHJIbcNMrlVmX9jT5yzE
DH1nxFS9lRXmfRbvocXtFJKQKJV/w3jAZdUFL37Krj1CDz/MUwIDAQABoEkwRwYJ
KoZIhvcNAQkOMTowODA2BgNVHREELzAtggxsYWJ0aWsxMjIwMTeCHWxhYnRpazEy
MjAxNy50ZWNoZWRnZWdyb3VwLmVzMA0GCSqGSIb3DQEBCwUAA4IBAQAi7pu4PMgx
YCtp0VwA1hXfk554M25RoDzSoY4a49ypN/9LQn2ecXhHwdYZdVY8XPOw3UQWKDf0
naJ0r14C2JfeCDv9ZyLnunTTovhzHTNLPm89+tSz63IhF4MdqWzKoH1dfNyJYbEW
p8Iv7tOXWjem+0KpwWtEprPVlAKv6Z6J4cL4dD3Hy9Hpse9xJiRjqYULl40sAyc+
sw613JDwIO1PamDGkDLebITEXZbhTyvguXu8ZLT758SvJGmc43p7WySG5ewUnMDJ
SXQwVKpcFUwp+7UIij99CnWDEVN+TDE276VCOH2Ee2tF/1JaLDTSsZ5rOk+78u9c
s2eJSzX601UB
-----END CERTIFICATE REQUEST-----
```
**ADVIERTASE** el fragmento:
```
... ... ... ...
        Requested Extensions:
            X509v3 Subject Alternative Name: 
                DNS:labtik122017, DNS:labtik122017.techedgegroup.es
... ... ... ...  
```

* Visualización del CSR con `grc-viewer`

```
$ gcr-viewer CSR_labtik122017_SAN.pem
```

![CSR_labtik122017_SAN.pem](images/gcr-viewer-CSR_Server02_image01.png "CSR_labtik122017_SAN.pem")

* Visualización con la herramienta en-línea >[CSR Decoder and Certificate Decoder (Red Krestel)(new)](https://redkestrel.co.uk/products/decoder/)

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder04_img01.png "https://redkestrel.co.uk/products/decoder/")

![https://redkestrel.co.uk/products/decoder/](images/CSR_Decoder04_img02.png
"https://redkestrel.co.uk/products/decoder/")


* Visualización del CSR con `openssl asn1parse`

```
$ openssl asn1parse -i -dump -in CSR_labtik122017_SAN.pem -inform PEM
    0:d=0  hl=4 l= 821 cons: SEQUENCE          
    4:d=1  hl=4 l= 541 cons:  SEQUENCE          
    8:d=2  hl=2 l=   1 prim:   INTEGER           :00
   11:d=2  hl=3 l= 166 cons:   SEQUENCE          
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
   61:d=3  hl=2 l=  44 cons:    SET               
   63:d=4  hl=2 l=  42 cons:     SEQUENCE          
   65:d=5  hl=2 l=   3 prim:      OBJECT            :organizationName
   70:d=5  hl=2 l=  35 prim:      UTF8STRING        :TIK Techedge Institute of Knowledge
  107:d=3  hl=2 l=  31 cons:    SET               
  109:d=4  hl=2 l=  29 cons:     SEQUENCE          
  111:d=5  hl=2 l=   3 prim:      OBJECT            :organizationalUnitName
  116:d=5  hl=2 l=  22 prim:      UTF8STRING        :Admin Lab Demo Servers
  140:d=3  hl=2 l=  38 cons:    SET               
  142:d=4  hl=2 l=  36 cons:     SEQUENCE          
  144:d=5  hl=2 l=   3 prim:      OBJECT            :commonName
  149:d=5  hl=2 l=  29 prim:      UTF8STRING        :labtik122017.techedgegroup.es
  180:d=2  hl=4 l= 290 cons:   SEQUENCE          
  184:d=3  hl=2 l=  13 cons:    SEQUENCE          
  186:d=4  hl=2 l=   9 prim:     OBJECT            :rsaEncryption
  197:d=4  hl=2 l=   0 prim:     NULL              
  199:d=3  hl=4 l= 271 prim:    BIT STRING        
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
  474:d=2  hl=2 l=  73 cons:   cont [ 0 ]        
  476:d=3  hl=2 l=  71 cons:    SEQUENCE          
  478:d=4  hl=2 l=   9 prim:     OBJECT            :Extension Request
  489:d=4  hl=2 l=  58 cons:     SET               
  491:d=5  hl=2 l=  56 cons:      SEQUENCE          
  493:d=6  hl=2 l=  54 cons:       SEQUENCE          
  495:d=7  hl=2 l=   3 prim:        OBJECT            :X509v3 Subject Alternative Name
  500:d=7  hl=2 l=  47 prim:        OCTET STRING      
      0000 - 30 2d 82 0c 6c 61 62 74-69 6b 31 32 32 30 31 37   0-..labtik122017
      0010 - 82 1d 6c 61 62 74 69 6b-31 32 32 30 31 37 2e 74   ..labtik122017.t
      0020 - 65 63 68 65 64 67 65 67-72 6f 75 70 2e 65 73      echedgegroup.es
  549:d=1  hl=2 l=  13 cons:  SEQUENCE          
  551:d=2  hl=2 l=   9 prim:   OBJECT            :sha256WithRSAEncryption
  562:d=2  hl=2 l=   0 prim:   NULL              
  564:d=1  hl=4 l= 257 prim:  BIT STRING        
      0000 - 00 22 ee 9b b8 3c c8 31-60 2b 69 d1 5c 00 d6 15   ."...<.1`+i.\...
      0010 - df 93 9e 78 33 6e 51 a0-3c d2 a1 8e 1a e3 dc a9   ...x3nQ.<.......
      0020 - 37 ff 4b 42 7d 9e 71 78-47 c1 d6 19 75 56 3c 5c   7.KB}.qxG...uV<\
      0030 - f3 b0 dd 44 16 28 37 f4-9d a2 74 af 5e 02 d8 97   ...D.(7...t.^...
      0040 - de 08 3b fd 67 22 e7 ba-74 d3 a2 f8 73 1d 33 4b   ..;.g"..t...s.3K
      0050 - 3e 6f 3d fa d4 b3 eb 72-21 17 83 1d a9 6c ca a0   >o=....r!....l..
      0060 - 7d 5d 7c dc 89 61 b1 16-a7 c2 2f ee d3 97 5a 37   }]|..a..../...Z7
      0070 - a6 fb 42 a9 c1 6b 44 a6-b3 d5 94 02 af e9 9e 89   ..B..kD.........
      0080 - e1 c2 f8 74 3d c7 cb d1-e9 b1 ef 71 26 24 63 a9   ...t=......q&$c.
      0090 - 85 0b 97 8d 2c 03 27 3e-b3 0e b5 dc 90 f0 20 ed   ....,.'>...... .
      00a0 - 4f 6a 60 c6 90 32 de 6c-84 c4 5d 96 e1 4f 2b e0   Oj`..2.l..]..O+.
      00b0 - b9 7b bc 64 b4 fb e7 c4-af 24 69 9c e3 7a 7b 5b   .{.d.....$i..z{[
      00c0 - 24 86 e5 ec 14 9c c0 c9-49 74 30 54 aa 5c 15 4c   $.......It0T.\.L
      00d0 - 29 fb b5 08 8a 3f 7d 0a-75 83 11 53 7e 4c 31 36   )....?}.u..S~L16
      00e0 - ef a5 42 38 7d 84 7b 6b-45 ff 52 5a 2c 34 d2 b1   ..B8}.{kE.RZ,4..
      00f0 - 9e 6b 3a 4f bb f2 ef 5c-b3 67 89 4b 35 fa d3 55   .k:O...\.g.K5..U
      0100 - 01 
```

# Seccion 02 - Ejemplos de utilización de `Java Keytool` para generar `CSR - Certificate Signing Requests'


## 02.02 `CSR` de `Usuario`  


## 02.03 `CSR` de `Dominio` (de `Servidor`)  


## 02.04 `CSR` de `Usuario` EXTENDIDO 


## 02.05 `CSR` de `Dominio` (de `Servidor`) EXTENDIDO 

# Seccion 03 - Ejemplos de utilización de `Java Keytool` para generar `CSR - Certificate Signing Requests'


## 03.02 `CSR` de `Usuario`  


## 03.03 `CSR` de `Dominio` (de `Servidor`)  


