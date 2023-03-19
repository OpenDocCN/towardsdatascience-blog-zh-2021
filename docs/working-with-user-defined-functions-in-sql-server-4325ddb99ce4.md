# 在 SQL Server 中使用用户定义的函数

> 原文：<https://towardsdatascience.com/working-with-user-defined-functions-in-sql-server-4325ddb99ce4?source=collection_archive---------36----------------------->

## 在本教程中，我们将讨论 SQL Server 中的用户定义函数。更具体地说，我们将讨论标量函数和表值函数。

![](img/804967fcc41d241d166e246515b10151.png)

扬·安东宁·科拉尔在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

写代码的时候，一定要以遵循 DRY 原则为目标(不要重复自己)。避免代码重复的一种方法是将代码块放在函数中，并在需要时调用它们。

SQL 中函数的概念类似于 Python 等其他编程语言。主要区别在于它们的实现方式。根据返回的数据，SQL 中有两种主要类型的用户定义函数:

1.  标量函数:这些类型的函数返回单个值，例如 float、int、varchar、DateTime 等。
2.  表值函数:这些函数返回表。

# 目录

*   先决条件。
*   创建函数。
*   在语句中使用函数。
*   更新/删除功能。
*   在函数中使用变量和条件语句。
*   结论。

# 先决条件

*   对 SQL 的基本理解。
*   带有数据库的 SQL Server。
*   [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)连接到您的数据库。

# 创建函数

## 标量函数

下面是一个简单函数的定义。它接受两个数字并返回它们的和。因为这个函数返回一个数字，所以它是一个标量函数。

```
CREATE FUNCTION scalar_func
    (
       @a AS INT, -- parameter a
       @b AS INT  -- parameter b
    )
    RETURNS INT   -- return type
    AS
    BEGIN
       RETURN @a + @b -- return statement
    END;
```

*   我们使用`Create function`命令来定义函数。它后面是函数的名称。在上面的例子中，函数的名字是`scalar_func`。
*   我们需要以下面的格式声明函数的参数。

`@VariableName AS Data Type`

在上面的例子中，我们定义了两个整数参数`a`和`b`。

*   结果的返回类型必须在参数的定义下面提到。在上面的例子中，我们返回的总和是一个整数。
*   在返回语句之后，我们创建一个包含函数逻辑的`BEGIN ... END`块。虽然在这种情况下，我们有一个返回语句，但我们不需要一个`BEGIN ... END`块。

## 表值函数

在创建表值函数之前，我们将创建一个简单的表。

```
-- Creating new table
    CREATE TABLE TEST(
      num1 INT,
      num2 INT
    );

-- Inserting values into new table
    INSERT INTO TEST
    VALUES
    (1,2),
    (2,3),
    (4,5);
```

该表包含两列。我们将创建一个函数，返回一个带有额外列的新表。这个额外的列将包含列`num1`和列`num2`中的数字之和。

```
CREATE FUNCTION table_valued_func()
    RETURNS TABLE
    AS
    RETURN
       -- statement to calculate sum
       SELECT num1 , num2, num1 + num2 AS 'SUM'
       FROM TEST;
```

*   上面的函数不接受任何参数。
*   SQL 语句只是计算总和，并将其存储在一个名为`SUM`的新列中。

# 在语句中使用函数

## 标量函数

```
-- invoking previously created scalar function
    SELECT dbo.scalar_func(1,2);
```

当在语句中使用函数时，我们需要在函数前面加上与之相关的数据库模式。Microsoft SQL Server 中的默认模式是`dbo`。如果没有提到数据库模式，SQL 将给出一个错误，

## 表值函数

由于该函数返回一个表，我们需要选择我们感兴趣的列。

```
-- invoking previously created table valued function
    SELECT * FROM dbo.table_valued_func();
```

像标量函数一样，我们需要提到数据库模式。

# 更新/删除功能

更新/删除标量函数和表值函数的语法是相同的。

## 更新

我们将更新我们的表值函数，在现有的 sum 上加 10，并将列名改为`New_Sum`。

```
ALTER FUNCTION table_valued_func()
    RETURNS TABLE
    AS
    RETURN
      -- updating statement to add 10 to sum
      SELECT num1 , num2, num1 + num2 + 10 AS 'NEW_SUM'
      FROM TEST;
```

`Alter`关键字用于更新功能。

## 滴

```
-- dropping previously created scalar function
    DROP FUNCTION dbo.scalar_func;-- dropping previously created tabular function
    DROP FUNCTION dbo.table_valued_func;
```

> *注意:不要在函数名后面加括号。*

# 在函数中使用变量和条件语句

## 变量

下面是声明和初始化变量的语法。

```
-- declaring integer variable
    DECLARE @result AS INT;-- initializing created varaible
    SET @result = @a + @b;
```

`DECLARE`关键字用于创建变量，而`SET`关键字用于初始化变量。

下面是一个使用变量的标量函数的例子。

```
CREATE FUNCTION scalar_func
    (
	 @a AS INT,
	 @b AS INT
    )
    RETURNS INT
    AS
    BEGIN
      -- using variables inside function
	 DECLARE @result AS INT
	 SET @result = @a + @b
	 RETURN @a + @b
    END;
```

## IF…ELSE 语句

`IF...ELSE`语句的语法类似于 Python 或 C++中的`IF...ELSE`语句。

```
DECLARE @num AS INT;
    SET @num = 4;
    IF @num % 2 = 0 

	BEGIN
	  SELECT 'Number is Even'
	END ELSE

        BEGIN
	  SELECT 'Number is Odd'
	END
```

上面这段代码检查变量`num`中的值是偶数还是奇数。基于该值，执行`IF`或`ELSE`程序块。

下面列出了使用`IF...ELSE`块的功能。

```
CREATE FUNCTION is_even(@num AS INT)
    RETURNS BIT
    AS
    BEGIN
      DECLARE @result AS BIT
      -- set variable to 1 if number is even    
       IF @num % 2 = 0
          SET @result = 1

      -- set variable to 0 if number is odd
        ELSE
          SET @result = 0
      RETURN @result
    END;
```

## 案例陈述

当您处理多个 if 语句时，最好使用 case 语句。它们使你的代码更容易阅读。下面是 case 语句的一般语法。

```
CASE
  WHEN  condition1  THEN  result1  
  WHEN  condition2  THEN  result2  
  .
  .
  .  
  ELSE  result  
END
```

像开关情况一样，检查所有情况，如果满足多个情况，将执行相应的代码块。

下面我们有一个使用 case 语句的函数。

```
CREATE FUNCTION is_greater
( 
   @a AS INT,
   @b AS INT
)
   RETURNS VARCHAR(30)
   AS
   BEGIN
   RETURN( 
        'A is' + 
	CASE
	  -- Case 1
	  WHEN @a > @b THEN 'Greater than'
	  -- Case 2
	  WHEN @a < @b THEN 'Smaller than'
	  ELSE 'Equal to'
	END
	+ 'B')
    END;
```

它比较两个整数，并根据比较结果返回一个字符串。

# 结论

正如我上面提到的，在编写 SQL 语句时，尽量遵循 DRY 原则。当您看到同一段代码在多个语句中使用时，请考虑将其放在一个函数中。函数使你的语句看起来更简洁。

编码快乐！

在 LinkedIn 上与我联系

<https://www.linkedin.com/in/rahulbanerjee2699/>  

*原载于*[*section . io*](https://www.section.io/engineering-education/sql-user-defined-functions/)*2021 年 2 月 1 日*