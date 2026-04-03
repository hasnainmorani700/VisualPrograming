# 🎯 C# ADO.NET & LINQ - EXAM CHEATSHEET

> **CRUD Windows Form Application | Entity Framework | LINQ to SQL**

---

## 📌 PART 1: DATABASE SETUP (SQL Server)

### Step 1: Create Database
```sql
CREATE DATABASE CollegeDB;
USE CollegeDB;
```

### Step 2: Create Tables

#### Students Table
```sql
CREATE TABLE Students
(
    ID INT PRIMARY KEY IDENTITY(1,1),
    Name VARCHAR(50),
    Age INT,
    Course VARCHAR(50)
);
```

#### Multiple Tables (For JOIN/EDM)
```sql
-- Students Table
CREATE TABLE Students
(
    StudentID INT PRIMARY KEY IDENTITY(1,1),
    Name VARCHAR(50),
    Age INT
);

-- Courses Table
CREATE TABLE Courses
(
    CourseID INT PRIMARY KEY IDENTITY(1,1),
    CourseName VARCHAR(50)
);

-- Enrollments Table (Relation)
CREATE TABLE Enrollments
(
    EnrollID INT PRIMARY KEY IDENTITY(1,1),
    StudentID INT,
    CourseID INT
);
```

### Step 3: Insert Sample Data
```sql
INSERT INTO Students VALUES ('Ali',22);
INSERT INTO Students VALUES ('Ahmed',23);

INSERT INTO Courses VALUES ('BSCS');
INSERT INTO Courses VALUES ('BSIT');

INSERT INTO Enrollments VALUES (1,1);
INSERT INTO Enrollments VALUES (2,2);
```

### Step 4: Verify Data
```sql
SELECT * FROM Students;
```

---

## 📌 PART 2: ADO.NET CRUD OPERATIONS

### Import Required Libraries
```csharp
using System.Data;
using System.Data.SqlClient;
```

### Create Database Connection
```csharp
SqlConnection con = new SqlConnection("Data Source=.;Initial Catalog=CollegeDB;Integrated Security=True");
```

### Load Data into DataGridView
```csharp
void LoadData()
{
    con.Open();
    SqlDataAdapter da = new SqlDataAdapter("SELECT * FROM Students", con);
    DataTable dt = new DataTable();
    da.Fill(dt);
    dataGridView1.DataSource = dt;
    con.Close();
}
```

### Call on Form Load
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    LoadData();
}
```

---

### ✅ INSERT (Save Button)
```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "INSERT INTO Students(Name,Age,Course) VALUES(@Name,@Age,@Course)";
    SqlCommand cmd = new SqlCommand(query, con);
    
    cmd.Parameters.AddWithValue("@Name", txtName.Text);
    cmd.Parameters.AddWithValue("@Age", txtAge.Text);
    cmd.Parameters.AddWithValue("@Course", txtCourse.Text);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Inserted Successfully");
    con.Close();
    LoadData();
}
```

### ✅ UPDATE (Update Button)
```csharp
private void btnUpdate_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "UPDATE Students SET Name=@Name, Age=@Age, Course=@Course WHERE ID=@ID";
    SqlCommand cmd = new SqlCommand(query, con);
    
    cmd.Parameters.AddWithValue("@ID", dataGridView1.CurrentRow.Cells[0].Value);
    cmd.Parameters.AddWithValue("@Name", txtName.Text);
    cmd.Parameters.AddWithValue("@Age", txtAge.Text);
    cmd.Parameters.AddWithValue("@Course", txtCourse.Text);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Updated Successfully");
    con.Close();
    LoadData();
}
```

### ✅ DELETE (Delete Button)
```csharp
private void btnDelete_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "DELETE FROM Students WHERE ID=@ID";
    SqlCommand cmd = new SqlCommand(query, con);
    
    cmd.Parameters.AddWithValue("@ID", dataGridView1.CurrentRow.Cells[0].Value);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Deleted Successfully");
    con.Close();
    LoadData();
}
```

### ✅ Load Selected Row into TextBoxes
```csharp
private void dataGridView1_CellClick(object sender, DataGridViewCellEventArgs e)
{
    txtName.Text = dataGridView1.Rows[e.RowIndex].Cells[1].Value.ToString();
    txtAge.Text = dataGridView1.Rows[e.RowIndex].Cells[2].Value.ToString();
    txtCourse.Text = dataGridView1.Rows[e.RowIndex].Cells[3].Value.ToString();
}
```

---

## 📌 PART 3: ENTITY DATA MODEL (EDM)

### What is EDM? (Exam Definition) ⭐
> **Entity Data Model (EDM)** is a conceptual model used by Entity Framework that represents database tables as objects and classes in C#.

### Steps to Create EDM:
1. Right Click Project → **Add** → **New Item**
2. Select **ADO.NET Entity Data Model**
3. Name → `CollegeModel.edmx`
4. Choose **EF Designer from Database**
5. Select SQL Server Connection
6. Select Tables: **Students, Courses, Enrollments**
7. Finish → Visual Studio auto-generates classes

### Auto-Generated Classes:
- `Student`
- `Course`
- `Enrollment`

### Create Database Context Object
```csharp
CollegeDBEntities db = new CollegeDBEntities();
```

---

## 📌 PART 4: LINQ TO SQL

### What is LINQ to SQL? (Exam Definition) ⭐
> **LINQ to SQL** allows developers to query relational databases using C# language syntax instead of SQL queries.

### Basic SELECT Query
```csharp
var students = from s in db.Students
               select s;

