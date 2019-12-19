Information Security
-------------------

## Principle of Least Privilege 에 대한 단상

최소한의 권한만을 부여하자는 의미인데, 정보보안 관점에서 중요도가 낮은 서비스 인 경우에는 운영비용과 보안성과의 Trade-off 관계가 성립된다.
보안 리스크는 보안사고에 일어날 경우, 발생하는 피해비용을 고려해야 할 텐데, 이것이 미미하다면 `Principle of Least Privilege` 에 있어 좀 더 유연하게 접근해도 되지 않을까?   

## SOC
System and Organization Controls

Case) Requirements related to SOC
```
k8s access log must be enabled and integrate with SOC.
- audit log must contains: user, id, timestamp,operaiton(eg, login), object
```

  

## References
- [Audit Log Best Practices For Information Security](https://reciprocitylabs.com/audit-log-best-practices-for-information-security/)

