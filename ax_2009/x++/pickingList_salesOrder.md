# Sales Order: Picking List
It creates a picking list of a sales order

	SalesFormLetter salesFormLetter;
	QueryRun queryRun;
	Query query;
	str strSalesTable = "V683904, V683905, V683906";
	;
	salesFormLetter = SalesFormLetter::construct(DocumentStatus::PackingSlip);
	query = new Query(QueryStr(SalesUpdate));
	query.dataSourceTable(tablenum(SalesTable)).addRange(fieldnum(SalesTable, SalesId)).value(strSalesTable);
	queryRun = new QueryRun(query);
	
	salesFormLetter.chooseLinesQuery(queryRun);
	salesFormLetter.transDate(systemdateget());
	salesFormLetter.specQty(SalesUpdate::All);
	salesFormLetter.printFormLetter(false);
	
	salesFormLetter.createParmUpdate();
	salesFormLetter.chooseLines(null,true);
	salesFormLetter.reArrangeNow(true);
	salesFormLetter.run();