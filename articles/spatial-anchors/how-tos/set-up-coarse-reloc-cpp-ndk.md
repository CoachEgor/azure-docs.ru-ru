---
title: Простое уточнение расположения в C++/NDK
description: В этой статье подробно объясняется, как создавать и находить привязки с использованием простого уточнения расположения на C++/NDK.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: db7480b8aa2a78e40cf52d22cbaa7a15422602e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "76545421"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-cndk"></a>Создание и поиск привязок с использованием простого уточнения расположения на C++/NDK

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android (Java)](set-up-coarse-reloc-java.md)
> * [C++ (NDK)](set-up-coarse-reloc-cpp-ndk.md)
> * [C++ (WinRT)](set-up-coarse-reloc-cpp-winrt.md)

Пространственные привязки Azure могут связывать данные датчика размещения на устройстве с созданными вами привязками. С помощью этих данных также можно быстро определить, есть ли привязки рядом с устройством. Дополнительные сведения см. в статье [Coarse relocalization](../concepts/coarse-reloc.md) (Простое уточнение расположения).

## <a name="prerequisites"></a>предварительные требования

Для работы с руководством вам потребуется следующее:

- Базовые знания C++ и <a href="https://developer.android.com/ndk/" target="_blank">Android Native Development Kit</a>.
- Прочесть статью [Описание службы "Пространственные привязки Azure"](../overview.md).
- Выполнить одно из [5-минутных руководств](../index.yml).
- Ознакомиться с инструкциями в статье [Создание и поиск привязок с помощью Пространственных привязок Azure](../create-locate-anchors-overview.md).

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```cpp
// Create the sensor fingerprint provider
std::shared_ptr<PlatformLocationProvider> sensorProvider;
sensorProvider = std::make_shared<PlatformLocationProvider>();

// Allow GPS
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->GeoLocationEnabled(true);

// Allow WiFi scanning
sensors->WifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
std::vector<std::string> uuids;
uuids.push_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.push_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

// Allow the set of known BLE beacons
sensors->BluetoothEnabled(true);
sensors->KnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```cpp
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession->LocationProvider(sensorProvider);

// Configure the near-device criteria
auto nearDeviceCriteria = std::make_shared<NearDeviceCriteria>();
nearDeviceCriteria->DistanceInMeters(5.0f);
nearDeviceCriteria->MaxResultCount(25);

// Set the session's locate criteria
auto anchorLocateCriteria = std::make_shared<AnchorLocateCriteria>();
anchorLocateCriteria->NearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession->CreateWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]