
## putExtras


```java
intent.putExtras(MainActivity.getmIntent());
```
-------------------------

```java
public void showAuthDialog() {  
   Intent intent = null;  
  intent = new Intent(this, AuthActivity.class);  
  intent.putExtras(MainActivity.getNfcIntent());  
  startActivityForResult(intent, MainActivity.AUTH_REQUEST);  
}
```
-------------------------
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjk1NjI3MTQxXX0=
-->