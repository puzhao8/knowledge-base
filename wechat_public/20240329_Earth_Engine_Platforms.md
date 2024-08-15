# 阿里地球引擎“出道”，地球引擎平台一览

随着对地观测技术的发展，基于卫星平台的地球观测已成为全球大数据的主要来源之一。仅2019一年，美国航空航天局（NASA） Landsat-7, Landsat-8, MODIS和欧洲空间局（ESA） Sentinel-1/-2/-3 卫星任务就获取了近5PB （$5 \times 10^6$ GB）的遥感数据。

近年来，各国政府和航天部门均采取了更为开放的地球观测数据免费分发政策，但如此海量的卫星遥感数据远远超出了个人计算机的内存、存储和处理能力，迫使用户只能对小规模、局部区域的数据进行研究和分析，难以推广到大尺度、长时序。而全球尺度、长时序、多源卫星遥感数据的分析和建模对研究全球环境变化、城市和人口变迁、生态环境保护和应对气候变化等意义重大，因而“地球大数据云计算平台”应运而生。

“地球大数据云计算平台”提供地球观测大数据云上管理、存储和获取、服务器端计算处理及数据、处理抽象等功能和服务，而无需下载大量数据到本地，代表性平台如谷歌地球引擎（Google Earth Engine）, 微软星球计算机（Microsoft Planetary Computer）和开放数据立方体（Open Data Cube）。

国内方面，2020年，航天宏图发布了与谷歌地球引擎类似的PIE Engine，提供了国内高分系列部分卫星数据；2022至今，由阿里云达摩院视觉技术实验室开发的AI Earth平台也日趋完善，实现了类谷歌地球引擎Python API功能函数，并开发了多种人工智能组件。“地球大数据云计算平台”已成为地球科学研究和应用的重要基础设施，国内相关云计算平台的发展意义重大，可广泛支持抗灾减灾、山林监测、水资源管理、碳储量估计、农作物长势监测和产量估计等。

## 全球主要地球引擎平台

| <div style="width:100px">地球引擎平台</div>        | <div style="width:70px">发布年份</div>  |         <div style="width:100px">开发国家</div>  |<div style="width:300px">官方网址</div>  |
| :--------------------------- | :--: | -------: |:-------|
| Google Earth Engine (GEE)     |  2010  |     美国 |https://earthengine.google.com/ |
| Open Data Cube (ODC) |  2019  | 澳大利亚 |https://www.opendatacube.org/|
| PIE Engine (PIE) |  2020  | 中国 |https://engine.piesat.cn/engine-studio/|
| Microsoft Planetary Computer (MPC)   |  2021  | 美国 |https://planetarycomputer.microsoft.com/|
| Earth Observation Data Center (EODC) |  2021  | 欧盟 |https://eodc.eu/|
| OpenEO Platform|  2021  | 欧空局 | https://openeo.cloud/|
| Analytical Insight of Earth (AI Earth, AIE) |  2022  | 中国 |https://engine-aiearth.aliyun.com/|


### 谷歌地球引擎（GEE）
GEE是一个赋能大规模地球科学计算和地理空间数据可视化的云平台，由谷歌研发，发布于2010年。如图1所示，该平台基于谷歌的大规模计算集群（Borg）、分布式数据库(Bigtable和Spanner)和分布式文件系统（Colossus）以及并行框架（FlumeJava）。

