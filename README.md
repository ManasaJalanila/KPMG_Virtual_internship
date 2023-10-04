# KPMG_Data Analytics_Virtual_Internship
The KPMG virtual internship repo contains the internship Tasks that I had learnt during the internship. To explore--
This internship contains 3 stages:
1. Communicating to the client -Sprocket Central Pty Limited, the company that is involved in the sales of bikes and cycling accessories to its customers.
   The marketing team of the client's company wants to know the issues related to data and mitigate them for which
   i have to generate a mail to the client that includes all the data quality issues related to accuracy, relevance, incompleteness, consistency, validity, etc
   based on datasets divided by client which are customer demographic, customer address, and Transactions.
   This file is named as " KPMG TASK1.docx".
    
2. In this second task, I had to create a presentation that involved  the introduction, Agenda, data exploration, data modeling and transforming,
    data visualization, Analysis, and interpretation to target the desired audience. The new list of 1000 customers has to be targeted in order to increase sales
   -is the primary element to solve.
   Attention to Details:
    I used Recency, Fluency, and Monetary (RFM) Analysis to deal with the data. Here is the description of  how data quality issues are dealt with within Power Query
   and Power Pivot for the transformation and modeling of the data.
   -> Used the first row as headers.
   -> Removed Duplicates and null values in the dataset.
          DistinctColumn = DISTINCT(TableName[ColumnName])
   ->Feature Engineering: Created calculated columns such as Age based on DOB in customer demographic and profit (list_price- standard_cost) in transactions.

    Age:
          Age = DATEDIFF (  Table [DateOfBirthColumn],  TODAY (), YEAR )
   Profit: 
          List_price-Standard_cost 

   -> Product_first_sold_date: irrelevant data was given where I changed its datatype to date.
   ->Deceased_indicator in the demographic of customers, should always be "NO".
   ->Extract Columns specifically from date_column to month

    Month: 
                  Month = VALUE (FORMAT (Table [Date Column], "MM")) 
                   Or 
                  Month = FORMAT (Table [Date Column], "MMM") 

    -> Merging the models using the primary key using the following DAX:
       Merge Columns of multiple datasets as a single table:
        Merged Column = LOOKUPVALUE (Table2[Column Name], Table2[CustomerID], Table1[CustomerID])
        Vice Versa of lookup value elements in Excel.  
        Columns used to merge a Customer_demographic table and customer address to the Transactional data/table: gender, past_3_years_bike _related purchases,
        age, job_title, job_industry_category, wealth_segment, owns car, postcode, state,property_valuation,Recency,Comparison_Date is max(data_column)which is last date.
       Transactions=lookupvalue(customer_demographic[gender],customer_demographic[customerid], Transactions [CustomerId])   
        and  
        Transactions=lookupvalue(Customer address [Postcode], customer address[customer_id], Transactions [Customer_id]) so on.
    Additionally, I learned how to write more than one DAX expression at a time either in the formula bar of the modeling tab or data/table view.
   ->Recency: Tells how recently the customer purchased the product.
   -> Frequency: Tells how many times/often  the customer  purchases the product.
   ->Monetary: Tells how much the customer spends while purchasing a product.
   Recency: As I have a single column of date, I use
                             Recency = DATEDIFF(Transactions[DateColumn], MAX(Transactions[DateColumn]), DAY)
   In Excel: Comparison_date-item of date_column
   Creating Tables /Measures/Columns for Analysis:
    Customer_id,MinRecency,ProductIDCount, TotalProfit, R_Score,F_Score,M_Score,RFM value,CustomerSegmentation
   
   Calculation  Measures:
   MinRecency = CALCULATE(MIN(Transactions[Recency]), ALL EXCEPT(Transactions, Transactions[CustomerID]))
   ProductIDCount = CALCULATE(COUNTROWS(Transactions), ALL EXCEPT(Transactions, Transactions[CustomerID]))
   TotalProfit=CALCULATE (SUM(Transactions[Profit]), ALL EXCEPT(Transactions, Transactions[CustomerID]))
 
  Below are all used CARD visuals to store their respective values,
  
  R_Score( based on  MinRecency): 4 is the latest arrival of the customer, and 1 is the earliest arrival. 
  SWITCH(TRUE(),Transactions[MinRecency]<=firstquartile && Transactions[MinRecency]>min,4,
                                 Transactions[MinRecency]>firstquartile&& Transactions[MinRecency]<=secondquartile,3,  
                                 Transactions[MinRecency]>secondquartile && Transactions[MinRecency]<=thirdquartile,2,1)
  min: used card visual to store the value
 max: used card visual to store the value 
 first_quartile= PERCENTILE.INC(Transactions[MinRecency], 0.25) 
 second_quartile(median)=PERCENTILE.INC(Transactions[MinRecency], 0.5)
 third_quartile=PERCENTILE.INC(Transactions[MinRecency], 0.75) 
                                                               
   F_Score(based on ProductIDCount ): 1 is less frequent, and 4 is more frequent.
   SWITCH(TRUE(),Transactions[ProductIDCount]<=firstquartile && Transactions[ProductIDCount]>min,1,
                                 Transactions[ProductIDCount]>firstquartile&& Transactions[ProductIDCounty]<=secondquartile,2,  
                                 Transactions[ProductIDCount]>secondquartile && Transactions[ProductIDCount]<=thirdquartile,3,4)
   min: used card visual to store the value
   max: used card visual to store the value
   irst_quartile=PERCENTILE.INC(Transactions[ ProductIDCount], 0.25)
   second_quartile(median)=PERCENTILE.INC(Transactions[ ProductIDCount],0.25)
   third_quartile=PERCENTILE.INC(Transactions[ ProductIDCount], 0.75) 
                                                               
   
   M_Score(based on  Sales/TotalProfit):  1 is spends less in purchasing product, 4Spends more in purchasing 
    SWITCH(TRUE(),Transactions[Sales/TotalProfit]<=firstquartile && Transactions[Sales/TotalProfit]>min,1,
                                 Transactions[Sales/TotalProfit]>firstquartile&& Transactions[Sales/TotalProfit]<=secondquartile,2,  
                                 Transactions[Sales/TotalProfit]>secondquartile && Transactions[Sales/TotalProfit]<=thirdquartile,3,4)
   min: used card visual to store the value
   max: used card visual to store the value
   first_quartile=PERCENTILE.INC(Transactions[ TotalProfit ], 0.25) 
   second_quartile(median)=PERCENTILE.INC(Transactions[ TotalProfit ], 0.50) 
   third_quartile=PERCENTILE.INC(Transactions[ TotalProfit ], 0.75) 
                                                               
   RFM value:  (100*R_Score)+(10*F_Score)+M_Score
   Customer Profile=SWITCH(TRUE(),Transactions[ RFM value]<=firstquartile && Transactions[ RFM value]>min,'Bronze customer',
                                 Transactions[ RFM valuet]>firstquartile&& Transactions[ RFM value]<=secondquartile,'Silver customer',  
                                 Transactions[t RFM value]>secondquartile && Transactions[ RFM value]<=thirdquartile,' Gold customer', 'Platinum customer')
   min: used card visual to store the value
   max: used card visual to store the value
   first_quartile= PERCENTILE.INC('Transactions[RFM value], 0.25)
   second_quartile(median)=PERCENTILE.INC('Transactions[RFM value], 0.50)
   third_quartile=PERCENTILE.INC('Transactions[RFM value], 0.75)
   
3. Finally, my favorite task is to create Visuals and create reports for the client's requirements.
       The report contains 4 pages of Canva,
         1. Total Profit based on Age group and wealth segment
         Measure: Agegroup=switch(true(),Transactions[Age]<=30  && Transactions[Age]>21,"21-30",
                                 Transactions[Age]>=31 && Transactions[Age]<=40,"31-40",  
                                 Transactions[Age]>=41 && Transactions[Age]<=50,"41-50",
                                 Transactions[Age]>=51 && Transactions[Age]<=60,"51-60",
                                  Transactions[Age]>=61 && Transactions[Age]<=70,"61-70",
                                  Transactions[Age]>=71 && Transactions[Age]<=80,"71-80",
                                  Transactions[Age]>=81 && Transactions[Age]<=92,"81-92")
        -> Total Profit based on Cars owned by different states by Age_group and Gender
        -> Bike-related purchases based on job industry, wealth segment, and Profits
        This report page has page navigations to Top customers followed by Brands And Products Followed by Months and Weekdays and back to the Report page 
        2. Top Customers by Age, Job industry, Wealth Segment, State
        3. Profit by brand, Product_line
        4. Profits by months and weekdays
The  Visuals used in the report are
TextBox, Buttons to navigate pages,  Back, Icons,
Clustered Column chart, Pie chart, donut chart, Line and stacked column chart, Tables, Slicers, Cards, Funnel, and Stacked Area chart.


Sincere, Thank You for pouring in Patience and Support to read my First Data Analytics Project.
For more information regarding the dataset, and reports presentations,  Don't forget to glance at the files included!!!
 

 
    
                                                                 
   
   
      
