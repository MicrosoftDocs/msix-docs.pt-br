---
Description: Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo da área de trabalho. Talvez você não precise fazer muito para preparar seu app para o processo de empacotamento.
title: Preparar para empacotar um aplicativo da área de trabalho (ponte de Desktop)
ms.date: 07/03/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10, UWP, MSIX
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 338ac2a974905670f8223045acaeaa2a73f14e33
ms.sourcegitcommit: e90f6c00a1ecbd6af2cfef206f233a21831285b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557916"
---
# <a name="prepare-to-package-a-desktop-application"></a>Preparar um pacote para um aplicativo de área de trabalho

Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo da área de trabalho. Talvez você não precise fazer muito para preparar seu aplicativo para o processo de empacotamento, mas se qualquer um dos itens a seguir aplica-se ao seu aplicativo, você precisará solucioná-lo antes do empacotamento. Lembre-se de que a Microsoft Store lida com o licenciamento e a atualização automática para você, portanto, você pode remover qualquer recurso relacionado a essas tarefas de seu código base.

+ __Seu aplicativo requer uma versão do .NET anteriores a 4.6.2__. Você precisa certificar-se de que seu aplicativo é executado no .NET 4.6.2. Você não pode exigir ou redistribuir versões anteriores a 4.6.2. Esta é a versão do .NET fornecida na Atualização de Aniversário do Windows 10. Verificar que seu aplicativo funcione com esta versão garantirá que seu aplicativo continuará a ser compatível com as atualizações futuras do Windows 10.  Se seu aplicativo tem como alvo o .NET Framework 4.0 ou posterior, espera-se para ser executado no .NET 4.6.2, mas você ainda deverá testá-lo.

+ __Seu aplicativo sempre é executado com privilégios de segurança elevada__. Seu aplicativo precisa trabalhar durante a execução como usuário interativo. Os usuários que instalam o Microsoft Store o seu aplicativo podem não ser administradores de sistema, portanto, exigir que o aplicativo seja executado com privilégios elevados significa que ele não será executado corretamente para usuários padrão. Os aplicativos que exigem a elevação para qualquer parte da funcionalidade não serão aceitos na Microsoft Store.

