# 🎨 GUI CONTROLS → CODE MAPPING

---

## 📋 FORM LAYOUT (Visual)

```
┌─────────────────────────────────────┐
│         StudentCRUD Form            │
├─────────────────────────────────────┤
│                                     │
│  Name:    [ txtName         ]       │
│  Age:     [ txtAge          ]       │
│  Course:  [ txtCourse       ]       │
│                                     │
│  [btnSave] [btnUpdate] [btnDelete]  │
│                                     │
├─────────────────────────────────────┤
│        dataGridView1                │
│  ┌─────────────────────────────┐    │
│  │ ID │ Name │ Age │ Course   │    │
│  ├─────────────────────────────┤    │
│  │ 1  │ Ali  │ 22  │ BSCS     │    │
│  │ 2  │ Ahmed│ 23  │ BSIT     │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

---

## 🎯 CONTROLS → CODE MAPPING TABLE

| Control | Name | Text | Linked Code |
|---------|------|------|-------------|
| **TextBox** | `txtName` | (empty) | `txtName.Text` |
| **TextBox** | `txtAge` | (empty) | `txtAge.Text` |
| **TextBox** | `txtCourse` | (empty) | `txtCourse.Text` |
| **Button** | `btnSave` | Save | `btnSave_Click()` |
| **Button** | `btnUpdate` | Update | `btnUpdate_Click()` |
| **Button** | `btnDelete` | Delete | `btnDelete_Click()` |
| **DataGridView** | `dataGridView1` | (empty) | `dataGridView1.DataSource` |
| **Label** | `label1` | Name | (display only) |
| **Label** | `label2` | Age | (display only) |
| **Label** | `label3` | Course | (display only) |

---

## 🔗 EVENT HANDLERS (Double-Click Creates These!)

### How to Link:
1. **Double-click Button** → Auto-generates Click event
2. **Select DataGridView** → Properties → Events → Double-click `CellClick`

---

### 1️⃣ btnSave → INSERT Code

```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "INSERT INTO Students(Name,Age,Course) VALUES(@Name,@Age,@Course)";
    SqlCommand cmd = new SqlCommand(query, con);
    
    // TextBox values → Database
    cmd.Parameters.AddWithValue("@Name", txtName.Text);
    cmd.Parameters.AddWithValue("@Age", txtAge.Text);
    cmd.Parameters.AddWithValue("@Course", txtCourse.Text);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Inserted Successfully");
    con.Close();
    LoadData();
}
```

**Memory:** "Save Button → Takes TextBox values → INSERT into DB"

---

### 2️⃣ btnUpdate → UPDATE Code

```csharp
private void btnUpdate_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "UPDATE Students SET Name=@Name, Age=@Age, Course=@Course WHERE ID=@ID";
    SqlCommand cmd = new SqlCommand(query, con);
    
    // Get ID from selected row
    cmd.Parameters.AddWithValue("@ID", dataGridView1.CurrentRow.Cells[0].Value);
    
    // TextBox values → Database
    cmd.Parameters.AddWithValue("@Name", txtName.Text);
    cmd.Parameters.AddWithValue("@Age", txtAge.Text);
    cmd.Parameters.AddWithValue("@Course", txtCourse.Text);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Updated Successfully");
    con.Close();
    LoadData();
}
```

**Memory:** "Update Button → Gets ID from grid → Updates with TextBox values"

---

### 3️⃣ btnDelete → DELETE Code

```csharp
private void btnDelete_Click(object sender, EventArgs e)
{
    con.Open();
    string query = "DELETE FROM Students WHERE ID=@ID";
    SqlCommand cmd = new SqlCommand(query, con);
    
    // Get ID from selected row
    cmd.Parameters.AddWithValue("@ID", dataGridView1.CurrentRow.Cells[0].Value);
    
    cmd.ExecuteNonQuery();
    MessageBox.Show("Data Deleted Successfully");
    con.Close();
    LoadData();
}
```

**Memory:** "Delete Button → Gets ID from grid → Deletes row"

---

### 4️⃣ dataGridView1 CellClick → Load TextBoxes

```csharp
private void dataGridView1_CellClick(object sender, DataGridViewCellEventArgs e)
{
    // Grid Cells → TextBox values
    txtName.Text = dataGridView1.Rows[e.RowIndex].Cells[1].Value.ToString();
    txtAge.Text = dataGridView1.Rows[e.RowIndex].Cells[2].Value.ToString();
    txtCourse.Text = dataGridView1.Rows[e.RowIndex].Cells[3].Value.ToString();
}
```

**Memory:** "Click grid row → Fills TextBoxes with data"

**Cell Index:**
- `Cells[0]` = ID
- `Cells[1]` = Name
- `Cells[2]` = Age
- `Cells[3]` = Course

---

### 5️⃣ Form Load → LoadData()

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    LoadData();
}

void LoadData()
{
    con.Open();
    SqlDataAdapter da = new SqlDataAdapter("SELECT * FROM Students", con);
    DataTable dt = new DataTable();
    da.Fill(dt);
    
    // Database → DataGridView
    dataGridView1.DataSource = dt;
    
    con.Close();
}
```

