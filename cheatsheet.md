# ⚡ 20-MIN CRAM SHEET - FULL MARKS TRICKS

---

## 🔥 TRICK 1: CRUD PATTERN (Memorize This!)

### **"O-P-E-C-L"** → Every CRUD Operation

```
O → Open connection     → con.Open()
P → Prepare query       → "INSERT/UPDATE/DELETE..."
E → Execute command     → cmd.ExecuteNonQuery()
C → Close connection    → con.Close()
L → Load data again     → LoadData()
```

---

## 🔥 TRICK 2: 3 SQL QUERIES (Write These EXACTLY)

### INSERT:
```sql
INSERT INTO Students(Name, Age, Course) VALUES(@Name, @Age, @Course)
```

### UPDATE:
```sql
UPDATE Students SET Name=@Name, Age=@Age, Course=@Course WHERE ID=@ID
```

### DELETE:
```sql
DELETE FROM Students WHERE ID=@ID
```

**Memory:** "UPDATE/DELETE always needs WHERE ID!"

---

## 🔥 TRICK 3: ADO.NET CODE (5 Lines Only!)

```csharp
con.Open();                                    // 1. Open
SqlCommand cmd = new SqlCommand(query, con);   // 2. Command
cmd.Parameters.AddWithValue("@Name", txtName.Text); // 3. Parameters
cmd.ExecuteNonQuery();                         // 4. Execute
con.Close(); LoadData();                       // 5. Close + Refresh
```

**Memory:** "Open → Command → Parameters → Execute → Close"

---

## 🔥 TRICK 4: LINQ CRUD (3 Lines Only!)

### INSERT:
```csharp
Student s = new Student();
s.Name = txtName.Text;
db.Students.Add(s); db.SaveChanges();
```

### UPDATE:
```csharp
Student s = db.Students.Find(id);
s.Name = txtName.Text;
db.SaveChanges();
```

### DELETE:
```csharp
Student s = db.Students.Find(id);
db.Students.Remove(s);
db.SaveChanges();
```

**Memory:**
- INSERT = **"Create → Add → Save"**
- UPDATE = **"Find → Fix → Save"**
- DELETE = **"Find → Remove → Save"**

---

## 🔥 TRICK 5: CONNECTION STRING

```
"Data Source=.;Initial Catalog=CollegeDB;Integrated Security=True"
```

**Memory:** **"D-C-I"**
- **D**ata Source = **.** (dot = local)
- **C**atalog = **Database name**
- **I**ntegrated Security = **True** (Windows login)

---

## 🔥 TRICK 6: LINQ JOIN (One Line!)

```csharp
from s in db.Students
join e in db.Enrollments on s.StudentID equals e.StudentID
join c in db.Courses on e.CourseID equals c.CourseID
select new { s.Name, c.CourseName };
```

**Memory:** "From → Join ON equals → Join ON equals → Select New"

**⚠️ IMPORTANT:** Use `equals` NOT `==`

---

## 🔥 TRICK 7: LOAD DATA (2 Lines!)

### ADO.NET:
```csharp
SqlDataAdapter da = new SqlDataAdapter("SELECT * FROM Students", con);
DataTable dt = new DataTable(); da.Fill(dt);
dataGridView1.DataSource = dt;
```

### LINQ:
```csharp
dataGridView1.DataSource = db.Students.ToList();
```

---

## 🔥 TRICK 8: GET SELECTED ROW DATA

```csharp
txtName.Text = dataGridView1.Rows[e.RowIndex].Cells[1].Value.ToString();
txtAge.Text = dataGridView1.Rows[e.RowIndex].Cells[2].Value.ToString();
```

**Memory:** "Cells[0]=ID, Cells[1]=Name, Cells[2]=Age"

---

## 🔥 TRICK 9: DEFINITIONS (One Line Each!)

| Term | One-Line Definition |
|------|---------------------|
| **ADO.NET** | Data access technology using SqlConnection & SqlCommand |
| **EDM** | Converts database tables into C# classes automatically |
| **LINQ** | Query database using C# syntax instead of SQL |
| **Data Binding** | Connect UI controls to database data automatically |
| **DataGridView** | Shows database records in table format |
| **Entity Framework** | ORM framework - works with objects not SQL |
| **SaveChanges()** | Commits all changes to database |
| **ExecuteNonQuery()** | Executes INSERT/UPDATE/DELETE (returns rows affected) |
| **SqlParameter** | Prevents SQL injection + handles data types |
| **SqlDataAdapter** | Fills data into DataTable (auto open/close) |

---

## 🔥 TRICK 10: ADO.NET vs LINQ (Table!)

| Point | ADO.NET | LINQ |
|-------|---------|------|
| Query | SQL strings | C# syntax |
| Safety | Runtime errors | Compile errors |
| Code | More | Less |
| Speed | Faster | Slower |
| IntelliSense | No | Yes |

---

## 🔥 TRICK 11: NAMESPACES (Write First!)

```csharp
using System.Data;
using System.Data.SqlClient;
```

**Memory:** "Data for tables, SqlClient for connection"

---

## 🔥 TRICK 12: FORM CONTROLS

| Control | Name | Purpose |
|---------|------|---------|
| TextBox | txtName | Input name |
| TextBox | txtAge | Input age |
| TextBox | txtCourse | Input course |
| Button | btnSave | Insert data |
| Button | btnUpdate | Update data |
| Button | btnDelete | Delete data |
| DataGridView | dataGridView1 | Show records |

---

## 🔥 TRICK 13: ERROR HANDLING (Add for Extra Marks!)

```csharp
try
{
    // Your CRUD code here
}
catch(Exception ex)
{
    MessageBox.Show("Error: " + ex.Message);
}
```

---

## 🔥 TRICK 14: VALIDATION (Add for Extra Marks!)

```csharp
if(txtName.Text == "")
{
    MessageBox.Show("Please fill all fields!");
    return;
}
```

---

## ⚡ FINAL 5-MIN QUICK REVISION

### MEMORIZE THESE ORDER:

1. **Connection String** → "Data Source=.;Initial Catalog=CollegeDB;Integrated Security=True"

2. **INSERT** → "VALUES(@Name, @Age, @Course)"

3. **UPDATE** → "WHERE ID=@ID"

4. **DELETE** → "WHERE ID=@ID"

5. **ADO.NET** → Open → Command → Parameters → Execute → Close

6. **LINQ INSERT** → Add → SaveChanges()

7. **LINQ UPDATE** → Find → Fix → SaveChanges()

8. **LINQ DELETE** → Find → Remove → SaveChanges()

9. **JOIN** → "on ... equals ..."

10. **Load Data** → "DataSource = ...ToList()"

---

## 🎯 EXAM HALL CHEAT CODES:

✅ Write namespaces FIRST  
✅ Write connection string  
✅ Add comments in code  
✅ Use Parameters (@Name)  
✅ Call LoadData() after CRUD  
✅ Draw flow diagram  
✅ Use table for comparison  
✅ Attempt ALL questions  

---

> **GO CRUSH IT! YOU GOT THIS! 💪🔥**
