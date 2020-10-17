# PLSQL

# INTRODUCTION

Procedural Language extension to Structured Query Language
It provides all software engineering features such as data encapsulation, exception handling, information hiding and object orientation. It provides all procedural conducts that are present in any third generation language (3GL)

A block is executed by PLSQL Engine. The engine is a virtual machine The engine is present inside the oracle database which is used for storing subprograms
PLSQL runtime environment doesn't affect the overall architecture
ALL statements are processed in the procedural statement executor
PLSQL program can run anywhere on an Oracle Server runs irrespective of the operating system and platform.

# PLSQL Block  Structure

Declare: define the variables, cursors, exceptions (optional)

Begin: It is mandatory and contains SQL and PLSQL statements

Exception: contains exceptions(optional)

End;  semicolan (;) is Mandatory


# VARIABLES

PLSQL VARIABLES: 
  Scalar store single value like varchar2,
	Reference stores the reference to a point like a pointer, Large Object(LOB): used for holding large data types. It specifies location of large objects like images stored outside the table,  
  Composite: available by using PLSQL collection and record variables
	
TYPES OF LOB: 
  
  • CLOB (Character LOB), stored large block of character data
	
  • BLOB(Binary LOB) Stores structured and unstructured binary objects in the database
	
  • BFILE Stores large binary files
	
  • NCLOB(National Language CLOB) stores blocks with multibyte NCHAR Unicode data


NON PLSQL VARIABLES: Bind Variables Also known as host variables. Created with the help of VARIABLE keyword. Referenced with a preceding semicolon.  PRINT command is used for printing bind variables. These are available even after the block is executed. It is not a global variable. It is an environment variable.    SET AUTOPRINT ON automatically displays bind variable in a successful PLSQL program. 

NOTNULL constraint can't be applies to a %TYPE attribute

# SYNTAX

COMMENTS IN PLSQL:

Single line Comment    --

Multiple Line Comments   /*  */

Character and Date literals must be enclosed in single quotation marks, Number can use normal as well as scientific notation

SQL FUNCTIONS IN PLSQL:
	
  • DECODE
	
  • Group Functions: AVG, MIN, MAX, COUNT, SUM, STDDEV & VARIANCE etc. These group functions are only applicable to rows in a table.
	
  • NEXTVAL, CURRVAL

DATE TYPE CONVERSIONS:
	
  • Implicit Conversion - Dynamic conversion automatically happens in case of 
		
   1. Character and Numbers
		
   2. Characters and Dates
	
  • Explicit Conversion Contains the functions stated below
		
   1. TO_CHAR
		
   2. TO_DATE
		
   3. TO_NUMBER
		
   4. TO_TIMESTAMP

Default date format:   DD-MON--YY

# SEQUENCES:

Functions: NEXTVAL, CURRVAL

CODE: 

    CREATE SEQUENCE sequence_name
      MINVALUE value
      MAXVALUE value
      START WITH value
      INCREMENT BY value
      CACHE value;
      
# NESTED BLOCK

If there is no variable in the inner block the outer block variable acts as the global variable and is used for performing the required tasks.
Inner block variable can only be accessed inside the inner block as it is a local variable. Qualifiers can be used to access the outer element having the same name as the inner element. It can be called by the following method-    
   
    BEGIN<<outer>>
    DECLARE
    var_name:='abc';
    BEGIN
	    DECLARE
	    var_name: = 'xyz';
	    BEGIN
	    DBMS_OUTPUT.PUT_LINE(outer.var_name);
	    DBMS_OUTPUT.PUT_LINE(.var_name);
	    END;
    END;
    
The above code will first print abc then xyz in the next line

SQL STATEMENTS IN PLSQL:

TRANSACTIONAL STATEMENTS:
	
  • ROLLBACK
	
  • SAVEPOINT
	
  • COMMIT
 
NOTE:
  
• PLSQL doesn't support DDL statements directly, DML statements can be used in PLSQL

• With INTO Clause queries must return only 1 row.

• For a single element add the WHERE clause to the SELECT query

• Name of local variable take precedence over Database tables. Database table columns name take precedence over local variable

• MERGE statements need condition when matched and when not matched and values are updated or inserted as per the above mentioned conditions.

# CONDITIONAL STATEMENTS:

IF Statement: Used to check a condition

    If condition
	    THEN
    ELSIF condition
	    THEN
    END;

CASE Expression: It works similar to SWITCH statement in different languages. It is used to return a result based on one or more alternatives.

    CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    [ELSE resultN+1]
    END

LOOPS:

Type of loops:

BASIC:
	  
    LOOP
	  EXIT WHEN condition;
	  END LOOP;
    
FOR: It works on the basis of count. The word REVERSE can be used for opposite indexing
	
    FOR I in REVERSE lower_num .. Upper_num  LOOP
	  Statements;
	  END LOOP;
    
WHILE: It works as per the condition (True/False)
	
    WHILE a<b LOOP
	  Statements;
	  END LOOP;
    
