# Parse HTML tables with rowspan and colspan attributes

In brief, some tables are not recorded/coded neatly where every row contains the same amount of columns.
Instead, these tables use attributes rowspan and colspan to map out the table. These are neat but a pain to
scrape. Check out the corresponding notebook for the code that processes these tables into
Pandas dataframes. 

For example, if you examine the tables found on Wikipedia's political party strength entry for [Wisconsin] (https://en.wikipedia.org/wiki/Political_party_strength_in_Wisconsin), you will see (cntrl-U in Chrome) that the tables are of the form described above. For a more concise example of the html tables at discussion, observe below the snippet from the corresponding stackoverlflow [page](http://stackoverflow.com/questions/28763891/what-should-i-do-when-tr-has-rowspan)


````
<table>
    <tr>
        <th rowspan="2">year</th>
        <th>col1</th>
        <th colspan="2">col2</th>
    </tr>
    <tr>
        <td rowspan="2">aaa</td>
        <td rowspan="2">bbb</td>
        <td rowspan="1">ccc</td>
    </tr>
    <tr>
        <td>2017</td>
        <td rowspan="2">ddd</td>
    </tr>
    <tr>
        <td>2018</td>
        <td>col1</td>
        <td colspan="2">col2</td>
    </tr>
</table>
```
which produces this table:
<table>
    <tr>
        <th rowspan="2">year</th>
        <th>col1</th>
        <th colspan="2">col2</th>
    </tr>
    <tr>
        <td rowspan="2">aaa</td>
        <td rowspan="2">bbb</td>
        <td rowspan="1">ccc</td>
    </tr>
    <tr>
        <td>2017</td>
        <td rowspan="2">ddd</td>
    </tr>
    <tr>
        <td>2018</td>
        <td>col1</td>
        <td colspan="2">col2</td>
    </tr>
</table>

You can use the functions below (as illustrated in the notebook) to parse the above table into a Pandas DataFrame:
```Python
#store the above html in a string
s = """
<table>
    <tr>
        <th rowspan="2">year</th>
        <th>col1</th>
        <th colspan="2">col2</th>
    </tr>
    <tr>
        <td rowspan="2">aaa</td>
        <td rowspan="2">bbb</td>
        <td rowspan="1">ccc</td>
    </tr>
    <tr>
        <td>2017</td>
        <td rowspan="2">ddd</td>
    </tr>
    <tr>
        <td>2018</td>
        <td>col1</td>
        <td colspan="2">col2</td>
    </tr>
</table>
"""

# make string into a bs4 object and find the table of interest
table = bs(s, 'lxml').find('table')

# use functions to parse the table into a Pandas DataFrame
rows, num_rows, num_cols = pre_process_table(table)
df = process_rows(rows, num_rows, num_cols)
print(df)
```
