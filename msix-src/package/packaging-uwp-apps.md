---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empacotando aplicativos MSIX
description: Para distribuir ou vender seu aplicativo do Windows, você precisa criar um pacote de aplicativo para ele.
ms.date: 07/18/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 7ecdc839618d7a0e050b89623bf106c2554463f3
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544782"
---
# <a name="package-a-desktop-or-uwp-app-in-visual-studio"></a>Empacotar um aplicativo UWP ou da área de trabalho no Visual Studio

Antes de distribuir seu aplicativo, você precisa empacotá-lo. Este artigo descreve o processo de configuração, criação e teste de um pacote MSIX usando o Visual Studio.

## <a name="types-of-app-packages"></a>Tipos de pacotes de aplicativo

- **Pacote do aplicativo (. msix ou. AppX)**  
    Um único pacote que contém seu aplicativo e seus recursos, direcionados a uma única arquitetura de dispositivo. Por exemplo, um pacote de aplicativos x64 ou x86. Para direcionar várias arquiteturas com um pacote de aplicativos, você precisaria gerar uma para cada arquitetura. 

- **Pacote de aplicativo (. msixbundle ou. appxbundle)**  
    Um lote de aplicativo é um tipo de pacote que pode conter vários pacotes de aplicativos, cada um deles é criado para dar suporte a uma arquitetura de dispositivo específico. Por exemplo, um lote de aplicativo pode conter três pacotes de aplicativo separado para configurações x86, x64 e ARM. Lotes de aplicativo devem ser gerados sempre que possível, pois eles permitem que seu aplicativo esteja disponível na maior variedade possível de dispositivos.  

- **Arquivo de carregamento do pacote do aplicativo (. msixupload ou. appxupload)-somente para envio da loja**  
    Um único arquivo que pode conter vários pacotes de aplicativos ou um lote de aplicativo para dar suporte a várias arquiteturas de processador. O arquivo de upload do pacote do aplicativo também contém um arquivo de símbolo para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) depois que o aplicativo tiver sido publicado no Microsoft Store. Esse arquivo será criado automaticamente para você se você estiver empacotando seu aplicativo com o Visual Studio com a intenção de enviá-lo ao Partner Center para publicar no Microsoft Store.

Aqui está uma visão geral das etapas de preparação e de criação de um pacote do app:

