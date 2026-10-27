---server.py

import socket, time

s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
s.bind(("0.0.0.0",1234))
print("Time Server Running...")

while True:
    data,addr=s.recvfrom(1024)
    msg=data.decode()
    if msg.lower()=="stop":
        s.sendto("Server stopped".encode(),addr)
        break
    elif msg.lower()=="time":
        s.sendto(time.ctime().encode(),addr)
    else:
        s.sendto("Invalid request".encode(),addr)
s.close()

---client.py

import socket

s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
addr=("127.0.0.1",1234)

while True:
    msg=input("Enter request (time/stop): ")
    s.sendto(msg.encode(),addr)
    data,addr=s.recvfrom(1024)
    print("Server:",data.decode())
    if msg.lower()=="stop": break
s.close()
