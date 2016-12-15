Customing the nature of a bean
===

### Lifecycle callbacks

������ͨ��ʵ��Spring��`InitializeingBean`��`DisposableBean`�ӿڣ��Ϳ���������������Bean���������ڡ��������ڵ���`afterPropertiesSet()`֮���`destroy()`֮ǰ������Bean�ڳ�ʼ��������Bean��ʱ��ִ��һЩ������

> JSR-250��`@PostConstruct`��`@PreDestroy`ע������ִ�SpringӦ���������ڻص������ʵ����ʹ����Щע����ζ��Bean�����������Spring�ض��Ľӿ��ϡ���ϸ���ݣ�����������ܡ�
��������߲���ʹ��JSR-250��ע�⣬��Ȼ���Կ���ʹ��`init-method`��`destroy-method��`����������Spring�ӿڡ�

�ڲ���˵��Spring���ʹ��`BeanPostProcessor`��ʵ��������ӿڵĻص���`BeanPostProcessor`�ܹ��ҵ������ú��ʵķ����������������Ҫ����һЩSpring����ֱ���ṩ������������Ϊ�������߿��Կ�������ʵ��һ��`BeanPostProcessor`���������Ϣ���Բο������������չ�㡣

���˳�ʼ�������ٻص���Spring����Ķ���Ҳʵ����`Lifecycle`�ӿ����ù���Ķ��������������������������͹رա�

�������ڻص��ڱ��ڻ������ϸ������

#### Initialization callbacks

`org.springframework.beans.factory.InitializingBean`�ӿ�����Bean�����еı�Ҫ����������������ɺ���ִ�г�ʼ��Bean�Ĳ�����`InitializingBean`�ӿ�����ָ��һ��������

```
void afterPropertiesSet() throws Exception;
```

Spring�Ŷ��ǽ��鿪���߲�Ҫʹ��`InitializingBean`�ӿڵģ���Ϊ�����Ὣ������ϵ�Spring���ض��ӿ�֮�ϡ���ͨ��ʹ��`@PostConstruct`ע�����ָ��һ��POJO��ʵ�ַ��������ʵ�ֽӿ�Ҫ���á��ڻ���XML������Ԫ�����ϣ������߿���ʹ��`init-method`������ָ��һ��û�в����ķ�����ʹ��Java���õĿ����߿���ʹ��`@Bean`֮�е�`initMethod`���ԣ��������£�

```
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
```

```
public class ExampleBean {

    public void init() {
        // do some initialization work
    }

}
```

�����´���һ��Ч����

```
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```
public class AnotherExampleBean implements InitializingBean {

    public void afterPropertiesSet() {
        // do some initialization work
    }

}
```

����ǰһ���汾�Ĵ�����û����ϵ�Spring�ġ�

#### Destruction callbacks

ʵ����`org.springframework.beans.factory.DisposableBean`�ӿڵ�Bean����ͨ������ͨ���ص�������Bean�����õ���Դ��`DisposableBean`�ӿڰ�����һ��������

```
void destroy() throws Exception;
```

ͬInitializingBean�����ƣ�Spring�Ŷ���Ȼ�����鿪������ʵ��`DisposableBean`�ص��ӿڣ���Ϊ�����Ὣ�����ߵĴ�����ϵ�Spring�����ϡ����ַ�ʽ������ʹ��`@PreDestroy`ע�����ָ��һ��Bean֧�ֵ����÷����������ڻ���XML������Ԫ�����У������߿�����Bean��ǩ��ָ��`destroy-method`���ԡ����ڻ���Java�����У�������Ҳ��������`@Bean`��`destroyMethod`��ʵ�����ٻص���

```
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```

```
public class ExampleBean {

    public void cleanup() {
        // do some destruction work (like releasing pooled connections)
    }

}
```

����Ĵ������ú����������ǵ�ͬ�ģ�

```
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```
public class AnotherExampleBean implements DisposableBean {

    public void destroy() {
        // do some destruction work (like releasing pooled connections)
    }

}
```

