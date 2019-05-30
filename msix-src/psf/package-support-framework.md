---
Description: Corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner de MSIX
title: Corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner de MSIX
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 837ca1566fb0ccf60288e1ca5020f26eed8ca7c8
ms.sourcegitcommit: c3bdc2150bba942dc95811746c7a0f14ce54fbc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985976"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correções de tempo de execução em um pacote MSIX usando a estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de software livre que ajuda você a aplicar correções para o seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele pode ser executado em um contêiner de MSIX. A estrutura de suporte de pacote ajuda seu aplicativo siga as práticas recomendadas do ambiente moderno do tempo de execução.

Para obter mais informações, consulte [estrutura de suporte do pacote](package-support-framework-overview.md).

Este guia ajudará você a identificar problemas de compatibilidade de aplicativo, e localizar, aplicar e estender o tempo de execução correções que corrigi-los.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidade de aplicativo empacotado

Primeiro, crie um pacote para seu aplicativo. Em seguida, instalá-lo, executá-lo e observar seu comportamento. Você pode receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Você também pode usar [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) para identificar problemas.  Problemas comuns se relacionam com as suposições de aplicativo em relação a permissões de caminho de diretório e o programa de trabalho.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usando o Monitor de processos para identificar um problema

[Monitor de processos](https://docs.microsoft.com/sysinternals/downloads/procmon) é um utilitário poderoso para observar o arquivo do aplicativo e as operações de registro e seus resultados.  Isso pode ajudar você a entender os problemas de compatibilidade de aplicativos.  Depois de abrir o Monitor de processos, adicionar um filtro (filtro > filtro...) para incluir apenas eventos do executável do aplicativo.

![Filtro de aplicativo ProcMon](images/procmon_app_filter.png)

Será exibida uma lista de eventos. Para muitos desses eventos, a palavra **sucesso** aparecerá na **resultado** coluna.

![Eventos de ProcMon](images/procmon_events.png)

Opcionalmente, você pode filtrar eventos para mostrar apenas as falhas.

![Excluir ProcMon sucesso](images/procmon_exclude_success.png)

Se você suspeitar de uma falha de acesso do sistema de arquivos, procure eventos com falha que estão sob o System32/SysWOW64 ou o caminho do arquivo de pacote. Filtros podem também ajudá-lo aqui, também. Iniciar na parte inferior desta lista e role para cima. Falhas que aparecem na parte inferior desta lista ocorreram mais recentemente. Preste atenção a maioria dos erros que contêm cadeias de caracteres como "acesso negado" e "nome do caminho não encontrado" e ignore as coisas que não parecem suspeitas. O [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tem dois problemas. Você pode ver esses problemas na lista que aparece na imagem a seguir.

![ProcMon txt](images/procmon_config_txt.png)

O primeiro problema que aparece nesta imagem, o aplicativo está falhando ler o arquivo "Txt" que está localizado no caminho "C:\Windows\SysWOW64". É improvável que o aplicativo está tentando fazer referência a esse caminho diretamente. É bem provável que ele está tentando ler esse arquivo usando um caminho relativo, e por padrão, "System32/SysWOW64" é o diretório de trabalho do aplicativo. Isso sugere que o aplicativo está esperando o diretório de trabalho a ser definido como em algum lugar no pacote. Procurando dentro o appx, podemos ver que o arquivo existe no mesmo diretório do executável.

![Aplicativo txt](images/psfsampleapp_config_txt.png)

O segundo problema é exibido na imagem a seguir.

![Arquivo de log ProcMon](images/procmon_logfile.png)

Nesta edição, o aplicativo está falhando gravar um arquivo. log em seu caminho de pacote. Isso seria sugerem que uma correção de redirecionamento de arquivo pode ajudar.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Localizar uma correção de tempo de execução

O PSF contém correções de tempo de execução que você pode usar no momento, como a correção de redirecionamento de arquivo.

### <a name="file-redirection-fixup"></a>Correção de redirecionamento de arquivo

Você pode usar o [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para redirecionar as tentativas de gravar ou ler dados em um diretório que não esteja acessível a partir de um aplicativo que é executado em um contêiner MSIX.

Por exemplo, se seu aplicativo grava em um arquivo de log que está no mesmo diretório que os executáveis de aplicativos, em seguida, você pode usar o [correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para criar esse arquivo de log em outro local, como o armazenamento de dados de aplicativo local.

### <a name="runtime-fixes-from-the-community"></a>Correções de tempo de execução da comunidade

Certifique-se de examinar as contribuições da comunidade para nossos [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) página. É possível que outros desenvolvedores resolveram um problema semelhante ao seu e compartilharam uma correção de tempo de execução.

## <a name="apply-a-runtime-fix"></a>Aplicar uma correção de tempo de execução

Você pode aplicar uma correção de tempo de execução existente com algumas ferramentas simples do SDK do Windows e seguindo estas etapas.

> [!div class="checklist"]
> * Crie uma pasta de layout do pacote
> * Obter os arquivos de estrutura de suporte do pacote
> * Adicioná-los ao seu pacote
> * Modificar o manifesto do pacote
> * Criar um arquivo de configuração

Vamos percorra cada tarefa.

### <a name="create-the-package-layout-folder"></a>Crie a pasta de layout do pacote

Se você já tiver um arquivo .msix (ou. AppX), você pode descompactar seu conteúdo em uma pasta de layout que servirá como a área de preparo para o seu pacote. Você pode fazer isso em um prompt de comando usando a ferramenta MakeAppx, com base em seu caminho de instalação do SDK, que é onde você encontrará a ferramenta makeappx.exe em seu computador Windows 10: x86: C:\Program arquivos (x86) \Windows Kits\10\bin\x86\makeappx.exe x64: C:\Program arquivos (x86) \Windows Kits\10\bin\x64\makeappx.exe

```powershell
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Isso lhe dará algo parecido com o seguinte.

![Layout do pacote](images/package_contents.png)

Se você não tiver um arquivo .msix (ou. AppX) para começar, você pode criar os arquivos e a pasta do pacote do zero.

### <a name="get-the-package-support-framework-files"></a>Obter os arquivos de estrutura de suporte do pacote

Você pode obter o pacote Nuget PSF usando a ferramenta de linha de comando do Nuget autônomo ou por meio do Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenha o pacote usando a ferramenta de linha de comando

Instalar a ferramenta de linha de comando do Nuget deste local: https://www.nuget.org/downloads. Em seguida, na linha de comando do Nuget, execute este comando:

```powershell
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenha o pacote usando o Visual Studio

No Visual Studio, o nó do projeto ou solução com o botão direito e selecione um dos comandos de Manage Nuget Packages.  Pesquise **Microsoft.PackageSupportFramework** ou **PSF** para localizar o pacote em Nuget.org. Em seguida, instalá-lo.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Adicionar os arquivos de estrutura de suporte do pacote ao seu pacote

Adicione os arquivos executáveis e DLLs necessárias de PSF de 32 bits e 64 bits para o diretório de pacote. Use a tabela a seguir como guia. Você também vai querer incluir as correções de tempo de execução que você precisa. Em nosso exemplo, é necessário que a correção de tempo de execução de redirecionamento de arquivo.

| Executável do aplicativo é x64 | Aplicativo executável é x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

O conteúdo do pacote agora deve ser semelhante ao seguinte.

![Binários de pacote](images/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

Abra seu manifesto de pacote em um editor de texto e, em seguida, defina as `Executable` atributo do `Application` elemento para o nome do arquivo executável PSF iniciador.  Se você souber a arquitetura do seu aplicativo de destino, selecione a versão apropriada, PSFLauncher32.exe ou PSFLauncher64.exe.  Caso contrário, PSFLauncher32.exe funcionará em todos os casos.  Aqui está um exemplo.

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

Criar um nome de arquivo ``config.json``e salve esse arquivo para a pasta raiz do seu pacote. Modificar a ID do aplicativo declarados do arquivo config para apontar para o executável que você acabou de ser substituído. Usando o conhecimento obtido do usando o Monitor de processo, você pode também definir o diretório de trabalho, bem como usar a correção de redirecionamento de arquivo para redirecionar leituras e gravações para arquivos. log no diretório relativo do pacote "PSFSampleApp".

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

A seguir está um guia para o esquema config:

| Array | key | Valor |
|-------|-----------|-------|
| applications | id |  Use o valor da `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor de seu arquivo de manifesto de pacote antes de modificá-lo. É o valor da `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo do pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome da `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho relativo do pacote para a correção,.msix/.appx para carregar. |
| correções | config | (Opcional) Controla como a lista de distribuição de correção se comporta. O formato exato desse valor varia de acordo com uma base de correção por correção como cada correção pode interpretar esse "blob" quanto desejar. |

O `applications`, `processes`, e `fixups` chaves são matrizes. Isso significa que você pode usar o arquivo Config XML para especificar mais de um aplicativo, processo e correção de DLL.

### <a name="package-and-test-the-app"></a>Teste o aplicativo e pacote

Em seguida, crie um pacote.

```powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Em seguida, assiná-lo.

```powershell
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obter mais informações, consulte [como criar um certificado de assinatura de pacote](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) e [como assinar um pacote usando o signtool](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Usando o PowerShell, instale o pacote.

>[!NOTE]
> Lembre-se de desinstalar o pacote pela primeira vez.

```powershell
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Execute o aplicativo e observar o comportamento com correção de tempo de execução foi aplicada.  Repita as etapas de empacotamento conforme necessário e diagnóstico.

### <a name="use-the-trace-fixup"></a>Usar a correção de rastreamento

Uma técnica alternativa para diagnosticar problemas de compatibilidade de aplicativo empacotado é usar a correção de rastreamento. Essa DLL está incluído com o PSF e fornece uma exibição de diagnóstico detalhada do comportamento do aplicativo, semelhante ao Monitor de processos.  Ele é projetado especialmente para revelar problemas de compatibilidade de aplicativos.  Para usar a correção de rastreamento, adicionar a DLL para o pacote, adicione o seguinte fragmento para o config. JSON e, em seguida, empacotar e instalar seu aplicativo.

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

Por padrão, a correção de rastreamento filtra as falhas que podem ser consideradas "esperadas".  Por exemplo, aplicativos podem tentar incondicionalmente excluir um arquivo sem verificar se ele já existe, ignorando o resultado. Isso tem a consequência infeliz que algumas falhas inesperadas podem obter filtradas, portanto, no exemplo acima, podemos optar por receber todas as falhas de funções do sistema de arquivos. Fazemos isso porque sabemos de antes que a tentativa de ler do arquivo config txt falhar com a mensagem "arquivo não encontrado". Essa é uma falha que é observada com frequência e geralmente não é considerada como inesperado. Na prática é provavelmente melhor para começar a filtragem apenas para falhas inesperadas e, em seguida, voltando para todas as falhas se não houver um problema que ainda não pode ser identificado.

Por padrão, a saída a correção de rastreamento é enviada para o depurador anexado. Neste exemplo, nós não estão indo para anexar um depurador e usará o [DebugView](https://docs.microsoft.com/sysinternals/downloads/debugview) programa da SysInternals para exibir sua saída. Depois de executar o aplicativo, podemos ver as mesmas falhas como antes, que nos aponta em direção as mesma correções de tempo de execução.

![O arquivo TraceShim não encontrado](images/traceshim_filenotfound.png)

![TraceShim acesso negado](images/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, estender ou criar uma correção de tempo de execução

Você pode usar o Visual Studio para depurar uma correção de tempo de execução, estender uma correção de tempo de execução ou criar um do zero. Você precisará fazer essas coisas para ter êxito.

> [!div class="checklist"]
> * Adicionar um projeto de empacotamento
> * Adicione o projeto para a correção de tempo de execução
> * Adicionar um projeto que inicia o inicializador PSF executável
> * Configurar o projeto de empacotamento

Quando terminar, sua solução será algo parecido com isso.

![Solução completa](images/runtime-fix-project-structure.png)

Vamos examinar cada projeto neste exemplo.

| Projeto | Finalidade |
|-------|-----------|
| DesktopApplicationPackage | Este projeto se baseia a [projeto de empacotamento de aplicativos do Windows](../desktop/desktop-to-uwp-packaging-dot-net.md) e ele produz o pacote MSIX. |
| Runtimefix | Isso é um projeto de biblioteca do C++ Dynamic-Linked que contém um ou mais funções de substituição que servem como a correção de tempo de execução. |
| PSFLauncher | Isso é um projeto vazio do C++. Esse projeto é um local para coletar os arquivos do distribuível de tempo de execução da estrutura de suporte do pacote. Ele produz um arquivo executável. Esse executável é a primeira coisa que é executado quando você inicia a solução. |
| WinFormsDesktopApplication | Este projeto contém o código-fonte de um aplicativo da área de trabalho. |

Para ver um exemplo completo que contém todos esses tipos de projetos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos percorrer as etapas para criar e configurar cada um desses projetos em sua solução.

### <a name="create-a-package-solution"></a>Criar uma solução de pacote

Se você ainda não tiver uma solução para seu aplicativo da área de trabalho, crie uma nova **solução em branco** no Visual Studio.

![Solução em branco](images/blank-solution.png)

Você talvez queira adicionar projetos qualquer aplicativo que você tem.

### <a name="add-a-packaging-project"></a>Adicionar um projeto de empacotamento

Se você ainda não tiver um **Windows Application Packaging Project**, crie uma e adicioná-lo à sua solução.

![Modelo de projeto de pacote](images/package-project-template.png)

Para obter mais informações sobre o projeto de empacotamento de aplicativos do Windows, consulte [empacotar seu aplicativo usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

Na **Gerenciador de soluções**, clique com botão direito do projeto de empacotamento, selecione **editar**e, em seguida, adicione isso à parte inferior do arquivo de projeto:

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

### <a name="add-project-for-the-runtime-fix"></a>Adicione o projeto para a correção de tempo de execução

Adicionar um C++ **biblioteca de vínculo dinâmico (DLL)** projeto à solução.

![Biblioteca de correção de tempo de execução](images/runtime-fix-library.png)

Clique com botão direito do que de projeto e, em seguida, escolha **propriedades**.

Nas páginas de propriedades, localize o **padrão de linguagem C++** campo e, em seguida, na lista suspensa ao lado do campo, selecione o **ISO padrão c++17 (/ std:c++17 + + 17)** opção.

![ISO 17 opção](images/iso-option.png)

Clique com botão direito nesse projeto e, em seguida, no menu de contexto, escolha o **gerenciar pacotes Nuget** opção. Certifique-se de que o **origem do pacote** opção for definida como **todos os** ou **nuget.org**.

Clique no ícone de configurações em Avançar nesse campo.

Pesquise o *PSF** Nuget do pacote e, em seguida, instalá-lo para este projeto.

![pacote do NuGet](images/psf-package.png)

Se você quiser depurar ou estender uma correção de tempo de execução existente, adicione os arquivos de correção de tempo de execução que obtidos usando as diretrizes descritas na [encontrar uma correção de tempo de execução](#find) seção deste guia.

Se você pretende criar uma nova correção, não adicione nada para este projeto ainda. Ajudaremos você a adicionar os arquivos certos para este projeto neste documento. Por enquanto, continuaremos configurando sua solução.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Adicionar um projeto que inicia o inicializador PSF executável

Adicionar um C++ **projeto vazio** projeto à solução.

![Projeto vazio](images/blank-app.png)

Adicione a **PSF** pacote do Nuget para este projeto, usando a mesma orientação descrita na seção anterior.

Abrir as páginas de propriedade para o projeto e, no **gerais** página de configurações, defina o **nome de destino** propriedade a ser ``PSFLauncher32`` ou ``PSFLauncher64`` dependendo da arquitetura do seu aplicativo.

![Referência de iniciador PSF](images/shim-exe-reference.png)

Adicione uma referência ao projeto de correção de tempo de execução em sua solução.

![referência de correção de tempo de execução](images/reference-fix.png)

A referência, com o botão direito e, em seguida, o **propriedades** janela, aplicar esses valores.

| Propriedade | Valor |
|-------|-----------|
| Copiar local | True |
| Copiar Assemblies de satélite de Local | True |
| Saída do Assembly de referência | True |
| Dependências de biblioteca de link | False |
| Entradas de dependência de biblioteca de link | False |

### <a name="configure-the-packaging-project"></a>Configurar o projeto de empacotamento

No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

![Adicionar referência de projeto](images/add-reference-packaging-project.png)

Escolha o projeto de iniciador PSF e seu projeto de aplicativo da área de trabalho e, em seguida, escolha o **Okey** botão.

![Projeto de desktop](images/package-project-references.png)

>[!NOTE]
> Se você não tiver o código-fonte ao seu aplicativo, basta escolha o projeto de iniciador PSF. Mostraremos como fazer referência a seu executável quando você cria um arquivo de configuração.

No **aplicativos** nó, o aplicativo iniciador PSF com o botão direito e, em seguida, escolha **definido como ponto de entrada**.

![Definir ponto de entrada](images/set-startup-project.png)

Adicione um arquivo chamado ``config.json`` ao seu projeto de empacotamento, em seguida, copie e cole o seguinte texto json no arquivo. Defina a **ação de pacote** propriedade **conteúdo**.

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

| Array | key | Valor |
|-------|-----------|-------|
| applications | id |  Use o valor da `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor de seu arquivo de manifesto de pacote antes de modificá-lo. É o valor da `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo do pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome da `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho relativo do pacote para a correção DLL seja carregada. |
| correções | config | (Opcional) Controla como a correção DLL se comporta. O formato exato desse valor varia de acordo com uma base de correção por correção como cada correção pode interpretar esse "blob" quanto desejar. |

Quando terminar, seu ``config.json`` arquivo será algo parecido com isso.

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
> O `applications`, `processes`, e `fixups` chaves são matrizes. Isso significa que você pode usar o arquivo Config XML para especificar mais de um aplicativo, processo e correção de DLL.

### <a name="debug-a-runtime-fix"></a>Depurar uma correção de tempo de execução

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que é iniciado é o aplicativo de iniciador PSF, que por sua vez, inicia o aplicativo de área de trabalho de destino.  Para depurar o aplicativo de área de trabalho de destino, você precisará anexar manualmente ao processo do aplicativo da área de trabalho escolhendo **Debug**->**anexar ao processo**e, em seguida, selecionando o aplicativo processo. Para permitir a depuração de um aplicativo .NET com uma DLL de correção de tempo de execução nativo, selecione os tipos de código gerenciado e nativo (depuração de modo misto).  

Depois que você configurou isso, você pode definir pontos de interrupção ao lado de linhas de código no código do aplicativo da área de trabalho e o projeto de correção de tempo de execução. Se você não tiver o código-fonte ao seu aplicativo, você poderá definir pontos de interrupção ao lado de linhas de código em seu projeto de correção de tempo de execução.

>[!NOTE]
> Enquanto o Visual Studio oferece a você o desenvolvimento mais simples e experiência de depuração, há algumas limitações, portanto, mais adiante neste guia, discutiremos outras técnicas de depuração que você pode aplicar.

## <a name="create-a-runtime-fix"></a>Criar uma correção de tempo de execução

Se não há ainda um tempo de execução corrige o problema que você deseja resolver, você pode criar uma nova correção de tempo de execução, escrevendo funções de substituição e incluindo quaisquer dados de configuração que faz sentido. Vamos examinar cada parte.

### <a name="replacement-functions"></a>Funções de substituição

Primeiro, identifique qual função chamadas falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você pode criar funções de substituição que você gostaria que o Gerenciador de tempo de execução para chamar em vez disso. Isso lhe dá uma oportunidade para substituir a implementação de uma função com o comportamento está de acordo com as regras do ambiente moderno do tempo de execução.

No Visual Studio, abra o projeto de correção de tempo de execução que você criou anteriormente neste guia.

Declare a ``FIXUP_DEFINE_EXPORTS`` macro e, em seguida, adicione uma instrução de inclusão para a `fixup_framework.h` na parte superior de cada um. Arquivo CPP onde você pretende adicionar as funções de sua correção de tempo de execução.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Certifique-se de que o `FIXUP_DEFINE_EXPORTS` macro aparece antes da instrução de inclusão.

Criar uma função que tem a mesma assinatura da função que tem comportamento que você deseja modificar. Aqui está um exemplo de função que substitui o `MessageBoxW` função.

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

A chamada para `DECLARE_FIXUP` mapeia o `MessageBoxW` função para sua nova função de substituição. Quando o aplicativo tenta chamar o `MessageBoxW` função, ele chamará a função de substituição em vez disso.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Proteger contra as chamadas recursivas a funções em tempo de execução correções

Opcionalmente, você pode aplicar o `reentrancy_guard` tipo para suas funções que protegem contra as chamadas recursivas a funções em tempo de execução correções.

Por exemplo, você pode produzir uma função de substituição para o `CreateFile` função. Sua implementação pode chamar o `CopyFile` função, mas a implementação do `CopyFile` função pode chamar o `CreateFile` função. Isso pode levar a um ciclo recursiva infinita de chamadas para o `CreateFile` função.

Para obter mais informações sobre `reentrancy_guard` consulte [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Dados de configuração

Se você quiser adicionar dados de configuração para a correção de tempo de execução, considere adicioná-lo para o ``config.json``. Dessa forma, você pode usar o `FixupQueryCurrentDllConfig` facilmente analisar esses dados. Este exemplo analisa um valor booliano e cadeia de caracteres do arquivo de configuração.

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

## <a name="other-debugging-techniques"></a>Outras técnicas de depuração

Enquanto o Visual Studio lhe permite o desenvolvimento mais simples e a experiência de depuração, há algumas limitações.

Em primeiro lugar, a depuração F5 executa o aplicativo Implantando arquivos flexíveis do caminho de pasta de layout de pacote, em vez de instalar de um .msix / pacote. AppX.  A pasta de layout normalmente não tem as mesmas restrições de segurança como uma pasta de pacote instalado. Como resultado, pode não ser possível reproduzir erros de negação de acesso de caminho de pacote antes de aplicar uma correção de tempo de execução.

Para resolver esse problema, use .msix / implantação do pacote. AppX, em vez de F5 flexível a implantação do arquivo.  Para criar um .msix / arquivo de pacote. AppX, use o [MakeAppx](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) utility a partir do SDK do Windows, conforme descrito acima. Ou, de dentro do Visual Studio, o nó do seu projeto de aplicativo com o botão direito e selecione **Store**->**criar pacotes de aplicativos**.

Outro problema com o Visual Studio é que ele não tem suporte interno para anexar a todos os processos filho iniciados pelo depurador.   Isso torna difícil de depurar a lógica no caminho de inicialização do aplicativo de destino, que deve ser anexado manualmente pelo Visual Studio após a inicialização.

Para resolver esse problema, use um depurador que ofereça suporte a anexar processo filho.  Observe que geralmente não é possível anexar um depurador de (JIT) just-in-time para o aplicativo de destino.  Isso ocorre porque a maioria das técnicas JIT envolvem iniciando o depurador em vez do aplicativo de destino, por meio da chave do registro ImageFileExecutionOptions.  Isso anula o mecanismo detouring usado pelo PSFLauncher.exe para injetar FixupRuntime.dll o aplicativo de destino.  WinDbg, incluído na [as ferramentas de depuração para Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/index)e obtidas da [SDK do Windows](https://developer.microsoft.com/windows/downloads/windows-10-sdk), anexar dá suporte ao processo de filho.  Ele também suporta diretamente [iniciar e depurar um aplicativo UWP](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar a inicialização do aplicativo de destino como um processo filho, iniciar ``WinDbg``.

```powershell
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

No ``WinDbg`` solicitar, habilitar a depuração de filho e defina pontos de interrupção apropriados.

```powershell
.childdbg 1
g
```

(executar até que o aplicativo de destino inicia e interrompe o depurador)

```powershell
sxe ld fixup.dll
g
```

(executar até a correção que dll é carregada)

```powershell
bp ...
```

>[!NOTE]
> [O PLMDebug](https://docs.microsoft.com/windows-hardware/drivers/debugger/plmdebug) também pode ser usado para anexar um depurador a um aplicativo ao ser iniciado e também está incluído na [ferramentas de depuração para Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/index).  No entanto, é mais complexo de usar do que o suporte direto agora fornecido pelo WinDbg.

## <a name="support"></a>Suporte

Tem dúvidas? Solicitar na [estrutura de suporte do pacote](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support) espaço de conversa no site da comunidade de tecnologia MSIX.
