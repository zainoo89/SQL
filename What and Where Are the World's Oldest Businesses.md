## 1. The oldest business in the world
<p><img src="https://assets.datacamp.com/production/repositories/5851/datasets/7dded924c6dc418d4a828f2f4daba99953c27a5a/400px-Eingang_zum_St._Peter_Stiftskeller.jpg" alt="The entrance to St. Peter Stiftskeller, a restaurant in Saltzburg, Austria. The sign over the entrance shows &quot;803&quot; - the year the business opened."></p>
<p><em>Image: St. Peter Stiftskeller, founded 803. Credit: <a href="https://commons.wikimedia.org/wiki/File:Eingang_zum_St._Peter_Stiftskeller.jpg">Pakeha</a>.</em></p>
<p>An important part of business is planning for the future and ensuring that the company survives changing market conditions. Some businesses do this really well and last for hundreds of years.</p>
<p>BusinessFinancing.co.uk <a href="https://businessfinancing.co.uk/the-oldest-company-in-almost-every-country">researched</a> the oldest company that is still in business in (almost) every country and compiled the results into a dataset. In this project, you'll explore that dataset to see what they found.</p>
<p>The database contains three tables.</p>
<h3 id="categories"><code>categories</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>category_code</code></td>
<td>varchar</td>
<td>Code for the category of the business.</td>
</tr>
<tr>
<td style="text-align:left;"><code>category</code></td>
<td>varchar</td>
<td>Description of the business category.</td>
</tr>
</tbody>
</table>
<h3 id="countries"><code>countries</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>country_code</code></td>
<td>varchar</td>
<td>ISO 3166-1 3-letter country code.</td>
</tr>
<tr>
<td style="text-align:left;"><code>country</code></td>
<td>varchar</td>
<td>Name of the country.</td>
</tr>
<tr>
<td style="text-align:left;"><code>continent</code></td>
<td>varchar</td>
<td>Name of the continent that the country exists in.</td>
</tr>
</tbody>
</table>
<h3 id="businesses"><code>businesses</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>business</code></td>
<td>varchar</td>
<td>Name of the business.</td>
</tr>
<tr>
<td style="text-align:left;"><code>year_founded</code></td>
<td>int</td>
<td>Year the business was founded.</td>
</tr>
<tr>
<td style="text-align:left;"><code>category_code</code></td>
<td>varchar</td>
<td>Code for the category of the business.</td>
</tr>
<tr>
<td style="text-align:left;"><code>country_code</code></td>
<td>char</td>
<td>ISO 3166-1 3-letter country code.</td>
</tr>
</tbody>
</table>
<p>Let's begin by looking at the range of the founding years throughout the world.</p>


```python
%%sql 
postgresql:///oldestbusinesses
 
-- Select the oldest and newest founding years from the businesses table
SELECT MIN(year_founded), MAX(year_founded)
    FROM businesses;
```

    1 rows affected.





<table>
    <tr>
        <th>min</th>
        <th>max</th>
    </tr>
    <tr>
        <td>578</td>
        <td>1999</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (1, 2)
    except AssertionError:
        assert False, "The results should have two columns and a single row."
def test_colnames():
    try:
        assert results.columns.tolist() == ['min', 'max']
    except AssertionError:
        assert False, "The results should have columns named `'min` and `'max'`."
def test_min_year_founded():
    try:
        assert results.loc[0, 'min'] == 578
    except AssertionError:
        assert False, "The oldest year founded is incorrect."
def test_max_year_founded():
    try:
        assert results.loc[0, 'max'] == 1999 
    except AssertionError:
        assert False, "The newest year founded is incorrect."
```






    5/5 tests passed




## 2. How many businesses were founded before 1000?
<p>Wow! That's a lot of variation between countries. In one country, the oldest business was only founded in 1999. By contrast, the oldest business in the world was founded back in 578. That's pretty incredible that a business has survived for more than a millennium.</p>
<p>I wonder how many other businesses there are like that.</p>


```python
%%sql

-- Get the count of rows in businesses where the founding year was before 1000
SELECT COUNT (*)
    FROM businesses
    WHERE year_founded < 1000;
```

     * postgresql:///oldestbusinesses
    1 rows affected.





<table>
    <tr>
        <th>count</th>
    </tr>
    <tr>
        <td>6</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (1, 1)
    except AssertionError:
        assert False, "The results should have a single column and a single row."
def test_colnames():
    try:
        assert results.columns.tolist() == ['count']
    except AssertionError:
        assert False, "The results should have a column named `'count`."
def test_count():
    try:
        assert last_output.DataFrame().loc[0, 'count'] == 6
    except AssertionError:
        assert False, "The count of businesses founded before 1000 is incorrect."
```






    4/4 tests passed




## 3. Which businesses were founded before 1000?
<p>Having a count is all very well, but I'd like more detail. Which businesses have been around for more than a millennium?</p>


```python
%%sql
SELECT *
    FROM businesses
