# Query builder #

Here an example of a query on salesLine table with a filter on a given itemId

    Query                query = new Query(); 
    QueryBuildDataSource qbdsSalesLine, qbdsInventTable;
    ;

    qbdsSalesLine   = query.addDataSource(tablenum(SalesLine));
    qbdsInventTable = qbdsSalesLine.addDataSource(tableNum(InventTable));
    qbdsInventTable.joinMode(JoinMode::ExistsJoin);
    qbdsInventTable.relations(false); // unlink current relation between tables
    qbdsInventTable.addLink(fieldNum(SalesLine, ItemId), fieldNum(InventTable, ItemId)); // set a new link
    qbdsInventTable.addRange(fieldnum(InventTable, ItemId)).value(queryvalue(_itemId));