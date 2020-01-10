---
Description: Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo de desktop. Talvez você não precise fazer muito para preparar seu app para o processo de empacotamento.
title: Preparar para empacotar um aplicativo de área de trabalho (MSIX)
ms.date: 08/22/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 3511b3f7bef1bc3a8ace27b5bc2eb6bd5b47cb45
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754856"
---
# <a name="prepare-to-package-a-desktop-application"></a>Preparar um pacote para um aplicativo de área de trabalho

Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo de área de trabalho. Talvez você não precise fazer muito para preparar o aplicativo para o processo de empacotamento, mas se qualquer um dos itens a seguir se aplicar ao seu aplicativo, você precisará solucioná-lo antes do empacotamento. Lembre-se de que a Microsoft Store lida com o licenciamento e a atualização automática para você, portanto, você pode remover qualquer recurso relacionado a essas tarefas de seu código base.

+ __Seu aplicativo .NET requer uma versão do .NET Framework anterior a 4.6.2__. Se você estiver empacotando um aplicativo .NET, recomendamos que o destino do aplicativo .NET Framework 4.6.2 ou posterior. A capacidade de instalar e executar aplicativos de desktop empacotados foi introduzida pela primeira vez no Windows 10, versão 1607 (também chamada de atualização de aniversário), e essa versão do sistema operacional inclui o .NET Framework 4.6.2 por padrão. Versões posteriores do so incluem versões posteriores do .NET Framework. Para obter uma lista completa de quais versões do .NET estão incluídas nas versões posteriores do Windows 10, consulte [Este artigo](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies).

  Espera-se que as versões de destino das .NET Framework anteriores à 4.6.2 em aplicativos de desktop empacotados funcionem na maioria dos casos. No entanto, se você visar uma versão anterior à 4.6.2, deverá testar completamente o aplicativo de área de trabalho empacotado antes de distribuí-lo aos usuários.

  + 4,0-4.6.1: os aplicativos destinados a essas versões do .NET Framework devem ser executados sem problemas no 4.6.2 ou posterior. Portanto, esses aplicativos devem ser instalados e executados sem alterações no Windows 10, versão 1607 ou posterior, com a versão do .NET Framework que está incluído no sistema operacional.

  + 2,0 e 3,5: em nossos testes, os aplicativos de área de trabalho empacotados que visam essas versões do .NET Framework geralmente funcionam, mas podem apresentar problemas de desempenho em alguns cenários. Para que esses aplicativos empacotados sejam instalados e executados, o [recurso .NET Framework 3,5](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10) deve ser instalado no computador de destino (esse recurso também inclui .NET Framework 2,0 e 3,0). Você também deve testar esses aplicativos cuidadosamente depois de empacotá-los.

+ __Seu aplicativo sempre é executado com privilégios de segurança elevados__. Seu aplicativo precisa funcionar durante a execução como usuário interativo. Os usuários que instalam seu aplicativo da Microsoft Store podem não ser administradores do sistema, portanto, exigir que seu aplicativo seja executado com privilégios elevados significa que ele não será executado corretamente para usuários padrão. Os aplicativos que exigem a elevação para qualquer parte da funcionalidade não serão aceitos na Microsoft Store.

+ __Seu aplicativo requer um driver de modo kernel ou um serviço do Windows__. A ponte de desktop é adequada para um aplicativo, mas não dá suporte a um driver de modo kernel ou a um serviço do Windows que precisa ser executado em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do seu aplicativo são carregados em processo, para processos que não estão no seu pacote do aplicativo do Windows__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Verifique se todas as extensões instaladas pelo aplicativo irão instalar onde o aplicativo está instalado__. O Windows permite que usuários e gerentes de ti alterem o local de instalação padrão para pacotes.  Consulte "Configurações-> armazenamento de > do sistema-> mais configurações de armazenamento-> Alterar onde o novo conteúdo é salvo em-> novos aplicativos serão salvos em".  Se você estiver instalando uma extensão com seu aplicativo, certifique-se de que a extensão não tenha restrições de pasta de instalação adicionais.  Por exemplo, algumas extensões podem desabilitar a instalação de seus extensão em unidades que não são do sistema.  Isso resultará em um erro 0x80073D01 (ERROR_DEPLOYMENT_BLOCKED_BY_POLICY) se o local padrão tiver sido alterado. 

