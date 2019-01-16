---
title: Atribución de uso del cliente para asociados de Azure
description: Información general sobre cómo hacer seguimiento del uso del cliente de las soluciones de Azure Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/17/2018
ms.author: yijenj
ms.openlocfilehash: da8fd886afa6028959247caa0c6e007df20fdd1b
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077749"
---
# <a name="azure-partner-customer-usage-attribution"></a>Atribución de uso del cliente para asociados de Azure

Como asociado de software de Azure, sus soluciones requieren componentes de Azure o necesitan implementarse directamente en la infraestructura de Azure. A los clientes que implementen una solución de asociado y aprovisionen sus propios recursos de Azure puede que les resulte difícil ver el estado de la implementación y lo que esta afecta al crecimiento de Azure. Al agregar un mayor nivel de visibilidad, se alinea con el equipo de ventas de Microsoft y obtiene crédito para los programas para partners de Microsoft. 

Ahora, Microsoft ofrece un método que ayuda a los asociados a mejorar el seguimiento del uso de Azure de las implementaciones de clientes de su software en Azure. Este nuevo método usa Azure Resource Manager para organizar la implementación de los servicios de Azure.

Como asociado de Microsoft, puede asociar el uso de Azure con todos los recursos de Azure que aprovisione en nombre de un cliente. La asociación puede formarla a través de Azure Marketplace, el repositorio del Inicio rápido, repositorios privados de GitHub y la involucración del cliente cara a cara. Para habilitar el seguimiento, hay dos enfoques disponibles:

- Plantillas de Azure Resource Manager: las plantillas de Resource Manager o las plantillas de solución para implementar los servicios de Azure para ejecutar el software del asociado. Los asociados pueden crear una plantilla de Resource Manager para definir la infraestructura y configuración de su solución de Azure. Una plantilla de Resource Manager les permite tanto a usted como a sus clientes implementar la solución a lo largo de su ciclo de vida. Puede estar seguro de que los recursos se implementan de forma consistente. 
- API de Azure Resource Manager: los asociados pueden llamar a las API de Resource Manager directamente para implementar una plantilla de Azure Resource Manager o para generar las llamadas a API para aprovisionar directamente los servicios de Azure. 

Se requiere la atribución de uso del cliente en todas las [plantillas de solución](./cloud-partner-portal-orig/cloud-partner-portal-solution-template-offer-publish.md) publicadas en Azure Marketplace. 

## <a name="use-resource-manager-templates"></a>Uso de plantillas de Resource Manager

Muchas soluciones de los asociados se implementan en una suscripción de cliente mediante plantillas de Resource Manager. Si tiene una plantilla de Resource Manager disponible en Azure Marketplace, en GitHub o como un Inicio rápido, el proceso de modificación de la misma para habilitar este nuevo método de seguimiento debería ser sencillo. Para más información, consulte [Creación e implementación de la primera plantilla de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).


## <a name="add-a-guid-to-your-template"></a>Adición de un identificador único global a una plantilla

Para agregar un identificador único global (GUID), se realizas una modificación única en el archivo de plantilla principal:

