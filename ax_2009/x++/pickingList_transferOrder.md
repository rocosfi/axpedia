#Transfer Order: Picking List
It creates a picking list of a transfer order

	InventTransferParmTable inventTransferParmTable;
	InventTransferMultiPick inventTransferMultiPick = InventTransferMultiPick::construct();
	
	;
	InventTransferParmTable.TransferId = "transferid" //Provide transferId
	inventTransferParmTable.EditLines = true;
	inventTransferParmTable.AutoReceiveQty = true;
	inventTransferParmTable.UpdateType = InventTransferUpdateType::PickingList;
	inventTransferParmTable.PickUpdateQty =InventTransferPickUpdateQty::All ;
	inventTransferMultiPick.runUpdate(InventTransferParmTable);