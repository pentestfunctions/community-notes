# Installing Certificate in User Store

* To intercept SSL/TLS communication we need the certificate of our proxy tool to be trusted by the device.
* Via the android setting we can easily install a certificate in the “user” CA store.
* User certificates are only trusted when :-
  * Android 6 (API level 23) or lower.
  * Or `network_security_config`  specifically include “user” certificates to be trusted.
  *   This can be defined in `xml/network_security_config.xml`

      ```xml
      <base-config cleartextTrafficPermitted="false">
          <trust-anchors>
              <certificates src="system" />
              <certificates src="user" />
          </trust-anchors>
      </base-config>
      ```

      *   Before defining this we need to include this line in `AndroidManifest.xml`

          ```xml
          <application
          	android:networkSecurityConfig="@xml/network_security_config">
          </application>
          ```
* NOTE :- By installing CA we are intentionally **weakening** the security of the device to allow us to decrypt the traffic.

***
