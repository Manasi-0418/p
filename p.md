# Python Programming for Security – All 35 Practical Answers
**Course:** 25PCY105P | **8 Marks: 20 Questions** | **12 Marks: 15 Questions**

> Run each program separately. Save it as a `.py` file. Install required libraries where mentioned. Network scanning and web requests should be limited to localhost or systems for which you have permission.

# PART A – 8 MARKS

## Q1. Check whether an IP address is public or private
```python
import ipaddress

ip = input("Enter IP address: ")
try:
    address = ipaddress.ip_address(ip)
    if address.is_private:
        print("Private IP Address")
    else:
        print("Public IP Address")
except ValueError:
    print("Invalid IP address")
```

## Q2. Check filename extension and display warning
```python
filename = input("Enter filename: ")

if filename.endswith(".txt"):
    print("Text file")
elif filename.endswith(".py"):
    print("Python file")
elif filename.endswith(".exe"):
    print("Warning: Executable file")
else:
    print("Unknown file type")
```

## Q3. Grant, deny or restrict access based on role and authentication
```python
role = input("Enter role (admin/user/guest): ").lower()
authenticated = input("Authenticated? (yes/no): ").lower()

if authenticated == "yes":
    if role == "admin":
        print("Access Granted")
    elif role == "user":
        print("Access Restricted")
    else:
        print("Access Denied")
else:
    print("Access Denied")
```

## Q4. Constant-time password/API-key verification using HMAC
```python
import hmac

stored_key = "MySecret123"
user_key = input("Enter API key: ")

if hmac.compare_digest(user_key, stored_key):
    print("Authentication successful")
else:
    print("Authentication failed")
```

## Q5. Accept n values in a list and search for a number
```python
n = int(input("Enter number of elements: "))
mylist = []

for i in range(n):
    value = int(input("Enter element: "))
    mylist.append(value)

num = int(input("Enter number to search: "))
if num in mylist:
    print("Element is present in list")
else:
    print("Element is not present in list")
```

## Q6. Accept n strings, reject duplicates and display even-length strings
```python
n = int(input("Enter number of strings: "))
myset = set()

for i in range(n):
    value = input("Enter string: ")
    myset.add(value)

print("Even length strings:")
for value in myset:
    if len(value) % 2 == 0:
        print(value)
```

## Q7. Demonstrate set operations: create, add, remove and pop
```python
myset = {10, 20, 30, 40}
print("Original set:", myset)

value = int(input("Enter value to add: "))
myset.add(value)
print("After adding:", myset)

value = int(input("Enter value to remove: "))
if value in myset:
    myset.remove(value)
else:
    print("Value not found")
print("After removing:", myset)

if myset:
    removed = myset.pop()
    print("Popped value:", removed)
print("Final set:", myset)
```

## Q8. Union, intersection and difference of open-port sets
```python
server1 = {21, 22, 80, 443}
server2 = {22, 25, 80, 8080}

print("Server 1 ports:", server1)
print("Server 2 ports:", server2)
print("Union:", server1.union(server2))
print("Intersection:", server1.intersection(server2))
print("Difference (Server1 - Server2):", server1.difference(server2))
```

## Q9. Read 5 integers and handle IndexError and ValueError
```python
mylist = []
for i in range(5):
    try:
        num = int(input("Enter integer: "))
        mylist.append(num)
    except ValueError:
        print("Please enter an integer")

try:
    position = int(input("Enter index position: "))
    print("Element:", mylist[position])
except IndexError:
    print("Invalid index position")
except ValueError:
    print("Please enter a valid integer index")
```

## Q10. Find malicious words and line numbers in a file
```python
malicious_words = ["hacker", "hack", "ransom", "dos", "cyber"]
filename = input("Enter filename:")
found = {}

try:
    with open(filename, "r") as file:
        for line_no, line in enumerate(file, start=1):
            for word in malicious_words:
                if word in line.lower():
                    if word not in found:
                        found[word] = []
                    found[word].append(line_no)

    for word, lines in found.items():
        print(f"{word} : {lines}")

except FileNotFoundError:
    print("File not Found")
```

## Q11. Generate 50 fake login records using Faker and display first 10 rows
```python
from faker import Faker
import pandas as pd
import random

fake = Faker()
records = []
for i in range(50):
    records.append({
        "time": fake.date_time().strftime("%Y-%m-%d %H:%M:%S"),
        "username": fake.user_name(),
        "ip": fake.ipv4(),
        "status": random.choice(["LOGIN_SUCCESS", "LOGIN_FAILED"])
    })

df = pd.DataFrame(records)
df.to_csv("login_records.csv", index=False)
print(df.head(10))
```

