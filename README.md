A numerical python interface to the berkeley database. This most useful in
combination with a higher level manager such as pycolumns. pycolumns has a
higher level interface that allows querying and comparison of "columns", each
of which is indexed using numpydb. 

Examples of Construction and Creation
```python
# creating a database with keys strings of length 20
# and data values that are 32-bit integers (4-byte).
# this is effectively an index on the strings.

# use the create helper function for creation.
import numpydb
key_dtype = 'S20'
data_dtype = 'i4'
numpydb.create(dbfile, key_dtype, data_dtype)

# constructing the NumpyBD and opening database for
# reading and updating
db=numpydb.NumpyDB(dbfile, mode='r+')

# construction without opening a file
db=numpydb.NumpyDB()

# now open the database for updating and reading.
db.open(dbfile, 'r+')

# show a representation of the database
# print(db) or just type db at an interactive prompt
print(db)
Filename: 'test-S20.db'
    key dtype: 'S20'
    data dtype: 'i4'
```
Putting Records:
```python
# keys and values can be scalar, sequence, or array.  They
# must be convertible to the same numpy data type as the keys
# and values in the database

db.put(keys, values)
```

Query Examples:
```python
import numpydb
db = numpydb.NumpyDB('somefile.db')

# Extract exact key matches from the database.  Can 
# be scalar, sequence, or array
values = db.match(values)

# Extract from a range of keys, using default inclusive interval
# [low,high].   Note between and range are synonyms
values = db.between(low, high)
values = db.range(low, high)

# Extract from different types of intervals.  Again, note range()
# is synonymous with between
values = db.between(low, high,'[]')
values = db.between(low, high,'[)')
values = db.between(low, high,'(]')
values = db.between(low, high,'()')
values = db.range(low, high,'()')


# one sided range queries
values = db.range1(low, '>')
values = db.range1(low, '>=')
values = db.range1(high,'<')
values = db.range1(high,'<=')
values = db.range1(key2match,'=')


# Can also return keys,values, e.g.
keys,values = db.match(values, select='both')
keys,values = db.between(low, high, select='both')

# Just counting the matches
count = db.match(values,select='count')
count = db.between(low, high,select='count')

# just getting the keys
keys = db.match(values, select='keys')
keys = db.between(low,high,select='keys')
```
