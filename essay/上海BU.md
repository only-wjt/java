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

# ![表格](./attachments/1542959601190.table.html)

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

字段权限在AWS各种策略中得优先次序

FORM_BEFORE_REMOVE
表单子表记录删除前被触发

由于删除操作在底层代码中使用了数据库事务，如果在事件中需要对数据库进行操作，需要使用系统参数提供的数据库连接。否则，会出现数据库事务的问题。

该参数仅在表单子表记录删除前（后）有效

		//注意：由于使用了事务，操作数据库需要使用如下方式获取的Connection连接
        //该参数仅在表单子表记录删除前（后）有效
        Connection conn = (Connection) param.getParameter(ListenerConst.FORM_EVENT_PARAM_CONNECTION);
		
删除子表时，与数据库操作有关系，与事务有关，应该采取上面的形式


开发者应妥善保管AWS PaaS提供的私钥/口令字符串，建议在调用端加密后存至配置文件，源码中禁止含有私钥/口令信息


### 此处是DW报表，可以在报表视图中设置URL外链，挂到表单上面，下面的代码是页面实现挂在报表的代码
````
<script type="text/javascript">// <![CDATA[
window.onload = function () {
    console.log("开始");
    $(".ht1").show();
    $(".ht2").hide();
    $(".ht3").hide();     
    $(".ht4").hide();
    var sid = $("#sid").val();
    var sfz = $("#PLATE_NUMBER").val();
    $('#ht1').attr('src','./w?sid=@sid&cmd=CLIENT_DW_PORTAL&processGroupId=obj_73d4deabe4464b9cb78e6cbf14579307'+
        '&appId=com.awspaas.user.apps.sfeg.car&dwViewId=obj_f39bba3a48ea4c2c91dd9ae95ffec085&hideSearcher=true'+
        '&hideSearcher=true&hideToolbar=true&condition=[{tp:"文本",cp:"like",fd:"PLATE_NUMBEROBJ_59A7FDD8F9E24AFF9EA862CE2BCC1558",cv:"'+sfz+'"}]');            
    $("#label1").click(function () {
        $(".ht1").show();
        $(".ht2").hide();
        $(".ht3").hide(); 
        $(".ht4").hide();
    var sid = $("#sid").val();
    var sfz = $("#PLATE_NUMBER").val();
    $('#ht1').attr('src','./w?sid=@sid&cmd=CLIENT_DW_PORTAL&processGroupId=obj_73d4deabe4464b9cb78e6cbf14579307'+
        '&appId=com.awspaas.user.apps.sfeg.car&dwViewId=obj_f39bba3a48ea4c2c91dd9ae95ffec085&hideSearcher=true'+
        '&hideSearcher=true&hideToolbar=true&condition=[{tp:"文本",cp:"like",fd:"PLATE_NUMBEROBJ_59A7FDD8F9E24AFF9EA862CE2BCC1558",cv:"'+sfz+'"}]');         
    });
    $("#label2").click(function () {
        $(".ht1").hide();
        $(".ht2").show();
        $(".ht3").hide();
        $(".ht4").hide(); 
    var sid = $("#sid").val();
    var sfz = $("#PLATE_NUMBER").val();
    $('#ht2').attr('src','./w?sid=@sid&cmd=CLIENT_DW_PORTAL&processGroupId=obj_adca646102034909b6e82e876ba1410b'+
        '&appId=com.awspaas.user.apps.sfeg.car&dwViewId=obj_900e13e0e2594ae9ba29fe09b914568f&hideSearcher=true'+
        '&hideSearcher=true&hideToolbar=true&condition=[{tp:"文本",cp:"like",fd:"PLATE_NUMBEROBJ_F9E1229546A1468883084BEEFC25876F",cv:"'+sfz+'"}]');      
    });
    $("#label3").click(function () {
        $(".ht1").hide();
        $(".ht2").hide();
        $(".ht3").show();
        $(".ht4").hide();
    var sid = $("#sid").val();
    var sfz = $("#PLATE_NUMBER").val();
    $('#ht3').attr('src','./w?sid=@sid&cmd=CLIENT_DW_PORTAL&processGroupId=obj_b816a9215bf645359372e8c94f1f665d'+
        '&appId=com.awspaas.user.apps.sfeg.car&dwViewId=obj_e8c6d713e5b94103a9a47618a245b077&hideSearcher=true'+
        '&hideSearcher=true&hideToolbar=true&condition=[{tp:"文本",cp:"like",fd:"PLATE_NUMBEROBJ_2451D95B72DE47BBB1DDF601FF74FD57",cv:"'+sfz+'"}]'); 
    });
    $("#label4").click(function () {
        $(".ht1").hide();
        $(".ht2").hide();
        $(".ht3").hide(); 
        $(".ht4").show(); 
    var sid = $("#sid").val();
    var sfz = $("#PLATE_NUMBER").val();
    $('#ht4').attr('src','./w?sid=@sid&cmd=CLIENT_DW_PORTAL&processGroupId=obj_4cc0323fc2ab4992bd4026a0f6554623'+
        '&appId=com.awspaas.user.apps.sfeg.car&dwViewId=obj_134608f8a8c043c0acf62c425045b5bc&hideSearcher=true'+
        '&hideSearcher=true&hideToolbar=true&condition=[{tp:"文本",cp:"like",fd:"PLATE_NUMBEROBJ_32344863B5DB45DEA4EDC3EAA1D21CC6",cv:"'+sfz+'"}]');
    });
}
// ]]></script>
````

## 本地许可过期解决方法

删除c盘目录下的用户目录中的.fe285ab55cf5e这样一串乱码的文件夹删除即可

![本地平台许可过期解决方法](https://www.github.com/only-wjt/images/raw/master/小书匠/本地许可过期解决方法.png)


## webservice配置使用方法

![webservice密码配置](https://www.github.com/only-wjt/images/raw/master/小书匠/webservice密码配置.png)
![webservice配置密码](https://www.github.com/only-wjt/images/raw/master/小书匠/webservice配置密码.png)
![密码配置](https://www.github.com/only-wjt/images/raw/master/小书匠/密码配置.png)
![sopa密码配置](https://www.github.com/only-wjt/images/raw/master/小书匠/soap密码配置.png)

详细代码请看本地E:\上海建工\上海建工\sfeg_ykt


## 年份(当前年份的前五年和后五年)
@numSub(@year,5)|@numSub(@year,4)|@numSub(@year,3)|@numSub(@year,2)|@numSub(@year,1)|@numAdd(@year)|@numAdd(@year,1)|@numAdd(@year,2)|@numAdd(@year,3)|@numAdd(@year,4)|@numAdd(@year,5)


## 部门字段可以多选并且可以以列表形式选填
![多选部门，还可以手填](https://www.github.com/only-wjt/images/raw/master/小书匠/1548294626957.png)

其中sql语句为：SELECT DEPARTMENTNAME AS org_name,DEPARTMENTNO AS org_code FROM ORGDEPARTMENT


## 根据集团外部和内部做不同而sql查询

SELECT DOCUMENT_NO FROM BO_EU_GW_FHLX WHERE ('$ORG_TYPE' = '02' AND (DOCUMENT_TYPE = '$DOCUMENT_TYPE' AND COMPANY_NAME='$COMPANY_NAME')) or ('$ORG_TYPE'='01' AND ( COMPANY_NAME = '' or COMPANY_NAME='$COMPANY_NAME'))

SELECT DOCUMENT_NO FROM BO_EU_GW_FHLX WHERE ('02' = '02' AND (DOCUMENT_TYPE = '1' AND COMPANY_NAME='第二工程公司')) or ('02'='01')