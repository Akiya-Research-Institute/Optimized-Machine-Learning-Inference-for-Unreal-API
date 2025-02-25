# 動作環境

## 対応Unreal Engineバージョン

- 5.3
- 5.4
- 5.5

ただし、CUDAまたはTensorRTを使用する場合は公式プラグインである「NNERuntimeORT」とは同時に使用することができません。

## 対応プラットフォーム

| Platform                   | Development | Target Build |
| -------------------------- | ----------- | ------------ |
| Windows 10 64bit           | ✅          | ✅          |
| Ubuntu 18.04.6 Desktop 64bit | ⚠️(Experimental) | ⚠️(Experimental) | 
| Android                    |             | ⚠️(Experimental) |

!!! Warning "Windowsでのみテストされています"
    UE5.3以降のバージョンはWindowsでのみテストされています。
    UbuntuやAndroidでは十分にテストされておらず正しく動作することが保証できません。そのため上記の通りExperimentalとしています。

!!! Warning "Androidでのモデル形式"
    Androidで実行するにはDNNモデルをORTフォーマットに変換する必要があります。詳細は[公式ドキュメント](https://onnxruntime.ai/docs/reference/ort-format-models.html#convert-onnx-models-to-ort-format)をご確認ください。

    !!! Warning "既知の問題"
        Androidで読み込めないモデルが存在することがわかっています。

## 対応するハードウェアアクセラレーション

| Platform                   | Default CPU | GPU DirectML | GPU CUDA | GPU TensorRT | NNAPI |
| -------------------------- | ----------- | ------------ | -------- |------------- | ----- |
| Windows 10 64bit           | ✅          | ✅          | ✅       | ✅          |       |
| Ubuntu 18.04.6 Desktop 64bit | ✅          |              | ✅      | ✅          |        |
| Android                    | ✅          |              |          |             | (Not tested yet) |

- DirectMLによるGPUアクセラレーションには、DirectX 12対応GPUが必要です。
- CUDAまたはTensorRTによるGPUアクセラレーションには、
    - 対応するNVIDIA GPUが必要です。
    - UE5.5以降ではデフォルトで有効になっている「NNERuntimeORT」を無効にする必要があります。
    - 下記に記載の特定バージョンのCUDA、cuDNN、TensorRTのインストールが必要です。

### 必要なCUDA, cuDNN, TensorRT バージョン

=== "Windows"

    |          | Other than RTX30** series               | RTX30** series                             |
    | -------- | --------------------------------------- | ------------------------------------------ |
    | CUDA     | 11.0.3                                  | 11.0.3                                     |
    | cuDNN    | 8.0.2 (July 24th, 2020), for CUDA 11.0  | 8.0.5 (November 9th, 2020), for CUDA 11.0  |
    | TensorRT | 7.1.3.4 for CUDA 11.0                   | 7.2.2.3 for CUDA 11.0                      |

    !!! Warning "RTX2080Tiユーザの方"
        RTX2080Tiを使用する場合、cuDNN v8.0.5を使う必要があるかもしれません。上記の組み合わせをいろいろ試してみてください。

=== "Linux"

    |          |                                              |
    | -------- | -------------------------------------------- |
    | CUDA     | 11.4.2 for Linux x86_64 Ubuntu 18.04         |
    | cuDNN    | 8.2.4 (September 2nd, 2021), for CUDA 11.4, Linux x86_64 |
    | TensorRT | 8.2.3.0 (8.2 GA Update 2) for Linux x86_64, CUDA 11.0-11.5 |

    !!! Warning "TensorRTがサポートする演算"
        LinuxでTensorRTを使用する場合、サポートしていない演算が含まれるDNNモデルは読み込めません。  
        どの演算がサポートされているかは、[公式ドキュメント](https://github.com/onnx/onnx-tensorrt/blob/85e79f629fb546a75d61e3027fb259a9529144fe/docs/operators.md)をご覧ください。  
        (NNEngineは、LinuxではTensorRT 8.2を使っています。)