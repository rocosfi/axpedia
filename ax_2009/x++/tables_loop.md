# Loop on all fields of all tables
 
	static void StandardTablesInfoWPC(Args _args)
	{
	    SysDictTable    dictTable;
	    SysDictField    dictField;
	    FieldId         fieldId;
	    Dictionary      dictionary = new Dictionary();
	    str             tableName;
	    int             i;
	
	    for (i=1 ; i<=dictionary.tableCnt() ; i++)
	    {
	        tableName = tableId2Name(dictionary.tableCnt2Id(i));
	        
            dictTable = new SysDictTable(dictionary.tableCnt2Id(i));
            fieldId = dictTable.fieldNext(0);

            while (fieldId)
            {
                dictField = dictTable.fieldObject(fieldId);

                if (dictField.isSql() && !dictField.isSystem())
                {
                    info(tableName + ' ' + dictField.name());                    
                }

                fieldId = dictTable.fieldNext(fieldId);
            }
	        
	    }	    
	}