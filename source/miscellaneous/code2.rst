Visual plan
============

step
-----

.. graphviz::

    digraph abc{
        a哦;
        额;
        c;
        度;

        a哦 -> 额;
        额 -> 度;
        c -> 度;
    }



.. graphviz::

    digraph mind{
        读取excel;
        保存projectname;
        读取excel;
        新增projectname;
        写入csv;
        清理csv中的重复值;
        读取csv;
        绘制甘特图;
        绘制表格;
        筛选status;

        读取excel -> 保存projectname;
        保存projectname -> 读取excel;
        读取excel -> 新增projectname;
        新增projectname -> 写入csv;
        写入csv -> 清理csv中的重复值;
        清理csv中的重复值 -> 读取csv;
        读取csv -> 绘制甘特图;
        读取csv -> 筛选status;
        筛选status -> 绘制表格;
    }


sample
-------

.. code:: python

    import pandas as pd
    import plotly.figure_factory as ff
    import plotly.graph_objects as go


    df = pd.read_excel('demo-proj.xlsx', header=None)
    print(df.loc[0][3])
    # names=df.iloc[3]
    # df = df[3:].copy()
    # df = df.rename(columns=names)
    # df = pd.read_excel('demo-proj.xlsx', header=None)
    # names=df.iloc[3]
    # df = df[4:].copy()
    # df = df.rename(columns=names)

    df = pd.read_excel('demo-proj.xlsx', skiprows = [0, 1, 2], parse_dates = [10, 11], date_parser = x: pd.to_datetime(x, format = '%Y/%m/%d'))

    df.to_csv('demo.csv', index = False, sep = ',', encoding = 'utf_8_sig')
    

    fig = ff.create_gantt(df)
    fig.update_yaxes(autorange="reversed")
    fig.show()

    fig1 = go.Figure(data=[go.Table(
        header=dict(values=list(df.columns),
                fill_color='paleturquoise',
                align='left'),
        cells=dict(values=[df.status, df.Risk],
               fill_color='lavender',
               align='left'))
    ])
    fig1.show()