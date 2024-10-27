### What is System Md?
**System md** is a **init Process** of the Linux Operating System. You may ask
**What is the Init Process**?
- Init Process is the first process that runs When the System boots Boots up:
- This is How The Series of Events Occur
- System Boots Up-->Kernel--->Init Process(System Md)
- Now after Initializing It is the Responsibility of the System md to start all other essential  services like networking,Memory,I/O


### What Advantage does System md has over normal Traditional Process Systems?
##### It is Observed that Traditional Init Process Start the Process in a Sequential manner while the  System Md Starts the Process in a Parallel Manner

#### It also provides a Snapshot Which Helps in System Recovery


#### Practical Syntax:
```bash
sudo systemctl start <service-name>
```

```bash
systemd-analyze
```
