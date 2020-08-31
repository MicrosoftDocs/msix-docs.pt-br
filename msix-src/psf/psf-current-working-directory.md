---
description: A estrutura de suporte do pacote ajuda a trabalhar com o diretório de trabalho.
title: Diretório de trabalho atual do Package support Framework
ms.date: 05/14/2020
ms.topic: article
keywords: Windows 10, UWP, msix, PSF
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8ef43fa5464fe8e3b6a6f264c8edf25d7fdb6bff
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090704"
---
# <a name="current-working-directory-psf"></a>Diretório de trabalho atual do PSF
A estrutura de suporte do pacote usa um config.jsno arquivo para configurar o comportamento de um aplicativo.

## <a name="proceedure"></a>Proceedure
Para configurar a execução do aplicativo para receber valores de parâmetro passados para ele durante a inicialização do aplicativo, as etapas a seguir devem ser concluídas. 

1. Baixar a estrutura de suporte do pacote
1. Configurar a estrutura de suporte do pacote (config.jsem)
1. Injetar os arquivos da estrutura de suporte do pacote no pacote de aplicativos
1. Atualizar o manifesto do aplicativo
1. Reempacotar o aplicativo

## <a name="download-the-package-support-framework"></a>Baixar a estrutura de suporte do pacote
A estrutura de suporte do pacote pode ser recuperada usando a ferramenta de linha de comando do NuGet autônoma ou por meio do Visual Studio.

### <a name="nuget-comandline-tool"></a>Ferramenta NuGet comandline:
Instale a ferramenta de linha de comando do NuGet do site do NuGet: [https://www.nuget.org/downloads](https://www.nuget.org/downloads) . Após a instalação, execute a linha de comando a seguir em uma janela administrativa do PowerShell.

``` powershell
nuget install Microsoft.PackageSupportFramework
```

### <a name="visual-studio"></a>Visual Studio:
No Visual Studio, clique com o botão direito do mouse no nó da solução/projeto e escolha um dos comandos gerenciar pacotes NuGet. Procure **Microsoft. PackageSupportFramework** ou **PSF** para localizar o pacote em NuGet.org. Em seguida, instale-o.


## <a name="create-the-configjson-file"></a>Criar o config.jsno arquivo

Para especificar os argumentos necessários para iniciar o aplicativo, você deve modificar o config.jsno arquivo especificando o executável do aplicativo e os parâmetros. Antes de configurar o config.jsno arquivo, examine a tabela a seguir para entender o strucuture JSON.

### <a name="json-schema"></a>Esquema JSON

|Array          | chave               | Valor  |
|---------------|-------------------|--------|
| de dimensionamento da Web  | id                | Use o valor do atributo ID do elemento Application no manifesto do pacote.|
| de dimensionamento da Web  | executável        | O caminho relativo do pacote para o executável que você deseja iniciar. É o valor do atributo executável do elemento Application. |
| de dimensionamento da Web  | argumentos         | Adicional Argumentos de parâmetro de linha de comando para o executável. |
| de dimensionamento da Web  | workingDirectory  | Adicional Um caminho relativo de pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usará o diretório system32 como o diretório de trabalho do aplicativo. Se você fornecer um valor na forma de uma cadeia de caracteres vazia, ele usará o diretório do executável referenciado. |
| de dimensionamento da Web  | monitor           | Adicional Se estiver presente, o monitor identificará um programa secundário que deve ser iniciado antes de iniciar o aplicativo primário. |
| processos     | executável        | Na maioria dos casos, esse será o nome do executável configurado acima com o caminho e a extensão de arquivo removidos. |
| ajustes        | dll               | Adicional Caminho relativo ao pacote para a correção,. msix/. Appx a ser carregado. |
| ajustes        | config            | Adicional Controla a forma com que a DL de correção se comporta. O formato exato desse valor varia de acordo com a correção, pois cada correção pode interpretar esse "blob" como desejado.|

As chaves aplicativos, processos e correções são matrizes. Isso significa que você pode usar a config.jsno arquivo para especificar mais de um aplicativo, processo e DLL de correção.

Uma entrada em processos tem um valor chamado executável, o valor para o qual deve ser formado como uma cadeia de caracteres RegEx para corresponder ao nome de um processo executável sem caminho ou extensão de arquivo. O iniciador esperará SDK do Windows sintaxe de ECMAlist de default:: library para a cadeia de caracteres RegEx.

``` json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PrimaryApp.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample"
        }
    ]
}
```

No exemplo acima config.jsno arquivo, o PrimaryApp.exe terá o diretório de trabalho Redirecionado para o diretório *PSFSampleApp*


## <a name="inject-the-package-support-framework-files-to-the-application-package"></a>Injetar os arquivos da estrutura de suporte do pacote no pacote de aplicativos

### <a name="create-the-package-staging-folder"></a>Criar a pasta de preparo do pacote
Se você já tiver um arquivo. msix (ou. AppX), poderá desempacotar seu conteúdo em uma pasta de layout que servirá como a área de preparo para seu pacote. Você pode fazer isso em um prompt de comando usando a ferramenta MakeAppx.

A ferramenta MakeAppx pode ser encontrada em seu local padrão:

| Arquitetura do SO | Diretório                                                   |
|-----------------|-------------------------------------------------------------|
| Windows 10 x86  | C:\Arquivos de Programas\windows Kits\10\bin \[ **versão**] \x86\makeappx.exe       |
| Windows 10 x64  | C:\Arquivos de programas (x86) \Windows Kits\10\bin \[ **versão**] \x64\makeappx.exe |

```powershell
makeappx unpack /p PrimaryApp.msix /d PackageContents
```

O comando do PowerShell acima vai exportar o conteúdo do aplicativo para um diretório local.

### <a name="inject-required-files"></a>Injetar arquivos necessários
Adicione as DLLs de estrutura de suporte de pacote de 32 bits e 64 bits necessárias e arquivos executáveis ao diretório do pacote. Use a tabela a seguir como guia, você também desejará incluir correções de tempo de execução conforme necessário. Visite o artigo [Package support Framework Runtime fixes](./package-support-framework.md) docs para obter orientação sobre como usar a estrutura de suporte do pacote para correções de tempo de execução.

| O executável do aplicativo é x64 | O executável do aplicativo é x86     |
|-------------------------------|-----------------------------------|
| PSFLauncher64.exe             | PSFLauncher32.exe                 |
| PSFRuntime64.dll              | PSFLauncher32.dll                 |
| PSFRunDll64.exe               | PSFRunDll32.exe                   |

Esses arquivos identificados acima, bem como o **config.jsno** arquivo, devem ser colocados dentro da raiz do diretório do pacote de aplicativos. Esses arquivos podem ser encontrados no diretório da estrutura de suporte do pacote instalado dentro do diretório **bin** .


## <a name="update-the-applications-manifest"></a>Atualizar o manifesto do aplicativo
Abra o manifesto de aplicativos em um editor de texto e, em seguida, defina o atributo executável do elemento de aplicativo como o nome do arquivo executável PSFLauncher. Se você souber a arquitetura do seu aplicativo de destino, selecione a versão apropriada, PSFLauncher [x64/x86]. exe. Se for desconhecido, PSFLauncher32.exe funcionará em todos os casos.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```


## <a name="re-package-the-application"></a>Reempacotar o aplicativo
Empacote novamente o aplicativo usando a ferramenta makeappx. Consulte o exemplo a seguir de como empacotar e assinar o aplicativo usando o PowerShell:

``` powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
signtool sign /a/v/fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```