# MySQL-connection-Python
practice using MySQL connection to connect Python and MySQL
This practice is about how to connect Python and MySQL using mysql-connector-Python.And how to use connection pool
Using Jupyter Notebook
Follow steps:
1. install the connector
   
       !pip install mysql-connector-python
3. In the code import mysql.connector/Python and set an alias
   
       import mysql.connector as connector
4. Establish connection between Python and MySQL database via connector API. Use the cridential to connect to your database. If needed we can add host="hostname" in the cridentials.
   
       connection=connector.connect(database="databasename",user="username",password="password)
5. Create cursor object to communicate with entire MySQL database
   
       cursor=connection.cursor()
6. Use cursor to execute SQL queries in string object, such as
    
       cursor.execute("CREATE DATABASE my_db")   
   for multiple lines use:
   
       cursor.execute("""   
       SELECT * FROM tb_name   
       GROUP BY   
       WHERE   
       ORDER BY   
       """)
   
   No need to commit for SELECT query.
   
8. For update/delete need commit the change once the query is executed. For example:
    
        ursor.execute("""   
        NSERT INTO Orders (OrderID, TableNo, MenuID, BookingID, Quantity, BillAmount)  
        VALUES  
        (1, 12, 1, 1, 2, 86);""")  
        connection.commit()
  
8. To fetch the result from cursor object we need to use:

        colns = cursor.columns_names    
        print(colns) #print table columns name    
        results = cursor.fetchall() #fetch all query results in a list. here we can use fetchone() or fetchmany(size = 2) for fetch only 1 or 2 query results    
        for result in results:    
          print(result) # use FOR loop to print the query results
      
9. Always remember to close connection and cursor when task is completed
    
       if connection.is_connected():   
         cursor.close()       
         connection.close()
10. To Implete and query stored procedure is almost the same as basic sql queries but some different.for example:
    
        peakHour_proc_query="""    
        CREATE PROCEDURE PeakHours()    
        BEGIN    
        SELECT HOUR(BookingSlot) as Hour,    
          COUNT(HOUR(BookingSlot)) as n_bookings,    
        FROM Bookings    
        GROUP BY Hour    
        ORDER BY n_bookings DESC;    
        END
        """    
        cursor.execute(peakHour_proc_query)    
        cursor.callproc("PeakHours") # after cursor executed the create procedure query we need to use callproc() to call the procedure    
        results=next(cursor.stored_results()) # use next(cursor.stored_results() to get the query results    
        dataset=results.fetchall()    
        for column_id in cursor.stored_results(): # get the column name we need to use    
            columns=[column[0] for column in column_id.discription]    
        print(columns)
        for data in dataset:    
            print(data)
11. To implement connection pool we need to import mysql.connection.pooling.mysqlconnectionpool
    
        from mysql.connection.pooling import MySQLConnectionPool    
        from mysql.connector import Error
12. To create a connection pool we need to use mySQLConnectionPool()
    
        pool = MySQLConnectionPool(pool_name = "pool_a",pool_size = 2,**dbconfig) # the **dbconfig is to get the database configuration dbconfig = {"database":"little_lemon_db","user" : "root","password" : ""}    
        connection = pool.get_connection()    
        cursor=connection.cursor() #use cursor object to communicate with MySQL database
14. The default pool_size = 3. When the pool size is not enough we need to add connection useing pool.add_connection(), for example:
 
        connection = connector.connect(user="root", password="")          
        pool.add_connection(cnx=connection)  #Add the connection to the pool    
        new_connection = pool.get_connection()
    
Note, always remember the steps: import connector --> connection=connector.connect(user='',passward='') --> cursor=connection.cursor() --> connection.close(),cursor.close()
