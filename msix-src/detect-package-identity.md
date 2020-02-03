---
title: Detectar a identidade do pacote e o contexto do runtime
description: Descreve como um aplicativo pode determinar se ele foi enviado como um pacote MSIX no Win 1709 ou posterior.
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 0fe0a721368b10f68ba30665883e9070692dbb04
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726586"
---
# <a name="detect-package-identity-and-runtime-context"></a>Detectar a identidade do pacote e o contexto do runtime

Você pode ter algumas versões do seu aplicativo que não foram distribuídas em um pacote MSIX. Em tempo de execução, seu aplicativo pode detectar se ele foi implantado como um pacote MSIX usando a API do Gerenciador de pacotes do Windows ou seu próprio instalador personalizado. Talvez você queira alterar o comportamento do aplicativo, como configurações de atualização, ou talvez queira aproveitar a funcionalidade disponível somente para pacotes MSIX.

Para determinar se o aplicativo está sendo executado como um pacote MSIX em uma versão do Windows que dá suporte ao conjunto de recursos completo do MSIX, você pode usar a função nativa [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) no Kernel32. dll. Quando um aplicativo de área de trabalho está sendo executado como um aplicativo não empacotado sem a identidade do pacote, essa função retorna um erro que pode ajudá-lo a inferir o contexto no qual o aplicativo está sendo executado.

Se a função for realizada com sucesso, isso significa que:

* Seu aplicativo é empacotado em um pacote MSIX.
* Seu aplicativo está em execução no Windows 10, versão 1709 (Build 16299) ou posterior com suporte completo a MSIX.

## <a name="use-getcurrentpackagefullname-in-native-code"></a>Usar GetCurrentPackageFullName em código nativo

O exemplo de código a seguir demonstra como usar [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) para determinar o contexto de um aplicativo.

```cpp
#define _UNICODE 1
#define UNICODE 1

#include <Windows.h>
#include <appmodel.h>
#include <malloc.h>
#include <stdio.h>

int __cdecl wmain()
{
    UINT32 length = 0;
    LONG rc = GetCurrentPackageFullName(&length, NULL);
    if (rc != ERROR_INSUFFICIENT_BUFFER)
    {
        if (rc == APPMODEL_ERROR_NO_PACKAGE)
            wprintf(L"Process has no package identity\n");
        else
            wprintf(L"Error %d in GetCurrentPackageFullName\n", rc);
        return 1;
    }

    PWSTR fullName = (PWSTR) malloc(length * sizeof(*fullName));
    if (fullName == NULL)
    {
        wprintf(L"Error allocating memory\n");
        return 2;
    }

    rc = GetCurrentPackageFullName(&length, fullName);
    if (rc != ERROR_SUCCESS)
    {
        wprintf(L"Error %d retrieving PackageFullName\n", rc);
        return 3;
    }
    wprintf(L"%s\n", fullName);

    free(fullName);

    return 0;
}
```

## <a name="use-getcurrentpackagefullname-function-in-managed-code"></a>Usar a função GetCurrentPackageFullName no código gerenciado

Para chamar o [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) em um aplicativo .NET Framework gerenciado, você precisará usar a [invocação de plataforma (P/Invoke)](https://docs.microsoft.com/dotnet/standard/native-interop/pinvoke) ou alguma outra forma de interoperabilidade.

Para simplificar esse processo, você pode usar a biblioteca [DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/) . Esta biblioteca dá suporte a .NET Framework 4 e posteriores e usa P/Invoke internamente para fornecer uma classe auxiliar que determina se o aplicativo está sendo executado em uma versão do Windows que dá suporte ao conjunto completo de recursos MSIX. Essa biblioteca também está disponível como um [pacote NuGet](https://www.nuget.org/packages/DesktopBridge.Helpers/).

Depois de instalar o pacote em seu projeto, você pode criar uma nova instância da classe `DesktopBridge.Helpers` e chamar o método `IsRunningAsUwp`. Esse método retornará true se seu aplicativo estiver sendo executado como um pacote MSIX no Windows 10, versão 1709 (Build 16299) ou posterior e false se um deles não for verdadeiro. O exemplo a seguir demonstra como chamar esse método.

```csharp
private bool IsRunningAsUwp()
{
   UwpHelpers helpers = new UwpHelpers();
   return helpers.IsRunningAsUwp();
}

private void Form1_Load(object sender, EventArgs e)
{
   if (IsRunningAsUwp())
   {
       txtUwp.Text = "I'm running as MSIX";
   }
   else
   {
       txtUwp.Text = "I'm running as a native desktop app";
   }
}
```
