---
Description: Este artigo lista o que você precisa saber antes de empacotar o aplicativo da área de trabalho. Talvez não seja necessário fazer muito para preparar o aplicativo para o processo de empacotamento.
title: Preparar para empacotar um aplicativo da área de trabalho (MSIX)
ms.date: 08/22/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 432017a083ae3f9553bea88902378ca3e6556bcb
ms.sourcegitcommit: a7f677e024e415168f8bf0080f4e115307833a1d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77576913"
---
# <a name="prepare-to-package-a-desktop-application"></a>Preparar um pacote para um aplicativo de área de trabalho

Este artigo lista o que você precisa saber antes de empacotar o aplicativo da área de trabalho. Poderá não ser necessário fazer muito para preparar o aplicativo para o processo de empacotamento, mas se qualquer um dos itens abaixo se aplica ao aplicativo, será necessário lidar com isso antes do empacotamento.

+ __Seu aplicativo .NET precisa de uma versão do .NET Framework anterior à 4.6.2__. Se estiver empacotando um aplicativo .NET, recomendamos que seu aplicativo dê preferência ao .NET Framework 4.6.2 ou posterior. A capacidade de instalar e executar aplicativos da área de trabalho empacotados apareceu pela primeira vez no Windows 10, versão 1607 (também chamada de Atualização de Aniversário) e essa versão do sistema operacional inclui o .NET Framework 4.6.2 por padrão. Versões posteriores do sistema operacional incluem versões posteriores do .NET Framework. Para obter uma lista completa das versões do .NET incluídas nas versões posteriores do Windows 10, confira [este artigo](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies).

  Espera-se que a preferência por versões do .NET Framework anteriores à 4.6.2 funcione na maioria dos casos. No entanto, caso dê preferência a uma versão anterior à 4.6.2, deverá testá-la completamente no aplicativo de área de trabalho empacotado antes da distribuição para os usuários.

  + 4.0 - 4.6.1: Espera-se que esses aplicativos que visam essas versões do .NET Framework funcionem sem problemas na versão 4.6.2 ou posteriores. Portanto, esses aplicativos devem ser instalados e executados sem alterações no Windows 10, versão 1607 ou posteriores com a versão do .NET Framework incluída no sistema operacional.

  + 2.0 e 3.5: Em nossos testes, os aplicativos de área de trabalho empacotados que visam essas versões do .NET Framework geralmente funcionam, mas podem apresentar problemas de desempenho em alguns cenários. Para que esses aplicativos empacotados sejam instalados e funcionem, o [recurso .NET Framework 3.5](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10) precisa estar instalado no computador de destino (esse recurso também inclui o .NET Framework 2.0 e 3.0). Você também deve testar esses aplicativos totalmente depois de empacotá-los.

+ __O aplicativo sempre é executado com privilégios de segurança elevados__. O aplicativo precisa funcionar sendo executado como o usuário interativo. Os usuários que instalam o aplicativo poderão não ser administradores de sistema, então exigir privilégios elevados para o aplicativo significa que ele não funcionará corretamente para usuários padrão. Caso planeje publicar o aplicativo na Microsoft Store, aplicativos que precisem de elevação de qualquer parte da funcionalidade não serão aceitos na Store.

+ __O aplicativo precisa de um driver de modo kernel ou de um serviço do Windows__. O MSIX não é compatível com um driver de modo kernel ou com um serviço do Windows que precisa funcionar em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do aplicativo são carregados no processo em processos que não estão no pacote do aplicativo do Windows__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Garanta que todas as extensões instaladas pelo aplicativo serão instaladas no local em que o aplicativo for instalado__. O Windows permite a usuários e gerentes de TI alterar o local de instalação padrão dos pacotes.  Veja "Configurações->Sistema->Armazenamento->Mais Configurações de Armazenamento-> Alterar onde o novo conteúdo é salvo -> Novos aplicativos serão salvos em".  Caso esteja instalando uma extensão com o aplicativo, garanta que ela não tenha restrições adicionais de pasta de instalação.  Por exemplo, algumas extensões podem desativar a instalação da extensão em pastas que não sejam do sistema.  Isso resultará em um erro 0x80073D01 (ERROR_DEPLOYMENT_BLOCKED_BY_POLICY) caso o local padrão tenha sido alterado. 

