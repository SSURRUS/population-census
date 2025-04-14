

## 下面我会对这些代码进行逐字解析，以方便大家更深入的理解代码以及明白一个完整的数据分析流程<br>
## 如需代码可以去主页提取 自述区间仅用于学习 <br>
```
from pyecharts.charts import * 
from pyecharts import options as opts 
from pyecharts.commons.utils import JsCode 
```
### 从pyecharts.charts模块中导入所有的内容（此处的*表示为通配符）而此处的pyecharts 是一个基于 ECharts 的 Python 数据可视化库
### 下面两个是从pyecharts导入options模块（对图表进行详细的配置）以及从 pyecharts.commons.utils 模块中导入 JsCode 类（用于在 pyecharts 图表中嵌入 JavaScript 代码）
```
import pandas as pd
```
#导入 pandas 库，pandas 是一个强大的数据处理和分析库，它提供了 DataFrame 和 Series 两种核心数据结构，适合处理表格数据（类似于 Excel 或 SQL 表）<br>
```
import random
```
#random 模块提供了生成随机数的功能<br>
```
from pyecharts.globals import CurrentConfig, NotebookType,OnlineHostType
```
从 pyecharts.globals 模块中导入 CurrentConfig、NotebookType 和 OnlineHostType<br>
CurrentConfig：一个全局配置对象，用于设置 pyecharts 的运行时参数，例如指定 Notebook 环境、设置在线资源地址等<br>
NotebookType：一个枚举类，定义了不同的 Notebook 环境类型，例如 Jupyter Notebook 或 Jupyter Lab<br>
OnlineHostType：一个枚举类，用于指定在线资源的来源（例如 ECharts 的 JavaScript 文件可以从哪个 CDN 加载）<br>
```
CurrentConfig.NOTEBOOK_TYPE = NotebookType.JUPYTER_NOTEBOOK
```
#将 CurrentConfig 的 NOTEBOOK_TYPE 属性设置为 NotebookType.JUPYTER_NOTEBOOK<br>
设置这个属性后，pyecharts 会调整渲染方式，确保图表能在 Jupyter Notebook 中正确显示（例如生成 HTML 和 JavaScript 代码，并嵌入到 Notebook 的输出单元中）<br>
```
# pd.set_option('precision', 2)
```

df = pd.read_csv('E:/data analyze/各地区人口.csv', usecols=['各地区人口', 'Unnamed: 1'])<br>
#使用 pandas 的 read_csv 方法读取一个 CSV 文件，并将其加载为一个 DataFrame 对象<br>
#usecols=['各地区人口', 'Unnamed: 1']：指定只读取 CSV 文件中的两列<br>
```
df.drop(labels=[0, 1], axis=0, inplace=True)
```
#从 DataFrame 中删除指定的行<br>
#labels=[0, 1]：指定要删除的行索引，这里是第 0 行和第 1 行<br>
#axis=0：指定操作的轴，axis=0 表示按行操作（axis=1 则表示按列操作）<br>
#inplace=True：表示直接修改原始 DataFrame，而不是返回一个新的 DataFrame<br>
```
df.rename(columns={'Unnamed: 1':'人口数', '各地区人口':'地区'}, inplace=True)
```
#重命名 DataFrame 的列名<br>
```
dictcode = {'北京': '北京市',
 '天津': '天津市',
 '河北': '河北省',
 '山西': '山西省',
 '内蒙古': '内蒙古自治区',
 '辽宁': '辽宁省',
 '吉林': '吉林省',
 '黑龙江': '黑龙江省',
 '上海': '上海市',
 '江苏': '江苏省',
 '浙江': '浙江省',
 '安徽': '安徽省',
 '福建': '福建省',
 '江西': '江西省',
 '山东': '山东省',
 '河南': '河南省',
 '湖北': '湖北省',
 '湖南': '湖南省',
 '广东': '广东省',
 '广西': '广西壮族自治区',
 '重庆': '重庆市',
 '四川': '四川省',
 '贵州': '贵州省',
 '云南': '云南省',
 '西藏': '西藏自治区',
 '陕西': '陕西省',
 '青海': '青海省',
 '宁夏': '宁夏回族自治区',
 '新疆': '新疆维吾尔自治区'}
```
#定义了一个 Python 字典（dict），用于存储中国各省级行政单位的简称和全称的映射关系<br>
#{"Key","Vakue"}<br>
```
coord = {<br>
    '湖北': [114.31667, 30.51667],
    '广东': [113.23333, 23.16667],
    '北京': [116.41667, 39.91667],
    '上海': [121.48, 31.22],
    '辽宁': [123.38, 41.8],
    '江西': [115.90000, 28.68333],
    '四川': [104.06, 30.67],
    '安徽': [117.27, 31.86],
    '广西': [108.33, 22.84],
    '新疆': [87.68, 43.77],
    '江苏': [118.78, 32.04],
    '河北': [114.48, 38.03],
    '浙江': [120.19, 30.26],
    '湖南': [113, 28.21],
    '甘肃': [103.82, 36.07],
    '福建': [119.28000, 26.08],
    '贵州': [106.71, 26.57],
    '海南': [110.35, 20.02],
    '河南': [113.65, 34.76],
    '黑龙江': [126.63, 45.75],
    '吉林': [125.35, 43.88],
    '内蒙古': [111.65, 40.82],
    '宁夏': [106.27, 38.47],
    '山东': [117, 36.65],
    '山西': [112.53, 37.87],
    '陕西': [108.95, 34.27],
    '天津': [117.2, 39.13],
    '西藏': [91.11, 29.97],
    '云南': [102.73, 25.04],
    '重庆': [106.54, 29.59],
    '青海': [101.74, 36.56]}
```
#定义了一个 Python 字典（dict），用于存储中国各省级行政单位的名称及其地理坐标<br>

```
data_pair = []
```
#初始化一个空列表 data_pair，用于存储处理后的数据<br>
```
for idx, row in df.iterrows():
```
#使用 df.iterrows() 遍历 DataFrame 的每一行<br>
#iterrows() 返回一个迭代器，每行数据以 (index, Series) 的形式返回<br>
#idx：行的索引（例如 0, 1, 2, ...）<br>
#row：行的数据，以 Series 形式存储，可以通过列名访问值（例如 row['地区']<br>

    name = row['地区'].replace(' ', '')   #从当前行的 地区 列获取值，并使用 replace(' ', '') 移除字符串中的所有空格
    try:  #开始一个 try-except 块，用于捕获可能出现的异常
        value = coord[name]  #从 coord 字典中获取 name 对应的坐标值
        value.append(float(row['人口数']))
        #将 row['人口数'] 转换为浮点数（float），并追加到 value 列表中
        #row['人口数']：从 DataFrame 中获取人口数据，可能是字符串（例如 '21893095'）或数值
        #float(row['人口数'])：将人口数据转换为浮点数（例如 21893095.0）
        #value.append(...)：将人口数追加到 value 列表中       
        data_pair.append([dictcode[name], value])
        #将 [地区全称, [经度, 纬度, 人口数]] 追加到 data_pair 列表中
        #dictcode[name]：从 dictcode 字典中获取 name 对应的全称 
        #例如：dictcode['北京'] = '北京市' 
        #value：包含经度、纬度和人口数的列表（例如 [116.41667, 39.91667, 21893095.0]）
    except KeyError:  #捕获 KeyError 异常，处理 name 不在 coord 或 dictcode 字典中的情况
        if name == '现役军人':  #检查 name 是否为 '现役军人' 
            soldier = row['人口数']    #如果是 '现役军人'，将 row['人口数'] 赋值给变量 soldier
        elif name == '全国[5]':    #检查 name 是否为 '全国[5]' 
            total = row['人口数']    #如果是 '全国[5]'，将 row['人口数'] 赋值给变量 total

```
df = pd.read_csv('E:/data analyze/全国人口年龄构成.csv')  #使用 pandas 的 read_csv 函数读取一个 CSV 文件，并将结果存储到变量 df 中
df.drop(labels=[0, 1], axis=0, inplace=True)  #使用 pandas 的 drop 方法从 DataFrame 中删除指定的行
```

