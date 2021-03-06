---
title: Запуск и остановка пакета средств разработки Azure Stack (ASDK) | Документация Майкрософт
description: Сведения о запуске и завершении работы пакета средств разработки Azure Stack (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 98ae7fe0fde922aee28ffc05b64d223a40085447
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340925"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Запуск и остановка пакета средств разработки Azure Stack (ASDK)
Не рекомендуется обычный перезапуск главного компьютера ASDK. Следуйте инструкциям по корректному завершению работы и перезапуску служб ASDK, приведенным в этой статье. 

## <a name="stop-azure-stack"></a>Остановка Azure Stack 
Чтобы правильно завершить работу служб Azure Stack и главного компьютера ASDK, используйте следующие команды PowerShell:

1. Войдите на узел управления с именем AzureStack\CloudAdmin.
2. Откройте PowerShell от имени администратора (не путайте с интегрированной средой сценариев Windows PowerShell).
3. Для установки сеанса привилегированной конечной точки (PEP) выполните следующие команды: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Затем в сеансе PEP используйте командлет **Stop-AzureStack**, чтобы остановить работу служб Azure Stack и завершить работу главного компьютера ASDK:

   ```powershell
   Stop-AzureStack
   ```
5. Просмотрите выходные данные PowerShell, чтобы убедиться, что все службы Azure Stack успешно завершили свою работу до завершения работы главного компьютера ASDK. Этот процесс занимает несколько минут.

## <a name="start-azure-stack"></a>Запуск Azure Stack 
Службы ASDK должны запускаться автоматически при запуске главного компьютера. Однако время запуска служб инфраструктуры ASDK зависит от производительности конфигурации аппаратного обеспечения главного компьютера ASDK. В некоторых случаях для успешного перезапуска всех служб может потребоваться несколько часов.

Независимо от того, каким образом была завершена работа ASDK, после включения главного компьютера необходимо выполнить шаги ниже, чтобы убедиться, что все службы Azure Stack запущены и полностью работоспособны: 

1. Включите главный компьютер ASDK. 
2. Войдите на узел управления с именем AzureStack\CloudAdmin.
3. Откройте PowerShell от имени администратора (не путайте с интегрированной средой сценариев Windows PowerShell).
4. Для установки сеанса привилегированной конечной точки (PEP) выполните следующие команды:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Затем в сеансе PEP выполните следующие команды, чтобы проверить состояние запуска служб Azure Stack:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Просмотрите выходные данные, чтобы убедиться, что службы Azure Stack успешно перезапущены.

Чтобы узнать больше о рекомендуемых процедурах для правильного завершения работы и перезапуска служб Azure Stack, ознакомьтесь со статьей [Запуск и остановка Azure Stack](.\.\azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Устранение неполадок при запуске и завершении работы 
Выполните эти шаги, если службы Azure Stack не запускаются в течение двух часов после включения главного компьютера ASDK:

1. Войдите на узел управления с именем AzureStack\CloudAdmin.
2. Откройте PowerShell от имени администратора (не путайте с интегрированной средой сценариев Windows PowerShell).
3. Для установки сеанса привилегированной конечной точки (PEP) выполните следующие команды:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Затем в сеансе PEP выполните следующие команды, чтобы проверить состояние запуска служб Azure Stack:

   ```powershell
   Test-AzureStack
   ```
5. Просмотрите выходные данные и устраните ошибки. Дополнительные сведения см. в статье [Запуск проверочного теста в Azure Stack](.\.\azure-stack-diagnostic-test.md).
6. Перезапустите службы Azure Stack из этого сеанса PEP, запустив командлет **Start-AzureStack**:

   ```powershell
   Start-AzureStack
   ```

Если запуск командлета **Start-AzureStack** завершается сбоем, посетите [форум технической поддержки Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack), чтобы получить помощь по устранению неполадок с ASDK. 

## <a name="next-steps"></a>Дополнительная информация 
Дополнительные сведения о средстве диагностики Azure Stack и регистрации проблем см. в статье [Средства диагностики Azure Stack](.\.\azure-stack-diagnostics.md).
