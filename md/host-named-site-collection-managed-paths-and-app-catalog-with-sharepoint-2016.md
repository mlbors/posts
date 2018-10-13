# Host-Named Site Collection, Managed Paths and App Catalog with SharePoint 2016 #

In this article, we are going to set up a _Host-Name Site Collection_, _Managed Paths_ and create a private _App Catalog_ using _SharePoint 2016 on-premise_.

## Introduction ##

_Host-Named Site Collections_ enable us to assign a unique _DNS_ name to _Site Collections_. So, it means that we can deploy many sites with unique _DNS_ names in the same _Web Application_ and it allows us to scale our environment to many customers. In other words, we can have something like http://sitea.domain.com and http://siteb.domain.com.

For the sake of our example, let's imagine that we need to set a development environment with multiple _Collections_, each for a different purpose, and a private _App Catalog_ for our _Add-Ins_.

Here, we are not going to see how to configure the domain names in _DNS_ and everything that is related to this part. So, of course, we have to check with the system administrator what could be done.

## Creating the HNSC ##

Because _Host-Named Site Collections_ can only be created with _PowerShell_ and not from the _Central Administration_, we are going to build a _PowerShell_ script that will create what we need: an _Application Pool_, a _Web Application_, a _Root Site Collection_ and our _Host-Named Site Collection_.

First, we need the _Application Pool_:

    New-SPServiceApplicationPool -Name $applicationPool -Account $managedAccount

We can now set our _Web Application_ and create the required binding for _IIS_:

    New-SPWebApplication -Name $webAppName -hostHeader -$webAppHostHeader -port $port -Url $webAppUrl -ApplicationPool $applicationPool -ApplicationPoolAccount (Get-SPManagedAccount $managedAccount) -AuthenticationProvider (New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication) -DatabaseName $dataBase
    New-WebBinding -Name $webAppName -IPAddress "*" -Port $port -Protocol http 

Then, we can create the _Root Site Collection_:

    New-SPSite -Url $rootCollectionUrl -HostHeaderWebApplication (Get-SPWebApplication $webAppName) -Name $rootCollectionName -Description $rootCollectionDescription -OwnerAlias $ownerAlias

Now, let's set up our _Host-Named Site Collection_:

    New-SPSite -Url $collectionUrl -HostHeaderWebApplication (Get-SPWebApplication $webAppName) -Name $collectionName -Description $collectionDescription -OwnerAlias $ownerAlias -language $language -Template $template

Finally, we can create a _Collection_ using a _Managed Path_:

    New-SPManagedPath -RelativeURL $managedPath -HostHeader -Explicit
    $url = $collectionUrl + "/" + $managedPath
    New-SPSite -Url $url -HostHeaderWebApplication $collectionUrl -Name $managedPathCollectionName -Description $managedPathCollectionDescription -OwnerAlias $ownerAlias -language $language -Template $template

