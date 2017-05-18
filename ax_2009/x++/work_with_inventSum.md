# Work with InventSum #
## Available physical calculation ##
    localInventDim                  = InventDim::findOrCreateBlank();
	localInventDim.InventLocationId = localSalesLine.inventDim().InventLocationId;
	localInventDim.InventSiteId     = localSalesLine.inventDim().InventSiteId;
	
	localInventDimParm.initFromInventDim(localInventDim);
	localInventSum  = InventSum::findSum(localSalesLine.itemId, localInventDim, localInventDimParm, InventSumFields::All);
	
	availQty = localInventSum.availPhysical();

or using inventOnHand class

	localInventDim = InventDim::findOrCreateBlank();
	static InventOnhand newItemDim(
	                  ItemId             _itemId,
	                  InventDim          _inventDim,
	                  InventDimParm      _inventDimParm
	                  )
	{
	    InventOnhand inventOnhand = new InventOnhand();
	    ;
	
	    inventOnhand.parmInventDim(_inventDim);
	    inventOnhand.parmInventDimParm(_inventDimParm);
	    inventOnhand.parmItemId(_itemId);
	
	    return inventOnhand;
	}

or using a query

	select firstonly
	        SUM(PostedQty),
	        SUM(PostedValue),
	        SUM(PhysicalValue),
	        SUM(Deducted),
	        SUM(Registered),
	        SUM(Received),
	        SUM(Picked),
	        SUM(ReservPhysical),
	        SUM(ReservOrdered),
	        SUM(OnOrder),
	        SUM(Ordered),
	        SUM(Arrived),
	        SUM(QuotationReceipt),
	        SUM(QuotationIssue),
	        SUM(PhysicalInvent),
	        SUM(PostedValueSecCur_RU),
	        SUM(PhysicalValueSecCur_RU),
	        SUM(AvailPhysical),
	        SUM(AvailOrdered)
	    from invSum
	        where invSum.ItemId == _inventTable.ItemId
	           && invSum.Closed == NoYes::No
	    exists join tableId from invDim
	        where (invDim.InventDimId       == invSum.inventDimId)
	           && (invDim.InventSiteId      == _location.InventSiteId)
	           && (invDim.InventLocationId  == _location.InventLocationId);
	
	    availQty = invSum.availPhysical();
