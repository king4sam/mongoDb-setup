# mongoDb-setup
###建立repMongoDb.sh
    vim repMongoDb.sh
------
    sudo apt-get update
    sudo apt-get install mongodb
    sudo service mongodb stop #暫時將mongo server關閉 
    sudo mongod #重新打開mongo server
    mkdir -p mongodb/rs0-0 mongodb/rs0-1 mongodb/rs0-2


###開啟三個終端機分別輸入
     mongod --port 27017 --dbpath /home/a901002666/mongodb/rs0-0 --replSet "rs0" --smallfiles --oplogSize 128 
     mongod --port 27018 --dbpath /home/a901002666/mongodb/rs0-1 --replSet "rs0" --smallfiles --oplogSize 128  
     mongod --port 27019 --dbpath /home/a901002666/mongodb/rs0-2 --replSet "rs0" --smallfiles --oplogSize 128 

###開啟第四個終端機,進入 27017 port的 mongodb=>
>mongo --port 27017

    rsconf = {
         _id: "rs0",
           members: [
                      {
                       _id: 0,
                       host: "<hostname>:27017"
                      }
                    ]
     }         
    rs.initiate(rsconf)
    rs.add("<hostname>:27018")
    rs.add("<hostname>:27019")
------
     mongod --port 27017 --dbpath /home/a901002666/mongodb/rs0-0 --replSet "rs0" --smallfiles --oplogSize 128 --fork --logpath  /home/a901002666/mongodb/rs0-0/mongodb.log
    
     mongod --port 27018 --dbpath /home/a901002666/mongodb/rs0-1 --replSet "rs0" --smallfiles --oplogSize 128 --fork --logpath  /home/a901002666/mongodb/rs0-1/mongodb.log
    
      mongod --port 27019 --dbpath /home/a901002666/mongodb/rs0-2 --replSet "rs0" --smallfiles --oplogSize 128 --fork --logpath  /home/a901002666/mongodb/rs0-2/mongodb.log

#####完成repelication set 設定 <hostname>:27017為primiry

    cfg = rs.conf()
    cfg.members[0].priority = 0
    cfg.members[0].hidden = true
    cfg.members[0].slaveDelay = 3600
    rs.reconfig(cfg)
======
>
reference:
http://docs.mongodb.org/master/tutorial/deploy-replica-set-for-testing/
http://docs.mongodb.org/master/tutorial/configure-a-delayed-replica-set-member/
>
ps: cat /etc/hostname 得到hostName
