---
Description: Você pode executar scripts com a estrutura de suporte do pacote para personalizar seu aplicativo de área de trabalho para o ambiente do usuário.
title: Executar scripts com a estrutura de suporte do pacote
ms.date: 09/20/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 886255fc6e376dc2bf8f166857f6fe5d4ee9ffd9
ms.sourcegitcommit: cc7fe74ea7c7b8c06190330023b3dff43034960e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310994"
---
# <a name="run-scripts-with-the-package-support-framework"></a>Executar scripts com a estrutura de suporte do pacote

Se você identificar problemas de compatibilidade com seu aplicativo depois de criar um pacote MSIX para ele, poderá determinar que alguns desses problemas podem ser resolvidos executando scripts para personalizar dinamicamente um aplicativo para o ambiente do usuário. Por exemplo, esses scripts podem alterar as chaves do registro ou executar modificações de arquivo com base na configuração do computador ou do servidor.

Você pode usar a estrutura de suporte do pacote (PSF) para executar um script do PowerShell antes que um executável do aplicativo empacotado seja executado e um script do PowerShell após a execução do executável do aplicativo. Cada executável de aplicativo definido no manifesto do aplicativo pode ter seus próprios scripts.

## <a name="prerequisites"></a>Pré-requisitos

Para permitir que os scripts sejam executados, você precisa definir a política de execução `Unrestricted` do `RemoteSigned`PowerShell como ou. Você pode fazer isso executando um destes comandos:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

A política de execução precisa ser definida para o executável do PowerShell de 64 bits e o executável do PowerShell de 32 bits. Certifique-se de abrir cada versão do PowerShell e execute um dos comandos mostrados acima.

Aqui estão os locais de cada executável.

* Computador de 64 bits:
  * executável de 64 bits:%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
  * executável de 32 bits:%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* computador de 32 bits:
  * executável de 32 bits:%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe

Para obter mais informações sobre as políticas de execução do PowerShell, consulte [Este artigo](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6).

## <a name="enable-scripts"></a>Habilitar scripts

Para especificar quais scripts serão executados para cada executável de aplicativo empacotado, você precisa modificar o [arquivo config. JSON](package-support-framework.md#create-a-configuration-file). Para informar ao PSF para executar um script antes da execução do aplicativo empacotado, adicione um item de configuração `startScript`chamado. Para informar ao PSF para executar um script depois que o aplicativo empacotado terminar, adicione um `endScript`item de configuração chamado.

### <a name="script-configuration-items"></a>Itens de configuração de script

A seguir estão os itens de configuração disponíveis para os scripts. O script final ignora os itens `waitForScriptToFinish` de `stopOnScriptError` configuração e.

| Nome da chave                | Tipo de valor | Necessário? | Padrão  | Descrição
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | cadeia de caracteres     | Sim       | N/D      | O caminho para o script, incluindo o nome e a extensão. O caminho é iniciado a partir do diretório raiz do aplicativo.
| `scriptArguments`         | cadeia de caracteres     | Não        | vazio    | Lista de argumentos delimitados por espaço. O formato é o mesmo para uma chamada de script do PowerShell. Essa cadeia de caracteres é anexada a `scriptPath` para fazer uma chamada PowerShell. exe válida.
| `runInVirtualEnvironment` | boolean    | Não        | true     | Especifica se o script deve ser executado no mesmo ambiente virtual em que o aplicativo empacotado é executado.
| `runOnce`                 | boolean    | Não        | true     | Especifica se o script deve ser executado uma vez por usuário, por versão.
| `showWindow`              | boolean    | Não        | false    | Especifica se a janela do PowerShell é mostrada.
| `stopOnScriptError`       | boolean    | Não        | false    | Especifica se deseja sair do aplicativo se o script inicial falhar.
| `waitForScriptToFinish`   | boolean    | Não        | true     | Especifica se o aplicativo empacotado deve aguardar a conclusão do script inicial antes de iniciar.
| `timeout`                 | DWORD      | Não        | LOOP | Quanto tempo o script terá permissão para ser executado. Quando o tempo decorrido, o script será interrompido.

> [!NOTE]
> Não `stopOnScriptError: true` há `waitForScriptToFinish: false` suporte para a configuração e para o aplicativo de exemplo. Se você definir esses dois itens de configuração, PSF retornará o erro ERROR_BAD_CONFIGURATION.


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
