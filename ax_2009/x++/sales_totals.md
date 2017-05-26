# Sales totals
SalesTotals class manipulates the total amount of a sales order

	SalesTotals salesTotals;
    SalesTable  salesTable;

    TaxAmountCur salesAmt, taxAmount, amt1, discountAmt, totcharges, totOfOrder, contributionRatio;
    Tax tax;

    ;
    salesTotals 	= SalesTotals::construct(_salesTable);
    tax 			= Tax::construct(NoYes::No);

    salesAmt          = salesTotals.totalBalance();
	taxAmount         = salesTotals.totalTaxAmount();
	discountAmt       = salesTotals.totalEndDisc();
	totcharges        = salesTotals.totalMarkup();
	totOfOrder        = salesTotals.totalAmount();
	contributionRatio = salesTotals.totalContributionRatio();

    info(Strfmt("Subtotal Amount %1",salesAmt ));
    info(Strfmt("The tax amount is %1",taxAmount ));
    info(Strfmt("The Discount is %1",discountAmt ));
    info(Strfmt("Charges/ Markup %1",totcharges ));
    info(Strfmt("Invoice Amount %1",totOfOrder ));
    info(Strfmt("Contribution ratio %1",contributionRatio ));
