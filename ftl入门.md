# freemarker语法介绍及其入门教程实例  #

## FreeMarker标签使用  ##
###一、FreeMarker模板文件主要有4个部分组成</br>
####  &#8195;1、文本，直接输出的部分  
####  &#8195;2、注释，即<#--...-->格式不会输出  
####  &#8195;3、插值（Interpolation）：即${..}或者#{..}格式的部分,将使用数据模型中的部分替代输出 
&#8195;插值规则  
&#8195;  FreeMarker的插值有如下两种类型  
&#8195; 1、通用插值：${expr}  
  
&#8195;通用插值，有可以分为四种情况  
&#8195;a、插值结果为字符串值：直接输出表达式结果  
&#8195;b、插值结果为数字值：根据默认格式(#setting 指令设置)将表达式结果转换成文本输出。可以使用内建的字符串函数格式单个插值，例如

    <#setting number_format = "currency" />  
    <#assign price = 42 />  
    ${price}  
    ${price?string}  
    ${price?string.number}  
    ${price?string.currency}  
    ${price?string.percent}

&#8195;c、输出值为日期值：根据默认格式(由 #setting 指令设置)将表达式结果转换成文本输出，可以使用内建的字符串函数格式化单个插值，例如

    <#assign lastUpdated = "2009-01-07 15:05"?datetime("yyyy-MM-dd HH:mm") />  
    ${lastUpdated?string("yyyy-MM-dd HH:mm:ss zzzz")};  
    ${lastUpdated?string("EEE,MMM d,yy")};  
    ${lastUpdated?string("EEEE,MMMM dd,yyyy,hh:mm:ss a '('zzz')'")};  
    ${lastUpdated?string.short};  
    ${lastUpdated?string.long};  
    ${lastUpdated?string.full}; 

&#8195;d、插值结果为布尔值

    <#assign foo=true />
    ${foo?string("是foo","非foo")}

&#8195;  2、数字格式化插值：#{expr}或者#{expr;format} 

&#8195;数字格式化插值  
&#8195;数字格式化插值可采用#{expr;format}的形式来格式化数字，其中format可以是：  
&#8195;mX:小数部分最小X位  
&#8195;MX:小数部分最大X位  
&#8195;例如：

    <#assign x = 2.582 />  
    <#assign y =4 />  
    #{x;M2};  
    #{y;M2};  
    #{x;m1};  
    #{y;m1};  
    #{x;m1M2};  
    #{y:m1M2}; 
 
####  &#8195;4、FTL指令：FreeMarker指令，和HTML标记类似，名字前加#予以区分，不会输出。
###二、表达式
&#8195;表达式是FreeMarker的核心功能。表达式放置在插值语法（${...}）之中时，表面需要输出表达式的值，表达式语法也可以与FreeMarker标签结合，用于控制输出  
####1、直接指定值
&#8195;例如：
&#8195;a、字符串

    ${'我的名字是\"yeek\"'};
    ${"我的文件保存在d：\\盘"};

&#8195;b、数值  
&#8195;c、布尔值  
&#8195;d、日期型  
&#8195;FreeMarker支持date、time、datetime三种类型，这三种类型的值无法直接指定，通常需要借助字符串的date、time、datetime三个内建函数进行转换才可以

    <#assign test1 = "2009-01-22"?date("yyyy-MM-dd") /> 
    <#assign test2 ="16:34:43"?time("HH:mm:ss") />  
    <#assign test2 = "2009-01-22 17:23:45"?datetime("yyyy-MM-dd HH:mm:ss") />  
    ${test1?string.full}

&#8195;e、集合  
&#8195;集合以方括号包括，各集合元素之间以英文逗号(,)分隔，看如下的示例:

    <#list ["星期一","星期二","星期三","星期四","星期五"] as x>
        ${x}
    </#list>

&#8195;f、Map集合  
&#8195;Map对象使用花括号包括，Map中的key-value对之间以英文冒号(:)隔开，多组key-value对之间以英文逗号(,) 隔开，例如

    <#assign score = {"语文":78,"数学":83,"Java":89} />
    <#list score?keys as key>
        ${key}--->${score[key]};
    </#list>