+ __Seu aplicativo requer um driver de modo kernel ou um serviço Windows__. A ponte é adequada para um aplicativo, mas não dá suporte a um driver de modo kernel ou um serviço Windows que precisa ser executado em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do seu aplicativo são carregados em processo, para processos que não estão no seu pacote do aplicativo do Windows__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Seu aplicativo usa uma ID modelo de usuário de aplicativo personalizado (AUMID)__ . Se o seu processo de chamar [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir sua própria AUMID, em seguida, ele só pode usar o AUMID gerado para ela pelo pacote de aplicativo do ambiente/Windows de modelo de aplicativo. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do Registro HKEY_LOCAL_MACHINE (HKLM)__ . Qualquer tentativa feita pelo seu aplicativo para criar uma chave em HKLM, ou para abrir um para modificação, resultará em falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada privada do registro, portanto, não se aplicam a noção de um hive do registro de usuário - e todo o computador (que é o que é HKLM). Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __Seu aplicativo usa uma subchave do registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Seu aplicativo grava na pasta AppData ou no registro com a intenção de compartilhar dados com outro aplicativo__. Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo UWP.

  Todas as entradas que seu aplicativo grava o hive HKEY_LOCAL_MACHINE do registro são redirecionadas para um arquivo binário isolado e todas as entradas que seu aplicativo grava o hive do registro HKEY_CURRENT_USER são colocadas em particular por usuário, local por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivos e do Registro, consulte [Bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).  

  Use uma maneira diferente de compartilhamento de dados entre processos. Para saber mais, consulte [Armazene e recupere configurações e outros dados de aplicativos](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Seu aplicativo grava no diretório de instalação para seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você coloca no mesmo diretório que o exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __A instalação do aplicativo requer interação do usuário__. Instalador de seu aplicativo deve ser capaz de executar silenciosamente, e ele deve instalar todos os pré-requisitos que não estão em por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo usa o diretório de trabalho atual__. Em tempo de execução, seu aplicativo da área de trabalho de pacote não obterá o mesmo diretório de trabalho que você especificou anteriormente em sua área de trabalho. Atalho LNK. Você precisará alterar seu CWD em tempo de execução se ter o diretório correto é importante para seu aplicativo para funcionar corretamente.

+ __Seu aplicativo requer UIAccess__. Se o seu aplicativo especifica `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto UAC, a conversão em UWP não será possível no momento. Para obter mais informações, consulte [Visão geral sobre a Automação da Interface do Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Seu aplicativo expõe objetos COM__. Processos e extensões de dentro do pacote podem registrar e usar servidores COM & OLE, tanto dentro do processo como fora do processo (OOP).  A Atualização dos Criadores adiciona suporte para COM integrados, o que fornece a habilidade de registrar servidores OOP COM & OLE que estão agora visíveis fora do pacote.  Consulte [Suporte ao Servido COM e ao Documento OLE para Ponte de Desktop](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge).

   O suporte ao COM Integrado funciona para APIs COM existentes, mas não funcionará para extensões de aplicativo que dependem da leitura direta do registro, uma vez que a localização do COM Integrado é privada.

+ __Seu aplicativo expõe os assemblies do GAC para uso por outros processos__. Na versão atual, seu aplicativo não pode expor os assemblies do GAC para uso por processos provenientes de executáveis externos ao seu pacote de aplicativo do Windows. Os processos de dentro do pacote podem registrar e usar conjuntos GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos.

+ __Seu aplicativo está vinculando as bibliotecas de tempo de execução C (CRT) de uma maneira sem suporte__. A biblioteca de tempo de execução C/C++ da Microsoft oferece rotinas de programação para o sistema operacional Microsoft Windows. Essas rotinas automatizam muitas tarefas comuns de programação que não são fornecidas pelas linguagens C e C++. Se seu aplicativo utiliza a biblioteca de tempo de execução do C/C++, você precisa garantir que ele é vinculado de maneira com suporte.

    O Visual Studio 2017 oferece suporte à vinculação estática e dinâmica para permitir que o código use arquivos DLL comuns, ou a vinculação estática, a fim de vincular a biblioteca diretamente no código para a versão atual da CRT. Se possível, recomendamos o uso de aplicativos dinâmicos de vinculação com o VS 2017.

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

    Observação: Em todos os casos, você deve vincular para o versão mais recente disponível publicamente CRT.

+ __Seu aplicativo instala e carrega os assemblies da pasta Windows side-by-side__. Por exemplo, seu aplicativo usa bibliotecas de tempo de execução C VC8 ou VC9 e dinamicamente é vinculando-as na pasta de lado a lado do Windows, que significa que seu código está usando os arquivos DLL comuns de uma pasta compartilhada. Isso não tem suporte. Você precisará vinculá-las de forma estática vinculando-as aos arquivos da biblioteca redistribuível diretamente no código.

+ __Seu aplicativo usa uma dependência na pasta System32/SysWOW64__. Para fazer essas DLLs funcionarem, você deve incluí-las na parte do sistema de arquivos virtual do pacote de aplicativo do Windows. Isso garante que o aplicativo se comporta como se as DLLs foram instaladas na **System32**/**SysWOW64** pasta. Na raiz do pacote, crie uma pasta chamada **VFS**. Dentro dessa pasta, crie uma pasta **SystemX64** e uma **SystemX86**. Em seguida, coloque a versão de 32 bits de sua DLL na pasta **SystemX86** e a versão de 64 bits na pasta **SystemX64**.

+ __Seu app usa um pacote de estrutura VCLibs__. As bibliotecas VCLibs podem ser instaladas diretamente da Microsoft Store, caso estejam definidas como uma dependência no pacote de aplicativo do Windows. Por exemplo, se seu aplicativo usa pacotes de Dev11 VCLibs, faça a seguinte alteração ao manifesto de pacote do aplicativo: Sob o `<Dependencies>` nó, adicione:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante a instalação a partir da Microsoft Store, a versão apropriada (x86 ou x64) da estrutura VCLibs será instalada antes da instalação do app.  
As dependências não serão instaladas se o aplicativo é instalado por sideload. Para instalar as dependências manualmente no seu computador, você deve baixar e instalar o pacote de estrutura VCLibs apropriado para Ponte de Desktop. Para obter mais informações sobre esses cenários, consulte [Usando o Visual C++ Runtime em um projeto Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

  **Pacotes de estrutura**:

  * [VC 14.0 pacotes da estrutura para a ponte de Desktop](https://www.microsoft.com/download/details.aspx?id=53175)
  * [VC 12.0 pacotes da estrutura para a ponte de Desktop](https://www.microsoft.com/download/details.aspx?id=53176)
  * [Pacotes do framework 11.0 VC para a ponte de Desktop](https://www.microsoft.com/download/details.aspx?id=53340)


+ __Seu aplicativo contém uma lista de atalhos personalizados__. Há vários problemas e limitações a que é preciso estar atento ao usar listas de atalhos.

    - __Arquitetura do aplicativo não coincide com o sistema operacional.__  As listas de atalhos no momento, não funcionam corretamente se o aplicativo e arquiteturas de sistema operacional não corresponderem (por exemplo, x86 aplicativo em execução em x64 Windows). Neste momento, nenhuma ação é diferente de recompilar seu aplicativo para a arquitetura correspondente.

    - __Seu aplicativo cria entradas de lista de atalhos e chama [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__ . Não defina AppID de forma programática no código. Isso fará com que as entradas da lista de atalhos não sejam exibidas. Se seu aplicativo precisa de uma Id personalizada, especifique-o usando o arquivo de manifesto. Consulte a [empacotar um aplicativo de área de trabalho manualmente](desktop-to-uwp-manual-conversion.md) para obter instruções. A AppID de seu aplicativo é especificada na seção *YOUR_PRAID_HERE*.

    - __Seu aplicativo adiciona um link de shell de lista de atalhos que faz referência a um executável em seu pacote__. Você não pode iniciar executáveis diretamente em seu pacote a partir de uma lista de atalhos (com a exceção do caminho absoluto do .exe do próprio aplicativo). Em vez disso, registre um alias de execução do aplicativo (que permite que seu aplicativo empacotado da área de trabalho Iniciar por meio de uma palavra-chave como se estivesse no caminho) e defina o caminho de destino do link para o alias em vez disso. Para obter detalhes sobre como usar a extensão appExecutionAlias, consulte [integrar seu aplicativo da área de trabalho com o Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions). Observe que, se você precisar que os ativos do link na lista de atalhos correspondam ao .exe original, defina os ativos, como o ícone, usando [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) e o nome de exibição com PKEY_Title como você faria para outras entradas personalizadas.

    - __Seu aplicativo adiciona entradas da lista um salto que faz referência a ativos no pacote do aplicativo por caminhos absolutos__. O caminho de instalação de um aplicativo pode ser alterado quando seus pacotes forem atualizados, alterando o local de ativos (como ícones, documentos, executável e assim por diante). Se entradas da lista de salto referenciam esses ativos por caminhos absolutos e, em seguida, o aplicativo deve atualizar sua lista de atalhos periodicamente o (por exemplo, no aplicativo inicie) para garantir a caminhos resolvidos corretamente. Como alternativa, use as APIs UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), que permitem referenciar ativos de cadeias de caracteres e imagens usando o esquema de URI package-relative ms-resource (que também reconhece linguagem, DPI e alto contraste).

+ __O aplicativo é iniciado um utilitário para executar tarefas__. Evite iniciar utilitários de comando, como o PowerShell e o Cmd.exe. Na verdade, se os usuários instalarem o aplicativo em um sistema que executa o Windows 10 S, em seguida, seu aplicativo não conseguirá iniciá-los em todos os. Isso pode impedir o aplicativo de envio para a Microsoft Store, porque todos os aplicativos enviados para a Microsoft Store devem ser compatíveis com o Windows 10 S.

Iniciar um utilitário pode geralmente fornecer uma maneira conveniente para obter informações do sistema operacional, acessar o registro ou acessar as funcionalidades do sistema. No entanto, você pode usar APIs UWP para realizar esses tipos de tarefas. Essas APIs são mais eficazes porque eles não precisam de um executável separado para executar, mas o mais importante, eles mantêm o aplicativo atinja fora do pacote. Design do aplicativo permaneça consistente com o isolamento, confiança e segurança que vem com um aplicativo que você já empacotados e seu aplicativo se comporte como esperado em sistemas que executam o Windows 10 S.

+ __Seu aplicativo hospeda extensões, plug-ins ou suplementos__.   Em muitos casos, as extensões do estilo COM provavelmente continuarão a funcionar desde que a extensão não tenha sido empacotada e seja instalada com confiança total. Isso ocorre porque esses instaladores podem usar seus recursos de confiança total para modificar o registro e colocar arquivos de extensão, onde quer que seu aplicativo host espera encontrá-los.

   No entanto, se essas extensões são empacotadas e, em seguida, instaladas como um pacote de aplicativo do Windows, eles não funcionarão porque cada pacote (o aplicativo host e a extensão) serão isolado uns dos outros. Para ler mais sobre como os aplicativos são isolados do sistema, consulte [nos bastidores da ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).

 Todos os aplicativos e extensões que os usuários instalam em um sistema que executa o Windows 10 S devem ser instalados como pacotes do aplicativo do Windows. Portanto, se você pretende compactar as extensões ou se planejar incentivar seus colaboradores empacotá-las, considere como você pode facilitar a comunicação entre o pacote de aplicativo de host e os pacotes de extensão. Uma maneira que você pode ser capaz de fazer isso é usando um [serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/app-services).

+ __Seu aplicativo gera o código__. Seu aplicativo pode gerar um código que consome na memória, mas evite gerado de escrever código para o disco, porque o processo de certificação de aplicativos do Windows não pode validar que o código antes do envio de aplicativo. Além disso, os aplicativos que escrever código para o disco não será executado corretamente em sistemas que executam o Windows 10 S. Isso pode impedir o aplicativo de envio para a Microsoft Store, porque todos os aplicativos enviados para a Microsoft Store devem ser compatíveis com o Windows 10 S.

>[!IMPORTANT]
> Depois de criar seu pacote de aplicativos do Windows, testar o seu aplicativo para garantir que ele funcione corretamente em sistemas que executam o Windows 10 S. Todos os aplicativos enviados para a Microsoft Store devem ser compatíveis com aplicativos do Windows 10 S. que não são compatíveis não será aceita no repositório. Consulte [Testar seu aplicativo do Windows para o Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Criar um pacote de aplicativo do Windows para seu aplicativo de desktop**

Consulte [Criar um pacote de aplicativo do Windows](desktop-to-uwp-root.md#convert)
