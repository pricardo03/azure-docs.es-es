---
title: 'Creación y asociación de directivas de punto de conexión de servicio: Azure Portal'
titlesuffix: Azure Virtual Network
description: En este artículo, aprenderá cómo configurar y asociar directivas de punto de conexión de servicio mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 09/18/2018
ms.author: kumud
ms.openlocfilehash: aaa07759ed8b10578d024d5838ac1d2658778695
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034672"
---
# <a name="create-change-or-delete-service-endpoint-policy-using-the-azure-portal"></a>Creación, cambio o eliminación de la directiva de punto de conexión de servicio mediante Azure Portal

Las directivas de puntos de conexión de servicio le permiten filtrar el tráfico de red virtual a recursos específicos de Azure, sobre puntos de conexión de servicio. Si no está familiarizado con estas directivas, consulte [Directivas de punto de conexión de servicio](virtual-network-service-endpoint-policies-overview.md) para más información.

 En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva de punto de conexión de servicio
> * Definición de Crear una directiva de punto de conexión de servicio
> * Creación de una red virtual con una subred
> * Asociación una directiva de punto de conexión de servicio a una subred

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-service-endpoint-policy"></a>Creación de una directiva de punto de conexión de servicio

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. En el panel de búsqueda, escriba "directiva de punto de conexión de servicio" y seleccione **Directiva de punto de conexión de servicio (versión preliminar)** y, a continuación, seleccione **Crear**.
3. Escriba o seleccione la siguiente información en **Básico**. 

   - Suscripción: seleccione la suscripción por directiva.    
   - Grupo de recursos: Haga clic en **Crear nuevo** y escriba *myResourceGroup*.     
   - Nombre: myEndpointPolicy
   - Ubicación: Centro occidental de EE.UU.     
 
   ![Conceptos básicos de Crear una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-startpane.PNG)
   
4. Especifique o seleccione la información siguiente en **Definiciones de directivas**.

   - En **+ Agregar un recurso**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de las opciones y, a continuación, seleccione **Agregar**.  
   - Ámbito: seleccione **Cuenta única** , **Todas las cuentas de la suscripción**  o **Todas las cuentas del grupo de recursos** .    
   - Suscripción: seleccione la suscripción para la cuenta de almacenamiento. Las cuentas de almacenamiento y las directivas pueden estar en distintas suscripciones.   
   - Grupo de recursos: Seleccione el grupo de recursos que necesite. Requerido, si el ámbito está establecido como "Todas las cuentas del grupo de recursos" o "Cuenta única".  
   - Recurso: mystorageaccountportal    
   - Haga clic en **+ Agregar un recurso** para seguir agregando más recursos.
   
   ![Definiciones de Crear una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-policydefinitionspane.PNG)
   
5. Opcional: escriba o seleccione la siguiente información en **Etiquetas**:
   
   - Clave: seleccione la clave de la directiva. Por ejemplo: Dept     
   - Valor: escriba el par de valor de la clave. Por ejemplo: Finance

6. Seleccione **Revisar + crear**. Valide la información y haga clic en **Crear**. Para hacer más modificaciones, haga clic en **Anterior**. 

   ![Validaciones finales de Crear una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-finalcreatereview.PNG)
  
 
## <a name="view-endpoint-policies"></a>Visualización de directivas de punto de conexión de servicio 

1. En el cuadro *Todos los servicios* del portal, comience a escribir *directivas de punto de conexión de servicio*. Seleccione **Directivas de punto de conexión de servicio (versión preliminar)**.
2. En **Suscripciones**, seleccione la suscripción y grupo de recursos, tal como se muestra en la siguiente imagen.

   ![Muestra de directiva](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicies.PNG)
       
3. Seleccione la directiva y haga clic en **Definiciones de directiva** para ver o agregar más definiciones.

   ![Muestra de definiciones de directivas](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicy-adddefinitions.PNG)

4. Seleccione **Subredes asociadas** para ver las subredes asociadas a la directiva. Para asociar la directiva a una subred, haga clic en "Navigate to virtual network in the same region" (Desplazarse a la red virtual en la misma región).

   ![Muestra de subredes asociadas](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-view-associatedsubnets.PNG)
 
## <a name="associate-a-policy-to-a-subnet"></a>Asociación de una directiva a una subred

>[!WARNING] 
> Asegúrese de que todos los recursos a los que se accede desde la subred, para el servicio seleccionado, se agregan a la directiva antes de asociarla. Una vez asociada la directiva, solo se permitirá el acceso a los recursos enumerados en la directiva para las regiones de punto de conexión del servicio. 

Antes de poder asociar una directiva a una subred, debe crear una red virtual y una subred y, a continuación, puede asociar la directiva a la subred:

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Red virtual**.
3. En **Crear red virtual**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de las opciones y, a continuación, seleccione **Crear**:
   - Nombre: myVirtualNetwork      
   - Espacio de direcciones: 10.0.0.0/16      
   - Suscripción: Seleccione su suscripción. La directiva debe estar en la misma suscripción que la red virtual     
   - Grupo de recursos: seleccione **Usar existente** y después seleccione *myResourceGroup*     
   - Ubicación: Centro occidental de EE.UU.     
   - Nombre de subred: privada     
   - Intervalo de direcciones: 10.0.0.0/24
     
4. En el cuadro **Search resources, services, and docs** (Buscar recursos, servicios y documentos) en la parte superior del portal, comience a escribir *myVirtualNetwork*. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela.
5. En **Configuración**, seleccione **Subredes** y, después, haga clic en **Privada**.
6. Como se muestra en la siguiente imagen, seleccione sucesivamente **Puntos de conexión de servicio**, **Microsoft.Storage**, **Directivas de punto de conexión de servicio**, **myEndpointPolicy** y, finalmente, **Guardar**:

   ![Asociación de directiva](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-associatepolicies.PNG)

>[!WARNING] 
>Desde esta subred se permitirá el acceso a los recursos de servicio de otras regiones, basándose en los grupos de seguridad de red. Para restringir el acceso solo a las regiones de punto de conexión, limite los grupos de seguridad de red a solo el tráfico de servicio en las regiones de punto de conexión. Para más información sobre cómo crear grupos de seguridad de red con etiquetas de servicio por región, consulte [Etiquetas de servicio de grupos de seguridad de red de Azure.](manage-network-security-group.md?toc=%2fcreate-a-security-rule%2f.json)

En el ejemplo siguiente, el grupo de seguridad de red está restringido a acceder solo a los recursos de Azure Storage en WestCentralUS y WestUS2, con una regla "Denegar todo" como regla de menor prioridad.

![Denegación de todos los grupos de seguridad de red](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-nsg-rules.PNG)


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha creado una directiva de punto de conexión de servicio y la ha asociado a una subred. Para saber más de las directivas de punto de conexión de servicio, consulte [Directivas de punto de conexión de servicio de redes virtuales (versión preliminar).](virtual-network-service-endpoint-policies-overview.md)