1. [Antes de empacotar seu aplicativo](#before-packaging-your-app). Siga estas etapas para garantir que seu aplicativo esteja pronto para ser empacotado.

2. [Configure seu projeto](#configure-your-project). Use o designer de manifesto do Visual Studio para configurar o pacote. Por exemplo, adicione imagens de bloco e escolha as orientações compatíveis com o aplicativo.

3. [Gerar um pacote de aplicativo](#generate-an-app-package). Use o assistente de empacotamento do Visual Studio para criar um pacote de aplicativo.


4. [Executar, depurar e testar um aplicativo empacotado](../desktop/desktop-to-uwp-debug.md). Execute e depure o pacote do aplicativo do Visual Studio ou instalando o pacote diretamente.


## <a name="before-packaging-your-app"></a>Antes de empacotar seu aplicativo

1. **Teste seu aplicativo.** Antes de empacotar seu aplicativo, verifique se ele funciona conforme o esperado em todas as famílias de dispositivos para as quais você planeja dar suporte. Essas famílias de dispositivos podem incluir desktop, celular, Surface Hub, Xbox, dispositivos IoT ou outros. Para obter mais informações sobre como implantar e testar seu aplicativo usando o Visual Studio, consulte [Implantando e Depurando aplicativos UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps) (também se aplica a aplicativos de desktop empacotados).

2. **Otimize seu aplicativo.** Você pode usar as ferramentas de criação de perfil e depuração do Visual Studio para otimizar o desempenho do aplicativo empacotado. Por exemplo, a ferramenta de linha do tempo para capacidade de resposta da interface do usuário, a ferramenta de uso da memória, a ferramenta de uso da CPU e muito mais. Para obter mais informações sobre essas ferramentas, consulte o tópico [Tour sobre recurso de perfil](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).

3. **Verifique a compatibilidade de .NET Native (para C# vb e aplicativos).** Na Plataforma Universal do Windows, existe um compilador nativo que melhorará o desempenho do tempo de execução do app. Com essa alteração, você deverá testar seu aplicativo nesse ambiente de compilação. Por padrão, a configuração de build **Release** habilita a cadeia de ferramentas .NET Native, então é importante testar seu aplicativo com essa configuração **Release** e verificar se seu aplicativo se comporta como o esperado.


## <a name="configure-your-project"></a>Configurar seu projeto

O arquivo de manifesto do app (Package.appxmanifest.xml) é um arquivo XML que contém as propriedades e as configurações necessárias para criar o pacote do app. Por exemplo, as propriedades no arquivo de manifesto do aplicativo descrevem a imagem a ser usada como o bloco do aplicativo e as orientações compatíveis com o aplicativo quando um usuário gira o dispositivo.

O designer de manifesto do Visual Studio permite a atualização do arquivo de manifesto sem a edição do XML bruto do arquivo.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configurar um pacote com o designer de manifesto

1. Em **Gerenciador de soluções**, expanda o nó do projeto do seu projeto de aplicativo.

2. Clique duas vezes no arquivo **Package.appxmanifest**. Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.

3. Agora você pode decidir como configurar o aplicativo. Cada guia contém informações que podem ser configuradas sobre seu aplicativo e links para mais informações, se necessário.


    ![Designer de manifesto do Visual Studio](images/packaging-screen1.jpg)

    Verifique se você tem todas as imagens necessárias para um aplicativo na guia **ativos visuais** .

    Da guia **Empacotamento**, você pode inserir dados de publicação. É ali que você pode escolher qual certificado usar para assinar seu aplicativo. Todos os aplicativos MSIX devem ser assinados com um certificado.

    > [!NOTE]
    > A partir do Visual Studio 2019, um certificado temporário não é mais gerado em projetos de desktop ou UWP empacotados. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos [neste artigo](create-certificate-package-signing.md).

    > [!IMPORTANT]
    > Se você estiver publicando seu aplicativo na Microsoft Store, seu aplicativo será assinado com um certificado confiável para você. Isso permite que o usuário instale e execute seu aplicativo sem precisar instalar o certificado de autenticação do aplicativo associado.

    Se você estiver instalando o pacote do aplicativo em seu dispositivo, primeiro precisará confiar no pacote. Para confiar o pacote, o certificado deve estar instalado no dispositivo do usuário.

4. Salve seu arquivo **Package.appxmanifest** depois de fazer as edições necessárias para o aplicativo.

Se você estiver distribuindo seu aplicativo por meio do Microsoft Store, o Visual Studio poderá associar seu pacote à loja. Para fazer isso, clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e escolha **publicar**->**associar aplicativo à loja** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**). Você também pode fazer isso no Assistente para **criar pacotes de aplicativos** , que é descrito na seção a seguir. Quando você associa seu aplicativo, alguns dos campos na guia Empacotamento do designer de manifesto são atualizados automaticamente.

## <a name="generate-an-app-package"></a>Gerar um pacote de aplicativo

Os aplicativos podem ser instalados sem serem publicados no armazenamento, publicando-os no seu site, usando ferramentas de gerenciamento de aplicativos, como Microsoft Intune e Configuration Manager, etc. Você também pode instalar diretamente um pacote MSIX para teste em seu computador local ou remoto.

### <a name="create-an-app-package-using-the-packaging-wizard"></a>Criar um pacote de aplicativo usando o assistente de empacotamento

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo a partir do Visual Studio 2019 versão 16,3. Se você estiver usando uma versão anterior, parte da interface do usuário poderá parecer diferente.
> Se você estiver empacotando um aplicativo de área de trabalho, clique com o botão direito do mouse no nó projeto de empacotamento de aplicativos do Windows.

1. No **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo.

2. Clique com o botão direito do mouse no projeto e escolha **publicar**->**criar pacotes de aplicativos** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**).

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

3. Selecione **Sideload** na primeira página do assistente e clique em **Avançar**.

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen10.png)

4. Na página **selecionar método de assinatura** , selecione se deseja ignorar a assinatura de empacotamento ou selecionar um certificado para assinatura. Você pode selecionar um certificado do repositório de certificados local, selecionar um arquivo de certificado ou criar um novo certificado. Para que um pacote MSIX seja instalado na máquina de um usuário final, ele deve ser assinado com um certificado confiável no computador. 

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/package-signing2.png)

