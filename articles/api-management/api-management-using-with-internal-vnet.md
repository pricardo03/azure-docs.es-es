---
title: Uso de Azure API Management con redes virtuales internas | Microsoft Docs
description: Aprenda a instalar y configurar Azure API Management en una red virtual interna.
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: d8cea95fbfb76f1dd1891045309a35aa1d0a8ab0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58099491"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Uso del servicio Azure API Management con una red virtual interna
Con Azure Virtual Network, Azure API Management puede administrar las API que no están accesibles desde Internet. Para establecer la conexión, hay una serie de tecnologías de VPN disponibles. API Management puede implementarse de dos modos en una red virtual:
* Externo
* Interno

Cuando API Management se implementa en el modo de red virtual interna, todos los puntos de conexión de servicio (puerta de enlace, portal del desarrollador, Azure Portal, administración directa y Git) solamente están visibles en una red virtual en la que usted controla el acceso. Ninguno de los puntos de conexión de servicio está registrado en el servidor DNS público.

Si utiliza API Management en modo interno, puede conseguir los siguientes escenarios:

* Puede conseguir que terceras personas puedan obtener acceso de forma segura a las API hospedadas en el centro de datos privado desde fuera de este centro utilizando conexiones de sitio a sitio o conexiones de VPN de Azure ExpressRoute.
* Puede permitir escenarios de nube híbrida exponiendo las API basadas en la nube y las API locales a través de una puerta de enlace común.
* Puede administrar las API hospedadas en varias ubicaciones geográficas mediante un único punto de conexión de puerta de enlace.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos que se describen en este artículo, debe tener:

+ **Una suscripción activa a Azure**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Una instancia de Azure API Management**. Para más información, vea [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).

## <a name="enable-vpn"> </a>Creación de una instancia de API Management en una red virtual interna
El servicio API Management en una red virtual interna se hospeda detrás de un [equilibrador de carga interno (clásico)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-classic-cloud). Esta es la única opción disponible y no se puede cambiar.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Habilitación de una conexión de red virtual mediante Azure Portal

1. Navegue a la instancia de Azure API Management en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Red virtual**.
3. Configure la instancia de API Management que se va a implementar dentro de la red virtual.

    ![Menú de configuración de una instancia de Azure API Management en una red virtual interna][api-management-using-internal-vnet-menu]

4. Seleccione **Guardar**.

Después de la implementación se realiza correctamente, debería ver **privada** dirección IP virtual y **pública** dirección IP virtual del servicio API Management en la hoja de información general. El **privada** dirección IP virtual es una carga equilibrada de la dirección IP desde dentro de la API de administración delegada subred sobre qué `gateway`, `portal`, `management` y `scm` pueden tener acceso a los puntos de conexión. El **pública** se utiliza la dirección IP virtual **sólo** para controlar el tráfico del plano a `management` punto de conexión de puerto 3443 y puede bloquearse hasta el [ApiManagement] [ ServiceTags] servicetag.

![Panel de API Management con una red virtual interna configurada][api-management-internal-vnet-dashboard]

> [!NOTE]
> La consola de prueba disponible en Azure Portal no funcionará con un servicio implementado en una red virtual **interna**, ya que la dirección Url de puerta de enlace no está registrada en el DNS público. En su lugar, debe usar la consola de prueba que se ofrece en el **Portal para desarrolladores**.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>Habilitación de una conexión de red virtual mediante cmdlets de PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

También puede habilitar la conectividad de la red virtual utilizando cmdlets de PowerShell.

* Crear un servicio API Management dentro de una red virtual: Use el cmdlet [New AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) para crear un servicio de Azure API Management dentro de una red virtual y configúrelo para utilizar el tipo de red virtual interna.

* Actualizar una implementación existente de un servicio API Management dentro de una red virtual: Use el cmdlet [actualización AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) para mover un servicio API Management existente en una red virtual y configúrelo para utilizar el tipo de red virtual interna.

