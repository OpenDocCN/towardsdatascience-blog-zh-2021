# SAS 到 Python —函数式与逐行翻译

> 原文：<https://towardsdatascience.com/sas-to-python-r-functional-vs-line-by-line-code-refactoring-4288e9f11eb0?source=collection_archive---------19----------------------->

## 数据帧结构和方法消除了在旧的过程语言中必要的迂回逻辑和额外的数据结构

![](img/afd83dbe90794f3a28ce3d9f4099a752.png)

[Unsplash.com](https://unsplash.com/photos/-3wygakaeQc)

在最近重构了使用医疗保险/医疗补助服务中心的公共数据计算医院评级的代码后(原始代码和数据可以在[https://quality net . CMS . gov/inhibitory/public-reporting/overall-ratings/SAS](https://qualitynet.cms.gov/inpatient/public-reporting/overall-ratings/sas)找到)，我想我可以在这里获得我的关键见解。

从这种转换中可以观察到，数据帧的力量已经完全改变了可读性和交付速度的标准。由于 SAS 是从早期的过程语言世界演变而来的，早期的过程语言世界包括 FORTRAN 和 COBOL，具有统一数据类型和元素式操作的数组类型数据结构，抽象级别可通过异构数据数组来实现，这些数据数组逐个元素地循环，以执行通常需要并行数组进行后续处理的算术或比较。

这里有一个例子。在下面的 SAS 代码中，目标是识别具有少于 100 个观察值的评级类别，并从数据集中删除这些列:

```
PROC SQL;
select measure_in_name into: measure_in separated by '' notrim
from include_measure0;
QUIT;
%put &measure_in;/* &measure_cnt: number of included measure */
PROC SQL;
select count(measure_in_name)
into: measure_cnt
from include_measure0;
QUIT;
%put &measure_cnt;/*COUNT # HOSPITALS PER MEASURE FOR ALL MEASURES*/
PROC TABULATE DATA=All_data_&year.&quarter out=measure_volume; 
var &measure_all;table n*(&measure_all);
RUN;PROC TRANSPOSE data=Measure_volume out=measure_volume_t;
RUN;/* IDENTIFY MEASURES WITH VOLUMN <=100 */
DATA less100_measure  (drop=_NAME_ _LABEL_ rename = (COL1=freq)); 
SET measure_volume_t (where = (col1<=100));if _name_ ^= '_PAGE_'  and _name_^='_TABLE_';
measure_name = tranwrd(_NAME_, '_N', '');RUN;
DATA R.less100_measure;SET less100_measure;run;*OP-2;/* CREATE a measure list for count<=100 */
PROC SQL;
select measure_Name
into: measure_exclude separated by '' notrim
from Less100_measure;
QUIT;/* REMOVE MEASURES WHICH HAVE HOSPITAL COUNTS <=100*/
DATA initial_data_&year.&quarter;
SET All_data_&year.&quarter;/* measure volume <=100*/
drop &measure_exclude ;
RUN;
```

代码首先创建一个结构来保存列名，然后创建一个结构来填充每个指定列中的观察计数。接下来，它确定哪些列的观察值少于 100 个，并创建一个结构来保存这些列名，最后，使用这些名称将它们从主数据数组中删除。

下面是使用相同初始数据的 pandas 数据框架的等效 python:

```
# grab all columns with hospital counts>100 
dfObs100=df[df.columns.intersection(df.columns[df.notna().sum()>=100])]
```

没有创建显式的数据结构，也没有执行基于元素的计算循环。一切都发生在 pandas 函数内部，这意味着您可以获得由包开发人员创建的用于循环行和列的最先进的内部方法的好处。

另一个例子是程序包中程序 1 的代码，它由一个 SAS 程序文件和另一个文件中的宏组成，用于计算标准化的组分数。以下是程序 1 的 SAS 代码:

```
******************************************
* Outcomes - Mortality       *
******************************************;
*option mprint;
/* count number of measures in Outcome Mortality Group */
PROC SQL;
select count(measure_in_name)
into: measure_OM_cnt /*number of measures in this domain*/
from Outcomes_mortality;/*Outcomes_mortality is generated from the SAS program '0 - Data and Measure Standardization_2021Apr'*/
QUIT;/*OM is used to define the data name for mortality Group; 
&measure_OM is the measures in Mortality Group;
&measure_OM_cnt is the number of measures in this Group;*/
/*output group score in R.Outcome_mortality*/
%grp_score(&MEASURE_ANALYSIS, OM, &measure_OM, &measure_OM_cnt,R.Outcome_mortality);***************************************
* Outcomes - Safety of Care     *
***************************************;/* count number of measures in Outcome Safety Group */
PROC SQL;
select count(measure_in_name)
into: measure_OS_cnt
from Outcomes_safety;/*Outcomes_safety is generated from the SAS program '0 - Data and Measure Standardization_2021Apr'*/
QUIT;/*OS is used to define the data name for Safety Group; 
&measure_OS is the measures in Safety Group;
&measure_OS_cnt is the number of measures in Safety Group;*/
/*output group score in R.Outcome_safety */
%grp_score(&MEASURE_ANALYSIS, OS, &measure_OS,  &measure_OS_cnt, R.Outcome_safety);********************************************
* Outcomes - Readmission        *
********************************************;/* count number of measures in Outcome Readmission Group */
PROC SQL;
select count(measure_in_name)
into: measure_OR_cnt
from Outcomes_readmission;/*Outcomes_readmission is generated from the SAS program '0 - Data and Measure Standardization_2021Apr'*/
QUIT;/*OR is used to define the data name for Readmission Group; 
&measure_OR is the measures in Readmission Group;
&measure_OR_cnt is the number of measures in Readmission Group;*/
/*output group score in R.Outcome_readmission*/
%grp_score(&MEASURE_ANALYSIS, OR, &measure_OR, &measure_OR_cnt, R.Outcome_readmission);************;******************************************
*  Patient Experience        *
******************************************;/* count number of measures in Patient Experience Group */
PROC SQL;
select count(measure_in_name)
into: measure_PtExp_cnt
from Ptexp;/*Ptexp is generated from the SAS program '0 - Data and Measure Standardization_2021Apr'*/
QUIT;/*PtExp is used to define the data name for Patient Experience Group; 
&measure_PtExp is the measures in Patient Experience Group;
&measure_PtExp_cnt is the number of measures in Patient Experience Group;*/
/*output group score in R.PtExp*/
%grp_score(&MEASURE_ANALYSIS, PtExp, &measure_PtExp,  &measure_PtExp_cnt,R.PtExp);**********************************************
* Timely and Effective Care                  *
**********************************************;/* count number of measures in Timely and Effective Care */
PROC SQL;
select count(measure_in_name)
into: measure_Process_cnt
from Process;/*Process is generated from the SAS program '0 - Data and Measure Standardization_2021Apr'*/
QUIT;
```

下面是程序 1 中使用的附带宏:

```
**********************************************************
* macro for calcuating group score for each measure group*
**********************************************************;%macro grp_score(indsn, gp, varlist,  nmeasure, Out_avg);
  data dat0 (keep=provider_id &varlist.  c1-c&nmeasure. total_cnt measure_wt avg ); 
  set &indsn.;array M(1:&nmeasure.) &varlist.;
  array C (1:&nmeasure.) C1-C&nmeasure.;DO k =1 TO &nmeasure.;
  if m[k] ^=. then C[k]=1;
     else C[k]=0;
  END;
  total_cnt=sum(of c1-c&nmeasure.);

  if total_cnt>0 then do;
  measure_wt=1/total_cnt;
  avg=sum(of &varlist.)*measure_wt;
  end;
  run;

  *standardization of group score;
  PROC STANDARD data=dat0 mean=0 std=1 out=dat1;var avg;run;*add mean and stddev into the data;
  ods output summary=new(drop=variable);
  proc means data=dat0 stackodsoutput mean std ;
   var avg;
  run;proc sql; 
    create table dat2 as
    select  *
    from dat0, new;
  quit;data &out_avg;merge dat2(rename=avg=score_before_std) dat1(keep=provider_ID avg rename=avg=grp_score);by provider_ID;run;
%mend;
```

下面是我用 python 编写的等价程序 1 代码:

```
# mortality scores
mortality=getScores(dfAllStd,df,mortalityFields)
# safety scores
safety=getScores(dfAllStd,df,safetyFields)
# readmission scores
readmit=getScores(dfAllStd,df,readmitFields)
# patient care scores
patient=getScores(dfAllStd,df,pxFields)
# process scores
process=getScores(dfAllStd,df,processFields)
```

使用支持 python 的函数 getScores()，大致相当于 SAS 宏:

```
def getScores(dfStd,dfAll,fields):
  # get fields counts and weights
  cnt=dfStd[dfStd.columns.intersection(fields)].count(axis=1,numeric_only=True)
  wt=(1/cnt).replace(np.inf,np.nan)#get raw scores
  scoreB4=dfStd[dfAll.columns.intersection(fields)].mean(axis=1)
  # standardize scores
  sMean=np.nanmean(scoreB4)
  sStddev=np.nanstd(scoreB4)
  score=(scoreB4-sMean)/sStddev
  #generate table of values
  table5=pd.DataFrame({'id':df['PROVIDER_ID'],'count':dfAllStd[dfAllStd.columns.intersection(fields)].count(axis=1,numeric_only=True),
                     'measure_wt':wt,'score_before':scoreB4,'score_std':score}) 
  return table5
```

正如您再次看到的，python 代码更小，并且使用隐式熊猫循环更容易阅读。

通过在 R 中使用数据帧可以获得类似的紧凑结果。事实上，隐式“应用”函数循环对于 R 计算性能至关重要。

另一个教训是，由于自动化重构工具只会逐行重建原始 SAS 代码的结构，所以典型的翻译人员只会复制旧结构及其原始设计问题。对于输入和输出的数据结构不太可能改变的操作，这可能是有意义的，但是，随着数据可用性的增长，很可能会越来越需要和可能输入更多的并行字段。具有新输出的新模型将更加常见，这使得代码很可能需要适应来自新建模技术的新输出。

如果语言转换的目的是使代码现代化并适应新的数据通道和建模能力，那么自动化代码重构将是一个障碍。因此，在开始重构旧的数据科学代码之前，重要的是确定您想要利用代码的哪些部分，并计划使用目标语言中可用的灵活功能，然后使用旧代码作为参考的功能规范进行重构。

总之，主要的收获是

*   逐行转换锁定了旧的程序结构，与新的语言功能相比，这可能是次优的
*   数据帧加上对其进行操作的函数可以创建更好、可读性更强的代码，从而更易于维护
*   进行逐行翻译的简单翻译器可能能够访问新的方法和模型，但是将关键的程序领域锁定在低级的数据和逻辑范例中
*   在 pandas 和其他包中使用嵌入式函数为列和行操作上的循环提供了一流的性能

最后，对于使用相似数据结构和循环的 FORTRAN 和 COBOL 的翻译，也存在同样的问题。现代语言的真正力量在于数据框架和功能，释放它需要超越简单机器翻译的非线性连接。