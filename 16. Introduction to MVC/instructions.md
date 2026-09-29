# Introduction to MVC – Book Management System

## 1. Create Database

```sql
CREATE DATABASE book_master;
GO

USE book_master;
GO
```

## 2. Create Table

```sql
CREATE TABLE book_info
(
    book_id INT IDENTITY(1,1) PRIMARY KEY,
    book_name VARCHAR(100) NOT NULL,
    author VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL
);
GO
```

## 3. Insert Sample Data

```sql
INSERT INTO book_info
(book_name, author, price)
VALUES
('C Programming', 'Dennis Ritchie', 450),
('Java Programming', 'James Gosling', 550),
('ASP.NET MVC', 'John Smith', 600);
GO
```

## 4. MVC Project Structure

```text
BookManagement
│
├── Controllers
│   └── BookController.cs
│
├── Models
│   └── Book.cs
│
├── Views
│   └── Book
│       ├── Index.cshtml
│       ├── Create.cshtml
│       ├── Edit.cshtml
│       ├── Details.cshtml
│       └── Delete.cshtml
│
├── Web.config
└── Global.asax
```

## 5. Model – Book.cs

```csharp
using System.ComponentModel.DataAnnotations;

namespace BookManagement.Models
{
    public class Book
    {
        public int book_id { get; set; }

        [Required(ErrorMessage = "Book name is required")]
        public string book_name { get; set; }

        [Required(ErrorMessage = "Author name is required")]
        public string author { get; set; }

        [Required(ErrorMessage = "Price is required")]
        [Range(1, 100000, ErrorMessage = "Price must be greater than 0")]
        public decimal price { get; set; }
    }
}
```

## 6. Connection String

```xml
<connectionStrings>
    <add name="BookConnection"
         connectionString="Data Source=.;Initial Catalog=book_master;Integrated Security=True"
         providerName="System.Data.SqlClient" />
</connectionStrings>
```

## 7. BookController.cs

```csharp
using System;
using System.Configuration;
using System.Data.SqlClient;
using System.Web.Mvc;
using BookManagement.Models;

namespace BookManagement.Controllers
{
    public class BookController : Controller
    {
        string cs = ConfigurationManager.ConnectionStrings["BookConnection"].ConnectionString;

        public ActionResult Index()
        {
            return View();
        }

        public ActionResult Create()
        {
            return View();
        }

        [HttpPost]
        public ActionResult Create(Book book)
        {
            if (ModelState.IsValid)
            {
                using (SqlConnection con = new SqlConnection(cs))
                {
                    string query = "INSERT INTO book_info (book_name, author, price) VALUES (@book_name, @author, @price)";

                    SqlCommand cmd = new SqlCommand(query, con);

                    cmd.Parameters.AddWithValue("@book_name", book.book_name);
                    cmd.Parameters.AddWithValue("@author", book.author);
                    cmd.Parameters.AddWithValue("@price", book.price);

                    con.Open();
                    cmd.ExecuteNonQuery();
                }

                return RedirectToAction("Index");
            }

            return View(book);
        }

        public ActionResult Details(int id)
        {
            Book book = null;

            using (SqlConnection con = new SqlConnection(cs))
            {
                string query = "SELECT * FROM book_info WHERE book_id=@book_id";

                SqlCommand cmd = new SqlCommand(query, con);
                cmd.Parameters.AddWithValue("@book_id", id);

                con.Open();

                SqlDataReader dr = cmd.ExecuteReader();

                if (dr.Read())
                {
                    book = new Book
                    {
                        book_id = Convert.ToInt32(dr["book_id"]),
                        book_name = dr["book_name"].ToString(),
                        author = dr["author"].ToString(),
                        price = Convert.ToDecimal(dr["price"])
                    };
                }
            }

            return View(book);
        }

        public ActionResult Edit(int id)
        {
            return Details(id);
        }

        [HttpPost]
        public ActionResult Edit(Book book)
        {
            if (ModelState.IsValid)
            {
                using (SqlConnection con = new SqlConnection(cs))
                {
                    string query = @"UPDATE book_info
                                     SET book_name=@book_name,
                                         author=@author,
                                         price=@price
                                     WHERE book_id=@book_id";

                    SqlCommand cmd = new SqlCommand(query, con);

                    cmd.Parameters.AddWithValue("@book_id", book.book_id);
                    cmd.Parameters.AddWithValue("@book_name", book.book_name);
                    cmd.Parameters.AddWithValue("@author", book.author);
                    cmd.Parameters.AddWithValue("@price", book.price);

                    con.Open();
                    cmd.ExecuteNonQuery();
                }

                return RedirectToAction("Index");
            }

            return View(book);
        }

        public ActionResult Delete(int id)
        {
            using (SqlConnection con = new SqlConnection(cs))
            {
                string query = "DELETE FROM book_info WHERE book_id=@book_id";

                SqlCommand cmd = new SqlCommand(query, con);

                cmd.Parameters.AddWithValue("@book_id", id);

                con.Open();
                cmd.ExecuteNonQuery();
            }

            return RedirectToAction("Index");
        }

        public ActionResult Search(string search)
        {
            return View("Index");
        }
    }
}
```

