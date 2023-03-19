# 使用 Flask 部署 Spotify 推荐模型

> 原文：<https://towardsdatascience.com/deploying-a-spotify-recommendation-model-with-flask-20007b76a20f?source=collection_archive---------10----------------------->

机器学习模型的真正价值在于可用性。如果模型没有被恰当地部署、使用，并且没有通过客户反馈的循环不断地更新，它注定会停留在 GitHub 的仓库中，永远不会达到它的实际潜力。在本文中，我们将学习如何通过几个简单的步骤在 Flask 中部署 Spotify 推荐模型。

我们将部署的应用程序存储在 recommendation_app 文件夹中。其结构如下所示:

```
wsgi.py
requirements.txt
data/
application/
   model.py
   features.py
   templates/
      home.html
      results.html
      about.html
   static/
      style.css
   routes.py
```

在根目录中，我们有 wsgi.py 文件，当我们在终端中运行' $ flask run app '时将调用该文件，它调用 create_app()函数在本地主机上创建我们的服务器。Requirements.txt 包含我们的项目在另一个需求中运行所需的依赖项。数据文件夹包含模型推荐歌曲所需的数据。

应用程序文件夹包含我们的 flask 应用程序的主要组件。显然，我们需要的第一个组件是模型本身。这里，我们将使用一个 Spotify 推荐模型，在给定一个 Spotify 播放列表的情况下，根据从 Spotify 库中检索到的音频特征推荐一些适合该播放列表的歌曲。如果您对如何检索此类模型的相关数据、基于音频特征执行聚类并开发推荐模型感兴趣，请随时查看本系列的以下文章:

*   [用 Python 从 Spotify 的 API 中提取歌曲数据](https://cameronwwatts.medium.com/extracting-song-data-from-the-spotify-api-using-python-b1e79388d50)
*   [EDA 和集群](https://medium.com/p/6d755624f787/edit)
*   [用 Spotify 搭建歌曲推荐系统](https://medium.com/@enjui.chang/part-iii-building-a-song-recommendation-system-with-spotify-cf76b52705e7)

此外，我们需要一个函数，将 Spotify 播放列表链接转换为包含相关功能的 pandas 数据帧，推荐将基于该数据帧进行——在我们的示例中，它存储在 features.py 文件中。简而言之，该函数获取一个可以从桌面 Spotify 应用程序获取的链接，并连续分割它以获得构成播放列表 ID 的字母和数字的组合。最后，通过使用 spotipy 库中的“user_playlist”函数和一系列字典操作，该函数创建了最终的数据帧。

最后，是时候创建一个简单的 flask 应用程序了！

flask 应用程序的最简单形式由两个文件夹组成——templates 和 static，以及一个包含应用程序路线的 python 文件。前一个文件夹包含用户输入数据时将呈现的 HTML 模板，后一个文件夹包含带有简单样式的 CSS 文件。路线文件将包含三条路线:

```
@app.route("/")
def home():
   return render_template('home.html')@app.route("/about")
def about():
   return render_template('about.html')@app.route('/recommend', methods=['POST'])
def recommend():
   URL = request.form['URL']
   df = extract(URL)
   edm_top40 = recommend_from_playlist(songDF, complete_feature_set,       df)
   number_of_recs = int(request.form['number-of-recs'])
my_songs = [] for i in range(number_of_recs):
my_songs.append([str(edm_top40.iloc[i,1]) + ' - '+ '"'+str(edm_top40.iloc[i,4])+'"', "https://open.spotify.com/track/"+ str(edm_top40.iloc[i,-6]).split("/")[-1]]) return render_template('results.html',songs= my_songs)
```

*   用户打开应用程序时将到达的家庭路线('/')；
*   推荐路径('/recommend)将处理来自表单的输入，并将推荐歌曲的列表发送回用户；
*   包含项目基本信息的信息路线('/about ')；

recommend()函数到底是做什么的？

*   首先，它通过 POST 请求从 HTML 表单中获取用户输入的 URL
*   它调用 extract()函数，该函数将 URL 转换为音频特征的数据帧；
*   它调用 recommend_from_playlist()函数根据用户的播放列表创建推荐歌曲的数据帧；
*   它根据通过 POST 请求在下拉菜单中选择的值，请求用户想要的推荐数量；
*   它创建一个空列表，用 x 首推荐歌曲填充，其中 x 是选择的推荐数；
*   最后，它呈现以 my_songs 列表作为输入的 results.html 模板，并为用户输出歌曲推荐；

为了让应用程序看起来更好看，我们将使用 bootstrap，它包含各种可以重用的元素，如按钮、标题、表单等。在我的例子中，我把引导程序用于标题、“关于”和“主页”按钮。此外，为了使推荐逐渐出现，我在 CSS 中使用了提供一些动画功能的关键帧标签。

```
@keyframes fastSlide {
0%   { transform: translate(120%) skewX(0deg) ; }
70%  { transform: translate(0%)   skewX(1deg) ; }
80%  { transform: translate(0%)   skewX(0deg)  ; }
95%  { transform: translate(0%)   skewX(0deg) ; }
100% { transform: translate(0%)   skewX(0deg)   ; }
}
```

例如，这允许我从屏幕右侧逐渐显示推荐。

要在本地运行我们的推荐应用程序，我们可以在根目录中运行“$ python wsgi . py”——这将确保服务器处于开发模式，我们可以在进行任何更改时调试和刷新它。

![](img/cb5b265b84cd85de5c75d35f4aa40d11.png)

图片作者。

![](img/90aa0acc2938792cda882b93e4367b3f.png)

图片作者。

正如我们所见，该应用程序运行正常，并根据播放列表链接和用户选择的推荐数量返回一组推荐。然而，我们还没有完成！如果我们希望有人使用我们的应用程序，而不必在本地安装所有的文件夹，该怎么办？

这就是云部署的用武之地！有各种各样的服务可以帮助人们部署他们的应用程序，但在本文中，我们将使用 Pythonanywhere.com，它有免费试用和负担得起的大型应用程序选项。我们所需要做的就是创建一个初学者帐户，创建一个适当的文件夹结构，并将我们所有的文件上传到那里。在我的例子中，我遵循了 [Sohan Dillikar](https://medium.com/u/e56fe5c33e6a?source=post_page-----20007b76a20f--------------------------------) 关于[如何在 PythonAnywhere 上免费托管你的 Flask 应用的教程](https://medium.com/swlh/how-to-host-your-flask-app-on-pythonanywhere-for-free-df8486eb6a42)——然而在我的例子中，我没有使用 requirements.txt 文件，而是使用下面的[教程](https://help.pythonanywhere.com/pages/Virtualenvs/)创建了一个虚拟环境并安装了软件包。

瞧，我们有了一个功能正常的网站，上面有我们的[应用程序](http://nazaryaremko1.pythonanywhere.com/)，可以随时使用并向潜在雇主展示:

网站演示文稿

很简单，对吧？