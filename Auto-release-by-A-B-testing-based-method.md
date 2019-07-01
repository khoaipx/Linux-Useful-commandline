We are SaaS (Software as a service) and work with http requests.

It's very important to divide requests into small part and release new features on each piece. So we design a A/B testing to speed up the release process for developers.

# Design

First, we need to split requests to small parts, then allow multiple code versions to run on those pieces. Many web servers/proxies can do this work like nginx, apache... or some open source balancer platform like [Netflix's eureka](https://github.com/Netflix/eureka)

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/03/abtest_design.jpg)

For each code version, if its features increase production's quality (we might need a measurement method to check the quality), it will be merged automatically to production code version.

# Automatic code release

In fact, we need 2 more modules to perform an automatically code release. They're service's performance and quality test, they need to be automatic, too. In this post we don't go further on them, suppose they existed.

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/03/code_release_flow.jpg)

# Code release automatically, step by step

## 0. Pick code version in commits queue.
 - If OK: go to 1.
 - if FAIL: go to 0.

## 1. Performance test: do a test of performance
 - if OK: go to 2.
 - if FAIL: remove code version, go to 0.

## 2. Test quality 1%

### a. Request test tray
 - if OK: go to 2.b.
 - if FAIL: go to 2.a.

### b. Release code in tray 1% to test quality
 - if OK: go to 3.
 - if FAIL: remove code version, go to 0.

## 3. Test quality 5%

### a. Request test tray
 - if OK: go to 3.b.
 - if FAIL: go to 3.a.

### b. Release code in tray 5% to test quality
 - if OK: go to 4.
 - if FAIL: remove code version, go to 0.

## 4. Release in master branch to see real quality

### a. Request master tray
 - if OK: go to 4.b.
 - if FAIL: go to 4.a.

### b. Release code in master tray
 - if OK: go to 5.
 - if FAIL: remove code version, go to 0.

## 5. Get bonus/awards: check if your feature increases at least 1% of quality
 - if OK: get bonus or proposal ($_$)
 - if FAIL: get praise from team (^_^)

# It's semi-automatic in fact

Yeah, code release stream above is just in theory. In fact, we need to monitor and have some double check each time a code version goes to production.

But at least, this model could be useful if we don't want to spend time too much on code review.

Have great time coding. ;)