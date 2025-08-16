# Advanced Network interception using VPN

* Apps can explicitly mention to ignore device proxy settings due to which even we have our certificate in system store we will not be able to intercept traffic.

* Here we can use Android VPN service to intercept traffic of such apps. (more detail [here](https://developer.android.com/develop/connectivity/vpn))
* For this we can use the open source VPN app [rethink-app](https://github.com/celzero/rethink-app) .
  * Assure that system certificate is already installed.
  * Then change DNS setting to **System DNS**
  * Then add a **HTTP(S) CONNECT** proxy (eg:- `http://192.168.29.1:8080`)
  * Finally start the VPN.
* **HTTP Toolkit** also use this same method under the hood.

***
