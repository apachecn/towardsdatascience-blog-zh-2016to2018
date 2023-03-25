# 用 PRAW 刮红迪网

> 原文：<https://towardsdatascience.com/scraping-reddit-with-praw-76efc1d1e1d9?source=collection_archive---------5----------------------->

![](img/265e32b7ecc9fbf64408e59ac7f0061f.png)

最近，我试图开始一个项目，该项目将使用自然语言处理来分类一个给定的帖子来自哪个子编辑。例如，这个模型应该能够预测一个帖子是否来自于 [r/Python](https://www.reddit.com/r/python) 子编辑或者 [r/Rlanguage](https://www.reddit.com/r/rlanguage) 子编辑。这个过程的第一步是从每个子编辑中收集一些帖子。在过去，我使用过 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 和 [requests](http://docs.python-requests.org/en/master/) python 库来完成这项工作。我的一些同事提到 API 包装器非常有用，可以简化大部分过程。这就是我如何偶然发现 Python Reddit API 包装器( [PRAW](https://praw.readthedocs.io/en/latest/) )。

我发现的最有帮助的文章之一是 Felippe Rodrigues 的“[如何用 Python 刮 Reddit](http://www.storybench.org/how-to-scrape-reddit-with-python/)”他在基本步骤和设置方面做得很好。如果你有兴趣做类似的事情，一定要去看看。我将这篇文章作为我自己项目的起点。下面我将带你了解如何使用 PRAW 进行设置，以及如何抓取文章标题、内容和其他元数据。你可以在我的个人 [Github](https://github.com/smidem/reddit_scraper) 上找到完整的代码。

开始使用 PRAW 的第一步是通过 reddit 创建一个应用程序。如果你转到这个[链接](https://www.reddit.com/prefs/apps)，你应该会在底部找到一个标有“创建应用”或“创建另一个应用”的按钮你必须给你的脚本一个名字，并填写一个描述。一旦完成，确保选择“脚本”选项，然后确保将以下内容放入重定向 uri 框: [http://localhost:8080](http://localhost:8080) 。这是由 [PRAW 文档](https://praw.readthedocs.io/en/latest/getting_started/authentication.html#script-application)建议的，但显然是必要的，因为 Reddit 需要一个重定向 uri，即使我们的应用程序不使用它。如果成功的话，你会得到两串看似随机的字符。你可以在左上角找到第一个，你的“个人使用脚本”，就在“个人使用脚本”的正下方这个应该有 14 个字符长。接下来你需要的是“秘密”。虽然这听起来像是幻想小说中的东西，但它应该表示为一个 27 个字符的字符串，列在“个人使用脚本”的下面。有了这两样东西，我们终于可以开始从 Reddit 抓取帖子的旅程了！

现在，您需要打开您最喜欢的编辑器或 IDE。我用 PyCharm 做这个项目，但是 Atom 或者 Sublime 也可以。我们首先需要创建一个 praw.ini 文件。这将允许我们在未来利用 PRAW Reddit 实例，这意味着如果您决定发布它，我们可以将我们的个人使用脚本和秘密从我们的主要脚本中删除。

你可以参考关于 praw.ini 文件的[文档](https://praw.readthedocs.io/en/latest/getting_started/authentication.html#script-application)，但是一定要确保这个文件确实叫做“praw.ini ”,并且它和你的抓取脚本位于同一个目录。可以在这个文件中定义多个凭证，但是今天我们将只使用一个凭证，所以我们将坚持使用[默认]站点。

```
[DEFAULT]
; this is your 14 character personal use script
client_id=51IfSlyxtyOUy3; this is your 27 character secret
client_secret=NToU1zG5d0LJq9fo3ryUtOigM5h; this is the name you gave your application
user_agent=subreddit scraper; this is username for the reddit account the app was created with
username=fake_username; password for the account
password=fake_password
```

请确保在这些字段中填写您的信息。上面的字段是使用随机生成的假值填写的。如果您不使用自己的信息替换这些字段，它将不起作用。

现在我们可以开始编写实际的抓取脚本了。第一步是导入必要的库，并使用我们在 praw.ini 文件中定义的凭证实例化 Reddit 实例。

```
from os.path import isfile
import praw
import pandas as pd
from time import sleep*# Get credentials from DEFAULT instance in praw.ini* reddit = praw.Reddit()
```

我决定创建一个类，允许我指定我感兴趣的特定子编辑、排序方法、文章数量以及结果是否应该写入文件。__init__ 方法如下所示:

```
class SubredditScraper:

    def __init__(self, sub, sort=**'new'**, lim=900, mode=**'w'**):
        self.sub = sub
        self.sort = sort
        self.lim = lim
        self.mode = mode print(
            **f'SubredditScraper instance created with values '
            f'sub = {sub}, sort = {sort}, lim = {lim}, mode = {mode}'**)
```

我加入了一些打印语句来感受一下脚本的进度，因为在处理数百个帖子时可能需要一点时间来运行。接下来，我们将为 subreddit 实例设置排序方法。

```
def set_sort(self):
    if self.sort == **'new'**:
        return self.sort, reddit.subreddit(self.sub).new(limit=self.lim)
    elif self.sort == **'top'**:
        return self.sort, reddit.subreddit(self.sub).top(limit=self.lim)
    elif self.sort == **'hot'**:
        return self.sort, reddit.subreddit(self.sub).hot(limit=self.lim)
    else:
        self.sort= **'hot'** print(**'Sort method was not recognized, defaulting to hot.'**)
        return self.sort, reddit.subreddit(self.sub).hot(limit=self.lim)
```

这个方法将把 reddit 实例的 subreddit 和排序参数设置为我们在实例化我们的类时指定的值。这将返回一个元组，我们将在下一个方法中解包它。如果排序方法不是“新”、“前”或“热”，则默认为“热”。

最后，我们可以开始从指定的子编辑中收集帖子和其他信息:

```
def get_posts(self):
    *"""Get unique posts from a specified subreddit."""* sub_dict = {
        **'selftext'**: [], **'title'**: [], **'id'**: [], **'sorted_by'**: [],
        **'num_comments'**: [], **'score'**: [], **'ups'**: [], **'downs'**: []}
    csv = **f'{self.sub}_posts.csv'** *# Attempt to specify a sorting method.* sort, subreddit = self.set_sort()

    *# Set csv_loaded to True if csv exists since you can't 
    # evaluate the truth value of a DataFrame.* df, csv_loaded = (pd.read_csv(csv), 1) if isfile(csv) else (**''**, 0)

    print(**f'csv = {csv}'**)
    print(**f'After set_sort(), sort = {sort} and sub = {self.sub}'**)
    print(**f'csv_loaded = {csv_loaded}'**)

    print(**f'Collecting information from r/{self.sub}.'**)
```

这里我们创建了一个占位符字典，其中包含了我们将从这个子编辑的每个帖子中收集的每个属性。那我们就要建立。csv 文件以备将来使用，我们正在对 set_sort 方法返回的元组进行解包。最后一项设置是查看我们之前是否为此子编辑收集过帖子。如果是这样，我们将加载它。csv 文件放入数据帧，并将布尔变量 csv_loaded 设置为 1。否则，df 将是一个空字符串，csv_loaded 将被设置为 0。

这给我们带来了真正的刮肉。我们将使用 for 循环来查看每篇文章，并收集我们感兴趣的属性。

```
for post in subreddit:

    *# Check if post.id is in df and set to True if df is empty.
    # This way new posts are still added to dictionary when df = ''* unique_id = post.id not in tuple(df.id) if csv_loaded else True

    *# Save any unique posts to sub_dict.* if unique_id:
        sub_dict[**'selftext'**].append(post.selftext)
        sub_dict[**'title'**].append(post.title)
        sub_dict[**'id'**].append(post.id)
        sub_dict[**'sorted_by'**].append(sort)
        sub_dict[**'num_comments'**].append(post.num_comments)
        sub_dict[**'score'**].append(post.score)
        sub_dict[**'ups'**].append(post.ups)
        sub_dict[**'downs'**].append(post.downs)
    sleep(0.1)
```

如果您计划多次使用这个脚本来收集大量的帖子，我们需要检查每个帖子是否是唯一的，或者我们是否已经将它添加到我们的。csv 文件。这就是我们的布尔变量 csv_loaded 派上用场的地方。我们将检查文章的 id 属性是否在数据框(df)的“id”列中。如果这是我们第一次为此子编辑收集帖子，那么 unique_id 将为每个帖子设置为 True。然后，我们将把我们感兴趣的文章的每个属性添加到占位符字典中。最后，也可能是最重要的，我们会在每次发布后睡十分之一秒。这是一种人为的限速器。如果我们不这样做，我们最终会达到 API 的请求限制(1000)。有几种方法可以绕过这个限制，其中之一是[请求一个刷新令牌](https://praw.readthedocs.io/en/latest/tutorials/refresh_token.html)，但是这暂时应该可以正常工作。

接下来，我们将把结果保存到. csv 文件中，或者根据我们之前是否抓取过这个子编辑来修改现有的文件:

```
new_df = pd.DataFrame(sub_dict)

*# Add new_df to df if df exists then save it to a csv.* if **'DataFrame'** in str(type(df)) and self.mode == **'w'**:
    pd.concat([df, new_df], axis=0, sort=0).to_csv(csv, index=False)
    print(
        **f'{len(new_df)} new posts collected and added to {csv}'**)
elif self.mode == **'w'**:
    new_df.to_csv(csv, index=False)
    print(**f'{len(new_df)} posts collected and saved to {csv}'**)
else:
    print(
        **f'{len(new_df)} posts were collected but they were not '
        f'added to {csv} because mode was set to "{self.mode}"'**)
```

这里我们使用 pandas concat 方法，以便将我们的新结果添加到现有结果中。如果存在 csv 文件，并且在实例化我们的类时 mode 被设置为“w ”,那么我们将它加载到 df 数据帧中。如果没有现存的。我们将把结果写入这个子编辑的 csv 文件。如果模式没有设置为“w ”,我们将打印出找到的帖子数量，而不是将它们写入文件。

这就是我们完善 SubredditScraper 类所需的全部内容。剩下要做的最后一件事是实例化我们的新类。

```
if __name__ == **'__main__'**:
    SubredditScraper(
        **'python'**,
         lim=997,
         mode=**'w'**,
         sort=**'new'**).get_posts()
```

如果你不熟悉`if __name__ == '__main__':`的用法，我建议看看[科里·斯查费的](https://www.youtube.com/channel/UCCezIgC97PvUuR4_gbFUs5g)关于这个话题的视频[这里](https://www.youtube.com/watch?v=sugvnHA7ElY)。这样，您应该能够运行这个脚本，从 python subreddit 中收集近 1000 篇帖子，并将这些帖子及其一些元数据保存到一个. csv 文件中。我希望这有所帮助！别忘了你可以在这里找到这个项目的完整代码。