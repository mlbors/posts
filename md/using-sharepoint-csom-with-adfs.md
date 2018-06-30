# Using SharePoint CSOM with ADFS #

Through this article, we are going to see how we can use _SharePoint CSOM_ when _ADFS_ is used for authentication. Then, we are also going to make a little side note about _WSS_.

## Introduction ##

Since _SharePoint 2010_, _SharePoint_ provides us a way to interact with _SharePoint_ sites called _Client Object Model_, or _CSOM_. So we are able to write scripts or add-ins without the need to program directly on the server where _SharePoint_ is installed.

Using _CSOM_ is pretty straightforward, however, when _ADFS_ is used, we can have some struggles. Just to refresh our mind, _ADFS_, or _Active Directory Federation Services_, that runs on _Windows Server_, allows single _sign-on access_ (_SSO_) to systems and applications located across organizational boundaries.

Let's see how we can achieve a simple authentication with such a setup. We will then have a little side note about old _SharePoint_ environments.

## Using CSOM ##

The trick is not so complicated. The secret is in the "_AuthenticationMode_" property and in the "_ExecutingWebRequest_" event of the "_ClientContext_" class. Let's make it with _C#_:

    private void _ConnectSPCSOM(string url, string user, SecureString password)
    {
        using (ClientContext context = new ClientContext(url))
        {
            context.AuthenticationMode = ClientAuthenticationMode.Default;
            context.Credentials = new System.Net.NetworkCredential(user, password);
            context.ExecutingWebRequest += new EventHandler<WebRequestEventArgs>(_MixedAuthRequestMethod);

            try
            {
                Web web = context.Web;
                Console.WriteLine("Loading web...");
                context.Load(web);
                context.ExecuteQuery();

                Console.WriteLine(web.Title);
                Console.WriteLine(web.Url);
            }
            catch (Exception exception)
            {
                Console.WriteLine("Exception of type" + exception.GetType() + "caught.");
                Console.WriteLine(exception.Message);
                Console.WriteLine(exception.StackTrace);
            }
        }
    }

    private void _MixedAuthRequestMethod(object sender, WebRequestEventArgs e)
    {
        try
        {
            e.WebRequestExecutor.RequestHeaders.Remove("X-FORMS_BASED_AUTH_ACCEPTED");
            e.WebRequestExecutor.RequestHeaders.Add("X-FORMS_BASED_AUTH_ACCEPTED", "f");
        }
        catch (Exception exception)
        {
            Console.WriteLine("Exception of type" + exception.GetType() + "caught.");
            Console.WriteLine(exception.Message);
            Console.WriteLine(exception.StackTrace);
        }
    }
<small>_C# code_</small>

As we can see, we use _CSOM_ as usual. However, we first set the authentication mode to "_default_". It is important that this line is placed before the next two lines. We then pass our credentials, using a "_SecureString_" for the password. Then we create a new "_EventHandler_" that uses the "_MixedAuthRequestMethod_" method. This method will add the right information in the request header to force _Windows Authentication_.

If we have to do it with _PowerShell_, we can imagine doing it like so:

    [System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SharePoint.Client") | Out-Null
    [System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SharePoint.Client.Runtime") | Out-Null

    Add-PSSnapin Microsoft.SharePoint.PowerShell

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

    $url = "SHAREPOINT-URL"
    $user = "username"
    $password = "plain-text-password"
    $secpw = ConvertTo-SecureString $password -AsPlainText -Force

    $context = New-Object Microsoft.SharePoint.Client.ClientContext($url)

    $context.AuthenticationMode = [Microsoft.SharePoint.Client.ClientAuthenticationMode]::Default
    $credentials = New-Object System.Net.NetworkCredential($user , $secpw)
    $context.Credentials = $credentials.$credentials

    MixedAuthRequestMethod $context;

    if (!$context.ServerObjectIsNull.Value) {

        Try 
        {
            $web = $context.Web
            $context.Load($web) 
            $context.ExecuteQuery()

            Write-Host $web.Title
            Write-Host $web.Url
        } 
        Catch 
        {
            Write-Host $_.Exception.Message
            Write-Host $_.Exception
            Write-Host "Can't connect to" $url -ForegroundColor Red
        }

    } else {
        Write-Host "Server object is null"
    }
<small>_PowerShell code_</small>

Now, with this little trick, we should be able to use _SharePoint CSOM_ with _ADFS_. However, we may still get a _401 error_. In such a case, we probably have to check the security of our environment. For example, if there is a _Reverse Proxy_ between us and the _SharePoint Front Server_, we can make a workaround by writing the _Front Server IP address_ in our _host_ file.

## Side note ##

We may find ourselves in such a situation where we want to achieve what we could achieve with _CSOM_ in an old _SharePoint_ environment, for example _WSS 3.0_. Unfortunately, here, _CSOM_ is not available. Nevertheless, there is a solution: _SharePoint Web Services_. There are many different _Web Services_ that are available at _URL_ like "https://servername/_vti_bin/Lists.asmx". To use a _Web Service_, in _Visual Studio_, we have to right-click on "_References_", then we have to choose "_Add Service Reference_", then "_Advanced_". We then have to enter the _URL_, validate it and click on "_Add Web Reference_". We can then use our _Web Service_ like so:

    private void _GetWSSCollectionsInfo(string url, string user, SecureString password)
    {
        TheWebService.Webs webs = new TheWebService.Webs
        {
            PreAuthenticate = true,
            Credentials = new System.Net.NetworkCredential(user, password),
            Url = url + "/_vti_bin/Webs.asmx"
        };

        try
        {
            XmlNode collection = webs.GetWebCollection();
            XmlNodeList nodes = collection.SelectNodes("*");

            if (collection == null || collection.ChildNodes[0] == null)
            {
                return;
            }

            foreach (XmlNode node in nodes)
            {
                Console.WriteLine("Title: " + node.Attributes["Title"].Value);
                Console.WriteLine("Url: " + node.Attributes["Url"].Value);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine("Exception of type" + exception.GetType() + "caught.");
            Console.WriteLine(exception.Message);
            Console.WriteLine(exception.StackTrace);
        }
    }
<small>_C# code_</small>

## Conclusion ##

Through this article, we saw how we can use _SharePoint CSOM_ with _ADFS_. The trick is to force _Windows Authentication_. We saw how we can do it with _C#_ and _PowerShell_. We also had a little side note about _SharePoint Web Services_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!