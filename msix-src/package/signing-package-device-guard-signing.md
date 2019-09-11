---
Description: Este artigo descreve a assinatura com a assinatura do Device Guard
title: Assinar um pacote do MSIX com a autenticação do Device Guard
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: f373b5a67e5af6eb0c88ae45c0a2b8f6e092ad39
ms.sourcegitcommit: 2eb663be861f2eb29f4882a15a4913ced9da833a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2019
ms.locfileid: "70014859"
---
# <a name="sign-an-msix-package-with-device-guard-signing"></a>Assinar um pacote do MSIX com a autenticação do Device Guard

A [assinatura do Device Guard](https://docs.microsoft.com/microsoft-store/device-guard-signing-portal) é um recurso do Device Guard que está disponível no Microsoft Store para negócios e educação. Ele permite que as empresas garantam que cada aplicativo vem de uma fonte confiável. A partir do Build 18945 do Windows 10 Insider Preview, você pode usar SignTool no SDK do Windows para assinar seus aplicativos MSIX com a assinatura do Device Guard. Esse suporte ao recurso permite que você incorpore facilmente a assinatura do Device Guard no fluxo de trabalho de criação e assinatura de pacotes MSIX.

A assinatura do Device Guard requer permissões no Microsoft Store para negócios e usa a autenticação do Azure Active Directory (AD). Para assinar um pacote MSIX com a assinatura do Device Guard, siga estas etapas.

1. Se você ainda não fez isso, [Inscreva-se no Microsoft Store for Business ou Microsoft Store for Education](https://docs.microsoft.com/microsoft-store/sign-up-microsoft-store-for-business).
    > [!NOTE]
    > Você só precisa usar este portal para configurar permissões para a assinatura do Device Guard.
2. No Microsoft Store para negócios (ou ou Microsoft Store para educação), atribua a si mesmo uma função com permissões necessárias para executar a assinatura do Device Guard.
3. Registre seu aplicativo no [portal do Azure](https://portal.azure.com/) com as configurações apropriadas para que você possa usar a autenticação do Azure AD com o Microsoft Store for Business.
4. Obtenha um token de acesso do Azure AD no formato JSON.
5. Execute SignTool para assinar seu pacote MSIX com a assinatura do Device Guard e passe o token de acesso do Azure AD obtido na etapa anterior.

As seções a seguir descrevem essas etapas mais detalhadamente.

## <a name="configure-permissions-for-device-guard-signing"></a>Configurar permissões para assinatura do Device Guard

Para usar a assinatura do Device Guard no Microsoft Store for Business ou Microsoft Store for Education, você precisa da função de **signatário do Device Guard** . Essa é a função de privilégios mínimos que tem a capacidade de assinar. Outras funções como **administrador global** e **proprietário da conta de cobrança** também podem assinar.

Para confirmar ou reatribuir funções:

1. Entre na [Microsoft Store para Empresas](https://businessstore.microsoft.com/).
2. Selecione **gerenciar** e selecione **permissões**.
3. Exibir **funções**.

Para obter mais informações, consulte [Funções e permissões na Microsoft Store para Empresas e Educação](https://docs.microsoft.com/microsoft-store/roles-and-permissions-microsoft-store-for-business).

## <a name="register-your-app-in-the-azure-portal"></a>Registrar seu aplicativo no portal do Azure

Para registrar seu aplicativo com as configurações apropriadas para que você possa usar a autenticação do Azure AD com o Microsoft Store para negócios:

1. Entre no [portal do Azure](https://portal.azure.com/) e siga as instruções em [início rápido: Registre um aplicativo com a plataforma](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app) de identidade da Microsoft para registrar o aplicativo que usará a assinatura do Device Guard.

    > [!NOTE]
    > Na seção **URI** de redirecionamento, recomendamos que você escolha **cliente público (Mobile & Desktop)** . Caso contrário, se você escolher **Web** para o tipo de aplicativo, será necessário fornecer um [segredo do cliente](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-credentials-to-your-web-application) ao obter um token de acesso do Azure ad posteriormente neste processo.

2. Depois de registrar seu aplicativo, na página principal para seu aplicativo no portal do Azure, clique em **permissões de API** e adicione uma permissão para a **API da Windows Store para empresas**.

3. Em seguida, selecione **permissões delegadas** e, em seguida, selecione **user_impersonation**.

## <a name="get-an-azure-ad-access-token"></a>Obter um token de acesso do AD do Azure

Em seguida, obtenha um token de acesso do Azure AD para seu aplicativo do Azure AD no formato JSON. Você pode fazer isso usando uma variedade de linguagens de programação e script. Para obter mais informações sobre esse processo, consulte autorizar o [acesso a Azure Active Directory aplicativos Web usando o fluxo de concessão de código OAuth 2,0](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Recomendamos que você recupere um [token de atualização](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) junto com o token de acesso, pois seu token de acesso expirará em uma hora.

> [!NOTE]
> Se você registrou seu aplicativo como um aplicativo **Web** no portal do Azure, deverá fornecer um segredo do cliente quando solicitar o token. Para obter mais informações, consulte a seção anterior.

O exemplo do PowerShell a seguir demonstra como solicitar um token de acesso.

```powershell
function GetToken()
{

    $c = Get-Credential -Credential $user
    
    $Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $c.UserName, $c.password
    $user = $Credentials.UserName
    $password = $Credentials.GetNetworkCredential().Password
    
    $tokenCache = "outfile.json"

    #replace <application-id> and <client_secret-id> with the Application ID from your Azure AD application registration
    $Body = @{
      'grant_type' = 'password'
      'client_id'= '<application-id>'
      'resource' = 'https://onestore.microsoft.com'
      'username' = $user
      'password' = $password
    }

    $webpage = Invoke-WebRequest 'https://login.microsoftonline.com/common/oauth2/token' -Method 'POST'  -Body $Body -UseBasicParsing
    $webpage.Content | Out-File $tokenCache -Encoding ascii
}
```

> [!NOTE]
> Recomandomos para salvar o arquivo JSON para uso posterior.

## <a name="sign-your-package"></a>Assinar seu pacote

Depois de ter o token de acesso do AD do Azure, você estará pronto para usar o SignTool para assinar seu pacote com a assinatura do Device Guard. Para obter mais informações sobre como usar SignTool para assinar pacotes, consulte [assinar um pacote de aplicativo usando Signtool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#prerequisites).

O exemplo de linha de comando a seguir demonstra como assinar um pacote com a assinatura do Device Guard.

```cmd
signtool sign /fd sha256 /dlib DgssLib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * Recomendamos que você use uma das opções de carimbo de data/hora ao assinar seu pacote. Se você não aplicar um [carimbo de data/hora](signing-package-overview.md#timestamping), a assinatura expirará em um ano e o aplicativo precisará ser assinado novamente.
> * Verifique se o nome do Publicador no manifesto do pacote corresponde ao certificado que você está usando para assinar o pacote. Com esse recurso, será seu certificado de folha. Por exemplo, se o certificado de folha for **CompanyName**, o nome do editor no manifesto deverá ser **CN = CompanyName**. Caso contrário, a operação de assinatura falhará.
> * Há suporte apenas para o algoritmo SHA256.
> * Quando você assina seu pacote com a assinatura do Device Guard, seu pacote não está sendo enviado pela Internet.

## <a name="test"></a>Teste

Para testar a assinatura do Device Guard, baixe o certificado raiz do organização da Microsoft Store para o portal de negócios.

1. Entre na [Microsoft Store para Empresas](https://businessstore.microsoft.com/).
2. Selecione **gerenciar** e, em seguida, **configurações**.
3. Exibir **dispositivos**.
4. Exibir **baixar o certificado raiz da sua organização para uso com o Device Guard**
5. Clique em **baixar**

Implante este certificado em seu dispositivo. Instale seu aplicativo assinado recentemente para verificar se você assinou com êxito seu aplicativo com a assinatura do Device Guard.

## <a name="common-errors"></a>Erros comuns

Aqui estão erros comuns que podem ser encontrados.

* 0x800700d: Esse erro comum significa que o formato do arquivo JSON do Azure AD é inválido.