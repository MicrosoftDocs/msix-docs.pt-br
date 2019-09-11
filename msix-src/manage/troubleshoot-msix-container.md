---
Description: Este guia explica como solucionar problemas de tempo de execução em um contêiner MSIX.
title: Solucionar problemas de tempo de execução em um contêiner MSIX
ms.date: 07/11/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: c1a8d489ca93c075ce8b87e98fb2ada3bade7d91
ms.sourcegitcommit: 9cb3d2cdbe03b300bef60ed949e5e4d3b24d35ba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864024"
---
# <a name="troubleshoot-runtime-issues-in-an-msix-container"></a>Solucionar problemas de tempo de execução em um contêiner MSIX 

Neste artigo, examinaremos como você pode solucionar problemas de tempo de execução que ocorrem em um contêiner MSIX. Os contêineres MSIX por si só são relativamente simples e diretos. À medida que mais aplicativos são executados dentro da mesma identidade do pacote com a ajuda de pacotes de modificação, o registro virtual e o sistema de arquivos virtual serão layed na ordem em que os aplicativos são instalados. 

Pode haver casos em que a ordem em que esses aplicativos são instalados pode causar problemas de imprevistos em que as chaves de registro esperadas podem ser substituídas e os arquivos esperados podem ser substituídos. 

Para ajudar a diagnosticar esses problemas, o [Invoke-CommandInDesktopPackage](https://docs.microsoft.com/en-us/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps) é um cmdlet do PowerShell que pode ser usado para executar um aplicativo dentro do contêiner MSIX. Isso permite que os usuários executem o prompt de comando, o editor do registro, o PowerShell dentro do contêiner MSIX e obtenham uma exibição do sistema de arquivos mesclado e do hive do registro mesclado. 

 > [!IMPORTANT]
 > Invoke-CommandInDesktopPackage exige que o dispositivo esteja no modo de desenvolvedor para os builds do Windows 10 anteriores a 18922.


## <a name="view-the-merged-file-system"></a>Exibir o sistema de arquivos mesclados

Para exibir o sistema de arquivos conforme observado pelos aplicativos que estão sendo executados dentro do contêiner, use o seguinte comando do PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "cmd.exe" -PreventBreakaway
```

O comando acima iniciará uma instância do cmd. exe no contêiner de pacote *contoso. AppPackage1_8h66172c634n0* . Como você está executando o prompt de comando de dentro do contêiner, você pode navegar pelo sistema de arquivos e exibir os arquivos mesclados. 

## <a name="view-the-merged-registry-hive"></a>Exibir o hive mesclado do registro

Para exibir o hive completo do registro de dispositivo, conforme observado pelos aplicativos que estão executando o contêiner Insider, use o seguinte comando do PowerShell:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "regedit.exe" -PreventBreakaway
```

O comando acima iniciará o editor do registro no contexto do contêiner do pacote *contoso. AppPackage1_8h66172c634n0* . Aqui você pode navegar pelo computador local e pelas chaves do registro do usuário atual e identificar possíveis infratores que estão causando o problema. 

 >[!TIP]
 > Use o sinalizador '-PreventBreakaway ' ao usar Invoke-CommandInDesktopPackage se desejar iniciar processos subsequentes no mesmo contêiner. Caso contrário, qualquer inicialização subsequente será interrompida no contêiner. 

 >[!NOTE]
 > Nem todos os aplicativos podem ser iniciados dentro do contêiner. Por exemplo, o Explorer. exe fará a busca do contêiner.
