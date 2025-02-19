# Windows using psutil:

    import psutil
    import time
    
    TARGET_PROCESS = "target.exe"  # Replace with your executable's name
    MEMORY_LIMIT = 1 * 1024 * 1024 * 1024  # 1GB in bytes
    
    def get_process_memory_usage(process_name):
        for proc in psutil.process_iter(['pid', 'name', 'memory_info']):
            if proc.info['name'] == process_name:
                return proc.info['memory_info'].rss  # Resident Set Size (actual memory used)
        return None
    
    while True:
        memory_usage = get_process_memory_usage(TARGET_PROCESS)
        if memory_usage is not None:
            print(f"{TARGET_PROCESS} is using {memory_usage / (1024 * 1024)} MB")
            if memory_usage > MEMORY_LIMIT:
                print(f"ALERT: {TARGET_PROCESS} exceeded 1GB memory usage!")
        else:
            print(f"{TARGET_PROCESS} not found.")
    
        time.sleep(5)  # Check every 5 seconds


# Linux using ps and grep:


    #!/bin/bash
    
    TARGET_PROCESS="target_executable"
    MEMORY_LIMIT=$((1024 * 1024))  # 1GB in KB
    
    while true; do
        MEM_USAGE=$(ps -eo cmd,rss | grep "$TARGET_PROCESS" | awk '{sum+=$NF} END {print sum}')
        
        if [[ -n "$MEM_USAGE" && "$MEM_USAGE" -gt "$MEMORY_LIMIT" ]]; then
            echo "ALERT: $TARGET_PROCESS exceeded 1GB memory usage! ($MEM_USAGE KB)"
        fi
    
        sleep 5
    done
