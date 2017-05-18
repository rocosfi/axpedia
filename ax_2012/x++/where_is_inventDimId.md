#Where the inventDimId is used
It checks all tables to find where is used the selected InventDimId
 
	static void checkInventDimIdERA(Args _args)
	{
	    #AOT
	
	    TreeNodeIterator    treeNodeIterator;
	    TreeNode            treeNode;
	    SysDictTable        sysDictTable;
	    SysDictField        sysDictField;
	    int                 fieldNo;
	    Common              common;
	    DictTable           dictTable;
	    FieldId             fieldId;
	
	    InventDimid         inventDimId = '***********';
	
	    boolean isCandidateConfigKey(ConfigurationKeyId _configurationKeyId)
	    {
	        boolean res = false;
	
	        if (_configurationKeyId != configurationKeyNum(SysDeletedObjects41)
	        &&  _configurationKeyId != configurationKeyNum(SysDeletedObjects60)
	        &&  _configurationKeyId != configurationKeyNum(SysDeletedObjects62))
	        {
	            res = true;
	        }
	
	        return res;
	    }
	
	    boolean isCandidateInventDimIdField(SysDictField _sysDictField)
	    {
	        // The field is affected if it uses InventDimId extended data type or a subtype of it
	        return (_sysDictField.isDerivedFrom(extendedTypeNum(InventDimId))
	                && isCandidateConfigKey(_sysDictField.configurationKeyId())
	                && _sysDictField.saveContents());
	
	    }
	
	    boolean isCandidateInventDimIdTable(SysDictTable _sysDictTable)
	    {
	        ConfigurationKeyId  configurationKeyId = _sysDictTable.configurationKeyId();
	        TableId             tableId = _sysDictTable.id();
	
	        // The table should only be evaluated if it has not been marked for deletion, it is
	        // not a temporary table and is not InventDim nor InventDimCleanUp
	        return (isCandidateConfigKey(configurationKeyId)
	                && !_sysDictTable.isTmp()
	                && !_sysDictTable.isTempDb()
	                && tableId  != tableNum(InventDim)
	                && tableId  != tableNum(InventDimCleanUp));
	    }
	
	    // Traverse through all tables
	    treeNodeIterator    = TreeNode::findNode(#TablesPath).AOTiterator();
	    treeNode            = treeNodeIterator.next();
	    while (treeNode)
	    {
	        // Loop through all fields in the current table
	        sysDictTable    = new SysDictTable(treeNode.applObjectId());
	        if (isCandidateInventDimIdTable(sysDictTable))
	        {
	            for (fieldNo=1;  fieldNo <= sysDictTable.fieldCnt();  fieldNo++)
	            {
	                sysDictField = new SysDictField(sysDictTable.id(), sysDictTable.fieldCnt2Id(fieldNo));
	                if (isCandidateInventDimIdField(sysDictField))
	                {
	                    dictTable = new DictTable(sysDictTable.id());
	                    fieldId = sysDictField.id();
	                    common = dictTable.makeRecord();
	
	                    select count(RecId) from common
	                        where common.(fieldId) == inventDimId;
	
	                    if(common.RecId)
	                    {
	                        info(dictTable.name()+' cont: '+strFmt('%1', common.RecId));
	                    }
	                }
	            }
	        }
	
	        treeNode = treeNodeIterator.next();
	    }
	}