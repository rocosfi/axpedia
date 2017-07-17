# Security key by code #


    public static boolean canAccess(str key)
    {
	    boolean ret;
	    SecurityKeySet  securityKeys;
	    
	    ;
	    
	    securityKeys = new SecurityKeySet();
	    securityKeys.loadUserRights(curuserid());
	    
	    ret = securityKeys.access(securitykeynum(key)) != AccessType::NoAccess;
	    return ret;
    }