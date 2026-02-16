# TCL-CTE-Fun-Store-TRIGGERS

-- // TRIGGERS
use largedata1;

show triggers in largedata1;

-- Create Employee Table
CREATE TABLE employee (
    empID INT PRIMARY KEY,
    empName VARCHAR(100),
    empAge INT,
    gender VARCHAR(20),
    empSalary INT
);


CREATE TRIGGER before_salary_update
BEFORE INSERT 
ON employee
FOR EACH ROW
    SET NEW.empsalary = NEW.empsalary + 5000;

INSERT INTO employee (empID, empName, empAge, gender, empSalary)
VALUES (102, 'Yash Doe', 30, 'Male', 90000);

select * from employee;

-----------------------------------------------------------------------------------------------------------
drop table employee;

use largedata1;

-- Create Employee Table
CREATE TABLE employee (
    empID INT PRIMARY KEY,
    empName VARCHAR(100),
    empAge INT,
    gender VARCHAR(20),
    empSalary INT
);

-- -- Create Employee Log Table

CREATE TABLE employee_log (
    logID INT AUTO_INCREMENT PRIMARY KEY,
    empID INT,
    empName VARCHAR(100),
    empSalary INT,
    logTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


-- Delimiter to support compound trigger body
DELIMITER $$

CREATE TRIGGER after_employee_insert
AFTER INSERT
ON employee
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (empID, empName, empSalary)
    VALUES (NEW.empID, NEW.empName, NEW.empSalary);
END;
$$

-- -- Reset Delimiter
DELIMITER ;

INSERT INTO employee (empID, empName, empAge, gender, empSalary)
VALUES (102, 'John Doe', 30, 'Male', 50000);

select * from employee_log;

	
