
# Lunar Shop

We have amazing new products for our gaming service! Unfortunately we don't sell our unreleased flag product yet !

    Fuzzing is NOT allowed for this challenge, doing so will lead to IP rate limiting!

https://meteor.sunshinectf.games 

1. Initial Discovery and Vulnerability Analysis

The challenge forbade fuzzing, so when IDs 12 and 13 failed, I knew the solution had to be a logic flaw or a classic vulnerability. My first step was probing the URL parameter:

    Vulnerability Test: I injected a single quote (') into the product_id: product_id=1'
<img width="1916" height="829" alt="image" src="https://github.com/user-attachments/assets/64a3fc9f-b05e-4985-8309-ada42a61f106" />


2. Determining Column Count

The goal was a UNION SELECT attack, but I needed the column count (N) first. After a quick test, I found that the query used 4 columns:

    Working Payload: product_id=1 UNION SELECT 1,2,3,4--

    Result: The page loaded successfully, replacing the original product data with my injected numbers (1, 2, 3, 4), confirming N=4.
<img width="1906" height="803" alt="image" src="https://github.com/user-attachments/assets/c82261a4-a142-40fc-a88f-3cf02ca40d0c" />

3. Schema Dumping and Table Discovery

To ensure my injected data was the only thing visible, I modified the ID to -1 (an impossible product ID), forcing the original query to return zero rows. I then injected the SQLite schema query (since standard commands like SHOW TABLES failed).

    Payload to Dump Table Names (into Name field/column 2):
    product_id=-1 UNION SELECT 1,group_concat(tbl_name),3,4 FROM sqlite_master WHERE type='table'--

    Result: The Name field of the product table was replaced with a list of all tables: products,sqlite_sequence,flag. The target table was clearly named flag.

<img width="1742" height="287" alt="image" src="https://github.com/user-attachments/assets/95b4adba-9430-4142-8bc8-24d9221d7615" />


4. Final Flag Retrieval

With the table name known, the final step was to select the contents of the flag table and display the output in the visible Name column:

    Final Payload: product_id=-1 UNION SELECT 1,flag,3,4 FROM flag--

    Result: The Name field was replaced by the final flag value!

<img width="1852" height="271" alt="image" src="https://github.com/user-attachments/assets/11dd039d-4133-40b8-8e32-d6ebb4d91454" />


Flag: sun{baby_SQL_injection_this_is_known_as_error_based_SQL_injection_8767289082762892}

