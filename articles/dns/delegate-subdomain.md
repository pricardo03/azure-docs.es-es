---
title: Delegación de un subdominio de Azure DNS
description: Obtenga información sobre cómo delegar un subdominio de Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/22/2019
ms.author: victorh
ms.openlocfilehash: 87a80703c473245660a850645ca3fef21bbd80f6
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452724"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Delegación de un subdominio de Azure DNS

Puede usar Azure Portal para delegar un subdominio DNS. Por ejemplo, si es propietario del dominio contoso.com, puede delegar un subdominio llamado *ingeniería* a otra zona independiente que se puede administrar por separado respecto de la zona contoso.com.

## <a name="prerequisites"></a>Requisitos previos

Para delegar un subdominio de Azure DNS, primero debe delegar el dominio público en Azure DNS. Consulte [Delegación de un dominio en Azure DNS](./dns-delegate-domain-azure-dns.md) para obtener instrucciones sobre cómo configurar los servidores de nombres para su delegación. Una vez que el dominio se delega a la zona de Azure DNS, puede delegar el subdominio.

En los ejemplos de este artículo se usa el dominio contoso.com. Al usar estos procedimientos debe sustituirlos por su propio dominio.

## <a name="create-a-zone-for-your-subdomain"></a>Creación de una zona para el subdominio

En primer lugar, cree la zona para el subdominio **ingeniería**.

1. En Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de búsqueda del servicio, escriba **DNS** y seleccione **Zona DNS**.
3. Seleccione **Crear**.
4. En el panel **Crear zona DNS**, escriba **ingieniería.contoso.com** en el cuadro de texto **Nombre**.
5. Seleccione el grupo de recursos para la zona. Es posible que desee usar el mismo grupo de recursos como zona primaria para mantener juntos los recursos similares.
6. Haga clic en **Create**(Crear).
7. Una vez que la implementación se haya realizado correctamente, vaya a la nueva zona.

## <a name="note-the-name-servers"></a>Observación de los servidores de nombres

A continuación, copie los cuatro servidores de nombres del subdominio.

1. En el panel de la zona **ingeniería**, observe los cuatro servidores de nombres para la zona. Estos servidores de nombres se utilizarán más adelante.
2. Cree un registro **D** que se usará para las pruebas. Por ejemplo, cree un registro D **www** y configúrelo con una dirección IP **10.10.10.10**.

## <a name="create-an-ns-record"></a>Creación de un registro NS

A continuación, cree un registro de servidor de nombres para la zona **ingeniería**.

1. Vaya a la zona del dominio primario.
2. Seleccione **+ Conjunto de registros**.
3. En el panel **Agregar conjunto de registros**, escriba **ingeniería** en el cuadro de texto **Nombre**.
4. En **Tipo**, seleccione **NS**.
5. En **Servidor de nombres**, escriba los cuatro servidores de nombres que anotó anteriormente en la zona **ingeniería**.
6. Haga clic en **OK**.

## <a name="test-the-delegation"></a>Prueba de la delegación

Utilice nslookup para probar la delegación.

1. Abra una ventana de PowerShell.
2. En el símbolo del sistema, escriba `nslookup www.engineering.<your domain name>.`.
3. Debería recibir una respuesta no autoritativa que muestra la dirección **10.10.10.10**.



## <a name="next-steps"></a>Pasos siguientes

Aprenda a [configurar búsquedas inversas de DNS para servicios hospedados en Azure](dns-reverse-dns-for-azure-services.md).