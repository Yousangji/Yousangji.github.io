---
title: "Error Handling"
date: 2019-01-15 18:26:28 -0400
layout: home
categories:
  - Spring
---

##Error Handling for REST with Spring

####Exception Handling by Spring Version
**Before Spring 3.2**  
the two main approaches to handling exceptions in a Spring MVC application were: HandlerExceptionResolver or the @ExceptionHandler annotation. Both of these have some clear downsides.

**Since 3.2**  
we’ve had the **@ControllerAdvice** annotation to address the limitations of the previous two solutions and to promote a unified exception handling throughout a whole application.

**Spring 5**  
introduces the ResponseStatusException class: A fast way for basic error handling in our REST APIs.

###Solutions

####1. Controller Level @ExceptionHandler
 : 단순히 @ExceptionHandler Annotation만으로 Controller 레벨에서 exception handling이 가능
 하지만 특정 controller에서만 작동하므로 global하게 적용할 수 없다는 단점이 명확하다. 
 
 ```java
public class FooController{
     
    //...
    @ExceptionHandler({ CustomException1.class, CustomException2.class })
    public void handleException() {
        //
    }
}
```

####2. HandlerExceptionResolver
장점 :  예외처리 메커니즘을 하나로 통일할 수 있음.

existing implementations
1. ExceptionHandlerExceptionResolver :  
상단의 @exceptionhandler가 작동하는데 필요한 핵심 컴포넌트  
2. DefaultHandlerExceptionResolver :  
HttpRequestMethodNotSupportedException, MethodArgumentNotValidException 과 같이 spring에서 기본으로 처리하는 exception으로 주로 400대 500 exception. body에 아무 내용도 포함하고 있지 않다는 한계가 있다.  [example]("https://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/mvc.html#mvc-ann-rest-spring-mvc-exceptions") : 
3. ResponseStatusExceptionResolver :  
@ResponseStatus를 사용해서 status code를 매핑해주는 역할. 따라서 상단의 defaultHandlerExceptionResolver와 마찬가지로 body에 아무런 내용을 포함하지 않는다는 한계가 있음.

4. Custom HandlerExceptionResolver  
아래 예시에서 확인할 수 있드시 ModelAndView를 return - body에 내용을 포함해줄 수 있

```java
@Component
public class RestResponseStatusExceptionResolver extends AbstractHandlerExceptionResolver {
 
    @Override
    protected ModelAndView doResolveException
      (HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        try {
            if (ex instanceof IllegalArgumentException) {
                return handleIllegalArgument((IllegalArgumentException) ex, response, handler);
            }
            ...
        } catch (Exception handlerException) {
            logger.warn("Handling of [" + ex.getClass().getName() + "] 
              resulted in Exception", handlerException);
        }
        return null;
    }
 
    private ModelAndView handleIllegalArgument
      (IllegalArgumentException ex, HttpServletResponse response) throws IOException {
        response.sendError(HttpServletResponse.SC_CONFLICT);
        String accept = request.getHeader(HttpHeaders.ACCEPT);
        ...
        return new ModelAndView();
    }
}
```
####3. @ControllerAdvice
 customResolver는 옛 MVC 방식인 ModelAndView를 사용해야한다는 한계가 있었는데 새 MVC 모델인 ResponseEntity 를 사용할 수 있는 exception handling 방법
 single, global error handling component
 세가지 면에서 장점
 - Full control over the body of the response as well as the status code
 - Mapping of several exceptions to the same method, to be handled together, and
 - It makes good use of the newer RESTful ResposeEntity response
 
 ```java
@ControllerAdvice
public class RestResponseEntityExceptionHandler 
  extends ResponseEntityExceptionHandler {
 
    @ExceptionHandler(value 
      = { IllegalArgumentException.class, IllegalStateException.class })
    protected ResponseEntity<Object> handleConflict(
      RuntimeException ex, WebRequest request) {
        String bodyOfResponse = "This should be application specific";
        return handleExceptionInternal(ex, bodyOfResponse, 
          new HttpHeaders(), HttpStatus.CONFLICT, request);
    }
}
```

####4. ResponseStatusException
장점
- Excellent for prototyping: We can implement a basic solution quite fast
- One type, multiple status codes: One exception type can lead to multiple different responses. This reduces tight coupling compared to the @ExceptionHandler
- We won’t have to create as many custom exception classes
- More control over exception handling since the exceptions can be created programmatically

단점
- There’s no unified way of exception handling: It’s more difficult to enforce some application-wide conventions, as opposed to @ControllerAdvice which provides a global approach
- Code duplication: We may find ourselves replicating code in multiple controllers

```java
@GetMapping("/actor/{id}")
public String getActorName(@PathVariable("id") int id) {
    try {
        return actorService.getActor(id);
    } catch (ActorNotFoundException ex) {
        throw new ResponseStatusException(
          HttpStatus.NOT_FOUND, "Actor Not Found", ex);
    }
}
```
we can implement a @ControllerAdvice globally, but also ResponseStatusExceptions locally.


####번외. Custom Error Message

````java
public class ApiError {
 
    private HttpStatus status;
    private String message;
    private List<String> errors;
 
    public ApiError(HttpStatus status, String message, List<String> errors) {
        super();
        this.status = status;
        this.message = message;
        this.errors = errors;
    }
 
    public ApiError(HttpStatus status, String message, String error) {
        super();
        this.status = status;
        this.message = message;
        errors = Arrays.asList(error);
    }
}
````