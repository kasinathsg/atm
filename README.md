importmysql.connector
# GLOBAL VARIABLES DECLARATION 
myConnnection ="" 
cursor="" 
userName="" 
password ="" 
cid="" 
#MODULE TO CHECK MYSQL CONNECTIVITY 
defMYSQLconnectionCheck (): 
globalmyConnection
globaluserName
global password 
userName = input("\n ENTER MYSQL SERVER'S USERNAME : ") 
password = input("\n ENTER MYSQL SERVER'S PASSWORD : ") myConnection=mysql.connector.connect(host="localhost",user=userName,passwd=password , auth_plugin='mysql_native_password' ) 
ifmyConnection: 
print("\n CONGRATULATIONS ! YOUR MYSQL CONNECTION HAS BEEN ESTABLISHED !") 
cursor=myConnection.cursor() 
cursor.execute("CREATE DATABASE IF NOT EXISTS ATM") 
cursor.execute("COMMIT") 
cursor.close()
returnmyConnection
else: 
print("\nERROR ESTABLISHING MYSQL CONNECTION CHECK USERNAME AND PASSWORD !") 
#MODULE TO ESTABLISHED MYSQL CONNECTION 
defMYSQLconnection (): 
globaluserName
global password 
globalmyConnection
myConnection=mysql.connector.connect(host="localhost",user=userName,passwd=password , database="ATM" , auth_plugin='mysql_native_password' ) 
ifmyConnection: 
returnmyConnection
else: 
print("\nERROR ESTABLISHING MYSQL CONNECTION !") 
myConnection.close()
# MODULE TO CREATE NEW CUSTOMER 
defnewCustomer():
globalcid
ifmyConnection: 
cursor=myConnection.cursor() 
createTable ="""CREATE TABLE IF NOT EXISTS CUSTOMER(CID VARCHAR(10) PRIMARY KEY ,CNAME VARCHAR(30) NOT NULL ,ADDRESS VARCHAR(30)NOT NULL ,PHONE VARCHAR(12) NOT NULL) """ 
cursor.execute(createTable) 
print("\nPlease Fill All The Information Carefully !") 
cid=input("Please Enter Customer ID : ") 
cname=input("Please Enter Customer Name : ") 
address=input("Please Enter Customer Address : ") 
phone=input("Please Enter Customer Contact No. : ") 
sql='INSERT INTO CUSTOMER(cid,cname,address,phone) values(%s,%s,%s,%s)' 
values=(cid,cname,address,phone) 
cursor.execute(sql,values) 
cursor.execute("COMMIT") 
cursor.close()
print("\nNew Customer Added Successfully !") 
# MODULE TO DISPLAY CUSTOMER INFORMATION :
defdisplayAllCustomer(): 
ifmyConnection: 
cursor=myConnection.cursor() 
cursor.execute("SELECT * FROM CUSTOMER") 
data = cursor.fetchall() 
if data: 
print("\n*****DETAILS OF ALL CUSTOMER*****") 
print(data) 
else: 
print("Sorry ! No Record Found , Please Try Again ! ") 
else: 
print("\nERROR ESTABLISHING MYSQL CONNECTION !") 
# MODULE TO SEARCH A CUSTOMER 
defsearchCustomer(): 
globalcid
ifmyConnection: 
cursor=myConnection.cursor() 
cid=input("Please Enter Customer ID : ") 
sql="SELECT * FROM CUSTOMER WHERE CID = %s" 
values=(cid,) 
data=cursor.execute(sql,values) 
data = cursor.fetchall() 
if data: 
print("\n*****CUSTOMER DETAILS*****") 
print(data) 
else: 
print("Sorry ! Customer NOT Found , Please Try Again ! ") 
else: 
print("\nSomthing Went Wrong ,Please Try Again !") 
# MODULE TO OPEN A NEW ACCOUNT 
defnewAccount(): 
globalcid
ifmyConnection: 
cursor=myConnection.cursor() 
cid=input("Please Enter Customer ID : ") 
sql="SELECT * FROM CUSTOMER WHERE CID = %s" 
values=(cid,) 
data=cursor.execute(sql,values) 
data = cursor.fetchall() 
if data: 
createTable ="""CREATE TABLE IF NOT EXISTS ACCOUNT(CID VARCHAR(10),ACCOUNT_NO INT PRIMARY KEY ,ACCOUNT_TYPE VARCHAR(20) NOT NULL ,AMOUNT INT NOT NULL , PIN INT NOT NULL UNIQUE) """ 
cursor.execute(createTable) 
account_no=int(input("PLEASE ENTER THE ACCOUNT NUMBER [0-9]: "))
account_type=input("PLEASE ENTER THE ACCOUNT TYPE [ S-SAVING / C - CURRENT : ") 
amount=int(input("PLEASE ENTER THE AMOUNT TO DEPOSIT : ")) 
ATM_pin=int(input("PLEASE ENTER THE ATM PIN [ FOUR DIGITIS ONLY ] : ")) 
sql='INSERT INTO ACCOUNT (cid,account_no,account_type,amount ,pin) VALUES (%s,%s,%s,%s,%s)' 
values1=(cid,account_no,account_type,amount,ATM_pin) 
cursor.execute(sql,values1) 
cursor.execute("COMMIT") 
print("\nNew Account Opend Successfully !") 
else: 
print("Sorry ! Customer NOT Found , Please Try Again ! ") 
else: print("\nSomthing Went Wrong ,Please Try Again !") 
# MODULE TO DISPLAY ALL ACCOUNTS 
defdisplayAllAccounts(): 
ifmyConnection: 
cursor=myConnection.cursor() 
cursor.execute("SELECT * FROM ACCOUNT") 
data = cursor.fetchall() 
if data: 
print("\n*****DETAILS OF ALL CUSTOMER*****") 
print(data) 
else: 
print("Sorry ! No Account Information , Please Try Again ! ") 
else: 
print("\nERROR ESTABLISHING MYSQL CONNECTION !") 
# MODULE TO SEARCH AN ACCOUNT 
defsearchAccount(): 
globalcid
ifmyConnection: 
cursor=myConnection.cursor() 
cid=input("PLEASE ENTER CUSTOMER ID : ") 
account_no=int(input("PLEASE ENTER THE ACCOUNT NUMBER [0-9]: ")) 
sql="SELECT * FROM ACCOUNT WHERE CID = %s AND ACCOUNT_NO =%s" 
values=(cid,account_no) 
data=cursor.execute(sql,values) 
data = cursor.fetchall() 
if data: 
print("\n*****CUSTOMER ACCOUNT DETAILS*****") 
print(data) 
else: 
print("Sorry ! Account Infromation NOT Found , Please Try Again ! ") 
else: 
print("Somthing Went Wrong ,Please Try Again !") 
# MODULE TO WITHDRAW AMOUNT 
defwithdrawAmount(): 
count =3 
ifmyConnection: 
cursor=myConnection.cursor() 
account_no=int(input("PLEASE ENTER THE ACCOUNT NUMBER [0-9]: ")) 
sql="SELECT * FROM ACCOUNT WHERE ACCOUNT_NO =%s" 
values=(account_no,) 
data=cursor.execute(sql,values) 
data = cursor.fetchall() 
if data: 
while True: 
ATM_PIN=int(input("PLEASE ENTER THE ATM PIN - ONLY 3 
ATTEMPTS ARE ALLOWED : ")) 
sql='SELECT * FROM ACCOUNT WHERE PIN = %s' 
values=(ATM_PIN,) 
cursor.execute(sql,values) 
data = cursor.fetchall() 
if data: 
amount=int(input("PLEASE ENTER AMOUNT TO WITHDRAW 
: ")) 
sql='UPDATE ACCOUNT SET AMOUNT = AMOUNT - %s' 
cursor.execute(sql ,(amount,)) 
cursor.execute("COMMIT") 
print("******* TRANSACTION SUCCESSFULLY COMPLETED ! 
*******") 
print("***** PLEASE TAKE ONEY AND REMOVE YOUR CARD ! 
*****") 
break
else: 
print("Wrong Pin ! Please enter a Valid PIN") 
count=count-1 
print("You are left with only ",count ,"Attempts") 
if count == 0: 
print("Your Card has been Blocked , Please Visit the Branch to activate it") 
break
else: 
print("Sorry ! Account Infromation NOT Found , Please Try Again ! ")
#MODULE TO PROVIDE HELP FOR USER 
defhelpMe(): 
print("Please, Visit The Offcial Website Of Your bank To Download The Mannual !!!")
#STARTING POINT OF THE SYSTEM 
myConnection = MYSQLconnectionCheck () 
ifmyConnection: 
MYSQLconnection () 
while(1): 
print("\n!=========================*************=======================!") 
print("! PLEASE ENTER 1 FOR NEW USER !") print("! PLEASE ENTER 2 TO DISPLAY ALL 
CUSTOMERS !") 
print("! PLEASE ENTER 3 TO SEARCH A CUSTOMER !") 
print("! PLEASE ENTER 4 TO OPEN NEW ACCOUNT !") 
print("! PLEASE ENTER 5 TO DISPLAY ALL ACCOUNTS !") 
print("! PLEASE ENTER 6 TO SEARCH AN ACCOUNT !") 
print("! PLEASE ENTER 7 TO WITHDRAW AMOUNT !") 
print("! PLEASE ENTER 8 TO EXIT !") 
print("! PLEASE ENTER 0 FOR HELP !") 
print("\n!===========================*****END*****====================!") 
choice = int(input("\n Please Enter Your Choice : ")) 
if choice == 1: 
newCustomer()
elif choice == 2: 
displayAllCustomer()
elif choice == 3: 
searchCustomer()
elif choice == 4: 
newAccount()
elif choice==5: 
displayAllAccounts()
elif choice==6: 
searchAccount()
elif choice==7: 
withdrawAmount()
elif choice==8: 
break
elif choice==0: 
helpMe()
else: 
print("Sorry ,May Be You Are Giving Me Wrong Input, Please Try Again !!! ") 
else: print("Check Your MYSQL Connection First !!! ")
