# Evohome Temperature Site working on Synology Diskstation
Log Evohome temperature and host graph on own site

This is the edited version of https://github.com/PieterVO/EvohomeTemperatureSite working on a Synology Diskstation
Fix: I changed the moduele MySQLdb to pymysql and rewrite the instalation steps.


  - Prepare te synology
      - (ssh to Synology diskstation)
      - sudo su
      - sudo curl -k https://bootstrap.pypa.io/get-pip.py | python
      - pip install requests
      - pip install pymysql
      - pip install EvohomeClient
  
  For some reason, after updating DSM the requrements has to be installed again. 

  - Edit Cronjob
      - sudo vi /etc/crontab (i = insert)
        - add: */5 * * * * root    /pathto/TemperatureLogging.py >> /pathto/cron.log
        - (exit with ctrl+c > : > wq!)
      - sudo /usr/syno/sbin/synoservicectl --restart crond


  - Sset up your webserver and create a mysql database.
  - You need to have a column with the name "Time" and default value CURRENT_TIMESTAMP (Use a capital for Time!)
  - You also need to have a column for each room in your house (it's easy if you name the columns the same like you did for the rooms in the evohome system)
  
  - Then you need to edit 3 files
    - DataLoader.php
      - line 3: correct username, password, name of the database, port,...
      - line 13: correct tablename
    - TemperatureLogging.py
      - line 12: evohome username and password
      - line 14: database information
      - line 22 to ...: fill in correct room names, you can add more rooms if you want (also add them on line 32)
      - line 32: correct room names and table name (rename EvohomeTemperatures to the correct name in your SQL server)
    - index.html
      - line 19: correct http adress for DataLoader.php
      - line 39 to ...: fill in correct room names (title and valueField), also add the rooms here and choose different color (lineColor)