Our _PowerShell_ script could look something like this:

    #******************#
    #***** PARAMS *****#
    #******************#

    Param(
        [string] $webAppName,
        [string] $webAppUrl,
        [string] $webAppHostHeader,
        [string] $applicationPool,
        [int] $port = 80,
        [string] $managedAccount,
        [string] $dataBase,
        [string] $rootCollectionUrl,
        [string] $rootCollectionName,
        [string] $rootCollectionDescription,
        [string] $collectionUrl,
        [string] $collectionName,
        [string] $collectionDescription,
        [string] $ownerAlias,
        [int] $language = 1033,
        [string] $template = "STS#0",
        [bool] $createAppPool = $true,
        [bool] $createWebApp = $true,
        [bool] $createRootCollection = $true,
        [bool] $createHostNameCollection = $true,
        [bool] $createManagedPathCollection = $true,
        [string] $managedPath,
        [string] $managedPathCollectionName,
        [string] $managedPathCollectionDescription
    )

    #****************************************#
    #****************************************#

    #********************#
    #***** INCLUDES *****#
    #********************#

    Add-PSSnapin Microsoft.SharePoint.PowerShell -ErrorAction SilentlyContinue

    #****************************************#
    #****************************************#

    #***********************************#
    #***** CREATE APPLICATION POOL *****#
    #***********************************#

    function CreateApplicationPool()
    {
        Write-Host "...creating application pool"
        New-SPServiceApplicationPool -Name $applicationPool -Account $managedAccount
        Write-Host "Application Pool created." -ForegroundColor Green
        Write-Host ""
    }

    #****************************************#
    #****************************************#

    #**********************************#
    #***** CREATE WEB APPLICATION *****#
    #**********************************#

    function CreateWebApplication()
    {
        Write-Host "...creating web application"

        New-SPWebApplication -Name $webAppName -hostHeader -$webAppHostHeader -port $port -Url $webAppUrl -ApplicationPool $applicationPool -ApplicationPoolAccount (Get-SPManagedAccount $managedAccount) -AuthenticationProvider (New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication) -DatabaseName $dataBase
        New-WebBinding -Name $webAppName -IPAddress "*" -Port $port -Protocol http      
        Write-Host "Web Application created." -ForegroundColor Green
        Write-Host ""
    }

    #****************************************#
    #****************************************#

    #**********************************#
    #***** CREATE ROOT COLLECTION *****#
    #**********************************#

    function CreateRootCollection()
    {
        Write-Host "...creating root collection"

        New-SPSite -Url $rootCollectionUrl -HostHeaderWebApplication (Get-SPWebApplication $webAppName) -Name $rootCollectionName -Description $rootCollectionDescription -OwnerAlias $ownerAlias
        Write-Host "Root Collection created." -ForegroundColor Green
        Write-Host ""    
    }

    #****************************************#
    #****************************************#

    #****************************************#
    #***** CREATE HOST-NAMED COLLECTION *****#
    #****************************************#

    function CreateHostNamedCollection()
    {
        Write-Host "...creating host-named collection"

        New-SPSite -Url $collectionUrl -HostHeaderWebApplication (Get-SPWebApplication $webAppName) -Name $collectionName -Description $collectionDescription -OwnerAlias $ownerAlias -language $language -Template $template
        Write-Host "Host-Named Collection created." -ForegroundColor Green
        Write-Host ""
    }

    #****************************************#
    #****************************************#

    #******************************************#
    #***** CREATE MANAGED PATH COLLECTION *****#
    #******************************************#

    function CreateManagedPathCollection() 
    {
        Write-Host "...creating managed path collection"

        New-SPManagedPath -RelativeURL $managedPath -HostHeader -Explicit
        Write-Host "Managed Path added." -ForegroundColor Green
        Write-Host ""

        $url = $collectionUrl + "/" + $managedPath
        New-SPSite -Url $url -HostHeaderWebApplication $collectionUrl -Name $managedPathCollectionName -Description $managedPathCollectionDescription -OwnerAlias $ownerAlias -language $language -Template $template
        Write-Host "Managed Path Collection created." -ForegroundColor Green
        Write-Host ""
    }

    #****************************************#
    #****************************************#

    #****************#
    #***** MAIN *****#
    #****************#

    function Main()
    {
        Write-Host "****************************************"
        Write-Host "***** CREATE HOST NAMED COLLECTION *****"
        Write-Host "****************************************"
        Write-Host " "

        Write-Host "***** START *****" -ForegroundColor Green
        Write-Host " "

        if ($createAppPool) {
            CreateApplicationPool
        }

        if ($createWebApp) {
            CreateWebApplication
        }

        if ($createRootCollection) {
            CreateRootCollection
        }

        if ($createHostNameCollection) {
            CreateHostNamedCollection
        }

        if ($createManagedPathCollection) {
            CreateManagedPathCollection
        }

        Write-Host " "
        Write-Host "***** END *****" -ForegroundColor Green
        Write-Host " "

        Read-Host -Prompt "Press ENTER to continue"
        exit
    }

    #****************************************#
    #****************************************#

    #******************#
    #***** SCRIPT *****#
    #******************#

    Main

