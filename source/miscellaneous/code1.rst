自动化周报sample
-----------------

.. code:: python

   import streamlit as st
   import pandas as pd
   import datetime as dt
   import os
   
   
   date = dt.date.today()
   st.markdown(date)
   
   st.title('产品中心周报')
   
   
   for file in os.listdir():
       if file.endswith('.xlsx'):
           name = os.path.splitext(file)[0]
           df_plan = pd.read_excel(file, sheet_name='日程表')
   #        for x in df_plan.index:
   #            df_plan1 = df_plan.fillna('0')
   #            df_plan.loc[x, '完成度'] = int(df_plan1.loc[x, '完成度'])
   #            df_plan = df_plan.fillna('')
           df_record = pd.read_excel(file, sheet_name='记录').fillna('')
           df_record = df_record[df_record['更新时间'] >= dt.datetime(2022, 5, 20)]
           df_risk = pd.concat([df_record[df_record['状态'] == '危险'], df_record[df_record['状态'] == '警告']])
           df_normal = pd.concat([df_record[df_record['状态'] == '正常'], df_record[df_record['状态'] == '']])
           df_next = df_plan[(df_plan['计划开始时间'] <= dt.datetime(2022, 6, 4)) & (df_plan['完成度'] < 1.0)]
           for y in df_next.index:
               df_next.loc[y, '计划开始时间'].strftime('%Y-%m-%d')
               df_next.loc[y, '计划结束时间'].strftime('%Y-%m-%d')
           st.header(name)
           st.header('风险')
           for i in df_risk.index:
               wbs = df_record.loc[i, 'WBS']
               workpkg = df_record.loc[i, '工作包']
               start = df_record.loc[i, '计划开始时间'].strftime('%Y-%m-%d')
               end = df_record.loc[i, '计划结束时间'].strftime('%Y-%m-%d')
               com = str(df_record.loc[i, '完成度'])
               status = df_record.loc[i, '状态']
               record = df_record.loc[i, '记录']
               head = wbs + '  ' + workpkg + '  ** ' + status + ' **'
               body = '已完成：' + com + '  ' + '计划开始于：' + start + '  ' + '计划完成于：' + '  ' + end
               tail = '进展：' + record
               st.write('- ' + head + '  ' + '|' + '  ' + body)
               st.write(tail)
           st.header('其他')
           for j in df_normal.index:
               wbs = df_normal.loc[j, 'WBS']
               workpkg = df_normal.loc[j, '工作包']
               start = df_normal.loc[j, '计划开始时间'].strftime('%Y-%m-%d')
               end = df_normal.loc[j, '计划结束时间'].strftime('%Y-%m-%d')
               com = str(df_normal.loc[j, '完成度'])
               status = df_normal.loc[j, '状态']
               record = df_normal.loc[j, '记录']
               head = str(wbs) + '  ' + str(workpkg) + '  **' + str(status) + '**'
               body = '已完成：' + com + '  ' + '计划开始于：' + start + '  ' + '计划完成于：' + '  ' + end
               tail = '进展：' + record
               st.write('- ' + head + '  ' + '|' + '  ' + body)
               st.write(tail)
           st.header('下一步工作')
           for k in df_next.index:
               wbs = df_next.loc[k, 'WBS']
               workpkg = df_next.loc[k, '工作包']
               start = df_next.loc[k, '计划开始时间'].strftime('%Y-%m-%d')
               end = df_next.loc[k, '计划结束时间'].strftime('%Y-%m-%d')
               com = str(df_next.loc[k, '完成度'])
               status = df_next.loc[k, '状态']
               head = str(wbs) + '  ' + str(workpkg) + '  **' + str(status) + '**'
               body = '已完成：' + com + '  ' + '计划开始于：' + start + '  ' + '计划完成于：' + '  ' + end
               st.write('- ' + head + '  ' + '|' + '  ' + body)
           st.header('进度计划')
           st.image(name+'.png')
   

另一个less版本

.. code:: python
   
   import streamlit as st
   import pandas as pd
   import datetime as dt
   import os
   
   
   date = dt.date.today()
   st.markdown(date)
   
   st.title('产品中心周报')
   
   
   
   for file in os.listdir():
       if file.endswith('.xlsx'):
           name = os.path.splitext(file)[0]
           df_plan = pd.read_excel(file, sheet_name='日程表')
   #        for x in df_plan.index:
   #            df_plan1 = df_plan.fillna('0')
   #            df_plan.loc[x, '完成度'] = int(df_plan1.loc[x, '完成度'])
   #            df_plan = df_plan.fillna('')
           df_record = pd.read_excel(file, sheet_name='记录').fillna('')
           df_record = df_record[df_record['更新时间'] >= dt.datetime(2022, 5, 20)]
           df_risk = pd.concat([df_record[df_record['状态'] == '危险'], df_record[df_record['状态'] == '警告']])
           df_normal = pd.concat([df_record[df_record['状态'] == '正常'], df_record[df_record['状态'] == '']])
           df_next = df_plan[(df_plan['计划开始时间'] <= dt.datetime(2022, 6, 4)) & (df_plan['完成度'] < 1.0)]
           for y in df_next.index:
               df_next.loc[y, '计划开始时间'].strftime('%Y-%m-%d')
               df_next.loc[y, '计划结束时间'].strftime('%Y-%m-%d')
           st.header(name)
           st.header('进度计划')
           st.image(name+'.png')
           st.header('风险')
           for i in df_risk.index:
               wbs = df_record.loc[i, 'WBS']
               workpkg = df_record.loc[i, '工作包']
               start = df_record.loc[i, '计划开始时间'].strftime('%Y-%m-%d')
               end = df_record.loc[i, '计划结束时间'].strftime('%Y-%m-%d')
               com = df_record.loc[i, '完成度']
               status = df_record.loc[i, '状态']
               action1 = df_record.loc[i, '防范策略']
               action2 = df_record.loc[i, '应对策略']
               record = df_record.loc[i, '记录']
               line1 = str(wbs) + '  ' + str(workpkg) + '  ** ' + str(status) + ' **'
               line2 = '已完成：' + str(com) + '  ' + '计划开始于：' + str(start) + '  ' + '计划完成于：' + '  ' + str(end)
               line3 = '** 防范策略 **： ' + str(action1) + '** 应对策略 **： ' + str(action2)
               line4 = '进展：' + str(record)
               st.write(line1)
               st.write(line2)
               st.write(line3)
               st.write(line4)
   