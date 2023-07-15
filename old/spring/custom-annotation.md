# Custom Annotation

[https://www.baeldung.com/spring-annotation-bean-pre-processor](https://www.baeldung.com/spring-annotation-bean-pre-processor)

On example of GenericDao

```
public class GenericDao<E> {
 
    private Class<E> entityClass;
 
    public GenericDao(Class<E> entityClass) {
        this.entityClass = entityClass;
    }
 
    public List<E> findAll() {
        // ...
    }
 
    public Optional<E> persist(E toPersist) {
        // ...
    }
}
```

Create annotation

```
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.METHOD})
@Documented
public @interface DataAccess {
    Class<?> entity();
}

using:
@DataAccess(entity=Person.class)
private GenericDao<Person> personDao;
```

Configure BeanPostProcessor

```
@Component
public class DataAccessAnnotationProcessor implements BeanPostProcessor {
 
    private ConfigurableListableBeanFactory configurableBeanFactory;
 
    @Autowired
    public DataAccessAnnotationProcessor(ConfigurableListableBeanFactory beanFactory) {
        this.configurableBeanFactory = beanFactory;
    }
 
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) 
      throws BeansException {
        this.scanDataAccessAnnotation(bean, beanName);
        return bean;
    }
 
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) 
      throws BeansException {
        return bean;
    }
 
    protected void scanDataAccessAnnotation(Object bean, String beanName) {
        this.configureFieldInjection(bean);
    }
 
    private void configureFieldInjection(Object bean) {
        Class<?> managedBeanClass = bean.getClass();
        FieldCallback fieldCallback = 
          new DataAccessFieldCallback(configurableBeanFactory, bean);
        ReflectionUtils.doWithFields(managedBeanClass, fieldCallback);
    }
}
```

Implementation _DataAccessFieldCallback_

```
public class DataAccessFieldCallback implements FieldCallback {
    private static Logger logger = LoggerFactory.getLogger(DataAccessFieldCallback.class);
     
    private static int AUTOWIRE_MODE = AutowireCapableBeanFactory.AUTOWIRE_BY_NAME;
 
    private static String ERROR_ENTITY_VALUE_NOT_SAME = "@DataAccess(entity) "
            + "value should have same type with injected generic type.";
    private static String WARN_NON_GENERIC_VALUE = "@DataAccess annotation assigned "
            + "to raw (non-generic) declaration. This will make your code less type-safe.";
    private static String ERROR_CREATE_INSTANCE = "Cannot create instance of "
            + "type '{}' or instance creation is failed because: {}";
 
    private ConfigurableListableBeanFactory configurableBeanFactory;
    private Object bean;
 
    public DataAccessFieldCallback(ConfigurableListableBeanFactory bf, Object bean) {
        configurableBeanFactory = bf;
        this.bean = bean;
    }
 
    @Override
    public void doWith(Field field) 
    throws IllegalArgumentException, IllegalAccessException {
        if (!field.isAnnotationPresent(DataAccess.class)) {
            return;
        }
        ReflectionUtils.makeAccessible(field);
        Type fieldGenericType = field.getGenericType();
        // In this example, get actual "GenericDAO' type.
        Class<?> generic = field.getType(); 
        Class<?> classValue = field.getDeclaredAnnotation(DataAccess.class).entity();
 
        if (genericTypeIsValid(classValue, fieldGenericType)) {
            String beanName = classValue.getSimpleName() + generic.getSimpleName();
            Object beanInstance = getBeanInstance(beanName, generic, classValue);
            field.set(bean, beanInstance);
        } else {
            throw new IllegalArgumentException(ERROR_ENTITY_VALUE_NOT_SAME);
        }
    }
 
    public boolean genericTypeIsValid(Class<?> clazz, Type field) {
        if (field instanceof ParameterizedType) {
            ParameterizedType parameterizedType = (ParameterizedType) field;
            Type type = parameterizedType.getActualTypeArguments()[0];
 
            return type.equals(clazz);
        } else {
            logger.warn(WARN_NON_GENERIC_VALUE);
            return true;
        }
    }
 
    public Object getBeanInstance(
      String beanName, Class<?> genericClass, Class<?> paramClass) {
        Object daoInstance = null;
        if (!configurableBeanFactory.containsBean(beanName)) {
            logger.info("Creating new DataAccess bean named '{}'.", beanName);
 
            Object toRegister = null;
            try {
                Constructor<?> ctr = genericClass.getConstructor(Class.class);
                toRegister = ctr.newInstance(paramClass);
            } catch (Exception e) {
                logger.error(ERROR_CREATE_INSTANCE, genericClass.getTypeName(), e);
                throw new RuntimeException(e);
            }
             
            daoInstance = configurableBeanFactory.initializeBean(toRegister, beanName);
            configurableBeanFactory.autowireBeanProperties(daoInstance, AUTOWIRE_MODE, true);
            configurableBeanFactory.registerSingleton(beanName, daoInstance);
            logger.info("Bean named '{}' created successfully.", beanName);
        } else {
            daoInstance = configurableBeanFactory.getBean(beanName);
            logger.info(
              "Bean named '{}' already exists used as current bean reference.", beanName);
        }
        return daoInstance;
    }
}
```
