---
Description: Execute o Desktop App Converter para empacotar um aplicativo da área de trabalho do Windows (como o Win32, o WPF e o Windows Forms).
title: Empacotar um aplicativo usando o Desktop App Converter (Ponte de Desktop)
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: 251ee703973695fae70b9e4d84b4bb46cb889432
ms.sourcegitcommit: e3a06eccd3322053b8b498cb6343fb6f711a7a0b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724570"
---
# <a name="package-a-desktop-application-using-the-desktop-app-converter"></a>Empacotar um aplicativo da área de trabalho com o Desktop App Converter

> [!NOTE]
> A ferramenta Desktop App Converter foi preterida. Recomendamos usar a [Ferramenta de Empacotamento MSIX](../packaging-tool/create-app-package-msi-vm.md).

![Ícone do DAC](images/dac.png)

O DAC (Desktop App Converter) cria pacotes para aplicativos da área de trabalho para integração aos últimos recursos do Windows, incluindo distribuição e manutenção por meio da Microsoft Store. Isso inclui aplicativos Win32 e aplicativos que você criou usando o .NET 4.6.1.

[Baixar o Desktop App Converter](https://aka.ms/converter)

Embora o termo "Converter" apareça no nome dessa ferramenta, na verdade, ela não converte o aplicativo. O aplicativo permanece inalterado. Entretanto, essa ferramenta gera um pacote do aplicativo do Windows com um identificador de pacote e a capacidade para chamar uma grande variedade de APIs do WinRT.

Você pode instalar esse pacote usando o cmdlet Add-AppxPackage do PowerShell no computador de desenvolvimento.

O conversor executa o instalador da área de trabalho em um ambiente isolado do Windows usando uma imagem base limpa fornecida como parte do download do conversor. Ele captura qualquer E/S do registro e do sistema de arquivos feita pelo instalador da área de trabalho e a empacota como parte da saída.

> [!IMPORTANT]
> Há suporte para o Desktop App Converter no Windows 10, versão 1607 e posterior. Ele só pode ser usado em projetos direcionados ao Atualização de Aniversário do Windows 10 (10.0; build 14393) ou uma versão posterior no Visual Studio.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>O DAC faz mais do que apenas gerar um pacote para você

Veja abaixo algumas coisas extras que ele pode fazer por você.

**Atualização do Windows 10 para Criadores**

:heavy_check_mark: Registre automaticamente manipuladores de visualização, manipuladores de miniaturas, manipuladores de propriedades, regras de firewall e sinalizadores de URL.

:heavy_check_mark: Registre automaticamente os mapeamentos do tipo de arquivo que permitem aos usuários agrupar arquivos usando a coluna **Tipo** no Explorador de Arquivos.

:heavy_check_mark: Registre os servidores COM públicos.

**Atualização de Aniversário do Windows 10 ou posterior**

:heavy_check_mark: Assine automaticamente o pacote para testar o aplicativo.

:heavy_check_mark: Valide o aplicativo em relação aos requisitos do aplicativo empacotado e da Microsoft Store.

Para encontrar uma lista completa de opções, confira a seção [Parâmetros](#command-reference) deste guia.

Se você está pronto para criar o pacote, vamos começar.

## <a name="first-prepare-your-application"></a>Primeiro, prepare o aplicativo

Examine este guia antes de começar a criar um pacote para o aplicativo: [Preparar um pacote de um aplicativo de área de trabalho](desktop-to-uwp-prepare.md).

## <a name="make-sure-that-your-system-can-run-the-converter"></a>Verifique se o sistema pode executar o conversor

Verifique se o sistema atende aos seguintes requisitos:

* Atualização de Aniversário do Windows 10 (10.0.14393.0 e posterior) Pro ou Enterprise Edition.
* Processador de 64 bits (x64)
* Virtualização assistida por hardware
* SLAT (Conversão de Endereços de Segundo Nível)
* [Software Development Kit do Windows (SDK do Windows) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375).

## <a name="start-the-desktop-app-converter"></a>Iniciar o Desktop App Converter

1. Baixe e instale o [Desktop App Converter](https://aka.ms/converter).

2. Execute o Desktop App Converter como administrador.

    ![executar o DAC como administrador](images/run-converter.png)

   Uma janela do console é exibida. Você usará essa janela do console para executar comandos.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>Ajuste alguns itens (aplicativos apenas com instaladores)

Avance para a próxima seção se o aplicativo não tiver um instalador.

1. Identifique o número de versão do sistema operacional.

   Para fazer isso, digite **winver** na caixa de diálogo **Executar** e escolha o botão **OK**.

   ![winver](images/winver.png)

   Você encontrará a versão do build do Windows usando a caixa de diálogo **Sobre o Windows**.

   ![Versão do Windows 10](images/win-10-version.png)

2. Baixe a [imagem base apropriada do Desktop App Converter](https://aka.ms/converterimages).

   Verifique se o número de versão mostrado no nome do arquivo corresponde ao número da versão do build do Windows.

   >[!IMPORTANT]
   > Se estiver usando o número de build **15063** e a versão secundária do build for igual ou superior a **.483** (por exemplo: **15063,540**), baixe o arquivo **BaseImage-15063-UPDATE.wim**. Se a versão secundária do build for inferior à **.483**, baixe o arquivo **BaseImage-15063.wim**. Se você já tiver configurado uma versão incompatível desse arquivo base, poderá corrigi-lo. Esta [postagem no blog](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/) explica como fazer isso.

3. Coloque o arquivo baixado em qualquer lugar no computador, em que poderá encontrá-lo mais tarde.

4. Na janela do console exibida quando você iniciou o Desktop App Converter, execute este comando: ```Set-ExecutionPolicy bypass```.
5. Configure o conversor executando este comando: ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```.
6. Reinicie o computador se necessário.

   As mensagens de status são exibidas na janela do console à medida que o conversor expande a imagem base. Se nenhuma mensagem de status for exibida, selecione qualquer tecla. Isso pode fazer com que o conteúdo da janela do console seja atualizado.

   ![mensagens de status na janela do console](images/bas-image-setup.png)

   Quando a imagem base estiver totalmente expandida, vá para a próxima seção.

## <a name="package-an-app"></a>Empacotar um aplicativo

Para empacotar o aplicativo, execute o comando ``DesktopAppConverter.exe`` na janela do console que você abriu quando iniciou o Desktop App Converter.  

Você especificará o nome do pacote, o fornecedor e o número de versão do aplicativo usando parâmetros.

> [!NOTE]
> Se você tiver reservado o nome do aplicativo na Microsoft Store, poderá obter os nomes do pacote e do fornecedor usando a [Central de Parceiros](https://partner.microsoft.com/dashboard). Se você pretender fazer sideload do aplicativo em outros sistemas, poderá fornecer nomes para eles, desde que o nome de fornecedor que você escolher corresponda ao nome indicado no certificado usado para assinar o aplicativo.

### <a name="a-quick-look-at-command-parameters"></a>Uma visão rápida sobre os parâmetros do comando

Estes são os parâmetros obrigatórios.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
Leia sobre cada um deles [aqui](#command-reference).

### <a name="examples"></a>Exemplos

Veja a seguir algumas maneiras comuns de empacotar o aplicativo.

* [Empacotar um aplicativo que tenha um arquivo instalador (.msi)](#installer-conversion)
* [Empacotar um aplicativo que tenha um arquivo executável de instalação](#setup-conversion)
* [Empacotar um aplicativo que não tenha um instalador](#no-installer-conversion)
* [Empacotar um aplicativo, assiná-lo e prepará-lo para envio à Store](#optional-parameters)

<a id="installer-conversion"></a>

#### <a name="package-an-application-that-has-an-installer-msi-file"></a>Empacotar um aplicativo que tenha um arquivo instalador (.msi)

Aponte para o arquivo instalador usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> Há duas coisas importantes para ter em mente aqui. Primeiro, verifique se o instalador está localizado em uma pasta independente e que apenas os arquivos relacionados a esse instalador estão na mesma pasta. O conversor copia todo o conteúdo dessa pasta para o ambiente isolado do Windows. <br> Em segundo lugar, se a Central de Parceiros atribuir uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.  

Se o instalador incluir instaladores para bibliotecas ou estruturas dependentes, talvez você precise organizar os itens de uma forma um pouco diferente. Confira [Encadeamento de vários instaladores com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/).

<a id="setup-conversion"></a>

#### <a name="package-an-application-that-has-a-setup-executable-file"></a>Empacotar um aplicativo que tenha um arquivo executável de instalação

Aponte para o executável de instalação usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Caso a Central de Parceiros atribua uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

O parâmetro ``InstallerArguments`` é um parâmetro opcional. No entanto, como o Desktop App Converter precisa do instalador para ser executado no modo autônomo, você poderá precisar usá-lo se o aplicativo exigir sinalizadores silenciosos para a execução silenciosa. O sinalizador ``/S`` é um sinalizador silencioso muito comum, mas o sinalizador que você usará poderá ser diferente dependendo da tecnologia do instalador usada para criar o arquivo de configuração.

<a id="no-installer-conversion"></a>

#### <a name="package-an-application-that-doesnt-have-an-installer"></a>Empacotar um aplicativo que não tenha um instalador

Neste exemplo, use o parâmetro ``Installer`` para apontar para a pasta raiz dos arquivos do aplicativo.

Use o parâmetro `AppExecutable` para apontar para o arquivo executável dos aplicativos.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Caso a Central de Parceiros atribua uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

<a id="optional-parameters"></a>

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>Empacotar um aplicativo, assiná-lo e executar verificações de validação no pacote

Este exemplo é semelhante ao primeiro, exceto que mostra como você pode assinar o aplicativo para testes locais e validar o aplicativo empacotado em relação aos requisitos da Ponte de Desktop e da Microsoft Store.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>Caso a Central de Parceiros atribua uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro.

O parâmetro ``Sign`` gera um certificado e assina o aplicativo com ele. Para executar seu aplicativo, você precisará instalar esse certificado gerado. Para saber como fazer isso, confira a seção [Executar o aplicativo empacotado](#run-app) deste guia.

Você pode validar o aplicativo usando o parâmetro ``Verify``.

### <a name="a-quick-look-at-optional-parameters"></a>Uma visão rápida dos parâmetros opcionais

Os parâmetros ``Sign`` e ``Verify`` são opcionais. Existem muitos outros parâmetros opcionais.  Veja a seguir alguns dos parâmetros opcionais mais comumente usados.

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
Leia sobre todos eles na próxima seção.

<a id="command-reference"></a>

### <a name="parameter-reference"></a>Referência de parâmetro

Esta é a lista completa dos parâmetros (organizados por categoria) que você pode usar quando executa o Desktop App Converter.

* [Instalação](#setup-params)
* [Conversão](#conversion-params)
* [Identidade do pacote](#identity-params)
* [Manifesto de pacote](#manifest-params)
* [Limpeza](#cleanup-params)
* [Arquitetura](#architecture-params)
* [Diversos](#other-params)

Veja também toda a lista executando o comando ``Get-Help`` na janela do console do aplicativo.   

||||
|-------------|-----------|-------------|
|<a id="setup-params"></a> <strong>Parâmetros de configuração</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |Necessária |Executa DesktopAppConverter no modo de instalação. O modo de instalação aceita a expansão de uma imagem de base fornecida.|
|-BaseImage &lt;String&gt; | Necessária |Caminho completo para uma imagem de base não expandida. Este parâmetro será necessário se -Setup for especificado.|
| -LogFile &lt;String&gt; |Opcional |Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado.|
|-NatSubnetPrefix &lt;String&gt; |Opcional |Valor de prefixo a ser usado para a instância do Nat. Normalmente, você desejaria alterar isso somente se seu computador host fosse anexado à mesma faixa de sub-rede do NetNat do conversor. Você pode consultar a configuração atual do NetNat do conversor usando o cmdlet **Get-NetNat**. |
|-NoRestart [&lt;SwitchParameter&gt;] |Necessária |Não solicite a reinicialização ao executar a instalação (é necessário reiniciar para habilitar o recurso de contêiner). |
|<a id="conversion-params"></a> <strong>Parâmetros de conversão</strong>|||
|-AppInstallPath &lt;String&gt;  |Opcional |O caminho completo da pasta raiz do aplicativo dos arquivos instalados se ele estivesse instalado (por exemplo, "C:\Program Files (x86) \MyApp").|
|-Destination &lt;String&gt; |Necessária |O destino desejado para a saída de appx do conversor - o DesktopAppConverter pode criar esse local caso ele ainda não exista.|
|-Installer &lt;String&gt; |Necessária |O caminho para o instalador do aplicativo, que precisa conseguir ser executado de maneira autônoma/silenciosa. Conversão sem instalador, este é o caminho para o diretório raiz dos arquivos do aplicativo. |
|-InstallerArguments &lt;String&gt; |Opcional |Uma lista separada por vírgulas ou cadeia de caracteres de argumentos para forçar o instalador a ser executado de forma autônoma/silenciosa. Este parâmetro é opcional se o instalador for um msi. Para obter um registro do instalador, forneça o argumento de log para o instalador aqui e use o caminho &lt;log_folder&gt;, que é um token que o conversor substitui pelo caminho apropriado. <br><br>**OBSERVAÇÃO**: os argumentos de log e os sinalizadores autônomos/silenciosos variam de acordo com as tecnologias de instalador. <br><br>Um exemplo de uso deste parâmetro: -InstallerArguments "/silent /log &lt;log_folder&gt;\install.log". Outro exemplo que não produz um arquivo de log pode ser parecido com: ```-InstallerArguments "/quiet", "/norestart"``` Novamente, você precisará direcionar literalmente todos os logs para o caminho do token &lt;log_folder&gt; se desejar que o conversor o capture e o coloque na pasta de log final.|
|-InstallerValidExitCodes &lt;Int32&gt; |Opcional |Uma lista separada por vírgula de códigos de saída que indica que o instalador foi executado com êxito (por exemplo: 0, 1234, 5678).  Por padrão, o valor é 0 para não msi e 0, 1641, 3010 para msi.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |Opcional |Um botão que, quando presente, informa a este script para chamar MakeAppx na saída. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |Opcional |Uma opção que, quando presente, instrui este script a empacotar a saída como um pacote MSIX. |
|<a id="identity-params"></a><strong>Parâmetros de identidade do pacote</strong>||
|-PackageName &lt;String&gt; |Necessária |O nome do pacote do aplicativo universal do Windows. Caso a Central de Parceiros atribua uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro. |
|-Publisher &lt;String&gt; |Necessária |O editor do seu pacote de aplicativo Universal do Windows |
|-Version &lt;Version&gt; |Necessária |O número de versão do seu pacote de aplicativo Universal do Windows |
|<a id="manifest-params"></a><strong>Parâmetros do manifesto do pacote</strong>||
|-AppExecutable &lt;String&gt; |Opcional |O nome do executável principal do aplicativo (ex: "MyApp.exe"). Esse parâmetro é necessário para uma conversão sem instalador. |
|-AppFileTypes &lt;String&gt;|Opcional |Uma lista separada por vírgula de tipos de arquivos aos quais o aplicativo será associado. Exemplo de uso: -AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;String&gt; |Opcional |Especifica um valor para definir a ID do Aplicativo no manifesto do pacote de aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. Em muitos casos, o uso do *PackageName* funciona bem. No entanto, caso a Central de Parceiros atribua uma identidade ao pacote que comece com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador de ponto) como o valor desse parâmetro. |
|-AppDisplayName &lt;String&gt;  |Opcional |Especifica um valor para definir o nome da exibição do aplicativo no manifesto do pacote de aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-AppDescription &lt;String&gt; |Opcional |Especifica um valor para definir a descrição do aplicativo no manifesto do pacote de aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|-PackageDisplayName &lt;String&gt; |Opcional |Especifica um valor para definir o nome da exibição do pacote no manifesto do pacote de aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-PackagePublisherDisplayName &lt;String&gt; |Opcional |Especifica um valor para definir o nome de exibição do fornecedor de pacotes no manifesto do pacote de aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *Publisher*. |
|<a id="cleanup-params"></a><strong>Parâmetros de limpeza</strong>|||
|-Cleanup [&lt;Option&gt;] |Necessária |Executa a limpeza de artefatos DesktopAppConverter. Há 3 opções válidas para o modo de limpeza. |
|-Cleanup All | |Exclui todas as imagens de base expandidas, remove todos os arquivos temporários do conversor, remove a rede de contêiner e desabilita o recurso Windows opcional, Contêineres. |
|-Cleanup WorkDirectory |Necessária |Remove todos os arquivos temporários do conversor. |
|-Cleanup ExpandedImage |Necessária |Exclui todas as imagens de base expandidas instaladas no computador host. |
|<a id="architecture-params"></a><strong>Parâmetros da arquitetura do pacote</strong>|||
|-PackageArch &lt;String&gt; |Necessária |Gera um pacote com a arquitetura especificada. As opções válidas são 'x86' ou 'x64'; por exemplo, -PackageArch x86. Esse parâmetro é opcional. Se não for especificado, o DesktopAppConverter tentará detectar automaticamente a arquitetura do pacote. Se a detecção automática falhar, o padrão será pacote x64. |
|<a id="other-params"></a><strong>Parâmetros diversos</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |Opcional |Caminho completo para uma imagem de base já expandida.|
|-LogFile &lt;String&gt;  |Opcional |Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado. |
| -Sign [&lt;SwitchParameter&gt;] |Opcional |Instrui este script a assinar o pacote de aplicativo do Windows de saída usando um certificado gerado para fins de teste. Essa opção deve estar presente junto com a opção ```-MakeAppx```. |
|&lt;Parâmetros comuns&gt; |Necessária |Esse cmdlet dá suporte a parâmetros comuns: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* e *OutVariable*. Para obter mais informações, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216). |
| -Verify [&lt;SwitchParameter&gt;] |Opcional |Uma opção que, quando presente, instrui o DAC a validar o pacote de aplicativo em relação aos requisitos da Ponte de Desktop e da Microsoft Store. O resultado é um relatório de validação "VerifyReport.xml", que é mais bem visualizado em um navegador. Essa opção deve estar presente junto com a opção `-MakeAppx`. |
|-PublishComRegistrations| Opcional| Examina todos os registos COM públicos feitos pelo instalador e publica os válidos no manifesto. Use esse sinalizador somente se desejar disponibilizar esses registros para outros aplicativos. Você não precisará usar esse sinalizador se esses registros forem usados apenas pelo aplicativo. <br><br>Examine [este artigo](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97) para verificar se os registros COM se comportam como esperado após o empacotamento do aplicativo.

<a id="run-app"></a>

## <a name="run-the-packaged-app"></a>Executar o aplicativo empacotado

Há duas maneiras de executar o aplicativo.

Uma delas é abrir um prompt de comando do PowerShell e digitar este comando ```Add-AppxPackage –Register AppxManifest.xml```. Provavelmente, essa é a maneira mais fácil de executar o aplicativo, porque você não precisa assiná-lo.

Outra maneira é assinar o aplicativo com um certificado. Se você usar o parâmetro ```sign```, o Desktop App Converter vai gerar um para você e assinar o aplicativo com ele. O arquivo recebe o nome de **auto-generated.cer**, e você pode encontrá-lo na pasta raiz do aplicativo empacotado.

Siga estas etapas para instalar o certificado gerado e executar o aplicativo.

1. Clique duas vezes no arquivo **auto-generated.cer** para instalar o certificado.

   ![arquivo de certificado gerado](images/generated-cert-file.png)

   > [!NOTE]
   > Se precisar inserir uma senha, use a senha padrão "123456".

2. Na caixa de diálogo **Certificado**, clique no botão **Instalar Certificado**.
3. No **Assistente de Importação de Certificados**, instale o certificado no **Computador Local** e coloque o certificado no repositório de certificados **Pessoas Confiáveis**.

   ![Repositório Pessoas Confiáveis](images/trusted-people-store.png)

5. Na pasta raiz do aplicativo empacotado, clique duas vezes no arquivo do pacote de aplicativo do Windows.

   ![Arquivo do pacote de aplicativo do Windows](images/windows-app-package.png)

6. Instale o aplicativo selecionando o botão **Instalar**.

   ![Botão Instalar](images/install.png)


## <a name="modify-the-packaged-app"></a>Modificar o aplicativo empacotado

Provavelmente, você fará alterações no aplicativo empacotado para resolver bugs, adicionar ativos visuais ou aprimorar o aplicativo com experiências modernas, como blocos dinâmicos.

Depois de fazer as alterações, você não precisará executar o conversor novamente. Na maioria dos casos, você pode reempacotar o aplicativo usando a ferramenta MakeAppx e o arquivo appxmanifest.xml que o DAC gera para o aplicativo. Confira [Gerar um pacote de aplicativo do Windows](desktop-to-uwp-manual-conversion.md#make-appx).

* Se você modificar um dos ativos visuais do aplicativo, gere um novo arquivo de índice de recurso do pacote e execute a ferramenta MakeAppx para gerar um novo pacote. Confira [Gerar um arquivo PRI (índice de recurso do pacote)](desktop-to-uwp-manual-conversion.md#make-pri).

* Caso deseje adicionar ícones ou blocos que são exibidos na barra de tarefas do Windows, na exibição de tarefas, em LT+TAB, no Assistente de Ajuste e, no canto inferior direito dos blocos Iniciar, confira [(Opcional) Adicionar ativos sem fundo baseados no destino](desktop-to-uwp-manual-conversion.md#target-based-assets).

> [!NOTE]
> Se você fizer alterações nas configurações do Registro criadas pelo instalador, precisará executar o Desktop App Converter novamente para escolher essas alterações.

As duas seções a seguir descrevem algumas correções opcionais no aplicativo empacotado que podem ser consideradas.

### <a name="delete-unnecessary-files-and-registry-keys"></a>Excluir chaves do Registro e arquivos desnecessários

O Desktop App Converter adota uma abordagem muito conservadora para filtrar arquivos e ruído do sistema no contêiner.

Se desejar, você poderá examinar a pasta VFS e excluir os arquivos que o instalador não necessita.  Você também poderá examinar o conteúdo de Reg.dat e excluir as chaves que não são instaladas/exigidas pelo aplicativo.

### <a name="fix-corrupted-pe-headers"></a>Corrigir cabeçalhos PE corrompidos

Durante o processo de conversão, o DesktopAppConverter executa automaticamente a PEHeaderCertFixTool para corrigir todos os cabeçalhos PE corrompidos. No entanto, você também pode executar a PEHeaderCertFixTool em um pacote de aplicativos UWP do Windows, arquivos flexíveis ou um binário específico. Aqui está um exemplo.

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

## <a name="known-issues-with-the-desktop-app-converter"></a>Problemas conhecidos no Desktop App Converter

### <a name="e_creating_isolated_env_failed-and-e_starting_isolated_env_failed-errors"></a>Erros E_CREATING_ISOLATED_ENV_FAILED e E_STARTING_ISOLATED_ENV_FAILED    

Se você receber um desses erros, verifique se está usando uma imagem base válida do [Centro de Download](https://aka.ms/converterimages).
Se estiver usando uma imagem base válida, tente usar ``-Cleanup All`` no comando.
Se isso não funcionar, envie-nos os logs para o email converter@microsoft.com para nos ajudar a investigar o caso.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: erro "O objeto já existe"

Você poderá receber esse erro ao configurar uma nova imagem base. Isso poderá acontecer se você tiver uma versão de pré-lançamento do Windows Insider em um computador de desenvolvimento que anteriormente tinha o Desktop App Converter instalado.

Para resolver esse problema, tente executar o comando `Netsh int ipv4 reset` em um prompt de comandos com privilégios elevados e reinicie o computador.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>Seu aplicativo .NET está compilado com a opção de build "AnyCPU" e falha durante a instalação

Isso poderá acontecer se o executável principal ou uma das dependências forem colocadas em qualquer lugar na hierarquia de pastas **Arquivos de Programas** ou **Windows\System32**.

Para resolver esse problema, tente usar o instalador de desktop específico da arquitetura (32 ou 64 bits) para gerar um pacote de aplicativo do Windows.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>A publicação de assemblies Fusion públicos lado a lado não funciona

 Durante a instalação, um aplicativo pode publicar assemblies Fusion públicos lado a lado, acessíveis para qualquer outro processo. Durante a criação do contexto de ativação do processo, esses assemblies são recuperados por um processo do sistema denominado CSRSS.exe. Quando isso é feito em um processo convertido, a criação do contexto de ativação e o carregamento do módulo desses assemblies falhará. Os assemblies Fusion lado a lado estão registrados nas seguintes localizações:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de Arquivos: %windir%\\SideBySide

Essa é uma limitação conhecida e, atualmente, não há nenhuma solução alternativa. Dito isso, os assemblies da caixa de entrada, como o ComCtl, são fornecidos com o sistema operacional e, portanto, é seguro ter uma dependência deles.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>Erro encontrado no XML. O atributo 'Executável' é inválido: o valor 'MyApp.EXE' é inválido de acordo com o tipo de dados

Isso poderá acontecer se os executáveis no aplicativo tiverem uma extensão **.EXE** em maiúsculas. Apesar disso, o uso de maiúsculas nessa extensão não deverá afetar a execução do aplicativo, mas poderá fazer com que o DAC gere esse erro.

Para resolver esse problema, tente especificar o sinalizador **-AppExecutable** ao empacotar o aplicativo e use ".exe" em minúsculas na extensão do executável principal (por exemplo: MYAPP.exe).    Como alternativa, converta as maiúsculas usadas em todos os executáveis no aplicativo em minúsculas (por exemplo, de .EXE para .exe).

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Assinaturas Authenticode corrompidas ou malformadas

Esta seção contém detalhes sobre como identificar problemas com arquivos PE no pacote de aplicativo do Windows que possam conter assinaturas Authenticode corrompidas ou malformadas. As assinaturas Authenticode inválidas nos arquivos PE, que podem estar em qualquer formato binário (por exemplo, .exe, .dll, .chm etc.), impedirão que o pacote seja assinado corretamente e, portanto, impedirão que ele seja implantado por meio de um pacote do aplicativo do Windows.

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

**Encontre respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Executar o aplicativo/encontrar e corrigir problemas**

Confira [Executar, depurar e testar um aplicativo da área de trabalho empacotado](desktop-to-uwp-debug.md)

**Distribuir o aplicativo**

Veja [Distribuir um aplicativo de área de trabalho empacotado](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).