���ǵ�һ�δ�����û����ϵ�Spring�ġ�

> `<bean/>`��ǩ��`destroy-method`���Ա�����Ϊ����ָ����ֵ����������Spring�ܹ��Զ��ļ�鵽`close`����`shutdown`����������ʵ��`java.lang.AutoCloseable`����`java.io.Closeable`����ƥ�䡣���������ָ����ֵ�������õ�`<beans/>`��`default-destroy-method`�������е�Beanʵ�������Ϊ��

#### Default initialization and destroy methods

�������߲�ʹ��Spring���е�`InitializingBean`��`DisposableBean`�ص��ӿ���ʵ�ֳ�ʼ�������ٷ�����ʱ�򣬿�����ͨ������ķ������ֶ��Ǻ���`init()`��`initialize()`������`dispose()`�ȵȡ���ô�����Խ�����ķ�������Ŀ�б�׼�����������еĿ����߶�ʹ��һ����������ȷ��һ���ԡ�

�����߿�������Spring���������ÿһ��Bean�������������ֵĳ�ʼ�������ٻص�������Ҳ����˵���κε�һ��Ӧ�ÿ����ߣ�������Ӧ�õ�����ʹ��һ����`init()`�ĳ�ʼ���ص���������Ҫ��ÿ��Bean�ж���`init-method="init"`�������ԡ�Spring IoC��������Bean������ʱ������Ǹ�����������ǰ�������ı�׼��������һ�������������Ҳ��ǿ�ƿ�����Ϊ�����ĳ�ʼ���Լ����ٻص�����ʹ��ͬ�������֡�

���迪���ߵĳ�ʼ���ص���������Ϊ`init()`�����ٵĻص�����Ϊ`destroy()`����ô�����ߵ���ͻ�������µĴ��룺

```
public class DefaultBlogService implements BlogService {

    private BlogDao blogDao;

    public void setBlogDao(BlogDao blogDao) {
        this.blogDao = blogDao;
    }

    // this is (unsurprisingly) the initialization callback method
    public void init() {
        if (this.blogDao == null) {
            throw new IllegalStateException("The [blogDao] property must be set.");
        }
    }

}
```

```
<beans default-init-method="init">

    <bean id="blogService" class="com.foo.DefaultBlogService">
        <property name="blogDao" ref="blogDao" />
    </bean>

</beans>
```

`<beans/>`��ǩ�����`default-init-method`���Ի���Spring IoC����ʶ�����`init`�ķ�������ΪBean�ĳ�ʼ���ص���������Bean������װ��֮�����Bean����ôһ�������Ļ���Spring�����ͻ��ں��ʵ�ʱ����á�

���Ƶģ�������Ҳ��������Ĭ�����ٻص�����������XML�����þ���`<beans/>`��ǩ����ʹ��`default-destroy-method`���ԡ�

������һЩBean��������һЩ�ص��������������õ�Ĭ�ϻص�������ͬ��ʱ�򣬿����߿���ͨ����ָ�ķ�ʽ�����ǵ�Ĭ�ϵĻص���������XMLΪ��������ͨ��ʹ��`<bean>`��ǩ��`init-method`��`destroy-method`�����ǵ�`<beans/>`�е����á�

Spring�������������±�֤��Bean����װ�������е������Ժ����̾Ϳ�ʼִ�г�ʼ���ص��������Ļ�����ʼ���ص�ֻ����ֱ�ӵ�Bean����װ�غú���ã���AOP��������û��Ӧ�õ�Bean�ϡ�����Ŀ��Bean����ȫ��ʼ���ã�Ȼ��AOP�����Լ�������������Ӧ�á����Ŀ��Bean�Լ������Ƿֿ�����ģ���ô�����ߵĴ���������������AOP��ֱ�Ӻ����õ�Bean��������ˣ��ڳ�ʼ��������Ӧ����������ǰ��ì�ܣ���Ϊ�����������Ŀ��Bean���������ںʹ���/��������������ΪͬBeanֱ�ӽ�����������ֵ�����

