---
description: Este artigo descreve como assinar um pacote MSIX com a assinatura do Device Guard, que permite às empresas garantir que os aplicativos venham de uma fonte confiável.
title: Assinar um pacote do MSIX com a autenticação do Device Guard
ms.date: 10/26/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 4fefea6e25cb878ff9d6f6ccd50e25d34bd29885
ms.sourcegitcommit: 07decd168211fdecdee069bcfb5d073b89f904ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97350028"
---
# <a name="sign-an-msix-package-with-device-guard-signing"></a>Assinar um pacote do MSIX com a autenticação do Device Guard

> [!IMPORTANT]
> O [serviço de assinatura do Device Guard v2](https://docs.microsoft.com/microsoft-store/device-guard-signing-portal)(DGSS v2) já está disponível. Dezembro de 2020-o mecanismo existente baseado na Web para o serviço de assinatura do Device Guard V1 foi desativado e não está mais disponível para uso. Faça a transição para a versão baseada no PowerShell do serviço (DGSS v2). Um [pacote NuGet](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client) que contém os componentes necessários do DGSS V2 e a documentação de migração está disponível. Leia os termos de uso da Microsoft incluídos no pacote NuGet; Observe que o uso de DGSS implica a aceitação desses termos. Para dúvidas, entre em contato conosco em DGSSMigration@microsoft.com .

A [assinatura do Device Guard](/microsoft-store/device-guard-signing-portal) é um recurso do Device Guard que está disponível no Microsoft Store para negócios e educação. Ele permite que as empresas garantam que cada aplicativo vem de uma fonte confiável. Você pode usar SignTool no SDK do Windows e o DGSSv2 dlib no pacote NuGet para assinar seus aplicativos MSIX com a assinatura do Device Guard. Esse suporte ao recurso permite que você incorpore facilmente a assinatura do Device Guard no fluxo de trabalho de criação e assinatura de pacotes MSIX.

A assinatura do Device Guard requer permissões no Microsoft Store para negócios e usa a autenticação do Azure Active Directory (AD). Para assinar um pacote MSIX com a assinatura do Device Guard, siga estas etapas.

1. Se você ainda não fez isso, [Inscreva-se no Microsoft Store for Business ou Microsoft Store for Education](/microsoft-store/sign-up-microsoft-store-for-business).
    > [!NOTE]
    > Você só precisa usar este portal para configurar permissões para a assinatura do Device Guard.
2. No Microsoft Store para negócios (ou ou Microsoft Store para educação), atribua a si mesmo uma função com permissões necessárias para executar a assinatura do Device Guard.
3. Registre seu aplicativo no [portal do Azure](https://portal.azure.com/) com as configurações apropriadas para que você possa usar a autenticação do Azure AD com o Microsoft Store for Business.
4. Obtenha um token de acesso do Azure AD no formato JSON.
5. Execute SignTool para assinar seu pacote MSIX com a assinatura do Device Guard e passe o token de acesso do Azure AD obtido na etapa anterior.

As seções a seguir descrevem essas etapas mais detalhadamente.

## <a name="configure-permissions-for-device-guard-signing"></a>Configurar permissões para assinatura do Device Guard

Para usar a assinatura do Device Guard no Microsoft Store for Business ou Microsoft Store for Education, você precisa da função de **signatário do Device Guard** . Essa é a função de privilégios mínimos que tem a capacidade de assinar. Outras funções como **administrador global** e **proprietário da conta de cobrança** também podem assinar. 

 > [!NOTE]
 > A função de assinante do Device Guard é usada quando você está assinando como um aplicativo. O administrador global e o proprietário da conta de cobrança são usados quando você assina como uma pessoa conectada.

Para confirmar ou reatribuir funções:

1. Entre na [Microsoft Store para Empresas](https://businessstore.microsoft.com/).
2. Selecione **gerenciar** e selecione **permissões**.
3. Exibir **funções**.

Para obter mais informações, consulte [Funções e permissões na Microsoft Store para Empresas e Educação](/microsoft-store/roles-and-permissions-microsoft-store-for-business).

## <a name="register-your-app-in-the-azure-portal"></a>Registrar seu aplicativo no portal do Azure

Para registrar seu aplicativo com as configurações apropriadas para que você possa usar a autenticação do Azure AD com o Microsoft Store para negócios:

1. Entre no [portal do Azure](https://portal.azure.com/) e siga as instruções em [início rápido: registrar um aplicativo com a plataforma de identidade da Microsoft](/azure/active-directory/develop/quickstart-register-app) para registrar o aplicativo que usará a assinatura do Device Guard.

    > [!NOTE]
    > Na seção **URI de redirecionamento** , recomendamos que você escolha **cliente público (Mobile & Desktop)**. Caso contrário, se você escolher **Web** para o tipo de aplicativo, será necessário fornecer um [segredo do cliente](/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-credentials-to-your-web-application) ao obter um token de acesso do Azure ad posteriormente neste processo.

2. Depois de registrar seu aplicativo, na página principal para seu aplicativo no portal do Azure, clique em **permissões de API**, em **APIs minha organização usa** e adicione uma permissão para a **API da Windows Store para empresas**.

3. Em seguida, selecione **permissões delegadas** e, em seguida, selecione **user_impersonation**.

## <a name="get-an-azure-ad-access-token"></a>Obter um token de acesso do Azure AD

Em seguida, obtenha um token de acesso do Azure AD para seu aplicativo do Azure AD no formato JSON. Você pode fazer isso usando uma variedade de linguagens de programação e script. Para obter mais informações sobre esse processo, consulte [autorizar o acesso a Azure Active Directory aplicativos Web usando o fluxo de concessão de código OAuth 2,0](/azure/active-directory/develop/v1-protocols-oauth-code). Recomendamos que você recupere um [token de atualização](/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) junto com o token de acesso, pois seu token de acesso expirará em uma hora.

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
      'client_secret' = '<client_secret>'
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

## <a name="obtain-the-device-guard-signing-version-2-dll"></a>Obter a DLL de assinatura do Device Guard versão 2
Para assinar com a versão 2 da assinatura do Device Guard, obtenha o **Microsoft.Acs.Dlib.dll** baixando o [pacote NuGet](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) que será usado para assinar seu pacote. Isso também é necessário para obter o certificado raiz. 

## <a name="sign-your-package"></a>Assinar seu pacote

Depois de ter o token de acesso do AD do Azure, você estará pronto para usar o SignTool para assinar seu pacote com a assinatura do Device Guard. Para obter mais informações sobre como usar SignTool para assinar pacotes, consulte [assinar um pacote de aplicativo usando Signtool](/windows/uwp/packaging/sign-app-package-using-signtool?context=%252fwindows%252fmsix%252frender#prerequisites).

O exemplo de linha de comando a seguir demonstra como assinar um pacote com a versão 2 da assinatura do Device Guard.

```cmd
signtool sign /fd sha256 /dlib Microsoft.Acs.Dlib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * Recomendamos que você use uma das opções de carimbo de data/hora ao assinar seu pacote. Se você não aplicar um [carimbo de data/hora](signing-package-overview.md#timestamping), a assinatura expirará em um ano e o aplicativo precisará ser assinado novamente.
> * Verifique se o nome do Publicador no manifesto do pacote corresponde ao certificado que você está usando para assinar o pacote. Com esse recurso, será seu certificado de folha. Por exemplo, se o certificado de folha for **CompanyName**, o nome do editor no manifesto deverá ser **CN = CompanyName**. Caso contrário, a operação de assinatura falhará.
> * Há suporte apenas para o algoritmo SHA256.
> * Quando você assina seu pacote com a assinatura do Device Guard, seu pacote não está sendo enviado pela Internet.

## <a name="test"></a>Teste

Para testar, baixe o certificado raiz clicando [aqui](https://www.microsoft.com/pkiops/certs/microsoft%20enterprise%20identity%20verification%20root%20certificate%20authority%202020.crt) ou baixando o [pacote NuGet](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) e obtendo-o com o comando:
```cmd
Get-RootCertificate
```

Instale o certificado raiz para as **autoridades de certificação raiz confiáveis** em seu dispositivo. Instale seu aplicativo assinado recentemente para verificar se você assinou com êxito seu aplicativo com a assinatura do Device Guard. 

> [!IMPORTANT]
> Para obter o isolamento, implante a política de CI WDAC para confiar em aplicativos assinados com o DGSSv2. Certifique-se de ler a documentação readme_cmdlets e a migração da documentação do DGSSv1 para a DGSSv2 que está incluída no pacote NuGet. 

## <a name="common-errors"></a>Erros comuns

Aqui estão erros comuns que podem ser encontrados.

* 0x800700d: esse erro comum significa que o formato do arquivo JSON do Azure AD é inválido.
* Talvez seja necessário aceitar os termos e condições de Microsoft Store para negócios antes de baixar o certificado raiz da assinatura do Device Guard. Isso pode ser feito adquirindo um aplicativo gratuito no Portal.
