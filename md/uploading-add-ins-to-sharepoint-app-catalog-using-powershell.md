# Uploading Add-Ins to SharePoint App Catalog using PowerShell #

In this small article, we are going to see how we can upload a _SharePoint Add-In_ to the _App Catalog_ using _PowerShell_. Let's get into it!

## Introduction ##

Of course, we can upload our precious and well-developed _SharePoint Add-Ins_ manually to the _App Catalog_. Unfortunately, this process can sometimes be tedious depending on our workflow. So here, we are going to craft a _PowerShell_ script to ease this process. For our exercise, we are targeting a _SharePoint on-premise_ installation.

## The script ##

Depending on where we want to use our script, we are going to handle two cases: directly on the server and remotely. So, our script is going to use both _SSOM_ and _CSOM_. The choice will be made by passing an argument to the script. We are also going to let the choice to force _Windows Authentication_ because this can be useful in case of an environment using _ADFS_.

    #******************#
    #***** PARAMS *****#
    #******************#

    Param(
        [string] $filePath,
        [string] $appList,
        [string] $siteUrl,
        [string] $username,
        [string] $password,
        [bool] $forceAuth = $true,
        [bool] $forceWindowsAuth = $false
    )

    #****************************************#
    #****************************************#

    #********************#
    #***** INCLUDES *****#
    #********************#

    [System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SharePoint.Client") | Out-Null
    [System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SharePoint.Client.Runtime") | Out-Null

    Add-PSSnapin Microsoft.SharePoint.PowerShell -ErrorAction SilentlyContinue

    #****************************************#
    #****************************************#

    #*************************************#
    #***** MIXED AUTH REQUEST METHOD *****#
    #*************************************#

    function MixedAuthRequestMethod()
    {
        param([Parameter(Mandatory=$true)][object]$clientContext)  
        Add-Type -TypeDefinition @"
        using System;
        using Microsoft.SharePoint.Client;
    
        namespace SPCSOM.SPOHelpers
        {
            public static class ClientContextHelper
            {
                public static void AddRequestHandler(ClientContext context)
                {
                    context.ExecutingWebRequest += new EventHandler<WebRequestEventArgs>(RequestHandler);
                }
    
                private static void RequestHandler(object sender, WebRequestEventArgs e)
                {
                    e.WebRequestExecutor.RequestHeaders.Remove("X-FORMS_BASED_AUTH_ACCEPTED");
                    e.WebRequestExecutor.RequestHeaders.Add("X-FORMS_BASED_AUTH_ACCEPTED", "f");
                }
            }
        }
    "@ -ReferencedAssemblies "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll", "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll";
    [SPCSOM.SPOHelpers.ClientContextHelper]::AddRequestHandler($clientContext);
    }

    #****************************************#
    #****************************************#

    #*************************#
    #***** START PROCESS *****#
    #*************************#

    function StartProcess()
    {
        Write-Host "*********************************"
        Write-Host "***** UPLOAD APP TO CATALOG *****"
        Write-Host "*********************************"
        Write-Host " "

        if ($filePath -eq $null -Or $appList -eq $null -Or $siteUrl -eq $null -Or $username -eq $null -Or $password -eq $null)
        {
            Write-Host "Missing value. Script will end." -ForegroundColor Red
            EndProcess
        }

        Write-Host "***** START *****" -ForegroundColor Green
        Write-Host " "
    }

    #****************************************#
    #****************************************#

    #******************************#
    #***** UPLOAD APPLICATION *****#
    #******************************#

    function UploadApplication()
    {
        $ctx
        $site

        Try
        {
            Write-Host "...application will be uploaded"

            if ($forceAuth)
            {
                $ctx = WithAuth
                $list = GetList $ctx $null
                $upload = GenerateUpload
                UploadFile $ctx $null $list $upload
                Write-Host "Application uploaded!" -ForegroundColor Green
                return
            }

            $site = WithoutAuth
            $list = GetList $null $site
            $upload = GenerateUpload
            UploadFile $null $site $list $upload
            Write-Host "Application uploaded!" -ForegroundColor Green
            return
            
        }
        Catch
        {
            Write-Host $_.Exception.Message -ForegroundColor Yellow
            Write-Host "An error occurred. Script will end." -ForegroundColor Red
            EndProcess $ctx $site
        }
    }

    #****************************************#
    #****************************************#

    #************************#
    #***** WITHOUT AUTH *****#
    #************************#

    function WithAuth()
    {
        Try
        {
            Write-Host "...connecting to server"

            $secpw = ConvertTo-SecureString $password -AsPlainText -Force 
            $ctx = New-Object Microsoft.SharePoint.Client.ClientContext($siteUrl)
            $ctx.AuthenticationMode = [Microsoft.SharePoint.Client.ClientAuthenticationMode]::Default
            $credentials = New-Object System.Net.NetworkCredential($username, $secpw)
            $ctx.Credentials = $credentials.$credentials

            if ($forceWindowsAuth)
            {
                MixedAuthRequestMethod $ctx;
            }

            if (!$ctx.ServerObjectIsNull.Value)
            {
                return $ctx;
            }
            else
            {
                Write-Host "Server object is null. Script will end." -ForegroundColor Red
                EndProcess
            }
        }
        Catch
        {
            Write-Host $_.Exception.Message -ForegroundColor Yellow
            Write-Host "An error occurred. Script will end." -ForegroundColor Red
            EndProcess
        }
    }

    #****************************************#
    #****************************************#

    #************************#
    #***** WITHOUT AUTH *****#
    #************************#

    function WithoutAuth()
    {
        Write-Host "...getting site"
        return Get-SPWeb $siteUrl
    }

    #****************************************#
    #****************************************#

    #********************#
    #***** GET LIST *****#
    #********************#

    function GetList($ctx, $site)
    {
        Write-Host "...getting list"

        if ($forceAuth)
        {
            $list = $ctx.Web.Lists.GetByTitle($appList)
            $ctx.Load($list)
            $ctx.ExecuteQuery()
            return $list
        }

        return $site.Lists[$appList]
    }

    #****************************************#
    #****************************************#

    #***************************#
    #***** GENERATE UPLOAD *****#
    #***************************#

    function GenerateUpload()
    {
        Write-Host "...generating upload"

        $file = Get-ChildItem $filePath;

        if ($forceAuth)
        {
            $fileName = $filePath.Substring($filePath.LastIndexOf("\") + 1)
            $fileStream = New-Object IO.FileStream($file, [System.IO.FileMode]::Open)
            $fileCreationInfo = New-Object Microsoft.SharePoint.Client.FileCreationInformation
            $fileCreationInfo.Overwrite = $true
            $fileCreationInfo.ContentStream = $fileStream
            $fileCreationInfo.URL = $fileName
            return $fileCreationInfo
        }

        return Get-ChildItem $filePath;
    }

    #****************************************#
    #****************************************#

    #***********************#
    #***** UPLOAD FILE *****#
    #***********************#

    function UploadFile($ctx, $site, $list, $upload)
    {
        Write-Host "...uploading file"

        $fileName = $filePath.Substring($filePath.LastIndexOf("\") + 1)

        if ($forceAuth)
        {
            $file = $list.RootFolder.Files.Add($upload)
            $ctx.ExecuteQuery()
            $ctx.Dispose()
            return
        }

        $file = $list.RootFolder.Files.Add($fileName, $upload.OpenRead(), $true)
        $site.Dispose()
        return
    }

    #****************************************#
    #****************************************#

    #***********************#
    #***** END PROCESS *****#
    #***********************#

    function EndProcess($ctx, $site)
    {
        if ($ctx)
        {
            $ctx.Dispose()
        }

        if ($site)
        {
            $site.Dipose()
        }

        Write-Host " "
        Write-Host "***** END *****" -ForegroundColor Green
        Write-Host " "
        exit
    }

    #****************************************#
    #****************************************#

    #****************#
    #***** MAIN *****#
    #****************#

    function Main()
    {
        StartProcess
        UploadApplication
        EndProcess
    }

    #****************************************#
    #****************************************#

    #******************#
    #***** SCRIPT *****#
    #******************#

    Main
_<small>PowerShell script</small>_

As we can see, there is no real magic trick in this script. Used correctly, it will let us upload our "_.app_" file directly from our server or remotely. So, we can imagine using it in an automated build/release process.

## Conclusion ##

Through this really short article, we saw how we can create a _PowerShell_ script to automate the _SharePoint Add-Ins_ uploading process.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!