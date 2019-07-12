---
Description: Este guia explica como solucionar problemas de tempo de execução em um contêiner de MSIX.
title: Solucionar problemas de tempo de execução em um contêiner de MSIX
ms.date: 07/11/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: fdfaedf51333f93edd7e12f1c2ad8ea879bfc8ae
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829379"
---
# <a name="troubleshoot-runtime-issues-in-an-msix-container"></a>Solucionar problemas de tempo de execução em um contêiner de MSIX 

Neste artigo, examinaremos como é possível solucionar problemas de tempo de execução que ocorrem em um contêiner MSIX. Contêineres MSIX por si só são relativamente simples e direta. Conforme mais aplicativos são executados dentro a mesma identidade do pacote com a Ajuda de pacotes de modificação, o registro virtual e o sistema de arquivos virtual será em camadas sobre na ordem em que os aplicativos são instalados. 

Pode haver casos onde a ordem na qual esses aplicativos são instalados pode causar problemas imprevistos onde as chaves do registro esperada poderão ser substituídas e arquivos esperados podem ser substituídos. 

Para ajudar a diagnosticar esses problemas [Invoke-CommandInDesktopPackage](https://docs.microsoft.com/en-us/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps) é um cmdlet do PowerShell que pode ser usado para executar um aplicativo dentro do contêiner MSIX. Isso permite que os usuários executem o prompt de comando, o editor do registro, PowerShell dentro do contêiner MSIX e obter uma exibição do hive do registro mesclado e sistema de arquivos mesclados. 

 > [!IMPORTANT]
 > CommandInDesktopPackage invocar requer que o dispositivo esteja no modo de desenvolvedor. 


## <a name="view-the-merged-file-system"></a>Modo de exibição do sistema de arquivos mesclados

Para exibir o sistema de arquivos, conforme observado pelos aplicativos que são executados dentro do contêiner, use o seguinte comando do PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "cmd.exe" -PreventBreakaway
```

O comando acima iniciará uma instância de cmd.exe no *29270sandstorm. AppPackage1_gah1vdar1nn7a* contêiner de pacote. Conforme você estiver executando o prompt de comando dentro do contêiner, você pode procurar o sistema de arquivos e exibir os arquivos mesclados. 

## <a name="view-the-merged-registry-hive"></a>Exibir o hive do registro mesclado

Para exibir o hive do registro de dispositivo completo como observada pelos aplicativos que estão executando o contêiner de insider, use o seguinte comando do PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "regedit.exe" -PreventBreakaway
```

O comando acima será iniciado o editor do registro dentro do contexto da *29270sandstorm. AppPackage1_gah1vdar1nn7a* contêiner de pacote. Aqui você pode navegar por meio do computador local e as chaves de registro de usuário atual e identificar a possível causa que está causando o problema. 

 >[!TIP]
 > Use '-PreventBreakaway' sinalizador ao usar Invoke-CommandInDesktopPackage se você gostaria de iniciar processos subsequentes no mesmo contêiner. Caso contrário, qualquer inicialização subsequente será interrompido para fora do contêiner. 

 >[!NOTE]
 > Nem todos os aplicativos podem ser iniciados dentro do contêiner. Por exemplo, explorer.exe será debates do contêiner.
