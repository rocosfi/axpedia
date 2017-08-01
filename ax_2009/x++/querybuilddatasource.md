# Form QueryBuildDataSource #

classDeclaration
	
	QueryBuildDataSource	qbds;
	TransDate        		dateCriteria;
    QueryBuildRange  		qbrDate;
	SalesType		 		salesTypeCriteria;
	QueryBuildRange  		qbrSalesType;


datasource.init

	qbds			= TABLE_Q.dataSourceTable(tablenum(TABLE));	
	qbrDate 		= SysQuery::findOrCreateRange(qbds, fieldnum(TABLE, FIELD));
	qbrSalesType 	= SysQuery::findOrCreateRange(qbds, fieldnum(TABLE, FIELD));

datasource.executeQuery

	qbrDate.value(dateCriteria ? queryValue(dateCriteria) : '');
	qbrDate.value(enum2str(salesTypeCriteria));