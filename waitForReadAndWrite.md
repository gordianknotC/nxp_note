


## Wait for Read and Write

- ### in Ntag_I2C_Command / MinimalNtag_I2C_Command
- waitForI2CWrite
```java
	@Override  
	public void waitforI2Cwrite(int timeoutMS) throws IOException, FormatException, TimeoutException {  
		try {  
			Thread.sleep(timeoutMS);  
		} catch (InterruptedException e) {  
			e.printStackTrace();  
		}  
		return;  
	}
```

- waitForI2CRead
```java
	@Override  
	public void waitforI2Cread(int timeoutMS) throws IOException, FormatException, TimeoutException {  
		try {  
			Thread.sleep(timeoutMS);  
		} catch (InterruptedException e) {  
			e.printStackTrace();  
		}  
		return;  
	}
``` 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjgzNzU4ODldfQ==
-->