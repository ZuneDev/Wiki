# Liberate
Liberate is a application developed by ``Netrix`` which makes it possible to use the ``explorer.exe`` on [Zune HD](../../Zune%20HD/index.md). It also provides functionality to run OpenGL, DirectX and Windows Apps on Liberate.

## Wi-Fi problem
In order to save battery, the Zune automatically disables its Wi-Fi connection after a few minutes. This causes problems when trying to run executables that utilzie Wi-Fi for extended periods of time, such as when using the Windows CE Remote debugging tools through Liberate. 

Liberate has a somewhat working fix in ``RelayInput.exe``, which simply pings `google.com` and `yahoo.com` every couple of minutes. However, this fix is only triggered when Liberate detects the Zune is online via ``ZDKCloud_IsConnected()``, which nowadays always returns ``FALSE`` since it uses the now-offline Zune webservices as a litmus test for internet connectivity.

Current research on a permanent fix are [here](../Keep%20WIFI%20Enabled.md).

---
[🗺️ OpenZDK](./index.md)