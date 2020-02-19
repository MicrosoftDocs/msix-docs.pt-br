---
description: Este artigo oferece orientação sobre como executar, depurar e testar o aplicativo de área de trabalho empacotado para prepará-lo para implantação.
title: Executar, depurar e testar um aplicativo de área de trabalho empacotado (Ponte de Desktop)
ms.date: 02/03/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: f778c51afe1e25bbadca48dc2d9644e8645bdde3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072806"
---
# <a name="run-debug-and-test-an-msix-package"></a>Executar, depurar e testar um pacote MSIX

Execute o aplicativo empacotado e veja a aparência dele sem precisar autenticá-lo. Em seguida, defina pontos de interrupção e percorra o código. Quando estiver pronto para testar o aplicativo em um ambiente de produção, autentique o aplicativo e instale-o. Este tópico mostra como executar cada um desses itens.

<a id="run-app" />

## <a name="run-your-application"></a>Executar o aplicativo

Você pode executar o aplicativo para testá-lo de forma local sem precisar obter um certificado e assiná-lo. A forma como você executa o aplicativo depende da ferramenta usada para criar o pacote.

### <a name="you-created-the-package-by-using-visual-studio"></a>Você criou o pacote usando o Visual Studio

Defina o projeto de empacotamento como projeto de inicialização e pressione F5 para iniciar o aplicativo.

### <a name="you-created-the-package-using-a-different-tool"></a>Você criou o pacote usando uma ferramenta diferente

Abra um prompt de comando do Windows PowerShell e pelo diretório raiz dos arquivos do pacote execute este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar o aplicativo, encontre-o no menu Iniciar do Windows.

![Aplicativo empacotado no menu Iniciar](images/converted-app-installed.png)

> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo e qualquer unidade na qual você instale o aplicativo empacotado deve estar formatada em NTFS.

## <a name="debug-your-app"></a>Depurar o aplicativo

A forma como você depura o aplicativo depende da ferramenta usada para criar o pacote.

Se tiver criado o pacote usando o [novo projeto de empacotamento](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponível no Visual Studio 2017 versão 15.4 e posteriores (incluindo o Visual Studio 2019), basta definir o projeto de empacotamento como o projeto de inicialização e pressione F5 para depurar o aplicativo.

Se tiver criado o pacote usando qualquer outra ferramenta, siga estas etapas:

1. Assegure-se de iniciar o aplicativo empacotado pelo menos uma vez para que ele seja instalado no computador local.

   Consulte a seção [Executar o aplicativo](#run-app) acima.

2. Inicie o Visual Studio.

   Se quiser depurar o aplicativo com permissões elevadas, inicie o Visual Studio usando a opção **Executar como Administrador**.

3. No Visual Studio, escolha**Depurar**->**Outros Destinos de Depuração**->**Depurar Pacote de Aplicativo Instalado**.

4. Na lista **Pacotes de Aplicativo Instalados**, selecione o pacote de aplicativo e escolha o botão **Anexar**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modificar o aplicativo entre sessões de depuração

Se você fizer alterações no aplicativo para corrigir bugs, reempacote-o e use a ferramenta MakeAppx. Consulte [Executar a ferramenta MakeAppx](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-application-lifecycle"></a>Depurar todo o ciclo de vida do aplicativo

Em alguns casos, você poderá querer um controle mais refinado do processo de depuração incluindo a capacidade de depurar o aplicativo antes que ele seja iniciado.

Você pode usar o [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obter controle total do ciclo de vida do aplicativo, incluindo suspensão, retomada e encerramento.

O [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) está incluído no SDK do Windows.

## <a name="test-your-app"></a>Teste seu aplicativo

Para implantar o aplicativo empacotado em um teste completo do aplicativo empacotado durante a preparação para distribuição, é necessário autenticar o pacote com um certificado que seja confiável no computador em que você está implantando o aplicativo.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Testar um aplicativo empacotado usando o Visual Studio

O Visual Studio autentica o aplicativo usando um certificado de teste. Você encontrará o certificado na pasta de saída gerada pelo assistente **Criar Pacotes de Aplicativo**. O arquivo de certificado tem a extensão *.cer* e você precisará instalar o certificado no armazenamento de certificados **Pessoas Confiáveis** no computador em que deseja testar o aplicativo. Veja [Empacotar um aplicativo UWP ou de área de trabalho no Visual Studio](../package/packaging-uwp-apps.md#generate-an-app-package).

### <a name="test-an-application-that-you-packaged-using-a-different-tool"></a>Testar um aplicativo empacotado usando uma ferramenta diferente

Se tiver empacotado o aplicativo fora do Visual Studio será possível autenticar o pacote do aplicativo usando uma Ferramenta de Assinatura. Se o certificado usado para autenticar não for confiável no computador em que está sendo testado, será necessário instalá-lo no armazenamento de certificados Pessoas Confiáveis antes de instalar o pacote de aplicativo. 

#### <a name="sign-your-application-package"></a>Assinar o pacote do aplicativo

Para autenticar manualmente o pacote do aplicativo:

1. Crie um certificado. Veja [Criar um certificado](../package/create-certificate-package-signing.md).

2. Instale o certificado no armazenamento de certificados **Pessoas Confiáveis** no sistema.

3. Autentique o aplicativo usando o certificado, consulte [Autenticar um pacote de aplicativo usando a SignTool](../package/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Assegure-se de que o nome do editor no certificado corresponda ao nome do editor no aplicativo.

**Exemplo relacionado**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Testar o aplicativo no Windows 10 S

Antes de publicar o aplicativo, assegure-se de que ele funcionará corretamente em dispositivos com o Windows 10 S. Na verdade, se estiver planejando publicar o aplicativo na Microsoft Store será necessário fazer isso, já que é um requisito da loja. Os aplicativos que não funcionam corretamente em dispositivos com o Windows 10 S não serão certificados.

Veja [Testar o aplicativo Windows para Windows 10 S](desktop-to-uwp-test-windows-s.md).

### <a name="run-another-process-inside-the-full-trust-container"></a>Executar outro processo dentro do contêiner de confiança total

Você pode chamar processos personalizados dentro do contêiner de um pacote do aplicativo especificado. Isso pode ser útil para testar os cenários (por exemplo, se você tiver um utilitário de teste personalizado e deseja testar a saída do aplicativo). Para fazer isso, use o cmdlet do PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Próximas etapas

Tem dúvidas? Pergunte-nos na [MSIX Tech Community](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX).
