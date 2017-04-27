---
title: "通过 Azure IoT 网关 SDK 在 IoT 网关上进行数据转换 | Microsoft Docs"
description: "通过 Azure IoT 网关 SDK 的一个自定义模块，使用 IoT 网关转换传感器数据的格式。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "iot 网关数据转换, iot 网关数据转换"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ca5631755fd9c46c4ee7a0f837dcbf4d75a99c25
ms.lasthandoff: 04/12/2017


---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-gateway-sdk"></a>通过 Azure IoT 网关 SDK，使用 IoT 网关进行传感器数据转换

> [!NOTE]
> 在开始本教程之前，请确保已按顺序完成以下课程：
> * [将 Intel NUC 设置为 IoT 网关](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [使用 IoT 网关将事项连接到云 - SensorTag 到 Azure IoT 中心](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

IoT 网关的一个目的是在将收集的数据发送到云之前，先处理这些数据。 Azure IoT 网关 SDK 引入了可创建并组合形成数据处理工作流的模块。 模块接收消息，对其执行某些操作，然后将其转手供其他模块处理。

## <a name="what-you-learn"></a>学习内容

了解如何创建一个模块，将消息从 SensorTag 转换为其他格式。

## <a name="what-you-do"></a>准备工作

* 创建一个模块，将收到的消息转换为 .json 格式。
* 编译该模块。
* 通过 Azure IoT 网关 SDK，将模块添加到 BLE 示例应用程序。
* 运行示例应用程序。

## <a name="what-you-need"></a>所需条件

* 按顺序完成以下教程：
  * [将 Intel NUC 设置为 IoT 网关](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [使用 IoT 网关将事项连接到云 - SensorTag 到 Azure IoT 中心](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* 在主计算机上运行的 SSH 客户端。 建议在 Windows 上使用 PuTTY。 Linux 和 macOS 已附带 SSH 客户端。
* 用于从 SSH 客户端访问网关的IP 地址、用户名和密码。
* Internet 连接。

## <a name="create-a-module"></a>创建模块

1. 在主计算机上，运行 SSH 客户端并连接到 IoT 网关。
1. 运行以下命令，将转换模块的源文件从 GitHub 克隆到 IoT 网关的主目录：

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   这是采用 C 编程语言编写的本机 Azure 网关 SDK 模块。 该模块将收到的消息转换为以下格式：

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>编译模块

若要编译模块，请运行以下命令：

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$\/\/" build.sh
# run the build shell script
./build.sh
```

编译完成后将获取 `libmy_module.so` 文件。 记下此文件的绝对路径。

## <a name="add-the-module-to-the-ble-sample-application"></a>将模块添加到 BLE 示例应用程序

1. 运行以下命令转到示例文件夹：

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. 运行以下命令打开配置文件：

   ```bash
   vi ble_gateway.json
   ```

1. 将以下代码插入 `modules` 部分以添加模块。

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. 将代码中的 `[Your libmy_module.so path]` 替换为 libmy_module.so` 文件的绝对路径。
1. 将 `links` 部分中的代码替换为以下代码：

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. 按 `ESC`，然后键入 `:wq` 保存文件。

## <a name="run-the-sample-application"></a>运行示例应用程序

1. 打开 SensorTag。
1. 通过运行以下命令，设置 SSL_CERT_FILE 环境变量：

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. 通过运行以下命令，运行具有所添加模块的示例应用程序：

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>后续步骤

已成功使用 IoT 网关将消息从 SensorTag 转换为 .json 格式。
如果希望了解关于 Azure IoT 网关模块的详细信息，可以在以下位置找到更多模块示例：https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules。