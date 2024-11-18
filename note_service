import zmq

def note_service():
    context = zmq.Context()
    socket = context.socket(zmq.REP)  # Reply socket
    socket.bind("tcp://*:6000")

    notes = {}
    note_counter = 1  # Counter for generating unique note IDs

    while True:
        # Wait for the next request from the client
        message = socket.recv_json()
        action = message.get('action')

        if action == 'create':
            note_content = message.get('content')
            author_name = message.get('author')
            note_id = note_counter
            notes[note_id] = {"content": note_content, "author": author_name}
            note_counter += 1  # Increment for next unique ID
            socket.send_json({"note_id": note_id, "note": notes[note_id]})

        elif action == 'get':
            note_id = message.get('id')
            note = notes.get(note_id)
            if note:
                socket.send_json({"note": note})
            else:
                socket.send_json({"message": "Note not found."})

        elif action == 'delete':
            note_id = message.get('id')
            if note_id in notes:
                del notes[note_id]
                socket.send_json({"message": "Note deleted."})
            else:
                socket.send_json({"message": "Note not found."})

        elif action == 'replace':
            note_id = message.get('id')
            new_content = message.get('content')
            new_author = message.get('author')

            if note_id in notes:
                if new_content:
                    notes[note_id]["content"] = new_content
                if new_author:
                    notes[note_id]["author"] = new_author
                socket.send_json({"note": notes[note_id]})
            else:
                socket.send_json({"message": "Note not found."})

        else:
            socket.send_json({"message": "Unknown action"})

if __name__ == "__main__":
    note_service()
