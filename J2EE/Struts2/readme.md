Struts2学习记录

1.配置Struts2的源代码和文档
struts2-core-2.3.7.jar 
源代码在 /src/core/src/main/java
文档在 /docs/struts2-core/apidocs/

2.配置struts.xml的自动完成提示
winrar打开struts2-core-2.3.7.jar 把struts-2.0.dtd解压出来。
打开myeclipse window-pregerences搜索catalog点击最下面的xml catalog ，在右面add一个Location 选择本地的struts-2.0.dtd keytype选URI，key填在struts.xml中复制的http://struts.apache.org/dtds/struts-2.0.dtd 确定。

3.建立struts的dev-mode，好处在于我们修改了配置文件的时候能够自动热替换 
设置struts.xml
<constant name="struts.devMode" value="true" />

4.struts.xml中 action的name="success"，默认为success
	 <package name="default" namespace="/" extends="struts-default">
        <action name="hello"><!--tomcat区分action的大小写 -->
            <result>
                /Hello.jsp
            </result><!--name=“success”可以不写，默认为success -->
        </action>
    </package>
 	如果namespace为空且含同名action优先执行namespace不为空的.
 	namespace决定了action的访问路径，默认为""，可以接收所有路径的action
	namespace可以写为/，或者/xxx，或者/xxx/yyy，对应的action访问路径为/index.action， /xxx/index.action，或者/xxx/yyy/index.action.
	namespace最好也用模块来进行命名
    <package name="main" extends="struts-default" namespace="">
        <action name="index">
            <result>/Namespace1.jsp</result>
        </action>
    </package>
    <package name="front" extends="struts-default" namespace="/front">
        <action name="index">
            <result>/Namespace.jsp</result>
        </action>
    </package>
5.struts.xml中package的namespace可以不写,意味着在任意路径访问action的name都可以访问到这个action，如果加了，就是在webapp路径后面+namespace+action的name来访问。

6.在myeclipse中直接ctrl+c，ctrl+v复制项目的时候一定要在改一下项目名字。
操作方法：项目右击properties——myeclipse-web 重新设置web-content-root为新项目名字。

7.设置新建jsp的默认编码 window-preferences-搜索jsp 点击下面的jsp，在encoding里选Chinese,National Standard

8.Action执行的时候并不一定要执行execute方法
可以在配置文件中配置Action的时候用method=来指定执行哪个方法也可以在url地址中动态指定（动态方法调用DMI）（推荐）
<br />
    <a href="<%=context %>/user/userAdd">添加用户</a>
    <br />
    <a href="<%=context %>/user/user!add">添加用户</a>
    <br />
这里user后面的!后面跟的事方法，前者会产生太多的action，所以不推荐使用 

9使用通配符时如果能同时匹配多个带*的action,则按照先后出现的顺序调用。如果有name为具体的action，则优先调用。
    <package name="actions" extends="struts-default" namespace="/actions">
        <action name="Student*" class="com.bjsxt.struts2.action.StudentAction" method="{1}">
            <result>/Student{1}_success.jsp</result>
        </action>
        
        <action name="*_*" class="com.bjsxt.struts2.action.{1}Action" method="{2}">
            <result>/{1}_{2}_success.jsp</result>
            <!-- {0}_success.jsp -->
        </action>
    </package>