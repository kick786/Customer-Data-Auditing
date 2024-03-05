-- Create a temporary table to store audit results
CREATE TABLE CustomerDataAudit (
    CustomerID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Email VARCHAR(100),
    Phone VARCHAR(20),
    AuditDate DATE,
    AuditDescription VARCHAR(255)
);

-- Insert audit results into the temporary table
INSERT INTO CustomerDataAudit (CustomerID, FirstName, LastName, Email, Phone, AuditDate, AuditDescription)
SELECT 
    CustomerID,
    FirstName,
    LastName,
    Email,
    Phone,
    GETDATE() AS AuditDate,
    CASE
        WHEN FirstName IS NULL THEN 'Missing first name'
        WHEN LastName IS NULL THEN 'Missing last name'
        WHEN Email IS NULL THEN 'Missing email'
        WHEN Phone IS NULL THEN 'Missing phone number'
        ELSE 'No issues found'
    END AS AuditDescription
FROM 
    Customers;

-- View audit results
SELECT * FROM CustomerDataAudit;
