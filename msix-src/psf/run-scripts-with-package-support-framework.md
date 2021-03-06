---
description: Você pode executar scripts com a estrutura de suporte do pacote para personalizar seu aplicativo de área de trabalho para o ambiente do usuário.
title: Executar scripts com o Package Support Framework
ms.date: 09/20/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f684eae955f9a1f03d2008aee5ed6e3f594c296c
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090654"
---
# <a name="run-scripts-with-the-package-support-framework"></a>Executar scripts com o Package Support Framework


Os scripts permitem que os profissionais de ti personalizem um aplicativo dinamicamente para o ambiente do usuário depois que ele é empacotado usando MSIX. Por exemplo, você pode usar scripts para configurar seu banco de dados, configurar uma VPN, montar uma unidade compartilhada ou executar uma verificação de licença dinamicamente. Os scripts fornecem muita flexibilidade. Eles podem alterar as chaves do registro ou executar modificações de arquivo com base na configuração do computador ou do servidor.

Você pode usar a estrutura de suporte de pacote (PSF) para executar um script do PowerShell antes que um executável de aplicativo empacotado seja executado e um script do PowerShell após a execução do executável do aplicativo para limpeza. Cada executável de aplicativo definido no manifesto do aplicativo pode ter seus próprios scripts. Você pode configurar o script para ser executado apenas uma vez na primeira inicialização do aplicativo e sem mostrar a janela do PowerShell para que os usuários não terminem o script prematuramente por engano. Há outras opções para configurar a maneira como os scripts podem ser executados, mostrados abaixo.

## <a name="prerequisites"></a>Pré-requisitos

Para permitir que os scripts sejam executados, você precisa definir a política de execução do PowerShell como `RemoteSigned` . Você pode fazer isso executando este comando:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

A política de execução precisa ser definida para o executável do PowerShell de 64 bits e o executável do PowerShell de 32 bits. Certifique-se de abrir cada versão do PowerShell e execute um dos comandos mostrados acima.

Aqui estão os locais de cada executável.

* Computador de 64 bits:
  * executável de 64 bits:% SystemRoot% \system32\WindowsPowerShell\v1.0\powershell.exe
  * executável de 32 bits:% SystemRoot% \SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* computador de 32 bits:
  * executável de 32 bits:% SystemRoot% \system32\WindowsPowerShell\v1.0\powershell.exe

Para obter mais informações sobre as políticas de execução do PowerShell, consulte [Este artigo](/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6).

Certifique-se também de incluir o arquivo de StartingScriptWrapper.ps1 em seu pacote e colocá-lo na mesma pasta que o executável. Você pode copiar esse arquivo do [pacote NuGet do PSF](https://www.nuget.org/packages/Microsoft.PackageSupportFramework/).

## <a name="enable-scripts"></a>Habilitar scripts

Para especificar quais scripts serão executados para cada executável de aplicativo empacotado, você precisará modificar o [config.jsno arquivo](package-support-framework.md#create-a-configuration-file). Para informar ao PSF para executar um script antes da execução do aplicativo empacotado, adicione um item de configuração chamado `startScript` . Para informar ao PSF para executar um script depois que o aplicativo empacotado terminar, adicione um item de configuração chamado `endScript` .

### <a name="script-configuration-items"></a>Itens de configuração de script

A seguir estão os itens de configuração disponíveis para os scripts. O script final ignora os `waitForScriptToFinish` itens de `stopOnScriptError` configuração e.

| Nome da chave                | Tipo de valor | Necessário? | Padrão  | Descrição
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | string     | Sim       | N/D      | O caminho para o script, incluindo o nome e a extensão. O caminho é relativo ao diretório de trabalho do aplicativo, se especificado, caso contrário, ele será iniciado no diretório raiz do pacote.
| `scriptArguments`         | Cadeia de caracteres     | No        | vazio    | Lista de argumentos delimitados por espaço. O formato é o mesmo para uma chamada de script do PowerShell. Essa cadeia de caracteres é anexada a `scriptPath` para fazer uma chamada de PowerShell.exe válida.
| `runInVirtualEnvironment` | booleano    | Não        | true     | Especifica se o script deve ser executado no mesmo ambiente virtual em que o aplicativo empacotado é executado.
| `runOnce`                 | booleano    | Não        | true     | Especifica se o script deve ser executado uma vez por usuário, por versão.
| `showWindow`              | booleano    | Não        | false    | Especifica se a janela do PowerShell é mostrada.
| `stopOnScriptError`       | booleano    | Não        | false    | Especifica se deseja sair do aplicativo se o script inicial falhar.
| `waitForScriptToFinish`   | booleano    | Não        | true     | Especifica se o aplicativo empacotado deve aguardar a conclusão do script inicial antes de iniciar.
| `timeout`                 | DWORD      | Não        | INFINITE | Quanto tempo o script terá permissão para ser executado. Quando o tempo decorrido, o script será interrompido.

> [!NOTE]
> `stopOnScriptError: true` `waitForScriptToFinish: false` Não há suporte para a configuração e para o aplicativo de exemplo. Se você definir esses dois itens de configuração, PSF retornará o erro ERROR_BAD_CONFIGURATION.


## <a name="sample-configuration"></a>Exemplo de configuração

Aqui está um exemplo de configuração usando dois executáveis de aplicativo diferentes.

```json
{
  "applications": [
    {
      "id": "Sample",
      "executable": "Sample.exe",
      "workingDirectory": "",
      "stopOnScriptError": false,
      "startScript":
      {
        "scriptPath": "RunMePlease.ps1",
        "scriptArguments": "\\\"First argument\\\" secondArgument",
        "runInVirtualEnvironment": true,
        "showWindow": true,
        "waitForScriptToFinish": false
      },
      "endScript":
      {
        "scriptPath": "RunMeAfter.ps1",
        "scriptArguments": "ThisIsMe.txt"
      }
    },
    {
      "id": "CPPSample",
      "executable": "CPPSample.exe",
      "workingDirectory": "",
      "startScript":
      {
        "scriptPath": "CPPStart.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runInVirtualEnvironment": true
      },
      "endScript":
      {
        "scriptPath": "CPPEnd.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runOnce": false
      }
    }
  ],
  "processes": [
    ...(taken out for brevity)
  ]
}
```
