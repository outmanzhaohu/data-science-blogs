# Comparing performance of sqlite and pandas

Depending on the task, you may need to choose one tool over another for
performance considerations. Below, we compare Python's `pandas` to `sqlite` for
some common data analysis operations: sort, select, load, join, filter, and
group by.

<div class='tableauPlaceholder' id='viz1487029455779' style='position: relative'><noscript><a href='#'><img alt='Comparing performance of sqlite vs. pandas ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Co&#47;Comparingperformanceofsqliteandpython-pandas&#47;Sheet1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='site_root' value='' /><param name='name' value='Comparingperformanceofsqliteandpython-pandas&#47;Sheet1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Co&#47;Comparingperformanceofsqliteandpython-pandas&#47;Sheet1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1487029455779');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>

# Analysis details

For the analysis, we ran our six tasks 10 times each, for 5 different sample
sizes, for each of 3 programs: `pandas`, `sqlite`, and `memory-sqlite` (where
database is in memory instead of on disk). See below for the definitions of
each task.


# Takeaways

sqlite or memory-sqlite is faster for the following tasks:

  * `select` two columns from data (> 10-100x faster. pandas is roughly
    constant time)
  * `filter` data (10x-50x faster)
  * `sort` by single column: pandas is always a bit slower, but this was the
    closest


pandas is faster for the following tasks:

  * `groupby` computation of a mean and sum (10x faster, but slower for
    smallest data)
  * `load` data from disk (5-10x faster, less difference for larger data)
  * `join` data (5x faster, but slower for smallest data)


Comparing memory-sqlite vs. sqlite, there was no meaningful difference,
especially as data size increased.
  
There is no significant speedup from loading sqlite in its own shell vs. via pandas


# Definitions and code

All code is on our github page: **TODO GITHUB LINK**.

Here are the exact definitions of our six tasks: sort, select, load, join,
filter, and group by (see `driver/sqlite_driver.py` or
`driver/pandas_driver.py`).

Sqlite is first, followed by pandas:

	def sort(self):
	  self._cursor.execute('SELECT * FROM employee ORDER BY name ASC;')
	  self._conn.commit()
	
	def sort(self):
	  self.df_employee.sort_values(by='name')


	def select(self):
	  self._cursor.execute('SELECT name, dept FROM employee;')
	  self._conn.commit()
	
	def select(self):
	  self.df_employee[["name", "dept"]]


	def load(self):
	  self._cursor.execute('CREATE TABLE employee (name varchar(255), dept char(1), birth int, salary double);')
	  df = pd.read_csv(self.employee_file)
	  df.columns = employee_columns
	  df.to_sql('employee', self._conn, if_exists='replace')
	
	  self._cursor.execute('CREATE TABLE bonus (name varchar(255), bonus double);')
	  df_bonus = pd.read_csv(self.bonus_file)
	  df_bonus.columns = bonus_columns
	  df_bonus.to_sql('bonus', self._conn, if_exists='replace')
	
	def load(self):
	  self.df_employee = pd.read_csv(self.employee_file)
	  self.df_employee.columns = employee_columns
	
	  self.df_bonus = pd.read_csv(self.bonus_file)
	  self.df_bonus.columns = bonus_columns


	def join(self):
	  self._cursor.execute('SELECT employee.name, employee.salary + bonus.bonus '
	                       'FROM employee INNER JOIN bonus ON employee.name = bonus.name')
	  self._conn.commit()
	
	def join(self):
	  joined = self.df_employee.merge(self.df_bonus, on='name')
	  joined['total'] = joined['bonus'] + joined['salary']


	def filter(self):
	  self._cursor.execute('SELECT * FROM employee WHERE dept = "a";')
	  self._conn.commit()
	
	def filter(self):
	  self.df_employee[self.df_employee['dept'] == 'a']


	def groupby(self):
	  self._cursor.execute('SELECT avg(birth), sum(salary) FROM employee GROUP BY dept;')
	  self._conn.commit()
	
	def groupby(self):
	  self.df_employee.groupby("dept").agg({'birth': np.mean, 'salary': np.sum})


# Details

pandas version: 0.19.1

sqlite version: 3.13.0

Test ran on Digital Ocean Ubuntu 14.04, 16GB memory, 8 core CPU.