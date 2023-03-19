# 用 Geopy 和 leav 制作酷地图

> 原文：<https://towardsdatascience.com/making-simple-maps-with-folium-and-geopy-4b9e8ab98c00?source=collection_archive---------15----------------------->

## 使用 Nominatim 地理编码器将城市名称转换为点位置，并使用 leav 进行可视化

![](img/f0024293a7b16ee0bc976805c315af69.png)

谢栋豪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这是一个由两部分组成的关于可视化城市和与他人分享您的成果的系列。对于那些“给我看看代码”的家伙来说，[这是链接](https://github.com/Alyeko/Making-Cool-Maps-In-Python)。以下是要涵盖的主题的顺序:

∘ [简介](#60c5)
∘ [重现代码](#16f4)
∘ [场景](#728c)
∘ [代码](#6f3f)
∘ [用 Geopandas](#1719)
∘ [用树叶](#d092)
∘ [用树叶做更多事情](#3a10)
∘ [回答提问](#73e0)

## **简介**

以这篇文章为例，简单介绍一下[地理编码](https://en.wikipedia.org/wiki/Address_geocoding)以及分别用 geopy 和 leav 绘图。除了 geopy 之外，还有许多其他方法可以进行地理编码。有许多可用的 API，其中一些执行普通的地理编码，而另一些则帮助您从 IP 地址获取位置信息。在这里查看[和](https://github.com/public-apis/public-apis)。对于这篇文章，我是从阅读[这篇](/beautifully-simple-maps-with-tableau-and-the-google-maps-api-6eeb89263c52)中获得灵感的。我知道有一个全 python 的方法来解决这个问题，所以我们来了。

## **复制代码需要什么**

一台电脑(显然？🤷🏾‍♀️)，你选择的 IDE，安装的 *geopy* 、 *shapely* 、 *geopandas* 、 *matplotlib* 和*叶子*和*随机*模块。您可以使用虚拟环境，这样这些新的包就不会与您已经安装在基础环境中的包发生冲突。选择权在你。

## 方案

AYK 快递公司在非洲各国首都有 11 个分支机构，每个分支机构雇用 10 至 30 人，这些分支机构都有网址。我们必须为快递公司生成一些数据，并将其可视化。公司应该在哪些国家开设新的分支机构？让我们看看如何在可视化的同时回答这个问题。

## **代码**

[GIF via giphy](https://media.giphy.com/media/UO5elnTqo4vSg/giphy.gif)

用于该任务的模块被导入。创建了一个由该公司有分支机构的 11 个城市组成的列表。对于 *get_coordinates* 函数，它将城市名称列表作为参数。*名称*是使用的地理编码器，存储在*地理定位器*变量中。然后对列表中的每个城市进行迭代，地理编码器将它们转换成坐标值。在每次迭代结束时，坐标被保存到字典中，字典在开始时被初始化为值和城市名、关键字。 *get_coordinates* 函数在 *city_list* 上被调用并保存到 *city_coords_dict。*

## 用 Geopandas 绘图

由于我们将主要在 geopandas 中工作，它有一个绘图功能，我们可以使用它来可视化我们的城市，然后再在 follow 中绘图。使用它的一个原因是因为它对矢量数据非常有用，另一个原因是因为我们正在处理 3D 世界中的点(城市),而不仅仅是 2D 平面中的点，所以我们需要这个属性。好吧，我们开始吧！

我们为所有城市点创建了一个 [shapely 点](https://shapely.readthedocs.io/en/stable/manual.html#points)列表，并存储在一个 *cities_geom* 中。 *d* ，dict 是用*城市*作为关键字创建的，城市名称、值和几何关键字的形状点都存储为值。 *cities_gdf* 是通过将 dict、 *d* 和 *crs* 传递给 geopandas 的 *GeoDataFrame* (带有几何列的 pandas 数据帧)方法创建的，然后进行绘制。但这并不意味着什么，不是吗？它只是一些标在 2D 坐标轴上的蓝点。现在，如果将这些点标绘在非洲地图上，可视化效果将会得到改善，您可以看到这些点之间的相互关系，还可以看到没有点的国家(如果您愿意，也可以是代表)。

Geopandas 有许多可供您探索的数据集。我们将使用的是 *naturalearth_lowres* ，它使我们能够访问世界地图，但我们的城市都是非洲的，因此我们将提取地理数据框中所有大陆值等于非洲的行，并将其存储为 *africa_gdf。*然后我们再次使用*绘图*函数，并传递参数*颜色*和*边缘颜色，*来定制我们的绘图。 *plt.rcParams* 允许您控制绘图*的大小。*为了标记非洲国家(用多边形表示)，我们为每个国家找到了一定会在它们各自的多边形中找到的点(使用 [*代表点()。x* 、*代表点()。y*](https://geopandas.readthedocs.io/en/latest/docs/reference/api/geopandas.GeoSeries.representative_point.html) )然后把标签放在那里，用*标注*。这里我们要做的最后一件事是断言 *cities_gdf* 的 crs 等于 *africa_gdf* 的 CRS，因为如果不是这样，我们的点数就不会落在正确的位置。

## 用叶子绘图

我们先用 Let 做一个简单的地图。

*cities_gdf* 的几何列被转换为 *json(* 但它自动成为 GeoJson，因为它有一个地理空间组件*)。叶子。Map()* 创建地图对象，该对象具有其他用于绘图的功能，如 *add_child* 。geojson 被传递给它，然后您就有了输出。

## 用叶子做更多事情

还记得我说过我们必须为分支机构生成数据吗？是啊，是时候了。我们需要这样做，这样我们就可以添加 popups 来使地图更具交互性。

除了已经存在的栏之外，还创建了新的栏(*名称*、*坐标*、*员工数量*和*网站】)*。

*add_markers_to_the_map* 采用一个树叶地图对象、地理数据框、颜色和图标参数。*点*变量被创建。它存储点的坐标、名称、员工人数和网站地址。这些将用于弹出窗口。一次迭代完成后，对于存储在 *points* 变量中的每个点，创建 *popup_text、popup* 和 *marker v* 变量，并将添加到 *the_map 中。*

*   *popup:* 创建一个 popup 对象并存储 *popup_text。*
*   *marker* :定制弹出窗口，并在某个特定位置显示。前缀“fa”代表[字体牛逼](https://fontawesome.com)，里面有很多图标。

然后我们用我们选择的参数和 TA-DA 调用函数！继续使用以下命令保存您的地图:

```
the_map.save(‘the_map.html’)
```

将地图保存为 html 文件意味着它可以在网页上使用。我们将在本系列的第二部分讨论这个问题。

## 对提问的回答

公司应该在哪些国家开设新的分支机构？

对于这个问题，你可以考虑两个城市，计算它们之间的距离，并创建一个匀称的 *LineString* 。然后，你可以使用 *LineString* 的*质心*属性来知道那个中心点是什么。它会落在一个国家，那里有你的答案。很简单，是吧？

## **结论**

这是一个总结！欢迎在评论中留下你的想法。快乐学习和编码。感谢阅读！😃👋

接下来:

[](/your-cool-folium-maps-on-the-web-313f9d1a6bcd) [## 你在网上的酷叶地图

### 利用 flask 和 heroku 与世界分享您的交互式地图

towardsdatascience.com](/your-cool-folium-maps-on-the-web-313f9d1a6bcd)