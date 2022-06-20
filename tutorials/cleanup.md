---
title: MariaDB Operator cleanup 
description: Learn how to cleanup the MariaDB Operator
---


### Cleaning Up Operator



**Step 1: Delete the operator's Custom Resources by using `kubectl delete` commands as below.**


 ```execute 
 kubectl delete -f MariaDBserver.yaml -n my-mariadb-operator-app
 ```

 


**Step 2: Delete the operators by using `kubectl delete` command as below.**
 
 
  
 ```execute 
 kubectl delete -f https://operatorhub.io/install/mariadb-operator-app.yaml
 ```
 

**Step 3: Delete all the yaml files.**
 
 
 
 ```execute  
 rm -rf MariaDBserver.yaml 
```

### Conclusion

You have successfully cleaned up the MariaDB Operator resources.
