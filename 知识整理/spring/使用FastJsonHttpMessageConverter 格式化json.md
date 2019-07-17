###1
格式化jSON

<mvc:annotation-driven content-negotiation-manager="contentNegotiationManager">
        <mvc:message-converters register-defaults="false">
            <!-- 避免IE执行AJAX时,返回JSON出现下载文件 -->
            <bean id="fastJsonHttpMessageConverter"
                  class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <!-- 这里顺序不能反，一定先写text/html,不然ie下出现下载提示 -->
                        <value>text/html;charset=UTF-8</value>
                        <value>application/json;charset=UTF-8</value>
                    </list>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
##