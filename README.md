# Dynamic DataGrids

Sometimes you may not know what columns a DataGrid will have at design time. This repo allows for the definition of DataGrid columns at runtime. 

https://github.com/user-attachments/assets/699ffe54-bb44-4434-a697-8d834de708d2

# Version 
Initial 1.0

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Global Script Setup
1. Create a Global Script called "DynamicDataGrid"
2. Add the input parameters below to the Global Script
   1. Columns
   2. DataGridClass
3. Drag a JavaScript action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.0 https://github.com/stadium-software/dynamic-DataGrid */
let obClassName = ~.Parameters.Input.DataGridClass;
let dg = document.querySelector("." + obClassName);
if (!dg) {
    console.error("A DataGrid with the class '" + obClassName + "' was not found");
    return false;
}
let scope = this;
let columns = ~.Parameters.Input.Columns;
let getObjectName = (obj) => {
    let objname = obj.id.replace("-container", "");
    do {
        let arrNameParts = objname.split(/_(.*)/s);
        objname = arrNameParts[1];
    } while ((objname.match(/_/g) || []).length > 0 && !scope[`${objname}Classes`]);
    return objname;
};
setDMValues(dg, "ColumnDefinitions", columns);
function setDMValues(ob, property, value) {
    let obname = getObjectName(ob);
    scope[`${obname}${property}`] = value;
}
```

## Type Setup
1. Create a new type called "Columnn"
2. Add the following properties to the type
   1. name (any)
   2. headerText (any)
   3. visible (any)

![](images/columns-type.png)

## Page Setup
1. Drag a *DataGrid* Control to the page
2. Add a unique classname of your choice to the *DataGrid* classes property (e.g. dynamic-datagrid)
3. The designer will show an error unless you define a column for your *DataGrid* in the *DataGrid* columns property. You can define a dummy column here which will be replaced at runtime. 

## Event Handler Setup
1. Drag a *List* to any event handler script and call it "ColumnsList"
2. Set the *List* type to "Columnn"
3. Populate the List with the columns for your *DataGrid* (you can use any data source for obtain the column names)
   1. name: The column name. The name must match the name of the data field you are planning to display. The name is case sensitive. 
   2. headerText: The heading of the column
   3. visible: a boolean that defines if the column is displayed or not
4. Drag the "DynamicDataGrid" script into the event handler
5. Provide values for the script input parameters
   1. Enter the unique classname you assigned to the *DataGrid* above in the "DataGridClass" input parameter (e.g. dynamic-datagrid)
   2. Select the "ColumnsList" in the Columns input parameter
6. Populate the *DataGrid* with data that matches the list of columns you defined above as per usual

**Columns List Example**
```json
[
    {"name":"ID","headerText":"ID","visible":true},
    {"name":"FirstName","headerText":"FirstName","visible":true},
    {"name":"LastName","headerText":"LastName","visible":true},
    {"name":"NoOfChildren","headerText":"NoOfChildren","visible":true},
    {"name":"NoOfPets","headerText":"NoOfPets","visible":true},
    {"name":"StartDate","headerText":"StartDate","visible":true},
    {"name":"EndDate","headerText":"EndDate","visible":true},
    {"name":"Healthy","headerText":"Healthy","visible":true},
    {"name":"Happy","headerText":"Happy","visible":true},
    {"name":"Subscription","headerText":"Subscription","visible":true}
]
```