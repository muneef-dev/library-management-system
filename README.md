# Library Member Management System

This project is a Java application designed to manage member information for a library.

## Classes

The project consists of the following classes:

* **Crud:** Handles database queries, including inserts, updates, deletes, and selects.
* **DbConnection:** Establishes and manages the database connection.
* **MemberManagementForm:** Provides the graphical user interface for interacting with member data.

## Functionality

The application supports the following functionalities:

* **Adding new members:**
* **Searching for members by ID:**
* **Viewing member details in a table:**
* **Updating member information:**
* **Deleting members:**

## Code Snippets

***Here is some shorted class***
```java
public class Crud {
    public static <T> T execute(String sql, Object... params) throws SQLException, ClassNotFoundException {
        // ... (code for executing SQL queries)
    }
}

public class DbConnection {
    private static DbConnection dbConnection;
    private final Connection connection;

    // ... (code for managing database connection)
}

public class MemberManagementForm extends javax.swing.JFrame {
    private void btnSaveActionPerformed(java.awt.event.ActionEvent evt) {
        // ... (code for saving member data)
    }

    private void btnSearchActionPerformed(java.awt.event.ActionEvent evt) {
        // ... (code for searching for a member)
    }

    // ... (code for other form actions)
}


