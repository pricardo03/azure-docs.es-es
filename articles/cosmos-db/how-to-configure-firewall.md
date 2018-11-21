---
title: Configuración del firewall por IP para la cuenta de Azure Cosmos
description: Aprenda a configurar directivas de control de acceso por IP para la compatibilidad con el firewall en cuentas de base de datos de Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: d9a00bccb83fc60c96594ffacc5abde98c0f8470
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632591"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Configuración del firewall por IP para la cuenta de Azure Cosmos

Puede proteger los datos almacenados en su cuenta de Azure Cosmos mediante el uso de firewalls por IP. Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Puede establecer un firewall por IP en la cuenta de Azure Cosmos mediante una de las maneras siguientes:

1. Desde Azure Portal
2. Mediante declaración al usar una plantilla de Azure Resource Manager
3. Mediante programación a través de la CLI de Azure o Azure PowerShell al actualizar la propiedad **ipRangeFilter**.

## <a id="configure-ip-policy"></a> Configuración de firewall por IP mediante Azure Portal

Para establecer la directiva de control de acceso de IP en Azure Portal, vaya a la página de la cuenta de Azure Cosmos DB, haga clic en **Firewall y redes virtuales** en el menú de navegación y cambie el valor de **Permitir acceso desde** a **Redes seleccionadas**. Luego, haga clic en **Guardar**. 

![Captura de pantalla que muestra cómo abrir la página Firewall en Azure Portal](./media/how-to-configure-firewall/azure-portal-firewall.png)

Una vez activado el control de acceso por IP, Azure Portal permite especificar direcciones IP específicas, rangos de direcciones IP, así como modificadores para habilitar el acceso a otros servicios de Azure y Azure Portal. En las secciones siguientes se proporcionan detalles acerca de estos modificadores.

> [!NOTE]
> Después de habilitar la directiva de control de acceso por IP para su cuenta de Azure Cosmos, se rechazan todas las solicitudes a la cuenta de Azure Cosmos desde máquinas fuera de la lista permitida de rangos de direcciones IP. La exploración de los recursos de Azure Cosmos DB desde el portal también se bloquea para garantizar la integridad del control de acceso.

### <a name="allow-requests-from-the-azure-portal"></a>Permitir las solicitudes desde Azure Portal 

Cuando se habilita una directiva de control de acceso de IP mediante programación, debe agregar la dirección IP correspondiente a la propiedad **ipRangeFilter** de Azure Portal para mantener el acceso. Las direcciones IP del portal son las siguientes:

|Region|Dirección IP|
|------|----------|
|Alemania|51.4.229.218|
|China|139.217.8.252|
|Gobierno de EE. UU.|52.244.48.71|
|Todas las regiones excepto los tres anteriores|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Para habilitar el acceso a Azure Portal, elija la opción **Permitir el acceso desde Azure Portal**, tal como se muestra en la captura de pantalla siguiente: 

