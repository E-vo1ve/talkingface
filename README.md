# Sadtalker配置文档

### 廖以涵 高天屹 组


## 一、通过Docker镜像使用Sadtalker（推荐）

#### 我们将项目封装成Docker镜像，镜像源地址 https://hub.docker.com/r/evo1ve/sadtalker

#### 如果您已经下载了Docker，可直接拉取使用

 `docker pull evo1ve/sadtalker`

#### Windows下，使用如下代码生成视频，其中文件目录需要替换成自己的路径

`docker run --gpus "all" --rm -v C:\Codes\data\processed:/SadTalker evo1ve/sadtalker --driven_audio /SadTalker/Jae-in_audio.wav --source_image /SadTalker/Jae-in_frame.jpg --expression_scale 1.0 --still --result_dir /SadTalker` 

#### Linux下，使用如下代码生成视频，其中文件目录需要替换成自己的路径

`docker run --gpus "all"--rm -v $(pwd):/host_dir sadtalker --driven_audio /host_dir/deyu.wav --source _image /host_dir/image.jpg --expression_scale 1.0l--still\.-result_dir /host_dir`  




## 二、本地部署Sadtalker（可选）

#### 网盘获取项目资源 https://pan.baidu.com/s/1QIG5t1WIO6s-zWgxToP-9g?pwd=uo5o

#### pip设置清华源

`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

#### 请先前往Anaconda官网 https://www.anaconda.com/ 安装Anaconda，启用Anaconda Prompt，进入命令行开始后开始本地部署：

#### 定位到你自定义保存的路径

`cd SadTalker`

#### 创建并激活环境

`conda create -n sadtalker python=3.8`  
`conda activate sadtalker`

#### 安装所需的库

`pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118`  
`conda install ffmpeg`  
`pip install -r requirements.txt`

#### 执行命令：`conda info`以找到你构建的虚拟环境的位置

#### 将源代码压缩包里面的：gfpgan\weights\GFPGANv1.4.pth 剪切到虚拟环境的 Lib\site-packages\gfpgan\weights 目录下

#### 本地部署完成

#### 生成视频的命令如下，其中文件目录需要替换成自己的路径

`python inference.py --driven_audio e:\temp\sadtalker\speech_0.wav --source_image e:\temp\sadtalker\1.png --result_dir e:\temp\sadtalker --still --preprocess full --enhancer gfpgan`



## 三、对生成视频进行PSNR SSIM FID指标评价

#### 我们将评价代码evaluate.py上传至项目下/evaluate中，请先定位至此目录下如果您选择使用Docker镜像进行部署，则需要创建一个虚拟环境，并在其中安装评估代码所需的库以运行evaluate.py。

#### 请先前往Anaconda官网 https://www.anaconda.com/ 安装Anaconda

#### 创建虚拟环境及安装所需库的命令如下：

#### 创建并激活环境 

`conda create -n sadtalker python=3.8`  

`conda activate sadtalker`

#### 安装 PyTorch 、TorchVision和Torchaudio

`pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118`

#### 安装其他依赖库 

`pip install -r requirements.txt`  

#### 确保evaluate.py在您当前的工作目录下，然后生成评价值，其中文件目录需要替换成自己的路径

`python evaluate.py --reference "C:\Codes\data\raw\videos\Obama.mp4" --generated "C:\Codes\data\processed\Obama\Obama_frame##Obama_audio.mp4" --output "C:\Codes\data\processed\Obama" --device cuda --batch_size 8`

#### 您可以使用/evaluate中的Obama1.mp4作为--generated video，使用Obama1_frame##Obama1_audio.mp4作为--reference进行评估代码的运行测试
