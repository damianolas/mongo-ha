# Premessa
Installazione di un cluster MongoDB di 3 nodi, su cloud AWS e OS AmazonLinux2.
Si prevede accesso dall'esterno su porta 27017.

# Procedura
- Crea security group per le istanze (27017 aperta per il gruppo stesso, per il Bastion e per i server applicativi che ne hanno bisogno)
- Crea EC2 AmazonLinux2  
- Installa mongoDB (https://docs.mongodb.com/manual/tutorial/install-mongodb-on-amazon/)  
- Consenti l'accesso dall'esterno:
  ``` 
  sudo vim /etc/mongod.conf
  ```
  e imposta:
  ```
  bindIp: 0.0.0.0
  ```
- 
