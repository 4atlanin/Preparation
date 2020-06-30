# Spring MVC
Servlet - интерфейс расширяющий функциональность сервера. Принимает, процессит реквесты и возвращает респонсы. 
#### Request life cycle
![alt text](https://justforchangesake.files.wordpress.com/2014/05/spring-request-lifecycle.jpg)
 1. запрос от клиента сперва проходит через фильтры, `javax.servlet.Filter`. Пример - `CorsFilter`, или можно свои запилить.
 2. Попадает в `DispatcherServlet`:
    - в реквест аттачится `WebApplicationContext`, `LocalResolver`, `ThemeResolver`.
    - если реквест мультипарт, то он заворачивается в `MultipartHttpServletRequest`. 
    - ищется подходящий хэндлер, `DefaultAnnotationHandlerMapping` который маппит аннотацию `@RequestMapping` на реквест.
    ```
    Following are the different mapping types supported.
    
    By path
    @RequestMapping(“path”)
    
    By HTTP method
    @RequestMapping(“path”, method=RequestMethod.GET). Other Http methods such as POST, PUT, DELETE, OPTIONS, and TRACE are also supported.
    
    By query parameter
    @RequestMapping(“path”, method=RequestMethod.GET, params=”param1”)
    
    By the presence of request header
    @RequestMapping(“path”, header=”content-type=text/*”)
    ```
    
    - `HandlerInterceptor.preHandle` если есть в конфигурации(добавляются через `WebMvcConfigurer`)
    - Попали в контроллер, сделали все дела.
    - Если ошибка то попадаем в **Handler exception resolver** , по дефолту используется `DefaultHandlerExceptionResolver`.  
    Можно сделать свой и пометить метод, принимающий `Throwable`, аннотацией `@ExceptionHandler`.
    - Возвращаемся через `HandlerInterceptor.postHandle` если есть в конфигурации
3. Попадает в `DispatcherServlet` )
4. Если нет аннотации `@ResponseBody`, то ViewResolver мапит имя view на конкретную имплементацию, например **FreeMarkerViewResolver**. Если есть `@ResponseBody` то возвращается респонс как есть.
 
