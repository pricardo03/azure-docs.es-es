---
title: 'Tutorial: Creación de un registro de alias de Azure DNS para hacer referencia a un registro de recursos en la zona.'
description: Este tutorial muestra cómo configurar un registro de alias de Azure DNS para hacer referencia a un registro de recursos dentro de la zona.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 09c1768602fede51d7ff2b23f48278a1d0b0cd2a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990848"
---
# <a name="tutorial-create-an-alias-record-to-refer-to-a-zone-resource-record"></a>Tutorial: Creación de un registro de alias para hacer referencia a un registro de recursos de zona

Los registros de alias pueden hacer referencia a otros conjuntos de registros del mismo tipo. Por ejemplo, puede hacer que un conjunto de registros CNAME de DNS sea un alias de otro conjunto de registros CNAME del mismo tipo. Esto es útil si desea que algunos conjuntos de registros sean alias y otros no por cuestiones de comportamiento.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un registro de alias para un registro de recursos en la zona
> * Probar el registro de alias


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Debe tener un nombre de dominio disponible que pueda hospedar en Azure DNS para realizar las pruebas. Debe tener control total de este dominio, incluida la capacidad de establecer los registros de nombres de servidor (NS) para el dominio.

Para que obtener instrucciones para hospedar el dominio en Azure DNS, consulte [Tutorial: Hospedaje del dominio en Azure DNS](dns-delegate-domain-azure-dns.md).


## <a name="create-an-alias-record"></a>Creación de un registro de alias

Cree un registro de alias que apunta a un registro de recursos en la zona.

### <a name="create-the-target-resource-record"></a>Creación del registro de recursos de destino
1. Haga clic en la zona de Azure DNS para abrir la zona.
2. Haga clic en **Conjunto de registros**.
3. En el cuadro de texto **Nombre**, escriba **server**.
4. En **Tipo**, seleccione **A**.
5. En el cuadro de texto **Dirección IP**, escriba **10.10.10.10**.
6. Haga clic en **OK**.

### <a name="create-the-alias-record"></a>Creación del registro de alias
1. Haga clic en la zona de Azure DNS para abrir la zona.
2. Haga clic en **Conjunto de registros**.
3. En el cuadro de texto **Nombre**, escriba **test**.
4. En **Tipo**, seleccione **A**.
5. Haga clic en **Sí** en la casilla de verificación **Alias del conjunto de registros** y seleccione la opción **Conjunto de registros de la zona**.
6. En **Conjunto de registros de la zona**, seleccione el registro **server**.
7. Haga clic en **OK**.

## <a name="test-the-alias-record"></a>Prueba del registro de alias

1. Inicie su herramienta nslookup favorita. Una opción es ir a [ https://network-tools.com/nslook ](https://network-tools.com/nslook).
2. Establezca el tipo de consulta para los registros A y busque **test.\<su nombre de dominio\>**. Debería obtener **10.10.10.10** para la respuesta.
3. En Azure Portal, cambie el registro A **server** a **10.11.11.11**.
4. Espere unos minutos y, después, utilice nslookup para el registro **test**.
5. Debería obtener **10.11.11.11** para la respuesta.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no es necesario, puede eliminar los registros de recursos **server** y **test** en su zona.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un registro de alias para hacer referencia a un registro de recursos dentro de la zona. Para más información acerca de Azure DNS y sobre las aplicaciones web, continúe con el tutorial sobre estas.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)
