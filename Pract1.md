Pract 1 : Installation, Configuration of Hadoop on windows 11 and Executing HDFS Commands

Download hadoop from https://hadoop.apache.org/releases.html

![image](https://github.com/user-attachments/assets/89eca352-eadd-4897-b8b5-4ab3aedd2132)

![image](https://github.com/user-attachments/assets/a8f4ee2f-9096-48ba-a7ef-9066dd84c6f3)

```xml
<configuration>
  <configuration>
    <property>
      <name>fs.defaultFS</name>
      <value>hdfs://localhost:9000</value>
    </property>
  </configuration>
</configuration>
```

![image](https://github.com/user-attachments/assets/683b78b9-54e0-47ca-b4ee-0f2534ab6c68)

```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

![image](https://github.com/user-attachments/assets/92885fe1-58d7-4e10-824f-b032682d357e)

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/C:/Users/91823/Desktop/bda_practical/hadoop/data/namenode</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/C:/Users/91823/Desktop/bda_practical/hadoop/data/datanode</value>
  </property>

</configuration>
```

![image](https://github.com/user-attachments/assets/e965f4a0-7a6e-4c76-8b01-fac476529989)

```bash
set JAVA_HOME="C:\Progra~1\Java\jdk-1.8"
```

![image](https://github.com/user-attachments/assets/ec599f6d-4e00-4967-8bb7-5c4bfae9334b)

```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
</configuration>
```

replace bin from https://drive.usercontent.google.com/download?id=1nCN_jK7EJF2DmPUUxgOggnvJ6k6tksYz&export=download&authuser=0

```bash
hdfs namenode -format
start-all.bat
```

![image](https://github.com/user-attachments/assets/079039d3-7d9b-4230-b85e-9e674921ffc9)


```cmd
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
```


#### References
- https://www.youtube.com/watch?v=knAS0w-jiUk
- https://www.youtube.com/watch?v=7O56u3LyPTY
- https://www.geeksforgeeks.org/hdfs-commands/