WHERE year_founded < 1000
ORDER BY year_founded;
```

     * postgresql:///oldestbusinesses
    6 rows affected.





<table>
    <tr>
        <th>business</th>
        <th>year_founded</th>
        <th>category_code</th>
        <th>country_code</th>
    </tr>
    <tr>
        <td>Kongō Gumi</td>
        <td>578</td>
        <td>CAT6</td>
        <td>JPN</td>
    </tr>
    <tr>
        <td>St. Peter Stifts Kulinarium</td>
        <td>803</td>
        <td>CAT4</td>
        <td>AUT</td>
    </tr>
    <tr>
        <td>Staffelter Hof Winery</td>
        <td>862</td>
        <td>CAT9</td>
        <td>DEU</td>
    </tr>
    <tr>
        <td>Monnaie de Paris </td>
        <td>864</td>
        <td>CAT12</td>
        <td>FRA</td>
    </tr>
    <tr>
        <td>The Royal Mint</td>
        <td>886</td>
        <td>CAT12</td>
        <td>GBR</td>
    </tr>
    <tr>
        <td>Sean&#x27;s Bar</td>
        <td>900</td>
        <td>CAT4</td>
        <td>IRL</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (6, 4)
    except AssertionError:
        assert False, "The results should have four columns and six rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['business', 'year_founded', 'category_code', 'country_code']
    except AssertionError:
        assert False, "The results should have the four columns from the `businesses` table."
def test_where_year_founded_lt_1000():
    try:
        assert results.loc[:, 'year_founded'].max() < 1000 
    except AssertionError:
        assert False, "The most recent year founded is not before 1000."
def test_ordered_by_year_founded():
    try:
        assert results.loc[:, 'year_founded'].is_monotonic 
    except AssertionError:
        assert False, "The rows are not ordered by increasing year founded."
```






    5/5 tests passed




## 4. Exploring the categories
<p>Now we know that the oldest, continuously operating company in the world is called Kongō Gumi. But was does that company do? The category codes in the <code>businesses</code> table aren't very helpful: the descriptions of the categories are stored in the <code>categories</code> table.</p>
<p>This is a common problem: for data storage, it's better to keep different types of data in different tables, but for analysis, you want all the data in one place. To solve this, you'll have to join the two tables together.</p>


```python
%%sql
SELECT bus.business, bus.year_founded, bus.country_code, cat.category
    FROM businesses as bus
    INNER JOIN categories as cat
        ON bus.category_code = cat.category_code
    WHERE year_founded < 1000
    ORDER BY year_founded;
```

     * postgresql:///oldestbusinesses
    6 rows affected.





<table>
    <tr>
        <th>business</th>
        <th>year_founded</th>
        <th>country_code</th>
        <th>category</th>
    </tr>
    <tr>
        <td>Kongō Gumi</td>
        <td>578</td>
        <td>JPN</td>
        <td>Construction</td>
    </tr>
    <tr>
        <td>St. Peter Stifts Kulinarium</td>
        <td>803</td>
        <td>AUT</td>
        <td>Cafés, Restaurants &amp; Bars</td>
    </tr>
    <tr>
        <td>Staffelter Hof Winery</td>
        <td>862</td>
        <td>DEU</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
    </tr>
    <tr>
        <td>Monnaie de Paris </td>
        <td>864</td>
        <td>FRA</td>
        <td>Manufacturing &amp; Production</td>
    </tr>
    <tr>
        <td>The Royal Mint</td>
        <td>886</td>
        <td>GBR</td>
        <td>Manufacturing &amp; Production</td>
    </tr>
    <tr>
        <td>Sean&#x27;s Bar</td>
        <td>900</td>
        <td>IRL</td>
        <td>Cafés, Restaurants &amp; Bars</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (6, 4)
    except AssertionError:
        assert False, "The results should have four columns and six rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['business', 'year_founded', 'country_code', 'category']
    except AssertionError:
        assert False, "The results should have business, year founded, and country code columns from the `businesses` table and category from the `categories` table."
def test_where_year_founded_lt_1000():
    try:
        assert results.loc[:, 'year_founded'].max() < 1000 
    except AssertionError:
        assert False, "The most recent year founded is not before 1000."
def test_ordered_by_year_founded():
    try:
        assert results.loc[:, 'year_founded'].is_monotonic 
    except AssertionError:
        assert False, "The rows are not ordered by increasing year founded."
```






    5/5 tests passed




## 5. Counting the categories
<p>With that extra detail about the oldest businesses, we can see that Kongō Gumi is a construction company. In that list of six businesses, we also see a café, a winery, and a bar. The two companies recorded as "Manufacturing and Production" are both mints. That is, they produce currency.</p>
<p>I'm curious as to what other industries constitute the oldest companies around the world, and which industries are most common.</p>


```python
%%sql
SELECT cat.category, count(*) AS n
    FROM categories AS cat
    INNER JOIN businesses AS bus
    ON cat.category_code = bus.category_code
    GROUP BY cat.category
    ORDER BY n DESC
    LIMIT 10;
