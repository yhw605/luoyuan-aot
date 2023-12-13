# luoyuan-aot

颜翰文的第二次遥感作业

邮箱：[hanwen_yan@outlook.com](mailto:hanwen_yan@outlook.com)或[10210533@njnu.edu.cn](mailto:10210533@njnu.edu.cn).

## 作业内容

本次作业研究了罗源闽光钢铁厂建立前后罗源县每年6月30日的AOD值与该钢铁厂建立的关系.

## 数据来源

本次作业使用了[MODIS L1B 1km数据](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/MOD021KM), [福建省地理数据库数据](https://bzdt.fjmap.net/widget/standardmap/result/result.html?resultid=702&yearver=%E5%BD%93%E5%89%8D%E7%89%88%E6%9C%AC)以及[ETOPO 2022 DEM高程数据](https://www.ncei.noaa.gov/products/etopo-global-relief-model).

## 文件目录说明

由于使用数据量较大, 因此无法将所有数据和中间文件放入仓库, 因此各目录下除/hdfs文件夹下的data-names.txt包括了一个所有数据的文件名.

## 所需依赖

本实验使用Python进行, 所需要的库包括gdal, ogr, osr, rasterio, geopandas, fiona, Py6S, scikit-learn, opencv, matplotlib, numpy.

## 代码运行

本文件夹整理较为仓促, 大部分无用代码没有删去

首先运行read_luoyuan_gdb2.ipynb, 得到ESRI Shapefile格式的罗源县的矢量数据和福建省县级行政区划的矢量数据.

之后运行geom\_corr2.ipynb对遥感影像进行几何校正和辐射定标. 该文件内先运行第一个代码块导入所需的Python库; 之后运行第二个代码块对遥感影像进行几何校正, 输出的文件见./hdf-output文件夹; 之后运行第三个代码块对影像辐射定标, 输出文件在./rad-corr文件夹.

下一步运行remove_cloud.ipynb对遥感影像进行云检测和去云. 先运行第一个代码块导入包, 之后运行第三个代码块进行云检测, 输出的文件在./cloud_mask文件夹下; 运行第五个代码块, 进行图像去云, 输出见./cloud-clip文件夹.

之后运行final.ipynb. 该文件的主要用途是构建6S模型, 训练SVM拟合并计算AOD. 2-6代码块构建了6S模型及查找表, 第7个代码块构建了线性回归模型, 第8个代码块构建了SVM回归模型并计算了模型的得分. 第9个代码块根据观测几何信息, 像元的辐亮度信息计算AOD值, 输出的文件在./aot550文件夹中.

运行raster\_shp\_clipped.ipynb文件, 对区域进行掩膜提取.

之后运行final2.ipynb. 这所做的是对AOD做高程的纠正, 和可视化各年度AOD的平均值和最大值. 但是由于后一步所用到的numpy数据文件stat2.npy在analysis.ipynb文件中. 因此在运行第一个代码块后运行analysis.ipynb文件, 该文件做了AOD在罗源县的空间分布可视化.