We can now use our script like so:

    .\create-host-named-collection.ps1 -webAppName "sp16dev1.com" -webAppUrl "http://sp16dev1.com" -webAppHostHeader "sp16dev1.com" -applicationPool "SharePoint - sp16dev1.com" -port 80 -managedAccount Domain\serviceAccount -dataBase WSS_Content_DevSP -rootCollectionUrl "http://rootsp16dev.com" -rootCollectionName "rootsp16dev" -rootCollectionDescription "Root collection" -collectionUrl "http://sp16dev.com" -collectionName "sp16dev" -collectionDescription "Main collection for development" -ownerAlias serviceAccount@domain.com -language 1033 -managedPath "addins-dev" -managedPathCollectionName "sp16dev-addins" -managedPathCollectionDescription "Collection for add-ins"

We could probably clean up a bit the number of arguments and add security checks to our script, but for now, it will be fine. If everything is alright, we should see our _Wep Application_ and our _Collections_ in the _Central Administration_.

## Creating the App Catalog ##

We are now going to set up our _App Catalog_. First, we need to go in the _Central Administration_, then "_System Settings_", "_Manage services in this farm_". We have to click on "_Enable Auto Provision_" for "_Microsoft SharePoint Foundation Subscription Settings Service_".

Next, we have to create the "_Subscription Settings_" service application and proxy:

    $SubscriptionSvcApp = New-SPSubscriptionSettingsServiceApplication -ApplicationPool 'SharePoint Web Services Default' -Name 'Subscriptions Settings Service Application' -DatabaseName 'Subscription'
    $SubscriptionSvcProxy = New-SPSubscriptionSettingsServiceApplicationProxy -ServiceApplication $SubscriptionSvcApp

We may also need to create a "_App Management Service Application_". It can be done under "_Manage Service Applications_". We have to click on "_New_" then "_App Management Service_". We can choose "_SharePoint Web Services Default_" for the "_Application Pool_".

We now have to head to the "_Apps_" page, click on the "_Configure App URLs_" link. In the "_App domain_" field, we have to enter the domain that we chose to host our apps and in the _"App prefix_" field, we need to specify which prefix we want to use. So, at the end of the day, we should have a _URL_ for an app would be something like so "_app-12345678ABCDEF.apps.sp16dev.com_".

Using _PowerShell_, we now have to configure our app _URLs_ for our tenant:

    Set-SPAppDomain apps.sp16dev.com
    Set-SPAppSiteSubscriptionName -Name "app" -Confirm:$false

Now, let's head back to the _Central Administration_, then "_Application Management_", "_Manage Web applications_" and select our web application. On the ribbon, let's click on "_Manage Features_" and activate the "_Apps that require accessible internet facing endpoints_" feature.

We can now also enable sideloading on our dev site if needed:

    Enable-SPFeature -identity "EnableAppSideLoading" -URL http://sp16dev.com/addins-dev

Finally, we can create our app catalog:

    New-SPSite -Url "http://sp16dev.com/apps" -HostHeaderWebApplication "http://sp16dev.com" -Name "apps" -Description "App Catalog" -OwnerAlias serviceAccount@domain.com -language 1033 -Template "APPCATALOG#0"

If everything is alright, we can navigate to "http://sp16dev.com/apps" where we can upload our well-crafted _Add-Ins_ and make them accessible in our different sites through the _App Catalog_.

## Side note ##

We may have trouble with our catalog. Maybe this last one will tell us that there is nothing from our organization even though there are deployed apps. [This post from _Microsoft_](https://blogs.technet.microsoft.com/sharepointdevelopersupport/2018/02/19/sharepoint-2016-apps-from-your-organization-there-is-nothing-here-yet/) could be the answer to this problem.

## Conclusion ##

Through this post, we saw what _Host-Named Site Collections_ are and how we can set up our environment by using them. We also took a look at how can create a private _App Catalog_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!