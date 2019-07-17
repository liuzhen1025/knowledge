##
RequestMappingHandlerAdapter
<bean id="jsonConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"></bean>

    <bean id="stringConverter"
          class="org.springframework.http.converter.StringHttpMessageConverter">
        <property name="supportedMediaTypes">
            <list>
                <value>text/plain;charset=UTF-8</value>
            </list>
        </property>
    </bean>

    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <ref bean="stringConverter"/>
                <ref bean="jsonConverter"/>
            </list>
        </property>
    </bean>
##

##
所有的converter 所在包
org.springframework.http.converter
##