## Q12. Top 5 IPs with failed logins and save bar chart
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("login_records.csv")
failed = df[df["status"] == "LOGIN_FAILED"]
top5 = failed["ip"].value_counts().head(5)
print(top5)

top5.plot(kind="bar")
plt.xlabel("IP Address")
plt.ylabel("Failed Login Count")
plt.title("Top 5 IPs with Failed Logins")
plt.tight_layout()
plt.savefig("top5_failed_ips.png")
plt.show()
```

## Q13. Find hour with maximum failed logins and plot hourly line chart
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("login_records.csv")
df["time"] = pd.to_datetime(df["time"])
failed = df[df["status"] == "LOGIN_FAILED"].copy()
failed["hour"] = failed["time"].dt.hour
hourly = failed.groupby("hour").size().reindex(range(24), fill_value=0)

print("Hour with maximum failed logins:", hourly.idxmax())
print(hourly)
hourly.plot(kind="line", marker="o")
plt.xlabel("Hour of Day")
plt.ylabel("Failed Logins")
plt.title("Failed Logins per Hour")
plt.xticks(range(24))
plt.tight_layout()
plt.savefig("failed_logins_per_hour.png")
plt.show()
```

## Q14. Fetch a local web page using requests
```python
import requests

url = input("Enter local test page URL: ")
try:
    response = requests.get(url, timeout=5)
    print("Status Code:", response.status_code)
    print("Response Headers:", response.headers)
    print("Page Size:", len(response.content), "bytes")
except requests.RequestException as e:
    print("Request failed:", e)
```

## Q15. Extract title and hyperlinks using BeautifulSoup
```python
import requests
from bs4 import BeautifulSoup

url = input("Enter URL: ")
response = requests.get(url, timeout=5)
soup = BeautifulSoup(response.text, "html.parser")

print("Title:", soup.title.get_text(strip=True) if soup.title else "No title")
print("Hyperlinks:")
for link in soup.find_all("a"):
    print(link.get("href"))
```

## Q16. Create and send ICMP echo request using Scapy
```python
from scapy.all import IP, ICMP, sr1

packet = IP(dst="127.0.0.1") / ICMP()
packet.show()
reply = sr1(packet, timeout=2, verbose=0)

if reply:
    print("Reply received")
else:
    print("No reply received")
```

## Q17. Display MD5, SHA-1 and SHA-256 hashes
```python
import hashlib

text = input("Enter a string: ").encode()
print("MD5:", hashlib.md5(text).hexdigest())
print("SHA-1:", hashlib.sha1(text).hexdigest())
print("SHA-256:", hashlib.sha256(text).hexdigest())
```

## Q18. Encrypt and decrypt a message using Fernet
```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
cipher = Fernet(key)
message = input("Enter message: ").encode()

encrypted = cipher.encrypt(message)
print("Encrypted:", encrypted)

decrypted = cipher.decrypt(encrypted)
print("Decrypted:", decrypted.decode())
```

## Q19. Generate salt and store password as PBKDF2 hash
```python
import hashlib
import os

password = input("Set password: ")
salt = os.urandom(16)
stored_hash = hashlib.pbkdf2_hmac("sha256", password.encode(), salt, 100000)

with open("password_store.txt", "w") as f:
    f.write(salt.hex() + "\n")
    f.write(stored_hash.hex() + "\n")

print("Password stored successfully")
```

## Q20. Verify password against stored salted hash
```python
import hashlib
import hmac

with open("password_store.txt", "r") as f:
    salt = bytes.fromhex(f.readline().strip())
    stored_hash = bytes.fromhex(f.readline().strip())

password = input("Enter your password: ")
new_hash = hashlib.pbkdf2_hmac("sha256", password.encode(), salt, 100000)

if hmac.compare_digest(new_hash, stored_hash):
    print("Password is correct")
else:
    print("Wrong password")
```

# PART B – 12 MARKS

