OA系统中的立项流程
==================
流程图
-----------
.. graphviz::

    digraph 信息系统中的立项流程{
      {
        创建 [shape=box fontsize=8 label="1 创建"];
        会签 [shape=box fontsize=8 label="2 会签"];
        投资委员会决策 [shape=box fontsize=8 label="3 投资委员会决策"];
        批准 [shape=box fontsize=8 label="4 批准"];
        通知&归档 [shape=box fontsize=8 label="5 通知&归档"];
      }
        创建 -> 会签;
        会签 -> 投资委员会决策;
        投资委员会决策 -> 批准;
        批准 -> 通知&归档;
    }




其中：会签步骤中，项目成功必要的关键领域分管领导一般为参加立项评审会议的成员。投资委员会决策步骤中，投资委员会会议秘书在向投资委员会提交议题并获得批准后完成节点。
