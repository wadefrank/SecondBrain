# 环境配置

```bash
# 1.create env
conda create --name CenterNet python=3.6
conda activate CenterNet
pip install torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f <https://download.pytorch.org/whl/torch_stable.html>
conda install -c conda-forge cudatoolkit-dev=11.1.1 -y

# 2.install COCOAPI
mkdir CenterNet
cd CenterNet
git clone <https://github.com/cocodataset/cocoapi.git>
cd cocoapi/PythonAPI
pip install cython
make
python setup.py install --user
# 可能会出现报错，将matplotlib那一行注释掉，然后手动安装
pip install matplotlib

# 3.clone CenterNet
cd ../..
git clone <https://github.com/xingyizhou/CenterNet>
cd CenterNet
vim requirements.txt 
# 将opencv-python修改为opencv-python==4.5.5.64
pip install -r requirements.txt

# Compile deformable convolutional (from DCNv2)
cd src/lib/models/networks/
rm -rf DCNv2
git clone -b pytorch_1.9 <https://github.com/lbin/DCNv2.git> DCNv2
cd DCNv2
./make.sh

```

# 参考资料

[扔掉anchor！真正的CenterNet——Objects as Points论文解读](https://zhuanlan.zhihu.com/p/66048276)