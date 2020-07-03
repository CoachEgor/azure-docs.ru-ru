---
title: Защита кластера в Windows с помощью сертификатов
description: Безопасный обмен данными в автономном или частном кластере Azure Service Fabric, а также между клиентами и кластером.
author: dkkapur
ms.topic: conceptual
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: cf7d418d8bca8f690acf29ba701fdc54ced1ca6c
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82562004"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>Защита автономного кластера под управлением Windows с помощью сертификатов X.509
В этой статье описано, как обезопасить обмен данными между разными узлами автономного кластера Windows. Кроме того, здесь рассматриваются способы аутентификации для клиентов, которые подключаются к кластеру при помощи сертификатов X.509. Аутентификация гарантирует, что только авторизованные пользователи могут получить доступ к кластеру и развернутым приложениям для выполнения задач управления. Безопасность на основе сертификатов необходимо включить в кластере при его создании.  

Дополнительные сведения о безопасности кластера, такие как безопасность узла на узел, безопасность клиента на узел и управление доступом на основе ролей, см. в статье [сценарии безопасности кластера](service-fabric-cluster-security.md).

## <a name="which-certificates-do-you-need"></a>Необходимые сертификаты
Сначала нужно [скачать пакет Service Fabric для Windows Server](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) на один из узлов в кластере. В скачанном пакете содержится файл ClusterConfig.X509.MultiMachine.json. Откройте его и ознакомьтесь с разделом security в разделе properties.

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClusterCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames": "Root"
            }
        ]
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

В этом разделе описаны сертификаты, необходимые для обеспечения безопасности автономного кластера Windows. Указывая сертификат кластера, задайте для параметра ClusterCredentialType значение _X509_. Если вы указываете сертификат сервера для внешних подключений, задайте для ServerCredentialType значение _X509_. Хотя это не обязательно, рекомендуем получить оба этих сертификата, чтобы защитить кластер должным образом. Если для этих параметров задано значение *X509*, вам также нужно указать соответствующие сертификаты. Иначе Service Fabric вызывает исключение. В некоторых сценариях может потребоваться указать только один из параметров — _ClientCertificateThumbprints_ или _ReverseProxyCertificate_. В этих случаях не нужно задавать для _ClusterCredentialType_ или _ServerCredentialType_ значение _X509_.


