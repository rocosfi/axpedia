# Personalize lookup #

You need two steps: override lookup method on the form and SysTableLookup with a query (better on the table).

Here an example

1) On the datasource field, override lookup method. For example, on the itemId field of a salesLine datasource 

		public void lookup(FormControl _formControl, str _filterStr)
		{
		    ;
	
		    InventTable::lookupItemId(_formControl, salesLine.FILTERFIELD);
		}


2) On the InventTable table, we provide a static client method called lookupItemId like this

		static client void lookupItemId(FormStringControl _ctrl,
		                                                FILTERFIELD _filterField) 
		{
		    SysTableLookup          sysTableLookup = SysTableLookup::newParameters(tablenum(InventTable), _ctrl);
		    Query                   query = new Query();
		    QueryBuildDataSource    queryBuildDataSource = query.addDataSource(tablenum(InventTable));
		    ;
		
		    sysTableLookup.addLookupfield(fieldnum(InventTable, ItemId));
		    sysTableLookup.addLookupfield(fieldnum(InventTable, ItemName));
		
		    if(_filterField != '') // or similar
		    {
		        queryBuildDataSource.addRange(fieldnum(InventTable, FILTER)).value(queryvalue(_filterField));
		    }
		   
		    sysTableLookup.parmQuery(query);
		    sysTableLookup.performFormLookup();
		}