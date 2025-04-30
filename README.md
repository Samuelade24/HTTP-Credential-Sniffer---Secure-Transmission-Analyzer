# JSniff-Creds-Cleartext-Credential-Detector-
Task: Monitor HTTP traffic for plaintext creds (Wireshark alternative).
from scapy.all import sniff, TCP

def packet_callback(packet):
    if packet.haslayer(TCP) and packet[TCP].dport == 80:
        payload = str(packet[TCP].payload)
        if "uname=" in payload and "pass=" in payload:
            print(f"[!] Credentials Found: {payload}")

sniff(filter="tcp port 80", prn=packet_callback, store=0)
