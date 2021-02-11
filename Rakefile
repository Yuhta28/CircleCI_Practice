namespace :mysql do
    task :default => [:create, :migrate, :drop]
  
    MYSQL_ROOT_PASSWORD = ""
    MYSQL_DATABASE = "yuta-db"
    def get_mysql_dbname()
      if DBNAME && !DBNAME.empty? then
        return DBNAME
      elsif ENV.include?(MYSQL_DATABASE) then
        return ENV[MYSQL_DATABASE]
      else
        puts "input database name"
        dbname = STDIN.gets
        ENV[MYSQL_DATABASE] = dbname.chop
        return ENV[MYSQL_DATABASE]
      end
    end
  
    ENV[MYSQL_ROOT_PASSWORD] = PASSWORD
    def get_mysql_password()
      pass = ENV[MYSQL_ROOT_PASSWORD]
      if pass && !pass.empty? then
        return pass
      else
        puts "input database password"
        system "stty -echo"
        pass = STDIN.gets
        system "stty echo"
        ENV[MYSQL_ROOT_PASSWORD] =  pass.chop
        return ENV[MYSQL_ROOT_PASSWORD]
      end
    end
  
    desc "create database"
    task :create do
      dbname = get_mysql_dbname()
      password = get_mysql_password()
      sh "#{MYPATH}mysql -u #{MYUSER} -e \"create database #{dbname}\""
    end
  
    desc "drop database"
    task :drop do
      dbname = get_mysql_dbname()
      password = get_mysql_password()
      sh "#{MYPATH}mysql -u #{MYUSER} -e \"drop database #{dbname}\""
    end
  
    desc "migrate database"
    task :migrate do
      dbname = get_mysql_dbname()
      password = get_mysql_password()
      sh "#{MYPATH}mysql -u #{MYUSER} -D #{dbname} < #{MIGRATE_DDL}"
    end
  
    desc "show databases"
    task :databases do
      dbname = get_mysql_dbname()
      password = get_mysql_password()
      sh "#{MYPATH}mysql -u #{MYUSER} -e \"show databases;\""
    end
  
    def mysql_exec(q)
      dbname = get_mysql_dbname()
      password = get_mysql_password()
      sh "#{MYPATH}mysql -u #{MYUSER} -D #{dbname} -e \"#{q}\""
    end
  
    desc "execute query (mysql:exec[\"QUERY\"])"
    task :exec, "query" do |x, args|
      mysql_exec(args.query)
    end
   
    desc "show tables"
    task :tables  do
      mysql_exec("show tables")
    end
  
    desc "describe table (mysql:desc[TABLENAME])"
    task :desc, "table_name" do |x, args|
      mysql_exec("desc #{args.table_name}")
    end
  end