dataGridView1.DataSource = students.ToList();
```

### ✅ INSERT using LINQ
```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    CollegeDBEntities db = new CollegeDBEntities();
    
    Student s = new Student();
    s.Name = txtName.Text;
    s.Age = Convert.ToInt32(txtAge.Text);
    
    db.Students.Add(s);
    db.SaveChanges();
    
    dataGridView1.DataSource = db.Students.ToList();
}
```

### ✅ UPDATE using LINQ
```csharp
private void btnUpdate_Click(object sender, EventArgs e)
{
    CollegeDBEntities db = new CollegeDBEntities();
    
    int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);
    Student s = db.Students.Find(id);
    
    s.Name = txtName.Text;
    s.Age = Convert.ToInt32(txtAge.Text);
    
    db.SaveChanges();
    
    dataGridView1.DataSource = db.Students.ToList();
}
```

### ✅ DELETE using LINQ
```csharp
private void btnDelete_Click(object sender, EventArgs e)
{
    CollegeDBEntities db = new CollegeDBEntities();
    
    int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);
    Student s = db.Students.Find(id);
    
    db.Students.Remove(s);
    db.SaveChanges();
    
    dataGridView1.DataSource = db.Students.ToList();
}
```

---

## 📌 PART 5: DATA BINDING

### What is Data Binding? (Exam Definition) ⭐
> **Data Binding** connects UI controls like DataGridView with database data automatically.

### Example
```csharp
dataGridView1.DataSource = db.Students.ToList();
```

### Load Data on Form Load
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    CollegeDBEntities db = new CollegeDBEntities();
    dataGridView1.DataSource = db.Students.ToList();
}
```

---

## 📌 PART 6: MULTIPLE TABLES (JOIN)

### LINQ JOIN Query
```csharp
var data = from s in db.Students
           join e in db.Enrollments on s.StudentID equals e.StudentID
           join c in db.Courses on e.CourseID equals c.CourseID
           select new
           {
               s.Name,
               s.Age,
               c.CourseName
           };

dataGridView1.DataSource = data.ToList();
```

### Output Example:
| Name | Age | Course |
|------|-----|--------|
| Ali | 22 | BSCS |
| Ahmed | 23 | BSIT |

---

## 📌 PART 7: FORM CONTROLS SETUP

### Controls Required:
| Control | Name | Text |
|---------|------|------|
| Label | label1 | Name |
| Label | label2 | Age |
| Label | label3 | Course |
| TextBox | txtName | - |
| TextBox | txtAge | - |
| TextBox | txtCourse | - |
| Button | btnSave | Save |
| Button | btnUpdate | Update |
| Button | btnDelete | Delete |
| DataGridView | dataGridView1 | - |

### Form Layout:
```
Name      [ txtName ]
Age       [ txtAge ]
Course    [ txtCourse ]

[Save]   [Update]   [Delete]

--------------------------------
|        DataGridView          |
--------------------------------
```

---

## 📌 PART 8: COMPLETE FLOW DIAGRAM

### ADO.NET Flow:
```
Create Database
      ↓
Create Table
      ↓
Design Windows Form
      ↓
Connect Database using ADO.NET
      ↓
Save Button → Insert Data
      ↓
Load Data → DataGridView
      ↓
Update Button → Update Data
      ↓
Delete Button → Delete Data
```

