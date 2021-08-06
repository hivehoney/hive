---
layout: post
title: Spring Security - @AuthenticationPrincipal
subtitle: 로그인한 사용자에 대한 정보 참조
categories: Spring
tags: [Spring]
---


최근 사이드 프로젝트를 진행하면서 로그인을 한 사용자에 정보를 어떻게 가지고 있을까 라는 고민을 가지게 되었다.
현재 내가 사용하고 있는 프레임워크는 SpringBoot 이며, Security 인증 후에 사용자의 정보를 가져오는 대표적인 방법을 소개하겠다.


### 사용자 체크 방법
* Bean에서 사용자 정보 가져오기
* Controller에서 사용자 정보 가져오기
* @AuthenticationPrincipal

### Bean

```java
Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal(); 
UserDetails userDetails = (UserDetails)principal; 
String username = principal.getUsername(); 
String password = principal.getPassword();
```
- SecurityContextHolder을 통해 가져오는 방법이다.

### Controller
```java
@Controller 
public class SecurityController { 
    @GetMapping("/username") 
    @ResponseBody 
    public String currentUserName(Principal principal) { 
        return principal.getName(); 
    } 
}
```

- Principal 객체에 접근해 정보를 가져온다.

```java
public int addActivity(@RequestParam("name") String name,Activity activity,Hash hash,Authentication authentication) {
    //현재 로그인한 유저의 정보를 받아옵니다.
    UserDetails userDetails = (UserDetails) authentication.getPrincipal();
    Member m = memRepo.findOneByMid(userDetails.getUsername());
    List<Activity> activityList = m.getActivities();
```

- Authentication을 통해서 현재 로그인한 사용자의 id를 통해 DB에서 사용자 정보를 가져온다.

### @AuthenticationPrincipal
```java
@Controller 
public class SecurityController { 
    @GetMapping("/username") 
    @ResponseBody 
    public String currentUserName(AuthenticationPrincipal User user) { 
        return user.getName(); 
    } 
}
```

- 이 애노테이션을 사용하면 UserDetailsService가 반환하는 User객체를 인수로 받아서 사용할 수 있다고 한다.
  @AuthenticationPrincipal은 파라미터 레벨에서 사용한다.