1. [Cree un GUID](#create-guids) mediante el método sugerido y [registre el GUID](#register-guids-and-offers).

1. Abra la plantilla de Resource Manager.

1. Agregue un nuevo recurso en el archivo de plantilla principal. El recurso solo debe estar en los archivos **mainTemplate.json** o **azuredeploy.json**, no en ninguna de las plantillas vinculadas o anidadas.

1. Escriba el valor de GUID después del prefijo **pid -** (por ejemplo, pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Compruebe si la plantilla tiene errores.

1. Vuelva a publicar la plantilla en los repositorios adecuados.

1. [Compruebe que el GUID es correcto en la implementación de plantillas](#verify-the-guid-deployment).

### <a name="sample-resource-manager-template-code"></a>Código de ejemplo de plantilla de Resource Manager

Para habilitar el seguimiento de recursos de la plantilla, deberá agregar los siguientes recursos adicionales en la sección de recursos. Asegúrese de modificar el siguiente código de ejemplo con sus propias entradas cuando lo agregue al archivo de plantilla principal.
El recurso solo se debe agregar al archivo **mainTemplate.json** o **azuredeploy.json**, y no a plantillas vinculadas o anidadas.
```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>Uso de las API de Resource Manager

En algunos casos, es posible que prefiera realizar llamadas directamente a las API REST de Resource Manager para implementar los servicios de Azure. [Azure admite varios SDK](https://docs.microsoft.com/azure/#pivot=sdkstools) para habilitar estas llamadas. Puede usar uno de los SDK, o llamar a las API REST directamente para implementar los recursos.

Si usa una plantilla de Resource Manager, para etiquetar la solución debe seguir las instrucciones que se han proporcionado anteriormente. Si no usa una plantilla de Resource Manager y realiza llamadas API directas, puede etiquetar la implementación para asociar el uso de los recursos de Azure. 

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Etiquetado de una implementación con las API de Resource Manager

En este método de seguimiento, al diseñar las llamadas API, se incluye un identificador único global en el encabezado del agente de usuario de la solicitud. Agregue el identificador único global a cada oferta o SKU. Dé formato a la cadena con el prefijo **pid -** e incluya el identificador único global generado por el asociado. Este es un ejemplo del formato del identificador único global para la inserción en el agente de usuario: 

![Formato de GUID de ejemplo](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> El formato de la cadena es importante. Si no se incluye el prefijo **pid-**, no es posible consultar los datos. El seguimiento de los distintos SDK se hace de forma diferente. Para implementar este método, revise la compatibilidad y el enfoque del seguimiento de su SDK de Azure preferido. 

#### <a name="example-the-python-sdk"></a>Ejemplo: SDK de Python

Para Python, use el atributo **config**. El atributo solo se puede agregar a un UserAgent. Este es un ejemplo:

![Agregar el atributo a un agente de usuario](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Agregue el atributo a cada cliente. No hay ninguna configuración estática global. Puede etiquetar una fábrica de cliente para asegurarse de que todos los clientes realizan el seguimiento. Para más información, consulte este [ejemplo de fábrica de cliente en GitHub](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79).

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Etiquetado de una implementación mediante Azure PowerShell

Si implementa recursos a través de Azure PowerShell, use el siguiente método para anexar el identificador único global:

```
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Etiquetado de una implementación mediante la CLI de Azure

Cuando use la CLI de Azure para anexar un identificador único global, establezca la variable de entorno **AZURE_HTTP_USER_AGENT**. Dicha variable se puede establecer en el ámbito de un script. Pero también se puede establecer globalmente para el ámbito de una shell:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```

## <a name="create-guids"></a>Creación de los identificadores únicos globales

Un GUID es un número de referencia único que tiene 32 dígitos hexadecimales. Para crear identificadores únicos globales para el seguimiento, debe usar un generador de GUID. El equipo de Azure Storage ha creado un [formulario de generación de GUID](https://aka.ms/StoragePartners) que enviará por correo electrónico un GUID en el formato correcto. Este GUID se puede reutilizar en los distintos sistemas de seguimiento. 

> [!Note]
> Es muy recomendable usar el [formulario del generador de GUID de Azure Storage](https://aka.ms/StoragePartners) para crear su GUID. Para más información, consulte las [P+F](#faq).

Se recomienda crear un GUID único para todos los canales de distribución y oferta de cada producto. Puede optar por usar un solo GUID para los múltiples canales de distribución del producto si no quiere que los informes se dividan. 

Si implementa un producto mediante una plantilla y está disponible tanto en Azure Marketplace como en GitHub, puede crear y registrar 2 GUID distintos:

*   Producto A en Azure Marketplace 
*   Producto A en GitHub

La creación de informes se realiza en función del valor de asociado comercial (id. de Partner de Microsoft) y de los GUID. 

Asimismo, si lo desea también puede realizar un seguimiento de los identificadores únicos globales a un nivel más detallado como el SKU donde las SKU son variantes de una oferta.

## <a name="register-guids-and-offers"></a>Registro de identificadores únicos globales y ofertas

Para incluir un identificador único global en el seguimiento, debe estar registrado.  

Todos los registros de identificadores únicos globales de plantillas se realizan a través de Cloud Partner Portal (CPP) de Azure Marketplace. 

Después de agregar el identificador único global a la plantilla o en el agente de usuario, y registrarlo en CPP, se realizará un seguimiento de todas las implementaciones. 

1. Regístrese en [Azure Marketplace](https://aka.ms/listonazuremarketplace) y consiga acceso a CPP.

   * Los asociados deben [tener un perfil en CPP](https://docs.microsoft.com/azure/marketplace/become-publisher). Le recomendamos que vea la oferta en Azure Marketplace o AppSource.
   * Los asociados pueden registrar varios identificadores únicos globales.
   * Los asociados puede registrar un identificador único global tanto para las plantillas de solución que no sean de Marketplace como para las ofertas.
 
1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

1. En la esquina superior derecha, seleccione el icono de su cuenta y, después, seleccione **Perfil de publicador**.

   ![Seleccionar Perfil de publicador](media/marketplace-publishers-guide/guid-image-for-lu.png)

1. En la **página de perfil**, haga clic en **Add Tracking GUID** (Agregar GUID de seguimiento).

   ![Seleccionar Add Tracking GUID (Agregar GUID de seguimiento)](media/marketplace-publishers-guide/guid-how-to-add-tracking.png)

1. En el cuadro **Tracking GUID** (GUID de seguimiento), escriba su identificador único global de seguimiento. Escriba solo el GUID, sin el prefijo **pid -**. En el cuadro **Custom Description** (Descripción personalizada), escriba el nombre o la descripción de la oferta.

   ![Página de perfil](media/marketplace-publishers-guide/guid-dev-center-login.png)
   
   ![Escriba el identificador único global y la descripción de la oferta](media/marketplace-publishers-guide/guid-dev-center-example.png)

1. Para registrar varios identificadores únicos globales, vuelva a seleccionar **Add Tracking GUID** (Agregar GUID de seguimiento). Aparecen más cuadros en la página.

   ![Volver a seleccionar Add Tracking GUID (Agregar GUID de seguimiento)](media/marketplace-publishers-guide/guid-dev-center-example-add.png)
   
   ![Escribir otro identificador único global y la descripción de la oferta](media/marketplace-publishers-guide/guid-dev-center-example-description.png)

1. Seleccione **Guardar**.

   ![Seleccionar Guardar](media/marketplace-publishers-guide/guid-dev-center-save.png)

Después de agregar el identificador único global a la plantilla o en el agente de usuario, y registrarlo en CPP, se realizará un seguimiento de todas las implementaciones. 

## <a name="verify-the-guid-deployment"></a>Comprobación de la implementación de GUID 

Tras modificar la plantilla y realizar una implementación de prueba, use el siguiente script de PowerShell para recuperar los recursos que implementó y etiquetó. 

Puede usar dicho script para comprobar que el GUID se ha agregado correctamente a la plantilla de Resource Manager. El script no se aplica a la implementación de la API de Resource Manager.

Inicie sesión en Azure. Antes de ejecutar el script seleccione la suscripción con la implementación que desea comprobar. Ejecute el script en el contexto de la suscripción de la implementación.

Tanto el **GUID** como el nombre **resourceGroup** son parámetros necesarios.

[El script original](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) se puede obtener en GitHub.

```
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="notify-your-customers"></a>Notificación a los clientes

Los asociados deben informar a sus clientes de las implementaciones que usan el seguimiento de GUID de Resource Manager. Microsoft notifica al asociado el uso de Azure que está asociado con estas implementaciones. Los ejemplos siguientes incluyen contenido que puede usar para notificar a los clientes estas implementaciones. En los ejemplos, reemplace \<PARTNER > por el nombre de su empresa. Los asociados deben asegurarse de que la notificación es compatible con sus directivas de privacidad y recopilación de datos, incluyendo las opciones que permiten excluir a los clientes del seguimiento. 

### <a name="notification-for-resource-manager-template-deployments"></a>Notificación de implementaciones de plantillas de Resource Manager

Al implementar esta plantilla, Microsoft puede identificar la instalación del software de \<PARTNER> con los recursos de Azure que se implementan. Microsoft puede correlacionar los recursos de Azure que se usan para admitir el software. Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Los datos se recopilan en función de las directivas de privacidad de Microsoft, que se encuentran en https://www.microsoft.com/trustcenter, y se rigen por ellas. 

### <a name="notification-for-sdk-or-api-deployments"></a>Notificación para las implementaciones de SDK o API

Al implementar el software \<PARTNER>, Microsoft puede identificar la instalación del software de \<PARTNER> con los recursos de Azure que se implementan. Microsoft puede correlacionar los recursos de Azure que se usan para admitir el software. Microsoft recopila esta información para proporcionar las mejores experiencias con sus productos y conseguir que sus negocios funcionen. Los datos se recopilan en función de las directivas de privacidad de Microsoft, que se encuentran en https://www.microsoft.com/trustcenter, y se rigen por ellas.

## <a name="get-support"></a>Obtención de soporte técnico

Si necesita ayuda, siga estos pasos.

1. Vaya a la [página de soporte técnico](https://go.microsoft.com/fwlink/?linkid=844975). 

1. En **Problem type** (Tipo de problema), seleccione **Marketplace Onboarding** (Incorporación a Marketplace).

1. En **Category** (Categoría), elija la categoría del problema:

   - Para problemas de asociación de uso, seleccione **Other** (Otros).
   - Para problemas de acceso al CPP de Azure Marketplace, seleccione **Access Problem** (Problema de acceso).
   
    ![Elegir la categoría de problema](media/marketplace-publishers-guide/lu-article-incident.png)

1. Haga clic en **Start Request** (Iniciar solicitud).

1. En la página siguiente, especifique los valores necesarios. Seleccione **Continuar**.

1. En la página siguiente, especifique los valores necesarios.

   > [!Important] 
   > En el cuadro **Incident title** (Título de la incidencia), escriba **Seguimiento del uso de ISV**. Describa el problema de forma detallada.
   
   ![Escribir Seguimiento del uso de ISV como título de la incidencia](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Complete el formulario y seleccione **Submit** (Enviar).

## <a name="faq"></a>Preguntas más frecuentes

**¿Cuál es la ventaja de agregar el identificador único global a la plantilla?**

Microsoft proporciona a sus asociados una vista de las implementaciones por parte de los clientes de sus plantillas, así como información detallada sobre su uso de influencia. Tanto Microsoft como el asociado pueden usar dicha información para impulsar una participación más estrecha entre los equipos de ventas. Tanto Microsoft como el asociado pueden usar los datos para obtener una vista más coherente del impacto que un asociado individual tiene en el crecimiento de Azure. 

**¿Cómo puedo agregar un GUID a una plantilla?**

El recurso de seguimiento está pensado para conectar la solución del asociado al uso de Azure por parte del cliente. Los datos de uso están vinculados a la identidad de Microsoft Partner Network (MPN ID) de un asociado. La creación de informes está disponibles para los asociados en el CPP.

**Una vez que se ha agregado un identificador único global, ¿se puede cambiar?**
 
Sí, cualquier cliente o asociado de implementación puede personalizar la plantilla y cambiar o quitar el identificador único global. Se recomienda que los asociados describan de forma proactiva la función de los recursos y el GUID a sus clientes y asociados, para evitar la eliminación o modificación del GUID de seguimiento. El cambio del identificador único global solo afecta a las implementaciones y a los recursos nuevos, no a los existentes.

**¿Cuándo estará disponible la opción de creación de informes?**

Una versión beta de la opción de creación de informes estará disponible pronto. La creación de informes se integrará en CPP.

**¿Puedo realizar un seguimiento de las plantillas implementadas desde un repositorio que no sea de Microsoft, como GitHub?**

Sí, si el GUID está presente cuando se implemente la plantilla, se realiza el seguimiento. Los asociados deben tener un perfil en CPP para registrar las plantillas relacionadas que se publican fuera de Azure Marketplace. 

**¿Hay alguna diferencia si se implementa la plantilla en Azure Marketplace o en otros repositorios como GitHub?**

Sí, los asociados que publiquen sus ofertas en Azure Marketplace pueden recibir datos más detallados acerca de las implementaciones que se realizan desde Azure Marketplace. Los asociados pueden exponer sus ofertas a los clientes tanto en el portal de Azure Marketplace como en Azure Portal. Las ofertas de Azure Marketplace también generan clientes potenciales al asociado.

**¿Qué ocurre si creo una plantilla personalizada para una involucración de clientes individuales?**

El identificador único global se puede agregar a la plantilla siempre que se desee. Si usa un identificador único global registrado existente, se incluye en la creación de informes. Si crea un identificador único global nuevo, es preciso que lo registre para que se incluya en el seguimiento.

**¿El cliente recibe también un informe?**

Los clientes pueden realizar el seguimiento del uso tanto de recursos individuales como de grupos de recursos definidos por los usuarios en Azure Portal.   

**¿Esta metodología de seguimiento es similar al asociado digital de registro (DPOR)?**

Este nuevo método de conexión de la implementación y el uso en la solución de un asociado proporciona un mecanismo que permite vincular una solución de asociado al uso de Azure. DPOR está pensado para asociar un asociado de consultoría (integrador de sistemas) o de administración (proveedor de servicios administrados) con la suscripción de Azure de un cliente.   

**¿Cuál es la ventaja de usar el formulario del generador de GUID de Azure Storage?**

El formulario del generador de GUID de Azure Storage garantiza la generación de un GUID del formato necesario. Además, si usa alguno de los métodos de seguimiento de planos de datos de Azure Storage, puede aprovechar el mismo GUID para el seguimiento de planos de control de Marketplace. Esto le permite aprovechar un único GUID unificado para la atribución de asociado sin tener que mantener distintos GUID.
