---
title: Квоты и доступность по регионам для службы "Экземпляры контейнеров Azure"
description: Квоты и доступность по регионам по умолчанию службы "Экземпляры контейнеров Azure".
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/27/2018
ms.author: danlep
ms.openlocfilehash: 427dd8bd4abb72e2750752d828e189921401e9e0
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902362"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Квоты и доступность по регионам для службы "Экземпляры контейнеров Azure"

Все службы Azure включают несколько стандартных ограничений и квот для ресурсов и функций. Указанные ниже разделы содержат подробную информацию об ограничениях ресурсов по умолчанию для нескольких ресурсов службы "Экземпляры контейнеров Azure" (ACI), а также сведения о доступности службы ACI в регионах Azure.

## <a name="service-quotas-and-limits"></a>Квоты и ограничения службы

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Регионы доступности

Служба "Экземпляры контейнеров Azure" доступна в следующих регионах с указанными ограничениями для ЦП и памяти.

| Расположение | ОС | ЦП | Память (ГБ) |
| -------- | -- | :---: | :-----------: |
| Восточная часть США, Северная Европа, Западная Европа, западная часть США, западная часть США 2 | Linux | 4. | 14 |
| Восточная Австралия, восточная часть США 2, Юго-Восточная Азия | Linux | 2 | 7 |
| Центральная Индия, центрально-южная часть США | Linux | 2 | 3,5 |
| Восточная часть США, Западная Европа, восточная часть США | Windows | 4. | 14 |
| Восточная Австралия, Центральная Индия, восточная часть США 2, Северная Европа, центрально-южная часть США, Юго-Восточная Азия, западная часть США 2 | Windows | 2 | 3,5 |

Экземпляры контейнеров, созданные с учетом этих ограничений, зависят от доступности ресурсов в регионе развертывания. Если в регионе высокая нагрузка, при развертывании экземпляров могут возникать сбои. Чтобы избежать таких сбоев, попробуйте развернуть экземпляры с меньшим количеством ЦП и объемом памяти либо повторите попытку развертывания позже.

Сообщите команде о нужных дополнительных регионах или повышенных ограничениях ЦП или памяти на странице [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Дополнительные сведения об устранении неполадок в развертывании экземпляра контейнера см. в статье [Устранение неполадок развертывания с помощью службы "Экземпляры контейнеров Azure"](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Дополнительная информация

Вы можете повысить лимит нескольких ограничений и квот. Чтобы запросить увеличение квоты одного или нескольких ресурсов, которые поддерживают такую возможность, отправьте [запрос в службу поддержки Azure][azure-support] (в качестве **типа проблемы** укажите "Квота").

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
