---
title: Управление секретами приложения Service Fabric Azure
description: Узнайте, как защитить значения секретов в приложении Service Fabric (не зависимо от платформы).
author: vturecek
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: 4a489993f982993d5703a9b46d42fffaa6134038
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79259062"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>Управление зашифрованными секретами в приложениях Service Fabric
В этом руководстве описаны шаги по управлению секретами в приложении Service Fabric. Секретом может считаться любая конфиденциальная информации, например строка подключения к хранилищу, пароль или другое значение, которое не должно обрабатываться в виде обычного текста.

Использование зашифрованных секретов в приложении Service Fabric состоит из трех этапов:
* настройка шифрования секретов и сертификата шифрования;
* указание зашифрованных секретов в приложении;
* расшифровка зашифрованных секретов из кода службы.

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>Настройка шифрования секретов и сертификата шифрования
Настройка сертификата шифрования и его использование для шифрования секретов отличается в Windows и Linux.
* [Настройка сертификата шифрования и шифрование секретов в кластерах Windows.][secret-management-windows-specific-link]
* [Настройте сертификат шифрования и Зашифруйте секреты в кластерах Linux.][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>Указание зашифрованных секретов в приложении
Предыдущий шаг описывает, как зашифровать секрет с помощью сертификата и создать строку в формате base-64 для использования в приложении. Эту строку в кодировке Base-64 можно указать как зашифрованный [параметр][parameters-link] в файле Settings. XML службы или в виде зашифрованной [переменной среды][environment-variables-link] в ServiceManifest. XML службы.

Укажите зашифрованный [параметр][parameters-link] в файле конфигурации Settings. XML вашей службы с атрибутом `IsEncrypted`, для которого задано значение `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
Укажите зашифрованную [переменную среды][environment-variables-link] в файле ServiceManifest. XML вашей службы с атрибутом `Type`, для которого задано значение `Encrypted`:
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

Секреты также должны быть добавлены в приложение Service Fabric, указав сертификат в манифесте приложения. Добавьте элемент **секретсцертификате** в **ApplicationManifest. XML** и включите отпечаток требуемого сертификата.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

### <a name="inject-application-secrets-into-application-instances"></a>Включение секретов приложений в экземпляры приложений
В идеале развертывание в разных средах необходимо сделать максимально автоматическим. Этого можно добиться, выполняя шифрование секретов в среде сборки и предоставляя зашифрованные секреты в качестве параметров при создании экземпляров приложений.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Использование переопределяемых параметров в Settings.xml
Файл конфигурации Settings.xml позволяет применять переопределяемые параметры, которые можно предоставить при создании приложения. Вместо предоставления параметру значения используйте атрибут `MustOverride` :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Чтобы переопределить значения в файле Settings.xml, объявите параметр override для службы в файле ApplicationManifest.xml:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Теперь при создании экземпляра приложения можно указать значение в качестве *параметра приложения* . Для облегчения интеграции в процессе сборки создание экземпляра приложения можно выполнить с помощью сценария PowerShell или написать на языке C#.

При использовании PowerShell параметр передается в команду `New-ServiceFabricApplication` в виде [хэш-таблицы](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

При использовании языка C# параметры приложения указываются в `ApplicationDescription` как `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-encrypted-secrets-from-service-code"></a>Расшифровка зашифрованных секретов из кода службы
Интерфейсы API для доступа к [параметрам][parameters-link] и [переменным среды][environment-variables-link] позволяют легко расшифровывать зашифрованные значения. Поскольку зашифрованная строка содержит сведения о сертификате, использованном для шифрования, то указывать сертификат вручную не требуется. Необходимо только установить сертификат на узле, на котором выполняется данная служба.

```csharp
// Access decrypted parameters from Settings.xml
ConfigurationPackage configPackage = FabricRuntime.GetActivationContext().GetConfigurationPackageObject("Config");
bool MySecretIsEncrypted = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].IsEncrypted;
if (MySecretIsEncrypted)
{
    SecureString MySecretDecryptedValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue();
}

// Access decrypted environment variables from ServiceManifest.xml
// Note: you do not have to call any explicit API to decrypt the environment variable.
string MyEnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

## <a name="next-steps"></a>Дальнейшие действия
* Service Fabricное [хранилище секретов](service-fabric-application-secret-store.md) 
* Ознакомьтесь с дополнительными сведениями о [безопасности приложений и служб](service-fabric-application-and-service-security.md).

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[service fabric secrets store]: service-fabric-application-secret-store.md