####2、输出变量值
&#8195;FreeMarker的表达式输出变量时，这些变量可以是顶层变量，也可以是Map对象中的变量，还可以是集合中的变量，并可以使用点(.)语法来访问Java对象的属性  
&#8195;a、顶层变量
    
	java

    Map root = new HashMap();  
    root.put("name","wenchao");
	
	ftl
	
	${name}

&#8195;对应顶层变量，直接使用${variableName}来输出变量值，变量名只能是数字、字母、下划线、$、@和#的组合，并不能以数字开头 
     
&#8195;b、输出集合元素
&#8195;如果需要输出集合元素，则可以根据集合元素的索引来输出元素。集合元素的索引以方括号指定。  
&#8195;假设有集合对象为：["星期一","星期二","星期三","星期四","星期五","星期六"],该集合对象名为week, 如果需要输出星期三，则可以使用如下语法：${week[2]}，集合里的第一个元素的索引是0  
&#8195;c、输出Map元素
&#8195;这里的Map对象可以是直接HashMap的实例，甚至包括 JavaBean实例，对应JavaBean实例，我们一样可以把其当成属性为key，属性为value的Map实例          
####3、字符串操作
&#8195;a、字符串链接  
&#8195;字符串连接有两种语法  
&#8195;A、使用${..}(或#{..})在字符串常量部分插入表达式的值，从而完成字符串连接  
&#8195;B、直接使用连接运算符（+）来连接字符串  
&#8195;使用第一种语法来连接字符串

    <#assign word = "word12" />
    ${"Hello,${word}!"}
&#8195;第二种使用连接符号来连接字符串
    
    ${"Hello," + "${word}" + "!"}
&#8195;值的注意的是，${..}只能用于文本部分，因此，下面的代码是错误的：

    <#if ${isBig}>Wow!</#if>
    <#if "${isBig}">Wow!</#if>
&#8195;应该写成：

    <#if isBig>Wow!</#if>
&#8195;b、截取字符串
 
    <#assign domain = "www.zuidaima.com" />   
    ${domain[0]}  
    ${domain[4]}  
    ${domain[1..4]}
####4、集合连接运算符
&#8195;这里所说的集合连接运算时将两个集合连接成一个新的集合，连接集合的运算符是+，例如

    <#list ["星期一"," 星期二","星期三"]+["星期四","星期五"] as x>
    	${x}
    </#list>
        
&#8195;5、Map连接运算符  
&#8195;Map对象的连接运算也是将两个Map对象连接成一个新的Map对象，Map对象的连接运算符是+。如果两个Map对象具有相同的key，则后加入Map里的key所对应的value替代原来key所对应的value
         
####6、算术运算符
&#8195;FreeMarker表达式中完全支持算术运算，FreeMarker支持的算术运算符包括： +，-，*，/，%  
&#8195;看如下代码示范

    <#assign x = 5 />  
    ${x* -100}  
    ${x/2}  
    ${12%10} 

&#8195;在表达式中使用算术运算时要注意以下几点。  
&#8195;A、运算符两边的运算数必须是数字，因此下面的代码是错误的：

    ${3*"5"}
&#8195;B、使用+（既可以作为加号，也可以作为字符串连接运算符）运算时，如果一边是数字，一边是字符串，就会自动将数字转化为字符串。例如

    ${3+"5"}
&#8195;输出结果：35
&#8195;C、使用内建的int函数可对数值取整。例如

    <#assign x = 5>  
    ${(x/2)?int}  
    ${1.1?int}  
    ${1.999?int}  
    ${-1.9999?int}  
    ${-1.1?int}  
     
####7、比较运算符
&#8195;表达式中支持的比较运算符有如下几个  
&#8195;a、=（或者==）:判断两个值是否相等  
&#8195;b、!=:判断两个值是否不相等  
&#8195;c、 >(或者gt):判断坐标值是否大于右边值  
&#8195;d、 >=(或者gte):判断坐标值是否大于等于右边值  
&#8195;e、 <(或者lt):判断左边值是否小于右边值  
&#8195;f、 <=(或者lte):判断左边值是否小于等于右边值       
            
