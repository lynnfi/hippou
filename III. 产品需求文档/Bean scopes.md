Bean scopes
====

�������߶���Bean��ʱ��ͬʱҲ�ᶨ���˸���δ���Beanʵ������Щ���崴���Ĺ����Ǻ���Ҫ�ģ���Ϊֻ��ͨ������Щ���̵����ã������߲��ܴ���ʵ������

�����߲������Կ���ע�벻ͬ��������Bean֮�У�Ҳ��������Bean�����������ַ����Ƿǳ�ǿ����ҵ���Ҳ�ǳ��õġ������߿���ͨ��������ָ������������򣬶�������Java�����������á�Bean�������ö���������
Spring���֧��5���������������������ǵ�������ʹ�û���web��`ApplicationContext`��ʱ�����Ч�ġ�

�������Springֱ��֧�ֵ��������ˣ���Ȼ������Ҳ�����Լ�����������

|������|����|
|---------|--|
|����(singleton)|��Ĭ�ϣ�ÿһ��Spring IoC������ӵ��Ψһ��һ��ʵ������|
|ԭ�ͣ�prototype��|һ��Bean���壬����������|
|����request��|һ��HTTP��������һ��Bean����Ҳ����˵��ÿһ��HTTP�������Լ���Beanʵ����ֻ�ڻ���web��Spring `ApplicationContext`�п���|
|�Ự��session��|�޶�һ��Bean��������ΪHTTP`session`���������ڡ�ͬ����ֻ�л���web��Spring `ApplicationContext`����ʹ��|
|ȫ�ֻỰ��global session��|�޶�һ��Bean��������Ϊȫ��HTTP`Session`���������ڡ�ͨ�������Ż���վ������ͬ����ֻ�л���web��Spring `ApplicationContext`����|
|Ӧ��(application)|�޶�һ��Bean��������Ϊ`ServletContext`���������ڡ�ͬ����ֻ�л���web��Spring `ApplicationContext`����|

> ��Spring 3.0�У�*�߳�������*�ǿ��õģ�������Ĭ��ע��ġ����˽�������Ϣ�����Բο����ĺ������`SimpleThreadScope`���ĵ�����Ҫ�˽����ע����������������Զ���������򣬿��Բο���������ݡ�

### The singleton scope

����Beanȫ��ֻ��һ�������ʵ�������н�����Bean��Ϊ����������£��������ؽ���ͬһ��ʵ����

����֮���������߶���һ��Bean��������Ϊ����ʱ��Spring IoC����ֻ�����Bean������������Bean��Ψһʵ������ЩΨһ��ʵ���Ỻ�浽�����У�������Ե���Bean����������ã����������������õ����Ψһ��ʵ����

![](singleton.png)

Spring�ĵ���Bean�������ģʽ֮�е�������ĵ���ģʽ����������ġ����ģʽ�еĵ���ģʽ�ǽ�һ�������������Ӳ����ģ�һ��ClassLoaderֻ��Ψһ��һ��ʵ����
��Spring�ĵ����������ǻ���ÿ��������ÿ��Beanֻ��һ��ʵ��������ζ�ţ���������߸���һ���ඨ����һ��Bean�ڵ�����Spring�����У���ôSpring���������Bean���崴��һ��Ψһ��Beanʵ����
*������������Spring��Ĭ��������*��������������ڻ���XML�����������õ���ģʽ��Bean��

```
<bean id="accountService" class="com.foo.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.foo.DefaultAccountService" scope="singleton"/>
```

### The prototype scope

�ǵ����ģ�ԭ�͵�Beanָ�ľ���ÿ������Beanʵ����ʱ�򣬷��صĶ�����ʵ����Bean����Ҳ����˵��ÿ��ע�뵽�����Bean����ͨ������`getBean()`����õ�Bean������ȫ�µ�ʵ����
���ǻ����̰߳�ȫ�ԵĿ��ǣ����ʹ����״̬��Bean������ԭ�������򣬶���״̬��Bean�����õ���������

���������˵����Spring��ԭ��������DAOͨ����������Ϊԭ�Ͷ�����Ϊ���͵�DAO�ǲ������κε�״̬�ġ�

![](prototype.png)

