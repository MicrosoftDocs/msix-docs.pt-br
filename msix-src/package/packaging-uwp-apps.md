---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empacotar aplicativos MSIX
description: Para distribuir ou vender seu aplicativo do Windows, é necessário criar um pacote do aplicativo para ele.
ms.date: 07/18/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: a78a7b09ebf11f4c6ddaeead6cd987bd724d548b
ms.sourcegitcommit: 45bb7e2f642a0c7165366bc0867afe803abfc202
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81433772"
---
# <a name="package-a-desktop-or-uwp-app-in-visual-studio"></a>Empacotar um aplicativo UWP ou da área de trabalho no Visual Studio

Antes de distribuir seu aplicativo, você precisa empacotá-lo. Este artigo descreve o processo de configuração, criação e teste de um pacote MSIX usando o Visual Studio.

## <a name="types-of-app-packages"></a>Tipos de pacotes do aplicativo

- **Pacote do aplicativo (.msix ou.appx)**  
    Um único pacote que contém seu aplicativo e os recursos dele, direcionados a uma única arquitetura de dispositivo. Por exemplo, um pacote de aplicativos x64 ou x86. Para direcionar várias arquiteturas com um lote de aplicativo, você precisaria gerar uma para cada arquitetura. 

- **Lote de aplicativo (.msixbundle ou .appxbundle)**  
    Um lote de aplicativo é um tipo de pacote que pode conter vários pacotes do aplicativo. Cada um deles é criado para dar suporte a uma arquitetura de dispositivo específico. Por exemplo, um lote de aplicativo pode conter três pacotes do aplicativo separados para as configurações x86, x64 e ARM. Os lotes de aplicativo devem ser gerados sempre que possível, pois eles permitem que seu aplicativo esteja disponível na maior variedade possível de dispositivos.  

- **Arquivo de upload do pacote do aplicativo (.msixupload ou .appxupload) – somente para envio à Store**  
    Um único arquivo que pode conter vários pacotes do aplicativo ou um lote de aplicativo para dar suporte a várias arquiteturas de processador. O arquivo de upload do pacote do aplicativo também conterá um arquivo de símbolo para [Analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) depois que seu aplicativo for publicado na Microsoft Store. Este arquivo será automaticamente criado para você se você estiver empacotando seu aplicativo com o Visual Studio com a intenção de enviá-lo ao Partner Center para publicação na Microsoft Store.

Aqui está uma visão geral das etapas de preparação e de criação de um pacote do aplicativo:

