---
title: Control de la unión a Azure AD híbrido de los dispositivos | Microsoft Docs
description: Aprenda a controlar la unión a Azure AD híbrido de los dispositivos en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 52f3859edcc8a524c56f91ad2a5346a69ea534ed
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103970"
---
# <a name="control-the-hybrid-azure-ad-join-of-your-devices"></a>Control de la unión a Azure AD híbrido de los dispositivos

La unión a Azure Active Directory (Azure AD) híbrido es un proceso para registrar automáticamente los dispositivos unidos a un dominio local con Azure AD. Hay casos en los que es mejor que no todos los dispositivos se registren automáticamente. Esto es así, por ejemplo, durante el lanzamiento inicial para comprobar que todo funciona según lo previsto.

En este artículo se proporciona orientación sobre cómo se puede controlar la unión a Azure AD híbrido de los dispositivos. 


## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que está familiarizado con:

-  [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)
 
-  [Planeamiento de la implementación de la unión a Azure Active Directory híbrido](hybrid-azuread-join-plan.md)

-  [Configuración de la unión a Azure Active Directory híbrido en dominios administrados](hybrid-azuread-join-managed-domains.md) o [Configuración de la unión a Azure Active Directory híbrido en dominios federados](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Control de los dispositivos actuales de Windows

Para dispositivos que ejecutan el sistema operativo de escritorio de Windows, la versión admitida es la Actualización de aniversario de Windows 10 (versión 1607), o una versión posterior. Como procedimiento recomendado, actualice a la versión más reciente de Windows 10.

Todos los dispositivos actuales de Windows se registran automáticamente en Azure AD durante el inicio del dispositivo o el inicio de sesión del usuario. Puede controlar este comportamiento con un objeto de directiva de grupo (GPO) o mediante System Center Configuration Manager.

Para controlar los dispositivos actuales de Windows, es preciso: 


1.  **Para todos los dispositivos**: deshabilitar el registro automático de dispositivos.
2.  **Para los dispositivos seleccionados**: habilitar el registro automático de dispositivos.

Después de comprobar que todo funciona según lo previsto, está preparado para volver a habilitar el registro automático de dispositivos para todos los dispositivos.



### <a name="group-policy-object"></a>Objeto de directiva de grupo 

Puede controlar el comportamiento de registro de los dispositivos mediante la implementación del siguiente GPO: **Registro de equipos unidos a un dominio como dispositivos**.

Para establecer el GPO:

1.  Abra el **Administrador del servidor** y vaya a **Herramientas** > **Administración de directivas de grupo**.

2.  Vaya al nodo correspondiente al dominio en el que quiera habilitar o deshabilitar el registro automático.

3.  Haga clic con el botón derecho en **Objetos de directiva de grupo** y seleccione **Nuevo**.

4.  Escriba un nombre (por ejemplo, **Unión a Azure AD híbrido**) para el objeto de directiva de grupo. 

5.  Seleccione **Aceptar**.

6.  Haga clic con el botón derecho en el nuevo GPO y seleccione **Editar**.

7.  Vaya a **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Registro de dispositivos**. 

8.  Haga clic con el botón derecho en **Registrar los equipos asociados a un dominio como dispositivos** y seleccione **Editar**.

    > [!NOTE] 
    > Esta plantilla de directiva de grupo ha cambiado de nombre desde versiones anteriores de la consola de Administración de directivas de grupo. Si usa una versión anterior de la consola, vaya a **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Workplace Join** > **Automatically workplace join client computers** (Unión automática al área de trabajo de equipos cliente). 

9.  Seleccione una de las siguientes opciones y, luego, **Aplicar**:

    - **Deshabilitado**: para evitar el registro automático de dispositivos.
    - **Habilitado**: para habilitar el registro automático de dispositivos.

10. Seleccione **Aceptar**.

Tiene que vincular el GPO a una ubicación de su elección. Por ejemplo, para establecer esta directiva para todos los dispositivos actuales unidos a dominio de su organización, vincule el GPO al dominio. Para realizar una implementación controlada, establezca esta directiva en dispositivos actuales de Windows unidos a dominio que pertenecen a una unidad organizativa o a un grupo de seguridad.

### <a name="configuration-manager-controlled-deployment"></a>Implementación controlada del Administrador de configuración 

Puede controlar el comportamiento de registro de dispositivos de los dispositivos actuales mediante la configuración de los valores de cliente siguientes: **Registrar automáticamente los nuevos dispositivos de Windows 10 unidos a un dominio con Azure Active Directory**.

Para configurar el valor de cliente:

1.  Abra **Administrador de configuración** y vaya a **Cloud Services**.

2.  En **Configuración del dispositivo**, seleccione una de las siguientes opciones para **Registrar automáticamente los nuevos dispositivos de Windows 10 unidos a un dominio con Azure Active Directory**:

    - **No**: para evitar el registro automático de dispositivos.
    - **Sí**: para habilitar el registro automático de dispositivos.


3.  Seleccione **Aceptar**.
    

Tiene que vincular este valor de cliente a una ubicación de su elección. Por ejemplo, para configurar este valor de cliente para todos los dispositivos actuales de Windows en su organización, vincule el valor de cliente al dominio. Para realizar una implementación controlada, puede configurar el valor de cliente en dispositivos actuales de Windows unidos a dominio que pertenecen a una unidad organizativa o a un grupo de seguridad.

> [!Important]
> Aunque la configuración anterior se encarga de los dispositivos de Windows 10 unidos a un dominio ya existentes, los dispositivos que se han unido a un dominio recientemente podrían intentar realizar la unión a Azure AD híbrido debido al posible retraso en la aplicación de la configuración de directiva de grupo o Configuration Manager en los dispositivos. 
>
> Para evitar esta situación, se recomienda crear una nueva imagen de Sysprep (usada como ejemplo para un método de aprovisionamiento). Hágalo desde un dispositivo que nunca se haya unido anteriormente a Azure AD híbrido y que ya tenga aplicada la configuración de directiva de grupo o la configuración de cliente de Configuration Manager. También debe utilizar la nueva imagen para aprovisionar los equipos nuevos que se unen al dominio de la organización. 

## <a name="control-windows-down-level-devices"></a>Control de dispositivos de Windows de nivel inferior

Para registrar dispositivos Windows de nivel inferior, tiene que descargar e instalar el paquete de Windows Installer (.msi) desde el Centro de descarga de la página [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/download/details.aspx?id=53554) (Microsoft Workplace Join para equipos sin Windows 10).

El paquete se puede implementar mediante un sistema de distribución de software como [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). El paquete admite las opciones de instalación silenciosa estándar mediante el parámetro quiet. La rama actual de Configuration Manager ofrece ventajas adicionales sobre las versiones anteriores, como la posibilidad de realizar el seguimiento de los registros completados.

El instalador crea una tarea programada en el sistema que se ejecuta en el contexto del usuario. La tarea se desencadena cuando el usuario inicia sesión en Windows. La tarea une de forma silenciosa el dispositivo con Azure AD con las credenciales de usuario después de realizar la autenticación con Azure AD.

Para controlar el registro de dispositivos, debe implementar el paquete de Windows Installer solo en un grupo seleccionado de dispositivos de Windows de nivel inferior. Si ha comprobado que todo funciona según lo previsto, está preparado para el lanzamiento del paquete a todos los dispositivos de nivel inferior.


## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)