All three types of loops require an exit statement to signify the end.

NOTE:

• PLSQL supports nested loop the end statement shoud reference the correct label while using nested loops

• CONTINUE: Statement is used to skip the remaining statements for the specific iteration in use

# CURSORS:

Cursor is a pointer in the private memory generated by the oracle server to handle the data of SELECT statement. Types of Cursors:

• Implicit: Created and managed by the Oracle server internally

• Explicit: User defined cursors

IMPLICIT CURSORS:

• SQL%FOUND

• SQL%NOTFOUND

• SQL%ROWCOUNT

If PLSQL doesn't return any row an EXCEPTION is raised.

EXPLICIT CURSORS:

The cursor is created when a SELECT statement returns a row. The cursor points to the reference of a row which allows are user to keep account of the rows being used.
Steps involved in working with Explicit Cursor:

    • DECLARE: Declaring a cursor with a name

    • OPEN: Dynamically allocates memory, It also assists in finding the active set of rows

    • FETCH: It fetches rows from cursor one at a time. The cursor keeps on iterating till the data is present. INTO clause is used in the FETCH statement.
	
    • Check for data: %NOTFOUND can be used to determine the EXIT condition

    • CLOSE: Disables the cursor. Can be opened later as per the demand.

%ISOPEN statement can be used to check if the cursor is open or closed.

INTO clause should not be used along with the SELECT clause in case of Cursors. In case of Explicit cursor if no rows are found it does not raise an exception. Row count should be observed in order to keep track of the rows.
Initially a user can open maximum of 50 cursors, it can be changed manually.

CURSOR FOR LOOP:

This is a shortcut method for fetching rows as it doesn't require the FETCH statement as it automatically fetches the row and move on to the next iteration, also it automatically closes the cursor once the iterations end. 

NOTE:

• Parametric Cursors: Add parameters while declaring the cursor. The cursor should also be opened along with the parameters.

• FORUPDATE clause: It is used to lock a single row and apply the required changes when multiple rows are fed from the database. This clause stops other sessions from accessing the particular row before the statement is executed.

• NOWWAIT statement makes sure that the Oracle server knows to wait for a certain time. The statement is optional but can be used as an alternative way for UPDATEFOR clause.

• WHERECURRENTOF clause is used in conjunction with the FORUPDATE clause it is used to specify the current value of the variable which assist in manipulating the specific row.

CURSOR VARIABLE: REF is used to set reference to a point, it acts like a pointer. The cursor has a data type of REF cursor. These can be used anywhere in the program. For instance it can be passed into a global variable as well as a bind variable.
A cursor is static but a cursor variable is dynamic as it points a location in the memory which keeps on changing from time to time.
There are toe types of Ref cursors:
	
• Strong Cursors: They have a specified return type because of which they are less error prone.
	
• Weak Cursors: Weak cursors can be associated to any query, therefore these are considered to be flexible in nature.

Cursor Variables can't be declared inside a package

The statements used for dynamic multirow query are:

    • OPEN-FOR

    • FETCH

    • CLOSE

CODE:
    
    DECLARE
    CURSOR declaration;
    Declare Variables
    BEGIN
    Assign Values if any
    OPEN Cursor;
    LOOP
	    FETCH from cursor;
	    EXIT WHEN row%NOTFOUND;
    END LOOP;
    CLOSE Cursor;
    END;
    /

# EXCEPTION HANDLING:

When a code has correct syntax but faces issues like fetching multiple data instead of single, Null pointer etc these are termed as Exception. Unlike errors exceptions can be handled. The PLSQL block is terminated after an exception is raised.
Types of Exception:

• PRE-DEFINED ORACLE SERVER ERROR: These are implicit exceptions and need not be declared. These exceptions needs to be referenced by their pre-defined name. Some examples of pre-defined exceptions are NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE etc.

• NON PRE-DEFINED ORACLE SERVER ERROR: These are also implicitly called by the oracle server but needs to be defined in the declarative section of the block. PRAGMAEXCEPTION_INIT is used to associate an error with the error number. SQLCODE and SQLERRM are used to return the error code and error message respectively.

• USER-DEFINED EXCEPTIONS: A user can define a custom exception as per the requirement. It has be declared in the DECLARE section and needs to be raised explicitly  and finally it has to be handled in the EXCEPTION section of the block. If a user exception is found all the other blocks are bypassed giving priority to the user-defined exception.

Code:

    DECLARE
    MYEXP EXCEPTION;
    BEGIN
    ENO := &EMPNO;
    IF Condition THEN
	    RAISE MYEXP;
    END IF;
    EXCEPTION
    WHEN MYEXP THEN
	    DBMS_OUTPUT.PUT_LINE('NO SUCH DATA PRESENT');
    WHEN OTHERS THEN
	    ERNO:=SQLCODE;
	    MSG:=SQLERRM;
	    INSERT INTO Table_name VALUES(ERNO,MSG);
    END;
    
