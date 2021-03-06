---
title: Использование пользовательской плитки с поддержкой Markdown на панелях мониторинга Azure
description: Узнайте, как добавить на панель мониторинга Azure плитку с поддержкой Markdown для отображения статического содержимого
ms.date: 01/08/2020
ms.topic: how-to
ms.custom: devx-track-js
ms.openlocfilehash: 5121142ecf568aa1ac9a7ec19f7211c6f9a6253f
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96745763"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Использование плитки с поддержкой Markdown на панелях мониторинга Azure для отображения статического содержимого

Вы можете добавить плитку с поддержкой Markdown на панели мониторинга Azure для отображения настраиваемого статического содержимого. Например, можно отобразить основные инструкции, изображение или набор гиперссылок на плитке Markdown.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Добавление плитки с поддержкой Markdown на панель мониторинга

1. Выберите **Панель мониторинга** на боковой панели портала Azure.

   ![Снимок экрана: Боковая панель портала](./media/azure-portal-markdown-tile/azure-portal-nav.png)

1. Если вы создали какие-либо пользовательские панели, в раскрывающемся меню в представлении панели мониторинга выберите панель, на которой должна отобразиться пользовательская плитка с поддержкой Markdown. Щелкните значок редактирования, чтобы открыть колонку **Коллекция плиток**.

   ![Снимок экрана с представлением редактирования на панели мониторинга](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

1. В **коллекции плиток** перейдите на плитку с именем **Markdown** и выберите **добавить**. Плитка будет добавлена на панель мониторинга, и откроется область **Изменить Markdown**.

1. Введите значения для **заголовка** и **подзаголовка**, которые отображаются на плитке после перехода к другому полю.

   ![Снимок экрана, показывающий результаты ввода заголовка и подзаголовка](./media/azure-portal-markdown-tile/azure-portal-dashboard-enter-title.png)

1. Выберите один из параметров для включения содержимого Markdown: **встроенное редактирование** или **Вставка содержимого с помощью URL-адреса**.

   - Если вы хотите ввести Markdown напрямую, выберите **встроенное редактирование** .

      ![Снимок экрана, показывающий ввод встроенного содержимого](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-inline-content.png)

   - Выберите пункт **Вставить содержимое с помощью URL-адреса** , если вы хотите использовать существующее содержимое Markdown, размещенное в сети.

      ![Снимок экрана, показывающий ввод URL-адреса](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-url.png)

      > [!NOTE]
      > Для повышения безопасности можно создать файл Markdown и сохранить его в [большом двоичном объекте учетной записи хранения Azure, где включено шифрование](../storage/common/storage-service-encryption.md), а затем указать файл с помощью параметра URL-адрес. Содержимое Markdown шифруется с помощью параметров шифрования учетной записи хранения. Только пользователи с разрешениями на доступ к файлу могут видеть содержимое Markdown на панели мониторинга. Может потребоваться задать для учетной записи хранения правило [общего доступа к ресурсам между источниками (CORS)](/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) , чтобы портал Azure ( _https://portal.azure.com/_ ) мог получить доступ к файлу Markdown в большом двоичном объекте.

1. Нажмите кнопку **Готово**, чтобы закрыть область **Изменить Markdown**. Содержимое появится на плитке Markdown, размер которой можно изменить, перетащив маркер в правом нижнем углу.

   ![Снимок экрана с пользовательской плиткой с поддержкой Мarkdown](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Возможности и ограничения содержимого Markdown

На плитке с поддержкой Markdown можно использовать любую комбинацию обычного текста, синтаксиса Markdown и содержимого HTML. На портале Azure используется библиотека _marked_ с открытым исходным кодом для преобразования содержимого, отображаемого на плитке, в формате HTML. Содержимое HTML, сгенерированное библиотекой _marked_, предварительно обрабатывается порталом перед отображением. Этот шаг помогает убедиться, что настройка не повлияет на безопасность или макет портала. Во время этой предварительной обработки удаляется любая часть HTML, представляющая потенциальную угрозу. На портале запрещены следующие типы содержимого:

* JavaScript — теги `<script>` и встроенные вычисления JavaScript будут удалены.
* iframes — теги `<iframe>` будут удалены.
* Стиль — теги `<style>` будут удалены. Встроенные атрибуты стиля для HTML-элементов официально не поддерживаются. Некоторые встроенные элементы стиля могут работать, но если они мешают макету портала, они могут перестать работать в любое время. Плитка с поддержкой Markdown предназначена для основного статического содержимого, использующего стили портала по умолчанию.

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о создании панели мониторинга см. в статье [Создание панелей мониторинга на портале Azure и предоставление общего доступа к ним](../azure-portal/azure-portal-dashboards.md).
