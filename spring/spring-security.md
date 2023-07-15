# Spring Security

* SecurityContextHolder - keeps security information include principal (user)
* SecurityContext
* Authentication - authorized user session object
* UserDetails - principal object
* UserDetailsService

```
// with a single method
UserDetails loadUserByUsername(String username) throws UsernameNotFoundException
```

Due to UserDetailService we can use UserDetails object

```
Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal();
if (principal instanceof UserDetails) {
  String username = ((UserDetails)principal).getUsername();
} else {
  String username = principal.toString();
}
```

## using

pom.xml

```
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

configuring - **WebSecurityConfig**

```
@Configuration 
@EnableWebSecurity 
public class WebSecurityConfig extends WebSecurityConfigurerAdapter { 
    @Autowired 
    UserService userService; 
    // this bean is used for password encrypt
    @Bean 
    public BCryptPasswordEncoder bCryptPasswordEncoder() { 
        return new BCryptPasswordEncoder(); 
    } 
    @Override 
    protected void configure(HttpSecurity httpSecurity) throws Exception { 
        httpSecurity 
                .csrf() 
                    .disable() 
                .authorizeRequests() 

                    //Доступ только для не зарегистрированных пользователей 
                    .antMatchers("/registration").not().fullyAuthenticated() 

                    //Доступ только для пользователей с ролью
                    .antMatchers("/admin/**").hasRole("ADMIN") 
                    .antMatchers("/news").hasRole("USER") 

                    //Доступ разрешен всем пользователей 
                    .antMatchers("/", "/resources/**").permitAll() 

                //Все остальные страницы требуют аутентификации 
                .anyRequest().authenticated() 
                .and() 
                    //Настройка для входа в систему 
                    .formLogin() 
                    .loginPage("/login") 
                    //Перенарпавление на главную страницу после успешного входа 
                    .defaultSuccessUrl("/") 
                    .permitAll() 
                .and() 
                    .logout() 
                    .permitAll() 
                    .logoutSuccessUrl("/"); 
    } 
    @Autowired 
    protected void configureGlobal(AuthenticationManagerBuilder auth) throws Exception { 
        auth.userDetailsService(userService).passwordEncoder(bCryptPasswordEncoder()); 
    } 
}
```

[read it](https://ru.wikibooks.org/wiki/Spring\_Security/%D0%A2%D0%B5%D1%85%D0%BD%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9\_%D0%BE%D0%B1%D0%B7%D0%BE%D1%80\_Spring\_Security)

[implementation](https://habr.com/ru/post/278411/)

![Authentication - Hands-On Spring Security 5 for Reactive Applications](https://static.packt-cdn.com/products/9781788995979/graphics/3773a705-4a06-4b4d-80e3-c7af0b5dfc20.png)[1652 × 1106](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fsubscription.packtpub.com%2Fbook%2Fapplication\_development%2F9781788995979%2F2%2Fch02lvl1sec24%2Fauthentication\&psig=AOvVaw0rp2YvQJqst0wkF37dJb0U\&ust=1587755363054000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCJiM8uWf\_-gCFQAAAAAdAAAAABAD)
