# Frida & Objection

* Installation
  * frida - `pip install frida-tools` or `pip3 install frida-tools` depending on what u are using.
  * objection - `pip install objection` or `pip3 install objection` depending on what u are using.
*   Patching with objection to inject Frida

    ```bash
    objection patchapk -s .\\FridaTarget.apk
    ```
*   Connecting to target application patched with objection

    ```bash
    frida -U FridaTarget
    ```

    * `-U` is used to tell the frida to connect the target application via USB (also works for emulators).
*   Installing frida server in the emulator

    * push the frida-server file from local pc to the emulator

    ```bash
    adb push frida-server /data/local/tmp/
    ```

    * Run as root make the frida-server file executable

    ```bash
    adb shell
    su
    cd /data/local/tmp/
    chmod +x frida-server
    ```

    * Run the server

    ```bash
    ./frida-server
    ```

    * Now connect to any running apps using;-

    ```bash
    frida -U <APP_NAME_LIKE_FridaTarget_or_OP>
    ```
* Creating frida scripts
  * Although we can create frida scripts in many languages but JavaScript API is documented in better way than other.
  * For creating own frida scripts refer:- [https://frida.re/docs/javascript-api/](https://frida.re/docs/javascript-api/)
*   Loading frida scripts

    * To load frida scripts we use `-l` flag

    ```bash
    frida -U -l script.js FridaTarget
    ```

    * For more options see help menu of frida

    ```bash
    frida --help
    ```

***