5. Conclua a página **selecionar e configurar pacotes** conforme descrito na seção [criar seu arquivo de carregamento de pacote do aplicativo usando o Visual Studio](#create-your-app-package-upload-file-using-visual-studio) .

### <a name="install-your-app-package-by-double-clicking"></a>Instale o pacote do aplicativo clicando duas vezes

Os pacotes de aplicativos podem ser instalados simplesmente clicando duas vezes no arquivo de pacote do aplicativo. Para fazer isso, navegue até o pacote do aplicativo ou o arquivo de pacote de aplicativo e clique duas vezes nele. O [instalador do aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) é iniciado e fornece as informações básicas do aplicativo, bem como um botão de instalação, barra de progresso da instalação e quaisquer mensagens de erro relevantes.

> [!NOTE]
> O instalador do aplicativo pressupõe que o pacote foi assinado com um certificado confiável no dispositivo. Se não foi, você precisará instalar o certificado de autenticação no repositório de autoridades de certificação de pessoas confiáveis ou de fornecedores confiáveis no dispositivo. Se você não tiver certeza de como fazer isso, consulte [Instalação de certificados de teste](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="install-your-app-package-using-an-install-script"></a>Instalar o pacote do aplicativo usando um script de instalação

1. Abra a pasta `*_Test`.
2. Clique com botão direito do mouse no arquivo **Add-AppDevPackage.ps1**. Escolha **Executar com PowerShell** e siga os prompts.  
    ![explorador de arquivos navegou até o script do PowerShell mostrado](images/packaging-screen7.jpg)

    Quando o pacote do app tiver sido instalado, a janela do PowerShell exibirá esta mensagem: **Seu aplicativo foi instalado com êxito.**

3. Clique no botão Iniciar para procurar o app pelo nome e o inicie.

### <a name="next-steps-debug-and-test-your-app-package"></a>Próximas etapas: Depurar e testar seu pacote de aplicativo

Consulte [Executar, depurar e testar um pacote de aplicativo](../desktop/desktop-to-uwp-debug.md) para saber como você pode depurar seu aplicativo no Visual Studio ou usando as ferramentas de depuração do Windows.

## <a name="generate-an-app-package-upload-file-for-store-submission"></a>Gerar um arquivo de carregamento de pacote de aplicativo para envio de armazenamento

Para distribuir seu aplicativo para o Microsoft Store, recomendamos que você gere um **arquivo de carregamento de pacote de aplicativo** (. msixupload ou. appxupload) e envie esse arquivo para o Partner Center. Embora seja possível enviar um pacote do aplicativo ou um pacote de aplicativos para o Partner Center sozinho, recomendamos que você envie um arquivo de carregamento do pacote do aplicativo.

Você pode criar um arquivo de carregamento de pacote de aplicativo usando o assistente para **criar pacotes de aplicativos** no Visual Studio, ou você pode criar um manualmente a partir de pacotes de aplicativo existentes ou de grupos de aplicativos.

> [!NOTE]
> Se você quiser criar um pacote de aplicativo (. msix ou. AppX) ou um pacote de aplicativo (. msixbundle ou. appxbundle) manualmente, consulte [criar um pacote de aplicativo com a ferramenta MakeAppx. exe](create-app-package-with-makeappx-tool.md).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Criar o arquivo de carregamento do pacote do aplicativo usando o Visual Studio

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo a partir do Visual Studio 2019 versão 16,3. Se você estiver usando uma versão anterior, parte da interface do usuário poderá parecer diferente.

1. Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.

2. Clique com o botão direito do mouse no projeto e escolha **publicar**->**criar pacotes de aplicativos** (antes do Visual Studio 2019 versão 16,3, o menu **publicar** é chamado de **repositório**). Se essa opção estiver desabilitada ou não aparecer, verifique se o projeto é um projeto universal do Windows.  

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

    O assistente **Criar pacotes de aplicativo** aparecerá.

3. Selecione **Microsoft Store usando um novo nome de aplicativo** na primeira caixa de diálogo e clique em **Avançar**.  

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen3.jpg)

    Se você já tiver associado seu projeto a um aplicativo na loja, também terá a opção de criar pacotes para o aplicativo de repositório associado. Se você escolher **Sideload**, o Visual Studio não gerará o arquivo de carregamento do pacote do aplicativo (. msixupload ou. appxupload) para envios do Partner Center. Se você quiser apenas criar um MSIX packge ou um pacote para distribuição que não seja de armazenamento, poderá selecionar essa opção.

4. Na próxima página, entre com sua conta de desenvolvedor no Partner Center. Se ainda não tiver uma conta de desenvolvedor, o assistente ajudará você a criar uma.

    ![Janela Criar Pacotes de Aplicativos com a seleção do nome do aplicativo mostrada](images/packaging-screen4.jpg)

5. Selecione o nome do aplicativo para o pacote na lista de aplicativos atualmente registrados para sua conta ou reserve um novo se você ainda não tiver reservado um no Partner Center.  

6. Certifique-se de que selecionou todas as três configurações de arquitetura (x86, x64 e ARM) na caixa de diálogo **Selecionar e Configurar Pacotes** para garantir que seu aplicativo possa ser implantado para a mais ampla variedade de dispositivos. Na caixa de listagem **Gerar lote de aplicativo**, selecione **Sempre**. Um pacote de aplicativo (. appxbundle ou. msixbundle) é preferencial em um único arquivo de pacote de aplicativo porque ele contém uma coleção de pacotes de aplicativos configurados para cada tipo de arquitetura de processador. Quando você opta por gerar o pacote de aplicativo, o pacote de aplicativo será incluído no arquivo final de upload do pacote do aplicativo (. appxupload ou. msixupload) juntamente com informações de depuração e análise de pane. Se você não tiver certeza de quais arquiteturas escolher ou deseja saber mais sobre quais arquiteturas são usadas por vários dispositivos, consulte [Arquiteturas de pacote do aplicativo](device-architecture.md).  

    ![Janela Criar Pacotes de Aplicativos com a configuração do pacote mostrada](images/packaging-screen5.jpg)

7. Inclua arquivos de símbolos públicos para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) do Partner Center depois que seu aplicativo tiver sido publicado. Configurar todos os detalhes adicionais, como a numeração de versão ou o local de saída do pacote.