## Q1. Matrix addition, multiplication, upper/lower triangular and symmetric checks
```python
import numpy as np

d = int(input("Enter dimension of matrix: "))
print("Enter first matrix values separated by spaces:")
arr = np.array(list(map(float, input().split())))
print("Enter second matrix values separated by spaces:")
arr1 = np.array(list(map(float, input().split())))

if len(arr) != d*d or len(arr1) != d*d:
    print("Wrong number of values")
else:
    mat1 = arr.reshape((d, d))
    mat2 = arr1.reshape((d, d))
    print("First Matrix:")
    print(mat1)
    print("Second Matrix:")
    print(mat2)
    print("Addition:")
    print(mat1 + mat2)
    print("Multiplication:")
    print(mat1 @ mat2)

    upper = True
    for i in range(d):
        for j in range(d):
            if i > j and mat1[i][j] != 0:
                upper = False
    if upper:
        print("First matrix is Upper Triangular")
    else:
        print("First matrix is not Upper Triangular")

    lower = True
    for i in range(d):
        for j in range(d):
            if i < j and mat2[i][j] != 0:
                lower = False
    if lower:
        print("Second matrix is Lower Triangular")
    else:
        print("Second matrix is not Lower Triangular")

    if np.allclose(mat1, mat1.T):
        print("First matrix is Symmetric")
    else:
        print("First matrix is not Symmetric")
```

## Q2. Check whether second matrix is inverse of first
```python
import numpy as np

d = int(input("Enter the dimension of the matrix: "))
print("Enter first matrix", d*d, "values separated by one space:")
input_str = input()
arr = np.array(list(map(float, input_str.split())))

if len(arr) != d*d:
    print("Wrong number of values for first matrix")
else:
    print("Enter second matrix", d*d, "values separated by one space:")
    input_str1 = input()
    arr1 = np.array(list(map(float, input_str1.split())))
    if len(arr1) != d*d:
        print("Wrong number of values in second matrix")
    else:
        mat1 = arr.reshape((d, d))
        mat2 = arr1.reshape((d, d))
        print(mat1)
        print(mat2)
        ident = np.identity(d)
        if np.allclose(mat1 @ mat2, ident):
            print("Second matrix is inverse of first")
        else:
            print("Second matrix is not inverse of first")
```

## Q3. Password security score using functions
```python
def calculate_score(password):
    score = 0
    if len(password) >= 8:
        score += 2
    elif len(password) >= 6:
        score += 1
    if any(char.isupper() for char in password):
        score += 2
    if any(char.islower() for char in password):
        score += 2
    if any(char.isdigit() for char in password):
        score += 2
    if any(not char.isalnum() for char in password):
        score += 2
    return score


def classify(score):
    if score <= 4:
        return "Weak"
    elif score <= 7:
        return "Medium"
    else:
        return "Strong"

password = input("Enter your password: ")
score = calculate_score(password)
strength = classify(score)
print("Password Score is:", score)
print("Password Strength is:", strength)
```

## Q4. Mixed-type list: insert, generate subset and display data types
```python
def convert_value(value):
    value = value.strip()
    if value.lower() == "true":
        return True
    elif value.lower() == "false":
        return False
    try:
        return int(value)
    except ValueError:
        pass
    try:
        return float(value)
    except ValueError:
        pass
    return value

n = int(input("Enter no of elements:"))
mylist = []
for i in range(n):
    value = input(f"Enter element {i+1}:")
    mylist.append(convert_value(value))

print("\nOriginal List:", mylist)
element = input("Enter element to be insert:")
element = convert_value(element)
position = int(input("Enter position to insert element:"))

if 0 <= position <= len(mylist):
    mylist.insert(position, element)
    print("\nGenerated list:", mylist)
else:
    print("Invalid Position")

start = int(input("Enter starting point:"))
end = int(input("Enter Ending point:"))
subset = mylist[start:end]
print("\nGenerating Substring:", subset)

print("\nElements with their datatype:")
for item in mylist:
    print(item, "--->", type(item).__name__)
```

## Q5. Tuple iteration, insert at index and sort
```python
mytuple = (40, 10, 30, 20)
print("Original tuple:", mytuple)

print("Tuple elements:")
for item in mytuple:
    print(item)

position = int(input("Enter position: "))
element = int(input("Enter new element: "))
mylist = list(mytuple)
mylist.insert(position, element)
mytuple = tuple(mylist)
print("Tuple after insertion:", mytuple)

sorted_tuple = tuple(sorted(mytuple))
print("Sorted tuple:", sorted_tuple)
```

