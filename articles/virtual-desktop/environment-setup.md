---
title: 'Entorno de Windows Vista previa del escritorio Virtual: Azure'
description: Los elementos básicos de un entorno de Windows Vista previa del escritorio Virtual.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: 6aa6c7326759e480235df5fe9d4b0878cd11024d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142384"
---
# <a name="windows-virtual-desktop-preview-environment"></a>Entorno de Windows Vista previa del escritorio Virtual

Vista previa de Escritorio Virtual de Windows es un servicio que ofrece a los usuarios un acceso fácil y seguro a sus escritorios virtualizados y RemoteApps. En este tema le indicará un poco más acerca de la estructura general del entorno de Escritorio Virtual de Windows.

## <a name="tenants"></a>Inquilinos

El inquilino de Escritorio Virtual de Windows es la interfaz principal para administrar el entorno de Escritorio Virtual de Windows. Cada inquilino de Escritorio Virtual de Windows debe estar asociado con Azure Active Directory que contenga los usuarios que iniciarán sesión el entorno. Desde el inquilino de Escritorio Virtual de Windows, puede comenzar a crear los grupos host para ejecutar cargas de trabajo de usuarios.

## <a name="host-pools"></a>Grupos host

Un grupo host es una colección de máquinas virtuales que se registren para Escritorio Virtual de Windows como hosts de sesión cuando se ejecuta el agente de Escritorio Virtual de Windows. Todas las máquinas virtuales de host de sesión en un grupo host se deben originar en la misma imagen para una experiencia de usuario coherente.

Un grupo host puede ser uno de los dos tipos:

- Personal, donde cada host de sesión se asigna a usuarios individuales.
- Agrupados, donde los hosts de sesión pueden aceptar conexiones de cualquier usuario autorizado para un grupo de aplicaciones del grupo host.

Puede establecer propiedades adicionales en el grupo host para cambiar su comportamiento de equilibrio de carga, ¿cuántas sesiones puede tardar cada host de sesión, y qué puede hacer el usuario a los hosts de sesión en el grupo host mientras está conectado a sus sesiones de Escritorio Virtual de Windows. Controlar los recursos publicados a los usuarios a través de grupos de aplicaciones.

## <a name="app-groups"></a>Grupos de aplicaciones

Un grupo de aplicaciones es una agrupación lógica de las aplicaciones instaladas en los hosts de sesión en el grupo host. Un grupo de aplicaciones puede ser uno de los dos tipos:

- RemoteApp, donde los usuarios tener acceso a la RemoteApps seleccionar individualmente y publicar en el grupo de aplicaciones
- Escritorio, donde los usuarios tener acceso a todo el escritorio

De forma predeterminada, se crea automáticamente un grupo de aplicación de escritorio (denominado "Grupo de aplicaciones de escritorio") siempre que cree un grupo host. Puede quitar este grupo de aplicaciones en cualquier momento. Sin embargo, no se puede crear otro grupo de aplicación de escritorio en el grupo host, mientras que existe un grupo de aplicación de escritorio. Para publicar RemoteApps, debe crear un grupo de aplicaciones de RemoteApp. Puede crear varios grupos de aplicaciones de RemoteApp para dar cabida a escenarios de trabajo diferente. Diferentes grupos de aplicaciones de RemoteApp pueden contener también superpuestos RemoteApps.

Para publicar recursos a los usuarios, debe asignarlos a grupos de aplicaciones. Al asignar a usuarios a grupos de aplicaciones, tenga en cuenta lo siguiente:

- No se puede asignar un usuario a un grupo de aplicación de escritorio y un grupo de aplicaciones de RemoteApp en el mismo grupo host.
- Un usuario puede asignarse a varios grupos de aplicación dentro del mismo grupo host y su fuente será una acumulación de ambos grupos de aplicaciones.

## <a name="tenant-groups"></a>Grupos de inquilinos

En el escritorio Virtual de Windows, el inquilino de Escritorio Virtual de Windows es donde ocurre la mayor parte de la instalación y configuración. El inquilino de Escritorio Virtual de Windows contiene los grupos host, grupos de aplicaciones y las asignaciones de usuario del grupo de aplicación. Sin embargo, puede haber ciertas situaciones donde es necesario para administrar a varios inquilinos de Escritorio Virtual de Windows al mismo tiempo, especialmente si es un proveedor de servicios en la nube (CSP) o un socio de hospedaje. En estas situaciones, puede usar un grupo personalizado de inquilino de Escritorio Virtual de Windows para colocar cada uno de los inquilinos de Escritorio Virtual de Windows de los clientes y administrar centralmente el acceso. Sin embargo, si solo administra a un único inquilino de Escritorio Virtual de Windows, no se aplica el concepto de grupo del inquilino y puede seguir controlar y administrar al inquilino que existe en el grupo del inquilino predeterminado.

## <a name="end-users"></a>Usuarios finales

Después de haber asignado a los usuarios a sus grupos de aplicación, puede conectar a una implementación de Escritorio Virtual de Windows con cualquiera de los clientes de Escritorio Virtual de Windows.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el acceso delegado y cómo asignar roles a los usuarios en [delegar el acceso en Windows Vista previa del escritorio Virtual](delegated-access-virtual-desktop.md).

Para obtener información sobre cómo configurar el inquilino de Escritorio Virtual de Windows, consulte [crear un inquilino en Windows Vista previa del escritorio Virtual](tenant-setup-azure-active-directory.md).

Para obtener información sobre cómo conectarse a escritorios virtuales de Windows, consulte uno de los siguientes artículos:

- [Conectarse desde Windows 10 o Windows 7](connect-windows-7-and-10.md)
- [Conectarse desde un explorador web](connect-web.md)
