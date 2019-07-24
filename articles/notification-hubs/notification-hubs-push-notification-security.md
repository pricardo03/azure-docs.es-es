---
title: Seguridad de Notification Hubs
description: En este tema se explica la seguridad de los Centros de notificaciones de Azure.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 05/31/2019
ms.author: jowargo
ms.openlocfilehash: 3f5b23028094b545262e9c01640890f2c0b989ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431258"
---
# <a name="notification-hubs-security"></a>Seguridad de Notification Hubs

## <a name="overview"></a>Información general

En este tema se describe el modelo de seguridad de Azure Notification Hubs.

## <a name="shared-access-signature-security-sas"></a>Seguridad de Firma de acceso compartido (SAS)

Notification Hubs implementa un esquema de seguridad de nivel de entidad llamado SAS (Firma de acceso compartido). Este esquema permite a las entidades de mensajería declarar hasta 12 reglas de autorización en su descripción que conceden derechos sobre esa entidad.

Cada regla contiene un nombre, un valor de clave (secreto compartido) y un conjunto de derechos, tal como se explica en [Notificaciones de seguridad](#security-claims). Al crear un Centro de notificaciones, automáticamente se crean dos reglas: una con derechos de **escucha** (que usa la aplicación cliente) y otra con todos **los** derechos (que usa el back-end de la aplicación).

Al realizar la administración de registros desde aplicaciones cliente, si la información enviada a través de notificaciones no es confidencial (por ejemplo, actualizaciones del información meteorológica), una forma común de acceder a un Centro de notificaciones es dar al valor de clave de la regla acceso de solo escucha a la aplicación cliente y proporcionar al valor de clave de la regla acceso completo al back-end de la aplicación.

Las aplicaciones no deben incrustar el valor de clave en las aplicaciones cliente de la Tienda Windows; en su lugar, la aplicación cliente debe recuperarlo del back-end de la aplicación en el inicio.

La clave con acceso de **escucha** permite a una aplicación cliente registrarse para cualquier etiqueta. Si la aplicación debe restringir los registros en etiquetas específicas a clientes específicos (por ejemplo, si las etiquetas representan identificadores de usuario), el back-end de la aplicación debe realizar los registros. Para más información, [consulte Administración de registros](notification-hubs-push-notification-registration-management.md). Tenga en cuenta que, de este modo, la aplicación cliente no tendrá acceso directo a Notification Hubs.

## <a name="security-claims"></a>Notificaciones de seguridad

De modo similar a otras entidades, las operaciones del Centro de notificaciones se permiten para tres notificaciones de seguridad: **escucha**, **envío** y **administración**.

| Notificación   | DESCRIPCIÓN                                          | Operaciones permitidas |
| ------- | ---------------------------------------------------- | ------------------ |
| Escuchar  | Crear o actualizar, leer y eliminar registros únicos | Crear o actualizar registro<br><br>Leer el registro<br><br>Leer todos los registros de un controlador<br><br>Eliminar registro |
| Envío    | Enviar mensajes al Centro de notificaciones                | Enviar mensaje |
| administración  | CRUD en Notification Hubs (incluida la actualización de las credenciales de PNS y claves de seguridad) y registros de lectura basados en etiquetas |Crear/Actualizar/Eliminar Centros de notificaciones<br><br>Leer registros por etiqueta |

Notification Hubs acepta tokens de firma generados con claves compartidas configuradas directamente en el centro de notificaciones.

No es posible enviar una notificación a más de un espacio de nombres. Los espacios de nombres son un contenedor lógico para centros de notificaciones y no participan en el envío de notificaciones.
Las directivas de acceso de nivel de espacio de nombres (credenciales) se pueden usar para operaciones de nivel de espacio de nombres, por ejemplo, para crear listas de centros de notificaciones, crear o eliminar centros de notificaciones, etc. Solo las directivas de acceso de nivel de centro permiten enviar notificaciones.
