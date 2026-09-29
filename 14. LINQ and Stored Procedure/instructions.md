# LINQ and Stored Procedure Practical

## 1. Create Database

```sql
CREATE DATABASE train_master;
GO

USE train_master;
GO
```

## 2. Create Table

```sql
CREATE TABLE train_info
(
    train_id INT IDENTITY(1,1) PRIMARY KEY,
    train_name VARCHAR(100),
    train_type VARCHAR(50),
    arrival_time TIME,
    departure_time TIME,
    start_location VARCHAR(100),
    end_location VARCHAR(100)
);
GO
```

## 3. Insert Stored Procedure

```sql
CREATE PROCEDURE InsertTrain
    @train_name VARCHAR(100),
    @train_type VARCHAR(50),
    @arrival_time TIME,
    @departure_time TIME,
    @start_location VARCHAR(100),
    @end_location VARCHAR(100)
AS
BEGIN
    INSERT INTO train_info
    (
        train_name,
        train_type,
        arrival_time,
        departure_time,
        start_location,
        end_location
    )
    VALUES
    (
        @train_name,
        @train_type,
        @arrival_time,
        @departure_time,
        @start_location,
        @end_location
    );
END
GO
```

## 4. View Stored Procedure

```sql
CREATE PROCEDURE GetAllTrains
AS
BEGIN
    SELECT *
    FROM train_info;
END
GO
```

## 5. Search Stored Procedure

```sql
CREATE PROCEDURE SearchTrain
    @train_id INT
AS
BEGIN
    SELECT *
    FROM train_info
    WHERE train_id = @train_id;
END
GO
```

## 6. Update Stored Procedure

```sql
CREATE PROCEDURE UpdateTrain
    @train_id INT,
    @train_name VARCHAR(100),
    @train_type VARCHAR(50),
    @arrival_time TIME,
    @departure_time TIME,
    @start_location VARCHAR(100),
    @end_location VARCHAR(100)
AS
BEGIN
    UPDATE train_info
    SET
        train_name = @train_name,
        train_type = @train_type,
        arrival_time = @arrival_time,
        departure_time = @departure_time,
        start_location = @start_location,
        end_location = @end_location
    WHERE train_id = @train_id;
END
GO
```

## 7. Delete Stored Procedure

```sql
CREATE PROCEDURE DeleteTrain
    @train_id INT
AS
BEGIN
    DELETE FROM train_info
    WHERE train_id = @train_id;
END
GO
```

## 8. Test Insert

```sql
EXEC InsertTrain
    'Rajdhani Express',
    'Express',
    '10:00',
    '10:15',
    'Delhi',
    'Mumbai';
```

## 9. Test View

```sql
EXEC GetAllTrains;
```

## 10. Test Search

```sql
EXEC SearchTrain 1;
```

## 11. Test Update

```sql
EXEC UpdateTrain
    1,
    'Rajdhani Express',
    'Superfast',
    '10:30',
    '10:45',
    'Delhi',
    'Mumbai';
```

## 12. Test Delete

```sql
EXEC DeleteTrain 1;
```

## 13. LINQ to SQL Data Context

```csharp
DataClassesDataContext db = new DataClassesDataContext();
```

## 14. LINQ View All Records

```csharp
var data = from t in db.train_infos
           select t;

GridView1.DataSource = data;
GridView1.DataBind();
```

## 15. LINQ Search

```csharp
int id = Convert.ToInt32(txtTrainId.Text);

var train = db.train_infos
              .FirstOrDefault(t => t.train_id == id);
```

## 16. LINQ Insert

```csharp
train_info train = new train_info();

train.train_name = txtTrainName.Text;
train.train_type = txtTrainType.Text;
train.arrival_time = TimeSpan.Parse(txtArrivalTime.Text);
train.departure_time = TimeSpan.Parse(txtDepartureTime.Text);
train.start_location = txtStartLocation.Text;
train.end_location = txtEndLocation.Text;

db.train_infos.InsertOnSubmit(train);
db.SubmitChanges();
```

## 17. LINQ Update

```csharp
int id = Convert.ToInt32(txtTrainId.Text);

var train = db.train_infos
              .FirstOrDefault(t => t.train_id == id);

if (train != null)
{
    train.train_name = txtTrainName.Text;
    train.train_type = txtTrainType.Text;
    train.arrival_time = TimeSpan.Parse(txtArrivalTime.Text);
    train.departure_time = TimeSpan.Parse(txtDepartureTime.Text);
    train.start_location = txtStartLocation.Text;
    train.end_location = txtEndLocation.Text;

    db.SubmitChanges();
}
```

