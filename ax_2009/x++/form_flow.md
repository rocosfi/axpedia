# Form method sequence flow #


This gives the information of method calls in the form level while

1. Opening the Form.
2. Creating/Updating/Deleting the record in the Form.
3. Closing the Form.


Sequence of Methods calls while **opening** the Form

	Form --- init ()
	Form --- Datasource --- init ()
	Form --- run ()
	Form --- Datasource --- execute Query ()
	Form --- Datasource --- active ()


Sequence of Methods calls while **closing** the Form

	Form --- canClose ()
	Form --- close ()

Sequence of Methods calls while **creating** the record in the Form

	Form --- Datasource --- create ()
	Form --- Datasource --- initValue ()
	Table --- initValue ()
	Form --- Datasource --- active ()

Sequence of Method calls while **saving** the record in the Form

	Form --- Datasource --- ValidateWrite ()
	Table --- ValidateWrite ()
	Form --- Datasource --- write ()
	Table --- insert ()

Sequence of Method calls while **deleting** the record in the Form

	Form --- Datasource --- validatedelete ()
	Table --- validatedelete ()
	Table --- delete ()
	Form --- Datasource --- active ()

Sequence of Methods calls while **modifying** the fields in the Form

	Table --- validateField ()
	Table --- modifiedField ()
