# Installing system certificates

* Due to default [network security config](https://developer.android.com/privacy-and-security/security-config) rules, most apps only support “**system**” certificates.
* To install our certificates in system store we need a **rooted** android device.
* First install certificate as a regular user certificate.
* Then ensure that you are root (`adb -s <emulator_name> root`).
  * To see the emulator name use `adb devices`
* Then do `adb -s <emulator_name> shell`

### 1) Backup the existing system certificates to the user certs folder

```bash
cp /system/etc/security/cacerts/* /data/misc/user/0/cacerts-added/
```

### 2) Create the in-memory mount on top of the system certs folder

```bash
mount -t tmpfs tmpfs /system/etc/security/cacerts
```

### 3) copy all system certs and our user cert into the tmpfs system certs folder

```bash
cp /data/misc/user/0/cacerts-added/* /system/etc/security/cacerts/
```

### 4) Fix any permissions & selinux context labels

```bash
chown root:root /system/etc/security/cacerts/*
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*
```

***