���������չʾ��XML����ζ���һ��ԭ�͵�Bean��

```
<bean id="accountService" class="com.foo.DefaultAccountService" scope="prototype"/>
```

����������������ȣ�Spring�ǲ�����ȫ����ԭ��Bean���������ڵģ�Spring����ֻ���ʼ���������Լ�װ����ЩBean�����ݸ�Client������֮��Ͳ�����ȥ��ԭ��Bean֮��Ķ����ˡ�
Ҳ����˵����ʼ���������ڻص������������������Bean�Ƕ�����õģ����������������ڻص�������ԭ��Bean�ǲ�����õġ����ԣ��ͻ��˴������ע������ԭ��Bean�Լ��ͷ�ԭ��Bean�����е�һЩ��Դ��
����ͨ��ʹ���Զ����`bean post-processor`����Spring�ͷŵ�ԭ��Bean�����е���Դ��

��ĳЩ������˵��Spring�����Ľ�ɫ����ȡ����Java��`new`�����������е��������ڵĿ�����Ҫ�ɿͻ���������

### Singleton beans with prototype-bean dependencies

��ʹ�õ���Bean��ʱ�򣬶��õ���Bean��������ԭ��Bean��ʱ����Ҫע�����*�����Ľ��������ڳ�ʼ���Ľ׶ε�*����ˣ������ԭ��Beanע�뵽������Bean֮�У�ֻ������һ��ԭ�͵�Bean��Ȼ��ע�뵽������Bean֮�С����������ԭ��Bean��Ȼ����ֻ��һ��ʵ���ġ�

Ȼ������������Ҫ����Bean��ԭ�͵�Bean��������Ҫÿ��������ʱ������һ���µ�ʵ������ô��Ͳ��ܹ���һ��ԭ�͵�Bean��ע�뵽һ��������Bean�����ˣ���Ϊ����ע��ֻ�����*һ��*��
��Spring������ʵ��������Bean��ʱ�򣬾ͻ�����Լ�ע������������������ʵ����Ҫÿ�ζ�����һ���µ�ʵ�������Բο�[Dependencies](Dependencies.md)�е�**����ע��**���֡�

### Request, session, global session, application, and WebSocket scopes

`request`,`session`�Լ�`global session`��������������ֻ���ڻ���web��Spring`ApplicationContext`ʵ�ֵģ�����`XmlWebApplicationContext`���в���ʹ�á�
��������߽����ڳ����Spring IoC�����б���`ClassPathXmlApplicationContext`��ʹ����Щ��������ô�����׳�һ��`IllegalStateException`��˵��ʹ����δ֪��������

### Initial web configuration

Ϊ���ܹ�ʹ��`request`,`session`�Լ�`global session`������web��Χ��Bean��,��Ҫ������Bean֮ǰ������һЩ���������á������ڱ�׼�������򣬱���`singleton`�Լ�`prototype`����������Щ���������õģ�

�����������ȡ����Servlet�Ļ�����

�������������ʹ����Spring Web MVC��ܵĻ���ÿһ�������ͨ��Spring��`DispatcherServlet`����`DispatcherPortlet`������ģ�Ҳ��û����������ĳ�ʼ�����á�`DispatcherServlet`��`DispatcherPortlet`�Ѿ���������ص�״̬��

���ʹ��Servlet 2.5��web������������ͨ��Spring��`DispatcherServlet`������JSF����Struts����������ô��������Ҫע��`org.springframework.web.context.request.RequestContextListener`����`ServletRequestListener`��
����Servlet 3.0�Ժ���Щ���ܹ�ͨ��`WebApplicationInitializer`�ӿ���ʵ�֡����ߣ������һЩ�ɰ汾�������Ļ���������`web.xml`���������µ�Listener������

```
<web-app>
    ...
    <listener>
        <listener-class>
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```

����Ƕ�Listener������Ϥ��Ҳ���Կ���ʹ��Spring��`RequestContextFilter`��Filter��ӳ��ȡ����webӦ�õ����ã������߿��Ը����������ӽ����ʵ����޸ġ�

```
<web-app>
    ...
    <filter>
        <filter-name>requestContextFilter</filter-name>
        <filter-class>org.springframework.web.filter.RequestContextFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestContextFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ...
</web-app>
```

