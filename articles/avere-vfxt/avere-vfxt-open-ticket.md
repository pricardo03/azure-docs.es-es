---
title: Cómo obtener soporte técnico para Avere vFXT para Azure
description: Explicación para abrir incidencias de soporte técnico sobre Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: rohogue
ms.openlocfilehash: 068747fd51779bd8a20636355a9ce2b032f6151d
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255365"
---
# <a name="get-help-with-your-system"></a>Obtenga ayuda con el sistema

Si necesita ayuda con Avere vFXT para Azure, estas son las diversas maneras de obtener soporte técnico:

* **Problema con Avere vFXT**: use Azure Portal para abrir una incidencia de soporte técnico referente a Avere vFXT tal como se describe [a continuación](#open-a-support-ticket-for-your-avere-vfxt).
* **Cuota**: si tiene un problema relacionado con la cuota, [solicite un aumento de cuota](#request-a-quota-increase).
* **Documentación y ejemplos**: si tiene problemas con esta documentación o con los ejemplos, vaya al final de la página que tenga el problema y use la sección de **comentarios** para consultar los problemas existentes y crear uno nuevo si es necesario.  

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Abrir una incidencia de soporte técnico en Avere vFXT

Si tiene problemas al implementar o usar Avere vFXT, solicite ayuda mediante Azure Portal.  

Siga estos pasos para asegurarse de que su incidencia de soporte técnico se etiqueta con un recurso del clúster. Gracias a esta etiqueta podremos ofrecerle el recurso correcto de soporte técnico. 

1. En [https://portal.azure.com](https://portal.azure.com), seleccione **Grupos de recursos**.

   ![captura de pantalla del menú izquierdo de Azure Portal con la opción "Grupos de recursos" marcada](media/avere-vfxt-ticket-rg.png)

1. Busque el grupo de recursos que contiene el clúster de vFXT donde se produjo el problema y haga clic en una de las máquinas virtuales de Avere.

    ![captura de pantalla del panel de "información general" del grupo de recursos de Azure Portal con una máquina virtual concreta marcada](media/avere-vfxt-ticket-vm.png)

1. En la página de la máquina virtual, desplácese hasta la parte inferior del panel izquierdo y haga clic en **Nueva solicitud de soporte técnico**.

    ![Captura de pantalla de la página de la máquina virtual de Azure Portal para la máquina virtual de la captura de pantalla anterior. El menú de la izquierda se desplaza hasta la parte inferior y se resalta la opción "New support request" (Nueva solicitud de soporte técnico).](media/avere-vfxt-ticket-request.png)

1. En la página uno de la solicitud de soporte técnico, haga clic en **Todos los servicios** y vaya a **Almacenamiento** para elegir **Avere vFXT**.

    ![captura de pantalla de la nueva pantalla de solicitud de soporte técnico en Azure Portal con el encabezado "Basics" (Datos básicos) y una marca indicando el elemento "Service" (Servicio). Se selecciona el botón "All services" (Todos los servicios) y en el campo del menú desplegable se mostrará el valor "Avere vFXT"](media/avere-vfxt-ticket-service.png)

1. En la página dos, elija el tipo de problema y la categoría que mejor coincida con su problema. Agregue un título corto y una descripción que incluya la hora en la que se produjo el problema. 

   ![captura de pantalla de la nueva pantalla de solicitud de soporte técnico con el encabezado "Problem" (Problema), que contiene varios campos que deben completarse](media/avere-vfxt-ticket-problem.png)

1. En la página tres, rellene la información de contacto y haga clic en **Crear**. Se enviarán un número de confirmación y otro de vale a su dirección de correo electrónico y un miembro del personal del servicio de soporte técnico se pondrá en contacto con usted.

## <a name="request-a-quota-increase"></a>Solicitar un aumento de cuota

Lea [Quota for the vFXT cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) (Cuota para el clúster de vFXT) para obtener información sobre qué componentes son necesarios para implementar Avere vFXT para Azure. También puede [solicitar un aumento de cuota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) desde Azure Portal.