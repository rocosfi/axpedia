# Runbase Pack - Unpack #
Here an example of a versioned pack - unpack

Class declaration

	class Test
	{
	    NoYes   exportOrder,
	            exportPackingSlip,
	            exportInvoice,
	            exportSalesQuotation; 
	
	    #define.CurrentVersion(2)
	
	    #localmacro.CurrentList
	        exportOrder,
	        exportPackingSlip,    
	        exportInvoice,
	        exportSalesQuotation 
	    #endmacro
	
	    #localmacro.CurrentList1
	        exportOrder,
	        exportPackingSlip,    
	        exportInvoice
	    #endmacro
	}


pack() method

	public container pack()
	{
	    return [#CurrentVersion, #CurrentList];
	}

unpack() method

	public boolean unpack(container packedClass)
	{
	    Version version = RunBase::getVersion(packedClass);
	    ;
	
	    switch (version)
	    {
	        case #CurrentVersion:
	            [version, #CurrentList] = packedClass;
	            break;
	        case 1: 
	            [version, #CurrentList1] = packedClass;
	            break;
	        default:
	            return false;
	    }
	    return true;
	}