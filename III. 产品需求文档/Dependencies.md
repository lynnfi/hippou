Dependencies
====

һ���������ҵӦ�ò���ֻ��һ�����󣨻�����Spring Bean����������򵥵�Ӧ�ö�Ҫ���������Эͬ���������ն��û�����һ��������Ӧ�õġ���һ���ֽ����Ϳ�������δӽ������嵥����Bean��������ЩBean��һ��Ӧ����Эͬ������

### Dependency Injection

*����ע��*��һ���ö���ֻͨ��������������������Ĳ����������õ��������������ǵ������Ĺ��̡���Щ����Ҳ�Ƕ�������ҪЭͬ�����Ķ����������ڴ���Bean��ʱ��ע����Щ����������������ȫ��ת����Bean�Լ�����ʵ�����������������������������Ҳ��֮Ϊ*���Ʒ�ת*��

��ʹ��������ע���׼���Ժ󣬻�����ڹ���ͽ������֮���������ʹ�ô�����ӵļ򵥡������ٹ�ע������Ҳ����Ҫ֪���������λ�á������Ļ��������ߵ���������ڲ��ԣ������ǵ������ߵ������ǽӿڻ��߳����������������߿��������ڵ�Ԫ������mock����

����ע����Ҫʹ�����ַ�ʽ��һ���ǻ��ڹ��캯����ע�룬��һ�ֵĻ���Setter����������ע�롣

### Constructor-based dependency Injection

���ڹ��캯��������ע������IoC������������Ĺ��캯�������캯���Ĳ����������Bean�������Ķ��󡣸����ô������ľ�̬������������һ�������������չʾ��һ����ͨ�����캯����ʵ������ע��ġ���Ҫע����ǣ������û���κ�*����*�ĵط���ֻ��һ���򵥵ģ����������κ���������ӿڣ��������ע�����ͨ�ࡣ

```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...

}
```

**���캯���Ĳ�������**

���캯���Ĳ���������ͨ��������������ƥ��ġ������Bean�Ĺ��캯���������������壬��ô������������˳��Ҳ���Ǿ�����Щ����ʵ�����Լ�װ�ص�˳�򡣲ο����´��룺

```
package x.y;

public class Foo {

    public Foo(Bar bar, Baz baz) {
        // ...
    }

}
```

����`Bar`��`Baz`�ڼ̳в���ϲ���أ�Ҳû��ʲô����Ļ��������������ȫ���Թ��������������߲���Ҫ��ȥ`<constructor-arg>`Ԫ����ָ�����캯��������������������Ϣ��

```
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>

    <bean id="bar" class="x.y.Bar"/>

    <bean id="baz" class="x.y.Baz"/>
</beans>
```

��������һ��Bean��ʱ���������ȷ���Ļ���ƥ��Ṥ������������������ӣ�.��ʹ�ü򵥵����͵�ʱ�򣬱���˵`<value>true</value>`��Spring IoC�������޷��ж�ֵ�����͵ģ��������޷�ƥ��ġ����Ǵ������£�

```
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }

}
```

�����������������£���������ͨ��ʹ�ù��캯��������`type`������ʵ�ּ����͵�ƥ�䡣���磺

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

����ʹ��`index`������ָ�����������λ�ã����磺

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

�������Ҳͬʱ��Ϊ�˽�����캯�����ж����ͬ���͵Ĳ����޷���ȷƥ������⡣��Ҫע����ǣ������ǻ���0��ʼ�ġ�
������Ҳ����ͨ��������������ȥ�������ԡ�

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

��Ҫע�����,��������Ĵ�����������˵��Ա�Ǳ���,����Spring�ſ��Դӹ��캯�����Ҳ������ơ�������Ҳ����ʹ��`@ConstructorProperties`ע������ʽ�������캯�������ƣ��������´��룺

```
package examples;

public class ExampleBean {

    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }

}
```

### Setter-based dependency injection

����Setter����������ע���������������Bean���޲ι��캯���������޲����Ĺ���������Ȼ����������Setter������ʵ�ֵ�����ע�롣

���������չʾ��ʹ��Setter�������е�����ע�룬����������ֻ�Ǽ򵥵�POJO���󣬲��������κ�Spring������Ľӿڣ��������ע�⡣

```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;

    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...

}
```

`ApplicationContext`������Bean���ڻ��ڹ��캯��������ע�룬���߻���Setter��ʽ������ע�붼��֧�ֵġ�ͬʱҲ֧��ʹ��Setter��ʽ��ͨ�����캯��ע������֮���ٴ�ע����������������`BeanDefinition`�п���ʹ��`PropertyEditor`ʵ��������ѡ��ע��ķ�ʽ��Ȼ����������Ŀ����߲���ֱ��ʹ����Щ�࣬���Ǹ�ϲ��XML��ʽ��`bean`���壬���߻���ע������������ʹ��`@Component`��`@Controller`�ȣ�������������`@Configuration`��������ʹ��`@Bean`�ķ�����

