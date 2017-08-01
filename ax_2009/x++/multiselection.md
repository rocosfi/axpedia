# Multiselection on form datasource #

	void clicked()
	{
	    Container            c;
	    MultiSelectionHelper helper;
	    SalesLine            localLine;
		int					 i = 1;
	    ;
	    
	    helper = MultiSelectionHelper::construct();
	    helper.parmDatasource(SalesLine_DS);
	
	    localLine = helper.getFirst();
	    while (localLine.RecId != 0)
	    {
	        c += localLine;
			c = conIns(c, i, localLine);
	        localLine = helper.getNext();
	        i++;
	    }

		// do something with container c
	}