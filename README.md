# SecureLogin
Multifactor Authentication Using QR code and Username/Password

In this project I'm going to build secure login system with database for clients.I performed two major task in this project that are multifactor authentication and encrption of user password. first create new database and fill it with data like username and password
then create server script and coonect it to the database and write client script which is use to send request to the server and server check the availibilty of username and password from the database and then server give access to the client login or not to reduce the major vulnerabilities.


## Features

- Secure login system
- Apply multi factor authentication
- encode decode user password


## Deployment

Liberaries which are use in this project

```bash
  import sqlite3
  
```
This library is use to create database tables colums and manage qurries and manage SQLite databse file

```bash
import hashlib
  
```
## Create Sample database
create database and store username and password into the database

```bash
conn = sqlite3.connect("userdata.db")
cur = conn.cursor()

cur.execute("""
CREATE TABLE IF NOT EXISTS userdata (
    id INTEGER PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
)
""")
username1, password1 = "nuralnine", hashlib.sha256("nuralpassword".encode()).hexdigest()
username2, password2 = "Fatima", hashlib.sha256("fatma0123".encode()).hexdigest()
username3, password3 = "Fatmaa", hashlib.sha256("fatma1023".encode()).hexdigest()
username4, password4 = "Fatmah", hashlib.sha256("fatma1203".encode()).hexdigest()
cur.execute("INSERT INTO userdata (username, password) VALUES(?, ?)", (username1, password1))
cur.execute("INSERT INTO userdata (username, password) VALUES(?, ?)", (username2, password2))
cur.execute("INSERT INTO userdata (username, password) VALUES(?, ?)", (username3, password3))
cur.execute("INSERT INTO userdata (username, password) VALUES(?, ?)", (username4, password4))
conn.commit()
  
```

## Create Server
In this section create server and connect it to the databse  

```bash
    password = c.recv(1024)
    password = hashlib.sha256(password).hexdigest()

    conn = sqlite3.connect("userdata.db")
    cur = conn.cursor()


    cur.execute("SELECT * FROM userdata WHERE username = ?", (username, password))

    if cur.fetchall():
        c.send("Login successful".encode())

        #secrets
        #service
    else:
        c.send("Login faild".encode())



while True:
    client, adder = server.accept()
    threading.Thread(target=handle_connection, args=(client)).start()
  
```


## Create Client 
Create client side script and connect it to the database

```bash
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("localhost", 9999))

message = client.recv(1024).decode()
client.send(input((message).encode()))
message = client.recv(1024).decode()
client.send(input(message).encode())
print(client.recv(1024).decode())
  
```

## QR code authentication
add QR code authenticationto make online system more secure
```bash
import time
import pyotp
import qrcode


class authenticator:
    def __init__(self):
        self.user = "2faauthenticationforpython"
        self.username = 'python'
        self.password = 'test'
        self.gnrtr = pyotp.totp.TOTP(self.user).provisioning_uri(
            name='python_test',
            issuer_name='python_2fa')

    def genQrcode(self):
        qrcode.make(self.gnrtr).save("qrcode.png")
  
```

## Output

![Capture1](https://user-images.githubusercontent.com/63055500/208398813-5d51a088-edfe-43c5-abf2-32e3f716c052.PNG)
![Capture2](https://user-images.githubusercontent.com/63055500/208398899-ec22b0ef-fc63-4703-8712-0d10713870cb.PNG)


![Capture3](https://user-images.githubusercontent.com/63055500/208400551-96e019a3-999a-46bd-ab82-8876c92e5360.PNG)
