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


## Here is our members
* M.M.M MUNEEF - KEG/IT/2021/F/0102 [Muneef README.md](https://github.com/muneef-dev/lms.git)
* M.H.F HAFSA - KEG/IT/2021/F/0074 - [Hafsa README.md](https://github.com/fathimahafsa21/LMS.git)
* M.A.F HAKEEMA - KEG/IT/2021/F/0052 - [Hakeema README.md](https://github.com/fathimahakeema/LMS.git)



## My Contributions to the Library Member Management System
As the group leader for this project, I played a significant role in guiding the development process and ensuring the overall quality of the codebase. Here's a breakdown of my specific contributions:


* ### Leadership and Code Review:
Provided leadership and direction to the team members, ensuring consistent code style and adherence to best practices.
Conducted code reviews, identifying and addressing potential issues to maintain code quality and functionality.
 Ex : DbConnection class enhancements


* ### As a developer
Performed read and update the crud function, as you can see below.


## Description of function

`Crud class` helps to execute SQL queries safely by handling parameter binding and query execution, making database interactions easier and more strong.

`DbConnection class` ensures that our application has a single, reusable connection to the MySQL database, making it easy to perform database operations without having to repeatedly establish connections.

`loadMembersIntoTable()` function fetches member information from the database and displays it in a table format on the screen through the GUI

`update()` function updates the details of a member in the database, shows a success message, resets the form, and refreshes the displayed member list.

When press the `Search button`:

* It tries to find a member in the database using their ID.
* If found, it shows the member's details in the form and prepares the form for updating.
* If not found, it lets know that the member was not found.



## Code Snippets

***Here is some class and functions***
```java

public class DbConnection {
    private static DbConnection dbConnection;
    private final Connection connection;

    public DbConnection() throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.cj.jdbc.Driver");
        connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/library","root","1234");
    }

    public static DbConnection getInstance() throws SQLException, ClassNotFoundException {
        return (dbConnection == null) ? dbConnection = new DbConnection() : dbConnection;
    }

    public Connection getConnection() {
        return connection;
    }
}


public class Crud {
     public static <T> T execute(String sql, Object...params) throws SQLException, ClassNotFoundException {
         // here var args use panneera
        PreparedStatement preparedStatement = DbConnection.getInstance().getConnection().prepareStatement(sql);
        for (int i = 0; i < params.length; i++){
            preparedStatement.setObject((i+1),params[i]);
        }
        if (sql.startsWith("SELECT")){
            return (T) preparedStatement.executeQuery();
        }
        return (T)(Boolean)(preparedStatement.executeUpdate()>0);
    }
}

    private void btnSearchActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnSearchActionPerformed
        try {
            ResultSet resultSet = Crud.execute("SELECT * FROM member WHERE member_id=?", txtId.getText());
            if (resultSet.next()) {
                txtMemberId.setText(resultSet.getString(1));
                txtName.setText(resultSet.getString(2));
                txtEmail.setText(resultSet.getString(3));
                txtPhoneNumber.setText(resultSet.getString(4));
                txtAddress.setText(resultSet.getString(5));

                JOptionPane.showMessageDialog(this, "Member found!");
                txtId.setText("");
                txtMemberId.setEditable(false);

                for (ActionListener al : btnSave.getActionListeners()) {
                    btnSave.removeActionListener(al);
                }

                btnSave.setText("Update");
                btnSave.addActionListener(new ActionListener() {
                    @Override
                    public void actionPerformed(ActionEvent e) {
                        try {
                            update();
                            txtMemberId.setEditable(true);
                        } catch (SQLException | ClassNotFoundException ex) {
                            JOptionPane.showMessageDialog(MemberManagementForm.this, "Error updating member. Please try again.", "Error", JOptionPane.ERROR_MESSAGE);
                            Logger.getLogger(MemberManagementForm.class.getName()).log(Level.SEVERE, null, ex);
                        }
                    }
                });

            } else {
                JOptionPane.showMessageDialog(this, "No member found with the given ID.", "Not Found", JOptionPane.INFORMATION_MESSAGE);
            }
        } catch (SQLException | ClassNotFoundException ex) {
            JOptionPane.showMessageDialog(this, "Error searching for member. Please try again.", "Error", JOptionPane.ERROR_MESSAGE);
            Logger.getLogger(MemberManagementForm.class.getName()).log(Level.SEVERE, null, ex);
        }
    }//GEN-LAST:event_btnSearchActionPerformed

    private void update() throws SQLException, ClassNotFoundException {
        Crud.execute("UPDATE member SET name = ?, email = ?, phone_number = ?, address = ? WHERE member_id = ?",
                txtName.getText(), txtEmail.getText(), txtPhoneNumber.getText(), txtAddress.getText(), txtMemberId.getText());
        JOptionPane.showMessageDialog(this, "Member Updated successfully!");
        btnSave.setText("Save");
        clearForm();
        loadMembersIntoTable();
    }

    private void loadMembersIntoTable() {
        try {
            ResultSet resultSet = Crud.execute("SELECT * FROM member");

            DefaultTableModel model = new DefaultTableModel();
            model.addColumn("Member Id");
            model.addColumn("Name");
            model.addColumn("Email");
            model.addColumn("Phone Number");
            model.addColumn("Address");

            while (resultSet.next()) {
                model.addRow(new Object[]{resultSet.getString(1), resultSet.getString(2), resultSet.getString(3), resultSet.getString(4), resultSet.getString(5)});
            }

            tblMember.setModel(model);
        } catch (SQLException | ClassNotFoundException ex) {
            Logger.getLogger(MemberManagementForm.class.getName()).log(Level.SEVERE, null, ex);
        }
    }


