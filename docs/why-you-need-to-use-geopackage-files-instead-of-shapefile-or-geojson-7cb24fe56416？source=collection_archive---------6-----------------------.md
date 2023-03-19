# 为什么需要使用 Geopackage 文件而不是 shapefile 或 GeoJSON

> 原文：<https://towardsdatascience.com/why-you-need-to-use-geopackage-files-instead-of-shapefile-or-geojson-7cb24fe56416?source=collection_archive---------6----------------------->

![](img/e3ae2fde1e9201a6295520e4c9204e8b.png)

伦敦公路网与优步游乐设施的图像。如 [Youtube 视频](https://www.youtube.com/watch?v=y-SA6bOv4Eo)所示。

# 当前矢量数据标准格式

如果您一直在使用矢量数据并进行空间分析，您应该知道 shapefile 和 geojson。这是两种最常用的矢量数据格式，用于存储数据和执行任何空间分析。特别是 GeoJSON，它已经成为向 web 应用程序提供数据的首选格式。但是，当您希望扩展您的工作并为大规模部署构建集成的自动化工作流时，这两种格式都有很多缺点。Geopackage 格式在这方面提供了多种功能。这就是为什么您需要使用 Geopackage 文件而不是 shapefile 或 GeoJSON。让我们更深入地了解细节。

如果你想了解更多关于地理空间数据以及它如何改变数据分析领域的信息，请点击这里的[查看我的博客。](https://www.samashti.space/articles/why-geospatial-data-is-the-way-forward-in-data-analytics)

[](https://www.samashti.space/articles/why-geospatial-data-is-the-way-forward-in-data-analytics) [## 为什么地理空间数据是数据分析的发展方向？- Samashti |博客

### 在过去的 10 年里，数据科学和分析已经成为各地的热门话题。数据分析帮助了…

www.samashti.space](https://www.samashti.space/articles/why-geospatial-data-is-the-way-forward-in-data-analytics) [](https://www.samashti.space/articles/download-rainfall-satellite-data-from-chrs-data-portal-using-python) [## 使用 Python - Samashti 从 CHRS 数据门户下载降雨卫星数据|博客

### 教程-使用 Python 模块轻松查询和下载降雨卫星数据，快速分析降雨…

www.samashti.space](https://www.samashti.space/articles/download-rainfall-satellite-data-from-chrs-data-portal-using-python) 

# Shapefiles 的问题

Shapefiles 已经存在很长时间了。ESRI 在 20 世纪 90 年代早期开发了这种格式&从那时起，它已经成为人们广泛采用的处理和共享矢量数据的标准格式之一。Shapefile 存储非拓扑矢量数据以及相关属性数据。尽管它被广泛使用，但对于现代用例来说，它有相当多的&显著的缺点；

*   Shapefile 是一种多文件格式。您保存的每个矢量层至少有 3 个文件(。shp，。shx &。dbf)和其他几个不同扩展名的附加文件。因此，如果您想与他人共享 shapefile，您必须共享一个图层的所有文件。如果你有几层，文件的数量就很大。每个项目的文件数量为 4-6 倍并不理想。
*   Shapefile 支持类似于带有列标题的表格数据集的相关属性数据。但是您只能使用十个字符来定义列标题，并且在列标题上需要一些描述/标识的情况下，使用列标题的缩写形式并不总是理想的。
*   形状文件的最大大小为 2GB。不能将包含更多可能超过 2GB 的要素的矢量图层导出为 shapefile。
*   shapefiles 在一个文件中不能有多个几何类型。
*   随着 shapefile 大小的增加以及处理更多的属性列和行，shapefile 的性能会急剧下降，甚至在 QGIS 上使用空间索引时也会变得缓慢。

# GeoJSONs 的问题

创建 GeoJSONs 部分是为了解决 shapefiles 的多文件问题。作为 web 应用程序上使用的 JSON 对象的扩展，它确实解决了 shapefiles 提出的一些问题。但是它有自己的一套限制；

*   对于具有属性的类似数量的矢量要素，在大多数情况下，GeoJSON 的文件大小几乎是 shapefile 的两倍。
*   GeoJSONs 没有空间索引。因此，在处理大量特性时，这很难处理。大多数时候，在 QGIS 地图画布上浏览空间要素是一项令人厌倦的任务。
*   每当您加载文件来运行一些任务时，整个文件都会被一次加载到内存中，这可能会在一些情况下产生问题，尤其是对于大文件。
*   此外，与 shapefile 和 geopackages 相比，文件的加载通常较慢，但内存消耗相似或更多。
*   如果文件大小超过了某个限制(根据我的经验，大约是 10–11gb)，这些特性可能会写得不完整，从而导致文件损坏。

# 什么是 GeoPackage？

由 [OGC](https://www.ogc.org/about) 开发的地理空间信息开放格式，他们定义地理包如下；

> *GeoPackage 是一种开放的、基于标准的、独立于平台的、可移植的、自描述的、紧凑的地理空间信息传输格式。*

geopackage 本质上是一个 SQLite 容器，采用 OGC 编码标准来存储矢量要素、切片矩阵(栅格数据)、非空间属性数据和扩展。

默认情况下，每个 geopackage 文件都有一些元表，如下所示，用于理解和处理地理空间图层。

```
'gpkg_spatial_ref_sys',
'gpkg_contents',
'gpkg_ogr_contents',
'gpkg_geometry_columns',
'gpkg_tile_matrix_set',
'gpkg_tile_matrix',
'gpkg_extensions',
'sqlite_sequence',
```

# 优势

*   开源，基于 SQLite 数据库
*   非常轻量级，但跨环境高度兼容(尤其是。在连接和带宽有限的移动设备中)
*   与 shapefiles 相比，Geopackages 的文件大小通常要小 1.1-1.3 倍，而 geojsons 的文件大小则小近 2 倍。

```
$ fs road_network/*
193M	road_network/roads.geojson
 70M	road_network/roads.gpkg
 81M	road_network/roads.shp
```

*   由于 geopackage 中的矢量图层本身具有 rtree 索引(空间索引)，因此在 QGIS 上加载文件或在文件数据库上进行查询的速度很快。
*   文件大小没有限制，它可以在较小的文件中处理大量的功能。
*   与 shapefiles 相比，通过为每列提供正确的上下文，列标题可以是全名和正确的。
*   与 shapefiles 相比，geopackages 的运行和算法输出速度更快(您可以在 QGIS 上尝试)。
*   单个 geopackage 文件可以有多个矢量图层，每个图层都有不同的几何类型。

```
$ ogrinfo ./outputs/road_network.gpkgINFO: Open of './outputs/road_network.gpkg' using driver 'GPKG' successful.1: roads_area_hex8_grids (Multi Polygon)
2: roads_area_hex9_grids (Multi Polygon)
3: roads_area_major_segments (Multi Polygon)
4: roads_network_lines (Line String)
5: roads_poly_line_vertices (Point)
6: roads_intersection_node_points (Point)
7: roads_end_node_points (Point)
```

*   除矢量图层外，您还可以拥有非空间属性表(熊猫表)。

```
$ ogrinfo ./india_villages_master_2011.gpkgINFO: Open of './india_villages_master_2011.gpkg' using driver 'GPKG' successful.1: village_data_master (None) # (non-spatial)
2: village_codes_mapping (None) # (non-spatial)
3: village_points (Point)
4: village_voronoi_polygons (Multi Polygon)
```

*   随着数据的更新，我们会定期对矢量图层进行修改。在 QGIS 或 python 上加载和编辑 geopackage 文件的功能会更快。
*   可以使用 GDAL、QGIS、Python、R、SQLite 和 Postgres 处理该文件(对每种模式的限制很少)
*   与 shapefile 或 geojson 相比，将 geopackage 添加和加载到 Postgres 数据库要快得多(对于一些较大的数据集，这需要花费很长时间),因为它已经是一种数据库格式，并且已经进行了空间索引(与 shapefile 或 geojson 相比)。
*   有趣的是，geopackages 还可以将栅格作为切片矩阵来处理。(当然，这也有一定的局限性)

# 如何在他们的工作流程中使用它？

与 shapefiles 和 GeoJSONs 相比，我们了解使用 geopackage 文件的优势。但是，我们如何以及在多大程度上可以将地理包文件集成到我们的空间分析工作流中？让我们探讨几个选项。

*   大型输出文件
*   平铺表格/多层文件
*   减少/避免输出的冗余文件
*   空间视图
*   仅将部分矢量图层加载到内存中
*   处理地理硕士
*   一个文件中的在制品(WIP)层
*   CartoDB 的文件导入
*   样品、默认颜色样式和其他属性

以上几点在我的博客上都有详细的解释。前往[我的博客](https://samashti.tech/why-you-need-to-use-geopackage-files-instead-of-shapefile-or-geojson/)阅读更多关于如何使用 geopackage 来加快空间分析工作流程的信息。

# GPAL

[GPAL](https://github.com/samashti/gpal) (Geopackage Python 访问库)是我为了解决使用 geopandas 读取和处理 Python 中的 Geopackage 文件的一些限制而构建的。我上面提到的 geopackage 格式的一些特性在 Python 中没有方法。因此，我开始构建一个模块。

目前，该模块可以使用 SQL 查询读取和处理 geopackage 文件上的文件操作，类似于 sqlite 数据库上的 sqlite3 模块。它有助于将部分矢量数据加载到内存中，而不是整个图层。目前，我还在开发其他一些功能。

# 有什么计划？

*   由于 geopackage 同时处理空间和非空间表，因此有必要从 python 模块中一致地处理这两种数据表。
*   表视图是数据库的一个特性，因为 geopackage 是基于 SQLite 的，所以我们可以将其扩展到空间视图，就像我上面提到的那样。但是它涉及到处理 geopackage 文件中的 gpkg 元表，并且需要用自己的方法来处理。
*   不同工作流中多层格式文件的处理方法。

Geopackage 是一种非常轻便且高度兼容的矢量文件格式。您可以节省大量存储空间，处理更少的文件，更快地运行算法和可视化。成为持续更新的开源格式是锦上添花。这种格式的所有当前和潜在的特性激励我参加 GPAL 项目，开发一些东西，我希望在 Python 上使用 geopackage 时增加更多的通用性。

我希望本文能帮助您理解 geopackage 相对于其他矢量文件格式的特性和优势。我很乐意收到任何进一步改进的建议。

如果你喜欢这个博客，就订阅这个博客，并在以后的博客文章中得到通知。如有任何疑问或讨论，你可以在 [LinkedIn](https://www.linkedin.com/in/nikhilhubballi/) 、 [Twitter](https://twitter.com/samashti_) 上找到我。查看我之前的博客，了解如何通过 python 脚本使用 QGIS 空间算法。

[](https://www.samashti.space/articles/how-to-use-qgis-spatial-algorithms-with-python-scripts) [## 如何通过 python 脚本使用 QGIS 空间算法？- Samashti |博客

### 当您了解 GIS 和空间分析时，QGIS 是您最先接触到的工具之一。你几乎可以处理…

www.samashti.space](https://www.samashti.space/articles/how-to-use-qgis-spatial-algorithms-with-python-scripts) [](https://www.samashti.space/articles/how-alternative-data-helping-companies-invest-big) [## 替代数据如何帮助公司进行大规模投资？- Samashti |博客

### 就在十年前，如果一家公司想将一种新产品或服务推向市场，识别潜在的…

www.samashti.space](https://www.samashti.space/articles/how-alternative-data-helping-companies-invest-big) 

# 一些转换

从命令行使用 GDAL

*   将形状文件转换为地理包
    `$ ogr2ogr -f GPKG filename.gpkg abc.shp`
*   所有文件(shapefile/geopackage)将被添加到一个地理包中。
    
*   将地理包添加到 postgres 数据库
    `$ ogr2ogr -f PostgreSQL PG:"host=localhost user=user dbname=testdb password=pwd" filename.gpkg layer_name`