> **���ڹ��캯�����ǻ���Setter������**
��Ϊ�����߿��Ի������ߣ�����ͨ���ȽϺõķ�ʽ��ͨ�����캯��ע��*��Ҫ������*ͨ��Setter��ʽ��ע��һЩ*��ѡ������*�����У���Setter���������`@Required`ע������������Ҫ��������
Spring�����Ƽ����ڹ��캯����ע�룬��Ϊ���ַ�ʽ���ʹ�����߽����������*���ɱ����*����ȷ����ע���������Ϊ`null`�����ң����ڹ��캯����ע���������ͻ��˵��õ�ʱ��Ҳ����ȫ����õġ���Ȼ������һ������˵������Ĺ��캯������Ҳ�Ƿǳ���Ĵ��뷽ʽ�����ַ�ʽ˵����ò������̫��Ĺ��ܣ�����ع�����ְͬ�ܷ��롣
����Setter��ע��ֻ�����ڿ�ѡ������������Ҳ�������һЩ�����Ĭ��ֵ��������Ҫ�Դ�����������з�NULL�ļ���ˡ�����Setter������ע����һ������֮���������ַ�ʽ��ע���ǿ��Խ��������ú�����ע��ġ�
����ע������ַ���ʺϴ�����������������ʱʹ�õ������Ŀ��ʱ�򣬿����߿��ܲ�û��Դ�룬���������Ĵ���Ҳû��setter��������ô��ֻ��ʹ�û��ڹ��캯��������ע���ˡ�

### Dependency resolution process

������Bean�Ľ������£�

* ����������������Ԫ������ʵ����`ApplicationContext`������Ԫ���ݿ���ͨ��XML�� Java ���룬����ע�⡣
* ÿһ��Bean������ͨ�����캯�������������Ի��߾�̬���������Ĳ���������ʾ����Щ��������Bean�����ĵ�ʱ��ע���װ�ء�
* ÿһ�����Ի��߹��캯���Ĳ�������ʵ�ʶ����ֵ��������������������Bean��
* ÿһ�����Ի��߹���������Ը�����ָ��������ת�����ɡ�SpringҲ���Խ�Stringת��Ĭ�ϵ�Java���ڵ����ͣ�����`int`,`long`,`String`,`boolean`�ȡ�

Spring������������������ʱ�����ÿһ��Bean����У�顣Ȼ����Bean��������Beanû������������ʱ���ǲ������ý�ȥ�ġ��������͵�Bean������������ʱ�����ó�Ԥʵ��״̬�ġ�Bean��`Scope`�ں����н��ܡ�������Bean��ֻ���������ʱ�򣬲Żᴴ������Ȼ����Bean�������һ��������ͼ�����ͼ��ʾBean֮���������ϵ���������ݴ�����������������Bean��˳��

> **ѭ������**
�����������Ҫʹ�û��ڹ��캯��������ע�룬��ô���п��ܳ���һ��ѭ�������ĳ�����
����˵����A�ڹ��캯������������B��ʵ��������B�Ĺ��캯��������A��ʵ�����������ô������A����B�໥ע��Ļ���Spring IoC�����ᷢ���������ʱ��ѭ�������������׳�`BeanCurrentlyInCreationException`��
�����߿���ͨ��ʹ��Setter��������������ע�룬�������Խ��������⡣���߾Ͳ�ʹ�û��ڹ��캯��������ע�룬����ʹ�û���Setter����������ע�롣����֮�����ܲ��Ƽ������ǿ����߿��Խ�ѭ����������Ϊ����Setter����������ע�롣

�����߿�������Spring����ȷ����Bean��Spring�ܹ��ڼ��صĹ����з������õ����⣬�������õ������ڵ�Bean������ѭ��������Spring�ᾡ���������Bean������ʱ��װ�����Ի��߽�����������Ҳ��ζ��Spring����������ȷ�����Beanע�����������ʱ���׳��쳣�����磬Bean�׳�ȱ�����Ի������Բ��Ϸ������ӳٵĽ���Ҳ��Ϊʲô`ApplicationContext`��ʵ�ֻ����Bean����Ԥʵ����״̬��������ͨ��`ApplicationContext`�Ĵ���������������ʹ��Bean֮ǰ����һЩ�ڴ���۷������õ����⡣������Ҳ���Ը���Ĭ�ϵ���Ϊ�õ���Bean�ӳټ��أ������Ǵ���Ԥʵ����״̬��
���������ѭ�������Ļ���Bean�����õ�������������ȫ���������ġ�������˵�����Bean A������Bean B����ôSpring IoC������������Bean B��Ȼ�����Bean A��Setter����������Bean A������֮��Bean�Ȼ�ʵ������Ȼ��ע��������Ȼ�������ص��������ڷ����ĵ��á�

### Examples of dependency injection

��������ӻ�ʹ�û���XML���õ�Ԫ���ݣ�Ȼ��ʹ��Setter��ʽ��������ע�롣�������£�

```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }

}
```

����������ӵ��У�Setter������������XML�ļ�����һ�£�����������ǻ��ڹ��캯��������ע��

```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- constructor injection using the nested ref element -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>

    <!-- constructor injection using the neater ref attribute -->
    <constructor-arg ref="yetAnotherBean"/>

    <constructor-arg type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public ExampleBean(
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }

}
```