```

     * postgresql:///oldestbusinesses
    10 rows affected.





<table>
    <tr>
        <th>category</th>
        <th>n</th>
    </tr>
    <tr>
        <td>Banking &amp; Finance</td>
        <td>37</td>
    </tr>
    <tr>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>22</td>
    </tr>
    <tr>
        <td>Aviation &amp; Transport</td>
        <td>19</td>
    </tr>
    <tr>
        <td>Postal Service</td>
        <td>16</td>
    </tr>
    <tr>
        <td>Manufacturing &amp; Production</td>
        <td>15</td>
    </tr>
    <tr>
        <td>Media</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Agriculture</td>
        <td>6</td>
    </tr>
    <tr>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>6</td>
    </tr>
    <tr>
        <td>Food &amp; Beverages</td>
        <td>6</td>
    </tr>
    <tr>
        <td>Tourism &amp; Hotels</td>
        <td>4</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (10, 2)
    except AssertionError:
        assert False, "The results should have two columns and ten rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['category', 'n']
    except AssertionError:
        assert False, "The results should have a category column and a count column named `'n'`."
def test_ordered_by_desc_n():
    try:
        assert results.loc[:, 'n'].is_monotonic_decreasing
    except AssertionError:
        assert False, "The rows are not ordered by descending count."
def test_count():
    try:
        assert results.loc[:, 'n'].values.tolist() == [37, 22, 19, 16, 15, 7, 6, 6, 6, 4]
    except AssertionError:
        assert False, "The category counts are not correct."
```






    5/5 tests passed




## 6. Oldest business by continent
<p>It looks like "Banking &amp; Finance" is the most popular category. Maybe that's where you should aim if you want to start a thousand-year business.</p>
<p>One thing we haven't looked at yet is where in the world these really old businesses are. To answer these questions, we'll need to join the <code>businesses</code> table to the <code>countries</code> table. Let's start by asking how old the oldest business is on each continent.</p>


```python
%%sql
SELECT MIN(bus.year_founded) AS oldest, country.continent
    FROM businesses as bus
    INNER JOIN countries as country
    ON bus.country_code = country.country_code
    GROUP BY country.continent
    ORDER BY oldest;
```

     * postgresql:///oldestbusinesses
    6 rows affected.





<table>
    <tr>
        <th>oldest</th>
        <th>continent</th>
    </tr>
    <tr>
        <td>578</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>803</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>1534</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>1565</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>1772</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>1809</td>
        <td>Oceania</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (6, 2)
    except AssertionError:
        assert False, "The results should have two columns and six rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['oldest', 'continent']
    except AssertionError:
        assert False, "The results should have columns named oldest, and continent."
def test_ordered_by_min_year_founded():
    try:
        assert results.loc[:, 'oldest'].is_monotonic 
    except AssertionError:
        assert False, "The rows are not ordered by year founded."
def test_count():
    try:
        assert results.loc[:, 'oldest'].values.tolist() == [578, 803, 1534, 1565, 1772, 1809]
    except AssertionError:
        assert False, "The year founded values are not correct."
```






    5/5 tests passed




## 7. Joining everything for further analysis
<p>Interesting. There's a jump in time from the older businesses in Asia and Europe to the 16th Century oldest businesses in North and South America, then to the 18th and 19th Century oldest businesses in Africa and Oceania. </p>
<p>As mentioned earlier, when analyzing data it's often really helpful to have all the tables you want access to joined together into a single set of results that can be analyzed further. Here, that means we need to join all three tables.</p>


```python
%%sql
SELECT bus.business, bus.year_founded, cat.category, cnt.country, cnt.continent
    FROM businesses as bus
    INNER JOIN categories as cat
    ON bus.category_code = cat.category_code
    INNER JOIN countries as cnt
        ON bus.country_code = cnt.country_code;
