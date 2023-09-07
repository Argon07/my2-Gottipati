#AGRAJA GOTTIIPATI
I'm pretty much intrested in computers and strives to become a Developer in software background.

![My picture](img.jpeg)


---

## Recommended Sports and Activities

---

Sports and physical activities are a great way to stay healthy and have fun. Here are four activities that I recommend trying:

| Sport/Activity  | Why I Recommend It                  | Hours/Week |
|-----------------|------------------------------------|------------|
| Cycling         | Cycling is an excellent cardio workout, and it's a fun way to explore your surroundings. Whether you're commuting or hitting the trails, it's a great choice. | 5-7 hours  |
| Soccer          | Soccer is the world's most popular sport, known for its teamwork and endurance. It's easy to start playing with just a ball and a field. | 2-3 hours  |
| Swimming        | Swimming is a full-body workout that's easy on the joints. It's perfect for cooling off in the summer and improving endurance. | 4-6 hours  |
| Table Tennis       | Table tennis is a fast-paced indoor sport that improves reflexes and concentration. It's perfect for social gatherings. | 3-4 hours  |

---

## Quotes by Scientists

---

> "It is not the strongest of the species that survive, nor the most intelligent, but the one most responsive to change."  
> — *Charles Darwin*

> "The path from dreams to success does exist. May you have the vision to find it, the courage to get on to it, and the perseverance to follow it." 
> — *Kalpana Chawla*

---
## code snippet

---
> How do I import a CSV file into MySQL?
<https://stackoverflow.com/questions/1299432/how-do-i-import-a-csv-file-into-mysql>


```
<?php

$databasehost = "localhost";
$databasename = "test";
$databasetable = "sample";
$databaseusername ="test";
$databasepassword = "";
$fieldseparator = ",";
$lineseparator = "\n";
$csvfile = "filename.csv";

/********************************/
/* Would you like to add an ampty field at the beginning of these records?
/* This is useful if you have a table with the first field being an auto_increment integer
/* and the csv file does not have such as empty field before the records.
/* Set 1 for yes and 0 for no. ATTENTION: don't set to 1 if you are not sure.
/* This can dump data in the wrong fields if this extra field does not exist in the table
/********************************/
$addauto = 0;
/********************************/

/* Would you like to save the mysql queries in a file? If yes set $save to 1.
/* Permission on the file should be set to 777. Either upload a sample file through ftp and
/* change the permissions, or execute at the prompt: touch output.sql && chmod 777 output.sql
/********************************/
$save = 1;
$outputfile = "output.sql";
/********************************/

if (!file_exists($csvfile)) {
        echo "File not found. Make sure you specified the correct path.\n";
        exit;
}

$file = fopen($csvfile,"r");

if (!$file) {
        echo "Error opening data file.\n";
        exit;
}

$size = filesize($csvfile);

if (!$size) {
        echo "File is empty.\n";
        exit;
}

$csvcontent = fread($file,$size);

fclose($file);

$con = @mysql_connect($databasehost,$databaseusername,$databasepassword) or die(mysql_error());
@mysql_select_db($databasename) or die(mysql_error());

$lines = 0;
$queries = "";
$linearray = array();

foreach(split($lineseparator,$csvcontent) as $line) {

        $lines++;

        $line = trim($line," \t");

        $line = str_replace("\r","",$line);

        /************************************
        This line escapes the special character. remove it if entries are already escaped in the csv file
        ************************************/
        $line = str_replace("'","\'",$line);
        /*************************************/

        $linearray = explode($fieldseparator,$line);

        $linemysql = implode("','",$linearray);

        if($addauto)
                $query = "insert into $databasetable values('','$linemysql');";
        else
                $query = "insert into $databasetable values('$linemysql');";

        $queries .= $query . "\n";

        @mysql_query($query);
}

@mysql_close($con);

if ($save) {

        if (!is_writable($outputfile)) {
                echo "File is not writable, check permissions.\n";
        }

        else {
                $file2 = fopen($outputfile,"w");

                if(!$file2) {
                        echo "Error writing to the output file.\n";
                }
                else {
                        fwrite($file2,$queries);
                        fclose($file2);
                }
        }

}

echo "Found a total of $lines records in this csv file.\n";

?>

```

<https://css-tricks.com/snippets/php/import-csv-into-mysql/>