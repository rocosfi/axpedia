# Create return order in x++
## Create header
	SalesTable createReturnOrderHeader( CustAccount          _custAcct,
	                                    ReturnReasonCodeId   _returnReasonCodeId,
	                                    SalesId              _salesId = '',
	                                    SalesId              _ROSalesId = '')
	{
	    SalesTable              prevSalesTable, newSalesTable,ROSalesTable;
	    SalesParameters         salesParameters;
	    ReturnPeriodOfValidity  periodofValid;
	    ;

	    // create the sales order header for the return order
	    salesParameters = SalesParameters::find();
	    periodofValid   = salesParameters.ReturnPeriodOfValidity;
	    ROSalesTable    = SalesTable::find(_ROSalesId);

	    if(!ROSalesTable)
	    {
	        // Initialize from Invoiced SO
	        if(_salesId)
	        {
	            prevSalesTable                        = SalesTable::find(_salesId);
	            newSalesTable.initReturnFromSalesTable(prevSalesTable);
	            newSalesTable.CustAccount             = prevSalesTable.CustAccount;
	        }
	        // Create new Return Order
	        else
	        {
	            newSalesTable.CustAccount             = _custAcct;
	            newSalesTable.SalesId                 = NumberSeq::newGetNum(SalesParameters::numRefSalesId()).num();
	            newSalesTable.ReturnItemNum           = NumberSeq::newGetNum(SalesParameters::numRefReturnItemNum()).num();
	            newSalesTable.SalesType               = SalesType::ReturnItem;
	            newSalesTable.ReturnStatus            = ReturnStatusHeader::Created;
	            newSalesTable.DeliveryDateControlType = SalesDeliveryDateControlType::None;
	            newSalesTable.ShippingDateRequested   = today() + periodofValid;
	        }
	        
	        newSalesTable.initFromCustTable();
	        
	        // periodofValid derive from Sales Parameter
	        if(periodofValid)
	        {
	            newSalesTable.ReturnDeadline      = today() + periodofValid;
	        }
	        else
	        {
	            newSalesTable.ReturnDeadline      = today();
	        }
	        
	        //Reason Code
	        newSalesTable.ReturnReasonCodeId      = _returnReasonCodeId;
	        newSalesTable.SalesType               = SalesType::ReturnItem;
	        newSalesTable.SalesStatus             = SalesStatus::Backorder;

	        if (newSalesTable.validateWrite())
	        {
	            newSalesTable.insert();
	            // return the new Return Order
	            return newSalesTable;
	        }
	    }
	    //return the existing RO
	    return ROSalesTable;
	}

## Create line
	
	SalesLine createReturnOrderLine(	ItemId  _itemId,
	                                    Qty     _qty,
	                                    SalesId _ROSalesId,
	                                    SalesId _salesId = '')
	{
	    CustInvoiceTrans    custInvoiceTrans;
	    SalesLine           salesLine;
	    SalesTable          newRetOrder;
	    CustInvoiceJour     custInvoiceJour;
	    InventDim           inventDim,pvInventDim;
	    ;

	    ttsBegin;
	    
	    if(_salesId)
	    {
	        // Need to populate all the necessary fields for the new salesline
	        // using the existing invoice and the new sales order
	        select custInvoiceJour where custInvoiceJour.RefNum      == RefNum::SalesOrder
	                                    && custInvoiceJour.SalesId  == _salesId;

	        if(custInvoiceJour)
	        {
	            select * from custInvoiceTrans
	                where custInvoiceTrans.SalesId == custInvoiceJour.SalesId
	                    && custInvoiceTrans.InvoiceId == custInvoiceJour.InvoiceId
	                    && custInvoiceTrans.ItemId == _itemid
	                    && custInvoiceTrans.InvoiceDate == custInvoiceJour.InvoiceDate
	                    && custInvoiceTrans.numberSequenceGroup == custInvoiceJour.numberSequenceGroup
	                join inventDim
	                    where inventDim.inventDimId == custInvoiceTrans.InventDimId;

	            salesLine.initFromCustInvoiceTrans(custInvoiceTrans);
	            salesLine.InventTransIdReturn     = custInvoiceTrans.InventTransId;
	            salesLine.InventDimId             = custInvoiceTrans.InventDimId;
	        }

	    }
	    
	    newRetOrder = SalesTable::find(_ROSalesId);
	    salesLine.initFromSalesTable(newRetOrder);
	    salesLine.ItemId = _itemId;
	    
	    // udpate the expected Return quantity
	    salesLine.SalesUnit = InventTableModule::find(_itemId, ModuleInventPurchSales::Invent).UnitId;
	    
	    // set the quantity and amount fields
	    salesLine.LineAmount = salesLine.returnLineAmount();
	    salesLine.SalesQty   = 0;

	    // find correct inventdim
	    inventDim.data(InventDim::find(salesLine.InventDimId));

	    pvInventDim           = inventDim::findOrCreate(inventDim);
	    salesLine.InventDimId = pvInventDim.inventDimId;

	    salesLine.createLine(true, false, true, false, false,false,false,false);
	    salesLine.ExpectedRetQty = -_qty;
	    salesLine.update();

	    ttsCommit;
	    
	    return salesLine;
	}