####8、逻辑运算符
&#8195;逻辑运算符有如下几个  
&#8195;a、逻辑与：&&  
&#8195;b、逻辑或：||  
&#8195;c、逻辑非：!  
&#8195;逻辑运算符只能作用于布尔值，否则将产生错误。
          
####9、内建函数
&#8195;FreeMarker还提供了一些内建函数来转换输出，可以在任何变量后紧跟?,?后紧跟内建函数，就可通过内建函数来转换输出变量  

*	下面是常用的内建的字符串函数  
&#8195;a、html：对字符串进行HTML编码  
&#8195;b、cap_first:将字符串第一个字母成大写  
&#8195;c、lower_case:将字符串转换成小写  
&#8195;d、upper_case:将字符串转换成大写  
&#8195;e、trim: 去掉字符串前后的空白字符  
*	下面是集合的常用的内建函数  
&#8195;a、size： 获得序列中元素的数目         
*	下面是数字值的常用的内建函数  
&#8195;a、int 取得数字的整数部分，例如

        <#assign test="Tom & Jerry" />  
    	${test?html}  
    	${test?upper_case?html}  
          
####10、空值处理运算符
&#8195;FreeMarker对空值的处理非常严格，FreeMarker的变量必须有值，没有被赋值的变量就会抛出异常。  
&#8195;FreeMarker的空值和默认值:

    Welcome ${user!}
    Welcome ${user!'your name'}     
###三、FreeMarker 的常用指令
####1、if指令
&#8195;分支控制语句，语法格式如下

    <#if condition>  
     ....  
    <#elseif condition2>  
       ...  
    <#elseif condition3>
       ...  
    <#else>  
       ...  
    </#if>  
####2、switch、case、default、break指令
    
    <#switch value>  
       <#case refValue>  
      ...  
      <#bread>  
       <#case refValue>  
      ...  
      <#bread>  
       <#default>  
      ...  
    </#switch>  
&#8195;虽然FreeMarker提供了switch指令，但它并不推荐使用switch指令来控制也输出，而是推荐使用FreeMarker的if..elseif..else 指令来替代它。
        
####3、list、break指令
&#8195;list指令时一个典型的迭代输出指令，用于迭代输出数据模型中的集合。list指令的语法格式如下：

    <#list sequence as item>  
      ...  
    </#list>  
&#8195;除此之外，迭代集合对象时，还包括两个特殊的循环变量：  
&#8195;a、item_index：当前变量的索引值。  
&#8195;b、item_has_next:是否存在下一个对象  
&#8195;也可以使用<#break>指令跳出迭代

    <#list ["星期一","星期二","星期三","星期四","星期五"] as x>  
    	${x_index +1}.${x} 
		<#if x_has_next>,</#if>  
    	<#if x = "星期四"><#break></#if>  
    </#list>  
      
####4、include 指令
&#8195;include指令的作用类似于JSP的包含指令，用于包含指定页，include指令的语法格式如下  
&#8195;<#include filename [options]  
&#8195;在上面的语法格式中，两个参数的解释如下  
&#8195;a、filename：该参数指定被包含的模板文件  
&#8195;b、options：该参数可以省略，指定包含时的选项，包含encoding和parse两个选项，encoding指定包含页面时所使用的解码集，而parse指定被包含是否作为FTL文件来解析。如果省略了parse选项值，则该选项值默认是true
####5、 import指令
&#8195;该指令用于导入FreeMarker模板中的所有变量，并将该变量放置在指定的Map对象中，import 指令的语法格式如下  

    <#import path as mapObject>
&#8195;在上面的语法格式中，path指定要被导入的模板文件，而mapObject是一个Map对象名，通过这行代码，将导致path模板中的所有变量都被放置在mapObject中
####6、noparse指令
&#8195;noparse指令指定FreeMarker不处理该指令里包含的内容，该指令的语法格式如下：

     <#noparse>
    	...
     </#noparse>
         