data_pair_age = []  #创建一个空列表，命名为 data_pair_age<br>
```
for idx, row in df.iterrows():  
```
#使用 pandas 的 iterrows() 方法遍历 DataFrame df 的每一行<br>
#iterrows() 是 pandas 的一个方法，用于逐行迭代 DataFrame<br>
#它返回一个生成器，每次迭代产生一个 (index, row) 元组，其中<br>
#index 是行的索引 <br>
#row 是一个 pandas.Series 对象，表示该行的数据，列名作为 Series 的索引<br>
```
    data_pair_age.append([row['全国人口年龄构成'].replace('其中：', ''), float(row['Unnamed: 2'])])
```
    #从每一行中提取数据，处理后添加到 data_pair_age 列表中
    #全国人口年龄构成 列（移除“其中：”）和 Unnamed: 2 列（转换为浮点数），组成 [年龄段, 数值] 对，并添加到 data_pair_age 列表中
```
df = pd.read_csv('E:/data analyze/各地区性别构成.csv') #读取 各地区性别构成.csv 文件
df.drop(labels=[0, 1], axis=0, inplace=True)  #删除前两行（索引 0 和 1）
data_pair_sex = [('男性', float(df.iloc[0, 1])), ('女性', float(df.iloc[0, 2]))]  
```
#从处理后的 DataFrame 的第一行（索引 2）中提取第 1 列和第 2 列的值（转换为浮点数）<br>
#分别与“男性”和“女性”配对，组成 [('男性', 值), ('女性', 值)] 的列表 data_pair_sex<br>
```
df = pd.read_csv('E:/data analyze/各地区每10万人口中拥有的各类受教育程度人数.csv')
```
#使用 pandas 的 read_csv 方法读取一个 CSV 文件<br>
df.drop(labels=[0], axis=0, inplace=True)<br>
#删除 DataFrame 中 第 0 行（也就是第一行，索引为 0 的那一行<br>
#axis=0 表示按“行”删除（如果是 axis=1 则表示按“列”删除）<br>
#inplace=True 表示直接在原始 df 上修改，而不是返回一个新的 DataFrame <br>
```
data_pair_edu = [
    ('大学', float(df.iloc[0, 1])/1e3),  #('大学', float(df.iloc[0, 1])/1e3)表示每 10 万人中大学学历的人数，除以 1000 得到单位为 百分比
    ('高中', float(df.iloc[0, 2])/1e3),
    ('初中', float(df.iloc[0, 3])/1e3),
    ('小学', float(df.iloc[0, 4])/1e3),
    ('其他', 100-(float(df.iloc[0, 4])+float(df.iloc[0, 3])+float(df.iloc[0, 2])+float(df.iloc[0, 1]))/1e3)
    ]
```
    #iloc[0, 1]：用整数位置索引第 0 行第 1 列，表示“某地区的大学学历人口数量（每 10 万人中）
```
data_pair_edu = [(x, round(y, 2)) for x, y in data_pair_edu]
```
#遍历 data_pair_edu 中的每个 (学历名称, 数值) 对<br>
#使用 round(y, 2) 将数值四舍五入为 小数点后两位<br>
```
data_pair #临时变量名

chart = Map3D(init_opts=opts.InitOpts(
    width='1000px',
    height='600px',
    theme='dark',
    bg_color='#000')
)
```
#创建了一个基础的 3D 地图组件 chart，设定了暗色风格、尺寸、背景颜色等参数，为接下来绘图做准备<br>

# 引用添加的地图<br>
```
chart.add_schema(
    maptype="china",
    ground_color='#999',
    shading="lambert",
    emphasis_label_opts=opts.LabelOpts(is_show=True),
    itemstyle_opts=opts.ItemStyleOpts(
        border_width=0.2,
        border_color="rgb(0,0,0)",
    ),
    light_opts=opts.Map3DLightOpts(
        main_shadow_quality='high',
        is_main_shadow=True,
        main_intensity=1,
        main_alpha=30,
        ambient_cubemap_texture='https://echarts.apache.org/examples/data-gl/asset/canyon.hdr',
        ambient_cubemap_diffuse_intensity=0.5,
        ambient_cubemap_specular_intensity=0.5
    ),
    post_effect_opts=opts.Map3DPostEffectOpts(
        is_enable=True,
        is_ssao_enable=True,
        ssao_radius=1,
        ssao_intensity=1
    )
)
```
#add_schema() 是 Map3D 的方法，用来设置地图的形状、视觉效果、光照、材质、边界等属性。<br>
#可以理解为：设置地图的“骨架”和外观，而 add() 则是把数据放进去<br>