+ __O aplicativo usa uma AUMID (ID do Modelo do Usuário do Aplicativo)__ . Se o processo chamar [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir sua própria AUMID, ele poderá usar apenas a AUMID gerada para ele pelo ambiente modelo/pacote do aplicativo do Windows. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do Registro HKEY_LOCAL_MACHINE (HKLM)__ . Qualquer tentativa do aplicativo de criar uma chave HKLM, ou de abrir uma para modificação, resultará em uma falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada privada do Registro, então a noção de um hive de Registro no âmbito do usuário e do computador (que é do que se trata o HKLM) não se aplica. Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __O aplicativo usa uma subchave de Registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __O aplicativo grava na pasta AppData ou no Registro com a intenção de compartilhar os dados com outro aplicativo__. Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo.

  Todas as entradas que o aplicativo grava para o hive de Registro HKEY_LOCAL_MACHINE são redirecionadas para um arquivo binário isolado e as possíveis entradas que o aplicativo grava no hive de Registro HKEY_CURRENT_USER são colocadas em um local privado por usuário e por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivo e Registro, consulte [Bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).  

  Use uma maneira diferente de compartilhamento de dados entre processos. Para saber mais, consulte [Armazene e recupere configurações e outros dados de aplicativos](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __O aplicativo grava no diretório de instalação do aplicativo__. Por exemplo, o aplicativo grava para um arquivo de log que você coloca no mesmo diretório do exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __Seu aplicativo usa o diretório de trabalho atual__. No tempo de execução, o aplicativo da área de trabalho empacotado não terá o mesmo diretório de trabalho especificado anteriormente no atalho .LNK da área de trabalho. Você precisa alterar o CWD no tempo de execução caso ter o diretório correto for importante para que o aplicativo funcione corretamente.

  > [!NOTE]
  > Caso o aplicativo precise ser gravado no diretório de instalação ou usar o diretório de trabalho atual, também será possível considerar adicionar uma correção de tempo de execução usando a [Estrutura de Suporte do Pacote](https://github.com/microsoft/MSIX-PackageSupportFramework) do pacote. Para obter mais detalhes, veja [este artigo](../psf/package-support-framework.md). 

+ __A instalação do aplicativo precisa da interação do usuário__. O instalador do aplicativo precisa poder ser executado silenciosamente e precisa instalar todos os pré-requisitos que não estejam por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo requer UIAccess__. Se o aplicativo especifica `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto UAC, a conversão em MSIX é compatível no momento. Para obter mais informações, consulte [Visão geral sobre a Automação da Interface do Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __O aplicativo expõe objetos COM__. Os processos e as extensões no pacote podem registrar e usar servidores COM & OLE, tanto dentro quanto fora do processo (OOP).  A Atualização para Criadores adiciona suporte a COM integrados, o que oferece a capacidade de registrar servidores OOP COM & OLE que agora ficam visíveis fora do pacote.  Veja [Suporte ao Servidor COM e ao Documento OLE para Ponte de Desktop](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge).

   O suporte ao COM Integrado funciona com APIs COM existentes, mas não funcionará em extensões de aplicativo que dependem da leitura direta do Registro, uma vez que a localização do COM Integrado é privada.

+ __O aplicativo expõe assemblies do GAC para serem usados por outros processos__. O aplicativo não pode expor conjuntos GAC para uso por processos originados de executáveis externos ao pacote do aplicativo do Windows. Os processos de dentro do pacote podem registrar e usar conjuntos GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos.

+ __O aplicativo está vinculando bibliotecas de runtime C (CRT) de maneira incompatível__. A biblioteca de runtime C/C++ da Microsoft oferece rotinas de programação para o sistema operacional Microsoft Windows. Essas rotinas automatizam muitas tarefas comuns de programação que não são fornecidas pelas linguagens C e C++. Caso o aplicativo use a biblioteca de runtime C/C++, você precisará garantir que ela seja vinculada de maneira compatível.

    O Visual Studio 2017 oferece suporte à vinculação estática e dinâmica para permitir que o código use arquivos DLL comuns, ou a vinculação estática, para vincular a biblioteca diretamente no código, para a versão atual da CRT. Se possível, recomendamos que o aplicativo use vinculação dinâmica com VS 2017.

    O suporte nas versões anteriores do Visual Studio varia. Para obter detalhes, consulte a tabela a seguir:

    <table>
    <th>Versão do Visual Studio</td><th>Vinculação dinâmica</th><th>Vinculação estática</th></th>
    <tr><td>2005 (VC 8)</td><td>Sem suporte</td><td>Suportado</td>
    <tr><td>2008 (VC 9)</td><td>Sem suporte</td><td>Suportado</td>
    <tr><td>2010 (VC 10)</td><td>Suportado</td><td>Suportado</td>
    <tr><td>2012 (VC 11)</td><td>Suportado</td><td>Sem suporte</td>
    <tr><td>2013 (VC 12)</td><td>Suportado</td><td>Sem suporte</td>
    <tr><td>2015 e 2017 (VC 14)</td><td>Suportado</td><td>Suportado</td>
    </table>

    Observação: Em todos os casos, você deve vincular à última CRT publicamente disponível.

+ __Seu aplicativo instala e carrega os assemblies da pasta lado a lado do Windows__. Por exemplo, o aplicativo usa bibliotecas de runtime C VC8 ou VC9 e as vincula dinamicamente da pasta lado a lado do Windows, ou seja, o código está usando os arquivos DLL comuns de uma pasta compartilhada. Isso não tem suporte. Você precisará vinculá-las de forma estática vinculando-as aos arquivos da biblioteca redistribuível diretamente no código.

+ __O aplicativo usa uma dependência na pasta System32/SysWOW64__. Para fazer essas DLLs funcionarem, você deve incluí-las na parte do sistema de arquivos virtual do pacote de aplicativo do Windows. Isso garante que o aplicativo se comporte como se as DLLs tivessem sido instaladas na pasta **System32**/**SysWOW64**. Na raiz do pacote, crie uma pasta chamada **VFS**. Dentro dessa pasta, crie uma pasta **SystemX64** e uma **SystemX86**. Em seguida, coloque a versão de 32 bits de sua DLL na pasta **SystemX86** e a versão de 64 bits na pasta **SystemX64**.

+ __O aplicativo usa um pacote de estrutura VCLibs__. Caso esteja convertendo um aplicativo C++ Win32, será necessário lidar com a implantação do Visual C++ Runtime. O SDK do Windows e o Visual Studio 2019 incluem os pacotes de estrutura mais recentes das versões 11.0, 12.0 e 14.0 do Visual C++ Runtime nas seguintes pastas:

    * **Pacotes de estrutura VC 14.0**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop\14.0

    * **Pacotes de estrutura VC 12.0**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.120\14.0

    * **Pacotes de estrutura VC 11.0**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.110\14.0

    Para usar um desses pacotes, você deve fazer referência a ele como uma dependência no manifesto do pacote. Quando os clientes instalam a versão de varejo do aplicativo pela Microsoft Store, o pacote será instalado pela Store juntamente com o aplicativo. As dependências não serão instaladas caso você carregue o aplicativo em outro local. Para instalar as dependências manualmente, você precisa instalar o pacote de estrutura correto usando o pacote de aplicativo apropriado para x86, x64 ou ARM localizado nas pastas de instalação listadas acima.

    Para fazer referência a um pacote de estrutura Visual C++ Runtime no aplicativo:

    1. Acesse a pasta de instalação do pacote de estrutura listada acima da versão do Visual C++ Runtime usada pelo aplicativo.

    2. Abra o arquivo SDKManifest.xml na pasta, localize o atributo `FrameworkIdentity-Debug` ou `FrameworkIdentity-Retail` (dependendo se está usando a versão de depuração ou de varejo do tempo de execução) e copie os valores `Name` e `MinVersion` desse atributo. Por exemplo, este é o atributo `FrameworkIdentity-Retail` do pacote de estrutura VC 14.0 atual.
        ```xml
        FrameworkIdentity-Retail = "Name = Microsoft.VCLibs.140.00.UWPDesktop, MinVersion = 14.0.27323.0, Publisher = 'CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US'"
        ```

    3. No manifesto do pacote do aplicativo, adicione o seguinte elemento `<PackageDependency>` no nó `<Dependencies>`. Assegure-se de substituir os valores `Name` e `MinVersion` pelos valores copiados na etapa anterior. O exemplo a seguir especifica uma dependência da versão atual do pacote de estrutura VC 14.0.
        ```xml
        <PackageDependency Name="Microsoft.VCLibs.140.00.UWPDesktop" MinVersion="14.0.27323.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
        ```

+ __O aplicativo contém uma lista de atalhos personalizada__. Ao usar a lista de atalhos, é importante ter em mente vários problemas e limitações.

    - __A arquitetura do aplicativo não corresponde ao sistema operacional.__  No momento, as listas de atalhos não funcionam corretamente se as arquiteturas do aplicativo e do sistema operacional não correspondem (por exemplo, um aplicativo x86 em execução no Windows x64). Atualmente não há uma solução alternativa diferente da recompilação do aplicativo para a arquitetura correspondente.

    - __O aplicativo cria entradas da lista de atalhos e chama [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__ . Não defina AppID de forma programática no código. Isso fará com que as entradas da lista de atalhos não sejam exibidas. Se o aplicativo precisar de uma ID personalizada, especifique-a usando o arquivo de manifesto. Veja [Empacotar manualmente um aplicativo da área de trabalho](desktop-to-uwp-manual-conversion.md) para obter instruções. A AppID de seu aplicativo é especificada na seção *YOUR_PRAID_HERE*.

    - __O aplicativo adiciona um link de shell de lista de atalhos que faz referência a um executável no pacote__. Você não pode iniciar executáveis diretamente em seu pacote a partir de uma lista de atalhos (com a exceção do caminho absoluto do .exe do próprio aplicativo). Registre um alias de execução do aplicativo (que permite que o aplicativo da área de trabalho empacotado seja iniciado por uma palavra-chave como se estivesse no PATH) e, em vez disso, defina o caminho de destino do link para o alias. Para obter detalhes sobre como usar a extensão appExecutionAlias, consulte [Integrar o aplicativo da área de trabalho com o Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions). Observe que, se você precisar que os ativos do link na lista de atalhos correspondam ao .exe original, defina os ativos, como o ícone, usando [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) e o nome de exibição com PKEY_Title como você faria para outras entradas personalizadas.

    - __O aplicativo adiciona entradas de lista de atalhos que fazem referência a ativos no pacote do aplicativo por caminhos absolutos__. O caminho de instalação de um aplicativo pode mudar quando os pacotes dele são atualizados, alterando o local dos ativos (como ícones, documentos, executáveis e assim por diante). Se as entradas da lista de atalhos fazem referência a esses ativos por caminhos absolutos, o aplicativo deve atualizar sua lista de atalhos periodicamente (por exemplo, na inicialização do aplicativo) para garantir que os caminhos sejam resolvidos corretamente. Como alternativa, use as APIs UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), que permitem referenciar ativos de cadeias de caracteres e imagens usando o esquema de URI package-relative ms-resource (que também reconhece linguagem, DPI e alto contraste).

+ __O aplicativo inicia um utilitário para executar tarefas__. Evite iniciar utilitários de comando, como o PowerShell e o Cmd.exe. Na verdade, se os usuários instalarem o aplicativo em um sistema com o Windows 10 S, o aplicativo não será capaz de iniciá-los. Isso pode bloquear o envio do aplicativo para a Microsoft Store porque todos os aplicativos enviados para a Microsoft Store devem ser compatíveis com o Windows 10 S.

     Com frequência, iniciar um utilitário pode possibilitar uma forma conveniente de obter informações do sistema operacional, acessar o registro ou acessar as funcionalidades do sistema. No entanto, em vez disso, você pode usar APIs UWP para realizar esses tipos de tarefas. Essas APIs são mais eficientes porque não precisam de um executável separado para funcionarem mas, mais importante, elas evitam que o aplicativo saia do pacote. O design do aplicativo continua de acordo com o isolamento, a confiança e a segurança derivadas do aplicativo empacotado e o aplicativo vai se comportar como esperado nos sistemas com o Windows 10 S.

+ __O aplicativo tem suplementos, plug-ins ou extensões__.   Em muitos casos, as extensões do tipo COM provavelmente continuarão a funcionar desde que a extensão não tenha sido empacotada e seja instalada com confiança total. Isso acontece porque esses instaladores podem usar os recursos de confiança total para modificar o Registro e colocar arquivos de extensão no local em que o aplicativo host espera encontrá-los.

   Entretanto, se essas extensões forem empacotadas e instaladas como um pacote de aplicativo do Windows, elas não funcionarão já que cada pacote (o aplicativo host e a extensão) estará isolado um do outro. Para saber mais sobre como os aplicativos são isolados do sistema, consulte [Bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).

     Todos os aplicativos e extensões que os usuários instalam em um sistema com o Windows 10 S devem ser instalados como pacotes do Aplicativo do Windows. Portanto, se pretende empacotar suas extensões ou planeja incentivar colaboradores a empacotá-las, considere como você pode facilitar a comunicação entre o pacote do aplicativo host e qualquer outro pacote de extensão. Uma forma de conseguir fazer isso é usar um [serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/app-services).

+ __Seu aplicativo gera código__. O aplicativo pode gerar código que é consumido na memória, mas evita gravar código gerado no disco porque o processo de Certificação de Aplicativos Windows não pode validar o código antes do envio do aplicativo. Além disso, os aplicativos que gravam código no disco não funcionarão corretamente no Windows 10 S. Isso pode impedir que o aplicativo seja enviado para a Microsoft Store, já que todos os aplicativos enviados para a Microsoft Store precisam ser compatíveis com o Windows 10 S.

>[!IMPORTANT]
> Depois de criar o pacote do aplicativo do Windows, teste o aplicativo para garantir que ele funcione corretamente em sistemas com Windows 10 S. Todos os aplicativos enviados para a Microsoft Store precisam ser compatíveis com o Windows 10 S. Os aplicativos que não são compatíveis não serão aceitos na Microsoft Store. Veja [Testar o aplicativo do Windows para o Windows 10 S](desktop-to-uwp-test-windows-s.md).