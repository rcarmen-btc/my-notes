# Python Specialization

#JSON #Python #SQL #XML

# Regular expressions

*regex, regexp*

---

```python
**^ # Matches the beginning of a line
$ # Matches the end of a line 
. # (dot) Matches any character
\s # Matches whitespace
\S # Matches any non-whitespace character
* # Repeats a char zero or more times
*? # Repeats a char zero or more times (non-greedy)
+ # Repeats a char one or more times 
+? # Repeats a char one or more times (non-greedy)
[aeiou] # Matches a single char in the listed set
[^AEOW] # Matches a single char not in the listed set
[a-z0-9] # The set of chars can include the range
( # Indicates where string extraction is to start
) # Indicates where string extraction is to end
\ # Escape chars**
```

## Library

---

```python
import re
```

## Methods

---

```python
# like .find()
import re

text = "Hello"

# returns True/False depending on whether the string matches the regexp
re.search('lo', text)

# if we actually want the matching strings to be extracted, we use re.fideall()
re.findall('^From \S+@(\S+)', line)
```

# Socket

---

**telnet** 

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('data.pr4e.org', 80))

cmd = 'GET http:data.pr4e.org/romeo.txt HTTP/1.0'.encode()

sock.send(cmd)

while True:
    data = sock.recv(512)
    if len(data) < 1:
        break
    print(data.decode())
sock.close()
```

# Unicode chars

---

```python
# The numeric value of a simple ASCII char
ord('A')
```

# urllib

---

```python
import urllib.request, urllib.parse, urllib.error

fhand = urllib.request.urlopen('https://www.coursera.org/learn/python-network-data/lecture/bwvyb/12-4-retrieving-web-pages')
for line in fhand:
    print(line.decode().strip())
```

# Web scraping, BeautifulSoup

---

```python
import urllib.request, urllib.parse, urllib.error
from bs4 import BeautifulSoup

url = input('Enter - ')
html = urllib.request.urlopen(url).read()
soup = BeautifulSoup(html, 'html.parser')

tags = soup('a')
for tag in tags:
	    print(tag.get('class', None))
```

# XML

---

## Simple XML

```python
import xml.etree.ElementTree as ET

data = """<note>
    <link type="text/css" id="dark-mode" rel="stylesheet" href=""/>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>"""

tree = ET.fromstring(data)
print('Link:', tree.find('link').get('type'))
print('To:', tree.find('to').text)
print('From:', tree.find('from').text)
print('Heading:', tree.find('heading').text)
print('Body:', tree.find('body').text)

# Link: text/css
# To: Tove
# From: Jani
# Heading: Reminder
# Body: Don't forget me this weekend!
```

## XML Catalog

```python
from traceback import print_tb
import xml.etree.ElementTree as ET

data = """<CATALOG>
    <CD>
    <TITLE>Picture book</TITLE>
    <ARTIST>Simply Red</ARTIST>
    <COUNTRY>EU</COUNTRY>
    <COMPANY>Elektra</COMPANY>
    <PRICE>7.20</PRICE>
    <YEAR>1985</YEAR>
    </CD>
    <CD>
    <TITLE>Red</TITLE>
    <ARTIST>The Communards</ARTIST>
    <COUNTRY>UK</COUNTRY>
    <COMPANY>London</COMPANY>
    <PRICE>7.80</PRICE>
    <YEAR>1987</YEAR>
    </CD>
    <CD>
    <TITLE>Unchain my heart</TITLE>
    <ARTIST>Joe Cocker</ARTIST>
    <COUNTRY>USA</COUNTRY>
    <COMPANY>EMI</COMPANY>
    <PRICE>8.20</PRICE>
    <YEAR>1987</YEAR>
    </CD>
</CATALOG>"""

stuff = ET.fromstring(data)

lst = stuff.findall('CD')
	print('Catalog lenth:', len(lst))

for i in lst:
    print(i.find('ARTIST').text)
    print('     Title:', i.find('TITLE').text, f'({i.find("YEAR").text})')
		print('     Attr:', i.get('Some attr'))

# Catalog lenth: 3
# Simply Red
#      Title: Picture book (1985)
# The Communards
#      Title: Red (1987)
# Joe Cocker
#      Title: Unchain my heart (1987)
```

# JSON

---

## Simple JSON

```python
import json

data = """{
    "glossary": {
        "title": "example glossary",
		"GlossDiv": {
            "title": "S",
			"GlossList": {
                "GlossEntry": {
                    "ID": "SGML",
					"SortAs": "SGML",
					"GlossTerm": "Standard Generalized Markup Language",
					"Acronym": "SGML",
					"Abbrev": "ISO 8879:1986",
					"GlossDef": {
                        "para": "A meta-markup language, used to create markup languages such as DocBook.",
						"GlossSeeAlso": ["GML", "XML"]
                    },
					"GlossSee": "markup"
                }
            }
        }
    }
}"""

info = json.loads(data)