#### Combining lifecycle mechanisms

��Spring 2.5֮�󣬿�����������ѡ��������Bean������������Ϊ��

* `InitializingBean`��`DisposableBean`�ص��ӿ�
* �Զ����`init()`�Լ�`destroy`����
* ʹ��`@PostConstruct`�Լ�`@PreDestroy`ע��

������Ҳ������Bean��������Щ����һ��ʹ��

> ���Bean�����˶���������ڻ��ƣ�����ÿ�����������˲�ͬ�ķ������֣���ôÿ�����õķ����ᰴ�պ���������˳����ִ�С�Ȼ���������������ͬ�����֣�����˵��ʼ���ص�Ϊ`init()`���ڲ�ֹһ���������ڻ�������Ϊ�������������£��������ֻ��ִ��һ�Ρ�

���һ��Bean�����˶���������ڻ��ƣ����Һ��в�ͬ�ķ�������ִ�е�˳�����£�

* ����`@PostConstruct`ע��ķ���
* ��`InitializingBean`�ӿ��е�`afterPropertiesSet()`����
* �Զ����`init()`����

���ٷ�����ִ��˳��ͳ�ʼ����ִ��˳����ͬ��

* ����`@PreDestroy`ע��ķ���
* ��`DisposableBean`�ӿ��е�`destroy()`����
* �Զ����`destroy()`����

#### Startup and shutdown callbacks

`Lifecycle`�ӿ���Ϊ�κ����Լ�������������Ķ�������һЩ�����ķ���������������ֹͣһЩ��̨���̣�:

```
public interface Lifecycle {

    void start();

    void stop();

    boolean isRunning();

}
```

�κ�Spring����Ķ��󶼿�ʵ������Ľӿڡ���ô��`ApplicationContext`�����յ�����������ֹͣ���ź�ʱ����������ʱ��ֹͣ���������ȳ�����`ApplicationContext`��֪ͨ�������������а������������ڶ���`ApplicationContext`ͨ��������`LifecycleProcessor`�������������е�`Lifecycle`��ʵ�ֶ���

```
public interface LifecycleProcessor extends Lifecycle {

    void onRefresh();

    void onClose();

}
```

������������ǿ��Է���`LifecycleProcessor`��`Lifecycle`�ӿڵ���չ��`LifecycleProcessor`�����������������������������ĵ�ˢ�º͹ر�������Ӧ��

> �����`org.springframework.context.Lifecycle`�ӿ�ֻ��Ϊ��ȷ�Ŀ�ʼ/ֹ֪ͣͨ�ṩһ����Լ����������ʾ��������ˢ�»��Զ���ʼ������ʵ��`org.springframework.context.SmartLifecycle`�ӿ������ȡ����ĳ��Bean���Զ��������̣����������׶Σ���ͬʱ��ֹ֪ͣͨ�����ܱ�֤������֮ǰ���֣��������Ĺر�����£����е�`Lifecycle`Bean���������ٻص�׼����֮ǰ�յ�ֹ֪ͣͨ��Ȼ�����������Ĵ���ڵ���ˢ�»���ֹͣˢ�³��Ե�ʱ��ֻ��������ٷ�����

�����͹رյ����Ǻ���Ҫ�ġ������ͬ��Bean֮�����`depends-on`�Ĺ�ϵ�Ļ�����������һ����Ҫ��������������ҹرյĸ��硣Ȼ�����е�ʱ��ֱ�ӵ�������δ֪�ģ��������߽���֪����һ��������Ҫ������г�ʼ��������������£�`SmartLifecycle`�ӿڶ�������һ��ѡ������丸�ӿ�`Phased`�е�`getPhase()`������

```
public interface Phased {

    int getPhase();

}
```

```
public interface SmartLifecycle extends Lifecycle, Phased {

    boolean isAutoStartup();

    void stop(Runnable callback);

}
```