��Bean����֮�еĹ��캯������������������`ExampleBean`��������

��������ӣ���ͨ����̬�Ĺ�������������Beanʵ���ġ�

```
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    // a private constructor
    private ExampleBean(...) {
        ...
    }

    // a static factory method; the arguments to this method can be
    // considered the dependencies of the bean that is returned,
    // regardless of how those arguments are actually used.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // some other operations...
        return eb;
    }

}
```

���������Ĳ�����Ҳ��ͨ�� `<constructor-arg/>`��ǩ��ָ���ģ��ͻ��ڹ��캯��������ע����һ�µġ�֮ǰ���ᵽ�������ص����Ͳ���Ҫ��`exampleBean`�е�`class`����һ�µģ�`class`ָ�����ǰ��������������ࡣ��Ȼ�ˣ������������һ�µġ�ʹ��`factory-bean`��ʵ��������������Bean�ģ�����Ͳ��������ˡ�

### 7.4.2 Dependencies and configuration in detail

��ǰ�������������߿���ͨ������Bean��������������������Bean������һЩֵ�ģ�Spring����XML������Ԫ����ͨ��֧��һЩ��Ԫ��`<property/>`�Լ�`<constructor-arg/>`���ﵽ��һĿ�ġ�

### Straight values (primitives, Strings, and so on)

Ԫ��`<property/>`��`value`�����������Ѻ��׶�����ʽ����һ�����Ի��߹��������Spring�ı���֮��������������Щ�ַ�����ֵת����ָ�������͡�

```
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>
```

���������ʹ�õ�p�����ռ䣬�Ǹ�Ϊ�򵥵�XML���á�

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>

</beans>
```

�����XML��Ϊ�ļ�࣬������Ϊ���Ե�������������ʱȷ���ģ��������ʱȷ���ģ����Գ���ʹ��[IntelliJ IDEA](http://www.jetbrains.com/idea/)����[Spring Tool Suite](https://spring.io/tools/sts)��Щ���߲����ڶ���Bean��ʱ���Զ�����������á���Ȼ���Ƽ�ʹ����ЩIDE��

������Ҳ���Զ���һ��`java.util.Properties`ʵ�������磺

```
<bean id="mappings"
    class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">

    <!-- typed as a java.util.Properties -->
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```

Spring�������Ὣ`<value/>`������ı�ͨ��ʹ��JavaBean��`PropertyEditor`����ת����һ��`java.util.Properties`ʵ������Ҳ��һ���ݾ���Ҳ��һЩSpring�ŶӸ�ϲ��ʹ��Ƕ�׵�`<value/>`Ԫ�ض�����`value`���Է��

**idrefԪ��**
`idref`Ԫ����һ�ּ򵥵���ǰУ�����ķ�ʽ��ͨ��id�����������е�������Bean�ķ�ʽ��

```
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean" />
    </property>
</bean>
```
������Bean�Ķ���������ʱ�������¶�������ȫһ�µġ�

```
<bean id="theTargetBean" class="..." />

<bean id="client" class="...">
    <property name="targetName" value="theTargetBean" />
</bean>
```

��һ�ַ�ʽ�Ǹ�ֵ���ᳫ�ģ���Ϊʹ����`idref`��ǩ�����ǵ�������*����׶�*�����Bean����У�飬ȷ��Beanһ�����ڡ����ڶ����汾�Ļ�����û���κ�У��ġ�ֻ��ʵ����������Bean `client`����ʵ����client��ʱ��Żᷢ�֡����`client`��һ��`prototype`��Bean����ô����ƴд֮��Ĵ���������������Ժ�ܾò��ܷ��֡�

> `idref`Ԫ�ص�`local`������4.0�Ժ��xsd���Ѿ�����֧���ˣ�����ʹ����`bean`���á���������˰汾�Ļ�����Ҫ��`idref local`���ö�ת���� `idref bean`���ɡ�

### References to other beans (collaborators)

`ref`Ԫ����`<constructor-arg/>`����`<property/>`�е�һ���ռ���ǩ�������߿���ͨ�������ǩ����һ��Bean��������һ��Bean������Ҫ����һ��Bean��ʱ�򣬱����õ�Bean����ʵ������Ȼ���������ԣ�Ҳ�������õ������������Bean�ǵ���Bean�Ļ�����ô��Bean������������ʼ�������յ�������һ��������������á�Bean�ķ�Χ�Լ�У��ȡ���ڿ������Ƿ�ͨ��`bean`,`local`,`parent`��Щ������ָ�������id����name���ԡ�

ͨ��ָ��Bean��`bean`�����е�`<ref/>`��ָ�������������һ�ַ�ʽ�����������������߸������е�Bean�������Ƿ���ͬһ��XML�ļ����嶼�������á�����`bean`�����е�ֵ���Ժ���������Bean�е�`id`����һ�£����ߺ����е�һ��`name`����һ�µġ�

```
<ref bean="someBean"/>
```

ͨ��ָ��Bean��`parent`���Իᴴ��һ�����õ���ǰ�����ĸ�����֮�С�`parent`���Ե�ֵ���Ը���Ŀ��Bean��`id`����һ�£����ߺ�Ŀ��Bean��`name`�����е�һ��һ�£�Ŀ��Bean�����ǵ�ǰ����Ŀ��Bean�����ĸ�������������һ��ֻ���ڴ��ڲ�λ�����������ϣ��ͨ��������������������һ�����ڵ�Bean��ʱ��Ż��õ�������ԡ�

```
<!-- in the parent context -->
<bean id="accountService" class="com.foo.SimpleAccountService">
    <!-- insert dependencies as required as here -->
