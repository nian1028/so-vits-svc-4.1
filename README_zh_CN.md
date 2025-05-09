# 马上要高考了，SvcDevelopTeam在此助各位考生高考旗开得胜，超常发挥。

# SoftVC VITS Singing Voice Conversion

[**English**](./README.md) | [**中文简体**](./README_zh_CN.md)

#### ✨ 带有F0曲线编辑器，角色混合时间轴编辑器的推理端 (Onnx模型的用途) : [MoeVoiceStudio(即将到来)](https://github.com/NaruseMioShirakana/MoeVoiceStudio)

#### ✨ 改善了交互的一个分支推荐：[34j/so-vits-svc-fork](https://github.com/34j/so-vits-svc-fork)

#### ✨ 支持实时转换的一个客户端：[w-okada/voice-changer](https://github.com/w-okada/voice-changer)

**本项目与Vits有着根本上的不同。Vits是TTS，本项目是SVC。本项目无法实现TTS，Vits也无法实现SVC，这两个项目的模型是完全不通用的。**

## 重要通知

这个项目是为了让开发者最喜欢的动画角色唱歌而开发的，任何涉及真人的东西都与开发者的意图背道而驰。

## 声明

本项目为开源、离线的项目，SvcDevelopTeam的所有成员与本项目的所有开发者以及维护者（以下简称贡献者）对本项目没有控制力。本项目的贡献者从未向任何组织或个人提供包括但不限于数据集提取、数据集加工、算力支持、训练支持、推理等一切形式的帮助；本项目的贡献者不知晓也无法知晓使用者使用该项目的用途。故一切基于本项目训练的AI模型和合成的音频都与本项目贡献者无关。一切由此造成的问题由使用者自行承担。

此项目完全离线运行，不能收集任何用户信息或获取用户输入数据。因此，这个项目的贡献者不知道所有的用户输入和模型，因此不负责任何用户输入。

本项目只是一个框架项目，本身并没有语音合成的功能，所有的功能都需要用户自己训练模型。同时，这个项目没有任何模型，任何二次分发的项目都与这个项目的贡献者无关。

## 📏 使用规约

# Warning：请自行解决数据集授权问题，禁止使用非授权数据集进行训练！任何由于使用非授权数据集进行训练造成的问题，需自行承担全部责任和后果！与仓库、仓库维护者、svc develop team 无关！

1. 本项目是基于学术交流目的建立，仅供交流与学习使用，并非为生产环境准备。
2. 任何发布到视频平台的基于 sovits 制作的视频，都必须要在简介明确指明用于变声器转换的输入源歌声、音频，例如：使用他人发布的视频 / 音频，通过分离的人声作为输入源进行转换的，必须要给出明确的原视频、音乐链接；若使用是自己的人声，或是使用其他歌声合成引擎合成的声音作为输入源进行转换的，也必须在简介加以说明。
3. 由输入源造成的侵权问题需自行承担全部责任和一切后果。使用其他商用歌声合成软件作为输入源时，请确保遵守该软件的使用条例，注意，许多歌声合成引擎使用条例中明确指明不可用于输入源进行转换！
4. 禁止使用该项目从事违法行为与宗教、政治等活动，该项目维护者坚决抵制上述行为，不同意此条则禁止使用该项目。
5. 继续使用视为已同意本仓库 README 所述相关条例，本仓库 README 已进行劝导义务，不对后续可能存在问题负责。
6. 如果将此项目用于任何其他企划，请提前联系并告知本仓库作者，十分感谢。

## 📝 模型简介

