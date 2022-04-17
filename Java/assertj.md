# 2022-04-17
## Difference between isEqualTo vs. isSameAs
- isEqualTo : compare the contents (**equals** method)
- isSameAs : compare the addresses (**==** operator)

```java
String s1 = "abc";
String s2 = "abc";

assertThat(s1).isEqualTo(s2); // success
assertThat(s1).isSameAs(s2); // success

String s3 = new String("abc");
String s4 = new String("abc");

assertThat(s3).isEqualTo(s4); // success
assertThat(s3).isSameAs(s4); // fail

Object o1 = new Object();
Object o2 = new Object();

assertThat(o1).isEqualTo(o2); // fail
assertThat(o1).isSameAs(o2); // fail
```
***
