# Microsoft Dynamics 365 Business Central - Training

## Links

- [Dynamics 365 Business Central](https://businesscentral.dynamics.com/5e74901d-334f-46e3-96d1-47d842585abd/Sandbox?company=CRONUS%20BE)

## Notes

- hostname: bcserver
- username: admin
- password: P@ssw0rd

## Setup BC docker container

0. Check requirements
    - a lot of free disk space
    - Docker desktop
    - ⚠️ run powershell commands w/ elevated permissions

1. BcContainerHelper powershell module

``` powershell
Install-Module -Name 'BcContainerHelper' -Force -Verbose
```

2. Docker config
    1. RMC on systray icon > "Switch to Windows containers..."

3. BcContainerHelper wizard (optional)

``` powershell
New-BcContainerWizard
```

3. Create docker container using generated command

``` powershell
$containerName = 'bcserver'
$password = 'P@ssw0rd'
$securePassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$credential = New-Object pscredential 'admin', $securePassword
$auth = 'UserPassword'
$artifactUrl = Get-BcArtifactUrl -type 'Sandbox' -country 'us' -select 'Latest'
New-BcContainer `
    -accept_eula `
    -containerName $containerName `
    -credential $credential `
    -auth $auth `
    -artifactUrl $artifactUrl `
    -updateHosts

```

4. Test
    1. Check if BC is running --> [http://bcserver/BC/?tenant=default](http://bcserver/BC/?tenant=default)
    2. Create and deploy new BC extention/app


## Reference

### Cloud BC

- [Get started with development in Microsoft Dynamics 365 Business Central - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/paths/development-get-started-business-central/)
- [AL Language extension for Microsoft Dynamics 365 Business Central - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-dynamics-smb.al)
- Test network connectivity for Business Central: [aka.ms/BCCP](https://aka.ms/BCCP)

### Docker BC

- [Business Central on Docker for non-experts… | Freddys blog](https://freddysblog.com/2019/08/20/business-central-on-docker-for-non-experts/)
- [Create a Docker for Business Central using BCContainerHelper](https://community.dynamics.com/blogs/post/?postid=34d79023-b847-41bb-b805-f4bc207f3065)

- [PowerShell Gallery | BcContainerHelper 1.0.0](https://www.powershellgallery.com/packages/BcContainerHelper/)
- [microsoft/navcontainerhelper: Official Microsoft repository for BcContainerHelper, a PowerShell module, which makes it easier to work with Business Central Containers on Docker.](https://github.com/microsoft/navcontainerhelper)

## Issues

### Authentication problem w/ Microsoft Cloud Sandbox

After using the AL:Go! command, 
    "12.0 Business Applications 2023 release wave 2"
    "Microsoft Cloud Sandbox"

! not prompted for credentials

! test network connectivity also fails on authentication part

VSCode AL output:

```
[2023-12-13 11:13:12.60] Using reference symbols cache paths: [x:\source\repos\kenny-braeckmans\BA-kennybraeckmans-2324\test\.alpackages]
[2023-12-13 11:13:13.01] Acquiring token for authority https://login.microsoftonline.com/common using correlation f85c6e52-c145-4d86-ae8a-e253f7c05b2b.
```

```
[2023-12-13 11:18:03.83] Using reference symbols cache paths: [x:\source\repos\kenny-braeckmans\BA-kennybraeckmans-2324\test\.alpackages]
[2023-12-13 11:18:04.24] Acquiring token for authority https://login.microsoftonline.com/common using correlation 10ee5e1f-ebf6-486f-88fc-471511208959.
```

