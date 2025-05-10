# ProvCon Dataset
This repository contains the provenance graph constructed using ProvCon presented at [WOSOC'25](https://www.ndss-symposium.org/ndss-paper/auto-draft-549/).

If you use this dataset, please cite:

```
@inproceedings{provcon25,
	title = {From {Observations} to {Insights}: {Constructing} {Effective} {Cyberattack} {Provenance} {With} {PROVCON}},
	language = {en},
	booktitle = {Workshop on {SOC} {Operations} and {Construction} ({WOSOC}) 2025},
	author = {Yusof, Anis and Li, Shaofei and Kawatra, Arshdeep Singh and Li, Ding and Chang, Ee-Chien and Liang, Zhenkai},
	year = {2025},
	isbn = {9798991927604},
	doi = {https://dx.doi.org/10.14722/wosoc.2025.23008},
}
```

## How Provenance Graph is Constructed
Using cyberattack information from CTI reports, ProvCon reproduce the cyberattack on a cyber range. After executing the attack activities throughout the environment, data are exracted and transformed into provenance graph. For Linux-based systems, the provenance graph is constructed using the `sysdig` logs. For Windows-based systems, the provenance graph is constructed using the `Sysmon` logs.

## Data Summary

Below describe the file structure of this repository. In addition to default system logging, we utilize [Linux Audit Daemon](https://linux.die.net/man/8/auditd) (`auditd`) and [Sysdig](https://github.com/draios/sysdig) for Linux-based systems and [System Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) (Sysmon) for Windows-based systems.

### Provenance Graph
The provenace graph is represented as a `*.dot` file, and is located under `/<apt>/provenance-graph/*.dot`. Every `*.dot` file correspond to one system that is involved in the reproduced cyberattack. The `*-provenance-graph.dot` correspond to a Linux-based system while `*-sysmon-provenance-graph.dot` correspond to a Windows-based system. The `*.dot` file can be visualized using [Graphviz](https://graphviz.org/) or transformed to other graph formats.

### System Logs
The zipped system logs are located in `/<apt>/logs/<instance>` for every system that is involved in the reproduced cyberattack. The `<instance>_system_logs` represent Linux-based systems and the logs can be viewed manually (e.g., text editor). The `<instance>_events_logs` represent Windows-based systems and the `*.evtx` logs can be viewed using `Event Viewer` or Python's `python-evtx` library.

#### Audit Logs
The audit logs for Linux-based systems are located under `/<apt>/logs/<instance>_system_logs/var/log/audit/*`. The audit logs (e.g., `audit.log`) is viewable using `ausearch`.

#### Sysdig Logs
The sysdig logs for Linux-based systems is at `/<apt>/logs/<instance>_system_logs/var/log/sysdig.zip`. Unzipping `sysdig.zip` gives a `sysdig.scap` capture file which can be opened or transformed to a `json` file using the `sysdig` program.

#### Sysmon Logs
The Sysmon logs for Windows-based systems is at `/<apt>/logs/<instance>_events_logs/Microsoft-Windows-Sysmon_Operational.evtx`. The Sysmon logs can be viewed using the same method as any other `*.evtx` logs (e.g., `Event Viewer`).

#### Annotation
The annotation is located at `/<apt>/annotation-attack.csv` as a semicolon `;` delimited `*.csv` file. The annotations provide labels for the corresponding attack activities recorded in the respective logs. Additionally, it contain attributed information (i.e., which system, what executable) as well as being tagged to the particular event across the attack sequence.

## Other Data
In additon to system logs that are used for constructing provenance graph, we also extract other types of data that are relevant for attack analysis.

### Network Logs
The network capture for any systems are located at `/<apt>/network/*.pcap`. Every `<instance>_combined.pcap` correspond to one system that is involved in the reproduced cyberattack. For network traffic monitoring, we utilize [tcpdump](https://www.tcpdump.org/) for Linux-based systems and [Packet Monitor](https://learn.microsoft.com/en-us/windows-server/networking/technologies/pktmon/pktmon) (Pktmon) for Windows-based systems.

### Memory Image
The memory dump are captured as an `*.elf` image for every system at the end of the reproduced cyberattack (i.e., after executing all activities in the cyber range). The image can be viewed and analyzed using memory forensic tools (e.g., Volatility with the appropriate profile). Due to the huge size of the memory dumps, we omit the image from this repository. We will provide the image when requested.