**Memory:** "Form loads → Calls LoadData → Fills grid with database records"

---

## 🔄 DATA FLOW DIAGRAM

```
TextBox (txtName) ──┐
                    │
TextBox (txtAge) ───┼──→ Button Click ──→ INSERT/UPDATE/DELETE ──→ Database
                    │
TextBox (txtCourse)─┘

Database ──→ LoadData() ──→ DataGridView (shows records)

DataGridView Row Click ──→ Fills TextBoxes
```

---

## 🎯 LINQ VERSION (Same Controls, Different Code)

### btnSave (LINQ):
```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    Student s = new Student();
    s.Name = txtName.Text;
    s.Age = Convert.ToInt32(txtAge.Text);
    s.Course = txtCourse.Text;
    
    db.Students.Add(s);
    db.SaveChanges();
    LoadData();
}
```

### btnUpdate (LINQ):
```csharp
private void btnUpdate_Click(object sender, EventArgs e)
{
    int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);
    Student s = db.Students.Find(id);
    
    s.Name = txtName.Text;
    s.Age = Convert.ToInt32(txtAge.Text);
    s.Course = txtCourse.Text;
    
    db.SaveChanges();
    LoadData();
}
```

### btnDelete (LINQ):
```csharp
private void btnDelete_Click(object sender, EventArgs e)
{
    int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);
    Student s = db.Students.Find(id);
    
    db.Students.Remove(s);
    db.SaveChanges();
    LoadData();
}
```

### LoadData (LINQ):
```csharp
void LoadData()
{
    dataGridView1.DataSource = db.Students.ToList();
}
```

---

## ⚡ QUICK MEMORY TRICKS

### Control Naming Pattern:
```
txt + FieldName  → TextBox (txtName, txtAge)
btn + Action     → Button (btnSave, btnUpdate)
grid + View      → DataGridView (dataGridView1)
```

### Data Flow:
```
INSERT: TextBox → Button → Database
UPDATE: TextBox + Grid → Button → Database
DELETE: Grid → Button → Database
SELECT: Database → LoadData() → Grid
CLICK: Grid → TextBox
```

### Cell Index Memory:
```
[0] = ID (hidden, used for UPDATE/DELETE)
[1] = Name (shown in txtName)
[2] = Age (shown in txtAge)
[3] = Course (shown in txtCourse)
```

---

## ✅ EXAM ANSWER TEMPLATE

**If asked: "Explain GUI controls and their events"**

```
Form Controls:
1. TextBox (txtName, txtAge, txtCourse) - User input
2. Button (btnSave, btnUpdate, btnDelete) - Trigger CRUD operations
3. DataGridView (dataGridView1) - Display records
4. Label (label1, label2, label3) - Show field names

Event Handlers:
1. btnSave_Click → INSERT query
2. btnUpdate_Click → UPDATE query
3. btnDelete_Click → DELETE query
4. dataGridView1_CellClick → Load row data into TextBoxes
5. Form1_Load → Call LoadData() on startup
```

---

> **GUI = TextBox + Button + Grid. That's it! 🎯**
