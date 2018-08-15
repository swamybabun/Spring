# Spring
Spring Framework

This Tutorial is for Spring Framework. Discusses all topis in Spring very clearly and quickly.

### IOC Container 

Responsibility of this is to instantiate the application class, to configure the object, to assemble the dependencies between the objects.

There are two types of IoC containers.Both are Interfaces. They are: 

- BeanFactory
- ApplicationContext : 

    BeanFactory + simple integration with Spring's AOP, message resource handling (for I18N), event propagation, application layer specific context (e.g. WebApplicationContext) for web application


| Constructor Injection | Setter Injection |
| --------------------- | ---------------- |
| No Partial Injection  |  Partial Injection possible (If there are 3 parameters, we can set 2 parameters to another setter)
| Does Not override the setter property |  Overrides the constructor property if both are defined.|
| Creates new instance if any modification occurs. | Doesn't create new instance if you change the property value. |
| Better for too many properties. | Better for few properties. |

### Bean Scopes :

- Singleton :   The bean instance will be only once and same instance will be returned by the IOC container. It is the default scope.
- Prototype :   The bean instance will be created each time when requested.
- Request :     The bean instance will be created per HTTP request. This will run in Web Aware ApplicationContext.
- Session :     The bean instance will be created per HTTP session. This will run in Web Aware ApplicationContext.
- Global session :     The bean instance will be created per HTTP global session. It can be used in portlet context only. This will run in Web Aware ApplicationContext.

### Spring Annotations:

- @Component	  :	Generic stereotype annotation for any Spring-managed component.
- @Controller	  :	Stereotypes a component as a Spring MVC controller.
- @Repository	  :	Stereotypes a component as a repository. Also indicates that SQLExceptions thrown from the component's methods should be translated into Spring DataAccessException.
- @Service	    :	Stereotypes a component as a service.
- @Resource     : means get me a known resource by name.
- @Inject or @Autowired : try to wire in a suitable other component by type.

<context:component-scan base-package="com.habuma.pirates" />
The base-package annotation tells Spring to scan com.habuma. pirates and all of its subpackages for beans to automatically register.

@Transactional : Declares transactional boundaries and rules on a bean and/or its methods.

The <tx:annotation-driven> element tells Spring to keep an eye out for beans that are annotated with @Transactional. In addition, you'll also need a platform transaction manager bean declared in the Spring context. For example, if your application uses Hibernate, you'll want to include the HibernateTransactionManager:

Example as below: 

    <bean id="transactionManager" class="org.springframework.orm.hibernate3. HibernateTransactionManager"> 
          <property name="sessionFactory" ref="sessionFactory" /> 
    </bean>
    
### AOP:
- PointCut : It's an expression to match join points.
- Join point : It is any point in your program such as method execution, exception handling, field access etc. Spring supports only method execution join point.
- Advice : an action taken by an aspect at a particular join point. 5 advices available.
- Aspect  :It is a class that contains advices, join points etc.
- Interceptor : It is an aspect that contains only one advice.


#### AOP Annotations :

- @Around advice	: Before and After (both throwing or returning).
- @After advice		: (both throwing or returning).
- @Before advice	: it executes before a join point.
- @AfterReturning advice : it executes after a joint point completes normally.It gets method returning value.
- @AfterThrowing advice : it executes if method exits by throwing an exception.


### Spring MVC:

The DispatcherServlet class works as the front controller in Spring MVC.
The @Controller annotation marks the class as controller class. It is applied on the class.
The @RequestMapping annotation maps the request with the method. It is applied on the method.


### Bean Lifecycle::

Instantiation
Populate Parameters
PopulateBeanName
PopulateBeanFactory
PreInitialization
AfterPropertiesSet
Call Custom init() method
PostInitialization
ReadyToUse

When shutdown

DisposableBean--> destroy() method 
Custom destroy() method
Then Shutdown

### Transaction Management:

- Programmatic transaction management

  This means that you have to manage the transaction with the help of programming. That gives you extreme flexibility, but it is difficult to maintain. DEVELOPER needs to code it explicitly.

- Declarative transaction management

  This means you separate transaction management from the business code. You only use annotations or XML-based configuration to manage the transactions.
We use <tx:advice /> tag for configuring this transaction management.

Sample Example:

    <tx:advice id = "txAdvice" transaction-manager = "transactionManager">
        <tx:attributes>
        <tx:method name = "create"/>
        </tx:attributes>
    </tx:advice>