</bean>
```

```
<!-- in the child (descendant) context -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
    </property>
    <!-- insert other configuration and dependencies as required here -->
</bean>
```

> ��`idref`��ǩһ����`ref`Ԫ���е�`local`��ǩ��xsd 4.0�Ժ��Ѿ�����֧���ˣ������߿���ͨ�����Ѵ��ڵ�`ref local`��Ϊ`ref bean`����ɸ���Spring��

### Inner beans

������`<bean/>`Ԫ�ص�`<property/>`����`<constructor-arg/>`Ԫ��֮�ڵ�Bean����*�ڲ�Bean*

```
<bean id="outer" class="...">
    <!-- instead of using a reference to a target bean, simply define the target bean inline -->
    <property name="target">
        <bean class="com.example.Person"> <!-- this is the inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```

�ڲ�Bean�Ķ����ǲ���Ҫָ��id�������ֵġ����ָ���ˣ�����Ҳ������֮��Ϊ�ֱ�Bean�����ֱ�ʶ������ͬʱҲ�������ڲ�Bean��`scope`��ǩ���ڲ�Bean *����* �����ģ����� *����* �����ⲿ��Beanͬʱ�����ġ����������޷����ڲ���Beanע�뵽�ⲿBean���������Bean�ġ�

��Ȼ��Ҳ�п��ܴ�ָ����Χ���յ��ƻ��Իص������磺һ������Χ���ڲ�Bean������һ��������Bean����ô�ڲ�Beanʵ����󶨵�������Bean����������Bean������ʵ�`request`��`scope`�������ڡ����ֳ������������ڲ�Beanͨ��ֻ�ǹ��������ⲿBean��


### Collections

��`<list/>`,`<set/>`,`<map/>`��`<props/>`Ԫ���У������߿�������Java��������`List`,`Set`,`Map`�Լ�`Properties`�����ԺͲ�����

```
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- results in a setAdminEmails(java.util.Properties) call -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- results in a setSomeList(java.util.List) call -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- results in a setSomeMap(java.util.Map) call -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- results in a setSomeSet(java.util.Set) call -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```

��Ȼ��map��key����value�������Ǽ��ϵ�value����������Ϊ����֮�е�һЩԪ�أ�

```
bean | ref | idref | list | set | map | props | value | null
```

**���Ϻϲ�**
Spring������Ҳ֧�����ϲ����ϡ������߿��Զ���һ������ʽ��`<list/>`,`<map/>`,`<set/>`����`<props/>`��ͬʱ������ʽ��`<list/>`,`<map/>`,`<set/>`����`<props/>`�̳в��Ҹ��Ǹ����ϡ�Ҳ����˵���Ӽ��ϵ�ֵ�Ǹ�Ԫ�غ���Ԫ�ؼ��ϵĺϲ�ֵ��������������ӡ�

```
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- the merge is specified on the child collection definition -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```

���Է��֣�������`child`Bean��ʹ����`merge=true`���ԡ���`child`Bean��������ʼ����ʵ������ʱ����ʵ���а�����`adminEmails`���Ͼ���`child`��`adminEmails`�Լ�`parent`��`adminEmails`���ϡ����£�

```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```

`child`��`Properties`���ϵ�ֵ�̳���`parent`��`<props/>`��`child`��ֵҲ֧����д`parent`��ֵ��

����ϲ�����Ϊ��`<list/>`,`<map/>`�Լ�`<set/>`֮��ļ������͵���Ϊ�����Ƶġ�`<list/>`���ض��������У���`List`�����������ƣ���������`ordered`����ġ����еĸ�Ԫ�������ֵ���������к���Ԫ�ص�ֵ֮ǰ�ġ�������`Map`,`Set`����`Properties`�ļ������ͣ��ǲ�����˳��ġ�

**���Ϻϲ�������**

�������ǲ��ܹ��ϲ���ͬ���͵ļ��ϵģ�����`Map`��`List`�ϲ����������������ô�������׳��쳣��`merge`�������Ǳ�����ָ�����ͼ����߼̳��ߣ��ӽڵ�Ķ����ϡ���ָ`merge`���Ե������ϵĶ�����������ģ������ںϲ���Ҳû���κ�Ч����

**ǿ���ͼ���**
��Java 5�Ժ󣬿����߿���ʹ��ǿ���͵ļ����ˡ�Ҳ���ǣ������߿�������һ��`Collection`���ͣ�Ȼ���������ֻ����`String`Ԫ�أ�������˵�������������ͨ��Spring��ע��ǿ���͵�`Collection`��Bean�У������߾Ϳ�������Spring������ת��֧����������

```
public class Foo {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```

```
<beans>
    <bean id="foo" class="x.y.Foo">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```

��`foo`������`accounts`׼��ע���ʱ��`accounts`�ķ�����Ϣ`Map<String, Float>`�ͻ�ͨ�������õ���������Spring ������ת��ϵͳ�ܹ�ʶ��ͬ�����ͣ������������`Float`Ȼ���ַ�����ֵ`9.99, 2.75`�Լ�`3.99`ת���ɶ�Ӧ��`Float`���͡�

### Null and empty string values

Spring���Ὣ���ԵĿղ�����ֱ�ӵ��ɿ��ַ�������������Ļ���XML��Ԫ�������þͻὫemail��������Ϊ`String`��ֵΪ`""`

```
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

��������Ӻ�����JAVA������һ�µġ�

```
exampleBean.setEmail("")
```

��`<null/>`Ԫ��������`null`��ֵ�����£�

```
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```
����Ĵ���������Java������һ����Ч����

```
exampleBean.setEmail(null)
```

### XML shortcut with the p-namespace

p�����ռ�����߿���ʹ��`bean`�����ԣ�������ʹ��Ƕ�׵�`<property/>`Ԫ�أ�����������������Ҫע���������

Spring��֧�ֻ���XML�ĸ�ʽ���������ռ���չ�ġ��������۵�`beans`�����ö��ǻ���XML�ģ�p�����ռ䲢���Ƕ�����XSD�ļ������Ƕ�����Spring Core֮�еġ�

����չʾ������XMLƬ����ͬ���Ľ����������һ��ʹ�õı�׼��XML��ʽ�����ڶ���ʹ����p�����ռ䡣

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="foo@bar.com"/>
    </bean>

    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="foo@bar.com"/>
</beans>
```
�����������Bean��չʾ��email���ԵĶ��塣���ֶ����֪Spring����һ�����Ե���������ǰ����������p�����ռ䲢û�б�׼ģʽ���壬����������������Ե�����Ϊ�������֡�

��������Ӱ�����2��Bean���壬�����������Bean��

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```

�������������п��Կ�����`john-modern`��ֹ����һ�����ԣ�Ҳͬʱʹ��������ĸ�ʽ������һ������ָ����һ��Bean����һ��Bean����ʹ�õ���`<property name="spouse" ref="jane"/>`��������Bean���õ�����һ��Bean�����ڶ���Bean�Ķ���ʹ����`p:spouse-ref="jane"`����Ϊһ��ָ��Bean�����á������������`spouse`�����Ե����֣���`-ref`���ֱ��������������ֱ�ӵ����ͣ�����������һ��Bean��

> p�����ռ䲢��ͬ��׼��XML��ʽһ�������磬�������Ե����ÿ��ܺ�һЩ��`Ref`��β���������ͻ������׼��XML��ʽ�Ͳ��ᡣSpring�Ŷ��Ƽ��������ܹ����Ŷ�����һ�£�Ҫʹ����һ�ַ�ʽ������Ҫͬʱʹ��3�ַ�����


### XML shortcut with the c-namespace

��p�����ռ����ƣ�c�����ռ�����Spring 3.1�״�����ģ�c�����ռ��������������������ù������������ʹ��`constructor-arg`Ԫ�ء�
�������һ��ʹ����c�����ռ�����ӣ�

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bar" class="x.y.Bar"/>
    <bean id="baz" class="x.y.Baz"/>

