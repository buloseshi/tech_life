###  鸟枪换炮，换了台3080ti
#####  懒得每次都搜代码配置了，直接来一套  
Win+WSl2, pytorch1.12 + cuda11.5 + cudnn8.x  
1. win端安装必要的软件：  
anaconda、Ubuntu发行版（anyone）、pycharm(建议专业版，可以直接调用wsl的linux内核，社区版也可以，但win端只能使用命令行)。 安装好后，win打开cmd,输入  
`wsl --set-version <安装的分发版名字> 2`  
确保安装的是wsl2，wsl1不支持cuda
2. wsl安装miniconda：  
因为我们只需要conda包管理器就可以了，不用下载完整的anaconda,  
`https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`  
`sudo bash Miniconda3-latest-Linux-x86_64.sh`
3. 确保win端安装了nvidia驱动：
进入ubuntu发行版命令行，安装cuda  
`wget https://developer.download.nvidia.com/compute/cuda/11.5.0/local_installers/cuda_11.5.0_495.29.05_linux.run`  
`sudo sh cuda_11.5.0_495.29.05_linux.run  --override`  
如果报错八成是win端的驱动太老，直接下载win端Geforce更新到最新驱动即可。
4. cudnn不是必需的❗。cuda架构可以使GPU用来进行并行计算，cudnn是针对深度神经网络计算加速库，需要自行到nvidia下载，下载deb版本，然后打开下载目录  
WSL中，使用`cp 源路径 目的路径`，将cudnn的deb包复制到linux下，然后`sudo dpkg -i <cudnn_name>.deb`
5. wsl安装pytorch：  
设置pip清华源(可选)，自己用的预览版的官方源挺快的`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`
`pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu115`  
如果下载速度太慢，就使用：  
`pip3 install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple`
5. 验证cuda是否可用：  
在wsl终端, 进入python：  
`import torch`  
`print(torch.cuda.is_availble())`  
输出是’ture'就ok了
6. wsl端配置完就基本完事了，接下来打开win上的Pycharm,添加解释器，选择WSL，选择系统解释器，是miniconda的就没错了。
7. 如果需要安装win上面的torch和cuda, 直接下载cuda安装包，pytorch就行，复制5
