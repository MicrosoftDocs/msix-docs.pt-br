---
Description: Execute o Desktop App Converter para empacotar um aplicativo da área de trabalho do Windows (como Win32, WPF e Windows Forms).
title: Empacotar um app usando o Desktop App Converter (Ponte de Desktop)
ms.date: 08/21/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: 6959d762430094cab449a9168defc8aac673fdc1
ms.sourcegitcommit: 6173086c11ffeb5fa836da6bd42711a9a626fc0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66411451"
---
# <a name="package-a-desktop-application-using-the-desktop-app-converter"></a>Empacotar um aplicativo da área de trabalho usando o Desktop App Converter

> [!NOTE]
> A ferramenta de Desktop App Converter foi preterida. É recomendável que você use o [ferramenta de empacotamento MSIX](../packaging-tool/create-app-package-msi-vm.md) em vez disso.

![Ícone DAC](images/dac.png)

O Desktop App Converter (DAC) cria pacotes para aplicativos da área de trabalho para integração com os recursos mais recentes do Windows, incluindo a distribuição e manutenção por meio do Microsoft Store. Isso inclui aplicativos Win32 e aplicativos que você criou usando o .NET 4.6.1.

[Baixar o Desktop App Converter](https://aka.ms/converter)

Embora o termo "Converter" apareça no nome dessa ferramenta, na verdade ela não converte seu app. Seu aplicativo permanece inalterado. Entretanto, essa ferramenta gera um pacote de aplicativo do Windows com um identificador de pacote e a capacidade para chamar uma grande variedade de APIs do WinRT.

Você pode instalar esse pacote usando o cmdlet Add-AppxPackage PowerShell em sua máquina de desenvolvimento.

O conversor executa o instalador da área de trabalho em um ambiente Windows isolado usando uma imagem de base limpa fornecida como parte do download do conversor. Ele captura qualquer E/S do registro e do sistema de arquivos feita pelo instalador da área de trabalho e a empacota como parte da saída.

> [!IMPORTANT]
> Há suporte para o Desktop App Converter de no Windows 10, versão 1607 e posterior. Ele só pode ser usado em projetos que se destinam a atualização de aniversário do Windows 10 (10.0; Build 14393) ou uma versão posterior no Visual Studio.

> [!NOTE]
> Confira <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">essa série</a> de vídeos de curta duração publicados na Microsoft Virtual Academy. Esses vídeos o orientam através de algumas maneiras comuns de usar o Desktop App Converter.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>O DAC faz mais do que apenas gerar um pacote para você

Aqui estão algumas coisas extras que ele pode fazer por você.

**Windows 10 Creators Update**

:heavy_check_mark: Registre automaticamente seus manipuladores de visualização, manipuladores de miniaturas, manipuladores de propriedades, regras de firewall, sinalizadores de URL.

:heavy_check_mark: Registre automaticamente os mapeamentos do tipo de arquivo que permitem aos usuários agrupar arquivos usando a coluna **Tipo** no Explorador de Arquivos.

:heavy_check_mark: Registre os servidores COM públicos.

**Atualização de aniversário do Windows 10 ou posterior**

:heavy_check_mark: Assine automaticamente o seu pacote para que você possa testar seu aplicativo.

:heavy_check_mark: Valide seu aplicativo contra aplicativos empacotados e requisitos da Microsoft Store.

Para encontrar uma lista completa de opções, veja a seção [Parâmetros](#command-reference) deste guia.

Se você estiver pronto para criar seu pacote, vamos começar.

## <a name="first-prepare-your-application"></a>Primeiro, prepare seu aplicativo

Examine este guia antes de começar a criar um pacote para seu aplicativo: [Preparar para empacotar um aplicativo da área de trabalho](desktop-to-uwp-prepare.md).

## <a name="make-sure-that-your-system-can-run-the-converter"></a>Certifique-se de que seu sistema possa executar o conversor

Certifique-se de que seu sistema atenda aos seguintes requisitos:

* Atualização de Aniversário do Windows 10 (10.0.14393.0 e posterior) Pro ou Enterprise Edition.
* Processador de 64 bits (x64)
* Virtualização assistida por hardware
* Conversão de Endereços de Segundo Nível (SLAT)
* [Software Development Kit do Windows (SDK do Windows) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375).

## <a name="start-the-desktop-app-converter"></a>Inicie o Desktop App Converter

1. Baixe e instale o [Desktop App Converter](https://aka.ms/converter).

2. Execute o Desktop App Converter como administrador.

    ![Execute o DAC como administrador](images/run-converter.png)

   Aparece uma janela de console. Você usará essa janela de console para executar comandos.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>Ajuste algumas coisas (aplicativos apenas com instaladores)

Você pode pular para a próxima seção se seu aplicativo não tiver um instalador.

1. Identifique o número da versão do seu sistema operacional.

   Para fazer isso, digite **winver** na caixa de diálogo **Executar** e escolha o botão **OK**.

   ![winver](images/winver.png)

   Você encontrará a versão da compilação do Windows usando a caixa de diálogo **Sobre o Windows**.

   ![Versão do Windows 10](images/win-10-version.png)

2. Baixe a imagem de base apropriada do [Desktop App Converter](https://aka.ms/converterimages).

   Verifique se o número da versão que aparece no nome do arquivo corresponde ao número da versão da compilação do Windows.

   >[!IMPORTANT]
   > Se você estiver usando o número de build **15063**, e a versão secundária da compilação é maior ou igual a **.483** (por exemplo: **15063.540**), certifique-se de baixar o **15063-BaseImage-UPDATE.wim** arquivo. Se a versão secundária de compilação for inferior a **.483**, baixe o arquivo **BaseImage 15063.wim**. Se você já tiver configurado uma versão incompatível desse arquivo base, é possível corrigi-lo. Essa [postagem de blog](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/) explica como fazer isso.

3. Coloque o arquivo baixado em qualquer lugar em seu computador, onde você poderá encontrá-lo mais tarde.

4. Na janela do console que apareceu quando você iniciou o Desktop App Converter, execute este comando: ```Set-ExecutionPolicy bypass```.
5. Configure o conversor executando este comando: ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```.
6. Reinicie seu computador se você for solicitado a fazê-lo.

   As mensagens de status aparecem na janela do console, pois o conversor expande a imagem base. Se você não ver nenhuma mensagem de status, pressione qualquer tecla. Isso pode fazer com que o conteúdo da janela do console seja atualizado.

   ![Mensagens de status na janela do console](images/bas-image-setup.png)

   Quando a imagem base estiver totalmente expandida, vá para a próxima seção.

## <a name="package-an-app"></a>Empacotar um app

Para empacotar seu aplicativo, execute o comando ``DesktopAppConverter.exe`` na janela do console que abriu quando você iniciou o Desktop App Converter.  

Você especificará o número de nome, editor e versão do pacote do aplicativo usando parâmetros.

> [!NOTE]
> Se você tiver reservado o nome do aplicativo em que a Microsoft Store, você pode obter os nomes de pacote e o publicador usando [Partner Center](https://partner.microsoft.com/dashboard). Se você pretende fazer o sideload do aplicativo para outros sistemas, você pode fornecer seus próprios nomes para esses, desde que o nome de fornecedor que você escolher corresponda ao nome no certificado usado para assinar seu aplicativo.

### <a name="a-quick-look-at-command-parameters"></a>Um olhar rápido sobre os parâmetros do comando

Aqui estão os parâmetros necessários.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
Você pode ler sobre cada um [aqui](#command-reference).

### <a name="examples"></a>Exemplos

Aqui estão algumas maneiras comuns de empacotar seu aplicativo.

* [Empacotar um aplicativo que tem um arquivo do instalador (. msi)](#installer-conversion)
* [Empacotar um aplicativo que tem um arquivo executável de instalação](#setup-conversion)
* [Empacotar um aplicativo que não tem um instalador](#no-installer-conversion)
* [Empacotar um aplicativo, assinar o aplicativo e prepará-lo para envio de Store](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-application-that-has-an-installer-msi-file"></a>Empacotar um aplicativo que tem um arquivo do instalador (. msi)

Aponte para o arquivo instalador usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> Há dois coisas importantes para ter em mente aqui. Primeiro, certifique-se de que seu instalador esteja localizado em uma pasta independente e que apenas os arquivos relacionados a esse instalador estão na mesma pasta. O conversor copia todo o conteúdo dessa pasta para o ambiente Windows isolado. <br> Em segundo lugar, se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.  

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

Se o instalador incluir instaladores para bibliotecas ou estruturas dependentes, talvez você precise organizar itens de uma forma um pouco diferente. Consulte [Encadeamento de vários instaladores com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/).

<a id="setup-conversion" />

#### <a name="package-an-application-that-has-a-setup-executable-file"></a>Empacotar um aplicativo que tem um arquivo executável de instalação

Aponte para o executável de instalação usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```
>[!IMPORTANT]
>Se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

O parâmetro ``InstallerArguments`` é um parâmetro opcional. No entanto, como o Desktop App Converter precisa seu instalador a ser executado no modo autônomo, você talvez precise usá-lo se seu aplicativo precisar silenciosas sinalizadores para ser executado silenciosamente. O sinalizador ``/S`` é um sinalizador silencioso muito comum, mas o sinalizador que você usa pode ser diferente dependendo da tecnologia do instalador que você usou para criar o arquivo de configuração.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="no-installer-conversion" />

#### <a name="package-an-application-that-doesnt-have-an-installer"></a>Empacotar um aplicativo que não tem um instalador

Neste exemplo, use o ``Installer`` parâmetro para apontar para a pasta raiz dos arquivos do aplicativo.

Use o parâmetro `AppExecutable` para apontar para o arquivo executável de seus aplicativos.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>Empacotar um app, assinar o app e executar verificações de validação no pacote

Este exemplo é semelhante à primeira, exceto que ele mostra como você pode assinar seu aplicativo para teste local e, em seguida, valide seu aplicativo contra aplicativos empacotados e requisitos da Microsoft Store.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>Se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

O ``Sign`` parâmetro gera um certificado e, em seguida, assina seu aplicativo com ele. Para executar seu aplicativo, você terá que instalar esse certificado gerado. Para saber como, consulte a seção [Executar o aplicativo empacotado](#run-app) deste guia.

Você pode validar você aplicativo usando o ``Verify`` parâmetro.

### <a name="a-quick-look-at-optional-parameters"></a>Uma análise rápida dos parâmetros opcionais

Os parâmetros ``Sign`` e ``Verify`` são opcionais. Existem muitos outros parâmetros opcionais.  Aqui estão alguns dos parâmetros opcionais mais comumente usados.

```CMD
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]
```
Você pode ler sobre todos eles na próxima seção.

<a id="command-reference" />

### <a name="parameter-reference"></a>Parâmetro Referência

Aqui está a lista completa de parâmetros (organizados por categoria) que você pode usar quando você executa o Desktop App Converter.

* [Instalação](#setup-params)
* [Conversão](#conversion-params)
* [Identidade do pacote](#identity-params)
* [Manifesto do pacote](#manifest-params)
* [Limpeza](#cleanup-params)
* [Arquitetura](#architecture-params)
* [Diversos](#other-params)

Você também pode exibir toda a lista executando o comando ``Get-Help`` na janela do console do aplicativo.   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>Parâmetros de configuração</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |Obrigatório |Executa DesktopAppConverter no modo de instalação. O modo de instalação aceita a expansão de uma imagem de base fornecida.|
|-BaseImage &lt;Cadeia de Caracteres&gt; | Obrigatório |Caminho completo para uma imagem de base não expandida. Este parâmetro será necessário se -Setup for especificado.|
| -LogFile &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado.|
|-NatSubnetPrefix &lt;Cadeia de Caracteres&gt; |Opcional |Valor de prefixo a ser usado para a instância do Nat. Normalmente, você desejaria alterar isso somente se seu computador host fosse anexado à mesma faixa de sub-rede do NetNat do conversor. Você pode consultar a configuração atual do NetNat do conversor usando o cmdlet **Get-NetNat**. |
|-NoRestart [&lt;SwitchParameter&gt;] |Obrigatório |Não solicite a reinicialização ao executar a instalação (é necessário reiniciar para habilitar o recurso de contêiner). |
|<a id="conversion-params" /> <strong>Parâmetros de conversão</strong>|||
|-AppInstallPath &lt;Cadeia de Caracteres&gt;  |Opcional |O caminho completo da pasta raiz do aplicativo dos arquivos instalados se ele estivesse instalado (por exemplo, "C:\Program Files (x86) \MyApp").|
|-Destination &lt;Cadeia de Caracteres&gt; |Obrigatório |O destino desejado para a saída de appx do conversor - o DesktopAppConverter pode criar esse local caso ele ainda não exista.|
|-Installer &lt;Cadeia de Caracteres&gt; |Obrigatório |O caminho para o instalador para seu aplicativo - deve ser capaz de executar sem supervisão / silenciosamente. Conversão de não-installer, esse é o caminho para o diretório raiz dos arquivos do aplicativo. |
|-InstallerArguments &lt;Cadeia de Caracteres&gt; |Opcional |Uma lista separada por vírgulas ou cadeia de caracteres de argumentos para forçar o instalador a ser executado de forma autônoma/silenciosa. Este parâmetro é opcional se o instalador for um msi. Para obter um registro do seu instalador, forneça o argumento de registro para o instalador aqui e use o caminho &lt;log_folder&gt;, que é um token que o conversor substitui com o caminho apropriado. <br><br>**OBSERVAÇÃO**: Os argumentos de log e autônoma/silencioso sinalizadores irão variar entre tecnologias de instalador. <br><br>Um exemplo de uso para esse parâmetro: - InstallerArguments "/silent /log &lt;log_folder&gt;\install.log" outro exemplo que não produz um arquivo de log pode se parecer com: ```-InstallerArguments "/quiet", "/norestart"``` Novamente, você deve direcionar, literalmente, todos os logs para o caminho de token &lt;log_folder&gt; se você quiser que o conversor de capturá-lo e colocá-lo na pasta de log final.|
|-InstallerValidExitCodes &lt;Int32&gt; |Opcional |Uma lista separada por vírgulas dos códigos de saída que indicam seu instalador foi executado com êxito (por exemplo: 0, 1234, 5678).  Por padrão, o valor é 0 para não msi e 0, 1641, 3010 para msi.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |Opcional |Um botão que, quando presente, informa a este script para chamar MakeAppx na saída. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |Opcional |Uma opção que, quando presente, diz a este script para empacotar a saída como um pacote MSIX. |
|<a id="identity-params" /><strong>Parâmetros de identidade do pacote</strong>||
|-PackageName &lt;Cadeia de Caracteres&gt; |Obrigatório |O nome do seu pacote de aplicativos Universais do Windows. Se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro. |
|-Publisher &lt;Cadeia de Caracteres&gt; |Obrigatório |O editor do seu pacote de aplicativo Universal do Windows |
|-Version &lt;Versão&gt; |Obrigatório |O número de versão do seu pacote de aplicativo Universal do Windows |
|<a id="manifest-params" /><strong>Parâmetros de manifesto de pacote</strong>||
|-AppExecutable &lt;Cadeia de Caracteres&gt; |Opcional |O nome do executável principal do aplicativo (ex: "MyApp.exe"). Esse parâmetro é necessário para uma conversão sem instalador. |
|-AppFileTypes &lt;Cadeia de Caracteres&gt;|Opcional |Uma lista separada por vírgulas de tipos de arquivos com a qual o aplicativo será associado. Exemplo de uso: - AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir a Id do Aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. Em muitos casos, usar o *PackageName* funciona bem. No entanto, se o Partner Center atribui uma identidade para o pacote que começa com um número, certifique-se de que você também passe o <i>AppId -</i> parâmetro e usar somente o sufixo de cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro. |
|-AppDisplayName &lt;Cadeia de Caracteres&gt;  |Opcional |Especifica um valor para definir o Nome da exibição do aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-AppDescription &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir a Descrição do Aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|-PackageDisplayName &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir o nome da exibição do pacote no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-PackagePublisherDisplayName &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir o nome de exibição do publicador de pacotes no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *Publisher*. |
|<a id="cleanup-params" /><strong>Parâmetros de limpeza</strong>|||
|-Cleanup [&lt;Opção&gt;] |Obrigatório |Executa a limpeza de artefatos DesktopAppConverter. Há 3 opções válidas para o modo de limpeza. |
|-Cleanup All | |Exclui todas as imagens de base expandidas, remove todos os arquivos temporários do conversor, remove a rede de contêiner e desabilita o recurso Windows opcional, Contêineres. |
|-Cleanup WorkDirectory |Obrigatório |Remove todos os arquivos temporários do conversor. |
|-Cleanup ExpandedImage |Obrigatório |Exclui todas as imagens de base expandidas instaladas no computador host. |
|<a id="architecture-params" /><strong>Parâmetros de arquitetura do pacote</strong>|||
|-PackageArch &lt;Cadeia de Caracteres&gt; |Obrigatório |Gera um pacote com a arquitetura especificada. As opções válidas são 'x86' ou 'x64'; por exemplo, -PackageArch x86. Este parâmetro é opcional. Se não for especificado, o DesktopAppConverter tentará detectar automaticamente a arquitetura do pacote. Se a detecção automática falhar, o padrão será pacote x64. |
|<a id="other-params" /><strong>Diversos parâmetros</strong>|||
|-ExpandedBaseImage &lt;Cadeia de Caracteres&gt;  |Opcional |Caminho completo para uma imagem de base já expandida.|
|-LogFile &lt;Cadeia de Caracteres&gt;  |Opcional |Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado. |
| -Sign [&lt;SwitchParameter&gt;] |Opcional |Avisa este script para assinar o pacote de aplicativos do Windows de saída usando um certificado gerado para fins de teste. Essa opção deve estar presente junto com a opção ```-MakeAppx```. |
|&lt;Parâmetros comuns&gt; |Obrigatório |Esse cmdlet dá suporte a parâmetros comuns: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable* , *OutBuffer*, *PipelineVariable*, e *OutVariable*. Para obter mais informações, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216). |
| -Verify [&lt;SwitchParameter&gt;] |Opcional |Uma opção que, quando presente, diz o DAC para validar o pacote do aplicativo contra aplicativos empacotados e requisitos da Microsoft Store. O resultado é um relatório de validação "VerifyReport.xml", que é melhor visualizado em um navegador. Essa opção deve estar presente junto com a opção `-MakeAppx`. |
|-PublishComRegistrations| Opcional| Digitaliza todos os registos COM públicos feitos pelo seu instalador e publica os válidos no seu manifesto. Use este sinalizador somente se desejar disponibilizar esses registros para outros aplicativos. Você não precisa usar este sinalizador se esses registros forem usados apenas pelo seu aplicativo. <br><br>Examine [este artigo](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97) para certificar-se de que seus registros COM se comportam como esperado após o empacotamento do seu app.

<a id="run-app" />

## <a name="run-the-packaged-app"></a>Execute o app empacotado

Há dois meios de executar seu aplicativo.

Uma maneira é abrir um prompt de comando do PowerShell e, em seguida, digitar este comando ```Add-AppxPackage –Register AppxManifest.xml```. Provavelmente é a maneira mais fácil para executar seu aplicativo porque você não tem a assinatura.

Outra maneira é assinar seu aplicativo com um certificado. Se você usar o ```sign``` parâmetro, o Desktop App Converter gerará um para você e, em seguida, assinar seu aplicativo com ele. O arquivo recebe o nome de **auto-generated.cer**, e você pode encontrá-lo na pasta raiz do seu app empacotado.

Siga estas etapas para instalar o certificado gerado e, em seguida, execute seu aplicativo.

1. Clique duas vezes no arquivo **auto-generated.cer** para instalar o certificado.

   ![arquivo de certificado gerado](images/generated-cert-file.png)

   > [!NOTE]
   > Se for solicitado uma senha a você, use a senha padrão "123456".

2. Na caixa de diálogo **Certificado**, clique no botão **Instalar certificado**.
3. No **Assistente de Importação de Certificados**, instale o certificado na **Máquina Local**, e coloque o certificado no repositório de certificados **Pessoas de confiança**.

   ![Repositório de pessoas de confiança](images/trusted-people-store.png)

5. Na pasta raiz de seu app empacotado, clique duas vezes no arquivo do pacote do aplicativo do Windows.

   ![Arquivo do pacote de aplicativo do Windows](images/windows-app-package.png)

6. Instale o aplicativo, pressionando o botão **Instalar**.

   ![Botão de instalação](images/install.png)


## <a name="modify-the-packaged-app"></a>Modificar o app empacotado

Provavelmente, você fará alterações ao seu aplicativo empacotado para resolver bugs, adicionar ativos visuais ou Aprimore seu aplicativo com experiências modernas, como blocos dinâmicos.

Depois de fazer suas alterações, você não precisa executar o conversor novamente. Na maioria dos casos, apenas é possível reempacotar seu aplicativo usando a ferramenta MakeAppx e o arquivo appxmanifest XML o DAC gera para seu aplicativo. Consulte [Gerar um pacote de aplicativo do Windows](desktop-to-uwp-manual-conversion.md#make-appx).

* Se você modificar qualquer um dos ativos visuais do aplicativo, gere um novo arquivo de Índice de recurso do pacote e execute a ferramenta MakeAppx para gerar um novo pacote. Consulte [Gerar um arquivo PRI (Índice de Recurso do Pacote)](desktop-to-uwp-manual-conversion.md#make-pri).

* Se você deseja adicionar ícone ou blocos que aparecem na barra de tarefas do Windows, na visão de tarefas, em LT+TAB, no Assistente de Ajuste e no canto inferior direito dos blocos em Iniciar, consulte [(Opcional) Adicionar ativos sem fundo com base em destino](desktop-to-uwp-manual-conversion.md#target-based-assets).

> [!NOTE]
> Se você fizer alterações nas configurações de registro que o instalador faz, você terá que executar o Conversor de aplicativos do desktop novamente para escolher essas alterações.

**Vídeos**

|Modificar e reempacotar a saída |Demo: Modificar e reempacotar a saída|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

As seções a seguir descrevem algumas correções opcionais para o aplicativo empacotado que você pode considerar.

### <a name="delete-unnecessary-files-and-registry-keys"></a>Exclua arquivos desnecessários e chaves de registro

O Desktop App Converter adota uma abordagem muito conservadora para filtrar arquivos e ruído do sistema no contêiner.

Se você quiser, você pode verificar a pasta VFS e excluir quaisquer arquivos que o seu instalador não necessita.  Você também pode rever o conteúdo do Reg.dat e excluir as chaves que não são instaladas/necessárias pelo aplicativo.

### <a name="fix-corrupted-pe-headers"></a>Corrigir cabeçalhos PE corrompidos

Durante o processo de conversão, o DesktopAppConverter executa automaticamente o PEHeaderCertFixTool para corrigir todos os cabeçalhos PE corrompidos. No entanto, você também pode executar o PEHeaderCertFixTool em um pacote de aplicativos UWP do Windows, arquivos soltos ou um binário específico. Aqui está um exemplo.

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>Telemetria do conversor de aplicativos da área de trabalho

O conversor de aplicativos da área de trabalho pode coletar informações sobre você e o seu uso do software e enviar essas informações para a Microsoft. Você pode saber mais sobre a coleta e o uso de dados da Microsoft na documentação do produto e na [Política de Privacidade da Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839). Você concorda em cumprir todas as cláusulas aplicáveis da Política de Privacidade da Microsoft.

Por padrão, a telemetria será habilitada para o conversor de aplicativos da área de trabalho. Adicione a seguinte chave do registro para definir a telemetria com uma configuração desejada:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Adicione ou edite o valor *DisableTelemetry* usando um DWORD definido como 1.
+ Para habilitar telemetria, remova a chave ou defina o valor como 0.

### <a name="language-support"></a>Suporte ao idioma

O Desktop App Converter não possui suporte para Unicode, assim, não podem ser usados caracteres chineses ou caracteres não ASCII com a ferramenta.

## <a name="known-issues-with-the-desktop-app-converter"></a>Problemas conhecidos com o Desktop App Converter

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED um E_STARTING_ISOLATED_ENV_FAILED erros    

Se você receber qualquer um desses erros, verifique se você está usando uma imagem base válida da [central de download](https://aka.ms/converterimages).
Se você estiver usando uma imagem base válida, tente usar ``-Cleanup All`` em seu comando.
Se isso não funcionar, envie-nos seus registros para converter@microsoft.com para nos ajudar a investigar.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: O objeto já existe o erro

Você pode receber esse erro ao configurar uma nova imagem base. Isso pode acontecer se você tiver versão de pré-lançamento do Windows Insider em uma máquina de desenvolvimento que anteriormente tinha o Desktop App Converter instalado.

Para resolver este problema, tente executar o comando `Netsh int ipv4 reset` a partir de um prompt de comando elevado e reinicie sua máquina.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>Seu aplicativo .NET é compilado com a opção de compilação "AnyCPU" e Falha na instalação

Isso pode acontecer se o executável principal ou qualquer uma das dependências foram colocadas em qualquer lugar na hierarquia de pastas **Arquivos de Programas** ou **Windows\System32**.

Para resolver este problema, tente usar seu instalador de desktop específico da arquitetura (32 bits ou 64 bits) para gerar um pacote de aplicativos do Windows.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>Publicar conjuntos públicos de Fusion lado a lado não funcionará

 Durante a instalação, um aplicativo pode publicar assemblies Fusion públicos lado a lado, acessíveis para qualquer outro processo. Durante a criação do contexto de ativação do processo, esses assemblies são recuperados por um processo do sistema denominado CSRSS.exe. Quando isso é feito em um processo convertido, a criação do contexto de ativação e o carregamento do módulo desses assemblies falhará. Os conjuntos Fusion lado a lado estão registrados nos seguintes locais:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de arquivos: % windir %\\SideBySide

Essa é uma limitação conhecida e não há atualmente nenhuma solução alternativa. Dito isso, as montagens da caixa de entrada, como o ComCtl, são fornecidas com o sistema operacional, portanto, ter uma dependência delas é seguro.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>Erro encontrado no XML. O atributo 'Executável' é inválido - O valor 'MyApp.EXE' é inválido de acordo com seu tipo de dados

Isso pode acontecer se os executáveis ​​em seu aplicativo tiverem uma extensão **.EXE** maiúscula. No entanto, o uso de maiusculas e minúsculas desta extensão não deve afetar se seu aplicativo é executado, isso pode causar o DAC gerar esse erro.

Para resolver esse problema, tente especificar o **AppExecutable -** sinalizar quando você empacota e use as letras minúsculas ".exe" como a extensão de seu executável principal (por exemplo: MYAPP.exe).    Como alternativa, você pode alterar as maiusculas e minúsculas para todos os executáveis em seu aplicativo de letra minúscula para maiuscula (por exemplo: do. EXE .exe).

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Assinaturas Authenticode corrompidas ou malformadas

Esta seção contém detalhes sobre como identificar problemas com arquivos PE em seu pacote de aplicativo do Windows que pode conter assinaturas Authenticode corrompidas ou malformadas. As assinaturas Authenticode inválidas em seus arquivos PE, que podem estar em qualquer formato binário (por exemplo, .exe,. dll,. chm, etc.), impedirão que o pacote seja assinado adequadamente e, portanto, impedirá que ele seja implantado de um pacote de aplicativo do Windows.

O local da assinatura Authenticode de um arquivo PE é especificado pela entrada da tabela de certificados nos diretórios de dados de cabeçalho opcionais e na tabela de certificados de atributos associados. Durante a verificação da assinatura, as informações especificadas nessas estruturas são usadas para localizar a assinatura em um arquivo PE. Se esses valores forem corrompidos, será possível que um arquivo pareça estar assinado de modo inválido.

Para a assinatura Authenticode estar correta, o seguinte deverá ser verdadeiro para a assinatura Authenticode:

- O início da entrada **WIN_CERTIFICATE** no arquivo PE não pode ultrapassar o final do executável
- A entrada **WIN_CERTIFCATE** deve estar localizada no final da imagem
- O tamanho da entrada **WIN_CERTIFICATE** deve ser positiva
- A entrada **WIN_CERTIFICATE** deve ser iniciada após a estrutura **IMAGE_NT_HEADERS32** para executáveis de 32 bits e a estrutura IMAGE_NT_HEADERS64 para executáveis de 64 bits

Para obter mais detalhes, consulte a [Especificação de Authenticode Portal Executable](https://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) e a [Especificação de formato de arquivo PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Observe que SignTool.exe pode gerar uma lista dos binários corrompidos ou malformados ao tentar assinar um pacote de aplicativo do Windows. Para fazer isso, ative o registro detalhado, definindo a variável de ambiente APPXSIP_LOG como 1 (por exemplo, ```set APPXSIP_LOG=1``` ) e execute novamente SignTool.exe.

Para corrigir esses binários malformados, certifique-se de que eles estejam em conformidade com os requisitos acima.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Executar o aplicativo / encontrar e corrigir problemas**

Consulte [executar, depurar e testar um aplicativo da área de trabalho de pacote](desktop-to-uwp-debug.md)

**Distribuir seu aplicativo**

Consulte [distribuir um aplicativo de área de trabalho do pacote](/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
