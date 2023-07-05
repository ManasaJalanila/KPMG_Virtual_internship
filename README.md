# KPMG_Virtual_internship
KPMG virtual internship repo contains the internship elements that i had learned in the internship. To explore--
This internship contains 3 stages:
1. Communicating to the client -Sprocket central pty limited, the company that is incolved in the sales of bikes and cycling accessories to its customers.
   The marketing team of the client's company wants to know the issues related to data and mitigate them for which
   i have to generate a mail to the client which includes all the data quality issues related to accuracy, relevance,incompleteness,consistency,validity etc
   based on dataset diven by client which are customer demographic, customer address,Ttransactions.
   This file is named as " KPMG TASK1.docx".
    
2. In this second task, i had to create a presentation that involves  the introduction, Agenda, data exploration , data modelling and transforming,
    data visualization, Analyze and interpretation to target the desired audience. For the new list of 1000 customers has to be targeted in order to increase the sales
   -is the primary element to solve.
   Attention to Details:
    I used Recency, Fluency, Monetary (RFM) Analysis to deal with the data. Here is the description of  how data quality issues are dealt within Power Query
   and Power Pivot for transformation and modelling the data.
   -> Used first row as headers.
   -> Removed Duplicates,Null values in the dataset.
          DistinctColumn = DISTINCT(TableName[ColumnName])
   ->Feature Engineering: Created calculated columns such as Age based on DOB in customer demographic, Profit (list_price- standard_cost) in transactions.

    Age:
          Age = DATEDIFF (  Table [DateOfBirthColumn],  TODAY (), YEAR )
   Profit: 
          List_price-Standard_cost 

   -> Product_first_sold_date: irrelevant data was given where i changed its datatype to date.
   ->Deceased_indicator in demographic of customers, should always be "NO".
   ->Extract Columns specifically from date_column to month

    Month: 
                  Month = VALUE (FORMAT (Table [Date Column], "MM")) 
                   Or 
                  Month = FORMAT (Table [Date Column], "MMM") 

    -> Merging the models using primary key using following DAX:
       Merge Columns of multiple datasets as a single table:
        Merged Column = LOOKUPVALUE (Table2[Column Name], Table2[CustomerID], Table1[CustomerID])
        Vice Versa of lookupvalue elements in Excel.  
        Columns used to merge a Customer_demographic table and customer address to the Transactional data/table: gender, past_3_years_bike _related purchases,
        age, job_title, job_industry_category, wealth_segment, owns car, postcode, state,property_valuation,Recency,Comparison_Date is max(data_column)which is last date.
       Transactions=lookupvalue(customer_demographic[gender],customer_demographic[customerid], Transactions [CustomerId])   
        and  
        Transactions=lookupvalue(Customer address [Postcode], customer address[customer_id], Transactions [Customer_id]) so on.
    Additionally, Learnt how to write more than one DAX Expressions at a time either in formula bar of modeling tab or data/table view.
   ->Recency: Tells how recently the customer purchased the product.
   -> Frequency: Tells how many times/often  the customer  purchases the product.
   ->Monetary: Tells how much does the customer spend while purchasing a product.
   Recency: As i have single column of date, i use
                             Recency = DATEDIFF(Transactions[DateColumn], MAX(Transactions[DateColumn]), DAY)
   In excel: Comparison_date-item of date_column
   Creating Tables /Measures/Columns for Analysis:
    Customer_id,MinRecency,ProductIDCount, TotalProfit, R_Score,F_Score,M_Score,RFM value,CustomerSegmentation
   
   Calculation  Measures:
   MinRecency = CALCULATE(MIN(Transactions[Recency]), ALLEXCEPT(Transactions, Transactions[CustomerID]))
   ProductIDCount = CALCULATE(COUNTROWS(Transactions), ALLEXCEPT(Transactions, Transactions[CustomerID]))
   TotalProfit=CALCULATE (SUM(Transactions[Profit]), ALLEXCEPT(Transactions, Transactions[CustomerID]))
 
  Below all used CARD visual ot store its respective value,
  R_Score( based on  MinRecency) :  min: used card visual to store the value
                                                                max: used card visual to store the value
                                                                 first_quartile= PERCENTILE.INC(Transactions[MinRecency], 0.25) 
                                                                second_quartile(median)=PERCENTILE.INC(Transactions[MinRecency], 0.50) 
                                                                 third_quartile=PERCENTILE.INC(Transactions[MinRecency], 0.75) 
                                                               
   F_Score(based on ProductIDCount ): min: used card visual to store the value
                                                                      max: used card visual to store the value
                                                                    first_quartile=PERCENTILE.INC(Transactions[ ProductIDCount], 0.25)
                                                                    second_quartile(median)=PERCENTILE.INC(Transactions[ ProductIDCount], 0.50)
                                                                    third_quartile=PERCENTILE.INC(Transactions[ ProductIDCount], 0.75) 
                                                               
   
   M_Score(based on  TotalProfit):     min: used card visual to store the value
                                                                max: used card visual to store the value
                                                                first_quartile=PERCENTILE.INC(Transactions[ TotalProfit ], 0.25) 
                                                                second_quartile(median)=PERCENTILE.INC(Transactions[ TotalProfit ], 0.50) 
                                                                 third_quartile=PERCENTILE.INC(Transactions[ TotalProfit ], 0.75) 
                                                               
   RFM value:  (100*R_Score)+(10*F_Score)+M_Score
                           min: used card visual to store the value
                           max: used card visual to store the value
                           first_quartile= PERCENTILE.INC('Transactions[RFM value], 0.25)
                          second_quartile(median)=PERCENTILE.INC('Transactions[RFM value], 0.50)
                          third_quartile=PERCENTILE.INC('Transactions[RFM value], 0.75)
    
                                                                 
   
   
      
