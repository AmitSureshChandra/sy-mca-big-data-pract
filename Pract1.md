Pract 1 : Installation, Configuration of Hadoop on windows 11 and Executing HDFS Commands

![image](https://github.com/user-attachments/assets/a8f4ee2f-9096-48ba-a7ef-9066dd84c6f3)

![image](https://github.com/user-attachments/assets/683b78b9-54e0-47ca-b4ee-0f2534ab6c68)

![image](https://github.com/user-attachments/assets/92885fe1-58d7-4e10-824f-b032682d357e)

![image](https://github.com/user-attachments/assets/e965f4a0-7a6e-4c76-8b01-fac476529989)

![image](https://github.com/user-attachments/assets/ec599f6d-4e00-4967-8bb7-5c4bfae9334b)

```
hdfs namenode -format
start-all.bat
```

![image](https://github.com/user-attachments/assets/079039d3-7d9b-4230-b85e-9e674921ffc9)


```
PS C:\Users\91823\Desktop\bda_practical\hadoop\sbin> hdfs dfs -ls /
Found 1 items
drwxr-xr-x   - 91823 supergroup          0 2024-08-19 08:23 /test

PS C:\Users\91823\Desktop\bda_practical\hadoop\sbin> hdfs dfs -mkdir /symca-amit

PS C:\Users\91823\Desktop\bda_practical\hadoop\sbin> hdfs dfs -ls /
Found 2 items
drwxr-xr-x   - 91823 supergroup          0 2024-08-19 09:03 /symca-amit
drwxr-xr-x   - 91823 supergroup          0 2024-08-19 08:23 /test

PS C:\Users\91823\Desktop\bda_practical\hadoop\sbin> hdfs dfs -touchz /symca-amit/demo.txt

PS C:\Users\91823\Desktop\bda_practical\hadoop\sbin> hdfs dfs -du /symca-amit
0  0  /symca-amit/demo.txt
0  0  /symca-amit/test.txt

PS C:\Users\91823> hdfs dfs -copyFromLocal log.txt /symca-amit

PS C:\Users\91823> hdfs dfs -du /symca-amit
0   0   /symca-amit/demo.txt
80  80  /symca-amit/log.txt
0   0   /symca-amit/test.txt

PS C:\Users\91823> hdfs dfs -cat /symca-amit/log.txt
doCheckPopCondition
current date2024-02-15
install date2024-02-15
triger event1



ref :
https://www.youtube.com/watch?v=knAS0w-jiUk
https://www.youtube.com/watch?v=7O56u3LyPTY
https://www.geeksforgeeks.org/hdfs-commands/
