---
title: 上海BU
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


权限组，中的组织机构依赖于岗位角色，并且会用到权限服务中的角色权限设置。


可以获取表单中的字段里面的值，，同时可以把值放到页面中存到Map中，之前做的在数据库中获取图片可以这样做
````
public class TestFormAfterLoad extends ExecuteListener {

    public void execute(ProcessExecutionContext param) throws Exception {
        //参数获取
        //注意：除特殊说明外，下列参数仅在该事件中场景有效
        //记录ID
        String boId = param.getParameterOfString(ListenerConst.FORM_EVENT_PARAM_BOID);
        //表单ID
        String formId = param.getParameterOfString(ListenerConst.FORM_EVENT_PARAM_FORMID);
        //BO表名
        String boName = param.getParameterOfString(ListenerConst.FORM_EVENT_PARAM_BONAME);

        //获取表单所有的标签
        Map<String, Object> macroLibraries = param.getParameterOfMap(ListenerConst.FORM_EVENT_PARAM_TAGS);

        String fieldHtml = (String) macroLibraries.get("FIELDNAME");//一个字段名
        if (UtilString.isEmpty(fieldHtml)) {
            //说明没有该标签
        } else {
            //1.可将HTML片段根据需求处理放回
            //2.可将该标签改为一个图片
            fieldHtml = "<img id='FIELDNAME' name='FIELDNAME' src=''/>";
            macroLibraries.put("FIELDNAME", fieldHtml);
        }
    }

}
````


FormGridRowLookAndFeel？？？  BOModel？？？


PROCESS_FORM_GRID_FILTER这个内容还挺多的

````
@Override
public FormGridRowLookAndFeel acceptRowData(ProcessExecutionContext context, List<BOItemModel> boItemList, BO boData) {
    String tableName = context.getParameterOfString(ListenerConst.FORM_EVENT_PARAM_BONAME);
    if (tableName.equals("BO_ACT_PUTONGSUB1")) {
        //创建一个对象
        FormGridRowLookAndFeel diyLookAndFeel = new FormGridRowLookAndFeel();
        String s3 = boData.getString("S3");
        String s4 = boData.getString("S4");
        if (s3 != null && s3.equals("50")) {
            diyLookAndFeel.setLink(false);// 设置这行数据不展示链接
            diyLookAndFeel.setRemove(false);// 设置这行数据不允许删除
            diyLookAndFeel.setCellCSS("style='background-color:yellow;font-color: ffffff;font-weight: bold;height: 125px'");
        }
        if (s3 != null && s3.equals("55")) {
            diyLookAndFeel.setDisplay(false);//不显示这条数据
        }
        if (s4 != null && s4.equals("60")) {
            boData.set("S1", "<img src='../commons/img/add1_16.png' border=0>" + s4);// 重新设定一个字段的值
        }
        //特别说明：
        //如果控制字段子表的`编辑`和`明细`链接的文字的显示隐藏
        //可判断字段名是否是`字段子表`UI组件，然后进行赋值
        //赋值规则：`编辑|明细`
        //如果不显示`编辑`，将该部分留空，如果不显示`明细`，将该部分留空
        //如：`|明细`，`编辑|`，`|`
        boData.set("字段子表字段名", "编辑|明细");

        //处理好之后，将该对象返回
        return diyLookAndFeel;
    }
    return null;//返回null按照原始数据展示子表
}
````

还可以进行子表字段的排序

场景2：指定子表排序
````
@Override
public String orderByStatement(ProcessExecutionContext context) {
    return "field1 asc,field2 desc";//两种字段组合的排序
    //return "field2 desc";//单个字段排序
}
````
场景3：自定义子表的表头Html
场景4：自定义AJAX子表的表头JSON

![流程表单事件](https://www.github.com/only-wjt/images/raw/master/小书匠/1542856184282.png)