## 18. LINQ Delete

```csharp
int id = Convert.ToInt32(txtTrainId.Text);

var train = db.train_infos
              .FirstOrDefault(t => t.train_id == id);

if (train != null)
{
    db.train_infos.DeleteOnSubmit(train);
    db.SubmitChanges();
}
```

## 19. Stored Procedure Insert

```csharp
db.InsertTrain(
    txtTrainName.Text,
    txtTrainType.Text,
    TimeSpan.Parse(txtArrivalTime.Text),
    TimeSpan.Parse(txtDepartureTime.Text),
    txtStartLocation.Text,
    txtEndLocation.Text
);
```

## 20. Stored Procedure View

```csharp
var data = db.GetAllTrains();

GridView1.DataSource = data;
GridView1.DataBind();
```

## 21. Stored Procedure Search

```csharp
int id = Convert.ToInt32(txtTrainId.Text);

var data = db.SearchTrain(id);

GridView1.DataSource = data;
GridView1.DataBind();
```

## 22. Stored Procedure Update

```csharp
db.UpdateTrain(
    Convert.ToInt32(txtTrainId.Text),
    txtTrainName.Text,
    txtTrainType.Text,
    TimeSpan.Parse(txtArrivalTime.Text),
    TimeSpan.Parse(txtDepartureTime.Text),
    txtStartLocation.Text,
    txtEndLocation.Text
);
```

## 23. Stored Procedure Delete

```csharp
db.DeleteTrain(
    Convert.ToInt32(txtTrainId.Text)
);
```

## 24. ASPX Page Labels and Controls

```aspx
<asp:Label ID="lblTrainId" runat="server" Text="Train ID"></asp:Label>
<asp:TextBox ID="txtTrainId" runat="server"></asp:TextBox>

<asp:Label ID="lblTrainName" runat="server" Text="Train Name"></asp:Label>
<asp:TextBox ID="txtTrainName" runat="server"></asp:TextBox>

<asp:Label ID="lblTrainType" runat="server" Text="Train Type"></asp:Label>
<asp:TextBox ID="txtTrainType" runat="server"></asp:TextBox>

<asp:Label ID="lblArrivalTime" runat="server" Text="Arrival Time"></asp:Label>
<asp:TextBox ID="txtArrivalTime" runat="server"></asp:TextBox>

<asp:Label ID="lblDepartureTime" runat="server" Text="Departure Time"></asp:Label>
<asp:TextBox ID="txtDepartureTime" runat="server"></asp:TextBox>

<asp:Label ID="lblStartLocation" runat="server" Text="Start Location"></asp:Label>
<asp:TextBox ID="txtStartLocation" runat="server"></asp:TextBox>

<asp:Label ID="lblEndLocation" runat="server" Text="End Location"></asp:Label>
<asp:TextBox ID="txtEndLocation" runat="server"></asp:TextBox>

<asp:Button ID="btnInsert" runat="server" Text="Insert" />
<asp:Button ID="btnUpdate" runat="server" Text="Update" />
<asp:Button ID="btnDelete" runat="server" Text="Delete" />
<asp:Button ID="btnSearch" runat="server" Text="Search" />
<asp:Button ID="btnView" runat="server" Text="View" />

<asp:GridView ID="GridView1" runat="server">
</asp:GridView>
```

## 25. Connection String

```xml
<connectionStrings>
    <add
        name="train_masterConnectionString"
        connectionString="Data Source=.;Initial Catalog=train_master;Integrated Security=True"
        providerName="System.Data.SqlClient" />
</connectionStrings>
```

## 26. Required Names

```text
Database:
train_master

Table:
train_info

Web Page:
traindata.aspx

Code Behind:
traindata.aspx.cs

Data Context:
DataClassesDataContext

Controls:
txtTrainId
txtTrainName
txtTrainType
txtArrivalTime
txtDepartureTime
txtStartLocation
txtEndLocation

Buttons:
btnInsert
btnUpdate
btnDelete
btnSearch
btnView

GridView:
GridView1
```

## Output

<img src="https://github.com/pritam-samanta-pu/WMAD-IMCA7-25/blob/main/outputs/14.png" alt="LINQ and Stored Procedure" style="width:50%;">
