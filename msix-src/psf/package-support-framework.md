---
description: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
title: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
ms.date: 05/14/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c6526587d94af34b6c5c7df9702f0ab19e64e095
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091254"
---
# <a name="get-started-with-package-support-framework"></a>Introdução à estrutura de suporte do pacote 

O [Package support Framework](package-support-framework-overview.md) é um Open Source kit que ajuda a aplicar correções ao seu aplicativo de área de trabalho existente (sem modificar o código) para que ele possa ser executado em um contêiner MSIX. O Package Support Framework ajuda seu aplicativo a seguir as melhores práticas do ambiente moderno do runtime.

Este artigo fornece uma visão aprofundada de cada componente da estrutura de suporte do pacote e guia passo a passo para usá-lo.

## <a name="understand-what-is-inside-a-package-support-framework"></a>Entender o que está dentro de uma estrutura de suporte de pacote

O Package Support Framework contém um executável, uma DLL do gerenciador de runtime e um conjunto de correções em runtime.

![PSF (estrutura de suporte do pacote)](images/package-support-framework.png)

Este é o processo: 
1. Crie um arquivo de configuração que especifique as correções que você deseja aplicar ao seu aplicativo. 
1. Modifique seu pacote para apontar para o arquivo executável do iniciador da estrutura de suporte do pacote (PSF).

Quando os usuários iniciam seu aplicativo, o iniciador da estrutura de suporte do pacote é o primeiro executável que é executado. Ele lê o arquivo de configuração e injeta as correções em runtime e a DLL do gerenciador de runtime no processo do aplicativo. O gerenciador de runtime aplica a correção quando ela é necessária para que o aplicativo seja executado em um contêiner MSIX.

![Injeção de DLL do Package Support Framework](images/package-support-framework-2.png)

## <a name="step-1-identify-packaged-application-compatibility-issues"></a>Etapa 1: identificar problemas de compatibilidade de aplicativos empacotados

Primeiro, crie um pacote para seu aplicativo. Em seguida, instale-o, execute-o e observe seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Use também o [Monitor do Processo](/sysinternals/downloads/procmon) para identificar problemas.  Problemas comuns relacionados a pressuposições de aplicativo em relação às permissões de diretório de trabalho e caminho do programa.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usando o Process Monitor para identificar um problema

O [Process Monitor](/sysinternals/downloads/procmon) é um utilitário poderoso para observar as operações de registro e arquivo de um aplicativo e seus resultados.  Isso pode ajudá-lo a entender os problemas de compatibilidade de aplicativos.  Depois de abrir o Process Monitor, adicione um filtro (filtro > filtro...) para incluir somente os eventos do executável do aplicativo.

![Filtro de aplicativo ProcMon](images/procmon_app_filter.png)

Uma lista de eventos será exibida. Para muitos desses eventos, a palavra **Success** será exibida na coluna **Result** .

![Eventos de ProcMon](images/procmon_events.png)

Opcionalmente, você pode filtrar eventos para mostrar apenas as falhas.

![ProcMon excluir êxito](images/procmon_exclude_success.png)