`DispatcherServlet`��`RequestContextListener`�Լ�`RequestContextFilter`���ı�������ȫһ�£����ǰ�request���󵽷��������`Thread`�ϡ����ʹ��Bean��֮��ĵ�������������ͻỰ��Χ�Ͽɼ���

### Request scope

�ο����µ�Bean����

```
<bean id="loginAction" class="com.foo.LoginAction" scope="request"/>
```

Spring��������ÿ���õ�`loginAction`������ÿ��HTTP�����ʱ�򶼻ᴴ��һ���µ�`LoginAction`ʵ����Ҳ����˵��`loginAction`Bean����������HTTP`Request`����ġ�
�����߿�������ı�ʵ����״̬����Ϊ����ͨ��`loginAction`������������ʵ�����������������߸ı��ʵ��״̬�����д�����Beanʵ�����Ǹ��ݶ������������ġ�����������ϣ����BeanҲ�����١�

### Session scope

�ο����µ�Bean���壺

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

Spring��������ÿ�ε��õ�`userPreferences`��һ��������HTTP�Ự����������һ���µ�`UserPreferences`ʵ��������֮��`userPreferences`Bean����������HTTP`Session`����ġ�
��`request-scoped`�������Bean�ϣ������߿�������ĸ���ʵ����״̬��ͬ����������HTTP`Session`������ʵ����ÿ��Session��������`userPreferences`�������µ�ʵ�������Կ����߸���Bean��״̬������������Bean��Ȼ�ǲ��ɼ��ġ���HTTP`Session`�����ˣ���ô�������`Session`��������BeanҲ�������ˡ�

### Global session scope

> �ò�����Ҫ������`portlet`�ģ��������Google�������`portlet`�������Ϣ��

�ο����µ�Bean���壺

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="globalSession"/>
```

`global session`������Ƚ�����֮ǰ�ᵽ�ı�׼��HTTP`Session`��������������ֻӦ���ڻ����Ż���portlet-based����webӦ�õ�����֮�еġ��Ż���Spec�ж����`global session`�����壺`global session`�����й����Ż���webӦ������������Ϊ`global session`�������BEan��������ȫ���Ż�`Session`���������ڵġ�

�����ʹ�ñ�׼�Ļ���Servlet��WebӦ�ã����Ҷ�����`global session`�������Bean����ôֻ�ǻ�ʹ�ñ�׼��HTTP`Session`�����򣬲��ᱨ��

### Application scope

�������µ�Bean���壺

```
<bean id="appPreferences" class="com.foo.AppPreferences" scope="application"/>
```

Spring������������webӦ��ʹ�õ�`appPreferences`��ʱ�򴴽�һ���µ�`AppPreferences`��ʵ����Ҳ����˵��`appPreferences`Bean����`ServletContext`����ģ�����һ����ͨ��`ServletContext`����һ����������������һЩ�̶�����˵��Spring�ĵ����������Ǽ�Ϊ���Ƶģ�����Ҳ�����²�֮ͬ����

* `application`��������ÿ��`ServletContext`�а���һ����������ÿ��Spring`ApplicationContext`֮�а���һ����ĳЩӦ���п��ܰ�����ֹһ��`ApplicationContext`����
* `application`�����������Ϊ`ServletContext`�����Կɼ�������Bean��`ApplicationContext`�ɼ���

### Scoped beans as dependencies

Spring IoC�����������������Bean����ʵ������ͬʱҲ����װ�������������������װ��һ��Bean��һ������������Bean����ȥ������HTTP���󷵻ص�Bean��,��ô������ѡ��ע��һ��AOP��������Ƕ��������Bean��Ҳ����˵����������Ҫע��һ�������������������ȿ����ҵ�ʵ�ʵ�Bean��Ҳ�ܹ�����һ��ȫ�µ�Bean��

> �����߻��ڵ���Bean��ʹ��`<aop:scoped-proxy/>`��ǩ��������һ�����������������þ���������ȡָ����Bean��
������ʹ��`<aop:scoped-proxy/>`������һ��ԭ��Bean��ʱ��ÿ��ͨ������ĵ��ö������һ���µ�Ŀ��ʵ����
���ң��������������Ψһ����ȡ��������Bean��Ψһ��ȫ�ķ�ʽ��������Ҳ����ͨ���򵥵�����ע��Ϊ`ObjectFactory<MyTargetBean>������ͨ������`getObject()`֮��ĵ�������ȡһЩָ����������������ֱ�Ӵ���������ʵ����
JSR-330�����ⲿ�ֵ����Ʋ�ͬ����`Provider`��ͨ��ʹ��`Provider<MyTargetBean>������һ����ص�`get()`��������ȡָ������������ϸ����[JSR-330](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#beans-standard-annotations)����Ϣ���Խ�ȥ��ϸ�˽⡣

��ο���������ӣ�

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- an HTTP Session-scoped bean exposed as a proxy -->
    <bean id="userPreferences" class="com.foo.UserPreferences" scope="session">
        <!-- instructs the container to proxy the surrounding bean -->
        <aop:scoped-proxy/>
    </bean>

    <!-- a singleton-scoped bean injected with a proxy to the above bean -->
    <bean id="userService" class="com.foo.SimpleUserService">
        <!-- a reference to the proxied userPreferences bean -->
        <property name="userPreferences" ref="userPreferences"/>
    </bean>
</beans>
```

