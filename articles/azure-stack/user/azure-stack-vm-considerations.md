---
title: Diferencias y consideraciones para las máquinas virtuales en Azure Stack | Microsoft Docs
description: Aprenda sobre las diferencias y consideraciones al trabajar con máquinas virtuales en Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: brenduns
ms.openlocfilehash: 50c0f293ac669ade4e45a5f45b0adf9a7c4b6c36
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
---
# <a name="considerations-for-virtual-machines-in-azure-stack"></a>Consideraciones sobre máquinas virtuales en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Las máquinas virtuales son recursos informáticos escalables y a petición que ofrece Azure Stack. Si usa máquinas virtuales, debe saber que existen diferencias entre las características que están disponibles en Azure y en Azure Stack. Este artículo proporciona información general sobre las consideraciones únicas para las máquinas virtuales y sus características en Azure Stack. Para obtener información acerca de las diferencias de alto nivel entre Azure y Azure Stack, consulte el artículo [Key considerations](azure-stack-considerations.md) (Consideraciones clave).

## <a name="cheat-sheet-virtual-machine-differences"></a>Hoja de referencia rápida: Diferencias entre máquinas virtuales

| Característica | Azure (global) | Azure Stack |
| --- | --- | --- |
| Imágenes de máquinas virtuales | Azure Marketplace contiene imágenes que puede usar para crear una máquina virtual. Consulte la página de [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) para ver la lista de imágenes que están disponibles en Azure Marketplace. | De forma predeterminada, no hay ninguna imagen disponible en el Marketplace de Azure Stack. El administrador de la nube de Azure Stack debe publicar o descargar imágenes en el Marketplace de Azure Stack antes de que los usuarios puedan usarlas. |
| Tamaños de máquina virtual | Azure admite una amplia variedad de tamaños para máquinas virtuales. Para aprender más acerca de las opciones y los tamaños disponibles, consulte los temas [Tamaños de máquinas virtuales Windows](../../virtual-machines/virtual-machines-windows-sizes.md) y [Tamaños de máquina virtual Linux](../../virtual-machines/linux/sizes.md). | Azure Stack admite un subconjunto de tamaños de máquinas virtuales que están disponibles en Azure. Para ver la lista de tamaños admitidos, consulte la sección [Tamaños de máquina virtual](#virtual-machine-sizes) de este artículo. |
| Cuotas de máquinas virtuales | [Límites de cuota](../../azure-subscription-service-limits.md#service-specific-limits) establecidos por Microsoft | El administrador de la nube de Azure Stack debe asignar cuotas antes de ofrecer máquinas virtuales a los usuarios. |
| Extensiones de máquina virtual |Azure admite una amplia variedad de extensiones para máquinas virtuales. Para aprender acerca de las extensiones disponibles, consulte el artículo [Extensiones y características de las máquinas virtuales](../../virtual-machines/windows/extensions-features.md).| Azure Stack admite un subconjunto de extensiones que están disponibles en Azure y cada una de las extensiones tienen versiones específicas. El administrador de la nube de Azure Stack puede elegir qué extensiones están disponibles para sus usuarios. Para ver la lista de extensiones admitidas, consulte la sección [Extensiones de máquina virtual](#virtual-machine-extensions) de este artículo. |
| Red de máquinas virtuales | Las direcciones IP públicas asignadas a la máquina virtual del inquilino son accesibles a través de Internet.<br><br><br>Azure Virtual Machines tiene un nombre de DNS fijo | Las direcciones IP públicas asignadas a una máquina virtual del inquilino son accesibles solo desde el entorno del Kit de desarrollo de Azure Stack. Un usuario debe tener acceso al Kit de desarrollo de Azure Stack a través de un [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) o una [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) para conectarse a una máquina virtual que se crea en Azure Stack.<br><br>Las máquinas virtuales creadas dentro de una instancia específica de Azure Stack tienen un nombre DNS basado en el valor que se configura mediante el administrador de la nube. |
| Almacenamiento de máquina virtual | Admite [discos administrados](../../virtual-machines/windows/managed-disks-overview.md). | Los discos administrados no se admiten todavía en Azure Stack. |
| Versiones de API | Azure tiene siempre las últimas versiones de API para todas las características de la máquina virtual. | Azure Stack es compatible con servicios específicos de Azure y versiones de API específicas para estos servicios. Para ver la lista de versiones de API compatibles, consulte la sección [versiones de API](#api-versions) de este artículo. |
|Conjuntos de disponibilidad de máquinas virtuales|Varios dominios de error (2 o 3 por región)<br>Varios dominios de actualización<br>Compatible con el disco administrado|Varios dominios de error (2 o 3 por región)<br>Varios dominios de actualización (hasta 20)<br>No compatible con el disco administrado|
|Conjuntos de escalado de máquinas virtuales|Compatible con escalado automático|No compatible con escalado automático.<br>Agregar más instancias a un conjunto de escalado con el portal, las plantillas de Resource Manager o PowerShell.

## <a name="virtual-machine-sizes"></a>Tamaños de máquina virtual
Azure impone límites de recursos de varias formas para evitar el consumo excesivo de recursos (nivel de servicio y local del servidor). Sin imponer límites en el consumo de recursos de un inquilino, la experiencia de este puede verse afectada si un vecino ruidoso consume recursos en exceso. 
- Para la salida de redes de la máquina virtual, hay extremos de ancho de banda. Los extremos de Azure Stack coinciden con los de Azure.  
- En el caso de los recursos de almacenamiento, Azure Stack implementa límites de IOPS de almacenamiento para evitar el consumo excesivo básico de recursos por parte de los inquilinos para el acceso de almacenamiento. 
- En el caso de las máquinas virtuales con varios discos de datos adjuntos, el rendimiento máximo de cada disco de datos individual es de 500 IOPS para HHD y de 2300 IOPS para SSD.

En la tabla siguiente se enumeran las máquinas virtuales que se admiten en Azure Stack junto con su configuración:

| type           | Tamaño          | Intervalo de tamaños admitidos |
| ---------------| ------------- | ------------------------ |
|Uso general |A básico        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Uso general |Estándar A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Uso general |Serie D       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Uso general |Serie Dv2     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Uso general |Serie DS      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Uso general |DSv2-series    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Memoria optimizada|Serie D       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Memoria optimizada|Serie DS      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Memoria optimizada|Serie Dv2     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Memoria optimizada|Serie DSv2 -  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Los tamaños de máquina virtual y sus cantidades de recursos asociados son coherentes entre Azure y Azure Stack. Por ejemplo, esta coherencia incluye la cantidad de memoria, el número de núcleos y la cantidad y el tamaño de los discos de datos que se pueden crear. Sin embargo, el rendimiento del mismo tamaño de máquina virtual en Azure Stack depende de las características subyacentes de un entorno de Azure Stack concreto.

## <a name="virtual-machine-extensions"></a>Extensiones de máquina virtual

 Azure Stack incluye un pequeño conjunto de extensiones. Las actualizaciones y las extensiones adicionales están disponibles a través de la redifusión de Marketplace.

Use el siguiente script de PowerShell para obtener la lista de extensiones de máquinas virtuales que están disponibles en su entorno de Azure Stack:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>Versiones de API

Las características de la máquina virtual en Azure Stack admiten las siguientes versiones de API:

![Tipos de recursos de máquinas virtuales](media/azure-stack-vm-considerations/vm-resoource-types.png)

Puede usar el siguiente script de PowerShell para obtener las versiones de API para características de máquinas virtuales que están disponibles en su entorno de Azure Stack:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```
La lista de versiones de API y tipos de recursos admitidos puede variar si el operador de nube actualiza su entorno de Azure Stack a una versión más reciente.

## <a name="next-steps"></a>Pasos siguientes

[Cree una máquina virtual Windows con PowerShell en Azure Stack](azure-stack-quick-create-vm-windows-powershell.md)
