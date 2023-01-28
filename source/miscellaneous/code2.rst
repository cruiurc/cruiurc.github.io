Visual plan
============

step
-----




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
        保存projectname -> 新增projectname;
        新增projectname -> 写入csv;
        写入csv -> 清理csv中的重复值;
        清理csv中的重复值 -> 读取csv;
        读取csv -> 绘制甘特图;
        读取csv -> 筛选status;
        筛选status -> 绘制表格;
    }

创建成本分析表，创建进度分析表，

df_count2['budget_sum']=df_count2['budget'].cumsum(axis=0)

code
-----
::
    import pandas as pd
    import plotly.figure_factory as ff
    import plotly.graph_objects as go
    import plotly.express as px
    from dash import Dash, html, dcc
    app = Dash(__name__)
    #读取excel，保存projectname;
    path = '/mnt/c/Users/crui/Desktop/learn/demo-proj.xlsx'
    source = '/mnt/c/Users/crui/Desktop/learn/data.csv'
    projectName = pd.read_excel(path, header=None).loc[0][3]
    #读取excel中的数据域;
    df_project = pd.read_excel(path, skiprows = [0, 1, 2], parse_dates = [10, 11], date_parser = lambda x: pd.to_datetime(x, format = '%Y/%m/%d'))
    #新增projectname;
    df_project['project'] = projectName
    #读取csv
    #df = pd.read_csv(source,  parse_dates = [10, 11], date_parser = lambda x: pd.to_datetime(x, format = '%Y/%m/%d'))
    #合并df并清理重复数据
    #df = pd.concat([df, df_project], axis=0)
    #drop_index = df.loc[df.duplicated(subset=['project', 'Task', 'wbs'], keep='last')].index
    #df.drop(labels=drop_index)
    df = df_project
    #将清理后结果写入csv
    df.to_csv(source, index = False, sep = ',', encoding = 'utf_8_sig')
    #绘制甘特图;
    #fig_gantt = ff.create_gantt(df, bar_width=0.4, height=850, showgrid_y=True, show_colorbar=True, colors='Resource')
    fig_gantt = px.timeline(df, x_start="Start", x_end='Finish', y='Task', color='Complete')
    fig_gantt.update_yaxes(autorange="reversed")
    #整理风险数据
    df_risk = df[df['status'].isin(['Yellow', 'Red'])]
    df_risk.to_csv('/mnt/c/Users/crui/Desktop/learn/'+projectName+'-risk.csv')
    #绘制风险表格;
    fig_risk = go.Figure(data=[go.Table(
        columnorder = [1, 2, 3, 4, 5],
        columnwidth = [20, 50, 50, 100, 100],
        header=dict(
            values=['WBS', 'Task', 'Status', 'note', 'Risk'],
            line_color='darkslategray',
            fill_color='royalblue',
            align=['left','center'],
            font=dict(color='white', size=12),
            height=40
        ),
        cells=dict(
            values=[df_risk.wbs, df_risk.Task, df_risk.status,df_risk.note, df_risk.risk],
            fill=dict(color=['paleturquoise', 'white']),
            align=['left', 'center'],
            font_size=12,
            height=30
        )
    )])
    #创建成本分析表
    df_cost = df.set_index('Finish').resample('w').sum()
    df_cost['budget_sum']=df_cost['budget'].cumsum(axis=0)
    df_cost['cost_sum']=df_cost['cost'].cumsum(axis=0)
    df_cost.to_csv('/mnt/c/Users/crui/Desktop/learn/'+projectName+'-report.csv')
    #绘制成本曲线
    fig_cost = go.Figure()
    fig_cost.add_trace(go.Scatter(x=df_cost.index, y=df_cost['budget_sum'], mode='lines+markers+text'))
    fig_cost.add_trace(go.Scatter(x=df_cost.index, y=df_cost['cost_sum'], mode='lines+markers+text'))
    app.layout = html.Div(children=[
        html.H1(children='Hello Dash'),
        html.Div(children='''
            Dash: A web application framework for your data.
        '''),
        dcc.Graph(
            id='example-table',
            figure=fig_gantt
        ),
        dcc.Graph(
            id='example-table1',
            figure=fig_risk
        ),
        dcc.Graph(
            id='example-table2',
            figure=fig_cost
        )
    ])
    if __name__ == '__main__':
        app.run_server(debug=True)
    
    


callback
---------
::

from dash import Dash, dcc, html, Input, Output
import plotly.express as px

import pandas as pd

app = Dash(__name__)

app.layout = html.Div([
    html.Div([

        html.Div([
            dcc.Dropdown(
                ['demo-proj', 'demo-proj1'],
                id='xaxis-column'
            )
        ])
    ])

    dcc.Graph(id='indicator-graphic')
])

@app.callback(
    Output('indicator-graphic', 'figure'),
    Input('xaxis-column', 'value'),
)

def update_graph(xaxis_column_name):
    path = '/mnt/c/Users/crui/Desktop/learn/' + xaxis_column_name +'.xlsx'
    pd.read_excel(path, skiprows = [0, 1, 2], parse_dates = [10, 11], date_parser = lambda x: pd.to_datetime(x, format = '%Y/%m/%d'))
    fig_gantt = px.timeline(df, x_start="Start", x_end='Finish', y='Task', color='Complete')
    fig_gantt.update_yaxes(autorange="reversed")
    return fig_gantt

if __name__ == '__main__':
    app.run_server(debug=True)











sample(expired)
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