---
title: Uso del portal de administración de Azure Stack | Microsoft Docs
description: Como operador de Azure Stack, debe aprender a usar el portal de administración.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: mabrigg
ms.openlocfilehash: 058e1f9abbf094fb49a21191f60195df53615451
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985301"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Uso del portal de administración de Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Hay dos portales en Azure Stack: el portal de administración y el portal de usuarios (también llamado en ocasiones portal de *inquilinos*). Como operador de Azure Stack, puede usar el portal de administración para la administración y las operaciones diarias de Azure Stack.

## <a name="access-the-administrator-portal"></a>Acceso al portal de administración

En un entorno de kit de desarrollo, primero deberá asegurarse de que puede [conectarse al host del kit de desarrollo](azure-stack-connect-azure-stack.md) mediante Conexión a Escritorio remoto o una red privada virtual (VPN).

Para acceder al portal de administración, vaya a la dirección URL del portal e inicie sesión con las credenciales de un operador de Azure Stack. En un sistema integrado, la dirección URL del portal varía según el nombre de la región y el nombre de dominio completo (FQDN) externo de la implementación de Azure Stack.

| Entorno | Dirección URL del portal de administración |   
| -- | -- | 
| Kit de desarrollo| https://adminportal.local.azurestack.external  |
| Sistemas integrados | https://adminportal.&lt;*región*&gt;.&lt;*FQDN*&gt; | 
| | |

 ![El portal de administración](media/azure-stack-manage-portals/admin-portal.png)

En el portal de administración, puede hacer cosas como las siguientes:

* Administrar la infraestructura (lo que incluye el mantenimiento, las actualizaciones y la capacidad del sistema, entre otros)
* Rellenado de Marketplace
* Crear suscripciones para los usuarios
* Crear planes y ofertas

El icono del **tutorial de inicio rápido** proporciona vínculos a documentación en línea para las tareas más comunes.

Aunque un operador puede crear recursos, como máquinas virtuales, redes virtuales y cuentas de almacenamiento en el portal de administración, debe [iniciar sesión en el portal de usuarios](user/azure-stack-use-portal.md) para crear y probar los recursos.

>[!NOTE]
>El vínculo **Crear una máquina virtual** del icono del tutorial de inicio rápido permite crear una máquina virtual en el portal de administración, pero su única función es validar Azure Stack después de la implementación inicial.

## <a name="understand-subscription-behavior"></a>Información acerca del comportamiento de una suscripción

En el portal de administración hay solo una suscripción que esté disponible para su uso. Esta suscripción es la *suscripción de proveedor predeterminada*. No se pueden agregar otras suscripciones y usarlas en el portal de administración.

Como operador de Azure Stack, puede agregar suscripciones para los usuarios (incluido usted mismo) desde el portal de administración. Los usuarios (incluido usted) pueden acceder a estas suscripciones y usarlas desde el portal de **usuarios**. Sin embargo, el portal de usuarios no proporciona acceso a ninguna de las funcionalidades administrativas u operativas del portal de administración.

Los portales de administración y de usuarios se apoyan en instancias independientes de Azure Resource Manager. Dada esta separación de Resource Manager, las suscripciones no sirven en todos los portales. Por ejemplo, si, como operador de Azure Stack, inicia sesión en el portal de usuarios, no puede acceder a la *suscripción de proveedor predeterminada*. Aunque no tiene acceso a las funciones administrativas, puede crear suscripciones para usted mismo a partir de ofertas públicas disponibles. Siempre que haya iniciado sesión en el portal de usuarios se le considera un usuario inquilino.

  >[!NOTE]
  >En el entorno del kit de desarrollo, si un usuario pertenece al mismo directorio de inquilino que el operador de Azure Stack, no se le impide iniciar sesión en el portal de administración. Sin embargo, no tiene acceso a ninguna de las funciones administrativas. Además, desde el portal de administración no puede agregar suscripciones ni acceder a las ofertas disponibles en el portal de usuarios.

## <a name="administrator-portal-tips"></a>Sugerencias del portal de administración

### <a name="customize-the-dashboard"></a>Personalización del panel

El panel contiene un conjunto de iconos predeterminados. Puede seleccionar **Edit dashboard** (Editar panel) para modificar el panel predeterminado o **New dashboard** (Nuevo panel) para agregar un panel personalizado. Es muy fácil agregar iconos a un panel. Por ejemplo, puede seleccionar **+ Create a resource** (+ Crear un recurso), hacer clic con el botón derecho en **Offers + Plans** (Ofertas y planes) y luego seleccionar **Pin to dashboard** (Anclar al panel).

En ocasiones, puede que vea un panel en blanco en el portal. Para recuperar el panel, haga clic en **Edit Dashboard** (Editar panel) y, a continuación, haga clic con el botón derecho y seleccione **Reset to default state** (Restaurar al estado predeterminado).

### <a name="quick-access-to-online-documentation"></a>Acceso rápido a la documentación en línea

Para acceder a la documentación del operador de Azure Stack, use el icono de Ayuda y soporte técnico (signo de interrogación) de la esquina superior derecha del portal de administración. Mueva el cursor hasta el icono y seleccione **Ayuda y soporte técnico**.

### <a name="quick-access-to-help-and-support"></a>Acceso rápido a ayuda y soporte técnico

Si selecciona el icono de ayuda y soporte técnico (signo de interrogación) en la esquina superior derecha del portal de administración y, a continuación, seleccione **Nueva solicitud de soporte técnico**, se producirá uno de los siguientes resultados:

- Si usa un sistema integrado, esta acción abre un sitio donde puede abrir directamente un vale de soporte técnico para los servicios de soporte al cliente de Microsoft (CSS). Consulte [dónde obtener soporte técnico](azure-stack-manage-basics.md#where-to-get-support) para saber cuándo debe acudir al servicio de soporte técnico de Microsoft o al soporte técnico del proveedor de hardware OEM (fabricante de equipo original).
- Si usa el kit de desarrollo, esta acción abre directamente el sitio de foros de Azure Stack. Estos foros se supervisan periódicamente. Dado que el kit de desarrollo es un entorno de evaluación, el servicio de asistencia al cliente (CSS) de Microsoft no ofrece soporte técnico oficial.

## <a name="next-steps"></a>Pasos siguientes

- [Administración de regiones en Azure Stack](azure-stack-region-management.md)
