---
title: Restringir el contenido de la red CDN de Azure por país o región | Microsoft Docs
description: Obtenga información sobre cómo restringir el acceso por país o región para el contenido de la red CDN de Azure mediante la característica filtrado geográfico.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: magattus
ms.openlocfilehash: 083d8f66a73471548c812e27325e1ec69ad5c45c
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869584"
---
# <a name="restrict-azure-cdn-content-by-countryregion"></a>Restringir el contenido de la red CDN de Azure por país o región

## <a name="overview"></a>Información general
Cuando un usuario solicita su contenido, de forma predeterminada, el contenido se proporciona sin tener en cuenta la ubicación desde la que el usuario realiza esta solicitud. Sin embargo, en algunos casos, es posible que desee restringir el acceso a contenidos por país o región. Con el *filtrado geográfico* característica, puede crear reglas en rutas de acceso específicas en el extremo CDN para permitir o bloquear el contenido en países o regiones seleccionadas.

> [!IMPORTANT]
> Los perfiles **Azure CDN Estándar de Microsoft** no admiten el filtrado geográfico basado en rutas.
> 

## <a name="standard-profiles"></a>Perfiles estándar
Los procedimientos de esta sección se aplican solo a perfiles de **Azure CDN Estándar de Akamai** y **Azure CDN Estándar de Verizon**. 

Con los perfiles de **Azure CDN Premium de Verizon**, debe usar el **Portal de administración** para activar el filtrado geográfico. Para más información, consulte [Perfiles de Azure CDN Premium de Verizon](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Definición de la ruta de acceso a directorio
Para acceder a la característica de filtrado geográfico, seleccione el punto de conexión de CDN en el portal y, luego, seleccione **Filtrado geográfico** en Configuración en el menú izquierdo. 

![Estándar de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-standard.png)

En el cuadro **Ruta de acceso**, especifique la ruta de acceso relativa a la ubicación a la que se les permitirá o denegará el acceso a los usuarios. 

Puede aplicar el filtrado geográfico a todos los archivos mediante una barra diagonal (/), o seleccionar carpetas concretas mediante la especificación de rutas de acceso a directorio (por ejemplo, */pictures/*). También puede aplicar el filtrado geográfico a un único archivo (por ejemplo, */pictures/city.png*). Se permiten varias reglas; después de especificar una regla, aparece una fila en blanco para que escriba la siguiente regla.

Por ejemplo, todos los filtros siguientes de ruta de acceso a directorio son válidos:   
*/*                                 
*/Photos/*     
*/Photos/Strasbourg/*     
*/Photos/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Definición del tipo de acción

En la lista **Acción**, seleccione **Permitir** o **Bloquear**: 

- **Permitir**: Solo los usuarios de los países o regiones especificados pueden acceder a los recursos solicitados desde la ruta recursiva.

- **Bloque**: Los usuarios de los países o regiones especificados se deniegan el acceso a los recursos solicitados desde la ruta recursiva. Si no hay otras opciones de filtrado de país o región se han configurado para esa ubicación, a continuación, todos los demás usuarios se permitirá acceso.

Por ejemplo, una regla de filtrado geográfico para bloquear la ruta de acceso */Photos/Strasbourg/* filtra los siguientes archivos:     
*http:\//\<punto de conexión >.azureedge.net/Photos/Strasbourg/1000.jpg*
*http:\//\<punto de conexión >.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countriesregions"></a>Definir los países o regiones
Desde el **códigos de país** lista, seleccione los países o regiones que desee bloquear o permitir para la ruta de acceso. 

Una vez haya acabado de seleccionar los países o regiones, seleccione **guardar** para activar la nueva regla de filtrado geográfico. 

![Reglas de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Limpieza de recursos
Para eliminar una regla, selecciónela de la lista en la página **Filtrado geográfico** y luego elija **Eliminar**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Perfiles de Azure CDN Premium de Verizon
En los perfiles de **Azure CDN Premium de Verizon**, la interfaz de usuario para crear una regla de filtrado geográfico es diferente:

1. En el menú superior del perfil de Azure CDN, seleccione **Administrar**.

2. En el portal de Verizon, seleccione **HTTP Large** (HTTP grande) y, luego, **Country Filtering** (Filtrado de países).

    ![Estándar de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Seleccione **Add Country Filter** (Agregar filtro de país).

    Aparece la página **Step One:** (Paso 1:).

4. Escriba la ruta de acceso al directorio, seleccione **Block** (Bloquear) o **Add** (Agregar) y, luego, seleccione **Next** (Siguiente).

    Aparece la página **Step Two:** (Paso 2:). 

5. Seleccione uno o más países o regiones en la lista y luego seleccione **finalizar** para activar la regla. 
    
    La nueva regla aparece en la tabla en la página **Country Filtering** (Filtrado de países).

    ![Reglas de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Limpieza de recursos
En la tabla de reglas de filtrado de país o región, seleccione el icono de eliminación junto a una regla que lo elimine o el icono de edición para modificarlo.

## <a name="considerations"></a>Consideraciones
* Los cambios en la configuración de filtrado geográfico no surten efecto inmediatamente:
   * En los perfiles de **Azure CDN Estándar de Microsoft**, la propagación se completa normalmente en 10 minutos. 
   * En los perfiles de **Azure CDN estándar**, la propagación normalmente se completa en un minuto. 
   * En los perfiles **Azure CDN Estándar de Verizon** y **Azure CDN Premium de Verizon**, la propagación se completa normalmente en 10 minutos. 
 
* Esta característica no admite caracteres comodín (por ejemplo, ‘*’).

* La configuración de filtrado geográfico asociada a la ruta de acceso relativa se aplica de forma recursiva a esa ruta.

* Se puede aplicar solo una regla a la misma ruta de acceso relativa. Es decir, no se puede crear varios filtros de país o región que apuntan a la misma ruta de acceso relativa. Sin embargo, dado que los filtros de país o región son recursivos, una carpeta puede tener varios filtros de país o región. En otras palabras, se puede asignar a una subcarpeta de una carpeta configurada previamente un filtro de país o región diferente.

* La característica filtrado geográfico usa códigos de país para definir los países o regiones desde el que una solicitud permitida o bloqueada para un directorio protegido. Aunque los perfiles de Akamai y Verizon admiten la mayoría de los mismos códigos de país, hay algunas diferencias. Para más información, consulte [Azure CDN Country Codes](/previous-versions/azure/mt761717(v=azure.100)) (Códigos de país de Azure CDN). 

