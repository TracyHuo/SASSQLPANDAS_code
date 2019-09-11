# 目录

* [一. 读入excel](#一--读入excel)  
* [二. 修改列名](#二--修改列名)   
* [三. 宏ProGIBIPI对单个GIBIPI数据集进行处理](#三--宏ProGIBIPI对单个GIBIPI数据集进行处理)    
* [四. 将GIBIPI结果综合至一张表](#四--将GIBIPI结果综合至一张表)  
 
    
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
             OUT = mylib.originT
             DBMS = xlsx
             REPLACE;
             RANGE = "Sheet1$A1:H61"; 
             GETNAMES = YES ;
    RUN;
```
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：mylib.originT数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/SASoriginT.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
* **Python Pandas**：  
```
df = pd.read_excel(r"F:\Clinical trials\SASSQLPANDAS\originT.xlsx",sheet_name="Sheet1");
```  
&ensp;&ensp;&ensp;&ensp;    
&ensp;&ensp;&ensp;&ensp;结果：df数据集  
&ensp;&ensp;&ensp;&ensp;    
![image](https://github.com/TracyHuo/SASSQLPANDAS_code/blob/master/Image/PANDASoriginT.PNG);  
&ensp;&ensp;&ensp;&ensp;   
&ensp;&ensp;&ensp;&ensp;  
# 二  修改列名   
&ensp;&ensp;&ensp;&ensp;    
* **代码**：  
&ensp;&ensp;&ensp;&ensp;    
