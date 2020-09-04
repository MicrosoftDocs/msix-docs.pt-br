---
title: Detectar a identidade do pacote e o contexto do runtime
description: Descreve como um aplicativo pode determinar se foi enviado como um pacote MSIX no Win 1709 ou posterior.
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: da35e755235f1dc22d51c52e54462e5765945e97
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091204"
---
# <a name="detect-package-identity-and-runtime-context"></a>Detectar a identidade do pacote e o contexto do runtime

Você poderá ter algumas versões do aplicativo que não foram distribuídas em um pacote MSIX. Em runtime, o aplicativo pode detectar se foi implantado como um pacote MSIX usando a API do Gerenciador de Pacotes do Windows ou o próprio instalador personalizado. O ideal é alterar o comportamento do aplicativo, como as configurações de atualização, ou aproveitar a funcionalidade disponível somente para pacotes MSIX.

Para determinar se o seu aplicativo está sendo executado como um pacote MSIX em uma versão do Windows que dê suporte ao conjunto completo de recursos do MSIX, use a função nativa [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) na kernel32.dll. Quando um aplicativo da área de trabalho está sendo executado como um aplicativo não empacotado sem a identidade do pacote, essa função retorna um erro que pode ajudar você a inferir o contexto no qual o aplicativo está sendo executado.

Se a função for bem-sucedida, isso indicará que:

* O aplicativo está empacotado em um pacote MSIX.
* O aplicativo está em execução no Windows 10, versão 1709 (build 16299) ou posterior com suporte completo a MSIX.

## <a name="use-getcurrentpackagefullname-in-native-code"></a>Usar GetCurrentPackageFullName no código nativo

O exemplo de código a seguir demonstra como usar [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) para determinar o contexto de um aplicativo.

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

Para chamar [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) em um aplicativo .NET Framework gerenciado, você precisará usar [P/Invoke (invocação de plataforma)](/dotnet/standard/native-interop/pinvoke) ou alguma outra forma de interoperabilidade.

Para simplificar esse processo, use a biblioteca [DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/). Esta biblioteca dá suporte ao .NET Framework 4 e posterior e usa P/Invoke internamente para fornecer uma classe auxiliar que determina se o aplicativo está sendo executado em uma versão do Windows que dá suporte ao conjunto completo de recursos MSIX. Essa biblioteca também está disponível como um [pacote NuGet](https://www.nuget.org/packages/DesktopBridge.Helpers/).

Depois de instalar o pacote no projeto, crie uma instância da classe `DesktopBridge.Helpers` e chame o método `IsRunningAsUwp`. Esse método retornará true se o aplicativo estiver sendo executado como um pacote MSIX no Windows 10, versão 1709 (build 16299) ou posterior e false se um desses requisitos não for true. A amostra a seguir descreve como chamar esse método.

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