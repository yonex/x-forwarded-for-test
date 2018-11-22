# x-forwarded-for-test
X-Forwarded-For と X-Real-IP のテストです

```sh
docker-compose up
```

wgetter -> nginx.A -> nginx.B -> app
```
app_1      | /before 10.1.1.101 "10.1.1.99, 10.1.1.100" 10.1.1.100 10.1.1.101
nginx.B_1  | /before 10.1.1.100 "10.1.1.99" - 10.1.1.100
nginx.A_1  | /before 10.1.1.99 "-" - 10.1.1.99
```

wgetter -> nginx.A -> nginx.B (set real_ip) -> app
```
app_1      | /after 10.1.1.101 "10.1.1.99, 10.1.1.99" 10.1.1.99 10.1.1.101
nginx.B_1  | /after 10.1.1.99 "10.1.1.99" - 10.1.1.100
nginx.A_1  | /after 10.1.1.99 "-" - 10.1.1.99
```

wgetter -> nginx.B -> app
```
app_1      | /before 10.1.1.101 "10.1.1.99" 10.1.1.99 10.1.1.101
nginx.B_1  | /before 10.1.1.99 "-" - 10.1.1.99
```

wgetter -> nginx.B (set real_ip) -> app
```
app_1      | /after 10.1.1.101 "10.1.1.99" 10.1.1.99 10.1.1.101
nginx.B_1  | /after 10.1.1.99 "-" - 10.1.1.99
```
