---
title: Atribución de uso del cliente para asociados de Azure
description: Información general sobre cómo hacer seguimiento del uso del cliente de las soluciones de Azure Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/26/2018
ms.author: ellacroi
ms.openlocfilehash: ce862758d97737d16ef26ca7172cad39f8d8336a
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359537"
---
# <a name="azure-partner-customer-usage-attribution"></a>Atribución de uso del cliente para asociados de Azure

Como asociado de software de Azure, sus soluciones requieren componentes de Azure o que se implementen directamente en la infraestructura de Azure.  Actualmente, cuando un cliente implementa una solución de asociado y aprovisiona sus propios recursos de Azure, es difícil que el asociado obtenga una mayor visibilidad del estado de esas implantaciones y es difícil obtener una óptica del impacto en el crecimiento de Azure. Agregar un mayor nivel de visibilidad permite que los asociados estén alineados con los equipos de ventas de Microsoft y obtengan crédito por los programas para asociados de Microsoft.   

Microsoft está creando un nuevo método para ayudar a los asociados a rastrear mejor la forma en que se usa Azure, que es el resultado de que un cliente implemente su software en Azure. Este nuevo método se basa en el uso de Azure Resource Manager para organizar la implementación de los servicios de Azure.

Como asociado de Microsoft, puede asociar el uso de Azure con cualquier recurso de Azure que aprovisione en nombre del cliente.  Esta asociación se puede hacer a través de Azure Marketplace, el repositorio QuickStart, los repositorios GitHub privados e incluso los contratos de clientes 1 en 1.  Para habilitar el seguimiento, hay dos enfoques disponibles:

- Plantillas de Azure Resource Manager: son las plantillas de Azure Resource Manager o las plantillas de solución para implementar los servicios de Azure y así poder ejecutar el software del asociado. Los asociados pueden crear una plantilla de Azure Resource Manager que defina la infraestructura y la configuración de su solución de Azure. Al crear una plantilla de Azure Resource Manager, tanto usted como sus clientes pueden implementar la solución a lo largo del ciclo de vida de la misma, y tener la seguridad de que los recursos se implementan de forma coherente. 

- API de Azure Resource Manager: los asociados pueden llamar a las API de Azure Resource Manager directamente para implementar una plantilla de Azure Resource Manager o para generar las llamadas a API para aprovisionar directamente los servicios de Azure. 

## <a name="method-1-azure-resource-manager-templates"></a>Método 1: plantillas de Azure Resource Manager 

Hoy en día muchas soluciones de los asociados se implementan en una suscripción de cliente mediante plantillas de Azure Resource Manager.  Si ya tiene una plantilla de Azure Resource Manager disponible en Azure Marketplace, en GitHub o como una guía de inicio rápido, el proceso de modificación de la plantilla para habilitar este nuevo método de seguimiento es sencillo.  Si no usa una plantilla de Azure Resource Manager, aquí tiene algunos vínculos que le ayudarán a entender mejor las plantillas de Azure Resource Manager y le proporcionarán información para crear una: 

