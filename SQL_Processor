import re

# re helps match strings and search for them

# Store the table data as a dictionary of table names and their rows
tables = {}


def dojob(sql):
    global tables

    # Regular expression patterns to match SQL commands
    create_match = re.match(r'create table (\w+) \((\w+) int\);', sql, re.IGNORECASE)
    insert_match = re.match(r'insert into (\w+) values\((\d+)\);', sql, re.IGNORECASE)

    if create_match:
        # If it's a "CREATE TABLE" command, extract table_name and column_name
        table_name, column = create_match.groups()
        # Create an empty list for the table to store rows
        tables.setdefault(table_name, [])

    elif insert_match:
        # If it's an "INSERT INTO" command, extract table_name and value
        table_name, value = insert_match.groups()
        # Check if the table has reached its maximum capacity of 3 rows
        if len(tables.get(table_name, [])) < 3:
            # If not full, append the value as an integer to the table
            tables.setdefault(table_name, []).append(int(value))
        else:
            # If full, print a message indicating the table is full
            print(f"Table {table_name} is full")

    elif sql.strip().lower().startswith("select"):
        # Matches words to commands
        select_match = re.search(r'select (\w+) from (\w+)(?: where (\w+)=(\d+))?;', sql, re.IGNORECASE)
        if select_match:
            column, table_name, condition_column, condition_value = select_match.groups()
            rows = tables.get(table_name, [])
            if condition_column and condition_value:
                rows = [row for row in rows if row == int(condition_value)]
        else:
            column, table_name = re.search(r'select (\w+) from (\w+);', sql, re.IGNORECASE).groups()
            rows = tables.get(table_name, [])

        # Print the table name
        print(f"Table {table_name}:")
        # Convert the rows to strings and join them with commas
        rows_str = ', '.join(map(str, rows))
        # Print the rows
        print(rows_str)
        # Return the rows as a list of integers
        return rows

    else:
        # If the command is not recognized, raise an error
        raise ValueError("Invalid SQL command.")


def main():
    try:
        sql_commands = [
            "create table a (i int);",
            "insert into a values(10);",
            "insert into a values(2);",
            "insert into a values(3);",
            "create table b (i int);",
            "insert into b values(1);",
            "insert into b values(4);",
            "insert into b values(5);",
            "select i from a;",
            "select i from b;",
            "select i from a where i=3;",
            "select i from b where i=4;"
        ]

        # Collect the values for each table
        table_values = {}
        for sql in sql_commands:
            values = dojob(sql)
            if values is not None:
                table_name = re.search(r'from (\w+)', sql, re.IGNORECASE).group(1)
                table_values.setdefault(table_name, []).append(values)

        # Print the collected values for each table
        for table_name, values_list in table_values.items():
            print(f"Table {table_name}:")
            for values in values_list:
                print(', '.join(map(str, values)))

    except ValueError as e:
        print(f"Error: {str(e)}")


if __name__ == "__main__":
    main()