ʹ�ô���ֻ��Ҫ�ڶ��������Bean����֮�м���һ���ӽڵ�`<aop:scoped-proxy/>`���ɡ�[Dependencies](Dependencies.md)�е�*����ע��*�о��ἰ����Bean������һЩ���⣬��Ҳ������ΪʲôҪʹ��`aop`�����ԭ�򡣼�������û��ʹ��`aop`�������ֱ�ӽ�������ע�룬�ο����µ����ӣ�

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

����������У�`userManager`������һ��������Bean��ע����һ��HTTP`Session`�����`userPreferences`��������Ȼ���������`userManager`��Spring������ֻ��ʵ����һ�Σ�������(��ǰ�����е�`userPreferences`)Ҳֻ��ע��һ�Ρ���Ҳ����ζ��`userManager`ÿ��ʹ�õĶ�����ͬ��`userPreferences`����

��ô��������;��Բ��ǿ�������Ҫ�����ֽ���������ע�뵽��������Bean�е�����ˣ�������˵��ע��һ��HTTP`Session`�����Bean��һ������֮�У�����˵����������ͨ��`userManager`����ȡָ����ĳ��HTTP`Session`��`userPreferences`�����ǲ����ܵġ���������������һ����ȡ`UserPreferences`����Ľӿڣ�����ӿڿ��Ը���Bean�����������������ȡ����������صĶ��󣨱���˵HTTP`Request`����HTTP`Session`�ȣ�������֮��ע��������`userManager`�У�����ʶ����������`UserPreferences`�Ǵ������������֮�У���`UserManager`ʵ�����÷�������ȡע�������`UserPreferences`����ʱ����ʵֻ������˴���ķ������ɴ���ȥ��ȡ�����Ķ�������������о���HTTP`Session`�����Bean��

���Ե�������ϣ���ܹ���ȷ��ʹ������`request`,`session`����`globalSession`�����Bean����Ϊ����ʱ����Ҫ�������µ��������ã�

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

**ѡ����������**

Ĭ������£�Spring�������������ʱ����Ϊ`<aop:scoped-proxy/>`�ı�ǩʱ���ᴴ��һ��*����CGLIB�Ĵ���*��

> CGLIB���������`public`�������ã����Բ�Ҫ�ڷ�`public`������ʹ�ô��������������ȡ��ָ����������

���ߣ������߿���ͨ��ָ`<aop:scoped-proxy/>`��ǩ��`proxy-target-class`���Ե�ֵΪ`false`������Spring������Ϊ��Щ���������Bean����һ����׼JDK�Ļ��ڽӿڵĴ���ʹ��JDK���ڽӿڵĴ�����ζ�ſ����߲���Ҫ��Ӧ�õ�·�����ö���Ŀ�����ɴ�����Ȼ����Ҳ��ζ�Ŷ��������Bean��Ҫ����ʵ��һ���ӿڣ��������Ǵ���Щ�ӿ�����ȡ�ġ�

