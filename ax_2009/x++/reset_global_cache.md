# Reset global cache #

	static void ResetCodeCacheId(Args _args)
	{
	    SysSQMSettings  sqm;
	    Guid            emptyGuid;
	    ;
	
	    emptyGuid=str2Guid('');
	    update_recordset sqm
	        setting GlobalGUID = emptyGuid;
	}