8. Clique em **Criar** para gerar o pacote do aplicativo. Se você selecionou um dos **desejo criar pacotes para carregar** nas opções de Microsoft Store na etapa 3 e estiver criando um pacote para o envio do Partner Center, o assistente criará um arquivo de upload de pacote (. appxupload ou. msixupload). Se você selecionou **desejo criar pacotes para Sideload** na etapa 3, o assistente criará um único pacote de aplicativo ou um pacote de aplicativo com base em suas seleções na etapa 6.

9. Quando seu aplicativo tiver sido empacotado com êxito, você verá essa caixa de diálogo e poderá recuperar o arquivo de carregamento do pacote do aplicativo do local de saída especificado. Neste ponto, você pode [validar o pacote do aplicativo no computador local ou em um computador remoto](#validate-your-app-package) e [automatizar os envios de armazenamento](#automate-store-submissions).

    ![Janela Criação de pacote concluída com opções de validação mostradas](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Criar o arquivo de carregamento do pacote do aplicativo manualmente

1. Coloque os seguintes arquivos em uma pasta:

    - Um ou mais pacotes de aplicativos (. msix ou. AppX) ou um pacote de aplicativo (. msixbundle ou. appxbundle).
    - Um arquivo. appxsym. Este é um arquivo. pdb compactado que contém símbolos públicos de seu aplicativo usado para o [Crash Analytics](https://docs.microsoft.com/windows/uwp/publish/health-report) no Partner Center. Você pode omitir esse arquivo, mas se você fizer isso, nenhuma informação analítica ou de depuração de pane estará disponível para seu aplicativo.

2. Selecione todos os arquivos dentro da pasta, clique com o botão direito do mouse nos arquivos e selecione **Enviar para** -> **pasta compactada (zipada)** .

3. Altere o nome da extensão do novo arquivo zip de. zip para. msixupload ou. appxupload.

## <a name="validate-your-app-package"></a>Validar seu pacote de aplicativos

Valide seu aplicativo antes de enviá-lo para o Partner Center para certificação em um computador local ou remoto. Você pode validar apenas compilações de lançamento para o pacote do app e não compilações de depuração. Para obter mais informações sobre como enviar seu aplicativo para o Partner Center, consulte [envios de aplicativos](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Validar o pacote do aplicativo localmente

1. Na página **término da criação do pacote** final do assistente para **criar pacotes de aplicativos** , deixe a opção **computador local** selecionada e clique em **Iniciar kit de certificação de aplicativos para Windows**. Para obter mais informações sobre como testar seu aplicativo com o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    O kit de certificação de aplicativos para Windows (WACK) executa vários testes e retorna os resultados. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) para obter informações mais específicas.

    Se você tiver um dispositivo Windows 10 remoto que deseja usar para teste, será necessário instalar manualmente o kit de certificação de aplicativos do Windows nesse dispositivo. A próxima seção guiará você por essas etapas. Depois de ter feito isso, você pode selecionar **Máquina remota** e clicar em **Iniciar o Kit de Certificação de Aplicativos Windows** para se conectar ao dispositivo remoto e executar os testes de validação.

2. Depois que o WACK tiver terminado e seu aplicativo tiver passado certificação, você estará pronto para enviar seu aplicativo para o Partner Center. Certifique-se de carregar o arquivo correto. O local padrão do arquivo pode ser encontrado na pasta raiz da sua solução `\[AppName]\AppPackages` e terminará com a extensão de arquivo. appxupload ou. msixupload. O nome estará no formato `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` ou `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` se você tiver optado por um pacote de aplicativo com toda a arquitetura de pacote selecionada.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Validar o pacote do aplicativo em um dispositivo Windows 10 remoto

1. Habilite seu dispositivo Windows 10 para desenvolvimento seguindo as instruções [habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) .
    >[!IMPORTANT]
    > Não é possível validar o pacote do aplicativo em um dispositivo ARM remoto para Windows 10.

2. {1&gt;{2&gt;Baixar e instalar as ferramentas remotas&lt;2}&lt;1} para Visual Studio. Essas ferramentas são usadas para executar remotamente o Kit de Certificação de Aplicativos Windows. Você pode obter mais informações sobre essas ferramentas, incluindo onde baixá-las visitando [executar MSIX applicationss em um computador remoto](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).

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

1. No final do [Assistente para criar pacotes de aplicativos](#create-your-app-package-upload-file-using-visual-studio), selecione **Enviar automaticamente para o Microsoft Store após a validação do kit de certificação de aplicativos do Windows** e clique em **reconfigurar**.

2. Na caixa de diálogo **definir configurações de envio de Microsoft Store** , insira a ID de locatário do Azure, a ID do cliente e a chave do cliente.

    ![Definir configurações de envio de Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Suas credenciais podem ser salvas em seu perfil para serem usadas em envios futuros

3. Clique em **OK**.

O envio será iniciado após a conclusão do teste WACK. Você pode acompanhar o progresso de envio na janela **verificar e publicar** .

![Verificar e publicar o progresso](images/packaging-screen9.jpg)
