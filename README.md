# OSTSI-CBFNet
Ocean Subsurface Temperature and Salinity Inversion based on CNN-BiLSTM Fusion Network

本项目提出了一种OSTSI-CBFNet模型，融合CNN-BiLSTM与双注意力机制，仅用海面遥感数据即可高精度反演北太平洋30–1000 m共15层的温盐场。项目模型训练代码已开源，欢迎复用与改进。
研究区域：北太平洋（120°E–90°W，0–60°N）。
数据：
	变量	   来源	    空间分辨率	    时间分辨率	
	SSH	    AVISO      	0.25°       	日	
	SSS	  SMAP/SMOS 	  0.125°     	日/月	
	SST	  UKMO OSTIA	  0.05°	        日	
	SSW     CCMP 	      0.125°	     6 h/月	
3D T/S  	CMEMS     GLORYS12V1	  0.25°	月	
数据预处理：裁剪北太平洋区域，双线性重采样到0.25°，统一时间分辨率为月均。无效网格点剔除；进行标准化。SST/SSS减去不同深度层的10年平均值差值(SST/SSS-avg[Sdepths=i/Tdepths=i])，生成次表层近似AST/ASS。3D T/S选15层（30–1000 m）。
2011-2020年的数据作为训练集和验证集（随机选取30%作为验证集，70%作为训练集），2021年的数据作为测试集。
模型：模型设计了时空双分支，CNN-CBAM提取11×11窗口的空间特征，BiLSTM-Attention提取前10个月的时间特征，融合后输出当前月份的温盐反演结果。

<img width="3143" height="1250" alt="image" src="https://github.com/user-attachments/assets/03c9a993-3bed-4e56-9116-5c9cda5bc717" />

创新：首次CNN-BiLSTM+双注意力用于温盐反演；引入次表层近似变量AST/ASS增强垂向约束，无需额外观测即提升了50–200 m的反演精度。
结果：2021全年15层平均R²=0.985，RMSE≤0.394 ℃/0.052 psu，优于CNN、BiLSTM、CNN-CBAM、Transformer、MTSAN等模型，且时空-垂向误差稳定。
发表文章：Mu J, Yang J, Wang C, et al. OSTSI-CBFNet: ocean subsurface temperature and salinity inversion based on CNN-BiLSTM fusion network with attention mechanism[J]. Journal of Applied Remote Sensing, 2024, 18(3): 038507-038507.
