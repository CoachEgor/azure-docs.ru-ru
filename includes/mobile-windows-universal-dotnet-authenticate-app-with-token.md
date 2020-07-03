---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: d71d52257b6e8cfa243207c9bfdb5c7de7d3dd37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "67185968"
---
1. В файле проекта MainPage.xaml.cs добавьте следующие операторы **using** :
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Замените метод **AuthenticateAsync** следующим кодом:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: https://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Sign in with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider, "{url_scheme_of_your_app}");
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must sign in. Sign-In Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    В этой версии **AuthenticateAsync** приложение пытается использовать для доступа к службе учетные данные, хранимые в **PasswordVault**. Обычная попытка входа предпринимается и при отсутствии хранимых учетных данных.
   
   > [!NOTE]
   > Срок действия кэшированного маркера может истечь до или после проверки подлинности, в процессе использования приложения. Чтобы узнать, как определить, истек ли срок действия маркера, см. сведения в [этой статье](https://aka.ms/jww5vp). Решение по обработке ошибок авторизации, связанных с просроченными маркерами, см. в записи [Caching and handling expired tokens in Azure Mobile Services managed SDK](https://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx) (Кэширование и обработка просроченных маркеров в SDK под управлением мобильных служб Azure). 
   > 
   > 
3. Дважды перезапустите приложение.
   
    Обратите внимание, что при первом перезапуске по-прежнему потребуется вход с использованием поставщика. При втором перезапуске будут использоваться кэшированные учетные данные и вход будет пропущен. 

