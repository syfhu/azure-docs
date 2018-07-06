---
 title: include file
 description: include file
 services: vpn-gateway
 author: cherylmc
 ms.service: vpn-gateway
 ms.topic: include
 ms.date: 03/21/2018
 ms.author: cherylmc
 ms.custom: include file
---
If you are having trouble connecting, check the following items:

- If you exported a client certificate, make sure that you exported it as a .pfx file using the default value 'Include all certificates in the certification path if possible'. When you export it using this value, the root certificate information is also exported. When the certificate is installed on the client computer, the root certificate which is contained in the .pfx file is then also installed on the client computer. The client computer must have the root certificate information installed. To check, go to **Manage user certificates** and navigate to **Trusted Root Certification Authorities\Certificates**. Verify that the root certificate is listed. The root certificate must be present in order for authentication to work.

- If you are using a certificate that was issued using an Enterprise CA solution and are having trouble authenticating, check the authentication order on the client certificate. You can check the authentication list order by double-clicking the client certificate, and going to **Details > Enhanced Key Usage**. Make sure the list shows 'Client Authentication' as the first item. If not, you need to issue a client certificate based on the User template that has Client Authentication as the first item in the list.

- For additional P2S troubleshooting information, see [Troubleshoot P2S connections](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).