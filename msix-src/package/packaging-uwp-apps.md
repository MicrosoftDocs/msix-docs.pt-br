---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empacotando aplicativos UWP
description: Para distribuir ou vender seu aplicativo UWP (Plataforma Universal do Windows), será necessário criar um pacote de apps para ele.
ms.date: 07/18/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: c876ad4311b9f7a05ed748cd866c7cc88eccc1fd
ms.sourcegitcommit: 5e253ff41330ac0cd6f85ed481da307713277eee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71210374"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>União de um aplicativo UWP com o Visual Studio

Para vender seu aplicativo UWP (Plataforma Universal do Windows) ou distribuí-lo para outros usuários, você precisa empacotá-lo. Se você não quiser distribuir seu app por meio da Microsoft Store, poderá fazer o sideload do pacote do app diretamente para um dispositivo ou distribuí-lo através de [Instalação Web](../app-installer/installing-windows10-apps-web.md). Este artigo descreve o processo de configuração, criação e teste de um pacote do aplicativo UWP usando o Visual Studio. Para obter mais informações sobre como gerenciar e implantar aplicativos da linha de negócios (LOB), consulte [Gerenciamento de aplicativos corporativos](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

No Windows 10, você pode enviar um pacote do aplicativo, um pacote de aplicativos ou um arquivo de carregamento completo do pacote de aplicativo para o [Partner Center](https://partner.microsoft.com/dashboard). Dessas opções, o envio de um arquivo de upload de pacote de aplicativo fornecerá a melhor experiência.

## <a name="types-of-app-packages"></a>Tipos de pacotes de aplicativo

- **Pacote do aplicativo (. msix ou. AppX)**  
    Um arquivo que contém seu aplicativo em um formato que pode ser adicionado por sideload em um dispositivo. Qualquer arquivo de pacote de aplicativo único criado pelo Visual Studio **não** deve ser enviado para o Partner Center e deve ser usado apenas para fins de Sideload e teste. Se você quiser enviar seu aplicativo para o Partner Center, use o arquivo de carregamento do pacote do aplicativo.  

- **Pacote de aplicativo (. msixbundle ou. appxbundle)**  
    Um lote de aplicativo é um tipo de pacote que pode conter vários pacotes de aplicativos, cada um deles é criado para dar suporte a uma arquitetura de dispositivo específico. Por exemplo, um lote de aplicativo pode conter três pacotes de aplicativo separado para configurações x86, x64 e ARM. Lotes de aplicativo devem ser gerados sempre que possível, pois eles permitem que seu aplicativo esteja disponível na maior variedade possível de dispositivos.  

- **Arquivo de carregamento do pacote do aplicativo (. msixupload ou. appxupload)**  
    Um único arquivo que pode conter vários pacotes de aplicativos ou um lote de aplicativo para dar suporte a várias arquiteturas de processador. O arquivo de upload do pacote do aplicativo também contém um arquivo de símbolo para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) depois que o aplicativo tiver sido publicado no Microsoft Store. Esse arquivo será criado automaticamente para você se você estiver empacotando seu aplicativo com o Visual Studio com a intenção de enviá-lo ao Partner Center para publicação.

Aqui está uma visão geral das etapas de preparação e de criação de um pacote do app:

1. [Antes de empacotar seu aplicativo](#before-packaging-your-app). Siga estas etapas para garantir que seu aplicativo esteja pronto para ser empacotado para o envio do Partner Center.

2. [Configure um pacote do aplicativo](#configure-an-app-package). Use o designer de manifesto do Visual Studio para configurar o pacote. Por exemplo, adicione imagens de bloco e escolha as orientações compatíveis com o aplicativo.

3. [Crie um arquivo de upload de pacote do aplicativo](#create-an-app-package-upload-file). Use o assistente de pacote de aplicativo do Visual Studio para criar um pacote do aplicativo e, em seguida, certificar o pacote com o Kit de Certificação de Aplicativos Windows.

4. [Fazer o sideload do pacote do aplicativo](#sideload-your-app-package). Depois do sideload do aplicativo para um dispositivo, você poderá testar se ele funciona como esperado.

Depois de concluir as etapas acima, você estará pronto para distribuir seu aplicativo. Se você tiver um aplicativo de linha de negócios (LOB) que não planeja vender porque ele é apenas para usuários internos, você pode Sideload esse aplicativo para instalá-lo em qualquer dispositivo Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empacotar o aplicativo

1. **Teste seu aplicativo.** Antes de empacotar seu aplicativo para o envio do Partner Center, verifique se ele funciona conforme o esperado em todas as famílias de dispositivos às quais você planeja dar suporte. Essas famílias de dispositivos podem incluir desktop, celular, Surface Hub, Xbox, dispositivos IoT ou outros. Para obter mais informações sobre como implantar e testar seu aplicativo usando o Visual Studio, consulte Implantando [e Depurando aplicativos UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

2. **Otimize seu aplicativo.** Você pode usar as ferramentas de criação de perfil e depuração do Visual Studio para otimizar o desempenho do aplicativo UWP. Por exemplo, a ferramenta de linha do tempo para capacidade de resposta da interface do usuário, a ferramenta de uso da memória, a ferramenta de uso da CPU e muito mais. Para obter mais informações sobre essas ferramentas, consulte o tópico [Tour sobre recurso de perfil](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).

3. **Verifique a compatibilidade de .NET Native (para C# vb e aplicativos).** Na Plataforma Universal do Windows, existe um compilador nativo que melhorará o desempenho do tempo de execução do app. Com essa alteração, você deverá testar seu aplicativo nesse ambiente de compilação. Por padrão, a configuração de build **Release** habilita a cadeia de ferramentas .NET Native, então é importante testar seu aplicativo com essa configuração **Release** e verificar se seu aplicativo se comporta como o esperado. Alguns problemas de depuração comuns que podem acontecer com o .NET Native serão explicados mais detalhadamente em [Depuração de aplicativos universais do Windows do .NET Native](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/).

## <a name="configure-an-app-package"></a>Configurar um pacote do aplicativo

O arquivo de manifesto do app (Package.appxmanifest.xml) é um arquivo XML que contém as propriedades e as configurações necessárias para criar o pacote do app. Por exemplo, as propriedades no arquivo de manifesto do aplicativo descrevem a imagem a ser usada como o bloco do aplicativo e as orientações compatíveis com o aplicativo quando um usuário gira o dispositivo.

O designer de manifesto do Visual Studio permite a atualização do arquivo de manifesto sem a edição do XML bruto do arquivo.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configure um pacote com o designer de manifesto

1. Em **Gerenciador de soluções**, expanda o nó do projeto do seu aplicativo UWP.

2. Clique duas vezes no arquivo **Package.appxmanifest**. Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.

3. Agora é possível decidir como configurar o aplicativo. Cada guia contém informações que você pode configurar sobre o aplicativo e links para obter mais informações, se necessário.

    ![Designer de manifesto do Visual Studio](images/packaging-screen1.jpg)

    Verifique se você tem todas as imagens exigidas para um aplicativo UWP na guia **Ativos visuais**.

    Da guia **Empacotamento**, você pode inserir dados de publicação. É aqui que você pode escolher qual certificado usar para assinar seu aplicativo. Todos os aplicativos UWP devem ser assinados com um certificado.

    > [!NOTE]
    > A partir do Visual Studio 2019, um certificado temporário não é mais gerado em projetos UWP. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos neste [artigo](create-certificate-package-signing.md).

    > [!IMPORTANT]
    > Se você estiver publicando seu aplicativo na Microsoft Store, seu aplicativo será assinado com um certificado confiável para você. Isso permite que o usuário instale e execute seu aplicativo sem precisar instalar o certificado de autenticação do aplicativo associado.

    Se você não estiver publicando seu aplicativo e deseja simplesmente carregar um pacote do aplicativo, você precisa confiar no pacote. Para confiar o pacote, o certificado deve estar instalado no dispositivo do usuário. Para obter mais informações sobre o sideload, consulte [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4. Salve seu arquivo **Package.appxmanifest** depois de fazer as edições necessárias para o aplicativo.

Se você estiver distribuindo seu aplicativo por meio do Microsoft Store, o Visual Studio poderá associar seu pacote à loja. Para fazer isso, clique com o botão direito do mouse no nome do projeto em Gerenciador de soluções e escolha **publicar**->**aplicativo associado com a loja** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**). Você também pode fazer isso no Assistente para **criar pacotes de aplicativos** , que é descrito na seção a seguir. Quando você associa seu aplicativo, alguns dos campos na guia Empacotamento do designer de manifesto são atualizados automaticamente.

## <a name="create-an-app-package-upload-file"></a>Crie um arquivo de upload de pacote do aplicativo

Para distribuir um aplicativo por meio do Microsoft Store você deve criar um pacote de aplicativo (. Appx ou. msix), pacote de aplicativo (. appxbundle ou. msixbundle) ou um arquivo de carregamento de pacote de aplicativo (. appxupload ou. msixupload) e [enviar o aplicativo empacotado para o Partner Center](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Embora seja possível enviar um pacote do aplicativo ou um pacote de aplicativos para o Partner Center sozinho, recomendamos que você envie um arquivo de carregamento do pacote do aplicativo. Você pode criar um arquivo de carregamento de pacote de aplicativo usando o assistente para **criar pacotes de aplicativos** no Visual Studio, ou você pode criar um manualmente a partir de pacotes de aplicativo existentes ou de grupos de aplicativos.

>[!NOTE]
> Se você quiser criar um pacote de aplicativo (. Appx ou. msix) ou um pacote de aplicativo (. appxbundle ou. msixbundle) manualmente, consulte [criar um pacote de aplicativo com a ferramenta MakeAppx. exe](create-app-package-with-makeappx-tool.md).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Criar o arquivo de carregamento do pacote do aplicativo usando o Visual Studio

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo a partir do Visual Studio 2019 versão 16,3. Se você estiver usando uma versão anterior, parte da interface do usuário poderá parecer diferente.

1. Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.

2. Clique com o botão direito do mouse no projeto e escolha **publicar**->**criar pacotes de aplicativos** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**). Se essa opção estiver desabilitada ou não aparecer, verifique se o projeto é um projeto Universal do Windows.  

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

    O assistente **Criar pacotes de aplicativo** aparecerá.

3. Selecione **Microsoft Store usando um novo nome de aplicativo** na primeira caixa de diálogo e clique em **Avançar**.  

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen3.jpg)

    Se você já tiver associado seu projeto a um aplicativo na loja, também terá a opção de criar pacotes para o aplicativo de repositório associado. Se você escolher **Sideload**, o Visual Studio não gerará o arquivo de carregamento do pacote do aplicativo (. msixupload ou. appxupload) para envios do Partner Center. Caso queira apenas fazer o sideload do aplicativo para executá-lo em dispositivos internos ou para testes, você pode selecionar essa opção. Para obter mais informações, consulte [Sideload Your app Package](#sideload-your-app-package).

4. Na próxima página, entre com sua conta de desenvolvedor no Partner Center. Se ainda não tiver uma conta de desenvolvedor, o assistente ajudará você a criar uma.

    ![Janela Criar Pacotes de Aplicativos com a seleção do nome do aplicativo mostrada](images/packaging-screen4.jpg)

5. Selecione o nome do aplicativo para o pacote na lista de aplicativos atualmente registrados para sua conta ou reserve um novo se você ainda não tiver reservado um no Partner Center.  

6. Certifique-se de que selecionou todas as três configurações de arquitetura (x86, x64 e ARM) na caixa de diálogo **Selecionar e Configurar Pacotes** para garantir que seu aplicativo possa ser implantado para a mais ampla variedade de dispositivos. Na caixa de listagem **Gerar lote de aplicativo**, selecione **Sempre**. Um pacote de aplicativo (. appxbundle ou. msixbundle) é preferencial em um único arquivo de pacote de aplicativo porque ele contém uma coleção de pacotes de aplicativos configurados para cada tipo de arquitetura de processador. Quando você opta por gerar o pacote de aplicativo, o pacote de aplicativo será incluído no arquivo final de upload do pacote do aplicativo (. appxupload ou. msixupload) juntamente com informações de depuração e análise de pane. Se você não tiver certeza de quais arquiteturas escolher ou deseja saber mais sobre quais arquiteturas são usadas por vários dispositivos, consulte [Arquiteturas de pacote do aplicativo](device-architecture.md).  

    ![Janela Criar Pacotes de Aplicativos com a configuração do pacote mostrada](images/packaging-screen5.jpg)

7. Inclua arquivos de símbolos públicos para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) do Partner Center depois que seu aplicativo tiver sido publicado. Configurar todos os detalhes adicionais, como a numeração de versão ou o local de saída do pacote.

8. Clique em **Criar** para gerar o pacote do aplicativo. Se você selecionou um dos **desejo criar pacotes para carregar** nas opções de Microsoft Store na etapa 3 e estiver criando um pacote para o envio do Partner Center, o assistente criará um arquivo de upload de pacote (. appxupload ou. msixupload). Se você selecionou **desejo criar pacotes para Sideload** na etapa 3, o assistente criará um único pacote de aplicativo ou um pacote de aplicativo com base em suas seleções na etapa 6.

9. Quando seu aplicativo tiver sido empacotado com êxito, você verá essa caixa de diálogo e poderá recuperar o arquivo de carregamento do pacote do aplicativo do local de saída especificado. Neste ponto, você pode [validar o pacote do aplicativo no computador local ou em um computador remoto](#validate-your-app-package) e automatizar os envios de [armazenamento](#automate-store-submissions).

    ![Janela Criação de pacote concluída com opções de validação mostradas](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Criar o arquivo de carregamento do pacote do aplicativo manualmente

1. Coloque os seguintes arquivos em uma pasta:

    - Um ou mais pacotes de aplicativos (. msix ou. AppX) ou um pacote de aplicativo (. msixbundle ou. appxbundle).
    - Um arquivo. appxsym. Este é um arquivo. pdb compactado que contém símbolos públicos de seu aplicativo usado para o [Crash Analytics](https://docs.microsoft.com/windows/uwp/publish/health-report) no Partner Center. Você pode omitir esse arquivo, mas se você fizer isso, nenhuma informação analítica ou de depuração de pane estará disponível para seu aplicativo.

2. Compactar a pasta.

3. Altere o nome da extensão de pasta compactada de. zip para. msixupload ou. appxupload.

## <a name="validate-your-app-package"></a>Validar o pacote do aplicativo

Valide seu aplicativo antes de enviá-lo para o Partner Center para certificação em um computador local ou remoto. Você pode validar apenas compilações de lançamento para o pacote do app e não compilações de depuração. Para obter mais informações sobre como enviar seu aplicativo para o Partner Center, consulte envios de [aplicativos](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Validar o pacote do aplicativo localmente

1. Na página **término da criação do pacote** final do assistente para **criar pacotes de aplicativos** , deixe a opção **computador local** selecionada e clique em **Iniciar kit de certificação de aplicativos para Windows**. Para obter mais informações sobre como testar seu aplicativo com o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    O kit de certificação de aplicativos para Windows (WACK) executa vários testes e retorna os resultados. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) para obter informações mais específicas.

    Se você tiver um dispositivo Windows 10 remoto que deseja usar para teste, será necessário instalar manualmente o kit de certificação de aplicativos do Windows nesse dispositivo. A próxima seção o guiará pelas etapas. Depois de ter feito isso, você pode selecionar **Máquina remota** e clicar em **Iniciar o Kit de Certificação de Aplicativos Windows** para se conectar ao dispositivo remoto e executar os testes de validação.

2. Depois que o WACK tiver terminado e seu aplicativo tiver passado certificação, você estará pronto para enviar seu aplicativo para o Partner Center. Certifique-se de carregar o arquivo correto. O local padrão do arquivo pode ser encontrado na pasta raiz da sua solução `\[AppName]\AppPackages` e terminará com a extensão de arquivo. appxupload ou. msixupload. O nome será do formulário `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` ou `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` se você tiver optado por um pacote de aplicativo com toda a arquitetura de pacote selecionada.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Validar o pacote do aplicativo em um dispositivo Windows 10 remoto

1. Habilite seu dispositivo Windows 10 para desenvolvimento seguindo as instruções [habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) .
    >[!IMPORTANT]
    > Não é possível validar o pacote do aplicativo em um dispositivo ARM remoto para Windows 10.

2. Baixe e instale as ferramentas remotas para o Visual Studio. Essas ferramentas são usadas para executar o Kit de Certificação de Aplicativos Windows remotamente. Você pode obter mais informações sobre essas ferramentas, inclusive onde baixá-las visitando [Executar aplicativos UWP em uma máquina remota](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).

3. Baixe o [Kit de certificação de aplicativos Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) necessário e instale-o em seu dispositivo Windows 10 remoto.

4. Na página **Criação de pacote concluída** do assistente, escolha o botão de opção **Máquina remota** e, em seguida, escolha o botão de reticências próximo ao botão **Conexão de teste**.
    >[!NOTE]
    > O botão de opção **máquina remota** só estará disponível se você tiver selecionado pelo menos uma configuração de solução que ofereça suporte à validação. Para obter mais informações sobre como testar o aplicativo com o WACK, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

5. Especifique uma forma de dispositivo dentro de sua sub-rede, ou forneça o Servidor de Nomes de Domínios DNS ou o endereço IP de um dispositivo que esteja fora de sua sub-rede.

6. Na lista **Modo de autenticação**, escolha **Nenhum**, se seu dispositivo não exigir que você se registre usando suas credenciais do Windows.

7. Escolha o botão **Selecionar** e, em seguida, escolha o botão **Iniciar o Kit de Certificação de Aplicativos Windows**. Se as ferramentas remotas estiverem sendo executadas nesse dispositivo, o Visual Studio se conectará ao aplicativo e, então, realizará o testes de validação. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatizar envios de armazenamento

A partir do Visual Studio 2019, você pode enviar o arquivo. appxupload gerado para o Microsoft Store diretamente do IDE, selecionando a opção **Enviar automaticamente para a Microsoft Store após a validação do kit de certificação de aplicativos do Windows** no final do [Assistente para criar pacotes de aplicativos](#create-your-app-package-upload-file-using-visual-studio). Esse recurso aproveita Azure Active Directory para acessar as informações de conta do Partner Center necessárias para publicar seu aplicativo. Para usar esse recurso, você precisará associar Azure Active Directory à sua conta do Partner Center e recuperar várias credenciais necessárias para envios.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associar Azure Active Directory à sua conta do Partner Center

Antes de recuperar as credenciais necessárias para envios de repositório automáticos, você deve primeiro seguir estas etapas no [painel do Partner Center](https://partner.microsoft.com/dashboard) se ainda não tiver feito isso.

1. [Associe sua conta do Partner Center ao Azure Active Directory da sua organização](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo locatário do Azure AD no Partner Center sem custo adicional.

2. [Adicione um aplicativo do Azure ad à sua conta do Partner Center](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Este aplicativo do Azure AD representa o aplicativo ou serviço que você usará para acessar os envios para sua conta do centro de desenvolvimento. Você deve atribuir esse aplicativo à função de **gerente** . Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Recuperar as credenciais necessárias para envios

Em seguida, você pode recuperar as credenciais do Partner Center necessárias para envios: a **ID do locatário do Azure**, a **ID do cliente** e a chave do **cliente**.

1. Vá para o [painel do Partner Center](https://partner.microsoft.com/dashboard) e entre com suas credenciais do Azure AD.

2. No painel do Partner Center, selecione o ícone de engrenagem (próximo ao canto superior direito do painel) e, em seguida, selecione **configurações do desenvolvedor**.

3. No menu **configurações** no painel esquerdo, clique em **usuários**.

4. Clique no nome do seu aplicativo do Azure AD para ir para as configurações do aplicativo. Nessa página, copie a **ID do locatário** e os valores da **ID do cliente** .

5. Na seção **chaves** , clique em **Adicionar nova chave**. Na próxima tela, copie o valor da **chave** , que corresponde ao segredo do cliente. Você não poderá acessar essas informações novamente depois de sair dessa página, portanto, lembre-se de não perdê-las. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Configurar envios de repositório automático no Visual Studio

Depois de concluir as etapas anteriores, você pode configurar envios automáticos de armazenamento no Visual Studio 2019.

1. No final do [Assistente para criar pacotes de aplicativos](#create-your-app-package-upload-file-using-visual-studio), selecione **Enviar automaticamente para o Microsoft Store após a validação do kit de certificação de aplicativos do Windows** e clique em reconfigurar.

2. Na caixa de diálogo **definir configurações de envio de Microsoft Store** , insira a ID de locatário do Azure, a ID do cliente e a chave do cliente.

    ![Definir configurações de envio de Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Suas credenciais podem ser salvas em seu perfil para serem usadas em envios futuros

3. Clique em **OK**.

O envio será iniciado após a conclusão do teste WACK. Você pode acompanhar o progresso de envio na janela **verificar e publicar** .

![Verificar e publicar o progresso](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>Fazer o sideload do pacote do aplicativo

Com os pacotes de aplicativos UWP, os aplicativos não são instalados em um dispositivo como estão com aplicativos da área de trabalho. Normalmente, você baixa os aplicativos UWP da Microsoft Store, que também instala o app em seu dispositivo para você. Os apps podem ser instalados sem ser publicados à Store (sideload). Isso permite que você instale e teste aplicativos usando o arquivo de pacote do aplicativo que você criou. Caso tenha um aplicativo que não queira vender na Loja, como um aplicativo de linha de negócios (LOB), você pode fazer o sideload desse aplicativo de maneira que outros usuários na empresa possam usá-lo.

Antes de poder Sideload seu aplicativo em um dispositivo de destino, você deve [habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

### <a name="create-an-app-package-for-slideloading"></a>Criar um pacote de aplicativo para slideloading

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo a partir do Visual Studio 2019 versão 16,3. Se você estiver usando uma versão anterior, parte da interface do usuário poderá parecer diferente.

1. Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.

2. Clique com o botão direito do mouse no projeto e escolha **publicar**->**criar pacotes de aplicativos** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**).

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

3. Selecione **Sideload** na primeira página do assistente e clique em **Avançar**.

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen3.jpg)

4. Na página **selecionar método de assinatura** , selecione se deseja ignorar a assinatura de empacotamento ou selecionar um certificado para assinatura. Você pode selecionar um certificado do repositório de certificados local, selecionar um arquivo de certificado ou criar um novo certificado.

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/package-signing.png)

5. Conclua a página **selecionar e configurar pacotes** conforme descrito na seção [criar seu arquivo de carregamento de pacote do aplicativo usando o Visual Studio](#create-your-app-package-upload-file-using-visual-studio) .

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Sideload seu pacote de aplicativo na atualização de aniversário do Windows 10 ou posterior

Introduzido na atualização de aniversário do Windows 10 (Windows 10, versão 1607), os pacotes de aplicativos podem ser instalados simplesmente clicando duas vezes no arquivo do pacote do aplicativo. Para usar isso, navegue até o pacote do aplicativo ou o arquivo de pacote de aplicativo e clique duas vezes nele. O [instalador do aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) é iniciado e fornece as informações básicas do aplicativo, bem como um botão de instalação, barra de progresso da instalação e quaisquer mensagens de erro relevantes.

![O Instalador de App é exibido para a instalação de um app de exemplo chamado Contoso](images/appinstaller-screen.png)

> [!NOTE]
> O instalador do aplicativo pressupõe que o aplicativo é confiável para o dispositivo. Se você estiver fazendo sideload de um app de desenvolvedor ou corporativo, será necessário instalar o certificado de autenticação no repositório Autoridades Certificação de Fornecedores ou Pessoas Confiáveis no dispositivo. Se você não tiver certeza de como fazer isso, consulte [Instalação de certificados de teste](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Sideload seu pacote de aplicativo em versões anteriores do Windows

1.  Copie as pastas da versão do app a ser instalado no dispositivo de destino.

    Se você tiver criado um lote de aplicativo, terá uma pasta baseada no número de versão e uma pasta `*_Test`. Por exemplo, estas duas pastas (em que a versão para instalar é 1.0.2.0):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Se você não tiver um lote de aplicativo, copie a pasta para a arquitetura correta e sua pasta `*_Test` correspondente. Estas duas pastas são um exemplo de um pacote do app com a arquitetura x64 e sua pasta `*_Test`:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  No dispositivo de destino, abra a pasta `*_Test`.
3.  Clique com botão direito do mouse no arquivo **Add-AppDevPackage.ps1**. Escolha **Executar com PowerShell** e siga os prompts.  
    ![O explorador de arquivos navegou para o script do PowerShell mostrado](images/packaging-screen7.jpg)

    Quando o pacote do aplicativo tiver sido instalado, a janela do PowerShell exibirá esta mensagem: **Seu aplicativo foi instalado com êxito.**
    >[!TIP]
    > Para abrir o menu de atalho em um Tablet, toque na tela onde você deseja clicar com o botão direito do mouse, segurar até que um círculo completo seja exibido e, em seguida, levante o dedo. O menu de atalho será exibido depois que você levantar o dedo.

4.  Clique no botão Iniciar para procurar o app pelo nome e o inicie.
