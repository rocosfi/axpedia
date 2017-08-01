# Open menu item by code #

    void clicked()
    {
	    FormRun    formRun;
	    Args       args;
		SalesTable salesTable;
	    ;
	    
	    super();
	    
	    args = new Args();
	    args.caller(element);
	    args.record(salesTable);
	    
	    formRun = new menuFunction(
		    menuitemdisplaystr(SalesTable),
		    menuitemtype::Display).create(args);
	    
	    formRun.init();
	    formRun.run();
    }