```
chart.add(
    "GDP",
    data_pair=data_pair,
    type_="bar3D", #bar3D 让地图区域上方“长出柱子”，高度表示数值大小
    bar_size=1.5,  #控制柱子的粗细
    min_height=3,  #所有柱子的最小高度
    shading="lambert", #设置光照类型为 lambert（漫反射），可以与 add_schema() 中的光照设置协调，增强真实感
    label_opts=opts.LabelOpts(
        is_show=False,  #不默认显示文字标签
        formatter=JsCode(
            "function(data){return data.name + ': ' + data.value[2];}"),
    )
)
```
#将 data_pair 中的数据以 3D 柱状图的形式显示在中国地图上<br>
#数据图层的名字，图例中会显示为 “GDP”<br>
```
chart.set_global_opts( #为图表设置全局配置（标题、图例、颜色、视觉映射等）
    visualmap_opts=opts.VisualMapOpts( #传入一个 VisualMapOpts 对象，定义如何通过颜色表示数值
        is_show=False, #不显示 visualMap 控件面板
        type_='color',  #映射类型为 颜色（color）
        dimension=2,  #指定数据中哪一个维度作为数值维度
        min_=1e7,
        max_=1e8,  #映射值范围从 1000 万 到 1 亿
        range_color=[  #这是颜色渐变序列，从低值到高值依次变化
            '#313695',
            '#4575b4',
            '#74add1',
            '#abd9e9',
            '#e0f3f8',
            '#ffffbf',
            '#fee090',
            '#fdae61',
            '#f46d43',
            '#d73027',
            '#a50026']
    ),
    title_opts=opts.TitleOpts(
        title="全国各省人口统计",  #设置主标题文字
        subtitle='数据来自全国第七次人口普查数据，未包含港澳台地区。', #设置副标题
        pos_left="2%",  #设置标题横向位置：距离图表左边 2%
        pos_top='1%',   #设置标题纵向位置，距离图表顶部 1%
        title_textstyle_opts=opts.TextStyleOpts(color='#00BFFF', font_size=20) #设置主标题的文字样式
    ),
    tooltip_opts=opts.TooltipOpts(is_show=False), #关闭默认的 tooltip 提示框
    legend_opts=opts.LegendOpts(is_show=False),  #关闭图例显示，图例通常位于图表右上或底部
    graphic_opts=[  #ECharts 的自定义图形系统，允许你在图表上添加任意图层或装饰
        opts.GraphicGroup(  #表示“图形组合”
            graphic_item=opts.GraphicItem(id_='1', left="100px", bottom="100px"), #left="100px", bottom="100px" 表示将整个组合放在图表画布左下角往右上偏移 100px
            children=[
                opts.GraphicRect(
                    graphic_item=opts.GraphicItem(
                        left="center", top="center", z=1  # 表示该矩形位于组合中心
                    ),
                    graphic_shape_opts=opts.GraphicShapeOpts( #定义图形的尺寸
                        width=200, height=80
                    ),
                    graphic_basicstyle_opts=opts.GraphicBasicStyleOpts(
                        # 颜色配置，这里设置为黑色，透明度为0.5
                        fill="rgba(0,0,0,0.5)",
                        line_width=4,
                        stroke="#fff",
                    ),
                ),
                opts.GraphicText( #用于和 GraphicRect（矩形背景）一起组成一个“信息面板”
                    graphic_item=opts.GraphicItem(
                        left="center", top="center", z=100 #让文本在其父元素（比如一个矩形区域）水平居中
                    ),
                    graphic_textstyle_opts=opts.GraphicTextStyleOpts( #控制文本内容和样式
                        # 要显示的文本
                        text='全国人口：{:,}\n\n现役军人：{:,}'.format(int(total), int(soldier)),
                        #使用 .format() 方法，将变量 total 和 soldier 格式化为 带千位分隔符的整数
                        font="bold italic 14px Microsoft YaHei", #设置字体样式
                        graphic_basicstyle_opts=opts.GraphicBasicStyleOpts( #设置文本颜色
                            fill="#fff"
                        ),
                    ),
                )
            ],
        ),
    ]
)
```
```
pie = Pie( #设置了初始化参数，包括主题、尺寸和背景色
    init_opts=opts.InitOpts(<br>
        theme='dark',
        width='1000px',
        height='400px',
        bg_color='rgb(0,0,0)')
)
pie.add(
    "",
    data_pair_age,
    # 指定饼图中心位置
    center=["20%", "35%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
    formatter='{b}\n{c}%'：
```
#{b}：显示饼图扇区的名称（即每个分类的名称，如 "18-25岁"）；<br>
#{c}：显示饼图扇区的数值（如 25，表示百分比值）；<br>
#\n：换行，确保每个标签内容分为两行显示<br>
)<br>
```
pie.add(
    "",
    data_pair_sex,
    # 指定饼图中心位置
    center=["50%", "35%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
    #{b}：显示饼图扇区的名称（即每个分类的名称，如 "18-25岁"）；
    #{c}：显示饼图扇区的数值（如 25，表示百分比值）；
    #\n：换行，确保每个标签内容分为两行显示
)

pie.add(
    "",
    data_pair_edu,
    # 指定饼图中心位置
    center=["80%", "35%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
)
```
```
pie.add(
    "",
    [('城镇', 63.89), ('农村', 36.11)], #饼图的 数据对（data_pair）。它表示了两个类别的数据和它们对应的百分比
    # 指定饼图中心位置
    center=["20%", "70%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
)

pie.add(
    "",
    [('汉族', 91.11), ('少数民族', 8.89)],
    # 指定饼图中心位置
    center=["50%", "70%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
)

pie.add(
    "",
    [('流动人口', 26.62), ('非流动人口', 73.38)],
    # 指定饼图中心位置
    center=["80%", "70%"],
    # 将饼图尺寸相应缩小，不然饼图会重叠
    radius=["15%", "25%"],
    label_opts=opts.LabelOpts(formatter='{b}\n{c}%')
)
```
```
pie.set_global_opts( #设置图表的 全局配置选项
    legend_opts=opts.LegendOpts(is_show=False), #配置 图例（legend）的显示与样式
    title_opts=[dict(text='人口画像', left='2%', top='1%', textStyle=dict(color='#00BFFF', fontSize=20)), 
                #这里设置了多个 标题文本，并分别给它们配置了具体的位置和样式
                dict(
                    text='年龄 ',
                    left='20%',
                    top='32%',
                    textAlign='center',
                    textStyle=dict(
                        color='#fff',
                        fontWeight='normal',
                        fontSize=15)),
                dict(
                    text='性别 ',
                    left='50%',
                    top='32%',
                    textAlign='center',
                    textStyle=dict(
                        color='#fff',
                        fontWeight='normal',
                        fontSize=15)),
                dict(text='教育 ', left='80%', top='32%', textAlign='center',
                     textStyle=dict(color='#fff', fontWeight='normal', fontSize=15)),
                dict(text='户籍 ', left='20%', top='67%', textAlign='center',
                     textStyle=dict(color='#fff', fontWeight='normal', fontSize=15)),
                dict(text='民族 ', left='50%', top='67%', textAlign='center',
                     textStyle=dict(color='#fff', fontWeight='normal', fontSize=15)),
                dict(text='流动 ', left='80%', top='67%', textAlign='center',
                     textStyle=dict(color='#fff', fontWeight='normal', fontSize=15))
                ],
    graphic_opts=[  #这部分代码设置了 图形元素 (graphic) 的选项
        opts.GraphicGroup(
            graphic_item=opts.GraphicItem(
                id_='2', left="center", top="40px"),
            children=[ #这部分代码定义了图形组中的子元素，即实际显示在图表中的图形（例如矩形、文本、线条等）
                opts.GraphicRect(
                    graphic_item=opts.GraphicItem(
                        left="center", top="center", z=1
                    ),
                    graphic_shape_opts=opts.GraphicShapeOpts( #定义了一个 矩形 图形，并配置了它的 位置、尺寸 和 样式
                        width=950, height=320
                    ),
                    graphic_basicstyle_opts=opts.GraphicBasicStyleOpts( #定义矩形的宽度和高度
                        fill="rgba(0,0,0,0)",
                        line_width=5,
                        stroke="#fff",
                    ),
                )
            ],
        )
    ]
)
colors = [
    '#313695',
    '#4575b4',
    '#74add1',
    '#abd9e9',
    '#e0f3f8',
    '#ffffbf',
    '#fee090',
    '#fdae61',
    '#f46d43',
    '#d73027',
    '#a50026'
]
random.shuffle(colors)
pie.set_colors(colors)
```
#对饼图的颜色进行 随机打乱，并将打乱后的颜色应用到饼图中<br>

