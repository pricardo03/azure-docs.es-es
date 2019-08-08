---
title: No se puede agregar un dominio personalizado mediante un certificado de Key Vault en Azure API Management | Microsoft Docs
description: Obtenga información sobre cómo solucionar el problema en el que no se puede agregar un dominio personalizado en Azure API Management mediante un certificado de Key Vault.
services: api-management
documentationcenter: ''
author: genlin
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: tehnoonr
ms.openlocfilehash: 7aaf29ddf52fb07524aa972f12ca18c235a40928
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68349267"
---
# <a name="failed-to-update-api-management-service-hostnames"></a>No se pudieron actualizar los nombres de host del servicio API Management

En este artículo se describe el error "No se pudieron actualizar los nombres de host del servicio API Management" que puede experimentar al agregar un dominio personalizado para el servicio API Management de Azure. Este artículo proporciona pasos de solución de problemas para ayudarle a resolver el problema.

## <a name="symptoms"></a>Síntomas

Al intentar agregar un dominio personalizado para el servicio API Management mediante un certificado de Azure Key Vault, se recibe el mensaje de error siguiente:

- No se pudieron actualizar los nombres de host del servicio API Management. Error en la solicitud al recurso "https://vaultname.vault.azure.net/secrets/secretname/?api-version=7.0" con código de estado: Prohibido para RequestId:. Mensaje de excepción: La operación devolvió un código de estado no válido "Forbidden".

## <a name="cause"></a>Causa

El servicio API Management no tiene permiso para acceder al almacén de claves que intenta usar para el dominio personalizado.

## <a name="solution"></a>Solución

Para resolver el problema, siga estos pasos:

1. Vaya a [Azure Portal](Https://portal.azure.com), seleccione la instancia de API Management y, a continuación, seleccione **Identidades administradas**. Asegúrese de que la opción **Registrar con Azure Active Directory** está establecida en **Sí**. 
    ![Registro con Azure Active Directory](./media/api-management-troubleshoot-cannot-add-custom-domain/register-with-aad.png)
1. En Azure Portal, abra el servicio **Key Vault** y seleccione el almacén de claves que intenta usar para el dominio personalizado.
1. Seleccione **Directivas de acceso** y compruebe si hay una entidad de servicio que coincida con el nombre de la instancia del servicio API Management. Si es así, seleccione la entidad de servicio y asegúrese de que el permiso **Get** aparece en **Permisos de secretos**.  
    ![Adición de directiva de acceso para la entidad de servicio](./media/api-management-troubleshoot-cannot-add-custom-domain/access-policy.png)
1. Si el servicio API Management no está en la lista, seleccione **Agregar directiva de acceso** y, a continuación, cree la siguiente directiva de acceso:
    - **Configurar a partir de una plantilla**: None
    - **Seleccionar la entidad de seguridad**: Busque el nombre del servicio API Management y, a continuación, selecciónelo en la lista.
    - **Permisos de claves**: None
    - **Permisos de secretos**: Obtener
    - **Permisos de certificados**: None
1. Seleccione **Aceptar** para crear la directiva de acceso.
1. Seleccione **Guardar** para guardar los cambios.

Compruebe si se ha resuelto el problema. Para ello, intente crear el dominio personalizado en el servicio API Management mediante el certificado de Key Vault.

## <a name="next-steps"></a>Pasos siguientes
Más información sobre el servicio API Management:

- Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
* Para conocer otras formas de proteger el servicio back-end, consulte [Autenticación de certificado mutua](api-management-howto-mutual-certificates.md).

* [Creación de una instancia del servicio de API Management](get-started-create-service-instance.md).
* [Administración de su primera API en Administración de API de Azure](import-and-publish.md).