```

     * postgresql:///oldestbusinesses
    163 rows affected.





<table>
    <tr>
        <th>business</th>
        <th>year_founded</th>
        <th>category</th>
        <th>country</th>
        <th>continent</th>
    </tr>
    <tr>
        <td>Spinzar Cotton Company</td>
        <td>1930</td>
        <td>Agriculture</td>
        <td>Afghanistan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>ALBtelecom</td>
        <td>1912</td>
        <td>Telecommunications</td>
        <td>Albania</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Andbank</td>
        <td>1930</td>
        <td>Banking &amp; Finance</td>
        <td>Andorra</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Liwa Chemicals</td>
        <td>1939</td>
        <td>Manufacturing &amp; Production</td>
        <td>United Arab Emirates</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bank of the Province of Buenos Aires</td>
        <td>1822</td>
        <td>Banking &amp; Finance</td>
        <td>Argentina</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Yerevan Ararat Brandy-Wine-Vodka Factory</td>
        <td>1877</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Armenia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Australia Post</td>
        <td>1809</td>
        <td>Postal Service</td>
        <td>Australia</td>
        <td>Oceania</td>
    </tr>
    <tr>
        <td>St. Peter Stifts Kulinarium</td>
        <td>803</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>Austria</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Azerbaijan Caspian Shipping Company</td>
        <td>1858</td>
        <td>Aviation &amp; Transport</td>
        <td>Azerbaijan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Brarudi</td>
        <td>1955</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Burundi</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Affligem Brewery</td>
        <td>1074</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Belgium</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Communauté Électrique du Bénin</td>
        <td>1968</td>
        <td>Energy</td>
        <td>Benin</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Air Burkina</td>
        <td>1967</td>
        <td>Aviation &amp; Transport</td>
        <td>Burkina Faso</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>M. M. Ispahani Limited</td>
        <td>1820</td>
        <td>Food &amp; Beverages</td>
        <td>Bangladesh</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Arsenal AD</td>
        <td>1878</td>
        <td>Defense</td>
        <td>Bulgaria</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>BMMI</td>
        <td>1883</td>
        <td>Retail</td>
        <td>Bahrain</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Sarajevska Pivara</td>
        <td>1864</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Bosnia and Herzegovina</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Olivaria Brewery</td>
        <td>1864</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Belarus</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Belize Bank</td>
        <td>1902</td>
        <td>Banking &amp; Finance</td>
        <td>Belize</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Banco Nacional de Bolivia</td>
        <td>1871</td>
        <td>Banking &amp; Finance</td>
        <td>Bolivia, Plurinational State of</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Casa da Moeda do Brasil</td>
        <td>1694</td>
        <td>Manufacturing &amp; Production</td>
        <td>Brazil</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Mount Gay Rum</td>
        <td>1703</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Barbados</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Hua Ho Department Store</td>
        <td>1947</td>
        <td>Retail</td>
        <td>Brunei Darussalam</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Tashi Group</td>
        <td>1959</td>
        <td>Aviation &amp; Transport</td>
        <td>Bhutan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Botswana Meat Commission</td>
        <td>1965</td>
        <td>Agriculture</td>
        <td>Botswana</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Banque Internationale pour la Centrafrique</td>
        <td>1946</td>
        <td>Banking &amp; Finance</td>
        <td>Central African Republic</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Hudson&#x27;s Bay Company</td>
        <td>1670</td>
        <td>Retail</td>
        <td>Canada</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Gasthof Sternen</td>
        <td>1230</td>
        <td>Tourism &amp; Hotels</td>
        <td>Switzerland</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Famae</td>
        <td>1811</td>
        <td>Defense</td>
        <td>Chile</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Ma Yu Ching&#x27;s Bucket Chicken House</td>
        <td>1153</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>China</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>La Poste</td>
        <td>1945</td>
        <td>Postal Service</td>
        <td>Côte d&#x27;Ivoire</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Cameroon Development Corporation</td>
        <td>1947</td>
        <td>Agriculture</td>
        <td>Cameroon</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Société nationale des Chemins de fer du Congo</td>
        <td>1889</td>
        <td>Aviation &amp; Transport</td>
        <td>Congo, The Democratic Republic of the</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Development Bank of the Central African States</td>
        <td>1975</td>
        <td>Banking &amp; Finance</td>
        <td>Congo</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Casa de Moneda de Colombia</td>
        <td>1621</td>
        <td>Manufacturing &amp; Production</td>
        <td>Colombia</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Central Bank of the Comoros</td>
        <td>1981</td>
        <td>Banking &amp; Finance</td>
        <td>Comoros</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Correios de Cabo Verde</td>
        <td>1849</td>
        <td>Postal Service</td>
        <td>Cabo Verde</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Florida Ice and Farm Company</td>
        <td>1908</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Costa Rica</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Cubana de Aviación</td>
        <td>1929</td>
        <td>Aviation &amp; Transport</td>
        <td>Cuba</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Bank of Cyprus</td>
        <td>1899</td>
        <td>Banking &amp; Finance</td>
        <td>Cyprus</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Pivovar Broumov</td>
        <td>1348</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Czechia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Staffelter Hof Winery</td>
        <td>862</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Germany</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Ethio-Djibouti Railways</td>
        <td>1901</td>
        <td>Aviation &amp; Transport</td>
        <td>Djibouti</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>The Chronicle (Dominica)</td>
        <td>1909</td>
        <td>Media</td>
        <td>Dominica</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Munke Mølle</td>
        <td>1135</td>
        <td>Manufacturing &amp; Production</td>
        <td>Denmark</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Hamoud Boualem</td>
        <td>1878</td>
        <td>Food &amp; Beverages</td>
        <td>Algeria</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Egyptian National Railways</td>
        <td>1854</td>
        <td>Aviation &amp; Transport</td>
        <td>Egypt</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Asmara Brewery</td>
        <td>1939</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Eritrea</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Casa de Ganaderos</td>
        <td>1218</td>
        <td>Agriculture</td>
        <td>Spain</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Raeapteek</td>
        <td>1422</td>
        <td>Medical</td>
        <td>Estonia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>National Bank of Ethiopia</td>
        <td>1906</td>
        <td>Banking &amp; Finance</td>
        <td>Ethiopia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Fiskars</td>
        <td>1649</td>
        <td>Consumer Goods</td>
        <td>Finland</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Monnaie de Paris </td>
        <td>864</td>
        <td>Manufacturing &amp; Production</td>
        <td>France</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>BGFIBank Group</td>
        <td>1971</td>
        <td>Banking &amp; Finance</td>
        <td>Gabon</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>The Royal Mint</td>
        <td>886</td>
        <td>Manufacturing &amp; Production</td>
        <td>United Kingdom</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Bank of Georgia</td>
        <td>1903</td>
        <td>Banking &amp; Finance</td>
        <td>Georgia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Halco Mining</td>
        <td>1962</td>
        <td>Mining</td>
        <td>Guinea</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Correios da Guiné-Bissau</td>
        <td>1973</td>
        <td>Postal Service</td>
        <td>Guinea-Bissau</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Guinea Ecuatorial Airlines</td>
        <td>1996</td>
        <td>Aviation &amp; Transport</td>
        <td>Equatorial Guinea</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Piraeus Bank</td>
        <td>1606</td>
        <td>Banking &amp; Finance</td>
        <td>Greece</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Corporación Multi Inversiones</td>
        <td>1920</td>
        <td>Food &amp; Beverages</td>
        <td>Guatemala</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Banks DIH</td>
        <td>1840</td>
        <td>Food &amp; Beverages</td>
        <td>Guyana</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>National Railroad of Honduras</td>
        <td>1870</td>
        <td>Aviation &amp; Transport</td>
        <td>Honduras</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Kraljevica Shipyard</td>
        <td>1729</td>
        <td>Manufacturing &amp; Production</td>
        <td>Croatia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Rhum Barbancourt</td>
        <td>1862</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Haiti</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Zwack</td>
        <td>1790</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Hungary</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Pindad</td>
        <td>1808</td>
        <td>Defense</td>
        <td>Indonesia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Wadia Group</td>
        <td>1736</td>
        <td>Manufacturing &amp; Production</td>
        <td>India</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Sean&#x27;s Bar</td>
        <td>900</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>Ireland</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>North Oil Company</td>
        <td>1928</td>
        <td>Energy</td>
        <td>Iraq</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Íslandspóstur</td>
        <td>1776</td>
        <td>Postal Service</td>
        <td>Iceland</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Café Abu Salem</td>
        <td>1914</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>Israel</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Marinelli Bell Foundry</td>
        <td>1040</td>
        <td>Manufacturing &amp; Production</td>
        <td>Italy</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Rose Hall</td>
        <td>1770</td>
        <td>Tourism &amp; Hotels</td>
        <td>Jamaica</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Arab Bank</td>
        <td>1930</td>
        <td>Banking &amp; Finance</td>
        <td>Jordan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Kongō Gumi</td>
        <td>578</td>
        <td>Construction</td>
        <td>Japan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bogatyr Access Komir</td>
        <td>1913</td>
        <td>Mining</td>
        <td>Kazakhstan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>KCB Group Limited</td>
        <td>1896</td>
        <td>Banking &amp; Finance</td>
        <td>Kenya</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>National Bank of Cambodia</td>
        <td>1954</td>
        <td>Banking &amp; Finance</td>
        <td>Cambodia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>KT Corporation</td>
        <td>1885</td>
        <td>Telecommunications</td>
        <td>Korea, Republic of</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>M.H. Alshaya Co.</td>
        <td>1890</td>
        <td>Retail</td>
        <td>Kuwait</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Electricite du Laos</td>
        <td>1959</td>
        <td>Energy</td>
        <td>Lao People&#x27;s Democratic Republic</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bank Audi</td>
        <td>1830</td>
        <td>Banking &amp; Finance</td>
        <td>Lebanon</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>National Port Authority</td>
        <td>1921</td>
        <td>Aviation &amp; Transport</td>
        <td>Liberia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Umma Bank</td>
        <td>1907</td>
        <td>Banking &amp; Finance</td>
        <td>Libya</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>1st National Bank of St Lucia</td>
        <td>1938</td>
        <td>Banking &amp; Finance</td>
        <td>Saint Lucia</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>National Bank of Liechtenstein</td>
        <td>1861</td>
        <td>Banking &amp; Finance</td>
        <td>Liechtenstein</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>George Steuart Group</td>
        <td>1835</td>
        <td>Food &amp; Beverages</td>
        <td>Sri Lanka</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Central Bank of Lesotho</td>
        <td>1978</td>
        <td>Banking &amp; Finance</td>
        <td>Lesotho</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Gubernija</td>
        <td>1665</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Lithuania</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Mousel</td>
        <td>1511</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Luxembourg</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Cēsu Alus</td>
        <td>1590</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Latvia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Attijariwafa Bank</td>
        <td>1904</td>
        <td>Banking &amp; Finance</td>
        <td>Morocco</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Air Madagascar</td>
        <td>1962</td>
        <td>Aviation &amp; Transport</td>
        <td>Madagascar</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>La Casa de Moneda de México</td>
        <td>1534</td>
        <td>Manufacturing &amp; Production</td>
        <td>Mexico</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Tutunski kombinat Prilep</td>
        <td>1873</td>
        <td>Consumer Goods</td>
        <td>North Macedonia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Office de Radiodiffusion-Télévision du Mali</td>
        <td>1957</td>
        <td>Media</td>
        <td>Mali</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>HSBC Bank Malta</td>
        <td>1882</td>
        <td>Banking &amp; Finance</td>
        <td>Malta</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Myanmar National Airlines</td>
        <td>1948</td>
        <td>Aviation &amp; Transport</td>
        <td>Myanmar</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Pošta Crne Gore</td>
        <td>1841</td>
        <td>Postal Service</td>
        <td>Montenegro</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Mongolian National Broadcaster</td>
        <td>1931</td>
        <td>Media</td>
        <td>Mongolia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Beira Railroad Corporation</td>
        <td>1892</td>
        <td>Aviation &amp; Transport</td>
        <td>Mozambique</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Central Bank of Mauritania</td>
        <td>1973</td>
        <td>Banking &amp; Finance</td>
        <td>Mauritania</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Mauritius Post</td>
        <td>1772</td>
        <td>Postal Service</td>
        <td>Mauritius</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Malawi Broadcasting Corporation</td>
        <td>1964</td>
        <td>Media</td>
        <td>Malawi</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Pos Malaysia</td>
        <td>1800</td>
        <td>Postal Service</td>
        <td>Malaysia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>NamPost</td>
        <td>1814</td>
        <td>Postal Service</td>
        <td>Namibia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Office of Radio and Television of Niger</td>
        <td>1960</td>
        <td>Media</td>
        <td>Niger</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>First Bank of Nigeria</td>
        <td>1894</td>
        <td>Banking &amp; Finance</td>
        <td>Nigeria</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Flor de Caña</td>
        <td>1890</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Nicaragua</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>The Brand Brewery</td>
        <td>1340</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Netherlands</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Posten Norge</td>
        <td>1647</td>
        <td>Postal Service</td>
        <td>Norway</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Nepal Bank Limited</td>
        <td>1937</td>
        <td>Banking &amp; Finance</td>
        <td>Nepal</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bank of New Zealand</td>
        <td>1861</td>
        <td>Banking &amp; Finance</td>
        <td>New Zealand</td>
        <td>Oceania</td>
    </tr>
    <tr>
        <td>Petroleum Development Oman</td>
        <td>1937</td>
        <td>Energy</td>
        <td>Oman</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>House of Habib</td>
        <td>1841</td>
        <td>Conglomerate</td>
        <td>Pakistan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>National Bank of Panama</td>
        <td>1904</td>
        <td>Banking &amp; Finance</td>
        <td>Panama</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Casa Nacional de Moneda</td>
        <td>1565</td>
        <td>Banking &amp; Finance</td>
        <td>Peru</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Destileria Limtuaco</td>
        <td>1853</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Philippines</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bochnia Salt Mine</td>
        <td>1248</td>
        <td>Mining</td>
        <td>Poland</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Kim Chong-t&#x27;ae Electric Locomotive Works</td>
        <td>1945</td>
        <td>Aviation &amp; Transport</td>
        <td>Korea, Democratic People&#x27;s Republic of</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>CTT-Correios de Portugal SA</td>
        <td>1520</td>
        <td>Postal Service</td>
        <td>Portugal</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Salam International Investment Limited</td>
        <td>1952</td>
        <td>Conglomerate</td>
        <td>Qatar</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Ursus Breweries</td>
        <td>1878</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Romania</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Petrodvorets Watch Factory</td>
        <td>1721</td>
        <td>Consumer Goods</td>
        <td>Russian Federation</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>National Post Office</td>
        <td>1922</td>
        <td>Postal Service</td>
        <td>Rwanda</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>House of Alireza</td>
        <td>1845</td>
        <td>Construction</td>
        <td>Saudi Arabia</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bank of Khartoum</td>
        <td>1913</td>
        <td>Banking &amp; Finance</td>
        <td>Sudan</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Dakar–Niger Railway</td>
        <td>1924</td>
        <td>Aviation &amp; Transport</td>
        <td>Senegal</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Singapore Post</td>
        <td>1819</td>
        <td>Postal Service</td>
        <td>Singapore</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Rokel Commercial Bank</td>
        <td>1917</td>
        <td>Banking &amp; Finance</td>
        <td>Sierra Leone</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>HSBC El Salvador</td>
        <td>1891</td>
        <td>Banking &amp; Finance</td>
        <td>El Salvador</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Radio Mogadishu</td>
        <td>1943</td>
        <td>Media</td>
        <td>Somalia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Apatin Brewery</td>
        <td>1756</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Serbia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Ivory Bank</td>
        <td>1994</td>
        <td>Banking &amp; Finance</td>
        <td>South Sudan</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Central Bank of São Tomé and Príncipe</td>
        <td>1975</td>
        <td>Banking &amp; Finance</td>
        <td>Sao Tome and Principe</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Kremnica Mint</td>
        <td>1328</td>
        <td>Manufacturing &amp; Production</td>
        <td>Slovakia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Gostilna Gastuž</td>
        <td>1467</td>
        <td>Tourism &amp; Hotels</td>
        <td>Slovenia</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Skyllbergs bruk</td>
        <td>1346</td>
        <td>Manufacturing &amp; Production</td>
        <td>Sweden</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Swazi Rail</td>
        <td>1963</td>
        <td>Aviation &amp; Transport</td>
        <td>Eswatini</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Air Seychelles</td>
        <td>1977</td>
        <td>Aviation &amp; Transport</td>
        <td>Seychelles</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bakdash</td>
        <td>1885</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>Syrian Arab Republic</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Cotontchad</td>
        <td>1971</td>
        <td>Agriculture</td>
        <td>Chad</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>La Poste du Togo</td>
        <td>1883</td>
        <td>Postal Service</td>
        <td>Togo</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>B.Grimm Group</td>
        <td>1878</td>
        <td>Conglomerate</td>
        <td>Thailand</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>House of Angostura</td>
        <td>1830</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Trinidad and Tobago</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>La Poste Tunisienne</td>
        <td>1847</td>
        <td>Postal Service</td>
        <td>Tunisia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Çemberlitaş Hamamı</td>
        <td>1584</td>
        <td>Tourism &amp; Hotels</td>
        <td>Turkey</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Bank of Taiwan</td>
        <td>1897</td>
        <td>Banking &amp; Finance</td>
        <td>Taiwan, Province of China</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Tanzania Breweries Limited</td>
        <td>1933</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>Tanzania, United Republic of</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Stanbic Bank Uganda Limited</td>
        <td>1906</td>
        <td>Banking &amp; Finance</td>
        <td>Uganda</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Drohobych salt plant</td>
        <td>1250</td>
        <td>Manufacturing &amp; Production</td>
        <td>Ukraine</td>
        <td>Europe</td>
    </tr>
    <tr>
        <td>Cafe Brasilero</td>
        <td>1877</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>Uruguay</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Shirley Plantation</td>
        <td>1638</td>
        <td>Agriculture</td>
        <td>United States</td>
        <td>North America</td>
    </tr>
    <tr>
        <td>Tashkent Aviation Production Association</td>
        <td>1932</td>
        <td>Manufacturing &amp; Production</td>
        <td>Uzbekistan</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Hacienda Chuao</td>
        <td>1660</td>
        <td>Food &amp; Beverages</td>
        <td>Venezuela, Bolivarian Republic of</td>
        <td>South America</td>
    </tr>
    <tr>
        <td>Vietnam Railways</td>
        <td>1881</td>
        <td>Aviation &amp; Transport</td>
        <td>Viet Nam</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>European Trust Company</td>
        <td>1991</td>
        <td>Banking &amp; Finance</td>
        <td>Vanuatu</td>
        <td>Oceania</td>
    </tr>
    <tr>
        <td>Yemenia Airways</td>
        <td>1962</td>
        <td>Aviation &amp; Transport</td>
        <td>Yemen</td>
        <td>Asia</td>
    </tr>
    <tr>
        <td>Premier FMCG</td>
        <td>1820</td>
        <td>Manufacturing &amp; Production</td>
        <td>South Africa</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>ZamPost</td>
        <td>1896</td>
        <td>Postal Service</td>
        <td>Zambia</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Standard Chartered Zimbabwe</td>
        <td>1892</td>
        <td>Banking &amp; Finance</td>
        <td>Zimbabwe</td>
        <td>Africa</td>
    </tr>
    <tr>
        <td>Meridian Corporation</td>
        <td>1999</td>
        <td>Media</td>
        <td>Kosovo</td>
        <td>Europe</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (163, 5)
    except AssertionError:
        assert False, "The results should have five columns and one hundred and sixty three rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['business', 'year_founded', 'category', 'country', 'continent']
    except AssertionError:
        assert False, "The results should have columns named business, year_founded, category, country, and continent."  
```






    3/3 tests passed