```
page = Page() #创建了一个 空的页面实例 page，然后你可以将多个图表添加到这个页面中
page.add(chart).add(pie) #page.add(...) 方法用于将一个或多个图表添加到页面中
page.render('./全国人口画像.html') #page.render() 方法用于 渲染并保存页面为 HTML 文件
page.render_notebook() # 在 Jupyter Notebook 中直接渲染并显示图表

df = pd.read_csv('E:/data analyze/各地区人口.csv') #使用 pandas 库中的 read_csv() 函数来读取
df.drop(labels=[0, 1, 2, 34], axis=0, inplace=True)
df.columns = ['地区', '人口数', '2020年占比', '2010年占比']
df['2020年占比'] = df['2020年占比'].astype('float') #astype('float') 将 '2020年占比' 列的数据类型转换为浮动类型（float）
df['2010年占比'] = df['2010年占比'].astype('float')
df['占比变化'] = df['2020年占比'] - df['2010年占比']
```
```
data_pair = [] #存储之后要构建的所有数据对
for idx, row in df.iterrows(): #df.iterrows() 是 pandas DataFrame 中用于遍历 DataFrame 每一行的函数。它返回一个迭代器，每次迭代会返回 行的索引（idx）和 行的数据（row）
    data_pair.append([dictcode[row['地区'].replace(' ', '')], round(row['占比变化'], 2)])
    map_chart = Map( #创建了一个 Map 类型的图表实例，赋值给变量 map_chart
        init_opts=opts.InitOpts( #init_opts：这是初始化图表时的配置选项
            theme='dark',
            width='1000px',
            height='700px',
            bg_color='#000')
    )
    map_chart.add('占比变化',
                  data_pair=data_pair,
                  maptype='china',
                  # 关闭symbol的显示
                  is_map_symbol_show=False,
                  label_opts=opts.LabelOpts(color='#fff'),
                  itemstyle_opts={
                      'normal': {
                          'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                          'shadowBlur': 5,  # 阴影大小
                          'shadowOffsetY': 0,  # Y轴方向阴影偏移
                          'shadowOffsetX': 0,  # x轴方向阴影偏移
                          'borderColor': '#fff'
                      }
                  }
                  )
    map_chart.set_global_opts( #设置了 map_chart 的 全局选项，特别是配置了 视觉映射（VisualMap） 的显示效果
        visualmap_opts=opts.VisualMapOpts(
            is_show=True,
            is_piecewise=True,
            series_index=0,
            pos_top='center',
            pos_left='2%',
            range_text=['人口占比变化：\n\n流入', '流出'],
            min_=-0.5,
            max_=0.5,
            precision=1,
            pieces=[
                {'min': 0.2, "color": 'red'},
                {'min': 0.1, 'max': 0.2, 'color': '#FF69B4'},
                {'min': 0, 'max': 0.1, 'color': '#FFB6C1'},
                {'min': -0.1, 'max': 0, 'color': '#87CEFA'},
                {'max': -0.1, 'min': -0.2, 'color': '#1E90FF'},
                {'max': -0.2, 'color': 'blue'}],
        ),

        #pieces 是一个列表，包含多个字典，每个字典表示 数值区间 对应的颜色

        legend_opts=opts.LegendOpts(is_show=False),
        tooltip_opts=opts.TooltipOpts(
            is_show=True,
            trigger='item',
            formatter='{b}:{c}%'
        ),
        title_opts=dict(
            text='2010-2020年各省人口占比变化',
            left='2%',
            top='1%',
            textStyle=dict(
                color='#00BFFF'))
    )
    #配置 图表全局选项 的一部分，涉及到图表的 图例（Legend）、提示框（Tooltip） 和 标题（Title） 配置

    map_chart.render('2010-2020年各省人口占比变化.html')
    map_chart.render_notebook()

    df = pd.read_csv('E:/data analyze/各地区人口年龄构成.csv')
    df.drop(labels=[0, 1], axis=0, inplace=True)
    df.columns = ['地区', '0—14岁', '15—59岁', '60岁及以上', '其中65岁及以上']
    df['0—14岁'] = df['0—14岁'].astype('float')
    df['15—59岁'] = df['15—59岁'].astype('float')
    df['60岁及以上'] = df['60岁及以上'].astype('float')

    bar = Bar(
        init_opts=opts.InitOpts(
            theme='dark',
            width='1000px',
            height='1000px',
            bg_color='#000')
    )
    bar.add_xaxis(
        df['地区'].tolist() #将 DataFrame 中的 "地区" 列转换为一个列表，并作为 x 轴的数据传入
    )
    bar.add_yaxis(
        '0—14岁',
        df['0—14岁'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'), #配置条形图上显示的标签
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '15—59岁',
        df['15—59岁'].tolist(), #stack=1 表示将这个系列的数据与之前的系列堆叠在一起，构成一个整体的堆叠条形图
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '60岁及以上',
        df['60岁及以上'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )
    bar.set_global_opts(
        xaxis_opts=opts.AxisOpts(is_show=False, max_=100),
        yaxis_opts=opts.AxisOpts(
            axisline_opts=opts.AxisLineOpts(is_show=False),
            axistick_opts=opts.AxisTickOpts(is_show=False)),
        legend_opts=opts.LegendOpts(is_show=True, pos_top='1%', pos_right='10%'),
        title_opts=opts.TitleOpts(
            title="全国各省人口年龄构成",
            # subtitle='数据来自全国第七次人口普查数据，未包含港澳台地区。',
            pos_left="5%",
            pos_top='1%',
            title_textstyle_opts=opts.TextStyleOpts(color='#00BFFF', font_size=20) #配置了 标题文本样式，使用了 opts.TextStyleOpts 来设置字体的颜色和大小
        ),
    )

    bar.reversal_axis() #反转坐标轴 
    bar.set_colors(['orange', 'blue', 'red']) #该方法用于设置条形图的颜色
    bar.render('./全国各省人口年龄构成.html')

    df = pd.read_csv('E:/data analyze/各地区性别构成.csv')
    df.drop(labels=[0, 1], axis=0, inplace=True)
    df.columns = ['地区', '女', '男', '性别比'] # 修改 DataFrame df 的列名
    df['男'] = df['男'].astype('float')
    df['女'] = df['女'].astype('float')
    df.sort_values(by='女', inplace=True) #用于对 DataFrame 中的数据按照指定列的值进行排序

    bar = Bar(
        init_opts=opts.InitOpts(
            theme='dark',
            width='1000px',
            height='1000px',
            bg_color='#000')
    )
    bar.add_xaxis(
        df['地区'].tolist()
    )
    bar.add_yaxis(
        '男性',
        df['男'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '女性',
        df['女'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.set_global_opts(
        xaxis_opts=opts.AxisOpts(is_show=False, min_=40, max_=60), #is_show=False：隐藏 X 轴，不显示轴线
        yaxis_opts=opts.AxisOpts(
            axisline_opts=opts.AxisLineOpts(is_show=False),
            axistick_opts=opts.AxisTickOpts(is_show=False)),
        legend_opts=opts.LegendOpts(is_show=True, pos_top='1%', pos_right='10%'),
        title_opts=opts.TitleOpts(
            title="全国各省人口性别构成",
            # subtitle='数据来自全国第七次人口普查数据，未包含港澳台地区。',
            pos_left="5%",
            pos_top='1%',
            title_textstyle_opts=opts.TextStyleOpts(color='#00BFFF', font_size=20)
        ),
    )

    bar.reversal_axis()
    bar.set_colors(['blue', 'red'])
    bar.render_notebook()

    df = pd.read_csv('E:/data analyze/各地区每10万人口中拥有的各类受教育程度人数.csv')
    df.drop(labels=[0, 1], axis=0, inplace=True)
    df.columns = ['地区', '大学', '高中', '初中', '小学']
    df['大学'] = df['大学'].astype('int') / 1e3
    df['高中'] = df['高中'].astype('int') / 1e3
    df['初中'] = df['初中'].astype('int') / 1e3
    df['小学'] = df['小学'].astype('int') / 1e3
    df['其他'] = 100 - df['大学'] - df['高中'] - df['初中'] - df['小学']
    df.sort_values(by='大学', inplace=True)

    bar = Bar(
        init_opts=opts.InitOpts(
            theme='dark',
            width='1000px',
            height='1000px',
            bg_color='#000')
    )
    bar.add_xaxis(
        df['地区'].tolist()
    )
    bar.add_yaxis(
        '大学（含大专）',
        df['大学'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '高中（含中专）',
        df['高中'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '初中',
        df['初中'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '小学',
        df['小学'].tolist(),
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.add_yaxis(
        '其他',
        [round(x, 2) for x in df['其他'].tolist()],
        stack=1,
        label_opts=opts.LabelOpts(is_show=True, formatter='{c}%', position='insideLeft'),
        itemstyle_opts={
            'normal': {
                'shadowColor': 'rgba(0, 0, 0, .5)',  # 阴影颜色
                'shadowBlur': 5,  # 阴影大小
                'shadowOffsetY': 2,  # Y轴方向阴影偏移
                'shadowOffsetX': 2,  # x轴方向阴影偏移
                'borderColor': '#fff'
            }
        }
    )

    bar.set_global_opts(
        xaxis_opts=opts.AxisOpts(is_show=False, max_=100),
        #xaxis_opts：设置 X 轴的全局属性。
        #is_show=False：隐藏 X 轴，即不显示 X 轴的轴线、刻度和标签。
        #max_=100：设置 X 轴的最大值为 100。即使 is_show=False 隐藏了 X 轴，它仍然决定了 X 轴的值域
        yaxis_opts=opts.AxisOpts(
            axisline_opts=opts.AxisLineOpts(is_show=False),
            axistick_opts=opts.AxisTickOpts(is_show=False)),
           #yaxis_opts：设置 Y 轴的全局属性。
           #axisline_opts=opts.AxisLineOpts(is_show=False)：隐藏 Y 轴的轴线，不显示 Y 轴的边界线。
           #axistick_opts=opts.AxisTickOpts(is_show=False)：隐藏 Y 轴的刻度线，不显示 Y 轴的刻度标记。
        legend_opts=opts.LegendOpts(is_show=True, pos_top='1%', pos_right='10%'),
        #legend_opts：设置图例（legend）的全局属性。
        #is_show=True：显示图例，图例用于标识图表中各个数据系列的含义。
        #pos_top='1%'：将图例放置在图表顶部，离顶部 1% 的位置。
        #pos_right='10%'：将图例放置在图表右侧，离右边 10% 的位置。
        title_opts=opts.TitleOpts(
            title="全国各省人口接受教育程度",
            # subtitle='数据来自全国第七次人口普查数据，未包含港澳台地区。',
            pos_left="5%",
            pos_top='1%',
            title_textstyle_opts=opts.TextStyleOpts(color='#00BFFF', font_size=20)
        ),
    )
    #通过设置图表的 标题（title）、标题位置（pos_left 和 pos_top）、以及 标题文本样式（title_textstyle_opts）来调整标题的显示效果

    bar.reversal_axis()
    bar.set_colors(['blue', '#1E90FF', '#87CEFA', '#FF69B4', 'red'])
    bar.render_notebook()
