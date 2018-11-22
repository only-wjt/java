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


FormGridRowLookAndFeel？？？