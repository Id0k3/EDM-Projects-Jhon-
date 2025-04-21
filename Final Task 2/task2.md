#Finals Task 2. Transforming ER into Relational Tables

-- Task 1: Create the student table
create database students_db;
CREATE TABLE student (
    username VARCHAR(50) PRIMARY KEY
);

-- Task 2: Create the assignment table
CREATE TABLE assignment (
    shortname VARCHAR(50) PRIMARY KEY,
    due_date DATE NOT NULL,
    url VARCHAR(255)
);

-- Task 3: Create the submission table
CREATE TABLE submission (
    username VARCHAR(50),
    shortname VARCHAR(50),
    version INT,
    submit_date DATE NOT NULL,
    data TEXT,
    PRIMARY KEY (username, shortname, version),
    FOREIGN KEY (username) REFERENCES student(username),
    FOREIGN KEY (shortname) REFERENCES assignment(shortname)
);

-- Task 4:  Table Relationships (Foreign Key Constraints)
-- Already included in the CREATE TABLE submission statement:
-- FOREIGN KEY (username) REFERENCES student(username)
-- FOREIGN KEY (shortname) REFERENCES assignment(shortname)

