# Protocols and Servers

## Telnet

### What is Telnet?

**Telnet** is a network **protocol** that allows you to connect to another computer remotely and access its **terminal** (command line). Think of it as controlling another computer from your own computer through a text-based interface.

Using **Telnet**, you can:
- Log into remote computers
- Run programs on remote systems
- Execute **system administration** tasks from anywhere
- Start processes and manage files remotely

### How Telnet Works

When you connect using **Telnet**, the process is simple:
1. You connect to the remote computer
2. You're asked for a **username**
3. You're asked for a **password**
4. Once authenticated, you get access to the remote computer's **terminal**

### The Security Problem

The biggest problem with **Telnet** is that it sends everything in **cleartext** (unencrypted). This means:
- Your **username** is sent without encryption
- Your **password** is sent without encryption
- All your commands are sent without encryption
- Anyone monitoring the network can see everything you type

This makes **Telnet** extremely vulnerable to attacks. Hackers can easily intercept your login credentials and gain unauthorized access to systems.

### The Secure Alternative

**Telnet** is no longer considered safe for remote administration. The secure alternative is **SSH (Secure Shell)**, which encrypts all communication between computers. **SSH** provides the same functionality as **Telnet** but with strong encryption to protect your data.

**Key takeaway:** Never use **Telnet** for sensitive work - always use **SSH** instead!

---

## Hypertext Transfer Protocol (HTTP)

### What is HTTP?

**HTTP (Hypertext Transfer Protocol)** is the **protocol** used to transfer **web pages** over the internet. Every time you visit a website, your **web browser** uses **HTTP** to communicate with the **web server**.

### How HTTP Works

When you browse the internet:
1. Your **web browser** (the **client**) sends a **request** to a **web server**
2. The **web server** responds by sending back the requested files
3. Your **browser** displays the **web page** to you

**HTTP** can transfer various types of files:
- **HTML pages** (the structure of websites)
- **Images** (like JPG, PNG files)
- **CSS files** (styling for websites)
- **JavaScript files** (interactive features)
- **Videos** and other media files

### Example: Loading a Web Page

Here's what happens when you visit a website:

1. **Browser requests HTML file** - Your browser asks for `index.html`
2. **Server sends HTML** - The **web server** sends the **HTML page**
3. **Browser requests images** - Your browser sees an image reference and requests `logo.jpg`
4. **Server sends image** - The **web server** sends the image file
5. **Browser displays page** - Your browser combines everything and displays the complete webpage

### The Security Problem

Just like **Telnet**, **HTTP** sends data in **cleartext** (unencrypted). This means:
- Anyone monitoring network traffic can see what websites you visit
- Attackers can intercept sensitive information
- **Passwords** and personal data sent over **HTTP** are not protected

### Using Telnet to Act as a Web Browser

Because **HTTP** uses **cleartext**, we can manually communicate with a **web server** using **Telnet**. This helps us understand how **HTTP** works behind the scenes.

**Steps to request a web page manually:**

1. Connect to the **web server** on **port 80** (default **HTTP** port):
```bash
   telnet MACHINE_IP 80
```

2. Request a specific page using the **GET** command:
```
   GET /index.html HTTP/1.1
```

3. Specify the **host**:
```
   host: telnet
```

4. Press **Enter** twice to send the **request**

### Popular Web Servers

**Web servers** are software that serve **web pages** to **browsers**. Popular choices include:

| Web Server | Type | Cost |
|------------|------|------|
| **Apache** | Open source | Free |
| **Nginx** | Open source | Free |
| **IIS (Internet Information Services)** | Closed source (Microsoft) | Requires license |

### Popular Web Browsers

**Web browsers** are applications that display **web pages**. Popular browsers include:

- **Chrome** (by Google)
- **Edge** (by Microsoft)
- **Firefox** (by Mozilla)
- **Safari** (by Apple)

All these **browsers** are free to download and use. Companies compete to make the best **browser** to gain more users.

---

## File Transfer Protocol (FTP)

### What is FTP?

**FTP (File Transfer Protocol)** is a **protocol** designed to transfer files between computers efficiently. It was created to make file sharing between different computer systems easy and reliable.

### The Security Problem

Like **Telnet** and **HTTP**, **FTP** also sends data in **cleartext**. This means:
- **Usernames** and **passwords** are sent without encryption
- Anyone monitoring the network can steal your credentials
- File contents can be intercepted during transfer

### How FTP Works

**FTP** uses **port 21** by default for sending commands. However, **FTP** is unique because it uses **two separate connections**:

1. **Control connection (port 21)** - For sending commands and receiving responses
2. **Data connection** - For transferring actual files

### FTP Modes

**FTP** operates in two different modes:

| Mode | Description |
|------|-------------|
| **Active Mode** | Data is sent from the **FTP server's port 20** to the **client** |
| **Passive Mode** | Data is sent from the **client's port** (above 1023) to the **server** |

### Using Telnet to Connect to FTP

We can use **Telnet** to manually interact with an **FTP server** and understand how it works.

**Steps to connect:**

