# ItsyBitsy

In this simple CTF which is part of SoC lvl.1 roadmap we will put our ELK skills to the test.

### Scenario:

During normal SOC monitoring, Analyst **John** observed an alert on an IDS solution indicating a potential C2 communication from a user **Browne** from the HR department. A suspicious file was accessed containing a malicious pattern **THM:{ ________ }**. A week-long HTTP connection logs have been pulled to investigate. Due to limited resources, only the connection logs could be pulled out and are ingested into the **connection_logs** index in **Kibana**. Our task in this room will be to examine the network connection logs of this user, find the link and the content of the file, and answer the questions.

### **How many events were returned for the month of March 2022?**

Upon login, we will go to burger menu on the left and click **Discover**

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled.png)

Now narrow the search for only March 2022 and type **Refresh** and check the number of hits.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%201.png)

**Answer:** 1482

### What is the IP associated with the suspected user in the logs?

Since we are talking about the user IP we are referring to the **source IP address**. Lets observe top source IP addresses from **source_ip** filter and pick the correct one.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%202.png)

**Answer:** 192.166.65.54

### The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?

Lets use source_ip filter and use our IP address as filter as well -  question is related to **legit windows binary** means code/software or part of it. Since downloading activity is associated with **http**, binary has to present itself via **user-agent**. User-agent servers as identifier of the software or OS which is communicating via http. Lets narrow the filter for the user-agent as well. Our binary is legit Windows command-line utility (per Microsoft documentation).

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%203.png)

**Answer:** bitsadmin

### The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?

Using our source IP filter combined with host filter as well will reveal our culprit.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%204.png)

**Answer:** pastebin.com

### What is the full URL of the C2 to which the infected host is connected?

By using filter from previous question we will also **uri** filter - or Uniform Resource Identifier - this is string of characters used to identify resource on the Internet - since question points us to full URL.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%205.png)

**Answer:** pastebin.com/yTg0Ah6a

### A file was accessed on the filesharing site. What is the name of the file accessed?

Lets firstly circle back to our Scenario on very beginning - our report says:

**A suspicious file was accessed containing a malicious pattern THM:{ ________ }**

First, we will identify the file type by using **resp_mime_type** filter. MIME types are a standardized way of indicating the nature and format of a document, file, or assortment of bytes. They are used by web servers to tell browsers how to handle different types of files and by email clients to understand how to display or deal with attachments, among many other uses. Lets add it to our existing scenario.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%206.png)

We can see that our file is plain text document - most common extension for these files are **.txt**

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%207.png)

Lets use curl to retrieve the context of the site and write simple grep to filter for .txt extension.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%208.png)

**Answer:** secret.txt

### The file contains a secret code with the format THM{_____}.

Since we already have all we need to get the flag we will tweak our previous grep and reveal the flag.

![Untitled](ItsyBitsy%20d42626deb7f8415199e9284824707ddd/Untitled%209.png)

**Answer:** THM{SECRET__CODE}

And we are done, thanks for tuning in.