## Q6. Word frequency using dictionary and most frequent word
```python
sentence = input("Enter a sentence: ")
words = sentence.lower().split()
frequency = {}

for word in words:
    frequency[word] = frequency.get(word, 0) + 1

print("Word frequency:")
for word, count in frequency.items():
    print(word, ":", count)

if frequency:
    most_frequent = max(frequency, key=frequency.get)
    print("Most frequent word:", most_frequent)
    print("Frequency:", frequency[most_frequent])
```

## Q7. Faker failed-login IP counts and suspicious IPs
```python
from faker import Faker
from collections import Counter
import random

fake = Faker()
ips = [fake.ipv4() for i in range(10)]
failed_ips = [random.choice(ips) for i in range(30)]

count = Counter(failed_ips)
print("Failed attempts per IP:")
for ip, attempts in count.items():
    print(ip, ":", attempts)

print("Suspicious IP addresses:")
for ip, attempts in count.items():
    if attempts > 3:
        print(ip, ":", attempts)
```

## Q8. parse_port with ValueError, range exception and finally
```python
def parse_port(value):
    try:
        port = int(value)
    except ValueError:
        raise ValueError("Invalid port: not a number")
    if port < 1 or port > 65535:
        raise Exception("Port must be between 1 and 65535")
    return port

try:
    value = input("Enter port number: ")
    port = parse_port(value)
    print("Valid port:", port)
except ValueError as e:
    print(e)
except Exception as e:
    print(e)
finally:
    print("Validation complete")
```

## Q9. Authentication log analysis with malformed-line handling
```python
import ipaddress
from datetime import datetime

filename = input("Enter log filename: ")
failed_count = {}
invalid_lines = []

try:
    with open(filename, "r") as file:
        for line_no, line in enumerate(file, start=1):
            fields = line.strip().split(",")
            if len(fields) != 4:
                invalid_lines.append((line_no, "Wrong number of fields"))
                continue
            timestamp, ip, status, username = fields
            try:
                datetime.strptime(timestamp, "%Y-%m-%d %H:%M:%S")
            except ValueError:
                invalid_lines.append((line_no, "Invalid timestamp"))
                continue
            try:
                ipaddress.ip_address(ip)
            except ValueError:
                invalid_lines.append((line_no, "Invalid IP address"))
                continue
            if status == "LOGIN_FAILED":
                failed_count[ip] = failed_count.get(ip, 0) + 1

    print("Failed login attempts per IP:")
    for ip, count in failed_count.items():
        print(ip, ":", count)
    print("Offending IPs:", [ip for ip, count in failed_count.items() if count > 3])
    print("Skipped lines:", invalid_lines)
    print("Total skipped lines:", len(invalid_lines))

except FileNotFoundError:
    print("Log file not found")
except PermissionError:
    print("Permission denied while opening log file")
```

## Q10. Compress repeated words using symbols and append dictionary
```python
import re

filename = input("Enter filename: ")
try:
    with open(filename, "r") as file:
        text = file.read()

    words = re.findall(r"\b\w+\b", text)
    counts = {}
    for word in words:
        key = word.lower()
        counts[key] = counts.get(key, 0) + 1

    symbols = ["@", "#", "$", "%", "&", "*"]
    dictionary = {}
    compressed = text
    index = 0
    for word, count in counts.items():
        if count > 1 and index < len(symbols):
            symbol = symbols[index]
            index += 1
            dictionary[word] = symbol
            compressed = re.sub(r"\b" + re.escape(word) + r"\b", symbol,
                                compressed, flags=re.IGNORECASE)

    with open("compressed.txt", "w") as file:
        file.write(compressed)
        file.write("\n\nReplacement Dictionary:\n")
        for word, symbol in dictionary.items():
            file.write(f"{word} : {symbol}\n")
    print("Compressed file created: compressed.txt")
except FileNotFoundError:
    print("File not found")
```

## Q11. Store student records in binary file and search by roll number
```python
import pickle

records = []
n = int(input("Enter number of students: "))
for i in range(n):
    rollno = int(input("Enter roll number: "))
    name = input("Enter name: ")
    marks = float(input("Enter marks: "))
    records.append({"rollno": rollno, "name": name, "marks": marks})

with open("students.dat", "wb") as file:
    pickle.dump(records, file)

search_roll = int(input("Enter roll number to search: "))
with open("students.dat", "rb") as file:
    data = pickle.load(file)

found = False
for record in data:
    if record["rollno"] == search_roll:
        print(record)
        found = True
if not found:
    print("Record not found")
```

