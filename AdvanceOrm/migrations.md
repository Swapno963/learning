Migrations can be reversed with migrate by passing the number of the previous migration. 

You have not manually edited your database - Django won’t be able to detect that your database doesn’t match your models, you’ll just get errors when migrations try to modify those tables.

You have not changed your models since you made their tables. For migrations to work, you must make the initial migration first and then make changes, as Django compares changes against migration files, not the database.