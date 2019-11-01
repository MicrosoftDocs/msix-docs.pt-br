---
description: Este artigo fornece orientação sobre como executar, depurar e testar seu aplicativo de área de trabalho empacotado para prepará-lo para implantação.
title: Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 7c8faa1ec1c6e8fcd86a59be46143d0d42b3b5ad
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328305"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Executar, depurar e testar um aplicativo de área de trabalho empacotado

Execute o aplicativo empacotado e veja como ele se parece sem precisar assinar. Em seguida, defina pontos de interrupção e percorra o código. Quando você estiver pronto para testar seu aplicativo em um ambiente de produção, assine seu aplicativo e, em seguida, instale-o. Este tópico mostra como fazer cada uma dessas coisas.

<a id="run-app" />

## <a name="run-your-application"></a>Executar seu aplicativo

Você pode executar seu aplicativo para testá-lo localmente sem ter que obter um certificado e conectá-lo. A forma como você executa o aplicativo depende de qual ferramenta foi usada para criar o pacote.

### <a name="you-created-the-package-by-using-visual-studio"></a>Você criou o pacote usando o Visual Studio

Defina o projeto de empacotamento como o projeto de inicialização e, em seguida, pressione CTRL+F5 para iniciar seu aplicativo.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Você criou o pacote manualmente ou usando o Desktop App Converter

Abra um aviso de comando do Windows PowerShell e, na subpasta **PackageFiles** da sua pasta de saída, execute este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar seu aplicativo, encontre-o no menu Iniciar do Windows.

![Aplicativo empacotado no menu iniciar](images/converted-app-installed.png)

> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo, e qualquer unidade na qual você instala o aplicativo empacotado deve ser formatada para o formato NTFS.

## <a name="debug-your-app"></a>Depurar seu aplicativo

A forma como você depura o aplicativo depende de qual ferramenta você usou para criar o pacote.

Se você criou seu pacote usando o [novo projeto de empacotamento](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponível na versão 15.4 do Visual Studio 2017, defina o projeto de empacotamento como o projeto de inicialização e, em seguida, pressione F5 para depurar seu aplicativo.

Se você criou seu pacote usando qualquer outra ferramenta, siga estas etapas.

1. Certifique-se de iniciar o aplicativo empacotado pelo menos uma vez para que ele seja instalado no computador local.

   Consulte a seção [Executar seu aplicativo](#run-app) acima.

2. Inicie o Visual Studio.

   Se você quiser depurar seu aplicativo com permissões elevadas, inicie o Visual Studio usando a opção **Executar como administrador** .

3. No Visual Studio, escolha **Depurar**->**Outros Destinos de Depuração**->**Depurar Pacote de Aplicativo Instalado**.

4. Na lista **Pacotes de Aplicativo Instalados**, selecione o pacote de aplicativo e então escolha o botão **Anexar**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modificar seu aplicativo entre sessões de depuração

Se você fizer suas alterações em seu aplicativo para corrigir bugs, empacote-o novamente usando a ferramenta MakeAppx. Consulte [Executar a ferramenta MakeAppx](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-application-lifecycle"></a>Depurar todo o ciclo de vida do aplicativo

Em alguns casos, talvez você queira um controle mais refinado sobre o processo de depuração, incluindo a capacidade de depurar seu aplicativo antes que ele seja iniciado.

Você pode usar o [o plmdebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obter controle total sobre o ciclo de vida do aplicativo, incluindo suspender, continuar e encerrar.

O [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) está incluso no SDK do Windows.

## <a name="test-your-app"></a>Testar o app

Para testar seu aplicativo em uma configuração realista enquanto você se prepara para a distribuição, é melhor assinar seu aplicativo e instalá-lo.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Testar um aplicativo que você empacotau usando o Visual Studio

O Visual Studio assina seu aplicativo usando um certificado de teste. Você encontrará esse certificado na pasta de saída gerada pelo assistente **Criar pacotes de aplicativo**. O arquivo de certificado tem a extensão *. cer* e você precisará instalar esse certificado no repositório de **autoridades de certificação raiz confiáveis** no computador no qual você deseja testar seu aplicativo. Consulte [Fazer o sideload do pacote](../package/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Testar um aplicativo que você empacota usando o conversor de aplicativo de área de trabalho (DAC)

Se você empacotar seu aplicativo usando o conversor de aplicativo de desktop, poderá usar o parâmetro ``sign`` para assinar automaticamente seu aplicativo usando um certificado gerado. Você precisará instalar esse certificado e, em seguida, instalar o aplicativo. Consulte [Executar o aplicativo empacotado](desktop-to-uwp-run-desktop-app-converter.md#run-app).


### <a name="manually-sign-apps-optional"></a>Assinar aplicativos manualmente (opcional)

Você também pode assinar seu aplicativo manualmente. Veja como

1. Crie um certificado. Consulte [Criar um certificado](../package/create-certificate-package-signing.md).

2. Instale esse certificado para no repositório de certificados **Trusted Root** ou **Trusted People** em seu sistema.

3. Assine seu aplicativo usando esse certificado, consulte [assinar um pacote de aplicativo usando Signtool](../package/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Certifique-se de que o nome do fornecedor no certificado corresponde ao nome do fornecedor do seu aplicativo.

**Exemplo relacionado**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Testar seu aplicativo para Windows 10 S

Antes de publicar seu aplicativo, certifique-se de que ele funcionará corretamente em dispositivos que executam o Windows 10 S. Na verdade, se você planeja publicar seu aplicativo no Microsoft Store, você deve fazer isso porque é um requisito de armazenamento. Os aplicativos que não funcionam corretamente em dispositivos que executam o Windows 10 S não serão certificados.

Consulte [testar seu aplicativo do Windows para Windows 10 S](desktop-to-uwp-test-windows-s.md).

### <a name="run-another-process-inside-the-full-trust-container"></a>Executar outro processo dentro do recipiente de confiança total

Você pode chamar processos personalizados dentro do contêiner de um pacote do aplicativo especificado. Isso pode ser útil para testar os cenários (por exemplo, se você tiver um utilitário de teste personalizado e deseja testar a saída do aplicativo). Para fazer isso, use o cmdlet do PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
