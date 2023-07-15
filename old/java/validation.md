# Validation

## Validation

### Javax Validation

3 base steps of validation

* Declaring constraint in model
* Getting validator from factory
* Validation object constraint violations

| [AssertFalse](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/AssertFalse.html)                                                                               | The annotated element must be false.                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [AssertFalse.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/AssertFalse.List.html)                                                                     | Defines several                                                                                       |
| [AssertTrue](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/AssertTrue.html)                                                                                 | The annotated element must be true.                                                                   |
| [AssertTrue.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/AssertTrue.List.html)                                                                       | Defines several                                                                                       |
| [DecimalMax](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/DecimalMax.html)                                                                                 | The annotated element must be a number whose value must be lower or  equal to the specified maximum.  |
| [DecimalMax.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/DecimalMax.List.html)                                                                       | Defines several                                                                                       |
| [DecimalMin](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/DecimalMin.html)                                                                                 | The annotated element must be a number whose value must be higher or  equal to the specified minimum. |
| [DecimalMin.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/DecimalMin.List.html)                                                                       | Defines several                                                                                       |
| [Digits](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Digits.html)                                                                                         |   null elements are considered valid.                                                                 |
| The annotated element must be a number within accepted range Supported types are: BigDecimal BigInteger CharSequence byte, short, int, long, and their respective wrapper types |                                                                                                       |
| [Digits.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Digits.List.html)                                                                               | Defines several                                                                                       |
| [Future](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Future.html)                                                                                         | The annotated element must be a date in the future.                                                   |
| [Future.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Future.List.html)                                                                               | Defines several                                                                                       |
| [Max](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Max.html)                                                                                               | The annotated element must be a number whose value must be lower or  equal to the specified maximum.  |
| [Max.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Max.List.html)                                                                                     | Defines several                                                                                       |
| [Min](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Min.html)                                                                                               | The annotated element must be a number whose value must be higher or  equal to the specified minimum. |
| [Min.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Min.List.html)                                                                                     | Defines several                                                                                       |
| [NotNull](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/NotNull.html)                                                                                       | The annotated element must not be null.                                                               |
| [NotNull.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/NotNull.List.html)                                                                             | Defines several                                                                                       |
| [Null](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Null.html)                                                                                             | The annotated element must be null.                                                                   |
| [Null.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Null.List.html)                                                                                   | Defines several                                                                                       |
| [Past](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Past.html)                                                                                             | The annotated element must be a date in the past.                                                     |
| [Past.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Past.List.html)                                                                                   | Defines several                                                                                       |
| [Pattern](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Pattern.html)                                                                                       | The annotated CharSequence must match the specified regular expression.                               |
| [Pattern.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Pattern.List.html)                                                                             | Defines several                                                                                       |
| [Size](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Size.html)                                                                                             | The annotated element size must be between the specified boundaries (included).                       |
| [Size.List](https://docs.oracle.com/javaee/7/api/javax/validation/constraints/Size.List.html)                                                                                   | Defines several                                                                                       |

My own annotations

```
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy=PersonAgeConstraintValidator.class)
public @interface PersonAgeConstraint {
    String message() default "{value.negative}";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

В message() указываем ключ (value.negative) из файла ресурса (ValidationMessages.properties) для сообщения

```
value.negative=Отрицательное\u0020значение
```

and validation class implementation

```
public class PersonAgeConstraintValidator implements ConstraintValidator<PersonAgeConstraint, Integer> {
    @Override
    public boolean isValid(Integer age, ConstraintValidatorContext constraintValidatorContext) {
        return age > 0;
    }
}
```

using

```
public class Person { 
    @Size(min=2, max=50) 
    private String Name; 
    @Digits(integer=3, fraction=0, message = "Не более 3-х знаков") 
    @PersonAgeConstraint 
    private Integer age; 
    public Person(String name, Integer age) { 
        Name = name; 
        this.age = age; 
    } 
}

public class DemoJValidationApplicationTests { 
    // Инициализация Validator 
    private static Validator validator; 
    static { 
        ValidatorFactory validatorFactory = Validation.buildDefaultValidatorFactory(); 
        validator = validatorFactory.usingContext().getValidator(); 
    } 
    @Test 
    public void testValidators() { 
        final Person person = new Person("Иван Петров", -4500); 
        Set<ConstraintViolation<Person>> validates = validator.validate(person); 
        Assert.assertTrue(validates.size() > 0); 
        validates.stream().map(v -> v.getMessage()) 
                .forEach(System.out::println); 
    } 
}
```

Messages for default annotations can define in ValidationMessages.properties

```
AnnotationName.entity.fieldname=сообщение
```

### Spring Validation

```
// validator
@Service 
public class PersonValidator implements Validator { 
    @Override 
    public boolean supports(Class<?> aClass) { 
        return Person.class.equals(aClass); 
    } 
    @Override 
    public void validate(Object obj, Errors errors) { 
        Person p = (Person) obj; 
        if (p.getAge() < 0) { 
            errors.rejectValue("age", "value.negative"); 
        } 
    } 
}

// validation
final Person person = new Person("Иван Петров", -4500); 
final DataBinder dataBinder = new DataBinder(person); 
dataBinder.addValidators(personValidator); 
dataBinder.validate();
```

###
