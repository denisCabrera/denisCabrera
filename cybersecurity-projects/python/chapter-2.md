---
cover: >-
  https://images.unsplash.com/photo-1550751827-4bd374c3f58b?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjeWJlcnxlbnwwfHx8fDE2ODQwNDg2MzZ8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Chapter 2

* Created simple tcp client
* Created simple udp client
* Created simple tcp server

{% tabs %}
{% tab title="TCP Client" %}
```python
import socket

target_host = "127.0.0.1"
target_port = 9998

# 1 create a socket object
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2 connect the client
client.connect((target_host, target_port))

# 3 send some data
client.send(b'GET / HTTP/1.1\r\nHost: www.google.com\r\n\r\n')

# 4 receive some data
response = client.recv(4096)

print(response.decode())
client.close()
```
{% endtab %}

{% tab title="TCP Server" %}
```python
import socket
import threading

IP = "0.0.0.0"
PORT = 9998


def main():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind((IP, PORT))
    server.listen(5)
    print(f'[*] Listening on {IP}:{PORT}')

    while True:
        client, address = server.accept()
        print(f'[*] Accepted connection from {address[0]}:{address[1]}')
        client_handler = threading.Thread(target=handle_client, args=(client,))
        client_handler.start()


def handle_client(client_socket):
    with client_socket as sock:
        request = sock.recv(1024)
        print(f'[*] Received: {request.decode("utf-8")}')
        sock.send(b'ACK')


if __name__ == '__main__':
    main()
```
{% endtab %}

{% tab title="UDP Client" %}
```python
import socket

target_host = "127.0.0.1"
target_port = 9997

# 1 create a socket object
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# 2 send some data
client.sendto(b"AAABBBCCC", (target_host, target_port))


# 3 receive some data
data, addr = client.recvfrom(4096)

print(data.decode())
client.close()
```
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/Screenshot from 2023-05-14 03-12-29 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot from 2023-05-14 02-40-55 (1).png" alt=""><figcaption><p>Received connection from host</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot from 2023-05-14 02-39-42 (1).png" alt=""><figcaption><p>ACK connection from the server</p></figcaption></figure>

Seitz, J., & Arnold, T. (2021). Black Hat Python, 2nd Edition: Python Programming for Hackers and Pentesters. No Starch Press.
