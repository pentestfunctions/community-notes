# ADB

## adb push (To transfer files between your computer and the device)

```sh
adb push <local_file_on_computer> <target_path_on_device>
```

* Example:-

```bash
adb push Desktop/test.png /sdcard/Downloads/
```

## adb pull (To transfer files between device and your computer)

```bash
adb pull <file_path_on_device> [<optional_target path_on_the_computer>]
```

* Example:-

```bash
adb pull /sdcard/Downloads
```

## adb install (To install an apk to the device form your computer)
```bash
adb install <path to .apk>
```

## All about a package (application)

* To list installed packages

```bash
adb shell pm list packages
```

* To list installed 3rd party packages

```bash
adb shell pm list packages -3
```

* Clear the application data without removing the actual application

```bash
adb shell pm clear <package_name>
```

* List activities and permissions of a package

```bash
adb shell dumpsys package <package_name>
```

* Start a specific activity of a specific package

```bash
adb shell am start <package_name>/.<activity_name>
```

* To pass some extra string with activity

```bash
adb shell am start -n <package_name>/.<activity_name> -e <extra thing here>
```

* Uninstall specified package

```bash
adb uninstall <package_name>
```

* To set some permission to app (modify app operation to allow it to do something)

```bash
adb shell appops set <package_name> <PERM_NAME> allow
```

## adb logcat (to view the ongoing logs)

* To view log in another format

```bash
adb logcat -v <log_format>
```

<table><thead><tr><th width="215.3333740234375">&#x3C;log_format></th><th>Description</th></tr></thead><tbody><tr><td>brief</td><td>Show priority, tag, and PID of the process issuing the message.</td></tr><tr><td>long</td><td>Show all metadata fields and separate messages with blank lines.</td></tr><tr><td>process</td><td>Show PID only.</td></tr><tr><td>raw</td><td>Show the raw log message with no other metadata fields.</td></tr><tr><td>tag</td><td>Show the priority and tag only.</td></tr><tr><td>thread</td><td>Show priority, PID, and TID of the thread issuing the message.</td></tr><tr><td>threadtime</td><td>Show the date, invocation time, priority, tag, PID, and TID of the thread issuing the message. (This is the default)</td></tr><tr><td>time</td><td>Show the date, invocation time, priority, tag, and PID of the process issuing the message.</td></tr></tbody></table>

* Log filtering

```bash
adb logcat "MainActivity:V *:S"
```

* `MainActivity:V` ensures that logs from the tag MainActivity with a severity of Verbose and above are logged.
* `*:S` Ensures that all other Tags are ignored (as nothing will log with log-level Silent or above)

⇒ Logging level

- V Verbose
- D Debug
- I Info
- W Warning
- E Error
- F Fatal
- S Silent

## Packet logging with tcpdump

```bash
emulator -tcpdump packets.cap -avd <avd_name>
```

* packets.cap name of the file where traffic will be captured

- Emulator\_API\_34 name of the emulator which is to be turned on (AvdId)
  * AvdId can be found in more in the emulator


## To install app with split configs

```bash
adb install-multiple <all apks with split parts>
```

***
