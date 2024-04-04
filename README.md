import json
import threading
from pathlib import Path

class DatabaseConnection:
    _instance = None
    _lock = threading.Lock()
    _config = None

    def __init__(self):
        raise RuntimeError('Call instance() instead')

    @classmethod
    def instance(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = cls.__new__(cls)
                    cls._instance._initialize()
        return cls._instance

    def _initialize(self):
        config_path = Path.home() / "db_config.json"
        with open(config_path, 'r') as file:
            self._config = json.load(file)

    def execute_query(self, query):
       
        return f"Executing: {query}"

    def get_connection_info(self):
        
        return self._config.copy()


if name == "__main__":
    db_connection = DatabaseConnection.instance()
    print(db_connection.get_connection_info())
    print(db_connection.execute_query("SELECT * FROM users"))

    another_connection = DatabaseConnection.instance()
    print(id(db_connection) == id(another_connection))
