---
description: Se você tiver um projeto de empacotamento no Visual estúdios e quiser aplicar uma estrutura de suporte de pacote
title: Aplicar o Package Support Framework no Visual Studio
ms.date: 05/14/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1f73c4512d79b1eb877da87c2422b49431bda3d
ms.sourcegitcommit: 4ecff6f1386c6239cfd79ddf82265f4194302bbb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92686873"
---
# <a name="apply-package-support-framework-in-visual-studio"></a>Aplicar o Package Support Framework no Visual Studio
A estrutura de suporte do pacote (PSF) é um [projeto](https://github.com/Microsoft/MSIX-PackageSupportFramework/) de software livre que permite que você aplique correções ao seu aplicativo de área de trabalho existente. O PSF permite que um aplicativo seja executado em um formato de pacote MSIX sem modificar o código. O Package Support Framework ajuda seu aplicativo a seguir as melhores práticas do ambiente moderno do runtime.

Nas seções a seguir, exploraremos como criar um novo projeto do Visual Studio, incluir a estrutura de suporte do pacote para a solução e criar correções de tempo de execução.

## <a name="step-1-create-a-package-solution-in-visual-studio"></a>Etapa 1: criar uma solução de pacote no Visual Studio
No Visual Studio, crie uma nova **solução de soluções do Visual Studio, em branco** . Inclua todos os projetos de aplicativo para a **solução em branco** recém-criada. 

## <a name="step-2-add-a-packaging-project"></a>Etapa 2: adicionar um projeto de empacotamento

Se você ainda não tiver um **projeto de empacotamento de aplicativos do Windows** , crie um e adicione-o à sua solução. Crie um novo **projeto de empacotamento de aplicativo** do Windows Universal-> do Windows > e adicione-o à sua solução recém-criada.

Para obter mais informações sobre o projeto de empacotamento de aplicativos do Windows, consulte [empacotar seu aplicativo usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

Em **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de empacotamento, selecione **Editar arquivo de projeto** e, em seguida, adicione-o à parte inferior do arquivo de projeto:

```xml
...
  <Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
    <ItemGroup>
      <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
      <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='_Runtime fix project name_'" />
      </FilteredNonWapProjProjectOutput>
      <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
      <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
    </ItemGroup>
  </Target>
</Project>
```

## <a name="step-3-add-project-for-the-runtime-fix"></a>Etapa 3: Adicionar o projeto para a correção de tempo de execução

Adicione um novo projeto de **Visual C++-> área de trabalho do Windows > biblioteca de Dynamic-Link (DLL)** à solução.

Em seguida, clique com o botão direito do mouse no projeto e escolha **Propriedades** .

Na página de propriedades, localize as **Propriedades de configuração – > C/C++-> linguagem-> o campo padrão de linguagem c++** . Em seguida, selecione ISO C++ 17 Standard (/std: c++ 17) no menu suspenso.

Clique com o botão direito do mouse no projeto e, no menu de contexto, escolha a opção **gerenciar pacotes NuGet** . Verifique se a opção **origem do pacote** está definida como **All** ou **NuGet.org** .

Clique no ícone de configurações próximo ao campo.

Pesquise os pacotes NuGet para **PSF** e, em seguida, instale o **Microsoft. PackageSupportFramework** para este projeto.

![pacote nuget](images/psf-package.png)

## <a name="step-4-add-a-project-that-starts-the-psf-launcher-executable"></a>Etapa 4: adicionar um projeto que inicia o executável do iniciador PSF
Adicione um novo **projeto Visual C++-> geral-> vazio** à solução.

Execute as seguintes etapas: 
1. Clique com o botão direito do mouse no projeto e, no menu de contexto, escolha a opção **gerenciar pacotes NuGet** . Verifique se a opção **origem do pacote** está definida como **All** ou **NuGet.org** .
1. Clique no ícone de configurações próximo ao campo.
1. Pesquise os pacotes NuGet para PSF e, em seguida, instale o Microsoft. PackageSupportFramework para este projeto.

Abra as **páginas de propriedades** do projeto e, na página configurações **gerais** , defina a propriedade **nome de destino** como ``PSFLauncher32`` ou ``PSFLauncher64`` dependendo da arquitetura do seu aplicativo.

Adicione uma referência de projeto ao projeto de correção de tempo de execução em sua solução.

Clique com o botão direito do mouse na referência e, em seguida, na janela **Propriedades** , aplique esses valores.

| Propriedade | Valor |
|-------|-----------|
| Copiar local | Verdadeiro |
| Assemblies Satélite do Local da Cópia | Verdadeiro |
| Saída do Assembly de Referência | Verdadeiro |
| Dependências da Biblioteca de Links | Falso |
| Entradas de dependência da biblioteca de links | Falso |

## <a name="step-5-configure-the-packaging-project"></a>Etapa 5: configurar o projeto de empacotamento

Para configurar o projeto de empacotamento, execute as seguintes etapas: 
1. No projeto de empacotamento, clique com o botão direito do mouse na pasta **aplicativos** e escolha **Adicionar referência** no menu suspenso.
1. Escolha o projeto do iniciador PSF e o projeto de aplicativo da área de trabalho e escolha o botão **OK** .
1. Selecione o **iniciador PSF** e o projeto de **aplicativo da área de trabalho** e clique no botão OK. Se o código-fonte do aplicativo não estiver disponível, selecione somente o projeto iniciador PSF.
1. No nó **aplicativos** , clique com o botão direito do mouse no aplicativo iniciador PSF e escolha **definir como ponto de entrada** .

Adicione um arquivo chamado ``config.json`` ao seu projeto de empacotamento e copie e cole o texto JSON a seguir no arquivo. Defina a propriedade **ação do pacote** como **conteúdo** .

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```

Forneça um valor para cada chave. Use essa tabela como um guia.

| Array | chave | Valor |
|-------|-----------|-------|
| de dimensionamento da Web | id |  Use o valor do `Id` atributo do `Application` elemento no manifesto do pacote. |
| de dimensionamento da Web | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do arquivo de manifesto do pacote antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| de dimensionamento da Web | workingDirectory | Adicional Um caminho relativo de pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usará o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome do `executable` configurado acima com o caminho e a extensão de arquivo removidos. |
| ajustes | dll | Caminho relativo do pacote para o DLL de correção a ser carregado. |
| ajustes | config | Adicional Controla como a DLL de correção se comporta. O formato exato desse valor varia de acordo com a correção, pois cada correção pode interpretar esse "blob" como desejado. |

Quando terminar, o ``config.json`` arquivo terá uma aparência semelhante a esta.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> As `applications` `processes` chaves, e `fixups` são matrizes. Isso significa que você pode usar a config.jsno arquivo para especificar mais de um aplicativo, processo e DLL de correção.

### <a name="debug-a-runtime-fix"></a>Depurar uma correção de tempo de execução

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que inicia é o aplicativo iniciador PSF, que, por sua vez, inicia o aplicativo de área de trabalho de destino.  Para depurar o aplicativo de área de trabalho de destino, você precisará anexar manualmente ao processo de aplicativo da área de trabalho escolhendo **debug->anexar ao processo** e, em seguida, selecionando o processo do aplicativo. Para permitir a depuração de um aplicativo .NET com uma DLL de correção de tempo de execução nativa, selecione tipos de código gerenciados e nativos (depuração de modo misto).  

Você pode definir pontos de interrupção ao lado das linhas de código no código do aplicativo da área de trabalho e no projeto de correção de tempo de execução. Se você não tiver o código-fonte para seu aplicativo, poderá definir pontos de interrupção somente ao lado das linhas de código em seu projeto de correção de tempo de execução.
