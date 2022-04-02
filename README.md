# Umi-OCR 批量图片转文字工具

软件用途：批量对本地图片文件进行离线OCR文字识别。win10 x64平台。

> 本软件是本地图片文件处理工具，没有实时屏幕截图识别的功能。

![](https://tupian.li/images/2022/03/30/image.png)

![](https://tupian.li/images/2022/03/30/win-2-2.png)

## 下载

[Umi-OCR 批量图片转文字 v1.1](https://github.com/hiroi-sora/Umi-OCR/releases/tag/v1.1)

## 简介

本软件用于批量导入本地图片，识别图片中的文本，输出到软件面板或本地txt文件。
除了能批量识别普通图片，本软件还有**忽略指定区域**的特殊功能。

> 类似含水印的视频截图、含有UI/按钮的游戏截图等，往往只需要提取字幕区域的文本，而避免提取到水印和UI文本。本软件可设置忽略某些区域内的文字，来实现这一目的。尤其是，特别适合用于批量提取Galgame截图中的台词。
> 
> 当有大量的影视和游戏截图需要整理归档，或者想翻找包含某一段台词/字幕的截图；将这些图片提取出文字、然后Ctrl+F是一个很有效的方法。这是开发本软件的初衷。

本软件使用离线OCR模块 [PaddleOCR-json 图片转文字程序](https://github.com/hiroi-sora/PaddleOCR-json) ，使用过程中无需联网。支持更换 [Paddle官方模型](https://gitee.com/paddlepaddle/PaddleOCR#pp-ocr%E7%B3%BB%E5%88%97%E6%A8%A1%E5%9E%8B%E5%88%97%E8%A1%A8%E6%9B%B4%E6%96%B0%E4%B8%AD)（v2.x版本）或自己训练的模型，支持修改PaddleOCR参数。通过添加不同的语言模型，软件可识别多国语言。

## 简单使用

### 准备

下载压缩包并解压，主程序 `Umi-OCR 批量图片转文字.exe` 与识别器模块文件夹 `PaddleOCR-json` 需置于同一目录下。

### 快速开始

![](https://tupian.li/images/2022/03/29/rm-2.jpg)

1. 打开主程序，将任意 **图片/文件夹** 拖入窗口中的白色背景表格区域，或点击左上方的 **浏览** 选择图片。
   - 若拖入文件夹，则加载文件夹下所有 **符合后缀（见后）** 的图片文件。
2. 点击右上方 **开始任务** ，等待进度条走完。
   - 任务进行中，可随时点击 **终止任务**（原开始任务按钮）来停止，但下次开始时依然会从头开始。
3. 点击 **识别内容** 选项卡查看输出文字，或者前往 **第一张图片的目录** 查看识别结果txt文件。
   - 识别内容选项卡中，可一键将全部文本 **复制到剪贴板** 。


### 基础设置

点击 **设置** 选项卡，配置参数。目前，所有设置项不会保存，下次打开软件将重置。

![](https://tupian.li/images/2022/03/29/win-2-0-1.png)

#### 本地输出文件：
- 将 **启用识别内容写入txt文件** 取消勾选后，不会再生成本地文件，只能在 **识别内容** 选项卡中查看输出信息。
- **输出目录** 和 **输出文件名** 设置生成的文件的位置和名称。当拖入第一张图片且这两项设置为空时，自动设置输出路径为第一张图片的父目录，输出文件名为 `[转文字]_{父目录}.txt`。除非要自定目录和名称，否则这两项默认留空即可。
- 软件 **处理列表** 标签页的 **清空表格** 按钮，除了会清空已导入的图片列表，还会清空 **输出目录** 和 **输出文件名** 设置。这样下次拖入新图片时，就能在新的位置存放输出文件。

#### 忽略图片中某些区域内的文字：
- 点击 **添加忽略区域** 展开配置忽略区的新窗口。具体配置方式见后。
- 点击 **清空所有区域** 清空已配置的所有忽略区域参数。
- 右方文字提示当前忽略区域的 **适用分辨率** 。

#### 识别器设置：

- **识别器路径** 配置当前使用的识别器exe程序。
- **图片后缀** 配置软件允许载入的图片后缀，不同后缀以空格分隔，必须全为小写。**正常情况下无需改动**。
  - 如果你有必要添加新的图片后缀，要保证该图片同时满足c++模块的`PaddleOCR`和python的`PIL`均可识别。比如 **.gif** 图片，虽然`PIL`可以识别，但`PaddleOCR`无法识别，载入gif文件会导致软件任务失败，因此不允许载入 **.gif** 。
  - 不在许可后缀范围内的文件，拖入软件也不会被载入。目前默认的图片后缀为：`.jpg .jpeg .png .webp`。实测支持更多后缀：` .bmp .jfif .tif .tiff`，将在下个版本写入发行版。

## 忽略区域功能

忽略区域是本软件特色功能。可用于批量识别视频截图时排除右上角水印的干扰，批量识别游戏截图时排除UI与按钮的干扰，让识别结果只留下干净的台词文本。

“忽略区域”是指图片上指定位置与大小的矩形区域，完全处于这些区域内的文字块，将被排除。

- 点击 **设置** 选项卡中的 **添加忽略区域** ，进入忽略区域选择窗口。
- 将任意图片 **拖入** 该窗口，可预览该图片。将新图片拖入窗口可切换预览，但已绘制的忽略区域不会消失；可切换不同图片来仔细调整忽略区域。
- 绘制 **忽略区域** ：拖入图片后，点击选中左起第一按钮 **+忽略区域 1** ，然后在图片上按住左键拖拽，绘制矩形区域。可 **撤销** 步骤。
- 绘制完后，点击 **完成** 返回软件主窗口。若不想应用此次绘制，则右上角X，取消。

简单案例见下。

#### 简单排除视频截图中的水印：

1. 打开忽略区域设置窗口，拖入任一张截图。
    稍等约1秒，面板上会显示出图片，识别到的文字区域会被虚线框起来。发现右上角的水印也被识别到了。
![](https://tupian.li/images/2022/03/30/image04bea2a47232088b.png)
2. 点击选择 **+忽略区域 1** ，鼠标按住，绘制矩形完全包裹住水印区域，范围可以大一些。
![](https://tupian.li/images/2022/03/30/image4a49b65bbd22c4a6.png)
3. 点击 **完成** 。返回主窗口， **开始任务** 。

#### 排除游戏截图中的两种UI：

- 假设有一组游戏截图，主要分为两类图片，这两类图片的文字位置和UI位置不太相同：
![](https://tupian.li/images/2022/03/30/image1.png)
  - A类（上图左）为对话模式，字数少，要保留的台词文本在画面下方，要排除的UI分布于底端。
  - B类（上图右）为历史文本模式，字数多，从上到下都有要保留的文本（与A类UI位置有重合），要排除的UI分布在两侧。
1. 拖入一张A类图片。选择 **+忽略区域 1** ，绘制矩形包裹住要排除的 **底端UI** 。
![](https://tupian.li/images/2022/03/30/image2ad97ff898e39d82f.png)
2. 拖入一张B类图片。选择 **+识别区域** ，绘制矩形包裹住 **小部分要保留的文本** 。注意只要该区域内含有任意保留文本即可，不需要画得很大，不需要包裹住所有保留文本；不能与A类图中 **可能存在的任何文本** 重合。
![](https://tupian.li/images/2022/03/30/image3.png)
1. 然后选择 **+忽略区域 2** ，绘制矩形包裹住B类图要排除的 **两侧UI** 。
![](https://tupian.li/images/2022/03/30/image4.png)
1. 点击 **完成** 。返回主窗口， **开始任务** 。

#### 忽略区域功能说明：

- **忽略区域1** ：正常情况下，处于 **忽略区域1** 内的文字 **不会** 输出。
- **识别区域** ：当识别区域内存在文本时，**忽略区域1失效** ；即处于忽略区域1内的文字也 **会** 被输出。
- **忽略区域2** ：当 **忽略区域1失效** 时，忽略区域2才生效；即处于区域1内的文字 **会** 输出、区域2内的文字 **不会** 输出。
  
    | 识别区域     | 忽略区域1 | 忽略区域2 |
    | ------------ | --------- | --------- |
    | × 不存在文字 | √ 生效    | × 失效    |
    | √ 存在文字   | × 失效    | √ 生效    |
- “忽略区域配置”只针对一种分辨率生效。假如配置的分辨率是1920x1080，那么批量识别图片时，只有符合1920x1080的图片才会排除干扰文本。
- 拖入预览的图片必须分辨率相同。假如先拖入1920x1080的图片，再拖入其它分辨率的图片；软件会弹窗警告。只有点击 **清空** 删除当前已配置的忽略区域，才能拖入其他分辨率图片，并应用此分辨率。


## 切换模型库和语言

#### 切换日文模式

软件自带日文识别库，将 **识别器路径** 修改为 `PaddleOCR-json\PaddleOCR_json_jp.exe` 即可。
![](https://tupian.li/images/2022/03/29/rm-6.png)

#### 载入新语言

以法文为例：

1. 前往 [PP-OCR系列 多语言识别模型列表](https://gitee.com/paddlepaddle/PaddleOCR/blob/release/2.4/doc/doc_ch/models_list.md#23-%E5%A4%9A%E8%AF%AD%E8%A8%80%E8%AF%86%E5%88%AB%E6%A8%A1%E5%9E%8B%E6%9B%B4%E5%A4%9A%E8%AF%AD%E8%A8%80%E6%8C%81%E7%BB%AD%E6%9B%B4%E6%96%B0%E4%B8%AD) 下载对应的 **推理模型**`french_mobile_v2.0_rec_infer.tar` 和 **字典文件**`french_dict.txt`。
2. 在`PaddleOCR-json`目录下创建文件夹`rec_fr`，将解压后的三个模型文件放进去。字典文件可直接放在目录下。
3. 复制一份识别器`PaddleOCR_json.exe`，命名为`PaddleOCR_json_fr.exe`
4. 复制一份配置单`PaddleOCR_json_config.txt`，命名为`PaddleOCR_json_fr_config.txt`
5. 打开配置单`PaddleOCR_json_fr_config.txt`，将`# rec config`相关的两个配置项改为：
    ```sh
    # rec config
    rec_model_dir  rec_fr
    char_list_file french_dict.txt
    ```
6. 保存文件，打开软件，将 **识别器路径** 改为 `PaddleOCR-json\PaddleOCR_json_fr.exe`。

#### 切换模型库

软件附带的模型可能过时，可以重新下载PaddleOCR的最新官方模型，享受更精准的识别质量。或者使用自己训练的模型。

1. 下载模型
 - 前往[PaddleOCR](https://gitee.com/paddlepaddle/PaddleOCR#pp-ocr%E7%B3%BB%E5%88%97%E6%A8%A1%E5%9E%8B%E5%88%97%E8%A1%A8%E6%9B%B4%E6%96%B0%E4%B8%AD)下载一组推理模型（非训练模型）。**中英文超轻量PP-OCRv2模型** 体积小、速度快，**中英文通用PP-OCR server模型** 体积大、精度高。一般来说，轻量模型的精度已经非常不错，无需使用标准模型。
 - 注意只能使用v2.x版模型，不能使用github上[PaddlePaddle/PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)的v1.1版。v1版模型每个压缩包内有model和params两个文件，v2版则是每组三个文件；不能混用。

2. 放置模型
- 将下载下来的方向分类器（如`ch_ppocr_mobile_v2.0_cls_infer.tar`）、检测模型（如`ch_PP-OCRv2_det_infer.tar`）、识别模型（如`ch_PP-OCRv2_rec_infer.tar`）解压，将文件分别放到对应文件夹 `cls、det、rec`。
- 打开PaddleOcr_json.exe。若无报错，则模型文件已正确加载。“Active code page: 65001”是正常现象。

3. 调整配置
- `[exe名称]_config.txt`是全局配置文件，可设置模型位置、识别参数等。调整它也许能获得更高的识别精度和效率。具体参考[官方文档](https://gitee.com/paddlepaddle/PaddleOCR/blob/release/2.4/doc/doc_ch/config.md#2-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%8F%82%E6%95%B0%E4%BB%8B%E7%BB%8D)。
- 如果修改了exe名称，也需要同步修改配置文件名的前缀。

## 测试报告

用它跑完了我~~珍藏的7000多张~~各类截图文件，效果十分满意。
跟半年前使用[百度云在线OCR接口](https://cloud.baidu.com/doc/OCR/index.html)（标准文字识别）跑的对比：

- Umi-OCR使用轻量模型时速度很快，平均识别耗时<1s（使用笔记本低压u）。在线OCR受限于网络，耗时>1s。
- Umi-OCR对符号的正确识别率更高，比如能正确识别中文逗号。在线OCR的结果中，很大一部分中文逗号被识别为英文。
- 对于文字内容，Umi-OCR与在线OCR的准确度几乎没有差异。都能满足所需。
- 排除UI与水印干扰，是Umi-OCR的独有技能。理论上在线OCR的高精度识别接口也能做到，~~不过那玩意死贵死贵~~。

## 开发说明

- 本软件工作流程是python调用c++编译的识别器exe程序，识别器exe再加载模型文件和必要的dll链接库，完成图片识别工作。因此可切换不同exe识别器和模型文件，实现切换多国语言的识别。而且，c++识别器比python版PaddleOCR具有更高的性能。
- 启动任务时，运行tk的主线程创建了一个新线程和一个事件循环。耗时量大的OCR任务在新线程中执行，对tk界面的修改由事件循环提交到主线程中执行。~~大概是这么回事？反正能跑起来就对了~~。无论任务跑得多满，界面都不会卡，~~除非实在顶不住~~。
- `PaddleOCR_json.exe`接收输入一个本地图片路径，以json格式字符串输出这张图片的识别结果，如此循环往复。具体见 [PaddleOCR-json 图片转文字程序](https://github.com/hiroi-sora/PaddleOCR-json#paddleocr-json-%E5%9B%BE%E7%89%87%E8%BD%AC%E6%96%87%E5%AD%97%E7%A8%8B%E5%BA%8F)
- 使用`pyinstaller`打包，参数为
  ```pyinstaller -F -w -i icon/icon.ico -n "Umi-OCR 批量图片转文字" main.py```

## TODO

- [ ] 输出内容可选为markdown风格并嵌入图片路径，这样可以用在输出文件里浏览图片和文本。
- [ ] 设置项能保存。

## 更新日志

`v1.1.1` `2022.3.30` 改正了漏洞：退出 [忽略区域窗口] 时，OCR子进程未关闭。

`v1.1.0` `2022.3.30` 添加新功能：[忽略区域窗口] 以虚线框 展示识别出的文字块。

`v1.0` `2022.3.28` “梦开始的地方”
