# Create a sequence number #

1. Create an EDT just like i have created 'ReasonCode'
2. Go to the Module Table : ex. SalesParameters
and Create a method:


	    public client server static NumberSequenceReference numRefReasonCode()
	    {
		    ;
		    return NumberSeqReference::findReference(typeId2ExtendedTypeId(typeid(ReasonCode)));	
	    }


3. Go to the module Class : ex. NumberSeqReference_Customer. Select the LoadModule Method:

	    numRef.DataTypeId = typeId2ExtendedTypeId(typeid(ReasonCode));
	    numRef.ReferenceHelp = literalstr("Reason Code");
	    numRef.WizardContinuous = False;
	    numRef.WizardManual = NoYes::No;
	    numRef.WizardAllowChangeDown = NoYes::No;
	    numRef.WizardAllowChangeUp = NoYes::No;
	    numRef.WizardHighest = 999999;
	    numRef.SortField = 418;
	    this.create(numRef);   


4. Go to the Table:  Override initvalue method just like below

		public void initValue()
		{
		    NumberSeq NumSeq;
		    ;

		    super();
		    NumSeq = NumberSeq::newGetNum(SalesParameters::numRefReasonCode(), true);
		    this.ReasonCode = NumSeq.num();
		}


5. Go to Module Parameter Form and select Number Sequence Tab, There is a line Created for Reason Code.
