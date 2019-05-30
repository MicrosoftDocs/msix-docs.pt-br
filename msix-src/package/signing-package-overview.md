# <a name="signing-windows-10-app-package"></a>Assinatura do pacote de aplicativo do Windows 10 

Assinatura de pacote de aplicativo é uma etapa necessária no processo de criação de um pacote de aplicativo do Windows 10 que pode ser implantado. Windows 10 requer que todos os aplicativos sejam assinados com um certificado de assinatura de código válido. 

Para instalar com êxito um aplicativo do Windows 10, o pacote não só precisa ser assinado, mas também confiável no dispositivo. Isso significa que o certificado tenha encadear para uma das raízes confiáveis no dispositivo. Windows 10 ser padrão, confia em certificados da maioria da autoridade de certificação que forneça certificados de assinatura de código. 

|Tópico| Descrição |
|:---|:---|
|[Pré-requisitos para assinatura](https://docs.microsoft.com/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#prerequisites)| Esta seção aborda os pré-requisitos necessários para assinar o pacote de aplicativo do Windows 10. | 
|[Usando o SignTool](https://docs.microsoft.com/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#using-signtool)| Esta seção discute como usar o SignTool do SDK do Windows 10 para assinar o pacote do aplicativo.|

## <a name="timestamping"></a>Carimbo de hora 

Além da assinatura do pacote de aplicativo com um certificado, é outra característica importante que determina a validade do código do certificado de autenticação **carimbo**. Carimbo preserva a assinatura, permitindo que o pacote do aplicativo aceita pela plataforma de implantação do aplicativo, mesmo depois que o certificado expirou. No momento de inspeção de pacote, o carimbo de hora permite que a assinatura do pacote a ser validado em relação a hora em que ele foi assinado. Isso permite que pacotes sejam aceitas, mesmo depois que o certificado não é mais válido. Pacotes que não estão em data/hora serão avaliados em relação a hora atual e se o certificado não for válido, o Windows não aceitará o pacote. 

A seguir estão os diferentes cenários em torno de carimbo com entrada/saída de assinatura do aplicativo:

| |Aplicativo está assinado sem carimbo de hora | Aplicativo é assinado com carimbo de hora |
|---|---------------------------------- | ------------------------------- |
| O certificado é válido |Aplicativo será instalado | Aplicativo será instalado |
| O certificado é invalid(expired) | Aplicativo não será instalado | Aplicativo será instalado como a autenticidade do certificado foi verificada durante a assinatura pela autoridade de carimbo de hora |

 > [!NOTE]
 > Se o aplicativo é instalado com êxito em um dispositivo, ele continuará a executar até mesmo após a expiração do certificado, independentemente de ele sendo data/hora ou não. 

## <a name="device-mode"></a>Modo de dispositivo

Windows 10 permite que os usuários selecionem o modo no qual executar o seu dispositivo no aplicativo configurações. Os modos são aplicativos da Microsoft Store, aplicativos de Sideload e modo de desenvolvedor. 

**Aplicativos da Microsoft Store** é mais seguro, pois ele apenas permite a instalação de aplicativos da Microsoft Store. Aplicativos em que a Microsoft Store passam pelo processo de certificação para garantir que os aplicativos são seguros para uso. 

**Aplicativos de sideload** e **modo de desenvolvedor** são mais permissivas de aplicativos que são assinados por outros certificados, desde que esses certificados são confiáveis e de cadeia para uma das raízes confiáveis no dispositivo. Somente Selecione modo de desenvolvedor, se você for um desenvolvedor e aplicativos do Windows 10 compilação ou de depuração. Encontre mais informações sobre o modo de desenvolvedor e o que ele oferece [aqui](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development). 
