OA系统中的立项流程
==================
流程图
-----------
.. graphviz::

    digraph 信息系统中的立项流程{
      {
        创建 [shape=box fontsize=8 label="1 创建 | 立项小组组长"];
        会签 [shape=box fontsize=8 label="2 会签 | 项目成功必要的关键领域分管领导"];
        投资委员会决策 [shape=box fontsize=8 label="3 投资委员会决策 | 投资委员会会议秘书"];
        批准 [shape=box fontsize=8 label="4 批准 | 董事长"];
        通知/归档 [shape=box fontsize=8 label="5 通知/归档"];
      }
        创建 -> 会签;
        会签 -> 投资委员会决策;
        投资委员会决策 -> 批准;
        批准 -> 通知/归档;
    }

.. graphviz::

    digraph 立项{
      {
        发起立项建议 [shape=box fontsize=8 label="1 发起立项建议"];
        成立立项小组 [shape=box fontsize=8 label="2 成立立项小组"];
        编制立项申请报告 [shape=box fontsize=8 label="3 编制立项申请报告"];
        召开立项评审会 [shape=box fontsize=8 label="4 召开立项评审会"];
        完成立项 [shape=box fontsize=8 label="5 完成立项"];
      }
        发起立项建议 -> 成立立项小组;
        成立立项小组 -> 编制立项申请报告;
        编制立项申请报告 -> 召开立项评审会;
        召开立项评审会 -> 完成立项;
    }



其中：会签步骤中，项目成功必要的关键领域分管领导一般为参加立项评审会议的成员。投资委员会决策步骤中，投资委员会会议秘书在向投资委员会提交议题并获得批准后完成节点。
