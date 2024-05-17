# CLEARTEXT communication to XXXX not permitted by network security policy
원인 : Android9 이상에서 http 접속시 오류 발생하도록 보안 정책이 변경됨
해결 :
1. https 를 사용한다
2.
xml를 만든다(위치는 res/xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
	<domain-config cleartextTrafficPermitted="true">
		<domain includeSubdomains="true">api.xxx.com</domain>
	</domain-config>
</network-security-config>
```
또는
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
	<base-config cleartextTrafficPermitted="true" />
</network-security-config>
```
그리고 Manifest에 이 xml파일을 config파일로 지정해준다
```xml
<application    ...    android:networkSecurityConfig="@xml/파일_이름">
```xml
3. Manifest Application에 선언
```xml
<application    ...    android:usesCleartextTraffic="true">
```