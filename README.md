# Database-Function
Codes for functions in the database
import pyodbc

def create_access_database(database_name):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    # Create a table to store user IDs and passwords
    cursor.execute('''
        CREATE TABLE Users (
            UserID AUTOINCREMENT PRIMARY KEY,
            Username VARCHAR(50) NOT NULL,
            Password VARCHAR(50) NOT NULL
        )
    ''')
    
    conn.commit()
    conn.close()
    print(f"Access database '{database_name}' created successfully.")

def add_user(database_name, username, password):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    # Insert new user into the Users table
    cursor.execute('''
        INSERT INTO Users (Username, Password)
        VALUES (?, ?)
    ''', (username, password))

    conn.commit()
    conn.close()
    print(f"User '{username}' added successfully.")

if __name__ == "__main__":
    # Name of the Access database
    database_name = "UserDatabase.accdb"
    
    # Create the Access database if it doesn't exist
    create_access_database(database_name)
    
    # Add a user to the database
    username = input("Enter username: ")
    password = input("Enter password: ")
    add_user(database_name, username, password)


###########################################################################
SQL commands for queries
SELECT [Patent Name].[Patent ID], [Patent Information].[Patent ID], [Compound Information].[Compound Name], [Solvent Information].[Compound Name]
FROM (([Compound Information] INNER JOIN [Patent Information] ON [Compound Information].[Compound Name] = [Patent Information].[Compound name]) INNER JOIN [Patent Name] ON [Compound Information].[Compound ID] = [Patent Name].[Patent ID]) INNER JOIN [Solvent Information] ON [Compound Information].[Compound Name] = [Solvent Information].[Compound Name];
###############################################################################






import pyodbc

def create_access_database(database_name):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    # Create a table to store user IDs and passwords
    cursor.execute('''
        CREATE TABLE Users (
            UserID AUTOINCREMENT PRIMARY KEY,
            Username VARCHAR(50) NOT NULL,
            Password VARCHAR(50) NOT NULL
        )
    ''')
    
    conn.commit()
    conn.close()
    print(f"Access database '{database_name}' created successfully.")

def add_user(database_name, username, password):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    # Insert new user into the Users table
    cursor.execute('''
        INSERT INTO Users (Username, Password)
        VALUES (?, ?)
    ''', (username, password))

    conn.commit()
    conn.close()
    print(f"User '{username}' added successfully.")

def list_users(database_name):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    cursor.execute('SELECT UserID, Username FROM Users')
    rows = cursor.fetchall()

    print("List of users:")
    for row in rows:
        print(f"ID: {row.UserID}, Username: {row.Username}")

    conn.close()

def update_password(database_name, username, new_password):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    cursor.execute('''
        UPDATE Users
        SET Password = ?
        WHERE Username = ?
    ''', (new_password, username))

    conn.commit()
    conn.close()
    print(f"Password for user '{username}' updated successfully.")

def delete_user(database_name, username):
    conn_str = (
        r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};'
        r'DBQ=' + database_name + ';'
    )
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()

    cursor.execute('''
        DELETE FROM Users
        WHERE Username = ?
    ''', (username,))

    conn.commit()
    conn.close()
    print(f"User '{username}' deleted successfully.")

if __name__ == "__main__":
    database_name = "UserDatabase.accdb"
    create_access_database(database_name)

    # Add some initial users
    add_user(database_name, "user1", "password1")
    add_user(database_name, "user2", "password2")
    add_user(database_name, "user3", "password3")

    # List all users
    list_users(database_name)

    # Update user password
    update_password(database_name, "user1", "new_password")

    # Delete a user
    delete_user(database_name, "user3")

    # List users after modifications
    list_users(database_name)
