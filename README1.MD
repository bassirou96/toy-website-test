New-AzResourceGroup -Name ToyWebsiteTest -Location westus




Créer un groupe de ressources dans Azure
Exécutez cette commande Azure PowerShell dans le terminal Visual Studio Code pour créer un groupe de ressources :

Azure PowerShell

Copier
New-AzResourceGroup -Name ToyWebsiteTest -Location westus
Vérifiez la sortie à partir de la commande. Il comprend un ResourceId, qui est l’ID complet du groupe de ressources.

Copiez l’ID du groupe de ressources dans un endroit sûr. Vous en aurez bientôt besoin.

Créer un principal de service et lui accorder l’accès au groupe de ressources
Pour créer un principal du service et lui attribuer le rôle Collaborateur pour votre groupe de ressources, exécutez le code Azure PowerShell suivant dans le terminal Visual Studio Code. Remplacez l’espace réservé RESOURCE_GROUP_ID par l’ID du groupe de ressources que vous avez copié à l’étape précédente.

Azure PowerShell

Copier
$resourceGroupId = '/subscriptions/0e6f60f8-51e5-4cc3-af11-2ab4c39c725c/resourceGroups/ToyWebsiteTest'

$azureContext = Get-AzContext
$servicePrincipal = New-AzADServicePrincipal `
-DisplayName ToyWebsiteTest `
-Role Contributor `
-Scope $resourceGroupId

$output = @{
   clientId = $($servicePrincipal.ApplicationId)
   clientSecret = $([System.Net.NetworkCredential]::new('', $servicePrincipal.Secret).Password)
   subscriptionId = $($azureContext.Subscription.Id)
   tenantId = $($azureContext.Tenant.Id)
}
$output | ConvertTo-Json
Sélectionnez la sortie JSON de la commande précédente. Voici à quoi cela ressemble :

JSON

Copier
{
  "clientId": "c6bf233f-d1b8-480a-9cf7-27e2186345d2",
  "clientSecret": "<secret value>",
  "subscriptionId": "f0750bbe-ea75-4ae5-b24d-a92ca601da2c",
  "tenantId": "dbd3173d-a96b-4c2f-b8e9-babeefa21304"
}