> [!NOTE]
> [Отпечаток](https://en.wikipedia.org/wiki/Public_key_fingerprint) — это основной идентификатор сертификата. Чтобы узнать, как извлечь отпечатки создаваемых сертификатов, см. статью [Практическое руководство. Извлечение отпечатка сертификата](https://msdn.microsoft.com/library/ms734695.aspx).
> 
> 

В следующей таблице перечислены сертификаты, необходимые для настройки кластера:

| **Параметр Цертификатеинформатион** | **Описание** |
| --- | --- |
| ClusterCertificate |Рекомендуется использовать в тестовой среде. Этот сертификат нужен, чтобы защитить связь между узлами кластера. Для обновления можно использовать два разных сертификата — основной и дополнительный. Укажите отпечаток основного сертификата в разделе Thumbprint, а отпечаток дополнительного сертификата — в переменных ThumbprintSecondary. |
| ClusterCertificateCommonNames |Рекомендуется использовать в рабочей среде. Этот сертификат нужен, чтобы защитить связь между узлами кластера. Можно использовать одно или два общих имени сертификата для кластера. CertificateIssuerThumbprint соответствует отпечатку издателя сертификата. Если используется несколько сертификатов с одним общим именем, вы можете указать несколько отпечатков издателя.|
| ClusterCertificateIssuerStores |Рекомендуется использовать в рабочей среде. Этот сертификат соответствует издателю сертификата кластера. Вместо указания отпечатков издателя в разделе ClusterCertificateCommonNames можно указать общее имя издателя и соответствующее имя хранилища в этом разделе.  Это позволит легко выделить сертификаты издателя кластера. Если используется несколько издателей, можно указать несколько сертификатов кластера. Если раздел IssuerCommonName пустой, все сертификаты в соответствующих хранилищах, указанных в разделе X509StoreNames, отправляются в список разрешений.|
| ServerCertificate |Рекомендуется использовать в тестовой среде. Этот сертификат предоставляется клиенту при попытке подключиться к этому кластеру. Для удобства можно использовать один сертификат для параметров ClusterCertificate и ServerCertificate. Для обновления можно использовать два разных сертификата сервера — основной и дополнительный. Укажите отпечаток основного сертификата в разделе Thumbprint, а отпечаток дополнительного сертификата — в переменных ThumbprintSecondary. |
| ServerCertificateCommonNames |Рекомендуется использовать в рабочей среде. Этот сертификат предоставляется клиенту при попытке подключиться к этому кластеру. CertificateIssuerThumbprint соответствует отпечатку издателя сертификата. Если используется несколько сертификатов с одним общим именем, вы можете указать несколько отпечатков издателя. Для удобства можно использовать один сертификат для параметров ClusterCertificateCommonNames и ServerCertificateCommonNames. Можно использовать одно или два общих имени сертификата для сервера. |
| ServerCertificateIssuerStores |Рекомендуется использовать в рабочей среде. Этот сертификат соответствует издателю сертификата сервера. Вместо указания отпечатков издателя в разделе ServerCertificateCommonNames можно указать общее имя издателя и соответствующее имя хранилища в этом разделе.  Это позволит легко выделить сертификаты издателя сервера. Если используется несколько издателей, можно указать несколько сертификатов сервера. Если раздел IssuerCommonName пустой, все сертификаты в соответствующих хранилищах, указанных в разделе X509StoreNames, отправляются в список разрешений.|
| ClientCertificateThumbprints |Установите этот набор сертификатов на клиентских компьютерах, прошедших аутентификацию. На компьютерах, которым нужно предоставить доступ к кластеру, можно установить несколько различных клиентских сертификатов. Укажите отпечаток каждого сертификата в переменной CertificateThumbprint. Если для параметра IsAdmin задано значение *true*, с клиентского компьютера с установленным сертификатом можно выполнять различные действия управления кластером от имени администратора. Если для параметра IsAdmin задано значение *false*, с клиентского компьютера с установленным сертификатом можно выполнять действия, зависящие от прав доступа пользователей, обычно только операции чтения. Дополнительные сведения о ролях см. в руководстве по [управлению доступом на основе ролей (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| ClientCertificateCommonNames |Укажите общее имя первого сертификата клиента для параметра CertificateCommonName. CertificateIssuerThumbprint — это отпечаток издателя сертификата. Дополнительные сведения об общих именах и издателе см. в статье [Работа с сертификатами](https://msdn.microsoft.com/library/ms731899.aspx). |
| ClientCertificateIssuerStores |Рекомендуется использовать в рабочей среде. Этот сертификат соответствует издателю сертификата клиента (роли администратора и стандартного пользователя). Вместо указания отпечатков издателя в разделе ClientCertificateCommonNames можно указать общее имя издателя и соответствующее имя хранилища в этом разделе.  Это позволит легко выделить сертификаты издателя клиента. Если используется несколько издателей, можно указать несколько сертификатов клиента. Если раздел IssuerCommonName пустой, все сертификаты в соответствующих хранилищах, указанных в разделе X509StoreNames, отправляются в список разрешений.|
| ReverseProxyCertificate |Рекомендуется использовать в тестовой среде. Это необязательный сертификат, который можно указать, если вы хотите защитить [обратный прокси-сервер](service-fabric-reverseproxy.md). Если вы используете этот сертификат, обязательно укажите для параметра nodeTypes значение reverseProxyEndpointPort. |
| ReverseProxyCertificateCommonNames |Рекомендуется использовать в рабочей среде. Это необязательный сертификат, который можно указать, если вы хотите защитить [обратный прокси-сервер](service-fabric-reverseproxy.md). Если вы используете этот сертификат, обязательно укажите для параметра nodeTypes значение reverseProxyEndpointPort. |

Ниже приведен пример конфигурации кластера, в котором указаны сертификаты клиента, сервера и кластера. Для сертификатов кластера, сервера и обратного прокси-сервера нельзя одновременно настроить отпечаток и общее имя для одного типа сертификата.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "10-2017",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        },
        "security": {
            "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClusterCertificateIssuerStores": [
                    {
                        "IssuerCommonName": "ClusterIssuer1",
                        "X509StoreNames" : "Root"
                    },
                    {
                        "IssuerCommonName": "ClusterIssuer2",
                        "X509StoreNames" : "Root"
                    }
                ],
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName",
                      "CertificateIssuerThumbprint": "7c fc 91 97 13 16 8d ff a8 ee 71 2b a2 f4 62 62 00 03 49 0d"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-rollover"></a>Смена сертификатов
Если вы используете общее имя сертификата вместо отпечатка, чтобы сменить сертификат, не нужно обновлять конфигурацию кластера. Для обновления отпечатков издателя убедитесь, что новый список отпечатков сопоставим со старым. Сначала нужно обновить конфигурацию, указав новые отпечатки издателя, а затем установить новые сертификаты (сертификат сервера или кластера и сертификаты издателя) в хранилище. Оставьте старый сертификат издателя в хранилище сертификатов как минимум на два часа после установки нового.
При использовании хранилищ издателя сертификата обновлять конфигурацию для выделения сертификата издателя не требуется. Установите новый сертификат издателя с более поздней датой истечения срока действия в соответствующем хранилище сертификатов и удалите старый сертификат издателя через несколько часов.

## <a name="acquire-the-x509-certificates"></a>Получение сертификатов X.509
Чтобы защитить связь в кластере, сначала необходимо получить сертификаты X.509 для узлов кластера. Кроме того, чтобы разрешить подключение к этому кластеру только для авторизованных компьютеров и пользователей, необходимо получить и установить сертификаты для клиентских компьютеров.

Для защиты кластеров, на которых выполняются рабочие нагрузки в рабочей среде, необходимо использовать сертификат X.509, подписанный [центром сертификации](https://en.wikipedia.org/wiki/Certificate_authority). Дополнительные сведения см. в руководстве по [получению сертификата](https://msdn.microsoft.com/library/aa702761.aspx).

Для кластеров, которые применяются для тестирования, можно использовать самозаверяющие сертификаты.

## <a name="optional-create-a-self-signed-certificate"></a>Создание самозаверяющего сертификата (необязательно)
Защищенный самозаверяющий сертификат создается при помощи сценария CertSetup.ps1, который находится в папке пакета SDK для Service Fabric в каталоге C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure. Измените в этом файле имя сертификата по умолчанию. (Найдите значение CN = ServiceFabricDevClusterCert.) Запустите этот скрипт от `.\CertSetup.ps1 -Install`имени.

Теперь экспортируйте сертификат в PFX-файл, защищенный паролем. Сначала получите отпечаток сертификата. 
1. В меню **Запуск** выберите **Управление сертификатами компьютеров**. 

2. Перейдите в папку **Local Computer\Personal** и найдите созданный сертификат. 

3. Дважды щелкните сертификат, чтобы открыть его, выберите вкладку **сведения** и прокрутите вниз до поля **отпечаток** . 

4. Удалите пробелы и вставьте значение отпечатка в приведенную ниже команду PowerShell. 

5. Укажите для параметра `String` безопасный пароль и выполните следующую команду PowerShell:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Чтобы просмотреть подробные сведения об установленном на компьютере сертификате, выполните следующую команду PowerShell:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

Кроме того, если у вас есть подписка Azure, следуйте инструкциям в статье [Создание кластера Service Fabric в Azure с помощью Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="install-the-certificates"></a>Установка сертификатов
Созданные сертификаты можно установить в узлах кластера. На узлах должна быть установлена последняя версия Windows PowerShell 3.x. Повторите эти шаги в каждом узле для сертификатов кластера и сервера, а также для дополнительных сертификатов.

1. Скопируйте PFX-файлы в узел.

2. Откройте окно PowerShell с правами администратора и выполните следующие команды. Замените значение *$pswd* паролем, который использовался при создании этого сертификата. Укажите в качестве значения *$PfxFilePath* полный путь к PFX-файлу, скопированному в этот узел.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Затем настройте контроль доступа для этого сертификата, чтобы служба Service Fabric, которая выполняется под учетной записью сетевой службы, могла использовать его, выполняя следующий сценарий. Укажите отпечаток сертификата и **сетевой службы** для учетной записи службы. Чтобы проверить правильность списков ACL в сертификате, откройте сертификат в **главном** > **Управление сертификатами компьютера** и просмотрите **все задачи** > **Управление закрытыми ключами**.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions, and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow" # "NT AUTHORITY\NetworkService" is the service account
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine-related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current ACL of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ACE to the ACL of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new ACL
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate
    get-acl $keyFullPath| fl
    ```
4. Эти действия необходимо выполнить для каждого сертификата сервера. Воспользуйтесь этими инструкциями, чтобы установить сертификаты клиента на компьютерах, которым нужно предоставить доступ к кластеру.

## <a name="create-the-secure-cluster"></a>Создание защищенного кластера
Когда вы настроите в файле ClusterConfig.X509.MultiMachine.json раздел security, можно приступать к настройке узлов и [созданию кластера](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster). Используйте файл ClusterConfig.X509.MultiMachine.json при создании кластера. Например, команда может выглядеть следующим образом:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Обеспечив защиту автономного кластера под управлением Windows и настройки прошедших проверку подлинности клиентов, которые могут к нему подключаться, выполните инструкции по [подключению к кластеру с помощью PowerShell](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell). Пример:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Затем можно выполнить другие команды PowerShell для работы с этим кластером. Например, вы можете выполнить [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) для отображения списка узлов в этом защищенном кластере.


Чтобы удалить кластер, подключитесь к узлу в кластере, в который вы скачали пакет Service Fabric, откройте командную строку и перейдите в папку с пакетом. Теперь выполните следующую команду.

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Из-за неправильной конфигурации сертификата кластер может не восстановиться во время развертывания. Для самостоятельной диагностики проблем безопасности просмотрите Просмотр событий группы >  **журналы приложений и служб****Microsoft-Service Fabric**.
> 
> 

