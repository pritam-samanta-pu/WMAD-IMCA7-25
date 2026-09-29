# 1. Calendar.aspx

```aspx
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Calendar.aspx.cs" Inherits="WebApplication1.Calendar" %>

<%@ Register Assembly="AjaxControlToolkit"
    Namespace="AjaxControlToolkit"
    TagPrefix="ajaxToolkit" %>

<!DOCTYPE html>

<html>
<head runat="server">
    <title>AJAX Calendar Navigation</title>
</head>

<body>
    <form id="form1" runat="server">

        <asp:ScriptManager
            ID="ScriptManager1"
            runat="server">
        </asp:ScriptManager>

        <div>

            <h2>AJAX Calendar Navigation</h2>

            <asp:Label
                ID="lblDate"
                runat="server"
                Text="Select Date:">
            </asp:Label>

            <asp:TextBox
                ID="txtDate"
                runat="server">
            </asp:TextBox>

            <ajaxToolkit:CalendarExtender
                ID="CalendarExtender1"
                runat="server"
                TargetControlID="txtDate"
                Format="dd/MM/yyyy"
                FirstDayOfWeek="Sunday">
            </ajaxToolkit:CalendarExtender>

        </div>

    </form>
</body>
</html>
```

# 2. Calendar.aspx.cs

```csharp
using System;

namespace WebApplication1
{
    public partial class Calendar : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }
    }
}
```

# 3. Calendar.aspx.designer.cs

```csharp
namespace WebApplication1
{
    public partial class Calendar
    {
        protected global::System.Web.UI.HtmlControls.HtmlForm form1;

        protected global::System.Web.UI.ScriptManager ScriptManager1;

        protected global::System.Web.UI.WebControls.Label lblDate;

        protected global::System.Web.UI.WebControls.TextBox txtDate;

        protected global::AjaxControlToolkit.CalendarExtender CalendarExtender1;
    }
}
```

# 4. Web.config

```xml
<?xml version="1.0"?>

<configuration>

  <system.web>

    <compilation debug="true" targetFramework="4.8" />

    <httpRuntime targetFramework="4.8" />

  </system.web>

</configuration>
```

# 5. Required NuGet Package

```text
AjaxControlToolkit
```

# 6. NuGet Package Manager Console

```powershell
Install-Package AjaxControlToolkit
```

# 7. Required Register Statement

```aspx
<%@ Register Assembly="AjaxControlToolkit"
    Namespace="AjaxControlToolkit"
    TagPrefix="ajaxToolkit" %>
```

# 8. Required ScriptManager

```aspx
<asp:ScriptManager
    ID="ScriptManager1"
    runat="server">
</asp:ScriptManager>
```

# 9. Calendar Extender

```aspx
<ajaxToolkit:CalendarExtender
    ID="CalendarExtender1"
    runat="server"
    TargetControlID="txtDate"
    Format="dd/MM/yyyy"
    FirstDayOfWeek="Sunday">
</ajaxToolkit:CalendarExtender>
```

# 10. TextBox

```aspx
<asp:TextBox
    ID="txtDate"
    runat="server">
</asp:TextBox>
```

# 11. Project Structure

```text
WebApplication1
│
├── Calendar.aspx
├── Calendar.aspx.cs
├── Calendar.aspx.designer.cs
├── Web.config
│
└── References
    └── AjaxControlToolkit
```

## Output

<img src="https://github.com/pritam-samanta-pu/WMAD-IMCA7-25/blob/main/outputs/15.png" alt="AJAX" style="width:50%;">