![GEE JavaScript 代码编辑器](https://files.mdnice.com/user/61769/f139a7bf-ec4a-4c70-9db3-9513a630663e.png)


GEE提供JavaScript API 和 Python API,并提供Data Catalog，其囊括了海量地理空间数据，包括卫星遥感数据、环境气候数据、地物分类数据、社会经济人口数据，以及地形地貌数据等，目前仍为数据资源最为丰富的地球引擎平台。除官方数据资源外，社区数据资源也较为丰富，如https://gee-community-catalog.org/，GEE上数据和代码均便于分享。

![](https://files.mdnice.com/user/61769/2c3bf791-4a0d-4c69-9ccd-85a491e505c4.png)

![图1. 谷歌地球引擎架构图](https://files.mdnice.com/user/61769/c53e6e0e-e084-4991-b709-2f53eabbf677.png)


如图2所示，通过GEE的代码编辑器，用户编写代码发送计算请求到谷歌服务器进行计算，然后在客户浏览器端显示返回的计算结果。用户无需下载数据到本地计算机就可以实现空时分析。

![图2. GEE服务器端计算](https://files.mdnice.com/user/61769/86725d03-c126-40fb-9707-a05250173869.png)


GEE采用ee.Image来抽象raster数据，每个ee.Image包含获取时间、地理位置等信息，一个Image可以包含一个或多个波段，每个波段包含name, datatype, scale和projection等, Image在空间或者时间上的集合用ee.ImageCollection表示。GEE采用ee.Feature表示vector数据及其属性，如geometry（包括点、线或者多边形），用ee.FeatureCollection表示一组相关的ee.Feature。并提供了预定义的函数对raster或vector进行数据操作，如排序、按时间过滤以及可视化等。

``` javascript
// Compute Normalized Difference Vegetation Index over MOD09GA product.
// NDVI = (NIR - RED) / (NIR + RED), where
// RED is sur_refl_b01, 620-670nm
// NIR is sur_refl_b02, 841-876nm

// Load a MODIS image.
var img = ee.Image('MODIS/006/MOD09GA/2022_07_01');

// Use the normalizedDifference(A, B) to compute (A - B) / (A + B)
var ndvi = img.normalizedDifference(['sur_refl_b02', 'sur_refl_b01']);

// Make a palette: a list of hex strings.
var palette = ['FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718',
               '74A901', '66A000', '529400', '3E8601', '207401', '056201',
               '004C00', '023B01', '012E01', '011D01', '011301'];

// Display the input image and the NDVI derived from it.
Map.addLayer(img.select(['sur_refl_b01', 'sur_refl_b04', 'sur_refl_b03']),
         {gain: [0.1, 0.1, 0.1]}, 'MODIS bands 1/4/3');
Map.addLayer(ndvi, {min: 0, max: 1, palette: palette}, 'NDVI');
```

以上几行Javascript代码就可以可视化MODIS全球尺度卫星影像和计算NDVI植被指数，如图3和图4所示。

![图3. 全球尺度MODIS 2022年7月1日影像](https://files.mdnice.com/user/61769/592028f0-d82d-4780-a137-8043f0e42e06.png)

![图4. 全球尺度MODIS 2022年7月1日NDVI](https://files.mdnice.com/user/61769/0a3e281b-1f9e-4a97-9594-b22f0a807802.png)

此外，GEE还提供了与人工智能（AI）技术集成的可能性，可以直接在GEE调用部署在Google AI Platform 或者 Vertet AI 的AI模型。当训练AI模型时，用户往往只需要准备训练数据（包含验证数据），根据验证集选定最优参数，模型训练完成后即可部署到AI Platform 或者 Vertex AI, 供GEE进行调用。如果用户计划将该模型应用到全球尺度上，可以直接在GEE中调用已部署的模型进行推理，而无需导出全球尺度测试数据集（或者推理数据集），但用户需为模型的推理所耗费的计算资源付费。

``` python
# Call AI model from AI Platform or Vertex AI in GEE
import ee
ee.Initialize()

model = ee.Model.fromAiPlatformPredictor(
    projectName = config.PROJECT,
    modelName = MODEL_NAME,
    version = VERSION_NAME,
    inputTileSize = [144, 144],
    inputOverlapSize = [8, 8],
    proj = ee.Projection('EPSG:4326').atScale(30),
    fixInputProj = True,
    outputBands = {'impervious': {
        'type': ee.PixelType.float()
      }
    }
)

# Do prediction on data in GEE with the deployed model
predictions = model.predictImage(image.toArray())
```


### 微软星球计算机（MPC）
MPC是一个高度模块化的开源地球引擎云平台，由微软研发，与多个开源软件集成度较高，该项目github主页：https://github.com/microsoft/PlanetaryComputer。该平台使用开源项目STAC（Spatio-Temporal Asset Catalog， 官方网址:https://stacspec.org/en）对海量数据进行刻画和描述，以便于数据的索引、集成和无缝使用。

![](https://files.mdnice.com/user/61769/f640006d-2272-4b7a-a9aa-bd9d681f8a8c.png)

![](https://files.mdnice.com/user/61769/134c3118-8337-41c1-8c7e-a410400b0187.png)

MPC包括Data Catalog, APIs, Hub, Applications四个组件，Data Catalog包含托管在Azure云上的PB级数据，APIs利用STAC产生的标准化metadata来促进对感兴趣数据和区域的查询，Hub是一个通过JupyterHub部署的工作台，预装了一些地理空间分析工具，方便用户使用，但是Hub里计算资源是收费的。Appications是由第三方基于MPC构建的应用，如https://carbonplan.org/research/forest-risks。

![](https://files.mdnice.com/user/61769/987a0e2f-3d80-44a2-bab7-e90ad97482fc.png)

MPC针对Raster主要采用Cloud optimized GeoTIFFs (COGs) 格式进行存储，也支持Cloud optimized NetCDFs, HDFs, and Zarr格式。更好地对数据分块和通过metadata对数据块进行描述，才能便于APIs的索引、查询和将最相关的数据流（stream）传输给计算单元，而不是像传统方式那样下载整个数据，再裁切出所需要的区域。MPC数据获取和使用是免费的，用户只需要stream数据到本地进行计算即可，无需付费。


![MPC Landsat-9 影像](https://files.mdnice.com/user/61769/b36ce7d1-70be-46e0-aff4-4496ea7cb03a.png)

``` python
import pystac
import planetary_computer
import rioxarray

item_url = "https://planetarycomputer.microsoft.com/api/stac/v1/collections/landsat-c2-l2/items/LC09_L2SP_126037_20240320_02_T1"

# Load the individual item metadata and sign the assets
item = pystac.Item.from_file(item_url)

signed_item = planetary_computer.sign(item)

# Open one of the data assets (other asset keys to use: 'red', 'blue', 'drad', 'emis', 'emsd', 'trad', 'urad', 'atran', 'cdist', 'green', 'nir08', 'lwir11', 'swir16', 'swir22', 'coastal', 'qa_pixel', 'qa_radsat', 'qa_aerosol')
asset_href = signed_item.assets["qa"].href
ds = rioxarray.open_rasterio(asset_href)
ds

```



### Open Data Cube (ODC)
ODC 曾用名 Australian Geoscience Data Cube (AGDC), 当前由Analytical Mechanics Associates (AMA), The Committee on Earth
Observation Satellites (CEOS), The Commonwealth Scientific and Industrial Research Organization
(CSIRO), Geoscience Australia (GA) and United States Geological Survey (USGS)支持。ODC是一个开源框架，采用Product 和 Dataset 的概念来抽象数据。Products是共享相同测量或者一些metadata的Dataset集合，而 Dataset 表示最小可独立描述和管理的数据。

![](https://files.mdnice.com/user/61769/94b461c8-6bf9-4ce7-b573-66d3b41fffce.png)

![](https://files.mdnice.com/user/61769/bc87547f-aa17-4833-88bd-e0de8d8ce561.png)


![ODC架构图](https://files.mdnice.com/user/61769/d1079435-6bcd-49c2-8ff3-2244ab632c2c.png)



### OpenEO Platform
OpenEO平台在资源层集成了ONDA, CREODIAS, EODC, Proba-V，Planet和GEE等，在后端层集成了European Data Cube, Sentinel Hub, DIAS, EODC, 和 PROBA-V。

![](https://files.mdnice.com/user/61769/8fc3cc4c-cf81-49ca-8c72-8e5b9fa3409a.png)

![openEO平台概览](https://files.mdnice.com/user/61769/b0c2c405-f681-49cf-ac9e-702243928d9b.png)


### 遥感计算云服务（PIE-Engine Studio）

![](https://files.mdnice.com/user/61769/1b4ab1e4-9aca-4f47-b94d-b7cd53b67d83.png)

![](https://files.mdnice.com/user/61769/0e1ada2e-d439-45e1-a297-99aabb05a275.png)

PIE-Engine Studio是一个类GEE平台，提供Javascript代码编辑器和Python API，通过结合海量卫星遥感影像、地理要素数据等空间数据，使得用户基于平台可以在任意尺度上研究算法模型并采取交互式编程验证，实现快速探索地表特征，发现变化和趋势。PIE-Engine Studio为大规模的地理数据分析和科学研究提供了免费、灵活和弹性的计算服务。但PIE Engine的Javascript编程平台缩放图层时响应速度不太理想。

### 阿里云地球引擎（AI Earth）
阿里云地球引擎除实现了类谷歌地球引擎Python API平台基本函数功能外，还包含以下重要特性：
- 实现了map.addLayer等函数以可视化地理数据或计算结果
- 提供了工具箱模式，支持以用户交互界面（GUI）实现数据选择和部分运算
- 提供了多种AI智能遥感解译工具和模型（如Segment Anything大视觉模型）,可供工具箱模式（Web-UI）和开发者模式(代码编辑器)调用，并允许用户使用自有训练数据进行模型微调，支持部署第三方AI模型
- 规模化能力强，自动按需申请计算资源
- 支持并行和分布式计算
- 数据源方面，包含了国内部分卫星遥感数据，如高分卫星等
- 采用有向无环图（Directed Acyclic Graph）来描述用户定义的计算过程
- 支持InSAR处理方面，有待进一步确认


![](https://files.mdnice.com/user/61769/d5336a39-b7b3-4164-aee6-2c47c3070b54.png)



![阿里云地球引擎架构图](https://files.mdnice.com/user/61769/dd6aef7f-3cf1-4cf8-9e0f-d49278e25a55.png)


![工具箱模式](https://files.mdnice.com/user/61769/2ec501b6-e6f9-4485-874e-7b84b4568d7b.png)



![AIE开发者模式](https://files.mdnice.com/user/61769/ef5eb874-2c3c-4d0b-b389-2903166ea2c3.png)

``` python
import aie
aie.Authenticate()
aie.Initialize()

# 指定需要检索的区域
feature_collection = aie.FeatureCollection('China_Province') \
                        .filter(aie.Filter.eq('province', '浙江省'))

geometry = feature_collection.geometry()

# 指定检索数据集，可设置检索的空间和时间范围，以及属性过滤条件（如传感器成像模式等）
image = aie.ImageCollection('SENTINEL_GRD') \
             .filterBounds(geometry) \
             .filterDate('2021-01-01', '2021-12-31') \
             .filter(aie.Filter.eq('sar:instrument_mode', 'IW')) \
             .limit(100).mosaic()
             
# 进行sar水体模型推理
model = aie.Model('sar_water')
predict_image = model.predict(image.select(['VV']))         
```




向地球引擎云平台研发人员们致敬！

![AI Earth 地球科学云平台开发人员](https://files.mdnice.com/user/61769/45e83bc0-0520-42ba-b927-e0e903195541.png)

- Xu Hao, Man Yuanbin, Yang Mingyang , Wu Jichao, Zhang Qi, and Wang Jing （论文[3]作者列表）
- Xia Hang, Song Ci, Zhang Hualong, Zhang Diao, Yu Quan, Guan Lijun, Zhu Yixuan, Xu Bin, Chen Mingyang, Shen Linlin, Luo Hao, Gong Yuan, Li Dongyang, Liu Shang, Guo Tingting, Chen Qiang, Zhang Mengting, Xue Tengfei, Hu Duoduo (团队其他成员).


论文[3]致谢部分节选：
... we would like to thank the providers of the hundreds of public datasets in AI Earth; in particular, NASA, USGS, NOAA, and ESA, whose enlightened open data policies and practices are responsible for the bulk of the data in our catalog.
（我们特别感谢AI Earth大量公开数据提供者，特别是美国航空航天局，美国地质勘探局，美国国家海洋和大气管理局和欧洲空间局，其开放数据政策才使得我们的大规模遥感数据库和平台得以建立。）


## 地球引擎平台对比

下表从数据抽象、处理抽象、物理抽象、可重复性、规模化、可扩展性、计算收费、数据下载和编程语言支持九个方面比较了五个代表性地球引擎云平台，包括GEE，AIE, MPC, ODC和OpenEO.

| <div style="width:100px">平台</div>       |  <div style="width:160px">谷歌地球引擎(GEE)</div> |         <div style="width:160px">阿里云地球引擎(AIE)</div> | <div style="width:160px">微软星球计算机(MPC)</div> |<div style="width:160px">开放数据立方体(ODC)</div> |<div style="width:160px">OpenEO</div> |
| :--------- | ----------------: | -----------: | -----------: |-----------: |-----------: |
| 数据抽象     |  Image, ImageCollection, Feature and FeatureCollection  |     Image, ImageCollection, Feature and FeatureCollection | STAC item & item collection |Product & Dataset |Collection & Granule |
| 处理抽象 |  Predefined pixel-wise functions  | Predefined pixel-wise functions | Xarray & Dask | Xarray & celery |User-defined functions, process, graphs & jobs |
| 物理抽象   |  storage & process  |   storage & process | storage & process |storage only |storage & process |
| 科学可重复性 |  medium (data & scripts links)  | medium (data name & scripts) | medium (data links & scripts) |low |low |
| 处理规模化 | 高(自动并行)  | 高（自动并行） | 中（dask tutorial） |中 (Celery) | -|
| 可扩展性 |  低（专有封闭软件） | 低（专有封闭软件） | 高（开源） |高（开源） |中（开源+专有软件） |
| 计算收费 |  科研免费，商业收费 | 科研免费，AI按时长 |  Hub计算收费 | - | - |
| 数据下载 |  免费，100 reqeusts/s | 20G/月内免费，超出8元/G | 免费，单位时间requests限制  | - | - |
| 编程语言 |  JavaScript/Python | Python | Python | Python | R/JavaScript/Python |

“谷歌地球引擎”和“微软星球计算机”均采取了免费数据下载政策（虽然限制了单位时间请求数，但未对下载总量进行限制），“阿里云地球引擎”的数据下载月总量限制（20G/月，如超出8元/G）可能不利于其迅速扩大用户群体。

## 卫星地球观测市场规模

2020年,卫星地球观测市场规模约483亿人民币，预计至2032年，市场规模将增至1148亿元人民币，主要细分市场为政府与国防、能源与自然资源、考古学与民用基础设施以及农林渔业。

![卫星地球观测市场规模](https://files.mdnice.com/user/61769/2ca28a2d-64b1-490f-9209-3d6946f5c695.png)


## 展望
阿里云地球引擎和PIE-Engine的奋起直追对我国遥感和地理大数据产业的发展有重要意义。我国卫星对地观测体系已取得长足进步，希望我国国家航天局（China National Space Administration， CNSA）和相关部门能够采取更为开放的数据政策，建立更完善的数据处理分发标准和流程，让我国的卫星数据易得、易用，发挥其应有的价值，减少不必要的审核过程和繁杂手续，大力支持相关产业的发展。

地球观测大数据云计算平台不仅需要分布式数据存储，更需要强大的并行和分布式计算能力。欧洲高性能计算联合组织 (EuroHPC JU) 汇集欧洲多个国家计算资源组建了超级计算机联盟（LUMI, 大型统一现代基础设施），用于处理大数据的百亿亿级超级计算机，并免费向高等院校开放使用。LUMI成员国包括芬兰、比利时、捷克共和国、丹麦、爱沙尼亚、冰岛、挪威、波兰、瑞典和瑞士[6]。我国是否可借鉴LUMI，将各地高性能计算中心整合起来，进行国家层次的统一管理、任务调度和系统维护，降低科研院所使用费用甚至免费开放，避免重复自建、管理和维护计算平台的成本，以期最大化现有计算资源的利用率，为地球观测大数据赋能，更好地为各行各业提供宝贵的计算资源。如有必要，也可交与商业公司进行运营维护。


## 参考文献

1. Gomes, Vitor CF, Gilberto R. Queiroz, and Karine R. Ferreira. ["An overview of platforms for big earth observation data management and analysis."](https://www.mdpi.com/2072-4292/12/8/1253) Remote Sensing 12, no. 8 (2020): 1253.

2. Gorelick, Noel, Matt Hancher, Mike Dixon, Simon Ilyushchenko, David Thau, and Rebecca Moore. ["Google Earth Engine: Planetary-scale geospatial analysis for everyone."](https://www.sciencedirect.com/science/article/pii/S0034425717302900) Remote sensing of Environment 202 (2017): 18-27.

3. Xu, Hao, Yuanbin Man, Mingyang Yang, Jichao Wu, Qi Zhang, and Jing Wang. ["Analytical Insight of Earth: A Cloud-Platform of Intelligent Computing for Geospatial Big Data."](https://arxiv.org/pdf/2312.16385.pdf) arXiv preprint arXiv:2312.16385 (2023).

4. Lewis, A.; Oliver, S.; Lymburner, L.; Evans, B.; Wyborn, L.; Mueller, N.; Raevksi, G.; Hooke, J.; Woodcock, R.; Sixsmith, J.; et al. [The Australian Geoscience Data Cube—Foundations and lessons learned.](https://www.sciencedirect.com/science/article/pii/S0034425717301086?via%3Dihub), Remote sensing of Environment, (2017), 202:276–292.

5. Large Unified Modern Infrastructure (LUMI): https://www.lumi-supercomputer.eu/about-lumi/
