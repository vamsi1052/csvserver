docker command executed 
docker run -d --name csvserver -p 9393:9300 -e CSVSERVER_BORDER=Orange -v $(pwd)/inputFile:/csvserver/inputdata infracloudio/csvserver:latest
------
curl -o ./part-1-output http://localhost:9393/raw
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   127  100   127    0     0  30795      0 --:--:-- --:--:-- --:--:-- 31750
----
cat part-1-logs 
2024/06/19 09:18:15 listening on ****
2024/06/19 09:18:25 wrote 538 bytes for /
2024/06/19 09:18:25 wrote 538 bytes for /favicon.ico
2024/06/19 10:16:56 wrote 127 bytes for /raw
-----
gencsv.sh --
#!/bin/bash

# Check if exactly two arguments are provided
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 2 8"
    exit 1
fi

# Assign arguments to variables
start_index=$1
end_index=$2

# Name of the output file
output_file="inputFile"

# Create or overwrite the file
> $output_file

# Generate the content and write to the file
for ((i = start_index; i <= end_index; i++)); do
    value=$((RANDOM % 500)) # Generate a random number between 0 and 499
    echo "$i, $value" >> $output_file
done

# Print a message indicating the file has been created
echo "File '$output_file' has been created with entries from $start_index to $end_index."
---------
inputfile 
2, 69
3, 402
4, 40
5, 415
6, 212
7, 81
8, 439
-----
http://localhost:9393/
Welcome to the CSV Server
Index	Value
2	69
3	402
4	40
5	415
6	212
7	81
8	439
----
