import os
import time

class MonitoredFile:
    def __init__(self, name):
        self.name = name
        self.last_modified = None
   
    def get_info(self):
        return f"File: {self.name}, Last modified: {self.last_modified}"

class MonitoredFolder:
    def __init__(self, folder_path):
        self.folder_path = folder_path
        self.files = {}
   
    def update_status(self):
        for filename in os.listdir(self.folder_path):
            file_path = os.path.join(self.folder_path, filename)
            if os.path.isfile(file_path):
                if filename not in self.files:
                    self.files[filename] = MonitoredFile(filename)
                self.files[filename].last_modified = time.ctime(os.path.getmtime(file_path))
   
    def commit(self):
        self.update_status()
        print("Folder status updated.")

    def get_file_info(self, filename):
        if filename in self.files:
            return self.files[filename].get_info()
        else:
            return f"File '{filename}' not found in monitored folder."

    def get_status(self):
        self.update_status()
        print("Current folder status:")
        for file in self.files.values():
            print(file.get_info())

def run_monitor():
    monitored_folder = MonitoredFolder("/path/to/monitored/folder")  
    while True:
        command = input("Enter command (commit / info <filename> / status): ").split()

        if command[0] == "commit":
            monitored_folder.commit()
        elif command[0] == "info":
            if len(command) == 2:
                print(monitored_folder.get_file_info(command[1]))
            else:
                print("Invalid command. Usage: info <filename>")
        elif command[0] == "status":
            monitored_folder.get_status()
        else:
            print("Invalid command.")

run_monitor()
