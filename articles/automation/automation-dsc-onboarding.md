---
title: Подключение компьютеров для управления с помощью службы "Настройка состояния службы автоматизации Azure"
description: Настройка компьютеров для управления с помощью службы "Настройка состояния службы автоматизации Azure".
services: automation
ms.service: automation
ms.component: dsc
author: DCtheGeek
ms.author: dacoulte
ms.topic: conceptual
ms.date: 08/08/2018
manager: carmonm
ms.openlocfilehash: 0c34950a9a851fdfd7fdd71d8bd1fe07c02b61aa
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004350"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Подключение компьютеров для управления с помощью службы "Настройка состояния службы автоматизации Azure"

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>В чем преимущества управления компьютерами с помощью службы "Настройка состояния службы автоматизации Azure"?

Служба "Настройка состояния службы автоматизации Azure" так же проста, как и [настройка требуемого состояния с использованием PowerShell](/powershell/dsc/overview). Это мощная служба управления конфигурациями для узлов DSC (физических и виртуальных машин), которую можно использовать в любом облачном или локальном центре обработки данных. Она обеспечивает быструю и простую масштабируемость тысяч компьютеров из безопасного центрального расположения. Вы можете легко переносить компьютеры в облачную среду, присваивать им декларативные конфигурации, а также просматривать отчеты, отражающие соответствие каждого компьютера требуемому состоянию. Уровень управления "Настройка состояния службы автоматизации Azure" используется для настройки требуемого состояния таким же образом, как уровень управления службы автоматизации Azure в сценариях PowerShell. Другими словами, служба автоматизации Azure помогает управлять сценариями PowerShell так же, как и конфигурациями DSC. Дополнительные сведения о преимуществах использования "Настройка состояния службы автоматизации Azure" см. в статье с [обзором этой службы](automation-dsc-overview.md).

Службу "Настройка состояния службы автоматизации Azure" можно использовать для управления разными компьютерами. Их типы перечислены ниже.

- Виртуальные машины Azure (развернутые в классической модели развертывания и модели Azure Resource Manager).
- Экземпляры EC2 Amazon Web Services (AWS).
- физические или виртуальные машины под управлением Windows, расположенные локально или в облачной службе, отличной от Azure или AWS;
- Физические или виртуальные машины под управлением Linux, расположенные локально, в Azure или облачной службе, отличной от Azure.

Кроме того, если вы не готовы управлять конфигурацией компьютера из облака, служба "Настройка состояния службы автоматизации Azure" может также использоваться как конечная точка только для отчетности. Это позволяет задать (отправить) требуемую конфигурацию через DSC локально и просмотреть подробные сведения отчетов о соответствии узла требуемому состоянию в службе автоматизации Azure.

