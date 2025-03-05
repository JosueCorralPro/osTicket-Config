# osTicket SLA & User Management Configuration 🛠️

## Overview
This script automates the configuration of **SLA (Service Level Agreements), users, agents, teams, and departments** in osTicket. It ensures that support tickets are handled efficiently by setting **response times, assigning permissions, and defining user roles**.

---

## 📌 Prerequisites
Before running this script, ensure:
- osTicket is **installed and configured**.
- You have **admin access** to the osTicket system.
- MySQL CLI or **phpMyAdmin** access for database modifications.

---

## 🔹 1. Configuring SLA Plans
SLA Plans define the **response and resolution time** for support tickets.

### **Manually via Web Interface:**
1. Log in as **Admin** (`http://your-server-ip/scp`).
2. Navigate to **Admin Panel** → **Manage** → **SLA Plans**.
3. Click **Add New SLA Plan** and enter details:
   - **Name:** `Standard Support`
   - **Grace Period:** `24 hours`
   - **Schedule:** `Monday - Friday (9 AM - 5 PM)`
   - **Alerts & Notices:** Enabled
4. Save changes.

### **Automating via MySQL:**
Run the following SQL query to add an SLA plan directly to the database:
```sql
INSERT INTO ost_sla (name, grace_period, schedule_id, notes, created, updated)
VALUES ('Standard Support', 24, 1, 'Default SLA for standard support tickets', NOW(), NOW());
```

---

## 🔹 2. Creating User Roles & Permissions
osTicket supports **Users, Agents (Support Staff), and Admins**.

### **Manually via Web Interface:**
1. Navigate to **Admin Panel** → **Staff** → **Roles**.
2. Click **Add New Role**:
   - **Role Name:** `Support Agent`
   - **Permissions:** Enable **Ticket Assignment, Closing, and Department Access**.
3. Save changes.

### **Automating User Role Creation via MySQL:**
```sql
INSERT INTO ost_role (name, notes, created, updated)
VALUES ('Support Agent', 'Standard support role', NOW(), NOW());
```

---

## 🔹 3. Adding Support Departments
Departments help organize ticket management by category.

### **Manually via Web Interface:**
1. Go to **Admin Panel** → **Agents** → **Departments**.
2. Click **Add New Department**:
   - **Name:** `Technical Support`
   - **Department Email:** `support@example.com`
   - **Auto-response:** Enabled
3. Save changes.

### **Automating Department Creation via MySQL:**
```sql
INSERT INTO ost_department (name, email_id, flags, created, updated)
VALUES ('Technical Support', 1, 1, NOW(), NOW());
```

---

## 🔹 4. Creating Agents & Assigning Departments
Agents are staff members who handle tickets.

### **Manually via Web Interface:**
1. Navigate to **Admin Panel** → **Staff** → **Agents**.
2. Click **Add New Agent**:
   - **Name:** `John Doe`
   - **Email:** `johndoe@example.com`
   - **Primary Department:** `Technical Support`
   - **Role:** `Support Agent`
3. Save and assign access permissions.

### **Automating Agent Creation via MySQL:**
```sql
INSERT INTO ost_staff (firstname, lastname, username, email, dept_id, role_id, isactive, created, updated)
VALUES ('John', 'Doe', 'jdoe', 'johndoe@example.com', 1, 2, 1, NOW(), NOW());
```

---

## 🔹 5. Adding Users
Users are the **end customers** who submit tickets.

### **Manually via Web Interface:**
1. Navigate to **Admin Panel** → **Users** → **Add New User**.
2. Enter user details:
   - **Full Name:** `Jane Smith`
   - **Email:** `janesmith@example.com`
   - **Organization:** Optional
3. Click **Create User**.

### **Automating User Creation via MySQL:**
```sql
INSERT INTO ost_user (name, email, created, updated)
VALUES ('Jane Smith', 'janesmith@example.com', NOW(), NOW());
```

---

## 🔹 6. Assigning SLA to Departments
Linking an SLA plan to a department ensures response deadlines.

### **Automating via MySQL:**
```sql
UPDATE ost_department SET sla_id = (SELECT id FROM ost_sla WHERE name = 'Standard Support') WHERE name = 'Technical Support';
```

---

## ✅ Final Checks
After executing the scripts, verify the settings in osTicket:
- Navigate to **Admin Panel** → **SLA Plans, Agents, Users, and Departments**.
- Confirm that all entries exist and are correctly assigned.

---

## 🎯 Conclusion
This setup ensures **efficient ticket routing, role-based access control, and timely support resolutions**. By automating user and SLA configuration, IT teams can streamline help desk operations.

📌 Next Steps:
- Configure **email fetching (IMAP/POP3)** for automated ticket creation.
- Enable **auto-assign rules** based on issue categories.
- Implement **custom ticket status and escalations**.

For additional details, refer to the [osTicket Documentation](https://docs.osticket.com/) 📖.

---

### **Author:** [Josue Corral](https://github.com/JosueCorralPro)  
**GitHub Repository:** [osTicket Setup](https://github.com/JosueCorralPro/osticket-setup)
