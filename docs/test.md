# Outlook Upload Applet
This applet allows Support to drag and drop emails from a outlook client to a webpage and the file will be uploaded to a
service specified by the applet parameters.

## Background
SBAB made transition from Lotus Notes to Microsoft Outlook and doing that the users could not drag and drop files from
the mail client.

### Why an Applet?
We looked at several options like internet explorer plugin, a plugin for outlook, and different use of java.
Since the time was an issue we choose the solution which we thought was the fastest and most likely to succeed.

|Solution|Description|Time|Risk|
|--------|-----------|:--------:|:--:|
| Outlook plugin | This seems to be the most intuitive solution, but the customer support didn't like that the needed to manually enter information. | Mid | Mid |
| IE plugin | The next option we looked at was a IE plugin, but there were several issues. Seasoned .NET web developers did not recommend this way. | Mid | High |
| Java Applet | It's an old technology which Oracle has declared they will drop the support soon. | Low | Low |
| JavaFX | This is probably the best way to go in the future. But since it is new technology there were some risks. | Low | Mid |

#### Some notes about JavaFX
JavaFX can be deployed on different platforms like IOS and Android, but also in browser. It can also access JavaScript from the application.
Here is an example:
```JavaScript
<head>
    <script type="text/javascript" src="http://java.com/js/dtjava.js"></script>
    <script>
        function deployIt() {
            var customerid = 95054;
            dtjava.embed(
                {            id: "OutlookUploadApplet",
                            url: "OutlookUploadApplet.jnlp",
                          width: 213,
                         height: 73,
                    placeholder: "place",
                    params: {
                              url: "<url to server>",
                              id: customerid
                    }
                },
                { javafx: "2.1+" },
                {}
            );
        }
        dtjava.addOnloadCallback(deployIt);
    </script>
</head>
<body>
    <div id="place"></div>
</body>
```

More examples on how to deploy a JavaFX application can be found at [Oracle docs][2]

## Dependencies
Dependencies used by Outlook Upload Applet

| Dependency | Description |
|------------|-------------|
| OutlDD |OutlDD enables Java programs to make use of the special Micosoft Outlook drag&drop data format. It can be used with Java 5.0 to 8.0 on Microsoft Windows XP, 2003, Vista, 2008 and 7 and Microsoft Outlook 2003 to 2013. |

Outldd is included in the lib catalog.

## Build
| Req. | Description |
|------|-------------|
| Java 1.7+ | Java 1.7 or later is recommended. |
| jarsigner | For signing java applets, should be bundled with the JDK |
| Maven 3 | Build tool |

### Building

```bash
maven clean install
```

### Signing
This step is only needed once to setup the keystore and add certificates.

Signing applets requires the following :
1. Signing tools
2. An RSA keypair and a certificate chain for the public keys
3. The applet and all its class files, bundled as JAR files

#### Getting RSA Certificates
RSA certificates may be purchased from a Certificate Authority (CA) that supports RSA, such as VeriSign and Thawte. Some CAs, such as VeriSign, implement different protocols for issuing certificates, depending on the particular signing tool you are using.

#### Getting Certificates with Jarsigner
Jarsigner is known to work with VeriSign and Thawte certificates and may work with Certificate Authorties. To use Jarsigner to sign applets using RSA certificates, obtain code signing certificates for Java from VeriSign, Thawte, or similar certificates from other CAs. During the process of certificate enrollment, you will be asked to provide the certificate signing request (CSR). To generate the CSR, follow these steps:

Use keytool to generate an RSA keypair (using the "-genkey -keyalg rsa" options). Make sure your distinguished name contains all the components mandated by VeriSign/Thawte. For example:
```
C:\Program Files\Java\jdk1.8.0\bin\keytool -genkey -keyalg rsa -alias MyCert
Enter keystore password: *********
What is your first and last name?
[Unknown]: XXXXXXX YYY
What is the name of your organizational unit?
[Unknown]: Partalen
What is the name of your organization?
[Unknown]: SBAB
What is the name of your City or Locality?
[Unknown]: Stockholm
What is the name of your State or Province?
[Unknown]:
What is the two-letter country code for this unit?
[Unknown]: SE
Is <CN=XXXXXXX YYY, OU=Partalen, O=SBAB,
                L=Stockholm, ST=, C=SE> correct?
[no]: yes

Enter key password for <MyCert>
(RETURN if same as keystore password): *********
```
Use "keytool -certreq" to generate a certification signing request. Copy the result and paste it into the VeriSign/Thawte webform. For example,
```bash
C:\Program Files\Java\jdk1.8.0\bin\keytool -certreq -alias MyCert

Enter keystore password:  *********
-----BEGIN NEW CERTIFICATE REQUEST-----
MIIBtjCCAR8CAQAwdjELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAkNBMRIwE
AYDVQQHEwlDdXBlcnRpbm8xGTAXBgNVBAoTEFN1biBNaWNyb3N5c3RlbX
MxFjAUBgNVBAsTDUphdmEgU29mdHdhcmUxEzARBgNVBAMTClN0YW5sZXk
gSG8wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBALTgU8PovA4y59eb
oPjY65BwCSc/zPqtOZKJlaW4WP+UhmebE+T2Mho7P5zXjGf7elo3tV5uI
3vzgGfnhgpf73EoMow8EJhly4w/YsXKqeJEqqvNogzAD+qUv7Ld6dLOv0
CO5qvpmBAO6mfaI1XAgx/4xU/6009jVQe0TgIoocB5AgMBAAGgADANBgk
qhkiG9w0BAQQFAAOBgQAWmLrkifKiUYtd4ykhBtPWSwW/IKkgyfIuNMML
dF1DH8neSnXf3ZLI32f2yXvs7u3/xn6chnTXh4HYCJoGYOAbB3WNbAoQR
i6u6TLLOvgv9pMNUo6v1qB0xly1faizjimVYBwLhOenkA3Bw7S8UIVfdv
84cO9dFUGcr/Pfrl3GtQ==
-----END NEW CERTIFICATE REQUEST-----

```
The CA (for example, VeriSign/Thawte) will send you a certificate reply (chain) by email. Copy the chain and store it in a file. Use "keytool -import" to import the chain into your keystore. For example:
```bash
C:\Program Files\Java\jdk1.6.0\bin\keytool -import -alias MyCert -file VSSStanleyNew.cer
```
Your RSA certificate and its supporting chain have been validated and imported into your keystore. You are now ready to use jarsigner to sign your JAR file.

Note: You must use the same alias name for all the above steps or no alias name, in which case the alias name defaults to "mykey".

### Manually sign applets using jarsigner
This is not needed but I put it here for reference if there is a problem with allowing maven automate this process or if there is a problem with the jar itself.

To sign applets using jarsigner, follow these steps:

Use jarsigner to sign the JAR file, using the RSA credentials in your keystore that were generated in the previous steps. Make sure the same alias name is specified. E.g.,
```
# jarsigner upload-applet.jar MyCert
Enter Passphrase for keystore: ********
```

Use "jarsigner -verify -verbose -certs" to verify the jar files
```
# jarsigner -verify -verbose -certs upload-applet.jar
         245 Wed Jun 13 11:48:52 PST 2000 META-INF/manifest.mf
         187 Wed Jun 13 11:48:52 PST 2000 META-INF/MYCERT.SF
         968 Wed Jun 13 11:48:52 PST 2000 META-INF/MYCERT.RSA
smk      943 Wed Jun 13 11:48:52 PST 2000 TestApplet.class
smk      163 Wed Jun 13 11:48:52 PST 2000 TestHelper.class

      X.509, CN=XXXXXXX YYY, OU=Partalen,
                O=SBAB, L=,
                ST=, C=SE (mycert)

  s = signature was verified
  m = entry is listed in manifest
  k = at least one certificate was found in keystore
  i = at least one certificate was found in identity scope

jar verified.
```

Your applet has been signed properly. You are now ready to deploy your RSA signed applet.

### Using maven to generate keystore and sign the applet
There is an easy way to create a test certificate and use that to sign the applet by using maven. It will create a keystore in
the target catalog and will be removed everytime you do a clean.
```
# mvn -Dgenerate-key install
```

### Using maven to use a production keystore for signing
For security reason we want our keys and passwords to be secure (as much as possible anyway), so to sign a jar with a
real certificate you can tell maven where to find the keystore and the passwords in the `~/.m2/settings.xml`

```
<settings>
  <profiles>
    <profile>
      <id>generate-upload-applet-release</id>
      <properties>
        <upload-applet.keystore.filename>{PathToYourKeystore}</upload-applet.keystore.path>
        <upload-applet.keystore.store.password>{YourPasswordHere}</upload-applet.keystore.store.password>
        <upload-applet.keystore.key.password>{YourPasswordHere}</upload-applet.keystore.key.password>
      </properties>
    </profile>
  </profiles>
<settings>
```

## Deploy
Upload the signed jar to a filearea on the server, like webapp catalog, where the applet can be downloaded.
On every page where you need the upload functionality add
```javascript
<APPLET archive="<url to upload-applet.jar>" code="UploadApplet" width="213" height="73">
    <PARAM NAME="id" VALUE="<id for the server>" />
    <PARAM NAME="url" VALUE="<server-url>" />
    <PARAM name="permissions" value="all-permissions" />
    <param name="cache_archive" VALUE="upload-applet.jar">
</APPLET>
```
|PARAM        |Description |
|-------------|------------|
| ID | Parameter is an identifier for the server which is used to connect the uploaded files to a customer support ticket |
| URL | Should point to the upload server address. Pårtalen uses different server URL's on different environment |
| PERMISSIONS | Sets the all-permissions security level needed for uploading files to the server
| CACHE_ARCHIVE | Makes the applet load faster |

You can also use Embedd (usually for the netscape familly) and Object (Internet Explorer). Read more at [Oracle Java SE Documentation][3]

## License
A license is needed to use Outldd and is purchased from the [OutlDD][1] site.
License is stored in `src/main/resource/de/wim/outldd/outldd.license`

[1]: http://www.wilutions.com/outldd/
[2]: https://docs.oracle.com/javafx/2/deployment/deployment_toolkit.htm
[3]: http://docs.oracle.com/javase/7/docs/technotes/guides/jweb/applet/using_tags.html
[4]: https://docs.oracle.com/javase/7/docs/technotes/guides/jweb/security/rsa_signing.html