## 8. Index.cshtml

```html
@model IEnumerable<BookManagement.Models.Book>

@{
    ViewBag.Title = "Book List";
}

<h2>Book Management</h2>

<p>
    @Html.ActionLink("Add New Book", "Create")
</p>

<form method="get" action="@Url.Action("Search", "Book")">
    <input type="text" name="search" placeholder="Search Book" />
    <input type="submit" value="Search" />
</form>

<br />

<table border="1">
    <tr>
        <th>Book ID</th>
        <th>Book Name</th>
        <th>Author</th>
        <th>Price</th>
        <th>Actions</th>
    </tr>

    @foreach (var item in Model)
    {
        <tr>
            <td>@item.book_id</td>
            <td>@item.book_name</td>
            <td>@item.author</td>
            <td>@item.price</td>
            <td>
                @Html.ActionLink("View", "Details", new { id = item.book_id })
                |
                @Html.ActionLink("Edit", "Edit", new { id = item.book_id })
                |
                @Html.ActionLink("Delete", "Delete", new { id = item.book_id })
            </td>
        </tr>
    }
</table>
```

## 9. Create.cshtml

```html
@model BookManagement.Models.Book @{ ViewBag.Title = "Add Book"; }

<h2>Add Book</h2>

@using (Html.BeginForm()) { @Html.ValidationSummary(true)

<div>
	@Html.LabelFor(model => model.book_name) @Html.TextBoxFor(model =>
	model.book_name) @Html.ValidationMessageFor(model => model.book_name)
</div>

<br />

<div>
	@Html.LabelFor(model => model.author) @Html.TextBoxFor(model => model.author)
	@Html.ValidationMessageFor(model => model.author)
</div>

<br />

<div>
	@Html.LabelFor(model => model.price) @Html.TextBoxFor(model => model.price)
	@Html.ValidationMessageFor(model => model.price)
</div>

<br />

<input type="submit" value="Insert" />

@Html.ActionLink("Back", "Index") }
```

## 10. Edit.cshtml

```html
@model BookManagement.Models.Book @{ ViewBag.Title = "Update Book"; }

<h2>Update Book</h2>

@using (Html.BeginForm()) { @Html.HiddenFor(model => model.book_id)
@Html.ValidationSummary(true)

<div>
	@Html.LabelFor(model => model.book_name) @Html.TextBoxFor(model =>
	model.book_name) @Html.ValidationMessageFor(model => model.book_name)
</div>

<br />

<div>
	@Html.LabelFor(model => model.author) @Html.TextBoxFor(model => model.author)
	@Html.ValidationMessageFor(model => model.author)
</div>

<br />

<div>
	@Html.LabelFor(model => model.price) @Html.TextBoxFor(model => model.price)
	@Html.ValidationMessageFor(model => model.price)
</div>

<br />

<input type="submit" value="Update" />

@Html.ActionLink("Back", "Index") }
```

## 11. Details.cshtml

```html
@model BookManagement.Models.Book @{ ViewBag.Title = "Book Details"; }

<h2>Book Details</h2>

<table border="1">
	<tr>
		<td>Book ID</td>
		<td>@Model.book_id</td>
	</tr>

	<tr>
		<td>Book Name</td>
		<td>@Model.book_name</td>
	</tr>

	<tr>
		<td>Author</td>
		<td>@Model.author</td>
	</tr>

	<tr>
		<td>Price</td>
		<td>@Model.price</td>
	</tr>
</table>

<br />

@Html.ActionLink("Edit", "Edit", new { id = Model.book_id }) |
@Html.ActionLink("Back", "Index")
```

## 12. Delete Action

```csharp
public ActionResult Delete(int id)
{
    using (SqlConnection con = new SqlConnection(cs))
    {
        string query = "DELETE FROM book_info WHERE book_id=@book_id";

        SqlCommand cmd = new SqlCommand(query, con);

        cmd.Parameters.AddWithValue("@book_id", id);

        con.Open();
        cmd.ExecuteNonQuery();
    }

    return RedirectToAction("Index");
}
```

## 13. Search Action

```csharp
public ActionResult Search(string search)
{
    List<Book> books = new List<Book>();

    using (SqlConnection con = new SqlConnection(cs))
    {
        string query = @"SELECT * FROM book_info
                         WHERE book_name LIKE @search
                         OR author LIKE @search";

        SqlCommand cmd = new SqlCommand(query, con);

        cmd.Parameters.AddWithValue("@search", "%" + search + "%");

        con.Open();

        SqlDataReader dr = cmd.ExecuteReader();

        while (dr.Read())
        {
            books.Add(new Book
            {
                book_id = Convert.ToInt32(dr["book_id"]),
                book_name = dr["book_name"].ToString(),
                author = dr["author"].ToString(),
                price = Convert.ToDecimal(dr["price"])
            });
        }
    }

    return View("Index", books);
}
```

## 14. Required Namespace for Search

```csharp
using System.Collections.Generic;
```

## 15. MVC Operations

```text
Insert
View
Search
Update
Delete
```

## 16. MVC Architecture

```text
Model
   ↓
Database

Controller
   ↓
Model
   ↓
View

View
   ↓
User
```