# PROCEDURE:

These are named PLSQL blocks also known as PLSQL Subprograms. These follow the same structure as that of anonymous block but procedures can accept parameters too. These are compiled only once and stored in the database as a schema object, hence can be used multiple times by just invoking them via their name. It's not compulsory for a Procedure to return something, although this can be achieved by modifying the mode to OUT or INOUT. 
It supports three modes:

• IN : Nothing is returned as an output. It is the default mode if nothing is specified.

• INOUT : It takes input and returns an output

• OUT : This modes returns an output for the subprogram

Parameters can be added to a Procedure. There are two types of parameters:

• Formal Parameters: These are the parameters declared in the parameter list of the subprogram. 

    For eg: CREATE PROCEDURE AB(a NUMBER).... Here a is the Formal Parameter. 

In case of IN mode formal parameters cannot be assigned any value while declaration or even after being passed in the procedure, where as in case of OUT and INOUT the parameter should be explicitly defined by the user

• Actual Parameter: These are the parameters which are returned by the subprogram 
    
    For eg: ... RETURN(a). Here a is the Actual Parameter

Both Formal Parameter and Actual Parameter must have the same data type.

A procedure can invoke other procedures or functions.

Code: 

    CREATE OR REPLACE PROCEDURE <P NAME >(PLIST)
    IS
    <DECLARE ALL THE VARIABLES>
    BEGIN
    <EXECUTEABLE STATEMENTS>
    EXCEPTION
    <EXCEPTION HNDLING>
    END;

For executing a procedure
    
    EXEC NAME(PARAMETERS);

# FUNCTION:

A Function is similar to a procedure but it is mandatory for a function to Return something. Bind variables can't be referenced in a block of a stored function. Only Stored Functions are Callable from SQL queries. Procedures can only be called from SQL queries if they are invoked by a Function.

DROP can be used to drop a function or a procedure.

Code:

    CREATE OR REPLACE FUNCTION <F NAME > RETURN Data type
    IS
    <DECLARE ALL THE VARIABLES>
    BEGIN
    <EXECUTEABLE STATEMENTS>
    EXCEPTION
    <EXCEPTION HNDLING>
    RETURN data
    END;


# PACKAGES:

Package is used to bundle a set of subprograms that can be referenced from a single place. It has two parts:
• Specification
• Body

A package itself can't accept parameters. Package gives an option of adding private variables that can only be accessed by the procedures and functions referenced by the package.

Overloading subprogram is applicable in PLSQL. Overloading is a term for allowing a user to create multiple sub programs with the same name but containing different parameters. 

Specification Code:

    CREATE OR REPLACE PACKAGE package_name IS
	    PROCEDURE P_Name(Parameters);
	    PROCEDURE P2_Name(Parameters);
    END package_name;

Body Code

    CREATE OR REPLACE PACKAGE BODY package_name IS
	    PROCEDURE P_Name(Parameters) IS
	    BEGIN
		    <EXECUTABLE STATEMENTS>
	    END P_Name;
	    PROCEDURE P2_Name(Parameters) IS
	    BEGIN
		    <EXECUTABLE STATEMENTS>
	    END P2_Name;
    END Package_name;


# TRIGGERS:

A Trigger is similar to a store procedure but is invoked in a different way. Triggers are implicitly invoked by the Oracle Database when it observes a triggering event such as INSERT, UPDATE etc.

TRIGGERING EVENTS:

• DML Operations

• DDL Operations

• Start up Session, End Session

• User Log On, Log Off
	
A trigger can be invoked at multiple times which needs to specified beforehand. The timings are stated as follows:

• BEFORE: It checks if the next query is executable or not. It is also used to store the data before the INSERT/ UPDATE query is applied. We can gather back the data using ROLLBACK statement.

• INSTEADOF: These ae written on top of views, hence are explicitly fired by the oracle server. It is known that a DML statement can't modify a view, INSTEADOF trigger acts as an alternative as it is fired as soon as the a DML statement is executed and it updates the view.
A user can assign a mode as ENABLED or DISABLED for a trigger

• AFTER: It is used to update information regarding the changes made in the database/table.

Qualifiers: These are used to differentiate between the old and new value after the changes made in the column : needs to added before the Qualifier before calling it, eg: :Old.name, :NEW.name. There are two types of Qualifiers:

• OLD

• NEW

COMPOUND TRIGGERS: A compound trigger is a single trigger which allows to take action on different trigger timing points. Qualifiers can't appear in the declaration of a BEFORE or AFTER trigger. The state of NEW can only be altered after BEFORE EACH SECTION. FOLLOWS clause need to be added to correctly implement the Compound Triggers. 

CODE:

    CREATE [OR REPLACE] TRIGGER trigger name
	    {BEFORE | AFTER } { DDL event} ON {DATABASE | SCHEMA}
	    [WHEN (...)]
	    DECLARE
		    Variable declarations
	    BEGIN
		    <EXECUTABLE STATEMENTS>
	    END;
