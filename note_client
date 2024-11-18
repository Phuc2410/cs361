import zmq

def client():
    context = zmq.Context()
    socket = context.socket(zmq.REQ)  # Request socket
    socket.connect("tcp://localhost:6000")

    print("Welcome to the Note-Taking App! Capture and store important information easily.")

    while True:
        print("\nOptions:")
        print("1. Create a new note")
        print("2. Get a note by ID")
        print("3. Delete a note by ID")
        print("4. Replace note content or author by ID")
        print("5. Exit")

        option = int(input("Choose an option (1-5): "))

        if option == 1:
            note_content = input("Enter the note content: ")
            author_name = input("Enter the author name: ")
            socket.send_json({"action": "create", "content": note_content, "author": author_name})
            response = socket.recv_json()
            print("Create Note Response:", response)

        elif option == 2:
            note_id = int(input("Enter the note ID to retrieve: "))
            socket.send_json({"action": "get", "id": note_id})
            response = socket.recv_json()
            print("Get Note Response:", response)

        elif option == 3:
            note_id = int(input("Enter the note ID to delete: "))
            while True:
                confirmation = input("Are you sure? You will lose all your progress. (yes/no): ").lower()
                if confirmation == "yes":
                    socket.send_json({"action": "delete", "id": note_id})
                    response = socket.recv_json()
                    print("Response:", response)
                    break
                elif confirmation == "no":
                    print("Delete action canceled.")
                    break
                else:
                    print("Invalid input. Please enter 'yes' or 'no'.")

        elif option == 4:
            note_id = int(input("Enter the note ID to modify: "))
            print("What would you like to replace?")
            print("1. Content")
            print("2. Author")
            print("3. Both Content and Author")
            replace_choice = int(input("Choose an option (1-3): "))

            if replace_choice == 1:
                new_content = input("Enter the new content: ")
                socket.send_json({"action": "replace", "id": note_id, "content": new_content})
            elif replace_choice == 2:
                new_author = input("Enter the new author name: ")
                socket.send_json({"action": "replace", "id": note_id, "author": new_author})
            elif replace_choice == 3:
                new_content = input("Enter the new content: ")
                new_author = input("Enter the new author name: ")
                socket.send_json({"action": "replace", "id": note_id, "content": new_content, "author": new_author})
            else:
                print("Invalid option. Returning to main menu.")
                continue

            response = socket.recv_json()  # Get the response after the replacement
            print("Replace Note Response:", response)

        elif option == 5:
            print("Exiting... Goodbye!")
            break

        else:
            print("Invalid option. Please try again.")

if __name__ == "__main__":
    client()