## 8. Counting categories by continent
<p>Having <code>businesses</code> joined to <code>categories</code> and <code>countries</code> together means we can ask questions about both these things together. For example, which are the most common categories for the oldest businesses on each continent?</p>


```python
%%sql
SELECT cnt.continent, cat.category, COUNT(*) AS n
    FROM businesses AS bus
    INNER JOIN categories AS cat
        ON bus.category_code = cat.category_code
    INNER JOIN countries AS cnt
        ON bus.country_code = cnt.country_code
    GROUP BY cnt.continent, cat.category;

```

     * postgresql:///oldestbusinesses
    56 rows affected.





<table>
    <tr>
        <th>continent</th>
        <th>category</th>
        <th>n</th>
    </tr>
    <tr>
        <td>North America</td>
        <td>Banking &amp; Finance</td>
        <td>4</td>
    </tr>
    <tr>
        <td>Oceania</td>
        <td>Postal Service</td>
        <td>1</td>
    </tr>
    <tr>
        <td>South America</td>
        <td>Food &amp; Beverages</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Tourism &amp; Hotels</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Media</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Medical</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Defense</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Manufacturing &amp; Production</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Postal Service</td>
        <td>4</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Aviation &amp; Transport</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>2</td>
    </tr>
    <tr>
        <td>South America</td>
        <td>Banking &amp; Finance</td>
        <td>3</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Food &amp; Beverages</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Manufacturing &amp; Production</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Postal Service</td>
        <td>9</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Telecommunications</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Food &amp; Beverages</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Consumer Goods</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Mining</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Oceania</td>
        <td>Banking &amp; Finance</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Agriculture</td>
        <td>1</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Manufacturing &amp; Production</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Mining</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Aviation &amp; Transport</td>
        <td>10</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Construction</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Energy</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Retail</td>
        <td>3</td>
    </tr>
    <tr>
        <td>South America</td>
        <td>Manufacturing &amp; Production</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Banking &amp; Finance</td>
        <td>5</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Banking &amp; Finance</td>
        <td>17</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Tourism &amp; Hotels</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Banking &amp; Finance</td>
        <td>6</td>
    </tr>
    <tr>
        <td>South America</td>
        <td>Defense</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Energy</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Conglomerate</td>
        <td>3</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Media</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Agriculture</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Mining</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Aviation &amp; Transport</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Food &amp; Beverages</td>
        <td>2</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>5</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Agriculture</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>3</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Agriculture</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Postal Service</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Defense</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Manufacturing &amp; Production</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Telecommunications</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>12</td>
    </tr>
    <tr>
        <td>South America</td>
        <td>Cafés, Restaurants &amp; Bars</td>
        <td>1</td>
    </tr>
    <tr>
        <td>North America</td>
        <td>Retail</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Media</td>
        <td>4</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Media</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Tourism &amp; Hotels</td>
        <td>1</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (56, 3)
    except AssertionError:
        assert False, "The results should have three columns and fifty six rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['continent', 'category', 'n']
    except AssertionError:
        assert False, "The results should have continent, category, and count (as 'n')."
