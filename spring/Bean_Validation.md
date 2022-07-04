# Bean Validation

- javax.validation.constraints
````
@NotNull: null 검증
@Null: null 검증
@Min: 최소값
@Max: 최대값
@Size: 길이, 크기 범위 검증
@Email: e-mail 검증
@Pattern: 정규 표현식 검증
@AssertTrue: true 검증
@AssertFalse: false 검증
@NotEmpty: null이나 size가 0 검증 (CharSequence, Collection, Map, 배열)
@NotBlank: null이나 whitespace 검증 (CharSequence)
@Positive: 양수 검증
@PositiveOrZero: 0 또는 양수 검증
@Negative: 음수 검증
@NegativeOrZero: 0 또는 음수 검증
@Past: 해당 시간이 과거 시간인지 검증
@PastOrPresent: 현재 또는 과거 시간인지 검증
@Future: 해당 시간이 미래 시간인지 검증
@FutureOrPresent: 현재 또는 미래 시간인지 검증
````

- 검증이 필요한 필드에 어노테이션 추가
````
public class PersonForm {

    @NotNull
    @Size(min=2, max=30)
    private String name;

    @NotNull
    @Min(18)
    private Integer age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String toString() {
        return "Person(Name: " + this.name + ", Age: " + this.age + ")";
    }
}
````

- @Valid 어노테이션 추가
````
@Controller
public class WebController implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/results").setViewName("results");
    }

    @GetMapping("/")
    public String showForm(PersonForm personForm) {
        return "form";
    }

    @PostMapping("/")
    public String checkPersonInfo(@Valid PersonForm personForm, BindingResult bindingResult) {

        if (bindingResult.hasErrors()) {
            return "form";
        }

        return "redirect:/results";
    }
}
````
### collection validation
1. use a custom list class that functions as both a List and a JavaBean
````
@RequestBody @Valid ValidList<CompanyTag> categories
````
````
public class ValidList<E> implements List<E> {

    @Valid
    private List<E> list;

    public ValidList() {
        this.list = new ArrayList<E>();
    }

    public ValidList(List<E> list) {
        this.list = list;
    }

    // Bean-like methods, used by javax.validation but ignored by JSON parsing

    public List<E> getList() {
        return list;
    }

    public void setList(List<E> list) {
        this.list = list;
    }

    // List-like methods, used by JSON parsing but ignored by javax.validation

    @Override
    public int size() {
        return list.size();
    }

    @Override
    public boolean isEmpty() {
        return list.isEmpty();
    }

    // Other list methods ...
}
````

2. @Valiated annotation on the class.
````
@Validated
@RestController
@RequestMapping("/parent")
public class ParentController {

  private FatherRepository fatherRepository;

  /**
   * DI
   */
  public ParentController(FatherRepository fatherRepository) {
    this.fatherRepository = fatherRepository;
  }

  @PostMapping("/test")
  public void test(@RequestBody @Valid List<Father> fathers) {

  }
}
````

## Custom Validation
````
@Documented
@Constraint(validatedBy = ContactNumberValidator.class)
@Target( { ElementType.METHOD, ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
public @interface ContactNumberConstraint {
    String message() default "Invalid phone number";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
````
````
public class ContactNumberValidator implements 
  ConstraintValidator<ContactNumberConstraint, String> {

    @Override
    public void initialize(ContactNumberConstraint contactNumber) {
    }

    @Override
    public boolean isValid(String contactField,
      ConstraintValidatorContext cxt) {
        return contactField != null && contactField.matches("[0-9]+")
          && (contactField.length() > 8) && (contactField.length() < 14);
    }

}
````
````
@Controller
public class ValidatedPhoneController {
 
    @GetMapping("/validatePhone")
    public String loadFormPage(Model m) {
        m.addAttribute("validatedPhone", new ValidatedPhone());
        return "phoneHome";
    }
    
    @PostMapping("/addValidatePhone")
    public String submitForm(@Valid ValidatedPhone validatedPhone,
      BindingResult result, Model m) {
        if(result.hasErrors()) {
            return "phoneHome";
        }
        m.addAttribute("message", "Successfully saved phone: "
          + validatedPhone.toString());
        return "phoneHome";
    }   
}
````

- https://spring.io/guides/gs/validating-form-input/
- https://stackoverflow.com/questions/28150405/validation-of-a-list-of-objects-in-spring
