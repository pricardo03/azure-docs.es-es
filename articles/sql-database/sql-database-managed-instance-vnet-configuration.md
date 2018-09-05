---
title: Configuración de red virtual con Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describen las opciones de configuración para una red virtual con Instancia administrada de Azure SQL Database.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: b17749999f7903746651403c5948933332dbee5d
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047939"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configuración de una red virtual para Instancia administrada de Azure SQL Database

Instancia administrada de Azure SQL Database (versión preliminar) debe implementarse dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure. Esta implementación permite los siguientes escenarios: 
- Conexión a Instancia administrada directamente desde una red local 
- Conexión de Instancia administrada con un servidor vinculado o con otro almacén de datos local 
- Conexión de Instancia administrada a los recursos de Azure  

## <a name="plan"></a>Plan

Planee cómo va a implementar Instancia administrada en una red virtual utilizando las respuestas a las siguientes preguntas: 
- ¿Tiene pensado implementar una o varias instancias administradas? 

  El número de instancias administradas determina el tamaño mínimo de la subred a asignar para las mismas. Para más información, consulte la sección [Determinación del tamaño de subred para Instancia administrada](#determine-the-size-of-subnet-for-managed-instances). 
- ¿Tiene que implementar Instancia administrada en una red virtual existente o va a crear una nueva red? 

   Si piensa usar una red virtual existente, tiene que modificar la configuración de red para dar cabida a Instancia administrada. Para más información, consulte [Modificación de una red virtual existente para Instancia administrada](#modify-an-existing-virtual-network-for-managed-instances). 

   Si piensa crear una nueva red virtual, consulte la sección [Creación de una nueva red virtual para Instancia administrada](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Requisitos

Para crear una Instancia administrada, tiene que dedicar una subred dentro de la red virtual que se ajuste a los requisitos siguientes:
- **Subred dedicada**: la subred no puede contener ningún otro servicio de nube asociado a ella, y no puede ser la subred de puerta de enlace. No podrá crear una instancia administrada en una subred que contenga otros recursos que no sean los de la instancia administrada, ni podrá agregar otros recursos más adelante dentro de la subred.
- **No ser NSG**: la subred no puede tener un grupo de seguridad de red asociado a ella. 
- **Tener una tabla de rutas específica**: la subred tiene que tener una tabla de rutas de usuario (UDR) con Internet de próximo salto 0.0.0.0/0 como única ruta asignada. Para más información, consulte la sección [Creación de la tabla de rutas necesaria y su asociación](#create-the-required-route-table-and-associate-it)
3. **DNS personalizado opcional**: si se especifica un DNS personalizado en la red virtual, es necesario agregar la dirección IP de las resoluciones recursivas de Azure (por ejemplo, 168.63.129.16) a la lista. Para más información, consulte [Configuración de DNS personalizado](sql-database-managed-instance-custom-dns.md).
4. **No tener ningún punto de conexión de servicio**: la subred no puede tener un punto de conexión de servicio asociado a ella. Asegúrese de que la opción Puntos de conexión de servicio está deshabilitada cuando cree la red virtual.
5. **Tener suficientes direcciones IP**: la subred debe tener el mínimo de 16 direcciones IP (el mínimo recomendado es 32 direcciones IP). Para más información, consulte la sección [Determinación del tamaño de subred para instancias administradas](#determine-the-size-of-subnet-for-managed-instances).

> [!IMPORTANT]
> No podrá implementar una nueva instancia administrada si la subred de destino no es compatible con todos los requisitos anteriores. La red virtual de destino y la subred tienen que mantenerse de acuerdo con estos requisitos de Instancia administrada (antes y después de la implementación), ya que cualquier infracción puede provocar en la instancia un estado defectuoso y hacer que deje de estar disponible. La recuperación a partir de un estado así, requiere la creación de una nueva instancia en una red virtual con las directivas de red compatibles, la recreación de los datos a nivel de instancia y la restauración de las bases de datos. Esto supone un tiempo de inactividad considerable para las aplicaciones.

Con la introducción de la _directiva de intención de red_, puede agregar un grupo de seguridad de red (NSG) en una subred de instancia administrada una vez creada la instancia administrada.

Ahora puede usar un grupo de seguridad de red para restringir los intervalos IP desde los que las aplicaciones y los usuarios pueden consultar y administrar los datos mediante el filtrado de tráfico de red que se dirige al puerto 1433. 

> [!IMPORTANT]
> Al configurar las reglas del grupo de seguridad de red que restringirán el acceso al puerto 1433, también debe insertar las reglas de entrada de prioridad más alta que se muestran en la tabla siguiente. En caso contrario, la directiva de intención de red bloquea el cambio por no cumplir los requisitos.

| NOMBRE       |PUERTO                        |PROTOCOLO|ORIGEN           |DESTINO|ACCIÓN|
|------------|----------------------------|--------|-----------------|-----------|------|
|management  |9000, 9003, 1438, 1440, 1452|Cualquiera     |Cualquiera              |Cualquiera        |PERMITIR |
|mi_subnet   |Cualquiera                         |Cualquiera     |MI SUBNET        |Cualquiera        |PERMITIR |
|health_probe|Cualquiera                         |Cualquiera     |AzureLoadBalancer|Cualquiera        |PERMITIR |

También se ha mejorado la experiencia de enrutamiento para que además de la ruta de Internet de tipo de próximo salto 0.0.0.0/0, ahora pueda agregar UDR para enrutar el tráfico hacia intervalos IP privados en el entorno local a través de una puerta de enlace de red virtual o un dispositivo de red virtual (NVA).

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Determinación del tamaño de subred para instancias administradas

Cuando se crea una instancia administrada, Azure asigna un número de máquinas virtuales según el nivel que seleccione durante el aprovisionamiento. Debido a que estas máquinas virtuales están asociadas a la subred, requieren direcciones IP. Para garantizar la alta disponibilidad durante las operaciones normales y el mantenimiento del servicio, Azure puede asignar máquinas virtuales adicionales. Como resultado, el número de direcciones IP necesarias en una subred es mayor que el número de instancias administradas en esa subred. 

Por definición, una instancia administrada necesita un mínimo de 16 direcciones IP en una subred y puede utilizar hasta 256 direcciones IP. En consecuencia, puede utilizar máscaras de subred /28 a /24 al definir los intervalos IP de la subred. 

> [!IMPORTANT]
> El tamaño de subred con 16 direcciones IP es el mínimo imprescindible con potencial limitado para obtener una escalabilidad horizontal de instancia administrada mayor. Se recomienda elegir una subred con el prefijo /27 o uno inferior. 

Si tiene previsto implementar varias instancias administradas dentro de la subred y tiene que optimizar el tamaño de la subred, utilice estos parámetros para hacer un cálculo: 

- Azure utiliza cinco direcciones IP de la subred para sus propias necesidades 
- Cada instancia de uso general necesita dos direcciones 
- Cada instancia de Crítico para la empresa necesita cuatro direcciones

**Ejemplo**: planea tener tres instancias administradas de Propósito general y dos de Crítico para la empresa. Esto significa que necesita 5 + 3 * 2 + 2 * 4 = 19 direcciones IP. Como los intervalos de IP se definen en potencias de 2, necesita el intervalo de IP de 32 (2 ^ 5) direcciones IP. Por lo tanto, tiene que reservar la subred con la máscara de subred de /27. 

> [!IMPORTANT]
> El cálculo que se muestra arriba quedará obsoleto con las futuras mejoras. 

## <a name="create-a-new-virtual-network-for-managed-instance-using-azure-resource-manager-deployment"></a>Creación de una red virtual para Instancia administrada con la implementación de Azure Resource Manager

La manera más fácil de crear y configurar la red virtual es usar la plantilla de implementación de Azure Resource Manager.

1. Inicie sesión en el Portal de Azure.

2. Use el botón **Implementar en Azure** para implementar una red virtual en la nube de Azure:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Este botón abrirá un formulario que puede usar para configurar el entorno de red, donde puede implementar Instancia administrada.

  > [!Note]
  > Esta plantilla de Azure Resource Manager implementará una red virtual con dos subredes. Una subred llamada **ManagedInstances** está reservada para instancias administradas y tiene una tabla de rutas configurada previamente, mientras que la otra subred se denomina **Default** y se usa para otros recursos que deben tener acceso a Instancia administrada (por ejemplo, Azure Virtual Machines). Puede quitar la subred **Default** si no la necesita.

3. Configure el entorno de red. En el formulario siguiente, puede configurar parámetros de su entorno de red:

![Configuración de la red de Azure](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

Puede cambiar los nombres de la red virtual y de las subredes, y ajustar los intervalos IP asociados a sus recursos de red. Una vez que presione el botón "Comprar", se creará el formulario y configurará el entorno. Si no necesita dos subredes, puede eliminar la predeterminada. 

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Modificación de una red virtual existente para instancias administradas 

Las preguntas y respuestas de esta sección muestran cómo agregar una instancia administrada a una red virtual existente. 

**¿Está usando un modelo de implementación clásico o de Resource Manager para la red virtual existente?** 

Una Instancia administrada solo se puede crear en redes virtuales de Resource Manager. 

**¿Desea crear una subred nueva para la instancia administrada de SQL o usar una existente?**

Si desea crear una nueva: 

- Calcule el tamaño de la subred siguiendo las directrices de la sección [Determinación del tamaño de subred para instancias administradas](#determine-the-size-of-subnet-for-managed-instances).
- Siga los pasos indicados en [Incorporación, cambio o eliminación de una subred de red virtual](../virtual-network/virtual-network-manage-subnet.md). 
- Cree una tabla de rutas que contenga una única entrada, **0.0.0.0/0**, como el próximo salto Internet y asóciela a la subred para la instancia administrada.  

Si desea crear una instancia administrada dentro de una subred existente, se recomienda el siguiente script de PowerShell para preparar la subred.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
La preparación de la subred se realiza en tres sencillos pasos:

- Validación: la subred y la red virtual seleccionadas se validan para garantizar el cumplimiento de los requisitos de red de la instancia administrada.
- Confirmación: se muestra al usuario un conjunto de cambios que se deberán realizar para preparar la subred para la implementación de la instancia administrada y se le solicita su autorización.
- Preparación: la subred y la red virtual se configuran adecuadamente.

**¿Tiene configurado un servidor DNS personalizado?** 

Si es así consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md). 

- Cree la tabla de rutas necesaria y asóciela: consulte [Creación de la tabla de rutas necesaria y su asociación](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md)
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-create-tutorial-portal.md).
- Para problemas DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md)
