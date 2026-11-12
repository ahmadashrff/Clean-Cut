# ✂️ Clean-Cut — Barbershop Management Syste

A desktop application for managing barbershop operations, built with **C# Windows Forms** and **SQL Server LocalDB**. Clean-Cut covers everything from recording haircuts and tracking customer debts to managing expenses, deductions, and user permissions — all in one place.

---

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Database Setup](#-database-setup)
- [Default Login](#-default-login)
- [Technologies Used](#-technologies-used)
- [License](#-license)

---

## ✨ Features

| Module | Description |
|---|---|
| 🔐 **Authentication** | Secure login with SHA-256 password hashing; change password at any time |
| 👤 **User Management** | Add, edit, and delete system users with role-based permissions |
| 🔑 **Permissions System** | Fine-grained access control: Cuts, Customers, Expenses, Users, Cut Types |
| 💈 **Cut Management** | Record haircuts with barber, customer, date, paid fees, and daily earnings |
| 🧑‍🤝‍🧑 **Customer Management** | Full customer profiles with phone numbers, debt tracking, and debt payment |
| 🗂️ **Cut Types** | Define and manage the types of cuts offered by the barbershop |
| 💸 **Expense Tracking** | Log and monitor operational expenses |
| ➖ **Deductions** | Record and manage financial deductions |
| 🔎 **Filtering & Search** | Real-time grid filtering by name, ID, date, fees, and more |

---

## 🏗️ Architecture

The solution follows a classic **3-Layer Architecture**:

```
┌────────────────────────────────┐
│   Presentation Layer (UI)      │  CleanCut (WinForms .NET 4.8)
│   Windows Forms & Controls     │
└───────────────┬────────────────┘
                │
┌───────────────▼────────────────┐
│   Business Logic Layer         │  CleanCutBussiness (Class Library)
│   Domain classes & rules       │
└───────────────┬────────────────┘
                │
┌───────────────▼────────────────┐
│   Data Access Layer            │  CleanCut_DataAccess (Class Library)
│   ADO.NET + Stored Procedures  │
└───────────────┬────────────────┘
                │
┌───────────────▼────────────────┐
│   Database                     │  SQL Server LocalDB (.mdf)
└────────────────────────────────┘
```

> All database operations are performed through **Stored Procedures** for security and performance.

---

## 📁 Project Structure

```
Clean-Cut/
├── CleanCut/                        # Presentation Layer (WinForms)
│   ├── GlobalClasses/
│   │   ├── clsGlobal.cs             # App-wide state (current logged-in user)
│   │   ├── clsUtils.cs              # SHA-256 password hashing utility
│   │   └── LocalDBHelper.cs         # LocalDB connection helper
│   ├── Controls/                    # Custom UI controls
│   ├── Data/
│   │   └── CleanCut.mdf             # SQL Server LocalDB database file
│   ├── Resources/                   # App icons and images
│   ├── frmLogin.cs                  # Login screen
│   ├── frmMain.cs                   # Main dashboard (menu navigation)
│   ├── frmCut.cs / frmAddUpdateCut.cs / frmCutInfo.cs
│   ├── frmCustomers.cs / frmAddUpdateCustomer.cs / frmCustomerInfo.cs
│   ├── frmCutTypes.cs / frmAddUpdateCutType.cs / frmCutTypeInfo.cs
│   ├── frmExpenses.cs / frmAddUpdateExpense.cs / frmExpensesInfo.cs
│   ├── frmDeductions.cs / frmAddUpdateDeduction.cs / frmDeductionInfo.cs
│   ├── frmUsers.cs / frmAddUpdateUser.cs / frmUserInfo.cs
│   ├── frmChangePassword.cs
│   ├── frmPayDebt.cs
│   └── App.config                   # Connection string configuration
│
├── CleanCutBussiness/               # Business Logic Layer
│   └── clsUserData.cs               # User entity & permissions enum
│
├── CleanCut_DataAccess/             # Data Access Layer
│   ├── clsDataConnection.cs         # Shared connection string
│   ├── clsCutData.cs                # Cut CRUD via stored procedures
│   ├── clsCutDetailsData.cs
│   ├── clsCutTypeData.cs
│   ├── clsCustomerData.cs
│   ├── clsDeductionData.cs
│   ├── clsExpensesData.cs
│   └── clsUserData.cs
│
└── Setup/                           # Installer
    ├── CutSetup.msi
    └── setup.exe
```

---

## 🖥️ Prerequisites

Before running the application, install the following:

| Dependency | Download |
|---|---|
| **.NET Framework 4.8** | [Download](https://go.microsoft.com/fwlink/?linkid=2088631) |
| **Visual C++ Redistributable** | [Download](https://aka.ms/vc14/vc_redist.x64.exe) |
| **SQL Server LocalDB** | [Download](https://download.microsoft.com/download/7/c/1/7c14e92e-bdcb-4f89-b7cf-93543e7112d1/SqlLocalDB.msi) |

For **development**, you also need:
- [Visual Studio 2022](https://visualstudio.microsoft.com/) (Community or higher)
- .NET desktop development workload

---

## 🚀 Getting Started

### Option A — Run the Installer (Recommended for end users)

1. Navigate to the `Setup/` folder.
2. Run `setup.exe` or `CutSetup.msi`.
3. Follow the installation wizard.
4. Launch **Clean-Cut** from the Start Menu or Desktop shortcut.

### Option B — Build from Source (For developers)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/Clean-Cut.git
   cd Clean-Cut
   ```

2. **Open the solution** in Visual Studio:
   ```
   CleanCut/CleanCut.sln
   ```

3. **Restore NuGet packages** (Visual Studio does this automatically on build).

4. **Build the solution** (`Ctrl+Shift+B`).

5. **Run the application** (`F5`).

---

## 🗄️ Database Setup

The application uses **SQL Server LocalDB** with an embedded `.mdf` database file located at:
```
CleanCut/Data/CleanCut.mdf
```

The connection string in `App.config` is pre-configured to attach this file automatically:
```xml
<add name="CleanCutDB"
     connectionString="Data Source=(LocalDB)\MSSQLLocalDB;
     AttachDbFilename=|DataDirectory|\Data\CleanCut.mdf;
     Integrated Security=True;"
     providerName="System.Data.SqlClient" />
```

> **No manual database setup is required.** The `.mdf` file is bundled with the project and attaches automatically on first run (requires SQL Server LocalDB to be installed).

---

## 🔐 Default Login

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `admin` |

> ⚠️ **Change the default password immediately after first login** using the *Account → Change Password* menu option.

---

## 🛠️ Technologies Used

- **Language:** C# (.NET Framework 4.8)
- **UI Framework:** Windows Forms (WinForms)
- **Database:** Microsoft SQL Server LocalDB
- **Data Access:** ADO.NET with Stored Procedures
- **Security:** SHA-256 password hashing (`System.Security.Cryptography`)
- **IDE:** Visual Studio 2022

---

## 🔒 Permissions

The system uses a **bitmask-based permission model**. Each user is assigned a combination of the following permissions:

| Permission | Value | Description |
|---|---|---|
| `pAll` | -1 | Full access to everything |
| `pCut` | 1 | Manage haircut records |
| `pCustomer` | 2 | Manage customer profiles |
| `pExpenses` | 4 | Manage expenses |
| `pUsers` | 8 | Manage system users |
| `pCutTypes` | 16 | Manage cut types |

---

## 📄 License

This project is open-source. Feel free to use, modify, and distribute it.

---

> Built with ❤️ for barbershop owners who want a simple, offline desktop solution to manage their business.
