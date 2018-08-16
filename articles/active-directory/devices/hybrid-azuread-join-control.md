---
title: Configuración de dispositivos híbridos unidos a Azure Active Directory | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 9ffc84009adfca60e9ae6b188b65b15e874e7d9c
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622177"
---
# <a name="how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Control de la unión de dispositivos a Azure AD híbrido

La unión a Azure AD híbrido es un proceso para registrar automáticamente los dispositivos unidos al dominio en el entorno local con Azure AD. Hay casos en los que es mejor que no todos los dispositivos se registren automáticamente. Esto es así, por ejemplo, durante el lanzamiento inicial para comprobar que todo funciona según lo previsto.

Este artículo proporciona orientación sobre cómo se puede controlar la unión a Azure AD híbrido de los dispositivos. 


## <a name="prerequisites"></a>Requisitos previos

En este artículo se asume que está familiarizado con:

-  [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)
 
-  [Planeación de la implementación de dispositivos híbridos unidos a Azure Active Directory](hybrid-azuread-join-plan.md)

-  Configuración de la unión de Azure Active Directory híbrido para [dominios administrados](hybrid-azuread-join-managed-domains.md) o [dominios federados](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Control de los dispositivos actuales de Windows

Para dispositivos que ejecutan el sistema operativo de escritorio de Windows, la versión admitida es la Actualización de aniversario de Windows 10 (versión 1607), o una versión posterior. Como procedimiento recomendado, actualice a la versión más reciente de Windows 10.

Todos los dispositivos actuales de Windows se registran automáticamente con Azure AD en el inicio del dispositivo o en el inicio de sesión del usuario. Puede controlar este comportamiento con un objeto de directiva de grupo (GPO) o mediante System Center Configuration Manager.

Para controlar los dispositivos actuales de Windows, es preciso: 

1.  **Para todos los dispositivos**: deshabilitar el registro automático de dispositivos.
2.  **Para dispositivos seleccionados**: deshabilitar el registro automático de dispositivos.

Si ha comprobado que todo funciona según lo previsto, está preparado para volver a habilitar el registro automático de dispositivos para todos los dispositivos.



## <a name="group-policy-object"></a>Objeto de directiva de grupo 

Puede controlar el comportamiento del registro de dispositivos de los dispositivos mediante la implementación del siguiente GPO: **Register domain-joined computers as devices** (Registrar equipos unidos al dominio como dispositivos)

**Para establecer el GPO**:

1.  Abra el **Administrador del servidor** y vaya a **Herramientas\>Administración de directivas de grupo**.

2.  Vaya al nodo del dominio correspondiente al dominio en el que desee habilitar o deshabilitar el registro automático.

3.  Haga clic con el botón derecho en **Objetos de directiva de grupo** y seleccione **Nuevo**.

4.  Escriba un nombre (por ejemplo, *Unión a Azure AD híbrido*) para el objeto de directiva de grupo. 

5.  Haga clic en **OK**.

6.  Haga clic con el botón derecho en el nuevo GPO y seleccione **Editar**.

7.  Vaya a **Configuración del equipo \> Directivas \> Plantillas administrativas \> Componentes de Windows \> Registro de dispositivos**. 

8.  Haga clic con el botón derecho en **Registrar los equipos asociados a un dominio como dispositivos** y seleccione **Editar**.

    > [!NOTE] 
    > Esta plantilla de directiva de grupo ha cambiado de nombre desde versiones anteriores de la consola de Administración de directivas de grupo. Si usa una versión anterior de la consola, vaya a **Configuración del equipo \> Directivas \> Plantillas administrativas \> Componentes de Windows \> Unión al área de trabajo \> Automatically workplace join client computers** (Unión automática al área de trabajo de equipos cliente). 

9.  Seleccione una de las siguientes opciones y, a continuación, haga clic en **Aplicar**:

    - **Deshabilitado**: para evitar el registro automático de dispositivos
    - **Habilitado**: para habilitar el registro automático de dispositivos

10. Haga clic en **OK**.

Tiene que vincular el GPO a una ubicación de su elección. Por ejemplo, para establecer esta directiva para todos los dispositivos actuales unidos a dominio de su organización, vincule el GPO al dominio. Para realizar una implementación controlada, establezca esta directiva en dispositivos actuales de Windows unidos a dominio que pertenecen a una unidad organizativa o a un grupo de seguridad.

### <a name="configuration-manager-controlled-deployment"></a>Implementación controlada del Administrador de configuración 

Puede controlar el comportamiento del registro de dispositivos de los dispositivos actuales mediante la configuración del siguiente valor de cliente: **Registrar automáticamente los nuevos dispositivos de Windows 10 unidos a un dominio con Azure Active Directory**


**Para establecer la configuración de cliente**:

1.  Abra **Administrador de configuración** y vaya a **Cloud Services**.

2.  En **Configuración del dispositivo**, seleccione una de las siguientes opciones para **Registrar automáticamente los nuevos dispositivos de Windows 10 unidos a un dominio con Azure Active Directory**:

    - **No**: para evitar el registro automático de dispositivos
    - **Sí**: para habilitar el registro automático de dispositivos


3.  Haga clic en **OK**.
    

Tiene que vincular este valor de cliente a una ubicación de su elección. Por ejemplo, para configurar este valor de cliente para todos los dispositivos actuales de Windows en su organización, vincule el valor de cliente al dominio. Para realizar una implementación controlada, puede configurar el valor de cliente en dispositivos actuales de Windows unidos a dominio que pertenecen a una unidad organizativa o a un grupo de seguridad.

> [!Important]
> Mientras que la configuración anterior se ocupa de los dispositivos de Windows 10 que se han unido al dominio existente, existe la posibilidad de que los nuevos dispositivos unidos a un dominio intenten completar la unión híbrida a Azure AD debido al posible retraso en la aplicación real de la directiva de grupo o la configuración del administrador de configuración en el dispositivo de Windows 10 unidos al nuevo dominio. Para evitar esto, se recomienda que cree una nueva imagen de sysprep (utilizada como ejemplo para un método de aprovisionamiento) a partir de un dispositivo que nunca se haya unido anteriormente a Azure AD híbrido y que ya tenga aplicada la configuración de directiva del grupo anterior o la configuración de cliente del administrador de configuración. También debe utilizar la nueva imagen para aprovisionar los equipos nuevos que se unen al dominio de la organización. 

## <a name="control-windows-down-level-devices"></a>Control de dispositivos de Windows de nivel inferior

Para registrar dispositivos de Windows de nivel inferior, tiene que descargar e instalar el paquete de Windows Installer (.msi) desde el Centro de descarga de la página [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) (Microsoft Workplace Join para equipos sin Windows 10).

El paquete se puede implementar mediante un sistema de distribución de software como System Center Configuration Manager. El paquete admite las opciones de instalación silenciosa estándar mediante el parámetro quiet. La rama actual de [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) ofrece ventajas adicionales con respecto a las versiones anteriores, como la capacidad de realizar el seguimiento de los registros completados.

El instalador crea una tarea programada en el sistema que se ejecuta en el contexto del usuario. La tarea se desencadena cuando el usuario inicia sesión en Windows. La tarea une de forma silenciosa el dispositivo con Azure AD con las credenciales de usuario después de realizar la autenticación con Azure AD.


Para controlar el registro de dispositivos, debe implementar el paquete de Windows Installer solo en un grupo seleccionado de dispositivos de Windows de nivel inferior. Si ha comprobado que todo funciona según lo previsto, está preparado para el lanzamiento del paquete a todos los dispositivos de nivel inferior.


## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)



