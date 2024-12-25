# EfficientSCI++数据处理代码学习笔记
一、cacti-datasets-pipelines构造数据处理的流水线
1.generation_meas.py生成测量值
（1）灰度测量值生成函数class GenerationGrayMeas:
传入一组视频帧，和mask相乘生成在相加得到measurement img，并且读入数据时归一化除以255。
（2）彩色bayer测量值生成函数GenerationBayerMeas:
三通道图像数据首先和rgb2raw权重矩阵相乘，随后沿着通道维度相加变为一个二维数据Y，每一帧的Y再和mask相乘、相加得到测量数据。
 ![image](https://github.com/user-attachments/assets/c1dd3c01-54bd-42b3-bd6d-7b9a72dd6e36)
 
2.builder.py：构建数据处理的流水线实例注册表，底层函数都是调用build_from_cfg根据config文件构造一个实例，可能是数据集也可能是数据处理流水线（数据读取、增强）
 
3.compose.py：将多个数据处理步骤组合成流水线，依次对输入数据进行处理 
4.augmentation.py：具体实现图像的变尺寸、随机旋转、翻转（水平、垂直）、随机裁剪等一系列操作。 
二、cacti-datasets其他操作 
（1）birnatdavis.py：用于加载.mat格式GT和measurement数据的自定义数据集类。 
（2）builder.py-build_datasets()返回一个DAVIS数据集 
（3）davis_bayer.py： 
生成rgb2raw矩阵。 
解析文件夹，使用cv2.read读取图片，所有图片经过pipeline处理。 
给定index返回这一组八帧图像的gt和measurement 
（4）davis.py和上述文件作用差不多，但是省略了生成rgb2raw矩阵。  
（5）real_data.py从mat文件读入，之间经过归一化、维度转换输出measurement数据  
（6）six_gray_sim_data.py从mat文件读入，经过处理返回数据的GT和measurement图像。 

