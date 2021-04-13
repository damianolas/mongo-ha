# Premessa
Installazione di un cluster MongoDB di 3 nodi, su cloud AWS e OS AmazonLinux2.
Si prevede accesso dall'esterno su porta 27017.

# Procedura
- Crea security group per le istanze (27017 aperta per il gruppo stesso, per il Bastion e per i server applicativi che ne hanno bisogno)
- Crea EC2 AmazonLinux2  

Installa mongoDB (https://docs.mongodb.com/manual/tutorial/install-mongodb-on-amazon/)  
- Consenti l'accesso dall'esterno:
  ``` 
  sudo vim /etc/mongod.conf
  ```
  e imposta:
  ```
  bindIp: 0.0.0.0
  ```
- Crea AMI del server
- Utilizza l'AMI creata per il setup degli altri due nodi MongoDB-secondary  

Inizia il deploy del Replica Set (https://docs.mongodb.com/manual/tutorial/deploy-replica-set/)
- Testa la connettività tra i tre nodi, mettendo in uno dei server, gli IP degli altri due. Se non si riscontrano errori, si farà l'accesso alla shell di Mongo del nodo indicato:
  ``` 
  mongo --host <IP>
  ```
- Valorizza la replication nel file di config (TUTTI I NODI):
  ``` 
  sudo vim /etc/mongod.conf  
  ```
  ``` 
  replication:
   replSetName: "rs0"
  ```
- Riavvia il servizio (TUTTI I NODI):
  ```
  sudo systemctl restart mongod
  ```
- Connettiti al mongo master:
  ```
  mongo
  ```
- Lancia il comando di replica:
  ```
  rs.initiate( {
   _id : "rs0",
   members: [
      { _id: 0, host: "<IP-nodo>:27017" },
      { _id: 1, host: "<IP-nodo>:27017" },
      { _id: 2, host: "<IP-nodo>:27017" }
   ]
  })
  ```
- Check replica:
  ```
  rs.status()
  rs.conf()
  ```
- Aggiungi l'utente Admin:  
  ```
  use admin
  db.createUser(
    {
      user: "Admin",
      pwd: "PASSWORD",
      roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
    }
  )
  ```
- Aggiungi eventuali altri utenti