### LINQ/Entity Framework Flow:
```
SQL Server Database
        ↓
Entity Data Model (.edmx)
        ↓
C# Classes Generated
        ↓
LINQ Queries
        ↓
Data Binding
        ↓
DataGridView Display
```

---

## 📌 PART 9: KEY DIFFERENCES (Exam Important!) ⭐

| Feature | ADO.NET | LINQ to Entities |
|---------|---------|------------------|
| Query Language | SQL strings | C# syntax |
| Connection | SqlConnection | Entity Context |
| Command | SqlCommand | LINQ queries |
| Data Access | Manual mapping | Auto object mapping |
| Type Safety | No | Yes |
| Error Prone | More | Less |

---

## 📌 PART 10: IMPORTANT METHODS QUICK REFERENCE

### ADO.NET Methods:
| Method | Purpose |
|--------|---------|
| `con.Open()` | Open database connection |
| `con.Close()` | Close database connection |
| `cmd.ExecuteNonQuery()` | Execute INSERT/UPDATE/DELETE |
| `SqlDataAdapter` | Fill DataTable with data |
| `da.Fill(dt)` | Load data into DataTable |

### LINQ/Entity Methods:
| Method | Purpose |
|--------|---------|
| `db.Students.ToList()` | Get all records |
| `db.Students.Find(id)` | Get single record by ID |
| `db.Students.Add(obj)` | Add new record |
| `db.Students.Remove(obj)` | Delete record |
| `db.SaveChanges()` | Commit changes to database |

---

## 📌 PART 11: QUICK EXAM ANSWERS

### Q: What is ADO.NET?
**A:** ADO.NET is a data access technology in .NET Framework that enables applications to connect to data sources like SQL Server. It uses classes like `SqlConnection`, `SqlCommand`, and `SqlDataAdapter`.

### Q: What is Entity Framework?
**A:** Entity Framework is an ORM (Object-Relational Mapping) framework that allows developers to work with databases using C# objects instead of writing SQL queries.

### Q: What is the purpose of `SaveChanges()`?
**A:** `SaveChanges()` commits all pending changes (insert, update, delete) made to the Entity Framework context to the database.

### Q: What is DataGridView?
**A:** DataGridView is a Windows Forms control used to display and edit data in a tabular format. It can be bound to various data sources.

### Q: Why use Parameters in SQL queries?
**A:** Parameters prevent SQL injection attacks and handle data type conversion automatically. Example: `@Name`, `@Age`

### Q: What is `ExecuteNonQuery()` used for?
**A:** `ExecuteNonQuery()` executes SQL commands that don't return rows (INSERT, UPDATE, DELETE) and returns the number of affected rows.

---

## 📌 PART 12: COMMON ERRORS & SOLUTIONS

| Error | Solution |
|-------|----------|
| Connection not open | Call `con.Open()` before executing command |
| Invalid column name | Check column names match database |
| Null reference exception | Check if row is selected before accessing |
| Conversion error | Use `Convert.ToInt32()` for numeric fields |
| Data not refreshing | Call `LoadData()` after CRUD operations |

---

## ✅ FINAL REVISION CHECKLIST

- [ ] SQL CREATE DATABASE & TABLE syntax
- [ ] ADO.NET Connection String format
- [ ] INSERT query with Parameters
- [ ] UPDATE query with WHERE clause
- [ ] DELETE query with WHERE clause
- [ ] LoadData() function using SqlDataAdapter
- [ ] DataGridView CellClick event
- [ ] EDM creation steps (.edmx file)
- [ ] LINQ SELECT query syntax
- [ ] LINQ INSERT/UPDATE/DELETE using Entity
- [ ] Data Binding to DataGridView
- [ ] LINQ JOIN for multiple tables
- [ ] Difference between ADO.NET and LINQ
- [ ] `SaveChanges()` purpose
- [ ] `ExecuteNonQuery()` purpose

---

## 🎯 EXAM TIPS

1. **Write SQL queries FIRST** when asked about database setup
2. **Show connection string** format clearly
3. **Always use Parameters** in SQL queries (security)
4. **Call LoadData()** after every CRUD operation
5. **Mention `SaveChanges()`** in LINQ operations
6. **Draw flow diagrams** for full marks
7. **Write definitions** for EDM, LINQ, Data Binding

---

> **You've got this! Focus on CRUD operations, LINQ syntax, and definitions. Good luck! 🚀**
