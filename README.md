# python期末项目文档

## [github文档]()（templates、static、app.py、数据文档）
* [data]()
* [templates]()
* [static]()
* [app.py]()

# 本次期末项目一共有9个url

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
* 第9个url：由“主题观点总结”按钮跳转而来，主要是项目结论与总结

### pythonanywhere
* [项目部署到云的Pythonanywhere链接](http://huangyuhui.pythonanywhere.com/) 


### HTML文档描述
###### HTML文档放在templates文件夹中。分别为：base.html 、entry.html 、viewlog.html 、world_gdp.html 、world_give_birth.html 、world_hbl.html 、world_man_woman.html 、world_number.html 、world_unemployment.html 、summary.html

* 1、base.html：基本模板档
* 2、entry.html ：首页模板档
* 3、viewlog.html：使用 def view_the_log()，log日志模板档
* 4、world_gdp.html ：使用 def yi_yu_select_8()，GDP情况模板档
* 5、world_give_birth.html：使用def yi_yu_select_7()，妇女生育孩子模板档
* 6、world_hbl.html：使用def yi_yu_select_2()，患病率模板档
* 7、world_man_woman.html：使用def do_search_3()，男性女性患者人数模板档
* 8、world_number.html ：def yi_yu_select_1()，总人数模板档
* 9、world_unemployment.html：def yi_yu_select_4()，失业率模板档
* 10、summary.html：def yi_yu_select_9()



### Python档描述
###### app.py
* 在pycharm安装了flask、pandas、pyecharts、cufflinks、plotly的模块包，并用import××× 导入使用
* 用"df_×× = pd.read_csv('data/表格名字.csv', dtype=××,encoding='××') ”来导入csv表格
* 实现首页面与子页面的数据嵌套，在子页面中实现交互图与交互数据的呈现
* 运用了推导式等

```
x_z = [int(x) for x in df0.columns.values[1:]]
x_z_zx = [str(x) for x in x_z]
```

* 运用了for循环和条件判断，可在世界各国患病率的表格上进行搜索指定的地区
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
    with open('data/children-per-woman.csv', encoding="unicode_escape") as data:
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
                           the_title='View Log',
                           the_row_titles=titles,
                           the_data=contents,
                           )
```

### Web App动作描述
* 首页页面设有7个跳转的按钮，点击即可跳转到下一级子页面
* 每个子页面都有一个“首页”按钮，返回首页
* “患病率”页面里，有“确定”按钮，点击可进入搜索各地区各年的抑郁症患病率
* 使用“the_region”实现页面与页面之间的跳转