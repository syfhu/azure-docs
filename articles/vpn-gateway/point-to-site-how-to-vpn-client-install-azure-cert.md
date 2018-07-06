---
title: 'Install a P2S client certificate | Azure'
description: Install a Mac or Windows client cert for P2S certificate authentication.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager, azure-service-management

ms.assetid:
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc

---
# Install a client certificate for Point-to-Site Azure certificate authentication connections

All clients that connect to a virtual network using Point-to-Site Azure certificate authentication require a client certificate. This article helps you install a client certificate that is used for authentication when connecting to a VNet using P2S.

## <a name="generate"></a>Generate and export a client certificate

You can generate a client certificate from either a root certificate that was generated using an Enterprise CA solution, or a self-signed root certificate. See the [PowerShell](vpn-gateway-certificates-point-to-site.md) or [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) instructions for steps. After generating client certificates, export them as .pfx files. Be sure to include the entire certificate chain when exporting.

## <a name="installwin"></a>Install certificate - Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Install certificate - Mac

Mac VPN clients are supported for the Resource Manager deployment model only. They are not supported for the classic deployment model.

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## Next steps

Continue with the Point-to-Site configuration steps.

* [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure portal (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