������ʱ��ӵ����͵�`phased`�Ķ������������������ر�ʱ�����෴��˳����ˣ����һ������ʵ����`SmartLifecycle`Ȼ������`getPhase()`����������`Integer.MIN_VALUE`�Ļ����ͻ��øö����������������������١���Ȼ�����`getPhase()`����������`Integer.MAX_VALUE`��˵���˸ö�����������������������١������ǵ�ʹ��`phased`��ֵ��ʱ��Ҳͬʱ��Ҫ�˽�����û��ʵ��`SmartLifecycle`��`Lifecycle`�����Ĭ��ֵ�����ֵΪ0����ˣ��κθ�ֵ�����������ڱ�׼�������֮ǰ�������ڱ�׼��������Ժ��ٽ������١�

`SmartLifecycle`�ӿ�Ҳ������һ��`stop`�Ļص��������κ�ʵ����`SmartLifecycle`�ӿڵĺ����������ڹر��������֮����ûص��е�`run()`����������������ʹ���첽�رա���`LifecycleProcessor`��Ĭ��ʵ��`DefaultLifecycleProcessor`��ȵ����õĳ�ʱʱ��֮���ٵ��ûص���Ĭ�ϵ�ÿһ�׶εĳ�ʱʱ��Ϊ30�롣�����߿���ͨ������һ������`lifecycleProcessor`��Bean������Ĭ�ϵ��������ڴ������������������Ҫ���ó�ʱʱ�䣬����ͨ�����´���������ã�

```
<bean id="lifecycleProcessor" class="org.springframework.context.support.DefaultLifecycleProcessor">
    <!-- timeout value in milliseconds -->
    <property name="timeoutPerShutdownPhase" value="10000"/>
</bean>
```

ǰ���ᵽ�ģ�`LifecycleProcessor`�ӿڶ����˻ص�������ˢ�º͹ر������ġ��رյĻ������`stop()`�����Ѿ���ȷ�����ˣ���ô�ͻ������رյ����̣�������������������ڹرվͲ��ᷢ�������������ˢ�µĻص���ʹ��`SmartLifecycle`����һ�����ԡ���������ˢ����ϣ����еĶ����Ѿ�ʵ��������ʼ��������ô�ͻ���ûص���Ĭ�ϵ��������ڴ���������ÿһ��`SmartLifecycle`�����`isAutoStartup()`���ص�Boolֵ�����Ϊ�棬���󽫻��Զ����������ǵȴ���ȷ�������ĵ��ã����ߵ����Լ���`start()`����(��ͬ��������ˢ�£���׼��������ʵ���ǲ����Զ�������)��`phased`��ֵ�Լ�`depends-on`��ϵ������������������ٵ�˳��

#### Shutting down the Spring IoC container gracefully in non-web applications


> ��һ����ֻ������ڷ�Web��Ӧ�á�Spring�Ļ���web��`ApplicationContext`ʵ���Ѿ��д�����webӦ�ùرյ�ʱ���ܹ��Զ��ĵĹر�Spring IoC������

����������ڷ�webӦ�û���ʹ��Spring IoC�����Ļ��� ���磬������ͻ��˵Ļ����£���������Ҫ��JVM��ע��һ���رյĹ��ӣ���ȷ���ڹر�Spring IoC������ʱ���ܹ�������ص����ٷ������ͷŵ����õ���Դ����Ȼ��������Ҳ����Ҫ��ȷ�����ú�ʵ����Щ���ٻص���

�����߿�����`ConfigurableApplicationContext`�ӿڵ���`registerShutdownHook()`��ע�����ٵĹ��ӣ�

```
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public final class Boot {

    public static void main(final String[] args) throws Exception {

        ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext(
                new String []{"beans.xml"});

        // add a shutdown hook for the above context...
        ctx.registerShutdownHook();

        // app runs here...

        // main method exits, hook is called prior to the app shutting down...

    }
}
```