## Q12. Extract email addresses and HTML form field names; save CSV
```python
import re
import csv
from bs4 import BeautifulSoup

filename = input("Enter HTML filename: ")
with open(filename, "r") as file:
    html = file.read()

emails = re.findall(r"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}", html)
soup = BeautifulSoup(html, "html.parser")
fields = [tag.get("name") for tag in soup.find_all(["input", "select", "textarea"])
          if tag.get("name")]

with open("extracted_data.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["Type", "Value"])
    for email in emails:
        writer.writerow(["Email", email])
    for field in fields:
        writer.writerow(["Form Field", field])

print("Emails:", emails)
print("Form field names:", fields)
print("Saved to extracted_data.csv")
```

## Q13. Validate host and port range, scan using python-nmap and save report
```python
import nmap
import ipaddress

host = input("Enter target host/IP: ")
try:
    ipaddress.ip_address(host)
    start_port = int(input("Enter starting port: "))
    end_port = int(input("Enter ending port: "))
    if not (1 <= start_port <= end_port <= 65535):
        raise ValueError("Invalid port range")

    scanner = nmap.PortScanner()
    scanner.scan(host, f"{start_port}-{end_port}", arguments="-sV")

    with open("nmap_report.txt", "w") as file:
        for scanned_host in scanner.all_hosts():
            for protocol in scanner[scanned_host].all_protocols():
                for port in scanner[scanned_host][protocol].keys():
                    state = scanner[scanned_host][protocol][port]["state"]
                    service = scanner[scanned_host][protocol][port]["name"]
                    line = f"{scanned_host}, {port}, {state}, {service}"
                    print(line)
                    file.write(line + "\n")
    print("Report saved to nmap_report.txt")
except ValueError as e:
    print("Invalid input:", e)
except Exception as e:
    print("Scan error:", e)
```

## Q14. Scapy TCP SYN scan of localhost ports 20–30
```python
from scapy.all import IP, TCP, sr1, send

target = "127.0.0.1"
for port in range(20, 31):
    packet = IP(dst=target) / TCP(dport=port, flags="S")
    reply = sr1(packet, timeout=1, verbose=0)

    if reply is None:
        print(f"Port {port}: No response (filtered)")
    elif reply.haslayer(TCP) and reply[TCP].flags == 0x12:
        print(f"Port {port}: OPEN")
        send(IP(dst=target) / TCP(dport=port, flags="R"), verbose=0)
    else:
        print(f"Port {port}: Closed")
```

## Q15. Encrypt a text file with Fernet; decrypt and test wrong key
**Encryption – save as `encrypt.py`:**
```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
with open("secret.key", "wb") as key_file:
    key_file.write(key)

with open("plain.txt", "rb") as file:
    data = file.read()

encrypted = Fernet(key).encrypt(data)
with open("encrypted.txt", "wb") as file:
    file.write(encrypted)
print("File encrypted successfully")
```

**Decryption – save as `decrypt.py`:**
```python
from cryptography.fernet import Fernet, InvalidToken

with open("secret.key", "rb") as key_file:
    key = key_file.read()
with open("encrypted.txt", "rb") as file:
    encrypted = file.read()

try:
    decrypted = Fernet(key).decrypt(encrypted)
    with open("decrypted.txt", "wb") as file:
        file.write(decrypted)
    print("File decrypted successfully")
except InvalidToken:
    print("Decryption failed: wrong key or modified file")
```

**Wrong-key test:** Replace `key` in the decryption program with another valid Fernet key; `InvalidToken` will be handled and the failure message displayed.

# Required packages and Kali commands

Install packages when internet is available:
```bash
sudo apt update
sudo apt install python3-numpy python3-scapy python3-requests python3-bs4 python3-nmap nmap
python3 -m pip install faker pandas matplotlib cryptography
```

Run a program:
```bash
python3 filename.py
```

Run Scapy/Nmap scans with required privileges where necessary:
```bash
sudo python3 filename.py
```

**Important:** For Q10 (8 marks), create the text file in the same directory as the Python program. For Q19–Q20 (8 marks), run the password-storage program before the verification program. For Pandas questions, run Q11 first to create `login_records.csv`, then Q12/Q13. For Q15 (12 marks), create `plain.txt` before running `encrypt.py`.
