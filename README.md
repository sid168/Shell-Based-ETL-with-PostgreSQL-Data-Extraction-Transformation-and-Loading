# Shell-Based-ETL-with-PostgreSQL-Data-Extraction-Transformation-and-Loading
In this project, we learn how to extract data from a file, transform it using shell commands, and load it into a PostgreSQL database using a shell script. This exercise covered a basic ETL process that can be extended for more complex tasks in real-world applications. This exercise comes from IBM ETL course on coursera. 


### Objectives

- Extract data from a delimited file using shell commands.
- Transform text data.
- Load data into a PostgreSQL database.

### Overview
We will extract user information from the `/etc/passwd` file, transform the data, and load it into a PostgreSQL table.

### Tools Used
- **Skills Network Cloud IDE** (based on Theia and Docker)
- **PostgreSQL** running in a Docker container


## Exercise 1: Extracting Data Using `cut` Command
The `cut` command helps extract selected characters or fields from a line of text.

1. **Extracting Characters:**
   - To extract the first four characters:
     ```bash
     echo "database" | cut -c1-4
     ```
     Output: `data`
   
   - To extract the 5th to 8th characters:
     ```bash
     echo "database" | cut -c5-8
     ```
     Output: `base`

2. **Extracting Fields:**
   - To extract the first field (username) from `/etc/passwd` (a colon-delimited file):
     ```bash
     cut -d":" -f1 /etc/passwd
     ```
   
   - To extract multiple fields (username, user id, and home directory):
     ```bash
     cut -d":" -f1,3,6 /etc/passwd
     ```

---

## Exercise 2: Transforming Data Using `tr` Command
The `tr` command is used to translate, squeeze, or delete characters.

1. **Translating Characters:**
   - To convert all lowercase alphabets to uppercase:
     ```bash
     echo "Shell Scripting" | tr "[:lower:]" "[:upper:]"
     ```
   
   - To convert all uppercase alphabets to lowercase:
     ```bash
     echo "Shell Scripting" | tr "[:upper:]" "[:lower:]"
     ```

2. **Squeezing Characters:**
   - To replace repeated spaces with a single space:
     ```bash
     ps | tr -s " "
     ```

3. **Deleting Characters:**
   - To delete all digits:
     ```bash
     echo "My login pin is 5634" | tr -d "[:digit:]"
     ```
     Output: `My login pin is`

---

## Exercise 3: Start the PostgreSQL Database
1. Start the PostgreSQL server by selecting **PostgreSQL Database Server -> Start** from the Skills Network tools.
2. Click on **PostgreSQL CLI** to interact with the database.

---

## Exercise 4: Create a Table in PostgreSQL
1. Connect to the `template1` database using the following command:
   ```bash
   \c template1
   ```

2. Create a `users` table:
   ```sql
   create table users(username varchar(50), userid int, homedirectory varchar(100));
   ```

---

## Exercise 5: Loading Data into PostgreSQL Table

1. **Create a Shell Script:**
   - Create a new shell script named `csv2db.sh`:
     ```bash
     touch csv2db.sh
     ```
   
   - Open the file and add the following lines:
     ```bash
     # Extract data from /etc/passwd file into a CSV file
     # Transform delimiter from ":" to ","
     # Load data into PostgreSQL database
     ```

2. **Extract Data:**
   - Add the following lines to extract username, user id, and home directory from `/etc/passwd`:
     ```bash
     cut -d":" -f1,3,6 /etc/passwd > extracted-data.txt
     ```

3. **Transform Data:**
   - Convert the delimiter from ":" to "," and save it in a CSV file:
     ```bash
     tr ":" "," < extracted-data.txt > transformed-data.csv
     ```

4. **Load Data into PostgreSQL:**
   - Add the following lines to load the CSV data into the `users` table:
     ```bash
     export PGPASSWORD=<yourpassword>
     echo "\c template1; COPY users FROM '/home/project/transformed-data.csv' DELIMITERS ',' CSV;" | psql --username=postgres --host=postgres
     ```

---

## Exercise 6: Execute the Final Script

1. **Run the script:**
   ```bash
   bash csv2db.sh
   ```

2. **Verify Data Load:**
   - Add the following command to verify the data in the `users` table:
     ```bash
     echo "SELECT * FROM users;" | psql --username=postgres --host=postgres template1
     ```

3. **Run the script again:**
   ```bash
   bash csv2db.sh
   ```

Now, the table `users` should be populated with data extracted from the `/etc/passwd` file.
