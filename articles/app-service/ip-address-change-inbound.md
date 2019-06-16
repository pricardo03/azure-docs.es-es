---
title: 'Preparación de un cambio de dirección IP de entrada: Azure App Service'
description: Si la dirección IP de entrada se va a cambiar, conozca qué debe hacer para que la aplicación continúe funcionando después del cambio.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: aaa89b5a3bb1af6878ed21e0160a534a1c989228
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61270071"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Preparación de un cambio de dirección IP de entrada

Si recibe una notificación indicándole que se van a cambiar las direcciones IP de entrada de la aplicación de Azure App Service, siga las instrucciones de este artículo.

## <a name="determine-if-you-have-to-do-anything"></a>Determinar si es necesario realizar alguna acción por su parte

* Opción 1: Si la aplicación de App Service no tiene un dominio personalizado, no se requiere ninguna acción.

* Opción 2: Si solo se configura un registro CNAME (registro DNS que apunta a un URI) en el portal de registro de dominios (proveedor de DNS de terceros o Azure DNS), no se requiere ninguna acción.

* Opción 3: Si se configura un registro A (registro DNS que apunta directamente a la dirección IP) en el portal de registro de dominios (proveedor de DNS de terceros o Azure DNS), reemplace la dirección IP existente por una nueva. Para encontrar la nueva dirección IP, siga las instrucciones de la sección siguiente.

* Opción 4: Si la aplicación está detrás de un equilibrador de carga, filtro de direcciones IP o cualquier otro mecanismo de IP que requiera una dirección IP de la aplicación, reemplace la dirección IP existente por una nueva. Para encontrar la nueva dirección IP, siga las instrucciones de la sección siguiente.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Busque la nueva dirección IP de entrada en Azure Portal

La nueva dirección IP de entrada que se le da a la aplicación está en el portal en el campo **Dirección IP Virtual**. Esta nueva dirección IP y la antigua están conectadas en este momento a la aplicación pero, más adelante, se desconectará la antigua.

1.  Abra [Azure Portal](https://portal.azure.com).

2.  En el menú de navegación izquierdo, seleccione **App Services**.

3.  Seleccione la aplicación de App Service en la lista.

1.  Si la aplicación es una aplicación de función, consulte la sección [Function app inbound IP address](../azure-functions/ip-addresses.md#function-app-inbound-ip-address) (Dirección IP entrante de la aplicación de función).

4.  En el encabezado **Configuración**, haga clic en **Propiedades** en el menú de navegación izquierdo y busque la sección denominada **Dirección IP virtual**.

5. Copie la dirección IP y vuelva a configurar el registro de dominios o mecanismo de IP.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo preparar un cambio de dirección IP que Azure inició. Para más información sobre las direcciones IP en Azure App Service, consulte [direcciones IP de entrada y salida en Azure App Service](overview-inbound-outbound-ips.md).