![Captura de pantalla que muestra cómo habilitar el acceso a Azure Portal](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Permitir las solicitudes de los centros de datos globales de Azure u otros orígenes dentro de Azure

Si accede a la cuenta de Azure Cosmos desde los servicios que no proporcionan un IP estático, por ejemplo, el análisis de Azure Stream, Azure Functions, etc., aún podrá usar el firewall por IP para limitar el acceso. Para permitir el acceso a la cuenta de Azure Cosmos desde dichos servicios, debe habilitarse una configuración de firewall. Para habilitar la configuración del firewall, agregue la dirección IP 0.0.0.0 a la lista de direcciones IP permitidas. 0.0.0.0 restringe las solicitudes a la cuenta de Azure Cosmos desde el intervalo IP del centro de datos de Azure. Esta configuración no permite que ningún otro intervalo IP acceda a la cuenta de Azure Cosmos.

> [!NOTE]
> Esta opción configura el firewall para permitir todas las solicitudes de Azure, incluidas las solicitudes de las suscripciones de otros clientes implementados en Azure. La lista de IP permitidos por esta opción es amplia, por lo que limita la eficacia de la directiva de firewall. Esta opción debe usarse solo si las solicitudes no se originan en IP estáticos ni subredes de redes virtuales. Si se elige esta opción, se permite automáticamente el acceso desde Azure Portal, ya que Azure Portal se implementa en Azure.

Para habilitar el acceso a Azure Portal, elija la opción **Aceptar conexiones desde centros de datos públicos de Azure**, tal como se muestra en la captura de pantalla siguiente: 

![Captura de pantalla que muestra cómo abrir la página Firewall en Azure Portal](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Solicitudes desde la dirección IP actual

Para simplificar el desarrollo, Azure Portal le ayuda a identificar y agregar la dirección IP de su equipo cliente a la lista de permitidos, para que las aplicaciones que se ejecutan en la máquina puedan acceder a la cuenta de Azure Cosmos. El portal detecta automáticamente la dirección IP del cliente. Puede que sea la dirección IP cliente de la máquina, pero también la dirección IP de la puerta de enlace de red. Asegúrese de quitar esta dirección IP antes de llevar las cargas de trabajo a producción. 

Para habilitar la dirección IP actual, seleccione **Agregar mi IP actual**, que agrega la IP actual a la lista de direcciones IP y, a continuación, haga clic en **Guardar**.

![Captura de pantalla que muestra cómo configurar un firewall para la dirección IP actual](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Solicitudes desde servicios en la nube

En Azure, los servicios en la nube son una forma muy común de hospedar la lógica de servicio de nivel intermedio mediante Azure Cosmos DB. Para permitir el acceso a su cuenta de Azure Cosmos desde un servicio en la nube, la dirección IP pública del servicio en la nube se debe agregar a la lista de direcciones IP permitidas asociada a dicha cuenta de Azure Cosmos mediante la [configuración de la directiva de control de acceso a la dirección IP](#configure-ip-policy). De esta forma se garantiza que todas las instancias de rol de servicios en la nube tengan acceso a la cuenta de Azure Cosmos. Puede recuperar las direcciones IP de los servicios en la nube en Azure Portal, tal como se muestra en la siguiente captura de pantalla:

![Captura de pantalla con la dirección IP pública de un servicio en la nube que se muestra en Azure Portal](./media/how-to-configure-firewall/public-ip-addresses.png)

Al escalar horizontalmente su servicio en la nube mediante la adición de instancias de rol adicionales, esas nuevas instancias tendrán acceso automáticamente a la cuenta de Azure Cosmos que forman parte del mismo servicio en la nube.

### <a name="requests-from-virtual-machines"></a>Solicitudes desde máquinas virtuales

También se pueden usar [máquinas virtuales](https://azure.microsoft.com/services/virtual-machines/) o [conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para hospedar servicios de nivel intermedio mediante Azure Cosmos DB. Para configurar la cuenta Cosmos con el fin de permitir el acceso desde máquinas virtuales, las direcciones IP públicas de máquinas virtuales o de conjuntos de escalado de máquinas virtuales se deben configurar en una de las direcciones IP permitidas para la cuenta de Azure Cosmos mediante la [configuración de la directiva de control de acceso a la dirección IP](#configure-ip-policy). Puede recuperar las direcciones IP para máquinas virtuales en Azure Portal, como se muestra en la captura de pantalla siguiente:

![Captura de pantalla con una dirección IP pública de una máquina virtual que se muestra en Azure Portal](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Al agregar instancias adicionales de máquina virtual al grupo, se les proporciona automáticamente acceso a su cuenta de Azure Cosmos.

### <a name="requests-from-the-internet"></a>Solicitudes desde Internet

Cuando se accede a la cuenta de Azure Cosmos desde un equipo de Internet, la dirección IP del cliente o el intervalo de direcciones IP de la máquina se debe agregar a la lista de direcciones IP permitidas para dicha cuenta.

## <a id="configure-ip-firewall-arm"></a>Configuración del firewall por IP mediante una plantilla de Resource Manager

Para configurar el control de acceso a su cuenta de Azure Cosmos, la plantilla de Resource Manager debe especificar el atributo **ipRangeFilter** con una lista de intervalos IP, que deben estar en la lista blanca. Por ejemplo, agregue el siguiente código JSON a la plantilla:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Configuración de la directiva de control de acceso IP con la CLI de Azure

El comando siguiente muestra cómo crear una cuenta de Azure Cosmos con control de acceso por IP: 

```azurecli-interactive

name="<Azure Cosmos account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Para actualizar la configuración de firewall para una cuenta existente, ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>Solución de problemas de la directiva de control de acceso por IP

Puede solucionar problemas de la directiva de control de acceso por IP mediante las siguientes opciones: 

### <a name="azure-portal"></a>Azure Portal 
Al habilitar la directiva de control de acceso por IP para su cuenta de Azure Cosmos, se bloquean todas las solicitudes a la cuenta desde máquinas fuera de la lista permitida de rangos de direcciones IP. Por tanto, si quiere habilitar las operaciones en el plano de los datos de Azure Portal, como explorar los contenedores y consultar los documentos, debe permitir expresamente el acceso a Azure Portal mediante el panel **Firewall** del portal.

### <a name="sdks"></a>SDK 
Cuando acceda a recursos de Azure Cosmos mediante el SDK desde máquinas que no están en la lista de permitidos, **se devuelve una respuesta genérica 404 No encontrado sin más detalles**. Compruebe la lista de direcciones IP permitidas para su cuenta y asegúrese de que se aplique la configuración de directiva correcta a su cuenta de Cosmos. 

### <a name="check-source-ips-in-blocked-requests"></a>Comprobación de direcciones IP de origen en solicitudes bloqueadas
Habilite el registro de diagnóstico en su cuenta de Azure Cosmos, estos registros muestran cada solicitud y respuesta. **Los mensajes relacionados con el firewall se registran internamente con un código de retorno 403**. Al filtrar estos mensajes, puede ver las direcciones IP de origen para las solicitudes bloqueadas. Consulte [Registro de diagnóstico de Azure Cosmos DB](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Solicitudes de subred con el punto de conexión de servicio para la base de datos de Azure Cosmos habilitada
Las solicitudes de una subred en una red virtual que tiene habilitado el punto de conexión de servicio para Azure Cosmos DB envía la identidad de la red virtual y la subred a las cuentas de Azure Cosmos. Estas solicitudes no tienen la dirección IP pública del origen, de modo que los filtros de IP las rechazan. Para permitir el acceso desde subredes específicas en redes virtuales, agregue la lista de control de acceso de red virtual que se describe en [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos](how-to-configure-vnet-service-endpoint.md). Las reglas de firewall pueden tardar hasta 15 minutos en aplicarse.


## <a name="next-steps"></a>Pasos siguientes

Para configurar el punto de conexión de servicio de red virtual para la cuenta de Azure Cosmos, consulte los artículos siguientes:

* [Control de acceso a una red virtual y subred para la cuenta de Azure Cosmos](vnet-service-endpoint.md)
* [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos](how-to-configure-vnet-service-endpoint.md)