歌声音色转换模型，通过SoftVC内容编码器提取源音频语音特征，与F0同时输入VITS替换原本的文本输入达到歌声转换的效果。同时，更换声码器为 [NSF HiFiGAN](https://github.com/openvpi/DiffSinger/tree/refactor/modules/nsf_hifigan)解决断音问题。

### 🆕 4.1-Stable 版本更新内容

+ 特征输入更换为 [Content Vec](https://github.com/auspicious3000/contentvec) 的第12层Transformer输出，并兼容4.0分支
+ 更新浅层扩散，可以使用浅层扩散模型提升音质
+ 增加whisper语音编码器的支持
+ 增加静态/动态声线融合
+ 增加响度嵌入
+ 增加特征检索,来自于[RVC](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)

### 🆕 关于兼容4.0模型的问题

+ 可通过修改4.0模型的config.json对4.0的模型进行支持，需要在config.json的model字段中添加speech_encoder字段，具体见下

```
  "model": {
    .........
    "ssl_dim": 256,
    "n_speakers": 200,
    "speech_encoder":"vec256l9"
  }
```

### 🆕 关于浅扩散
![Diagram](shadowdiffusion.png)

## 💬 关于 Python 版本问题

在进行测试后，我们认为`Python 3.8.9`能够稳定地运行该项目

## 📥 预先下载的模型文件

#### **必须项**

**以下编码器需要选择一个使用**

##### **1. 若使用contentvec作为声音编码器（推荐）**
+ contentvec ：[checkpoint_best_legacy_500.pt](https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr)
  + 放在`pretrain`目录下

```shell
# contentvec
wget -P pretrain/ http://obs.cstcloud.cn/share/obs/sankagenkeshi/checkpoint_best_legacy_500.pt
# 也可手动下载放在pretrain目录
```

##### **2. 若使用hubertsoft作为声音编码器**
+ soft vc hubert：[hubert-soft-0d54a1f4.pt](https://github.com/bshall/hubert/releases/download/v0.1/hubert-soft-0d54a1f4.pt)
  + 放在`pretrain`目录下

##### **3. 若使用Whisper-ppg作为声音编码器**
- download model at [medium.pt](https://openaipublic.azureedge.net/main/whisper/models/345ae4da62f9b3d59415adc60127b97c714f32e89e936602e85993674d08dcb1/medium.pt)
  - 放在`pretrain`目录下
 
##### **4. 若使用OnnxHubert/ContentVec作为声音编码器**
- download model at [MoeSS-SUBModel](https://huggingface.co/NaruseMioShirakana/MoeSS-SUBModel/tree/main)
  - 放在`pretrain`目录下

#### **编码器列表**
- "vec768l12"
- "vec256l9"
- "vec256l9-onnx"
- "vec256l12-onnx"
- "vec768l9-onnx"
- "vec768l12-onnx"
- "hubertsoft-onnx"
- "hubertsoft"
- "whisper-ppg"

#### **可选项(强烈建议使用)**

+ 预训练底模文件： `G_0.pth` `D_0.pth`
  + 放在`logs/44k`目录下

+ 扩散模型预训练底模文件： `model_0.pt `
  + 放在`logs/44k/diffusion`目录下

从svc-develop-team(待定)或任何其他地方获取Sovits底模

扩散模型引用了[DDSP-SVC](https://github.com/yxlllc/DDSP-SVC)的Diffusion Model，底模与[DDSP-SVC](https://github.com/yxlllc/DDSP-SVC)的扩散模型底模通用，可以去[DDSP-SVC](https://github.com/yxlllc/DDSP-SVC)获取扩散模型的底模

虽然底模一般不会引起什么版权问题，但还是请注意一下，比如事先询问作者，又或者作者在模型描述中明确写明了可行的用途

#### **可选项(根据情况选择)**

如果使用`NSF-HIFIGAN增强器`或`浅层扩散`的话，需要下载预训练的NSF-HIFIGAN模型，如果不需要可以不下载

+ 预训练的NSF-HIFIGAN声码器 ：[nsf_hifigan_20221211.zip](https://github.com/openvpi/vocoders/releases/download/nsf-hifigan-v1/nsf_hifigan_20221211.zip)
  + 解压后，将四个文件放在`pretrain/nsf_hifigan`目录下

```shell
# nsf_hifigan
wget -P pretrain/ https://github.com/openvpi/vocoders/releases/download/nsf-hifigan-v1/nsf_hifigan_20221211.zip
unzip -od pretrain/nsf_hifigan pretrain/nsf_hifigan_20221211.zip
# 也可手动下载放在pretrain/nsf_hifigan目录
# 地址：https://github.com/openvpi/vocoders/releases/tag/nsf-hifigan-v1
```

## 📊 数据集准备

仅需要以以下文件结构将数据集放入dataset_raw目录即可

```
dataset_raw
├───speaker0
│   ├───xxx1-xxx1.wav
│   ├───...
│   └───Lxx-0xx8.wav
└───speaker1
    ├───xx2-0xxx2.wav
    ├───...
    └───xxx7-xxx007.wav
```

可以自定义说话人名称

```
dataset_raw
└───suijiSUI
    ├───1.wav
    ├───...
    └───25788785-20221210-200143-856_01_(Vocals)_0_0.wav
```

## 🛠️ 数据预处理

### 0. 音频切片

将音频切片至`5s - 15s`, 稍微长点也无伤大雅，实在太长可能会导致训练中途甚至预处理就爆显存

可以使用[audio-slicer-GUI](https://github.com/flutydeer/audio-slicer)、[audio-slicer-CLI](https://github.com/openvpi/audio-slicer)

一般情况下只需调整其中的`Minimum Interval`，普通陈述素材通常保持默认即可，歌唱素材可以调整至`100`甚至`50`

切完之后手动删除过长过短的音频

**如果你使用Whisper-ppg声音编码器进行训练，所有的切片长度必须小于30s**

### 1. 重采样至44100Hz单声道

```shell
python resample.py
```

#### 注意

虽然本项目拥有重采样、转换单声道与响度匹配的脚本resample.py，但是默认的响度匹配是匹配到0db。这可能会造成音质的受损。而python的响度匹配包pyloudnorm无法对电平进行压限，这会导致爆音。所以建议可以考虑使用专业声音处理软件如`adobe audition`等软件做重采样、转换单声道与响度匹配处理。若使用其他软件做重采样、转换单声道与响度匹配，则可以不运行上述命令。

若手动处理音频，需要以以下文件结构将数据集放入dataset目录即可。若无该目录可以自行创建。

```
dataset
└───44k
    ├───speaker0
    │   ├───xxx1-xxx1.wav
    │   ├───...
    │   └───Lxx-0xx8.wav
    └───speaker1
        ├───xx2-0xxx2.wav
        ├───...
        └───xxx7-xxx007.wav
```

可以自定义说话人名称

```
dataset
└───44k
     └───suijiSUI
           ├───1.wav
           ├───...
           └───25788785-20221210-200143-856_01_(Vocals)_0_0.wav
```

### 2. 自动划分训练集、验证集，以及自动生成配置文件

```shell
python preprocess_flist_config.py --speech_encoder vec768l12
```

speech_encoder拥有四个选择

```
vec768l12
vec256l9
hubertsoft
whisper-ppg
```

如果省略speech_encoder参数，默认值为vec768l12

**使用响度嵌入**

若使用响度嵌入，需要增加`--vol_aug`参数，比如：

```shell
python preprocess_flist_config.py --speech_encoder vec768l12 --vol_aug
```

使用后训练出的模型将匹配到输入源响度，否则为训练集响度。

#### 此时可以在生成的config.json与diffusion.yaml修改部分参数

* `keep_ckpts`：训练时保留最后几个模型，`0`为保留所有，默认只保留最后`3`个

* `all_in_mem`,`cache_all_data`：加载所有数据集到内存中，某些平台的硬盘IO过于低下、同时内存容量 **远大于** 数据集体积时可以启用

* `batch_size`：单次训练加载到GPU的数据量，调整到低于显存容量的大小即可


### 3. 生成hubert与f0

```shell
python preprocess_hubert_f0.py --f0_predictor dio
```

f0_predictor拥有四个选择

```
crepe
dio
pm
harvest
```

如果训练集过于嘈杂，请使用crepe处理f0

如果省略f0_predictor参数，默认值为dio

尚若需要浅扩散功能（可选），需要增加--use_diff参数，比如

```shell
python preprocess_hubert_f0.py --f0_predictor dio --use_diff
```

执行完以上步骤后 dataset 目录便是预处理完成的数据，可以删除 dataset_raw 文件夹了

## 🏋️‍♀️ 训练

### 扩散模型（可选）

尚若需要浅扩散功能，需要训练扩散模型，扩散模型训练方法为:

```shell
python train_diff.py -c configs/diffusion.yaml
```

### 主模型训练

```shell
python train.py -c configs/config.json -m 44k
```

模型训练结束后，模型文件保存在`logs/44k`目录下，扩散模型在`logs/44k/diffusion`下

## 🤖 推理

使用 [inference_main.py](inference_main.py)

```shell
# 例
python inference_main.py -m "logs/44k/G_30400.pth" -c "configs/config.json" -n "君の知らない物語-src.wav" -t 0 -s "nen"
```

必填项部分：
+ `-m` | `--model_path`：模型路径
+ `-c` | `--config_path`：配置文件路径
+ `-n` | `--clean_names`：wav 文件名列表，放在 raw 文件夹下
+ `-t` | `--trans`：音高调整，支持正负（半音）
+ `-s` | `--spk_list`：合成目标说话人名称
+ `-cl` | `--clip`：音频强制切片，默认0为自动切片，单位为秒/s

可选项部分：部分具体见下一节
+ `-lg` | `--linear_gradient`：两段音频切片的交叉淡入长度，如果强制切片后出现人声不连贯可调整该数值，如果连贯建议采用默认值0，单位为秒
+ `-f0p` | `--f0_predictor`：选择F0预测器,可选择crepe,pm,dio,harvest,默认为pm(注意：crepe为原F0使用均值滤波器)
+ `-a` | `--auto_predict_f0`：语音转换自动预测音高，转换歌声时不要打开这个会严重跑调
+ `-cm` | `--cluster_model_path`：聚类模型或特征检索索引路径，如果没有训练聚类或特征检索则随便填
+ `-cr` | `--cluster_infer_ratio`：聚类方案或特征检索占比，范围0-1，若没有训练聚类模型或特征检索则默认0即可
+ `-eh` | `--enhance`：是否使用NSF_HIFIGAN增强器,该选项对部分训练集少的模型有一定的音质增强效果，但是对训练好的模型有反面效果，默认关闭
+ `-shd` | `--shallow_diffusion`：是否使用浅层扩散，使用后可解决一部分电音问题，默认关闭，该选项打开时，NSF_HIFIGAN增强器将会被禁止
+ `-usm` | `--use_spk_mix`：是否使用角色融合/动态声线融合
+ `-lea` | `--loudness_envelope_adjustment`：输入源响度包络替换输出响度包络融合比例，越靠近1越使用输出响度包络
+ `-fr` | `--feature_retrieval`：是否使用特征检索，如果使用聚类模型将被禁用，且cm与cr参数将会变成特征检索的索引路径与混合比例

浅扩散设置：
+ `-dm` | `--diffusion_model_path`：扩散模型路径
+ `-dc` | `--diffusion_config_path`：扩散模型配置文件路径
+ `-ks` | `--k_step`：扩散步数，越大越接近扩散模型的结果，默认100
+ `-od` | `--only_diffusion`：纯扩散模式，该模式不会加载sovits模型，以扩散模型推理
+ `-se` | `--second_encoding`：二次编码，浅扩散前会对原始音频进行二次编码，玄学选项，有时候效果好，有时候效果差

### 注意！

如果使用`whisper-ppg` speech encoder 进行推理，需要将`--clip`设置为25，`-lg`设置为1。否则将无法正常推理。

## 🤔 可选项

如果前面的效果已经满意，或者没看明白下面在讲啥，那后面的内容都可以忽略，不影响模型使用(这些可选项影响比较小，可能在某些特定数据上有点效果，但大部分情况似乎都感知不太明显)

### 自动f0预测

4.0模型训练过程会训练一个f0预测器，对于语音转换可以开启自动音高预测，如果效果不好也可以使用手动的，但转换歌声时请不要启用此功能！！！会严重跑调！！
+ 在inference_main中设置auto_predict_f0为true即可

### 聚类音色泄漏控制

介绍：聚类方案可以减小音色泄漏，使得模型训练出来更像目标的音色（但其实不是特别明显），但是单纯的聚类方案会降低模型的咬字（会口齿不清）（这个很明显），本模型采用了融合的方式，可以线性控制聚类方案与非聚类方案的占比，也就是可以手动在"像目标音色" 和 "咬字清晰" 之间调整比例，找到合适的折中点

使用聚类前面的已有步骤不用进行任何的变动，只需要额外训练一个聚类模型，虽然效果比较有限，但训练成本也比较低

+ 训练过程：
  + 使用cpu性能较好的机器训练，据我的经验在腾讯云6核cpu训练每个speaker需要约4分钟即可完成训练
  + 执行`python cluster/train_cluster.py`，模型的输出会在`logs/44k/kmeans_10000.pt`
  + 聚类模型目前可以使用gpu进行训练，执行`python cluster/train_cluster.py --gpu`
+ 推理过程：
  + `inference_main.py`中指定`cluster_model_path`
  + `inference_main.py`中指定`cluster_infer_ratio`，`0`为完全不使用聚类，`1`为只使用聚类，通常设置`0.5`即可

### 特征检索

介绍：跟聚类方案可以减小音色泄漏，咬字比聚类稍好，但会降低推理速度，采用了融合的方式，可以线性控制特征检索与非特征检索的占比，

+ 训练过程：
  首先需要在生成hubert与f0后执行：

```shell
python train_index.py -c configs/config.json
```

模型的输出会在`logs/44k/feature_and_index.pkl`

+ 推理过程：
  + 需要首先制定`--feature_retrieval`，此时聚类方案会自动切换到特征检索方案
  + `inference_main.py`中指定`cluster_model_path` 为模型输出文件
  + `inference_main.py`中指定`cluster_infer_ratio`，`0`为完全不使用特征检索，`1`为只使用特征检索，通常设置`0.5`即可

### [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/svc-develop-team/so-vits-svc/blob/4.1-Stable/sovits4_for_colab.ipynb) [sovits4_for_colab.ipynb](https://colab.research.google.com/github/svc-develop-team/so-vits-svc/blob/4.1-Stable/sovits4_for_colab.ipynb)

## 🗜️ 模型压缩

生成的模型含有继续训练所需的信息。如果确认不再训练，可以移除模型中此部分信息，得到约 1/3 大小的最终模型。

使用 [compress_model.py](compress_model.py)

```shell
# 例
python compress_model.py -c="configs/config.json" -i="logs/44k/G_30400.pth" -o="logs/44k/release.pth"
```

## 👨‍🔧 声线混合

### 静态声线混合

**参考`webUI.py`文件中，小工具/实验室特性的静态声线融合。**

介绍:该功能可以将多个声音模型合成为一个声音模型(多个模型参数的凸组合或线性组合)，从而制造出现实中不存在的声线 
**注意：**

1. 该功能仅支持单说话人的模型
2. 如果强行使用多说话人模型，需要保证多个模型的说话人数量相同，这样可以混合同一个SpaekerID下的声音
3. 保证所有待混合模型的config.json中的model字段是相同的
4. 输出的混合模型可以使用待合成模型的任意一个config.json，但聚类模型将不能使用
5. 批量上传模型的时候最好把模型放到一个文件夹选中后一起上传
6. 混合比例调整建议大小在0-100之间，也可以调为其他数字，但在线性组合模式下会出现未知的效果
7. 混合完毕后，文件将会保存在项目根目录中，文件名为output.pth
8. 凸组合模式会将混合比例执行Softmax使混合比例相加为1，而线性组合模式不会

### 动态声线混合

**参考`spkmix.py`文件中关于动态声线混合的介绍**

角色混合轨道 编写规则：

角色ID : \[\[起始时间1, 终止时间1, 起始数值1, 起始数值1], [起始时间2, 终止时间2, 起始数值2, 起始数值2]]

起始时间和前一个的终止时间必须相同，第一个起始时间必须为0，最后一个终止时间必须为1 （时间的范围为0-1）

全部角色必须填写，不使用的角色填\[\[0., 1., 0., 0.]]即可

融合数值可以随便填，在指定的时间段内从起始数值线性变化为终止数值，内部会自动确保线性组合为1（凸组合条件），可以放心使用

推理的时候使用`--use_spk_mix`参数即可启用动态声线混合

## 📤 Onnx导出

使用 [onnx_export.py](onnx_export.py)

+ 新建文件夹：`checkpoints` 并打开
+ 在`checkpoints`文件夹中新建一个文件夹作为项目文件夹，文件夹名为你的项目名称，比如`aziplayer`
+ 将你的模型更名为`model.pth`，配置文件更名为`config.json`，并放置到刚才创建的`aziplayer`文件夹下
+ 将 [onnx_export.py](onnx_export.py) 中`path = "NyaruTaffy"` 的 `"NyaruTaffy"` 修改为你的项目名称，`path = "aziplayer" (onnx_export_speaker_mix，为支持角色混合的onnx导出)`
+ 运行 [onnx_export.py](onnx_export.py) 
+ 等待执行完毕，在你的项目文件夹下会生成一个`model.onnx`，即为导出的模型

注意：Hubert Onnx模型请使用MoeSS提供的模型，目前无法自行导出（fairseq中Hubert有不少onnx不支持的算子和涉及到常量的东西，在导出时会报错或者导出的模型输入输出shape和结果都有问题）

## ☀️ 旧贡献者

因为某些原因原作者进行了删库处理，本仓库重建之初由于组织成员疏忽直接重新上传了所有文件导致以前的contributors全部木大，现在在README里重新添加一个旧贡献者列表

*某些成员已根据其个人意愿不将其列出*

<table>
  <tr>
    <td align="center"><a href="https://github.com/MistEO"><img src="https://avatars.githubusercontent.com/u/18511905?v=4" width="100px;" alt=""/><br /><sub><b>MistEO</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/XiaoMiku01"><img src="https://avatars.githubusercontent.com/u/54094119?v=4" width="100px;" alt=""/><br /><sub><b>XiaoMiku01</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/ForsakenRei"><img src="https://avatars.githubusercontent.com/u/23041178?v=4" width="100px;" alt=""/><br /><sub><b>しぐれ</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/TomoGaSukunai"><img src="https://avatars.githubusercontent.com/u/25863522?v=4" width="100px;" alt=""/><br /><sub><b>TomoGaSukunai</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/Plachtaa"><img src="https://avatars.githubusercontent.com/u/112609742?v=4" width="100px;" alt=""/><br /><sub><b>Plachtaa</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/zdxiaoda"><img src="https://avatars.githubusercontent.com/u/45501959?v=4" width="100px;" alt=""/><br /><sub><b>zd小达</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/Archivoice"><img src="https://avatars.githubusercontent.com/u/107520869?v=4" width="100px;" alt=""/><br /><sub><b>凍聲響世</b></sub></a><br /></td>
  </tr>
</table>

## 📚 一些法律条例参考

#### 任何国家，地区，组织和个人使用此项目必须遵守以下法律

#### 《民法典》

##### 第一千零一十九条

任何组织或者个人不得以丑化、污损，或者利用信息技术手段伪造等方式侵害他人的肖像权。未经肖像权人同意，不得制作、使用、公开肖像权人的肖像，但是法律另有规定的除外。未经肖像权人同意，肖像作品权利人不得以发表、复制、发行、出租、展览等方式使用或者公开肖像权人的肖像。对自然人声音的保护，参照适用肖像权保护的有关规定。

##### 第一千零二十四条

【名誉权】民事主体享有名誉权。任何组织或者个人不得以侮辱、诽谤等方式侵害他人的名誉权。

##### 第一千零二十七条

【作品侵害名誉权】行为人发表的文学、艺术作品以真人真事或者特定人为描述对象，含有侮辱、诽谤内容，侵害他人名誉权的，受害人有权依法请求该行为人承担民事责任。行为人发表的文学、艺术作品不以特定人为描述对象，仅其中的情节与该特定人的情况相似的，不承担民事责任。

#### 《[中华人民共和国宪法](http://www.gov.cn/guoqing/2018-03/22/content_5276318.htm)》

#### 《[中华人民共和国刑法](http://gongbao.court.gov.cn/Details/f8e30d0689b23f57bfc782d21035c3.html?sw=中华人民共和国刑法)》

#### 《[中华人民共和国民法典](http://gongbao.court.gov.cn/Details/51eb6750b8361f79be8f90d09bc202.html)》

## 💪 感谢所有的贡献者
<a href="https://github.com/svc-develop-team/so-vits-svc/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=svc-develop-team/so-vits-svc" />
</a>
