#####  从0到1配置WSL2深度学习开发环境
Win+WSl2, pytorch1.12 + cuda11.5 + cudnn8.x  
*解释：wsl是windows提供的系统级虚拟机功能，无需安装vmware或ubuntu双系统，可直接在win中使用linux环境作为代码环境<br>
anaconda是一个非常流行的python包管理工具，可以方便的使用其环境安装所需的python包，cuda是nvidia开发的模型训练框架，将计算任务转移至nvidia显卡上运算，加快训练速度<br>
pytorch是深度学习领域最常用的python包，里面封装了大量常用的各种深度学习算法和函数，安装简单<br>
由于全新安装linux系统会需要很多与代码无关的问题，例如linux中的各种驱动问题，以及网络配置，包括后续代码debug，使用wsl可以规避这一系列问题，linux只作为代码运行环境，这样可以将注意力回到代码上，而不是处理各种操作系统的使用问题上<br>
1. win端安装必要的软件：  
anaconda、Ubuntu发行版（anyone）、pycharm(建议专业版，可以直接调用wsl的linux内核，社区版也可以，但win端只能使用命令行)。<br>
1.1 安装WSL2子系统作为运行环境<br>
   win管理员模式打开cmd/powershell,输入<br>`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`<br>
   启用Linux子系统组件<br>
   完成后再输入<br>`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`<br>
   启用windows虚拟化环境<br>
   win可直接在应用商店搜索ubuntu或者打开cmd/powershell输入<br> `wsl --install`默认安装ubuntu环境，可使用`wsl --install -d <ubuntupreview>`<br>
   安装ubuntu预览版，无需二次配置源<br>
  安装好后，win打开cmd,输入 <br> 
  `wsl --set-version <安装的分发版名字> 2` <br> 
  确保安装的是wsl2，wsl1不支持cuda<br>
  
3. wsl安装miniconda：<br>  
因为我们只需要conda包管理器就可以了，不用下载完整的anaconda,<br>
打开powershell,输入<br>`ubuntupreview.exe`<br>或者你自己安装的Ubuntu发行版的名字，打开ubuntu子系统，成功后会进入ubuntu命令控制行，后续操作即通过命令行在ubuntu子系统中完成<br>
输入`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`<br>下载miniconda3安装文件<br>
再输入
`sudo bash Miniconda3-latest-Linux-x86_64.sh`安装miniconda3<br>
安装完成后，命令行输入`conda activate`,成功即代表成功进入conda环境

5. 下一步端安装cuda，首先要确保win端安装好了nvidia驱动，使用Geforce experience更新到最新驱动即可：<br>
与上面一致，通过cmd/powershell进入ubuntu发行版命令行，安装cuda，在linux命令行输入  <br>
`wget https://developer.download.nvidia.com/compute/cuda/11.5.0/local_installers/cuda_11.5.0_495.29.05_linux.run` <br>
下载cuda11.5(截至编辑日最新版，可去nvdia官网查找最新版安装，一般环境11.5可以满足需求)<br>
`sudo sh cuda_11.5.0_495.29.05_linux.run  --override`<br>
安装cuda<br>
如果报错八成是win端的驱动太老，直接下载win端Geforce更新到最新驱动即可。<br>

7. cudnn不是必需的❗。cuda架构可以使GPU用来进行并行计算，而cudnn是针对深度神经网络计算加速库，需要自行到nvidia下载，下载deb版本，然后打开下载目录  
WSL中，使用`cp 源路径 目的路径`，将cudnn的deb包复制到linux下，然后`sudo dpkg -i <cudnn_name>.deb`<br>
不安装cudnn影响不大，但可以一次性配置好<br>

8. wsl安装pytorch：  
设置pip清华源(可选)，自己用的预览版的官方源挺快的,输入安装命令：<br>
`pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu115`
注意，cuda版本与pytorch版本具有对应关系，不同的cuda版本对应不同的pytorch版本，如不对应则无法使用nvidia显卡进行模型训练<br>
以上命令为cuda11.3与pytorch1.12,如需要其他版本可使用如下命令安装，更改后缀版本号即可，具体可在pytorch官网查询<br>
`pip install torch==1.12.0+cu116 torchvision==0.13.0+cu116 torchaudio==0.12.0 --extra-index-url https://download.pytorch.org/whl/cu116`<br>
如下载速度慢，可以设置pip国内源加快下载速度，命令如下：<br>
`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`<br>
之后再安装所需包，例如：<br>
`pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu115`<br>
可以直接使用官方源进行下载，即pip install xxx,如果官方源下载速度慢可考虑使用上述代码换源并下载。<br>
如果下载速度太慢，就使用：<br>  
`pip3 install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple`

5. 验证cuda是否可用：  <br>
在wsl终端, 进入python： <br> 
`import torch` <br> 
`print(torch.cuda.is_availble())`  <br>
输出是’ture'就ok了<br>

6. wsl端配置完就基本完事了，接下来打开win上的Pycharm,添加解释器，选择WSL，选择系统解释器，是miniconda的就没错了。

8. 如果需要安装win上面的torch和cuda, 直接在win端安装anaconda，下载cuda安装包，pytorch就行，从5开始