```
<!-- DefaultUserPreferences implements the UserPreferences interface -->
<bean id="userPreferences" class="com.foo.DefaultUserPreferences" scope="session">
    <aop:scoped-proxy proxy-target-class="false"/>
</bean>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

`DefaultUserPreferences`ʵ����`UserPreferences`�����ṩ�˽ӿ�����ȡʵ�ʵĶ��󡣸������Ϣ���Բο�[AOP����](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#aop-proxying)��

### Custom scopes

Bean������������ǿ���չ�ģ������߿��Զ����Լ���һЩ�������������¶����Ѿ����ڵ������򣬵�����һ��Spring�Ŷ��ǲ��Ƽ��ģ����ҿ����߲��ܹ���д`singleton`�Լ�`prototype`������

### Creating a custom scope

Ϊ���ܹ�ʹSpring���Թ������߶���������򣬿�������Ҫʵ��`org.springframework.beans.factory.config.Scope`�ӿڡ���֪�����ʵ�ֿ������Լ�����������򣬿��Բο�Spring��ܵ�һЩʵ�ֻ�����`Scope`��javadoc���������Ϳ�������Ҫʵ�ֵ�һЩϸ�ڡ�

`Scope`�ӿ��к���4����������ȡ�����Ƴ������������ٵȡ�

����ķ�������һ�����ڵ�������Ķ��󡣱���˵`Session`��������ʵ�֣��ú��������ػỰ�������Bean�����Bean�����ڣ��÷����ᴴ��һ���µ�ʵ����

```
Object get(String name, ObjectFactory objectFactory)
```

����ķ����Ὣ�����Ƴ�������ͬ������`Session`Ϊ�����ú�����ɾ��`Session`�������Bean��ɾ���Ķ������Ϊ����ֵ���أ����޷��ҵ������ʱ����Է���`null`��

```
Object remove(String name)
```

����ķ�����ע��һ���ص�����������Ҫ���ٻ������������ٵ�ʱ����á���ϸ���Բο���javadoc��Spring�������ʵ�����ҵ�����������ٻص���������Ϣ��

```
void registerDestructionCallback(String name, Runnable destructionCallback)
```

����ķ������ȡ����������ֱ�ʶ�����ֱ�ʶ������������������

```
String getConversationId()
```


### Using a custom scope

��ʵ���˿����ߵ��Զ���������֮�󣬿����߻���Ҫ��Spring�����ܹ�ʶ��������µ�����������ķ���������Spring����������ע���µ�������ġ�

```
void registerScope(String scopeName, Scope scope);
```

�����������`ConfigurableBeanFactory`�Ľӿ��������ģ��ڴ������`ApplicationContext`��ʵ���ж��ǿ����õģ�����ͨ��`BeanFactory`���������á�

`registerScope(..)`�����ĵ�һ���������������������Ψһ��һ�����֣�������˵������Spring����֮�е�`singleton`��`prototype`�������������֡��ڶ��������������Ǹ���Scope�ӿ���ʵ�ֵľ���Ķ���

�ٶ�������ʵ�����Զ����������Ȼ�������²�����ע�ᡣ

> ���������ʹ����`SimpleThreadScope`���������Spring������ʵ�ֵģ�����û��Ĭ��ע�ᡣ��������ʵ�ֵ�`Scope`Ҳ����ͨ�����·�ʽ��ע�ᡣ

```
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```

֮�󣬿����߿���ͨ���������Ƶ�Bean������ʹ���Զ����`Scope`:

```
<bean id="..." class="..." scope="thread">
```

�ڶ��Ƶ�`Scope`�У�������Ҳ�����ڽ���ͨ����̷�ʽ��ע���Լ���`Scope`�������߿���ͨ������`CustomScopeConfigurer`����ʵ�֣�

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
        <property name="scopes">
            <map>
                <entry key="thread">
                    <bean class="org.springframework.context.support.SimpleThreadScope"/>
                </entry>
            </map>
        </property>
    </bean>

    <bean id="bar" class="x.y.Bar" scope="thread">
        <property name="name" value="Rick"/>
        <aop:scoped-proxy/>
    </bean>

    <bean id="foo" class="x.y.Foo">
        <property name="bar" ref="bar"/>
    </bean>

</beans>
```