    <!-- traditional declaration -->
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
        <constructor-arg value="foo@bar.com"/>
    </bean>

    <!-- c-namespace declaration -->
    <bean id="foo" class="x.y.Foo" c:bar-ref="bar" c:baz-ref="baz" c:email="foo@bar.com"/>

</beans>
```

`c:`�����ռ�ʹ���˺�`p:`�����ռ������Ƶķ�ʽ��ʹ����`-ref`���������ã������ң�ͬ���ģ�c�����ռ�Ҳ���Ƕ�����XSD��ģʽ֮�У�������Spring Core֮�У���

������������֮�У����캯���Ĳ������ֲ������ã�ͨ��������ֽ���û��debug��Ϣ�ı��룩�������߿���ʹ����������ӣ�

```
<!-- c-namespace index declaration -->
<bean id="foo" class="x.y.Foo" c:_0-ref="bar" c:_1-ref="baz"/>
```

> ����XML�﷨�������ĸ���Ĵ���Ҫ��ʹ��`_`��ΪXML�������ֲ��������ֿ�ʼ��

ʵ���ϣ����캯���Ľ���������ƥ������Ǻܸ�Ч�ģ����Ǳ�Ҫ��Spring�Ŷ��Ƽ���������ʹ�������ռ䡣

### Compound property names

�����߿������������Ե�ʱ�����û�ϵ����ԣ�ֻҪ���е����·�����������һ���������֣�����Ϊ`null`��
�ο����µĶ��塣

```
<bean id="foo" class="foo.Bar">
    <property name="fred.bob.sammy" value="123" />
</bean>
```

`foo`��һ��`fred`�����ԣ�������`fred`������һ��`bob`���ԣ���`bob`����֮����һ��`sammy`���ԣ���ô������`sammy`���Ի�����Ϊ`123`����Ҫ�����������ܹ���Ч��`fred`������Ҫ��һ��`bob`��������`fred`����ֻ�ǹ���֮��Ϊ`null`��������׳�`NullPointerException`��

### 7.4.3 Using depends-on

���һ��Bean����һ��Bean�������Ļ���ͨ����˵���BeanҲ������һ��Bean������֮һ����������£������߿���������XMLԪ���ݵ�ʱ��ʹ��`<ref/>`��ǩ��Ȼ������ʱBean֮���������ϵ����ֱ�ӹ����ġ����磺��Ҫ������ľ�̬ʵ���������������������ݿ�����ע�ᡣ`depends-on`���Ի�ʹ��ȷ��ǿ��������Bean������֮ǰ�ͻ��ʼ�������������ʹ��`depends-on`�������ñ�ʾ����Bean�ϵ������ġ�

```
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />
```

�����Ҫ�������Bean�������ṩ���������Ϊ`depends-on`��ֵ���Զ��ţ��ո񣬻��߷ֺŷָ���£�

```
<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
```

> Bean�е�`depends-on`���Կ���ͬʱָ��һ����ʼ��ʱ��������Լ�һ����Ӧ������ʱ����������Bean������������Ķ�����`depends-on`���Ե�Bean���������٣����ڱ�`depends-on`��Bean�����٣�����`depends-on`���Կ������ٵ�˳��

### 7.4.4 Lazy-initialized beans

Ĭ������£�`ApplicationContext`����ʵ�����Ĺ����д������������еĵ���Bean���ܵ���˵�����Ԥ��ʼ���Ǻܲ���ġ���Ϊ�����ܼ�ʱ���ֻ����ϵ�һЩ���ô��󣬶�����ϵͳ�����˺ܾ�֮��ŷ��֡���������Ϊ����������Ҫ�ģ������߿���ͨ����Bean���Ϊ�ӳټ��ؾ�����ֹ���Ԥ��ʼ�����ӳٳ�ʼ����Bean��֪ͨIoC��Ҫ��BeanԤ��ʼ�������ڱ����õ�ʱ��Ż�ʵ������
��XML�У�����ͨ��`<bean/>`Ԫ�ص�`lazy-init`���������������Ϊ�����£�

```
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

����Bean����Ϊ�����XML��ʱ��`ApplicationContext`֮�е�`lazy`Bean�ǲ�������`ApplicationContext`�����������뵽Ԥ��ʼ��״̬�ģ�����Щ���ӳټ��ص�Bean�Ǵ���Ԥ��ʼ����״̬�ġ�

Ȼ�������һ���ӳټ��ص�������Ϊһ���������ӳټ��ص�Bean�����������ڵĻ���`ApplicationContext`��Ȼ����`ApplicationContext`������ʱ����أ���Ϊ��Ϊ����Bean�������������ŵ���Bean��ʵ������ʵ������
�����߿���ͨ��ʹ��`<beans/>`��`default-lazy-init`��������������ο���Bean�Ƿ��ӳٳ�ʼ�������磺

```
<beans default-lazy-init="true">
    <!-- no beans will be pre-instantiated... -->
</beans>
```

### 7.4.5 Autowiring collaborators

Spring�������Ը���Bean֮���������ϵ*�Զ�װ��*�������߿�����Springͨ��`ApplicationContext`�����Զ�������Щ�������Զ���װ���кܶ���ŵ㣺

* �Զ�װ���ܹ����Եļ���ָ�������Ի����ǹ��������
* �Զ�װ�ؿ�����չ�����ߵĶ��󡣱���˵�������������Ҫ��һ���������������ܹ�����Ҫ�������ر���ĸ������þ��ܹ��Զ����㡣�������Զ�װ���ڿ����������Ǽ��ȸ�Ч�ģ�������ȷ��ѡ��װ�ص�������ʹϵͳ���ӵ��ȶ���

��ʹ�û���XML��Ԫ�������õ�ʱ�򣬿����߿���ָ���Զ�װ��ķ�ʽ��ͨ������`<bean/>`Ԫ�ص�`autowire`���ԾͿ����ˡ��Զ�װ�����������ַ�ʽ�������߿���ָ��ÿ��Bean��װ�ط�ʽ������Bean��֪����μ����Լ���������

|ģʽ|����|
|----|----|
|no|��Ĭ�ϣ���װ�ء�Bean�����ñ���ͨ��`ref`Ԫ����ָ�������ڱȽϴ���Ŀ�Ĳ��𣬲������޸�Ĭ�ϵ����ã���Ϊ��ָ��Ӿ���ơ���ĳ�̶ֳ�����˵��Ĭ�ϵ���ʽҲ˵����ϵͳ�Ľṹ��|
|byName|ͨ��������װ�䡣Spring��������е�Beanֱ�����ֺ�������ͬ��һ��Bean������װ�ء�����˵�����Bean����Ϊ�����������Զ�װ�䣬��������һ����������Ϊ`master`(Ҳ���ǰ���һ��`setMaster(..)`����)��Spring�ͻ��������Ϊ`master`��Bean��Ȼ����֮װ��|
|byType|�����Ҫ�Զ�װ������Ե�����������֮�д��ڵĻ����ͻ��Զ�װ�䡣�������֮�д��ڲ�ֹһ������ƥ��Ļ����ͻ��׳�һ���ش���쳣��˵����������ò�Ҫʹ��byType���Զ�װ���Ǹ�Bean�����û��ƥ���Bean���ڵĻ��������׳��쳣��ֻ�����Բ������á�|
|���캯��|������*byType*��ע�룬����Ӧ�õĹ��캯���Ĳ��������û��һ��Bean�����ͺ͹��캯������������һ�£���ô��Ȼ���׳�һ���ش���쳣|

ͨ�� *byType* ���� *���캯��* ���Զ�װ�䷽ʽ�������߿���װ�������ǿ���ͼ��ϡ�����˵�����֮�У���������֮�е�ƥ��ָ�����͵�Bean���Զ�װ�䵽Bean�����������ע�롣�����߿����Զ�װ��keyΪ`String`��ǿ���͵�`Map`���Զ�װ���`Map`ֵ��������е�Beanʵ��ֵ��ƥ��ָ�������ͣ�`Map`��key�����������Bean�����֡�

### Limitations and disadvantages of autowiring

�Զ�װ���������������Ŀ�Ŀ���������ʹ�ã��Ṥ���ĺܺá������������ȫ��ʹ�ã���ֻ����֮���Զ�װ�伸��Bean�Ļ�����������Ի󿪷��ߡ�

������һЩ�Զ�װ������ƺ�����

* ��ȷ��`property`�Լ�`constructor-arg`�������ã��Ḳ�ǵ��Զ�װ������á��������ܹ��Զ�װ����ν�ļ����ԣ�����`Primitive`���ͻ����ַ�����
* �Զ�װ�䲢�о�ȷװ��׼ȷ������������ı���������Spring�ᾡ��С�������ⲻ��Ҫ�Ĵ���װ�䣬����Spring����Ķ����ϵ��Ȼ�����ĵ���������ô��ȷ��
* װ�����Ϣ�Կ����߿ɼ��Բ��ã���Ϊ��һ�ж���Spring��������
* �����еĿ��ܻ���ںܶ��Beanƥ��Setter�������߹������������˵���飬���ϻ���Map�ȡ�Ȼ������ȴϣ������һ��ƥ���ֵ����������Ϣ���޷������ġ����û�ж�һ�޶���Bean����ô�ͻ��׳��쳣��

�ں���ĳ����������������µ�ѡ��

* �����Զ�װ�������ھ�ȷװ��
* ����ͨ������`autowire-candidate`����Ϊ`false`����ֹ�Զ�װ��
* ͨ������`<bean/>`Ԫ�ص�`primary`����Ϊ`true`��ָ��һ��beanΪ��Ҫ�ĺ�ѡBean
* ʵ�ָ���Ļ���ע���ϸ���ȵ�װ�����á�


### Excluding a bean from autowiring

��ÿ��Bean�Ļ���֮�ϣ������߿�����ֹBean���Զ�װ�䡣�ڻ���XML�������У���������`<bean/>`Ԫ�ص�`autowire-candidate`����Ϊ`false`��������һ�㡣�����ڶ�ȡ��������ú󣬻������Bean�����Զ�װ��Ľṹ�в��ɼ�������ע����ʽ�����ñ���`@Autowired`��

�����߿���ͨ��ģʽƥ�������Bean�������������Զ�װ��ĺ�ѡ�ߡ����ϲ��`<beans/>`Ԫ�ػ���`default-autowire-candidates`�����������ö���ģʽ�����磬�����Զ�װ���ѡ�ߵ�������*Repository*��β����������`*Repository`�������Ҫ���ö���ģʽ��ֻ��Ҫ�ö��ŷָ������ɡ���ȻBean�����������`autowire-candidate`�Ļ��������Ϣӵ�и��ߵ����ȼ���

�������Щ������������Щ����Ҫ�Զ�װ���Bean�Ǻ���Ч�ġ���Ȼ�Ⲣ����˵����Bean�����޷��Զ�װ��������Bean������˵��ЩBean������Ϊ�Զ�װ�������ĺ�ѡ�ˡ�

### 7.4.6 Method injection

�ڴ������Ӧ�ó����£��������Bean���ǵ����ġ������������Bean��Ҫ����һ�������Ļ��߷ǵ�����Bean����ʹ�õ�ʱ�򣬿�����ֻ��Ҫ����������BeanΪ���Bean�����Լ��ɡ�������ʱ����Ϊ��ͬ��Bean�������ڵĲ�ͬ���������⡣���赥����Bean A��ÿ������������ʹ���˷ǵ�����Bean B������ֻ�ᴴ��Bean Aһ�Σ���ֻ��һ���������������ԡ���ô�������޷���Bean Aÿ�ζ��ṩһ���µ�Bean B��ʵ����

һ������������Ƿ���һЩIoC�������߿���ͨ��ʵ��`ApplicationContextAware`�ӿ���Bean A���Կ���`ApplicationContext`���Ӷ�ͨ������`getBean("B")`����Bean A ��Ҫ�µ�ʵ����ʱ������ȡ���µ�Bʵ�����ο���������ӣ�

```
// a class that uses a stateful Command-style class to perform some processing
package fiona.apple;

// Spring-API imports
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class CommandManager implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // grab a new instance of the appropriate Command
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    protected Command createCommand() {
        // notice the Spring API dependency!
        return this.applicationContext.getBean("command", Command.class);
    }

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

����Ĵ��벢��������ʮ�����⣬��Ϊҵ��Ĵ����Ѿ���Spring����������һ��Spring�ṩ��һ����΢�߼��ĵ����Է���ע��ķ�ʽ���������������������⡣

### Lookup method injection

���ҷ���ע���������һ�ָ�����������Bean�ķ����������ز��ҵ���һ�������е�Bean�����������ҷ���ͨ���Ͱ���ǰ�泡���ᵽ��Bean��Spring���ͨ��ʹ��CGLIB�����ɵ��ֽ�������̬�������������Ǹ���ķ���ʵ�ַ���ע�롣

> * Ϊ���������̬�����෽����������ôSpring��������Ҫ�̳е����Bean������`final`�ģ������ǵķ���Ҳ������`final`�ġ�
* ��������ĵ�Ԫ������Ϊ���ڳ��󷽷������Ա���ʵ������������
* ���ɨ�������ľ��巽��Ҳ��Ҫ�����ࡣ
* һ���ؼ����������ڲ��ҷ����빤�������ǲ���Эͬ�����ģ������ǲ��ܺ�������֮�е�`@Bean`�ķ�������Ϊ�������ڸ��𴴽�ʵ�������Ǵ���һ������ʱ�����ࡣ
* ��󣬱�ע��ĵ������Ķ����ܱ����л���

����ǰ��Ĵ���Ƭ���е�`CommandManager`�࣬���Ƿ��ַ���Spring�����ᶯ̬�ĸ���`createCommand()`������`CommandManager`�಻��ӵ���κε�Spring���������£�

```
package fiona.apple;

// no more Spring imports!

public abstract class CommandManager {

