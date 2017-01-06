# Parse HTML tables with rowspan and colspan attributes

In brief, some tables are not recorded/coded neatly where every row contains the same amount of columns.
Instead, these tables use attributes rowspan and colspan to map out the table. These are neat but a pain to
scrape. Check out the corresponding notebook for the code that processes these tables into
Pandas dataframes. 

For example, if you examine the tables found on Wikipedia's political party strength entry for [Wisconsin] (https://en.wikipedia.org/wiki/Political_party_strength_in_Wisconsin), you will see (cntrl-U in Chrome) that the tables are of the form described above. For a more concise example of the html tables at discussion, observe below the snippet from the corresponding stackoverlflow [page](http://stackoverflow.com/questions/28763891/what-should-i-do-when-tr-has-rowspan)


````
<table width="100%" border="1">
    <tr>
        <td rowspan="2">one</td>
        <td>two</td>
        <td>three</td>
    </tr>
    <tr>
        <td colspan="2">February</td>
    </tr>
    <tr>
        <td>end</td>
        <td>End</td>
    </tr>
</table>
```
which produces this table:
<table width="100%" border="1">
    <tr>
        <td rowspan="2">one</td>
        <td>two</td>
        <td>three</td>
    </tr>
    <tr>
        <td colspan="2">February</td>
    </tr>
    <tr>
        <td>end</td>
        <td>End</td>
    </tr>
</table>

You can use the functions below (as illustrated in the notebook) to parse the above table into a Pandas DataFrame:
```Python
#store the above html in a string
s = """
<table width="100%" border="1">
    <tr>
        <td rowspan="2">one</td>
        <td>two</td>
        <td>three</td>
    </tr>
    <tr>
        <td colspan="2">February</td>
    </tr>
    <tr>
        <td>end</td>
        <td>End</td>
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