Se você suspeitar de uma falha de acesso ao sistema de arquivos, Procure eventos com falha que estejam sob o caminho de arquivo system32/SysWOW64 ou pacote. Os filtros também podem ajudar aqui. Inicie na parte inferior desta lista e role para cima. As falhas que aparecem na parte inferior desta lista ocorreram mais recentemente. Preste mais atenção aos erros que contêm cadeias de caracteres como "acesso negado" e "caminho/nome não encontrado" e ignore as coisas que não parecem suspeitas. O [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tem dois problemas. Você pode ver esses problemas na lista que aparece na imagem a seguir.

![Config.txt ProcMon](images/procmon_config_txt.png)

No primeiro problema que aparece nessa imagem, o aplicativo está falhando ao ler do arquivo "Config.txt" que está localizado no caminho "C:\Windows\SysWOW64". É improvável que o aplicativo esteja tentando referenciar esse caminho diretamente. Provavelmente, ele está tentando ler a partir desse arquivo usando um caminho relativo e, por padrão, "system32/SysWOW64" é o diretório de trabalho do aplicativo. Isso sugere que o aplicativo está esperando que seu diretório de trabalho atual seja definido como algum lugar no pacote. Olhando dentro do Appx, podemos ver que o arquivo existe no mesmo diretório que o executável.

![Config.txt do aplicativo](images/psfsampleapp_config_txt.png)

O segundo problema aparece na imagem a seguir.

![Arquivo de log ProcMon](images/procmon_logfile.png)

Nesse problema, o aplicativo está falhando em gravar um arquivo. log em seu caminho de pacote. Isso sugere que uma correção de redirecionamento de arquivo pode ajudar.

<a id="find"></a>

## <a name="step-2-find-a-runtime-fix"></a>Etapa 2: encontrar uma correção de tempo de execução

O PSF contém correções de tempo de execução que você pode usar no momento, como a correção de redirecionamento de arquivo.

### <a name="file-redirection-fixup"></a>Correção de redirecionamento de arquivo

Você pode usar a [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para redirecionar tentativas de gravação ou leitura de dados em um diretório que não está acessível a partir de um aplicativo que é executado em um contêiner MSIX.

Por exemplo, se seu aplicativo gravar em um arquivo de log que está no mesmo diretório que o executável de seus aplicativos, você poderá usar a [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para criar esse arquivo de log em outro local, como o armazenamento de dados do aplicativo local.

### <a name="runtime-fixes-from-the-community"></a>Correções de tempo de execução da Comunidade

Certifique-se de examinar as contribuições da Comunidade em nossa página do [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) . É possível que outros desenvolvedores tenham resolvido um problema semelhante ao seu e tenham compartilhado uma correção de tempo de execução.

## <a name="step-3-apply-a-runtime-fix"></a>Etapa 3: aplicar uma correção de tempo de execução

Você pode aplicar uma correção de tempo de execução existente com algumas ferramentas simples do SDK do Windows e seguindo estas etapas.

> [!div class="checklist"]
> * Criar uma pasta de layout de pacote
> * Obter os arquivos da estrutura de suporte do pacote
> * Adicioná-los ao seu pacote
> * Modificar o manifesto do pacote
> * Criar um arquivo de configuração

Vamos examinar cada tarefa.

### <a name="create-the-package-layout-folder"></a>Criar a pasta de layout do pacote

Se você já tiver um arquivo. msix (ou. AppX), poderá desempacotar seu conteúdo em uma pasta de layout que servirá como a área de preparo para seu pacote. Você pode fazer isso em um prompt de comando usando a ferramenta MakeAppx, com base no caminho de instalação do SDK, é aqui que você encontrará a ferramenta de makeappx.exe em seu PC com Windows 10: x86: C:\Arquivos de programas (x86) \Windows Kits\10\bin\x86\makeappx.exe x64: C:\Arquivos de programas (x86) \Windows Kits\10\bin\x64\makeappx.exe

```powershell
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Isso fornecerá algo parecido com o seguinte.

![Layout do pacote](images/package_contents.png)

Se você não tiver um arquivo. msix (ou. AppX) para começar, poderá criar a pasta e os arquivos do pacote do zero.

### <a name="get-the-package-support-framework-files"></a>Obter os arquivos da estrutura de suporte do pacote

Você pode obter o pacote NuGet do PSF usando a ferramenta de linha de comando do NuGet autônomo ou por meio do Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obter o pacote usando a ferramenta de linha de comando

Instale a ferramenta de linha de comando do NuGet deste local: https://www.nuget.org/downloads . Em seguida, na linha de comando do NuGet, execute este comando: 

```powershell
nuget install Microsoft.PackageSupportFramework
```

Como alternativa, você pode renomear a extensão do pacote para. zip e descompactá-la. Todos os arquivos de que você precisa estarão na pasta/bin.

#### <a name="get-the-package-by-using-visual-studio"></a>Obter o pacote usando o Visual Studio

No Visual Studio, clique com o botão direito do mouse no nó da sua solução ou do projeto e escolha um dos comandos gerenciar pacotes NuGet.  Procure **Microsoft. PackageSupportFramework** ou **PSF** para localizar o pacote em NuGet.org. Em seguida, instale-o.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Adicionar os arquivos da estrutura de suporte do pacote ao seu pacote

Adicione as DLLs de PSF e arquivos executáveis de 32 bits e de 64 bits necessários ao diretório do pacote. Use a tabela a seguir como guia. Você também desejará incluir correções de tempo de execução necessárias. Em nosso exemplo, precisamos da correção de tempo de execução de redirecionamento de arquivo.

| O executável do aplicativo é x64 | O executável do aplicativo é x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

O conteúdo do pacote agora deve ser semelhante a este.

![Binários de pacote](images/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

Abra o manifesto do pacote em um editor de texto e defina o `Executable` atributo do `Application` elemento como o nome do arquivo executável do iniciador PSF.  Se você souber a arquitetura do seu aplicativo de destino, selecione a versão apropriada, PSFLauncher32.exe ou PSFLauncher64.exe.  Se não, PSFLauncher32.exe funcionará em todos os casos.  Veja um exemplo.

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

### <a name="create-a-configuration-file"></a>Criar um arquivo de configuração

Crie um nome de arquivo ``config.json`` e salve esse arquivo na pasta raiz do seu pacote. Modifique a ID de aplicativo declarada do config.jsno arquivo para apontar para o executável que você acabou de substituir. Usando o conhecimento obtido usando o Process Monitor, você também pode definir o diretório de trabalho, bem como usar a correção de redirecionamento de arquivo para redirecionar leituras/gravações para arquivos. log no diretório "PSFSampleApp" relativo ao pacote.

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```

Veja a seguir um guia para o config.jsno esquema:

| Array | chave | Valor |
|-------|-----------|-------|
| de dimensionamento da Web | id |  Use o valor do `Id` atributo do `Application` elemento no manifesto do pacote. |
| de dimensionamento da Web | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do arquivo de manifesto do pacote antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| de dimensionamento da Web | workingDirectory | Adicional Um caminho relativo de pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usará o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome do `executable` configurado acima com o caminho e a extensão de arquivo removidos. |
| ajustes | dll | Caminho relativo ao pacote para a correção,. msix/. Appx a ser carregado. |
| ajustes | config | Adicional Controla como a DLL de correção se comporta. O formato exato desse valor varia de acordo com a correção, pois cada correção pode interpretar esse "blob" como desejado. |

As `applications` `processes` chaves, e `fixups` são matrizes. Isso significa que você pode usar a config.jsno arquivo para especificar mais de um aplicativo, processo e DLL de correção.

### <a name="package-and-test-the-app"></a>Empacotar e testar o aplicativo

Em seguida, crie um pacote.

```powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Em seguida, assine-o.

```powershell
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obter mais informações, consulte [como criar um certificado de assinatura de pacote](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) e [como assinar um pacote usando Signtool](/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Usando o PowerShell, instale o pacote.

>[!NOTE]
> Lembre-se de desinstalar o pacote primeiro.

```powershell
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Execute o aplicativo e observe o comportamento com a correção de tempo de execução aplicada.  Repita as etapas de diagnóstico e empacotamento conforme necessário.

### <a name="check-whether-the-package-support-framework-is-running"></a>Verificar se a estrutura de suporte do pacote está em execução 

Você pode verificar se a correção de tempo de execução está em execução. Uma maneira de fazer isso é abrir o **Gerenciador de tarefas** e clicar em **mais detalhes**. Localize o aplicativo ao qual a estrutura de suporte do pacote foi aplicada e expanda os detalhes do aplicativo para veiw mais detalhes. Você deve ser capaz de exibir se a estrutura de suporte do pacote está em execução. 

### <a name="use-the-trace-fixup"></a>Usar a correção de rastreamento

Uma técnica alternativa para diagnosticar problemas de compatibilidade de aplicativos empacotados é usar a correção de rastreamento. Essa DLL está incluída no PSF e fornece uma exibição de diagnóstico detalhada do comportamento do aplicativo, semelhante ao Process Monitor.  Ele é especialmente projetado para revelar problemas de compatibilidade de aplicativos.  Para usar a correção de rastreamento, adicione a DLL ao pacote, adicione o fragmento a seguir ao seu config.jsativado e, em seguida, empacote e instale o aplicativo.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Por padrão, a correção de rastreamento filtra falhas que podem ser consideradas "esperadas".  Por exemplo, os aplicativos podem tentar excluir um arquivo incondicionalmente sem verificar se ele já existe, ignorando o resultado. Isso tem a conseqüência de que algumas falhas inesperadas podem ser filtradas, portanto, no exemplo acima, optamos por receber todas as falhas das funções FileSystem. Fazemos isso porque sabemos antes que a tentativa de ler a partir do arquivo de Config.txt falha com a mensagem "arquivo não encontrado". Essa é uma falha que é frequentemente observada e, geralmente, não é presumida como inesperada. Na prática, é provável que seja melhor começar a filtragem apenas para falhas inesperadas e, em seguida, voltar a todas as falhas se houver um problema que ainda não possa ser identificado.

Por padrão, a saída da correção de rastreamento é enviada para o depurador anexado. Para este exemplo, não vamos anexar um depurador e, em vez disso, usaremos o programa [DebugView](/sysinternals/downloads/debugview) da Sysinternals para exibir sua saída. Depois de executar o aplicativo, podemos ver as mesmas falhas que antes, o que nos indicaria nas mesmas correções de tempo de execução.

![Arquivo TraceShim não encontrado](images/traceshim_filenotfound.png)

![Acesso negado ao TraceShim](images/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, estender ou criar uma correção de tempo de execução

Você pode usar o Visual Studio para depurar uma correção de tempo de execução, estender uma correção de tempo de execução ou criar uma do zero. Você precisará fazer essas coisas para ter êxito.

> [!div class="checklist"]
> * Adicionar um projeto de empacotamento
> * Adicionar projeto para a correção de tempo de execução
> * Adicionar um projeto que inicia o executável do iniciador PSF
> * Configurar o projeto de empacotamento

Quando terminar, sua solução terá uma aparência semelhante a esta.

![Solução concluída](images/runtime-fix-project-structure.png)

Vamos examinar cada projeto neste exemplo.

| Project | Finalidade |
|-------|-----------|
| DesktopApplicationPackage | Este projeto é baseado no [projeto de empacotamento de aplicativos do Windows](../desktop/desktop-to-uwp-packaging-dot-net.md) e gera o pacote MSIX. |
| Runtimefix | Este é um projeto de biblioteca de vínculo dinâmico C++ que contém uma ou mais funções de substituição que servem como correção de tempo de execução. |
| PSFLauncher | Este é um projeto vazio do C++. Este projeto é um local para coletar os arquivos distribuíveis de tempo de execução da estrutura de suporte do pacote. Ele gera um arquivo executável. Esse executável é a primeira coisa que é executada quando você inicia a solução. |
| WinFormsDesktopApplication | Este projeto contém o código-fonte de um aplicativo de área de trabalho. |

Para examinar um exemplo completo que contém todos esses tipos de projetos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos percorrer as etapas para criar e configurar cada um desses projetos em sua solução.

### <a name="create-a-package-solution"></a>Criar uma solução de pacote

Se você ainda não tiver uma solução para seu aplicativo de área de trabalho, crie uma nova **solução em branco** no Visual Studio.

![Solução em branco](images/blank-solution.png)

Talvez você também queira adicionar qualquer projeto de aplicativo que você tenha.

### <a name="add-a-packaging-project"></a>Adicionar um projeto de empacotamento

Se você ainda não tiver um **projeto de empacotamento de aplicativos do Windows**, crie um e adicione-o à sua solução.

![Modelo de projeto de pacote](images/package-project-template.png)

Para obter mais informações sobre o projeto de empacotamento de aplicativos do Windows, consulte [empacotar seu aplicativo usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de empacotamento, selecione **Editar**e, em seguida, adicione-o à parte inferior do arquivo de projeto:

```xml
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Adicionar projeto para a correção de tempo de execução

Adicione um projeto de **dll (biblioteca de vínculo dinâmico)** do C++ à solução.

![Biblioteca de correção de tempo de execução](images/runtime-fix-library.png)

Clique com o botão direito do mouse no projeto e escolha **Propriedades**.

Nas páginas de propriedades, encontre o campo **padrão da linguagem C++** e, em seguida, na lista suspensa ao lado desse campo, selecione a opção **ISO C++ 17 Standard (/std: C++ 17)** .

![Opção ISO 17](images/iso-option.png)

Clique com o botão direito do mouse no projeto e, no menu de contexto, escolha a opção **gerenciar pacotes NuGet** . Verifique se a opção **origem do pacote** está definida como **All** ou **NuGet.org**.

Clique no ícone de configurações próximo ao campo.

Procure o pacote NuGet *PSF** e instale-o para este projeto.

![pacote nuget](images/psf-package.png)

Se você quiser depurar ou estender uma correção de tempo de execução existente, adicione os arquivos de correção de tempo de execução obtidos usando as diretrizes descritas na seção [encontrar uma correção de tempo de execução](#find) deste guia.

Se você pretende criar uma correção totalmente nova, não adicione nada a esse projeto ainda. Ajudaremos você a adicionar os arquivos corretos a esse projeto posteriormente neste guia. Por enquanto, continuaremos Configurando sua solução.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Adicionar um projeto que inicia o executável do iniciador PSF

Adicione um projeto de **projeto vazio** do C++ à solução.

![Projeto vazio](images/blank-app.png)

Adicione o pacote NuGet do **PSF** a este projeto usando as mesmas diretrizes descritas na seção anterior.

Abra as páginas de propriedades do projeto e, na página configurações **gerais** , defina a propriedade **nome de destino** como ``PSFLauncher32`` ou ``PSFLauncher64`` dependendo da arquitetura do seu aplicativo.

![Referência do iniciador PSF](images/shim-exe-reference.png)

Adicione uma referência de projeto ao projeto de correção de tempo de execução em sua solução.

![referência de correção de tempo de execução](images/reference-fix.png)

Clique com o botão direito do mouse na referência e, em seguida, na janela **Propriedades** , aplique esses valores.

| Propriedade | Valor |
|-------|-----------|
| Copiar local | Verdadeiro |
| Assemblies Satélite do Local da Cópia | Verdadeiro |
| Saída do Assembly de Referência | Verdadeiro |
| Dependências da Biblioteca de Links | Falso |
| Entradas de dependência da biblioteca de links | Falso |

### <a name="configure-the-packaging-project"></a>Configurar o projeto de empacotamento

No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

![Adicionar Referência de Projeto](images/add-reference-packaging-project.png)

Escolha o projeto do iniciador PSF e o projeto de aplicativo da área de trabalho e escolha o botão **OK** .

![Projeto de área de trabalho](images/package-project-references.png)

>[!NOTE]
> Se você não tiver o código-fonte para seu aplicativo, basta escolher o projeto iniciador PSF. Mostraremos como fazer referência ao seu executável ao criar um arquivo de configuração.

No nó **aplicativos** , clique com o botão direito do mouse no aplicativo iniciador PSF e escolha **definir como ponto de entrada**.

![Definir ponto de entrada](images/set-startup-project.png)

Adicione um arquivo chamado ``config.json`` ao seu projeto de empacotamento e copie e cole o texto JSON a seguir no arquivo. Defina a propriedade **ação do pacote** como **conteúdo**.

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

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que inicia é o aplicativo iniciador PSF, que, por sua vez, inicia o aplicativo de área de trabalho de destino.  Para depurar o aplicativo de área de trabalho de destino, você precisará anexar manualmente ao processo de aplicativo da área de trabalho escolhendo **debug->anexar ao processo**e, em seguida, selecionando o processo do aplicativo. Para permitir a depuração de um aplicativo .NET com uma DLL de correção de tempo de execução nativa, selecione tipos de código gerenciados e nativos (depuração de modo misto).  

Depois de configurar isso, você pode definir pontos de interrupção ao lado das linhas de código no código do aplicativo da área de trabalho e no projeto de correção de tempo de execução. Se você não tiver o código-fonte para seu aplicativo, poderá definir pontos de interrupção somente ao lado das linhas de código em seu projeto de correção de tempo de execução.

Como a depuração F5 executa o aplicativo implantando arquivos soltos do caminho da pasta de layout do pacote, em vez de instalar de um pacote. msix/. Appx, a pasta de layout normalmente não tem as mesmas restrições de segurança que uma pasta de pacote instalada. Como resultado, talvez não seja possível reproduzir os erros de negação de acesso ao caminho do pacote antes de aplicar uma correção de tempo de execução.

Para resolver esse problema, use a implantação de pacote. msix/. Appx em vez de F5 implantação de arquivo flexível.  Para criar um arquivo de pacote. msix/. Appx, use o utilitário [MakeAppx](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) do SDK do Windows, conforme descrito acima. Ou, no Visual Studio, clique com o botão direito do mouse no nó do projeto de aplicativo e selecione **Store-> criar pacotes de aplicativos**.

Outro problema com o Visual Studio é que ele não tem suporte interno para anexar a processos filho iniciados pelo depurador.   Isso dificulta a depuração da lógica no caminho de inicialização do aplicativo de destino, que deve ser anexado manualmente pelo Visual Studio após a inicialização.

Para resolver esse problema, use um depurador que dá suporte à anexação de processo filho.  Observe que geralmente não é possível anexar um depurador JIT (just-in-time) ao aplicativo de destino.  Isso ocorre porque a maioria das técnicas de JIT envolve a inicialização do depurador no lugar do aplicativo de destino, por meio da chave do registro ImageFileExecutionOptions.  Isso anula o mecanismo de desvio usado pelo PSFLauncher.exe para injetar FixupRuntime.dll no aplicativo de destino.  O WinDbg, incluído nas [ferramentas de depuração para Windows](/windows-hardware/drivers/debugger/index), e obtido do [SDK do Windows](https://developer.microsoft.com/windows/downloads/windows-10-sdk), dá suporte à anexação do processo filho.  Agora, ele também dá suporte à [inicialização e à depuração direta de um aplicativo UWP](/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar a inicialização do aplicativo de destino como um processo filho, inicie ``WinDbg`` .

```powershell
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

No ``WinDbg`` prompt, habilite a depuração de filho e defina os pontos de interrupção apropriados.

```powershell
.childdbg 1
g
```

(execute até que o aplicativo de destino inicie e quebre no depurador)

```powershell
sxe ld fixup.dll
g
```

(execute até que a DLL de correção seja carregada)

```powershell
bp ...
```

>[!NOTE]
> O [o plmdebug](/windows-hardware/drivers/debugger/plmdebug) também pode ser usado para anexar um depurador a um aplicativo na inicialização e também está incluído nas [ferramentas de depuração para Windows](/windows-hardware/drivers/debugger/index).  No entanto, é mais complexo usar do que o suporte direto agora fornecido pelo WinDbg.

## <a name="support"></a>Suporte

Tem dúvidas? Pergunte-nos sobre o [pacote de suporte de estrutura](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support) de conversa do Framework no site da Comunidade do MSIX Tech.
