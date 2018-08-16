
## <a name="start-your-powershell-session"></a>Запуск сеанса PowerShell
Сначала установите и запустите последнюю версию [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) . Дополнительные сведения можно узнать в статье [Установка и настройка Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> В примерах ниже используется [модель развертывания Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), поэтому применяются [командлеты Azure Resource Manager](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Выполните командлет [**Connect-AzureRmAccount**](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount). Откроется окно входа, в котором нужно ввести свои учетные данные. Используйте для входа те же учетные данные, что и для входа на портал Azure.

    Connect-AzureRmAccount

Если у вас несколько подписок, то используйте командлет [**Set-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext), чтобы выбрать подписку, которую будет использовать сеанс PowerShell. Чтобы узнать, какую подписку использует текущий сеанс PowerShell, выполните командлет [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext). Чтобы просмотреть все подписки, выполните командлет [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

