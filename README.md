# DonationsSQL
### Objective: 
Cleaning data in SQL to then create inferences on donations status

### Description: 

#### Find the average and sum of the donation by day, month, and year 

Code: 

-- Create donations by month, average donation and sum of donations for June

CREATE PROCEDURE DonationsbyDateCity AS BEGIN
    SELECT 
        A.city,
        DATEPART(YEAR, D.donation_date) AS Year,
        DATEPART(MONTH, D.donation_date) AS Month,
        AVG(CAST(D.donation_amount AS DECIMAL)) AS avg_donation,
        SUM(CAST(D.donation_amount AS DECIMAL)) AS sum_donation
    FROM Donation AS D
    JOIN Address AS A ON D.addressID = A.addressID
    GROUP BY 
        A.city,
        DATEPART(YEAR, D.donation_date),
        DATEPART(MONTH, D.donation_date);
END
EXEC DonationsbyDateCity;


-- Create the stored procedure and name; parameters are city and month, can be selected in execution line
-- Calculate the average and sum of donations by day, month, and year
  -- USE DATEPART FUNCTION FOR EASIER READABILITY AND TO EXTRACT COMPONENTS OF DATE COLUMN FOR TABLE

CREATE PROCEDURE totaldonationavgsum (@city NVARCHAR(100), @month INT)
AS
BEGIN
  SELECT 
    DATEPART(YEAR, d.donation_date) AS Year,
    DATEPART(MONTH, d.donation_date) AS Month,
    DATEPART(DAY, d.donation_date) AS Day,
    AVG(CAST(d.donation_amount AS decimal)) AS avg_donation,
    SUM(CAST(d.donation_amount AS decimal)) AS sum_donation
  FROM Donation d
  INNER JOIN Address a ON d.addressID = a.addressID
  WHERE 
    DATEPART(MONTH, d.donation_date) = @month AND
    a.city = @city
  GROUP BY 
    DATEPART(YEAR, d.donation_date),
    DATEPART(MONTH, d.donation_date),
    DATEPART(DAY, d.donation_date)
END
EXEC totaldonationavgsum 'Toronto', 6
EXEC totaldonationavgsum 'Vancouver', 6

#### The average and sum of the donations by postal code and City in a specific month. define the city and month as variables to allow flexibility. 


 -- Average and sum of donations by postal code and city in a specific month

 
CREATE PROCEDURE [dbo].[DonationbyCityPostalCodeDate]
    @city VARCHAR(100),
    @month INT
AS
BEGIN
    SELECT
        a.city,
        a.postal_code,
        AVG(CAST(d.donation_amount AS DECIMAL)) AS avg_donation,
        SUM(CAST(d.donation_amount AS DECIMAL)) AS sum_donation
    FROM Donation AS d
    INNER JOIN Address AS a ON d.addressID = a.addressID
    WHERE
        a.city = @city
        AND DATEPART(MONTH, d.donation_date) = @month
    GROUP BY a.city, a.postal_code;
END


EXEC DonationbyCityPostalCodeDate @city = 'Toronto', @month = 6;
EXEC DonationbyCityPostalCodeDate @city = 'Vancouver', @month = 6;