## <a name="apim-dns-configuration"></a>Configuración de DNS
Cuando API Management está en modo de red virtual externa, el DNS está administrado por Azure. En el modo de red virtual interna, es usted quien tiene que administrar su propio enrutamiento.

> [!NOTE]
> El servicio API Management no escucha las solicitudes procedentes de direcciones IP. Solo responde a las solicitudes dirigidas al nombre de host establecido en los puntos de conexión de servicio. Estos puntos de conexión pueden ser la puerta de enlace, Azure Portal y el portal del desarrollador, el punto de conexión de administración directa y GIT.

### <a name="access-on-default-host-names"></a>Acceso de nombres de host predeterminados
Cuando se crea un servicio API Management, denominado "contosointernalvnet" por ejemplo, los siguientes extremos de servicio se configuran de forma predeterminada:

   * Puerta de enlace o proxy: contosointernalvnet.azure-api.net

   * El portal de Azure y el portal para desarrolladores: contosointernalvnet.portal.azure-api.net

   * Direct management endpoint: contosointernalvnet.management.azure-api.net

   * Git: contosointernalvnet.scm.azure-api.net

Para obtener acceso a estos puntos de conexión de servicio de API Management, puede crear una máquina virtual en una subred conectada a la red virtual en la que está implementado API Management. Suponiendo que la dirección IP virtual interna para el servicio es 10.1.0.5, puede asignar el archivo de hosts, % SystemDrive%\drivers\etc\hosts, como se indica a continuación:

   * 10.1.0.5     contosointernalvnet.azure-api.net

   * 10.1.0.5     contosointernalvnet.portal.azure-api.net

   * 10.1.0.5     contosointernalvnet.management.azure-api.net

   * 10.1.0.5     contosointernalvnet.scm.azure-api.net

Así, podrá tener acceso a todos los puntos de conexión de servicio desde la máquina virtual que ha creado.
Si utiliza un servidor DNS personalizado en una red virtual, también puede crear registros D de DNS y acceder a estos puntos de conexión desde cualquier lugar de la red virtual.

### <a name="access-on-custom-domain-names"></a>Acceso de nombres de dominio personalizados

1. Si no quiere acceder al servicio API Management con los nombres de host predeterminados, puede definir nombres de dominio personalizados para todos los puntos de conexión de servicio, tal y como se indica en la siguiente ilustración:

   ![Configuración de un dominio personalizado para API Management][api-management-custom-domain-name]

2. A continuación, puede crear registros en el servidor DNS para acceder a los puntos de conexión que solo están accesibles desde dentro de la red virtual.

## <a name="routing"> </a> Enrutamiento
+ Se reservará una dirección IP virtual privada con equilibrio de carga desde el intervalo de subred y se utilizará para tener acceso a los puntos de conexión del servicio API Management desde la red virtual.
+ También se reservará una dirección IP pública (VIP) con equilibrio de carga para proporcionar acceso al punto de conexión del servicio de administración solo a través del puerto 3443.
+ Se usará una dirección IP de un intervalo de IP de subred (DIP) para el acceso a los recursos dentro de la red virtual y una dirección IP pública (VIP) para el acceso a los recursos fuera de la red virtual.
+ Las direcciones IP privadas y públicas con equilibrio de carga pueden encontrarse en la hoja Información general/nformación esencial en Azure Portal.

## <a name="related-content"></a>Contenido relacionado
Para obtener más información, consulte los artículos siguientes:
* [Problemas comunes de configuración de red al establecer Azure API Management en una red virtual][Common network configuration problems]
* [Preguntas más frecuentes (P+F) acerca de Azure Virtual Network](../virtual-network/virtual-networks-faq.md)
* [Creating a record in DNS](https://msdn.microsoft.com/library/bb727018.aspx) (Creación de un registro en DNS)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-using-with-internal-vnet.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

[ServiceTags]: ../virtual-network/security-overview.md#service-tags