> [!NOTE]
> За управление виртуальными машинами Azure с помощью State Configuration не взимается дополнительная плата, если установлена версия расширения виртуальной машины DSC выше, чем 2.70. Дополнительные сведения см. на странице [**цен на службу автоматизации**](https://azure.microsoft.com/pricing/details/automation/).

В следующих разделах описываются способы подключения каждого типа компьютеров к службе "Настройка состояния службы автоматизации Azure".

## <a name="azure-virtual-machines-classic"></a>Виртуальные машины Azure (классические).

Служба "Настройка состояния службы автоматизации Azure" позволяет легко подключать виртуальные машины Azure (классические) для управления их конфигурациями с помощью портала Azure или PowerShell. В процессе работы расширение DSC регистрирует виртуальную машину в службе "Настройка состояния службы автоматизации Azure", исключая необходимость выполнения администратором удаленного входа на виртуальную машину. Так как с виртуальными машинами Azure расширение DSC работает асинхронно, можно воспользоваться алгоритмом отслеживания хода выполнения и устранения неполадок, который приведен ниже в разделе [**Устранение неполадок при подключении виртуальной машины Azure**](#troubleshooting-azure-virtual-machine-onboarding).

### <a name="azure-portal"></a>Портал Azure

На [портале Azure](http://portal.azure.com/) последовательно выберите **Обзор** -> **Виртуальные машины (классика)**. Выберите виртуальную машину Windows, которую необходимо подключить. В колонке панели мониторинга виртуальной машины щелкните **Все параметры** -> **Расширения** -> **Добавить** -> **Azure Automation DSC** -> **Создать**.
Введите необходимые [значения локального диспетчера конфигураций DSC PowerShell](/powershell/dsc/metaconfig4), регистрационный ключ вашей учетной записи и URL-адрес регистрации. Кроме того, можно ввести конфигурацию узла, которая будет назначена виртуальной машине.

![Расширения виртуальной машины Azure для DSC](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

Сведения о поиске URL-адреса регистрации и ключа учетной записи службы автоматизации для подключения компьютера см. в приведенном ниже разделе [**Безопасная регистрация**](#secure-registration).

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Connect-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ''
$ServiceName = ''
$AutomationAccountName = ''
$AutomationAccountResourceGroup = ''

# fill in the name of a Node Configuration in Azure Automation State Configuration, for this VM to conform to
# NOTE: DSC Node Configuration names are case sensitive in the portal.
$NodeConfigName = ''

# get Azure Automation State Configuration registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation State Configuration
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ''
    ModulesUrl = 'https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip'
    ConfigurationFunction = 'RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2'

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://docs.microsoft.com/powershell/dsc/metaConfig for more details
    Properties = @{
        RegistrationKey = @{
            UserName = 'notused'
            Password = 'PrivateSettingsRef:RegistrationKey'
        }
        RegistrationUrl = $RegistrationInfo.Endpoint
        NodeConfigurationName = $NodeConfigName
        ConfigurationMode = 'ApplyAndMonitor'
        ConfigurationModeFrequencyMins = 15
        RefreshFrequencyMins = 30
        RebootNodeIfNeeded = $False
        ActionAfterReboot = 'ContinueConfiguration'
        AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.76 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

> [!NOTE]
> Имена конфигурации узла State Configuration чувствительны к регистру на портале. Если регистр не соответствует, узел не будет отображаться на вкладке **Узлы**.

## <a name="azure-virtual-machines"></a>Виртуальные машины Azure

Служба "Настройка состояния службы автоматизации Azure" позволяет легко подключать виртуальные машины Azure для управления конфигурацией с помощью портала Azure, шаблонов Azure Resource Manager или PowerShell. В процессе работы расширение DSC регистрирует виртуальную машину в службе "Настройка состояния службы автоматизации Azure", исключая необходимость выполнения администратором удаленного входа на виртуальную машину.
Так как с виртуальными машинами Azure расширение DSC работает асинхронно, можно воспользоваться алгоритмом отслеживания хода выполнения и устранения неполадок, который приведен ниже в разделе [**Устранение неполадок при подключении виртуальной машины Azure**](#troubleshooting-azure-virtual-machine-onboarding).

### <a name="azure-portal"></a>Портал Azure

На [портале Azure](https://portal.azure.com/)перейдите к учетной записи службы автоматизации Azure, чтобы подключить виртуальные машины. На странице State Configuration на вкладке **Узлы** щелкните **+ Добавить**.

Выберите виртуальную машину Azure для подключения.

Если на компьютере не установлено требуемое расширение состояний PowerShell и состояние питания запущено, щелкните **Подключиться**.

В разделе **Регистрация** введите необходимые вам [значения локального диспетчера конфигураций DSC PowerShell](/powershell/dsc/metaconfig4). Кроме того, можно ввести конфигурацию узла, которая будет назначена виртуальной машине.

![](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Шаблоны Azure Resource Manager

Виртуальные машины Azure можно развернуть и подключить к службе "Настройка состояния службы автоматизации Azure" с помощью шаблонов Azure Resource Manager. Пример шаблона, который подключает существующую виртуальную машину к службе "Настройка состояния службы автоматизации Azure", см. в статье [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) (Настройка виртуальной машины с помощью расширения DSC и Azure Automation DSC). Расположение ключа и URL-адреса регистрации, которые шаблон принимает в качестве входных данных, см. в приведенном ниже разделе [**Безопасная регистрация**](#secure-registration).

### <a name="powershell"></a>PowerShell

На портале Azure виртуальные машины можно подключать с помощью командлета [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) в PowerShell.

## <a name="amazon-web-services-aws-virtual-machines"></a>виртуальные машины Amazon Web Services (AWS);

Вы можете легко подключить виртуальные машины Amazon Web Services для управления конфигурацией с помощью "Настройка состояния службы автоматизации Azure", используя набор инструментов DSC AWS. Дополнительные сведения об этом наборе инструментов см. [здесь](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>физические или виртуальные машины под управлением Windows, расположенные локально или в облачной службе, отличной от Azure или AWS;

Компьютеры под управлением Windows, расположенные локально или в облачных службах, отличных от Azure (например, в Amazon Web Services), также можно подключить к службе "Настройка состояния службы автоматизации Azure" при наличии на них исходящего доступа в Интернет. Для этого требуется выполнить несколько простых шагов.

1. Убедитесь, что на компьютерах, которые будут подключены к службе "Настройка состояния службы автоматизации Azure", установлена последняя версия [WMF 5](http://aka.ms/wmf5latest).
1. Создайте папку с необходимыми метаконфигурациями DSC, как указано ниже в разделе [**Создание метаконфигураций DSC**](#generating-dsc-metaconfigurations).
1. Удаленно примените метаконфигурации PowerShell DSC на компьютерах, которые нужно подключить. **Для выполнения этой команды на компьютере должна быть установлена последняя версия [WMF 5](http://aka.ms/wmf5latest)**.

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Если метаконфигурации PowerShell DSC не удалось применить удаленно, скопируйте папку метаконфигураций (см. шаг 2) на каждый компьютер, который нужно подключить. Затем локально вызовите **Set-DscLocalConfigurationManager** на каждом компьютере, который нужно подключить.
1. Используя портал Azure или командлеты, убедитесь, что все компьютеры, которые нужно подключить, теперь отображаются как узлы State Configuration, зарегистрированные в вашей учетной записи службы автоматизации Azure.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Физические или виртуальные машины под управлением Linux, расположенные локально, в Azure или облачной службе, отличной от Azure.

Компьютеры под управлением Linux, расположенные локально, в Azure или в облачной службе, отличной от Azure, также можно подключить к службе "Настройка состояния службы автоматизации Azure" при наличии на них исходящего доступа в Интернет. Для этого требуется выполнить несколько простых шагов.

1. Убедитесь, что на компьютерах, которые будут подключены к службе "Настройка состояния службы автоматизации Azure", установлена последняя версия [службы настройки требуемого состояния PowerShell для Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux).
1. Если [значения по умолчанию локального диспетчера конфигураций DSC PowerShell](/powershell/dsc/metaconfig4) соответствуют требуемым, а подключаемые компьютеры должны извлекать данные из "Настройка состояния службы автоматизации Azure" **и** передают их туда, сделайте следующее:

   - На каждом компьютере под управлением Linux, который будет подключен к службе Azure Automation State Configuratin, используйте файл `Register.py` для подключения с помощью значений по умолчанию локального диспетчера конфигураций DSC PowerShell:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Расположение ключа и URL-адреса регистрации для учетной записи службы автоматизации см. в приведенном ниже разделе [**Безопасная регистрация**](#secure-registration).

     Если значения по умолчанию локального диспетчера конфигураций DSC PowerShell **не** соответствуют требуемым или подключаемые компьютеры должны только подчиняться службе "Настройка состояния службы автоматизации Azure", но не извлекать из нее параметры конфигурации или модули PowerShell, то выполните шаги 3–6. В противном случае сразу перейдите к шагу 6.

1. Создайте папку с необходимыми метаконфигурациями DSC, как указано ниже в разделе [**Создание метаконфигураций DSC**](#generating-dsc-metaconfigurations).
1. Удаленно примените метаконфигурации PowerShell DSC на компьютерах, которые нужно подключить:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Для выполнения этой команды на компьютере должна быть установлена последняя версия [WMF 5](http://aka.ms/wmf5latest) .

1. Если не удалось применить метаконфигурации PowerShell DSC удаленно, скопируйте соответствующую метаконфигурацию из папки (шаг 5) на каждый компьютер под управлением Linux, который нужно подключить. Затем вызовите `SetDscLocalConfigurationManager.py` локально на каждом компьютере под управлением Linux, который нужно подключить к службе "Настройка состояния службы автоматизации Azure":

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

1. С помощью портала Azure или командлетов убедитесь, что все компьютеры, которые нужно подключить, теперь отображаются как узлы DSC, зарегистрированные в вашей учетной записи службы автоматизации Azure.

## <a name="generating-dsc-metaconfigurations"></a>Создание метаконфигураций DSC

Для универсального внедрения любого компьютера в службу "Настройка состояния службы автоматизации Azure" можно создать [метаконфигурацию DSC](/powershell/dsc/metaconfig), при использовании которой агент DSC на соответствующем компьютере будет настроен на извлечение данных из службы "Настройка состояния службы автоматизации Azure" и (или) передачу в эту службу отчетов. Метаконфигурации DSC для службы "Настройка состояния службы автоматизации Azure" можно создавать, используя либо конфигурацию PowerShell DSC, либо командлеты PowerShell в службе автоматизации Azure.

> [!NOTE]
> Метаконфигурации DSC содержат секретные данные, необходимые при подключении компьютера к учетной записи службы автоматизации для управления. Обеспечьте должную защиту создаваемых метаконфигураций или удаляйте их сразу после использования.

### <a name="using-a-dsc-configuration"></a>Использование конфигурации DSC

1. Запустите на компьютере, входящем в локальную среду, VSCode (или другой предпочитаемый редактор) от имени администратора. На этом компьютере должна быть установлена последняя версия [WMF 5](http://aka.ms/wmf5latest) .
1. Локально выполните следующий скрипт. Этот сценарий содержит конфигурацию PowerShell DSC для создания метаконфигураций и команду, запускающую этот процесс.

> [!NOTE]
> Имена конфигурации узла State Configuration чувствительны к регистру на портале. Если регистр не соответствует, узел не будет отображаться на вкладке **Узлы**.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Введите регистрационный ключ и URL-адрес для учетной записи автоматизации, а также имена виртуальных машин, которые необходимо внедрить. Все остальные параметры являются необязательными. Расположение ключа и URL-адреса регистрации для учетной записи службы автоматизации см. в приведенном ниже разделе [**Безопасная регистрация**](#secure-registration).
1. Если вы хотите, чтобы компьютеры передавали сведения о состоянии DSC в службу "Настройка состояния службы автоматизации Azure", не извлекая конфигурацию или модули PowerShell, присвойте для параметра **ReportOnly** значение Тrue.
1. Выполните скрипт. В рабочем каталоге появится папка **DscMetaConfigs**, содержащая метаконфигурации PowerShell DSC для подключаемых компьютеров (в качестве администратора):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Использование командлетов службы автоматизации Azure

Если значения по умолчанию локального диспетчера конфигураций DSC PowerShell соответствуют требуемым и вы хотите внедрить компьютеры таким образом, чтобы позволить им извлекать данные из службы "Настройка состояния службы автоматизации Azure" и передавать в эту службу отчеты, легко создать необходимые конфигурации DSC позволят командлеты службы автоматизации Azure:

1. Запустите на компьютере, входящем в локальную среду, консоль PowerShell или VSCode от имени администратора.
2. Подключитесь к Azure Resource Manager с помощью `Connect-AzureRmAccount`.
3. Из учетной записи службы автоматизации, к которой будут подключены узлы, загрузите метаконфигурации DSC PowerShell для подключаемых компьютеров:

   ```powershell
   # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzureRmAutomationDscOnboardingMetaconfig @Params
   ```

1. В рабочем каталоге появится папка ***DscMetaConfigs***, содержащая метаконфигурации PowerShell DSC для подключаемых компьютеров (в качестве администратора):

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Безопасная регистрация

Компьютеры можно безопасно подключать к учетной записи службы автоматизации Azure с помощью протокола регистрации DSC WMF 5. Этот протокол позволяет узлу DSC проходить проверку подлинности на сервере отчетов или опрашивающем сервере PowerShell DSC (включая "Настройка состояния службы автоматизации Azure"). Узел регистрируется на сервере с использованием **URL-адреса регистрации**, проходя аутентификацию с помощью **Регистрационного ключа**. Во время регистрации узел DSC и сервер отчетов или опрашивающий сервер DSC создают для этого узла уникальный сертификат, который будет использоваться для проверки подлинности после регистрации на сервере. Этот процесс исключает возможность подмены подключенных узлов друг другом, например, если один из узлов взломан и является вредоносным. После регистрации регистрационный ключ повторно не используется для проверки подлинности и удаляется из узла.

Данные, необходимые для протокола регистрации State Configuration, можно найти в подразделе **Ключи** раздела **Параметры учетной записи** на портале Azure. Откройте эту колонку, щелкнув значок ключа на панели **Основные компоненты** в учетной записи службы автоматизации.

![URL-адрес и ключи службы автоматизации Azure](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- «Регистрационный URL-адрес» — это поле «URL-адрес» в колонке «Управление ключами».
- Регистрационный ключ — это поле «Первичный ключ доступа» или «Вторичный ключ доступа» в колонке «Управление ключами». Можно использовать любой из этих ключей.

Для повышения безопасности первичный и вторичный ключи доступа учетной записи службы автоматизации можно создавать повторно в любое время (на странице **Управление ключами**). Это исключает возможность регистрации узла с помощью ранее использованных ключей.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Устранение неполадок при подключении виртуальной машины Azure

Служба "Настройка состояния службы автоматизации Azure" позволяет легко подключать виртуальные машины Microsoft Azure для управления конфигурациями. Расширение DSC для виртуальных машин Azure используется для регистрации виртуальных машин в службе "Настройка состояния службы автоматизации Azure". Так как с виртуальными машинами Azure расширение DSC работает асинхронно, важно отслеживать ход выполнения и устранять неполадки.

> [!NOTE]
> Независимо от способа подключения виртуальной машины Microsoft Azure к службе "Настройка состояния службы автоматизации Azure" с расширением DSC для виртуальных машин Azure, узел будет отображаться как зарегистрированный в службе автоматизации Azure приблизительно через час. Это связано с тем, что расширение DSC для виртуальных машин Azure устанавливает на виртуальную машину платформу Windows Management Framework 5.0, которая требуется для размещения этой виртуальной машины в DSC службы "Настройка состояния службы автоматизации Azure".

Чтобы устранить неполадки или просмотреть состояние расширения DSC для виртуальных машин Azure, на портале Azure перейдите к подключаемой виртуальной машине и выберите **Расширения** в разделе **Параметры**. Затем щелкните **DSC** или **DSCForLinux** в зависимости от операционной системы. Для получения дополнительных сведений щелкните **Просмотреть подробные сведения о состоянии**.

## <a name="certificate-expiration-and-reregistration"></a>Истечение срока действия сертификата и повторная регистрация

Возможно, после регистрации компьютера в качестве узла DSC в службе "Настройка состояния службы автоматизации Azure" этот узел нужно будет зарегистрировать повторно. Такая необходимость может возникнуть по ряду причин.

- После регистрации каждый узел автоматически согласовывает уникальный сертификат для проверки подлинности, срок действия которого составляет один год. В настоящее время протокол регистрации PowerShell DSC не может автоматически обновлять сертификаты при приближении даты окончания срока их действия, поэтому по истечении года необходимо повторно регистрировать узлы. Перед повторной регистрацией убедитесь, что на каждом узле работает Windows Management Framework 5.0 RTM. Если истек срок действия сертификата аутентификации узла и узел не зарегистрирован повторно, он не может взаимодействовать со службой автоматизации Azure и получает пометку "Не отвечает". Повторная регистрация, выполненная не позднее чем через 90 дней с момента истечения срока действия сертификата или в любой момент после истечения срока действия сертификата, приведет к созданию и использованию нового сертификата.
- Чтобы изменить какие-либо [значения локального диспетчера конфигураций PowerShell DSC](/powershell/dsc/metaconfig4) , заданные при первоначальной регистрации узла, например ConfigurationMode. Сейчас эти значения агента DSC можно изменить только в ходе повторной регистрации. Единственным исключением является конфигурация узла, назначенная узлу, — ее можно изменить на платформе Azure Automation DSC напрямую.

Повторная регистрация узла выполняется аналогично первоначальной, то есть с помощью любого из средств, описанных в этом документе. Перед повторной регистрацией отменять регистрацию узла в "Настройка состояния службы автоматизации Azure" не нужно.

## <a name="next-steps"></a>Дополнительная информация

- Чтобы приступить к работе со службой "Настройка состояния службы автоматизации Azure", см. сведения в [этой статье](automation-dsc-getting-started.md).
- Сведения о компилировании конфигураций DSC, которые затем можно назначить целевым узлам, см. в статье [Компилирование конфигураций в Azure Automation DSC](automation-dsc-compile.md).
- Справочник по командлетам PowerShell для службы "Настройка состояния службы автоматизации Azure" см. в [этой статье](/powershell/module/azurerm.automation/#automation).
- Сведения о ценах см. на странице [с ценами на службу "Настройка состояния службы автоматизации Azure"](https://azure.microsoft.com/pricing/details/automation/).
- Пример использования службы "Настройка состояния службы автоматизации Azure" и Chocolatey в конвейере непрерывного развертывания см. в [этой статье](automation-dsc-cd-chocolatey.md).