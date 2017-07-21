# Unbalanced transactions #

This job aligns unbalanced transaction 

	static void TheAxaptaResetTTS(Args _args)
	{
	    while (appl.ttsLevel() > 0)
	    {
	        info(strfmt("Level %1 aborted",appl.ttsLevel()));
	        ttsAbort;
	    }
	}