1. Connect to **FTP server** on **port 21**:
```bash
   telnet MACHINE_IP 21
```

2. Provide **username**:
```
   USER frank
```

3. Provide **password**:
```
   PASS D2xc9CgD
```

4. Use **FTP commands** to interact with the server

### Common FTP Commands

| Command | What It Does |
|---------|--------------|
| `USER` | Specify the **username** |
| `PASS` | Specify the **password** |
| `STAT` | Get server status information |
| `SYST` | Show the system type (e.g., UNIX, Windows) |
| `PASV` | Switch to **passive mode** |
| `TYPE A` | Switch to **ASCII mode** (for text files) |
| `TYPE I` | Switch to **binary mode** (for images, programs, etc.) |
| `LIST` | List files in current directory |
| `QUIT` | Disconnect from server |


### Using a Real FTP Client

While **Telnet** helps us understand **FTP**, using an actual **FTP client** is much easier for downloading files.

**Steps to download a file:**

1. Connect to **FTP server**:
```bash
   ftp MACHINE_IP
```

2. Enter **username** and **password** when prompted

3. List files:
```
   ls
```

4. Switch to **ASCII mode** for text files:
```
   ascii
```

5. Download a file:
```
   get README.txt
```

### Popular FTP Server Software

**FTP servers** are programs that allow file sharing. Popular options include:

- **vsftpd** (Very Secure FTP Daemon)
- **ProFTPD**
- **uFTP**

### Security Warning

Because **FTP** sends **login credentials**, **commands**, and **files** in **cleartext**, it's easy for attackers to:
- Steal your **username** and **password**
- Intercept files being transferred
- Monitor all your **FTP** activity

**Always consider using secure alternatives like SFTP (SSH File Transfer Protocol) or FTPS (FTP Secure) instead!**

---

## Simple Mail Transfer Protocol (SMTP)

### What is SMTP?

**SMTP (Simple Mail Transfer Protocol)** is the **protocol** used for sending **email messages** between **email servers**. Think of it as the postal service for the internet - it's responsible for delivering your **emails** from your computer to the recipient's **email server**.

### How Email Delivery Works

Sending an **email** over the internet involves several components working together. Let's understand each one:

| Component | Full Name | What It Does |
|-----------|-----------|--------------|
| **MUA** | Mail User Agent | Your **email client** (like Gmail, Outlook) |
| **MSA** | Mail Submission Agent | Checks your **email** for errors before sending |
| **MTA** | Mail Transfer Agent | **Email server** that routes and delivers messages |
| **MDA** | Mail Delivery Agent | Stores **emails** until recipient collects them |

### The Email Journey (5 Steps)

Here's how an **email** travels from sender to recipient:

**Step 1: You compose email**
- You write an **email** in your **email client (MUA)**
- Your **MUA** connects to the **Mail Submission Agent (MSA)**

**Step 2: Email checked and submitted**
- The **MSA** checks your **email** for errors
- If everything is correct, it's transferred to the **Mail Transfer Agent (MTA)**
- The **MSA** and **MTA** are usually on the same **server**

**Step 3: Email routed to recipient**
- Your **MTA** finds the recipient's **MTA server**
- Your **MTA** sends the **email** to the recipient's **MTA**

**Step 4: Email stored for recipient**
- The recipient's **MTA** also functions as a **Mail Delivery Agent (MDA)**
- The **email** is stored in the recipient's mailbox

**Step 5: Recipient retrieves email**
- The recipient's **email client (MUA)** checks the **MDA**
- They download and read your **email**

### Email Protocols

Different **protocols** are used at different stages:

| Protocol | Purpose |
|----------|---------|
| **SMTP** | Sending **email** from **client** to **server** and between **servers** |
| **POP3** | Downloading **email** from **server** to **client** |
| **IMAP** | Accessing and managing **email** on the **server** |

We'll cover **POP3** and **IMAP** in the next tasks.

### How SMTP Works

**SMTP servers** listen on **port 25** by default. Because **SMTP** uses **cleartext**, we can use **Telnet** to manually send an **email** and see how it works.

### Sending Email with Telnet

Here's how to manually send an **email** using **SMTP**:

**Steps:**

1. Connect to **SMTP server** on **port 25**:
```bash
   telnet MACHINE_IP 25
```

2. Say hello to the server:
```
   helo telnet
```

3. Specify sender:
```
   mail from: <sender@email.com>
```

4. Specify recipient:
```
   rcpt to: <recipient@email.com>
```

5. Begin email content:
```
   data
```

6. Type your message:
```
   subject: Sending email with Telnet
   Hello Frank,
   I am just writing to say hi!
```

7. End message with a period on its own line:
```
   .
```

8. Quit:
```
   quit
```


### Understanding SMTP Commands

| Command | What It Does |
|---------|--------------|
| `helo` | Introduce yourself to the **SMTP server** |
| `mail from:` | Specify who is sending the **email** |
| `rcpt to:` | Specify who will receive the **email** |
| `data` | Begin typing the **email** message |
| `.` | End the **email** (period on its own line) |
| `quit` | Close the connection |