    public Object process(Object commandState) {
        // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```

�ڰ�����Ҫע��ķ����Ŀͻ����൱�У�ע��ķ�����Ҫ�����µĺ���ǩ�� 

```
<public|protected> [abstract] <return-type> theMethodName(no-arguments);
```

�������Ϊ������ô��̬���ɵ������ʵ��������������򣬶�̬���ɵ�����Ḳ�����еĶ����ԭ���������磺

```
<!-- a stateful bean deployed as a prototype (non-singleton) -->
<bean id="command" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- inject dependencies here as required -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="command"/>
</bean>
```

�����commandManager�ڵ�����Ҫһ��command Beanʵ����ʱ��ͻ�����Լ��ķ���`createCommand()`��������һ��Ҫ��������`command` Bean��Ϊprototype���͵�Bean����������BeanΪ�����ģ���ô�������ע�뷵�صĽ�����ͬһ��ʵ����

### Arbitrary method replacement

��ǰ��������У�����֪�����ҷ������������������κ������������Bean�ķ����ġ����������������һ���֣�����һ����Ҫʹ��������ܡ�

ͨ�����û���XML������Ԫ���ݣ������߿���ʹ��`replaced-method`Ԫ�����滻һ�����ڵķ�����ʵ�֡��������������

```
public class MyValueCalculator {

    public String computeValue(String input) {
        // some real code...
    }

    // some other methods...

}
```

һ��ʵ����`org.springframework.beans.factory.support.MethodReplacer`�ӿڵ�����ṩһ���·����Ķ��塣

```
/**
 * meant to be used to override the existing computeValue(String)
 * implementation in MyValueCalculator
 */
public class ReplacementComputeValue implements MethodReplacer {

    public Object reimplement(Object o, Method m, Object[] args) throws Throwable {
        // get the input value, work with it, and return a computed result
        String input = (String) args[0];
        ...
        return ...;
    }
}
```

�����Ҫ����Bean�ķ�����Ҫ����XML���£�

```
<bean id="myValueCalculator" class="x.y.z.MyValueCalculator">
    <!-- arbitrary method replacement -->
    <replaced-method name="computeValue" replacer="replacementComputeValue">
        <arg-type>String</arg-type>
    </replaced-method>
</bean>

<bean id="replacementComputeValue" class="a.b.c.ReplacementComputeValue"/>
```

�����߿���ʹ�ø����`<replaced-method>`�е�`<arg=type/>`Ԫ����ָ����Ҫ���ǵķ���������Ҫ���ǵķ����������ط���ʱ��ָ���������Ǳ���ġ�Ϊ�˷���������ַ����������ǻ�ƥ���������ͣ���ȫ��ͬ��`java.lang.String`��

```
java.lang.String
String
Str
```

��Ϊͨ����˵�����ĸ����Ѿ��㹻����ͬ�ķ����ˣ����ֿ�ݵ�д������ʡȥ�ܶ�Ĵ��롣