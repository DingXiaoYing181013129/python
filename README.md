# python期末项目文档

## [github文档](https://github.com/DingXiaoYing181013129/python)（templates、static、app.py、数据文档）
* [data](https://github.com/DingXiaoYing181013129/python/tree/master/data)
* [templates](https://github.com/DingXiaoYing181013129/python/tree/master/templates)
* [static](https://github.com/DingXiaoYing181013129/python/tree/master/static)
* [app.py](https://github.com/DingXiaoYing181013129/python/blob/master/app.py)

## 本次期末项目一共有9个url

### 前端url描述
* 数据交互：每个页面均有相应的可视化图与文字简介
* 第1个url：是首页。主要展示世界抑郁症总的基本情况，其中有7个按钮提供跳转
* 第2个url：由“总人数”按钮跳转而来。此页面是世界地图的交互可视化，在地图上方设有一个按钮，用于返回首页
* 第3个url：由“患病率”按钮跳转而来。可视化地图下方是一个搜索框和世界各地区患病率表格。在搜索框中输入地区名，能够显示此地区2008年——2017年的数据；若此地区不存在，将会跳转到另一个url页面，上面提示：“您搜索的结果不存在！！”。本页面仍然是世界地图的交互可视化，在地图上方设有一个按钮，用于返回首页。
* 第4个url：搜索后跳转的url，分为数据存在与数据不存在（if条件判断）
* 第5个url：由“男性与女性患者人数”按钮跳转而来。此页面是条形图的交互可视化，在地图上方设有一个按钮，用于返回首页
* 第6个url：由“患病率与失业率”按钮跳转而来。此页面是折线图的交互可视化，主要显示世界抑郁症患病率、世界各国失业率前10的国家。在其上方设有一个按钮，用于返回首页。
* 第7个url：由“每个妇女生育孩子数”按钮跳转而来。此页面是地图交互可视化，在地图上方设有一个按钮，用于返回首页
* 第8个url：由“世界人均GDP情况”按钮跳转而来。此页面是地图交互可视化，在地图上方设有一个按钮，用于返回首页
* 第9个url：由“主题观点总结”按钮跳转而来，主要是项目结论与观点总结

## pythonanywhere
* [项目部署到云的Pythonanywhere链接](http://huangyuhui.pythonanywhere.com/) 


## HTML文档描述
#### HTML文档放在templates文件夹中。分别为：base.html 、entry.html 、viewlog.html 、world_gdp.html 、world_give_birth.html 、world_hbl.html 、world_man_woman.html 、world_number.html 、world_unemployment.html 、summary.html

* 1、base.html：基本模板档
* 2、entry.html ：首页模板档
* 3、viewlog.html：使用 def view_the_log()，log日志模板档
* 4、world_gdp.html ：使用 def yi_yu_select_8()，GDP情况模板档
* 5、world_give_birth.html：使用def yi_yu_select_7()，妇女生育孩子模板档
* 6、world_hbl.html：使用def yi_yu_select_2()，患病率模板档
* 7、world_man_woman.html：使用def do_search_3()，男性女性患者人数模板档
* 8、world_number.html ：def yi_yu_select_1()，总人数模板档
* 9、world_unemployment.html：def yi_yu_select_4()，失业率模板档
* 10、summary.html：def yi_yu_select_9()，观点总结模板档



## Python档描述
#### app.py
* 在pycharm安装了flask、pandas、pyecharts、cufflinks、plotly等等的模块包，并用 "import×××" 导入使用
* 用"df_×× = pd.read_csv('data/表格名字.csv', dtype=××,encoding='××') ”导入csv表格 注：此为本地使用
  若在pythonanywhere部署使用，则为df_×× = pd.read_csv('/home/huangyuhui/mysite/data/表格名字.csv', dtype=××, encoding = '××')
* 运用函数实现不同的功能
* 使用 "return render_template"实现python文档与html文档的数据交互
* 实现首页面与子页面的数据嵌套，在子页面中实现交互图与交互数据的呈现
* 运用了推导式等。例子：

```
x_z = [int(x) for x in df0.columns.values[1:]]
x_z_zx = [str(x) for x in x_z]
```

* 运用了for循环和条件判断（if  else）。例子：两个页面的之间的交互 运行代码：可在世界各国患病率的表格上搜索指定的地区
```
@app.route('/form', methods=['POST'])
def view_the_log() -> 'html':
    contents = []
    the_region = request.form["the_region_selected_10"]
    print(the_region)
    with open('data/number-with-depression-by-country-hbl.csv', encoding="unicode_escape") as log:
        ignore = log.readline()
        flights = {}

        for line in log:
            contents.append([])
            for item in line.strip().split(','):
                contents[-1].append(item)
        titles = ('地区', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015', '2016', '2017')
        return render_template('form.html',
                               the_title='View Log',
                               the_row_titles=titles,
                               the_data=contents,
                               )


@app.route('/search', methods=['POST'])
def do_search() -> 'html':

    search = request.form['search']
    with open('data/number-with-depression-by-country-hbl.csv', encoding="unicode_escape") as data:
        ignore = data.readline()
        flights = {}

        for line in data:
            k, v, d, e, r, t, y, b, f, c, n = line.strip().split(',')
            flights[k] = k, v, d, e, r, t, y, b, f, c, n
            

    if search in flights.keys():
        contents = [flights[search]]


    else:
        return render_template('fault.html',
                               the_title='data can not be find',
                               )

    titles = ('地区', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015', '2016', '2017')
    return render_template('viewlog.html',
                           the_title='search result',
                           the_row_titles=titles,
                           the_data=contents,
                           )
```

* 若将其连接到“患病率页面”并部署到pythonanywhere，代码则为：
```
@app.route('/world_hbl',methods=['POST'])
def yi_yu_select_2():
    with open("世界抑郁症患病率.html", encoding="utf8", mode="r") as f2:
        plot_all_2 = "".join(f2.readlines())
    the_region = request.form["the_region_selected_2"]
    print(the_region)
    title = '世界抑郁症情况及其相关因素研究'
    contents = []
    with open('/home/huangyuhui/mysite/data/number-with-depression-by-country-hbl.csv', encoding='utf-8') as data1:
        ignore = data1.readline()
        countries = {}

        for line in data1:
            contents.append([])
            for item in line.strip().split(','):
                contents[-1].append(item)

    titles = ('地区', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015', '2016', '2017')
    return render_template('world_hbl.html',
                            the_plot_all_2 = plot_all_2,
                            the_title = title,
                            the_row_titles=titles,
                            the_data=contents,
                            )


@app.route('/search', methods=['POST'])
def do_search_1():
    search = request.form['place']
    with open('/home/huangyuhui/mysite/data/number-with-depression-by-country-hbl.csv', encoding='utf-8') as data2:
        ignore = data2.readline()
        countries = {}
        for line in data2:
            k, v, d, e, r, t, y, b, f, c, n = line.strip().split(',')
            countries[k] = k, v, d, e, r, t, y, b, f, c, n

    if search in countries.keys():
        contents = [countries[search]]
    else:
        contents = ['您搜索的结果不存在！！']
```

## Web App动作描述
* 首页页面设有7个跳转的按钮，点击即可跳转到下一级的子页面。 例子：
```
<form method="POST" action="/要跳转的下一级子页面">
    <p><input name="在app.py中用函数定义的the_region的名称" value='按钮名称' type='SUBMIT'></p>
</form>
```
* 每个子页面都有一个“首页”按钮，点击即可返回首页
* “患病率”页面里，有一个“确定”按钮，点击可进入搜索世界各地区各年的抑郁症患病率
* 使用 " the_region = request.form["×××"] " 实现页面与页面之间的跳转
* 返回历史上一页按钮的代码：
```
<input type="button" name="Submit" onclick="javascript:history.back(-1);" value="返回">
```

## log日志系统
* 记录搜索结果 [点击查看](http://huangyuhui.pythonanywhere.com/viewlog)
![](python/log.png)