### The Security Problem

**SMTP** sends everything in **cleartext**, including:
- Sender and recipient **email addresses**
- The entire **email** message
- Any attached information

Your **email client** normally handles all these **SMTP** commands automatically behind a user-friendly interface. You don't need to memorize these commands - understanding how they work helps you better understand **email** security and potential vulnerabilities.

---

## Post Office Protocol 3 (POP3)

### What is POP3?

**POP3 (Post Office Protocol version 3)** is a **protocol** used to download **email messages** from an **email server** to your **email client**. Think of it as collecting mail from a post office box.

### How POP3 Works

When you use **POP3**:

1. Your **mail client** connects to the **POP3 server**
2. You **authenticate** with **username** and **password**
3. The **client** downloads new **email messages**
4. Optionally, the **emails** are deleted from the **server** (default behavior)

**POP3 servers** listen on **port 110** by default.

### Using Telnet to Connect to POP3

We can use **Telnet** to manually retrieve **emails** and understand how **POP3** works.

**Steps:**

1. Connect to **POP3 server** on **port 110**:
```bash
   telnet MACHINE_IP 110
```

2. Provide **username**:
```
   USER frank
```

3. Provide **password**:
```
   PASS D2xc9CgD
```

4. Use **POP3 commands** to manage **emails**

### Common POP3 Commands

| Command | What It Does |
|---------|--------------|
| `USER` | Specify your **username** |
| `PASS` | Specify your **password** |
| `STAT` | Get mailbox statistics (number of messages and total size) |
| `LIST` | List all **email messages** with their sizes |
| `RETR` | Retrieve a specific **email** message |
| `QUIT` | Close the connection and apply changes |


### The Security Problem

Just like other **protocols** we've seen, **POP3** sends everything in **cleartext**:
- Your **username** is visible
- Your **password** is visible
- **Email** contents are visible
- Anyone monitoring network traffic can steal your credentials

### How Mail Clients Use POP3

Your **email client (MUA)** like Outlook or Thunderbird automatically:
- Connects to the **POP3 server (MDA)**
- **Authenticates** with your credentials
- Downloads new **email messages**
- Displays them in a user-friendly interface

You don't see these commands, but they're happening behind the scenes.

### When to Use POP3

**POP3** is best when:
- You only check **email** from one device
- You want to save **emails** locally
- You have limited **server** storage space
- You don't need synchronization across devices

For synchronization across multiple devices, **IMAP** (covered in the next task) is a better choice.

---

## Internet Message Access Protocol (IMAP)

### What is IMAP?

**IMAP (Internet Message Access Protocol)** is a more advanced **email protocol** than **POP3**. It allows you to manage your **emails** directly on the **server** and keep everything synchronized across multiple devices.

### How IMAP Works

With **IMAP**:
- **Emails** stay on the **server**
- Your **email client** shows you what's on the **server**
- When you read, delete, or organize **emails**, changes are saved on the **server**
- All your devices see the same **emails** and folders

### IMAP Connection Details

**IMAP servers** listen on **port 143** by default.

### Using Telnet to Connect to IMAP

We can use **Telnet** to manually interact with an **IMAP server**:
```bash
telnet MACHINE_IP 143
```

### Important IMAP Feature

**IMAP** requires each **command** to be preceded by a **random string** (called a tag). This helps track responses. Common practice is to use:
- `c1` for first command
- `c2` for second command
- `c3` for third command, and so on

### Common IMAP Commands

| Command | What It Does |
|---------|--------------|
| `LOGIN` | **Authenticate** with **username** and **password** |
| `LIST` | List all mail folders |
| `EXAMINE` | Check a specific folder (like INBOX) |
| `SELECT` | Select a folder to work with |
| `LOGOUT` | Close the connection |

### The Security Problem

Just like **POP3**, **IMAP** sends credentials in **cleartext**:
- **Username** is visible: `frank`
- **Password** is visible: `D2xc9CgD`
- Anyone monitoring network traffic can steal login credentials

### IMAP in Real Email Clients

Modern **email clients** like Gmail, Outlook, and Thunderbird use **IMAP** automatically. When you:
- Mark an **email** as read
- Delete an **email**
- Move **emails** to folders
- Star/flag important **emails**

All these changes are sent to the **IMAP server** and synchronized across all your devices.

### Advantages of IMAP

1. **Multiple device support** - Access **emails** from phone, tablet, laptop
2. **Synchronized folders** - All devices see same folder structure
3. **Read/unread tracking** - Status synced across devices
4. **Server-side storage** - **Emails** backed up on **server**
5. **Search on server** - Can search all **emails** without downloading
6. **Selective download** - Only download **emails** you want to read

### Security Recommendation

Both **POP3** and **IMAP** send data in **cleartext**. Always use secure versions:
- **POP3S** (POP3 over SSL/TLS) - **Port 995**
- **IMAPS** (IMAP over SSL/TLS) - **Port 993**

These encrypt your connection and protect your **passwords** and **email** content from attackers!

---