print(f'Glossary: {info["glossary"]["title"]}')
```

## JSON list

---

```python
import json
import urllib.request, urllib.parse, urllib.error

serviceurl = 'https://maps.google.com/maps/api/geocode/json?'

while True:
    # address = input('Enter the location: ') 
    address = 'Ann Arbor, MI'
    if len(address) < 1:
        break
    
    url = serviceurl + urllib.parse.urlencode({'address': address})

    print('Retrieving', url)
    uh = urllib.request.urlopen(url)
    data = uh.read().decode()
    print('Retrieved', len(data), 'chars')

    try:
        js = json.loads(data)
    except:
        js = None
    
        
    if not js or 'status' not in js or js['status'] != 'OK':
        print("Fail")
        print(data)
        continue

    lat = js['results'][0]['geometry']['location']['lat']
    lng = js['results'][0]['geometry']['location']['lng']
    print('lat', lat, 'lng', lng)
    location = js['results'][0]['formatted_address']
    print(location)
```

# OOP

---

We know this shit

# —[Using Databases with Python](https://www.coursera.org/learn/python-databases/home/welcome)—

# Relational Databases

## SQL

---

**Create table**

```sql
CREATE TABLE Users (name VARCHAR(128), email VARCHAR(128))
```

**Insert a row (rows) into a table**

```sql
INSERT INTO Users (name, email) VALUES ('JohDFn', 'John@gFDmail.com')
```

**Delete a row (rows)**

```sql
DELETE FROM Users WHERE email='hii@gmail.com'
```

**Update a row (rows)**

```sql
UPDATE Users SET name='John2' WHERE email='John@gmail.com'
```

**Retrieving records: SELECT**

```sql
SELECT * FROM Users

# or with WHERE

SELECT * FROM Users WHERE email='5@gmail.com'

#or 

SELECT name FROM Users WHERE email='5@gmail.com'
```

**Sorting with ORDER BY**

```sql
SELECT name, email FROM Users ORDER BY name
```

## Python with SQLite

---

```python
import re
import sqlite3

class Table:

    cur = None
    conn = None

    tab_name = ''
    name = ''
    age = 0
    

    def __init__(self, db_name: str) -> None:
        self.conn = sqlite3.Connection(db_name + '.sqlite')
        self.cur = self.conn.cursor() 

    def __del__(self):
        self.conn.close()
    
    def create_table(self, tab_name: str, qname_type: dict):
        list_of_arguments = ''
        self.tab_name = tab_name
        for name, type in qname_type.items():
            list_of_arguments += name + ' ' + type + ', '
            
        create_query = 'CREATE TABLE ' + tab_name + ' (' + list_of_arguments + ')'
        create_query = create_query[:-3] + ')'

        self.cur.execute('DROP TABLE IF EXISTS {}'.format(tab_name))
        self.cur.execute(create_query)
    
    def insert(self, val_tuple: tuple):
        insert_query = 'INSERT INTO ' + self.tab_name + ' VALUES ' + str(val_tuple)
        print(insert_query)
        self.cur.execute(insert_query)
        self.conn.commit()

    def delete(self, qwhere_str: str):
        delete_query = 'DELETE FROM ' + self.tab_name + ' WHERE ' + qwhere_str  
        self.cur.execute(delete_query)
        self.conn.commit()
        print(delete_query)

t = Table('example')

t.create_table('Counts', {'org': 'TEXT', 'count': 'INTEGER'})

d = {}
with open('data.txt') as f:
    for line in f:
        if re.findall('^From: (.+@.+)', line):
            d[line.strip().split()[1].split('@')[1]] = d.get(line.strip().split()[1].split('@')[1], 0) + 1 

for i, v in d.items():
    t.insert((i, v))    
```

## Designing a Data Model

---

**Creating all data bases | DO NOT DO DATA REPLICATION IN TABLE**

```sql
-- Artist table
CREATE TABLE Artist (
	id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
	name TEXT
);

-- Genre table
CREATE TABLE Genre (
	id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE, 
	name TEXT
);

-- Album table
CREATE TABLE Album (
	id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
	artist_id INTEGER,
	title TEXT
);

-- Track table
CREATE TABLE Track (
	id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
	title TEXT,
	album_id INTEGER,
	genre_id INTEGER,
	len INTEGER,
	rating INTEGER, 
	count INTEGER
);
```

**Insert data** 

```sql
INSERT INTO Artist (name) VALUES ('Led Zepplin');
INSERT INTO Artist (name) VALUES ('AC/DC');

INSERT INTO Genre (name) VALUES ('Rock');
INSERT INTO Genre (name) VALUES ('Metal');

INSERT INTO Album (title, artist_id) VALUES ('Who Made Who', 2);
INSERT INTO Album (title, artist_id) VALUES ('IV', 1);

INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Black Dod', 5, 297, 0, 2, 1);
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Stairway', 4, 343, 0, 2, 1);
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('About to Rock', 5, 227, 0, 1, 2);
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Who Made Who', 3, 123, 0, 1, 1);
```

**Connecting tables**

```sql
-- ON work line WHERE
SELECT Album.title, Artist.name FROM Album JOIN Artist ON Album.artist_id = Artist.id

SELECT Album.title, Album.artist_id, Artist.id, Artist.name FROM Album JOIN Artist ON Album.artist_id = Artist.id

-- Without NO clause
SELECT Track.title, Track.genre_id, Genre.id, Genre.name FROM Track JOIN Genre
```

## From .xml to .sqlite

---

```python
import sqlite3
import xml.etree.ElementTree as ET

def lookup(el, arg):
    for i in range(len(el) - 1):
        if el[i].text == arg: 
            return el[i + 1].text
    return None
            
query = """
DROP TABLE IF EXISTS Artist;
DROP TABLE IF EXISTS Album;
DROP TABLE IF EXISTS Track;
DROP TABLE IF EXISTS Genre;

CREATE TABLE Artist (
    id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name TEXT
);

CREATE TABLE Genre (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name    TEXT UNIQUE
);

CREATE TABLE Album (
    id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    artist_id INTEGER,
    title TEXT
);

CREATE TABLE Track (
    id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    title TEXT,
    album_id INTEGER,
    genre_id INTEGER,
    len INTEGER,
    rating INTEGER, 
    count INTEGER
);
"""

conn = sqlite3.Connection('example.sqlite')
cur = conn.cursor() 
cur.executescript(query)

tree = ET.parse('tracks/Library.xml')
root = tree.getroot().findall('dict/dict/dict')

for el in root:
    if lookup(el, 'Track ID') is None:
        continue

    name = lookup(el, 'Name')         
    genre = lookup(el, 'Genre')
    artist = lookup(el, 'Artist')         
    album = lookup(el, 'Album')
    count = lookup(el, 'Play Count')
    rating = lookup(el, 'Rating')
    length = lookup(el, 'Total Time')
    

    if name is None or artist is None or album is None:
        continue
    
    print("-----------------\nName: {}\nArtist: {}\nAlbum: {}\nPlay Count: {}\nRating: {}\nTotal time: {}".format(name, artist, album, count, rating, length))

    cur.execute('INSERT OR IGNORE INTO Artist (name) VALUES (?)', (artist, ))
    cur.execute('SELECT id FROM Artist WHERE name = ?', (artist, ))
    artist_id = cur.fetchone()[0]

    cur.execute('INSERT OR IGNORE INTO Album (title, artist_id) VALUES (?, ?)', (album, artist_id))
    cur.execute('SELECT id FROM Album WHERE title = ?', (album, ))
    album_id = cur.fetchone()[0]

    genre_id = None
    if genre is not None:
        cur.execute('INSERT OR IGNORE INTO Genre (name) VALUES (?)', (genre, ))
        cur.execute('SELECT id FROM Genre WHERE name = ?', (genre, ))
        genre_id = cur.fetchone()[0]

    cur.execute('INSERT OR REPLACE INTO Track (title, album_id, genre_id, len, rating, count) VALUES (?, ?, ?, ?, ?, ?)', (album, album_id, genre_id, length, rating, count))

conn.commit()
```

## Many-to-many

---

```python
import sqlite3
import json

query = """
DROP TABLE IF EXISTS User;
DROP TABLE IF EXISTS Member;
DROP TABLE IF EXISTS Course;

CREATE TABLE User (
    id     INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name   TEXT UNIQUE
);

CREATE TABLE Course (
    id     INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    title  TEXT UNIQUE
);

CREATE TABLE Member (
    user_id     INTEGER,
    course_id   INTEGER,
    role        INTEGER,
    PRIMARY KEY (user_id, course_id)
)
"""

conn = sqlite3.Connection('example.sqlite')
cur = conn.cursor() 
cur.executescript(query)

with open('data.txt') as f:
    all = json.load(f)

for student in all:
    cur.execute('insert or ignore into User (name) values (?)', (student[0], ))
    cur.execute('select id from User where name = ?', (student[0], ))
    user_id = cur.fetchone()[0]

    cur.execute('insert or ignore into Course (title) values (?)', (student[1], ))
    cur.execute('select id from Course where title = ?', (student[1], ))
    course_id = cur.fetchone()[0]

    cur.execute('insert or ignore into Member (user_id, course_id, role) values (?, ?, ?)', (user_id, course_id, student[2])) 

conn.commit()
```

**Commands**

```sql
SELECT User.name,Course.title, Member.role FROM 
    User JOIN Member JOIN Course 
    ON User.id = Member.user_id AND Member.course_id = Course.id
    ORDER BY User.name DESC, Course.title DESC, Member.role DESC LIMIT 2;

SELECT 'XYZZY' || hex(User.name || Course.title || Member.role ) AS X FROM 
    User JOIN Member JOIN Course 
    ON User.id = Member.user_id AND Member.course_id = Course.id
    ORDER BY X LIMIT 1;
```