We supply url, username, password to DriverManagerDataSource

    <bean id="dataSource" 
        class = "org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name = "driverClassName" value = "com.mysql.jdbc.Driver"/>
        <property name = "url" value = "jdbc:mysql://localhost:3306/TEST"/>
        <property name = "username" value = "root"/>
        <property name = "password" value = "cohondob"/>
    </bean>

We autowire this datasource to transaction manager.	

    <!-- Initialization for TransactionManager -->
       <bean id = "transactionManager"
          class = "org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name = "dataSource" ref = "dataSource" />    
       </bean>

### Transaction Isolation levels:

#### Transaction levels :

We Can add transaction level to the code using below annotation.

	@Transactional(isolation=TransactionDefinition.ISOLATION_DEFAULT)

Spring has five isolation levels.

- TransactionDefinition.ISOLATION_DEFAULT : Default from underlying database. For Oracle it is  READ_COMMITTED.
- TransactionDefinition.ISOLATION_SERIALIZABLE
- TransactionDefinition.ISOLATION_READ_COMMITTED
- TransactionDefinition.ISOLATION_READ_UNCOMMITTED
- TransactionDefinition.ISOLATION_REPEATABLE_READ

1. READ_UNCOMMITTED 
	
	This isolation level states that a transaction may read data that is still uncommitted by other transaction.vulnerable  to dirty reads, non-repeatable reads and phantom reads.
	
2. READ_COMMITTED
	
	This isolation level states that a transaction can't read data that is not yet committed by other transactions. This means that the dirty read is no longer an issue but vulnerable to non-repeatable reads and phantom reads.

3. REPEATABLE_READ
	
	This isolation level states that if a transaction reads one record from the database multiple times the result of all those reading operations must always be the same. This eliminates both the dirty read and the non-repeatable read issues, but vulnerable to phantom reads.

4. SERIALIZABLE
	
	This isolation level is the most restrictive of all isolation levels. Transactions are executed with locking at all levels (read, range and write locking) so they appear as if they were executed in a serialized way. This leads to a scenario where none of the issues mentioned above may occur, but in the other way we don't allow transaction concurrency and consequently introduce a performance penalty.

5. DEFAULT

	This isolation is from underlying database. This is springs default isolation level.

 
| | dirty reads | non-repeatable reads |  phantom reads |
| READ_UNCOMMITTED | yes | yes | yes |
| READ_UNCOMMITTED | no | yes | yes |
| REPEATABLE_READ | no | no | yes |
| SERIALIZABLE | no | no | no |


Ref : http://www.byteslounge.com/tutorials/spring-transaction-isolation-tutorial

#### Propagation Levels:

- REQUIRED : If transaction already exists then uses it. If not then create a new transaction.
- REQUIRES_NEW :  A new physical transaction will always be created by the container.
- NESTED :  Will run in nested transactions behaviour
- MANDATORY : opened transaction must already exist, else exception thrown.
- NEVER :  existing opened transaction must not already exist if exists, exception is thrown.
- NOT_SUPPORTED : Executes outside of the scope of any transaction.
- SUPPORTS :  If transaction exists, it runs in scope of that. If not exists, it runs in non-transactional way.


Spring REQUIRED behavior means that the same transaction will be used if there is an already opened transaction in the current bean method execution context. If there is no existing transaction the Spring container will create a new one. If multiple methods configured as REQUIRED behavior are called in a nested way they will be assigned distinct logical transactions but they will all share the same physical transaction. In short this means that if an inner method causes a transaction to rollback, the outer method will fail to commit and will also rollback the transaction

REQUIRES_NEW behavior means that a new physical transaction will always be created by the container. In other words the inner transaction may commit or rollback independently of the outer transaction, i.e. the outer transaction will not be affected by the inner transaction result: they will run in distinct physical transactions.

The NESTED behavior makes nested Spring transactions to use the same physical transaction but sets savepoints between nested invocations so inner transactions may also rollback independently of outer transactions. This may be familiar to JDBC aware developers as the savepoints are achieved with JDBC save points, so this behavior should only be used with Spring JDBC managed transactions

The MANDATORY behavior states that an existing opened transaction must already exist. If not an exception will be thrown by the container.

The NEVER behavior states that an existing opened transaction must not already exist. If a transaction exists an exception will be thrown by the container.

The NOT_SUPPORTED behavior will execute outside of the scope of any transaction. If an opened transaction already exists it will be paused.

The SUPPORTS behavior will execute in the scope of a transaction if an opened transaction already exists. If there isn't an already opened transaction the method will execute anyway but in a non-transactional way.

Ref : http://www.byteslounge.com/tutorials/spring-transaction-propagation-tutorial


Thanks.
