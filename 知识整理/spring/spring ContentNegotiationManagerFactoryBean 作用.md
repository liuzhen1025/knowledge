####1 
spring ContentNegotiationManagerFactoryBean 作用
一.什么是内容协商
简单点说，就是同一资源,可以有多种表现形式，比如xml、json等，具体使用哪种表现形式，是可以协商的。
这是RESTfull的一个重要特性，Spring Web MVC也支持这个功能。
 
      1.Spring MVC REST是如何决定采用何种方式(视图)来展示内容呢？
一：根据Http请求的header中的Accept属性的值来判读，比如：
Accept: application/xml                将返回xml格式数据 
Accept: application/json               将返回json格式数据
 
优点：是这种方式是理想的标准方式
缺点：是由于浏览器的差异，导致发送的Accept Header头可能会不一样，从而导致服务器不知要返回什么格式的数据
 
二：根据扩展名来判断，比如：
/mvc/test.xml  将返回xml格式数据 
/mvc/test.json 将返回json格式数据 
/mvc/test.html 将返回html格式数据 
 
缺点：丧失了同一URL的多种展现方式。在实际环境中使用还是较多的，因为这种方式更符合程序员的习惯
 
三：根据参数来判断
/mvc/test?format=xml        将返回xml数据 
/mvc/test?format=json       将返回json数据 
 
缺点：需要额外的传递format参数，URL变得冗余繁琐，缺少了REST的简洁风范


      2.使用内容协商的功能，如果不使用第三种方式的话，3.2的版本可以什么都不用配置，默认就能支持前面两种。下面还是看看怎么配置，示例如下：
 
需要在spring的配置文件中做配置，示例如下：
<!--1、检查扩展名（如my.pdf）；2、检查Parameter（如my?format=pdf）；3、检查Accept Header-->
    <bean id= "contentNegotiationManager" class= "org.springframework.web.accept.ContentNegotiationManagerFactoryBean">
        <!-- 扩展名至mimeType的映射,即 /user.json => application/json -->
        <property name= "favorPathExtension" value= "true" />
        <!-- 用于开启 /userinfo/123?format=json 的支持 -->
        <property name= "favorParameter" value= "true" />
        <property name= "parameterName" value= "format"/>
        <!-- 是否忽略Accept Header -->
        <property name= "ignoreAcceptHeader" value= "false"/>
	<!--扩展名到MIME的映射；favorPathExtension, favorParameter是true时起作用  -->
        <property name= "mediaTypes"> 
            <value>
                json=application/json
                xml=application/xml
                html=text/html
            </value>
        </property>
	<!--<property name="mediaTypes">-->
   		 <!--<map>-->
        		<!--<entry key="xml" value="application/xml"/>-->
        		<!--<entry key="json" value="text/plain"/>-->
    	   		<!--<entry key="xls" value="application/vnd.ms-excel"/>-->
    		<!--</map>-->
	<!--</property>--> 
        <!-- 默认的content type ,在没有扩展名和参数时即: "/user/1" 时的默认展现形式  -->
        <property name= "defaultContentType" value= "text/html" />
    </bean>

视图定义：
 <bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
        <property name="order" value="0"/>
        <property name="contentNegotiationManager" ref="contentNegotiationManager"/>
        <property name="viewResolvers">
            <list>
                <!-- 这个类用于jsp视图解析 -->
                <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                    <property name="prefix" value="/WEB-INF/page/"/>
                    <property name="suffix" value=".jsp"/>
                </bean>
 
            </list>
        </property>
        <property name="defaultViews">
            <list>
                <bean class="org.springframework.web.servlet.view.json.MappingJackson2JsonView">
                </bean>
                <!-- for application/xml -->
                <bean class="org.springframework.web.servlet.view.xml.MarshallingView">
                    <property name="marshaller">
                        <bean class="org.springframework.oxm.castor.CastorMarshaller">
                            <property name="validating" value="false"></property>
                        </bean>
                    </property>
                </bean>
            </list>
        </property>
    </bean>

在mvc:annotation-driven里面配置使用内容协商
<mvc:annotation-driven
      conversion-service= "conversionService"
      content-negotiation-manager= "contentNegotiationManager”/>

#