def test_count():
    try:
        assert results.loc[:, 'n'].sort_values(ascending=False).values.tolist() == [17, 12, 10, 9, 8, 7, 6, 5, 5, 4, 4, 4, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
    except AssertionError:
        assert False, "The counts are not correct."
```






    4/4 tests passed




## 9. Filtering counts by continent and category
<p>Combining continent and business category led to a lot of results. It's difficult to see what is important. To trim this down to a manageable size, let's restrict the results to only continent/category pairs with a high count.</p>


```python
%%sql
SELECT cnt.continent, cat.category, COUNT(*) AS n
    FROM businesses AS bus
    INNER JOIN categories AS cat
        ON bus.category_code = cat.category_code
    INNER JOIN countries AS cnt
        ON bus.country_code = cnt.country_code
    GROUP BY cnt.continent, cat.category
    HAVING COUNT(*) > 5
    ORDER BY n DESC;
```

     * postgresql:///oldestbusinesses
    7 rows affected.





<table>
    <tr>
        <th>continent</th>
        <th>category</th>
        <th>n</th>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Banking &amp; Finance</td>
        <td>17</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Distillers, Vintners, &amp; Breweries</td>
        <td>12</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Aviation &amp; Transport</td>
        <td>10</td>
    </tr>
    <tr>
        <td>Africa</td>
        <td>Postal Service</td>
        <td>9</td>
    </tr>
    <tr>
        <td>Europe</td>
        <td>Manufacturing &amp; Production</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Aviation &amp; Transport</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Asia</td>
        <td>Banking &amp; Finance</td>
        <td>6</td>
    </tr>
</table>




```python
%%nose
last_output = _

def test_resultset():
    try:
        assert str(type(last_output)) == "<class 'sql.run.ResultSet'>"
    except AssertionError:
        assert False, "Please ensure a SQL ResultSet is the output of the code cell." 
results = last_output.DataFrame()
def test_shape():
    try:
        assert results.shape == (7, 3)
    except AssertionError:
        assert False, "The results should have three columns and seven rows."
def test_colnames():
    try:
        assert results.columns.tolist() == ['continent', 'category', 'n']
    except AssertionError:
        assert False, "The results should have continent, category, and count (as 'n')."
def test_count():
    try:
        assert results.loc[:, 'n'].values.tolist() == [17, 12, 10, 9, 8, 7, 6]
    except AssertionError:
        assert False, "The counts are not correct."
```






    4/4 tests passed



