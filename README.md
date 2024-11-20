CS361

**REQUEST:**
The note_service provides functionality to create, retrieve, update, and delete notes.
zeroMQ socket to connect the service running on the PORT 6000
context = zmq.Context()
    socket = context.socket(zmq.REQ)  # Request socket
    socket.connect("tcp://localhost:6000")
For example: Request to create a note
import zmq

context = zmq.Context()
socket = context.socket(zmq.REQ)
socket.connect("tcp://localhost:6000")

request = {
    "action": "create",
    "content": "My first note",
    "author": "John Doe"
}

socket.send_json(request)
response = socket.recv_json()
print(response)

**RECEIVE:**
Use recv_json to receive responses from the service
For example: Receiving a Response to Retrieve a Note
request = {
    "action": "get",
    "id": 1
}

socket.send_json(request)
response = socket.recv_json()
print(response)  
