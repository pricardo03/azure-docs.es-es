---
title: Controlar Azure instancias grandes de HANA a través del portal de Azure | Microsoft Docs
description: Describe cómo puede identificar e interactuar con instancias grandes de HANA de Azure a través del portal
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/02/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8240308b3e0955b1d4d3ef2e82cad215daf95b00
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009375"
---
# <a name="azure-hana-large-instances-control-through-azure-portal"></a>Control de instancias grandes de HANA en Azure mediante Azure Portal
Este documento describe el procedimiento [instancias grandes de HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) se presentan en [portal Azure](https://portal.azure.com) y qué actividades pueden realizarse a través del portal de Azure con unidades de instancia grande de HANA que se implementan. Visibilidad de instancias grandes de HANA en Azure portal se proporciona a través de un proveedor de recursos de Azure para HANA instancias grandes, que actualmente se encuentra en versión preliminar pública

## <a name="register-hana-large-instance-resource-provider"></a>Registrar proveedor de recursos de instancia grande de HANA
Normalmente, la suscripción de Azure que se usaba para implementaciones de instancias grandes de HANA se registra para el proveedor de recursos de instancia grande de HANA. Sin embargo, si no ve que implementa unidades de instancia grande de HANA, debe registrar el proveedor de recursos en su suscripción de Azure. Hay dos maneras de registrar el proveedor de recursos de instancia grande de HANA

### <a name="register-through-cli-interface"></a>Registrarse a través de la interfaz CLI
Debe haber iniciado sesión en su suscripción de Azure usada para la implementación de instancia grande de HANA a través de la interfaz de CLI de Azure. Puede (re) del registro el proveedor de instancias grandes de HANA con este comando:
    
    az provider register --namespace Microsoft.HanaOnAzure

Para obtener más información, vea el artículo [tipos y proveedores de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-cli)


### <a name="register-through-azure-portal"></a>Registrarse a través del portal de Azure
Puede (re) del registro el proveedor de recursos de instancia grande de HANA a través del portal de Azure. Necesario mostrar su suscripción en Azure portal y haga doble clic en la suscripción, que se usó para implementar sus unidades de instancia grande de HANA. Uno se encuentra en la página de información general de su suscripción, seleccione "Proveedores de recursos" tal como se muestra a continuación y escriba "HANA" en la ventana de búsqueda. 

![Registrar HLI RP a través del portal de Azure](./media/hana-li-portal/portal-register-hli-rp.png)

En la captura de pantalla que se muestra, ya se registró el proveedor de recursos. En caso de que todavía no está registrado el proveedor de recursos, presione "Re-register" o "registrar".

Para obtener más información, vea el artículo [tipos y proveedores de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-powershell)


## <a name="display-of-hana-large-instance-units-in-the-azure-portal"></a>Visualización de unidades de instancia grande de HANA en Azure portal
Al enviar una solicitud de implementación de instancia grande de HANA, deberá especificar la suscripción de Azure que se conectan a instancias grandes de HANA así. Se recomienda, utilizar la misma suscripción que se usa para implementar el nivel de aplicación que funciona con las unidades de instancia grande de HANA.
Como su primera introducción se implementan instancias grandes de HANA, una nueva [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) se crea en la suscripción de Azure que ha enviado en la solicitud de implementación para las instancias grandes de HANA.  Nuevo grupo de recursos enumerará todas las unidades de instancia grande de HANA que se ha implementado en la suscripción específica.

Para encontrar el nuevo grupo de recursos de Azure, enumerar el grupo de recursos en su suscripción, vaya a través del panel de navegación izquierdo de Azure portal

![Panel de navegación en Azure portal](./media/hana-li-portal/portal-resource-group.png)

En la lista de grupos de recursos, Introducción aparece, puede que necesite filtrar según la suscripción que usó para tener las instancias grandes de HANA implementadas

![Filtrar los grupos de recursos en Azure portal](./media/hana-li-portal/portal-filtering-subscription.png)

Después de filtrado a la suscripción correcta, todavía puede tener una larga lista de grupos de recursos. Busque uno con una revisión posterior a la de **- Txxx** donde "xxx" es tres dígitos, como **-T050**. 

Tal y como se encuentra el grupo de recursos, enumera los detalles de la misma. La lista recibió podría ser similar a:

![Lista HLI en Azure portal](./media/hana-li-portal/portal-hli-units-list.png)

Todas las unidades que se muestran son que representan una sola unidad de instancia grande de HANA que se ha implementado en su suscripción. En este caso, examine ocho unidades de instancia grande de HANA diferentes, que se implementaron en su suscripción.

Si ha implementado a varios inquilinos de instancia grande de HANA en la misma suscripción de Azure, encontrará varios grupos de recursos de Azure 


## <a name="look-at-attributes-of-single-hli-unit"></a>Examine los atributos de unidad HLI
En la lista de las unidades de instancia grande de HANA, puede haga clic en una sola unidad y obtener los detalles de la única unidad de instancia grande de HANA. 

En la pantalla de información general, recibe una presentación de la unidad, que es similar a:

![Muestra una visión general de una unidad HLI](./media/hana-li-portal/portal-show-overview.png)

Examinar se muestran los atributos diferentes, esos atributos parecen apenas diferentes a los atributos de la máquina virtual de Azure. En el encabezado de la izquierda, muestra el grupo de recursos, región de Azure, nombre de la suscripción y ID, así como algunas etiquetas que ha agregado. De forma predeterminada, las unidades de instancia grande de HANA no tienen etiqueta asignada. En el lado derecho del encabezado, se muestra el nombre de la unidad asignado cuando se realiza la implementación. Se muestra el sistema operativo, así como la dirección IP. Como con máquinas virtuales que se escriba la unidad de instancia grande de HANA con el número de CPU subprocesos y memoria también se muestra. Obtener más detalles sobre las diferentes unidades de instancia grande de HANA, se muestran aquí:

- [SKU disponibles para HLI](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus)
- [Arquitectura de almacenamiento de SAP HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-storage-architecture) 

Un campo adicional en la columna derecha del encabezado informa sobre el estado de energía de la unidad de instancia grande de HANA.

> [!NOTE]
> El estado de energía describe si la unidad de hardware se alimenta activado o desactivado. No se proporciona información sobre el sistema operativo, puesta en marcha. Como el reinicio de una unidad de instancia grande de HANA experimentarán un poco de tiempo donde se cambia el estado de la unidad a **iniciando** para mover el estado de **iniciado**. El estado de **iniciado** significa que el sistema operativo se está iniciando o que el sistema operativo ha se ha iniciado completamente. Como resultado, después del reinicio de la unidad, no se puede esperar iniciar inmediatamente la sesión en la unidad tan pronto como el estado cambia a **iniciado**.
> 


## <a name="check-activities-of-a-single-hana-large-instance-unit"></a>Compruebe las actividades de una sola unidad de instancia grande de HANA 
Más allá de lo que proporciona una visión general de las unidades de instancia grande de HANA, puede comprobar las actividades de la unidad particular. Un registro de actividad podría ser similar a:

![Panel de navegación en Azure portal](./media/hana-li-portal/portal-activity-list.png)

Una de las actividades principales que se registran son reinicios de una unidad. Los datos de muestra incluyen el estado de la actividad de la marca de tiempo de desencadenar la actividad, el identificador de suscripción de que desencadena la actividad y el usuario de Azure que desencadenó la actividad. 

Otra actividad que está grabando son cambios en la unidad en los metadatos de Azure. Además de reinicio iniciado, puede ver la actividad de **HANAInstances escribir**. Este tipo de actividad no realiza ningún cambio en la propia unidad de instancia grande de HANA, pero es documentar los cambios realizados en los metadatos de la unidad de Azure. En el caso de que aparece, se agrega y elimina una etiqueta (consulte la sección siguiente).

## <a name="add-and-delete-an-azure-tag-to-a-hana-large-instance-unit"></a>Agregar y eliminar una etiqueta de Azure a una unidad de instancia grande de HANA
Otra posibilidad tiene consiste en Agregar un [etiqueta](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) a una unidad de instancia grande de HANA. La manera en que se les asigne etiquetas no difiere de la asignación de etiquetas a las máquinas virtuales. Como con las máquinas virtuales que existen las etiquetas en los metadatos de Azure y, para instancias grandes de HANA, tiene las mismas restricciones que las etiquetas para las máquinas virtuales.

Eliminar etiquetas funciona del mismo modo que con las máquinas virtuales. Ambas actividades, aplicar y eliminar una etiqueta se mostrará en el registro de actividad de la unidad de instancia grande de HANA determinado.

## <a name="check-properties-of-a-hana-large-instance-unit"></a>Comprobar las propiedades de una unidad de instancia grande de HANA
La sección **propiedades** incluye información importante que se obtiene cuando se le entregó a través de las instancias de a. Es una sección donde obtendrá toda la información que podría ser necesario en la compatibilidad de los casos o que necesita para establecer una configuración de la instantánea de almacenamiento. Por lo tanto, esta sección es una colección de datos en torno a la instancia, la conectividad de la instancia de Azure y el back-end de almacenamiento. La parte superior de la sección siguiente aspecto:


![parte superior de las propiedades HLI en Azure portal](./media/hana-li-portal/portal-properties-top.png)

Los primeros elementos datos, que ya se mostró en la pantalla información general. Pero una parte importante de datos es el identificador de circuito de ExpressRoute, que obtuvo como la primera unidades implementadas se han traspasado. En algunos casos de soporte técnico, puede que obtenga le pida que los datos. Una entrada de datos importantes se muestra en la parte inferior de la captura de pantalla. Los datos de muestra están la dirección IP de la cabeza del almacenamiento NFS que aísla el almacenamiento para su **inquilino** en la pila de instancia grande de HANA. Esta dirección IP también es necesario cuando se edita la [copias de seguridad de instantánea de archivo de configuración de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore#set-up-storage-snapshots). 

Como se desplaza hacia abajo en el panel de propiedades, obtener datos adicionales, como un identificador de recurso único para la unidad de instancia grande de HANA o el identificador de suscripción que se asignó a la implementación.

## <a name="restart-a-hana-large-instance-unit-through-azure-portal"></a>Reiniciar una unidad de instancia grande de HANA a través del portal de Azure
Iniciar un reinicio del sistema operativo Linux, hay varias situaciones donde el sistema operativo no ha podido finalizar un reinicio correctamente. Con el fin de forzar un reinicio, es necesario para abrir una solicitud de servicio para las operaciones de Microsoft lleva a cabo un reinicio de alimentación de la unidad de instancia grande de HANA. La funcionalidad de un reinicio de energía de una unidad de instancia grande de HANA está integrada ahora en el portal de Azure. Que se encuentre en la parte de información general de la unidad de instancia grande de HANA, verá el botón de reinicio de la sección de datos

![Reinicie paso #1 en el portal de Azure](./media/hana-li-portal/portal-restart-first-step.png)

Como se presiona el botón de reinicio, le pregunta si realmente desea reiniciar la unidad. Cuando confirme presionando el botón "Sí", se reiniciará la unidad.

> [!NOTE]
> En el proceso de reinicio, experimentará un poco de tiempo donde se cambia el estado de la unidad a **iniciando** para mover el estado de **iniciado**. El estado de **iniciado** significa que el sistema operativo se está iniciando o que el sistema operativo ha se ha iniciado completamente. Como resultado, después del reinicio de la unidad, no se puede esperar iniciar inmediatamente la sesión en la unidad tan pronto como el estado cambia a **iniciado**.

> [!IMPORTANT]
> Según la cantidad de memoria en la unidad de instancia grande de HANA, un reinicio y el reinicio del hardware y el sistema operativo pueden tardar hasta una hora


## <a name="open-a-support-request-for-hana-large-instances"></a>Abra una solicitud de soporte técnico para instancias grandes de HANA
Fuera de la pantalla de portal de Azure de unidades de instancia grande de HANA, puede crear solicitudes de soporte técnico específicamente para una unidad grande de HANA instancia así. Cuando siga el vínculo **nueva solicitud de soporte técnico** 

![iniciar servicio solicitud paso #1 en el portal de Azure](./media/hana-li-portal/portal-initiate-support-request.png)

Para obtener el servicio de instancias grandes de SAP HANA aparecen en la siguiente pantalla, es posible que deba seleccionar ' todos los servicios "tal como se muestra a continuación

![Seleccione todos los servicios en Azure portal](./media/hana-li-portal/portal-create-service-request.png)

En la lista de servicios, puede encontrar el servicio **SAP HANA instancias grandes**. Como desee que el servicio, puede seleccionar tipos de problemas específicos, tal como se muestra:


![Seleccione la clase problem en Azure portal](./media/hana-li-portal/portal-select-problem-class.png)

En cada uno de los tipos de problemas diferentes, se le ofrece una selección de subtipos de problema que se debe seleccionar para caracterizar su problema. Después de seleccionar el subtipo, ahora puede nombrar al asunto. Cuando haya terminado con el proceso de selección, puede mover al siguiente paso de la creación. En el **soluciones** sección, se apunta a la documentación en torno a instancias grandes de HANA, que puede dar a un puntero a una solución del problema. Si no se puede encontrar una solución para su problema en la documentación de sugerido, vaya al paso siguiente. En el siguiente paso, va que se le pregunte si el problema es con las máquinas virtuales o con unidades de instancia grande de HANA. Esta información ayuda a dirigir la solicitud de soporte técnico a los especialistas en correctos. 

![Detalles del caso de soporte técnico en Azure portal](./media/hana-li-portal/portal-support-request-details.png)

Como respuesta a las preguntas y proporciona detalles adicionales, puede ir el paso siguiente para revisar la solicitud de soporte técnico y el envío de él.

## <a name="next-steps"></a>Pasos siguientes

- [Procedimiento para supervisar SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)
- [Supervisión y solución de problemas en el lado HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot)

