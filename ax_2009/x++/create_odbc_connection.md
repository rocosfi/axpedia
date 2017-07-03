# Create ODBC connection #

Open **Data Source (ODBC)** windows application on the machine where AOS is installed.

Follow these steps:



1. Add a new System DSN connection
2. Select SQL Server native Client 10.0
3. Insert Name, Description and Database server
4. Select Windows integrated authentication
5. Change the database selecting yours

Repeat the steps for a User DSN connection (in order to work fine with jobs and other Ax client interactions) 

Your driver is ready and with a simple statement you can retrieve your data. For example:

	static void testDB(Args _args)
	{
	    ODBCConnection myODBC;
	    Statement myStatement;
	    LoginProperty myLoginProperty;
	    Resultset myResultset;
	
	    str mySQLStatement;
	    str myConnectionString;
	
	    str myDSN      = "YOUR_ODBC_BAME";
	    str myUserName = ""; // leave empty if you want to use Windows auth
	    str myPassword = ""; // leave empty if you want to use Windows auth
	    ;
	
	    myConnectionString = strFmt("DSN=%1;UID=%2;PWD=%3", myDSN, myUserName, myPassword);
	
	    myLoginProperty = new LoginProperty();
	    myLoginProperty.setOther(myConnectionString);
	
	    try
	    {
	        myODBC      = new OdbcConnection(myLoginProperty);
	        myStatement = myODBC.createStatement();
	
	        mySQLStatement = "SELECT LastName,FirstName FROM test";
	        myResultSet=myStatement.executeQuery(mySQLStatement);
	        while (myResultSet.next())
	        {
	            info(strFmt("%1 %2", myResultSet.getString(1), myResultSet.getString(2)));
	        }
	    } 
	    catch
	    {
	        error('Unexpected error');
	    }
	}