*   [Crear e implementar su primera plantilla de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Guía para crear una plantilla de solución en Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="instructions-add-a-guid-to-your-existing-azure-resource-manager-template"></a>Instrucciones: agregar un GUID a la plantilla de Azure Resource Manager existente

Agregar el GUID es una modificación única del archivo de plantilla principal:
 1. Cree un GUID; supongamos que el valor generado es eb7927c8-dd66-43e1-b0cf-c346a422063.
 2. Abra la plantilla de Azure Resource Manager.
 3. Agregue un nuevo recurso en el archivo de plantilla principal. El recurso solo debe estar en mainTemplate.json o en azuredeploy.json, no en ninguna de las plantillas vinculadas o anidadas.
 4. Escriba el GUID después "pid-", tal como se indicó anteriormente.

   Debe tener un aspecto similar al siguiente ejemplo: `pid-eb7927c8-dd66-43e1-b0cf-c346a422063`

 5. Compruebe la plantilla por si tiene errores.
 6. Vuelva a publicar la plantilla en los repositorios adecuados.

## <a name="sample-template-code"></a>Código de la plantilla de ejemplo

```

{ // add this resource to the mainTemplate.json (do not add the entire file)
      "apiVersion": "2018-02-01",
      "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your GUID here
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": []
        }
      }
    } // remove all comments from the file when done

```

## <a name="method-2-azure-resource-manager-apis"></a>Método 2: API de Azure Resource Manager

En algunos casos, es posible que prefiera realizar llamadas directamente a las API de REST de Azure Resource Manager para implementar los servicios de Azure. [Azure admite varios SDK](https://docs.microsoft.com/azure/#pivot=sdkstools) para habilitar esta opción.  Puede usar uno de los SDK, o llamar a las API REST directamente para implementar los recursos.

Si usa una plantilla de Azure Resource Manager, debe etiquetar la solución utilizando las instrucciones anteriores.  Si no está usando una plantilla de Azure Resource Manager y quiere realizar llamadas API directas, puede etiquetar la implementación para asociar el uso de los recursos de Azure. 

**Cómo etiquetar una implementación mediante las API de Azure Resource Manager:** en este caso, al diseñar las llamadas API las tendrá que incluir en un GUID en el encabezado del agente de usuario en la solicitud. Debe agregar el GUID en cada oferta o SKU.  Asimismo, la cadena debe tener un formato en el que conste el prefijo pid - y, a continuación, debe incluir el GUID que creó el asociado.   

>[!Note] 
>Este es el formato del GUID para insertarlo en el agente de usuario: pid-eb7927c8-dd66-43e1-b0cf-c346a422063     // escriba el GUID después de "pid-"

El formato de la cadena es importante. Si no se incluye el prefijo "pid-", no es posible consultar los datos. Si tiene diferentes SDK, habrá diferentes formas de hacer esto.  Para implementar este método, deberá revisar la compatibilidad y el enfoque de su SDK preferido de Azure. 

**Ejemplo de cómo utilizar el SDK de Python:** en Python, deberá usar el atributo "config". Solo lo puede agregar a un UserAgent. Este es un ejemplo:

```python

client = azure.mgmt.servicebus.ServiceBusManagementClient(**parameters)
        client.config.add_user_agent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

>Esto debe realizarse en cada cliente, ya que no hay ninguna configuración estática global (puede crear una fábrica de cliente para asegurarse de que todos los clientes están involucrados). 
>[Información de referencia adicional](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)

Cómo etiquetar una implementación mediante Azure PowerShell o la CLI de Azure: si implementa los recursos a través de AzurePowerShell, puede anexar el GUID mediante el método siguiente:

```

[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

Para anexar el GUID cuando use la CLI de Azure, establezca la variable de entorno AZURE_HTTP_USER_AGENT.  Puede establecer esta variable en el ámbito de un script o de forma global, para usarla en el ámbito de shell:

```

export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'


```

## <a name="registering-guidsoffers"></a>Registrar los GUID y las ofertas

Para incluir el GUID en el seguimiento, debe registrarlo.  

Todos los registros de los GUID de plantilla se realizarán a través de Cloud Partner Portal (CPP) de Azure Marketplace. 

Regístrese en [Azure Marketplace](http://aka.ms/listonazuremarketplace) y consiga acceso a Cloud Partner Portal.

*   Los asociados deben [tener un perfil en CPP](https://docs.microsoft.com/azure/marketplace/become-publisher), y es recomendable agregar la oferta en Azure Marketplace o AppSource. 
*   Los asociados podrán registrar varios GUID. 
*   Asimismo, los asociados también podrán registrar un GUID para las plantillas de solución que no sean de Marketplace o de ofertas.

Una vez que haya agregado el GUID a la plantilla o en el agente de usuario y haya registrado el GUID en el CPP, se realizará un seguimiento de todas las implementaciones. 

## <a name="verification-of-guid-deployment"></a>Comprobación de la implementación del GUID 

Una vez que haya modificado la plantilla y realizado una implementación de prueba, puede usar el siguiente script de PowerShell para recuperar los recursos que implementó y etiquetó. 

Puede usarlo para comprobar si el GUID se ha agregado correctamente a la plantilla de Azure Resource Manager. Recuerde que no se aplica a la implementación de la API de Azure Resource Manager.

Inicie sesión en Azure y seleccione la suscripción que contiene la implementación que quiere comprobar antes de ejecutar el script. Se debe ejecutar dentro del contexto de la suscripción de la implementación.

El nombre del grupo de recursos y el GUID de la implementación son parámetros necesarios.

Puede encontrar el script original [aquí](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1).

```

Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName'
)

#get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

#find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

#find all deploymentOperations in a deployment by name (since PowerShell does not surface outputResources on the deployment or correlationId on the deploymentOperation)

foreach ($deployment in $deployments){

#get deploymentOperations by deploymentName and then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}


```

## <a name="guidance-on-creating-guids"></a>Instrucciones sobre cómo crear GUID

Un GUID (identificador único global) es un número de referencia exclusivo de 32 dígitos hexadecimales. Para crear un GUID, debe usar un generador de GUID para crear sus GUID para el seguimiento.  Hay varios [generadores de GUID en línea](https://www.bing.com/search?q=guid%20generator&qs=n&form=QBRE&sp=-1&ghc=2&pq=guid%20g&sc=8-6&sk=&cvid=0BAFAFCD70B34E4296BB97FBFA3E1B4E) que puede usar.

Se recomienda crear un GUID único para todos los canales de distribución y oferta.  Por ejemplo, si tiene dos soluciones y ambas se implementan a través de una plantilla y están disponible en Azure Marketplace y en GitHub.  Cree cuatro GUID:

*   Oferta A en Azure Marketplace 
*   Oferta A en GitHub
*   Oferta B en Azure Marketplace 
*   Oferta B en GitHub

El mismo asociado se encargará de realizar en informe (id. de asociado de Microsoft) y el GUID. 

Asimismo, también puede realizar un seguimiento de los GUID con más detalle; es decir, puede mostrar las SKU (donde las SKU sean variantes de una oferta).

## <a name="guidance-on-privacy-and-data-collection"></a>Información sobre la privacidad y la recopilación de datos

Los asociados deben enviar mensajes para informar a sus clientes de que las implementaciones que incluyen el seguimiento de GUI de Azure Resource Manager permitirán a Microsoft informar al asociado sobre el uso de Azure relacionado con esas implementaciones.  A continuación se muestran algunos lenguajes de ejemplo. Donde se indica "ASOCIADO", debe escribir el nombre de su propia empresa. -Además, los asociados deben asegurarse de que el lenguaje está en línea con sus propias directivas de privacidad y recopilación de datos, incluyendo las opciones que permiten excluir a los clientes del seguimiento: 

**Implementaciones de plantillas de Azure Resource Manager**

Al implementar esta plantilla, Microsoft será capaz de identificar la instalación de software de ASOCIADO con los recursos de Azure implementados.  Microsoft podrá correlacionar los recursos de Azure que se usan para admitir el software.  Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Estos datos se recopilarán y se regirán en función de las directivas de privacidad de Microsoft, que se encuentra en https://www.microsoft.com/trustcenter. 

**Implementaciones de SDK o API**

Al implementar software de ASOCIADO, Microsoft será capaz de identificar la instalación de software de ASOCIADO con los recursos de Azure implementados.  Microsoft podrá correlacionar los recursos de Azure que se usan para admitir el software.  Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Estos datos se recopilarán y se regirán en función de las directivas de privacidad de Microsoft, que se encuentra en https://www.microsoft.com/trustcenter.

## <a name="support"></a>Soporte técnico

Siga estos pasos si necesita ayuda:
 1. Visite la página de soporte técnico se encuentra en [go.microsoft.com/fwlink/?linkid=844975](https://go.microsoft.com/fwlink/?linkid=844975)
 2. Si tiene problemas con la asociación de uso: seleccione el tipo de problema: **Incorporación a Marketplace** y la categoría: **Otros**; a continuación, haga clic en **Iniciar solicitud.** 
>[!Note]
>Si tiene problemas al obtener acceso a Cloud Partner Portal de Azure Marketplace: seleccione el tipo de problema: **Incorporación a Marketplace** y la categoría: **Problema de acceso**; a continuación, haga clic en **Iniciar solicitud.**
 3. Rellene los campos obligatorios en la página siguiente y haga clic en **Continuar.**
 4. Complete los campos de texto sin formato en la página siguiente.  
 

 
>[!Important] 
>Rellene el título del incidente con **"Seguimiento de uso de ISV"** y describa su problema en detalle en el campo de texto sin formato que tiene a continuación.  Complete el resto del formulario y haga clic en **Enviar**.

## <a name="faqs"></a>Preguntas más frecuentes

**¿Cuál es la ventaja de agregar el GUID a la plantilla?**

Microsoft proporcionará a sus asociados una vista de las implementaciones de clientes de sus plantillas e información detallada sobre su uso influido.  Tanto Microsoft como el asociado pueden usar esta información para impulsar una participación más estrecha entre los equipos de ventas. Microsoft y el asociado también pueden usarla para obtener una vista más coherente del impacto que un asociado individual tiene en el crecimiento de Azure. 

**¿Cómo puedo agregar un GUID a una plantilla?**

El recurso de seguimiento está pensado para conectar la solución del asociado a los clientes que usan Azure.  Los datos de uso están relacionados con la identidad de Microsoft Partner Network (MPN ID) del asociado, y los informes estarán disponibles para los asociados en Cloud Partner Portal (CPP).  

**Una vez que se ha agregado un GUID, ¿se puede cambiar?**
 
Sí, un asociado de implementación o el cliente puede personalizar la plantilla y puede cambiar o quitar el GUID. Se recomienda que los asociados describan de forma proactiva la función de los recursos y el GUID a sus clientes y asociados, para evitar la eliminación o modificación del GUID de seguimiento.  Si cambia el GUID solo afectará a los recursos y las implementaciones nuevas y no a las existentes.

**¿Cuándo estará disponible la opción de creación de informes?**

Una versión beta de la opción de creación de informes estará disponible pronto.  La creación de informes se integrará en Cloud Partner Portal (CPP).

**¿Puedo realizar un seguimiento de las plantillas implementadas desde un repositorio que no sea de Microsoft, como GitHub?**

Sí, si el GUID está presente cuando se implemente la plantilla, se realizará el seguimiento.  
Los asociados deben tener un perfil en Cloud Partner Portal para registrar las plantillas relacionadas que se hayan publicado fuera de Azure Marketplace. 

**¿Hay alguna diferencia si se implementa la plantilla en Azure Marketplace o en otros repositorios como GitHub?**

Sí, los asociados que publiquen sus ofertas en Azure Marketplace pueden recibir datos más detallados sobre las implementaciones de Azure Marketplace.  Los asociados podrán exponer sus ofertas a los clientes en el portal de Azure Marketplace y en el portal de administración de Azure. Asimismo, las ofertas de Azure Marketplace también generan clientes potenciales para el socio comercial.

**¿Qué ocurre si creo una plantilla personalizada para una involucración de clientes individuales?**

Puede agregar el GUID a la plantilla siempre que quiera.  Si usa un GUID existente que ya se haya registrado, se incluirá en el informe.  Si decide crear un nuevo GUID, deberá registrarlo para que se incluya en el seguimiento.

**¿El cliente recibe también un informe?**

Los clientes pueden realizar el seguimiento del uso de recursos individuales o grupos de recursos que hayan definido esos mismos clientes en el portal de administración de Azure.   

**¿Esta metodología de seguimiento es similar al asociado digital de registro (DPOR)?**

Este nuevo método para conectar la implementación y usarla en la solución de un asociado está pensado para proporcionar un mecanismo que permita vincular una solución de asociado al uso de Azure.  DPOR está pensado para asociar un asociado de consultoría (Integrador de sistemas) o de administración (Proveedor de servicios administrados) con la suscripción de Azure de un cliente.   
