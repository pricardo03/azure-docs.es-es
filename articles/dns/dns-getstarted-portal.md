---
title: 'Guía de inicio rápido: Creación de una zona y un registro DNS mediante Azure Portal'
description: Use esta guía de inicio rápido para aprender a crear una zona y un registro DNS en Azure DNS. Esta es una guía paso a paso para crear y administrar su primera zona y su primer registro DNS con Azure Portal.
services: dns
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: quickstart
ms.date: 6/13/2018
ms.author: victorh
ms.openlocfilehash: 0acb5bf18c078d8b7eb6a5c14a61fcef622f9f2d
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831134"
---
# <a name="quickstart-configure-azure-dns-for-name-resolution-using-the-azure-portal"></a>Guía de inicio rápido: Configuración de Azure DNS para la resolución de nombres mediante Azure Portal

 Puede configurar Azure DNS para resolver nombres de host en el dominio público. Por ejemplo, si ha adquirido el nombre de dominio contoso.com de un registrador de nombres de dominio, puede configurar Azure DNS para hospedar el dominio contoso.com y resolver www.contoso.com en la dirección IP del servidor web o la aplicación web.

En esta guía de inicio rápido, creará un dominio de prueba y, luego, un registro de dirección llamado "www" para resolverlo en la dirección IP 10.10.10.10.

Es importante saber que todos los nombres y direcciones IP usados en esta guía son solo ejemplos y no están diseñados para representar un escenario del mundo real. Sin embargo, si procede, también se describen escenarios del mundo real.

<!---
You can also perform these steps using [Azure PowerShell](dns-getstarted-powershell.md) or the cross-platform [Azure CLI](dns-getstarted-cli.md).
--->

Una zona DNS se usa para contener las entradas DNS de un dominio concreto. Para iniciar el hospedaje de su dominio en DNS de Azure, debe crear una zona DNS para ese nombre de dominio. Cada entrada DNS (o registro) del dominio se crea luego en esta zona DNS. Se muestra cómo hacerlo en los pasos siguientes.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-dns-zone"></a>Creación de una zona DNS

1. Inicie sesión en el Portal de Azure.
2. En la parte superior izquierda, haga clic en **+ Crear un recurso**, después en **Redes** y, finalmente, en **Zona DNS** para abrir la página **Crear zona DNS**.

    ![Zona DNS](./media/dns-getstarted-portal/openzone650.png)

4. En la página **Crear zona DNS**, escriba los valores siguientes y haga clic en **Crear**:


   | **Configuración** | **Valor** | **Detalles** |
   |---|---|---|
   |**Nombre**|contoso.xyz|En esta guía de inicio rápido, el nombre de la zona DNS de este ejemplo puede ser cualquier valor que quiera, siempre y cuando no esté ya configurado en los servidores de Azure DNS. Un valor del mundo real sería un dominio que haya comprado a un registrador de nombres de dominio.|
   |**Suscripción**|[Su suscripción]|Seleccione la suscripción en la que se creará la zona DNS.|
   |**Grupos de recursos**|**Crear nuevo:** dns-test|Cree un grupo de recursos. El nombre del grupo de recursos debe ser único dentro de la suscripción seleccionada. |
   |**Ubicación**|Este de EE. UU||

La creación de la zona puede tardar unos minutos.

## <a name="create-a-dns-record"></a>Creación de un registro de DNS

Ahora cree un nuevo registro de dirección (registro "D"). Los registros "D" se usan para resolver un nombre de host en una dirección IP IPv4.

1. En el panel **Favoritos** de Azure Portal, haga clic en **Todos los recursos**. Haga clic en la zona DNS **contoso.xyz** en la página Todos los recursos. Si la suscripción que seleccionó ya tiene varios recursos en ella, puede escribir **contoso.xyz** en el cuadro **Filtrar por nombre…** para acceder fácilmente a la zona DNS.

1. En la parte superior de la página **Zona DNS**, seleccione **+ Conjunto de registros** para abrir la página **Agregar conjunto de registros**.

1. En la página **Agregar conjunto de registros**, escriba los valores siguientes y haga clic en **Aceptar**. En este ejemplo, va a crear un registro "D".

   |**Configuración** | **Valor** | **Detalles** |
   |---|---|---|
   |**Nombre**|www|Nombre del registro. Éste es el nombre que desea usar para el host que desea resolver en una dirección IP.|
   |**Tipo**|Una | Tipo de registro DNS que se va a crear. Los registros "D" son las más comunes, pero hay otros tipos de registros para los servidores de correo (MX), las direcciones IP v6 (AAAA), etc. |
   |**TTL**|1|Período de vida de la solicitud DNS. Especifica cuánto tiempo pueden los clientes y servidores DNS almacenar en caché una respuesta.|
   |**Unidad de TTL**|hours|Medición de tiempo para el valor de TTL.|
   |**Dirección IP**|10.10.10.10| Este valor es la dirección IP en la que se resuelve el registro "D". Se trata de un valor de prueba para esta guía de inicio rápido. Para obtener un ejemplo del mundo real, tendría que escribir la dirección IP pública del servidor web.|


Como en esta guía de inicio rápido no adquiere realmente un nombre de dominio real, no es necesario configurar Azure DNS como servidor DNS con el registrador de nombres de dominio. Pero en un escenario real normalmente querría que todos los usuarios de Internet pudieran resolver su nombre de host para conectarse al servidor o aplicación web. Para más información sobre ese escenario del mundo real, consulte [Delegación de un dominio en Azure DNS](dns-delegate-domain-azure-dns.md).


## <a name="test-the-name-resolution"></a>Probar la resolución de nombres

Ahora que tiene una zona de prueba, con un registro "D" de prueba, puede probar la resolución de nombres con una herramienta llamada *nslookup*. 

1. En primer lugar, debe tener en cuenta los servidores de nombres de Azure DNS que va a usar con nslookup. 

   Los servidores de nombres de su zona se enumerarán en la página **Información general** de la zona DNS. Copie el nombre de uno de los servidores de nombres:

   ![zona](./media/dns-getstarted-portal/viewzonens500.png)

2. Abra un símbolo del sistema y ejecute el comando siguiente:

   ```
   nslookup <host name> <name server>
   
   For example:

   nslookup www.contoso.xyz ns1-08.azure-dns.com
   ```

Debe ver algo parecido a lo que aparece en la siguiente captura de pantalla:

![nslookup](media/dns-getstarted-portal/nslookup.PNG)

Esto confirma que la resolución de nombres funciona correctamente. www.contoso.xyz se resuelve en 10.10.10.10, que es como lo ha configurado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos **dns-test** para eliminar los recursos que ha creado en esta guía de inicio rápido. Para ello, haga clic en el grupo de recursos **dns-test** y luego haga clic en **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)