+ __Seu aplicativo usa uma AUMID (ID de modelo de usuário de aplicativo) personalizada__. Se o processo chamar [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir seu próprio AUMID, ele só poderá usar o AUMID gerado para ele pelo pacote de aplicativos do Windows/ambiente do modelo de aplicativo. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do registro HKEY_LOCAL_MACHINE (HKLM)__ . Qualquer tentativa feita pelo aplicativo de criar uma chave HKLM ou para abrir uma para modificação resultará em uma falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada privada do registro, portanto, a noção de um hive de registro de todo o usuário e máquina (que é o que é HKLM) não se aplica. Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __Seu aplicativo usa uma subchave do registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Seu aplicativo grava na pasta AppData ou no registro com a intenção de compartilhar dados com outro aplicativo__. Após a conversão, o AppData é redirecionado para o armazenamento de dados do aplicativo local, que é um armazenamento privado para cada aplicativo.

  Todas as entradas que seu aplicativo grava no HKEY_LOCAL_MACHINE hive do registro são redirecionadas para um arquivo binário isolado e as entradas que seu aplicativo grava no hive do registro de HKEY_CURRENT_USER são colocadas em um local privado por usuário e por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivos e do Registro, consulte [Bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).  

  Use uma maneira diferente de compartilhamento de dados entre processos. Para obter mais informações, consulte [Armazene e recupere configurações e outros dados de aplicativo](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Seu aplicativo grava no diretório de instalação para seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você colocou no mesmo diretório que o exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __Seu aplicativo usa o diretório de trabalho atual__. Em tempo de execução, o aplicativo de área de trabalho empacotado não obterá o mesmo diretório de trabalho que você especificou anteriormente na área de trabalho. O atalho LNK. Você precisa alterar seu CWD em tempo de execução se tiver o diretório correto para que seu aplicativo funcione corretamente.

  > [!NOTE]
  > Se seu aplicativo precisar gravar no diretório de instalação ou usar o diretório de trabalho atual, você também pode considerar adicionar uma correção de tempo de execução usando a [estrutura de suporte do pacote](https://github.com/microsoft/MSIX-PackageSupportFramework) ao seu pacote. Para obter mais detalhes, consulte [Este artigo](../psf/package-support-framework.md). 

+ __A instalação do aplicativo requer interação do usuário__. O instalador do aplicativo deve ser capaz de executar silenciosamente e deve instalar todos os seus pré-requisitos que não estão ativados por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo requer o UIAccess__. Se seu aplicativo especificar `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto do UAC, a conversão para MSIX não terá suporte no momento. Para obter mais informações, consulte [Visão geral sobre a Segurança da Automação da Interface de Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Seu aplicativo expõe objetos com__. Processos e extensões de dentro do pacote podem registrar e usar servidores COM & OLE, tanto dentro do processo como fora do processo (OOP).  A Atualização dos Criadores adiciona suporte para COM integrados, o que fornece a habilidade de registrar servidores OOP COM & OLE que estão agora visíveis fora do pacote.  Consulte [Suporte ao Servido COM e ao Documento OLE para Ponte de Desktop](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge).

   O suporte ao COM Integrado funciona para APIs COM existentes, mas não funcionará para extensões de aplicativo que dependem da leitura direta do registro, uma vez que a localização do COM Integrado é privada.

+ __Seu aplicativo expõe assemblies do GAC para uso por outros processos__. Na versão atual, seu aplicativo não pode expor assemblies GAC para uso por processos provenientes de executáveis externos ao seu pacote de aplicativo do Windows. Os processos de dentro do pacote podem registrar e usar conjuntos GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos.

+ __Seu aplicativo está vinculando as bibliotecas de tempo de execução C (CRT) de forma não suportada__. A biblioteca de tempo de execução C/C++ da Microsoft oferece rotinas de programação para o sistema operacional Microsoft Windows. Essas rotinas automatizam muitas tarefas comuns de programação que não são fornecidas pelas linguagens C e C++. Se seu aplicativo utilizar a biblioteca CC++ /Runtime, você precisará garantir que ela esteja vinculada de uma maneira com suporte.

    O Visual Studio 2017 oferece suporte à vinculação estática e dinâmica para permitir que o código use arquivos DLL comuns, ou a vinculação estática, a fim de vincular a biblioteca diretamente no código para a versão atual da CRT. Se possível, recomendamos que seu aplicativo use a vinculação dinâmica com o VS 2017.

    O suporte nas versões anteriores do Visual Studio varia. Para obter detalhes, consulte a tabela a seguir:

    <table>
    <th>Versão do Visual Studio</td><th>Vinculação dinâmica</th><th>Vinculação estática</th></th>
    <tr><td>2005 (VC 8)</td><td>Sem suporte</td><td>Com suporte</td>
    <tr><td>2008 (VC 9)</td><td>Sem suporte</td><td>Com suporte</td>
    <tr><td>2010 (VC 10)</td><td>Com suporte</td><td>Com suporte</td>
    <tr><td>2012 (VC 11)</td><td>Com suporte</td><td>Sem suporte</td>
    <tr><td>2013 (VC 12)</td><td>Com suporte</td><td>Sem suporte</td>
    <tr><td>2015 e 2017 (VC 14)</td><td>Com suporte</td><td>Com suporte</td>
    </table>

    Observação: em todos os casos, você deve vincular ao CRT disponível publicamente mais recente.

+ __Seu aplicativo instala e carrega assemblies da pasta lado a lado do Windows__. Por exemplo, seu aplicativo usa as bibliotecas de tempo de execução C VC8 ou VC9 e está vinculando-as dinamicamente da pasta lado a lado do Windows, o que significa que seu código está usando os arquivos DLL comuns de uma pasta compartilhada. Isso não tem suporte. Você precisará vinculá-las de forma estática vinculando-as aos arquivos da biblioteca redistribuível diretamente no código.

+ __Seu aplicativo usa uma dependência na pasta system32/SysWOW64__. Para fazer essas DLLs funcionarem, você deve incluí-las na parte do sistema de arquivos virtual do pacote de aplicativo do Windows. Isso garante que o aplicativo se comporta como se as DLLs estivessem instaladas na pasta **System32**/**SysWOW64** . Na raiz do pacote, crie uma pasta chamada **VFS**. Dentro dessa pasta, crie uma pasta **SystemX64** e uma **SystemX86**. Em seguida, coloque a versão de 32 bits de sua DLL na pasta **SystemX86** e a versão de 64 bits na pasta **SystemX64**.

+ __Seu app usa um pacote de estrutura VCLibs__. Se você estiver convertendo C++ um aplicativo Win32, deverá lidar com a implantação do tempo C++ de execução Visual. O Visual Studio 2019 e o SDK do Windows incluem os pacotes mais recentes do Framework para a versão 11,0, 12,0 e C++ 14,0 do tempo de execução Visual nas seguintes pastas:

    * **Pacotes do VC 14,0 Framework**: C:\Arquivos de programas (x86) \Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop\14.0

    * **Pacotes do VC 12,0 Framework**: C:\Arquivos de programas (x86) \Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.desktop.120\14.0

    * **Pacotes do VC 11,0 Framework**: C:\Arquivos de programas (x86) \Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.desktop.110\14.0

    Para usar um desses pacotes, você deve fazer referência ao pacote como uma dependência no manifesto do pacote. Quando os clientes instalam a versão comercial do seu aplicativo da Microsoft Store, o pacote será instalado da loja junto com seu aplicativo. As dependências não serão instaladas se você carregar seu aplicativo. Para instalar as dependências manualmente, você deve instalar o pacote de estrutura apropriado usando o pacote. Appx apropriado para x86, x64 ou ARM localizado nas pastas de instalação listadas acima.

    Para fazer referência a C++ um pacote do Visual Runtime Framework em seu aplicativo:

    1. Vá para a pasta de instalação do pacote do Framework listada acima para a C++ versão do tempo de execução Visual usada pelo seu aplicativo.

    2. Abra o arquivo SDKManifest. xml nessa pasta, localize o atributo `FrameworkIdentity-Debug` ou `FrameworkIdentity-Retail` (dependendo se você estiver usando a versão de depuração ou de varejo do tempo de execução) e copie os valores `Name` e `MinVersion` desse atributo. Por exemplo, aqui está o atributo `FrameworkIdentity-Retail` para o pacote de estrutura VC 14,0 atual.
        ```xml
        FrameworkIdentity-Retail = "Name = Microsoft.VCLibs.140.00.UWPDesktop, MinVersion = 14.0.27323.0, Publisher = 'CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US'"
        ```

    3. No manifesto do pacote para seu aplicativo, adicione o seguinte elemento `<PackageDependency>` sob o nó `<Dependencies>`. Substitua os valores de `Name` e `MinVersion` pelos valores que você copiou na etapa anterior. O exemplo a seguir especifica uma dependência para a versão atual do pacote da estrutura VC 14,0.
        ```xml
        <PackageDependency Name="Microsoft.VCLibs.140.00.UWPDesktop" MinVersion="14.0.27323.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
        ```

+ __Seu aplicativo contém uma lista de atalhos personalizada__. Há vários problemas e limitações a que é preciso estar atento ao usar listas de atalhos.

    - __A arquitetura do aplicativo não corresponde ao sistema operacional.__  As listas de atalhos atualmente não funcionarão corretamente se as arquiteturas do aplicativo e do sistema operacional não corresponderem (por exemplo, um aplicativo x86 em execução em Windows x64). Neste momento, não há nenhuma solução alternativa diferente de recompilar seu aplicativo para a arquitetura correspondente.

    - __Seu aplicativo cria entradas de lista de atalhos e chama [ICustomDestinationList:: setappid](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__ . Não defina AppID de forma programática no código. Isso fará com que as entradas da lista de atalhos não sejam exibidas. Se seu aplicativo precisar de uma ID personalizada, especifique-a usando o arquivo de manifesto. Consulte [empacotar um aplicativo da área de trabalho manualmente](desktop-to-uwp-manual-conversion.md) para obter instruções. A AppID de seu aplicativo é especificada na seção *YOUR_PRAID_HERE*.

    - __Seu aplicativo adiciona um link de Shell de lista de atalhos que faz referência a um executável em seu pacote__. Você não pode iniciar executáveis diretamente em seu pacote a partir de uma lista de atalhos (com a exceção do caminho absoluto do .exe do próprio aplicativo). Em vez disso, registre um alias de execução do aplicativo (que permite que o aplicativo da área de trabalho do pacote inicie por meio de uma palavra-chave como se estivesse no caminho) e defina o caminho de destino do link como o alias. Para obter detalhes sobre como usar a extensão appExecutionAlias, consulte [integrar seu aplicativo de área de trabalho com o Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions). Observe que, se você precisar que os ativos do link na lista de atalhos correspondam ao .exe original, defina os ativos, como o ícone, usando [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) e o nome de exibição com PKEY_Title como você faria para outras entradas personalizadas.

    - __Seu aplicativo adiciona entradas de lista de atalhos que fazem referência a ativos no pacote do aplicativo por caminhos absolutos__. O caminho de instalação de um aplicativo pode ser alterado quando seus pacotes são atualizados, alterando o local dos ativos (como ícones, documentos, executáveis e assim por diante). Se as entradas da lista de atalhos fizerem referência a tais ativos por caminhos absolutos, o aplicativo deverá atualizar sua lista de atalhos periodicamente (como na inicialização do aplicativo) para garantir que os caminhos sejam resolvidos corretamente. Como alternativa, use as APIs UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), que permitem referenciar ativos de cadeias de caracteres e imagens usando o esquema de URI package-relative ms-resource (que também reconhece linguagem, DPI e alto contraste).

+ __Seu aplicativo inicia um utilitário para executar tarefas__. Evite iniciar utilitários de comando, como o PowerShell e o Cmd.exe. Na verdade, se os usuários instalarem seu aplicativo em um sistema que executa o Windows 10 S, seu aplicativo não conseguirá iniciá-los. Isso pode impedir que seu aplicativo seja enviado para o Microsoft Store porque todos os aplicativos enviados para o Microsoft Store devem ser compatíveis com o Windows 10 S.

Iniciar um utilitário pode geralmente fornecer uma maneira conveniente para obter informações do sistema operacional, acessar o registro ou acessar as funcionalidades do sistema. No entanto, você pode usar APIs UWP para realizar esses tipos de tarefas. Essas APIs são mais eficazes porque não precisam de um executável separado para serem executadas, mas o mais importante são impedir que o aplicativo chegue fora do pacote. O design do aplicativo permanece consistente com o isolamento, a confiança e a segurança que acompanham um aplicativo que você pacoteu e seu aplicativo se comportará conforme o esperado em sistemas que executam o Windows 10 S.

+ __Seu aplicativo hospeda suplementos, plug-ins ou extensões__.   Em muitos casos, as extensões do estilo COM provavelmente continuarão a funcionar desde que a extensão não tenha sido empacotada e seja instalada com confiança total. Isso ocorre porque esses instaladores podem usar seus recursos de confiança total para modificar o registro e posicionar os arquivos de extensão onde quer que seu aplicativo host os encontre.

   No entanto, se essas extensões forem empacotadas e, em seguida, instaladas como um pacote de aplicativo do Windows, elas não funcionarão porque cada pacote (o aplicativo host e a extensão) será isolado um do outro. Para ler mais sobre como os aplicativos são isolados do sistema, consulte nos [bastidores da ponte de desktop](desktop-to-uwp-behind-the-scenes.md).

 Todos os aplicativos e extensões que os usuários instalam em um sistema que executa o Windows 10 S devem ser instalados como pacotes do aplicativo do Windows. Portanto, se você pretende empacotar suas extensões ou planeja incentivar seus colaboradores a empacotá-las, considere como você pode facilitar a comunicação entre o pacote de aplicativo host e quaisquer pacotes de extensão. Uma maneira que você pode ser capaz de fazer isso é usando um [serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/app-services).

+ __Seu aplicativo gera código__. Seu aplicativo pode gerar código que consome na memória, mas evite escrever código gerado em disco porque o processo de certificação de aplicativo do Windows não pode validar esse código antes do envio do aplicativo. Além disso, os aplicativos que gravam o código no disco não serão executados corretamente em sistemas que executam o Windows 10 S. Isso pode impedir que seu aplicativo seja enviado para o Microsoft Store porque todos os aplicativos enviados para o Microsoft Store devem ser compatíveis com o Windows 10 S.

>[!IMPORTANT]
> Depois de criar seu pacote de aplicativo do Windows, teste seu aplicativo para garantir que ele funcione corretamente em sistemas que executam o Windows 10 S. Todos os aplicativos enviados para o Microsoft Store devem ser compatíveis com o Windows 10 S. os aplicativos que não são compatíveis não serão aceitos no repositório. Consulte [Testar seu aplicativo do Windows para o Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Criar um pacote de aplicativos do Windows para seu aplicativo de desktop**

Consulte [Criar um pacote de aplicativo do Windows](desktop-to-uwp-root.md#convert)
