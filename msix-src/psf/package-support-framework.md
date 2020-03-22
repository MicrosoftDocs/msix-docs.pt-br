---
Description: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
title: Corrigir problemas que impedem que o aplicativo da área de trabalho seja executado em um contêiner MSIX
ms.date: 08/07/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0c280cbff2d2151cb2a791165df9dd5f23d10c7d
ms.sourcegitcommit: e703ffe4c635d9b127ecf8c02e087370b676aa9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108179"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correções de tempo de execução a um pacote MSIX usando a estrutura de suporte do pacote

O [Package support Framework](package-support-framework-overview.md) é um Open Source kit que ajuda a aplicar correções ao seu aplicativo de área de trabalho existente (sem modificar o código) para que ele possa ser executado em um contêiner MSIX. O Package Support Framework ajuda seu aplicativo a seguir as melhores práticas do ambiente moderno do runtime.

Este artigo ajuda você a identificar problemas de compatibilidade de aplicativos e a localizar, aplicar e estender correções de tempo de execução que os resolvem. As seções deste artigo destinam-se a diferentes funções:

* Profissionais de ti ou administradores: as seções mais relevantes são [identificar problemas de compatibilidade de aplicativos empacotados](#identify-packaged-application-compatibility-issues), [encontrar uma correção de tempo de execução](#find-a-runtime-fix)e [aplicar uma correção de tempo de execução](#apply-a-runtime-fix).
* Desenvolvedores: embora os desenvolvedores possam encontrar o artigo inteiro útil, as seções mais relevantes são [depurar, estender ou criar uma correção de tempo de execução](#debug-extend-or-create-a-runtime-fix), [criar uma correção de tempo de execução](#create-a-runtime-fix)e [outras técnicas de depuração](#other-debugging-techniques). d

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidade de aplicativos empacotados

Primeiro, crie um pacote para seu aplicativo. Em seguida, instale-o, execute-o e observe seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Use também o [Monitor do Processo](https://docs.microsoft.com/sysinternals/downloads/procmon) para identificar problemas.  Problemas comuns relacionados a pressuposições de aplicativo em relação às permissões de diretório de trabalho e caminho do programa.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usando o Process Monitor para identificar um problema

O [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) é um utilitário poderoso para observar as operações de registro e arquivo de um aplicativo e seus resultados.  Isso pode ajudá-lo a entender os problemas de compatibilidade de aplicativos.  Depois de abrir o Process Monitor, adicione um filtro (filtro > filtro...) para incluir somente os eventos do executável do aplicativo.

![Filtro de aplicativo ProcMon](images/procmon_app_filter.png)

Uma lista de eventos será exibida. Para muitos desses eventos, a palavra **Success** será exibida na coluna **Result** .

![Eventos de ProcMon](images/procmon_events.png)

Opcionalmente, você pode filtrar eventos para mostrar apenas as falhas.

![ProcMon excluir êxito](images/procmon_exclude_success.png)

Se você suspeitar de uma falha de acesso ao sistema de arquivos, Procure eventos com falha que estejam sob o caminho de arquivo system32/SysWOW64 ou pacote. Os filtros também podem ajudar aqui. Inicie na parte inferior desta lista e role para cima. As falhas que aparecem na parte inferior desta lista ocorreram mais recentemente. Preste mais atenção aos erros que contêm cadeias de caracteres como "acesso negado" e "caminho/nome não encontrado" e ignore as coisas que não parecem suspeitas. O [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tem dois problemas. Você pode ver esses problemas na lista que aparece na imagem a seguir.

![ProcMon config. txt](images/procmon_config_txt.png)

No primeiro problema que aparece nessa imagem, o aplicativo está falhando ao ler do arquivo "config. txt" que está localizado no caminho "C:\Windows\SysWOW64". É improvável que o aplicativo esteja tentando referenciar esse caminho diretamente. Provavelmente, ele está tentando ler a partir desse arquivo usando um caminho relativo e, por padrão, "system32/SysWOW64" é o diretório de trabalho do aplicativo. Isso sugere que o aplicativo está esperando que seu diretório de trabalho atual seja definido como algum lugar no pacote. Olhando dentro do Appx, podemos ver que o arquivo existe no mesmo diretório que o executável.

![App config. txt](images/psfsampleapp_config_txt.png)

O segundo problema aparece na imagem a seguir.

![Arquivo de log ProcMon](images/procmon_logfile.png)

Nesse problema, o aplicativo está falhando em gravar um arquivo. log em seu caminho de pacote. Isso sugere que uma correção de redirecionamento de arquivo pode ajudar.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Encontrar uma correção de tempo de execução

O PSF contém correções de tempo de execução que você pode usar no momento, como a correção de redirecionamento de arquivo.

### <a name="file-redirection-fixup"></a>Correção de redirecionamento de arquivo

Você pode usar a [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para redirecionar tentativas de gravação ou leitura de dados em um diretório que não está acessível a partir de um aplicativo que é executado em um contêiner MSIX.

Por exemplo, se seu aplicativo gravar em um arquivo de log que está no mesmo diretório que o executável de seus aplicativos, você poderá usar a [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para criar esse arquivo de log em outro local, como o armazenamento de dados do aplicativo local.

### <a name="runtime-fixes-from-the-community"></a>Correções de tempo de execução da Comunidade

Certifique-se de examinar as contribuições da Comunidade em nossa página do [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) . É possível que outros desenvolvedores tenham resolvido um problema semelhante ao seu e tenham compartilhado uma correção de tempo de execução.

## <a name="apply-a-runtime-fix"></a>Aplicar uma correção de tempo de execução

Você pode aplicar uma correção de tempo de execução existente com algumas ferramentas simples do SDK do Windows e seguindo estas etapas.

> [!div class="checklist"]
> * Criar uma pasta de layout de pacote
> * Obter os arquivos da estrutura de suporte do pacote
> * Adicioná-los ao seu pacote
> * Modificar o manifesto do pacote
> * Criar um arquivo de configuração

Vamos examinar cada tarefa.

### <a name="create-the-package-layout-folder"></a>Criar a pasta de layout do pacote

Se você já tiver um arquivo. msix (ou. AppX), poderá desempacotar seu conteúdo em uma pasta de layout que servirá como a área de preparo para seu pacote. Você pode fazer isso em um prompt de comando usando a ferramenta MakeAppx, com base no caminho de instalação do SDK, é aqui que você encontrará a ferramenta MakeAppx. exe em seu PC com Windows 10: x86: C:\Arquivos de programas (x86) \Windows Kits\10\bin\x86\makeappx.exe x64: C:\Arquivos de programas ( x86) \Windows Kits\10\bin\x64\makeappx.exe

```powershell
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Isso fornecerá algo parecido com o seguinte.

![Layout do pacote](images/package_contents.png)

Se você não tiver um arquivo. msix (ou. AppX) para começar, poderá criar a pasta e os arquivos do pacote do zero.

### <a name="get-the-package-support-framework-files"></a>Obter os arquivos da estrutura de suporte do pacote

Você pode obter o pacote NuGet do PSF usando a ferramenta de linha de comando do NuGet autônomo ou por meio do Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obter o pacote usando a ferramenta de linha de comando

Instale a ferramenta de linha de comando do NuGet deste local: https://www.nuget.org/downloads. Em seguida, na linha de comando do NuGet, execute este comando: 

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
| [PSFLauncher64. exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32. exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64. dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32. dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64. exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32. exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

O conteúdo do pacote agora deve ser semelhante a este.

![Binários de pacote](images/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

Abra o manifesto do pacote em um editor de texto e, em seguida, defina o atributo `Executable` do elemento `Application` como o nome do arquivo executável do iniciador PSF.  Se você souber a arquitetura do seu aplicativo de destino, selecione a versão apropriada, PSFLauncher32. exe ou PSFLauncher64. exe.  Caso contrário, o PSFLauncher32. exe funcionará em todos os casos.  Aqui está um exemplo.

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

Crie um nome de arquivo ``config.json``e salve esse arquivo na pasta raiz do seu pacote. Modifique a ID do aplicativo declarado do arquivo config. JSON para apontar para o executável que você acabou de substituir. Usando o conhecimento obtido usando o Process Monitor, você também pode definir o diretório de trabalho, bem como usar a correção de redirecionamento de arquivo para redirecionar leituras/gravações para arquivos. log no diretório "PSFSampleApp" relativo ao pacote.

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

Veja a seguir um guia para o esquema config. JSON:

| Array | key | {1&gt;Valor&lt;1} |
|-------|-----------|-------|
| aplicativos | {1&gt;id&lt;1} |  Use o valor do atributo `Id` do elemento `Application` no manifesto do pacote. |
| aplicativos | executá | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do arquivo de manifesto do pacote antes de modificá-lo. É o valor do atributo `Executable` do elemento `Application`. |
| aplicativos | workingDirectory | Adicional Um caminho relativo de pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usará o diretório `System32` como o diretório de trabalho do aplicativo. |
| processos | executá | Na maioria dos casos, esse será o nome do `executable` configurado acima com o caminho e a extensão de arquivo removidos. |
| ajustes | dll | Caminho relativo ao pacote para a correção,. msix/. Appx a ser carregado. |
| ajustes | config | Adicional Controla como a DLL de correção se comporta. O formato exato desse valor varia de acordo com a correção, pois cada correção pode interpretar esse "blob" como desejado. |

As chaves `applications`, `processes`e `fixups` são matrizes. Isso significa que você pode usar o arquivo config. JSON para especificar mais de um aplicativo, processo e DLL de correção.

### <a name="package-and-test-the-app"></a>Empacotar e testar o aplicativo

Em seguida, crie um pacote.

```powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Em seguida, assine-o.

```powershell
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obter mais informações, consulte [como criar um certificado de assinatura de pacote](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) e [como assinar um pacote usando Signtool](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

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

Uma técnica alternativa para diagnosticar problemas de compatibilidade de aplicativos empacotados é usar a correção de rastreamento. Essa DLL está incluída no PSF e fornece uma exibição de diagnóstico detalhada do comportamento do aplicativo, semelhante ao Process Monitor.  Ele é especialmente projetado para revelar problemas de compatibilidade de aplicativos.  Para usar a correção de rastreamento, adicione a DLL ao pacote, adicione o fragmento a seguir ao seu config. JSON e, em seguida, empacote e instale o aplicativo.

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

Por padrão, a correção de rastreamento filtra falhas que podem ser consideradas "esperadas".  Por exemplo, os aplicativos podem tentar excluir um arquivo incondicionalmente sem verificar se ele já existe, ignorando o resultado. Isso tem a conseqüência de que algumas falhas inesperadas podem ser filtradas, portanto, no exemplo acima, optamos por receber todas as falhas das funções FileSystem. Fazemos isso porque sabemos antes que a tentativa de ler do arquivo config. txt falha com a mensagem "arquivo não encontrado". Essa é uma falha que é frequentemente observada e, geralmente, não é presumida como inesperada. Na prática, é provável que seja melhor começar a filtragem apenas para falhas inesperadas e, em seguida, voltar a todas as falhas se houver um problema que ainda não possa ser identificado.

Por padrão, a saída da correção de rastreamento é enviada para o depurador anexado. Para este exemplo, não vamos anexar um depurador e, em vez disso, usaremos o programa [DebugView](https://docs.microsoft.com/sysinternals/downloads/debugview) da Sysinternals para exibir sua saída. Depois de executar o aplicativo, podemos ver as mesmas falhas que antes, o que nos indicaria nas mesmas correções de tempo de execução.

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

| {1&gt;Projeto&lt;1} | Finalidade |
|-------|-----------|
| DesktopApplicationPackage | Este projeto é baseado no [projeto de empacotamento de aplicativos do Windows](../desktop/desktop-to-uwp-packaging-dot-net.md) e gera o pacote MSIX. |
| Runtimefix | Este é um C++ projeto de biblioteca vinculado dinâmico que contém uma ou mais funções de substituição que servem como correção de tempo de execução. |
| PSFLauncher | Este é C++ um projeto vazio. Este projeto é um local para coletar os arquivos distribuíveis de tempo de execução da estrutura de suporte do pacote. Ele gera um arquivo executável. Esse executável é a primeira coisa que é executada quando você inicia a solução. |
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

Adicione um C++ projeto de **dll (biblioteca de vínculo dinâmico)** à solução.

![Biblioteca de correção de tempo de execução](images/runtime-fix-library.png)

Clique com o botão direito do mouse no projeto e escolha **Propriedades**.

Nas páginas de propriedades, encontre o  **C++ campo padrão de idioma** e, na lista suspensa ao lado desse campo, selecione a opção **ISO c++ 17 Standard (/std: c++ 17)** .

![Opção ISO 17](images/iso-option.png)

Clique com o botão direito do mouse no projeto e, no menu de contexto, escolha a opção **gerenciar pacotes NuGet** . Verifique se a opção **origem do pacote** está definida como **All** ou **NuGet.org**.

Clique no ícone de configurações próximo ao campo.

Procure o pacote NuGet *PSF** e instale-o para este projeto.

![pacote NuGet](images/psf-package.png)

Se você quiser depurar ou estender uma correção de tempo de execução existente, adicione os arquivos de correção de tempo de execução obtidos usando as diretrizes descritas na seção [encontrar uma correção de tempo de execução](#find) deste guia.

Se você pretende criar uma correção totalmente nova, não adicione nada a esse projeto ainda. Ajudaremos você a adicionar os arquivos corretos a esse projeto posteriormente neste guia. Por enquanto, continuaremos Configurando sua solução.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Adicionar um projeto que inicia o executável do iniciador PSF

Adicione um C++ projeto de **projeto vazio** à solução.

![Projeto vazio](images/blank-app.png)

Adicione o pacote NuGet do **PSF** a este projeto usando as mesmas diretrizes descritas na seção anterior.

Abra as páginas de propriedades do projeto e, na página configurações **gerais** , defina a propriedade **nome de destino** como ``PSFLauncher32`` ou ``PSFLauncher64`` dependendo da arquitetura do seu aplicativo.

![Referência do iniciador PSF](images/shim-exe-reference.png)

Adicione uma referência de projeto ao projeto de correção de tempo de execução em sua solução.

![referência de correção de tempo de execução](images/reference-fix.png)

Clique com o botão direito do mouse na referência e, em seguida, na janela **Propriedades** , aplique esses valores.

| Propriedade | {1&gt;Valor&lt;1} |
|-------|-----------|
| Copiar local | True |
| Copiar assemblies satélite locais | True |
| Saída do assembly de referência | True |
| Vincular dependências de biblioteca | False |
| Entradas de dependência da biblioteca de links | False |

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

Forneça um valor para cada chave. Use esta tabela como um guia.

| Array | key | {1&gt;Valor&lt;1} |
|-------|-----------|-------|
| aplicativos | {1&gt;id&lt;1} |  Use o valor do atributo `Id` do elemento `Application` no manifesto do pacote. |
| aplicativos | executá | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do arquivo de manifesto do pacote antes de modificá-lo. É o valor do atributo `Executable` do elemento `Application`. |
| aplicativos | workingDirectory | Adicional Um caminho relativo de pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usará o diretório `System32` como o diretório de trabalho do aplicativo. |
| processos | executá | Na maioria dos casos, esse será o nome do `executable` configurado acima com o caminho e a extensão de arquivo removidos. |
| ajustes | dll | Caminho relativo do pacote para o DLL de correção a ser carregado. |
| ajustes | config | Adicional Controla como a DLL de correção se comporta. O formato exato desse valor varia de acordo com a correção, pois cada correção pode interpretar esse "blob" como desejado. |

Quando terminar, seu arquivo de ``config.json`` será semelhante a este.

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
> As chaves `applications`, `processes`e `fixups` são matrizes. Isso significa que você pode usar o arquivo config. JSON para especificar mais de um aplicativo, processo e DLL de correção.

### <a name="debug-a-runtime-fix"></a>Depurar uma correção de tempo de execução

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que inicia é o aplicativo iniciador PSF, que, por sua vez, inicia o aplicativo de área de trabalho de destino.  Para depurar o aplicativo de área de trabalho de destino, você precisará anexar manualmente ao processo do aplicativo de desktop escolhendo **depurar**->**anexar ao processo**e, em seguida, selecionando o processo do aplicativo. Para permitir a depuração de um aplicativo .NET com uma DLL de correção de tempo de execução nativa, selecione tipos de código gerenciados e nativos (depuração de modo misto).  

Depois de configurar isso, você pode definir pontos de interrupção ao lado das linhas de código no código do aplicativo da área de trabalho e no projeto de correção de tempo de execução. Se você não tiver o código-fonte para seu aplicativo, poderá definir pontos de interrupção somente ao lado das linhas de código em seu projeto de correção de tempo de execução.

>[!NOTE]
> Embora o Visual Studio ofereça a você a experiência de desenvolvimento e depuração mais simples, há algumas limitações, portanto, mais adiante neste guia, discutiremos outras técnicas de depuração que você pode aplicar.

## <a name="create-a-runtime-fix"></a>Criar uma correção de tempo de execução

Se ainda não houver uma correção de tempo de execução para o problema que você deseja resolver, você poderá criar uma nova correção de tempo de execução escrevendo funções de substituição e incluindo quaisquer dados de configuração que façam sentido. Vamos examinar cada parte.

### <a name="replacement-functions"></a>Funções de substituição

Primeiro, identifique quais chamadas de função falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você poderá criar funções de substituição que você deseja que sejam chamadas pelo gerenciador de runtime. Isso oferecerá uma oportunidade de substituir a implementação de uma função por um comportamento que esteja de acordo com as regras do ambiente moderno do runtime.

No Visual Studio, abra o projeto de correção de tempo de execução que você criou anteriormente neste guia.

Declare a macro ``FIXUP_DEFINE_EXPORTS`` e, em seguida, adicione uma instrução include para o `fixup_framework.h` na parte superior de cada um. Arquivo CPP no qual você pretende adicionar as funções de sua correção de tempo de execução.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Verifique se a macro `FIXUP_DEFINE_EXPORTS` aparece antes da instrução include.

Crie uma função que tenha a mesma assinatura da função que é o comportamento que você deseja modificar. Aqui está uma função de exemplo que substitui a função `MessageBoxW`.

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

A chamada para `DECLARE_FIXUP` mapeia a função `MessageBoxW` para a nova função de substituição. Quando o aplicativo tentar chamar a função de `MessageBoxW`, ele chamará a função de substituição em vez disso.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Proteger contra chamadas recursivas para funções em correções de tempo de execução

Opcionalmente, você pode aplicar o tipo de `reentrancy_guard` às funções que protegem contra chamadas recursivas para funções em correções de tempo de execução.

Por exemplo, você pode produzir uma função de substituição para a função `CreateFile`. Sua implementação pode chamar a função `CopyFile`, mas a implementação da função `CopyFile` pode chamar a função `CreateFile`. Isso pode levar a um ciclo recursivo infinito de chamadas para a função `CreateFile`.

Para obter mais informações sobre `reentrancy_guard` consulte [Authoring.MD](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Dados de configuração

Se você quiser adicionar dados de configuração à correção de tempo de execução, considere adicioná-los ao ``config.json``. Dessa forma, você pode usar o `FixupQueryCurrentDllConfig` para analisar facilmente esses dados. Este exemplo analisa um valor booliano e de cadeia de caracteres desse arquivo de configuração.

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

## <a name="other-debugging-techniques"></a>Outras técnicas de depuração

Embora o Visual Studio ofereça a você a experiência de desenvolvimento e depuração mais simples, há algumas limitações.

Primeiro, a depuração F5 executa o aplicativo implantando arquivos soltos do caminho da pasta de layout do pacote, em vez de instalar de um pacote. msix/. Appx.  A pasta de layout normalmente não tem as mesmas restrições de segurança que uma pasta de pacote instalada. Como resultado, talvez não seja possível reproduzir os erros de negação de acesso ao caminho do pacote antes de aplicar uma correção de tempo de execução.

Para resolver esse problema, use a implantação de pacote. msix/. Appx em vez de F5 implantação de arquivo flexível.  Para criar um arquivo de pacote. msix/. Appx, use o utilitário [MakeAppx](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) do SDK do Windows, conforme descrito acima. Ou, no Visual Studio, clique com o botão direito do mouse no nó do projeto de aplicativo e selecione **armazenar**->**criar pacotes de aplicativos**.

Outro problema com o Visual Studio é que ele não tem suporte interno para anexar a processos filho iniciados pelo depurador.   Isso dificulta a depuração da lógica no caminho de inicialização do aplicativo de destino, que deve ser anexado manualmente pelo Visual Studio após a inicialização.

Para resolver esse problema, use um depurador que dá suporte à anexação de processo filho.  Observe que geralmente não é possível anexar um depurador JIT (just-in-time) ao aplicativo de destino.  Isso ocorre porque a maioria das técnicas de JIT envolve a inicialização do depurador no lugar do aplicativo de destino, por meio da chave do registro ImageFileExecutionOptions.  Isso anula o mecanismo de despasseio usado pelo PSFLauncher. exe para injetar FixupRuntime. dll no aplicativo de destino.  O WinDbg, incluído nas [ferramentas de depuração para Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/index), e obtido do [SDK do Windows](https://developer.microsoft.com/windows/downloads/windows-10-sdk), dá suporte à anexação do processo filho.  Agora, ele também dá suporte à [inicialização e à depuração direta de um aplicativo UWP](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar a inicialização do aplicativo de destino como um processo filho, inicie ``WinDbg``.

```powershell
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

No prompt de ``WinDbg``, habilite a depuração de filho e defina os pontos de interrupção apropriados.

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
> O [o plmdebug](https://docs.microsoft.com/windows-hardware/drivers/debugger/plmdebug) também pode ser usado para anexar um depurador a um aplicativo na inicialização e também está incluído nas [ferramentas de depuração para Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/index).  No entanto, é mais complexo usar do que o suporte direto agora fornecido pelo WinDbg.

## <a name="support"></a>Suporte

Tem dúvidas? Pergunte-nos sobre o [pacote de suporte de estrutura](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support) de conversa do Framework no site da Comunidade do MSIX Tech.