### ApplicationContextAware and BeanNameAware

��`ApplicationContext`�ڴ���ʵ����`org.springframework.context.ApplicationContextAware`�ӿڵĶ���ʱ���ö����ʵ�������һ����`ApplicationContext`�����á�

```
public interface ApplicationContextAware {

    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;

}
```

����Bean���ܹ�ͨ����̵ķ�ʽ�����ʹ���`ApplicationContext`�ˡ�ͨ��`ApplicationContext`�ӿڣ�����ͨ��������ת������֪�Ľӿڵ����࣬����`ConfigurableApplicationContext`���ܹ��ṩһЩ����Ĺ��ܡ����е�һ���÷����ǿ���ͨ����̵ķ�ʽ����ȡ������Bean���е�ʱ��������������á���Ȼ��Spring�Ŷ��Ƽ���ò�Ҫ����������Ϊ��������ϴ��뵽Spring�ϣ�ͬʱҲû����ѭIoC�ķ��`ApplicationContext`�������ķ��������ṩһЩ��������Դ�ķ��ʣ�����Ӧ���¼������߽���`MessageSource`֮��Ĺ��ܡ���Щ��Ϣ�ں��������`ApplicationContext`�������лὲ����

��Spring 2.5�汾�У��Զ�װ��Ҳ�ǻ��`ApplicationContext`��һ�ַ�ʽ����ͳ�Ĺ��캯����ͨ�����͵�װ�ط�ʽ��ǰ��[����](Dependencies.md)�����������������ͨ�����캯��������setter�����ķ�ʽע�룬������Ҳ����ͨ��ע��ע��ķ�ʽ��

��`ApplicationContext`������һ��ʵ����`org.springframework.beans.factory.BeanNameAware`�ӿڵ��࣬��ô�����Ϳ�����������ֽ������á�

```
public interface BeanNameAware {

    void setBeanName(string name) throws BeansException;

}
```

����ص��ĵ��ô��������������Ժ󣬵��ǳ�ʼ���ص�֮ǰ������`InitializingBean`��`afterPropertiesSet()`�����Լ��Զ���ĳ�ʼ�������ȡ�

### Other Aware interfaces

������������������Aware�ӿڣ�Spring���ṩ��һϵ�е�`Aware`�ӿ�����Bean������������ЩBean��ҪһЩ�����*������ʩ*��Ϣ������Ҫ��һЩ`Aware`�ӿڶ���������н�����������

|����|ע�������|
|----|----------|
|`ApplicationContextAware`|������`ApplicationContext`|
|`ApplicationEventPlulisherAware`|`ApplicationContext`�е��¼�������|
|`BeanClassLoaderAware`|����Beanʹ�õ��������|
|`BeanFactoryAware`|������`BeanFactory`|
|`BeanNameAware`|Bean������|
|`BootstrapContextAware`|�������е���Դ������`BootstrapContext`��ͨ������JCA��������Ч|
|`LoadTimeWeaverAware`|�����ڼ䴦���ඨ���weaver|
|`MessageSourceAware`|������Ϣ�����ò���|
|`NotificationPublisherAware`|Spring JMX֪ͨ������|
|`PortletConfigAware`|������ǰ���е�`PortletConfig`������web�µ�Spring `ApplicationContext`�пɼ�|
|`PortletContextAware`|������ǰ���е�`PortletContext`������web�µ�Spring `ApplicationContext`�пɼ�|
|`ResourceLoaderAware`|���õ���Դ������|
|`ServletConfigAware`|������ǰ���е�`ServletConfig`������web�µ�Spring `ApplicationContext`�пɼ�|
|`ServletContextAware`|������ǰ���е�`ServletContext`������web�µ�Spring `ApplicationContext`�пɼ�|


�ٴε�������������Щ�ӿڵ�ʹ����Υ��IoCԭ��ģ����Ǳ�Ҫ����ò�Ҫʹ�á�
