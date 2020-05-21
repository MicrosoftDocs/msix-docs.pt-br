---
Description: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
title: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
ms.date: 05/13/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0af2ab1de7c43ed6060048d68e148afefbc1e10f
ms.sourcegitcommit: 8d6bc53d5f5ae80d9ce191fe81660407e9f11e0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83430428"
---
# <a name="create-a-package-support-framework-fixup"></a>Criar a correção do Package Support Framework 

Se não houver correção de tempo de execução para o problema, você poderá criar uma nova correção de tempo de execução escrevendo funções de substituição e incluindo quaisquer dados de configuração que façam sentido. Vamos examinar cada parte.

### <a name="replacement-functions"></a>Funções de substituição

Primeiro, identifique quais chamadas de função falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você poderá criar funções de substituição que você deseja que sejam chamadas pelo gerenciador de runtime. Isso oferecerá uma oportunidade de substituir a implementação de uma função por um comportamento que esteja de acordo com as regras do ambiente moderno do runtime.

Declare a ``FIXUP_DEFINE_EXPORTS`` macro e, em seguida, adicione uma instrução include para a `fixup_framework.h` na parte superior de cada uma. Arquivo CPP no qual você pretende adicionar as funções de sua correção de tempo de execução.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Verifique se a `FIXUP_DEFINE_EXPORTS` macro aparece antes da instrução include.

Crie uma função que tenha a mesma assinatura da função que é o comportamento que você deseja modificar. Aqui está uma função de exemplo que substitui a `MessageBoxW` função.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

A chamada para `DECLARE_FIXUP` mapeia a `MessageBoxW` função para a nova função de substituição. Quando o aplicativo tentar chamar a `MessageBoxW` função, ele chamará a função de substituição em vez disso.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Proteger contra chamadas recursivas para funções em correções de tempo de execução

O `reentrancy_guard` tipo pode ser adicionado às suas funções para protegê-los contra chamadas de função recursivas.

Por exemplo, você pode produzir uma função de substituição para a `CreateFile` função. Sua implementação pode chamar a `CopyFile` função, mas a implementação da `CopyFile` função pode chamar a `CreateFile` função. Isso pode levar a um ciclo recursivo infinito de chamadas para a `CreateFile` função.

Para obter mais informações sobre o, `reentrancy_guard` consulte [Authoring.MD](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Dados de configuração

Se você quiser adicionar dados de configuração à correção de tempo de execução, considere adicioná-los ao ``config.json`` . Dessa forma, você pode usar o `FixupQueryCurrentDllConfig` para analisar facilmente esses dados. Este exemplo analisa um valor booliano e de cadeia de caracteres desse arquivo de configuração.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

### <a name="fixup-metadata"></a>Metadados de correção

Cada correção e o aplicativo iniciador PSF tem um arquivo de metadados XML que contém as seguintes informações:

* Versão: a versão do PSF está em MAJOR. Secundária. Formato de PATCH de acordo com [sem versão 2](https://semver.org/).
* Plataforma mínima do Windows: a versão mínima do Windows necessária para a correção ou o inicializador PSF.
* Descrição: uma breve descrição da correção.
* WhenToUse: heurística sobre quando você deve aplicar a correção.

Para obter um exemplo, consulte o arquivo de metadados [FileRedirectionFixupMetadata. xml](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/fixups/FileRedirectionFixup/FileRedirectionFixupMetadata.xml) para a correção de redirecionamento. O esquema de metadados está disponível [aqui](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/MetadataSchema.xsd).