####7、escape、noescape指令
&#8195;当你使用escape指令包围模板中的一部分时，在块中出现的插值 (${...}) 会和转义表达式自动结合。这是一个避免编写相似表达式的很方便的方法。 它不会影响在字符串形式的插值(比如在 <#assign x = "Hello ${user}!">)。而且，它也不会影响数值插值 (#{...})。 

    <#escape x as x?html>
      First name: ${firstName}
      Last name: ${lastName}
      Maiden name: ${maidenName}
    </#escape> 
&#8195;事实上它等同于：

    First name: ${firstName?html}
    Last name: ${lastName?html}
    Maiden name: ${maidenName?html}

&#8195;有时需要暂时为一个或两个在转义区块中的插值关闭转义。你可以通过关闭， 过后再重新开启转义区块来达到这个功能，但是那么你不得不编写两遍转义表达式。 你可以使用非转义指令来替代：

    <#escape x as x?html>
      From: ${mailMessage.From}
      Subject: ${mailMessage.Subject}
      <#noescape>Message: ${mailMessage.htmlFormattedBody}</#noescape>
      ...
    </#escape>
&#8195;和这个是等同的：

    From: ${mailMessage.From?html}
    Subject: ${mailMessage.Subject?html}
    Message: ${mailMessage.htmlFormattedBody}
    ...

####8、assign指令
&#8195;它用于为该模板页面创建或替换一个顶层变量
        
####9、setting指令
&#8195;该指令用于设置FreeMarker的运行环境，该指令的语法格式如下：

    <#setting name = value>
&#8195;name 的取值范围包括如下几个  
&#8195;locale ：该选项指定该模板所用的国家/语言选项  
&#8195;number_format:该选项指定格式化输出数字的格式  
&#8195;boolean_format:该选项指定两个布尔值的语法格式，默认值是"true、false"  
&#8195;date_format,time_format,datetime_format：该选项指定格式化输出日期的格式  
&#8195;time_zone:  设置格式化输出日期时所使用的时区  
####10、macro、nested、return指令

    <#macro name param1 param2 ... paramN>
      ...
      <#nested loopvar1, loopvar2, ..., loopvarN>
      ...
      <#return>
      ...
    </#macro>
&#8195;这里：

*	name： 宏变量的名称，它不是表达式。和 顶层变量 的语法相同，比如 myMacro 或 my\-macro。 然而，它可以被写成字符串的形式，如果宏名称中包含保留字符时，这是很有用的， 比如 <#macro "foo~bar">...。 注意这个字符串没有扩展插值(如 "${foo}")。  
*	param1， param2，等...： 局部变量 的名称，存储参数的值 (不是表达式)，在 = 号后面和默认值(是表达式)是可选的。 默认值也可以是另外一个参数，比如 <#macro section title label=title>。参数名称和 顶层变量 的语法相同，所以有相同的特性和限制。  
*	paramN， 最后一个参数，可能会有三个点(...)， 这就意味着宏接受可变数量的参数，不匹配其它参数的参数可以作为最后一个参数 (也被称作笼统参数)。当宏被命名参数调用， paramN 将会是包含宏的所有未声明的键/值对的哈希表。当宏被位置参数调用， paramN 将是额外参数的序列。 (在宏内部，要查找参数，可以使用 myCatchAllParam?is_sequence。)  
*	loopvar1， loopvar2等...： 可选的，循环变量 的值， 是 nested 指令想为嵌套内容创建的。这些都是表达式。  
*	return 和 nested 指令是可选的，而且可以在 <#macro ...> 和 </#macro> 之间被用在任意位置和任意次数。

&#8195;没有默认值的参数必须在有默认值参数 (paramName=defaultValue) 之前。

    <#macro test foo bar="Bar" baaz=-1>
      Test text, and the params: ${foo}, ${bar}, ${baaz}
    </#macro>
    <@test foo="a" bar="b" baaz=5*5-2/>
    <@test foo="a" bar="b"/>
    <@test foo="a" baaz=5*5-2/>
    <@test foo="a"/>