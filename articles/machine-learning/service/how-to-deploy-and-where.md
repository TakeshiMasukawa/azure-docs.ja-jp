---
title: Web サービスとしてモデルをデプロイする
titleSuffix: Azure Machine Learning service
description: お客様の Azure Machine Learning service モデルをデプロイする方法と場所について説明します (Azure Container Instances、Azure Kubernetes Service、Azure IoT Edge、フィールド プログラマブル ゲート アレイ)。
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 7fc40945588c272ae0ae80ba17b7b3752cab4306
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353313"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning service を使用してモデルをデプロイする

Azure Machine Learning service には、SDK を使用してお客様のトレーニング済みのモデルをデプロイする方法が複数用意されています。 このドキュメントでは、Web サービスとして Azure クラウドに、または IoT エッジ デバイスに、モデルをデプロイする方法を説明します。

> [!IMPORTANT]
> モデルを Web サービスとしてデプロイするときのクロスオリジン リソース共有 (CORS) は、現在サポートされていません。

次のコンピューティング ターゲットにモデルをデプロイできます。

| コンピューティング ターゲット | デプロイの種類 | 説明 |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Web サービス | 高速なデプロイ。 開発またはテストに適しています。 |
| [Azure Kubernetes Service (AKS)](#aks) | Web サービス | 高スケールの運用デプロイに適しています。 自動スケール、および高速な応答時間を実現します。 |
| [Azure IoT Edge](#iotedge) | IoT モジュール | IoT デバイスにモデルをデプロイします。 推論はデバイス上で行われます。 |
| [フィールド プログラマブル ゲート アレイ (FPGA)](#fpga) | Web サービス | リアルタイムの推論に適した超低遅延。 |

モデルのデプロイのプロセスは、すべてのコンピューティング先で同様です。

1. モデルをトレーニングして登録します。
1. モデルを使用するイメージを構成して登録します。
1. イメージをコンピューティング先にデプロイします。
1. 展開をテスト

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


デプロイ ワークフローに関連する概念の詳細については、「[Azure Machine Learning service でモデルを管理、デプロイ、および監視する](concept-model-management-and-deployment.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

- Azure サブスクリプション。 Azure サブスクリプションをお持ちでない場合は、開始する前に無料アカウントを作成してください。 [無料版または有料版の Azure Machine Learning service](http://aka.ms/AMLFree) を今日からお試しいただけます。

- Azure Machine Learning サービス ワークスペースと、Azure Machine Learning SDK for Python がインストール済み。 これら前提条件を取得する方法については､[Azure Machine Learning のクイック スタートの概要](quickstart-get-started.md)で説明しています｡

- トレーニング済みのモデル。 トレーニング済みのモデルがない場合は、[モデルのトレーニング](tutorial-train-models-with-aml.md)に関するチュートリアルの手順を使用して、Azure Machine Learning service で 1 つトレーニングして登録します。

    > [!NOTE]
    > Azure Machine Learning service は Python 3 で読み込むことができる任意の汎用モデルと連携できますが、このドキュメントの例では pickle 形式で格納されたモデルの使用方法を示しています。
    > 
    > ONNX モデルの使用の詳細については、[ONNX と Azure Machine Learning](how-to-build-deploy-onnx.md) に関するドキュメントを参照してください。

## <a id="registermodel"></a> トレーニング済みのモデルを登録する

モデル レジストリは、トレーニング済みのモデルを Azure クラウドに格納して整理するための方法の 1 つです。 モデルは Azure Machine Learning service ワークスペースに登録されています。 モデルは、Azure Machine Learning や別のサービスを使用してトレーニングできます。 ファイルからモデルを登録するには、次のコードを使用します。

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**推定所要時間**: 約 10 秒。

詳細については、[Model クラス](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)のリファレンス ドキュメントを参照してください。

## <a id="configureimage"></a> イメージの作成と登録

デプロイされたモデルは、イメージとしてパッケージ化されます。 イメージには、モデルの実行に必要な依存関係が含まれています。

**Azure Container Instance**、**Azure Kubernetes Service**、**Azure IoT Edge** のデプロイでは、イメージの構成を作成するために [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) クラスが使用されます。 そして、新しい Docker イメージを作成するために、このイメージの構成が使用されます。 

次のコードは、新しいイメージの構成を作成する方法を示しています。

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

**推定所要時間**: 約 10 秒。

この例で使用されている重要なパラメーターを次の表で説明します。

| パラメーター | 説明 |
| ----- | ----- |
| `execution_script` | サービスに送信された要求を受け取るために使用される Python スクリプトを指定します。 この例では、スクリプトは `score.py` ファイルに含まれています。 詳細については、「[実行スクリプト](#script)」セクションを参照してください。 |
| `runtime` | イメージが Python を使用していることを示します。 他に `spark-py` オプションがあります。これは Python と共に Apache Spark を使用します。 |
| `conda_file` | Conda 環境ファイルを指定するために使用されます。 このファイルによって、デプロイされたモデルの Conda 環境が定義されます。 このファイルの作成の詳細については、[環境ファイル (myenv.yml) の作成](tutorial-deploy-models-with-aml.md#create-environment-file)に関するページを参照してください。 |

詳細については、[ContainerImage クラス](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)のリファレンス ドキュメントを参照してください。

### <a id="script"></a> 実行スクリプト

実行スクリプトでは、デプロイ済みイメージに送信されたデータを受け取り、モデルに渡します。 次に、モデルから返された応答を受け取り、それをクライアントに返します。 このスクリプトはこのモデルに固有のものです。そのため、モデルが受け入れ、返すデータを理解する必要があります。 通常、このスクリプトにはモデルの読み込みと実行の 2 つの関数が含まれています。

* `init()`:通常は、この関数によってモデルがグローバル オブジェクトに読み込まれます。 この関数は、Docker コンテナーを開始するときに 1 回だけ実行されます。 

* `run(input_data)`:この関数では、モデルを使用して、入力データに基づいて値が予測されます。 実行に対する入力と出力は、通常、JSON を使用してシリアル化およびシリアル化解除が実行されます。 また、未加工のバイナリ データも使用できます。 モデルに送信する前、またはクライアントに返す前のデータを変換することができます。 

#### <a name="working-with-json-data"></a>JSON データの使用

JSON データを受け入れて返すスクリプトの例を次に示します。 `run` 関数によって、モデルが受け入れる形式に JSON のデータを変換し、応答を JSON に変換してから返します。

```python
# import things required by this script
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

# load the model
def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

# Passes data to the model and returns the prediction
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

#### <a name="working-with-binary-data"></a>バイナリ データの使用

モデルが__バイナリ データ__を受け入れる場合は、`AMLRequest`、`AMLResponse`、および `rawhttp` を使用します。 バイナリ データを受け入れ、POST 要求に対して反転したバイトを返すスクリプトの例を次に示します。 GET 要求に対しては、応答本文で完全な URL を返します。

```python
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

# Accept and return binary data
@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    # handle GET requests
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    # handle POST requests
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `azureml.contrib` 名前空間は、このサービスが改善されるに従って頻繁に変更されます。 そのため、この名前空間内のものはすべて Microsoft によって完全にサポートされているものではなく、プレビューとして見なされる必要があります。
>
> これをローカルの開発環境でテストする必要がある場合は、次のコマンドを使用して、`contrib` 名前空間にコンポーネントをインストールできます。 
> ```shell
> pip install azureml-contrib-services
> ```

### <a id="createimage"></a> イメージを登録する

イメージの構成を作成したら、それを使用してイメージを登録できます。 このイメージは、お客様のワークスペースのコンテナー レジストリに格納されます。 一度作成すれば、同じイメージを複数のサービスにデプロイできます。

```python
# Register the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**推定所要時間**: 約 3 分です。

複数のイメージを同じ名前で登録すると、イメージは自動的にバージョン管理されます。 たとえば、最初に `myimage` として登録されたイメージには、ID `myimage:1` が割り当てられます。 次に `myimage` としてイメージを登録すると、新しいイメージの ID は `myimage:2` になります。

詳細については、[ContainerImage クラス](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)のリファレンス ドキュメントを参照してください。

## <a id="deploy"></a> イメージをデプロイする

デプロイに取りかかる際、お客様がデプロイするコンピューティング先によってプロセスが若干異なります。 次のセクションの情報を使用して、デプロイ方法を確認してください。

* [Azure Container Instances](#aci)
* [Azure Kubernetes Services](#aks)
* [Project Brainwave (フィールド プログラマブル ゲート アレイ)](#fpga)
* [Azure IoT Edge デバイス](#iotedge)

> [!NOTE]
> **Web サービスとしてデプロイする**場合、使用できるデプロイ方法が 3 つあります。
>
> | 方法 | メモ |
> | ----- | ----- |
> | [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) | この方法を使用する前に、モデルを登録し、イメージを作成する必要があります。 |
> | [deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) | この方法を使用する場合、モデルを登録したり、イメージを作成したりする必要はありません。 ただし、モデルまたはイメージの名前、あるいは関連付けられたタグおよび説明を制御することはできません。 |
> | [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) | この方法を使用する場合、イメージを作成する必要はありません。 ただし、作成されるイメージの名前を制御することはできません。 |
>
> このドキュメントの例では、`deploy_from_image` を使用します。

### <a id="aci"></a>Azure Container Instances へのデプロイ

以下の条件が 1 つ以上満たされている場合は、Azure Container Instances を使用してお客様のモデルを Web サービスとしてデプロイします。

- モデルを迅速にデプロイおよび検証する必要があります。 ACI のデプロイは、5 分もかからずに完了します。
- 開発中のモデルをテストします。 ACI のクォータとリージョンの可用性を確認するには、「[Azure Container Instances のクォータとリージョンの可用性](https://docs.microsoft.com/azure/container-instances/container-instances-quotas)」を参照してください。

Azure Container Instances にデプロイするには、次の手順を使用します。

1. デプロイ構成を定義します。 次の例では、1 つの CPU コアと 1 GB のメモリが使用される構成を定義しています。

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. このドキュメントの「[イメージを作成する](#createimage)」セクションで作成されたイメージをデプロイするには、次のコードを使用します。

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **推定所要時間**: 約 3 分です。

    > [!TIP]
    > デプロイ中にエラーが発生した場合は、`service.get_logs()` を使用して AKS サービス ログを表示します。 ログに記録された情報に、エラーの原因が示されている可能性があります。

詳細については、[AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) クラスと [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) クラスのリファレンス ドキュメントを参照してください。

### <a id="aks"></a>Azure Kubernetes Service へのデプロイ

お客様のモデルを高スケールの運用 Web サービスとしてデプロイするには、Azure Kubernetes Service (AKS) を使用します。 既存の AKS クラスターを使用するか、Azure Machine Learning SDK、CLI、または Azure portal を使用して新しいクラスターを作成できます。

AKS クラスターの作成は、ワークスペースに対する 1 回限りのプロセスです。 複数のデプロイでこのクラスターを再利用できます。 クラスターを削除した場合、次回デプロイする必要があるときに、新しいクラスターを作成する必要があります。

Azure Kubernetes Service には次の機能があります。

* 自動スケール
* ログの記録
* モデル データ収集
* Web サービスの高速な応答時間

#### <a name="create-a-new-cluster"></a>新しいクラスターを作成する

新しい Azure Kubernetes Service クラスターを作成するには、次のコードを使用します。

> [!IMPORTANT]
> AKS クラスターの作成は、ワークスペースの 1 回限りのプロセスです。 一度作成すれば、複数のデプロイでこのクラスターを再利用できます。 クラスターまたはそれを含むリソース グループを削除した場合、次回デプロイする必要があるときに、新しいクラスターを作成する必要があります。
> [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) で、agent_count と vm_size にカスタム値を使用する場合は、agent_count と vm_size の積が 12 仮想 CPU 以上である必要があります。 たとえば、vm_size に "Standard_D3_v2" (4 仮想 CPU) を使用する場合、agent_count は 3 以上にする必要があります。

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aml-aks-1' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                    name = aks_name, 
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

**推定所要時間**: 約 20 分です。

#### <a name="use-an-existing-cluster"></a>既存のクラスターを使用する

お客様の Azure サブスクリプションに既に AKS クラスターがあり、そのバージョンが 1.11.* である場合は、それを使用してお客様のイメージをデプロイできます。 次のコードは、既存のクラスターをお客様のワークスペースにアタッチする方法を示しています。

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attatch the cluster to your workgroup
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

**推定所要時間**: 約 3 分です。

#### <a name="deploy-the-image"></a>イメージをデプロイする

このドキュメントの「[イメージを作成する](#createimage)」セクションで作成されたイメージを Azure Kubernetes Server クラスターにデプロイするには、次のコードを使用します。

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
service.wait_for_deployment(show_output = True)
print(service.state)
```

**推定所要時間**: 約 3 分です。

> [!TIP]
> デプロイ中にエラーが発生した場合は、`service.get_logs()` を使用して AKS サービス ログを表示します。 ログに記録された情報に、エラーの原因が示されている可能性があります。

詳細については、[AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) クラスと [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) クラスのリファレンス ドキュメントを参照してください。

### <a id="fpga"></a>フィールド プログラマブル ゲート アレイ (FPGA) へのデプロイ

Project Brainwave を使用すると、リアルタイムの推論要求に対して超低遅延を実現できます。 Project Brainwave では、Azure クラウドのフィールド プログラマブル ゲート アレイにデプロイされているディープ ニューラル ネットワーク (DNN) が高速化されます。 一般に使用される DNN は、転移学習の Featurizer として使用したり、独自のデータからトレーニングされた重みでカスタマイズしたりできます。

Project Brainwave を使用したモデルのデプロイのチュートリアルについては、[FPGA へのデプロイ](how-to-deploy-fpga-web-service.md)に関するドキュメントを参照してください。

### <a id="iotedge"></a>Azure IoT Edge へのデプロイ

Azure IoT Edge デバイスとは、Azure IoT Edge ランタイムを実行している Linux または Windows ベースのデバイスです。 機械学習モデルは、これらのデバイスに IoT Edge モジュールとしてデプロイできます。 IoT Edge デバイスにモデルをデプロイすると、データを処理するためにクラウドに送信する必要がなくなり、デバイスがモデルを直接使用できます。 応答時間が早くなり、データ転送量が少なくなります。

Azure IoT Edge モジュールは、コンテナー レジストリからお客様のデバイスにデプロイされます。 お客様のモデルからイメージを作成すると、お客様のワークスペースのコンテナー レジストリにそれが格納されます。

#### <a name="set-up-your-environment"></a>環境をセットアップする

* 開発環境。 詳細については、「[開発環境を構成する方法](how-to-configure-environment.md)」のドキュメントを参照してください。

* Azure サブスクリプション内の [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md)。 

* トレーニング済みのモデル。 モデルをトレーニングする方法の例は、「[Azure Machine Learning で画像分類モデルをトレーニングする方法](tutorial-train-models-with-aml.md)」のドキュメントを参照してください。 あらかじめトレーニングされたモデルは、[Azure IoT Edge 用 AI ツールキットの GitHub リポジトリ](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial)で入手できます。

#### <a name="prepare-the-iot-device"></a>IoT デバイスの準備
IoT ハブを作成し、[このスクリプト](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister)を使用してデバイスを登録または再利用する必要があります。

``` bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister
sudo chmod +x createNregister
sudo ./createNregister <The Azure subscriptionID you want to use> <Resourcegroup to use or create for the IoT hub> <Azure location to use e.g. eastus2> <the Hub ID you want to use or create> <the device ID you want to create>
```

結果として得られる接続文字列を "cs":"{copy this string}" の後ろに保存します。

[このスクリプト](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge)を UbuntuX64 IoT Edge ノードまたは DSVM にダウンロードしてお客様のデバイスを初期化し、次のコマンドを実行します。

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

IoT Edge ノードは、お客様の IoT ハブの接続文字列を受け入れる準備が整っています。 ```device_connection_string:``` の行を探して、引用符に囲まれた上記の接続文字列を貼り付けます。

また、お客様のデバイスを登録して IoT ランタイムをインストールする方法を詳しく確認するには、「[クイック スタート: 初めての IoT Edge モジュールを Linux x64 デバイスに展開する](../../iot-edge/quickstart-linux.md)」のドキュメントに従ってください。


#### <a name="get-the-container-registry-credentials"></a>コンテナー レジストリの資格情報の取得
IoT Edge モジュールをお客様のデバイスにデプロイするには、Azure Machine Learning service によって Docker イメージが格納されるコンテナー レジストリの資格情報が Azure IoT に必要です。

次の 2 つの方法で、必要なコンテナー レジストリの資格情報を簡単に取得できます。

+ **Azure portal**:

  1. [Azure Portal](https://portal.azure.com/signin/index) にサインインします。

  1. Azure Machine Learning サービス ワークスペースに移動し、__[概要]__ を選択します。 コンテナー レジストリの設定に移動するには、__[レジストリ]__ リンクを選択します。

     ![コンテナー レジストリ エントリの画像](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. コンテナー レジストリに入ったら、**[アクセス キー]** を選択して、管理者ユーザーを有効にします。
 
     ![アクセス キー画面の画像](./media/how-to-deploy-and-where/findaccesskey.png)

  1. **[ログイン サーバー]**、**[ユーザー名]**、**[パスワード]** の値を保存します。 

+ **Python スクリプト**:

  1. コンテナーを作成するために上記で実行したコードの後に、次の Python スクリプトを使用します。

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. ContainerURL、servername、username、password の値を保存します。 

     これらの資格情報は、お客様のプライベート コンテナー レジストリ内のイメージに IoT Edge デバイスからアクセスできるようにするために必要です。

#### <a name="deploy-the-model-to-the-device"></a>デバイスにモデルをデプロイする

[このスクリプト](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel)を実行し、上記の手順で取得した情報 (コンテナー レジストリ名、ユーザー名、パスワード、イメージの場所の URL、目的のデプロイの名前、IoT Hub 名、お客様が作成したデバイスの ID) を指定することで、モデルを簡単にデプロイできます。 次の手順に従って、VM でこれを実行できます。 

```bash 
wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel
sudo chmod +x deploymodel
sudo ./deploymodel <ContainerRegistryName> <username> <password> <imageLocationURL> <DeploymentID> <IoTHubname> <DeviceID>
```

または、「[Azure portal から Azure IoT Edge モジュールをデプロイする](../../iot-edge/how-to-deploy-modules-portal.md)」のドキュメントの手順に従って、イメージをお客様のデバイスにデプロイできます。 デバイスの__レジストリ設定__を構成するときは、お客様のワークスペースにあるコンテナー レジストリの__ログイン サーバー__、__ユーザー名__、__パスワード__を使用します。

> [!NOTE]
> Azure IoT を使い慣れていない場合は、サービスの概要について次のドキュメントで確認してください。
>
> * [クイック スタート: 初めての IoT Edge モジュールを Linux デバイスにデプロイする](../../iot-edge/quickstart-linux.md)
> * [クイック スタート: 初めての IoT Edge モジュールを Windows デバイスにデプロイする](../../iot-edge/quickstart.md)


## <a name="testing-web-service-deployments"></a>Web サービスのデプロイのテスト

Web サービスのデプロイをテストするには、Webservice オブジェクトの `run` メソッドを使用できます。 次の例では、JSON ドキュメントが Web サービスに設定され、結果が表示されます。 送信されるデータは、モデルによって期待されるデータと一致する必要があります。 この例では、データ形式は糖尿病モデルによって期待される入力と一致しています。

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

Web サービスは REST API なので、さまざまなプログラミング言語でクライアント アプリケーションを作成できます。 詳細については、[Web サービスを使用するクライアント アプリケーションの作成](how-to-consume-web-service.md)に関するページを参照してください。

## <a id="update"></a> Web サービスを更新する

Web サービスを更新するには、`update` メソッドを使用します。 次のコードは、Web サービスを更新して新しいイメージを使用する方法を示しています。

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

> [!NOTE]
> イメージの更新時に、Web サービスは自動的には更新されません。 新しいイメージを使用するには、各サービスを手動で更新する必要があります。

詳細については、[Webservice クラス](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)のリファレンス ドキュメントを参照してください。

## <a name="clean-up"></a>クリーンアップ

デプロイされた Web サービスを削除するには、`service.delete()` を使用します。

イメージを削除するには、`image.delete()` を使用します。

登録済みのモデルを削除するには、`model.delete()` を使用します。

詳細については、[WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--)、[Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--)、および [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--) のリファレンス ドキュメントを参照してください。

## <a name="next-steps"></a>次の手順

* [SSL を使用して Azure Machine Learning Web サービスをセキュリティで保護する](how-to-secure-web-service.md)
* [Web サービスとしてデプロイされた ML モデルを使用する](how-to-consume-web-service.md)
* [バッチ予測を実行する方法](how-to-run-batch-predictions.md)
* [Application Insights を使用して Azure Machine Learning のモデルを監視する](how-to-enable-app-insights.md)
* [実稼働環境でモデルのデータを収集する](how-to-enable-data-collection.md)
* [Azure Machine Learning service SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning service と Azure Virtual Network を使用する](how-to-enable-virtual-network.md)
