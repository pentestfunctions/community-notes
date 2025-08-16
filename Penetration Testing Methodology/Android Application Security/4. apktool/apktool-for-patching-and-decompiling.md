# apktool (for patching and decompiling)

* decompiling the app
* then change its network configurations to trust user certificates also
* recompiling the apk
* sign the apk so that it can be installed on any device
* [hextree video demo ⇗](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/patching-network-security-config-with-ap)

### unpack the target .apk

```bash
apktool d translate.apk
```

* modify the AndroidManifest.xml to add a networkSecurityConfig
  * create a permissive xml/network\_security\_config.xml

```
cd translate
```

### repackage the .apk

```bash
apktool b
```

### ensure the .apk is zipaligned

```bash
[...]/build-tools/34.0.0/zipalign -p -f -v 4 ./dist/translate.apk translate2.apk
```

### create a keystore to sign the apk

```bash
keytool -genkey -v -keystore research.keystore -alias research_key -keyalg RSA -keysize 2048 -validity 10000
```

### sign the apk with apksigner

```bash
[...]/build-tools/34.0.0/apksigner sign --ks ./research.keystore ./translate2.apk
```

### Signature Verification

* To verify if an app is signed or not (also works for aab)

```bash
jarsigner -verify -verbose -certs your_app.aab
```

* for only apk

```bash
apksigner verify your_app.apk
```

***