1. [Antes de empacotar seu aplicativo](#before-packaging-your-app). Siga estas etapas para se verificar se seu aplicativo está pronto para ser empacotado.

2. [Configurar seu projeto](#configure-your-project). Use o designer de manifesto do Visual Studio para configurar o pacote. Por exemplo, adicione imagens de bloco e escolha as orientações compatíveis com o aplicativo.

3. [Gerar um pacote do aplicativo](#generate-an-app-package). Use o assistente de empacotamento do Visual Studio para criar um pacote do aplicativo.


4. [Executar, depurar e testar um aplicativo empacotado](../desktop/desktop-to-uwp-debug.md). Execute e depure o pacote do aplicativo do Visual Studio ou instale o pacote diretamente.


## <a name="before-packaging-your-app"></a>Antes de empacotar seu aplicativo

1. **Teste seu aplicativo.** Antes de empacotar seu aplicativo, verifique se ele funciona conforme esperado em todas as famílias de dispositivos às quais você pretende dar suporte. Essas famílias de dispositivos podem incluir desktop, celular, Surface Hub, Xbox, dispositivos IoT ou outros. Para obter mais informações sobre como implantar e testar seu aplicativo usando o Visual Studio, confira [Implantar e depurar aplicativos UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps) (também se aplica a aplicativos da área de trabalho empacotados).

2. **Otimizar seu aplicativo.** Você pode usar as ferramentas de criação de perfil e depuração do Visual Studio para otimizar o desempenho do seu aplicativo empacotado. Por exemplo, a ferramenta de Linha do tempo para capacidade de resposta da interface do usuário, a ferramenta de Uso de memória, a ferramenta de Uso da CPU e muito mais. Para obter mais informações sobre essas ferramentas, confira o tópico [Tour sobre o recurso de criação de perfil](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).

3. **Verifique a compatibilidade nativa do .NET (para aplicativos VB e C#).** Na Plataforma Universal do Windows, há um compilador nativo que aprimorará o desempenho do runtime do aplicativo. Com essa alteração, você deverá testar seu aplicativo nesse ambiente de compilação. Por padrão, a configuração de build **Liberação** habilita a cadeia de ferramentas nativa do .NET. Portanto, é importante testar seu aplicativo com essa configuração **Liberação** e verificar se ele se comporta como o esperado. Você pode encontrar mais informações em [compilando aplicativos com .net Native](https://docs.microsoft.com/dotnet/framework/net-native/).


## <a name="configure-your-project"></a>Configurar seu projeto

O arquivo de manifesto do aplicativo (Package.appxmanifest.xml) é um arquivo XML que contém as propriedades e as configurações necessárias para criar o pacote do aplicativo. Por exemplo, as propriedades no arquivo de manifesto do aplicativo descrevem a imagem a ser usada como o bloco do aplicativo e as orientações compatíveis com o aplicativo quando um usuário gira o dispositivo.

O designer de manifesto do Visual Studio permite a atualização do arquivo de manifesto sem a edição do XML bruto do arquivo.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configurar um pacote com o designer de manifesto

1. No **Gerenciador de Soluções**, expanda o nó do projeto do seu projeto de aplicativo.

2. Clique duas vezes no arquivo **Package.appxmanifest**. Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.

3. Agora você pode decidir como configurar o aplicativo. Cada guia contém informações que podem ser configuradas sobre seu aplicativo e links para mais informações, se necessário.


    ![Designer de manifesto do Visual Studio](images/packaging-screen1.jpg)

    Verifique se você tem todas as imagens exigidas para um aplicativo na guia **Ativos Visuais**.

    Da guia **Empacotamento**, você pode inserir dados de publicação. É ali que você pode escolher qual certificado usar para assinar seu aplicativo. Todos os aplicativos MSIX devem ser assinados com um certificado.

    > [!NOTE]
    > Do Visual Studio 2019 em diante, um certificado temporário não é mais gerado nos projetos UWP ou da área de trabalho empacotados. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos [neste artigo](create-certificate-package-signing.md).

    > [!IMPORTANT]
    > Se você estiver publicando seu aplicativo na Microsoft Store, ele será autenticado com um certificado confiável para você. Isso permite que o usuário instale e execute seu aplicativo sem instalar o certificado de autenticação do aplicativo associado.

    Se você estiver instalando o pacote do aplicativo em seu dispositivo, primeiro será necessário confiar no pacote. Para confiar no pacote, o certificado deve estar instalado no dispositivo do usuário.

4. Salve seu arquivo **Package.appxmanifest** após fazer as edições necessárias para o aplicativo.

Se você estiver distribuindo seu aplicativo por meio da Microsoft Store, o Visual Studio poderá associar seu pacote à Store. Para fazer isso, clique com o botão direito do mouse no nome do seu projeto no Gerenciador de Soluções e escolha **Publicar**->**Associar o aplicativo à Store** (para versões anteriores à 16.3 do Visual Studio 2019, o menu **Publicar** é denominado **Store**). Você também pode fazer isso no assistente **Criar pacotes do aplicativo**, descrito na seção a seguir. Quando você associa seu aplicativo, alguns dos campos na guia Empacotamento do designer de manifesto são atualizados automaticamente.

## <a name="generate-an-app-package"></a>Gerar um pacote do aplicativo

Os aplicativos podem ser instalados sem serem publicados no armazenamento, publicando-os no seu site, usando ferramentas de gerenciamento de aplicativos, como Microsoft Intune e Configuration Manager, etc. Você também pode instalar diretamente um pacote MSIX para teste em seu computador local ou remoto.

### <a name="create-an-app-package-using-the-packaging-wizard"></a>Criar um pacote do aplicativo usando o assistente de empacotamento

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo do Visual Studio 2019 versão 16.3 em diante. Se estiver usando uma versão anterior, algumas interfaces do usuário poderão ter aparência diferente.
> Se você estiver empacotando um aplicativo da área de trabalho, clique com o botão direito do mouse no nó Projeto de Empacotamento de Aplicativo do Windows.

1. No **Gerenciador de Soluções**, abra a solução para seu projeto de aplicativo.

2. Clique com o botão direito do mouse no projeto e escolha **Publicar**->**Criar pacotes do aplicativo** (para versões anteriores à 16.3 do Visual Studio 2019, o menu **Publicar** é denominado **Store**).

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

3. Selecione **Sideload** na primeira página do assistente e clique em **Avançar**.

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen10.png)

4. Na página **Selecionar método de autenticação**, selecione se você deseja ignorar a autenticação do empacotamento ou selecione um certificado para autenticação. Você pode selecionar um certificado em seu repositório de certificados local, selecionar um arquivo de certificado ou criar um certificado. Para um pacote MSIX ser instalado em um computador do usuário final, ele deve ser assinado com um certificado confiável na máquina. 

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/package-signing2.png)

5. Preencha a página **Selecionar e configurar pacotes** conforme descrito na seção [Criar seu arquivo de upload do pacote do aplicativo usando o Visual Studio](#create-your-app-package-upload-file-using-visual-studio).

### <a name="install-your-app-package-by-double-clicking"></a>Instalar seu pacote do aplicativo clicando duas vezes

Os pacotes do aplicativo podem ser instalados simplesmente clicando duas vezes no arquivo do pacote do aplicativo. Para fazer isso, navegue até o arquivo do pacote do aplicativo ou do lote de aplicativo e clique duas vezes nele. O [Instalador de Aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) é iniciado e fornece as informações básicas sobre o aplicativo, bem como um botão de instalação, a barra de progresso da instalação e as mensagens de erro relevantes.

> [!NOTE]
> O Instalador de Aplicativo pressupõe que o pacote foi assinado com um certificado confiável no dispositivo. Caso contrário, será necessário instalar o certificado de autenticação no repositório Autoridades de Certificação de Editores ou Pessoas Confiáveis no dispositivo. Se você não tem certeza de como fazer isso, confira [Instalar Certificados de Teste](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="install-your-app-package-using-an-install-script"></a>Instalar seu pacote do aplicativo usando um script de instalação

1. Abra a pasta `*_Test`.
2. Clique com botão direito do mouse no arquivo **Add-AppDevPackage.ps1**. Escolha **Executar com PowerShell** e siga os prompts.  
    ![Explorador de arquivos posicionado no script do PowerShell mostrado](images/packaging-screen7.jpg)

    Quando o pacote do app tiver sido instalado, a janela do PowerShell exibirá esta mensagem: **Seu aplicativo foi instalado com êxito.**

3. Clique no botão Iniciar para procurar o aplicativo pelo nome e o inicie.

### <a name="next-steps-debug-and-test-your-app-package"></a>Próximas etapas: Depurar e testar seu pacote de aplicativo

Confira [Executar, depurar e testar um pacote do aplicativo](../desktop/desktop-to-uwp-debug.md) para saber como você pode depurar seu aplicativo no Visual Studio ou usando as ferramentas de depuração do Windows.

## <a name="generate-an-app-package-upload-file-for-store-submission"></a>Gerar um arquivo de upload do pacote do aplicativo para envio à Store

Para distribuir seu aplicativo para a Microsoft Store, recomendamos gerar um **arquivo de upload do pacote do aplicativo** (.msixupload ou .appxupload) e envie esse arquivo ao Partner Center. Embora seja possível enviar um pacote do aplicativo ou lote de aplicativo ao Partner Center sozinho, recomendamos enviar um arquivo de upload do pacote do aplicativo.

Você pode criar um arquivo de upload do pacote do aplicativo usando o assistente **Criar Pacotes do Aplicativo** no Visual Studio ou você pode criar um manualmente com base em pacotes do aplicativo ou lotes de aplicativo existentes.

> [!NOTE]
> Se você deseja criar um pacote do aplicativo (.msixbundle ou .appxbundle) manualmente, confira [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Criar seu arquivo de upload do pacote do aplicativo usando o Visual Studio

> [!NOTE]
> As instruções e capturas de tela a seguir descrevem o processo do Visual Studio 2019 versão 16.3 em diante. Se estiver usando uma versão anterior, algumas interfaces do usuário poderão ter aparência diferente.

1. Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.

2. Clique com o botão direito do mouse no projeto e escolha **Publicar**->**Criar pacotes do aplicativo** (para versões anteriores à 16.3 do Visual Studio 2019, o menu **Publicar** é denominado **Store**). Se essa opção estiver desabilitada ou não aparecer, verifique se o projeto é um projeto universal do Windows.  

    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

    O assistente **Criar pacotes de aplicativo** aparecerá.

3. Selecione **Microsoft Store usando um novo nome do aplicativo** na primeira caixa de diálogo e clique em **Avançar**.  

    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen3.jpg)

    Se você já tiver associado seu projeto a um aplicativo na Store, também terá uma opção de criar pacotes para o aplicativo da Store associado. Se você escolher **Sideload**, o Visual Studio não gerará o arquivo (.msixupload ou .appxupload) de upload do pacote do aplicativo para envios ao Partner Center. Se apenas desejar criar um pacote ou lote MSIX para distribuição fora da Store, você poderá selecionar essa opção.

4. Na próxima página, entre com sua conta de desenvolvedor no Partner Center. Se ainda não tiver uma conta de desenvolvedor, o assistente ajudará você a criar uma.

    ![Janela Criar Pacotes de Aplicativos com a seleção do nome do aplicativo mostrada](images/packaging-screen4.jpg)

5. Selecione o nome do aplicativo para seu pacote na lista de aplicativos registrados atualmente em sua conta ou reserve um novo se você ainda não reservou um no Partner Center.  

6. Verifique se você selecionou todas as três configurações de arquitetura (x86, x64 e ARM) na caixa de diálogo **Selecionar e Configurar Pacotes** para verificar se seu aplicativo pode ser implantado na mais ampla variedade de dispositivos. Na caixa de listagem **Gerar lote de aplicativo**, selecione **Sempre**. Um lote de aplicativo (.appxbundle ou .msixbundle) tem preferência sobre um único arquivo de pacote do aplicativo, pois ele contém uma coleção de pacotes do aplicativo configurados para cada tipo de arquitetura do processador. Ao optar por gerar o lote de aplicativo, ele será incluído no arquivo (.appxupload ou .msixupload) de upload do pacote do aplicativo final junto com informações de depuração e análise de falha. Se você não tem certeza de quais arquiteturas escolher ou deseja saber mais sobre quais arquiteturas são usadas por vários dispositivos, confira [Arquiteturas do pacote do aplicativo](device-architecture.md).  

    ![Janela Criar Pacotes de Aplicativos com a configuração do pacote mostrada](images/packaging-screen5.jpg)

7. Inclua arquivos de símbolo públicos para [Analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) no Partner Center depois que seu aplicativo for publicado. Configure detalhes adicionais, como a numeração de versão ou o local de saída do pacote.

8. Clique em **Criar** para gerar o pacote do aplicativo. Se você selecionou uma das opções de **Desejo criar pacotes para carregar na Microsoft Store** na etapa 3 e estiver criando um pacote para envio ao Partner Center, o assistente criará um arquivo (.appxupload ou .msixupload) de upload do pacote. Se você selecionou **Desejo criar pacotes para sideload** na etapa 3, o assistente criará um único pacote do aplicativo ou um lote de aplicativo com base nas suas seleções na etapa 6.

9. Quando seu aplicativo for empacotado com êxito, você verá essa caixa de diálogo e poderá recuperar o arquivo de upload do pacote do aplicativo do local de saída especificado. Nesse ponto, você poderá [validar seu pacote do aplicativo no computador local ou em um computador remoto](#validate-your-app-package) e [automatizar envios à loja](#automate-store-submissions).

    ![Janela Criação de pacote concluída com opções de validação mostradas](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Criar seu arquivo de upload do pacote do aplicativo manualmente

1. Coloque os seguintes arquivos em uma pasta:

    - Um ou mais pacotes do aplicativo (.msix ou .appx) ou um lote de aplicativo (.msixbundle ou .appxbundle).
    - Um arquivo .appxsym. Esse é um arquivo .pdb compactado que contém símbolos públicos do aplicativo usados para a [análise de falhas](https://docs.microsoft.com/windows/uwp/publish/health-report) no Partner Center. Você pode omitir esse arquivo, mas, se fizer isso, nenhuma informação de análise de falha ou de depuração estará disponível para seu aplicativo.

2. Selecione todos os arquivos dentro da pasta, clique com o botão direito do mouse neles e selecione **Enviar para** -> **Pasta compactada (zipada)** .

3. Altere o nome da extensão do novo arquivo zip de .zip para .msixupload ou .appxupload.

## <a name="validate-your-app-package"></a>Validar seu pacote de aplicativos

Valide o aplicativo antes de enviá-lo para o Partner Center para certificação em um computador local ou remoto. Você pode validar apenas builds de versão para o pacote do aplicativo, e não builds de depuração. Para obter mais informações sobre como enviar seu aplicativo para o Partner Center, confira [Envios de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Validar seu pacote do aplicativo localmente

1. Na página final **Criação do pacote concluída** do assistente **Criar pacotes do aplicativo**, deixe a opção **Computador local** selecionada e clique em **Iniciar Kit de Certificação de Aplicativos Windows**. Para obter mais informações sobre como testar seu aplicativo com o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    O WACK (Kit de Certificação de Aplicativos Windows) realiza diversos testes e retorna os resultados. Confira [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) para obter informações mais específicas.

    Se você tiver um dispositivo remoto do Windows 10 que deseja usar para teste, precisará instalar o Kit de Certificação de Aplicativos Windows manualmente nesse dispositivo. A próxima seção guiará você por essas etapas. Depois de ter feito isso, você pode selecionar **Máquina remota** e clicar em **Iniciar o Kit de Certificação de Aplicativos Windows** para se conectar ao dispositivo remoto e executar os testes de validação.

2. Após a conclusão do WACK e a certificação do aplicativo, você estará pronto para enviar seu aplicativo para o Partner Center. Certifique-se de carregar o arquivo correto. O local padrão do arquivo pode ser encontrado na pasta raiz da sua solução `\[AppName]\AppPackages` e terminará com a extensão de arquivo .msixupload ou .appxupload. O nome será do formulário `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` ou `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` se você optar por um lote de aplicativo com todos os pacotes de arquitetura selecionados.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Validar seu pacote do aplicativo em um dispositivo Windows 10 remoto

1. Habilite seu dispositivo Windows 10 para desenvolvimento seguindo as instruções de [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
    >[!IMPORTANT]
    > Você não pode validar seu pacote do aplicativo em um dispositivo ARM remoto para Windows 10.

2. {1&gt;{2&gt;Baixar e instalar as ferramentas remotas&lt;2}&lt;1} para Visual Studio. Essas ferramentas são usadas para executar remotamente o Kit de Certificação de Aplicativos Windows. Você pode obter mais informações sobre essas ferramentas, inclusive onde baixá-las, acessando [Executar aplicativos MSIX em um computador remoto](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).

3. Baixe o [Kit de Certificação de Aplicativos Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) exigido e, depois, instale-o em seu dispositivo Windows 10 remoto.

4. Na página **Criação de pacote concluída** do assistente, escolha o botão de opção **Máquina remota** e, em seguida, escolha o botão de reticências próximo ao botão **Conexão de teste**.
    >[!NOTE]
    > O botão de opção **Computador Remoto** estará disponível somente se você tiver selecionado pelo menos uma configuração de solução compatível com a validação. Para obter mais informações sobre como testar o aplicativo com o WACK, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

5. Especifique uma forma de dispositivo dentro de sua sub-rede, ou forneça o Servidor de Nomes de Domínios DNS ou o endereço IP de um dispositivo que esteja fora de sua sub-rede.

6. Na lista **Modo de autenticação**, escolha **Nenhum**, se seu dispositivo não exigir que você se registre usando suas credenciais do Windows.

7. Escolha o botão **Selecionar** e, em seguida, escolha o botão **Iniciar o Kit de Certificação de Aplicativos Windows**. Se as ferramentas remotas estiverem sendo executadas nesse dispositivo, o Visual Studio se conectará ao aplicativo e, então, realizará os testes de validação. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatizar envios à Store

Do Visual Studio 2019 em diante, você pode enviar o arquivo .appxupload gerado à Microsoft Store diretamente do IDE selecionando a opção **Enviar automaticamente à Microsoft Store após a validação do Kit de Certificação de Aplicativos Windows** no fim do [Assistente para Criar pacotes do aplicativo](#create-your-app-package-upload-file-using-visual-studio). Esse recurso usa o Azure Active Directory para acessar as informações sobre a conta do Partner Center necessárias para publicar seu aplicativo. Para usar esse recurso, você precisará associar o Azure Active Directory à conta do Partner Center e recuperar várias credenciais necessárias para envios.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associar o Azure Active Directory à sua conta do Partner Center

Antes de poder recuperar as credenciais necessárias para envios automáticos à Store, primeiro você precisará seguir essas etapas no [painel do Partner Center](https://partner.microsoft.com/dashboard) caso não tenha feito isso ainda.

1. [Associar sua conta do Partner Center ao Azure Active Directory de sua organização](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você poderá criar um locatário do Azure AD no Partner Center sem nenhum preço adicional.

2. [Adicionar um aplicativo do Azure AD à sua conta do Partner Center](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Esse aplicativo do Azure AD representa o aplicativo ou o serviço que você usará para acessar envios para sua conta do Centro de Desenvolvimento. Você deve atribuir esse aplicativo à função **Gerente**. Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Recuperar as credenciais necessárias para envios

Em seguida, você poderá recuperar as credenciais do Partner Center necessárias para envios: a **ID do Locatário do Azure**, a **ID do Cliente** e a **Chave do cliente**.

1. Acesse o [painel do Partner Center](https://partner.microsoft.com/dashboard) e entre em com suas credenciais do Azure AD.

2. No painel do Partner Center, selecione o ícone de engrenagem (ao lado do canto superior direito do painel) e selecione as **Configurações do desenvolvedor**.

3. No menu **Configurações** no painel esquerdo, clique em **Usuários**.

4. Clique no nome do seu aplicativo do Azure AD para acessar as configurações do aplicativo. Nesta página, copie os valores da **ID do Locatário** e da **ID do Cliente**.

5. Na seção **Chaves**, clique em **Adicionar nova chave**. Na próxima tela, copie o valor **Chave**, que corresponde ao segredo do cliente. Você não poderá acessar essas informações novamente depois que sair desta página; portanto, não as perca. Para obter mais informações, confira [Gerenciar chaves para um aplicativo do Azure AD](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Configurar envios automáticos à Store no Visual Studio

Após concluir as etapas anteriores, você poderá configurar envios automáticos à Store no Visual Studio 2019.

1. No fim do [assistente para Criar pacotes do aplicativo](#create-your-app-package-upload-file-using-visual-studio), selecione **Enviar automaticamente para a Microsoft Store após a validação do Kit de Certificação de Aplicativos Windows** e clique **Reconfigurar**.

2. Na caixa de diálogo **Definir configurações de Envio à Microsoft Store**, insira a ID do locatário, a ID do Cliente e a chave do Cliente do Azure.

    ![Definir configurações de Envio à Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Suas credenciais podem ser salvas em seu perfil para serem usados em envios futuros

3. Clique em **OK**.

O envio será iniciado depois que o teste de WACK for concluído. Você pode acompanhar o progresso do envio na janela **Verificar e Publicar**.

![Verificar e Publicar o progresso](images/packaging-screen9.jpg)
