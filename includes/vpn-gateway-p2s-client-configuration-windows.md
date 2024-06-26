---
 title: include file
 description: include file
 services: vpn-gateway
 author: cherylmc
 ms.service: vpn-gateway
 ms.topic: include
 ms.date: 07/12/2021
 ms.author: cherylmc

---
You can use the same VPN client configuration package on each Windows client computer, as long as the version matches the architecture for the client. For the list of client operating systems that are supported, see the Point-to-Site section of the [VPN Gateway FAQ](../articles/vpn-gateway/vpn-gateway-vpn-faq.md#P2S).

>[!NOTE]
>You must have Administrator rights on the Windows client computer from which you want to connect.
>

### Install the configuration files

1. Select the VPN client configuration files that correspond to the architecture of the Windows computer. For a 64-bit processor architecture, choose the 'VpnClientSetupAmd64' installer package. For a 32-bit processor architecture, choose the 'VpnClientSetupX86' installer package.
1. Double-click the package to install it. If you see a SmartScreen popup, click **More info**, then **Run anyway**.

### Verify and connect

1. Verify that you have installed a client certificate on the client computer. A client certificate is required for authentication when using the native Azure certificate authentication type. To view the client certificate, open **Manage User Certificates**. The client certificate is installed in **Current User\Personal\Certificates**.
1. To connect, navigate to **Network Settings** and click **VPN**. The VPN connection shows the name of the virtual network that it connects to.
