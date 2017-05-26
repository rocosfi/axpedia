# Conversions

	// string with 3 decimals
	Num2Str(total, 0, 3, DecimalSeparator::Auto, ThousandSeparator::Auto);

	// real with 3 decimals
	decRound(total, 3);

	// date to string conversion
	s = date2Str(currentDate,
	        321,
	        DateDay::Digits2,	
	        DateSeparator::Hyphen, // separator1
	        DateMonth::Digits2,
	        DateSeparator::Hyphen, // separator2
	        DateYear::Digits4);
			
	// datetime to string conversion
	s = datetime2str(currentDate, 321);
