# 目录

* [一. 读入excel](#一--读入excel)  
* [二. 修改列名](#二--修改列名)   
* [三. 创建计算列](#三--创建计算列)    
* [四. 列选择与行的条件选择](#四--列选择与行的条件选择)  
* [五. 排序](#五--排序)  
* [六. 分组统计](#六--分组统计)   
    * [1. 求每个Treatment的Yeaple均值和Schiff最大值](#1--求每个Treatment的Yeaple均值和Schiff最大值)   
    * [2. 求每个水平组合所含观测数](#2--求每个水平组合所含观测数)  
    * [3. 分组去重](#3--分组去重)  
* [七. 分组统计后的条件选择](#七--分组统计后的条件选择) 

 
    
&ensp;&ensp;&ensp;&ensp;  


# 一  读入excel  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;原始excel示例数据如下：originT.xlsx    
&ensp;&ensp;&ensp;&ensp;  
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/originT.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
 PROC IMPORT DATAFILE = "F:\Clinical trials\SASSQLPANDAS\originT.xlsx"   /*导入excel时，excel文件应处于关闭状态，否则报错*/
             OUT = mylib.desen
             DBMS = xlsx
             REPLACE;
             RANGE = "Sheet1$A1:H61"; 
             GETNAMES = YES ;
    RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.desen数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SASdesen.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
Desen = pd.read_excel(r"F:\Clinical trials\SASSQLPANDAS\originT.xlsx",sheet_name="Sheet1");
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：desen数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDASdesen.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
# 二  修改列名   
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
DATA mylib.DesenC ;
    SET mylib.Desen;
    RENAME Name=Patient ;
RUN;
PROC PRINT data=mylib.DesenC ;
RUN; 
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS2.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenC AS   
    SELECT ID, Name AS Patient, Age, Gender, Treatment, Phase, Yeaple, Schiff
    FROM mylib.Desen;
QUIT;
PROC PRINT data=mylib.DesenC ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL2.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
DesenC = Desen.rename(columns={"Name":"Patient"})
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：DesenC  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS2.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
# 三  创建计算列   
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
DATA mylib.DesenC ;
    SET mylib.Desen;
    EValue = Yeaple/30+Schiff;
RUN;
PROC PRINT data=mylib.DesenC ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS3.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenC AS   
    SELECT ID, Name, Age, Gender, Treatment, Phase, Yeaple, Schiff, (Yeaple/30+Schiff) AS EValue
    FROM mylib.Desen;
QUIT;
PROC PRINT data=mylib.DesenC ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL3.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
Desen["EValue"] = Desen["Yeaple"]/30 + Desen["Schiff"]
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：Desen  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS3.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;    
# 四  列选择与行的条件选择   
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
DATA mylib.DesenC ;
    SET mylib.Desen;
    IF (20<=Age<=30) AND Gender="F" ; /*IF或WHERE实现行的条件选择*/
    KEEP ID Name Age Gender ;         /*KEEP,DROP及数据集选项实现列选择*/
RUN;
PROC PRINT data=mylib.DesenC ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS4.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenC AS   
    SELECT ID, Name, Age, Gender   /*SELECT实现对列的选择*/
    FROM mylib.Desen
    WHERE (20<=Age<=30)AND(Gender="F") ;   /*WHERE实现对行的条件选择*/
QUIT;
PROC PRINT data=mylib.DesenC ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenC数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL4.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```

DesenC= Desen.loc[(20<=Desen["Age"])&(Desen["Gender"]=="F"), ("ID", "Name", "Age", "Gender")].loc[Desen["Age"]<=30, :]
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：DesenC  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS3.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
# 五  排序   
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SORT data=mylib.Desen out=mylib.DesenS ;
    BY Gender DESCENDING Age ;
RUN;
PROC PRINT data=mylib.DesenS ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenS数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS5.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenS AS 
    SELECT * 
    FROM mylib.Desen
    ORDER BY Gender, Age DESC ;
QUIT;
PROC PRINT data=mylib.DesenS ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenS数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL5.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
# ascending设置by里的依赖变量是升序还是降序

DesenS = Desen.sort_values(by=["Gender", "Age"], axis=0, ascending=[True, False]) 
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：DesenS  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS5.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
# 六  分组统计   
&ensp;&ensp;&ensp;&ensp;  
## 1  求每个Treatment的Yeaple均值和Schiff最大值  
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SORT data=mylib.Desen out=temp;
    BY Treatment;
RUN;
PROC MEANS data=temp maxdec=2 ;  /*MEANS过程有两套计算系统*/
    CLASS Treatment;
    VAR Yeaple Schiff;  
    OUTPUT OUT=mylib.DesenG Mean(Yeaple)=Yeaple_Mean Max(Schiff)=Schiff_Max ;
RUN;
PROC PRINT data=mylib.DesenG ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：MEANS过程输出 
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS61.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenG数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS62.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenG AS
    SELECT Treatment,avg(Yeaple) AS Yeaple_Mean, max(Schiff) AS Schiff_Max  
    FROM mylib.Desen;
QUIT;
PROC PRINT data=mylib.DesenG ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenG数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL6.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
DesenG = Desen.groupby(by=["Treatment"]).aggregate({"Yeaple":np.mean, "Schiff":np.max})
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：DesenG  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS61.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
其实，dataframe的describe()方法 可以获得所有数值型列的分组描述性信息（count, mean, std, min, 25%, 50%, 75%, max）如下：  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS62.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;  
## 2  求每个水平组合所含观测数  
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SORT data=mylib.Desen out=temp;
    BY Gender Treatment; 
RUN;
PROC MEANS data=temp n;
    CLASS Gender Treatment; 
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：MEANS过程输出 
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS63.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    SELECT Gender, Treatment, count(*) AS count
    FROM mylib.Desen
GROUP BY Gender, Treatment;
QUIT;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL63.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
Desen.groupby(by=["Gender","Treatment"]).count()  
# 或者
Desen.groupby(by=["Gender","Treatment"]).size()  
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS63.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
## 3  分组去重  
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SORT data=mylib.Desen out=mylib.D nodupkey;
    BY Gender Treatment;
RUN;
PROC PRINT data=mylib.D ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.D ;
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS64.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    SELECT Gender, Treatment, count(*) AS count
    FROM mylib.Desen
GROUP BY Gender, Treatment; /*日志提示GROUP BY转化为ORDER BY，因为未使用汇总函数*/
QUIT;  
/*也可以使用*/
PROC SQL;
    SELECT DISTINCT CATS(Gender, Treatment) AS combination, Gender, Treatment
    FROM mylib.Desen
QUIT;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL64.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
Desen.groupby(by=["Gender","Treatment"]).first()  
# 或者
Desen.groupby(by=["Gender","Treatment"]).size()  
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS63.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  

# 七  分组统计后的条件选择   
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp;  
&ensp;&ensp;&ensp;&ensp; SQL里，SELECT—FROM—GROUP BY—HAVING 实现的是：先对表进行分组统计，得到结果，然后从这个结果里，用HAVING指定的条件选择行。 此过程在PYTHON里可以分两步实现，首先是groupby分组统计，得到统计结果表，把此表赋给一个df变量，然后对df表再进行.loc条件选取。而SAS里，也可以先PROC MEANS OUTPUT OUT=得到统计结果数据集，然后再对它进行DATA步—IF条件选取，即分两步进行。  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;  
* **SAS**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SORT data=mylib.Desen out = temp;
    BY Name;
RUN;
PROC MEANS data=temp;
    CLASS Name;
    OUTPUT OUT=mylib.DesenC MEAN(Yeaple)=Yeaple_Mean ;
RUN;
DATA mylib.DesenS ;
    SET mylib.DesenC;
    IF Yeaple_Mean >30 ;
RUN;
PROC PRINT data=mylib.DesenS ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenS数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SAS7.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **SQL**：  
&ensp;&ensp;&ensp;&ensp;    
```
PROC SQL;
    CREATE TABLE mylib.DesenS AS
    SELECT Name,avg(Yeaple) AS Yeaple_Mean 
    FROM mylib.Desen
    GROUP BY Name
    HAVING Yeaple_Mean>30 ;
QUIT;
PROC PRINT data=mylib.DesenS ;
RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.DesenS数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SQL7.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
DesenS = Desen.groupby(by=["Name"]).aggregate({"Yeaple":np.mean})
DesenS = DesenG.loc[DesenG["Yeaple"]>30,:]
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：DesenS  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDAS7.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;   
